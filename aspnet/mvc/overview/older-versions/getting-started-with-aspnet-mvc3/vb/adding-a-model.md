---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "Hinzufügen eines Modells (VB) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a><span data-ttu-id="1fe91-103">Hinzufügen eines Modells (VB)</span><span class="sxs-lookup"><span data-stu-id="1fe91-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="1fe91-104">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1fe91-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="1fe91-105">In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1fe91-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="1fe91-106">Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.</span><span class="sxs-lookup"><span data-stu-id="1fe91-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="1fe91-107">Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1fe91-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="1fe91-108">Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:</span><span class="sxs-lookup"><span data-stu-id="1fe91-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="1fe91-109">Visual Studio Web Developer Express SP1-Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="1fe91-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="1fe91-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="1fe91-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="1fe91-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)</span><span class="sxs-lookup"><span data-stu-id="1fe91-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="1fe91-112">Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1fe91-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="1fe91-113">Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1fe91-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="1fe91-114">[Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="1fe91-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="1fe91-115">Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/adding-a-model.md) dieses Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="1fe91-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="1fe91-116">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="1fe91-116">Adding a Model</span></span>

<span data-ttu-id="1fe91-117">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1fe91-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="1fe91-118">Diese Klassen werden die "" Modellteil der ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1fe91-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="1fe91-119">Verwenden Sie eine .NET Framework Datenzugriffs-Technologie, die das Entity Framework genannt definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="1fe91-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="1fe91-120">Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="1fe91-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="1fe91-121">Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="1fe91-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="1fe91-122">(Diese werden auch bezeichnet als POCO-Klassen aus "Plain-Old CLR Objects".) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="1fe91-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="1fe91-123">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="1fe91-123">Adding Model Classes</span></span>

<span data-ttu-id="1fe91-124">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="1fe91-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="1fe91-125">Benennen Sie die Klasse "Movie".</span><span class="sxs-lookup"><span data-stu-id="1fe91-125">Name the class "Movie".</span></span>

<span data-ttu-id="1fe91-126">Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="1fe91-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="1fe91-127">Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="1fe91-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="1fe91-128">Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1fe91-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="1fe91-129">Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="1fe91-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="1fe91-130">Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1fe91-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="1fe91-131">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1fe91-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="1fe91-132">Weitere Informationen zu `DbContext` und `DbSet`, finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="1fe91-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="1fe91-133">Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `imports` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="1fe91-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="1fe91-134">Die vollständige *Movie.vb* Datei wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="1fe91-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="1fe91-135">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="1fe91-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="1fe91-136">Die `MovieDBContext` erstellten Klasse behandelt die Aufgabe der Herstellen einer Verbindung mit der Datenbank und die Zuordnung `Movie` -Objekten, die Datenbankdatensätze.</span><span class="sxs-lookup"><span data-stu-id="1fe91-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="1fe91-137">Eine Frage, die möglicherweise aufgefordert, ist jedoch, wie Sie zur Angabe der Datenbank hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="1fe91-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="1fe91-138">Sie erledigen, die Sie durch Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1fe91-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="1fe91-139">Öffnen Sie das Stammverzeichnis der Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="1fe91-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="1fe91-140">(Nicht die *"Web.config"* in der Datei die *Ansichten* Ordner.) Beide die folgenden Abbildung anzeigen *"Web.config"* öffnen; die Dateien der *"Web.config"* Datei rot markiert.</span><span class="sxs-lookup"><span data-stu-id="1fe91-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="1fe91-141">Fügen Sie die folgende Verbindungszeichenfolge, um die `<connectionStrings>` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="1fe91-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="1fe91-142">Das folgende Beispiel zeigt einen Teil der *"Web.config"* Datei mit der neuen Verbindungszeichenfolge hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="1fe91-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="1fe91-143">Diese kleine Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="1fe91-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="1fe91-144">Als Nächstes müssen Sie ein neues erstellen `MoviesController` -Klasse, die Sie verwenden können, an die Filmdaten angezeigt und ermöglichen Benutzern die neuen Film-Angebote erstellen.</span><span class="sxs-lookup"><span data-stu-id="1fe91-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1fe91-145">[Zurück](adding-a-view.md)
[Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="1fe91-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
