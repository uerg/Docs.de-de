---
title: Hochladen von Dateien auf eine Razor-Seite in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Dateien auf eine Razor-Seite hochladen.
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 24eaa0dd9293cc932c51d280300308e835a0840e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="773e6-103">Hochladen von Dateien auf eine Razor-Seite in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="773e6-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="773e6-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="773e6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="773e6-105">In diesem Abschnitt wird veranschaulicht, wie Dateien auf eine Razor-Seite hochgeladen werden.</span><span class="sxs-lookup"><span data-stu-id="773e6-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="773e6-106">Die [RazorPagesMovie-Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in diesem Tutorial verwendet die einfache Modellbindung zum Hochladen von Dateien. Dies ist beim Hochladen von kleinen Dateien besonders praktisch.</span><span class="sxs-lookup"><span data-stu-id="773e6-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="773e6-107">Informationen zum Streamen von großen Dateien finden Sie unter [Hochladen großer Dateien über Streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="773e6-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="773e6-108">In den folgenden Schritten fügen Sie eine Funktion zum Hochladen eines Filmzeitplans zu der Beispiel-App hinzu.</span><span class="sxs-lookup"><span data-stu-id="773e6-108">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="773e6-109">Ein Filmzeitplan wird durch eine `Schedule`-Klasse dargestellt.</span><span class="sxs-lookup"><span data-stu-id="773e6-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="773e6-110">Die Klasse enthält zwei Versionen des Zeitplans.</span><span class="sxs-lookup"><span data-stu-id="773e6-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="773e6-111">Eine Version wird für Kunden bereitgestellt, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="773e6-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="773e6-112">Die andere Version wird für Mitarbeiter des Unternehmens verwendet, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="773e6-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="773e6-113">Jede Version wird als separate Datei hochgeladen.</span><span class="sxs-lookup"><span data-stu-id="773e6-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="773e6-114">Das Tutorial veranschaulicht, wie zwei Dateiuploads von einer Seite mit einem einzigen POST an den Server durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="773e6-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="773e6-115">Hinzufügen einer FileUpload-Klasse</span><span class="sxs-lookup"><span data-stu-id="773e6-115">Add a FileUpload class</span></span>

<span data-ttu-id="773e6-116">Im folgenden Beispiel erstellen Sie eine Razor-Seite, die einige Dateiuploads verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="773e6-116">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="773e6-117">Fügen Sie eine `FileUpload`-Klasse hinzu, die an die Seite gebunden ist, um die Zeitplandaten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="773e6-117">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="773e6-118">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="773e6-118">Right click the *Models* folder.</span></span> <span data-ttu-id="773e6-119">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="773e6-119">Select **Add** > **Class**.</span></span> <span data-ttu-id="773e6-120">Nennen Sie die Klasse **FileUpload**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="773e6-120">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="773e6-121">Die Klasse verfügt über eine Eigenschaft für den Titel des Zeitplans und eine Eigenschaft für jede der beiden Versionen des Zeitplans.</span><span class="sxs-lookup"><span data-stu-id="773e6-121">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="773e6-122">Alle drei Eigenschaften sind erforderlich, und der Titel muss 3 bis 60 Zeichen lang sein.</span><span class="sxs-lookup"><span data-stu-id="773e6-122">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="773e6-123">Hinzufügen einer Hilfsmethode zum Hochladen von Dateien</span><span class="sxs-lookup"><span data-stu-id="773e6-123">Add a helper method to upload files</span></span>

<span data-ttu-id="773e6-124">Um Codeduplikate für die Verarbeitung hochgeladener Zeitplandateien zu vermeiden, fügen Sie zunächst eine statische Hilfsmethode hinzu.</span><span class="sxs-lookup"><span data-stu-id="773e6-124">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="773e6-125">Erstellen Sie einen *Dienstprogramme*-Ordner in der App, und fügen Sie eine *FileHelpers.cs*-Datei mit dem folgenden Inhalt hinzu.</span><span class="sxs-lookup"><span data-stu-id="773e6-125">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="773e6-126">Die Hilfsmethode `ProcessFormFile` verwendet [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary), und gibt eine Zeichenfolge zurück, in der die Größe und der Inhalt der Datei enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="773e6-126">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="773e6-127">Der Inhaltstyp und die Länge werden überprüft.</span><span class="sxs-lookup"><span data-stu-id="773e6-127">The content type and length are checked.</span></span> <span data-ttu-id="773e6-128">Wenn die Datei die Validierungsüberprüfung nicht besteht, wird ein Fehler zu `ModelState` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="773e6-128">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="773e6-129">Hinzufügen der Schedule-Klasse</span><span class="sxs-lookup"><span data-stu-id="773e6-129">Add the Schedule class</span></span>

<span data-ttu-id="773e6-130">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="773e6-130">Right click the *Models* folder.</span></span> <span data-ttu-id="773e6-131">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="773e6-131">Select **Add** > **Class**.</span></span> <span data-ttu-id="773e6-132">Nennen Sie die Klasse **Schedule**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="773e6-132">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="773e6-133">Die Klasse verwendet `Display`- und `DisplayFormat`-Attribute, die benutzerfreundliche Titel und Formatierungen erzeugen, wenn die Zeitplandaten gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="773e6-133">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="773e6-134">Aktualisieren von MovieContext</span><span class="sxs-lookup"><span data-stu-id="773e6-134">Update the MovieContext</span></span>

<span data-ttu-id="773e6-135">Geben Sie eine `DbSet`-Klasse in der `MovieContext` (*Models/MovieContext.cs*) für die Zeitpläne ein:</span><span class="sxs-lookup"><span data-stu-id="773e6-135">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="773e6-136">Hinzufügen der Zeitplantabelle zur Datenbank</span><span class="sxs-lookup"><span data-stu-id="773e6-136">Add the Schedule table to the database</span></span>

<span data-ttu-id="773e6-137">Öffnen Sie die Paket-Manager-Konsole (Package Manger Console, PMC): **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="773e6-137">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="773e6-139">Führen Sie in der PMC die folgenden Befehle aus.</span><span class="sxs-lookup"><span data-stu-id="773e6-139">In the PMC, execute the following commands.</span></span> <span data-ttu-id="773e6-140">Diese Befehle fügen eine `Schedule`-Tabelle zur Datenbank hinzu:</span><span class="sxs-lookup"><span data-stu-id="773e6-140">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="773e6-141">Hinzufügen einer Dateiupload-Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="773e6-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="773e6-142">Erstellen Sie im Ordner *Seiten* einen Ordner *Zeitpläne*.</span><span class="sxs-lookup"><span data-stu-id="773e6-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="773e6-143">Erstellen Sie im Ordner *Zeitpläne* eine Seite namens *Index.cshtml* zum Hochladen eines Zeitplans mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="773e6-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="773e6-144">Jede Formulargruppe enthält ein **\<label>**, das den Namen jeder Klasseneigenschaft anzeigt.</span><span class="sxs-lookup"><span data-stu-id="773e6-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="773e6-145">Die `Display`-Attribute im `FileUpload`-Modell stellen die Anzeigewerte für die Bezeichnungen bereit.</span><span class="sxs-lookup"><span data-stu-id="773e6-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="773e6-146">Beispielsweise ist als Anzeigename für die `UploadPublicSchedule`-Eigenschaft `[Display(Name="Public Schedule")]` festgelegt, wodurch „Public Schedule“ in der Bezeichnung angezeigt wird, wenn das Formular rendert.</span><span class="sxs-lookup"><span data-stu-id="773e6-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="773e6-147">Jede Formulargruppe umfasst einen Validierungsbereich (**\<span>**).</span><span class="sxs-lookup"><span data-stu-id="773e6-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="773e6-148">Wenn die Eingabe des Benutzers nicht mit den Eigenschaftsattributen übereinstimmt, die in der `FileUpload`-Klasse festgelegt wurden, oder wenn eine der Dateivalidierungsüberprüfungen der `ProcessFormFile`-Methode fehlschlägt, schlägt die Validierung des Modells fehl.</span><span class="sxs-lookup"><span data-stu-id="773e6-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="773e6-149">Wenn die Modellvalidierung fehlschlägt, wird eine hilfreiche Validierungsmeldung für den Benutzer gerendert.</span><span class="sxs-lookup"><span data-stu-id="773e6-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="773e6-150">Beispielsweise erhält die `Title`-Eigenschaft die Anmerkungen `[Required]` und `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="773e6-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="773e6-151">Wenn der Benutzer keinen Titel angibt, erhält er eine Meldung, die angibt, dass ein Wert erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="773e6-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="773e6-152">Wenn ein Benutzer einen Wert eingibt, der weniger als 3 oder mehr als 60 Zeichen umfasst, erhält er eine Meldung, die angibt, dass die Länge des Werts falsch ist.</span><span class="sxs-lookup"><span data-stu-id="773e6-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="773e6-153">Wenn eine Datei ohne Inhalt bereitgestellt wird, wird eine Meldung angezeigt, dass die Datei leer ist.</span><span class="sxs-lookup"><span data-stu-id="773e6-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="773e6-154">Hinzufügen des Seitenmodells</span><span class="sxs-lookup"><span data-stu-id="773e6-154">Add the page model</span></span>

<span data-ttu-id="773e6-155">Fügen Sie das Seitenmodell (*Index.cshtml.cs*) zu dem Ordner *Zeitpläne* hinzu:</span><span class="sxs-lookup"><span data-stu-id="773e6-155">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="773e6-156">Das Seitenmodell (`IndexModel` in *Index.cshtml.cs*) bindet die `FileUpload`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="773e6-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="773e6-157">Das Modell verwendet zudem eine Liste der Zeitpläne (`IList<Schedule>`), um die in der Datenbank gespeicherten Zeitpläne auf der Seite anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="773e6-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="773e6-158">Wenn die Seite mit `OnGetAsync` geladen wird, wird `Schedules` aus der Datenbank aufgefüllt und dazu verwendet, eine HTML-Tabelle geladener Zeitpläne zu generieren:</span><span class="sxs-lookup"><span data-stu-id="773e6-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="773e6-159">Wenn das Formular auf dem Server bereitgestellt wird, wird `ModelState` überprüft.</span><span class="sxs-lookup"><span data-stu-id="773e6-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="773e6-160">Falls ungültig, wird `Schedule` erneut erstellt, und die Seite rendert mit mindestens einer Validierungsmeldung, die angibt, warum die Validierung der Seite fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="773e6-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="773e6-161">Falls gültig, werden die `FileUpload`-Eigenschaften in *OnPostAsync* verwendet, um den Dateiupload für die beiden Versionen des Zeitplans abzuschließen und um ein neues `Schedule`-Objekt zu erstellen, um die Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="773e6-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="773e6-162">Der Zeitplan wird dann in der Datenbank gespeichert:</span><span class="sxs-lookup"><span data-stu-id="773e6-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="773e6-163">Verknüpfen der Dateiupload-Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="773e6-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="773e6-164">Öffnen Sie *_Layout.cshtml*, und fügen Sie eine Verknüpfung zu der Navigationsleiste hinzu, um die Dateiuploadseite zu erreichen:</span><span class="sxs-lookup"><span data-stu-id="773e6-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="773e6-165">Hinzufügen einer Seite zum Bestätigen des Zeitplan-Löschvorgangs</span><span class="sxs-lookup"><span data-stu-id="773e6-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="773e6-166">Wenn der Benutzer klickt, um den Zeitplan zu löschen, sollten Sie ihm die Chance geben, den Vorgang abzubrechen.</span><span class="sxs-lookup"><span data-stu-id="773e6-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="773e6-167">Fügen Sie eine Seite zum Bestätigen des Löschvorgangs (*Delete.cshtml*) zum *Zeitpläne*-Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="773e6-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="773e6-168">Im Seitenmodell (*Delete.cshtml.cs*) lädt einen einzelnen Zeitplan, der durch `id` in den Routendaten der Anforderung identifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="773e6-168">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="773e6-169">Fügen Sie die Datei *Delete.cshtml.cs* zum *Zeitpläne*-Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="773e6-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="773e6-170">Die `OnPostAsync`-Methode verarbeitet das Löschen des Zeitplans über ihre `id`:</span><span class="sxs-lookup"><span data-stu-id="773e6-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="773e6-171">Nachdem der Zeitplan erfolgreich gelöscht wurde, sendet `RedirectToPage` den Benutzer zurück zur *Index.cshtml*-Seite der Zeitpläne.</span><span class="sxs-lookup"><span data-stu-id="773e6-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="773e6-172">Die Razor-Seite der Arbeitszeitpläne</span><span class="sxs-lookup"><span data-stu-id="773e6-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="773e6-173">Wenn die Seite geladen wird, werden Bezeichnungen und Eingaben für den Zeitplantitel, den öffentlichen Zeitplan und den privaten Zeitplan mit einer „Absenden“-Schaltfläche gerendert:</span><span class="sxs-lookup"><span data-stu-id="773e6-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Razor-Seite der Zeitpläne wie beim ersten Ladevorgang ohne Validierungsfehler und leere Felder](uploading-files/_static/browser1.png)

<span data-ttu-id="773e6-175">Das Klicken auf die Schaltfläche **Hochladen**, ohne eines der Felder aufzufüllen, verstößt gegen die `[Required]`-Attribute für das Modell.</span><span class="sxs-lookup"><span data-stu-id="773e6-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="773e6-176">`ModelState` ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="773e6-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="773e6-177">Die Validierungsfehlermeldungen werden dem Benutzer angezeigt:</span><span class="sxs-lookup"><span data-stu-id="773e6-177">The validation error messages are displayed to the user:</span></span>

![Die Validierungsfehlermeldungen werden neben jedem Eingabesteuerelement angezeigt](uploading-files/_static/browser2.png)

<span data-ttu-id="773e6-179">Geben Sie zwei Buchstaben in das Feld **Titel** ein.</span><span class="sxs-lookup"><span data-stu-id="773e6-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="773e6-180">Die Validierungsmeldung ändert sich und gibt an, dass der Titel zwischen 3 und 60 Zeichen aufweisen muss:</span><span class="sxs-lookup"><span data-stu-id="773e6-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Validierungsmeldung zum Titel wurde geändert](uploading-files/_static/browser3.png)

