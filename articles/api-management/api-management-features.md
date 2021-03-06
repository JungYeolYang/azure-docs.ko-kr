---
title: Azure API Management 계층의 기능 기반 비교 | Microsoft Docs
description: 이 문서에서는 제공하는 기능을 기반으로 하여 API Management 계층을 비교합니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: eae36aa6e60e3da03c59952a1d9e035e6a773d2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531084"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Management 계층의 기능 기반 비교

각 API Management [가격 책정 계층](https://aka.ms/apimpricing)은 고유한 기능 세트 및 단위당 [용량](api-management-capacity.md)을 제공합니다. 다음 표에는 각 계층에서 사용할 수 있는 주요 기능이 요약되어 있습니다. 일부 기능은 다르게 작동하거나, 계층에 따라 다른 기능을 제공할 수 있습니다. 이러한 경우 해당 차이점은 개별 기능을 설명하는 설명서 문서에 제공됩니다.

| 기능                                                                                      | 소비<sup>미리 보기</sup> | Developer      | Basic          | 표준       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD 통합<sup>1</sup>                                                             | 아닙니다.                            | 사용자 계정 컨트롤            | no             | 예            | 예            |
| 가상 네트워크(VNet) 지원                                                               | 아닙니다.                            | 사용자 계정 컨트롤            | 아니오             | 아니요             | 예            |
| 다중 지역 배포                                                                      | 아닙니다.                            | 아니요             | 아니요             | 아니요             | 예            |
| 여러 사용자 지정 도메인 이름                                                                 | 아닙니다.                            | 아니요             | 아니요             | 아니요             | 예            |
| 개발자 포털<sup>2</sup>                                                                 | 아닙니다.                            | 예            | 예            | 예            | 예            |
| 기본 제공 캐시                                                                               | 아닙니다.                            | 예            | 예            | 예            | 예            |
| 기본 제공 분석                                                                           | 아닙니다.                            | 예            | 예            | 예            | 예            |
| [SSL 설정](api-management-howto-manage-protocols-ciphers.md)                             | 아닙니다.                            | 예            | 예            | 예            | 예            |
| [외부 캐시](https://aka.ms/apimbyoc)                                                    | 예                           | 아니요<sup>3</sup> | 아니요<sup>3</sup> | 아니요<sup>3</sup> | 아니요<sup>3</sup> |
| [클라이언트 인증서 인증](api-management-howto-mutual-certificates-for-clients.md) | 아니요<sup>4</sup>                | 예            | 예            | 예            | 예            |
| [Backup 및 복원](api-management-howto-disaster-recovery-backup-restore.md)               | 아닙니다.                            | 예            | 예            | 예            | 예            |
| [Git을 통한 관리](api-management-configuration-repository-git.md)                        | 아닙니다.                            | 예            | 예            | 예            | 예            |
| 직접 관리 API                                                                        | 아닙니다.                            | 예            | 예            | 예            | 예            |
| Azure Monitor 로그 및 메트릭                                                               | 아니요<sup>5</sup>                | 예            | 예            | 예            | 예            |

<sup>1</sup> 개발자 포털에서 사용자 로그인의 ID 공급자로 Azure AD(및 Azure AD B2C)를 사용할 수 있습니다.<br/>
<sup>2</sup> 관련 기능(예: 사용자, 그룹, 문제, 애플리케이션, 이메일 템플릿 및 알림)을 포함합니다.<br/>
<sup>3</sup> 이 계층에 대한 외부 캐시 지원이 곧 제공될 예정입니다.<br/>
<sup>4</sup> 클라이언트 인증서 인증이 일반 공급 이전에 소비 계층에 추가됩니다.<br/>
<sup>5</sup> 전체 Azure Monitor 지원이 소비 계층에 추가됩니다.
