---
title: 'Anpassen von SQL Information Protection: Azure Security Center'
description: Erfahren Sie, wie Sie Information Protection-Richtlinien im Azure Security Center anpassen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2ebf2bc7-232a-45c4-a06a-b3d32aaf2500
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/11/2020
ms.author: memildin
ms.openlocfilehash: aa73fed0af0d6cd7154118d8987f42e55814e25a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91448929"
---
# <a name="customize-the-sql-information-protection-policy-in-azure-security-center-preview"></a>Anpassen der SQL Information Protection-Richtlinie im Azure Security Center (Vorschau)
 
In Azure Security Center kann eine SQL Information Protection-Richtlinie für Ihren gesamten Azure-Mandanten definiert und angepasst werden.

Information Protection ist eine erweiterte Sicherheitsfunktion für Ermittlung, Klassifizierung, Bezeichnung und Berichterstellung zu vertraulichen Daten in Ihren Azure-Datenressourcen. Das Ermitteln und Klassifizieren Ihrer vertraulichen Daten (Geschäfts-, Finanz-, Gesundheits-, personenbezogene Daten usw.) kann eine entscheidende Rolle in der Strategie Ihrer Organisation zum Datenschutz spielen. Sie kann für Folgendes als Infrastruktur gelten:
- Unterstützung beim Einhalten von Datenschutzstandards und gesetzlich geregelten Complianceanforderungen
- Sicherheitsszenarien wie Überwachung und Warnung bei anomalem Zugriff auf vertrauliche Daten
- Steuern des Zugriffs auf und Härten der Sicherheit von Datenspeichern, die sensible Daten enthalten
 
[SQL Information Protection](../azure-sql/database/data-discovery-and-classification-overview.md) implementiert dieses Paradigma für Ihre SQL-Datenspeicher, die derzeit für Azure SQL-Datenbank unterstützt. SQL Information Protection ermittelt und klassifiziert automatisch potenziell sensible Daten, stellt einen Bezeichnungsmechanismus zum dauerhaften Kennzeichnen der sensiblen Daten mit Klassifizierungsattributen bereit und bietet ein detailliertes Dashboard, das den Klassifizierungsstatus der Datenbank zeigt. Darüber hinaus wird die Vertraulichkeit des Resultsets von SQL-Abfragen berechnet, sodass Abfragen, die sensible Daten extrahieren, explizit überwacht und die Daten geschützt werden können. Weitere Informationen zu SQL Information Protection finden Sie unter [Azure SQL-Datenbank – Datenermittlung und -klassifizierung](../azure-sql/database/data-discovery-and-classification-overview.md).
 
Der Klassifizierungsmechanismus basiert auf zwei primären Konstrukten, die die Klassifizierungstaxonomie bilden: **Bezeichnungen** und **Informationstypen**.
- **Bezeichnungen**: Die wichtigsten Klassifizierungsattribute zum Definieren der Vertraulichkeitsstufe der in der Spalte gespeicherten Daten. 
- **Informationstypen**: Bieten zusätzliche Granularität für den Typ der in der Spalte gespeicherten Daten.
 
Information Protection umfasst eine integrierte Gruppe von Bezeichnungen und Typen von Informationen, die standardmäßig verwendet werden. Um diese Bezeichnungen und Typen anzupassen, können Sie die Information Protection-Richtlinie in Azure Security Center anpassen.
 
## <a name="customize-the-information-protection-policy"></a>Anpassen der Information Protection-Richtlinie
Um die Information Protection-Richtlinie für Ihren Azure-Mandanten anpassen zu können, benötigen Sie [Administratorrechte für die Stammverwaltungsgruppe des Mandanten](security-center-management-groups.md). 
 
1. Navigieren Sie im Security Center-Hauptmenü unter **RESSOURCENSICHERHEIT** zu **Daten & Speicherung**, und klicken Sie auf die Schaltfläche **SQL Information Protection**.

   ![Konfigurieren der Information Protection-Richtlinie](./media/security-center-info-protection-policy/security-policy.png) 
 
2. Auf der Seite **SQL Information Protection** sehen Sie die aktuelle Gruppe von Bezeichnungen. Dies sind die wichtigsten Klassifizierungsattribute, die verwendet werden, um die Vertraulichkeitsstufe Ihrer Daten zu kategorisieren. In dieser Ansicht können Sie die **Information Protection-Bezeichnungen** und **Informationstypen** für den Mandanten konfigurieren. 
 
### <a name="customizing-labels"></a>Anpassen von Bezeichnungen
 
1. Sie können vorhandene Bezeichnungen bearbeiten bzw. löschen oder eine neue Bezeichnung hinzufügen. Um eine vorhandene Bezeichnung zu bearbeiten, wählen Sie diese Bezeichnung aus, und klicken Sie dann im oberen Bereich oder im Kontextmenü auf der rechten Seite auf **Konfigurieren**. Um eine neue Bezeichnung hinzuzufügen, klicken Sie in der oberen Menüleiste oder am unteren Rand der Tabelle „Bezeichnungen“ auf **Bezeichnung erstellen**.
2. Auf dem Bildschirm **Vertraulichkeitsbezeichnung konfigurieren** können Sie den Namen der Bezeichnung und die Beschreibung erstellen oder ändern. Sie können die Bezeichnung durch Umschalten des Steuerelements **Aktiviert** aktivieren oder deaktivieren. Zu guter Letzt können Sie Informationstypen hinzufügen oder entfernen, die der Bezeichnung zugeordnet sind. Alle ermittelten Daten, die dem Informationstyp entsprechen, beziehen automatisch die zugehörige Vertraulichkeitsbezeichnung in die Klassifizierungsempfehlungen ein.
3. Klicken Sie auf **OK**.
 
   ![Konfigurieren der Vertraulichkeitsbezeichnung](./media/security-center-info-protection-policy/config-sensitivity-label.png)
 
