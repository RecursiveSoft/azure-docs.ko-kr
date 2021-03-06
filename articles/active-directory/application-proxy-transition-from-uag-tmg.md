---
title: "Microsoft Forefront에서 Azure AD 응용 프로그램 프록시로 전환 | Microsoft Docs"
description: "Microsoft Forefront TMG 및 UAG 솔루션에서 Azure Active Directory 응용 프로그램 프록시로 이동하는 방법에 대한 기본 사항을 설명합니다."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: c887adaae811972efd7fcea86eaa67a899f3f4c2
ms.openlocfilehash: 542dd7df7e0b887298522f29cb597f1df73709cb
ms.lasthandoff: 02/27/2017


---
# <a name="transition-to-azure-ad-application-proxies-from-microsoft-forefront"></a>Microsoft Forefront에서 Azure AD 응용 프로그램 프록시로 전환

이 문서에서는 Microsoft Forefront TMG(Threat Management Gateway) 및 UAG(United Access Gateway) 솔루션에서 이러한 Azure AD(Azure Active Directory) 응용 프로그램 프록시인 웹 응용 프로그램 프록시 및 Azure AD 응용 프로그램 프록시로 이동하는 방법을 설명합니다.

> [!NOTE]
> Azure AD Application Proxy는 Premium 또는 Basic 버전의 Azure AD로 업그레이드하는 경우에만 사용할 수 있는 기능입니다. 자세한 내용은 [Azure Active Directory 버전](active-directory-editions.md)을 참조하세요.


Forefront TMG 및 UAG에서 새 응용 프로그램 프록시로 전환하는 방법에 대한 자세한 정보는 [Microsoft에서 관련된 백서를 다운로드](http://download.microsoft.com/download/3/E/3/3E335D93-6DB8-4834-90A8-B86105419F05/Microsoft%20TMG%20and%20UAG%20EOL%20and%20transitioning%20to%20WAP%20and%20AADAP.docx)할 수 있습니다.

## <a name="functionality-details-for-the-conversion"></a>변환에 대한 기능 정보

|**TMG/UAG 기능**|**웹 응용 프로그램 프록시/Azure AD 응용 프로그램 프록시**|
|:-----|:-----|
|브라우저 앱에 대한 선택적 HTTP 게시|Windows Server 2012 R2의 웹 응용 프로그램 프록시에서 사용할 수 있습니다. 현재 Azure AD 응용 프로그램 프록시에서 사용할 수 있습니다.|
|AD FS(Active Directory Federation Services) 통합|Windows Server 2012 R2의 웹 응용 프로그램 프록시에서 사용할 수 있습니다. 현재 Azure AD 응용 프로그램 프록시에서 사용할 수 있습니다.|
|풍부한 프로토콜 게시(예: Citrix, Lync, RDG)|Windows Server 2012 R2의 웹 응용 프로그램 프록시에서 사용할 수 있습니다. 현재 Azure AD 응용 프로그램 프록시에서 사용할 수 있습니다.|
|ActiveSync(HTTP 기본) 및 RDG에 대한 사전 인증|웹 응용 프로그램 프록시 또는 Azure AD 응용 프로그램 프록시에서 현재 사용할 수 없습니다.|
|포털|웹 응용 프로그램 프록시에 Intune 또는 System Center를 사용합니다. Azure AD 응용 프로그램 프록시에 Azure AD 액세스 패널 또는 Office 365 앱 시작 관리자를 사용합니다.|
|끝점 상태 검색|Intune 또는 System Center를 사용합니다.|
|SSL 터널링|Windows SSL 또는 VPN 기능을 사용합니다.|
|계층 2/3 방화벽|Windows Server 기능을 사용합니다.|
|웹 응용 프로그램 방화벽|현재 Microsoft에서 제공하는 솔루션이 없습니다.|
|보안 웹 게이트웨이(정방향 프록시)|현재 Microsoft에서 제공하는 솔루션이 없습니다.|


## <a name="next-steps"></a>다음 단계

[블로그: 웹 응용 프로그램 프록시](https://blogs.technet.microsoft.com/applicationproxyblog/tag/web-application-proxy)<br>
[블로그: Azure AD 응용 프로그램 프록시](https://blogs.technet.microsoft.com/applicationproxyblog/tag/aad-ap)

