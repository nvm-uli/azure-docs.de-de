---
title: 'Schnellstart: Senden von benutzerdefinierten Ereignissen an eine Azure-Funktion: Event Grid'
description: 'Schnellstart: Verwenden Sie Azure Event Grid und die Azure CLI oder das Portal, um ein Thema zu veröffentlichen und dieses Ereignis zu abonnieren. Als Endpunkt wird eine Azure-Funktion verwendet.'
services: event-grid
keywords: ''
author: banisadr
ms.author: babanisa
ms.date: 11/15/2019
ms.topic: quickstart
ms.service: event-grid
ms.openlocfilehash: 5c4ba510360475e1365d4901136c94181e8c3da3
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74174867"
---
# <a name="quickstart-route-custom-events-to-an-azure-function-with-event-grid"></a>Schnellstart: Weiterleiten von benutzerdefinierten Ereignissen an eine Azure-Funktion mit Event Grid

Azure Event Grid ist ein Ereignisdienst für die Cloud. Azure Functions ist einer der unterstützten Ereignishandler. In diesem Artikel erstellen Sie über das Azure-Portal ein benutzerdefiniertes Thema, abonnieren dieses benutzerdefinierte Thema und lösen das Ereignis zum Anzeigen des Ergebnisses aus. Sie senden Ereignisse an eine Azure-Funktion.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-azure-function"></a>Azure-Funktion erstellen

Vor dem Abonnieren des benutzerdefinierten Themas erstellen wir zunächst eine Funktion zur Verarbeitung der Ereignisse. Klicken Sie im Azure-Portal auf „Ressource erstellen“, geben Sie „Funktion“ ein, wählen Sie dann „Funktions-App“, und klicken Sie auf „Erstellen“. Wählen Sie unter „Ressourcengruppe“ die Option „Neu erstellen“ aus, und geben Sie der Gruppe einen Namen. Sie wird im weiteren Verlauf des Tutorials verwendet. Geben Sie der Funktions-App einen Namen, lassen Sie den Umschalter „Veröffentlichen“ auf „Code“ eingestellt, wählen Sie eine Runtime und Region aus, und klicken Sie auf „Erstellen“.

Sobald Ihre Funktions-App bereit ist, navigieren Sie zu ihr, und klicken Sie auf "+ Neue Funktion". Wählen Sie „Im Portal“ als Entwicklungsumgebung aus, und klicken Sie auf „Weiter“. Wählen Sie unter „Funktion erstellen“ die Option „Weitere Vorlagen“ aus, um mehr Vorlagen anzuzeigen. Suchen Sie dann nach „ Azure Event Grid-Trigger“, und wählen Sie die Option aus. Wenn Sie diesen Trigger zum ersten Mal verwenden, müssen Sie möglicherweise auf „Installieren“ klicken, um die Erweiterung zu installieren.

![Event Grid-Trigger für Funktion](./media/custom-event-to-function/grid-trigger.png)

Nachdem Sie die Erweiterung installiert haben, klicken Sie auf „Weiter“, geben Sie Ihrer Funktion einen Namen, und klicken Sie dann auf „Erstellen“.

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]

## <a name="create-a-custom-topic"></a>Erstellen eines benutzerdefinierten Themas

Ein Event Grid-Thema verfügt über einen benutzerdefinierten Endpunkt für die Veröffentlichung Ihrer Ereignisse. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im linken Navigationsmenü die Option **Alle Dienste** aus, suchen Sie nach **Event Grid**, und wählen Sie **Event Grid-Themen** aus. 

    ![Auswählen von „Event Grid-Themen“](./media/custom-event-to-function/select-event-grid-topics.png)
3. Wählen Sie auf der Symbolleiste der Seite **Event Grid-Themen** die Option **+ Hinzufügen** aus. 

    ![Schaltfläche zum Hinzufügen von Event Grid-Themen](./media/custom-event-to-function/add-event-grid-topic-button.png)

4. Gehen Sie auf der Seite **Thema erstellen** wie folgt vor:

    1. Geben Sie unter **Name** einen eindeutigen Namen für das benutzerdefinierte Thema an. Der Name des Themas muss eindeutig sein, da er durch einen DNS-Eintrag dargestellt wird. Verwenden Sie nicht den Namen, der in der Abbildung zu sehen ist. Erstellen Sie stattdessen einen eigenen Namen. Dieser muss zwischen drei und 50 Zeichen lang sein und darf nur folgende Zeichen enthalten: a–z, A–Z, 0–9 und Bindestriche (-).
    2. Wählen Sie Ihr Azure- **Abonnement**aus.
    3. Wählen Sie dieselbe Ressourcengruppe wie in den vorherigen Schritten aus.
    4. Wählen Sie einen **Standort** für das Event Grid-Thema aus.
    5. Behalten Sie im Feld **Ereignisschema** den Standardwert **Event Grid-Schema** bei. 

       ![Seite „Thema erstellen“](./media/custom-event-to-function/create-custom-topic.png)
    6. Klicken Sie auf **Erstellen**. 

