---
title: "Windows VM에 대한 계획된 유지 관리 | Microsoft Docs"
description: "Azure 계획된 유지 관리의 정의와 계획된 유지 관리가 Azure를 실행하는 Windows 가상 컴퓨터에 주는 영향에 대해 알아봅니다."
services: virtual-machines-windows
documentationcenter: 
author: drewm
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: eb4b92d8-be0f-44f6-a6c3-f8f7efab09fe
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: drewm
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: fecc7666f80f7a9a5b2f8daaa65fd1766616fc85
ms.lasthandoff: 03/03/2017


---
# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Azure에서 가상 컴퓨터에 대한 계획된 유지 관리
Azure 계획된 유지 관리의 정의와 계획된 유지 관리가 Windows 가상 컴퓨터의 가용성에 주는 영향에 대해 알아봅니다. 이 문서는 [Linux 가상 컴퓨터](virtual-machines-linux-planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에도 적용됩니다.

이 문서는 Azure 계획 유지 보수 프로세스에 대한 배경을 제공합니다. VM이 다시 부팅되는 문제를 해결하려는 경우 [VM 재부팅 로그 보기에 대해 자세히 설명하는 이 블로그 게시물을 읽으십시오](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Azure에서 계획된 유지 관리를 수행하는 이유
Microsoft Azure는 가상 컴퓨터의 기반이 되는 호스트 인프라의 안정성, 성능 및 보안을 향상시키기 위해 전 세계적으로 주기적인 업데이트를 수행합니다. 이러한 많은 업데이트는 메모리 유지 업데이트를 비롯하여 가상 컴퓨터 또는 클라우드 서비스에 영향을 주지 않고 수행됩니다.

그렇지만 일부 업데이트는 인프라에 필요한 업데이트를 적용하기 위해 가상 컴퓨터를 다시 부팅해야 합니다. 가상 컴퓨터는 인프라를 패치하는 동안 종료되었다가 다시 시작됩니다.

가상 컴퓨터의 가용성에 영향을 미칠 수 있는 유지 관리 유형에는 계획된 유지 관리와 계획되지 않은 유지 관리가 있습니다. 이 페이지에서는 Microsoft Azure에서 계획된 유지 관리를 수행하는 방법을 설명합니다. 계획되지 않은 유지 관리에 대한 자세한 내용은 [계획된 유지 관리 및 계획되지 않은 유지 관리 이해](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)를 참조하세요.

[!INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]

