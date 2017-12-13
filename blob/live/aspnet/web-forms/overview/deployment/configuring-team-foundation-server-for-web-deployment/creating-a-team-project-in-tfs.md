---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Erstellen ein Teamprojekt in TFS | Microsoft Docs
author: jrjlee
description: Dieses Thema beschreibt die Erstellung ein neuen Teamprojekts in Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 4cb0d72330086ecb8cd9e6fb70ce0a57698dda5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="d8d23-103">Erstellen ein Teamprojekt in TFS</span><span class="sxs-lookup"><span data-stu-id="d8d23-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="d8d23-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d8d23-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d8d23-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="d8d23-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d8d23-106">Dieses Thema beschreibt die Erstellung ein neuen Teamprojekts in Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="d8d23-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="d8d23-107">Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="d8d23-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="d8d23-108">Übersicht über den Task</span><span class="sxs-lookup"><span data-stu-id="d8d23-108">Task Overview</span></span>

<span data-ttu-id="d8d23-109">Zum Bereitstellen und ein neues Teamprojekt in TFS verwenden, müssen Sie die folgenden allgemeinen Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="d8d23-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="d8d23-110">Erteilen Sie Berechtigungen für den Benutzer, der das neue Teamprojekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d8d23-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="d8d23-111">Erstellen Sie das Teamprojekt.</span><span class="sxs-lookup"><span data-stu-id="d8d23-111">Create the team project.</span></span>
- <span data-ttu-id="d8d23-112">Die Teammitglieder, die für das Projekt arbeiten, erteilen Sie Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="d8d23-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="d8d23-113">Checken Sie Teile des Inhalts.</span><span class="sxs-lookup"><span data-stu-id="d8d23-113">Check in some content.</span></span>

