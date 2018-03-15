---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "Erstellen und Ausführen einer Bereitstellung Befehlsdatei | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, wie eine Befehlsdatei erstellt, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild)-Projektdateien in einem einzelnen Schritt erneut ausführen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="3b7ca-103">Erstellen und Ausführen einer Befehlsdatei Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="3b7ca-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="3b7ca-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="3b7ca-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="3b7ca-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="3b7ca-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="3b7ca-106">In diesem Thema wird beschrieben, wie eine Befehlsdatei erstellt, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild)-Projektdateien einstufiger, wiederholbare ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="3b7ca-107">Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Vorgesetzten Kontakts](the-contact-manager-solution.md) Lösung & #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="3b7ca-108">Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [Verständnis des Build-Prozesses](understanding-the-build-process.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="3b7ca-109">Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="3b7ca-110">Übersicht über das</span><span class="sxs-lookup"><span data-stu-id="3b7ca-110">Process Overview</span></span>

<span data-ttu-id="3b7ca-111">In diesem Thema erfahren Sie, wie zum Erstellen und Ausführen einer Befehlsdatei, die diese Projektdateien verwendet wird, um eine wiederholbare Bereitstellungen in Ihrer zielumgebung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="3b7ca-112">Im Wesentlichen die Befehlsdatei einfach muss einen MSBuild-Befehl enthalten, die:</span><span class="sxs-lookup"><span data-stu-id="3b7ca-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="3b7ca-113">Weist MSBuild an, führen Sie die Umgebung Unabhängigkeit *Publish.proj* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="3b7ca-114">Teilt die *Publish.proj* Datei, die sich die Datei enthält den umgebungsspezifische projekteinstellungen und wo sie suchen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="3b7ca-115">Erstellen Sie einen MSBuild-Befehl</span><span class="sxs-lookup"><span data-stu-id="3b7ca-115">Create an MSBuild Command</span></span>

<span data-ttu-id="3b7ca-116">Wie in beschrieben [Verständnis des Build-Prozesses](understanding-the-build-process.md), die Projektdatei umgebungsspezifische & #x 2014; z. B. *Env Dev.proj*& #x 2014; dient für den Import in die Unabhängigkeit von der Umgebung *Publish.proj* Datei während des Buildvorgangs.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="3b7ca-117">In Kombination bieten zwei dieser Dateien einen vollständigen Satz von Anweisungen, die MSBuild Bereitstellen von Informationen zum Erstellen und Bereitstellen der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="3b7ca-118">Die *Publish.proj* Datei verwendet eine **importieren** Element, um die Umgebung spezifischen-Projektdatei importieren.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="3b7ca-119">Bei Verwendung von MSBuild.exe zum Erstellen und Bereitstellen der Projektmappe Contact Manager müssen Sie daher auf:</span><span class="sxs-lookup"><span data-stu-id="3b7ca-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="3b7ca-120">MSBuild.exe ausführen, auf die *Publish.proj* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="3b7ca-121">Geben Sie den Speicherort der Projektdatei umgebungsspezifische durch Angabe eines Befehlszeilenparameters namens **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="3b7ca-122">Zu diesem Zweck sieht die MSBuild-Befehl folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="3b7ca-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="3b7ca-123">Hier ist es ein einfacher Schritt zu einer Bereitstellung wiederholbare, Schritt für Schritt zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="3b7ca-124">Alles, was Sie tun müssen MSBuild-Befehl eine CMD-Datei hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="3b7ca-125">In der Projektmappe Contact Manager Veröffentlichungsordner enthält eine Datei namens *veröffentlichen Dev.cmd* genau dies ausführt.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="3b7ca-126">Die **/fl** -Schalter wird MSBuild zum Erstellen einer Protokolldatei mit dem Namen *msbuild.log* in das Arbeitsverzeichnis an, in dem MSBuild.exe aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="3b7ca-127">Um bereitzustellen, oder stellen Sie die Kontakt-Manager-Lösung, müssen Sie lediglich führen die *veröffentlichen Dev.cmd* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="3b7ca-128">Wenn Sie die Datei ausführen, sehen MSBuild:</span><span class="sxs-lookup"><span data-stu-id="3b7ca-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="3b7ca-129">Alle Projekte in der Projektmappe zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="3b7ca-130">Bereitstellbare Webpaketen für die Anwendung Webprojekte zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="3b7ca-131">Generieren Sie DBSCHEMA und DeployManifest-Dateien für die Datenbankprojekte.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="3b7ca-132">Bereitstellen Sie die Webpakete an den Webserver.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="3b7ca-133">Bereitstellen Sie die Datenbank auf dem Datenbankserver.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="3b7ca-134">Führen Sie die Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="3b7ca-134">Run the Deployment</span></span>

