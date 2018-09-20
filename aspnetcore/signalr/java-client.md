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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="c2970-103">ASP.NET Core SignalR-Java-Client</span><span class="sxs-lookup"><span data-stu-id="c2970-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="c2970-104">Durch [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="c2970-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="c2970-105">Die Java-Clients kann Verbindungen mit einem ASP.NET Core SignalR-Server aus Java-Code, einschließlich Android-apps.</span><span class="sxs-lookup"><span data-stu-id="c2970-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="c2970-106">Wie die [JavaScript-Client](xref:signalr/javascript-client) und [.NET Client](xref:signalr/dotnet-client), dem Java-Client können Sie zum Empfangen und Senden von Nachrichten mit einem Hub in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="c2970-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="c2970-107">Der Java-Client ist in ASP.NET Core 2.2 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c2970-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="c2970-108">In diesem Artikel erwähnten Beispiel Java-Konsolenanwendung verwendet die SignalR-Java-Client.</span><span class="sxs-lookup"><span data-stu-id="c2970-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="c2970-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2970-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="c2970-110">Installieren Sie das Paket der SignalR-Java-client</span><span class="sxs-lookup"><span data-stu-id="c2970-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="c2970-111">Die *Signalr-0.1.0-Preview 2-35174* JAR-Datei ermöglicht Clients, die mit SignalR-Hubs herstellen.</span><span class="sxs-lookup"><span data-stu-id="c2970-111">The *signalr-0.1.0-preview2-35174* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c2970-112">Die letzte Versionsnummer der JAR-Datei finden Sie unter den [Maven-Suchergebnissen](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="c2970-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="c2970-113">Wenn Sie Gradle verwenden zu können, fügen Sie die folgende Zeile in der `dependencies` Teil Ihrer *build.gradle* Datei:</span><span class="sxs-lookup"><span data-stu-id="c2970-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

<span data-ttu-id="c2970-114">Wenn Sie mit Maven den folgenden Zeilen hinzufügen der `<dependencies>` Element Ihrer *"pom.xml"* Datei:</span><span class="sxs-lookup"><span data-stu-id="c2970-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="c2970-115">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="c2970-115">Connect to a hub</span></span>

<span data-ttu-id="c2970-116">Herstellen einer `HubConnection`, `HubConnectionBuilder` verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="c2970-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="c2970-117">Die Hub-URL und Protokoll-Ebene kann beim Erstellen einer Verbindungs konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="c2970-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="c2970-118">Konfigurieren Sie alle erforderlichen Optionen durch das Aufrufen einer der der `HubConnectionBuilder` Methoden vor dem `build`.</span><span class="sxs-lookup"><span data-stu-id="c2970-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="c2970-119">Starten Sie die Verbindung mit `start`.</span><span class="sxs-lookup"><span data-stu-id="c2970-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c2970-120">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="c2970-120">Call hub methods from client</span></span>

<span data-ttu-id="c2970-121">Ein Aufruf von `send` eine hubmethode aufruft.</span><span class="sxs-lookup"><span data-stu-id="c2970-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="c2970-122">Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `send`.</span><span class="sxs-lookup"><span data-stu-id="c2970-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c2970-123">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="c2970-123">Call client methods from hub</span></span>

<span data-ttu-id="c2970-124">Verwendung `hubConnection.on` Methoden auf dem Client definieren, die der Hub aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="c2970-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="c2970-125">Definieren Sie die Methoden, nach der Erstellung von jedoch vor dem Starten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="c2970-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="c2970-126">Bekannte Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="c2970-126">Known limitations</span></span>

<span data-ttu-id="c2970-127">Dies ist eine frühe Vorabversion des Java-Clients.</span><span class="sxs-lookup"><span data-stu-id="c2970-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="c2970-128">Es gibt viele Features, die noch nicht unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="c2970-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="c2970-129">Die folgenden Lücken sind für zukünftige Versionen gearbeitet:</span><span class="sxs-lookup"><span data-stu-id="c2970-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="c2970-130">Nur primitive Typen als Parameter akzeptiert werden können und Rückgabetypen.</span><span class="sxs-lookup"><span data-stu-id="c2970-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="c2970-131">Die APIs sind synchron.</span><span class="sxs-lookup"><span data-stu-id="c2970-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="c2970-132">Der Aufruftyp "Senden" wird zu diesem Zeitpunkt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c2970-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="c2970-133">"Aufrufen", und der Rückgabewerte streaming werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c2970-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="c2970-134">Es wird nur das JSON-Protokoll unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c2970-134">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="c2970-135">Nur die WebSockets-Übertragung wird unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c2970-135">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2970-136">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c2970-136">Additional resources</span></span>

* [<span data-ttu-id="c2970-137">Java-API-Referenz</span><span class="sxs-lookup"><span data-stu-id="c2970-137">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
