---
title: "Windows VM에 대한 Azure Storage 솔루션 | Microsoft Docs"
description: "Azure 인프라 서비스에서 저장소 솔루션을 배포하기 위한 핵심 디자인 및 구현 지침에 대해 알아봅니다."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f692f98beaee16bef24bb7fbf716a9b4b8edeb6c
ms.lasthandoff: 03/18/2017


---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Windows VM에 대한 Azure Storage 인프라 지침

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

이 문서에서는 최적의 VM(가상 컴퓨터) 성능을 위한 저장소 요구 사항 및 디자인 고려 사항을 이해하는 데 주안점을 둡니다.

## <a name="implementation-guidelines-for-storage"></a>저장소에 대한 구현 지침
의사 결정:

* Azure Managed Disks를 사용할 것인가? 아니면 비관리 디스크를 사용할 것인가?
* 작업을 위해 표준 저장소를 사용해야 하나? 아니면 프리미엄 저장소를 사용해야 하나?
* 1023GB보다 큰 디스크를 만들기 위해 디스크 스트라이프가 필요한가?
* 작업에 대한 최적의 I/O 성능을 얻기 위해 디스크 스트라이프가 필요한가?
* IT 작업 또는 인프라를 호스트하는 데 필요한 저장소 계정 집합은 무엇인가?

작업:

* 배포하는 응용 프로그램의 I/O 요구를 검토하고 저장소 계정의 적절한 수와 유형을 계획합니다.
* 명명 규칙을 사용하여 저장소 계정 집합을 만듭니다. Azure PowerShell 또는 포털을 사용할 수 있습니다.

## <a name="storage"></a>저장소
Azure 저장소는 VM(가상 컴퓨터) 및 응용 프로그램의 배포 및 관리 작업의 핵심 부분입니다. Azure 저장소는 파일 데이터, 구조화되지 않은 데이터 및 메시지를 저장하기 위한 서비스를 제공하며, VM을 지원하는 인프라의 일부이기도 합니다.

[Azure Managed Disks](../storage/storage-managed-disks-overview.md)는 배후에서 저장소를 처리해 줍니다. 비관리 디스크를 사용하는 경우 Azure VM의 디스크(VHD 파일)를 갖기 위해 저장소 계정을 만들어야 합니다. 확장하는 경우에는 디스크를 포함하는 저장소의 IOPS 제한을 초과하지 않도록 저장소 계정을 추가로 만들었는지 확인해야 합니다. Managed Disks로 저장소를 처리하면 저장소 계정 한도(예: 20,000 IOPS/계정)로 인해 더 이상 제한을 받지 않습니다. 사용자 지정 이미지(VHD 파일)를 여러 저장소 계정에 복사할 필요도 없습니다. 중앙 위치(Azure 지역당 하나의 저장소 계정)에서 저장소 계정을 관리할 수 있고 이를 통해 구독에 수백 개의 VM을 만들 수 있습니다. 새 배포에 Managed Disks를 사용하는 것이 좋습니다.

VM 지원을 위해 다음 두 가지 저장소 계정 유형을 사용할 수 있습니다.

* Standard 저장소 계정은 Blob 저장소(Azure VM 디스크를 저장하는 데 사용), 테이블 저장소, 큐 저장소 및 파일 저장소에 대한 액세스 권한을 제공합니다.
* [Premium 저장소](../storage/storage-premium-storage.md) 계정은 AlwaysOn 클러스터의 SQL Server와 같이 I/O 집약적 워크로드에 대해 대기 시간이 낮은 고성능 디스크 지원을 제공합니다. Premium 저장소는 현재 Azure VM 디스크만 지원합니다.

Azure는 운영 체제 디스크, 임시 디스크를 및 0개 이상의 선택적 데이터 디스크로 VM을 만듭니다. 운영 체제 디스크 및 데이터 디스크는 Azure 페이지 Blob이지만 임시 디스크는 컴퓨터가 있는 노드에 로컬로 저장됩니다. VM은 유지 관리 이벤트 동안 호스트 간에 마이그레이션될 수 있으므로 응용 프로그램을 디자인할 때는 비영구 데이터에 대해 이러한 임시 디스크만 사용하도록 해야 합니다. 임시 디스크에 저장된 모든 데이터는 손실됩니다.

