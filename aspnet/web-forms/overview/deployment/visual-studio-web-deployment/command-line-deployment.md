---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Befehlszeilenbereitstellung | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: f079beabccd253ff13c19d3192181ddbdf00b3bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830966"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="2f7a3-103">ASP.NET-webbereitstellung mithilfe von Visual Studio: Befehlszeilenbereitstellung</span><span class="sxs-lookup"><span data-stu-id="2f7a3-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="2f7a3-104">durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2f7a3-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2f7a3-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="2f7a3-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="2f7a3-106">Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="2f7a3-107">Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2f7a3-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="2f7a3-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2f7a3-108">Overview</span></span>

<span data-ttu-id="2f7a3-109">In diesem Tutorial erfahren Sie, wie Sie das Visual Studio Web aufrufen Pipeline über die Befehlszeile zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="2f7a3-110">Dies ist nützlich für Szenarien, in denen Sie möchten [Automatisierung des Bereitstellungsprozesses](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anstatt dies manuell in Visual Studio, in der Regel mithilfe einer [source Code Versionskontrollsystem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="2f7a3-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="2f7a3-111">Nehmen Sie eine Änderung bereitstellen</span><span class="sxs-lookup"><span data-stu-id="2f7a3-111">Make a change to deploy</span></span>

<span data-ttu-id="2f7a3-112">Die Seite "Info" wird derzeit den Code der Vorlage angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-112">Currently the About page displays the template code.</span></span>

![Zu den Seiten mit Vorlagencode](command-line-deployment/_static/image1.png)

<span data-ttu-id="2f7a3-114">Sie müssen, die durch Code ersetzen, die eine Zusammenfassung der Student Registrierung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="2f7a3-115">Öffnen der *About.aspx* Seite, löschen Sie alle Markups innerhalb der `MainContent` `Content` -Element und fügt das folgende Markup an seiner Stelle:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="2f7a3-116">Führen Sie das Projekt, und wählen Sie die **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-116">Run the project and select the **About** page.</span></span>

![Seite „Info“](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="2f7a3-118">Test über die Befehlszeile bereitstellen</span><span class="sxs-lookup"><span data-stu-id="2f7a3-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="2f7a3-119">Sie wird nicht eine andere Datenbank ändern, also deaktivieren DbDacFx-datenbankbereitstellung für die Datenbank Aspnet-ContosoUniversity bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="2f7a3-120">Öffnen der **Webveröffentlichung** Assistenten, und die in jeder der drei Veröffentlichungsprofile, deaktivieren die **Update Database** auf das Kontrollkästchen der **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="2f7a3-121">Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="2f7a3-122">Mit der rechten Maustaste in des Symbols für **Developer-Eingabeaufforderung für VS2012** , und klicken Sie auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="2f7a3-123">Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad zu Ihrer Lösungsdatei an:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="2f7a3-124">MSBuild wird die Projektmappe erstellt und in der testumgebung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Befehlszeilenausgabe](command-line-deployment/_static/image3.png)

<span data-ttu-id="2f7a3-126">Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="2f7a3-127">Wenn Sie alle Schüler und Studenten im Test erstellt haben, sehen Sie eine leere Seite unter dem **Statistiken der Studentendaten Text** Überschrift.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="2f7a3-128">Wechseln Sie zu der **Schüler/Studenten** auf **hinzufügen "Student"**, und fügen Sie einige Schüler/Studenten hinzu, und wieder die **zu** Seite, um die Statistiken der Studentendaten finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Zu den Seiten im Test-Umgebung](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="2f7a3-130">Key-Befehlszeilenoptionen</span><span class="sxs-lookup"><span data-stu-id="2f7a3-130">Key command line options</span></span>

<span data-ttu-id="2f7a3-131">Der Befehl, der eingegebene Pfad der Projektmappe und zwei Eigenschaften an MSBuild übergeben werden:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="2f7a3-132">Bereitstellen der Lösung und der Bereitstellung von einzelner Projekten</span><span class="sxs-lookup"><span data-stu-id="2f7a3-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="2f7a3-133">Angeben der Projektmappendatei bewirkt, dass alle Projekte in der Projektmappe erstellt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="2f7a3-134">Wenn Sie mehrere Webprojekte in der Projektmappe verfügen, gilt die folgende MSBuild-Verhalten auf:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="2f7a3-135">Die Eigenschaften, die Sie in der Befehlszeile angeben, werden jedem Projekt übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="2f7a3-136">Daher muss jedes Webprojekt ein Veröffentlichungsprofil mit dem Namen verfügen, die Sie angeben.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="2f7a3-137">Bei Angabe von `/p:PublishProfile=Test`, jedes Webprojekt benötigen ein Veröffentlichungsprofil mit dem Namen *Test*.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="2f7a3-138">Wenn ein anderes noch nicht erstellt werden, könnten Sie ein Projekt erfolgreich veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="2f7a3-139">Weitere Informationen finden Sie in der Stack Overflow-Thread [MSBuild ein Fehler auftritt, mit zwei Paketen](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="2f7a3-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="2f7a3-140">Wenn Sie ein einzelnes Projekt anstelle einer Projektmappe angeben, müssen Sie einen Parameter hinzuzufügen, der angibt, die Visual Studio-Version.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="2f7a3-141">Wenn Sie Visual Studio 2012 verwenden wäre wie im folgenden Beispiel der Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="2f7a3-142">Die Versionsnummer für Visual Studio 2010 ist 10,0.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="2f7a3-143">Weitere Informationen finden Sie unter [Visual Studio-Projekt zur Anwendungskompatibilität und VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi-Blog.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="2f7a3-144">Das Veröffentlichungsprofil angeben</span><span class="sxs-lookup"><span data-stu-id="2f7a3-144">Specifying the publish profile</span></span>

<span data-ttu-id="2f7a3-145">Sie können das Veröffentlichungsprofil angeben, nach Namen oder den vollständigen Pfad zu der *pubxml* Datei, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="2f7a3-146">Webveröffentlichung mit Methoden, die für die Befehlszeilen-Veröffentlichung unterstützt</span><span class="sxs-lookup"><span data-stu-id="2f7a3-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="2f7a3-147">Veröffentlichen von drei Methoden werden für die Veröffentlichung über die Befehlszeile unterstützt:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="2f7a3-148">`MSDeploy` – Mit Web Deploy veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="2f7a3-149">`Package` – Durch Erstellen einer Web Deploy-Paket veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="2f7a3-150">Sie müssen das Paket separat von der MSBuild-Befehl installieren, der erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="2f7a3-151">`FileSystem` – Kopieren von Dateien in einem bestimmten Ordner veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="2f7a3-152">Angeben der Build-Konfiguration und Plattform</span><span class="sxs-lookup"><span data-stu-id="2f7a3-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="2f7a3-153">Die Buildkonfiguration und Plattform müssen in Visual Studio oder in der Befehlszeile festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="2f7a3-154">Die Veröffentlichungsprofile enthalten Eigenschaften, die mit dem Namen sind `LastUsedBuildConfiguration` und `LastUsedPlatform`, aber Sie können diese Eigenschaften festlegen, um zu bestimmen, wie das Projekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="2f7a3-155">Weitere Informationen finden Sie unter [MSBuild: festlegen die Konfigurationseigenschaft](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi-Blog.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="2f7a3-156">In der Stagingumgebung bereitstellen</span><span class="sxs-lookup"><span data-stu-id="2f7a3-156">Deploy to staging</span></span>

<span data-ttu-id="2f7a3-157">Um in Azure bereitstellen, müssen Sie das Kennwort an die Befehlszeile hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="2f7a3-158">Wenn Sie das Kennwort im Veröffentlichungsprofil in Visual Studio gespeichert, es wurde in verschlüsselter Form gespeichert in der Ihre *. pubxml.user* Datei.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="2f7a3-159">Diese Datei wird von MSBuild nicht zugegriffen werden, wenn Sie eine Bereitstellung über die Befehlszeile ausführen, müssen Sie das Kennwort in einem Parameter über die Befehlszeile zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="2f7a3-160">Kopieren Sie das Kennwort, die Sie benötigen die *.publishsettings* zuvor für die staging-Website heruntergeladene Datei.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="2f7a3-161">Das Kennwort ist der Wert der `userPWD` -Attribut für die Web Deploy `publishProfile` Element.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy-Kennwort](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="2f7a3-163">Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**, und klicken Sie auf das Symbol, um die Eingabeaufforderung zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="2f7a3-164">(Sie haben nicht als Administrator dieses Mal öffnen, da Sie in IIS auf dem lokalen Computer bereitstellen.)</span><span class="sxs-lookup"><span data-stu-id="2f7a3-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="2f7a3-165">Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie den Pfad zur Projektmappendatei durch den Pfad zu Ihrer Lösungsdatei hinzufügen und das Kennwort mit Ihrem Kennwort an:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="2f7a3-166">Beachten Sie, dass diese über die Befehlszeile einen zusätzlichen Parameter enthält: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="2f7a3-167">In diesem Tutorial geschrieben wird, die `AllowUntrustedCertificate` Eigenschaft muss festgelegt werden, wenn Sie über die Befehlszeile in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="2f7a3-168">Wenn die Lösung für diesen Fehler veröffentlicht wird, benötigen Sie kein diesen Parameter.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="2f7a3-169">Öffnen Sie einen Browser und navigieren Sie zu der URL der staging-Website, und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="2f7a3-170">Wie Sie für die testumgebung gesehen haben, müssen Sie möglicherweise erstellen Sie einige Schüler/Studenten, um die Statistiken auf finden Sie unter den **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="2f7a3-171">Für die Produktion bereitstellen</span><span class="sxs-lookup"><span data-stu-id="2f7a3-171">Deploy to production</span></span>

<span data-ttu-id="2f7a3-172">Der Prozess für die Bereitstellung in der Produktion ähnelt der Prozess für das Staging.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="2f7a3-173">Kopieren Sie das Kennwort, die Sie benötigen die *.publishsettings* heruntergeladene Datei, zuvor für die Website für die Produktion.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="2f7a3-174">Open **Developer-Eingabeaufforderung für VS2012**.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="2f7a3-175">Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie den Pfad zur Projektmappendatei durch den Pfad zu Ihrer Lösungsdatei hinzufügen und das Kennwort mit Ihrem Kennwort an:</span><span class="sxs-lookup"><span data-stu-id="2f7a3-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="2f7a3-176">Für einen Standort für Produktion, wenn es auch Änderung an einer Datenbank Bestand, in der Regel kopieren Sie die *app\_offline.htm* Datei an den Standort vor der Bereitstellung und löschen Sie ihn nach der erfolgreichen Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="2f7a3-177">Öffnen Sie einen Browser und navigieren Sie zu der URL der staging-Website, und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="2f7a3-178">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="2f7a3-178">Summary</span></span>

<span data-ttu-id="2f7a3-179">Sie haben nun ein Anwendungsupdate bereitgestellt, über die Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-179">You have now deployed an application update by using the command line.</span></span>

![Zu den Seiten im Test-Umgebung](command-line-deployment/_static/image6.png)

<span data-ttu-id="2f7a3-181">Im nächsten Tutorial, sehen Sie ein Beispiel zum Erweitern Sie im Web veröffentlichen Pipeline.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="2f7a3-182">Im Beispiel wird zeigen, wie Dateien bereitstellen, die nicht im Projekt enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2f7a3-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f7a3-183">[Zurück](deploying-a-database-update.md)
> [Weiter](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="2f7a3-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
