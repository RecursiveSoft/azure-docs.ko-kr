---
title: "Apache Storm 및 HBase를 사용하여 센서 데이터 분석 | Microsoft Docs"
description: "가상 네트워크를 사용하여 Apache Storm에 연결하는 방법을 알아봅니다. HBase와 함께 Storm을 사용하여 이벤트 허브에서 센서 데이터를 처리하고 D3.js를 통해 이를 시각화합니다."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 138c90a1a9cbf3b85856f372beeb4472edb9e2e8
ms.lasthandoff: 03/25/2017


---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>HDInsight(Hadoop)에서 Apache Storm, 이벤트 허브 및 HBase를 사용하여 센서 데이터 분석

HDInsight의 Apache Storm을 사용하여 Azure 이벤트 허브에서 센서 데이터를 처리하는 방법에 대해 알아보니다. 데이터는 HDInsight의 Apache HBase에 저장되며 D3.js를 사용하여 시각화됩니다.

이 문서에 사용되는 Azure Resource Manager 템플릿은 리소스 그룹에 여러 Azure 리소스를 만드는 방법을 보여 줍니다. 템플릿은 Azure Virtual Network, 두 개의 HDInsight 클러스터(Storm, HBase) 및 Azure Web App을 만듭니다. 실시간 웹 대시보드의 node.js 구현은 웹앱에 자동으로 배포됩니다.

