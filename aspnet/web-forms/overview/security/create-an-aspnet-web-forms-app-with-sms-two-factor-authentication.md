---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Erstellen einer ASP.NET Web Forms-app mit SMS-zwei-Faktor-Authentifizierung (c#) | Microsoft-Dokumentation
author: Erikre
description: In diesem Tutorial erfahren Sie, wie zum Erstellen einer ASP.NET Web Forms-app mit einer zweistufigen Authentifizierung. In diesem Tutorial wurde entwickelt, um das Tutorial mit dem Titel Cr ergänzen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1231ed0cb238ad35c12405dd9a264ccfd7332960
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401291"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Erstellen einer ASP.NET Web Forms-app mit SMS-zwei-Faktor-Authentifizierung (c#)
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen von ASP.NET Web Forms-App mit E-Mail und SMS-zwei-Faktor-Authentifizierung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> In diesem Tutorial erfahren Sie, wie zum Erstellen einer ASP.NET Web Forms-app mit einer zweistufigen Authentifizierung. In diesem Tutorial wurde entworfen, um das Tutorial mit dem Titel ergänzen [erstellen eine sichere ASP.NET Web Forms-app mit benutzerregistrierung, e-Mail-Bestätigung und kennwortzurücksetzung](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). In diesem Tutorial basiert außerdem auf andersons [MVC-Tutorial](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Einführung

Dieses Tutorial führt Sie durch die Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung, die zweistufige Authentifizierung mithilfe von Visual Studio unterstützt. Zweistufige Authentifizierung ist eine zusätzliche Benutzer Authentifizierungsschritt. Dieser zusätzliche Schritt generiert eine eindeutige persönliche Identifikationsnummer (PIN), bei der Anmeldung. Die PIN wird häufig als das Senden einer e-Mail oder SMS-Nachricht an den Benutzer gesendet. Der Benutzer Ihrer App klicken Sie dann die PIN eingibt, als Measure zusätzliche Authentifizierung beim Anmelden.

### <a name="tutorial-tasks-and-information"></a>Tutorial Aufgaben und Informationen:

- [Erstellen einer ASP.NET Web Forms-App](#createWebForms)
- [Einrichten von SMS und zwei-Faktor-Authentifizierung](#SMS)
- [Aktivieren Sie die zweistufige Authentifizierung für registrierte Benutzer](#use2FA)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Erstellen einer ASP.NET Web Forms-App

Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher sowie. Darüber hinaus müssen Sie erstellen eine [Twilio](https://www.twilio.com/try-twilio) zu berücksichtigen, wie nachstehend beschrieben.

> [!NOTE]
> Wichtig: Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen eines neuen Projekts (**Datei**  - &gt; **neues Projekt**), und wählen Sie die **ASP.NET-Webanwendung** Vorlage zusammen mit .NET Framework Version 4.5.2 von der **neues Projekt** Dialogfeld.
2. Von der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web Forms** Vorlage. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Klicken Sie auf **OK** zum Erstellen eines neuen Projekts.  
    ![Neues ASP.NET-Projekt (Dialogfeld)](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Aktivieren Sie Secure Sockets Layer (SSL) für das Projekt ein. Führen Sie die Schritte zur Verfügung, in der **SSL aktivieren, für das Projekt** Teil der [erste Schritte mit Web Forms-tutorialreihe](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Öffnen Sie in Visual Studio die **-Paket-Manager-Konsole** (**Tools**  - &gt; **NuGet-Paket-Manager**  - &gt; **-Paket-Manager-Konsole**), und geben Sie den folgenden Befehl aus:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Einrichten von SMS und zwei-Faktor-Authentifizierung

In diesem Tutorial verwendet Twilio, jedoch können Sie alle SMS-Anbieter.

1. Erstellen Sie eine [Twilio](https://www.twilio.com/try-twilio) Konto.
2. Von der **Dashboard** Ihrem Twilio-Konto kopieren auf der Registerkarte die **Konto-SID** und **Auth-Token.** Sie werden sie später zu Ihrer app hinzufügen.
3. Von der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio **Telefonnummer** ebenfalls.
4. Stellen Sie die Twilio **Konto-SID**, **Authentifizierungstoken** und **Telefonnummer** der app zur Verfügung. Einfachheit halber speichern Sie diese Werte in der *"Web.config"* Datei. Wenn Sie in Azure bereitstellen, können Sie die Werte, die sicher im Speichern der **"appSettings"** Abschnitt auf der Website, Registerkarte "konfigurieren". Auch, wenn Sie die Telefonnummer hinzufügen möchten, nur verwenden Sie, Zahlen.   
   Beachten Sie, dass Sie auch die SendGrid-Anmeldeinformationen hinzufügen können. SendGrid ist ein e-Mail-Benachrichtigung-Dienst. Für ausführliche Informationen zum Aktivieren von SendGrid finden Sie im Abschnitt "Hook Sie SendGrid" im Tutorial mit dem Titel [erstellen Sie eine sichere ASP.NET Web Forms-App mit benutzerregistrierung, e-Mail-Bestätigung und kennwortzurücksetzung.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern. In diesem Beispiel ist das Konto und die Anmeldeinformationen werden gespeichert der **"appSettings"** Teil der *"Web.config"* Datei. In Azure können sicher gespeichert werden diese Werte auf die **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Registerkarte im Azure-Portal. Weitere Informationen finden Sie unter andersons Thema [bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* dateiänderungen dazu die folgenden Gelb: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Fügen Sie die folgenden `using` Anweisungen am Anfang der *IdentityConfig.cs* Datei: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Update der *Account/Manage.aspx* Datei durch das Entfernen von Zeilen in gelb hervorgehoben:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. In der `Page_Load` Handler, der die *Manage.aspx.cs* CodeBehind, heben Sie die auskommentierung die Zeile des Codes in gelb hervorgehoben werden, damit er angezeigt, wie wird folgt: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Im Codebehind von *Konto*/*TwoFactorAuthenticationSignIn.aspx.cs*, aktualisieren Sie die `Page_Load` Ereignishandler, indem Sie den folgenden Code in gelb hervorgehoben hinzufügen: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Machen Sie den obigen Code ändern, wird "Anbieter" DropDownList mit den Authentifizierungsoptionen nicht auf den ersten Wert zurückgesetzt werden. Dadurch wird den Benutzer erfolgreich alle Optionen, die beim authentifizieren, nicht nur des erste zu verwendende auswählen.
10. In **Projektmappen-Explorer**, mit der rechten Maustaste *"default.aspx"* , und wählen Sie **als Startseite festlegen**.
11. Durch Ihre Anwendung testen, erstellen Sie zunächst die app (**STRG**+**UMSCHALT**+**B**), und klicken Sie dann die app ausführen (**F5**) und Wählen Sie entweder **registrieren** ein neues Benutzerkonto erstellen oder auswählen **melden Sie sich bei** , wenn das Benutzerkonto, das bereits registriert wurde.
12. Sobald Sie (als Benutzer) angemeldet haben, klicken Sie auf die Benutzer-ID (e-Mail-Adresse) in der Navigationsleiste angezeigt der **Konto verwalten** Seite (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Klicken Sie auf **hinzufügen** neben **Telefonnummer** auf die **Konto verwalten** Seite.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Fügen Sie die Telefonnummer, an dem möchten Sie (als Benutzer) zum Empfangen von SMS-Nachrichten (SMS), und klicken Sie auf, die **senden** Schaltfläche.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    An diesem Punkt die app verwendet die Anmeldeinformationen aus der *"Web.config"* mit Twilio in Verbindung setzen. Eine SMS-Nachricht (Textnachricht) wird auf dem Telefon, das dem Benutzerkonto zugeordneten gesendet. Sie können überprüfen, ob die Twilio-Nachricht gesendet wurde, indem Sie die Twilio-Dashboard anzeigen.
15. Innerhalb weniger Sekunden erhalten-Telefon mit dem Benutzerkonto eine SMS mit den Überprüfungscode. Geben Sie den Überprüfungscode und drücken Sie **senden**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Aktivieren Sie die zweistufige Authentifizierung für ein registrierter Benutzer

An diesem Punkt haben Sie zwei-Faktor-Authentifizierung für Ihre app aktiviert. Damit ein Benutzer zwei-Faktor-Authentifizierung verwenden können sie einfach ihre Einstellungen, die über die Benutzeroberfläche ändern. 

1. Als Benutzer der app Sie können zwei-Faktor-Authentifizierung für Ihr bestimmtes Konto durch Klicken auf die Benutzer-ID (e-Mail-Alias) in der Navigationsleiste angezeigt der **Konto verwalten** Seite. Klicken Sie auf die **aktivieren** Link zu einer zweistufigen Authentifizierung für das Konto zu aktivieren.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Melden Sie sich ab und dann wieder anmelden. Wenn Sie e-Mail-Adresse aktiviert haben, können Sie entweder SMS oder e-Mail-Adresse für die zweistufige Authentifizierung auswählen. Wenn Sie nicht-e-Mail aktiviert haben, finden Sie im Tutorial, das mit dem Titel [erstellen Sie eine sichere ASP.NET Web Forms-App mit Benutzerregistrierung, e-Mail-Bestätigung und Kennwortzurücksetzung](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Die Two-Factor Authentication-Seite wird angezeigt, in dem Sie den Code (von SMS oder e-Mail-Adresse) eingeben können.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Durch Klicken auf die **diesen Browser merken** das Kontrollkästchen schließen Sie aus einer zweistufigen Authentifizierung zu verwenden, melden Sie sich, wenn mit dem Browser und dem Gerät, in dem Sie das Kontrollkästchen aktiviert haben. Solange böswillige Benutzer Zugriff auf Ihrem Gerät erhalten können, zwei-Faktor-Authentifizierung aktivieren und auf die **diesen Browser merken** erhalten Sie praktische Kennwortzugriff für einen Schritt, gleichzeitig die starke beibehalten zweistufige Authentifizierung den Schutz für den gesamten Zugriff von nicht vertrauenswürdigen Geräten. Sie können auf allen privaten Geräten dazu, die Sie regelmäßig verwenden.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Zweistufige Authentifizierung mithilfe von SMS und E-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Links zu ASP.NET Identity – empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Bereitstellen einer sicheren ASP.NET Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms-tutorialreihe - Hinzufügen eines OAuth 2.0-Anbieters](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms tutorialreihe - SSL für das Projekt aktivieren](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Erstellen die app in Facebook, und verbinden die app auf das Projekt](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
