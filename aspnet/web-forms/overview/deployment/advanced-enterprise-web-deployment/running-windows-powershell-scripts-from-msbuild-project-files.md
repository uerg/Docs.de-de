---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "Ausführen von Windows PowerShell-Skripts von MSBuild-Projektdateien | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Prozesses Build- und Bereitstellungsprozess ausgeführt wird. Sie können ein Skript lokal ausführen (das heißt, auf die b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="9e4be-104">Ausführen von Windows PowerShell-Skripts von MSBuild-Projektdateien</span><span class="sxs-lookup"><span data-stu-id="9e4be-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="9e4be-105">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9e4be-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9e4be-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="9e4be-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9e4be-107">In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Prozesses Build- und Bereitstellungsprozess ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9e4be-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="9e4be-108">Sie können ein Skript lokal (das heißt, auf dem Buildserver) oder Remote auf einem Ziel-Webserver oder Datenbankserver wie.</span><span class="sxs-lookup"><span data-stu-id="9e4be-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="9e4be-109">Es gibt viele Gründe, warum Sie möglicherweise nach der Bereitstellung Windows PowerShell-Skript ausführen möchten.</span><span class="sxs-lookup"><span data-stu-id="9e4be-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="9e4be-110">Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="9e4be-111">Eine benutzerdefinierte Ereignisquelle zur Registrierung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="9e4be-112">Ein Dateisystemverzeichnis für Uploads zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9e4be-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="9e4be-113">Bereinigen Sie Buildverzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="9e4be-113">Clean up build directories.</span></span>
> - <span data-ttu-id="9e4be-114">Schreiben Sie Einträge in eine benutzerdefinierte Protokolldatei.</span><span class="sxs-lookup"><span data-stu-id="9e4be-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="9e4be-115">Einladen von Benutzern zu einer neu bereitgestellte Webanwendung-e-Mails zu senden.</span><span class="sxs-lookup"><span data-stu-id="9e4be-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="9e4be-116">Erstellen Sie Benutzerkonten mit den entsprechenden Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="9e4be-117">Konfigurieren der Replikation zwischen SQL Server-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="9e4be-118">In diesem Thema erfahren Sie, wie Sie Windows PowerShell-Skripts sowohl lokal als auch Remote über ein benutzerdefiniertes Ziel in einer Microsoft Build Engine (MSBuild)-Projektdatei ausführen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="9e4be-119">Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="9e4be-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="9e4be-120">Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an.</span><span class="sxs-lookup"><span data-stu-id="9e4be-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="9e4be-121">Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="9e4be-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9e4be-122">Übersicht über den Task</span><span class="sxs-lookup"><span data-stu-id="9e4be-122">Task Overview</span></span>

