---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Steuert die Anzeige von Binärdaten in der webbasierten Daten (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm betrachten wir die Optionen zum Präsentieren von Binärdaten in eine Webseite, einschließlich der Anzeige einer Bilddatei und das Bereitstellen eines Links "Herunterladen" f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b56fc45ea8fb5aee934530fc62e23b9d364242
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>Anzeigen von Binärdaten in den Data-Websteuerelementen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) oder [PDF herunterladen](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> In diesem Lernprogramm betrachten wir die Optionen zum Präsentieren von Binärdaten in eine Webseite, einschließlich der Anzeige einer Bilddatei und das Bereitstellen eines Links "Herunterladen" für eine PDF-Datei ein.


## <a name="introduction"></a>Einführung

In dem vorherigen Lernprogramm wir untersucht die zwei Techniken zum Zuordnen von binären Daten mit einem zugrunde liegenden Datenmodell von Anwendung s und verwendet das FileUpload-Steuerelement zum Hochladen von Dateien in einem Browser die Web Server s im Dateisystem. Wir haben noch, um festzustellen, wie die hochgeladenen Binärdaten mit dem Datenmodell zugeordnet. Nachdem eine Datei hochgeladen und im Dateisystem gespeichert wurde, muss ein Pfad zu der Datei, also in die entsprechende Datenbank-Datensatzes gespeichert werden. Wenn die Daten direkt in der Datenbank gespeichert ist, klicken Sie dann die hochgeladenen Binärdaten müssen nicht im Dateisystem gespeichert werden, aber in die Datenbank eingefügt werden müssen.

Bevor wir sehen Sie sich die Daten mit dem Datenmodell zuordnen, können Sie jedoch s uns zunächst an, wie die Binärdaten für den Endbenutzer bereitstellen. Ist darstellen von Textdaten einfach genug, aber wie werden Binärdaten dargestellt soll? Es hängt vom, natürlich Typ von Binärdaten. Für Bilder möchten wir wahrscheinlich auch zur Anzeige des Bilds; für PDF-Dateien Microsoft Word-Dokumente, ZIP-Dateien und anderen Arten von Binärdaten, bietet einen Downloadlink ist wahrscheinlich besser geeignet.

In diesem Lernprogramm aus, die wir an, wie die binären Daten zusammen mit seiner zugehörigen Textdaten, die mit Daten zu präsentieren Ereignishandlerabschnitt steuert Web wie GridView und DetailsView. In den nächsten Lernprogrammen müssen wir unsere Aufmerksamkeit Verknüpfen einer hochgeladenen Datei mit der Datenbank aktivieren.

## <a name="step-1-providingbrochurepathvalues"></a>Schritt 1: Bereitstellung`BrochurePath`Werte

Die `Picture` Spalte in der `Categories` Tabelle enthält bereits die Binärdaten für die verschiedenen Kategorie Bilder. Insbesondere die `Picture` Spalte für jeden Datensatz enthält den binären Inhalt ein Bitmapbild körnig, niedrige Qualität, 16 Farben. Jede Kategorie-Abbild wird 172 Pixel breit und 120 Pixel hoch und belegt ungefähr 11 KB. Welche weitere s, den Inhalt im Binärformat in der `Picture` -Spalte enthält eine 78 Byte bestehende [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) Header, der entfernt werden muss, ehe Sie das Bild. Diesen Headerinformationen ist vorhanden, da die Northwind-Datenbank die Stämme in Microsoft Access enthält. In Access ist die binäre Daten mit dem Datentyp OLE-Objekt, das auf diesen Header ergreift gespeichert. Jetzt sehen wir, wie die Header aus dieser Bilder mit schlechter Qualität zu entfernen, um das Bild anzuzeigen. In einem späteren Lernprogramm müssen wir erstellen eine Schnittstelle für das Aktualisieren einer Kategorie s `Picture` Spalte, und Ersetzen Sie diese Bitmap-Images, die OLE-Header mit entsprechenden JPG-Bilder ohne die unnötige OLE-Header verwenden.

In dem vorherigen Lernprogramm wurde erläutert, wie das Steuerelement FileUpload verwenden. Aus diesem Grund können Sie fahren Sie fort und fügen Sie die Web Server s im Dateisystem Broschüren Dateien hinzu. Auf diese Weise jedoch nicht aktualisiert die `BrochurePath` Spalte in der `Categories` Tabelle. In den nächsten Lernprogrammen erfahren wir, wie Sie dies zu erreichen, aber vorläufig müssen Werte für diese Spalte manuell bereitstellen.

In diesem Lernprogramm s Download finden Sie sieben Broschüren-PDF-Dateien in den `~/Brochures` Ordner, eine für jede Kategorie außer Seafood. Ich weggelassen absichtlich Hinzufügen einer Seafood Broschüren zur Veranschaulichung Szenarien behandeln, in denen nicht alle Datensätze Binärdaten verknüpft haben. Beim Aktualisieren der `Categories` Tabelle mit den folgenden Werten, mit der rechten Maustaste auf die `Categories` Knoten im Server-Explorer, und wählen Sie Tabellendaten anzeigen. Geben Sie dann die virtuellen Pfade zu den Dateien Broschüren für jede Kategorie, die eine Broschüren wie in Abbildung 1 dargestellt wurde. Da es keine Broschüren für die Kategorie Seafood ist, behalten ihre `BrochurePath` Spaltenwert s als `NULL`.


[![Geben Sie die Werte manuell für die Kategorien s BrochurePath Tabellenspalte](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Abbildung 1**: Geben Sie manuell die Werte für die `Categories` Tabelle s `BrochurePath` Spalte ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Schritt 2: Einen Downloadlink für die Broschüren in einem GridView bereitstellen

Mit der `BrochurePath` Wertetabelle für die `Categories` Tabelle wir re kann jetzt eine GridView zu erstellen, die jede Kategorie zusammen mit einem Link zum Herunterladen der Kategorie s Broschüren aufgelistet. In Schritt 4 verlängern wir diese GridView um auch auf kategoriebild s anzuzeigen.

Starten, indem Sie eine GridView aus der Toolbox auf den Designer ziehen die `DisplayOrDownloadData.aspx` auf der Seite der `BinaryData` Ordner. Legen Sie die GridView s `ID` auf `Categories` und wählen Sie über den GridView s smart Tag, um es an eine neue Datenquelle binden. Binden sie insbesondere an eine mit dem Namen ObjectDataSource `CategoriesDataSource` , abruft, Daten mithilfe der `CategoriesBLL` s-Objekt `GetCategories()` Methode.


[![Erstellen Sie eine neue, mit dem Namen CategoriesDataSource ObjectDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource namens `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der CategoriesBLL-Klasse](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![Abrufen der Liste der Kategorien, die mit der GetCategories()-Methode](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Abbildung 4**: Rufen Sie die Liste der Kategorien mithilfe der `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


Nach Abschluss des Assistenten konfigurieren von Datenquellen hinzufügen Visual Studio automatisch ein BoundField auf die `Categories` GridView für die `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, und `BrochurePath` `DataColumn` s. Fahren Sie fort, und entfernen Sie die `NumberOfProducts` BoundField seit der `GetCategories()` Methode s Abfrage wird diese Informationen nicht abgerufen werden. Auch entfernen, die `CategoryID` BoundField und benennen Sie die `CategoryName` und `BrochurePath` BoundFields `HeaderText` zu Kategorie und Broschüren, Eigenschaften bzw. Nachdem diese Änderungen vorgenommen wurden, sollte die GridView und ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Diese Seite über einen Browser anzeigen (siehe Abbildung 5). Jede der acht Kategorien wird aufgeführt. Die sieben Kategorien mit `BrochurePath` Werte haben die `BrochurePath` Wert in der jeweiligen BoundField angezeigt. Seafood, besitzt eine `NULL` Wert für seine `BrochurePath`, zeigt eine leere Zelle.


[![Jede Kategorie s Name, Beschreibung und BrochurePath-Wert enthält.](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Abbildung 5**: jede Kategorie s Name, Beschreibung und `BrochurePath` aufgelistete Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


Anstatt den Text der Anzeige der `BrochurePath` Spalte wir einen Link zu der Broschüren erstellen möchten. Um dies zu erreichen, entfernen Sie die `BrochurePath` BoundField und Ersetzen Sie ihn durch einen HyperLinkField. Legen Sie die neue HyperLinkField s `HeaderText` Eigenschaft Broschüren, seine `Text` Eigenschaft, um die Ansicht Broschüren, und die zugehörige `DataNavigateUrlFields` Eigenschaft `BrochurePath`.


![Fügen Sie eine HyperLinkField für BrochurePath hinzu.](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Abbildung 6**: Hinzufügen einer HyperLinkField für `BrochurePath`


Dadurch wird eine Spalte von Links an die GridView hinzugefügt, wie in Abbildung 7 gezeigt. Klicken auf einen Link anzeigen Broschüren wird entweder die PDF-Datei direkt im Browser anzeigen oder fordert den Benutzer zum Herunterladen der Datei, je nachdem, ob ein PDF-Reader installiert ist und die Browsereinstellungen s.


[![Eine Kategorie s Broschüren kann angezeigt werden, indem Sie auf die Ansicht Broschüren Link](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Abbildung 7**: eine Kategorie s Broschüren können angezeigt werden, indem Sie auf die Ansicht Broschüren Link ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![Die Kategorie s Broschüren PDF wird angezeigt.](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Abbildung 8**: die Kategorie s Broschüren PDF angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ausblenden von Text der Broschüren für Kategorien ohne eine Broschüren

Wie Abbildung 7 zeigt ein Beispiel, das `BrochurePath` HyperLinkField zeigt seine `Text` Eigenschaftswert (Ansicht Broschüren) für alle Datensätze, unabhängig davon, ob dort s nicht`NULL` Wert für `BrochurePath`. Natürlich ist es möglich, wenn `BrochurePath` ist `NULL`, und klicken Sie dann der Link als nur-Text angezeigt wird wie bei der Kategorie "Seafood" der Fall ist (siehe Abbildung 7). Statt der Text anzeigen Broschüren angezeigt, ist es möglicherweise nützliche dieser Kategorien ohne eine `BrochurePath` alternativen Text, z. B. keine Broschüren verfügbar anzuzeigen.

Um dieses Verhalten zu bieten, müssen wir ein TemplateField verwenden, deren Inhalt wird über einen Aufruf an eine Methode, die die jeweilige Ausgabe basierend auf ausgibt generiert, die `BrochurePath` Wert. Wir zuerst untersucht diese Technik Formatierung zurück in die [mithilfe von TemplateFields, im des GridView-Steuerelements](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Lernprogramm.

Aktivieren Sie die HyperLinkField in ein TemplateField durch Auswahl der `BrochurePath` HyperLinkField klicken und dann auf die Convert dieses Feld in ein TemplateField verknüpfen Sie im Dialogfeld Spalten bearbeiten.


![Die HyperLinkField in ein TemplateField konvertieren](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Abbildung 9**: die HyperLinkField in ein TemplateField konvertieren


Dadurch wird ein TemplateField mit erstellt ein `ItemTemplate` , enthält einen HyperLink-Web-Steuerelement, dessen `NavigateUrl` Eigenschaft gebunden ist die `BrochurePath` Wert. Ersetzen Sie dieses Markup durch einen Aufruf der Methode `GenerateBrochureLink`, und übergeben Sie den Wert des `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Als Nächstes erstellen Sie eine `protected` in der ASP.NET-Methode-Methode Seite s Code-Behind-Klasse, die mit dem Namen `GenerateBrochureLink` zurückgibt eine `string` und akzeptiert eine `object` als Eingabeparameter.


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Diese Methode bestimmt, ob das übergebene in `object` Wert ist eine Datenbank `NULL` und, wenn dies der Fall ist, gibt eine Meldung gibt an, dass die Kategorie einer Broschüren fehlt. Wenn vorhanden ist, andernfalls ein `BrochurePath` Wert, es s in einem Link angezeigt. Beachten Sie, dass bei der `BrochurePath` liegt sie s übergebenen präsentieren die [ `ResolveUrl(url)` Methode](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Diese Methode löst übergebenen *Url*, und ersetzen die `~` Zeichen mit dem entsprechenden virtuellen Pfad. Wenn die Anwendung das Stammverzeichnis ist z. B. `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` zurück `/Tutorial55/Brochures/Meat.pdf`.

Abbildung 10 zeigt die Seite, nachdem diese Änderungen angewendet wurden. Beachten Sie, dass die Kategorie "Seafood" s `BrochurePath` Feld zeigt jetzt den Text, der keine Broschüren verfügbar.


[![Der Text keine Broschüren verfügbar für die Kategorien ohne ein angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Abbildung 10**: der Text keine Broschüren verfügbar wird angezeigt, für die Kategorien ohne ein ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Schritt 3: Hinzufügen einer Webseite, um eine Kategorie s Bild anzuzeigen

Wenn ein Benutzer auf eine ASP.NET-Seite besucht, empfängt er ASP.NET s HTML-Seite. Die empfangene HTML ist nur Text und enthält keinen Binärdaten bestehen. Alle zusätzlichen Binärdaten, z. B. Bilder, Audiodateien, Macromedia Flash-Anwendungen, eingebettete Windows Media Player-Videos usw., sind als separate Ressourcen auf dem Webserver vorhanden. Der HTML-Code enthält Verweise auf diese Dateien, aber nicht der tatsächliche Inhalt der Dateien einschließt.

Z. B. in HTML der `<img>` Element wird verwendet, um ein Bild mit Verweisen auf die `src` Attribut verweist auf die Image-Datei wie folgt:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Wenn ein Browser folgenden HTML-Code erhält, erleichtert eine andere Anforderung an dem Webserver, den binären Inhalt der Bilddatei abzurufen, wodurch die klicken Sie dann im Browser angezeigt. Dasselbe Konzept gilt für alle Binärdaten. In Schritt2 wurde der Broschüren nicht an den Browser als Teil des HTML-Markup der Seite s gesendet, nach unten. Das gerenderte HTML bereitgestellt stattdessen Hyperlinks, die, die beim Klicken auf den Browser, um das PDF-Dokument direkte Anforderung verursacht.

Um anzuzeigen, oder Benutzerberechtigungen zum Herunterladen von binären Daten, die sich innerhalb der Datenbank befindet, müssen wir einer separaten Webseite zu erstellen, die Daten zurückgibt. Für die Anwendung dort s nur ein binäres Feld "Daten" direkt in der Abbildung der Category-s-Datenbank gespeichert. Deshalb benötigen wir eine Seite, die beim Aufruf gibt die Bilddaten für eine bestimmte Kategorie.

Fügen Sie eine neue ASP.NET-Seite, um die `BinaryData` Ordner mit dem Namen `DisplayCategoryPicture.aspx`. Hierbei, lassen Sie das Kontrollkästchen Masterseite auswählen deaktiviert. Diese Seite erwartet eine `CategoryID` Wert in der Abfragezeichenfolge und gibt die Binärdaten des dieser Kategorie s `Picture` Spalte. Da auf dieser Seite Binärdaten und nichts zurückgibt, ist es nicht im Abschnitt HTML-Markup erforderlich. Aus diesem Grund klicken Sie auf der Registerkarte "Datenquelle" in der unteren linken Ecke, und entfernen Sie alle im Markup Seite s mit Ausnahme der `<%@ Page %>` Richtlinie. D. h. `DisplayCategoryPicture.aspx` s deklarativem Markup sollte von einer einzelnen Zeile bestehen:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Wenn dort der `MasterPageFile` Attribut in der `<%@ Page %>` Richtlinie entfernen.

Fügen Sie in der Seite "s" Code-Behind-Klasse den folgenden Code zu der `Page_Load` Ereignishandler:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Dieser Code beginnt mit dem Lesen in den `CategoryID` Querystring-Wert in einer Variablen namens `categoryID`. Als Nächstes die Bilddaten abgerufen wird, über einen Aufruf an die `CategoriesBLL` Klasse s `GetCategoryWithBinaryDataByCategoryID(categoryID)` Methode. Diese Daten an den Client zurückgegeben werden, mithilfe der `Response.BinaryWrite(data)` -Methode, aber bevor er aufgerufen wird, die `Picture` Spalte Wert s OLE-Header muss entfernt werden. Wird dies durch Erstellen einer `byte` Array mit dem Namen `strippedImageData` enthält, die genau 78 Zeichen kleiner als die in der `Picture` Spalte. Die [ `Array.Copy` Methode](https://msdn.microsoft.com/library/z50k9bft.aspx) dient zum Kopieren der Daten aus `category.Picture` ab Position 78 über auf `strippedImageData`.

Die `Response.ContentType` Eigenschaft gibt an, die [MIME-Typ](http://en.wikipedia.org/wiki/MIME) des Inhalts zurückgegeben wird, damit der Browser weiß, wie es gerendert. Da die `Categories` Tabelle s `Picture` Spalte ist eine Bitmap, die Bitmap MIME-Typ wird verwendet, hier (Image/Bmp). Wenn Sie den MIME-Typ weglassen, werden die meisten Browser noch das Bild ordnungsgemäß angezeigt werden, da sie den Typ, der basierend auf dem Inhalt der Datei s binäre Bilddaten per Rückschluss ableiten können. Allerdings es unerlässlich, gehören die MIME s eingeben, wenn möglich. Finden Sie unter der [Internet Assigned Numbers Authority s Website](http://www.iana.org/) für eine vollständige Liste der [MIME-Medientypen](http://www.iana.org/assignments/media-types/).

Mit dieser Seite erstellt, eine bestimmte Kategorie s Bild angezeigt werden kann, indem besuchen `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Abbildung 11 zeigt das Bild der Getränke-Kategorie s, die in angezeigt werden kann `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Die Kategorie "Getränke"-s, auf die Bild angezeigt wird](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Abbildung 11**: die Kategorie "Getränke" s Bild angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


IF, wenn das Unternehmen besuchende `DisplayCategoryPicture.aspx?CategoryID=categoryID`, erhalten Sie eine Ausnahme, die auf Cast-Objekt des Typs "System.DBNull" Typ "System.Byte []" liest, es gibt zwei Dinge, die dies verursachen können. Zunächst wird die `Categories` Tabelle s `Picture` Spalte lässt `NULL` Werte. Die `DisplayCategoryPicture.aspx` Seite jedoch davon aus, dass es einen nicht-`NULL` Wert vorhanden. Die `Picture` Eigenschaft von der `CategoriesDataTable` kann nicht direkt zugegriffen werden, wenn es wurde eine `NULL` Wert. Wenn Sie zulassen möchten `NULL` Werte für die `Picture` Spalte d die folgende Bedingung enthalten sein sollen:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

Der obige Code geht davon aus, gibt es einige Bilddatei, die mit dem Namen s `NoPictureAvailable.gif` in den `Images` Ordner, der für diese Kategorien ohne ein Bild angezeigt werden soll.

Diese Ausnahme kann verursacht werden, wenn die `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` Methode s `SELECT` -Anweisung wurde zurückgesetzt, an der Hauptabfrage s Spaltenliste; dies geschehen kann, wenn Sie Ad-hoc-SQL-Anweisungen und gibt Sie es erneut führen Sie den Assistenten für die TableAdapter hauptspeicherpool für Abfragen. Kontrollkästchen, um sicherzustellen, dass `GetCategoryWithBinaryDataByCategoryID` Methode s `SELECT` -Anweisung enthält, weiterhin die `Picture` Spalte.

> [!NOTE]
> Jedes Mal, wenn die `DisplayCategoryPicture.aspx` ist besucht, wird die Datenbank zugegriffen wird und die Daten der angegebenen Kategorie s-Bild werden zurückgegeben. Wenn die Kategorie s Bild erkannt t geändert, da der Benutzer zuletzt angezeigt hat, ist dies jedoch unnötigen Aufwand. Glücklicherweise ermöglicht HTTP *bedingte ruft*. Mit einer bedingten GET, sendet der Client, der die HTTP-Anforderung entlang einer [ `If-Modified-Since` HTTP-Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , Datum und Uhrzeit der letzten der Client diese Ressource vom Webserver abrufen bereitstellt. Wenn der Inhalt nicht geändert wurde, da dies Datum angegeben, der Webserver mit folgendem Antworten kann eine [Statuscode nicht geändert (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) und Zurücksenden des angeforderten Ressource s Inhalts verzichten. Diese Technik Infrastrukturkosten kurz gesagt, den Webserver von Inhalten für eine Ressource gesendet, wenn es nicht geändert wurde, seit der Client sie zuletzt verwendet haben.


Um dieses Verhalten zu implementieren allerdings erfordert das Hinzufügen einer `PictureLastModified` Spalte die `Categories` Tabelle beim Erfassen der `Picture` Spalte zuletzt aktualisiert wurde, sowie Code zum Überprüfen der `If-Modified-Since` Header. Weitere Informationen zu den `If-Modified-Since` Header und bedingten GET-Workflow finden Sie unter [HTTP bedingte GET für RSS Hacker](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) und [ein tiefer sehen Sie sich das Ausführen von HTTP-Anforderungen auf einer ASP.NET-Seite](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Schritt 4: Anzeigen der Bilder Kategorie in eine GridView

Nun mit dem eine Webseite, eine bestimmte Kategorie s Bild anzuzeigen, können wir zeigen Sie es mithilfe der [Image-Websteuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) oder eine HTML- `<img>` Element zeigen `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Bilder von Datenbankdaten, dessen URL bestimmt wird, können in der GridView oder mithilfe der ImageField DetailsView angezeigt werden. Enthält die ImageField `DataImageUrlField` und `DataImageUrlFormatString` Eigenschaften, die wie die s HyperLinkField funktionieren `DataNavigateUrlFields` und `DataNavigateUrlFormatString` Eigenschaften.

S erweitern, können die `Categories` GridView in `DisplayOrDownloadData.aspx` durch Hinzufügen einer ImageField um jede Kategorie s Bild anzuzeigen. Einfach die ImageField hinzufügen, und legen seine `DataImageUrlField` und `DataImageUrlFormatString` Eigenschaften `CategoryID` und `DisplayCategoryPicture.aspx?CategoryID={0}`zugeordnet. Dadurch wird eine GridView-Spalte, die rendert erstellt ein `<img>` Element, dessen `src` attributverweise `DisplayCategoryPicture.aspx?CategoryID={0}`, wobei {0} mit der Zeile GridView s ersetzt wird `CategoryID` Wert.


![Hinzufügen einer ImageField an die GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Abbildung 12**: Hinzufügen einer ImageField an die GridView


Nach dem Hinzufügen der ImageField, sollte die GridView s deklarative Syntax wie soothe aussehen folgende:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Beachten Sie, wie jeder Datensatz jetzt ein Bild für die Kategorie enthält.


[![Die Kategorie s Bild wird für jede Zeile angezeigt.](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Abbildung 13**: die Kategorie s Bild für jede Zeile angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm untersucht wir Gewusst wie: Darstellen von Binärdaten. Darstellung der Daten hängt vom Typ der Daten ab. Broschüren PDF-Dateien, wir angeboten dem Benutzer eine Ansicht Broschüren verknüpfen, die beim Klicken auf die Benutzer direkt an die PDF-Datei dauerte. Die Kategorie-s-Bild haben wir eine Seite zum Abrufen und Zurückgeben von Binärdaten aus der Datenbank zum ersten Mal erstellt und dann diese Seite verwendet, um jede Kategorie s Bild in einer GridView anzeigen.

Jetzt, dass wir haben erläutert, Vorgehensweise beim Anzeigen von Binärdaten, wir re bereit zum Ausführen von INSERT-, Updates und Löschvorgänge in der Datenbank die binären Daten zu untersuchen. In den nächsten Lernprogrammen sehen wir uns wie ihre entsprechenden Datenbankeintrag eine hochgeladene Datei zugeordnet. In diesem Lernprogramm danach sehen wir, zum Aktualisieren von vorhandener Binärdaten als auch die binären Daten zu löschen, wenn die zugehörige Eintrag entfernt wird.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Teresa Murphy und Dave Gardner. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](uploading-files-cs.md)
> [Weiter](including-a-file-upload-option-when-adding-a-new-record-cs.md)
