---
title: Contoso Linux 서비스 데스크 앱을 Azure 및 Azure MySQL에 다시 호스트 | Microsoft Docs
description: Contoso가 Azure VM 및 Azure MySQL로 마이그레이션하여 온-프레미스 Linux 앱을 다시 호스트하는 방법을 알아봅니다.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 114f4ccccaa861928263eb59b4e43379989abcca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60670510"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Contoso 마이그레이션: 온-프레미스 Linux 앱을 Azure VM 및 Azure MySQL에 다시 호스트

이 문서에서는 Contoso가 Azure 및 Azure MySQL로 마이그레이션하여 해당 온-프레미스 2계층 Linux 서비스 데스크 앱(osTicket)을 다시 호스트하는 방법을 보여줍니다.

이 문서는 가상 회사 Contoso가 온-프레미스 리소스를 Microsoft Azure 클라우드로 마이그레이션하는 방법을 보여주는 문서 시리즈 중 하나입니다. 시리즈에는 배경 정보 및 마이그레이션 인프라를 설정하고 다양한 유형의 마이그레이션을 실행하는 방법을 보여 주는 시나리오가 포함되어 있습니다. 시나리오가 복잡해지고 있으며, 앞으로 계속해서 문서가 추가될 것입니다.

**문서** | **세부 정보** | **상태**
--- | --- | ---
[문서 1: 개요](contoso-migration-overview.md) | 문서 시리즈, Contoso의 마이그레이션 전략 및 시리즈에서 사용되는 샘플 앱에 대해 간략히 설명합니다. | 사용 가능
[문서 2: Azure 인프라 배포](contoso-migration-infrastructure.md) | Contoso에서 마이그레이션을 위해 온-프레미스 인프라와 Azure 인프라를 준비합니다. 이 시리즈의 모든 마이그레이션 관련 문서에서 동일한 인프라가 사용됩니다. | 사용 가능
[문서 3: Azure로 마이그레이션하기 위한 온-프레미스 리소스 평가](contoso-migration-assessment.md)  | Contoso가 VMware에서 실행되는 온-프레미스 SmartHotel360 앱의 평가를 실행합니다. Contoso에서 Azure Migrate 서비스를 사용하여 앱 VM을 평가하고, Database Migration Assistant를 사용하여 앱 SQL Server 데이터베이스를 평가합니다. | 사용 가능
[문서 4: Azure VM 및 SQL Database Managed Instance에서 앱 다시 호스트](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso가 온-프레미스 SmartHotel360 앱을 Azure로 리프트 앤 시프트 방식으로 마이그레이션합니다. Contoso에서 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)를 사용하여 앱 프런트 엔드 VM을 마이그레이션하고, [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview)를 사용하여 앱 데이터베이스를 Azure SQL Database Managed Instance로 마이그레이션합니다. | 사용 가능   
[문서 5: Azure VM에서 앱 다시 호스트](contoso-migration-rehost-vm.md) | Contoso에서 Site Recovery 서비스를 사용하여 SmartHotel360 앱 VM을 Azure VM으로 마이그레이션합니다. | 사용 가능
[문서 6: Azure VM 및 SQL Server AlwaysOn 가용성 그룹에서 앱 다시 호스트](contoso-migration-rehost-vm-sql-ag.md) | Contoso가 SmartHotel360 앱을 마이그레이션합니다. Contoso에서 Site Recovery를 사용하여 앱 VM을 마이그레이션하고, Database Migration Service를 사용하여 앱 데이터베이스를 AlwaysOn 가용성 그룹으로 보호되는 SQL Server 클러스터로 마이그레이션합니다. | 사용 가능 
[문서 7: Azure VM에서 Linux 앱 다시 호스트](contoso-migration-rehost-linux-vm.md) | Contoso에서 Azure Site Recovery를 사용하여 Linux osTicket 앱을 Azure VM으로 리프트 앤 시프트 방식으로 마이그레이션합니다. | 사용 가능
문서 8: Linux 앱을 Azure VM 및 Azure MySQL에 다시 호스트 | Contoso에서 Azure Site Recovery를 사용하여 Linux osTicket 앱을 Azure VM으로 마이그레이션하고, MySQL Workbench를 사용하여 앱 데이터베이스를 Azure MySQL 서버 인스턴스로 마이그레이션합니다. | 이 문서의 내용
[문서 9: Azure Web Apps 및 Azure SQL Database에서 앱 리팩터링](contoso-migration-refactor-web-app-sql.md) | Contoso에서 SmartHotel360 앱을 Azure Web App으로 마이그레이션하고, Database Migration Assistant를 사용하여 앱 데이터베이스를 Azure SQL Server 인스턴스로 마이그레이션합니다. | 사용 가능
[문서 10: Azure Web Apps 및 Azure MySQL에서 Linux 앱 리팩터링](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso에서 지속적인 업데이트를 위해 GitHub와 통합된 Azure Traffic Manager를 사용하여 Linux osTicket 앱을 여러 Azure 지역의 Azure 웹앱으로 마이그레이션합니다. Contoso에서 앱 데이터베이스를 Azure Database for MySQL 인스턴스로 마이그레이션합니다. | 사용 가능 
[문서 11: Azure DevOps Services에서 TFS 리팩터링](contoso-migration-tfs-vsts.md) | Contoso에서 온-프레미스 Team Foundation Server 배포를 Azure의 Azure DevOps Services로 마이그레이션합니다. | 사용 가능
[문서 12: Azure 컨테이너 및 Azure SQL Database에서 앱 재설계](contoso-migration-rearchitect-container-sql.md) | Contoso에서 SmartHotel 앱을 Azure로 마이그레이션합니다. 그런 다음, 웹앱 계층을 Azure Service Fabric에서 실행되는 Windows 컨테이너로 재설계하고 Azure SQL Database를 사용하여 데이터베이스를 재설계합니다. | 사용 가능
[문서 13: Azure에서 앱 다시 빌드](contoso-migration-rebuild.md) | Contoso에서 다양한 Azure 기능과 서비스(Azure App Service, AKS(Azure Kubernetes Service), Azure Functions, Azure Cognitive Services 및 Azure Cosmos DB 포함)를 사용하여 SmartHotel 앱을 다시 빌드합니다. | 사용 가능
[문서 14: Azure로의 마이그레이션 확장](contoso-migration-scale.md) | 마이그레이션 조합을 시도한 후 Contoso는 Azure로 전체 마이그레이션을 확장할 준비를 합니다. | 사용 가능


이 문서에서 Contoso는 2계층 Linux Apache MySQL PHP(LAMP) 서비스 데스크 앱(osTicket)을 Azure로 마이그레이션합니다. 이 오픈 소스 앱을 사용하려면 [GitHub](https://github.com/osTicket/osTicket)에서 다운로드할 수 있습니다.



## <a name="business-drivers"></a>비즈니스 영향 요소

IT 리더십 팀은 비즈니스 파트너와의 긴밀한 협력을 통해 다음과 같이 기업이 달성하고자 하는 바를 잘 이해하고 있습니다.

- **비즈니스 성장 해결**: Contoso는 성장하고 있으며, 이에 따라 온-프레미스 시스템과 인프라에 부담을 주고 있습니다.
- **위험 제한**: 서비스 데스크 앱은 비즈니스에 매우 중요합니다. Contoso는 이 앱을 안전하게 Azure로 이동하려고 합니다.
- **확장**:  Contoso는 지금 앱을 변경하지 않고 앱을 안정적으로 유지하려고 합니다.


## <a name="migration-goals"></a>마이그레이션 목표

Contoso 클라우드 팀은 최상의 마이그레이션 방법을 확인하기 위해 이 마이그레이션의 목표를 정했습니다.

- 마이그레이션 후 Azure의 앱에는 현재 온-프레미스 VMware 환경에서 수행되는 것과 동일한 성능 기능이 있어야 합니다.  앱은 온-프레미스에서와 마찬가지로 클라우드에서도 매우 중요하게 유지됩니다. 
- Contoso는 이 앱에 투자하려고 하지 않습니다.  비즈니스에 중요하기는 하지만 Contoso는 단지 현재 형태로 안전하게 클라우드로 이동하고 싶어 합니다.
- 몇 가지 Windows 앱 마이그레이션을 완료한 후 Contoso는 Azure에서 Linux 기반 인프라를 사용하는 방법을 알고 싶어 합니다.
- Contoso는 애플리케이션이 클라우드로 이동된 후 데이터베이스 관리 작업을 최소화하려고 합니다.

## <a name="proposed-architecture"></a>제안된 아키텍처

이 시나리오에서는

- 앱이 두 개의 VM인 OSTICKETWEB 및 OSTICKETMYSQL에 계층화되어 있습니다.
- VM은 VMware ESXi 호스트 **contosohost1.contoso.com**(버전 6.5)에 있습니다.
- VMware 환경은 VM에서 실행되는 vCenter Server 6.5(**vcenter.contoso.com**)에서 관리합니다.
- Contoso는 온-프레미스 데이터 센터(contoso-datacenter)와 온-프레미스 도메인 컨트롤러(**contosodc1**)를 갖고 있습니다.
- OSTICKETWEB의 웹 계층 앱은 Azure IaaS VM으로 마이그레이션됩니다.
- 앱 데이터베이스는 Azure Database for MySQL PaaS 서비스로 마이그레이션됩니다.
- Contoso는 프로덕션 워크로드를 마이그레이션하므로 리소스는 프로덕션 리소스 그룹 **ContosoRG**에 상주합니다.
- 리소스는 주 지역(미국 동부 2)에 복제되고 프로덕션 네트워크(VNET-PROD-EUS2)에 배치됩니다.
    - 웹 VM은 프런트 엔드 서브넷(PROD-FE-EUS2)에 상주합니다.
    - 데이터베이스 인스턴스는 데이터베이스 서브넷(PROD-DB-EUS2)에 상주합니다.
- 앱 데이터베이스는 MySQL 도구를 사용하여 Azure MySQL로 마이그레이션됩니다.
- 마이그레이션이 완료되면 Contoso 데이터 센터의 온-프레미스 VM은 서비스 해제됩니다.


![시나리오 아키텍처](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>마이그레이션 프로세스

Contoso는 다음과 같이 마이그레이션 프로세스를 완료합니다.

웹 VM을 마이그레이션하려면

1. 첫 번째 단계로, Contoso는 Site Recovery를 배포하는 데 필요한 온-프레미스 인프라 및 Azure를 설정합니다.
2. Azure와 온-프레미스 구성 요소를 준비한 후 Contoso는 웹 VM에 대해 복제를 설정하고 사용하도록 설정합니다.
3. 복제가 작동하여 실행되면 Contoso는 Azure로 장애 조치(failover)하여 VM을 마이그레이션합니다.

데이터베이스를 마이그레이션하려면

1. Contoso는 Azure에 MySQL 인스턴스를 프로비전합니다.
2. Contoso는 MySQL 워크벤치를 설정하고, 데이터베이스를 로컬에 백업합니다.
3. 그런 다음, Contoso는 로컬 백업의 데이터베이스를 Azure로 복원합니다.

![마이그레이션 프로세스](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure 서비스

**서비스** | **설명** | **비용**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | 서비스는 Azure VM, 온-프레미스 VM 및 물리적 서버에 대한 마이그레이션 및 재해 복구를 오케스트레이션하고 관리합니다.  | Azure로 복제하는 동안 Azure Storage 비용이 청구됩니다.  장애 조치(failover) 발생 시 Azure VM이 생성되고, 요금이 발생합니다. 요금 및 가격 책정에 대해 [자세히 알아보세요](https://azure.microsoft.com/pricing/details/site-recovery/).
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | 데이터베이스는 오픈 소스 MySQL 서버 엔진을 기반으로 합니다. 완전히 관리되는 엔터프라이즈용 커뮤니티 MySQL 데이터베이스를 앱 개발 및 배포용 서비스로 제공합니다. 

 
## <a name="prerequisites"></a>필수 조건

이 시나리오를 위해 Contoso에 필요한 항목은 다음과 같습니다.

**요구 사항** | **세부 정보**
--- | ---
**Azure 구독** | Contoso는 이전 문서에서 구독을 만들었습니다. Azure 구독이 아직 없는 경우 [체험 계정](https://azure.microsoft.com/pricing/free-trial/)을 만듭니다.<br/><br/> 체험 계정을 만들면 구독 관리자로서 모든 작업을 수행할 수 있습니다.<br/><br/> 기존 구독을 사용하고 관리자가 아닌 경우 관리자와 협력하여 소유자 또는 기여자 권한을 할당받아야 합니다.<br/><br/> 보다 세부적인 권한이 필요한 경우 [이 문서](../site-recovery/site-recovery-role-based-linked-access-control.md)를 검토합니다. 
**Azure 인프라** | Contoso는 [마이그레이션을 위한 Azure 인프라](contoso-migration-infrastructure.md)에 설명된 대로 Azure 인프라를 설정합니다.<br/><br/> Site Recovery 고유의 [네트워크](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) 및 [저장소](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) 요구 사항에 대해 자세히 알아보세요.
**온-프레미스 서버** | 온-프레미스 vCenter 서버에서 5.5, 6.0 또는 6.5 버전을 실행해야 합니다.<br/><br/> 5.5, 6.0 또는 6.5 버전을 실행하는 ESXi 호스트<br/><br/> ESXi 호스트에서 실행되는 하나 이상의 VMware VM
**온-프레미스 VM** | Site Recovery를 사용한 마이그레이션에 대해 지원되는 [Linux VM 요구 사항을 검토](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines)합니다.<br/><br/> 지원되는 [Linux 파일 및 저장소 시스템](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage)을 확인합니다.<br/><br/> VM은 [Azure 요구 사항](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements)을 충족해야 합니다.


## <a name="scenario-steps"></a>시나리오 단계

Contoso 관리자에서 마이그레이션을 수행하는 방법은 다음과 같습니다.

> [!div class="checklist"]
> * **1단계: Site Recovery를 위한 Azure 준비**: 복제된 데이터를 저장할 Azure Storage 계정 및 Recovery Services 자격 증명 모음을 만듭니다.
> * **2단계: Site Recovery를 위한 온-프레미스 VMware 준비**: VM 검색 및 에이전트 설치를 위한 계정과 장애 조치 후에 Azure VM에 연결하기 위한 준비를 합니다.
>   * **3단계: 데이터베이스를 프로비전]**: Azure에서 Azure MySQL 데이터베이스 인스턴스를 프로비전합니다.
> * **4단계: VM 복제**: Site Recovery 원본 및 대상 환경을 구성하고, 복제 정책을 설정하고, Azure Storage로 VM 복제를 시작합니다.
> * **5단계: 데이터베이스 마이그레이션**: MySQL 도구를 사용하여 마이그레이션을 설정합니다.
> * **6단계: Site Recovery를 사용하여 VM 마이그레이션**: 마지막으로 테스트 장애 조치(failover)를 실행하여 모든 항목이 작동하는지 확인한 다음, 전체 장애 조치(failover)를 실행하여 VM을 Azure로 마이그레이션합니다.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1단계: Site Recovery 서비스를 위한 Azure 준비

Contoso가 Site Recovery를 수행하려면 몇 가지 Azure 구성 요소가 필요합니다.

- 장애 조치된 리소스가 있는 VNet Contoso는 [Azure 인프라 배포](contoso-migration-infrastructure.md) 중 VNet을 이미 만들었습니다.
- 복제된 데이터를 저장할 새 Azure Storage 계정. 
- Azure의 Recovery Services 자격 증명 모음

Contoso 관리자는 다음과 같이 저장소 계정 및 자격 증명 모음을 만듭니다.

1. 미국 동부 2 지역에 저장소 계정(**contosovmsacc20180528**)을 만듭니다.

   - 저장소 계정은 Recovery Services 자격 증명 모음과 동일한 영역에 있어야 합니다.
   - 범용 계정과 표준 저장소 및 LRS 복제를 사용합니다.

     ![Site Recovery 저장소](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. 네트워크 및 저장소 계정이 준비되면 자격 증명 모음(ContosoMigrationVault)을 만들어 주 지역인 미국 동부 2 지역의 **ContosoFailoverRG** 리소스 그룹에 배치합니다.

    ![Recovery Services 자격 증명 모음](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**도움이 더 필요하세요?**

Azure에서 Site Recovery를 설정하는 방법에 대해 [알아보세요](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure).


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2단계: Site Recovery를 위한 온-프레미스 VMware 준비

Contoso 관리자는 다음과 같이 온-프레미스 VMware 인프라를 준비합니다.

- VM 검색을 자동화하기 위해 vCenter 서버에 계정을 만듭니다.
- 복제될 VMware VM에서 모바일 서비스 자동 설치를 허용하는 계정을 만듭니다.
- 마이그레이션 이후 온-프레미스 VM이 생성될 때 Azure VM에 연결할 수 있도록 준비합니다.


### <a name="prepare-an-account-for-automatic-discovery"></a>자동 검색용 계정 준비

Site Recovery에서는 다음 작업을 위해 VMware 서버에 액세스해야 합니다.

- VM을 자동으로 검색합니다. 최소한 읽기 전용 계정이 필요합니다.
- 복제, 장애 조치 및 장애 복구를 오케스트레이션합니다. 디스크 만들기 및 제거, VM 전원 켜기와 같은 작업을 실행할 수 있는 계정이 필요합니다.

Contoso 관리자는 다음과 같이 계정을 설정합니다.

1. vCenter 수준에서 역할을 만듭니다.
2. 그런 다음, 해당 역할에 필요한 권한을 할당합니다.


### <a name="prepare-an-account-for-mobility-service-installation"></a>모바일 서비스 설치를 위한 계정 준비

Contoso가 마이그레이션하려는 각 VM에 모바일 서비스가 설치되어 있어야 합니다.

- VM에 대해 복제를 사용하도록 설정하면 Site Recovery에서 이 구성 요소를 자동으로 강제 설치할 수 있습니다.
- 자동 설치를 위해 Site Recovery에 VM에 대한 액세스 권한이 있는 계정이 필요합니다. 
- 계정 정보는 복제 설정 중에 입력됩니다. 
- 설치 권한만 있으면 계정은 도메인 또는 로컬 계정일 수 있습니다.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>장애 조치(Failover) 후 Azure VM에 연결할 준비

Contoso는 Azure로 장애 조치(failover) 이후 Azure VM에 연결하고 싶어 합니다. 이를 위해 Contoso 관리자는 다음을 수행해야 합니다.

- 인터넷을 통해 액세스하려면 마이그레이션 전에 온-프레미스 Linux VM에서 SSH를 사용하도록 설정합니다.  Ubuntu의 경우 다음 명령을 사용하여 이 작업을 완료할 수 있습니다. **Sudo apt-get ssh install -y**.
- 장애 조치(failover) 이후 **부트 진단**을 검사하여 VM 스크린샷을 확인해야 합니다.
- 그래도 해결되지 않으면 VM이 실행 중인지 확인하고 이러한 [문제 해결 팁](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)를 검토해야 합니다.

**도움이 더 필요하세요?**

- 자동 검색을 위한 역할 만들기와 할당에 대해 [알아보세요](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery).
- 모바일 서비스 강제 설치를 위한 계정을 만드는 방법에 대해 [알아보세요](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation).


## <a name="step-3-provision-azure-database-for-mysql"></a>3단계: Azure Database for MySQL 프로비전

Contoso 관리자는 주 지역인 미국 동부 2 지역에 MySQL 데이터베이스 인스턴스를 프로비전합니다.

1. Azure Portal에서 Azure Database for MySQL 리소스를 만듭니다. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Azure 데이터베이스에 대해 **contosoosticket** 이름을 추가합니다. 프로덕션 리소스 그룹 **ContosoRG**에 데이터베이스를 추가하고 해당 자격 증명을 지정합니다.
3. 온-프레미스 MySQL 데이터베이스가 버전 5.7이므로 호환성을 위해 이 버전을 선택합니다. 데이터베이스 요구 사항과 일치하는 기본 크기를 사용합니다.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. **백업 중복성 옵션**에서 **지역 중복**을 사용하도록 선택합니다. 이 옵션을 사용하면 가동이 중단될 경우 보조 지역인 미국 중부 지역의 데이터베이스를 복원할 수 있습니다. 데이터베이스를 프로비전하는 경우에만 이 옵션을 구성할 수 있습니다.

     ![중복](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. **VNET-PROD-EUS2** 네트워크 &gt; **서비스 엔드포인트**에서 SQL 서비스에 대한 서비스 엔드포인트(데이터베이스 서브넷)를 추가합니다.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. 서브넷을 추가한 후 프로덕션 네트워크의 데이터베이스 서브넷에서 액세스할 수 있도록 허용하는 가상 네트워크 규칙을 만듭니다.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>4단계: 온-프레미스 VM 복제

웹 VM을 Azure로 마이그레이션하기 위해 Contoso 관리자는 먼저 복제를 설정하고 사용하도록 설정합니다.

### <a name="set-a-protection-goal"></a>보호 목표 설정

1. 자격 증명 모음의 자격 증명 모음 이름(ContosoVMVault) 아래에서 복제 목표를 설정합니다(**시작** > **Site Recovery** > **인프라 준비**).
2. 자체 컴퓨터가 온-프레미스에 있고, VMware VM이며, Azure로 복제하려고 한다고 지정합니다.

    ![복제 목표](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>배포 계획 확인

계속하려면 **예, 완료함**을 선택하여 배포 계획을 완료했음을 확인합니다. 이 시나리오에서는 Contoso가 단일 VM만 마이그레이션하므로 배포 계획이 필요하지 않습니다.

### <a name="set-up-the-source-environment"></a>원본 환경 설정

이제 Contoso 관리자는 원본 환경을 구성합니다. 이 작업을 위해 OVF 템플릿을 사용하여 Site Recovery 구성 서버를 고가용성 온-프레미스 VMware VM으로 배포합니다. 구성 서버가 작동하여 실행되면 자격 증명 모음에 등록합니다.

구성 서버는 다음과 같이 여러 구성 요소를 실행합니다.

- 구성 서버 구성 요소 - 온-프레미스와 Azure 간의 통신을 조정하고 데이터 복제를 관리합니다.
- 프로세스 서버 - 복제 게이트웨이의 역할을 합니다. 복제 데이터를 수신하고 캐싱, 압축 및 암호화를 사용하여 최적화하며 복제 데이터를 Azure 저장소로 전송합니다.
- 또한 프로세스 서버는 복제하려는 VM에 모바일 서비스를 설치하고 온-프레미스 VMware VM의 자동 검색을 수행합니다.

Contoso 관리자는 다음과 같이 이를 수행합니다.


1. **인프라 준비** > **원본** > **구성 서버**에서 OVF 템플릿을 다운로드합니다.
    
    ![OVF 다운로드](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. 템플릿을 VMware로 가져와서 VM을 만들고 VM을 배포합니다.

    ![OVF 템플릿](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. VM을 처음 작동하면 Windows Server 2016 설치 환경으로 부팅됩니다. 사용권 계약에 동의하고 관리자 암호를 입력합니다.
4. 설치가 완료되면 VM에 관리자 권한으로 로그인합니다. 처음 로그인하면 Azure Site Recovery 구성 도구가 기본적으로 실행됩니다.
5. 도구에서 자격 증명 모음에 구성 서버를 등록하는 데 사용할 이름을 지정합니다.
6. VM이 Azure에 연결할 수 있는지 도구에서 확인합니다.
7. 연결이 설정되면 Azure 구독에 로그인합니다. 구성 서버를 등록할 자격 증명 모음에 자격 증명이 액세스할 수 있어야 합니다.

    ![구성 서버 등록](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. 도구에서 몇 가지 구성 작업을 수행한 후 다시 부팅합니다.
9. 머신에 다시 로그인하면 구성 서버 관리 마법사가 자동으로 시작됩니다.
10. 마법사에서 복제 트래픽을 받을 NIC를 선택합니다. 이 설정은 구성된 후에 변경할 수 없습니다.
11. 구성 서버를 등록할 구독, 리소스 그룹 및 자격 증명 모음을 선택합니다.

    ![자격 증명 모음](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. 이제 MySQL 서버 및 VMWare PowerCLI를 다운로드하여 설치합니다. 
13. 유효성 검사 이후 vCenter 서버나 vSphere 호스트의 FQDN 또는 IP 주소를 지정합니다. 기본 포트는 그대로 두고 vCenter 서버의 이름을 지정합니다.
14. 자동 검색을 위해 만든 계정과 Site Recovery에서 모바일 서비스를 자동으로 설치하는 데 사용되는 자격 증명을 입력합니다. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. 등록이 완료되면 Azure Portal에서 구성 서버 및 VMware 서버가 자격 증명 모음의 **원본** 페이지에 표시되는지 확인합니다. 검색하는 데 15분 이상 걸릴 수 있습니다. 
15. 모두 제대로 표시되면 Site Recovery가 VMware 서버에 연결하고 VM을 검색합니다.

### <a name="set-up-the-target"></a>대상 설정

이제 Contoso 관리자가 대상 복제 설정을 입력합니다.

1. **인프라 준비** > **대상**에서 대상 설정을 선택합니다.
2. Site Recovery에서 지정된 대상에 Azure 저장소 계정과 네트워크가 있는지 확인합니다.


### <a name="create-a-replication-policy"></a>복제 정책 만들기

원본 및 대상이 설정되면 Contoso 관리자에서 복제 정책을 만들 준비가 된 것입니다.

1. **인프라 준비** > **복제 설정** > **복제 정책** >  **만들기 및 연결**에서 **ContosoMigrationPolicy** 정책을 만듭니다.
2. 다음 기본 설정을 사용합니다.
    - **RPO 임계값**: 기본값은 60분입니다. 이 값은 복구 지점을 만드는 빈도를 지정합니다. 연속 복제가 이 제한을 초과하면 경고가 생성됩니다.
    - **복구 지점 보존**: 기본값은 24시간입니다. 이 값은 각 복구 지점에 대한 보존 기간을 지정합니다. 복제된 VM은 하나의 시간대에서 임의의 시점으로 복구할 수 있습니다.
    - **앱 일치 스냅숏 빈도**: 기본값은 1시간입니다. 이 값은 애플리케이션 일치 스냅숏이 만들어지는 빈도를 지정합니다.
 
        ![복제 정책 만들기](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. 정책은 구성 서버와 자동으로 연결됩니다. 

    ![복제 정책 연결](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**도움이 더 필요하세요?**

- [온-프레미스 VMware VM 재해 복구 설정](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial)에서 이러한 모든 단계에 대한 전체 연습을 확인할 수 있습니다.
- [원본 환경 설정](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [구성 서버 배포](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), [복제 설정 구성](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication)에 도움이 되는 자세한 지침이 제공됩니다.
- Linux용 Azure 게스트 에이전트에 대해 [자세히 알아보세요](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux).

### <a name="enable-replication-for-the-web-vm"></a>웹 VM에 대해 복제 사용

이제 Contoso 관리자가 **OSTICKETWEB** VM의 복제를 시작할 수 있습니다.

1. **애플리케이션 복제** > **원본** > **+복제**에서 원본 설정을 선택합니다.
2. 가상 머신을 사용하도록 설정하고 vCenter 서버 및 구성 서버를 비롯한 원본 설정을 선택합니다.

    ![복제 사용](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. 이제 대상 설정을 지정합니다. 여기에는 장애 조치(failover) 이후 Azure VM이 배치되는 네트워크 및 리소스 그룹과 복제된 데이터가 저장되는 저장소 계정이 포함됩니다. 

     ![복제 사용](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. 복제를 위해 **OSTICKETWEB**을 선택합니다. 

    ![복제 사용](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. VM 속성에서 VM에 모바일 서비스를 자동으로 설치하는 데 사용할 계정을 선택합니다.

     ![모바일 서비스](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. **복제 설정** > **복제 설정 구성**에서 올바른 복제 정책이 적용되었는지 확인하고 **복제 사용**을 선택합니다. 모바일 서비스가 자동으로 설치됩니다.
6.  **작업**에서 복제 진행 상황을 추적합니다. **보호 완료** 작업이 실행된 후에는 컴퓨터가 장애 조치(failover)를 수행할 준비가 되어 있습니다.


**도움이 더 필요하세요?**

[복제를 사용하도록 설정](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication)에서 이러한 모든 단계의 전체 연습을 확인할 수 있습니다.


## <a name="step-5-migrate-the-database"></a>5단계: 데이터베이스 마이그레이션

Contoso 관리자는 MySQL 도구를 통해 백업 및 복원을 사용하여 데이터베이스를 마이그레이션합니다. MySQL Workbench를 설치하고, OSTICKETMYSQL에서 데이터베이스를 백업한 다음, Azure Database for MySQL 서버에 복원합니다.

### <a name="install-mysql-workbench"></a>MySQL Workbench 설치

1. [필수 조건을 확인하고 MySQL Workbench를 다운로드](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool)합니다.
2. [설치 지침](https://dev.mysql.com/doc/workbench/en/wb-installing.html)에 따라 Windows용 MySQL Workbench를 설치합니다.
3. MySQL Workbench에서 OSTICKETMYSQL에 대한 MySQL 연결을 만듭니다. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. 데이터베이스를 **osticket**으로 로컬 자체 포함 파일에 내보냅니다.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. 데이터베이스가 로컬에 백업된 후 Azure Database for MySQL 인스턴스에 대한 연결을 만듭니다.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. 이제 자체 포함 파일에서 Azure MySQL 인스턴스의 데이터베이스를 가져올(복원) 수 있습니다. 인스턴스에 대해 새 스키마(osticket)가 생성됩니다.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>6단계: Site Recovery를 사용하여 VM 마이그레이션

마지막으로 Contoso 관리자는 빠른 테스트 장애 조치를 실행한 다음, VM을 마이그레이션합니다.

### <a name="run-a-test-failover"></a>테스트 장애 조치(failover) 실행

테스트 장애 조치(failover)를 실행하면 마이그레이션 전에 모든 요소가 예상대로 작동하는지 확인하는 데 도움이 됩니다. 

1. 사용할 수 있는 최근 특정 시점(**가장 최근에 처리됨**)으로 테스트 장애 조치를 실행합니다.
2. 장애 조치를 트리거하기 전에 Site Recovery에서 원본 VM 종료를 시도하도록 **장애 조치를 시작하기 전에 머신 종료**를 선택합니다. 종료가 실패하더라도 장애 조치는 계속됩니다. 
3. 테스트 장애 조치가 다음과 같이 실행됩니다. 

    - 필수 구성 요소 확인은 마이그레이션에 필요한 모든 조건이 충족되었는지 확인하기 위해 실행합니다.
    - 장애 조치(failover)는 데이터를 처리하며 이 데이터에서 Azure VM이 만들어질 수 있습니다. 최신 복구 지점을 선택하는 경우 해당 데이터에서 복구 지점이 만들어집니다.
    - 이전 단계에서 처리한 데이터를 사용하여 Azure VM이 만들어집니다.

3. 장애 조치가 완료되면 Azure Portal에 Azure VM 복제본이 표시됩니다. VM의 크기가 적당하고, VM이 올바른 네트워크에 연결되어 있으며 실행 중인지 확인합니다. 
4. 확인한 후 장애 조치(failover)를 정리하고 모든 관찰 내용을 기록 및 저장합니다.

### <a name="migrate-the-vm"></a>VM 마이그레이션

VM을 마이그레이션하기 위해 Contoso 관리자는 VM을 포함하는 복구 계획을 만들고 계획을 Azure로 장애 조치(failover)합니다.

1. 계획을 만들고, **OSTICKETWEB**을 추가합니다.

    ![복구 계획](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. 계획에 대해 장애 조치(failover)를 실행합니다. 최근 복구 지점을 선택하고 Site Recovery가 장애 조치(failover)를 트리거하기 전에 온-프레미스 VM을 종료하도록 지정합니다. **작업** 페이지에서 장애 조치(failover) 진행률을 확인할 수 있습니다.

    ![장애 조치(failover)](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. 장애 조치(failover) 중에 vCenter Server는 명령을 실행하여 ESXi 호스트에서 실행 중인 두 개의 VM을 중지합니다.

    ![장애 조치(failover)](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. 장애 조치(failover) 후 Azure Portal에 예상대로 Azure VM이 표시되는지 확인합니다.

    ![장애 조치(failover)](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. VM을 확인한 후 마이그레이션을 완료합니다. 그러면 VM에 대한 복제가 중지되고 VM에 대한 Site Recovery 청구가 중지됩니다.

    ![장애 조치(failover)](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**도움이 더 필요하세요?**

- 테스트 장애 조치를 실행하는 방법에 대해 [알아보세요](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure). 
- 복구 계획을 만드는 방법에 대해 [알아보세요](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).
- Azure로 장애 조치(failover)하는 방법을 [알아보세요](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover).


### <a name="connect-the-vm-to-the-database"></a>데이터베이스에 VM 연결

마이그레이션 프로세스의 최종 단계로, Contoso 관리자는 Azure Database for MySQL을 가리키도록 응용 프로그램의 연결 문자열을 업데이트합니다. 

1. Putty 또는 다른 SSH 클라이언트를 사용하여 OSTICKETWEB VM에 대한 SSH 연결을 만듭니다. VM은 개인 VM이므로 개인 IP 주소를 사용하여 연결합니다.

    ![데이터베이스에 연결](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![데이터베이스에 연결](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. **OSTICKETWEB** VM이 **OSTICKETMYSQL** 데이터베이스와 통신할 수 있도록 설정을 업데이트합니다. 현재 구성은 온-프레미스 IP 주소 172.16.0.43으로 하드 코드되어 있습니다.

    **업데이트 이전**
    
    ![IP 업데이트](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **업데이트 이후**
    
    ![IP 업데이트](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![IP 업데이트](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. **systemctl restart apache2**를 사용하여 서비스를 다시 시작합니다.

    ![다시 시작](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. 마지막으로, Contoso 도메인 컨트롤러 중 하나에서 **OSTICKETWEB**의 DNS 레코드를 업데이트합니다.

    ![DNS 업데이트](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>마이그레이션 후 정리

마이그레이션이 완료되면 osTicket 앱 계층이 Azure VM에서 실행됩니다.

이제 Contoso는 다음과 같은 작업을 수행해야 합니다. 
- vCenter 인벤토리에서 VMware VM을 제거합니다.
- 로컬 백업 작업에서 온-프레미스 VM을 제거합니다.
- 업데이트 내부 문서에 새 위치 및 IP 주소가 표시됩니다. 
- 온-프레미스 VM과 상호 작용하는 모든 리소스를 검토하고 모든 관련 설정이나 문서를 업데이트하여 새 구성을 반영합니다.
- Contoso는 Azure Migrate 서비스를 종속성 매핑과 함께 사용하여 마이그레이션할 **OSTICKETWEB** VM을 평가했습니다. 이제 이 용도로 설치한 에이전트(Microsoft Monitoring Agent/Dependency Agent)를 VM에서 제거해야 합니다.

## <a name="review-the-deployment"></a>배포 검토

앱이 현재 실행되고 있으므로 Contoso는 새 인프라를 완전히 운용하고 보안을 유지해야 합니다.

### <a name="security"></a>보안

Contoso 보안 팀은 VM과 데이터베이스를 검토하여 보안 문제를 확인합니다.

- 액세스를 제어할 VM에 대한 NSG(네트워크 보안 그룹)를 검토합니다. NSG는 애플리케이션에 허용된 트래픽만 통과할 수 있도록 제한하는 데 사용됩니다.
- 디스크 암호화 및 Azure KeyVault를 사용하여 VM 디스크의 데이터 보안을 유지하는 것도 고려합니다.
- VM과 데이터베이스 인스턴스 간의 통신에 SSL이 구성되어 있지 않습니다. 데이터베이스 트래픽을 해킹할 수 없도록 하려면 이 작업을 수행해야 합니다.

VM 보안 모범 사례에 대해 [자세히 알아보세요](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms).

### <a name="bcdr"></a>BCDR

비즈니스 연속성 및 재해 복구를 위해 Contoso는 다음 작업을 수행합니다.

- **데이터를 안전하게 유지**: Contoso는 Azure Backup 서비스를 사용하여 앱 VM의 데이터를 백업합니다. [자세히 알아보기](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 데이터베이스에 대한 백업을 구성할 필요가 없습니다. Azure Database for MySQL에서 자동으로 서버 백업을 만들고 저장합니다. 데이터베이스에 대해 지리적 중복을 사용하도록 선택했으므로 복원력이 높고 프로덕션 준비가 완료되었습니다.
- **앱 가동 및 실행 유지**: Contoso는 Site Recovery를 사용하여 Azure의 앱 VM을 보조 지역에 복제합니다. [자세히 알아보기](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).


### <a name="licensing-and-cost-optimization"></a>라이선스 및 비용 최적화

- 리소스를 배포한 후 Contoso는 [Azure 인프라](contoso-migration-infrastructure.md#set-up-tagging) 배포 중 결정한 사항에 따라 Azure 태그를 할당합니다.
- Contoso Ubuntu 서버에 대한 라이선스 문제는 없습니다.
- Contoso는 Microsoft의 자회사인 Cloudyn에서 라이선스를 부여하는 Azure Cost Management를 사용하도록 설정할 것입니다. Azure 및 기타 클라우드 리소스를 활용하고 관리하는 데 도움이 되는 다중 클라우드 비용 관리 솔루션입니다.  Azure Cost Management에 대해 [자세히 알아보세요](https://docs.microsoft.com/azure/cost-management/overview).


## <a name="next-steps"></a>다음 단계

이 시나리오에서는 최종 다시 호스트 시나리오를 보여주었습니다. Contoso는 온-프레미스 Linux osTicket 앱의 프런트 엔드 VM을 Azure VM으로 마이그레이션했으며, 앱 데이터베이스를 Azure MySQL 인스턴스로 마이그레이션했습니다.

마이그레이션 시리즈의 다음 자습서 집합에서는 Contoso가 간단한 리프트 앤 시프트 마이그레이션이 아니라 앱 리팩터링을 포함하는 복잡한 일련의 마이그레이션을 수행한 방법을 보여 드리겠습니다.
