---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Teil 8: Einkaufswagen mit Ajax-Updates | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 8 behandelt Einkaufswagen mit Ajax-Updates.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369795"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Teil 8: Einkaufswagen mit Ajax-Updates
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 8 behandelt Einkaufswagen mit Ajax-Updates.


Lassen wir Benutzern Alben in den Einkaufswagen zu platzieren, ohne zu registrieren, aber sie müssen als Gäste vollständige Auschecken registrieren. Die Einkaufs- und Auschecken wird in zwei Controller getrennt werden: eine ShoppingCart-Controller, der können anonym eine Warenkorb Elemente hinzufügt und ein Auschecken-Controller, der den Kassenvorgang behandelt. Wir beginnen mit den Warenkorb legen in diesem Abschnitt, und anschließend den Kassenvorgang im folgenden Abschnitt zu erstellen.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Hinzufügen der Warenkorb, Reihenfolge und OrderDetail-Modell-Klassen

Unsere Prozesse Warenkorb und Auschecken veranlasst einiger neuer Klassen verwenden. Mit der rechten Maustaste in den Ordner "Models", und fügen Sie eine Warenkorb-Klasse (Cart.cs) durch den folgenden Code.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Diese Klasse ist recht ähnlich für andere Personen, die wir bisher, mit Ausnahme des Attributs [Key] für die Datensatz-ID-Eigenschaft verwendet haben. Unsere im Warenkorb hat einen Zeichenfolgenbezeichner, der mit dem Namen CartID können anonymes einkaufen, aber die Tabelle enthält einen ganze Zahl Primärschlüssel, der mit dem Namen Datensatz-ID ein. Gemäß der Konvention erwartet, dass Entity Framework Code First, dass der primäre Schlüssel für eine Tabelle mit dem Namen Einkaufswagen wird entweder CartId oder die ID, aber wir einfach, die über Anmerkungen oder Code überschreiben können, wenn wir möchten. Dies ist ein Beispiel, wie wir verwendet die einfachen Konventionen in Entity Framework Code First-Wenn diese uns erfüllen, aber wir sind nicht von ihnen eingeschränkt, wenn dies nicht der Fall.

Als Nächstes fügen Sie eine Order-Klasse (Order.cs) durch den folgenden Code hinzu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Diese Klasse dient die Zusammenfassung und Übermittlung von Informationen zu einer Bestellung nachverfolgen. **Nicht noch kompiliert**, da es sich um eine Navigationseigenschaft OrderDetails besitzt, der von einer Klasse abhängig ist dies nicht getan haben wir noch erstellt. Korrigieren Sie wir jetzt durch das Hinzufügen eine Klasse OrderDetail.cs, Hinzufügen des folgenden Codes benannt.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Wir erstellen eine letzte Aktualisierung um unsere MusicStoreEntities Klasse "dbsets" einbeziehen, die diese neuen Modellklassen, darunter auch einen "DbSet" verfügbar machen&lt;Interpreten&gt;. Die aktualisierte MusicStoreEntities-Klasse angezeigt wird, wie unten.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Verwalten von der Geschäftslogik Warenkorb

Als Nächstes erstellen wir die ShoppingCart-Klasse im Ordner "Models". Die ShoppingCart-Modell verarbeitet die Datenzugriff auf die Warenkorb-Tabelle. Darüber hinaus wird es der Geschäftslogik zum Hinzufügen und Entfernen von Elementen aus dem Warenkorb verarbeitet.

Da wir keine Benutzer zum Registrieren für ein Konto aus, um Elemente zu ihrem Einkaufswagen hinzufügen möchten, zugewiesen Benutzer einen temporären eindeutigen Bezeichner (mithilfe einer GUID oder den globally unique Identifier) beim Zugreifen auf den Einkaufswagen. Wir speichern diese ID mithilfe der ASP.NET-Sitzung-Klasse.

*Hinweis: Die ASP.NET-Sitzung ist eine bequeme Möglichkeit, benutzerspezifische Informationen speichern, die ablaufen wird, nachdem sie die Site zu verlassen. Während unsachgemäße Verwendung des Sitzungsstatus für größere Sites die Auswirkungen auf die Leistung haben kann, funktioniert die einfache Verwendung auch für Demonstrationszwecke.*

Die ShoppingCart-Klasse macht die folgenden Methoden verfügbar:

**AddToCart** ein Album als Parameter akzeptiert und fügt es der Einkaufswagen des Benutzers hinzu. Da die Warenkorb-Tabelle Menge für jedes Album nachverfolgt, enthält sie Logik, um bei Bedarf eine neue Zeile erstellen oder nur die Menge erhöht, wenn der Benutzer bereits eine Kopie des Albums aufgegeben hat.

**RemoveFromCart** übernimmt ein Album-ID und entfernt sie aus dem Einkaufswagen des Benutzers. Wenn der Benutzer nur eine Kopie des Albums im Einkaufswagen haben, wird die Zeile entfernt.

**EmptyCart** entfernt alle Elemente aus dem Einkaufswagen eines Benutzers.

**GetCartItems** Ruft eine Liste der CartItems für Sie Anzeige- oder ab.

**GetCount** Ruft ein die Gesamtanzahl von Alben, die ein Benutzer im Einkaufswagen hat.

**GetTotal** berechnet die Gesamtkosten für alle Elemente im Warenkorb.

**CreateOrder** wandelt den Einkaufswagen in einen Auftrag während der Phase Auschecken.

**GetCart** ist eine statische Methode, wodurch unsere-Controller, ein Warenkorb-Objekt abzurufen. Er verwendet den **GetCartId** Methode zum Behandeln von lesen die CartId aus der Sitzung des Benutzers. Die Methode GetCartId erfordert HttpContextBase, sodass des Benutzers CartId mit der Sitzung des Benutzers zu lesen.

Hier ist die vollständige **ShoppingCart-Klasse**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Unser Shopping Cart-Controller, müssen einige komplexen Informationen zu seiner Ansichten kommunizieren, die nicht ordnungsgemäß für unsere Modellobjekte zugeordnet sind. Wir wollen nicht ändern, die Modelle, die Ansichten anpassen; Modellklassen sollte die Domäne, nicht in der Benutzeroberfläche darstellen. Eine Lösung wäre, übergeben die Informationen an den Ansichten die ViewBag-Klasse verwenden, haben wir mit den Informationen der Store Manager-Dropdownliste aus, aber eine Vielzahl von Informationen über "ViewBag" übergeben, ruft schwierig zu verwalten.

Dies ist die Verwendung der *"ViewModel"* Muster. Verwendung dieses Musters erstellen wir die stark typisierte Klassen, die für unseren bestimmten Ansicht-Szenarien optimiert sind, und das Verfügbarmachen der Eigenschaften für das dynamische Werte bzw. den Inhalt von unserem Ansichtsvorlagen benötigt werden. Die Controllerklassen können klicken Sie dann Auffüllen und übergeben diese Klassen Ansicht optimiert, unsere ansichtsvorlage verwenden. Dadurch wird typsicherheit, Überprüfung und IntelliSense-Editor in Vorlagen anzeigen.

Wir erstellen zwei Ansichtsmodelle für die Verwendung in den Einkaufswagen-Controller: die ShoppingCartViewModel übernimmt den Inhalt der Einkaufswagen eines Benutzers, und die ShoppingCartRemoveViewModel wird verwendet, um Informationen zur Bestätigung angezeigt, wenn ein Benutzer etwas entfernt in den Einkaufswagen.

Erstellen wir einen neuen Ordner "ViewModels" im Stammverzeichnis des unseres Projekts halber organisiert an. Mit der rechten Maustaste in des Projekts, wählen Sie Add / neuen Ordner.

![](mvc-music-store-part-8/_static/image1.jpg)

Benennen Sie den Ordner "ViewModels".

![](mvc-music-store-part-8/_static/image1.png)

Fügen Sie anschließend die ShoppingCartViewModel-Klasse im Ordner "ViewModels" ein. Er verfügt über zwei Eigenschaften: eine Liste der im Warenkorb, und ein decimal-Wert, den Gesamtpreis für alle Elemente im Warenkorb enthalten soll.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Fügen Sie nun die ShoppingCartRemoveViewModel zum Ordner "ViewModels" mit den folgenden vier Eigenschaften hinzu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Die Einkaufswagencontroller

