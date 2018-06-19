---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity | Microsoft Docs
author: HaoK
description: In diesem Lernprogramm erfahren Sie, wie zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail-eingerichtet. In diesem Artikel von Rick Anderson geschrieben wurde ( @RickAndMSFT ), Pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876136"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity
====================
durch [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> In diesem Lernprogramm erfahren Sie, wie zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail-eingerichtet.
> 
> In diesem Artikel von Rick Anderson geschrieben wurde ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung und Suhas Joshi. Das NuGet-Beispiel wurde in erster Linie durch Hao Kung geschrieben.


In diesem Thema werden die folgenden Themen behandelt:

- [Erstellen des Beispiels Identität](#build)
- [Einrichten von SMS für die zweistufige Authentifizierung](#SMS)
- [Zweistufige Authentifizierung aktivieren](#enable2)
- [Vorgehensweise: Registrieren Sie einen Multi-Factor Authentication-Anbieter](#reg)
- [Kombinieren von sozialen und lokalen Anmeldekonten](#combine)
- [Kontosperrung vor Brute-Force-Angriffen](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Erstellen des Beispiels Identität

NuGet verwenden Sie in diesem Abschnitt ein Beispiel herunterladen möchten, die wir mit verwendet werden kann. Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren von Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.

> [!NOTE]
> Warnung: Sie müssen Visual Studio installieren [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) dieses Lernprogramms.


1. Erstellen Sie ein neues ***leere*** ASP.NET-Webprojekt.
2. Geben Sie den folgenden, in der Paket-Manager-Konsole die folgenden Befehle:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   In diesem Lernprogramm verwenden wir [SendGrid](http://sendgrid.com/) das Senden von e-Mails und [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) für Sms-Texte. Die `Identity.Samples` Paket wird installiert, den wir arbeiten mit Code.
3. Legen Sie die [Projekt zur Verwendung von SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Optionale*: befolgen Sie die Anweisungen in meinem [e-Mail-Bestätigung Lernprogramm](account-confirmation-and-password-recovery-with-aspnet-identity.md) SendGrid einbinden und führen Sie die app und registrieren ein e-Mail-Konto.
5. * Optional: * Demo e-Mail-Link-Bestätigungscode aus dem Beispiel entfernen (die `ViewBag.Link` Code in die Konto-Controller. Finden Sie unter der `DisplayEmail` und `ForgotPasswordConfirmation` Aktionsmethoden und Razor-Ansichten).
6. <em>Optional: * Entfernen der `ViewBag.Status` Code aus verwalten und Konto für Controller und die *Views\Account\VerifyCode.cshtml</em> und <em>Views\Manage\VerifyPhoneNumber.cshtml</em> Razor-Ansichten. Alternativ können Sie behalten die `ViewBag.Status` So testen Sie die Funktionsweise dieser app lokal ohne zu verknüpfen und Senden von e-Mail und SMS-Nachrichten anzeigen.

> [!NOTE]
> Warnung: Wenn Sie die Sicherheitseinstellungen in diesem Beispiel ändern, Produktionen apps müssen einer sicherheitsüberprüfung unterzogen werden, die explizit, die Änderungen vorgenommen aufruft.


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
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresse:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Eng zusammenliegen, SMS-Anbieter-Benutzeranmeldeinformationen**  
  
   Twilio:  
   Aus der **Dashboard** Registerkarte Ihrem Twilio-Konto Kopie der **Konto-SID** und **autorisierungstokens**.  
  
   ASPSMS:  
   Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrer selbst definierten **Kennwort**.  
  
   Wir werden diese Werte später gespeichert, in der Variablen `SMSAccountIdentification` und `SMSAccountPassword` .
4. **Angeben von "SenderID" / Absender**  
  
   Twilio:  
   Aus der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio-Telefonnummer.  
  
   ASPSMS:  
   Innerhalb der **entsperren Urheber** Menü, entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).  
  
   Wir werden diesen Wert später gespeichert, in der Variablen `SMSAccountFrom` .
5. **SMS-Anbieter-Anmeldeinformationen in der app übertragen**  
  
   Stellen Sie die Anmeldeinformationen und die Telefonnummer des Absenders an die app zur Verfügung:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Sicherheit – sensible Daten nie im Quellcode speichern. Das Konto und die Anmeldeinformationen werden der Code oben, um das Beispiel einfach zu halten hinzugefügt. Finden Sie unter der Jon Atten [ASP.NET-MVC: Beibehalten von privaten Einstellungen außerhalb des Datenquellen-Steuerelements](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementierung der Datenübertragung an SMS-Anbieter**  
  
   Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* Datei.  
  
   Je nach verwendeter SMS-Anbieter aktivieren Sie entweder die **Twilio** oder **ASPSMS** Abschnitt: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Führen Sie die app, und melden Sie sich mit dem Konto, das Sie zuvor registriert.
8. Klicken Sie auf Ihre Benutzer-ID, das aktiviert wird, die `Index` Aktionsmethode in `Manage` Controller.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Klicken Sie auf Hinzufügen.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. In wenigen Sekunden erhalten Sie eine Textnachricht mit den Überprüfungscode. Geben Sie ihn, und drücken Sie die **Absenden**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Die Ansicht verwalten zeigt, dass Ihre Telefonnummer hinzugefügt wurde.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Überprüfen Sie den code

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Die `Index` Aktionsmethode in `Manage` Controller legt die Statusmeldung basierend auf der vorherigen Aktion und enthält Links, um Ihr lokales Kennwort ändern oder Hinzufügen eines lokalen Kontos. Die `Index` Methode zeigt auch den Zustand oder die 2FA phone Anzahl, externer Anmeldungen, 2FA aktiviert, und zu behalten, 2FA-Methode für diesen Browser (Dies wird weiter unten erläutert). Durch Klicken auf Ihre Benutzer-ID (e-Mail) in der Titelleiste, keine Nachricht übergeben werden. Durch Klicken auf die **Telefonnummer: Entfernen** verknüpfen übergibt `Message=RemovePhoneSuccess` als Abfragezeichenfolge.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld, um eine zehnstellige Nummer eingeben, die SMS-Nachrichten empfangen können.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Durch Klicken auf die **senden Prüfcode** Schaltfläche sendet die Telefonnummer an den HTTP-POST `AddPhoneNumber` Aktionsmethode.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Die `GenerateChangePhoneNumberTokenAsync` Methode generiert das Sicherheitstoken, die in der SMS-Nachricht festgelegt werden. Wenn der SMS-Dienst konfiguriert wurde, wird das Token gesendet, als die Zeichenfolge &quot;Ihr Sicherheitscode lautet &lt;token&gt;&quot;. Die `SmsService.SendAsync` aufzurufende Methode asynchron aufgerufen wird, und klicken Sie dann auf die app umgeleitet wird die `VerifyPhoneNumber` Aktionsmethode (in der das folgende Dialogfeld angezeigt), in dem Sie den Überprüfungscode eingeben können.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Nachdem Sie geben Sie den Code, und klicken Sie auf Absenden, der Code an die HTTP-POST gesendet wird `VerifyPhoneNumber` Aktionsmethode.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Die `ChangePhoneNumberAsync` Methode überprüft die bereitgestellten Sicherheitscode. Wenn der Code korrekt ist, wird die Telefonnummer hinzugefügt der `PhoneNumber` Feld der `AspNetUsers` Tabelle. Wenn dieser Aufruf erfolgreich ist, wird die `SignInAsync` -Methode aufgerufen wird:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Die `isPersistent` Parameter legt fest, ob die authentifizierungssitzung über mehrere Anforderungen hinweg persistent gespeichert wird.

Wenn Sie Ihrem Sicherheitsprofil ändern, ein neuen sicherheitsstempel generiert und gespeichert, der `SecurityStamp` Feld der *AspNetUsers* Tabelle. Beachten Sie, dass die `SecurityStamp` Feld unterscheidet sich das Sicherheitscookie. Das Sicherheitscookie befindet sich nicht in der `AspNetUsers` Tabelle (oder an anderer Stelle in der Identity-Datenbank). Das Cookie Sicherheitstoken ist selbstsigniert mit [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) und wird erstellt, mit der `UserId, SecurityStamp` und Zeitinformationen Ablauf.

Die Cookie-Middleware überprüft das Cookie bei jeder Anforderung. Die `SecurityStampValidator` Methode in der `Startup` Klasse trifft die-Datenbank und in regelmäßigen Abständen, überprüft der sicherheitsstempel wie angegeben mit der `validateInterval`. Dies geschieht nur alle 30 Minuten (in unserem Beispiel), es sei denn, Sie Ihrem Sicherheitsprofil ändern. Das Intervall von 30 Minuten wurde gewählt, um Roundtrips zur Datenbank zu minimieren.

Die `SignInAsync` Methode muss aufgerufen werden, wenn das Sicherheitsprofil geändert wurde. Wenn das Sicherheitsprofil ändert, wird die Datenbank Updates der `SecurityStamp` Feld, und ohne Aufruf der `SignInAsync` Methode Sie angemeldet bleiben würde *nur* bis das nächste Mal die OWIN-Pipeline erreicht die Datenbank (der `validateInterval`). Können Sie testen, indem Sie ändern die `SignInAsync` Methode sofort zurück, und Festlegen des Cookies `validateInterval` Eigenschaft von 30 Minuten auf 5 Sekunden:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Mit den oben genannten Methodencode ändert, können Sie Ihrem Sicherheitsprofil ändern (z. B. durch Ändern des Status von **zwei Faktor aktiviert**) und Sie werden abgemeldet in 5 Sekunden bei der `SecurityStampValidator.OnValidateIdentity` Methode schlägt fehl. Entfernen Sie die Zeile zurück, in der `SignInAsync` -Methode, stellen Sie ein anderes Sicherheitsprofil für ändern und Sie nicht werden abgemeldet. Die `SignInAsync` Methode generiert einen neuen Cookie.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Zweistufige Authentifizierung aktivieren

In der Beispiel-app müssen Sie die Benutzeroberfläche verwenden, um zweistufige Authentifizierung (2FA) zu aktivieren. Klicken Sie auf Ihre Benutzer-ID (e-Mail-Alias) in der Navigationsleiste, um 2FA zu aktivieren.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Klicken Sie auf 2FA aktivieren.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Melden Sie sich ab und dann wieder anzumelden. Wenn Sie e-Mail aktiviert haben (finden Sie unter meinem [vorherigen Lernprogramm](account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail-Nachrichten für 2FA auswählen.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Vergewissern Sie sich Codepage wird angezeigt, in dem Sie den Code (von SMS oder e-Mail) eingeben können.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA zu verwenden, um mit diesem Computer und den Browser anmelden. 2FA aktivieren und dann auf die **Denken Sie daran Browser** werden Ihnen starken 2FA Schutz vor böswilligen Benutzern, die auf Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihren Computer haben. Sie können auf einem Computer aus privaten so vorgehen, die Sie regelmäßig verwenden. Durch Festlegen von **Denken Sie daran Browser**, erhalten Sie die zusätzliche Sicherheit des 2FA von Computern, die Sie nicht regelmäßig verwenden und erhalten Sie die Vorteile nicht auf Ihren eigenen Computern 2FA durchlaufen muss. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Vorgehensweise: Registrieren Sie einen Multi-Factor Authentication-Anbieter

Wenn Sie ein neues MVC-Projekt, erstellen die *IdentityConfig.cs* Datei enthält den folgenden Code aus, um einen Multi-Factor Authentication-Anbieter zu registrieren:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Fügen Sie eine Telefonnummer für 2FA hinzu.

Die `AddPhoneNumber` Aktionsmethode in der `Manage` Controller wird ein Sicherheitstoken generiert und sendet es an das Telefon Zahl Sie bereitgestellt haben.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Nach dem Senden des Tokens, leitet sie an der `VerifyPhoneNumber` Aktionsmethode, in dem Sie den Code so registrieren Sie SMS für 2FA eingeben können. SMS-2FA wird nicht verwendet werden, bis Sie die Telefonnummer überprüft haben.

## <a name="enabling-2fa"></a>2FA aktivieren

Die `EnableTFA` Aktionsmethode ermöglicht 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Beachten Sie die `SignInAsync` muss aufgerufen werden, da aktivieren 2FA eine Änderung auf das Sicherheitsprofil handelt. Wenn 2FA aktiviert ist, muss der Benutzer mit 2FA anmelden, verwenden die 2FA-Ansätze, die sie registriert haben, (SMS und e-Mail im Beispiel).

Sie können weitere 2FA Anbieter z. B. QR-Code-Generatoren hinzufügen, oder Schreiben Sie besitzen (finden Sie unter [mithilfe von Google Authenticator mit ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Die 2FA Codes sind mit generiert [Algorithmus zum einmaligen zeitbasierte](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) und Codes sechs Minuten lang gültig sind. Wenn Sie den Code eingeben, mehr als sechs Minuten dauern, erhalten Sie eine Fehlermeldung von ungültigem Code.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Kombinieren von sozialen und lokalen Anmeldekonten

Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden. In der folgenden Reihenfolge &quot; RickAndMSFT@gmail.com &quot; wird zuerst als eine lokale Anmeldung erstellt, aber Sie können erstellen Sie das Konto als sozialen Protokoll zuerst und anschließend fügen Sie eine lokale Anmeldung hinzu.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Klicken Sie auf den Link zu einem anderen Protokoll im Dienst, und akzeptieren Sie die app-Anforderungen. Die beiden Konten wurden kombiniert, Sie können mit entweder Konto anmelden. Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls der Anmeldung sozialen Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.

In der folgenden Abbildung ist Tom soziales Netzwerk anzumelden (die daraufhin aus der **externer Anmeldungen: 1** auf der Seite angezeigt).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Durch Klicken auf **ein Kennwort** bietet die Möglichkeit, fügen ein lokales Protokoll auf dem gleichen Konto zugeordnet.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Kontosperrung vor Brute-Force-Angriffen

Benutzersperre aktivieren, um die Konten in Ihrer app von Wörterbuchangriffe zu schützen. Im folgenden code in die `ApplicationUserManager Create` Methode konfiguriert gesperrt war:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Der obige Code ermöglicht Sperre nur für die zweistufige Authentifizierung. Während Sie gesperrt war für Anmeldungen, indem ändern aktivieren können `shouldLockout` auf "true" in der `Login` -Methode des kontocontrollers, sollten Sie nicht gesperrt war für Anmeldungen aktivieren, da es das Konto anfällig macht [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) Anmeldung Angriffe. Im Beispielcode, Sperrung deaktiviert ist, für das Administratorkonto in erstellt die `ApplicationDbInitializer Seed` Methode:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Dass der Benutzer ein überprüfte e-Mail-Konto muss

Der folgende Code muss ein Benutzer eine überprüfte e-Mail-Konto haben, bevor sie sich anmelden können:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Wie SignInManager 2FA Anforderung prüft, ob

Sowohl der lokalen Anmeldung, und soziale anmelden überprüfen, um festzustellen, ob 2FA aktiviert ist. Wenn 2FA aktiviert ist, die `SignInManager` Anmeldung Methodenrückgabe `SignInStatus.RequiresVerification`, und der Benutzer umgeleitet werden die `SendCode` Aktionsmethode, geben Sie den Code, um das Protokoll in der Sequenz abschließen müssen. Wenn der Benutzer RememberMe hat für das lokale Benutzer-Cookie festgelegt ist die `SignInManager` zurück `SignInStatus.Success` und er keinen 2FA durchlaufen.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Der folgende code zeigt die `SendCode` Aktionsmethode. Ein [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) wird erstellt, mit allen 2FA-Methoden, die für den Benutzer aktiviert. Die [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) wird zum Übergeben der [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) Hilfsfunktion, die dem Benutzer ermöglicht, wählen Sie den Ansatz 2FA (in der Regel die e-Mail- und SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Sobald der Benutzer die 2FA-Methode, sendet der `HTTP POST SendCode` Aktionsmethode aufgerufen wird, die `SignInManager` sendet, die an den 2FA-Code, und der Benutzer umgeleitet werden die `VerifyCode` Aktionsmethode, die in dem sie den Code zum Abschließen der Anmeldung eingeben können.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA Sperre

Obwohl Sie die kontosperrung auf Anmeldung Versuch Kennwortfehler festlegen können, wird dieser Ansatz Ihrer Anmeldename anfällig für [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) sperren. Es wird empfohlen, dass Sie nur mit 2FA kontosperrung verwenden. Wenn die `ApplicationUserManager` wird erstellt, legt der Beispielcode 2FA Sperre und `MaxFailedAccessAttemptsBeforeLockout` bis fünf. Sobald ein Benutzer (über lokales Konto oder soziale Konto) anmeldet, jedem fehlgeschlagenen Versuch zur 2FA gespeichert ist, und wenn die maximalen Versuche erreicht ist, der Benutzer fünf Minuten gesperrt ist (Sie können festlegen, dass die Sperre mit `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET Identity empfohlene Ressourcen](../getting-started/aspnet-identity-recommended-resources.md) vollständige Liste der Identity-Blogs, Videos, Lernprogramme und gute daher verknüpft.
- [MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) zeigt außerdem das Hinzufügen von Profilinformationen zur Users-Tabelle.
- [ASP.NET MVC und Identität 2.0: Kenntnisse der Grundlagen](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) von John Atten.
- [Kontobestätigung und Kennwortwiederherstellung mit ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Einführung zu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ankündigung der RTM-Version von ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) von Pranav Rastogi.
- [Identität für ASP.NET 2.0: Einrichten von Kontovalidierung und zweistufige Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) von John Atten.