4. Bezeichnungen werden in aufsteigender Reihenfolge ihres Vertraulichkeitsgrads aufgeführt. Um die Rangfolge zwischen den Bezeichnungen zu ändern, ziehen Sie die Bezeichnungen zur Neuanordnung an die entsprechende Position in der Tabelle oder verwenden die Schaltflächen **Nach oben verschieben** und **Nach unten verschieben**, um die Reihenfolge zu ändern. 
 
    ![Bezeichnungsliste](./media/security-center-info-protection-policy/move-up.png)
 
5. Klicken Sie dann am oberen Rand des Bildschirms auf **Speichern**.
 
 
## <a name="adding-and-customizing-information-types"></a>Hinzufügen und Anpassen von Informationstypen
 
1. Sie können Informationstypen durch Klicken auf **Informationstypen verwalten** verwalten und anpassen.
2. Um einen neuen **Informationstyp** hinzuzufügen, klicken Sie in der oberen Menüleiste auf die Option **Informationstyp erstellen**. Sie können für den **Informationstyp** einen Namen, eine Beschreibung und Suchmusterzeichenfolgen konfigurieren. Suchzeichenfolgenmuster können optional Schlüsselwörter mit Platzhalterzeichen (mit dem Zeichen „%“) enthalten, anhand derer die automatisierte Ermittlungs-Engine sensible Daten in Ihren Datenbanken basierend auf den Metadaten der Spalten ermittelt.
 
    ![Erstellen des Informationstyps](./media/security-center-info-protection-policy/info-types.png)
 
3. Sie können auch die integrierten **Informationstypen** konfigurieren, indem Sie zusätzliche Suchmusterzeichenfolgen hinzufügen, einige der vorhandenen Zeichenfolgen deaktivieren oder die Beschreibung ändern. Integrierte **Informationstypen** können nicht gelöscht oder deren Namen bearbeitet werden. 
4. **Informationstypen** werden in aufsteigender Reihenfolge der Ermittlung aufgeführt, d.h., die Typen, die weiter oben in der Liste stehen, werden bei der Suche nach Übereinstimmungen bevorzugt. Um die Rangfolge zwischen den Informationstypen zu ändern, ziehen Sie die Typen an die entsprechende Position in der Tabelle oder verwenden die Schaltflächen **Nach oben verschieben** und **Nach unten verschieben**, um die Reihenfolge zu ändern. 
5. Klicken Sie anschließend auf **OK**.
6. Nachdem die Verwaltung Ihrer Informationstypen abgeschlossen ist, ordnen Sie den relevanten Bezeichnungen die entsprechenden Typen zu, indem Sie für eine bestimmte Bezeichnung auf **Konfigurieren** klicken und nach Bedarf Informationstypen hinzufügen oder löschen.
7. Klicken Sie auf dem Hauptblatt **Bezeichnungen** auf **Speichern**, um alle Änderungen zu übernehmen.
 
Nachdem Ihre Information Protection-Richtlinie vollständig definiert und gespeichert wurde, wird sie auf die Datenklassifizierung in allen Azure SQL-Datenbank-Instanzen in Ihrem Mandanten angewendet.

## <a name="manage-sql-information-protection-using-azure-powershell"></a>Verwalten von SQL Information Protection mit Azure PowerShell

- [Get-AzSqlInformationProtectionPolicy](https://docs.microsoft.com/powershell/module/az.security/get-azsqlinformationprotectionpolicy): Ruft die effektive SQL Information Protection-Richtlinie für den Mandanten ab.
- [Set-AzSqlInformationProtectionPolicy](https://docs.microsoft.com/powershell/module/az.security/set-azsqlinformationprotectionpolicy): Legt die effektive SQL Information Protection-Richtlinie für den Mandanten fest.
 
## <a name="next-steps"></a>Nächste Schritte
 
In diesem Artikel haben Sie erfahren, wie eine SQL Information Protection-Richtlinie im Azure Security Center definiert wird. Weitere Informationen zum Klassifizieren und Schutz von sensiblen Daten in Ihren SQL-Datenbank-Instanzen mit SQL Information Protection finden Sie unter [Azure SQL-Datenbank – Datenermittlung und -klassifizierung](../azure-sql/database/data-discovery-and-classification-overview.md). 

Weitere Informationen zu Sicherheitsrichtlinien und zur Datensicherheit im Azure Security Center finden Sie in den folgenden Artikeln:
 
- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md): Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Azure Security Center-Datensicherheit](security-center-data-security.md): Erfahren Sie, wie Security Center Daten verwaltet und schützt.
