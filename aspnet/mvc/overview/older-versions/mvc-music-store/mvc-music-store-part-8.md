---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Teil 8: Einkaufswagen mit Ajax-Updates | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 8 deckt Einkaufswagen mit Ajax-Updates.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 75e1dff96f8b56d74c28ff9d522f4766fbad669f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Teil 8: Einkaufswagen mit Ajax-Updates
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 8 deckt Einkaufswagen mit Ajax-Updates.


Bieten wir Benutzern die Alben in ihrem Einkaufswagen zu platzieren, ohne diesen zu registrieren, aber sie müssen als Gäste vollständige Auschecken registrieren. Die Warenkorb und Auschecken wird in zwei Controller getrennt werden: eine ShoppingCart-Controller dadurch anonym Hinzufügen von Elementen zu einem Einkaufswagen und einen Checkout-Controller die des Auscheckvorgangs behandelt. Wir müssen mit dem Einkaufswagen in diesem Abschnitt beginnen, und erstellen dann des Auscheckvorgangs im folgenden Abschnitt.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Hinzufügen von Modellklassen Einkaufswagen, Bestell- und OrderDetail

Unsere Prozesse im Einkaufswagen und Auschecken stellen einige neuer Klassen verwenden. Mit der rechten Maustaste in den Ordner Models, und fügen Sie eine Klasse mit dem Einkaufswagen (Cart.cs), durch den folgenden Code.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Diese Klasse ist ziemlich ähnliche an andere Personen, die wir mit Ausnahme des Attributs [Key] für die Eigenschaft RecordId bisher verwendet haben. Unsere Warenkorb werden einen Zeichenfolgenbezeichner, der mit dem Namen CartID zum Zulassen anonymer Warenkorb haben, aber die Tabelle enthält einen ganze Zahl Primärschlüssel, der mit dem Namen RecordId. Gemäß der Konvention wird von Entity Framework Code First erwartet, dass der Primärschlüssel für eine Tabelle mit dem Namen Einkaufswagen, CartId oder ID, aber wir leicht, die über Anmerkungen oder Code überschreiben können gegebenenfalls. Dies ist ein Beispiel, wie wir verwenden können einfache Konventionen in Entity Framework Code First, wenn sie uns anpassen, aber es sind nicht von ihnen eingeschränkt, wenn diese nicht.

Als Nächstes fügen Sie eine Order-Klasse (Order.cs) durch den folgenden Code hinzu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Diese Klasse wird nachverfolgt, Zusammenfassung und die Übermittlung von Informationen für eine Bestellung. **Er wird nicht kompiliert werden noch**, da er eine Navigationseigenschaft OrderDetails aufweist, der von einer Klasse abhängt wir noch nicht noch erstellt. Wir beheben, die jetzt durch Hinzufügen von Klasse OrderDetail.cs, den folgenden Code hinzufügen.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Stellen wir eine letzte Aktualisierung um unsere MusicStoreEntities-Klasse, um DbSets einzuschließen, die diese neue Modellklassen, darunter auch ein ' DbSet ' verfügbar zu machen&lt;Interpreten&gt;. Die aktualisierte MusicStoreEntities-Klasse angezeigt wird, als unten.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Verwalten von der Geschäftslogik Einkaufswagen

Als Nächstes erstellen wir im Ordner Models die ShoppingCart-Klasse. Die ShoppingCart-Modell verarbeitet den Datenzugriff auf die Warenkorb-Tabelle. Darüber hinaus wird es der Geschäftslogik zum Hinzufügen und Entfernen von Elementen aus dem Einkaufswagen behandelt werden.

Da wir nicht benötigen Benutzer zum Registrieren für ein Konto nur für Elemente in seinen Einkaufswagen legen hinzufügen möchten, wir weist Benutzer einen temporären eindeutigen Bezeichner (mithilfe eines GUID oder ein global eindeutiger Bezeichner) beim Zugriff auf des Einkaufswagen Sinn macht. Speichern wir diese ID mithilfe der ASP.NET Session-Klasse.

*Hinweis: Die ASP.NET-Sitzung ist eine bequeme Möglichkeit, Sie benutzerspezifische Informationen speichern, die abläuft, wenn sie die Website verlassen haben. Missbrauch des Sitzungsstatus Leistungseinbußen an größeren Standorten kann zwar aufweisen, funktioniert unsere hell verwenden ebenfalls zu Demonstrationszwecken.*

