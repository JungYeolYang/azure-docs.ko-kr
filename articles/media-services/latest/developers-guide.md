---
title: Azure Media Services v3 SDK - Azure
description: 이 문서에서는 SDK를 사용하여 Media Services v3 API로 개발을 시작하는 방법의 개요를 제공합니다.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 04/11/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9fb4d1561a661387f759aada9e776d43a95aa5c7
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564512"
---
# <a name="develop-against-media-services-v3-api-using-sdks"></a>SDK를 사용하여 Media Services v3 API로 개발

개발자인 경우 Media Services [REST API](https://aka.ms/ams-v3-rest-ref) 또는 REST API와 상호 작용할 수 있도록 하는 클라이언트 라이브러리를 사용하여 사용자 지정 미디어 워크플로를 손쉽게 만들고, 관리하고 유지할 수 있습니다. [Media Services v3](https://aka.ms/ams-v3-rest-sdk) API는 OpenAPI 사양(이전 명칭 Swagger)에 기반을 두고 있습니다.

> [!NOTE]
> Azure Media Services v3 SDK는 스레드로부터 안전을 보장하지 않습니다. 다중 스레드 애플리케이션을 개발하는 경우 클라이언트를 보호하는 고유한 스레드 동기화 논리를 추가하거나 스레드별로 새 AzureMediaServicesClient 개체를 사용해야 합니다. 코드에서 클라이언트에 제공하는 선택적 개체(예: .NET의 HttpClient 인스턴스) 때문에 다중 스레드 문제가 도입될 수 있다는 점에도 주의해야 합니다.

이 항목에서는 SDK, 도구, 설명서에 대한 링크를 제공합니다.

## <a name="prerequisites"></a>필수 조건

Media Services에 대한 개발을 시작하려면 다음이 필요합니다.

- 활성 Azure 구독. Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)을 만듭니다.
- [기본 개념 알아보기](concepts-overview.md)
- [Media Services v3 API로 개발](media-services-apis-overview.md) 검토
- [Media Services 계정 만들기 - CLI](create-account-cli-how-to.md)

## <a name="start-developing-with-sdks"></a>SDK를 사용하여 개발 시작

### <a name="net"></a>.NET

[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)를 사용하여 [Media Services에 연결](configure-connect-dotnet-howto.md)합니다.

Media Services [.NET 참조](https://aka.ms/ams-v3-dotnet-ref) 설명서를 살펴봅니다.

### <a name="java"></a>자바

[Java SDK](https://aka.ms/ams-v3-java-sdk)를 사용하여 [Media Services에 연결](configure-connect-java-howto.md)합니다.

Media Services [Java 참조](https://aka.ms/ams-v3-java-ref) 설명서를 살펴봅니다.

### <a name="nodejs"></a>Node.js

[Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk)를 사용하여 [Media Services에 연결](configure-connect-nodejs-howto.md)합니다.

Media Services [Node.js 참조](https://aka.ms/ams-v3-nodejs-ref) 설명서를 살펴보고 node.js와 함께 Media Services API를 사용하는 방법을 보여주는 [샘플](https://github.com/Azure-Samples/media-services-v3-node-tutorials)을 확인하세요.

### <a name="python"></a>Python

[Python SDK](https://aka.ms/ams-v3-python-sdk)를 사용합니다.

Media Services [Python 참조](https://aka.ms/ams-v3-python-ref) 설명서를 검토합니다.

### <a name="go"></a>Go

[Go SDK](https://aka.ms/ams-v3-go-sdk)를 사용합니다.

Media Services [Go 참조](https://aka.ms/ams-v3-go-ref) 설명서를 검토합니다.

### <a name="ruby"></a>Ruby

[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)를 사용합니다.

## <a name="azure-media-services-explorer"></a>Azure Media Services 탐색기

[Azure Media Services 탐색기](https://github.com/Azure/Azure-Media-Services-Explorer)(AMSE)는 Media Services에 대해 알아보고자 하는 Windows 고객이 사용할 수 있는 도구입니다. AMSE는 Media Services에서 VOD 및 실시간 콘텐츠의 업로드, 다운로드, 인코딩, 스트리밍을 수행하는 Winforms/C# 애플리케이션입니다. AMSE 도구는 코드를 작성하지 않고 Media Services를 테스트하려는 고객을 위한 도구입니다. AMSE 코드는 Media Services를 사용하여 개발하려는 고객을 위한 리소스로 제공됩니다.

AMSE는 오픈 소스 프로젝트이며, 커뮤니티에서 지원을 제공합니다(문제는 https://github.com/Azure/Azure-Media-Services-Explorer/issues)에 보고할 수 있음). 이 프로젝트에는 [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/)(Microsoft 오픈 소스 준수 사항)이 적용됩니다. 자세한 내용은 [Code of Conduct FAQ(준수 사항 FAQ)](https://opensource.microsoft.com/codeofconduct/faq/)를 참조하거나 opencode@microsoft.com에 추가 질문 또는 의견을 알려주세요.

## <a name="next-steps"></a>다음 단계

[개요](media-services-overview.md)
