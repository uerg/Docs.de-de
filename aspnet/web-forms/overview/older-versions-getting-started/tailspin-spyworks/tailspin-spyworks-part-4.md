---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Teil 4: Auflisten von Produkten | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 4 deckt Auflisten von Produkten mit der GridView Vertr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a>Teil 4: Angebot Produkte
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 4 werden die Produkte Angebot des GridView-Steuerelements behandelt.


## <a id="_Toc260221670"></a>  Auflisten von Produkten mit des GridView-Steuerelements

Fangen wir implementieren unsere ProductsList.aspx-Seite "Mit der rechten Maustaste auf" auf unserer Projektmappe, und wählen "Hinzufügen" und "Neues Element".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Wählen Sie "Web Form mithilfe Masterseite", und geben Sie einen Seitennamen ProductsList.aspx".

Klicken Sie auf "Hinzufügen".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Als nächstes wählen Sie den Ordner "Formatvorlagen", in dem wir die Site.Master-Seite platziert, und wählen Sie ihn aus dem Fenster "Inhalt des Ordners".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Klicken Sie auf "Ok", um die Seite zu erstellen.

Die Datenbank wird mit Produktdaten aufgefüllt, wie unten dargestellt.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Nach der Erstellung unserer Seite wir eine Datenquelle für die Entität erneut Zugriff auf diese Produktdaten verwenden, aber in diesem Fall müssen wir die Product-Entitäten auswählen, und wir müssen Sie die Elemente begrenzen, die zurückgegeben werden, um nur die für die ausgewählte Kategorie.

Um dies zu erreichen erfahren EntityDataSource zum automatischen Generieren der WHERE-Klausel aus, und geben wir die WhereParameter.

Beachten Sie, dass bei der Menüelemente in unserem "Product Category-Menü" erstellt haben wir dynamisch den Link erstellt die Abfragezeichenfolge für jeden Link der CatagoryID hinzugefügt wird. Wir werden diesen QueryString-Parameter den WHERE-Parameter Ableiten der Entität-Datenquelle informieren.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Konfigurieren Sie anschließend das ListView-Steuerelement, um eine Liste von Produkten anzuzeigen. Um eine optimale Leistung Warenkorb erstellen wir mehrere präzise Funktionen in jedes einzelne Produkt angezeigt, in unserem ListVew komprimiert werden.

- Der Produktname wird ein Link auf der Produkt-Detailansicht sein.
- Das Produkt Preis wird angezeigt.
- Ein Bild des Produkts angezeigt, und wir dynamisch wähle das Image aus einem Katalog-Images-Verzeichnis in der vorliegenden Anwendung.
- Es enthält einen Link, um sofort des bestimmten Produkts zum Einkaufswagen hinzufügen.

Im folgenden wird das Markup für die gegebene Instanz des ListView-Steuerelement.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Dynamisch erstellen wir einige Links für die einzelnen Produkte angezeigt werden.

Bevor wir eigenen neuen Seite testen müssen wir darüber hinaus die Verzeichnisstruktur für das Produkt Katalog-Images erstellen, die wie folgt.

![](tailspin-spyworks-part-4/_static/image1.png)

Nach unserer Produktbilder zugegriffen werden können wir unser Produkt Listenseite testen.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Homepage der Website klicken Sie auf einen der Links Liste Kategorie.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Nun müssen wir die Seite "ProductDetials.apsx" und die AddToCart-Funktionalität zu implementieren.

Mithilfe des Datei -&gt;neu erstellen Sie einen Seitennamen ProductDetails.aspx über Website für die Gestaltungsvorlage aus, wie zuvor.

Wir verwenden erneut EntityDataSource-Steuerelement, um den bestimmten Produktdatensatz in der Datenbank zugreifen, und verwenden wir ein ASP.NET FormView-Steuerelement zum Anzeigen von der Produktdaten wie folgt.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Machen Sie sich keine Gedanken Sie, wenn die Formatierung für Sie ein wenig seltsam aussieht. Das Markup oben verlässt Platz im Anzeigelayout für eine Reihe von Funktionen, die wir später implementieren.

Der Einkaufswagen repräsentiert die komplexere Logik in der vorliegenden Anwendung. Um zu beginnen, mithilfe des Datei -&gt;neu zum Erstellen eines Seitenblob MyShoppingCart.aspx aufgerufen.

Beachten Sie, dass wir den Namen ShoppingCart.aspx nicht auswählen.

Die Datenbank enthält eine Tabelle mit dem Namen "ShoppingCart". Wenn wir ein Entity Data Model generiert wurde eine Klasse für jede Tabelle in der Datenbank erstellt. Das Entity Data Model generiert daher eine Entitätsklasse, die mit dem Namen "ShoppingCart". Wir konnten das Modell bearbeiten, sodass konnten wir verwenden Sie diesen Namen für unsere shopping Cart-Implementierung oder für unseren Anforderungen zu erweitern, aber wir stattdessen, wählen Sie einfach einen Namen, der den Konflikt zu vermeiden, wird deaktiviert wird.

Es ist auch Folgendes zu beachten, dass wir einen einfache Einkaufswagen erstellen und die shopping Cart-Logik mit shopping Cart-Anzeige einbetten. Es empfiehlt sich auch unsere warenkorbsoftware in einer vollständig separaten Business-Ebene zu implementieren.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-3.md)
> [Weiter](tailspin-spyworks-part-5.md)
