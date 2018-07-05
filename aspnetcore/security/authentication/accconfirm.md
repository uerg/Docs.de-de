---
title: Kontobestätigung und kennwortwiederherstellung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen einer ASP.NET Core-app mit e-Mail-Bestätigung und kennwortzurücksetzung.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803271"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Kontobestätigung und kennwortwiederherstellung in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)

In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET Core-app mit e-Mail-Bestätigung und kennwortzurücksetzung erstellen. Dieses Tutorial ist der **nicht** ein Thema ab. Sie sollten mit vertraut sein:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentifizierung](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für die Versionen 1.1 von ASP.NET Core MVC und 2.x.

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Erstellen Sie ein neues ASP.NET Core-Projekt mit .NET Core-CLI

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Gibt an, die einzelne Benutzerkonten-Projektvorlage.
* Fügen Sie auf Windows, die `-uld` Option. Es gibt an, dass LocalDB statt SQLite verwendet werden soll.
* Führen Sie `new mvc --help` um Hilfe zu diesem Befehl erhalten.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn Sie die CLI oder SQLite verwenden, führen Sie Folgendes in einem Befehlsfenster ein:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Gibt an, die einzelne Benutzerkonten-Projektvorlage.
* Fügen Sie auf Windows, die `-uld` Option. Es gibt an, dass LocalDB statt SQLite verwendet werden soll.
* Führen Sie `new mvc --help` um Hilfe zu diesem Befehl erhalten.

---

Alternativ können Sie ein neues ASP.NET Core-Projekt mit Visual Studio erstellen:

* Erstellen Sie in Visual Studio ein neues **Webanwendung** Projekt.
* Wählen Sie **ASP.NET Core 2.0**. **.NET Core** ausgewählt ist, in der folgenden Abbildung, Sie können jedoch **.NET Framework**.
* Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.
* Behalten Sie den Standardwert **in-app-Store Benutzerkonten**.

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Testen Sie die Registrierung neuer Benutzer

Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer. Befolgen Sie die Anweisungen zum Ausführen von Entity Framework Core-Migrationen aus. Die ausschließliche Überprüfung der auf die e-Mail-Adresse an diesem Punkt ist, mit der [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) Attribut. Nach der Übermittlung der Registrierungs, werden Sie bei der app angemeldet. Weiter unten in diesem Tutorial wird der Code aktualisiert, damit neue Benutzer können sich nicht anmelden, bis ihre e-Mail-Adresse überprüft wurde.

## <a name="view-the-identity-database"></a>Abrufen der Identität-Datenbank

Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anweisungen zum Anzeigen der SQLite-Datenbank.

Für Visual Studio:

* Von der **Ansicht** , wählen Sie im Menü **Objekt-Explorer von SQL Server** (SSOX).
* Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer-13)**. Mit der rechten Maustaste auf **Dbo. "Aspnetusers"** > **zeigen Daten**:

![Über das Kontextmenü für die Tabelle "aspnetusers" im Objekt-Explorer von SQL Server](accconfirm/_static/ssox.png)

Beachten Sie die Tabelle `EmailConfirmed` Feld `False`.

Sie möchten möglicherweise verwenden Sie diese e-Mail erneut im nächsten Schritt, wenn die app eine Bestätigung per e-Mail sendet. Mit der rechten Maustaste auf die Zeile, und wählen **löschen**. Löschen den e-Mail-Alias erleichtert es in den folgenden Schritten.

---

## <a name="require-https"></a>Anforderung von HTTPS

