---
title: Azure-Funktion als Ereignishandler für Azure Event Grid-Ereignisse
description: Beschreibt, wie Sie Azure-Funktionen als Ereignishandler für Event Grid-Ereignisse verwenden können.
ms.topic: conceptual
ms.date: 09/18/2020
ms.openlocfilehash: cd500eed180096388eede96f768f08b896ca6456
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91873726"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Azure-Funktion als Ereignishandler für Event Grid-Ereignisse

Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses eine Maßnahme. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Functions** ist einer dieser Dienste. 


Um eine Azure-Funktion als Handler für Ereignisse zu verwenden, verfolgen Sie einen der folgenden Ansätze: 

-   Verwenden eines [Event Grid-Triggers](../azure-functions/functions-bindings-event-grid-trigger.md).  Angeben von **Azure-Funktion** als **Endpunkttyp**. Geben Sie dann die Azure-Funktions-App und die Funktion an, die Ereignisse verarbeitet. 
-   Verwenden eines [HTTP-Triggers](../azure-functions/functions-bindings-http-webhook.md).  Geben Sie **Webhook** als **Endpunkttyp** an. Geben Sie dann die URL für die Azure-Funktion an, die Ereignisse verarbeitet. 

Es wird empfohlen, den ersten Ansatz (Event Grid-Trigger) zu verwenden, da er im Vergleich zum zweiten Ansatz folgende Vorteile bietet:
-   Event Grid überprüft Event Grid-Trigger automatisch. Bei generischen HTTP-Triggern müssen Sie die [Überprüfungsantwort](webhook-event-delivery.md) selbst implementieren.
-   Event Grid passt die Rate, mit der Ereignisse übermittelt werden, automatisch an eine Funktion an, die von einem Event Grid-Ereignis ausgelöst wird, basierend auf der erkannten Rate, mit der die Funktion Ereignisse verarbeiten kann. Diese Ratenübereinstimmungsfunktion verhindert Übermittlungsfehler, die sich aus der Unfähigkeit einer Funktion ergeben, Ereignisse zu verarbeiten, da die Ereignisverarbeitungsrate der Funktion im Laufe der Zeit variieren kann. Um die Effizienz bei hohem Durchsatz zu verbessern, aktivieren Sie Batchverarbeitung für das Ereignisabonnement. Weitere Informationen finden Sie unter [Aktivieren von Batchverarbeitung](#enable-batching).

    > [!NOTE]
    > Zurzeit kann für eine Azure Functions-App kein Event Grid-Trigger verwendet werden, wenn das Ereignis im **CloudEvents**-Schema übermittelt wird. Verwenden Sie stattdessen einen HTTP-Trigger.

## <a name="tutorials"></a>Tutorials

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Schnellstart: Verarbeiten von Ereignissen mit Funktionen](custom-event-to-function.md) | Beschreibt das Senden eines benutzerdefinierten Ereignisses an eine Funktion, damit dieses verarbeitet wird |
| [Tutorial: Automatisieren der Größenänderung von hochgeladenen Images per Event Grid](resize-images-on-storage-blob-upload-event.md) | Benutzer laden Bilder über eine Web-App in ein Speicherkonto hoch. Wenn ein Speicherblob erstellt wird, sendet Event Grid ein Ereignis an die Funktions-App, die daraufhin die Größe des hochgeladenen Bilds anpasst. |
| [Tutorial: Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md) | Wenn Event Hubs eine Capture-Datei erstellt, sendet Event Grid ein Ereignis an eine Funktions-App. Die App ruft die Capture-Datei ab und migriert Daten zu einem Data Warehouse. |
| [Tutorial: Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |

## <a name="rest-example-for-put"></a>REST-Beispiel (für PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "AzureFunction",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Web/sites/<FUNCTION APP NAME>/functions/<FUNCTION NAME>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 6400
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="enable-batching"></a>Aktivieren von Batchverarbeitung
Aktivieren Sie für höheren Durchsatz Batchverarbeitung für das Abonnement. Wenn Sie das Azure-Portal verwenden, können Sie die maximale Anzahl von Ereignissen pro Batch und die bevorzugte Batchgröße in KB zum Zeitpunkt der Erstellung eines Abonnements oder nach der Erstellung festlegen. 

Sie können Batcheinstellungen über das Azure-Portal, mit PowerShell, mit der CLI oder mit einer Resource Manager-Vorlage erstellen. 

### <a name="azure-portal"></a>Azure-Portal
Navigieren Sie zum Zeitpunkt der Erstellung eines Abonnements in der Benutzeroberfläche auf der Seite **Ereignisabonnement erstellen** zur Registerkarte **Erweiterte Funktionen**, und legen Sie Werte für **Max. Anzahl von Ereignissen pro Batch** und **Bevorzugte Batchgröße in KB** fest. 
    
:::image type="content" source="./media/custom-event-to-function/enable-batching.png" alt-text="Aktivieren der Batchverarbeitung zum Zeitpunkt der Erstellung eines Abonnements":::

Sie können diese Werte für ein vorhandenes Abonnement auf der Registerkarte **Funktionen** der Seite **Event Grid-Thema** aktualisieren. 

:::image type="content" source="./media/custom-event-to-function/features-batch-settings.png" alt-text="Aktivieren der Batchverarbeitung zum Zeitpunkt der Erstellung eines Abonnements":::

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
Sie können **maxEventsPerBatch** und **preferredBatchSizeInKilobytes** in einer Azure Resource Manager-Vorlage festlegen. Weitere Informationen finden Sie unter [Microsoft.EventGrid eventSubscriptions-Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/eventsubscriptions).

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie können den Befehl [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create&preserve-view=true) oder [az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update&preserve-view=true) verwenden, um batchbezogene Einstellungen mithilfe der folgenden Parameter zu konfigurieren: `--max-events-per-batch` oder `--preferred-batch-size-in-kilobytes`.

### <a name="azure-powershell"></a>Azure PowerShell
Sie können das Cmdlet [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) oder [Update-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/update-azeventgridsubscription) zum Konfigurieren von batchbezogenen Einstellungen mithilfe der folgenden Parameter verwenden: `-MaxEventsPerBatch` oder `-PreferredBatchSizeInKiloBytes`.

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 
