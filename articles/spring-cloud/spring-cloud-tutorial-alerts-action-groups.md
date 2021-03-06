---
title: 'Tutorial: Überwachen von Azure Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Spring Cloud-Warnungen verwenden.
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/18/2019
ms.openlocfilehash: 2be21b20c394ae8505ad18f2c411db7aab06215f
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689565"
---
# <a name="tutorial-monitor-spring-cloud-resources-using-alerts-and-action-groups"></a>Tutorial: Überwachen von Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen

Mit Azure Spring Cloud-Warnungen können Sie Ressourcen auf der Grundlage von Bedingungen (etwa dem verfügbaren Speicher, der Anforderungsrate oder der Datennutzung) überwachen Eine Warnung sendet eine Benachrichtigung, wenn Raten oder Bedingungen die definierten Spezifikationen erfüllen.

Zum Einrichten einer Warnungspipeline müssen zwei Schritte ausgeführt werden: 
1. Richten Sie eine Aktionsgruppe mit den Aktionen (E-Mail, SMS, Runbook oder Webhook) ein, die beim Auslösen einer Warnung ausgeführt werden sollen. Aktionsgruppen können für unterschiedliche Warnungen erneut verwendet werden.
2. Richten Sie Warnungsregeln ein. Die Regeln binden Metrikmuster basierend auf Zielressource, Metrik, Bedingung, Zeitaggregation usw. an die Aktionsgruppen.

## <a name="prerequisites"></a>Voraussetzungen
Zusätzlich zu den erforderlichen Azure Spring-Komponenten werden für dieses Tutorial die folgenden Ressourcen benötigt:

* Eine bereitgestellte Azure Spring Cloud-Instanz.  Befolgen Sie unseren [Schnellstart](spring-cloud-quickstart-launch-app-cli.md), um zu beginnen.

* Eine zu überwachende Azure-Ressource, beispielsweise die im folgenden Artikel implementierte Datenbank: [Verwenden der Spring Data-Apache Cassandra-API mit Azure Cosmos DB](https://docs.microsoft.com/azure/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db).
 
Die folgenden Prozeduren initialisieren sowohl **Aktionsgruppe** als auch **Warnung** ausgehend von der Option **Warnungen** im linken Navigationsbereich einer Spring Cloud-Instanz. (Die Prozedur kann auch über die Seite **Monitor – Übersicht** im Azure-Portal gestartet werden.) 

Navigieren Sie von einer Ressourcengruppe zu Ihrer Spring Cloud-Instanz. Wählen Sie im linken Bereich **Warnungen** und dann **Aktionen verwalten** aus.

![Screenshot der Seite „Ressourcengruppe“ im Portal](media/alerts-action-groups/action-1-a.png)

## <a name="set-up-action-group"></a>Einrichten einer Aktionsgruppe

Wählen Sie **+ Aktionsgruppe hinzufügen** aus, um die Prozedur zum Initialisieren einer neuen **Aktionsgruppe** zu starten.

![Screenshot der Option „Aktionsgruppe hinzufügen“ im Portal](media/alerts-action-groups/action-1.png)

Gehen Sie auf der Seite **Aktionsgruppe hinzufügen** wie folgt vor:

 1. Geben Sie unter **Name der Aktionsgruppe** und **Kurzname** jeweils einen Namen ein.

 1. Geben Sie **Abonnement** und **Ressourcengruppe** an.

 1. Geben Sie unter **Aktionsname** einen Namen ein.

 1. Wählen Sie unter **Aktionstyp** eine Option aus.  Dadurch wird ein weiterer Bereich auf der rechten Seite geöffnet, in dem Sie die Aktion definieren können, die bei Aktivierung ausgeführt wird.

 1. Definieren Sie die Aktion mithilfe der Optionen im rechten Bereich.  In diesem Fall wird eine E-Mail-Benachrichtigung verwendet.

 1. Klicken Sie im rechten Aktionsbereich auf **OK**.

 1. Klicken Sie im Dialogfeld **Aktionsgruppe hinzufügen** auf **OK**. 

  ![Screenshot: Definieren einer Aktion im Portal](media/alerts-action-groups/action-2.png)

## <a name="set-up-alert"></a>Einrichten einer Warnung 

In den vorherigen Schritten wurde eine **Aktionsgruppe** erstellt, die eine Benachrichtigung per E-Mail verwendet. Sie können auch Telefonbenachrichtigungen, Webhooks, Azure Functions usw. verwenden.  

Navigieren Sie zum Konfigurieren einer **Warnung** zurück zur Seite **Warnungen**, und klicken Sie auf **Warnungsregeln verwalten**.

  ![Screenshot: Definieren einer Warnung im Portal](media/alerts-action-groups/alerts-2.png)

1. Wählen Sie die **Ressource** für die Warnung aus.

1. Klicken Sie auf **+ Neue Warnungsregel**.

  ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-3.png)

1. Geben Sie auf der Seite **Regel erstellen** Werte für **RESSOURCE**, **BEDINGUNG** und **AKTIONEN** an.  Wählen Sie im Bereich **AKTIONEN** die zuvor definierte **Aktionsgruppe** aus.

1. Geben Sie unter **WARNUNGSDETAILS** einen Namen für die Warnungsregel ein.

1. Klicken Sie auf **Warnungsregel erstellen**.

  ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-4.png)

Vergewissern Sie sich, dass die neue Warnungsregel aktiviert ist.

  ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-5.png)

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* [SMS-Warnungsverhalten in Aktionsgruppen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-sms-behavior)
* [Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
