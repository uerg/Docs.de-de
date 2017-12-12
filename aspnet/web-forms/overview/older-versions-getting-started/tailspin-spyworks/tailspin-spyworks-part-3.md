---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "Teil 3: Layout und die Kategorie Menü | Microsoft Docs"
author: JoeStagner
description: "Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 3 beschreibt das Hinzufügen von Layout und ein Kategorie-Menü."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a>Teil 3: Layout und die Kategorie Menü
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 3 beschreibt das Hinzufügen von Layout und ein Kategorie-Menü.


## <a id="_Toc260221669"></a>Einige Layout und ein Menü Kategorie hinzufügen

Auf unserer Website Masterseite fügen wir ein div-Tag für die linke Spalte, die unsere Product Category-Menü enthält.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Beachten Sie, dass die gewünschte Ausrichtung und andere Formatierung von CSS-Klasse bereitgestellt werden, die wir unsere Datei "Style.css" hinzugefügt.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Das Product Category-Menü wird dynamisch erstellt zur Laufzeit durch Abfragen der Commerce-Datenbank, für die vorhandene Produktkategorien und erstellen die Menüelemente und entsprechenden verknüpft.

Zu diesem Zweck verwenden wir zwei von ASP. NET leistungsstarke Data-Steuerelemente. Das Steuerelement "Entity Data Source" und "ListView"-Steuerelement.

![](tailspin-spyworks-part-3/_static/image1.jpg)

Wir wechseln Sie zu "Entwurfsansicht", und verwenden Sie die Hilfsprogramme, um unsere Steuerelemente zu konfigurieren.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Legen Sie die EntityDataSource-ID-Eigenschaft für EDS\_Kategorie\_Menü, und klicken Sie auf "Datenquelle konfigurieren".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Wählen Sie die CommerceEntities-Verbindung, die für uns erstellt wurde, wenn wir die Quelle des Entitätsdatenmodells für unsere Commerce-Datenbank erstellt, und klicken Sie auf "Weiter".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Wählen Sie den Namen der Entitätenmenge "Kategorien", und übernehmen Sie die übrigen Optionen als Standard. Klicken Sie auf "Fertig stellen".

Nun legen Sie die ID-Eigenschaft der ListView-Steuerelement-Instanz, die wir auf unserer Seite ListView platziert\_ProductsMenu, und aktivieren Sie die Hilfsfunktion.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Obwohl wir konnte Steuerelementoptionen verwenden, um die Anzeige der Daten zu formatieren und Formatierung, unsere im Menü erstellen wird nur erfordern einfache Markup damit wir den Code in der Quellansicht eingeben, wird.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Bitte beachten Sie die Anweisung "Eval": &lt;% # Eval("CategoryName") %&gt;

Der ASP.NET-Syntax &lt;% # %&gt; ist eine Kurzschreibweise-Konvention, die weist das Laufzeitmodul an, wie in enthalten ist, und gibt die Ergebnisse aus "in Zeile" ausführen.

Die Anweisung Eval("CategoryName") angewiesen, die für den aktuellen Eintrag in der gebundenen Auflistung von Datenelementen, rufen den Wert des Entitätsmodell-Elementnamen "CatagoryName" ab. Dies ist die kurze Syntax für eine sehr leistungsstarke Funktion.

Können die Anwendung jetzt ausführen.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Beachten Sie, dass unsere Product Category-Menü jetzt angezeigt wird. wenn es für eine Kategorie Menüelemente zeigen sehen wir die im Menü Element-Link verweist auf eine Seite, die wir noch implementieren müssen mit dem Namen ProductsList.aspx und enthält es ein Zeichenfolgenargument dynamischen Abfrage erstellt haben die  Id der Kategorie.

>[!div class="step-by-step"]
[Zurück](tailspin-spyworks-part-2.md)
[Weiter](tailspin-spyworks-part-4.md)
