---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Teil 10: Abschließende Updates für die Navigation und das Websitedesign – Zusammenfassung | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 10 sind die abschließende Updates für die Navigation und S...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836806"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Teil 10: Abschließende Updates für die Navigation und das Websitedesign – Zusammenfassung
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 10 sind die abschließende Updates für die Navigation und das Websitedesign – Zusammenfassung.


Wir haben alle wichtige Funktionen für unsere Website abgeschlossen, aber es gibt noch einige Funktionen der Website-Navigation, auf der Startseite und der Store durchsuchen Seite hinzufügen.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Erstellen die Shopping Cart teilweise Zusammenfassungsansicht

Die Anzahl der Elemente im Einkaufswagen des Benutzers für die gesamte Website verfügbar gemacht werden soll.

![](mvc-music-store-part-10/_static/image1.png)

Wir können einfach implementieren dies durch das Erstellen einer Teilansicht der unsere Site.master hinzugefügt wird.

Wie zuvor gezeigt, enthält die ShoppingCart-Controller eine CartSummary Aktionsmethode die Teilansicht zurückgibt:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Klicken Sie zum Erstellen der Teilansicht CartSummary mit der rechten Maustaste auf den Ordner Views/ShoppingCart, und wählen Sie die Ansicht hinzufügen. Benennen Sie die Ansicht CartSummary, und aktivieren Sie das Kontrollkästchen "Eine Teilansicht erstellen" aus, wie unten dargestellt.

![](mvc-music-store-part-10/_static/image2.png)

Die Teilansicht CartSummary ist ganz einfach – es ist nur ein Link zur der die Anzahl der Elemente zusammen in den ShoppingCart Indexansicht. Der vollständige Code für CartSummary.cshtml lautet wie folgt aus:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Wir können eine Teilansicht in einer beliebigen Seite in der Website, einschließlich des Masters Standort mithilfe der Methode Html.RenderAction einschließen. RenderAction muss der Name der Aktion ("CartSummary") und des Controllernamens ("ShoppingCart") wie unten angegeben.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Vor dem Hinzufügen des Standorts Layout, auch erstellen im Menü "Genre" wir damit wir alle unsere Site.master-Updates auf einmal erstellen können.

## <a name="creating-the-genre-menu-partial-view"></a>Erstellen die Teilansicht für "Genre" im Menü

Wir können machen es viel einfacher für unsere Benutzer, über den Store zu navigieren, durch das Hinzufügen einer "Genre" im Menü der alle Genres verfügbar in unseres neuen Speichers aufgeführt sind.

![](mvc-music-store-part-10/_static/image3.png)

Wir befolgen die gleichen Schritte auch eine Teilansicht GenreMenu erstellen, und klicken Sie dann wir können hinzufügen beide mit dem übergeordneten Standort. Fügen Sie zunächst die folgende GenreMenu-Controlleraktion, die StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Diese Aktion gibt eine Liste der Genres, die von der Teilansicht, angezeigt wird, welche als Nächstes erstellt wird.

*Hinweis: Wir haben das Attribut [ChildActionOnly] an diese Controlleraktion hinzugefügt gibt an, dass nur diese Aktion aus einer Teilansicht verwendet werden soll. Dieses Attribut wird verhindert, dass die Controlleraktion, die durch Navigieren zu /Store/GenreMenu ausgeführt werden. Dies ist nicht erforderlich für Teilansichten, aber es hat sich bewährt, da Sie sicherstellen, dass unsere Controlleraktionen wie geplant verwendet werden soll. Wir sind auch zurück PartialView statt anzeigen, können Sie die Ansichts-Engine, wissen, dass das Layout sollten nicht für diese Ansicht verwendet werden, wie es in den anderen Ansichten eingefügt wird.*

Mit der rechten Maustaste auf die GenreMenu Controlleraktion aus, und erstellen Sie eine Teilansicht mit dem Namen GenreMenu die stark typisiert ist, verwenden die Datenklasse "Genre" anzeigen, wie unten dargestellt.

![](mvc-music-store-part-10/_static/image4.png)

