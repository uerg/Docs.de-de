---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Teil 7: Hinzufügen von Funktionen | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 7 fügt zusätzliche Funktionen, z. B. übe Konto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>Teil 7: Hinzufügen von Funktionen
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 7 fügt zusätzliche Funktionen wie Konto überprüfen, produktprüfungen, und "gängige Elemente" und "auch erworbenen" Benutzersteuerelemente hinzu.


## <a id="_Toc260221673"></a>  Hinzufügen von Funktionen

Wenn Benutzer unsere Katalog durchsuchen können, platzieren Sie Elemente im Einkaufswagen, und abgeschlossen Sie des Auscheckvorgangs, stehen Sie eine Reihe unterstützen Funktionen, wir aufnimmt, um unsere Website zu verbessern.

1. Überprüfen Sie das Konto (Liste Bestellungen platziert und zeigen Sie Details an.)
2. Einige bestimmten Kontext-Inhalt auf der Startseite hinzufügen.
3. Hinzufügen einer Funktion können Benutzer überprüfen Sie die Produkte im Katalog.
4. Erstellen Sie ein benutzerdefiniertes Steuerelement zum Anzeigen von beliebten Elemente und fügen Sie dieses, die steuern, auf der Startseite.
5. Erstellen eines Benutzersteuerelements "Auch" gekauft ", und fügen Sie es auf der Detailseite hinzu.
6. Hinzufügen eines Kontakts Seite.
7. Hinzufügen einer zu Seite.
8. Globaler Fehler

## <a id="_Toc260221674"></a>  Account-Überprüfung

Erstellen Sie zwei ASPX-Seiten, die einen benannten OrderList.aspx und die anderen benannten OrderDetails.aspx im Ordner "Konto"

OrderList.aspx wird GridView und EntityDataSoure nutzen, fast so wie wir zuvor haben.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Die EntityDataSoure wählt die Datensätze aus der Orders-Tabelle gefiltert wird, auf den Benutzernamen (siehe die WhereParameter) die wir in einer Sitzungsvariablen festgelegt, wenn die Benutzeranmeldung's.

Beachten Sie auch diese Parameter in der HyperlinkField der GridView ein:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Dazu geben Sie den Link, um die Reihenfolge Detailansicht für jedes Produkt, das Feld "OrderID" als eine QueryString-Parameter auf der Seite "OrderDetails.aspx" angeben.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Die Aufträge und eine FormView zum Anzeigen der Auftragsdaten und anderen EntityDataSource mit GridView zum Anzeigen der Zeilenelemente für alle der Reihenfolge zuzugreifen, verwenden Sie ein EntityDataSource-Steuerelement.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

In der Code-Behind-Datei (OrderDetails.aspx.cs) haben wir zwei wenig Bits des Housekeeping.

Zunächst müssen wir sicherstellen, dass OrderDetails immer eine OrderId ruft.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Wir müssen auch zu berechnen und die Gesamtsumme aus die Einzelposten des Auftrags anzuzeigen.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Auf der Startseite

Fügen Sie einige statische Inhalte auf der Seite "default.aspx" ein.

Zunächst erstellen ich einen Ordner "Content" und darin ein Ordner "Abbilder" (und ich werde sind, ein Bild auf der Homepage verwendet werden.)

Fügen Sie das folgende Markup hinzu, in den Platzhalter unteren Rand der Seite "default.aspx".

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Produktübersicht

Zunächst fügen eine Schaltfläche mit einem Link zu einem Formular wir, die es verwenden können, um eine produktprüfung einzugeben.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Beachten Sie, dass wir die "ProductID" in der Abfragezeichenfolge übergeben werden

Nächste fügen Sie die Seite mit dem Namen ReviewAdd.aspx

