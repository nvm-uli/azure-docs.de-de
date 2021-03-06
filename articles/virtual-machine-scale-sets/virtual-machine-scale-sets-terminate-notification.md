---
title: Beendigungsbenachrichtigungen für Instanzen von Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Informationen zum Aktivieren von Beendigungsbenachrichtigungen für Instanzen von Azure-VM-Skalierungsgruppen
services: virtual-machine-scale-sets
documentationcenter: ''
author: shandilvarun
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2019
ms.author: vashan
ms.openlocfilehash: d3d7f92b3803114321bc7420b5c4ba059aabcb9d
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74705924"
---
# <a name="terminate-notification-for-azure-virtual-machine-scale-set-instances-preview"></a>Beendigungsbenachrichtigung für Instanzen von Azure-VM-Skalierungsgruppen (Vorschauversion)
Für Skalierungsgruppeninstanzen kann der Empfang von Beendigungsbenachrichtigungen aktiviert und ein vordefiniertes Verzögerungstimeout für den Beendigungsvorgang festgelegt werden. Die Beendigungsbenachrichtigung wird über „Azure Metadata Service – [Scheduled Events](../virtual-machines/windows/scheduled-events.md)“ gesendet. Dieser Subdienst sendet Benachrichtigungen über die Durchführung und Verzögerung einschneidender Vorgänge, z. B. Neustarts und erneute Bereitstellungen. In der Vorschaulösung wurde der Scheduled Events-Liste mit „Terminate“ ein weiteres Ereignis hinzugefügt. Welche Verzögerung dem Beendigungsereignis zugeordnet wird, hängt von dem Verzögerungslimit ab, das der Benutzer in der Konfiguration des Skalierungsgruppenmodells festlegt.

Nachdem Sie sich für das Feature registriert haben, müssen Skalierungsgruppeninstanzen nicht mehr auf den Ablauf des angegebenen Timeouts warten, bevor die Instanz gelöscht werden kann. Nach dem Empfang einer Beendigungsbenachrichtigung kann die Instanz jederzeit gelöscht werden, bevor das Beendigungstimeout abläuft.

> [!IMPORTANT]
> Die Beendigungsbenachrichtigung für Skalierungsgruppeninstanzen befindet sich derzeit in der Public Preview. Es ist kein Opt-in-Verfahren erforderlich, um die unten beschriebenen Funktionen der Public Preview zu nutzen.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enable-terminate-notifications"></a>Aktivieren von Beendigungsbenachrichtigungen
Es gibt mehrere Möglichkeiten, Beendigungsbenachrichtigungen für Skalierungsgruppeninstanzen zu aktivieren. Dies ist unten in den Beispielen beschrieben.

### <a name="rest-api"></a>REST-API

Im folgenden Beispiel wird eine Beendigungsbenachrichtigung für das Skalierungsgruppenmodell aktiviert.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "virtualMachineProfile": {
            "scheduledEventsProfile": {
                "terminateNotificationProfile": {
                    "notBeforeTimeout":"PT5M",
                    "enable":true
                }
            }
        }
    }        
}

```

Durch den vorangehenden Block wird eine Timeoutverzögerung von 5 Minuten (wie durch *PT5M* angegeben) für jeden beliebigen Beendigungsvorgang festgelegt, der für Instanzen in der Skalierungsgruppe ausgeführt wird. Das Feld *notBeforeTimeout* kann einen beliebigen Wert zwischen 5 und 15 Minuten im ISO 8601-Format enthalten. Sie können das Standardtimeout für den Beendigungsvorgang ändern, indem Sie die *notBeforeTimeout*-Eigenschaft unter *terminateNotificationProfile* wie oben beschrieben ändern.

Nachdem Sie *scheduledEventsProfile* für das Skalierungsgruppenmodell aktiviert und *notBeforeTimeout* festgelegt haben, aktualisieren Sie die einzelnen Instanzen entsprechend dem [neuesten Modell](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model), um die Änderungen widerzuspiegeln.

> [!NOTE]
>Beendigungsbenachrichtigungen für Skalierungsgruppeninstanzen können nur mit API-Version 2019-03-01 und höher aktiviert werden.

### <a name="azure-powershell"></a>Azure PowerShell
Beim Erstellen einer neuen Skalierungsgruppe können Sie Beendigungsbenachrichtigungen für die Skalierungsgruppe mithilfe des Cmdlets [New-AzVmss](/powershell/module/az.compute/new-azvmss) aktivieren.

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -TerminateScheduledEvents
```

