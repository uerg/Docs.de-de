---
title: Externe Anmeldung Setup mit ASP.NET Core Twitter
author: rick-anderson
description: Dieses Lernprogramm veranschaulicht die Integration von Twitter-Konto-Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f59f7d1bf0280cef8f7757e8cd57d4872769b3d
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688995"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Externe Anmeldung Setup mit ASP.NET Core Twitter

Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Diesem Lernprogramm erfahren Sie, wie Sie Ihren Benutzern ermöglichen [melden Sie sich mit ihren Twitter-Konto](https://dev.twitter.com/web/sign-in/desktop-browser) ein Beispielprojekt für ASP.NET Core 2.0 erstellt wurde, auf die [vorherige Seite](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Erstellen Sie die app in Twitter

* Navigieren Sie zu [ https://apps.twitter.com/ ](https://apps.twitter.com/) und melden Sie sich. Wenn Sie einen Twitter-Konto noch nicht haben, verwenden die **[jetzt registrieren](https://twitter.com/signup)** Link, um eines zu erstellen. Nach der Anmeldung, die **Anwendungsverwaltung** Seite wird angezeigt:

![Twitter Application Management in Microsoft Edge öffnen](index/_static/TwitterAppManage.png)

* Tippen Sie auf **neue App** , und füllen Sie die Anwendung **Namen**, **Beschreibung** und öffentlichen **Website** URI (Dies kann temporär sein bis Registrieren Sie den Domänennamen):

![Erstellen einer Anwendungsseite](index/_static/TwitterCreate.png)

* Geben Sie die Entwicklung URI mit */signin-twitter* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-twitter`). Verarbeiten der Twitter-Authentifizierungsschema, die weiter unten in diesem Lernprogramm konfiguriert automatisch Anforderungen, die bei */signin-twitter* Route zum Implementieren des OAuth-Fluss.

* Füllen Sie den Rest des Formulars, und tippen Sie auf **die Twitter-Anwendung erstellen**. Anwendungsdetails des neuen werden angezeigt:

![Registerkarte "Details" auf der Seite "Anwendung"](index/_static/TwitterAppDetails.png)

* Bei der Bereitstellung der Website müssen Sie rufen die **Anwendungsverwaltung** Seite, und registrieren Sie einen neuen öffentlichen URI.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Speichern von Twitter ConsumerKey und ConsumerSecret

Verknüpfen Sie die sensiblen Einstellungen wie Twitter `Consumer Key` und `Consumer Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](xref:security/app-secrets). Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret`.

Diese Token befinden sich auf die **Schlüssel und Zugriffstoken** Registerkarte nach dem Erstellen einer neuen Twitter-Anwendung:

![Registerkarte "Schlüssel und Zugriffstoken"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Twitter-Authentifizierung konfigurieren

Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) Paket ist bereits installiert.

* Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.
* Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Hinzufügen den Twitter-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Hinzufügen die Twitter-Middleware in der `Configure` Methode im *Startup.cs* Datei:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

Finden Sie unter der [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen von Twitter-Authentifizierung unterstützt. Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.

## <a name="sign-in-with-twitter"></a>Melden Sie sich mit Twitter

Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**. Eine Option zur Anmeldung mit Twitter angezeigt wird:

![-Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneTwitter.png)

Durch Klicken auf **Twitter** leitet an Twitter für die Authentifizierung:

![Twitter-Seite "Authentifizierung"](index/_static/TwitterLogin.png)

Nach der Eingabe der Anmeldeinformationen für Twitter, werden Sie zurück zur Website umgeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.

Sie sind jetzt angemeldet mit Ihren Twitter-Anmeldeinformationen:

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a>Problembehandlung

* **ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*. Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.
* Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler. Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.

## <a name="next-steps"></a>Nächste Schritte

* In diesem Artikel wurde gezeigt, wie Sie mit Twitter authentifizieren können. Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](xref:security/authentication/social/index).

* Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ConsumerSecret` im Entwicklerportal Twitter.

* Legen Sie die `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret` als Anwendungseinstellungen im Azure-Portal. Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.
