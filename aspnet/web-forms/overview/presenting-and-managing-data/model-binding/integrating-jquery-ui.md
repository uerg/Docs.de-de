---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrieren von JQuery UI Datepicker mit wurden die modellbindung und WebForms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: da3c8f347a709a4c9a47fd0ecce5201d9b0cd1b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="3c36b-104">Integrieren von JQuery UI Datepicker mit wurden die modellbindung und WebForms</span><span class="sxs-lookup"><span data-stu-id="3c36b-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="3c36b-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3c36b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3c36b-106">Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="3c36b-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="3c36b-107">Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="3c36b-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="3c36b-108">Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="3c36b-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="3c36b-109">Dieses Lernprogramm veranschaulicht das Hinzufügen der JQuery UI [Datepicker-Widget](http://jqueryui.com/datepicker/) in ein Web Form, und verwenden Modell Binden an um die Datenbank mit dem ausgewählten Wert zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="3c36b-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="3c36b-110">Dieses Lernprogramm baut auf das Projekt erstellt, der [erste](retrieving-data.md) und [zweite](updating-deleting-and-creating-data.md) Teile der Reihe.</span><span class="sxs-lookup"><span data-stu-id="3c36b-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="3c36b-111">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar.</span><span class="sxs-lookup"><span data-stu-id="3c36b-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="3c36b-112">Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3c36b-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="3c36b-113">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="3c36b-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="3c36b-114">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="3c36b-114">What you'll build</span></span>

<span data-ttu-id="3c36b-115">In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="3c36b-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="3c36b-116">Fügen Sie eine Eigenschaft mit dem Modell die Studenten Registrierungsdatum aufzeichnen</span><span class="sxs-lookup"><span data-stu-id="3c36b-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="3c36b-117">Ermöglichen Sie dem Benutzer, wählen Sie das Registrierungsdatum mithilfe der JQuery UI Datepicker-widget</span><span class="sxs-lookup"><span data-stu-id="3c36b-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="3c36b-118">Überprüfungsregeln für das Registrierungsdatum erzwingen</span><span class="sxs-lookup"><span data-stu-id="3c36b-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="3c36b-119">Das Widget JQuery UI Datepicker kann Benutzer problemlos ein Datum aus dem Kalender auswählen, die angezeigt werden, wenn der Benutzer mit dem Feld interagiert.</span><span class="sxs-lookup"><span data-stu-id="3c36b-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="3c36b-120">Mithilfe dieses Widgets kann einfacher für Benutzer als ein Datum, manuell eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="3c36b-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="3c36b-121">Integrieren von Datepicker Widgets in einer Seite, die wurden die modellbindung für Datenvorgänge verwendet, ist nur eine kleine Menge an zusätzliche Arbeit erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3c36b-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="3c36b-122">Fügen Sie eine neue Eigenschaft mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="3c36b-122">Add a new property to the model</span></span>

<span data-ttu-id="3c36b-123">Fügen Sie zunächst eine **"DateTime"** Eigenschaft, um Ihre Student modellieren und migrieren Sie diese Änderung in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3c36b-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="3c36b-124">Open **UniversityModels.cs**, und das Modell Student den hervorgehobenen Code hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="3c36b-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="3c36b-125">Die **RangeAttribute** zum Erzwingen von Validierungsregeln für die Eigenschaft enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="3c36b-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="3c36b-126">In diesem Lernprogramm wird davon ausgegangen, dass Contoso University am 1. Januar 2013 gegründet wurde und frühere Registrierungsdaten daher nicht gültig sind.</span><span class="sxs-lookup"><span data-stu-id="3c36b-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="3c36b-127">Fügen Sie im Fenster Paketverwaltung eine Migration mithilfe des Befehls **hinzufügen Migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="3c36b-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="3c36b-128">Beachten Sie, dass der Code für die Migration der neue Datetime-Spalte der Student-Tabelle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3c36b-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="3c36b-129">Um mit dem Wert überein, den Sie in der RangeAttribute angegeben haben, fügen Sie einen Standardwert für die neue Spalte, wie in den folgenden hervorgehobenen Code dargestellt.</span><span class="sxs-lookup"><span data-stu-id="3c36b-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="3c36b-130">Speichern Sie die Änderung in der Migrationsdatei.</span><span class="sxs-lookup"><span data-stu-id="3c36b-130">Save your change to the migration file.</span></span>

<span data-ttu-id="3c36b-131">Sie müssen nicht erneut Ausgangswert für die Daten.</span><span class="sxs-lookup"><span data-stu-id="3c36b-131">You do not need to seed the data again.</span></span> <span data-ttu-id="3c36b-132">Öffnen Sie deshalb **"Configuration.cs"** in den Ordner und entfernen, oder kommentieren Sie den Code in der **Ausgangswert** Methode.</span><span class="sxs-lookup"><span data-stu-id="3c36b-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="3c36b-133">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="3c36b-133">Save and close the file.</span></span>

<span data-ttu-id="3c36b-134">Führen Sie nun den Befehl **Update-Database '**.</span><span class="sxs-lookup"><span data-stu-id="3c36b-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="3c36b-135">Beachten Sie, dass die Spalte in der Datenbank und aller vorhandenen Datensätze jetzt vorhanden ist der Standardwert für EnrollmentDate aufweisen.</span><span class="sxs-lookup"><span data-stu-id="3c36b-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="3c36b-136">Hinzufügen von dynamischen Steuerelementen für Registrierungsdatum</span><span class="sxs-lookup"><span data-stu-id="3c36b-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="3c36b-137">Fügen Sie jetzt Steuerelemente zum Anzeigen und Bearbeiten der Registrierungsdatum.</span><span class="sxs-lookup"><span data-stu-id="3c36b-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="3c36b-138">An diesem Punkt wird der Wert durch ein Textfeld, das bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="3c36b-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="3c36b-139">Später in diesem Lernprogramm ändern Sie im Textfeld auf die JQuery Datepicker-Widget.</span><span class="sxs-lookup"><span data-stu-id="3c36b-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="3c36b-140">Zunächst ist es wichtig zu beachten, dass Sie nicht ändern, müssen die **AddStudent.aspx** Datei.</span><span class="sxs-lookup"><span data-stu-id="3c36b-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="3c36b-141">Das Steuerelement erzeugt wird, wird automatisch die neue Eigenschaft gerendert.</span><span class="sxs-lookup"><span data-stu-id="3c36b-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="3c36b-142">Open **Students.aspx**, und fügen Sie den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c36b-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="3c36b-143">Führen Sie die Anwendung, und beachten Sie, dass Sie den Wert des Datums Registrierung festlegen können, indem Sie ein Datum eingeben.</span><span class="sxs-lookup"><span data-stu-id="3c36b-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="3c36b-144">Wenn Sie einen neuen Studenten hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="3c36b-144">When adding a new student:</span></span>

![Zeitpunkt](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="3c36b-146">Oder Bearbeiten eines vorhandenen Werts:</span><span class="sxs-lookup"><span data-stu-id="3c36b-146">Or, editing an existing value:</span></span>

![Datum bearbeiten](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="3c36b-148">Geben Sie die Date-funktioniert, aber es möglicherweise nicht der benutzerfreundlichkeit, die Sie bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="3c36b-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="3c36b-149">Aktivieren Sie im nächsten Abschnitt ein Datum durch einen Kalender auswählen.</span><span class="sxs-lookup"><span data-stu-id="3c36b-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="3c36b-150">Installieren von NuGet-Paket zum Arbeiten mit JQuery UI</span><span class="sxs-lookup"><span data-stu-id="3c36b-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="3c36b-151">Die **vom UI** NuGet-Paket ermöglicht eine einfache Integration der JQuery UI-Komponenten in die Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="3c36b-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="3c36b-152">Um dieses Paket zu verwenden, installieren Sie es über NuGet.</span><span class="sxs-lookup"><span data-stu-id="3c36b-152">To use this package, install it through NuGet.</span></span>

![Fügen Sie vom UI hinzu.](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="3c36b-154">Die Version des vom UI, die Sie installieren möglicherweise in Konflikt mit der Version von JQuery in Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3c36b-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="3c36b-155">Bevor Sie mit diesem Lernprogramm fortfahren, wiederholen Sie dann ein Ausführen Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3c36b-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="3c36b-156">Wenn Sie einen JavaScript-Fehler auftritt, müssen Sie die JQuery-Version abstimmen.</span><span class="sxs-lookup"><span data-stu-id="3c36b-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="3c36b-157">Sie können entweder die erwartete Version von JQuery Ordner "Skripts" (Version 1.8.2 zum Zeitpunkt des Schreibens dieses Lernprogramms) hinzufügen oder in Site.master Geben Sie den Pfad zu der JQuery-Datei.</span><span class="sxs-lookup"><span data-stu-id="3c36b-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="3c36b-158">Passen Sie an "DateTime" Vorlage Einbeziehung Datepicker-widget</span><span class="sxs-lookup"><span data-stu-id="3c36b-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="3c36b-159">Die dynamische Datenvorlage für die Bearbeitung eines Datetime-Werts wird das Widget Datepicker hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="3c36b-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="3c36b-160">Das Widget die Vorlage hinzufügen, wird es automatisch im Formular zum Hinzufügen eines neuen Studenten und in der Rasteransicht für Bearbeitung Studenten gerendert.</span><span class="sxs-lookup"><span data-stu-id="3c36b-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="3c36b-161">Open **"DateTime"\_Edit.ascx zeigt**, und fügen Sie den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c36b-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="3c36b-162">In der CodeBehind-Datei wird die minimalen und maximalen Datumsangaben für die DatePicker festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="3c36b-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="3c36b-163">Wenn Sie diese Werte festlegen, werden Sie verhindern, dass Benutzer Navigieren auf ungültige Datumsangaben.</span><span class="sxs-lookup"><span data-stu-id="3c36b-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="3c36b-164">Rufen Sie die minimalen und maximalen Werte aus den **RangeAttribute** für die Eigenschaft "DateTime", sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="3c36b-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="3c36b-165">Open **"DateTime"\_Edit.ascx.cs**, und fügen Sie folgenden hervorgehobenen Code hinzu, auf der Seite\_Load-Methode.</span><span class="sxs-lookup"><span data-stu-id="3c36b-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="3c36b-166">Führen Sie die Webanwendung, und navigieren Sie zu der Seite "AddStudent".</span><span class="sxs-lookup"><span data-stu-id="3c36b-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="3c36b-167">Geben Sie Werte für die Felder aus, und beachten Sie, dass beim Klicken auf das Textfeld für Registrierungsdatum Kalender angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="3c36b-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Datumsauswahl](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="3c36b-169">Wählen Sie ein Datum aus, und klicken Sie auf **einfügen**.</span><span class="sxs-lookup"><span data-stu-id="3c36b-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="3c36b-170">Die RangeAttribute erzwingt die Überprüfung auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="3c36b-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="3c36b-171">Indem die Datepicker MinDate-Eigenschaft festlegen, gelten Sie auch Validierung auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="3c36b-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="3c36b-172">Der Kalender ermöglicht kein Benutzer auf ein Datum vor dem Wert der MinDate navigieren.</span><span class="sxs-lookup"><span data-stu-id="3c36b-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="3c36b-173">Wenn Sie einen Datensatz in der Rasteransicht bearbeiten, wird der Kalender ebenfalls angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3c36b-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="3c36b-175">Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="3c36b-175">Conclusion</span></span>

<span data-ttu-id="3c36b-176">In diesem Lernprogramm haben Sie gelernt, wie eine JQuery-Widget in ein Webformular integriert, das modellbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3c36b-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="3c36b-177">In der nächsten [Lernprogramm](using-query-string-values-to-retrieve-data.md), verwenden Sie einen Wert der Abfragezeichenfolge bei der Auswahl.</span><span class="sxs-lookup"><span data-stu-id="3c36b-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3c36b-178">[Zurück](sorting-paging-and-filtering-data.md)
[Weiter](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="3c36b-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
