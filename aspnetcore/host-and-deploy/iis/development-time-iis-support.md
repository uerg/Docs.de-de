---
title: "IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core"
author: shirhatti
description: "Ermitteln Sie die Unterstützung für das Debuggen von ASP.NET Core-apps, wenn IIS unter Windows Server zurückliegt."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="89ffc-103">IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89ffc-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="89ffc-104">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="89ffc-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="89ffc-105">Dieser Artikel beschreibt [Visual Studio](https://www.visualstudio.com/vs/) Unterstützung für das Debuggen von ASP.NET Core-apps, die hinter IIS unter Windows Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="89ffc-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="89ffc-106">Dieses Thema führt Sie durch die Aktivierung dieser Funktion und Einrichten eines Projekts.</span><span class="sxs-lookup"><span data-stu-id="89ffc-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89ffc-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="89ffc-107">Prerequisites</span></span>

* <span data-ttu-id="89ffc-108">Visual Studio (2017/Version 15.3 oder höher)</span><span class="sxs-lookup"><span data-stu-id="89ffc-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="89ffc-109">ASP.NET und die Workload für die Webentwicklung *ODER* die plattformübergreifende Workload für die .NET Core-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="89ffc-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="89ffc-110">Aktivieren von IIS</span><span class="sxs-lookup"><span data-stu-id="89ffc-110">Enable IIS</span></span>

<span data-ttu-id="89ffc-111">Aktivieren Sie IIS.</span><span class="sxs-lookup"><span data-stu-id="89ffc-111">Enable IIS.</span></span> <span data-ttu-id="89ffc-112">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="89ffc-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="89ffc-113">Aktivieren Sie das Kontrollkästchen **Internetinformationsdienste**.</span><span class="sxs-lookup"><span data-stu-id="89ffc-113">Select the **Internet Information Services** checkbox.</span></span>

![Windows-Features, die das aktivierte Kontrollkästchen der Internetinformationsdienste als schwarzes Quadrat anzeigt (nicht mit einem Häkchen), was angibt, dass einige der IIS-Features aktiviert sind](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="89ffc-115">Wenn die IIS-Installation ein Neustart erforderlich ist, starten Sie das System neu.</span><span class="sxs-lookup"><span data-stu-id="89ffc-115">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="89ffc-116">Aktivieren der Entwicklungszeit-IIS-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="89ffc-116">Enable development-time IIS support</span></span>

<span data-ttu-id="89ffc-117">Starten Sie Visual Studio-Installer.</span><span class="sxs-lookup"><span data-stu-id="89ffc-117">Launch the Visual Studio installer.</span></span> <span data-ttu-id="89ffc-118">Wählen Sie die **Entwicklungszeit IIS unterstützen** Komponente.</span><span class="sxs-lookup"><span data-stu-id="89ffc-118">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="89ffc-119">Die Komponente aufgelistet, als optional in der **Zusammenfassung** Bereich für die **ASP.NET und zur Webentwicklung** arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="89ffc-119">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="89ffc-120">Hiermit werden installiert, die [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), ein systemeigenes IIS-Modul, das zum Ausführen von ASP.NET Core apps erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="89ffc-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="89ffc-124">Konfigurieren des Projekts</span><span class="sxs-lookup"><span data-stu-id="89ffc-124">Configure the project</span></span>

<span data-ttu-id="89ffc-125">Erstellen Sie ein neues Startprofil, um die Entwicklungszeit-IIS-Unterstützung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="89ffc-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="89ffc-126">Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="89ffc-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="89ffc-127">Klicken Sie auf die Registerkarte **Debuggen**. Wählen Sie **IIS** aus dem Dropdownfeld **Start** aus.</span><span class="sxs-lookup"><span data-stu-id="89ffc-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="89ffc-128">Bestätigen Sie, dass die Funktion **Launch browser** (Browser starten) mit der richtigen URL aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="89ffc-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="89ffc-133">Auch manuell hinzufügen ein Profils starten, das die [launchSettings.json](http://json.schemastore.org/launchsettings) Datei in der app:</span><span class="sxs-lookup"><span data-stu-id="89ffc-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="89ffc-134">Visual Studio möglicherweise einen Neustart aufgefordert, wenn Sie nicht als Administrator ausführen.</span><span class="sxs-lookup"><span data-stu-id="89ffc-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="89ffc-135">Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="89ffc-135">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89ffc-136">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="89ffc-136">Additional resources</span></span>

* [<span data-ttu-id="89ffc-137">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="89ffc-137">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="89ffc-138">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="89ffc-138">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="89ffc-139">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="89ffc-139">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
