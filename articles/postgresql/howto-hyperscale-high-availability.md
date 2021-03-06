---
title: Konfigurieren von Hochverfügbarkeit – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Aktivieren oder Deaktivieren von Hochverfügbarkeit
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: a8d4b5949b34d16191e9ec10a1dd39faff3660dc
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74977654"
---
# <a name="configure-hyperscale-citus-high-availability"></a>Konfigurieren von Hochverfügbarkeit für Hyperscale (Citus)

Azure Database for PostgreSQL – Hyperscale (Citus) bietet Hochverfügbarkeit (High Availability, HA), um Datenbankausfälle zu vermeiden. Wenn Hochverfügbarkeit aktiviert ist, erhält jeder Knoten in einer Servergruppe einen Standknoten. Wenn der ursprüngliche Knoten einen Fehler aufweist, wird der Standbyknoten höher gestuft, um ihn zu ersetzen.

> [!IMPORTANT]
> Da Hochverfügbarkeit die Anzahl der Server in der Gruppe verdoppelt, werden auch die Kosten verdoppelt.

Die Aktivierung von Hochverfügbarkeit ist während der Erstellung der Servergruppe oder danach auf der Registerkarte **Konfigurieren** für Ihre Servergruppe im Azure-Portal möglich. Die Benutzeroberfläche sieht in beiden Fällen ähnlich aus. Ziehen Sie den Schieberegler für **Hochverfügbarkeit** auf „JA“:

![Schieberegler für Hochverfügbarkeit](./media/howto-hyperscale-high-availability/01-ha-slider.png)

Klicken Sie auf die Schaltfläche **Speichern**, um Ihre Auswahl zu übernehmen. Das Aktivieren von Hochverfügbarkeit kann einige Zeit in Anspruch nehmen, da die Servergruppe Standbyknoten bereitstellen und Daten an diese streamen muss.

Auf der Registerkarte **Übersicht** für die Servergruppe werden alle Knoten und deren Standbyknoten zusammen mit einer Spalte **Hochverfügbarkeit** aufgelistet, die anzeigt, ob Hochverfügbarkeit für jeden Knoten erfolgreich aktiviert wurde.

![Spalte „Hochverfügbarkeit“ in der Übersicht der Servergruppe](./media/howto-hyperscale-high-availability/02-ha-column.png)

### <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Hochverfügbarkeit](concepts-hyperscale-high-availability.md).
