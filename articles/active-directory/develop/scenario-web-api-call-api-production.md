---
title: Verschieben einer Web-API, die Web-APIs aufruft, in die Produktion – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-API in die Produktion verschieben, die Web-APIs aufruft.
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
ms.openlocfilehash: 0d59a5b2a74c10e36103713725113cbe8c9cc412
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965168"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>Web-API, die Web-APIs aufruft (Übergang in die Produktion)

Sobald Sie ein Token zum Aufrufen von Web-APIs abgerufen haben, können Sie Ihre App in die Produktionsumgebung verschieben.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Weitere Informationen

Nachdem Sie sich mit den Grundlagen des Aufrufens von Web-APIs aus Ihrer eigenen Web-API vertraut gemacht haben, könnte Sie dieses Tutorial über den Code interessieren, der zum Erstellen einer geschützten Web-API verwendet wird, die Web-APIs aufruft.

| Beispiel | Plattform | BESCHREIBUNG |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2-Web-API, Desktop (WPF) | Die ASP.NET Core 2.2 Web-API ruft Microsoft Graph auf, wird selbst aber von einer WPF-Anwendung über Microsoft Identity Platform (v2.0) aufgerufen |
