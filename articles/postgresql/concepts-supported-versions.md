---
title: Azure Database for PostgreSQL에서 지원되는 버전
description: PostgreSQL용 Azure 데이터베이스에서 지원되는 버전에 대해 설명합니다.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: 16064d662c5b101e30f8d2fbb64b39db0848f49f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702317"
---
# <a name="supported-postgresql-database-versions"></a>지원되는 PostgreSQL 데이터베이스 버전
Microsoft은 Azure Database for PostgreSQL 서비스에서 PostgreSQL 엔진의 n-2 버전을 지원하는 것을 목표로 하고 있습니다. 버전은 Azure(n)의 현재 주요 버전과 두 개의 이전 주요 버전(-2)이 됩니다.

PostgreSQL용 Azure 데이터베이스는 현재 다음 버전을 지원합니다.

## <a name="postgresql-version-107"></a>PostgreSQL 버전 10.7
이 부 버전의 향상된 기능 및 수정 내용에 대한 자세한 내용은 [PostgreSQL 설명서](https://www.postgresql.org/docs/10/static/release-10-7.html)를 참조하세요.

## <a name="postgresql-version-9612"></a>PostgreSQL Version 9.6.12
이 부 버전의 향상된 기능 및 수정 내용에 대한 자세한 내용은 [PostgreSQL 설명서](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html)를 참조하세요.

## <a name="postgresql-version-9516"></a>PostgreSQL 버전 9.5.16
이 부 버전의 향상된 기능 및 수정 내용에 대한 자세한 내용은 [PostgreSQL 설명서](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html)를 참조하세요.

## <a name="managing-updates-and-upgrades"></a>업데이트 및 업그레이드 관리
Azure Database for PostgreSQL는 부 버전 패치를 자동으로 관리합니다. 현재, 주 버전 업그레이드는 지원되지 않습니다. 예를 들어 PostgreSQL 9.5에서 PostgreSQL 9.6으로의 업그레이드는 지원되지 않습니다. 다음의 주 버전으로 업그레이드하려는 경우 새 엔진 버전을 사용하여 만든 서버에 주 버전을 [덤프 및 복원](./howto-migrate-using-dump-and-restore.md)하는 데이터베이스를 만듭니다.

## <a name="next-steps"></a>다음 단계
다른 PostgreSQL 확장 지원에 대한 자세한 내용은 [PostgreSQL 확장](concepts-extensions.md)을 참조하세요.
