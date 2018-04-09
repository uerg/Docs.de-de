---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Datenbank zuerst mit ASP.NET MVC: Verbessern der Datenüberprüfung | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="aaf47-104">EF Datenbank zuerst mit ASP.NET MVC: Verbessern der Datenüberprüfung</span><span class="sxs-lookup"><span data-stu-id="aaf47-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="aaf47-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aaf47-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aaf47-106">MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="aaf47-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="aaf47-107">Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="aaf47-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="aaf47-108">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="aaf47-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="aaf47-109">In diesem Teil der Reihe konzentriert sich auf das Hinzufügen von datenanmerkungen in das Datenmodell geben überprüfungsanforderungen zu erfüllen und die Formatierung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="aaf47-110">Es wurde basierend auf Feedback von Benutzern im Abschnitt "Kommentare" verbessert.</span><span class="sxs-lookup"><span data-stu-id="aaf47-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="aaf47-111">Hinzufügen von datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="aaf47-111">Add data annotations</span></span>

<span data-ttu-id="aaf47-112">In einer früheren Thema haben Sie gesehen, werden einige Datenüberprüfungsregeln automatisch auf die Benutzereingabe angewendet.</span><span class="sxs-lookup"><span data-stu-id="aaf47-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="aaf47-113">Beispielsweise können Sie nur eine Zahl für die Dienstqualität Eigenschaft bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="aaf47-114">Um weitere Daten Validierungsregeln angeben, können Sie Ihrer Modellklasse datenanmerkungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="aaf47-115">Diese Anmerkungen werden in der gesamten Webanwendung für die angegebene Eigenschaft angewendet.</span><span class="sxs-lookup"><span data-stu-id="aaf47-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="aaf47-116">Sie können auch die Formatierungsattribute anwenden, die sich ändern, wie die Eigenschaften angezeigt werden; ändern z. B. für Beschriftungen verwendeten Wert ein.</span><span class="sxs-lookup"><span data-stu-id="aaf47-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="aaf47-117">In diesem Lernprogramm fügen Sie datenanmerkungen zum Beschränken der Länge der Werte für die FirstName, LastName und der MiddleName-Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="aaf47-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="aaf47-118">In der Datenbank sind diese Werte auf 50 Zeichen begrenzt. Allerdings wird in der Webanwendung, zeichenbeschränkung derzeit nicht erzwungen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="aaf47-119">Wenn ein Benutzer mehr als 50 Zeichen für einen dieser Werte bereitstellt, stürzt der Seite "beim Versuch, den Wert in der Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="aaf47-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="aaf47-120">Sie werden auch Dienstqualität auf Werte zwischen 0 und 4 einschränken.</span><span class="sxs-lookup"><span data-stu-id="aaf47-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="aaf47-121">Öffnen der **Student.cs** in der Datei die **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="aaf47-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="aaf47-122">Fügen Sie der Klasse folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="aaf47-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="aaf47-123">Fügen Sie in Enrollment.cs den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="aaf47-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="aaf47-124">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="aaf47-124">Build the solution.</span></span>

