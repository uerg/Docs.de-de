---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Anzeigen von Zuordnungen in einer ASP.NET Web Pages (Razor) Standort | Microsoft Docs
author: tfitzmac
description: In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten in einer ASP.NET Web Pages (Razor) Website anhand der Zuordnung von Bing, Google, Ma bereitgestellten Dienste anzeigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Anzeigen von Maps an einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten in einer ASP.NET Web Pages (Razor) Website anhand der Zuordnung von Bing, Google, MapQuest und Yahoo bereitgestellten Dienste angezeigt.
> 
> Lernen Sie:
> 
> - Wie Sie eine Zuordnung basierend auf einer Adresse zu generieren.
> - So generieren Sie eine Zuordnung basierend auf Breiten-und Längenkoordinaten.
> - Informationen zum Registrieren einer Bing Maps-Entwicklerkonto und einen Schlüssel für die Verwendung mit Bing Maps abzurufen.
> 
> Dies ist die ASP.NET-Funktion im Artikel eingeführt:
> 
> - Die `Maps` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Lernprogramm funktioniert auch mit WebMatrix-3.


Webseiten, können Sie Zuordnungen auf einer Seite anzeigen, indem Sie mithilfe von `Maps` Helper. Sie können Zuordnungen basierend auf einer Adresse oder einen Satz von Längen- und Breitengrad Koordinaten generieren. Die `Maps` -Klasse können Sie einen an beliebten Zuordnung Module Aufruf, einschließlich Bing, Google, MapQuest und Yahoo.

Die Schritte zum Hinzufügen der Zuordnung zu einer Seite sind identisch, unabhängig davon, die welche die Zuordnung Module, die Sie aufrufen. Sie fügen nur einen Verweis der JavaScript-Datei, mit der verfügbaren Methoden, um die Zuordnung anzuzeigen, und rufen dann Methoden der der `Maps` Helper.

Sie wählen einen Map-Dienst abhängig davon, welcher `Maps` Hilfsmethode, die Sie verwenden. Sie können diese verwenden:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installieren die einzelnen Teile, die Sie benötigen

Wenn Zuordnungen anzeigen möchten, benötigen Sie diese:

- Die `Maps` Helper. Dieses Hilfsprogramm ist in der ASP.NET Web Helpers Library-Version 2. Wenn Sie bereits mit die Bibliothek hinzugefügt haben, können Sie es an Ihrem Standort als NuGet-Paket installieren. Weitere Informationen finden Sie unter [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372). (Suchen Sie in der Galerie der `microsoft-web-helpers` Paket.)
- Die jQuery-Bibliothek. Einige der WebMatrix-Websitevorlagen enthalten bereits jQuery-Bibliotheken in ihre *Skript* Ordner. Wenn Sie nicht über diese Bibliotheken verfügen, können Sie herunterladen die neueste jQuery-Bibliothek direkt aus der [jQuery.org](http://jQuery.org) Standort. Oder Sie können einen neuen Standort mithilfe einer Vorlage erstellen (z. B. die **Starter Site** Vorlage), und kopieren Sie die jQuery-Dateien von diesem Standort zu Ihrem aktuellen Standort.

Schließlich, wenn Sie die Bing Maps verwenden möchten, müssen Sie zuerst erstellen ein free-Konto und einen Schlüssel abzurufen. Um einen Schlüssel zu erhalten, gehen Sie folgendermaßen vor:

1. Erstellen Sie ein Konto auf dem [Bing Maps-Entwicklerkonto](https://www.microsoft.com/maps/developers/web.aspx). Benötigen Sie ein Microsoft-Konto (Windows Live ID) sowie ein.

    Sie können angeben, dass Sie die Verwendung des Schlüssels für möchten **Auswertung/Test**. Wenn Sie auf dem lokalen Computer mithilfe von WebMatrix und IIS Express die Zuordnungsfunktion testen, rufen Sie die **Website** Arbeitsbereich, und beachten Sie die URL Ihrer Website (z. B. `http://localhost:50408`, obwohl die Portnummer wahrscheinlich unterschiedlich sind). Sie können dies *"localhost"* Adresse wie der Standort, wenn Sie sich registrieren.
2. Nachdem Sie für ein Konto registriert haben, wechseln Sie zu den Bing Maps Account Center, und klicken Sie auf **erstellen oder Anzeigen von Schlüsseln**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Notieren Sie sich den Schlüssel, den Bing erstellt.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Erstellen einer Zuordnung basierend auf einer Adresse (mithilfe von Google)

Das folgende Beispiel zeigt, wie eine Seite zu erstellen, die eine Zuordnung basierend auf einer Adresse rendert. In diesem Beispiel wird gezeigt, wie Google Maps verwendet wird.

1. Erstellen Sie eine Datei mit dem Namen *MapAddress.cshtml* im Stammverzeichnis der Website. Auf dieser Seite generiert eine Zuordnung basierend auf eine Adresse, die Sie übergeben.
2. Kopieren Sie den folgenden Code in die Datei überschreiben der vorhandenen Inhalts.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Beachten Sie die folgenden Funktionen von der Seite ":

    - Die `<script>` Element in der `<head>` Element. Im Beispiel die `<script>` Elementverweise der *Jquery-1.6.4.min.js* -Datei, die eine verkleinerte (komprimiert) Version der jQuery-Bibliothek Version 1.6.4 ist. Beachten Sie, dass der Verweis annimmt, dass die *js* Datei befindet sich in der *Skripts* Ordner der Website. 

        > [!NOTE]
        > Wenn Sie eine andere Version von jQuery-Bibliothek verwenden, achten Sie darauf, dass Sie ordnungsgemäß auf diese Version Zeigt sind.
    - Der Aufruf der `@Maps.GetGoogleHtml` im Text der Seite. Um eine Adresse zuordnen, müssen Sie eine Adresszeichenfolge übergeben. Die Methoden für die andere Module der Karte funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Führen Sie die Seite, und geben Sie eine Adresse. Die Seite zeigt eine Zuordnung, basierend auf Google Maps, die den Speicherort zeigt, den Sie angegeben haben.

     ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Erstellen einer Zuordnung basierend auf den Breiten- und Längengrad Koordinaten (über Bing)

In diesem Beispiel wird gezeigt, wie zum Erstellen einer Zuordnung basierend auf Koordinaten wird. Dieses Beispiel zeigt, wie zur Verwendung von Bing Maps und Ihre Bing-Schlüssel enthalten. (Sie können eine Zuordnung basierend auf die andere Module der Zuordnung auch ohne Verwendung eines Bing-Schlüssels mit Koordinaten erstellen.)

1. Erstellen Sie eine Datei mit dem Namen *MapCoordinates.cshtml* im Stammverzeichnis der Website und Ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und Markup:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Ersetzen Sie `your-key-here` mit dem Bing Maps-Schlüssel, die Sie zuvor generiert.
3. Führen Sie die *MapCoordinates.cshtml* Seite Geben Sie die Breiten-und Längenkoordinaten, und klicken Sie dann auf die **Map It!** Schaltfläche. (Wenn Sie nicht, dass alle Koordinaten wissen, versuchen Sie Folgendes. Dies ist ein Speicherort in das Microsoft in Redmond wurde auf.)

   - Breitengrad: 47.6781005859375
   - Longitude: -122.158317565918

     Die Seite wird angezeigt, über die Koordinaten, die Sie angegeben haben.

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Microsoft.Maps-API-Referenz](https://msdn.microsoft.com/library/gg427611.aspx)
