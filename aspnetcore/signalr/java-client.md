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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="d9413-103">ASP.NET Core SignalR-Java-client</span><span class="sxs-lookup"><span data-stu-id="d9413-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="d9413-104">Durch [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="d9413-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="d9413-105">Die Java-Clients kann Verbindungen mit einem ASP.NET Core SignalR-Server aus Java-Code, einschließlich Android-apps.</span><span class="sxs-lookup"><span data-stu-id="d9413-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="d9413-106">Wie die [JavaScript-Client](xref:signalr/javascript-client) und [.NET Client](xref:signalr/dotnet-client), dem Java-Client können Sie zum Empfangen und Senden von Nachrichten mit einem Hub in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="d9413-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="d9413-107">Der Java-Client ist in ASP.NET Core 2.2 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d9413-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="d9413-108">In diesem Artikel erwähnten Beispiel Java-Konsolenanwendung verwendet die SignalR-Java-Client.</span><span class="sxs-lookup"><span data-stu-id="d9413-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="d9413-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9413-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="d9413-110">Installieren Sie das Paket der SignalR-Java-client</span><span class="sxs-lookup"><span data-stu-id="d9413-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="d9413-111">Die *Signalr-1.0.0-preview3-35501* JAR-Datei ermöglicht Clients, die mit SignalR-Hubs herstellen.</span><span class="sxs-lookup"><span data-stu-id="d9413-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="d9413-112">Die letzte Versionsnummer der JAR-Datei finden Sie unter den [Maven-Suchergebnissen](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="d9413-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="d9413-113">Wenn Sie Gradle verwenden zu können, fügen Sie die folgende Zeile in der `dependencies` Teil Ihrer *build.gradle* Datei:</span><span class="sxs-lookup"><span data-stu-id="d9413-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="d9413-114">Wenn Sie mit Maven den folgenden Zeilen hinzufügen der `<dependencies>` Element Ihrer *"pom.xml"* Datei:</span><span class="sxs-lookup"><span data-stu-id="d9413-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="d9413-115">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="d9413-115">Connect to a hub</span></span>

<span data-ttu-id="d9413-116">Herstellen einer `HubConnection`, `HubConnectionBuilder` verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="d9413-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="d9413-117">Die Hub-URL und Protokoll-Ebene kann beim Erstellen einer Verbindungs konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="d9413-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="d9413-118">Konfigurieren Sie alle erforderlichen Optionen durch das Aufrufen einer der der `HubConnectionBuilder` Methoden vor dem `build`.</span><span class="sxs-lookup"><span data-stu-id="d9413-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="d9413-119">Starten Sie die Verbindung mit `start`.</span><span class="sxs-lookup"><span data-stu-id="d9413-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="d9413-120">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="d9413-120">Call hub methods from client</span></span>

<span data-ttu-id="d9413-121">Ein Aufruf von `send` eine hubmethode aufruft.</span><span class="sxs-lookup"><span data-stu-id="d9413-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="d9413-122">Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `send`.</span><span class="sxs-lookup"><span data-stu-id="d9413-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="d9413-123">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="d9413-123">Call client methods from hub</span></span>

<span data-ttu-id="d9413-124">Verwendung `hubConnection.on` Methoden auf dem Client definieren, die der Hub aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="d9413-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="d9413-125">Definieren Sie die Methoden, nach der Erstellung von jedoch vor dem Starten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="d9413-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="d9413-126">Hinzufügen der Protokollierung</span><span class="sxs-lookup"><span data-stu-id="d9413-126">Add logging</span></span>

<span data-ttu-id="d9413-127">Der SignalR-Java-Client verwendet die [SLF4J](https://www.slf4j.org/) Bibliothek für die Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="d9413-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="d9413-128">Es ist eine allgemeine protokollierungs-API an, die Benutzer der Bibliothek, wählen ihre eigenen spezifischen protokollierungsimplementierung bereitgestellt werden, indem Sie in einer bestimmten Protokollierung Abhängigkeit bringen kann.</span><span class="sxs-lookup"><span data-stu-id="d9413-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="d9413-129">Der folgende Codeausschnitt zeigt, wie Sie mit `java.util.logging` mit dem SignalR-Java-Client.</span><span class="sxs-lookup"><span data-stu-id="d9413-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="d9413-130">Wenn Sie die Anmeldung bei Ihrer Abhängigkeiten nicht konfigurieren, lädt SLF4J eine Protokollierung standardmäßig nicht-Vorgang die folgende Warnmeldung an:</span><span class="sxs-lookup"><span data-stu-id="d9413-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="d9413-131">Dies kann problemlos ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="d9413-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="d9413-132">Bekannte Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="d9413-132">Known limitations</span></span>

<span data-ttu-id="d9413-133">Dies ist ein Preview Release von der Java-Client.</span><span class="sxs-lookup"><span data-stu-id="d9413-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="d9413-134">Einige Funktionen werden nicht unterstützt:</span><span class="sxs-lookup"><span data-stu-id="d9413-134">Some features aren't supported:</span></span>

* <span data-ttu-id="d9413-135">Es wird nur das JSON-Protokoll unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d9413-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="d9413-136">Nur die WebSockets-Übertragung wird unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d9413-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="d9413-137">Streaming wird noch nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d9413-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9413-138">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d9413-138">Additional resources</span></span>

* [<span data-ttu-id="d9413-139">Java-API-Referenz</span><span class="sxs-lookup"><span data-stu-id="d9413-139">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
