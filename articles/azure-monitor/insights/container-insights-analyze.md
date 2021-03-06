---
title: 컨테이너용 Azure Monitor를 사용하여 AKS 클러스터 성능 모니터링 | Microsoft Docs
description: 이 문서에서는 컨테이너용 Azure Monitor를 사용하여 성능 및 로그 데이터를 살펴보고 분석하는 방법을 설명합니다.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 8fb1d0083796671119de2b4d7feefe738b602fe2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60497374"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>컨테이너용 Azure Monitor를 사용하여 AKS 클러스터 성능 이해 
컨테이너용 Azure Monitor를 사용하면 성능 차트 및 상태 정보를 통해 AKS(Azure Kubernetes Service) 클러스터의 워크로드를 모니터링할 수 있습니다. 즉, AKS 클러스터에서 직접 볼 수도 있고 Azure Monitor에서 구독의 모든 AKS 클러스터를 볼 수도 있습니다. 특정 AKS 클러스터를 모니터링할 때에도 ACI(Azure Container Instances)를 볼 수 있습니다.

이 문서는 이러한 두 관점을 이해하고 검색된 문제를 신속하게 평가, 조사 및 해결하는 데 도움이 됩니다.

컨테이너용 Azure Monitor를 사용하도록 설정하는 방법은 [컨테이너용 Azure Monitor 등록](container-insights-onboard.md)을 참조하세요.

