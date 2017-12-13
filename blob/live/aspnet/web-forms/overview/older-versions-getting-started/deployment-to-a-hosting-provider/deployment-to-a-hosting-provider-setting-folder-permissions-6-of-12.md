---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Festlegen von Ordnerberechtigungen - 6 von 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 42085fff5f1aed1440f49e1e2ceee0cf0e751e2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a><span data-ttu-id="a5e17-103">Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Festlegen von Ordnerberechtigungen - 6 von 12</span><span class="sxs-lookup"><span data-stu-id="a5e17-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Setting Folder Permissions - 6 of 12</span></span>
====================
<span data-ttu-id="a5e17-104">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a5e17-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a5e17-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="a5e17-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="a5e17-106">Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält.</span><span class="sxs-lookup"><span data-stu-id="a5e17-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="a5e17-107">Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren.</span><span class="sxs-lookup"><span data-stu-id="a5e17-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="a5e17-108">Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a5e17-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="a5e17-109">Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a5e17-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a5e17-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a5e17-110">Overview</span></span>

<span data-ttu-id="a5e17-111">In diesem Lernprogramm legen Sie Berechtigungen für die *Elmah* Ordner in der bereitgestellten Web site, damit die Anwendung Protokolldateien in diesem Ordner erstellen kann.</span><span class="sxs-lookup"><span data-stu-id="a5e17-111">In this tutorial, you set folder permissions for the *Elmah* folder in the deployed web site so that the application can create log files in that folder.</span></span>

<span data-ttu-id="a5e17-112">Wenn Sie eine Webanwendung in Visual Studio mithilfe von Visual Studio Development Server (Cassini) testen, können Sie die Anwendung unter der Identität ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a5e17-112">When you test a web application in Visual Studio using the Visual Studio Development Server (Cassini), the application runs under your identity.</span></span> <span data-ttu-id="a5e17-113">Sie sind sehr wahrscheinlich ein Administrator auf dem Entwicklungscomputer und vollständige dann berechtigt, alle Dateien in einem beliebigen Ordner Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-113">You are most likely an administrator on your development computer and have full authority to do anything to any file in any folder.</span></span> <span data-ttu-id="a5e17-114">Wird jedoch eine Anwendung unter IIS ausgeführt wird, ausgeführt unter der Identität, die definiert, die für den Anwendungspool aus, dem der Standort zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="a5e17-114">But when an application runs under IIS, it runs under the identity defined for the application pool that the site is assigned to.</span></span> <span data-ttu-id="a5e17-115">Dies ist normalerweise eine systemdefinierte-Konto, das über beschränkte Berechtigungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="a5e17-115">This is typically a system-defined account that has limited permissions.</span></span> <span data-ttu-id="a5e17-116">Standardmäßig verfügt über Lese- und Ausführungsberechtigungen für Ihre Webanwendung Dateien und Ordner, jedoch keinen Schreibzugriff auf.</span><span class="sxs-lookup"><span data-stu-id="a5e17-116">By default it has read and execute permissions on your web application's files and folders, but it doesn't have write access.</span></span>

<span data-ttu-id="a5e17-117">Dies ist ein Problem, wenn die Anwendung erstellt oder Updates-Dateien, die ist eine häufige in Webanwendungen müssen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-117">This becomes an issue if your application creates or updates files, which is a common need in web applications.</span></span> <span data-ttu-id="a5e17-118">In der Anwendung Contoso University Elmah erstellt XML-Dateien in den *Elmah* Ordner, um Details zu Fehlern zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a5e17-118">In the Contoso University application, Elmah creates XML files in the *Elmah* folder in order to save details about errors.</span></span> <span data-ttu-id="a5e17-119">Auch wenn Sie etwa Elmah nicht verwenden, kann Ihre Website ermöglichen es Benutzern Hochladen von Dateien oder Durchführen weiterer Aufgaben, die Daten in einen Ordner auf Ihrer Website zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="a5e17-119">Even if you don't use something like Elmah, your site might let users upload files or perform other tasks that write data to a folder in your site.</span></span>

<span data-ttu-id="a5e17-120">Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a5e17-120">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="testing-error-logging-and-reporting"></a><span data-ttu-id="a5e17-121">Testen die Fehler, Protokollierung und-berichterstellung</span><span class="sxs-lookup"><span data-stu-id="a5e17-121">Testing Error Logging and Reporting</span></span>

<span data-ttu-id="a5e17-122">Um festzustellen, wie die Anwendung in IIS nicht richtig funktioniert (obwohl dies der Fall, wenn Sie ihn in Visual Studio getestet), Sie können dazu führen, dass einen Fehler, der normalerweise von Elmah protokolliert werden würde, und öffnen Sie das Elmah-Fehlerprotokoll, um die Details anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-122">To see how the application doesn't work correctly in IIS (although it did when you tested it in Visual Studio), you can cause an error that would normally be logged by Elmah, and then open the Elmah error log to see the details.</span></span> <span data-ttu-id="a5e17-123">Wenn Elmah konnte nicht zum Erstellen einer XML-Datei und speichern die Fehlerdetails, sehen Sie eine leere Fehlerbericht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-123">If Elmah was unable to create an XML file and store the error details, you see an empty error report.</span></span>

