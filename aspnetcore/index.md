---
title: Einführung in ASP.NET Core
author: rick-anderson
description: Dieser Artikel enthält eine Einführung in ASP.NET Core, ein plattformübergreifendes, leistungsstarkes Open-Source-Framework für das Erstellen moderner, cloudbasierter Anwendungen, die mit dem Internet verbunden sind.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: e9bca9fe22dbb64086eba3445c50941c5440974c
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253065"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="f0c8e-103">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0c8e-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="f0c8e-104">Von [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="f0c8e-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="f0c8e-105">ASP.NET Core ist ein plattformübergreifendes, leistungsstarkes [Open-Source](https://github.com/aspnet/home)-Framework zum Erstellen moderner, cloudbasierter mit dem Internet verbundener Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="f0c8e-106">ASP.NET Core ermöglicht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f0c8e-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="f0c8e-107">Erstellen von Web-Apps und -diensten, [IoT-Apps](https://www.microsoft.com/internet-of-things/) und mobilen Back-Ends.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="f0c8e-108">Verwenden Ihrer bevorzugten Entwicklungstools unter Windows, macOS und Linux</span><span class="sxs-lookup"><span data-stu-id="f0c8e-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="f0c8e-109">Bereitstellen in der Cloud oder im lokalen System</span><span class="sxs-lookup"><span data-stu-id="f0c8e-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="f0c8e-110">Ausführen in [.NET Core oder .NET Framework](/dotnet/articles/standard/choosing-core-framework-server)</span><span class="sxs-lookup"><span data-stu-id="f0c8e-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="f0c8e-111">Gründe für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0c8e-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="f0c8e-112">Millionen von Entwicklern setzen bei der Erstellung von Web-Apps auf [ASP.NET 4.x](/aspnet/overview).</span><span class="sxs-lookup"><span data-stu-id="f0c8e-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="f0c8e-113">Bei ASP.NET Core handelt es sich um eine Neugestaltung von ASP.NET 4.x mit Änderungen an der Architektur, die ein schlankeres Framework mit größerer Modularität ergeben.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="f0c8e-114">Erstellen von Web-APIs und Webbenutzeroberflächen mithilfe von ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f0c8e-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="f0c8e-115">ASP.NET Core MVC bietet Funktionen zum Erstellen von [Web-APIs](xref:tutorials/first-web-api) und [Web-Apps](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="f0c8e-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="f0c8e-116">Das Muster [Model-View-Controller (MVC)](xref:mvc/overview) sorgt dafür, dass Ihre Web-APIs und Web-Apps testfähig sind.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="f0c8e-117">[Razor Pages](xref:razor-pages/index) (neu in ASP.NET Core 2.0) sind ein seitenbasiertes Programmiermodell, mit dem das Erstellen einer Webbenutzeroberfläche einfacher und produktiver wird.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="f0c8e-118">Das [Razor-Markup](xref:mvc/views/razor) bietet eine produktive Syntax für [Razor Pages](xref:razor-pages/index) und [MVC-Ansichten](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="f0c8e-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="f0c8e-119">[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) ermöglichen serverseitigem Code das Mitwirken am Erstellen und Rendern von HTML-Elementen in Razor-Dateien.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="f0c8e-120">Die integrierte Unterstützung für [mehrere Datenformate und Inhaltsaushandlung](xref:web-api/advanced/formatting) ermöglicht Ihren Web-APIs das Erreichen einer breiten Palette von Clients, wie z.B. Browser und Mobilgeräte.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="f0c8e-121">Die [Modellbindung](xref:mvc/models/model-binding) ordnet Daten aus HTTP-Anforderungen automatisch Aktionsmethodenparametern zu.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="f0c8e-122">Die [Modellvalidierung](xref:mvc/models/validation) führt automatisch eine client- und serverseitige Validierung aus.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="f0c8e-123">Clientseitige Entwicklung</span><span class="sxs-lookup"><span data-stu-id="f0c8e-123">Client-side development</span></span>

<span data-ttu-id="f0c8e-124">ASP.NET Core integriert sich nahtlos in gängige clientseitige Frameworks und Bibliotheken, einschließlich [Angular](xref:spa/angular), [React](xref:spa/react) und [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f0c8e-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="f0c8e-125">Weitere Informationen finden Sie unter [Clientseitige Entwicklung](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="f0c8e-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="f0c8e-126">ASP.NET Core, das .NET Framework anzielt.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="f0c8e-127">ASP.NET Core kann .NET Core oder .NET Framework anzielen.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="f0c8e-128">ASP.NET Core-Apps, die .NET Framework anzielen, sind nicht plattformübergreifend, sondern können nur unter Windows ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="f0c8e-129">Es ist nicht geplant, den Support für das Anzielen von .NET Framework in ASP.NET Core zu löschen.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="f0c8e-130">Allgemein besteht ASP.NET Core aus [.NET Standard](/dotnet/standard/net-standard)-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="f0c8e-131">Solange .NET Standard 2.0 unterstützt wird, können mit .NET Standard 2.0 geschriebene Apps überall ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="f0c8e-132">ASP.NET Core 2.x wird unter .NET Framework-Versionen unterstützt, die mit dem .NET Standard 2.0 kompatibel sind:</span><span class="sxs-lookup"><span data-stu-id="f0c8e-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="f0c8e-133">.NET Framework 4.7.1 und später wird dringend empfohlen.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="f0c8e-134">.NET Framework 4.6.1 und höher.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="f0c8e-135">Das Anzielen auf .NET Core bringt mit jedem Release mehr und mehr Vorteile mit sich.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="f0c8e-136">Einige Vorteile von .NET Core gegenüber .NET Framework sind:</span><span class="sxs-lookup"><span data-stu-id="f0c8e-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="f0c8e-137">Plattformübergreifend</span><span class="sxs-lookup"><span data-stu-id="f0c8e-137">Cross-platform.</span></span> <span data-ttu-id="f0c8e-138">Wird unter macOS, Linux und Windows ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="f0c8e-139">Leistungssteigerung</span><span class="sxs-lookup"><span data-stu-id="f0c8e-139">Improved performance</span></span>
* <span data-ttu-id="f0c8e-140">Parallele Versionsverwaltung</span><span class="sxs-lookup"><span data-stu-id="f0c8e-140">Side-by-side versioning</span></span>
* <span data-ttu-id="f0c8e-141">Neue APIs</span><span class="sxs-lookup"><span data-stu-id="f0c8e-141">New APIs</span></span>
* <span data-ttu-id="f0c8e-142">Quelle öffnen</span><span class="sxs-lookup"><span data-stu-id="f0c8e-142">Open source</span></span>

<span data-ttu-id="f0c8e-143">Es wird daran gearbeitet, die API-Lücke von .NET Framework zu .NET Core zu schließen.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="f0c8e-144">Das [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) stellt Tausende nur unter Windows verfügbare APIs in .NET Core zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="f0c8e-145">Diese APIs waren in .NET Core 1.x nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="f0c8e-146">Herunterladen eines Beispiels</span><span class="sxs-lookup"><span data-stu-id="f0c8e-146">How to download a sample</span></span>

<span data-ttu-id="f0c8e-147">Viele der Artikel und Tutorials enthalten Links zu Beispielcode.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-147">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="f0c8e-148">[Laden Sie die Zip-Datei des ASP.NET Repositorys herunter](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="f0c8e-148">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="f0c8e-149">Entpacken Sie die Datei *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-149">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="f0c8e-150">Navigieren Sie mit der URL in der Beispielverknüpfung zum Beispielverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-150">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0c8e-151">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f0c8e-151">Next steps</span></span>

<span data-ttu-id="f0c8e-152">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="f0c8e-152">For more information, see the following resources:</span></span>

* [<span data-ttu-id="f0c8e-153">Erste Schritte mit Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f0c8e-153">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="f0c8e-154">ASP.NET Core – Grundlagen</span><span class="sxs-lookup"><span data-stu-id="f0c8e-154">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="f0c8e-155">Im [wöchentlichen ASP.NET Community Standup](https://live.asp.net/) werden die Fortschritte und Pläne des Teams behandelt.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-155">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="f0c8e-156">Zudem werden neue Blogs und Drittanbietersoftware vorgestellt.</span><span class="sxs-lookup"><span data-stu-id="f0c8e-156">It features new blogs and third-party software.</span></span>
