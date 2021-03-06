---
title: "Azure Active Directory B2C: 제한 사항 | Microsoft Docs"
description: "Azure Active Directory B2C의 제한 사항 목록"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: bryanla
ms.assetid: 04ec3310-98bb-4bb1-856d-ddc66913b390
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/17/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: 610b72f9f979f76d2d9c5180cb51ddae82a3e823
ms.lasthandoff: 03/23/2017


---
# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: 제한 사항

아직 지원되지 않는 Azure AD(Azure Active Directory) B2C의 여러 특징 및 기능이 있습니다. 이러한 대다수의 알려진 문제 및 제한 사항은 앞으로 해결될 것이지만 Azure AD B2C를 사용하여 소비자 지향 응용 프로그램을 빌드하는 경우 알고 있어야 합니다.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Azure AD B2C 테넌트를 만드는 동안 문제

[Azure AD B2C 테넌트를 만드는](active-directory-b2c-get-started.md) 동안 문제가 발생하는 경우 참고 자료로 [Azure AD 테넌트 또는 Azure AD B2C 테넌트 만들기--문제점 및 해결 방법](active-directory-b2c-support-create-directory.md)을 참조하세요.

기존 B2C 테넌트를 삭제하고 동일한 도메인 이름으로 다시 만들어야 하는 경우 알려진 문제가 발생합니다. 다른 도메인 이름으로 B2C 테넌트를 만들어야 합니다.

## <a name="branding-issues-on-verification-email"></a>확인 메일에 대한 브랜딩 문제

기본 확인 메일에 Microsoft 브랜딩이 포함되어 있습니다. 이는 나중에 제거될 예정입니다. 이제 [회사 브랜딩 기능](../active-directory/active-directory-add-company-branding.md)을 사용하여 제거할 수 있습니다.

## <a name="branding-issues-on-local-account-sign-in-page-in-a-sign-in-policy"></a>로그인 정책에서 로컬 계정 로그인 페이지의 브랜딩 문제

로그인 정책의 로컬 계정 로그인 페이지는 [여기](active-directory-b2c-reference-ui-customization.md)서 설명하는 페이지 UI 사용자 지정 기능이 아니라 [회사 브랜딩 기능](../active-directory/active-directory-add-company-branding.md)만으로 사용자 지정할 수 있습니다. 또한 사용자 이름 및 암호 필드에 레이블 또는 자리 표시자를 사용할 수 없습니다. 이 문제를 해결하려면 완전히 사용자 지정 가능한 "등록 또는 로그인 정책"을 대신 사용하는 것이 좋습니다. 로그인 정책에서 로컬 계정 로그인 페이지를 완전히 사용자 지정하려면 [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/13062033-b2c-fully-customizable-sign-in-page)에서 해당 기능에 투표합니다.

## <a name="restrictions-on-applications"></a>응용 프로그램에 대한 제한 사항

다음과 같은 응용 프로그램의 형식은 현재 Azure AD B2C에서 지원되지 않습니다. 응용 프로그램의 지원되는 종류에 대한 설명은 [Azure AD B2C: 응용 프로그램의 종류](active-directory-b2c-apps.md)를 참조하세요.

### <a name="daemons--server-side-applications"></a>디먼/서버 쪽 응용 프로그램

장기 실행 프로세스를 포함하거나 사용자 없이 작동하는 응용 프로그램은 Web API와 같은 보안 리소스에 액세스하는 방법도 필요합니다. 이러한 응용 프로그램은 [OAuth 2.0 클라이언트 자격 증명 흐름](active-directory-b2c-reference-protocols.md)에서 소비자의 위임된 ID 대신 응용 프로그램의 ID를 사용하여 인증하고 토큰을 가져올 수 있습니다. 이 흐름을 Azure AD B2C에서 아직 사용할 수 없으므로 이제는 응용 프로그램이 대화형 소비자 로그인 흐름이 발생한 후 토큰을 가져올 수 있습니다.


### <a name="web-api-chains-on-behalf-of"></a>Web API 체인(On-Behalf-Of)

많은 아키텍처에는 다른 다운스트림 Web API를 호출해야 하는 Web API가 포함되어 있으며 둘 다 Azure AD B2C로 보안됩니다. 이 시나리오는 Web API 백 엔드를 포함하는 네이티브 클라이언트에서 일반적이며, Web API 백 엔드가 다시 Azure AD Graph API와 같은 Microsoft 온라인 서비스를 호출합니다.

On-Behalf-Of 흐름이라고도 하는 OAuth 2.0 Jwt 전달자 자격 증명 권한 부여를 사용하여 이 연결된 Web API 시나리오를 지원할 수 있습니다. 그러나 On-Behalf-Of 흐름은 현재 Azure AD B2C에 구현되어 있지 않습니다.

## <a name="restrictions-on-reply-urls"></a>회신 URL에 대한 제한 사항

