---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualisieren, löschen und Erstellen von Daten mit modellbindung und WebForms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885844"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="d00d7-104">Aktualisieren, löschen und Erstellen von Daten mit modellbindung und WebForms</span><span class="sxs-lookup"><span data-stu-id="d00d7-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="d00d7-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d00d7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d00d7-106">Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="d00d7-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d00d7-107">Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="d00d7-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d00d7-108">Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d00d7-109">Dieses Lernprogramm veranschaulicht das Erstellen, aktualisieren und Löschen von Daten mit modellbindung.</span><span class="sxs-lookup"><span data-stu-id="d00d7-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="d00d7-110">Sie können die folgenden Eigenschaften festlegen:</span><span class="sxs-lookup"><span data-stu-id="d00d7-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="d00d7-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="d00d7-111">DeleteMethod</span></span>
> - <span data-ttu-id="d00d7-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="d00d7-112">InsertMethod</span></span>
> - <span data-ttu-id="d00d7-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="d00d7-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="d00d7-114">Diese Eigenschaften erhalten den Namen der Methode, die den entsprechenden Vorgang verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="d00d7-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="d00d7-115">Innerhalb dieser Methode können bereitstellen Sie die Logik für die Interaktion mit den Daten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="d00d7-116">Dieses Lernprogramm baut auf das Projekt erstellt, in der ersten [Teil](retrieving-data.md) der Reihe.</span><span class="sxs-lookup"><span data-stu-id="d00d7-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="d00d7-117">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar.</span><span class="sxs-lookup"><span data-stu-id="d00d7-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d00d7-118">Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d00d7-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d00d7-119">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d00d7-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d00d7-120">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="d00d7-120">What you'll build</span></span>

<span data-ttu-id="d00d7-121">In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="d00d7-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d00d7-122">Dynamic Data-Vorlagen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d00d7-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="d00d7-123">Aktivieren Sie aktualisieren und Löschen von Daten über die Bindung Modellmethoden</span><span class="sxs-lookup"><span data-stu-id="d00d7-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="d00d7-124">Anwenden von Validierungsregeln Daten – ermöglichen das Erstellen eines neuen Datensatzes in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="d00d7-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="d00d7-125">Dynamic Data-Vorlagen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d00d7-125">Add dynamic data templates</span></span>

