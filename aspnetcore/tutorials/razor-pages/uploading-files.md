---
title: Hochladen von Dateien auf eine Razor-Seite in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Dateien auf eine Razor Page hochladen.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 43268e24b67279b57c990a6289922ae38d883221
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275956"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="3dd75-103">Hochladen von Dateien auf eine Razor-Seite in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3dd75-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="3dd75-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3dd75-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3dd75-105">In diesem Abschnitt wird veranschaulicht, wie Dateien auf eine Razor Page hochgeladen werden.</span><span class="sxs-lookup"><span data-stu-id="3dd75-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="3dd75-106">Die [RazorPagesMovie-Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in diesem Tutorial verwendet die einfache Modellbindung zum Hochladen von Dateien. Dies ist beim Hochladen von kleinen Dateien besonders praktisch.</span><span class="sxs-lookup"><span data-stu-id="3dd75-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="3dd75-107">Informationen zum Streamen von großen Dateien finden Sie unter [Hochladen großer Dateien über Streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="3dd75-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="3dd75-108">In den folgenden Schritten fügen Sie der Beispiel-App eine Funktion zum Hochladen eines Filmzeitplans hinzu.</span><span class="sxs-lookup"><span data-stu-id="3dd75-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="3dd75-109">Ein Filmzeitplan wird durch eine `Schedule`-Klasse dargestellt.</span><span class="sxs-lookup"><span data-stu-id="3dd75-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="3dd75-110">Die Klasse enthält zwei Versionen des Zeitplans.</span><span class="sxs-lookup"><span data-stu-id="3dd75-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="3dd75-111">Eine Version wird für Kunden bereitgestellt, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="3dd75-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="3dd75-112">Die andere Version wird für Mitarbeiter des Unternehmens verwendet, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="3dd75-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="3dd75-113">Jede Version wird als separate Datei hochgeladen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="3dd75-114">Das Tutorial veranschaulicht, wie zwei Dateiuploads von einer Seite mit einem einzigen POST an den Server durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="3dd75-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="3dd75-115">Sicherheitsüberlegungen</span><span class="sxs-lookup"><span data-stu-id="3dd75-115">Security considerations</span></span>

<span data-ttu-id="3dd75-116">Gehen Sie mit Bedacht vor, wenn Sie Benutzern die Möglichkeit geben, Dateien auf einen Server hochzuladen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="3dd75-117">Angreifer könnten [Denial-of-Service-Angriffe](/windows-hardware/drivers/ifs/denial-of-service) und andere Angriffe auf das System ausführen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="3dd75-118">Folgende Schritte können Sie dabei unterstützen, die Wahrscheinlichkeit eines erfolgreichen Angriffs zu verringern:</span><span class="sxs-lookup"><span data-stu-id="3dd75-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="3dd75-119">Laden Sie Dateien in einem dedizierten Uploadbereich auf dem System hoch, wodurch es einfacher ist, Sicherheitsmaßnahmen für den hochgeladenen Inhalt zu ergreifen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="3dd75-120">Stellen Sie beim Erteilen der Berechtigung zum Hochladen von Dateien sicher, dass für den Uploadbereich keine Ausführungsberechtigungen gewährt wurden.</span><span class="sxs-lookup"><span data-stu-id="3dd75-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="3dd75-121">Verwenden Sie einen von der App bestimmten sicheren Dateinamen, keine vom Benutzer eingegebenen Namen oder den Dateinamen der hochgeladenen Datei.</span><span class="sxs-lookup"><span data-stu-id="3dd75-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="3dd75-122">Lassen Sie nur einen festgelegten Satz genehmigter Dateierweiterungen zu.</span><span class="sxs-lookup"><span data-stu-id="3dd75-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="3dd75-123">Stellen Sie sicher, dass clientseitige Überprüfungen auf dem Server durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="3dd75-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="3dd75-124">Clientseitige Überprüfungen sind leicht zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="3dd75-125">Überprüfen Sie die Größe des Uploads, und verhindern Sie Uploads von unerwartet großen Dateien.</span><span class="sxs-lookup"><span data-stu-id="3dd75-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="3dd75-126">Führen Sie für den hochgeladenen Inhalt eine Viren-/Malwareüberprüfung durch.</span><span class="sxs-lookup"><span data-stu-id="3dd75-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="3dd75-127">Das Hochladen von schädlichem Code auf ein System ist häufig der erste Schritt, um Code mit der folgenden Absicht auszuführen:</span><span class="sxs-lookup"><span data-stu-id="3dd75-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="3dd75-128">Vollständige Übernahme eines Systems</span><span class="sxs-lookup"><span data-stu-id="3dd75-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="3dd75-129">Überladen des Systems mit dem Ziel eines vollständigen Systemausfalls</span><span class="sxs-lookup"><span data-stu-id="3dd75-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="3dd75-130">Kompromittieren von Benutzer- oder Systemdaten</span><span class="sxs-lookup"><span data-stu-id="3dd75-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="3dd75-131">Anwenden von Graffiti auf eine öffentliche Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="3dd75-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="3dd75-132">Hinzufügen einer FileUpload-Klasse</span><span class="sxs-lookup"><span data-stu-id="3dd75-132">Add a FileUpload class</span></span>

<span data-ttu-id="3dd75-133">Erstellen Sie eine Razor Page, die einige Dateiuploads verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="3dd75-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="3dd75-134">Fügen Sie eine `FileUpload`-Klasse hinzu, die an die Seite gebunden ist, um die Zeitplandaten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="3dd75-135">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="3dd75-135">Right click the *Models* folder.</span></span> <span data-ttu-id="3dd75-136">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="3dd75-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="3dd75-137">Nennen Sie die Klasse **FileUpload**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="3dd75-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="3dd75-138">Die Klasse verfügt über eine Eigenschaft für den Titel des Zeitplans und eine Eigenschaft für jede der beiden Versionen des Zeitplans.</span><span class="sxs-lookup"><span data-stu-id="3dd75-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="3dd75-139">Alle drei Eigenschaften sind erforderlich, und der Titel muss 3 bis 60 Zeichen lang sein.</span><span class="sxs-lookup"><span data-stu-id="3dd75-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="3dd75-140">Hinzufügen einer Hilfsmethode zum Hochladen von Dateien</span><span class="sxs-lookup"><span data-stu-id="3dd75-140">Add a helper method to upload files</span></span>

<span data-ttu-id="3dd75-141">Um Codeduplikate für die Verarbeitung hochgeladener Zeitplandateien zu vermeiden, fügen Sie zunächst eine statische Hilfsmethode hinzu.</span><span class="sxs-lookup"><span data-stu-id="3dd75-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="3dd75-142">Erstellen Sie einen *Dienstprogramme*-Ordner in der App, und fügen Sie eine *FileHelpers.cs*-Datei mit dem folgenden Inhalt hinzu.</span><span class="sxs-lookup"><span data-stu-id="3dd75-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="3dd75-143">Die Hilfsmethode `ProcessFormFile` verwendet [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary), und gibt eine Zeichenfolge zurück, in der die Größe und der Inhalt der Datei enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="3dd75-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="3dd75-144">Der Inhaltstyp und die Länge werden überprüft.</span><span class="sxs-lookup"><span data-stu-id="3dd75-144">The content type and length are checked.</span></span> <span data-ttu-id="3dd75-145">Wenn die Datei die Validierungsüberprüfung nicht besteht, wird ein Fehler zu `ModelState` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3dd75-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="3dd75-146">Speichern der Datei auf einem Datenträger</span><span class="sxs-lookup"><span data-stu-id="3dd75-146">Save the file to disk</span></span>

<span data-ttu-id="3dd75-147">Die Beispiel-App speichert hochgeladene Dateien in Datenbankfeldern.</span><span class="sxs-lookup"><span data-stu-id="3dd75-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="3dd75-148">Verwenden Sie ein [FileStream](/dotnet/api/system.io.filestream)-Objekt, um eine Datei auf dem Datenträger zu speichern.</span><span class="sxs-lookup"><span data-stu-id="3dd75-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="3dd75-149">Im folgenden Beispiel wird eine Datei, die sich in `FileUpload.UploadPublicSchedule` befindet, in ein `FileStream`-Objekt innerhalb der `OnPostAsync`-Methode kopiert.</span><span class="sxs-lookup"><span data-stu-id="3dd75-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="3dd75-150">Das `FileStream`-Objekt schreibt die Datei unter dem angegebenen Pfad und Dateinamen (`<PATH-AND-FILE-NAME>`) auf den Datenträger:</span><span class="sxs-lookup"><span data-stu-id="3dd75-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="3dd75-151">Der Workerprozess muss über Schreibberechtigungen für den durch `filePath` angegebenen Speicherort verfügen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="3dd75-152">Das `filePath`-Objekt *muss* den Dateinamen enthalten.</span><span class="sxs-lookup"><span data-stu-id="3dd75-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="3dd75-153">Wird der Dateiname nicht angegeben, wird eine [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) zur Laufzeit ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="3dd75-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="3dd75-154">Hochgeladene Dateien dürfen nie persistent in der Verzeichnisstruktur gespeichert werden, in der sich auch die App befindet.</span><span class="sxs-lookup"><span data-stu-id="3dd75-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="3dd75-155">Im Codebeispiel wird kein serverseitiger Schutz vor Uploads von Dateien mit Schadsoftware bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="3dd75-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="3dd75-156">Wie Sie die Angriffsoberfläche beim Akzeptieren von Benutzerdateien reduzieren, erfahren Sie in den folgenden Artikeln:</span><span class="sxs-lookup"><span data-stu-id="3dd75-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="3dd75-157">Unrestricted File Upload (Uneingeschränkter Dateiupload)</span><span class="sxs-lookup"><span data-stu-id="3dd75-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="3dd75-158">Azure-Sicherheit: Sicherstellen, dass entsprechende Kontrollen gelten, wenn Dateien vom Benutzer akzeptiert werden</span><span class="sxs-lookup"><span data-stu-id="3dd75-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="3dd75-159">Speichern der Datei in Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3dd75-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="3dd75-160">Informationen zum Speichern von Dateiinhalt in Azure Blob Storage finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="3dd75-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="3dd75-161">Das Thema veranschaulicht, wie Sie mit [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) einen [FileStream](/dotnet/api/system.io.filestream) in Blob Storage speichern.</span><span class="sxs-lookup"><span data-stu-id="3dd75-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="3dd75-162">Hinzufügen der Schedule-Klasse</span><span class="sxs-lookup"><span data-stu-id="3dd75-162">Add the Schedule class</span></span>

<span data-ttu-id="3dd75-163">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="3dd75-163">Right click the *Models* folder.</span></span> <span data-ttu-id="3dd75-164">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="3dd75-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="3dd75-165">Nennen Sie die Klasse **Schedule**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="3dd75-165">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="3dd75-166">Die Klasse verwendet `Display`- und `DisplayFormat`-Attribute, die benutzerfreundliche Titel und Formatierungen erzeugen, wenn die Zeitplandaten gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="3dd75-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="3dd75-167">Aktualisieren von MovieContext</span><span class="sxs-lookup"><span data-stu-id="3dd75-167">Update the MovieContext</span></span>

<span data-ttu-id="3dd75-168">Geben Sie eine `DbSet`-Klasse in der `MovieContext` (*Models/MovieContext.cs*) für die Zeitpläne ein:</span><span class="sxs-lookup"><span data-stu-id="3dd75-168">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="3dd75-169">Hinzufügen der Zeitplantabelle zur Datenbank</span><span class="sxs-lookup"><span data-stu-id="3dd75-169">Add the Schedule table to the database</span></span>

<span data-ttu-id="3dd75-170">Öffnen Sie die Paket-Manager-Konsole (Package Manger Console, PMC): **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="3dd75-170">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3dd75-172">Führen Sie in der PMC die folgenden Befehle aus.</span><span class="sxs-lookup"><span data-stu-id="3dd75-172">In the PMC, execute the following commands.</span></span> <span data-ttu-id="3dd75-173">Diese Befehle fügen eine `Schedule`-Tabelle zur Datenbank hinzu:</span><span class="sxs-lookup"><span data-stu-id="3dd75-173">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="3dd75-174">Hinzufügen einer Dateiupload-Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="3dd75-174">Add a file upload Razor Page</span></span>

<span data-ttu-id="3dd75-175">Erstellen Sie im Ordner *Seiten* einen Ordner *Zeitpläne*.</span><span class="sxs-lookup"><span data-stu-id="3dd75-175">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="3dd75-176">Erstellen Sie im Ordner *Zeitpläne* eine Seite namens *Index.cshtml* zum Hochladen eines Zeitplans mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="3dd75-176">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="3dd75-177">Jede Formulargruppe enthält ein **\<label>**, das den Namen jeder Klasseneigenschaft anzeigt.</span><span class="sxs-lookup"><span data-stu-id="3dd75-177">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="3dd75-178">Die `Display`-Attribute im `FileUpload`-Modell stellen die Anzeigewerte für die Bezeichnungen bereit.</span><span class="sxs-lookup"><span data-stu-id="3dd75-178">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="3dd75-179">Beispielsweise ist als Anzeigename für die `UploadPublicSchedule`-Eigenschaft `[Display(Name="Public Schedule")]` festgelegt, wodurch „Public Schedule“ in der Bezeichnung angezeigt wird, wenn das Formular rendert.</span><span class="sxs-lookup"><span data-stu-id="3dd75-179">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="3dd75-180">Jede Formulargruppe umfasst einen Validierungsbereich (**\<span>**).</span><span class="sxs-lookup"><span data-stu-id="3dd75-180">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="3dd75-181">Wenn die Eingabe des Benutzers nicht mit den Eigenschaftsattributen übereinstimmt, die in der `FileUpload`-Klasse festgelegt wurden, oder wenn eine der Dateivalidierungsüberprüfungen der `ProcessFormFile`-Methode fehlschlägt, schlägt die Validierung des Modells fehl.</span><span class="sxs-lookup"><span data-stu-id="3dd75-181">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="3dd75-182">Wenn die Modellvalidierung fehlschlägt, wird eine hilfreiche Validierungsmeldung für den Benutzer gerendert.</span><span class="sxs-lookup"><span data-stu-id="3dd75-182">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="3dd75-183">Beispielsweise erhält die `Title`-Eigenschaft die Anmerkungen `[Required]` und `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="3dd75-183">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="3dd75-184">Wenn der Benutzer keinen Titel angibt, erhält er eine Meldung, die angibt, dass ein Wert erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="3dd75-184">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="3dd75-185">Wenn ein Benutzer einen Wert eingibt, der weniger als 3 oder mehr als 60 Zeichen umfasst, erhält er eine Meldung, die angibt, dass die Länge des Werts falsch ist.</span><span class="sxs-lookup"><span data-stu-id="3dd75-185">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="3dd75-186">Wenn eine Datei ohne Inhalt bereitgestellt wird, wird eine Meldung angezeigt, dass die Datei leer ist.</span><span class="sxs-lookup"><span data-stu-id="3dd75-186">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="3dd75-187">Hinzufügen des Seitenmodells</span><span class="sxs-lookup"><span data-stu-id="3dd75-187">Add the page model</span></span>

<span data-ttu-id="3dd75-188">Fügen Sie das Seitenmodell (*Index.cshtml.cs*) zu dem Ordner *Zeitpläne* hinzu:</span><span class="sxs-lookup"><span data-stu-id="3dd75-188">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="3dd75-189">Das Seitenmodell (`IndexModel` in *Index.cshtml.cs*) bindet die `FileUpload`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="3dd75-189">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="3dd75-190">Das Modell verwendet zudem eine Liste der Zeitpläne (`IList<Schedule>`), um die in der Datenbank gespeicherten Zeitpläne auf der Seite anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="3dd75-190">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="3dd75-191">Wenn die Seite mit `OnGetAsync` geladen wird, wird `Schedules` aus der Datenbank aufgefüllt und dazu verwendet, eine HTML-Tabelle geladener Zeitpläne zu generieren:</span><span class="sxs-lookup"><span data-stu-id="3dd75-191">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="3dd75-192">Wenn das Formular auf dem Server bereitgestellt wird, wird `ModelState` überprüft.</span><span class="sxs-lookup"><span data-stu-id="3dd75-192">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="3dd75-193">Falls ungültig, wird `Schedule` erneut erstellt, und die Seite rendert mit mindestens einer Validierungsmeldung, die angibt, warum die Validierung der Seite fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="3dd75-193">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="3dd75-194">Falls gültig, werden die `FileUpload`-Eigenschaften in *OnPostAsync* verwendet, um den Dateiupload für die beiden Versionen des Zeitplans abzuschließen und um ein neues `Schedule`-Objekt zu erstellen, um die Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="3dd75-194">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="3dd75-195">Der Zeitplan wird dann in der Datenbank gespeichert:</span><span class="sxs-lookup"><span data-stu-id="3dd75-195">The schedule is then saved to the database:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="3dd75-196">Verknüpfen der Dateiupload-Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="3dd75-196">Link the file upload Razor Page</span></span>

<span data-ttu-id="3dd75-197">Öffnen Sie *_Layout.cshtml*, und fügen Sie eine Verknüpfung zu der Navigationsleiste hinzu, um die Dateiuploadseite zu erreichen:</span><span class="sxs-lookup"><span data-stu-id="3dd75-197">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="3dd75-198">Hinzufügen einer Seite zum Bestätigen des Zeitplan-Löschvorgangs</span><span class="sxs-lookup"><span data-stu-id="3dd75-198">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="3dd75-199">Wenn der Benutzer klickt, um einen Zeitplan zu löschen, erhält er die Möglichkeit zum Abbrechen des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="3dd75-199">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="3dd75-200">Fügen Sie eine Seite zum Bestätigen des Löschvorgangs (*Delete.cshtml*) zum *Zeitpläne*-Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="3dd75-200">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="3dd75-201">Im Seitenmodell (*Delete.cshtml.cs*) lädt einen einzelnen Zeitplan, der durch `id` in den Routendaten der Anforderung identifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="3dd75-201">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="3dd75-202">Fügen Sie die Datei *Delete.cshtml.cs* zum *Zeitpläne*-Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="3dd75-202">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="3dd75-203">Die `OnPostAsync`-Methode verarbeitet das Löschen des Zeitplans über ihre `id`:</span><span class="sxs-lookup"><span data-stu-id="3dd75-203">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="3dd75-204">Nachdem der Zeitplan erfolgreich gelöscht wurde, sendet `RedirectToPage` den Benutzer zurück zur *Index.cshtml*-Seite der Zeitpläne.</span><span class="sxs-lookup"><span data-stu-id="3dd75-204">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="3dd75-205">Die Razor Page der Arbeitszeitpläne</span><span class="sxs-lookup"><span data-stu-id="3dd75-205">The working Schedules Razor Page</span></span>

<span data-ttu-id="3dd75-206">Wenn die Seite geladen wird, werden Bezeichnungen und Eingaben für den Zeitplantitel, den öffentlichen Zeitplan und den privaten Zeitplan mit einer „Absenden“-Schaltfläche gerendert:</span><span class="sxs-lookup"><span data-stu-id="3dd75-206">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Razor Page der Zeitpläne wie beim ersten Ladevorgang ohne Validierungsfehler und leere Felder](uploading-files/_static/browser1.png)

<span data-ttu-id="3dd75-208">Das Klicken auf die Schaltfläche **Hochladen**, ohne eines der Felder aufzufüllen, verstößt gegen die `[Required]`-Attribute für das Modell.</span><span class="sxs-lookup"><span data-stu-id="3dd75-208">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="3dd75-209">`ModelState` ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="3dd75-209">The `ModelState` is invalid.</span></span> <span data-ttu-id="3dd75-210">Die Validierungsfehlermeldungen werden dem Benutzer angezeigt:</span><span class="sxs-lookup"><span data-stu-id="3dd75-210">The validation error messages are displayed to the user:</span></span>

![Die Validierungsfehlermeldungen werden neben jedem Eingabesteuerelement angezeigt](uploading-files/_static/browser2.png)

<span data-ttu-id="3dd75-212">Geben Sie zwei Buchstaben in das Feld **Titel** ein.</span><span class="sxs-lookup"><span data-stu-id="3dd75-212">Type two letters into the **Title** field.</span></span> <span data-ttu-id="3dd75-213">Die Validierungsmeldung ändert sich und gibt an, dass der Titel zwischen 3 und 60 Zeichen aufweisen muss:</span><span class="sxs-lookup"><span data-stu-id="3dd75-213">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Validierungsmeldung zum Titel wurde geändert](uploading-files/_static/browser3.png)

<span data-ttu-id="3dd75-215">Wenn ein oder mehrere Zeitpläne hochgeladen werden, rendert der Bereich **Loaded Schedules** (Geladene Zeitpläne) die geladenen Zeitpläne:</span><span class="sxs-lookup"><span data-stu-id="3dd75-215">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabelle der geladenen Zeitpläne, die den Titel jedes Zeitplans, den Uploadzeitpunkt in UTC, die Dateigröße der öffentlichen Version und die Dateigröße der privaten Version anzeigt](uploading-files/_static/browser4.png)

<span data-ttu-id="3dd75-217">Der Benutzer kann von dort aus auf den Link **Löschen** klicken, um zur Anzeige der Bestätigung des Löschvorgangs zu gelangen, die die Möglichkeit bietet, diesen zu bestätigen oder abzubrechen.</span><span class="sxs-lookup"><span data-stu-id="3dd75-217">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3dd75-218">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="3dd75-218">Troubleshooting</span></span>

<span data-ttu-id="3dd75-219">Informationen zur Behandlung von Problemen beim Hochladen von `IFormFile` finden Sie unter [Dateiuploads in ASP.NET Core](xref:mvc/models/file-uploads#troubleshooting) im Abschnitt „Problembehandlung“.</span><span class="sxs-lookup"><span data-stu-id="3dd75-219">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="3dd75-220">Vielen Dank für Ihr Interesse an dieser Einführung in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3dd75-220">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="3dd75-221">Wir freuen uns über Ihr Feedback.</span><span class="sxs-lookup"><span data-stu-id="3dd75-221">We appreciate feedback.</span></span> <span data-ttu-id="3dd75-222">[Erste Schritte mit MVC und EF Core](xref:data/ef-mvc/intro) ist ein ausgezeichneter Anschlussartikel an dieses Tutorial.</span><span class="sxs-lookup"><span data-stu-id="3dd75-222">[Get started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3dd75-223">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3dd75-223">Additional resources</span></span>

* [<span data-ttu-id="3dd75-224">Dateiuploads in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3dd75-224">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="3dd75-225">IFormFile</span><span class="sxs-lookup"><span data-stu-id="3dd75-225">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

> [!div class="step-by-step"]
> [<span data-ttu-id="3dd75-226">Zurück: Validierung</span><span class="sxs-lookup"><span data-stu-id="3dd75-226">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
