---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "Erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, ein e-Mail-Bestätigung und das Kennwort zurücksetzen (c#) | Microsoft Docs"
author: Rick-Anderson
description: "In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Web-app mit e-Mail-Bestätigung und das Kennwort zurückzusetzen, verwenden das Mitgliedschaftssystem ASP.NET Identity zu erstellen. Sie Zertifizierungsstelle..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 5689031015279484cc616090a767a8c25eefa3c1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, ein e-Mail-Bestätigung und das Kennwort zurücksetzen (c#)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Web-app mit e-Mail-Bestätigung und das Kennwort zurückzusetzen, verwenden das Mitgliedschaftssystem ASP.NET Identity zu erstellen. Sie können herunterladen die fertigen Anwendung [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Der Download enthält die debugging-Hilfen, mit denen Sie die e-Mail-Bestätigung und SMS zu testen, ohne eine e-Mail oder SMS-Anbieter einrichten zu müssen.
> 
> Dieses Lernprogramm wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Erstellen einer ASP.NET MVC-app

Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: Sie installieren müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage. WebForms unterstützt auch ASP.NET Identity, damit Sie ähnliche Schritte in einer Web Forms-app folgen können.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert. Später in diesem Lernprogramm wird es in Azure bereitstellen. Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Legen Sie die [Projekt zur Verwendung von SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Führen Sie die app, klicken Sie auf die **registrieren** verknüpfen und einen Benutzer registrieren. An diesem Punkt wird die ausschließliche Überprüfung auf die e-Mail-Adresse mit der [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) Attribut.
5. Wechseln Sie in Server-Explorer zu **Daten Connections\DefaultConnection\Tables\AspNetUsers**, mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**.

    Die folgende Abbildung zeigt die `AspNetUsers` Schema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Klicken Sie mit der rechten Maustaste auf die **AspNetUsers** Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Die e-Mail wurde an diesem Punkt nicht bestätigt.
7. Klicken Sie auf die Zeile, und wählen Sie löschen. Sie fügen diese e-Mail erneut im nächsten Schritt hinzu, und senden eine e-Mail zur kaufbestätigung.

## <a name="email-confirmation"></a>E-Mail-Bestätigung

Es wird empfohlen, die e-Mail-Adresse eines eine neue benutzerregistrierung, um zu überprüfen, sind sie nicht eine andere Person Identität zu bestätigen (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse). Angenommen, Sie ein Diskussionsforum hatten, Sie möchten verhindern `"bob@example.com"` aus der Registrierung als `"joe@contoso.com"`. Ohne e-Mail-Bestätigung `"joe@contoso.com"` konnten unerwünschte e-Mails von Ihrer app abgerufen werden. Angenommen, Bob versehentlich als registriert `"bib@example.com"` und hadn't bemerkt, er wäre Kennwort wiederherstellen zu verwenden, da die app hat seine richtige e-Mail-Nachricht nicht. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und stellt keinen Schutz vor bestimmt Spammern, sie haben viele arbeiten e-Mail-Aliase, die sie verwenden können, um zu registrieren.

In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie per e-Mail, eine SMS oder einen anderen Mechanismus bestätigt wurden. <a id="build"></a>In den folgenden Abschnitten wird e-Mail-Bestätigung aktivieren und ändern Sie den Code, um zu verhindern, dass neu registrierte Benutzer anmelden, bis ihre e-Mails bestätigt wurde.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Verknüpfen mit SendGrid

Obwohl dieses Lernprogramm zeigt nur zum Hinzufügen von e-Mail-Benachrichtigung über [SendGrid](http://sendgrid.com/), können Sie e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden (finden Sie unter [zusätzliche Ressourcen](#addRes)).

1. Geben Sie den folgenden, in der Paket-Manager-Konsole den folgenden Befehl aus: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Wechseln Sie zu der [Azure SendGrid-Anmeldeseite](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) und kostenlos SendGrid-Konto registrieren. Fügen Sie Code ähnlich dem folgenden SendGrid konfigurieren:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Sie müssen das Hinzufügen eines u. a. folgende:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Um dieses Beispiel einfach zu halten, speichern wir die Appeinstellungen in der *"Web.config"* Datei:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sicherheit – sensible Daten nie im Quellcode speichern. Das Konto und die Anmeldeinformationen werden in der AppSetting gespeichert. Sie können auf Azure sicher diese Werte speichern, auf die  **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  Registerkarte im Azure-Portal. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Aktivieren von e-Mail-Bestätigung im Konto-controller

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Überprüfen Sie die *Views\Account\ConfirmEmail.cshtml* Datei hat die richtigen Razor-Syntax. (Der @-Zeichen in der ersten Zeile möglicherweise nicht vorhanden. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Führen Sie die app, und klicken Sie auf den Link registrieren. Nachdem Sie das Registrierungsformular übermitteln, sind Sie angemeldet.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Überprüfen Sie Ihr e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>E-Mail-Bestätigung vor dem Anmelden

Derzeit verwendet werden, nachdem ein Benutzer das Registrierungsformular abgeschlossen wurde, sind sie angemeldet. In der Regel möchten Sie ihre e-Mails zu überprüfen, bevor im Clientbrowser. In der folgende Abschnitt ändern wir den Code, um neue Benutzer, um eine bestätigte e-Mail-Adresse zu erhalten, bevor sie angemeldet sind (authentifiziert) erfordern. Update der `HttpPost Register` Methode mit den folgenden hervorgehobenen Änderungen:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Auskommentieren der `SignInAsync` -Methode, der Benutzer wird nicht signiert werden durch die Registrierung. Die `TempData["ViewBagLink"] = callbackUrl;` Zeile kann zur [Debuggen Sie die Anwendung](#dbg) und Testen Sie die Registrierung ohne das Senden von e-Mail. `ViewBag.Message`wird verwendet, um die Confirm-Anweisungen anzuzeigen. Die [Beispiel herunterladen](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) enthält Code, um e-Mail-Bestätigung zu testen, ohne das Einrichten von e-Mails und zum Debuggen der Anwendung eingesetzt werden können.

Erstellen Sie eine `Views\Shared\Info.cshtml` Datei, und fügen Sie das folgende Razor-Markup:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Hinzufügen der [Authorize-Attribut](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) auf die `Contact` Aktionsmethode des Home-Controllers. Sie können klicken Sie auf der **wenden Sie sich an** Link, um zu überprüfen, ob anonyme Benutzer haben keinen Zugriff und authentifizierte Benutzer haben Zugriff.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Sie müssen außerdem ein update der `HttpPost Login` Aktionsmethode:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Update der *Views\Shared\Error.cshtml* können Sie die Fehlermeldung anzeigen:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Löschen Sie alle Konten in der **AspNetUsers** Tabelle, die den e-Mail-Alias enthalten, Sie testen möchten. Führen Sie die app, und stellen Sie sicher, dass Sie sich anmelden können nicht, wenn Sie Ihre e-Mail-Adresse bestätigt haben. Nachdem Sie Ihre e-Mail-Adresse bestätigt haben, klicken Sie auf die **Kontakt** Link.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Zurücksetzen des Kennworts/Wiederherstellung

Entfernen Sie die Kommentarzeichen aus der `HttpPost ForgotPassword` Aktionsmethode im Kontocontroller:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Entfernen Sie die Kommentarzeichen aus der `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in der *Views\Account\Login.cshtml* Razor-Datei:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Die Anmeldeseite wird nun einen Link zum Zurücksetzen des Kennworts angezeigt.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Senden von e-Mail-Bestätigungslink

Sobald ein Benutzer ein neues lokales Konto erstellt, werden sie einen Link zur Bestätigung per e-Mail gesendet, den sie für erforderlich sind, verwenden, bevor sie sich anmelden können. Wenn der Benutzer versehentlich der e-Mail zur kaufbestätigung löscht oder die e-Mail-Adresse nie eingeht, benötigen sie den Link zur Bestätigung erneut gesendet. Die folgenden codeänderungen zeigen, wie dies zu ermöglichen.

Fügen Sie die folgende Hilfsmethode zum unteren Rand der *Controllers\AccountController.cs* Datei:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aktualisieren Sie die Register-Methode, um das neue Hilfsobjekt verwenden:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aktualisieren Sie die Login-Methode, um das Kennwort erneut zu senden bei, wenn das Benutzerkonto nicht bestätigt wurde:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Kombinieren von sozialen und lokalen Anmeldekonten

Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden. In der folgenden Reihenfolge  **RickAndMSFT@gmail.com**  wird zuerst als eine lokale Anmeldung erstellt, aber Sie können erstellen Sie das Konto als sozialen Protokoll zuerst und anschließend fügen Sie eine lokale Anmeldung hinzu.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Klicken Sie auf den Link zu einem anderen Protokoll im Dienst, und akzeptieren Sie die app-Anforderungen. Die beiden Konten wurden kombiniert, Sie können mit entweder Konto anmelden. Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls der Anmeldung sozialen Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.

In der folgenden Abbildung ist Tom soziales Netzwerk anzumelden (die daraufhin aus der **externer Anmeldungen: 1** auf der Seite angezeigt).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Durch Klicken auf **ein Kennwort** bietet die Möglichkeit, fügen ein lokales Protokoll auf dem gleichen Konto zugeordnet.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>E-Mail-Bestätigung in größerer

Meine Lernprogramm [Kontobestätigung und Kennwortwiederherstellung mit ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wechselt in diesem Thema mit Informationen.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debuggen der app

Wenn Sie eine e-Mail mit dem Link nicht erhalten:

- Überprüfen Sie Ihren Junk oder Spam-Ordner.
- Melden Sie sich bei Ihrem SendGrid-Konto, und klicken Sie auf die [-e-Mail von aktivitätsverknüpfungen](https://sendgrid.com/logs/index).

Um den Link für die Überprüfung per e-Mail zu testen, laden die [abgeschlossenen Beispiel](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Der Link zur Bestätigung und Bestätigungscodes werden auf der Seite angezeigt.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Links zu ASP.NET Identity empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konto zur Bestätigung und Kennwortwiederherstellung mit ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wird ausführlicher auf Wiederherstellung und Konto für die Bestätigung des Kennworts.
- [MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) dieses Lernprogramm veranschaulicht das Schreiben einer ASP.NET MVC 5-Apps mit Facebook und Google OAuth 2 Autorisierung. Es wird gezeigt, wie der Identity-Datenbank zusätzliche Daten hinzugefügt werden.
- [Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In diesem Lernprogramm fügt Azure-Bereitstellung zum Sichern Ihrer Apps mit Rollen, wie die Mitgliedschafts-API zu verwenden, um Benutzern und Rollen und zusätzliche Sicherheitsfunktionen hinzuzufügen.
- [Erstellen einer Google-app für OAuth 2, und verbinden die app zum Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Erstellen die app in Facebook, und verbinden die app zum Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Einrichten von SSL in das Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
