---
title: Verbinden Ihrer Umgebung mit Power BI – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Time Series Insights mit Power BI verbinden können, um Daten unternehmensweit freizugeben, aufzuzeichnen und anzuzeigen.
author: deepakpalled
ms.author: dpalled
manager: cshankar
services: time-series-insights
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: 8a464a8a635e0531eba2f9cc6fbceeb9edcea8bb
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74705972"
---
# <a name="visualize-data-from-time-series-insights-in-power-bi"></a>Visualisieren der Daten von Time Series Insights in Power BI

Azure Time Series Insights ist eine Plattform zum Speichern, Verwalten, Abfragen und Visualisieren von Zeitreihendaten in der Cloud. [Power BI](https://powerbi.microsoft.com) ist ein Tool zur Unternehmensanalyse mit umfangreichen Visualisierungsfunktionen, mit dem Sie Erkenntnisse und Ergebnisse unternehmensweit teilen können. Beide Dienste können jetzt integriert werden, um das Beste aus den inhärenten Visualisierungsfunktionen von Time Series Insights und von Power BI zu erhalten.

Sie lernen Folgendes:

* Verbinden von Time Series Insights mit Power BI über den Cloudconnector
* Erstellen visueller Elemente mit Ihren Daten in Power BI
* Veröffentlichen des Berichts für Power BI und Teilen mit dem restlichen Unternehmen

Am Ende erfahren Sie, wie Sie Zeitreihendaten mit Azure Time Series Insights visualisieren und mit der leistungsstarken Datenvisualisierung und den einfachen Freigabefunktionen von Power BI erweitern können.

Stellen Sie sicher, dass Sie sich für ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/free/) registrieren, falls Sie noch keins besitzen.

## <a name="prerequisites"></a>Voraussetzungen

