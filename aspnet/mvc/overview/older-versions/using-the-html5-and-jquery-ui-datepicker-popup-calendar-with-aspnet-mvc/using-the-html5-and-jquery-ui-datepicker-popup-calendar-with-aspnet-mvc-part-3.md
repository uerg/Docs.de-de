---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: baaca74d584a6d5028824e877c561494cdff044e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577222"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="470e0-103">Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3</span><span class="sxs-lookup"><span data-stu-id="470e0-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="470e0-104">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="470e0-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="470e0-105">In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="470e0-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="470e0-106">Arbeiten mit komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="470e0-106">Working with Complex Types</span></span>

<span data-ttu-id="470e0-107">In diesem Abschnitt Sie erstellen Sie eine Adressklasse und erfahren Sie, wie zum Erstellen einer Vorlage aus, um es anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="470e0-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="470e0-108">In der *Modelle* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *Person.cs* , fügen Sie zwei Typen: ein `Person` Klasse und ein `Address` Klasse.</span><span class="sxs-lookup"><span data-stu-id="470e0-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="470e0-109">Die `Person` Klasse enthält eine Eigenschaft, die als typisiert ist `Address`.</span><span class="sxs-lookup"><span data-stu-id="470e0-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="470e0-110">Die `Address` Typ ist ein komplexer Typ, d. h., es ist keiner der integrierten Typen wie `int`, `string`, oder `double`.</span><span class="sxs-lookup"><span data-stu-id="470e0-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="470e0-111">Stattdessen verfügt er über mehrere Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="470e0-111">Instead, it has several properties.</span></span> <span data-ttu-id="470e0-112">Der Code für die neuen Klassen sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="470e0-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="470e0-113">In der `Movie` Controller, fügen Sie die folgenden `PersonDetail` Aktion aus, um eine Personeninstanz angezeigt:</span><span class="sxs-lookup"><span data-stu-id="470e0-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="470e0-114">Klicken Sie dann fügen Sie den folgenden Code der `Movie` Controller zum Auffüllen der `Person` Modell mit einigen Beispieldaten:</span><span class="sxs-lookup"><span data-stu-id="470e0-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="470e0-115">Öffnen der *Views\Movies\PersonDetail.cshtml* Datei, und fügen Sie das folgende Markup für die `PersonDetail` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="470e0-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="470e0-116">Drücken Sie STRG + F5, um die Anwendung auszuführen, und navigieren zu *Filme/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="470e0-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="470e0-117">Die `PersonDetail` Ansicht enthält nicht die `Address` komplexer Typ sein, wie Sie sehen in diesem Screenshot.</span><span class="sxs-lookup"><span data-stu-id="470e0-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="470e0-118">(Es ist keine Adresse angezeigt.)</span><span class="sxs-lookup"><span data-stu-id="470e0-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="470e0-119">Die `Address` Modellieren von Daten werden nicht angezeigt werden, da es sich um einen komplexen Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="470e0-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="470e0-120">Um die Informationen zur Adresse anzuzeigen, öffnen Sie die *Views\Movies\PersonDetail.cshtml* -Datei erneut, und fügen Sie das folgende Markup hinzu.</span><span class="sxs-lookup"><span data-stu-id="470e0-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="470e0-121">Das vollständige Markup für die `PersonDetail` nun Ansicht folgendermaßen aussieht:</span><span class="sxs-lookup"><span data-stu-id="470e0-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="470e0-122">Die Anwendung erneut ausführen und Anzeigen der `PersonDetail` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="470e0-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="470e0-123">Die Adressinformationen wird jetzt angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="470e0-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="470e0-124">Erstellen einer Vorlage für einen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="470e0-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="470e0-125">In diesem Abschnitt erstellen Sie eine Vorlage, die zum Rendern verwendet werden wird die `Address` komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="470e0-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="470e0-126">Wenn Sie eine Vorlage zum Erstellen der `Address` Typ ASP.NET MVC können automatisch verwenden, um ein Adressmodell, an einer beliebigen Stelle in der Anwendung formatieren.</span><span class="sxs-lookup"><span data-stu-id="470e0-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="470e0-127">Dies bietet Ihnen eine Möglichkeit zum Steuern des Renderns der `Address` Typ aus einer Stelle in der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="470e0-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="470e0-128">In der *Views\Shared\DisplayTemplates* Ordner, erstellen Sie eine stark typisierte Teilansicht mit dem Namen **Adresse**:</span><span class="sxs-lookup"><span data-stu-id="470e0-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="470e0-129">Klicken Sie auf **hinzufügen**, und öffnen Sie dann auf die neue *Views\Shared\DisplayTemplates\Address.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="470e0-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="470e0-130">Die neue Ansicht enthält die folgende generierte Markup:</span><span class="sxs-lookup"><span data-stu-id="470e0-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="470e0-131">Führen Sie die Anwendung und Anzeigen der `PersonDetail` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="470e0-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="470e0-132">Dieses Mal die `Address` Vorlage, die Sie gerade erstellt haben dient zum Anzeigen der `Address` komplexer Typ sein, damit die Anzeige wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="470e0-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="470e0-133">Zusammenfassung:-Möglichkeiten, um das Anzeigeformat für Modell und die Vorlage anzugeben.</span><span class="sxs-lookup"><span data-stu-id="470e0-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="470e0-134">Sie haben gesehen, dass Sie das Format oder die Vorlage für eine Modelleigenschaft angeben können, mithilfe der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="470e0-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="470e0-135">Anwenden der `DisplayFormat` -Attribut auf eine Eigenschaft im Modell.</span><span class="sxs-lookup"><span data-stu-id="470e0-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="470e0-136">Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="470e0-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="470e0-137">Anwenden einer [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut auf eine Eigenschaft im Modell und die Angabe des Datentyps.</span><span class="sxs-lookup"><span data-stu-id="470e0-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="470e0-138">Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="470e0-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="470e0-139">Wenn die Anwendung enthält eine *date.cshtml* Vorlage in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* -Ordner, die Vorlage wird verwendet, um das Rendern der `DateTime` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="470e0-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="470e0-140">Andernfalls zeigt die integrierten ASP.NET vorlagensystem die Eigenschaft als Datum.</span><span class="sxs-lookup"><span data-stu-id="470e0-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="470e0-141">Erstellen einer Anzeigevorlage für die in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* Ordner, deren Name entspricht den Datentyp, der formatiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="470e0-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="470e0-142">Beispielsweise haben Sie gesehen, die die *Views\Shared\DisplayTemplates\DateTime.cshtml* wurde verwendet, um das Rendern `DateTime` Eigenschaften in einem Modell, ohne dass das Hinzufügen eines Attributs mit dem Modell oder Ansichten Markup hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="470e0-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="470e0-143">Mithilfe der ["UIHint"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut für das Modell, das die Vorlage zum Anzeigen der Modelleigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="470e0-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="470e0-144">Explizite Hinzufügen der Anzeigename der Vorlage, die [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) in einer Ansicht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="470e0-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="470e0-145">Der Ansatz, den Sie verwenden, hängt davon ab, was Sie in Ihrer Anwendung tun müssen.</span><span class="sxs-lookup"><span data-stu-id="470e0-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="470e0-146">Es ist nicht ungewöhnlich, kombinieren diese Ansätze, um genau die Art der Formatierung zu erhalten, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="470e0-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="470e0-147">Wechseln Sie im nächsten Abschnitt etwas TheBeerHouse und Verschieben von anpassen, wie Daten angezeigt werden, zum Anpassen, wie er eingegeben wird.</span><span class="sxs-lookup"><span data-stu-id="470e0-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="470e0-148">Sie müssen den bearbeiten-Ansichten in der Anwendung aus, um eine elegante Möglichkeit, die Datumsangaben festlegen, geben Sie "DatePicker" jQuery einbinden.</span><span class="sxs-lookup"><span data-stu-id="470e0-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="470e0-149">[Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="470e0-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
