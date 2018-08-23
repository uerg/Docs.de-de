---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrieren von JQuery UI Datepicker mit modellbindung und Web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 81cc5f010f2c6c7707223679717e34108a5a7aa3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834734"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="519be-104">Integrieren von JQuery UI Datepicker mit modellbindung und Web forms</span><span class="sxs-lookup"><span data-stu-id="519be-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="519be-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="519be-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="519be-106">Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="519be-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="519be-107">Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement).</span><span class="sxs-lookup"><span data-stu-id="519be-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="519be-108">Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.</span><span class="sxs-lookup"><span data-stu-id="519be-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="519be-109">In diesem Tutorial wird gezeigt, wie der JQuery UI hinzufügen ["DatePicker" Widget](http://jqueryui.com/datepicker/) auf einem Web Form, und verwenden modellbindung, um die Datenbank mit dem ausgewählten Wert zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="519be-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="519be-110">Dieses Tutorial baut auf das Projekt erstellt haben, der [erste](retrieving-data.md) und [zweite](updating-deleting-and-creating-data.md) Teilen der Reihe.</span><span class="sxs-lookup"><span data-stu-id="519be-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="519be-111">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB.</span><span class="sxs-lookup"><span data-stu-id="519be-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="519be-112">Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="519be-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="519be-113">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="519be-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="519be-114">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="519be-114">What you'll build</span></span>

<span data-ttu-id="519be-115">In diesem Tutorial müssen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="519be-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="519be-116">Fügen Sie eine Eigenschaft an Ihr Modell zum Aufzeichnen des Studenten Registrierungsdatum</span><span class="sxs-lookup"><span data-stu-id="519be-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="519be-117">Ermöglichen Sie es dem Benutzer für die Verwendung des JQuery UI Datepicker-Widget für die Registrierung Datumsauswahl</span><span class="sxs-lookup"><span data-stu-id="519be-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="519be-118">Regeln für die datenvalidierung für das Registrierungsdatum erzwingen</span><span class="sxs-lookup"><span data-stu-id="519be-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="519be-119">JQuery UI Datepicker-Widgets kann Benutzer ganz einfach ein Datum aus einem Kalender auswählen, die angezeigt werden, wenn das Feld der Benutzer interagiert.</span><span class="sxs-lookup"><span data-stu-id="519be-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="519be-120">Mithilfe dieses Widgets kann praktischer für Benutzer als ein Datum, manuell eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="519be-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="519be-121">Die Integration des Widgets "DatePicker" auf einer Seite, die modellbindung für Datenoperationen verwendet, erfordert nur wenig zusätzliche Arbeit.</span><span class="sxs-lookup"><span data-stu-id="519be-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="519be-122">Fügen Sie eine neue Eigenschaft mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="519be-122">Add a new property to the model</span></span>

<span data-ttu-id="519be-123">Fügen Sie zunächst eine **"DateTime"** Eigenschaft, um Ihren Schüler/Studenten modellieren und migrieren Sie diese Änderung in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="519be-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="519be-124">Open **UniversityModels.cs**, und fügen Sie den hervorgehobenen Code hinzu, mit dem Modell "Student".</span><span class="sxs-lookup"><span data-stu-id="519be-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="519be-125">Die **RangeAttribute** zum Erzwingen von Validierungsregeln für die Eigenschaft enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="519be-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="519be-126">In diesem Tutorial wird davon ausgegangen, dass Contoso University, am 1. Januar 2013 gegründet wurde und frühere Registrierungsdaten daher nicht gültig sind.</span><span class="sxs-lookup"><span data-stu-id="519be-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="519be-127">Fügen Sie im Fenster "Package Management", eine Migration mithilfe des Befehls **hinzufügen-Migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="519be-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="519be-128">Beachten Sie, dass der Code für die Migration die neue Datetime-Spalte der Tabelle "Student" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="519be-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="519be-129">Um den Wert übereinstimmen, den Sie in der RangeAttribute angegeben, fügen Sie einen Standardwert für die neue Spalte hinzu, wie im folgenden hervorgehobenen Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="519be-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="519be-130">Speichern Sie die Änderung, um die Migrationsdatei.</span><span class="sxs-lookup"><span data-stu-id="519be-130">Save your change to the migration file.</span></span>

