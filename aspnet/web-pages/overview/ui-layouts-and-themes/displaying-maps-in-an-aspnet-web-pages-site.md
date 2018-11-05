---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Anzeigen von Karten in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten in einer ASP.NET Web Pages (Razor)-Website, die basierend auf der Zuordnung von Bing, Google, Ma bereitgestellten Dienste anzuzeigen...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: cde27c54b11ee91b193dffd61e3a354c6cf2449a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020987"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Anzeigen von Karten in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten in einer ASP.NET Web Pages (Razor)-Website, die basierend auf der Zuordnung von Bing, Google, Yahoo und MapQuest bereitgestellten Dienste angezeigt wird.
> 
> Sie lernen Folgendes:
> 
> - Wie Sie eine Zuordnung, die basierend auf einer Adresse zu generieren.
> - So generieren Sie eine Zuordnung, auf Grundlage von Breiten-und Längenkoordinaten.
> - So registrieren ein Bing Maps-Developer-Konto und einen Schlüssel für die Verwendung mit Bing Maps zu erhalten.
> 
> Dies ist die ASP.NET-Funktion, die in diesem Artikel eingeführt:
> 
> - Die `Maps` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> In diesem Tutorial funktioniert auch mit WebMatrix 3.


In Webseiten können Sie Zuordnungen auf einer Seite anzeigen, indem Sie mithilfe von `Maps` Helper. Sie können Zuordnungen, die basierend auf einer Adresse oder auf einen Satz von Koordinaten für Längen- und Breitengrad generieren. Die `Maps` -Klasse ermöglicht, die Sie beliebte Map-Engines wie Bing, Google, Yahoo und MapQuest aufruft.

Die Schritte zum Hinzufügen der Zuordnung zu einer Seite sind identisch unabhängig davon, die welches die Map-Engines, die Sie aufrufen. Sie einfach einen JavaScript-Dateiverweis, mit der verfügbaren Methoden zum Anzeigen der Karte, hinzufügen, und klicken Sie dann Sie Methoden zum Aufrufen der `Maps` Helper.

Sie wählen einen Map-Diensts, auf dessen Grundlage die `Maps` Hilfsmethode, die Sie verwenden. Sie können diese verwenden:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installieren die Teile, die Sie benötigen

Wenn Zuordnungen anzeigen möchten, benötigen Sie diese Elemente:

