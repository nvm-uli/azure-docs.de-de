---
title: Skriptaktion für Python-Pakete mit Jupyter in Azure HDInsight
description: Eine Schritt-für-Schritt-Anleitung zum Verwenden einer Skriptaktion für die Konfiguration von Jupyter Notebooks die mit Spark-Clustern in HDInsight verfügbar sind, sodass sie externe Python-Pakete verwenden.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: a8654f6c9c6c6d020872d2c89e0dd141db4e0451
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215554"
---
# <a name="safely-manage-python-environment-on-azure-hdinsight-using-script-action"></a>Sicheres Verwalten der Python-Umgebung in Azure HDInsight mithilfe einer Skriptaktion

> [!div class="op_single_selector"]
> * [Verwenden von Cell Magic](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Verwenden von Skriptaktionen](apache-spark-python-package-installation.md)

Für den Spark-Cluster verfügt HDInsight mit Anaconda Python 2.7 und Python 3.5 über zwei integrierte Python-Installationen. In einigen Fällen müssen Kunden die Python-Umgebung anpassen, um z.B. externe Python-Pakete oder andere Python-Versionen zu installieren. n diesem Artikel zeigen wir die bewährte Methode zur sicheren Verwaltung von Python-Umgebungen für einen [Apache Spark](https://spark.apache.org/)-Cluster in HDInsight.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]  
   > Wenn Sie noch nicht über einen Spark-Cluster unter HDInsight (Linux) verfügen, können Sie während der Clustererstellung Skriptaktionen ausführen. Informationen zum Verwenden benutzerdefinierter Skriptaktionen finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Unterstützung für Open-Source-Software in HDInsight-Clustern

Der Microsoft Azure HDInsight-Dienst verwendet eine Reihe von Open-Source-Technologien für Apache Hadoop. Microsoft Azure bietet lediglich allgemeinen Support für Open-Source-Technologien. Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/). Der HDInsight-Dienst bietet zusätzliche Unterstützung für integrierte Komponenten.

Es gibt zwei Arten von Open-Source-Komponenten, die im HDInsight-Dienst verfügbar sind:

