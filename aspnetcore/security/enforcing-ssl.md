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
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Erzwingen von HTTPS in einer ASP.NET Core-app

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

- HTTPS für alle Anforderungen erforderlich.
- Alle HTTP-Anforderungen auf HTTPS umleiten.

> [!WARNING]
> Verwenden Sie **nicht** `RequireHttpsAttribute` auf Web-APIs, an welche Sie vertraulichen Informationen senden. `RequireHttpsAttribute`verwendet HTTP-Statuscodes um Browser von HTTP an HTTPS umzuleiten. API-Clients verstehen diese möglicherweise nicht oder führen keine Umleitung von HTTP auf HTTPS durch. Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden. Web-APIs sollten daher entweder:
>
>* Nicht auf das HTTP-Protokoll eingehen.
>* Die Verbindung mit dem Statuscode 400 (Ungültige Anforderung) schließen, und die Anforderung nicht verarbeiten.

## <a name="require-https"></a>HTTPS erforderlich

Die [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) wird verwendet, um HTTPS erforderlich ist. `[RequireHttpsAttribute]`ergänzen können, Controllern oder Methoden oder global angewendet werden können. Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert. Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).

Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Anwenden der `[RequireHttps]` Attribut für alle Controller/Razor-Seiten wird nicht so sicher wie die Verwendung von HTTPS global betrachtet. Sie können nicht gewährleisten die `[RequireHttps]` -Attribut angewendet wird, wenn neue Domänencontroller und Razor-Seiten hinzugefügt werden
