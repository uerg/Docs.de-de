---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer SQL Server-Datenbankupdate - 11 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: aeec69c7373a111d30e8f32a374a9f02fb4c080a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="a80a5-103">Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer SQL Server-Datenbankupdate - 11 12</span><span class="sxs-lookup"><span data-stu-id="a80a5-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="a80a5-104">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a80a5-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a80a5-105">Startprojekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="a80a5-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="a80a5-106">Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält.</span><span class="sxs-lookup"><span data-stu-id="a80a5-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="a80a5-107">Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren.</span><span class="sxs-lookup"><span data-stu-id="a80a5-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="a80a5-108">Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a80a5-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="a80a5-109">Ein Lernprogramm, das zeigt Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt und wird gezeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact wird gezeigt, wie Windows Azure-Websites bereitstellen, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a80a5-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a80a5-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a80a5-110">Overview</span></span>

<span data-ttu-id="a80a5-111">In diesem Lernprogramm wird gezeigt, wie eine Aktualisierung der Datenbank auf einer vollständigen SQL Server-Datenbank bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="a80a5-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="a80a5-112">Da Code First-Migrationen auf die gesamte Arbeit aktualisieren die Datenbank verfügt, ist der Prozess nahezu identisch mit was Sie für SQL Server Compact in haben der [Bereitstellen eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="a80a5-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="a80a5-113">Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a80a5-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="a80a5-114">Hinzufügen einer neuen Spalte zu einer Tabelle</span><span class="sxs-lookup"><span data-stu-id="a80a5-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="a80a5-115">In diesem Abschnitt des Lernprogramms treffen Sie eine Datenbank ändern und entsprechende Änderungen am Code, klicken Sie dann in Visual Studio als Vorbereitung für das Bereitstellen in den Test- und produktionsumgebungen Umgebungen testen.</span><span class="sxs-lookup"><span data-stu-id="a80a5-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="a80a5-116">Die Änderung umfasst das Hinzufügen einer `OfficeHours` Spalte die `Instructor` Entität und die neuen Informationen in den **Lehrkräfte** Webseite.</span><span class="sxs-lookup"><span data-stu-id="a80a5-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="a80a5-117">Öffnen Sie im Projekt ContosoUniversity.DAL *Instructor.cs* und fügen Sie die folgende Eigenschaft zwischen den `HireDate` und `Courses` Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="a80a5-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="a80a5-118">Aktualisieren Sie die Initialisiererklasse, sodass er die neue Spalte mit Testdaten Ausgangswerte.</span><span class="sxs-lookup"><span data-stu-id="a80a5-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="a80a5-119">Open *Migrations\Configuration.cs* , und Ersetzen Sie den Codeblock, der beginnt, `var instructors = new List<Instructor>` mit den folgenden Codeblock enthält die neue Spalte:</span><span class="sxs-lookup"><span data-stu-id="a80a5-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="a80a5-120">Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und fügen Sie ein neues Vorlagenfeld für Geschäftszeiten unmittelbar vor dem schließenden `</Columns>` Tag in der ersten `GridView` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="a80a5-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="a80a5-121">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="a80a5-121">Build the solution.</span></span>

