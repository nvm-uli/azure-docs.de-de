---
title: Verbinden von Zscaler-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Zscaler-Daten mit Azure Sentinel verbinden.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2019
ms.author: rkarlin
ms.openlocfilehash: 45351cc29b2b7028863aff06ab5a511674604d6f
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048961"
---
# <a name="connect-zscaler-internet-access-to-azure-sentinel"></a>Verbinden von Zscaler Internet Access mit Azure Sentinel

> [!IMPORTANT]
> Der Zscaler-Datenconnector in Azure Sentinel ist derzeit als öffentliche Vorschau verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel wird erläutert, wie Sie die Zscaler Internet Access-Appliance mit Azure Sentinel verbinden. Der Zscaler-Datenconnektor ermöglicht es Ihnen, Ihre ZIA-Protokolle (Zscaler Internet Access) auf einfache Weise mit Azure Sentinel zu verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. Die Verwendung von Zscaler für Azure Sentinel bietet Ihnen mehr Einblicke in die Internetnutzung Ihrer Organisation und erweitert die Funktionen für Sicherheitsvorgänge. 


## <a name="how-it-works"></a>So funktioniert's

Sie müssen einen Agent auf einem dedizierten Linux-Computer (VM oder lokal) bereitstellen, um die Kommunikation zwischen Zscaler Internet Access und Azure Sentinel zu ermöglichen. Die folgende Abbildung beschreibt die Einrichtung für eine Linux-VM in Azure.

 ![CEF in Azure](./media/connect-cef/cef-syslog-azure.png)

Alternativ gilt diese Einrichtung, wenn Sie einen virtuellen Computer in einer anderen Cloud oder einen lokalen Computer verwenden. 

 ![CEF lokal](./media/connect-cef/cef-syslog-onprem.png)


## <a name="security-considerations"></a>Sicherheitshinweise

Stellen Sie sicher, dass Sie die Sicherheit des Computers gemäß der Sicherheitsrichtlinie Ihrer Organisation konfigurieren. Beispielsweise können Sie das Netzwerk so konfigurieren, dass es der Sicherheitsrichtlinie für das Unternehmensnetzwerk entspricht, und die Ports und Protokolle im Daemon Ihren Anforderungen entsprechend ändern. Anhand der folgenden Anleitungen können Sie die Sicherheitskonfiguration Ihrer Computer verbessern:   [Sichern virtueller Computer in Azure](../virtual-machines/linux/security-policy.md), [Bewährte Methoden für die Netzwerksicherheit](../security/fundamentals/network-best-practices.md).

