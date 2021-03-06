---
title: Azure Resource Graph 개요
description: Azure Resource Graph 서비스가 어떻게 대규모의 복잡한 리소스 쿼리를 지원하는지 알아봅니다.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/30/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: d76a5b32403bd14f18181580f891925130808922
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60002887"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Azure Resource Graph 서비스 개요

Azure Resource Graph는 모든 구독 및 관리 그룹에서 대규모로 쿼리를 수행할 수 있는 효율적인 고성능 리소스 탐색 기능을 제공하여 작업 환경을 효과적으로 관리할 수 있도록 Azure 리소스 관리를 확장할 수 있게 디자인된 Azure의 서비스입니다. 이러한 쿼리는 다음 기능을 제공합니다.

- 리소스 속성별로 복합 필터링, 그룹화 및 정렬을 사용하여 리소스를 쿼리하는 기능
- 거버넌스 요구 사항에 따라 리소스를 반복적으로 탐색하고 결과 식을 정책 정의로 변환하는 기능
- 광범위한 클라우드 환경에서 정책을 적용할 때 미치는 영향을 평가하는 기능
- [리소스 속성에 대한 세부 변경 내용](./how-to/get-resource-changes.md)(미리 보기)에 대한 기능.

이 설명서에서는 각 기능을 자세히 설명합니다.

> [!NOTE]
> Azure Resource Graph는 Azure Portal의 새로운 찾아보기 '모든 리소스' 탐색 환경과 Azure Policy의 [변경 내역](../policy/how-to/determine-non-compliance.md#change-history-preview)에서 사용됩니다.
> _시각적 개체 차이_. 이 기능은 고객이 대규모 환경을 관리할 수 있도록 디자인되었습니다.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Resource Graph가 Azure Resource Manager를 보완하는 방법

Azure Resource Manager는 현재 여러 리소스 필드, 특히 리소스 이름, ID, 유형, 리소스 그룹, 구독 및 위치를 제공하는 제한된 리소스 캐시로 데이터를 보냅니다. 이전에는 다양한 리소스 속성으로 작업하려는 경우 각 개별 리소스 공급자를 호출하고 각 리소스에 대한 요청 속성 세부 정보를 호출해야 했습니다.

Azure Resource Graph를 사용하면 각 리소스 공급자를 별도로 호출하지 않고도 리소스 공급자가 반환하는 이러한 속성에 액세스할 수 있습니다. 지원되는 리소스 유형의 목록을 보려면 [전체 모드 배포용 리소스](../../azure-resource-manager/complete-mode-deletion.md) 테이블에서 **예**를 찾습니다.

Azure Resource Graph를 통해 다음을 수행할 수 있습니다.

- 각 리소스 공급자를 별도로 호출하지 않고 리소스 공급자가 반환하는 속성에 액세스합니다.
- 리소스에 대한 지난 14일 동안의 변경 기록을 보고 변경된 속성과 시기를 확인합니다. (미리 보기)

## <a name="the-query-language"></a>쿼리 언어

지금까지 Azure Resource Graph에 대해 알아보았으므로 이제 쿼리 생성 방법을 자세히 살펴보겠습니다.

Azure Resource Graph의 쿼리 언어는 Azure Data Explorer의 [Kusto 쿼리 언어](../../data-explorer/data-explorer-overview.md)에 기반을 두고 있다는 것을 이해하는 것이 중요합니다.

먼저 Azure Resource Graph에서 사용할 수 있는 작업 및 함수에 대한 자세한 내용은 [Resource Graph 쿼리 언어](./concepts/query-language.md)를 참조하세요.
리소스를 찾아보려면 [리소스 탐색](./concepts/explore-resources.md)을 참조하세요.

## <a name="permissions-in-azure-resource-graph"></a>Azure Resource Graph의 권한

Resource Graph를 사용하려면 쿼리하려는 리소스에 대해 적어도 읽기 액세스 권한이 있는 RBAC([역할 기반 액세스 제어](../../role-based-access-control/overview.md))에 적절한 권한이 부여되어야 합니다. Azure 개체 또는 개체 그룹에 대한 `read` 이상의 권한이 없으면 결과가 반환되지 않습니다.

> [!NOTE]
> Resource Graph는 로그인하는 동안 보안 주체에 제공되는 구독을 사용합니다. 활성 세션 중에 추가된 새 구독의 리소스를 보려면 보안 주체가 컨텍스트를 새로 고쳐야 합니다. 이 작업은 로그아웃했다가 다시 로그인하면 자동으로 수행됩니다.

## <a name="throttling"></a>제한

Resource Graph에 대한 쿼리는 모든 고객에게 최상의 경험과 응답 시간을 제공하도록 제한됩니다. 자주 수행되는 대규모 쿼리에 Resource Graph API를 사용하려는 조직은 Resource Graph 페이지의 '피드백' 포털을 사용하세요. 비즈니스 사례를 제공하고, 연락을 받을 수 있도록 '피드백과 관련하여 Microsoft에서 이메일을 보내도 좋습니다' 확인란을 선택해야 합니다.

## <a name="running-your-first-query"></a>첫 번째 쿼리 실행

Resource Graph는 Azure CLI, Azure PowerShell 및 Azure SDK for .NET을 지원합니다. 쿼리는 각 언어에서 동일하게 구조화됩니다. [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) 및 [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module)에서 Resource Graph를 사용하도록 설정하는 방법에 대해 알아봅니다.

## <a name="next-steps"></a>다음 단계

- [Azure CLI](first-query-azurecli.md)로 첫 번째 쿼리를 실행합니다.
- [Azure PowerShell](first-query-powershell.md)로 첫 번째 쿼리를 실행합니다.
- [시작 쿼리](./samples/starter.md)로 시작합니다.
- [고급 쿼리](./samples/advanced.md)를 통해 이해를 향상시킵니다.