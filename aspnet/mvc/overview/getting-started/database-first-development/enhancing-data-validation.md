---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Database First mit ASP.NET MVC: Optimieren der Datenüberprüfung | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: a328aa8aec2c512d77ddabec31b3785b8742e018
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376713"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="791ec-104">EF Database First mit ASP.NET MVC: Optimieren der Datenüberprüfung</span><span class="sxs-lookup"><span data-stu-id="791ec-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="791ec-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="791ec-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="791ec-106">Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="791ec-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="791ec-107">Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="791ec-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="791ec-108">Der generierte Code entspricht die Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="791ec-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="791ec-109">Dieser Teil der Serie konzentriert sich auf das Hinzufügen von datenanmerkungen in das Datenmodell, geben Sie die überprüfungsanforderungen zu erfüllen und Formatierung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="791ec-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="791ec-110">Es wurde basierend auf dem Feedback von Benutzern im Abschnitt "Kommentare" verbessert.</span><span class="sxs-lookup"><span data-stu-id="791ec-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="791ec-111">Hinzufügen von datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="791ec-111">Add data annotations</span></span>

<span data-ttu-id="791ec-112">In einem vorhergehenden Thema haben Sie gesehen, gelten einige Regeln für die datenvalidierung automatisch auf die Benutzereingabe.</span><span class="sxs-lookup"><span data-stu-id="791ec-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="791ec-113">Beispielsweise können Sie nur eine Zahl für die Eigenschaft auf Unternehmensniveau bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="791ec-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="791ec-114">Um weitere Regeln für die datenvalidierung anzugeben, können Sie Ihrer Modellklasse datenanmerkungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="791ec-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="791ec-115">Diese Anmerkungen werden in Ihre Webanwendung für die angegebene Eigenschaft angewendet.</span><span class="sxs-lookup"><span data-stu-id="791ec-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="791ec-116">Sie können auch Formatierungsattribute anwenden, die sich ändern, wie die Eigenschaften angezeigt werden; z. B. das Ändern des Werts, der für Symboltitel verwendet.</span><span class="sxs-lookup"><span data-stu-id="791ec-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="791ec-117">In diesem Tutorial fügen Sie datenanmerkungen, um die Länge der die Werte für die FirstName, MiddleName und LastName-Eigenschaften zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="791ec-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="791ec-118">In der Datenbank sind diese Werte auf 50 Zeichen begrenzt. Allerdings wird in der Webanwendung, zeichenbeschränkung derzeit nicht erzwungen.</span><span class="sxs-lookup"><span data-stu-id="791ec-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="791ec-119">Wenn ein Benutzer mehr als 50 Zeichen für einen dieser Werte bereitstellt, stürzt die Seite beim Versuch, den Wert in der Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="791ec-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="791ec-120">Sie werden auch auf Unternehmensniveau mit Werten zwischen 0 und 4 einschränken.</span><span class="sxs-lookup"><span data-stu-id="791ec-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="791ec-121">Öffnen der **Student.cs** Datei die **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="791ec-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="791ec-122">Fügen Sie folgenden hervorgehobenen Code zur Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="791ec-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="791ec-123">Fügen Sie in Enrollment.cs folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="791ec-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="791ec-124">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="791ec-124">Build the solution.</span></span>

