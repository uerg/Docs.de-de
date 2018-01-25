---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Verwenden der DropDownList-Hilfsmethode mit ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 278d04aec68e93f3ebfd12d06a96b59f3bcbef4b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Verwenden der DropDownList-Hilfsmethode mit ASP.NET MVC
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit der [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Helper und [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) Helper in einer ASP.NET MVC-Webanwendung. Sie können Microsoft Visual Web Developer 2010 Express Service Pack 1, eine kostenlose Version von Microsoft Visual Studio ist, um das Lernprogramm ausführen. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:

- [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Diesem Lernprogramm wird vorausgesetzt, Sie haben die [Einführung in ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Lernprogramm oder[ASP.NET MVC-Music Store](../mvc-music-store/mvc-music-store-part-1.md) Lernprogramm oder Sie bei der Entwicklung mit ASP.NET MVC vertraut sind. In diesem Lernprogramm beginnt mit einem geänderten Projekt aus der [ASP.NET MVC-Music Store](../mvc-music-store/mvc-music-store-part-1.md) Lernprogramm. Sie können das Startprojekt mit dem folgenden Link herunterladen [die C#-Version herunterladen](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Ein Visual Web Developer-Projekt, das im abgeschlossenen Lernprogramm C#-Quellcode ist zu diesem Thema steht zur Verfügung. [Herunterladen](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Was müssen Sie erstellen

Erstellen Sie Aktionsmethoden und Ansichten, mit denen die [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Hilfsprogramm, um eine Kategorie auszuwählen. Verwenden Sie ebenfalls **jQuery** eine Insert-Kategorie-Dialogfeld hinzufügen, die verwendet werden kann, wenn eine neue Kategorie (z. B. "Genre" oder Künstlers) benötigt wird. Nachstehend finden Sie ein Screenshot der Ansicht für das Erstellen, die Sie mit einer neuen "Genre" und hinzufügen ein neues Interpreten Links.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erfahren

Hier ist Sie lernen:

- Gewusst wie: Verwenden der [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) Helper Kategoriedaten auswählen.
- Gewusst wie: Hinzufügen einer **jQuery** Dialogfeld, um neue Kategorien hinzuzufügen.

### <a name="getting-started"></a>Erste Schritte

Starten Sie das Startprojekt mit dem folgenden Link herunterladen [herunterladen](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). In Windows Explorer, klicken Sie mit der rechten Maustaste auf die *DDL\_Starter.zip* Datei, und wählen Sie Eigenschaften aus. In der **DDL\_Starter.zip Eigenschaften** (Dialogfeld), wählen Sie zum Aufheben der Sperre.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Klicken Sie mit der mit der rechten Maustaste auf die DDL-Anweisungen\_Starter.zip-Datei, und wählen **alle extrahieren** um die Datei zu entpacken. Öffnen der *StartMusicStore.sln* Datei mit Visual Web Developer 2010 Express ("Visual Web Developer" oder "VWD" für kurze) oder Visual Studio 2010.

Drücken Sie STRG + F5, um die Anwendung auszuführen, und klicken Sie auf die **Test** Link.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Wählen Sie die **Film Kategorie auswählen ' (einfach)** Link. Eine Liste Film Typ auswählen, wird mit Comedy der ausgewählte Wert angezeigt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Klicken Sie mit der rechten Maustaste im Browser und Quelltext anzeigen auswählen. Der HTML-Code für die Seite wird angezeigt. Der folgende Code zeigt die HTML-Code für die select-Element.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Sie können sehen, dass jedes Element in der select-Liste einen Wert (0 für die Aktion, für Filmen 1, 2 für Comedy und 3 für Science-Fiction) und einen Anzeigenamen (Aktion, Filmen Comedy und Science-Fiction) aufweist. Der obige Code ist die standard-HTML-Code für eine select-Liste.

Ändern Sie die select-Liste in Filmen, und drücken die **Absenden** Schaltfläche. Die URL im Browser ist `http://localhost:2468/Home/CategoryChosen?MovieType=1` und die Seite zeigt **Sie ausgewählt haben: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Öffnen der *Controllers\HomeController. cs* Datei, und überprüfen Sie die `SelectCategory` Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Die [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) Hilfsprogramm verwendet zum Erstellen einer HTML-Auswahlliste erfordert eine **IEnumerable&lt;SelectListItem &gt;** , entweder explizit oder implizit. Sie können also übergeben der **IEnumerable&lt;SelectListItem &gt;**  explizit auf die [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) Helper oder Sie können Hinzufügen der **IEnumerable&lt; SelectListItem &gt;**  auf die [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) mit dem gleichen Namen für die **SelectListItem** wie die Modelleigenschaft. Übergibt die **SelectListItem** implizit oder explizit im nächsten Teil des Lernprogramms abgedeckt ist. Der obige Code zeigt die einfachste Möglichkeit zum Erstellen einer **IEnumerable&lt;SelectListItem &gt;**  und füllen sie mit Text und Werte. Hinweis Die `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) hat die [ausgewählte](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) -Eigenschaftensatz auf **"true";** Dies bewirkt, dass die gerenderte select-Liste anzuzeigende **Comedy** als das ausgewählte Element in der Liste.

Die **IEnumerable&lt;SelectListItem &gt;**  erstellt höher hinzugefügt wird, um die [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) mit dem Namen MovieType. Sieht wie wir übergeben der **IEnumerable&lt;SelectListItem &gt;**  implizit in den [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) Hilfsprogramm unten angezeigt.

Öffnen der *Views\Home\SelectCategory.cshtml* Datei, und überprüfen Sie das Markup.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

In der dritten Zeile wird das Layout auf Ansichten/freigegeben festgelegt/\_einfache\_Layout.cshtml, die eine vereinfachte Version der Datei Standardlayout ist. Wir führen Sie diese Option, um die Anzeige beibehalten und HTML einfache gerendert.

In diesem Beispiel werden wir nicht den Zustand der Anwendung ändern, damit wir die Daten mit übermitteln, wird ein **HTTP GET**, nicht **HTTP POST**. Finden Sie im Abschnitt W3C [kurze Checkliste für die Auswahl HTTP GET- oder POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Da wir nicht ändern die Anwendung und das Formular, verwenden wir die [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) Überladung, die an die Aktionsmethode, Controller und Formular ermöglicht (**HTTP POST** oder **HTTP-GET**). Ansichten in der Regel enthalten die [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) Überladung, die keine Parameter akzeptiert. Wird für die keine Parameter-Version standardmäßig Posten von Beiträgen der Formulardaten in die POST-Version der gleichen Aktionsmethode und den Controller.

Die folgende Zeile

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

übergibt ein Argument an die **DropDownList** Helper. Dieser Zeichenfolge übereinstimmt, "MovieType" in unserem Beispiel führt zwei Aufgaben aus:

- Es stellt den Schlüssel für die **DropDownList** Hilfsmethode zum Suchen einer **IEnumerable&lt;SelectListItem &gt;**  in der **ViewBag**.
- Es wird zum Form-Elements MovieType datengebundenen. Wenn die Sendemethode ist **HTTP GET**, `MovieType` werden eine Abfragezeichenfolge. Wenn die Sendemethode ist **HTTP POST**, `MovieType` wird in den Nachrichtentext hinzugefügt. Die folgende Abbildung zeigt die Abfragezeichenfolge mit dem Wert 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Der folgende code zeigt die `CategoryChosen` Methode, die das Formular zu gesendet wurde.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Navigieren Sie zurück zu der Seite "Test", und wählen Sie die **HTML SelectList** Link. Die HTML-Seite rendert ein select-Element der einfachen ASP.NET MVC-Testseite ähnelt. Klicken Sie mit der mit der rechten Maustaste auf das Browserfenster, und wählen Sie **Quelltext anzeigen**. Das HTML-Markup für die select-Liste ist im Wesentlichen identisch. Der HTML-Testseite, funktioniert es wie ASP.NET MVC-Aktionsmethode und Anzeigen von zuvor von uns getesteten.

### <a name="improving-the-movie-select-list-with-enums"></a>Verbessern der Film-Select-Liste bei Enumerationen

Wenn die Kategorien in der Anwendung fest sind und sich nicht ändert, können Sie Enumerationen, damit Ihr Code robuster und einfacher zu erweitern, nutzen. Wenn Sie eine neue Kategorie hinzufügen, wird die richtige Kategoriewert generiert. Das Kopieren und Einfügen ein Fehler wird vermieden, wenn Sie eine neue Kategorie hinzufügen, aber vergessen, den Kategoriewert zu aktualisieren.

Öffnen der *Controllers\HomeController. cs* -Datei und überprüfen Sie den folgenden Code:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Die [Enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` erfasst die vier Film-Typen. Der `SetViewBagMovieType` Methode erstellt der **IEnumerable&lt;SelectListItem &gt;**  aus der `eMovieCategories` **Enum**, und legt der `Selected` Eigenschaft aus der `selectedMovie` Parameter. Die `SelectCategoryEnum` Aktionsmethode verwendet die gleiche Ansicht als das `SelectCategory` Aktionsmethode.

Navigieren Sie zu der Seite "Test", und klicken Sie auf die `Select Movie Category (Enum)` Link. Dieses Mal wird statt eines Werts (Anzahl) angezeigt wird, eine Zeichenfolge, die die Enumeration darstellt, angezeigt.

### <a name="posting-enum-values"></a>Posten von Beiträgen Enumerationswerte

HTML-Formulare werden in der Regel verwendet, Daten an den Server gesendet. Der folgende code zeigt die `HTTP GET` und `HTTP POST` -Versionen der `SelectCategoryEnumPost` Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Durch Übergeben einer `eMovieCategories` Enumeration der `POST` -Methode, können wir die Enum-Wert und die Enum-Zeichenfolge extrahieren. Führen Sie das Beispiel, und navigieren Sie zu der Seite "Test". Klicken Sie auf die `Select Movie Category(Enum Post)` Link. Wählen Sie einen Film, und drücken Sie dann die Schaltfläche "Absenden". Der Wert und den Namen des Typs Film, wird angezeigt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Erstellen eine mehrere Select Section-Element

Die [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML-Hilfsobjekt rendert die HTML `<select>` Element mit dem `multiple` -Attribut, das der Benutzer eine Mehrfachauswahl treffen kann. Navigieren Sie zu dem Link testen, und wählen Sie dann die **mehrfach wählen Land** Link. Die gerenderte Benutzeroberfläche können Sie mehrere Länder auswählen. In der folgenden Abbildung werden die Elemente Canada und China ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Untersuchen den Code MultiSelectCountry

Überprüfen Sie den folgenden Code aus der *Controllers\HomeController. cs* Datei.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Die `GetCountries` Methode erstellt eine Liste von Ländern, und übergibt es an die `MultiSelectList` Konstruktor. Die `MultiSelectList` Konstruktorüberladung, die verwendet wird, der `GetCountries` oben genannten Methode nimmt vier Parameter:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *Elemente*: ein [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) mit den Elementen in der Liste. Im Beispiel oben, die Liste von Ländern.
2. *DataValueField*: der Name der Eigenschaft in der **IEnumerable** Liste, die den Wert enthält. Im Beispiel oben die `ID` Eigenschaft.
3. *DataTextField*: der Name der Eigenschaft in der **IEnumerable** Liste, die zur Darstellung der Informationen enthält. Im Beispiel oben die `name` Eigenschaft.
4. *SelectedValues*: die Liste der ausgewählten Werte.

Im Beispiel oben die `MultiSelectCountry` -Methode übergibt eine `null` Wert für den ausgewählten Ländern, damit keine Ländern ausgewählt ist, wenn die Benutzeroberfläche angezeigt wird. Der folgende Code zeigt das Razor-Markup zum Rendern verwendet die `MultiSelectCountry` anzeigen.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Das HTML-Hilfsobjekt [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) oben akzeptieren zwei Parameter, den Namen der Eigenschaft zu bindende Modell verwendeten Methode und die [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) mit den Optionen und Werte. Die `ViewBag.YouSelected` oben angegeben Codes wird verwendet, um die Werte der Länder und Regionen anzuzeigen, Sie bei der Übermittlung des Formulars ausgewählt. Überprüfen Sie die HTTP POST-Überladung, der die `MultiSelectCountry` Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Die `ViewBag.YouSelected` dynamische Eigenschaft enthält die ausgewählten Länder, Sperrung für die `Countries` Eintrag in der formularauflistung. In dieser Version wird die GetCountries-Methode übergeben, eine Liste der Länder und Regionen ausgewählten daher die `MultiSelectCountry` angezeigt wird, in der Benutzeroberfläche ausgewählten Länder und Regionen ausgewählt sind.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Herstellen einer wählen Element angezeigten mit der ausgewählten Ernte jQuery-Plug-in

Der Ernte [ausgewählte](http://harvesthq.github.com/chosen/) jQuery-Plug-in kann hinzugefügt werden, um ein HTML &lt;wählen&gt; Element zum Erstellen eines Benutzers angezeigte Benutzeroberfläche. Die folgenden Bilder zeigen die Ernte [ausgewählte](http://harvesthq.github.com/chosen/) jQuery-Plug-in mit `MultiSelectCountry` anzeigen.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

In den folgenden zwei Bildern **Kanada** ausgewählt ist.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

In der Abbildung oben Kanada ausgewählt ist, und sie enthält ein **x** Sie klicken können, um die Auswahl zu entfernen. Die folgende Abbildung zeigt, China, Kanada und Japan ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Einbinden von der ausgewählten Ernte jQuery-Plug-in

Im folgende Abschnitt ist einfacher zu verwenden, wenn Sie etwas Erfahrung mit jQuery haben. Wenn Sie vor dem jQuery nie verwendet haben, empfiehlt es sich, die folgenden Lernprogramme für jQuery beheben.

- [Funktionsweise von jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) von [John Resig](http://ejohn.org/)
- [Erste Schritte mit jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) von [Jörn Zaefferer](http://bassistance.de/)
- [Beispiele für jQuery Live](http://codylindley.com/blogstuff/js/jquery/#) von [Cody Lindley](http://codylindley.com/)

Das ausgewählte Plug-in ist Starter und abgeschlossenen Beispielprojekte, die in diesem Lernprogramm begleitet enthalten. Für dieses Lernprogramm müssen Sie nur jQuery zu verwenden, um bis zu der Benutzeroberfläche zu verknüpfen. Um die ausgewählte Ernte jQuery-Plug-in in einer ASP.NET MVC-Projekt verwenden, müssen Sie folgende Aktionen ausführen:

1. Ausgewählte Plug-In herunterladen [Github](https://github.com/harvesthq/chosen/). Dieser Schritt ist für Sie ausgeführt wurde.
2. Fügen Sie den ausgewählten Ordner dem ASP.NET MVC-Projekt hinzu. Fügen Sie die Ressourcen aus der ausgewählten-Plug-Ins, die Sie im vorherigen Schritt in den ausgewählten Ordner heruntergeladen. Dieser Schritt ist für Sie ausgeführt wurde.
3. Das ausgewählte Plug-in zum Einbinden der **DropDownList** oder **ListBox** HTML-Hilfsobjekt.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Das ausgewählte Plug-in anzuzeigende MultiSelectCountry verknüpft wird.

Öffnen der *Views\Home\MultiSelectCountry.cshtml* und fügen eine `htmlAttributes` Parameter an die `Html.ListBox`. Sie fügen der-Parameter enthält einen Klassennamen für die select-Liste (`@class = "chzn-select"`). Der vollständige Code wird unten gezeigt:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Wir sind im obigen Code hinzufügen, die HTML-Attribut und Attributwert `class = "chzn-select"`. Das @-Zeichen vorausgeht Klasse hat nichts mit Razor-Ansichtsmodul tun. `class`ist eine [C#-Schlüsselwort](https://msdn.microsoft.com/library/x53a06bb.aspx). C#-Schlüsselwörter können nicht als Bezeichner verwendet werden, es sei denn, sie als Präfix @ enthalten. Im Beispiel oben `@class` ist ein gültiger Bezeichner jedoch **Klasse** ist, da **Klasse** ist ein Schlüsselwort.

Fügen Sie Verweise auf die *Chosen/chosen.jquery.js* und *Chosen/chosen.css* Dateien. Die *Chosen/chosen.jquery.js* und implementiert die funktional der das ausgewählte Plug-in. Die *Chosen/chosen.css* Datei enthält die Formatvorlage. Fügen Sie diese Verweise auf den unteren Rand der *Views\Home\MultiSelectCountry.cshtml* Datei. Der folgende Code zeigt, wie auf das ausgewählte Plug-in verwiesen wird.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktivieren Sie das ausgewählte Plug-in über den Klassennamen in verwendet das **Html.ListBox** Code. Im obigen Beispiel ist der Name der Klasse `chzn-select`. Fügen Sie die folgende Zeile am Ende der *Views\Home\MultiSelectCountry.cshtml* Datei anzeigen. Diese Zeile wird das ausgewählte Plug-in aktiviert.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Die folgende Zeile ist die Syntax zum Aufrufen von der jQuery-Funktion bereit, die das DOM-Element mit dem Klassennamen auswählt `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Legen Sie dann aus dem oben genannten Aufruf zurückgegebene umschlossene gilt die gewählte Methode (`.chosen();`), die das ausgewählte Plug-in verknüpft.

Der folgende Code zeigt das abgeschlossene *Views\Home\MultiSelectCountry.cshtml* Datei anzeigen.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu der `MultiSelectCountry` anzeigen. Versuchen Sie es hinzufügen und Löschen von Ländern. Das bereitgestellte Beispiel zum Herunterladen enthält auch eine `MultiCountryVM` -Methode und anzeigen, die die MultiSelectCountry-Funktionalität mithilfe einer Sicht zu modellieren, statt eine **ViewBag**.

Im nächsten Abschnitt sehen Sie die Funktionsweise der ASP.NET MVC-Gerüstbau mit der **DropDownList** Helper.

>[!div class="step-by-step"]
[Nächste](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
