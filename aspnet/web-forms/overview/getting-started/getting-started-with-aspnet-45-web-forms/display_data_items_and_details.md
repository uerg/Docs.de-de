---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Anzeigen von Daten, Elemente und Details | Microsoft Docs
author: Erikre
description: "Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 809d7a9c21a3ddf5dfd07d079eb8fe0d1d81712d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="display-data-items-and-details"></a>Anzeigen von Daten, Elemente und Details
====================
Durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


Diesem Lernprogramm wird beschrieben, wie Datenelemente und mithilfe von ASP.NET Web Forms- und Entity Framework Code First Datenelement-Details angezeigt. Dieses Lernprogramm baut auf dem vorherigen Lernprogramm "Und Navigation in der Benutzeroberfläche" und ist Teil der Wingtip Toys Store Reihe von Lernprogrammen. Wenn Sie dieses Lernprogramm abgeschlossen haben, müssen Sie für Produkte angezeigt werden die *ProductsList.aspx* Seiten- und Details für ein einzelnes Produkt auf der *ProductDetails.aspx* Seite.

## <a name="what-youll-learn"></a>Lernen Sie:

- So fügen Sie ein Steuerelement zum Anzeigen von Produkten aus der Datenbank.
- Herstellen der Verbindung ein Steuerelement in den ausgewählten Datentyp.
- So fügen Sie ein Steuerelement zur Anzeige von Produktdetails aus der Datenbank.
- Informationen zum Abrufen eines Werts aus der Abfragezeichenfolge und diesen Wert verwenden, um die Daten einzuschränken, die aus der Datenbank abgerufen werden.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Dies sind die Funktionen, die im Lernprogramm eingeführt:

- Wurden die Modellbindung
- Wertanbieter

## <a name="adding-a-data-control-to-display-products"></a>Hinzufügen eines Datensteuerelements zum Anzeigen von Produkten

Beim Binden von Daten an ein Serversteuerelement, gibt es verschiedene Möglichkeiten, die Sie verwenden können. Die am häufigsten verwendeten Optionen enthalten ein Datenquellen-Steuerelement hinzufügen, durch manuelles Hinzufügen von Code oder mithilfe der modellbindung.

### <a name="using-a-data-source-control-to-bind-data"></a>Verwenden ein Datenquellen-Steuerelement zum Binden von Daten

Ein Datenquellen-Steuerelement hinzufügen, können zu den Datenquellen-Steuerelement an das Steuerelement zu verknüpfen, die die Daten anzeigt. Dieser Ansatz bietet die Möglichkeit, serverseitige Steuerelemente direkt mit Datenquellen, statt eine programmgesteuerte Vorgehensweise deklarativ zu verbinden.

### <a name="coding-by-hand-to-bind-data"></a>Schreiben von Code manuell zum Binden von Daten

Code manuell umfasst das Hinzufügen Lesen des Werts, Überprüfung auf einen null-Wert, bei dem Versuch, die sie in den entsprechenden Typ zu konvertieren, überprüfen, ob die Konvertierung erfolgreich war und schließlich den Wert in der Abfrage verwenden. Wenn Sie vollständige Kontrolle über Ihre Datenzugriffslogik beibehalten müssen, würden Sie diesen Ansatz verwenden.

### <a name="using-model-binding-to-bind-data"></a>Mithilfe der Modellbindung, die zum Binden von Daten

Wurden die modellbindung verwenden, können Sie Ergebnisse mit viel weniger Code binden und bietet Ihnen die Möglichkeit, die Funktionalität in der gesamten Anwendung wiederzuverwenden. Modell Bindung zielt darauf ab, um Arbeiten mit codeorientierten Datenzugriffs-Logik zu vereinfachen und gleichzeitig die Vorteile der ein umfassendes, Datenbindung Framework beibehalten.

## <a name="displaying-products"></a>Anzeigen von Produkten

In diesem Lernprogramm verwenden Sie wurden die modellbindung zum Binden von Daten. So konfigurieren ein Steuerelement zum modellbindung verwenden, um Daten auszuwählen, legen Sie des Steuerelements `SelectMethod` -Eigenschaft auf den Namen einer Methode im Code der Seite. Das Steuerelement ruft die Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite und die zurückgegebenen Daten automatisch gebunden. Es ist nicht erforderlich, explizit aufrufen, die `DataBind` Methode.

Verwenden die folgenden Schritte aus, müssen Sie das Markup im Ändern der *"ProductList.aspx"* Seite, damit die Seite Produkte angezeigt werden kann.

