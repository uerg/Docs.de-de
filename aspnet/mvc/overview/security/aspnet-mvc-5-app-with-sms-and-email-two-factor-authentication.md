---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: ASP.NET MVC 5-app mit SMS und e-Mail-Adresse einer zweistufigen Authentifizierung | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Web-app mit einer zweistufigen Authentifizierung zu erstellen. Führen Sie erstellen eine sichere ASP.NET MVC 5-Web-app mit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7987953ae94105be8f4856069ce13b86aec7e7f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393956"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>ASP.NET MVC 5-app mit SMS und e-Mail-zwei-Faktor-Authentifizierung
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Web-app mit einer zweistufigen Authentifizierung zu erstellen. Führen Sie [erstellen eine sichere ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) bevor Sie fortfahren. Sie können die fertige Anwendung herunterladen [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Der Download enthält Debuggen-Hilfsprogramme, mit denen Sie die Bestätigung per e-Mail und SMS zu testen, ohne das Einrichten einer e-Mail oder SMS-Anbieter.
> 
> In diesem Tutorial wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Erstellen einer ASP.NET MVC-app](#createMvc)
- [SMS für die zweistufige Authentifizierung einrichten](#SMS)
- [Zwei-Faktor-Authentifizierung aktivieren](#enable2)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Erstellen einer ASP.NET MVC-app

Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.

> [!NOTE]
> Warnung: Führen Sie [erstellen eine sichere ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) bevor Sie fortfahren. Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage. Web Forms unterstützt auch ASP.NET Identity, sodass Sie ähnliche Schritte in einer Web Forms-app ausführen konnte.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert. Später in diesem Tutorial werden wir in Azure bereitstellen. Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Legen Sie die [Projekt für die Verwendung von SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>SMS für die zweistufige Authentifizierung einrichten

Dieses Tutorial enthält Anweisungen für die mithilfe von Twilio oder ASPSMS, jedoch können Sie alle anderen SMS-Anbieter.

1. **Erstellen ein Benutzerkonto mit einem SMS-Anbieter**  
  
   Erstellen Sie eine [Twilio](https://www.twilio.com/try-twilio) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) Konto.
2. **Installieren zusätzlicher Pakete oder Hinzufügen von Dienstverweisen**  
  
   Twilio:  
   Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Folgender Dienstverweis muss hinzugefügt werden:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresse:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Ermitteln der Anmeldeinformationen des Benutzers für SMS-Anbieter**  
  
   Twilio:  
   Von der **Dashboard** Ihrem Twilio-Konto kopieren auf der Registerkarte die **Konto-SID** und **Authentifizierungstoken**.  
  
   ASPSMS:  
   Navigieren Sie in Ihren kontoeinstellungen zu **Userkey** und kopieren Sie ihn zusammen mit Ihrer selbstdefinierte **Kennwort**.  
  
   Wir diese Werte in einem späteren Zeitpunkt speichert die *"Web.config"* Datei in den Schlüsseln `"SMSAccountIdentification"` und `"SMSAccountPassword"` .
4. **Angeben der Absender-ID / Ersteller**  
  
   Twilio:  
   Von der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio-Telefonnummer.  
  
   ASPSMS:  
   In der **entsperren Urheber** Menü ein oder mehrere Urheber zu entsperren, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).  
  
   Wir diesen Wert in einem späteren Zeitpunkt speichert die *"Web.config"* Datei innerhalb des Schlüssels `"SMSAccountFrom"` .
5. **SMS-Anbieter-Anmeldeinformationen werden in app übertragen.**  
  
   Stellen Sie die Anmeldeinformationen und die Telefonnummer des Absenders an die app zur Verfügung. Einfachheit halber werden wir diese Werte im Speichern der *"Web.config"* Datei. Wenn wir in Azure bereitstellen, können wir die Werte, die sicher im Speichern der **Anwendungseinstellungen** Abschnitt auf der Website, Registerkarte "konfigurieren". 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern. Das Konto und die Anmeldeinformationen werden auf den Code aus, um das Beispiel möglichst einfach zu halten hinzugefügt. Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementierung der Datenübertragung an SMS-Anbieter**  
  
   Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* Datei.  
  
   Aktivieren Sie je nach verwendeter SMS-Anbieter entweder die **Twilio** oder **ASPSMS** Abschnitt: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Update der *Views\Manage\Index.cshtml* Razor-Ansicht: (Hinweis: nicht nur die Kommentare im vorhandenen Code zu entfernen, verwenden Sie den folgenden Code.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Überprüfen Sie die `EnableTwoFactorAuthentication` und `DisableTwoFactorAuthentication` Aktionsmethoden in die `ManageController` haben die[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) Attribut:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Führen Sie die app aus, und melden Sie sich mit dem Konto an, die, das Sie zuvor registriert.
10. Klicken Sie auf Ihre Benutzer-ID, das aktiviert wird, die `Index` Aktionsmethode im `Manage` Controller.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Klicken Sie auf Hinzufügen.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Die `AddPhoneNumber` Action-Methode zeigt ein Dialogfeld, um eine Telefonnummer eingeben, die SMS-Nachrichten empfangen kann.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Innerhalb weniger Sekunden erhalten Sie eine Textnachricht mit dem Überprüfungscode. Geben Sie ihn, und drücken Sie die **senden**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Die Ansicht "verwalten" zeigt, dass Ihre Telefonnummer hinzugefügt wurde.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Zwei-Faktor-Authentifizierung aktivieren

In der app generierte Vorlage müssen Sie die Benutzeroberfläche verwenden, um die zweistufige Authentifizierung (2FA) zu aktivieren. Klicken Sie auf Ihre Benutzer-ID (e-Mail-Alias) in der Navigationsleiste, um die Aktivierung der 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Klicken Sie auf Aktivieren 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Abmelden, klicken Sie dann erneut an. Wenn Sie e-Mail-Adresse aktiviert haben (finden Sie unter meinem [vorherigen Tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail-Adresse für 2FA auswählen.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Die Überprüfen von Code-Seite wird angezeigt, in dem Sie den Code (von SMS oder e-Mail-Adresse) eingeben können.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Durch Klicken auf die **diesen Browser merken** das Kontrollkästchen schließen Sie aus 2FA zu verwenden, melden Sie sich, wenn mit dem Browser und dem Gerät, in dem Sie das Kontrollkästchen aktiviert haben. Solange böswillige Benutzer auf Ihr Gerät zugreifen können, 2FA aktiviert und auf die **diesen Browser merken** bieten Ihnen bequemen Zugriff für einen Schritt Kennwort, diagnosesuchläufen starken 2FA Schutz für den gesamten Zugriff von nicht vertrauenswürdigen Geräten. Sie können auf allen privaten Geräten dazu, die Sie regelmäßig verwenden.

Dieses Tutorial enthält eine kurze Einführung in die Aktivierung der 2FA auf einer neuen ASP.NET MVC-Anwendung. Meine Tutorial [zweistufige Authentifizierung mithilfe von SMS und e-Mail-Adresse mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) mehr Details für den Code hinter dem Beispiel.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Zwei-Faktor-Authentifizierung, die mithilfe von SMS und e-Mail-Adresse mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) mehr Details für die zweistufige Authentifizierung
- [Links zu ASP.NET Identity – empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wird ausführlicher auf die Wiederherstellungs- und Konto die Bestätigung des Kennworts.
- [MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-app mit Facebook und Google OAuth 2-Autorisierung zu schreiben. Außerdem wird veranschaulicht, mit der Identity-Datenbank zusätzliche Daten hinzuzufügen.
- [Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank zu Azure-Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In diesem Tutorial wird Azure-Bereitstellung hinzugefügt. So sichern Sie Ihre app mit Rollen, wie Sie die Mitgliedschafts-API zu verwenden, um zusätzliche Sicherheitsfeatures, Benutzer und Rollen hinzuzufügen.
- [Beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Erstellen die app in Facebook, und verbinden die app auf das Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Einrichten von SSL im Projekt](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
