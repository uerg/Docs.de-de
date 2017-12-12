---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Unter Verwendung von externen Websites in einer ASP.NET-Webanwendung angemeldet Pages (Razor) Standort | Microsoft Docs
author: tfitzmac
description: "In diesem Artikel wird erläutert, wie der ASP.NET Web Pages (Razor)-Site, die mithilfe von Google, Facebook, Twitter, Yahoo und andere Standorte anmelden – d. h. wie unterstützt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Protokollierung bei der Verwendung von externen Sites in einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie der ASP.NET Web Pages (Razor)-Site, die mithilfe von Google, Facebook, Twitter, Yahoo und andere Standorte anmelden – d. h. wie OAuth- und OpenID an Ihrem Standort unterstützt.
> 
> Lernen Sie:
> 
> - Wie die Anmeldung von anderen Standorten aktiviert, bei der Verwendung der Vorlage Starter Site von WebMatrix.
> 
> Dies ist die ASP.NET-Funktion im Artikel eingeführt:
> 
> - Die `OAuthWebSecurity` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix-3

ASP.NET Web Pages bietet Unterstützung für [OAuth](http://oauth.net/) und [OpenID](http://openid.net/) Anbieter. Diese Anbieter können Sie Benutzern die Anmeldung in Ihre Website mit ihren vorhandenen Anmeldeinformationen von Facebook, Twitter, Microsoft und Google lassen. Beispielsweise können zur Anmeldung mit Facebook-Konto Benutzer eine Facebook-Symbol, wählen Sie dem diese an die Facebook-Anmeldeseite weitergeleitet, wo er ihre Benutzerinformationen ein. Sie können dann die Facebook-Anmeldung mit ihrem Konto auf der Website zuordnen. Eine verwandte-Erweiterung für die Web Pages-Mitgliedschaft-Funktionen ist, dass Benutzer mehrere Anmeldungen (einschließlich Anmeldungen über social Network-Sites) zuordnen, können mit einem einzelnen Konto auf Ihrer Website.

Dieses Bild zeigt die Anmeldeseite von der **Starter Site** Vorlage, in dem ein Benutzer eine Symbol für Facebook, Twitter, Google oder Microsoft zum Aktivieren der Protokollierung mit einem externen Konto auswählen kann:

![externen Anbietern](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Sie können OAuth- und OpenID-Mitgliedschaft aktivieren, indem Sie einige Codezeilen in Auskommentieren der **Starter Site** Vorlage. Die Methoden und Eigenschaften verwenden Sie zum Arbeiten mit dem OAuth und OpenID-Anbieter sind in der `WebMatrix.Security.OAuthWebSecurity` Klasse. Die **Starter Site** eine vollständige Mitgliedschaft-Infrastruktur umfasst die Vorlage, mit einer Anmeldeseite, eine Mitgliedschaftsdatenbank und der gesamte Code müssen Sie Benutzern die Anmeldung in Ihre Website mithilfe von lokalen Anmeldeinformationen oder die von einem anderen Standort zu ermöglichen .

Dieser Abschnitt enthält ein Beispiel, damit Benutzer von externen Standorten auf einer Website anzumelden, die basierend auf den **Starter Site** Vorlage. Nach dem Erstellen einer Startwebsite, führen Sie diese (Details folgen):

- Für die Standorte, die einen OAuth-Anbieter (Facebook, Twitter und Microsoft) verwenden, erstellen Sie eine Anwendung auf der externen Website an. Dies gibt Ihnen die Anwendungsschlüssel, die Sie benötigen, um die Login-Funktion für diese Standorte aufzurufen.
- Für Standorte, die einen OpenID-Anbieter (Google) verwenden, müssen Sie keine Anwendung zu erstellen. Für alle diese Standorte müssen Sie ein Konto verfügen, um anzumelden und entwickleranwendungen zu erstellen.

    > [!NOTE]
    > Microsoft-Anwendungen akzeptieren nur eine live-URL für eine Website verwenden, damit Sie eine lokale Website-URL für das Testen von Benutzernamen verwenden können.
- Bearbeiten Sie einige Dateien in Ihrer Website, um den entsprechenden Authentifizierungsanbieter anzugeben und eine Anmeldung auf der Website übermitteln, die Sie verwenden möchten.

Dieser Artikel enthält separate Anweisungen für die folgenden Aufgaben:

- [Aktivieren des Google-Anmeldungen](#To_enable_Google_logins)
- [Aktivieren der Facebook-Anmeldungen](#To_enable_Facebook_logins)
- [Aktivieren der Twitter-Anmeldungen](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Aktivieren des Google-Anmeldungen

1. Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die auf der Vorlage Starter Site von WebMatrix basiert.
2. Öffnen der  *\_AppStart.cshtml* Seite und die auskommentierung der folgenden Codezeile. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testen von Google-Anmeldung

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **melden Sie sich** Schaltfläche.
2. Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt, und wählen Sie entweder die **Google** oder **Yahoo** Schaltfläche zum Absenden. Dieses Beispiel verwendet die Google-Anmeldung. 

    Die Webseite leitet die Anforderung an der Google-Anmeldeseite.

    ![Google-Anmeldung](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Geben Sie Anmeldeinformationen für ein vorhandenes Google-Konto ein.
4. Wenn Sie Google Sie gefragt werden, ob Sie zulassen möchten *"localhost"* um Informationen aus dem Konto zu verwenden, klicken Sie auf **zulassen**.

    Der Code verwendet das Google-Token zur Authentifizierung des Benutzers, und klicken Sie dann auf Ihrer Website auf diese Seite gibt. Auf dieser Seite können Benutzer ihre Google-Anmeldung mit einem vorhandenen Konto auf Ihrer Website zu verknüpfen, oder sie können ein neues Konto an Ihrem Standort ordnen Sie die externe Anmeldung mit registrieren.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Wählen Sie die **zuordnen** Schaltfläche. Der Browser gibt zur Startseite der Anwendung.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Aktivieren der Facebook-Anmeldungen

1. Wechseln Sie zu der [Facebook-Entwickler-Website](https://developers.facebook.com/apps) (Melden Sie sich, wenn Sie noch nicht angemeldet sind).
2. Wählen Sie die **neue App** Schaltfläche und befolgen Sie dann die Anweisungen, die neue Anwendung erstellen und benennen.
3. Im Abschnitt **wählen Sie die Integration Ihrer app mit Facebook**, wählen Sie die **Website** Abschnitt.
4. Füllen Sie die **Website-URL** Feld mit der URL Ihrer Website (z. B. `http://www.example.com`). Die **Domäne** Feld ist optional; Sie können Hiermit können Sie die Bereitstellung der Authentifizierung für eine gesamte Domäne (z. B. *example.com*). 

    > [!NOTE]
    > Wenn Sie einen Standort, auf dem lokalen Computer mit einer URL ausgeführt werden wie `http://localhost:12345` (wobei die Anzahl ist eine lokale Portnummer), können Sie diesen Wert zum Hinzufügen der **Website-URL** bei Testen Ihrer Website im Feld. Jedoch jedes Mal, wenn die Portnummer des lokalen standortänderungen, müssen Sie beim Aktualisieren der **Website-URL** Feld Ihrer Anwendung.
5. Wählen Sie die **Änderungen speichern** Schaltfläche.
6. Wählen Sie die **Apps** Registerkarte erneut aus, und zeigen Sie dann auf die Startseite für die Anwendung.
7. Kopieren der **App-ID** und **App-Geheimnis** Werte für Ihre Anwendung und in eine temporäre Textdatei einfügen. Diese Werte werden an dem Facebook-Anbieter in Ihrem Websitecode übergeben werden.
8. Beenden Sie die Facebook-Entwicklerwebsite ein.

Jetzt nehmen Sie Änderungen an zwei Seiten in Ihrer Website, damit Benutzer können sich bei der Website mithilfe ihrer Facebook-Konten anmelden können.

1. Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die auf der Vorlage Starter Site von WebMatrix basiert.
2. Öffnen der  *\_AppStart.cshtml* Seite und die auskommentierung des Codes für die Facebook-OAuth-Anbieter. Der unkommentierten Codeblock sieht wie folgt aus: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopieren der **App-ID** Wert aus der Facebook-Anwendung als Wert für die `appId` Parameter (in Anführungszeichen).
4. Kopie **App-Geheimnis** Wert aus der Facebook-Anwendung als der `appSecret` Parameterwert.
5. Speichern und schließen Sie die Datei.

### <a name="testing-facebook-login"></a>Testen der Facebook-Anmeldung

1. Führen Sie des Standorts *default.cshtml* Seite, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt der **Facebook** Symbol. 

    Die Webseite leitet die Anforderung an die Facebook-Anmeldeseite.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Melden Sie sich eine Facebook-Konto. 

    Der Code verwendet das Facebook-Token, um Sie zu authentifizieren und gibt dann zurück auf eine Seite in dem Sie Ihre Facebook-Anmeldung mit Ihrer Website Anmeldung zuordnen. Der Name oder e-Mail-Adresse eines Benutzers in gefüllt wird die **E-Mail** Feld im Formular.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, um zur Startseite, und Sie angemeldet sind.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Aktivieren der Twitter-Anmeldungen

1. Navigieren Sie zu der [Twitter-Entwickler-Website](https://dev.twitter.com/).
2. Wählen Sie die **Erstellen einer App** verknüpfen, und melden Sie sich dann bei der Website.
3. Auf der **erstellen Sie eine Anwendung** bilden, "füllen" der **Namen** und **Beschreibung** Felder.
4. In der **WebSite** Feld, geben Sie die URL Ihrer Website (z. B. `http://www.example.com`). 

    > [!NOTE]
    > Wenn Sie Ihre Website lokal testen (über eine URL wie `http://localhost:12345`), ist die URL, Twitter nicht akzeptieren. Allerdings können Sie möglicherweise die lokalen Loopback-IP-Adresse verwenden (z. B. `http://127.0.0.1:12345`). Dies vereinfacht das Testen Ihrer Anwendung lokal. Jedoch jedes Mal, wenn die Portnummer Ihres lokalen Standorts geändert wird, Sie müssen zum Aktualisieren der **WebSite** Feld Ihrer Anwendung.
5. In der **Rückruf-URL** Feld, geben Sie eine URL für die Seite in Ihrer Website, die Benutzern, nach der Anmeldung in Twitter zurückgegeben werden sollen. Geben Sie beispielsweise die gleiche URL, die Sie in eingegeben haben, um Benutzer zur Startseite des Starter-Site zu senden (die ihre angemeldeten Status erkannt wird), die **WebSite** Feld.
6. Akzeptieren Sie die Bestimmungen ein, und wählen Sie die **die Twitter-Anwendung erstellen** Schaltfläche.
7. Auf der **Meine Anwendungen** Angebotsseite, wählen Sie die Anwendung, die Sie erstellt haben.
8. Auf der **Details** Registerkarte, führen Sie einen Bildlauf nach unten aus, und wählen Sie die **erstellen Sie eigene Zugriffstoken** Schaltfläche.
9. Auf der **Details** Registerkarte, kopieren Sie die **Consumerschlüssel** und **Consumerschlüssel** Werte für Ihre Anwendung und in eine temporäre Textdatei einfügen. Sie müssen diese Werte in Ihrem Websitecode auf Twitter-Anbieter übergeben.
10. Die Twitter-Website zu beenden.

Jetzt nehmen Sie Änderungen an zwei Seiten in Ihrer Website, damit Benutzer werden bei der Website mithilfe ihrer Konten Twitter anmelden können.

1. Erstellen Sie oder öffnen Sie eine ASP.NET Web Pages-Website, die auf der Vorlage Starter Site von WebMatrix basiert.
2. Öffnen der  *\_AppStart.cshtml* Seite und die auskommentierung des Codes für den Twitter-OAuth-Anbieter. Der unkommentierten Codeblock sieht wie folgt: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerKey` Parameter (in Anführungszeichen).
4. Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerSecret` Parameter.
5. Speichern und schließen Sie die Datei.

### <a name="testing-twitter-login"></a>Testen der Twitter-Anmeldung

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt der **Twitter** Symbol. 

    Die Webseite leitet die Anforderung an einen Twitter-Anmeldeseite für die Anwendung, die Sie erstellt haben.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Melden Sie sich ein Twitter-Konto.
4. Der Code verwendet das Twitter-Token zur Authentifizierung des Benutzers und wechselt zurück zu einer Seite, in dem Sie zuordnen können Ihre Anmeldung mit Ihrem Konto für die Website. Der Name oder e-Mail-Adresse in gefüllt wird die **E-Mail** Feld im Formular.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, um zur Startseite, und Sie angemeldet sind.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Anpassen des Verhaltens der standortweiten](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkID=202904)
