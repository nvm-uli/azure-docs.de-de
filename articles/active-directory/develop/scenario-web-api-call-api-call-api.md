---
title: 'Web-API, die Web-APIs aufruft: Microsoft Identity Platform | Azure'
description: Hier erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5829ca41aaa4bd61f8878657e5eedbf6351b5df4
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423577"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>Web-API, die Web-APIs aufruft – Aufrufen einer API

Wenn Sie über ein Token verfügen, können Sie eine geschützte Web-API aufrufen. Dies erfolgt über den Controller Ihrer ASP.NET/ASP.NET Core-Web-API.

## <a name="controller-code"></a>Controllercode

Hier ist die Fortsetzung des in [Aufrufen geschützter Web-APIs durch Web-APIs – Anfordern eines Tokens](scenario-web-api-call-api-acquire-token.md) gezeigten Beispielcodes, der in den Aktionen des API-Controllers aufgerufen wird und eine Downstream-API (namens „todolist“) aufruft.

Nachdem Sie das Token abgerufen haben, können Sie dieses als Bearertoken zum Aufrufen der Downstream-API verwenden.

```csharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-web-api-call-api-production.md)
