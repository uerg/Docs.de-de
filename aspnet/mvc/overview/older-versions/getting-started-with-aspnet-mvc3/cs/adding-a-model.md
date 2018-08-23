---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Hinzufügen eines Modells (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b68f857777b1b69ca401c29261211f40df253e7a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837470"
---
<a name="adding-a-model-c"></a><span data-ttu-id="49587-104">Hinzufügen eines Modells (c#)</span><span class="sxs-lookup"><span data-stu-id="49587-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="49587-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="49587-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="49587-106">In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49587-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="49587-107">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="49587-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="49587-108">Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="49587-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="49587-109">Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:</span><span class="sxs-lookup"><span data-stu-id="49587-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="49587-110">Visual Studio Web Developer Express SP1-Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="49587-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="49587-111">ASP.NET MVC 3 Toolsupdate</span><span class="sxs-lookup"><span data-stu-id="49587-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="49587-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)</span><span class="sxs-lookup"><span data-stu-id="49587-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="49587-113">Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="49587-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="49587-114">Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas.</span><span class="sxs-lookup"><span data-stu-id="49587-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="49587-115">[Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="49587-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="49587-116">Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/adding-a-model.md) in diesem Tutorial.</span><span class="sxs-lookup"><span data-stu-id="49587-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="49587-117">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="49587-117">Adding a Model</span></span>

<span data-ttu-id="49587-118">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="49587-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="49587-119">Diese Klassen werden der "Model" Teil der ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="49587-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="49587-120">Verwenden Sie eine .NET Framework-Datenzugriff-Technologie als Entity Framework bezeichnet definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="49587-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="49587-121">Der Entity Framework (häufig als EF bezeichnet) unterstützt, wird, ein Paradigma für die Entwicklung aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="49587-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="49587-122">Code zuerst können Sie Modellobjekte zu erstellen, indem einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="49587-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="49587-123">(Diese werden auch bezeichnet als POCO-Klassen, von "Plain-Old CLR Objects" ein.) Sie können dann die Datenbank erstellt haben, im laufenden Betrieb von Klassen, die einen Entwicklungsworkflow sehr übersichtlich und eine schnelle ermöglicht haben.</span><span class="sxs-lookup"><span data-stu-id="49587-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="49587-124">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="49587-124">Adding Model Classes</span></span>

<span data-ttu-id="49587-125">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="49587-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="49587-126">Name der *Klasse* "Movie".</span><span class="sxs-lookup"><span data-stu-id="49587-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="49587-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="49587-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="49587-128">Fügen Sie die folgenden fünf Eigenschaften, die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="49587-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="49587-129">Wir verwenden die `Movie` Klasse zur Darstellung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="49587-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="49587-130">Jede Instanz eine `Movie` eine Zeile in einer Datenbanktabelle, und jede Eigenschaft des Objekts entspricht der `Movie` Klasse wird an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="49587-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="49587-131">Fügen Sie in der gleichen Datei Folgendes `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="49587-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="49587-132">Die `MovieDBContext` Klasse darstellt, die Entity Framework filmdatenbankkontext, die verarbeitet werden, abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="49587-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="49587-133">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="49587-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="49587-134">Weitere Informationen zu `DbContext` und `DbSet`, finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="49587-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="49587-135">Damit Sie verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="49587-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="49587-136">Die vollständige *Movie.cs* Datei ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="49587-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="49587-137">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="49587-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="49587-138">Die `MovieDBContext` erstellten Klasse übernimmt die Aufgabe der Verbindung zur Datenbank und Zuordnung `Movie` Objekte mit Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="49587-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="49587-139">Eine Frage, die Sie wahrscheinlich gebeten, die ist jedoch, nach welcher Datenbank geben sie eine Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="49587-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="49587-140">Sie müssen dies durch das Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="49587-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="49587-141">Öffnen Sie den Stammordner der Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="49587-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="49587-142">(Nicht die *"Web.config"* Datei die *Ansichten* Ordner.) Die folgende Abbildung beides anzeigen *"Web.config"* Dateien; öffnen die *"Web.config"* Datei rot markiert.</span><span class="sxs-lookup"><span data-stu-id="49587-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="49587-143">Fügen Sie die folgende Verbindungszeichenfolge für die `<connectionStrings>` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="49587-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="49587-144">Das folgende Beispiel zeigt einen Teil der *"Web.config"* -Datei mit der neuen Verbindungszeichenfolge hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="49587-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="49587-145">Diese geringe Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="49587-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="49587-146">Als Nächstes erstellen Sie ein neues `MoviesController` -Klasse, die Sie verwenden können, die Filmdaten angezeigt und können Benutzer neue Film Auflistungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="49587-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="49587-147">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="49587-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