<span data-ttu-id="a80a5-122">Öffnen der **Package Manager Console** Fenster, und wählen ContosoUniversity.DAL als die **Standardprojekt**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="a80a5-123">Geben Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a80a5-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="a80a5-124">Führen Sie die Anwendung, und wählen Sie die **Lehrkräfte** Seite.</span><span class="sxs-lookup"><span data-stu-id="a80a5-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="a80a5-125">Dauert es etwas länger als üblich, geladen werden, da das Entity Framework die Datenbank neu erstellt und ihn mit Testdaten startet, die Seite.</span><span class="sxs-lookup"><span data-stu-id="a80a5-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="a80a5-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a80a5-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="a80a5-127">Das Datenbankupdate bereitstellen, in der Testumgebung</span><span class="sxs-lookup"><span data-stu-id="a80a5-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="a80a5-128">Wenn Sie Code First-Migrationen verwenden, ist die Methode für die Bereitstellung der Änderung an einer Datenbank mit SQL Server genauso wie bei SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="a80a5-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="a80a5-129">Sie müssen jedoch den Test ändern Veröffentlichungsprofil, da er weiterhin so eingerichtet ist, von SQL Server Compact zu SQL Server zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="a80a5-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="a80a5-130">Der erste Schritt ist die Verbindung Zeichenfolge Transformationen zu entfernen, die Sie im vorherigen Lernprogramm erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="a80a5-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="a80a5-131">Diese sind nicht mehr erforderlich, da Verbindung Zeichenfolge Transformationen im Veröffentlichungsprofil, angeben müssen, wie Sie ausgeführt haben, bevor Sie konfiguriert die **SQL packen/veröffentlichen** Registerkarte für die Migration zu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a80a5-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="a80a5-132">Öffnen der *Web.Test.config* Datei, und entfernen Sie die `connectionStrings` Element.</span><span class="sxs-lookup"><span data-stu-id="a80a5-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="a80a5-133">Die einzige verbleibende Transformation in der *Web.Test.config* -Datei ist für die `Environment` Wert in der `appSettings` Element.</span><span class="sxs-lookup"><span data-stu-id="a80a5-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="a80a5-134">Jetzt können Sie das Veröffentlichungsprofil und Veröffentlichen in der testumgebung aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a80a5-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="a80a5-135">Öffnen der **Web veröffentlichen** -Assistenten, und wechseln Sie dann auf die **Profil** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="a80a5-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="a80a5-136">Wählen Sie die **Test** Veröffentlichungsprofil.</span><span class="sxs-lookup"><span data-stu-id="a80a5-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="a80a5-137">Wählen Sie die **Einstellungen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="a80a5-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="a80a5-138">Klicken Sie auf **aktivieren Sie die neue Datenbank, die publishing Verbesserungen**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="a80a5-139">In das Feld die Verbindungszeichenfolge für **SchoolContext**, geben Sie den gleichen Wert, der in verwendet das *Web.Test.config* Transformationsdatei im vorherigen Lernprogramm:</span><span class="sxs-lookup"><span data-stu-id="a80a5-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="a80a5-140">Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="a80a5-141">(In Ihrer Version von Visual Studio, können Sie das Kontrollkästchen Beschriftung **gelten Code First-Migrationen**.)</span><span class="sxs-lookup"><span data-stu-id="a80a5-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="a80a5-142">In das Feld die Verbindungszeichenfolge für **DefaultConnection**, geben Sie den gleichen Wert, der in verwendet das *Web.Test.config* Transformationsdatei im vorherigen Lernprogramm:</span><span class="sxs-lookup"><span data-stu-id="a80a5-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="a80a5-143">Lassen Sie **Datenbank aktualisieren** deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="a80a5-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="a80a5-144">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-144">Click **Publish**.</span></span>

<span data-ttu-id="a80a5-145">Visual Studio stellt die codeänderungen in der testumgebung und öffnet den Browser, um die Startseite des Contoso-Universität.</span><span class="sxs-lookup"><span data-stu-id="a80a5-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="a80a5-146">Wählen Sie die Seite "Lehrkräfte".</span><span class="sxs-lookup"><span data-stu-id="a80a5-146">Select the Instructors page.</span></span>

