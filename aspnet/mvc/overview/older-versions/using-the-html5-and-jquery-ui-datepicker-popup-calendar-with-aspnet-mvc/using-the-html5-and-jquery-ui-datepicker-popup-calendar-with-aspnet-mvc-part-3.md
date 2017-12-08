---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3 | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: dc81961094928025e25cf62ce4d51d12bc67b80c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="0336e-103">Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3</span><span class="sxs-lookup"><span data-stu-id="0336e-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="0336e-104">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0336e-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="0336e-105">In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="0336e-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="0336e-106">Arbeiten mit komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="0336e-106">Working with Complex Types</span></span>

<span data-ttu-id="0336e-107">In diesem Abschnitt Sie erstellen eine Adressklasse und erfahren Sie, wie beim Erstellen einer Vorlage aus, um es anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0336e-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="0336e-108">In der *Modelle* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *Person.cs* dort legen Sie zwei Typen: ein `Person` Klasse und ein `Address` Klasse.</span><span class="sxs-lookup"><span data-stu-id="0336e-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="0336e-109">Die `Person` Klasse enthält eine Eigenschaft, die als typisiert ist `Address`.</span><span class="sxs-lookup"><span data-stu-id="0336e-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="0336e-110">Die `Address` Typ ist ein komplexer Typ, d. h., es ist keines der integrierten Typen wie `int`, `string`, oder `double`.</span><span class="sxs-lookup"><span data-stu-id="0336e-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="0336e-111">Stattdessen hat es mehrere Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="0336e-111">Instead, it has several properties.</span></span> <span data-ttu-id="0336e-112">Der Code für den neuen Klassen sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0336e-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="0336e-113">In der `Movie` Controller, fügen Sie die folgenden `PersonDetail` Aktion aus, um eine Personeninstanz angezeigt:</span><span class="sxs-lookup"><span data-stu-id="0336e-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="0336e-114">Fügen Sie folgenden Code, der `Movie` Controller zum Auffüllen der `Person` Modell mit Beispieldaten:</span><span class="sxs-lookup"><span data-stu-id="0336e-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="0336e-115">Öffnen der *Views\Movies\PersonDetail.cshtml* Datei, und fügen Sie das folgende Markup für die `PersonDetail` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0336e-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="0336e-116">Drücken Sie STRG + F5, um die Anwendung auszuführen, und navigieren zu *Filme/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="0336e-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="0336e-117">Die `PersonDetail` Ansicht enthält nicht die `Address` komplexer Typ, wie Sie sehen in diesem Screenshot.</span><span class="sxs-lookup"><span data-stu-id="0336e-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="0336e-118">(Es ist keine Adresse dargestellt.)</span><span class="sxs-lookup"><span data-stu-id="0336e-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="0336e-119">Die `Address` Modelldaten werden nicht angezeigt werden, da es sich um einen komplexen Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="0336e-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="0336e-120">Um die Adressinformationen anzuzeigen, öffnen die *Views\Movies\PersonDetail.cshtml* Datei erneut, und fügen Sie das folgende Markup hinzu.</span><span class="sxs-lookup"><span data-stu-id="0336e-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="0336e-121">Das vollständige Markup für die `PersonDetail` jetzt Ansicht wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="0336e-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="0336e-122">Die Anwendung erneut ausführen und Anzeigen der `PersonDetail` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0336e-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="0336e-123">Die Adressinformationen werden nun angezeigt:</span><span class="sxs-lookup"><span data-stu-id="0336e-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="0336e-124">Erstellen einer Vorlage für einen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="0336e-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="0336e-125">In diesem Abschnitt erstellen Sie eine Vorlage, die verwendet werden soll, zum Rendern der `Address` komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="0336e-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="0336e-126">Beim Erstellen einer Vorlage für die `Address` Typ ASP.NET-MVC automatisch verwenden, um eine Adressmodell an einer beliebigen Stelle in der Anwendung zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="0336e-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="0336e-127">Dies bietet Ihnen eine Möglichkeit zum Steuern des Renderns von der `Address` Typ von nur einem Ort in der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0336e-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="0336e-128">In der *Views\Shared\DisplayTemplates* Ordner, erstellen Sie eine stark typisierte Teilansicht mit dem Namen **Adresse**:</span><span class="sxs-lookup"><span data-stu-id="0336e-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="0336e-129">Klicken Sie auf **hinzufügen**, und öffnen Sie die neue *Views\Shared\DisplayTemplates\Address.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="0336e-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="0336e-130">Die neue Ansicht enthält die folgenden generierten Markup:</span><span class="sxs-lookup"><span data-stu-id="0336e-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="0336e-131">Führen Sie die Anwendung und Anzeigen der `PersonDetail` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0336e-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="0336e-132">Dieses Mal die `Address` die neu erstellte Vorlage dient zum Anzeigen der `Address` komplexer Typ, damit die Anzeige wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="0336e-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="0336e-133">Zusammenfassung: Möglichkeiten zum Angeben der Modell-Anzeigeformat und Vorlage</span><span class="sxs-lookup"><span data-stu-id="0336e-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="0336e-134">Sie haben gesehen, dass das Format oder die Vorlage für eine Modelleigenschaft angeben können, legen Sie mithilfe der folgenden Vorgehensweisen:</span><span class="sxs-lookup"><span data-stu-id="0336e-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="0336e-135">Anwenden der `DisplayFormat` -Attribut auf eine Eigenschaft im Modell.</span><span class="sxs-lookup"><span data-stu-id="0336e-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="0336e-136">Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="0336e-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="0336e-137">Anwenden einer [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut auf eine Eigenschaft im Modell und die Angabe des Datentyps.</span><span class="sxs-lookup"><span data-stu-id="0336e-137">Applying a [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="0336e-138">Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0336e-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="0336e-139">Wenn die Anwendung enthält eine *date.cshtml* Vorlage in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* Ordner, die Vorlage wird verwendet, um das Rendern der `DateTime` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0336e-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="0336e-140">Andernfalls zeigt die integrierten ASP.NET Datenvorlagen System die Eigenschaft als Datum.</span><span class="sxs-lookup"><span data-stu-id="0336e-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="0336e-141">Erstellen einer Anzeigevorlage für die in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* Ordner, deren Namen übereinstimmen, den Datentyp, die Sie formatieren möchten.</span><span class="sxs-lookup"><span data-stu-id="0336e-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="0336e-142">Beispielsweise haben Sie gesehen, die die *Views\Shared\DisplayTemplates\DateTime.cshtml* wurde zum Rendern verwendet `DateTime` Eigenschaften in einem Modell, ohne das Hinzufügen eines Attributs für das Modell und Ansichten Markup hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0336e-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="0336e-143">Mithilfe der [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut für das Modell auf die Vorlage zum Anzeigen der Modelleigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="0336e-143">Using the [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="0336e-144">Das explizite Hinzufügen der Anzeigename für die Vorlage, die [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) in einer Ansicht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="0336e-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="0336e-145">Der Ansatz, den Sie verwenden, hängt davon ab, welche Schritte Sie in der Anwendung ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="0336e-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="0336e-146">Es ist nicht ungewöhnlich, dass zum Mischen von dieser Ansätze, um genau die Art der Formatierung zu erhalten, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="0336e-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="0336e-147">Wechseln Sie im nächsten Abschnitt etwas TheBeerHouse und Verschieben von anpassen, wie Daten angezeigt werden, um anpassen, wie er eingegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0336e-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="0336e-148">Sie müssen die jQuery-Datepicker, die den Edit-Ansichten in der Anwendung um benutzerdefinierbaren Möglichkeit zum Angeben von Datumsangaben verknüpft.</span><span class="sxs-lookup"><span data-stu-id="0336e-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0336e-149">[Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="0336e-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
