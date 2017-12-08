---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 4 | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2b76d2e449d491fd8d808343065b22ba267f1152
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 4
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


### <a name="adding-a-template-for-editing-dates"></a>Hinzufügen einer Vorlage für die Bearbeitung von Datumsangaben

In diesem Abschnitt erstellen Sie eine Vorlage für die Bearbeitung von Datumsangaben, die angewendet werden, wenn ASP.NET MVC zeigt die Benutzeroberfläche für die Bearbeitung von Modelleigenschaften, die mit markiert sind die **Datum** Enumeration von der [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) Standardtitel. Die Vorlage wird nur das Datum rendern. Zeit wird nicht angezeigt. In der Vorlage verwenden Sie die [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) Popupkalenders, bieten eine Möglichkeit, Daten zu bearbeiten.

Zunächst öffnen Sie die *Movie.cs* und fügen die [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut mit der **Datum** -Enumeration der `ReleaseDate` Eigenschaft, wie im folgenden Code gezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Dieser Code bewirkt, dass die `ReleaseDate` Feld angezeigt werden, ohne die Zeit in beiden Vorlagen anzeigen und Bearbeiten von Vorlagen. Wenn Ihre Anwendung enthält eine *date.cshtml* Vorlage in der *Views\Shared\EditorTemplates* Ordner oder in der *Views\Movies\EditorTemplates* Ordner, die Vorlage wird verwendet, um alle Rendern `DateTime` Eigenschaft beim Bearbeiten. Andernfalls wird das integrierte ASP.NET Datenvorlagen System die Eigenschaft als Datum angezeigt.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie einen Bearbeitungslink, um sicherzustellen, dass das Eingabefeld für den freigabetermin nur das Datum angezeigt wird.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

In **Projektmappen-Explorer**, erweitern Sie die *Ansichten* Ordner, erweitern Sie die *Shared* Ordner ein, und klicken Sie dann mit der rechten Maustaste die *Views\Shared\EditorTemplates* Ordner.

Klicken Sie auf **hinzufügen**, und klicken Sie dann auf **Ansicht**. Die **Ansicht hinzufügen** Dialogfeld wird angezeigt.

In der **Sichtname** geben &quot;Datum&quot;.

Wählen Sie die **als eine Teilansicht erstellen** Kontrollkästchen. Stellen Sie sicher, dass die **mithilfe eine Seite Layout- oder Masterseite** und **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert sind.

Klicken Sie auf **Hinzufügen**. Die *Views\Shared\EditorTemplates\Date.cshtml* Vorlage erstellt wurde.

Fügen Sie folgenden Code, der *Views\Shared\EditorTemplates\Date.cshtml* Vorlage.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

Die erste Zeile deklariert das Modell soll eine `DateTime` Typ. Obwohl Sie müssen nicht den Typ des Modells in Bearbeitung zu deklarieren und Vorlagen anzuzeigen, ist es eine bewährte Methode, sodass Sie während der Kompilierung erhalten Überprüfen des Modells an die Ansicht übergeben wird. (Ein weiterer Vorteil ist, dass Sie anschließend IntelliSense für das Modell in Visual Studio in der Ansicht erhalten.) Wenn der Typ des Modells nicht deklariert ist, betrachtet er über den ASP.NET MVC eine [dynamische](https://msdn.microsoft.com/en-us/library/dd264741.aspx) geben und es gibt keine Kompilierung Überprüfung des Typs. Wenn Sie das Modell zu deklarieren einer `DateTime` Typ, es wird stark typisiert.

Die zweite Zeile wird nur literalen zeigt HTML-Markup &quot;Datumsvorlage mithilfe&quot; bevor Sie ein Datumsfeld. Sie müssen diese Zeile vorübergehend verwenden, um sicherzustellen, dass diese Datumsvorlage verwendet wird.

Die nächste Zeile ist ein ["HTML.TextBox"](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.inputextensions.textbox.aspx) -Hilfsprogramm, das rendert ein `input` Feld, das ein Textfeld ist. Der dritte Parameter für das Hilfsprogramm verwendet einen anonymen Typ, um die Klasse für das Textfeld festzulegen `datefield` und den Typ auf `date`. (Da `class` ist eine reservierte in c# müssen Sie verwenden die `@` escape-Zeichen der `class` -Attribut in der C#-Parser.)

Die `date` Typ ist ein Eingabe-HTML5-Typ, der zum Rendern eines Kalendersteuerelements HTML5 HTML5-fähigen Browser ermöglicht. Später fügen Sie JavaScript die jQuery-Datepicker zum Verknüpfen der `Html.TextBox` Element mit dem `datefield` Klasse.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Können Sie sicherstellen, dass die `ReleaseDate` Eigenschaft in der Bearbeitungsansicht verwendet die Bearbeitungsvorlage, da die Vorlage angezeigt &quot;Datumsvorlage mithilfe&quot; kurz vor dem Ausführen der `ReleaseDate` Text-Eingabefeld, wie in dieser Abbildung dargestellt:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Zeigen Sie in Ihrem Browser die Quelle der Seite ein. (Z. B. mit der rechten Maustaste in der Seite, und wählen Sie **Quelltext anzeigen**.) Im folgenden Beispiel zeigt einige der das Markup für die Seite zur Veranschaulichung der `class` und `type` gerenderten HTML-Attribute.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Zurück zu den *Views\Shared\EditorTemplates\Date.cshtml* Vorlage, und entfernen Sie die &quot;Datumsvorlage mithilfe&quot; Markup. Jetzt sieht die abgeschlossene Vorlage:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Hinzufügen einer jQuery UI Datepicker-Popupkalenders mit NuGet

In diesem Abschnitt fügen Sie der [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) Popupkalenders zur Vorlage Datum bearbeiten. Die [jQuery UI](http://jqueryui.com/) Bibliothek bietet Unterstützung für erweiterte Auswirkungen und anpassbare Widgets Animation. Es baut die jQuery JavaScript-Bibliothek. Datepicker-Popupkalenders vereinfacht das einfache und natürliche Eingabe von Datumsangaben, die unter Verwendung eines anderen nicht durch Eingabe eine Zeichenfolge. Die Popupkalenders beschränkt außerdem Benutzern rechtliche Datumsangaben – Textfeldzellen-Eintrag für ein Datum würde etwa Eingabe ermöglichen `2/33/1999` (Februar 33rd, 1999), aber die [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) Popupkalenders gestattet, die nicht.

Zunächst müssen Sie die jQuery UI-Bibliotheken installieren. Zu diesem Zweck verwenden Sie NuGet, also eine Paket-Manager, die in SP1-Versionen von Visual Studio 2010 und Visual Web Developer enthalten ist.

In Visual Web Developer aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager** und wählen Sie dann **NuGet-Pakete verwalten**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Hinweis: Wenn die **Tools** Menü nicht angezeigt. die **Bibliothekspaket-Manager** Befehl müssen Sie die Installation von NuGet mithilfe der folgenden Anweisungen auf der [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) auf der Seite die NuGet-Website.   
  
Bei Verwendung von Visual Studio anstelle von Visual Web Developer aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager** und wählen Sie dann **Bibliothekspaketverweis hinzufügen**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

In der **MVCMovie - NuGet-Pakete verwalten** (Dialogfeld), klicken Sie auf die **Online** links auf die Registerkarte, und geben Sie dann &quot;jQuery.UI&quot; in das Suchfeld. Wählen Sie j **Abfrage UI WIdgets: Datepicker**, und wählen Sie dann die **installieren** Schaltfläche.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet werden dem Projekt diese Debugversionen und verkleinerte Versionen von jQuery UI Core und dem jQuery UI DatePicker hinzugefügt:

- *jQuery.UI.Core.js*
- *jQuery.UI.Core.Min.js*
- *jQuery.UI.DatePicker.js*
- *jQuery.UI.DatePicker.Min.js*

Hinweis: Die Debugversionen (die Dateien ohne die *. min.js* Erweiterung) eignen sich für das Debuggen, aber in einer Produktionswebsite, würden Sie nur die verkleinerte Versionen einschließen.

Um die jQuery-Datumsauswahl tatsächlich verwenden zu können, müssen Sie ein jQuery-Skript zu erstellen, die das Widget "Kalender" der Vorlage bearbeiten verknüpft wird. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Skripts* Ordner, und wählen **hinzufügen**, klicken Sie dann **neues Element**, und klicken Sie dann **JScript Datei**. Nennen Sie die Datei *DatePickerReady.js*.

Fügen Sie folgenden Code, der *DatePickerReady.js* Datei:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Wenn Sie nicht mit jQuery vertraut sind, wird hier eine kurze Erläuterung der Funktionsweise dieser: die erste Zeile ist die &quot;jQuery bereit&quot; -Funktion, die aufgerufen wird, wenn die DOM-Elemente auf einer Seite geladen haben. Die zweite Zeile wählt alle DOM-Elemente, deren Name der Klasse `datefield`, ruft dann die `datepicker` Funktion für jede von ihnen. (Beachten Sie, dass Sie hinzugefügt haben die `datefield` Klasse, um die *Views\Shared\EditorTemplates\Date.cshtml* Vorlage zuvor im Lernprogramm.)

Öffnen Sie als Nächstes die *Views\Shared\\_Layout.cshtml* Datei. Sie müssen Verweise auf die folgenden Dateien hinzufügen, die alle benötigt werden, damit Sie die Datumsauswahl verwenden können:

- *Content/Themes/Base/jQuery.UI.Core.CSS*
- *Content/Themes/Base/jQuery.UI.DatePicker.CSS*
- *Content/Themes/Base/jQuery.UI.Theme.CSS*
- *jQuery.UI.Core.Min.js*
- *jQuery.UI.DatePicker.Min.js*
- *DatePickerReady.js*

Das folgende Beispiel zeigt dem tatsächlichen Code, dass Sie am Ende hinzufügen, sollten die `head` Element in der *Views\Shared\\_Layout.cshtml* Datei.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Die vollständige `head` Abschnitt ist im folgenden dargestellt:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Die [Content URL-Hilfsprogramm](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.content.aspx) -Methode konvertiert den Ressourcenpfad ein absoluter Pfad. Verwenden Sie `@URL.Content` auf diese Ressourcen ordnungsgemäß zu verweisen, wenn die Anwendung auf IIS ausgeführt wird.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie einen Link, und platzieren Sie die Einfügemarke in die **ReleaseDate** Feld. Der jQuery UI-Popupkalenders wird angezeigt.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Wie bei den meisten jQuery-Steuerelementen können mit der Datepicker umfassend anpassen. Informationen finden Sie unter [visuelle Anpassung: Entwerfen eines jQuery UI-Designs](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) auf die [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) Standort.

### <a name="supporting-the-html5-date-input-control"></a>Unterstützung der HTML5-Datum-Eingabesteuerelement

Weitere Browser HTML5 unterstützen, Sie müssen die Eingabe, z. B. native HTML5 nutzen möchten die `date` Eingabeelement und den jQuery UI-Kalender nicht verwenden. Sie können Logik hinzufügen, um die Anwendung automatisch HTML5-Steuerelemente verwendet, wenn der Browser unterstützt. Ersetzen Sie den Inhalt der zu diesem Zweck die *DatePickerReady.js* Datei durch Folgendes:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Die erste Zeile dieses Skripts verwendet Modernizr um sicherzustellen, dass die HTML5-Datumseingabe unterstützt wird. Wenn sie nicht unterstützt wird, wird stattdessen der jQuery UI DatePicker eingebunden. ([Modernizr](http://www.modernizr.com/docs/) ist eine Open Source-JavaScript-Bibliothek, die die Verfügbarkeit von systemeigenen Implementierungen von HTML5- und CSS3 erkennt. Modernizr ist in jeder neuen ASP.NET MVC-Projekte, die Sie erstellen enthalten.)

Nachdem Sie diese Änderung vorgenommen haben, können Sie es testen mit einem Browser, der HTML5, z. B. Opera 11 unterstützt. Führen Sie die Anwendung mit einem HTML5-kompatiblen Browser, und bearbeiten Sie einen Film-Eintrag. Das Datumssteuerelement HTML5 wird anstelle der jQuery UI-Popupkalenders verwendet:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Da neue Versionen der Browser HTML5 inkrementell implementieren, wird ein guter Ansatz für jetzt Code zu Ihrer Website hinzugefügt, der auf eine Vielzahl von Unterstützung für HTML5 zutrifft. Z. B. eine robustere *DatePickerReady.js* Skript unten werden angezeigt, mit dem Ihre Website-Support-Browsern mit Unterstützung für HTML5-Datumssteuerelement nur teilweise.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Dieses Skript wählt HTML5 `input` Elemente des Typs `date` , nicht vollständig unterstützen das HTML5-Steuerelement. Für diese Elemente der jQuery UI-Popupkalenders verknüpft und ändert anschließend die `type` -Attribut aus `date` auf `text`. Durch Ändern der `type` -Attribut aus `date` auf `text`, teilunterstützung für HTML5-Datum entfernt ist. Ein selbst eine robustere *DatePickerReady.js* Skript finden Sie unter [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>NULL-Werte zu Datumsangaben hinzufügen an den Vorlagen

Wenn Sie verwenden Sie eine vorhandene Vorlagen für Datum und eine null-Datum übergeben, erhalten Sie einen Laufzeitfehler. Um die Vorlagen Datum stabiler zu gestalten, ändern Sie sie an, um null-Werte behandelt. Unterstützung von Datumsangaben NULL-Werte zulässt, ändern Sie den Code in der *Views\Shared\DisplayTemplates\DateTime.cshtml* , der dem folgenden:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Der Code gibt eine leere Zeichenfolge zurück, wenn das Modell ist **null**.

Ändern Sie den Code in der *Views\Shared\EditorTemplates\Date.cshtml* zu folgender Datei:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Wenn dieser Code ausgeführt wird, wenn das Modell nicht null ist, ist des Modells `DateTime` Wert wird verwendet. Wenn das Modell auf null festgelegt ist, wird stattdessen das aktuelle Datum verwendet.

### <a name="wrapup"></a>Nachbereitung

Dieses Lernprogramm wurde, behandelt die Grundlagen von Hilfsvorlagen von ASP.NET und zeigt, wie mithilfe der jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Anwendung. Versuchen Sie diese Ressourcen für Weitere Informationen:

- Informationen, Lokalisierung, finden Sie unter Rajeeshs Blog [JQueryUI Datepicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Informationen zu jQuery UI finden Sie unter [jQuery UI](http://docs.jquery.com/UI).
- Weitere Informationen zum Lokalisieren von Datepicker-Steuerelement finden Sie unter [UI/Datepicker/Lokalisierung](http://docs.jquery.com/UI/Datepicker/Localization).
- Weitere Informationen zu ASP.NET MVC-Vorlagen, finden Sie in Brad Wilsons blogreihe auf [ASP.NET MVC 2-Vorlagen](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Obwohl die Reihe für ASP.NET MVC 2 geschrieben wurde, gilt das Material weiterhin für die aktuelle Version von ASP.NET MVC.

>[!div class="step-by-step"]
[Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
