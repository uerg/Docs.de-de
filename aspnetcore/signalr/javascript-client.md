---
title: ASP.NET Core SignalR-JavaScript-client
author: rachelappel
description: Übersicht über ASP.NET Core SignalR JavaScript-Client.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938043"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="b4a2e-103">ASP.NET Core SignalR-JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="b4a2e-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="b4a2e-104">Von [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b4a2e-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b4a2e-105">Die ASP.NET Core SignalR JavaScript-Clientbibliothek ermöglicht Entwicklern von serverseitigen hubcode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="b4a2e-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b4a2e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="b4a2e-107">Installieren Sie das Paket der SignalR-client</span><span class="sxs-lookup"><span data-stu-id="b4a2e-107">Install the SignalR client package</span></span>

<span data-ttu-id="b4a2e-108">Die SignalR-JavaScript-Clientbibliothek wird bereitgestellt, als ein [Npm](https://www.npmjs.com/) Paket.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="b4a2e-109">Wenn Sie Visual Studio verwenden, führen Sie `npm install` aus der **-Paket-Manager-Konsole** während Sie sich in den Stammordner.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="b4a2e-110">Führen Sie für Visual Studio Code, den Befehl in der **integriertes Terminal**.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="b4a2e-111">Npm installiert den Inhalt des Pakets in der *"node_modules"\\ @aspnet\signalr\dist\browser*  Ordner.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="b4a2e-112">Erstellen Sie einen neuen Ordner namens *Signalr* unter der *"Wwwroot"\\Lib* Ordner.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="b4a2e-113">Kopieren der *signalr.js* -Datei in die *Wwwroot\lib\signalr* Ordner.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="b4a2e-114">Verwenden Sie den SignalR JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="b4a2e-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="b4a2e-115">Verweisen auf den SignalR JavaScript-Client in der `<script>` Element.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b4a2e-116">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="b4a2e-116">Connect to a hub</span></span>

<span data-ttu-id="b4a2e-117">Der folgende Code erstellt und startet eine Verbindung.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="b4a2e-118">Die Hub-Name ist Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="b4a2e-119">Cross-Origin-Verbindungen</span><span class="sxs-lookup"><span data-stu-id="b4a2e-119">Cross-origin connections</span></span>

<span data-ttu-id="b4a2e-120">Browser werden in der Regel Verbindungen aus der gleichen Domäne wie die angeforderte Seite laden.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="b4a2e-121">Es gibt jedoch Situationen, wenn eine Verbindung mit einer anderen Domäne erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="b4a2e-122">Um zu verhindern, dass eine schädliche Website sensible Daten von einem anderen Standort lesen [Cross-Origin-Verbindungen](xref:security/cors) sind standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="b4a2e-123">Um eine cors-Anforderung zu ermöglichen, aktivieren Sie ihn in das `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b4a2e-124">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="b4a2e-124">Call hub methods from client</span></span>

<span data-ttu-id="b4a2e-125">JavaScript-Clients öffentliche Methoden aufrufen Hubs durch die Verwendung `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="b4a2e-126">Die `invoke` -Methode akzeptiert zwei Argumente:</span><span class="sxs-lookup"><span data-stu-id="b4a2e-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="b4a2e-127">Der Name der hubmethode.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-127">The name of the hub method.</span></span> <span data-ttu-id="b4a2e-128">Im folgenden Beispiel wird der Hub-Name `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="b4a2e-129">In der hubmethode definierten Argumente.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="b4a2e-130">Im folgenden Beispiel wird der Name des Arguments `message`.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b4a2e-131">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="b4a2e-131">Call client methods from hub</span></span>

<span data-ttu-id="b4a2e-132">Zum Empfangen von Nachrichten aus dem Hub Definieren einer Methode mit dem `connection.on` Methode.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="b4a2e-133">Der Name des Client-JavaScript-Methode.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="b4a2e-134">Im folgenden Beispiel wird der Name der Methode `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="b4a2e-135">Argumente übergibt der Hub, an die Methode.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="b4a2e-136">Im folgenden Beispiel wird den Wert des Arguments `message`.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="b4a2e-137">Der vorangehende Code in `connection.on` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="b4a2e-138">SignalR bestimmt, welche Clientmethode aufrufen, die nach übereinstimmenden Namen der Methode und Argumente, die in definierten `SendAsync` und `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="b4a2e-139">Rufen Sie als bewährte Methode `connection.start` nach `connection.on` damit Ihre Handler registriert werden, bevor alle Nachrichten empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="b4a2e-140">Fehlerbehandlung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="b4a2e-140">Error handling and logging</span></span>

<span data-ttu-id="b4a2e-141">Kette eine `catch` Methode am Ende der `connection.start` Methode zum Behandeln von clientseitigen Fehlern.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="b4a2e-142">Verwendung `console.error` zu Ausgabefehler in der Browserkonsole.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="b4a2e-143">Einrichten von clientseitigen Protokollierung, Ablaufverfolgung durch Übergeben einer Protokollierung und der Typ des Ereignisses zu protokollieren, wenn die Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="b4a2e-144">Nachrichten werden mit der angegebenen Ebene oder höher protokolliert.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="b4a2e-145">Verfügbaren Protokollebenen lauten wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="b4a2e-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="b4a2e-146">`signalR.LogLevel.Error` : Error-Meldungen.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="b4a2e-147">Protokolle `Error` nur Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="b4a2e-148">`signalR.LogLevel.Warning` : Warnung Nachrichten zu potenziellen Fehlern.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="b4a2e-149">Protokolle `Warning`, und `Error` Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="b4a2e-150">`signalR.LogLevel.Information` : Status-Nachrichten ohne Fehler.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="b4a2e-151">Protokolle `Information`, `Warning`, und `Error` Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="b4a2e-152">`signalR.LogLevel.Trace` : Ablaufverfolgungsmeldungen Sie aus.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="b4a2e-153">Protokolliert alle Elemente einschließlich Daten, die zwischen Hub und dem Client übertragen.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="b4a2e-154">Verwenden der `configureLogging` Methode `HubConnectionBuilder` so konfigurieren Sie die Protokollebene.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="b4a2e-155">Nachrichten werden in der Browserkonsole protokolliert.</span><span class="sxs-lookup"><span data-stu-id="b4a2e-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="b4a2e-156">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="b4a2e-156">Related resources</span></span>

* [<span data-ttu-id="b4a2e-157">Hubs</span><span class="sxs-lookup"><span data-stu-id="b4a2e-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b4a2e-158">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="b4a2e-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b4a2e-159">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="b4a2e-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="b4a2e-160">Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4a2e-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
