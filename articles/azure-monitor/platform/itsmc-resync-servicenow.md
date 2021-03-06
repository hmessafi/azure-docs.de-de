---
title: Manuelles Beheben von ServiceNow-Synchronisierungsproblemen
description: Setzen Sie die Verbindung mit ServiceNow zurück, damit Warnungen in Microsoft Azure ServiceNow wieder aufrufen können.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: a3382f93990612b0ab34eb0848cbf3d6577c44ff
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87087933"
---
# <a name="how-to-manually-fix-servicenow-sync-problems"></a>Manuelles Beheben von ServiceNow-Synchronisierungsproblemen

Azure Monitor kann Verbindungen mit ITSM-Drittanbietern (IT Service Management) herstellen. ServiceNow ist einer dieser Anbieter.

Aus Sicherheitsgründen müssen Sie möglicherweise das Authentifizierungstoken für die Verbindung mit ServiceNow aktualisieren.
Verwenden Sie das folgende Synchronisierungsverfahren, um die Verbindung erneut zu aktivieren und das Token zu aktualisieren:


1. Suchen Sie die Lösung im oberen Suchbanner, und wählen Sie dann die relevanten Lösungen aus.

    ![Neue Verbindung](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. Wählen Sie auf dem Bildschirm der Lösung im Abonnementfilter „Alles auswählen“ aus, und filtern Sie dann nach „ServiceDesk“.

    ![Neue Verbindung](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. Wählen Sie die Lösung Ihrer ITSM-Verbindung aus.
1. Wählen Sie im linken Banner die ITSM-Verbindung aus.

    ![Neue Verbindung](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Wählen Sie in der Liste jeden Connector aus. 
    1. Klicken Sie jeweils auf den Namen des Connectors, um ihn zu konfigurieren.
    1. Löschen Sie alle Connectors, die nicht mehr verwendet werden.

    1. Aktualisieren Sie die Felder entsprechend [diesen Definitionen](./itsmc-connections.md) anhand Ihres Partnertyps.

    1. Klicken Sie auf „Synchronisieren“.

       ![Neue Verbindung](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Klicken Sie auf „Speichern“.

        ![Neue Verbindung](media/itsmc-resync-servicenow/save-8bit.png)

f.    Überprüfen Sie die Benachrichtigungen, um festzustellen, ob der Vorgang erfolgreich abgeschlossen wurde. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [ITSM-Verbindungen](itsmc-connections.md)
