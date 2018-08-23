---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Hinzufügen von Inhalten zur Quellcodeverwaltung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird erläutert, wie der quellcodeverwaltung in Team Foundation Server (TFS) 2010 Inhalt hinzugefügt wird. Es wird beschrieben, Hinzufügen von Projektmappen und Projekte zu einem Team-Projekt hinzufügen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 2119705a75d0717d05d4a7db69b3f5d38b1cdd45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833916"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="ccaf4-104">Hinzufügen von Inhalten zur Quellcodeverwaltung</span><span class="sxs-lookup"><span data-stu-id="ccaf4-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="ccaf4-105">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ccaf4-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ccaf4-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="ccaf4-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ccaf4-107">In diesem Thema wird erläutert, wie der quellcodeverwaltung in Team Foundation Server (TFS) 2010 Inhalt hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="ccaf4-108">Es beschreibt das Hinzufügen von Projektmappen und Projekte zu einem Teamprojekt in TFS, und es wird erläutert, wie externe Abhängigkeiten wie Frameworks oder Assemblys zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="ccaf4-109">In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="ccaf4-110">Übersicht über den Task</span><span class="sxs-lookup"><span data-stu-id="ccaf4-110">Task Overview</span></span>

<span data-ttu-id="ccaf4-111">In den meisten Fällen sollte jedes Mitglied des Entwicklerteams Inhalten zur quellcodeverwaltung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="ccaf4-112">Um eine Projektmappe zur quellcodeverwaltung in TFS hinzufügen möchten, müssen Sie die folgenden allgemeinen Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="ccaf4-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="ccaf4-113">Verbinden Sie mit einem Teamprojekt.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-113">Connect to a team project.</span></span>
- <span data-ttu-id="ccaf4-114">Ordnen Sie die Team Project-Ordnerstruktur auf dem Server, auf eine Ordnerstruktur auf dem lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="ccaf4-115">Die Lösung und seinen Inhalt zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="ccaf4-116">Externen Abhängigkeiten zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="ccaf4-117">In diesem Thema zeigt Sie, wie Sie diese Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="ccaf4-118">Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie ein neues Teamprojekt von TFS, um die Inhalte verwalten, bereits erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="ccaf4-119">Weitere Informationen zum Erstellen eines neuen Teamprojekts finden Sie unter [Erstellen eines Teamprojekts in TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="ccaf4-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="ccaf4-120">Wem diese Verfahren werden?</span><span class="sxs-lookup"><span data-stu-id="ccaf4-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="ccaf4-121">In den meisten Fällen sollte jedes Mitglied des Entwicklerteams möglicherweise hinzufügen und ändern die Inhalte in bestimmten Teamprojekten.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="ccaf4-122">Verbinden mit einem Teamprojekt und erstellen Sie eine Ordnerzuordnung</span><span class="sxs-lookup"><span data-stu-id="ccaf4-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="ccaf4-123">Bevor Sie alle Inhalte zur quellcodeverwaltung hinzufügen, müssen Sie eine Verbindung mit einem Teamprojekt, und erstellen Sie eine Zuordnung zwischen der Ordnerstruktur auf dem Server und dem Dateisystem auf dem lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="ccaf4-124">**Eine Verbindung mit einem Teamprojekt, und ordnen einen lokalen Pfad**</span><span class="sxs-lookup"><span data-stu-id="ccaf4-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="ccaf4-125">Öffnen Sie Visual Studio 2010, auf der Entwicklerarbeitsstation.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="ccaf4-126">In Visual Studio auf die **Team** Menü klicken Sie auf **Herstellen einer Verbindung mit Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ccaf4-127">Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 3 bis 6 auslassen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="ccaf4-128">In der **Verbindung mit Teamprojekt** Dialogfeld klicken Sie auf **Server**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="ccaf4-129">In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="ccaf4-130">In der **Team Foundation Server hinzufügen** Dialogfeld Geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="ccaf4-131">In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **schließen**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="ccaf4-132">In der **Herstellen einer Verbindung mit Teamprojekt** Dialogfeld Wählen Sie die TFS-Instanz, die Sie eine Verbindung herstellen möchten, markieren Sie das Team möchten project, Sammlung, wählen Sie das Teamprojekt, das Sie hinzufügen möchten, und klicken Sie dann auf **Connect**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="ccaf4-133">In der **Team Explorer** , erweitern Sie Ihr Teamprojekt, und doppelklicken Sie dann auf **Quellcodeverwaltung**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="ccaf4-134">Auf der **Quellcodeverwaltungs-Explorer** auf **nicht zugeordnet**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="ccaf4-135">In der **Zuordnung** Dialogfeld die **lokalen Ordner** Feld, navigieren Sie zu (oder erstellen) fungieren als Stammordner für das Teamprojekt, und klicken Sie auf einen lokalen Ordner **Zuordnung**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="ccaf4-136">Wenn Sie zum Herunterladen der Quelldateien aufgefordert werden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="ccaf4-137">An diesem Punkt haben Sie die serverseitigen Ordner für das Teamprojekt in einen lokalen Ordner auf der Entwicklerarbeitsstation zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="ccaf4-138">Sie haben außerdem jeglichen vorhandenen Inhalt aus dem Teamprojekt auf Ihrem lokalen Ordnerstruktur heruntergeladen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="ccaf4-139">Sie können jetzt starten, um Ihre eigenen Inhalte zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="ccaf4-140">Projekte und Projektmappen zur Quellcodeverwaltung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="ccaf4-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="ccaf4-141">Um Projekte und Projektmappen zur quellcodeverwaltung hinzufügen möchten, müssen Sie zunächst in den zugeordneten Ordner für das Teamprojekt auf dem lokalen Computer verschieben.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="ccaf4-142">Sie können Inhalte in die Erweiterungen mit dem Server synchronisiert. Klicken Sie dann überprüfen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="ccaf4-143">**Projekte zur quellcodeverwaltung hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="ccaf4-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="ccaf4-144">Verschieben Sie auf der Entwicklerarbeitsstation die Projekte und Projektmappen auf einen geeigneten Speicherort innerhalb der Struktur zugeordneten Ordner für das Teamprojekt aus.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ccaf4-145">Viele Organisationen haben einen bevorzugten Ansatz wie Projekte und Projektmappen in der quellcodeverwaltung angeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="ccaf4-146">Anleitungen zum Ordner der Struktur finden Sie unter [so wird's gemacht: Struktur der Quellcode-Verwaltungsordnern in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="ccaf4-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="ccaf4-147">Öffnen Sie die Projektmappe in Visual Studio 2010 ein.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="ccaf4-148">In der **Projektmappen-Explorer** rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **Projektmappe zur Quellcodeverwaltung hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="ccaf4-149">In einigen Fällen, je nachdem, wie Ihre Organisation zum Strukturieren von Inhalt in TFS, gerne ganz von vorne müssen Sie zum Hinzufügen von Projekten zur quellcodeverwaltung durchzugehen, um eine präzisere Kontrolle über den Aufbau von Quellcode bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="ccaf4-150">Überprüfen Sie, ob die **Quellcodeverwaltungs-Explorer** Registerkarte zeigt den Inhalt, die Sie in der Ordnerstruktur für das Teamprojekt hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="ccaf4-151">Die **Quellcodeverwaltungs-Explorer** Registerkarte zeigt Ihre Inhalte mit keine weiteren eingabeaufforderungen, da Sie einen zugeordneten Ordner auf dem lokalen Dateisystem Ihrer Projektmappe hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="ccaf4-152">Wenn Ihre Lösung an einem nicht zugeordneten Speicherort war, würden Sie an Speicherorte von Ordnern in TFS und Ihrem lokalen Dateisystem aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="ccaf4-153">Auf der **Quellcodeverwaltungs-Explorer** Registerkarte die **Ordner** Bereich mit der rechten Maustaste in des Teamprojekts (z. B. **ContactManager**), und klicken Sie dann auf **Einchecken Ausstehende Änderungen**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="ccaf4-154">In der **Einchecken – Quelldateien** Dialogfeld Geben Sie einen Kommentar, und klicken Sie dann auf **Einchecken**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="ccaf4-155">An diesem Punkt haben Sie die Projektmappe zur quellcodeverwaltung in TFS hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="ccaf4-156">Externe Abhängigkeiten zur Quellcodeverwaltung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="ccaf4-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="ccaf4-157">Wenn Sie ein Projekt oder eine Projektmappe zur quellcodeverwaltung hinzufügen, werden alle Dateien und Ordner in Ihrem Projekt oder Projektmappe auch hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="ccaf4-158">Allerdings basieren in vielen Fällen, Projekte und Projektmappen auch auf externe Abhängigkeiten, ebenso wie bei lokalen Assemblys ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="ccaf4-159">In diesem Fall müssen Sie eine hinzufügen, dass solche Ressourcen zur quellcodeverwaltung, Team Build und anderen Mitgliedern des Entwicklerteams können Ihren Code erfolgreich zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="ccaf4-160">Die Ordnerstruktur für die beispiellösung Contact Manager enthält beispielsweise einen Ordner namens Pakete.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="ccaf4-161">Die Assembly und verschiedene unterstützende Ressourcen enthält für das ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="ccaf4-162">Der Ordner "Pakete" ist nicht Teil der Contact Manager-Lösung, aber die Projektmappe ohne nicht erfolgreich erstellt.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="ccaf4-163">Um Team Build zum Erstellen der Projektmappe zu aktivieren, müssen Sie den Ordner "Pakete" zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="ccaf4-164">Die Einbeziehung von einem Ordner "Pakete" ist typisch für was geschieht, wenn Sie der Projektmappe mit der NuGet-Erweiterung für Visual Studio 2010 die Entity Framework oder ähnliche Ressourcen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="ccaf4-165">**Inhalt der nicht Teil des Projekts zur quellcodeverwaltung hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="ccaf4-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="ccaf4-166">Stellen Sie sicher, dass die Elemente, die Sie hinzufügen möchten (z. B. den Ordner "Pakete") in einen geeigneten Speicherort in einen zugeordneten Ordner auf Ihrem lokalen Dateisystem befinden.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="ccaf4-167">In Visual Studio 2010 In der **Team Explorer** , erweitern Sie Ihr Teamprojekt, und doppelklicken Sie dann auf **Quellcodeverwaltung**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="ccaf4-168">Auf der **Quellcodeverwaltungs-Explorer** Registerkarte die **Ordner** wählen Sie im Bereich der Ordner, der das Element enthält, oder Sie Elemente hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="ccaf4-169">Klicken Sie auf die **Elemente zu Ordner hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="ccaf4-170">In der **zur Quellcodeverwaltung hinzufügen** Dialogfeld wählen den Ordner oder die Elemente, die Sie hinzufügen möchten, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="ccaf4-171">Auf der **ausgeschlossene Elemente** Registerkarte, wählen Sie alle erforderlichen Elemente, die wurden (z. B. Assemblys) automatisch ausgeschlossen, und klicken Sie dann auf **Element(e) einschließen**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="ccaf4-172">Auf der **hinzuzufügenden Elemente** Registerkarte, stellen Sie sicher, dass alle Dateien, die Sie einschließen möchten, finden Sie, und klicken Sie dann auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="ccaf4-173">In der **Quellcodeverwaltungs-Explorer** Fenster, klicken Sie auf die **Einchecken** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="ccaf4-174">In der **Einchecken – Quelldateien** Dialogfeld Geben Sie einen Kommentar, und klicken Sie dann auf **Einchecken**.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="ccaf4-175">An diesem Punkt haben Sie die externen Abhängigkeiten für Ihre Projektmappe zur quellcodeverwaltung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ccaf4-176">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="ccaf4-176">Conclusion</span></span>

<span data-ttu-id="ccaf4-177">In diesem Thema beschrieben, wie Sie eine Verbindung mit einem Teamprojekt herstellen, ordnen eine Ordnerstruktur und Inhalten zur quellcodeverwaltung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="ccaf4-178">Weitere Informationen zum Arbeiten mit Elementen unter quellcodeverwaltung finden Sie unter [verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="ccaf4-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="ccaf4-179">Im nächsten Thema, [konfigurieren eine TFS-Build-Server für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md), beschreibt, wie Sie einen TFS Team Build-Server zum Erstellen und Bereitstellen Ihrer Lösung vorbereiten.</span><span class="sxs-lookup"><span data-stu-id="ccaf4-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="ccaf4-180">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="ccaf4-180">Further Reading</span></span>

<span data-ttu-id="ccaf4-181">Ausführlichere Informationen zum Arbeiten mit Datenquellen-Steuerelement in TFS finden Sie unter [verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="ccaf4-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ccaf4-182">[Zurück](creating-a-team-project-in-tfs.md)
> [Weiter](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="ccaf4-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
