---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: "Die Überprüfung der Daten Anmerkung Validierungssteuerelemente (c#) | Microsoft Docs"
author: microsoft
description: Nutzen Sie die Daten Anmerkung Modellbinder validiert innerhalb einer ASP.NET MVC-Anwendung. Erfahren Sie, wie die verschiedenen Typen von Validierungssteuerelement verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 306dcb0197dfc9317ea9665dd2b1c058ba8bd712
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="d0f47-104">Die Überprüfung der Daten Anmerkung Validierungssteuerelemente (c#)</span><span class="sxs-lookup"><span data-stu-id="d0f47-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="d0f47-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d0f47-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d0f47-106">Nutzen Sie die Daten Anmerkung Modellbinder validiert innerhalb einer ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d0f47-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="d0f47-107">Erfahren Sie, wie die verschiedenen Typen von Validierungssteuerelement-Attribute verwenden und mit ihnen arbeiten, im Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d0f47-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="d0f47-108">In diesem Lernprogramm erfahren Sie, wie die Validierungssteuerelemente-Datenanmerkung die Validierung in ASP.NET MVC-Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0f47-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="d0f47-109">Der Vorteil der Verwendung der Validierungssteuerelemente-Datenanmerkung ist die Möglichkeit, einfach durch Hinzufügen von einem oder mehreren Attributen – z. B. die erforderliche oder StringLength-Attribut – an Klasseneigenschaften-Objekt validiert.</span><span class="sxs-lookup"><span data-stu-id="d0f47-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="d0f47-110">Bevor Sie die Validierungssteuerelemente-Datenanmerkung verwenden können, müssen Sie den Daten Anmerkungen Modellbinder herunterladen.</span><span class="sxs-lookup"><span data-stu-id="d0f47-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="d0f47-111">Sie können Anmerkungen Modell Binder Datenbeispiel von der CodePlex-Website herunterladen, indem Sie auf [hier](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="d0f47-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="d0f47-112">Es ist wichtig zu verstehen, dass die Daten Anmerkungen Modellbinder nicht offizieller Bestandteil des Microsoft ASP.NET MVC-Framework ist.</span><span class="sxs-lookup"><span data-stu-id="d0f47-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="d0f47-113">Obwohl die Daten Anmerkungen Modellbinder vom Microsoft ASP.NET MVC-Team erstellt wurde, bietet Microsoft keine offizielle Microsoft-Support für den Modellbinder für Daten Anmerkungen beschrieben und in diesem Lernprogramm verwendet.</span><span class="sxs-lookup"><span data-stu-id="d0f47-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="d0f47-114">Mithilfe der Modellbindung des Daten-Anmerkung</span><span class="sxs-lookup"><span data-stu-id="d0f47-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="d0f47-115">Um die Daten Anmerkungen Modellbinder in einer ASP.NET MVC-Anwendung verwenden zu können, müssen Sie zunächst einen Verweis auf die Microsoft.Web.Mvc.DataAnnotations.dll-Assembly und die System.ComponentModel.DataAnnotations.dll-Assembly hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d0f47-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="d0f47-116">Wählen Sie die Menüoption **Projekt "," Verweis hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d0f47-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="d0f47-117">Klicken Sie anschließend auf die **Durchsuchen** Registerkarte, und navigieren Sie zum Speicherort, in dem heruntergeladen (und entzippt) von Daten Anmerkungen Modellbinder-Beispiel (finden Sie unter **Abbildung 1**).</span><span class="sxs-lookup"><span data-stu-id="d0f47-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="d0f47-118">**Abbildung 1**: Hinzufügen eines Verweises auf den Daten Anmerkungen Modellbinder ([klicken Sie hier, um das Bild in voller Größe angezeigt](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d0f47-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="d0f47-119">Wählen Sie die Microsoft.Web.Mvc.DataAnnotations.dll-Assembly und die System.ComponentModel.DataAnnotations.dll-Assembly, und klicken Sie auf die **OK** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="d0f47-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="d0f47-120">Sie können keine System.ComponentModel.DataAnnotations.dll-Assembly, die mit .NET Framework Service Pack 1 mit den Daten Anmerkungen Modellbinder enthalten.</span><span class="sxs-lookup"><span data-stu-id="d0f47-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="d0f47-121">Sie müssen die Version der Assembly System.ComponentModel.DataAnnotations.dll im Anmerkungen Modell Binder Datenbeispiel Download verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0f47-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="d0f47-122">Schließlich müssen Sie die DataAnnotations-Modellbindung in der Datei "Global.asax" zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="d0f47-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="d0f47-123">Fügen Sie die folgende Zeile des Codes der Anwendung\_Start()-Ereignishandler, damit die Anwendung\_Methode Start() sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d0f47-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="d0f47-124">Diese Codezeile registriert die AtaAnnotationsModelBinder als den Standardmodellbinder für die gesamte ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d0f47-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="d0f47-125">Hierbei werden die Daten Anmerkung Validierungssteuerelement-Attribute</span><span class="sxs-lookup"><span data-stu-id="d0f47-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="d0f47-126">Wenn Sie den Modellbinder für Daten Anmerkungen verwenden, verwenden Sie die Validierungssteuerelement Attribute validiert.</span><span class="sxs-lookup"><span data-stu-id="d0f47-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="d0f47-127">System.ComponentModel.DataAnnotations-Namespace enthält die folgenden Validierungssteuerelement-Attribute:</span><span class="sxs-lookup"><span data-stu-id="d0f47-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="d0f47-128">Im Bereich – können Sie überprüfen, ob der Wert einer Eigenschaft zwischen einem angegebenen Bereich von Werten liegt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="d0f47-129">Der reguläre Ausdruck – können Sie überprüfen, ob der Wert einer Eigenschaft ein angegebenes reguläres Ausdrucksmuster übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="d0f47-130">Erforderlich – ermöglicht es Ihnen, eine Eigenschaft als erforderlich markieren.</span><span class="sxs-lookup"><span data-stu-id="d0f47-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="d0f47-131">StringLength – ermöglicht Ihnen die Angabe eine maximale Länge für eine Zeichenfolgeneigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d0f47-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="d0f47-132">Überprüfung: die Basisklasse für alle Validierungssteuerelement-Attribute.</span><span class="sxs-lookup"><span data-stu-id="d0f47-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d0f47-133">Wenn Ihre Anforderungen für die Validierung mithilfe einer standardmäßigen Validierungssteuerelemente nicht erfüllt werden müssen Sie immer die Möglichkeit, erstellen ein benutzerdefiniertes Validierungssteuerelement-Attribut durch ein neues Validierungssteuerelement-Attribut aus der Überprüfung Basisattribut erben.</span><span class="sxs-lookup"><span data-stu-id="d0f47-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="d0f47-134">Die Produktklasse in **Codebeispiel 1** wird veranschaulicht, wie diese Attribute Validierungssteuerelement verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0f47-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="d0f47-135">Die Eigenschaften Name, Beschreibung und UnitPrice gekennzeichnet werden nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="d0f47-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="d0f47-136">Die Name-Eigenschaft muss ein String-length-Funktion haben, die weniger als 10 Zeichen lang ist.</span><span class="sxs-lookup"><span data-stu-id="d0f47-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="d0f47-137">Die UnitPrice-Eigenschaft muss schließlich Muster eines regulären Ausdrucks übereinstimmen, das einen Währungsbetrag darstellt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="d0f47-138">**Auflisten von 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="d0f47-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="d0f47-139">Die Produktklasse zeigt, wie ein zusätzliches Attribut: das DisplayName-Attribut.</span><span class="sxs-lookup"><span data-stu-id="d0f47-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="d0f47-140">Das DisplayName-Attribut können Sie den Namen der Eigenschaft ändern, wenn die Eigenschaft in einer Fehlermeldung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d0f47-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="d0f47-141">Anstelle der Anzeige der Fehlermeldung "das UnitPrice-Feld ist erforderlich" können Sie anzeigen, der Fehlermeldung "das Price-Feld ist erforderlich".</span><span class="sxs-lookup"><span data-stu-id="d0f47-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d0f47-142">Wenn die Fehlermeldung angezeigt, indem ein Validator vollständig angepasst werden soll, können Sie eine benutzerdefinierte Fehlermeldung an das Validierungssteuerelement "ErrorMessage"-Eigenschaft wie folgt zuweisen:`<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="d0f47-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="d0f47-143">Können Sie die Produktklasse in **Codebeispiel 1** mit Create()-Controlleraktion in **auflisten 2**.</span><span class="sxs-lookup"><span data-stu-id="d0f47-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="d0f47-144">Diese Controlleraktion erneut die Erstellungsansicht an, wenn Modellstatus alle Fehler enthält.</span><span class="sxs-lookup"><span data-stu-id="d0f47-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="d0f47-145">**Auflisten von 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="d0f47-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="d0f47-146">Schließlich erstellen Sie die Ansicht im **auflisten 3** durch die Create()--Aktion mit der rechten Maustaste, und wählen die Menüoption **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d0f47-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="d0f47-147">Erstellen Sie eine stark typisierte Ansicht mit der Produkt-Klasse, wie die Modellklasse.</span><span class="sxs-lookup"><span data-stu-id="d0f47-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="d0f47-148">Wählen Sie **erstellen** aus der Dropdownliste der Ansicht Inhalt (finden Sie unter **Abbildung 2**).</span><span class="sxs-lookup"><span data-stu-id="d0f47-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="d0f47-149">**Abbildung 2**: die Erstellungsansicht hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d0f47-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="d0f47-150">**Auflisten von 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="d0f47-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d0f47-151">Entfernen Sie das Feld "Id" aus dem Formular erstellen, die generiert werden, indem Sie die **Ansicht hinzufügen** Option des Menüs.</span><span class="sxs-lookup"><span data-stu-id="d0f47-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="d0f47-152">Da das Feld "Id" eine Identity-Spalte entspricht, möchten Sie Benutzern ermöglichen, einen Wert für dieses Feld einzugeben.</span><span class="sxs-lookup"><span data-stu-id="d0f47-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="d0f47-153">Wenn Sie das Formular für das Erstellen eines Produkts übermitteln und Werte für die erforderlichen Felder eingeben, der Validierungsfehler Nachrichten **Abbildung 3** werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="d0f47-154">**Abbildung 3**: Pflichtfelder fehlt</span><span class="sxs-lookup"><span data-stu-id="d0f47-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="d0f47-155">Bei der Eingabe eine ungültige Währungsbetrag, und klicken Sie dann auf die Fehlermeldung in **Abbildung 4** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="d0f47-156">**Abbildung 4**: Ungültige Währungsbetrag</span><span class="sxs-lookup"><span data-stu-id="d0f47-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="d0f47-157">Verwenden von Daten Anmerkung Validierungssteuerelemente mit Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d0f47-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="d0f47-158">Wenn Sie Microsoft Entity Framework verwenden, um Ihren Modellklassen Daten generieren können nicht Sie das Validierungssteuerelement Attribute direkt in Ihren Klassen anwenden.</span><span class="sxs-lookup"><span data-stu-id="d0f47-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="d0f47-159">Da Entity Framework-Designer die Modellklassen generiert werden, werden alle vorgenommenen Änderungen an der Modellklassen das nächste Mal überschrieben werden vorgenommenen Änderungen im Designer.</span><span class="sxs-lookup"><span data-stu-id="d0f47-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="d0f47-160">Wenn Sie mit den vom Entity Framework generierten Klassen die Validierungssteuerelemente verwenden möchten, müssen Sie Meta-Datenklassen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d0f47-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="d0f47-161">Sie gelten die Validierungssteuerelemente für Meta Datenklasse statt der Validierungssteuerelemente auf die tatsächliche Klasse anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="d0f47-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="d0f47-162">Angenommen, dass Sie eine Film-Klasse, die mit dem Entity Framework erstellt haben (siehe **Abbildung 5**).</span><span class="sxs-lookup"><span data-stu-id="d0f47-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="d0f47-163">Angenommen Sie, darüber hinaus, dass der Filmtitel und Director erforderlichen Eigenschaften machen möchten.</span><span class="sxs-lookup"><span data-stu-id="d0f47-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="d0f47-164">In diesem Fall können Sie erstellen die partielle Klasse und die Datenklasse in Meta **auflisten 4**.</span><span class="sxs-lookup"><span data-stu-id="d0f47-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="d0f47-165">**Abbildung 5**: Film-Klasse, die vom Entity Framework generierten</span><span class="sxs-lookup"><span data-stu-id="d0f47-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="d0f47-166">**Auflisten von 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="d0f47-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="d0f47-167">Die Datei im **auflisten 4** enthält zwei Klassen, die mit dem Namen Film und MovieMetaData aus.</span><span class="sxs-lookup"><span data-stu-id="d0f47-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="d0f47-168">Die Film-Klasse ist eine partielle Klasse.</span><span class="sxs-lookup"><span data-stu-id="d0f47-168">The Movie class is a partial class.</span></span> <span data-ttu-id="d0f47-169">Er entspricht der partiellen Klasse generiert, die vom Entity Framework, die in der Datei DataModel.Designer.vb enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="d0f47-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="d0f47-170">Derzeit werden teilweise Eigenschaften von .NET Framework nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="d0f47-171">Daher besteht keine Möglichkeit, auf die Eigenschaften der Film-Klasse, die in der Datei DataModel.Designer.vb definiert, durch Anwenden der Validierungssteuerelement-Attribute auf die Eigenschaften der Film-Klasse, die in der Datei definiert die Validierungssteuerelement-Attribute gelten **auflisten 4**.</span><span class="sxs-lookup"><span data-stu-id="d0f47-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="d0f47-172">Beachten Sie, dass die Teilklasse Film mit einem MetadataType-Attribut ergänzt wird, der auf die Klasse MovieMetaData aus zeigt.</span><span class="sxs-lookup"><span data-stu-id="d0f47-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="d0f47-173">Die MovieMetaData aus-Klasse enthält Eigenschaften von Netzwerkproxy für die Eigenschaften der Film-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d0f47-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="d0f47-174">Die Validierungssteuerelement-Attribute werden den Eigenschaften der Klasse MovieMetaData aus angewendet.</span><span class="sxs-lookup"><span data-stu-id="d0f47-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="d0f47-175">Die Eigenschaften für Titel, Director und DateReleased, die alle als erforderlichen Eigenschaften gekennzeichnet sind.</span><span class="sxs-lookup"><span data-stu-id="d0f47-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="d0f47-176">Die Director-Eigenschaft muss eine Zeichenfolge zugewiesen werden, die weniger als 5 Zeichen enthält.</span><span class="sxs-lookup"><span data-stu-id="d0f47-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="d0f47-177">Schließlich das DisplayName-Attribut angewendet wird, und die DateReleased-Eigenschaft zeigt eine Fehlermeldung wie "das Feld Date freigegeben ist erforderlich."</span><span class="sxs-lookup"><span data-stu-id="d0f47-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="d0f47-178">statt den Fehler ist"das Feld DateReleased erforderlich."</span><span class="sxs-lookup"><span data-stu-id="d0f47-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d0f47-179">Beachten Sie, dass die Proxyeigenschaften in der Klasse MovieMetaData aus nicht benötigen, um dieselben Typen wie die entsprechenden Eigenschaften in der Klasse Film darzustellen.</span><span class="sxs-lookup"><span data-stu-id="d0f47-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="d0f47-180">Beispielsweise ist die Director-Eigenschaft eine Zeichenfolgeneigenschaft, in der Film-Klasse und eine Eigenschaft des Objekts in der Klasse MovieMetaData aus.</span><span class="sxs-lookup"><span data-stu-id="d0f47-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="d0f47-181">Die Seite in **Abbildung 6** veranschaulicht die Fehlermeldungen zurückgegeben, wenn Sie ungültige Werte für die Film-Eigenschaften eingeben.</span><span class="sxs-lookup"><span data-stu-id="d0f47-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="d0f47-182">**Abbildung 6**: Entity Framework mit Validierungssteuerelemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="d0f47-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="d0f47-183">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d0f47-183">Summary</span></span>

<span data-ttu-id="d0f47-184">In diesem Lernprogramm haben Sie gelernt, wie die Daten Anmerkung Modellbinder validiert innerhalb einer ASP.NET MVC-Anwendung nutzen.</span><span class="sxs-lookup"><span data-stu-id="d0f47-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="d0f47-185">Sie haben gelernt, wie die verschiedenen Typen von Validierungssteuerelement-Attribute, z. B. die erforderlichen und StringLength-Attribute verwendet.</span><span class="sxs-lookup"><span data-stu-id="d0f47-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="d0f47-186">Außerdem haben Sie gelernt, diese Attribute bei der Arbeit mit Microsoft Entity Framework nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0f47-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d0f47-187">[Zurück](validating-with-a-service-layer-cs.md)
[Weiter](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d0f47-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
