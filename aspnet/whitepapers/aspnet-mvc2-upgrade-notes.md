---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aktualisieren einer ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: "In diesem Dokument wird sowohl mit einem Assistenten und manuell eine ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 aktualisieren. Dieses Dokument ist auch für d verfügbar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="2ac05-104">Aktualisieren einer ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="2ac05-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="2ac05-105">In diesem Dokument wird sowohl mit einem Assistenten und manuell eine ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2ac05-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="2ac05-106">Dieses Dokument ist auch zur [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ac05-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="2ac05-107">Einführung</span><span class="sxs-lookup"><span data-stu-id="2ac05-107">Introduction</span></span>

<span data-ttu-id="2ac05-108">ASP.NET MVC 2 kann parallel zu ASP.NET MVC 1.0 auf dem gleichen Server installiert werden.</span><span class="sxs-lookup"><span data-stu-id="2ac05-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="2ac05-109">Dies ermöglicht Anwendung Entwickler Flexibilität bei der Wahl, wenn eine 1.0 für ASP.NET MVC-Anwendung für ASP.NET MVC 2 aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2ac05-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="2ac05-110">Visual Studio 2010 umfasst einen Assistenten, Upgrades vorhandene 1.0 für ASP.NET MVC-Projekte mit Visual Studio 2008 in ASP.NET MVC 2 integriert.</span><span class="sxs-lookup"><span data-stu-id="2ac05-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="2ac05-111">Der Upgrade-Assistent wird durch ein 1.0 für ASP.NET MVC-Projekt in Visual Studio 2010 öffnen initiiert.</span><span class="sxs-lookup"><span data-stu-id="2ac05-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="2ac05-112">Upgrade-Assistent für ASP.NET MVC 1.0 für Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="2ac05-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="2ac05-113">Um eine ASP.NET MVC 1.0-Anwendung für ASP.NET MVC 2 in Visual Studio 2008 SP1 zu aktualisieren, verwenden Sie die (nicht unterstützte) MvcAppConverter-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2ac05-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="2ac05-114">Sie können diese Anwendung unter folgender URL herunterladen:</span><span class="sxs-lookup"><span data-stu-id="2ac05-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="2ac05-115">https://go.Microsoft.com/fwlink/?LinkId=185351</span><span class="sxs-lookup"><span data-stu-id="2ac05-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="2ac05-116">Manuelles Aktualisieren eines ASP.NET MVC 1,0-Projekts</span><span class="sxs-lookup"><span data-stu-id="2ac05-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="2ac05-117">Um eine vorhandene 1.0 für ASP.NET MVC-Anwendung auf Version 2 zu aktualisieren, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="2ac05-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="2ac05-118">Erstellen Sie eine Sicherungskopie des vorhandenen Projekts.</span><span class="sxs-lookup"><span data-stu-id="2ac05-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="2ac05-119">Öffnen Sie in einem Text-Editor die Projektdatei (die Datei mit der Dateierweiterung CSPROJ- oder VBPROJ), und suchen Sie das ProjectTypeGuid-Element.</span><span class="sxs-lookup"><span data-stu-id="2ac05-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="2ac05-120">Ersetzen Sie den Wert des jeweiligen Elements die GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="2ac05-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="2ac05-121">Wenn Sie fertig sind, sollte der Wert dieses Elements wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="2ac05-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="2ac05-122">Bearbeiten Sie die Datei "Web.config" im Stammordner Web-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2ac05-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="2ac05-123">Suchen Sie nach System.Web.Mvc, Version = 1.0.0.0, und Ersetzen Sie alle Instanzen mit System.Web.Mvc, Version = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="2ac05-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="2ac05-124">Wiederholen Sie den vorherigen Schritt für die Datei "Web.config" im Ordner "Ansichten" ein.</span><span class="sxs-lookup"><span data-stu-id="2ac05-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="2ac05-125">Öffnen Sie das Projekt mit Visual Studio, und klicken Sie in **Projektmappen-Explorer**, erweitern Sie die **Verweise** Knoten.</span><span class="sxs-lookup"><span data-stu-id="2ac05-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="2ac05-126">Löschen Sie den Verweis auf System.Web.Mvc (die auf die Version 1.0-Assembly verwiesen wird).</span><span class="sxs-lookup"><span data-stu-id="2ac05-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="2ac05-127">Fügen Sie einen Verweis auf System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="2ac05-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="2ac05-128">Das folgende BindingRedirect-Element in die Datei "Web.config" im Stammverzeichnis Anwendung, klicken Sie im Abschnitt Datenausdrücke hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="2ac05-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="2ac05-129">Erstellen Sie eine neue, leere ASP.NET MVC 2-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2ac05-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="2ac05-130">Kopieren Sie die Dateien aus dem Ordner Scripts, der die neue Anwendung in den Ordner "Skripts" der vorhandenen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2ac05-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="2ac05-131">Aktualisieren Sie die vorhandene Ressourcenkennzeichnung™ s CSS-Datei mit den CSS-Stil-Definitionen in der Datei "Site.CSS" ändern.</span><span class="sxs-lookup"><span data-stu-id="2ac05-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="2ac05-132">Kompilieren Sie die Anwendung, und führen Sie es.</span><span class="sxs-lookup"><span data-stu-id="2ac05-132">Compile the application and run it.</span></span> <span data-ttu-id="2ac05-133">Falls ein Fehler auftritt, finden Sie im Abschnitt wichtige Änderungen von der [Neuigkeiten in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) Seite.</span><span class="sxs-lookup"><span data-stu-id="2ac05-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
