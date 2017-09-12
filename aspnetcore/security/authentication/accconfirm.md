---
title: "Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core"
author: rick-anderson
description: "Veranschaulicht das Erstellen einer ASP.NET Core-Apps mit der e-Mail-Bestätigung und das Kennwort zurücksetzen."
keywords: "ASP.NET Core, das Kennwort zurückzusetzen, e-Mail-Bestätigung, Sicherheit"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: 2f99a5d3db84c3fd3f7ebcb8bccd9a4b8bc8e2b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Kontobestätigung und kennwortwiederherstellung in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Apps mit e-Mail-Bestätigung und das Kennwort zurücksetzen.

## <a name="create-a-new-aspnet-core-project"></a>Erstellen eines neuen ASP.NET Core-Projekts

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Dieser Schritt gilt für Visual Studio unter Windows. Finden Sie im nächsten Abschnitt CLI-Anweisungen.

Das Lernprogramm ist Visual Studio 2017 Preview 2 oder höher erforderlich.

* Erstellen Sie in Visual Studio eine neue Webanwendungsprojekt.
* Wählen Sie **ASP.NET Core 2.0**. Die folgende Abbildung anzeigen **.NET Core** ausgewählt, aber Sie können auswählen, **.NET Framework**.
* Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.
* Behalten Sie den Standardwert **in app-Store Benutzerkonten**.

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Das Lernprogramm ist Visual Studio 2017 oder höher erforderlich.

* Erstellen Sie in Visual Studio eine neue Webanwendungsprojekt.
* Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>.NET Core CLI projekterstellung für MacOS und Linux

Wenn Sie die CLI oder SQLite verwenden, führen Sie in einem Befehlsfenster Folgendes:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Gibt die Vorlage für die einzelnen Benutzerkonten an.
* Fügen Sie auf Windows, die `-uld` Option. Die `-uld` Option erstellt eine LocalDB-Verbindungszeichenfolge anstelle einer SQLite-Datenbank.
* Führen Sie `new mvc --help` um Hilfe zu diesem Befehl zu erhalten.

## <a name="test-new-user-registration"></a>Testen Sie die Registrierung neuer Benutzer

Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer. Befolgen Sie die Anweisungen zum Ausführen von Entity Framework Core-Migrationen aus. An diesem Punkt wird die ausschließliche Überprüfung auf die e-Mail-Adresse mit der [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) Attribut. Nachdem Sie die Registrierung senden, werden Sie in der app angemeldet. Später in diesem Lernprogramm werden wir dies ändern, damit neue Benutzer anmelden können, bis ihre e-Mails überprüft wurde.

## <a name="view-the-identity-database"></a>Anzeigen der Identity-Datenbank

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Aus der **Ansicht** klicken Sie im Menü **Objekt-Explorer von SQL Server** (SSOX). 
* Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer 13)**. Mit der rechten Maustaste auf **Dbo. AspNetUsers** > **zeigen Daten**:

![Kontextmenü für AspNetUsers-Tabelle in SQL Server-Objekt-Explorer](accconfirm/_static/ssox.png)

Beachten Sie die `EmailConfirmed` Feld ist `False`.

Möglicherweise möchten diese e-Mail-Adresse verwenden erneut im nächsten Schritt, wenn die app eine e-Mail zur kaufbestätigung sendet. Mit der rechten Maustaste auf die Zeile, und wählen **löschen**. Löschen die e-Mail-Adresse alias wird nun erleichtert in den folgenden Schritten.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anweisungen zum Anzeigen der SQLite-Datenbank. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Erfordern von SSL und Einrichten von IIS Express für SSL

