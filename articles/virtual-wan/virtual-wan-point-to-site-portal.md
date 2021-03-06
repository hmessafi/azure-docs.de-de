---
title: Verwenden von Azure Virtual WAN zum Erstellen einer Point-to-Site-Verbindung mit Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um eine Point-to-Site-VPN-Verbindung mit Azure zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 10/06/2020
ms.author: cherylmc
ms.openlocfilehash: 84f8563a6b03f10f4cbc647426c350d9fac52780
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812683"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Tutorial: Erstellen einer Benutzer-VPN-Verbindung per Azure Virtual WAN

In diesem Tutorial wird beschrieben, wie Sie Virtual WAN zum Verbinden Ihrer Ressourcen in Azure über eine IPsec/IKE (IKEv2)- oder OpenVPN-VPN-Verbindung nutzen. Für diese Art von Verbindung muss auf dem Clientcomputer ein Client konfiguriert sein. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines WAN
> * Erstellen einer P2S-Konfiguration
> * Erstellen eines Hubs
> * Angeben von DNS-Servern
> * Herunterladen eines VPN-Clientprofils
> * Anzeigen Ihrer Virtual WAN-Instanz

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [Before beginning](../../includes/virtual-wan-before-include.md)]

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Erstellen eines Virtual WAN

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>Erstellen einer P2S-Konfiguration

Eine P2S-Konfiguration (Point-to-Site) definiert die Parameter für das Herstellen der Verbindung mit Remoteclients.

[!INCLUDE [Create client profiles](../../includes/virtual-wan-p2s-configuration-include.md)]

## <a name="create-hub-with-point-to-site-gateway"></a><a name="hub"></a>Erstellen eines Hubs mit Point-to-Site-Gateways

[!INCLUDE [Create hub](../../includes/virtual-wan-p2s-hub-include.md)]

## <a name="specify-dns-server"></a><a name="dns"></a>Angeben des DNS-Servers

Mit den Benutzer-VPN-Gateways des Virtual WAN können Sie bis zu fünf DNS-Server angeben. Sie können sie während des Prozesses zum Erstellen des Hubs konfigurieren oder zu einem späteren Zeitpunkt ändern. Suchen Sie hierfür nach dem virtuellen Hub. Wählen Sie unter **Benutzer-VPN (Point-to-Site)** die Option **Konfigurieren** aus, und geben Sie die IP-Adresse(n) des DNS-Servers in die entsprechenden Textfelder für die **benutzerdefinierten DNS-Server** ein.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/custom-dns.png" alt-text="Benutzerdefinierter DNS-Server" lightbox="media/virtual-wan-point-to-site-portal/custom-dns-expand.png":::

## <a name="download-vpn-profile"></a><a name="download"></a>Herunterladen des VPN-Profils

Verwenden Sie das VPN-Profil, um Ihre Clients zu konfigurieren.

[!INCLUDE [Download profile](../../includes/virtual-wan-p2s-download-profile-include.md)]

### <a name="configure-user-vpn-clients"></a>Konfigurieren von Benutzer-VPN-Clients

Verwenden Sie das heruntergeladene Profil, um die Clients für den Remotezugriff zu konfigurieren. Das Verfahren unterscheidet sich für jedes Betriebssystem. Befolgen Sie die Anweisungen, die für Ihr System gelten.

[!INCLUDE [Configure clients](../../includes/virtual-wan-p2s-configure-clients-include.md)]

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
1. Auf der Seite **Übersicht** steht jeder Punkt auf der Karte für einen Hub.
1. Im Abschnitt **Hubs und Verbindungen** können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Bytes anzeigen.

## <a name="clean-up-resources"></a><a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
