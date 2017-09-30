---
title: "IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core"
author: shirhatti
description: "Erhalten Sie Unterstützung für das Debuggen von ASP.NET Core-Anwendungen, wenn sie hinter IIS unter Windows Server ausgeführt werden."
keywords: ASP.NET Core, Internetinformationsdienste, IIS, Windows Server, ASP.NET Core-Modul, Debuggen
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="841d9-104">IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="841d9-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="841d9-105">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="841d9-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="841d9-106">Dieser Artikel beschreibt die Unterstützung für [Visual Studio](https://www.visualstudio.com/vs/) für das Debuggen von ASP.NET Core-Anwendungen, die hinter IIS unter Windows Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="841d9-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="841d9-107">Diese Thema führt leitet Sie durch die Aktivierung dieser Funktion und der Einrichtung Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="841d9-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="841d9-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="841d9-108">Prerequisites</span></span>

* <span data-ttu-id="841d9-109">Visual Studio (2017/Version 15.3 oder höher)</span><span class="sxs-lookup"><span data-stu-id="841d9-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="841d9-110">ASP.NET und die Workload für die Webentwicklung *ODER* die plattformübergreifende Workload für die .NET Core-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="841d9-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="841d9-111">Aktivieren von IIS</span><span class="sxs-lookup"><span data-stu-id="841d9-111">Enable IIS</span></span>

<span data-ttu-id="841d9-112">Aktivieren Sie IIS auf Ihrem System.</span><span class="sxs-lookup"><span data-stu-id="841d9-112">Enable IIS on your system.</span></span> <span data-ttu-id="841d9-113">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="841d9-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="841d9-114">Aktivieren Sie das Kontrollkästchen **Internetinformationsdienste**.</span><span class="sxs-lookup"><span data-stu-id="841d9-114">Select the **Internet Information Services** checkbox.</span></span>

![Windows-Features, die das aktivierte Kontrollkästchen der Internetinformationsdienste als schwarzes Quadrat anzeigt (nicht mit einem Häkchen), was angibt, dass einige der IIS-Features aktiviert sind](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="841d9-116">Wenn Ihre IIS-Installation einen Neustart erfordert, starten Sie Ihr System neu.</span><span class="sxs-lookup"><span data-stu-id="841d9-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="841d9-117">Aktivieren der Entwicklungszeit-IIS-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="841d9-117">Enable development-time IIS support</span></span>

<span data-ttu-id="841d9-118">Sobald Sie IIS installiert haben, starten Sie den Visual Studio-Installer, um Ihre vorhandene Visual Studio-Installation zu ändern.</span><span class="sxs-lookup"><span data-stu-id="841d9-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="841d9-119">Wählen Sie im Installer die Komponente **Entwicklungszeit-IIS-Unterstützung** aus.</span><span class="sxs-lookup"><span data-stu-id="841d9-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="841d9-120">Die Komponente ist als optionale Komponente im Bereich **Zusammenfassung** für die Workload **ASP.NET und Webentwicklung** aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="841d9-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="841d9-121">Dadurch wird das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) installiert, was ein natives IIS-Modul ist, das zur Ausführung von ASP.NET Core-Anwendungen erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="841d9-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="841d9-125">Konfigurieren des Projekts</span><span class="sxs-lookup"><span data-stu-id="841d9-125">Configure the project</span></span>

<span data-ttu-id="841d9-126">Erstellen Sie ein neues Startprofil, um die Entwicklungszeit-IIS-Unterstützung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="841d9-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="841d9-127">Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="841d9-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="841d9-128">Klicken Sie auf die Registerkarte **Debuggen**. Wählen Sie **IIS** aus dem Dropdownfeld **Start** aus.</span><span class="sxs-lookup"><span data-stu-id="841d9-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="841d9-129">Bestätigen Sie, dass die Funktion **Launch browser** (Browser starten) mit der richtigen URL aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="841d9-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="841d9-134">Sie können Ihrer [launchSettings.json](http://json.schemastore.org/launchsettings)-Datei in der App alternativ auch manuell ein Startprofil hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="841d9-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="841d9-135">Sie werden möglicherweise aufgefordert, Visual Studio neu zu starten, wenn Sie das Programm nicht als Administrator ausgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="841d9-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="841d9-136">Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="841d9-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="841d9-137">Herzlichen Glückwunsch!</span><span class="sxs-lookup"><span data-stu-id="841d9-137">Congratulations!</span></span> <span data-ttu-id="841d9-138">Jetzt ist Ihr Projekt für die Entwicklungszeit-IIS-Unterstützung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="841d9-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="841d9-139">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="841d9-139">Additional resources</span></span>

* [<span data-ttu-id="841d9-140">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="841d9-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="841d9-141">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="841d9-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="841d9-142">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="841d9-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
