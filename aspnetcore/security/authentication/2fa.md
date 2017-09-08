---
title: Zweistufige Authentifizierung mit SMS
author: rick-anderson
description: Zeigt, wie zum Einrichten der zweistufigen Authentifizierung (2FA) mit ASP.NET Core
keywords: ASP.NET Core, SMS, Authentication, 2FA, zweistufige Authentifizierung, zweistufige Authentifizierung
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 77404de2f367cb12ba25433899198b69f9e5a7f2
ms.sourcegitcommit: 9a22c64759a7285ba788a37039bea5fe95f45f21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2017
---
# <a name="two-factor-authentication-with-sms"></a>Zweistufige Authentifizierung mit SMS

Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Schweiz-Entwickler](https://github.com/Swiss-Devs)

Dieses Tutorial bezieht sich auf ASP.NET Core nur 1.x. Finden Sie unter [aktivieren QR-Code-Generierung für Authentifikator-apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) für ASP.NET Core 2.0 und höher.

Dieses Lernprogramm zeigt, wie zum Einrichten der zweistufigen Authentifizierung (2FA) mithilfe von SMS. Anweisungen werden bestimmte für [Twilio](https://www.twilio.com/) und [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), aber Sie können andere SMS-Anbieter verwenden. Es wird empfohlen [Kontobestätigung und Kennwortwiederherstellung](accconfirm.md) vor dem Starten dieses Lernprogramms.

Anzeigen der [abgeschlossenen Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Erstellen eines neuen ASP.NET Core-Projekts

Erstellen eine neue ASP.NET Core-Web-app mit dem Namen `Web2FA` mit einzelnen Benutzerkonten. Befolgen Sie die Anweisungen in [Erzwingen von SSL in einer ASP.NET Core app](xref:security/enforcing-ssl) einrichten und SSL erforderlich.

### <a name="create-an-sms-account"></a>Erstellen Sie ein SMS-Konto

Erstellen Sie ein SMS-Konto, z. B. von [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Zeichnen Sie die Anmeldeinformationen für die Authentifizierung (für Twilio: AccountSid und AuthToken, für ASPSMS: Userkey und Kennwort).

#### <a name="figuring-out-sms-provider-credentials"></a>Eng zusammenliegen, SMS-Anbieter-Anmeldeinformationen

**Twilio:**  
Kopieren Sie aus Ihrem Twilio-Konto in der Registerkarte Dashboard der **Konto-SID** und **autorisierungstokens**.

**ASPSMS:**  
Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrem **Kennwort**.

Speichern wir diese Werte mit dem geheimen Schlüssel-Manager-Tool innerhalb der Schlüssel später `SMSAccountIdentification` und `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Angeben von "SenderID" / Absender

**Twilio:**  
Kopieren Sie auf der Registerkarte Zahlen Ihrer Twilio **Telefonnummer**. 

**ASPSMS:**  
Klicken Sie im Menü Urheber entsperren entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt). 

Wir werden später speichern den Wert mit dem geheimen Schlüssel-Manager-Tool im Schlüssel `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Geben Sie Anmeldeinformationen für den SMS-Dienst

Wir verwenden die [Optionen Muster](xref:fundamentals/configuration#options-config-objects) auf die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen. 

   * Erstellen Sie eine Klasse zum Abrufen von SMS-Sicherheitsschlüssel. Für dieses Beispiel die `SMSoptions` Klasse wird erstellt, der *Services/SMSoptions.cs* Datei.

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Legen Sie die `SMSAccountIdentification`, `SMSAccountPassword` und `SMSAccountFrom` mit der [Secret-Manager-Tool](xref:security/app-secrets). Zum Beispiel:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Fügen Sie das NuGet-Paket für den SMS-Anbieter. Aus der Paket-Manager-Konsole (PMC) ausführen:

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Fügen Sie Code in der *Services/MessageServices.cs* Datei SMS zu aktivieren. Verwenden Sie die Twilio oder Abschnitt ASPSMS:


**Twilio:**  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Konfigurieren Sie starten auf, wenn verwenden`SMSoptions`

Hinzufügen `SMSoptions` in dem Dienstcontainer den `ConfigureServices` Methode in der *Startup.cs*:

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Zweistufige Authentifizierung aktivieren

Öffnen der *Views/Manage/Index.cshtml* Razor-Datei, und entfernen Sie der Kommentar Zeichen (damit kein Markup auskommentiert ist).

## <a name="log-in-with-two-factor-authentication"></a>Melden Sie sich mit einer zweistufigen Authentifizierung

* Die app auszuführen und einen neuen Benutzer registrieren

![-Webanwendung Register Ansicht öffnen Sie in Microsoft Edge](2fa/_static/login2fa1.png)

* Tippen Sie auf Ihren Benutzernamen, das aktiviert wird, die `Index` Aktionsmethode im Controller verwalten. Tippen Sie dann auf die Telefonnummer **hinzufügen** Link.

![Verwalten von anzeigen](2fa/_static/login2fa2.png)

* Hinzufügen einer Telefonnummer, die den Prüfcode empfangen wird, und tippen Sie auf **senden Prüfcode**.

![Seite "Phone Number" hinzufügen](2fa/_static/login2fa3.png)

* Sie erhalten eine SMS mit der Überprüfungscode. Geben Sie ihn, und tippen Sie auf **senden**

![Überprüfen Sie die Seite "Phone Number"](2fa/_static/login2fa4.png)

Wenn Sie eine Textnachricht nicht erhalten, finden Sie unter Seite für Twilio-Protokoll.

* Die Ansicht verwalten zeigt, dass Ihre Telefonnummer wurde erfolgreich hinzugefügt wurde.

![Verwalten von anzeigen](2fa/_static/login2fa5.png)

* Tippen Sie auf **aktivieren** zweistufige Authentifizierung zu aktivieren.

![Verwalten von anzeigen](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Die zweistufige Authentifizierung testen

* Melden Sie sich ab.

* Anmelden.

* Das Benutzerkonto verfügt über zweistufige Authentifizierung aktiviert, daher Sie die zweite Stufe der Authentifizierung bereitzustellen müssen. In diesem Lernprogramm haben Sie die Überprüfung per Bürotelefon aktiviert. Die integrierten Vorlagen ermöglichen außerdem das Einrichten von e-Mail als zweiter Faktor. Sie können zusätzliche zweite Faktoren für die Authentifizierung, z. B. QR-Codes einrichten. Tippen Sie auf **übermitteln**.

![Überprüfungscode senden](2fa/_static/login2fa7.png)

* Geben Sie den Code, den Sie per SMS-Textnachricht erhalten.

* Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA für die Anmeldung bei Verwendung der gleichen Geräte und Browser verwenden. 2FA aktivieren und dann auf **Denken Sie daran Browser** werden Ihnen starken 2FA Schutz vor böswilligen Benutzern, die auf Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihr Gerät haben. Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden. Durch Festlegen von **Denken Sie daran Browser**, erhalten Sie die zusätzliche Sicherheit des 2FA von Geräten, die Sie nicht regelmäßig verwenden und erhalten Sie die Vorteile nicht auf Ihren eigenen Geräten 2FA durchlaufen muss.

![Vergewissern Sie sich anzeigen](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Kontosperrung zum Schutz vor Brute-Force-Angriffen

Es wird empfohlen, dass Sie kontosperrung mit 2FA verwenden. Sobald ein Benutzer (über ein lokales Konto oder soziale Konto) anmeldet, jedem fehlgeschlagenen Versuch zur 2FA gespeichert ist, und wenn die maximale Versuche (Standard ist 5) wird erreicht, der Benutzer fünf Minuten gesperrt ist (Sie können festlegen, dass die Sperre mit `DefaultAccountLockoutTimeSpan`). Der folgende Code konfiguriert Konto für 10 Minuten nach 10 fehlgeschlagenen Versuchen ausgesperrt werden.

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