<span data-ttu-id="d00d7-126">Um bestmögliche benutzererfahrung bieten und codewiederholungen minimieren, verwenden Sie dynamische Datenvorlagen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="d00d7-127">Sie können problemlos in Ihre vorhandene Website vorgefertigten dynamic Data-Vorlagen integrieren, durch die Installation von NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="d00d7-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="d00d7-128">Aus der **NuGet-Pakete verwalten**, installieren Sie die **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="d00d7-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![Dynamic Data-Vorlagen](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="d00d7-130">Beachten Sie, dass das Projekt jetzt einen Ordner namens enthält **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="d00d7-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="d00d7-131">In diesem Ordner finden Sie die Vorlagen, die von dynamischen Steuerelementen in WebForms automatisch angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="d00d7-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![Dynamic Data-Ordner](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="d00d7-133">Enable aktualisieren und löschen</span><span class="sxs-lookup"><span data-stu-id="d00d7-133">Enable updating and deleting</span></span>

<span data-ttu-id="d00d7-134">Aktivieren von Benutzern zu aktualisieren und Löschen von Datensätzen in der Datenbank ist sehr ähnlich, an den Prozess zum Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="d00d7-135">In der **UpdateMethod** und **DeleteMethod** Eigenschaften, geben Sie die Namen der Methoden, die diese Vorgänge ausführen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="d00d7-136">Sie können auch Geben Sie die automatische Generierung von bearbeiten und Löschen von Schaltflächen, mit einem GridView-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="d00d7-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="d00d7-137">Die folgende hervorgehobene Code werden die Ergänzungen an die GridView-Code dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d00d7-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="d00d7-138">In der CodeBehind-Datei fügen Sie eine using-Anweisung für **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="d00d7-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="d00d7-139">Anschließend fügen Sie das folgende Update und delete-Methode.</span><span class="sxs-lookup"><span data-stu-id="d00d7-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="d00d7-140">Die **TryUpdateModel** Methode gilt die übereinstimmenden datengebundene Werte aus dem Webformular an, für das Datenelement.</span><span class="sxs-lookup"><span data-stu-id="d00d7-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="d00d7-141">Das Datenelement wird basierend auf den Wert des Id-Parameters abgerufen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="d00d7-142">Erzwingen Sie die überprüfungsanforderungen zu erfüllen</span><span class="sxs-lookup"><span data-stu-id="d00d7-142">Enforce validation requirements</span></span>

<span data-ttu-id="d00d7-143">Die überprüfungsattribute aus, denen auf die Eigenschaften FirstName, LastName und Jahr in der Student-Klasse angewendet werden automatisch erzwungen werden, wenn die Daten zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d00d7-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="d00d7-144">Die DynamicField--Steuerelemente hinzufügen, Client und Server Validierungssteuerelemente, die basierend auf der Validierungsattribute.</span><span class="sxs-lookup"><span data-stu-id="d00d7-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="d00d7-145">Die Eigenschaften FirstName und LastName sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d00d7-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="d00d7-146">FirstName nicht überschreiten 20 Zeichen lang sein und Nachname darf 40 Zeichen nicht überschreiten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="d00d7-147">Jahr muss einen gültigen Wert für die AcademicYear-Enumeration.</span><span class="sxs-lookup"><span data-stu-id="d00d7-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="d00d7-148">Wenn der Benutzer eine die Prüfungsanforderungen verletzt, wird das Update nicht fortgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="d00d7-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="d00d7-149">Fügen Sie ValidationSummary Steuerelement über die GridView hinzu, um die Fehlermeldung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="d00d7-150">Wenn die Validierungsfehler von der modellbindung anzeigen zu können, legen Sie die **ShowModelStateErrors** -Eigenschaftensatz auf **"true"**.</span><span class="sxs-lookup"><span data-stu-id="d00d7-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="d00d7-151">Führen Sie die Webanwendung, und aktualisieren und Löschen von Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualisieren von Daten](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="d00d7-153">Beachten Sie, dass, wenn in den Bearbeitungsmodus automatisch der Wert für die Eigenschaft "Year" als eine Dropdown-Liste gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d00d7-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="d00d7-154">Die Year-Eigenschaft ist ein Enumerationswert, und die dynamische Datenvorlage für einen Enumerationswert gibt eine Dropdownliste für die Bearbeitung.</span><span class="sxs-lookup"><span data-stu-id="d00d7-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="d00d7-155">Sie finden diese Vorlage durch Öffnen der **Enumeration\_Edit.ascx zeigt** in der Datei die **DynamicData**/**FieldTemplates** Ordner.</span><span class="sxs-lookup"><span data-stu-id="d00d7-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="d00d7-156">Wenn Sie gültige Werte angeben, wird das Update erfolgreich abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="d00d7-157">Wenn Sie eine der Anforderungen Überprüfung verletzen, das Update wird nicht fortgesetzt, und folgende Fehlermeldung wird oberhalb des Rasters angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d00d7-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Fehlermeldung](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="d00d7-159">Neue Datensätze hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d00d7-159">Add new records</span></span>

<span data-ttu-id="d00d7-160">GridView-Steuerelement enthält keinen der **InsertMethod** Eigenschaft und kann daher nicht für das Hinzufügen eines neuen Datensatzes mit modellbindung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d00d7-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="d00d7-161">Sie finden die InsertMethod-Eigenschaft in der **FormView**, **DetailsView**, oder **ListView** Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="d00d7-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="d00d7-162">In diesem Lernprogramm verwenden Sie ein FormView-Steuerelement, um einen neuen Datensatz hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="d00d7-163">Fügen Sie zunächst einen Link auf die neue Seite, die Sie erstellen einen neuen Datensatz hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="d00d7-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="d00d7-164">Fügen Sie oberhalb der ValidationSummary:</span><span class="sxs-lookup"><span data-stu-id="d00d7-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="d00d7-165">Der neue Link wird am oberen Rand der Inhalt für die Seite "Students" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d00d7-165">The new link will appear at the top of the content for the Students page.</span></span>

![Link "Neues"](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="d00d7-167">Anschließend fügen Sie eine neue WebForm mithilfe einer Masterseite hinzu, und nennen Sie sie **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="d00d7-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="d00d7-168">Wählen Sie Site.Master, als die Gestaltungsvorlage.</span><span class="sxs-lookup"><span data-stu-id="d00d7-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="d00d7-169">Sie werden die Felder für das Hinzufügen eines neuen Studenten mit Rendern einer **erzeugt wird** Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="d00d7-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="d00d7-170">Das Steuerelement erzeugt wird rendert, bearbeitbaren Eigenschaften in der Klasse, die in der ItemType-Eigenschaft angegeben.</span><span class="sxs-lookup"><span data-stu-id="d00d7-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="d00d7-171">Die StudentID-Eigenschaft wurde gekennzeichnet, mit der **[ScaffoldColumn(false)]** Attribut, damit es nicht gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d00d7-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="d00d7-172">Fügen Sie in den von der Seite "AddStudent" MainContent-Platzhalter den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="d00d7-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="d00d7-173">Fügen Sie in der Code-Behind-Datei (AddStudent.aspx.cs) eine **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.</span><span class="sxs-lookup"><span data-stu-id="d00d7-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="d00d7-174">Anschließend fügen Sie die folgenden Methoden zum Einfügen eines neuen Datensatzes und einen Ereignishandler für die Schaltfläche "Abbrechen" angeben.</span><span class="sxs-lookup"><span data-stu-id="d00d7-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="d00d7-175">Alle Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d00d7-175">Save all of the changes.</span></span>

<span data-ttu-id="d00d7-176">Führen Sie die Webanwendung, und erstellen Sie einen neuen Studenten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-176">Run the web application and create a new student.</span></span>

![Hinzufügen von neuen Studenten](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="d00d7-178">Klicken Sie auf **einfügen** , und beachten Sie die neue Studenten erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d00d7-178">Click **Insert** and notice the new student has been created.</span></span>

![Anzeigen von neuen Studenten](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="d00d7-180">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="d00d7-180">Conclusion</span></span>

<span data-ttu-id="d00d7-181">In diesem Lernprogramm ermöglichte aktualisieren, löschen und Erstellen von Daten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="d00d7-182">Sie wird sichergestellt, dass es sich um eine Überprüfungsregeln angewendet werden, bei der Interaktion mit den Daten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="d00d7-183">In der nächsten [Lernprogramm](sorting-paging-and-filtering-data.md) in dieser Serie, aktivieren Sie sortieren, paging und Filtern von Daten.</span><span class="sxs-lookup"><span data-stu-id="d00d7-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d00d7-184">[Zurück](retrieving-data.md)
> [Weiter](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="d00d7-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
