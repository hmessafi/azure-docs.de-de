---
title: Azure Service Bus als Event Grid-Quelle
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für Service Bus-Ereignisse bereitgestellt werden.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 81293321b3a8fb989023a231c905996b4059bd81
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86121133"
---
# <a name="azure-service-bus-as-an-event-grid-source"></a>Azure Service Bus als Event Grid-Quelle

In diesem Artikel werden die Eigenschaften und das Schema für Service Bus-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

## <a name="event-grid-event-schema"></a>Event Grid-Ereignisschema

### <a name="available-event-types"></a>Verfügbare Ereignistypen

Service Bus gibt die folgenden Ereignistypen aus:

| Ereignistyp | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | Wird ausgelöst, wenn aktive Nachrichten in einer Warteschlange oder in einem Abonnement vorliegen und kein lauschender Empfänger vorhanden ist. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | Wird ausgelöst, wenn aktive Nachrichten in einer Warteschlange für unzustellbare Nachrichten vorliegen und kein lauschender Empfänger vorhanden ist. |

### <a name="example-event"></a>Beispielereignis

Das folgende Beispiel zeigt das Schema für ein Ereignis mit aktiven Nachrichten ohne Listener:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Das Schema für ein Ereignis mit Warteschlange für unzustellbare Nachrichten sieht ganz ähnlich aus:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| topic | Zeichenfolge | Vollständiger Ressourcenpfaf zur Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | Zeichenfolge | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | Zeichenfolge | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | Zeichenfolge | Eindeutiger Bezeichner für das Ereignis. |
| data | Objekt (object) | Ereignisdaten für Blob Storage. |
| dataVersion | Zeichenfolge | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | Zeichenfolge | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| namespaceName | Zeichenfolge | Der Service Bus-Namespace, in dem sich die Ressource befindet. |
| requestUri | Zeichenfolge | Der URI für die spezifische Warteschlange oder das Abonnement, von der bzw. von dem das Ereignis ausgegeben wird. |
| entityType | Zeichenfolge | Die Art der Service Bus-Entität, die Ereignisse ausgibt (Warteschlange oder Abonnement). |
| queueName | Zeichenfolge | Die Warteschlange mit aktiven Nachrichten, falls eine Warteschlange abonniert wird. Bei Verwendung von Themen/Abonnements ist der Wert NULL. |
| topicName | Zeichenfolge | Das Thema, zu dem das Service Bus-Abonnement mit aktiven Nachrichten gehört. Bei Verwendung einer Warteschlange ist der Wert NULL. |
| subscriptionName | Zeichenfolge | Das Service Bus-Abonnement mit aktiven Nachrichten. Bei Verwendung einer Warteschlange ist der Wert NULL. |

## <a name="tutorials-and-how-tos"></a>Tutorials und Anleitungen
|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Tutorial: Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |
| [Integration von Azure Service Bus in Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Übersicht über die Integration von Service Bus in Event Grid. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
* Ausführlichere Informationen zur Verwendung von Azure Event Grid mit Service Bus finden Sie in der [Übersicht über die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* Versuchen Sie, [Service Bus-Ereignisse mit Functions und Logic Apps zu empfangen](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json).
