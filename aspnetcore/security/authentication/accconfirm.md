---
title: "Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core"
author: rick-anderson
description: "Informationen Sie zum Erstellen einer ASP.NET Core-Apps mit der e-Mail-Bestätigung und das Kennwort zurücksetzen."
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b236b4e5d3a4fa7212453f2aec209d145f5f5e32
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Kontobestätigung und kennwortwiederherstellung in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)

In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Apps mit e-Mail-Bestätigung und das Kennwort zurücksetzen. Dieses Lernprogramm ist **nicht** ein Thema ab. Sie sollten mit vertraut sein:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentifizierung](xref:security/authentication/index)
* [Kontobestätigung und Kennwortwiederherstellung](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für die ASP.NET Core MVC 1.1 "und" 2.x-Versionen.

## <a name="prerequisites"></a>Erforderliche Komponenten

[.NET Core 2.1.4 SDK](https://www.microsoft.com/net/core) oder höher.

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Erstellen Sie ein neues ASP.NET Core-Projekt mit der .NET Core-CLI

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* `--auth Individual` Gibt die Projektvorlage der einzelnen Benutzerkonten an.
* Fügen Sie auf Windows, die `-uld` Option. Es gibt an, dass LocalDB anstelle von SQLite verwendet werden soll.
* Führen Sie `new mvc --help` um Hilfe zu diesem Befehl zu erhalten.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn Sie die CLI oder SQLite verwenden, führen Sie in einem Befehlsfenster Folgendes:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Gibt die Projektvorlage der einzelnen Benutzerkonten an.
* Fügen Sie auf Windows, die `-uld` Option. Es gibt an, dass LocalDB anstelle von SQLite verwendet werden soll.
* Führen Sie `new mvc --help` um Hilfe zu diesem Befehl zu erhalten.

---

Alternativ können Sie ein neues ASP.NET Core-Projekt mit Visual Studio erstellen:

* Erstellen Sie in Visual Studio ein neues **Webanwendung** Projekt.
* Wählen Sie **ASP.NET Core 2.0**. **.NET Core** in der folgenden Abbildung ausgewählt ist, aber Sie können auswählen, **.NET Framework**.
* Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.
* Behalten Sie den Standardwert **in app-Store Benutzerkonten**.

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Testen Sie die Registrierung neuer Benutzer

Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer. Befolgen Sie die Anweisungen zum Ausführen von Entity Framework Core-Migrationen aus. An diesem Punkt wird die ausschließliche Überprüfung auf die e-Mail-Adresse mit der [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) Attribut. Nach dem Senden der Registrierungs, sind Sie in der app angemeldet. Später in diesem Lernprogramm wird der Code aktualisiert, damit neue Benutzer anmelden können, bis ihre e-Mails überprüft wurde.

## <a name="view-the-identity-database"></a>Anzeigen der Identity-Datenbank

Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anleitungen zur Anzeige der SQLite-Datenbank.

Für Visual Studio:

* Aus der **Ansicht** klicken Sie im Menü **Objekt-Explorer von SQL Server** (SSOX).
* Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer 13)**. Mit der rechten Maustaste auf **Dbo. AspNetUsers** > **zeigen Daten**:

![Kontextmenü für AspNetUsers-Tabelle in SQL Server-Objekt-Explorer](accconfirm/_static/ssox.png)

Beachten Sie der Tabelle `EmailConfirmed` Feld ist `False`.

Möglicherweise möchten diese e-Mail-Adresse verwenden erneut im nächsten Schritt, wenn die app eine e-Mail zur kaufbestätigung sendet. Mit der rechten Maustaste auf die Zeile, und wählen **löschen**. Löschen den e-Mail-Alias erleichtert es in den folgenden Schritten.

---

## <a name="require-https"></a>HTTPS erforderlich

Finden Sie unter [HTTPS erforderlich](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>E-Mail-Bestätigung

Es wird empfohlen, die e-Mail-Adresse eines eine neue benutzerregistrierung zu bestätigen. E-Mail-Bestätigung hilft Ihnen bei der überprüfen, haben sie keinen Identitätswechsel eine andere Person (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse). Angenommen, ein Diskussionsforum mussten, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com." Ohne e-Mail-Bestätigung "nolivetto@contoso.com" erhalten Sie unerwünschten e-Mail konnte aus Ihrer app. Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" und hadn't bemerkt, dass die falsche Schreibweise des "Yli". Sie wäre nicht kennwortwiederherstellung verwenden, da die app ihre korrekte e-Mail-Adresse hat. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots. E-Mail-Bestätigung stellt keinen Schutz vor böswilligen Benutzern viele e-Mail-Konten.

In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie eine bestätigte e-Mail-Adresse aufweisen.

Update `ConfigureServices` eine bestätigte e-Mail-Adresse erforderlich ist:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` verhindert, dass registrierte Benutzer anmelden, bis ihre-e-Mail bestätigt ist.

### <a name="configure-email-provider"></a>Konfigurieren von e-Mail-Anbieter

In diesem Lernprogramm wird SendGrid zum Senden von e-Mails. Sie benötigen ein SendGrid-Konto und ein Schlüssel zum Senden von e-Mail. Sie können andere e-Mail-Anbieter verwenden. ASP.NET Core 2.x enthält `System.Net.Mail`, womit Sie das Senden von e-Mails aus Ihrer app. Es wird empfohlen, dass Sie SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden. SMTP ist schwer zu schützen und ordnungsgemäß eingerichtet wurden.

Die [Optionen Muster](xref:fundamentals/configuration/options) wird verwendet, um die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

Erstellen Sie eine Klasse, um den e-Mail-Schlüssel abzurufen. Für dieses Beispiel die `AuthMessageSenderOptions` Klasse wird erstellt, der *Services/AuthMessageSenderOptions.cs* Datei:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](xref:security/app-secrets). Zum Beispiel:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Unter Windows, geheime Schlüssel-Manager speichert Schlüssel/Wert-Paare in ein *secrets.json* in der Datei die `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` Verzeichnis.

Der Inhalt der *secrets.json* Datei sind nicht verschlüsselt. Die *secrets.json* Datei wird unten gezeigt (die `SendGridKey` Wert entfernt wurde.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurieren Sie Start AuthMessageSenderOptions Verwendung

Hinzufügen `AuthMessageSenderOptions` dem Dienstcontainer am Ende der `ConfigureServices` Methode in der *Startup.cs* Datei:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurieren Sie die AuthMessageSender-Klasse

Dieses Lernprogramm veranschaulicht das Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden.

Installieren der `SendGrid` NuGet-Paket:

* Über die Befehlszeile:

    `dotnet add package SendGrid`

* Der Paket-Manager-Konsole geben Sie den folgenden Befehl aus:

 `Install-Package SendGrid`

Finden Sie unter [erste Schritte mit SendGrid kostenlos](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto registrieren.

#### <a name="configure-sendgrid"></a>Konfigurieren von SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um die SendGrid zu konfigurieren, fügen Sie Code ähnlich dem folgenden in *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Fügen Sie Code in *Services/MessageServices.cs* ähnlich der folgenden SendGrid konfigurieren:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Konto bestätigen und Kennwort Wiederherstellung aktivieren

Die Vorlage, den Code für die Wiederherstellung für Konto bestätigen und das Kennwort hat. Suchen der `OnPostAsync` Methode im *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Verhindern Sie, dass neu registrierte Benutzern zur Verfügung wird automatisch durch die folgende Zeile auskommentiert angemeldet werden:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Die vollständige Methode ist mit der geänderten Zeile hervorgehoben dargestellt:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Zum Aktivieren der kontobestätigung kommentieren Sie den folgenden Code ein:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Hinweis:** Code verhindert, dass einen neu registrierten Benutzer wird automatisch durch die folgende Zeile auskommentiert angemeldet:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Kennwortwiederherstellung aktivieren, indem Sie den Code in Auskommentieren der `ForgotPassword` Aktion *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Kommentieren Sie die Form-Elements im *Views/Account/ForgotPassword.cshtml*. Möglicherweise möchten Sie entfernen die `<p> For more information on how to enable reset password ... </p>` Element, das einen Link zu dieser Artikel enthält.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Registrieren, e-Mails zu bestätigen und Kennwort zurücksetzen

Führen Sie die Web-app, und Testen Sie die kontobestätigung und das Kennwort eine Wiederherstellung durchführen.

* Die app auszuführen und einen neuen Benutzer registrieren

 ![Webansicht Anwendung Konto registrieren](accconfirm/_static/loginaccconfirm1.png)

* Suchen Sie nach der Link für die Bestätigung Ihrer e-Mail. Finden Sie unter [Debuggen e-Mail](#debug) , wenn Sie die e-Mail nicht erhalten.
* Klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.
* Melden Sie sich mit Ihren e-Mail- und das Kennwort an.
* Melden Sie sich ab.

### <a name="view-the-manage-page"></a>Anzeigen der Seite "verwalten"

Wählen Sie Ihren Benutzernamen im Browser: ![Browserfenster mit Benutzernamen](accconfirm/_static/un.png)

Möglicherweise müssen Sie die Navigationsleiste, um Benutzername erweitern.

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt. Die **E-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mails bestätigt wurde.

![Seite "verwalten"](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dies wird später in diesem Lernprogramm erwähnt.
![Seite "verwalten"](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test Zurücksetzen des Kennworts

* Wenn Sie angemeldet sind, wählen Sie **Logout**.
* Wählen Sie die **melden Sie sich** verknüpfen, und wählen Sie die **haben Sie Ihr Kennwort vergessen?** Link.
* Geben Sie die e-Mail, die Sie verwendet, um das Konto zu registrieren.
* Eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts gesendet wird. Überprüfen Sie Ihre e-Mail-Adresse, und klicken Sie auf den Link, um Ihr Kennwort zurückzusetzen. Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie sich mit Ihrer e-Mail- und das neue Kennwort anmelden.

<a name="debug"></a>

### <a name="debug-email"></a>Debuggen-e-Mail

Wenn Sie e-Mail-nicht funktioniert:

* Erstellen einer [Konsolen-app zum Senden von e-Mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Überprüfen Sie die [e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.
* Überprüfen Sie Ihre Spam-Ordner.
* Wiederholen Sie dann eine andere e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail, usw.).
* Wiederholen Sie den senden an andere e-Mail-Konten.

**Eine bewährte Sicherheitsmethode** entspricht **nicht** Produktion geheime Schlüssel in Test- und verwenden. Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-geheimen als Anwendungseinstellungen im Azure-Web-App-Portal festlegen. Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.

## <a name="combine-social-and-local-login-accounts"></a>Kombinieren von sozialen und lokalen Anmeldekonten

Um in diesem Abschnitt abzuschließen, müssen Sie zunächst ein externen Authentifizierungsanbieters aktivieren. Finden Sie unter [Facebook, Google, und die externe Anbieter Authentifizierung](xref:security/authentication/social/index).

Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden. In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird als eine lokale Anmeldung; zuerst erstellt jedoch zunächst erstellen Sie das Konto als sozialen Anmeldung werden können, und fügen Sie eine lokale Anmeldung hinzu.

![-Webanwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.

![Verwalten von anzeigen](accconfirm/_static/manage.png)

Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen. In der folgenden Abbildung wird die Facebook dem externen Authentifizierungsanbieter:

![Verwalten Sie externer Anmeldungen Ansicht Facebook auflisten](accconfirm/_static/fb.png)

Die beiden Konten wurden kombiniert. Sie können mit entweder Konto anmelden. Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls ihre sozialen Anmeldung Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Aktivieren Sie kontobestätigung, nachdem an einem Standort Benutzer gelten.

Aktivieren des kontobestätigung an einem Standort mit Benutzern sperrt alle vorhandenen Benutzer. Vorhandene Benutzer werden gesperrt, da ihre Konten bestätigt werden nicht. Beenden Benutzersperre umgehen, verwenden Sie eine der folgenden Vorgehensweisen:

* Aktualisieren der Datenbank, um als bestätigt werden alle vorhandenen Benutzer zu markieren.
* Bestätigen Sie vorhandene Benutzer. Beispielsweise Batch-Send-e-Mails mit Bestätigung Links.
