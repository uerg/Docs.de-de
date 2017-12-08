---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: ASP.NET MVC 5-Anwendung mit SMS und e-Mail-zweistufige Authentifizierung | Microsoft Docs
author: Rick-Anderson
description: "In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Web-app mit einer zweistufigen Authentifizierung erstellen. Führen Sie erstellen einen sicheren ASP.NET MVC 5-Web-app mit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>ASP.NET MVC 5-Anwendung mit SMS und e-Mail-zweistufige Authentifizierung
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Web-app mit einer zweistufigen Authentifizierung erstellen. Führen Sie [erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) bevor Sie fortfahren. Sie können herunterladen die fertigen Anwendung [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Der Download enthält die debugging-Hilfen, mit denen Sie die e-Mail-Bestätigung und SMS zu testen, ohne eine e-Mail oder SMS-Anbieter einrichten zu müssen.
> 
> Dieses Lernprogramm wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Erstellen einer ASP.NET MVC-app](#createMvc)
- [Einrichten von SMS für die zweistufige Authentifizierung](#SMS)
- [Zweistufige Authentifizierung aktivieren](#enable2)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Erstellen einer ASP.NET MVC-app

Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: Führen Sie [erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) bevor Sie fortfahren. Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage. WebForms unterstützt auch ASP.NET Identity, damit Sie ähnliche Schritte in einer Web Forms-app folgen können.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert. Später in diesem Lernprogramm wird es in Azure bereitstellen. Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
3. Legen Sie die [Projekt zur Verwendung von SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Einrichten von SMS für die zweistufige Authentifizierung

Dieses Lernprogramm enthält Anweisungen für die Verwendung von Twilio oder ASPSMS, aber Sie können andere SMS-Anbieter verwenden.

1. **Erstellen ein Benutzerkonto mit einem SMS-Anbieter**  
  
 Erstellen einer [Twilio](https://www.twilio.com/try-twilio) oder ein [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) Konto.
2. **Installieren zusätzlicher Pakete oder Hinzufügen von Dienstverweisen**  
  
 Twilio:  
 Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:  
    `Install-Package Twilio`  
  
 ASPSMS:  
 Die folgenden Dienstverweis muss hinzugefügt werden:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 Adresse:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Namespace:  
    `ASPSMSX2`
3. **Eng zusammenliegen, SMS-Anbieter-Benutzeranmeldeinformationen**  
  
 Twilio:  
 Aus der **Dashboard** Registerkarte Ihrem Twilio-Konto Kopie der **Konto-SID** und **autorisierungstokens**.  
  
 ASPSMS:  
 Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrer selbst definierten **Kennwort**.  
  
 Speichern wir diese Werte in einem späteren Zeitpunkt die *"Web.config"* -Datei in den Schlüsseln `"SMSAccountIdentification"` und `"SMSAccountPassword"` .
4. **Angeben von "SenderID" / Absender**  
  
 Twilio:  
 Aus der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio-Telefonnummer.  
  
 ASPSMS:  
 Innerhalb der **entsperren Urheber** Menü, entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).  
  
 Wir werden später speichern den Wert in der *"Web.config"* Datei innerhalb des Schlüssels `"SMSAccountFrom"` .
5. **SMS-Anbieter-Anmeldeinformationen in der app übertragen**  
  
 Stellen Sie die Anmeldeinformationen und die Telefonnummer des Absenders der app zur Verfügung. Um die Dinge einfach zu halten speichern wir diese Werte in der *"Web.config"* Datei. Wenn wir in Azure bereitstellen, speichern wir die Werte, die sicher in die **Anwendungseinstellungen** Abschnitt auf der Website, Registerkarte "konfigurieren". 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sicherheit – sensible Daten nie im Quellcode speichern. Das Konto und die Anmeldeinformationen werden der Code oben, um das Beispiel einfach zu halten hinzugefügt. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementierung der Datenübertragung an SMS-Anbieter**  
  
 Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* Datei.  
  
 Je nach verwendeter SMS-Anbieter aktivieren Sie entweder die **Twilio** oder **ASPSMS** Abschnitt: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Update der *Views\Manage\Index.cshtml* Razor-Ansicht: (Hinweis: nicht nur die Kommentare im vorhandenen Code zu entfernen, verwenden Sie den folgenden Code.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Überprüfen Sie die `EnableTwoFactorAuthentication` und `DisableTwoFactorAuthentication` Aktionsmethoden in der `ManageController` haben die[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) Attribut:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Führen Sie die app, und melden Sie sich mit dem Konto, das Sie zuvor registriert.
10. Klicken Sie auf Ihre Benutzer-ID, das aktiviert wird, die `Index` Aktionsmethode in `Manage` Controller.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Klicken Sie auf Hinzufügen.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld, um eine zehnstellige Nummer eingeben, die SMS-Nachrichten empfangen können.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. In wenigen Sekunden erhalten Sie eine Textnachricht mit den Überprüfungscode. Geben Sie ihn, und drücken Sie die **Absenden**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Die Ansicht verwalten zeigt, dass Ihre Telefonnummer hinzugefügt wurde.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Zweistufige Authentifizierung aktivieren

In der app generierte Vorlage müssen Sie die Benutzeroberfläche verwenden, um zweistufige Authentifizierung (2FA) zu aktivieren. Klicken Sie auf Ihre Benutzer-ID (e-Mail-Alias) in der Navigationsleiste, um 2FA zu aktivieren.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Klicken Sie auf 2FA aktivieren.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Abmelden, klicken Sie dann-melden Sie sich wieder im. Wenn Sie e-Mail aktiviert haben (finden Sie unter meinem [vorherigen Lernprogramm](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail-Nachrichten für 2FA auswählen.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Vergewissern Sie sich Codepage wird angezeigt, in dem Sie den Code (von SMS oder e-Mail) eingeben können.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA verwenden, um sich anzumelden, wenn mit dem Browser und dem Gerät, in denen Sie das Feld überprüft. Als böswillige Benutzer auf Ihr Gerät zugreifen können, 2FA aktivieren, und klicken Sie auf die **Denken Sie daran Browser** wird bieten Ihnen einfachen einschrittigen Kennwortzugriff und gleichzeitig die starke 2FA Schutz für alle Zugriffsvorgänge beibehalten von nicht vertrauenswürdigen Geräten. Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden.

Dieses Lernprogramm enthält eine kurze Einführung in die 2FA auf eine neue ASP.NET MVC-app aktivieren. Meine Lernprogramm [zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) ins Detail, auf den Code hinter dem Beispiel wird.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) wechselt in den Details zu einer zweistufigen Authentifizierung
- [Links zu ASP.NET Identity empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Konto zur Bestätigung und Kennwortwiederherstellung mit ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wird ausführlicher auf Wiederherstellung und Konto für die Bestätigung des Kennworts.
- [MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) dieses Lernprogramm veranschaulicht das Schreiben einer ASP.NET MVC 5-Apps mit Facebook und Google OAuth 2 Autorisierung. Es wird gezeigt, wie der Identity-Datenbank zusätzliche Daten hinzugefügt werden.
- [Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf Azure-Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In diesem Lernprogramm fügt Azure-Bereitstellung zum Sichern Ihrer Apps mit Rollen, wie die Mitgliedschafts-API zu verwenden, um Benutzern und Rollen und zusätzliche Sicherheitsfunktionen hinzuzufügen.
- [Erstellen einer Google-app für OAuth 2, und verbinden die app zum Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Erstellen die app in Facebook, und verbinden die app zum Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Einrichten von SSL in das Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