<span data-ttu-id="d8d23-114">In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen, und es identifiziert die Benutzer und Rollen Auftrag, die wahrscheinlich für jede Prozedur zuständig sind.</span><span class="sxs-lookup"><span data-stu-id="d8d23-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="d8d23-115">Denken Sie daran, dass abhängig von der Struktur Ihrer Organisation, jede dieser Aufgaben einer anderen Person verantwortlich sein kann.</span><span class="sxs-lookup"><span data-stu-id="d8d23-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="d8d23-116">Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie installiert und TFS konfiguriert haben und Sie eine Teamprojektsammlung im Rahmen des Konfigurationsvorgangs erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="d8d23-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="d8d23-117">Weitere Informationen zu diesen Annahmen und weitere allgemeine Hintergrundinformationen für dieses Szenario finden Sie unter [eine TFS-Build-Server für die Bereitstellung konfigurieren](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="d8d23-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="d8d23-118">Gewähren von Berechtigungen an den Teamprojektersteller</span><span class="sxs-lookup"><span data-stu-id="d8d23-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="d8d23-119">Um ein neues Teamprojekt erstellen, werden diese Berechtigungen erforderlich:</span><span class="sxs-lookup"><span data-stu-id="d8d23-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="d8d23-120">Sie benötigen die **neue Projekte erstellen** -Berechtigung für die TFS-Anwendungsebene.</span><span class="sxs-lookup"><span data-stu-id="d8d23-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="d8d23-121">Sie erteilen diese Berechtigung in der Regel durch Hinzufügen von Benutzern, die **Projektauflistungsadministratoren** TFS-Gruppe.</span><span class="sxs-lookup"><span data-stu-id="d8d23-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="d8d23-122">Die **Team Foundation-Administratoren** globale Gruppe enthält auch diese Berechtigung.</span><span class="sxs-lookup"><span data-stu-id="d8d23-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="d8d23-123">Sie benötigen die Berechtigung zum Erstellen neuer Team-Standorte innerhalb der SharePoint-Websitesammlung, die die TFS-Teamprojektsammlung entspricht.</span><span class="sxs-lookup"><span data-stu-id="d8d23-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="d8d23-124">Sie erteilen diese Berechtigung in der Regel durch Hinzufügen des Benutzers zu einer SharePoint-Gruppe mit **Vollzugriff** -Rechte für die SharePoint-websiteauflistung.</span><span class="sxs-lookup"><span data-stu-id="d8d23-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="d8d23-125">Wenn Sie SQL Server Reporting Services-Funktionen verwenden, muss Sie Mitglied der **Team Foundation-Inhalts-Manager** Rolle in Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="d8d23-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="d8d23-126">Wem diese Verfahren ausgeführt werden?</span><span class="sxs-lookup"><span data-stu-id="d8d23-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="d8d23-127">In der Regel führt die Person oder Gruppe, die die TFS-Bereitstellung verwaltet auch diese Verfahren aus.</span><span class="sxs-lookup"><span data-stu-id="d8d23-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="d8d23-128">Da dies einen mit weit reichenden Berechtigungen Satz von Berechtigungen ist, werden neue Teamprojekte mit Verantwortung für die Verwaltung einer TFS-Bereitstellung in der Regel durch eine kleine Teilmenge von Benutzern erstellt.</span><span class="sxs-lookup"><span data-stu-id="d8d23-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="d8d23-129">Entwicklern werden nicht in der Regel die Berechtigungen erforderlich, um die Erstellung neuer Teamprojekte gewährt werden.</span><span class="sxs-lookup"><span data-stu-id="d8d23-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="d8d23-130">Erteilen von Berechtigungen in TFS</span><span class="sxs-lookup"><span data-stu-id="d8d23-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="d8d23-131">Wenn Sie die Erstellung neuer Teamprojekte Benutzern ermöglichen möchten, ist die erste Aufgabe auf hoher Ebene beim Hinzufügen des Benutzers, der **Projektauflistungsadministratoren** Gruppe für die Teamprojektsammlung.</span><span class="sxs-lookup"><span data-stu-id="d8d23-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="d8d23-132">**Hinzufügen ein Benutzers zu der Gruppe "Projektauflistungsadministratoren" Projekt "**</span><span class="sxs-lookup"><span data-stu-id="d8d23-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="d8d23-133">Auf dem TFS-Server auf die **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft Team Foundation Server 2010**, und klicken Sie dann auf **Team Foundation -Verwaltungskonsole**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="d8d23-134">Erweitern Sie in der Strukturansicht des Navigationsbereichs **Logikschicht**, und klicken Sie dann auf **Teamprojektsammlungen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="d8d23-135">In der **Teamprojektsammlungen** klicken Sie im Bereich der Teamprojektsammlung, die Sie verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="d8d23-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="d8d23-136">Auf der **allgemeine** auf **Gruppenmitgliedschaft**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="d8d23-137">In der **globale Gruppen** wählen Sie im Dialogfeld die **Projektauflistungsadministratoren** Gruppe, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="d8d23-138">In der **Eigenschaften für die Team Foundation Server-Gruppe** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="d8d23-139">In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers aus, um neue Teamprojekte erstellen zu können. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="d8d23-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="d8d23-140">In der **Eigenschaften für die Team Foundation Server-Gruppe** (Dialogfeld), klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="d8d23-141">In der **globale Gruppen** (Dialogfeld), klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="d8d23-142">Erteilen von Berechtigungen in SharePointServices</span><span class="sxs-lookup"><span data-stu-id="d8d23-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="d8d23-143">Als Nächstes müssen Sie neue Teamwebsites in der SharePoint-Websitesammlung zu erstellen, die TFS-Teamprojektsammlung entsprechen, die Benutzerberechtigung geben.</span><span class="sxs-lookup"><span data-stu-id="d8d23-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="d8d23-144">**Erteilen von Vollzugriffsberechtigungen für die SharePoint-Websitesammlung**</span><span class="sxs-lookup"><span data-stu-id="d8d23-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="d8d23-145">In der Team Foundation Server-Verwaltungskonsole auf der **Teamprojektsammlungen** Seite, wählen Sie die Teamprojektsammlung, die Sie verwalten möchten.</span><span class="sxs-lookup"><span data-stu-id="d8d23-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="d8d23-146">Auf der **SharePoint-Website** Registerkarte, notieren Sie den Wert von der **aktuelle Standard Standort** URL.</span><span class="sxs-lookup"><span data-stu-id="d8d23-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="d8d23-147">Öffnen Sie Internet Explorer, und fahren Sie mit der URL, die Sie in Schritt 2 notiert haben.</span><span class="sxs-lookup"><span data-stu-id="d8d23-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d8d23-148">Wenn Sie nicht Windows als Benutzer, die die Teamprojektsammlung erstellt hat angemeldet sind, müssen Sie in SharePoint als dieser Benutzer anmelden, um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="d8d23-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="d8d23-149">Auf der **Websiteaktionen** Menü klicken Sie auf **Standorteinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="d8d23-150">Auf der **Standorteinstellungen** Seite **Benutzer und Berechtigungen**, klicken Sie auf **Personen und Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="d8d23-151">Klicken Sie in den linken Navigationsbereich auf **Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="d8d23-152">Auf der **Personen und Gruppen: alle Gruppen** auf **Gruppen einrichten für diesen Standort**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > <span data-ttu-id="d8d23-153">Möglicherweise erhalten Sie eine **HTTP 404 Not Found** Fehler aufgrund eines doppelten Codierung HTTP-Fehlers.</span><span class="sxs-lookup"><span data-stu-id="d8d23-153">You may receive an **HTTP 404 Not Found** error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="d8d23-154">In diesem Fall ersetzen Sie dabei die URL:</span><span class="sxs-lookup"><span data-stu-id="d8d23-154">If this occurs, replace the URL with this:</span></span>   
    > <span data-ttu-id="d8d23-155">[*URL Websitesammlung*] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="d8d23-155">[*site collection URL*]/\_layouts/permsetup.aspx</span></span>  
    > <span data-ttu-id="d8d23-156">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d8d23-156">For example:</span></span>  
    > <span data-ttu-id="d8d23-157">http://TFS/Sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="d8d23-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="d8d23-158">Auf der **Gruppen einrichten für diesen Standort** Seite, fügen Sie den Benutzer, die Teamprojekte erstellen, wird die **Besitzer** Gruppe, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="d8d23-159">Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8d23-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="d8d23-160">Ein neues Teamprojekt erstellen und Hinzufügen von Benutzern</span><span class="sxs-lookup"><span data-stu-id="d8d23-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="d8d23-161">Nachdem Sie die erforderlichen Berechtigungen haben, können Sie die **Team Explorer** Fenster in Visual Studio 2010 zum Erstellen eines neuen Teamprojekts.</span><span class="sxs-lookup"><span data-stu-id="d8d23-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="d8d23-162">Dieser Ansatz bietet einen Assistenten, der die erforderliche Informationen erfasst und führt die erforderlichen Aufgaben in TFS, SharePoint und SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="d8d23-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="d8d23-163">Sie müssen auch das Erteilen von Berechtigungen für das neue Teamprojekt Mitgliedern der Developer-Team, zu ermöglichen, hinzufügen und Ändern von Inhalt.</span><span class="sxs-lookup"><span data-stu-id="d8d23-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="d8d23-164">Wem diese Verfahren ausgeführt werden?</span><span class="sxs-lookup"><span data-stu-id="d8d23-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="d8d23-165">In der Regel führt eine TFS-Administrator oder ein Entwickler Teamleiter diese Verfahren.</span><span class="sxs-lookup"><span data-stu-id="d8d23-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="d8d23-166">Erstellen eines neuen Teamprojekts</span><span class="sxs-lookup"><span data-stu-id="d8d23-166">Create a New Team Project</span></span>

