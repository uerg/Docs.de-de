---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Einkaufswagen | Microsoft Docs
author: Erikre
description: "Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 5c0e16df7d60b944c96f8d5510225fff321124d1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="shopping-cart"></a>Einkaufswagen
====================
Durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm wird beschrieben, die Geschäftslogik erforderlich, um den Einkaufswagen legen die Wingtip Toys Beispiel ASP.NET Web Forms-Anwendung hinzufügen. Dieses Lernprogramm baut auf dem vorherigen Lernprogramm "Anzeige Daten Elemente und Details" und ist Teil der Wingtip Toys Store Reihe von Lernprogrammen. Wenn Sie dieses Lernprogramm abgeschlossen haben, werden die Benutzern der Beispiel-app werden hinzufügen, entfernen und ändern die Produkte im Einkaufswagen.

## <a name="what-youll-learn"></a>Lernen Sie:

1. Vorgehensweise: Erstellen von einem Einkaufswagen für die Webanwendung.
2. Zum Aktivieren der Benutzer, dem Warenkorb Elemente hinzugefügt werden.
3. Gewusst wie: Hinzufügen einer [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) -Steuerelement zum Anzeigen von shopping Cart-Details.
4. Informationen zum Berechnen und die Gesamtsumme der Bestellungen anzuzeigen.
5. Vorgehensweise beim Entfernen und Aktualisieren von Elementen in den Einkaufswagen Sinn macht.
6. Wie einen shopping Cart Leistungsindikator eingeschlossen.

## <a name="code-features-in-this-tutorial"></a>Codefunktionen in diesem Lernprogramm:

1. Entity Framework Code First
2. Datenanmerkungen
3. Stark typisierte Datensteuerelemente
4. Wurden die modellbindung

## <a name="creating-a-shopping-cart"></a>Erstellen den Einkaufswagen legen

Weiter oben in dieser Reihe von Lernprogrammen haben Sie Seiten und Code zum Anzeigen von Produktdaten aus einer Datenbank hinzugefügt. In diesem Lernprogramm erstellen Sie einen Einkaufswagen, um die Produkte zu verwalten, dass Benutzer kaufen interessiert sind. Benutzer werden in der Lage zu durchsuchen und dem Warenkorb Elemente hinzufügen, selbst wenn sie nicht registriert oder angemeldet sind. Um shopping Cart-Zugriff zu verwalten, weisen Sie Benutzern eine eindeutige `ID` zum ersten Mal Einkaufswagen einen global eindeutigen Bezeichner (GUID) verwenden, wenn der Benutzer den Warenkorb zugreift. Sie müssen dies speichern `ID` mithilfe des ASP.NET Sitzungsstatus.

