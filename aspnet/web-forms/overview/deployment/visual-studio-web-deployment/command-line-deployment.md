---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Befehlszeile Bereitstellung | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890527"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="74fa4-103">ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellung über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="74fa4-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="74fa4-104">durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="74fa4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="74fa4-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="74fa4-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="74fa4-106">Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="74fa4-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="74fa4-107">Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="74fa4-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="74fa4-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="74fa4-108">Overview</span></span>

<span data-ttu-id="74fa4-109">Dieses Lernprogramm veranschaulicht das Aufrufen der Visual Studio Web veröffentlichen Pipeline über die Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="74fa4-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="74fa4-110">Dies ist hilfreich, wenn Sie möchten [automatisieren Sie den Bereitstellungsprozess](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anstatt dies manuell in Visual Studio, in der Regel durch die Verwendung einer [source Code Versionskontrollsystem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="74fa4-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="74fa4-111">Nehmen Sie eine Änderung bereitstellen</span><span class="sxs-lookup"><span data-stu-id="74fa4-111">Make a change to deploy</span></span>

<span data-ttu-id="74fa4-112">Derzeit zeigt die Seite "Info" im Vorlagencode.</span><span class="sxs-lookup"><span data-stu-id="74fa4-112">Currently the About page displays the template code.</span></span>

![Zu den Seiten mit Vorlagencode](command-line-deployment/_static/image1.png)

<span data-ttu-id="74fa4-114">Sie werden, die durch Code ersetzen, die eine Zusammenfassung der Student-Anmeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="74fa4-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="74fa4-115">Öffnen der *About.aspx* Seite, löschen Sie alle Markups in der `MainContent` `Content` -Element, und legen Sie das folgende Markup an seiner Stelle:</span><span class="sxs-lookup"><span data-stu-id="74fa4-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="74fa4-116">Führen Sie das Projekt, und wählen Sie die **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="74fa4-116">Run the project and select the **About** page.</span></span>

![Seite „Info“](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="74fa4-118">Bereitstellen Sie in Test mithilfe der Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="74fa4-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="74fa4-119">Sie wird nicht einer anderen Datenbank ändern, daher deaktivieren DbDacFx-datenbankbereitstellung für das Aspnet-ContosoUniversity Datenbank bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="74fa4-120">Öffnen der **Web veröffentlichen** Assistenten, und in jeder der drei Veröffentlichungsprofile, Deaktivieren der **Update Database** Kontrollkästchen auf der **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="74fa4-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="74fa4-121">Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**.</span><span class="sxs-lookup"><span data-stu-id="74fa4-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="74fa4-122">Mit der rechten Maustaste des Symbol für **Developer-Eingabeaufforderung für VS2012** , und klicken Sie auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="74fa4-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="74fa4-123">Geben Sie an der Befehlszeile aus, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad zu Ihrer Projektmappendatei den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="74fa4-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="74fa4-124">MSBuild erstellt die Lösung und stellt sie in der testumgebung bereit.</span><span class="sxs-lookup"><span data-stu-id="74fa4-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Befehlszeilenausgabe](command-line-deployment/_static/image3.png)