* **Integrierte Komponenten** – Diese Komponenten sind in HDInsight-Clustern vorinstalliert und stellen Kernfunktionen des Clusters bereit. So gehören beispielsweise Apache Hadoop YARN Resource Manager, die Apache Hive-Abfragesprache (HiveQL) und die Mahout Library zu dieser Kategorie. Eine vollständige Liste der Clusterkomponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Apache Hadoop-Clusterversionen](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Benutzerdefinierte Komponenten** – Als Benutzer des Clusters können Sie in Ihrem Workload eine beliebige in der Community verfügbare oder von Ihnen erstellte Komponente installieren oder verwenden.

> [!IMPORTANT]
> Mit dem HDInsight-Cluster bereitgestellte Komponenten werden vollständig unterstützt. Der Microsoft-Support unterstützt Sie beim Isolieren und Beheben von Problemen im Zusammenhang mit diesen Komponenten.
>
> Für benutzerdefinierte Komponenten steht kommerziell angemessener Support für eine weiterführende Behebung des Problems zur Verfügung. Der Microsoft-Support kann das Problem möglicherweise beheben, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [https://apache.org](https://apache.org). Beispiel: [Hadoop](https://hadoop.apache.org/).

## <a name="understand-default-python-installation"></a>Grundlegendes zur Python-Standardinstallation

Der Spark-Cluster in HDInsight wird mit der Anaconda-Installation erstellt. Im Cluster gibt es zwei Python-Installationen: Anaconda Python 2.7 and Python 3.5. In der folgenden Tabelle sind die Python-Standardeinstellungen für Spark, Livy und Jupyter aufgeführt.

| |Python 2,7|Python 3.5|
|----|----|----|
|`Path`|/usr/bin/anaconda/bin|/usr/bin/anaconda/envs/py35/bin|
|Spark|Standard ist 2.7|–|
|Livy|Standard ist 2.7|–|
|Jupyter|PySpark-Kernel|PySpark3-Kernel|

## <a name="safely-install-external-python-packages"></a>Sicheres Installieren externer Python-Pakete

Der HDInsight-Cluster hängt von der integrierten Python-Umgebung (Python 2.7 oder 3.5) ab. Die direkte Installation von benutzerdefinierten Paketen in diesen standardmäßig integrierten Umgebungen kann zu unerwarteten Änderungen der Bibliotheksversionen führen und den Cluster weiter zerlegen. Um benutzerdefinierte externe Python-Pakete für Ihre Spark-Anwendungen sicher zu installieren, führen Sie die folgenden Schritte aus.

1. Erstellen Sie mithilfe von Conda eine virtuelle Python-Umgebung. Eine virtuelle Umgebung bietet einen isolierten Raum für Ihre Projekte, ohne andere zu beeinträchtigen. Beim Erstellen der virtuellen Python-Umgebung können Sie die Python-Version angeben, die Sie verwenden möchten. Beachten Sie, dass Sie immer noch eine virtuelle Umgebung erstellen müssen, auch wenn Sie Python 2.7 und 3.5 verwenden möchten. Dadurch soll sichergestellt werden, dass die Standardumgebung des Clusters nicht beschädigt wird. Führen Sie Skriptaktionen für alle Knoten in Ihrem Cluster mit dem folgenden Skript aus, um eine virtuelle Python-Umgebung zu erstellen. 

    -   `--prefix` gibt einen Pfad zu einer virtuellen Conda-Umgebung an. Es gibt mehrere Konfigurationen, für die basierend auf dem hier angegebenen Pfad weitere Änderungen vorgenommen werden müssen. In diesem Beispiel wird py35new verwendet, da der Cluster bereits über eine bestehende virtuelle Umgebung namens py35 verfügt.
    -   `python=` gibt die Python-Version für die virtuelle Umgebung an. In diesem Beispiel wird die Version 3.5 verwendet, die gleiche Version wie die bereits im Cluster integrierte. Sie können auch andere Python-Versionen verwenden, um die virtuelle Umgebung zu erstellen.
    -   `anaconda` legt package_spec als Anaconda fest, um Anaconda-Pakete in der virtuellen Umgebung zu installieren.
    
    ```bash
    sudo /usr/bin/anaconda/bin/conda create --prefix /usr/bin/anaconda/envs/py35new python=3.5 anaconda --yes 
    ```

2. Installieren Sie bei Bedarf in der erstellten virtuellen Umgebung externe Python-Pakete. Führen Sie Skriptaktionen für alle Knoten in Ihrem Cluster mit dem folgenden Skript aus, um externe Python-Pakete zu installieren. Sie müssen hier über sudo-Berechtigungen verfügen, um Dateien in den Ordner der virtuellen Umgebung zu schreiben.

    Sie können den [Paketindex](https://pypi.python.org/pypi) nach einer vollständigen Liste mit verfügbaren Paketen durchsuchen. Sie können die Liste der verfügbaren Pakete auch aus anderen Quellen abrufen. So können Sie beispielsweise Pakete installieren, die über [conda-forge](https://conda-forge.org/feedstocks/) verfügbar gemacht wurden.

    -   `seaborn` ist der Name des Pakets, das Sie installieren möchten.
    -   `-n py35new` gibt den Namen der virtuellen Umgebung an, die gerade erstellt wird. Stellen Sie sicher, dass Sie den Namen entsprechend der von Ihnen erstellten virtuellen Umgebung ändern.

    ```bash
    sudo /usr/bin/anaconda/bin/conda install seaborn -n py35new --yes
    ```

    Wenn Sie den Namen der virtuellen Umgebung nicht kennen, können Sie SSH an den Headerknoten des Clusters senden und `/usr/bin/anaconda/bin/conda info -e` ausführen, um alle virtuellen Umgebungen anzuzeigen.

3. Ändern Sie die Spark- und Livy-Konfigurationen, und zeigen Sie auf die erstellte virtuelle Umgebung.

    1. Öffnen Sie die Ambari-Benutzeroberfläche, und wechseln Sie zur Spark2-Seite auf die Registerkarte „Konfigurationen“.
    
        ![Ändern der Spark- und Livy-Konfiguration über Ambari](./media/apache-spark-python-package-installation/ambari-spark-and-livy-config.png)
 
    2. Erweitern Sie „livy2-env“, und fügen Sie die unten aufgeführten Anweisungen unten hinzu. Wenn Sie die virtuelle Umgebung mit einem anderen Präfix installiert haben, ändern Sie den Pfad entsprechend.

        ```
        export PYSPARK_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        export PYSPARK_DRIVER_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        ```

        ![Ändern der Livy-Konfiguration über Ambari](./media/apache-spark-python-package-installation/ambari-livy-config.png)

    3. Erweitern Sie „Advanced spark2-env“, und ersetzen Sie unten die vorhandene PYSPARK_PYTHON-Anweisung für den Export. Wenn Sie die virtuelle Umgebung mit einem anderen Präfix installiert haben, ändern Sie den Pfad entsprechend.

        ```
        export PYSPARK_PYTHON=${PYSPARK_PYTHON:-/usr/bin/anaconda/envs/py35new/bin/python}
        ```

        ![Ändern der Spark-Konfiguration über Ambari](./media/apache-spark-python-package-installation/ambari-spark-config.png)

    4. Speichern Sie die Änderungen, und starten Sie die betreffenden Dienste neu. Damit diese Änderungen übernommen werden, müssen Sie den Spark2-Dienst neu starten. Über die Ambari-Benutzeroberfläche werden Sie zu einem Neustart aufgefordert. Klicken Sie daher auf „Neu starten“, um alle betreffenden Dienste neu zu starten.

        ![Ändern der Spark-Konfiguration über Ambari](./media/apache-spark-python-package-installation/ambari-restart-services.png)
 
4.  Wenn Sie die neu erstellte virtuelle Umgebung unter Jupyter verwenden möchten, müssen Sie die Jupyter-Konfigurationen ändern und Jupyter neu starten. Führen Sie Skriptaktionen für alle Headerknoten mit der folgenden Anweisung aus, um Jupyter auf die neu erstellte virtuelle Umgebung zu verweisen. Stellen Sie sicher, dass Sie den Pfad zu dem Präfix ändern, das Sie für Ihre virtuelle Umgebung angegeben haben. Nachdem Sie diese Skriptaktion ausgeführt haben, starten Sie den Jupyter-Dienst über die Ambari-Benutzeroberfläche neu, um diese Änderung zu übernehmen.

    ```
    sudo sed -i '/python3_executable_path/c\ \"python3_executable_path\" : \"/usr/bin/anaconda/envs/py35new/bin/python3\"' /home/spark/.sparkmagic/config.json
    ```

    Sie können die Python-Umgebung in Jupyter Notebook doppelt bestätigen, indem Sie den folgenden Code ausführen:

    ![Überprüfen der Python-Version in Jupyter Notebook](./media/apache-spark-python-package-installation/check-python-version-in-jupyter.png)

## <a name="known-issue"></a>Bekannte Probleme

Es gibt einen bekannten Fehler in Anaconda-Version 4.7.11 und 4.7.12. Wenn Ihre Skriptaktionen bei `"Collecting package metadata (repodata.json): ...working..."` hängen und mit `"Python script has been killed due to timeout after waiting 3600 secs"` fehlschlagen, können Sie [dieses Skript](https://gregorysfixes.blob.core.windows.net/public/fix-conda.sh) herunterladen und damit Skriptaktionen für alle Knoten ausführen, um das Problem zu beheben.

Wenn Sie wissen möchten, welche Anaconda-Version Sie verwenden, können Sie SSH auf dem Clusterheaderknoten anwenden und `/usr/bin/anaconda/bin/conda --v` ausführen.

## <a name="seealso"></a>Weitere Informationen

* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen

* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden externer Pakete mit Jupyter Notebooks in Apache Spark-Clustern unter HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](apache-spark-intellij-tool-plugin.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Apache Spark-Anwendungen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Apache Zeppelin Notebooks mit einem Apache Spark-Cluster unter HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel für Jupyter Notebook in Apache Spark-Clustern für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