> [!NOTE] 
> 
> Der Sitzungsstatus für ASP.NET ist eine bequeme Möglichkeit, Sie benutzerspezifische Informationen speichern, die ablaufen soll, nachdem der Benutzer die Website verlassen hat. Missbrauch des Sitzungsstatus Leistungseinbußen an größeren Standorten kann zwar aufweisen, light-Sitzung Zustand funktioniert ebenso gut für Demonstrationszwecke verwenden. Das Beispielprojekt des Wingtip Toys zeigt, wie Sitzungszustand ohne einen externen Anbieter verwenden, in dem Status der Sitzung gespeicherten in-Process auf dem Webserver, der die Standortdatenbank gehostet wird. Bei größeren Sites, die mehrere Instanzen einer Anwendung bereitstellen oder für Standorte, die mehrere Instanzen einer Anwendung auf verschiedenen Servern ausführen, sollten Sie **Windows Azure Cache Service**. Dieser Cache-Dienst bietet einen verteilten Cachedienst, der außerhalb der Website, und löst das Problem der Verwendung von in-Process-Sitzungszustand. Weitere Informationen finden Sie unter [wie ASP.NET-Sitzungsstatus mit Windows Azure-Websites verwenden](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Fügen Sie CartItem als eine Modellklasse hinzu.

Weiter oben in diesem Lernprogramm Reihe Sie das Schema für die Kategorie- und Daten definiert, durch das Erstellen der `Category` und `Product` Klassen in der *Modelle* Ordner. Nun fügen Sie eine neue Klasse zum Definieren des Schemas für den Einkaufswagen Sinn macht. Weiter unten in diesem Lernprogramm fügen Sie eine Klasse, um den Datenzugriff auf behandeln die `CartItem` Tabelle. Diese Klasse bietet die Geschäftslogik zum Hinzufügen, entfernen und Aktualisieren von Elementen in den Einkaufswagen Sinn macht.

1. Mit der rechten Maustaste die *Modelle* Ordner, und wählen **hinzufügen**  - &gt; **neues Element**. 

    ![Warenkorb - neues Element](shopping-cart/_static/image1.png)
2. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt. Wählen Sie **Code**, und wählen Sie dann **Klasse**. 

    ![Warenkorb - Dialogfeld "Neues Element" hinzufügen](shopping-cart/_static/image2.png)
3. Nennen Sie diese neue Klasse *CartItem.cs*.
4. Klicken Sie auf **Hinzufügen**.  
 Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

Die `CartItem` Klasse enthält das Schema, die ein Benutzer dem Einkaufswagen fügt jedes Produkt definieren. Diese Klasse ähnelt der anderen Schemaklassen, die Sie weiter oben in diesem Lernprogramm erstellt haben. Gemäß der Konvention Entity Framework Code First erwartet, dass der Primärschlüssel für die `CartItem` Tabelle ist, entweder `CartItemId` oder `ID`. Jedoch der Code überschreibt das Standardverhalten mithilfe der-Anmerkung Daten `[Key]` Attribut. Die `Key` Attribut der Element-ID-Eigenschaft gibt an, dass die `ItemID` Eigenschaft ist der Primärschlüssel.

Die `CartId` Eigenschaft gibt an, die `ID` des Benutzers, der das Element, das erwerben zugeordnet ist. Fügen Sie Code zum Erstellen dieses Benutzers `ID` Wenn der Benutzer den Einkaufswagen zugreift. Dies `ID` wird auch als eine ASP.NET-Sitzung-Variable gespeichert werden.

### <a name="update-the-product-context"></a>Aktualisieren Sie die Produktkontext

Zusätzlich zum Hinzufügen von der `CartItem` -Klasse, Sie müssen zum Aktualisieren der Datenbank Context-Klasse, die die Entitätsklassen verwaltet und den Datenzugriff auf die Datenbank bereitstellt. Zu diesem Zweck fügen Sie das neu erstellte `CartItem` model-Klasse, um die `ProductContext` Klasse.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *ProductContext.cs* in der Datei die *Modelle* Ordner.
2. Den hervorgehobenen Code zum Hinzufügen der *ProductContext.cs* Datei wie folgt:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Wie bereits erwähnt in diesem Lernprogramm Reihe, die den Code in der *ProductContext.cs* Datei fügt die `System.Data.Entity` Namespace, damit Sie Zugriff auf die Kernfunktionen von Entity Framework haben. Diese Funktionalität umfasst die Möglichkeit, Abfragen, einfügen, aktualisieren und Löschen von Daten mit stark typisierten Objekten arbeiten. Die `ProductContext` Klasse fügt den Zugriff auf das neu hinzugefügte `CartItem` modellschemas.

### <a name="managing-the-shopping-cart-business-logic"></a>Verwalten von Shopping Cart Geschäftslogik

Als Nächstes erstellen Sie die `ShoppingCart` Klasse in einer neuen *Logik* Ordner. Die `ShoppingCart` Klasse behandelt den Datenzugriff auf die `CartItem` Tabelle. Die Klasse gehören auch die Geschäftslogik zum Hinzufügen, entfernen und Aktualisieren von Elementen in den Einkaufswagen Sinn macht.

Die shopping Cart-Logik, die Sie hinzufügen, enthält Funktionen zum Verwalten der folgenden Aktionen:

1. Hinzufügen von Elementen in den Einkaufswagen legen
2. Entfernen von Elementen aus dem Warenkorb
3. Die Warenkorb-ID abrufen
4. Abrufen von Elementen aus dem Warenkorb
5. Die Menge aller shopping Cart Elemente insgesamt
6. Aktualisieren die Einkaufswagendaten

Eine Warenkorb-Seite (*ShoppingCart.aspx*) und die einkaufswagenklasse werden zusammen verwendet werden, auf Einkaufswagendaten zugreifen. Die Warenkorb-Seite zeigt alle Elemente, die der Benutzer dem Einkaufswagen waren hinzufügt. Neben dem Warenkorb Seiten- und Klasse, erstellen Sie eine Seite (*AddToCart.aspx*), um den Einkaufswagen Produkte hinzuzufügen. Außerdem fügen Sie Code der *"ProductList.aspx"* Seite und die *ProductDetails.aspx* Seite, die einen Link zum Bereitstellen der *AddToCart.aspx* Seite, sodass der Benutzer hinzufügen kann Produkte in den Einkaufswagen legen.

Das folgende Diagramm zeigt den grundlegenden Vorgang, der tritt auf, wenn der Benutzer ein Produkt der Einkaufswagen waren hinzufügt.

![Warenkorb - zum Einkaufswagen hinzufügen](shopping-cart/_static/image3.png)

Klickt der Benutzer die **Add To Cart** Link auf die *"ProductList.aspx"* Seite oder den *ProductDetails.aspx* Seite, die Anwendung, wechseln Sie automatisch auf die *AddToCart.aspx* Seite und dann automatisch in die *ShoppingCart.aspx* Seite. Die *AddToCart.aspx* Seite wird select Produkts zum Einkaufswagen hinzufügen, durch Aufrufen einer Methode in die ShoppingCart-Klasse. Die *ShoppingCart.aspx* Seite zeigt die Produkte, die dem Warenkorb hinzugefügt wurden.

#### <a name="creating-the-shopping-cart-class"></a>Erstellen die Einkaufswagenklasse

Die `ShoppingCart` Klasse wird in einen separaten Ordner in der Anwendung hinzugefügt werden, sodass eine klare Unterscheidung zwischen Modell (Ordner Models), die Seiten (Stammordner) und die Logik (Logik Ordner) werden.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **WingtipToys**Projekt, und wählen Sie **hinzufügen**-&gt;**neuer Ordner**. Nennen Sie diesen Ordner *Logik*.
2. Mit der rechten Maustaste die *Logik* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.
3. Fügen Sie eine neue Klassendatei mit dem Namen *ShoppingCartActions.cs*.
4. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Die `AddToCart` Methode ermöglicht die einzelnen Produkte in den Einkaufswagen Sinn macht, die basierend auf das Produkt aufgenommen werden `ID`. Das Produkt der Einkaufswagen hinzugefügt wurde, oder die Menge wird erhöht, wenn der Warenkorb bereits ein Element für dieses Produkt enthält.

Die `GetCartId` Methodenrückgabe dem Einkaufswagen `ID` für den Benutzer. Der Einkaufswagen `ID` wird verwendet, um die Elemente zu verfolgen, die ein Benutzer im Einkaufswagen verfügt. Wenn der Benutzer keiner vorhandenen Einkaufswagen `ID`, eine neue Einkaufswagen `ID` wird für sie erstellt. Wenn der Benutzer als ein registrierter Benutzer, dem Einkaufswagen angemeldet ist `ID` auf Eingabe des Benutzernamens festgelegt ist. Jedoch, wenn der Benutzer nicht, in dem Einkaufswagen angemeldet ist `ID` in einen eindeutigen Wert (eine GUID) festgelegt ist. Eine GUID wird sichergestellt, dass diese nur einen Warenkorb für jeden Benutzer, basierend auf der Sitzung erstellt wird.

Die `GetCartItems` Methode gibt eine Liste der Artikel im Warenkorb für den Benutzer zurück. Weiter unten in diesem Lernprogramm sehen Sie, dass wurden die modellbindung, zum Anzeigen der Warenkorb-Elemente verwendet wird in der shopping Cart mithilfe der `GetCartItems` Methode.

### <a name="creating-the-add-to-cart-functionality"></a>Erstellen die Add To Cart-Funktionalität

Wie bereits erwähnt, erstellen Sie eine Seite "Verarbeitung" mit dem Namen *AddToCart.aspx* , die zur neue Produkten zum Einkaufswagen des Benutzers hinzufügen verwendet werden. Diese Seite aufrufen, wird die `AddToCart` Methode in der `ShoppingCart` -Klasse, die Sie gerade erstellt haben. Die *AddToCart.aspx* Seite wird erwarten, dass ein Produkt `ID` übergeben wird. Dieses Produkt `ID` verwendet, wenn der Aufruf der `AddToCart` Methode in der `ShoppingCart` Klasse.

> [!NOTE] 
> 
> Ändern Sie den Code-Behind (*AddToCart.aspx.cs*) für diese Seite, nicht die Seite UI (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>So erstellen die Add To Cart Funktionalität:

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **WingtipToys**Projekt, klicken Sie auf **hinzufügen**  - &gt; **neues Element**.  
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Fügen Sie eine standardmäßige neue Seite (Web Form) an die Anwendung mit dem Namen *AddToCart.aspx*. 

    ![Warenkorb - Webformular hinzufügen](shopping-cart/_static/image4.png)
3. In **Projektmappen-Explorer**, mit der rechten Maustaste die *AddToCart.aspx* Seite, und klicken Sie dann auf **Code anzeigen**. Die *AddToCart.aspx.cs* Code-Behind-Datei im Editor geöffnet ist.
4. Ersetzen Sie den vorhandenen Code in der *AddToCart.aspx.cs* CodeBehind durch Folgendes:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Wenn die *AddToCart.aspx* Seite geladen ist, wird das Produkt `ID` aus der Abfragezeichenfolge abgerufen wird. Als Nächstes eine Instanz von der einkaufswagenklasse erstellt und zum Aufrufen der `AddToCart` -Methode, die Sie zuvor in diesem Lernprogramm hinzugefügt. Die `AddToCart` in enthaltenen-Methode der *ShoppingCartActions.cs* Datei, enthält die Logik zum ausgewählten Produkts zum Einkaufswagen hinzufügen, oder erhöhen die Produktmenge des ausgewählten Produkts. Wenn das Produkt nicht dem Warenkorb hinzugefügt wurde, wird das Produkt hinzugefügt der `CartItem` Tabelle der Datenbank. Wenn das Produkt bereits dem Warenkorb hinzugefügt wurde, und der Benutzer ein weiteres Element desselben Produkts fügt, wird der Produktmenge erhöht die `CartItem` Tabelle. Zum Schluss die Seite leitet an die *ShoppingCart.aspx* Seite, die Sie im nächsten Schritt hinzufügen, in denen der Benutzer eine aktualisierte Liste der Elemente im Warenkorb sieht.

Wie bereits erwähnt, einen Benutzer `ID` wird verwendet, um die Produkte zu identifizieren, die einem bestimmten Benutzer zugeordnet sind. Dies `ID` wird hinzugefügt, um eine Zeile in der `CartItem` Tabelle jedes Mal, die der Benutzer dem Einkaufswagen ein Produkts hinzugefügt.

### <a name="creating-the-shopping-cart-ui"></a>Den Einkaufswagen UI erstellen

Die *ShoppingCart.aspx* Seite zeigt die Produkte, die der Benutzer seinen Einkaufswagen legen hinzugefügt wurde. Es wird auch ermöglichen das Hinzufügen, entfernen und Aktualisieren von Elementen in den Einkaufswagen Sinn macht.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste **WingtipToys**, klicken Sie auf **hinzufügen**  - &gt; **neues Element**.  
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Fügen Sie eine neue Seite (Web Form), die durch Auswahl eine Masterseite enthält **Webformular mit Gestaltungsvorlage**. Nennen Sie die neue Seite *ShoppingCart.aspx*.
3. Wählen Sie **Site.Master** anzufügende Masterseite auf das neu erstellte *aspx* Seite.
4. In der *ShoppingCart.aspx* Seite, ersetzen Sie das vorhandene Markup durch Folgendes Markup:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Die *ShoppingCart.aspx* Seite enthält eine **GridView** Steuerelement namens `CartList`. Dieses Steuerelement verwendet wurden die modellbindung, um die Einkaufswagendaten aus der Datenbank zum Binden der **GridView** Steuerelement. Beim Festlegen der `ItemType` Eigenschaft von der **GridView** zu steuern, die Datenbindungsausdruck `Item` steht in das Markup des Steuerelements und das Steuerelement wird stark typisiert. Wie weiter oben in diesem Lernprogramm erwähnt, können Sie auswählen, Details zu den `Item` mithilfe von IntelliSense-Objekt. So konfigurieren ein Steuerelement zum modellbindung verwenden, um Daten auszuwählen, legen Sie die `SelectMethod` Eigenschaft des Steuerelements. In der oben genannten Markup, legen Sie die `SelectMethod` GetShoppingCartItems-Methode verwendet wird, die eine Liste von ausgibt `CartItem` Objekte. Die **GridView** Datensteuerelement Ruft die Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite und die zurückgegebenen Daten automatisch gebunden. Die `GetShoppingCartItems` Methode muss noch hinzugefügt werden.

#### <a name="retrieving-the-shopping-cart-items"></a>Abrufen von Shopping Cart-Elemente

Anschließend fügen Sie Code, um die *ShoppingCart.aspx.cs* CodeBehind zum Abrufen und zum Auffüllen der Shopping Cart-Benutzeroberfläche.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *ShoppingCart.aspx* Seite, und klicken Sie dann auf **Code anzeigen**. Die *ShoppingCart.aspx.cs* Code-Behind-Datei im Editor geöffnet ist.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Wie bereits erwähnt, die `GridView` Daten steuern Aufrufe der `GetShoppingCartItems` Methode zum entsprechenden Zeitpunkt während der Lebensdauer der Seite der Reihe nach und die zurückgegebenen Daten automatisch gebunden. Die `GetShoppingCartItems` Methode erstellt eine Instanz der `ShoppingCartActions` Objekt. Der Code verwendet dann diese Instanz zurückzugebenden Elemente im Warenkorb durch Aufrufen der `GetCartItems` Methode.

### <a name="adding-products-to-the-shopping-cart"></a>Hinzufügen von Produkten zum Einkaufswagen

Wenn entweder die *"ProductList.aspx"* oder *ProductDetails.aspx* Seite wird angezeigt, die der Benutzer wird in der Lage, das Produkt den Einkaufswagen Sinn macht, die über einen Link hinzuzufügen. Wenn sie den Link klicken, wird die Anwendung auf der Seite "Verarbeitung" mit dem Namen navigiert *AddToCart.aspx*. Die *AddToCart.aspx* Seite ruft den `AddToCart` Methode in der `ShoppingCart` -Klasse, die Sie zuvor in diesem Lernprogramm hinzugefügt.

Fügen Sie nun eine **Add to Cart** Link, um sowohl die *"ProductList.aspx"* Seite und die *ProductDetails.aspx* Seite. Dieser Link wird das Produkt enthalten `ID` , die aus der Datenbank abgerufen wird.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die Seite mit dem Namen *"ProductList.aspx"*.
2. Fügen Sie das Markup in gelb hervorgehoben werden die *"ProductList.aspx"* Seite so, dass die gesamte Seite wie folgt aussieht:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testen den Einkaufswagen

Führen Sie die Anwendung aus, um festzustellen, wie Sie Produkten zum Einkaufswagen hinzufügen.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Nachdem das Projekt mit die Datenbank neu erstellt, der Browser öffnen und Anzeigen der *"default.aspx"* Seite.
2. Wählen Sie **Autos** aus dem Navigationsmenü Kategorie.  
 Die *"ProductList.aspx"* Seite wird angezeigt, der nur die Produkte, die in der Kategorie "Cars" enthalten. 

    ![Warenkorb - Autos](shopping-cart/_static/image5.png)
3. Klicken Sie auf die **Add to Cart** neben dem ersten Produkt aufgeführt (Car konvertiert werden kann).   
 Die *ShoppingCart.aspx* wird mit der Auswahl im Einkaufskorb Seite angezeigt. 

    ![Warenkorb - Warenkorb](shopping-cart/_static/image6.png)
4. Weitere Produkte anzeigen, indem er **Ebenen** aus dem Navigationsmenü Kategorie.
5. Klicken Sie auf die **Add to Cart** neben dem ersten Produkt aufgeführt.  
 Die *ShoppingCart.aspx* Seite wird angezeigt, mit dem zusätzliche Element.
6. Schließen Sie den Browser.

### <a name="calculating-and-displaying-the-order-total"></a>Berechnen und die Gesamtsumme der Bestellungen anzeigen

Zusätzlich zum Hinzufügen von Produkten zum Einkaufswagen, fügen Sie eine `GetTotal` Methode, um die `ShoppingCart` -Klasse und der gesamte Bestellwert in die Warenkorb-Seite angezeigt.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCartActions.cs* in der Datei die *Logik* Ordner.
2. Fügen Sie die folgenden `GetTotal` Methode in gelb hervorgehoben werden die `ShoppingCart` Klasse, sodass die Klasse wie folgt aussieht:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Zunächst wird die `GetTotal` Methode ruft die ID des Warenkorbs für den Benutzer. Dann Ruft die Methode ab dem Einkaufswagen insgesamt durch Multiplizieren des Produktpreis durch die Produktmenge für jedes Produkt in den Einkaufswagen aufgeführt.

> [!NOTE] 
> 
> Der obige Code verwendet die nullable-Typ "`int?`". Auf NULL festlegbare Typen können die Werte eines zugrunde liegenden Typs und auch als null-Wert darstellen. Weitere Informationen finden Sie unter [mithilfe von auf NULL festlegbaren Typen](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Ändern Sie die Shopping Cart-Anzeige

Als Nächstes ändern Sie den Code für die *ShoppingCart.aspx* Seite Aufrufen der `GetTotal` -Methode und der Anzeige, die auf die Gesamtanzahl der *ShoppingCart.aspx* Seite beim Laden der Seite.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *ShoppingCart.aspx* Seite und wählen Sie **Code anzeigen**.
2. In der *ShoppingCart.aspx.cs* Datei, aktualisieren Sie die `Page_Load` Handler durch Hinzufügen des folgenden Codes in gelb hervorgehoben:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Wenn die *ShoppingCart.aspx* geladen wurde, lädt das shopping Cart-Objekt ist, und ruft dann die shopping Cart insgesamt ab, durch Aufrufen der `GetTotal` Methode der `ShoppingCart` Klasse. Wenn der Warenkorb leer ist, wird dazu eine Meldung angezeigt.

### <a name="testing-the-shopping-cart-total"></a>Testen der Shopping Cart gesamt

Führen Sie die Anwendung jetzt sehen, wie Sie nicht nur ein Produkts zum Einkaufswagen hinzufügen können, aber Sie können die shopping Cart insgesamt finden Sie unter.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Der Browser öffnen und Anzeigen der *"default.aspx"* Seite.
2. Wählen Sie **Autos** aus dem Navigationsmenü Kategorie.
3. Klicken Sie auf die **Add To Cart** neben dem ersten Produkt.   
 Die *ShoppingCart.aspx* Seite wird angezeigt, mit der Gesamtsumme der Bestellungen. 

    ![Warenkorb - Einkaufswagen gesamt](shopping-cart/_static/image7.png)
4. Dem Einkaufswagen einige andere Produkte (z. B. eine Ebene) hinzugefügt.
5. Die *ShoppingCart.aspx* Seite wird angezeigt, mit einer aktualisierten Summe für alle Produkte, die Sie hinzugefügt haben. 

    ![Warenkorb - mehrere Produkte](shopping-cart/_static/image8.png)
6. Beenden Sie die ausgeführte Anwendung, indem Sie das Browserfenster schließen.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Hinzufügen von Schaltflächen Aktualisieren und Auschecken dem Warenkorb

Damit wird der Benutzer den Einkaufswagen zu ändern, fügen Sie ein **Update** Schaltfläche und ein **Auschecken** Schaltfläche, um die Warenkorb-Seite. Die **Auschecken** Schaltfläche wird nicht verwendet, bis Sie weiter unten in diesem Lernprogramm Reihe.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCart.aspx* Seite im Stammverzeichnis des das Webanwendungsprojekt.
2. Hinzufügen der **Update** Schaltfläche und die **Auschecken** -Schaltfläche, um die *ShoppingCart.aspx* Seite, fügen Sie das Markup, die auf das vorhandene Markup gelb hervorgehoben, entsprechend der folgender Code:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Wenn der Benutzer klickt der **Update** Schaltfläche, die `UpdateBtn_Click` -Ereignishandler wird aufgerufen. Dieser Ereignishandler wird der Code aufrufen, den Sie im nächsten Schritt hinzufügen.

Als Nächstes können Sie den Code in den Aktualisieren der *ShoppingCart.aspx.cs* Datei so durchlaufen Sie den Warenkorb und rufen die `RemoveItem` und `UpdateItem` Methoden.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCart.aspx.cs* Datei im Stammverzeichnis des das Webanwendungsprojekt.
2. Fügen Sie die folgenden Codeabschnitte in gelb hervorgehoben werden die *ShoppingCart.aspx.cs* Datei:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Klickt der Benutzer die **Update** Schaltfläche auf der *ShoppingCart.aspx* Seite, die die UpdateCartItems-Methode aufgerufen wird. Die UpdateCartItems-Methode ruft die aktualisierten Werte für jedes Element in den Einkaufswagen Sinn macht. Klicken Sie dann die UpdateCartItems-Methode ruft die `UpdateShoppingCartDatabase` -Methode (hinzugefügt und im nächsten Schritt beschrieben) entweder hinzufügen oder Entfernen von Elementen aus dem Warenkorb. Sobald die Datenbank aktualisiert wurde, entsprechend der Updates auf den Einkaufswagen Sinn macht, die **GridView** Steuerelement wird auf die Warenkorb-Seite aktualisiert, durch Aufrufen der `DataBind` Methode für die **GridView**. Darüber hinaus wird der gesamte Bestellwert auf die Warenkorb-Seite aktualisiert, um die aktualisierte Liste der Elemente widerzuspiegeln.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualisieren und Entfernen von Artikel im Warenkorb

Auf der *ShoppingCart.aspx* Seite sehen, da Sie Steuerelemente wurden für die Menge eines Elements aktualisieren und löschen ein Element hinzugefügt. Fügen Sie nun den Code, der diese Steuerelemente arbeiten erkennen lässt.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCartActions.cs* in der Datei die *Logik* Ordner.
2. Fügen Sie den folgenden Code in gelb hervorgehoben werden die *ShoppingCartActions.cs* Klassendatei:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Die `UpdateShoppingCartDatabase` aus aufgerufene Methode der `UpdateCartItems` Methode für die *ShoppingCart.aspx.cs* Seite ist, enthält die Logik zum Aktualisieren oder Entfernen von Elementen aus dem Warenkorb. Die `UpdateShoppingCartDatabase` Methode durchläuft alle Zeilen in der shopping Cart-Liste. Wenn eine shopping Cart-Element entfernend gekennzeichnet wurde, oder die Menge kleiner als 1 ist, die `RemoveItem` -Methode aufgerufen wird. Anderenfalls wird das shopping Cart-Element für überprüft aktualisiert, wenn die `UpdateItem` Methode wird aufgerufen. Nachdem das shopping Cart-Element aktualisiert oder entfernt wurde, werden Änderungen an der Datenbank gespeichert.

Die `ShoppingCartUpdates` Struktur wird verwendet, um die shopping Cart-Elemente enthalten. Die `UpdateShoppingCartDatabase` -Methode verwendet die `ShoppingCartUpdates` Struktur zu bestimmen, ob eines der Elemente, die aktualisiert oder entfernt werden müssen.

In den nächsten Lernprogrammen verwenden Sie die `EmptyCart` Methode zum Deaktivieren der Warenkorb Einkaufswagen nach dem Kauf von Produkten. Aber jetzt verwenden Sie die `GetCount` -Methode, die Sie gerade hinzugefügt haben die *ShoppingCartActions.cs* Datei, um zu bestimmen, wie viele Elemente in den Warenkorb enthalten sind.

### <a name="adding-a-shopping-cart-counter"></a>Hinzufügen eines Shopping Cart-Leistungsindikators

Damit wird den Benutzer, die Gesamtanzahl der Elemente in den Warenkorb anzuzeigen, fügen Sie einen Leistungsindikator der *Site.Master* Seite. Dieser Leistungsindikator wird auch als einen Link zu dem Einkaufswagen fungieren.

1. In **Projektmappen-Explorer**öffnen die *Site.Master* Seite.
2. Ändern Sie das Markup, indem Sie den warenkorblink klicken Indikator hinzufügen, wie in Gelb Abschnitt Navigation dargestellt werden, damit er wie folgt aussieht:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Aktualisieren Sie den Code-Behind der als Nächstes die *Site.Master.cs* Datei durch Hinzufügen des Codes in gelb hervorgehoben werden wie folgt:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Bevor Sie die Seite als HTML gerendert wird die `Page_PreRender` Ereignis wird ausgelöst. In der `Page_PreRender` Handler, der Gesamtzahl der Einkaufswagen bestimmt, indem die `GetCount` Methode. Der zurückgegebene Wert wird hinzugefügt, um die `cartCount` Spanne im Markup enthalten die *Site.Master* Seite. Die `<span>` Tags ermöglicht die inneren Elemente ordnungsgemäß gerendert werden soll. Wenn einer beliebigen Seite des Standorts angezeigt wird, wird die shopping Cart insgesamt angezeigt. Der Benutzer kann auch die shopping Cart insgesamt zum Anzeigen des Einkaufswagen klicken.

## <a name="testing-the-completed-shopping-cart"></a>Testen den abgeschlossenen Einkaufswagen

Sie können die Anwendung jetzt sehen, wie Sie hinzufügen können, löschen und Aktualisieren von Elementen in den Warenkorb ausführen. Die shopping Cart insgesamt wider, die Gesamtkosten für alle Elemente in den Einkaufswagen Sinn macht.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Wählen Sie **Autos** aus dem Navigationsmenü Kategorie.
3. Klicken Sie auf die **Add To Cart** neben dem ersten Produkt.   
 Die *ShoppingCart.aspx* Seite wird angezeigt, mit der Gesamtsumme der Bestellungen.
4. Wählen Sie **Ebenen** aus dem Navigationsmenü Kategorie.
5. Klicken Sie auf die **Add To Cart** neben dem ersten Produkt.
6. Die Menge des ersten Elements in den Warenkorb auf 3 festlegen, und wählen Sie die **Element entfernen** Sie das Kontrollkästchen für das zweite Element.<a id="a"></a>
7. Klicken Sie auf die **aktualisieren** Schaltfläche, um die Warenkorb-Seite zu aktualisieren und neue Gesamtsumme der Bestellungen anzuzeigen. 

    ![Warenkorb - Warenkorb-Update](shopping-cart/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie den Einkaufswagen legen für die Wingtip Toys Web Forms-beispielanwendung erstellt. Im Verlauf dieses Lernprogramms haben Sie Entity Framework Code First, datenanmerkungen, stark typisierte Datensteuerelemente und wurden die modellbindung verwendet.

Der Einkaufswagen unterstützt hinzufügen, löschen und Aktualisieren von Elementen, die der Benutzer für den Einkauf ausgewählt wurde. Zusätzlich zur Implementierung shopping Cart-Funktionalität, haben Sie gelernt, wie anzuzeigende shopping Cart-Elemente in einem **GridView** steuern und die Gesamtsumme der Bestellungen zu berechnen.

## <a name="addition-information"></a>Zusätzliche Informationen

[ASP.NET Session State (Übersicht)](https://msdn.microsoft.com/en-us/library/ms178581.aspx)

>[!div class="step-by-step"]
[Zurück](display_data_items_and_details.md)
[Weiter](checkout-and-payment-with-paypal.md)
