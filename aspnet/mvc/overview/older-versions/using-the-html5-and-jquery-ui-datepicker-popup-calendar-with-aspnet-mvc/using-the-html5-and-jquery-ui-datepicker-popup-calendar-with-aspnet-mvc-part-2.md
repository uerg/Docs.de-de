---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 2818e69f912509a6392333bad8f5a1afa55884f6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375288"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 2
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


## <a name="adding-an-automatic-datetime-template"></a>Hinzufügen einer Vorlage für eine automatische "DateTime"

Im ersten Teil dieses Lernprogramms haben gesehen, wie Sie das Modell Formatierung explizit angeben, Attribute hinzufügen können, und wie Sie explizit die Vorlage angeben können, die zum Rendern des Modells verwendet wird. Z. B. die [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut in den folgenden Code explizit gibt an, die Formatierung für die `ReleaseDate` Eigenschaft.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Im folgenden Beispiel die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attributs unter Verwendung der `Date` -Enumeration gibt an, dass die Date-Vorlage zum Rendern des Modells verwendet werden soll. Wenn kein Datumsvorlage in Ihrem Projekt vorhanden ist, wird die integrierten Vorlage verwendet.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Allerdings ASP. MVC kann ausführen typübereinstimmung mit Konvention-vor-Konfiguration, anhand einer Vorlage mit dem Namen eines Typs übereinstimmt. Dadurch können Sie eine Vorlage zu erstellen, die Daten automatisch formatiert, Attribute oder Code ganz ohne. Für diesen Teil des Tutorials, erstellen Sie eine Vorlage, die automatisch auf die Eigenschaften des Modells vom Typ ["DateTime"](https://msdn.microsoft.com/library/system.datetime.aspx). Sie müssen kein Attribut oder andere Konfiguration verwenden, um anzugeben, dass die Vorlage verwendet werden soll, um alle Eigenschaften des Modells vom Typ zu rendern ["DateTime"](https://msdn.microsoft.com/library/system.datetime.aspx).

Außerdem erfahren Sie, eine Möglichkeit, die die Anzeige der einzelnen Eigenschaften oder sogar einzelne Felder anpassen.

Um zu beginnen, entfernen Sie vorhandene Formatierungsinformationen lassen Sie uns, und zeigen Sie die vollständigen Daten in der Anwendung an.

Öffnen der *Movie.cs* Datei, und kommentieren Sie die `DataType` -Attribut für die `ReleaseDate` Eigenschaft:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Beachten Sie, dass die `ReleaseDate` Eigenschaft zeigt jetzt das Datum und die Zeit, da dies die Standardeinstellung ist, wenn keine Formatierungsinformationen bereitgestellt wird.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Hinzufügen von CSS-Formatvorlagen für das Testen der neuer Vorlagen

Vor dem Erstellen einer Vorlage zum Formatieren von Datumsangaben, fügen Sie ein Paar eines CSS-Formatierungsregeln, die Sie auf die neuen Vorlagen anwenden können. Diese können Sie überprüfen, ob die gerenderte Seite die neue Vorlage verwendet.

Öffnen der *Content\Site.cs*s-Datei, und fügen Sie die folgenden CSS-Regeln an das Ende der Datei hinzu:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Hinzufügen von "DateTime" angezeigt-Vorlagen

Jetzt können Sie die neue Vorlage erstellen. In der *ansichten\filme* Ordner, erstellen Sie eine *DisplayTemplates* Ordner.

In der *Views\Shared* Ordner, erstellen Sie eine *DisplayTemplates* Ordner und ein *EditorTemplates* Ordner.

Die Anzeigevorlagen im der *Views\Shared\DisplayTemplates* Ordner wird von allen Controllern verwendet werden. Die Anzeigevorlagen im der *Views\Movie\DisplayTemplates* Ordner verwendet werden nur von der `Movie` Controller. (Wenn angezeigt, eine Vorlage mit dem gleichen Namen in beiden Ordnern, die Vorlage in wird der *Views\Movie\DisplayTemplates* Ordner – d. h. die spezifischere Vorlage – hat Vorrang vor für Ansichten, die vom der `Movie` Controller.)

In **Projektmappen-Explorer**, erweitern Sie die *Ansichten* Ordner, erweitern Sie die *Shared* Ordner, und klicken Sie dann mit der rechten Maustaste die *Views\Shared\DisplayTemplates* Ordner.

Klicken Sie auf **hinzufügen** , und klicken Sie dann auf **Ansicht**. Die **Ansicht hinzufügen** Dialogfeld wird angezeigt.

In der **Sichtname** geben `DateTime`. (Sie müssen diesen Namen verwenden, um mit dem Namen des Typs übereinstimmen.)

Wählen Sie die **erstellen als Teilansicht** Kontrollkästchen. Stellen Sie sicher, dass die **mithilfe eine Seite Layout- oder Masterseite** und **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert sind.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Klicken Sie auf **Hinzufügen**. Ein *dem Namen* Vorlage wird erstellt, der *Views\Shared\DisplayTemplates*.

Die folgende Abbildung zeigt die *Ansichten* Ordner **Projektmappen-Explorer** nach der `DateTime` Anzeige- und -Editor-Vorlagen erstellt werden.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Öffnen der *Views\Shared\DisplayTemplates\DateTime.cshtml* -Datei und fügen Sie das folgende Markup, das verwendet die [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) Methode, um die Eigenschaft als Datum ohne die Zeit, zu formatieren. (Die `{0:d}` Format Gibt kurzen Datumsformat an.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Wiederholen Sie diesen Schritt zum Erstellen einer `DateTime` Vorlage in der *Views\Movie\DisplayTemplates* Ordner. Verwenden Sie den folgenden Code in die *Views\Movie\DisplayTemplates\DateTime.cshtml* Datei.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

Die `loud-1` CSS-Klasse bewirkt, dass das Datum in roter Fettschrift angezeigt zu werden. Sie hinzugefügt haben die `loud-1` CSS-Klasse, wie eine temporäre Maßnahme, sodass Sie leichter finden können, wann dieser speziellen Vorlage verwendet wird.

Was genau Sie gemacht haben, erstellt und angepasste Vorlagen, die ASP.NET, zum Anzeigen von Datumsangaben verwendet wird. Die allgemeine Vorlage (in der *Views\Shared\DisplayTemplates* Ordner) zeigt ein einfaches kurzes Datumsformat. Die Vorlage, die speziell für die `Movie` Controller (in der *Views\Movies\DisplayTemplates* Ordner) ein kurzes Datum, das ebenfalls formatiert wird als roter Fettschrift angezeigt.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Der Browser rendert die Indexansicht für die Anwendung.

Die `ReleaseDate` Eigenschaft zeigt jetzt das Datum in roter Fettschrift ohne die Zeit. Dadurch können Sie bestätigen, dass die `DateTime` Hilfsvorlage in die *Views\Movies\DisplayTemplates* Ordner ausgewählt ist, über die `DateTime` Hilfsvorlage im freigegebenen Ordner (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Benennen Sie jetzt die *Views\Movies\DisplayTemplates\DateTime.cshtml* Datei *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Dieses Mal die `ReleaseDate` Eigenschaft zeigt ein Datum aus, ohne die Zeit und ohne die roter Fettschrift an. Dies verdeutlicht, dass eine Vorlage mit dem Namen des ausgewählten Typs (in diesem Fall `DateTime`) wird automatisch verwendet, um alle Modelleigenschaften des betreffenden Typs anzuzeigen. Nachdem Sie die umbenannt der *dem Namen* Datei *LoudDateTime.cshtml*, ASP.NET nicht mehr finden Sie eine Vorlage in der *Views\Movies\DisplayTemplates* Ordner, damit sie verwendet die *dem Namen* Vorlage aus den * Views\Movies\Shared\* Ordner.

(Die Vorlage ist Groß-/Kleinschreibung, damit Sie der Name der Vorlagendatei mit Groß-/Kleinschreibung erstellt haben, können. Z. B. *DATETIME.chstml, dem Namen*, und *dem Namen* entsprechen alle dem `DateTime` geben.)

Um zu überprüfen: an diesem Punkt die `ReleaseDate` Feld wird angezeigt wird, mit der *Views\Movies\DisplayTemplates\DateTime.cshtml* Vorlage, die die Daten, die mit dem kurzen Datumsformat angezeigt, aber andernfalls keine spezielles Format hinzugefügt.

### <a name="using-uihint-to-specify-a-display-template"></a>Verwenden von UIHint zum Angeben einer Anzeigevorlage

Wenn Ihre Webanwendung, die viele verfügt `DateTime` Felder wird standardmäßig, die Sie alle oder die meisten von ihnen im nur-Datum-Format, anzeigen möchten die *dem Namen* Vorlage ist ein guter Ansatz. Aber was geschieht, wenn Sie einige Daten, in dem das vollständige Datum und die Uhrzeit angezeigt werden sollen? Kein Problem. Sie können eine weitere Vorlage erstellen und verwenden Sie die ["UIHint"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut Formatierung für das vollständige Datum und die Uhrzeit angeben. Sie können diese Vorlage dann selektiv anwenden. Sie können die ["UIHint"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) -Attributs auf der Ebene des Modells oder Sie können die Vorlage in der Ansicht angeben. In diesem Abschnitt sehen Sie, wie Sie mit der `UIHint` Attribut, um die Formatierung für einige Instanzen von Datum / Uhrzeit-Feldern selektiv zu ändern.

Öffnen der *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* Datei, und Ersetzen Sie den vorhandenen Code durch Folgendes:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Dies bewirkt, dass das vollständiges Datum und die Uhrzeit, die angezeigt werden, und fügt die CSS-Klasse, die der Text, grüne und große macht.

Öffnen der *Movie.cs* -Datei und fügen die ["UIHint"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) -Attribut auf die `ReleaseDate` -Eigenschaft, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Dadurch weiß ASP.NET MVC, die beim Anzeigen der `ReleaseDate` Eigenschaft (insbesondere und nicht einfach ein beliebiges `DateTime` Objekt), muss bei Verwendung der *LoudDateTime.cshtml* Vorlage.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Beachten Sie, dass die `ReleaseDate` Eigenschaft zeigt jetzt das Datum und Uhrzeit in großer Schriftart grün.

Wechseln Sie zurück zur der `UIHint` -Attribut in der *Movie.cs* Datei, und kommentieren Sie ihn aus daher *LoudDateTime.cshtml* Vorlage wird nicht verwendet werden. Führen Sie die Anwendung erneut aus. Das Datum der Veröffentlichung ist nicht groß und grün angezeigt. Dies stellt sicher, dass die *Views\Shared\DisplayTemplates\DateTime.cshtml* Vorlage wird verwendet, in den Index und Details.

Wie bereits erwähnt, können Sie auch eine Vorlage in einer Sicht anwenden, die dadurch Sie die Vorlage für eine einzelne Instanz einiger Daten anwenden können. Öffnen der *Views\Movies\Details.cshtml* anzeigen. Hinzufügen `"LoudDateTime"` als zweiten Parameter von der [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) rufen Sie für die `ReleaseDate` Feld. Der fertige Code sieht wie folgt aus:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Dies gibt an, dass die `LoudDateTime` Vorlage sollte verwendet werden, zum Anzeigen der Modelleigenschaft, unabhängig davon, welche Attribute für das Modell angewendet werden.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Stellen Sie sicher, dass die Indexseite Film verwendet die *Views\Shared\DisplayTemplates\DateTime.cshtml* Vorlage (Rot fett) und die *Movie\Details* wird mithilfe der *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* Vorlage ("Groß" und "Grün").

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Im nächsten Abschnitt erstellen Sie eine Vorlage für einen komplexen Typ.

> [!div class="step-by-step"]
> [Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
