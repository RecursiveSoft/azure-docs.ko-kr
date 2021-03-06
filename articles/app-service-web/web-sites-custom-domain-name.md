---
title: "Azure 앱에 사용자 지정 도메인 이름 매핑"
description: "Azure 앱 서비스에서 앱으로 사용자 지정 도메인 이름(베니티 도메인)을 매핑하는 방법에 대해 알아봅니다."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 48644a39-107c-45fb-9cd3-c741974ff590
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 59565c22ecd42985e8a6b81c4983fc2e87637e36
ms.openlocfilehash: 589701270770494e4ec4d127a252712249da9f3a
ms.lasthandoff: 02/16/2017


---
# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Azure 앱에 사용자 지정 도메인 이름 매핑
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

이 문서는 웹앱, 모바일 앱 백 엔드 또는 [Azure 앱 서비스](../app-service/app-service-value-prop-what-is.md)의 API 앱에 사용자 지정 도메인 이름을 수동으로 매핑하는 방법을 보여 줍니다. 

> [!NOTE] 
> 언제든지 [Azure에서 직접 사용자 지정 도메인 이름을 구입](custom-dns-web-site-buydomains-web-app.md)할 수 있습니다.
>
>

앱에 사용자 지정 도메인을 매핑하는 세 가지 주요 단계가 있습니다.

