---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Erstellen Sie eine ASP.NET Web Forms-Anwendung mit SMS zweistufige Authentifizierung (c#) | Microsoft Docs
author: Erikre
description: In diesem Lernprogramm wird gezeigt, wie eine ASP.NET Web Forms-Anwendung mit einer zweistufigen Authentifizierung erstellen. Dieses Lernprogramm wurde entworfen, um das Lernprogramm, mit dem Titel Cr ergänzen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 6c040fd3e0592b8cfd230dcd85ed3293f0a22ba7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Erstellen Sie eine ASP.NET Web Forms-Anwendung mit SMS zweistufige Authentifizierung (c#)
====================
by [Erik Reitan](https://github.com/Erikre)

[Herunterladen von ASP.NET Web Forms-Anwendung mithilfe von E-Mail und SMS zweistufige Authentifizierung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> In diesem Lernprogramm wird gezeigt, wie eine ASP.NET Web Forms-Anwendung mit einer zweistufigen Authentifizierung erstellen. Dieses Lernprogramm wurde entworfen, um das Lernprogramm, mit dem Titel ergänzen [erstellen Sie eine sichere ASP.NET Web Forms-app mit der benutzerregistrierung, eine e-Mail-Bestätigung und das Kennwort zurücksetzen](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Darüber hinaus dieses Lernprogramm basiert auf andersons [MVC-Lernprogramm](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Einführung

Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung, die zweistufige Authentifizierung mit Visual Studio unterstützt. Zweistufige Authentifizierung ist einen Authentifizierungsschritt durch zusätzliche Benutzer. Dieser zusätzliche Schritt generiert eine eindeutige persönliche Identifikationsnummer (PIN) während der Anmeldung. Die PIN wird häufig als eine e-Mail oder SMS-Nachricht an den Benutzer gesendet. Der Benutzer Ihrer App klicken Sie dann die PIN eingibt, wie ein Measure für die zusätzliche Authentifizierung beim Anmelden.

### <a name="tutorial-tasks-and-information"></a>Lernprogramm Aufgaben und Informationen:

- [Erstellen einer ASP.NET Web Forms-App](#createWebForms)
- [Einrichten von SMS und zweistufige Authentifizierung](#SMS)
- [Zweistufige Authentifizierung für den registrierten Benutzer aktivieren](#use2FA)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Erstellen einer ASP.NET Web Forms-App

Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher sowie. Außerdem müssen Sie zum Erstellen einer [Twilio](https://www.twilio.com/try-twilio) zu berücksichtigen, wie nachstehend beschrieben.

> [!NOTE]
> Wichtig: Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen eines neuen Projekts (**Datei**  - &gt; **neues Projekt**), und wählen Sie die **ASP.NET-Webanwendung** Vorlage zusammen mit .NET Framework Version 4.5.2 aus der **neues Projekt** (Dialogfeld).
2. Aus der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web Forms** Vorlage. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Klicken Sie auf **OK** zum Erstellen eines neuen Projekts.  
    ![Neues ASP.NET-Projekt (Dialogfeld)](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Aktivieren Sie Secure Sockets Layer (SSL) für das Projekt ein. Führen Sie die Schritte zur Verfügung, in der **"SSL aktivieren", für das Projekt** Teil der [erste Schritte mit Web Forms Reihe von Lernprogrammen](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Öffnen Sie in Visual Studio die **Package Manager Console** (**Tools**  - &gt; **NuGet-Paket-Manager**  - &gt; **Package Manager Console**), und geben Sie den folgenden Befehl aus:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Einrichten von SMS und zweistufige Authentifizierung

Dieses Lernprogramm verwendet Twilio, jedoch können Sie alle SMS-Anbieter.

1. Erstellen einer [Twilio](https://www.twilio.com/try-twilio) Konto.
2. Aus der **Dashboard** Registerkarte Ihrem Twilio-Konto Kopie der **Konto-SID** und **Auth-Token.** Sie werden sie später zu Ihrer app hinzufügen.
3. Aus der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio **Telefonnummer** ebenfalls.
4. Stellen Sie die Twilio **Konto-SID**, **Autorisierungstokens** und **Telefonnummer** für die app verfügbar. Auf der Einfachheit halber speichern Sie diese Werte in der *"Web.config"* Datei. Wenn Sie in Azure bereitstellen, können Sie die Werte, die sicher im Speichern der **"appSettings"** Abschnitt auf der Website, Registerkarte "konfigurieren". Wenn Sie die Telefonnummer hinzufügen möchten, nur verwenden Sie außerdem Zahlen.   
   Beachten Sie, dass Sie auch die SendGrid-Anmeldeinformationen hinzufügen können. SendGrid ist eine e-Mail-Benachrichtigung-Dienst. Ausführliche Informationen zum Aktivieren von SendGrid, finden Sie im Abschnitt "Hook einrichten SendGrid" im Lernprogramm mit der Bezeichnung [erstellen Sie eine Secure ASP.NET Web Forms-App mit der benutzerregistrierung, eine e-Mail-Bestätigung und das Kennwort zurücksetzen.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sicherheit – sensible Daten nie im Quellcode speichern. In diesem Beispiel wird das Konto und die Anmeldeinformationen werden gespeichert der **"appSettings"** Teil der *"Web.config"* Datei. Sie können auf Azure sicher diese Werte speichern, auf die **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Registerkarte im Azure-Portal. Weitere Informationen finden Sie unter andersons Thema [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* dateiänderungen, indem Sie die folgenden machen Gelb: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Fügen Sie die folgenden `using` Anweisungen am Anfang der *IdentityConfig.cs* Datei: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Update der *Account/Manage.aspx* Datei durch das Entfernen der Zeilen in gelb hervorgehoben:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. In der `Page_Load` Handler, der die *Manage.aspx.cs* Code-Behind, heben Sie die auskommentierung der Codezeile gelb hervorgehoben, damit er angezeigt, wie wird folgt: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Im Codebehind des *Konto*/*TwoFactorAuthenticationSignIn.aspx.cs*, aktualisieren Sie die `Page_Load` Handler durch Hinzufügen des folgenden Codes in gelb hervorgehoben: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Machen Sie den oben aufgeführten Code ändern, wird, enthält die Authentifizierungsoptionen DropDownList "Anbieter" nicht auf den ersten Wert zurückgesetzt werden. Dies ermöglicht den Benutzer, alle Optionen beim authentifizieren, nicht nur des erste zu verwendende erfolgreich auszuwählen.
10. In **Projektmappen-Explorer**, mit der rechten Maustaste *"default.aspx"* , und wählen Sie **als Startseite festlegen**.
11. Durch Ihre Anwendung testen, erstellen Sie zuerst die app (**STRG**+**UMSCHALT**+**B**) und führen Sie die app (**F5**) und Wählen Sie entweder **registrieren** ein neues Benutzerkonto erstellen oder auswählen **melden Sie sich** das Benutzerkonto, das bereits registriert wurde.
12. Sobald Sie (als der Benutzer) angemeldet haben, klicken Sie auf die Benutzer-ID (e-Mail-Adresse) in der Navigationsleiste angezeigt der **Konto verwalten** Seite (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Klicken Sie auf **hinzufügen** neben **Telefonnummer** auf die **Konto verwalten** Seite.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Fügen Sie die Rufnummer an, in dem Sie (als der Benutzer) möchten SMS-Nachrichten (SMS) empfangen, und klicken Sie auf, die **Absenden** Schaltfläche.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    An diesem Punkt die app verwendet die Anmeldeinformationen aus der *"Web.config"* Twilio zu kontaktieren. Telefon mit dem Benutzerkonto wird eine Nachricht SMS (Textnachricht) gesendet. Sie können überprüfen, ob die Twilio-Nachricht gesendet wurde, indem Sie die Twilio-Dashboard anzeigen.
15. In wenigen Sekunden erhalten Telefon mit dem Benutzerkonto eine SMS mit der Überprüfungscode. Geben Sie den Überprüfungscode und drücken Sie **Absenden**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Aktivieren Sie zweistufige Authentifizierung für einen registrierten Benutzer

An diesem Punkt haben Sie zweistufige Authentifizierung für Ihre app aktiviert. Für einen Benutzer zweistufige Authentifizierung zu verwenden können sie einfach ihre Einstellungen, die über die Benutzeroberfläche ändern. 

1. Als Benutzer der app kann zweistufige Authentifizierung für Ihr spezifisches Konto durch Klicken auf die Benutzer-ID (e-Mail-Alias) in der Navigationsleiste angezeigt der **Konto verwalten** Seite. Klicken Sie auf die **aktivieren** Link zu einer zweistufigen Authentifizierung für das Konto zu aktivieren.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Melden Sie sich ab und dann wieder anzumelden. Wenn Sie e-Mail aktiviert haben, können Sie SMS oder e-Mail-Nachrichten für die zweistufige Authentifizierung auswählen. Wenn Sie nicht-e-Mail aktiviert haben, finden Sie im Lernprogramm, das mit dem Titel [erstellen Sie eine Secure ASP.NET Web Forms-App mit Benutzerregistrierung "," e-Mail-Bestätigung "und" Kennwort zurücksetzen](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Seite für die zweistufige Authentifizierung wird angezeigt, in dem Sie den Code (von SMS oder e-Mail) eingeben können.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu zweistufige Authentifizierung zu verwenden, um sich anzumelden, wenn mit dem Browser und dem Gerät, in denen Sie das Feld überprüft. Als böswillige Benutzer auf Ihr Gerät zugreifen können, zweistufige Authentifizierung aktivieren, und klicken Sie auf die **Denken Sie daran Browser** wird bieten Ihnen einfachen einschrittigen Kennwortzugriff und strong gleichzeitig beibehalten zweistufige Authentifizierung den Schutz für alle Zugriff von nicht vertrauenswürdigen Geräten. Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Zweistufige Authentifizierung mithilfe von SMS und E-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Links zu ASP.NET Identity empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Bereitstellen von Mitgliedschaft, OAuth und SQL-Datenbank eine sichere ASP.NET Web Forms-App für eine Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms Reihe von Lernprogrammen - Hinzufügen einer OAuth 2.0-Anbieter](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms Reihe von Lernprogrammen - SSL für das Projekt aktivieren](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Kontobestätigung und Kennwortwiederherstellung mit ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Erstellen die app in Facebook, und verbinden die app zum Projekt](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
