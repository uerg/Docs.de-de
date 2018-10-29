---
title: Authentifizierung und Autorisierung in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie Authentifizierung und Autorisierung in ASP.NET Core SignalR verwenden.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: 31d5f753e043157caf43fa8df54e310ea0efd17b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207939"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="86be3-103">Authentifizierung und Autorisierung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="86be3-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="86be3-104">Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="86be3-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="86be3-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(Herunterladen von)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="86be3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="86be3-106">Authentifizieren von Benutzern, die Verbindung mit einer SignalR-hub</span><span class="sxs-lookup"><span data-stu-id="86be3-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="86be3-107">SignalR kann verwendet werden, mit [Authentifizierung in ASP.NET Core](xref:security/authentication/index) , jede Verbindung ein Benutzers zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="86be3-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="86be3-108">In einem Hub Authentifizierungsdaten über aufgerufen werden können die [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="86be3-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="86be3-109">Authentifizierung kann den Hub zum Aufrufen von Methoden für alle Verbindungen mit einem Benutzer verknüpft (finden Sie unter [Verwalten von Benutzern und Gruppen in SignalR](xref:signalr/groups) Informationen).</span><span class="sxs-lookup"><span data-stu-id="86be3-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="86be3-110">Mehrere Verbindungen können einen einzelnen Benutzer zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="86be3-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="86be3-111">Cookie-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="86be3-111">Cookie authentication</span></span>

<span data-ttu-id="86be3-112">In einer browserbasierten-app ermöglicht Cookie-Authentifizierung für Ihre vorhandenen Anmeldeinformationen des Benutzers automatisch an die SignalR-Verbindungen übermittelt.</span><span class="sxs-lookup"><span data-stu-id="86be3-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="86be3-113">Wenn Sie den Browserclient zu verwenden, ist keine zusätzliche Konfiguration erforderlich.</span><span class="sxs-lookup"><span data-stu-id="86be3-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="86be3-114">Wenn der Benutzer zu Ihrer app angemeldet ist, erbt die SignalR-Verbindung automatisch diese Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="86be3-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="86be3-115">Cookie-Authentifizierung wird nicht empfohlen, es sei denn, der app lediglich zum Authentifizieren von Benutzern aus dem Webclient benötigt.</span><span class="sxs-lookup"><span data-stu-id="86be3-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="86be3-116">Bei Verwendung der [.NET Client](xref:signalr/dotnet-client), `Cookies` Eigenschaft kann konfiguriert werden, der `.WithUrl` Aufruf, um ein Cookie bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="86be3-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="86be3-117">Verwenden der Cookieauthentifizierung vom .NET-Client erfordert jedoch die app eine API zum Austauschen von Authentifizierungsdaten für ein Cookie bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="86be3-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="86be3-118">Token-Bearer-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="86be3-118">Bearer token authentication</span></span>

<span data-ttu-id="86be3-119">Token-Bearer-Authentifizierung wird empfohlen, wenn Clients als Browser-Clients verwenden.</span><span class="sxs-lookup"><span data-stu-id="86be3-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="86be3-120">Bei diesem Ansatz wird dem Client ein Zugriffstoken, die der Server überprüft und zur Identifizierung des Benutzers verwendet.</span><span class="sxs-lookup"><span data-stu-id="86be3-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="86be3-121">Die Details des Bearer-token-Authentifizierung sind über den Rahmen dieses Dokuments hinaus.</span><span class="sxs-lookup"><span data-stu-id="86be3-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="86be3-122">Auf dem Server Bearer-token-Authentifizierung erfolgt über die [JWT-Bearer-Middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="86be3-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="86be3-123">In JavaScript-Client das Token kann bereitgestellt werden, mithilfe der [AccessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) Option.</span><span class="sxs-lookup"><span data-stu-id="86be3-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="86be3-124">In der .NET Client besteht ein ähnliches [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) -Eigenschaft, die zum Konfigurieren des Tokens verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="86be3-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="86be3-125">Die token Access-Funktion, die Sie angeben, wird aufgerufen, bevor **jeder** HTTP-Anforderung, die von SignalR.</span><span class="sxs-lookup"><span data-stu-id="86be3-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="86be3-126">Wenn Sie das Token zu erneuern, um die Verbindung aktiv bleibt, (da es beim Herstellen der Verbindung ablaufen kann) müssen, tun Sie dies innerhalb dieser Funktion und das aktualisierte Token zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="86be3-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="86be3-127">Bearertoken werden in standard-Web-APIs einen HTTP-Header gesendet.</span><span class="sxs-lookup"><span data-stu-id="86be3-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="86be3-128">SignalR ist jedoch nicht möglich, diese Header in Browsern festlegen, wenn Sie einige Transporte verwenden.</span><span class="sxs-lookup"><span data-stu-id="86be3-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="86be3-129">Bei Verwendung von WebSockets "und" Server-Sent Ereignisse wird das Token als ein Abfragezeichenfolgen-Parameter übertragen.</span><span class="sxs-lookup"><span data-stu-id="86be3-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="86be3-130">Um dies auf dem Server zu unterstützen, sind zusätzliche Konfigurationsschritte erforderlich:</span><span class="sxs-lookup"><span data-stu-id="86be3-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a><span data-ttu-id="86be3-131">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="86be3-131">Windows authentication</span></span>

