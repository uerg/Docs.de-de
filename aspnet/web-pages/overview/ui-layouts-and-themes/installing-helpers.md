---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installieren eines Hilfsprogramms in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu installieren. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup pro enthält...
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: db3dff9f2d70577bb0618335c0100b9899e87727
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838613"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b5d8e-104">Installieren eines Hilfsprogramms in einer ASP.NET Web Pages (Razor)-Website</span><span class="sxs-lookup"><span data-stu-id="b5d8e-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b5d8e-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b5d8e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b5d8e-106">Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu installieren.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="b5d8e-107">Ein *Helper* ist eine wiederverwendbare Komponente, die enthält Code und Markup zum Ausführen einer Aufgabe, die mühsam oder komplex sein können.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="b5d8e-108">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b5d8e-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b5d8e-109">So installieren Sie eine Hilfsprogramm auf einer Website mit WebMatrix 3 erstellt.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5d8e-110">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b5d8e-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="b5d8e-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="b5d8e-112">Übersicht über die Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b5d8e-112">Overview of Helpers</span></span>

<span data-ttu-id="b5d8e-113">Einige Aufgaben, die Benutzer häufig auf Webseiten ausführen möchten, viel Code erfordern oder zusätzliche Kenntnisse erfordern.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="b5d8e-114">Beispiele sind ein Diagramm für die Daten angezeigt: Platzieren eine Twitter-Schaltfläche "Folgen" auf einer Seite; Senden einer e-Mail aus Ihrer Website; Abschneiden oder zum Skalieren von Bildern; Verwenden von PayPal für Ihre Website.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="b5d8e-115">Um diese Arten von Elementen führen zu erleichtern, können Sie verwenden ASP.NET Web Pages *Hilfsprogramme*.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="b5d8e-116">Hilfsmethoden sind Komponenten, die Sie für einen Standort und, mit denen Installation typische Aufgaben mit nur einer oder zwei Zeilen mit Razor-Code.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="b5d8e-117">ASP.NET Web Pages verfügt über einige Hilfsprogramme integriert.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="b5d8e-118">Allerdings sind viele Hilfsprogramme in Paketen (-add-ins), die bereitgestellt werden, mit dem NuGet-Paket-Manager verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="b5d8e-119">NuGet können Sie ein Paket zum Installieren auswählen und dann übernimmt es alle Details der Installation.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="b5d8e-120">Installieren eines Hilfsprogramms in WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="b5d8e-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="b5d8e-121">Klicken Sie in WebMatrix 3, auf die **NuGet** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Dialogfeld für NuGet-Katalog in WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="b5d8e-123">Dies startet den NuGet-Paket-Manager und die verfügbaren Pakete angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="b5d8e-124">Geben Sie in das Suchfeld ein Schlüsselwort für das Hilfsprogramm, die, das Sie installieren möchten.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Dialogfeld für NuGet-Katalog in WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="b5d8e-126">Wählen Sie das Paket, und klicken Sie dann auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="b5d8e-127">Klicken Sie auf **Ja** Wenn gefragt, ob Sie öffnen möchten, installieren Sie das Paket und anzugeben, dass Sie die Lizenzbedingungen akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="b5d8e-128">Wenn dies der erste Mal Sie eine Hilfsprogramm installiert haben ist, erstellt NuGet Ordner auf Ihrer Website für den Code, der das Hilfsprogramm bildet.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="b5d8e-129">Um ein Hilfsprogramm zu deinstallieren, klicken Sie auf die **Katalog** , zeigen Sie auf die **installiert** Registerkarte, und wählen Sie das Paket deinstalliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="b5d8e-130">Installieren die Twitter-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="b5d8e-130">Installing the Twitter helper</span></span>

<span data-ttu-id="b5d8e-131">Die neueste Version von der Twitter-API ist nicht kompatibel ist, mit der Twitter-Hilfsprogramm, die, das Sie über NuGet installieren.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="b5d8e-132">Lesen Sie stattdessen die [Twitter-Hilfsprogramm mit WebMatrix](twitter-helper.md) Thema enthält Informationen zum Einrichten der Twitter-Hilfsprogramm in Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="b5d8e-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b5d8e-133">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b5d8e-133">Additional Resources</span></span>


[<span data-ttu-id="b5d8e-134">Einführung in ASP.NET Web Pages 2 – Grundlagen der Programmierung</span><span class="sxs-lookup"><span data-stu-id="b5d8e-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="b5d8e-135">Twitter-Hilfsprogramm mit WebMatrix</span><span class="sxs-lookup"><span data-stu-id="b5d8e-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
