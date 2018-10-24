---
title: Wählen zwischen ASP.NET 4.x und ASP.NET Core
author: rick-anderson
description: Erklärt ASP.NET Core vs. ASP.NET 4.x und wie man sich für eines von beiden entscheidet.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: f046491e2ec68b6beaad581e2b04e6688a81f2d1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911044"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="7bf05-103">Wählen zwischen ASP.NET 4.x und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf05-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="7bf05-104">ASP.NET Core ist eine Neugestaltung von ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="7bf05-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="7bf05-105">Dieser Artikel listet die Unterschiede auf.</span><span class="sxs-lookup"><span data-stu-id="7bf05-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="7bf05-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf05-106">ASP.NET Core</span></span>

<span data-ttu-id="7bf05-107">ASP.NET Core ist ein plattformübergreifendes Open-Source-Framework zum Erstellen moderner, cloudbasierter Web-Apps unter Windows, macOS oder Linux.</span><span class="sxs-lookup"><span data-stu-id="7bf05-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="7bf05-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="7bf05-108">ASP.NET 4.x</span></span>

<span data-ttu-id="7bf05-109">ASP.NET 4.x ist ein ausgereiftes Framework, das sämtliche Dienste bietet, die zum Erstellen erstklassiger serverbasierter Web-Apps unter Windows für Unternehmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="7bf05-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="7bf05-110">Auswahl des Frameworks</span><span class="sxs-lookup"><span data-stu-id="7bf05-110">Framework selection</span></span>

<span data-ttu-id="7bf05-111">Die folgende Tabelle vergleicht ASP.NET Core mit ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="7bf05-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="7bf05-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf05-112">ASP.NET Core</span></span> | <span data-ttu-id="7bf05-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="7bf05-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="7bf05-114">Entwickeln für Windows, macOS oder Linux</span><span class="sxs-lookup"><span data-stu-id="7bf05-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="7bf05-115">Entwickeln für Windows</span><span class="sxs-lookup"><span data-stu-id="7bf05-115">Build for Windows</span></span>|
|<span data-ttu-id="7bf05-116">[Razor-Seiten](xref:razor-pages/index) werden für das Erstellen einer Webbenutzeroberfläche mit ASP.NET Core 2.x empfohlen.</span><span class="sxs-lookup"><span data-stu-id="7bf05-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="7bf05-117">Weitere Informationen finden Sie unter [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) und [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="7bf05-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="7bf05-118">Verwenden von [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/) oder [WebHooks](/aspnet/webhooks/) oder [Web Pages](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="7bf05-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="7bf05-119">Mehrere Versionen pro Computer</span><span class="sxs-lookup"><span data-stu-id="7bf05-119">Multiple versions per machine</span></span>|<span data-ttu-id="7bf05-120">Eine Version pro Computer</span><span class="sxs-lookup"><span data-stu-id="7bf05-120">One version per machine</span></span>|
|<span data-ttu-id="7bf05-121">Entwickeln mit Visual Studio, [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) oder [Visual Studio Code](https://code.visualstudio.com/) unter Verwendung von C# oder F#</span><span class="sxs-lookup"><span data-stu-id="7bf05-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="7bf05-122">Entwickeln mit Visual Studio unter Verwendung von C#, VB oder F#</span><span class="sxs-lookup"><span data-stu-id="7bf05-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="7bf05-123">Höhere Leistung als ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="7bf05-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="7bf05-124">Gute Leistung</span><span class="sxs-lookup"><span data-stu-id="7bf05-124">Good performance</span></span>|
|[<span data-ttu-id="7bf05-125">Wählen der .NET Framework- oder .NET Core-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="7bf05-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="7bf05-126">Verwenden der .NET Framework-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="7bf05-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="7bf05-127">Weitere Informationen über Unterstützung für ASP.NET Core 2.x in .NET Framework finden Sie unter [ASP.NET Core targeting .NET Framework](xref:index#target-framework).</span><span class="sxs-lookup"><span data-stu-id="7bf05-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="7bf05-128">ASP.NET Core-Szenarien</span><span class="sxs-lookup"><span data-stu-id="7bf05-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="7bf05-129">[Razor-Seiten](xref:razor-pages/index) werden für das Erstellen einer Webbenutzeroberfläche mit ASP.NET Core 2.x empfohlen.</span><span class="sxs-lookup"><span data-stu-id="7bf05-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="7bf05-130">Websites</span><span class="sxs-lookup"><span data-stu-id="7bf05-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="7bf05-131">APIs</span><span class="sxs-lookup"><span data-stu-id="7bf05-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="7bf05-132">Echtzeit</span><span class="sxs-lookup"><span data-stu-id="7bf05-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="7bf05-133">Bereitstellen einer ASP.NET Core-App in Azure</span><span class="sxs-lookup"><span data-stu-id="7bf05-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="7bf05-134">ASP.NET 4.x-Szenarios</span><span class="sxs-lookup"><span data-stu-id="7bf05-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="7bf05-135">Websites</span><span class="sxs-lookup"><span data-stu-id="7bf05-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="7bf05-136">APIs</span><span class="sxs-lookup"><span data-stu-id="7bf05-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="7bf05-137">Echtzeit</span><span class="sxs-lookup"><span data-stu-id="7bf05-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="7bf05-138">Erstellen einer ASP.NET 4.x-Web-App in Azure</span><span class="sxs-lookup"><span data-stu-id="7bf05-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="7bf05-139">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7bf05-139">Additional resources</span></span>

* [<span data-ttu-id="7bf05-140">Einführung in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7bf05-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="7bf05-141">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf05-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
