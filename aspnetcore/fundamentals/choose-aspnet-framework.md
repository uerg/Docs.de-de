---
title: Wählen zwischen ASP.NET 4.x und ASP.NET Core
author: rick-anderson
description: Erklärt ASP.NET Core vs. ASP.NET 4.x und wie man sich für eines von beiden entscheidet.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: fa281a068dfdcd8a7c45dd5d0a0c3084c4bc8dbc
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549071"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>Wählen zwischen ASP.NET 4.x und ASP.NET Core

ASP.NET Core ist eine Neugestaltung von ASP.NET 4.x. Dieser Artikel listet die Unterschiede auf.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core ist ein plattformübergreifendes Open-Source-Framework zum Erstellen moderner, cloudbasierter Web-Apps unter Windows, macOS oder Linux.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x ist ein ausgereiftes Framework, das sämtliche Dienste bietet, die zum Erstellen erstklassiger serverbasierter Web-Apps unter Windows für Unternehmen erforderlich sind.

## <a name="framework-selection"></a>Auswahl des Frameworks

Die folgende Tabelle vergleicht ASP.NET Core mit ASP.NET 4.x.

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|Entwickeln für Windows, macOS oder Linux|Entwickeln für Windows|
|[Razor-Seiten](xref:razor-pages/index) werden für das Erstellen einer Webbenutzeroberfläche mit ASP.NET Core 2.x empfohlen. Weitere Informationen finden Sie unter [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) und [SignalR](xref:signalr/introduction).|Verwenden von [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/) oder [WebHooks](/aspnet/webhooks/) oder [Web Pages](/aspnet/web-pages)|
|Mehrere Versionen pro Computer|Eine Version pro Computer|
|Entwickeln mit Visual Studio, [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) oder [Visual Studio Code](https://code.visualstudio.com/) unter Verwendung von C# oder F#|Entwickeln mit Visual Studio unter Verwendung von C#, VB oder F#|
|Höhere Leistung als ASP.NET 4.x|Gute Leistung|
|[Wählen der .NET Framework- oder .NET Core-Laufzeit](/dotnet/articles/standard/choosing-core-framework-server)|Verwenden der .NET Framework-Laufzeit|

Weitere Informationen über Unterstützung für ASP.NET Core 2.x in .NET Framework finden Sie unter [ASP.NET Core targeting .NET Framework](xref:index#target-framework).

## <a name="aspnet-core-scenarios"></a>ASP.NET Core-Szenarien

* [Razor-Seiten](xref:razor-pages/index) werden für das Erstellen einer Webbenutzeroberfläche mit ASP.NET Core 2.x empfohlen.
* [Websites](xref:tutorials/first-mvc-app/index)
* [APIs](xref:tutorials/first-web-api)
* [Echtzeit](xref:signalr/index)
* [Bereitstellen einer ASP.NET Core-App in Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>ASP.NET 4.x-Szenarios

* [Websites](/aspnet/mvc)
* [APIs](/aspnet/web-api)
* [Echtzeit](/aspnet/signalr)
* [Erstellen einer ASP.NET 4.x-Web-App in Azure](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Einführung in ASP.NET](/aspnet/overview)
* [Einführung in ASP.NET Core](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.  Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).