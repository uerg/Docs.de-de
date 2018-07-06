---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Deklarative Parameter (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir veranschaulichen ein Parametersatz, der auf einen hartcodierten Wert verwenden, zur Auswahl der Daten in einem DetailsView-Steuerelement angezeigt.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 01330d6c743fa9534cdb5dfa42bde5dbbe954c40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839228"
---
<a name="declarative-parameters-vb"></a>Deklarative Parameter (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) oder [PDF-Datei herunterladen](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> In diesem Tutorial werden wir veranschaulichen ein Parametersatz, der auf einen hartcodierten Wert verwenden, zur Auswahl der Daten in einem DetailsView-Steuerelement angezeigt.


## <a name="introduction"></a>Einführung

In der [letzten Tutorial](displaying-data-with-the-objectdatasource-vb.md) erläutert, Anzeigen von Daten mit die GridView, DetailsView oder FormView-Steuerelemente gebunden werden, um ein ObjectDataSource-Steuerelement, das aufgerufen der `GetProducts()` Methode aus der `ProductsBLL` Klasse. Die `GetProducts()` Methode gibt eine stark typisierte DataTable, die mit der alle Datensätze aus der Northwind-Datenbank aufgefüllt `Products` Tabelle. Die `ProductsBLL` -Klasse enthält zusätzliche Methoden zum Zurückgeben nur Teilmengen der Produkte - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, und `GetProductsBySupplierID(supplierID)`. Diese drei Methoden erwarten einen Eingabeparameter, der angibt, wie Sie die zurückgegebenen Produktinformationen zu filtern.

Zum Aufrufen von Methoden, die Eingabeparameter zu erwarten, aber zu diesem Zweck müssen wir angeben, woher die Werte für diese Parameter kommen, kann dem ObjectDataSource-Steuerelement verwendet werden. Die Parameterwerte können hartcodiert werden, oder können stammen aus einer Vielzahl von dynamischen Quellen, einschließlich: Querystring-Werte, Sitzungsvariablen, Wert der Eigenschaft für ein Steuerelement auf der Seite, oder andere.

In diesem Tutorial beginnen zeigen, wie Sie mithilfe eines Parameters, der auf einen hartcodierten Wert festgelegt. Insbesondere betrachten wir eine DetailsView hinzufügen, um die Seite mit Informationen zu einem bestimmten Produkt, d. h. die Chef-Anton Gumbo Mischung mit einem `ProductID` 5. Als Nächstes sehen wir, wie Sie den Wert des Parameters, basierend auf einem Websteuerelement festlegen. Insbesondere verwenden wir ein TextBox-Element, damit der Benutzer, die in einem anderen Land, geben Sie nach dem sie eine Schaltfläche, um die Liste mit Lieferanten, die in diesem Land befinden, finden klicken können.

## <a name="using-a-hard-coded-parameter-value"></a>Verwenden einen hartcodierten Parameterwert

Im ersten Beispiel zu starten, durch das Hinzufügen von einem DetailsView-Steuerelement, um die `DeclarativeParams.aspx` auf der Seite die `BasicReporting` Ordner. Wählen Sie aus DetailsViews Smarttag &lt;neue Datenquelle&gt; aus der Dropdownliste aus, und wählen Sie ein ObjectDataSource-Steuerelement hinzufügen.


[![Ein ObjectDataSource-Steuerelement auf der Seite hinzufügen](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Abbildung 1**: Fügen Sie ein ObjectDataSource-Steuerelement auf der Seite ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image3.png))


Dadurch wird die Assistenten für das ObjectDataSource-Steuerelement-Datenquelle wählen Sie automatisch gestartet. Wählen Sie die `ProductsBLL` -Klasse aus dem ersten Bildschirm des Assistenten.


[![Wählen Sie die ProductsBLL-Klasse](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Abbildung 2**: Wählen Sie die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image6.png))


Da wir Informationen über ein bestimmtes Produkt angezeigt werden soll, die wir verwenden möchten die `GetProductByProductID(productID)` Methode.


[![Wählen Sie die GetProductByProductID(productID)-Methode](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie die `GetProductByProductID(productID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image9.png))


Da die Methode, die wir ausgewählt haben, einen Parameter enthält, ist eine weitere Bildschirm für den Assistenten, wobei wir gefragt werden, zum Definieren des Wertes für den Parameter verwendet werden. Die Liste auf der linken Seite zeigt alle Parameter für die ausgewählte Methode. Für `GetProductByProductID(productID)` ist nur ein `productID`. Auf der rechten Seite können wir den Wert für den ausgewählten Parameter angeben. Dropdownliste für die Parameter-Quelle Listet die verschiedenen möglichen Quellen für den Parameterwert auf. Da wir einen hartcodierten Wert 5 für angeben möchten die `productID` -Parameter, lassen Sie die Parameter-Quelle mit "None", und geben Sie 5 in das Textfeld "DefaultValue" ein.


[![Eine Hard-Coded Parameter Wert von 5 verwendet für die ProductID-Parameter](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Abbildung 4**: A Hard-Coded Parameter Wert von 5 wird die `productID` Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image12.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten deklaratives Markup für das ObjectDataSource-Steuerelement enthält eine `Parameter` -Objekt in der `SelectParameters` Auflistung für jeden der von der Methode definiert, die Eingabeparameter der `SelectMethod` Diese Eigenschaft. Da die Methode, die in diesem Beispiel wir verwenden nur einen einzelnen Eingabeparameter erwartet `parameterID`, hier nur ein Eintrag vorhanden ist. Die `SelectParameters` Sammlung kann jede abgeleitete Klasse enthalten die `Parameter` -Klasse in der `System.Web.UI.WebControls` Namespace. Für die hartcodierten Parameterwerte in der Base `Parameter` Klasse wird verwendet, aber der andere Parameter Source-Optionen einer abgeleiteten `Parameter` Klasse wird verwendet; Sie können auch eigene erstellen [benutzerdefinierten Parametertypen](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), falls erforderlich.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Wenn auf Ihrem eigenen Computer deklarative Markup Anschluss sehen Sie an diesem Punkt Mai, die Werte für die `InsertMethod`, `UpdateMethod`, und `DeleteMethod` Eigenschaften als auch `DeleteParameters`. Das "ObjectDataSource" Datenquelle auswählen-Assistent gibt automatisch die Methoden aus der `ProductBLL` zum Einfügen, aktualisieren und Löschen verwendet werden, damit es sei denn, Sie explizit die heraus deaktiviert haben, sie in das Markup oben aufgenommen werden müssen.


Wenn diese Seite besuchen, ruft die Daten-Websteuerelement dem ObjectDataSource-Steuerelement `Select` -Methode, die aufgerufen wird die `ProductsBLL` -Klasse `GetProductByProductID(productID)` Methode, die mit dem hartcodierten Wert 5 für die `productID` input-Parameters. Die Methode gibt einen stark typisierten `ProductDataTable` -Objekt, das eine einzelne Zeile mit Informationen über die Chef-Anton Gumbo Mix enthält (das Produkt mit `ProductID` 5).


[![Informationen zu Chef Antons Gumbo Mix werden angezeigt.](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Abbildung 5**: Informationen zu Chef Antons Gumbo Mix angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Festlegen des Parameterwerts auf den Eigenschaftswert eines Websteuerelements

Das "ObjectDataSource"-Parameter, die Werte auch festgelegt werden können, basierend auf dem Wert eines Websteuerelements auf der Seite. Um dies zu veranschaulichen, lassen Sie uns eine GridView, die alle Lieferanten auflistet, die in einem vom Benutzer angegebene Land befinden. Zum Ausführen dieser Start durch Hinzufügen eines Textfelds auf der Seite, in der der Benutzer einen Ländernamen eingeben kann. Legen Sie dieses TextBox-Steuerelements `ID` Eigenschaft `CountryName`. Fügen Sie auch ein Steuerelement Schaltfläche hinzu.


[![Fügen Sie ein Textfeld auf die Seite mit der ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Abbildung 6**: Fügen Sie ein Textfeld auf der Seite mit `ID` `CountryName` ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image18.png))


Hinzufügen einer GridView-Ansicht aus, auf der Seite und aus dem Smarttag, wählen Sie anschließend eine neue "ObjectDataSource" hinzufügen. Da wir Lieferanten Informationen wählen angezeigt werden soll die `SuppliersBLL` Klasse aus dem ersten Bildschirm des Assistenten. Wählen Sie aus dem zweiten Bildschirm der `GetSuppliersByCountry(country)` Methode.


[![Wählen Sie die GetSuppliersByCountry(country)-Methode](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Abbildung 7**: Wählen Sie die `GetSuppliersByCountry(country)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image21.png))


Da die `GetSuppliersByCountry(country)` Methode verfügt über einen Eingabeparameter, die der Assistent umfasst wiederum einen letzten Bildschirm zum Auswählen der Wert des Parameters. Legen Sie dieses Mal die Parameterquelle steuern. Dadurch wird die ControlID Dropdown-Liste mit den Namen der Steuerelemente auf der Seite ausgefüllt; Wählen Sie die `CountryName` Steuerelement aus der Liste. Wenn zuerst die Seite besucht wird die `CountryName` TextBox, leer ist, sind, so dass keine Ergebnisse zurückgegeben werden, und es wird nichts angezeigt. Wenn einige Ergebnisse werden standardmäßig angezeigt werden sollen, legen Sie das DefaultValue-Textfeld entsprechend fest.


[![Legen Sie den Parameterwert auf den Wert für die CountryName-Steuerelement](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Abbildung 8**: Legen Sie den Parameterwert auf den `CountryName` Steuerelementwert ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image24.png))


Das "ObjectDataSource" deklarative Markup unterscheidet sich geringfügig vom ersten Beispiel mit einer [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) anstelle des standardmäßigen `Parameter` Objekt. Ein `ControlParameter` verfügt über zusätzliche Eigenschaften, die an die `ID` das Websteuerelement und den Eigenschaftswert angibt, für den Parameter verwendet (`PropertyName`). Das Konfigurieren von Datenquellen-Assistent konnte intelligent genug, um zu bestimmen, dass für ein Textfeld, wir Sie verwenden möchten die `Text` -Eigenschaft für den Wert des Parameters. Wenn Sie jedoch einen anderen Eigenschaftswert aus dem Web-Steuerelement verwendet werden sollen. Sie können ändern, die `PropertyName` Wert hier oder auf den Link "Erweiterte Eigenschaften einblenden" im Assistenten.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Wenn die Seite zum ersten Mal besuchen der `CountryName` Textfeld leer ist. Das "ObjectDataSource" `Select` Methode wird von der GridView, aber der Wert immer noch aufgerufen `Nothing` übergeben wird, in der `GetSuppliersByCountry(country)` Methode. TableAdapter konvertiert die `Nothing` in einer Datenbank `NULL` Wert (`DBNull.Value`), jedoch die Abfrage ein, die die `GetSuppliersByCountry(country)` Methode ist so geschrieben, dass es Rückgabewert nicht Werte, wenn eine `NULL` Wert wird angegeben, für die `@CategoryID`Parameter. Kurz gesagt, es werden keine Lieferanten zurückgegeben.

Nachdem der Besucher in einem anderen Land, jedoch gibt ein und klickt auf die Schaltfläche anzeigen Lieferanten, um ein Postback, dem ObjectDataSource-Steuerelement die dazu führen, dass `Select` Methode erneut abgefragt, in des TextBox-Steuerelements übergeben `Text` Wert wie die `country` Parameter.


[![Die Lieferanten von Kanada werden angezeigt.](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Abbildung 9**: die Lieferanten von Kanada angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Alle Lieferanten in der Standardeinstellung angezeigt

Stattdessen als keiner der Lieferanten anzeigen, wenn Sie die Seite zuerst anzeigen sollen zeigen *alle* Lieferanten zuerst, damit der Benutzer in der Liste nach unten zu kürzen, indem Sie einen Ländernamen in das Textfeld eingeben. Wenn das Textfeld leer ist, ist die `SuppliersBLL` Klasse `GetSuppliersByCountry(country)` Methode übergeben `Nothing` für seine *`country`* Eingabeparameter. Dies `Nothing` Wert wird dann nach unten in der DAL übergeben `GetSupplierByCountry(country)` -Methode, in dem er wird in einer Datenbank konvertiert `NULL` Wert für die `@Country` Parameter in der folgenden Abfrage:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Der Ausdruck `Country = NULL` immer False zurückgegeben, auch für Datensätze, deren `Country` Spalte weist eine `NULL` Wert; aus diesem Grund werden keine Datensätze zurückgegeben.

Zurückzugebenden *alle* Lieferanten, wenn das Land Textfeld leer ist, können wir verbessern die `GetSuppliersByCountry(country)` -Methode in der die BLL zum Aufrufen der `GetSuppliers()` Methode, wenn als Land-Parameter `Nothing` und rufen Sie die DAL `GetSuppliersByCountry(country)` Methode, die andernfalls. Dadurch müssen die Auswirkungen der alle Lieferanten, wenn kein Land angegeben wird und die entsprechende Datenteilmenge Lieferanten zurückgeben, wenn der Country-Parameter enthalten ist.

Ändern der `GetSuppliersByCountry(country)` -Methode in der die `SuppliersBLL` -Klasse folgendermaßen:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Durch diese Änderung der `DeclarativeParams.aspx` Seite zeigt alle Lieferanten beim ersten Mal besucht hat (oder jedes Mal, wenn die `CountryName` Textfeld leer ist).


[![Alle Lieferanten sind jetzt standardmäßig angezeigt.](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Abbildung 10**: alle Lieferanten sind jetzt standardmäßig angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Zusammenfassung

Um Methoden mit Eingabeparametern verwenden zu können, müssen wir die Werte für die Parameter in dem ObjectDataSource-Steuerelement angeben `SelectParameters` Auflistung. Verschiedene Arten von Parametern ermöglichen den Wert des Parameters aus verschiedenen Quellen abgerufen werden sollen. Der standardmäßige Parametertyp verwendet einen hartcodierten Wert aber so einfach (und ohne eine einzige Zeile Code) Parameterwerte aus der Abfragezeichenfolge, Sitzung Variablen, Cookies und auch neben der freien Eingabe von Werten aus Web-Steuerelemente auf der Seite abgerufen werden können.

Die Beispiele, die wir uns angesehen, in diesem Tutorial haben gezeigt, wie deklarative Parameterwerte verwenden. Allerdings kann es Zeiten werden, wenn wir benötigen eine Source-Parameter verwenden, die nicht verfügbar, z. B. das aktuelle Datum und die Uhrzeit ist, oder, wenn unsere Website Mitgliedschaft, die Benutzer-ID des Besuchers verwendet wurde. Für solche Szenarien können wir die Werte der Parameter vor dem ObjectDataSource-Steuerelement programmgesteuert festlegen, das Aufrufen des zugrunde liegenden Objekts-Methode. Erfahren Sie, wie Sie dazu in der [nächsten Tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-data-with-the-objectdatasource-vb.md)
> [Weiter](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
