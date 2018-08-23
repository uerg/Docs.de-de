---
uid: mvc/overview/releases/mvc51-release-notes
title: Was ist neu in ASP.NET MVC 5.1 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831596"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="c425e-102">Neuerungen in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="c425e-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="c425e-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c425e-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="c425e-104">Dieses Thema beschreibt, welche neuerungen für ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="c425e-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="c425e-105">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="c425e-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="c425e-106">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="c425e-106">Download</span></span>](#download)
- [<span data-ttu-id="c425e-107">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="c425e-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="c425e-108">Neue Features in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="c425e-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="c425e-109">Attribut-routing-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="c425e-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="c425e-110">Bootstrap-Unterstützung für Editor-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="c425e-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="c425e-111">Unterstützung von Enumerationen in Ansichten</span><span class="sxs-lookup"><span data-stu-id="c425e-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="c425e-112">Unaufdringliche Validierung für MinLength/MaxLength-Attribute</span><span class="sxs-lookup"><span data-stu-id="c425e-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="c425e-113">Die "this"-Kontext unterstützen in unaufdringlichen Ajax</span><span class="sxs-lookup"><span data-stu-id="c425e-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="c425e-114">[Bekannte Probleme und Änderungen](#KnownBreakingChanges)- [Fehlerbehebungen](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="c425e-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="c425e-115">Softwareanforderungen</span><span class="sxs-lookup"><span data-stu-id="c425e-115">Software Requirements</span></span>

- <span data-ttu-id="c425e-116">Visual Studio 2012: Herunterladen [ASP.NET und Web Tools 2013.1 für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="c425e-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="c425e-117">Visual Studio 2013: Herunterladen [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="c425e-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="c425e-118">Dieses Update ist erforderlich, für die Bearbeitung von ASP.NET MVC 5.1 Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="c425e-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="c425e-119">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="c425e-119">Download</span></span>

<span data-ttu-id="c425e-120">Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="c425e-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="c425e-121">Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="c425e-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="c425e-122">Das neueste Paket mit ASP.NET MVC 5.1 RTM hat die folgende Version: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="c425e-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="c425e-123">Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="c425e-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="c425e-124">Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.</span><span class="sxs-lookup"><span data-stu-id="c425e-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="c425e-125">Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="c425e-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="c425e-126">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="c425e-126">Documentation</span></span>

<span data-ttu-id="c425e-127">Lernprogramme und Weitere Informationen zu ASP.NET MVC 5.1 RTM stehen auf der Website für ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="c425e-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="c425e-128">Neue Features in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="c425e-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="c425e-129">Attribut-routing-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="c425e-129">Attribute routing improvements</span></span>

 <span data-ttu-id="c425e-130">Attributrouting jetzt aktivieren der versionsverwaltung und Header unterstützt Einschränkungen basierend Routenauswahl.</span><span class="sxs-lookup"><span data-stu-id="c425e-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="c425e-131">Viele Aspekte der attributrouten können jetzt über angepasst werden die `IDirectRouteFactory` Schnittstelle und `RouteFactoryAttribute` Klasse.</span><span class="sxs-lookup"><span data-stu-id="c425e-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="c425e-132">Das Routenpräfix ist nun erweiterbar, über die `IRoutePrefix` Schnittstelle und `RoutePrefixAttribute` Klasse.</span><span class="sxs-lookup"><span data-stu-id="c425e-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="c425e-133">Unterstützung von Enumerationen in Ansichten</span><span class="sxs-lookup"><span data-stu-id="c425e-133">Enum support in views</span></span>

1. <span data-ttu-id="c425e-134">Neue `@Html.EnumDropDownListFor()` Helper-Methoden.</span><span class="sxs-lookup"><span data-stu-id="c425e-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="c425e-135">Diese sollte verwendet werden, wie die meisten der HTML-Hilfsprogrammen, mit der Einschränkung, die Auswertung des Ausdrucks muss ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ oder ein [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) , in denen `T` ist ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ.</span><span class="sxs-lookup"><span data-stu-id="c425e-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="c425e-136">Verwendung `EnumHelper.IsValidForEnumHelper()` um diese Anforderungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="c425e-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="c425e-137">Neue `EnumHelper.GetSelectList()` Methoden, die Zurückgeben einer `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="c425e-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="c425e-138">Dies ist nützlich, wenn Sie eine select-Liste vor dem aufrufen, z. B. bearbeiten müssen `@Html.DropDownListFor()`, oder wenn Sie möchten zum Anzeigen der Namen der `@Html.EnumDropDownListFor()` zeigt.</span><span class="sxs-lookup"><span data-stu-id="c425e-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="c425e-139">Der folgende Code zeigt diese APIs.</span><span class="sxs-lookup"><span data-stu-id="c425e-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="c425e-140">Sie können ein vollständiges Beispiel finden Sie unter [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="c425e-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="c425e-141">Bootstrap-Unterstützung für Editor-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="c425e-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="c425e-142">Jetzt können wir übergeben in HTML-Attributen in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) als ein [anonymes Objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="c425e-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="c425e-143">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c425e-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="c425e-144">Unaufdringliche Validierung für das MinLengthAttribute und MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="c425e-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="c425e-145">Die clientseitige Validierung für String und Array wird jetzt unterstützt für Eigenschaften mit ergänzt die [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) und [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) Attribute.</span><span class="sxs-lookup"><span data-stu-id="c425e-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="c425e-146">Die "this"-Kontext unterstützen in unaufdringlichen Ajax</span><span class="sxs-lookup"><span data-stu-id="c425e-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="c425e-147">Die Rückruffunktionen (`OnBegin, OnComplete, OnFailure, OnSuccess`) wird jetzt in der Lage, suchen Sie das aufrufende Element über die `this` Kontext.</span><span class="sxs-lookup"><span data-stu-id="c425e-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="c425e-148">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c425e-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="c425e-149">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="c425e-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="c425e-150">Attribut-Routing</span><span class="sxs-lookup"><span data-stu-id="c425e-150">Attribute Routing</span></span>

<span data-ttu-id="c425e-151">Allerdings haben Mehrdeutigkeiten in Attribut-routing-Übereinstimmungen werden jetzt die erste Übereinstimmung auswählen, anstatt einen Fehler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="c425e-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="c425e-152">Attributrouten untersagt die `{controller}` Parameter, und mit der `{action}` Parameter auf Routen für Aktionen platziert.</span><span class="sxs-lookup"><span data-stu-id="c425e-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="c425e-153">Verwendet diese Parameter würde wahrscheinlich zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="c425e-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="c425e-154">Gerüstbau für MVC-Web-API mit 5.1 Pakete führt 5.0-Pakete in ein Projekt für diejenigen, die nicht bereits im Projekt vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="c425e-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="c425e-155">Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.1 RTM wird nicht der Visual Studio-Tools wie z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="c425e-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="c425e-156">Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="c425e-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="c425e-157">Daher wird das Gerüst ASP.NET die vorherige Version (5.0.0.0) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="c425e-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="c425e-158">Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten.</span><span class="sxs-lookup"><span data-stu-id="c425e-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="c425e-159">Wenn Sie ASP.NET-Gerüstbau nach der Aktualisierung der Pakete Ihrer Projekte zu Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen von Web-API und ASP.NET MVC konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="c425e-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="c425e-160">Syntaxhervorhebung für Razor-Ansichten in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c425e-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="c425e-161">Wenn Sie zu ASP.NET MVC 5.1 RTM aktualisieren, ohne zu Visual Studio 2013 zu aktualisieren, werden Sie Visual Studio-Editor-Unterstützung nicht abgerufen werden zur syntaxhervorhebung während der Bearbeitung der Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="c425e-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="c425e-162">Sie benötigen zum Aktualisieren von Visual Studio 2013, um diese Unterstützung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="c425e-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="c425e-163">Typ umbenennen</span><span class="sxs-lookup"><span data-stu-id="c425e-163">Type Renames</span></span>

<span data-ttu-id="c425e-164">Einige der für die Attribut-routing-Erweiterbarkeit verwendeten Typen werden in 5.1 RTM umbenannt.</span><span class="sxs-lookup"><span data-stu-id="c425e-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="c425e-165">**Alter Typname (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="c425e-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="c425e-166">**Neuer Typname (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="c425e-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="c425e-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="c425e-167">IDirectRouteProvider</span></span> | <span data-ttu-id="c425e-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="c425e-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="c425e-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="c425e-169">RouteProviderAttribute</span></span> | <span data-ttu-id="c425e-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="c425e-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="c425e-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="c425e-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="c425e-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="c425e-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="c425e-173">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="c425e-173">Bug Fixes</span></span>

<span data-ttu-id="c425e-174">Diese Version enthält auch mehrere Fehler behoben.</span><span class="sxs-lookup"><span data-stu-id="c425e-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="c425e-175">Sie können hier die vollständige Liste finden:</span><span class="sxs-lookup"><span data-stu-id="c425e-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="c425e-176">5.1.0-Paket</span><span class="sxs-lookup"><span data-stu-id="c425e-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="c425e-177">5.1.1-Paket</span><span class="sxs-lookup"><span data-stu-id="c425e-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="c425e-178">Die 5.1.2 Paket enthält die IntelliSense-Updates, aber keine Programmfehlerbehebungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="c425e-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
