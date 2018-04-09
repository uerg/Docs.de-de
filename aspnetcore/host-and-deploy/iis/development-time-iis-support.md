---
title: IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core
author: shirhatti
description: Ermitteln Sie die Unterstützung für das Debuggen von ASP.NET Core-apps, wenn IIS unter Windows Server zurückliegt.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="69590-103">IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69590-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="69590-104">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="69590-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="69590-105">Dieser Artikel beschreibt [Visual Studio](https://www.visualstudio.com/vs/) Unterstützung für das Debuggen von ASP.NET Core-apps, die hinter IIS unter Windows Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="69590-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="69590-106">Dieses Thema führt Sie durch die Aktivierung dieser Funktion und Einrichten eines Projekts.</span><span class="sxs-lookup"><span data-stu-id="69590-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69590-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="69590-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="69590-108">Aktivieren von IIS</span><span class="sxs-lookup"><span data-stu-id="69590-108">Enable IIS</span></span>

<span data-ttu-id="69590-109">Aktivieren Sie IIS.</span><span class="sxs-lookup"><span data-stu-id="69590-109">Enable IIS.</span></span> <span data-ttu-id="69590-110">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="69590-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="69590-111">Aktivieren Sie das Kontrollkästchen **Internetinformationsdienste**.</span><span class="sxs-lookup"><span data-stu-id="69590-111">Select the **Internet Information Services** checkbox.</span></span>

![Windows-Features, die das aktivierte Kontrollkästchen der Internetinformationsdienste als schwarzes Quadrat anzeigt (nicht mit einem Häkchen), was angibt, dass einige der IIS-Features aktiviert sind](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="69590-113">Wenn die IIS-Installation einen Neustart erfordert, starten Sie das System neu.</span><span class="sxs-lookup"><span data-stu-id="69590-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="69590-114">Aktivieren der Entwicklungszeit-IIS-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="69590-114">Enable development-time IIS support</span></span>

<span data-ttu-id="69590-115">Starten Sie Visual Studio-Installer.</span><span class="sxs-lookup"><span data-stu-id="69590-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="69590-116">Wählen Sie die **Entwicklungszeit IIS unterstützen** Komponente.</span><span class="sxs-lookup"><span data-stu-id="69590-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="69590-117">Die Komponente aufgelistet, als optional in der **Zusammenfassung** Bereich für die **ASP.NET und zur Webentwicklung** arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="69590-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="69590-118">Hiermit werden installiert, die [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), ein systemeigenes IIS-Modul, das zum Ausführen von ASP.NET Core apps erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="69590-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="69590-122">Konfigurieren des Projekts</span><span class="sxs-lookup"><span data-stu-id="69590-122">Configure the project</span></span>

<span data-ttu-id="69590-123">Erstellen Sie ein neues Startprofil, um die Entwicklungszeit-IIS-Unterstützung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="69590-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="69590-124">Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="69590-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="69590-125">Klicken Sie auf die Registerkarte **Debuggen**. Wählen Sie **IIS** aus dem Dropdownfeld **Start** aus.</span><span class="sxs-lookup"><span data-stu-id="69590-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="69590-126">Bestätigen Sie, dass die Funktion **Launch browser** (Browser starten) mit der richtigen URL aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="69590-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="69590-131">Auch manuell hinzufügen ein Profils starten, das die [launchSettings.json](http://json.schemastore.org/launchsettings) Datei in der app:</span><span class="sxs-lookup"><span data-stu-id="69590-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="69590-132">Visual Studio möglicherweise einen Neustart aufgefordert, wenn Sie nicht als Administrator ausführen.</span><span class="sxs-lookup"><span data-stu-id="69590-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="69590-133">Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="69590-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69590-134">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="69590-134">Additional resources</span></span>

* [<span data-ttu-id="69590-135">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="69590-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="69590-136">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="69590-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="69590-137">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="69590-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
