---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Hinzufügen von sozialen Netzwerken zu ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie Ihre Website in soziale Netzwerkdienste integrieren. In diesem Kapitel erfahren Sie, wie Sie den Personenkreis Lesezeichenlink/Ihrer Website...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: d2b970e7b80e4129d0a912f648f9c4a54df531b2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021156"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Hinzufügen von soziale Websites für Networking ASP.NET-Webseiten (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt das Hinzufügen von Links zu sozialen Netzwerken für Facebook, Twitter, Reddit und Digg zu Seiten, die auf einer Website für ASP.NET Web Pages (Razor), und wie Sie Twitter-Feeds, Xbox Spieler Karten und Gravatar-Bilder enthalten.
> 
> Sie lernen Folgendes:
> 
> - Wie Sie Personen Lesezeichenlink/Ihrer Website zu überlassen.
> - So fügen Sie einen Twitter-Feed.
> - Gewusst wie: Hinzufügen von Facebook **wie** Schaltfläche, um Seiten.
> - Informationen zum Rendern von Bildern von Gravatar.com.
> - Wie eine Spiele Xbox-Karte auf Ihrer Website anzuzeigen.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web-Hilfsbibliothek (NuGet-Paket)
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages-3, mit Ausnahme der Teile, die die ASP.NET Web-Hilfsbibliothek verwenden.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Verknüpfen Ihre Website auf Websites für soziale Netzwerke

Wenn der Benutzer etwas an Ihrem Standort zufrieden sind, möchten sie häufig für Freunde freigeben. Sie können diesen Vorgang vereinfachen durch Anzeigen der Symbole (Symbole), die Benutzer klicken können, um eine Seite auf Digg, Reddit, Facebook, Twitter oder ähnliche Websites gemeinsam nutzen.

Um diese Symbole anzuzeigen, fügen die `LinkSharecode` Hilfsmethode zu einer Seite. Personen, die Ihre Seite besuchen, können ein einzelnes Symbol klicken. Wenn sie ein Konto mit dieser Website für soziale Netzwerke verfügen, können sie einen Link zu Ihrer Seite klicken Sie dann auf dieser Website bereitstellen.

