---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Hinzufügen eines Modells | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829515"
---
<a name="adding-a-model"></a><span data-ttu-id="c6763-104">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="c6763-104">Adding a Model</span></span>
====================
<span data-ttu-id="c6763-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c6763-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="c6763-106">Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="c6763-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="c6763-107">Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="c6763-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="c6763-108">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c6763-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="c6763-109">Diese Klassen bilden die &quot;Modell&quot; Teils der ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c6763-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="c6763-110">Verwenden Sie eine .NET Framework-Datenzugriff-Technologie als bezeichnet die [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="c6763-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="c6763-111">Der Entity Framework (häufig als EF bezeichnet) unterstützt, wird, ein Paradigma für die Entwicklung aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="c6763-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="c6763-112">Code zuerst können Sie Modellobjekte zu erstellen, indem einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="c6763-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="c6763-113">(Diese werden auch bezeichnet als POCO-Klassen von &quot;einfache alte CLR-Objekte.&quot;) Sie können dann die Datenbank erstellt haben, im laufenden Betrieb von Klassen, die einen Entwicklungsworkflow sehr übersichtlich und eine schnelle ermöglicht haben.</span><span class="sxs-lookup"><span data-stu-id="c6763-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="c6763-114">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="c6763-114">Adding Model Classes</span></span>

<span data-ttu-id="c6763-115">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="c6763-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="c6763-116">Geben Sie die *Klasse* Namen &quot;Film&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6763-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="c6763-117">Fügen Sie die folgenden fünf Eigenschaften, die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="c6763-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="c6763-118">Wir verwenden die `Movie` Klasse zur Darstellung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c6763-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="c6763-119">Jede Instanz eine `Movie` eine Zeile in einer Datenbanktabelle, und jede Eigenschaft des Objekts entspricht der `Movie` Klasse wird an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="c6763-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="c6763-120">Fügen Sie in der gleichen Datei Folgendes `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="c6763-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="c6763-121">Die `MovieDBContext` Klasse darstellt, die Entity Framework filmdatenbankkontext, die verarbeitet werden, abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c6763-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="c6763-122">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="c6763-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="c6763-123">Damit Sie verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="c6763-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="c6763-124">Die vollständige *Movie.cs* Datei ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c6763-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="c6763-125">(Mehrere using-Anweisungen, die nicht benötigt wurden entfernt.)</span><span class="sxs-lookup"><span data-stu-id="c6763-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="c6763-126">Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="c6763-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="c6763-127">Die `MovieDBContext` erstellten Klasse übernimmt die Aufgabe der Verbindung zur Datenbank und Zuordnung `Movie` Objekte mit Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="c6763-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="c6763-128">Eine Frage, die Sie wahrscheinlich gebeten, die ist jedoch, nach welcher Datenbank geben sie eine Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="c6763-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="c6763-129">Sie müssen dies durch das Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c6763-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="c6763-130">Öffnen Sie den Stammordner der Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="c6763-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="c6763-131">(Nicht die *"Web.config"* Datei die *Ansichten* Ordner.) Öffnen der *"Web.config"* Datei rot umrandet.</span><span class="sxs-lookup"><span data-stu-id="c6763-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="c6763-132">Fügen Sie die folgende Verbindungszeichenfolge für die `<connectionStrings>` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="c6763-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="c6763-133">Das folgende Beispiel zeigt einen Teil der *"Web.config"* -Datei mit der neuen Verbindungszeichenfolge hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="c6763-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="c6763-134">Diese geringe Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="c6763-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="c6763-135">Als Nächstes erstellen Sie ein neues `MoviesController` -Klasse, die Sie verwenden können, die Filmdaten angezeigt und können Benutzer neue Film Auflistungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c6763-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c6763-136">[Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="c6763-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
