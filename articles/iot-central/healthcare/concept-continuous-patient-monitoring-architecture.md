---
title: Architektur für die kontinuierliche Patientenüberwachung in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial finden Sie Informationen zu einer Lösungsarchitektur für die kontinuierliche Patientenüberwachung.
author: philmea
ms.author: philmea
ms.date: 09/14/2020
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: ffecd09d1084188195da83568ab3fe32ef2cdaac
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90972221"
---
# <a name="continuous-patient-monitoring-architecture"></a>Architektur für die ständige Überwachung von Patienten

In diesem Artikel wird die Architektur einer Lösung beschrieben, die auf der Grundlage der Anwendungsvorlage für die **kontinuierliche Patientenüberwachung** erstellt wurde:

Lösungen für die kontinuierliche Patientenüberwachung können mithilfe der bereitgestellten App-Vorlage sowie anhand der weiter unten skizzierten Architektur erstellt werden.

:::image type="content" source="media/cpm-architecture.png" alt-text="Architektur für die ständige Überwachung von Patienten":::

## <a name="details"></a>Details

In diesem Abschnitt werden die einzelnen Teile des Architekturdiagramms ausführlicher erläutert:

### <a name="bluetooth-low-energy-ble-medical-devices"></a>Medizinische Geräte mit Bluetooth Low Energy (BLE)

Bei vielen medizinischen Wearables, die in IoT-Lösungen im Gesundheitswesen zum Einsatz kommen, handelt es sich um BLE-Geräte. Diese Geräte können nicht direkt mit der Cloud kommunizieren und benötigen ein Gateway für den Datenaustausch mit Ihrer Cloudlösung. In dieser Architektur wird als Gateway eine Mobiltelefonanwendung verwendet.

### <a name="mobile-phone-gateway"></a>Mobiltelefongateway

Die Hauptfunktion der Mobiltelefonanwendung besteht darin, BLE-Daten von medizinischen Geräten zu erfassen und an IoT Central weiterzugeben. Die App führt Patienten außerdem durch die Geräteeinrichtung und ermöglicht es ihnen, ihre persönlichen Gesundheitsdaten anzuzeigen. Bei anderen Lösungen kann ein Tabletgateway oder ein statisches Gateway in einem Krankenzimmer verwendet werden. Für Android und iOS steht eine Open-Source-Beispielanwendung für Mobilgeräte zur Verfügung, die als Ausgangspunkt für die Anwendungsentwicklung verwendet werden kann. Weitere Informationen zur mobilen IoT Central-App für die kontinuierliche Patientenüberwachung finden Sie [hier](https://docs.microsoft.com/samples/iot-for-all/iotc-cpm-sample/iotc-cpm-sample/).

### <a name="export-to-azure-api-for-fhirreg"></a>Exportieren nach Azure API for FHIR&reg;

Azure IoT Central ist HIPAA-konform und für HITRUST&reg; zertifiziert. Per [Azure API for FHIR](../../healthcare-apis/overview.md) können Patientengesundheitsdaten auch an andere Dienste gesendet werden. Azure API for FHIR ist eine standardbasierte API für klinische Gesundheitsdaten. Mit dem [Azure IoT-Konnektor für FHIR](https://docs.microsoft.com/azure/healthcare-apis/iot-fhir-portal-quickstart) können Sie Azure API for FHIR als Ziel für den kontinuierlichen Datenexport über IoT Central verwenden.

### <a name="machine-learning"></a>Machine Learning

Verwenden Sie Machine Learning-Modelle mit Ihren FHIR-Daten, um Erkenntnisse zu gewinnen und Ihr Pflegeteam bei der Entscheidungsfindung zu unterstützen. Weitere Informationen finden Sie in der [Dokumentation zu Azure Machine Learning](../../machine-learning/index.yml).

### <a name="provider-dashboard"></a>Anbieterdashboard

Erstellen Sie mithilfe der Daten von Azure API for FHIR ein Dashboard für Patientenerkenntnisse, oder integrieren Sie sie direkt in eine elektronische Patientenakte für Pflegeteams. Pflegeteams können das Dashboard verwenden, um Patienten zu unterstützen und frühzeitig Anzeichen für eine Verschlechterung zu erkennen. Weitere Informationen finden Sie unter [Tutorial: Erstellen eines Power BI-Anbieterdashboards](howto-health-data-triage.md).

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes empfiehlt sich das Tutorial [Bereitstellen einer App für die ständige Überwachung von Patienten und exemplarische Vorgehensweise für die zugehörige Vorlage](tutorial-continuous-patient-monitoring.md).
