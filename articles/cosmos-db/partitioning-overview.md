---
title: Azure Cosmos DB에서 분할
description: Azure Cosmos DB에서 분할의 개요입니다.
ms.author: rimman
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.openlocfilehash: e88be8e7b94566ff94dd94a8679f8ade9d54c0b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60928705"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB에서 분할

Azure Cosmos DB를 사용 하 여 분할 응용 프로그램의 성능 요구 사항에 맞도록 데이터베이스의 개별 컨테이너를 확장 합니다. 분할에서 컨테이너의 항목을 호출 하는 고유 하위 집합으로 나뉩니다 *논리 파티션이*합니다. 값에 따라 논리 파티션이 형성 된를 *파티션 키* 컨테이너의 각 항목을 사용 하 여 연결 합니다. 논리적 분할의 모든 항목에는 동일한 파티션 키 값을 포함 합니다.

예를 들어, 컨테이너는 항목을 보유 합니다. 각 항목에 대 한 고유 값에는 `UserID` 속성입니다. 하는 경우 `UserID` 파티션으로 역할 컨테이너의 항목에 대 한 키 및 고유 1,000 가지 `UserID` 값을 논리적 파티션 수가 1,000 컨테이너에 대해 만들어집니다.

항목의 논리 파티션을 결정 하는 파티션 키 외에도 컨테이너의 각 항목에는 *항목의 ID* (논리 파티션 내에서 고유한). 항목을 만들고 파티션 키 및 항목 ID를 결합 *인덱스*를 고유 하 게 식별 하는 항목입니다.

[파티션 키를 선택](partitioning-overview.md#choose-partitionkey) 응용 프로그램의 성능 영향을 주는 중요 한 결정 됩니다.

## <a name="managing-logical-partitions"></a>논리 파티션 관리

Azure Cosmos DB 투명 하 고 자동으로 배치의 실제 파티션 효율적으로 컨테이너의 확장성 및 성능 요구를 충족 시키는 논리적 파티션 관리 합니다. 응용 프로그램의 처리량 및 저장소 요구 사항 증가, Azure Cosmos DB 자동으로 더 많은 서버 간에 부하를 분산할 논리 파티션이 이동 합니다. 

Azure Cosmos DB를 사용 하 여 해시 기반 분할 논리 파티션이 실제 파티션 간에 분산 합니다. Azure Cosmos DB 파티션 키 값 항목의 해시입니다. 해시 된 결과는 실제 파티션을 결정합니다. 그런 다음 Azure Cosmos DB 키의 공간을 할당 파티션 키 해시 균등 하 게 실제 파티션 간에 합니다.

단일 논리 파티션 내의 데이터에 액세스 하는 쿼리는 여러 파티션에 액세스 하는 쿼리보다 더 비용 효율적입니다. 트랜잭션 (저장된 프로시저 또는 트리거)는 단일 논리 파티션에의 항목에 대해서만 허용 됩니다.

Azure Cosmos DB는 파티션을 관리 하는 방법에 대 한 자세한 내용은 참조 하세요 [논리 파티션이](partition-data.md)합니다. (빌드 또는 응용 프로그램을 실행 하려면 내부 세부 정보를 이해 하는 데 필요한 아닙니다 되지만 궁금한 점이 있으면 판독기 여기에 추가 합니다.)

## <a id="choose-partitionkey"></a>파티션 키 선택

다음은 파티션 키를 선택 하는 것에 대 한 좋은 지침입니다.

* 단일 논리 파티션에 10GB의 저장소의 상한값을 적용 합니다.  

* Azure Cosmos 컨테이너는 최소 처리량 400 요청 단위 / 초 (RU/s)의 경우 동일한 파티션 키에 대 한 요청 파티션에 할당 된 처리량을 초과할 수 없습니다. 요청 할당된 된 처리량을 초과 하는 경우 요청은 속도가 제한 됩니다. 따라서 애플리케이션 내에서 "핫 스폿"을 초래하지 않는 파티션 키를 선택해야 합니다.

* 값 범위가 방대하고 논리 파티션 간에 액세스 패턴이 균등하게 분산되는 파티션 키를 선택해야 합니다. 이렇게 하면 논리 파티션 집합에서 데이터 및 컨테이너의 작업은 분산 데이터 저장소 및 처리량에 대 한 리소스를 논리 파티션으로 분산 될 수 있도록 합니다.

* 시간이 지남에 따라 균등 하 게 하 고 모든 파티션에 균등 하 게 작업 부하를 분산 하는 파티션 키를 선택 합니다. 파티션 키를 선택할 때는 효율적인 파티션 쿼리 및 확장성을 위해 항목을 여러 파티션에 분산의 목표에 대 한 트랜잭션에 대 한 필요성을 조정 해야 합니다.

* 파티션 키에 대 한 후보 쿼리에서 필터로 자주 나타나는 속성을 포함할 수 있습니다. 필터 조건자에 파티션 키를 포함하여 쿼리를 효율적으로 라우팅할 수 있습니다.

## <a name="next-steps"></a>다음 단계

* 에 대 한 자세한 [분할 및 Azure Cosmos DB에서 수평](partition-data.md)합니다.
* [Azure Cosmos DB에서 프로비전된 처리량](request-units.md)에 대한 자세한 정보
* [Azure Cosmos DB에서 글로벌 배포](distribute-data-globally.md)에 대한 자세한 정보
