---
title: 'Erstellen einer NFS-Freigabe: Azure Files'
description: In diesem Artikel erfahren Sie, wie Sie eine Azure-Dateifreigabe erstellen, die mithilfe des NFS-Protokolls (Network File System) eingebunden werden kann.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 2a848cb77336fc89172d55a6204d66b9e5be5976
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90705230"
---
# <a name="how-to-create-an-nfs-share"></a>Erstellen einer NFS-Freigabe

Azure-Dateifreigaben sind vollständig verwaltete Dateifreigaben, die in der Cloud gespeichert werden. Der Zugriff erfolgt entweder über das Server Message Block-Protokoll (SMB) oder das Network File System-Protokoll (NFS). In diesem Artikel geht es um das Erstellen einer Dateifreigabe, für die das NFS-Protokoll verwendet wird. Weitere Informationen zu den beiden Protokollen finden Sie unter [Protokolle für Azure-Dateifreigaben](storage-files-compare-protocols.md).

## <a name="limitations"></a>Einschränkungen

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Regionale Verfügbarkeit

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen eines [FileStorage-Kontos](storage-how-to-create-premium-fileshare.md)

    > [!IMPORTANT]
    > Der Zugriff auf NFS-Freigaben ist nur über vertrauenswürdige Netzwerke möglich. Verbindungen zu Ihrer NFS-Freigabe müssen einer der folgenden Quellen entstammen:

    - Entweder [Erstellen eines privaten Endpunkts](storage-files-networking-endpoints.md#create-a-private-endpoint) (empfohlen) oder [Einschränken des Zugriffs auf den öffentlichen Endpunkt](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)
    - [Konfigurieren eines Site-to-Site-VPN zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)
    - Konfigurieren von [ExpressRoute](../../expressroute/expressroute-introduction.md)
- Falls Sie die Azure CLI verwenden möchten, [installieren Sie die neueste Version](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="register-the-nfs-41-protocol"></a>Registrieren des NFS 4.1-Protokolls

Wenn Sie das Azure PowerShell-Modul oder die Azure CLI nutzen, registrieren Sie Ihr Feature mithilfe der folgenden Befehle:

### <a name="powershell"></a>PowerShell

```azurepowershell
Connect-AzAccount
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context
Register-AzProviderFeature -FeatureName AllowNfsFileShares -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az login
az feature register --name AllowNfsFileShares
                    --namespace Microsoft.Storage
                    --subscription <yourSubscriptionIDHere>
az provider register --namespace Microsoft.Storage
```

## <a name="verify-that-the-feature-is-registered"></a>Überprüfen, ob das Feature registriert ist

Die Registrierungsgenehmigung kann bis zu einer Stunde dauern. Verwenden Sie die folgenden Befehle, um zu überprüfen, ob die Registrierung abgeschlossen wurde:

### <a name="powershell"></a>PowerShell

```azurepowershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNfsFileShares
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az feature show --name AllowNfsFileShares --namespace Microsoft.Storage --subscription <yourSubscriptionIDHere>
```

## <a name="create-an-nfs-share"></a>Erstellen einer NFS-Freigabe

# <a name="portal"></a>[Portal](#tab/azure-portal)

Nachdem Sie nun ein FileStorage-Konto erstellt und das Netzwerk konfiguriert haben, können Sie eine NFS-Dateifreigabe erstellen. Der Prozess ähnelt dem Erstellen einer SMB-Freigabe, Sie wählen jedoch anstelle von **SMB** beim Erstellen der Freigabe **NFS** aus.

1. Navigieren Sie zu Ihrem Speicherkonto, und wählen Sie **Dateifreigaben** aus.
1. Wählen Sie **+ Dateifreigabe** aus, um eine neue Dateifreigabe zu erstellen.
1. Benennen Sie Ihre Dateifreigabe, und wählen Sie eine bereitgestellte Kapazität aus.
1. Wählen Sie für **Protokoll** die Option **NFS (Vorschau)** aus.
1. Treffen Sie für **Root-Squash** eine Auswahl.

    - Root-Squash (Standardwert): Zugriff für den Remotesuperuser (Stamm) wird UID (65534) und GID (65534) zugeordnet.
    - Kein Root-Squash: Remotesuperuser (Stamm) erhält Zugriff als Stamm.
    - All Squash: Der gesamte Benutzerzugriff wird UID (65534) und GID (65534) zugeordnet.
    
1. Klicken Sie auf **Erstellen**.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/create-nfs-file-share.png" alt-text="Screenshots des Blatts für die Erstellung einer Dateifreigabe":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Stellen Sie sicher, dass .NET Framework installiert ist. Siehe [Download .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) (Herunterladen von .NET Framework).
 
1. Überprüfen Sie mit dem folgenden Befehl, ob PowerShell Version `5.1` oder höher installiert ist.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   Informationen zum Aktualisieren der PowerShell-Version finden Sie unter [Aktualisieren einer vorhandenen Windows PowerShell-Version](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).
    
1. Installieren Sie die aktuelle Version des PowerShellGet-Moduls.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force  
   ```

1. Schließen Sie die PowerShell-Konsole, und öffnen Sie sie dann erneut.

1. Installieren Sie die Version **2.5.2-preview** des **Az.Storage**-Vorschaumoduls:

   ```powershell
   Install-Module Az.Storage -Repository PsGallery -RequiredVersion 2.5.2-preview -AllowClobber -AllowPrerelease -Force  
   ```

   Weitere Informationen zum Installieren von PowerShell-Modulen finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).
   
1. Im Azure PowerShell-Modul verwenden Sie das Cmdlet [New-AzRmStorageShare](/powershell/module/az.storage/new-azrmstorageshare) zum Erstellen einer Premium-Dateifreigabe.

> [!NOTE]
> Die möglichen Freigabegrößen werden durch das Kontingent bestimmt, die Abrechnung erfolgt gemäß der bereitgestellten Größe. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage/files/).

  ```powershell
  New-AzRmStorageShare `
   -ResourceGroupName $resourceGroupName `
   -StorageAccountName $storageAccountName `
   -Name myshare `
   -EnabledProtocol NFS `
   -RootSquash RootSquash `
   -Context $storageAcct.Context
  ```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

In der Azure CLI verwenden Sie den Befehl [az storage share create](/cli/azure/storage/share-rm) zum Erstellen einer Premium-Dateifreigabe.

> [!NOTE]
> Die möglichen Freigabegrößen werden durch das Kontingent bestimmt, die Abrechnung erfolgt gemäß der bereitgestellten Größe. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage/files/).

```azurecli-interactive
az storage share-rm create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --enabled-protocol NFS \
    --root-access RootSquash \
    --name "myshare" 
```
---

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun eine NFS-Freigabe erstellt haben, müssen Sie sie in Ihren Linux-Client einbinden, um sie verwenden zu können. Weitere Informationen dazu finden Sie unter [Einbinden einer NFS-Freigabe](storage-files-how-to-mount-nfs-shares.md).

Wenn Probleme auftreten, finden Sie unter [Fehlerbehebung für NFS-Dateifreigaben in Azure](storage-troubleshooting-files-nfs.md) weitere Informationen.