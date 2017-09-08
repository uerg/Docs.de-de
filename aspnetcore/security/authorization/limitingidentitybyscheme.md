---
title: "Einschränken der Identität nach Schema"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="b01c9-103">Einschränken der Identität nach Schema</span><span class="sxs-lookup"><span data-stu-id="b01c9-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="b01c9-104">In einigen Szenarien ist es z. B. Single Page Applications möglich, mehrere Authentifizierungsmethoden zum Schluss.</span><span class="sxs-lookup"><span data-stu-id="b01c9-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="b01c9-105">Ihre Anwendung kann z. B. Cookie-basierte Authentifizierung, um sich anzumelden und trägerauthentifizierung für JavaScript-Anforderungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="b01c9-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="b01c9-106">In einigen Fällen können Sie mehrere Instanzen der authentifizierungsmiddleware verwenden.</span><span class="sxs-lookup"><span data-stu-id="b01c9-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="b01c9-107">Angenommen, zwei Cookie Middlewares, in denen eine enthält eine grundlegende Identität und einem wird erstellt, wenn ein Multi-Factor Authentication ausgelöst wurde, da der Benutzer einen Vorgang angefordert, der zusätzliche Sicherheit erfordert.</span><span class="sxs-lookup"><span data-stu-id="b01c9-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="b01c9-108">Authentifizierungsschemas werden mit dem Namen, wenn Authentifizierung-Middleware z. B. während der Authentifizierung konfiguriert ist</span><span class="sxs-lookup"><span data-stu-id="b01c9-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

<span data-ttu-id="b01c9-109">In dieser Konfiguration wurden zwei Authentifizierung Middlewares hinzugefügt, eine für Cookies und eine für trägertoken.</span><span class="sxs-lookup"><span data-stu-id="b01c9-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="b01c9-110">Wenn Sie mehrere authentifizierungsmiddleware hinzufügen, sollten Sie sicherstellen, dass keine Middleware für die automatische Ausführung konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="b01c9-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="b01c9-111">Sie dazu die `AutomaticAuthenticate` Optionen-Eigenschaft auf "false".</span><span class="sxs-lookup"><span data-stu-id="b01c9-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="b01c9-112">Diese Filtern nach Schema funktionieren nicht, wenn Sie diese Vorgehensweise nicht einhalten.</span><span class="sxs-lookup"><span data-stu-id="b01c9-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="b01c9-113">Wählen das Schema mit dem Authorize-Attribut</span><span class="sxs-lookup"><span data-stu-id="b01c9-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="b01c9-114">Keine Authentifizierung ist Middleware so konfiguriert, dass automatisch ausgeführt, und erstellen eine Identität, die Sie zum Zeitpunkt der Autorisierung auswählen müssen, welche Middleware verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="b01c9-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="b01c9-115">Die einfachste Möglichkeit, die die Middleware auswählen möchten, dass Sie mit autorisieren ist die Verwendung der `ActiveAuthenticationSchemes` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b01c9-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="b01c9-116">Diese Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste der Authentifizierungsschemas verwenden.</span><span class="sxs-lookup"><span data-stu-id="b01c9-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="b01c9-117">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b01c9-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="b01c9-118">Im Beispiel oben das Cookie und der trägerauthentifizierung wird Middlewares ausgeführt und haben die Möglichkeit, zu erstellen, und fügen eine Identität für den aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="b01c9-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="b01c9-119">Durch Angabe eines einzelnen Schemas wird nur die angegebene Middleware ausgeführt;</span><span class="sxs-lookup"><span data-stu-id="b01c9-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="b01c9-120">In diesem Fall würde nur die Middleware mit der trägerschema ausgeführt und Cookie-basiert Identitäten werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b01c9-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="b01c9-121">Das Schema mit Richtlinien auswählen</span><span class="sxs-lookup"><span data-stu-id="b01c9-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="b01c9-122">Wenn Sie es vorziehen, geben Sie die gewünschten Schemas in [Richtlinie](policies.md#security-authorization-policies-based) können Sie festlegen, die `AuthenticationSchemes` Auflistung bei Ihrer Richtlinie hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b01c9-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="b01c9-123">In diesem Beispiel wird die Over18 Richtlinie wird nur ausgeführt, mit der Identität erstellt, indem die `Bearer` Middleware.</span><span class="sxs-lookup"><span data-stu-id="b01c9-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
