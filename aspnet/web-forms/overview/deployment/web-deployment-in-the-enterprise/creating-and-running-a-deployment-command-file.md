---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Erstellen und Ausführen einer Bereitstellungstyps Befehlsdatei | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie eine Befehlsdatei erstellen, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild) Projektdateien als einen Schritt für Schritt, erneut ausführen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395372"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="b2e0e-103">Erstellen und Ausführen einer Befehlsdatei für die Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="b2e0e-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="b2e0e-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b2e0e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b2e0e-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="b2e0e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b2e0e-106">In diesem Thema wird beschrieben, wie eine Befehlsdatei erstellen, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild) Projektdateien als Schritt für Schritt, wiederholbaren Prozess ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="b2e0e-107">In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager](the-contact-manager-solution.md) Lösung&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="b2e0e-108">Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Verständnis des Prozesses erstellen](understanding-the-build-process.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="b2e0e-109">Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="b2e0e-110">Prozessübersicht</span><span class="sxs-lookup"><span data-stu-id="b2e0e-110">Process Overview</span></span>

<span data-ttu-id="b2e0e-111">In diesem Thema erfahren Sie, wie zum Erstellen und Ausführen einer Befehlsdatei, die diese Projektdateien verwendet wird, um eine wiederholbare Bereitstellung für Ihre zielumgebung durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="b2e0e-112">Im Wesentlichen die Befehlsdatei muss einfach nur einen MSBuild-Befehl enthält, die:</span><span class="sxs-lookup"><span data-stu-id="b2e0e-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="b2e0e-113">Informiert MSBuild, führen Sie die Umgebung agnostisch *Publish.proj* Datei.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="b2e0e-114">Teilt die *Publish.proj* Datei, welche Datei enthält die umgebungsspezifischen projekteinstellungen und wo Sie sich befindet.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="b2e0e-115">Erstellen Sie ein MSBuild-Befehl</span><span class="sxs-lookup"><span data-stu-id="b2e0e-115">Create an MSBuild Command</span></span>

<span data-ttu-id="b2e0e-116">Siehe [Verständnis des Prozesses erstellen](understanding-the-build-process.md), umgebungsspezifische Projektdatei&#x2014;z. B. *Env-Dev.proj*&#x2014;dient in der Umgebung agnostisch importiert werden sollen *Publish.proj* Datei zum Zeitpunkt der Erstellung.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="b2e0e-117">Zusammen bieten diese beiden Dateien einen vollständigen Satz von Anweisungen, die MSBuild anweisen, wie zum Erstellen und Bereitstellen Ihrer Lösung.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="b2e0e-118">Die *Publish.proj* Datei verwendet ein **importieren** Element, um die umgebungsspezifischen-Projektdatei importieren.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="b2e0e-119">Wenn Sie MSBuild.exe zum Erstellen und Bereitstellen von Contact Manager-Lösung verwenden, müssen Sie:</span><span class="sxs-lookup"><span data-stu-id="b2e0e-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="b2e0e-120">Führen Sie MSBuild.exe auf die *Publish.proj* Datei.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="b2e0e-121">Geben Sie den Speicherort der Projektdatei umgebungsspezifische durch Angabe der Parameter für die Befehlszeilen mit dem Namen **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="b2e0e-122">Zu diesem Zweck sieht der MSBuild-Befehl folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="b2e0e-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="b2e0e-123">Hier ist es eine einfache Schritt, in einer Bereitstellung für wiederholbare, Schritt für Schritt zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="b2e0e-124">Müssen Sie lediglich Ihre MSBuild-Befehl eine CMD-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="b2e0e-125">In der Projektmappe Contact Manager-Ordner "Publish" umfasst eine Datei namens *veröffentlichen-Dev.cmd* , die dies ist genau.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="b2e0e-126">Die **/fl** -Schalter weist MSBuild zum Erstellen einer Protokolldatei mit dem Namen *msbuild.log* in das Arbeitsverzeichnis, in denen MSBuild.exe aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="b2e0e-127">Zum Bereitstellen oder die Kontakt-Manager-Lösung erneut bereitzustellen, müssen Sie lediglich ausgeführt wird die *veröffentlichen-Dev.cmd* Datei.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="b2e0e-128">Wenn Sie die Datei ausführen, wird MSBuild:</span><span class="sxs-lookup"><span data-stu-id="b2e0e-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="b2e0e-129">Alle Projekte in der Projektmappe zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="b2e0e-130">Generieren Sie bereitstellbare Webpakete für Webanwendungsprojekte.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="b2e0e-131">Generieren Sie DBSCHEMA und DeployManifest-Dateien für die Datenbankprojekte.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="b2e0e-132">Stellen Sie die Webpakete an den Webserver bereit.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="b2e0e-133">Bereitstellen der Datenbank mit dem Datenbankserver her.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="b2e0e-134">Führen Sie die Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="b2e0e-134">Run the Deployment</span></span>