Die ShoppingCart-Klasse stellt die folgenden Methoden:

**AddToCart** ein Album als Parameter akzeptiert und der Einkaufswagen des Benutzers hinzugefügt. Da die Warenkorb-Tabelle Menge für jedes Album überwacht wird, enthält sie Logik zum Erstellen Sie ggf. einer neuen Zeile oder die Menge nur erhöht, wenn der Benutzer bereits eine Kopie des Albums aufgegeben hat.

**RemoveFromCart** übernimmt ein Album-ID und entfernt sie aus der Einkaufswagen des Benutzers. Wenn der Benutzer nur eine Kopie des Albums in ihrem Einkaufswagen hat, wird die Zeile entfernt.

**EmptyCart** entfernt alle Elemente aus dem Einkaufswagen des Benutzers.

**GetCartItems** Ruft eine Liste von CartItems für die Anzeige oder Verarbeitung ab.

**GetCount** Ruft ein die Gesamtanzahl von Alben ein Benutzers im Einkaufswagen ist.

**GetTotal** berechnet die Gesamtkosten für alle Elemente im Warenkorb.

**CreateOrder** konvertiert den Einkaufswagen einer Bestellung Phase Auschecken.

**GetCart** ist eine statische Methode, wodurch unsere Controller, ein Warenkorb-Objekt abzurufen. Er verwendet die **GetCartId** Methode, lesen die CartId aus der Sitzung des Benutzers zu behandeln. Die GetCartId-Methode erfordert HttpContextBase, sodass der Benutzer CartId mit Sitzung des Benutzers zu lesen.

Hier wird die vollständige **ShoppingCart Klasse**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Unser Shopping Cart-Controller müssen einige komplexen Informationen zu den Ansichten zu übergeben, die unserer Modellobjekte ordnungsgemäß zugeordnet ist. Wir möchten unsere Modelle entsprechend unserer Ansichten ändern; Modellklassen sollten unsere Domäne, die nicht in der Benutzeroberfläche darstellen. Eine Lösung wäre, übergeben Sie die Informationen an unsere Sichten, die die ViewBag-Klasse verwenden, haben wir mit den Informationen der Speicher-Manager-Dropdownliste aus, sondern übergeben eine Vielzahl von Informationen über ViewBag ruft schwer zu verwalten.

Eine Lösung für diese ist die Verwendung der *ViewModel* Muster. Bei Verwendung dieses Muster erstellen wir stark typisierter Klassen, die für unsere bestimmte Ansicht Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte/Content durch unsere Ansichtsvorlagen erforderlich machen. Unsere Controllerklassen können Auffüllen und diese Ansicht optimiert Klassen an unsere anzeigen, die zu verwendende Vorlage übergeben. Dies ermöglicht die typsicherheit, kompilierzeitüberprüfung und Editor IntelliSense in Vorlagen anzeigen.

Wir erstellen zwei Ansichtsmodelle für die Verwendung in unserer Warenkorb-Controller: die ShoppingCartViewModel, den Inhalt der Einkaufswagen des Benutzers gespeichert werden, und die ShoppingCartRemoveViewModel wird verwendet, um Bestätigungsinformationen zur angezeigt, wenn ein Benutzer ein Element entfernt aus Einkaufswagen.

Erstellen Sie einen neuen Ordner für die ViewModels wir im Stammverzeichnis des unsere Projekt organisiert Dinge zu. Mit der rechten Maustaste des Projekts, wählen Sie die Add / neuen Ordner.

![](mvc-music-store-part-8/_static/image1.jpg)

Benennen Sie den Ordner ViewModels ein.

![](mvc-music-store-part-8/_static/image1.png)

Als Nächstes fügen Sie die ShoppingCartViewModel-Klasse, im Ordner "ViewModels". Sie verfügt über zwei Eigenschaften: eine Liste der Warenkorb und einen decimal-Wert, den Gesamtpreis für alle Elemente im Warenkorb enthalten soll.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Fügen Sie jetzt die ShoppingCartRemoveViewModel zum ViewModels Ordner, mit den folgenden vier Eigenschaften.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Die Einkaufswagencontroller

