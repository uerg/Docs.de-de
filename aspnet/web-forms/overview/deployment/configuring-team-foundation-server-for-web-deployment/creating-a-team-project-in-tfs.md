---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Erstellen eines Teamprojekts in TFS | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellen.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 98725b9d2f669e6520f24c3a8122be086002e96a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810223"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="63910-103">Erstellen eines Teamprojekts in TFS</span><span class="sxs-lookup"><span data-stu-id="63910-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="63910-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="63910-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="63910-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="63910-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="63910-106">In diesem Thema wird beschrieben, wie Sie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellen.</span><span class="sxs-lookup"><span data-stu-id="63910-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="63910-107">In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="63910-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="63910-108">Übersicht über den Task</span><span class="sxs-lookup"><span data-stu-id="63910-108">Task Overview</span></span>

<span data-ttu-id="63910-109">Für das Bereitstellen und ein neues Teamprojekt in TFS verwenden, müssen Sie die folgenden allgemeinen Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="63910-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="63910-110">Erteilen Sie Berechtigungen für den Benutzer, der das neue Teamprojekt erstellen sollen.</span><span class="sxs-lookup"><span data-stu-id="63910-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="63910-111">Erstellen Sie das Teamprojekt.</span><span class="sxs-lookup"><span data-stu-id="63910-111">Create the team project.</span></span>
- <span data-ttu-id="63910-112">Gewähren von Berechtigungen Sie für die Teammitglieder, die auf das Projekt zu funktionieren.</span><span class="sxs-lookup"><span data-stu-id="63910-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="63910-113">Überprüfen Sie in Teil des Inhalts.</span><span class="sxs-lookup"><span data-stu-id="63910-113">Check in some content.</span></span>

