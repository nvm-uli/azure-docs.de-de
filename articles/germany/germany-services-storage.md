---
title: Speicherdienste von Azure Deutschland | Microsoft-Dokumentation
description: Dieses Thema enthält einen Vergleich der Speicherdienste für Azure Deutschland. Außerdem finden Sie hier weitere relevante Informationen.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: ralfwi
ms.openlocfilehash: c916fe1f115e95badef83f276523004c04127ba0
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896266"
---
# <a name="azure-germany-storage-services"></a>Speicherdienste von Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/de-de/microsoft-cloud-2019-rechenzentren-deutschland/) haben wir keine neuen Kunden mehr akzeptiert und stellen keine neuen Funktionen und Services an den ursprünglichen Standorten von Microsoft Cloud Deutschland mehr bereit.
>
> Basierend auf die Entwicklung der Kundenbedürfnisse konzentriert sich unsere Cloudstrategie für Deutschland auf die Bereitstellung der [neuen Cloudregionen in Deutschland](https://news.microsoft.com/de-de/microsoft-eroeffnet-neue-cloud-rechenzentrumsregionen-in-deutschland/), die zu unserem globalen Cloudangebot passen.
>
> Starten Sie Ihre [Migration](https://docs.microsoft.com/de-de/azure/germany/germany-migration-main) noch heute und nutzen Sie die Vorteile der umfangreichen Funktionalität, Sicherheit auf Unternehmensebene und zahlreichen verfügbaren Funktionen, die in unseren neuen Rechenzentrumsregionen in Deutschland verfügbar sind.

## <a name="storage"></a>Storage
Einzelheiten zu Azure Storage und seiner Verwendung finden Sie in der [globalen Dokumentation zu Storage](../storage/index.yml).

In Azure Storage gespeicherte Daten werden repliziert, um Hochverfügbarkeit sicherzustellen. Bei georedundanten Speichern und georedundanten Speichern mit Lesezugriff repliziert Azure Daten zwischen den *Regionspaaren*. Bei Azure Deutschland sind folgende Regionspaare verfügbar:

| Primäre Region | Sekundäre (zugehörige) Region |
| --- | --- |
| Deutschland, Mitte | Deutschland, Nordosten |
| Deutschland, Nordosten | Deutschland, Mitte |

Bei der Datenreplikation werden die Daten innerhalb deutscher Grenzen gehalten. Die primäre und sekundäre Region werden einander zugeordnet, um sicherzustellen, dass ausreichend Abstand zwischen den Rechenzentren besteht, um bei einem Ausfall eines großen Gebiets oder bei einem Notfall die Verfügbarkeit sicherzustellen. Wählen Sie bei georedundanten Speichern mit hoher Verfügbarkeit beim Erstellen eines Speicherkontos entweder georedundante Speicher oder georedundante Speicher mit Lesezugriff aus.  

Die Speicherdienstverschlüsselung schützt ruhende Daten in Azure Storage-Konten. Wenn Sie diese Funktion aktivieren, verschlüsselt Azure automatisch Daten, bevor sie in den Speicher eingefügt werden. Die Daten werden mithilfe einer 256-Bit-AES-Verschlüsselung verschlüsselt. Die Speicherdienstverschlüsselung unterstützt die Verschlüsselung von Block-, Anfüge- und Seitenblobs.

### <a name="storage-service-availability-by-azure-germany-region"></a>Speicherdienstverfügbarkeit nach Azure Deutschland-Region

| Dienst | Deutschland, Mitte | Deutschland, Nordosten |
| --- | --- | --- |
| [Blob Storage](../storage/common/storage-introduction.md#blob-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Azure Files](../storage/common/storage-introduction.md#azure-files) | Allgemein verfügbar | Allgemein verfügbar |
| [Tabellenspeicherung](../storage/common/storage-introduction.md#table-storage) |Allgemein verfügbar  |Allgemein verfügbar |
| [Queue Storage](../storage/common/storage-introduction.md#queue-storage) |Allgemein verfügbar | Allgemein verfügbar |
| [Hot/Cold Blob Storage](../storage/blobs/storage-blob-storage-tiers.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Service Encryption](../storage/common/storage-service-encryption.md) |Allgemein verfügbar |Allgemein verfügbar |
| Import/Export |Nicht verfügbar |Nicht verfügbar |
| StorSimple |Nicht verfügbar |Nicht verfügbar |

### <a name="variations"></a>Abweichungen
Die URLs für Speicherkonten in Azure Deutschland unterscheiden sich von denen in der globalen Azure-Umgebung:

| Dienstart | Globale Azure-Umgebung | Azure Deutschland |
| --- | --- | --- |
| Blob Storage | *.blob.core.windows.net | *.blob.core.cloudapi.de |
| Azure Files | *.file.core.windows.net | *.file.core.cloudapi.de | 
| Queue Storage | *.queue.core.windows.net | *.queue.core.cloudapi.de |
| Table Storage | *.table.core.windows.net | *.table.core.cloudapi.de |

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die passenden Endpunkte berücksichtigen. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../storage/common/storage-configure-connection-string.md). 
>
>

Weitere Informationen zu APIs finden Sie unter [CloudStorageAccount-Konstruktor](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount.-ctor).

Das zu verwendende Endpunktsuffix für diese Überladungen lautet *core.cloudapi.de*.

> [!NOTE]
> Wenn während der [Einbindung der Dateifreigabe](../storage/files/storage-dotnet-how-to-use-files.md) der Fehler 53 („Der Netzwerkpfad wurde nicht gefunden.“) zurückgegeben wird, blockiert möglicherweise eine Firewall den ausgehenden Port. Versuchen Sie, die Dateifreigabe auf einem virtuellen Computer einzubinden, der sich im selben Azure-Abonnement wie das Speicherkonto befindet.
>
>


## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.
