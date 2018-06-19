---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Hinzufügen eines Modells (c#) | Microsoft Docs
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879347"
---
<a name="adding-a-model-c"></a><span data-ttu-id="4f623-104">Hinzufügen eines Modells (c#)</span><span class="sxs-lookup"><span data-stu-id="4f623-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="4f623-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4f623-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="4f623-106">In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f623-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="4f623-107">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="4f623-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="4f623-108">Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="4f623-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="4f623-109">Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:</span><span class="sxs-lookup"><span data-stu-id="4f623-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="4f623-110">Visual Studio Web Developer Express SP1-Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="4f623-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="4f623-111">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="4f623-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="4f623-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)</span><span class="sxs-lookup"><span data-stu-id="4f623-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="4f623-113">Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="4f623-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="4f623-114">Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="4f623-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="4f623-115">[Die C#-Version herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="4f623-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="4f623-116">Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/adding-a-model.md) dieses Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="4f623-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="4f623-117">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="4f623-117">Adding a Model</span></span>

<span data-ttu-id="4f623-118">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4f623-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="4f623-119">Diese Klassen werden die "" Modellteil der ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4f623-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="4f623-120">Verwenden Sie eine .NET Framework Datenzugriffs-Technologie, die das Entity Framework genannt definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="4f623-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="4f623-121">Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="4f623-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="4f623-122">Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="4f623-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="4f623-123">(Diese werden auch bezeichnet als POCO-Klassen aus "Plain-Old CLR Objects".) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="4f623-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="4f623-124">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="4f623-124">Adding Model Classes</span></span>

<span data-ttu-id="4f623-125">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="4f623-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="4f623-126">Name der *Klasse* "Movie".</span><span class="sxs-lookup"><span data-stu-id="4f623-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="4f623-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="4f623-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="4f623-128">Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="4f623-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="4f623-129">Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="4f623-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="4f623-130">Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="4f623-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="4f623-131">Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="4f623-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="4f623-132">Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4f623-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="4f623-133">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4f623-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="4f623-134">Weitere Informationen zu `DbContext` und `DbSet`, finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="4f623-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="4f623-135">Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="4f623-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="4f623-136">Die vollständige *Movie.cs* Datei wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="4f623-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="4f623-137">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="4f623-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="4f623-138">Die `MovieDBContext` erstellten Klasse behandelt die Aufgabe der Herstellen einer Verbindung mit der Datenbank und die Zuordnung `Movie` -Objekten, die Datenbankdatensätze.</span><span class="sxs-lookup"><span data-stu-id="4f623-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="4f623-139">Eine Frage, die möglicherweise aufgefordert, ist jedoch, wie Sie zur Angabe der Datenbank hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="4f623-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="4f623-140">Sie erledigen, die Sie durch Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4f623-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="4f623-141">Öffnen Sie das Stammverzeichnis der Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="4f623-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="4f623-142">(Nicht die *"Web.config"* in der Datei die *Ansichten* Ordner.) Beide die folgenden Abbildung anzeigen *"Web.config"* öffnen; die Dateien der *"Web.config"* Datei rot markiert.</span><span class="sxs-lookup"><span data-stu-id="4f623-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="4f623-143">Fügen Sie die folgende Verbindungszeichenfolge, um die `<connectionStrings>` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="4f623-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="4f623-144">Das folgende Beispiel zeigt einen Teil der *"Web.config"* Datei mit der neuen Verbindungszeichenfolge hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="4f623-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="4f623-145">Diese kleine Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="4f623-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="4f623-146">Als Nächstes müssen Sie ein neues erstellen `MoviesController` -Klasse, die Sie verwenden können, an die Filmdaten angezeigt und ermöglichen Benutzern die neuen Film-Angebote erstellen.</span><span class="sxs-lookup"><span data-stu-id="4f623-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4f623-147">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="4f623-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
