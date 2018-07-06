---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Anzeigen von Binärdaten in der Datenweb-(VB) steuert | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial untersuchen wir die Optionen zum Präsentieren von Binärdaten in eine Webseite, einschließlich der Anzeige der Bilddatei und die Bereitstellung von einem Link "Download" f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: a9d298ef328e951f235a6cfcd41b73fafefb0dfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373106"
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Anzeigen von Binärdaten in den Datenwebsteuerelementen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) oder [PDF-Datei herunterladen](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> In diesem Tutorial untersuchen wir die Optionen zum Präsentieren von Binärdaten in eine Webseite, einschließlich der Anzeige der Bilddatei und die Bereitstellung von einem Link "Download" für eine PDF-Datei ein.


## <a name="introduction"></a>Einführung

Im vorherigen Tutorial haben wir untersucht die beiden Techniken für das Zuordnen von binärer Daten zu einem zugrunde liegenden Datenmodell von Application s und verwendet das Steuerelement "FileUpload" zum Hochladen von Dateien in einem Browser auf s-Dateisystem der Web-Server. Wir haben, um zu erfahren, wie Sie die hochgeladene binäre Daten mit dem Datenmodell zuordnen. Nachdem eine Datei hochgeladen und im Dateisystem gespeichert wurde, muss ein Pfad zu der Datei, also in die entsprechende Datenbank-Datensatz gespeichert werden. Wenn die Daten direkt in der Datenbank gespeichert wird, klicken Sie dann die hochgeladenen binäre Daten müssen nicht im Dateisystem gespeichert werden, aber in die Datenbank eingefügt werden müssen.

Vor dem betrachten wir die Daten mit dem Datenmodell zuordnen, können Sie jedoch, s, die Einführung an, wie die binäre Daten für den Endbenutzer bereitzustellen. Ist darstellen von Textdaten recht einfach, aber wie werden binäre Daten dargestellt soll? Es hängt, natürlich, den Typ der binären Daten. Bilder möchten wir wahrscheinlich das Bild anzuzeigen; für PDF-Dateien ist Microsoft Word-Dokumenten, ZIP-Dateien und andere Arten von Binärdaten, einen Link zum Herunterladen bereitstellen wahrscheinlich besser geeignet.

In diesem Lernprogramm aus, die wir uns an wie die binären Daten zusammen mit der zugehörigen Textdaten, die mit Daten zu präsentieren steuert Web wie GridView und DetailsView. Im nächsten Tutorial werden wir unsere Aufmerksamkeit zum Zuordnen einer hochgeladenen Datei mit der Datenbank aktivieren.

## <a name="step-1-providingbrochurepathvalues"></a>Schritt 1: Bereitstellen von`BrochurePath`Werte

Die `Picture` -Spalte in der `Categories` Tabelle enthält bereits die Binärdaten für die verschiedenen Bilder für die Kategorie. Insbesondere die `Picture` Spalte für jeden Datensatz enthält den binären Inhalt der ein körnig, geringer Qualität, 16 Farben Bitmap-Bild. Jede Kategorie-Image ist 172 Pixel breit und 120 Pixel hoch und beansprucht ungefähr 11 KB. Weitere Neuheiten, wird der binäre Inhalt in die `Picture` -Spalte enthält eine 78 Byte bestehende [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) Header, der entfernt werden muss, bevor das Bild angezeigt. Diesen Headerinformationen ist vorhanden, da die Northwind-Datenbank ihren Wurzeln in Microsoft Access verfügt. In Access mithilfe der OLE-Objekt-Datentyp, der auf diesen Header fügt Binärdaten gespeichert. Jetzt sehen wir, wie Sie die Header aus dieser Bilder mit geringer Qualität zu entfernen, um das Bild anzuzeigen. In einem späteren Tutorial erstellen wir eine Schnittstelle zum Aktualisieren einer Kategorie s `Picture` Spalte, und Ersetzen Sie diese Bitmap-Images, die OLE-Header mit den entsprechenden JPG-Bilder ohne die unnötige OLE-Header verwenden.

Im vorherigen Tutorial wurde erläutert, wie das Steuerelement "FileUpload" verwenden. Aus diesem Grund können Sie fortfahren und s-Dateisystem der Web-Server Broschüre Dateien hinzufügen. Auf diese Weise jedoch aktualisiert nicht die `BrochurePath` -Spalte in der `Categories` Tabelle. Im nächsten Tutorial erfahren Sie, wie ein, um dies zu erreichen, aber jetzt müssen wir Werte für diese Spalte manuell zu bieten.

In diesem Tutorial s Download finden Sie sieben PDF-Broschüre-Dateien in die `~/Brochures` Ordner, eine für jede Kategorie außer Seafood. Ich absichtlich ausgelassen wird, Hinzufügen einer Seafood Broschüre zur Veranschaulichung der Umgang mit Szenarien, in denen nicht alle Datensätze Binärdaten verknüpft haben. Beim Aktualisieren der `Categories` Tabelle mit den folgenden Werten, mit der rechten Maustaste auf die `Categories` Knoten im Server-Explorer, und wählen Sie die Tabellendaten anzeigen. Geben Sie dann die virtuellen Pfade zu den Broschüre-Dateien für jede Kategorie, die eine Broschüre, wie in Abbildung 1 dargestellt. Da keine Broschüre für die Kategorie "Seafood" ist, lassen Sie die `BrochurePath` s Spaltenwert als `NULL`.


[![Geben Sie die Werte manuell für die Kategorien s BrochurePath Tabellenspalte](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Abbildung 1**: Geben Sie manuell die Werte für die `Categories` s'-Tabelle `BrochurePath` Spalte ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Schritt 2: Bereitstellen von einen Downloadlink für die Broschüren in einer GridView-Ansicht

Mit der `BrochurePath` Werte für die `Categories` Tabelle wir erneut bereit, um einer GridView-Ansicht zu erstellen, die in jeder Kategorie zusammen mit einem Link zum Herunterladen der Broschüre Kategorie s werden. In Schritt 4 erweitern wir diese GridView, um auch das kategoriebild s anzuzeigen.

Durch Ziehen einer GridView-Ansicht aus der Toolbox in den Designer der Start der `DisplayOrDownloadData.aspx` auf der Seite die `BinaryData` Ordner. Legen Sie die GridView-s `ID` zu `Categories` und über GridView s Smarttags, wählen Sie sie an eine neue Datenquelle zu binden. Binden Sie es insbesondere um ein mit dem Namen "ObjectDataSource" `CategoriesDataSource` , abruft, Daten mithilfe der `CategoriesBLL` s-Objekt `GetCategories()` Methode.


[![Erstellen Sie eine neue, mit dem Namen CategoriesDataSource "ObjectDataSource"](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der CategoriesBLL-Klasse](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Abbildung 3**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `CategoriesBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Abrufen der Liste der Kategorien, die mit der GetCategories()-Methode](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Abbildung 4**: Rufen Sie die Liste der Kategorien mithilfe der `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Nach dem Fertigstellen des Assistenten für die Datenquelle konfigurieren Visual Studio werden automatisch hinzugefügt, ein BoundField auf die `Categories` GridView für die `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, und `BrochurePath` `DataColumn` s. Fahren Sie fort, und entfernen Sie die `NumberOfProducts` BoundField seit der `GetCategories()` s-Methodenabfrage werden diese Informationen nicht abgerufen werden. Entfernen Sie auch die `CategoryID` BoundField, und benennen Sie die `CategoryName` und `BrochurePath` BoundFields `HeaderText` Eigenschaften die Kategorie und Broschüre, bzw. Nach diesen Änderungen sollte GridView und "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Diese Seite über einen Browser anzuzeigen (siehe Abbildung 5). Jede der acht Kategorien wird angezeigt. Der sieben Kategorien `BrochurePath` Werte haben die `BrochurePath` Wert in der jeweiligen BoundField angezeigt. Seafood, die eine `NULL` Wert für die `BrochurePath`, zeigt eine leere Zelle.


[![Jede Kategorie s Name, Beschreibung und BrochurePath Wert enthält.](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Abbildung 5**: jede Kategorie s Name, Beschreibung und `BrochurePath` Wert aufgeführt wird ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Anstatt den Text der anzuzeigen die `BrochurePath` Spalte, die wir einen Link zu der Broschüre erstellen möchten. Entfernen Sie zu diesem Zweck die `BrochurePath` BoundField und Ersetze ihn durch einen HyperLinkField. Legen Sie die neue HyperLinkField s `HeaderText` Eigenschaft Broschüre, seine `Text` Eigenschaft Broschüre anzeigen, und die zugehörige `DataNavigateUrlFields` Eigenschaft `BrochurePath`.


![Fügen Sie eine HyperLinkField für BrochurePath hinzu.](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Abbildung 6**: Hinzufügen einer HyperLinkField für `BrochurePath`


Dadurch wird eine Spalte von Links an die GridView, hinzugefügt, wie in Abbildung 7 dargestellt. Klicken auf einen Link Broschüre anzeigen wird entweder die PDF-Datei direkt im Browser angezeigt oder fordert den Benutzer zum Herunterladen der Datei, je nachdem, ob ein PDF-Reader installiert ist und der Browsereinstellungen s.


[![Eine Kategorie-s-Broschüre kann angezeigt werden, indem Sie auf den Link zum Anzeigen von Broschüre](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Abbildung 7**: einer Kategorie s Broschüre können angezeigt werden, indem Sie auf den Link zum Anzeigen von Broschüre ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Die Kategorie s Broschüre PDF-Datei wird angezeigt.](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Abbildung 8**: der Kategorie %% s Broschüre PDF-Datei angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Der Text der Ansicht Broschüre ausblenden für Kategorien, ohne eine Broschüre

Wie in Abbildung 7 dargestellt, die `BrochurePath` HyperLinkField zeigt die `Text` Eigenschaftswert (Broschüre anzeigen) für alle Datensätze, unabhängig davon, ob dort s nicht`NULL` Wert für `BrochurePath`. Natürlich Wenn `BrochurePath` ist `NULL`, und klicken Sie dann der Link als nur-Text angezeigt wird, wie bei der Kategorie "Seafood" der Fall ist (siehe Abbildung 7). Anstatt den Text Broschüre anzeigen anzeigt, ist es möglicherweise hilfreich diese Kategorien ohne eine `BrochurePath` Wert anzeigen alternativen Text, z. B. keine Broschüre verfügbar.

Um dieses Verhalten zu bieten, müssen wir ein TemplateField verwenden, dessen Inhalt wird über einen Aufruf einer Seitenmethode, die die jeweilige Ausgabe basierend auf ausgibt, generiert, der `BrochurePath` Wert. Wir zunächst untersucht diese Technik Formatierung wieder in die [Verwenden von TemplateFields, im GridView-Steuerelement](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Tutorial.

Aktivieren Sie die HyperLinkField in ein TemplateField durch Auswählen der `BrochurePath` HyperLinkField und klicken Sie dann auf das konvertierte dieses Feld in ein TemplateField-link in das Dialogfeld "Spalten bearbeiten".


![Die HyperLinkField in ein TemplateField konvertieren](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Abbildung 9**: die HyperLinkField in ein TemplateField konvertieren


Dadurch entsteht ein TemplateField mit einer `ItemTemplate` , enthält ein HyperLink-Web-Steuerelement, dessen `NavigateUrl` Eigenschaft gebunden ist die `BrochurePath` Wert. Ersetzen Sie dieses Markup durch einen Aufruf der Methode `GenerateBrochureLink`, und übergeben Sie den Wert der `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Als Nächstes erstellen Sie eine `Protected` -Methode in der ASP.NET Seite s Code-Behind-Klasse, die mit dem Namen `GenerateBrochureLink` zurückgibt, die eine `String` und akzeptiert eine `Object` als Eingabeparameter.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Diese Methode bestimmt, ob der übergegebenen `Object` Wert ist eine Datenbank `NULL` und, wenn dies der Fall ist, gibt eine Meldung gibt an, dass die Kategorie eine Broschüre fehlt. Wenn vorhanden ist, andernfalls eine `BrochurePath` Wert sie s in einem Link angezeigt. Beachten Sie, dass bei der `BrochurePath` Wert übergebenen s darstellen der [ `ResolveUrl(url)` Methode](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Diese Methode löst der übergegebenen *Url*, und Ersetzen Sie dabei die `~` Zeichen mit dem entsprechenden virtuellen Pfad. Wenn die Anwendung als Stamm ist z. B. `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` zurück `/Tutorial55/Brochures/Meat.pdf`.

Abbildung 10 zeigt die Seite, nachdem diese Änderungen angewendet wurden. Beachten Sie, dass der Kategorie "Seafood" s `BrochurePath` Feld zeigt jetzt den Text, der keine Broschüre verfügbar.


[![Der Text ohne Broschüre verfügbar ist für diese Kategorien ohne ein Broschüre angezeigt](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Abbildung 10**: der Text ohne Broschüre verfügbar für diese Kategorien ohne ein Broschüre angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Schritt 3: Hinzufügen einer Webseite, um ein Bild Kategorie s anzeigen

Wenn ein Benutzer auf eine ASP.NET-Seite besucht, erhalten sie die ASP.NET-Seite s HTML. Das empfangene HTML ist nur mit Text und enthält keine binären Daten. Alle zusätzlichen Binärdaten, z. B. Bilder, Audiodateien, Macromedia Flash-Anwendungen, embedded Windows Media Player-Videos usw., werden als separate Ressourcen auf dem Webserver vorhanden. Der HTML-Code enthält Verweise auf diese Dateien, jedoch keine der tatsächliche Inhalt der Dateien.

Z. B. in HTML die `<img>` Element wird verwendet, um ein Bild mit Verweisen auf die `src` Attribut verweist auf die Imagedatei wie folgt:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Wenn ein Browser HTML-empfängt, erleichtert eine weitere Anforderung an dem Webserver den binären Inhalt der Bilddatei abgerufen, die sie dann im Browser angezeigt wird. Das gleiche Konzept gilt für beliebige binären Daten. In Schritt2 wurde die Broschüre nicht an den Browser als Teil des HTML-Markup der Seite s gesendet, nach unten. Die gerenderte HTML bereitgestellt stattdessen links, die, die beim Klicken auf den Browser, um die PDF-Dokument direkte Anforderung verursacht.

Um anzuzeigen, oder binäre Daten herunterladen, die in der Datenbank befindet sich durch Benutzer zulassen, müssen wir eine separate Webseite erstellen, die der Daten zurückgibt. Bei unserer Anwendung dort s nur ein Binärdaten Feld direkt in der Abbildung der Kategorie-s-Datenbank gespeichert. Daher benötigen wir eine Seite, die beim Aufruf gibt die Bilddaten für eine bestimmte Kategorie.

Hinzufügen einer neuen ASP.NET-Seite zu den `BinaryData` Ordner mit dem Namen `DisplayCategoryPicture.aspx`. Dabei, lassen Sie die Masterseite auswählen das Kontrollkästchen deaktiviert. Diese Seite erwartet, dass eine `CategoryID` Wert in der Abfragezeichenfolge und gibt die binären Daten der dieser Kategorie s `Picture` Spalte. Da auf dieser Seite Binärdaten und nichts zurückgibt, ist es im Abschnitt HTML-Markup nicht erforderlich. Aus diesem Grund klicken Sie auf der Registerkarte "Datenquelle" in der unteren linken Ecke, und entfernen Sie alle im Markup Seite s mit Ausnahme der `<%@ Page %>` Richtlinie. D. h. `DisplayCategoryPicture.aspx` s deklaratives Markup sollte von einer einzelnen Zeile bestehen:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Wenn angezeigt wird der `MasterPageFile` -Attribut in der `<%@ Page %>` Richtlinie, entfernen Sie sie.

Fügen Sie in der Seite "s" Code-Behind-Klasse den folgenden Code, der `Page_Load` -Ereignishandler:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Dieser Code beginnt mit dem Lesen der `CategoryID` Querystring-Wert in einer Variablen namens `categoryID`. Als Nächstes wird die Bilddaten abgerufen, über einen Aufruf an die `CategoriesBLL` Klasse s `GetCategoryWithBinaryDataByCategoryID(categoryID)` Methode. Diese Daten werden an den Client zurückgegeben, mit der `Response.BinaryWrite(data)` -Methode, aber bevor diese aufgerufen wird, die `Picture` Spalte Wert s OLE-Header muss entfernt werden. Dies geschieht durch Erstellen einer `Byte` Array mit dem Namen `strippedImageData` enthält, die genau 78 Zeichen kleiner als die in der `Picture` Spalte. Die [ `Array.Copy` Methode](https://msdn.microsoft.com/library/z50k9bft.aspx) wird verwendet, um das Kopieren der Daten aus `category.Picture` beginnen an der Position 78 `strippedImageData`.

Die `Response.ContentType` Eigenschaft gibt an, die [MIME-Typ](http://en.wikipedia.org/wiki/MIME) des Inhalts wird zurückgegeben, sodass der Browser rendern kann. Da die `Categories` Tabelle s `Picture` Spalte ist eine Bitmap-Bild, das die Bitmap-MIME-Typ wird verwendet, hier (Image/Bmp). Wenn Sie den MIME-Typ weglassen, werden die meisten Browser immer noch das Image ordnungsgemäß angezeigt werden, da sie den Typ basierend auf den Inhalt der Datei s binäre Bilddaten per Rückschluss ableiten können. Allerdings es s rechnen, gehören die MIME nach Möglichkeit zu geben. Finden Sie unter den [Website der Internet Assigned Numbers Authority-s](http://www.iana.org/) für eine vollständige Liste der [MIME-Medientypen](http://www.iana.org/assignments/media-types/).

Mit dieser Seite, die erstellt wird, kann eine bestimmte Kategorie s Bild angezeigt werden finden Sie unter `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Abbildung 11 zeigt das Bild der Getränke-Kategorie s, die aus angezeigt werden kann `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Die Kategorie "Getränke"-s, die Bild angezeigt wird](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Abbildung 11**: die Kategorie "Getränke" s-Bild wird angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


IF, wenn das Unternehmen besuchen `DisplayCategoryPicture.aspx?CategoryID=categoryID`, erhalten Sie eine Ausnahme, die nicht möglich, wandelt ein Objekt des Typs "System.DBNull" Typ "System.Byte []" gelesen, es gibt zwei Dinge, die die Ursache sein können. Zunächst wird die `Categories` Tabelle s `Picture` Spalte lässt `NULL` Werte. Die `DisplayCategoryPicture.aspx` Seite jedoch nimmt an, dass es nicht`NULL` Wert vorhanden. Die `Picture` Eigenschaft der `CategoriesDataTable` kann nicht direkt zugegriffen werden, wenn dies ist eine `NULL` Wert. Wenn Sie zulassen möchten `NULL` Werte für die `Picture` Spalte d die folgende Bedingung enthalten sein sollen:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Der obige Code geht davon aus, gibt es einige image-Datei mit dem Namen s `NoPictureAvailable.gif` in die `Images` Ordner, den Sie für diese Kategorien ohne Bilder anzeigen möchten.

Diese Ausnahme kann auch verursacht werden, wenn die `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` s-Methode `SELECT` Anweisung auf der Liste der Hauptabfrage s-Spalte, dies passieren kann, wenn Sie Ad-hoc-SQL-Anweisungen verwenden und Sie haben, erneut des Assistenten für die TableAdapter ausführen zurückgesetzt wurde die Hauptabfrage. Kontrollkästchen, um sicherzustellen, dass `GetCategoryWithBinaryDataByCategoryID` s-Methode `SELECT` Anweisung umfasst immer noch die `Picture` Spalte.

> [!NOTE]
> Jedes Mal, wenn die `DisplayCategoryPicture.aspx` wird aufgerufen, der die Datenbank zugegriffen wird und die Daten der angegebenen Kategorie s-Grafik werden zurückgegeben. Wenn die Kategorie s Bild hat t geändert, da der Benutzer es zuletzt angezeigt wurde, ist dies jedoch unnötigen Aufwand. Glücklicherweise können Sie HTTP für *bedingte ruft*. Mit bedingten GET, sendet der Client, der die HTTP-Anforderung an eine [ `If-Modified-Since` HTTP-Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , der das Datum und Uhrzeit der letzten der Client diese Ressource vom Webserver abrufen bereitstellt. Wenn der Inhalt nicht geändert hat, da dies das Datum angegeben, kann der Webserver mit folgendem Antworten einer [Statuscode nicht geändert (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) und verzichten, senden den Inhalt der angeforderten Ressource s zurück. Kurz gesagt, muss diese Methode den Webserver von Inhalten für eine Ressource senden, wenn er nicht geändert wurde, seit der Client sie zuletzt verwendet haben.


Um dieses Verhalten zu implementieren allerdings erfordert das Hinzufügen einer `PictureLastModified` Spalte die `Categories` Tabelle beim Erfassen der `Picture` Spalte zuletzt aktualisiert wurde, sowie Code zum Überprüfen der `If-Modified-Since` Header. Weitere Informationen zu den `If-Modified-Since` Header und den bedingten GET-Workflow finden Sie unter [HTTP für RSS-Hacker bedingte GET](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) und [ein ausführlicher betrachten, Ausführen von HTTP-Anforderungen in einer ASP.NET-Seite](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Schritt 4: Anzeigen der Kategorie-Bilder in einer GridView-Ansicht

Nun, da wir eine Webseite zum Anzeigen einer bestimmten Kategorie s Bild haben, können angezeigt, mit der [Image-Websteuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) oder eine HTML- `<img>` Element auf `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Bilder, deren URL durch Daten bestimmt wird, können in der GridView oder mithilfe der ImageField DetailsView angezeigt werden. Enthält die ImageField `DataImageUrlField` und `DataImageUrlFormatString` Eigenschaften, die wie die HyperLinkField s funktionieren `DataNavigateUrlFields` und `DataNavigateUrlFormatString` Eigenschaften.

S erweitern, können die `Categories` GridView in `DisplayOrDownloadData.aspx` durch Hinzufügen einer ImageField jedes Bild Kategorie s angezeigt. Die ImageField Sie fügen einfach, und legen Sie dessen `DataImageUrlField` und `DataImageUrlFormatString` Eigenschaften `CategoryID` und `DisplayCategoryPicture.aspx?CategoryID={0}`bzw. Dadurch entsteht eine GridView-Spalte, die rendert eine `<img>` Element, dessen `src` attributverweise `DisplayCategoryPicture.aspx?CategoryID={0}`, wobei {0} wird mit der GridView-Zeile s ersetzt `CategoryID` Wert.


![Hinzufügen einer ImageField an die GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Abbildung 12**: Hinzufügen einer ImageField an die GridView


Nach dem Hinzufügen der ImageField an, die GridView s deklarative Syntax sieht wie soothe folgende:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Nehmen Sie einen Moment Zeit, zum Anzeigen dieser Seite über einen Browser ein. Beachten Sie, wie jeder Datensatz jetzt ein Bild für die Kategorie enthält.


[![Die Kategorie s Bild wird für jede Zeile angezeigt.](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Abbildung 13**: Bild, für jede Zeile angezeigt wird die Kategorie-s ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial untersucht es wie binäre Daten dargestellt. Wie die Daten dargestellt werden, hängt von den Typ der Daten ab. Für die PDF-Broschüre-Dateien, wir angeboten dem Benutzer eine Ansicht Broschüre verknüpft werden, dass, wenn Sie geklickt haben, hat den Benutzer direkt in die PDF-Datei. Für das Category-s-Bild müssen wir zuerst eine Seite zum Abrufen und Zurückgeben von Binärdaten aus der Datenbank erstellt und verwendet dann diese Seite jeder Kategorie-s-Bild in einer GridView-Ansicht angezeigt.

Nun, wir haben erläutert, wie zum Anzeigen von Binärdaten, es erneut bereit, die zum Ausführen von INSERT-, Updates und löschungen in der Datenbank die binären Daten zu untersuchen. Im nächsten Tutorial betrachten wir zum Zuordnen einer hochgeladenen Datei mit der entsprechenden Datenbankdatensatz. In diesem Tutorial, sehen wir, wie beim Aktualisieren von vorhandener Binärdaten als auch die binären Daten zu löschen, wenn die zugehörige Eintrag entfernt wird.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Teresa Murphy und Dave Gardner. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](uploading-files-vb.md)
> [Weiter](including-a-file-upload-option-when-adding-a-new-record-vb.md)
