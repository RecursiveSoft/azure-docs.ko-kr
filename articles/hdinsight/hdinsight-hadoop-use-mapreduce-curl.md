---
title: "HDInsight에서 Hadoop과 MapReduce 및 Curl 사용 | Microsoft Docs"
description: "원격으로 Curl을 사용하여 HDInsight에서 Hadoop으로 MapReduce 작업을 실행하는 방법에 대해 알아봅니다."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 41733f76e5f0c82c5321158d6a919f35f6faee3a
ms.lasthandoff: 03/21/2017


---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Curl을 사용하여 HDInsight에서 Hadoop과 MapReduce 작업 실행
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

이 문서에서 HDInsight 클러스터의 Hadoop에서 MapReduce 작업을 실행하기 위해 Curl을 사용하는 방법에 대해 배웁니다.

Curl은 MapReduce 작업 실행을 원시 HTTP 요청을 사용하여 HDInsight와 함께 작용하는 방법을 설명하기 위해 사용됩니다. 이 Curl은 HDInsight 클러스터에서 제공하는 WebHCat REST API(이전의 Templeton)를 사용하여 작동합니다.

> [!NOTE]
> Linux 기반 Hadoop 서버를 익숙하게 사용하지만 HDInsight는 처음인 경우 [HDInsight의 Linux 기반 Hadoop에 대해 알아야 할 정보](hdinsight-hadoop-linux-information.md)를 참조하세요.


## <a id="prereq"></a>필수 조건
이 문서의 단계를 완료하려면 다음이 필요합니다.

* HDInsight 클러스터의 Hadoop
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Curl을 사용하여 MapReduce 작업 실행
> [!NOTE]
> Curl 또는 기타 모든 REST 통신을 WebHCat 함께 사용하면 HDInsight 클러스터 관리자의 사용자 이름 및 암호를 제공하여 요청을 인증해야 합니다. 또한 요청을 보낼 서버에 사용되는 URI의 일부로 클러스터 이름을 사용해야 합니다.
> 
> 이 섹션의 명령에서 **USERNAME**은 클러스터에 대해 인증할 사용자로 바꾸고 **PASSWORD**는 사용자 계정의 암호로 바꿉니다. **CLUSTERNAME** 을 클러스터의 이름으로 바꿉니다.
> 
> REST API는 [기본 액세스 인증](http://en.wikipedia.org/wiki/Basic_access_authentication)을 사용하여 보안이 유지됩니다. 자격 증명이 안전하게 서버에 전송되려면 항상 HTTPS를 사용하여 요청을 보냅니다.
> 
> 

1. 명령줄에서 다음 명령을 사용하여 HDInsight 클러스터에 연결할 수 있는지 확인합니다.
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    그러면 다음과 같은 응답이 표시됩니다.
   
        {"status":"ok","version":"v1"}
   
    이 명령에서 사용된 매개 변수는 다음과 같습니다.
   
   * **-u**: 요청을 인증하는 데 사용되는 사용자 이름 및 암호입니다.
   * **-G**: GET 요청임을 나타냅니다.
     
     URI 시작 부분인 **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**은 모든 요청에서 동일합니다.
2. MapReduce 작업을 제출하려면 다음 명령을 사용합니다.
   
        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
   
    URI(/ mapreduce/jar)의 끝이 WebHCat을 지시하면 이 요청은 jar 파일에 있는 클래스에서 MapReduce 작업을 시작합니다. 이 명령에서 사용된 매개 변수는 다음과 같습니다.
   
   * **-d**: `-G`가 사용되지 않으므로 요청은 POST 메서드로 기본 설정됩니다. `-d` 는 요청과 함께 전송되는 데이터 값을 지정합니다.
     
     * **user.name**: 명령을 실행하는 사용자입니다.
     * **jar**: 실행할 클래스가 포함된 jar 파일의 위치입니다.
     * **class**: MapReduce 논리가 포함된 클래스입니다.
     * **arg**: MapReduce 작업으로 전달할 인수입니다. 이 예제에서는 출력에 사용되는 디렉터리와 입력 텍스트 파일입니다.
     
     이 명령은 작업 상태를 확인하는데 사용할 수 있는 작업 ID를 반환해야 합니다.
     
       {"id":"job_1415651640909_0026"}
3. 작업 상태를 확인하려면 다음 명령을 사용합니다. **JOBID** 를 이전 단계에서 반환된 값으로 바꿉니다. 예를 들어 반환 값이 `{"id":"job_1415651640909_0026"}`인 경우 JOBID는 `job_1415651640909_0026`이 됩니다.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    작업이 완료되면 상태는 "SUCCEEDED"가 됩니다.
   
   > [!NOTE]
   > 이 Curl 요청은 작업에 관한 정보가 있는 JSON 문서를 반환합니다. jq는 상태 값을 검색하는 데에만 사용됩니다.
   > 
   > 
4. 작업 상태가 **SUCCEEDED**로 변경되면 Azure Blob 저장소에서 작업 결과를 검색할 수 있습니다. 쿼리와 함께 전달된 `statusdir` 매개 변수에는 출력 파일의 위치(이 경우 **wasbs:///example/curl**)가 포함됩니다. HDInsight 클러스터에서 사용하는 기본 저장소 컨테이너의 **example/curl** 디렉터리에 작업의 출력을 저장합니다.

[Azure CLI](../cli-install-nodejs.md)를 사용하여 이러한 파일을 나열하고 다운로드할 수 있습니다. 예를 들어 **example/curl**에 파일을 나열하려면 다음 명령을 사용합니다.

    azure storage blob list <container-name> example/curl

파일을 다운로드하려면 다음을 사용합니다.

    azure storage blob download <container-name> <blob-name> <destination-file>

> [!NOTE]
> `-a` 및 `-k` 매개 변수를 사용하여 Blob을 포함하는 저장소 계정 이름을 지정하거나 **AZURE\_STORAGE\_ACCOUNT** 및 **AZURE\_STORAGE\_ACCESS\_KEY** 환경 변수를 설정해야 합니다. 자세한 내용은 [HDInsight에 데이터를 업로드하는 방법](hdinsight-upload-data.md) 을 참조하세요.
> 
> 

## <a id="summary"></a>요약
이 문서에서 볼 수 있듯이, 실행, 모니터링 및 HDInsight 클러스터의 Hive 작업의 결과 확인하려면 원시 HTTP 요청을 사용할 수 있습니다.

이 문서에 사용된 REST 인터페이스에 대한 자세한 내용은 [WebHCat 참조](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)를 참조하세요.

## <a id="nextsteps"></a>다음 단계
HDInsight의 MapReduce 작업에 대한 일반적인 정보:

* [HDInsight에서 Hadoop과 MapReduce 사용](hdinsight-use-mapreduce.md)

HDInsight에서 Hadoop으로 작업하는 다른 방법에 관한 정보:

* [HDInsight에서 Hadoop과 Hive 사용](hdinsight-use-hive.md)
* [HDInsight에서 Hadoop과 Pig 사용](hdinsight-use-pig.md)


