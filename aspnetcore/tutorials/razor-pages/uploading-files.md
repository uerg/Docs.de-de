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
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>Hochladen von Dateien auf eine Razor-Seite in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

In diesem Abschnitt wird veranschaulicht, wie Dateien auf eine Razor-Seite hochgeladen werden.

Die [RazorPagesMovie-Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in diesem Tutorial verwendet die einfache Modellbindung zum Hochladen von Dateien. Dies ist beim Hochladen von kleinen Dateien besonders praktisch. Informationen zum Streamen von großen Dateien finden Sie unter [Hochladen großer Dateien über Streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

In den folgenden Schritten fügen Sie der Beispiel-App eine Funktion zum Hochladen eines Filmzeitplans hinzu. Ein Filmzeitplan wird durch eine `Schedule`-Klasse dargestellt. Die Klasse enthält zwei Versionen des Zeitplans. Eine Version wird für Kunden bereitgestellt, `PublicSchedule`. Die andere Version wird für Mitarbeiter des Unternehmens verwendet, `PrivateSchedule`. Jede Version wird als separate Datei hochgeladen. Das Tutorial veranschaulicht, wie zwei Dateiuploads von einer Seite mit einem einzigen POST an den Server durchgeführt werden.

## <a name="security-considerations"></a>Sicherheitsüberlegungen

Gehen Sie mit Bedacht vor, wenn Sie Benutzern die Möglichkeit geben, Dateien auf einen Server hochzuladen. Angreifer könnten [Denial-of-Service-Angriffe](/windows-hardware/drivers/ifs/denial-of-service) und andere Angriffe auf das System ausführen. Folgende Schritte können Sie dabei unterstützen, die Wahrscheinlichkeit eines erfolgreichen Angriffs zu verringern:

* Laden Sie Dateien in einem dedizierten Uploadbereich auf dem System hoch, wodurch es einfacher ist, Sicherheitsmaßnahmen für den hochgeladenen Inhalt zu ergreifen. Stellen Sie beim Erteilen der Berechtigung zum Hochladen von Dateien sicher, dass für den Uploadbereich keine Ausführungsberechtigungen gewährt wurden.
* Verwenden Sie einen von der App bestimmten sicheren Dateinamen, keine vom Benutzer eingegebenen Namen oder den Dateinamen der hochgeladenen Datei.
* Lassen Sie nur einen festgelegten Satz genehmigter Dateierweiterungen zu.
* Stellen Sie sicher, dass clientseitige Überprüfungen auf dem Server durchgeführt werden. Clientseitige Überprüfungen sind leicht zu umgehen.
* Überprüfen Sie die Größe des Uploads, und verhindern Sie Uploads von unerwartet großen Dateien.
* Führen Sie für den hochgeladenen Inhalt eine Viren-/Malwareüberprüfung durch.

> [!WARNING]
> Das Hochladen von schädlichem Code auf ein System ist häufig der erste Schritt, um Code mit der folgenden Absicht auszuführen:
> * Vollständige Übernahme eines Systems
> * Überladen des Systems mit dem Ziel eines vollständigen Systemausfalls
> * Kompromittieren von Benutzer- oder Systemdaten
> * Anwenden von Graffiti auf eine öffentliche Schnittstelle

## <a name="add-a-fileupload-class"></a>Hinzufügen einer FileUpload-Klasse

Erstellen Sie eine Razor-Seite, die einige Dateiuploads verarbeitet. Fügen Sie eine `FileUpload`-Klasse hinzu, die an die Seite gebunden ist, um die Zeitplandaten abzurufen. Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*. Wählen Sie **Hinzufügen** > **Klasse** aus. Nennen Sie die Klasse **FileUpload**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

Die Klasse verfügt über eine Eigenschaft für den Titel des Zeitplans und eine Eigenschaft für jede der beiden Versionen des Zeitplans. Alle drei Eigenschaften sind erforderlich, und der Titel muss 3 bis 60 Zeichen lang sein.

## <a name="add-a-helper-method-to-upload-files"></a>Hinzufügen einer Hilfsmethode zum Hochladen von Dateien

Um Codeduplikate für die Verarbeitung hochgeladener Zeitplandateien zu vermeiden, fügen Sie zunächst eine statische Hilfsmethode hinzu. Erstellen Sie einen *Dienstprogramme*-Ordner in der App, und fügen Sie eine *FileHelpers.cs*-Datei mit dem folgenden Inhalt hinzu. Die Hilfsmethode `ProcessFormFile` verwendet [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) und [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary), und gibt eine Zeichenfolge zurück, in der die Größe und der Inhalt der Datei enthalten sind. Der Inhaltstyp und die Länge werden überprüft. Wenn die Datei die Validierungsüberprüfung nicht besteht, wird ein Fehler zu `ModelState` hinzugefügt.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a>Speichern der Datei auf einem Datenträger

Die Beispiel-App speichert den Dateiinhalt in einem Datenbankfeld. Um den Dateiinhalt auf einem Datenträger zu speichern, verwenden Sie einen [FileStream](/dotnet/api/system.io.filestream):

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

Der Workerprozess muss über Schreibberechtigungen für den durch `filePath` angegebenen Speicherort verfügen.

### <a name="save-the-file-to-azure-blob-storage"></a>Speichern der Datei in Azure Blob Storage

Informationen zum Speichern von Dateiinhalt in Azure Blob Storage finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Das Thema veranschaulicht, wie Sie mit [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) einen [FileStream](/dotnet/api/system.io.filestream) in Blob Storage speichern.

## <a name="add-the-schedule-class"></a>Hinzufügen der Schedule-Klasse

Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*. Wählen Sie **Hinzufügen** > **Klasse** aus. Nennen Sie die Klasse **Schedule**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

Die Klasse verwendet `Display`- und `DisplayFormat`-Attribute, die benutzerfreundliche Titel und Formatierungen erzeugen, wenn die Zeitplandaten gerendert werden.

## <a name="update-the-moviecontext"></a>Aktualisieren von MovieContext

Geben Sie eine `DbSet`-Klasse in der `MovieContext` (*Models/MovieContext.cs*) für die Zeitpläne ein:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Hinzufügen der Zeitplantabelle zur Datenbank

Öffnen Sie die Paket-Manager-Konsole (Package Manger Console, PMC): **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.

![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

Führen Sie in der PMC die folgenden Befehle aus. Diese Befehle fügen eine `Schedule`-Tabelle zur Datenbank hinzu:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Hinzufügen einer Dateiupload-Razor-Seite

Erstellen Sie im Ordner *Seiten* einen Ordner *Zeitpläne*. Erstellen Sie im Ordner *Zeitpläne* eine Seite namens *Index.cshtml* zum Hochladen eines Zeitplans mit dem folgenden Inhalt:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Jede Formulargruppe enthält ein **\<label>**, das den Namen jeder Klasseneigenschaft anzeigt. Die `Display`-Attribute im `FileUpload`-Modell stellen die Anzeigewerte für die Bezeichnungen bereit. Beispielsweise ist als Anzeigename für die `UploadPublicSchedule`-Eigenschaft `[Display(Name="Public Schedule")]` festgelegt, wodurch „Public Schedule“ in der Bezeichnung angezeigt wird, wenn das Formular rendert.

Jede Formulargruppe umfasst einen Validierungsbereich (**\<span>**). Wenn die Eingabe des Benutzers nicht mit den Eigenschaftsattributen übereinstimmt, die in der `FileUpload`-Klasse festgelegt wurden, oder wenn eine der Dateivalidierungsüberprüfungen der `ProcessFormFile`-Methode fehlschlägt, schlägt die Validierung des Modells fehl. Wenn die Modellvalidierung fehlschlägt, wird eine hilfreiche Validierungsmeldung für den Benutzer gerendert. Beispielsweise erhält die `Title`-Eigenschaft die Anmerkungen `[Required]` und `[StringLength(60, MinimumLength = 3)]`. Wenn der Benutzer keinen Titel angibt, erhält er eine Meldung, die angibt, dass ein Wert erforderlich ist. Wenn ein Benutzer einen Wert eingibt, der weniger als 3 oder mehr als 60 Zeichen umfasst, erhält er eine Meldung, die angibt, dass die Länge des Werts falsch ist. Wenn eine Datei ohne Inhalt bereitgestellt wird, wird eine Meldung angezeigt, dass die Datei leer ist.

## <a name="add-the-page-model"></a>Hinzufügen des Seitenmodells

Fügen Sie das Seitenmodell (*Index.cshtml.cs*) zu dem Ordner *Zeitpläne* hinzu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Das Seitenmodell (`IndexModel` in *Index.cshtml.cs*) bindet die `FileUpload`-Klasse:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Das Modell verwendet zudem eine Liste der Zeitpläne (`IList<Schedule>`), um die in der Datenbank gespeicherten Zeitpläne auf der Seite anzuzeigen:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Wenn die Seite mit `OnGetAsync` geladen wird, wird `Schedules` aus der Datenbank aufgefüllt und dazu verwendet, eine HTML-Tabelle geladener Zeitpläne zu generieren:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Wenn das Formular auf dem Server bereitgestellt wird, wird `ModelState` überprüft. Falls ungültig, wird `Schedule` erneut erstellt, und die Seite rendert mit mindestens einer Validierungsmeldung, die angibt, warum die Validierung der Seite fehlgeschlagen ist. Falls gültig, werden die `FileUpload`-Eigenschaften in *OnPostAsync* verwendet, um den Dateiupload für die beiden Versionen des Zeitplans abzuschließen und um ein neues `Schedule`-Objekt zu erstellen, um die Daten zu speichern. Der Zeitplan wird dann in der Datenbank gespeichert:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Verknüpfen der Dateiupload-Razor-Seite

Öffnen Sie *_Layout.cshtml*, und fügen Sie eine Verknüpfung zu der Navigationsleiste hinzu, um die Dateiuploadseite zu erreichen:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Hinzufügen einer Seite zum Bestätigen des Zeitplan-Löschvorgangs

Wenn der Benutzer klickt, um einen Zeitplan zu löschen, erhält er die Möglichkeit zum Abbrechen des Vorgangs. Fügen Sie eine Seite zum Bestätigen des Löschvorgangs (*Delete.cshtml*) zum *Zeitpläne*-Ordner hinzu:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Im Seitenmodell (*Delete.cshtml.cs*) lädt einen einzelnen Zeitplan, der durch `id` in den Routendaten der Anforderung identifiziert wurde. Fügen Sie die Datei *Delete.cshtml.cs* zum *Zeitpläne*-Ordner hinzu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

Die `OnPostAsync`-Methode verarbeitet das Löschen des Zeitplans über ihre `id`:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

Nachdem der Zeitplan erfolgreich gelöscht wurde, sendet `RedirectToPage` den Benutzer zurück zur *Index.cshtml*-Seite der Zeitpläne.

## <a name="the-working-schedules-razor-page"></a>Die Razor-Seite der Arbeitszeitpläne

Wenn die Seite geladen wird, werden Bezeichnungen und Eingaben für den Zeitplantitel, den öffentlichen Zeitplan und den privaten Zeitplan mit einer „Absenden“-Schaltfläche gerendert:

![Razor-Seite der Zeitpläne wie beim ersten Ladevorgang ohne Validierungsfehler und leere Felder](uploading-files/_static/browser1.png)

Das Klicken auf die Schaltfläche **Hochladen**, ohne eines der Felder aufzufüllen, verstößt gegen die `[Required]`-Attribute für das Modell. `ModelState` ist ungültig. Die Validierungsfehlermeldungen werden dem Benutzer angezeigt:

![Die Validierungsfehlermeldungen werden neben jedem Eingabesteuerelement angezeigt](uploading-files/_static/browser2.png)

Geben Sie zwei Buchstaben in das Feld **Titel** ein. Die Validierungsmeldung ändert sich und gibt an, dass der Titel zwischen 3 und 60 Zeichen aufweisen muss:

![Validierungsmeldung zum Titel wurde geändert](uploading-files/_static/browser3.png)

Wenn ein oder mehrere Zeitpläne hochgeladen werden, rendert der Bereich **Loaded Schedules** (Geladene Zeitpläne) die geladenen Zeitpläne:

![Tabelle der geladenen Zeitpläne, die den Titel jedes Zeitplans, den Uploadzeitpunkt in UTC, die Dateigröße der öffentlichen Version und die Dateigröße der privaten Version anzeigt](uploading-files/_static/browser4.png)

Der Benutzer kann von dort aus auf den Link **Löschen** klicken, um zur Anzeige der Bestätigung des Löschvorgangs zu gelangen, die die Möglichkeit bietet, diesen zu bestätigen oder abzubrechen.

## <a name="troubleshooting"></a>Problembehandlung

Informationen zur Behandlung von Problemen beim Hochladen von `IFormFile` finden Sie unter [Dateiuploads in ASP.NET Core](xref:mvc/models/file-uploads#troubleshooting) im Abschnitt „Problembehandlung“.

Vielen Dank für Ihr Interesse an dieser Einführung in Razor-Seiten. Wir freuen uns über Ihr Feedback. [Erste Schritte mit MVC und EF Core](xref:data/ef-mvc/intro) ist ein ausgezeichneter Anschlussartikel an dieses Tutorial.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Dateiuploads in ASP.NET Core](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Zurück: Validierung](xref:tutorials/razor-pages/validation)