계획되지 않은 유지 관리 또는 하드웨어 오류로부터 데이터를 보호하기 위해 기본 Azure Storage 환경에서는 내구성 및 고가용성을 제공합니다. Azure Storage 환경을 디자인하는 경우에 VM 저장소를 복제 하도록 선택할 수 있습니다.

* 지정된 Azure 데이터 센터 내에서 로컬로
* 지정된 지역 내에서 Azure 데이터 센터에서
* 서로 다른 지역의 Azure 데이터 센터에서

[고가용성을 위한 복제 옵션에 대해 좀 더 자세히](../storage/storage-introduction.md#replication-for-durability-and-high-availability)읽어보세요.

운영 체제 디스크 및 데이터 디스크 크기는 최대 1023기가바이트(GB)입니다. Blob의 최대 크기는 1024GB이며 VHD 파일의 메타데이터(바닥글)를 포함해야 합니다(1GB는 1024<sup>3</sup>바이트임). Windows Server 2012의 저장 공간을 사용하면 데이터 디스크를 함께 풀링하여 1023GB보다 큰 논리 볼륨을 VM에 제공함으로써 이러한 제한을 극복할 수 있습니다.

Azure Storage 배포를 디자인할 때는 확장성이 어느 정도 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure 구독 및 서비스 제한, 할당량 및 제약 조건](../azure-subscription-service-limits.md#storage-limits)을 참조하세요. [Azure Storage 확장성 및 성능 목표](../storage/storage-scalability-targets.md)도 참조하세요.

응용 프로그램 저장소의 경우 문서, 이미지, 백업, 구성 데이터, 로그 등의 구조화되지 않은 개체 데이터를 저장할 수 있습니다. Blob Storage를 사용합니다. 응용 프로그램이 VM에 연결된 가상 디스크에 쓰는 대신, Azure Blob 저장소에 직접 쓸 수 있습니다. Blob Storage는 가용성 요구와 비용 제약에 따라 [핫 및 콜드 저장소 계층](../storage/storage-blob-storage-tiers.md) 옵션도 제공합니다.

## <a name="striped-disks"></a>스트라이프 디스크
데이터 디스크의 스트라이프는 대부분의 경우에 1023GB보다 큰 디스크를 만들 수 있을 뿐만 아니라 단일 볼륨에 대한 저장소를 지원하기 위해 여러 Blob을 허용하여 성능을 강화합니다. 스트라이프에서는 단일 논리적 디스크에서 데이터를 읽고 쓰는 데 필요한 I/O가 병렬로 진행됩니다.

Azure는 VM 크기에 따라 사용 가능한 데이터 디스크 수 및 대역폭의 양에 제한을 둡니다. 자세한 내용은 [가상 컴퓨터의 크기](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)를 참조하세요.

Azure 데이터 디스크에 디스크 스트라이프를 사용하고 있는 경우 다음 지침을 고려합니다.

* 데이터 디스크는 항상 최대 크기(1023GB)여야 합니다.
* VM 크기에 허용된 최대 데이터 디스크를 연결합니다.
* 저장소 공간을 사용합니다.
* Azure 데이터 디스크 캐싱 옵션을 사용하지 않습니다(캐싱 정책 = 없음).

자세한 내용은 [저장소 공간 - 성능을 높이기 위한 설계](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx)를 참조하세요.

## <a name="multiple-storage-accounts"></a>여러 저장소 계정
별도의 저장소 계정을 만들지 않으므로 이 섹션은 [Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에 적용되지 않습니다. 

비관리 디스크에 대한 Azure Storage 환경을 디자인할 때 배포하는 VM 수가 증가할 경우 여러 저장소 계정을 사용할 수 있습니다. 이 방법을 사용하면 기본 Azure Storage 인프라 전반에 I/O가 분산되므로 VM 및 응용 프로그램에 대해 최적의 성능을 유지할 수 있습니다. 배포하는 응용 프로그램을 디자인할 때는 각 VM의 I/O 요구 사항을 고려하고 Azure Storage 계정에 이러한 I/O를 분산해야 합니다. I/O 요구가 높은 모든 VM을 하나 또는 두 개의 저장소 계정으로만 그룹화하지 않도록 합니다.

다른 Azure Storage 옵션의 I/O 기능 및 일부 권장 최대값에 대한 자세한 내용은 [Azure 저장소 확장성 및 성능 목표](../storage/storage-scalability-targets.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


