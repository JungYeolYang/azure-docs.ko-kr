---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 42ab8be45d4086589f0793531003700e7552a440
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64744371"
---
**아웃바운드 데이터 전송**: [아웃바운드 데이터 전송](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure 데이터 센터에서 데이터 전송) 시 대역폭 사용에 대해 청구가 발생합니다.

**트랜잭션**: 표준 관리형 디스크에서 수행하는 트랜잭션 수에 따라 요금이 청구됩니다. 표준 Ssd에 256 3:n2}kib/의 I/O 단위 크기를 트랜잭션 수가 계정에 사용 됩니다. 더 큰 I/O 크기는 256KiB 크기의 여러 I/O로 계산됩니다. 표준 Hdd 각 IO 작업의 I/O 크기에 관계 없이 단일 트랜잭션으로 간주 됩니다.

트랜잭션 비용을 포함 하 여 Managed Disks에 대 한 가격 책정에 대 한 자세한 내용은 참조 하세요 [Managed Disks 가격 책정](https://azure.microsoft.com/pricing/details/managed-disks)합니다.

### <a name="ultra-ssd-vm-reservation-fee"></a>Ultra SSD VM 예약 요금

Azure VM은 울트라 SSD와 호환되는지 여부를 나타낼 수 있습니다. 울트라 디스크 호환 VM은 컴퓨팅 VM 인스턴스와 블록 스토리지 배율 단위 간에 전용 대역폭 용량을 할당하여 성능을 최적화하고 대기 시간을 줄입니다. VM에 이 기능을 추가하면 울트라 디스크를 연결하지 않고 VM에서 울트라 디스크 기능을 활성화한 경우에만 적용되는 예약 요금이 발생합니다. 울트라 디스크 호환 VM에 울트라 디스크가 연결되면 이 요금이 적용되지 않습니다. 이 요금은 VM에서 프로비전되는 vCPU당 부과됩니다.

울트라 디스크 가격 책정 세부 정보는 [Azure Disks 가격 책정 페이지](https://azure.microsoft.com/pricing/details/managed-disks/)를 참조하세요.