---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Anzeigen von Datenelementen und Details | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 2184c04d5f2361526be0409178dc0a6c665ebc4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833316"
---
<a name="display-data-items-and-details"></a>Anzeigen von Datenelementen und Details
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


Dieses Tutorial beschreibt die Vorgehensweise beim Anzeigen von Datenelementen und Details von Daten mithilfe von ASP.NET Web Forms und Entity Framework Code First. Dieses Tutorial baut auf dem vorherigen Lernprogramm "Und Navigation in der Benutzeroberfläche" und ist Teil der tutorialreihe Wingtip Toys-Store. Wenn Sie dieses Tutorial abgeschlossen haben, werden Sie auf Produkte angezeigt werden die *ProductsList.aspx* Seite und die Details dazu, ein einzelnes Produkt auf der *ProductDetails.aspx* Seite.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- So fügen Sie ein Steuerelement zum Anzeigen von Produkten aus der Datenbank.
- So verbinden Sie ein Steuerelement in den ausgewählten Datentyp werden.
- So fügen Sie ein Steuerelement zum Anzeigen von Produktdetails aus der Datenbank.
- Informationen zum Abrufen eines Werts aus der Abfragezeichenfolge und diesen Wert verwenden, um die Daten einzuschränken, die aus der Datenbank abgerufen werden.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Dies sind die Funktionen, die in diesem Lernprogramm eingeführt:

- Modellbindung
- Wertanbieter

## <a name="adding-a-data-control-to-display-products"></a>Ein Steuerelement zum Anzeigen von Produkten hinzufügen

Beim Binden von Daten an ein Serversteuerelement, gibt es eine Reihe von Optionen, die Sie verwenden können. Die am häufigsten verwendeten Optionen umfassen ein Datenquellen-Steuerelement hinzufügen, Hinzufügen von Code von hand oder mit der modellbindung.

### <a name="using-a-data-source-control-to-bind-data"></a>Verwenden ein Datenquellen-Steuerelement zum Binden von Daten

Ein Datenquellen-Steuerelement hinzufügen, können Sie die Datenquellen-Steuerelement mit dem Steuerelement verknüpfen, das Daten anzeigt. Dieser Ansatz ermöglicht Ihnen, deklarativ serverseitige Steuerelemente direkt mit Datenquellen, anstatt eine programmgesteuerte Vorgehensweise verbunden.

### <a name="coding-by-hand-to-bind-data"></a>Codieren von Hand zum Binden von Daten

Hinzufügen, dass der Code von Hand umfasst Lesen des Werts, einen null-Wert gesucht, es wird versucht, die sie in den entsprechenden Typ zu konvertieren, überprüfen, ob die Konvertierung erfolgreich war und schließlich den Wert in der Abfrage verwenden. Wenn Sie vollständige Kontrolle über Ihre Datenzugriffslogik beibehalten möchten, würden Sie diesen Ansatz verwenden.

### <a name="using-model-binding-to-bind-data"></a>Verwenden von Modellbindung, die zum Binden von Daten

Mit der modellbindung ermöglicht Ihnen, binden Ergebnisse mit sehr viel weniger Code und gibt Ihnen die Möglichkeit, die die Funktionalität in der gesamten Anwendung wiederverwenden. Die modellbindung zielt darauf ab, auf die Arbeit mit Code fokussierter Datenzugriffslogik zu vereinfachen, ohne dabei die Vorteile eines Frameworks für umfangreiche und Datenbindung.

## <a name="displaying-products"></a>Produkte anzeigen

In diesem Tutorial verwenden Sie modellbindung zum Binden von Daten. Zum Konfigurieren eines Datensteuerelements, um die modellbindung zu verwenden, um Daten auszuwählen, die Sie festlegen des Steuerelements `SelectMethod` -Eigenschaft auf den Namen einer Methode im Code der Seite. Das Steuerelement ruft die Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite und die zurückgegebenen Daten automatisch gebunden. Es ist nicht erforderlich, explizit aufrufen, die `DataBind` Methode.

Verwenden die folgenden Schritte aus, ändern Sie das Markup in der *"ProductList.aspx"* Seite, damit die Seite Produkte angezeigt werden kann.

