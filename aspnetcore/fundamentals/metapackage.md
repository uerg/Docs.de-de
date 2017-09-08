---
title: "Microsoft.AspNetCore.All Metapackage für ASP.NET Core 2.x und höher"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All Metapackage umfasst alle unterstützten Pakete."
keywords: ASP.NET Core, die NuGet-Paket, Microsoft.AspNetCore.All, Metapackage
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Microsoft.AspNetCore.All Metapackage für ASP.NET Core 2.x

Diese Funktion erfordert ASP.NET Core 2.x.

Die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Metapackage für ASP.NET Core enthält:

* Alle unterstützten Pakete vom Team ASP.NET Core.
* Alle unterstützten Pakete durch das Entity Framework Core. 
* Interne und 3rd Party Abhängigkeiten von ASP.NET Core und Entity Framework Core verwendet wird. 

Alle Funktionen von ASP.NET Core 2.x und Entity Framework Core 2.x befinden sich die `Microsoft.AspNetCore.All` Paket. Die Standard-Projektvorlagen verwenden dieses Paket an.

Die Versionsnummer der `Microsoft.AspNetCore.All` Metapackage darstellt, die ASP.NET Core und Entity Framework Core-Version (mit der Version von .NET Core ausgerichtet).

Anwendungen, die die `Microsoft.AspNetCore.All` Metapackage profitieren Sie automatisch von der .NET Core-Runtime-Speicher. Der Common Language Runtime-Speicher enthält alle Common Language Runtime-Objekte, die zum Ausführen von ASP.NET Core 2.x-Clientanwendungen erforderlich sind. Bei Verwendung der `Microsoft.AspNetCore.All` Metapackage, **keine** Objekte aus der referenzierten ASP.NET Core NuGet-Pakete werden mit der Anwendung bereitgestellt &mdash; .NET Core-Runtime-Store enthält diese Ressourcen. <!-- todo add link to Runtime store -->Die Ressourcen in der Common Language Runtime Speicher vorkompiliert um Anwendungsstartzeit zu verbessern.

Sie können das Trimming gliedert verwenden, so entfernen Sie Pakete, die Sie nicht verwenden. Zugeschnittene Pakete werden in der veröffentlichten Anwendungsausgabe ausgeschlossen.

Die folgenden *csproj* Dateiverweise der `Microsoft.AspNetCore.All` Metapackage für ASP.NET Core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
