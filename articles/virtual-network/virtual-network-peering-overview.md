---
title: "Azure Virtual Network 피어링 | Microsoft Docs"
description: "Azure의 가상 네트워크 피어링에 대해 알아봅니다."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: narayan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6fbcdcf77f46a3c643e8fedc1d112588cbd7befc
ms.lasthandoff: 04/03/2017


---
# <a name="virtual-network-peering"></a>가상 네트워크 피어링
VNet(가상 네트워크) 피어링은 Azure 백본 네트워크를 통해 동일한 지역에 있는 두 개의 VNet을 연결할 수 있습니다. 피어링되면 두 개의 VNet을 연결하기 위해 하나로 표시합니다. 두 개의 VNet을 별도 리소스로 관리할 수는 있지만 피어링된 VNet의 VM(가상 컴퓨터)은 개인 IP 주소를 사용하여 직접 서로 통신할 수 있습니다.

동일한 VNet에 있는 VM 간에 트래픽이 라우팅되는 것과 마찬가지로 피어링된 VNet에 있는 VM 간의 트래픽은 Azure 인프라를 통해 라우팅됩니다. VNet 피어링을 사용하는 이점은 다음과 같습니다.

* 다른 VNet에 있는 리소스 간에 짧은 대기 시간, 높은 대역폭 연결
* 네트워크 가상 어플라이언스, VPN 게이트웨이와 같은 리소스를 피어링된 VNet에서 전송 지점으로 사용 가능
* Azure Resource Manager 배포 모델을 통해 만든 두 개의 VNet을 피어링 가능 또는 클래식 배포 모델을 통해 만든 VNet에 Resource Manager를 통해 만든 하나의 VNet을 피어링 가능 두 가지 Azure 배포 모델의 차이점에 대해 자세히 알아보려면 [Azure 배포 모델 이해](../azure-resource-manager/resource-manager-deployment-model.md) 문서를 참조하세요.

VNet 피어링의 요구 사항 및 주요 측면은 다음과 같습니다.

* 피어링된 VNet은 동일한 Azure 지역에 있어야 합니다.
* 피어링된 VNet에는 겹치지 않는 IP 주소 공간이 있어야 합니다.
* VNet 피어링은 두 개의 VNet 간에 가능하며 피어링을 통해 파생된 전이적 관계가 없습니다. 예를 들어, VNetA가 VNetB와 피어링되고 VNetB가 VNetC와 피어링되는 경우 VNetA는 VNetC와 피어링되지 *않습니다*.
* 양쪽 구독의 권한 있는 사용자가 피어링을 허용하고 구독이 동일한 Active Directory 테넌트에 연결되면 다른 두 개의 구독에 있는 VNet을 피어링할 수 있습니다.
* VNet은 둘 다 Resource Manager 배포 모델을 통해 생성된 경우 또는 하나가 Resource Manager 배포 모델을 통해 생성되고 다른 하나가 클래식 배포 모델을 통해 만들어진 경우 피어링될 수 있습니다. 그러나 둘 다 클래식 배포 모델을 통해 만든 경우 서로 피어링될 수 없습니다. 서로 다른 배포 모델을 통해 만든 VNet을 피어링하는 경우 VNet은 모두 *동일한* 구독에 있어야 합니다. *다른* 구독에 있는 다른 배포 모델을 통해 만든 VNet을 피어링하는 기능은 **미리 보기** 릴리스 상태입니다. 자세한 내용은 [PowerShell을 사용하여 가상 네트워크 피어링 만들기](virtual-networks-create-vnetpeering-arm-ps.md) 문서를 참조하세요.
* 피어링된 VNet에 있는 VM 간의 통신에 추가 대역폭이 제한되지 않지만 적용되는 VM 크기에 따라 최대 네트워크 대역폭이 제한됩니다. 다양한 VM 크기의 최대 네트워크 대역폭에 대한 자세한 내용을 알아보려면 [Windows](../virtual-machines/windows/sizes.md) 또는 [Linux](../virtual-machines/linux/sizes.md) VM 크기 문서를 참조하세요.

