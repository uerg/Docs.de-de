---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Warenkorb | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 22253b8708efd9ce505c9fbeb9cb2e942588b37d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375777"
---
<a name="shopping-cart"></a>Einkaufswagen
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial wird beschrieben, die Geschäftslogik erforderlich, um einen Einkaufswagen gelegt hat der Wingtip Toys-beispielanwendung von ASP.NET Web Forms hinzuzufügen. Dieses Tutorial baut auf dem vorherigen Lernprogramm "Anzeige Elemente und Details zum Data" und ist Teil der tutorialreihe Wingtip Toys-Store. Wenn Sie dieses Tutorial abgeschlossen haben, werden die Benutzer die Beispiel-app hinzufügen, entfernen und ändern die Produkte im Einkaufswagen können.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

1. Vorgehensweise: Erstellen Sie einen Einkaufswagen gelegt hat, für die Webanwendung.
2. Wie Benutzer Elemente zum Einkaufswagen hinzufügen können.
3. Gewusst wie: Hinzufügen einer [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) Steuerelement shopping Cart-Details angezeigt.
4. Informationen zum Berechnen und die Gesamtsumme der Bestellungen anzuzeigen.
5. Vorgehensweise beim Entfernen und Aktualisieren von Elementen in den Einkaufswagen.
6. Wie Sie einen shopping Cart-Indikator enthalten.

## <a name="code-features-in-this-tutorial"></a>Features des Code in diesem Tutorial:

1. Entitätsframework Code First
2. Datenanmerkungen
3. Stark typisierte Datensteuerelemente
4. Modellbindung

## <a name="creating-a-shopping-cart"></a>Erstellen einen Einkaufswagen gelegt hat

Weiter oben in dieser tutorialreihe haben Sie Seiten und Code zum Anzeigen von Produktdaten aus einer Datenbank hinzugefügt. In diesem Tutorial erstellen Sie einen Einkaufswagen gelegt hat, um die Produkte zu verwalten, erwerben Benutzer interessiert sind. Benutzer werden suchen und Hinzufügen von Elementen zum Einkaufswagen, auch wenn sie nicht registriert oder angemeldet sind. Zum Verwalten des Zugriffs für shopping Cart weisen Sie Benutzern eine eindeutige `ID` zum ersten Mal Einkaufswagen einen global eindeutigen Bezeichner (GUID) verwenden, wenn der Benutzer dem Einkaufswagen zugreift. Sie speichern diese `ID` mithilfe den ASP.NET Sitzungszustand.

