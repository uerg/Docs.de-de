---
title: ASP.NET Core SignalR-Java-client
author: mikaelm12
description: Erfahren Sie, wie Sie mit dem ASP.NET Core SignalR-Java-Client.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 3d2cfe5f58cc41741512c68cebbbc90e8f714cba
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148784"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR-Java-client

Durch [Mikael Mengistu](https://twitter.com/MikaelM_12)

Die Java-Clients kann Verbindungen mit einem ASP.NET Core SignalR-Server aus Java-Code, einschließlich Android-apps. Wie die [JavaScript-Client](xref:signalr/javascript-client) und [.NET Client](xref:signalr/dotnet-client), dem Java-Client können Sie zum Empfangen und Senden von Nachrichten mit einem Hub in Echtzeit. Der Java-Client ist in ASP.NET Core 2.2 und höher verfügbar.

In diesem Artikel erwähnten Beispiel Java-Konsolenanwendung verwendet die SignalR-Java-Client.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installieren Sie das Paket der SignalR-Java-client

Die *Signalr-1.0.0-preview3-35501* JAR-Datei ermöglicht Clients, die mit SignalR-Hubs herstellen. Die letzte Versionsnummer der JAR-Datei finden Sie unter den [Maven-Suchergebnissen](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Wenn Sie Gradle verwenden zu können, fügen Sie die folgende Zeile in der `dependencies` Teil Ihrer *build.gradle* Datei:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Wenn Sie mit Maven den folgenden Zeilen hinzufügen der `<dependencies>` Element Ihrer *"pom.xml"* Datei:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Verbinden mit einem hub

Herstellen einer `HubConnection`, `HubConnectionBuilder` verwendet werden soll. Die Hub-URL und Protokoll-Ebene kann beim Erstellen einer Verbindungs konfiguriert werden. Konfigurieren Sie alle erforderlichen Optionen durch das Aufrufen einer der der `HubConnectionBuilder` Methoden vor dem `build`. Starten Sie die Verbindung mit `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Aufrufen von Hub-Methoden von client

Ein Aufruf von `send` eine hubmethode aufruft. Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>Rufen Sie Client-Methoden von Hub-Instanz

Verwendung `hubConnection.on` Methoden auf dem Client definieren, die der Hub aufgerufen werden kann. Definieren Sie die Methoden, nach der Erstellung von jedoch vor dem Starten der Verbindung.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Hinzufügen der Protokollierung

Der SignalR-Java-Client verwendet die [SLF4J](https://www.slf4j.org/) Bibliothek für die Protokollierung. Es ist eine allgemeine protokollierungs-API an, die Benutzer der Bibliothek, wählen ihre eigenen spezifischen protokollierungsimplementierung bereitgestellt werden, indem Sie in einer bestimmten Protokollierung Abhängigkeit bringen kann. Der folgende Codeausschnitt zeigt, wie Sie mit `java.util.logging` mit dem SignalR-Java-Client.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Wenn Sie die Anmeldung bei Ihrer Abhängigkeiten nicht konfigurieren, lädt SLF4J eine Protokollierung standardmäßig nicht-Vorgang die folgende Warnmeldung an:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Dies kann problemlos ignoriert werden.

## <a name="known-limitations"></a>Bekannte Einschränkungen

Dies ist ein Preview Release von der Java-Client. Einige Funktionen werden nicht unterstützt:

* Es wird nur das JSON-Protokoll unterstützt.
* Nur die WebSockets-Übertragung wird unterstützt.
* Streaming wird noch nicht unterstützt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Java-API-Referenz](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