<span data-ttu-id="74fa4-126">Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="74fa4-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="74fa4-127">Wenn Sie alle Studenten im Test erstellt haben, sehen Sie eine leere Seite unter das **Student Text Statistiken** Überschrift.</span><span class="sxs-lookup"><span data-stu-id="74fa4-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="74fa4-128">Wechseln Sie zu der **Studenten** auf **Student hinzufügen**, und einige Studenten hinzufügen, und fahren Sie dann mit der **zu** Seite Student-Statistik anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Zu den Seiten in der testumgebung](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="74fa4-130">Key-Befehlszeilenoptionen</span><span class="sxs-lookup"><span data-stu-id="74fa4-130">Key command line options</span></span>

<span data-ttu-id="74fa4-131">Der Befehl, den eingegebene Pfad der Projektmappe und zwei Eigenschaften an MSBuild übergeben:</span><span class="sxs-lookup"><span data-stu-id="74fa4-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="74fa4-132">Bereitstellen der Lösung im Vergleich zur Bereitstellung von einzelnen Projekten</span><span class="sxs-lookup"><span data-stu-id="74fa4-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="74fa4-133">Die Projektmappendatei Angabe bewirkt, dass alle Projekte in der Projektmappe erstellt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="74fa4-134">Wenn Sie mehrere Webprojekte in der Projektmappe haben, gilt MSBuild Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="74fa4-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="74fa4-135">Die Eigenschaften, die Sie in der Befehlszeile angeben, werden auf jedes Projekt übergeben.</span><span class="sxs-lookup"><span data-stu-id="74fa4-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="74fa4-136">Daher muss jedes Webprojekt ein Veröffentlichungsprofil mit dem Namen verfügen, die Sie angeben.</span><span class="sxs-lookup"><span data-stu-id="74fa4-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="74fa4-137">Bei Angabe von `/p:PublishProfile=Test`, jedes Webprojekt benötigen ein Veröffentlichungsprofil mit dem Namen *Test*.</span><span class="sxs-lookup"><span data-stu-id="74fa4-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="74fa4-138">Sie können ein Projekt erfolgreich veröffentlichen, wenn eine andere nicht selbst erstellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="74fa4-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="74fa4-139">Weitere Informationen finden Sie im Stackoverflow-Thread [MSBuild schlägt fehl mit zwei Pakete](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="74fa4-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="74fa4-140">Wenn Sie ein einzelnes Projekt anstelle einer Projektmappe angeben, müssen Sie einen Parameter hinzufügen, der das Visual Studio-Version angibt.</span><span class="sxs-lookup"><span data-stu-id="74fa4-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="74fa4-141">Bei Verwendung von Visual Studio 2012 wäre die Befehlszeile im folgenden Beispiel ähnelt:</span><span class="sxs-lookup"><span data-stu-id="74fa4-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="74fa4-142">Die Versionsnummer für Visual Studio 2010 ist 10,0.</span><span class="sxs-lookup"><span data-stu-id="74fa4-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="74fa4-143">Weitere Informationen finden Sie unter [Visual Studio-Projekt zur Anwendungskompatibilität und VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi-Blog.</span><span class="sxs-lookup"><span data-stu-id="74fa4-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="74fa4-144">Das Veröffentlichungsprofil angeben</span><span class="sxs-lookup"><span data-stu-id="74fa4-144">Specifying the publish profile</span></span>

<span data-ttu-id="74fa4-145">Sie können das Veröffentlichungsprofil angeben, nach Namen oder den vollständigen Pfad zu der *pubxml* Datei, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="74fa4-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="74fa4-146">Web veröffentlichen für Befehlszeilen Veröffentlichung unterstützte Methoden</span><span class="sxs-lookup"><span data-stu-id="74fa4-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="74fa4-147">Veröffentlichen von drei Methoden werden für die Veröffentlichung über die Befehlszeile unterstützt:</span><span class="sxs-lookup"><span data-stu-id="74fa4-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="74fa4-148">`MSDeploy` -Mithilfe von Web Deploy veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="74fa4-149">`Package` -Veröffentlichen Sie, indem Sie ein Web Deploy-Paket erstellen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="74fa4-150">Sie müssen das Paket getrennt von der MSBuild-Befehl installieren, die es erstellt.</span><span class="sxs-lookup"><span data-stu-id="74fa4-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="74fa4-151">`FileSystem` -Durch Kopieren von Dateien in einem angegebenen Ordner veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="74fa4-152">Angeben der Build-Konfiguration und Plattform</span><span class="sxs-lookup"><span data-stu-id="74fa4-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="74fa4-153">Die Buildkonfiguration und die Plattform müssen in Visual Studio oder in der Befehlszeile festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="74fa4-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="74fa4-154">Veröffentlichungsprofile enthalten Eigenschaften, die mit dem Namen sind `LastUsedBuildConfiguration` und `LastUsedPlatform`, aber diese Eigenschaften können nicht festgelegt werden, um zu bestimmen, wie das Projekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="74fa4-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="74fa4-155">Weitere Informationen finden Sie unter [MSBuild: Gewusst wie: festlegen die Konfigurationseigenschaft](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi-Blog.</span><span class="sxs-lookup"><span data-stu-id="74fa4-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="74fa4-156">In die Stagingumgebung bereitstellen</span><span class="sxs-lookup"><span data-stu-id="74fa4-156">Deploy to staging</span></span>

<span data-ttu-id="74fa4-157">Um in Azure bereitstellen, müssen Sie das Kennwort in der Befehlszeile hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="74fa4-158">Wenn Sie das Kennwort in das Veröffentlichungsprofil in Visual Studio gespeichert, es wurde in verschlüsselter Form gespeichert in der Ihre *. pubxml.user* Datei.</span><span class="sxs-lookup"><span data-stu-id="74fa4-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="74fa4-159">Diese Datei wird von MSBuild nicht zugegriffen werden, wenn Sie eine Bereitstellung über die Befehlszeile ausführen, müssen Sie das Kennwort in einem Befehlszeilenparameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="74fa4-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="74fa4-160">Kopieren Sie das Kennwort, die Sie benötigen die *publishsettings* die heruntergeladene Datei zuvor für das staging-Website.</span><span class="sxs-lookup"><span data-stu-id="74fa4-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="74fa4-161">Das Kennwort ist der Wert der `userPWD` -Attribut für das Web Deploy `publishProfile` Element.</span><span class="sxs-lookup"><span data-stu-id="74fa4-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy-Kennwort](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="74fa4-163">Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**, und klicken Sie auf das Symbol, um das Eingabeaufforderungsfenster zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="74fa4-164">(Sie müssen als Administrator dieses Mal öffnen, da Sie für IIS auf dem lokalen Computer bereitstellen werden nicht.)</span><span class="sxs-lookup"><span data-stu-id="74fa4-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="74fa4-165">Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad in der Projektmappendatei und das Kennwort durch Ihr Kennwort ein:</span><span class="sxs-lookup"><span data-stu-id="74fa4-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="74fa4-166">Beachten Sie, dass dieser über die Befehlszeile einen zusätzlichen Parameter enthält: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="74fa4-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="74fa4-167">Wie in diesem Lernprogramm geschrieben wird, die `AllowUntrustedCertificate` Eigenschaft muss festgelegt werden, wenn Sie über die Befehlszeile in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="74fa4-168">Wenn diese Fehlerkorrektur veröffentlicht wird, brauchen Sie keine Parameter.</span><span class="sxs-lookup"><span data-stu-id="74fa4-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="74fa4-169">Öffnen Sie einen Browser, und wechseln Sie zu der URL der staging-Website und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="74fa4-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="74fa4-170">Wie Sie zuvor für die testumgebung gesehen haben, müssen Sie möglicherweise einige Studenten Statistiken für erhalten Sie erstellen die **zu** Seite.</span><span class="sxs-lookup"><span data-stu-id="74fa4-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="74fa4-171">Für die Produktion bereitstellen</span><span class="sxs-lookup"><span data-stu-id="74fa4-171">Deploy to production</span></span>

<span data-ttu-id="74fa4-172">Der Prozess für die Bereitstellung bis hin zur Produktion ähnelt der Prozess für das Staging.</span><span class="sxs-lookup"><span data-stu-id="74fa4-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="74fa4-173">Kopieren Sie das Kennwort, die Sie benötigen die *publishsettings* die heruntergeladene Datei zuvor für die Produktion-Website.</span><span class="sxs-lookup"><span data-stu-id="74fa4-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="74fa4-174">Open **Developer-Eingabeaufforderung für VS2012**.</span><span class="sxs-lookup"><span data-stu-id="74fa4-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="74fa4-175">Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad in der Projektmappendatei und das Kennwort durch Ihr Kennwort ein:</span><span class="sxs-lookup"><span data-stu-id="74fa4-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="74fa4-176">Für einen Standort Produktion gäbe auch Änderung an einer Datenbank, in der Regel kopieren Sie die *app\_offline.htm* Datei wird in der Website vor der Bereitstellung und nach der erfolgreichen Bereitstellung zu löschen.</span><span class="sxs-lookup"><span data-stu-id="74fa4-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="74fa4-177">Öffnen Sie einen Browser, und wechseln Sie zu der URL der staging-Website und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="74fa4-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="74fa4-178">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="74fa4-178">Summary</span></span>

<span data-ttu-id="74fa4-179">Sie haben jetzt ein Anwendungsupdate bereitgestellt, über die Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="74fa4-179">You have now deployed an application update by using the command line.</span></span>

![Zu den Seiten in der testumgebung](command-line-deployment/_static/image6.png)

<span data-ttu-id="74fa4-181">In den nächsten Lernprogrammen sehen Sie ein Beispiel zum Erweitern Sie im Web veröffentlichen Pipeline.</span><span class="sxs-lookup"><span data-stu-id="74fa4-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="74fa4-182">Im Beispiel wird gezeigt, wie Dateien bereitstellen, die nicht im Projekt enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="74fa4-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74fa4-183">[Zurück](deploying-a-database-update.md)
> [Weiter](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="74fa4-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
