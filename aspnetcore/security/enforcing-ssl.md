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
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Erzwingen von HTTPS in einer ASP.NET Core-app

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

- HTTPS für alle Anforderungen erforderlich.
- Alle HTTP-Anforderungen auf HTTPS umleiten.

> [!WARNING]
> Verwenden Sie `RequireHttpsAttribute` **nicht** bei Web-APIs, die vertrauliche Informationen entgegennehmen. `RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter. API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch. Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden. Web-APIs sollten daher entweder:
>
>* nicht auf HTTP lauschen oder
>* die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.

## <a name="require-https"></a>HTTPS erforderlich

Die [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) wird verwendet, um HTTPS erforderlich ist. `[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können. Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert. Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).

Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer. denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.