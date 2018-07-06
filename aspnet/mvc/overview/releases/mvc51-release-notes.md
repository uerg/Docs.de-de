---
uid: mvc/overview/releases/mvc51-release-notes
title: Was ist neu in ASP.NET MVC 5.1 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d2e67f64e725e73c3bf9021295da3fe870079a45
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825647"
---
<a name="whats-new-in-aspnet-mvc-51"></a>Neuerungen in ASP.NET MVC 5.1
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt, welche neuerungen für ASP.NET Web MVC 5.1.

- [Softwareanforderungen](#SoftwareRequirements)
- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Features in ASP.NET MVC 5.1](#new-features)

    - [Attribut-routing-Verbesserungen](#AttributeRouting)
    - [Bootstrap-Unterstützung für Editor-Vorlagen](#Bootstrap)
    - [Unterstützung von Enumerationen in Ansichten](#Enum)
    - [Unaufdringliche Validierung für MinLength/MaxLength-Attribute](#Unobtrusive)
    - [Die "this"-Kontext unterstützen in unaufdringlichen Ajax](#thisContext)
- [Bekannte Probleme und Änderungen](#KnownBreakingChanges)- [Fehlerbehebungen](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

- Visual Studio 2012: Herunterladen [ASP.NET und Web Tools 2013.1 für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Herunterladen [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Dieses Update ist erforderlich, für die Bearbeitung von ASP.NET MVC 5.1 Razor-Ansichten.

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht. Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation. Das neueste Paket mit ASP.NET MVC 5.1 RTM hat die folgende Version: "5.1.2". Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.

Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET MVC 5.1 RTM stehen auf der Website für ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Neue Features in ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Attribut-routing-Verbesserungen

 Attributrouting jetzt aktivieren der versionsverwaltung und Header unterstützt Einschränkungen basierend Routenauswahl. Viele Aspekte der attributrouten können jetzt über angepasst werden die `IDirectRouteFactory` Schnittstelle und `RouteFactoryAttribute` Klasse. Das Routenpräfix ist nun erweiterbar, über die `IRoutePrefix` Schnittstelle und `RoutePrefixAttribute` Klasse. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Unterstützung von Enumerationen in Ansichten

1. Neue `@Html.EnumDropDownListFor()` Helper-Methoden. Diese sollte verwendet werden, wie die meisten der HTML-Hilfsprogrammen, mit der Einschränkung, die Auswertung des Ausdrucks muss ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ oder ein [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) , in denen `T` ist ein [Enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) Typ. Verwendung `EnumHelper.IsValidForEnumHelper()` um diese Anforderungen zu überprüfen.
2. Neue `EnumHelper.GetSelectList()` Methoden, die Zurückgeben einer `IList<SelectListItem>`. Dies ist nützlich, wenn Sie eine select-Liste vor dem aufrufen, z. B. bearbeiten müssen `@Html.DropDownListFor()`, oder wenn Sie möchten zum Anzeigen der Namen der `@Html.EnumDropDownListFor()` zeigt.

Der folgende Code zeigt diese APIs.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Sie können ein vollständiges Beispiel finden Sie unter [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Bootstrap-Unterstützung für Editor-Vorlagen

Jetzt können wir übergeben in HTML-Attributen in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) als ein [anonymes Objekt](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Zum Beispiel:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Unaufdringliche Validierung für das MinLengthAttribute und MaxLengthAttribute

Die clientseitige Validierung für String und Array wird jetzt unterstützt für Eigenschaften mit ergänzt die [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) und [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) Attribute.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Die "this"-Kontext unterstützen in unaufdringlichen Ajax

Die Rückruffunktionen (`OnBegin, OnComplete, OnFailure, OnSuccess`) wird jetzt in der Lage, suchen Sie das aufrufende Element über die `this` Kontext. Zum Beispiel:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

### <a name="attribute-routing"></a>Attribut-Routing

Allerdings haben Mehrdeutigkeiten in Attribut-routing-Übereinstimmungen werden jetzt die erste Übereinstimmung auswählen, anstatt einen Fehler gemeldet.

Attributrouten untersagt die `{controller}` Parameter, und mit der `{action}` Parameter auf Routen für Aktionen platziert. Verwendet diese Parameter würde wahrscheinlich zu Mehrdeutigkeiten führen. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Gerüstbau für MVC-Web-API mit 5.1 Pakete führt 5.0-Pakete in ein Projekt für diejenigen, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.1 RTM wird nicht der Visual Studio-Tools wie z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung aktualisiert. Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (5.0.0.0). Daher wird das Gerüst ASP.NET die vorherige Version (5.0.0.0) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind. Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten. Wenn Sie ASP.NET-Gerüstbau nach der Aktualisierung der Pakete Ihrer Projekte zu Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen von Web-API und ASP.NET MVC konsistent sind. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Syntaxhervorhebung für Razor-Ansichten in Visual Studio 2013

Wenn Sie zu ASP.NET MVC 5.1 RTM aktualisieren, ohne zu Visual Studio 2013 zu aktualisieren, werden Sie Visual Studio-Editor-Unterstützung nicht abgerufen werden zur syntaxhervorhebung während der Bearbeitung der Razor-Ansichten. Sie benötigen zum Aktualisieren von Visual Studio 2013, um diese Unterstützung zu erhalten. 

### <a name="type-renames"></a>Typ umbenennen

Einige der für die Attribut-routing-Erweiterbarkeit verwendeten Typen werden in 5.1 RTM umbenannt.

| **Alter Typname (5.1 RC)** | **Neuer Typname (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Fehlerkorrekturen

Diese Version enthält auch mehrere Fehler behoben. Sie können hier die vollständige Liste finden:

- [5.1.0-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

Die 5.1.2 Paket enthält die IntelliSense-Updates, aber keine Programmfehlerbehebungen bereitgestellt.
