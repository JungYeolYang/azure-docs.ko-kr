---
title: '빠른 시작: Python을 사용하여 텍스트 분석 API 호출'
titleSuffix: Azure Cognitive Services
description: Azure Cognitive Services에서 텍스트 분석 API 사용을 빠르게 시작하는 데 도움이 되는 정보 및 코드 샘플을 확인합니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: 69eb3789586233b824da1ef6a9c338b07281f324
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001391"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>빠른 시작: Python을 사용하여 텍스트 분석 Cognitive Service 호출 
<a name="HOLTop"></a>

이 연습에서는 [텍스트 분석 API](//go.microsoft.com/fwlink/?LinkID=759711) 및 Python을 사용하여 [언어 감지](#Detect), [감정 분석](#SentimentAnalysis) 및 [핵심 구 추출](#KeyPhraseExtraction)을 수행하는 방법을 보여 줍니다.

바인더 배지 시작을 클릭하여 [MyBinder](https://mybinder.org)에서 Jupyter Notebook으로 또는 명령줄에서 이 예제를 실행할 수 있습니다.

[![바인더](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

### <a name="command-line"></a>명령 줄

Jupyter용 커널 [IPython](https://ipython.org/install.html)을 업데이트해야 할 수 있습니다.
```bash
pip install --upgrade IPython
```

[요청](http://docs.python-requests.org/en/master/) 라이브러리를 업데이트해야 할 수 있습니다.
```bash
pip install requests
```

API 기술 문서는 [API 정의](//go.microsoft.com/fwlink/?LinkID=759346)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

* [!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

* 등록하는 동안 생성된 [엔드포인트 및 액세스 키](../How-tos/text-analytics-how-to-access-key.md).

* 다음 가져오기, 구독 키 및 `text_analytics_base_url`은 아래의 모든 빠른 시작에 사용됩니다. 가져오기를 추가합니다.

    ```python
    import requests
    # pprint is pretty print (formats the JSON)
    from pprint import pprint
    from IPython.display import HTML
    ```
    
    이러한 줄을 추가한 후 `subscription_key`를 앞에서 받은 유효한 구독 키로 바꿉니다.
    
    ```python
    subscription_key = '<ADD KEY HERE>'
    assert subscription_key
    ```
    
    그런 다음, 이 줄을 추가한 후 `text_analytics_base_url`의 지역이 서비스를 설정할 때 사용한 지역과 일치하는지 확인합니다. 평가판 키를 사용하는 경우에는 아무것도 변경할 필요가 없습니다.
    
    ```python
    text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/"
    ```

<a name="Detect"></a>

## <a name="detect-languages"></a>언어 감지

언어 감지 API는 [언어 감지 메서드](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7)를 사용하여 텍스트 문서의 언어를 감지합니다. 해당 지역에 대한 언어 감지 API의 서비스 엔드포인트는 다음 URL을 통해 사용할 수 있습니다.

```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/languages


API에 대한 페이로드는 각각 `id` 및 `text` 특성을 포함하는 `documents` 목록으로 구성됩니다. `text` 특성은 분석할 텍스트를 저장합니다. 

`documents` 사전을 언어 감지에 사용할 다른 텍스트로 바꿉니다.

```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

다음 몇 줄의 코드에서는 Python의 `requests` 라이브러리를 사용하여 언어 감지 API를 호출하고 문서의 언어를 확인합니다.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

다음 코드 줄에서는 JSON 데이터를 HTML 테이블로 렌더링합니다.

```python
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

성공적인 JSON 응답:

```json
    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
                                           'name': 'English',
                                           'score': 1.0}],
                    'id': '1'},
                   {'detectedLanguages': [{'iso6391Name': 'es',
                                           'name': 'Spanish',
                                           'score': 1.0}],
                    'id': '2'},
                   {'detectedLanguages': [{'iso6391Name': 'zh_chs',
                                           'name': 'Chinese_Simplified',
                                           'score': 1.0}],
                    'id': '3'}],
     'errors': []}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>감정 분석

감정 분석 API는 [감정 메서드](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)를 사용하여 텍스트 레코드 세트의 감정(긍정적 감정 또는 부정적 감정)을 감지합니다. 다음 예제에서는 두 개의 문서(영어 문서와 스페인어 문서 각 1개)에 점수를 매깁니다.

감정 분석을 위한 서비스 엔드포인트는 다음 URL을 통해 해당 지역에 사용할 수 있습니다.

```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment

언어 감지 예제와 마찬가지로, 문서 목록으로 구성된 `documents` 키가 있는 사전이 서비스에 제공됩니다. 각 문서는 `id`, 분석할 `text` 및 텍스트의 `language`로 구성된 튜플입니다. 이전 섹션의 언어 감지 API를 사용하여 이 필드를 채울 수 있습니다.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

이제 감정 API를 사용하여 문서의 감정을 분석할 수 있습니다.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

성공적인 JSON 응답:

```json
{'documents': [{'id': '1', 'score': 0.7673527002334595},
                {'id': '2', 'score': 0.18574094772338867},
                {'id': '3', 'score': 0.5}],
    'errors': []}
```

문서의 감정 점수는 0.0에서 1.0 사이이며, 점수가 높을수록 보다 긍정적인 감정을 나타냅니다.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>핵심 구 추출

핵심 구 추출 API는 [핵심 구 메서드](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)를 사용하여 텍스트 문서에서 핵심 구를 추출합니다. 이 연습 섹션에서는 영어 문서와 스페인어 문서 둘 다에서 핵심 구를 추출합니다.

핵심 구 추출 서비스의 서비스 엔드포인트는 다음 URL을 통해 액세스합니다.

```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases

문서 컬렉션은 감정 분석에 사용된 것과 동일합니다.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

다음 코드 줄을 사용하여 JSON 개체를 HTML 테이블로 렌더링할 수 있습니다.

```python
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

다음 몇 줄의 코드에서는 Python의 `requests` 라이브러리를 사용하여 언어 감지 API를 호출하고 문서의 언어를 확인합니다.
```python
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

성공적인 JSON 응답:
```json
{'documents': [
    {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
    {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
    {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
    {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
    'errors': []
}
```

## <a name="identify-entities"></a>엔터티 식별

엔터티 API는 [엔터티 메서드](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)를 사용하여 텍스트 문서에서 잘 알려진 엔터티를 식별합니다. 다음 예제에서는 영어 문서의 엔터티를 식별합니다.

엔터티 링크 설정 서비스의 서비스 엔드포인트는 다음 URL을 통해 액세스합니다.

```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/entities

문서 컬렉션은 다음과 같습니다.

```python
documents = {'documents' : [
  {'id': '1', 'text': 'Microsoft is an It company.'}
]}
```
이제 문서를 텍스트 분석 API에 전송하여 응답을 받을 수 있습니다.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```

성공적인 JSON 응답:
```json
{  
   "documents":[  
      {  
         "id":"1",
         "entities":[  
            {  
               "name":"Microsoft",
               "matches":[  
                  {  
                     "wikipediaScore":0.20872054383103444,
                     "entityTypeScore":0.99996185302734375,
                     "text":"Microsoft",
                     "offset":0,
                     "length":9
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Microsoft",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Microsoft",
               "bingId":"a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "type":"Organization"
            },
            {  
               "name":"Technology company",
               "matches":[  
                  {  
                     "wikipediaScore":0.82123868042800585,
                     "text":"It company",
                     "offset":16,
                     "length":10
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Technology company",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Technology_company",
               "bingId":"bc30426e-22ae-7a35-f24b-454722a47d8f"
            }
         ]
      }
   ],
    "errors":[]
}
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [텍스트 분석 및 Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>참고 항목 

 [Text Analytics 개요](../overview.md)  
 [FAQ(질문과 대답)](../text-analytics-resource-faq.md)
