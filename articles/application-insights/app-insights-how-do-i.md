---
title: "Application Insights에서 어떻게 할까요? | Microsoft Docs"
description: "Application Insights의 FAQ"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 9a3df0ad2483471023ebb954d613bc5cad8fb7bf
ms.openlocfilehash: 9e54ee2d67a8dfb5b480db01219e128607e26f51


---
# <a name="how-do-i--in-application-insights"></a>Application Insights에서 다음을 수행하는 방법
## <a name="get-an-email-when-"></a>전자 메일을 받는 경우
### <a name="email-if-my-site-goes-down"></a>내 사이트가 다운되면 전자 메일로 알림
[가용성 웹 테스트](app-insights-monitor-web-app-availability.md)를 설정합니다.

### <a name="email-if-my-site-is-overloaded"></a>내 사이트가 과부하되면 전자 메일로 알림
[서버 응답 시간](app-insights-alerts.md) 에서 **경고**를 설정합니다. 1-2초 임계값이 적용됩니다.

![](./media/app-insights-how-do-i/030-server.png)

응용 프로그램이 오류 코드를 반환하여 부하의 흔적을 표시할 수도 있습니다. **실패한 요청**에 대한 경고를 설정합니다.

**서버 예외**에 대한 경고를 설정하기 위해 데이터를 확인하는 데 [일부 추가 설치](app-insights-asp-net-exceptions.md) 를 수행해야 할 수 있습니다.

### <a name="email-on-exceptions"></a>예외에 대해 메일 보내기
1. [예외 모니터링 설정](app-insights-asp-net-exceptions.md)
2. [경고 설정](app-insights-alerts.md) 

### <a name="email-on-an-event-in-my-app"></a>내 응용 프로그램에서 이벤트 발생 시 전자 메일로 알림
특정 이벤트가 발생할 때 전자 메일을 받으려 한다고 가정해 보겠습니다. Application Insights는 이 기능을 직접 제공하지 않지만 [메트릭이 임계값에 도달했을 때](app-insights-alerts.md)경고를 보낼 수 있습니다.

