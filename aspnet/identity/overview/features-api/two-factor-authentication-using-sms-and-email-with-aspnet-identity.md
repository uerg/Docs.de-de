---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Zwei-Faktor-Authentifizierung, die mithilfe von SMS und e-Mail-Adresse mit ASP.NET Identity | Microsoft-Dokumentation
author: HaoK
description: Dieses Tutorial zeigt Ihnen, wie Sie die zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail-Adresse einrichten. Dieser Artikel von Rick Anderson geschrieben wurde ( @RickAndMSFT ), Pr...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b253923696e35e59c196578a232f53c11671d16
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577976"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Zwei-Faktor-Authentifizierung, die mithilfe von SMS und e-Mail-Adresse mit ASP.NET Identity
====================
durch [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Dieses Tutorial zeigt Ihnen, wie Sie die zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail-Adresse einrichten.
> 
> Dieser Artikel von Rick Anderson geschrieben wurde ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung und Suhas Joshi. Das NuGet-Beispiel wurde in erster Linie von Hao Kung geschrieben.


Dieses Thema behandelt Folgendes:

- [Erstellen der Identity-Beispiele](#build)
- [SMS für die zweistufige Authentifizierung einrichten](#SMS)
- [Zwei-Faktor-Authentifizierung aktivieren](#enable2)
- [Gewusst wie: einen zwei-Faktor-Authentifizierung-Anbieter registrieren](#reg)
- [Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung](#combine)
- [Die Sperrung von Konten vor brute-Force-Angriffen](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Erstellen der Identity-Beispiele

In diesem Abschnitt verwenden Sie NuGet das Herunterladen eines Beispiels, mit denen wir zusammenarbeiten werden. Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren von Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.

> [!NOTE]
> Warnung: Sie müssen Visual Studio installieren [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) zum Durchführen dieses Tutorials.


1. Erstellen Sie ein neues ***leere*** ASP.NET-Webprojekt.
2. Geben Sie den folgenden, in der Paket-Manager-Konsole die folgenden Befehle aus:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   In diesem Tutorial verwenden wir [SendGrid](http://sendgrid.com/) zum Senden von e-Mails und [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) für Sms Hilfsmittel. Die `Identity.Samples` Paket wird installiert, den wir arbeiten mit Code.
3. Legen Sie die [Projekt für die Verwendung von SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Optionale*: befolgen Sie die Anweisungen in Mein [e-Mail-Bestätigung Tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) SendGrid einbinden und führen Sie die app, und ein e-Mail-Konto zu registrieren.
5. * Optional: * den Link zur Bestätigungscode der Demo-e-Mail aus dem Beispiel zu entfernen (die `ViewBag.Link` Code im Kontocontroller. Finden Sie unter den `DisplayEmail` und `ForgotPasswordConfirmation` Aktionsmethoden und Razor-Ansichten).
6. <em>Optional: * Entfernen der `ViewBag.Status` Code aus die Controller verwalten und das Konto und die *Views\Account\VerifyCode.cshtml</em> und <em>Views\Manage\VerifyPhoneNumber.cshtml</em> Razor-Ansichten. Alternativ können Sie halten die `ViewBag.Status` So testen die Funktionsweise dieser app lokal ohne zu verknüpfen und Senden von e-Mail und SMS-Nachrichten an.

> [!NOTE]
> Warnung: Wenn Sie die Sicherheitseinstellungen in diesem Beispiel ändern, Produktionen apps müssen eine sicherheitsüberprüfung unterzogen werden, die explizit, die Änderungen vorgenommen aufruft.


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
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresse:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Ermitteln der Anmeldeinformationen des Benutzers für SMS-Anbieter**  
  
   Twilio:  
   Von der **Dashboard** Ihrem Twilio-Konto kopieren auf der Registerkarte die **Konto-SID** und **Authentifizierungstoken**.  
  
   ASPSMS:  
   Navigieren Sie in Ihren kontoeinstellungen zu **Userkey** und kopieren Sie ihn zusammen mit Ihrer selbstdefinierte **Kennwort**.  
  
   Wir werden diese Werte später speichern, in den Variablen `SMSAccountIdentification` und `SMSAccountPassword` .
4. **Angeben der Absender-ID / Ersteller**  
  
   Twilio:  
   Von der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio-Telefonnummer.  
  
   ASPSMS:  
   In der **entsperren Urheber** Menü ein oder mehrere Urheber zu entsperren, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).  
  
   Wir werden diesen Wert später in der Variablen speichern `SMSAccountFrom` .
5. **SMS-Anbieter-Anmeldeinformationen werden in app übertragen.**  
  
   Stellen Sie die Anmeldeinformationen und die Telefonnummer des Absenders der app zur Verfügung:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern. Das Konto und die Anmeldeinformationen werden auf den Code aus, um das Beispiel möglichst einfach zu halten hinzugefügt. Finden Sie unter der Jon Atten [ASP.NET MVC: Beibehalten von privaten Einstellungen außerhalb des Datenquellen-Steuerelements](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementierung der Datenübertragung an SMS-Anbieter**  
  
   Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* Datei.  
  
   Aktivieren Sie je nach verwendeter SMS-Anbieter entweder die **Twilio** oder **ASPSMS** Abschnitt: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Führen Sie die app aus, und melden Sie sich mit dem Konto an, die, das Sie zuvor registriert.
8. Klicken Sie auf Ihre Benutzer-ID, das aktiviert wird, die `Index` Aktionsmethode im `Manage` Controller.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Klicken Sie auf Hinzufügen.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Innerhalb weniger Sekunden erhalten Sie eine Textnachricht mit dem Überprüfungscode. Geben Sie ihn, und drücken Sie die **senden**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Die Ansicht "verwalten" zeigt, dass Ihre Telefonnummer hinzugefügt wurde.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Überprüfen Sie den code

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Die `Index` Aktionsmethode im `Manage` Controller legt die Statusmeldung, die basierend auf Ihrer früheren Aktion und bietet Links, um Ihr lokales Kennwort ändern oder Hinzufügen eines lokalen Kontos. Die `Index` Methode zeigt auch den Zustand oder Ihre 2FA phone Anzahl, externer Anmeldungen, 2FA aktiviert, und denken Sie daran 2FA-Methode für diesen Browser (weiter unten erläutert). Durch Klicken auf die Ihre Benutzer-ID (e-Mail) in der Titelleiste, keine Nachricht übergeben. Durch Klicken auf die **Telefonnummer: Entfernen von** verknüpfen übergibt `Message=RemovePhoneSuccess` als Abfragezeichenfolge.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

Die `AddPhoneNumber` Action-Methode zeigt ein Dialogfeld, um eine Telefonnummer eingeben, die SMS-Nachrichten empfangen kann.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Durch Klicken auf die **Überprüfungscode senden** Schaltfläche stellt die Telefonnummer für den HTTP-POST `AddPhoneNumber` Aktionsmethode.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Die `GenerateChangePhoneNumberTokenAsync` generiert die Methode das Sicherheitstoken, die in der SMS-Nachricht festgelegt werden. Wenn der SMS-Dienst konfiguriert wurde, wird das Token als Zeichenfolge gesendet &quot;Ihr Sicherheitscode lautet &lt;token&gt;&quot;. Die `SmsService.SendAsync` Methode asynchron aufgerufen wird, und klicken Sie dann auf die app umgeleitet wird die `VerifyPhoneNumber` Aktionsmethode (in der das folgende Dialogfeld angezeigt), in dem Sie den Prüfcode eingeben können.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Nachdem Sie geben Sie den Code, und klicken Sie auf Absenden, der Code an die HTTP-POST gesendet wird `VerifyPhoneNumber` Aktionsmethode.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Die `ChangePhoneNumberAsync` Methode überprüft die bereitgestellte Sicherheitscode. Wenn der Code korrekt ist, wird die Telefonnummer hinzugefügt der `PhoneNumber` Feld der `AspNetUsers` Tabelle. Wenn dieser Aufruf erfolgreich ist, wird die `SignInAsync` aufgerufen:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Die `isPersistent` Parameter legt fest, ob die authentifizierungssitzung über mehrere Anforderungen hinweg persistent gespeichert wird.

Wenn Sie Ihrem Sicherheitsprofil ändern, ein neuen sicherheitsstempel generiert und gespeichert, der `SecurityStamp` Feld der *"aspnetusers"* Tabelle. Beachten Sie, dass die `SecurityStamp` Feld unterscheidet sich das Sicherheitscookie. Das Sicherheitscookie befindet sich nicht in der `AspNetUsers` Tabelle (oder an anderer Stelle in der Identity-DB). Das Sicherheitstoken für das Cookie ist selbstsigniert mit [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) und wird erstellt, mit der `UserId, SecurityStamp` und Ablauf von Uhrzeitangaben.

Die Cookie-Middleware überprüft das Cookie bei jeder Anforderung. Die `SecurityStampValidator` -Methode in der die `Startup` Klasse erreicht die Datenbank und in regelmäßigen Abständen, überprüft der sicherheitsstempel gemäß der Angabe der `validateInterval`. Dies geschieht nur alle 30 Minuten (in unserem Beispiel), es sei denn, Sie zu Ihrem Sicherheitsprofil ändern. Das 30-Minuten-Intervall wurde gewählt, um Roundtrips zur Datenbank zu minimieren.

Die `SignInAsync` -Methode muss aufgerufen werden, wenn eine Änderung, um das Sicherheitsprofil vorgenommen wird. Wenn sich das Sicherheitsprofil ändert, wird die Datenbank Updates der `SecurityStamp` Feld und ohne Aufrufen der `SignInAsync` Methode, die Sie angemeldeten verbleiben *nur* bis das nächste Mal die OWIN-Pipeline erreicht die Datenbank (die `validateInterval`). Sie können dies testen, indem Sie ändern die `SignInAsync` Methode sofort zurückgegeben, und Festlegen des Cookies `validateInterval` Eigenschaft von 30 Minuten auf 5 Sekunden:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Mithilfe der oben genannten ändert, können Sie Ihrem Sicherheitsprofil ändern (z. B. durch Ändern des Status **zwei-Faktor aktiviert**) und Sie werden abgemeldet innerhalb von 5 Sekunden bei der `SecurityStampValidator.OnValidateIdentity` -Methode fehlschlägt. Entfernen Sie die Zeile zurück, in der `SignInAsync` -Methode, stellen Sie eine andere Sicherheitsprofil zu ändern, und Sie werden nicht abgemeldet. Die `SignInAsync` Methode generiert ein neues Sicherheitscookie.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Zwei-Faktor-Authentifizierung aktivieren

In der Beispiel-app müssen Sie die Benutzeroberfläche verwenden, um die zweistufige Authentifizierung (2FA) zu aktivieren. Klicken Sie auf Ihre Benutzer-ID (e-Mail-Alias) in der Navigationsleiste, um die Aktivierung der 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Klicken Sie auf Aktivieren 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Melden Sie sich ab und dann wieder anmelden. Wenn Sie e-Mail-Adresse aktiviert haben (finden Sie unter meinem [vorherigen Tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail-Adresse für 2FA auswählen.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Die Überprüfen von Code-Seite wird angezeigt, in dem Sie den Code (von SMS oder e-Mail-Adresse) eingeben können.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Durch Klicken auf die **diesen Browser merken** das Kontrollkästchen schließen Sie 2FA zu verwenden, um mit diesem Computer und den Browser anmelden müssen. 2FA aktiviert und auf die **diesen Browser merken** erhalten Sie starke 2FA Schutz vor böswilligen Benutzern, die Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihrem Computer haben. Sie können auf einem privaten Computer dazu, die Sie regelmäßig verwenden. Durch Festlegen von **diesen Browser merken**, Sie erhalten die Erhöhung der Sicherheit der 2FA von Computern, die Sie nicht regelmäßig verwenden und Sie erhalten die Vorteile nicht auf Ihren eigenen Computern 2FA durchlaufen haben. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Gewusst wie: einen zwei-Faktor-Authentifizierung-Anbieter registrieren

Wenn Sie ein neues MVC-Projekt, erstellen die *IdentityConfig.cs* -Datei enthält den folgenden Code, um einen zwei-Faktor-Authentifizierung-Anbieter zu registrieren:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Fügen Sie eine Telefonnummer für 2FA

Die `AddPhoneNumber` Aktionsmethode in der `Manage` Controller ein Sicherheitstoken generiert und sendet es an das Telefon nummerieren Sie angegeben haben.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Nach dem Senden des Tokens, leitet es an der `VerifyPhoneNumber` Action-Methode, in dem Sie den Code zum Registrieren von SMS für 2FA eingeben können. SMS-2FA wird nicht verwendet werden, bis Sie die Telefonnummer überprüft haben.

## <a name="enabling-2fa"></a>Aktivierung der 2FA

Die `EnableTFA` Aktionsmethode ermöglicht 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Beachten Sie die `SignInAsync` muss aufgerufen werden, da aktivieren 2FA eine Änderung an das Sicherheitsprofil ist. Wenn 2FA aktiviert ist, muss der Benutzer mit 2FA sich angemeldet haben, über die 2FA Herangehensweisen, die sie registriert haben, (SMS und e-Mail-Adresse in dem Beispiel).

Sie können weitere 2FA Anbieter wie z. B. QR-Code-Generatoren hinzufügen, oder Sie können schreiben, Sie besitzen (finden Sie unter [mithilfe von Google Authenticator mit ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Die Codes 2FA werden mit generiert [zeitbasierte Einmalkennwort Algorithmus](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) und Codes sechs Minuten lang gültig sind. Wenn Sie mehr als sechs Minuten lang, um den Code einzugeben verwenden, erhalten Sie die folgende Fehlermeldung: Ungültiger Code.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung

Sie können lokale als auch social kombinieren, durch Klicken auf Ihre e-Mail-Link. In der folgenden Reihenfolge &quot; RickAndMSFT@gmail.com &quot; wird zuerst als einen lokalen Benutzernamen erstellt, aber Sie können das Konto als eine soziale Anmeldung erste erstellen und dann fügen Sie eine lokale Anmeldung hinzu.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die 0 externe (Anmeldungen per sozialem Netzwerk) mit diesem Konto verknüpft.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Klicken Sie auf den Link zu einem anderen Protokoll im Dienst, und akzeptieren Sie die app-Anforderungen. Wurden die beiden Konten zusammengefasst, die mit Konten anmelden können. Sie sollten Ihre Benutzer lokale Konten hinzufügen, falls der sozialen Anmeldung Authentication-Dienst nicht unterbrochen ist oder wahrscheinlich haben sie Zugriff auf ihre Konten sozialer Netzwerke verloren.

In der folgenden Abbildung, Tom ist eine soziale Anmeldung (den Sie können die **externer Anmeldungen: 1** auf der Seite angezeigt).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Durch Klicken auf **wählen Sie ein Kennwort** ermöglicht es Ihnen, ein lokales Protokoll hinzufügen, mit dem gleichen Konto verknüpft ist.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Die Sperrung von Konten vor brute-Force-Angriffen

Durch Aktivieren der Benutzersperre können Sie die Konten in Ihrer app von Wörterbuchangriffe schützen. Im folgenden code in die `ApplicationUserManager Create` -Methode konfiguriert gesperrt:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Der obige Code ermöglicht die Sperre nur für die zweistufige Authentifizierung. Während Sie gesperrt für Anmeldenamen ändern können `shouldLockout` auf "true", in der `Login` Methode des kontocontrollers, wir empfehlen Sie nicht gesperrt war, für die Anmeldungen aktivieren, da das Konto anfällig ist [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) Anmeldung Angriffe. Im Beispielcode, Sperre deaktiviert ist, für das Administratorkonto in erstellt die `ApplicationDbInitializer Seed` Methode:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Dass der Benutzer ein überprüften e-Mail-Konto

Der folgende Code muss ein Benutzer ein überprüfte-e-Mail-Konto verfügen, bevor sie sich anmelden können:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Wie SignInManager 2FA Anforderung prüft, ob

Lokale Anmeldung sowohl die sozialen Anmeldung überprüfen, ob 2FA aktiviert ist. Wenn 2FA aktiviert ist, die `SignInManager` Anmeldemethode gibt `SignInStatus.RequiresVerification`, und der Benutzer umgeleitet die `SendCode` Aktionsmethode, geben Sie den Code, um das Protokoll in der Sequenz abgeschlossen werden. müssen. Wenn der Benutzer RememberMe festgelegt ist, auf dem lokalen Benutzer-Cookie, der `SignInManager` zurück `SignInStatus.Success` und sie haben keinen 2FA durchlaufen.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Der folgende code zeigt die `SendCode` Aktionsmethode. Ein [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) wird erstellt, mit allen 2FA-Methoden, die für den Benutzer aktiviert. Die [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) übergeben wird, um die [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) Helper, wodurch der Benutzer die Auswahl den 2FA-Ansatz (in der Regel e-Mail und SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Sobald der Benutzer den 2FA-Ansatz, sendet der `HTTP POST SendCode` Aktionsmethode aufgerufen wird, die `SignInManager` sendet, die auf den 2FA-Code, und der Benutzer umgeleitet wird die `VerifyCode` Aktionsmethode, die in dem sie den Code zum Abschließen der Anmeldung eingeben können.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA-Sperre

Obwohl Sie die kontosperrung auf Anmeldefehler in Verbindung mit Kennwort Versuch festlegen können, wird dieser Ansatz Ihr Anmeldename anfällig für [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) Sperrungen. Es wird empfohlen, dass Sie nur mit 2FA Sperrung von Konten verwenden. Wenn die `ApplicationUserManager` wird erstellt, legt der Beispielcode 2FA Sperre und `MaxFailedAccessAttemptsBeforeLockout` fünf. Nach der Anmeldung des Benutzers (über lokales Konto oder ein Konto für soziales Netzwerk) und jeder fehlerhafte Versuch am 2FA wird gespeichert, wenn die maximalen Versuche erreicht wird, der Benutzer gesperrt ist fünf Minuten (Sie können festlegen, mit die Sperre `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET Identity – empfohlene Ressourcen](../getting-started/aspnet-identity-recommended-resources.md) vollständige Liste der Identity-Blogs, Videos, Lernprogramme und gut SO verknüpft.
- [MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) außerdem gezeigt, wie der Tabelle Informationen zum Profil hinzufügen.
- [ASP.NET MVC und Identität 2.0: Kenntnisse der Grundlagen](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) von John Atten.
- [Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Einführung zu ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Ankündigung der RTM-Version von ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) von Pranav Rastogi.
- [ASP.NET 2.0-Identität: Einrichten von überprüft und zwei-Faktor-Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) von John Atten.