현재, Azure AD B2C에 등록된 앱은 제한된 회신 URL 값 집합으로 제한됩니다. 웹앱 및 서비스에 대한 회산 URL은 스키마 `https`로 시작해야 하고 모든 회신 URL 값은 단일 DNS 도메인을 공유해야 합니다. 예를 들어 다음 리디렉션 URL 중 하나가 있는 웹앱은 등록할 수 없습니다.

`https://login-east.contoso.com`
`https://login-west.contoso.com`

등록 시스템은 기존 회신 URL의 전체 DNS 이름을 편집하려는 회신 URL의 DNS 이름과 비교합니다. 다음 조건 중 하나라도 충족되는 경우 DNS 이름을 추가하는 요청이 실패하게 됩니다.

* 새 회신 URL의 전체 DNS 이름이 기존 회신 URL의 DNS 이름과 일치하지 않는 경우
* 새 회신 URL의 전체 DNS 이름이 기존 회신 URL의 하위 도메인이 아닌 경우

예를 들어 앱에 다음 회신 URL이 있는 경우

`https://login.contoso.com`

다음과 같이 추가할 수 있습니다.

`https://login.contoso.com/new`

이 경우 DNS 이름이 정확히 일치합니다. 또는 다음을 수행할 수 있습니다.

`https://new.login.contoso.com`

이 경우 login.contoso.com의 DNS 하위 도메인을 참조합니다. 회신 URL로 login-east.contoso.com 및 login-west.contoso.com을 사용하는 앱을 원하는 경우 다음 회신 URL을 순서대로 추가해야 합니다.

`https://contoso.com`
`https://login-east.contoso.com`
`https://login-west.contoso.com`

뒤의 두 개는 첫 번째 회신 URL의 하위 도메인이므로 추가할 수 있습니다. 이 제한은 향후 릴리스에서 제거될 예정입니다.

Azure AD B2C에 앱을 등록하는 방법을 알아보려면 [Azure Active Directory B2C에 응용 프로그램을 등록하는 방법](active-directory-b2c-app-registration.md)을 참조하세요.

## <a name="restriction-on-libraries-and-sdks"></a>라이브러리 및 SDK에 대한 제한 사항

Azure AD B2C를 작동하는 Microsoft 지원 라이브러리 집합은 현재 매우 제한적입니다. .NET 기반 웹앱 및 서비스뿐만 아니라 NodeJS 웹앱 및 서비스에 대한 지원을 합니다.  또한 Windows 및 다른.NET 앱에서 Azure AD B2C와 함께 사용할 수 있는 MSAL로 알려진 미리보기 .NET 클라이언트 라이브러리도 있습니다.

현재 iOS 및 Android를 포함한 다른 언어 또는 플랫폼을 지원하는 라이브러리가 없습니다.  위에 언급한 것들과는 다른 플랫폼에서 빌드하려는 경우 필요에 따라 [OAuth 2.0 및 OpenID Connect 프로토콜 참조](active-directory-b2c-reference-protocols.md) 를 참조하여 오픈 소스 SDK를 사용하는 것이 좋습니다.  Azure AD B2C는 통합에 대한 일반 OAuth 또는 OpenID Connect 라이브러리 사용이 가능하도록 하는 OAuth 및 OpenID Connect를 구현합니다,

