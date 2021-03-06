---
title: Problembehandlung für regionsübergreifende Replikation in Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel werden Fehlermeldungen und Lösungen beschrieben, mit denen Sie Probleme der regionsübergreifenden Replikation für Azure NetApp Files beheben können.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: 3aaa5d2bc6fdbda0d1db212539c719aa65cae61b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90708434"
---
# <a name="troubleshoot-cross-region-replication"></a>Problembehandlung für die regionsübergreifende Replikation

In diesem Artikel werden Fehlermeldungen und Lösungen beschrieben, mit denen Sie Probleme der regionsübergreifenden Replikation für Azure NetApp Files beheben können. 

## <a name="errors-creating-replication"></a>Fehler bei Erstellung einer Replikation  

|     Fehlermeldung    |     Lösung    |
|-|-|
|     `Volume {0} cannot   be used as source because it is already in replication`    |     Sie können keine Replikation mit einem Quellvolume erstellen, das bereits Teil einer Datenreplikationsbeziehung ist.    |
|     `Peered region   '{0}' is not accepted`    |     Sie versuchen, eine Replikation zwischen nicht per Peering verbundenen Regionen zu erstellen.    |
|     `RemoteVolumeResource   '{0}' of wrong type '{1}'`    |     Überprüfen Sie, ob es sich bei der Remoteressourcen-ID um eine Volumenressourcen-ID handelt.    |

## <a name="errors-authorizing-volume"></a>Fehler bei Autorisierung eines Volumes  

|     Fehlermeldung    |     Lösung    |
|-|-|
|     `Missing value   for 'AuthorizeSourceReplication'`    |     Auf der Benutzeroberfläche oder in der API-Anforderung fehlt `RemoteResourceID` oder ist ungültig (beheben Sie die Fehlermeldung).    |
|     `Missing value   for 'RemoteVolumeResourceId'`    |     Auf der Benutzeroberfläche oder in der API-Anforderung fehlt `RemoteResourceID` oder ist ungültig.    |
|     `Data   Protection volume not found for RemoteVolumeResourceId: {remoteResourceId}`    |     Überprüfen Sie, ob `RemoteResourceID` korrekt oder für den Benutzer vorhanden ist.    |
|     `Remote volume   '{0}' is not configured for replication`    |     Beim Zielvolume handelt es sich um kein Volume zum Schutz von Daten.    |
|     `Remote volume   '{0}' does not have source volume '{1}' as RemoteVolumeResourceId`    |     Das Volume für den Schutz von Daten verfügt nicht über dieses Quellvolume in seiner Remoteressourcen-ID (eine falsche Quell-ID wurde eingegeben).    |
|     `The   destination volume replication creation failed (message: {0})`    |     Dieser Fehler weist auf einen Serverfehler hin. Wenden Sie sich an den Support.    |

## <a name="errors-deleting-replication"></a>Fehler beim Löschen der Replikation

|     Fehlermeldung    |     Lösung    |
|-|-|
|     `Replication   cannot be deleted, mirror state needs to be in status: Broken before deleting`    |     Überprüfen Sie, ob die Replikation entweder unterbrochen oder deinitialisiert wurde und sich im Leerlauf befindet (Initialisierungsfehler).    |
|     `Cannot delete   source replication`    |     Das Löschen der Replikation auf Quellseite ist nicht zulässig. Sorgen Sie dafür, dass die Replikation auf Zielseite gelöscht wird.    |

## <a name="errors-resyncing-volume"></a>Fehler bei Neusynchronisierung eines Volumes

|     Fehlermeldung    |     Lösung    |
|-|-|
|     `Volume Replication is in invalid status: (Mirrored|Uninitialized) for operation: 'ResyncReplication'`     |     Überprüfen Sie, ob die Volumereplikation den Zustand „Unterbrochen“ aufweist.    |

## <a name="errors-deleting-snapshot"></a>Fehler beim Löschen einer Momentaufnahme 

|     Fehlermeldung    |     Lösung    |
|-|-|
|     `Snapshot   cannot be deleted, parent volume is a Data Protection volume with a   replication object`    |     Überprüfen Sie, ob Sie die Replikation des Volumes unterbrochen haben, wenn Sie diese Momentaufnahme löschen möchten.    |
|     `Cannot delete   volume replication generated snapshot`    |     Das Löschen von Replikationsbaselinemomentaufnahmen ist nicht zulässig.    |

## <a name="next-steps"></a>Nächste Schritte  

* [Regionsübergreifende Replikation](cross-region-replication-introduction.md)
* [Regionsübergreifende Replikation: Anforderungen und Überlegungen](cross-region-replication-requirements-considerations.md)
* [Erstellen eines Replikationspeerings](cross-region-replication-create-peering.md)
* [Anzeigen des Integritätsstatus der Replikationsbeziehung](cross-region-replication-display-health-status.md)
* [Verwalten der Notfallwiederherstellung](cross-region-replication-manage-disaster-recovery.md)
* [Problembehandlung für die regionsübergreifende Replikation](troubleshoot-cross-region-replication.md)