5. Nachdem das benutzerdefinierte Thema erstellt wurde, erhalten Sie eine entsprechende Benachrichtigung. Wählen Sie **Zu Ressourcengruppe wechseln** aus. 

   ![Benachrichtigung über die erfolgreiche Erstellung](./media/custom-event-to-function/success-notification.png)

6. Wählen Sie auf der Seite **Ressourcengruppe** das Event Grid-Thema aus. 

   ![Auswählen der Ressource für das Event Grid-Thema](./media/custom-event-to-function/select-event-grid-topic.png)

7. Daraufhin wird die Seite **Event Grid-Thema** für Ihre Event Grid-Instanz angezeigt. Lassen Sie diese Seite geöffnet. Sie wird im weiteren Verlauf der Schnellstartanleitung benötigt. 

    ![Startseite des Event Grid-Themas](./media/custom-event-to-function/event-grid-topic-home-page.png)

## <a name="subscribe-to-custom-topic"></a>Abonnieren eines benutzerdefinierten Themas

Sie abonnieren ein Event Grid-Thema, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten und wohin diese gesendet werden sollen.

1. Wählen Sie nun auf der Symbolleiste der Seite **Event Grid-Thema** für Ihr benutzerdefiniertes Thema die Option **+ Ereignisabonnement** aus.

   ![Hinzufügen eines Ereignisabonnements](./media/custom-event-to-function/new-event-subscription.png)

2. Gehen Sie auf der Seite **Ereignisabonnement erstellen** wie folgt vor:
    1. Geben Sie unter **Name** einen Namen für das Ereignisabonnement an.
    3. Wählen Sie **Azure-Funktion** als **Endpunkttyp** aus. 
    4. Wählen Sie **Endpunkt auswählen** aus. 

       ![Angeben von Werten für das Ereignisabonnement](./media/custom-event-to-function/provide-subscription-values.png)

    5. Wählen Sie als Endpunkt der Funktion das Azure-Abonnement und die Ressourcengruppe aus, in der sich Ihre Funktions-App befindet, und wählen Sie dann die zuvor erstellte Funktions-App und Funktion aus. Klicken Sie auf **Auswahl bestätigen**.

       ![Angeben der Endpunkt-URL](./media/custom-event-to-function/provide-endpoint.png)

    6. Wählen Sie auf der Seite **Ereignisabonnement erstellen** die Option **Erstellen** aus.

## <a name="send-an-event-to-your-topic"></a>Senden eines Ereignisses an Ihr Thema

Als Nächstes lösen wir ein Ereignis aus, um zu sehen, wie Event Grid die Nachricht an Ihren Endpunkt weiterleitet. Verwenden Sie die Azure-Befehlszeilenschnittstelle oder PowerShell, um ein Testereignis an Ihr benutzerdefiniertes Thema zu senden. Üblicherweise werden die Ereignisdaten von einer Anwendung oder einem Azure-Dienst gesendet.

Im ersten Beispiel wird die Azure-Befehlszeilenschnittstelle verwendet. Es ruft die URL und den Schlüssel für das benutzerdefinierte Thema sowie Beispielereignisdaten ab. Verwenden Sie für `<topic name>` den Namen Ihres benutzerdefinierten Themas. Es werden Beispielereignisdaten erstellt. Bei dem `data`-Element des JSON-Codes handelt es sich um die Nutzlast Ihres Ereignisses. Für dieses Feld kann ein beliebiger wohlgeformter JSON-Code verwendet werden. Sie können auch das Betrefffeld zur erweiterten Weiterleitung und Filterung verwenden. CURL ist ein Hilfsprogramm zum Senden von HTTP-Anforderungen.


### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
1. Wählen Sie im Azure-Portal die Option **Cloud Shell** aus. Wählen Sie links oben im Cloud Shell-Fenster die Option **Bash** aus. 

    ![Cloud Shell – Bash](./media/custom-event-quickstart-portal/cloud-shell-bash.png)
1. Führen Sie den folgenden Befehl aus, um den **Endpunkt** für das Thema abzurufen: Aktualisieren Sie nach dem Kopieren und Einfügen des Befehls den Namen des Themas (**topic name**) und den Namen der Ressourcengruppe (**resource group name**), bevor Sie den Befehl ausführen. 

    ```azurecli
    endpoint=$(az eventgrid topic show --name <topic name> -g <resource group name> --query "endpoint" --output tsv)
    ```
2. Führen Sie den folgenden Befehl aus, um den **Schlüssel** für das benutzerdefinierte Thema abzurufen: Aktualisieren Sie nach dem Kopieren und Einfügen des Befehls den Namen des Themas (**topic name**) und den Namen der Ressourcengruppe (**resource group name**), bevor Sie den Befehl ausführen. 

    ```azurecli
    key=$(az eventgrid topic key list --name <topic name> -g <resource group name> --query "key1" --output tsv)
    ```