<span data-ttu-id="a80a5-147">Wenn die Anwendung auf dieser Seite ausgeführt wird, wird versucht, Zugriff auf die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a80a5-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="a80a5-148">Code First-Migrationen wird überprüft, ob die Datenbank aktuell und feststellt, dass die neueste Migration noch nicht angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="a80a5-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="a80a5-149">Code First-Migrationen die letzte Migration gilt, führt die `Seed` -Methode, und klicken Sie dann auf die Seite normal ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a80a5-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="a80a5-150">Sie finden die neue Geschäftszeiten-Spalte mit den per Seeding hinzugefügte Daten.</span><span class="sxs-lookup"><span data-stu-id="a80a5-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="a80a5-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a80a5-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="a80a5-152">Das Datenbankupdate Bereitstellen in der Produktionsumgebung.</span><span class="sxs-lookup"><span data-stu-id="a80a5-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="a80a5-153">Sie müssen auch das Veröffentlichungsprofil für die produktionsumgebung zu ändern.</span><span class="sxs-lookup"><span data-stu-id="a80a5-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="a80a5-154">In diesem Fall müssen Sie das vorhandene Profil entfernen und ein neues erstellen, indem Sie eine aktualisierte PUBLISHSETTINGS-Datei importieren.</span><span class="sxs-lookup"><span data-stu-id="a80a5-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="a80a5-155">Die aktualisierte Datei wird die Verbindungszeichenfolge für die SQL Server-Datenbank am Cytanium enthalten.</span><span class="sxs-lookup"><span data-stu-id="a80a5-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="a80a5-156">Wie Sie gesehen haben, wenn Sie in der testumgebung bereitgestellt haben, nicht mehr benötigte Verbindung Zeichenfolge Transformationen in der *Web.Production.config* Transformationsdatei.</span><span class="sxs-lookup"><span data-stu-id="a80a5-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="a80a5-157">Öffnen, die Datei, und Entfernen der `connectionStrings` Element.</span><span class="sxs-lookup"><span data-stu-id="a80a5-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="a80a5-158">Die verbleibenden-Transformationen sind für die `Environment` Wert in der `appSettings` Element und die `location` -Element, das Zugriff auf die Fehlerberichte Elmah beschränkt.</span><span class="sxs-lookup"><span data-stu-id="a80a5-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="a80a5-159">Vor der Erstellung eines neues Veröffentlichungsprofils für die Produktion herunterladen eine aktualisierten PUBLISHSETTINGS-Datei die gleiche Weise wie Sie zuvor in war der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="a80a5-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="a80a5-160">(Klicken Sie in der Systemsteuerung Cytanium auf **Websites**, und klicken Sie dann auf die **contosouniversity.com** Website.</span><span class="sxs-lookup"><span data-stu-id="a80a5-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="a80a5-161">Wählen Sie die **Webveröffentlichung** Registerkarte, und klicken Sie dann auf **Veröffentlichungsprofil herunterladen, für diese Website**.) Der Grund dafür ist, damit die Datenbank-Verbindungszeichenfolge in der PUBLISHSETTINGS-Datei übernommen.</span><span class="sxs-lookup"><span data-stu-id="a80a5-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="a80a5-162">Die Verbindungszeichenfolge nicht beim ersten Verwenden Sie die Datei heruntergeladen haben, da weiterhin von SQL Server Compact mithilfe wurden und SQL Server-Datenbank am Cytanium hadn't noch erstellt.</span><span class="sxs-lookup"><span data-stu-id="a80a5-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="a80a5-163">Jetzt können Sie das Veröffentlichungsprofil und Veröffentlichen in der produktionsumgebung aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a80a5-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="a80a5-164">Öffnen der **Web veröffentlichen** -Assistenten, und wechseln Sie dann auf die **Profil** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="a80a5-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="a80a5-165">Klicken Sie auf **Profile verwalten**, und klicken Sie dann das Produktions-Profil löschen.</span><span class="sxs-lookup"><span data-stu-id="a80a5-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="a80a5-166">Schließen der **Web veröffentlichen** Assistenten, um diese Änderung zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a80a5-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="a80a5-167">Öffnen der **Web veröffentlichen** Assistenten erneut aus, und klicken Sie dann auf **Import**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="a80a5-168">Auf der **Verbindung** Registerkarte **Ziel-URL** auf den entsprechenden Wert, wenn Sie eine temporäre URL verwenden.</span><span class="sxs-lookup"><span data-stu-id="a80a5-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="a80a5-169">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-169">Click **Next**.</span></span>

