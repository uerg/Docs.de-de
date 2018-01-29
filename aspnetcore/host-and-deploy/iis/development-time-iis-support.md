---
title: "IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core"
author: shirhatti
description: "Erhalten Sie Unterstützung für das Debuggen von ASP.NET Core-Anwendungen, wenn sie hinter IIS unter Windows Server ausgeführt werden."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="60ca5-103">IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60ca5-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="60ca5-104">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="60ca5-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="60ca5-105">Dieser Artikel beschreibt die Unterstützung für [Visual Studio](https://www.visualstudio.com/vs/) für das Debuggen von ASP.NET Core-Anwendungen, die hinter IIS unter Windows Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="60ca5-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="60ca5-106">Dieses Thema führt Sie durch die Aktivierung dieser Funktion und Einrichten eines Projekts.</span><span class="sxs-lookup"><span data-stu-id="60ca5-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60ca5-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="60ca5-107">Prerequisites</span></span>

* <span data-ttu-id="60ca5-108">Visual Studio (2017/Version 15.3 oder höher)</span><span class="sxs-lookup"><span data-stu-id="60ca5-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="60ca5-109">ASP.NET und die Workload für die Webentwicklung *ODER* die plattformübergreifende Workload für die .NET Core-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="60ca5-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="60ca5-110">Aktivieren von IIS</span><span class="sxs-lookup"><span data-stu-id="60ca5-110">Enable IIS</span></span>

<span data-ttu-id="60ca5-111">Aktivieren Sie IIS.</span><span class="sxs-lookup"><span data-stu-id="60ca5-111">Enable IIS.</span></span> <span data-ttu-id="60ca5-112">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="60ca5-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="60ca5-113">Aktivieren Sie das Kontrollkästchen **Internetinformationsdienste**.</span><span class="sxs-lookup"><span data-stu-id="60ca5-113">Select the **Internet Information Services** checkbox.</span></span>

![Windows-Features, die das aktivierte Kontrollkästchen der Internetinformationsdienste als schwarzes Quadrat anzeigt (nicht mit einem Häkchen), was angibt, dass einige der IIS-Features aktiviert sind](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="60ca5-115">Wenn die IIS-Installation einen Neustart erforderlich ist, starten Sie das System neu.</span><span class="sxs-lookup"><span data-stu-id="60ca5-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="60ca5-116">Aktivieren der Entwicklungszeit-IIS-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="60ca5-116">Enable development-time IIS support</span></span>

<span data-ttu-id="60ca5-117">Starten Sie Visual Studio-Installer, um die vorhandene Visual Studio-Installation zu ändern, nach der Installation von IIS.</span><span class="sxs-lookup"><span data-stu-id="60ca5-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="60ca5-118">Wählen Sie im Installer die Komponente **Entwicklungszeit-IIS-Unterstützung** aus.</span><span class="sxs-lookup"><span data-stu-id="60ca5-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="60ca5-119">Die Komponente ist als optionale Komponente im Bereich **Zusammenfassung** für die Workload **ASP.NET und Webentwicklung** aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="60ca5-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="60ca5-120">Dadurch wird das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) installiert, was ein natives IIS-Modul ist, das zur Ausführung von ASP.NET Core-Anwendungen erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="60ca5-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="60ca5-124">Konfigurieren des Projekts</span><span class="sxs-lookup"><span data-stu-id="60ca5-124">Configure the project</span></span>

<span data-ttu-id="60ca5-125">Erstellen Sie ein neues Startprofil, um die Entwicklungszeit-IIS-Unterstützung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="60ca5-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="60ca5-126">Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="60ca5-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="60ca5-127">Klicken Sie auf die Registerkarte **Debuggen**. Wählen Sie **IIS** aus dem Dropdownfeld **Start** aus.</span><span class="sxs-lookup"><span data-stu-id="60ca5-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="60ca5-128">Bestätigen Sie, dass die Funktion **Launch browser** (Browser starten) mit der richtigen URL aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="60ca5-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="60ca5-133">Auch manuell hinzufügen ein Profils starten, das die [launchSettings.json](http://json.schemastore.org/launchsettings) Datei in der app:</span><span class="sxs-lookup"><span data-stu-id="60ca5-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="60ca5-134">Visual Studio möglicherweise einen Neustart aufgefordert, wenn Sie nicht als Administrator ausführen.</span><span class="sxs-lookup"><span data-stu-id="60ca5-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="60ca5-135">Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="60ca5-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="60ca5-136">Herzlichen Glückwunsch!</span><span class="sxs-lookup"><span data-stu-id="60ca5-136">Congratulations!</span></span> <span data-ttu-id="60ca5-137">An diesem Punkt ist das Projekt für IIS Entwicklungszeit Unterstützung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="60ca5-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="60ca5-138">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="60ca5-138">Additional resources</span></span>

* [<span data-ttu-id="60ca5-139">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="60ca5-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="60ca5-140">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="60ca5-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="60ca5-141">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="60ca5-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