1. In **Projektmappen-Explorer**öffnen die *"ProductList.aspx"* Seite.
2. Ersetzen Sie das vorhandene Markup durch das folgende Markup:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Dieser Code verwendet ein **ListView** Steuerelement mit dem Namen "ProductList" für die Produkte.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

Die **ListView** -Steuerelement zeigt Daten in ein Format, das Sie mithilfe von Vorlagen und Stile definieren. Es ist nützlich für Daten in einer sich wiederholenden Struktur. Dies **ListView** Beispiel zeigt einfach Daten aus der Datenbank, aber Sie können Benutzer zu bearbeiten, einfügen und Löschen von Daten und zum Sortieren und Paging der Daten, ganz ohne Programmieraufwand.

Durch Festlegen der `ItemType` -Eigenschaft in der **ListView** zu steuern, die XPath-Datenbindungsausdruck `Item` verfügbar ist und das Steuerelement wird stark typisiert. Wie im vorherigen Tutorial erwähnt, können Sie auswählen, dass das Objekt mithilfe von IntelliSense, z. B. Angeben von Details der `ProductName`:

![Anzeigen von Daten Elementen und Details - IntelliSense](display_data_items_and_details/_static/image1.png)

Sie werden darüber hinaus wurden die modellbindung verwenden, an eine `SelectMethod` Wert. Dieser Wert (`GetProducts`) entsprechen die Methode, die Sie mit der CodeBehind-zum Anzeigen von Produkten im nächsten Schritt hinzufügen.

### <a name="adding-code-to-display-products"></a>Hinzufügen von Code zum Anzeigen von Produkten

In diesem Schritt fügen Sie Code zum Auffüllen der **ListView** -Steuerelement mit Produktdaten aus der Datenbank. Der Code wird mit Produkten durch einzelne Kategorie sowie alle Produkte unterstützen.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste *"ProductList.aspx"* , und klicken Sie dann auf **Ansichtscode**.
2. Ersetzen Sie den vorhandenen Code in die *ProductList.aspx.cs* -Datei mit den folgenden Code:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Dieser Code zeigt die `GetProducts` -Methode, die vom verwiesen wird die `ItemType` Eigenschaft der **ListView** steuern, der *"ProductList.aspx"* Seite. Um die Ergebnisse in einer bestimmten Kategorie in der Datenbank zu beschränken, mit dem Code wird die `categoryId` Wert aus der Abfragezeichenfolgenwert übergeben, um die *"ProductList.aspx"* Seite, wenn die *"ProductList.aspx"* Seite ist navigiert. Die `QueryStringAttribute` -Klasse in der `System.Web.ModelBinding` Namespace wird verwendet, um den Wert der Variable Abfrage Zeichenfolgen-Id abzurufen. Dies weist die modellbindung, um zu versuchen, einen Wert aus der Abfragezeichenfolge zum Binden der `categoryId` Parameter zur Laufzeit.

Wenn Sie eine gültige Kategorie an der Seite "als Abfragezeichenfolge übergeben wird, sind die Ergebnisse der Abfrage auf diese Produkte in der Datenbank, die mit übereinstimmen beschränkt die `categoryId` Wert. Z. B. wenn die URL, die *ProductsList.aspx* Seite lautet wie folgt:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Die Seite zeigt nur die Produkte, in denen die `category` gleich `1`.

Wenn keine Abfragezeichenfolge enthalten ist, bei der Navigation die *"ProductList.aspx"* Seite werden alle Produkte angezeigt.

Die Quellen der Werte für diese Methoden werden als bezeichnet *Wert Anbieter* (z. B. *QueryString*), und die Parameterattribute, die angeben, welcher Wertanbieter verwendet werden als Wert Anbieterattribute (z. B. "`id`"). ASP.NET umfasst Wertanbieter und die zugehörigen Attribute für alle typischen Quellen von Benutzereingaben in einer Web Forms-Anwendung, z. B. der Abfragezeichenfolgen, Cookies, Formularwerte, Steuerelemente, Ansichtszustand, Sitzungsstatus und Profileigenschaften. Sie können auch benutzerdefinierte Wertanbieter erstellen.

### <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung jetzt, um festzustellen, wie Sie alle Produkte oder nur eine Reihe von Produkten nach Kategorie begrenzt anzeigen können.

