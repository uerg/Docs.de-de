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
<a name="whats-new-in-aspnet-mvc-51"></a>Was ist neu in ASP.NET MVC 5.1
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt die Neuigkeiten für ASP.NET Web MVC 5.1.

- [Softwareanforderungen](#SoftwareRequirements)
- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Funktionen in ASP.NET MVC 5.1](#new-features)

    - [-Attribut routing Verbesserungen](#AttributeRouting)
    - [Bootstrap-Unterstützung für Editor-Vorlagen](#Bootstrap)
    - [Enum-Unterstützung in den Ansichten](#Enum)
    - [Unaufdringlichen Überprüfung für MinLength/MaxLength-Attribute](#Unobtrusive)
    - [Unterstützung von 'this' Kontext in der unaufdringlichen Ajax](#thisContext)
- [Bekannte Probleme und Änderungen, die die](#KnownBreakingChanges)- [Fehlerkorrekturen](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

- Visual Studio 2012: Herunterladen [ASP.NET- und Webdienst-Tools für Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Herunterladen [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Dieses Update ist erforderlich, für die Bearbeitung von ASP.NET MVC 5.1 Razor-Ansichten.

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung. Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation. Das aktuellste Paket von ASP.NET MVC 5.1 RTM wurde die folgende Version: "5.1.2". Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.

Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET MVC 5.1 RTM sind von der Website für ASP.NET (https://www.asp.net) verfügbar. 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Neue Funktionen in ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>-Attribut routing Verbesserungen

 Routing jetzt unterstützt Einschränkungen, versionsverwaltung und Header aktivieren Attribut basierten Routenauswahl. Viele Aspekte der attributenrouten sind jetzt über anpassbare der `IDirectRouteFactory` Schnittstelle und `RouteFactoryAttribute` Klasse. Das Routenpräfix ist jetzt erweiterbar ist, über die `IRoutePrefix` Schnittstelle und `RoutePrefixAttribute` Klasse. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Enum-Unterstützung in den Ansichten

1. Neue `@Html.EnumDropDownListFor()` Hilfsmethoden. Diese sollte verwendet werden, wie die meisten der HTML-Hilfsmethoden, mit der Einschränkung, die Auswertung des Ausdrucks muss ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ oder eine [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) , in dem `T` ist ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ. Verwendung `EnumHelper.IsValidForEnumHelper()` um diese Anforderungen zu überprüfen.
2. Neue `EnumHelper.GetSelectList()` Methoden, die Zurückgeben einer `IList<SelectListItem>`. Dies ist nützlich, wenn Sie eine select-Liste vor dem aufrufen, z. B. bearbeiten müssen `@Html.DropDownListFor()`, oder wenn die Namen angezeigt werden sollen die `@Html.EnumDropDownListFor()` zeigt.

Der folgende Code zeigt diese APIs.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Sie können ein vollständiges Beispiel finden Sie unter [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Bootstrap-Unterstützung für Editor-Vorlagen

Jetzt können wir übergeben in HTML-Attribute in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) als ein [anonymes Objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Zum Beispiel:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Unaufdringlichen Validierung MinLengthAttribute und MaxLengthAttribute

Die clientseitige Validierung für Strings und Arrays von Typen wird jetzt unterstützt, für Eigenschaften mit ergänzt die ["minLength"](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) und [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) Attribute.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Unterstützung von 'this' Kontext in der unaufdringlichen Ajax

Die Rückruffunktionen (`OnBegin, OnComplete, OnFailure, OnSuccess`) wird nun in der Lage, suchen Sie das aufrufende Element über den `this` Kontext. Zum Beispiel:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

### <a name="attribute-routing"></a>Routing-Attribut

Mehrdeutigkeiten im Attribut routing Übereinstimmungen meldet jetzt eine Fehlermeldung statt die erste Übereinstimmung auswählen.

Attributenrouten sind nicht zulässig, von der Verwendung der `{controller}` Parameter, und von der Verwendung der `{action}` Parameter auf die Routen zu Aktionen platziert. Dieser Parameter wird würde sehr wahrscheinlich zu Mehrdeutigkeiten führen. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Gerüstbau MVC/Web-API in einem Projekt mit 5.1 Pakete-Ergebnissen in 5.0-Paketen für Argumente, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.1 RTM wird nicht Visual Studio-Tools wie z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung aktualisiert. Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (5.0.0.0). Daher wird das Gerüst ASP.NET die Vorgängerversion (5.0.0.0) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind. Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben. Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren die Pakete der Projekte, auf die Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen der Web-API und ASP.NET MVC konsistent sind. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Syntaxhervorhebung für Razor-Ansichten in Visual Studio 2013

Wenn Sie in ASP.NET MVC 5.1 RTM aktualisieren, ohne Visual Studio 2013 zu aktualisieren, werden Sie Unterstützung von Visual Studio-Editor nicht abgerufen werden zur syntaxhervorhebung beim Bearbeiten der Razor-Ansichten. Sie müssen zum Aktualisieren von Visual Studio 2013, um diese Unterstützung zu erhalten. 

### <a name="type-renames"></a>Umbenennen von Typ

Einige der für die Attribut-routing-Erweiterbarkeit verwendeten Typen sind in 5.1 RTM umbenannt.

| **Alter Typname (5.1 RC)** | **Neuer Typname (5.1-RTM-Version)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Fehlerkorrekturen

Diese Version enthält auch verschiedene Fehlerbehebungen. Sie können die vollständige Liste hier finden:

- [5.1.0-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

Die 5.1.2 Paket enthält, aber keine Fehlerkorrekturen IntelliSense-Updates.
