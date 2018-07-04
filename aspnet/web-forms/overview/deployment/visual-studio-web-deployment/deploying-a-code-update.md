---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Codeupdates | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368213"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="8cc91-103">ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Codeupdates</span><span class="sxs-lookup"><span data-stu-id="8cc91-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="8cc91-104">durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8cc91-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="8cc91-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="8cc91-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="8cc91-106">Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="8cc91-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="8cc91-107">Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8cc91-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="8cc91-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="8cc91-108">Overview</span></span>

<span data-ttu-id="8cc91-109">Nach der erstbereitstellung Ihre Arbeit verwalten und entwickeln Ihre Website wird fortgesetzt, und vor dem Long-Wert wird ein Update bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="8cc91-110">Dieses Tutorial führt Sie durch den Prozess der Bereitstellung eines Updates an Ihrem Anwendungscode.</span><span class="sxs-lookup"><span data-stu-id="8cc91-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="8cc91-111">Das Update, das Implementieren und bereitstellen, die in diesem Tutorial muss keine Änderung an einer Datenbank werden; sehen Sie, ob Sie zum Bereitstellen der Änderung an einer Datenbank im nächsten Tutorial unterschiedlich ist.</span><span class="sxs-lookup"><span data-stu-id="8cc91-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="8cc91-112">Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8cc91-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="8cc91-113">Einer codeänderung</span><span class="sxs-lookup"><span data-stu-id="8cc91-113">Make a code change</span></span>

<span data-ttu-id="8cc91-114">Fügen Sie ein einfaches Beispiel eines Updates für Ihre Anwendung, um die **Dozenten** Seite eine Liste der Kurse, die von den ausgewählten Dozenten unterrichtet.</span><span class="sxs-lookup"><span data-stu-id="8cc91-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="8cc91-115">Wenn das Ausführen der **Dozenten** Seite werden Sie feststellen, dass es gibt **wählen** Verknüpfungen im Raster, aber nicht der Fall ist etwas anderes als stellen die Zeile Background grau dargestellt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

