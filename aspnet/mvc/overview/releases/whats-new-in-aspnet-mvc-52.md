---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Neues in ASP.NET MVC 5.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>Was ist neu in ASP.NET MVC 5.2
====================
durch [Microsoft](https://github.com/microsoft)

In diesem Thema wird beschrieben, mit ASP.NET MVC 5.2 Neuigkeiten [Microsoft.AspNet.MVC 5.2.2](#52) und [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Softwareanforderungen](#softRequire)
- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Funktionen in ASP.NET MVC 5.2](#new-features)

    - [-Attribut routing Verbesserungen](#attributerouting)
- [Bekannte Probleme und aktueller Änderungen](#knownbreakingchanges)
- [Fehlerkorrekturen](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Softwareanforderungen

- Visual Studio 2012: Herunterladen [ASP.NET- und Webdienst-Tools für Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Herunterladen [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) oder höher. Dieses Update ist erforderlich, für die Bearbeitung von ASP.NET MVC-5.2-Razor-Ansichten.

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung. Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation. Das aktuellste Paket von ASP.NET MVC 5.2 hat die folgende Version: "5.2.0". Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.

Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:

Install-Package Microsoft.AspNet.Mvc-Version 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET MVC 5.2 stehen auf der Website für ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Neue Funktionen in ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>-Attribut routing Verbesserungen

Routing-Attribut jetzt bietet einen Erweiterungspunkt aufgerufen IDirectRouteProvider, die vollständige Kontrolle über wie attributenrouten erkannt und konfiguriert werden kann. Ein IDirectRouteProvider ist verantwortlich für die Bereitstellung einer Liste von Aktionen und Controller zusammen mit zugehörigen Routeninformationen an genau welche Routingkonfiguration für diese Aktionen gewünscht wird. Eine Implementierung IDirectRouteProvider kann angegeben werden, wenn MapAttributes/MapHttpAttributeRoutes aufrufen.

Anpassen von IDirectRouteProvider werden einfachste durch unsere Standardimplementierung DefaultDirectRouteProvider erweitern. Diese Klasse stellt separate überschreibbare virtuelle Methoden, um die Logik zum Erkennen von Attribute, routeneinträge erstellen und durch die Ermittlung Routenpräfix und Bereichspräfix zu ändern.

Mit der neuen routing Erweiterbarkeit von Attribut des IDirectRouteProvider könnte ein Benutzer Folgendes ausführen:

1. Vererbung von attributenrouten zu unterstützen. Beispielsweise werden Blog und Store-Controller im folgenden Szenario eine Attribut-Route-Konvention verwendet, die durch die BaseController definiert ist. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Automatisches Generieren von Routennamen für attributenrouten. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Ändern Sie routenpräfixe an einem zentralen Ort aus, bevor die Routen der Routingtabelle hinzugefügt werden.
4. Filtern Sie die Controller, die Sie auf der routing-Attribut gesucht. Wir hoffen bald, Blog auf 3 und 4.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook behebt für geänderte API-Oberfläche

Das MVC-Facebook-Paket [wurde unterbrochen,](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) aufgrund einige API-Änderungen durch Facebook. Wir präsentieren auch ein neues Facebook-Paket (Microsoft.AspNet.Facebook 1.0.0), um diese Probleme zu beheben.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Gerüstbau MVC/Web-API in einem Projekt mit 5.2.0 Pakete führt 5.1.2 Pakete für Argumente, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.2.0 aktualisiert nicht Visual Studio-Tools wie z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung. Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (z. B. 5.1.2 in Update 2). Daher wird das Gerüst ASP.NET die Vorgängerversion (z. B. 5.1.2 in Update 2) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind. Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben. Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren die Pakete der Projekte auf Web-API 2.2 oder ASP.NET MVC 5.2 verwenden, stellen Sie sicher, dass die Versionen der Web-API und ASP.NET MVC konsistent sind.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Microsoft.jQuery.Unobtrusive.Validation NuGet-Paket-Installation schlägt fehl, weil sie nicht, eine Version des Microsoft.jQuery.Unobtrusive.Validation kompatibel jQuery 1.4.1 finden kann

Microsoft.jQuery.Unobtrusive.Validation erfordert jQuery &gt;= 1.8 und jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1,8) benötigt jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Aus diesem Grund bei NuGet die JQuery-1.8 und jQuery.Validation 1.8 zur gleichen Zeit installiert schlägt sie fehl. Wenn dieses Problem angezeigt wird, können Sie einfach die Version des jQuery.Validation zu aktualisieren &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) verfügt über die jQuery-Cap festen zuerst, Sie sollten Lage installieren Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Einige internationale e-Mail-Adressen erkannt Überprüfung Version des NuGet-Pakets 1.13.0 nicht.

jQuery.Validation Version des NuGet-Pakets 1.11.1 ist die letzte bekannte Version, die erkennt, befolgen gültige e-Mail-Adressen. Alle späteren Versionen möglicherweise nicht erkannt werden können. Zum Beispiel:

E-Mail-Adresse Internationalisierung (EAI) Standard (z. B. [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized Resource Identifiers (IRI) (z. b., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Das Problem wird gemeldet, am [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Syntaxhervorhebung für Razor-Ansichten in Visual Studio 2013

Wenn Sie in ASP.NET MVC 5.2 aktualisieren, ohne Visual Studio 2013 zu aktualisieren, werden Sie Unterstützung von Visual Studio-Editor nicht abgerufen werden zur syntaxhervorhebung beim Bearbeiten der Razor-Ansichten. Sie müssen zum Aktualisieren von Visual Studio 2013, um diese Unterstützung zu erhalten.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Fehlerbehebungen und kleinere Feature-updates

Diese Version enthält auch mehrere Programmfehlerbehebungen und kleinere Funktion Updates. Sie können die vollständige Liste hier finden:

- [5.2-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Diese Version haben keine neuen Funktionen oder bei Fehlerbehebungen in MVC. Wir haben eine [ändern, die in Webseiten](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) für eine beträchtliche Leistungssteigerung darstellen und aktualisiert anschließend alle anderen abhängigen Pakete, die wir besitzen, um diese neue Version von Webseiten abhängig.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Informieren Sie sich über die Version [hier](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Diese Version enthält nur Fehlerkorrekturen. Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) auf die Liste der in dieser Version behobene Probleme anzuzeigen.
