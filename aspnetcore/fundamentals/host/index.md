---
title: Webhost und generischer Host in ASP.NET Core
author: guardrex
description: Erfahren Sie mehr über den Webhost von ASP.NET Core und den generischen Host von .NET, die für das Starten von Apps und das Verwalten der Lebensdauer verantwortlich sind.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284577"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>Webhost und generischer Host in ASP.NET Core

.NET-Apps konfigurieren und starten einen *Host*. Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer. Es stehen zwei Host-APIs zur Verfügung:

* [Web Host](xref:fundamentals/host/web-host): eignet sich für das Hosten von Web-Apps
* [Generischer Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 oder höher): eignet sich für das Hosten von anderen Apps als Web-Apps (z.B. Apps, die Hintergrundaufgaben ausführen) In einem zukünftigen Release eignet sich der generische Host zum Hosten von jeder Art von Anwendung, einschließlich Web-Apps. Der generische Host wird den Webhost später einmal ersetzen.

Um ASP.NET Core-*Web-Apps* zu hosten, sollten Entwickler den auf <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> basierenden Webhost verwenden. Für *alle anderen Apps* sollten Entwickler den generischen, auf <xref:Microsoft.Extensions.Hosting.HostBuilder> basierenden Host verwenden.

<xref:fundamentals/host/hosted-services>  
Erfahren Sie, wie Sie Hintergrundtasks mit gehosteten Diensten in ASP.NET Core implementieren.

<xref:fundamentals/configuration/platform-specific-configuration>  
Erfahren Sie, wie Sie eine ASP.NET Core-App aus einer Assembly mit Verweisen oder ohne Verweise mithilfe einer Implementierung von <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> erweitern.
