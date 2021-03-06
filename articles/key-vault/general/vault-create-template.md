---
title: 'Azure-Leitfaden: Erstellen eines Azure-Schlüsseltresors und einer Tresorzugriffsrichtlinie mit einer Azure Resource Manager-Vorlage | Microsoft-Dokumentation'
description: Veranschaulicht, wie Sie Azure-Schlüsseltresore und Tresorzugriffsrichtlinien erstellen, indem Sie die Azure Resource Manager-Vorlage verwenden.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/5/2020
ms.author: mbaldwin
ms.openlocfilehash: cf19561005fe2e98b7b5cf6812ff9224fd9474dc
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804268"
---
# <a name="how-to-create-azure-key-vault-and-vault-access-policy-using-a-resource-manager-template"></a>Erstellen von Azure Key Vault-und Tresorzugriffsrichtlinien mithilfe einer Resource Manager-Vorlage

[Azure Key Vault](../general/overview.md) ist ein Clouddienst, der einen sicheren Speicher für Geheimnisse bereitstellt, z. B. für Schlüssel, Kennwörter, Zertifikate usw. In diesem Leitfaden geht es um die Bereitstellung einer Azure Resource Manager-Vorlage (ARM-Vorlage) zum Erstellen eines Schlüsseltresors.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie für diesen Artikel die folgenden Schritte aus:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="create-key-vault-resource-manager-template"></a>Erstellen einer Key Vault Resource Manager-Vorlage

Die folgende Vorlage zeigt eine einfache Möglichkeit zum Erstellen eines Schlüsseltresors. Einige Werte werden in der Vorlage angegeben.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the key vault."
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "Specifies whether the key vault is a standard vault or a premium vault."
      }
    }
   },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2019-09-01",
      "name": "[parameters('keyVaultName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "enabledForDeployment": "false",
        "enabledForDiskEncryption": "false",
        "enabledForTemplateDeployment": "false",
        "tenantId": "[subscription().tenantId]",
        "accessPolicies": [],
        "sku": {
          "name": "[parameters('skuName')]",
          "family": "A"
        },
        "networkAcls": {
          "defaultAction": "Allow",
          "bypass": "AzureServices"
        }
      }
    }
  ]
}

```

Weitere Informationen zu Key Vault-Vorlageneinstellungen finden Sie unter [Key Vault ARM-Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults).

> [!IMPORTANT]
> Wenn eine Vorlage erneut bereitgestellt wird, setzt sie alle vorhandenen Zugriffsrichtlinien im Schlüsseltresor außer Kraft. Es wird empfohlen, die `accessPolicies`-Eigenschaft mit vorhandenen Zugriffsrichtlinien aufzufüllen, um zu vermeiden, dass Sie den Zugriff auf den Schlüsseltresor verlieren. 

## <a name="add-access-policy-to-key-vault-resource-manager-template"></a>Hinzufügen einer Zugriffsrichtlinie zu einer Key Vault Resource Manager-Vorlage

Sie können Zugriffsrichtlinien für einen vorhandenen Schlüsseltresor ohne erneute Bereitstellung der gesamten Schlüsseltresorvorlage bereitstellen. Die folgende Vorlage zeigt eine einfache Möglichkeit zum Erstellen von Zugriffsrichtlinien.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the key vault."
      }
    },
    "objectId": {
      "type": "string",
      "metadata": {
        "description": "Specifies the object ID of a user, service principal or security group in the Azure Active Directory tenant for the vault. The object ID must be unique for the list of access policies. Get it by using Get-AzADUser or Get-AzADServicePrincipal cmdlets."
      }
    },
    "keysPermissions": {
      "type": "array",
      "defaultValue": [
        "list"
      ],
      "metadata": {
        "description": "Specifies the permissions to keys in the vault. Valid values are: all, encrypt, decrypt, wrapKey, unwrapKey, sign, verify, get, list, create, update, import, delete, backup, restore, recover, and purge."
      }
    },
    "secretsPermissions": {
      "type": "array",
      "defaultValue": [
        "list"
      ],
      "metadata": {
        "description": "Specifies the permissions to secrets in the vault. Valid values are: all, get, list, set, delete, backup, restore, recover, and purge."
      }
    },
    "certificatePermissions": {
      "type": "array",
      "defaultValue": [
        "list"
      ],
      "metadata": {
        "description": "Specifies the permissions to certificates in the vault. Valid values are: all,  create, delete, update, deleteissuers, get, getissuers, import, list, listissuers, managecontacts, manageissuers,  recover, backup, restore, setissuers, and purge."
      }
    },
  "resources": [
     {
      "type": "Microsoft.KeyVault/vaults/accessPolicies",
      "name": "[concat(parameters('keyVaultName'), '/add')]",
      "apiVersion": "2019-09-01",
      "properties": {
        "accessPolicies": [
          {
            "tenantId": "[subscription().tenantId]",
            "objectId": "[parameters('objectId')]",
            "permissions": {
              "keys": "[parameters('keysPermissions')]",
              "secrets": "[parameters('secretsPermissions')]",
              "certificates": [parameters('certificatesPermissions')]
            }
          }
        ]
      }
    }
  ]
}

```
Weitere Informationen zu Key Vault-Vorlageneinstellungen finden Sie unter [Key Vault ARM-Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults/accesspolicies).

## <a name="other-available-key-vault-resource-manager-templates"></a>Weitere verfügbare Key Vault Resource Manager-Vorlagen

Für Key Vault-Objekte stehen weitere Resource Manager-Vorlagen zur Verfügung:

| Geheimnisse | Schlüssel | Zertifikate |
|--|--|--|
|[Schnellstart](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-template)<br>[Verweis](https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults/secrets)|–|–|

Weitere Key Vault-Vorlagen finden Sie hier: [Key Vault Resource Manager-Referenz](https://docs.microsoft.com/azure/templates/microsoft.keyvault/allversions)

## <a name="deploy-the-templates"></a>Bereitstellen der Vorlagen

Sie können das Azure-Portal verwenden, um die oben genannten Vorlagen mithilfe der Option „Eigene Vorlage im Editor erstellen“ im folgenden Leitfaden bereitzustellen: [Bereitstellen von Ressourcen mithilfe einer benutzerdefinierten Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal#deploy-resources-from-custom-template)

Sie können die oben aufgeführten Vorlagen auch in Dateien speichern und die folgenden Befehle verwenden:  [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) und [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create):

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile key-vault-template.json
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file key-vault-template.json
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit weiteren Schnellstarts und Tutorials fortfahren möchten, müssen Sie diese Ressourcen nicht bereinigen. Wenn die Ressourcen nicht mehr benötigt werden, löschen Sie die Ressourcengruppe. Dadurch werden der Schlüsseltresor und die zugehörigen Ressourcen gelöscht. Wenn Sie die Ressourcengruppe mit der Azure CLI oder Azure PowerShell löschen möchten, verwenden Sie die folgenden Schritte.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="resources"></a>Ressourcen

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- Lesen Sie weitere Informationen zu [Azure Resource Manager](../../azure-resource-manager/management/overview.md).
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)

## <a name="next-steps"></a>Nächste Schritte

- [Sicherer Zugriff auf einen Schlüsseltresor](secure-your-key-vault.md)
- [Authentifizieren bei einem Schlüsseltresor](authentication.md)
- [Entwicklerleitfaden zu Azure Key Vault](developers-guide.md)
