---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Erstellen von MVC 5-App mit Facebook, Twitter, LinkedIn und Google "oauth2" Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: "In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Webanwendung zu erstellen, die Benutzern ermöglicht, melden Sie sich mithilfe von OAuth 2.0 mit Anmeldeinformationen aus einer externen authentifizieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: ccf4329e6684d07570bfaabfaa1a570664fb2ca3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Erstellen einer ASP.NET MVC 5-App mit Facebook, Twitter, LinkedIn und Google "oauth2" Sign-on (c#)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> Dieses Lernprogramm veranschaulicht das Erstellen einer ASP.NET MVC 5-Webanwendung, die Benutzern ermöglicht, melden Sie sich mit [OAuth 2.0](http://oauth.net/2/) mit Anmeldeinformationen eines externen Authentifizierungsanbieters, z. B. Facebook, Twitter, LinkedIn, Microsoft oder Google. Der Einfachheit halber konzentriert sich dieses Lernprogramms zum Arbeiten mit den Anmeldeinformationen von Facebook und Google.
> 
> Aktivieren diese Anmeldeinformationen auf Ihre Websites bietet einen erheblichen Vorteil Schreibberechtigung Millionen Benutzer bereits Konten mit diesen externen Anbietern. Diese Benutzer möglicherweise eher für Ihre Website registrieren können, wenn sie nicht zum Erstellen und speichern einen neuen Satz von Anmeldeinformationen verfügen.
> 
> Siehe auch [ASP.NET MVC 5-Anwendung mit SMS und e-Mail-Zweifaktorenauthentifizierung](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Das Lernprogramm zeigt auch zum Hinzufügen von Profildaten für den Benutzer sowie zum Membership-API zu verwenden, um Rollen hinzuzufügen. Dieses Lernprogramm wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (folgen Sie mir bitte auf Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Erste Schritte

Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren von Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher. Hilfe bei Dropbox, GitHub, Linkedin, Instagram, Puffer, Salesforce, DATENSTROM, Stapel Exchange, Tripit, Twitch, Twitter, Yahoo und vieles mehr, finden Sie in diesem [eine Stop-Handbuch](http://www.oauthforaspnet.com/).

> [!NOTE]
> Sie müssen Visual Studio installieren [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher, Google OAuth 2 verwenden und lokal ohne SSL-Warnungen zu debuggen.


Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie können, verwenden Sie das Menü, und wählen **Datei**, und klicken Sie dann **neues Projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Erstellen einer ersten Anwendung

Klicken Sie auf **neues Projekt**, und wählen Sie dann **Visual C#-** auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**. Namen für das Projekt "MvcAuth", und klicken Sie dann auf **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC**. Wenn die Authentifizierung nicht **einzelne Benutzerkonten**, klicken Sie auf die **Authentifizierung ändern** Schaltfläche und wählen Sie **einzelne Benutzerkonten**. Durch Überprüfen **Host in der Cloud**, wird die app sehr einfach, die in Azure gehostet werden.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Wenn Sie ausgewählt haben **Host in der Cloud**, führen Sie das Dialogfeld "konfigurieren".

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Mithilfe von NuGet auf die neueste owin-Middleware aktualisieren

Verwenden Sie den NuGet-Paket-Manager beim Aktualisieren der [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Wählen Sie **Updates** im linken Menü. Klicken Sie auf die **alle aktualisieren** Schaltfläche oder Sie können nur owin-Pakete (in der nächsten Abbildung dargestellt) suchen:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

In der folgenden Abbildung werden nur die OWIN-Pakete angezeigt:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Aus Paket-Manager-Konsole (PMC), geben Sie den `Update-Package` Befehl, der alle Pakete aktualisiert wird.

Drücken Sie **F5** oder **STRG + F5** um die Anwendung auszuführen. In der folgenden Abbildung ist die Portnummer 1234. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

Je nach Größe Ihres Browserfensters, müssen Sie möglicherweise das Symbol "Navigation" anzeigen klicken Sie auf die **Startseite**, **zu**, **Kontakt**, **registrieren**und **melden Sie sich** Links.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Einrichten von SSL in das Projekt

Zum Authentifizierungsanbieter z. B. Google und Facebook herstellen, müssen Sie die Installation von IIS Express einrichten, um die Verwendung von SSL. Es ist wichtig, die mithilfe von SSL nach der Anmeldung nicht und abzulegen und wieder auf HTTP, Ihre Anmeldecookie wird nur als geheimer Schlüssel Ihres Benutzernamens und Kennworts und ohne Verwendung von SSL, die Sie in Klartext über das Netzwerk gesendet. Darüber hinaus haben Sie bereits die Zeitdauer zum Ausführen des Handshakes und Sichern Sie die (das ist der Großteil der macht langsamer als HTTP HTTPS) ausgeführt, bevor die MVC-Pipeline ausgeführt wird, also wieder in den HTTP-umleiten, nachdem Sie angemeldet sind Stellen wird nicht die aktuelle Anforderung oder zukünftige Anforderungen, die wesentlich schneller.

1. In **Projektmappen-Explorer**, klicken Sie auf die **MvcAuth** Projekt.
2. Drücken Sie die F4-Taste, um die Projekteigenschaften anzuzeigen. Alternativ können Sie aus der **Ansicht** Menü Sie auswählen können, **Fenster "Eigenschaften"**.
3. Änderung **SSL aktiviert** auf "true".  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Kopieren Sie die SSL-URL (der ist `https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).
5. In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **MvcAuth** Projekt, und wählen Sie **Eigenschaften**.
6. Wählen Sie die **Web** Registerkarte, und fügen Sie die SSL-URL in die **Projekt-Url** Feld. Speichern Sie die Datei (STRG + S). Sie benötigen diese URL, Facebook und Google-Authentifizierung-apps zu konfigurieren.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Hinzufügen der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) -Attribut auf die `Home` Controller aus, um alle Anforderungen benötigen muss HTTPS verwenden. Ein sicherer Ansatz ist, fügen die [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) Filter zur Anwendung. Finden Sie im Abschnitt &quot;schützen Sie die Anwendung mit SSL und dem Attribut autorisieren&quot; in meinem tutoral [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein Teil der Home-Controller ist unten dargestellt.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Drücken Sie STRG+F5, um die Anwendung auszuführen. Wenn Sie das Zertifikat in der Vergangenheit installiert haben, überspringen Sie die restlichen Teil dieses Abschnitts und springen zu [beim Erstellen einer Google-app für OAuth-2, und verbinden die app auf das Projekt](#goog), führen Sie andernfalls die Anweisungen, um das selbstsignierte vertrauen Zertifikat, das IIS Express generiert hat.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lesen der **Sicherheitswarnung** Dialogfeld und klicken Sie dann auf **Ja** gegebenenfalls zum Installieren des Zertifikats, das "localhost" darstellt.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Zeigt, d. h. die *Home* Seite, und es werden keine SSL-Warnungen.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome auch akzeptiert das Zertifikat und HTTPS-Inhalt ohne eine Warnung wird angezeigt. Firefox verwendet einen eigenen Zertifikatspeicher an, damit sie eine Warnung angezeigt wird. Sie können problemlos klicken, für die Anwendung **ich verstehe, dass die Risiken**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Erstellen einer Google-app für OAuth 2, und verbinden die app zum Projekt

1. Navigieren Sie zu der [Google-Entwicklerkonsole](https://console.developers.google.com/).
1. Wenn Sie ein Projekt, bevor Sie erstellt haben, wählen Sie **Anmeldeinformationen** die Registerkarte "Links", und wählen Sie dann **erstellen**.
1. Klicken Sie in der linken Registerkarte auf **Anmeldeinformationen**.
1. Klicken Sie auf **Anmeldeinformationen erstellen** dann **OAuth-Client-ID**. 

    1. In der **Client-ID erstellen** Dialogfeld, behalten Sie den Standardwert **-Webanwendung** für den Anwendungstyp.
    2. Festlegen der **autorisiert JavaScript** Ursprünge auf dem oben verwendeten SSL-URL (`https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben)
    3. Legen Sie die **autorisierte umleitungs-URI** an:  
         `https://localhost:44300/signin-google`
5. Klicken Sie auf das Menüelement des OAuth-Zustimmung-Bildschirm, und legen Sie die e-Mail-Adresse und Product Name. Wenn Sie das Formular auf abgeschlossen haben **speichern**.
6. Klicken Sie auf das Menüelement für die Bibliothek, die nach **Google + API**, klicken Sie darauf, und drücken Sie dann aktivieren.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 Die folgende Abbildung zeigt die aktivierte APIs.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. In der Google APIs API-Manager finden Sie auf der **Anmeldeinformationen** Registerkarte zum Abrufen der **Clientkennung**. Herunterladen eine JSON-Datei mit geheimen Schlüsseln die Anwendung zu speichern. Kopieren und Einfügen der **ClientId** und **ClientSecret** in der `UseGoogleAuthentication` Methode gefunden, der *Startup.Auth.cs* in der Datei die *App_Start* Ordner. Die **ClientId** und **ClientSecret** unten aufgeführten Werte sind Beispiele und funktionieren nicht.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sicherheit – sensible Daten nie im Quellcode speichern. Das Konto und die Anmeldeinformationen werden der Code oben, um das Beispiel einfach zu halten hinzugefügt. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendung. Klicken Sie auf die **melden Sie sich** Link.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Klicken Sie unter **einen anderen Dienst zum Anmelden verwenden**, klicken Sie auf **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Wenn Sie eine der oben aufgeführten Schritte verpasst haben, erhalten Sie einen HTTP 401-Fehler. Überprüfen Sie die obigen Schritte erneut. Wenn Sie eine erforderliche Einstellung verpasst haben (z. B. **Produktname**), Element, und speichern Sie die fehlende hinzufügen, die es dauert einige Minuten, bis die Authentifizierung funktioniert.
10. Sie werden auf der Google-Website umgeleitet werden, in dem Sie Ihre Anmeldeinformationen eingeben.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert, Berechtigungen für die Webanwendung zu geben, die Sie soeben erstellt haben:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Klicken Sie auf **akzeptieren**. Nun werden Sie umgeleitet an die **registrieren** Seite der Anwendung MvcAuth, wo Sie Ihre Google-Konto registrieren. Sie haben die Möglichkeit, das Ändern des lokalen e-Mail-Registrierung für Ihr Konto Gmail verwendet, aber im Allgemeinen das standardmäßige e-Mail-Alias (d. h., die Sie für die Authentifizierung verwendet eine) bleiben sollen. Klicken Sie auf **Registrieren**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Erstellen die app in Facebook, und verbinden die app zum Projekt

Für die Facebook OAuth2-Authentifizierung müssen Sie dem Projekt einige Einstellungen aus einer Anwendung kopieren, die Sie in Facebook zu erstellen.

1. In Ihrem Browser, navigieren Sie zu [https://developers.facebook.com/apps](https://developers.facebook.com/apps) und melden Sie sich, indem Sie Ihre Facebook-Anmeldeinformationen eingeben.
2. Wenn Sie bereits als ein Facebook-Entwickler registriert werden nicht, klicken Sie auf **registrieren Sie als Entwickler** und befolgen Sie die Anweisungen zum Registrieren.
3. Auf der **Apps** auf **neue App**.

    ![Neue app erstellen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Geben Sie eine **Anwendungsnamen** und **Kategorie**, klicken Sie dann auf **erstellen App**.

    Dies muss innerhalb der Facebook eindeutig sein. Die **App Namespace** ist der Teil der URL, die Ihre App verwenden möchten, auf die Facebook-Anwendung für die Authentifizierung (z. B. https://apps.facebook.com/ {App-Namespace}) zugreifen. Wenn Sie nicht angeben einer **App Namespace**, die **App-ID** für die URL verwendet werden. Die **App-ID** ist eine lange vom System generierte Zahl, die Sie im nächsten Schritt sehen.

    ![Neuen App-Dialogfeld "erstellen"](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Senden Sie die Standardsicherheit Kontrollkästchen.

    ![Sicherheitsüberprüfung](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Wählen Sie **Einstellungen** für den linken Menü überwachungsabfragebalkens![ Facebook-Entwickler-Menüleiste](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Auf der **grundlegende** Abschnitt der Seite Einstellungen wählen **Plattform hinzufügen** um anzugeben, dass Sie eine Websiteanwendung hinzufügen. ![Grundlegende Einstellungen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Wählen Sie **Website** aus den Plattformoptionen.  
  
    ![Plattform-Optionen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Notieren Sie sich Ihre **App-ID** und Ihre **App-Geheimnis** , damit Sie später in diesem Lernprogramm beide Zertifikate in der MVC-Anwendung hinzufügen können. Darüber hinaus fügen Sie der Website-URL (`https://localhost:44300/`) zum Testen Ihrer MVC-Anwendung. Fügen Sie außerdem eine **Contact Email**. Aktivieren Sie das Kontrollkästchen **Änderungen speichern**.   

    ![Seite "Details" basisanwendung](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Beachten Sie, dass Sie nur authentifizieren mit dem e-Mail-Alias, den Sie registriert haben. Andere Benutzer und Testkonten werden nicht registrieren können. Sie können andere Facebook Konten Zugriff auf die Anwendung auf die Facebook gewähren **Developer Rollen** Registerkarte.
10. Öffnen Sie in Visual Studio *App\_Start\Startup.Auth.cs*.
11. Kopieren Sie die **AppId** und **App-Geheimnis** in die `UseFacebookAuthentication` Methode. Die **AppId** und **App-Geheimnis** unten aufgeführten Werte sind Beispiele und funktionieren nicht.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Klicken Sie auf **Änderungen speichern**.
13. Drücken Sie **STRG + F5** um die Anwendung auszuführen.


Wählen Sie **melden Sie sich** auf die Anmeldeseite angezeigt. Klicken Sie auf **Facebook** unter **einen anderen Dienst zum Anmelden verwenden.**

Geben Sie Ihre Facebook-Anmeldeinformationen ein.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Sie werden aufgefordert, die Berechtigung für die Anwendung den Zugriff auf Ihr öffentliches Profil und "Friend" Liste erteilen.

![Facebook-app-details](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Sie sind jetzt angemeldet. Sie können dieses Konto jetzt mit der Anwendung registrieren.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Wenn Sie sich registrieren, wird ein Eintrag hinzugefügt, um die *Benutzer* Tabelle der Mitgliedschaftsdatenbank.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Überprüfen Sie die Daten der Benutzergruppenmitgliedschaft

In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Erweitern Sie **DefaultConnection (MvcAuth)**, erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **AspNetUsers** , und klicken Sie auf **Tabellendaten anzeigen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![Tabellendaten aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Hinzufügen von Profildaten für die Benutzerklasse

In diesem Abschnitt fügen Geburtsdatum und home Stadt auf die Daten des Benutzers während der Registrierung, Sie wie in der folgenden Abbildung gezeigt.

![REG mit home Stadt und Geburtst.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Öffnen der *Models\IdentityModels.cs* Datei und der Eigenschaften für die Datums- und von zuhause Örtlichkeit Geburtsdatum hinzufügen:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Öffnen der *Models\AccountViewModels.cs* Datei- und welche birth Date und von zuhause Örtlichkeit Eigenschaften in `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Öffnen der *Controllers\AccountController.cs* Datei, und fügen Sie Code für Birth Date und von zuhause Stadt, in der `ExternalLoginConfirmation` Aktionsmethode wie gezeigt:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Geburtsdatum und home Örtlichkeit zum Hinzufügen der *Views\Account\ExternalLoginConfirmation.cshtml* Datei:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Löschen Sie die Mitgliedschaftsdatenbank, damit Sie erneut Ihrer Facebook-Konto mit Ihrer Anwendung registrieren und stellen Sie sicher, dass Sie die neue Geburtsdatum und Profilinformationen home Örtlichkeit hinzufügen können.

Von **Projektmappen-Explorer**, klicken Sie auf die **alle Dateien anzeigen** Symbol und dann mit der rechten Maustaste *hinzufügen\_Data\aspnet-MvcAuth -&lt;Datumsstempel&gt;mdf* , und klicken Sie auf **löschen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Aus der **Tools** Menü klicken Sie auf **NuGet-Paket-Manager**, klicken Sie dann auf **Package Manager Console** (PMC). Geben Sie die folgenden Befehle in der Systemmonitor aus.

1. Enable-Migrationen
2. Init hinzufügen-Migration
3. Datenbank aktualisieren

Führen Sie die Anwendung aus, und Verwenden von FaceBook und Google anmelden und registrieren einige Benutzer.

## <a name="examine-the-membership-data"></a>Überprüfen Sie die Daten der Benutzergruppenmitgliedschaft

In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Klicken Sie mit der rechten Maustaste auf **AspNetUsers** , und klicken Sie auf **Tabellendaten anzeigen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Die `HomeTown` und `BirthDate` Felder unten angezeigt.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Ihre App abmelden und mit einem anderen Konto anmelden

Melden Sie sich bei Ihrer app mit Facebook, und klicken Sie dann melden Sie sich ab und versucht, melden Sie sich werden erneut mit einem anderen Facebook-Konto (mit dem gleichen Browser), Sie sofort auf den vorherigen Facebook-Konto angemeldet sein, die Sie verwendet. Um ein anderes Konto verwenden zu können, müssen Sie zum Facebook zu navigieren, und melden Sie sich am Facebook. Die gleiche Regel gilt für alle anderen 3rd Party Authentifizierungsanbieter. Alternativ können Sie mit einem anderen Konto anmelden, mit einem anderen Browser.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Einführung in die Yahoo und LinkedIn OAuth Sicherheitsanbieter für OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) von Jerrie Pelser Anweisungen Yahoo und LinkedIn aktualisiert. Finden Sie unter der Jerrie ziemlich sozialen Anmeldeschaltflächen für ASP.NET MVC 5 abzurufenden sozialen Anmeldeschaltflächen aktivieren.

Führen Sie meine Lernprogramm [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), die in diesem Lernprogramm fortgesetzt und zeigt die folgende:

1. Wie Sie Ihre app in Azure bereitstellen.
2. Wie Sie Apps mit Rollen gesichert.
3. Zum Sichern Ihrer Apps mit der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) und [autorisieren](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) Filter.
4. Wie die Mitgliedschafts-API zu verwenden, um Benutzer und Rollen hinzuzufügen.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Sie können sogar angefordert und Abstimmen über neue Funktionen in ASP.NET hinzugefügt werden. Sie können z. B. für ein Tool zum Abstimmen [erstellen und Verwalten von Benutzern und Rollen.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Eine gute Erklärung der Funktionsweise von ASP.NET externen Authentifizierungsdienste, finden Sie unter mcmurrays [externen Authentifizierungsdienste](https://asp.net/web-api/overview/security/external-authentication-services). Roberts Artikel geht auch ins Detail, bei der Aktivierung von Microsoft und Twitter-Authentifizierung. Tom Dykstra des ausgezeichnete [EF/MVC-Lernprogramm](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) wird gezeigt, wie mit dem Entity Framework funktionieren.