Diese Seite wird das ASP.NET AJAX-Steuerelement-Toolkit verwenden. Wenn Sie noch nicht getan haben, damit Sie es aus herunterladen [DevExpress](http://devexpress.com/act) besteht die Anleitung zum Einrichten des Toolkits für die Verwendung mit Visual Studio hier [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Ziehen Sie im Entwurfsmodus Steuerelemente und Validierungssteuerelemente aus der Toolbox, und erstellen Sie ein Format wie die folgende.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Das Markup sieht in etwa wie folgt.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Nun, dass wir Reviews eingeben können, können diese Berichte auf der Seite "Product" angezeigt.

Fügen Sie diesem Markup auf der Seite "ProductDetails.aspx".

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Die Anwendung jetzt ausführen und das Navigieren zu einem Produkt zeigt die Produktinformationen, einschließlich kundenbewertungen.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Gängige Elementsteuerelement (Benutzersteuerelemente erstellen)

Um auf Ihrer Website Umsatz steigern, werden wir eine Reihe von Funktionen zu "vorgeschlagene Sell" gängigen oder verwandte Produkte hinzufügen.

Die erste dieser Funktionen wird eine Liste mit den gängigeren Produkt in unserer Produktkatalog sein.

Es wird ein "Benutzersteuerelement" zum Anzeigen der meistverkaufte Elemente auf der Startseite der Anwendung erstellt. Da dies ein Steuerelement sein wird, können wir es auf einer beliebigen Seite durch einfach ziehen und Ablegen von das Steuerelement in Visual Studio-Designer auf eine andere Seite, die wir gefällt.

In Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf den Namen der Projektmappe, und erstellen Sie ein neues Verzeichnis mit dem Namen "Steuerelemente". Während es nicht notwendig ist, unterstützen wir unsere Projekt durch das Erstellen von unseren Benutzersteuerelemente im Verzeichnis "Steuerelemente" beibehalten.

Mit der rechten Maustaste auf den Ordner, und wählen Sie "Neues Element":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Geben Sie einen Namen für das Steuerelement des "PopularItems". Beachten Sie, dass die Dateierweiterung für Benutzersteuerelemente .ascx nicht aspx.

Unsere Benutzersteuerelement für gängige Elemente werden wie folgt definiert werden.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Hier verwenden wir eine Methode, die wir noch nicht in dieser Anwendung verwendet haben. Verwenden wir die wiederholungsmodul-Steuerelement, und anstatt ein Datenquellen-Steuerelement sind wir Wiederholungsmodul-Steuerelement binden, um die Ergebnisse einer LINQ to Entities-Abfrage.

In den Code hinter der unserer Kontrolle erfolgt, die wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Beachten Sie auch diese wichtigen Zeile am oberen Rand des Steuerelements Markup.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Da die am häufigsten verwendeten Elemente nicht regelmäßig zur ändert können wir eine schmerzenden Richtlinie zum Verbessern der Leistung der Anwendung hinzufügen. Diese Richtlinie bewirkt, dass die Steuerelemente Code nur ausgeführt werden, wenn die zwischengespeicherte Ausgabe des Steuerelements abläuft. Andernfalls wird die zwischengespeicherte Version der Ausgabe des Steuerelements verwendet werden.

Jetzt haben wir führen lediglich unsere Default.aspc-Seite unserer neuen Kontrolle einschließt.

Mithilfe von ziehen und ablegen, um eine Instanz des Steuerelements in der open-Spalte der unsere Standardformular platzieren.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Zeigt die am häufigsten verwendeten Elemente jetzt Wenn wir unsere Anwendung auf der Startseite ausgeführt werden.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Auch gekauft" steuern (Benutzersteuerelemente mit Parametern)

Dauert vorgeschlagenen Verkauf an die nächste Ebene durch Hinzufügen von Kontext Besonderheit der zweites Benutzersteuerelement, das wir erstellen müssen.

Die Logik zum Berechnen der obersten Elemente "Auch" gekauft "ist nicht trivial.

Unserer Kontrolle "Auch gekauft" wählen die OrderDetails-Datensätze, die (zuvor gekauft) für die aktuell ausgewählte "ProductID", und ziehen Sie die OrderIDs für jede eindeutige Bestellung, die gefunden wird.

Wählen Sie dann werden wir al Produkte von diesen Aufträge und die Summe der Mengen erworben haben. Wir die Produkte nach der Menge Summe sortieren und zeigt die obersten fünf Elemente.

Dieser Algorithmus wird angesichts die Komplexität von diese Logik, wie eine gespeicherte Prozedur implementiert.

Die T-SQL für die gespeicherte Prozedur lautet wie folgt.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Beachten Sie, dass diese gespeicherte Prozedur (SelectPurchasedWithProducts) in der Datenbank vorhanden waren, wenn wir sie enthalten in der vorliegenden Anwendung, und wenn wir, die zusätzlich zu den Tabellen und Sichten, die es benötigt generiert, das Entity Data Model angegebenen Entity Data Model Diese gespeicherte Prozedur sollte enthalten sein.

Zugriff auf die gespeicherte Prozedur aus dem Entity Data Model müssen wir die Funktion zu importieren.

Doppelklick auf dem Entity Data Model im Projektmappen-Explorer im Designer zu öffnen, und öffnen die Model-Browser, und klicken Sie dann mit der rechten Maustaste im Designer, und wählen Sie "Funktionsimport hinzufügen".

![](tailspin-spyworks-part-7/_static/image1.png)

Auf diese Weise wird dieses Dialogfeld geöffnet.

![](tailspin-spyworks-part-7/_static/image2.png)

Füllen Sie die Felder, wie Sie oben sehen die "SelectPurchasedWithProducts" auswählen, und verwenden Sie den Namen der Prozedur für den Namen der importierten Funktion.

Klicken Sie auf "Ok".

Dies, die wir einfach für die gespeicherte Prozedur programmieren können, wie wir eines beliebigen anderen Elements im Modell möglicherweise getan haben.

Deshalb in unserem "Steuerelemente" Ordner erstellen ein neuen Benutzersteuerelements, das mit dem Namen AlsoPurchased.ascx.

Das Markup für dieses Steuerelement wird das Steuerelement PopularItems sehr bekannt vorkommen.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Der wesentliche Unterschied ist, die nicht das Zwischenspeichern der Ausgabe werden, da des Elements, das gerendert werden vom Produkt abweichen.

Die "ProductID" wird "Property" an das Steuerelement.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Das Steuerelement PreRender-Ereignishandler wir Eed drei Schritte auszuführen.

1. Stellen Sie sicher, dass die "ProductID" festgelegt ist.
2. Feststellen Sie, ob alle Produkte aus, die erworben wurden, mit der aktuellen Aktivität.
3. Geben Sie einige Elemente wie #2 festgelegt.

Beachten Sie, wie einfach es ist, rufen Sie die gespeicherte Prozedur über das Modell.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Nachdem ermittelt wurde, gibt es "auch gekauft werden" können wir einfach Repeater binden, für die von der Abfrage zurückgegebenen Ergebnisse.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Wenn es keine Elemente "auch gekauft wurden" zeigen wir andere gängige Elemente einfach aus unserem Katalog.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Die "Auch gekauft" Elemente anzeigen, öffnen Sie die Seite ProductDetails.aspx und ziehen die AlsoPurchased im Projektmappen-Explorer, damit es an dieser Position in das Markup angezeigt wird.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Auf diese Weise wird einen Verweis auf das Steuerelement am oberen Rand der Seite "Produktdetails" erstellt.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Da das Benutzersteuerelement AlsoPurchased diverse "ProductID" erfordert wird die Eigenschaft "ProductID" unserer Kontrolle mithilfe einer Eval-Anweisung für das aktuelle Modell-Datenelement der Seite festgelegt.

![](tailspin-spyworks-part-7/_static/image3.png)

Wenn wir erstellen, und jetzt auszuführen, und navigieren Sie zu einem Produkt sehen wir die Elemente "Auch gekauft".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-6.md)
> [Weiter](tailspin-spyworks-part-8.md)
