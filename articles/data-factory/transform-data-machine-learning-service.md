---
title: Ausführen von Azure Machine Learning-Pipelines
description: Hier erfahren Sie, wie Sie Ihre Azure Machine Learning-Pipelines in Ihren Azure Data Factory-Pipelines ausführen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: daperlov
author: djpmsft
manager: anandsub
ms.date: 10/10/2019
ms.openlocfilehash: ef630486860def2781634926f4e9385cd030c171
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74913238"
---
# <a name="execute-azure-machine-learning-pipelines-in-azure-data-factory"></a>Ausführen von Azure Machine Learning-Pipelines in Azure Data Factory-Pipelines

Führen Sie Ihre Azure Machine Learning-Pipelines als Schritt in Ihren Azure Data Factory-Pipelines aus. Die Machine Learning-Pipelineausführungsaktivität ermöglicht Batchvorhersageszenarien wie etwa die Ermittlung möglicher Kreditausfälle, die Ermittlung der Stimmung oder die Analyse von Kundenverhaltensmustern.

## <a name="syntax"></a>Syntax

```json
{
    "name": "Machine Learning Execute Pipeline",
    "type": "AzureMLExecutePipeline",
    "linkedServiceName": {
        "referenceName": "AzureMLService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mlPipelineId": "machine learning pipeline ID",
        "experimentName": "experimentName",
        "mlPipelineParameters": {
            "mlParameterName": "mlParameterValue"
        }
    }
}

```

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
name | Name der Aktivität in der Pipeline | Zeichenfolge | Ja
type | Die Art der Aktivität lautet „AzureMLExecutePipeline“. | Zeichenfolge | Ja
linkedServiceName | Mit Azure Machine Learning verknüpfter Dienst | Verweis auf den verknüpften Dienst | Ja
mlPipelineId | ID der veröffentlichten Azure Machine Learning-Pipeline | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Ja
experimentName | Name des Ausführungsverlaufexperiments der Machine Learning-Pipelineausführung | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein
mlPipelineParameters | Schlüssel-Wert-Paare, die an den veröffentlichten Azure Machine Learning-Pipeline-Endpunkt übergeben werden sollen. Die Schlüssel müssen den Namen von Pipelineparametern entsprechen, die in der veröffentlichten Machine Learning-Pipeline definiert sind. | Objekt mit Schlüssel-Wert-Paaren (oder Ausdruck mit resultType-Objekt) | Nein
mlParentRunId | ID der übergeordneten Azure Machine Learning-Pipelineausführung | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein
continueOnStepFailure | Gibt an, ob die Ausführung weiterer Schritte in der Machine Learning-Pipelineausführung fortgesetzt werden soll, wenn ein Schritt nicht erfolgreich war. | boolean | Nein

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden:

* [Ausführen der Datenflussaktivität](control-flow-execute-data-flow-activity.md)
* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)
