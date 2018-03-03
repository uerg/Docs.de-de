---
title: Erzwingen von HTTPS in einer ASP.NET Core-app
author: rick-anderson
description: Veranschaulicht, wie HTTPS/TLS in einer ASP.NET Core erfordern Web-app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Erzwingen von HTTPS in einer ASP.NET Core-app

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

- HTTPS für alle Anforderungen erforderlich.
- Umleiten Sie alle HTTP-Anforderungen an HTTPS.

> [!WARNING]
> Führen Sie **nicht** verwenden `RequireHttpsAttribute` auf Web-APIs, die vertraulichen Informationen zu erhalten. `RequireHttpsAttribute` verwendet HTTP-Statuscodes Browsern von HTTP an HTTPS umgeleitet. API-Clients möglicherweise nicht verstehen oder Umleitung von HTTP in HTTPS unterliegen. Diese Clients möglicherweise Informationen über HTTP gesendet werden. Web-APIs sollten entweder:
>
>* Nicht auf HTTP überwacht.
>* Schließen Sie die Verbindung mit dem Statuscode 400 (Ungültige Anforderung), und verarbeiten Sie die Anforderung nicht.

## <a name="require-https"></a>HTTPS erforderlich

Die [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) wird verwendet, um HTTPS erforderlich ist. `[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können. Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert. Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).

Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Anwenden der `[RequireHttps]` Attribut für alle Controller/Razor-Seiten wird nicht so sicher wie die Verwendung von HTTPS global betrachtet. Sie können nicht gewährleisten die `[RequireHttps]` -Attribut angewendet wird, wenn neue Domänencontroller und Razor-Seiten hinzugefügt werden.