<span data-ttu-id="791ec-125">Navigieren Sie zu einer Seite zum Bearbeiten oder erstellen einen für Schüler und Studenten.</span><span class="sxs-lookup"><span data-stu-id="791ec-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="791ec-126">Wenn Sie versuchen, mehr als 50 Zeichen eingeben, wird eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="791ec-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Anzeigen der Fehlermeldung](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="791ec-128">Navigieren Sie zu der Seite zum Bearbeiten von Registrierungen und versucht, eine Grade-Eigenschaft über 4 bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="791ec-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Bereichsfehler auf Unternehmensniveau](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="791ec-130">Eine vollständige Liste von datenanmerkungen Überprüfung können Sie auf Eigenschaften und Klassen anwenden, finden Sie unter [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="791ec-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="791ec-131">Hinzufügen von Metadatenklassen</span><span class="sxs-lookup"><span data-stu-id="791ec-131">Add metadata classes</span></span>

<span data-ttu-id="791ec-132">Die Validierungsattribute direkt an die Modellklasse hinzufügen funktioniert, wenn Sie nicht, dass die erwarten zu ändernde Datenbank; aber wenn Ihre Datenbank geändert wird und Sie die Model-Klasse zu generieren müssen, verlieren alle Attribute Sie, die Sie auf die die Model-Klasse angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="791ec-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="791ec-133">Dieser Ansatz kann sehr ineffizient und anfällig für Verlust wichtiger Validierungsregeln sein.</span><span class="sxs-lookup"><span data-stu-id="791ec-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="791ec-134">Um dieses Problem zu vermeiden, können Sie eine Metadatenklasse hinzufügen, die die Attribute enthält.</span><span class="sxs-lookup"><span data-stu-id="791ec-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="791ec-135">Wenn Sie die Model-Klasse, auf die Metadatenklasse zuordnen, werden diese Attribute für das Modell angewendet.</span><span class="sxs-lookup"><span data-stu-id="791ec-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="791ec-136">Bei diesem Ansatz kann die Model-Klasse erneut generiert werden, ohne zu verlieren alle Attribute, die auf die Metadatenklasse angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="791ec-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="791ec-137">In der **Modelle** Ordner, fügen Sie eine Klasse, die mit dem Namen **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="791ec-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![Fügen Sie Metadatenklasse](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="791ec-139">Ersetzen Sie den Code in Metadata.cs, durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="791ec-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="791ec-140">Diese Metadatenklassen enthalten alle die Validierungsattribute, die Sie zuvor für die Modellklassen angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="791ec-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="791ec-141">Die **Anzeige** Attribut zum Ändern des Werts, der für Symboltitel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="791ec-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="791ec-142">Nun müssen Sie die ViewModel-Klassen mit den Metadatenklassen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="791ec-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="791ec-143">In der **Modelle** Ordner, fügen Sie eine Klasse, die mit dem Namen **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="791ec-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="791ec-144">Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="791ec-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="791ec-145">Beachten Sie, die jede Klasse, als markiert ist eine `partial` -Klasse, und jedes entspricht dem Namen und Namespace wie die Klasse, die automatisch generiert wird.</span><span class="sxs-lookup"><span data-stu-id="791ec-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="791ec-146">Das Metadatenattribut auf der partiellen Klasse anwenden, stellen Sie sicher, dass die Attribute für die datenvalidierung der automatisch generierte Klasse angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="791ec-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="791ec-147">Diese Attribute werden nicht verloren, wenn Sie die ViewModel-Klassen neu generieren, da das Attribut von Metadaten in partiellen Klassen angewendet wird, die nicht erneut generiert werden.</span><span class="sxs-lookup"><span data-stu-id="791ec-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="791ec-148">Öffnen Sie die ContosoModel.edmx-Datei, um die automatisch generierte Klassen erneut zu generieren.</span><span class="sxs-lookup"><span data-stu-id="791ec-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="791ec-149">Wieder mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="791ec-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="791ec-150">Auch wenn Sie die Datenbank nicht geändert haben, wird dieser Prozess die Klassen neu generieren.</span><span class="sxs-lookup"><span data-stu-id="791ec-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="791ec-151">In der **aktualisieren** Registerkarte **Tabellen** und **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="791ec-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![Tabellen aktualisieren](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="791ec-153">Speichern Sie die ContosoModel.edmx-Datei, um die Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="791ec-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="791ec-154">Öffnen Sie die Student.cs oder die Enrollment.cs-Datei, und beachten Sie, dass die Attribute für die datenvalidierung, die Sie zuvor angewendet nicht mehr in der Datei.</span><span class="sxs-lookup"><span data-stu-id="791ec-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="791ec-155">Allerdings wird führen Sie die Anwendung aus, und beachten Sie, dass die Validierungsregeln immer noch angewendet werden, wenn Sie Daten eingeben.</span><span class="sxs-lookup"><span data-stu-id="791ec-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="791ec-156">[Zurück](customizing-a-view.md)
> [Weiter](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="791ec-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