Finden Sie unter [verbindliche Verwendung von HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>E-Mail-Bestätigung erforderlich

Es ist eine bewährte Methode, die e-Mail-Adresse einer neuen benutzerregistrierung zu bestätigen. E-Mail-Bestätigung können, um zu überprüfen, ob sie sind eine andere Person keinen Identitätswechsel (d. h. sie noch nicht registriert mit einer anderen Person für den e-Mail-Adresse). Angenommen, Sie haben ein Diskussionsforum, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com". Ohne e-Mail-Bestätigung "nolivetto@contoso.com" könnte unerwünschte e-Mail-Adresse aus Ihrer app erhalten. Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" und noch nicht bemerkt, dass die falsche Schreibweise von "Yli". Sie wäre nicht kennwortwiederherstellung zu verwenden, da die app die korrekte e-Mail-Adresse besitzt. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots. E-Mail-Bestätigung stellt keinen Schutz vor böswilligen Benutzern viele e-Mail-Konten bereit.

Im Allgemeinen möchten Sie verhindern, dass neue Benutzer keine Daten zu Ihrer Website veröffentlichen, bevor sie eine e-Mail bestätigte haben.

Update `ConfigureServices` eine bestätigte e-Mail-Adresse erforderlich ist:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` verhindert, dass registrierte Benutzer anmelden, bis ihre-e-Mail bestätigt ist.

### <a name="configure-email-provider"></a>Konfigurieren von e-Mail-Anbieter

In diesem Tutorial wird SendGrid zum Senden von e-Mails verwendet. Sie benötigen eine SendGrid-Konto und einen Schlüssel zum Senden von e-Mails. Sie können andere e-Mail-Anbieter verwenden. ASP.NET Core 2.x enthält `System.Net.Mail`, wodurch Sie zum Senden von e-Mail-Adresse aus Ihrer app. Es wird empfohlen, dass Sie über SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden. SMTP ist schwierig, sichere und ordnungsgemäß eingerichtet.

Die [optionsmuster](xref:fundamentals/configuration/options) wird verwendet, um die benutzereinstellungen für Konto und den Schlüssel zugreifen. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

Erstellen Sie eine Klasse, um den Schlüssel für sichere e-Mails abrufen. In diesem Beispiel die `AuthMessageSenderOptions` Klasse wird erstellt, der *Services/AuthMessageSenderOptions.cs* Datei:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](xref:security/app-secrets). Zum Beispiel:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Auf Windows, speichert Secret Manager-Schlüssel/Wert-Paare in einem *secrets.json* Datei die `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` Verzeichnis.

Den Inhalt der *secrets.json* Datei sind nicht verschlüsselt. Die *secrets.json* Datei wird unten gezeigt (die `SendGridKey` Wert entfernt wurde.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurieren Sie starten, um AuthMessageSenderOptions zu verwenden.

Hinzufügen `AuthMessageSenderOptions` zum Dienstcontainer am Ende der `ConfigureServices` -Methode in der die *"Startup.cs"* Datei:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurieren Sie die AuthMessageSender-Klasse

In diesem Tutorial wird gezeigt, wie Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mails über SMTP und andere Mechanismen senden.

Installieren Sie die `SendGrid` NuGet-Paket:

* Von der Befehlszeile aus:

    `dotnet add package SendGrid`

* Geben Sie über die Paket-Manager-Konsole den folgenden Befehl aus:

  `Install-Package SendGrid`

Finden Sie unter [kostenlos erste Schritte mit SendGrid](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto zu registrieren.

#### <a name="configure-sendgrid"></a>Konfigurieren von SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Zum Konfigurieren von SendGrid fügen Sie Code wie in der folgenden *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Fügen Sie Code in *Services/MessageServices.cs* ähnlich der folgenden SendGrid zu konfigurieren:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Konto-Bestätigung und -Wiederherstellung aktivieren

Die Vorlage weist den Code für die Konto-Bestätigung und -Wiederherstellung. Suchen der `OnPostAsync` -Methode in der *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Verhindern Sie, dass neu registrierte Benutzern automatisch durch die Sie die folgende Zeile Kommentierung angemeldet werden:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Die complete-Methode wird mit der geänderten Zeile hervorgehoben dargestellt:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Um kontobestätigung zu aktivieren, entfernen Sie in den folgenden Code:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Hinweis:** Code verhindert, dass einen neu registrierten Benutzer werden automatisch angemeldet, indem Sie die folgende Zeile Kommentierung:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Kennwortwiederherstellung aktivieren, indem Sie die Kommentierung des Codes in die `ForgotPassword` Aktion *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Kommentieren Sie das Formularelement in *Views/Account/ForgotPassword.cshtml*. Möglicherweise möchten Sie entfernen die `<p> For more information on how to enable reset password ... </p>` -Element, das einen Link zu diesem Artikel enthält.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Registrieren und bestätigen Sie die e-Mail-Adresse und die kennwortzurücksetzung

Führen Sie die Web-app, und Testen Sie die kontobestätigung und das Kennwort eine Wiederherstellung durchführen.

* Die app ausführen und einen neuen Benutzer registrieren

  ![Web-Anwendungsansicht Konto registrieren](accconfirm/_static/loginaccconfirm1.png)

* Überprüfen Sie Ihre e-Mail-Adresse für die der Link für die Bestätigung. Finden Sie unter [Debuggen-e-Mail](#debug) sollten Sie die e-Mail nicht erhalten.
* Klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.
* Melden Sie sich mit Ihren e-Mail-Adresse und Ihr Kennwort.
* Melden Sie sich ab.

### <a name="view-the-manage-page"></a>Anzeigen der Seite "verwalten"

Wählen Sie Ihren Benutzernamen im Browser: ![Browserfenster mit Benutzername](accconfirm/_static/un.png)

Möglicherweise müssen Sie die Navigationsleiste, um Benutzername zu erweitern.

![Navigationsleiste](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt. Die **-e-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mail-Adresse wurde bestätigt.

![Seite "verwalten"](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dies wird später in diesem Tutorial erwähnt.
![Seite "verwalten"](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test Zurücksetzen des Kennworts

* Wenn Sie angemeldet sind, wählen Sie **Logout**.
* Wählen Sie die **melden Sie sich bei** verknüpfen, und wählen Sie die **haben Ihr Kennwort vergessen?** Link.
* Geben Sie die e-Mail-Adresse, die Sie verwendet, um das Konto zu registrieren.
* Es wird eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts gesendet. Überprüfen Sie Ihre e-Mail-Adresse ein, und klicken Sie auf den Link zum Zurücksetzen Ihres Kennworts. Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie sich mit Ihrem e-Mail-Adresse und das neue Kennwort anmelden.

<a name="debug"></a>

### <a name="debug-email"></a>Debuggen von e-Mail-Adresse

Wenn Sie e-Mail-Adresse funktioniert nicht:

* Erstellen Sie eine [Konsolen-app zum Senden von e-Mails](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Überprüfen Sie die [-e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.
* Überprüfen Sie Ihren Spam-Ordner.
* Versuchen Sie es einer anderen e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail usw.)
* Versuchen Sie es an andere e-Mail-Konten gesendet.

**Eine bewährte Sicherheitsmethode** besteht darin, **nicht** produktionsgeheimnisse in Test- und entwicklungsumgebungen verwendet. Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-Geheimnisse Anwendungseinstellungen im Azure-Web-App-Portal festlegen. Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.

## <a name="combine-social-and-local-login-accounts"></a>Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung

Um diesen Abschnitt abgeschlossen haben, müssen Sie zuerst einen externer Authentifizierungsanbieter aktivieren. Finden Sie unter [Facebook, Google und externe Anbieter Authentifizierung](xref:security/authentication/social/index).

Sie können lokale als auch social kombinieren, durch Klicken auf Ihre e-Mail-Link. In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird zuerst als eine lokale Anmeldung; erstellt, Sie können jedoch zunächst erstellen Sie das Konto als Anmeldedaten eines sozialen Netzwerks, und fügen Sie eine lokale Anmeldung hinzu.

![Web-Anwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die 0 externe (Anmeldungen per sozialem Netzwerk) mit diesem Konto verknüpft.

![Verwalten von anzeigen](accconfirm/_static/manage.png)

Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen. In der folgenden Abbildung ist die Facebook dem externen Authentifizierungsanbieter:

![Verwalten Sie Ihre Facebook auflisten externer Anmeldungen-Ansicht](accconfirm/_static/fb.png)

Die beiden Konten wurden kombiniert. Sie können mit beiden Konto anmelden. Sie sollten Ihre Benutzer lokale Konten hinzufügen, falls ihre sozialen Authentifizierungsdiensts ausgefallen ist oder eher sie den Zugriff auf ihre Konten sozialer Netzwerke verlieren.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Kontobestätigung zu aktivieren, nachdem Sie einen Standort der Benutzer enthält

Aktivierung kontobestätigung auf einer Website für Benutzer, alle vorhandenen Benutzer sperren. Vorhandene Benutzer werden gesperrt, da es sich bei ihrem Konto bestätigt werden nicht. Um vorhandene Benutzersperre zu umgehen, verwenden Sie einen der folgenden Ansätze:

* Ändern der Datenbank markieren Sie alle vorhandenen Benutzer als bestätigt wird.
* Bestätigen Sie die vorhandenen Benutzer. Z. B. Batch-Send-e-Mails mit Bestätigung Links.