1. In der **Projektmappen-Explorer**, mit der rechten Maustaste die *"default.aspx"* Seite und wählen Sie **in Browser anzeigen**.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Wählen Sie **Autos** im Navigationsmenü Product Category.  
 Die *"ProductList.aspx"* Seite wird angezeigt, die nur die Produkte in der Kategorie "Cars" enthalten. Weiter unten in diesem Tutorial zeigen Sie Produktdetails.  

    ![Anzeigen von Daten Elementen und Details - Autos](display_data_items_and_details/_static/image2.png)
3. Wählen Sie **Produkte** oben im Navigationsmenü aus.  
 In diesem Fall die *"ProductList.aspx"* Seite wird angezeigt, aber dieses Mal es die gesamte Liste der Produkte zeigt.   

    ![Anzeigen von Daten Elementen und Details - Produkte](display_data_items_and_details/_static/image3.png)
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

### <a name="adding-a-data-control-to-display-product-details"></a>Ein Steuerelement zum Anzeigen von Produktdetails hinzufügen

Als Nächstes ändern Sie das Markup in der *ProductDetails.aspx* Seite, die Sie im vorherigen Tutorial hinzugefügt, damit die Seite mit Informationen über ein einzelnes Produkt anzeigen kann.

1. In **Projektmappen-Explorer**öffnen die *ProductDetails.aspx* Seite.
2. Ersetzen Sie das vorhandene Markup durch das folgende Markup:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Dieser Code verwendet ein **FormView** -Steuerelement zum Anzeigen der Details eines einzelnen Produkts. Dieses Markup verwendet Methoden wie jene, die verwendet werden, zum Anzeigen von Daten in die *"ProductList.aspx"* Seite. Die **FormView** Steuerelement wird verwendet, um ein einzelner Datensatz aus einer Datenquelle zu einem Zeitpunkt angezeigt. Bei Verwendung der **FormView** -Steuerelement, erstellen Sie Vorlagen, um das Anzeigen und Bearbeiten von datengebundenen Werten. Die Vorlagen enthalten die Steuerelemente, Bindungsausdrücke und Formatierungen, die das Aussehen und die Funktionalität des Formulars zu definieren.

Um das obenstehende Markup in der Datenbank zu verbinden, müssen Sie zusätzlichen Code zum Hinzufügen der *ProductDetails.aspx* Code.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste *ProductDetails.aspx* , und klicken Sie dann auf **Ansichtscode**.  
   Die *ProductDetails.aspx.cs* Datei angezeigt.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Dieser Code überprüft wird, für eine "`productID`" Query-String-Wert. Wenn Sie ein gültigen Abfragezeichenfolgen-Wert gefunden wird, wird das entsprechende Produkt angezeigt. Wenn keine Abfragezeichenfolge gefunden wird, oder die Abfragezeichenfolgen-Wert ungültig ist, wird kein Produkt angezeigt, auf die *ProductDetails.aspx* Seite.

### <a name="running-the-application"></a>Ausführen der Anwendung

Jetzt können Sie die Anwendung aus, um ein einzelnes Produkt angezeigt sehen ausführen Basis auf der Id des Produkts.

1. Drücken Sie **F5** während Sie sich in Visual Studio, um die Anwendung auszuführen.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Wählen Sie im Navigationsmenü der Kategorie "Boote" ein.  
 Die *"ProductList.aspx"* angezeigt wird.
3. Wählen Sie das Produkt "Papier Schiff", aus der Liste der Produkte.  
 Die *ProductDetails.aspx* angezeigt wird.   

    ![Anzeigen von Daten Elementen und Details - Produkte](display_data_items_and_details/_static/image4.png)
4. Schließen Sie den Browser.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial der Reihe haben Sie hinzufügen Markup und Code zum Anzeigen einer Produktliste und Produktdetails anzuzeigen. Während dieses Prozesses haben Sie über stark typisierte Datensteuerelemente, modellbindung und Wertanbieter. Im nächsten Tutorial fügen Sie einen Einkaufswagen gelegt hat der Wingtip Toys-beispielanwendung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Abrufen und Anzeigen von Daten mit modellbindung und Web forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Zurück](ui_and_navigation.md)
> [Weiter](shopping-cart.md)