* Herunterladen und Installieren der aktuellen Version von [Power BI Desktop](https://powerbi.microsoft.com/downloads/)
* Besitzen oder Erstellen einer [Azure Time Series Insights Preview-Instanz](time-series-insights-update-how-to-manage.md)

> [!IMPORTANT]
> Der Power BI-Connector wird derzeit in den Umgebungen mit *nutzungsbasierter Bezahlung* der Time Series Insights-Vorschau unterstützt, die als **Warm Storage** konfiguriert werden.

## <a name="connect-data-from-time-series-insights-to-power-bi"></a>Verbinden der Daten von Time Series Insights mit Power BI

Gehen Sie wie folgt vor, um die Time Series Insights-Umgebung mit Power BI zu verbinden:

1. Öffnen Sie den Time Series Insights-Explorer.                      
1. Exportieren Sie Daten als Abfrage oder als Rohdaten.                       
1. Öffnen Sie Power BI Desktop.
1. Laden Sie Daten über die benutzerdefinierte Abfrage.

### <a name="export-data-into-power-bi-desktop"></a>Exportieren Sie Daten in Power BI Desktop.

Erste Schritte:

1. Öffnen Sie den Time Series Insights Preview-Explorer, und bearbeiten Sie Ihre Daten.
1. Nachdem Sie eine Ansicht erstellt haben, mit der Sie zufrieden sind, navigieren Sie zum Dropdownmenü **Weitere Aktionen**, und klicken Sie auf **Mit Power BI verbinden**. 

    [![Export des Time Series Insights Preview-Explorers](media/how-to-connect-power-bi/time-series-insights-export-option.png)](media/how-to-connect-power-bi/time-series-insights-export-option.png#lightbox)

1. Legen Sie Ihre Parameter auf dieser Registerkarte fest:

   1. Geben Sie einen relativen Zeitrahmen für die Anzeige an. Wenn Sie mit Ihrer bestehenden Ansicht zufrieden sind, belassen Sie diese als **Vorhandener Zeitrahmen**. 
   1. Wählen Sie zwischen **aggregierten Ereignissen** und **Rohereignissen**. 
   
       > [!NOTE]
       > Sie können Ihre Daten jederzeit später in Power BI aggregieren, aber nach der Aggregation nicht zu Rohdaten zurückkehren. 
       
       > [!NOTE]
       > Es gibt ein 100-K-Limit für die Ereignisanzahl für Daten der Ebene „Rohereignisse“.

       [![Verbinden](media/how-to-connect-power-bi/connect-to-power-bi.png)](media/how-to-connect-power-bi/connect-to-power-bi.png#lightbox)

   1. Wenn Sie Ihre Time Series Insights-Instanz nicht für **Warm Storage** konfiguriert haben, erhalten Sie eine Warnung.

       [![Verbinden](media/how-to-connect-power-bi/connect-to-power-bi-warning.png)](media/how-to-connect-power-bi/connect-to-power-bi-warning.png#lightbox)

       > [!TIP]
       > Sie können Ihre bestehende Instanz im Azure-Portal für **Warm Storage** konfigurieren.

1. Wählen Sie **Abfrage in Zwischenablage kopieren** aus.
1. Starten Sie jetzt Power BI Desktop.
1. Wählen Sie in Power BI Desktop auf der Registerkarte **Start** die Option **Daten abrufen** in der oberen linken Ecke und anschließend **Mehr** aus.

    [![Dropdownliste für Start](media/how-to-connect-power-bi/power-bi-home-drop-down.png)](media/how-to-connect-power-bi/power-bi-home-drop-down.png#lightbox)

1. Suchen Sie nach **Time Series Insights**, wählen Sie **Azure Time Series Insights (Beta)** und dann **Verbinden** aus.

    [![Verbinden von Power BI mit Time Series Insights](media/how-to-connect-power-bi/connect-to-time-series-insights.png)](media/how-to-connect-power-bi/connect-to-time-series-insights.png#lightbox)

    Alternativ können Sie auch zur Registerkarte **Azure** navigieren, **Azure Time Series Insights (Beta)** und dann **Verbinden** auswählen.
    
1. Es wird ein Dialogfeld mit einer Meldung angezeigt, in dem Sie um die Berechtigung zum Herstellen von Verbindungen mit Ressourcen von Drittanbietern gebeten werden. Wählen Sie **Weiter** aus.

    [![Auswählen von „Benutzerdefinierte Abfrage erstellen“](media/how-to-connect-power-bi/confirm-the-connection.png)](media/how-to-connect-power-bi/confirm-the-connection.png#lightbox)

1. Wählen Sie im Dropdownmenü unter **Datenquelle** die Option **Benutzerdefinierte Abfrage erstellen** aus. Fügen Sie den Inhalt Ihrer Zwischenablage unten in das optionale Feld **Benutzerdefinierte Abfrage (optional)** ein, und drücken Sie dann auf **OK**.

    [![Übergeben der benutzerdefinierten Abfrage und Auswählen von „OK“](media/how-to-connect-power-bi/custom-query-load.png)](media/how-to-connect-power-bi/custom-query-load.png#lightbox)  

1. Die Datentabelle wird jetzt geladen. Drücken Sie **Laden**, um sie in Power BI zu laden.

    [![Überprüfen der geladenen Daten in der Tabelle und Auswählen von „Laden“](media/how-to-connect-power-bi/review-the-loaded-data-table.png)](media/how-to-connect-power-bi/review-the-loaded-data-table.png#lightbox)  

Wenn Sie diese Schritte abgeschlossen haben, wechseln Sie zum nächsten Abschnitt.

## <a name="create-a-report-with-visuals"></a>Erstellen eines Berichts mit Visuals

Nachdem Sie die Daten jetzt in Power BI importiert haben, ist es an der Zeit, einen Bericht mit visuellen Elementen zu erstellen.

1. Stellen Sie auf der linken Seite des Fensters sicher, dass Sie die Ansicht **Bericht** ausgewählt haben.

    [![Auswählen der Ansicht „Bericht“](media/how-to-connect-power-bi/select-the-report-view.png)](media/how-to-connect-power-bi/select-the-report-view.png#lightbox)

1.  Wählen Sie in der Spalte **Visualisierungen** das gewünschte visuelle Element aus. Wählen Sie z. B. **Liniendiagramm** aus. Dadurch wird Ihrem Canvas ein leeres Liniendiagramm hinzugefügt.
 
1.  Wählen Sie in der Liste **Felder** die Option **Zeitstempel** aus, und ziehen Sie sie in das Feld **Achse**, um Elemente entlang der X-Achse anzuzeigen.

1.  Wählen Sie erneut in der Liste **Felder** die Option **TimeSeriesId** aus, und ziehen Sie sie in das Feld **Werte**, um Elemente entlang der Y-Achse anzuzeigen.

    [![Erstellen eines Liniendiagramms](media/how-to-connect-power-bi/power-bi-line-chart.png)](media/how-to-connect-power-bi/power-bi-line-chart.png#lightbox)

1.  Um ein weiteres Diagramm zum Canvas hinzuzufügen, klicken Sie an eine beliebige Stelle auf dem Canvas außerhalb des Liniendiagramms, und wiederholen Sie diesen Vorgang.

    [![Erstellen zusätzlicher freizugebender Diagramme](media/how-to-connect-power-bi/power-bi-additional-charts.png)](media/how-to-connect-power-bi/power-bi-additional-charts.png#lightbox)

Nachdem Sie Ihren Bericht erstellt haben, können Sie ihn in Power BI Reporting Services veröffentlichen.

## <a name="advanced-editing"></a>Erweiterte Bearbeitung

Wenn Sie bereits ein Dataset in Power BI geladen haben, die Abfrage aber ändern möchten (z. B. Datum/Uhrzeit oder Umgebungs-ID-Parameter), können Sie dies über die „Erweiterter Editor“-Funktion von Power BI erreichen. Weitere Informationen finden Sie in der [Dokumentation zu Power BI](https://docs.microsoft.com/power-bi/desktop-query-overview).

Als Übersicht:

1. Wählen Sie in Power BI Desktop **Abfragen bearbeiten** aus.
1. Drücken Sie auf **Erweiterter Editor**.

    [![Bearbeiten von Abfragen im erweiterten Editor](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png)](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png#lightbox)

1. Ändern Sie die JSON-Nutzlast nach Bedarf.
1. Wählen Sie **Fertig** und dann **Schließen und übernehmen** im **Fenster des Power Query-Editors** aus.

Sie sollten sehen, dass die gewünschten Änderungen übernommen werden.  

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure Time Series Insights finden Sie unter [Power BI-Connectorkonzepte](https://docs.microsoft.com/power-bi/desktop-query-overview).

* Weitere Informationen zu [Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-query-overview).

* Weitere Informationen finden Sie unter [Time Series Insights GA-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart) und [Time Series Insights Preview-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).
