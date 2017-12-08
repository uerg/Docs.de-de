---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: "Ausführen von einfachen Überprüfung (c#) | Microsoft Docs"
author: StephenWalther
description: "Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendung. In diesem Lernprogramm führt Stephen Walther Sie Modellzustand und die Validierung, HTML-Hilfsobjekt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 005872308d9d4d8ac7feb12dd5ab1fc463d0140e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="30e8f-104">Ausführen von einfachen Überprüfung (c#)</span><span class="sxs-lookup"><span data-stu-id="30e8f-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="30e8f-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="30e8f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="30e8f-106">Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="30e8f-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="30e8f-107">In diesem Lernprogramm führt Stephen Walther Sie Modellzustand sowie die Validierung, HTML-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="30e8f-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="30e8f-108">Ziel dieses Lernprogramms wird erläutert, wie Sie die Validierung innerhalb einer ASP.NET MVC-Anwendung ausführen können.</span><span class="sxs-lookup"><span data-stu-id="30e8f-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="30e8f-109">Beispielsweise erfahren Sie, wie Sie verhindern, dass eine Person senden eines Formulars, das einen Wert für ein Pflichtfeld nicht enthält.</span><span class="sxs-lookup"><span data-stu-id="30e8f-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="30e8f-110">Erfahren Sie, wie Modellzustand und die Validierung HTML-Hilfsmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="30e8f-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="30e8f-111">Grundlegendes zu Modellstatus</span><span class="sxs-lookup"><span data-stu-id="30e8f-111">Understanding Model State</span></span>

<span data-ttu-id="30e8f-112">Sie mithilfe Modellstatus – oder genauer gesagt das Modellzustandswörterbuch - Validierungsfehler dar.</span><span class="sxs-lookup"><span data-stu-id="30e8f-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="30e8f-113">Beispielsweise überprüft die Create()-Aktion im Codebeispiel 1 die Eigenschaften einer Klasse Product vor dem Hinzufügen der Product-Klasse zu einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="30e8f-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="30e8f-114">Ich bin nicht empfehlen, dass Sie die Überprüfung oder Datenbanklogik einen Controller hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="30e8f-115">Ein Controller sollte nur im Zusammenhang mit der flusssteuerung für die Anwendung Logik enthalten.</span><span class="sxs-lookup"><span data-stu-id="30e8f-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="30e8f-116">Wir machen eine Verknüpfung zum Dinge einfach zu halten.</span><span class="sxs-lookup"><span data-stu-id="30e8f-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="30e8f-117">**1 – Controllers\ProductController.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="30e8f-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="30e8f-118">Im Codebeispiel 1 werden die Eigenschaften Name, Beschreibung und "UnitsInStock" die Produktklasse überprüft.</span><span class="sxs-lookup"><span data-stu-id="30e8f-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="30e8f-119">Wenn eine dieser Eigenschaften einen Überprüfungstest fehlschlagen wird ein Fehler auf das Modellzustandswörterbuch (dargestellt durch die ModelState-Eigenschaft der Controllerklasse) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="30e8f-120">Wenn Fehler, im Modellstatus vorliegen gibt die ModelState.IsValid-Eigenschaft "false" zurück.</span><span class="sxs-lookup"><span data-stu-id="30e8f-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="30e8f-121">In diesem Fall wird das HTML-Formular für das Erstellen eines neuen Produkts erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="30e8f-122">Wenn keine gültigkeitsprobleme bestehen, wird das neue Produkt andernfalls mit der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="30e8f-123">Verwenden die Hilfsprogramme für die Validierung</span><span class="sxs-lookup"><span data-stu-id="30e8f-123">Using the Validation Helpers</span></span>

<span data-ttu-id="30e8f-124">ASP.NET MVC-Framework umfasst zwei Hilfsmethoden der Überprüfung: das Html.ValidationMessage()-Hilfsobjekt und der Html.ValidationSummary()-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="30e8f-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="30e8f-125">Verwenden Sie diese zwei Hilfsmethoden in einer Ansicht, um Überprüfungsfehlermeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="30e8f-126">Die Hilfsprogramme Html.ValidationMessage() und Html.ValidationSummary() dienen in den Ansichten erstellen und bearbeiten, die von ASP.NET-MVC-Gerüstbau automatisch generiert werden.</span><span class="sxs-lookup"><span data-stu-id="30e8f-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="30e8f-127">Führen Sie diese Schritte aus, um die Erstellungsansicht generiert:</span><span class="sxs-lookup"><span data-stu-id="30e8f-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="30e8f-128">Mit der rechten Maustaste der Create()-Aktion des Produkts-Controllers ein, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="30e8f-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="30e8f-129">In der **Ansicht hinzufügen** Dialogfeld, aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="30e8f-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="30e8f-130">Aus der **Datenklasse anzeigen** Dropdown Liste, wählen Sie die Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="30e8f-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="30e8f-131">Aus der **Inhalt anzeigen** Dropdown-Liste wählen erstellen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="30e8f-132">Klicken Sie auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="30e8f-132">Click the **Add** button.</span></span>


