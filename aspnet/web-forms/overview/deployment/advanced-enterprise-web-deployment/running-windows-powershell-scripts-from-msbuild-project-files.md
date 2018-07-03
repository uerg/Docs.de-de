---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ausführen von Windows PowerShell-Skripts aus MSBuild-Projektdateien | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Build & Deployment-Prozesses ausgeführt wird. Sie können ein Skript lokal ausführen (das heißt, auf die b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362894"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="81aeb-104">Ausführen von Windows PowerShell-Skripts aus MSBuild-Projektdateien</span><span class="sxs-lookup"><span data-stu-id="81aeb-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="81aeb-105">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="81aeb-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="81aeb-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="81aeb-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="81aeb-107">In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Build & Deployment-Prozesses ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="81aeb-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="81aeb-108">Sie können ein Skript lokal ausführen, (das heißt, auf dem Buildserver) oder Remote auf einem Ziel-Webserver oder Datenbankserver wie.</span><span class="sxs-lookup"><span data-stu-id="81aeb-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="81aeb-109">Es gibt viele Gründe, warum Sie möglicherweise einen Windows PowerShell-Skript nach der Bereitstellung ausführen möchten.</span><span class="sxs-lookup"><span data-stu-id="81aeb-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="81aeb-110">Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:</span><span class="sxs-lookup"><span data-stu-id="81aeb-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="81aeb-111">Fügen Sie eine benutzerdefinierte Ereignisquelle in der Registrierung hinzu.</span><span class="sxs-lookup"><span data-stu-id="81aeb-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="81aeb-112">Generieren Sie ein Dateisystemverzeichnis für Uploads.</span><span class="sxs-lookup"><span data-stu-id="81aeb-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="81aeb-113">Bereinigen Sie Buildverzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="81aeb-113">Clean up build directories.</span></span>
> - <span data-ttu-id="81aeb-114">Geschrieben Sie Einträge in eine benutzerdefinierte Protokolldatei.</span><span class="sxs-lookup"><span data-stu-id="81aeb-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="81aeb-115">Einladen von Benutzern zu einer neu bereitgestellten Webanwendung-e-Mails zu senden.</span><span class="sxs-lookup"><span data-stu-id="81aeb-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="81aeb-116">Erstellen Sie Benutzerkonten mit den entsprechenden Berechtigungen an.</span><span class="sxs-lookup"><span data-stu-id="81aeb-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="81aeb-117">Konfigurieren der Replikation zwischen SQL Server-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="81aeb-118">In diesem Thema zeigt Ihnen, wie Sie Windows PowerShell-Skripts in ein benutzerdefiniertes Ziel in einer Projektdatei Microsoft Build Engine (MSBuild) sowohl lokal als auch remote ausführen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="81aeb-119">In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="81aeb-120">Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten.</span><span class="sxs-lookup"><span data-stu-id="81aeb-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="81aeb-121">Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="81aeb-122">Übersicht über den Task</span><span class="sxs-lookup"><span data-stu-id="81aeb-122">Task Overview</span></span>

<span data-ttu-id="81aeb-123">Um ein Windows PowerShell-Skript als Teil einer automatisierten oder Schritt für Schritt Bereitstellungsprozess auszuführen, müssen Sie diese allgemeinen Aufgaben ausführen:</span><span class="sxs-lookup"><span data-stu-id="81aeb-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="81aeb-124">Fügen Sie das Windows PowerShell-Skript zu Ihrer Projektmappe und quellcodeverwaltung.</span><span class="sxs-lookup"><span data-stu-id="81aeb-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="81aeb-125">Erstellen Sie einen Befehl, der Windows PowerShell-Skript aufruft.</span><span class="sxs-lookup"><span data-stu-id="81aeb-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="81aeb-126">Versehen Sie reservierten XML-Zeichen in Ihrem Befehl mit Escapezeichen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="81aeb-127">Erstellen eines Ziels in der benutzerdefinierten MSBuild-Projektdatei, und Verwenden der **Exec** Aufgabe, die den Befehl ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="81aeb-128">In diesem Thema zeigt Sie, wie Sie diese Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="81aeb-129">Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit MSBuild-Ziele und Eigenschaften vertraut sind und Sie erfahren, wie eine benutzerdefinierte MSBuild-Projektdatei zu verwenden, um einen Build & Deployment-Prozess zu steuern.</span><span class="sxs-lookup"><span data-stu-id="81aeb-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="81aeb-130">Weitere Informationen finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="81aeb-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="81aeb-131">Erstellen und Hinzufügen von Windows PowerShell-Skripts</span><span class="sxs-lookup"><span data-stu-id="81aeb-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="81aeb-132">Die Aufgaben in diesem Thema verwenden Sie ein Windows PowerShell-Beispielskript, mit dem Namen **LogDeploy.ps1** zum veranschaulichen des zum Ausführen von Skripts aus MSBuild.</span><span class="sxs-lookup"><span data-stu-id="81aeb-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="81aeb-133">Die **LogDeploy.ps1** Skript enthält eine einfache Funktion, die einen Eintrag für einzeilige in eine Protokolldatei schreibt:</span><span class="sxs-lookup"><span data-stu-id="81aeb-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="81aeb-134">Die **LogDeploy.ps1** Skript akzeptiert zwei Parameter.</span><span class="sxs-lookup"><span data-stu-id="81aeb-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="81aeb-135">Der erste Parameter stellt den vollständigen Pfad, in die Protokolldatei zu der Sie einen Eintrag hinzufügen möchten, und der zweite Parameter stellt das Bereitstellungsziel aus, dem in der Protokolldatei aufgezeichnet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="81aeb-136">Wenn Sie das Skript ausführen, fügt es eine Zeile in die Protokolldatei im folgenden Format hinzu:</span><span class="sxs-lookup"><span data-stu-id="81aeb-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="81aeb-137">Zu den **LogDeploy.ps1** Skript MSBuild zur Verfügung, müssen Sie:</span><span class="sxs-lookup"><span data-stu-id="81aeb-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="81aeb-138">Das Skript zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-138">Add the script to source control.</span></span>
- <span data-ttu-id="81aeb-139">Fügen Sie das Skript für Ihre Lösung in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="81aeb-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="81aeb-140">Sie müssen nicht stellen Sie das Skript mit Ihrer Lösungsinhalt, unabhängig davon, ob das Skript auf dem Buildserver oder auf einem Remotecomputer ausgeführt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="81aeb-141">Eine Möglichkeit ist das Skript einen Projektmappenordner hinzu.</span><span class="sxs-lookup"><span data-stu-id="81aeb-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="81aeb-142">Da Sie das Windows PowerShell-Skript im Rahmen des Bereitstellungsprozesses verwenden möchten, ist es im Contact Manager-Beispiel sinnvoll, fügen das Skript in den Projektmappenordner veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="81aeb-143">Der Inhalt der Projektmappenordner werden zum Erstellen von Servern als Quellcode kopiert.</span><span class="sxs-lookup"><span data-stu-id="81aeb-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="81aeb-144">Allerdings bilden sie kein Teil des Projektausgabe.</span><span class="sxs-lookup"><span data-stu-id="81aeb-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="81aeb-145">Ein Windows PowerShell-Skript ausführen, auf dem Buildserver</span><span class="sxs-lookup"><span data-stu-id="81aeb-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="81aeb-146">In einigen Szenarien möchten möglicherweise Windows PowerShell-Skripts auf dem Computer ausgeführt wird, in dem Ihre Projekte erstellt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="81aeb-147">Beispielsweise können Sie ein Windows PowerShell-Skript verwenden, die Buildordner bereinigen oder Einträge in eine benutzerdefinierte Protokolldatei geschrieben.</span><span class="sxs-lookup"><span data-stu-id="81aeb-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="81aeb-148">In Bezug auf Syntax ist ein Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausführen dasselbe wie das Ausführen eines Windows PowerShell-Skripts über eine reguläre Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="81aeb-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="81aeb-149">Wenn Sie dies an einer Eingabeaufforderung ausführen, müssen Sie die Windows PowerShell, die ausführbare Datei aufrufen und Verwenden der **– Befehl** Parameter, um Ihre Anweisungen bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="81aeb-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="81aeb-150">Wie zuvor müssen Sie einige zusätzliche Schalter angeben und reservierten XML-Zeichen als Escapezeichen, beim Ausführen des Befehls aus MSBuild:</span><span class="sxs-lookup"><span data-stu-id="81aeb-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="81aeb-151">Zum Schluss wie zuvor können Sie die Exec Aufgabe in einem benutzerdefinierten MSBuild-Ziel zum Ausführen des Befehls:</span><span class="sxs-lookup"><span data-stu-id="81aeb-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="81aeb-152">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="81aeb-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="81aeb-153">Wenn Sie dieses Ziel im Rahmen des Buildprozesses ausführen, führt Windows PowerShell das Skript auf dem Computer, die Sie, in angegeben der – Computername Argument.</span><span class="sxs-lookup"><span data-stu-id="81aeb-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="81aeb-154">In diesem Thema beschrieben, wie Sie ein Windows PowerShell-Skript über eine MSBuild-Projektdatei ausführen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="81aeb-155">Sie können diesen Ansatz verwenden, um ein Windows PowerShell-Skript, entweder lokal oder auf einem Remotecomputer befindet, als Teil eines automatisierten oder Schritt für Schritt Build & Deployment-Prozesses ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="81aeb-156">Anleitungen zum Signieren von Windows PowerShell-Skripts, und Verwalten von Ausführungsrichtlinien finden Sie unter **Ausführen von Windows PowerShell-Skripts**.</span><span class="sxs-lookup"><span data-stu-id="81aeb-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="81aeb-157">Anleitungen zum Ausführen von Windows PowerShell-Befehle von einem Remotecomputer finden Sie unter **Ausführen von Remotebefehlen**.</span><span class="sxs-lookup"><span data-stu-id="81aeb-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="81aeb-158">Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter Grundlegendes zur Projektdatei und Verständnis des Prozesses erstellen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="81aeb-159">Sie können über diese Argumentwerte auswählen:</span><span class="sxs-lookup"><span data-stu-id="81aeb-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="81aeb-160">Der Wert **uneingeschränkt** ermöglicht Windows PowerShell zum Ausführen des Skripts, unabhängig davon, ob das Skript signiert ist.</span><span class="sxs-lookup"><span data-stu-id="81aeb-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="81aeb-161">Der Wert **"RemoteSigned"** ermöglicht Windows PowerShell zum Ausführen von nicht signierten Skripts, die auf dem lokalen Computer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="81aeb-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="81aeb-162">Allerdings müssen Skripts, die an anderer Stelle erstellt wurden, signiert werden.</span><span class="sxs-lookup"><span data-stu-id="81aeb-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="81aeb-163">(In der Praxis können Sie sehr wahrscheinlich nicht zu einem Windows PowerShell-Skript lokal auf einem Buildserver erstellt haben).</span><span class="sxs-lookup"><span data-stu-id="81aeb-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="81aeb-164">Der Wert **"AllSigned"** ermöglicht Windows PowerShell, um nur digital signierte Skripts auszuführen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="81aeb-165">Ist die standardausführungsrichtlinie **Restricted**, wodurch verhindert wird, dass Windows PowerShell alle Skriptdateien ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="81aeb-166">Abschließend müssen Sie reservierten XML-Zeichen mit Escapezeichen versehen, die auftreten, in Ihrem Windows PowerShell-Befehl:</span><span class="sxs-lookup"><span data-stu-id="81aeb-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="81aeb-167">Ersetzen Sie einzelne Anführungszeichen mit  **&amp;Apos;**</span><span class="sxs-lookup"><span data-stu-id="81aeb-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="81aeb-168">Ersetzen von doppelten Anführungszeichen mit  **&amp;Quot;**</span><span class="sxs-lookup"><span data-stu-id="81aeb-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="81aeb-169">Ersetzen Sie kaufmännische und-Zeichen mit  **&amp;Amp;**</span><span class="sxs-lookup"><span data-stu-id="81aeb-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="81aeb-170">Wenn Sie diese Änderungen vornehmen, wird der Befehl sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="81aeb-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="81aeb-171">Innerhalb der benutzerdefinierten MSBuild-Projektdatei, können Sie ein neues Ziel erstellen und Verwenden der **Exec** Aufgabe zum Ausführen dieses Befehls:</span><span class="sxs-lookup"><span data-stu-id="81aeb-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="81aeb-172">Beachten Sie, dass in diesem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="81aeb-172">In this example, note that:</span></span>

- <span data-ttu-id="81aeb-173">Alle Variablen werden wie die Parameterwerte und den Speicherort der ausführbaren Datei Windows PowerShell werden als MSBuild-Eigenschaften deklariert.</span><span class="sxs-lookup"><span data-stu-id="81aeb-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="81aeb-174">Bedingungen sind enthalten, damit Benutzer diese Werte über die Befehlszeile außer Kraft setzen können.</span><span class="sxs-lookup"><span data-stu-id="81aeb-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="81aeb-175">Die **MSDeployComputerName** Eigenschaft wird an anderer Stelle in der Projektdatei deklariert.</span><span class="sxs-lookup"><span data-stu-id="81aeb-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="81aeb-176">Wenn Sie dieses Ziel im Rahmen des Buildprozesses ausführen, wird Windows PowerShell den Befehl ausführen und einen Protokolleintrag, der die angegebene Datei zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="81aeb-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="81aeb-177">Ein Windows PowerShell-Skript ausführen auf einem Remotecomputer</span><span class="sxs-lookup"><span data-stu-id="81aeb-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="81aeb-178">Windows PowerShell ist in der Lage der Ausführung von Skripts auf Remotecomputern über [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="81aeb-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="81aeb-179">Zu diesem Zweck müssen Sie verwenden die [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) Cmdlet.</span><span class="sxs-lookup"><span data-stu-id="81aeb-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="81aeb-180">Dadurch können Sie das Skript für eine oder mehrere Remotecomputer ausführen, ohne das Skript auf den Remotecomputern zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="81aeb-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="81aeb-181">Ergebnisse werden auf dem lokalen Computer zurückgegeben, in denen Sie das Skript ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="81aeb-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="81aeb-182">Vor der Verwendung der **Invoke-Command** -Cmdlet zum Ausführen von Windows PowerShell-Skripts auf einem Remotecomputer befindet, müssen Sie einen WinRM-Listener zum Akzeptieren von remote-Nachrichten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="81aeb-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="81aeb-183">Sie erreichen dies durch Ausführen des Befehls **Winrm Quickconfig** auf dem Remotecomputer.</span><span class="sxs-lookup"><span data-stu-id="81aeb-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="81aeb-184">Weitere Informationen finden Sie unter [Installation und Konfiguration für Windows-Remoteverwaltung](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="81aeb-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="81aeb-185">Im Windows PowerShell-Fenster, verwenden Sie diese Syntax zum Ausführen der **LogDeploy.ps1** Skript auf einem Remotecomputer:</span><span class="sxs-lookup"><span data-stu-id="81aeb-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="81aeb-186">Es gibt verschiedene andere Möglichkeiten der Verwendung von **Invoke-Command** zum Ausführen eines Skripts-Datei, aber dieser Ansatz ist vergleichsweise einfach, wenn Sie Parameterwerte bereitstellen und verwalten Sie Pfade mit Leerzeichen müssen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="81aeb-187">Wenn Sie dies an einer Eingabeaufforderung ausführen, müssen Sie die Windows PowerShell, die ausführbare Datei aufrufen und Verwenden der **– Befehl** Parameter, um Ihre Anweisungen bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="81aeb-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="81aeb-188">Wie zuvor müssen Sie einige zusätzliche Schalter angeben und reservierten XML-Zeichen als Escapezeichen, beim Ausführen des Befehls aus MSBuild:</span><span class="sxs-lookup"><span data-stu-id="81aeb-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="81aeb-189">Zum Schluss wie zuvor können Sie die **Exec** Aufgabe in einem benutzerdefinierten MSBuild-Ziel zum Ausführen des Befehls:</span><span class="sxs-lookup"><span data-stu-id="81aeb-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="81aeb-190">Wenn Sie dieses Ziel im Rahmen des Buildprozesses ausführen, führt Windows PowerShell das Skript auf dem Computer, die Sie, in angegeben der **– Computername** Argument.</span><span class="sxs-lookup"><span data-stu-id="81aeb-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="81aeb-191">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="81aeb-191">Conclusion</span></span>

<span data-ttu-id="81aeb-192">In diesem Thema beschrieben, wie Sie ein Windows PowerShell-Skript über eine MSBuild-Projektdatei ausführen.</span><span class="sxs-lookup"><span data-stu-id="81aeb-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="81aeb-193">Sie können diesen Ansatz verwenden, um ein Windows PowerShell-Skript, entweder lokal oder auf einem Remotecomputer befindet, als Teil eines automatisierten oder Schritt für Schritt Build & Deployment-Prozesses ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="81aeb-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="81aeb-194">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="81aeb-194">Further Reading</span></span>

<span data-ttu-id="81aeb-195">Anleitungen zum Signieren von Windows PowerShell-Skripts, und Verwalten von Ausführungsrichtlinien finden Sie unter [Ausführen von Windows PowerShell-Skripts](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="81aeb-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="81aeb-196">Anleitungen zum Ausführen von Windows PowerShell-Befehle von einem Remotecomputer finden Sie unter [Ausführen von Remotebefehlen](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="81aeb-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="81aeb-197">Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="81aeb-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="81aeb-198">[Zurück](taking-web-applications-offline-with-web-deploy.md)
> [Weiter](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="81aeb-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
