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
# <a name="limiting-identity-by-scheme"></a>Einschränken der Identität nach Schema

<a name=security-authorization-limiting-by-scheme></a>

In einigen Szenarien ist es z. B. Single Page Applications möglich, mehrere Authentifizierungsmethoden zum Schluss. Ihre Anwendung kann z. B. Cookie-basierte Authentifizierung, um sich anzumelden und trägerauthentifizierung für JavaScript-Anforderungen verwenden. In einigen Fällen können Sie mehrere Instanzen der authentifizierungsmiddleware verwenden. Angenommen, zwei Cookie Middlewares, in denen eine enthält eine grundlegende Identität und einem wird erstellt, wenn ein Multi-Factor Authentication ausgelöst wurde, da der Benutzer einen Vorgang angefordert, der zusätzliche Sicherheit erfordert.

Authentifizierungsschemas werden mit dem Namen, wenn Authentifizierung-Middleware z. B. während der Authentifizierung konfiguriert ist

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

In dieser Konfiguration wurden zwei Authentifizierung Middlewares hinzugefügt, eine für Cookies und eine für trägertoken.

>[!NOTE]
>Wenn Sie mehrere authentifizierungsmiddleware hinzufügen, sollten Sie sicherstellen, dass keine Middleware für die automatische Ausführung konfiguriert ist. Sie dazu die `AutomaticAuthenticate` Optionen-Eigenschaft auf "false". Diese Filtern nach Schema funktionieren nicht, wenn Sie diese Vorgehensweise nicht einhalten.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Wählen das Schema mit dem Authorize-Attribut

Keine Authentifizierung ist Middleware so konfiguriert, dass automatisch ausgeführt, und erstellen eine Identität, die Sie zum Zeitpunkt der Autorisierung auswählen müssen, welche Middleware verwendet werden soll. Die einfachste Möglichkeit, die die Middleware auswählen möchten, dass Sie mit autorisieren ist die Verwendung der `ActiveAuthenticationSchemes` Eigenschaft. Diese Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste der Authentifizierungsschemas verwenden. Beispiel:

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

Im Beispiel oben das Cookie und der trägerauthentifizierung wird Middlewares ausgeführt und haben die Möglichkeit, zu erstellen, und fügen eine Identität für den aktuellen Benutzer. Durch Angabe eines einzelnen Schemas wird nur die angegebene Middleware ausgeführt;

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

In diesem Fall würde nur die Middleware mit der trägerschema ausgeführt und Cookie-basiert Identitäten werden ignoriert.

## <a name="selecting-the-scheme-with-policies"></a>Das Schema mit Richtlinien auswählen

Wenn Sie es vorziehen, geben Sie die gewünschten Schemas in [Richtlinie](policies.md#security-authorization-policies-based) können Sie festlegen, die `AuthenticationSchemes` Auflistung bei Ihrer Richtlinie hinzufügen.

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

In diesem Beispiel wird die Over18 Richtlinie wird nur ausgeführt, mit der Identität erstellt, indem die `Bearer` Middleware.
