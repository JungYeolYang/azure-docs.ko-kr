---
ms.openlocfilehash: b609a708a987194398c53bdf83f0d6e1f281808d
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62113552"
---
기본적으로 Mobile Apps 백 엔드의 API는 익명으로 호출할 수 있습니다. 다음으로, 인증된 클라이언트로만 액세스를 제한해야 합니다.  

* **Node.js 백 엔드(Azure Portal을 통해)**:  

    Mobile Apps 설정에서 **간편한 테이블**을 클릭하고 테이블을 선택합니다. **사용 권한 변경**을 클릭하고 모든 사용 권한에 대해 **인증된 액세스만**을 선택한 다음 **저장**을 클릭합니다.
* **.NET 백 엔드(C#)**:  

    서버 프로젝트에서 **컨트롤러** > **TodoItemController.cs**로 이동합니다. 다음과 같이 **TodoItemController** 클래스에 `[Authorize]` 특성을 추가합니다. 특정 메서드로만 액세스를 제한하기 위해 이 특성을 클래스 대신 해당 메서드에만 적용할 수도 있습니다. 서버 프로젝트를 다시 게시합니다.

    ```
    [Authorize]
    public class TodoItemController : TableController<TodoItem>
    ```

* **(Node.js 코드를 통해) Node.js 백 엔드** :  

    테이블 액세스에 대한 인증을 요청하려면 Node.js 서버 스크립트에 다음 줄을 추가합니다.

    ```
    table.access = 'authenticated';
    ```

    자세한 내용은 참조 하세요. [방법: 테이블에 대 한 액세스 인증 요구](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth)합니다. 사이트에서 빠른 시작 코드 프로젝트를 다운로드 하는 방법에 알아보려면 참조 [방법: Git를 사용 하 여 Node.js 백 엔드 빠른 시작 코드 프로젝트 다운로드](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart)합니다.