---
title: IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core
author: shirhatti
description: Ermitteln Sie die Unterstützung für das Debuggen von ASP.NET Core-apps, wenn IIS unter Windows Server zurückliegt.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="3f55a-103">IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f55a-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="3f55a-104">Durch [Sourabh Shirhatti](https://twitter.com/sshirhatti) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3f55a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3f55a-105">Dieser Artikel beschreibt [Visual Studio](https://www.visualstudio.com/vs/) Unterstützung für das Debuggen von ASP.NET Core-apps, die hinter IIS unter Windows Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3f55a-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="3f55a-106">Dieses Thema führt Sie durch die Aktivierung dieser Funktion und Einrichten eines Projekts.</span><span class="sxs-lookup"><span data-stu-id="3f55a-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f55a-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="3f55a-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="3f55a-108">Aktivieren von IIS</span><span class="sxs-lookup"><span data-stu-id="3f55a-108">Enable IIS</span></span>

1. <span data-ttu-id="3f55a-109">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="3f55a-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="3f55a-110">Wählen Sie die **Internetinformationsdienste (IIS)** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="3f55a-110">Select the **Internet Information Services** check box.</span></span>

![Windows-Features mit Internetinformationsdienste (IIS) das Kontrollkästchen aktiviert als schwarze Quadrat (nicht mit einem Häkchen), der angibt, dass einige der IIS-Funktionen aktiviert sind](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="3f55a-112">Die IIS-Installation möglicherweise einen Neustart des Systems erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3f55a-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="3f55a-113">Konfigurieren von IIS</span><span class="sxs-lookup"><span data-stu-id="3f55a-113">Configure IIS</span></span>

<span data-ttu-id="3f55a-114">IIS muss es sich um eine Website wie folgt konfiguriert sein:</span><span class="sxs-lookup"><span data-stu-id="3f55a-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="3f55a-115">Hostname für ein Hostnamen an, der die app-Start-Profil-URL entspricht.</span><span class="sxs-lookup"><span data-stu-id="3f55a-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="3f55a-116">Die Bindung für Port 443 mit einem zugeordneten Zertifikat.</span><span class="sxs-lookup"><span data-stu-id="3f55a-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="3f55a-117">Z. B. die **Hostnamen** für eine zusätzliche Website auf "Localhost" (das Start-Profil verwenden ebenfalls "Localhost" weiter unten in diesem Thema) festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="3f55a-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="3f55a-118">Der Port ist auf "443" (HTTPS) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="3f55a-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="3f55a-119">Die **IIS Express Entwicklungszertifikat** funktioniert auf die Website, aber ein gültiges Zertifikat zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="3f55a-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Fügen Sie Fenster in IIS mit den Satz der Bindung für "localhost" über Port 443 mit einem Zertifikat zugeordnet.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="3f55a-121">Verfügt die IIS-Installation bereits eine **Default Web Site** mit einem Hostnamen, die Hostnamen für die app starten Profil-URL entspricht:</span><span class="sxs-lookup"><span data-stu-id="3f55a-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="3f55a-122">Fügen Sie eine portbindung für Port 443 (HTTPS) hinzu.</span><span class="sxs-lookup"><span data-stu-id="3f55a-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="3f55a-123">Weisen Sie ein gültiges Zertifikat der Website an.</span><span class="sxs-lookup"><span data-stu-id="3f55a-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="3f55a-124">Aktivieren Sie die Entwicklungszeit IIS-Unterstützung in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f55a-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="3f55a-125">Starten Sie Visual Studio-Installer.</span><span class="sxs-lookup"><span data-stu-id="3f55a-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="3f55a-126">Wählen Sie die **Entwicklungszeit IIS unterstützen** Komponente.</span><span class="sxs-lookup"><span data-stu-id="3f55a-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="3f55a-127">Die Komponente aufgelistet, als optional in der **Zusammenfassung** Bereich für die **ASP.NET und zur Webentwicklung** arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="3f55a-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="3f55a-128">Die Komponente installiert den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), dies ist ein systemeigenes IIS-Modul, das zum Ausführen von ASP.NET Core apps hinter IIS in einer reverse-Proxy-Konfiguration erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3f55a-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="3f55a-132">Konfigurieren des Projekts</span><span class="sxs-lookup"><span data-stu-id="3f55a-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="3f55a-133">HTTPS-Umleitung</span><span class="sxs-lookup"><span data-stu-id="3f55a-133">HTTPS redirection</span></span>

<span data-ttu-id="3f55a-134">Für ein neues Projekt, wählen Sie dieses Kontrollkästchen, um **für HTTPS konfigurieren** in der **neue ASP.NET-Webanwendung für Core** Fenster:</span><span class="sxs-lookup"><span data-stu-id="3f55a-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Neue ASP.NET Core Web Application-Fenster mit der für HTTPS-Kontrollkästchen konfigurieren.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="3f55a-136">Verwenden Sie in einem vorhandenen Projekt HTTPS-Umleitung Middleware in `Startup.Configure` durch Aufrufen der [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="3f55a-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="3f55a-137">Starten Sie IIS Profil</span><span class="sxs-lookup"><span data-stu-id="3f55a-137">IIS launch profile</span></span>

<span data-ttu-id="3f55a-138">Erstellen Sie ein neues Launch-Profil Entwicklungszeit IIS-Unterstützung hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="3f55a-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="3f55a-139">Für **Profil**, wählen die **neu** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="3f55a-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="3f55a-140">Nennen Sie das Profil "IIS" im Popup-Fenster.</span><span class="sxs-lookup"><span data-stu-id="3f55a-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="3f55a-141">Wählen Sie **OK** zum Erstellen des Profils.</span><span class="sxs-lookup"><span data-stu-id="3f55a-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="3f55a-142">Für die **starten** wählen Sie die Einstellung **IIS** aus der Liste.</span><span class="sxs-lookup"><span data-stu-id="3f55a-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="3f55a-143">Wählen Sie das Kontrollkästchen für **starten Browser** , und geben Sie die Endpunkt-URL.</span><span class="sxs-lookup"><span data-stu-id="3f55a-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="3f55a-144">Verwenden Sie das HTTPS-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="3f55a-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="3f55a-145">In diesem Beispiel wird `https://localhost/WebApplication1` verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f55a-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="3f55a-146">In der **Umgebungsvariablen** Abschnitt der **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="3f55a-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="3f55a-147">Geben Sie eine Umgebungsvariable mit dem Schlüssel `ASPNETCORE_ENVIRONMENT` und den Wert `Development`.</span><span class="sxs-lookup"><span data-stu-id="3f55a-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="3f55a-148">In der **Webservereinstellungen** legen Sie im Bereich der **App-URL**.</span><span class="sxs-lookup"><span data-stu-id="3f55a-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="3f55a-149">In diesem Beispiel wird `https://localhost/WebApplication1` verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f55a-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="3f55a-150">Speichern Sie das Profil ein.</span><span class="sxs-lookup"><span data-stu-id="3f55a-150">Save the profile.</span></span>

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="3f55a-155">Auch manuell hinzufügen ein Profils starten, das die [launchSettings.json](http://json.schemastore.org/launchsettings) Datei in der app:</span><span class="sxs-lookup"><span data-stu-id="3f55a-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a><span data-ttu-id="3f55a-156">Führen Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="3f55a-156">Run the project</span></span>

<span data-ttu-id="3f55a-157">Legen Sie die Schaltfläche "ausführen" in der Visual Studio-Benutzeroberfläche, auf die **IIS** Profil, und wählen Sie die Schaltfläche, um die app zu starten:</span><span class="sxs-lookup"><span data-stu-id="3f55a-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Führen Sie Schaltfläche aus "" in der Visual Studio-Symbolleiste auf das Profil "IIS" festgelegt.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="3f55a-159">Visual Studio möglicherweise einen Neustart aufgefordert, wenn Sie nicht als Administrator ausführen.</span><span class="sxs-lookup"><span data-stu-id="3f55a-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="3f55a-160">Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="3f55a-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="3f55a-161">Wenn eine nicht vertrauenswürdige Entwicklungszertifikat verwendet wird, der Browser benötigen Sie möglicherweise zum Erstellen einer Ausnahme für das Zertifikat nicht vertrauenswürdig.</span><span class="sxs-lookup"><span data-stu-id="3f55a-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f55a-162">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3f55a-162">Additional resources</span></span>

* [<span data-ttu-id="3f55a-163">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="3f55a-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="3f55a-164">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="3f55a-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="3f55a-165">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="3f55a-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="3f55a-166">Erzwingen von HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f55a-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
