---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Teil 10: Letzten Updates der Navigation und Standortentwurfs, Schlussfolgerung | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 10 umfasst die letzten Updates der Navigation und e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878593"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Teil 10: Letzten Updates der Navigation und Standortentwurfs, Zusammenfassung
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 10 werden die letzten Updates der Navigation und Standortentwurfs, Schlussfolgerung behandelt.


Wir alle wichtige Funktionen für unsere Website abgeschlossen haben, jedoch können wir noch einige Features für die Websitenavigation, auf der Startseite und der Seite "Speicher Durchsuchen".

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Shopping Cart Zusammenfassung Teilansicht erstellen

Wir möchten die Anzahl der Elemente im Einkaufswagen des Benutzers für die gesamte Website verfügbar zu machen.

![](mvc-music-store-part-10/_static/image1.png)

Wir können einfach implementieren Sie dies durch das Erstellen einer Teilansicht der unsere Site.master hinzugefügt wird.

Wie bereits zuvor gezeigt, enthält die ShoppingCart-Controller eine CartSummary Aktionsmethode eine Teilansicht zurück:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

CartSummary Teilansicht erstellen, mit der rechten Maustaste auf den Ordner Ansichten/ShoppingCart, und wählen Sie die Ansicht hinzufügen. Benennen Sie die Ansicht CartSummary, und aktivieren Sie das Kontrollkästchen "Erstellen eine Teilansicht" aus, wie unten dargestellt.

![](mvc-music-store-part-10/_static/image2.png)

Die Teilansicht CartSummary ist ganz einfach – es ist nur ein Link auf die ShoppingCart-Indexansicht, die die Anzahl der Elemente in den Einkaufswagen angezeigt wird. Der vollständige Code für CartSummary.cshtml lautet wie folgt:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Wir können eine Teilansicht in eine andere Seite in der Website, einschließlich der Website-Master mithilfe der Methode Html.RenderAction einschließen. RenderAction erfordert, geben Sie den Aktionsnamen ("CartSummary") und den Namen des Controllers ("ShoppingCart") als unten.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Vor dem Hinzufügen dieser an den Standort Layout, wird auch im Menü "Genre" erstellt, damit wir alle unsere Site.master Updates gleichzeitig vornehmen können.

## <a name="creating-the-genre-menu-partial-view"></a>Die "Genre" Menü Teilansicht erstellen

Wir können ändern, es viel einfacher für die Benutzer über den Store durch Hinzufügen einer "Genre" Menü "alle die Genres verfügbar in unserem Store listet navigieren.

![](mvc-music-store-part-10/_static/image3.png)

Befolgen wir die gleichen Schritte auch eine Teilansicht GenreMenu erstellen, und klicken Sie dann wir hinzugefügt werden können sowohl mit dem übergeordneten Standort. Fügen Sie zunächst die folgenden GenreMenu Controlleraktion hinzu der StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Diese Aktion wird eine Liste von Genres der von der Teilansicht angezeigt wird, die wir als Nächstes erstellt.

*Hinweis: Wir haben das Attribut [ChildActionOnly] diese Controlleraktion hinzugefügt bedeutet, dass wir nur diese Aktion aus einer Teilansicht verwendet werden soll. Dieses Attribut verhindert Controlleraktion durch Navigieren zum /Store/GenreMenu ausgeführt werden. Dies ist nicht erforderlich für Teilansichten, aber es wird empfohlen, da wir möchten sicherstellen, dass unsere Controlleraktionen verwendet werden, wie wir möchten. Wir sind auch zurückgeben PartialView statt anzeigen, können Sie das Ansichtsmodul, dass sie das Layout für diese Ansicht verwenden, sollten nicht wissen, wie sie in den anderen Ansichten enthalten wird, ist.*

Mit der rechten Maustaste auf die GenreMenu-Controlleraktion, und erstellen Sie eine Teilansicht mit dem Namen GenreMenu die stark typisiert ist, verwenden die Datenklasse "Genre" anzeigen, wie unten dargestellt.

![](mvc-music-store-part-10/_static/image4.png)

Aktualisieren Sie den Code anzeigen, für die GenreMenu partielle Ansicht zum Anzeigen von Elementen, die eine ungeordnete Liste wie folgt verwenden.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aktualisieren von Site-Layout für unsere Teilansichten

