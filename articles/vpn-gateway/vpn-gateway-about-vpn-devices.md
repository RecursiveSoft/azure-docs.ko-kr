---
title: "크로스-프레미스 Azure 연결에 대한 VPN 장치 정보 | Microsoft Docs"
description: "이 문서에서는 S2S VPN 게이트웨이 크로스-프레미스 연결에 대한 VPN 장치 및 IPsec 매개 변수에 대해 설명합니다. 구성 지침과 샘플에 대한 링크를 제공합니다."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/03/2017
ms.author: yushwang;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: ab0a0ecba9d9e930cbc0ec5d4b83e252d2e50f7b
ms.lasthandoff: 03/16/2017


---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>사이트 간 VPN Gateway 연결에 대한 VPN 장치 및 IPsec/IKE 매개 변수 정보

VPN Gateway를 사용하여 S2S(사이트 간) 크로스-프레미스 VPN 연결을 구성하려면 VPN 장치가 필요합니다. 온-프레미스 네트워크와 가상 네트워크 간의 보안 연결을 만들려고 할 때마다 또는 하이브리드 솔루션을 만드는 데 사이트 간 연결을 사용할 수 있습니다. 이 문서에서는 Azure VPN Gateway에 대한 IPsec/IKE 매개 변수 목록 및 Azure VPN Gateway에 연결하는 유효성이 검사된 VPN 장치 목록을 제공합니다.


