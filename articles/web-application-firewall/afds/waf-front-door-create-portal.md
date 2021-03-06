---
title: 'Tutorial: Erstellen von WAF-Richtlinien für Azure Front Door – Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal eine Web Application Firewall-Richtlinie (WAF) erstellen.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: victorh
ms.openlocfilehash: 7c7ea5297276ed9a1d1f2ca8f4190997dcab57c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90602216"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Tutorial: Erstellen einer Web Application Firewall-Richtlinie auf Azure Front Door im Azure-Portal

Dieses Tutorial zeigt Ihnen, wie Sie eine einfache Azure Web Application Firewall-Richtlinie (WAF) erstellen und auf einen Front-End-Host in Azure Front Door anwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer WAF-Richtlinie
> * Deren Zuordnung zu einem Front-End-Host
> * Konfigurieren von WAF-Regeln

## <a name="prerequisites"></a>Voraussetzungen

Gehen Sie zum Erstellen eines Front Door-Profils gemäß den Anweisungen unter [Schnellstart: Erstellen einer Front Door-Instanz für eine hoch verfügbare globale Webanwendung](../../frontdoor/quickstart-create-front-door.md) vor. 

## <a name="create-a-web-application-firewall-policy"></a>Erstellen einer Web Application Firewall-Richtlinie

Erstellen Sie zuerst im Portal mithilfe des verwalteten Standardregelsatzes eine einfache WAF-Richtlinie. 

1. Klicken Sie links auf dem Bildschirm auf **Ressource erstellen**. Suchen Sie nach **WAF**, wählen Sie **Web Application Firewall (Vorschau)** aus, und klicken Sie auf **Erstellen**.
2. Geben Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus. Übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription            |Wählen Sie den Namen Ihres Front Door-Abonnements aus.|
    | Resource group          |Wählen Sie den Namen Ihrer Front Door-Ressourcengruppe aus.|
    | Richtlinienname             |Geben Sie einen eindeutigen Namen für Ihre WAF-Richtlinie ein.|

   ![Erstellen einer WAF-Richtlinie](../media/waf-front-door-create-portal/basic.png)

3. Klicken Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Zuordnung** auf **Front-End-Host hinzufügen**. Geben Sie die folgenden Einstellungen ein, und klicken Sie dann auf **Hinzufügen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Front Door              | Wählen Sie Ihren Front Door-Profilnamen aus.|
    | Front-End-Host           | Wählen Sie den Namen des Front Door-Hosts aus, und klicken Sie dann auf **Hinzufügen**.|
    
    > [!NOTE]
    > Wenn der Front-End-Host einer WAF-Richtlinie zugeordnet ist, wird er abgeblendet angezeigt. Sie müssen zuerst den Front-End-Host aus der zugehörigen Richtlinie entfernen und ihn dann wieder einer neuen WAF-Richtlinie zuordnen.
1. Klicken Sie auf**Überprüfen + erstellen** und dann auf **Erstellen**.

## <a name="configure-web-application-firewall-rules-optional"></a>Konfigurieren von Web Application Firewall-Regeln (optional)

### <a name="change-mode"></a>Ändern des Modus

Wenn Sie eine WAF-Richtlinie erstellen, wird diese standardmäßig im Modus **Erkennung** betrieben. Im Modus **Erkennung** blockiert WAF keine Anforderungen. Stattdessen werden Anforderungen, die den WAF-Regeln entsprechen, in WAF-Protokollen aufgezeichnet.
Um WAF in Aktion zu sehen, können Sie den Modus von **Erkennung** in **Prävention** ändern. Im Modus **Prävention** werden Anforderungen, die mit Regeln übereinstimmen, die im Standardregelsatz definiert sind, blockiert und in WAF-Protokollen aufgezeichnet.

 ![Ändern des WAF-Richtlinienmodus](../media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Benutzerdefinierte Regeln

Sie können eine benutzerdefinierte Regel erstellen, indem Sie im Abschnitt **Benutzerdefinierte Regeln** die Option **Benutzerdefinierte Regel hinzufügen** wählen. Die Seite für die Konfiguration einer benutzerdefinierten Regel wird geöffnet. Unten ist ein Beispiel für die Konfiguration einer benutzerdefinierten Regel zum Blockieren einer Anforderung für den Fall angegeben, in dem die Abfragezeichenfolge **blockme** enthält.

![Benutzerdefinierte Regeln](../media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Standardregelsatz

Der von Azure verwaltete Standardregelsatz ist standardmäßig aktiviert. Um eine einzelne Regel innerhalb einer Regelgruppe zu deaktivieren, erweitern Sie die Regeln innerhalb dieser Regelgruppe. Aktivieren Sie das **Kontrollkästchen** vor der Regelnummer, und klicken Sie oben auf der Registerkarte auf **Deaktivieren**. Um Aktionstypen für einzelne Regeln innerhalb des Regelsatzes zu ändern, aktivieren Sie das Kontrollkästchen vor der Regelnummer, und wählen Sie dann oben die Registerkarte **Aktion ändern** aus.

 ![Ändern des WAF-Regelsatzes](../media/waf-front-door-create-portal/managed2.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie die Ressourcengruppe und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informieren Sie sich ausführlicher über Azure Front Door.](../../frontdoor/front-door-overview.md)