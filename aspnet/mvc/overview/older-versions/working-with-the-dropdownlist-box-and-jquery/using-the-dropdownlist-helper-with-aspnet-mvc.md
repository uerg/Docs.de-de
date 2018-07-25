---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Verwenden des DropDownList-Hilfsprogramms mit ASP.NET MVC | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6dbffe715990de5c0b3b834e354379e414925816
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219055"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Verwenden des DropDownList-Hilfsprogramms mit ASP.NET MVC
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit der [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Helper und dem [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) Helper in einer ASP.NET MVC-Webanwendung. Sie können Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio auf dem Tutorial folgen. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:

- [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). In diesem Tutorial wird vorausgesetzt, Sie haben die [Einführung zu ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Tutorial oder[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) Tutorial oder Sie mit der ASP.NET MVC-Entwicklung vertraut sind. Dieses Tutorial beginnt mit einem geänderten-Projekt aus der [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) Tutorial. Sie können das Startprojekt mit den folgenden Link herunterladen [Laden Sie die C#-Version](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Ein Visual Web Developer-Projekt mit dem abgeschlossenen Tutorial C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Herunterladen](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Sie lernen Folgendes

Erstellen Sie Aktionsmethoden und Ansichten, mit denen die [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Hilfsprogramm, um eine Kategorie auszuwählen. Sie können auch **jQuery** ein Insert-Kategorie-Dialogfeld hinzufügen, die verwendet werden kann, wenn eine neue Kategorie (z. B. "Genre" "oder" Interpret ") erforderlich ist. Im folgenden finden Sie ein Screenshot der Ansicht "erstellen" mit Links einen neuen "Genre" und Hinzufügen eines neuen Künstlers.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Fähigkeiten, mit denen, die Sie lernen Folgendes

Hier ist Sie lernen Folgendes:

- Gewusst wie: Verwenden Sie die [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Helper Kategoriedaten auswählen.
- Gewusst wie: Hinzufügen einer **jQuery** Dialogfeld zum Hinzufügen von neuer Kategorien.

### <a name="getting-started"></a>Erste Schritte

Starten, indem Sie das Starter-Projekt mit den folgenden Link herunterladen [herunterladen](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). Im Windows-Explorer, klicken Sie mit der rechten Maustaste auf die *DDL\_Starter.zip* Datei, und wählen Sie Eigenschaften. In der **DDL\_Starter.zip Eigenschaften** Dialogfeld Option zulassen.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Klicken Sie mit der rechten Maustaste auf die DDL-Anweisungen\_Starter.zip-Datei, und wählen **alle extrahieren** auf die Datei zu entpacken. Öffnen der *StartMusicStore.sln* -Datei mit Visual Web Developer 2010 Express ("Visual Web Developer" oder "VWD" kurz) oder Visual Studio 2010.

Drücken Sie STRG + F5, um die Anwendung auszuführen, und klicken Sie auf die **Test** Link.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Wählen Sie die **Select Film Category (einfach)** Link. Eine Liste Film auswählen wird angezeigt, mit Komödie der ausgewählte Wert.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Klicken Sie mit der rechten Maustaste in den Browser, und wählen Anzeigen der Quelle. Der HTML-Code für die Seite wird angezeigt. Der folgende Code zeigt den HTML-Code für die select-Element.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Sie können sehen, dass jedes Element in der select-Liste einen Wert (0 für die Aktion, Drama 1, 2 für Komödie und 3 für Science-Fiction) und einen Anzeigenamen ein (Aktion, Drama, Komödie und Science-Fiction) verfügt. Der obige Code ist die standard-HTML-Code für eine select-Liste.

Ändern Sie die select-Liste in Drama ein, und drücken die **senden** Schaltfläche. Die URL im Browser ist `http://localhost:2468/Home/CategoryChosen?MovieType=1` und die Seite zeigt **Sie ausgewählt haben: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Öffnen der *Controllers\HomeController. cs* Datei, und sehen die `SelectCategory` Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Die [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) Hilfsprogramm zum Erstellen einer HTML-Auswahlliste erfordert eine **"IEnumerable"&lt;SelectListItem &gt;** , entweder explizit oder implizit. Also können Sie übergeben die **"IEnumerable"&lt;SelectListItem &gt;**  explizit auf die [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) -Hilfsprogramm oder Sie können Hinzufügen der **"IEnumerable"&lt; SelectListItem &gt;**  auf die ["ViewBag"](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) mit dem gleichen Namen für die **SelectListItem** wie die Modelleigenschaft. Übergeben der **SelectListItem** impliziten und expliziten wird im nächsten Teil des Tutorials behandelt. Der obige Code zeigt die einfachste Möglichkeit zum Erstellen einer **"IEnumerable"&lt;SelectListItem &gt;**  mit Text und die Werte aufgefüllt wird. Beachten Sie die `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) hat die [ausgewählte](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) -Eigenschaftensatz auf **"true";** Dies bewirkt, dass die gerenderte select-Liste angezeigt **Komödie** wie das ausgewählte Element in der Liste.

Die **"IEnumerable"&lt;SelectListItem &gt;**  erstellt oben wurde die ["ViewBag"](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) mit dem Namen MovieType. Wie wir übergeben den **"IEnumerable"&lt;SelectListItem &gt;**  implizit auf die [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) Hilfsprogramm unten.

Öffnen der *Views\Home\SelectCategory.cshtml* Datei, und überprüfen Sie das Markup.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Die dritte Zeile, wir legen Sie auf das Layout auf Views/Shared/\_einfache\_Layout.cshtml, die eine vereinfachte Version der Datei Standardlayout ist. Wir dies tun, um die Anzeige zu halten und gerenderten HTML-Code einfach.

In diesem Beispiel werden wir nicht den Zustand der Anwendung, ändern, damit wir die Daten mit übermittelt eine **HTTP GET**, nicht **HTTP POST**. Finden Sie im Abschnitt der W3C [kurze Checkliste für die Auswahl HTTP GET- oder POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Da wir nicht ändern der Anwendung und veröffentlichen die Form, verwenden wir die [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) Überladung, die an die Aktionsmethode, Controller und die Formular-Methode ermöglicht (**HTTP POST** oder **HTTP GET**). Ansichten in der Regel enthalten die [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) Überladung, die keine Parameter akzeptiert. Die keine Parameter-Version wird standardmäßig zum Senden von Daten aus dem Formular auf die POST-Version der gleiche Aktionsmethode und den Controller.

Die folgende Zeile

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

übergibt ein Argument, das **DropDownList** Helper. Diese Zeichenfolge, die "MovieType" in unserem Beispiel bewirkt zwei Dinge:

- Es stellt den Schlüssel für die **DropDownList** Hilfsmethode zum Suchen einer **"IEnumerable"&lt;SelectListItem &gt;**  in die **"ViewBag"**.
- Es ist, das Formularelement MovieType datengebunden. Ist die Sendemethode **HTTP GET**, `MovieType` werden eine Abfragezeichenfolge. Ist die Sendemethode **HTTP POST**, `MovieType` im Nachrichtentext hinzugefügt. Die folgende Abbildung zeigt die Abfragezeichenfolge mit dem Wert 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Der folgende code zeigt die `CategoryChosen` Methode, die das Formular, um übermittelt wurde.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Navigieren Sie zurück zu der Seite "Test", und wählen Sie die **HTML SelectList** Link. Die HTML-Seite rendert eine select-Element wie die einfache ASP.NET MVC-Testseite. Klicken Sie mit der rechten Maustaste auf das Browserfenster, und wählen Sie **Quelltext anzeigen**. Das HTML-Markup für die select-Liste ist im Wesentlichen identisch. Das HTML-Testseite, funktioniert wie die ASP.NET MVC-Aktionsmethode und die Ansicht, die wir zuvor getestet.

### <a name="improving-the-movie-select-list-with-enums"></a>Verbessern der Movie-Select-Liste mit den Enumerationen

Wenn die Kategorien in Ihrer Anwendung festgelegt sind und sich nicht ändert, können Sie von Enumerationen, machen Sie den Code robuster und einfacher zu erweitern, nutzen. Wenn Sie eine neue Kategorie hinzufügen, wird die richtige Kategoriewert generiert. Das Kopieren und Einfügen ein Fehler vermieden, wenn Sie eine neue Kategorie hinzufügen, aber vergessen, den Kategoriewert zu aktualisieren.

Öffnen der *Controllers\HomeController. cs* Datei, und überprüfen Sie den folgenden Code:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Die [Enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` erfasst die vier Movie-Typen. Die `SetViewBagMovieType` -Methode erstellt die **"IEnumerable"&lt;SelectListItem &gt;**  aus der `eMovieCategories` **Enum**, und legt fest der `Selected` Eigenschaft aus der `selectedMovie` Parameter. Die `SelectCategoryEnum` Aktionsmethode verwendet die gleiche Ansicht wie die `SelectCategory` Aktionsmethode.

Navigieren Sie zu der Seite "Test", und klicken Sie auf die `Select Movie Category (Enum)` Link. Dieses Mal wird anstelle eines Werts (Anzahl) angezeigt wird, eine Zeichenfolge, die die Enumeration darstellt.

### <a name="posting-enum-values"></a>Veröffentlichen von Enum-Werte

HTML-Formulare werden in der Regel verwendet, Daten an den Server zu veröffentlichen. Der folgende code zeigt die `HTTP GET` und `HTTP POST` Versionen der `SelectCategoryEnumPost` Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Durch Übergeben einer `eMovieCategories` Enumeration, die `POST` -Methode, extrahieren wir sowohl den Enumerationswert als auch die Enum-Zeichenfolge. Führen Sie das Beispiel aus, und navigieren Sie zu der Seite "Test". Klicken Sie auf die `Select Movie Category(Enum Post)` Link. Wählen Sie einen Film, und drücken Sie dann auf die Schaltfläche "Senden". Die Anzeige zeigt den Wert und den Namen des Typs Film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Erstellen eine mehrere Select Section-Element

Der [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML-Hilfsobjekt rendert die HTML-Code `<select>` -Element mit der `multiple` -Attribut, das der Benutzer mehrere Optionen auswählen kann. Navigieren Sie zu dem Link "testen", und wählen Sie dann die **mit mehreren wählen Land** Link. Die gerenderte Benutzeroberfläche ermöglicht Ihnen die Auswahl von mehreren Ländern. In der folgenden Abbildung sind Canada und China ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Untersuchen des Codes MultiSelectCountry

Überprüfen Sie den folgenden Code aus der *Controllers\HomeController. cs* Datei.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Die `GetCountries` Methode erstellt eine Liste der Länder und anschließend übergibt es an der `MultiSelectList` Konstruktor. Die `MultiSelectList` Konstruktorüberladung, die in verwendet die `GetCountries` oben genannte Methode benötigt vier Parameter:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *Elemente*: ein ["IEnumerable"](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) mit den Elementen in der Liste. Im obigen Beispiel ist die Liste der Länder.
2. *DataValueField*: der Name der Eigenschaft in der **"IEnumerable"** Liste, die den Wert enthält. Im obigen Beispiel ist die `ID` Eigenschaft.
3. *DataTextField*: der Name der Eigenschaft in der **"IEnumerable"** Liste, die die Informationen für die Anzeige enthält. Im obigen Beispiel ist die `name` Eigenschaft.
4. *SelectedValues*: die Liste der ausgewählten Werte.

Im obigen Beispiel ist die `MultiSelectCountry` -Methode übergibt ein `null` Wert für den ausgewählten Ländern, damit keine Länder ausgewählt ist, wenn die Benutzeroberfläche angezeigt wird. Der folgende Code zeigt das Razor-Markup zum Rendern verwendet die `MultiSelectCountry` anzeigen.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Das HTML-Hilfsobjekt [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) Methode verwendet, über nehmen zwei Parameter: den Namen der Eigenschaft an das Modell gebunden und die [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) , die die Optionen und Werte enthält. Die `ViewBag.YouSelected` obigen Code wird verwendet, um die Werte der Länder und Regionen anzuzeigen, Sie beim Senden des Formulars ausgewählt haben. Überprüfen Sie die HTTP POST-Überladung von der `MultiSelectCountry` Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Die `ViewBag.YouSelected` dynamische Eigenschaft enthält den ausgewählten Ländern/Regionen, erhalten für die `Countries` Eintrag in der formularauflistung. In dieser Version wird die GetCountries-Methode übergeben, eine Liste mit den ausgewählten Ländern/Regionen, also bei der `MultiSelectCountry` angezeigt wird, die den ausgewählten Ländern/Regionen werden in der Benutzeroberfläche ausgewählt.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Machen eine auswählen-Element angezeigten mit dem jQuery-Plug-in-Harvest-ausgewählt

Die Harvest [ausgewählte](http://harvesthq.github.com/chosen/) jQuery-Plug-in kann hinzugefügt werden, um ein HTML &lt;wählen&gt; Element zum Erstellen eines Benutzers benutzerfreundliche Benutzeroberfläche. Die Bilder unten veranschaulichen die Harvest [ausgewählte](http://harvesthq.github.com/chosen/) jQuery-Plug-in mit `MultiSelectCountry` anzeigen.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

In den zwei folgenden Abbildungen **Kanada** ausgewählt ist.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

In der Abbildung oben Kanada ausgewählt ist, und er enthält eine **x** klicken Sie auf, um die Auswahl zu entfernen. Die folgende Abbildung zeigt, Kanada, China und Japan ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Einbinden der Harvest ausgewählte jQuery-Plug-in

Im folgende Abschnitt ist leichter zu verstehen, wenn Sie über Erfahrung mit jQuery verfügen. Wenn Sie jQuery vor noch nie verwendet haben, empfiehlt es sich um die folgenden jQuery-Tutorials versuchen Sie es.

- [Funktionsweise von jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) von [John Resig](http://ejohn.org/)
- [Erste Schritte mit jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) von [Jörn Zaefferer](http://bassistance.de/)
- [Beispiele für jQuery Live](http://codylindley.com/blogstuff/js/jquery/#) von [Cody Lindley](http://codylindley.com/)

Das ausgewählte Plug-in ist enthalten, in der Starter und das vollständige Beispiel-Projekte, die in diesem Lernprogramm begleitet. Für dieses Tutorial müssen Sie nur jQuery verwenden, um sie auf der Benutzeroberfläche zu verknüpfen. Um die Harvest ausgewählte jQuery-Plug-in in einem ASP.NET MVC-Projekt verwenden möchten, müssen Sie folgende Aktionen ausführen:

1. Ausgewählte-Plug-In herunterladen [Github](https://github.com/harvesthq/chosen/). Dieser Schritt ist für Sie ausgeführt wurde.
2. Fügen Sie den ausgewählten Ordner dem ASP.NET MVC-Projekt hinzu. Fügen Sie Ressourcen, aus der ausgewählten-Plug-Ins, die Sie im vorherigen Schritt in den ausgewählten Ordner heruntergeladen. Dieser Schritt ist für Sie ausgeführt wurde.
3. Das ausgewählte Plug-in zum Einbinden der **DropDownList** oder **ListBox** HTML-Hilfsobjekt.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Das ausgewählte Plug-in zur Ansicht MultiSelectCountry verknüpft wird.

Öffnen der *Views\Home\MultiSelectCountry.cshtml* -Datei und fügen eine `htmlAttributes` Parameter, um die `Html.ListBox`. Sie fügen der-Parameter enthält einen Klassennamen für die select-Liste (`@class = "chzn-select"`). Der vollständige Code wird unten gezeigt:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Es wird im obigen Code hinzugefügt, die HTML-Attribut und Attributwert `class = "chzn-select"`. Die \@ vorherigen-Klasse verfügt über keinerlei Bezug zur Razor-ansichtsengine Zeichen. `class` ist eine [c#-Schlüsselwort](https://msdn.microsoft.com/library/x53a06bb.aspx). C#-Schlüsselwörter nicht als Bezeichner verwendet werden, es sei denn, sie enthalten \@ als Präfix. Im obigen Beispiel `@class` ist ein gültiger Bezeichner jedoch **Klasse** ist nicht, da **Klasse** ist ein Schlüsselwort.

Fügen Sie Verweise auf die *Chosen/chosen.jquery.js* und *Chosen/chosen.css* Dateien. Die *Chosen/chosen.jquery.js* und implementiert die Funktionalität des Plug-Ins ausgewählt. Die *Chosen/chosen.css* Datei enthält die Stile. Fügen Sie diese Verweise auf das Ende der *Views\Home\MultiSelectCountry.cshtml* Datei. Der folgende Code zeigt, wie Sie auf das ausgewählte Plug-in zu verweisen.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktivieren Sie das ausgewählte Plug-in mit dem Klassennamen in verwendet die **Html.ListBox** Code. Im obigen Beispiel ist der Klassenname `chzn-select`. Fügen Sie die folgende Zeile am unteren Rand der *Views\Home\MultiSelectCountry.cshtml* Ansichtsdatei. Diese Zeile wird das ausgewählte Plug-in aktiviert.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Die folgende Zeile ist die Syntax zum Aufrufen von der jQuery-bereitschaftsfunktion, wählt der DOM-Element mit dem Klassennamen `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Den umschlossenen festgelegt von der obige Aufruf zurückgegeben wird, gilt die gewählte Methode (`.chosen();`), die das ausgewählte Plug-in verknüpft.

Der folgende Code zeigt die abgeschlossene *Views\Home\MultiSelectCountry.cshtml* Ansichtsdatei.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu der `MultiSelectCountry` anzeigen. Versuchen Sie es hinzufügen und Löschen von Ländern. Das bereitgestellte Beispiel zum Herunterladen enthält auch eine `MultiCountryVM` -Methode und die Ansicht, die die MultiSelectCountry-Funktionalität, die mit einer Ansicht implementiert zu modellieren, anstelle von einer **"ViewBag"**.

Im nächsten Abschnitt sehen Sie die Funktionsweise der ASP.NET MVC-Gerüstbau mit der **DropDownList** Helper.

> [!div class="step-by-step"]
> [Nächste](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