Aktualisieren Sie den Code anzeigen, für die GenreMenu Teilansicht zum Anzeigen von Elementen, die eine unsortierte Liste wie folgt mit.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Layout der Website auf unsere Teilansichten aktualisieren

Wir können unsere Teilansichten hinzufügen, um das Layout der Website (/Ansichten/Shared/\_Layout.cshtml) durch Aufrufen von Html.RenderAction(). Wir fügen sie beide an sowie einige zusätzliche Markup angezeigt, wie unten dargestellt:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Wenn die Anwendung ausgeführt wird, werden wir das Genre im linken Navigationsbereich, und die Zusammenfassung der Warenkorb am oberen sehen.

## <a name="update-to-the-store-browse-page"></a>Aktualisieren Sie auf der Seite "Store durchsuchen"

Die Seite Store durchsuchen ist funktionsfähig, aber nicht sehr gut aussehen. Wir aktualisieren können Sie die Seite, um die Alben in einem besseren Layout anzeigen, indem Sie den Anzeigecode (gefunden in /Views/Store/Browse.cshtml) wie folgt aktualisieren:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Wir erstellen hier Url.Action statt Html.ActionLink verwenden werden, sodass wir die spezielle Formatierung auf den Link, um die Grafik Album enthalten anwenden können.

*Hinweis: Wir werden eine generische Albumtitel für diese Alben angezeigt. Diese Informationen werden in der Datenbank gespeichert und kann über den Store Manager bearbeitet werden. Sie sind Willkommen, Ihre eigene Grafik hinzufügen.*

Nun werden wir ein Genre durchsuchen können, wir die Alben, die in einem Raster mit dem Album Bildmaterial dargestellt angezeigt.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualisieren die Homepage zum Anzeigen von Selling Alben oben

Wir möchten unsere meistverkaufte Alben auf der Startseite auf den Umsatz steigern, feature. Wir stellen einige Updates zu unseren HomeController zu behandeln, die aus, und auch einige zusätzlichen Grafiken hinzufügen.

Zunächst fügen wir eine Navigationseigenschaft auf unsere Album-Klasse, sodass EntityFramework weiß, dass sie zugeordnet sind. Die letzten Zeilen der unsere **Album** Klasse sollte jetzt wie folgt aussehen:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Hinweis: Dies benötigen Sie eine using-Anweisung im System.Collections.Generic-Namespace zu importieren.*

Zunächst fügen wir ein StoreDB-Feld und die using-Anweisungen, wie unsere anderen Controller MvcMusicStore.Models. Als Nächstes fügen die folgende Methode HomeController wir die unsere Datenbank zum Suchen von Top-selling Alben gemäß OrderDetails abfragt.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Dies ist eine private Methode, da wir nicht, als eine Controlleraktion verfügbar zu machen möchten. Wir werden es im HomeController aus Gründen der Einfachheit einschließlich, aber es wird empfohlen, die Ihre Geschäftslogik in separaten Dienstklassen nach Bedarf zu verschieben.

Vorhanden, können wir die Index-Controlleraktion zum Abfragen von den Top 5 mit dem Verkauf von Alben, und gibt sie zurück zur Ansicht aktualisieren.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Der vollständige Code für die aktualisierte HomeController wird wie unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Abschließend müssen wir unsere Ansicht "Start Index" zu aktualisieren, sodass sie eine Liste der Alben anzeigen kann, durch den Typ des Modells aktualisieren und im unteren Bereich die Albumliste hinzugefügt. Wir übernehmen die Gelegenheit, um auch eine Überschrift und eine heraufstufung-Abschnitt der Seite hinzugefügt werden.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Nun, wenn wir die Anwendung ausführen, sehen wir unsere aktualisierte Startseite mit Top von selling Alben und unsere Werbe-Meldung.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Schlussbemerkung

Wir haben gesehen, dass ASP.NET MVC erleichtert usw. mit Zugriff auf die Datenbank, Mitgliedschaft, AJAX, eine komplexe Website zu erstellen. sehr schnell. Hoffentlich hat in diesem Tutorial Sie die Tools angegeben, die Sie erstellen Ihre eigenen ASP.NET MVC-Anwendungen beginnen möchten!


> [!div class="step-by-step"]
> [Vorherige](mvc-music-store-part-9.md)
