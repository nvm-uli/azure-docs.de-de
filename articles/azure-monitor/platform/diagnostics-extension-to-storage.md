---
title: Speichern und Anzeigen von Diagnosedaten in Azure Storage
description: Erfahren Sie, wie Sie Azure-Diagnosedaten in einem Azure Storage-Konto erfassen, damit Sie sie mit einem der verschiedenen verfügbaren Tools anzeigen können.
services: azure-monitor
author: jpconnock
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/01/2016
ms.author: jeconnoc
ms.subservice: diagnostic-extension
ms.openlocfilehash: 35e852a36ebc52edff338ed640419afe32297b81
ms.sourcegitcommit: 8a2949267c913b0e332ff8675bcdfc049029b64b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74304960"
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Speichern und Anzeigen von Diagnosedaten im Azure-Speicher
Diagnosedaten werden nicht dauerhaft gespeichert, wenn sie nicht in den Microsoft Azure-Speicheremulator oder den Azure-Speicher übertragen werden. Sobald die Daten gespeichert wurden, können sie mit einem der verschiedenen verfügbaren Tools angezeigt werden.

## <a name="specify-a-storage-account"></a>Festlegen eines Speicherkontos
Legen Sie das Speicherkonto fest, das Sie in der Datei „ServiceConfiguration.cscfg“ verwenden möchten. Die Kontoinformationen werden als Verbindungszeichenfolge in einer Konfigurationseinstellung definiert. Das folgende Beispiel zeigt die Standardverbindungszeichenfolge, die für ein neues Clouddienstprojekt in Visual Studio erstellt wurde:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Sie können diese Verbindungszeichenfolge zum Bereitstellen von Kontoinformationen für ein Azure-Speicherkonto ändern.

Je nach Art der gesammelten Diagnosedaten verwendet die Azure-Diagnose entweder den Blob-Dienst oder den Tabellenspeicherdienst. Die folgende Tabelle zeigt die beibehaltenen Datenquellen und deren Format.

| Datenquelle | Speicherformat |
| --- | --- |
| Azure-Protokolle |Table |
| IIS 7.0-Protokolle |Blob |
| Infrastrukturprotokolle der Azure-Diagnose |Table |
| Ablaufprotokolle für fehlgeschlagene Anforderungen |Blob |
| Windows-Ereignisprotokolle |Table |
| Leistungsindikatoren |Table |
| Absturzabbilder |Blob |
| Benutzerdefinierte Fehlerprotokolle |Blob |

## <a name="transfer-diagnostic-data"></a>Übertragen von Diagnosedaten
Bei SDK 2.5 und höher kann die Anforderung zum Übertragen von Diagnosedaten über die Konfigurationsdatei erfolgen. Sie können Diagnosedaten in geplanten Intervallen wie in der Konfiguration festgelegt übertragen.

Bei SDK 2.4 und früher kann die Anforderung zum Übertragen von Diagnosedaten sowohl über die Konfigurationsdatei als auch programmgesteuert erfolgen. Der programmgesteuerte Ansatz ermöglicht Ihnen auch bedarfsgesteuerte Übertragungen.

> [!IMPORTANT]
> Wenn Sie Diagnosedaten in ein Azure-Speicherkonto übertragen, fallen Kosten für die von den Diagnosedaten genutzten Speicherressourcen an.
> 
> 

## <a name="store-diagnostic-data"></a>Speichern von Diagnosedaten
Protokolldaten werden im Blob- oder Tabellenspeicher mit den folgenden Namen gespeichert:

**Tabellen**

* **WadLogsTable** – Mit dem Ablaufverfolgungslistener in Code erstellte Protokolle
* **WADDiagnosticInfrastructureLogsTable** – Diagnosemonitor und Konfigurationsänderungen
* **WADDirectoriesTable** – Vom Diagnosemonitor überwachte Verzeichnisse.  Dies umfasst IIS-Protokolle, Protokolle zu IIS-Anforderungsfehlern und benutzerdefinierte Verzeichnisse.  Der Speicherort der Blob-Protokolldatei ist im Feld „Container“ festgelegt, ihr Name im Feld „RelativePath“.  Das Feld „AbsolutePath“ gibt den Speicherort und Namen der Datei wie auf dem virtuellen Azure-Computer vorhanden an.
* **WADPerformanceCountersTable** – Leistungsindikatoren
* **WADWindowsEventLogsTable** – Windows-Ereignisprotokolle

**Blobs**

* **wad-control-container** – (Nur für SDK 2.4 und früher) Enthält die XML-Konfigurationsdateien, die die Azure-Diagnose steuern.
* **wad-iis-failedreqlogfiles** – Enthält Informationen aus den Protokollen zu IIS-Anforderungsfehlern.
* **wad-iis-logfiles:** enthält Informationen zu IIS-Protokollen.
* **"custom"** – Ein benutzerdefinierter Container basierend auf Konfigurationsverzeichnissen, die vom Diagnosemonitor überwacht werden.  Der Name dieses Blob-Containers wird in WADDirectoriesTable festgelegt.

## <a name="tools-to-view-diagnostic-data"></a>Tools zum Anzeigen von Diagnosedaten
Mehrere Tools stehen zum Anzeigen der Daten nach der Übertragung an den Speicher zur Verfügung. Beispiel:

* Server-Explorer in Visual Studio – wenn Sie die Azure-Tools für Microsoft Visual Studio installiert haben, können Sie den Azure-Speicherknoten im Server-Explorer zum Anzeigen schreibgeschützter Blob- und Tabellendaten aus Ihren Azure-Speicherkonten verwenden. Sie können Daten aus dem lokalen Speicheremulatorkonto und auch aus den für Azure erstellten Speicherkonten anzeigen. Weitere Informationen finden Sie unter [Durchsuchen und Verwalten von Speicherressourcen mit dem Server-Explorer](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage).
* Beim [Microsoft Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine eigenständige App, über die Sie ganz einfach mit Azure Storage-Daten arbeiten können – unter Windows, OSX und Linux.
* [Azure Management Studio](https://www.cerebrata.com/products/azure-management-studio/introduction) enthält Azure Diagnostics Manager, mit dem Sie die Diagnosedaten anzeigen, herunterladen und verwalten können, die von den auf Azure ausgeführten Anwendungen gesammelt werden.

## <a name="next-steps"></a>Nächste Schritte
[Verfolgen des Ablaufs in einer Cloud Services-Anwendung mit der Azure-Diagnose](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)