1. [*(A 레코드만)* 앱의 IP 주소를 가져옵니다](#vip)의 API 앱에 사용자 지정 도메인 이름을 수동으로 매핑하는 방법을 보여 줍니다.
2. [도메인을 앱에 매핑하는 DNS 레코드를 만듭니다](#createdns). 
   * **어디에서**: 도메인 등록 기관의 고유한 관리 도구(예: Azure DNS, GoDaddy 등).
   * **왜**: 도메인 등록 기관에서 Azure 앱에 대해 원하는 사용자 지정 도메인을 확인하는 방법을 알도록 합니다.
3. [Azure 앱에 대해 사용자 지정 도메인 이름을 사용](#enable)합니다.
   * **어디에서**: [Azure 포털](https://portal.azure.com).
   * **왜**: 앱에서 사용자 지정 도메인 이름에 대한 요청에 응답하는 방법을 알도록 합니다.
4. [DNS 전파를 확인합니다](#verify).

## <a name="types-of-domains-you-can-map"></a>매핑할 수 있는 도메인 형식
Azure 앱 서비스를 사용하면 다음 범주의 사용자 지정 도메인을 앱에 매핑할 수 있습니다.

* **루트 도메인** - 도메인 등록 기관을 통해 예약한 도메인 이름입니다(일반적으로 `@` 호스트 레코드로 표시). 
  예를 들어 **contoso.com**입니다.
* **하위 도메인** - 루트 도메인 아래에 있는 모든 도메인입니다. 예를 들어 **www.contoso.com**(`www` 호스트 레코드로 표시됨)입니다.  동일한 루트 도메인의 다양한 하위 도메인을 Azure의 다양한 앱에 매핑할 수 있습니다.
* **와일드 카드 도메인** - [가장 왼쪽의 DNS 레이블이 `*`](https://en.wikipedia.org/wiki/Wildcard_DNS_record)인 하위 도메인입니다(예: 호스트 레코드 `*` 및 `*.blogs`). 예를 들어 **\*.contoso.com**입니다.

## <a name="types-of-dns-records-you-can-use"></a>사용할 수 있는 DNS 레코드 유형
필요에 따라 두 가지 서로 다른 표준 DNS 레코드 유형을 사용하여 사용자 지정 도메인을 매핑할 수 있습니다. 

* [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) - 사용자 지정 도메인 이름을 Azure 앱의 가상 IP 주소로 직접 매핑합니다. 
* [CNAME](https://en.wikipedia.org/wiki/CNAME_record) - 사용자 지정 도메인 이름을 앱의 Azure 도메인 이름 **&lt;*appname*>.azurewebsites.net**으로 매핑합니다. 

CNAME의 장점은 IP 주소 변경 간에도 유지된다는 점입니다. 앱을 삭제하고 다시 만들거나 높은 가격 책정 계층에서 **공유** 계층으로 다시 변경하는 경우 앱의 가상 IP 주소가 변경될 수 있습니다. 이러한 변경 과정에서 CNAME 레코드는 여전히 유효하지만 A 레코드는 업데이트해야 합니다. 

이 자습서는 A 레코드 및 CNAME 레코드를 사용하는 단계를 보여 줍니다.

> [!IMPORTANT]
> 루트 도메인에 대한 CNAME 레코드를 만들지 마세요(즉, "루트 레코드"). 자세한 내용은 [루트 도메인에서 CNAME 레코드를 사용할 수 없는 이유](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain)를 참조하세요.
> 루트 도메인을 Azure 앱에 매핑하려면 대신 A 레코드를 사용하세요.
> 
> 

<a name="vip"></a>

## <a name="step-1-a-record-only-get-apps-ip-address"></a>1단계. *(A 레코드만)* 앱의 IP 주소 가져오기
A 레코드를 사용하여 사용자 지정 도메인 이름을 매핑하려면 Azure 앱의 IP 주소가 필요합니다. 그렇지 않고 CNAME 레코드를 사용하여 매핑하는 경우 이 단계를 건너뛰고 다음 섹션으로 이동합니다.

1. [Azure 포털](https://portal.azure.com)에 로그인합니다.
2. 왼쪽 메뉴에서 **앱 서비스** 를 클릭합니다.
3. 앱을 클릭한 다음 **사용자 지정 도메인**을 클릭합니다.
4. 호스트 이름 섹션 위에 있는 IP 주소를 메모합니다.
   
   ![A 레코드로 사용자 지정 도메인 이름 매핑: Azure 앱 서비스 앱의 IP 주소 가져오기](./media/web-sites-custom-domain-name/virtual-ip-address.png)
5. 이 포털 블레이드를 열어 둡니다. DNS 레코드를 만든 후에 여기로 다시 돌아옵니다.

<a name="createdns"></a>

## <a name="step-2-create-the-dns-records"></a>2단계. DNS 레코드 만들기
도메인 등록 기관에 로그인하고 해당 도구를 사용하여 A 레코드 또는 CNAME 레코드를 추가합니다. 각 등록 기관의 UI는 약간 다르므로 공급자의 설명서를 참조해야 합니다. 하지만 다음은 몇 가지 일반적인 지침입니다.

1. DNS 레코드를 관리하기 위한 페이지를 찾습니다. **도메인 이름**, **DNS** 또는 **이름 서버 관리** 레이블이 지정된 사이트의 링크 또는 영역을 찾습니다. 계정 정보를 확인한 다음 **내 도메인**과 같은 링크를 검색하여 링크를 찾을 수 있습니다.
2. DNS 레코드를 추가하거나 편집할 수 있는 링크를 찾습니다. 이 링크는 **영역 파일**, **DNS 레코드** 또는 **고급** 구성 링크일 수 있습니다.
3. 레코드를 만들고 변경 내용을 저장합니다.
   * [A 레코드에 대한 지침이 여기에 나와 있습니다](#a).
   * [CNAME 레코드에 대한 지침이 여기에 나와 있습니다](#cname).

<a name="a"></a>

### <a name="create-an-a-record"></a>A 레코드 만들기
A 레코드를 사용하여 Azure 앱의 IP 주소에 매핑하려면 실제로 A 레코드와 TXT 레코드를 둘 다 만들어야 합니다. A 레코드는 DNS 확인 자체를 위한 것이고 TXT 레코드는 Azure에서 사용자가 사용자 지정 도메인 이름을 소유했는지 확인하기 위한 것입니다. 

다음과 같이 A 레코드를 구성합니다((@는 일반적으로 루트 도메인을 나타냄).

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN 예</th>
    <th>호스트</th>
    <th>값</th>
  </tr>
  <tr>
    <td>contoso.com(루트)</td>
    <td>@</td>
    <td><a href="#vip">1단계</a>의 IP 주소</td>
  </tr>
  <tr>
    <td>www.contoso.com(하위)</td>
    <td>www</td>
    <td><a href="#vip">1단계</a>의 IP 주소</td>
  </tr>
  <tr>
    <td>\*.contoso.com(와일드카드)</td>
    <td>\*</td>
    <td><a href="#vip">1단계</a>의 IP 주소</td>
  </tr>
</table>

추가 TXT 레코드는 &lt;*subdomain*>.&lt;*rootdomain*>에서 &lt;*appname*>.azurewebsites.net으로 매핑하는 규칙을 사용합니다. 다음과 같이 TXT 레코드를 구성합니다.

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN 예</th>
    <th>TXT 호스트</th>
    <th>TXT 값</th>
  </tr>
  <tr>
    <td>contoso.com(루트)</td>
    <td>@</td>
    <td>&lt;<i>appname</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com(하위)</td>
    <td>www</td>
    <td>&lt;<i>appname</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>\*.contoso.com(와일드카드)</td>
    <td>\*</td>
    <td>&lt;<i>appname</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>

### <a name="create-a-cname-record"></a>CNAME 레코드 만들기
Azure 앱의 기본 도메인 이름에 매핑하는 데 CNAME 레코드를 사용하는 경우 A 레코드로 수행하는 것처럼 추가 TXT 레코드가 필요하지 않습니다. 

> [!IMPORTANT]
> 루트 도메인에 대한 CNAME 레코드를 만들지 마세요(즉, "루트 레코드"). 자세한 내용은 [루트 도메인에서 CNAME 레코드를 사용할 수 없는 이유](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain)를 참조하세요.
> 루트 도메인을 Azure 앱에 매핑하려면 대신 [A 레코드](#a) 를 사용하세요.
> 
> 

다음과 같이 CNAME 레코드를 구성합니다((@는 일반적으로 루트 도메인을 나타냄).

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN 예</th>
    <th>CNAME 호스트</th>
    <th>CNAME 값</th>
  </tr>
  <tr>
    <td>www.contoso.com(하위)</td>
    <td>www</td>
    <td>&lt;<i>appname</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>\*.contoso.com(와일드카드)</td>
    <td>\*</td>
    <td>&lt;<i>appname</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>

## <a name="step-3-enable-the-custom-domain-name-for-your-app"></a>3단계. 앱에 대해 사용자 지정 도메인 이름 사용
Azure 포털의 **사용자 지정 도메인** 블레이드로 돌아가( [1단계](#vip)참조) 사용자 지정 도메인의 정규화된 도메인 이름(FQDN)을 목록에 추가해야 합니다.

1. 로그인하지 않은 경우 [Azure 포털](https://portal.azure.com)에 로그인합니다.
2. Azure 포털의 왼쪽 메뉴에서 **앱 서비스** 를 클릭합니다.
3. 앱을 클릭한 다음 **사용자 지정 도메인** > **호스트 추가**의 API 앱에 사용자 지정 도메인 이름을 수동으로 매핑하는 방법을 보여 줍니다.
4. 사용자 지정 도메인의 FQDN을 목록에 추가합니다(예: **www.contoso.com**).
   
   ![Azure 앱에 사용자 지정 도메인 이름 매핑: 도메인 이름 목록에 추가](./media/web-sites-custom-domain-name/add-custom-domain.png)
   
   > [!NOTE]
   > Azure에서 여기에 사용된 도메인 이름을 확인합니다. [2단계](#createdns)에서 DNS 레코드를 만든 것과 동일한 도메인 이름인지 확인합니다. 
   > 
   > 
5. **유효성 검사**를 클릭합니다.
6. **유효성 검사** 를 클릭하면 Azure에서 도메인 검증 워크플로가 시작됩니다. 여기서는 호스트 이름 가용성 및 보고서 성공 여부 외에 도메인 소유권을 확인하거나 자세한 오류 사항과 그에 대한 해결 방법을 확인할 수 있습니다.    
7. 유효성 검사가 성공하면 **호스트 이름 추가** 단추가 활성화되며 호스트 이름을 할당할 수 있게 됩니다. 
8. Azure에서 새 사용자 지정 도메인 이름 구성을 완료한 경우 브라우저에서 사용자 지정 도메인 이름으로 이동합니다. 브라우저에 Azure 앱이 열리면 사용자 지정 도메인 이름이 올바르게 구성된 것입니다.

## <a name="migrate-an-active-domain-name"></a>활성 도메인 이름 마이그레이션

매핑하려는 도메인 이름을 이미 기존 웹 사이트에서 사용하고 가동 중지 시간을 방지하려는 경우 [App Service에 활성 사용자 지정 도메인 마이그레이션](app-service-custom-domain-name-migrate.md)을 참조하세요.

<a name="verify"></a>

## <a name="verify-dns-propagation"></a>DNS 적용 확인
구성 단계를 완료하고 나면 DNS 공급자에 따라 변경 내용이 적용되는 데 다소 시간이 걸릴 수 있습니다. [http://digwebinterface.com/](http://digwebinterface.com/)을 사용하여 예상대로 DNS 적용이 작동하는지 확인할 수 있습니다. 사이트를 찾은 후 텍스트 상자에 호스트 이름을 지정하고 **Dig**를 클릭합니다. 최근 변경 내용이 적용되었는지 확인하려면 결과를 확인합니다.  

![Azure 앱에 사용자 지정 도메인 이름 매핑: DNS 전파 확인](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [!NOTE]
> DNS 항목이 적용되는 데는 최대 48시간이 걸릴 수 있습니다(더 걸리는 경우도 있음). 모든 항목이 올바르게 구성된 경우 성공적으로 적용될 때까지 기다려야 합니다.
> 
> 

## <a name="next-steps"></a>다음 단계
[Azure에서 SSL 인증서를 구입](web-sites-purchase-ssl-web-site.md)하거나 [다른 곳에서 SSL 인증서를 사용](web-sites-configure-ssl-certificate.md)하여 HTTPS로 사용자 지정 도메인 이름의 보안을 유지하는 방법을 알아봅니다.

> [!NOTE]
> Azure 계정을 등록하기 전에 Azure App Service를 시작하려면 [App Service 체험](https://azure.microsoft.com/try/app-service/)으로 이동합니다. App Service에서 단기 스타터 웹앱을 즉시 만들 수 있습니다. 신용 카드는 필요하지 않으며 약정도 필요하지 않습니다.
> 
> 

[Azure DNS 시작](../dns/dns-getstarted-create-dnszone.md)  
[사용자 지정 도메인에서 웹앱에 대한 DNS 레코드 만들기](../dns/dns-web-sites-custom-domain.md)  
[Azure DNS로 도메인 위임](../dns/dns-domain-delegation.md)

<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png