Im obigen Beispiel wird eine neue Skalierungsgruppe mit aktivierten Beendigungsbenachrichtigungen und einem Standardtimeout von fünf Minuten erstellt. Beim Erstellen einer neuen Skalierungsgruppe ist für den *TerminateScheduledEvents*-Parameter kein Wert erforderlich. Um den Timeoutwert zu ändern, geben Sie das gewünschte Timeout über den *TerminateScheduledEventNotBeforeTimeoutInMinutes*-Parameter an.

Verwenden Sie das Cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss), um Beendigungsbenachrichtigungen für eine bestehende Skalierungsgruppe zu aktivieren.

```azurepowershell-interactive
Update-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -TerminateScheduledEvents $true
  -TerminateScheduledEventNotBeforeTimeoutInMinutes 15
```
Im vorangehenden Beispiel werden Beendigungsbenachrichtigungen für eine bestehende Skalierungsgruppe aktiviert. Außerdem wird ein 15-minütiges Timeout für das Beendigungsereignis festgelegt.

Nachdem Sie Scheduled Events für das Skalierungsgruppenmodell aktiviert und das Timeout festgelegt haben, aktualisieren Sie die einzelnen Instanzen entsprechend dem [neuesten Modell](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model), um die Änderungen widerzuspiegeln.

## <a name="get-terminate-notifications"></a>Empfangen von Beendigungsbenachrichtigungen

Beendigungsbenachrichtigungen werden über [Scheduled Events](../virtual-machines/windows/scheduled-events.md) zugestellt. Dabei handelt es sich um einen Azure Metadata Service. Der Azure-Metadatendienst macht Informationen zu ausgeführten virtuellen Computern mithilfe eines innerhalb einer VM zugänglichen REST-Endpunkts verfügbar. Die Informationen werden über eine nicht routingfähige IP-Adresse bereitgestellt, die außerhalb der VM nicht verfügbar gemacht wird.

Scheduled Events wird für die Skalierungsgruppe aktiviert, wenn Sie zum ersten Mal Ereignisse anfordern. Beim ersten Aufruf ist eine Verzögerung der Antwort um bis zu zwei Minuten zu erwarten. Senden Sie regelmäßig Abfragen an den Endpunkt, um anstehende Wartungsereignisse und den Status fortlaufender Wartungsaktivitäten zu ermitteln.

Scheduled Events wird für die Skalierungsgruppe deaktiviert, wenn von den Skalierungsgruppeninstanzen 24 Stunden lang keine Anforderung eingeht.

### <a name="endpoint-discovery"></a>Endpunktermittlung
Für VNET-fähige VMs ist der Metadata Service über eine statische, nicht routingfähige IP-Adresse (169.254.169.254) erreichbar.

Der vollständige Endpunkt für die neueste Version von Scheduled Events ist in dieser Vorschauversion:
> 'http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01 '

### <a name="query-response"></a>Abfrageantwort
Eine Antwort enthält ein Array geplanter Ereignisse. Ein leeres Array bedeutet, dass derzeit keine Ereignisse geplant sind.

