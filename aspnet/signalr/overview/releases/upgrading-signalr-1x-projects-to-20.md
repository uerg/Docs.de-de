---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Aktualisieren von SignalR 1.x-Projekten auf Version 2 | Microsoft Docs
author: pfletcher
description: In diesem Thema wird beschrieben, wie ein vorhandenes SignalR 1.x-Projekt auf SignalR aktualisiert 2.x sowie zum Behandeln von Problemen, die auftreten können, während des Upgradevorgangs...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505739"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="f0171-103">Aktualisieren von SignalR 1.x-Projekten auf Version 2</span><span class="sxs-lookup"><span data-stu-id="f0171-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="f0171-104">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f0171-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="f0171-105">In diesem Thema wird beschrieben, wie ein vorhandenes SignalR 1.x-Projekt auf SignalR aktualisiert 2.x sowie zum Behandeln von Problemen, die während des Upgrades auftreten können.</span><span class="sxs-lookup"><span data-stu-id="f0171-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f0171-106">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="f0171-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f0171-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f0171-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f0171-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f0171-108">.NET 4.5</span></span>
> - <span data-ttu-id="f0171-109">SignalR-Version 1 und 2</span><span class="sxs-lookup"><span data-stu-id="f0171-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="f0171-110">Verwenden von Visual Studio 2012 mit diesem Lernprogramm</span><span class="sxs-lookup"><span data-stu-id="f0171-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="f0171-111">Um Visual Studio 2012 mit diesem Lernprogramm verwenden, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="f0171-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="f0171-112">Update der [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="f0171-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="f0171-113">Installieren der [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0171-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="f0171-114">Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und 2013.1 von Web Tools für Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="f0171-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="f0171-115">Hierbei werden z. B. Visual Studio-Vorlagen für SignalR-Klassen installiert **Hub**.</span><span class="sxs-lookup"><span data-stu-id="f0171-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="f0171-116">Einige Vorlagen (z. B. **OWIN-Startklasse**) ist nicht verfügbar; verwenden Sie für diese stattdessen eine Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="f0171-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f0171-117">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="f0171-117">Questions and comments</span></span>
> 
> <span data-ttu-id="f0171-118">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="f0171-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f0171-119">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f0171-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f0171-120">SignalR 2 bietet eine konsistente entwicklungserfahrung über Server-Plattformen mit [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="f0171-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="f0171-121">Dieser Artikel beschreibt die Schritte, die erforderlich sind, eine SignalR-1.x-Anwendung auf Version 2 aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="f0171-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="f0171-122">Während es empfohlen wird, um Anwendungen für SignalR 2 zu aktualisieren, werden SignalR 1.x weiterhin unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f0171-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="f0171-123">In diesem Lernprogramm beschreibt, wie eine im Internet gehostete Anwendung mit SignalR-2 zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="f0171-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="f0171-124">Selbst gehostete Anwendungen (diejenigen, die ein Server in einer Konsolenanwendung, Windows-Dienst oder ein anderer Prozess hosten) werden nun unter SignalR-2 unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f0171-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="f0171-125">Informationen zu den ersten Schritten beim Erstellen einer selbst gehosteten Anwendung mit SignalR-2 finden Sie unter [Lernprogramm: SignalR Selbsthosting](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="f0171-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="f0171-126">Inhalt</span><span class="sxs-lookup"><span data-stu-id="f0171-126">Contents</span></span>

<span data-ttu-id="f0171-127">Den folgenden Abschnitten werden die Aufgaben im Zusammenhang mit der Aktualisierung von SignalR-Projekte und Behandlung von Problemen, die auftreten können.</span><span class="sxs-lookup"><span data-stu-id="f0171-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="f0171-128">Beispiel: Das erste Schritte-Lernprogramm auf SignalR 2 aktualisieren</span><span class="sxs-lookup"><span data-stu-id="f0171-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="f0171-129">Problembehandlung bei Fehler während des Upgrades</span><span class="sxs-lookup"><span data-stu-id="f0171-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="f0171-130">Beispiel: Aktualisieren der Getting Started Tutorial Anwendung SignalR 2</span><span class="sxs-lookup"><span data-stu-id="f0171-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="f0171-131">In diesem Abschnitt Aktualisieren Sie die Anwendung in der [Version 1.x SignalR das Lernprogramm für erste Schritte](../older-versions/index.md) SignalR 2 verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0171-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="f0171-132">Nachdem Sie das erste Schritte-Lernprogramm abgeschlossen haben, mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="f0171-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="f0171-133">Überprüfen Sie, ob die **Zielframework** festgelegt ist, um **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="f0171-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="f0171-134">Öffnen Sie die Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="f0171-134">Open the Package Manager Console.</span></span> <span data-ttu-id="f0171-135">Entfernen von SignalR 1.x aus dem Projekt mit dem folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="f0171-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="f0171-136">Installieren Sie die SignalR-2, mithilfe des folgenden Befehls:</span><span class="sxs-lookup"><span data-stu-id="f0171-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="f0171-137">Aktualisieren Sie in der HTML-Seite für SignalR entsprechend der Version des jetzt im Projekt enthaltenen Skripts wird der Verweis auf das Skript ein.</span><span class="sxs-lookup"><span data-stu-id="f0171-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="f0171-138">Entfernen Sie den Aufruf MapHubs, in der Globale Anwendungsklasse.</span><span class="sxs-lookup"><span data-stu-id="f0171-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="f0171-139">Maustaste auf die Projektmappe, und wählen Sie **hinzufügen**, **neues Element...** . Wählen Sie im Dialogfeld **Owin-Startklasse**.</span><span class="sxs-lookup"><span data-stu-id="f0171-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="f0171-140">Benennen Sie die neue Klasse **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="f0171-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="f0171-141">Ersetzen Sie den Inhalt der tritt durch den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="f0171-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="f0171-142">Das Assemblyattribut Owins-Startprozess, die ausgeführt wird die Klasse hinzugefügt die `Configuration` Methode, wenn die Owin-gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="f0171-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="f0171-143">Dies wiederum ruft die `MapSignalR` -Methode, die Routen für alle SignalR-Hubs in der Anwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="f0171-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="f0171-144">Führen Sie das Projekt, und kopieren Sie die URL des der Hauptseite in einen anderen Browser oder Browserbereich als vor.</span><span class="sxs-lookup"><span data-stu-id="f0171-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="f0171-145">Jeder Seite wird ein Benutzername angefordert, und Nachrichten für jede Seite in beide Bereiche Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f0171-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="f0171-146">Problembehandlung bei Fehler während des Upgrades</span><span class="sxs-lookup"><span data-stu-id="f0171-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="f0171-147">Dieser Abschnitt beschreibt die Probleme, die während des Upgrades auftreten können.</span><span class="sxs-lookup"><span data-stu-id="f0171-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="f0171-148">Eine umfangreichere Liste von Fehlern und Problemen, die mit einer SignalR-Anwendung auftreten können, finden Sie unter [SignalR Problembehandlung](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f0171-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="f0171-149">"Der Aufruf ist zwischen den folgenden Methoden oder Eigenschaften"</span><span class="sxs-lookup"><span data-stu-id="f0171-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="f0171-150">Dieser Fehler tritt auf, wenn ein Verweis auf `Microsoft.AspNet.SignalR.Owin` wird nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="f0171-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="f0171-151">Dieses Paket ist veraltet. der Verweis muss entfernt werden, und die 1.x-Version des Pakets SelfHost muss deinstalliert werden.</span><span class="sxs-lookup"><span data-stu-id="f0171-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="f0171-152">Hubmethoden fehl automatisch.</span><span class="sxs-lookup"><span data-stu-id="f0171-152">Hub methods fail silently</span></span>

<span data-ttu-id="f0171-153">Überprüfen Sie, ob die Skriptverweise in Ihrem Client bis zum Datum, und dass die `OwinStartup` -Attribut für die Startklasse. die richtige Klasse und den Assemblynamen für das Projekt wurde.</span><span class="sxs-lookup"><span data-stu-id="f0171-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="f0171-154">Versuchen Sie außerdem, die Hubs-Adresse (/ Signalr/Hubs) in Ihrem Browser öffne; Jeder Fehler, der angezeigt wird bietet weitere Informationen zu können, wo der Fehler ist.</span><span class="sxs-lookup"><span data-stu-id="f0171-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
