---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SignalR mit horizontaler Skalierung mit SQLServer | Microsoft Docs
author: MikeWasson
description: Versionen der Software verwendete in diesem Thema Visual Studio 2013 .NET 4.5 SignalR Version 2-Vorgängerversionen des in diesem Thema Informationen zu früheren Versionen von...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874368"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="0446f-103">SignalR mit horizontaler Skalierung mit SQLServer</span><span class="sxs-lookup"><span data-stu-id="0446f-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="0446f-104">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0446f-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0446f-105">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="0446f-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="0446f-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0446f-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0446f-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0446f-107">.NET 4.5</span></span>
> - <span data-ttu-id="0446f-108">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="0446f-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0446f-109">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="0446f-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="0446f-110">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0446f-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="0446f-111">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="0446f-111">Questions and comments</span></span>
> 
> <span data-ttu-id="0446f-112">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="0446f-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0446f-113">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0446f-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="0446f-114">In diesem Lernprogramm verwenden Sie SQL Server zum Verteilen von Nachrichten auf einer SignalR-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="0446f-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="0446f-115">Sie können auch in diesem Lernprogramm ausführen, auf einen einzelnen Testcomputer, aber um vollständige Transparenzeffekt zu erzielen, müssen Sie die SignalR-Anwendung auf zwei oder mehr Servern bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0446f-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="0446f-116">Sie müssen SQL Server auch auf einem Server oder auf einem separaten dedizierten Server installieren.</span><span class="sxs-lookup"><span data-stu-id="0446f-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="0446f-117">Eine andere Möglichkeit ist das Lernprogramm mithilfe von virtuellen Computern in Azure ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="0446f-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="0446f-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="0446f-118">Prerequisites</span></span>

<span data-ttu-id="0446f-119">Microsoft SQLServer 2005 oder höher.</span><span class="sxs-lookup"><span data-stu-id="0446f-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="0446f-120">Der Rückwand unterstützt Desktop- und Server-Editionen von SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0446f-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="0446f-121">Er unterstützt keine SQL Server Compact Edition oder Azure SQL-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0446f-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="0446f-122">(Wenn die Anwendung in Azure gehostet wird, empfiehlt es sich die Service Bus-Rückwandplatine stattdessen.)</span><span class="sxs-lookup"><span data-stu-id="0446f-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="0446f-123">Übersicht</span><span class="sxs-lookup"><span data-stu-id="0446f-123">Overview</span></span>

