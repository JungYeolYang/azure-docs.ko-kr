---
title: Azure Cosmos DB에서 다중 마스터를 구성하는 방법
description: Azure Cosmos DB의 애플리케이션에서 다중 마스터를 구성하는 방법을 알아봅니다.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: b862c59002369662d37b6d6a9de28370b0000497
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682273"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Azure Cosmos DB를 사용하는 애플리케이션에서 다중 마스터를 구성하는 방법

애플리케이션에서 다중 마스터 기능을 사용하려면 Azure Cosmos DB에서 다중 지역 쓰기를 사용하도록 설정하고 멀티호밍 기능을 구성해야 합니다. 멀티호밍은 애플리케이션이 배포된 지역을 설정하여 구성됩니다.

## <a id="netv2"></a>.NET SDK v2

애플리케이션에서 다중 마스터를 사용하도록 설정하려면 `UseMultipleWriteLocations`를 true로 설정하고 `SetCurrentLocation`을 애플리케이션이 배포되고 Azure Cosmos DB가 복제되는 지역으로 구성합니다.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK v3(미리 보기)

애플리케이션에서 다중 마스터를 사용하도록 설정하려면 `UseCurrentRegion`을 애플리케이션이 배포되고 Cosmos DB가 복제되는 지역으로 구성합니다.

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>Java 비동기 SDK

애플리케이션에서 다중 마스터를 사용하도록 설정하려면 `policy.setUsingMultipleWriteLocations(true)`를 설정하고 `policy.setPreferredLocations`를 애플리케이션이 배포되고 Cosmos DB가 복제되는 지역으로 구성합니다.

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>Node.js, JavaScript, TypeScript SDK

애플리케이션에서 다중 마스터를 사용하도록 설정하려면 `connectionPolicy.UseMultipleWriteLocations`를 true로 설정하고 `connectionPolicy.PreferredLocations`를 애플리케이션이 배포되고 Cosmos DB가 복제되는 지역으로 구성합니다.

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>Python SDK

애플리케이션에서 다중 마스터를 사용하도록 설정하려면 `connection_policy.UseMultipleWriteLocations`를 true로 설정하고 `connection_policy.PreferredLocations`를 애플리케이션이 배포되고 Cosmos DB가 복제되는 지역으로 구성합니다.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>다음 단계

이제 다음 문서를 읽을 수 있습니다.

* [Azure Cosmos DB의 일관성 관리에 세션 토큰 활용](how-to-manage-consistency.md#utilize-session-tokens)
* [Azure Cosmos DB의 충돌 유형 및 해결 정책](conflict-resolution-policies.md)
* [Azure Cosmos DB의 고가용성](high-availability.md)
* [Azure Cosmos DB의 일관성 수준](consistency-levels.md)
* [Azure Cosmos DB에서 적절한 일관성 수준 선택](consistency-levels-choosing.md)
* [Azure Cosmos DB의 일관성, 가용성 및 성능 절충](consistency-levels-tradeoffs.md)
* [다양한 일관성 수준의 가용성 및 성능 절충](consistency-levels-tradeoffs.md)
* [전역적으로 프로비전된 처리량 크기 조정](scaling-throughput.md)
* [글로벌 배포 - 내부 살펴보기](global-dist-under-the-hood.md)