<span data-ttu-id="3b7ca-135">Wenn Sie eine Befehlsdatei für Ihre zielumgebung erstellt haben, sollten Sie einfach mit der Datei die gesamte Bereitstellung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="3b7ca-136">**Die Kontakt-Manager-Lösung an Ihre testumgebung bereitstellen**</span><span class="sxs-lookup"><span data-stu-id="3b7ca-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="3b7ca-137">Auf der Arbeitsstation Developer öffnen Sie Windows Explorer, und navigieren Sie zum Speicherort von der *veröffentlichen Dev.cmd* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="3b7ca-138">Doppelklicken Sie auf die Datei, um es auszuführen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="3b7ca-139">Wenn ein **Datei öffnen-Sicherheitswarnung** Dialogfeld angezeigt wird, klicken Sie auf **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="3b7ca-140">Wenn Ihre Konfigurationseinstellungen und Testserver eingerichtet sind, ordnungsgemäß im Eingabeaufforderungsfenster zeigt eine **der Buildvorgang war erfolgreich** Meldung, wenn MSBuild die Verarbeitung der Projektdateien abgeschlossen hat.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="3b7ca-141">Wenn dies der erste Mal ist, haben Sie die Lösung für diese Umgebung bereitgestellt, müssen Sie die Test-Web--Servercomputer zum Hinzufügen der **Db\_Datawriter** und **Db\_Datareader**Rollen auf die **ContactManager** Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="3b7ca-142">Hierin wird beschrieben, [Konfigurieren eines Datenbankservers für Webveröffentlichung bereitstellen](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3b7ca-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="3b7ca-143">Sie müssen nur dieser Berechtigungen zuweisen, wenn Sie die Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="3b7ca-144">Standardmäßig die Datenbank auf jeder Bereitstellung & #x 2014, während des Erstellungsprozesses wird nicht neu erstellt; stattdessen wird die vorhandene Datenbank auf das neueste Schema vergleichen und nur die erforderlichen Änderungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="3b7ca-145">Daher müssen Sie nur diesen Datenbankrollen erstmalig ordnen Sie die Projektmappe bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="3b7ca-146">Öffnen Sie Internet Explorer, und navigieren Sie zu der URL der Kontakt-Manager-Anwendung (z. B. `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="3b7ca-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="3b7ca-147">Stellen Sie sicher, dass die Anwendung erwartungsgemäß funktioniert, und Sie die Kontakte hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="3b7ca-148">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="3b7ca-148">Conclusion</span></span>

<span data-ttu-id="3b7ca-149">Erstellen eine Befehlsdatei mit der MSBuild-Anweisungen, bietet Ihnen eine schnelle und einfache Möglichkeit der Erstellung und Bereitstellung einer Lösung mit mehreren Projekten in einer bestimmten zielumgebung.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="3b7ca-150">Wenn Sie Ihre Lösung in mehreren zielumgebungen bereitstellen müssen, können Sie mehrere Befehlsdateien erstellen.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="3b7ca-151">In jeder Datei die MSBuild-Befehl wird die gleiche universal-Projektdatei erstellt, aber es wird eine andere umgebungsspezifische Projektdatei angeben.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="3b7ca-152">Beispielsweise könnte eine Befehlsdatei ein Entwickler veröffentlichen oder testumgebung dieses MSBuild-Befehl enthalten:</span><span class="sxs-lookup"><span data-stu-id="3b7ca-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="3b7ca-153">Eine Befehlsdatei zum Veröffentlichen in einer Stagingumgebung könnte diese MSBuild-Befehl enthalten:</span><span class="sxs-lookup"><span data-stu-id="3b7ca-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="3b7ca-154">Anleitungen zum Anpassen der umgebungsspezifische-Projektdateien für eigene Server-Umgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="3b7ca-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="3b7ca-155">Sie können den Buildprozess für jede Umgebung auch anpassen, indem Überschreiben von Eigenschaften oder das Festlegen von verschiedenen anderen Schaltern in MSBuild-Befehl.</span><span class="sxs-lookup"><span data-stu-id="3b7ca-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="3b7ca-156">Weitere Informationen finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b7ca-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3b7ca-157">[Zurück](deploying-database-projects.md)
[Weiter](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="3b7ca-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