Wir können unsere Teilansichten hinzufügen, zu der Website-Layout (/Ansichten/freigegeben/\_Layout.cshtml) durch Aufrufen von Html.RenderAction(). Wir fügen sie beide an, als auch einige zusätzliche Markup zum Anzeigen, wie unten dargestellt:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Jetzt Wenn wir die Anwendung ausführen, wird es der "Genre" im linken Navigationsbereich und die Zusammenfassung der Warenkorb am oberen angezeigt.

## <a name="update-to-the-store-browse-page"></a>Aktualisieren Sie auf der Seite "Speicher Durchsuchen"

Die Seite Speicher Durchsuchen ist funktionsfähig, aber nicht sehr gut aussehen. Wir aktualisieren können Sie die Seite, um die Alben in einem besser Layout anzeigen, indem Sie den Code anzeigen (in /Views/Store/Browse.cshtml gefunden) wie folgt aktualisieren:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Wir nehmen hier Url.Action statt Html.ActionLink verwenden, sodass wir die spezielle Formatierung auf den Link, um die Grafik Album enthalten anwenden können.

*Hinweis: Es werden eine generische Albumtitel für diese Alben angezeigt. Diese Informationen werden in der Datenbank gespeichert und kann bearbeitet werden, über den Speicher-Manager. Willkommen beim Ihre eigene Grafik hinzufügen werden.*

Wenn wir zu einem "Genre" navigieren, wird es nun die Alben, die in einem Raster mit Album Grafik dargestellt angezeigt.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualisieren die Homepage, um nach oben Verkaufsmanager Alben anzeigen

Wir möchten unsere meistverkaufte Alben auf der Startseite den Umsatz steigern, feature. Wir stellen einige Updates an unsere HomeController zu behandeln, und in sowie einige zusätzlichen Grafiken hinzufügen.

Zunächst fügen wir eine Navigationseigenschaft unsere Album-Klasse, sodass EntityFramework erkennt, dass sie verknüpft sind. Die letzten Zeilen der unsere **Album** Klasse sollte jetzt wie folgt aussehen:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Hinweis: Dies erfordert, dass hinzufügen eine using-Anweisung System.Collections.Generic-Namespace zu importieren.*

Zunächst fügen wir ein StoreDB-Feld und die MvcMusicStore.Models using-Anweisungen, wie Sie in unserer anderen Controller. Als Nächstes fügen die folgende Methode, die HomeController wir, die abfragt, unsere Datenbank zum obere Verkaufsmanager Alben entsprechend OrderDetails suchen.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Dies ist eine private Methode, da wir nicht, um es als eine Controlleraktion verfügbar zu machen möchten. Wir werden diese im HomeController aus Gründen der Einfachheit einschließlich, aber es wird empfohlen, Ihre Geschäftslogik in separaten Dienstklassen nach Bedarf zu verschieben.

Mit diesem erfüllt können wir Controlleraktion Index zum Abfragen der obersten 5 verkaufen Alben und Geräte auf die Ansicht aktualisieren.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Der vollständige Code für die aktualisierte HomeController ist wie unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Schließlich müssen wir unsere Startseite Indexansicht aktualisieren, sodass eine Liste von Alben angezeigt werden können, durch Aktualisieren des Typ des Modells und im unteren Bereich die Albumliste hinzugefügt. Wir übernehmen diese Möglichkeit, die auch einer Überschrift und eine Promotion-Abschnitt der Seite hinzufügen.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Wenn wir die Anwendung ausführen, wird wir nun unsere aktualisierten Startseite mit Top Verkaufsmanager Alben und unsere Werbe-Meldung angezeigt.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Schlussbemerkung

Wir haben gesehen, dass ASP.NET MVC erleichtert usw. eine anspruchsvolle Website mit Datenbankzugriff, AJAX-Mitgliedschaft zu erstellen. ziemlich schnell. Hoffentlich hat in diesem Lernprogramm Ihnen die Tools gegeben, die Sie beginnen damit, eigene ASP.NET-MVC Anwendungen erstellen müssen!


> [!div class="step-by-step"]
> [Vorherige](mvc-music-store-part-9.md)
