---
title: 'Erstellen einer Funktions-App mit verbundenem Speicher: Azure CLI'
description: 'Azure CLI-Skriptbeispiel: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Storage herstellt'
ms.topic: sample
ms.date: 04/20/2017
ms.custom: mvc
ms.openlocfilehash: 9c2a70066e673bfa164a58884891f29e220c7bd6
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74532831"
---
# <a name="create-a-function-app-with-a-named-storage-account-connection"></a>Erstellen einer Funktions-App mit einer benannten Speicherkontoverbindung 

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und stellt eine Verbindung zwischen der Funktion und einem Azure Storage-Konto her. Die erstellte App-Einstellung mit der Verbindung kann mit einem [Storage-Trigger oder einer Storage-Bindung](../functions-bindings-storage-blob.md) verwendet werden. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Verwenden Sie im Fall der lokalen Befehlszeilenschnittstelle mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und fügt einer App-Einstellung die Speicher-Verbindungszeichenfolge hinzu.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellen Sie eine Ressourcengruppe mit einem Speicherort. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellen Sie ein Speicherkonto. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im serverlosen [Verbrauchsplan](../functions-scale.md#consumption-plan). |
| [az storage account show-connection-string](/cli/azure/storage/account#az-storage-account-show-connection-string) | Ruft die Verbindungszeichenfolge für das Konto ab. |
| [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | Legt die Verbindungszeichenfolge als App-Einstellung in der Funktions-App fest. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