<span data-ttu-id="30e8f-133">Stellen Sie sicher, dass Ihre Anwendung vor dem Hinzufügen einer Ansicht zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="30e8f-134">Andernfalls, die Liste der Klassen erscheinen nicht in der **Datenklasse anzeigen** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="30e8f-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="30e8f-135">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="30e8f-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="30e8f-136">**Abbildung 01**: Hinzufügen einer Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="30e8f-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="30e8f-137">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="30e8f-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="30e8f-138">**Abbildung 02**: Erstellen einer stark typisierten Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="30e8f-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="30e8f-139">Nachdem Sie diese Schritte abgeschlossen haben, erhalten Sie die Erstellungsansicht 2 aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="30e8f-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="30e8f-140">**Auflisten von 2 – Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="30e8f-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="30e8f-141">Auflisten von 2 wird das Hilfsobjekt Html.ValidationSummary() sofort über die HTML-Formular aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="30e8f-142">Dieses Hilfsprogramm wird verwendet, um eine Liste der Überprüfungsfehlermeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="30e8f-143">Das Hilfsobjekt Html.ValidationSummary() rendert die Fehler in einer Liste mit Aufzählungszeichen aus.</span><span class="sxs-lookup"><span data-stu-id="30e8f-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="30e8f-144">Das Hilfsobjekt Html.ValidationMessage() wird neben jeder HTML-Formular Felder bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="30e8f-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="30e8f-145">Dieses Hilfsprogramm wird verwendet, um eine Fehlermeldung rechts neben einem Formularfeld anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="30e8f-146">In Fall 2 auflisten wird das Hilfsprogramm Html.ValidationMessage() ein Sternchen angezeigt, wenn ein Fehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="30e8f-147">Die Seite in Abbildung 3 veranschaulicht die Fehlermeldungen, die durch die Überprüfung Hilfsprogramme gerendert wird, wenn fehlende Felder mit ungültigen Werten des Formulars senden.</span><span class="sxs-lookup"><span data-stu-id="30e8f-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="30e8f-148">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="30e8f-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="30e8f-149">**Abbildung 03**: Erstellen Sie die Ansicht mit Problemen übermittelt ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="30e8f-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="30e8f-150">Beachten Sie, dass die Darstellung der HTML-Datei die Eingabe-Felder auch geändert werden, wenn ein Validierungsfehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="30e8f-151">Die Html.TextBox() Helper rendert eine *Klasse = "Eingabe-Validierungsfehler"* -Attribut, wenn es ein Validierungsfehler tritt verknüpft sind, mit der Eigenschaft, die von der Html.TextBox() Helper gerendert.</span><span class="sxs-lookup"><span data-stu-id="30e8f-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="30e8f-152">Es gibt drei Klassen cascading Stylesheet verwendet, um die Darstellung von Überprüfungsfehlern zu steuern:</span><span class="sxs-lookup"><span data-stu-id="30e8f-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="30e8f-153">Eingabe-Überprüfung-Fehler - angewendet, um die &lt;input&gt; Tag von Html.TextBox() Helper gerendert.</span><span class="sxs-lookup"><span data-stu-id="30e8f-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="30e8f-154">Feld-Überprüfung-Fehler - angewendet wird, um die &lt;umfassen&gt; Tag, die durch das Hilfsprogramm Html.ValidationMessage() gerendert.</span><span class="sxs-lookup"><span data-stu-id="30e8f-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="30e8f-155">Summary-Validierungsfehler - angewendet wird, um die &lt;Ul&gt; Tag, die durch das Hilfsprogramm Html.ValidationSumamry() gerendert.</span><span class="sxs-lookup"><span data-stu-id="30e8f-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="30e8f-156">Sie können diese Klassen für cascading Stylesheet ändern und aus diesem Grund ändern die Darstellung von Validierungsfehlern, ändern die Datei "Site.CSS" im Ordner "Content".</span><span class="sxs-lookup"><span data-stu-id="30e8f-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="30e8f-157">HtmlHelper-Klasse enthält schreibgeschützte statische Eigenschaften aus, für das Abrufen der Namen der Überprüfung CSS verknüpften Klassen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="30e8f-158">Diese statische Eigenschaften werden ValidationInputCssClassName ValidationFieldCssClassName und ValidationSummaryCssClassName benannt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="30e8f-159">Überprüfung und Postbinding Überprüfung Prebinding</span><span class="sxs-lookup"><span data-stu-id="30e8f-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="30e8f-160">Wenn Sie das HTML-Formular für das Erstellen eines Produkts senden und Sie einen ungültigen Wert für das Preisfeld und keinen Wert für das Feld "UnitsInStock" eingeben, klicken Sie dann erhalten die Überprüfung Nachrichten angezeigt, die in Abbildung 4 Sie.</span><span class="sxs-lookup"><span data-stu-id="30e8f-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="30e8f-161">Woher stammen diese Überprüfungsfehlermeldungen?</span><span class="sxs-lookup"><span data-stu-id="30e8f-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="30e8f-162">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="30e8f-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="30e8f-163">**Abbildung 04**: Validierungsfehler Prebinding ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="30e8f-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="30e8f-164">Es gibt tatsächlich zwei Arten von Validierungsfehlermeldungen - erzeugt werden, bevor die HTML-Felder auf eine Klasse gebunden sind und die generiert werden, nachdem die Felder der Klasse gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="30e8f-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="30e8f-165">Das heißt, es sind Überprüfungsfehler prebinding und postbinding Validierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="30e8f-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="30e8f-166">Die Create()--Aktion, die von der Produkt-Controller im Codebeispiel 1 verfügbar gemacht werden akzeptiert eine Instanz der Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="30e8f-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="30e8f-167">Die Signatur der Methode erstellen, sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="30e8f-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="30e8f-168">Die Werte der HTML-Formularfelder aus dem Formular erstellen gebunden sind durch so genannte einen Modellbinder für die ProductToCreate-Klasse.</span><span class="sxs-lookup"><span data-stu-id="30e8f-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="30e8f-169">Der Standardmodellbinder Fügt eine Fehlermeldung zum Modellzustand automatisch beim Formularfeld nicht auf eine Formulareigenschaft gebunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="30e8f-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="30e8f-170">Der Standardmodellbinder kann nicht die Zeichenfolge "Apple" der Preis-Eigenschaft der Product-Klasse gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="30e8f-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="30e8f-171">Sie können kein decimal-Eigenschaft eine Zeichenfolge zuweisen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="30e8f-172">Daher fügt der Modellbinder Fehler Modellzustand hinzu.</span><span class="sxs-lookup"><span data-stu-id="30e8f-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="30e8f-173">Der Standardmodellbinder kann nicht auch einen null-Wert einer Eigenschaft zuweisen, die keine NULL-Werte annimmt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="30e8f-174">Insbesondere kann nicht der Modellbinder einen null-Wert der Eigenschaft "UnitsInStock" zuweisen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="30e8f-175">Erneut, der Modellbinder aufgibt und Modellzustand eine Fehlermeldung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="30e8f-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="30e8f-176">Wenn Sie die Darstellung dieser anpassen, prebinding Fehlermeldungen möchten müssen Sie Ressourcenzeichenfolgen für diese Nachrichten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="30e8f-177">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="30e8f-177">Summary</span></span>

<span data-ttu-id="30e8f-178">Das Ziel dieses Lernprogramms wurde um die grundlegende Funktionsweise der Validierung in ASP.NET MVC-Framework zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="30e8f-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="30e8f-179">Sie haben gelernt, wie Modellzustand und die Validierung HTML-Hilfsmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="30e8f-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="30e8f-180">Wir besprochen haben auch die Unterscheidung zwischen prebinding und postbinding Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="30e8f-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="30e8f-181">In anderen Lernprogrammen besprechen wir verschiedene Strategien zum Verschieben von Validierungscodes aus Ihrem Domänencontroller und in Ihren Modellklassen.</span><span class="sxs-lookup"><span data-stu-id="30e8f-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="30e8f-182">[Zurück](displaying-a-table-of-database-data-cs.md)
[Weiter](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="30e8f-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