Um die TLS-Kommunikation zwischen der Sicherheitslösung und dem Syslog-Computer zu verwenden, müssen Sie den Syslog-Daemon (rsyslog oder syslog-ng) für die Kommunikation in TLS konfigurieren: [Verschlüsseln von Syslog-Datenverkehr mit TLS – rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [Verschlüsseln von Protokollmeldungen mit TLS – syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).

 
## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass auf dem Linux-Computer, den Sie als Proxy verwenden, eines der folgenden Betriebssysteme ausgeführt wird:

- 64 Bit
  - CentOS 6 und 7
  - Amazon Linux 2017.09
  - Oracle Linux 6 und 7
  - Red Hat Enterprise Linux Server 6 und 7
  - Debian GNU/Linux 8 und 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS und 18.04 LTS
  - SUSE Linux Enterprise Server 12
- 32 Bit
   - CentOS 6
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 6
   - Debian GNU/Linux 8 und 9
   - Ubuntu Linux 14.04 LTS und 16.04 LTS
 
 - Daemonversionen
   - Syslog-ng: 2.1 bis 3.22.1
   - Rsyslog: v8
  
 - Unterstützte Syslog-RFCs
   - Syslog RFC 3164
   - Syslog RFC 5424
 
Stellen Sie sicher, dass Ihr Computer auch die folgenden Anforderungen erfüllt: 
- Berechtigungen
    - Sie müssen über erhöhte Berechtigungen (sudo) auf dem Computer verfügen. 
- Softwareanforderungen
    - Stellen Sie sicher, dass Python auf dem Computer ausgeführt wird.
## <a name="step-1-deploy-the-agent"></a>SCHRITT 1: Bereitstellen des Agents

In diesem Schritt müssen Sie den Linux-Computer auswählen, der als Proxy zwischen Azure Sentinel und Ihrer Sicherheitslösung fungieren soll. Sie müssen auf dem Proxycomputer ein Skript ausführen, das die folgenden Aufgaben übernimmt:
- Installieren des Log Analytics-Agents und Konfigurieren des Agents, um auf Syslog-Nachrichten an Port 514 über TCP zu lauschen und die CEF-Nachrichten an Ihren Azure Sentinel-Arbeitsbereich zu senden.
- Konfigurieren des Syslog-Daemons, um CEF-Nachrichten mithilfe von Port 25226 an den Log Analytics-Agent weiterzuleiten.
- Festlegen des Syslog-Agents für das Erfassen von Daten und sicheres Senden der Daten an Log Analytics, um sie dort zu analysieren und anzureichern.
 
 
1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie **Zscaler** und dann **Connectorseite öffnen** aus. 

1. Wählen Sie unter **Syslog-Agent installieren und konfigurieren** Ihren Computertyp aus: Azure, andere Cloud oder lokal. 
   > [!NOTE]
   > Da das Skript im nächsten Schritt den Log Analytics-Agent installiert und den Computer mit Ihrem Azure Sentinel-Arbeitsbereich verbindet, stellen Sie sicher, dass dieser Computer nicht mit einem anderen Arbeitsbereich verbunden ist.
1. Sie müssen über erhöhte Berechtigungen (sudo) auf dem Computer verfügen. Stellen Sie mithilfe des folgenden Befehls sicher, dass Sie auf dem Computer über Python verfügen: `python –version`

1. Führen Sie das folgende Skript auf dem Proxycomputer aus.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Wenn das Skript ausgeführt wird, stellen Sie sicher, dass keine Fehler- oder Warnmeldungen angezeigt werden.


## <a name="step-2-configure-your-zscaler-to-send-cef-messages"></a>SCHRITT 2: Konfigurieren von Zscaler zum Senden von CEF-Nachrichten

1. Auf der Zscaler-Appliance müssen Sie folgende Werte festlegen, damit die erforderlichen Protokolle von der Appliance im erforderlichen Format an den Azure Sentinel-Syslog-Agent (der auf dem Log Analytics-Agent basiert) gesendet werden. Sie können diese Parameter in Ihrer Appliance ändern, sofern Sie diese im Syslog-Daemon auf dem Azure Sentinel-Agent ändern.
    - Protokoll = TCP
    - Port = 514
    - Format = CEF
    - IP-Adresse: Achten Sie darauf, die CEF-Nachrichten an die IP-Adresse des virtuellen Computers zu senden, den Sie für diesen Zweck reserviert haben.
 Weitere Informationen finden Sie im [Leitfaden zur Bereitstellung von Zscaler in Azure Sentinel](https://aka.ms/ZscalerCEFInstructions).
 
   > [!NOTE]
   > Diese Lösung unterstützt Syslog RFC 3164 und RFC 5424.


1. Um das relevante Schema in Log Analytics für die CEF-Ereignisse zu verwenden, suchen Sie nach `CommonSecurityLog`.

## <a name="step-3-validate-connectivity"></a>SCHRITT 3: Überprüfen der Konnektivität

1. Öffnen Sie Log Analytics, um sicherzustellen, dass Protokolle mithilfe des CommonSecurityLog-Schemas empfangen werden.<br> Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

1. Vor dem Ausführen des Skripts empfiehlt es sich, Nachrichten aus Ihrer Sicherheitslösung zu senden, um sicherzustellen, dass sie an den von Ihnen konfigurierten Syslog-Proxycomputer weitergeleitet werden. 
1. Sie müssen über erhöhte Berechtigungen (sudo) auf dem Computer verfügen. Stellen Sie mithilfe des folgenden Befehls sicher, dass Sie auf dem Computer über Python verfügen: `python –version`
1. Führen Sie das folgende Skript aus, um die Konnektivität zwischen Agent, Azure Sentinel und Ihrer Sicherheitslösung zu überprüfen. Es überprüft, ob die Daemonweiterleitung ordnungsgemäß konfiguriert ist, an den richtigen Ports gelauscht wird und die Kommunikation zwischen dem Daemon und dem Log Analytics-Agent nicht blockiert wird. Das Skript sendet auch Pseudonachrichten „TestCommonEventFormat“, um die End-to-End-Konnektivität zu überprüfen. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Zscaler Internet Access mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

