---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Deklarative Parameter (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm wird dies wie einen auf einen hartcodierten Wert festgelegten Parameter zur Auswahl der Daten in einem DetailsView-Steuerelement angezeigt veranschaulicht.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 7759b3d078ddabd335034f2ff76f10fb0de7dd28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="declarative-parameters-vb"></a>Deklarative Parameter (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) oder [PDF herunterladen](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> In diesem Lernprogramm wird dies wie einen auf einen hartcodierten Wert festgelegten Parameter zur Auswahl der Daten in einem DetailsView-Steuerelement angezeigt veranschaulicht.


## <a name="introduction"></a>Einführung

In der [letzten Lernprogramm](displaying-data-with-the-objectdatasource-vb.md) erläutert, Anzeigen von Daten mit den GridView, DetailsView und FormView-Steuerelementen, die an einem ObjectDataSource-Steuerelement, das aufgerufen gebunden der `GetProducts()` Methode aus der `ProductsBLL` Klasse. Die `GetProducts()` Methodenrückgabe eine stark typisierten "DataTable" mit der alle Datensätze aus der Northwind-Datenbank aufgefüllt `Products` Tabelle. Die `ProductsBLL` Klasse enthält zusätzliche Methoden für die Rückgabe einfach Teilmengen der Produkte - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, und `GetProductsBySupplierID(supplierID)`. Diese drei Methoden erwarten, dass einen Eingabeparameter, der angibt, wie die zurückgegebenen Produktinformationen gefiltert.

Das ObjectDataSource kann verwendet werden, zum Aufrufen von Methoden, die Eingabeparameter erwartet, jedoch zu diesem Zweck müssen wir angeben, woher die Werte für diese Parameter aus. Die Parameterwerte können Programmcode oder stammen aus einer Vielzahl von dynamischen Quellen, einschließlich: Querystring-Werte, Sitzungsvariablen, Wert der Eigenschaft für ein Websteuerelement auf der Seite oder andere.

Für dieses Lernprogramm beginnen wir mit zeigen, wie Sie mithilfe eines Parameters, der auf einen hartcodierten Wert festgelegt. Genauer gesagt betrachten wir eine DetailsView hinzufügen, auf der Seite mit Informationen zu einem bestimmten Produkt, nämlich Chef Anton's Gumbo Mix, besitzt eine `ProductID` von 5. Als Nächstes sehen wir wie den Parameterwert basierend auf einem Webserver-Steuerelement festlegen. Insbesondere verwenden wir ein Textfeld, damit der Benutzer, die in einem Land, geben Sie nach dem sie eine Schaltfläche, um die Liste der Lieferanten anzuzeigen, die in diesem Land befinden klicken können.

## <a name="using-a-hard-coded-parameter-value"></a>Verwenden einen hartcodierten Parameterwert

Starten Sie für das erste Beispiel durch Hinzufügen eines DetailsView-Steuerelements, zu der `DeclarativeParams.aspx` auf der Seite der `BasicReporting` Ordner. Wählen Sie in der DetailsView Smarttag &lt;neue Datenquelle&gt; aus der Dropdownliste aus, und wählen Sie ein ObjectDataSource hinzufügen.


[![Hinzufügen von einem ObjectDataSource auf der Seite "](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen von einem ObjectDataSource zur Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image3.png))


Dadurch wird automatisch das ObjectDataSource-Steuerelement-Datenquelle auswählen-Assistent gestartet. Wählen Sie die `ProductsBLL` Klasse aus dem ersten Bildschirm des Assistenten.


[![Wählen Sie die ProductsBLL-Klasse](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Abbildung 2**: Wählen Sie die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image6.png))


Da es Informationen über ein bestimmtes Produkt angezeigt werden soll, die wir verwenden möchten die `GetProductByProductID(productID)` Methode.


[![Wählen Sie die GetProductByProductID(productID)-Methode](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie die `GetProductByProductID(productID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image9.png))


Da die Methode, die wir ausgewählt einen Parameter enthält, besteht eine weitere Bildschirm für die Assistenten, in dem wir gefragt werden, definieren Sie den Wert für den Parameter verwendet werden soll. Die Liste auf der linken Seite zeigt alle Parameter für die ausgewählte Methode. Für `GetProductByProductID(productID)` ist nur ein `productID`. Auf der rechten Seite können wir den Wert für den ausgewählten Parameter angeben. Der Parameter Quelle Dropdown-Liste zählt die verschiedenen möglichen Quellen für den Parameterwert. Da wir den hartcodierten Wert 5 für angeben möchten die `productID` Parameter, lassen Sie die Parameter-Quelle als None, und geben Sie 5 in das Textfeld "DefaultValue".


[![Eine Hard-Coded Parameter-Wert von 5 wird verwendet "ProductID" und Parameter](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Abbildung 4**: A Hard-Coded Parameter Wert von 5 verwendet für die `productID` Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image12.png))


Nach Abschluss des Assistenten für die Datenquelle konfigurieren deklaratives Markup für das ObjectDataSource-Steuerelement enthält eine `Parameter` Objekt in der `SelectParameters` Auflistung aller Eingabeparametern, die von der Methode definiert, die der `SelectMethod` Diese Eigenschaft. Da die Methode, die wir in diesem Beispiel verwenden nur einen einzelnen Eingabeparameter erwartet `parameterID`, hier nur ein Eintrag vorhanden ist. Die `SelectParameters` Auflistung darf eine beliebige Klasse, abgeleitet wird, die `Parameter` -Klasse in der `System.Web.UI.WebControls` Namespace. Für hartcodierte Parameterwerte in die Basis `Parameter` Klasse wird verwendet, aber für die anderen Parameter Quelle ' Optionen ' ein abgeleiteter `Parameter` Klasse verwendet wird; Sie können auch eigene erstellen [benutzerdefinierte Parametertypen](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), sofern erforderlich.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Wenn Sie auf Ihrem eigenen Computer deklarative Markup entlang befolgen daraufhin an diesem Punkt Mai enthalten Werte für die `InsertMethod`, `UpdateMethod`, und `DeleteMethod` Eigenschaften als auch `DeleteParameters`. Assistent für das ObjectDataSource-Datenquelle auswählen gibt automatisch an die Methoden aus der `ProductBLL` zum Einfügen, aktualisieren und löschen, einsetzen, wenn Sie die out-explizit deaktiviert sie im obigen Markup enthalten sein müssen.


Wenn auf dieser Seite besuchen, rufen die Daten Websteuerelement das ObjectDataSource `Select` -Methode, die aufgerufen wird die `ProductsBLL` Klasse `GetProductByProductID(productID)` Methode, die mit den hartcodierten Wert von 5 für die `productID` Eingabeparameter. Die Methode gibt einen stark typisierten zurück `ProductDataTable` Objekt, das eine einzelne Zeile mit Informationen zu Chef Anton's Gumbo Mix enthält (das Produkt mit `ProductID` 5).


[![Informationen zu Chef Anton's Gumbo Mix werden angezeigt.](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Abbildung 5**: Es werden Informationen zu Chef Anton's Gumbo Mix angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Festlegen des Parameterwerts an den Eigenschaftswert eines Steuerelements Web

Das ObjectDataSource-Parameter können auch Werte festgelegt werden, basierend auf dem Wert eines Webserver-Steuerelements auf der Seite. Um dies zu veranschaulichen, lassen Sie uns eine GridView, die alle Lieferanten auflistet, die in einem vom Benutzer angegebene Land befinden. Um ein Textfeld hinzufügen, auf die Seite, in der der Benutzer einen Ländernamen eingeben kann, um starten zu erreichen. Legen Sie diese TextBox-Steuerelement `ID` Eigenschaft `CountryName`. Fügen Sie auch eine Schaltfläche Websteuerelement hinzu.


[![Fügen Sie ein Textfeld auf der Seite mit der ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Abbildung 6**: Fügen Sie ein Textfeld auf der Seite mit `ID` `CountryName` ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image18.png))


Als Nächstes fügen Sie eine GridView, zur Seite und aus dem Smarttag Sie wahlweise eine neue ObjectDataSource hinzufügen. Da wir Lieferanten Informationen auswählen angezeigt werden soll die `SuppliersBLL` Klasse aus dem ersten Bildschirm des Assistenten. Wählen Sie aus dem zweiten Bildschirm die `GetSuppliersByCountry(country)` Methode.


[![Wählen Sie die GetSuppliersByCountry(country)-Methode](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Abbildung 7**: Wählen Sie die `GetSuppliersByCountry(country)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image21.png))


Da die `GetSuppliersByCountry(country)` Methode verfügt über einen Eingabeparameter, der Assistenten erneut einen abschließenden Bildschirm zum Auswählen der Wert des Parameters enthält. Legen Sie dieses Mal die Parameterquelle steuern. Füllt diese mit den Namen der Steuerelemente auf der Seite in der Dropdownliste ControlID; Wählen Sie die `CountryName` Steuerelement aus der Liste. Wenn die Seite zuerst besucht die `CountryName` Textfeld wird leer sein, damit keine Ergebnisse zurückgegeben werden, und es wird nichts angezeigt. Wenn einige Ergebnisse standardmäßig angezeigt werden sollen, wird das Textfeld "DefaultValue" entsprechend festgelegt.


[![Der Wert des Parameters auf den Steuerelementwert CountryName festgelegt](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Abbildung 8**: Legen Sie den Parameterwert auf den `CountryName` Steuerelementwert ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image24.png))


Das ObjectDataSource deklarative Markup unterscheidet sich geringfügig vom ersten Beispiel mit einer [ControlParameter](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.controlparameter.aspx) anstelle des `Parameter` Objekt. Ein `ControlParameter` verfügt über zusätzliche Eigenschaften zur Angabe der `ID` das Websteuerelement und den Eigenschaftswert angibt, für den Parameter verwendet (`PropertyName`). Konfigurieren von Datenquellen-Assistenten wurde intelligent genug, um zu ermitteln, die für ein Textfeld wird wahrscheinlich verwenden möchten die `Text` Eigenschaft für den Parameterwert. Wenn Sie jedoch einen anderen Eigenschaftswert aus dem Web-Steuerelement verwenden möchten können Sie ändern die `PropertyName` Wert hier oder indem Sie auf den Link "Erweiterte Eigenschaften einblenden" im Assistenten.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Wenn Sie die Seite zum ersten Mal besuchen der `CountryName` Textfeld ist leer. Das ObjectDataSource `Select` Methode wird immer noch aufgerufen, indem die GridView, aber der Wert `Nothing` übergeben wird, in der `GetSuppliersByCountry(country)` Methode. TableAdapter konvertiert der `Nothing` in einer Datenbank `NULL` Wert (`DBNull.Value`), aber die Abfrage verwendet werden, indem Sie die `GetSuppliersByCountry(country)` Methode geschrieben wird, dass alle keinen zurückgibt Werte an, wenn eine `NULL` Wert für die angegebenist`@CategoryID`Parameter. Kurz gesagt, werden keine Lieferanten zurückgegeben.

Sobald der Besucher jedoch in einem Land eingibt und klickt auf die Schaltfläche anzeigen Lieferanten, um dazu führen, dass ein Postback handeln, das ObjectDataSource `Select` Methode erneut abgefragt, in des TextBox-Steuerelements übergeben `Text` als Wert der `country` Parameter.


[![Diese Lieferanten aus Kanada werden angezeigt.](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Abbildung 9**: die Lieferanten aus Kanada angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Alle Lieferanten in der Standardeinstellung angezeigt

Stattdessen als keines der Lieferanten anzeigen, wenn die Seite zuerst anzeigen wir möchten möglicherweise *alle* Lieferanten zunächst, sodass der Benutzer zu der Liste nach unten zu kürzen, einen Landesnamen in das Textfeld eingeben. Wenn das Textfeld leer ist, wird die `SuppliersBLL` Klasse `GetSuppliersByCountry(country)` Methode übergeben `Nothing` für seine  *`country`*  Eingabeparameter. Dies `Nothing` Wert wird dann nach unten in der DAL übergeben `GetSupplierByCountry(country)` -Methode, in dem er wird in einer Datenbank übersetzt `NULL` Wert für die `@Country` Parameter in der folgenden Abfrage:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Der Ausdruck `Country = NULL` immer False zurückgibt, selbst für Datensätze, deren `Country` Spalte hat einen `NULL` Wert; deshalb keine Datensätze zurückgegeben werden.

Zurückzugebenden *alle* Lieferanten, wenn das Land Textfeld leer ist, können wir ergänzen die `GetSuppliersByCountry(country)` Methode in der BLL zum Aufrufen der `GetSuppliers()` Methode, wenn der Country-Parameter ist `Nothing` und zum Aufrufen der DAL `GetSuppliersByCountry(country)` Methode, die andernfalls. Dies hat den Effekt der alle Lieferanten, wenn keine Land angegeben wird und die entsprechenden Teilmenge der Lieferanten zurückgeben, wenn die Country-Parameter enthalten ist.

Ändern der `GetSuppliersByCountry(country)` Methode in der `SuppliersBLL` Klasse, um die folgenden:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Durch diese Änderung der `DeclarativeParams.aspx` Seite zeigt alle Lieferanten beim ersten Mal besucht hat (oder wenn die `CountryName` Textfeld leer ist).


[![Alle Lieferanten sind jetzt standardmäßig angezeigt.](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Abbildung 10**: alle Lieferanten sind jetzt standardmäßig angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Zusammenfassung

Um Methoden mit Eingabeparametern verwenden zu können, müssen wir die Werte für die Parameter in der ObjectDataSource anzugeben `SelectParameters` Auflistung. Verschiedene Typen von Parametern können für den Parameterwert aus verschiedenen Quellen abgerufen werden soll. Der standardmäßige Parametertyp verwendet einen hartcodierten Wert, aber wie leicht (und ohne eine Codezeile) Parameterwerte aus den Querystring, Session-Variablen, Cookies und sogar Benutzereingaben Werten Websteuerelemente auf der Seite abgerufen werden können.

Die Beispiele, die wir unter in diesem Lernprogramm wurde gezeigt, wie deklarative Parameterwerte verwenden. Allerdings kann es Zeiten sein, wenn wir müssen einen Parameterquelle verwenden, nicht verfügbar, z. B. das aktuelle Datum und die Uhrzeit ist, oder, wenn unsere Website Mitgliedschaft, die Benutzer-ID des Besuchers verwendet wurde. Für solche Szenarien können wir die Parameterwerte programmgesteuert vor dem ObjectDataSource festlegen, Aufrufen der Methode für die zugrunde liegende Objekt. Wir sehen, wie Sie dazu in der [nächsten Lernprogramm](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](displaying-data-with-the-objectdatasource-vb.md)
[Weiter](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
