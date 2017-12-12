---
title: Freigeben von Cookies zwischen Anwendungen
author: rick-anderson
description: "Dieses Dokument wird erläutert, wie der Data Protection Stapel unterstützt die Freigabe von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-apps."
keywords: ASP.NET Core,ASP.NET,cookies,Interop,cookie freigeben
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="ad798-104">Freigeben von Cookies zwischen Anwendungen</span><span class="sxs-lookup"><span data-stu-id="ad798-104">Sharing cookies between applications</span></span>

<span data-ttu-id="ad798-105">Websites bestehen häufig viele einzelne Webanwendungen, alle arbeiten zusammen kuenftige.</span><span class="sxs-lookup"><span data-stu-id="ad798-105">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="ad798-106">Wenn ein Anwendungsentwickler eine gute Single-Sign-on-Erfahrung zu bieten möchte, benötigen sie häufig alle anderen Webanwendungen innerhalb der Website zur Freigabe von Authentifizierungstickets untereinander.</span><span class="sxs-lookup"><span data-stu-id="ad798-106">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="ad798-107">Zur Unterstützung dieses Szenarios kann Data Protection Stapel Katana-Cookieauthentifizierung und ASP.NET Core Cookie Authentifizierungstickets freigeben.</span><span class="sxs-lookup"><span data-stu-id="ad798-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="ad798-108">Freigeben von Authentifizierungscookies zwischen Anwendungen</span><span class="sxs-lookup"><span data-stu-id="ad798-108">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="ad798-109">Authentifizierungscookies zwischen zwei verschiedenen ASP.NET Core nutzen zu können, konfigurieren Sie jede Anwendung, die Cookies wie folgt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="ad798-109">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="ad798-110">In Ihrer Methode konfigurieren, verwenden Sie die CookieAuthenticationOptions datenschutzdiensts für Cookies und die AuthenticationScheme einrichten, entsprechend der ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ad798-110">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="ad798-111">Wenn Sie mit der Identität verwenden:</span><span class="sxs-lookup"><span data-stu-id="ad798-111">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="ad798-112">Wenn Sie Cookies direkt verwenden:</span><span class="sxs-lookup"><span data-stu-id="ad798-112">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="ad798-113">Die `DataProtectionProvider` erfordert die `Microsoft.AspNetCore.DataProtection.Extensions` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="ad798-113">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="ad798-114">Wenn auf diese Weise verwendet wird, sollte DirectoryInfo zu einem Schlüssel, insbesondere für Authentifizierungscookies ereignissitzungspuffer Speicherort verweisen.</span><span class="sxs-lookup"><span data-stu-id="ad798-114">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="ad798-115">Die cookieauthentifizierungsmiddleware wird die explizit bereitgestellte Implementierung verwenden die DataProtectionProvider, jetzt also isoliert von der Datenschutzsystem durch andere Teile der Anwendung verwendet.</span><span class="sxs-lookup"><span data-stu-id="ad798-115">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="ad798-116">Der Anwendungsname wird ignoriert (absichtlich dies der Fall ist, da Sie versuchen, auf mehrere Anwendungen freigegeben Nutzlasten zu erhalten).</span><span class="sxs-lookup"><span data-stu-id="ad798-116">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="ad798-117">Sollten Sie erwägen, die DataProtectionProvider zu konfigurieren, sodass Schlüssel im Ruhezustand,, wie in verschlüsselt werden der folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="ad798-117">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="ad798-118">Freigeben von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="ad798-118">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="ad798-119">ASP.NET 4.x-Anwendungen, die Katana-cookieauthentifizierungsmiddleware verwenden, können Authentifizierungscookies generieren, die mit der ASP.NET Core cookieauthentifizierungsmiddleware kompatibel sind konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="ad798-119">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="ad798-120">Dies ermöglicht eine große Site einzelanwendungen schrittweise dabei aber nicht auf eine reibungslosen einmaliges Anmelden auf Erfahrung am gesamten Standort aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ad798-120">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="ad798-121">Sie können feststellen, ob die vorhandene Anwendung durch das Vorhandensein eines Aufrufs von UseCookieAuthentication in Ihrem Projekt Startup.Auth.cs Katana-cookieauthentifizierungsmiddleware verwendet.</span><span class="sxs-lookup"><span data-stu-id="ad798-121">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="ad798-122">ASP.NET 4.x-Webanwendungsprojekte mit Visual Studio 2013 erstellt und später die Katana-cookieauthentifizierungsmiddleware wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="ad798-122">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="ad798-123">Die ASP.NET 4.x-Anwendung muss .NET Framework 4.5.1 abzielen, oder höher, andernfalls die nötigen NuGet-Pakete nicht installieren.</span><span class="sxs-lookup"><span data-stu-id="ad798-123">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="ad798-124">Um Authentifizierungscookies zwischen 4.x ASP.NET-Anwendungen und ASP.NET Core-Anwendungen freizugeben, konfigurieren Sie die ASP.NET Core-Anwendung, wie bereits erwähnt, und klicken Sie dann konfigurieren Sie 4.x ASP.NET-Anwendungen, indem Sie die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="ad798-124">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="ad798-125">Installieren Sie das Paket Microsoft.Owin.Security.Interop in jede der ASP.NET 4.x-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="ad798-125">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="ad798-126">Suchen Sie in Startup.Auth.cs den Aufruf von UseCookieAuthentication, das in der Regel wie folgt aussieht.</span><span class="sxs-lookup"><span data-stu-id="ad798-126">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="ad798-127">Ändern Sie den Aufruf von UseCookieAuthentication wie folgt, das Ändern der CookieName entsprechend den Namen, die verwendet werden, indem Sie die ASP.NET Core cookieauthentifizierungsmiddleware, eine Instanz einer DataProtectionProvider, die an einem Speicherort Schlüsselspeicher initialisiert wurde bereitgestellt und Legen Sie CookieManager auf Interop-ChunkingCookieManager, damit das segmentierungsformat kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="ad798-127">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

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
    <span data-ttu-id="ad798-128">DirectoryInfo muss auf am gleichen Speicherort zu verweisen, die Sie die ASP.NET Core Anwendung verweist und sollte mit den gleichen Einstellungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="ad798-128">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="ad798-129">Der ASP.NET 4.x und ASP.NET Core-Anwendungen sind nun konfiguriert, um Authentifizierungscookies freizugeben.</span><span class="sxs-lookup"><span data-stu-id="ad798-129">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="ad798-130">Sie müssen sicherstellen, dass das Identitätssystem für jede Anwendung in der gleichen Benutzerdatenbank gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ad798-130">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="ad798-131">Andernfalls wird das Identitätssystem Fehler zur Laufzeit generiert, wenn er versucht, die Informationen in das Authentifizierungscookie anhand der Informationen in der Datenbank übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="ad798-131">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
