---
title: "Einführung in ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: d8516643393cdf9175adc78ab98420dc1dc5d1d9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-aspnet-core"></a>Einführung in ASP.NET Core

Von [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core ist ein plattformübergreifendes, leistungsstarkes [Open-Source](https://github.com/aspnet/home)-Framework zum Erstellen moderner, cloudbasierter mit dem Internet verbundener Anwendungen. ASP.NET Core ermöglicht Folgendes:

* Erstellen von Web-Apps und -diensten, IoT-Apps und mobilen Back-Ends
* Verwenden Ihrer bevorzugten Entwicklungstools unter Windows, macOS und Linux
* Bereitstellen in die Cloud oder im lokalen System
* Ausführen in [.NET Core oder .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)

## <a name="why-use-aspnet-core"></a>Gründe für ASP.NET Core

Millionen von Entwicklern setzen bei der Erstellung von Web-Apps auf ASP.NET. Bei ASP.NET Core handelt es sich um eine Neugestaltung von ASP.NET mit Änderungen an der Architektur, die ein schlankeres und modulares Framework ergeben.

ASP.NET Core bietet die folgenden Vorteile:

* Eine einheitliche Umgebung zum Erstellen der Webbenutzeroberfläche und von Web-APIs
* Integration von [modernen clientseitigen Frameworks](xref:client-side/index) und Entwicklungsworkflows
* Ein cloudfähiges auf der Umgebung basierendes [Konfigurationssystem](xref:fundamentals/configuration)
* Integrierte [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection)
* Eine schlanke, leistungsstarke und modulare HTTP-Anforderungspipeline
* Möglichkeit des Hostens in IIS oder eigenständigen Hostens in Ihrem eigenen Prozess
* Möglichkeit der Ausführung in [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), wodurch eine echte Versionsverwaltung paralleler Apps unterstützt wird
* Tools zum Vereinfachen einer modernen Webentwicklung
* Fähigkeit zur Erstellung und Ausführung unter Windows, macOS und Linux
* Open Source und mit Fokus auf der Community

ASP.NET Core besteht vollständig aus [NuGet](https://www.nuget.org/)-Paketen. Dadurch können Sie Ihre App so optimieren, dass nur die benötigten NuGet-Pakete enthalten sind. Die Vorteile eines kleineren App-Oberflächenbereichs umfassen straffere Sicherheit, verringerte Wartungsarbeiten und verbesserte Leistung.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Erstellen von Web-APIs und Webbenutzeroberflächen mithilfe von ASP.NET Core MVC

ASP.NET Core MVC bietet Funktionen zum einfacheren Erstellen von [Web-APIs](xref:tutorials/index#building-web-apis) und [Web-Apps](xref:tutorials/index#building-web-applications):

* Das Muster [Model-View-Controller (MVC)](xref:mvc/overview) sorgt dafür, dass Ihre Web-APIs und Web-Apps [testfähig](testing/index.md) sind.
* [Razor-Seiten](xref:mvc/razor-pages/index) (neu in 2.0) sind ein seitenbasiertes Programmiermodell, mit dem das Erstellen einer Webbenutzeroberfläche einfacher und produktiver wird.
* Die [Razor-Syntax](xref:mvc/views/razor) bietet eine produktive Sprache für [Razor-Seiten](xref:mvc/razor-pages/index) und [MVC-Ansichten](xref:mvc/views/overview).
* [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) ermöglichen serverseitigem Code das Mitwirken am Erstellen und Rendern von HTML-Elementen in Razor-Dateien.
* Die integrierte Unterstützung für [mehrere Datenformate und Inhaltsaushandlung](mvc/models/formatting.md) ermöglicht Ihren Web-APIs das Erreichen einer breiten Palette von Clients, wie z.B. Browser und Mobilgeräte.
* Die [Modellbindung](xref:mvc/models/model-binding) ordnet Daten aus HTTP-Anforderungen automatisch Aktionsmethodenparametern zu.
* Die [Modellvalidierung](xref:mvc/models/validation) führt automatisch eine client- und serverseitige Validierung aus.

## <a name="client-side-development"></a>Clientseitige Entwicklung

ASP.NET Core ist auf eine nahtlose Integration mit einer Vielzahl clientseitiger Frameworks wie z.B. [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) und [Bootstrap](xref:client-side/bootstrap) ausgelegt. Weitere Details finden Sie unter [Clientseitige Entwicklung](client-side/index.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [ASP.NET Core-Tutorials](xref:tutorials/index)
* [ASP.NET Core – Grundlagen](xref:fundamentals/index)
* Im wöchentlichen [ASP.NET Community Standup](https://live.asp.net/) werden die Fortschritte und Pläne des Teams behandelt sowie neue Blogs und Software von Drittanbietern vorgestellt.
