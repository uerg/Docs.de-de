---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: "Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 2 | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 271c244ab0b9e2524a33ea6ff4d41893ce22472f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 2
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


## <a name="adding-an-automatic-datetime-template"></a>Hinzufügen einer Vorlage für eine automatische "DateTime"

Im ersten Teil dieses Lernprogramms haben Sie gesehen wie Sie das Modell Formatierung explizit angeben, Attribute hinzufügen können und wie Sie explizit die Vorlage angeben können, die zum Rendern des Modells verwendet wird. Z. B. die [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut in den folgenden Code explizit gibt die Formatierung für die `ReleaseDate` Eigenschaft.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Im folgenden Beispiel die [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut mit dem `Date` Enumeration gibt an, dass die Date-Vorlage zum Rendern des Modells verwendet werden soll. Wenn keine Datumsvorlage in Ihrem Projekt vorhanden ist, wird die integrierten Vorlage verwendet.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Allerdings ASP. MVC Aktualisierungsoption Typ Abgleich mit der Konvention-Over-Konfiguration kann anhand einer Vorlage, die den Namen eines Typs entspricht. Dadurch können Sie eine Vorlage erstellen, die Daten automatisch formatiert, ohne Attribute oder Code überhaupt. Für diesen Teil des Lernprogramms, erstellen Sie eine Vorlage, die automatisch auf Modelleigenschaften vom Typ angewendet wird ["DateTime"](https://msdn.microsoft.com/en-us/library/system.datetime.aspx). Sie brauchen ein Attribut oder einer anderen Konfiguration verwenden, um anzugeben, dass die Vorlage verwendet werden soll, um Eigenschaften für alle Modelle des Typs zu rendern ["DateTime"](https://msdn.microsoft.com/en-us/library/system.datetime.aspx).

Sie erfahren auch eine Möglichkeit, die Anzeige der einzelnen Eigenschaften oder sogar einzelne Felder anpassen.

Um zu beginnen, sehen wir entfernen Sie vorhandene Formatierungsinformationen und vollständige Datumsangaben in der Anwendung anzeigen.

Öffnen der *Movie.cs* Datei, und kommentieren Sie dann die `DataType` Attribut für die `ReleaseDate` Eigenschaft:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Beachten Sie, dass die `ReleaseDate` Eigenschaft zeigt jetzt das Datum und die Zeit, da dies die Standardeinstellung ist, wenn keine Formatierungsinformationen bereitgestellt wird.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>CSS-Formatvorlagen zu Testzwecken neue Vorlagen hinzufügen

Bevor Sie eine Vorlage zum Formatieren von Datumsangaben erstellen, fügen Sie wenige CSS-Stilregeln, die Sie auf die neuen Vorlagen anwenden können. Dadurch können Sie sicherstellen, dass die gerenderte Seite die neue Vorlage verwendet wird.

Öffnen der *Content\Site.cs*s-Datei, und fügen Sie die folgenden CSS-Regeln an das Ende der Datei hinzu:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Hinzufügen von "DateTime" Anzeigevorlagen

Jetzt können Sie die neue Vorlage erstellen. In der *Views\Movies* Ordner, erstellen Sie eine *DisplayTemplates* Ordner.

In der *Views\Shared* Ordner, erstellen Sie eine *DisplayTemplates* Ordner und eine *EditorTemplates* Ordner.

Die Anzeigevorlagen im die *Views\Shared\DisplayTemplates* Ordner werden von allen Controllern verwendet werden. Die Anzeigevorlagen im die *Views\Movie\DisplayTemplates* Ordner verwendet werden nur von der `Movie` Controller. (Wenn in beiden Ordnern, die Vorlage in eine Vorlage mit demselben Namen angezeigt wird der *Views\Movie\DisplayTemplates* Ordner –, also die spezifischere Vorlage – hat Vorrang vor für Ansichten, die zurückgegeben werden, indem die `Movie` Controller.)

In **Projektmappen-Explorer**, erweitern Sie die *Ansichten* Ordner, erweitern Sie die *Shared* Ordner ein, und klicken Sie dann mit der rechten Maustaste die *Views\Shared\DisplayTemplates* Ordner.

Klicken Sie auf **hinzufügen** , und klicken Sie dann auf **Ansicht**. Die **Ansicht hinzufügen** Dialogfeld wird angezeigt.

In der **Sichtname** geben `DateTime`. (Sie müssen diesen Namen verwenden, um den Namen des Typs übereinstimmen.)

Wählen Sie die **als eine Teilansicht erstellen** Kontrollkästchen. Stellen Sie sicher, dass die **mithilfe eine Seite Layout- oder Masterseite** und **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert sind.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Klicken Sie auf **Hinzufügen**. Ein *DateTime.cshtml* Vorlage wird erstellt, der *Views\Shared\DisplayTemplates*.

Die folgende Abbildung zeigt die *Ansichten* Ordner **Projektmappen-Explorer** nach der `DateTime` Anzeige- und -Editor-Vorlagen erstellt werden.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Öffnen der *Views\Shared\DisplayTemplates\DateTime.cshtml* Datei, und fügen Sie das folgende Markup, das verwendet die [String.Format](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) Methode, um die Eigenschaft als Datum ohne Uhrzeit formatieren. (Die `{0:d}` Format Gibt kurzen Datumsformat.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Wiederholen Sie diesen Schritt zum Erstellen einer `DateTime` Vorlage in der *Views\Movie\DisplayTemplates* Ordner. Verwenden Sie den folgenden Code in die *Views\Movie\DisplayTemplates\DateTime.cshtml* Datei.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

Die `loud-1` CSS-Klasse bewirkt, dass das Datum in roter Fettschrift angezeigt werden. Hinzugefügte der `loud-1` CSS-Klasse, ebenso wie eine temporäre Maßnahme, sodass Sie leicht erkennen können, wann diese bestimmten Vorlage verwendet wird.

Was Sie schon wird erstellt und angepasste Vorlagen, die ASP.NET zum Anzeigen von Datumsangaben verwenden. Weitere allgemeine Vorlage (in der *Views\Shared\DisplayTemplates* Ordner) zeigt ein einfache kurzes Datum. Die Vorlage, die speziell für die `Movie` Controller (in der *Views\Movies\DisplayTemplates* Ordner) ein kurzes Datum, das ebenfalls formatiert wird als roter Fettschrift angezeigt.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Der Browser rendert die Indexansicht für die Anwendung.

Die `ReleaseDate` Eigenschaft zeigt jetzt das Datum in roter Fettschrift ohne Uhrzeit. Dadurch können Sie bestätigen, dass die `DateTime` Hilfsvorlage in der *Views\Movies\DisplayTemplates* Ordner ausgewählt ist, über die `DateTime` Hilfsvorlage im freigegebenen Ordner (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Benennen Sie jetzt die *Views\Movies\DisplayTemplates\DateTime.cshtml* Datei *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Dieses Mal die `ReleaseDate` Eigenschaft zeigt ein Datum ohne Uhrzeit und ohne die roter Fettschrift an. Dies veranschaulicht, dass eine Vorlage mit dem Namen der Daten eingeben (in diesem Fall `DateTime`) wird automatisch verwendet, um alle Modelleigenschaften dieses Typs anzuzeigen. Nachdem Sie umbenannt haben die *DateTime.cshtml* Datei wird in *LoudDateTime.cshtml*, ASP.NET nicht mehr gefunden, eine Vorlage in der *Views\Movies\DisplayTemplates* Ordner, damit es verwendet die *DateTime.cshtml* Vorlage aus der * Views\Movies\Shared\* Ordner.

(Die Vorlage entsprechenden ist Groß-/Kleinschreibung zu beachten, damit Sie den Dateinamen für die Vorlage mit jeder Schreibweise erstellt haben, konnten. Deutet, *DATETIME.chstml, datetime.cshtml*, und *DaTeTiMe.cshtml* entsprechen alle dem `DateTime` Typ.)

Um zu überprüfen: an diesem Punkt der `ReleaseDate` Feld wird angezeigt mit der *Views\Movies\DisplayTemplates\DateTime.cshtml* Vorlage, die die Daten unter Verwendung ein kurzen Datumsformat angezeigt, jedoch Fügt andernfalls keine besonderen Format.

### <a name="using-uihint-to-specify-a-display-template"></a>Verwenden von UIHint zum Angeben einer Anzeigevorlage

Wenn Ihre Webanwendung viele hat `DateTime` Felder wird standardmäßig alle oder die meisten von ihnen im DATEONLY-Format angezeigt werden soll die *DateTime.cshtml* Vorlage ist ein guter Ansatz. Aber was geschieht, wenn Sie einige Daten denen der vollständiges Datum und Uhrzeit angezeigt werden sollen? Kein Problem. Sie können eine weitere Vorlage erstellen und verwenden Sie die [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut, um die Formatierung für die vollständiges Datum und Uhrzeit angeben. Sie können dann selektiv auf diese Vorlage anwenden. Sie können die [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut auf der Ebene des Modells oder Sie können angeben, welche Vorlage in einer Ansicht. In diesem Abschnitt erfahren Sie, wie Sie verwenden die `UIHint` Attribut, um die Formatierung für einige Instanzen von Datum / Uhrzeit-Felder selektiv ändern.

Öffnen der *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* -Datei und Ersetzen Sie den vorhandenen Code durch Folgendes:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Dies bewirkt, dass das vollständiges Datum und die Uhrzeit angezeigt werden und fügt die CSS-Klasse, die den Text Grün und große macht.

Öffnen der *Movie.cs* und fügen die [UIHint](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) -Attribut auf die `ReleaseDate` -Eigenschaft verwenden, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Das weist ASP.NET MVC, die beim Anzeigen der `ReleaseDate` Eigenschaft (insbesondere und nicht einfach ein beliebiges `DateTime` Objekt), sollte die *LoudDateTime.cshtml* Vorlage.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Beachten Sie, dass die `ReleaseDate` Eigenschaft zeigt jetzt das Datum und die Uhrzeit in großer Schriftart grün.

Zurück zu den `UIHint` Attribut in der *Movie.cs* Datei, und kommentieren Sie es aus also die *LoudDateTime.cshtml* Vorlage wird nicht verwendet werden. Führen Sie die Anwendung erneut aus. Das Veröffentlichungsdatum ist nicht groß und grün angezeigt. Dies stellt sicher, dass die *Views\Shared\DisplayTemplates\DateTime.cshtml* Vorlage wird in den Index und Details Ansichten verwendet.

Wie bereits erwähnt, können Sie auch eine Vorlage in einer Ansicht anwenden, die dadurch Sie die Vorlage für eine einzelne Instanz einiger Daten anwenden können. Öffnen der *Views\Movies\Details.cshtml* anzeigen. Hinzufügen `"LoudDateTime"` als zweiten Parameter von der [Html.DisplayFor](https://msdn.microsoft.com/en-us/library/ee407420.aspx) rufen Sie für die `ReleaseDate` Feld. Der fertige Code sieht wie folgt aus:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Gibt an, dass die `LoudDateTime` Vorlage sollte verwendet werden, zum Anzeigen der Modelleigenschaft, unabhängig davon, welche Attribute für das Modell angewendet werden.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Stellen Sie sicher, dass die Indexseite Film verwendet die *Views\Shared\DisplayTemplates\DateTime.cshtml* Vorlage (Rot fett) und die *Movie\Details* wird mithilfe der *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* Vorlage (große und Grün).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Im nächsten Abschnitt erstellen Sie eine Vorlage für einen komplexen Typ.

>[!div class="step-by-step"]
[Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