> [!IMPORTANT]
> 온-프레미스 VPN 장치와 Azure VPN 게이트웨이 간에 연결 문제가 발생하는 경우 [알려진 장치 호환성 문제](#known)를 참조하세요. 


###<a name="items-to-note-when-viewing-the-tables"></a>테이블 확인 시 주의 사항:

* Azure VPN 게이트웨이에 대한 용어가 변경되었습니다. 두 용어를 모두 사용할 수 있습니다. 기능은 변경되지 않고 이름만 변경됩니다.
  * 정적 라우팅 = 정책 기반
  * 동적 라우팅 = 경로 기반
* 고성능 VPN Gateway 및 경로 기반 VPN Gateway에 대한 사양은 별도로 언급하지 않는 한 동일합니다. 예를 들어 경로 기반 VPN Gateway와 호환되는 확인된 VPN 장치는 Azure 고성능 VPN Gateway와도 호환됩니다.

> [!NOTE]
> 사이트 간 연결을 구성할 때 VPN 장치에 공용 IPv4 IP 주소가 필요합니다.                                                                                                                                                                               


## <a name="devicetable"></a>확인된 VPN 장치
장치 공급업체와 협력하여 표준 VPN 장치의 유효성을 검사했습니다. 다음 목록에 포함된 장치 제품군의 모든 장치는 Azure VPN 게이트웨이에서 작동해야 합니다. 구성할 솔루션에 대해 만들어야 하는 게이트웨이 유형을 확인하려면 [VPN 게이트웨이 정보](vpn-gateway-about-vpngateways.md) 를 참조하세요.

VPN 장치를 구성하려면 적절한 장치 제품군에 해당하는 링크를 참조하세요. VPN 장치 지원은 장치 제조업체에 문의하세요.

|**공급업체**          |**장치 패밀리**     |**최소 OS 버전** |**정책 기반** |**경로 기반** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |호환되지 않음  |[구성 가이드](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |AR 시리즈 VPN 라우터 |2.9.2                  |곧 출시됩니다     |호환되지 않음  |
| Barracuda Networks, Inc. |Barracuda NextGen 방화벽 F 시리즈 |정책 기반: 5.4.3<br>경로 기반: 6.2.0 |[구성 가이드](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[구성 가이드](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen 방화벽 X 시리즈 |Barracuda Firewall 6.5 |[구성 가이드](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |호환되지 않음 |
| Brocade            |Vyatta 5400 vRouter   |Virtual Router 6.6R3 GA|[구성 가이드](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |호환되지 않음 |
| Check Point |Security Gateway |R77.30 |[구성 가이드](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[구성 가이드](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| 시스코              |ASA       |8.3 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |호환되지 않음 |
| 시스코 |ASR |정책 기반: IOS 15.1<br>경로 기반: IOS 15.2 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| 시스코 |ISR |정책 기반: IOS 15.0<br>경로 기반*: IOS 15.1 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[구성 샘플*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 이상 |[구성 가이드](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |호환되지 않음 |
| Dell SonicWALL |TZ 시리즈, NSA 시리즈<br>SuperMassive 시리즈<br>E-클래스 NSA 시리즈 |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |[SonicOS 6.2에 대한 구성 가이드](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[SonicOS 5.9에 대한 구성 가이드](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[SonicOS 6.2에 대한 구성 가이드](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[SonicOS 5.9에 대한 구성 가이드](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| F5 |BIG-IP 시리즈 |12.0 |[구성 가이드](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[구성 가이드](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.2 |[구성 가이드](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |[구성 가이드](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| IIJ(Internet Initiative Japan) |SEIL 시리즈 |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[구성 가이드](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |호환되지 않음 |
| Juniper |SRX |정책 기반: JunOS 10.2<br>경로 기반: JunOS 11.4 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |J 시리즈 |정책 기반: JunOS 10.4r9<br>경로 기반: JunOS 11.4 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[구성 샘플](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |라우팅 및 원격 액세스 서비스 |Windows Server 2012 |호환되지 않음 |[구성 샘플](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| 개방형 시스템 AG |핵심 업무 제어 보안 게이트웨이 |해당 없음 |[구성 가이드](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |[구성 가이드](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan |Openswan |2.6.32 |(출시 예정) |호환되지 않음 |
| Palo Alto Networks |PAN-OS를 실행하는 모든 장치 |PAN-OS<br>정책 기반: 6.1.5 이상<br>경로 기반: 7.1.4 |[구성 가이드](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[구성 가이드](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| WatchGuard |모두 |Fireware XTM<br> 정책 기반: v11.11.x<br>경로 기반: v11.12.x |[구성 가이드](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[구성 가이드](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) ISR 7200 시리즈 라우터는 정책 기반 VPN만을 지원합니다.

## <a name="additionaldevices"></a>확인되지 않은 VPN 장치
장치가 확인된 VPN 장치 테이블에 없더라도 사이트 간 연결을 사용할 수 있습니다. 추가 지원 및 구성 지침은 장치 제조업체에 문의하세요.

## <a name="editing"></a>장치 구성 샘플 편집
제공된 VPN 장치 구성 샘플을 다운로드한 후 환경에 대한 설정을 반영하기 위해 일부 값을 바꿔야 합니다.

###<a name="to-edit-a-sample"></a>샘플을 편집하려면

1. 메모장을 사용하여 샘플을 엽니다.
2. 모든 <*text*> 문자열을 검색하여 환경에 관련된 값으로 바꿉니다. < 및 >를 포함해야 합니다. 이름을 지정할 때 선택하는 이름은 고유해야 합니다. 명령이 작동하지 않는 경우 해당 장치 제조업체 설명서를 참조하세요.

| **샘플 텍스트** | **변경** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |이 개체에 대해 선택한 이름입니다. 예: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |이 개체에 대해 선택한 이름입니다. 예: myAzureNetwork |
| &lt;RP_AccessList&gt; |이 개체에 대해 선택한 이름입니다. 예: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |이 개체에 대해 선택한 이름입니다. 예: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |이 개체에 대해 선택한 이름입니다. 예: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |범위를 지정합니다. 예: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |서브넷 마스크를 지정합니다. 예: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |온-프레미스 범위를 지정합니다. 예: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |온-프레미스 서브넷 마스크를 지정합니다. 예: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |이 정보는 가상 네트워크와 관련이 있으며 **게이트웨이 IP 주소**인 관리 포털에 있습니다. |
| &lt;SP_PresharedKey&gt; |이 정보는 가상 네트워크와 관련이 있으며 키 관리인 관리 포털에 있습니다. |

## <a name="IPSec"></a>IPsec/IKE 매개 변수
> [!NOTE]
> 다음 테이블에 나열된 값이 Azure VPN Gateway에서 지원되더라도 현재 Azure VPN Gateway에서 특정 알고리즘 또는 매개 변수의 조합을 지정하거나 선택할 메커니즘이 없습니다. 온-프레미스 VPN 장치에서 제약 조건을 지정해야 합니다.
> 
> 또한 **MSS**를 **1350**에 고정해야 합니다.

아래 표에:

* SA = 보안 연결
* IKE 1단계는 "주 모드"라고도 합니다.
* IKE 2단계는 "빠른 모드"라고도 합니다.

### <a name="ike-phase-1-main-mode-parameters"></a>IKE 1단계(주 모드) 매개 변수
| **속성**          |**정책 기반**    | **경로 기반**    |
| ---                   | ---               | ---               |
| IKE 버전           |IKEv1              |IKEv2              |
| Diffie-Hellman 그룹  |그룹 2(1024비트) |그룹 2(1024비트) |
| 인증 방법 |미리 공유한 키     |미리 공유한 키     |
| 암호화 및 해싱 알고리즘 |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| SA 수명           |28,800초     |28,800초     |

### <a name="ike-phase-2-quick-mode-parameters"></a>IKE 2단계(빠른 모드) 매개 변수
| **속성**                  |**정책 기반**| **경로 기반**                              |
| ---                           | ---           | ---                                         |
| IKE 버전                   |IKEv1          |IKEv2                                        |
| 암호화 및 해싱 알고리즘 |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[RouteBased QM SA 제품](#RouteBasedOffers) |
| SA 수명(시간)            |3,600초  |27,000초                                |
| SA 수명(바이트)           |102,400,000 KB | -                                           |
| PFS(Perfect Forward Secrecy) |아니요             |[RouteBased QM SA 제품](#RouteBasedOffers) |
| 작동하지 않는 피어 검색(DPD)     |지원되지 않음  |지원됨                                    |


### <a name ="RouteBasedOffers"></a>RouteBased VPN IPsec 보안 연결(IKE 빠른 모드 SA) 제품
다음 표는 IPsec SA(IKE 빠른 모드) 제품을 나열합니다. 제안이 제시되거나 수락되는 기본 설정 순서대로 제안이 나열되어 있습니다.

#### <a name="azure-gateway-as-initiator"></a>Azure 게이트웨이(초기자)
|-  |**암호화**|**인증**|**PFS 그룹**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM(AES256)      |없음         |
| 2 |AES256        |SHA1              |없음         |
| 3 |3DES          |SHA1              |없음         |
| 4 |AES256        |SHA256            |없음         |
| 5 |AES128        |SHA1              |없음         |
| 6 |3DES          |SHA256            |없음         |

#### <a name="azure-gateway-as-responder"></a>Azure 게이트웨이(응답자)
|-  |**암호화**|**인증**|**PFS 그룹**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM(AES256)      |없음         |
| 2 |AES256        |SHA1              |없음         |
| 3 |3DES          |SHA1              |없음         |
| 4 |AES256        |SHA256            |없음         |
| 5 |AES128        |SHA1              |없음         |
| 6 |3DES          |SHA256            |없음         |
| 7 |DES           |SHA1              |없음         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |없음         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* 경로 기반 및 고성능 VPN Gateway를 사용하여 IPsec ESP NULL 암호화를 지정할 수 있습니다. Null 기반 암호화는 전송 중인 데이터를 보호하지 않으며, 최대 처리량 및 최소 대기 시간이 필요한 경우에만 사용됩니다.  클라이언트에서는 VNet 간 통신 시나리오 또는 솔루션의 다른 곳에서 암호화가 적용된 경우에 이 암호화를 사용할 수 있습니다.
* 인터넷을 통한 프레미스 간 연결의 경우 중요한 통신의 보안을 보장하려면 위의 테이블에 나열된 암호화 및 해시 알고리즘을 사용하는 기본 Azure VPN Gateway 설정을 사용하세요.

## <a name="known"></a>알려진 장치 호환성 문제

> [!IMPORTANT]
> 해당 내용은 타사 VPN 장치 및 Azure VPN 게이트웨이 간의 알려진 호환성 문제입니다. Azure 팀은 여기에 나열된 문제를 해결하기 위해 공급 업체와 함께 적극적으로 작업 중입니다. 문제가 해결되면 이 페이지는 가장 최신 정보로 업데이트됩니다. 주기적으로 다시 확인하세요.

###<a name="feb-16-2017"></a>2017년 2월 16일

Azure 경로 기반 VPN에 대한 **7.1.4 이전 버전으로 Palo Alto Networks 장치**: 7.1.4 이전의 PAN-OS 버전으로 Palo Alto Networks에서 VPN 장치를 사용하고 Azure 경로 기반 VPN 게이트웨이에 연결 문제가 발생하는 경우 다음 단계를 수행하세요.

1. Palo Alto Networks 장치의 펌웨어 버전을 확인합니다. PAN-OS 버전이 7.1.4보다 오래된 경우 7.1.4로 업그레이드하세요.
2. Palo Alto Networks 장치에서 Azure VPN Gateway로 연결하는 경우 단계 2 SA(또는 빠른 모드 SA) 수명을 28,800초(8시간)로 변경합니다.
3. 그래도 연결 문제가 발생하면 Azure Portal에서 지원 요청을 여세요.

