---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installieren eine Hilfsmethode in einer ASP.NET Web-Seiten (Razor) Standort | Microsoft Docs
author: tfitzmac
description: Dieser Artikel beschreibt, wie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) installieren. Ein Hilfsprogramm ist eine wiederverwendbare Komponente sein, die Code und Markup zum pro umfasst...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="1fb8b-104">Installieren eine Hilfsprogramm an einem Standort der ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="1fb8b-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="1fb8b-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1fb8b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1fb8b-106">Dieser Artikel beschreibt, wie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) installieren.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="1fb8b-107">Ein *Helper* ist eine wiederverwendbare Komponente, die Code und Markup zum Ausführen einer Aufgabe, die möglicherweise als zu aufwendig oder komplexe enthält.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="1fb8b-108">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="1fb8b-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1fb8b-109">So installieren Sie eine Hilfsprogramm in einer Website mit WebMatrix 3 erstellt.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1fb8b-110">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="1fb8b-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1fb8b-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="1fb8b-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="1fb8b-112">Übersicht über die Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="1fb8b-112">Overview of Helpers</span></span>

<span data-ttu-id="1fb8b-113">Einige Aufgaben, die Benutzer häufig auf Webseiten viel Code erfordern, oder zusätzliche Kenntnisse erfordern.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="1fb8b-114">Beispiele für ein Diagramm für Daten anzeigen; Platzieren eine Twitter-Schaltfläche "Follow" auf einer Seite; sendende e-Mail aus Ihrer Website; Zuschneiden oder Ändern der Bildgröße; Verwenden von PayPal für Ihre Website.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="1fb8b-115">Um diese Arten von Aufgaben zu erleichtern, können Sie mithilfe von ASP.NET Web Pages *Hilfsprogramme*.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="1fb8b-116">Hilfsprogramme sind Komponenten, für deren Installation für einen Standort und, mit denen Sie typische Aufgaben mit nur einer Zeile oder zwei der Razor-Code.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="1fb8b-117">ASP.NET Web Pages verfügt über einige integrierte Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="1fb8b-118">Viele Hilfsprogramme stehen jedoch in Paketen (-add-ins), die mit der NuGet-Paket-Manager bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="1fb8b-119">NuGet ermöglicht die Auswahl ein Pakets installiert, und klicken Sie dann diese übernimmt alle Details der Installation.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="1fb8b-120">Eine Hilfsprogramm installieren in WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="1fb8b-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="1fb8b-121">Klicken Sie in WebMatrix-3, auf die **NuGet** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Im Dialogfeld NuGet Gallery WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="1fb8b-123">Dies startet den NuGet-Paket-Manager und die verfügbaren Pakete angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="1fb8b-124">Geben Sie in das Suchfeld ein Schlüsselwort für das Hilfsprogramm, die, das Sie installieren möchten.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Im Dialogfeld NuGet Gallery WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="1fb8b-126">Wählen Sie das Paket, und klicken Sie dann auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="1fb8b-127">Klicken Sie auf **Ja** Wenn gefragt, ob Sie verwenden möchten, installieren Sie das Paket und anzugeben, dass Sie ihnen zustimmen.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="1fb8b-128">Wenn dies der erste Mal Sie eine Hilfsprogramm installiert haben ist, erstellt NuGet Ordner in Ihre Website für den Code, der das Hilfsprogramm bildet.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="1fb8b-129">Um ein Hilfsprogramm deinstallieren möchten, klicken Sie auf die **Katalog** Schaltfläche, klicken Sie auf die **installiert** Registerkarte, und wählen Sie das Paket deinstalliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="1fb8b-130">Installieren das Twitter-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="1fb8b-130">Installing the Twitter helper</span></span>

<span data-ttu-id="1fb8b-131">Die neueste Version von der Twitter-API ist nicht kompatibel mit der Twitter-Hilfsprogramm, das Sie über NuGet installieren.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="1fb8b-132">Lesen Sie stattdessen die [Twitter-Hilfsprogramm mit WebMatrix](twitter-helper.md) Thema Informationen zum Einrichten der Twitter-Hilfsprogramm in Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="1fb8b-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1fb8b-133">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1fb8b-133">Additional Resources</span></span>


[<span data-ttu-id="1fb8b-134">Einführung in ASP.NET Web Pages 2 - Grundlagen der Programmierung</span><span class="sxs-lookup"><span data-stu-id="1fb8b-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="1fb8b-135">Twitter-Hilfsprogramm mit WebMatrix</span><span class="sxs-lookup"><span data-stu-id="1fb8b-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