> [!NOTE]
> 이 문서의 정보와 이 문서의 예제에서는 HDInsight 버전 3.5가 필요합니다.
>
> Linux는 HDInsight 버전 3.4 이상에서 사용되는 유일한 운영 체제입니다. 자세한 내용은 [Windows에서 HDInsight 사용 중단](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

* Azure 구독. [Azure 무료 평가판](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)을 참조하세요.
  
  > [!IMPORTANT]
  > 기존 HDInsight 클러스터가 필요하지 않습니다. 이 문서의 단계는 다음 리소스를 만듭니다.
  > 
  > * Azure Virtual Network
  > * HDInsight 클러스터의 Storm(Linux 기반, 작업자 노드 2개)
  > * HDInsight 클러스터의 HBase(Linux 기반, 작업자 노드 2개)
  > * 웹 대시보드를 호스트하는 Azure 웹앱

* [Node.js](http://nodejs.org/): 개발 환경에서 로컬로 웹 대시보드를 미리 보는 데 사용합니다.
* [Java 및 JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Storm 토폴로지를 개발하는 데 사용합니다.
* [Maven](http://maven.apache.org/what-is-maven.html): 프로젝트를 빌드하고 컴파일하고 하는 데 사용합니다.
* [Git](http://git-scm.com/): GitHub에서 프로젝트를 다운로드하는 데 사용합니다.
* **SSH** 클라이언트: Linux 기반 HDInsight 클러스터에 연결하는 데 사용합니다. 자세한 내용은 [HDInsight와 함께 SSH 사용](hdinsight-hadoop-linux-use-ssh-unix.md)을 참조하세요.
    
    > [!NOTE]
    > SSH를 사용하여 로컬 개발 환경과 HDInsight 클러스터 간에 파일을 복사하는 데 사용되는 `scp` 명령에 대한 액세스 권한도 필요합니다.


## <a name="architecture"></a>아키텍처

![아키텍처 다이어그램](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

이 예제는 다음 구성 요소로 구성됩니다.

* **Azure 이벤트 허브**: 센서에서 수집된 데이터를 포함합니다.
* **HDInsight의 Storm**: 이벤트 허브의 데이터를 실시간으로 처리합니다.
* **HDInsight의 HBase**: Storm에서 처리된 후에 데이터에 대한 영구 NoSQL 데이터 저장소를 제공합니다.
* **Azure 가상 네트워크 서비스**: HDInsight의 Storm 클러스터와 HDInsight의 HBase 클러스터 간 보안 통신을 설정합니다.
  
  > [!NOTE]
  > Java HBase 클라이언트 API를 사용하는 경우에 가상 네트워크가 필요합니다. HBase 클러스터에 대한 공용 게이트웨이를 통해 노출되지 않습니다. HBase 및 Storm 클러스터를 동일한 가상 네트워크에 설치하면 Storm 클러스터(또는 가상 네트워크의 다른 시스템)에서 클라이언트 API를 사용하여 HBase에 직접 액세스할 수 있습니다.

* **대시보드 웹 사이트**: 실시간으로 데이터 차트를 작성하는 예제 대시보드입니다.
  
  * 웹 사이트는 Node.js로 구현되므로 모든 클라이언트 운영 체제에서 테스트용으로 실행하거나 Azure 웹 사이트에 배포할 수 있습니다.
  * [Socket.io](http://socket.io/) 는 Storm 토폴로지와 웹 사이트 간의 실시간 통신에 사용됩니다.
    
    > [!NOTE]
    > 통신에 Socket.io를 사용하는 것은 구현 세부 정보입니다. 원시 WebSocket 또는 SignalR과 같은 모든 통신 프레임워크를 사용할 수 있습니다.

  * [D3.js](http://d3js.org/) 는 웹 사이트로 전송되는 데이터의 그래프를 작성하는 데 사용됩니다.

> [!IMPORTANT]
> Storm 및 HBase 모두에 대해 하나의 HDInsight 클러스터를 만드는 지원 방법이 없으므로 두 클러스터가 필요합니다.

이 토폴로지는 [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) 클래스를 사용하여 이벤트 허브에서 데이터를 읽고 [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) 클래스를 사용하여 HBase에 데이터를 씁니다. 웹 사이트와의 통신은 [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java)를 사용하여 수행됩니다.

다음 다이어그램은 토폴로지 레이아웃을 설명합니다.

![토폴로지 다이어그램](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> 이는 토폴로지의 단순화된 보기입니다. 런타임에 읽고 있는 이벤트 허브의 각 파티션에 대해 각 구성 요소의 인스턴스가 만들어집니다. 이러한 인스턴스는 클러스터의 노드에 분산되며 노드 간에 다음과 같이 데이터가 라우팅됩니다.
> 
> * Spout에서 파서로 전송되는 데이터는 부하 분산됩니다.
> * 파서에서 대시보드 및 HBase로 전송되는 데이터는 장치 ID별로 그룹화되므로 같은 장치의 메시지는 항상 동일한 구성 요소로 흐릅니다.

### <a name="topology-components"></a>토폴로지 구성 요소

* **EventHub Spout**: spout는 Apache Storm 버전 0.10.0 이상의 일부로 제공됩니다.
  
  > [!NOTE]
  > 이 예제에서 사용되는 Event Hubs Spout에는 HDInsight 클러스터 버전 3.3 또는 3.4에 대한 Storm이 필요합니다. HDInsight의 Storm 이전 버전에서 이벤트 허브를 사용하는 방법에 대한 자세한 내용은 [HDInsight의 Storm으로 Azure 이벤트 허브에서 이벤트 처리](hdinsight-storm-develop-java-event-hub-topology.md)를 참조하세요.

* **ParserBolt.java**: Spout에서 내보내는 데이터는 원시 JSON이며, 한 번에 둘 이상의 이벤트를 내보낼 때도 있습니다. 이 Bolt는 Spout에서 내보낸 데이터를 읽어 여러 필드가 포함된 튜플로 새 스트림에 내보내는 방법을 보여 줍니다.
* **DashboardBolt.java**: 이 구성 요소는 Java용 Socket.io 클라이언트 라이브러리를 사용하여 웹 대시보드로 데이터를 실시간으로 보내는 방법을 보여 줍니다.
* **Temperature.java**: 토폴로지를 정의하고 **Config.properties** 파일에서 구성 데이터를 로드합니다.

## <a name="prepare-your-environment"></a>환경 준비

이 예제를 사용하려면 먼저 Storm 토폴로지에서 읽을 Azure 이벤트 허브를 만들어야 합니다.

### <a name="configure-event-hub"></a>이벤트 허브 구성

이벤트 허브는 이 예제의 데이터 원본입니다. 다음 단계에 따라 이벤트 허브를 만듭니다.

1. [Azure Portal](https://portal.azure.com)에서 **+ 새로 만들기** -> **사물 인터넷** -> **Event Hubs**를 선택합니다.
2. **네임스페이스 만들기** 블레이드에서 다음 태스크를 수행합니다.
   
   1. 네임스페이스의 **이름** 을 입력합니다.
   2. 가격 책정 계층을 선택합니다. **기본** 이면 충분합니다.
   3. 사용할 Azure **구독** 을 선택합니다.
   4. 기존 리소스 그룹을 선택하거나 새 리소스 그룹을 만듭니다.
   5. Event Hub의 **위치** 를 선택합니다.
   6. **대시보드에 고정**을 선택하고 **만들기**를 클릭합니다.

3. 만들기 프로세스가 완료되면 네임스페이스에 대한 Event Hubs 블레이드가 표시됩니다. 여기에서 **+ Event Hub 추가**를 선택합니다. **Event Hub 만들기** 블레이드에서 **sensordata**의 이름을 입력한 다음 **만들기**를 선택합니다. 다른 필드는 기본값으로 둡니다.
4. 네임스페이스에 대한 Event Hubs 블레이드에서 **Event Hubs**를 선택합니다. **sensordata** 항목을 선택합니다.
5. sensordata Event Hub에 대한 블레이드에서 **공유 액세스 정책**을 선택합니다. **+ 추가** 링크를 사용하여 다음 정책에 추가합니다.

    | 정책 이름 | 클레임 |
    | ----- | ----- |
    | devices | 보내기 |
    | storm | 수신 대기 |

1. 두 정책을 모두 선택하고 **기본 키** 값을 적어둡니다. 이후 단계에서 두 정책에 대한 값이 필요합니다.

## <a name="download-and-configure-the-project"></a>프로젝트 다운로드 및 구성

다음을 사용하여 GitHub에서 프로젝트를 다운로드합니다.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

명령이 완료되면 다음 디렉터리 구조가 생성됩니다.

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                hbase-site.xml - connection information for the HBase cluster.
                Config.properties - configuration information for the topology. How to read from Event Hub and the URI to the dashboard.
            src/ - the Java bolts and topology definition.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> 이 문서에서는 이 샘플에 포함된 코드에 대해 자세히 알아보지 않습니다. 그러나 코드는 완전히 주석 처리되어 있습니다.

**hdinsight-eventhub-example/TemperatureMonitor/resources/Config.properties** 파일을 열고 다음 줄에 Event Hub 정보를 추가합니다.

    eventhubspout.username = <shared access policy name that can read from Event Hub>
    eventhubspout.password = <shared access policy key>
    eventhubspout.namespace = <namespace of your Event Hub
    eventhubspout.entitypath = <name of your event hub>
    eventhubspout.partitions.count = 2

이 정보를 추가한 후 파일을 저장합니다.

## <a name="compile-and-test-locally"></a>로컬로 컴파일 및 테스트

테스트하기 전에 대시보드를 시작하여 토폴로지의 출력을 확인하고 이벤트 허브에 저장할 데이터를 생성해야 합니다.

> [!IMPORTANT]
> 이 토폴로지의 HBase 구성 요소는 로컬로 테스트하는 경우에 활성화되지 않습니다. 이는 HBase 클러스터의 Java API는 클러스터를 포함하는 Azure 가상 네트워크 외부에서 액세스할 수 없기 때문입니다.

### <a name="start-the-web-application"></a>웹 응용 프로그램 시작

1. 새 명령 또는 터미널을 열고 디렉터리를 **hdinsight-eventhub-example/dashboard**로 변경합니다. 다음 명령을 사용하여 웹 응용 프로그램에서 필요한 종속성을 설치합니다.
   
        npm install

2. 다음 명령을 사용하여 웹 응용 프로그램을 시작합니다.
   
        node server.js
   
    다음 텍스트와 유사한 메시지가 표시됩니다.
   
        Server listening at port 3000

3. 웹 브라우저를 열고 **http://localhost:3000/**을 주소로 입력합니다. 다음 이미지와 유사한 결과가 표시됩니다.
   
    ![웹 대시보드](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    이 명령 프롬프트 또는 터미널을 계속 열어 두세요. 테스트 후 Ctrl+C를 사용하여 웹 서버를 중지합니다.

### <a name="start-generating-data"></a>데이터 생성 시작

> [!NOTE]
> 이 섹션의 단계에서는 모든 플랫폼에서 사용할 수 있도록 Node.js를 사용합니다. 다른 언어 예제는 **SendEvents** 디렉터리를 참조하세요.

1. 새 프롬프트, 쉘 또는 터미널을 열고 디렉터리를 **hdinsight-eventhub-example/SendEvents/nodejs**로 변경합니다. 응용 프로그램에서 필요한 종속성을 설치하려면 다음 명령을 사용합니다.
   
        npm install

2. 텍스트 편집기에서 **app.js** 파일을 열고 이전에 가져온 이벤트 허브 정보를 추가합니다.
   
        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
   
   > [!NOTE]
   > 이 예제에서는 Event Hub의 이름으로 **sensordata**를 사용하며 **송신** 클레임이 있는 정책 이름으로 **devices**를 사용한다고 가정합니다.

3. 다음 명령을 사용하여 이벤트 허브에 새 항목을 삽입합니다.
   
        node app.js
   
    Event Hub로 전송된 데이터가 포함된 여러 줄의 출력이 표시됩니다.
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>토폴로지 시작

1. 새 명령 프롬프트, 셸 또는 터미널을 열고 디렉터리를 **hdinsight-eventhub-example/TemperatureMonitor**로 변경한 후 다음 명령을 사용하여 토폴로지를 시작합니다.
   
        mvn compile exec:java
   
    이 명령은 토폴로지를 로컬 모드로 시작합니다. **dev.properties** 파일에 포함된 값은 Event Hubs 및 로컬 대시보드 웹 사이트에 대한 연결 정보를 제공합니다. 일단 시작되면 토폴로지는 Event Hub에서 항목을 읽고 로컬 컴퓨터에서 실행되는 대시보드에 전송합니다. 웹 대시보드에 다음 이미지와 유사한 줄이 표시됩니다.
   
    ![데이터가 표시된 대시보드](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. 이벤트 허브가 실행 중인 동안 이전 단계의 `node app.js` 명령을 사용하여 새 데이터를 대시보드로 보냅니다. 온도 값은 임의로 생성되기 때문에 큰 온도 변화를 표시하도록 그래프가 업데이트됩니다.
   
   > [!NOTE]
   > `node app.js` 명령을 사용할 때 **hdinsight-eventhub-example/SendEvents/Nodejs** 디렉터리에 있어야 합니다.

3. 대시보드가 업데이트되는지 확인한 후에 Ctrl+C를 사용하여 토폴로지를 중지합니다. Ctrl+C를 사용하여 로컬 웹 서버를 중지할 수도 있습니다.

## <a name="create-a-storm-and-hbase-cluster"></a>Storm 및 HBase 클러스터 만들기

이 섹션의 단계에서는 [Azure Resource Manager 템플릿](../azure-resource-manager/resource-group-template-deploy.md) 을 사용하여 Azure 가상 네트워크를 만들고 해당 가상 네트워크에서 Storm 및 HBase 클러스터를 만듭니다. 또한 이 템플릿은 Azure 웹앱을 만들고 대시보드의 복사본을 배포합니다.

> [!NOTE]
> 가상 네트워크는 Storm 클러스터에서 실행되는 토폴로지가 HBase Java API를 사용하여 HBase 클러스터와 직접 통신할 수 있도록 하는 데 사용됩니다.

이 문서에 사용되는 Resource Manager 템플릿은 공용 Blob 컨테이너 **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json**에 있습니다.

1. Azure에 로그인하고 Azure Portal에서 Azure Resource Manager 템플릿을 열려면 다음 단추를 클릭합니다.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.5.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. **사용자 지정 배포** 블레이드에서 다음 값을 입력합니다.
   
    ![HDInsight 매개 변수](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * **기본 클러스터 이름**: 이 값은 Storm 및 HBase 클러스터의 기본 이름으로 사용됩니다. 예를 들어 **abc**를 입력하면 **storm-abc**라는 Storm 클러스터와 **hbase-abc**라는 HBase 클러스터가 생성됩니다.
   * **클러스터 로그인 사용자 이름**: Storm 및 HBase 클러스터의 관리자 이름입니다.
   * **클러스터 로그인 암호**: Storm 및 HBase 클러스터의 관리자 사용자 암호입니다.
   * **SSH 사용자 이름**: Storm 및 HBase 클러스터를 만들기 위한 SSH 사용자입니다.
   * **SSH 암호**: Storm 및 HBase 클러스터에 대한 SSH 사용자의 암호입니다.
   * **위치**: 클러스터가 만들어지는 지역입니다.
     
     **확인** 을 클릭하여 매개 변수를 저장합니다.

3. **기본** 섹션에서 리소스 그룹을 만들거나 기존 리소스 그룹을 선택합니다.
4. **리소스 그룹 위치** 드롭다운 메뉴에서 **설정** 섹션의 **위치** 매개 변수에 대해 선택한 것과 동일한 위치를 선택합니다.
5. 사용 약관을 읽은 다음 **위에 명시된 사용 약관에 동의함**을 선택합니다.
6. 마지막으로 **대시보드에 고정**을 선택한 다음 **구매**를 선택합니다. 클러스터를 만드는 데 약 20분이 걸립니다.

리소스를 만들면 클러스터 및 웹 대시보드를 포함하는 리소스 그룹에 대한 블레이드로 리디렉션됩니다.

![vnet 및 클러스터에 대한 리소스 그룹 블레이드](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> HDInsight 클러스터의 이름은 **storm-BASENAME** 및 **hbase-BASENAME**입니다. 여기서 BASENAME은 템플릿에 제공된 이름입니다. 이후 단계에서 클러스터에 연결할 때 이러한 이름을 사용합니다. 또한 대시보드 사이트의 이름은 **basename-dashboard**입니다. 이 값은 이 문서의 뒷부분에서 사용됩니다.

## <a name="configure-the-dashboard-bolt"></a>대시보드 bolt 구성

웹앱으로 배포된 대시보드에 데이터를 전송하려면 **Config.properties** 파일에서 다음 줄을 수정해야 합니다.

    dashboard.uri: http://localhost:3000

`http://localhost:3000`을 `http://BASENAME-dashboard.azurewebsites.net`으로 변경하고 파일을 저장합니다. **BASENAME** 을 이전 단계에서 제공한 기본 이름으로 바꿉니다. 또한 이전에 만든 리소스 그룹을 사용하여 대시보드를 선택하고 URL을 확인할 수도 있습니다.

## <a name="create-the-hbase-table"></a>HBase 테이블 만들기

HBase에 데이터를 저장하려면 먼저 테이블을 만들어야 합니다. Storm 토폴로지 내에서 리소스를 만들려고 하면 여러 인스턴스가 동일한 리소스를 만들려고 할 수 있으므로 Storm이 데이터를 써야 하는 리소스를 미리 만듭니다. 토폴로지 외부에 리소스를 만들고 읽기/쓰기 및 분석에 Storm을 사용합니다.

1. SSH를 사용하여 클러스터 생성 중에 템플릿에 제공한 SSH 사용자 이름 및 암호를 사용하여 HBase 클러스터에 연결합니다. 예를 들어 `ssh` 명령을 사용하여 연결하는 경우 다음 구문을 사용합니다.
   
        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
   
    이 명령에서 **USERNAME**을 클러스터를 만들 때 제공한 SSH 사용자 이름으로 바꾸고 **BASENAME**을 제공한 기본 이름으로 바꿉니다. 확인 메시지가 표시되면 SSH 사용자의 암호를 입력합니다.

2. SSH 세션에서 HBase 셸을 시작합니다.
   
        hbase shell
   
    셸이 로드되면 `hbase(main):001:0>` 프롬프트가 표시됩니다.

3. HBase 셸에서 다음 명령을 입력하여 센서 데이터를 저장할 테이블을 만듭니다.
   
        create 'SensorData', 'cf'

4. 다음 명령을 사용하여 테이블이 생성되었는지 확인합니다.
   
        scan 'SensorData'
   
    이렇게 하면 테이블에 0개의 행이 있음을 나타내는 다음 예제와 비슷한 정보가 반환됩니다.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. `exit`를 눌러 HBase 셸을 종료합니다.

## <a name="configure-the-hbase-bolt"></a>HBase bolt 구성

Storm 클러스터에서 HBase에 쓰려면 HBase 클러스터의 구성 세부 정보를 HBase bolt에 제공해야 합니다. 이 예제에서는 HBase 클러스터에서 **hbase-site.xml** 파일을 사용합니다.

### <a name="download-the-hbase-sitexml"></a>hbase-site.xml 다운로드

명령 프롬프트에서 SCP를 사용하여 클러스터에서 **hbase-site.xml** 파일을 다운로드합니다. 다음 예제에서는 **USERNAME**을 클러스터를 만들 때 제공한 SSH 사용자로 바꾸고 **BASENAME**을 이전에 제공한 기본 이름으로 바꿉니다. 확인 메시지가 표시되면 SSH 사용자의 암호를 입력합니다. `/path/to/TemperatureMonitor/resources/hbase-site.xml` 을 TemperatureMonitor 프로젝트의 이 파일 경로로 바꿉니다.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

이 명령은 **hbase-site.xml**을 지정된 경로에 다운로드합니다.

### <a name="enable-the-hbase-bolt"></a>HBase bolt 사용

HBase bolt 구성 요소를 사용하려면 **TemperatureMonitor/src/main/java/com/microsoft/examples/Temperature.java** 파일을 열고 다음 줄의 주석처리를 제거합니다.

    // topologyBuilder.setBolt("HBase", new HBaseBolt("SensorData", mapper).withConfigKey("hbase.conf"), spoutConfig.getPartitionCount())
    //  .fieldsGrouping("Parser", "hbasestream", new Fields("deviceid")).setNumTasks(spoutConfig.getPartitionCount());

줄의 주석 처리를 제거한 후에 파일을 저장합니다.

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>솔루션을 빌드, 패키지하여 HDInsight에 배포

개발 환경에서 다음 단계를 수행하여 Storm 토폴로지를 Storm 클러스터에 배포합니다.

1. **TemperatureMonitor** 디렉터리에서 다음 명령을 사용하여 새 빌드를 수행하고 프로젝트에서 JAR 패키지를 만듭니다.
   
        mvn clean compile package
   
    이 명령은 프로젝트의 **대상** 디렉터리에 **TemperatureMonitor-1.0-SNAPSHOT.jar**라는 이름을 만듭니다.

2. scp를 사용하여 **TemperatureMonitor-1.0-SNAPSHOT.jar** 파일을 Storm 클러스터에 업로드합니다. 다음 예제에서는 **USERNAME**을 클러스터를 만들 때 제공한 SSH 사용자로 바꾸고 **BASENAME**을 이전에 제공한 기본 이름으로 바꿉니다. 확인 메시지가 표시되면 SSH 사용자의 암호를 입력합니다.
   
        scp target/TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar

   > [!NOTE]
   > 파일 업로드에 몇 분 정도 걸릴 수 있습니다.

3. 파일이 업로드되면 SSH를 사용하여 클러스터에 연결합니다.
   
        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. 토폴로지를 시작하려면 SSH 세션에서 다음 명령을 사용합니다.
   
        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar com.microsoft.examples.Temperature temperature

5. 토폴로지가 시작되면 브라우저에서 Azure에 게시한 웹 사이트를 열고 `node app.js` 명령을 사용하여 이벤트 허브로 데이터를 보냅니다. 정보를 표시하기 위해 웹 대시보드가 업데이트됩니다.
   
    ![dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase 데이터 보기

다음 단계를 사용하여 HBase에 연결하며 데이터가 테이블에 기록되었는지 확인합니다.

1. SSH를 사용하여 HBase 클러스터에 연결합니다.
   
        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. SSH 세션에서 HBase 셸을 시작합니다.
   
        hbase shell
   
    셸이 로드되면 `hbase(main):001:0>` 프롬프트가 표시됩니다.

3. 테이블의 행 보기:
   
        scan 'SensorData'
   
    이 명령은 테이블에 데이터가 있음을 나타내는 다음 텍스트와 비슷한 정보를 반환합니다.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > 이 검색 작업은 테이블에서 최대 10개의 행을 반환합니다.

## <a name="delete-your-clusters"></a>클러스터 삭제
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

클러스터, 저장소, 웹앱을 한꺼번에 삭제하려면 해당 항목을 포함하는 리소스 그룹을 삭제합니다.

## <a name="next-steps"></a>다음 단계

HDInsight의 Storm 토폴로지에 대한 자세한 내용은 [HDInsight의 Storm에 대한 예제 토폴로지](hdinsight-storm-example-topology.md)를 참조하세요.

Apache Storm에 대한 자세한 내용은 [Apache Storm](https://storm.incubator.apache.org/) 사이트를 참조하세요.

HDInsight의 HBase에 대한 자세한 내용은 [HDInsight HBase 개요](hdinsight-hbase-overview.md)를 참조하세요.

Socket.io에 대한 자세한 내용은 [socket.io](http://socket.io/) 사이트를 참조하세요.

D3.js에 대한 자세한 내용은 [D3.js - 데이터 기반 문서](http://d3js.org/)를 참조하세요.

Java에서 토폴로지를 만들기에 대한 자세한 내용은 [HDInsight의 Apache Storm용 Java 토폴로지 개발](hdinsight-storm-develop-java-topology.md)을 참조하세요.

.NET에서 토폴로지를 만들기에 대한 자세한 내용은 [Visual Studio를 사용하여 HDInsight의 Apache Storm용 C# 토폴로지 개발](hdinsight-storm-develop-csharp-visual-studio-topology.md)을 참조하세요.

[azure-portal]: https://portal.azure.com

