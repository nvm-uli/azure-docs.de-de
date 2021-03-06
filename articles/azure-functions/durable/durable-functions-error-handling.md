---
title: Fehlerbehandlung in Durable Functions – Azure
description: Erfahren Sie, wie Sie Fehler in der Durable Functions-Erweiterung für Azure Functions behandeln.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 0900e3f3b76f4a82e06fe3c0e6d9bbe63b545f56
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74231405"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Fehlerbehandlung in Durable Functions (Azure Functions)

Durable Function-Orchestrierungen sind im Code implementiert und können die integrierten Funktionen zur Fehlerbehandlung der Programmiersprache nutzen. Daher gibt es eigentlich keine neuen Konzepte, die Sie bei der Einbindung der Fehlerbehandlung und Kompensierung in Ihre Orchestrierungen beachten müssen. Es gibt jedoch einige wenige Verhaltensweisen, die Sie kennen sollten:

## <a name="errors-in-activity-functions"></a>Fehler in Aktivitätsfunktionen

Jede Ausnahme, die in einer Aktivitätsfunktion ausgelöst wird, wird zurück zur Orchestratorfunktion gemarshallt und als `FunctionFailedException` ausgelöst. Sie können Code zur Fehlerbehandlung und zur Kompensierung in der Orchestratorfunktion schreiben, der Ihren Bedürfnissen entspricht.

Betrachten Sie beispielsweise die folgende Orchestratorfunktion, die Guthaben von einem Konto auf ein anderes überträgt:

### <a name="precompiled-c"></a>Vorkompilierter C#-Code

```csharp
[FunctionName("TransferFunds")]
public static async Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        {
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

### <a name="c-script"></a>C#-Skript

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(IDurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        {
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

> [!NOTE]
> Die vorherigen C#-Beispiele gelten für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const transferDetails = context.df.getInput();

    yield context.df.callActivity("DebitAccount",
        {
            account = transferDetails.sourceAccount,
            amount = transferDetails.amount,
        }
    );

    try {
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.destinationAccount,
                amount = transferDetails.amount,
            }
        );
    }
    catch (error) {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.sourceAccount,
                amount = transferDetails.amount,
            }
        );
    }
});
```

Wenn der erste Funktionsaufruf von **CreditAccount** fehlschlägt, wird dies durch die Orchestratorfunktion kompensiert, indem die Gelder auf das Quellkonto zurücküberwiesen werden.

## <a name="automatic-retry-on-failure"></a>Automatische Wiederholung bei einem Fehler

Wenn Sie Aktivitätsfunktionen oder untergeordnete Orchestrierungsfunktionen aufrufen, können Sie eine Richtlinie für automatische Wiederholungen angeben. Im folgenden Beispiel wird versucht, eine Funktion bis zu 3-mal mit je 5 Sekunden Wartezeit zwischen den einzelnen Wiederholungsversuchen aufzurufen:

### <a name="precompiled-c"></a>Vorkompilierter C#-Code

```csharp
[FunctionName("TimerOrchestratorWithRetry")]
public static async Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

### <a name="c-script"></a>C#-Skript

```csharp
public static async Task Run(IDurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

> [!NOTE]
> Die vorherigen C#-Beispiele gelten für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const retryOptions = new df.RetryOptions(5000, 3);

    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

Die `CallActivityWithRetryAsync`-API (.NET) oder die `callActivityWithRetry`-API (JavaScript) erstellt einen `RetryOptions`-Parameter. Untergeordnete Orchestrierungsaufrufe, die die `CallSubOrchestratorWithRetryAsync`-API (.NET) oder `callSubOrchestratorWithRetry`-API (JavaScript) verwenden, können dieselben Wiederholungsrichtlinien verwenden.

Es stehen mehrere Optionen zur Verfügung, um die automatische Wiederholungsrichtlinie anzupassen:

* **Maximale Anzahl von Versuchen**: Die maximale Anzahl der Wiederholungsversuche.
* **Intervall für erste Wiederholung**: Die abzuwartende Zeitspanne bis zum ersten Wiederholungsversuch.
* **Backoff-Koeffizient**: Der Koeffizient, der verwendet wird, um die Rate für die Erhöhung des Backoffs zu bestimmen. Der Standardwert lautet 1.
* **Max. Wiederholungsintervall**: Die maximale Zeitspanne zwischen den Wiederholungsversuchen.
* **Timeout für Wiederholungsversuche**: Die maximale Zeitspanne für das Ausführen von Wiederholungsversuchen. Das Standardverhalten ist das Wiederholen auf unbestimmte Zeit.
* **Handle**: Es kann ein benutzerdefinierter Rückruf angegeben werden, der bestimmt, ob eine Funktion wiederholt werden soll.

## <a name="function-timeouts"></a>Funktion-Timeouts

Vielleicht möchten Sie einen Funktionsaufruf innerhalb einer Orchestratorfunktion verwerfen, wenn der Vorgang zu lange dauert. Die richtige Vorgehensweise ist das Erstellen eines [durable timer](durable-functions-timers.md) (permanenter Timer) mithilfe von `context.CreateTimer` (.NET) oder `context.df.createTimer` (JavaScript) in Verbindung mit `Task.WhenAny` (.NET) oder `context.df.Task.any` (JavaScript), wie im folgenden Beispiel:

### <a name="precompiled-c"></a>Vorkompilierter C#-Code

```csharp
[FunctionName("TimerOrchestrator")]
public static async Task<bool> Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

### <a name="c-script"></a>C#-Skript

```csharp
public static async Task<bool> Run(IDurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

> [!NOTE]
> Die vorherigen C#-Beispiele gelten für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("FlakyFunction");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    } else {
        // timeout case
        return false;
    }
});
```

> [!NOTE]
> Dieser Mechanismus beendet laufende Aktivitätsausführungsfunktionen nicht. Stattdessen lässt er zu, dass die Orchestratorfunktion das Ergebnis ignoriert und fortfährt. Weitere Informationen finden Sie in der Dokumentation zu [Timern](durable-functions-timers.md#usage-for-timeout).

## <a name="unhandled-exceptions"></a>Nicht behandelte Ausnahmen

Fällt eine Orchestratorfunktion mit einer nicht behandelten Ausnahme aus, werden die Details der Ausnahme protokolliert, und die Instanz wird mit einem `Failed`-Status abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu „ewigen“ Orchestrierungen](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Learn how to diagnose problems (Informationen zur Diagnose von Problemen)](durable-functions-diagnostics.md)
