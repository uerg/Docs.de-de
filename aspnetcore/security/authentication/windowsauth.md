---
title: Konfigurieren der Windows-Authentifizierung in ASP.NET Core
author: scottaddie
description: Informationen Sie zum Konfigurieren der Windows-Authentifizierung in ASP.NET Core mit IIS Express, IIS, HTTP.sys und WebListener.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 15e388433cc9b01e9db3e2fb56aca1ebb5ba5ba4
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284412"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="e74c4-103">Konfigurieren der Windows-Authentifizierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e74c4-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="e74c4-104">Von [Steve Smith](https://ardalis.com) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e74c4-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e74c4-105">Windows-Authentifizierung können konfiguriert werden, für ASP.NET Core-apps mit IIS gehosteten [HTTP.sys](xref:fundamentals/servers/httpsys), oder [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="e74c4-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="e74c4-106">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e74c4-106">Windows Authentication</span></span>

<span data-ttu-id="e74c4-107">Windows-Authentifizierung basiert auf dem Betriebssystem zur Authentifizierung von Benutzern von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="e74c4-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="e74c4-108">Sie können Windows-Authentifizierung verwenden, wenn Ihre Server in einem Unternehmensnetzwerk, die mithilfe von Active Directory-domänenidentitäten oder andere Windows-Konten zur Identifizierung von Benutzern ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e74c4-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="e74c4-109">Windows-Authentifizierung ist am besten für Intranetumgebungen, in denen Benutzer, Client-Anwendungen und Webserver mit der gleichen Windows-Domäne gehören.</span><span class="sxs-lookup"><span data-stu-id="e74c4-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="e74c4-110">[Weitere Informationen zur Windows-Authentifizierung und der Installation für IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="e74c4-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="e74c4-111">Aktivieren der Windows-Authentifizierung in ASP.NET Core-Apps</span><span class="sxs-lookup"><span data-stu-id="e74c4-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="e74c4-112">Die Visual Studio Web Application-Vorlage kann für die Unterstützung der Windows-Authentifizierung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="e74c4-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="e74c4-113">Verwenden Sie die Windows-Authentifizierung-app-Vorlage</span><span class="sxs-lookup"><span data-stu-id="e74c4-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="e74c4-114">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e74c4-114">In Visual Studio:</span></span>

1. <span data-ttu-id="e74c4-115">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="e74c4-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="e74c4-116">Wählen Sie aus der Liste der Vorlagen-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="e74c4-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="e74c4-117">Wählen Sie die **Authentifizierung ändern** Schaltfläche, und wählen **Windows-Authentifizierung**.</span><span class="sxs-lookup"><span data-stu-id="e74c4-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="e74c4-118">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="e74c4-118">Run the app.</span></span> <span data-ttu-id="e74c4-119">Der Benutzername wird angezeigt, in der oberen rechten der app.</span><span class="sxs-lookup"><span data-stu-id="e74c4-119">The username appears in the top right of the app.</span></span>

![Screenshot des Windows-Authentifizierung-Browsers](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="e74c4-121">Für die Entwicklung verwenden, die mit IIS Express bietet die Vorlage für alle Konfigurationen, die Windows-Authentifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="e74c4-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="e74c4-122">Der folgende Abschnitt zeigt, wie Sie eine ASP.NET Core-app für Windows-Authentifizierung manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e74c4-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="e74c4-123">Visual Studio-Einstellungen für Windows und die anonyme Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e74c4-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="e74c4-124">Visual Studio-Projekt **Eigenschaften** Seite **Debuggen** Registerkarte enthält das Kontrollkästchen für die Windows-Authentifizierung und die anonyme Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="e74c4-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Windows-Authentifizierung-Browser-Screenshot mit hervorgehobenen Authentifizierungsoptionen](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="e74c4-126">Alternativ können diese beiden Eigenschaften konfiguriert werden, der *"launchsettings.JSON"* Datei:</span><span class="sxs-lookup"><span data-stu-id="e74c4-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="e74c4-127">Aktivieren der Windows-Authentifizierung mit IIS</span><span class="sxs-lookup"><span data-stu-id="e74c4-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="e74c4-128">IIS verwendet den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zum Hosten von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="e74c4-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="e74c4-129">Windows-Authentifizierung ist in IIS nicht auf die app konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e74c4-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="e74c4-130">Die folgenden Abschnitte zeigen, wie Sie die IIS-Manager verwenden, um eine ASP.NET Core-app zur Verwendung von Windows-Authentifizierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e74c4-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="e74c4-131">IIS-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e74c4-131">IIS configuration</span></span>

<span data-ttu-id="e74c4-132">Den Rollendienst "IIS" für Windows-Authentifizierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e74c4-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="e74c4-133">Weitere Informationen finden Sie unter [Aktivieren der Windows-Authentifizierung in IIS-Rollendienste (Siehe Schritt2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="e74c4-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="e74c4-134">Middleware für die Integration von IIS ist standardmäßig konfiguriert, um Anforderungen zu authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="e74c4-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="e74c4-135">Weitere Informationen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS: IIS-Optionen (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="e74c4-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="e74c4-136">ASP.NET Core-Modul ist zum Weiterleiten von der Windows-Authentifizierung-Token für die app wird standardmäßig konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e74c4-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="e74c4-137">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul: Attribute des-Elements AspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="e74c4-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="e74c4-138">Erstellen einer neuen IIS-Website</span><span class="sxs-lookup"><span data-stu-id="e74c4-138">Create a new IIS site</span></span>

<span data-ttu-id="e74c4-139">Geben Sie einen Namen und den Ordner, und erlauben Sie, dass Sie einen neuen Anwendungspool zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e74c4-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="e74c4-140">Anpassen der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e74c4-140">Customize authentication</span></span>

<span data-ttu-id="e74c4-141">Öffnen Sie die Authentifizierungsfunktionen für den Standort an.</span><span class="sxs-lookup"><span data-stu-id="e74c4-141">Open the Authentication features for the site.</span></span>

![Menü "IIS-Authentifizierung"](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="e74c4-143">Deaktivieren Sie anonyme Authentifizierung und aktivieren Sie der Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="e74c4-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS-Authentifizierungseinstellungen](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="e74c4-145">Veröffentlichen Sie Ihr Projekt in der IIS-Site-Ordner</span><span class="sxs-lookup"><span data-stu-id="e74c4-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="e74c4-146">Veröffentlichen Sie die app mithilfe von Visual Studio oder .NET Core-CLI, in den Zielordner aus.</span><span class="sxs-lookup"><span data-stu-id="e74c4-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio, Dialogfeld "Veröffentlichen"](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="e74c4-148">Erfahren Sie mehr über [in IIS veröffentlichen](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="e74c4-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="e74c4-149">Starten Sie die app aus, um sicherzustellen, dass die Windows-Authentifizierung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="e74c4-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="e74c4-150">Aktivieren der Windows-Authentifizierung mit HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="e74c4-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="e74c4-151">Obwohl Windows-Authentifizierung von Kestrel nicht unterstützt wird, können Sie [HTTP.sys](xref:fundamentals/servers/httpsys) selbst gehostete Szenarios für Windows zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e74c4-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="e74c4-152">Im folgenden Beispiel wird der app Web-Host, um die "http.sys" mit der Windows-Authentifizierung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="e74c4-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="e74c4-153">HTTP.sys delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="e74c4-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="e74c4-154">Die Benutzermodusauthentifizierung wird nicht von Kerberos und HTTP.sys unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e74c4-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="e74c4-155">Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens/-Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="e74c4-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="e74c4-156">Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.</span><span class="sxs-lookup"><span data-stu-id="e74c4-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="e74c4-157">HTTP.sys wird nicht auf Nano Server-Version 1709 oder höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e74c4-157">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="e74c4-158">Um Windows-Authentifizierung und HTTP.sys mit Nano Server verwenden möchten, verwenden eine [Server Core (Microsoft/Windowsservercore) Container](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="e74c4-158">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="e74c4-159">Weitere Informationen zur Server Core finden Sie unter [Was ist, dass der Server Core-Installationsoption in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="e74c4-159">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="e74c4-160">Aktivieren der Windows-Authentifizierung mit WebListener</span><span class="sxs-lookup"><span data-stu-id="e74c4-160">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="e74c4-161">Obwohl Windows-Authentifizierung von Kestrel nicht unterstützt wird, können Sie [WebListener](xref:fundamentals/servers/weblistener) selbst gehostete Szenarios für Windows zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e74c4-161">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="e74c4-162">Im folgenden Beispiel wird die app Web-Host zum Verwenden von WebListener mit Windows-Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="e74c4-162">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="e74c4-163">WebListener delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="e74c4-163">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="e74c4-164">Die Benutzermodusauthentifizierung wird nicht von Kerberos und WebListener unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e74c4-164">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="e74c4-165">Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens bzw. -Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="e74c4-165">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="e74c4-166">Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.</span><span class="sxs-lookup"><span data-stu-id="e74c4-166">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="e74c4-167">Arbeiten mit Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e74c4-167">Work with Windows Authentication</span></span>

<span data-ttu-id="e74c4-168">Der Konfigurationsstatus des anonymen Zugriffs bestimmt die Art, wie die `[Authorize]` und `[AllowAnonymous]` Attribute werden in der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="e74c4-168">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="e74c4-169">In den folgenden zwei Abschnitten wird erläutert, wie die nicht zulässigen und die zulässigen Zustände des anonymen Zugriffs zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="e74c4-169">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="e74c4-170">Anonyme Zugriffe nicht zulassen</span><span class="sxs-lookup"><span data-stu-id="e74c4-170">Disallow anonymous access</span></span>

<span data-ttu-id="e74c4-171">Wenn Windows-Authentifizierung aktiviert ist und der anonyme Zugriff deaktiviert ist, die `[Authorize]` und `[AllowAnonymous]` Attribute haben keine Auswirkung.</span><span class="sxs-lookup"><span data-stu-id="e74c4-171">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="e74c4-172">Wenn der IIS-Site (oder HTTP.sys oder WebListener-Server) konfiguriert ist, um den anonymen Zugriff verweigern, erreicht die Anforderung nie Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="e74c4-172">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="e74c4-173">Aus diesem Grund die `[AllowAnonymous]` Attribut ist nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="e74c4-173">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="e74c4-174">Anonymen Zugriff zulassen</span><span class="sxs-lookup"><span data-stu-id="e74c4-174">Allow anonymous access</span></span>

<span data-ttu-id="e74c4-175">Wenn sowohl für Windows-Authentifizierung als auch für anonymen Zugriff aktiviert sind, verwenden die `[Authorize]` und `[AllowAnonymous]` Attribute.</span><span class="sxs-lookup"><span data-stu-id="e74c4-175">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="e74c4-176">Die `[Authorize]` Attribut können Sie Teile der app schützen, das wirklich Windows-Authentifizierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e74c4-176">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="e74c4-177">Die `[AllowAnonymous]` -Attribut überschreibt `[Authorize]` Attribut Nutzung in apps, die anonymen Zugriff zulassen.</span><span class="sxs-lookup"><span data-stu-id="e74c4-177">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="e74c4-178">Finden Sie unter [einfache Autorisierung](xref:security/authorization/simple) Nutzungsdetails Attribut.</span><span class="sxs-lookup"><span data-stu-id="e74c4-178">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="e74c4-179">In ASP.NET Core 2.x, die `[Authorize]` Attribut erfordert zusätzliche Konfigurationsschritte in *"Startup.cs"* sollen anonyme Anforderungen für die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="e74c4-179">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="e74c4-180">Die empfohlene Konfiguration variiert leicht basierend auf dem Webserver verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e74c4-180">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="e74c4-181">Standardmäßig werden Benutzer, die Autorisierung zum Zugriff auf eine Seite nicht über eine leere HTTP 403-Antwort angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e74c4-181">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="e74c4-182">Die [StatusCodePages Middleware](xref:fundamentals/error-handling#configure-status-code-pages) kann konfiguriert werden, um Benutzern mit "Zugriff verweigert" Benutzerfreundlicher zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e74c4-182">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="e74c4-183">IIS</span><span class="sxs-lookup"><span data-stu-id="e74c4-183">IIS</span></span>

<span data-ttu-id="e74c4-184">Wenn Sie IIS verwenden, fügen Sie den folgenden der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="e74c4-184">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="e74c4-185">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="e74c4-185">HTTP.sys</span></span>

<span data-ttu-id="e74c4-186">Wenn Sie "http.sys" verwenden, fügen Sie den folgenden der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="e74c4-186">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="e74c4-187">Identitätswechsel</span><span class="sxs-lookup"><span data-stu-id="e74c4-187">Impersonation</span></span>

<span data-ttu-id="e74c4-188">ASP.NET Core implementiert keine Identitätswechsel.</span><span class="sxs-lookup"><span data-stu-id="e74c4-188">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="e74c4-189">Apps, die mit der Anwendungsidentität für alle Anforderungen, die mithilfe von app-Pool oder Prozess Identität ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e74c4-189">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="e74c4-190">Wenn Sie explizit eine Aktion im Auftrag eines Benutzers ausführen müssen, verwenden Sie `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="e74c4-190">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="e74c4-191">Eine einzelne Aktion in diesem Kontext ausgeführt, und schließen Sie den Kontext.</span><span class="sxs-lookup"><span data-stu-id="e74c4-191">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="e74c4-192">Beachten Sie, dass `RunImpersonated` nicht asynchrone Vorgänge unterstützt und sollte nicht für komplexe Szenarien verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e74c4-192">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="e74c4-193">Z. B. wird nicht wrapping gesamte Anforderungen oder Middleware verkettet unterstützt oder empfohlen.</span><span class="sxs-lookup"><span data-stu-id="e74c4-193">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
