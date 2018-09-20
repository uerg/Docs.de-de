---
title: ASP.NET Core SignalR-Java-client
author: mikaelm12
description: Erfahren Sie, wie Sie mit dem ASP.NET Core SignalR-Java-Client.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482917"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR-Java-Client

Durch [Mikael Mengistu](https://twitter.com/MikaelM_12)

Die Java-Clients kann Verbindungen mit einem ASP.NET Core SignalR-Server aus Java-Code, einschließlich Android-apps. Wie die [JavaScript-Client](xref:signalr/javascript-client) und [.NET Client](xref:signalr/dotnet-client), dem Java-Client können Sie zum Empfangen und Senden von Nachrichten mit einem Hub in Echtzeit. Der Java-Client ist in ASP.NET Core 2.2 und höher verfügbar.

In diesem Artikel erwähnten Beispiel Java-Konsolenanwendung verwendet die SignalR-Java-Client.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installieren Sie das Paket der SignalR-Java-client

Die *Signalr-0.1.0-Preview 2-35174* JAR-Datei ermöglicht Clients, die mit SignalR-Hubs herstellen. Die letzte Versionsnummer der JAR-Datei finden Sie unter den [Maven-Suchergebnissen](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Wenn Sie Gradle verwenden zu können, fügen Sie die folgende Zeile in der `dependencies` Teil Ihrer *build.gradle* Datei:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Wenn Sie mit Maven den folgenden Zeilen hinzufügen der `<dependencies>` Element Ihrer *"pom.xml"* Datei:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Verbinden mit einem hub

Herstellen einer `HubConnection`, `HubConnectionBuilder` verwendet werden soll. Die Hub-URL und Protokoll-Ebene kann beim Erstellen einer Verbindungs konfiguriert werden. Konfigurieren Sie alle erforderlichen Optionen durch das Aufrufen einer der der `HubConnectionBuilder` Methoden vor dem `build`. Starten Sie die Verbindung mit `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Aufrufen von Hub-Methoden von client

Ein Aufruf von `send` eine hubmethode aufruft. Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Rufen Sie Client-Methoden von Hub-Instanz

Verwendung `hubConnection.on` Methoden auf dem Client definieren, die der Hub aufgerufen werden kann. Definieren Sie die Methoden, nach der Erstellung von jedoch vor dem Starten der Verbindung.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Bekannte Einschränkungen

Dies ist eine frühe Vorabversion des Java-Clients. Es gibt viele Features, die noch nicht unterstützt werden. Die folgenden Lücken sind für zukünftige Versionen gearbeitet:

* Nur primitive Typen als Parameter akzeptiert werden können und Rückgabetypen.
* Die APIs sind synchron.
* Der Aufruftyp "Senden" wird zu diesem Zeitpunkt unterstützt. "Aufrufen", und der Rückgabewerte streaming werden nicht unterstützt.
* Es wird nur das JSON-Protokoll unterstützt.
* Nur die WebSockets-Übertragung wird unterstützt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Java-API-Referenz](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