!["Dozenten" mit Auswahl](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="8cc91-117">Nachdem Sie Code hinzufügen, die ausgeführt, wenn wird die **wählen** Link geklickt wird, und zeigt eine Liste der Kurse, die von den ausgewählten Dozenten unterrichtet.</span><span class="sxs-lookup"><span data-stu-id="8cc91-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="8cc91-118">In *Instructors.aspx*, fügen Sie das folgende Markup direkt nach der **ErrorMessageLabel** `Label` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="8cc91-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="8cc91-119">Führen Sie die Seite, und wählen Sie einen Dozenten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-119">Run the page and select an instructor.</span></span> <span data-ttu-id="8cc91-120">Sie sehen eine Liste der Kurse, die durch dieses Dozenten unterrichtet.</span><span class="sxs-lookup"><span data-stu-id="8cc91-120">You see a list of courses taught by that instructor.</span></span>

    ![Dozentenseite mit Schulungskurse](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="8cc91-122">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="8cc91-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="8cc91-123">Bereitstellen des Code-Updates für die testumgebung</span><span class="sxs-lookup"><span data-stu-id="8cc91-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="8cc91-124">Bevor Sie Ihre Veröffentlichungsprofile verwenden können, für Test-, Staging- und produktionsumgebungen bereitstellen, müssen Sie Optionen für die Veröffentlichung der Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="8cc91-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="8cc91-125">Sie müssen nicht mehr die Bereitstellungsskripts erteilen und die Daten für die Mitgliedschaftsdatenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="8cc91-126">Öffnen der **Webveröffentlichung** Assistenten, indem Sie mit der rechten Maustaste in des Projekts ContosoUniversity und auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="8cc91-127">Klicken Sie auf die **Test** Profil in der **Profil** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="8cc91-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="8cc91-128">Klicken Sie auf die **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="8cc91-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="8cc91-129">Klicken Sie unter **DefaultConnection** in die **Datenbanken** deaktivieren Sie im Abschnitt der **Aktualisieren einer Datenbank** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="8cc91-130">Klicken Sie auf die **Profil** Registerkarte, und klicken Sie dann auf die **Staging** Profil in der **Profil** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="8cc91-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="8cc91-131">Wenn Sie gefragt werden, wenn Sie die Änderungen speichern möchten die **Test** möchten, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="8cc91-132">Stellen Sie die gleiche Änderung in der Staging-Profil ein.</span><span class="sxs-lookup"><span data-stu-id="8cc91-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="8cc91-133">Wiederholen Sie den Vorgang, um die gleiche Änderung im produktionsprofil vornehmen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="8cc91-134">Schließen der **Webveröffentlichung** Assistenten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="8cc91-135">Bereitstellung in der testumgebung ist nun einfach der Ausführung von nur einem Klick erneut veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="8cc91-136">Um diesen Prozess zu beschleunigen, können Sie die **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste.</span><span class="sxs-lookup"><span data-stu-id="8cc91-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="8cc91-137">In der **Ansicht** Menü wählen **Symbolleisten** und wählen Sie dann **klicken Sie auf Webveröffentlichung mit einem**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="8cc91-139">In **Projektmappen-Explorer**, wählen Sie das Projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="8cc91-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="8cc91-140">die **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste wählen Sie die **Test** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung** (das Symbol mit den Pfeilen nach links und rechts).</span><span class="sxs-lookup"><span data-stu-id="8cc91-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="8cc91-142">Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser automatisch geöffnet wird, auf der Startseite.</span><span class="sxs-lookup"><span data-stu-id="8cc91-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="8cc91-143">Führen Sie die dozentenseite, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="8cc91-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="8cc91-144">Sie würden normalerweise auch tun Regressionstests (d. h. den Rest des Standorts, um sicherzustellen, dass die neue Änderung keine vorhandene Funktionalität nicht beeinträchtigt haben testen).</span><span class="sxs-lookup"><span data-stu-id="8cc91-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="8cc91-145">Aber für dieses Tutorial überspringen Sie diesen Schritt und fahren Sie mit der Bereitstellung des Updates zu Staging und Produktion.</span><span class="sxs-lookup"><span data-stu-id="8cc91-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="8cc91-146">Wenn Sie erneut bereitstellen, Web Deploy bestimmt automatisch, welche Dateien geändert wurden, und nur Kopien geänderte Dateien auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="8cc91-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="8cc91-147">Standardmäßig verwendet Web Deploy zuletzt geänderten Datumsangaben für Dateien um zu bestimmen, welche geändert haben.</span><span class="sxs-lookup"><span data-stu-id="8cc91-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="8cc91-148">Einige Quellcodeverwaltungssysteme Ändern der Datei Datumsangaben sogar wenn Sie den Inhalt der Datei nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="8cc91-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="8cc91-149">In diesem Fall empfiehlt es sich so konfigurieren Sie Web Deploy, um die Dateiprüfsummen zu verwenden, um zu bestimmen, welche Dateien geändert wurden.</span><span class="sxs-lookup"><span data-stu-id="8cc91-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="8cc91-150">Weitere Informationen finden Sie unter [Warum alle meine Dateien erneut bereitgestellt, obwohl ich nicht ändern?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in die ASP.NET Bereitstellung – häufig gestellte Fragen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="8cc91-151">Nutzen Sie die Anwendung offline schalten, während der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="8cc91-151">Take the application offline during deployment</span></span>

<span data-ttu-id="8cc91-152">Die Änderung, die Sie bereitstellen, ist jetzt eine einfache Änderung an einer einzelnen Seite.</span><span class="sxs-lookup"><span data-stu-id="8cc91-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="8cc91-153">Aber manchmal Sie größere Änderungen bereitstellen oder Bereitstellen von Anwendungscode und datenbankänderungen, und die Website möglicherweise fehlerhaften Verhalten, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="8cc91-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="8cc91-154">Um zu verhindern, dass Benutzer auf den Standort zugreifen, während der Bereitstellung ausgeführt wird, können Sie eine *app\_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="8cc91-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="8cc91-155">Wenn Sie eine Datei namens einfügen *app\_offline.htm* im Stammordner Ihrer Anwendung, IIS automatisch die Datei statt der Ausführung Ihrer Anwendung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="8cc91-156">Damit um den Zugriff während der Bereitstellung zu verhindern, Sie fügen *app\_offline.htm* im Stammordner, den Bereitstellungsvorgang ausführen, und entfernen Sie *app\_offline.htm* nach dem erfolgreichen die Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="8cc91-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="8cc91-157">Sie können konfigurieren, Web Deploy, um automatisch einen Standardwert versetzen *app\_offline.htm* Datei auf dem Server beim Start bereitstellen und entfernen Sie sie an, wenn er abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="8cc91-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="8cc91-158">Zu diesem Zweck müssen Sie lediglich ist Ihre veröffentlichungsprofildatei (.pubxml) das folgende XML-Element hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="8cc91-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="8cc91-159">In diesem Tutorial erfahren Sie zum Erstellen und verwenden ein benutzerdefinierten *app\_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="8cc91-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="8cc91-160">Mithilfe von *app\_offline.htm* auf der Stagingsite ist nicht erforderlich, da Sie nicht über Benutzer, die Zugriff auf die Stagingsite verfügen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="8cc91-161">Aber es wird empfohlen, Sie verwenden Test die Möglichkeit, die Sie in der Produktion bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="8cc91-162">Erstellen Sie app\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="8cc91-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="8cc91-163">In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="8cc91-164">Erstellen Sie eine **HTML-Seite** mit dem Namen *app\_offline.htm* (löschen Sie die endgültige "l" in der *.html* -Erweiterung, die Visual Studio wird standardmäßig erstellt).</span><span class="sxs-lookup"><span data-stu-id="8cc91-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="8cc91-165">Ersetzen Sie das Vorlagenmarkup, durch das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="8cc91-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="8cc91-166">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="8cc91-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="8cc91-167">App kopieren\_offline.htm in den Stammordner der Website</span><span class="sxs-lookup"><span data-stu-id="8cc91-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="8cc91-168">Sie können alle FTP-Tool verwenden, zum Kopieren von Dateien mit der Website.</span><span class="sxs-lookup"><span data-stu-id="8cc91-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="8cc91-169">[FileZilla](http://filezilla-project.org/) ist ein beliebtes Tool für die FTP und wird in den Screenshots gezeigt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="8cc91-170">Um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort.</span><span class="sxs-lookup"><span data-stu-id="8cc91-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="8cc91-171">Die URL wird auf der Website-Dashboard-Seite im Azure-Verwaltungsportal angezeigt, und den Benutzernamen und das Kennwort für FTP finden Sie in der *.publishsettings* -Datei, die Sie zuvor heruntergeladen haben.</span><span class="sxs-lookup"><span data-stu-id="8cc91-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="8cc91-172">Die folgenden Schritte zeigen, wie Sie diese Werte zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="8cc91-173">Klicken Sie in der Azure-Verwaltungsportal auf **Websites** Registerkarte, und klicken Sie dann auf die staging-Website.</span><span class="sxs-lookup"><span data-stu-id="8cc91-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="8cc91-174">Auf der **Dashboard** Seite Scrollen Sie zu suchen, die der FTP-Hostname angegeben wird, in der **Blick** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP-Hostname](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="8cc91-176">Öffnen Sie das Staging *.publishsettings* Datei in Notepad oder einem anderen Texteditor.</span><span class="sxs-lookup"><span data-stu-id="8cc91-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="8cc91-177">Suchen der `publishProfile` -Element für das FTP-Profil.</span><span class="sxs-lookup"><span data-stu-id="8cc91-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="8cc91-178">Kopieren der `userName` und `userPWD` Werte.</span><span class="sxs-lookup"><span data-stu-id="8cc91-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP-Anmeldeinformationen](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="8cc91-180">Öffnen Sie Ihren FTP-Tool, und melden Sie sich bei der FTP-URL.</span><span class="sxs-lookup"><span data-stu-id="8cc91-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="8cc91-181">Kopie *app\_offline.htm* im Projektmappenordner, den *"/ Site/wwwroot"* Ordner auf der staging-Website.</span><span class="sxs-lookup"><span data-stu-id="8cc91-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Kopieren Sie "App_offline"](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="8cc91-183">Navigieren Sie zu Ihrer staging-Website-URL.</span><span class="sxs-lookup"><span data-stu-id="8cc91-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="8cc91-184">Sie sehen, dass die *app\_offline.htm* Seite wird nun anstelle Ihrer Startseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm im Browserfenster](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="8cc91-186">Sie können nun in der Stagingumgebung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="8cc91-187">Bereitstellen des Code-Updates für Staging und Produktion</span><span class="sxs-lookup"><span data-stu-id="8cc91-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="8cc91-188">In der **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste wählen Sie die **Staging** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="8cc91-189">Visual Studio stellt die aktualisierte Anwendung bereit und öffnet der Browser zur Startseite der Website.</span><span class="sxs-lookup"><span data-stu-id="8cc91-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="8cc91-190">Die *app\_offline.htm* Datei wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="8cc91-191">Bevor Sie zum Überprüfen der erfolgreichen Bereitstellung testen können, müssen Sie entfernen die *app\_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="8cc91-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="8cc91-192">Zurück zu Ihrem FTP-Tool, und Löschen von **app\_offline.htm** von der staging-Website.</span><span class="sxs-lookup"><span data-stu-id="8cc91-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="8cc91-193">Klicken Sie im Browser öffnen Sie die dozentenseite auf der Stagingsite, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="8cc91-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="8cc91-194">Führen Sie das gleiche Verfahren für die Produktion, ebenso wie für staging.</span><span class="sxs-lookup"><span data-stu-id="8cc91-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="8cc91-195">Überprüfen von Änderungen und Bereitstellen von Dateien</span><span class="sxs-lookup"><span data-stu-id="8cc91-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="8cc91-196">Visual Studio 2012 gibt Ihnen außerdem die Möglichkeit, einzelne Dateien bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="8cc91-197">Für eine ausgewählte Datei können Sie Unterschiede zwischen der lokalen Version und die bereitgestellte Version anzeigen, die Datei in die zielumgebung bereitstellen oder kopieren Sie die Datei von der zielumgebung für das lokale Projekt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="8cc91-198">In diesem Abschnitt des Tutorials erfahren Sie, wie diese Funktionen verwendet.</span><span class="sxs-lookup"><span data-stu-id="8cc91-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="8cc91-199">Nehmen Sie eine Änderung bereitstellen</span><span class="sxs-lookup"><span data-stu-id="8cc91-199">Make a change to deploy</span></span>

1. <span data-ttu-id="8cc91-200">Open *Content/Site.css*, und suchen Sie den Block für die `body` Tag.</span><span class="sxs-lookup"><span data-stu-id="8cc91-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="8cc91-201">Ändern Sie den Wert für `background-color` aus `#fff` zu `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="8cc91-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="8cc91-202">Überprüfen Sie die Änderung im Fenster Vorschau veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="8cc91-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="8cc91-203">Bei Verwendung der **Web veröffentlichen** Assistenten zum Veröffentlichen des Projekts, sehen Sie, welche Änderungen ausgeführt werden soll veröffentlicht werden, durch Doppelklicken auf die Datei in die **Vorschau** Fenster.</span><span class="sxs-lookup"><span data-stu-id="8cc91-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="8cc91-204">Mit der rechten Maustaste in des ContosoUniversity-Projekts, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="8cc91-205">Von der **Profil** Dropdown-Liste die **Test** Veröffentlichungsprofil.</span><span class="sxs-lookup"><span data-stu-id="8cc91-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="8cc91-206">Klicken Sie auf **Vorschau**, und klicken Sie dann auf **Vorschau starten**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="8cc91-207">In der **Vorschau** Bereich doppelklicken Sie auf **"Site.CSS"**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Doppelklicken Sie auf "Site.CSS"](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="8cc91-209">Die **Vorschau der Änderungen** Dialogfeld zeigt eine Vorschau der Änderungen, die bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="8cc91-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Vorschau der Änderungen auf "Site.CSS"](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="8cc91-211">Wenn Sie doppelklicken Sie auf die *"Web.config"* -Datei, die **Vorschau der Änderungen** Dialogfeld zeigt die Auswirkungen des Builds Konfiguration von Transformationen, und veröffentlichen Sie die Profil-Transformationen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="8cc91-212">An diesem Punkt haben dies nicht etwas, das würde dazu führen, dass die *"Web.config"* Datei auf dem Server zu ändern, sodass Sie erwarten, dass keine Änderungen sehen,.</span><span class="sxs-lookup"><span data-stu-id="8cc91-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="8cc91-213">Allerdings die **Vorschau der Änderungen** Fenster zeigt fälschlicherweise zwei Änderungen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="8cc91-214">Offenbar zwei XML-Elemente werden entfernt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="8cc91-215">Diese Elemente werden durch die Veröffentlichung hinzugefügt, bei der Auswahl **Code First-Migrationen ausführen beim Start der Anwendung** für ein Code First Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="8cc91-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="8cc91-216">Der Vergleich erfolgt, bevor der Veröffentlichungsprozess diese Elemente hinzugefügt, sodass sie aussieht, wie sie entfernt werden, obwohl sie nicht entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="8cc91-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="8cc91-217">Dieser Fehler wird in einer zukünftigen Version behoben werden.</span><span class="sxs-lookup"><span data-stu-id="8cc91-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="8cc91-218">Klicken Sie auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-218">Click **Close**.</span></span>
6. <span data-ttu-id="8cc91-219">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-219">Click **Publish**.</span></span>
7. <span data-ttu-id="8cc91-220">Wenn der Browser zur Startseite der Test-Website geöffnet wird, drücken Sie STRG + F5, um eine Aktualisierung schwierig zu bewirken, um die Auswirkungen der Änderung CSS finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="8cc91-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Auswirkung der CSS-Änderung](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="8cc91-222">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="8cc91-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="8cc91-223">Veröffentlichen Sie bestimmte Dateien im Projektmappen-Explorer</span><span class="sxs-lookup"><span data-stu-id="8cc91-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="8cc91-224">Angenommen Sie, Sie nicht wie die blauen Hintergrund und die ursprüngliche Farbe wiederherstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="8cc91-225">In diesem Abschnitt müssen Sie die ursprünglichen Einstellungen wiederherstellen, indem Sie eine bestimmte Datei direkt aus veröffentlichen **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="8cc91-226">Open *Content/Site.css* und Wiederherstellen der `background-color` auf `#fff`.</span><span class="sxs-lookup"><span data-stu-id="8cc91-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="8cc91-227">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Content/Site.css* Datei.</span><span class="sxs-lookup"><span data-stu-id="8cc91-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="8cc91-228">Das Kontextmenü enthält, dass drei Optionen veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-228">The context menu shows three publish options.</span></span>

    ![Optionen im Projektmappen-Explorer veröffentlichen](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="8cc91-230">Klicken Sie auf **Vorschau der Änderungen auf "Site.CSS"**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="8cc91-231">Ein Fenster wird geöffnet, um die Unterschiede zwischen der lokalen Datei und die Version in der zielumgebung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="8cc91-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content / "Site.CSS"](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="8cc91-233">In **Projektmappen-Explorer**, mit der rechten Maustaste **"Site.CSS"** erneut aus, und klicken Sie auf **veröffentlichen "Site.CSS"**.</span><span class="sxs-lookup"><span data-stu-id="8cc91-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="8cc91-234">Die **Webveröffentlichungsaktivität** zeigt an, dass die Datei veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="8cc91-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Fenster "Web-Aktivität veröffentlichen"](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="8cc91-236">Öffnen Sie einen Browser auf die `http://localhost/contosouniversity` URL, und drücken Sie dann STRG + F5, um einen hartcodierten dazu führen, dass aktualisieren, um die Auswirkungen des CSS, ändern, finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="8cc91-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Auf der Startseite mit normalen CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="8cc91-238">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="8cc91-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="8cc91-239">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="8cc91-239">Summary</span></span>

<span data-ttu-id="8cc91-240">Sie haben nun gesehen, mehrere Möglichkeiten, ein Anwendungsupdate bereitstellen, die keine Änderung an einer Datenbank verbunden ist, und Sie haben gesehen, wie Sie eine Vorschau der Änderungen, um sicherzustellen, dass was aktualisiert werden, was Sie erwarten.</span><span class="sxs-lookup"><span data-stu-id="8cc91-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="8cc91-241">Die dozentenseite verfügt jetzt über eine **Kurse unterrichtet** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="8cc91-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Dozentenseite mit Schulungskurse](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="8cc91-243">Im nächste Tutorial erfahren Sie, wie zum Bereitstellen der Änderung an einer Datenbank: Fügen Sie ein Feld "BirthDate" in der Datenbank und zur dozentenseite.</span><span class="sxs-lookup"><span data-stu-id="8cc91-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8cc91-244">[Zurück](deploying-to-production.md)
> [Weiter](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="8cc91-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