Finden Sie unter [Erzwingen von SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>E-Mail-Bestätigung

Es wird empfohlen, die e-Mail-Adresse eines eine neue benutzerregistrierung, um zu überprüfen, sind sie nicht eine andere Person Identität zu bestätigen (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse). Angenommen, ein Diskussionsforum mussten, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com." Ohne e-Mail-Bestätigung "nolivetto@contoso.com" könnte unerwünschte e-Mails aus Ihrer app abrufen. Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" hadn't Tippfehler nun der "Yli", wäre sie nicht kennwortwiederherstellung verwenden, da die app ihre korrekte e-Mail-Adresse hat. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und stellt keinen Schutz vor bestimmt Spammern Domänenbenutzern viele funktionierenden e-Mail-Aliase, die sie verwenden können, um zu registrieren.

In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie eine bestätigte e-Mail-Adresse aufweisen. 

Update `ConfigureServices` eine bestätigte e-Mail-Adresse erforderlich ist:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
Die vorangehende Zeile wird verhindert, dass registrierte Benutzern zur Verfügung protokolliert werden, bis ihre-e-Mail bestätigt ist. Allerdings verhindert dieser Zeile nicht neue Benutzer im protokolliert werden, nachdem sie registriert werden. Der Standardcode meldet einen Benutzer auf, nachdem sie registriert werden. Nachdem sie sich abmelden, können sie nicht erneut anmelden, bis sie registriert werden. Später in diesem Lernprogramm ändern wir der Code so neu registrierte Benutzer sind **nicht** angemeldet.

### <a name="configure-email-provider"></a>Konfigurieren von e-Mail-Anbieter

In diesem Lernprogramm wird SendGrid zum Senden von e-Mails. Sie benötigen ein SendGrid-Konto und ein Schlüssel zum Senden von e-Mail. Sie können andere e-Mail-Anbieter verwenden. ASP.NET Core 2.x enthält `System.Net.Mail`, womit Sie das Senden von e-Mails aus Ihrer app. Es wird empfohlen, dass Sie SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden.

Die [Optionen Muster](xref:fundamentals/configuration#options-config-objects) wird verwendet, um die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration#fundamentals-configuration).

Erstellen Sie eine Klasse, um den e-Mail-Schlüssel abzurufen. Für dieses Beispiel die `AuthMessageSenderOptions` Klasse wird erstellt, der *Services/AuthMessageSenderOptions.cs* Datei.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](../app-secrets.md). Zum Beispiel:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Unter Windows, geheimen-Manager speichert die Schlüssel/Wert-Paare in einem *secrets.json* Datei im Verzeichnis %APPDATA%/Microsoft/UserSecrets/ < WebAppName UserSecretsId >.

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

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurieren Sie die AuthMessageSender-Klasse

Dieses Lernprogramm veranschaulicht das Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden.

* Installieren der `SendGrid` NuGet-Paket. Aus der Paket-Manager-Konsole, geben Sie den folgenden den folgenden Befehl aus:

  `Install-Package SendGrid`

* Finden Sie unter [erste Schritte mit SendGrid kostenlos](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto registrieren.

#### <a name="configure-sendgrid"></a>Konfigurieren von SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Fügen Sie Code in *Services/EmailSender.cs* ähnlich der folgenden SendGrid konfigurieren:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Fügen Sie Code in *Services/MessageServices.cs* ähnlich der folgenden SendGrid konfigurieren:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Konto bestätigen und Kennwort Wiederherstellung aktivieren

Die Vorlage, den Code für die Wiederherstellung für Konto bestätigen und das Kennwort hat. Suchen der `[HttpPost] Register` Methode in der *AccountController.cs* Datei.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Verhindern Sie, dass neu registrierte Benutzern zur Verfügung wird automatisch durch die folgende Zeile auskommentiert angemeldet werden:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

Die vollständige Methode ist mit der geänderten Zeile hervorgehoben dargestellt:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kommentieren Sie den Code zum Aktivieren der kontobestätigung.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Hinweis: Wir haben auch einen neu registrierter Benutzer verhindert, dass automatisch durch die folgende Zeile auskommentiert angemeldet sein:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Kennwortwiederherstellung aktivieren, indem Sie den Code in Auskommentieren der `ForgotPassword` Aktion in der *Controllers/AccountController.cs* Datei.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Kommentieren Sie die Form-Elements im *Views/Account/ForgotPassword.cshtml*. Möglicherweise möchten Sie entfernen die `<p> For more information on how to enable reset password ... </p>` Element, das einen Link zu dieser Artikel enthält.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

![Navigationsleiste](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt. Die **E-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mails bestätigt wurde. 

![Seite "verwalten"](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dies wird auf dieser Seite später in diesem Lernprogramm geht.
![Seite "verwalten"](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test Zurücksetzen des Kennworts

* Wenn Sie angemeldet sind, wählen Sie **Logout**.  
* Wählen Sie die **melden Sie sich** verknüpfen, und wählen Sie die **haben Sie Ihr Kennwort vergessen?** Link.
* Geben Sie die e-Mail, die Sie verwendet, um das Konto zu registrieren.
* Eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts werden gesendet. Überprüfen Sie Ihre e-Mail-Adresse, und klicken Sie auf den Link, um Ihr Kennwort zurückzusetzen.  Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie mit Ihrem e-Mail- und das neue Kennwort anmelden.

<a name="debug"></a>

### <a name="debug-email"></a>Debuggen-e-Mail

Wenn Sie e-Mail-nicht funktioniert:

* Überprüfen Sie die [e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.
* Überprüfen Sie Ihre Spam-Ordner.
* Wiederholen Sie dann eine andere e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail, usw.).
* Erstellen einer [Konsolen-app zum Senden von e-Mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Wiederholen Sie den senden an andere e-Mail-Konten.

**Hinweis:** eine bewährte Sicherheitsmethode Produktion geheime Schlüssel in Test- und nicht zu verwenden ist. Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-geheimen als Anwendungseinstellungen im Azure-Web-App-Portal festlegen. Das Konfigurationssystem wird Setup beim Lesen von Schlüsseln von Umgebungsvariablen.

## <a name="prevent-login-at-registration"></a>Zu verhindern, dass die Anmeldung bei der Registrierung

Mit den aktuellen Vorlagen, sobald ein Benutzer das Registrierungsformular abgeschlossen ist. sie angemeldet sind (authentifiziert). In der Regel möchten Sie ihre e-Mails zu überprüfen, bevor im Clientbrowser. Im folgenden Abschnitt werden wir ändern Sie den Code erfordern neue Benutzer haben eine bestätigte e-Mail-Adresse aus, bevor sie angemeldet sind. Update der `[HttpPost] Login` Aktion in der *AccountController.cs* Datei mit den folgenden hervorgehobenen Änderungen.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Hinweis:** eine bewährte Sicherheitsmethode Produktion geheime Schlüssel in Test- und nicht zu verwenden ist. Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-geheimen als Anwendungseinstellungen im Azure-Web-App-Portal festlegen. Das Konfigurationssystem wird Setup beim Lesen von Schlüsseln von Umgebungsvariablen.


## <a name="combine-social-and-local-login-accounts"></a>Kombinieren von sozialen und lokalen Anmeldekonten

Hinweis: Dieser Abschnitt gilt nur für ASP.NET Core 1.x. Für ASP.NET Core 2.x, finden Sie unter [dies](https://github.com/aspnet/Docs/issues/3753) Problem.

Um in diesem Abschnitt abzuschließen, müssen Sie zunächst ein externen Authentifizierungsanbieters aktivieren. Finden Sie unter [Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern](social/index.md).

Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden. In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird als eine lokale Anmeldung; zuerst erstellt jedoch zunächst erstellen Sie das Konto als sozialen Anmeldung werden können, und fügen Sie eine lokale Anmeldung hinzu.

![-Webanwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

Klicken Sie auf die **verwalten** Link. Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.

![Verwalten von anzeigen](accconfirm/_static/manage.png)

Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen. In der folgenden Abbildung wird die Facebook dem externen Authentifizierungsanbieter:

![Verwalten Sie externer Anmeldungen Ansicht Facebook auflisten](accconfirm/_static/fb.png)

Die beiden Konten wurden kombiniert. Sie werden können mit entweder Konto anmelden. Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls der Anmeldung sozialen Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.
