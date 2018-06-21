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
# <a name="host-in-aspnet-core"></a><span data-ttu-id="76bac-103">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76bac-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="76bac-104">.NET-Apps konfigurieren und starten einen *Host*.</span><span class="sxs-lookup"><span data-stu-id="76bac-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="76bac-105">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="76bac-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="76bac-106">Es stehen zwei Host-APIs zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="76bac-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="76bac-107">[Web Host](xref:fundamentals/host/web-host): eignet sich für das Hosten von Web-Apps</span><span class="sxs-lookup"><span data-stu-id="76bac-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="76bac-108">[Generischer Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 oder höher): eignet sich für das Hosten von anderen Apps als Web-Apps (z.B. Apps, die Hintergrundaufgaben ausführen)</span><span class="sxs-lookup"><span data-stu-id="76bac-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="76bac-109">In einem zukünftigen Release eignet sich der generische Host zum Hosten von jeder Art von Anwendung, einschließlich Web-Apps.</span><span class="sxs-lookup"><span data-stu-id="76bac-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="76bac-110">Der generische Host wird den Webhost später einmal ersetzen.</span><span class="sxs-lookup"><span data-stu-id="76bac-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="76bac-111">Um ASP.NET Core-*Web-Apps* zu hosten, sollten Entwickler den auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) basierenden Webhost verwenden.</span><span class="sxs-lookup"><span data-stu-id="76bac-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="76bac-112">Für *alle anderen Apps* sollten Entwickler den generischen, auf [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) basierenden Host verwenden.</span><span class="sxs-lookup"><span data-stu-id="76bac-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
