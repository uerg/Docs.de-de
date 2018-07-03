---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Was ist neu in ASP.NET MVC 5.2 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: bcffaa9fddfb13db0b7bd203029e850903a13a54
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389953"
---
<a name="whats-new-in-aspnet-mvc-52"></a>Neuerungen in ASP.NET MVC 5.2
====================
durch [Microsoft](https://github.com/microsoft)

In diesem Thema wird beschrieben, neues für ASP.NET MVC 5.2, ["Microsoft.Aspnet.Mvc" 5.2.2](#52) und [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Softwareanforderungen](#softRequire)
- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Features in ASP.NET MVC 5.2](#new-features)

    - [Attribut-routing-Verbesserungen](#attributerouting)
- [Bekannte Probleme und aktueller Änderungen](#knownbreakingchanges)
- [Fehlerbehebungen](#bug-fixes)
- ["Microsoft.Aspnet.Mvc" 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Softwareanforderungen

- Visual Studio 2012: Herunterladen [ASP.NET und Web Tools 2013.1 für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Herunterladen [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) oder höher. Dieses Update ist erforderlich, für die Bearbeitung von ASP.NET MVC 5.2 Razor-Ansichten.

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht. Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation. Das neueste Paket mit ASP.NET MVC 5.2 hat die folgende Version: "5.2.0". Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.

Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:

Install-Package "Microsoft.Aspnet.Mvc"-Version 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET MVC 5.2 stehen auf der Website für ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Neue Features in ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Attribut-routing-Verbesserungen

Attributrouting jetzt bietet einen Erweiterungspunkt aufgerufen IDirectRouteProvider, die ermöglicht die vollständige Kontrolle darüber, wie attributrouten erkannt und konfiguriert werden. Ein IDirectRouteProvider ist verantwortlich für die Angabe einer Liste von Aktionen und Controller zusammen mit zugehörigen Routeninformationen an genau welche Routingkonfiguration für diese Aktionen erforderlich ist. Eine IDirectRouteProvider-Implementierung kann angegeben werden, wenn MapAttributes/MapHttpAttributeRoutes aufrufen.

Anpassen von IDirectRouteProvider werden am einfachsten durch die Erweiterung unserer Standardimplementierung DefaultDirectRouteProvider. Diese Klasse stellt separate überschreibbare virtuelle Methoden, um die Logik zum Ermitteln von Attributen, routeneinträge erstellen und Ermitteln von Routenpräfix und Bereichspräfix zu ändern.

Mit der das neue Attribut routing-Erweiterbarkeit von IDirectRouteProvider kann ein Benutzer Folgendes tun:

1. Vererbung von attributrouten zu unterstützen. Z. B. im folgenden Szenario verwenden Blog "und" Store-Controller eine Route attributkonvention, die durch die BaseController definiert wird. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Routennamen für attributrouten automatisch zu generieren. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Ändern Sie routenpräfixe an einem zentralen Ort aus, bevor die Routen der Routentabelle hinzugefügt werden.
4. Herausfiltern Sie, die Controller, die Sie auf das attributrouting, die gesucht. Wir hoffen, bald zum Blog auf 3 und 4.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook-Korrekturen für geänderte API-Oberfläche

Das MVC-Facebook-Paket [wurde unterbrochen,](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) aufgrund einige API-Änderungen durch Facebook. Außerdem veröffentlichen wir ein neues Facebook-Paket (Microsoft.AspNet.Facebook 1.0.0), um diese Probleme zu beheben.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Gerüstbau MVC-Web-API in einem Projekt mit 5.2.0 Pakete führt 5.1.2 Pakete, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.2.0 aktualisiert nicht Visual Studio-Tools wie z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung. Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (z. B. 5.1.2 in Update 2). Daher wird das Gerüst ASP.NET die vorherige Version (z. B. 5.1.2 in Update 2) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind. Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten. Bei Verwendung von ASP.NET-Gerüstbau nach der Aktualisierung der Pakete Ihrer Projekte zu Web-API 2.2 oder ASP.NET MVC 5.2 Stellen Sie sicher, dass die Versionen von Web-API und ASP.NET MVC konsistent sind.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Microsoft.jQuery.Unobtrusive.Validation NuGet-Paketinstallation schlägt fehl, da es nicht auf eine Version von Microsoft.jQuery.Unobtrusive.Validation kompatibel. die jQuery-1.4.1 suchen

Microsoft.jQuery.Unobtrusive.Validation erfordert jQuery &gt;= 1.8 und jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) benötigt jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Aus diesem Grund bei NuGet die JQuery-1.8 und jQuery.Validation 1.8 gleichzeitig installiert, schlägt sie fehl. Wenn Sie dieses Problem auftritt, können Sie einfach aktualisieren, auf die Version des jQuery.Validation zu &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) IValidator.h feste Obergrenze für das jQuery zuerst, Sie sollten möglicherweise installieren Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Die Jquery. Einige internationale e-Mail-Adressen erkennt Überprüfung Nuget-Paketversion 1.13.0 nicht.

jQuery.Validation Nuget-Paketversion 1.11.1 ist die letzte bekannte Version, die erkennt, folgt gültige e-Mail-Adressen. Alle höheren Versionen kann möglicherweise nicht erkannt. Zum Beispiel:

E-Mail-Adresse Internationalisierung (EAI) Standard (z. B. [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + internationaler Resource Identifiers (IRIs) (z. b., [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Unter wird das Problem gemeldet. [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Syntaxhervorhebung für Razor-Ansichten in Visual Studio 2013

Wenn Sie zu ASP.NET MVC 5.2 aktualisieren, ohne zu Visual Studio 2013 zu aktualisieren, werden Sie Visual Studio-Editor-Unterstützung nicht abgerufen werden zur syntaxhervorhebung während der Bearbeitung der Razor-Ansichten. Sie benötigen zum Aktualisieren von Visual Studio 2013, um diese Unterstützung zu erhalten.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Fehlerbehebungen und kleinere Feature-updates

Diese Version umfasst auch mehrere Fehlerbehebungen und kleinere Feature Updates. Sie können hier die vollständige Liste finden:

- [5.2-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>"Microsoft.Aspnet.Mvc" 5.2.2

Diese Version keine neuen Features oder Programmfehlerbehebungen in MVC. Wir haben eine [in Webseiten ändern](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) für eine beträchtliche Leistungssteigerung darstellen und aktualisiert alle anderen abhängigen Pakete, die wir besitzen, um auf diese neue Version von Web Pages abhängig sind.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Informieren Sie sich über die Version [hier](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Diese Version enthält nur Fehler behoben. Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) um die Liste der in diesem Release behobene Probleme anzuzeigen.
