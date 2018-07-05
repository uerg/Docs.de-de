---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Ausführen einer einfachen Überprüfung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendungen. In diesem Tutorial führt Stephen Walther Sie Modellstatus und den überprüfungshelfer HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 26900229630b25affe21a5bb801ef0247711d26b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399460"
---
<a name="performing-simple-validation-vb"></a><span data-ttu-id="f6332-104">Ausführen einer einfachen Überprüfung (VB)</span><span class="sxs-lookup"><span data-stu-id="f6332-104">Performing Simple Validation (VB)</span></span>
====================
<span data-ttu-id="f6332-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f6332-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f6332-106">Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="f6332-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="f6332-107">In diesem Tutorial führt Stephen Walther Sie Modellzustand sowie die Validierung, HTML-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="f6332-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="f6332-108">Das Ziel in diesem Tutorial wird beschrieben, wie Sie die Validierung in ASP.NET MVC-Anwendungen ausführen können.</span><span class="sxs-lookup"><span data-stu-id="f6332-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="f6332-109">Beispielsweise erfahren Sie, wie Sie verhindern, dass eine Person senden eines Formulars, das nicht über einen Wert für ein erforderliches Feld enthält.</span><span class="sxs-lookup"><span data-stu-id="f6332-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="f6332-110">Erfahren Sie, wie Sie mit der Modellzustand sowie die Überprüfung-HTML-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="f6332-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="f6332-111">Grundlegendes zu Modellstatus</span><span class="sxs-lookup"><span data-stu-id="f6332-111">Understanding Model State</span></span>

<span data-ttu-id="f6332-112">Können Sie Modellstatus – oder genauer gesagt, das Modellzustandswörterbuch - Validierungsfehler darstellen.</span><span class="sxs-lookup"><span data-stu-id="f6332-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="f6332-113">Die Aktion Create() in Codebeispiel 1 überprüft beispielsweise die Eigenschaften der einer Produktklasse vor dem Hinzufügen der Product-Klasse zu einer Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="f6332-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="f6332-114">Ich bin nicht empfehlen, dass Sie Ihre Logik zum Überprüfen oder die Datenbank auf einen Controller hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f6332-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="f6332-115">Ein Controller sollte nur im Zusammenhang mit der Steuerung des Anwendungsflusses Logik enthalten.</span><span class="sxs-lookup"><span data-stu-id="f6332-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="f6332-116">Wir werden eine Verknüpfung mit der Einfachheit halber dauert.</span><span class="sxs-lookup"><span data-stu-id="f6332-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="f6332-117">**1 – Controllers\ProductController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="f6332-117">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

<span data-ttu-id="f6332-118">Im Codebeispiel 1 werden die Eigenschaften Name, Beschreibung und UnitsInStock der Product-Klasse überprüft.</span><span class="sxs-lookup"><span data-stu-id="f6332-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="f6332-119">Wenn eine dieser Eigenschaften einen Überprüfungstest fehlschlagen wird ein Fehler auf das Modellzustandswörterbuch (dargestellt durch die ModelState-Eigenschaft der Controllerklasse) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f6332-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="f6332-120">Wenn Fehler vorhanden, im modellzustands sind gibt die Eigenschaft ModelState.IsValid "false" zurück.</span><span class="sxs-lookup"><span data-stu-id="f6332-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="f6332-121">In diesem Fall wird das HTML-Formular zum Erstellen eines neuen Produkts erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f6332-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="f6332-122">Andernfalls treten keine Validierungsfehler mehr auftreten, wird das neue Produkt mit der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f6332-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="f6332-123">Verwenden die Überprüfung-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="f6332-123">Using the Validation Helpers</span></span>

<span data-ttu-id="f6332-124">ASP.NET MVC-Framework enthält zwei Hilfsmethoden für Überprüfung: das Html.ValidationMessage()-Hilfsobjekt und der Html.ValidationSummary()-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="f6332-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="f6332-125">Verwenden Sie dieses beiden Hilfsprogramme in einer Ansicht, um Überprüfungsfehlermeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f6332-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="f6332-126">Die Hilfsprogramme Html.ValidationMessage() und Html.ValidationSummary() werden in den Bearbeitungs- und änderungsansichten verwendet, die von der ASP.NET-MVC-Gerüstbau automatisch generiert werden.</span><span class="sxs-lookup"><span data-stu-id="f6332-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="f6332-127">Um die Ansicht "erstellen" zu generieren, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="f6332-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="f6332-128">Mit der rechten Maustaste der Create()-Aktion in der Product-Controller, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="f6332-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="f6332-129">In der **Ansicht hinzufügen** Dialogfeld aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="f6332-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="f6332-130">Von der **Datenklasse anzeigen** Dropdownliste wählen Sie die Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6332-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="f6332-131">Von der **Inhalt anzeigen** Dropdown-Liste wählen erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6332-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="f6332-132">Klicken Sie auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f6332-132">Click the **Add** button.</span></span>


