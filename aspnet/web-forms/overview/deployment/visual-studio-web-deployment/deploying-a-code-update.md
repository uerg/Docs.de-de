---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von einem Code-Update | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="1c218-103">ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von einem Code-Update</span><span class="sxs-lookup"><span data-stu-id="1c218-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="1c218-104">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1c218-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1c218-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="1c218-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="1c218-106">Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1c218-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="1c218-107">Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1c218-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="1c218-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1c218-108">Overview</span></span>

<span data-ttu-id="1c218-109">Nach der anfänglichen Bereitstellung können Ihre Arbeit verwalten und entwickeln Ihre Website fortgesetzt und lange wird eine Aktualisierung bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="1c218-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="1c218-110">Dieses Lernprogramm führt Sie durch den Prozess der Bereitstellung von Aktualisierungen an den Anwendungscode.</span><span class="sxs-lookup"><span data-stu-id="1c218-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="1c218-111">Das Update, das Sie implementieren und bereitstellen, die in diesem Lernprogramm beinhaltet keine Änderung an einer Datenbank; sehen Sie, ob Sie zum Bereitstellen der Änderung an einer Datenbank in den nächsten Lernprogrammen unterschiedlich ist.</span><span class="sxs-lookup"><span data-stu-id="1c218-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="1c218-112">Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1c218-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="1c218-113">Eine codeänderung</span><span class="sxs-lookup"><span data-stu-id="1c218-113">Make a code change</span></span>

