DNS(Domain Name System)는 인터넷에서 리소스를 찾는 데 사용됩니다. 예를 들어 브라우저에서 웹앱 주소를 입력하거나 웹 페이지의 링크를 클릭하면 브라우저에서 DNS를 사용하여 도메인을 IP 주소로 변환합니다. IP 주소는 일종의 거리 주소와 비슷하지만 사람에게 그다지 친화적이지는 않습니다. 예를 들어 192.168.1.88 또는 2001:0:4137:1f67:24a2:3888:9cce:fea3과 같은 IP 주소보다는 **contoso.com** 과 같은 DNS 이름을 기억하기는 것이 훨씬 쉽습니다.

DNS 시스템은 *레코드*를 기반으로 합니다. 레코드는 *contoso.com*과 같은 구체적인 **이름**을 IP 주소 또는 DNS 이름과 연결합니다. 웹 브라우저와 같은 응용 프로그램에서 DNS의 이름을 조회할 경우 레코드를 찾아 해당 레코드가 가리키는 항목을 주소로 사용합니다. 가리키는 값이 IP 주소인 경우 브라우저는 해당 값을 사용합니다. 다른 DNS 이름을 가리키면 응용 프로그램은 다시 확인을 수행해야 합니다. 최종적으로 모든 이름 확인은 IP 주소 확인으로 완료됩니다.

앱 서비스에서 웹앱을 만들 때 DNS 이름이 웹앱에 자동으로 할당 됩니다. 이 이름은 **&lt;yourwebappname&gt;.azurewebsites.net**의 형식으로 사용됩니다. 또한 DNS 레코드를 만들 때 사용할 수 있는 가상 IP 주소도 있으므로 **.azurewebsites.net**을 가리키는 레코드를 만들거나 IP 주소를 가리킬 수 있습니다.

> [!NOTE]
> 웹앱을 삭제하고 다시 만들거나 App Service 계획 모드가 **기본**, **공유** 또는 **표준**으로 설정된 이후 모드를 **무료**로 변경하면 웹앱의 IP 주소가 변경됩니다.
> 
> 

레코드의 유형은 여러 가지이며 각 유형에는 고유한 함수 및 제한이 있지만 웹앱의 경우 *A* 및 *CNAME* 레코드, 두 가지만 주의하면 됩니다.

### <a name="address-record-a-record"></a>주소 레코드(A 레코드)
A 레코드는 **contoso.com**, **www.contoso.com** 등의 도메인이나 **\*.contoso.com** 등의 *와일드카드 도메인*을 IP 주소에 매핑합니다. 앱 서비스에서 웹앱의 경우 서비스의 가상 IP 또는 웹앱용으로 구입한 특정 IP 주소입니다.

A 레코드가 CNAME 레코드보다 나은 주요 장점은 다음과 같습니다.

* **contoso.com**과 같은 루트 도메인을 IP 주소로 매핑할 수 있습니다. 많은 등록 기관에서 A 레코드 사용만 허용합니다.
* **\*.contoso.com**과 같이 와일드카드를 사용하는 하나의 항목만 지정하고, 이 항목 하나로 **mail.contoso.com**, **blogs.contoso.com** 또는 **www.contso.com**과 같은 여러 하위 도메인에 대한 요청을 처리할 수 있습니다.

> [!NOTE]
> A 레코드는 고정 IP 주소에 매핑되므로 변경 내용을 웹앱의 IP 주소로 자동으로 확인할 수 없습니다. A 레코드에 사용할 IP 주소는 웹앱에 대한 사용자 지정 도메인 이름 설정을 구성할 때 제공됩니다. 그러나 웹앱을 삭제하고 다시 만들거나 App Service 계획 모드를 다시 **무료**로 변경할 경우 이 값이 변경될 수 있습니다.
> 
> 

### <a name="alias-record-cname-record"></a>별칭 레코드(CNAME 레코드)
CNAME 레코드는 **mail.contoso.com** 또는 **www.contoso.com**과 같은 *특정* DNS 이름을 다른(Canonical) 도메인 이름으로 매핑합니다. App Service Web Apps의 경우 Canonical 도메인 이름은 웹앱의 **&lt;yourwebappname>.azurewebsites.net** 도메인 이름입니다. CNAME을 만들면 **&lt;yourwebappname>.azurewebsites.net** 도메인 이름에 대한 별칭이 생성됩니다. CNAME 항목은 **&lt;yourwebappname>.azurewebsites.net** 도메인 이름의 IP 주소로 확인되므로 웹앱의 IP 주소가 변경되더라도 어떤 조치를 취할 필요는 없습니다.

> [!NOTE]
> **www.contoso.com**과 같은 CNAME 레코드를 사용하고 **contoso.com**과 같은 비루트 이름을 사용하면 일부 도메인 등록 기관에서 하위 도메인만 매핑할 수 있습니다. CNAME 레코드에 대한 자세한 내용은 등록 기관에서 제공하는 설명서인 <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME 레코드에 대한 Wikipedia 항목</a> 또는 <a href="http://tools.ietf.org/html/rfc1035">IETF 도메인 이름 - 구현 및 사양</a> 문서를 참조하세요.
> 
> 

### <a name="web-app-dns-specifics"></a>웹앱 DNS 설명
웹앱에서 A 레코드를 사용하려면 먼저 다음 TXT 레코드 중 하나를 만들어야 합니다.

* **루트 도메인용** - **&lt;yourwebappname&gt;.azurewebsites.net**에 **@**의 DNS TXT 레코드를 만듭니다.
* **특정 하위 도메인용** - **&lt;yourwebappname&gt;.azurewebsites.net**에 **&lt;sub-domain>**의 DNS 이름을 만듭니다. 예를 들어 A 레코드가 **blogs.contoso.com**에 대한 레코드인 경우 **blogs**입니다.
* **와일드카드 하위 도메인용** - **&lt;yourwebappname&gt;.azurewebsites.net**에 *****의 DNS TXT 레코드를 만듭니다.

이 TXT 레코드로 사용하려는 도메인을 소유하고 있는지를 확인할 수 있습니다. 또한 이 레코드는 웹앱의 가상 IP 주소를 가리키는 A 레코드를 만듭니다.

다음 단계를 수행하면 웹앱의 IP 주소 및 **.azurewebsites.net** 이름을 찾을 수 있습니다.

1. 브라우저에서 [Azure 포털](https://portal.azure.com)을 엽니다.
2. **Web Apps** 블레이드에서 웹앱의 이름을 클릭하고 페이지 맨 아래에서 **사용자 지정 도메인**을 선택합니다.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. **사용자 지정 도메인** 블레이드에 가상 IP 주소가 표시됩니다. 이 정보는 DNS 레코드를 만들 때 사용되므로 저장합니다.
   
    ![](./media/custom-dns-web-site/virtual-ip-address.png)
   
   > [!NOTE]
   > 사용자 지정 도메인 이름은 **무료** 웹앱과 함께 사용할 수 없으며, 꼭 App Service 계획을 **공유**, **기본**, **표준**, **프리미엄** 계층으로 업그레이드 해야 합니다. 사용자 웹앱의 가격 책정 계층 변경을 포함한 App Service 계획의 가격 책정 계층에 대한 자세한 정보는 [웹앱을 확장하는 방법](../articles/app-service-web/web-sites-scale.md)을 참조하세요.
   > 
   > 



<!--HONumber=Nov16_HO3-->