<span data-ttu-id="63910-114">In diesem Thema wird gezeigt, wie Sie diese Schritte ausführen, und es erkennt, die Benutzer und die Auftrags-Rollen, die wahrscheinlich für jede Prozedur verantwortlich sind.</span><span class="sxs-lookup"><span data-stu-id="63910-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="63910-115">Denken Sie daran, dass Sie abhängig von der Struktur Ihrer Organisation, diese Aufgaben die Verantwortung für eine andere Person sein kann.</span><span class="sxs-lookup"><span data-stu-id="63910-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="63910-116">Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, die Sie installiert und konfiguriert TFS, und Sie eine Teamprojektsammlung im Rahmen des Konfigurationsvorgangs erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="63910-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="63910-117">Weitere Informationen zu diesen Annahmen und weitere allgemeine Informationen zu dem Szenario, finden Sie unter [konfigurieren Sie eine TFS-Build-Server für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="63910-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="63910-118">Gewähren von Berechtigungen für den Teamprojektersteller</span><span class="sxs-lookup"><span data-stu-id="63910-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="63910-119">Um ein neues Teamprojekt erstellen zu können, benötigen Sie diese Berechtigungen:</span><span class="sxs-lookup"><span data-stu-id="63910-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="63910-120">Sie benötigen die **neue Projekte erstellen** -Berechtigung für die TFS-Anwendungsebene.</span><span class="sxs-lookup"><span data-stu-id="63910-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="63910-121">Sie erteilen diese Berechtigung in der Regel durch Hinzufügen von Benutzern, die **Projektauflistungsadministratoren** TFS-Gruppe.</span><span class="sxs-lookup"><span data-stu-id="63910-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="63910-122">Die **Team Foundation-Administratoren** globale Gruppe enthält auch diese Berechtigung.</span><span class="sxs-lookup"><span data-stu-id="63910-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="63910-123">Sie benötigen die Berechtigung zum Erstellen neuer Team-Standorte innerhalb der SharePoint-Websitesammlung, die die TFS-Teamprojektsammlung entspricht.</span><span class="sxs-lookup"><span data-stu-id="63910-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="63910-124">Sie erteilen diese Berechtigung in der Regel durch Hinzufügen des Benutzers zu einer SharePoint-Gruppe mit **Vollzugriff** Rechte auf dem SharePoint-websiteauflistung.</span><span class="sxs-lookup"><span data-stu-id="63910-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="63910-125">Wenn Sie SQL Server Reporting Services-Funktionen verwenden, müssen Sie Mitglied werden die **Team Foundation-Inhalts-Manager** Rolle in Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="63910-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="63910-126">Wem diese Verfahren werden?</span><span class="sxs-lookup"><span data-stu-id="63910-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="63910-127">In der Regel führt die Person oder Gruppe, die die TFS-Bereitstellung verwaltet auch diese Verfahren aus.</span><span class="sxs-lookup"><span data-stu-id="63910-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="63910-128">Da dies einen weitreichenden Satz von Berechtigungen ist, werden neue Teamprojekte in der Regel von einer kleinen Gruppe von Benutzern mit Verantwortung für die Verwaltung einer TFS-Bereitstellung erstellt.</span><span class="sxs-lookup"><span data-stu-id="63910-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="63910-129">Entwickler nicht in der Regel die erforderlichen Berechtigungen zum Erstellen neuer Teamprojekte erhält.</span><span class="sxs-lookup"><span data-stu-id="63910-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="63910-130">Erteilen von Berechtigungen in TFS</span><span class="sxs-lookup"><span data-stu-id="63910-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="63910-131">Wenn Sie einen Benutzer zum Erstellen neuer Teamprojekte aktivieren möchten, ist die allgemeine erste Aufgabe beim Hinzufügen des Benutzers, der **Projektauflistungsadministratoren** für die Teamprojektsammlung.</span><span class="sxs-lookup"><span data-stu-id="63910-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="63910-132">**Einen Benutzer der Gruppe Administratoren der Projektsammlung hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="63910-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="63910-133">Auf dem TFS-Server auf die **starten** , zeigen Sie auf **Programme**, klicken Sie auf **Microsoft Team Foundation Server 2010**, und klicken Sie dann auf **Team Foundation -Verwaltungskonsole**.</span><span class="sxs-lookup"><span data-stu-id="63910-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="63910-134">Erweitern Sie in der Strukturansicht Navigation **Anwendungsebene**, und klicken Sie dann auf **Team Project Collections**.</span><span class="sxs-lookup"><span data-stu-id="63910-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="63910-135">In der **Team Project Collections** wählen Sie im Bereich der teamprojektauflistung, die Sie verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="63910-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="63910-136">Auf der **allgemeine** auf **Gruppenmitgliedschaft**.</span><span class="sxs-lookup"><span data-stu-id="63910-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="63910-137">In der **globale Gruppen** wählen Sie im Dialogfeld die **Projektauflistungsadministratoren** gruppieren, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="63910-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="63910-138">In der **Team Foundation Server-Gruppeneigenschaften** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="63910-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="63910-139">In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers neue Teamprojekte erstellt werden sollen. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="63910-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="63910-140">In der **Team Foundation Server-Gruppeneigenschaften** Dialogfeld klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="63910-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="63910-141">In der **globale Gruppen** Dialogfeld klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="63910-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="63910-142">Erteilen von Berechtigungen in SharePointServices</span><span class="sxs-lookup"><span data-stu-id="63910-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="63910-143">Als Nächstes müssen Sie neue Teamwebsites in der SharePoint-Websitesammlung zu erstellen, die die TFS-Teamprojektsammlung entspricht, die Benutzer die Berechtigung gewähren.</span><span class="sxs-lookup"><span data-stu-id="63910-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="63910-144">**Erteilen von Vollzugriffsberechtigungen für die SharePoint-Websitesammlung**</span><span class="sxs-lookup"><span data-stu-id="63910-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="63910-145">In der Team Foundation Server-Verwaltungskonsole auf der **Team Project Collections** Seite, wählen Sie die Teamprojektsammlung, die Sie verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="63910-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="63910-146">Auf der **SharePoint-Website** Registerkarte, beachten Sie den Wert der **aktueller Standardort der Website** URL.</span><span class="sxs-lookup"><span data-stu-id="63910-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="63910-147">Öffnen Sie Internet Explorer, und fahren Sie mit der URL, die Sie in Schritt 2 notiert haben.</span><span class="sxs-lookup"><span data-stu-id="63910-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63910-148">Wenn Sie nicht auf Windows als Benutzer, die die Teamprojektsammlung erstellt hat angemeldet sind, müssen Sie in SharePoint als dieser Benutzer melden Sie sich um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="63910-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="63910-149">Auf der **Websiteaktionen** Menü klicken Sie auf **Standorteinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="63910-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="63910-150">Auf der **Standorteinstellungen** Seite **Benutzern und Berechtigungen**, klicken Sie auf **Personen und Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="63910-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="63910-151">Klicken Sie im linken Bereich auf **Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="63910-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="63910-152">Auf der **Personen und Gruppen: alle Gruppen** auf **Gruppen einrichten für diesen Standort**.</span><span class="sxs-lookup"><span data-stu-id="63910-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="63910-153">Erhalten Sie möglicherweise eine <strong>HTTP 404 Not Found</strong> Fehler aufgrund eines doppelten Codierung HTTP-Fehlers.</span><span class="sxs-lookup"><span data-stu-id="63910-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="63910-154">In diesem Fall ersetzen Sie dabei die URL:</span><span class="sxs-lookup"><span data-stu-id="63910-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="63910-155">`[site_collection_URL]/_layouts/permsetup.aspx` Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="63910-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="63910-156">Auf der **Gruppen einrichten für diesen Standort** Seite fügen Sie den Benutzer, die Teamprojekte zu erstellen, wird die **Besitzer** gruppieren, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="63910-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="63910-157">Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="63910-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="63910-158">Ein neues Teamprojekt erstellen und Hinzufügen von Benutzern</span><span class="sxs-lookup"><span data-stu-id="63910-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="63910-159">Nachdem Sie die erforderlichen Berechtigungen verfügen, können Sie mithilfe der **Team Explorer** Fenster in Visual Studio 2010 zum Erstellen eines neuen Teamprojekts.</span><span class="sxs-lookup"><span data-stu-id="63910-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="63910-160">Dieser Ansatz bietet einen Assistenten, der alle erforderlichen Informationen erfasst und führt die erforderlichen Aufgaben in TFS, SharePoint und SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="63910-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="63910-161">Sie müssen auch zum Gewähren von Berechtigungen für das neue Teamprojekt für Mitglied des Entwicklerteams, aktivieren sie zum Hinzufügen und Ändern des Inhalts.</span><span class="sxs-lookup"><span data-stu-id="63910-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="63910-162">Wem diese Verfahren werden?</span><span class="sxs-lookup"><span data-stu-id="63910-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="63910-163">In der Regel führt ein TFS-Administrator oder Teamleiter sind Entwickler diese Verfahren aus.</span><span class="sxs-lookup"><span data-stu-id="63910-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="63910-164">Erstellen Sie ein neues Teamprojekt</span><span class="sxs-lookup"><span data-stu-id="63910-164">Create a New Team Project</span></span>

