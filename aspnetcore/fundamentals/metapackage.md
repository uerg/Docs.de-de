---
title: Das Metapaket „Microsoft.AspNetCore.All“ für ASP.NET Core 2.0 und höher
author: Rick-Anderson
description: Das Metapaket „Microsoft.AspNetCore.All“ enthält alle unterstützten ASP.NET Core- und Entity Framework Core-Pakete zusammen mit ihren Abhängigkeiten.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: c0d7d7fb5f41a91f8d881dd7880d8adcaa478968
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Das Metapaket „Microsoft.AspNetCore.All“ für ASP.NET Core 2.0

> [!NOTE]
> Es wird empfohlen, für Anwendungen, die ASP.NET Core 2.1 und höher anzielen, das Paket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage) statt diesem Paket zu verwenden. Weitere Informationen finden Sie in diesem Artikel unter [Migrieren von Microsoft.AspNetCore.All zu Microsoft.AspNetCore.App](#migrate).

Für dieses Feature ist ASP.NET Core 2.x für .NET Core 2.x erforderlich.

Das Metapaket [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) für ASP.NET Core enthält:

* alle unterstützten Pakete des ASP.NET Core-Teams
* alle unterstützten Pakete von Entity Framework Core 
* interne und Drittanbieterabhängigkeiten, die von ASP.NET Core und Entity Framework Core verwendet werden 

In dem Paket `Microsoft.AspNetCore.All` sind alle Features von ASP.NET Core 2.x und Entity Framework Core 2.x enthalten. Die Standardprojektvorlagen für ASP.NET Core 2.0 verwenden dieses Paket.

Die Versionsnummer des Metapakets `Microsoft.AspNetCore.All` stellt die ASP.NET Core-Version und die Entity Framework Core-Version dar.

Anwendungen, die das Metapaket `Microsoft.AspNetCore.All` verwenden, profitieren automatisch von dem [.NET Core-Laufzeitspeicher](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Der Laufzeitspeicher enthält alle Laufzeitobjekte, die für die Ausführung von ASP.NET Core 2.x-Anwendungen erforderlich sind. Bei Verwendung des Metapakets `Microsoft.AspNetCore.All` werden **keine** Objekte aus den referenzierten ASP.NET Core NuGet-Paketen mit der Anwendung &mdash; bereitgestellt. Der .NET Core-Laufzeitspeicher enthält diese Objekte. Die Objekte im Laufzeitspeicher sind zur Verbesserung der Startzeit der Anwendung vorkompiliert.

Sie können den Trimmprozess für Pakete verwenden, um nicht verwendete Pakete zu entfernen. Getrimmte Pakete werden aus der veröffentlichten Anwendungsausgabe ausgeschlossen.

Die folgende *.csproj*-Datei verweist auf das Metapaket `Microsoft.AspNetCore.All` für ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migrieren von Microsoft.AspNetCore.All zu Microsoft.AspNetCore.App

Folgende Pakete sind in `Microsoft.AspNetCore.All`, aber nicht in `Microsoft.AspNetCore.App` enthalten. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Wenn Sie von `Microsoft.AspNetCore.All` zu `Microsoft.AspNetCore.App` migrieren möchten und Ihre App APIs aus den oben aufgeführten Paketen verwendet, fügen Sie in Ihrem Projekt Verweise auf diese Pakete hinzu.

Alle Abhängigkeiten der vorangehenden Pakete, die keine Abhängigkeiten von `Microsoft.AspNetCore.App` sind, sind nicht implizit enthalten. Zum Beispiel:

* `StackExchange.Redis` als Abhängigkeit von `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` als Abhängigkeit von `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`