<span data-ttu-id="a80a5-170">Auf der **Einstellungen** auf **aktivieren Sie die neue Datenbank, die publishing Verbesserungen**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="a80a5-171">In der Verbindung Zeichenfolge Dropdown-Liste für **SchoolContext**, wählen Sie die Verbindungszeichenfolge Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a80a5-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="a80a5-173">Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="a80a5-174">In der Verbindung Zeichenfolge Dropdown-Liste für **DefaultConnection**, wählen Sie die Verbindungszeichenfolge Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a80a5-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="a80a5-175">Wählen Sie die **Profil** auf **Profile verwalten**, und benennen Sie das Profil aus "contosouniversity.com - Web Deploy" in "Produktion".</span><span class="sxs-lookup"><span data-stu-id="a80a5-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="a80a5-176">Schließen Sie das Veröffentlichungsprofil, um die Änderung zu speichern, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="a80a5-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="a80a5-177">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="a80a5-177">Click **Publish**.</span></span> <span data-ttu-id="a80a5-178">(Für eine echte Produktionswebsite, kopieren Sie *app\_offline.htm* in der Produktions- und Put es in den Projektordner vor der Veröffentlichung, entfernen Sie ihn dann bei der Bereitstellung abgeschlossen ist.)</span><span class="sxs-lookup"><span data-stu-id="a80a5-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="a80a5-179">Visual Studio stellt die codeänderungen in der testumgebung und öffnet den Browser, um die Startseite des Contoso-Universität.</span><span class="sxs-lookup"><span data-stu-id="a80a5-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="a80a5-180">Wählen Sie die Seite "Lehrkräfte".</span><span class="sxs-lookup"><span data-stu-id="a80a5-180">Select the Instructors page.</span></span>

<span data-ttu-id="a80a5-181">Code First-Migrationen aktualisiert die Datenbank die gleiche Weise, wie in der testumgebung.</span><span class="sxs-lookup"><span data-stu-id="a80a5-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="a80a5-182">Sie finden die neue Geschäftszeiten-Spalte mit den per Seeding hinzugefügte Daten.</span><span class="sxs-lookup"><span data-stu-id="a80a5-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="a80a5-184">Sie haben jetzt erfolgreich bereitgestellt eine Anwendung aktualisieren, die eine datenbankänderung enthalten mithilfe einer SQL Server-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a80a5-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="a80a5-185">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="a80a5-185">More Information</span></span>

<span data-ttu-id="a80a5-186">Dies schließt diese Reihe von Lernprogrammen zum Bereitstellen einer ASP.NET-WEBANWENDUNG zwischen einem Hostinganbieter eines Drittanbieters.</span><span class="sxs-lookup"><span data-stu-id="a80a5-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="a80a5-187">Weitere Informationen zu den Themen in diesen Lernprogrammen behandelt, finden Sie unter der [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="a80a5-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="a80a5-188">Bestätigungen</span><span class="sxs-lookup"><span data-stu-id="a80a5-188">Acknowledgements</span></span>

<span data-ttu-id="a80a5-189">Ich möchte die vielen Dank, dass die folgenden Personen, die bedeutende Beiträge auf den Inhalt dieser Reihe von Lernprogrammen vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="a80a5-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="a80a5-190">Alberto Poblacion, MVP &amp; MCT, Spanien</span><span class="sxs-lookup"><span data-stu-id="a80a5-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="a80a5-191">Jarod Ferguson, Plattform die Entwicklung von Data-MVP, USA</span><span class="sxs-lookup"><span data-stu-id="a80a5-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="a80a5-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a80a5-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="a80a5-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a80a5-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="a80a5-194">Mike PPPoE, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a80a5-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="a80a5-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a80a5-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="a80a5-196">Raffaele Rialdi, Italien</span><span class="sxs-lookup"><span data-stu-id="a80a5-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="a80a5-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a80a5-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="a80a5-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="a80a5-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="a80a5-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="a80a5-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="a80a5-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="a80a5-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="a80a5-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="a80a5-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="a80a5-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="a80a5-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a80a5-203">[Zurück](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Weiter](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="a80a5-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
