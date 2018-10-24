---
title: Einführung in ASP.NET Core
author: rick-anderson
description: Dieser Artikel enthält eine Einführung in ASP.NET Core, ein plattformübergreifendes, leistungsstarkes Open-Source-Framework für das Erstellen moderner, cloudbasierter Anwendungen, die mit dem Internet verbunden sind.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 69ab702e9d9f8d746b7bc546d4f2bbb831ff59c7
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911688"
---
# <a name="introduction-to-aspnet-core"></a>Einführung in ASP.NET Core

Von [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core ist ein plattformübergreifendes, leistungsstarkes [Open-Source](https://github.com/aspnet/home)-Framework zum Erstellen moderner, cloudbasierter mit dem Internet verbundener Anwendungen. ASP.NET Core ermöglicht Folgendes:

* Erstellen von Web-Apps und -diensten, [IoT-Apps](https://www.microsoft.com/internet-of-things/) und mobilen Back-Ends.
* Verwenden Ihrer bevorzugten Entwicklungstools unter Windows, macOS und Linux
* Bereitstellen in der Cloud oder im lokalen System
* Ausführen in [.NET Core oder .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)

## <a name="why-use-aspnet-core"></a>Gründe für ASP.NET Core

Millionen von Entwicklern setzen bei der Erstellung von Web-Apps auf [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview). Bei ASP.NET Core handelt es sich um eine Neugestaltung von ASP.NET 4.x mit Änderungen an der Architektur, die ein schlankeres Framework mit größerer Modularität ergeben.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Erstellen von Web-APIs und Webbenutzeroberflächen mithilfe von ASP.NET Core MVC

ASP.NET Core MVC bietet Funktionen zum Erstellen von [Web-APIs](xref:tutorials/index#build-web-apis) und [Web-Apps](xref:tutorials/index#build-web-apps):

* Das Muster [Model-View-Controller (MVC)](xref:mvc/overview) sorgt dafür, dass Ihre Web-APIs und Web-Apps [testfähig](xref:test/index) sind.
* [Razor Pages](xref:razor-pages/index) (neu in ASP.NET Core 2.0) sind ein seitenbasiertes Programmiermodell, mit dem das Erstellen einer Webbenutzeroberfläche einfacher und produktiver wird.
* Das [Razor-Markup](xref:mvc/views/razor) bietet eine produktive Syntax für [Razor Pages](xref:razor-pages/index) und [MVC-Ansichten](xref:mvc/views/overview).
* [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) ermöglichen serverseitigem Code das Mitwirken am Erstellen und Rendern von HTML-Elementen in Razor-Dateien.
* Die integrierte Unterstützung für [mehrere Datenformate und Inhaltsaushandlung](xref:web-api/advanced/formatting) ermöglicht Ihren Web-APIs das Erreichen einer breiten Palette von Clients, wie z.B. Browser und Mobilgeräte.
* Die [Modellbindung](xref:mvc/models/model-binding) ordnet Daten aus HTTP-Anforderungen automatisch Aktionsmethodenparametern zu.
* Die [Modellvalidierung](xref:mvc/models/validation) führt automatisch eine client- und serverseitige Validierung aus.

## <a name="client-side-development"></a>Clientseitige Entwicklung

ASP.NET Core integriert sich nahtlos in gängige clientseitige Frameworks und Bibliotheken, einschließlich [Angular](xref:spa/angular), [React](xref:spa/react) und [Bootstrap](xref:client-side/bootstrap). Weitere Informationen finden Sie unter [Clientseitige Entwicklung](xref:client-side/index).

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core, das .NET Framework anzielt.

ASP.NET Core kann .NET Core oder .NET Framework anzielen. ASP.NET Core-Apps, die .NET Framework anzielen, sind nicht plattformübergreifend, sondern können nur unter Windows ausgeführt werden. Es ist nicht geplant, den Support für das Anzielen von .NET Framework in ASP.NET Core zu löschen. Allgemein besteht ASP.NET Core aus [.NET Standard](/dotnet/standard/net-standard)-Bibliotheken. Solange .NET Standard 2.0 unterstützt wird, können mit .NET Standard 2.0 geschriebene Apps überall ausgeführt werden.

ASP.NET Core 2.x wird unter .NET Framework-Versionen unterstützt, die mit dem .NET Standard 2.0 kompatibel sind:

* .NET Framework 4.7.1 und später wird dringend empfohlen.
* .NET Framework 4.6.1 und höher.

Das Anzielen auf .NET Core bringt mit jedem Release mehr und mehr Vorteile mit sich. Einige Vorteile von .NET Core gegenüber .NET Framework sind:

* Plattformübergreifend Wird unter macOS, Linux und Windows ausgeführt.
* Leistungssteigerung
* Parallele Versionsverwaltung
* Neue APIs
* Quelle öffnen

Es wird daran gearbeitet, die API-Lücke von .NET Framework zu .NET Core zu schließen. Das [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) stellt Tausende nur unter Windows verfügbare APIs in .NET Core zur Verfügung. Diese APIs waren in .NET Core 1.x nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Erste Schritte mit Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [ASP.NET Core-Tutorials](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [ASP.NET Core – Grundlagen](xref:fundamentals/index)
* Im [wöchentlichen ASP.NET Community Standup](https://live.asp.net/) werden die Fortschritte und Pläne des Teams behandelt. Zudem werden neue Blogs und Drittanbietersoftware vorgestellt.
