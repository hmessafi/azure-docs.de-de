---
title: include file
description: include file
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/19/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 367a0b1d17f8d5ebe4f46835ace963b00e75354e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "68229252"
---
So erstellen Sie eine IoT Hub-Instanz über das Azure-Portal:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Ressource erstellen** > **Internet der Dinge (IoT)**  > **IoT Hub** aus.

    ![Auswahl für die IoT Hub-Installation](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. Erstellen Sie Ihre kostenlose IoT Hub-Instanz mit den Werten aus den folgenden Tabellen:

    | Einstellung | Wert |
    | ------- | ----- |
    | Subscription | Wählen Sie Ihr Azure-Abonnement in der Dropdownliste aus. |
    | Resource group | Erstellen Sie eine neue Ressourcengruppe. In diesem Tutorial wird der Name **tutorials-iot-hub-rg** verwendet. |
    | Region | In diesem Tutorial wird **USA, Westen** verwendet. Sie können die Region auswählen, die Ihnen am nächsten ist. |
    | Name | Im folgenden Screenshot wird der Name **tutorials-iot-hub** verwendet. Sie müssen einen eigenen eindeutigen Namen wählen, wenn Sie Ihren Hub erstellen. |

    ![Hub-Einstellungen 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | Einstellung | Wert |
    | ------- | ----- |
    | Tarif und Skalierung | F1 Free. Pro Abonnement ist jeweils nur ein kostenloser Hub zulässig. |
    | IoT Hub-Einheiten | 1 |

    ![Hub-Einstellungen 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. Klicken Sie auf **Erstellen**. Die Erstellung des Hubs kann mehrere Minuten dauern.

    ![Hub-Einstellungen 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. Notieren Sie sich den gewählten IoT Hub-Namen. Sie verwenden diesen Wert später im Tutorial.