<span data-ttu-id="a5e17-124">Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, und fordern dann eine ungültige URL wie *Studentsxxx.aspx*.</span><span class="sxs-lookup"><span data-stu-id="a5e17-124">Open a browser and go to `http://localhost/ContosoUniversity`, and then request an invalid URL like *Studentsxxx.aspx*.</span></span> <span data-ttu-id="a5e17-125">Daraufhin eine vom System generierte Fehlerseite statt der *GenericErrorPage.aspx* Seite, da die `customErrors` Einstellung in der Datei "Web.config" ist "RemoteOnly", und Sie IIS lokal ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="a5e17-125">You see a system-generated error page instead of the *GenericErrorPage.aspx* page because the `customErrors` setting in the Web.config file is "RemoteOnly" and you are running IIS locally:</span></span>

<span data-ttu-id="a5e17-126">[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a5e17-126">[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)</span></span>

<span data-ttu-id="a5e17-127">Führen Sie nun *Elmah.axd* den Fehlerbericht anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-127">Now run *Elmah.axd* to see the error report.</span></span> <span data-ttu-id="a5e17-128">Eine leere Protokoll Fehlerseite wird angezeigt, da Elmah konnte nicht zum Erstellen einer XML-Datei in die *Elmah* Ordner:</span><span class="sxs-lookup"><span data-stu-id="a5e17-128">You see an empty error log page because Elmah was unable to create an XML file in the *Elmah* folder:</span></span>

<span data-ttu-id="a5e17-129">[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a5e17-129">[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)</span></span>

## <a name="setting-write-permission-on-the-elmah-folder"></a><span data-ttu-id="a5e17-130">Einstellung über die Schreibberechtigung für den Ordner Elmah</span><span class="sxs-lookup"><span data-stu-id="a5e17-130">Setting Write Permission on the Elmah Folder</span></span>

<span data-ttu-id="a5e17-131">Sie können Berechtigungen für Ordner manuell festlegen oder Sie können es automatisch im Rahmen des Bereitstellungsprozesses vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-131">You can set folder permissions manually or you can make it an automatic part of the deployment process.</span></span> <span data-ttu-id="a5e17-132">Somit automatische komplexen MSBuild-Code erfordert, und da Sie nur dazu erstmalig haben Sie bereitstellen, dieses Lernprogramm nur wird gezeigt, wie Sie dies manuell durchführen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-132">Making it automatic requires complex MSBuild code, and since you only have to do this the first time you deploy, this tutorial only shows how to do it manually.</span></span> <span data-ttu-id="a5e17-133">(Informationen dazu, wie in diesem Teil des Bereitstellungsprozesses vornehmen, finden Sie unter [Einstellung Ordnerberechtigungen auf Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi-Blog.)</span><span class="sxs-lookup"><span data-stu-id="a5e17-133">(For information about how to make this part of the deployment process, see [Setting Folder Permissions on Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) on Sayed Hashimi's blog.)</span></span>

<span data-ttu-id="a5e17-134">In **Windows­Explorer**, navigieren Sie zu *C:\inetpub\wwwroot\ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="a5e17-134">In **Windows Explorer**, navigate to *C:\inetpub\wwwroot\ContosoUniversity*.</span></span> <span data-ttu-id="a5e17-135">Mit der rechten Maustaste die *Elmah* Ordner wählen **Eigenschaften**, und wählen Sie dann die **Sicherheit** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="a5e17-135">Right-click the *Elmah* folder, select **Properties**, and then select the **Security** tab.</span></span>

<span data-ttu-id="a5e17-136">[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a5e17-136">[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)</span></span>

<span data-ttu-id="a5e17-137">(Wenn Sie nicht sehen **DefaultAppPool** in der **Gruppen-oder Benutzernamen** Liste Sie wahrscheinlich verwendet eine andere Methode als die in diesem Lernprogramm, um IIS und ASP.NET 4 auf Ihrem Computer einzurichten.</span><span class="sxs-lookup"><span data-stu-id="a5e17-137">(If you don't see **DefaultAppPool** in the **Group or user names** list, you probably used some other method than the one specified in this tutorial to set up IIS and ASP.NET 4 on your computer.</span></span> <span data-ttu-id="a5e17-138">In diesem Fall feststellen Sie, welche Identität vom University Contoso-Anwendung, und über die Schreibberechtigung erteilen dieser Identität zugewiesen, Anwendungspool verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a5e17-138">In that case, find out what identity is used by the application pool assigned to the Contoso University application, and grant write permission to that identity.</span></span> <span data-ttu-id="a5e17-139">Finden Sie unter den Links zur Anwendungspoolidentitäten am Ende dieses Lernprogramms.)</span><span class="sxs-lookup"><span data-stu-id="a5e17-139">See the links about application pool identities at the end of this tutorial.)</span></span>

<span data-ttu-id="a5e17-140">Klicken Sie auf **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="a5e17-140">Click **Edit**.</span></span> <span data-ttu-id="a5e17-141">In der **Berechtigungen für Elmah** wählen Sie im Dialogfeld **DefaultAppPool**, und wählen Sie dann die **schreiben** Kontrollkästchen in der **zulassen** Spalte.</span><span class="sxs-lookup"><span data-stu-id="a5e17-141">In the **Permissions for Elmah** dialog box, select **DefaultAppPool**, and then select the **Write** check box in the **Allow** column.</span></span>

<span data-ttu-id="a5e17-142">[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a5e17-142">[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)</span></span>

<span data-ttu-id="a5e17-143">Klicken Sie auf **OK** in beiden Dialogfeldern.</span><span class="sxs-lookup"><span data-stu-id="a5e17-143">Click **OK** in both dialog boxes.</span></span>

## <a name="retesting-error-logging-and-reporting"></a><span data-ttu-id="a5e17-144">Ein erneuter Test Fehler Protokollierung und-berichterstellung</span><span class="sxs-lookup"><span data-stu-id="a5e17-144">Retesting Error Logging and Reporting</span></span>

<span data-ttu-id="a5e17-145">Testen, indem Sie verursacht einen Fehler erneut auf die gleiche Weise (eine ungültige URL-Anforderung), und führen Sie die **Fehlerprotokoll** Seite.</span><span class="sxs-lookup"><span data-stu-id="a5e17-145">Test by causing an error again in the same way (request a bad URL) and run the **Error Log** page.</span></span> <span data-ttu-id="a5e17-146">Dieses Mal wird der Fehler auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a5e17-146">This time the error appears on the page.</span></span>

<span data-ttu-id="a5e17-147">[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a5e17-147">[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)</span></span>

<span data-ttu-id="a5e17-148">Sie benötigen Berechtigungen auch schreiben, auf die *App\_Daten* Ordner daran, dass Sie SQL Server Compact-Datenbankdateien in diesem Ordner verfügen und Sie zum Aktualisieren von Daten in diesen Datenbanken können möchten.</span><span class="sxs-lookup"><span data-stu-id="a5e17-148">You also need write permission on the *App\_Data* folder because you have SQL Server Compact database files in that folder, and you want to be able to update data in those databases.</span></span> <span data-ttu-id="a5e17-149">In diesem Fall Sie haben jedoch keinen zusätzlichen nichts zu tun, da des Bereitstellungsprozesses automatisch über die Schreibberechtigung für setzt die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="a5e17-149">In that case, however, you don't have to do anything extra because the deployment process automatically sets write permission on the *App\_Data* folder.</span></span>

<span data-ttu-id="a5e17-150">Sie haben jetzt alle Aufgaben müssen Sie Contoso University abgeschlossen in IIS auf dem lokalen Computer ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="a5e17-150">You have now completed all of the tasks necessary to get Contoso University working correctly in IIS on your local computer.</span></span> <span data-ttu-id="a5e17-151">In den nächsten Lernprogrammen werden Sie die Website öffentlich verfügbar machen, indem sie in einem Hostinganbieter bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="a5e17-151">In the next tutorial, you will make the site publicly available by deploying it to a hosting provider.</span></span>

## <a name="more-information"></a><span data-ttu-id="a5e17-152">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="a5e17-152">More Information</span></span>

<span data-ttu-id="a5e17-153">In diesem Beispiel wurde der Grund, warum Elmah Log-Dateien können nicht gespeichert wurde, relativ offensichtlich.</span><span class="sxs-lookup"><span data-stu-id="a5e17-153">In this example, the reason why Elmah was unable to save log files was fairly obvious.</span></span> <span data-ttu-id="a5e17-154">Sie können IIS-Protokollierung in Fällen verwenden, wo die Ursache des Problems nicht so offensichtlich ist. finden Sie unter [Troubleshooting Failed Anforderungen Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der Website IIS.net.</span><span class="sxs-lookup"><span data-stu-id="a5e17-154">You can use IIS tracing in cases where the cause of the problem is not so obvious; see [Troubleshooting Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) on the IIS.net site.</span></span>

<span data-ttu-id="a5e17-155">Weitere Informationen über das Gewähren von Berechtigungen für die Identitäten des dienstanwendungspools finden Sie unter [Anwendungspoolidentitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sicherer Inhalt in IIS über Dateisystem-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der Website IIS.net.</span><span class="sxs-lookup"><span data-stu-id="a5e17-155">For more information about how to grant permissions to application pool identities, see [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) and [Secure Content in IIS Through File System ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) on the IIS.net site.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a5e17-156">[Zurück](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[Weiter](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="a5e17-156">[Previous](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)</span></span>