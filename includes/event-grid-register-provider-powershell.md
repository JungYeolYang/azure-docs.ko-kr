---
title: 포함 파일
description: 포함 파일
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 68a208af1a9aa9e73f2af99021d195f264fb21f1
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55664440"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid 리소스 공급자를 사용하도록 설정

이전에 Azure 구독에서 Event Grid를 사용하지 않은 경우 Event Grid 리소스 공급자를 등록해야 할 수 있습니다. 다음 명령 실행:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

등록을 완료하는 데 잠시 시간이 걸릴 수 있습니다. 상태를 확인하려면 다음을 실행합니다.

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

`RegistrationStatus`가 `Registered`이면 진행할 준비가 된 것입니다.
