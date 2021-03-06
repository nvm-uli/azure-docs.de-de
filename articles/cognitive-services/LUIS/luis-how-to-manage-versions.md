---
title: 'Verwalten von Versionen: LUIS'
titleSuffix: Azure Cognitive Services
description: Mithilfe von Versionen können Sie verschiedene Modelle erstellen und veröffentlichen. Es ist eine bewährte Methode, das aktuell aktive Modell in eine andere Version der App zu klonen, bevor Änderungen am Modell vorgenommen werden.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 138b84a9b7f54782fd6254304a3fdcf4dba83182
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74221926"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Verwenden von Versionen, um Staging- und Produktions-Apps nicht durch Bearbeitungsschritte oder Tests zu beeinträchtigen

Mithilfe von Versionen können Sie verschiedene Modelle erstellen und veröffentlichen. Es ist eine bewährte Methode, das aktuell aktive Modell in eine andere [Version](luis-concept-version.md) der App zu klonen, bevor Änderungen am Modell vorgenommen werden. 

Öffnen Sie zum Arbeiten mit Versionen Ihre App, indem Sie den jeweiligen Namen auf der Seite **Meine Apps**, dann in der oberen Leiste **Verwalten** und anschließend im linken Navigationsbereich **Versionen** auswählen. 

Aus der Liste der Versionen geht hervor, welche Versionen veröffentlicht wurden, wo sie veröffentlicht wurden und welche Version aktuell aktiv ist. 

> [!div class="mx-imgBorder"]
> [![Abschnitt „Verwalten“, Seite „Versionen“](./media/luis-how-to-manage-versions/versions-import.png "Abschnitt „Verwalten“, Seite „Versionen“")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Klonen einer Version

1. Wählen Sie die Version aus, die Sie klonen möchten, und wählen Sie dann auf der Symbolleiste **Klonen** aus. 

2. Geben Sie im Dialogfeld **Version klonen** einen Namen für die neue Version ein, z.B. „0.2“.

   ![Dialogfeld „Version klonen“](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Die Versions-ID darf nur Zeichen, Ziffern oder „.“ enthalten und nicht länger als 10 Zeichen sein.
 
   Eine neue Version mit dem angegebenen Namen wird erstellt und als aktive Version festgelegt.

## <a name="set-active-version"></a>Festlegen der aktiven Version

Wählen Sie in der Liste eine Version und dann auf der Symbolleiste **Aktivieren** aus. 

> [!div class="mx-imgBorder"]
> [![Abschnitt „Verwalten“, Seite „Versionen“, Erstellen einer Versionsaktion](./media/luis-how-to-manage-versions/versions-other.png "Abschnitt „Verwalten“, Seite „Versionen“, Erstellen einer Versionsaktion")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Importversion

Sie können eine `.json`- oder eine `.lu`-Version Ihrer Anwendung importieren.

1. Wählen Sie auf der Symbolleiste die Option **Importieren** und dann das Format aus. 

2. Geben Sie im Popupfenster **Neue Version importieren** den neuen, aus zehn Zeichen bestehenden Namen der Version ein. Wenn die Version in der Datei in der App bereits vorhanden ist, müssen Sie lediglich eine Versions-ID festlegen.

    ![Abschnitt „Verwalten“, Seite „Versionen“, Importieren einer neuen Version](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Nachdem Sie eine Version importiert haben, wird die neue Version die aktive Version.

### <a name="import-errors"></a>Importfehler

* Tokenizer-Fehler: Wenn Sie beim Importieren einen **Tokenizer-Fehler** erhalten, versuchen Sie, eine Version zu importieren, die einen anderen [Tokenizer](luis-language-support.md#custom-tokenizer-versions) verwendet als die aktuell verwendete App. Informationen zum Beheben dieses Problems finden Sie unter [Migrieren zwischen Tokenizer-Versionen](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Andere Aktionen

* Um eine Version zu **löschen**, wählen Sie eine Version in der Liste und dann auf der Symbolleiste **Löschen** aus. Klicken Sie auf **OK**. 
* Um eine Version **umzubenennen**, wählen Sie eine Version in der Liste und dann auf der Symbolleiste **Umbenennen** aus. Geben Sie den neuen Namen ein, und wählen Sie **Fertig** aus. 
* Um eine Version zu **exportieren**, wählen Sie eine Version in der Liste und dann auf der Symbolleiste **Exportieren** aus. Wählen Sie JSON aus, um einen Export für die Sicherung durchzuführen. Wählen Sie **Export for container** (Für Container exportieren) aus, um [diese App in einem LUIS-Container zu verwenden](luis-container-howto.md).  

