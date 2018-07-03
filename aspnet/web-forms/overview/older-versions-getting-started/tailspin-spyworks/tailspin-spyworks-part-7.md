---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Teil 7: Hinzufügen von Funktionen | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 7 fügt zusätzliche Funktionen wie das Konto übe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389175"
---
<a name="part-7-adding-features"></a>Teil 7: Hinzufügen von Features
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 7 fügt zusätzliche Funktionen wie das Konto überprüfen, produktbesprechungen und "beliebtesten Elemente" und "auch erworbenen" Benutzersteuerelemente hinzu.


## <a id="_Toc260221673"></a>  Hinzufügen von Funktionen

Obwohl Benutzer unserem Katalog durchsuchen können, platzieren Sie Elemente in ihren Einkaufskorb legen, und abgeschlossen Sie des Auscheckvorgangs, gibt es zahlreiche unterstützende Funktionen, die wir einfügen, um die Verbesserung unserer Website.

1. Überprüfen Sie das Konto (Liste Bestellungen platziert und zeigen Sie Details.)
2. Fügen Sie einige spezifischen Kontext-Inhalt, auf die Titelseite.
3. Fügen Sie ein Feature, das Benutzern lesen Sie die Produkte im Katalog hinzu.
4. Erstellen Sie ein Benutzersteuerelement zum Anzeigen der beliebtesten Elemente und Ort, die steuern, auf der Titelseite ein.
5. Erstellen eines Benutzersteuerelements "Auch gekauft", und fügen sie die Seite für Produktdetails hinzu.
6. Hinzufügen eines Kontakts Seite.
7. Hinzufügen einer zu Seite.
8. Globaler Fehler

## <a id="_Toc260221674"></a>  Konto überprüfen

Erstellen Sie zwei ASPX-Seiten, die eine benannte OrderList.aspx und die andere benannte OrderDetails.aspx im Ordner "Konto"

Ähnlich wie zuvor schon nutzen OrderList.aspx GridView und EntityDataSoure.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Die EntityDataSoure wählt die Datensätze aus der Tabelle Orders, die den Benutzernamen, das gefiltert (siehe die WhereParameter) die wir in einer Sitzungsvariablen festgelegt, wenn die Benutzeranmeldung in ist.

Beachten Sie auch diese Parameter werden in der HyperlinkField der GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Dazu geben Sie den Link zum Anzeigen Details für jedes Produkt, das Feld "OrderID" als QueryString-Parameter auf der Seite "OrderDetails.aspx" angeben.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Wir verwenden ein EntityDataSource-Steuerelement den Zugriff auf die Bestellungen und einem FormView-Steuerelement zum Anzeigen der Daten und eine andere EntityDataSource mit einer GridView-Ansicht zum Anzeigen aller der Bestellung Einzelposten.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

In der CodeBehind-Datei (OrderDetails.aspx.cs) haben wir zwei kleine Teile Wartungsaufgaben.

Zunächst müssen wir sicherstellen, dass OrderDetails immer ein "OrderID" ruft.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Wir müssen auch zum Berechnen und die Gesamtsumme aus die Einzelposten des Auftrags anzeigen.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Auf der Startseite

Fügen Sie einige statische Inhalte auf der Seite "default.aspx" ein.

Zunächst erstelle ich einen Ordner "Content" und darin einen Ordner "Images" (und ich werde ein Bild auf der Homepage verwendet werden.)

Fügen Sie in den Platzhalter unteren Rand der Seite "default.aspx" das folgende Markup hinzu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Produktbesprechungen

Zunächst fügen eine Schaltfläche mit einem Link zu einem Formular wir, die wir verwenden können, die eine produktprüfung eingeben.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Beachten Sie, dass die "ProductID" in der Abfragezeichenfolge übergeben werden

Nächste fügen Sie die Seite mit dem Namen ReviewAdd.aspx

