---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Hinzufügen eines Modells | Microsoft Docs
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871960"
---
<a name="adding-a-model"></a><span data-ttu-id="4ebb9-104">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="4ebb9-104">Adding a Model</span></span>
====================
<span data-ttu-id="4ebb9-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4ebb9-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="4ebb9-106">Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="4ebb9-107">Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="4ebb9-108">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="4ebb9-109">Diese Klassen werden die &quot;Modell&quot; Teil der ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="4ebb9-110">Verwenden Sie eine .NET Framework Datenzugriffs-Technologie als bezeichnet den [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="4ebb9-111">Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="4ebb9-112">Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="4ebb9-113">(Diese werden auch bezeichnet als POCO-Klassen aus &quot;Plain-Old CLR-Objekte.&quot;) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="4ebb9-114">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="4ebb9-114">Adding Model Classes</span></span>

<span data-ttu-id="4ebb9-115">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="4ebb9-116">Geben Sie die *Klasse* Namen &quot;Film&quot;.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="4ebb9-117">Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="4ebb9-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="4ebb9-118">Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="4ebb9-119">Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="4ebb9-120">Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="4ebb9-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="4ebb9-121">Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="4ebb9-122">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="4ebb9-123">Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="4ebb9-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="4ebb9-124">Die vollständige *Movie.cs* Datei wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="4ebb9-125">(Mehrere using-Anweisungen, die nicht benötigt wurden entfernt.)</span><span class="sxs-lookup"><span data-stu-id="4ebb9-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="4ebb9-126">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="4ebb9-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="4ebb9-127">Die `MovieDBContext` erstellten Klasse behandelt die Aufgabe der Herstellen einer Verbindung mit der Datenbank und die Zuordnung `Movie` -Objekten, die Datenbankdatensätze.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="4ebb9-128">Eine Frage, die möglicherweise aufgefordert, ist jedoch, wie Sie zur Angabe der Datenbank hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="4ebb9-129">Sie erledigen, die Sie durch Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="4ebb9-130">Öffnen Sie das Stammverzeichnis der Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="4ebb9-131">(Nicht die *"Web.config"* in der Datei die *Ansichten* Ordner.) Öffnen der *"Web.config"* Datei rot umrandet.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="4ebb9-132">Fügen Sie die folgende Verbindungszeichenfolge, um die `<connectionStrings>` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="4ebb9-133">Das folgende Beispiel zeigt einen Teil der *"Web.config"* Datei mit der neuen Verbindungszeichenfolge hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="4ebb9-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="4ebb9-134">Diese kleine Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="4ebb9-135">Als Nächstes müssen Sie ein neues erstellen `MoviesController` -Klasse, die Sie verwenden können, an die Filmdaten angezeigt und ermöglichen Benutzern die neuen Film-Angebote erstellen.</span><span class="sxs-lookup"><span data-stu-id="4ebb9-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4ebb9-136">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="4ebb9-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