Azure Monitor는 구독의 리소스 그룹에 배포된 모든 모니터링되는 AKS 클러스터를 보여주는 다중 클러스터 보기를 제공합니다.  솔루션에서 모니터링되지 않는 것으로 식별된 AKS 클러스터도 보여줍니다. 즉시 클러스터 상태를 이해할 수 있으며, 여기서 노드 및 컨트롤러 성능 페이지로 드릴다운하거나 클러스터의 성능 차트로 이동할 수 있습니다.  모니터링되지 않는 것으로 식별된 AKS 클러스터의 경우 언제든지 해당 클러스터에 모니터링을 사용할 수 있습니다.  

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인
[Azure Portal](https://portal.azure.com)에 로그인합니다. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure Monitor에서 다중 클러스터 보기 
배포된 모든 AKS 클러스터의 상태를 보려면 Azure Portal의 왼쪽 창에서 **모니터**를 선택합니다.  **인사이트** 섹션에서 **컨테이너**를 선택합니다.  

![Azure Monitor 다중 클러스터 대시보드 예제](./media/container-insights-analyze/azmon-containers-multiview-1018.png)

**모니터링되는 클러스터** 탭에서 다음 정보를 살펴볼 수 있습니다.

1. 중요 또는 비정상 상태인 클러스터 수와 정상 또는 보고하지 않음(알 수 없는 상태라고도 함) 상태인 클러스터 수.
2. 내 [AKS 엔진(Azure Kubernetes 엔진)](https://github.com/Azure/aks-engine)이 모두 정상인가요?
3. 얼마나 많은 노드, 사용자 및 시스템 pod 클러스터당 배포는?
4. 디스크 공간 크기를 사용할 수는 용량 문제를?

다음과 같은 상태가 있습니다. 

* **정상** – VM에서 검색된 문제가 없으며 필요한 대로 잘 작동합니다. 
* **위험** – 중요한 문제가 하나 이상 검색되었으며, 정상 작동 상태로 복원하려면 이러한 문제를 해결해야 합니다.
* **경고** - 해결해야 하는 문제가 하나 이상 검색되었으며, 이러한 문제를 해결하지 않으면 상태 조건이 위험으로 변경될 수 있습니다.
* **알 수 없음** – 서비스를 노드 또는 Pod와 연결할 수 없으면 상태가 알 수 없음으로 변경됩니다.
* **찾을 수 없음** - 이 솔루션의 워크로드를 포함하고 있는 작업 영역, 리소스 그룹 또는 구독이 삭제되었습니다.
* **권한 없음** - 사용자에게 작업 영역의 데이터를 읽는 데 필요한 권한이 없습니다.
* **오류** - 작업 영역에서 데이터를 읽으려고 시도하는 동안 오류가 발생했습니다.
* **올바르게 구성되지 않음** - 지정된 작업 영역에서 컨테이너용 Azure Monitor가 올바르게 구성되지 않았습니다.
* **데이터 없음** - 최근 30분 동안 작업 영역에 데이터가 보고되지 않았습니다.

전반적인 클러스터 상태를 계산 하는 상태 *최저* 세 가지 상태 중 하나라도 한 가지 예외로 – 세 가지 상태 *알 수 없는*, 전반적인 클러스터 상태 **알수없음**.  

다음 표에서는 다중 클러스터 보기에서 모니터링되는 클러스터의 상태를 제어하는 계산 방법을 자세히 분석합니다.

| |상태 |가용성 |  
|-------|-------|-----------------|  
|**사용자 Pod**| | |  
| |Healthy |100% |  
| |Warning |90-99% |  
| |중요 |<90% |  
| |알 수 없음 |지난 30분 동안 보고하지 않은 경우 |  
|**시스템 Pod**| | |  
| |Healthy |100% |
| |Warning |N/A |
| |중요 |<100% |
| |알 수 없음 |지난 30분 동안 보고하지 않은 경우 |
|**Node** | | |
| |Healthy |>85% |
| |Warning |60-84% |
| |중요 |<60% |
| |알 수 없음 |지난 30분 동안 보고하지 않은 경우 |

클러스터 목록에서, 클러스터 이름을 클릭하여 **클러스터** 페이지로 드릴다운하거나, 해당 특정 클러스터의 **노드** 열에서 롤업 노드를 클릭하여 **노드** 성능 페이지로 드릴다운하거나, **사용자 Pod** 또는 **시스템 Pod** 열의 롤업을 클릭하여 **컨트롤러** 성능 페이지로 드릴다운할 수 있습니다.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>AKS 클러스터에서 직접 성능 보기
왼쪽 창에서 **인사이트**를 선택하여 AKS 클러스터에서 직접 컨테이너용 Azure Monitor에 액세스할 수 있습니다. AKS 클러스터에 대한 정보 보기는 다음 네 가지 관점으로 구성됩니다.

- 프로비전
- 노드 
- Controllers  
- 컨테이너

**인사이트**를 클릭하면 열리는 기본 페이지는 **클러스터**이며, 이 페이지에는 클러스터의 주요 성능 메트릭을 표시하는 4개의 성능 꺾은선형 차트가 있습니다. 

![클러스터 탭의 예제 성능 차트](./media/container-insights-analyze/containers-cluster-perfview.png)

성능 차트에는 네 가지 성능 메트릭이 표시됩니다.

- **노드 CPU 사용률&nbsp;%**: 전체 클러스터의 관점에서 집계된 CPU 사용률을 보여줍니다. 차트 위에 있는 백분위 선택기에서 개별적이거나 결합된 **평균**, **최소**, **최대**, **50번째**, **90번째** 및 **95번째**를 선택하여 시간 범위에 대한 결과를 필터링할 수 있습니다. 
- **노드 메모리 사용률&nbsp;%**: 전체 클러스터의 관점에서 집계된 메모리 사용률을 보여줍니다. 차트 위에 있는 백분위 선택기에서 개별적이거나 결합된 **평균**, **최소**, **최대**, **50번째**, **90번째** 및 **95번째**를 선택하여 시간 범위에 대한 결과를 필터링할 수 있습니다. 
- **노드 수**: Kubernetes의 노드 수 및 상태입니다. 표시되는 클러스터 노드의 상태는 *모두*, *준비됨* 및 *준비되지 않음*이고 차트 위의 선택기에서 개별적이거나 결합되어 필터링될 수 있습니다. 
- **활동 Pod 수**: Kubernetes의 Pod 수 및 상태입니다. 표시되는 Pod의 상태는 *모두*, *보류 중*, *실행 중* 및 *알 수 없음*이고 차트 위의 선택기에서 개별적이거나 결합되어 필터링될 수 있습니다. 

왼쪽/오른쪽 화살표 키를 사용하여 차트의 각 데이터 요소를 따라 순환하고, 위쪽/아래쪽 화살표 키를 사용하여 백분위 수 줄을 따라 순환할 수 있습니다.

컨테이너에 대 한 azure Monitor에서는 Azure Monitor [메트릭 탐색기](../platform/metrics-getting-started.md), 상관 관계를 지정 고유한 점도 차트를 만들 및 추세를 조사 하 고 수 있는 대시보드에 고정 합니다. 메트릭 탐색기에서 사용할 수도 있습니다 조건을 기반으로 메트릭을 시각화를 설정한 경우는 [메트릭 기반 경고 규칙](../platform/alerts-metric.md)합니다.  

## <a name="view-container-metrics-in-metrics-explorer"></a>메트릭 탐색기에서 컨테이너 메트릭 보기
메트릭 탐색기에서 집계 된 노드를 볼 수 있으며 컨테이너에 대 한 Azure Monitor에서 사용률 메트릭을 pod 수 있습니다. 다음 표에서 컨테이너 메트릭을 시각화 하는 메트릭 차트를 사용 하는 방법을 이해할 수 있도록 세부 정보를 보여 줍니다.

|네임스페이스 | 메트릭 |
|----------|--------|
| insights.container/nodes | |
| | cpuUsageMillicores |
| | cpuUsagePercentage |
| | memoryRssBytes |
| | memoryRssPercentage |
| | memoryWorkingSetBytes |
| | memoryWorkingSetPercentage |
| | nodesCount |
| insights.container/pods | |
| | PodCount |

적용할 수 있습니다 [분할](../platform/metrics-charts.md#apply-splitting-to-a-chart) 차원 보기의 다양 한 세그먼트를 시각화 하는 메트릭의 서로 비교 합니다. 노드의 경우에 따라 차트를 분할할 수 있습니다 합니다 *호스트* 차원에 있는 pod에서 별로 분할할 수 있습니다이 다음과 같은 차원 및:

* Controller
* Kubernetes 네임 스페이스
* 노드
* 단계

## <a name="analyze-nodes-controllers-and-container-health"></a>노드, 컨트롤러 및 컨테이너 상태를 분석 합니다.

**노드**, **컨트롤러** 및 **컨테이너** 탭으로 전환하면 속성 창이 페이지의 오른쪽에 자동으로 표시됩니다.  Kubernetes 개체를 구성 하 고 정의한 레이블을 포함 하는 항목이 선택의 속성이 표시 됩니다. 창을 표시하거나 숨기려면 창에서 **>>** 링크를 클릭합니다.  

![Kubernetes 큐브 뷰 속성 창 예제](./media/container-insights-analyze/perspectives-preview-pane-01.png)

계층 구조에서 개체를 펼치면 선택한 개체에 따라 속성 창이 업데이트됩니다. 이 창에서 창 위쪽의 **Kubernetes 이벤트 로그 보기** 링크를 클릭하여 미리 정의된 로그 검색을 통해 Kubernetes 이벤트를 볼 수도 있습니다. Kubernetes 로그 데이터를 보는 방법에 대한 자세한 내용은 [로그를 검색하여 데이터 분석](container-insights-log-search.md)을 참조하세요. **컨테이너** 보기에서 컨테이너를 검토할 때 컨테이너 로그를 실시간으로 볼 수 있습니다. 이 기능 및 액세스 권한을 부여하고 제어하는 데 필요한 구성에 대한 자세한 내용은 [컨테이너용 Azure Monitor를 사용하여 실시간으로 컨테이너 로그를 보는 방법](container-insights-live-logs.md)을 참조하세요. 

사용 하 여는 **+ 필터 추가** 하 여 뷰에 대 한 결과 필터링 할 옵션 페이지의 맨 위에서 **Service**, **노드**, **Namespace**, 또는  **노드 풀** 선택한 필터 범위를 선택한 후 다음에 표시 된 값 중 하나는 **값을 선택** 필드입니다.  필터가 구성되면 AKS 클러스터의 측면을 살펴볼 때 필터가 전역적으로 적용됩니다.  수식은 등호만 지원됩니다.  첫 번째 필터에 필터를 추가하여 결과를 더 좁힐 수 있습니다.  예를 들어 **노드**로 필터를 지정한 경우 두 번째 필터는 **서비스** 또는 **네임스페이스**만 선택할 수 있습니다.  

![필터를 사용하여 결과를 좁히는 예제](./media/container-insights-analyze/add-filter-option-01.png)

한 탭에서 필터를 지정하면 다른 필터를 선택할 때 계속 적용되고 지정된 필터 옆에 있는 **x**를 클릭하면 필터가 삭제됩니다.   

**노드** 탭으로 전환하면 행 계층 구조는 클러스터의 노드로 시작하는 Kubernetes 개체 모델을 따릅니다. 노드를 확장하면 노드에서 실행 중인 하나 이상의 창을 볼 수 있습니다. 하나 이상의 컨테이너가 Pod로 그룹화된 경우 계층 구조에서 마지막 행으로 표시됩니다. 호스트에 프로세서 또는 메모리 부족 문제가 있는 경우 호스트에서 실행 중인 Pod와 관련되지 않은 워크로드의 수를 볼 수도 있습니다.

![성능 보기의 Kubernetes 노드 계층 예제](./media/container-insights-analyze/containers-nodes-view.png)

Linux OS를 실행하는 Azure Container Instances 가상 노드는 목록에서 마지막 AKS 클러스터 노드 뒤에 표시됩니다.  ACI 가상 노드를 확장하면 해당 노드에서 실행되는 하나 이상의 ACI Pod 및 컨테이너를 볼 수 있습니다.  노드가 아닌 Pod에 대한 메트릭만 수집 및 보고됩니다.

![Container Instances가 나열된 노드 계층 구조 예제](./media/container-insights-analyze/nodes-view-aci.png)

펼친 노드에서, 노드에서 실행되는 Pod 또는 컨테이너에서 컨트롤러로 드릴다운하여 해당 컨트롤러에 대해 필터링된 성능 데이터를 볼 수 있습니다. 특정 노드에 대한 **컨트롤러** 열 아래의 값을 클릭합니다.   
![성능 보기에서 보여 주는 노드에서 컨트롤러로의 드릴다운 예제](./media/container-insights-analyze/drill-down-node-controller.png)

페이지의 위쪽에서 컨트롤러 또는 컨테이너를 선택하고, 해당 개체에 대한 상태 및 리소스 사용률을 검토할 수 있습니다.  대신, 메모리 사용률을 검토하려면 **메트릭** 드롭다운 목록에서 **메모리 RSS** 또는 **메모리 작업 집합**을 선택합니다. **메모리 RSS**는 Kubernetes 버전 1.8 이상에서만 지원됩니다. 그렇지 않으면 **Min&nbsp;%** 에 대한 값이 *NaN&nbsp;%* 로 표시됩니다. 이것은 정의되지 않았거나 표현할 수 없는 값을 나타내는 숫자 데이터 형식 값입니다. 

![컨테이너 노드 성능 보기](./media/container-insights-analyze/containers-node-metric-dropdown.png)

기본적으로 성능 데이터는 마지막 6시간을 기준으로 하지만, 왼쪽 위에 있는 **TimeRange** 옵션을 사용하여 기간을 변경할 수 있습니다. 백분위 선택기에서 **평균**, **최소**, **최대**, **50번째**, **90번째** 및 **95번째**를 선택하여 시간 범위 내에서 결과를 필터링할 수도 있습니다. 

![데이터 필터링에 대한 백분위 선택 영역](./media/container-insights-analyze/containers-metric-percentile-filter.png)

**추세** 열 아래에서 마우스로 막대 그래프를 가리키면 선택한 메트릭에 따라 각 막대가 15분의 샘플 기간 내에서 CPU 또는 메모리 사용량을 보여줍니다. 키보드로 추세 차트를 선택한 후 Alt+PageUp 또는 Alt+PageDown 키를 사용하여 각 막대를 개별적으로 순환하고 마우스를 위로 가져가 동일한 세부 정보를 볼 수 있습니다.

![예제 차트를 마우스로 막대 추세](./media/container-insights-analyze/containers-metric-trend-bar-01.png)    

다음 예제에서 목록의 첫 번째인 *aks-nodepool1-* 노드는 **컨테이너** 값이 9이며, 이것은 배포된 전체 컨테이너 수를 롤업한 값입니다.

![노드당 컨테이너 롤업 예제](./media/container-insights-analyze/containers-nodes-containerstotal.png)

이를 통해 클러스터의 노드 간에 컨테이너가 적절히 분산되어 있는지 여부를 빠르게 알 수 있습니다. 

노드를 볼 때 표시되는 정보는 다음 표에 설명되어 있습니다.

| 열 | 설명 | 
|--------|-------------|
| Name | 호스트의 이름입니다. |
| 상태 | 노드 상태의 Kubernetes 보기입니다. |
| 평균&nbsp;%, 최소&nbsp;%, 최대&nbsp;%, 50번째&nbsp;%, 90번째&nbsp;% | 선택한 기간 동안 백분위에 기반한 평균 노드 백분율입니다. |
| 평균, 최소, 최대, 50번째, 90번째 | 선택한 기간 동안 백분위를 기준으로 하는 평균 노드 실제 값입니다. 평균 값은 노드에 대해 설정된 CPU/메모리 제한에서 측정됩니다. Pod 및 컨테이너의 경우에는 호스트에서 보고된 평균 값입니다. |
| 컨테이너 | 컨테이너의 수입니다. |
| 작동 시간 | 노드가 시작되었거나 다시 부팅된 이후 경과된 시간을 나타냅니다. |
| Controllers | 컨테이너 및 Pod용입니다. 어떤 컨트롤러가 상주하는지 보여줍니다. 모든 Pod가 컨트롤러에 있는 것은 아니므로 **N/A**가 표시될 수 있습니다. | 
| 추세 평균&nbsp;%, 최소&nbsp;%, 최대&nbsp;%, 50번째&nbsp;%, 90번째&nbsp;% | 컨트롤러의 평균 백분위 메트릭 비율을 나타내는 막대 그래프 추세입니다. |

선택기에서 **컨트롤러**를 선택합니다.

![컨트롤러 보기 선택](./media/container-insights-analyze/containers-controllers-tab.png)

여기서 컨트롤러와 ACI 가상 노드 컨트롤러 또는 컨트롤러에 연결되지 가상 노드 Pod의 성능 상태를 볼 수 있습니다.

![<Name> 컨트롤러 성능 보기](./media/container-insights-analyze/containers-controllers-view.png)

행 계층 구조는 컨트롤러로 시작하고, 컨트롤러를 펼치면 하나 이상의 Pod가 표시됩니다.  Pod를 펼치면 마지막 행에 Pod로 그룹화된 컨테이너가 표시됩니다. 펼친 컨트롤러에서 실행되는 노드로 드릴다운하여 해당 노드에 대해 필터링된 성능 데이터를 볼 수 있습니다. 컨트롤러에 연결되지 않은 ACI Pod는 목록의 마지막에 나열됩니다.

![Container Instances Pod가 나열된 컨트롤러 계층 구조 예제](./media/container-insights-analyze/controllers-view-aci.png)

특정 컨트롤러에 대한 **노드** 열 아래의 값을 클릭합니다.   

![성능 보기에서 보여주는 노드에서 컨트롤러로의 드릴다운 예제](./media/container-insights-analyze/drill-down-controller-node.png)

컨트롤러를 볼 때 표시되는 정보는 다음 표에 설명되어 있습니다.

| 열 | 설명 | 
|--------|-------------|
| Name | 컨트롤러의 이름입니다.|
| 상태 | *확인*, *종료됨*, *실패함*, *중지됨* 또는 *일시 정지됨*과 같은 상태로 실행이 완료된 컨테이너의 롤업 상태입니다. 컨테이너가 실행 중이지만 상태가 제대로 표시되지 않았거나 에이전트에 의해 선택되지 않았고 30분 넘게 응답하지 않은 경우 상태는 *알 수 없음*이 됩니다. 상태 아이콘에 대한 자세한 내용은 아래 표에 나와 있습니다.|
| 평균&nbsp;%, 최소&nbsp;%, 최대&nbsp;%, 50번째&nbsp;%, 90번째&nbsp;% | 각 엔터티에 선택한 메트릭 및 백분위 수의 평균 백분율의 평균을 롤업 합니다. |
| 평균, 최소, 최대, 50번째, 90번째  | 선택된 백분위에 대한 컨테이너의 평균 CPU 밀리코어 또는 메모리 성능의 롤업입니다. 평균 값은 Pod에 대해 설정된 CPU/메모리 제한에서 측정됩니다. |
| 컨테이너 | 컨트롤러 또는 Pod에 대한 컨테이너의 총 수입니다. |
| Restarts | 컨테이너에서 다시 시작한 횟수의 롤업입니다. |
| 작동 시간 | 컨테이너가 시작된 이후 경과된 시간을 나타냅니다. |
| 노드 | 컨테이너 및 Pod용입니다. 컨테이너 또는 Pod가 있는 컨트롤러를 표시합니다. | 
| 추세 평균&nbsp;%, 최소&nbsp;%, 최대&nbsp;%, 50번째&nbsp;%, 90번째&nbsp;%| 컨트롤러의 평균 백분위 메트릭을 나타내는 막대 그래프 추세입니다. |

상태 필드의 아이콘은 컨테이너의 온라인 상태를 나타냅니다.
 
| 아이콘 | 상태 | 
|--------|-------------|
| ![실행이 준비됨 상태 아이콘](./media/container-insights-analyze/containers-ready-icon.png) | 실행 중(준비됨)|
| ![대기 중 또는 일시 중지됨 상태 아이콘](./media/container-insights-analyze/containers-waiting-icon.png) | 대기 중 또는 일시 중지됨|
| ![마지막으로 보고된 실행 중 상태 아이콘](./media/container-insights-analyze/containers-grey-icon.png) | 하지만 마지막으로 보고된 실행 중 상태는 30분 넘게 응답되지 않았습니다.|
| ![성공 상태 아이콘](./media/container-insights-analyze/containers-green-icon.png) | 성공적으로 중지됨 또는 중지하지 못함|

상태 아이콘은 Pod에서 제공하는 것에 따라 개수를 보여줍니다. 두 가지 나쁜 상태를 보여주고 있으며, 상태를 마우스로 가리키면 컨테이너에 있는 모든 Pod의 롤업 상태가 표시됩니다. 준비 상태가 아니면 상태 값은 **(0)** 을 표시합니다. 

선택기에서 **컨테이너**를 선택합니다.

![컨테이너 보기 선택](./media/container-insights-analyze/containers-containers-tab.png)

여기서 Azure Kubernetes 및 Azure Container Instances 컨테이너의 성능 상태를 볼 수 있습니다.  

![<Name> 컨트롤러 성능 보기](./media/container-insights-analyze/containers-containers-view.png)

컨테이너에서 Pod 또는 노드로 드릴다운하여 해당 개체에 대해 필터링된 성능 데이터를 볼 수 있습니다. 특정 컨테이너에 대한 **Pod** 또는 **노드** 열 아래의 값을 클릭합니다.   

![성능 보기에서 보여주는 노드에서 컨트롤러로의 드릴다운 예제](./media/container-insights-analyze/drill-down-controller-node.png)

컨테이너를 볼 때 표시되는 정보는 다음 표에 설명되어 있습니다.

| 열 | 설명 | 
|--------|-------------|
| Name | 컨트롤러의 이름입니다.|
| 상태 | 컨테이너의 상태입니다(있는 경우). 상태 아이콘에 대한 자세한 내용은 아래 표에 나와 있습니다.|
| 평균&nbsp;%, 최소&nbsp;%, 최대&nbsp;%, 50번째&nbsp;%, 90번째&nbsp;% | 선택한 메트릭 및 백분위에 대한 각 엔터티 평균 백분율의 롤업입니다. |
| 평균, 최소, 최대, 50번째, 90번째  | 선택된 백분위에 대한 컨테이너의 평균 CPU 밀리코어 또는 메모리 성능의 롤업입니다. 평균 값은 Pod에 대해 설정된 CPU/메모리 제한에서 측정됩니다. |
| Pod | Pod가 위치한 컨테이너입니다.| 
| 노드 |  컨테이너가 있는 노드입니다. | 
| Restarts | 컨테이너가 시작된 이후 경과된 시간을 나타냅니다. |
| 작동 시간 | 컨테이너가 시작 또는 다시 부팅된 이후 경과된 시간을 나타냅니다. |
| 추세 평균&nbsp;%, 최소&nbsp;%, 최대&nbsp;%, 50번째&nbsp;%, 90번째&nbsp;% | 컨테이너의 평균 백분위 메트릭 비율을 나타내는 막대 그래프 추세입니다. |

상태 필드의 아이콘은 다음 표에 설명된 대로 Pod의 온라인 상태를 나타냅니다.
 
| 아이콘 | 상태 |  
|--------|-------------|  
| ![실행이 준비됨 상태 아이콘](./media/container-insights-analyze/containers-ready-icon.png) | 실행 중(준비됨)|  
| ![대기 중 또는 일시 중지됨 상태 아이콘](./media/container-insights-analyze/containers-waiting-icon.png) | 대기 중 또는 일시 중지됨|  
| ![마지막으로 보고된 실행 중 상태 아이콘](./media/container-insights-analyze/containers-grey-icon.png) | 하지만 마지막으로 보고된 실행 중 상태는 30분 넘게 응답되지 않았습니다.|  
| ![종료됨 상태 아이콘](./media/container-insights-analyze/containers-terminated-icon.png) | 성공적으로 중지됨 또는 중지하지 못함|  
| ![실패 상태 아이콘](./media/container-insights-analyze/containers-failed-icon.png) | 실패 상태 |  

## <a name="next-steps"></a>다음 단계
- 검토 합니다 [컨테이너에 대 한 Azure Monitor를 사용 하 여 성능 경고를 만들](container-insights-alerts.md) DevOps 또는 운영 프로세스 및 프로시저를 지원 하기 위해 높은 CPU 및 메모리 사용률에 대 한 경고를 만드는 방법. 
- 뷰 [쿼리 예제 로그](container-insights-log-search.md#search-logs-to-analyze-data) 에 미리 정의 된 쿼리 및 평가 하거나 경고를 시각화, 분석 하 여 클러스터에 대 한 사용자 지정 하는 예제를 참조 하세요.
