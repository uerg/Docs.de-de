---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "Versionshinweise für ASP.NET und Webtools 2013.1 für Visual Studio 2012 | Microsoft Docs"
author: microsoft
description: "Dieses Dokument beschreibt die Version von ASP.NET und 2013.1 für Web-Tools für Visual Studio 2012."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1e4ee8eb4901305bf6a8c9c5b949dc4ee10290e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="92b21-103">Versionshinweise für ASP.NET und Webtools 2013.1 für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="92b21-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="92b21-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="92b21-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="92b21-105">Dieses Dokument beschreibt die Version von ASP.NET und 2013.1 für Web-Tools für Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="92b21-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="92b21-106">Inhalt</span><span class="sxs-lookup"><span data-stu-id="92b21-106">Contents</span></span>

- [<span data-ttu-id="92b21-107">Hinweise zur installationsnachbereitung</span><span class="sxs-lookup"><span data-stu-id="92b21-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="92b21-108">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="92b21-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="92b21-109">Neue Funktionen in ASP.NET und Webtools 2013.1 für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="92b21-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="92b21-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="92b21-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="92b21-111">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="92b21-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="92b21-112">ASP.NET MVC 5-Vorlage</span><span class="sxs-lookup"><span data-stu-id="92b21-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="92b21-113">ASP.NET Web API 2-Vorlage</span><span class="sxs-lookup"><span data-stu-id="92b21-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="92b21-114">Elementvorlagen</span><span class="sxs-lookup"><span data-stu-id="92b21-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="92b21-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="92b21-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="92b21-116">ASP.NET Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="92b21-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="92b21-117">Razor-Editor</span><span class="sxs-lookup"><span data-stu-id="92b21-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="92b21-118">NuGet-2.7</span><span class="sxs-lookup"><span data-stu-id="92b21-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="92b21-119">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="92b21-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="92b21-120">ASP.NET Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="92b21-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="92b21-121">MVC und Web-API Gerüstbau - HTTP 404, wurde nicht gefunden.</span><span class="sxs-lookup"><span data-stu-id="92b21-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="92b21-122">Visual Studio Express 2012 für das Web funktioniert nicht mehr nach dem Hinzufügen eines gerüstelements</span><span class="sxs-lookup"><span data-stu-id="92b21-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="92b21-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="92b21-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="92b21-124">Anzeigen von Cshtml-Datei Browserauswahl oder F5 bewirkt, dass einen Serverfehler</span><span class="sxs-lookup"><span data-stu-id="92b21-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="92b21-125">URL Rewrite und Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="92b21-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="92b21-126">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="92b21-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="92b21-127">Hinweise zur installationsnachbereitung</span><span class="sxs-lookup"><span data-stu-id="92b21-127">Installation Notes</span></span>

<span data-ttu-id="92b21-128">[Installieren Sie](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET und Tools für Visual Studio 2012 2013.1.</span><span class="sxs-lookup"><span data-stu-id="92b21-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="92b21-129">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="92b21-129">Software Requirements</span></span>

<span data-ttu-id="92b21-130">Sie benötigen Visual Studio 2012 oder Visual Studio Express 2012 für das Web.</span><span class="sxs-lookup"><span data-stu-id="92b21-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="92b21-131">Neue Funktionen in ASP.NET und Webtools 2013.1 für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="92b21-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="92b21-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="92b21-132">Bootstrap</span></span>

<span data-ttu-id="92b21-133">Wenn Sie das Gerüst für MVC 5-Controller und Ansichten erstellen, verwendet das Markup für die Ansichten [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="92b21-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="92b21-134">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="92b21-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="92b21-135">ASP.NET MVC 5-Vorlage</span><span class="sxs-lookup"><span data-stu-id="92b21-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="92b21-136">Eine neue MVC 5-Vorlage wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="92b21-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="92b21-137">Er verweist auf die neueste MVC 5-NuGet-Pakete, und Sie können Gerüstbau Controller und Ansichten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="92b21-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="92b21-138">ASP.NET Web API 2-Vorlage</span><span class="sxs-lookup"><span data-stu-id="92b21-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="92b21-139">Eine neue Web-API 2-Vorlage wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="92b21-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="92b21-140">Er verweist auf die neueste Web API 2-NuGet-Pakete, und Sie können Gerüstbau Controller und Ansichten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="92b21-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="92b21-141">Elementvorlagen</span><span class="sxs-lookup"><span data-stu-id="92b21-141">Item Templates</span></span>

<span data-ttu-id="92b21-142">Wir haben neue Elementvorlagen für MVC 5-Ansichten, Web Pages (Razor-3) und Web-API 2-Controller hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="92b21-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="92b21-143">Sie installieren die verwandte NuGet-Pakete für das Projekt beim Hinzufügen neuer Elemente.</span><span class="sxs-lookup"><span data-stu-id="92b21-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="92b21-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="92b21-144">Entity Framework 6</span></span>

<span data-ttu-id="92b21-145">Wenn Sie einen MVC oder Web-API-Controller, die Verwendung von Entity Framework zu erstellen, verwenden wir Framework 6.</span><span class="sxs-lookup"><span data-stu-id="92b21-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="92b21-146">Weitere Informationen über Entity Framework finden Sie unter der [Entity Framework-Versionsverlaufs](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="92b21-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="92b21-147">Sie können auch herunterladen und installieren den Entity Framework 6-Tools für Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="92b21-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="92b21-148">Finden Sie unter der [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="92b21-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="92b21-149">ASP.NET Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="92b21-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="92b21-150">ASP.NET Gerüstbau ist ein Code-Generierung-Framework für ASP.NET-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="92b21-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="92b21-151">Es vereinfacht den Standardcode zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.</span><span class="sxs-lookup"><span data-stu-id="92b21-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="92b21-152">In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte.</span><span class="sxs-lookup"><span data-stu-id="92b21-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="92b21-153">Mit diesem Update können Sie jetzt Gerüst für eine beliebige ASP.NET-Projekt, einschließlich Web Forms verwenden.</span><span class="sxs-lookup"><span data-stu-id="92b21-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="92b21-154">Dieses Update Generieren von Seiten für eine Web Forms-Projekt nicht unterstützt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt MVC-Abhängigkeiten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="92b21-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="92b21-155">Unterstützung für das Generieren von Seiten für Web Forms wird in einem zukünftigen Update hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="92b21-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="92b21-156">Mithilfe von Gerüstbau stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind.</span><span class="sxs-lookup"><span data-stu-id="92b21-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="92b21-157">Z. B. Wenn Sie mit einer ASP.NET Web Forms-Projekt beginnen und dann mithilfe von Gerüstbau einer Web-API-Controller hinzufügen, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="92b21-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="92b21-158">Um Gerüstbau für MVC eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster.</span><span class="sxs-lookup"><span data-stu-id="92b21-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="92b21-159">Es gibt zwei Optionen für MVC Gerüstbau; Minimal "und" vollständig ".</span><span class="sxs-lookup"><span data-stu-id="92b21-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="92b21-160">Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="92b21-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="92b21-161">Wenn Sie die Option "vollständig", wählen Sie die minimale Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.</span><span class="sxs-lookup"><span data-stu-id="92b21-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="92b21-162">Unterstützung für asynchrone Controller Gerüstbau verwendet die neue asynchrone Funktionen von Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="92b21-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="92b21-163">Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="92b21-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="92b21-164">In diesen Lernprogrammen erfahren Gerüstbau mit Visual Studio 2013, aber sie gelten auch für ASP.NET und 2013.1 von Web Tools für Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="92b21-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="92b21-165">Razor-Editor</span><span class="sxs-lookup"><span data-stu-id="92b21-165">Razor Editor</span></span>

<span data-ttu-id="92b21-166">Mit diesem Update unterstützt Visual Studio 2012 jetzt 3 Razor-Tools/bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="92b21-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="92b21-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="92b21-167">NuGet 2.7</span></span>

<span data-ttu-id="92b21-168">NuGet 2.7 enthält einen umfangreichen Satz von neuen Features, die beschrieben werden ausführlich auf [NuGet 2.7-Anmerkungen zu dieser Version](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="92b21-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="92b21-169">Diese Version von NuGet beseitigt die Notwendigkeit für Benutzer ermöglichen, dass explizit NuGet wiederherstellen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="92b21-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="92b21-170">Bei der Installation von NuGet 2.7 stimmen Benutzer implizit automatisch wiederherstellen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="92b21-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="92b21-171">Benutzer können außerhalb des Pakets Wiederherstellung über die NuGet-Einstellungen in Visual Studio explizit deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="92b21-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="92b21-172">Diese Änderung vereinfacht die Funktionsweise der Wiederherstellung des Pakets.</span><span class="sxs-lookup"><span data-stu-id="92b21-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="92b21-173">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="92b21-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="92b21-174">ASP.NET Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="92b21-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="92b21-175">MVC und Web-API Gerüstbau - HTTP 404, wurde nicht gefunden.</span><span class="sxs-lookup"><span data-stu-id="92b21-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="92b21-176">Wenn Sie beim Hinzufügen eines gerüstelements zu einem Projekt ein Fehler auftritt, ist es möglich, die Ihr Projekt in einem inkonsistenten Zustand gelassen werden.</span><span class="sxs-lookup"><span data-stu-id="92b21-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="92b21-177">Einige Änderungen vorgenommen werden Gerüstbau wird ein Rollback, aber andere Änderungen, z. B. die installierten NuGet-Pakete werden nicht zurückgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="92b21-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="92b21-178">Falls die routing konfigurationsänderungen zurückgesetzt werden, erhalten Benutzer einen HTTP 404-Fehler beim Navigieren zu Elemente Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="92b21-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="92b21-179">Klicken Sie zum Beheben dieses Fehlers für MVC eine neue Elements mit Gerüst hinzufügen, und wählen Sie MVC 5-Abhängigkeiten (minimale oder vollständige).</span><span class="sxs-lookup"><span data-stu-id="92b21-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="92b21-180">Dabei werden alle erforderlichen Änderungen zu Ihrem Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="92b21-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="92b21-181">So beheben Sie diesen Fehler für die Web-API:</span><span class="sxs-lookup"><span data-stu-id="92b21-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="92b21-182">Fügen Sie die folgende Klasse "webapiconfig" zu Ihrem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="92b21-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="92b21-183">Konfigurieren Sie in der Anwendung WebApiConfig.Register\_Start-Methode in "Global.asax" wie folgt:</span><span class="sxs-lookup"><span data-stu-id="92b21-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="92b21-184">Visual Studio Express 2012 für das Web funktioniert nicht mehr nach dem Hinzufügen eines gerüstelements</span><span class="sxs-lookup"><span data-stu-id="92b21-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="92b21-185">Wenn Visual Studio Express 2012 für das Web nicht mehr funktioniert nach dem Hinzufügen des Elements mit Gerüst mit Entity Framework (z. B. Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework), ist es möglich, dass Visual Studio Express nicht das systemeigene Image einer Assembly zu laden. Abhängig von System.Web.Extensions hinzu.</span><span class="sxs-lookup"><span data-stu-id="92b21-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="92b21-186">Um dieses Problem zu beheben, konfigurieren Sie Visual Studio Express mit MSIL-Images System.Web.Extensions arbeiten:</span><span class="sxs-lookup"><span data-stu-id="92b21-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="92b21-187">Öffnen Sie Eingabeaufforderung im Administratormodus.</span><span class="sxs-lookup"><span data-stu-id="92b21-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="92b21-188">Wechseln Sie zu %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE oder % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (für 64-Bit-Windows).</span><span class="sxs-lookup"><span data-stu-id="92b21-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="92b21-189">VWDExpress.exe.config in einem Text-Editor zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="92b21-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="92b21-190">Fügen Sie die folgende Zeile unter der &lt;Konfiguration&gt;/&lt;Runtime&gt; Element:</span><span class="sxs-lookup"><span data-stu-id="92b21-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="92b21-191">Neustart des Visual Studio Express 2012 für das Web.</span><span class="sxs-lookup"><span data-stu-id="92b21-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="92b21-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="92b21-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="92b21-193">Anzeigen von Cshtml-Datei WithBrowse WithorF5causes eines Serverfehlers</span><span class="sxs-lookup"><span data-stu-id="92b21-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="92b21-194">Beim Erstellen einer MVC 5-Projekt in Visual Studio 2012 (oder öffnen Sie in Visual Studio 2012 ein MVC 5-Projekt, das in Visual Studio 2013 erstellt wurde) und versucht, eine Cshtml-Datei mithilfe von Browserauswahl oder F5 anzeigen, wird eine Fehlermeldung, die besagt - **Serverfehler in '/' Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="92b21-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="92b21-195">Der Server versucht, zu navigieren`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="92b21-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="92b21-196">Um dieses Problem zu beheben, ändern Sie die **Startaktion** in Ihrem Projekt zum Festlegen von **bestimmte Seite**.</span><span class="sxs-lookup"><span data-stu-id="92b21-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="92b21-197">Sie müssen nicht auf einen Wert für die Seite bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="92b21-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="92b21-198">Nach dieser Änderung, die in das Stammverzeichnis der Anwendung navigiert F5 auswählen (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="92b21-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="92b21-199">Dieses Verhalten ist nicht mit dem Verhalten für MVC 5-Projekte in Visual Studio 2013 identisch, in dem die **aktuelle Seite** Einstellung startet die Seite öffnen.</span><span class="sxs-lookup"><span data-stu-id="92b21-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="92b21-200">URL Rewrite und Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="92b21-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="92b21-201">Nach dem Upgrade auf ASP.NET Razor-3 oder ASP.NET MVC 5, kann die Notation tilde(~) nicht mehr ordnungsgemäß funktioniert, bei Verwendung von URL-schreibt.</span><span class="sxs-lookup"><span data-stu-id="92b21-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="92b21-202">Der URL-Rewrite beeinflusst die tilde(~) Notation in HTML-Elementen wie &lt;A /&gt;, &lt;Skript /&gt;, &lt;LINK /&gt;, und daher die Tilde nicht mehr in das Stammverzeichnis zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="92b21-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="92b21-203">Angenommen, Sie schreiben Sie Anforderungen für **asp.net/content** auf **asp.net**, Href-Attribut im &lt;A Href = "~/content/" /&gt; löst in **/content/ Inhalt /** anstelle von  **/** .</span><span class="sxs-lookup"><span data-stu-id="92b21-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="92b21-204">Um diese Änderung zu unterdrücken, legen Sie die **IIS\_WasUrlRewritten** Kontext auf "false" in jeder Webseite oder in **Anwendung\_BeginRequest** in "Global.asax".</span><span class="sxs-lookup"><span data-stu-id="92b21-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="92b21-205">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="92b21-205">Templates</span></span>

<span data-ttu-id="92b21-206">Wenn Sie ASP.NET MVC Projekte mit Visual Studio 2012 für Windows 8.1 oder Windows Server 2012 R2, Visual Studio zu erstellen, zeigt eine Fehlermeldung, die angibt, "Konfigurieren von Web [Url] für ASP.NET 4.5 fehlgeschlagen".</span><span class="sxs-lookup"><span data-stu-id="92b21-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Fehler bei der Konfiguration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="92b21-208">Dieser Fehler wird angezeigt, weil die ASP.NET 4.5-Funktion von Visual Studio 2012 nicht aktiviert wird, wenn sie auf diese Versionen von Windows installiert ist.</span><span class="sxs-lookup"><span data-stu-id="92b21-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="92b21-209">Um ASP.NET 4.5 zu aktivieren, führen Sie die Schritte, die in beschriebenen [Windows-Funktionen ein- oder ausschalten](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="92b21-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).</span></span>

![Aktivieren Sie oder deaktivieren Sie Windows-features](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="92b21-211">Alternativ können Sie ASP.NET 4.5 über die Befehlszeile aktivieren.</span><span class="sxs-lookup"><span data-stu-id="92b21-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="92b21-212">Öffnen Sie Eingabeaufforderung im Administratormodus.</span><span class="sxs-lookup"><span data-stu-id="92b21-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="92b21-213">Führen Sie den folgenden Befehl zum Aktivieren von ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="92b21-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
