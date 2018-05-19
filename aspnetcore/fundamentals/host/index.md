---
title: Hosten in ASP.NET Core
author: guardrex
description: Erfahren Sie mehr über den Webhost von ASP.NET Core und den generischen Host von .NET, die für das Starten von Apps und das Verwalten der Lebensdauer verantwortlich sind.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>Hosten in ASP.NET Core

.NET-Apps konfigurieren und starten einen *Host*. Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer. Es stehen zwei Host-APIs zur Verfügung:

* [Web Host](xref:fundamentals/host/web-host): eignet sich für das Hosten von Web-Apps
* [Generischer Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 oder höher): eignet sich für das Hosten von anderen Apps als Web-Apps (z.B. Apps, die Hintergrundaufgaben ausführen) In einem zukünftigen Release eignet sich der generische Host zum Hosten von jeder Art von Anwendung, einschließlich Web-Apps. Der generische Host wird den Webhost später einmal ersetzen.

Zurzeit sollten Entwickler den [Webhost](xref:fundamentals/host/web-host) basierend auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zum Hosten von ASP.NET Core-Apps verwenden.
