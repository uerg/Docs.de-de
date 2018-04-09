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
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrieren von JQuery UI Datepicker mit wurden die modellbindung und WebForms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource). Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.
> 
> Dieses Lernprogramm veranschaulicht das Hinzufügen der JQuery UI [Datepicker-Widget](http://jqueryui.com/datepicker/) in ein Web Form, und verwenden Modell Binden an um die Datenbank mit dem ausgewählten Wert zu aktualisieren.
> 
> Dieses Lernprogramm baut auf das Projekt erstellt, der [erste](retrieving-data.md) und [zweite](updating-deleting-and-creating-data.md) Teile der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar. Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:

1. Fügen Sie eine Eigenschaft mit dem Modell die Studenten Registrierungsdatum aufzeichnen
2. Ermöglichen Sie dem Benutzer, wählen Sie das Registrierungsdatum mithilfe der JQuery UI Datepicker-widget
3. Überprüfungsregeln für das Registrierungsdatum erzwingen

Das Widget JQuery UI Datepicker kann Benutzer problemlos ein Datum aus dem Kalender auswählen, die angezeigt werden, wenn der Benutzer mit dem Feld interagiert. Mithilfe dieses Widgets kann einfacher für Benutzer als ein Datum, manuell eingegeben werden. Integrieren von Datepicker Widgets in einer Seite, die wurden die modellbindung für Datenvorgänge verwendet, ist nur eine kleine Menge an zusätzliche Arbeit erforderlich.

## <a name="add-a-new-property-to-the-model"></a>Fügen Sie eine neue Eigenschaft mit dem Modell

Fügen Sie zunächst eine **"DateTime"** Eigenschaft, um Ihre Student modellieren und migrieren Sie diese Änderung in der Datenbank. Open **UniversityModels.cs**, und das Modell Student den hervorgehobenen Code hinzuzufügen.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Die **RangeAttribute** zum Erzwingen von Validierungsregeln für die Eigenschaft enthalten ist. In diesem Lernprogramm wird davon ausgegangen, dass Contoso University am 1. Januar 2013 gegründet wurde und frühere Registrierungsdaten daher nicht gültig sind.

Fügen Sie im Fenster Paketverwaltung eine Migration mithilfe des Befehls **hinzufügen Migration AddEnrollmentDate**. Beachten Sie, dass der Code für die Migration der neue Datetime-Spalte der Student-Tabelle hinzugefügt. Um mit dem Wert überein, den Sie in der RangeAttribute angegeben haben, fügen Sie einen Standardwert für die neue Spalte, wie in den folgenden hervorgehobenen Code dargestellt.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Speichern Sie die Änderung in der Migrationsdatei.

Sie müssen nicht erneut Ausgangswert für die Daten. Öffnen Sie deshalb **"Configuration.cs"** in den Ordner und entfernen, oder kommentieren Sie den Code in der **Ausgangswert** Methode. Speichern und schließen Sie die Datei.

Führen Sie nun den Befehl **Update-Database '**. Beachten Sie, dass die Spalte in der Datenbank und aller vorhandenen Datensätze jetzt vorhanden ist der Standardwert für EnrollmentDate aufweisen.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Hinzufügen von dynamischen Steuerelementen für Registrierungsdatum

Fügen Sie jetzt Steuerelemente zum Anzeigen und Bearbeiten der Registrierungsdatum. An diesem Punkt wird der Wert durch ein Textfeld, das bearbeitet werden. Später in diesem Lernprogramm ändern Sie im Textfeld auf die JQuery Datepicker-Widget.

Zunächst ist es wichtig zu beachten, dass Sie nicht ändern, müssen die **AddStudent.aspx** Datei. Das Steuerelement erzeugt wird, wird automatisch die neue Eigenschaft gerendert.

Open **Students.aspx**, und fügen Sie den folgenden hervorgehobenen Code hinzu.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Führen Sie die Anwendung, und beachten Sie, dass Sie den Wert des Datums Registrierung festlegen können, indem Sie ein Datum eingeben. Wenn Sie einen neuen Studenten hinzufügen:

![Zeitpunkt](integrating-jquery-ui/_static/image1.png)

Oder Bearbeiten eines vorhandenen Werts:

![Datum bearbeiten](integrating-jquery-ui/_static/image2.png)

Geben Sie die Date-funktioniert, aber es möglicherweise nicht der benutzerfreundlichkeit, die Sie bereitstellen möchten. Aktivieren Sie im nächsten Abschnitt ein Datum durch einen Kalender auswählen.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installieren von NuGet-Paket zum Arbeiten mit JQuery UI

Die **vom UI** NuGet-Paket ermöglicht eine einfache Integration der JQuery UI-Komponenten in die Webanwendung. Um dieses Paket zu verwenden, installieren Sie es über NuGet.

![Fügen Sie vom UI hinzu.](integrating-jquery-ui/_static/image3.png)

Die Version des vom UI, die Sie installieren möglicherweise in Konflikt mit der Version von JQuery in Ihrer Anwendung. Bevor Sie mit diesem Lernprogramm fortfahren, wiederholen Sie dann ein Ausführen Ihrer Anwendung. Wenn Sie einen JavaScript-Fehler auftritt, müssen Sie die JQuery-Version abstimmen. Sie können entweder die erwartete Version von JQuery Ordner "Skripts" (Version 1.8.2 zum Zeitpunkt des Schreibens dieses Lernprogramms) hinzufügen oder in Site.master Geben Sie den Pfad zu der JQuery-Datei.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Passen Sie an "DateTime" Vorlage Einbeziehung Datepicker-widget

Die dynamische Datenvorlage für die Bearbeitung eines Datetime-Werts wird das Widget Datepicker hinzugefügt werden. Das Widget die Vorlage hinzufügen, wird es automatisch im Formular zum Hinzufügen eines neuen Studenten und in der Rasteransicht für Bearbeitung Studenten gerendert. Open **"DateTime"\_Edit.ascx zeigt**, und fügen Sie den folgenden hervorgehobenen Code hinzu.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

In der CodeBehind-Datei wird die minimalen und maximalen Datumsangaben für die DatePicker festgelegt werden. Wenn Sie diese Werte festlegen, werden Sie verhindern, dass Benutzer Navigieren auf ungültige Datumsangaben. Rufen Sie die minimalen und maximalen Werte aus den **RangeAttribute** für die Eigenschaft "DateTime", sofern vorhanden. Open **"DateTime"\_Edit.ascx.cs**, und fügen Sie folgenden hervorgehobenen Code hinzu, auf der Seite\_Load-Methode.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Führen Sie die Webanwendung, und navigieren Sie zu der Seite "AddStudent". Geben Sie Werte für die Felder aus, und beachten Sie, dass beim Klicken auf das Textfeld für Registrierungsdatum Kalender angezeigt wird.

![Datumsauswahl](integrating-jquery-ui/_static/image4.png)

Wählen Sie ein Datum aus, und klicken Sie auf **einfügen**. Die RangeAttribute erzwingt die Überprüfung auf dem Server. Indem die Datepicker MinDate-Eigenschaft festlegen, gelten Sie auch Validierung auf dem Client. Der Kalender ermöglicht kein Benutzer auf ein Datum vor dem Wert der MinDate navigieren.

Wenn Sie einen Datensatz in der Rasteransicht bearbeiten, wird der Kalender ebenfalls angezeigt.

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Lernprogramm haben Sie gelernt, wie eine JQuery-Widget in ein Webformular integriert, das modellbindung verwendet wird.

In der nächsten [Lernprogramm](using-query-string-values-to-retrieve-data.md), verwenden Sie einen Wert der Abfragezeichenfolge bei der Auswahl.

> [!div class="step-by-step"]
> [Zurück](sorting-paging-and-filtering-data.md)
> [Weiter](using-query-string-values-to-retrieve-data.md)
