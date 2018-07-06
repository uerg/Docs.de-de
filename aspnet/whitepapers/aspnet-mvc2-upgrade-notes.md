---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aktualisieren von ASP.NET MVC 1.0-Anwendungen zu ASP.NET MVC 2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird beschrieben wie Sie manuell, und mit einem Assistenten eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2 aktualisieren. In diesem Dokument finden Sie auch für d...
ms.author: aspnetcontent
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: fe1696cd8f98f2ff253d385b62a6bcd74b536d33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823870"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="3298f-104">Aktualisieren von ASP.NET MVC 1.0-Anwendungen zu ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="3298f-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="3298f-105">In diesem Dokument wird beschrieben wie Sie manuell, und mit einem Assistenten eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2 aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="3298f-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="3298f-106">In diesem Dokument finden Sie auch für [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="3298f-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="3298f-107">Einführung</span><span class="sxs-lookup"><span data-stu-id="3298f-107">Introduction</span></span>

<span data-ttu-id="3298f-108">ASP.NET MVC 2 kann parallel zu ASP.NET MVC 1.0 auf dem gleichen Server installiert werden.</span><span class="sxs-lookup"><span data-stu-id="3298f-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="3298f-109">Dadurch wird die Anwendung Entwickler Flexibilität bei der Entscheidung, wann eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2 aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="3298f-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="3298f-110">Visual Studio 2010 enthält einen Assistenten, Upgrades, die vorhandene ASP.NET MVC 1.0-Projekten mit Visual Studio 2008 zu ASP.NET MVC 2 erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="3298f-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="3298f-111">Der Upgrade-Assistent wird initiiert, von einer ASP.NET MVC 1.0-Projekt in Visual Studio 2010 geöffnet.</span><span class="sxs-lookup"><span data-stu-id="3298f-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="3298f-112">Upgrade-Assistent für ASP.NET MVC 1.0 für Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="3298f-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="3298f-113">Um eine ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 in Visual Studio 2008 SP1 zu aktualisieren, verwenden Sie die (nicht unterstützte) MvcAppConverter-Anwendung ein.</span><span class="sxs-lookup"><span data-stu-id="3298f-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="3298f-114">Sie können diese Anwendung von folgender URL herunterladen:</span><span class="sxs-lookup"><span data-stu-id="3298f-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="3298f-115">Manuelles Aktualisieren eines ASP.NET MVC 1.0-Projekts</span><span class="sxs-lookup"><span data-stu-id="3298f-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="3298f-116">Um eine vorhandene ASP.NET MVC 1.0-Anwendung auf Version 2 zu aktualisieren, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="3298f-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="3298f-117">Erstellen Sie eine Sicherungskopie des vorhandenen Projekts.</span><span class="sxs-lookup"><span data-stu-id="3298f-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="3298f-118">Klicken Sie in einem Text-Editor öffnen Sie die Projektdatei (die Datei mit der Dateinamenerweiterung .csproj oder .vbproj), und suchen Sie nach den ProjectTypeGuid-Element.</span><span class="sxs-lookup"><span data-stu-id="3298f-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="3298f-119">Ersetzen Sie die GUID-{603c0e0b-db56-11dc-be95-000d561079b0} mit {F85E285D-A4E0-4152-9332-AB1D724D3325}, als der Wert dieses Elements.</span><span class="sxs-lookup"><span data-stu-id="3298f-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="3298f-120">Wenn Sie fertig sind, sollte der Wert dieses Elements wie folgt lauten:</span><span class="sxs-lookup"><span data-stu-id="3298f-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="3298f-121">Bearbeiten Sie die Datei "Web.config" im Stammordner Web-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3298f-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="3298f-122">Suchen Sie nach der System.Web.Mvc, Version = 1.0.0.0, und Ersetzen Sie alle Instanzen mit System.Web.Mvc, Version = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="3298f-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="3298f-123">Wiederholen Sie den vorherigen Schritt für die Datei "Web.config" im Ordner "Views" ein.</span><span class="sxs-lookup"><span data-stu-id="3298f-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="3298f-124">Öffnen Sie das Projekt mit Visual Studio, und klicken Sie in **Projektmappen-Explorer**, erweitern Sie die **Verweise** Knoten.</span><span class="sxs-lookup"><span data-stu-id="3298f-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="3298f-125">Löschen Sie den Verweis auf System.Web.Mvc (die auf die Version 1.0-Assembly verwiesen wird).</span><span class="sxs-lookup"><span data-stu-id="3298f-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="3298f-126">Fügen Sie einen Verweis auf System.Web.Mvc (v2.0.0.0) hinzu.</span><span class="sxs-lookup"><span data-stu-id="3298f-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="3298f-127">Fügen Sie der Datei "Web.config" im Stammverzeichnis Anwendung, die im Abschnitt "Konfiguration" mit dem folgenden BindingRedirect-Element hinzu:</span><span class="sxs-lookup"><span data-stu-id="3298f-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="3298f-128">Erstellen Sie eine neue leere ASP.NET MVC 2-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3298f-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="3298f-129">Kopieren Sie die Dateien aus dem Ordner "Scripts" für die neue Anwendung in den Ordner "Scripts" der vorhandenen Anwendung ein.</span><span class="sxs-lookup"><span data-stu-id="3298f-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="3298f-130">Aktualisieren Sie die vorhandene Webanwendung€™ s CSS-Datei mit der CSS-Formatdefinitionen in der Datei "Site.CSS".</span><span class="sxs-lookup"><span data-stu-id="3298f-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="3298f-131">Kompilieren Sie die Anwendung, und führen Sie ihn aus.</span><span class="sxs-lookup"><span data-stu-id="3298f-131">Compile the application and run it.</span></span> <span data-ttu-id="3298f-132">Wenn Fehler auftreten, finden Sie im Abschnitt grundlegende Änderungen in der die [Neuigkeiten in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) Seite.</span><span class="sxs-lookup"><span data-stu-id="3298f-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