경고는 사용자 지정 이벤트는 아니지만 [사용자 지정 메트릭](app-insights-api-custom-events-metrics.md#trackmetric)에서 설정할 수 있습니다. 이벤트 발생 시 메트릭 향상을 위한 몇 가지 코드를 작성합니다.

    telemetry.TrackMetric("Alarm", 10);

또는

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

경고에는 두 상태가 있기 때문에 경고 종료를 고려할 때 낮은 값을 보내야 합니다.

    telemetry.TrackMetric("Alarm", 0.5);

[메트릭 탐색기](app-insights-metrics-explorer.md) 에서 차트를 만들어 경고를 확인합니다.

![](./media/app-insights-how-do-i/010-alarm.png)

이제 메트릭이 짧은 기간 동안 중간 값 위로 상승하면 발생하는 경고를 설정합니다.

![](./media/app-insights-how-do-i/020-threshold.png)

평균 기간을 최소로 설정합니다.

메트릭이 임계값 위와 아래로 가면 전자 메일을 받게 됩니다.

몇 가지 고려할 점은 다음과 같습니다.

* 경고는 "경고" 및 "정상"의 두 상태가 있습니다. 상태는 메트릭 수신 시에만 평가됩니다.
* 상태가 변경될 때만 전자 메일을 보냅니다. 이 때문에 높은 값과 낮은 값의 메트릭을 모두 보내야 합니다.
* 경고를 평가하기 위해 이전 기간 동안 수신한 값에서 평균을 취합니다. 이 작업은 메트릭이 수신될 때마다 발생하므로 설정한 기간보다 더 자주 전자 메일이 전달될 수 있습니다.
* "경고" 및 "정상"에서 모두 전자 메일을 보내므로 원샷 이벤트를&2;상태 조건으로 간주할 수 있습니다. 예를 들어, "작업 완료" 이벤트 대신, 작업 시작과 종료 시 전자 메일을 받는 "작업 진행 중"  조건을 적용합니다.

### <a name="set-up-alerts-automatically"></a>자동 경고 설정
[PowerShell을 사용하여 새 경고 만들기](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>PowerShell을 사용하여 Application Insights 관리
* [새 리소스 만들기](app-insights-powershell-script-create-resource.md)
* [새 경고 만들기](app-insights-alerts.md#automation)

## <a name="application-versions-and-stamps"></a>응용 프로그램 버전 및 스탬프
### <a name="separate-the-results-from-dev-test-and-prod"></a>개발, 테스트 및 프로덕션에서 결과 분리
* 서로 다른 환경에서 서로 다른 iKey 설정
* 서로 다른 스탬프(개발, 테스트, 프로덕션)에 대해 서로 다른 속성 값으로 원격 분석 태그

[자세히 알아보기](app-insights-separate-resources.md)

### <a name="filter-on-build-number"></a>빌드 번호 필터링
앱의 새 버전을 게시하면서 다른 빌드의 원격 분석을 구분하고자 할 수 있습니다.

응용 프로그램 버전 속성을 설정하여 [검색](app-insights-diagnostic-search.md) 및 [메트릭 탐색기](app-insights-metrics-explorer.md) 결과를 필터링할 수 있습니다.

![](./media/app-insights-how-do-i/050-filter.png)

응용 프로그램 버전 속성은 몇 가지 다른 방법으로 설정할 수 있습니다.

* 직접 설정:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* [원격 분석 이니셜라이저](app-insights-api-custom-events-metrics.md#defaults) 에서 해당 줄을 래핑하여 모든 TelemetryClient 인스턴스가 일관되게 설정되었는지 확인합니다.
* [ASP.NET] `BuildInfo.config`에서 버전을 설정합니다. 웹 모듈은 BuildLabel 노드에서 버전을 선택합니다. 프로젝트에 이 파일을 포함하고 솔루션 탐색기에서 항상 복사 속성을 설정합니다.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] MSBuild에서 BuildInfo.config를 자동으로 생성합니다. 이 작업을 수행하려면.csproj 파일에 몇 줄을 추가합니다.

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    그러면 *yourProjectName*.BuildInfo.config 파일이 생성됩니다. 게시 프로세스의 이름이 BuildInfo.config로 바뀝니다.

    Visual Studio를 사용하여 빌드할때 빌드 레이블에는 자리 표시자(AutoGen_...)가 포함됩니다. MSBuild로 빌드할 때는 정확한 버전 번호가 입력됩니다.

    MSBuild가 버전 번호를 생성하게 하려면 AssemblyReference.cs에서 `1.0.*` 같이 버전을 설정합니다.

## <a name="monitor-backend-servers-and-desktop-apps"></a>백엔드 서버 및 데스크톱 앱 모니터링
[Windows Server SDK 모듈을 사용합니다](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>데이터 가상화
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>여러 앱의 메트릭이 있는 대시보드
* [메트릭 탐색기](app-insights-metrics-explorer.md)에서 차트를 사용자 지정하고 즐겨찾기에 저장합니다. Azure 대시보드에 고정합니다.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>다른 원본 및 Application Insights의 데이터가 표시된 대시보드
* [Power BI에 원격 분석을 내보냅니다](app-insights-export-power-bi.md).

또는

* SharePoint를 대시보드로 사용하여 SharePoint 웹 파트에 데이터를 표시 합니다. [연속 내보내기 및 스트림 분석을 사용하여 SQL로 내보냅니다](app-insights-code-sample-export-sql-stream-analytics.md).  PowerView를 사용하여 데이터베이스를 검사하고 PowerView에 대한 SharePoint 웹 파트를 만듭니다.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>익명 또는 인증된 사용자 필터링
사용자가 로그인할 경우 [인증된 사용자 ID](app-insights-api-custom-events-metrics.md#authenticated-users)를 설정할 수 있습니다. 이 작업은 자동으로 수행되지 않습니다.

그런 다음 다음을 수행할 수 있습니다.

* 특정 사용자 ID 검색

![](./media/app-insights-how-do-i/110-search.png)

* 익명 또는 인증된 사용자에 대해 메트릭 필터링

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>속성 이름 또는 값 수정
[필터](app-insights-api-filtering-sampling.md#filtering)를 만듭니다. 그러면 원격 분석을 수정하거나 필터링한 후 앱에서 Application Insights로 전송할 수 있습니다.

## <a name="list-specific-users-and-their-usage"></a>특정 사용자와 그 사용 방법을 나열 
[특정 사용자만 검색](#search-specific-users)하려는 경우 [인증된 사용자 ID](app-insights-api-custom-events-metrics.md#authenticated-users)를 설정할 수 있습니다.

사용자가 보는 페이지, 로그인 빈도 등과 같은 데이터와 사용자 목록이 필요한 경우 두 가지 옵션이 있습니다.

* [인증된 사용자 ID를 설정하고](app-insights-api-custom-events-metrics.md#authenticated-users) [데이터베이스로 내보내고](app-insights-code-sample-export-sql-stream-analytics.md) 적합한 도구를 사용하여 사용자 데이터를 분석합니다.
* 사용자 수가 적은 경우, 관심이 있는 데이터를 사용하는 사용자 지정 이벤트나 메트릭을 메트릭 값 또는 이벤트 이름 형태로 보내고 사용자 ID를 속성으로 설정합니다. 페이지 보기를 분석하려면 표준 JavaScript trackPageView 호출을 대체합니다. 서버 측 원격 분석을 분석하려면 원격 분석 이니셜라이저를 사용하여 사용자 ID를 모든 서버 원격 분석에 추가합니다. 그런 다음 메트릭과 사용자 ID에 대한 검색을 필터링 및 분할할 수 있습니다.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Application Insights에 대한 내 앱의 트래픽 줄이기
* [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)에서 성능 카운터 수집기 등 필요하지 않은 모듈을 모두 사용하지 않도록 설정합니다.
* SDK에서 [샘플링 및 필터링](app-insights-api-filtering-sampling.md)을 사용합니다.
* 웹 페이지에서 모든 페이지 뷰에 대해 보고되는 Ajax 호출 수를 제한합니다. 스크립트 조각에서 `instrumentationKey:...` 뒤에 `,maxAjaxCallsPerView:3`(또는 적절한 숫자)을 삽입합니다.
* [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric)을 사용하는 경우 결과를 보내기 전에 메트릭 값의 배치 집계를 계산합니다. 이를 제공하는 TrackMetric() 오버로드가 있습니다.

[가격 책정 및 할당량](app-insights-pricing.md)에 대해 자세히 확인합니다.

## <a name="disable-telemetry"></a>원격 분석 사용 안 함

서버로부터 원격 분석의 컬렉션 및 전송을 **동적으로 중지 및 시작** 하려면:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



**선택한 표준 수집기(예: 성능 카운터, HTTP 요청 또는 종속성)를 사용하지 않도록 설정** 하려면 [ApplicationInsights.config](app-insights-api-custom-events-metrics.md)에서 관련 줄을 삭제하거나 주석으로 처리합니다. 사용자 고유의 TrackRequest 데이터를 전송하려는 경우를 예로 들 수 있습니다.

## <a name="view-system-performance-counters"></a>시스템 성능 카운터 보기
메트릭 탐색기에서 표시할 수 있는 메트릭 중에는 시스템 성능 카운터 집합이 있습니다. 이름이 **서버** 인 미리 정의된 블레이드에서 그중 몇 가지를 표시합니다.

![Application Insights 리소스를 열고서버 클릭](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>성능 카운터 데이터가 없는 경우
* **IIS 서버** . [상태 모니터를 설치합니다](app-insights-monitor-performance-live-website-now.md).
* **Azure 웹 사이트** - 성능 카운터는 아직 지원되지 않습니다. Azure 웹 사이트 제어판의 표준 부분으로 몇 가지 메트릭을 가져올  수 있습니다.
* **Unix 서버** - [collectd 설치](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>더 많은 성능 카운터를 표시하려면
* 먼저 [새 차트를 추가하고](app-insights-metrics-explorer.md) 제공한 기본 집합에 카운터가 있는지 확인합니다.
* 없으면 [성능 카운터 모듈에서 수집한 집합에 카운터를 추가합니다](app-insights-performance-counters.md).



<!--HONumber=Feb17_HO1-->


