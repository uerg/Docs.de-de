---
uid: mvc/overview/releases/mvc51-release-notes
title: Neues in ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="cbee2-102">Was ist neu in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="cbee2-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="cbee2-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cbee2-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="cbee2-104">Dieses Thema beschreibt die Neuigkeiten für ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="cbee2-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="cbee2-105">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="cbee2-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="cbee2-106">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="cbee2-106">Download</span></span>](#download)
- [<span data-ttu-id="cbee2-107">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="cbee2-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="cbee2-108">Neue Funktionen in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="cbee2-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="cbee2-109">-Attribut routing Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="cbee2-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="cbee2-110">Bootstrap-Unterstützung für Editor-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="cbee2-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="cbee2-111">Enum-Unterstützung in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="cbee2-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="cbee2-112">Unaufdringlichen Überprüfung für MinLength/MaxLength-Attribute</span><span class="sxs-lookup"><span data-stu-id="cbee2-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="cbee2-113">Unterstützung von 'this' Kontext in der unaufdringlichen Ajax</span><span class="sxs-lookup"><span data-stu-id="cbee2-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="cbee2-114">[Bekannte Probleme und Änderungen, die die](#KnownBreakingChanges)- [Fehlerkorrekturen](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="cbee2-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="cbee2-115">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="cbee2-115">Software Requirements</span></span>

- <span data-ttu-id="cbee2-116">Visual Studio 2012: Herunterladen [ASP.NET- und Webdienst-Tools für Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="cbee2-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="cbee2-117">Visual Studio 2013: Herunterladen [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="cbee2-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="cbee2-118">Dieses Update ist erforderlich, für die Bearbeitung von ASP.NET MVC 5.1 Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="cbee2-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="cbee2-119">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="cbee2-119">Download</span></span>

<span data-ttu-id="cbee2-120">Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="cbee2-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="cbee2-121">Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="cbee2-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="cbee2-122">Das aktuellste Paket von ASP.NET MVC 5.1 RTM wurde die folgende Version: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="cbee2-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="cbee2-123">Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="cbee2-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="cbee2-124">Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.</span><span class="sxs-lookup"><span data-stu-id="cbee2-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="cbee2-125">Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:</span><span class="sxs-lookup"><span data-stu-id="cbee2-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="cbee2-126">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="cbee2-126">Documentation</span></span>

<span data-ttu-id="cbee2-127">Lernprogramme und Weitere Informationen zu ASP.NET MVC 5.1 RTM sind von der Website für ASP.NET (https://www.asp.net) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="cbee2-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="cbee2-128">Neue Funktionen in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="cbee2-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="cbee2-129">-Attribut routing Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="cbee2-129">Attribute routing improvements</span></span>

 <span data-ttu-id="cbee2-130">Routing jetzt unterstützt Einschränkungen, versionsverwaltung und Header aktivieren Attribut basierten Routenauswahl.</span><span class="sxs-lookup"><span data-stu-id="cbee2-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="cbee2-131">Viele Aspekte der attributenrouten sind jetzt über anpassbare der `IDirectRouteFactory` Schnittstelle und `RouteFactoryAttribute` Klasse.</span><span class="sxs-lookup"><span data-stu-id="cbee2-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="cbee2-132">Das Routenpräfix ist jetzt erweiterbar ist, über die `IRoutePrefix` Schnittstelle und `RoutePrefixAttribute` Klasse.</span><span class="sxs-lookup"><span data-stu-id="cbee2-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="cbee2-133">Enum-Unterstützung in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="cbee2-133">Enum support in views</span></span>

1. <span data-ttu-id="cbee2-134">Neue `@Html.EnumDropDownListFor()` Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="cbee2-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="cbee2-135">Diese sollte verwendet werden, wie die meisten der HTML-Hilfsmethoden, mit der Einschränkung, die Auswertung des Ausdrucks muss ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ oder eine [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) , in dem `T` ist ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ.</span><span class="sxs-lookup"><span data-stu-id="cbee2-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="cbee2-136">Verwendung `EnumHelper.IsValidForEnumHelper()` um diese Anforderungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="cbee2-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="cbee2-137">Neue `EnumHelper.GetSelectList()` Methoden, die Zurückgeben einer `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="cbee2-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="cbee2-138">Dies ist nützlich, wenn Sie eine select-Liste vor dem aufrufen, z. B. bearbeiten müssen `@Html.DropDownListFor()`, oder wenn die Namen angezeigt werden sollen die `@Html.EnumDropDownListFor()` zeigt.</span><span class="sxs-lookup"><span data-stu-id="cbee2-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="cbee2-139">Der folgende Code zeigt diese APIs.</span><span class="sxs-lookup"><span data-stu-id="cbee2-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="cbee2-140">Sie können ein vollständiges Beispiel finden Sie unter [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="cbee2-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="cbee2-141">Bootstrap-Unterstützung für Editor-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="cbee2-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="cbee2-142">Jetzt können wir übergeben in HTML-Attribute in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) als ein [anonymes Objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbee2-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="cbee2-143">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cbee2-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="cbee2-144">Unaufdringlichen Validierung MinLengthAttribute und MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="cbee2-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="cbee2-145">Die clientseitige Validierung für Strings und Arrays von Typen wird jetzt unterstützt, für Eigenschaften mit ergänzt die ["minLength"](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) und [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) Attribute.</span><span class="sxs-lookup"><span data-stu-id="cbee2-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="cbee2-146">Unterstützung von 'this' Kontext in der unaufdringlichen Ajax</span><span class="sxs-lookup"><span data-stu-id="cbee2-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="cbee2-147">Die Rückruffunktionen (`OnBegin, OnComplete, OnFailure, OnSuccess`) wird nun in der Lage, suchen Sie das aufrufende Element über den `this` Kontext.</span><span class="sxs-lookup"><span data-stu-id="cbee2-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="cbee2-148">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cbee2-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="cbee2-149">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="cbee2-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="cbee2-150">Routing-Attribut</span><span class="sxs-lookup"><span data-stu-id="cbee2-150">Attribute Routing</span></span>

<span data-ttu-id="cbee2-151">Mehrdeutigkeiten im Attribut routing Übereinstimmungen meldet jetzt eine Fehlermeldung statt die erste Übereinstimmung auswählen.</span><span class="sxs-lookup"><span data-stu-id="cbee2-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="cbee2-152">Attributenrouten sind nicht zulässig, von der Verwendung der `{controller}` Parameter, und von der Verwendung der `{action}` Parameter auf die Routen zu Aktionen platziert.</span><span class="sxs-lookup"><span data-stu-id="cbee2-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="cbee2-153">Dieser Parameter wird würde sehr wahrscheinlich zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="cbee2-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="cbee2-154">Gerüstbau MVC/Web-API in einem Projekt mit 5.1 Pakete-Ergebnissen in 5.0-Paketen für Argumente, die nicht bereits im Projekt vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="cbee2-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="cbee2-155">Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.1 RTM wird nicht Visual Studio-Tools wie z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="cbee2-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="cbee2-156">Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="cbee2-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="cbee2-157">Daher wird das Gerüst ASP.NET die Vorgängerversion (5.0.0.0) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cbee2-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="cbee2-158">Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben.</span><span class="sxs-lookup"><span data-stu-id="cbee2-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="cbee2-159">Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren die Pakete der Projekte, auf die Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen der Web-API und ASP.NET MVC konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="cbee2-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="cbee2-160">Syntaxhervorhebung für Razor-Ansichten in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cbee2-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="cbee2-161">Wenn Sie in ASP.NET MVC 5.1 RTM aktualisieren, ohne Visual Studio 2013 zu aktualisieren, werden Sie Unterstützung von Visual Studio-Editor nicht abgerufen werden zur syntaxhervorhebung beim Bearbeiten der Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="cbee2-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="cbee2-162">Sie müssen zum Aktualisieren von Visual Studio 2013, um diese Unterstützung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="cbee2-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="cbee2-163">Umbenennen von Typ</span><span class="sxs-lookup"><span data-stu-id="cbee2-163">Type Renames</span></span>

<span data-ttu-id="cbee2-164">Einige der für die Attribut-routing-Erweiterbarkeit verwendeten Typen sind in 5.1 RTM umbenannt.</span><span class="sxs-lookup"><span data-stu-id="cbee2-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="cbee2-165">**Alter Typname (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="cbee2-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="cbee2-166">**Neuer Typname (5.1-RTM-Version)**</span><span class="sxs-lookup"><span data-stu-id="cbee2-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="cbee2-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="cbee2-167">IDirectRouteProvider</span></span> | <span data-ttu-id="cbee2-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="cbee2-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="cbee2-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="cbee2-169">RouteProviderAttribute</span></span> | <span data-ttu-id="cbee2-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="cbee2-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="cbee2-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="cbee2-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="cbee2-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="cbee2-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="cbee2-173">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="cbee2-173">Bug Fixes</span></span>

<span data-ttu-id="cbee2-174">Diese Version enthält auch verschiedene Fehlerbehebungen.</span><span class="sxs-lookup"><span data-stu-id="cbee2-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="cbee2-175">Sie können die vollständige Liste hier finden:</span><span class="sxs-lookup"><span data-stu-id="cbee2-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="cbee2-176">5.1.0-Paket</span><span class="sxs-lookup"><span data-stu-id="cbee2-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="cbee2-177">5.1.1-Paket</span><span class="sxs-lookup"><span data-stu-id="cbee2-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="cbee2-178">Die 5.1.2 Paket enthält, aber keine Fehlerkorrekturen IntelliSense-Updates.</span><span class="sxs-lookup"><span data-stu-id="cbee2-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
