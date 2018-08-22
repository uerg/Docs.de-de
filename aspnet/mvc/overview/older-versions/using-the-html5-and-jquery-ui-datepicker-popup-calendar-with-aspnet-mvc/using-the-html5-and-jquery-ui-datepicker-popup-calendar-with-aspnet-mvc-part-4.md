---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 58c0ae54e31c2cb4e85da11f7402ea248d9dfb3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827181"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 4
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


### <a name="adding-a-template-for-editing-dates"></a>Hinzufügen einer Vorlage für die Bearbeitung von Daten

In diesem Abschnitt erstellen Sie eine Vorlage für die Bearbeitung von Datumsangaben, die angewendet werden, wenn ASP.NET MVC zeigt die Benutzeroberfläche zum Bearbeiten der Eigenschaften des Modells, die mit markiert sind die **Datum** Enumeration der [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) der Standardtitel. Die Vorlage wird nur das Datum rendern. Zeit wird nicht angezeigt werden. Verwenden Sie in der Vorlage die [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) Popupkalenders aus, um eine Möglichkeit zum Bearbeiten von Daten bereitzustellen.

Öffnen Sie zunächst die *Movie.cs* -Datei und fügen die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut mit der **Datum** Enumeration, die `ReleaseDate` -Eigenschaft, wie im folgenden Code gezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Dieser Code bewirkt, dass die `ReleaseDate` Feld angezeigt werden, ohne die Zeit in beiden Vorlagen anzeigen und Bearbeiten von Vorlagen. Wenn Ihre Anwendung enthält eine *date.cshtml* Vorlage in der *Views\Shared\EditorTemplates* Ordner oder in der *Views\Movies\EditorTemplates* -Ordner, die Vorlage wird verwendet, um alle für das Renderziel `DateTime` Eigenschaft während der Bearbeitung. Andernfalls wird die integrierte vorlagensystem von ASP.NET die Eigenschaft als Datum angezeigt.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie einen Link "Bearbeiten", um sicherzustellen, dass das Eingabefeld für das Datum der Veröffentlichung nur das Datum angezeigt wird.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

In **Projektmappen-Explorer**, erweitern Sie die *Ansichten* Ordner, erweitern Sie die *Shared* Ordner, und klicken Sie dann mit der rechten Maustaste die *Views\Shared\EditorTemplates* Ordner.

Klicken Sie auf **hinzufügen**, und klicken Sie dann auf **Ansicht**. Die **Ansicht hinzufügen** Dialogfeld wird angezeigt.

In der **Sichtname** geben &quot;Datum&quot;.

Wählen Sie die **erstellen als Teilansicht** Kontrollkästchen. Stellen Sie sicher, dass die **mithilfe eine Seite Layout- oder Masterseite** und **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert sind.

Klicken Sie auf **Hinzufügen**. Die *Views\Shared\EditorTemplates\Date.cshtml* Vorlage erstellt wird.

Fügen Sie den folgenden Code der *Views\Shared\EditorTemplates\Date.cshtml* Vorlage.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