<span data-ttu-id="519be-131">Sie müssen sich nicht um die Daten erneut zu starten.</span><span class="sxs-lookup"><span data-stu-id="519be-131">You do not need to seed the data again.</span></span> <span data-ttu-id="519be-132">Öffnen Sie daher **Configuration.cs** in den Ordner "Migrations" und entfernen oder kommentieren Sie den Code in die **Ausgangswert** Methode.</span><span class="sxs-lookup"><span data-stu-id="519be-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="519be-133">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="519be-133">Save and close the file.</span></span>

<span data-ttu-id="519be-134">Führen Sie nun den Befehl **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="519be-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="519be-135">Beachten Sie, dass die Spalte, die jetzt in der Datenbank vorhanden ist und alle vorhandenen Datensätze den Standardwert für EnrollmentDate haben.</span><span class="sxs-lookup"><span data-stu-id="519be-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="519be-136">Hinzufügen von dynamischen Steuerelementen für Registrierungsdatum</span><span class="sxs-lookup"><span data-stu-id="519be-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="519be-137">Fügen Sie jetzt Steuerelemente zum Anzeigen und bearbeiten das Registrierungsdatum.</span><span class="sxs-lookup"><span data-stu-id="519be-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="519be-138">An diesem Punkt wird der Wert über einem Textfeld, das bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="519be-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="519be-139">Weiter unten in diesem Tutorial ändern Sie im Textfeld auf die JQuery Datepicker-Widget.</span><span class="sxs-lookup"><span data-stu-id="519be-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="519be-140">Erstens ist es wichtig zu beachten, dass Sie keine Änderung vornehmen müssen die **AddStudent.aspx** Datei.</span><span class="sxs-lookup"><span data-stu-id="519be-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="519be-141">Das Steuerelement erzeugt wird, wird automatisch die neue Eigenschaft gerendert.</span><span class="sxs-lookup"><span data-stu-id="519be-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="519be-142">Open **Students.aspx**, und fügen Sie folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="519be-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="519be-143">Führen Sie die Anwendung, und beachten Sie, dass Sie den Wert des Datums Registrierung festlegen können, indem Sie ein Datum eingeben.</span><span class="sxs-lookup"><span data-stu-id="519be-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="519be-144">Beim Hinzufügen eines neuen Studenten ein:</span><span class="sxs-lookup"><span data-stu-id="519be-144">When adding a new student:</span></span>

