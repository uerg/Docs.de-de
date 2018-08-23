---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Erste Schritte mit SignalR 2 | Microsoft-Dokumentation'
author: pfletcher
description: Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Pa...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 647dab496acd63dc774236ed448bd6b37b19c707
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832410"
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="63065-104">Tutorial: Erste Schritte mit SignalR 2</span><span class="sxs-lookup"><span data-stu-id="63065-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="63065-105">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="63065-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="63065-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="63065-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="63065-107">Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen.</span><span class="sxs-lookup"><span data-stu-id="63065-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="63065-108">Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen Sie eine HTML-Seite zum Senden und Meldungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="63065-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="63065-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="63065-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="63065-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="63065-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="63065-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="63065-111">.NET 4.5</span></span>
> - <span data-ttu-id="63065-112">SignalR-Version 2</span><span class="sxs-lookup"><span data-stu-id="63065-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="63065-113">In diesem Tutorial mithilfe von Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="63065-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="63065-114">Um Visual Studio 2012 mit diesem Tutorial verwenden möchten, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="63065-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="63065-115">Aktualisieren Ihrer [-Paket-Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="63065-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="63065-116">Installieren Sie die [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="63065-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="63065-117">Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und Web Tools 2013.1 für Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="63065-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="63065-118">Dadurch wird Visual Studio-Vorlagen für SignalR-Klassen wie z. B. installiert **Hub**.</span><span class="sxs-lookup"><span data-stu-id="63065-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="63065-119">Einige Vorlagen (z. B. **OWIN-Startklasse**) nicht zur Verfügung; verwenden Sie für diese stattdessen eine Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="63065-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="63065-120">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="63065-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="63065-121">Weitere Informationen zu früheren Versionen von SignalR, finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="63065-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="63065-122">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="63065-122">Questions and comments</span></span>
> 
> <span data-ttu-id="63065-123">Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="63065-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="63065-124">Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="63065-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="63065-125">Übersicht</span><span class="sxs-lookup"><span data-stu-id="63065-125">Overview</span></span>

<span data-ttu-id="63065-126">In diesem Tutorial wird SignalR-Entwicklung eingeführt, wie eine einfache browserbasierte Chat-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="63065-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="63065-127">Sie werden die SignalR-Bibliothek mit einer leeren ASP.NET-Webanwendung hinzufügen, erstellen Sie eine hubklasse für das Senden von Nachrichten für Clients und erstellen Sie eine HTML-Seite, mit dem Benutzer, senden und Empfangen von Chatnachrichten.</span><span class="sxs-lookup"><span data-stu-id="63065-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="63065-128">Ein ähnliches Tutorial, das Erstellen eine chatanwendung in MVC 5 zeigt die Verwendung einer MVC-Ansicht, finden Sie unter [erste Schritte mit SignalR 2 und MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="63065-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="63065-129">Dieses Tutorial veranschaulicht das Erstellen von SignalR-Anwendungen in Version 2.</span><span class="sxs-lookup"><span data-stu-id="63065-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="63065-130">Weitere Informationen zu Änderungen zwischen SignalR 1.x und 2 finden Sie unter [aktualisieren SignalR 1.x-Projekte](../releases/upgrading-signalr-1x-projects-to-20.md) und [Versionsanmerkungen von Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="63065-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="63065-131">SignalR ist eine Open Source-.NET Bibliothek zum Erstellen von Webanwendungen, die live Benutzerinteraktionen oder datenaktualisierungen in Echtzeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="63065-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="63065-132">Beispiele hierfür sind soziale Anwendungen, mehrere Benutzer Spiele, Business-Zusammenarbeit und Neuigkeiten, Wetter oder finanzielle Anwendungsupdates an.</span><span class="sxs-lookup"><span data-stu-id="63065-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="63065-133">Diese werden häufig in Echtzeit Anwendungen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="63065-133">These are often called real-time applications.</span></span>

<span data-ttu-id="63065-134">SignalR vereinfacht das Erstellen von Echtzeit-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="63065-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="63065-135">Es enthält eine Server-Bibliothek von ASP.NET und eine JavaScript-Clientbibliothek zum Verwalten von Client / Server-Verbindungen ein, und Übertragen von Inhaltsupdates an Clients zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="63065-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="63065-136">Sie können eine vorhandene ASP.NET-Anwendung um Echtzeitfunktionen zu erhalten. die SignalR-Bibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="63065-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="63065-137">Das Tutorial veranschaulicht die folgenden Entwicklungsaufgaben von SignalR:</span><span class="sxs-lookup"><span data-stu-id="63065-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="63065-138">Eine ASP.NET-Webanwendung wird die SignalR-Bibliothek hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="63065-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="63065-139">Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="63065-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="63065-140">Erstellen eine OWIN-Startklasse zum Konfigurieren der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="63065-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="63065-141">Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.</span><span class="sxs-lookup"><span data-stu-id="63065-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="63065-142">Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="63065-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="63065-143">Jeden neuer Benutzer kann Kommentare Posten und Kommentare hinzugefügt, nachdem der Benutzer im Chat verknüpft.</span><span class="sxs-lookup"><span data-stu-id="63065-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Chatinstanzen](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="63065-145">Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="63065-145">Sections:</span></span>

- [<span data-ttu-id="63065-146">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="63065-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="63065-147">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="63065-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="63065-148">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="63065-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="63065-149">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="63065-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="63065-150">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="63065-150">Set up the Project</span></span>

<span data-ttu-id="63065-151">Fügen Sie in diesem Abschnitt zeigt, wie Sie Visual Studio 2013 und SignalR Version 2 zu verwenden, um eine leere ASP.NET-Webanwendung erstellen SignalR hinzu, und erstellen Sie die chatanwendung.</span><span class="sxs-lookup"><span data-stu-id="63065-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="63065-152">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="63065-152">Prerequisites:</span></span>

- <span data-ttu-id="63065-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="63065-153">Visual Studio 2013.</span></span> <span data-ttu-id="63065-154">Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET-Downloads](https://www.asp.net/downloads) um die kostenlose Visual Studio 2013 Express Entwicklungstool zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="63065-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="63065-155">Verwenden Sie die folgenden Schritte aus Visual Studio 2013 erstellen eine leere ASP.NET-Webanwendung und Hinzufügen von SignalR-Bibliothek:</span><span class="sxs-lookup"><span data-stu-id="63065-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="63065-156">Erstellen Sie eine ASP.NET-Webanwendung in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="63065-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Erstellen Sie web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="63065-158">In der **neues ASP.NET-Projekt** Fenster verlassen **leere** ausgewählt, und klicken Sie auf **Erstellen eines Projekts**.</span><span class="sxs-lookup"><span data-stu-id="63065-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Leeres Web erstellen](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="63065-160">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | SignalR-Hubklasse (v2)**.</span><span class="sxs-lookup"><span data-stu-id="63065-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="63065-161">Nennen Sie die Klasse **ChatHub.cs** und fügen sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="63065-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="63065-162">Dieser Schritt erstellt der **ChatHub** -Klasse und fügt Sie dem Projekt einen Satz von Skriptdateien und Assemblyverweise, die SignalR unterstützen.</span><span class="sxs-lookup"><span data-stu-id="63065-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63065-163">Sie können auch SignalR zu einem Projekt hinzufügen, indem Sie öffnen die **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole** und einen Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="63065-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="63065-164">Wenn Sie die Konsole verwenden, um SignalR hinzuzufügen, erstellen Sie die SignalR-hubklasse als separater Schritt nach dem Hinzufügen von SignalR.</span><span class="sxs-lookup"><span data-stu-id="63065-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63065-165">Wenn Sie Visual Studio 2012, verwenden die **SignalR-Hubklasse (v2)** Vorlage nicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="63065-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="63065-166">Sie können ein einfacher hinzufügen **Klasse** namens `ChatHub` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="63065-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="63065-167">In **Projektmappen-Explorer**, erweitern Sie den Knoten des Skripts.</span><span class="sxs-lookup"><span data-stu-id="63065-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="63065-168">Skriptbibliotheken für jQuery und SignalR werden in das Projekt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="63065-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="63065-169">Ersetzen Sie den Code in der neuen **ChatHub** Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="63065-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="63065-170">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | OWIN-Startklasse**.</span><span class="sxs-lookup"><span data-stu-id="63065-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="63065-171">Nennen Sie die neue Klasse `Startup` , und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="63065-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63065-172">Wenn Sie Visual Studio 2012, verwenden die **OWIN-Startklasse** Vorlage nicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="63065-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="63065-173">Sie können ein einfacher hinzufügen **Klasse** namens `Startup` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="63065-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="63065-174">Ändern Sie den Inhalt der neuen Startup-Klasse, die der folgenden.</span><span class="sxs-lookup"><span data-stu-id="63065-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="63065-175">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | HTML-Seite**.</span><span class="sxs-lookup"><span data-stu-id="63065-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="63065-176">Nennen Sie die neue Seite `index.html`.</span><span class="sxs-lookup"><span data-stu-id="63065-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="63065-177">Sie müssen möglicherweise die Versionsnummern für die Verweise auf JQuery und SignalR-Bibliotheken zu ändern</span><span class="sxs-lookup"><span data-stu-id="63065-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="63065-178">In **Projektmappen-Explorer**mit der rechten Maustaste auf die HTML-Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.</span><span class="sxs-lookup"><span data-stu-id="63065-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="63065-179">Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="63065-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63065-180">Eine höhere Version von SignalR-Skripts kann durch den Paket-Manager installiert werden.</span><span class="sxs-lookup"><span data-stu-id="63065-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="63065-181">Stellen Sie sicher, dass die folgenden Skriptverweise zu den Versionen der Skriptdateien im Projekt entsprechen (sie werden anders, wenn Sie SignalR, einen Hub hinzufügen, sondern mithilfe von NuGet hinzugefügt haben.)</span><span class="sxs-lookup"><span data-stu-id="63065-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="63065-182">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="63065-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="63065-183">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="63065-183">Run the Sample</span></span>

1. <span data-ttu-id="63065-184">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="63065-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="63065-185">Die HTML-Seite lädt in eine Instanz eines Webbrowsers und aufforderungen für einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="63065-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="63065-187">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="63065-187">Enter a user name.</span></span>
3. <span data-ttu-id="63065-188">Kopieren Sie die URL aus der Adresszeile des Browsers, und öffnen Sie zwei Browserinstanzen für weitere.</span><span class="sxs-lookup"><span data-stu-id="63065-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="63065-189">Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="63065-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="63065-190">Klicken Sie in den einzelnen Instanzen Browser Fügt einen Kommentar hinzu, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="63065-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="63065-191">Die Kommentare sollte in alle Browserinstanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="63065-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63065-192">Diese einfache chatanwendung werden den Kontext der Diskussion auf dem Server nicht verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="63065-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="63065-193">Der Hub sendet, Kommentare, um alle aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="63065-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="63065-194">Benutzer, die im Chat später zusammenfügen sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie aufgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="63065-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="63065-195">Der folgende Screenshot zeigt die Chat-Anwendung, die auf drei Browserinstanzen, die aktualisiert werden, wenn eine Instanz eine Nachricht sendet:</span><span class="sxs-lookup"><span data-stu-id="63065-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="63065-197">In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="63065-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="63065-198">Es gibt eine Skriptdatei namens **Hubs** , die den SignalR-Bibliothek wird zur Laufzeit dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="63065-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="63065-199">Diese Datei, verwaltet die Kommunikation zwischen der jQuery-Skripts und serverseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="63065-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="63065-200">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="63065-200">Examine the Code</span></span>

<span data-ttu-id="63065-201">Der SignalR Chat-Anwendung zeigt zwei grundlegende Aufgaben für die SignalR-Entwicklung: Erstellen eines Hubs wie das wichtigste Coordination-Objekt, auf dem Server, und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="63065-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="63065-202">SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="63065-202">SignalR Hubs</span></span>

<span data-ttu-id="63065-203">Im Codebeispiel enthält die **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse.</span><span class="sxs-lookup"><span data-stu-id="63065-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="63065-204">Ableiten von der **Hub** Klasse ist eine gute Möglichkeit, eine SignalR-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="63065-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="63065-205">Sie können öffentliche Methoden auf Ihrem Hub-Klasse erstellen und dann Zugriff auf diese Methoden durch einen Aufruf über Skripts auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="63065-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="63065-206">Im Code Chat Clients rufen die **ChatHub.Send** Methode, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="63065-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="63065-207">Der Hub wiederum sendet die Nachricht an alle Clients durch den Aufruf **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="63065-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="63065-208">Die **senden** -Methode demonstriert, mehrere Hub-Konzepte:</span><span class="sxs-lookup"><span data-stu-id="63065-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="63065-209">Deklarieren Sie öffentliche Methoden für einen Hub, damit Clients, die sie aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="63065-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="63065-210">Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** dynamische Eigenschaft alle Clients den Zugriff auf, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="63065-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="63065-211">Aufrufen einer Funktion auf dem Client (z. B. die `broadcastMessage` Funktion) zum Aktualisieren von Clients.</span><span class="sxs-lookup"><span data-stu-id="63065-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="63065-212">SignalR und jQuery</span><span class="sxs-lookup"><span data-stu-id="63065-212">SignalR and jQuery</span></span>

<span data-ttu-id="63065-213">Die HTML-Seite im Codebeispiel veranschaulicht, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit einer SignalR-Hub.</span><span class="sxs-lookup"><span data-stu-id="63065-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="63065-214">Die wesentlichen Aufgaben in den Code deklarieren einen Proxy für den Hub, Deklarieren einer Funktion, die Inhalt per Pushvorgang an Clients der Server aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="63065-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="63065-215">Der folgende Code deklariert einen Verweis auf eine hubproxy-Klasse.</span><span class="sxs-lookup"><span data-stu-id="63065-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="63065-216">In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case.</span><span class="sxs-lookup"><span data-stu-id="63065-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="63065-217">Das Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="63065-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="63065-218">Der folgende Code ist, wie Sie eine Callback-Funktion im Skript erstellen.</span><span class="sxs-lookup"><span data-stu-id="63065-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="63065-219">Die hubklasse auf dem Server ruft diese Funktion, um Inhaltsupdates per Pushvorgang an jeden Client übertragen.</span><span class="sxs-lookup"><span data-stu-id="63065-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="63065-220">Die beiden Zeilen an, dass die HTML-Codierung vor der Anzeige des Inhalts sind optional, und zeigen eine einfache Möglichkeit zum Skript-Injection verhindern.</span><span class="sxs-lookup"><span data-stu-id="63065-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="63065-221">Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="63065-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="63065-222">Der Code wird die Verbindung gestartet und leitet diese dann in eine Funktion zum Behandeln der Click-Ereignis auf der **senden** auf der HTML-Seite.</span><span class="sxs-lookup"><span data-stu-id="63065-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="63065-223">Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wurde, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="63065-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="63065-224">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="63065-224">Next Steps</span></span>

<span data-ttu-id="63065-225">Sie haben gelernt, dass es sich bei SignalR handelt es sich ein Framework zum Erstellen von Echtzeit-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="63065-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="63065-226">Sie erfahren auch mehrere Aufgaben bei der Entwicklung von SignalR: wie eine ASP.NET-Anwendung SignalR hinzugefügt, wie zum Erstellen einer hubklasse und das Senden und Empfangen von Nachrichten aus dem Hub.</span><span class="sxs-lookup"><span data-stu-id="63065-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="63065-227">Eine exemplarische Vorgehensweise, um die SignalR-beispielanwendung in Azure bereitstellen, finden Sie unter [mithilfe von SignalR mit Web-Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="63065-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="63065-228">Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [ASP.NET Web-app in Azure App Service erstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="63065-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="63065-229">Erweiterte Konzepte der SignalR-Entwicklungen finden Sie unter den folgenden Websites für SignalR-Quellcode und Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="63065-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="63065-230">SignalR-Projekt</span><span class="sxs-lookup"><span data-stu-id="63065-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="63065-231">SignalR Github und Beispiele</span><span class="sxs-lookup"><span data-stu-id="63065-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="63065-232">SignalR-Wiki</span><span class="sxs-lookup"><span data-stu-id="63065-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