<span data-ttu-id="f6332-133">Stellen Sie sicher, dass Sie Ihre Anwendung vor dem Hinzufügen einer Ansicht zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6332-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="f6332-134">Andernfalls nicht die Liste der Klassen angezeigt, der **Datenklasse anzeigen** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="f6332-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="f6332-135">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f6332-135">[![The New Project dialog box](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="f6332-136">**Abbildung 01**: Hinzufügen einer Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f6332-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="f6332-137">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f6332-137">[![The New Project dialog box](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span></span>

<span data-ttu-id="f6332-138">**Abbildung 02**: Erstellen einer stark typisierten Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f6332-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-vb/_static/image4.png))</span></span>


<span data-ttu-id="f6332-139">Nachdem Sie diese Schritte abgeschlossen haben, erhalten Sie die Ansicht "erstellen" in Liste 2.</span><span class="sxs-lookup"><span data-stu-id="f6332-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="f6332-140">**Codebeispiel 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="f6332-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

<span data-ttu-id="f6332-141">Programmausdruck 2 wird das Hilfsprogramm Html.ValidationSummary() sofort über das HTML-Formular aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f6332-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="f6332-142">Dieses Hilfsprogramm wird verwendet, um eine Liste der Überprüfungsfehlermeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f6332-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="f6332-143">Das Hilfsprogramm Html.ValidationSummary() rendert die Fehler in einer Liste mit Aufzählungszeichen.</span><span class="sxs-lookup"><span data-stu-id="f6332-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="f6332-144">Das Hilfsprogramm Html.ValidationMessage() wird neben jeder HTML-Formular Felder bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f6332-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="f6332-145">Dieses Hilfsprogramm wird verwendet, eine Fehlermeldung, die rechts neben einem Formularfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f6332-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="f6332-146">Fall 2 auflisten werden die Html.ValidationMessage()-Hilfsprogramm ein Sternchen angezeigt, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="f6332-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="f6332-147">Die Seite in Abbildung 3 zeigt die Fehlermeldungen, die durch die Überprüfung-Hilfsprogramme gerendert wird, wenn das Formular, fehlende Felder mit ungültigen Werten übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="f6332-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="f6332-148">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f6332-148">[![The New Project dialog box](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span></span>

<span data-ttu-id="f6332-149">**Abbildung 03**: Erstellen Sie die Ansicht mit Problemen übermittelt ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f6332-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-vb/_static/image6.png))</span></span>


<span data-ttu-id="f6332-150">Beachten Sie, dass die Darstellung des HTML-Codes die Eingabe-Felder auch geändert werden, wenn ein Überprüfungsfehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="f6332-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="f6332-151">Die Html.TextBox() Helper rendert eine *Klasse = "Eingabe-Validation-Error"* -Attribut, wenn ein Überprüfungsfehler vorliegt, die mit der Eigenschaft, die vom Hilfsprogramm Html.TextBox() gerendert verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="f6332-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="f6332-152">Es gibt drei Klassen cascading Stylesheet verwendet, um die Darstellung von Validierungsfehlern zu steuern:</span><span class="sxs-lookup"><span data-stu-id="f6332-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="f6332-153">Eingabe-Überprüfungsfehlern - angewendet wird, um die &lt;Eingabe&gt; Tag von Html.TextBox() Helper gerendert.</span><span class="sxs-lookup"><span data-stu-id="f6332-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="f6332-154">Feld-Überprüfungsfehlern - angewendet wird, um die &lt;umfassen&gt; Tag, die vom Hilfsprogramm Html.ValidationMessage() gerendert.</span><span class="sxs-lookup"><span data-stu-id="f6332-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="f6332-155">Summary-Validierungsfehler: angewendet wird, um die &lt;Ul&gt; Tag, die vom Hilfsprogramm Html.ValidationSumamry() gerendert.</span><span class="sxs-lookup"><span data-stu-id="f6332-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="f6332-156">Sie können diese Klassen für cascading Stylesheet ändern und die Darstellung der Fehler bei der Validierung, daher durch Ändern der Datei "Site.CSS", die im Ordner "Content" ändern.</span><span class="sxs-lookup"><span data-stu-id="f6332-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f6332-157">HtmlHelper-Klasse enthält schreibgeschützte statische Eigenschaften Abrufen der Namen der Überprüfung CSS zu verwandten Klassen.</span><span class="sxs-lookup"><span data-stu-id="f6332-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="f6332-158">Diese statische Eigenschaften werden ValidationInputCssClassName ValidationFieldCssClassName und ValidationSummaryCssClassName benannt.</span><span class="sxs-lookup"><span data-stu-id="f6332-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="f6332-159">Prebinding Validierung und Überprüfung der Postbinding</span><span class="sxs-lookup"><span data-stu-id="f6332-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="f6332-160">Wenn Sie das HTML-Formular zum Erstellen eines Produkts senden und Sie einen ungültigen Wert für Feld für den Preis und keinen Wert für das UnitsInStock-Feld eingeben, klicken Sie dann die validierungsmeldungen werden angezeigt, die in Abbildung 4 erhalten Sie.</span><span class="sxs-lookup"><span data-stu-id="f6332-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="f6332-161">Woher kommen diese Überprüfungsfehlermeldungen?</span><span class="sxs-lookup"><span data-stu-id="f6332-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="f6332-162">[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f6332-162">[![The New Project dialog box](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span></span>

<span data-ttu-id="f6332-163">**Abbildung 04**: Validierungsfehler Prebinding ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f6332-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-vb/_static/image8.png))</span></span>


