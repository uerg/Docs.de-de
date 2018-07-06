---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualisieren, löschen und Erstellen von Daten mit modellbindung und Web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 1cf9873db177b67927b579def1eedd08e3e9a762
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821631"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="0bfbf-104">Aktualisieren, löschen und Erstellen von Daten mit modellbindung und Web forms</span><span class="sxs-lookup"><span data-stu-id="0bfbf-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="0bfbf-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0bfbf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0bfbf-106">Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="0bfbf-107">Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement).</span><span class="sxs-lookup"><span data-stu-id="0bfbf-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="0bfbf-108">Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="0bfbf-109">Dieses Tutorial veranschaulicht das Erstellen, aktualisieren und Löschen von Daten mit modellbindung.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="0bfbf-110">Sie werden die folgenden Eigenschaften festlegen:</span><span class="sxs-lookup"><span data-stu-id="0bfbf-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="0bfbf-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="0bfbf-111">DeleteMethod</span></span>
> - <span data-ttu-id="0bfbf-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="0bfbf-112">InsertMethod</span></span>
> - <span data-ttu-id="0bfbf-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="0bfbf-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="0bfbf-114">Diese Eigenschaften erhalten den Namen der Methode, die den entsprechenden Vorgang verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="0bfbf-115">In dieser Methode geben Sie die Logik für die Interaktion mit den Daten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="0bfbf-116">Dieses Tutorial baut auf das Projekt erstellt haben, in der ersten [Teil](retrieving-data.md) der Reihe.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="0bfbf-117">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="0bfbf-118">Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="0bfbf-119">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0bfbf-120">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="0bfbf-120">What you'll build</span></span>

<span data-ttu-id="0bfbf-121">In diesem Tutorial müssen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="0bfbf-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="0bfbf-122">Hinzufügen von dynamic Data-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="0bfbf-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="0bfbf-123">Aktivieren Sie aktualisieren und Löschen von Daten über Bindung Objektmodellmethoden</span><span class="sxs-lookup"><span data-stu-id="0bfbf-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="0bfbf-124">Anwenden von Regeln für die datenvalidierung – ermöglichen das Erstellen eines neuen Datensatzes in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="0bfbf-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="0bfbf-125">Hinzufügen von dynamic Data-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="0bfbf-125">Add dynamic data templates</span></span>

