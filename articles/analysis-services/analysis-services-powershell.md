---
title: PowerShell을 사용하여 Azure Analysis Services 관리 | Microsoft Docs
description: PowerShell 사용한 Azure Analysis Services 관리입니다.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 1f9c30f1c914f6c8d42967e014d967ba0d5b85cc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58893846"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>PowerShell을 사용하여 Azure Analysis Services 관리

이 문서에서는 Azure Analysis Services 서버 및 데이터베이스 관리 작업을 수행하는 데 사용되는 PowerShell cmdlet에 대해 설명합니다. 

서버 만들기 또는 삭제, 서버 작업 일시 중단 또는 다시 시작 또는 서비스 수준(계층) 변경과 같은 서버 관리 작업은 Azure Resource Manager(리소스) 및 Analysis Services(서버) cmdlet을 사용합니다. 역할 멤버 추가 또는 제거, 처리 또는 분할과 같은 기타 데이터베이스 관리 작업에서는 SQL Server Analysis Services와 동일한 SqlServer 모듈에 포함된 cmdlet을 사용합니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>권한

대부분의 PowerShell 작업에는 관리하는 Analysis Services 서버에 대해 관리자 권한이 있어야 합니다. 예약된 PowerShell 작업은 무인 작업입니다. 스케줄러를 실행하는 계정 또는 서비스 주체에게는 Analysis Services 서버에 대해 관리자 권한이 있어야 합니다. 

Azure PowerShell cmdlet을 사용 하 여 서버 작업 계정 또는 스케줄러를 실행 하는 계정에도 속해야 합니다 리소스에 대 한 소유자 역할과 [azure 역할 기반 Access Control (RBAC)](../role-based-access-control/overview.md)합니다. 

## <a name="resource-management-operations"></a>리소스 관리 작업 

Module - [Az.AnalysisServices](/powershell/module/az.analysisservices)

|Cmdlet|설명| 
|------------|-----------------| 
|[Get-AzAnalysisServicesServer](/powershell/module/az.analysisservices/get-azanalysisservicesserver)|서버 인스턴스에 대한 세부 정보를 가져옵니다.|  
|[New-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver)|서버 인스턴스를 만듭니다.|   
|[New-AzAnalysisServicesFirewallConfig](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallconfig)|새 Analysis Services 방화벽 구성을 만듭니다.|   
|[New-AzAnalysisServicesFirewallRule](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallrule)|새 Analysis Services 방화벽 규칙을 만듭니다.|   
|[Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/remove-azanalysisservicesserver)|서버 인스턴스를 제거합니다.|  
|[Resume-AzAnalysisServicesServer](/powershell/module/az.analysisservices/resume-azanalysisservicesserver)|서버 인스턴스를 다시 시작합니다.|  
|[Suspend-AzAnalysisServicesServer](/powershell/module/az.analysisservices/suspend-azanalysisservicesserver)|서버 인스턴스를 일시 중단합니다.| 
|[Set-AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver)|서버 인스턴스를 수정합니다.|   
|[Test-AzAnalysisServicesServer](/powershell/module/az.analysisservices/test-azanalysisservicesserver)|서버 인스턴스의 존재 여부를 테스트합니다.| 

## <a name="server-management-operations"></a>서버 관리 작업

모듈 - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|설명| 
|------------|-----------------| 
|[Add-AzAnalysisServicesAccount](/powershell/module/az.analysisservices/add-AzAnalysisServicesaccount)|Azure Analysis Services 서버 cmdlet 요청에 사용할 인증된 계정을 추가합니다.| 
|[Export-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/export-AzAnalysisServicesinstancelog)|현재 로그온 한 환경에서 추가 AzAnalysisServicesAccount 명령에 지정 된 대로 Analysis Services 서버 인스턴스에서 로그 내보내기|  
|[Restart-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|현재 로그인 환경의; Analysis Services 서버 인스턴스를 다시 시작 추가 AzAnalysisServicesAccount 명령에 지정 합니다.|  
|[Sync-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|지정 된 현재 로그온 한 환경에서 추가 AzAnalysisServicesAccount 명령에 지정 된 대로 있는 모든 쿼리 스케일 아웃 인스턴스에 Analysis Services 서버 인스턴스에서 지정된 된 데이터베이스를 동기화합니다.|  

## <a name="database-operations"></a>데이터베이스 작업

Azure Analysis Services 데이터베이스 작업에서는 SQL Server Analysis Services와 동일한 [SqlServer 모듈](https://www.powershellgallery.com/packages/SqlServer)을 사용합니다. 그러나 모든 cmdlet이 Azure Analysis Services에서 지원되는 것은 아닙니다. 자세한 내용은 [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell)을 참조하세요.

SqlServer 모듈은 TMSL(테이블 형식 모델 스크립트 언어) 쿼리 또는 스크립트를 허용하는 범용 Invoke-ASCmd cmdlet뿐만 아니라 작업 관련 데이터베이스 관리 cmdlet도 제공합니다. SqlServer 모듈의 다음 cmdlet은 Azure Analysis Services에서 지원됩니다.

  
|Cmdlet|설명|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|데이터베이스 역할에 구성원을 추가합니다.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Analysis Services 데이터베이스를 백업합니다.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|데이터베이스 역할에서 구성원을 제거합니다.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|TMSL 스크립트를 실행합니다.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|데이터베이스를 처리합니다.|  
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|파티션을 처리합니다.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|테이블을 처리합니다.|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|파티션을 병합합니다.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Analysis Services 데이터베이스를 복원합니다.| 
  

## <a name="related-information"></a>관련 정보

* [SQL Server PowerShell 모듈 다운로드](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS 다운로드](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SqlServer module in PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer)(PowerShell 갤러리의 SqlServer 모듈)    
* [Tabular Model Programming for Compatibility Level 1200 and higher](/sql/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)(호환성 수준 1200 이상에 대한 테이블 형식 모델 프로그래밍)
