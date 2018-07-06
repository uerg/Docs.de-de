---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft-Dokumentation
author: rick-anderson
description: (April 2011 umfasst Tools Update) ASP.NET MVC 3 ist ein Framework zum Erstellen von skalierbaren, auf Standards basierende Webanwendungen, die bewährte Entwurfsmuster mit...
ms.author: aspnetcontent
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 2c2d53734cd62e59ee4f550713b0576a73b0e32b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834769"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(April 2011 umfasst Tools Update)*
> 
> ASP.NET MVC 3 ist ein Framework zum Erstellen von skalierbaren, auf Standards basierende Webanwendungen mit bewährte Entwurfsmuster und die Leistung von ASP.NET und .NET Framework.
> 
> Die Installation erfolgt Seite-an-Seite mit ASP.NET MVC 2, damit sie noch heute mit beginnen!
> 
> Herunterladen der [hier Installer](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Wichtige Features

- Integrierte gerüstsystem, die über NuGet erweiterbar
- HTML 5-aktivierten Projektvorlagen
- Ausdrucksbasierte Ansichten, einschließlich der neuen Razor-Ansichtsengine
- Leistungsstarke Hooks mit Dependency Injection und globale Aktionsfilter
- Rich-JavaScript-Unterstützung mit unaufdringliches JavaScript, jQuery-Validierung und JSON-Bindung
- *Lesen die vollständigen Liste [unten](#overview)*

## <a name="top-links"></a>Oben Links

Neuerungen in ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3-Veröffentlichung](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express und Orchard veröffentlicht – das Microsoft Januar Webversion im Kontext](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [Ankündigung der Veröffentlichung von ASP.NET MVC 3, IIS Express, SQL CE-4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Anmerkungen zu dieser Version für ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installation und Hilfe

- Installieren Sie ASP.NET MVC 3 mithilfe der [Webplattform-Installer (empfohlen)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installieren Sie ASP.NET MVC 3 mithilfe der [ausführbaren Installer](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installieren Sie [ASP.NET MVC 3 für Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lesen der [Einführung in ASP.NET MVC 3-Tutorial](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Hier erhalten Sie Hilfe und Erläutern Sie ASP.NET MVC 3 in der [Foren](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 – Übersicht

ASP.NET MVC 3 baut auf ASP.NET MVC 1 und 2, Hinzufügen von hervorragenden Funktionen, die Ihren Code vereinfachen und umfassendere Erweiterbarkeit ermöglichen. Dieses Thema enthält eine Übersicht über viele der neuen Features, die in dieser Version, die in den folgenden Abschnitten organisiert enthalten sind:

- [Erweiterbare Gerüstbau mit MvcScaffold-integration](#BM_MvcScaffolding)
- [HTML 5-aktivierten Projektvorlagen](#BM_HTML5)
- [Die Razor-Ansichtsengine](#BM_TheRazorViewEngine)
- [Unterstützung für mehrere Ansichts-Engines](#BM_Support_for_Multiple_View_Engines)
- [Controller-Verbesserungen](#BM_Controller_Improvements)
- [JavaScript und Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Verbesserungen für die Validierung](#BM_Model_Validation_Improvements)
- [Dependency Injection-Verbesserungen](#BM_Dependency_Injection_Improvements)
- [Weitere neue Features](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Erweiterbare Gerüstbau mit MvcScaffold-integration

Das neue gerüstsystem erleichtert es auswählen und produktiv verwenden, wenn Sie vollständig mit dem Framework vertraut sind und so die allgemeine Entwicklungsaufgaben automatisieren, wenn Sie Erfahrung haben und wissen bereits, was Sie tun.

Dies wird vom neuen NuGet unterstützt *Gerüstbau* Paket namens **MvcScaffolding**. Der Begriff "Gerüstbau" von vielen softwaretechnologien verwendet wird, bedeutet "durch schnelle Generieren von beschreibt die grundlegenden Schritte der Software, die Sie bearbeiten und anpassen". Die Gerüstbau-Paket, das wir für ASP.NET MVC erstellen ist in verschiedenen Szenarien sinnvoll:

- **Wenn Sie ASP.NET MVC zum ersten Mal vertraut**, denn es ermöglicht Ihnen eine schnelle Möglichkeit zum nützlich, funktionierenden Code abzurufen, die Sie bearbeiten und gemäß Ihren Anforderungen anpassen können. Es erspart Ihnen die nbsp eine leere Seite betrachten und müssen nicht wissen, wo Sie anfangen!
- **Wenn Sie ASP.NET MVC gut kennen und werden jetzt einige neue-Add-On-Technologie Durchsuchen** z. B. eine objektrelationale Zuordnung, die eine Ansichts-Engine, die eine Tests-Bibliothek, usw., da der Ersteller des dieser Technologie auch ein Gerüst-Paket für sie erstellt haben, kann.
- **Wenn Ihre Arbeit umfasst das Erstellen von wiederholt ähnliche Klassen oder Dateien irgendeiner Art**, da Sie benutzerdefinierter gerüstbauer erstellen können, die ausgegeben wird, prüfvorrichtungen, Bereitstellungsskripts oder was auch immer Sie benötigen. Allen Mitgliedern Ihres Teams kann Ihr benutzerdefinierter gerüstbauer, zu verwenden.

Andere Features in MvcScaffolding sind:

- Unterstützung für c# und VB-Projekte
- Unterstützung für den Razor und die ASPX-Engines anzeigen
- Unterstützt in ASP.NET MVC-Bereiche der Gerüstbau und Verwenden von benutzerdefinierten Ansicht Layouts/Master
- Sie können die Ausgabe ganz einfach anpassen, indem Sie die Bearbeitung von T4-Vorlagen
- Sie können völlig neue scaffolder mithilfe benutzerdefinierter PowerShell-Logik und benutzerdefinierte T4-Vorlagen hinzufügen. Diese (und beliebige benutzerdefinierte Parameter, die Sie diese gegeben haben) automatisch in die Konsole-Liste angezeigt.
- Sie erhalten die NuGet-Pakete, die mit zusätzlichen scaffolder für verschiedene Technologien (z. B. besteht eine Proof of Concept-eine für LINQ to SQL jetzt) und vermischen sie zusammen

ASP.NET MVC 3 Tools Update enthält hervorragende Visual Studio-Unterstützung für diese gerüstsystem, z. B.:

- Fügen Sie, dass die Controller-Dialogfeld unterstützt jetzt die vollständige automatische Gerüstbau für Create, Read, Update und Delete-Controlleraktionen und die entsprechenden Ansichten hinzu. Standardmäßig erstellt das Gerüst für dieses Code für den Datenzugriff mithilfe von EF Code First.
- Hinzufügen von Domänencontroller-Dialogfeld unterstützt *extensible Gerüste* über NuGet-Pakete wie z. B. *MvcScaffolding*. Dadurch wird unter Verwendung der benutzerdefinierten Gerüste in das Dialogfeld ermöglicht Ihnen die Erstellung Gerüste für andere datenzugriffstechnologien wie NHibernate oder sogar JET mit ODBCDirect, wenn Sie Interesse nachlesen können.

Weitere Informationen zu den Gerüstbau in ASP.NET MVC 3 finden Sie unter den folgenden Ressourcen:

- Steve Sandersons post-Serie, einschließlich: 

    1. [Einführung: Erstellen des Gerüsts für des ASP.NET MVC 3-Projekts mit dem MvcScaffolding-Paket](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standardverwendung: Typische Anwendungsfälle und Optionen](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [1: N Beziehungen](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Gerüstbau Gerüstbauaktionen und Komponententests](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Überschreiben die T4-Vorlagen](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Dieser Beitrag: Erstellen benutzerdefinierter gerüstbauer](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselmans Post von seiner Sitzung PDC 2010 [erstellen einen Blog mit Microsoft "Unbenannt-Paket der Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Anmerkungen zur Version von MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5-Projektvorlagen

Das Dialogfeld "Neues Projekt" enthält ein Kontrollkästchen aktivieren, HTML 5-Versionen der Projektvorlagen das Projekt. Diese Vorlagen nutzen Modernizr 1.7 um Kompatibilität für HTML5 und CSS 3 in kompatiblen Browsern zu unterstützen.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Die Razor-Ansichtsengine

Im Lieferumfang von ASP.NET MVC 3 ist einer neue Anzeige-Engine mit dem Namen Razor, die die folgenden Vorteile bietet:

- Razor-Syntax ist übersichtliches und, erfordern eine Mindestanzahl von Tastatureingaben.
- Razor ist einfach, teilweise erfahren, da es auf vorhandene Sprachen wie c# und Visual Basic basiert.
- Visual Studio bietet IntelliSense und die farbliche Kennzeichnung für Razor-Syntax.
- Razor-Ansichten möglich Komponententests ausgeführt werden, ohne dass Sie die Anwendung ausführen oder einen Webserver zu starten.

Einige der neuen Razor-Features umfassen Folgendes:

- `@model` Die Syntax zum Angeben des Typs, der an die Ansicht übergeben wird.
- `@* *@` Kommentarsyntax.
- Die Möglichkeit, Standardwerte anzugeben (z. B. `layoutpage`) einmal für eine gesamte Site.
- Die `Html.Raw` -Methode zum Anzeigen von Text ohne HTML-Codierung es.
- Unterstützung für das Freigeben von Code für mehrere Ansichten (*\_viewstart.cshtml* oder  *\_viewstart.vbhtml* Dateien).

Razor enthält auch neue HTML-Hilfsprogramme, z. B. Folgendes an:

- `Chart` Rendert ein Diagramm bietet die gleichen Funktionen wie das Diagrammsteuerelement in ASP.NET 4.
- `WebGrid` Rendert ein Datenraster, paging und Sortierung Funktionen an.
- `Crypto` Verwendet, die Hashalgorithmen, um ordnungsgemäß zu erstellen, mit Salt-Wert und Kennwörter gehasht.
- `WebImage` Rendert ein Bild an.
- `WebMail` Sendet eine E-Mail.

Weitere Informationen zu Razor finden Sie unter den folgenden Ressourcen:

- [Scott Guthries Blogbeitrag Einführung in Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Einführung in Scott Guthries Blog-Beitrag der @model Schlüsselwort](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthries Blog-Beitrag Einführung in Razor-layouts](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor-API-Kurzreferenz](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Anmerkungen zur Version von MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Unterstützung für mehrere Ansichts-Engines

Die **Ansicht hinzufügen** Dialogfeld in ASP.NET MVC 3 können Sie die Ansichts-Engine auswählen, mit dem Sie arbeiten möchten, und die **neues Projekt** Dialogfeld können Sie die standardansichts-Engine für ein Projekt angeben. Sie können die Web Forms-Ansichts-Engine (ASPX), Razor oder eine Open-Source-Ansichtsmodul wie z. B. [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), oder [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Controller-Verbesserungen

### <a name="global-action-filters"></a>Globale Aktionsfilter

Gelegentlich möchten Sie Logik ausführen, bevor eine Aktionsmethode ausgeführt wird oder nach einer Aktionsmethode ausgeführt wird. Um dies zu unterstützen, bereitgestellt, ASP.NET MVC 2 Aktionsfilter verwendet werden. Aktionsfilter sind benutzerdefinierte Attribute, die ein deklaratives Mittel hinzuzufügende einfügen vor und nach Abschluss der Aktion Verhalten für bestimmte Controlleraktionsmethoden bereitstellen. In einigen Fällen möchten jedoch möglicherweise vorausgehende Aktion oder nach Abschluss der Aktion Verhalten anzugeben, die für alle Aktionsmethoden angewendet wird. MVC 3 können Sie die globale Filter angeben, indem Sie sie zum Hinzufügen der `GlobalFilters` Auflistung. Weitere Informationen zu globalen Aktionsfilter verwendet werden finden Sie unter den folgenden Ressourcen:

- [Scott Guthries Blog auf das MVC-3 (Vorschau)](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtern in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Neue "ViewBag"-Eigenschaft

Unterstützung von MVC 2-Controller eine `ViewData` -Eigenschaft, die Ihnen ermöglicht, Daten an eine ansichtsvorlage mithilfe eines Wörterbuchs spät gebundene API übergeben. In MVC 3, können Sie auch etwas einfacher Syntax mit dem `ViewBag` Eigenschaft, um den gleichen Zweck zu erreichen. Beispielsweise anstelle des Schreibens von `ViewData["Message"]="text"`, können Sie schreiben `ViewBag.Message="text"`. Sie müssen keine stark typisierten Klassen mit definieren die `ViewBag` Eigenschaft. Da es sich um eine dynamische Eigenschaft handelt, können Sie stattdessen einfach abrufen oder Festlegen von Eigenschaften und lösen sie dynamisch zur Laufzeit. Intern `ViewBag` Eigenschaften gespeichert werden, als Name/Wert-Paare in der `ViewData` Wörterbuch. (Hinweis: in den meisten Vorabversionen von MVC 3, die `ViewBag` Eigenschaft wurde mit dem Namen der `ViewModel` Eigenschaft.)

### <a name="new-actionresult-types"></a>Neue "ActionResult"-Typen

Die folgenden `ActionResult` Typen und zugehörigen Hilfsmethoden werden neue oder verbesserte in MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Gibt eine HTTP-Statuscode 404 an den Client zurück.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Gibt eine temporäre Umleitung (HTTP 302 Status-Code) oder eine permanente Umleitung (HTTP 301-Statuscode), je nach einem booleschen Parameter zurück. In Verbindung mit dieser Änderung der [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) -Klasse verfügt jetzt über drei Methoden zum Ausführen von dauerhafte umleitungen: `RedirectPermanent`, `RedirectToRoutePermanent`, und `RedirectToActionPermanent`. Diese Methoden zurückgeben eine Instanz von `RedirectResult` mit der `Permanent` -Eigenschaftensatz auf `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Gibt einen benutzerdefinierten HTTP-Statuscode zurück.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript und Ajax-Verbesserungen

Standardmäßig verwenden Ajax und Validierung Hilfsprogramme in MVC 3 einen unaufdringlichen JavaScript-Ansatz. Unaufdringliches JavaScript wird vermieden, Inline-JavaScript in HTML einzufügen. Dies macht den HTML-Code kleiner und weniger überladen und erleichtert es, auszutauschen, oder passen Sie die JavaScript-Bibliotheken. Überprüfung-Hilfsprogramme in MVC 3 auch verwenden, die `jQueryValidate` -Plug-in in der Standardeinstellung. Wenn Sie MVC 2-Verhalten soll, können Sie deaktivieren, unaufdringliches JavaScript verwenden eine *"Web.config"* Datei festlegen. Weitere Informationen über JavaScript und Ajax-Verbesserungen finden Sie unter den folgenden Ressourcen:

- [Einführung in das unaufdringliches JavaScript auf der Wikipedia-Website](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilsons unaufdringliches JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilsons unaufdringliches JavaScript-Validierung Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (Lernprogramm auf der ASP.NET-Website)
- [Anmerkungen zur Version von MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Die clientseitige Validierung, die standardmäßig aktiviert

In früheren Versionen von MVC, müssen Sie explizit aufrufen, die `Html.EnableClientValidation` Methode aus einer Sicht, um die clientseitige Validierung zu aktivieren. In MVC 3 ist dies nicht mehr erforderlich, da die clientseitige Validierung standardmäßig aktiviert ist. (Deaktivieren Sie diese Option mit einer Einstellung in der *"Web.config"* Datei.)

Damit können die clientseitige Validierung funktioniert müssen Sie weiterhin auf den entsprechenden jQuery und jQuery-Validierung-Bibliotheken auf Ihrer Website verweisen. Sie können diese Bibliotheken auf Ihrem eigenen Server zu hosten oder Verweis auf ein Content Delivery Network (CDN), wie die CDNs von Microsoft oder Google.

### <a name="remote-validator"></a>Die Remotebestätigung

ASP.NET MVC 3 unterstützt den neuen [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) -Klasse, die Ihnen ermöglicht, die jQuery-Validierung-Plug-ins nutzen Sie die remotebestätigung-Unterstützung. Dadurch wird die Bibliothek die clientseitige Validierung automatisch serverseitige Aufrufen eine benutzerdefinierte Methode, die Sie auf dem Server zu definieren, um die Validierungslogik ausgeführt werden, die nur ausgeführt werden können.

Im folgenden Beispiel die `Remote` Attribut gibt an, dass die Clientvalidierung eine Aktion, die mit dem Namen aufruft `UserNameAvailable` auf die `UsersController` Klasse zum Überprüfen der `UserName` Feld.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Das folgende Beispiel zeigt den entsprechenden Controller an.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Weitere Informationen zur Verwendung der `Remote` Attribut, finden Sie unter [Vorgehensweise: Implementieren der Remotevalidierung in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in der MSDN Library.

### <a name="json-binding-support"></a>Bindungsunterstützung für das JSON-

ASP.NET MVC 3 unterstützt integrierte JSON-Bindung, die Aktionsmethoden anwenden, um JSON-codierten Daten empfangen und Modell binden sie Aktionsmethodenparameter ermöglicht. Diese Funktion ist nützlich in Szenarien, die im Zusammenhang mit Client-Vorlagen und Datenbindung. (Client-Vorlagen können zum Formatieren und Anzeigen von ein einzelnes Datenelement oder ein Satz von Datenelementen mithilfe von Vorlagen, die auf dem Client ausgeführt werden.) MVC 3 können Sie ganz einfach Clientvorlagen mit Aktionsmethoden, auf dem Server verbinden, das Senden und Empfangen von JSON-Daten. Weitere Informationen zur Unterstützung von JSON-Bindung finden Sie unter den **JavaScript und AJAX-Verbesserungen** Abschnitt [Blogbeitrag von Scott Guthries MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Verbesserungen für die Validierung

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations" Metadaten-Attributen

ASP.NET MVC 3 unterstützt `DataAnnotations` Metadaten Attribute wie `DisplayAttribute`.

### <a name="validationattribute-class"></a>"ValidationAttribute"-Klasse

Die `ValidationAttribute` Klasse wurde verbessert, in der .NET Framework 4, um ein neues unterstützen `IsValid` Überladung, die Informationen zum aktuellen Validierungskontext, z. B. welches Objekt überprüft wird. Dies ermöglicht umfangreichere Szenarien, in dem Sie den aktuellen Wert basierend auf einer anderen Eigenschaft des Modells überprüfen können. Beispielsweise ist die neue `CompareAttribute` -Attributs können Sie die Werte von zwei Eigenschaften eines Modells zu vergleichen. Im folgenden Beispiel die `ComparePassword` Eigenschaft muss übereinstimmen. die `Password` Feld, damit es gültig ist.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Überprüfung von Schnittstellen

Die [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) Schnittstelle können Sie für die Validierung der Modellebene und können Sie Überprüfung Fehlermeldungen bereitzustellen, die in den Zustand des gesamten Modells oder zwischen zwei Eigenschaften innerhalb des Modells beziehen . MVC 3 ruft nun Fehler von der `IValidatableObject` -Schnittstelle bei der modellbindung und automatisch Kennzeichen oder Highlights Felder innerhalb einer Ansicht mithilfe der integrierten HTML-Formular-Hilfsprogramme betroffen.

Die [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) Schnittstelle ermöglicht, ASP.NET MVC, um zur Laufzeit zu ermitteln, ob ein Validierungssteuerelement Unterstützung für Clientvalidierung verfügt. Diese Schnittstelle wurde entwickelt, damit es mit einer Vielzahl von prüfungsframeworks integriert werden kann.

Weitere Informationen zu den Schnittstellen der Validierung finden Sie unter den **Verbesserungen für die Validierung** Abschnitt [Blogbeitrag von Scott Guthries MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Beachten Sie jedoch, dass der Verweis auf "IValidateObject" im Blog "IValidatableObject" werden soll.)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Dependency Injection-Verbesserungen

ASP.NET MVC 3 bietet bessere Unterstützung für Dependency Injection (DI) anwenden und die Integration von in Containern Dependency Injection oder Inversion des Control (IOC). Unterstützung für DI wurde in den folgenden Bereichen hinzugefügt:

- Controller ("registrieren" und "Einfügen von Controllerfactorys, Controller einfügen").
- Sichten (registrieren und Ansichts-Engines, Injektion von Abhängigkeiten in Ansichtsseiten einfügen).
- Aktionsfilter (Suchen und Filtern einfügen).
- Modellbinder ("registrieren" und "Einfügen").
- Modell Validierungsanbieter ("registrieren" und "Einfügen").
- Modellmetadaten-Anbieter ("registrieren" und "Einfügen").
- Wertanbieter ("registrieren" und "Einfügen").

MVC 3 unterstützt die [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) -Bibliothek und beliebige DI-Container, die diese Bibliothek unterstützt `IServiceLocator` Schnittstelle. Sie unterstützt auch eine neue `IDependencyResolver` -Schnittstelle, die Integration von DI-Frameworks vereinfacht.

Weitere Informationen über Dependency Injection in MVC 3 finden Sie unter den folgenden Ressourcen:

- [Brad Wilsons Reihe von Blogbeiträgen über Dienstidentifizierung](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Anmerkungen zur Version von MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Weitere neue Features

### <a name="nuget-integration"></a>NuGet-Integration

ASP.NET MVC 3 wird automatisch installiert und aktiviert NuGet als Teil des Setups. NuGet ist eine kostenlose Open-Source-Paket-Manager, mit der sie leicht zu finden, installieren und Verwenden von Bibliotheken für .NET und Tools in Ihren Projekten. Es funktioniert mit allen Visual Studio-Projekttypen (einschließlich ASP.NET Web Forms und ASP.NET MVC).

Mit NuGet können Entwickler open Source-Projekte (z. B. Projekte wie Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks und Elmah) zu verwalten, zum Packen ihrer Bibliotheken und registrieren sie in einem Onlinekatalog herunter. Es ist einfach, für .NET-Entwickler, die mit einer dieser Bibliotheken das Paket zu finden und installieren Sie es in Projekten, denen sie gerade arbeiten.

Mit dem ASP.NET 3 Tools Update umfassen Projektvorlagen JavaScript-Bibliotheken vorinstalliert NuGet-Pakete, sodass sie über NuGet aktualisiert werden. Entity Framework Code First wird auch als NuGet-Paket bereits installiert.

Weitere Informationen zu NuGet finden Sie in der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Zwischenspeichern der Ausgabe von Teilrendering von Seiten

ASP.NET MVC wurde die ausgabezwischenspeicherung der Antworten, die gesamte Seite seit Version 1 unterstützt. MVC 3 unterstützt auch Zwischenspeichern der Ausgabe des Teilrenderings von Seiten, die können Sie problemlos Cachebereiche oder -Fragmente eine Antwort. Weitere Informationen zum Zwischenspeichern finden Sie unter der **teilweise Seitenausgabecache** Abschnitt [Scott Guthries Blog-Beitrag auf MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) und die **untergeordnete Aktion Ausgabezwischenspeicherung** Teil der [MVC 3 – Anmerkungen zu dieser](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Eine präzise Kontrolle über die Request-Überprüfung

ASP.NET MVC verfügt über integrierte Anforderungsvalidierung, automatisch mit dem vor XSS und HTML Injection-Angriffen zu schützen. Allerdings unter Umständen explizit Disable Request-Überprüfung, möchten Sie z. B. Wenn Sie möchten Benutzer HTML Inhalt (z. B. im Blog-Einträge oder CMS-Inhalt) senden können. Sie können jetzt Hinzufügen einer [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) -Attribut auf die Modelle oder Modelle zum Deaktivieren der Anforderungsvalidierung auf Grundlage einzelner Eigenschaften während der modellbindung anzeigen. Weitere Informationen zur anforderungsüberprüfung finden Sie unter den folgenden Ressourcen:

- Die **unaufdringliches JavaScript und Validierung** im Abschnitt [Scott Guthries Blog-Beitrag auf MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Anmerkungen zur Version von MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Erweiterbare "New Project" (Dialogfeld)

In ASP.NET MVC 3 können Sie die Projektvorlagen, Ansichts-Engines, hinzufügen und der Komponententest-Projekt-Frameworks, die die **neues Projekt** Dialogfeld.

### <a name="template-scaffolding-improvements"></a>Vorlage Gerüstbau-Verbesserungen

ASP.NET MVC 3-gerüstbauvorlagen, einen besseren primären Schlüsseleigenschaften in Modellen zu identifizieren und behandeln sie entsprechend als in früheren Versionen von MVC. (Z. B. stellen die folgenden gerüstbauvorlagen jetzt sicher, dass der primäre Schlüssel nicht als Felder bearbeitbaren Formular Gerüst erstellt wurde.)

Standardmäßig verwenden die Gerüste erstellen und bearbeiten nun die `Html.EditorFor` Hilfsprogramm statt der `Html.TextBoxFor` Helper. Dies verbessert die Unterstützung für Metadaten für das Modell in Form von Daten Anmerkung Attribute, wenn die **Ansicht hinzufügen** Dialogfeld wird eine Ansicht generiert.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Neue Überladungen für "Html.LabelFor" und "Html.LabelForModel"

Neue Überladungen der Methode wurde für die `LabelFor` und `LabelForModel` Helper-Methoden. Die neuen Überladungen können Sie angeben oder überschreiben den Text der Bezeichnung.

### <a name="sessionless-controller-support"></a>Sessionless-Controller-Unterstützung

In ASP.NET MVC 3 Sie angeben können, ob eine Controller-Klasse, um den Sitzungsstatus verwendet werden soll, und wenn dies der Fall ist, gibt an, ob der Sitzungszustand muss Lese-/Schreibzugriff oder schreibgeschützt. Weitere Informationen zur Unterstützung von sessionless-Controller finden Sie unter [MVC 3 – Anmerkungen zu dieser](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Neue "AdditionalMetadataAttribute"-Klasse

Sie können die [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) -Attributs zum Auffüllen der `ModelMetadata.AdditionalValues` Wörterbuch für eine Modelleigenschaft. Beispielsweise verfügt ein Ansichtsmodell auf eine Eigenschaft, die nur für Administratoren angezeigt werden sollen, können Sie diese Eigenschaft versehen, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Diese Metadaten wird eine beliebige Vorlage anzeigen oder Editor zur Verfügung gestellt, wenn ein Produktmodell-Ansicht gerendert wird. Es ist Ihre Aufgabe, die Metadateninformationen zu interpretieren.

### <a name="accountcontroller-improvements"></a>AccountController-Verbesserungen

AccountController-Komponente in der Internet-Projektvorlage wurde erheblich verbessert.

### <a name="new-intranet-project-template"></a>Neue Projektvorlage für Intranet

Eine neue Projektvorlage für Intranet enthalten ist die Windows-Authentifizierung ermöglicht und AccountController-Komponente entfernt.
