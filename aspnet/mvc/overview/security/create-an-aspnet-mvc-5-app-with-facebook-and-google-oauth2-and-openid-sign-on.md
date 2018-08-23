---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Erstellen von MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Webanwendung erstellen, die Benutzern ermöglicht, melden Sie sich mithilfe von OAuth 2.0 mit den Anmeldeinformationen aus einer externen ne...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833966"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Erstellen einer ASP.NET MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial erfahren Sie, wie zum Erstellen einer ASP.NET MVC 5-Webanwendung, die Benutzern ermöglicht, melden Sie sich mit [OAuth 2.0](http://oauth.net/2/) mit den Anmeldeinformationen eines externen Authentifizierungsanbieters, z. B. Facebook, Twitter, LinkedIn, Microsoft oder Google. Der Einfachheit halber konzentriert sich in diesem Tutorial zum Arbeiten mit Anmeldeinformationen aus Facebook und Google.
> 
> Aktivieren diese Anmeldeinformationen in Ihrer Web Sites bietet entscheidende Vorteile, da Sie Millionen Benutzer bereits Konten mit dieser externen Anbietern haben. Diese Benutzer möglicherweise eher für Ihre Website registrieren können, wenn sie nicht zum Erstellen und speichern einen neuen Satz von Anmeldeinformationen verfügen.
> 
> Siehe auch [ASP.NET MVC 5-app mit SMS und e-Mail-Adresse einer zweistufigen Authentifizierung](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Das Tutorial zeigt auch das Hinzufügen von Profildaten für den Benutzer und das Membership-API zu verwenden, um Rollen hinzuzufügen. In diesem Tutorial wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (mir bitte auf Twitter folgen: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Erste Schritte

Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren von Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher. Hilfe bei Dropbox, GitHub, Linkedin, Instagram, Puffer, Salesforce, DATENSTROM, Stack Exchange, Tripit, twitch integrierte, Twitter, Yahoo! und vieles mehr, finden Sie in diesem [Beispielprojekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Sie müssen Visual Studio installieren [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher, Google OAuth 2 zu verwenden und Lokales Debuggen ohne SSL-Warnungen.


Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie können, verwenden Sie das Menü, und wählen Sie **Datei**, und klicken Sie dann **neues Projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Klicken Sie auf **neues Projekt**, und wählen Sie dann **Visual C#-** auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**. Benennen Sie das Projekt "MvcAuth", und klicken Sie dann auf **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC**. Die Authentifizierung ist nicht **einzelne Benutzerkonten**, klicken Sie auf die **Authentifizierung ändern** Schaltfläche, und wählen **einzelne Benutzerkonten**. Durch Überprüfen **in der Cloud hosten**, die app wird sehr einfach, die in Azure gehostet werden.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Wenn Sie ausgewählt haben **in der Cloud hosten**, führen Sie das Dialogfeld "konfigurieren".

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Verwenden Sie NuGet, um auf die neueste OWIN-Middleware zu aktualisieren

Verwenden Sie den NuGet-Paket-Manager zum Aktualisieren der [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Wählen Sie **Updates** im linken Menü. Klicken Sie auf die **alle aktualisieren** Schaltfläche oder Sie können nur OWIN-Pakete (in der nächsten Abbildung dargestellt) suchen:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

In der folgenden Abbildung werden nur Pakete der OWIN angezeigt:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Über die Paket-Manager-Konsole (PMC), können Sie eingeben der `Update-Package` -Befehl, der alle Pakete aktualisiert wird.

Drücken Sie **F5** oder **STRG + F5** zum Ausführen der Anwendung. In der folgenden Abbildung ist die Nummer des Ports 1234. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

Je nach Größe Ihres Browserfensters, müssen Sie möglicherweise die Symbol "berichtsnavigation", um anzuzeigen, klicken Sie auf die **Startseite**, **zu**, **wenden Sie sich an**, **registrieren**und **melden Sie sich bei** Links.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Einrichten von SSL im Projekt

Um an den Authentifizierungsanbieter, z. B. Google und Facebook zu verbinden, müssen Sie IIS Express einrichten, um die Verwendung von SSL. Es ist wichtig, nach der Anmeldung mithilfe von SSL nicht und abzulegen und wieder auf HTTP, Ihre Anmeldecookie wird nur als Geheimnis Ihres Benutzernamens und Kennworts und ohne Verwendung von SSL, die Sie in Klartext über das Netzwerk gesendet. Darüber hinaus bietet Sie haben bereits die Zeit zum Ausführen des Handshakes und Sichern Sie den Kanal (Was ist der Großteil der macht langsamer als HTTP HTTPS) die MVC-Pipeline ausgeführt wird, nach dem Sie angemeldet sind also zurück an den HTTP-Umleitung wird nicht stellen Sie vor der aktuellen Anforderung oder in Zukunft Anforderungen, die viel schneller.

1. In **Projektmappen-Explorer**, klicken Sie auf die **MvcAuth** Projekt.
2. Drücken Sie die F4-Taste, um die Projekteigenschaften anzuzeigen. Sie können auch aus der **Ansicht** Menü Sie auswählen können, **Fenster "Eigenschaften"**.
3. Änderung **SSL-fähigen** auf "true".  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Kopieren Sie die SSL-URL (die `https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).
5. In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **MvcAuth** Projekt, und wählen **Eigenschaften**.
6. Wählen Sie die **Web** Registerkarte, und fügen Sie die SSL-URL in die **Projekt-Url** Feld. Speichern Sie die Datei (STRG + S). Sie benötigen diese URL zum Konfigurieren von Facebook und Google-Authentifizierung-apps.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Hinzufügen der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) -Attribut auf die `Home` Controller alle Anforderungen erforderlich ist, muss HTTPS verwenden. Eine sicherere Möglichkeit ist das Hinzufügen der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) Filter, um die Anwendung. Finden Sie im Abschnitt &quot;Schützen der Anwendung durch SSL und das Authorize-Attribut von&quot; in meiner tutoral [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein Teil der Home-Controller ist unten dargestellt.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Drücken Sie STRG+F5, um die Anwendung auszuführen. Wenn Sie das Zertifikat in der Vergangenheit installiert haben, können Sie das im restlichen Teil dieses Abschnitts überspringen und fahren Sie mit [beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt](#goog), führen Sie andernfalls die Anweisungen, um das selbstsignierte vertrauen Zertifikat, das IIS Express generiert hat.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lesen der **Sicherheitswarnung** Dialogfeld, und klicken Sie dann auf **Ja** ggf. zum Installieren des Zertifikats, das "localhost" darstellt.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Zeigt, d. h. die *Startseite* Seite, und es gibt keine SSL-Warnungen.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome ist außerdem das Zertifikat akzeptiert und HTTPS-Inhalt ohne Warnung zeigt. Firefox verwendet einen eigenen Zertifikatspeicher, damit sie eine Warnung angezeigt wird. Sie können problemlos klicken, für die Anwendung **ich verstehe die Risiken**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt

> [!WARNING]
> Aktuelle Google-OAuth-Anweisungen, finden Sie unter [Konfigurieren von Google-Authentifizierung in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Navigieren Sie zu der [Google-Entwicklerkonsole](https://console.developers.google.com/).
2. Wenn Sie ein Projekt, bevor Sie erstellt haben, wählen Sie **Anmeldeinformationen** in der linken Registerkarte, und wählen Sie dann **erstellen**.
3. Klicken Sie auf der linken Registerkarte auf **Anmeldeinformationen**.
4. Klicken Sie auf **Anmeldeinformationen erstellen** dann **OAuth-Client-ID**. 

    1. In der **Client-ID erstellen** Dialogfeld behalten Sie den Standardwert **Webanwendung** für den Anwendungstyp.
    2. Legen Sie die **autorisierte JavaScript** Ursprünge, die oben verwendeten SSL-URL (`https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben)
    3. Legen Sie die **autorisierte umleitungs-URI** auf:  
         `https://localhost:44300/signin-google`
5. Klicken Sie auf das Menüelement des OAuth-Zustimmung-Bildschirm, und setzen Sie Ihre e-Mail-Adresse und Product Name. Sie haben beim Klicken Sie im Formular **speichern**.
6. Klicken Sie auf das Menüelement für die Bibliothek, die nach **Google + API**, klicken Sie darauf, und drücken Sie dann aktivieren.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Die folgende Abbildung zeigt die aktivierte APIs.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Über den Google APIs-API-Manager finden Sie auf die **Anmeldeinformationen** Registerkarte zum Abrufen der **Client-ID**. Laden Sie dieses herunter, um eine JSON-Datei mit Geheimnissen aus Anwendungen zu speichern. Kopieren und Einfügen der **"ClientID"** und **ClientSecret** in die `UseGoogleAuthentication` Methode finden Sie unter den *Startup.Auth.cs* Datei die *App_Start* Ordner. Die **"ClientID"** und **ClientSecret** unten aufgeführten Werte sind Beispiele und funktionieren nicht.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern. Das Konto und die Anmeldeinformationen werden auf den Code aus, um das Beispiel möglichst einfach zu halten hinzugefügt. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendungs. Klicken Sie auf die **melden Sie sich bei** Link.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Klicken Sie unter **verwenden Sie einen anderen Dienst anmelden**, klicken Sie auf **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Wenn Sie einen der oben genannten Schritte verpasst haben, erhalten Sie einen HTTP 401-Fehler. Überprüfen Sie die obigen Schritte erneut. Wenn Sie eine erforderliche Einstellung verpasst haben (z. B. **Produktname**), fügen Sie das fehlende Element hinzu, und speichern, es dauert einige Minuten, bis die Authentifizierung funktioniert.
10. Sie werden zur Google-Website umgeleitet, wo Sie Ihre Anmeldeinformationen eingeben.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert werden, so erteilen Berechtigungen für die Webanwendung, die Sie gerade erstellt haben:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Klicken Sie auf **akzeptieren**. Jetzt werden Sie umgeleitet an die **registrieren** Seite der Anwendung MvcAuth, wo Sie Ihr Google-Konto registrieren. Sie haben die Möglichkeit, den Namen lokaler e-Mail-Registrierung für Ihr Gmail-Konto zu ändern, aber Sie möchten in der Regel behalten Sie das Standard-e-Mail-Alias (d. h. diejenige, die Sie für die Authentifizierung verwendet). Klicken Sie auf **Registrieren**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Erstellen die app in Facebook, und verbinden die app auf das Projekt

> [!WARNING]
> Aktuelle Anweisungen für die Facebook OAuth2-Authentifizierung, finden Sie unter [Konfigurieren der Facebook-Authentifizierung](/aspnet/core/security/authentication/social/facebook-logins)


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Überprüfen Sie die Mitgliedschaftsdaten

In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Erweitern Sie **DefaultConnection (MvcAuth)**, erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **"aspnetusers"** , und klicken Sie auf **Tabellendaten anzeigen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![Tabellendaten "aspnetusers"](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Hinzufügen von Profildaten zur Benutzerklasse

In diesem Abschnitt fügen Geburtsdatum und home Stadt auf die Benutzerdaten während der Registrierung, Sie wie in der folgenden Abbildung dargestellt.

![REG mit home Town und Geburtst.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Öffnen der *Models\IdentityModels.cs* -Datei und fügen Sie Birth Date als auch zu Hause Town-Eigenschaften:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Öffnen der *Models\AccountViewModels.cs* -Datei und die Gruppe birth Date als auch zu Hause Town-Eigenschaften in `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Öffnen der *controllers\accountcontroller* Datei, und fügen Sie Code für Birth Date als auch zu Hause Stadt in der `ExternalLoginConfirmation` Aktionsmethode wie gezeigt:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Geburtsdatum und home Town zum Hinzufügen der *Views\Account\ExternalLoginConfirmation.cshtml* Datei:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Löschen Sie die Mitgliedschaftsdatenbank, damit Sie erneut registrieren Sie Ihr Facebook-Konto mit Ihrer Anwendung und stellen Sie sicher, dass Sie die neue Geburtsdatum und home Town-Profilinformationen hinzufügen können.

Von **Projektmappen-Explorer**, klicken Sie auf die **alle Dateien anzeigen** Symbol, und klicken Sie dann auf Rechtsklick *hinzufügen\_Data\aspnet-MvcAuth -&lt;Datumsstempel&gt;-.mdf* , und klicken Sie auf **löschen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Von der **Tools** Menü klicken Sie auf **NuGet-Paket-Manager**, klicken Sie dann auf **-Paket-Manager-Konsole** (PMC). Geben Sie die folgenden Befehle aus, in der PMC.

1. Enable-Migrations
2. Add-Migration-Init
3. Datenbank aktualisieren

Führen Sie die Anwendung aus, und Verwenden von FaceBook und Google, sich anzumelden und einige Benutzer zu registrieren.

## <a name="examine-the-membership-data"></a>Überprüfen Sie die Mitgliedschaftsdaten

In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Klicken Sie mit der rechten Maustaste auf **"aspnetusers"** , und klicken Sie auf **Tabellendaten anzeigen**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Die `HomeTown` und `BirthDate` Felder werden unten angezeigt.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Der App abmelden und mit einem anderen Konto anmelden

Wenn Sie melden Sie sich an Ihre app mit Facebook, und melden Sie sich, und versuchen Sie es für die Anmeldung werden erneut mit einem anderen Facebook-Konto (mit dem gleichen Browser), Sie sofort mit der vorherigen Facebook-Konto angemeldet sein, die Sie verwendet. Um ein anderes Konto verwenden möchten, müssen Sie navigieren zu Facebook, und melden Sie sich bei Facebook. Das gleiche gilt für alle anderen 3rd Party Authentication-Anbieter. Alternativ können Sie mit einem anderen Konto anmelden, mit einem anderen Browser.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Einführung in die Yahoo und LinkedIn-OAuth-Sicherheitsanbieter für OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) von Jerrie Pelser Yahoo und LinkedIn-Anweisungen. Finden Sie unter der Jerrie so ziemlich sozialen Netzwerken basierende Anmeldung Schaltflächen für ASP.NET MVC 5, um die Anmeldung für soziale Netzwerke Schaltflächen aktivieren zu erhalten.

Führen Sie meine Tutorial [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), die in diesem Tutorial fortgesetzt, und zeigt die folgende:

1. Informationen zum Bereitstellen Ihrer app in Azure.
2. Wie Sie app-Rollen sichern.
3. So sichern Sie Ihre app mit der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) und [autorisieren](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) Filter.
4. Verwenden die Mitgliedschafts-API Benutzer und Rollen hinzufügen.

Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können. Sie können auch neue Themen anfordern [anzeigen Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Sie können sogar Fragen und stimmen über neue Funktionen in ASP.NET hinzugefügt werden. Sie können z. B. für ein Tool zum Abstimmen [erstellen und Verwalten von Benutzern und Rollen.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Eine gute Erklärung der Funktionsweise von ASP.NET External Authentication Services, finden Sie unter mcmurrays [externe Authentifizierungsdienste](https://asp.net/web-api/overview/security/external-authentication-services). Den Artikel von Robert wird auch im Detail, bei der Aktivierung von Microsoft und Twitter-Authentifizierung. Tom Dykstra die ausgezeichnete [EF/MVC-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) für die Arbeit mit dem Entity Framework veranschaulicht.