![기본 VNet 피어링](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>연결
두 개의 VNet이 피어링된 후에 VNet의 VM 또는 Cloud Services는 피어링된 VNet에 연결된 다른 리소스와 직접 연결할 수 있습니다. 두 개의 VNet은 전체 IP 수준에서 연결됩니다.

피어링된 VNet에 있는 두 개의 VM 간의 왕복 네트워크 대기 시간은 단일 VNet 내의 왕복의 경우와 같습니다. 네트워크 처리량은 해당 크기에 비례하는 VM에 허용되는 대역폭을 기반으로 합니다. 피어링 내에서 대역폭에 대한 추가 제한이 없습니다.

피어링된 VNet에 있는 VM 간의 트래픽은 게이트웨이가 아닌 Azure의 백 엔드 인프라를 통해 직접 라우팅됩니다.

VNet에 연결된 VM은 피어링된 VNet에서 ILB(내부 부하 분산) 끝점에 액세스할 수 있습니다. NSG(네트워크 보안 그룹)는 다른 VNet에 대한 액세스를 차단하는 VNet 또는 원하는 경우 서브넷에 적용될 수 있습니다.

피어링을 구성할 경우 VNet 간에 NSG 규칙을 열거나 닫을 수 있습니다. 피어링된 VNet 간에 전체 연결을 여는 경우(기본 옵션) 특정 서브넷에 NSG를 적용하여 특정 액세스를 거부하거나 차단할 수 있습니다. NSG에 대해 자세히 알아보려면 [네트워크 보안 그룹](virtual-networks-nsg.md)을 참조하세요.

Azure에서 제공한 VM에 대한 내부 DNS 이름 확인은 피어링된 VNet에 적용되지 않습니다. VM에는 로컬 VNet 내에서만 확인할 수 있는 내부 DNS 이름이 있습니다. 그러나 피어링된 VNet에 연결된 VM을 VNet에 대한 DNS 서버로 구성합니다. 자세한 내용은 [자체 DNS 서버를 이용한 이름 확인](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) 문서를 참조하세요.

## <a name="service-chaining"></a>서비스 체이닝
이 문서의 뒷부분에 있는 다이어그램에서 보여준 대로 피어링된 VNet의 VM을 가리키는 UDR(사용자 정의 경로)을 "다음 홉" IP 주소로 구성할 수 있습니다. 이렇게 하면 서비스 체이닝을 설정하여 UDR을 통해 하나의 VNet에서부터 피어링된 VNet에서 실행되는 가상 어플라이언스로 트래픽을 보낼 수 있습니다.

허브 및 스포크 유형의 환경을 효율적으로 만들 수도 있습니다. 그러면 허브가 네트워크 가상 어플라이언스와 같은 인프라 구성 요소를 호스팅할 수 있습니다. 모든 스포크 VNet은 허브 VNet에서 실행되는 어플라이언스뿐만 아니라 그에 대한 트래픽의 하위 집합과 피어링할 수 있습니다. 즉, VNet 피어링을 사용하면 UDR에 대한 다음 홉 IP 주소는 피어링된 VNet에 있는 VM의 IP 주소가 될 수 있습니다. UDR에 대한 자세한 내용은 [사용자 정의 경로](virtual-networks-udr-overview.md) 문서를 참조하세요.

## <a name="gateways-and-on-premises-connectivity"></a>게이트웨이 및 온-프레미스 연결
각 VNet은 다른 VNet과 피어링되었는지 여부와 상관없이 고유한 게이트웨이를 가지고 있으며 이를 온-프레미스 네트워크에 연결하는 데 사용할 수 있습니다. 사용자는 VNet이 피어링된 경우에도 게이트웨이를 사용하여 [VNet 간 연결](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)을 구성할 수도 있습니다.

VNet 간 연결을 위한 옵션을 모두 구성한 경우 VNet 간의 트래픽은 피어링 구성(즉, Azure 백본)을 통해 이동합니다.

VNet이 피어링되면 사용자는 피어링된 VNet에 있는 게이트웨이를 온-프레미스 네트워크에 대한 전송 지점으로 구성할 수도 있습니다. 이 경우 원격 게이트웨이를 사용하는 VNet은 고유한 게이트웨이를 사용할 수 없습니다. VNet은 하나의 게이트웨이만을 사용할 수 있습니다. 게이트웨이는 다음 그림에서 보여준 대로 피어링된 VNet의 로컬 게이트웨이 또는 원격 게이트웨이일 수 있습니다.

![VNet 피어링 전송](./media/virtual-networks-peering-overview/figure02.png)

게이트웨이 전송은 다른 배포 모델을 통해 만든 VNet 간의 피어링 관계에서 지원되지 않습니다. 게이트웨이 전송을 작동시키기 위해 피어링 관계인 두 VNet을 Resource Manager를 통해 만들어야 합니다.

단일 Azure ExpressRoute 연결을 공유하는 VNet이 피어링된 경우 둘 사이의 트래픽은 피어링 관계(즉, Azure 백본 네트워크)를 거치게 됩니다. 각각의 VNet에서 로컬 게이트웨이를 사용하여 온-프레미스 회로에 연결할 수 있습니다. 또는 공유 게이트웨이를 사용하고 온-프레미스 연결에 대한 전송을 구성할 수 있습니다.

## <a name="provisioning"></a>프로비전
VNet 피어링은 권한이 필요한 작업입니다. VirtualNetworks 네임스페이스에서 별도 기능입니다. 사용자는 특정 권한을 지정하여 피어링하는 권한을 부여할 수 있습니다. 가상 네트워크에 대한 읽기-쓰기 액세스 권한이 있는 사용자는 이러한 권한을 자동으로 상속합니다.

피어링 기능의 관리자 또는 권한있는 사용자 중 하나인 사용자는 다른 VNet에 대한 피어링 작업을 시작할 수 있습니다. 다른 쪽에 피어링에 대해 동일한 요청이 있고 다른 요구 사항을 충족하는 경우 피어링이 설정됩니다.

두 VNet 간의 VNet 피어링을 설정하는 방법에 대해 자세히 알아보려면 이 문서 중 [다음 단계](#next-steps) 섹션에 있는 문서를 참조하세요.

## <a name="limits"></a>제한
단일 가상 네트워크에 허용되는 피어링의 수에 대한 제한이 있습니다. 자세한 내용은 [Azure 네트워킹 제한](../azure-subscription-service-limits.md#networking-limits) 을 참조하세요.

## <a name="pricing"></a>가격
피어링 VNet을 활용하는 수신 및 송신 트래픽에 대한 명목 요금이 부과됩니다. 자세한 내용은 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/virtual-network)를 참조합니다.

## <a name="next-steps"></a>다음 단계
다음을 사용하여 VNet 피어링을 만드는 방법에 대해 알아봅니다.

* [Azure 포털](virtual-networks-create-vnetpeering-arm-portal.md)
* [Azure PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
* [Azure Resource Manager 템플릿](virtual-networks-create-vnetpeering-arm-template-click.md)

