---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Erstellen eine sichere ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Web-app mit e-Mail-Bestätigung und kennwortzurücksetzung mithilfe von ASP.NET Identity-Mitgliedschaftssystem zu erstellen. Sie Zertifizierungsstelle...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387042"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Erstellen einer sicheren ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Web-app mit e-Mail-Bestätigung und kennwortzurücksetzung mithilfe von ASP.NET Identity-Mitgliedschaftssystem zu erstellen. Sie können die fertige Anwendung herunterladen [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Der Download enthält Debuggen-Hilfsprogramme, mit denen Sie die Bestätigung per e-Mail und SMS zu testen, ohne das Einrichten einer e-Mail oder SMS-Anbieter.
> 
> In diesem Tutorial wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Erstellen einer ASP.NET MVC-app

Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage. Web Forms unterstützt auch ASP.NET Identity, sodass Sie ähnliche Schritte in einer Web Forms-app ausführen konnte.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert. Später in diesem Tutorial werden wir in Azure bereitstellen. Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Legen Sie die [Projekt für die Verwendung von SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Die app auszuführen, klicken Sie auf die **registrieren** verknüpfen, und registrieren Sie einen Benutzer. Die ausschließliche Überprüfung der auf die e-Mail-Adresse an diesem Punkt ist, mit der [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) Attribut.
5. Wechseln Sie in Server-Explorer zu **Daten Connections\DefaultConnection\Tables\AspNetUsers**, mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**.

    Die folgende Abbildung zeigt die `AspNetUsers` Schema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Klicken Sie mit der rechten Maustaste auf die **"aspnetusers"** Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Die e-Mail wurde an diesem Punkt nicht bestätigt.
7. Klicken Sie auf die Zeile, und wählen Sie löschen. Sie fügen Sie diese e-Mail erneut im nächsten Schritt hinzu, und eine Bestätigung per e-Mail zu senden.

## <a name="email-confirmation"></a>E-Mail-Bestätigung

Es hat sich bewährt, bestätigen die e-Mail-Adresse einer neuen benutzerregistrierung, um zu überprüfen, ob sie keine Identität einer anderen Person (d. h. sie noch nicht registriert mit einer anderen Person für den e-Mail-Adresse). Angenommen, Sie ein Diskussionsforum hatten, Sie möchten zu verhindern, dass `"bob@example.com"` aus der Registrierung als `"joe@contoso.com"`. Ohne e-Mail-Bestätigung `"joe@contoso.com"` unerwünschte e-Mail-Adresse Ihrer app abrufen konnte. Angenommen, das Bob versehentlich als registriert `"bib@example.com"` und noch nicht bemerkt, er wäre Kennwort wiederherstellen zu verwenden, da die app nicht seine richtige e-Mail-Nachricht nicht. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und nicht bieten Schutz vor bestimmt Spammern, sie haben viele funktionierenden e-Mail-Aliase, die sie verwenden können, um zu registrieren.

Im Allgemeinen möchten Sie verhindern, dass neue Benutzer keine Daten zu Ihrer Website veröffentlichen, bevor sie per e-Mail, eine SMS oder einen anderen Mechanismus bestätigt wurden. <a id="build"></a>In den folgenden Abschnitten werden wir Aktivieren von e-Mail-Bestätigung und ändern Sie den Code, um zu verhindern, dass die neu registrierte Benutzer anmelden, bis ihre e-Mail-Adresse bestätigt wurde.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Verknüpfen mit SendGrid

Obwohl dieses Tutorial nur das zeigt Hinzufügen von e-Mail-Benachrichtigung über [SendGrid](http://sendgrid.com/), senden Sie eine e-Mail mit SMTP und andere Mechanismen (finden Sie unter [Zusatzressourcen](#addRes)).

1. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Wechseln Sie zu der [Anmeldeseite von Azure-SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) und registrieren Sie sich für ein kostenloses SendGrid-Konto. Konfigurieren von SendGrid durch Hinzufügen von Code ähnlich dem folgenden in *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Sie müssen beim Hinzufügen der Folgendes enthält:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

In diesem Beispiel der Einfachheit halber speichern wir die app-Einstellungen in der *"Web.config"* Datei:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern. Das Konto und die Anmeldeinformationen werden in der AppSetting gespeichert. In Azure können sicher gespeichert werden diese Werte auf die **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Registerkarte im Azure-Portal. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Aktivieren von e-Mail-Bestätigung im Kontocontroller

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Überprüfen Sie die *Views\Account\ConfirmEmail.cshtml* Datei hat die richtigen Razor-Syntax. (Der @-Zeichen in der ersten Zeile möglicherweise nicht vorhanden. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Führen Sie die app aus, und klicken Sie auf den Link "Register". Nachdem Sie das Registrierungsformular senden, sind Sie angemeldet.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Überprüfen Sie Ihre e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>E-Mail-Bestätigung vor der Anmeldung verlangen

Derzeit verwendet werden, wenn ein Benutzer das Registrierungsformular abgeschlossen ist, sind sie angemeldet. Im Allgemeinen möchten Sie ihre e-Mail-Adresse zu bestätigen, bevor Sie diese Anmeldung. Im folgenden Abschnitt ändern wir den Code, um neue Benutzer, um eine bestätigte e-Mail-Adresse haben, bevor sie angemeldet sind (authentifiziert) erforderlich. Update der `HttpPost Register` -Methode mit dem folgenden hervorgehobenen Änderungen vor:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Durch Auskommentieren der `SignInAsync` -Methode, der Benutzer wird nicht durch die Registrierung angemeldet werden. Die `TempData["ViewBagLink"] = callbackUrl;` Zeile kann nicht verwendet werden [Debuggen Sie die Anwendung](#dbg) und Testen Sie die Registrierung ohne e-Mail zu senden. `ViewBag.Message` wird verwendet, die Confirm-Anweisungen angezeigt. Die [Beispiel herunterladen](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) enthält Code, um e-Mail-Bestätigung zu testen, ohne das Einrichten von e-Mail-Adresse und kann auch zum Debuggen der Anwendung verwendet werden.

Erstellen Sie eine `Views\Shared\Info.cshtml` Datei, und fügen Sie das folgende Razor-Markup hinzu:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Hinzufügen der [Authorize-Attribut](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) auf die `Contact` Aktionsmethode des Home-Controllers. Klicken Sie auf die **wenden Sie sich an** Link, um zu überprüfen, ob anonyme Benutzer keinen Zugriff haben und authentifizierte Benutzer Zugriff haben.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Sie müssen auch aktualisieren die `HttpPost Login` Aktionsmethode:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Update der *Views\Shared\Error.cshtml* können Sie die Fehlermeldung anzeigen:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Löschen von Konten in der **"aspnetusers"** Tabelle, die den e-Mail-Alias enthalten, Sie testen möchten. Führen Sie die app aus, und stellen Sie sicher, dass Sie können sich nicht anmelden, bis Sie Ihre e-Mail-Adresse bestätigt haben. Nachdem Sie Ihre e-Mail-Adresse bestätigt haben, klicken Sie auf die **wenden Sie sich an** Link.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Zurücksetzen des Kennworts/Wiederherstellung

Entfernen Sie die Kommentarzeichen aus der `HttpPost ForgotPassword` Aktionsmethode im Kontocontroller-:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Entfernen Sie die Kommentarzeichen aus der `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in die *Views\Account\Login.cshtml* Razor-Ansichtsdatei:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Die Anmeldeseite geöffnet wird nun einen Link zum Zurücksetzen des Kennworts angezeigt.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Bestätigungslink erneut-e-Mail

Sobald ein Benutzer ein neues lokales Konto erstellt wird, werden sie einen Link zur Bestätigung per e-Mail gesendet, die, den Sie für erforderlich sind, verwenden, bevor sie sich anmelden können. Wenn der Benutzer versehentlich die Bestätigungs-e-Mail löscht oder die e-Mail nicht ankommt, benötigen sie den Link zur Bestätigung erneut gesendet. Die folgenden codeänderungen zeigen, wie dies zu ermöglichen.

Fügen Sie die folgende Hilfsmethode zum unteren Rand der *controllers\accountcontroller* Datei:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aktualisieren Sie die Register-Methode, um die neue Hilfe verwenden:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aktualisieren der Login-Methode, um das Kennwort erneut zu senden, wenn das Benutzerkonto nicht bestätigt wurde:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung

Sie können lokale als auch social kombinieren, durch Klicken auf Ihre e-Mail-Link. In der folgenden Reihenfolge **RickAndMSFT@gmail.com** wird zuerst als einen lokalen Benutzernamen erstellt, aber Sie können das Konto als eine soziale Anmeldung erste erstellen und dann fügen Sie eine lokale Anmeldung hinzu.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die **externer Anmeldungen: 0** mit diesem Konto verknüpft.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Klicken Sie auf den Link zu einem anderen Protokoll im Dienst, und akzeptieren Sie die app-Anforderungen. Wurden die beiden Konten zusammengefasst, die mit Konten anmelden können. Sie sollten Ihre Benutzer lokale Konten hinzufügen, falls der sozialen Anmeldung Authentication-Dienst nicht unterbrochen ist oder wahrscheinlich haben sie Zugriff auf ihre Konten sozialer Netzwerke verloren.

In der folgenden Abbildung, Tom ist eine soziale Anmeldung (den Sie können die **externer Anmeldungen: 1** auf der Seite angezeigt).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Durch Klicken auf **wählen Sie ein Kennwort** ermöglicht es Ihnen, ein lokales Protokoll hinzufügen, mit dem gleichen Konto verknüpft ist.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>E-Mail-Bestätigung ausführlicher

Meine Tutorial [Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wechselt in diesem Thema mit weiteren Details.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debuggen der app

Wenn Sie eine e-Mail mit der Link nicht erhalten:

- Überprüfen Sie Ihren Junk-e- bzw. Spam-Ordner.
- Melden Sie sich bei Ihrem SendGrid-Konto, und klicken Sie auf die [-e-Mail-Aktivitätslink](https://sendgrid.com/logs/index).

Um den Link für die Überprüfung ohne e-Mail-Konto testen möchten, laden die [abgeschlossene Beispiel](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Der Link zur Bestätigung und Bestätigungscodes werden auf der Seite angezeigt.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Links zu ASP.NET Identity – empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wird ausführlicher auf die Wiederherstellungs- und Konto die Bestätigung des Kennworts.
- [MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-app mit Facebook und Google OAuth 2-Autorisierung zu schreiben. Außerdem wird veranschaulicht, mit der Identity-Datenbank zusätzliche Daten hinzuzufügen.
- [Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In diesem Tutorial wird Azure-Bereitstellung hinzugefügt. So sichern Sie Ihre app mit Rollen, wie Sie die Mitgliedschafts-API zu verwenden, um zusätzliche Sicherheitsfeatures, Benutzer und Rollen hinzuzufügen.
- [Beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Erstellen die app in Facebook, und verbinden die app auf das Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Einrichten von SSL im Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
