---
title: Schnellstart – Hinzufügen von VoIP-Telefonie zu einer Web-App mithilfe von Azure Communication Services
description: In diesem Tutorial erfahren Sie, wie Sie die Clientbibliothek für Telefonie von Azure Communication Services für JavaScript verwenden.
author: ddematheu
ms.author: nimag
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: b66ee8117e5326a8ed8c1a1ad973fb13e942e0c7
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91761970"
---
In diesem Schnellstart erfahren Sie, wie Sie einen Anruf mithilfe der Clientbibliothek für Telefonie von Azure Communication Services für JavaScript beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/), Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen)
- Eine aktive Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein Benutzerzugriffstoken, um den Anruf-Client zu instanziieren. Erfahren Sie mehr über das [Erstellen und Verwalten von Benutzerzugriffstoken](../../access-tokens.md).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir calling-quickstart && cd calling-quickstart
```

Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

### <a name="install-the-package"></a>Installieren des Pakets

Verwenden Sie den Befehl `npm install`, um die Clientbibliothek für Telefonie von Azure Communication Services für JavaScript zu installieren.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

Mit der Option `--save` wird die Bibliothek als Abhängigkeit in Ihrer Datei **package.json** aufgelistet.

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

In dieser Schnellstartanleitung wird Webpack verwendet, um die Anwendungsressourcen zu bündeln. Führen Sie den folgenden Befehl aus, um die npm-Pakete „webpack“, „webpack-cli“ und „webpack-dev-server“ zu installieren und als Entwicklungsabhängigkeiten in Ihrer Datei **package.json** aufzulisten:

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Erstellen Sie im Stammverzeichnis Ihres Projekts die Datei **index.html**. Diese Datei wird zum Konfigurieren eines grundlegenden Layouts verwendet, das es dem Benutzer ermöglicht, einen Azure Communications Bot aufzurufen.

Hier folgt der Code:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Calling Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Calling Quickstart</h1>
    <input 
      id="callee-id-input"
      type="text"
      placeholder="Who would you like to call?"
      style="margin-bottom:1em; width: 200px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei mit dem Namen **client.js**, die die Anwendungslogik für diese Schnellstartanleitung enthalten soll. Fügen Sie den folgenden Code hinzu, um den Client für Telefonie zu importieren und Verweise auf die DOM-Elemente abzurufen, damit wir unsere Geschäftslogik anfügen können. 

```javascript
import { CallClient, CallAgent } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");

// quickstart code goes here
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features der Clientbibliothek für Telefonie von Azure Communication Services:

| Name                             | Beschreibung                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | „CallClient“ ist der Haupteinstiegspunkt der Clientbibliothek für Telefonie.                                                                       |
| CallAgent                        | „CallAgent“ dient zum Starten und Verwalten von Anrufen.                                                                                            |
| AzureCommunicationUserCredential | Die AzureCommunicationUserCredential-Klasse implementiert die CommunicationUserCredential-Schnittstelle, die zum Instanziieren von „CallAgent“ verwendet wird. |


## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Sie müssen `<USER_ACCESS_TOKEN>` durch ein gültiges Benutzerzugriffstoken für Ihre Ressource ersetzen. Wenn Sie noch über kein Token verfügen, finden Sie in der Dokumentation zu [Benutzerzugriffstoken](../../access-tokens.md) weitere Informationen. Initialisieren Sie mithilfe von `CallClient` eine `CallAgent`-Instanz mit einem `CommunicationUserCredential`-Objekt, das es uns ermöglicht, Anrufe zu tätigen und zu empfangen. Fügen Sie den folgenden Code zu **client.js** hinzu.

```javascript
const callClient = new CallClient();
const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
let callAgent;

callAgent = await callClient.createCallAgent(tokenCredential);
callButton.disabled = false;
```

## <a name="start-a-call"></a>Beginnen eines Anrufs

Fügen Sie einen Ereignishandler hinzu, um einen Anruf zu initiieren, wenn auf `callButton` geklickt wird:

```javascript
callButton.addEventListener("click", () => {
    // start a call
    const userToCall = calleeInput.value;
    call = callAgent.call(
        [{ communicationUserId: userToCall }],
        {}
    );
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```

## <a name="end-a-call"></a>Beenden eines Anrufs

Fügen Sie einen Ereignislistener hinzu, um den aktuellen Anruf zu beenden, wenn auf `hangUpButton` geklickt wird:

```javascript
hangUpButton.addEventListener("click", () => {
  // end the current call
  call.hangUp({ forEveryone: true });

  // toggle button states
  hangUpButton.disabled = true;
  callButton.disabled = false;
});
```

Die Eigenschaft `forEveryone` beendet den Anruf für alle Anrufteilnehmer.

## <a name="run-the-code"></a>Ausführen des Codes

Verwenden Sie `webpack-dev-server`, um Ihre App zu erstellen und auszuführen. Führen Sie den folgenden Befehl aus, um den Anwendungshost auf einem lokalen Webserver zu bündeln:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Navigieren Sie in Ihrem Browser zu http://localhost:8080/. Daraufhin sollte Folgendes angezeigt werden:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Screenshot der fertigen JavaScript-Anwendung":::

Sie können einen ausgehenden VoIP-Anruf tätigen, indem Sie eine Benutzer-ID im Textfeld eingeben und auf die Schaltfläche **StartCall** (Anruf beginnen) klicken. Wenn Sie `8:echo123` anrufen, werden Sie mit einem Echobot verbunden. Dies eignet sich hervorragend für die ersten Schritte und zum Überprüfen, ob Ihre Audiogeräte funktionieren.
