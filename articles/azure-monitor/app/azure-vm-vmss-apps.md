---
title: Überwachen der Leistung von Anwendungen auf Azure-VMs und in Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Leistungsüberwachung für Anwendungen auf Azure-VMs und in Azure-VM-Skalierungsgruppen. Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 08/26/2019
ms.openlocfilehash: 248dfb83c26d3f49fb492272ee3bd87d1e34fefa
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73161476"
---
# <a name="deploy-the-azure-monitor-application-insights-agent-on-azure-virtual-machines-and-azure-virtual-machine-scale-sets"></a>Bereitstellen des Azure Monitor Application Insights-Agents auf virtuellen Azure-Computern und in Azure-VM-Skalierungsgruppen

Die Überwachung der .NET-basierten Webanwendungen auf [virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/) und in [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) lässt sich jetzt einfacher denn je aktivieren. Profitieren Sie von allen Vorteilen der Verwendung von Application Insights, ohne den Code zu ändern.

Dieser Artikel führt Sie Schritt für Schritt durch das Aktivieren der Application Insights-Überwachung mithilfe des Application Insights-Agents und bietet eine vorläufige Anleitung zur Automatisierung des Prozesses für umfangreiche Bereitstellungen.

> [!IMPORTANT]
> Der Azure Application Insights-Agent für .NET befindet sich derzeit in der Public Preview.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Einige Features werden möglicherweise nicht unterstützt oder bieten möglicherweise nur eingeschränkte Funktionen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

Es gibt zwei Methoden, um die Überwachung von Anwendungen zu aktivieren, die auf virtuellen Azure-Computern und in Azure-VM-Skalierungsgruppen gehostet werden:

* **Codefrei** über Application Insights-Agent
    * Diese Methode stellt die einfachste Möglichkeit der Aktivierung dar und erfordert keine erweiterte Konfiguration. Sie wird häufig als „Laufzeitüberwachung“ bezeichnet.

    * Für virtuelle Azure-Computer und Azure-VM-Skalierungsgruppen wird empfohlen, mindestens diese Überwachungsstufe zu aktivieren. Danach können Sie basierend auf dem jeweiligen Szenario überprüfen, ob eine manuelle Instrumentierung erforderlich ist.

    * Der Application Insights-Agent sammelt automatisch die gleichen Abhängigkeitssignale wie das .NET SDK. Weitere Informationen finden Sie unter [Automatisches Sammeln von Abhängigkeiten](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies#net).
        > [!NOTE]
        > Zurzeit werden nur IIS-gehostete .NET-Anwendungen unterstützt. Verwenden Sie ein SDK zum Instrumentieren von ASP.NET Core-, Java- und Node.js-Anwendungen, die auf virtuellen Azure-Computern und in VM-Skalierungsgruppen gehostet werden.

* **Codebasiert** über SDK

    * Dieser Ansatz ermöglicht eine wesentlich stärkere Anpassung, erfordert jedoch das [Hinzufügen einer Abhängigkeit von den Application Insights SDK-NuGet-Paketen](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Diese Methode bedeutet auch, dass Sie die Updates auf die neueste Version der Pakete selbst verwalten müssen.

    * Wenn Sie benutzerdefinierte API-Aufrufe zum Nachverfolgen von Ereignissen/Abhängigkeiten ausführen müssen, die bei der Agent-basierten Überwachung nicht standardmäßig erfasst werden, müssen Sie diese Methode verwenden. Weitere Informationen finden Sie im Artikel zur [API für benutzerdefinierte Ereignisse und Metriken](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics).

> [!NOTE]
> Wenn sowohl die Agent-basierte Überwachung als auch die manuelle SDK-basierte Instrumentierung erkannt wird, werden nur die Einstellungen der manuellen Instrumentierung berücksichtigt. Dadurch wird verhindert, dass doppelte Daten gesendet werden. Weitere Informationen dazu finden Sie im [Abschnitt zur Problembehandlung](#troubleshooting) weiter unten.

## <a name="manage-application-insights-agent-for-net-applications-on-azure-virtual-machines-using-powershell"></a>Verwalten des Application Insights-Agents für .NET-Anwendungen auf virtuellen Azure-Computern mithilfe von PowerShell

> [!NOTE]
> Bevor Sie den Application Insights-Agent installieren, brauchen Sie einen Instrumentierungsschlüssel. [Erstellen Sie eine neue Application Insights-Ressource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource), oder kopieren Sie den Instrumentierungsschlüssel aus einer vorhandenen Application Insights-Ressource.

> [!NOTE]
> Neu bei PowerShell? Sehen Sie sich den [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azps-2.5.0) an.

Installieren oder Aktualisieren des Application Insights-Agents als Erweiterung für virtuelle Azure-Computer
```powershell
$publicCfgJsonString = '
{
  "redfieldConfiguration": {
    "instrumentationKeyMap": {
      "filters": [
        {
          "appFilter": ".*",
          "machineFilter": ".*",
          "instrumentationSettings" : {
            "instrumentationKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "<myVmLocation>" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

> [!NOTE]
> Sie können den Application Insights-Agent mithilfe einer PowerShell-Schleife bedarfsorientiert als Erweiterung für mehrere virtuelle Computer installieren oder aktualisieren.

Deinstallieren der Application Insights-Agent-Erweiterung auf einem virtuellen Azure-Computer
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

Abfragen des Status der Application Insights-Agent-Erweiterung für einen virtuellen Azure-Computer
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

Abrufen der Liste der installierten Erweiterungen für einen virtuellen Azure-Computer
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```
Sie können installierte Erweiterungen auch auf dem [Blatt für den virtuellen Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/extensions/overview) im Portal anzeigen.

> [!NOTE]
> Überprüfen Sie die Installation, indem Sie auf den Live Metrics Stream innerhalb der Application Insights-Ressource klicken, die dem Instrumentierungsschlüssel zugeordnet ist, den Sie zum Bereitstellen der Application Insights-Agent-Erweiterung verwendet haben. Wenn Sie Daten von mehreren virtuellen Computern senden, wählen Sie unter „Servername“ den virtuellen Azure-Zielcomputer aus. Es kann bis zu einer Minute dauern, bis Daten fließen.

## <a name="manage-application-insights-agent-for-net-applications-on-azure-virtual-machine-scale-sets-using-powershell"></a>Verwalten des Application Insights-Agents für .NET-Anwendungen in Azure-VM-Skalierungsgruppen mithilfe von PowerShell

Installieren oder Aktualisieren des Application Insights-Agents als Erweiterung für Azure-VM-Skalierungsgruppen
```powershell
$publicCfgHashtable =
@{
  "redfieldConfiguration"= @{
    "instrumentationKeyMap"= @{
      "filters"= @(
        @{
          "appFilter"= ".*";
          "machineFilter"= ".*";
          "instrumentationSettings"= @{
            "instrumentationKey"= "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"; # Application Insights Instrumentation Key, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
          }
        }
      )
    }
  }
};
$privateCfgHashtable = @{};

$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -TypeHandlerVersion "2.8" -Setting $publicCfgHashtable -ProtectedSetting $privateCfgHashtable

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Deinstallieren der Erweiterung zur Anwendungsüberwachung in Azure-VM-Skalierungsgruppen
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Abfragen des Status der Erweiterung zur Anwendungsüberwachung für Azure-VM-Skalierungsgruppen
```powershell
# Not supported by extensions framework
```

Abrufen der Liste der installierten Erweiterungen für Azure-VM-Skalierungsgruppen
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>Problembehandlung

Hier finden Sie Tipps zur Problembehandlung für die Application Insights Monitoring-Agent-Erweiterung für .NET-Anwendungen, die auf virtuellen Azure-Computern und in VM-Skalierungsgruppen ausgeführt wird.

> [!NOTE]
> .NET Core-, Java- und Node.js-Anwendungen werden auf virtuellen Azure-Computern und in Azure-VM-Skalierungsgruppen nur über die manuelle SDK-basierte Instrumentierung unterstützt. Daher gelten die folgenden Schritte nicht für diese Szenarien.

Die Ausgabe der Erweiterungsausführung wird in Dateien in folgenden Verzeichnissen protokolliert:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Nächste Schritte
* Hier erfahren Sie, wie Sie [eine Anwendung in einer Azure-VM-Skalierungsgruppe bereitstellen](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md).
* [Richten Sie Verfügbarkeitswebtests ein](monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihr Endpunkt nicht verfügbar ist.