- Die `Maps` Helper. Dieses Hilfsprogramm ist in Version 2 von der ASP.NET Web Helpers Library. Wenn Sie bereits mit die Bibliothek hinzugefügt haben, können Sie es an Ihrem Standort als NuGet-Paket installieren. Weitere Informationen finden Sie unter [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372). (Im Katalog, suchen Sie nach der `microsoft-web-helpers` Paket.)
- Die jQuery-Bibliothek. Einige der Vorlagen, WebMatrix-Website umfasst bereits jQuery-Bibliotheken in ihren *Skript* Ordner. Wenn Sie nicht über diese Bibliotheken verfügen, können Sie die neuesten jQuery-Bibliothek direkt aus der [jQuery.org](http://jQuery.org) Standort. Oder Sie können einen neuen Standort mithilfe einer Vorlage erstellen (z. B. die **Starter Site** Vorlage), und kopieren Sie die jQuery-Dateien auf dieser Website Ihrem aktuellen Standort.

Erstellen Sie schließlich sollten Sie Bing-Karten zu verwenden, Sie müssen zuerst ein free-Konto und einen Schlüssel zu erhalten. Um einen Schlüssel zu erhalten, gehen Sie folgendermaßen vor:

1. Erstellen Sie ein Konto für die [Bing Maps-Entwicklerkonto](https://www.microsoft.com/maps/developers/web.aspx). Sie müssen ein Microsoft-Konto (Windows Live ID) sowie verfügen.

    Sie können angeben, dass Sie den Schlüssel für die verwenden möchten **Auswertung/Test**. Wenn Sie die Mapping-Funktion auf dem lokalen Computer mithilfe von WebMatrix und IIS Express testen, wechseln Sie die **Standort** -Arbeitsbereich, und beachten Sie die URL Ihrer Website (z. B. `http://localhost:50408`, obwohl Ihre Portnummer wahrscheinlich unterscheiden). Sie können dies verwenden *"localhost"* Adresse wie der Standort, wenn Sie sich registrieren.
2. Nachdem Sie für ein Konto registriert haben, wechseln Sie zu den Bing Maps-Kontocenter, und klicken Sie auf **erstellen oder Anzeigen von Schlüsseln**:

    ![Mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Notieren Sie den Schlüssel, den Bing erstellt.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Erstellen einer Zuordnung basierend auf einer Adresse (mithilfe von Google)

Das folgende Beispiel zeigt, wie Sie eine Seite erstellen, die eine Zuordnung, die basierend auf einer Adresse rendert. Dieses Beispiel zeigt, wie Sie Google Maps zu verwenden.

1. Erstellen Sie eine Datei mit dem Namen *MapAddress.cshtml* im Stammverzeichnis der Website. Auf dieser Seite generiert eine Karte, die basierend auf einer Adresse, die an sie übergeben werden.
2. Kopieren Sie den folgenden Code in die Datei, die vorhandene Inhalte überschrieben.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Beachten Sie, dass die folgenden Funktionen von der Seite:

    - Die `<script>` Element in der `<head>` Element. Im Beispiel die `<script>` Elementverweise der *Jquery-1.6.4.min.js* -Datei, die eine verkleinerte (komprimierte) Version der jQuery-Bibliothek, Version 1.6.4 ist. Beachten Sie, dass der Verweis annimmt, dass die *js* Datei befindet sich in der *Skripts* Ordner der Website. 

        > [!NOTE]
        > Wenn Sie eine andere Version der jQuery-Bibliothek verwenden, achten Sie darauf, dass Sie ordnungsgemäß auf diese Version verwiesen werden.
    - Der Aufruf der `@Maps.GetGoogleHtml` im Hauptteil der Seite. Um eine Adresse zuzuordnen, müssen Sie eine Adresszeichenfolge übergeben. Die Methoden für die anderen Module Karte funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Führen Sie die Seite, und geben Sie eine Adresse. Die Seite zeigt eine Code Map, basierend auf Google Maps, der den Speicherort zeigt, den Sie angegeben haben.

     ![1-Zuordnung](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Erstellen einer Zuordnung basierend auf den Breiten- und Längengrad koordiniert (mit Bing)

Dieses Beispiel zeigt, wie zum Erstellen einer Zuordnung basierend auf Koordinaten. Dieses Beispiel zeigt, wie Sie mit der Bing Maps und zur Ihre Bing-Schlüssel. (Sie können eine Zuordnung, die basierend auf Koordinaten mithilfe der anderen Zuordnung Module auch ohne Verwendung eines Bing-Schlüssels erstellen.)

1. Erstellen Sie eine Datei mit dem Namen *MapCoordinates.cshtml* im Stammverzeichnis des Standorts und Ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und Markup:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Ersetzen Sie dies `your-key-here` mit dem Bing Maps-Schlüssel, den Sie zuvor erstellt.
3. Führen Sie die *MapCoordinates.cshtml* Seite Geben Sie Breiten-und Längenkoordinaten, und klicken Sie dann auf die **Map It!** Schaltfläche. (Wenn Sie nicht, dass alle Koordinaten wissen, versuchen Sie Folgendes. Dies ist ein Speicherort auf dem Campus von Microsoft in Redmond.)

   - Breitengrad: 47.6781005859375
   - Längengrad:-122.158317565918

     Die Seite wird angezeigt, mit die Koordinaten, die Sie angegeben haben.

     ![Mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Microsoft.Maps-API-Referenz](https://msdn.microsoft.com/library/gg427611.aspx)
