---
title: Twitter-Einrichtung der externen Anmeldung mit ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht die Integration von Twitter-Konto der Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735803"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Twitter-Einrichtung der externen Anmeldung mit ASP.NET Core

Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial erfahren Sie, wie Sie es Benutzern ermöglichen [melden Sie sich mit ihrem Twitter-Konto](https://dev.twitter.com/web/sign-in/desktop-browser) erstellt mit einem ASP.NET Core 2.0-Beispielprojekt die [Vorgängerseite](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Erstellen einer app in Twitter

* Navigieren Sie zu [ https://apps.twitter.com/ ](https://apps.twitter.com/) und melden Sie sich. Wenn Sie noch kein Twitter-Konto besitzen, verwenden Sie die **[registrieren Sie sich jetzt](https://twitter.com/signup)** Link zu erstellen. Nach der Anmeldung die **Anwendungsverwaltung** Seite wird angezeigt:

  ![Twitter Application Management in Microsoft Edge öffnen](index/_static/TwitterAppManage.png)

* Tippen Sie auf **Create New App** , und füllen Sie die Anwendung **Namen**, **Beschreibung** und öffentliche **Website** URI (Dies kann temporär sein bis Registrieren Sie den Domänennamen):

  ![Erstellen einer Anwendungsseite](index/_static/TwitterCreate.png)

* Geben Sie Ihre Entwicklung URI mit `/signin-twitter` angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-twitter`). Das Twitter-Authentifizierungsschema, das später in diesem Tutorial konfiguriert automatisch behandelt Anforderungen an `/signin-twitter` Route zum Implementieren von OAuth-Fluss.

  > [!NOTE]
  > Der URI-Segment `/signin-twitter` ist als der standardrückruf des Authentifizierungsanbieters Twitter festgelegt. Sie können den standardrückruf-URI ändern, während der Konfiguration der Twitter-Authentifizierung-Middleware über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft der [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) -Klasse.

* Geben Sie im Rest des Formulars, und tippen Sie auf **Erstellen Ihrer Twitter-Anwendung**. Die Anwendungsdetails der neuen werden angezeigt:

  ![Registerkarte "Details" auf der Seite "Anwendung"](index/_static/TwitterAppDetails.png)

* Bei der Bereitstellung der Website müssen Sie noch einmal die **Anwendungsverwaltung** Seite, und registrieren Sie einen neuen öffentlichen URI.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Das Speichern von Twitter-Consumerkey- und ConsumerSecret

Verknüpfen Sie sensible Einstellungen wie Twitter `Consumer Key` und `Consumer Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [Secret Manager](xref:security/app-secrets). Benennen Sie die Token für die Zwecke dieses Lernprogramms, `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret`.

Diese Token finden Sie auf die **Keys and Access Tokens** Registerkarte nach der Erstellung Ihrer neuen Twitter-Anwendung:

![Registerkarte "Schlüssel und Zugriffstoken"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Konfigurieren der Twitter-Authentifizierung

Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) Paket ist bereits installiert.

* Um dieses Paket mit Visual Studio 2017 zu installieren, mit der Maustaste, auf das Projekt, und wählen **NuGet-Pakete verwalten**.
* Führen Sie Folgendes in Ihrem Projektverzeichnis, um mit .NET Core-CLI zu installieren:

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Fügen Sie den Twitter-Dienst in der `ConfigureServices` -Methode in der *"Startup.cs"* Datei:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Hinzufügen die Twitter-Middleware in der `Configure` -Methode in der *"Startup.cs"* Datei:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

Finden Sie unter den [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Twitter-Authentifizierung unterstützt. Dies kann verwendet werden, um verschiedene Informationen über den Benutzer anzufordern.

## <a name="sign-in-with-twitter"></a>Melden Sie sich mit Twitter

Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich bei**. Eine Option aus, um sich mit Twitter anmelden wird angezeigt:

![-Webanwendung: Benutzer nicht authentifiziert.](index/_static/DoneTwitter.png)

Durch Klicken auf **Twitter** an Twitter umgeleitet wird, für die Authentifizierung:

![Twitter-Authentifizierungsseite](index/_static/TwitterLogin.png)

Geben Sie Ihre Twitter-Anmeldeinformationen, werden Sie zurück zur Website weitergeleitet, in dem Sie Ihre e-Mail-Adresse festlegen können.

Sie sind jetzt angemeldet, sich mit Ihren Twitter-Anmeldeinformationen:

![-Webanwendung: Benutzerauthentifizierung](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Problembehandlung

* **ASP.NET Core 2.x nur:** Wenn Identität nicht, durch den Aufruf konfiguriert ist `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: Die Option 'SignInScheme' muss angegeben werden*. Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass dies geschehen ist.
* Wenn die Standortdatenbank nicht erstellt wurde, indem die ursprüngliche Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler. Tippen Sie auf **Migrationen anwenden** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus fortgesetzt.

## <a name="next-steps"></a>Nächste Schritte

* In diesem Artikel wurde gezeigt, wie Sie Twitter authentifizieren können. Führen Sie einen ähnlichen Ansatz für die Authentifizierung mit anderen Anbietern aufgeführt, auf die [Vorgängerseite](xref:security/authentication/social/index).

* Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ConsumerSecret` im Twitter-Entwicklerportal.

* Legen Sie die `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret` Anwendungseinstellungen im Azure-Portal. Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.