Der Controller Warenkorb hat drei Hauptfunktionen: Hinzufügen von Elementen zu einem Einkaufswagen und Entfernen von Elementen aus dem Warenkorb Elemente im Warenkorb anzeigen. Verwenden der drei Klassen wir machen wird gerade erstellt haben: ShoppingCartViewModel ShoppingCartRemoveViewModel "und" ShoppingCart. Wie bei den StoreController und StoreManagerController fügen wir ein Feld, um eine Instanz der MusicStoreEntities befindet.

Fügen Sie einen neuen Warenkorb-Controller, auf das Projekt mithilfe der Vorlage der leeren Controller.

![](mvc-music-store-part-8/_static/image2.png)

Hier ist der vollständige ShoppingCart-Controller. Die Aktionen für Index "und" Controller hinzufügen, sollte sehr vertraut aussehen. Die Controller-Aktionen entfernen und CartSummary behandeln zwei spezielle Fälle, die wir im folgenden Abschnitt eingehen werde.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>AJAX-Updates mit jQuery

Wir erstellen neben eine Shopping Cart-Indexseite, die stark typisiert, die ShoppingCartViewModel und verwendet die Listenansicht-Vorlage, die mit derselben Methode wie vor.

![](mvc-music-store-part-8/_static/image3.png)

Jedoch wird statt einer Html.ActionLink, um Elemente aus dem Einkaufswagen entfernen, jQuery verwendet, um das Click-Ereignis für alle Links in dieser Ansicht die HTML-RemoveLink Klasse "verknüpfen". Anstatt veröffentlichen das Formular, wird diese Click-Ereignishandler nur einen AJAX-Rückruf an unsere RemoveFromCart Controlleraktion vornehmen. Die RemoveFromCart ein JSON serialisierte Ergebnis zurückgegeben, die unsere jQuery-Rückruf dann analysiert und führt vier schnellen Updates auf der Seite mit jQuery:

- 1. Entfernt das Album gelöschte, aus der Liste
- 2. Aktualisiert die Warenkorb-Anzahl in der Kopfzeile
- 3. Zeigt eine Update-Nachricht an den Benutzer
- 4. Aktualisiert den Gesamtpreis Warenkorb

Da das Remove-Szenario durch ein Ajax-Rückruf in die Ansicht "Index" verarbeitet wird, benötigen wir nicht für RemoveFromCart Aktion eine zusätzliche Ansicht. Hier ist der vollständige Code für die /ShoppingCart/Index-Ansicht:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Um dies zu testen, müssen wir unsere warenkorbsoftware Elemente hinzugefügt werden können. Wir aktualisieren unsere **Store Details** Ansicht, um eine Schaltfläche "Element zum Einkaufswagen hinzufügen" enthalten. Während wir schon dabei sind, zählen wir einige zusätzliche Informationen zum Album die wir hinzugefügt haben, da wir in dieser Ansicht zum zuletzt aktualisiert: Genre, Künstler, Preis und Albumcover. Der aktualisierte Code der Store-Details anzeigen angezeigt wird, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Jetzt können wir klicken Sie auf, über den Store und hinzufügen und Entfernen von Alben in und aus unsere warenkorbsoftware zu testen. Führen Sie die Anwendung, und navigieren Sie zu der Store-Index.

![](mvc-music-store-part-8/_static/image4.png)

Klicken Sie anschließend auf ein Genre, um eine Liste der Alben anzuzeigen.

![](mvc-music-store-part-8/_static/image5.png)

Durch Klicken auf eine Albumtitel jetzt zeigt unserer aktualisierten Album Detailansicht, darunter die Schaltfläche "Element zum Einkaufswagen hinzufügen".

![](mvc-music-store-part-8/_static/image6.png)

Klicken Sie auf die Schaltfläche "Element zum Einkaufswagen hinzufügen" zeigt unsere Shopping Cart-Index-Ansicht mit der shopping Cart Zusammenfassungsliste.

![](mvc-music-store-part-8/_static/image7.png)

Nach dem Laden Sie Ihr Warenkorb ist leer, können Sie auf der Warenkorb Link entfernen, das Ajax-Update zu Ihrem Warenkorb anzeigen klicken.

![](mvc-music-store-part-8/_static/image8.png)

Wir haben das fertige eines funktionsfähiges Einkaufswagen, nicht registrierte Benutzer Elemente Einkaufswagen hinzufügen können. Im folgenden Abschnitt werden wir zu registrieren und Ausführen des Auscheckvorgangs ermöglichen.


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-7.md)
> [Weiter](mvc-music-store-part-9.md)