iOS 및 Android 빠른 시작 자습서는 Azure AD B2C와의 호환성에 대해 테스트한 오픈 소스 라이브러리를 사용합니다.  모든 빠른 시작 자습서는 [시작](active-directory-b2c-overview.md#get-started) 섹션에서 확인할 수 있습니다.

## <a name="restriction-on-protocols"></a>프로토콜에 대한 제한 사항

Azure AD B2C는 OpenID Connect 및 OAuth 2.0을 지원합니다. 그러나 각 프로토콜의 일부 특징과 기능은 구현되지 않습니다. Azure AD B2C에서 지원되는 프로토콜 기능의 범위를 더 잘 이해하려면 [OpenID Connect 및 OAuth 2.0 프로토콜 참조](active-directory-b2c-reference-protocols.md)를 자세히 읽어보세요. SAML 및 WS-Fed 프로토콜은 지원되지 않습니다.

## <a name="restriction-on-tokens"></a>토큰에 대한 제한 사항

Azure AD B2C에서 발급된 토큰은 대부분 JSON Web Token, 즉 JWT로 구현됩니다. 그러나 JWT에 포함된 일부 정보("클레임"이라고 함)는 부족하거나 누락되었습니다. "preferred_username" 클레임을 예로 들 수 있습니다.  클레임의 값, 형식 또는 의미가 시간에 따라 달려져도 기존 정책에 대한 토큰은 영향을 받지않고 그대로 유지됩니다 - 프로덕션 앱의 값에 의존할 수 있습니다.  값이 변함에 따라 각 정책에 대해 그러한 변경을 구성할 기회를 줄 것입니다.  Azure AD B2C 서비스에서 현재 내보내는 토큰을 더 잘 이해하려면 [토큰 참조](active-directory-b2c-reference-tokens.md)를 자세히 읽어보세요.

## <a name="restriction-on-nested-groups"></a>중첩된 그룹에 대한 제한

중첩된 그룹 멤버 자격은 Azure AD B2C 테넌트에서 지원되지 않습니다. 이 기능을 추가할 계획이 없습니다.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Azure AD Graph API에서 차등 쿼리 기능에 대한 제한

[Azure AD Graph API에서 차등 쿼리 기능](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) 은 Azure AD B2C 테넌트에서 지원되지 않습니다. 이 기능은 우리의 장기 로드맵입니다.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Azure 클래식 포털에서 사용자 관리의 문제

B2C 기능은 Azure 포털에 액세스할 수 있습니다. 그러나 Azure 클래식 포털을 사용하여 사용자 관리를 포함하여 다른 테넌트 기능에 액세스할 수 있습니다. 현재 Azure 클래식 포털의 사용자 관리( **사용자** 탭)와 관련해서 알려진 몇 가지 문제가 있습니다.

* 로컬 계정 사용자(즉, 메일 주소 및 암호 또는 사용자 이름 및 암호로 등록한 소비자)의 경우 **사용자 이름** 필드가 등록할 때 사용한 로그인 식별자(메일 주소 또는 사용자 이름)와 일치하지 않습니다. 즉, Azure 클래식 포털에 표시되는 필드가 실제로 UPN(사용자 계정 이름)이기 때문에 B2C 시나리오에서 사용하지 않습니다. 로컬 계정의 로그인 식별자를 보려면 [Graph Explorer](https://graphexplorer.cloudapp.net/)에서 사용자 개체를 찾습니다. 소셜 계정 사용자와 동일한 문제를 찾을 수 있지만(즉, Facebook, Google + 등으로 등록된 소비자) 이 경우 이야기할 로그인 식별자가 없습니다.

    ![로컬 계정 - UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)
* 로컬 계정 사용자의 경우 **프로필** 탭에서 필드를 편집하고 변경 내용을 저장할 수 없습니다.

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Azure 클래식 포털의 관리자가 시작한 암호 재설정 관련 문제

Azure 클래식 포털에서 로컬 계정 기반 소비자에 대한 암호를 재설정하는 경우(**사용자** 탭의 **암호 재설정** 명령) 등록 또는 로그인 정책을 사용한다면 해당 소비자가 다음 로그인 시 자신의 암호를 변경할 수 없고 응용 프로그램에서 잠깁니다. 연습으로 [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) 를 사용하여 소비자의 암호를(암호 만료 없이) 재설정하거나 등록 또는 로그인 정책 대신 로그인 정책을 사용합니다.

## <a name="issues-with-creating-a-custom-attribute"></a>사용자 지정 특성 만들기 관련 문제

[Azure Portal에서 추가된 사용자 지정 특성](active-directory-b2c-reference-custom-attr.md) 은 B2C 테넌트에 즉시 만들어지지 않습니다. 사용자 지정 특성을 B2C 테넌트에 생성되게 하고 Graph API를 통해 사용할 수 있으려면 적어도 정책 중 하나에서 사용해야 합니다.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Azure 클래식 포털에서 도메인 확인 문제

현재는 [Azure 클래식 포털](https://manage.windowsazure.com/)에서 도메인을 성공적으로 확인할 수 없습니다.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Safari 브라우저에서 MFA 정책을 사용하는 로그인 문제

HTTP 400(잘못된 요청) 오류가 있는 Safari 브라우저에서 (MFA를 사용하는) 일시적인 로그인 정책 실패에 대한 요청입니다. Safari의 쿠키 크기가 낮게 제한되기 때문입니다. 이 문제에 대한 해결 방법은 다음과 같이 몇 가지가 있습니다.

* "로그인 정책" 대신 "등록 또는 로그인 정책"을 사용합니다.
* 정책에서 요청되는 **응용 프로그램 클레임** 수를 줄입니다.

## <a name="issues-with-windows-desktop-wpf-apps-using-azure-ad-b2c"></a>Azure AD B2C를 사용할 때의 Windows 데스크톱 WPF 앱 문제

Windows 데스크톱 WPF 앱에서 Azure AD B2C로의 요청이 때때로 다음 오류를 발생하며 실패합니다. "브라우저 기반 인증 대화 상자를 완료하지 못했습니다. 원인: 프로토콜을 알 수 없으며 일치하는 플러그 가능한 프로토콜이 입력되지 않았습니다.”

이 문제는 Azure AD B2C에서 제공하는 인증 코드의 크기 때문에 발생합니다. 이 크기는 토큰에 요청된 클레임 수와 상호 연관됩니다. 이 문제의 해결 방법은 토큰에 요청된 클레임 수를 줄이고 Graph API에서 다른 클레임을 별도로 쿼리하는 것입니다.

