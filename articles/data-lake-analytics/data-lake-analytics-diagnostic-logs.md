---
title: Aktivieren und Anzeigen von Diagnoseprotokollen für Azure Data Lake Analytics
description: Enthält eine Beschreibung der Grundlagen zum Einrichten von Diagnoseprotokollen und zum damit verbundenen Zugriff für Azure Data Lake Analytics.
services: data-lake-analytics
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 02/12/2018
ms.openlocfilehash: f1f4320f0bfb924883eb7ae4807dcb714cd89983
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91331929"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics

Die Diagnoseprotokollierung ermöglicht Ihnen das Erfassen von Zugriffsüberwachungspfaden. Diese Protokolle enthalten beispielsweise folgende Informationen:

* Liste der Benutzer, die auf die Daten zugegriffen haben
* Häufigkeit des Datenzugriffs
* Menge der im Konto gespeicherten Daten

## <a name="enable-logging"></a>Aktivieren der Protokollierung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Öffnen Sie Ihr Data Lake Analytics-Konto, und wählen Sie im Abschnitt __Überwachung__ die Option **Diagnoseprotokolle** aus. Wählen Sie anschließend __Diagnose aktivieren__.

    ![Der Screenshot zeigt, dass die Aktion „Diagnoseprotokolle“ aktiviert und die Aktion „Diagnosen zum Erfassen der folgenden Protokolle aktivieren“ hervorgehoben ist.](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Geben Sie unter __Diagnoseeinstellungen__ einen __Namen__ für diese Protokollierungskonfiguration ein, und wählen Sie dann Protokollierungsoptionen aus.

    ![Aktivieren der Diagnose zum Sammeln von Überwachungs- und Anforderungsprotokollen](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

   * Sie können die Daten auf drei Arten speichern bzw. verarbeiten.

     * Wählen Sie __In einem Speicherkonto archivieren__, um Protokolle in einem Azure-Speicherkonto zu speichern. Verwenden Sie diese Option, wenn Sie die Daten archivieren möchten. Bei Auswahl dieser Option müssen Sie ein Azure-Speicherkonto zum Speichern der Protokolle angeben.

     * Wählen Sie **An einen Event Hub streamen**, um die Protokolldaten an einen Azure Event Hub zu streamen. Wählen Sie diese Option, wenn Sie eine nachgelagerte Verarbeitungspipeline einsetzen, die eingehende Protokolle in Echtzeit analysiert. Wenn Sie diese Option auswählen, müssen Sie die Details für den Azure Event Hub angeben, den Sie verwenden möchten.

     * Wählen Sie __An Log Analytics senden__ aus, um die Daten an den Azure Monitor-Dienst zu senden. Verwenden Sie diese Option, wenn Sie Azure Monitor-Protokolle zum Erfassen und Analysieren von Protokollen verwenden möchten.
   * Geben Sie an, ob Sie Überwachungsprotokolle oder Anforderungsprotokolle oder beides abrufen möchten.  Ein Anforderungsprotokoll erfasst jede API-Anforderung. Ein Überwachungsprotokoll zeichnet alle Vorgänge auf, die von dieser API-Anforderung ausgelöst werden.

   * Geben Sie für __In einem Speicherkonto archivieren__ an, wie viele Tage lang die Daten beibehalten werden sollen.

   * Klicken Sie auf __Speichern__.

        > [!NOTE]
        > Wählen Sie entweder __In einem Speicherkonto archivieren__, __An einen Event Hub streamen__ oder __An Log Analytics senden__ aus, bevor Sie auf die Schaltfläche __Speichern__ klicken.

### <a name="use-the-azure-storage-account-that-contains-log-data"></a>Im Azure Storage-Konto, das die Protokolldaten enthält

1. Wenn Sie die Blobcontainer mit Protokollierungsdaten anzeigen möchten, öffnen Sie das für Data Lake Analytics für die Protokollierung verwendete Azure Storage-Konto, und klicken Sie auf __Blobs__.

   * Der Container **insights-logs-audit** enthält die Überwachungsprotokolle.
   * Der Container **insights-logs-requests** enthält die Anforderungsprotokolle.

2. Innerhalb der Container werden die Protokolle in der folgenden Dateistruktur gespeichert:

   ```text
   resourceId=/
     SUBSCRIPTIONS/
       <<SUBSCRIPTION_ID>>/
         RESOURCEGROUPS/
           <<RESOURCE_GRP_NAME>>/
             PROVIDERS/
               MICROSOFT.DATALAKEANALYTICS/
                 ACCOUNTS/
                   <DATA_LAKE_ANALYTICS_NAME>>/
                     y=####/
                       m=##/
                         d=##/
                           h=##/
                             m=00/
                               PT1H.json
   ```

   > [!NOTE]
   > Auf dem Blatt `##` -Einträge im Pfad enthalten Jahr, Monat, Tag und Stunde der Protokollerstellung. Data Lake Analytics erstellt eine Datei pro Stunde, sodass `m=` immer den Wert `00` enthält.

    Der vollständige Pfad zu einem Überwachungsprotokoll kann beispielsweise wie folgt lauten:

    `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Entsprechend kann der vollständige Pfad zu einem Anforderungsprotokoll wie folgt lauten:

    `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="log-structure"></a>Protokollstruktur

Die Überwachungs- und Anforderungsprotokolle liegen in einem strukturierten JSON-Format vor.

### <a name="request-logs"></a>Anforderungsprotokolle

Hier ist ein Beispiel für einen Eintrag im JSON-formatierten Anforderungsprotokoll. Jedes Blob hat ein Stammobjekt namens **records** , das ein Array mit Protokollobjekten enthält.

```json
{
"records":
  [
    . . . .
    ,
    {
         "time": "2016-07-07T21:02:53.456Z",
         "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
         "category": "Requests",
         "operationName": "GetAggregatedJobHistory",
         "resultType": "200",
         "callerIpAddress": "::ffff:1.1.1.1",
         "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
         "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
         "properties": {
             "HttpMethod":"POST",
             "Path":"/JobAggregatedHistory",
             "RequestContentLength":122,
             "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
             "StartTime":"2016-07-07T21:02:52.472Z",
             "EndTime":"2016-07-07T21:02:53.456Z"
             }
    }
    ,
    . . . .
  ]
}
```

#### <a name="request-log-schema"></a>Anforderungsprotokollschema

| Name | type | BESCHREIBUNG |
| --- | --- | --- |
| time |String |Der Zeitstempel (UTC) des Protokolls. |
| resourceId |String |Die ID der Ressource, für die der Vorgang erfolgt ist |
| category |String |Die Protokollkategorie. Beispiel: **Anforderungen**. |
| operationName |String |Der Name des protokollierten Vorgangs. Beispiel: GetAggregatedJobHistory. |
| resultType |String |Der Status des Vorgangs, beispielsweise 200. |
| callerIpAddress |String |Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| correlationId |String |Der Bezeichner des Protokolls. Dieser Wert kann verwendet werden, um einen Satz zusammengehöriger Protokolleinträgen zu gruppieren. |
| identity |Object |Die Identität, die das Protokoll erstellt hat. |
| properties |JSON |Details hierzu finden Sie im nächsten Abschnitt (Eigenschaftenschema des Anforderungsprotokolls). |

#### <a name="request-log-properties-schema"></a>Eigenschaftenschema des Anforderungsprotokolls

| Name | type | BESCHREIBUNG |
| --- | --- | --- |
| HttpMethod |String |Die HTTP-Methode, die für den Vorgang verwendet werden. Beispiel: GET. |
| `Path` |String |Der Pfad, in dem der Vorgang durchgeführt wurde. |
| RequestContentLength |INT |Die Inhaltslänge der HTTP-Anforderung. |
| ClientRequestId |String |Der Bezeichner, der diese Anforderung eindeutig identifiziert |
| StartTime |String |Der Zeitpunkt, zu dem der Server die Anforderung empfangen hat. |
| EndTime |String |Der Zeitpunkt, zu dem der Server eine Antwort gesendet hat. |

### <a name="audit-logs"></a>Überwachungsprotokolle

Hier ist ein Beispiel für einen Eintrag im JSON-formatierten Überwachungsprotokoll. Jedes Blob hat ein Stammobjekt namens **records** , das ein Array mit Protokollobjekten enthält.

```json
{
"records":
  [
    {
         "time": "2016-07-28T19:15:16.245Z",
         "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
         "category": "Audit",
         "operationName": "JobSubmitted",
         "identity": "user@somewhere.com",
         "properties": {
             "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
             "JobName": "New Job",
             "JobRuntimeName": "default",
             "SubmitTime": "7/28/2016 7:14:57 PM"
             }
    }
  ]
}
```

#### <a name="audit-log-schema"></a>Überwachungsprotokollschema

| Name | type | BESCHREIBUNG |
| --- | --- | --- |
| time |String |Der Zeitstempel (UTC) des Protokolls. |
| resourceId |String |Die ID der Ressource, für die der Vorgang erfolgt ist |
| category |String |Die Protokollkategorie. Beispiel: **Überwachung**. |
| operationName |String |Der Name des protokollierten Vorgangs. Beispiel: JobSubmitted. |
| resultType |String |Ein Unterstatus für den Auftragsstatus (operationName). |
| resultSignature |String |Weitere Informationen zum Auftragsstatus (operationName). |
| identity |String |Der Benutzer, der den Vorgang angefordert hat. Beispiel: susan@contoso.com. |
| properties |JSON |Details hierzu finden Sie im nächsten Abschnitt (Eigenschaftenschema des Überwachungsprotokolls). |

> [!NOTE]
> Die Elemente **resultType** und **resultSignature** liefern Informationen zum Ergebnis eines Vorgangs und enthalten nur einen Wert, wenn ein Vorgang abgeschlossen wurde. Beispielsweise ist nur ein Wert vorhanden, wenn **operationName** den Wert **JobStarted** oder **JobEnded** enthält.
>
>

#### <a name="audit-log-properties-schema"></a>Eigenschaftenschema des Überwachungsprotokolls

| Name | type | BESCHREIBUNG |
| --- | --- | --- |
| JobId |String |Die ID, die dem Auftrag zugewiesen ist. |
| JobName |String |Der Name, der für den Auftrag angegeben wurde. |
| JobRunTime |String |Die zum Verarbeiten des Auftrags verwendete Laufzeit. |
| SubmitTime |String |Der Zeitpunkt (UTC), zu dem der Auftrag übermittelt wurde. |
| StartTime |String |Die Uhrzeit (UTC), zu der der Auftrag nach der Übermittlung gestartet wurde |
| EndTime |String |Die Uhrzeit, zu der der Auftrag beendet wurde |
| Parallelität |String |Die Anzahl von Data Lake Analytics-Einheiten, die für diesen Auftrag während der Übermittlung angefordert wurden |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime** und **Parallelität** bietet Informationen zu einem Vorgang. Diese Einträge enthalten nur dann einen Wert, wenn ein Vorgang gestartet oder beendet wurde. Beispiel: **SubmitTime** enthält nur einen Wert, sobald **operationName** den Wert **JobSubmitted** hat.

## <a name="process-the-log-data"></a>Verarbeitung der Protokolldaten

Azure Data Lake Analytics stellt ein Muster bereit, nach dem die Protokolldaten verarbeitet und analysiert werden sollen. Sie finden das Beispiel unter [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Nächste Schritte

[Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)