<span data-ttu-id="63910-165">Im nächste Verfahren wird beschrieben, erstellen Sie ein neues Teamprojekt in TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="63910-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="63910-166">**Um ein neues Teamprojekt zu erstellen.**</span><span class="sxs-lookup"><span data-stu-id="63910-166">**To create a new team project**</span></span>

1. <span data-ttu-id="63910-167">Auf der **starten** Startmenü **Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, mit der rechten Maustaste **Microsoft Visual Studio 2010**, und klicken Sie dann auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="63910-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63910-168">Wenn Sie nicht Visual Studio 2010 als Administrator ausführen, schlägt der Assistent für neue Teamprojekte im letzten Schritt fehl.</span><span class="sxs-lookup"><span data-stu-id="63910-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="63910-169">Wenn die **User Account Control** klicken Sie im angezeigten Dialogfeld **Ja**.</span><span class="sxs-lookup"><span data-stu-id="63910-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="63910-170">In Visual Studio auf die **Team** Menü klicken Sie auf **Herstellen einer Verbindung mit Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="63910-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63910-171">Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 4 bis 7 weglassen.</span><span class="sxs-lookup"><span data-stu-id="63910-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="63910-172">In der **Verbindung mit Teamprojekt** Dialogfeld klicken Sie auf **Server**.</span><span class="sxs-lookup"><span data-stu-id="63910-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="63910-173">In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="63910-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="63910-174">In der **Team Foundation Server hinzufügen** Dialogfeld Geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="63910-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="63910-175">In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="63910-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="63910-176">In der **Herstellen einer Verbindung mit Teamprojekt** wählen Sie im Dialogfeld die TFS-Instanz, die Sie eine Verbindung herstellen, wählen Sie das Team möchten die projektauflistung zu hinzu, und klicken Sie dann auf **Connect**.</span><span class="sxs-lookup"><span data-stu-id="63910-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="63910-177">In der **Team Explorer** der rechten Maustaste auf das Team die Projektsammlung, und klicken Sie dann auf **neues Teamprojekt**.</span><span class="sxs-lookup"><span data-stu-id="63910-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="63910-178">In der **neues Teamprojekt** Dialogfeld Geben Sie einen Namen und eine Beschreibung für das Teamprojekt, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="63910-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63910-179">Wenn Ihr Teamprojekt Leerzeichen enthält, können Sie einige Probleme auftreten, auf das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) verwenden Sie zum Bereitstellen von Paketen aus den Ausgabepfad.</span><span class="sxs-lookup"><span data-stu-id="63910-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="63910-180">Leerzeichen im Pfad können viel zum Ausführen von Befehlen mit Web Deploy erschweren.</span><span class="sxs-lookup"><span data-stu-id="63910-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="63910-181">Auf der **Auswählen einer Prozessvorlage** Seite, wählen Sie eine Prozessvorlage, die Sie verwenden, um den Entwicklungsprozess zu verwalten, und klicken Sie dann auf möchten **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="63910-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63910-182">Weitere Informationen zu Prozessvorlagen für TFS finden Sie unter [Prozessvorlagen und Tools](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="63910-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="63910-183">Auf der **Teamwebsiteeinstellungen** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="63910-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="63910-184">Mit dieser Einstellung erstellt oder identifiziert wird, eine SharePoint-Teamwebsite, die mit dem TFS-Teamprojekt zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="63910-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="63910-185">Ihr Entwicklungsteam können diese Website verwalten Dokumentation, Diskussionsthemen teilnehmen, Wiki-Seiten erstellen und verschiedene andere Aufgaben, die nicht mit Code verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="63910-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="63910-186">Weitere Informationen finden Sie unter [Interaktionen zwischen SharePoint-Produkte und Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="63910-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="63910-187">Auf der **Quellcode-Verwaltungseinstellungen geben** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="63910-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="63910-188">Diese Einstellung identifiziert, oder die Position in der TFS-Ordnerhierarchie, die als einen Stammordner für Ihre Inhalte fungieren wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="63910-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="63910-189">Auf der **Teamprojekteinstellungen bestätigen** auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="63910-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="63910-190">Wenn das neue Teamprojekt wird erfolgreich erstellt, auf die **Teamprojekt wurde erstellt** auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="63910-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="63910-191">Hinzufügen von Benutzern zu einem Teamprojekt</span><span class="sxs-lookup"><span data-stu-id="63910-191">Add Users to a Team Project</span></span>

