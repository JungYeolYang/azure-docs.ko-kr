---
title: Azure Resource Manager 템플릿을 사용하여 가상 머신 확장 배포 | Microsoft Docs
description: Azure Resource Manager 템플릿을 사용하여 가상 머신 확장을 배포하는 방법 알아보기
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 5657ebb2a5b29e4ec5360480c1fef6cb92dad9c8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60388536"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>자습서: Azure Resource Manager 템플릿을 사용하여 가상 머신 확장 배포

[Azure 가상 머신 확장](../virtual-machines/extensions/features-windows.md)을 사용하여 Azure VM에서 배포 후 구성 및 자동화 작업을 수행하는 방법을 알아봅니다. Azure VM에서 여러 다양한 VM 확장을 사용할 수 있습니다. 이 자습서에서는 Azure Resource Manager 템플릿의 사용자 지정 스크립트 확장을 배포하여 Windows VM에서 PowerShell 스크립트를 실행합니다.  이 스크립트는 VM에 웹 서버를 설치합니다.

이 자습서에서 다루는 작업은 다음과 같습니다.

> [!div class="checklist"]
> * PowerShell 스크립트 준비
> * 빠른 시작 템플릿 열기
> * 템플릿 편집
> * 템플릿 배포
> * 배포 확인

Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

이 문서를 완료하려면 다음이 필요합니다.

* Resource Manager Tools 확장이 있는 [Visual Studio Code](https://code.visualstudio.com/)  [확장 설치](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)를 참조하세요.
* 보안을 강화하려면 가상 머신 관리자 계정에 생성된 암호를 사용합니다. 암호를 생성하는 방법에 대한 샘플은 다음과 같습니다.

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault는 암호화 키 및 기타 비밀을 보호하기 위한 것입니다. 자세한 내용은 [자습서: Resource Manager 템플릿 배포에 Azure Key Vault 통합](./resource-manager-tutorial-use-key-vault.md)을 참조하세요. 또한 3개월마다 암호를 업데이트하는 것도 좋습니다.

## <a name="prepare-a-powershell-script"></a>PowerShell 스크립트 준비

다음 콘텐츠가 포함된 PowerShell 스크립트는 [공개적으로 액세스 가능한 Azure Storage 계정](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1)에서 공유됩니다.

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

사용자 고유의 위치에 파일을 게시하기로 선택하는 경우 자습서의 뒷부분에서 템플릿의 `fileUri` 요소를 업데이트해야 합니다.

## <a name="open-a-quickstart-template"></a>빠른 시작 템플릿 열기

Azure 빠른 시작 템플릿은 Resource Manager 템플릿용 리포지토리입니다. 템플릿을 처음부터 새로 만드는 대신 샘플 템플릿을 찾아서 사용자 지정할 수 있습니다. 이 자습서에 사용되는 템플릿의 이름은 [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/)입니다.

1. Visual Studio Code에서 **파일** > **파일 열기**를 차례로 선택합니다.
1. **파일 이름** 상자에서 다음 URL을 붙여넣습니다. https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json

1. 파일을 열려면 **열기**를 선택합니다.  
    템플릿은 5개의 리소스를 정의합니다.

   * **Microsoft.Storage/storageAccounts**. [템플릿 참조](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)를 참조하세요.
   * **Microsoft.Network/publicIPAddresses**. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)를 참조하세요.
   * **Microsoft.Network/virtualNetworks**. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)를 참조하세요.
   * **Microsoft.Network/networkInterfaces**. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)를 참조하세요.
   * **Microsoft.Compute/virtualMachines**. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)를 참조하세요.

     템플릿을 사용자 지정하기 전에 템플릿의 몇 가지 기본적인 내용을 이해하면 유용합니다.

1. **파일** > **다른 이름으로 저장**을 선택하여 파일 복사본을 로컬 컴퓨터에 *azuredeploy.json*이라는 이름으로 저장합니다.

## <a name="edit-the-template"></a>템플릿 편집

다음 콘텐츠를 사용하여 기존 템플릿에 가상 머신 확장 리소스를 추가합니다.

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

이 리소스 정의에 대한 자세한 내용은 [확장 참조](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions)를 참조하세요. 다음은 중요한 요소입니다.

* **name**: 확장 리소스는 가상 머신 개체의 자식 리소스이므로 이름에 가상 머신 이름 접두사가 있어야 합니다. [자식 리소스](./resource-group-authoring-templates.md#child-resources)를 참조하세요.
* **dependsOn**: 가상 머신을 만든 후 확장 리소스를 만듭니다.
* **fileUris**: 스크립트 파일이 저장되는 위치입니다. 제공된 위치를 사용하지 않으려면 값을 업데이트해야 합니다.
* **commandToExecute**: 이 명령은 스크립트를 호출합니다.  

## <a name="deploy-the-template"></a>템플릿 배포

배포 절차는 다음의 “템플릿 배포” 섹션을 참조하세요. [자습서: 종속 리소스가 있는 Azure Resource Manager 템플릿 만들기](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template)에서 사용한 대로 Cloud Shell 배포 방법을 사용합니다. 가상 머신 관리자 계정에 대해 생성된 암호를 사용하는 것이 좋습니다. 이 문서의 [필수 구성 요소](#prerequisites) 섹션을 참조하세요.

## <a name="verify-the-deployment"></a>배포 확인

1. Azure Portal에서 VM을 선택합니다.
1. VM 개요에서 **복사하려면 클릭**을 선택하여 IP 주소를 복사한 다음, 브라우저 탭에 붙여넣습니다.  
   기본 IIS(인터넷 정보 서비스) 시작 페이지가 열립니다.

![인터넷 정보 서비스 시작 페이지](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>리소스 정리

배포한 Azure 리소스가 더 이상 필요하지 않은 경우 리소스 그룹을 삭제하여 정리합니다.

1. Azure Portal의 왼쪽 메뉴에서 **리소스 그룹**을 선택합니다.
2. **이름으로 필터링** 상자에서 리소스 그룹 이름을 입력합니다.
3. 해당 리소스 그룹 이름을 선택합니다.  
    리소스 그룹에서 6개의 리소스가 표시됩니다.
4. 최상위 메뉴에서 **리소스 그룹 삭제**를 선택합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 가상 머신 및 가상 머신 확장을 배포했습니다. 이 확장은 가상 머신에 IIS 웹 서버를 설치했습니다. SQL Database 확장을 사용하여 BACPAC 파일을 가져오는 방법은 다음을 참조하세요.

> [!div class="nextstepaction"]
> [SQL 확장 배포](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)
