---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "Konto zur Bestätigung und Kennwortwiederherstellung mit ASP.NET Identity (c#) | Microsoft Docs"
author: HaoK
description: "Erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen, vor dem Durchführen des Lernprogramms zuerst abgeschlossen werden soll. In diesem Lernprogramm..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 548baaaa06980fb793c079b66b6edc34422eb579
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Kontobestätigung und Kennwortwiederherstellung mit ASP.NET Identity (c#)
====================
durch [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Vor dem Ausführen dieses Lernprogramms führen Sie zuerst [erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Dieses Lernprogramm enthält weitere Informationen und erfahren Sie, wie e-Mail zur kontobestätigung der lokalen einrichten und ermöglichen Benutzern die vergessene Kennwort in ASP.NET Identity zurückgesetzt. In diesem Artikel von Rick Anderson geschrieben wurde ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung und Suhas Joshi. Das NuGet-Beispiel wurde in erster Linie durch Hao Kung geschrieben.


Ein lokales Benutzerkonto verweisen, muss der Benutzer ein Kennwort für das Konto zu erstellen und dieses Kennwort wird in der Web-app (sichere) gespeichert. Außerdem unterstützt ASP.NET Identity soziale Konten den Benutzer ein Kennwort für die app zu erstellen, die nicht erforderlich ist. [Soziale Konten](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) einen Drittanbieter (z. B. Google, Twitter, Facebook oder Microsoft) zum Authentifizieren von Benutzern verwenden. In diesem Thema werden die folgenden Themen behandelt:

- [Erstellen einer ASP.NET MVC-app](#createMvc) und Untersuchen von ASP.NET Identity-Funktionen.
- [Erstellen des Beispiels Identität](#build)
- [Richten Sie e-Mail-Bestätigung](#email)

Neue Benutzer registrieren ihre e-Mail-Alias, der ein lokales Konto erstellt.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Klicken auf die Schaltfläche "registrieren" sendet eine e-Mail zur kaufbestätigung, die eine Überprüfungstoken an ihre e-Mail-Adresse enthält.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Der Benutzer wird eine e-Mail mit einem bestätigungstoken für ihr Konto gesendet werden.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Durch Klicken auf den Link klicken, wird das Konto bestätigt.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Zurücksetzen des Kennworts/Wiederherstellung

Lokale Benutzer ihr Kennwort vergessen haben ein Sicherheitstoken an ihre e-Mail-Konto gesendet featuresammlung um sein Kennwort zurückzusetzen.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Der Benutzer erhält eine e-Mail mit einem Link, sodass sie ihr Kennwort zurücksetzen bald.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Der Link gelangen sie zur Seite "Zurücksetzen".  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Klicken auf die **zurücksetzen** Schaltfläche wird bestätigt das Kennwort zurückgesetzt wurde.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET Web-app

Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren von Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.

> [!NOTE]
> Warnung: Sie müssen Visual Studio installieren [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) dieses Lernprogramms.


1. Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage. WebForms unterstützt auch ASP.NET Identity, damit Sie ähnliche Schritte in einer Web Forms-app folgen können.
2. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**.
3. Führen Sie die app, klicken Sie auf die **registrieren** verknüpfen und einen Benutzer registrieren. An diesem Punkt wird die ausschließliche Überprüfung auf die e-Mail-Adresse mit der [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) Attribut.
4. Wechseln Sie in Server-Explorer zu **Daten Connections\DefaultConnection\Tables\AspNetUsers**, mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**.

    Die folgende Abbildung zeigt die `AspNetUsers` Schema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Klicken Sie mit der rechten Maustaste auf die **AspNetUsers** Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 Die e-Mail wurde an diesem Punkt nicht bestätigt.

Sie können ihn zur Verwendung der andere Datenspeicher und zusätzliche Felder hinzufügen, Konfigurieren der Standardspeicher für die Daten für ASP.NET Identity ist Entity Framework. Finden Sie unter [zusätzliche Ressourcen](#addRes) Abschnitt am Ende dieses Lernprogramms.

Die [OWIN-Startklasse](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) wird aufgerufen, wenn die Anwendung gestartet und ruft die `ConfigureAuth` Methode in *App\_Start\Startup.Auth.cs*, die OWIN-Pipeline konfiguriert und initialisiert ASP.NET Identity. Überprüfen Sie die `ConfigureAuth` Methode. Jede `CreatePerOwinContext` Aufruf registriert einen Rückruf (gespeichert der `OwinContext`), wird pro Anforderung zum Erstellen einer Instanz des angegebenen Typs einmal aufgerufen werden. Sie können einen Haltepunkt im Konstruktor festgelegt und `Create` Methode jeden Typs (`ApplicationDbContext, ApplicationUserManager`) und überprüfen Sie, ob sie bei jeder Anforderung aufgerufen werden. Eine Instanz der `ApplicationDbContext` und `ApplicationUserManager` befindet sich in der OWIN-Kontext, die in der gesamten Anwendung zugegriffen werden kann. ASP.NET Identity hooks in die OWIN-Pipeline über Cookie-Middleware bewirken. Weitere Informationen finden Sie unter [pro Anforderung Prozesslebensdauer-Verwaltung für UserManager-Klasse in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Wenn Sie Ihrem Sicherheitsprofil ändern, ein neuen sicherheitsstempel generiert und gespeichert, der `SecurityStamp` Feld der *AspNetUsers* Tabelle. Beachten Sie, dass die `SecurityStamp` Feld unterscheidet sich das Sicherheitscookie. Das Sicherheitscookie befindet sich nicht in der `AspNetUsers` Tabelle (oder an anderer Stelle in der Identity-Datenbank). Das Cookie Sicherheitstoken ist selbstsigniert mit [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) und wird erstellt, mit der `UserId, SecurityStamp` und Zeitinformationen Ablauf.

Die Cookie-Middleware überprüft das Cookie bei jeder Anforderung. Die `SecurityStampValidator` Methode in der `Startup` Klasse trifft die-Datenbank und in regelmäßigen Abständen, überprüft der sicherheitsstempel wie angegeben mit der `validateInterval`. Dies geschieht nur alle 30 Minuten (in unserem Beispiel), es sei denn, Sie Ihrem Sicherheitsprofil ändern. Das Intervall von 30 Minuten wurde gewählt, um Roundtrips zur Datenbank zu minimieren. Finden Sie unter meinem [zweistufige Authentifizierung Lernprogramm](index.md) Weitere Details.

Pro die Kommentare im Code die `UseCookieAuthentication` Methode unterstützt die Cookieauthentifizierung. Die `SecurityStamp` Feld und zugehörigen Code bietet eine zusätzliche Ebene der Sicherheit Ihrer App, wenn Sie Ihr Kennwort ändern Sie protokolliert aus dem Browser, die Sie sich angemeldet haben. Die `SecurityStampValidator.OnValidateIdentity` Methode ermöglicht die app das Token überprüft werden soll, wenn der Benutzer anmeldet, die verwendet wird, wenn Sie ein Kennwort ändern, oder verwenden die externe Anmeldung. Dies ist erforderlich, um sicherzustellen, dass alle Token (Cookies) mit dem alten Kennwort generiert ungültig werden. Im Beispielprojekt, wenn Sie Benutzerkennworts klicken Sie dann ein neues Token für den Benutzer generiert wird, werden alle vorherigen Token für ungültig erklärt und `SecurityStamp` Feld aktualisiert wird.

Das Identitätssystem können Sie zum Konfigurieren Ihrer Anwendung, wenn das Sicherheitsprofil Benutzer ändert (z. B. wenn der Benutzer das Kennwort oder die Änderungen ändert Anmeldung zugeordnete (z.B. vom Facebook, Google, Microsoft-Konto usw.), der Benutzer wird protokolliert, allen Browserinstanzen. Z. B. die folgende Abbildung zeigt die [einmaliges Abmelden Beispiel](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) app, die dem Benutzer ermöglicht, alle Browserinstanzen (in diesem Fall Internet Explorer, Firefox und Chrome) abmelden, indem Sie auf eine Schaltfläche. Alternativ können auf im Beispiel eine bestimmte Browserinstanz nur abmelden.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

Die [einmaliges Abmelden Beispiel](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) -app veranschaulicht, wie Sie beim Neugenerieren des Sicherheitstokens von ASP.NET Identity können. Dies ist erforderlich, um sicherzustellen, dass alle Token (Cookies) mit dem alten Kennwort generiert ungültig werden. Diese Funktion bietet eine zusätzliche Sicherheitsebene für Ihre Anwendung. Wenn Sie Ihr Kennwort ändern, werden Sie abgemeldet werden, in dem Sie sich an der Anwendung angemeldet haben.

Die *App\_Start\IdentityConfig.cs* -Datei enthält die `ApplicationUserManager`, `EmailService` und `SmsService` Klassen. Die `EmailService` und `SmsService` Klassen implementieren jeweils die `IIdentityMessageService` Schnittstelle, damit Sie häufig verwendeter Methoden in jeder Klasse zum Konfigurieren von e-Mail und SMS verfügen. Obwohl dieses Lernprogramm zeigt nur zum Hinzufügen von e-Mail-Benachrichtigung über [SendGrid](http://sendgrid.com/), können Sie e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden.

Die `Startup` Klasse enthält auch eine Textvorlage zum Hinzufügen von soziale Anmeldungen (Facebook, Twitter, usw.), finden Sie unter meinem Lernprogramm [MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) für Weitere Informationen.

Überprüfen Sie die `ApplicationUserManager` Klasse, die die Identitätsinformationen für den Benutzer enthält, und konfiguriert die folgenden Funktionen:

- Für das Domänenkennwort Stärke.
- Benutzer-gesperrt war (Versuche und Uhrzeit).
- Zweistufige Authentifizierung (2FA). Ich werde 2FA und SMS in ein weiteres Lernprogramm behandelt.
- Einbinden von e-Mails und SMS-Dienste. (Ich werde SMS in ein weiteres Lernprogramm behandelt).

Die `ApplicationUserManager` Klasse abgeleitet wird, von der generischen `UserManager<ApplicationUser>` Klasse. `ApplicationUser`leitet sich von [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser`leitet sich von der generischen `IdentityUser` Klasse:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Die oben genannten Eigenschaften zur berichtsausführung die Verarbeitung mit den Eigenschaften in der `AspNetUsers` obigen Tabelle.

Generische Argumente auf `IUser` ermöglichen es Ihnen, eine Klasse, die Verwendung verschiedener Typen für den Primärschlüssel abgeleitet werden. Finden Sie unter der [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) Beispiel, in dem veranschaulicht, wie den Primärschlüssel von String in Int "oder" GUID zu ändern.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) ist definiert *Models\IdentityModels.cs* als:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Der hervorgehobene Code oben generiert eine ["ClaimsIdentity"](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity und OWIN Cookieauthentifizierung sind anspruchsbasierte, daher das Framework benötigt die app zum Generieren einer `ClaimsIdentity` für den Benutzer. `ClaimsIdentity`enthält Informationen über alle Ansprüche für den Benutzer, z. B. den Namen des Benutzers, Alter und welche Rollen der Benutzer angehört. Sie können auch weitere Ansprüche für den Benutzer zu diesem Zeitpunkt hinzufügen.

Die OWIN `AuthenticationManager.SignIn` Methode übergibt der `ClaimsIdentity` und meldet sich der Benutzer:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) zeigt, wie Sie zusätzliche Eigenschaften hinzufügen können die `ApplicationUser` Klasse.

## <a name="email-confirmation"></a>E-Mail-Bestätigung

Es ist eine gute Idee, bestätigen die e-Mail-Adresse, ein neuer Benutzer mit registrieren, um zu überprüfen, sind sie nicht eine andere Person Identitätswechsel (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse). Angenommen, Sie ein Diskussionsforum hatten, Sie möchten verhindern `"bob@example.com"` aus der Registrierung als `"joe@contoso.com"`. Ohne e-Mail-Bestätigung `"joe@contoso.com"` konnten unerwünschte e-Mails von Ihrer app abgerufen werden. Angenommen, Bob versehentlich als registriert `"bib@example.com"` und hadn't bemerkt, er wäre Kennwort wiederherstellen zu verwenden, da die app hat seine richtige e-Mail-Nachricht nicht. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und stellt keinen Schutz vor bestimmt Spammern, sie haben viele arbeiten e-Mail-Aliase, die sie verwenden können, um zu registrieren. Im folgenden Beispiel wird der Benutzer kann nicht für das Ändern seines Kennworts, bis ihr Konto bestätigt wurde (indem sie durch Klicken auf einen Link zur Bestätigung auf das e-Mail-Konto diese registriert empfangen.) Sie können dieser Ablauf auf andere Szenarien anwenden, z. B. senden einen Link, um zu bestätigen und Zurücksetzen des Kennworts auf neue Konten erstellt, durch den Administrator, die dem Benutzer eine e-Mail senden, wenn sie ihr Profil und so weiter geändert wurden. In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie per e-Mail, eine SMS oder einen anderen Mechanismus bestätigt wurden. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Erstellen ein vollständiges Beispiel

In diesem Abschnitt verwenden Sie NuGet Ein ausführlicheres Beispiel herunterzuladen, dem wir mit verwendet werden kann.

1. Erstellen Sie ein neues ***leere*** ASP.NET-Webprojekt.
2. Geben Sie den folgenden, in der Paket-Manager-Konsole die folgenden Befehle: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 In diesem Lernprogramm verwenden wir [SendGrid](http://sendgrid.com/) zum Senden von e-Mail. Die `Identity.Samples` Paket wird installiert, den wir arbeiten mit Code.
3. Legen Sie die [Projekt zur Verwendung von SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Testen Sie lokales Konto erstellen, durch Ausführen der app, die durch Klicken auf die **registrieren** verknüpfen, und veröffentlichen Sie das Registrierungsformular.
5. Klicken Sie auf die Demo-e-Mail, die e-Mail-Bestätigung simuliert.
6. Entfernen Sie die e-Mail-Link zur Bestätigung Democode aus dem Beispiel (die `ViewBag.Link` Code in die Konto-Controller. Finden Sie unter der `DisplayEmail` und `ForgotPasswordConfirmation` Aktionsmethoden und Razor-Ansichten).

> [!NOTE]
> Warnung: Wenn Sie die Sicherheitseinstellungen in diesem Beispiel ändern, Produktionen apps müssen einer sicherheitsüberprüfung unterzogen werden, die die vorgenommenen Änderungen explizit aufruft.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Überprüfen Sie den Code in der App\_Start\IdentityConfig.cs

Im Beispiel wird gezeigt, wie zum Erstellen eines Kontos ein, und fügen Sie diese der *Admin* Rolle. Ersetzen Sie die e-Mail im Beispiel durch die e-Mail-Adresse, die Verwendung für das Administratorkonto ein. Die einfachste Möglichkeit jetzt, erstellen Sie ein Administratorkonto wird programmgesteuert in die `Seed` Methode. Wir hoffen, ein Tool in der Zukunft haben, mit denen Sie zum Erstellen und Verwalten von Benutzern und Rollen. Der Beispielcode ist möglich, Sie erstellen und Verwalten von Benutzern und Rollen, jedoch benötigen Sie zuerst ein Administratorkonto zum Ausführen von Rollen und Benutzer-Admin-Seiten. In diesem Beispiel wird das Administratorkonto ein erstellt, wenn die Datenbank mit Anfangsdaten gefüllt ist.

Ändern Sie das Kennwort ein, und ändern Sie den Namen in einem Konto an, in denen Sie e-Mail-Benachrichtigungen empfangen kann.

> [!WARNING]
> Sicherheit – sensible Daten nie im Quellcode speichern.

Wie bereits erwähnt, die `app.CreatePerOwinContext` Aufruf in die Startklasse fügt Rückrufe, die `Create` Methode der app DB Inhalten, Benutzer-Manager und die Rolle Manager Klassen. Die OWIN-pipeline Aufrufe der `Create` Methode für diese Klassen für jede Anforderung und speichert den Kontext für jede Klasse. Die Konto-Controller verfügbar macht, die Benutzer-Manager aus dem HTTP-Kontext (enthält die OWIN-Kontext):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Wenn ein Benutzer ein lokales Konto registriert die `HTTP Post Register` -Methode aufgerufen wird:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Der obige Code verwendet die Modelldaten zum Erstellen eines neuen Benutzerkontos mit der e-Mail- und das Kennwort eingegeben. Wenn der e-Mail-Alias im Datenspeicher, kontoerstellung fehlschlägt, und das Formular wird erneut angezeigt. Die `GenerateEmailConfirmationTokenAsync` Methode erstellt einen sicheren bestätigungstoken und speichert sie in den ASP.NET Identity-Datenspeicher. Die [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) Methode erstellt eine Verknüpfung mit der `UserId` und bestätigungstoken. Dieser Link wird dann an den Benutzer per e-Mail, die der Benutzer kann dann auf den Link in der e-Mail-Anwendung auf das Konto zu bestätigen.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Richten Sie e-Mail-Bestätigung

Wechseln Sie zu der [Azure SendGrid-Anmeldeseite](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) und für den free-Konto registrieren. Fügen Sie Code ähnlich dem folgenden SendGrid konfigurieren:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> E-Mail-Clients akzeptieren häufig nur Textnachrichten (HTML). Geben Sie die Nachricht im Text und HTML. Im obigen SendGrid-Beispiel ist dies mit der `myMessage.Text` und `myMessage.Html` oben gezeigten Code.


Der folgende Code zeigt, wie zum Senden von e-Mail mithilfe der [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) -Klasse Where `message.Body` gibt nur die Verbindung.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Sicherheit – sensible Daten nie im Quellcode speichern. Das Konto und die Anmeldeinformationen werden in der AppSetting gespeichert. Sie können auf Azure sicher diese Werte speichern, auf die  **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  Registerkarte im Azure-Portal. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Geben Sie Ihre SendGrid-Anmeldeinformationen, führen Sie die app, beim e-Mail-Alias registriert wird, kann Klicken Sie auf den Link "bestätigen" in Ihre e-Mail-Adresse. Sehen Sie hierzu mit der [Outlook.com](http://outlook.com) e-Mail-Konto, finden Sie unter der John Atten [C#-SMTP-Konfiguration für Outlook.Com SMTP-Host](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) und seine[ASP.NET Identity 2.0: Festlegen der Kontovalidierung und einer zweistufigen Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) sendet.

Sobald ein Benutzer klickt auf die **registrieren** Schaltfläche wird eine e-Mail zur kaufbestätigung, enthält eine Überprüfungstoken an ihre e-Mail-Adresse gesendet.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Der Benutzer wird eine e-Mail mit einem bestätigungstoken für ihr Konto gesendet werden.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Überprüfen Sie den code

Der folgende Code veranschaulicht die `POST ForgotPassword`-Methode.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Die Methode fehlschlägt im Hintergrund auf, wenn die e-Mail-Adresse des Benutzers nicht bestätigt wurde. Wenn ein Fehler für eine ungültige e-Mail-Adresse angezeigt wurde, konnte böswillige Benutzer diese Informationen verwenden, gültige Benutzer-ID (e-Mail-Alias) für Angriffe gefunden.

Der folgende code zeigt die `ConfirmEmail` Methode in den Kontocontroller, die aufgerufen wird, klickt der Benutzer den Bestätigungslink in der e-Mail an Sie gesendet:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Sobald ein vergessenes Kennwort-Token verwendet wurde, wird er ungültig. Die folgende Änderung des Codes in der `Create` -Methode (in der *App\_Start\IdentityConfig.cs* Datei) legt die Token läuft in drei Stunden ab.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Mit dem obigen Code läuft das Kennwort vergessen haben und die e-Mail-bestätigungstoken in 3 Stunden. Die Standardeinstellung `TokenLifespan` ist ein Tag.

Der folgende Code zeigt die e-Mail-Bestätigung-Methode:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Um Ihre app sicherer machen, unterstützt die ASP.NET Identity zweistufige Authentifizierung (2FA). Finden Sie unter [ASP.NET-Identität 2.0: Einrichten der Überprüfung des Kontos und zweistufige Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) von John Atten. Obwohl Sie die kontosperrung auf Anmeldung Versuch Kennwortfehler festlegen können, wird dieser Ansatz Ihrer Anmeldename anfällig für [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) sperren. Es wird empfohlen, dass Sie nur mit 2FA kontosperrung verwenden.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) zeigt außerdem das Hinzufügen von Profilinformationen zur Users-Tabelle.
- [ASP.NET MVC und Identität 2.0: Kenntnisse der Grundlagen](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) von John Atten.
- [Einführung zu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ankündigung der RTM-Version von ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) von Pranav Rastogi.
