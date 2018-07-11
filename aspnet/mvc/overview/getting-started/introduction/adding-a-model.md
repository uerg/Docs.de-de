---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Hinzufügen eines Modells | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211053"
---
<a name="adding-a-model"></a><span data-ttu-id="1200b-102">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="1200b-102">Adding a Model</span></span>
====================
<span data-ttu-id="1200b-103">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1200b-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="1200b-104">In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1200b-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="1200b-105">Diese Klassen bilden die &quot;Modell&quot; Teil der ASP.NET MVC-app.</span><span class="sxs-lookup"><span data-stu-id="1200b-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="1200b-106">Verwenden Sie eine .NET Framework-Datenzugriff-Technologie als bezeichnet die [Entity Framework](https://docs.microsoft.com/ef/) definieren und mit diesen Modellklassen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="1200b-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="1200b-107">Der Entity Framework (häufig als EF bezeichnet) unterstützt, wird, ein Paradigma für die Entwicklung aufgerufen *Code First*.</span><span class="sxs-lookup"><span data-stu-id="1200b-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="1200b-108">Code zuerst können Sie Modellobjekte zu erstellen, indem einfache Klassen schreiben.</span><span class="sxs-lookup"><span data-stu-id="1200b-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="1200b-109">(Diese werden auch bezeichnet als POCO-Klassen von &quot;einfache alte CLR-Objekte.&quot;) Sie können dann die Datenbank erstellt haben, im laufenden Betrieb von Klassen, die einen Entwicklungsworkflow sehr übersichtlich und eine schnelle ermöglicht haben.</span><span class="sxs-lookup"><span data-stu-id="1200b-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="1200b-110">Wenn Sie zum ersten Erstellen der Datenbank erforderlich sind, können Sie dieses Lernprogramm aus, um mehr über MVC und EF-app-Entwicklung erfahren weiterhin folgen.</span><span class="sxs-lookup"><span data-stu-id="1200b-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="1200b-111">Führen Sie dann Tom Fizmakens [ASP.NET-Gerüstbau](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) Tutorial, in dem die Datenbank-first-Ansatz behandelt.</span><span class="sxs-lookup"><span data-stu-id="1200b-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="1200b-112">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="1200b-112">Adding Model Classes</span></span>

<span data-ttu-id="1200b-113">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="1200b-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="1200b-114">Geben Sie die *Klasse* Namen &quot;Film&quot;.</span><span class="sxs-lookup"><span data-stu-id="1200b-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="1200b-115">Fügen Sie die folgenden fünf Eigenschaften, die `Movie` Klasse:</span><span class="sxs-lookup"><span data-stu-id="1200b-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="1200b-116">Wir verwenden die `Movie` Klasse zur Darstellung von Filmen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1200b-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="1200b-117">Jede Instanz eine `Movie` eine Zeile in einer Datenbanktabelle, und jede Eigenschaft des Objekts entspricht der `Movie` Klasse wird an eine Spalte in der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1200b-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="1200b-118">Hinweis: Um System.Data.Entity und die verknüpfte Klasse verwenden, müssen Sie installieren die [Entity Framework-NuGet-Pakets](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="1200b-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="1200b-119">Führen Sie den Link, um weitere Anweisungen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="1200b-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="1200b-120">Fügen Sie in der gleichen Datei Folgendes `MovieDBContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="1200b-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="1200b-121">Die `MovieDBContext` Klasse darstellt, die Entity Framework filmdatenbankkontext, die verarbeitet werden, abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1200b-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="1200b-122">Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1200b-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="1200b-123">Damit Sie verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="1200b-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="1200b-124">Hierzu können Sie manuell hinzufügen, die mit-Anweisung, oder Sie können die roten Wellenlinien zeigen, klicken Sie auf `Show potential fixes` , und klicken Sie auf `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="1200b-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="1200b-125">Hinweis: Einige nicht verwendete `using` Anweisungen entfernt wurden.</span><span class="sxs-lookup"><span data-stu-id="1200b-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="1200b-126">Visual Studio wird nicht verwendete Abhängigkeiten als grau angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1200b-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="1200b-127">Sie können die nicht verwendete Abhängigkeiten entfernen, indem Sie mit dem Mauszeiger auf die grauen Abhängigkeiten, klicken Sie auf `Show potential fixes` , und klicken Sie auf **nicht verwendete Using-Direktiven entfernen.**</span><span class="sxs-lookup"><span data-stu-id="1200b-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="1200b-128">Schließlich haben wir ein Modell (das M in MVC) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1200b-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="1200b-129">Im nächsten Abschnitt Arbeiten Sie mit der Datenbank-Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="1200b-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1200b-130">[Zurück](adding-a-view.md)
> [Weiter](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="1200b-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