<span data-ttu-id="aaf47-125">Navigieren Sie zu einer Seite zum Bearbeiten oder ein Teilnehmers zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="aaf47-126">Wenn Sie versuchen, mehr als 50 Zeichen eingeben, wird eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aaf47-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Anzeigen der Fehlermeldung](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="aaf47-128">Suchen Sie die Seite zur Bearbeitung der Registrierung, und versucht, eine Klasse über 4 bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Bereichsfehler für die Dienstqualität](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="aaf47-130">Eine vollständige Liste der datenanmerkungen Überprüfung kann auf Klassen und Eigenschaften angewendet werden soll, finden Sie unter [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="aaf47-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="aaf47-131">Hinzufügen von Metadatenklassen</span><span class="sxs-lookup"><span data-stu-id="aaf47-131">Add metadata classes</span></span>

<span data-ttu-id="aaf47-132">Die überprüfungsattribute direkt die Modellklasse hinzufügen funktioniert, wenn Sie nicht, die beabsichtigen zu ändernde Datenbank; jedoch wenn Änderungen an der Datenbank, und Sie die Modellklasse neu generieren müssen, verlieren alle Attribute Sie den Modell-Klasse angewendet werden mussten.</span><span class="sxs-lookup"><span data-stu-id="aaf47-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="aaf47-133">Dieser Ansatz kann sehr ineffizient und Verlust wichtiger Validierungsregeln fehleranfällig sein.</span><span class="sxs-lookup"><span data-stu-id="aaf47-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="aaf47-134">Um dieses Problem zu vermeiden, können Sie eine Metadatenklasse hinzufügen, die die Attribute enthält.</span><span class="sxs-lookup"><span data-stu-id="aaf47-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="aaf47-135">Wenn Sie den Modellklasse, um die Metadatenklasse zuordnen, werden diese Attribute auf das Modell angewendet.</span><span class="sxs-lookup"><span data-stu-id="aaf47-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="aaf47-136">Bei dieser Vorgehensweise kann der Skriptobjektmodell-Klasse neu generiert werden, ohne dass auch alle Attribute, die auf die Metadatenklasse angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="aaf47-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="aaf47-137">In der **Modelle** Ordner, fügen Sie eine Klasse mit dem Namen **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="aaf47-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![Fügen Sie Metadatenklasse](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="aaf47-139">Ersetzen Sie den Code in Metadata.cs durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="aaf47-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="aaf47-140">Diese Metadatenklassen enthalten alle die überprüfungsattribute aus, denen Sie auf der Modellklassen zuvor schon angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="aaf47-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="aaf47-141">Die **Anzeige** Attribut wird verwendet, um den Wert für Beschriftungen verwendete zu ändern.</span><span class="sxs-lookup"><span data-stu-id="aaf47-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="aaf47-142">Nun müssen Sie die Modellklassen mit den Metadatenklassen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="aaf47-143">In der **Modelle** Ordner, fügen Sie eine Klasse mit dem Namen **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="aaf47-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="aaf47-144">Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="aaf47-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="aaf47-145">Beachten Sie, die jede Klasse als markiert ist eine `partial` -Klasse, und jedes entspricht dem Namen und Namespace wie die Klasse, die automatisch generiert wird.</span><span class="sxs-lookup"><span data-stu-id="aaf47-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="aaf47-146">Das Metadaten-Attribut auf die partielle Klasse anwenden, stellen Sie sicher, dass die überprüfungsattribute Daten auf die automatisch generierte Klasse angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="aaf47-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="aaf47-147">Diese Attribute werden nicht verloren, wenn Sie die Modellklassen neu generieren, da das Metadatenattribut in partielle Klassen angewendet wird, die nicht erneut generiert werden.</span><span class="sxs-lookup"><span data-stu-id="aaf47-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="aaf47-148">Öffnen Sie die ContosoModel.edmx-Datei, um die automatisch generierten Klassen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="aaf47-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="aaf47-149">Noch einmal: mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="aaf47-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="aaf47-150">Obwohl Sie die Datenbank nicht geändert haben, wird dieser Prozess die Klassen erneut generieren.</span><span class="sxs-lookup"><span data-stu-id="aaf47-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="aaf47-151">In der **aktualisieren** Registerkarte **Tabellen** und **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="aaf47-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![Aktualisieren von Tabellen](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="aaf47-153">Speichern Sie die ContosoModel.edmx-Datei, um die Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="aaf47-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="aaf47-154">Öffnen Sie die Datei Student.cs oder Enrollment.cs, und beachten Sie, dass die Validierungsattribute für Daten, die Sie zuvor angewendet nicht mehr in der Datei enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="aaf47-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="aaf47-155">Allerdings wird führen Sie die Anwendung aus, und beachten Sie, dass die Validierungsregeln weiterhin angewendet werden, wenn Sie Daten eingeben.</span><span class="sxs-lookup"><span data-stu-id="aaf47-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aaf47-156">[Zurück](customizing-a-view.md)
> [Weiter](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="aaf47-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