<span data-ttu-id="b2e0e-135">Wenn Sie eine Befehlsdatei für Ihre zielumgebung erstellt haben, sollten Sie die gesamte Bereitstellung abgeschlossen, indem Sie einfach die Datei ausführen können.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="b2e0e-136">**Zum Bereitstellen der Contact Manager-Lösung an Ihre testumgebung**</span><span class="sxs-lookup"><span data-stu-id="b2e0e-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="b2e0e-137">Klicken Sie auf der Entwicklerarbeitsstation Windows Explorer öffnen, und navigieren Sie dann auf den Speicherort der der *veröffentlichen-Dev.cmd* Datei.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="b2e0e-138">Doppelklicken Sie auf die Datei aus, um es auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="b2e0e-139">Wenn ein **Datei öffnen-Sicherheitswarnung** klicken Sie im angezeigten Dialogfeld **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="b2e0e-140">Wenn Ihre Konfigurationseinstellungen und die Testserver eingerichtet sind, ordnungsgemäß im Eingabeaufforderungsfenster wird angezeigt ein **Buildvorgang war erfolgreich.** Meldung, wenn MSBuild die Projektdateien Verarbeitung abgeschlossen hat.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="b2e0e-141">Ist dies beim ersten Sie die Lösung für diese Umgebung bereitgestellt haben, müssen Sie das Computerkonto Test Web Server, Hinzufügen der **Db\_Datawriter** und **Db\_Datareader**Rollen auf die **ContactManager** Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="b2e0e-142">Dieses Verfahren wird beschrieben, [Konfigurieren eines Datenbankservers für die Webveröffentlichung bereitstellen](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b2e0e-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="b2e0e-143">Sie müssen nur diese Berechtigungen zuweisen, wenn Sie die Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="b2e0e-144">In der Standardeinstellung des Buildprozesses werden nicht neu erstellt die Datenbank bei jeder Bereitstellung&#x2014;stattdessen die vorhandene Datenbank auf dem neuesten Schema verglichen und nur die erforderlichen Änderungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="b2e0e-145">Daher müssen Sie nur diesen Datenbankrollen beim ersten ordnen Sie die Lösung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="b2e0e-146">Öffnen Sie Internet Explorer, und navigieren Sie zu der URL der Contact Manager-Anwendung (z. B. `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="b2e0e-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="b2e0e-147">Stellen Sie sicher, dass die Anwendung ordnungsgemäß funktioniert, und Sie können Kontakte hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="b2e0e-148">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="b2e0e-148">Conclusion</span></span>

<span data-ttu-id="b2e0e-149">Erstellen eine Befehlsdatei, die die MSBuild-Anweisungen enthält, bietet Ihnen eine schnelle und einfache Möglichkeit, erstellen und Bereitstellen von Projektmappen mit mehreren Projekten für ein bestimmtes Ziel-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="b2e0e-150">Wenn Sie wiederholte Bereitstellen Ihrer Lösung in mehreren zielumgebungen müssen, können Sie mehrere Befehlsdateien erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="b2e0e-151">Klicken Sie in jeder Befehlsdatei MSBuild-Befehl wird die gleiche universal-Projekt-Datei erstellen, aber es wird eine andere Umgebung spezifischen Projektdatei angeben.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="b2e0e-152">Beispielsweise kann eine Befehlsdatei für Entwickler veröffentlichen oder testumgebung dieses MSBuild-Befehl enthalten:</span><span class="sxs-lookup"><span data-stu-id="b2e0e-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="b2e0e-153">Eine Befehlsdatei in einer Stagingumgebung zu veröffentlichen, könnte dieses MSBuild-Befehl enthalten:</span><span class="sxs-lookup"><span data-stu-id="b2e0e-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="b2e0e-154">Anleitungen zum Anpassen der umgebungsspezifischen Projektdateien für Ihre eigenen serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="b2e0e-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="b2e0e-155">Sie können den Buildprozess für jede Umgebung auch anpassen, durch Überschreiben von Eigenschaften oder Festlegen von verschiedenen anderen Schaltern in Ihrem MSBuild-Befehl.</span><span class="sxs-lookup"><span data-stu-id="b2e0e-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="b2e0e-156">Weitere Informationen finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2e0e-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2e0e-157">[Zurück](deploying-database-projects.md)
> [Weiter](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="b2e0e-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
