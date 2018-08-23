---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Teil 4: Auflisten von Produkten | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 4 enthält die Auflistung von Produkten mit der GridView-Vertr....
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827456"
---
<a name="part-4-listing-products"></a>Teil 4: Auflistung von Produkten
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 4 enthält die Auflistung von Produkten mit GridView-Steuerelement.


## <a id="_Toc260221670"></a>  Auflistung von Produkten mit GridView-Steuerelement

Auf unserer Lösung und dann "Hinzufügen" und "Neues Element" Implementieren von unserer Seite ProductsList.aspx "Rechten Maustaste auf den" beginnen.

![](tailspin-spyworks-part-4/_static/image1.jpg)

Wählen Sie "Web Form mithilfe von Masterseite", und geben Sie einen Seitennamen ProductsList.aspx".

Klicken Sie auf "Hinzufügen".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Als nächstes wählen Sie den "Stile"-Ordner, in dem wir die Site.Master-Seite platziert, und wählen Sie ihn in das Fenster "Inhalt des Ordners".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Klicken Sie auf "Ok", um die Seite zu erstellen.

Unsere Datenbank wird mit Produktdaten aufgefüllt, wie unten dargestellt.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Nach der Erstellung unserer Seite verwenden wir erneut eine Datenquelle für die Entität auf die Produktdaten zugreifen, aber in diesem Fall müssen wir die Product-Entitäten auswählen, und wir müssen die Elemente einzuschränken, um nur die für die ausgewählte Kategorie zurückgegeben werden.

Um dies zu erreichen wir verraten EntityDataSource zum automatischen Generieren der WHERE-Klausel aus, und wir legen die WhereParameter.

Sie erinnern sich, dass beim Erstellen der Menüelemente in unsere "Product Category-Menü" Wir dynamisch die Verknüpfung erstellt die Abfragezeichenfolge für jeden Link der CatagoryID hinzugefügt. Es informiert, dass der Entity-Datenquelle, QueryString-Parameter den WHERE-Parameter abgeleitet.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Als Nächstes konfigurieren wir das ListView-Steuerelement, um eine Liste von Produkten anzuzeigen. Um eine optimale Einkaufserlebnis erstellen wir mehrere präzise Funktionen in jedes einzelne Produkt angezeigt, in unserem ListVew komprimiert werden.

- Der Name des Produkts werden ein Link zur Detailansicht des Produkts.
- Den Preis des Produkts wird angezeigt.
- Ein Bild des Produkts wird angezeigt, und dynamisch wähle das Image aus einer Katalog-Bildverzeichnis in unserer Anwendung.
- Es enthält einen Link, um sofort das jeweiligen Produkt zum Einkaufswagen hinzuzufügen.

Hier ist das Markup für die gegebene Instanz des ListView-Steuerelement.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Dynamisch erstellen wir einige Links für die einzelnen Produkte angezeigt werden.

Bevor wir eigene neue Seite testen müssen wir darüber hinaus die Verzeichnisstruktur für das Produkt Katalog-Images erstellen, die wie folgt.

![](tailspin-spyworks-part-4/_static/image1.png)

Sobald unser Produktbilder zugegriffen werden kann, können wir unsere Produktseite für die Liste testen.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Klicken Sie auf einen der Links die Kategorie-Liste, auf der Startseite der Website.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Jetzt müssen wir die ProductDetials.apsx-Seite und die AddToCart-Funktionalität zu implementieren.

Verwenden Sie Datei -&gt;neu, um einen Seitennamen ProductDetails.aspx mithilfe der Website-Masterseite, wie schon zuvor zu erstellen.

Verwenden wir erneut einen EntityDataSource-Steuerelement auf den bestimmten Produktdatensatz in der Datenbank zugreifen, und wir verwenden ein ASP.NET FormView-Steuerelement, um die Produktdaten wie folgt anzeigen.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Machen Sie sich keine Gedanken Sie, wenn die Formatierung für Sie ein wenig seltsam aussieht. Das Markup oben bleibt Platz im Anzeigelayout für eine Reihe von Features, die wir später implementiert werden.

Der Warenkorb stellt die komplexere Logik in unserer Anwendung dar. Verwenden Sie zum Einstieg Dateien&gt;neu, um eine Seite mit dem MyShoppingCart.aspx zu erstellen.

Beachten Sie, dass wir nicht den Namen ShoppingCart.aspx auswählen.

Unsere Datenbank enthält eine Tabelle namens "ShoppingCart". Wenn wir ein Entity Data Model generiert wurde eine Klasse für jede Tabelle in der Datenbank erstellt. Aus diesem Grund generiert der Entity Data Model eine Entitätsklasse, die mit dem Namen "ShoppingCart". Es konnte das Modell bearbeitet werden, damit wir verwenden Sie diesen Namen für die shopping Cart-Implementierung oder für unsere Anforderungen erweitern können, aber entscheiden wir uns wird stattdessen, wählen Sie einfach einen Namen, der den Konflikt vermieden wird.

Es ist auch zu beachten Sie, dass wir einen einfachen Warenkorb erstellen und betten die shopping Cart-Logik, mit der shopping Cart-Anzeige. Wir können auch auswählen, unsere warenkorbsoftware in einer vollständig separaten Business-Ebene zu implementieren.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-3.md)
> [Weiter](tailspin-spyworks-part-5.md)