<span data-ttu-id="0446f-124">Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.</span><span class="sxs-lookup"><span data-stu-id="0446f-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="0446f-125">Erstellen Sie eine neue leere Datenbank ein.</span><span class="sxs-lookup"><span data-stu-id="0446f-125">Create a new empty database.</span></span> <span data-ttu-id="0446f-126">Der Rückwand erstellt die erforderlichen Tabellen in dieser Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0446f-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="0446f-127">Ihre Anwendung diese NuGet-Pakete hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="0446f-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="0446f-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="0446f-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="0446f-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="0446f-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="0446f-130">Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0446f-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="0446f-131">Fügen Sie den folgenden Code, um tritt an der Rückwand zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="0446f-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="0446f-132">Dieser Code dient zum Konfigurieren der Rückwand mit den Standardwerten für [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) und [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="0446f-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="0446f-133">Informationen zum Ändern dieser Werte finden Sie unter [SignalR-Leistung: Warteschlangen für horizontale Skalierung Metriken](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="0446f-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="0446f-134">Konfigurieren Sie die Datenbank</span><span class="sxs-lookup"><span data-stu-id="0446f-134">Configure the Database</span></span>

<span data-ttu-id="0446f-135">Entscheiden Sie, ob die Anwendung Windows-Authentifizierung oder SQL Server-Authentifizierung für den Datenbankzugriff verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0446f-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="0446f-136">Unabhängig davon, stellen Sie sicher, dass der Datenbankbenutzer hat Berechtigungen zum Anmelden, Erstellen von Schemas und Tabellen erstellen.</span><span class="sxs-lookup"><span data-stu-id="0446f-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="0446f-137">Erstellen Sie eine neue Datenbank für die Backplane verwenden.</span><span class="sxs-lookup"><span data-stu-id="0446f-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="0446f-138">Sie können der Datenbank auf einen beliebigen Namen geben.</span><span class="sxs-lookup"><span data-stu-id="0446f-138">You can give the database any name.</span></span> <span data-ttu-id="0446f-139">Sie müssen keine Tabellen in der Datenbank zu erstellen. der Rückwand erstellt die erforderlichen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="0446f-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="0446f-140">Aktivieren Sie Service Broker</span><span class="sxs-lookup"><span data-stu-id="0446f-140">Enable Service Broker</span></span>

<span data-ttu-id="0446f-141">Es wird empfohlen, zum Aktivieren von Service Broker für die Datenbank Rückwandplatine.</span><span class="sxs-lookup"><span data-stu-id="0446f-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="0446f-142">Service Broker bietet systemeigene Unterstützung für Messaging- und Warteschlangen in SQL Server, letztere der Rückwand Updates noch effizienter zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="0446f-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="0446f-143">(Allerdings funktioniert der Rückwand auch ohne Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="0446f-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="0446f-144">Um zu überprüfen, ob Service Broker aktiviert ist, Fragen Sie die **ist\_Broker\_aktiviert** Spalte in der **sys.databases** -Katalogsicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0446f-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="0446f-145">Um Service Broker zu aktivieren, verwenden Sie die folgende SQL-Abfrage:</span><span class="sxs-lookup"><span data-stu-id="0446f-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="0446f-146">Wenn diese Abfrage angezeigt wird, ein Deadlock auftreten, stellen Sie sicher, dass keine Programme, die mit der Datenbank verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="0446f-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="0446f-147">Wenn Sie die Ablaufverfolgung aktiviert haben, die ablaufverfolgungen ebenfalls angezeigt, ob Service Broker aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="0446f-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="0446f-148">Erstellen einer SignalR-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0446f-148">Create a SignalR Application</span></span>

<span data-ttu-id="0446f-149">Erstellen einer SignalR-Anwendung entweder mit diesen Lernprogrammen:</span><span class="sxs-lookup"><span data-stu-id="0446f-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="0446f-150">Erste Schritte mit SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="0446f-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="0446f-151">Erste Schritte mit SignalR 2.0 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="0446f-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="0446f-152">Ändern Sie als Nächstes die chatanwendung zur Unterstützung von Warteschlangen für horizontale Skalierung mit SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0446f-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="0446f-153">Fügen Sie zuerst das SignalR.SqlServer NuGet-Paket zum Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="0446f-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="0446f-154">In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="0446f-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0446f-155">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="0446f-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="0446f-156">Öffnen Sie als Nächstes die Startup.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="0446f-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="0446f-157">Fügen Sie folgenden Code, der **konfigurieren** Methode:</span><span class="sxs-lookup"><span data-stu-id="0446f-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="0446f-158">Bereitstellen und Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0446f-158">Deploy and Run the Application</span></span>

<span data-ttu-id="0446f-159">Vorbereiten der Windows Server-Instanzen, die SignalR-Anwendung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="0446f-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="0446f-160">Fügen Sie die IIS-Rolle hinzu.</span><span class="sxs-lookup"><span data-stu-id="0446f-160">Add the IIS role.</span></span> <span data-ttu-id="0446f-161">Schließen Sie "Anwendungsentwicklung"-Funktionen, einschließlich des WebSocket-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="0446f-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="0446f-162">Außerdem enthalten Sie den Verwaltungsdienst (unter "Verwaltungstools" aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="0446f-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="0446f-163">**Installieren Sie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="0446f-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="0446f-164">Wenn Sie die IIS-Manager ausführen, werden Sie zum Installieren von Microsoft-Webplattform aufgefordert, oder Sie können [Herunterladen der Intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="0446f-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="0446f-165">Klicken Sie in der Webplattform-Installer Web Deploy suchen Sie, und installieren Sie Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="0446f-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="0446f-166">Überprüfen Sie, dass die Web-Management-Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="0446f-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="0446f-167">Wenn dies nicht der Fall ist, starten Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="0446f-167">If not, start the service.</span></span> <span data-ttu-id="0446f-168">(Wenn Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den Management-Dienst installiert, wenn Sie die IIS-Rolle hinzugefügt.)</span><span class="sxs-lookup"><span data-stu-id="0446f-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="0446f-169">Öffnen Sie zum Schluss Port 8172 für TCP.</span><span class="sxs-lookup"><span data-stu-id="0446f-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="0446f-170">Dies ist der Port, den der Web Deploy-Tool verwendet.</span><span class="sxs-lookup"><span data-stu-id="0446f-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="0446f-171">Jetzt sind Sie bereit für die Bereitstellung von Visual Studio-Projekt vom Entwicklungscomputer auf den Server.</span><span class="sxs-lookup"><span data-stu-id="0446f-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="0446f-172">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="0446f-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="0446f-173">Ausführlichere Dokumentation zu Web Deploy finden Sie unter [ASP.NET-Bereitstellung für Visual Studio und ASP.NET Web](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="0446f-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="0446f-174">Wenn Sie die Anwendung mit zwei Servern bereitstellen, können jede Instanz in einem separaten Browserfenster zu öffnen und sehen, dass jeder SignalR-Nachrichten aus den anderen erhält.</span><span class="sxs-lookup"><span data-stu-id="0446f-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="0446f-175">(Selbstverständlich würden in einer produktionsumgebung die beiden Servern hinter einem Lastenausgleich sit.)</span><span class="sxs-lookup"><span data-stu-id="0446f-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="0446f-176">Nachdem Sie die Anwendung ausführen, sehen Sie sich, dass SignalR automatisch Tabellen in der Datenbank erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="0446f-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="0446f-177">SignalR verwendet die Tabellen.</span><span class="sxs-lookup"><span data-stu-id="0446f-177">SignalR manages the tables.</span></span> <span data-ttu-id="0446f-178">Solange die Anwendung bereitgestellt wurde, keine löschen Sie Zeilen, ändern Sie die Tabelle, usw.</span><span class="sxs-lookup"><span data-stu-id="0446f-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
