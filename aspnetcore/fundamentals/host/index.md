---
title: Hosten in ASP.NET Core
author: guardrex
description: Erfahren Sie mehr über den Webhost von ASP.NET Core und den generischen Host von .NET, die für das Starten von Apps und das Verwalten der Lebensdauer verantwortlich sind.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336049"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="a22e1-103">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a22e1-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="a22e1-104">.NET-Apps konfigurieren und starten einen *Host*.</span><span class="sxs-lookup"><span data-stu-id="a22e1-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="a22e1-105">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="a22e1-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a22e1-106">Es stehen zwei Host-APIs zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="a22e1-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="a22e1-107">[Web Host](xref:fundamentals/host/web-host): eignet sich für das Hosten von Web-Apps</span><span class="sxs-lookup"><span data-stu-id="a22e1-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="a22e1-108">[Generischer Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 oder höher): eignet sich für das Hosten von anderen Apps als Web-Apps (z.B. Apps, die Hintergrundaufgaben ausführen)</span><span class="sxs-lookup"><span data-stu-id="a22e1-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="a22e1-109">In einem zukünftigen Release eignet sich der generische Host zum Hosten von jeder Art von Anwendung, einschließlich Web-Apps.</span><span class="sxs-lookup"><span data-stu-id="a22e1-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="a22e1-110">Der generische Host wird den Webhost später einmal ersetzen.</span><span class="sxs-lookup"><span data-stu-id="a22e1-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="a22e1-111">Um ASP.NET Core-*Web-Apps* zu hosten, sollten Entwickler den auf <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> basierenden Webhost verwenden.</span><span class="sxs-lookup"><span data-stu-id="a22e1-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="a22e1-112">Für *alle anderen Apps* sollten Entwickler den generischen, auf <xref:Microsoft.Extensions.Hosting.HostBuilder> basierenden Host verwenden.</span><span class="sxs-lookup"><span data-stu-id="a22e1-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="a22e1-113">Erfahren Sie, wie Sie Hintergrundtasks mit gehosteten Diensten in ASP.NET Core implementieren.</span><span class="sxs-lookup"><span data-stu-id="a22e1-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="a22e1-114">Erfahren Sie, wie Sie eine ASP.NET Core-App aus einer Assembly mit Verweisen oder ohne Verweise mithilfe einer Implementierung von <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> erweitern.</span><span class="sxs-lookup"><span data-stu-id="a22e1-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
