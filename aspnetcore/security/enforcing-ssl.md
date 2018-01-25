---
title: Erzwingen von SSL in einer ASP.NET Core-app
author: rick-anderson
description: Zeigt, wie So fordern Sie SSL in einem Kern der ASP.NET Web-app
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Erzwingen von SSL in einer ASP.NET Core-app

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

- Erfordern Sie SSL für alle Anforderungen (gilt nur für HTTPS-Anforderungen).
- Umleiten Sie alle HTTP-Anforderungen an HTTPS.

## <a name="require-ssl"></a>Anfordern von SSL

Die [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um SSL erforderlich ist. Controllern oder Methoden mit diesem Attribut ergänzen kann, oder Sie können es Global wie folgt anwenden:

Fügen Sie folgenden Code zum `ConfigureServices` in `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

Der hervorgehobene Code oben ist erforderlich, alle Anforderungen verwenden `HTTPS`, daher HTTP-Anforderungen werden ignoriert. Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting) für Weitere Informationen.

Verwendung von HTTPS Global (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Anwenden der `[RequireHttps]` Attribut an alle Controller ist nicht so sicher wie die Verwendung von HTTPS global betrachtet. Sie können nicht gewährleisten neue Domänencontroller hinzugefügt, um Ihre app werden Denken Sie daran, gelten die `[RequireHttps]` Attribut.