3. Kopieren Sie die folgende Anweisung mit der Ereignisdefinition, und drücken Sie die **EINGABETASTE**. 

    ```json
    event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/motorcycles", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "make": "Ducati", "model": "Monster"},"dataVersion": "1.0"} ]'
    ```
4. Führen Sie den folgenden **Curl**-Befehl aus, um das Ereignis zu veröffentlichen:

    ```
    curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
    ```

### <a name="azure-powershell"></a>Azure PowerShell
Im zweiten Beispiel werden ähnliche Schritte mit PowerShell ausgeführt.

1. Wählen Sie im Azure-Portal die Option **Cloud Shell** aus (navigieren Sie alternativ zu https://shell.azure.com/). Wählen Sie links oben im Cloud Shell-Fenster die Option **PowerShell** aus. Eine Beispielabbildung des Fensters **Cloud Shell** finden Sie im Abschnitt für die Azure-Befehlszeilenschnittstelle.
2. Legen Sie die folgenden Variablen fest. Aktualisieren Sie nach dem Kopieren und Einfügen der einzelnen Befehle den Namen des Themas (**topic name**) und den Namen der Ressourcengruppe (**resource group name**), bevor Sie den Befehl ausführen:

    ```powershell
    $resourceGroupName = <resource group name>
    $topicName = <topic name>
    ```
3. Führen Sie die folgenden Befehle aus, um den **Endpunkt** und die **Schlüssel** für das Thema abzurufen:

    ```powershell
    $endpoint = (Get-AzEventGridTopic -ResourceGroupName $resourceGroupName -Name $topicName).Endpoint
    $keys = Get-AzEventGridTopicKey -ResourceGroupName $resourceGroupName -Name $topicName
    ```
4. Bereiten Sie das Ereignis vor. Kopieren Sie die Anweisungen, und führen Sie sie im Cloud Shell-Fenster aus. 

    ```powershell
    $eventID = Get-Random 99999

    #Date format should be SortableDateTimePattern (ISO 8601)
    $eventDate = Get-Date -Format s

    #Construct body using Hashtable
    $htbody = @{
        id= $eventID
        eventType="recordInserted"
        subject="myapp/vehicles/motorcycles"
        eventTime= $eventDate   
        data= @{
            make="Ducati"
            model="Monster"
        }
        dataVersion="1.0"
    }
    
    #Use ConvertTo-Json to convert event body from Hashtable to JSON Object
    #Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
    $body = "["+(ConvertTo-Json $htbody)+"]"
    ```
5. Verwenden Sie das Cmdlet **Invoke-WebRequest**, um das Ereignis zu senden. 

    ```powershell
    Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
    ```

### <a name="verify-in-the-event-grid-viewer"></a>Überprüfen im Event Grid-Viewer
Sie haben das Ereignis ausgelöst, und Event Grid hat die Nachricht an den Endpunkt gesendet, den Sie beim Abonnieren konfiguriert haben. Navigieren Sie zu Ihrer ausgelösten Event Grid-Funktion, und öffnen Sie die Protokolle. In den Protokollen sollte eine Kopie der Datennutzlast des Ereignisses angezeigt werden. Ist dies nicht der Fall, achten Sie darauf, das Protokollfenster zuerst zu öffnen, oder klicken Sie auf „Erneute Verbindungsherstellung“, und versuchen Sie dann erneut, ein Testereignis zu senden.

![Protokoll eines erfolgreichen Funktionstriggers](./media/custom-event-to-function/successful-function.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie dieses Ereignis weiterverwenden möchten, können Sie die Bereinigung der in diesem Artikel erstellten Ressourcen überspringen. Löschen Sie andernfalls die Ressourcen, die Sie in diesem Artikel erstellt haben.

1. Wählen Sie im linken Menü die Option **Ressourcengruppen** aus. Sollte die Option nicht angezeigt werden, wählen Sie im linken Menü die Option **Alle Dienste** und anschließend **Ressourcengruppen** aus. 
2. Wählen Sie die Ressourcengruppe aus, um die Seite **Ressourcengruppe** zu öffnen. 
3. Wählen Sie auf der Symbolleiste die Option **Ressourcengruppe löschen** aus. 
4. Bestätigen Sie den Löschvorgang, indem Sie den Namen der Ressourcengruppe eingeben und **Löschen** auswählen. 

    ![Ressourcengruppen](./media/custom-event-to-function/delete-resource-groups.png)

    Die andere Ressourcengruppe, die auf der Abbildung zu sehen ist, wurde vom Cloud Shell-Fensters erstellt und verwendet. Löschen Sie sie, falls Sie das Cloud Shell-Fenster nicht mehr benötigen. 
```

## Next steps

Now that you know how to create topics and event subscriptions, learn more about what Event Grid can help you do:

- [About Event Grid](overview.md)
- [Route Blob storage events to a custom web endpoint](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Stream big data into a data warehouse](event-grid-event-hubs-integration.md)