![Abbildung 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), falls Sie bereits it. - hinzugefügt haben erstellen Sie eine Seite mit dem Namen *ListLinkShare.cshtml* und hinzufügen Das folgende Markup:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    In diesem Beispiel ist bei der `LinkShare` ausgeführt, den Seitentitel als Parameter verwendet, das den Titel der Seite auf der Website für soziale Netzwerke weiterleitet übergeben wird. Jedoch könnten Sie eine beliebige Zeichenfolge übergeben Sie die gewünschten. In diesem Beispiel gibt auch die Websites für soziale Netzwerke, die in der Liste enthalten. Sie können die Websites für soziale Netzwerke angeben, die auf Ihrer Website relevant sind.
2. Führen Sie die *ListLinkShare.cshtml* Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
3. Klicken Sie auf ein Symbol für einen dieser Standorte, denen Sie registriert sind. Der Link leitet Sie auf der Seite auf der ausgewählten soziales Netzwerk-Website, in dem Sie gemeinsam nutzen, können, einen Link. Z. B. Wenn Sie den Reddit-Link klicken, Sie gelangen auf die `submit to reddit` auf der Website Reddit.

     ![Abbildung 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Hinzufügen einer Twitter-Feed

Weitere Informationen zur Verwendung einer Twitter-Hilfsprogramms, das mit der aktuellen Version der Twitter-API kompatibel ist, finden Sie unter [Twitter-Hilfsprogramm](../ui-layouts-and-themes/twitter-helper.md). Dieses Beispiel zeigt, wie Sie Ihre eigenen Hilfsprogramm schreiben, sodass Sie den Code aus vielen Seiten einfach wiederverwenden können.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Anzeigen von Facebook &quot;wie&quot; Schaltfläche

In einigen Fällen ist die beste Möglichkeit, den Code direkt aus dem Anbieter des sozialen Netzwerks Netzwerk statt eines Hilfsprogramms abrufen. Dies ist insbesondere dann, wenn der Anbieter des sozialen Netzwerks ihrer Optionen updates schneller als das Hilfsobjekt aktualisiert wird.

Um Facebook-Features (z. B. die Schaltfläche "Like") auf Ihrer Website hinzuzufügen, können Sie Codeausschnitte aus Abrufen der ["Developers.Facebook.com"](https://developers.facebook.com/) Standort. Auf der Facebook-Website verwenden Sie ihre Tools, um einen Codeausschnitt zu generieren, der auf Ihrer Website relevant sind.

Der folgende hervorgehobene Code ist der Code, der das Tool wie Schaltfläche auf der Website "Developers.Facebook.com" abgerufen wurde. Sie müssen Ihre eigenen app-ID angeben.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Rendern einer Gravatar-Bilds

Ein *Gravatar* (eine &quot;weltweit anerkannten Avatar&quot;) ist ein Bild, das für mehrere Websites als Ihren Avatar verwendet werden kann &#8212; , also ein Image, das Sie darstellt. Beispielsweise kann ein Gravatar identifizieren eine Person in einem Forumsbeitrag in einem Blogkommentar, und so weiter. (Sie können Ihre eigenen Gravatar an der Gravatar-Website unter registrieren [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Wenn Sie Images, die neben den Namen oder e-Mail-Adressen der Personen auf Ihrer Website anzeigen möchten, können Sie das Gravatar-Hilfsprogramm.

In diesem Beispiel verwenden Sie eine einzelne Gravatar, die sich selbst darstellt. Eine weitere Möglichkeit zum Verwenden einer Gravatar besteht darin, Personen, die ihre Gravatar-Adresse angeben, wenn sie auf der Website registrieren. (Sie können erfahren Sie, wie Benutzer in registrieren können [Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).) Klicken Sie dann, wenn Sie die Informationen für diesen Benutzer anzeigen, können Sie einfach hinzufügen der Gravatar, in dem Sie den Namen des Benutzers anzeigen.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen einer neuen Webseite mit dem Namen *Gravatar.cshtml*.
3. Fügen Sie der Datei das folgende Markup hinzu: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Die `Gravatar.GetHtml` Methode zeigt die Gravatar-Image auf der Seite. Um die Größe des Bilds zu ändern, können Sie eine Zahl als zweiten Parameter einschließen. Die Standardgröße ist 80. Zahlen von weniger als 80 stellen das Image kleiner. Die Anzahl ist größer als 80 das Bild vergrößert wird.
4. In der `Gravatar.GetHtml` Methoden ersetzen `<Your Gravatar account here>` mit der e-Mail-Adresse, die Sie für Ihr Konto Gravatar verwenden. (Wenn Sie nicht über ein Gravatar-Konto verfügen, können Sie die e-Mail-Adresse einer Person, der die verwenden.)
5. Führen Sie die Seite in Ihrem Browser. Die Seite zeigt zwei Gravatar-Bilder für die e-Mail-Adresse, die Sie angegeben haben. Das zweite Bild ist kleiner als die erste. 

    ![Abbildung 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Anzeigen einer Spieler Xbox-Karte

Wenn Personen Microsoft Xbox-online-Spiele spielen, hat jeder Benutzer eine eindeutige ID. Statistiken werden für jeden Spieler in Form einer Karte Spieler beibehalten, zeigt ihre Ruf, Spieler Bewertung und zuletzt Spiele gespielt. Wenn Sie eine Xbox Spieler sind, können Sie Ihre Spiele Karte auf Seiten auf Ihrer Website anzeigen, indem Sie mit der `GamerCard` Helper.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Seite mit dem Namen *XboxGamer.cshtml* und fügen Sie das folgende Markup hinzu.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Sie verwenden die `GamerCard.GetHtml` -Eigenschaft an den Alias für die Spieler Karte angezeigt werden.
3. Führen Sie die Seite in Ihrem Browser. Die Seite zeigt die Spiele Xbox-Karte, die Sie angegeben haben.

    ![Abbildung 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
