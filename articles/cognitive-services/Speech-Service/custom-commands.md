---
title: 'Speech-Dienst: Benutzerdefinierte Befehle (Vorschau)'
titleSuffix: Azure Cognitive Services
description: Ein Überblick über die Features, Funktionen und Einschränkungen für Benutzerdefinierte Befehle (Vorschau), einer Lösung zum Erstellen von Sprachassistenten.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: dd3e27aefe03ee30e721fcbb915d9ad4b3821618
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806302"
---
# <a name="custom-commands-preview"></a>Benutzerdefinierte Befehle (Vorschau)

[Sprachassistenten](voice-assistants.md) hören den Benutzern zu und ergreifen als Reaktion eine Aktion, wobei sie dabei häufig antworten. Sie verwenden die [Spracherkennung](speech-to-text.md), um die Sprache des Benutzers zu erfassen, und ergreifen dann Maßnahmen zum in natürlicher Sprache verstandenen Text. Diese Aktion umfasst häufig die Sprachausgabe des Assistenten, die mit [Sprachsynthese](text-to-speech.md) generiert wurde. Geräte stellen eine Verbindung mit Assistenten über das `DialogServiceConnector`-Objekt des Speech SDKs her.

**Benutzerdefinierte Befehle (Vorschau)** sind eine optimierte Lösung für die Erstellung eines Sprachassistenten. Es bietet eine einheitliche Benutzeroberfläche für die Erstellung, ein automatisches Hostingmodell und eine relativ geringere Komplexität im Vergleich zu anderen Erstellungsoptionen für Assistenten wie [Direct Line Speech](direct-line-speech.md). Diese Vereinfachung geht jedoch mit einer Verringerung der Flexibilität einher. Daher ist Benutzerdefinierte Befehle (Vorschau) am besten geeignet für die Aufgabenerledigung oder Befehl-und-Steuerung-Szenarien (Command-and-Control). Das Feature eignet sich besonders gut für IoT-Geräte (Internet of Things, Internet der Dinge) und monitorlose Geräte.

Für komplexe dialogorientierte Interaktionen und die Integration mit anderen Lösungen wie der [Lösung für virtuelle Assistenten und der Unternehmensvorlage](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) wird die Verwendung von Direct Line Speech empfohlen.

Geeignete Kandidaten für Benutzerdefinierte Befehle (Vorschau) verfügen über ein festes Vokabular mit klar definierten Variablensätzen. So sind z. B. Aufgaben zur Gebäudeautomatisierung, wie die Steuerung eines Thermostaten, ideal.

   ![Beispiele für Szenarien zur Aufgabenerledigung](media/voice-assistants/task-completion-examples.png "Beispiele für die Aufgabenerledigung")

## <a name="getting-started-with-custom-commands-preview"></a>Erste Schritte mit Benutzerdefinierte Befehle (Vorschau)

Der erste Schritt zur Verwendung von Benutzerdefinierte Befehle (Vorschau) zur Erstellung eines Sprachassistenten besteht im [Abrufen eines Speech-Abonnementschlüssels](get-started.md) und dem Zugriff auf den Generator für Benutzerdefinierte Befehle (Vorschau) im [Speech Studio](https://speech.microsoft.com). Von dort aus können Sie eine neue Anwendung für Benutzerdefinierte Befehle (Vorschau) erstellen und veröffentlichen, woraufhin eine Anwendung auf dem Gerät mit ihr über das Speech SDK kommunizieren kann.

   ![Erstellungsablauf für Benutzerdefinierte Befehle (Vorschau)](media/voice-assistants/custom-commands-flow.png "Erstellungsablauf für Benutzerdefinierte Befehle (Vorschau)")

Wir bieten Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können.

* [Erstellen einer Anwendung für benutzerdefinierte Befehle (Vorschau)](quickstart-custom-speech-commands-create-new.md)
* [Erstellen einer Anwendung für benutzerdefinierte Befehle (Vorschau) mit Parametern](quickstart-custom-speech-commands-create-parameters.md)
* [Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle (Vorschau) mit dem Speech SDK, C#](quickstart-custom-speech-commands-speech-sdk.md)

## <a name="sample-code"></a>Beispielcode

Beispielcode für die Erstellung eines Sprachassistenten mit Benutzerdefinierte Befehle (Vorschau) ist auf GitHub verfügbar.

* [Beispiele für Sprachassistenten (SDK)](https://aka.ms/csspeech/samples)

## <a name="customization"></a>Anpassung

Sprachassistenten, die mithilfe des Speech-Dienstes erstellt wurden, können die gesamte Bandbreite der Anpassungsoptionen nutzen, die für [Spracherkennung](speech-to-text.md), [Sprachsynthese](text-to-speech.md) und die [Auswahl benutzerdefinierter Stichwörter](speech-devices-sdk-create-kws.md) zur Verfügung stehen.

> [!NOTE]
> Die Anpassungsoptionen unterscheiden sich je nach Sprache/Gebietsschema. Weitere Informationen erhalten Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Speech-Diensts](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