<span data-ttu-id="63910-192">Nun, dass Sie das neue Teamprojekt erstellt haben, können Sie Berechtigungen für Benutzer, damit sie beginnen, hinzufügen und die Zusammenarbeit an Inhalten können gewähren.</span><span class="sxs-lookup"><span data-stu-id="63910-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="63910-193">**Hinzufügen von Benutzern zu einem Teamprojekt**</span><span class="sxs-lookup"><span data-stu-id="63910-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="63910-194">In Visual Studio 2010 in der **Team Explorer** Fenster mit der rechten Maustaste in des Teamprojekts, zeigen Sie auf **Teamprojekteinstellungen**, und klicken Sie dann auf **Gruppenmitgliedschaft**.</span><span class="sxs-lookup"><span data-stu-id="63910-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="63910-195">Um einen Benutzer hinzufügen, ändern und Entfernen von Code in der quellcodeverwaltung zu aktivieren, müssen Sie ihn hinzufügen der **Mitwirkende** Gruppe.</span><span class="sxs-lookup"><span data-stu-id="63910-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="63910-196">In der **Projektgruppen** wählen Sie im Dialogfeld die **Mitwirkende** gruppieren, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="63910-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="63910-197">In der **Team Foundation Server-Gruppeneigenschaften** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="63910-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="63910-198">In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers mit dem Teamprojekt hinzufügen möchten. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="63910-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="63910-199">In der **Team Foundation Server-Gruppeneigenschaften** Dialogfeld klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="63910-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="63910-200">In der **Projektgruppen** Dialogfeld klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="63910-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="63910-201">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="63910-201">Conclusion</span></span>

<span data-ttu-id="63910-202">An diesem Punkt das neue Teamprojekt ist einsatzbereit, und Ihr Entwicklerteam kann beginnen, Hinzufügen von Inhalt und die Zusammenarbeit bei der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="63910-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="63910-203">Im nächsten Thema, [Inhalten zur Quellcodeverwaltung hinzufügen](adding-content-to-source-control.md), beschreibt, wie Sie Inhalte zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="63910-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="63910-204">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="63910-204">Further Reading</span></span>

<span data-ttu-id="63910-205">Umfassendere Anleitung zum Erstellen von Teamprojekten in TFS finden Sie [Erstellen eines Teamprojekts](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="63910-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="63910-206">Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="63910-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="63910-207">Weitere Informationen zum Hinzufügen von Benutzern zu Teamprojekten finden Sie unter [Hinzufügen von Benutzern zu Teamprojekten](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="63910-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63910-208">[Zurück](configuring-team-foundation-server-for-web-deployment.md)
> [Weiter](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="63910-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
