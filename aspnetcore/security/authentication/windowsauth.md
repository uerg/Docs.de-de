---
title: Konfigurieren der Windowsauthentifizierung in ASP.NET Core
author: ardalis
description: 'Gewusst wie: Konfigurieren von Windows-Authentifizierung in ASP.NET Core'
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 008a647295334e957c33c6db7f80687645b3b928
ms.sourcegitcommit: 69b3255f8b6f5db9e7d21f391420602d7ba9f4db
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="fb383-104">Konfigurieren der Windowsauthentifizierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb383-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="fb383-105">Durch [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="fb383-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="fb383-106">Windows-Authentifizierung kann für ASP.NET Core-apps, die mit IIS oder WebListener gehostet konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="fb383-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="fb383-107">Was ist Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="fb383-107">What is Windows authentication</span></span>

<span data-ttu-id="fb383-108">Windows-Authentifizierung basiert auf dem Betriebssystem zur Authentifizierung von Benutzern von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="fb383-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="fb383-109">Sie können Windows-Authentifizierung verwenden, wenn Ihre Server in einem Unternehmensnetzwerk mithilfe von Active Directory-Domäne-Identitäten oder andere Windows-Konten zur Identifizierung von Benutzern ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fb383-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="fb383-110">Windows-Authentifizierung ist eine sichere Form der Authentifizierung, die beste für Intranetumgebungen, in dem Benutzer, Clientanwendungen und Webservern auf derselben Windows-Domäne gehören, geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="fb383-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="fb383-111">[Weitere Informationen zu Windows-Authentifizierung und die Installation für IIS](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="fb383-111">[Learn more about Windows Authentication and installing it for IIS](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="fb383-112">Aktivieren der Windows-Authentifizierung in einer ASP.NET Core-Anwendung</span><span class="sxs-lookup"><span data-stu-id="fb383-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="fb383-113">Der Visual Studio Web Application-Vorlage kann für die Unterstützung der Windows-Authentifizierung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="fb383-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="fb383-114">Mithilfe der Windows-Authentifizierung-app-Vorlage</span><span class="sxs-lookup"><span data-stu-id="fb383-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="fb383-115">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fb383-115">In Visual Studio:</span></span>
* <span data-ttu-id="fb383-116">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="fb383-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="fb383-117">Wählen Sie aus der Liste der Vorlagen-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="fb383-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="fb383-118">Wählen Sie die Schaltfläche "Authentifizierung ändern" und **Windows-Authentifizierung**.</span><span class="sxs-lookup"><span data-stu-id="fb383-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="fb383-119">Führen Sie die app an.</span><span class="sxs-lookup"><span data-stu-id="fb383-119">Run the app.</span></span> <span data-ttu-id="fb383-120">Der Benutzername wird angezeigt, in der oberen rechten der app.</span><span class="sxs-lookup"><span data-stu-id="fb383-120">The username appears in the top right of the app.</span></span>

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="fb383-122">Die Vorlage bereitstellt für Entwicklungen mit IIS Express die zur Verwendung der Windows-Authentifizierung erforderlich Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fb383-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="fb383-123">Im nächste Abschnitt zeigt, wie eine ASP.NET Core-app für Windows-Authentifizierung manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fb383-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="fb383-124">Visual Studio-Einstellungen für Windows und anonyme Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="fb383-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="fb383-125">Der Visual Studio-Eigenschaftenseite, Registerkarte "Debuggen" bietet das Kontrollkästchen für Windows-Authentifizierung und die anonyme Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="fb383-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="fb383-127">Sie können auch konfigurieren, diese Eigenschaften in der `launchSettings.json` Datei:</span><span class="sxs-lookup"><span data-stu-id="fb383-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="fb383-128">Aktivieren der Windows-Authentifizierung mit IIS</span><span class="sxs-lookup"><span data-stu-id="fb383-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="fb383-129">IIS verwendet den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) (ANCM) zum Hosten von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="fb383-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="fb383-130">Die ANCM Flüsse Windows-Authentifizierung in IIS standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="fb383-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="fb383-131">Konfiguration der Windows-Authentifizierung erfolgt innerhalb von IIS nicht auf das Anwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="fb383-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="fb383-132">Die folgenden Abschnitte zeigen, wie IIS-Manager verwenden, um eine ASP.NET Core-Anwendung, um die Windows-Authentifizierung zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="fb383-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="fb383-133">Erstellen einer neuen IIS-Website</span><span class="sxs-lookup"><span data-stu-id="fb383-133">Create a new IIS site</span></span>

<span data-ttu-id="fb383-134">Geben Sie einen Namen und den Ordner, und lassen sie einen neuen Anwendungspool zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fb383-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="fb383-135">Anpassen der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="fb383-135">Customize Authentication</span></span>

<span data-ttu-id="fb383-136">Öffnen Sie die Authentifizierung für den Standort.</span><span class="sxs-lookup"><span data-stu-id="fb383-136">Open the Authentication menu for the site.</span></span>

![Menü für IIS-Authentifizierung](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="fb383-138">Deaktivieren Sie anonyme Authentifizierung und aktivieren Sie die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="fb383-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS-Authentifizierungseinstellungen](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="fb383-140">Veröffentlichen Sie Ihr Projekt in den IIS-Website-Ordner</span><span class="sxs-lookup"><span data-stu-id="fb383-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="fb383-141">Mithilfe von Visual Studio oder die .NET Core-CLI *veröffentlichen* Ihrer app in den Zielordner.</span><span class="sxs-lookup"><span data-stu-id="fb383-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Visual Studio, Dialogfeld "Veröffentlichen"](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="fb383-143">Erfahren Sie mehr über [in IIS veröffentlichen](https://docs.microsoft.com/aspnet/core/publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="fb383-143">Learn more about [publishing to IIS](https://docs.microsoft.com/aspnet/core/publishing/iis).</span></span>

<span data-ttu-id="fb383-144">Starten Sie die app aus, um sicherzustellen, dass die Windows-Authentifizierung funktionsfähig ist.</span><span class="sxs-lookup"><span data-stu-id="fb383-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="fb383-145">Aktivieren der Windows-Authentifizierung mit WebListener</span><span class="sxs-lookup"><span data-stu-id="fb383-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="fb383-146">Obwohl Kestrel Windows-Authentifizierung nicht unterstützt, können Sie [WebListener](xref:fundamentals/servers/weblistener) selbst gehostete Szenarien unter Windows unterstützt.</span><span class="sxs-lookup"><span data-stu-id="fb383-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="fb383-147">Das folgende Beispiel konfiguriert die app Webhost um WebListener mit Windows-Authentifizierung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="fb383-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a><span data-ttu-id="fb383-148">Arbeiten mit Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="fb383-148">Working with Windows authentication</span></span>

<span data-ttu-id="fb383-149">Wenn Ihre app auf Windows-Authentifizierung und anonymem Zugriff verwendet, können Sie die ``[Authorize]`` und ``[AllowAnonymous]`` Attribute.</span><span class="sxs-lookup"><span data-stu-id="fb383-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="fb383-150">Apps, die keinen anonymen aktiviert stimmen nicht erfordern ``[Authorize]``; die app so behandelt, als eine Authentifizierung erforderlich ist, werden die anonyme Anforderungen abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="fb383-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="fb383-151">Beachten Sie, wenn die IIS-Site konfiguriert ist **nicht** anonymen Zugriff erlaubt die ``[AllowAnonymous]`` Attribut führt **nicht** anonyme Anforderungen zulassen.</span><span class="sxs-lookup"><span data-stu-id="fb383-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="fb383-152">Die ``[AllowAnonymous]`` -Attribut überschreibt ``[Authorize]`` -Attribut Auslastung in apps, die den anonymen Zugriff zulassen.</span><span class="sxs-lookup"><span data-stu-id="fb383-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="fb383-153">Identitätswechsel</span><span class="sxs-lookup"><span data-stu-id="fb383-153">Impersonation</span></span>

<span data-ttu-id="fb383-154">ASP.NET Core implementiert Identitätswechsel nicht.</span><span class="sxs-lookup"><span data-stu-id="fb383-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="fb383-155">Apps, die mit der Anwendungsidentität für alle Anforderungen, die mit app-Pool oder Prozess Identität ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="fb383-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="fb383-156">Wenn Sie explizit eine Aktion im Auftrag eines Benutzers ausführen müssen, verwenden Sie ``WindowsIdentity.RunImpersonated``.</span><span class="sxs-lookup"><span data-stu-id="fb383-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="fb383-157">Eine einzelne Aktion in diesem Kontext ausgeführt, und schließen Sie dann den Kontext.</span><span class="sxs-lookup"><span data-stu-id="fb383-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="fb383-158">Beachten Sie, dass ``RunImpersonated`` Async nicht unterstützt und sollte nicht für komplexe Szenarios verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fb383-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="fb383-159">Z. B. wird wrapping gesamten Anfragen oder Middleware Ketten nicht unterstützt oder empfohlen.</span><span class="sxs-lookup"><span data-stu-id="fb383-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>
