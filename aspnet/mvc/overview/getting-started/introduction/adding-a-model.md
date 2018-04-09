---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Hinzufügen eines Modells | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a><span data-ttu-id="6e103-102">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="6e103-102">Adding a Model</span></span>
====================
<span data-ttu-id="6e103-103">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6e103-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="6e103-104">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6e103-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="6e103-105">Diese Klassen werden die &quot;Modell&quot; Teil von ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6e103-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="6e103-106">Verwenden Sie eine .NET Framework Datenzugriffs-Technologie als bezeichnet den [Entity Framework](https://docs.microsoft.com/ef/) definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="6e103-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="6e103-107">Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="6e103-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="6e103-108">Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="6e103-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="6e103-109">(Diese werden auch bezeichnet als POCO-Klassen aus &quot;Plain-Old CLR-Objekte.&quot;) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="6e103-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="6e103-110">Wenn Sie beim ersten Erstellen der Datenbank erforderlich sind, können Sie dieses Lernprogramm aus, um weitere Informationen zu app-Entwicklung von MVC und EF weiterhin folgen.</span><span class="sxs-lookup"><span data-stu-id="6e103-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="6e103-111">Sie können dann folgen Tom Fizmakens [ASP.NET Gerüstbau](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) Lernprogramms, das Datenbank beim ersten Ansatz behandelt.</span><span class="sxs-lookup"><span data-stu-id="6e103-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="6e103-112">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="6e103-112">Adding Model Classes</span></span>

<span data-ttu-id="6e103-113">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="6e103-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="6e103-114">Geben Sie die *Klasse* Namen &quot;Film&quot;.</span><span class="sxs-lookup"><span data-stu-id="6e103-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="6e103-115">Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="6e103-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="6e103-116">Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="6e103-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="6e103-117">Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="6e103-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="6e103-118">Hinweis: Damit System.Data.Entity und die verknüpfte Klasse verwenden zu können, müssen Sie installieren die [Entity Framework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="6e103-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="6e103-119">Führen Sie den Link, um weitere Anweisungen aus.</span><span class="sxs-lookup"><span data-stu-id="6e103-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="6e103-120">Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="6e103-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="6e103-121">Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6e103-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="6e103-122">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6e103-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="6e103-123">Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="6e103-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="6e103-124">Dazu können Sie manuell hinzufügen, die mit-Anweisung, oder Sie können die roten Wellenlinien zeigen, klicken Sie auf `Show potential fixes` , und klicken Sie auf `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="6e103-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="6e103-125">Hinweis: Einige nicht verwendete `using` Anweisungen wurden entfernt.</span><span class="sxs-lookup"><span data-stu-id="6e103-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="6e103-126">Visual Studio wird nicht verwendete Abhängigkeiten als grau angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6e103-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="6e103-127">Sie können nicht verwendete Abhängigkeiten entfernen, indem Sie mit dem Mauszeiger auf die grauen Abhängigkeiten, klicken Sie auf `Show potential fixes` , und klicken Sie auf **nicht verwendete Using-Direktiven entfernen.**</span><span class="sxs-lookup"><span data-stu-id="6e103-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="6e103-128">Schließlich haben wir ein Modell (das M in MVC) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6e103-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="6e103-129">Im nächsten Abschnitt Arbeiten Sie mit der Datenbank-Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="6e103-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6e103-130">[Zurück](adding-a-view.md)
> [Weiter](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="6e103-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
