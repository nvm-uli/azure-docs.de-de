---
title: Anfügen an Azure Blockchain Service mithilfe von Geth
description: Anfügen an eine Geth-Instanz in einem Azure Blockchain Service-Transaktionsknoten
ms.date: 11/20/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 9da78eac1dc429bcc0ad52bb9cb2f1fb743a90d4
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455835"
---
# <a name="quickstart-use-geth-to-attach-to-an-azure-blockchain-service-transaction-node"></a>Schnellstart: Anfügen an einen Azure Blockchain Service-Transaktionsknoten mithilfe von Geth

In dieser Schnellstartanleitung verwenden Sie den Geth-Client, um eine Anfügung an eine Geth-Instanz in einem Azure Blockchain Service-Transaktionsknoten durchzuführen. Nach dem Anfügen verwenden Sie die Geth-JavaScript-Konsole, um eine Web3-JavaScript-DApp-API aufzurufen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Geth](https://github.com/ethereum/go-ethereum/wiki/geth).
* [Quickstart: Create a blockchain member using the Azure portal (Schnellstart: Erstellen eines Blockchainmitglieds über das Azure-Portal)](create-member.md) oder [Schnellstart: Erstellen eines Blockchainmitglieds für den Azure Blockchain-Dienst mithilfe der Azure CLI](create-member-cli.md)

## <a name="get-geth-connection-string"></a>Abrufen der Geth-Verbindungszeichenfolge

Sie können die Geth-Verbindungszeichenfolge für einen Azure Blockchain Service-Transaktionsknoten über das Azure-Portal abrufen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu Ihrem Azure Blockchain Service-Mitglied. Klicken Sie auf **Transaktionsknoten** und dann auf den Link „default transaction node“ (Standardtransaktionsknoten).

    ![Standardtransaktionsknoten auswählen](./media/connect-geth/transaction-nodes.png)

1. Wählen Sie **Connection strings** (Verbindungszeichenfolgen).
1. Kopieren Sie die Verbindungszeichenfolge aus **HTTPS (Access key 1)** (HTTPS (Zugriffsschlüssel 1)). Sie benötigen die Zeichenfolge für den nächsten Abschnitt.

    ![Verbindungszeichenfolge](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>Herstellen einer Verbindung mit Geth

1. Öffnen Sie eine Eingabeaufforderung oder Shell.
1. Verwenden Sie den Unterbefehl „geth attach“, um eine Anfügung an die ausgeführte Geth-Instanz auf Ihrem Transaktionsknoten durchzuführen. Fügen Sie die Verbindungszeichenfolge als Argument für den Unterbefehl ein. Beispiel:

    ``` bash
    geth attach <connection string>
    ```

1. Sobald eine Verbindung mit der Ethereum-Konsole des Transaktionsknotens hergestellt wurde, können Sie die Web3-JavaScript- DApp-API oder die Administrator-API aufrufen.

    Mit der folgenden API können Sie beispielsweise den Wert von chainId ermitteln:

    ``` bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    In diesem Beispiel lautet der Wert von chainId 661.

    ![Azure Blockchain-Option](./media/connect-geth/geth-attach.png)

1. Geben Sie `exit` ein, um die Verbindung zur Konsole zu trennen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie den Geth-Client verwendet, um eine Anfügung an eine Geth-Instanz auf einem Azure Blockchain-Transaktionsknoten durchzuführen. Im nächsten Tutorial erfahren Sie, wie Sie das Azure Blockchain Development Kit für Ethereum verwenden, um eine Smart Contract-Funktion über eine Transaktion zu erstellen, bereitstellen und auszuführen.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Bereitstellen von Smart Contracts in Azure Blockchain Service](send-transaction.md)