<span data-ttu-id="9e4be-123">Um ein Windows PowerShell-Skript im Rahmen einer automatisierten oder einstufiger Bereitstellung auszuführen, müssen Sie diese allgemeinen Aufgaben ausführen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="9e4be-124">Fügen Sie das Windows PowerShell-Skript zur Projektmappe und zur quellcodeverwaltung hinzu.</span><span class="sxs-lookup"><span data-stu-id="9e4be-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="9e4be-125">Erstellen Sie einen Befehl, der das Windows PowerShell-Skript aufruft.</span><span class="sxs-lookup"><span data-stu-id="9e4be-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="9e4be-126">Versehen Sie reservierten XML-Zeichen in Ihrem Befehl mit Escapezeichen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="9e4be-127">Erstellen Sie ein Ziel in der benutzerdefinierte MSBuild-Projektdatei, und verwenden Sie die **Exec** Task zum Ausführen des Befehls.</span><span class="sxs-lookup"><span data-stu-id="9e4be-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="9e4be-128">In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="9e4be-129">Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit der MSBuild-Ziele und Eigenschaften vertraut sind und Sie erfahren, wie eine benutzerdefinierte MSBuild-Projektdatei zu verwenden, um einen Prozess Build- und Bereitstellungsprozess Laufwerk.</span><span class="sxs-lookup"><span data-stu-id="9e4be-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="9e4be-130">Weitere Informationen finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9e4be-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="9e4be-131">Erstellen und Hinzufügen von Windows PowerShell-Skripts</span><span class="sxs-lookup"><span data-stu-id="9e4be-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="9e4be-132">Die Aufgaben in diesem Thema verwenden Sie ein Windows PowerShell-Beispielskript, mit dem Namen **LogDeploy.ps1** zum Ausführen von Skripts von MSBuild veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="9e4be-133">Die **LogDeploy.ps1** Skript enthält eine einfache Funktion, die einen Eintrag einzeilige in eine Protokolldatei schreibt:</span><span class="sxs-lookup"><span data-stu-id="9e4be-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="9e4be-134">Die **LogDeploy.ps1** Skript akzeptiert zwei Parameter.</span><span class="sxs-lookup"><span data-stu-id="9e4be-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="9e4be-135">Der erste Parameter den vollständigen Pfad darstellt, in die Protokolldatei einen Eintrag hinzugefügt werden soll, und der zweite Parameter darstellt, das Bereitstellungsziel aus, dem in der Protokolldatei aufgezeichnet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="9e4be-136">Wenn Sie das Skript ausführen, fügt eine Zeile in die Protokolldatei im folgenden Format hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e4be-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="9e4be-137">Vornehmen der **LogDeploy.ps1** Skript MSBuild zur Verfügung, müssen Sie:</span><span class="sxs-lookup"><span data-stu-id="9e4be-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="9e4be-138">Das Skript zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-138">Add the script to source control.</span></span>
- <span data-ttu-id="9e4be-139">Fügen Sie das Skript, um die Projektmappe in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9e4be-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="9e4be-140">Sie müssen die Bereitstellungsskripts mit Ihrer Lösungsinhalt, unabhängig davon, ob das Skript auf dem Buildserver oder auf einem Remotecomputer ausgeführt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="9e4be-141">Eine Möglichkeit ist das Skript in einen Projektmappenordner hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="9e4be-142">Da Sie Windows PowerShell-Skript als Teil des Bereitstellungsprozesses verwenden möchten, ist es im Beispiel Contact Manager sinnvoll ist, fügen das Skript in den Projektmappenordner veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="9e4be-143">Den Inhalt der Projektmappenordner werden zum Erstellen von Servern als Quellmaterial kopiert.</span><span class="sxs-lookup"><span data-stu-id="9e4be-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="9e4be-144">Allerdings bilden sie kein Teil des Projektausgabe.</span><span class="sxs-lookup"><span data-stu-id="9e4be-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="9e4be-145">Ein Windows PowerShell-Skript ausführen, auf dem Buildserver</span><span class="sxs-lookup"><span data-stu-id="9e4be-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="9e4be-146">In einigen Szenarien empfiehlt es sich um Windows PowerShell-Skripts auf dem Computer ausgeführt wird, in dem Ihre Projekte erstellt.</span><span class="sxs-lookup"><span data-stu-id="9e4be-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="9e4be-147">Beispielsweise können Sie ein Windows PowerShell-Skript verwenden, bereinigen Sie Buildordner oder die Einträge in eine benutzerdefinierte Protokolldatei zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="9e4be-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="9e4be-148">Im Hinblick auf die Syntax ist ein Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausführen dasselbe wie das Ausführen von Windows PowerShell-Skript aus einer normalen Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="9e4be-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="9e4be-149">Sie müssen die ausführbare powershell.exe aufrufen und Verwenden der **– Befehl** Switch, geben Sie die Befehle, die Windows PowerShell ausgeführt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="9e4be-150">(In Windows PowerShell v2, können Sie auch die **– Datei** wechseln).</span><span class="sxs-lookup"><span data-stu-id="9e4be-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="9e4be-151">Der Befehl sollte dieses Format aufweisen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="9e4be-152">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9e4be-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="9e4be-153">Wenn der Pfad für das Skript Leerzeichen enthält, müssen Sie den Dateipfad in einfachen Anführungszeichen, die nach einem kaufmännischen und-Zeichen einschließen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="9e4be-154">Sie können doppelte Anführungszeichen nicht verwenden, da Sie bereits sie verwendet haben, mit den Befehl eingeschlossen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="9e4be-155">Es gibt einige zusätzliche Überlegungen beim Aufrufen dieses Befehls von MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9e4be-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="9e4be-156">Sie sollten zunächst berücksichtigen die **– NonInteractive** Flag, um sicherzustellen, dass das Skript im stillen Modus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9e4be-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="9e4be-157">Als Nächstes sollten Sie enthalten die **– "ExecutionPolicy"** Flag mit einem Wert des entsprechenden Arguments.</span><span class="sxs-lookup"><span data-stu-id="9e4be-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="9e4be-158">Dies gibt an, dass Windows PowerShell wird für Ihr Skript und können Sie die standardausführungsrichtlinie, überschreiben die Ausführung des Skripts möglicherweise verhindert werden, die Ausführungsrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="9e4be-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="9e4be-159">Sie können diese Argumentwerte auswählen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="9e4be-160">Der Wert **uneingeschränkt** lässt Windows PowerShell, führen Sie das Skript, unabhängig davon, ob das Skript nicht signiert ist.</span><span class="sxs-lookup"><span data-stu-id="9e4be-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="9e4be-161">Der Wert **"RemoteSigned"** lässt Windows PowerShell, nicht signierte Skripts auszuführen, die auf dem lokalen Computer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="9e4be-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="9e4be-162">Allerdings müssen die Skripts, die an anderer Stelle erstellt wurden signiert werden.</span><span class="sxs-lookup"><span data-stu-id="9e4be-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="9e4be-163">(In der Praxis können Sie sehr wahrscheinlich nicht zu Windows PowerShell-Skript lokal auf einem Build-Server erstellt haben).</span><span class="sxs-lookup"><span data-stu-id="9e4be-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="9e4be-164">Der Wert **"AllSigned"** lässt Windows PowerShell, nur signierte Skripts auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="9e4be-165">Ist die standardausführungsrichtlinie **eingeschränkte**, dadurch wird verhindert, dass Windows PowerShell alle Skriptdateien ausführen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="9e4be-166">Schließlich müssen Sie alle reservierten XML-Zeichen mit Escapezeichen versehen, die in Windows PowerShell-Befehls auftreten:</span><span class="sxs-lookup"><span data-stu-id="9e4be-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="9e4be-167">Ersetzen Sie einfache Anführungszeichen mit  **&amp;Apos;**</span><span class="sxs-lookup"><span data-stu-id="9e4be-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="9e4be-168">Ersetzen von doppelten Anführungszeichen mit  **&amp;Quot;**</span><span class="sxs-lookup"><span data-stu-id="9e4be-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="9e4be-169">Ersetzen Sie kaufmännische und-Zeichen mit  **&amp;Amp;**</span><span class="sxs-lookup"><span data-stu-id="9e4be-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="9e4be-170">Wenn Sie diese Änderungen vornehmen, wird der Befehl so aussehen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="9e4be-171">Innerhalb der benutzerdefinierte MSBuild-Projektdatei können ein neues Ziel erstellen und Verwenden der **Exec** Aufgabe zum Ausführen dieses Befehls:</span><span class="sxs-lookup"><span data-stu-id="9e4be-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="9e4be-172">Beachten Sie, dass in diesem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9e4be-172">In this example, note that:</span></span>