<span data-ttu-id="0bfbf-126">Um die bestmögliche benutzererfahrung bieten und minimieren codewiederholungen, verwenden Sie dynamische Datenvorlagen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="0bfbf-127">Sie können vorgefertigte dynamic Data-Vorlagen in Ihrer vorhandenen Website einfach integrieren, durch die Installation von NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="0bfbf-128">Von der **NuGet-Pakete verwalten**, installieren Sie die **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![Dynamic Data-Vorlagen](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="0bfbf-130">Beachten Sie, dass das Projekt nun einen Ordner namens enthält **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="0bfbf-131">In diesem Ordner finden Sie die Vorlagen, die automatisch auf dynamischen Steuerelementen in Ihren Webformularen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![Dynamic Data-Ordner](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="0bfbf-133">Enable aktualisieren und löschen</span><span class="sxs-lookup"><span data-stu-id="0bfbf-133">Enable updating and deleting</span></span>

<span data-ttu-id="0bfbf-134">Aktivieren von Benutzern zum Aktualisieren und Löschen von Datensätzen in der Datenbank ist sehr ähnlich ist, an den Prozess zum Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="0bfbf-135">In der **UpdateMethod** und **DeleteMethod** Eigenschaften, Sie geben Sie die Namen der Methoden, die diese Vorgänge durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="0bfbf-136">Sie können auch Geben Sie die automatische Generierung von bearbeiten und Löschen von Schaltflächen, mit einem GridView-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="0bfbf-137">Der folgende hervorgehobene Code zeigt die der GridView-Code-Erweiterungen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="0bfbf-138">In der CodeBehind-Datei, fügen Sie eine using-Anweisung für **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="0bfbf-139">Anschließend fügen Sie das folgende Update und delete-Methoden.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="0bfbf-140">Die **TryUpdateModel** Methode gilt die entsprechende datengebundene Werte aus dem Webformular, für das Datenelement.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="0bfbf-141">Das Datenelement wird basierend auf den Wert des Id-Parameters abgerufen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="0bfbf-142">Erzwingen Sie die überprüfungsanforderungen zu erfüllen</span><span class="sxs-lookup"><span data-stu-id="0bfbf-142">Enforce validation requirements</span></span>

<span data-ttu-id="0bfbf-143">Die Validierungsattribute, die auf die Eigenschaften "FirstName, LastName und Year" in der Klasse "Student" angewendet werden automatisch erzwungen werden, wenn die Daten aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="0bfbf-144">Die DynamicField--Steuerelemente hinzufügen, Client und Server-Validierungssteuerelemente, die basierend auf den Validierungsattributen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="0bfbf-145">Die FirstName und LastName-Eigenschaften sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="0bfbf-146">FirstName kann nicht mehr, als 20 Zeichen umfassen und darf in "LastName" 40 Zeichen nicht überschreiten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="0bfbf-147">Jahr, muss ein gültiger Wert für die AcademicYear-Enumeration sein.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="0bfbf-148">Wenn der Benutzer eine der Voraussetzungen für Validierung verletzt, wird das Update nicht fortgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="0bfbf-149">Um die Fehlermeldung anzuzeigen, fügen Sie ein ValidationSummary-Steuerelement oben GridView hinzu.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="0bfbf-150">Um Fehler bei der Validierung von der modellbindung anzuzeigen, legen die **ShowModelStateErrors** -Eigenschaftensatz auf **"true"**.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="0bfbf-151">Führen Sie die Webanwendung aktualisieren und Löschen von Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualisieren von Daten](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="0bfbf-153">Beachten Sie, dass, wenn in den Bearbeitungsmodus automatisch der Wert für die Eigenschaft "Year" als eine Dropdownliste gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="0bfbf-154">Die Eigenschaft "Year" ist ein Enumerationswert, und die dynamic Data-Vorlage für einen Enumerationswert gibt eine Dropdown-Liste für die Bearbeitung.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="0bfbf-155">Sie finden diese Vorlage, indem Sie öffnen die **Enumeration\_Edit.ascx zeigt** Datei die **DynamicData**/**"FieldTemplates"** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="0bfbf-156">Wenn Sie gültige Werte angeben, wird das Update erfolgreich abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="0bfbf-157">Wenn Sie eine der Voraussetzungen für Validierung verletzen würden, das Update wird nicht fortgesetzt, und eine Fehlermeldung wird oberhalb des Rasters angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Fehlermeldung](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="0bfbf-159">Neue Datensätze hinzufügen</span><span class="sxs-lookup"><span data-stu-id="0bfbf-159">Add new records</span></span>

<span data-ttu-id="0bfbf-160">Das GridView-Steuerelement enthält keinen der **InsertMethod** Eigenschaft und kann daher nicht für das Hinzufügen eines neuen Datensatzes mit modellbindung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="0bfbf-161">Sie finden die InsertMethod-Eigenschaft in der **FormView**, **DetailsView**, oder **ListView** Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="0bfbf-162">In diesem Tutorial verwenden Sie einen FormView-Steuerelement, um einen neuen Datensatz hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="0bfbf-163">Fügen Sie zunächst einen Link auf die neue Seite, die Sie zum Hinzufügen eines neuen Eintrags erstellen.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="0bfbf-164">Fügen Sie oberhalb der ValidationSummary:</span><span class="sxs-lookup"><span data-stu-id="0bfbf-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="0bfbf-165">Der neue Link wird am oberen Rand der Inhalt für die Schüler/Studenten-Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-165">The new link will appear at the top of the content for the Students page.</span></span>

![Neuer link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="0bfbf-167">Anschließend fügen Sie ein neues Webformular mit einer Masterseite hinzu, und nennen Sie sie **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="0bfbf-168">Wählen Sie als Masterseite Site.Master.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="0bfbf-169">Rendert Sie die Felder zum Hinzufügen eines neuen Studenten mit einem **erzeugt wird** Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="0bfbf-170">Das Steuerelement erzeugt wird, rendert, bearbeitbaren Eigenschaften in der Klasse, die in der ItemType-Eigenschaft angegeben.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="0bfbf-171">Die Eigenschaft "StudentID" wurde mit markiert die **[ScaffoldColumn(false)]** Attribut, damit es nicht gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="0bfbf-172">Fügen Sie im Platzhalter MainContent der AddStudent Seite den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="0bfbf-173">Fügen Sie in der CodeBehind-Datei (AddStudent.aspx.cs) eine **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="0bfbf-174">Anschließend fügen Sie die folgenden Methoden zum angeben, wie zum Einfügen eines neuen Datensatzes und einen Ereignishandler für die Schaltfläche "Abbrechen".</span><span class="sxs-lookup"><span data-stu-id="0bfbf-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="0bfbf-175">Speichern Sie alle Änderungen an.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-175">Save all of the changes.</span></span>

<span data-ttu-id="0bfbf-176">Führen Sie die Webanwendung, und erstellen Sie einen neuen Studenten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-176">Run the web application and create a new student.</span></span>

![Hinzufügen von neuen Studenten](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="0bfbf-178">Klicken Sie auf **einfügen** und beachten Sie, dass der neue Student erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-178">Click **Insert** and notice the new student has been created.</span></span>

![Anzeigen von neuen Studenten](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="0bfbf-180">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="0bfbf-180">Conclusion</span></span>

<span data-ttu-id="0bfbf-181">In diesem Tutorial haben aktiviert Sie aktualisieren, löschen und Erstellen von Daten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="0bfbf-182">Sie wird sichergestellt, dass Validierungsregeln angewendet werden, bei der Interaktion mit den Daten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="0bfbf-183">In den nächsten [Tutorial](sorting-paging-and-filtering-data.md) in dieser Reihe, aktivieren Sie sortieren, paging und Filtern von Daten.</span><span class="sxs-lookup"><span data-stu-id="0bfbf-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0bfbf-184">[Zurück](retrieving-data.md)
> [Weiter](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="0bfbf-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
