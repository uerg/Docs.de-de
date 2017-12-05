---
title: "Einführung in ASP.NET Core"
author: rick-anderson
description: "Bietet eine Einführung in ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="e71b1-104">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e71b1-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="e71b1-105">Von [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="e71b1-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="e71b1-106">ASP.NET Core ist ein plattformübergreifendes, leistungsstarkes [Open-Source](https://github.com/aspnet/home)-Framework zum Erstellen moderner, cloudbasierter mit dem Internet verbundener Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="e71b1-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="e71b1-107">ASP.NET Core ermöglicht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e71b1-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="e71b1-108">Erstellen von Web-Apps und -diensten, [IoT-Apps](https://www.microsoft.com/en-us/internet-of-things/) und mobilen Back-Ends.</span><span class="sxs-lookup"><span data-stu-id="e71b1-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="e71b1-109">Verwenden Ihrer bevorzugten Entwicklungstools unter Windows, macOS und Linux</span><span class="sxs-lookup"><span data-stu-id="e71b1-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e71b1-110">Bereitstellen in die Cloud oder im lokalen System</span><span class="sxs-lookup"><span data-stu-id="e71b1-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="e71b1-111">Ausführen in [.NET Core oder .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)</span><span class="sxs-lookup"><span data-stu-id="e71b1-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="e71b1-112">Gründe für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e71b1-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="e71b1-113">Millionen von Entwicklern setzen bei der Erstellung von Web-Apps auf ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e71b1-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="e71b1-114">Bei ASP.NET Core handelt es sich um eine Neugestaltung von ASP.NET mit Änderungen an der Architektur, die ein schlankeres und modulares Framework ergeben.</span><span class="sxs-lookup"><span data-stu-id="e71b1-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="e71b1-115">ASP.NET Core bietet die folgenden Vorteile:</span><span class="sxs-lookup"><span data-stu-id="e71b1-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="e71b1-116">Eine einheitliche Umgebung zum Erstellen der Webbenutzeroberfläche und von Web-APIs</span><span class="sxs-lookup"><span data-stu-id="e71b1-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="e71b1-117">Integration von [modernen clientseitigen Frameworks](xref:client-side/index) und Entwicklungsworkflows</span><span class="sxs-lookup"><span data-stu-id="e71b1-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="e71b1-118">Ein cloudfähiges auf der Umgebung basierendes [Konfigurationssystem](xref:fundamentals/configuration/index)</span><span class="sxs-lookup"><span data-stu-id="e71b1-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="e71b1-119">Integrierte [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="e71b1-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="e71b1-120">Eine schlanke, leistungsstarke und modulare HTTP-Anforderungspipeline</span><span class="sxs-lookup"><span data-stu-id="e71b1-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="e71b1-121">Möglichkeit des Hostens in IIS oder eigenständigen Hostens in Ihrem eigenen Prozess</span><span class="sxs-lookup"><span data-stu-id="e71b1-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="e71b1-122">Möglichkeit der Ausführung in [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), wodurch eine echte Versionsverwaltung paralleler Apps unterstützt wird</span><span class="sxs-lookup"><span data-stu-id="e71b1-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="e71b1-123">Tools zum Vereinfachen einer modernen Webentwicklung</span><span class="sxs-lookup"><span data-stu-id="e71b1-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="e71b1-124">Fähigkeit zur Erstellung und Ausführung unter Windows, macOS und Linux</span><span class="sxs-lookup"><span data-stu-id="e71b1-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e71b1-125">Open Source und mit Fokus auf der Community</span><span class="sxs-lookup"><span data-stu-id="e71b1-125">Open-source and community-focused.</span></span>

<span data-ttu-id="e71b1-126">ASP.NET Core besteht vollständig aus [NuGet](https://www.nuget.org/)-Paketen.</span><span class="sxs-lookup"><span data-stu-id="e71b1-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="e71b1-127">Dadurch können Sie Ihre App so optimieren, dass nur die benötigten NuGet-Pakete enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="e71b1-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="e71b1-128">Die Vorteile eines kleineren App-Oberflächenbereichs umfassen straffere Sicherheit, verringerte Wartungsarbeiten und verbesserte Leistung.</span><span class="sxs-lookup"><span data-stu-id="e71b1-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="e71b1-129">Erstellen von Web-APIs und Webbenutzeroberflächen mithilfe von ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e71b1-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="e71b1-130">ASP.NET Core MVC bietet Funktionen zum einfacheren Erstellen von [Web-APIs](xref:tutorials/index#building-web-apis) und [Web-Apps](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="e71b1-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="e71b1-131">Das Muster [Model-View-Controller (MVC)](xref:mvc/overview) sorgt dafür, dass Ihre Web-APIs und Web-Apps [testfähig](testing/index.md) sind.</span><span class="sxs-lookup"><span data-stu-id="e71b1-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="e71b1-132">[Razor-Seiten](xref:mvc/razor-pages/index) (neu in 2.0) sind ein seitenbasiertes Programmiermodell, mit dem das Erstellen einer Webbenutzeroberfläche einfacher und produktiver wird.</span><span class="sxs-lookup"><span data-stu-id="e71b1-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="e71b1-133">Die [Razor-Syntax](xref:mvc/views/razor) bietet eine produktive Sprache für [Razor-Seiten](xref:mvc/razor-pages/index) und [MVC-Ansichten](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="e71b1-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="e71b1-134">[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) ermöglichen serverseitigem Code das Mitwirken am Erstellen und Rendern von HTML-Elementen in Razor-Dateien.</span><span class="sxs-lookup"><span data-stu-id="e71b1-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="e71b1-135">Die integrierte Unterstützung für [mehrere Datenformate und Inhaltsaushandlung](mvc/models/formatting.md) ermöglicht Ihren Web-APIs das Erreichen einer breiten Palette von Clients, wie z.B. Browser und Mobilgeräte.</span><span class="sxs-lookup"><span data-stu-id="e71b1-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="e71b1-136">Die [Modellbindung](xref:mvc/models/model-binding) ordnet Daten aus HTTP-Anforderungen automatisch Aktionsmethodenparametern zu.</span><span class="sxs-lookup"><span data-stu-id="e71b1-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="e71b1-137">Die [Modellvalidierung](xref:mvc/models/validation) führt automatisch eine client- und serverseitige Validierung aus.</span><span class="sxs-lookup"><span data-stu-id="e71b1-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="e71b1-138">Clientseitige Entwicklung</span><span class="sxs-lookup"><span data-stu-id="e71b1-138">Client-side development</span></span>

<span data-ttu-id="e71b1-139">ASP.NET Core ist auf eine nahtlose Integration mit einer Vielzahl clientseitiger Frameworks wie z.B. [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) und [Bootstrap](xref:client-side/bootstrap) ausgelegt.</span><span class="sxs-lookup"><span data-stu-id="e71b1-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="e71b1-140">Weitere Details finden Sie unter [Clientseitige Entwicklung](client-side/index.md).</span><span class="sxs-lookup"><span data-stu-id="e71b1-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e71b1-141">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e71b1-141">Next steps</span></span>

<span data-ttu-id="e71b1-142">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="e71b1-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="e71b1-143">ASP.NET Core-Tutorials</span><span class="sxs-lookup"><span data-stu-id="e71b1-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="e71b1-144">ASP.NET Core – Grundlagen</span><span class="sxs-lookup"><span data-stu-id="e71b1-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="e71b1-145">Im wöchentlichen [ASP.NET Community Standup](https://live.asp.net/) werden die Fortschritte und Pläne des Teams behandelt sowie neue Blogs und Software von Drittanbietern vorgestellt.</span><span class="sxs-lookup"><span data-stu-id="e71b1-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