1. In **Projektmappen-Explorer**öffnen die *"ProductList.aspx"* Seite.
2. Ersetzen Sie das vorhandene Markup durch Folgendes Markup:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Dieser Code verwendet eine **ListView** -Steuerelement mit dem Namen "ProductList", um die Produkte anzuzeigen.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

Die **ListView** Steuerelement zeigt Daten in einem Format, das Sie mithilfe von Vorlagen und Formatvorlagen definieren. Es ist nützlich für Daten in einer sich wiederholenden Struktur. Dies **ListView** Beispiel einfach zeigt Daten aus der Datenbank, jedoch ermöglichen Benutzern bearbeiten, einfügen und Löschen von Daten, und Sortieren und Daten der Seite, ohne Code zu.

Durch Festlegen der `ItemType` Eigenschaft in der **ListView** zu steuern, die Datenbindungsausdruck `Item` verfügbar ist und das Steuerelement wird stark typisiert. Wie im vorherigen Lernprogramm erwähnt, können Sie auswählen, Details des Item-Objekts unter Verwendung von IntelliSense, z. B. Angeben der `ProductName`:

![Anzeigen von Daten Elemente und Details – IntelliSense](display_data_items_and_details/_static/image1.png)

Sie sind darüber hinaus wurden die modellbindung verwenden, um Geben Sie einen `SelectMethod` Wert. Dieser Wert (`GetProducts`) wird an die Methode, die Sie auf der Code-behind, zum Anzeigen von Produkten im nächsten Schritt hinzugefügt wird entsprechen.

### <a name="adding-code-to-display-products"></a>Hinzufügen von Code zum Anzeigen von Produkten

In diesem Schritt fügen Sie Code zum Auffüllen der **ListView** -Steuerelement mit Produktdaten aus der Datenbank. Der Code unterstützt zeigt Produkte nach einzelnen Kategorie sowie alle Produkte.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste *"ProductList.aspx"* , und klicken Sie dann auf **Code anzeigen**.
2. Ersetzen Sie den vorhandenen Code in der *ProductList.aspx.cs* Datei durch den folgenden Code:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Dieser Code zeigt, die `GetProducts` -Methode, die vom verwiesen wird die `ItemType` Eigenschaft von der **ListView** steuern, der *"ProductList.aspx"* Seite. Um die Ergebnisse einer bestimmten Kategorie in der Datenbank zu beschränken, die mit dem Code wird die `categoryId` Wert aus dem Wert der Abfragezeichenfolge übergeben, um die *"ProductList.aspx"* Seite, wenn die *"ProductList.aspx"* Seite ist navigiert. Die `QueryStringAttribute` -Klasse in der `System.Web.ModelBinding` Namespace wird verwendet, um den Wert der Variable Abfrage Zeichenfolgen-Id abzurufen. Dies weist wurden die modellbindung zu dem Versuch, einen Wert aus der Abfragezeichenfolge zum Binden der `categoryId` Parameter zur Laufzeit.

Wenn Sie eine gültige Kategorie auf der Seite "als eine Abfragezeichenfolge übergeben wird, sind die Ergebnisse der Abfrage auf diese Produkte in der Datenbank, die mit übereinstimmen beschränkt die `categoryId` Wert. Für die Instanz, wenn die URL für die *ProductsList.aspx* Seite lautet wie folgt:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Die Seite zeigt nur die Produkte, in denen die `category` gleich `1`.

Wenn keine Abfragezeichenfolge enthalten, beim Navigieren ist zu den *"ProductList.aspx"* Seite werden alle Produkte angezeigt werden.

Die Quelle der Werte für diese Methoden werden als bezeichnet *Wert Anbieter* (z. B. *QueryString*), und die Parameterattribute, die angeben, welche Wertanbieter zu verwenden, werden als Wert bezeichnet Anbieterattribute (z. B. "`id`"). ASP.NET umfasst Wertanbieter und die entsprechenden Attribute für alle typischen Quellen von Benutzereingaben in einer Web Forms-Anwendung, z. B. die Abfragezeichenfolge, die Cookies, Formularwerte, Steuerelemente, Ansichtszustand, Sitzungsstatus und Profileigenschaften an. Sie können auch benutzerdefinierte Wertanbieter schreiben.

### <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung jetzt sehen, wie Sie alle Produkte oder nur einen Satz von Produkten nach Kategorie beschränkt anzeigen können.

