---
title: Microsoft Authenticator 앱에 회사 또는 학교 계정 추가 - Azure Active Directory | Microsoft Docs
description: 2단계 인증을 위해 Microsoft Authenticator 앱에 회사 또는 학교 계정을 추가하는 방법을 설명합니다.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: 231f3c44344f4119bab6e1efb5546e6fded0c784
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473874"
---
# <a name="add-your-work-or-school-account"></a>회사 또는 학교 계정 추가
조직에서 2단계 인증을 사용하는 경우 인증 방법 중 하나로 Microsoft Authenticator 앱을 사용하도록 회사 또는 학교 계정을 설정할 수 있습니다.

>[!Important]
>계정을 추가하려면 Microsoft Authenticator 앱을 다운로드하여 설치해야 합니다. [앱 다운로드 및 설치](user-help-auth-app-download-install.md) 문서의 단계를 아직 진행하지 않은 경우 지금 진행하세요.

## <a name="add-your-work-or-school-account"></a>회사 또는 학교 계정 추가

1. 컴퓨터에서 [추가 보안 확인](https://aka.ms/mfasetup) 페이지로 이동합니다.

    >[!Note]
    >**추가 보안 인증** 페이지가 표시되지 않는 경우에는 관리자가 보안 정보(미리 보기) 환경을 켰을 수 있습니다. 이 경우 [인증자 앱을 사용하도록 보안 정보 설정](security-info-setup-auth-app.md) 섹션의 지침을 따라야 합니다. 이외의 경우에는 조직의 지원 센터에 도움을 요청해야 합니다. 보안 정보에 대 한 자세한 내용은 참조 하세요. [보안 정보 (미리 보기) 개요](user-help-security-info-overview.md)합니다.

2. **인증자 앱** 옆에 있는 확인란을 선택한 다음, **구성**을 선택합니다.

    **모바일 앱 구성** 페이지가 나타납니다.
    
    ![QR 코드를 제공하는 화면](./media/user-help-auth-app-download-install/auth-app-barcode.png)

3. Microsoft Authenticator 앱을 열고 오른쪽 위에 있는 **사용자 지정 및 제어** 아이콘에서 **계정 추가**를 선택한 다음, **회사 또는 학교 계정**을 선택합니다.

4. 디바이스 카메라를 사용해 컴퓨터의 **모바일 앱 구성** 화면에서 QR 코드를 스캔하고 **완료**를 선택합니다.

    >[!Note]
    >카메라가 QR 코드를 캡처할 수 없는 경우에는 2단계 인증을 위해 Microsoft Authenticator 앱에 계정 정보를 수동으로 추가할 수 있습니다. 자세한 내용 및 이를 수행하는 방법은 [수동으로 계정 추가](user-help-auth-app-add-account-manual.md)를 참조하세요.

5. 디바이스에서 앱의 **계정** 화면을 검토하여 계정이 올바르며 6자리 확인 코드가 연결되어 있는지 확인합니다. 보안 수준을 높이기 위해 확인 코드는 30초마다 변경되므로 코드 하나를 여러 번 사용할 수 없습니다.

    ![계정 화면](./media/user-help-auth-app-download-install/auth-app-accounts.png)

## <a name="next-steps"></a>다음 단계

- 앱에 계정을 추가한 후에는 디바이스에서 Authenticator 앱을 사용하여 로그인할 수 있습니다. 자세한 내용은 [앱을 사용하여 로그인](user-help-auth-app-sign-in.md)을 참조하세요.

- iOS를 실행하는 디바이스의 경우에는 계정 자격 증명 및 관련 앱 설정(예: 계정 순서)을 클라우드에 백업할 수도 있습니다. 자세한 내용은 [Microsoft Authenticator 앱을 통한 백업 및 복구](user-help-auth-app-backup-recovery.md)를 참조하세요.