Der Einkaufswagen Controller hat drei Hauptfunktionen: Hinzufügen von Elementen zu einem Einkaufswagen, Entfernen von Elementen aus dem Einkaufswagen und Anzeigen von Elementen in den Warenkorb. Verwenden von drei Klassen wir werden vorgenommen erstellte: ShoppingCartViewModel ShoppingCartRemoveViewModel und ShoppingCart. Wie bei den StoreController und StoreManagerController fügen wir ein Feld, um eine Instanz des MusicStoreEntities enthalten.

Fügen Sie einen neuen Shopping Cart-Controller zum Projekt mit der Vorlage der leeren Controller.

![](mvc-music-store-part-8/_static/image2.png)

Hier ist der vollständige ShoppingCart-Controller. Die Aktionen "Index" und "Controller hinzufügen sollten sehr bekannt vorkommen. Die Controller-Aktionen entfernen und CartSummary behandeln zwei Sonderfälle, die im folgenden Abschnitt erläutert.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>AJAX-Updates mit jQuery

Wir müssen neben eine Shopping Cart Indexseite erstellen, die ist stark typisiert werden, um die ShoppingCartViewModel und die Listenansicht-Vorlage, die mit derselben Methode wie vor verwendet.

![](mvc-music-store-part-8/_static/image3.png)

Anstatt eine Html.ActionLink Elemente aus dem Einkaufswagen zu entfernen, müssen wir jedoch verwenden jQuery "Einrichten von Click-Ereignis für alle Links in dieser Sicht die HTML-RemoveLink Klasse Netzwerkdaten". Anstatt die Buchung des Formulars, wird diese Click-Ereignishandler nur einen AJAX-Rückruf an unsere RemoveFromCart Controlleraktion vornehmen. Die RemoveFromCart gibt ein Ergebnis JSON serialisiert unsere jQuery-Rückruf dann analysiert und vier schnelle Updates auf der Seite unter Verwendung von jQuery ausführt:

- 1. Entfernt das gelöschte Album aus der Liste
- 2. Aktualisiert die Warenkorb-Anzahl in der Kopfzeile
- 3. Zeigt dem Benutzer eine Änderungsnachricht
- 4. Aktualisiert den Gesamtpreis Warenkorb

Da das Remove-Szenario durch ein Ajax-Rückruf in die Indexansicht behandelt wird, erforderlich wir eine zusätzliche Ansicht für nicht RemoveFromCart Aktion. Hier wird der vollständige Code für die /ShoppingCart/Index-Ansicht:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Um dies zu testen, müssen wir unsere warenkorbsoftware Elemente hinzugefügt werden können. Aktualisieren wir unsere **Store Details** Sicht auf die Schaltfläche "Zum Warenkorb hinzufügen" enthalten. Während es bei dieser Gelegenheit können wir enthalten einige zusätzliche Informationen zum Album, der es hinzugefügt haben, da wir in dieser Ansicht zuletzt aktualisiert: "Genre", Interpret, Preis und Album Art. Der aktualisierte Code der Store Details anzeigen angezeigt wird, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Jetzt können wir über den Store auf und hinzufügen und Entfernen von Alben zu und von unsere warenkorbsoftware zu testen. Führen Sie die Anwendung, und navigieren Sie zu der Columnstore-Index.

![](mvc-music-store-part-8/_static/image4.png)

Klicken Sie dann auf eine "Genre" zum Anzeigen einer Liste von Alben auf.

![](mvc-music-store-part-8/_static/image5.png)

Albumtitel jetzt auf zeigt unseren aktualisierte Album Detailansicht, einschließlich der Schaltfläche "Zum Warenkorb hinzufügen".

![](mvc-music-store-part-8/_static/image6.png)

Klicken auf die Schaltfläche "Zum Warenkorb hinzufügen" zeigt unseren Shopping Cart-Index-Ansicht mit der shopping Cart Zusammenfassungsliste.

![](mvc-music-store-part-8/_static/image7.png)

Nach dem Laden von Ihrem Einkaufswagen, können Sie auf Entfernen aus dem Einkaufswagen Link des Ajax-Updates zu Ihrem Warenkorb anzeigen klicken.

![](mvc-music-store-part-8/_static/image8.png)

Wir haben eine funktionierende Einkaufswagen dadurch nicht registrierte Benutzer das Hinzufügen von Elementen zum Einkaufswagen erstellt. Im folgenden Abschnitt werden wir ihnen das Registrieren und Ausführen des Auscheckvorgangs gewähren.


>[!div class="step-by-step"]
[Zurück](mvc-music-store-part-7.md)
[Weiter](mvc-music-store-part-9.md)