Sofern geplante Ereignisse vorliegen, enthält die Antwort ein Array mit Ereignissen. Die Antwort für das „Terminate“-Ereignis sieht wie folgt aus:
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Terminate",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled",
            "NotBefore": {timeInUTC},
        }
    ]
}
```
DocumentIncarnation ist ein ETag und bietet eine einfache Möglichkeit, um zu untersuchen, ob sich die Ereignisnutzlast seit der letzten Abfrage geändert hat.

Weitere Informationen zu den einzelnen vorangehenden Feldern finden Sie in der Scheduled Events-Dokumentation für [Windows](../virtual-machines/windows/scheduled-events.md#event-properties) und [Linux](../virtual-machines/linux/scheduled-events.md#event-properties).

### <a name="respond-to-events"></a>Reagieren auf Ereignisse
Sobald Sie von einem bevorstehenden Ereignis erfahren und Ihre Logik um kontrolliertes Herunterfahren ergänzt haben, können Sie ein ausstehendes Ereignis genehmigen, indem Sie einen POST-Aufruf vom Typ „EventId“ an den Metadata Service senden. Durch den POST-Aufruf wird Azure mitgeteilt, dass das Löschen des virtuellen Computers fortgesetzt werden kann.

Im Folgenden finden Sie den im POST-Anforderungstext erwarteten JSON-Code. Die Anforderung sollte eine Liste mit StartRequests-Elementen enthalten. Jedes StartRequest-Element enthält die EventId für das Ereignis, das Sie beschleunigen möchten:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

Stellen Sie sicher, dass durch jede VM in der Skalierungsgruppe nur die EventID genehmigt wird, die für die VM relevant ist. Eine VM kann ihren eigenen VM-Namen [über Instanzmetadaten](virtual-machine-scale-sets-instance-ids.md#instance-metadata-vm-name) abrufen. Dieser Name hat das Format „{scale-set-name}_{instance-id}“ und wird im Abschnitt „Ressourcen“ der oben beschriebenen Abfrageantwort angezeigt.

Weitere Informationen finden Sie in den Beispielskripts für das Abfragen und Reagieren auf Ereignisse mit [PowerShell](../virtual-machines/windows/scheduled-events.md#powershell-sample) und [Python](../virtual-machines/linux/scheduled-events.md#python-sample).

## <a name="tips-and-best-practices"></a>Tipps und bewährte Methoden
-   Beendigungsbenachrichtigungen nur für Löschvorgänge – Durch alle Löschvorgänge (manuelles Löschen oder durch Autoskalierung initiiertes, horizontales Herunterskalieren) werden Terminate-Ereignisse generiert, sofern *scheduledEventsProfile* für die Skalierungsgruppe aktiviert wurde. Durch andere Vorgänge wie Neustart, Reimaging, erneute Bereitstellung und Beenden/Zuordnung aufheben werden keine Terminate-Ereignisse generiert. Für VMs mit niedriger Priorität können keine Beendigungsbenachrichtigungen aktiviert werden.
-   Keine verbindliche Wartezeit für Timeouts – Sie können den Beendigungsvorgang jederzeit starten, nachdem das Ereignis empfangen wurde und bevor die *NotBefore*-Zeit des Ereignisses abläuft.
-   Verbindliche Löschung bei Timeout – Die Vorschau bietet keine Möglichkeit zur Erhöhung des Timeoutwerts, nachdem ein Ereignis generiert wurde. Nach Ablauf des Timeouts wird das ausstehende Terminate-Ereignis verarbeitet, und die VM wird gelöscht.
-   Änderbarer Timeoutwert – Sie können den Timeoutwert jederzeit ändern, bevor eine Instanz gelöscht wird, indem Sie die *notBeforeTimeout*-Eigenschaft des Skalierungsgruppenmodells ändern und die VM-Instanzen entsprechend dem neuesten Modell aktualisieren.
-   Alle ausstehenden Löschvorgänge genehmigen – Wenn es einen ausstehenden, nicht genehmigten Löschvorgang auf VM_1 gibt und ein weiteres Terminate-Ereignis auf VM_2 genehmigt wurde, wird VM_2 erst gelöscht, nachdem das Beendigungsereignis für VM_1 genehmigt wurde bzw. das Timeout abgelaufen ist. Sobald Sie das Beendigungsereignis für VM_1 genehmigen, werden sowohl VM_1 als auch VM_2 gelöscht.
-   Alle gleichzeitigen Löschvorgänge genehmigen (Ergänzung des vorangehenden Beispiels) – Wenn VM_1 und VM_2 dieselbe *NotBefore*-Zeit aufweisen, müssen beide Beendigungsereignisse genehmigt werden, da VMs ansonsten erst wieder gelöscht werden können, wenn das Timeout abgelaufen ist.

## <a name="troubleshoot"></a>Problembehandlung
### <a name="failure-to-enable-scheduledeventsprofile"></a>Fehler beim Aktivieren von scheduledEventsProfile
Wenn Sie einen BadRequest-Fehler mit der Fehlermeldung „Member 'scheduledEventsProfile' für Objekt vom Typ 'VirtualMachineProfile' nicht gefunden“ erhalten, überprüfen Sie die für Skalierungsgruppenvorgänge verwendete API-Version. Für diese Vorschauversion ist Compute-API-Version **2019-03-01** oder höher erforderlich.

### <a name="failure-to-get-terminate-events"></a>Fehler beim Abrufen von Terminate-Ereignissen
Wenn Sie keine **Terminate**-Ereignisse über Scheduled Events erhalten, dann überprüfen Sie die API-Version, die zum Abrufen der Ereignisse verwendet wird. Für Terminate-Ereignisse wird die Metadata Service-API-Version **2019-01-01** benötigt.
>'http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01 '

### <a name="getting-terminate-event-with-incorrect-notbefore-time"></a>Abrufen von Terminate-Ereignissen mit falscher NotBefore-Zeit  
Nachdem Sie *scheduledEventsProfile* für das Skalierungsgruppenmodell aktiviert und *notBeforeTimeout* festgelegt haben, aktualisieren Sie die einzelnen Instanzen entsprechend dem [neuesten Modell](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model), um die Änderungen widerzuspiegeln.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über das [Bereitstellen Ihrer Anwendung](virtual-machine-scale-sets-deploy-app.md) in VM-Skalierungsgruppen.
