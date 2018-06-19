---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Hinzufügen von sozialen Netzwerken zu ASP.NET Web Sites (Razor) Seiten | Microsoft Docs
author: tfitzmac
description: In diesem Kapitel wird erläutert, wie Dienste für soziale Netzwerke Ihrer Website integriert werden. In diesem Kapitel erfahren Sie, wie Besucher Lesezeichenlink/Ihrer Website...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899342"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Hinzufügen von Social Networking zu ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Seiten in einer Website für ASP.NET Web Pages (Razor) Links mit sozialen Netzwerken für Facebook, Twitter, Reddit und Digg hinzugefügt und wie z. B. Twitter-Feeds, Xbox Spiele Karten und a. mit Gravatar-Images.
> 
> Lernen Sie:
> 
> - Wie Sie Personen Lesezeichenlink/Ihrer Website zu ermöglichen.
> - So fügen Sie einen Twitter-Feed.
> - Gewusst wie: Hinzufügen einer Facebook **wie** Schaltfläche, um Seiten.
> - Wie Gravatar.com Bilder gerendert.
> - Vorgehensweise beim Anzeigen von einer Xbox-Spiele-Karte auf der Website.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web-Hilfsbibliothek (NuGet-Paket)
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 3, mit Ausnahme der Teile, die der ASP.NET Web-Hilfsbibliothek verwenden.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Verknüpfen Ihre Website auf Social Networking-Websites

Wenn etwas auf Ihrer Website Benutzer möchten gern, möchte sie häufig Freunde weitergeben. Sie können diesen Vorgang vereinfachen durch die Anzeige von Glyphen (Symbole), die Benutzer klicken können, um eine Seite im Digg, Reddit, Facebook, Twitter oder ähnliche Websites gemeinsam nutzen.

Um diese Symbole anzuzeigen, fügen Sie der `LinkSharecode` Helper zu einer Seite. Personen, die Ihre-Seite besuchen können ein einzelnes Symbol klicken. Wenn sie ein Konto mit dieser Website für soziale Netzwerke verfügen, können sie einen Link zu der Seite klicken Sie dann auf dieser Site veröffentlichen.