Diese Seite wird die ASP.NET AJAX Control Toolkit verwenden. Wenn Sie noch nicht getan haben, damit Sie es aus herunterladen [DevExpress](http://devexpress.com/act) und Anleitungen zum Einrichten des Toolkits für die Verwendung mit Visual Studio hier [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Ziehen Sie im Entwurfsmodus Steuerelemente und Validierungssteuerelemente aus der Toolbox, und erstellen Sie eine Formulierung wie unten angegeben.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Das Markup wird wie folgt aussehen.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Nun, da wir Reviews eingeben können, können diese Bewertungen auf der Seite angezeigt werden sollen.

Fügen Sie dieses Markup auf der Seite "ProductDetails.aspx" hinzu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Die Anwendung jetzt ausführen, und navigieren Sie zu einem Produkt zeigt die Produktinformationen, einschließlich der Überprüfungen durch den Kunden.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Beliebte ItemsControl (Erstellen von Benutzersteuerelementen)

Um Umsatzsteigerung auf Ihrer Website fügen wir einige Funktionen auf "vorgeschlagene Sell" gebräuchlichsten und verwandte Produkte hinzu.

Der erste dieser Funktionen wird eine Liste mit den beliebtesten Produkt in unserem Produktkatalog.

Wir erstellen eine "User-Control" um die beliebtesten Elemente auf der Startseite der Anwendung anzuzeigen. Da dies ein Steuerelement sein wird, können wir es auf einer beliebigen Seite durch einfaches Ziehen und Ablegen des Steuerelements in Visual Studio Designer auf jeder Seite, die uns gefallen.

Klicken Sie im Projektmappen-Explorer für Visual Studio mit der rechten Maustaste auf den Namen der Projektmappe, und erstellen Sie ein neues Verzeichnis namens "Steuerelemente". Obwohl es nicht erforderlich ist, ist, helfen wir unser Projekt, das durch das Erstellen von unserem Benutzersteuerelemente im Verzeichnis "Steuerelemente" zu halten.

Mit der rechten Maustaste auf den Ordner "Steuerelemente", und wählen Sie "Neues Element":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Geben Sie einen Namen für das Steuerelement von "PopularItems". Beachten Sie, dass die Dateierweiterung für Benutzersteuerelemente ASCX nicht aspx.

Unsere beliebten Elemente Benutzersteuerelement wird wie folgt definiert werden.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Hier verwenden wir eine Methode, die wir noch nicht in dieser Anwendung verwendet haben. Wir verwenden das Repeater-Steuerelement, und anstelle von Datenquellen-Steuerelement sind wir das Repeater-Steuerelement binden, um die Ergebnisse einer LINQ to Entities-Abfrage.

In den Code hinter der das Steuerelement das machen wir wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Beachten Sie auch diese wichtige Zeile am Anfang Markup des Steuerelements.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Da die am häufigsten verwendeten Elemente pro minütlich nicht ändert, können wir eine schmerzenden Direktive zur Verbesserung der Leistung der Anwendung hinzufügen. Diese Anweisung bewirkt, dass die Steuerelemente Code nur ausgeführt werden, wenn die zwischengespeicherte Ausgabe des Steuerelements abläuft. Andernfalls wird die zwischengespeicherte Version der die Ausgabe des Steuerelements verwendet werden.

Jetzt alles, was schon alles ist das neue Steuerelement auf unserer Seite Default.aspc enthalten.

Mithilfe von ziehen und ablegen, um eine Instanz des Steuerelements in der Spalte öffnen das Standard-Formular zu platzieren.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Wenn wir unsere Anwendung auf der Startseite ausführen zeigt die am häufigsten verwendeten Elemente jetzt.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Auch gekauft" zu steuern (Benutzersteuerelemente mit Parametern)

Das zweite Steuerelement, das wir erstellen dauert vorgeschlagenen auf die nächste Stufe durch Hinzufügen von Kontext Spezifität verkaufen.

Die Logik zum Berechnen Sie die Elemente "Auch gekauft" ist nicht trivial.

Unsere "Auch gekauft"-Steuerelement die OrderDetails-Datensätze, die (zuvor gekauft) für den aktuell ausgewählten "ProductID" auswählen und ziehen Sie die Auftrags-ID für jede eindeutige Bestellung, die gefunden wurde.

Klicken Sie dann werden wir al wählen Sie die Produkte aus allen Bestellungen und Sum Mengen erworben haben. Wir sortieren die Produkte, indem die Summe der Menge und zeigt die ersten fünf Elemente.

Angesichts die Komplexität dieser Logik, werden wir diesen Algorithmus als eine gespeicherte Prozedur implementieren.

Das T-SQL für die gespeicherte Prozedur lautet wie folgt aus:

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Beachten Sie, dass diese gespeicherte Prozedur (SelectPurchasedWithProducts), die in der Datenbank vorhanden waren, als wir haben es in unserer Anwendung, und wenn wir, dass zusätzlich zu den Tabellen und Sichten, die es erforderlich, das Entity Data Model angegebenen Entity Data Model generierten eingefügt Diese gespeicherte Prozedur sollte enthalten werden.

Die gespeicherte Prozedur aus dem Entity Data Model für den Zugriff auf müssen wir die Funktion zu importieren.

Einen Doppelklick auf das Entity Data Model im Projektmappen-Explorer im Designer zu öffnen, und öffnen den Browser, und klicken Sie dann mit der rechten Maustaste im Designer, und wählen Sie "Funktionsimport hinzufügen".

![](tailspin-spyworks-part-7/_static/image1.png)

Auf diese Weise wird dieses Dialogfeld geöffnet.

![](tailspin-spyworks-part-7/_static/image2.png)

Füllen Sie die Felder, wie Sie oben sehen, die "SelectPurchasedWithProducts" auswählen, und verwenden Sie den Namen der Prozedur für den Namen unserer importierten Funktion.

Klicken Sie auf "Ok".

Danach, die wir einfach die gespeicherte Prozedur programmieren können, wie wir jedes andere Element im Modell.

Erstellen Sie ein neues Benutzersteuerelement mit dem Namen AlsoPurchased.ascx also in den Ordner "Steuerelemente".

Das Markup für dieses Steuerelement wird an das Steuerelement PopularItems vertraut sein.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Der wichtige Unterschied ist, dass, die nicht die Zwischenspeicherung der Ausgabe werden, da des Elements, das gerendert werden vom Produkt voneinander unterscheiden.

Die "ProductID" wird eine "Property" für das Steuerelement.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

In PreRender Ereignishandler des Steuerelements wir Eed drei Dinge erreichen.

1. Stellen Sie sicher, dass die "ProductID" festgelegt ist.
2. Überprüfen Sie, ob alle Produkte, die zuvor erworben wurden, mit der aktuellen Instanz.
3. Ausgabe einige Elemente wie #2 festgelegt.

Beachten Sie, wie einfach es ist, rufen Sie die gespeicherte Prozedur über das Modell.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Nachdem ermittelt wurde, gibt es "auch erworben werden" können wir einfach die Repeater, die von der Abfrage zurückgegebenen Ergebnisse binden.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Gäbe es keine Elemente "auch gekauft" zeigen wir einfach andere beliebte Elemente aus unserem Katalog.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Die "Auch gekauft" Elemente anzeigen, öffnen Sie die Seite ProductDetails.aspx, und ziehen das AlsoPurchased-Steuerelement aus dem Projektmappen-Explorer, sodass sie an dieser Position im Markup angezeigt wird.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Auf diese Weise wird einen Verweis auf das Steuerelement am oberen Rand der Seite "ProductDetails" erstellt.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Da das Benutzersteuerelement AlsoPurchased eine Reihe von "ProductID" erfordert werden wir die ProductID-Eigenschaft, der das Steuerelement mit einer Eval-Anweisung für das aktuelle Modell-Datenelement der Seite festgelegt.

![](tailspin-spyworks-part-7/_static/image3.png)

Wenn wir erstellen, und jetzt ausführen, und navigieren Sie zu einem Produkt sehen Sie die Elemente "Auch gekauft".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-6.md)
> [Weiter](tailspin-spyworks-part-8.md)
