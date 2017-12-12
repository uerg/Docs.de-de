---
title: "Abhängigkeitsinjektion in Sichten"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 4586f50bc663b7269914dfff28b61342e3991a48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="c3c56-103">Abhängigkeitsinjektion in Sichten</span><span class="sxs-lookup"><span data-stu-id="c3c56-103">Dependency injection into views</span></span>

<span data-ttu-id="c3c56-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c3c56-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c3c56-105">ASP.NET Core unterstützt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in Sichten.</span><span class="sxs-lookup"><span data-stu-id="c3c56-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="c3c56-106">Dies kann hilfreich für Ansicht-spezifische Dienste, z. B. Lokalisierung oder Daten, die nur für das Auffüllen der Ansichtselemente erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="c3c56-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="c3c56-107">Sollten Sie versuchen, verwalten [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) zwischen dem Controller und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="c3c56-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="c3c56-108">Die meisten Daten, die Ihre Ansichten anzeigen sollte auf dem Controller übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="c3c56-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="c3c56-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c3c56-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="c3c56-110">Ein einfaches Beispiel</span><span class="sxs-lookup"><span data-stu-id="c3c56-110">A Simple Example</span></span>

<span data-ttu-id="c3c56-111">Können Sie einen Dienst einfügen, in einer Ansicht mit der `@inject` Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="c3c56-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="c3c56-112">Sie können sich vorstellen `@inject` als Hinzufügen einer Eigenschaft, um die Ansicht aus, und füllen die Eigenschaft, die mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="c3c56-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="c3c56-113">Die Syntax für `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="c3c56-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="c3c56-114">Ein Beispiel für `@inject` in Aktion:</span><span class="sxs-lookup"><span data-stu-id="c3c56-114">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="c3c56-115">Diese Ansicht zeigt eine Liste von `ToDoItem` zusammen mit einer Zusammenfassung darüber angezeigt, Allgemeine Statistik-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="c3c56-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="c3c56-116">Die Zusammenfassung wird aufgefüllt, von der injizierten `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="c3c56-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="c3c56-117">Dieser Dienst ist für die Abhängigkeitsinjektion in registriert `ConfigureServices` in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c3c56-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="c3c56-118">Die `StatisticsService` führt einige Berechnungen auf den Satz von `ToDoItem` Instanzen, die sie über ein Repository zugreift:</span><span class="sxs-lookup"><span data-stu-id="c3c56-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="c3c56-119">Das beispielrepository verwendet eine speicherinterne Auflistung.</span><span class="sxs-lookup"><span data-stu-id="c3c56-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="c3c56-120">Die Implementierung, die oben gezeigte (das auf alle Daten im Arbeitsspeicher ausgeführt wird) wird nicht empfohlen für große, Remote zugegriffen Datasets.</span><span class="sxs-lookup"><span data-stu-id="c3c56-120">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="c3c56-121">Das Beispiel zeigt Daten aus das Modell, das an die Sicht gebunden ist und der Dienst in der Sicht eingefügt:</span><span class="sxs-lookup"><span data-stu-id="c3c56-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Zum Auflisten von insgesamt Elementen anzeigen abgeschlossen Elemente, die durchschnittliche Priorität und eine Liste von Aufgaben mit den jeweiligen Prioritätsstufen und boolesche Werte, der angibt, Abschluss.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="c3c56-123">Suchen von Daten auffüllen</span><span class="sxs-lookup"><span data-stu-id="c3c56-123">Populating Lookup Data</span></span>

<span data-ttu-id="c3c56-124">Ansicht Injection kann zum Auffüllen der Optionen im UI-Elemente wie Dropdownlisten hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="c3c56-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="c3c56-125">Betrachten Sie ein Benutzerformular für das Profil, das Optionen zum Angeben von Geschlecht, Status und andere Einstellungen enthält.</span><span class="sxs-lookup"><span data-stu-id="c3c56-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="c3c56-126">Rendern eines Formulars, die von mit einer standard-MVC-Ansatz, müsste den Controller anfordern Datenzugriffsdienste für jede dieser Gruppen von Optionen, und füllen Sie ein Modell oder `ViewBag` mit jeder Satz von Optionen zum gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="c3c56-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="c3c56-127">Ein alternativer Ansatz fügt Dienste direkt in der Ansicht, um die Optionen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="c3c56-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="c3c56-128">Dies minimiert den Umfang des Codes, die vom Controller, verschieben diese Ansicht Element Konstruktion Logik in die Sicht selbst erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="c3c56-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="c3c56-129">Controlleraktion anzuzeigenden ein Formular zum Bearbeiten des Profils muss nur dem Formular die Profilinstanz übergeben:</span><span class="sxs-lookup"><span data-stu-id="c3c56-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="c3c56-130">Das HTML-Formular verwendet, um diese Einstellungen aktualisieren enthält Dropdownlisten für drei Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="c3c56-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Aktualisieren Sie Profil-Ansicht mit einem Formular die Eingabe von Name, Geschlecht, Status und Lieblingsfarbe.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="c3c56-132">Diese Listen werden von einem Dienst aufgefüllt, die in der Sicht eingefügt wurde:</span><span class="sxs-lookup"><span data-stu-id="c3c56-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="c3c56-133">Die `ProfileOptionsService` ist ein UI-Level-Dienst bieten nur die Daten, die für dieses Formular benötigt:</span><span class="sxs-lookup"><span data-stu-id="c3c56-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="c3c56-134">Vergessen Sie nicht, um Typen zu registrieren, Sie über die Abhängigkeitsinjektion in fordert, der `ConfigureServices` Methode im *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c3c56-134">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="c3c56-135">Überschreiben von Diensten</span><span class="sxs-lookup"><span data-stu-id="c3c56-135">Overriding Services</span></span>

<span data-ttu-id="c3c56-136">Zusätzlich zu den Räumen neue Dienste, werden dieses Verfahren auch zum Überschreiben von zuvor eingefügte Services auf einer Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="c3c56-136">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="c3c56-137">Die folgende Abbildung zeigt alle verfügbaren Felder auf der Seite, die im ersten Beispiel verwendet:</span><span class="sxs-lookup"><span data-stu-id="c3c56-137">The figure below shows all of the fields available on the page used in the first example:</span></span>

![IntelliSense-Kontextmenü für eine typisierte @-Symbol Html, Komponente, StatsService und Url-Felder auflisten](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="c3c56-139">Wie Sie sehen können, sind die Standardfelder `Html`, `Component`, und `Url` (als auch die `StatsService` , die wir eingefügten).</span><span class="sxs-lookup"><span data-stu-id="c3c56-139">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="c3c56-140">Wenn Sie z. B. der Standard-HTML-Hilfsmethoden durch eigene ersetzen möchten, Sie können problemlos dazu `@inject`:</span><span class="sxs-lookup"><span data-stu-id="c3c56-140">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="c3c56-141">Wenn Sie vorhandene Dienste erweitern möchten, können Sie dieses Verfahren einfach beim erben von oder die vorhandene Implementierung durch eigene wrapping verwenden.</span><span class="sxs-lookup"><span data-stu-id="c3c56-141">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="c3c56-142">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="c3c56-142">See Also</span></span>

* <span data-ttu-id="c3c56-143">Simon Timms Blog: [Suchdaten in Ihrer Ansicht abrufen](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="c3c56-143">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