<span data-ttu-id="d8d23-167">Das nächste Verfahren beschreibt, wie ein neues Teamprojekt in TFS 2010 zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d8d23-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="d8d23-168">**Zum Erstellen eines neuen Teamprojekts**</span><span class="sxs-lookup"><span data-stu-id="d8d23-168">**To create a new team project**</span></span>

1. <span data-ttu-id="d8d23-169">Auf der **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, mit der rechten Maustaste **Microsoft Visual Studio 2010**, und klicken Sie dann auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d8d23-170">Wenn Sie nicht Visual Studio 2010 als Administrator ausführen, schlägt der Assistent für neue Teamprojekte im letzten Schritt fehl.</span><span class="sxs-lookup"><span data-stu-id="d8d23-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="d8d23-171">Wenn die **User Account Control** Dialogfeld angezeigt wird, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="d8d23-172">In Visual Studio auf die **Team** Menü klicken Sie auf **mit Team Foundation Server verbinden**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d8d23-173">Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 4 bis 7 weglassen.</span><span class="sxs-lookup"><span data-stu-id="d8d23-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="d8d23-174">In der **Verbindung mit Teamprojekt** (Dialogfeld), klicken Sie auf **Server**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="d8d23-175">In der **Team Foundation Server hinzufügen/entfernen** (Dialogfeld), klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="d8d23-176">In der **Team Foundation Server hinzufügen** (Dialogfeld), geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="d8d23-177">In der **Team Foundation Server hinzufügen/entfernen** (Dialogfeld), klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="d8d23-178">In der **Verbindung mit Teamprojekt herstellen** wählen Sie im Dialogfeld die TFS-Instanz, die Sie verbinden möchten, markieren Sie das Team möchten Sie verwenden möchten, hinzu, und klicken Sie dann auf projektauflistung **verbinden**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="d8d23-179">In der **Team Explorer** der rechten Maustaste auf die Teamprojektsammlung Sammlung, und klicken Sie dann auf **neues Teamprojekt**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="d8d23-180">In der **neues Teamprojekt** (Dialogfeld), geben Sie einen Namen und eine Beschreibung für das Teamprojekt, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d8d23-181">Wenn das Teamprojekt Leerzeichen enthält, können Sie einige Probleme stoßen, wenn Sie gelangen zu der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) zum Bereitstellen von Paketen aus den Ausgabepfad verwenden.</span><span class="sxs-lookup"><span data-stu-id="d8d23-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="d8d23-182">Leerzeichen im Pfad können viel schwieriger zu Web Deploy-Befehle ausführen erleichtern.</span><span class="sxs-lookup"><span data-stu-id="d8d23-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="d8d23-183">Auf der **Auswählen einer Prozessvorlage** Seite, wählen Sie die Prozessvorlage, die Sie verwenden, um den Entwicklungsprozess zu verwalten, und klicken Sie dann auf möchten **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d8d23-184">Weitere Informationen zu Prozessvorlagen für TFS, finden Sie unter [Prozessvorlagen und Tools](https://msdn.microsoft.com/en-us/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="d8d23-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/en-us/vstudio/aa718795).</span></span>
12. <span data-ttu-id="d8d23-185">Auf der **Team Standorteinstellungen** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="d8d23-186">Mit dieser Einstellung erstellt oder identifiziert wird, eine SharePoint-Teamwebsite, die die TFS-Teamprojekt zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="d8d23-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="d8d23-187">Ihr Entwicklungsteam können diese Website verwalten-Dokumentation, Diskussionsthemen teilnehmen, Wiki-Seiten erstellen und verschiedene andere Aufgaben ausführen, die nicht mit Code verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="d8d23-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="d8d23-188">Weitere Informationen finden Sie unter [Interaktionen zwischen SharePoint-Produkte und Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8d23-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="d8d23-189">Auf der **Quellcodeverwaltungseinstellungen geben** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="d8d23-190">Diese Einstellung identifiziert, oder erstellt den Speicherort in der Hierarchie der TFS-Ordner, die als einen Stammordner für Ihre Inhalte fungiert.</span><span class="sxs-lookup"><span data-stu-id="d8d23-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="d8d23-191">Auf der **Teamprojekteinstellungen bestätigen** auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="d8d23-192">Wenn das neue Teamprojekt wurde erfolgreich erstellt, auf die **Teamprojekt wurde erstellt** auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="d8d23-193">Hinzufügen von Benutzern zu einem Teamprojekt</span><span class="sxs-lookup"><span data-stu-id="d8d23-193">Add Users to a Team Project</span></span>

<span data-ttu-id="d8d23-194">Nun, dass Sie das neue Teamprojekt erstellt haben, können Sie für Benutzer, damit sie beginnen, hinzufügen und das gemeinsame Bearbeiten von Inhalt können Berechtigungen gewähren.</span><span class="sxs-lookup"><span data-stu-id="d8d23-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="d8d23-195">**Zum Hinzufügen von Benutzern zu einem Teamprojekt**</span><span class="sxs-lookup"><span data-stu-id="d8d23-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="d8d23-196">In Visual Studio 2010 in der **Team Explorer** Fenster mit der rechten Maustaste in des Teamprojekts, zeigen Sie auf **Teamprojekteinstellungen**, und klicken Sie dann auf **Gruppenmitgliedschaft**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="d8d23-197">Um einen Benutzer hinzufügen, ändern und Entfernen von Code in die quellcodeverwaltung zu aktivieren, müssen Sie ihn hinzufügen der **Contributors** Gruppe.</span><span class="sxs-lookup"><span data-stu-id="d8d23-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="d8d23-198">In der **Teamprojektgruppen** wählen Sie im Dialogfeld die **Contributors** Gruppe, und klicken Sie dann auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="d8d23-199">In der **Eigenschaften für die Team Foundation Server-Gruppe** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="d8d23-200">In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers mit dem Teamprojekt hinzufügen möchten. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="d8d23-201">In der **Eigenschaften für die Team Foundation Server-Gruppe** (Dialogfeld), klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="d8d23-202">In der **Teamprojektgruppen** (Dialogfeld), klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="d8d23-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d8d23-203">Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="d8d23-203">Conclusion</span></span>

<span data-ttu-id="d8d23-204">Zu diesem Zeitpunkt das neue Teamprojekt einsatzbereit ist, und Ihr Team Developer kann beginnen, Hinzufügen von Inhalt und Zusammenarbeit bei der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="d8d23-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="d8d23-205">Im nächsten Thema [Hinzufügen von Inhalt zur Quellcodeverwaltung](adding-content-to-source-control.md), beschreibt, wie Inhalt zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d8d23-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="d8d23-206">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="d8d23-206">Further Reading</span></span>

<span data-ttu-id="d8d23-207">Umfassendere Anleitung zum Erstellen von Teamprojekten in TFS finden Sie unter [Erstellen eines Teamprojekts](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="d8d23-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="d8d23-208">Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8d23-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span></span> <span data-ttu-id="d8d23-209">Weitere Informationen zum Hinzufügen von Benutzern zu Teamprojekten finden Sie unter [Hinzufügen von Benutzern zu Teamprojekten](https://msdn.microsoft.com/en-us/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8d23-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/en-us/library/bb558971.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d8d23-210">[Zurück](configuring-team-foundation-server-for-web-deployment.md)
[Weiter](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="d8d23-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