1. In der **Projektmappen-Explorer**, mit der rechten Maustaste die *"default.aspx"* Seite und wählen Sie **in Browser anzeigen**.  
 Der Browser öffnen und Anzeigen der *"default.aspx"* Seite.
2. Wählen Sie **Autos** im Navigationsmenü Product Category.  
 Die *"ProductList.aspx"* Seite wird angezeigt, der nur die Produkte, die in der Kategorie "Cars" enthalten. Weiter unten in diesem Lernprogramm werden Sie Produktdetails anzuzeigen.  

    ![Anzeigen von Daten Elemente und Details – Autos](display_data_items_and_details/_static/image2.png)
3. Wählen Sie **Produkte** aus dem Navigationsmenü oben.  
 In diesem Fall die *"ProductList.aspx"* Seite wird angezeigt, aber diesmal es die gesamte Liste der Produkte zeigt.   

    ![Anzeigen von Daten Elementen und Details - Produkte](display_data_items_and_details/_static/image3.png)
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

### <a name="adding-a-data-control-to-display-product-details"></a>Hinzufügen eines Datensteuerelements, um Produktdetails anzuzeigen.

Als Nächstes ändern Sie das Markup in der *ProductDetails.aspx* Seite, die Sie im vorherigen Lernprogramm hinzugefügt, damit die Seite mit Informationen über ein einzelnes Produkt anzeigen kann.

1. In **Projektmappen-Explorer**öffnen die *ProductDetails.aspx* Seite.
2. Ersetzen Sie das vorhandene Markup durch Folgendes Markup:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Dieser Code verwendet eine **FormView** -Steuerelement zum Anzeigen von Details eines einzelnen Produkts. Dieses Markup verwendet Methoden wie z. B. solche, die verwendet werden, zum Anzeigen von Daten in der *"ProductList.aspx"* Seite. Die **FormView** Steuerelement wird verwendet, um die Anzeige eines einzelnen Datensatzes zu einem Zeitpunkt aus einer Datenquelle. Bei Verwendung der **FormView** -Steuerelement können Sie Vorlagen zum Anzeigen und Bearbeiten von datengebundenen Werte erstellen. Die Vorlagen enthalten Steuerelemente, Bindungsausdrücke und Formatierung, die das Erscheinungsbild und die Funktionalität des Formulars zu definieren.

Um die oben genannten Markup in der Datenbank zu verbinden, müssen Sie zusätzlichen Code zum Hinzufügen der *ProductDetails.aspx* Code.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste *ProductDetails.aspx* , und klicken Sie dann auf **Code anzeigen**.  
 Die *ProductDetails.aspx.cs* Datei angezeigt.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Dieser Code sucht eine "`productID`" Abfragezeichenfolgen-Wert. Wenn Sie ein gültigen Abfragezeichenfolgen-Wert gefunden wird, wird das entsprechende Produkt angezeigt. Wenn keine Abfragezeichenfolge wurde gefunden, oder der Abfragezeichenfolgen-Wert ungültig ist, wird kein Produkt angezeigt, auf die *ProductDetails.aspx* Seite.

### <a name="running-the-application"></a>Ausführen der Anwendung

Jetzt können Sie die Anwendung für ein einzelnes Produkt angezeigt finden Sie unter Ausführen basierend auf der Produkt-Id ein.

1. Drücken Sie **F5** während Sie sich in Visual Studio, um die Anwendung auszuführen.  
 Der Browser öffnen und Anzeigen der *"default.aspx"* Seite.
2. Wählen Sie "Booten" im Navigationsmenü Kategorie an.  
 Die *"ProductList.aspx"* angezeigt wird.
3. Wählen Sie das Produkt "Papier Boot" aus der Produktliste aus.  
 Die *ProductDetails.aspx* Seite wird angezeigt.   

    ![Anzeigen von Daten Elementen und Details - Produkte](display_data_items_and_details/_static/image4.png)
4. Schließen Sie den Browser.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm der Reihe Sie haben Markup und Code hinzufügen, um eine Produktliste anzuzeigen und um Produktdetails anzuzeigen. Während dieses Vorgangs müssen Sie zu stark typisierten Datensteuerelementen, wurden die modellbindung und Wertanbieter gelernt. In den nächsten Lernprogrammen fügen Sie einen Einkaufswagen mit der beispielanwendung des Wingtip Toys.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Abrufen und Anzeigen von Daten mit modellbindung und WebForms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

>[!div class="step-by-step"]
[Zurück](ui_and_navigation.md)
[Weiter](shopping-cart.md)