<span data-ttu-id="86be3-132">Wenn [Windows-Authentifizierung](xref:security/authentication/windowsauth) erfolgt in Ihrer app kann SignalR, die diese Identität zum Sichern von Hubs verwenden.</span><span class="sxs-lookup"><span data-stu-id="86be3-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="86be3-133">Um Nachrichten an einzelne Benutzer zu senden, müssen Sie jedoch einen benutzerdefinierten Anbieter für die Benutzer-ID hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="86be3-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="86be3-134">Dies ist, da das System der Windows-Authentifizierung nicht den Anspruch "Name Identifier", den SignalR verwendet bereitstellt, um zu bestimmen, den Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="86be3-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="86be3-135">Fügen Sie eine neue Klasse, die implementiert `IUserIdProvider` und Abrufen von einer der Ansprüche aus dem Benutzer, die als Bezeichner zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="86be3-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="86be3-136">Um beispielsweise den Anspruch "Name" verwenden (Dies ist die Windows-Benutzernamen im Format `[Domain]\[Username]`), erstellen Sie die folgende Klasse:</span><span class="sxs-lookup"><span data-stu-id="86be3-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="86be3-137">Statt `ClaimTypes.Name`, können Sie einen beliebigen Wert aus der `User` (z. B. die Windows-SID-ID, usw.).</span><span class="sxs-lookup"><span data-stu-id="86be3-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="86be3-138">Die von Ihnen ausgewählte Wert muss zwischen allen Benutzern in Ihrem System eindeutig.</span><span class="sxs-lookup"><span data-stu-id="86be3-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="86be3-139">Andernfalls konnte eine Nachricht für einen einzelnen Benutzer vorgesehen am Ende zu einem anderen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="86be3-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="86be3-140">Registrieren Sie diese Komponente in Ihre `Startup.ConfigureServices` Methode **nach** den Aufruf von `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="86be3-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="86be3-141">Im .NET Client Windows-Authentifizierung aktiviert werden muss, durch Festlegen der [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="86be3-141">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="86be3-142">Windows-Authentifizierung wird nur von Browser-Clients unterstützt, wenn Sie Microsoft Internet Explorer oder Microsoft Edge verwenden.</span><span class="sxs-lookup"><span data-stu-id="86be3-142">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="86be3-143">Autorisieren von Benutzern für den Zugriff Hubs und hubmethoden</span><span class="sxs-lookup"><span data-stu-id="86be3-143">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="86be3-144">Standardmäßig können alle Methoden in einem Hub nicht authentifizierter Benutzer aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="86be3-144">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="86be3-145">Um die Authentifizierung erforderlich ist, gelten die [autorisieren](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) Attribut mit dem Hub:</span><span class="sxs-lookup"><span data-stu-id="86be3-145">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="86be3-146">Können Sie die Konstruktorargumente und die Eigenschaften der `[Authorize]` Attribut, um den Zugriff auf nur übereinstimmende bestimmte Benutzer beschränken [Autorisierungsrichtlinien](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="86be3-146">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="86be3-147">Wenn Sie eine benutzerdefinierte Autorisierungsrichtlinie aufgerufen haben z. B. `MyAuthorizationPolicy` können Sie sicherstellen, dass nur Benutzer, die dieser Richtlinie entsprechen den Hub, mit dem folgenden Code zugreifen können:</span><span class="sxs-lookup"><span data-stu-id="86be3-147">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="86be3-148">Einzelne hubmethoden haben die `[Authorize]` ebenfalls angewendet.</span><span class="sxs-lookup"><span data-stu-id="86be3-148">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="86be3-149">Wenn der aktuelle Benutzer die Richtlinie angewendet wird, an die Methode nicht übereinstimmt, wird ein Fehler an den Aufrufer zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="86be3-149">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```
