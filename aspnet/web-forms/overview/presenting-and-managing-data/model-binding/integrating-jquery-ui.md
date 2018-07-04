---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrieren von JQuery UI Datepicker mit modellbindung und Web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 7d60d2945dbf9daca33422ab82b9265fedc2ba31
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393833"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrieren von JQuery UI Datepicker mit modellbindung und Web forms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement). Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.
> 
> In diesem Tutorial wird gezeigt, wie der JQuery UI hinzufügen ["DatePicker" Widget](http://jqueryui.com/datepicker/) auf einem Web Form, und verwenden modellbindung, um die Datenbank mit dem ausgewählten Wert zu aktualisieren.
> 
> Dieses Tutorial baut auf das Projekt erstellt haben, der [erste](retrieving-data.md) und [zweite](updating-deleting-and-creating-data.md) Teilen der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB. Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial müssen Sie folgende Aktionen ausführen:

1. Fügen Sie eine Eigenschaft an Ihr Modell zum Aufzeichnen des Studenten Registrierungsdatum
2. Ermöglichen Sie es dem Benutzer für die Verwendung des JQuery UI Datepicker-Widget für die Registrierung Datumsauswahl
3. Regeln für die datenvalidierung für das Registrierungsdatum erzwingen

JQuery UI Datepicker-Widgets kann Benutzer ganz einfach ein Datum aus einem Kalender auswählen, die angezeigt werden, wenn das Feld der Benutzer interagiert. Mithilfe dieses Widgets kann praktischer für Benutzer als ein Datum, manuell eingegeben werden. Die Integration des Widgets "DatePicker" auf einer Seite, die modellbindung für Datenoperationen verwendet, erfordert nur wenig zusätzliche Arbeit.

## <a name="add-a-new-property-to-the-model"></a>Fügen Sie eine neue Eigenschaft mit dem Modell

Fügen Sie zunächst eine **"DateTime"** Eigenschaft, um Ihren Schüler/Studenten modellieren und migrieren Sie diese Änderung in der Datenbank. Open **UniversityModels.cs**, und fügen Sie den hervorgehobenen Code hinzu, mit dem Modell "Student".

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Die **RangeAttribute** zum Erzwingen von Validierungsregeln für die Eigenschaft enthalten ist. In diesem Tutorial wird davon ausgegangen, dass Contoso University, am 1. Januar 2013 gegründet wurde und frühere Registrierungsdaten daher nicht gültig sind.

Fügen Sie im Fenster "Package Management", eine Migration mithilfe des Befehls **hinzufügen-Migration AddEnrollmentDate**. Beachten Sie, dass der Code für die Migration die neue Datetime-Spalte der Tabelle "Student" hinzugefügt. Um den Wert übereinstimmen, den Sie in der RangeAttribute angegeben, fügen Sie einen Standardwert für die neue Spalte hinzu, wie im folgenden hervorgehobenen Code gezeigt.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Speichern Sie die Änderung, um die Migrationsdatei.

Sie müssen sich nicht um die Daten erneut zu starten. Öffnen Sie daher **Configuration.cs** in den Ordner "Migrations" und entfernen oder kommentieren Sie den Code in die **Ausgangswert** Methode. Speichern und schließen Sie die Datei.

Führen Sie nun den Befehl **Update-Database**. Beachten Sie, dass die Spalte, die jetzt in der Datenbank vorhanden ist und alle vorhandenen Datensätze den Standardwert für EnrollmentDate haben.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Hinzufügen von dynamischen Steuerelementen für Registrierungsdatum

Fügen Sie jetzt Steuerelemente zum Anzeigen und bearbeiten das Registrierungsdatum. An diesem Punkt wird der Wert über einem Textfeld, das bearbeitet werden. Weiter unten in diesem Tutorial ändern Sie im Textfeld auf die JQuery Datepicker-Widget.

Erstens ist es wichtig zu beachten, dass Sie keine Änderung vornehmen müssen die **AddStudent.aspx** Datei. Das Steuerelement erzeugt wird, wird automatisch die neue Eigenschaft gerendert.

Open **Students.aspx**, und fügen Sie folgenden hervorgehobenen Code hinzu.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Führen Sie die Anwendung, und beachten Sie, dass Sie den Wert des Datums Registrierung festlegen können, indem Sie ein Datum eingeben. Beim Hinzufügen eines neuen Studenten ein:

![bestimmten Datum](integrating-jquery-ui/_static/image1.png)

Alternativ dazu können Sie einen vorhandenen Wert zu bearbeiten:

![Datum bearbeiten](integrating-jquery-ui/_static/image2.png)

Geben das Datum funktioniert, aber es möglicherweise nicht die kundenerfahrung zu sorgen, die Sie bereitstellen möchten. Im nächsten Abschnitt können Sie ein Datum durch einen Kalender auswählen.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installieren von NuGet-Paket zum Arbeiten mit JQuery UI

Die **vom UI** NuGet-Paket ermöglicht eine einfache Integration mit der JQuery-UI-Widgets in Ihre Webanwendung. Um dieses Paket zu verwenden, installieren Sie es über NuGet ein.

![Hinzufügen der vom-Benutzeroberfläche](integrating-jquery-ui/_static/image3.png)

Die Version des vom-Benutzeroberfläche, die Sie installieren kann mit der Version von JQuery in Ihre Anwendung in Konflikt stehen. Führen Sie bevor Sie mit diesem Tutorial fortfahren Ihre Anwendung. Wenn Sie einen JavaScript-Fehler auftritt, müssen Sie die JQuery-Version abstimmen. Sie können entweder die erwartete Version von JQuery zu Ihrem Ordner "Scripts" (Version 1.8.2 zum Zeitpunkt des Schreibens in diesem Tutorial) hinzufügen oder Site.master geben den Pfad zu der JQuery-Datei.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Anpassen der Vorlage "DateTime", "DatePicker" Widget enthält

Die dynamische Datenvorlage für die Bearbeitung eines Datetime-Werts wird das Widget "DatePicker" hinzugefügt werden. Das Widget die Vorlage hinzufügen, wird es automatisch im Formular zum Hinzufügen eines neuen Studenten und in der Rasteransicht für Bearbeitung Schüler/Studenten gerendert. Open **"DateTime"\_Edit.ascx zeigt**, und fügen Sie folgenden hervorgehobenen Code hinzu.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Legen Sie in der CodeBehind-Datei die minimalen und maximalen Datumsangaben für "DatePicker" fest. Wenn Sie diese Werte festlegen, werden Sie verhindert, dass Benutzer ungültige Datumsangaben navigieren. Rufen Sie die minimalen und maximalen Werte aus der **RangeAttribute** für die Eigenschaft "DateTime", sofern vorhanden. Open **"DateTime"\_Edit.ascx.cs**, und fügen Sie folgenden hervorgehobenen Code hinzu, auf der Seite\_Load-Methode.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Führen Sie die Webanwendung, und navigieren Sie zu der Seite "AddStudent". Geben Sie Werte für die Felder aus, und beachten Sie, dass wenn Sie im Textfeld für den Enrollment Date klicken, auf der Kalender angezeigt wird.

![Datumsauswahl](integrating-jquery-ui/_static/image4.png)

Wählen Sie ein Datum aus, und klicken Sie auf **einfügen**. Die RangeAttribute erzwingt die Überprüfung auf dem Server. Durch die MinDate-Eigenschaft für "DatePicker" festlegen, gelten Sie auch Validierung auf dem Client. Der Kalender gestattet keine Benutzer auf ein Datum vor den Wert der MinDate navigieren.

Wenn Sie einen Datensatz in der Rasteransicht bearbeiten, wird der Kalender wird ebenfalls angezeigt.

![DatePicker in GridView-Ansicht](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial haben Sie gelernt, wie ein Widget von JQuery in einem Web Form zu integrieren, die modellbindung verwendet wird.

In den nächsten [Tutorial](using-query-string-values-to-retrieve-data.md), verwenden Sie einen Abfragezeichenfolgenwert bei der Auswahl.

> [!div class="step-by-step"]
> [Zurück](sorting-paging-and-filtering-data.md)
> [Weiter](using-query-string-values-to-retrieve-data.md)
