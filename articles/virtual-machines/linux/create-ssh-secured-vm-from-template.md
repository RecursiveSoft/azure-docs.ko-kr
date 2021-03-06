---
title: "Azure 템플릿을 사용하여 Linux VM 만들기 | Microsoft Docs"
description: "Azure Resource Manager 템플릿을 사용하여 Azure에 Linux VM을 만듭니다."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: v-livech
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: c8b4c8cd7e6e6e07efbaae7ac024160e1e0a3568
ms.lasthandoff: 04/03/2017


---
# <a name="how-to-create-a-linux-vm-using-an-azure-resourec-manager-template"></a>Azure Resource Manager 템플릿을 사용하여 Linux VM을 만드는 방법
이 문서에서는 Azure 템플릿을 사용하여 Azure에서 Linux 가상 컴퓨터를 신속하게 배포하는 방법을 보여 줍니다.  이 문서의 내용을 실행하기 위해 필요한 사항:

* Azure 계정([무료 평가판 받기](https://azure.microsoft.com/pricing/free-trial/))
* `azure login`으로 로그인된 [Azure CLI](../../cli-install-nodejs.md)
* Azure Resource Manager 모드 `azure config mode arm`으로 *있어야 하는* Azure CLI

[Azure 포털](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)을 사용하여 Linux VM을 신속히 배포할 수도 있습니다.

## <a name="quick-command-summary"></a>빠른 명령 요약
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>자세한 연습
템플릿을 사용하면 사용자 이름 및 호스트 이름과 같은 시작 작업 중에 사용자 지정하려는 설정으로 Azure에서 VM을 만들 수 있습니다. 이 문서의 경우 SSH에 오픈된 포트 22를 포함한 NSG(네트워크 보안 그룹)와 함께 Ubuntu VM을 활용하여 Azure 템플릿을 시작합니다.

Azure Resource Manager 템플릿은 이 문서에서와 같이 Ubuntu VM을 시작하는 등 간단한 일회성 작업에 사용할 수 있는 JSON 파일입니다.  Azure 템플릿은 테스트, 개발 또는 프로덕션 배포 스택처럼 전체 환경의 복잡한 Azure 구성을 만드는 데도 사용될 수 있습니다.

## <a name="create-the-linux-vm"></a>Linux VM 만들기
다음 코드 예제에서는 `azure group create`를 호출하여 [이 Azure Resource Manager 템플릿](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)을 사용하여 리소스 그룹을 만드는 동시에 SSH 보안 Linux VM을 배포하는 방법을 보여 줍니다. 예제에서 환경에 고유한 이름을 사용해야 합니다. 이 예제에서는 `myResourceGroup`을 리소스 그룹 이름으로, `myVM`을 VM 이름으로 사용하고 있습니다.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

출력에는 다음과 같은 출력 블록이 있습니다.

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

해당 예에서는 `--template-uri` 매개 변수를 사용하여 VM을 배포했습니다.  인수인 템플릿 파일에 대한 경로가 있는 `--template-file` 매개 변수를 사용하여 템플릿을 다운로드하거나 로컬로 만들고 전달할 수 있습니다. Azure CLI는 템플릿에서 필요한 매개 변수에 대한 메시지를 표시합니다.

## <a name="next-steps"></a>다음 단계
[템플릿 갤러리](https://azure.microsoft.com/documentation/templates/) 를 검색하여 다음에 배포할 앱 프레임워크를 검색합니다.


