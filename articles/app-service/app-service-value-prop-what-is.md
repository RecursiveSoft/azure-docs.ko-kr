---
title: "웹, 모바일, API 앱에 대한 Azure App Service | Microsoft Docs"
description: "Azure 앱 서비스를 사용하여 웹 및 모바일 앱을 개발, 배포 및 관리하는 방법을 알아봅니다."
keywords: "앱 서비스, Azure 앱 서비스, 앱 서비스 비용, 규모, 확장성, 앱 배포, Azure 앱 배포, PaaS, Platform-as-a-Service, 웹사이트, 웹 사이트, 웹, Azure 모바일"
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: a773e43b28b144dd8341b276eee3fa504d4f1080
ms.lasthandoff: 03/21/2017


---
# <a name="what-is-azure-app-service"></a>Azure 앱 서비스 정의
*앱 서비스* 는 Microsoft Azure의 PaaS( [platform-as-a-service](https://en.wikipedia.org/wiki/Platform_as_a_service) ) 제품입니다. 플랫폼 및 장치에 웹 및 모바일 앱을 만듭니다. SaaS 솔루션과 앱을 통합하고 온-프레미스 응용 프로그램을 사용하여 연결하고 비즈니스 프로세스를 자동화합니다. Azure는 공유 VM 리소스 또는 전용 VM을 선택하여 완전히 관리되는 VM(가상 컴퓨터)에서 앱을 실행합니다.

앱 서비스는 이전에 개별적으로 Azure 웹 사이트 및 Azure 모바일 서비스로 전달한 웹 및 모바일 기능을 포함합니다. 또한 비즈니스 프로세스를 자동화하고 클라우드 API를 호스팅하는 새 기능을 포함합니다. 단일 통합 서비스인 앱 서비스를 통해 웹 사이트, 모바일 앱 백 엔드, RESTful API 및 비즈니스 프로세스 등 다양한 구성 요소를 단일 솔루션으로 작성할 수 있습니다.

다음 4분 비디오에서는 앱 서비스와 이전 Azure 제품의 연결점 및 새로운 기능에 대한 간략한 설명을 제공합니다.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>앱 서비스를 사용하는 이유는 무엇인가요?
다음은 앱 서비스의 몇 가지 주요 기능입니다.

* **여러 언어 및 프레임워크** - 앱 서비스에서는 ASP.NET, Node.js, Java, PHP 및 Python을 최고 수준으로 지원합니다. 앱 서비스 VM에서 [Windows PowerShell 및 기타 스크립트 또는 실행 파일](../app-service-web/web-sites-create-web-jobs.md) 을 실행할 수도 있습니다.
* **DevOps 최적화** - Visual Studio Team Services, GitHub, BitBucket으로 [연속 통합 및 배포](../app-service-web/app-service-continuous-deployment.md) 를 설정합니다. [테스트 및 스테이징 환경](../app-service-web/web-sites-staged-publishing.md)을 통해 업데이트를 승격합니다. [A/B 테스트](../app-service-web/app-service-web-test-in-production-get-start.md)를 수행합니다. [Azure PowerShell](/powershell/azureps-cmdlets-docs) 또는 [플랫폼 간 CLI(명령줄 인터페이스)](../cli-install-nodejs.md)를 사용하여 App Service에서 앱을 관리합니다.
* **고가용성을 가진 글로벌 규모 조정** - 수동 또는 자동으로 규모를 [강화](../app-service-web/web-sites-scale.md) 또는 [확장](../monitoring-and-diagnostics/insights-how-to-scale.md)합니다. Microsoft의 글로벌 데이터 센터 인프라의 모든 위치에서 앱을 호스팅하고 앱 서비스 [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) 를 사용하면 고가용성이 보장됩니다.
* **SaaS 플랫폼 및 온-프레미스 데이터에 연결** - 엔터프라이즈 시스템(예: SAP, Siebel 및 Oracle), SaaS 서비스(예: Salesforce 및 Office 365), 인기 있는 인터넷 서비스(예: Facebook 및 Twitter) 등을 위한 50개 이상의 [커넥터](../connectors/apis-list.md) 에서 선택합니다. [하이브리드 연결](../biztalk-services/integration-hybrid-connection-overview.md) 및 [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md)를 사용하여 온-프레미스 데이터에 액세스합니다.
* **보안 및 규정 준수** - 앱 서비스는 [ISO, SOC 및 PCI 규격](https://www.microsoft.com/TrustCenter/)입니다.
* **응용 프로그램 템플릿** - WordPress, Joomla, Drupal 등의 인기 있는 오픈 소스 소프트웨어를 설치하는 마법사를 사용할 수 있는 [Azure Marketplace](https://azure.microsoft.com/marketplace/)의 광범위한 템플릿 목록에서 선택합니다.
* **Visual Studio 통합** - Visual Studio의 전용 도구는 생성, 배포, 디버깅 작업을 간소화합니다.

## <a name="app-types-in-app-service"></a>앱 서비스의 앱 유형
App Service는 여러 *앱 형식*을 제공하며 각각은 특정 워크로드를 호스트하도록 계획되었습니다.

* [**Web Apps**](../app-service-web/app-service-web-overview.md) - 웹 사이트와 웹 응용 프로그램을 호스팅합니다.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) 모바일 앱 백 엔드를 호스팅합니다.
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - 클라우드 API를 호스팅합니다.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) - 비즈니스 프로세스를 자동화하고 코드를 작성하지 않고도 클라우드를 통해 시스템과 데이터를 통합합니다.

여기서 *앱* 이라는 단어는 워크로드를 전용으로 실행하는 호스팅 리소스를 의미합니다. 예를 들어 "웹앱"의 경우 함께 브라우저에 대한 기능을 제공하는 계산 리소스와 응용 프로그램 코드를 모두 웹앱으로 생각하는 것이 익숙할 것입니다. 하지만 앱 서비스에서 *웹앱* 은 Azure에서 응용 프로그램 코드를 호스팅하기 위해 제공하는 계산 리소스입니다. 

응용 프로그램은 다른 종류의 여러 앱 서비스 앱으로 구성될 수 있습니다. 예를 들어 응용 프로그램이 웹 프런트 엔드와 RESTful API 백 엔드로 구성되는 경우 다음을 수행할 수 있습니다.

- 프런트 엔드와 api를 단일 웹앱에 배포합니다.  
- 프런트 엔드 코드를 웹앱에 배포하고 백 엔드 코드를 API 앱에 배포합니다. 



## <a name="app-service-plans"></a>앱 서비스 계획
[App Service 계획](azure-web-sites-web-hosting-plans-in-depth-overview.md)은 앱을 호스트하는 데 사용되는 실제 리소스의 컬렉션을 나타냅니다.

App Service 계획은 다음을 정의합니다.

- **지역**(미국 서부, 미국 동부 등)
- **확장 개수**(1,&2;,&3;개 인스턴스 등)
- **인스턴스 크기**(소, 중, 대)
- **SKU**(무료, 공유, 기본, 표준, 프리미엄)

**App Service 계획**에 할당된 모든 응용 프로그램은 정의된 리소스를 공유하므로 여러 앱을 호스팅할 때 비용을 절감할 수 있습니다.

**App Service 계획**은 **무료** 및 **공유** SKU에서 **기본**, **표준**, **프리미엄** SKU로 확장하여 이에 따라 더 많은 리소스 및 기능에 액세스할 수 있도록 합니다. App Service 계획이 **기본** 이상으로 설정되면 VM **크기** 및 확장 개수도 제어할 수 있습니다.

App Service 계획의 **SKU** 및 **규모**에 따라 호스트되는 앱 수가 아닌 비용이 결정됩니다. 

확장성 및 네트워크 격리가 필요한 경우 [앱 서비스 환경](../app-service-web/app-service-app-service-environment-intro.md)에서 앱을 실행할 수 있습니다.

## <a name="pricing"></a>가격
앱 서비스 비용에 대한 정보는 [앱 서비스 가격 책정](https://azure.microsoft.com/pricing/details/app-service/)을 참조하세요.

## <a name="test-drive-app-service"></a>App Service 시험 사용
[샘플 웹앱, 모바일 앱 또는 논리 앱을 만들고](https://azure.microsoft.com/try/app-service/)&1;시간 동안 재생합니다. 신용 카드나 약정, 번거로운 과정도 필요하지 않습니다.

또는 [무료 Azure 계정](https://azure.microsoft.com/pricing/free-trial/)을 열고 시작 자습서 중 하나를 사용해 봅니다.

* [자습서: 웹앱 만들기](../app-service-web/app-service-web-get-started.md)
* [자습서: 모바일 앱 만들기](../app-service-mobile/app-service-mobile-android-get-started.md)
* [자습서: API 앱 만들기](../app-service-api/app-service-api-dotnet-get-started.md)
* [자습서: 논리 앱 만들기](../logic-apps/logic-apps-create-a-logic-app.md)


