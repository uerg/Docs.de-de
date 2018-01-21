---
title: Freigeben von Cookies zwischen Anwendungen
author: rick-anderson
description: "Dieses Dokument wird erläutert, wie der Data Protection Stapel unterstützt die Freigabe von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-apps."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 0cbf5a3e9dfe8f99433800ac5c10ed36b4de6527
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="sharing-cookies-between-applications"></a>Freigeben von Cookies zwischen Anwendungen

Websites bestehen häufig viele einzelne Webanwendungen, alle arbeiten zusammen kuenftige. Wenn ein Anwendungsentwickler eine gute Single-Sign-on-Erfahrung zu bieten möchte, benötigen sie häufig alle anderen Webanwendungen innerhalb der Website zur Freigabe von Authentifizierungstickets untereinander.

Zur Unterstützung dieses Szenarios kann Data Protection Stapel Katana-Cookieauthentifizierung und ASP.NET Core Cookie Authentifizierungstickets freigeben.

## <a name="sharing-authentication-cookies-between-applications"></a>Freigeben von Authentifizierungscookies zwischen Anwendungen

Authentifizierungscookies zwischen zwei verschiedenen ASP.NET Core nutzen zu können, konfigurieren Sie jede Anwendung, die Cookies wie folgt verwendet werden soll.

In Ihrer Methode konfigurieren, verwenden Sie die CookieAuthenticationOptions datenschutzdiensts für Cookies und die AuthenticationScheme einrichten, entsprechend der ASP.NET 4.x.

Wenn Sie mit der Identität verwenden:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Wenn Sie Cookies direkt verwenden:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
Die `DataProtectionProvider` erfordert die `Microsoft.AspNetCore.DataProtection.Extensions` NuGet-Paket.

Wenn auf diese Weise verwendet wird, sollte DirectoryInfo zu einem Schlüssel, insbesondere für Authentifizierungscookies ereignissitzungspuffer Speicherort verweisen. Die cookieauthentifizierungsmiddleware wird die explizit bereitgestellte Implementierung verwenden die DataProtectionProvider, jetzt also isoliert von der Datenschutzsystem durch andere Teile der Anwendung verwendet. Der Anwendungsname wird ignoriert (absichtlich dies der Fall ist, da Sie versuchen, auf mehrere Anwendungen freigegeben Nutzlasten zu erhalten).

>[!WARNING]
>Sollten Sie erwägen, die DataProtectionProvider zu konfigurieren, sodass Schlüssel im Ruhezustand,, wie in verschlüsselt werden der folgenden Beispiel.
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Freigeben von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-Anwendungen

ASP.NET 4.x-Anwendungen, die Katana-cookieauthentifizierungsmiddleware verwenden, können Authentifizierungscookies generieren, die mit der ASP.NET Core cookieauthentifizierungsmiddleware kompatibel sind konfiguriert werden. Dies ermöglicht eine große Site einzelanwendungen schrittweise dabei aber nicht auf eine reibungslosen einmaliges Anmelden auf Erfahrung am gesamten Standort aktualisieren.

>[!TIP]
> Sie können feststellen, ob die vorhandene Anwendung durch das Vorhandensein eines Aufrufs von UseCookieAuthentication in Ihrem Projekt Startup.Auth.cs Katana-cookieauthentifizierungsmiddleware verwendet. ASP.NET 4.x-Webanwendungsprojekte mit Visual Studio 2013 erstellt und später die Katana-cookieauthentifizierungsmiddleware wird standardmäßig verwendet.

> [!NOTE]
> Die ASP.NET 4.x-Anwendung muss .NET Framework 4.5.1 abzielen, oder höher, andernfalls die nötigen NuGet-Pakete nicht installieren.

Um Authentifizierungscookies zwischen 4.x ASP.NET-Anwendungen und ASP.NET Core-Anwendungen freizugeben, konfigurieren Sie die ASP.NET Core-Anwendung, wie bereits erwähnt, und klicken Sie dann konfigurieren Sie 4.x ASP.NET-Anwendungen, indem Sie die folgenden Schritte aus.

1.  Installieren Sie das Paket Microsoft.Owin.Security.Interop in jede der ASP.NET 4.x-Anwendungen.

2.   Suchen Sie in Startup.Auth.cs den Aufruf von UseCookieAuthentication, das in der Regel wie folgt aussieht.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Ändern Sie den Aufruf von UseCookieAuthentication wie folgt, das Ändern der CookieName entsprechend den Namen, die verwendet werden, indem Sie die ASP.NET Core cookieauthentifizierungsmiddleware, eine Instanz einer DataProtectionProvider, die an einem Speicherort Schlüsselspeicher initialisiert wurde bereitgestellt und Legen Sie CookieManager auf Interop-ChunkingCookieManager, damit das segmentierungsformat kompatibel ist.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    DirectoryInfo muss auf am gleichen Speicherort zu verweisen, die Sie die ASP.NET Core Anwendung verweist und sollte mit den gleichen Einstellungen konfiguriert werden.

Der ASP.NET 4.x und ASP.NET Core-Anwendungen sind nun konfiguriert, um Authentifizierungscookies freizugeben.

> [!NOTE]
> Sie müssen sicherstellen, dass das Identitätssystem für jede Anwendung in der gleichen Benutzerdatenbank gezeigt wird. Andernfalls wird das Identitätssystem Fehler zur Laufzeit generiert, wenn er versucht, die Informationen in das Authentifizierungscookie anhand der Informationen in der Datenbank übereinstimmen.
