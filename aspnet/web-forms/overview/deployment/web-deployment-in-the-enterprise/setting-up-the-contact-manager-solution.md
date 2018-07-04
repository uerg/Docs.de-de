---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Einrichten der Contact Manager-Lösung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie zum Herunterladen und konfigurieren Sie die Projektmappe Contact Manager lokal auf einer Entwicklerarbeitsstation ausgeführt wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: a7675a793909ec4d95164ee47a3a43f73600c5bc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366688"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="27d32-103">Einrichten der Contact Manager-Lösung</span><span class="sxs-lookup"><span data-stu-id="27d32-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="27d32-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="27d32-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="27d32-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="27d32-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="27d32-106">In diesem Thema wird beschrieben, wie zum Herunterladen und konfigurieren Sie die Projektmappe Contact Manager lokal auf einer Entwicklerarbeitsstation ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="27d32-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="27d32-107">Systemanforderungen</span><span class="sxs-lookup"><span data-stu-id="27d32-107">System Requirements</span></span>

<span data-ttu-id="27d32-108">Contact Manager-Lösung lokal ausführen und die anderen in diesem Lernprogramm beschriebene Aufgaben ausführen, müssen Sie zum Installieren der Software auf der Entwicklerarbeitsstation:</span><span class="sxs-lookup"><span data-stu-id="27d32-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="27d32-109">Visual Studio 2010 Service Pack 1, Premium oder Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="27d32-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="27d32-110">Internetinformationsdienste (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="27d32-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="27d32-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="27d32-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="27d32-112">IIS-Webbereitstellungstool (Web Deploy) 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="27d32-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="27d32-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="27d32-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="27d32-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="27d32-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="27d32-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="27d32-115">.NET Framework 4</span></span>
- <span data-ttu-id="27d32-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="27d32-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="27d32-117">Mit Ausnahme von Visual Studio 2010 können Sie herunterladen und installieren Sie die neuesten Versionen der Produkte und Komponenten über die [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="27d32-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="27d32-118">Herunterladen Sie und extrahieren Sie die Lösung</span><span class="sxs-lookup"><span data-stu-id="27d32-118">Download and Extract the Solution</span></span>

<span data-ttu-id="27d32-119">Sie können die beispielanwendung Contact Manager aus der MSDN Code Gallery herunterladen [hier](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="27d32-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="27d32-120">Konfigurieren und Ausführen der Lösung</span><span class="sxs-lookup"><span data-stu-id="27d32-120">Configure and Run the Solution</span></span>

<span data-ttu-id="27d32-121">Zum Konfigurieren und Contact Manager-Lösung auf Ihrem lokalen Computer ausführen, müssen Sie diese allgemeinen Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="27d32-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="27d32-122">Wenn Sie eine noch nicht, erstellen Sie eine lokale Datenbank für ASP.NET-Anwendungsdienste die Mitgliedschafts- und Rollendatenbanken Management-Features aktiviert.</span><span class="sxs-lookup"><span data-stu-id="27d32-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="27d32-123">Bearbeiten von Verbindungszeichenfolgen in der *"Web.config"* Dateien, um auf Ihre lokale SQL Server Express-Instanz zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="27d32-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="27d32-124">Führen Sie die Projektmappe von Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="27d32-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="27d32-125">Der übrige Teil dieses Abschnitts enthält weitere Anleitungen dazu, wie diese Aufgaben ausführen.</span><span class="sxs-lookup"><span data-stu-id="27d32-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="27d32-126">**Zum Erstellen der Anwendung-Services-Datenbank**</span><span class="sxs-lookup"><span data-stu-id="27d32-126">**To create the application services database**</span></span>

1. <span data-ttu-id="27d32-127">Öffnen Sie eine Visual Studio 2010-Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="27d32-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="27d32-128">Zu diesem Zweck die **starten** , zeigen Sie auf **Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, klicken Sie auf **Visual Studio-Tools**, und klicken Sie dann Klicken Sie auf **Visual Studio-Eingabeaufforderung (2010)**.</span><span class="sxs-lookup"><span data-stu-id="27d32-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="27d32-129">Geben Sie folgenden Befehl an der Eingabeaufforderung, und drücken Sie dann die EINGABETASTE:</span><span class="sxs-lookup"><span data-stu-id="27d32-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="27d32-130">Verwenden der **-c** verwenden, um die Verbindungszeichenfolge für Ihren Datenbankserver anzugeben.</span><span class="sxs-lookup"><span data-stu-id="27d32-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="27d32-131">Verwenden der **– ein** verwenden, um anzugeben, die Anwendung services-Funktionen, die Sie in der Datenbank hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="27d32-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="27d32-132">In diesem Fall **m** gibt an, dass Sie zum Hinzufügen der Unterstützung für den Mitgliedschaftsanbieter und **r** gibt an, dass die Unterstützung für die Rollen-Manager hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="27d32-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="27d32-133">Verwenden der **– d** verwenden, um einen Namen für die Anwendung-Services-Datenbank anzugeben.</span><span class="sxs-lookup"><span data-stu-id="27d32-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="27d32-134">Wenn Sie diese Option weglassen, erstellt das Dienstprogramm eine Datenbank mit dem Standardnamen der **Aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="27d32-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="27d32-135">Wenn die Datenbank wurde erfolgreich erstellt wurde, wird der Eingabeaufforderung eine Bestätigung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="27d32-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="27d32-136">Weitere Informationen zu den Aspnet\_Regsql Hilfsprogramm finden Sie unter [ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="27d32-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="27d32-137">Der nächste Schritt ist, um sicherzustellen, dass die Verbindungszeichenfolgen in der Contact Manager-Lösung für Ihre lokale Instanz von SQL Server Express zeigen.</span><span class="sxs-lookup"><span data-stu-id="27d32-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="27d32-138">**Um die Verbindungszeichenfolgen zu aktualisieren.**</span><span class="sxs-lookup"><span data-stu-id="27d32-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="27d32-139">Öffnen Sie die Projektmappe Contact Manager, klicken Sie in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="27d32-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="27d32-140">In der **Projektmappen-Explorer** Fenster, erweitern Sie die **ContactManager.Mvc** Projekt, und doppelklicken Sie dann auf die **"Web.config"** Knoten.</span><span class="sxs-lookup"><span data-stu-id="27d32-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27d32-141">Das ContactManager.Mvc-Projekt enthält zwei *"Web.config"* Dateien.</span><span class="sxs-lookup"><span data-stu-id="27d32-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="27d32-142">Sie müssen die Datei auf Projektebene bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="27d32-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="27d32-143">In der **ConnectionStrings** -Element, stellen Sie sicher, dass die Verbindungszeichenfolge mit dem Namen **ApplicationServices** verweist auf Ihre lokale Datenbank für ASP.NET-Anwendungsdienste.</span><span class="sxs-lookup"><span data-stu-id="27d32-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="27d32-144">In der **Projektmappen-Explorer** Fenster, erweitern Sie die **ContactManager.Service** Projekt, und doppelklicken Sie dann auf die **"Web.config"** Knoten.</span><span class="sxs-lookup"><span data-stu-id="27d32-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="27d32-145">In der **ConnectionStrings** Elements in der Verbindungszeichenfolge, die mit dem Namen **ContactManagerContext**, überprüfen Sie, ob die **Datenquelle** -Eigenschaftensatz auf Ihre lokale Instanz von SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="27d32-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="27d32-146">Sie müssen nicht alles in der Verbindungszeichenfolge ändern.</span><span class="sxs-lookup"><span data-stu-id="27d32-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="27d32-147">Speichern Sie alle geöffneten Dateien.</span><span class="sxs-lookup"><span data-stu-id="27d32-147">Save all open files.</span></span>

<span data-ttu-id="27d32-148">Sie sollten jetzt Contact Manager-Lösung auf Ihrem lokalen Computer ausführen können.</span><span class="sxs-lookup"><span data-stu-id="27d32-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="27d32-149">Wenn Sie folgende Schritte ausführen, ohne zuvor eine Datenbank, erstellt ASP.NET die Datenbank erstmals Sie versuchen, einen Benutzer zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="27d32-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="27d32-150">Allerdings haben Sie die Datenbank manuell erstellen Sie noch viel mehr Kontrolle über die Anwendung Dienste Funktionsgruppe, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="27d32-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="27d32-151">**Die Projektmappe Contact Manager ausführen**</span><span class="sxs-lookup"><span data-stu-id="27d32-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="27d32-152">Drücken Sie F5, klicken Sie in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="27d32-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="27d32-153">Internet Explorer startet und die URL der Anwendung wenden Sie sich an Manager ASP.NET MVC 3-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="27d32-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="27d32-154">Standardmäßig zeigt die Anwendung die **alle Kontakte** Seite.</span><span class="sxs-lookup"><span data-stu-id="27d32-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="27d32-155">Fügen Sie einige Kontakte, und stellen Sie sicher, dass die Anwendung wie erwartet funktioniert.</span><span class="sxs-lookup"><span data-stu-id="27d32-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="27d32-156">Navigieren Sie zu `http://localhost:50114/Account/Register` (passen Sie die URL aus, wenn Sie die Anwendung auf einen anderen Port hosten).</span><span class="sxs-lookup"><span data-stu-id="27d32-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="27d32-157">Fügen Sie einen Benutzernamen, e-Mail-Adresse und Kennwort hinzu, und stellen Sie sicher, dass Sie ein Konto erfolgreich registrieren können.</span><span class="sxs-lookup"><span data-stu-id="27d32-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="27d32-158">Navigieren Sie zu `http://localhost:50114/Account/LogOn` (passen Sie die URL aus, wenn Sie die Anwendung auf einen anderen Port hosten).</span><span class="sxs-lookup"><span data-stu-id="27d32-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="27d32-159">Stellen Sie sicher, dass Sie mit dem soeben erstellten Konto anmelden können.</span><span class="sxs-lookup"><span data-stu-id="27d32-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="27d32-160">Schließen Sie Internet Explorer zum Beenden des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="27d32-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="27d32-161">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="27d32-161">Conclusion</span></span>

<span data-ttu-id="27d32-162">An diesem Punkt sollte Contact Manager-Lösung vollständig konfiguriert werden, führen Sie auf dem lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="27d32-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="27d32-163">Sie können die Lösung als Verweis verwenden, wenn Sie über die anderen Themen in diesem Tutorial arbeiten.</span><span class="sxs-lookup"><span data-stu-id="27d32-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="27d32-164">Im nächsten Thema, [Grundlegendes zur Projektdatei](understanding-the-project-file.md), wird erläutert, wie Sie die benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien in der Contact Manager-Lösung verwenden können, um den Bereitstellungsprozess zu steuern.</span><span class="sxs-lookup"><span data-stu-id="27d32-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="27d32-165">[Zurück](the-contact-manager-solution.md)
> [Weiter](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="27d32-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