<span data-ttu-id="f6332-164">Es gibt tatsächlich zwei Arten von Meldungen für Validierungsfehler: die generierten werden, bevor die HTML-Formular-Felder zu einer Klasse gebunden sind, und die generiert werden, nachdem die Formularfelder auf die Klasse gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="f6332-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="f6332-165">Das heißt, es sind Überprüfungsfehler prebinding und postbinding Validierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="f6332-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="f6332-166">Die Create()-Aktion, die von der Produkt-Controller in Codebeispiel 1 verfügbar gemacht werden akzeptiert eine Instanz der Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6332-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="f6332-167">Die Signatur der Create-Methode sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="f6332-167">The signature of the Create method looks like this:</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

<span data-ttu-id="f6332-168">Die Werte der HTML-Formularfelder aus dem Formular erstellen, sind der ProductToCreate-Klasse einen Modellbinder vom gebunden.</span><span class="sxs-lookup"><span data-stu-id="f6332-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="f6332-169">Der Standardmodellbinder hinzufügt, eine Fehlermeldung Modellstatus automatisch Wenn ein Formularfeld nicht auf eine Formulareigenschaft gebunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="f6332-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="f6332-170">Die Zeichenfolge "Apple" kann nicht von der Standardmodellbinder der Preis-Eigenschaft der Product-Klasse gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="f6332-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="f6332-171">Sie können keinen decimal-Eigenschaft eine Zeichenfolge zuweisen.</span><span class="sxs-lookup"><span data-stu-id="f6332-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="f6332-172">Daher fügt die modellbindung einen Fehler Modellzustand hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6332-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="f6332-173">Der Standardmodellbinder darf nicht auch den Wert zuweisen "Nothing" einer Eigenschaft, die nicht dem Wert "Nothing" akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="f6332-173">The default model binder also cannot assign the value Nothing to a property that does not accept the value Nothing.</span></span> <span data-ttu-id="f6332-174">Insbesondere kann die modellbindung dem Wert "Nothing" die UnitsInStock keiner Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="f6332-174">In particular, the model binder cannot assign the value Nothing to the UnitsInStock property.</span></span> <span data-ttu-id="f6332-175">Auch hier die modellbindung gibt und Modellzustand eine Fehlermeldung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f6332-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="f6332-176">Wenn Sie die Darstellung dieser anpassen, prebinding Fehlermeldungen möchten müssen Sie Ressourcenzeichenfolgen für diese Nachrichten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6332-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="f6332-177">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f6332-177">Summary</span></span>

<span data-ttu-id="f6332-178">Das Ziel in diesem Tutorial wurde die grundlegende Funktionsweise der Validierung in ASP.NET MVC-Framework beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f6332-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="f6332-179">Sie haben gelernt, wie Modellzustand sowie die Überprüfung-HTML-Hilfsprogramme verwenden.</span><span class="sxs-lookup"><span data-stu-id="f6332-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="f6332-180">Erläutert auch die Unterscheidung zwischen prebinding und postbinding Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="f6332-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="f6332-181">In anderen Lernprogrammen besprechen wir verschiedene Strategien für Ihr Validierungscode aus Ihren Controllern und in Ihren Modellklassen verschieben.</span><span class="sxs-lookup"><span data-stu-id="f6332-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6332-182">[Zurück](displaying-a-table-of-database-data-vb.md)
> [Weiter](validating-with-the-idataerrorinfo-interface-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f6332-182">[Previous](displaying-a-table-of-database-data-vb.md)
[Next](validating-with-the-idataerrorinfo-interface-vb.md)</span></span>
