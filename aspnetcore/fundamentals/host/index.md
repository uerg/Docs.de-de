---
title: Hosten in ASP.NET Core
author: guardrex
description: Erfahren Sie mehr über den Webhost von ASP.NET Core und den generischen Host von .NET, die für das Starten von Apps und das Verwalten der Lebensdauer verantwortlich sind.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276616"
---
# <a name="host-in-aspnet-core"></a>Hosten in ASP.NET Core

.NET-Apps konfigurieren und starten einen *Host*. Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer. Es stehen zwei Host-APIs zur Verfügung:

* [Web Host](xref:fundamentals/host/web-host): eignet sich für das Hosten von Web-Apps
* [Generischer Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 oder höher): eignet sich für das Hosten von anderen Apps als Web-Apps (z.B. Apps, die Hintergrundaufgaben ausführen) In einem zukünftigen Release eignet sich der generische Host zum Hosten von jeder Art von Anwendung, einschließlich Web-Apps. Der generische Host wird den Webhost später einmal ersetzen.

Um ASP.NET Core-*Web-Apps* zu hosten, sollten Entwickler den auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) basierenden Webhost verwenden. Für *alle anderen Apps* sollten Entwickler den generischen, auf [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) basierenden Host verwenden.