<span data-ttu-id="1c218-114">Als einfaches Beispiel eines Updates für Ihre Anwendung, fügen Sie auf der **Lehrkräfte** Seite eine Liste der ausgewählten Dozent unterrichtet Kurse.</span><span class="sxs-lookup"><span data-stu-id="1c218-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="1c218-115">Wenn das Ausführen der **Lehrkräfte** Seite werden Sie feststellen, dass es gibt **wählen** Links im Raster, aber nicht der Fall ist etwas anderes als stellen die Zeile Background grau dargestellt.</span><span class="sxs-lookup"><span data-stu-id="1c218-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Seite "Lehrkräfte" mit der Auswahl](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="1c218-117">Nachdem Sie Code hinzufügen, die ausgeführt, wenn wird die **wählen** Link geklickt wird, und zeigt eine Liste der ausgewählten Dozent unterrichtet Kurse.</span><span class="sxs-lookup"><span data-stu-id="1c218-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="1c218-118">In *Instructors.aspx*, fügen Sie das folgende Markup unmittelbar nach der **ErrorMessageLabel** `Label` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="1c218-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="1c218-119">Führen Sie die Seite, und wählen Sie einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="1c218-119">Run the page and select an instructor.</span></span> <span data-ttu-id="1c218-120">Sie sehen eine Liste der Kurse, Dozent unterrichtet.</span><span class="sxs-lookup"><span data-stu-id="1c218-120">You see a list of courses taught by that instructor.</span></span>

    ![Trainer-Seite mit Kurse vermittelten](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="1c218-122">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="1c218-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="1c218-123">Bereitstellen des Code-Updates in der testumgebung</span><span class="sxs-lookup"><span data-stu-id="1c218-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="1c218-124">Bevor Sie Ihre Veröffentlichungsprofile verwenden können, um auf Test-, Staging- und produktionsumgebung bereitzustellen, müssen Sie Optionen für die Veröffentlichung der Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="1c218-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="1c218-125">Sie müssen nicht mehr die Bereitstellungsskripts erteilen und die Daten für die Mitgliedschaftsdatenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="1c218-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="1c218-126">Öffnen der **Web veröffentlichen** Assistenten, indem Sie mit der rechten Maustaste des Projekts ContosoUniversity und auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1c218-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="1c218-127">Klicken Sie auf die **Test** im Profil der **Profil** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="1c218-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="1c218-128">Klicken Sie auf die **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="1c218-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="1c218-129">Klicken Sie unter **DefaultConnection** in der **Datenbanken** deaktivieren Sie im Abschnitt der **Datenbank aktualisieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="1c218-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="1c218-130">Klicken Sie auf die **Profil** Registerkarte, und klicken Sie dann auf die **Staging** im Profil der **Profil** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="1c218-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="1c218-131">Wenn Sie gefragt werden, wenn Sie die vorgenommenen Änderungen speichern möchten die **Test** möchten, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="1c218-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="1c218-132">Stellen Sie die gleiche Änderung in den Staging-Profil ein.</span><span class="sxs-lookup"><span data-stu-id="1c218-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="1c218-133">Wiederholen Sie den Vorgang, um die gleiche Änderung in der Produktion-Profil vornehmen.</span><span class="sxs-lookup"><span data-stu-id="1c218-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="1c218-134">Schließen der **Web veröffentlichen** Assistenten.</span><span class="sxs-lookup"><span data-stu-id="1c218-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="1c218-135">Bereitstellung in der testumgebung ist nun problemlos der Ausführung nur einem Klick erneut veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1c218-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="1c218-136">Um diesen Prozess zu beschleunigen, können Sie die **Web eine klicken Sie auf Publish** Symbolleiste.</span><span class="sxs-lookup"><span data-stu-id="1c218-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="1c218-137">In der **Ansicht** Menü Wählen Sie **Symbolleisten** und wählen Sie dann **Web eine klicken Sie auf Publish**.</span><span class="sxs-lookup"><span data-stu-id="1c218-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="1c218-139">In **Projektmappen-Explorer**, wählen Sie das Projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="1c218-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="1c218-140">die **Web eine klicken Sie auf Publish** Symbolleiste, wählen Sie die **Test** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen** (das Symbol mit den Pfeilen nach links und rechts).</span><span class="sxs-lookup"><span data-stu-id="1c218-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="1c218-142">Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser automatisch geöffnet wird, um zur Startseite.</span><span class="sxs-lookup"><span data-stu-id="1c218-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="1c218-143">Führen Sie die Seite Lehrkräfte, und wählen Sie einen Kursleiter, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="1c218-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="1c218-144">Sie würden normalerweise Gleiches Regressionstests (d. h. den Rest des Standorts, um sicherzustellen, dass die neue Änderung vorhandene Funktionalität unterbrechen nicht testen).</span><span class="sxs-lookup"><span data-stu-id="1c218-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="1c218-145">Aber für dieses Lernprogramm überspringen Sie diesen Schritt und fahren Sie mit das Update auf Staging- und produktionsumgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1c218-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="1c218-146">Wenn Sie erneut bereitstellen, Web Deploy automatisch bestimmt, welche Dateien geändert wurden, und nur Kopien Dateien auf dem Server geändert.</span><span class="sxs-lookup"><span data-stu-id="1c218-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="1c218-147">Standardmäßig verwendet Web Deploy zuletzt geändert von Datumsangaben auf Dateien um zu bestimmen, welche geändert haben.</span><span class="sxs-lookup"><span data-stu-id="1c218-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="1c218-148">Einige Quellcode-Verwaltungssysteme ändern Datei Datumsangaben auch, wenn Sie den Inhalt der Datei nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="1c218-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="1c218-149">In diesem Fall empfiehlt es sich so konfigurieren Sie Web Deploy, um die Dateiprüfsummen verwenden, um zu bestimmen, welche Dateien geändert wurden.</span><span class="sxs-lookup"><span data-stu-id="1c218-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="1c218-150">Weitere Informationen finden Sie unter [Warum alle Dateien abrufen erneut bereitgestellt, obwohl ich sie geändert haben?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) in der ASP.NET Bereitstellung – häufig gestellte Fragen.</span><span class="sxs-lookup"><span data-stu-id="1c218-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="1c218-151">Nehmen Sie die Anwendung offline, während der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="1c218-151">Take the application offline during deployment</span></span>

<span data-ttu-id="1c218-152">Die Änderung, die Sie bereitstellen, ist jetzt eine einfache Änderung an einer einzelnen Seite.</span><span class="sxs-lookup"><span data-stu-id="1c218-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="1c218-153">Aber manchmal Sie größere Änderungen bereitstellen oder Bereitstellen von Code und Datenbank ändert, und die Website möglicherweise falsch verhält, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="1c218-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="1c218-154">Um zu verhindern, dass Benutzer auf den Standort zugreifen, während der Bereitstellung ausgeführt wird, können Sie eine *app\_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="1c218-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="1c218-155">Wenn Sie eine Datei namens einfügen *app\_offline.htm* im Stammordner der Anwendung, IIS automatisch diese Datei statt der Anwendungsstatus angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1c218-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="1c218-156">Damit während der Bereitstellung den Zugriff verhindern möchten, Sie platzieren *app\_offline.htm* im Stammordner, den Bereitstellungsvorgang ausführen, und entfernen Sie *app\_offline.htm* nach dem erfolgreichen Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="1c218-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="1c218-157">Sie können konfigurieren, Web Deploy, um automatisch einen Standardwert einzufügen *app\_offline.htm* Datei auf dem Server beim Start bereitstellen und sie nach dem Abschluss zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="1c218-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="1c218-158">Dazu, dass alle Sie lediglich müssen ist das folgende XML-Element die veröffentlichungsprofildatei (.pubxml) hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="1c218-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="1c218-159">In diesem Lernprogramm erfahren Sie, wie zum Erstellen und verwenden ein benutzerdefinierten *app\_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="1c218-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="1c218-160">Mit *app\_offline.htm* in die Stagingsite ist nicht erforderlich, da Sie nicht über Benutzer, die Zugriff auf die Stagingsite verfügen.</span><span class="sxs-lookup"><span data-stu-id="1c218-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="1c218-161">Aber es wird empfohlen, staging verwenden, um alles, was die Möglichkeit zu testen, die Sie in der Produktion bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="1c218-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="1c218-162">Erstellen Sie die app\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1c218-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="1c218-163">In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="1c218-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="1c218-164">Erstellen einer **HTML-Seite** mit dem Namen *app\_offline.htm* (löschen Sie die endgültige "l" in der *.html* -Erweiterung, die Visual Studio standardmäßig erstellt).</span><span class="sxs-lookup"><span data-stu-id="1c218-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="1c218-165">Ersetzen Sie das Vorlagenmarkup durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="1c218-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="1c218-166">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="1c218-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="1c218-167">Kopie app\_offline.htm in den Stammordner der Website</span><span class="sxs-lookup"><span data-stu-id="1c218-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="1c218-168">Sie können alle FTP-Tool verwenden, zum Kopieren von Dateien mit der Website.</span><span class="sxs-lookup"><span data-stu-id="1c218-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="1c218-169">[FileZilla](http://filezilla-project.org/) ist ein gängiges Tool für die FTP- und in die Screenshots angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="1c218-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="1c218-170">Um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort.</span><span class="sxs-lookup"><span data-stu-id="1c218-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="1c218-171">Die URL wird auf der Website-Dashboard-Seite im Azure-Verwaltungsportal angezeigt, und den Benutzernamen und das Kennwort für FTP finden Sie in der *publishsettings* -Datei, die Sie zuvor heruntergeladen haben.</span><span class="sxs-lookup"><span data-stu-id="1c218-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="1c218-172">Die folgenden Schritte zeigen, wie Sie diese Werte abrufen.</span><span class="sxs-lookup"><span data-stu-id="1c218-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="1c218-173">Klicken Sie in der Azure-Verwaltungsportal auf **Websites** Registerkarte, und klicken Sie dann auf den staging-Website.</span><span class="sxs-lookup"><span data-stu-id="1c218-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="1c218-174">Auf der **Dashboard** Seite, scrollen Sie zu suchen, die der FTP-in Hostname der **Blick** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="1c218-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP-Hostname](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="1c218-176">Öffnen Sie das Staging *publishsettings* Datei in Editor oder einem anderen Texteditor.</span><span class="sxs-lookup"><span data-stu-id="1c218-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="1c218-177">Suchen der `publishProfile` -Element für das FTP-Profil.</span><span class="sxs-lookup"><span data-stu-id="1c218-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="1c218-178">Kopieren der `userName` und `userPWD` Werte.</span><span class="sxs-lookup"><span data-stu-id="1c218-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP-Anmeldeinformationen](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="1c218-180">Öffnen Sie Ihren FTP-Tool, und melden Sie sich bei der FTP-URL.</span><span class="sxs-lookup"><span data-stu-id="1c218-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="1c218-181">Kopie *app\_offline.htm* im Projektmappenordner, den */Site / "Wwwroot"* Ordner auf der staging-Website.</span><span class="sxs-lookup"><span data-stu-id="1c218-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Kopieren Sie app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="1c218-183">Navigieren Sie zu Ihrer staging-Website-URL.</span><span class="sxs-lookup"><span data-stu-id="1c218-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="1c218-184">Sie sehen, dass die *app\_offline.htm* Seite wird jetzt anstatt Ihre Startseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1c218-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm im Browserfenster](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="1c218-186">Sie können nun in die Stagingumgebung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1c218-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="1c218-187">Bereitstellen des Code-Updates für Staging und Produktion</span><span class="sxs-lookup"><span data-stu-id="1c218-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="1c218-188">In der **Web eine klicken Sie auf Publish** Symbolleiste, wählen Sie die **Staging** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1c218-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="1c218-189">Visual Studio stellt die aktualisierte Anwendung bereit und öffnet den Browser, um die Homepage der Website.</span><span class="sxs-lookup"><span data-stu-id="1c218-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="1c218-190">Die *app\_offline.htm* Datei wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1c218-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="1c218-191">Sie testen können, um die erfolgreiche Bereitstellung zu überprüfen, müssen Sie Sie entfernen die *app\_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="1c218-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="1c218-192">In Ihrem FTP-Tool zurück, und löschen **app\_offline.htm** von der staging-Website.</span><span class="sxs-lookup"><span data-stu-id="1c218-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="1c218-193">Klicken Sie im Browser öffnen Sie die Seite Lehrkräfte in die Stagingsite, und wählen Sie einen Kursleiter, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="1c218-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="1c218-194">Halten Sie die gleiche Prozedur für die Produktion aus, wie Sie für die Stagingtabelle.</span><span class="sxs-lookup"><span data-stu-id="1c218-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="1c218-195">Überprüfen der Änderungen und Bereitstellen von Dateien</span><span class="sxs-lookup"><span data-stu-id="1c218-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="1c218-196">Visual Studio 2012 bietet Ihnen außerdem die Möglichkeit, einzelne Dateien bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="1c218-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="1c218-197">Für eine ausgewählte Datei können Sie Unterschiede zwischen der lokalen Version und der bereitgestellten Version anzeigen, die Datei in die zielumgebung bereitstellen oder kopieren Sie die Datei aus der zielumgebung zum lokalen Projekt.</span><span class="sxs-lookup"><span data-stu-id="1c218-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="1c218-198">In diesem Abschnitt des Lernprogramms finden Sie unter Verwendung dieser Funktionen.</span><span class="sxs-lookup"><span data-stu-id="1c218-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="1c218-199">Nehmen Sie eine Änderung bereitstellen</span><span class="sxs-lookup"><span data-stu-id="1c218-199">Make a change to deploy</span></span>

1. <span data-ttu-id="1c218-200">Open *Content/Site.css*, und suchen Sie den Block für die `body` Tag.</span><span class="sxs-lookup"><span data-stu-id="1c218-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="1c218-201">Ändern Sie den Wert für `background-color` aus `#fff` auf `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="1c218-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="1c218-202">Die Änderung im Vorschaufenster veröffentlichen anzeigen</span><span class="sxs-lookup"><span data-stu-id="1c218-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="1c218-203">Bei Verwendung der **Web veröffentlichen** Assistenten zum Veröffentlichen des Projekts können Sie sehen, welche Schritte durchlaufen Änderungen veröffentlicht werden, durch Doppelklicken auf die Datei in die **Vorschau** Fenster.</span><span class="sxs-lookup"><span data-stu-id="1c218-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="1c218-204">Mit der rechten Maustaste des ContosoUniversity-Projekts, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1c218-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="1c218-205">Aus der **Profil** Dropdown-Liste der **Test** Veröffentlichungsprofil.</span><span class="sxs-lookup"><span data-stu-id="1c218-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="1c218-206">Klicken Sie auf **Vorschau**, und klicken Sie dann auf **starten Preview**.</span><span class="sxs-lookup"><span data-stu-id="1c218-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="1c218-207">In der **Vorschau** Bereich doppelklicken Sie auf **"Site.CSS" ändern**.</span><span class="sxs-lookup"><span data-stu-id="1c218-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Doppelklicken Sie auf "Site.CSS" ändern](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="1c218-209">Die **Vorschau der Änderungen** Dialogfeld zeigt eine Vorschau der Änderungen, die bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1c218-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Vorschau der Änderungen an "Site.CSS" ändern](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="1c218-211">Doppelklicken Sie auf die *"Web.config"* Datei, die **Vorschau der Änderungen** Dialogfeld zeigt die Auswirkung des Builds Konfiguration Transformationen und Transformationen Profil veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1c218-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="1c218-212">An diesem Punkt Sie nicht getan haben, die dazu führen würde, dass die *"Web.config"* Datei auf dem Server zu ändern, sodass Sie erwarten, dass keine Änderungen sehen,.</span><span class="sxs-lookup"><span data-stu-id="1c218-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="1c218-213">Allerdings die **Vorschau der Änderungen** Fenster sind zwei Änderungen falsch dargestellt.</span><span class="sxs-lookup"><span data-stu-id="1c218-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="1c218-214">Anscheinend zwei XML-Elemente entfernt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="1c218-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="1c218-215">Diese Elemente werden durch die Veröffentlichung hinzugefügt, bei der Auswahl **führen Sie Code First-Migrationen beim Anwendungsstart** für ein Code First Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="1c218-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="1c218-216">Der Vergleich erfolgt, bevor der Veröffentlichungsprozess dieser Elemente hinzugefügt, damit sie scheinen sie entfernt werden, obwohl sie nicht entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="1c218-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="1c218-217">Dieser Fehler wird in einer zukünftigen Version behoben werden.</span><span class="sxs-lookup"><span data-stu-id="1c218-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="1c218-218">Klicken Sie auf **Schließen**.</span><span class="sxs-lookup"><span data-stu-id="1c218-218">Click **Close**.</span></span>
6. <span data-ttu-id="1c218-219">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1c218-219">Click **Publish**.</span></span>
7. <span data-ttu-id="1c218-220">Wenn der Browser an die Startseite der der Test-Website geöffnet wird, drücken Sie STRG + F5, um eine feste Aktualisierung verursachen, um die Auswirkung der Änderung CSS finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="1c218-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Auswirkung der Änderung von CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="1c218-222">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="1c218-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="1c218-223">Bestimmte Dateien im Projektmappen-Explorer veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="1c218-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="1c218-224">Angenommen Sie, Sie nicht wie die blauen Hintergrund und die ursprüngliche Farbe wiederherstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="1c218-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="1c218-225">In diesem Abschnitt müssen Sie die ursprünglichen Einstellungen wiederherstellen, indem Sie eine bestimmte Datei direkt aus veröffentlichen **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1c218-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="1c218-226">Open *Content/Site.css* und Wiederherstellen der `background-color` auf `#fff`.</span><span class="sxs-lookup"><span data-stu-id="1c218-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="1c218-227">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Content/Site.css* Datei.</span><span class="sxs-lookup"><span data-stu-id="1c218-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="1c218-228">Das Kontextmenü enthält drei Optionen veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1c218-228">The context menu shows three publish options.</span></span>

    ![Optionen im Projektmappen-Explorer veröffentlichen](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="1c218-230">Klicken Sie auf **Vorschau ändert sich in "Site.CSS" ändern**.</span><span class="sxs-lookup"><span data-stu-id="1c218-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="1c218-231">Ein Fenster wird geöffnet, um die Unterschiede zwischen der lokalen Datei und die Version des Zertifikats in der zielumgebung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="1c218-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Inhalt / "Site.CSS" ändern.](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="1c218-233">In **Projektmappen-Explorer**, mit der rechten Maustaste **"Site.CSS" ändern** erneut aus, und klicken Sie auf **veröffentlichen "Site.CSS" ändern**.</span><span class="sxs-lookup"><span data-stu-id="1c218-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="1c218-234">Die **Webaktivität veröffentlichen** Fenster angezeigt wird, dass die Datei veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="1c218-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Web veröffentlichen Aktivitätsfenster](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="1c218-236">Öffnen Sie einen Browser, um die `http://localhost/contosouniversity` URL, und drücken Sie STRG + F5, um dazu führen, dass eine harte aktualisiert, damit die Auswirkungen von den CSS-Code ändern, finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="1c218-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Startseite mit normalen CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="1c218-238">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="1c218-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="1c218-239">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="1c218-239">Summary</span></span>

<span data-ttu-id="1c218-240">Sie haben nun gesehen, mehrere Möglichkeiten, ein Anwendungsupdate bereitstellen, die keine Änderung an einer Datenbank beinhaltet, und haben Sie gesehen, wie die Änderungen aus, um sicherzustellen, dass was aktualisiert werden, was Sie erwarten wird in der Vorschau anzeigen.</span><span class="sxs-lookup"><span data-stu-id="1c218-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="1c218-241">Die Seite Lehrkräfte verfügt jetzt über eine **Kurse vermittelten** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="1c218-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Trainer-Seite mit Kurse vermittelten](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="1c218-243">Im nächste Lernprogramm, in dem Sie erfahren, wie Sie eine Änderung einer bereitstellen: Fügen Sie ein Feld "BirthDate" in der Datenbank und auf der Seite "Lehrkräfte".</span><span class="sxs-lookup"><span data-stu-id="1c218-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1c218-244">[Zurück](deploying-to-production.md)
[Weiter](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="1c218-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
