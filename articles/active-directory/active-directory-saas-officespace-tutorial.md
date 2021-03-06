---
title: "자습서: OfficeSpace Software와 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory 및 OfficeSpace Software 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: fb61a2d622b0d829a134b6ce5dfef6e9fb44fa1e
ms.lasthandoff: 03/22/2017


---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>자습서: OfficeSpace Software와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 OfficeSpace Software를 통합하는 방법에 대해 알아봅니다.

OfficeSpace Software를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- OfficeSpace Software에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 OfficeSpace Software에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure 관리 포털에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](active-directory-appssoaccess-whatis.md)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

OfficeSpace Software와의 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- OfficeSpace Software Single Sign-On이 설정된 구독


> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.


이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 않도록 합니다.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.


## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 OfficeSpace Software 추가
2. Azure AD Single Sign-on 구성 및 테스트


## <a name="adding-officespace-software-from-the-gallery"></a>갤러리에서 OfficeSpace Software 추가
OfficeSpace Software의 Azure AD 통합을 구성하려면 갤러리의 OfficeSpace Software를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 OfficeSpace Software를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure 관리 포털](https://portal.azure.com)**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![응용 프로그램][2]
    
3. 대화 상자 위쪽에 있는 **추가** 단추를 클릭합니다.

    ![응용 프로그램][3]

4. 검색 상자에 **OfficeSpace Software**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_001.png)

5. 결과 창에서 **OfficeSpace Software**를 선택한 다음 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 OfficeSpace Software에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 OfficeSpace Software 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 OfficeSpace Software의 관련 사용자 간에 연결이 형성되어야 합니다.

이 연결 관계는 Azure AD의 **사용자 이름** 값을 OfficeSpace Software의 **Username** 값으로 할당하여 설정합니다.

OfficeSpace Software에서 Azure AD Single Sign-on을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[OfficeSpace Software 테스트 사용자 만들기](#creating-an-officespace-software-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 OfficeSpace Software에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure 관리 포털에서 Azure AD Single Sign-On을 사용하도록 설정하고 OfficeSpace Software 응용 프로그램에서 Single Sign-On을 구성합니다.

**OfficeSpace Software에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure 관리 포털의 **OfficeSpace Software** 응용 프로그램 통합 페이지에서 **Single sign-on**을 클릭합니다.

    ![Single Sign-On 구성][4]

2. **Single sign on** 대화 상자에서 **모드**로 **SAML 기반 로그온**을 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_01.png)

3. **OfficeSpace Software 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_02.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴 `https://<company name>.officespacesoftware.com/users/sign_in/saml`을 사용하여 URL을 입력합니다.

    b. **식별자** 텍스트 상자에 `<company name>.officespacesoftware.com` 패턴으로 값을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자로 값을 업데이트해야 합니다. [OfficeSpace Software 지원 팀](mailto:support@officespacesoftware.com)에 문의하여 이러한 값을 받을 수 있습니다. 

4. OfficeSpace Software 응용 프로그램은 특정 형식의 SAML 어설션이 필요합니다. 이 응용 프로그램에 대한 다음 클레임을 구성하세요. 응용 프로그램 통합 페이지의 **"사용자 특성"** 섹션에서 이러한 특성의 값을 관리할 수 있습니다. 다음 스크린샷은 이에 대한 예제를 보여 줍니다.
    
    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_03.png)

5. **Single sign on** 대화 상자의 **사용자 특성** 섹션에서 **사용자 식별자**로 **user.mail**을 선택하고 아래 표에 표시되는 각 행에 대해 다음 단계를 수행합니다.
    
    | 특성 이름 | 특성 값 |
    | --- | --- |    
    | email | user.mail |
    | name | user.displayname |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. **특성 추가**를 클릭하여 **특성 추가** 대화 상자를 엽니다.

    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_04.png)

    ![Single Sign-on 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_05.png)
    
    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.
    
    c. **값** 목록에서 해당 행에 대해 표시된 특성을 입력합니다.
    
    d. **확인**을 클릭합니다.

6. **SAML 서명 인증서** 섹션에서 **인증서(base64)**를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_08.png) 

7. **Save**를 클릭합니다.

    ![Single Sign-on 구성](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. **OfficeSpace Software 구성** 섹션에서 **OfficeSpace Software 구성**을 클릭하여 **로그온 구성** 창을 엽니다.

    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_09.png) 

    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_10.png)

9. 다른 웹 브라우저 창에서 OfficeSpace Software 테넌트에 관리자로 로그인합니다.

10. **설정**으로 이동한 후 **커넥터**를 클릭합니다.

    ![앱 쪽에서 Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. **SAML 인정**을 클릭합니다.

    ![앱 쪽에서 Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. **SAML Authentication** 섹션에서 다음 단계를 수행합니다.

    ![앱 쪽에서 Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    a. **로그아웃 공급자 URL** 텍스트 상자에 Azure AD 응용 프로그램 구성 창의 **로그아웃 URL** 값을 입력합니다.

    b. **클라이언트 IdP 대상 URL** 텍스트 상자에 Azure AD 응용 프로그램 구성 창의 **SAML Single Sign-On 서비스 URL** 값을 입력합니다.

    c. 다운로드한 인증서에서 **지문** 값을 복사한 다음 **클라이언트 IdP 인증서 지문** 텍스트 상자에 붙여 넣습니다. 

    ㄹ. **설정 저장**을 클릭합니다.

    > [!NOTE]
    > 자세한 내용은 [인증서의 지문 값을 검색하는 방법](http://youtu.be/YKQF266SAxI) 
  

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure 관리 포털에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure 관리 포털**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png) 

2. **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭하여 사용자 목록을 표시합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png) 

3. 대화 상자 위쪽에서 **추가**를 클릭하여 **사용자** 대화 상자를 엽니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png) 

4. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    c. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다. 



### <a name="creating-an-officespace-software-test-user"></a>OfficeSpace Software 테스트 사용자 만들기

이 섹션의 목적은 OfficeSpace Software에서 Britta Simon이라는 사용자를 만드는 것입니다. OfficeSpace Software는 적시에 프로비전을 지원하며 기본적으로 사용하도록 설정합니다.

이 섹션에 작업 항목이 없습니다. 새 사용자가 아직 존재하지 않는 경우 OfficeSpace Software에 액세스하는 동안 만들어질 수 있습니다.

> [!NOTE]
> 사용자를 수동으로 만들어야 하는 경우 [OfficeSpace Software 지원 팀](mailto:support@officespacesoftware.com)에 문의해야 합니다.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 OfficeSpace Software에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 OfficeSpace Software에 할당하려면 다음 단계를 수행합니다.**

1. Azure 관리 포털에서 응용 프로그램 보기를 열고 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **OfficeSpace Software**를 선택합니다.

    ![Single Sign-On 구성](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_50.png) 

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    


### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 OfficeSpace Software 타일을 클릭하면 OfficeSpace Software 응용 프로그램에 자동으로 로그온됩니다.


## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](active-directory-saas-tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On이란 무엇입니까?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png