> [!NOTE] 
> 
> Der Sitzungszustand von ASP.NET ist eine bequeme Möglichkeit, benutzerspezifische Informationen speichern, die ablaufen wird, nachdem der Benutzer die Site verlässt. Während unsachgemäße Verwendung des Sitzungsstatus für größere Sites die Auswirkungen auf die Leistung haben kann, Licht Ins mithilfe der Sitzung, die Zustand funktioniert auch für Demonstrationszwecke. Die Wingtip Toys-Beispielprojekt veranschaulicht Sie Sitzungszustand ohne einen externen Anbieter, wobei Sitzungszustand gespeicherten in-Process auf dem Webserver, der die Standortdatenbank hostet. Bei größeren Sites, die mehrere Instanzen einer Anwendung bereitstellen oder Standorten, an denen mehrere Instanzen einer Anwendung auf verschiedenen Servern ausführen, sollten Sie **Windows Azure Cache Service**. Dieser Cache-Dienst bietet es sich um ein verteilter Cachedienst, der für die Website extern ist und löst das Problem der Verwendung von in-Process-Sitzungszustand. Weitere Informationen finden Sie unter [verwendet ASP.NET Session State mit Windows Azure-Websites wie](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Fügen Sie CartItem als eine Modellklasse hinzu.

Weiter oben in dieser tutorialreihe definiert Sie das Schema für die Kategorie und Daten durch das Erstellen der `Category` und `Product` Klassen in der *Modelle* Ordner. Fügen Sie nun eine neue Klasse zum Definieren des Schemas für den Einkaufswagen hinzu. Weiter unten in diesem Tutorial fügen Sie eine Klasse zum Behandeln des Datenzugriffs für die `CartItem` Tabelle. Diese Klasse wird die Geschäftslogik zum Hinzufügen, entfernen und aktualisieren die Elemente im Warenkorb bereitstellen.

1. Mit der rechten Maustaste die *Modelle* Ordner, und wählen **hinzufügen**  - &gt; **neues Element**. 

    ![Warenkorb - neues Element](shopping-cart/_static/image1.png)
2. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt. Wählen Sie **Code**, und wählen Sie dann **Klasse**. 

    ![Warenkorb - Dialogfeld für neue Elemente hinzufügen](shopping-cart/_static/image2.png)
3. Nennen Sie diese neue Klasse *CartItem.cs*.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

Die `CartItem` -Klasse enthält das Schema, die definieren, jedes Produkt zum Einkaufswagen hinzufügen. Diese Klasse ähnelt den anderen Klassen, die Sie zuvor in dieser tutorialreihe erstellt haben. Gemäß der Konvention, Entity Framework Code First erwartet, dass der primäre Schlüssel für die `CartItem` Tabelle werden entweder `CartItemId` oder `ID`. Allerdings der Code überschreibt das Standardverhalten, mithilfe der-Anmerkung Daten `[Key]` Attribut. Die `Key` Attribut der Element-ID-Eigenschaft gibt an, dass die `ItemID` Eigenschaft ist der primäre Schlüssel.

Die `CartId` Eigenschaft gibt an, die `ID` des Benutzers, der das Element, das erwerben zugeordnet ist. Fügen Sie Code zum Erstellen dieser Benutzer `ID` Wenn der Benutzer greift auf den Einkaufswagen. Dies `ID` werden auch als ASP.NET-Sitzung-Variable gespeichert.

### <a name="update-the-product-context"></a>Aktualisieren Sie den Produktkontext

Neben dem Hinzufügen der `CartItem` -Klasse, Sie müssen den Datenbankkontext-Klasse zu aktualisieren, verwaltet die Entitätsklassen und den Datenzugriff auf die Datenbank bereitstellt. Zu diesem Zweck fügen Sie die neu erstellte `CartItem` model-Klasse, um die `ProductContext` Klasse.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *ProductContext.cs* Datei die *Modelle* Ordner.
2. Fügen Sie den hervorgehobenen Code die *ProductContext.cs* -Datei wie folgt:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Wie bereits erwähnt in dieser tutorialreihe, den Code in die *ProductContext.cs* Datei fügt die `System.Data.Entity` Namespace, damit Sie Zugriff auf die Kernfunktionen des Entity Framework haben. Diese Funktionen umfassen die Möglichkeit, Abfragen, einfügen, aktualisieren und Löschen von Daten durch Verwendung stark typisierter Objekte. Die `ProductContext` Klasse fügt den Zugriff auf die neu hinzugefügte `CartItem` Modellklasse.

### <a name="managing-the-shopping-cart-business-logic"></a>Die Shopping Cart-Geschäftslogik verwalten

Als Nächstes erstellen Sie die `ShoppingCart` Klasse in einer neuen *Logik* Ordner. Die `ShoppingCart` Klasse behandelt den Datenzugriff auf die `CartItem` Tabelle. Die Klasse umfasst auch die Geschäftslogik zum Hinzufügen, entfernen und aktualisieren die Elemente im Warenkorb.

Die shopping Cart-Logik, die Sie hinzufügen, enthält Funktionen zum Verwalten der folgenden Aktionen:

1. Hinzufügen von Elementen zum Einkaufswagen
2. Entfernen von Elementen aus dem Warenkorb
3. Die Warenkorb-ID abrufen
4. Abrufen von Elementen aus dem Einkaufswagen
5. Addieren Sie die Menge an alle dem Artikel im Warenkorb
6. Aktualisieren die Einkaufswagendaten

Eine Warenkorb-Seite (*ShoppingCart.aspx*) und die einkaufswagenklasse werden zusammen verwendet werden, auf die Einkaufswagendaten. Die Warenkorb-Seite zeigt alle Elemente, die der Benutzer zum Einkaufswagen hinzufügt. Neben den Bezug auf den Warenkorb Seite und die Klasse, erstellen Sie eine Seite (*AddToCart.aspx*), um den Einkaufswagen Produkte hinzuzufügen. Außerdem fügen Sie Code für die *"ProductList.aspx"* Seite und die *ProductDetails.aspx* Seite, die einen Link zum Bereitstellen der *AddToCart.aspx* Seite, damit der Benutzer hinzufügen kann Produkte in den Einkaufswagen legen.

Das folgende Diagramm zeigt das grundlegende Verfahren, das auftritt, wenn der Benutzer ein Produkts zum Einkaufswagen hinzufügt.

![Warenkorb - zum Einkaufswagen hinzufügen](shopping-cart/_static/image3.png)

Klickt der Benutzer die **Add To Cart** Link auf die *"ProductList.aspx"* Seite oder die *ProductDetails.aspx* Seite, die Anwendung navigiert zu der *AddToCart.aspx* Seite und dann automatisch in die *ShoppingCart.aspx* Seite. Die *AddToCart.aspx* Seite wird das Auswählen des Produkts zum Einkaufswagen hinzufügen, durch Aufrufen einer Methode in die ShoppingCart-Klasse. Die *ShoppingCart.aspx* Seite zeigt die Produkte, die zum Einkaufswagen hinzugefügt wurden.

#### <a name="creating-the-shopping-cart-class"></a>Erstellen der Shopping Cart-Klasse

Die `ShoppingCart` Klasse werden in einen separaten Ordner in der Anwendung hinzugefügt werden, sodass eine klare Unterscheidung zwischen des Modells (Ordner "Models"), die Seiten (Stammordner) und die Logik (Ordner für Logik) fallen.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **WingtipToys**Projekt, und wählen **hinzufügen**-&gt;**neuer Ordner**. Nennen Sie diesen Ordner *Logik*.
2. Mit der rechten Maustaste die *Logik* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.
3. Fügen Sie eine neue Klassendatei mit dem Namen *ShoppingCartActions.cs*.
4. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Die `AddToCart` Methode ermöglicht die einzelnen Produkte in den Einkaufswagen basierend auf dem Produkt einbezogen werden `ID`. Das Produkt wird dem Warenkorb hinzugefügt, oder wenn der Warenkorb bereits ein Element für dieses Produkt enthält, wird die Menge erhöht.

Die `GetCartId` Methodenrückgabe des Warenkorbs `ID` für den Benutzer. Der Einkaufswagen `ID` wird verwendet, um die Elemente zu verfolgen, die ein Benutzer in ihrem Einkaufswagen besitzt. Wenn der Benutzer keiner vorhandenen Warenkorb `ID`, eine neue Warenkorb `ID` wird für sie erstellt. Wenn die Anmeldung des Benutzers als registrierter Benutzer, dem Einkaufswagen `ID` festgelegt ist, die Ihrem jeweiligen Benutzernamen. Aber wenn der Benutzer nicht, in den Warenkorb angemeldet ist `ID` in einen eindeutigen Wert (eine GUID) festgelegt ist. Eine GUID wird sichergestellt, dass dieser nur einen Warenkorb für jeden Benutzer, die basierend auf der Sitzung erstellt wird.

Die `GetCartItems` Methode gibt eine Liste der Artikel im Warenkorb für den Benutzer zurück. Weiter unten in diesem Tutorial sehen Sie, dass modellbindung, zum Anzeigen der Warenkorb-Elemente verwendet wird in die shopping Cart mithilfe der `GetCartItems` Methode.

### <a name="creating-the-add-to-cart-functionality"></a>Erstellen die Add To Cart-Funktion

Wie bereits erwähnt, erstellen Sie eine Verarbeitung-Seite namens *AddToCart.aspx* , Hinzufügen von neuen Produkten zum Einkaufswagen des Benutzers verwendet wird. Auf dieser Seite ruft die `AddToCart` -Methode in der die `ShoppingCart` -Klasse, die Sie gerade erstellt haben. Die *AddToCart.aspx* Seite werden erwarten, dass ein Produkt `ID` übergeben wird. Dieses Produkt `ID` verwendet wird, wenn der Aufruf der `AddToCart` -Methode in der die `ShoppingCart` Klasse.

> [!NOTE] 
> 
> Ändern Sie den Code-Behind (*AddToCart.aspx.cs*) für diese Seite, nicht die Seiten-UI (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Zum Erstellen von Add To Cart Funktionen:

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **WingtipToys**Projekt, klicken Sie auf **hinzufügen**  - &gt; **neues Element**.  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Fügen Sie eine standardmäßige neue Seite (Web Form), für die Anwendung, die mit dem Namen *AddToCart.aspx*. 

    ![Warenkorb - Web Form hinzufügen](shopping-cart/_static/image4.png)
3. In **Projektmappen-Explorer**, mit der rechten Maustaste die *AddToCart.aspx* Seite, und klicken Sie dann auf **Ansichtscode**. Die *AddToCart.aspx.cs* Code-Behind-Datei im Editor geöffnet ist.
4. Ersetzen Sie den vorhandenen Code in die *AddToCart.aspx.cs* CodeBehind durch Folgendes:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Wenn die *AddToCart.aspx* Seite geladen wurde, wird das Produkt `ID` aus der Abfragezeichenfolge abgerufen wird. Als Nächstes eine Instanz von der einkaufswagenklasse erstellt und verwendet Sie zum Aufrufen der `AddToCart` -Methode, die Sie zuvor in diesem Lernprogramm hinzugefügt haben. Die `AddToCart` in enthaltenen-Methode der *ShoppingCartActions.cs* Datei, die Logik für das ausgewählte Produkt zum Einkaufswagen hinzufügen, oder erhöhen die Produktmenge des ausgewählten Produkts enthält. Wenn nicht das Produkt zum Einkaufswagen hinzugefügt wurde, das Produkt wurde die `CartItem` Tabelle der Datenbank. Wenn das Produkt bereits zum Einkaufswagen hinzugefügt wurde und der Benutzer fügt ein zusätzliches Element desselben Produkts hinzu, wird die Produktmenge erhöht die `CartItem` Tabelle. Zum Schluss die Seite leitet an die *ShoppingCart.aspx* Seite, die Sie im nächsten Schritt hinzufügen, in denen der Benutzer eine aktualisierte Liste der Elemente im Warenkorb sieht.

Wie bereits erwähnt, einen Benutzer `ID` wird verwendet, um die Produkte zu identifizieren, die einen bestimmten Benutzer zugeordnet sind. Dies `ID` wird hinzugefügt, um eine Zeile in der `CartItem` Tabelle jedes Mal, die der Benutzer ein Produkt zum Einkaufswagen hinzugefügt.

### <a name="creating-the-shopping-cart-ui"></a>Den Einkaufswagen UI erstellen

Die *ShoppingCart.aspx* Seite zeigt die Produkte, die der Benutzer zu ihrem Einkaufswagen hinzugefügt hat. Es stellt auch die Möglichkeit, hinzufügen, entfernen und aktualisieren die Elemente im Warenkorb bereit.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste **WingtipToys**, klicken Sie auf **hinzufügen**  - &gt; **neues Element**.  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Hinzufügen einer neuen Seite (Web Form), die eine Masterseite dazu enthält **Webformular mit Gestaltungsvorlage**. Nennen Sie die neue Seite *ShoppingCart.aspx*.
3. Wählen Sie **Site.Master** anzufügende Masterseite auf das neu erstellte *aspx* Seite.
4. In der *ShoppingCart.aspx* Seite, ersetzen Sie das vorhandene Markup durch das folgende Markup:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Die *ShoppingCart.aspx* Seite enthält eine **GridView** Steuerelement mit dem Namen `CartList`. Dieses Steuerelement verwendet die modellbindung binden Sie die Einkaufswagendaten aus der Datenbank, die **GridView** Steuerelement. Beim Festlegen der `ItemType` Eigenschaft der **GridView** zu steuern, die XPath-Datenbindungsausdruck `Item` finden Sie in das Markup des Steuerelements und das Steuerelement wird stark typisiert. Wie weiter oben in dieser tutorialreihe erwähnt, können Sie auswählen, Details zu den `Item` Objekt unter Verwendung von IntelliSense. Um ein Steuerelement zum modellbindung zu verwenden, zum Auswählen von Daten zu konfigurieren, legen Sie die `SelectMethod` -Eigenschaft des Steuerelements. In das Markup oben, legen Sie die `SelectMethod` die GetShoppingCartItems-Methode verwenden, wodurch eine Liste der `CartItem` Objekte. Die **GridView** -Steuerelement ruft die Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite und die zurückgegebenen Daten automatisch gebunden. Die `GetShoppingCartItems` Methode muss immer noch hinzugefügt werden.

#### <a name="retrieving-the-shopping-cart-items"></a>Abrufen der Artikel im Warenkorb

Anschließend fügen Sie Code für die *ShoppingCart.aspx.cs* CodeBehind zum Abrufen und Auffüllen der Shopping Cart-Benutzeroberfläche.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *ShoppingCart.aspx* Seite, und klicken Sie dann auf **Ansichtscode**. Die *ShoppingCart.aspx.cs* Code-Behind-Datei im Editor geöffnet ist.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Wie bereits erwähnt, die `GridView` Daten steuern, Aufrufe der `GetShoppingCartItems` Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite blättern und bindet automatisch die zurückgegebenen Daten. Die `GetShoppingCartItems` Methode erstellt eine Instanz der `ShoppingCartActions` Objekt. Klicken Sie dann mit der Code dieser Instanz verwendet, um die Elemente zusammen in den durch den Aufruf zurückgeben dem `GetCartItems` Methode.

### <a name="adding-products-to-the-shopping-cart"></a>Hinzufügen von Produkten zum Einkaufswagen

Wenn entweder die *"ProductList.aspx"* oder *ProductDetails.aspx* Seite wird angezeigt, die der Benutzer kann das Produkt zum Einkaufswagen über einen Link hinzufügen. Wenn sie den Link klicken, wird die Anwendung navigiert, um die Seite "Verarbeitung" mit dem Namen *AddToCart.aspx*. Die *AddToCart.aspx* Seite ruft die `AddToCart` -Methode in der die `ShoppingCart` -Klasse, die Sie zuvor in diesem Lernprogramm hinzugefügt haben.

Fügen Sie nun eine **zum Warenkorb hinzufügen** Link sowohl die *"ProductList.aspx"* Seite und die *ProductDetails.aspx* Seite. Dieser Link enthält das Produkt `ID` , die aus der Datenbank abgerufen wird.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die Seite mit dem Namen *"ProductList.aspx"*.
2. Fügen Sie das Markup in Gelb zu markiert die *"ProductList.aspx"* Seite, damit die gesamte Seite wie folgt aussieht:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testen des Einkaufswagens

Führen Sie die Anwendung aus, um festzustellen, wie Sie Produkten zum Einkaufswagen hinzufügen.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Nachdem das Projekt mit die Datenbank neu erstellt, der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Wählen Sie **Autos** im Navigationsmenü der Kategorie.  
 Die *"ProductList.aspx"* Seite wird angezeigt, die nur die Produkte in der Kategorie "Cars" enthalten. 

    ![Warenkorb - Autos](shopping-cart/_static/image5.png)
3. Klicken Sie auf die **zum Warenkorb hinzufügen** neben das erste Produkt aufgeführt (Autos konvertiert werden kann).   
 Die *ShoppingCart.aspx* Seite wird angezeigt, die Auswahl in Ihrem Warenkorb. 

    ![Warenkorb - Warenkorb](shopping-cart/_static/image6.png)
4. Zusätzliche Produkte anzeigen, indem er **Ebenen** im Navigationsmenü der Kategorie.
5. Klicken Sie auf die **zum Warenkorb hinzufügen** neben das erste Produkt, aufgeführt.  
 Die *ShoppingCart.aspx* Seite wird angezeigt, mit dem zusätzliche Element.
6. Schließen Sie den Browser.

### <a name="calculating-and-displaying-the-order-total"></a>Berechnen und Anzeigen von der Gesamtsumme der Bestellungen

Zusätzlich zum Hinzufügen von Produkten zum Einkaufswagen, fügen Sie eine `GetTotal` Methode, um die `ShoppingCart` Klasse und den Betrag in die Warenkorb-Seite angezeigt.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCartActions.cs* Datei die *Logik* Ordner.
2. Fügen Sie die folgenden `GetTotal` Methode, die in Gelb zu markiert die `ShoppingCart` Klasse, sodass die Klasse wie folgt aussieht:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Zunächst wird die `GetTotal` Methode ruft die ID des Warenkorbs für den Benutzer ab. Klicken Sie dann Ruft die Methode ab dem Einkaufswagen insgesamt durch Multiplikation der Produktpreis mit die Produktmenge für jedes Produkt im Einkaufswagen aufgeführt.

> [!NOTE] 
> 
> Der obige Code verwendet einen nullable-Typ "`int?`". Auf NULL festlegbare Typen können alle Werte eines zugrunde liegenden Typs sowie als null-Wert darstellen. Weitere Informationen finden Sie unter [Using Nullable Types](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Ändern Sie die Shopping Cart-Anzeige

Als Nächstes ändern Sie den Code für die *ShoppingCart.aspx* Seite Aufrufen der `GetTotal` -Methode und die Anzeige ab, die auf die Gesamtanzahl der *ShoppingCart.aspx* Seite beim Laden der Seite.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *ShoppingCart.aspx* Seite und wählen Sie **Ansichtscode**.
2. In der *ShoppingCart.aspx.cs* Datei, aktualisieren Sie die `Page_Load` Ereignishandler, indem Sie den folgenden Code in gelb hervorgehoben hinzufügen:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Wenn die *ShoppingCart.aspx* Seite geladen wird, lädt das shopping Cart-Objekt und ruft dann die einkaufswagensumme durch Aufrufen der `GetTotal` -Methode der der `ShoppingCart` Klasse. Wenn der Einkaufswagen leer ist, wird zu diesem Zweck eine Meldung angezeigt.

### <a name="testing-the-shopping-cart-total"></a>Testen der Einkaufswagensumme

Führen Sie die Anwendung jetzt, um festzustellen, wie Sie nicht nur ein Produkts zum Einkaufswagen hinzufügen können, aber Sie können die einkaufswagensumme finden Sie unter.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Wählen Sie **Autos** im Navigationsmenü der Kategorie.
3. Klicken Sie auf die **Add To Cart** neben das erste Produkt.   
 Die *ShoppingCart.aspx* Seite wird angezeigt, mit der Gesamtsumme der Bestellungen. 

    ![Warenkorb - Warenkorb gesamt](shopping-cart/_static/image7.png)
4. Fügen Sie dem Einkaufswagen einige andere Produkte (z. B. eine Ebene) hinzu.
5. Die *ShoppingCart.aspx* Seite wird angezeigt, mit einer aktualisierten Summe für alle Produkte, die Sie hinzugefügt haben. 

    ![Warenkorb - mehrere Produkte](shopping-cart/_static/image8.png)
6. Beenden Sie die ausgeführte app, indem Sie das Browserfenster schließen.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Update und Auschecken Schaltflächen zum Einkaufswagen hinzufügen

Damit wird der Benutzer den Einkaufswagen zu ändern, fügen Sie eine **Update** Schaltfläche und ein **Auschecken** Schaltfläche, um die Warenkorb-Seite. Die **Auschecken** Schaltfläche wird erst später in diesem Tutorial nicht verwendet.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCart.aspx* Seite im Stammverzeichnis des Webanwendungsprojekts.
2. Hinzufügen der **Update** Schaltfläche und die **Auschecken** Schaltfläche, um die *ShoppingCart.aspx* Seite, fügen Sie das Markup in Gelb, um das vorhandene Markup, hervorgehoben, siehe die folgender Code:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Klickt der Benutzer die **Update** Schaltfläche der `UpdateBtn_Click` -Ereignishandler wird aufgerufen. Dieser Ereignishandler ruft den Code, den Sie im nächsten Schritt hinzufügen.

Sie können als Nächstes aktualisieren Sie den Code, der in enthalten die *ShoppingCart.aspx.cs* Datei so durchlaufen Sie das im Warenkorb, und rufen die `RemoveItem` und `UpdateItem` Methoden.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCart.aspx.cs* Datei im Stammverzeichnis des Webanwendungsprojekts.
2. Fügen Sie die folgenden Codeabschnitte in Gelb zu markiert die *ShoppingCart.aspx.cs* Datei:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Klickt der Benutzer die **Update** Schaltfläche der *ShoppingCart.aspx* Seite, die die UpdateCartItems-Methode aufgerufen wird. Die UpdateCartItems-Methode ruft die aktualisierten Werte für jeden Artikel im Warenkorb ab. Klicken Sie dann die UpdateCartItems-Methode ruft die `UpdateShoppingCartDatabase` -Methode (hinzugefügt und im nächsten Schritt beschrieben) entweder hinzufügen oder Entfernen von Elementen aus dem Warenkorb. Sobald die Updates in den Einkaufswagen legen, entsprechend die Datenbank aktualisiert wurde die **GridView** Steuerelement wird aktualisiert, auf die Warenkorb-Seite durch Aufrufen der `DataBind` -Methode für die **GridView**. Darüber hinaus wird der Betrag für die Warenkorb-Seite die aktualisierte Liste der Elemente entsprechend aktualisiert.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualisieren und entfernen die Artikel im Warenkorb

Auf der *ShoppingCart.aspx* Seite sehen, da Sie Steuerelemente wurden für die Menge eines Elements zu aktualisieren und Entfernen eines Elements hinzugefügt. Fügen Sie nun den Code, der diese Steuerelemente werden kann.

1. In **Projektmappen-Explorer**öffnen die *ShoppingCartActions.cs* Datei die *Logik* Ordner.
2. Fügen Sie folgenden Code in Gelb zu markiert die *ShoppingCartActions.cs* Klassendatei:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Die `UpdateShoppingCartDatabase` Methode namens aus der `UpdateCartItems` Methode für die *ShoppingCart.aspx.cs* Seite, die Logik zum Aktualisieren oder Entfernen von Elementen aus dem Warenkorb enthält. Die `UpdateShoppingCartDatabase` Methode durchläuft alle Zeilen in der shopping Cart-Liste. Wenn ein Artikel im Warenkorb dieses markiert entfernt werden sollen, oder die Menge kleiner als 1, die `RemoveItem` Methode wird aufgerufen. Hingegen wird für den Artikel im Warenkorb dieses überprüft wird, wenn der `UpdateItem` Methode wird aufgerufen. Nachdem der Artikel im Warenkorb dieses aktualisiert oder entfernt wurde, werden die Änderungen in der Datenbank gespeichert.

Die `ShoppingCartUpdates` Struktur wird verwendet, um alle dem Artikel im Warenkorb enthalten. Die `UpdateShoppingCartDatabase` -Methode verwendet die `ShoppingCartUpdates` Struktur, die bestimmen, ob keines der Elemente, die aktualisiert oder entfernt werden müssen.

Im nächsten Tutorial, verwenden Sie die `EmptyCart` Methode zum Deaktivieren der Warenkorb Einkaufswagen, nach dem Kauf von Produkten. Jetzt verwenden Sie jedoch die `GetCount` -Methode, die Sie gerade hinzugefügt, um haben die *ShoppingCartActions.cs* Datei, um zu bestimmen, wie viele Elemente im Warenkorb sind.

### <a name="adding-a-shopping-cart-counter"></a>Hinzufügen eines Shopping Cart-Leistungsindikators

Damit um den Benutzer die Gesamtanzahl der Elemente im Warenkorb anzeigen zu können, fügen Sie einen Leistungsindikator an, die *Site.Master* Seite. Dieser Leistungsindikator wird auch als einen Link zum Einkaufswagen fungieren.

1. In **Projektmappen-Explorer**öffnen die *Site.Master* Seite.
2. Ändern Sie das Markup, durch den warenkorblink klicken Indikator hinzufügen, wie in Gelb im Navigationsbereich-Abschnitt dargestellt werden, damit es wie folgt aussieht:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Aktualisieren Sie den Code-Behind der als Nächstes die *Site.Master.cs* Datei durch Hinzufügen des Codes, die wie folgt in gelb hervorgehoben:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Bevor Sie die Seite als HTML gerendert wird die `Page_PreRender` Ereignis wird ausgelöst. In der `Page_PreRender` Handler, der die Gesamtzahl der dem Einkaufswagen bestimmt, indem die `GetCount` Methode. Der zurückgegebene Wert wird hinzugefügt, um die `cartCount` Spanne, die im Markup enthalten die *Site.Master* Seite. Die `<span>` Tags können die inneren Elemente ordnungsgemäß gerendert werden soll. Wenn Seite der Website angezeigt wird, wird die einkaufswagensumme angezeigt. Benutzer kann auch die einkaufswagensumme zum Einkaufswagen ordnungsgemäß anzeigen klicken.

## <a name="testing-the-completed-shopping-cart"></a>Testen den abgeschlossenen Einkaufswagen

Sie können die Anwendung jetzt, um anzuzeigen, wie Sie hinzufügen, löschen und Aktualisieren von Elementen in den Einkaufswagen ausführen. Die einkaufswagensumme spiegeln die Gesamtkosten für alle Elemente im Warenkorb.

1. Drücken Sie **F5**, um die Anwendung auszuführen.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Wählen Sie **Autos** im Navigationsmenü der Kategorie.
3. Klicken Sie auf die **Add To Cart** neben das erste Produkt.   
 Die *ShoppingCart.aspx* Seite wird angezeigt, mit der Gesamtsumme der Bestellungen.
4. Wählen Sie **Ebenen** im Navigationsmenü der Kategorie.
5. Klicken Sie auf die **Add To Cart** neben das erste Produkt.
6. Die Menge des ersten Elements in den Einkaufswagen auf 3 festgelegt, und wählen Sie die **Element entfernen** Sie das Kontrollkästchen für das zweite Element.<a id="a"></a>
7. Klicken Sie auf die **aktualisieren** Schaltfläche, um die Warenkorb-Seite zu aktualisieren und die neue Gesamtsumme der Bestellungen anzuzeigen. 

    ![Warenkorb - Warenkorb-Update](shopping-cart/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie einen Einkaufswagen gelegt hat, für die Wingtip Toys Web Forms-beispielanwendung erstellt. In diesem Tutorial haben Sie Entity Framework Code First, datenanmerkungen, stark typisierte Datensteuerelemente und modellbindung verwendet.

Der Einkaufswagen unterstützt hinzufügen, löschen und Aktualisieren von Elementen, die der Benutzer zum Kauf ausgewählt hat. Zusätzlich zur Implementierung der shopping Cart-Funktionalität, haben Sie erfahren, wie Sie im Artikel im Warenkorb anzeigen einer **GridView** steuern und die Gesamtsumme der Bestellungen berechnen.

## <a name="addition-information"></a>Zusätzliche Informationen

[Übersicht über die ASP.NET Session State](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Zurück](display_data_items_and_details.md)
> [Weiter](checkout-and-payment-with-paypal.md)
