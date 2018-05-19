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
# <a name="host-in-aspnet-core"></a><span data-ttu-id="0a065-103">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a065-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="0a065-104">.NET-Apps konfigurieren und starten einen *Host*.</span><span class="sxs-lookup"><span data-stu-id="0a065-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="0a065-105">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="0a065-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0a065-106">Es stehen zwei Host-APIs zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="0a065-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="0a065-107">[Web Host](xref:fundamentals/host/web-host): eignet sich für das Hosten von Web-Apps</span><span class="sxs-lookup"><span data-stu-id="0a065-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="0a065-108">[Generischer Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 oder höher): eignet sich für das Hosten von anderen Apps als Web-Apps (z.B. Apps, die Hintergrundaufgaben ausführen)</span><span class="sxs-lookup"><span data-stu-id="0a065-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="0a065-109">In einem zukünftigen Release eignet sich der generische Host zum Hosten von jeder Art von Anwendung, einschließlich Web-Apps.</span><span class="sxs-lookup"><span data-stu-id="0a065-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="0a065-110">Der generische Host wird den Webhost später einmal ersetzen.</span><span class="sxs-lookup"><span data-stu-id="0a065-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="0a065-111">Zurzeit sollten Entwickler den [Webhost](xref:fundamentals/host/web-host) basierend auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zum Hosten von ASP.NET Core-Apps verwenden.</span><span class="sxs-lookup"><span data-stu-id="0a065-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