Die erste Zeile deklariert werden das Modell eine `DateTime` Typ. Obwohl Sie müssen nicht den Modelltyp in Bearbeitung zu deklarieren und Vorlagen anzuzeigen, ist es eine bewährte Methode, sodass Sie während der Kompilierung Überprüfen des Modells an die Ansicht übergeben wird. (Ein weiterer Vorteil ist, dass Sie anschließend IntelliSense für das Modell in Visual Studio in der Ansicht erhalten.) Wenn Sie der Typ des Modells nicht deklariert ist, ASP.NET MVC betrachtet sie als eine [dynamische](https://msdn.microsoft.com/library/dd264741.aspx) geben, und es ist keine Kompilierung typüberprüfung. Wenn Sie das Modell soll deklarieren eine `DateTime` Typ, wird sie stark typisiert.

Die zweite Zeile ist nur literalen HTML-Markup, das zeigt &quot;Datum mithilfe einer Vorlage&quot; vor ein Datumsfeld. Sie werden diese Zeile vorübergehend verwenden, um sicherzustellen, dass diese Datumsvorlage verwendet wird.

Die nächste Zeile ist ein ["HTML.TextBox"](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) Hilfsprogramm, das rendert eine `input` Feld, das ein Textfeld ist. Der dritte Parameter für das Hilfsprogramm verwendet einen anonymen Typ, um die Klasse für das Textfeld festzulegen `datefield` und den Typ auf `date`. (Da `class` ist ein reservierter in c# müssen Sie verwenden die `@` escape-Zeichen die `class` -Attribut in der C#-Parser.)

Die `date` Typ ist ein HTML5-Eingabe-Typ, der HTML5-fähigen Browser zum Rendern eines HTML5-Kalender-Steuerelements ermöglicht. Später fügen Sie JavaScript-Code zum Einbinden von "DatePicker" jQuery auf die `Html.TextBox` Element mit dem `datefield` Klasse.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Können Sie sicherstellen, dass die `ReleaseDate` -Eigenschaft in der Bearbeitungsansicht verwendet die Bearbeitungsvorlage, da die Vorlage angezeigt &quot;Datum mithilfe einer Vorlage&quot; kurz vor dem Ausführen der `ReleaseDate` Text-Eingabefeld, wie in dieser Abbildung dargestellt:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Zeigen Sie den Quellcode der Seite in Ihrem Browser. (Z. B. mit der rechten Maustaste in der Seite, und wählen Sie **Quelltext anzeigen**.) Das folgende Beispiel zeigt einen Teil des Markups auf der Seite zur Veranschaulichung der `class` und `type` Attribute in der gerenderten HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Wechseln Sie zurück zur der *Views\Shared\EditorTemplates\Date.cshtml* Vorlage, und Entfernen der &quot;Datum mithilfe einer Vorlage&quot; Markup. Jetzt sieht die vollständige Vorlage wie folgt aus:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Hinzufügen von jQuery UI Datepicker-Popupkalenders mit NuGet

In diesem Abschnitt fügen Sie der [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) -Popupkalenders mit der Date-Edit-Vorlage. Die [jQuery-Benutzeroberfläche](http://jqueryui.com/) -Bibliothek bietet Unterstützung für Animationen, Erweiterte Effekte und anpassbare Widgets. Es basiert auf JavaScript-Bibliothek jQuery. Das Datepicker-Popupkalenders ist es einfach und natürlich Eingabe von Datumsangaben, die unter Verwendung eines anderen nicht durch Eingabe einer Zeichenfolge. Die Popupkalenders beschränkt außerdem rechtliche Datumsangaben Benutzer – gewöhnlichen Text-Eintrag für ein Datum würde Ihnen ermöglichen, die etwa so geben Sie `2/33/1999` (Februar 33rd, 1999), aber die [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) Popupkalenders gestattet, die nicht.

Zunächst müssen Sie die jQuery-UI-Bibliotheken zu installieren. Zu diesem Zweck verwenden Sie NuGet die Paket-Manager ist, der in SP1-Version von Visual Studio 2010 und Visual Web Developer enthalten ist.

In Visual Web Developer aus der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager** und wählen Sie dann **NuGet-Pakete verwalten**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Hinweis: Wenn die **Tools** Menü nicht angezeigt werden soll die **Bibliothekspaket-Manager** Befehls müssen Sie NuGet installieren mithilfe der Anweisungen auf der [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) auf der Seite der NuGet-Website.   
  
Wenn Sie von Visual Studio anstelle von Visual Web Developer verwenden die **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager** und wählen Sie dann **Bibliothekspaketverweis hinzufügen**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

In der **MVCMovie - NuGet-Pakete verwalten** Dialogfeld klicken Sie auf die **Online** links auf die Registerkarte, und geben Sie dann &quot;jQuery.UI&quot; in das Suchfeld. Wählen Sie j **Abfrage UI-WIdgets: Datepicker**, und wählen Sie dann die **installieren** Schaltfläche.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

Diese Debugversionen und minimierte jQuery UI Core und dem jQuery UI DatePicker-Versionen werden von NuGet zu Ihrem Projekt hinzugefügt:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Hinweis: Die Debugversionen (die Dateien ohne die *. min.js* Erweiterung) eignen sich für das Debuggen, aber in einer Produktionssite, würden Sie nur die minimierten Versionen einschließen.

Um der Datumsauswahl der jQuery tatsächlich verwenden zu können, müssen Sie zum Erstellen eines jQuery-Skripts, das sich das Kalender-Widget auf die Bearbeitungsvorlage verknüpft sind. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Skripts* Ordner, und wählen **hinzufügen**, klicken Sie dann **neues Element**, und klicken Sie dann **JScript Datei**. Nennen Sie die Datei *DatePickerReady.js*.

Fügen Sie den folgenden Code der *DatePickerReady.js* Datei:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Wenn Sie nicht mit jQuery vertraut sind, ist hier eine kurze Erläuterung der Funktionsweise dieser: die erste Zeile der &quot;jQuery bereit&quot; -Funktion, die aufgerufen wird, wenn die DOM-Elemente auf einer Seite geladen wurde. Die zweite Zeile wählt alle DOM-Elemente, die den Namen der Klasse `datefield`, ruft dann die `datepicker` Funktion für jede von ihnen. (Beachten Sie, dass Sie hinzugefügt haben die `datefield` -Klasse auf die *Views\Shared\EditorTemplates\Date.cshtml* Vorlage, die zuvor in diesem Tutorial.)

Öffnen Sie als Nächstes die *Views\Shared\\"_Layout.cshtml"* Datei. Sie müssen zum Hinzufügen von Verweisen auf die folgenden Dateien, die alle erforderlich sind, damit Sie die Datumsauswahl verwenden können:

- *Content/Themes/Base/jQuery.UI.Core.CSS*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/Themes/Base/jQuery.UI.Theme.CSS*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

Das folgende Beispiel zeigt dem tatsächlichen Code, die Sie hinzufügen sollten, am unteren Rand der `head` Element in der *Views\Shared\\"_Layout.cshtml"* Datei.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Die vollständige `head` Abschnitt wird hier gezeigt:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Die [Content URL-Hilfsprogramm](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) Methode konvertiert den Ressourcenpfad in einen absoluten Pfad. Verwenden Sie `@URL.Content` auf diese Ressourcen ordnungsgemäß verwiesen wird, wenn die Anwendung unter IIS ausgeführt wird.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie einen Link "Bearbeiten", und fügen Sie die Einfügemarke in die **ReleaseDate** Feld. JQuery UI-Popupkalenders wird angezeigt.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Wie die meisten jQuery-Steuerelemente Sie können "DatePicker" sie umfassend anpassen. Weitere Informationen finden Sie unter [visuellen Anpassung: Entwerfen eines jQuery UI-Designs](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) auf die [jQuery-Benutzeroberfläche](http://learn.jquery.com/jquery-ui/getting-started/) Standort.

### <a name="supporting-the-html5-date-input-control"></a>Das Datum-Steuerelement HTML5 unterstützt

Wie Browsern HTML5 unterstützt, sollten Sie eingeben, z. B. systemeigene HTML5 verwenden die `date` Eingabeelement und den jQuery UI-Kalender nicht verwenden. Sie können Logik hinzufügen, um die Anwendung automatisch HTML5-Steuerelemente verwendet, wenn der Browser unterstützt. Ersetzen Sie den Inhalt der zu diesem Zweck die *DatePickerReady.js* Datei durch Folgendes:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Die erste Zeile dieses Skript verwendet Modernizr, um sicherzustellen, dass die Datumseingabe HTML5 unterstützt wird. Wenn sie nicht unterstützt wird, ist der jQuery UI DatePicker stattdessen eingebunden. ([Modernizr](http://www.modernizr.com/docs/) ist eine Open-Source-JavaScript-Bibliothek, die die Verfügbarkeit systemeigener Implementierungen von HTML5 und CSS3 erkennt. Modernizr ist in alle neuen ASP.NET MVC-Projekte, die Sie erstellen enthalten.)

Nachdem Sie diese Änderung vorgenommen haben, können Sie es testen mithilfe eines Browsers, das HTML5, z. B. Opera 11 unterstützt. Führen Sie die Anwendung mit einem HTML5-kompatiblen Browser, und bearbeiten Sie einen Film-Eintrag. Das HTML5-Datum-Steuerelement wird anstelle der jQuery UI-Popupkalenders verwendet:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Da neue Versionen von Browsern HTML5 inkrementell implementiert werden, ist jetzt ein guter Ansatz, Hinzufügen von Code zu Ihrer Website, die eine Vielzahl von HTML5-Support aufnehmen kann. Beispielsweise eine stabilere *DatePickerReady.js* Skript finden Sie unten, mit dem Ihre Website unterstützen Browser, die das HTML5-Datum-Steuerelement nur teilweise zu unterstützen.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Dieses Skript wählt HTML5 `input` Elementen des Typs `date` , nicht das Datumssteuerelement HTML5 vollständig unterstützen. Für diese Elemente, die jQuery-UI-Popupkalenders verknüpft und ändert anschließend die `type` -Attribut vom `date` zu `text`. Durch Ändern der `type` -Attribut vom `date` zu `text`, lässt sich teilweise Unterstützung von HTML5-Datum. Sogar noch robuster *DatePickerReady.js* Skript finden Sie unter [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Hinzufügen von auf NULL festlegbare Daten an den Vorlagen

Wenn Sie eine der vorhandenen Datum Vorlagen verwenden, und übergeben ein null-Datum, erhalten Sie einen Laufzeitfehler. Um die Vorlagen Datum stabiler zu gestalten, ändern Sie sie an, um null-Werte behandelt. Um auf NULL festlegbare Daten zu unterstützen, ändern Sie den Code in die *Views\Shared\DisplayTemplates\DateTime.cshtml* folgt:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Der Code gibt eine leere Zeichenfolge zurück, wenn das Modell ist **null**.

Ändern Sie den Code in die *Views\Shared\EditorTemplates\Date.cshtml* -Datei in Folgendes:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Wenn dieser Code ausgeführt wird, wenn das Modell nicht null ist, des Modells `DateTime` Wert wird verwendet. Wenn das Paketmodell null ist, wird stattdessen das aktuelle Datum verwendet.

### <a name="wrapup"></a>Nachbereitung

In diesem Lernprogramm wurden die Grundlagen von Hilfsvorlagen von ASP.NET behandelt, und erfahren Sie, wie Sie mit der jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Anwendung. Probieren Sie diese Ressourcen für Weitere Informationen:

- Informationen dazu, Lokalisierung, finden Sie unter Rajeeshs Blog [JQueryUI Datepicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Weitere Informationen zu jQuery-Benutzeroberfläche, finden Sie unter [jQuery-Benutzeroberfläche](http://docs.jquery.com/UI).
- Weitere Informationen zum Lokalisieren von des Datepicker-Steuerelements, finden Sie unter [UI / "DatePicker" / Lokalisierung](http://docs.jquery.com/UI/Datepicker/Localization).
- Weitere Informationen zu den ASP.NET MVC-Vorlagen finden Sie in Brad Wilsons Blog-Reihe auf [ASP.NET MVC 2-Vorlagen](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Obwohl der Reihe für ASP.NET MVC 2 geschrieben wurde wurde, gilt immer noch das Material aus, für die aktuelle Version von ASP.NET MVC.

> [!div class="step-by-step"]
> [Vorherige](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
