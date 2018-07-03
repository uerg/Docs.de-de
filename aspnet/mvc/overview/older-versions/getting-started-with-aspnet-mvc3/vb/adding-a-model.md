---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Hinzufügen eines Modells (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: fb5620921aa2575e7336661b61bb6d1f4afa4517
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389097"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="8b04d-103">Hinzufügen eines Modells (VB)</span><span class="sxs-lookup"><span data-stu-id="8b04d-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="8b04d-104">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8b04d-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="8b04d-105">In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b04d-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="8b04d-106">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="8b04d-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="8b04d-107">Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="8b04d-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="8b04d-108">Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:</span><span class="sxs-lookup"><span data-stu-id="8b04d-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="8b04d-109">Visual Studio Web Developer Express SP1-Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="8b04d-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="8b04d-110">ASP.NET MVC 3 Toolsupdate</span><span class="sxs-lookup"><span data-stu-id="8b04d-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="8b04d-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)</span><span class="sxs-lookup"><span data-stu-id="8b04d-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="8b04d-112">Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="8b04d-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="8b04d-113">Ein Visual Web Developer-Projekt mit Quellcode VB.NET steht zur Ergänzung dieses Themas.</span><span class="sxs-lookup"><span data-stu-id="8b04d-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="8b04d-114">[Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="8b04d-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="8b04d-115">Wenn Sie C# -Code bevorzugen, wechseln Sie zu der [c#-Version](../cs/adding-a-model.md) in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="8b04d-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="8b04d-116">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="8b04d-116">Adding a Model</span></span>

<span data-ttu-id="8b04d-117">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8b04d-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="8b04d-118">Diese Klassen werden der "Model" Teil der ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8b04d-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="8b04d-119">Verwenden Sie eine .NET Framework-Datenzugriff-Technologie als Entity Framework bezeichnet definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="8b04d-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="8b04d-120">Der Entity Framework (häufig als EF bezeichnet) unterstützt, wird, ein Paradigma für die Entwicklung aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="8b04d-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="8b04d-121">Code zuerst können Sie Modellobjekte zu erstellen, indem einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="8b04d-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="8b04d-122">(Diese werden auch bezeichnet als POCO-Klassen, von "Plain-Old CLR Objects" ein.) Sie können dann die Datenbank erstellt haben, im laufenden Betrieb von Klassen, die einen Entwicklungsworkflow sehr übersichtlich und eine schnelle ermöglicht haben.</span><span class="sxs-lookup"><span data-stu-id="8b04d-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="8b04d-123">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="8b04d-123">Adding Model Classes</span></span>

<span data-ttu-id="8b04d-124">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="8b04d-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="8b04d-125">Nennen Sie die Klasse "Movie".</span><span class="sxs-lookup"><span data-stu-id="8b04d-125">Name the class "Movie".</span></span>

<span data-ttu-id="8b04d-126">Fügen Sie die folgenden fünf Eigenschaften, die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="8b04d-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="8b04d-127">Wir verwenden die `Movie` Klasse zur Darstellung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8b04d-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="8b04d-128">Jede Instanz eine `Movie` eine Zeile in einer Datenbanktabelle, und jede Eigenschaft des Objekts entspricht der `Movie` Klasse wird an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="8b04d-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="8b04d-129">Fügen Sie in der gleichen Datei Folgendes `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="8b04d-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="8b04d-130">Die `MovieDBContext` Klasse darstellt, die Entity Framework filmdatenbankkontext, die verarbeitet werden, abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8b04d-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="8b04d-131">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8b04d-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="8b04d-132">Weitere Informationen zu `DbContext` und `DbSet`, finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="8b04d-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="8b04d-133">Damit Sie verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `imports` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="8b04d-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="8b04d-134">Die vollständige *Movie.vb* Datei ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="8b04d-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="8b04d-135">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8b04d-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="8b04d-136">Die `MovieDBContext` erstellten Klasse übernimmt die Aufgabe der Verbindung zur Datenbank und Zuordnung `Movie` Objekte mit Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="8b04d-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="8b04d-137">Eine Frage, die Sie wahrscheinlich gebeten, die ist jedoch, nach welcher Datenbank geben sie eine Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="8b04d-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="8b04d-138">Sie müssen dies durch das Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8b04d-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="8b04d-139">Öffnen Sie den Stammordner der Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="8b04d-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="8b04d-140">(Nicht die *"Web.config"* Datei die *Ansichten* Ordner.) Die folgende Abbildung beides anzeigen *"Web.config"* Dateien; öffnen die *"Web.config"* Datei rot markiert.</span><span class="sxs-lookup"><span data-stu-id="8b04d-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="8b04d-141">Fügen Sie die folgende Verbindungszeichenfolge für die `<connectionStrings>` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="8b04d-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="8b04d-142">Das folgende Beispiel zeigt einen Teil der *"Web.config"* -Datei mit der neuen Verbindungszeichenfolge hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="8b04d-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="8b04d-143">Diese geringe Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="8b04d-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="8b04d-144">Als Nächstes erstellen Sie ein neues `MoviesController` -Klasse, die Sie verwenden können, die Filmdaten angezeigt und können Benutzer neue Film Auflistungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8b04d-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8b04d-145">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="8b04d-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
