---
title: Google externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht die Integration von Google-Konto der Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 4bb5a36cf654deb694d60da126fa42baf382f729
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735764"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Google externe Anmeldung Setup in ASP.NET Core

Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial erfahren Sie, wie Sie Benutzern die Anmeldung mit ihrer Google +-Kontos mit einem ASP.NET Core 2.0-Beispielprojekt auf erstellt ermöglichen, sich die [Vorgängerseite](xref:security/authentication/social/index). Befolgen Sie zunächst die [offizielle Schritte](https://developers.google.com/identity/sign-in/web/devconsole-project) zum Erstellen einer neuen app im Google-API-Konsole.

## <a name="create-the-app-in-google-api-console"></a>Erstellen einer app in Google-API-Konsole

* Navigieren Sie zu [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) und melden Sie sich. Wenn Sie bereits über ein Google-Konto haben, verwenden Sie **Weitere Optionen** > **[Konto erstellen](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  Link zu erstellen:

![Google-API-Konsole](index/_static/GoogleConsoleLogin.png)

* Sie werden zur **-API-Manager-Bibliothek** Seite:

![Angebotsseite auf der Seite für die API-Manager-Bibliothek](index/_static/GoogleConsoleSwitchboard.png)

* Tippen Sie auf **erstellen** , und geben Sie Ihre **Projektname**:

![Dialogfeld "Neues Projekt"](index/_static/GoogleConsoleNewProj.png)

* Nach dem Akzeptieren des Dialogfelds werden Sie an der Library-Seite können Sie auf die Funktionen für Ihre neue app weitergeleitet. Suchen **Google + API** in der Liste und klicken Sie auf den zugehörigen Link, um die API-Funktion hinzufügen:

![Suchen Sie nach "Google +-API" auf der Seite für die API-Manager-Bibliothek](index/_static/GoogleConsoleChooseApi.png)

* Die Seite für die neu hinzugefügte-API wird angezeigt. Tippen Sie auf **aktivieren** um Google +-Anmeldung in der Funktion Ihrer app hinzufügen:

![Angebotsseite auf der Seite für die API-Manager Google +-API](index/_static/GoogleConsoleEnableApi.png)

* Nach der Aktivierung der API, tippen Sie auf **Anmeldeinformationen erstellen** so konfigurieren Sie die geheimen Schlüssel:

![Erstellen Sie Schaltfläche "Anmeldeinformationen" auf API-Manager Google +-API-Seite](index/_static/GoogleConsoleGoCredentials.png)

* Wählen Sie Folgendes aus:
  * **Google + API**
  * **Webserver (z. B. node.js, Tomcat)**, und
  * **Benutzerdaten**:

![Seite für die API-Manager-Anmeldeinformationen: Erfahren Sie, welche Art von Anmeldeinformationen benötigen Bereich](index/_static/GoogleConsoleChooseCred.png)

* Tippen Sie auf **welche Anmeldeinformationen benötige ich?** die gelangen Sie zum zweiten Schritt des app-Konfiguration, **erstellen Sie eine OAuth 2.0-Client-ID**:

![Seite für die API-Manager-Anmeldeinformationen: Erstellen Sie eine OAuth 2.0-Client-ID](index/_static/GoogleConsoleCreateClient.png)

* Da wir ein Google +-Projekt, mit nur einem Merkmal (Anmelden) erstellen, können wir geben Sie den gleichen **Namen** für die OAuth 2.0-Client-ID, das wir für das Projekt verwendet.

* Geben Sie Ihre Entwicklung URI mit `/signin-google` angefügt, die in der **autorisierte weiterleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-google`). Die Google-Authentifizierung konfiguriert, die später in diesem Tutorial behandelt die Anforderungen an automatisch `/signin-google` Route zum Implementieren von OAuth-Fluss.

> [!NOTE]
> Der URI-Segment `/signin-google` als den standardrückruf des Google-Authentifizierungsanbieter festgelegt ist. Sie können den standardrückruf-URI ändern, während der Konfiguration die Middleware für die Google-Authentifizierung über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft der [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) Klasse.

* Tabstopp drücken, um das Hinzufügen der **autorisierte weiterleitungs-URIs** Eintrag.

* Tippen Sie auf **Client-ID erstellen**, die mit dem dritten Schritt gelangen Sie **richten Sie die OAuth 2.0-Zustimmungsdialogfeld**:

![Seite für die API-Manager-Anmeldeinformationen: Einrichten der OAuth 2.0-zustimmungsbildschirm](index/_static/GoogleConsoleAddCred.png)

* Geben Sie Ihre öffentlich **e-Mail-Adresse** und **Produktname** für Ihre app angezeigt wird, wenn es sich bei Google + der Benutzer zur Anmeldung aufgefordert. Weitere Optionen sind verfügbar unter **Weitere Anpassungsoptionen**.

* Tippen Sie auf **Weiter** bis zum letzten Schritt fortfahren **Anmeldeinformationen herunterladen**:

![Seite für die API-Manager-Anmeldeinformationen: Anmeldeinformationen herunterladen](index/_static/GoogleConsoleFinish.png)

* Tippen Sie auf **herunterladen** um eine JSON-Datei mit Geheimnissen aus Anwendungen, zu speichern und **Fertig** um die Erstellung der neuen app abzuschließen.

* Bei der Bereitstellung der Website müssen Sie noch einmal die **Webkonsole von Google** und registrieren Sie eine neue öffentliche Url.

## <a name="store-google-clientid-and-clientsecret"></a>Store-Google-ClientID und ClientSecret

Verknüpfen Sie sensible Einstellungen wie Google `Client ID` und `Client Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [Secret Manager](xref:security/app-secrets). Benennen Sie die Token für die Zwecke dieses Lernprogramms, `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret`.

Die Werte für diese Token finden Sie in der JSON-Datei heruntergeladen, die im vorherigen Schritt unter `web.client_id` und `web.client_secret`.

## <a name="configure-google-authentication"></a>Konfigurieren der Google-Authentifizierung

::: moniker range=">= aspnetcore-2.0"

Fügen Sie den Google-Dienst in der `ConfigureServices` -Methode in der *"Startup.cs"* Datei:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) -Paket ist installiert.

* Um dieses Paket mit Visual Studio 2017 zu installieren, mit der Maustaste, auf das Projekt, und wählen **NuGet-Pakete verwalten**.
* Führen Sie Folgendes in Ihrem Projektverzeichnis, um mit .NET Core-CLI zu installieren:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Fügen Sie die Google-Middleware in der `Configure` -Methode in der *"Startup.cs"* Datei:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

Finden Sie unter den [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Google-Authentifizierung unterstützt. Dies kann verwendet werden, um verschiedene Informationen über den Benutzer anzufordern.

## <a name="sign-in-with-google"></a>Mit Google anmelden

Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich bei**. Eine Option aus, um sich mit Google anmelden wird angezeigt:

![Webanwendung, die in Microsoft Edge ausgeführt wird: Benutzer nicht authentifiziert.](index/_static/DoneGoogle.png)

Wenn Sie Google klicken, werden Sie für die Authentifizierung an Google umgeleitet:

![Dialogfeld "Google-Authentifizierung"](index/_static/GoogleLogin.png)

Nach dem Eingeben Ihrer Google-Anmeldeinformationen an, werden dann Sie zurück zur Website weitergeleitet, in dem Sie Ihre e-Mail-Adresse festlegen können.

Sie sind jetzt angemeldet, sich mit Ihren Google-Anmeldeinformationen:

![Webanwendung, die in Microsoft Edge ausgeführt wird: Benutzerauthentifizierung](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Problembehandlung

* Wenn Sie erhalten eine `403 (Forbidden)` Fehlerseite aus Ihrer eigenen app beim Ausführen im Entwicklungsmodus (oder unterbrechen im Debugger mit dem gleichen Fehler), sicher, dass **Google + API** in aktiviert wurde die **-API-Manager-Bibliothek** durch die folgenden Schritte [weiter oben auf dieser Seite](#create-the-app-in-google-api-console). Wenn die Anmeldung funktioniert nicht, und Sie sind keine Fehler erhalten, wechseln Sie in Development-Modus, um das Problem einfacher Debuggen lässt.
* **ASP.NET Core 2.x nur:** Wenn Identität nicht, durch den Aufruf konfiguriert ist `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: Die Option 'SignInScheme' muss angegeben werden*. Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass dies geschehen ist.
* Wenn die Standortdatenbank nicht erstellt wurde, indem die ursprüngliche Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler. Tippen Sie auf **Migrationen anwenden** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus fortgesetzt.

## <a name="next-steps"></a>Nächste Schritte

* In diesem Artikel wurde gezeigt, wie Sie mit Google authentifiziert werden können. Führen Sie einen ähnlichen Ansatz für die Authentifizierung mit anderen Anbietern aufgeführt, auf die [Vorgängerseite](xref:security/authentication/social/index).

* Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ClientSecret` in der Google-API-Konsole.

* Legen Sie die `Authentication:Google:ClientId` und `Authentication:Google:ClientSecret` Anwendungseinstellungen im Azure-Portal. Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.