<span data-ttu-id="773e6-182">Wenn ein oder mehrere Zeitpläne hochgeladen werden, rendert der Bereich **Loaded Schedules** (Geladene Zeitpläne) die geladenen Zeitpläne:</span><span class="sxs-lookup"><span data-stu-id="773e6-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabelle der geladenen Zeitpläne, die den Titel jedes Zeitplans, den Uploadzeitpunkt in UTC, die Dateigröße der öffentlichen Version und die Dateigröße der privaten Version anzeigt](uploading-files/_static/browser4.png)

<span data-ttu-id="773e6-184">Der Benutzer kann von dort aus auf den Link **Löschen** klicken, um zur Anzeige der Bestätigung des Löschvorgangs zu gelangen, die die Möglichkeit bietet, diesen zu bestätigen oder abzubrechen.</span><span class="sxs-lookup"><span data-stu-id="773e6-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="773e6-185">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="773e6-185">Troubleshooting</span></span>

<span data-ttu-id="773e6-186">Informationen zur Behandlung von Problemen beim Hochladen von `IFormFile` finden Sie unter [Dateiuploads in ASP.NET Core](xref:mvc/models/file-uploads#troubleshooting) im Abschnitt „Problembehandlung“.</span><span class="sxs-lookup"><span data-stu-id="773e6-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="773e6-187">Vielen Dank für Ihr Interesse an dieser Einführung in Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="773e6-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="773e6-188">Wir freuen uns über Kommentare, die Sie hinterlassen.</span><span class="sxs-lookup"><span data-stu-id="773e6-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="773e6-189">[Erste Schritte mit MVC und EF Core](xref:data/ef-mvc/intro) ist ein ausgezeichneter Anschlussartikel an dieses Tutorial.</span><span class="sxs-lookup"><span data-stu-id="773e6-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="773e6-190">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="773e6-190">Additional resources</span></span>

* [<span data-ttu-id="773e6-191">Dateiuploads in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="773e6-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="773e6-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="773e6-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="773e6-193">Zurück: Validierung</span><span class="sxs-lookup"><span data-stu-id="773e6-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