![Abbildung 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch darauf - hinzugefügt haben eine Seite mit dem Namen "erstellen" *ListLinkShare.cshtml* und hinzufügen Das folgende Markup:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    In diesem Beispiel wird bei der `LinkShare` ausgeführt, den Titel der Seite wird als Parameter, der wiederum den Titel der Seite auf der Website für soziale Netzwerke übergibt übergeben. Sie konnte jedoch in einer beliebigen Zeichenfolge übergeben, die Sie möchten. In diesem Beispiel gibt auch die social Network-Sites in der Liste eingeschlossen werden sollen. Sie können die social Network-Sites angeben, die auf Ihrer Website relevant sind.
2. Führen Sie die *ListLinkShare.cshtml* Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
3. Klicken Sie auf ein Symbol für einen dieser Standorte, denen Sie registriert sind. Der Link leitet Sie zur Seite "" auf dem ausgewählten sozialen Netzwerk-Standort, in dem Sie freigeben können, einen Link. Z. B. Wenn Sie den Reddit Link klicken, Sie werden erstellt die `submit to reddit` Seite Reddit-Website.

     ![Abbildung 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Hinzufügen einer Twitter-Feed

Weitere Informationen zur Verwendung von einer Twitter-Hilfsprogramms, das mit der aktuellen Version von der Twitter-API kompatibel ist, finden Sie unter [Twitter-Hilfsprogramm](../ui-layouts-and-themes/twitter-helper.md). Dieses Beispiel zeigt, wie Sie eine eigene Hilfsfunktion schreiben, sodass Sie einfach den Code von viele Seiten wiederverwenden können.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Anzeigen einer Facebook &quot;wie&quot; Schaltfläche

In einigen Fällen ist die beste Möglichkeit zum Abrufen des Codes direkt aus der Anbieter für soziale Netzwerke, anstatt auf ein Hilfsprogramm. Dies gilt vor allem, wenn Anbieter des sozialen Netzwerks ihrer Optionen aktualisiert schneller als das Hilfsobjekt aktualisiert wird.

Um Facebook-Funktionen (z. B. die Like-Schaltfläche) Ihrer Website hinzugefügt haben, können Sie Codeausschnitte vom Abrufen der [developers.facebook.com](https://developers.facebook.com/) Standort. Auf der Facebook-Website verwenden Sie die Tools, um einen Codeausschnitt zu generieren, der relevant für Ihre Website ist.

Die folgende hervorgehobene Code ist der Code, der aus dem Tool wie Schaltfläche auf der Website developers.facebook.com abgerufen wurde. Sie müssen Ihre eigene app-ID angeben.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Rendern einer a. mit Gravatar-Bild

Ein *a. mit Gravatar* (eine &quot;Global erkannten Avatar&quot;) ist ein Bild, das als den Avatar für mehrere Websites verwendet werden kann &#8212; , also ein Bild, das Sie darstellt. Eine a. mit Gravatar kann z. B. eine Person in einem Forumsbeitrag in einem Blogkommentar zu identifizieren und so weiter. (Registrieren Sie Ihren eigenen a. mit Gravatar an der a. mit Gravatar-Website unter [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Wenn Sie Bilder neben den Namen oder die e-Mail-Adressen der Personen auf Ihrer Website anzeigen möchten, können Sie die a. mit Gravatar-Hilfsprogramm verwenden.

In diesem Beispiel verwenden Sie eine einzelne a. mit Gravatar, die sich selbst darstellt. Eine andere Möglichkeit, eine a. mit Gravatar verwenden ist, damit Benutzer ihre a. mit Gravatar-Adresse angeben, wenn sie auf Ihrer Website registriert. (Sie können erfahren Sie, wie Personen, die in registrieren können [Sicherheit hinzufügen und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).) Klicken Sie dann, wenn Sie Informationen für diesen Benutzer anzeigen, können Sie hinzufügen die a. mit Gravatar, in dem Sie den Namen des Benutzers anzeigen.

1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Webseite mit dem Namen *Gravatar.cshtml*.
3. Fügen Sie der Datei das folgende Markup hinzu: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Die `Gravatar.GetHtml` Methode zeigt das a. mit Gravatar-Bild auf der Seite. Um die Größe des Bilds zu ändern, können Sie eine Zahl als zweiten Parameter einschließen. Die Standardgröße beträgt 80. Zahlen kleiner als 80 stellen das Bild kleiner. Zahlen, die größer als 80 vergrößern Sie das Bild.
4. In der `Gravatar.GetHtml` Methoden ersetzen `<Your Gravatar account here>` mit der e-Mail-Adresse, die Sie für Ihr Konto a. mit Gravatar verwenden. (Wenn Sie kein a. mit Gravatar-Konto haben, können Sie die e-Mail-Adresse einer Person, der die verwenden.)
5. Führen Sie die Seite in Ihrem Browser. Die Seite zeigt zwei a. mit Gravatar-Images für die e-Mail-Adresse, die Sie angegeben haben. Das zweite Bild ist kleiner als das erste. 

    ![Abbildung 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Anzeigen von einer Xbox-Spiele-Karte

Wenn Personen Microsoft Xbox online spielen, hat jeder Benutzer eine eindeutige ID. Statistiken werden für jeden Spieler in Form einer Spieler Karte beibehalten zeigt ihre Bewertung, Spiele Ergebnis, und zuletzt wiedergegeben, Spiele. Wenn Sie ein Spieler Xbox sind, können Sie Ihre Spiele Karte auf Seiten auf Ihrer Website anzeigen, mit der `GamerCard` Helper.

1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Seite mit dem Namen *XboxGamer.cshtml* und fügen Sie das folgende Markup hinzu.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Verwenden Sie die `GamerCard.GetHtml` -Eigenschaft an den Alias für die Spiele Karte angezeigt werden.
3. Führen Sie die Seite in Ihrem Browser. Die Seite zeigt die Spiele Xbox-Karte, die Sie angegeben haben.

    ![Abbildung 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