![bestimmten Datum](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="519be-146">Alternativ dazu können Sie einen vorhandenen Wert zu bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="519be-146">Or, editing an existing value:</span></span>

![Datum bearbeiten](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="519be-148">Geben das Datum funktioniert, aber es möglicherweise nicht die kundenerfahrung zu sorgen, die Sie bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="519be-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="519be-149">Im nächsten Abschnitt können Sie ein Datum durch einen Kalender auswählen.</span><span class="sxs-lookup"><span data-stu-id="519be-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="519be-150">Installieren von NuGet-Paket zum Arbeiten mit JQuery UI</span><span class="sxs-lookup"><span data-stu-id="519be-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="519be-151">Die **vom UI** NuGet-Paket ermöglicht eine einfache Integration mit der JQuery-UI-Widgets in Ihre Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="519be-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="519be-152">Um dieses Paket zu verwenden, installieren Sie es über NuGet ein.</span><span class="sxs-lookup"><span data-stu-id="519be-152">To use this package, install it through NuGet.</span></span>

![Hinzufügen der vom-Benutzeroberfläche](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="519be-154">Die Version des vom-Benutzeroberfläche, die Sie installieren kann mit der Version von JQuery in Ihre Anwendung in Konflikt stehen.</span><span class="sxs-lookup"><span data-stu-id="519be-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="519be-155">Führen Sie bevor Sie mit diesem Tutorial fortfahren Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="519be-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="519be-156">Wenn Sie einen JavaScript-Fehler auftritt, müssen Sie die JQuery-Version abstimmen.</span><span class="sxs-lookup"><span data-stu-id="519be-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="519be-157">Sie können entweder die erwartete Version von JQuery zu Ihrem Ordner "Scripts" (Version 1.8.2 zum Zeitpunkt des Schreibens in diesem Tutorial) hinzufügen oder Site.master geben den Pfad zu der JQuery-Datei.</span><span class="sxs-lookup"><span data-stu-id="519be-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="519be-158">Anpassen der Vorlage "DateTime", "DatePicker" Widget enthält</span><span class="sxs-lookup"><span data-stu-id="519be-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="519be-159">Die dynamische Datenvorlage für die Bearbeitung eines Datetime-Werts wird das Widget "DatePicker" hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="519be-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="519be-160">Das Widget die Vorlage hinzufügen, wird es automatisch im Formular zum Hinzufügen eines neuen Studenten und in der Rasteransicht für Bearbeitung Schüler/Studenten gerendert.</span><span class="sxs-lookup"><span data-stu-id="519be-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="519be-161">Open **"DateTime"\_Edit.ascx zeigt**, und fügen Sie folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="519be-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="519be-162">Legen Sie in der CodeBehind-Datei die minimalen und maximalen Datumsangaben für "DatePicker" fest.</span><span class="sxs-lookup"><span data-stu-id="519be-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="519be-163">Wenn Sie diese Werte festlegen, werden Sie verhindert, dass Benutzer ungültige Datumsangaben navigieren.</span><span class="sxs-lookup"><span data-stu-id="519be-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="519be-164">Rufen Sie die minimalen und maximalen Werte aus der **RangeAttribute** für die Eigenschaft "DateTime", sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="519be-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="519be-165">Open **"DateTime"\_Edit.ascx.cs**, und fügen Sie folgenden hervorgehobenen Code hinzu, auf der Seite\_Load-Methode.</span><span class="sxs-lookup"><span data-stu-id="519be-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="519be-166">Führen Sie die Webanwendung, und navigieren Sie zu der Seite "AddStudent".</span><span class="sxs-lookup"><span data-stu-id="519be-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="519be-167">Geben Sie Werte für die Felder aus, und beachten Sie, dass wenn Sie im Textfeld für den Enrollment Date klicken, auf der Kalender angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="519be-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Datumsauswahl](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="519be-169">Wählen Sie ein Datum aus, und klicken Sie auf **einfügen**.</span><span class="sxs-lookup"><span data-stu-id="519be-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="519be-170">Die RangeAttribute erzwingt die Überprüfung auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="519be-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="519be-171">Durch die MinDate-Eigenschaft für "DatePicker" festlegen, gelten Sie auch Validierung auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="519be-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="519be-172">Der Kalender gestattet keine Benutzer auf ein Datum vor den Wert der MinDate navigieren.</span><span class="sxs-lookup"><span data-stu-id="519be-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="519be-173">Wenn Sie einen Datensatz in der Rasteransicht bearbeiten, wird der Kalender wird ebenfalls angezeigt.</span><span class="sxs-lookup"><span data-stu-id="519be-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker in GridView-Ansicht](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="519be-175">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="519be-175">Conclusion</span></span>

<span data-ttu-id="519be-176">In diesem Tutorial haben Sie gelernt, wie ein Widget von JQuery in einem Web Form zu integrieren, die modellbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="519be-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="519be-177">In den nächsten [Tutorial](using-query-string-values-to-retrieve-data.md), verwenden Sie einen Abfragezeichenfolgenwert bei der Auswahl.</span><span class="sxs-lookup"><span data-stu-id="519be-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="519be-178">[Zurück](sorting-paging-and-filtering-data.md)
> [Weiter](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="519be-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
