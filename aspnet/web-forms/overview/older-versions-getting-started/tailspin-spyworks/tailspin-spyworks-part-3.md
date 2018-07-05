---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Teil 3: Layout- und Kategoriemenü | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 3 beschreibt das Hinzufügen von Layout und ein kategoriemenü.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817524"
---
<a name="part-3-layout-and-category-menu"></a>Teil 3: Layout- und Kategoriemenü
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 3 beschreibt das Hinzufügen von Layout und ein kategoriemenü.


## <a id="_Toc260221669"></a>  Hinzufügen von einigen Layout- und eine Kategoriemenü

In unserem Masterseite der Website fügen wir ein DIV-Element für die Spalte der linken Seite, die unser Produkt kategoriemenü enthalten soll.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Beachten Sie, dass die gewünschte Ausrichtung und sonstigen Formatierungen von CSS-Klasse bereitgestellt werden, die wir die Datei "Style.CSS" hinzugefügt.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Das Product Category-Menü wird dynamisch erstellt zur Laufzeit durch Abfragen der Commerce-Datenbank, für vorhandene Produktkategorien und erstellen die Menüelemente und entsprechenden links.

Zu diesem Zweck verwenden wir zwei von ASP zu gewährleisten. NET leistungsstarke Data-Steuerelemente. Das Steuerelement "Entity Data Source" und das Steuerelement "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Wir wechseln Sie zu "Design View", und die Hilfsprogramme zur Konfiguration der unsere Steuerelemente verwenden.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Legen wir die EntityDataSource-ID-Eigenschaft für EDS\_Kategorie\_Menü, und klicken Sie auf "Datenquelle konfigurieren".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Wählen Sie die CommerceEntities-Verbindung, die für uns erstellt wurde, wenn wir das Entitätsdatenmodell für die Quelle für unsere Commerce-Datenbank erstellt haben, und klicken Sie auf "Weiter".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Wählen Sie den Namen der Entitätenmenge für "Categories", und lassen Sie die übrigen Standardeinstellungen der Optionen. Klicken Sie auf "Fertig stellen".

Jetzt legen wir die ID-Eigenschaft von der Instanz des ListView-Steuerelements, die wir auf unserer Seite aus, um die ListView platziert\_ProductsMenu und seinem Hilfsobjekt aktivieren.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Jedoch können wir Optionen zur aufgabensteuerung zum Formatieren der Anzeige der Daten und Formatierung, unsere im Menü erstellen, benötigen nur einfache Markup damit wir den Code in der Datenquellensicht eingeben, wird.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Bitte beachten Sie die Anweisung "Eval": &lt;% # Eval("CategoryName") %&gt;

Der ASP.NET-Syntax &lt;% # %&gt; ist eine Kurzform-Konvention, die weist die Laufzeit ausgeführt wird, was in enthalten ist, und die Ergebnisse "in Zeile".

Rufen Sie Eval("CategoryName") angewiesen, die für den aktuellen Eintrag in der gebundenen Auflistung von Datenelementen, den Wert der Elementnamen Entity Model "CatagoryName". Dies ist die kürzere Syntax für ein sehr leistungsfähiges Feature.

Können die Anwendung nun ausführen.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Beachten Sie, dass unser Produkt kategoriemenü wird nun angezeigt, wenn wir über eine Kategorie Menüelemente zeigen sehen wir, die im Menü Element Link verweist auf eine Seite, die wir noch implementieren müssen, mit dem Namen ProductsList.aspx und, wir ein Argument für eine dynamische Abfrage erstellt haben, enthält die  Kategorie-Id.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-2.md)
> [Weiter](tailspin-spyworks-part-4.md)
