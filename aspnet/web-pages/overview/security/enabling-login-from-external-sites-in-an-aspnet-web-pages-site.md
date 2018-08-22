---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Anmelden mithilfe externer Websites in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Artikel wird erläutert, wie bei einer ASP.NET Web Pages (Razor)-Website, die mithilfe von Facebook, Google, Twitter, Yahoo und anderen Websites anmelden – d. h. wie unterstützt...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a74b13e9d1ddb5bc02f4ea5184108de5e014ead0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823601"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Anmelden mithilfe externer Websites in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie bei einer ASP.NET Web Pages (Razor)-Website, die mithilfe von Facebook, Google, Twitter, Yahoo und anderen Websites anmelden – d. h. wie OAuth und OpenID an Ihrem Standort unterstützt.
> 
> Sie lernen Folgendes:
> 
> - Anmeldung von anderen Websites aktivieren, bei der Verwendung der Vorlage Starter Site von WebMatrix
> 
> Dies ist die ASP.NET-Funktion, die in diesem Artikel eingeführt:
> 
> - Die `OAuthWebSecurity` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

ASP.NET Web Pages bietet Unterstützung für [OAuth](http://oauth.net/) und [OpenID](http://openid.net/) Anbieter. Verwenden diese Anbieter, können Sie Benutzern die Anmeldung an Ihrem Standort mit ihren vorhandenen Anmeldeinformationen aus Facebook, Twitter, Microsoft und Google lassen. Beispielsweise können um mit einer Facebook-Konto anzumelden, Benutzer nur ein Facebook-Symbol auswählen sie an die Facebook-Anmeldeseite umgeleitet, in dem sie ihre Benutzerinformationen eingeben. Sie können dann die Facebook-Anmeldung mit ihrem Konto auf der Website zuordnen. Eine ähnliche Verbesserung auf die Funktionen der Webseiten-Mitgliedschaft ist, dass Benutzer mehrere Anmeldungen (einschließlich Anmeldungen von Websites für soziale Netzwerke) zuordnen, können mit einem einzigen Konto auf Ihrer Website.

Diese Abbildung zeigt die Anmeldeseite von der **Starter Site** Vorlage, in dem ein Benutzer eine Symbol für Facebook, Twitter, Google oder Microsoft zum Aktivieren der Protokollierung mit einem externen Konto auswählen kann:

![externen Anbietern](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Sie können die Mitgliedschaft von OAuth und OpenID aktivieren, indem Sie die Kommentierung von ein paar Codezeilen in der **Starter Site** Vorlage. Die Methoden und Eigenschaften können Sie arbeiten mit dem OAuth und OpenID-Anbieter sind in der `WebMatrix.Security.OAuthWebSecurity` Klasse. Die **Starter Site** Vorlage umfasst eine Infrastruktur der vollständigen, komplett mit einer Anmeldeseite eine Mitgliedschaftsdatenbank und der gesamte Code müssen Sie Benutzern die Anmeldung bei Ihrer Website, die mit lokalen Anmeldeinformationen oder die von einem anderen Standort kann .

Dieser Abschnitt enthält ein Beispiel, damit Benutzer, die von externen Websites auf einer Website melden Sie sich auf der Grundlage der **Starter Site** Vorlage. Nach der Erstellung einer Startwebsite, führen Sie diese (Details folgen):

- Für die Standorte, die einen OAuth-Anbieter (Facebook, Twitter und Microsoft) verwenden, erstellen Sie eine Anwendung auf der externen Website. Dadurch können Sie die Anwendungsschlüssel, die Sie benötigen, um das Feature "Anmeldung" für die Websites aufrufen.
- Für Standorte, die einen OpenID-Anbieter (Google) zu verwenden, müssen Sie keinen zum Erstellen einer Anwendung. Für alle diese Standorte müssen Sie ein Konto verfügen, um sich anmelden und entwickleranwendungen erstellen.

    > [!NOTE]
    > Microsoft-Anwendungen akzeptieren nur eine live-URL für eine Arbeitswebsite, damit Sie eine lokale Website-URL für das Testen von Anmeldungen nicht verwenden können.
- Bearbeiten Sie einige Dateien auf Ihrer Website, um den entsprechenden Authentifizierungsanbieter anzugeben und um eine Anmeldung auf der Website zu übermitteln, die Sie verwenden möchten.

Dieser Artikel enthält separate Anweisungen für die folgenden Aufgaben:

- [Aktivieren von Google-Anmeldungen](#To_enable_Google_logins)
- [Aktivieren die Facebook-logins](#To_enable_Facebook_logins)
- [Aktivieren von Twitter-Anmeldungen](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Aktivieren von Google-Anmeldungen

1. Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die basierend auf der Vorlage Starter Site von WebMatrix.
2. Öffnen der  *\_AppStart.cshtml* Seite, und heben Sie die auskommentierung der folgenden Zeile des Codes. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testen die Google-Anmeldung

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **melden Sie sich bei** Schaltfläche.
2. Auf der *Anmeldung* auf der Seite die **verwenden Sie einen anderen Dienst anmelden** Abschnitt, und wählen Sie entweder die **Google** oder **Yahoo** Schaltfläche "Senden". Dieses Beispiel verwendet die Google-Anmeldung. 

    Die Webseite leitet die Anforderung an die Google-Anmeldeseite.

    ![Google-Anmeldung](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Geben Sie Anmeldeinformationen für ein vorhandenes Google-Konto ein.
4. Wenn Sie Google fragt, ob Sie zulassen möchten *"localhost"* um Informationen aus dem Konto zu verwenden, klicken Sie auf **zulassen**.

    Der Code verwendet das Google-Token zur Authentifizierung des Benutzers und gibt dann zurück zu dieser Seite auf Ihrer Website. Sie können ein neues Konto auf der Website so ordnen Sie die externe Anmeldung mit registrieren, oder auf dieser Seite können Benutzer ihre Google-Anmeldung mit einem vorhandenen Konto auf Ihrer Website zu verknüpfen.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Wählen Sie die **zuordnen** Schaltfläche. Gibt zurück, der Browser zur Startseite Ihrer Anwendung.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Aktivieren die Facebook-Logins

1. Wechseln Sie zu der [Facebook-Entwickler-Website](https://developers.facebook.com/apps) (Melden Sie sich, wenn Sie noch nicht angemeldet sind).
2. Wählen Sie die **Create New App** Schaltfläche aus, und klicken Sie dann führen Sie die aufforderungen zum Benennen und die neue Anwendung erstellen.
3. Im Abschnitt **wählen, wie Ihre app mit Facebook integrieren**, wählen Sie die **Website** Abschnitt.
4. Geben Sie die **Website-URL** Feld mit der URL Ihrer Website (z. B. `http://www.example.com`). Die **Domäne** Feld ist optional; Sie können dies verwenden, zur Authentifizierung für eine gesamte Domäne (z. B. *"example.com"*). 

    > [!NOTE]
    > Wenn Sie einen Standort, auf dem lokalen Computer mit einer URL ausgeführt werden wie `http://localhost:12345` (wobei die Anzahl ist eine lokale Portnummer), können Sie diesen Wert zum Hinzufügen der **Website-URL** Feld für das Testen Ihrer Website. Jedoch jedes Mal, wenn die Portnummer des lokalen standortänderungen, müssen Sie zum Aktualisieren der **Website-URL** Feld der Anwendung.
5. Wählen Sie die **Save Changes** Schaltfläche.
6. Wählen Sie die **Apps** Registerkarte erneut aus, und zeigen Sie dann auf die Startseite für Ihre Anwendung.
7. Kopieren der **App-ID** und **App-Geheimnis** Werte für Ihre Anwendung und fügen Sie sie in eine temporäre Textdatei. Sie werden diese Werte mit dem Facebook-Anbieter in Ihrem Websitecode übergeben.
8. Beenden Sie die Facebook-Entwickler-Website.

Nun können Sie Änderungen auf beiden Seiten auf Ihrer Website, damit Benutzer bei der Website mit ihren Facebook-Konten anmelden können.

1. Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die basierend auf der Vorlage Starter Site von WebMatrix.
2. Öffnen der  *\_AppStart.cshtml* Seite, und kommentieren Sie den Code für die Facebook-OAuth-Anbieter. Der auskommentierungen aufgehoben wurden Codeblock sieht folgendermaßen aus: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopieren der **App-ID** Wert aus der Facebook-Anwendung als Wert für die `appId` Parameter (innerhalb der Anführungszeichen).
4. Kopie **App-Geheimnis** Wert aus der Facebook-Anwendung als der `appSecret` Parameterwert.
5. Speichern und schließen Sie die Datei.

### <a name="testing-facebook-login"></a>Testen die Facebook-Anmeldung

1. Führen Sie die Website des *default.cshtml* Seite, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* auf der Seite die **verwenden Sie einen anderen Dienst anmelden** Abschnitt der **Facebook** Symbol. 

    Die Webseite leitet die Anforderung an die Facebook-Anmeldeseite.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Melden Sie sich ein Facebook-Konto. 

    Der Code wird das Facebook-Token zur Authentifizierung verwendet und gibt dann zurück zu einer Seite in dem Sie Ihre Facebook-Anmeldung mit Ihrer Website zuordnen. Ihr Benutzername oder e-Mail-Adresse wird ausgefüllt, in der **-e-Mail** Feld im Formular.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, auf der Startseite, und Sie angemeldet sind.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Aktivieren von Twitter-Anmeldungen

1. Navigieren Sie zu der [Twitter-Entwickler-Website](https://dev.twitter.com/).
2. Wählen Sie die **erstellen Sie eine App** verknüpfen, und melden Sie sich bei der Website.
3. Auf der **erstellen Sie eine Anwendung** bilden, füllen Sie die **Namen** und **Beschreibung** Felder.
4. In der **WebSite** Geben Sie die URL Ihrer Website (z. B. `http://www.example.com`). 

    > [!NOTE]
    > Wenn Sie Ihre Website lokal testen (über eine URL wie `http://localhost:12345`), ist die URL, Twitter nicht akzeptieren. Allerdings ist Sie möglicherweise die lokalen Loopback-IP-Adresse verwenden (z. B. `http://127.0.0.1:12345`). Dies vereinfacht das Testen der Anwendung lokal. Jedoch jedes Mal, wenn die Portnummer des lokalen Standorts geändert wird, Sie müssen beim Aktualisieren der **WebSite** Feld der Anwendung.
5. In der **Rückruf-URL** Geben Sie eine URL für die Seite Ihrer Website, die Benutzern, nach der Anmeldung bei Twitter zurückgegeben werden sollen. Geben Sie beispielsweise die gleiche URL, die Sie im eingegeben haben, um Benutzer zur Startseite der Starter Site zu senden (was deren Status angemeldeten erkannt wird), die **WebSite** Feld.
6. Akzeptieren Sie die Bedingungen, und wählen Sie die **Erstellen Ihrer Twitter-Anwendung** Schaltfläche.
7. Auf der **Meine Anwendungen** Startseite, wählen Sie die Anwendung, die Sie erstellt haben.
8. Auf der **Details** Registerkarte, scrollen Sie nach unten, und wählen Sie die **erstellen My Access Token** Schaltfläche.
9. Auf der **Details** Registerkarte, kopieren Sie die **Consumerschlüssel** und **Consumer Secret** Werte für Ihre Anwendung und fügen Sie sie in eine temporäre Textdatei. Sie müssen diese Werte mit dem Twitter-Anbieter in Ihrem Websitecode übergeben.
10. Die Twitter-Website zu beenden.

Nun können Sie Änderungen auf beiden Seiten auf Ihrer Website, damit Benutzer bei der Website mit ihren Twitter-Konten anmelden können.

1. Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die basierend auf der Vorlage Starter Site von WebMatrix.
2. Öffnen der  *\_AppStart.cshtml* Seite, und entfernen Sie den Code für den Twitter-OAuth-Anbieter. Der auskommentierungen aufgehoben wurden Codeblock sieht folgendermaßen aus: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerKey` Parameter (innerhalb der Anführungszeichen).
4. Kopieren der **Consumer Secret** Wert aus der Twitter-Anwendung als Wert für die `consumerSecret` Parameter.
5. Speichern und schließen Sie die Datei.

### <a name="testing-twitter-login"></a>Testen von Twitter-Anmeldung

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* auf der Seite die **verwenden Sie einen anderen Dienst anmelden** Abschnitt der **Twitter** Symbol. 

    Die Webseite leitet die Anforderung an eine Twitter-Anmeldeseite für die Anwendung, die Sie erstellt haben.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Melden Sie sich ein Twitter-Konto.
4. Der Code verwendet das Twitter-Token zur Authentifizierung des Benutzers und wechselt zurück zu einer Seite, in dem Sie zuordnen können Ihre Anmeldung mit Ihrem Konto für die Website. Ihren Namen oder e-Mail-Adresse wird ausgefüllt, in der **-e-Mail** Feld im Formular.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, auf der Startseite, und Sie angemeldet sind.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Anpassen des Verhaltens von Websiteseiten](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkID=202904)
