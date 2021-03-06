---
title: Administratorrolle für Azure Red Hat OpenShift-Cluster | Microsoft-Dokumentation
description: Zuweisen und Verwenden der Administratorrolle für Azure Red Hat OpenShift-Cluster
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 38686ba35285159d7a27724b5402a6b6e2f3a696
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88815520"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Administratorrolle für Azure Red Hat OpenShift-Kunden
 
Sie sind der Clusteradministrator eines Azure Red Hat OpenShift-Clusters. Ihr Konto verfügt über erhöhte Berechtigungen und über Zugriff auf alle vom Benutzer erstellten Projekte.

Wenn an Ihr Konto die Autorisierungsrolle „customer-admin-cluster“ gebunden ist, kann das Konto ein Projekt automatisch verwalten.

> [!Note] 
> Die Clusterrolle „customer-admin-cluster“ ist nicht mit der Clusterrolle „cluster-admin“ identisch.

Beispielsweise können Sie Aktionen ausführen, die mit einer Gruppe von Verben (`create`) verknüpft sind, um einen Satz von Ressourcennamen (`templates`) zu verarbeiten. Führen Sie den folgenden Befehl aus, um Details dieser Rollen und ihren Satz von Verben und Ressourcen anzuzeigen:

`$ oc get clusterroles customer-admin-cluster -o yaml`

Die Verbnamen werden nicht unbedingt alle direkt den `oc`-Befehlen zugeordnet. Sie entsprechen genereller den Typen von CLI-Vorgängen, die Sie ausführen können. 

Beispielsweise bedeutet das `list`-Verb, dass Sie eine Liste aller Objekte eines Ressourcennamens anzeigen können (`oc get`). Das `get`-Verb bedeutet, dass Sie die Details eines bestimmten Objekts anzeigen können, wenn Sie dessen Namen kennen (`oc describe`).

## <a name="configure-the-customer-administrator-role"></a>Konfigurieren der Kundenadministratorrolle

Sie können die Clusterrolle „customer-admin-cluster“ nur während der Clustererstellung konfigurieren, indem Sie das Flag `--customer-admin-group-id` angeben. Dieses Feld kann derzeit nicht im Azure-Portal konfiguriert werden. Informationen zum Konfigurieren von Azure Active Directory und der Gruppe „Administratoren“ finden Sie unter [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md).

## <a name="confirm-membership-in-the-customer-administrator-role"></a>Bestätigen der Mitgliedschaft in der Kundenadministratorrolle

Um Ihre Mitgliedschaft in der Gruppe der Kundenadministratoren zu bestätigen, verwenden Sie die Befehle `oc get nodes` oder `oc projects` der OpenShift CLI. `oc get nodes` zeigt eine Liste von Knoten an, wenn Sie über die Rolle customer-admin-cluster verfügen, und einen Berechtigungsfehler, wenn Sie nur über die Rolle customer-admin-project verfügen. `oc projects` zeigt alle Projekte im Cluster an und nicht nur die Projekte, an denen Sie arbeiten.

Um die Rollen und Berechtigungen in Ihrem Cluster genauer zu untersuchen, können Sie den Befehl [`oc policy who-can <verb> <resource>`](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_rbac.html#managing-role-bindings) verwenden.

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie die Clusterrolle „customer-admin-cluster“:
> [!div class="nextstepaction"]
> [Azure Active Directory-Integration für Azure Red Hat OpenShift](howto-aad-app-configuration.md).