- <span data-ttu-id="9e4be-173">Alle Variablen, wie Parameterwerte und den Speicherort der ausführbaren Windows PowerShell-Datei werden als MSBuild-Eigenschaften deklariert.</span><span class="sxs-lookup"><span data-stu-id="9e4be-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="9e4be-174">Bedingungen sind enthalten, damit Benutzer diese Werte in der Befehlszeile überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="9e4be-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="9e4be-175">Die **MSDeployComputerName** Eigenschaft an anderer Stelle in der Projektdatei deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="9e4be-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="9e4be-176">Wenn Sie dieses Ziel als Teil des Buildprozesses ausführen, wird Windows PowerShell den Befehl und einen Protokolleintrag schreiben, zu der Datei, die Sie angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="9e4be-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="9e4be-177">Ein Windows PowerShell-Skript ausführen auf einem Remotecomputer</span><span class="sxs-lookup"><span data-stu-id="9e4be-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="9e4be-178">Windows PowerShell ist zur Ausführung von Skripts auf Remotecomputern über [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="9e4be-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="9e4be-179">Zu diesem Zweck müssen Sie verwenden die [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) Cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9e4be-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="9e4be-180">Dadurch können Sie das Skript auf einem oder mehreren Remotecomputern ausführen, ohne kopieren das Skript auf den Remotecomputern.</span><span class="sxs-lookup"><span data-stu-id="9e4be-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="9e4be-181">Ergebnisse werden auf dem lokalen Computer zurückgegeben, in denen Sie das Skript ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9e4be-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="9e4be-182">Vor der Verwendung der **Invoke-Command** -Cmdlet zum Ausführen von Windows PowerShell-Skripts auf einem Remotecomputer befindet, müssen Sie einen WinRM-Listener zur Aufnahme von remote-Nachrichten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9e4be-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="9e4be-183">Hierzu können Sie durch Ausführen des Befehls **Winrm Quickconfig** auf dem Remotecomputer.</span><span class="sxs-lookup"><span data-stu-id="9e4be-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="9e4be-184">Weitere Informationen finden Sie unter [Installation und Konfiguration für Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="9e4be-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="9e4be-185">Im Windows PowerShell-Fenster, verwenden Sie diese Syntax zum Ausführen der **LogDeploy.ps1** Skript auf einem Remotecomputer:</span><span class="sxs-lookup"><span data-stu-id="9e4be-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="9e4be-186">Es gibt verschiedene andere Möglichkeiten der Verwendung von **Invoke-Command** mit einem Skript-Datei, aber dieser Ansatz ist vergleichsweise einfach, wenn Sie Parameter angeben und Verwalten von Pfaden mit Leerzeichen müssen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="9e4be-187">Wenn Sie dies an einer Eingabeaufforderung ausführen, müssen Sie die ausführbare Windows PowerShell aufrufen und Verwenden der **– Befehl** Parameter, um Ihre Anweisungen bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="9e4be-188">Wie vor, müssen Sie einige zusätzliche Switches bereitstellen und escape reservierten XML-Zeichen, wenn Sie den Befehl auf MSBuild ausführen:</span><span class="sxs-lookup"><span data-stu-id="9e4be-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="9e4be-189">Zum Schluss wie zuvor können Sie die **Exec** Aufgabe in eine benutzerdefinierte MSBuild-Ziel zum Ausführen des Befehls:</span><span class="sxs-lookup"><span data-stu-id="9e4be-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="9e4be-190">Wenn Sie dieses Ziel als Teil des Buildprozesses ausführen, führen Windows PowerShell das Skript auf dem Computer, die Sie, in angegeben der **– Computername** Argument.</span><span class="sxs-lookup"><span data-stu-id="9e4be-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9e4be-191">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="9e4be-191">Conclusion</span></span>

<span data-ttu-id="9e4be-192">In diesem Thema beschriebenen Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausführen.</span><span class="sxs-lookup"><span data-stu-id="9e4be-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="9e4be-193">Sie können diesen Ansatz verwenden, ein Windows PowerShell-Skript im Rahmen einer automatisierten Prozesses oder eines einstufiger Build- und Bereitstellungsprozess entweder lokal oder auf einem Remotecomputer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9e4be-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9e4be-194">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="9e4be-194">Further Reading</span></span>

<span data-ttu-id="9e4be-195">Anleitungen zum Signieren von Windows PowerShell-Skripts und Verwalten von Ausführungsrichtlinien finden Sie unter [Ausführen von Windows PowerShell-Skripts](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e4be-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="9e4be-196">Um Hilfe bei der Windows PowerShell-Befehle von einem Remotecomputer ausführen, finden Sie unter [Ausführen von Remotebefehlen](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e4be-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="9e4be-197">Weitere Informationen finden Sie unter benutzerdefinierte MSBuild-Projektdateien um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9e4be-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9e4be-198">[Zurück](taking-web-applications-offline-with-web-deploy.md)
[Weiter](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="9e4be-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
