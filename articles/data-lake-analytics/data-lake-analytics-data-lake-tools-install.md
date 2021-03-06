---
title: Azure Data Lake Tools for Visual Studio 설치
description: 이 문서에서는 Azure Data Lake Tools for Visual Studio를 설치하는 방법을 알아봅니다.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 05/03/2018
ms.openlocfilehash: 3269d38b691ec4dd9573a241c89dadc285787143
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60408138"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio용 Data Lake 도구 설치

Visual Studio를 사용하여 Azure Data Lake Analytics 계정을 만들고, [U-SQL](data-lake-analytics-u-sql-get-started.md)로 작업을 정의하고, 작업을 Data Lake Analytics 서비스에 제출하는 방법에 대해 알아봅니다. 데이터 레이크 분석에 대한 자세한 내용은 [Azure 데이터 레이크 분석 개요](data-lake-analytics-overview.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

* **Visual Studio**: Express 제외한 모든 버전이 지원 됩니다.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **.NET용 Microsoft Azure SDK** 버전 2.7.1 이상.  [웹 플랫폼 설치 관리자](https://www.microsoft.com/web/downloads/platform.aspx)를 사용하여 설치합니다.
* **Data Lake Analytics** 계정. 계정을 만들려면 [Azure Portal을 사용하여 Azure Data Lake Analytics 시작](data-lake-analytics-get-started-portal.md)을 참조하세요.

## <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Azure Data Lake Tools for Visual Studio 2017 설치

Azure Data Lake Tools for Visual Studio는 Visual Studio 2017 15.3 이상에서 지원됩니다. 도구는 Visual Studio 설치 관리자에서 **데이터 저장소 및 처리** 및 **Azure 개발** 워크로드의 일부입니다. Visual Studio 설치의 일부로 이러한 두 워크로드 중 하나를 사용하도록 설정합니다.  

다음과 같이 **데이터 저장 및 처리** 워크로드를 사용하도록 설정합니다. ![데이터 저장소 및 처리 워크 로드를 사용 하도록 설정](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

다음과 같이 **Azure 개발** 워크로드를 사용하도록 설정합니다. ![Azure 개발 워크 로드를 사용 하도록 설정](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Azure Data Lake Tools for Visual Studio 2013 및 2015 설치

[다운로드 센터](https://aka.ms/adltoolsvs)에서 Azure Data Lake Tools for Visual Studio를 다운로드하여 설치합니다. 설치가 끝나면 다음을 확인할 수 있습니다.
* **서버 탐색기** > **Azure** 노드에 **Data Lake Analytics** 노드가 포함됩니다. 
* **도구** 메뉴에 **Data Lake** 항목이 있습니다.


## <a name="next-steps"></a>다음 단계
* 진단 정보를 기록하려면 [Azure Data Lake Analytics에 대한 진단 로그에 액세스](data-lake-analytics-diagnostic-logs.md)를 참조하세요.
* 더 복잡한 쿼리를 보려면 [Azure 데이터 레이크 분석을 사용하여 웹 사이트 로그 분석](data-lake-analytics-analyze-weblogs.md)을 참조하세요.
* 꼭짓점 실행 보기를 사용하려면 [Data Lake Tools for Visual Studio에서 Vertex Execution View 사용](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)을 참조
