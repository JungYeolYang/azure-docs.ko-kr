---
title: Machine Learning Studio 작업 영역 관리
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio 작업 영역에 대한 액세스를 관리하고, Machine Learning API 웹 서비스를 배포 및 관리합니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: a947f9a94dd4ceed624e6b04a671b21b8926d25e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322790"
---
# <a name="manage-an-azure-machine-learning-studio-workspace"></a>Azure Machine Learning Studio 작업 영역 관리

> [!NOTE]
> Machine Learning 웹 서비스 포털의 웹 서비스 관리에 대한 자세한 내용은 [Azure Machine Learning 웹 서비스 포털에서 웹 서비스 관리](manage-new-webservice.md)를 참조하세요.
> 
> 

Azure Portal에서 Machine Learning Studio 작업 영역을 관리할 수 있습니다.



## <a name="use-the-azure-portal"></a>Azure Portal 사용

Azure Portal에서 Studio 작업 영역을 관리하려면:

1. Azure 구독 관리자 계정을 사용하여 [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. 페이지 맨 위에 있는 검색 상자에 "machine learning Studio 작업 영역"을 입력한 다음, **Machine Learning Studio 작업 영역**을 선택합니다.
3. 관리하려는 작업 영역을 클릭합니다.

표준 리소스 관리 정보 및 사용할 수 있는 옵션 외에 다음을 수행할 수 있습니다.

- **속성** 보기 - 이 페이지에는 작업 영역 및 리소스 정보가 표시됩니다. 이 작업 영역이 연결된 구독 및 리소스 그룹을 변경할 수 있습니다.
- **스토리지 키 다시 동기화** - 이 작업 영역은 Storage 계정에 대한 키를 유지 관리합니다. 저장소 계정이 키를 변경하면 **키 다시 동기화**를 클릭하여 작업 영역과 키를 동기화할 수 있습니다.

이 Studio 작업 영역과 연결된 웹 서비스를 관리하려면 Machine Learning 웹 서비스 포털을 사용하세요. 자세한 내용은 [Azure Machine Learning 웹 서비스 포털에서 웹 서비스 관리](manage-new-webservice.md)를 참조하세요.

> [!NOTE]
> 새로운 웹 서비스를 배포 또는 관리하려면 웹 서비스가 배포된 구독에 대한 참여자 또는 관리자 역할을 할당받아야 합니다. Machine Learning Studio 작업 영역에 다른 사용자를 초대하는 경우 구독에 대한 기여자 또는 관리자 역할을 할당해야 해당 사용자가 웹 서비스를 배포하거나 관리할 수 있습니다. 
> 
>액세스 권한 설정에 대한 자세한 내용은 [RBAC 및 Azure Portal을 사용하여 액세스 관리](../../role-based-access-control/role-assignments-portal.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계
* [Azure Resource Manager 템플릿을 사용하여 Machine Learning을 배포](deploy-with-resource-manager-template.md)하는 방법에 대해 자세히 알아봅니다. 
