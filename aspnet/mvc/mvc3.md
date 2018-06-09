---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (April 2011 umfasst Tools Update) ASP.NET MVC 3 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die über gut eingeführte Entwurfsmuster...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034736"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(April 2011 umfasst Tools Update)*
> 
> ASP.NET MVC 3 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die unter Verwendung von bewährte Entwurfsmuster geeinigt und die Leistung von ASP.NET und .NET Framework.
> 
> Es installiert die Seite-an-Seite mit ASP.NET MVC 2, damit sie noch heute mit beginnen!
> 
> Herunterladen der [Installer hier](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Top-Funktionen

- Integrierte Gerüstbau System erweiterbar über NuGet
- HTML 5 aktiviert-Projektvorlagen
- Ausdrucksbasierte Ansichten, einschließlich der neuen Razor-Ansichtsmodul
- Leistungsstarke Hooks mit Abhängigkeiteneinschleusung und globale Aktionsfilter
- Rich-JavaScript-Unterstützung mit unaufdringliches JavaScript, jQuery-Validierung und JSON-Bindung
- *Lesen die vollständigen Liste [unten](#overview)*

## <a name="top-links"></a>Oben Links

Was ist neu in ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 veröffentlicht](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC 3-, WebMatrix, NuGet, IIS Express und Orchard des Transaktionskontexts zurückgegeben-das Microsoft Januar Webversion im Kontext](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [Ankündigung Version von ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Versionshinweise zu ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installation und Hilfe

- Installieren Sie ASP.NET MVC 3 mit dem [Webplattform-Installer (empfohlen)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installieren Sie ASP.NET MVC 3 mit dem [ausführbaren Installer](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installieren Sie [ASP.NET MVC 3 für Visual Studio 11 Developer Preview?](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lesen der [Einführung in ASP.NET MVC 3-Lernprogramm](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Abrufen von Hilfe und Erläutern Sie ASP.NET MVC 3 in der [Foren](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Übersicht über ASP.NET MVC 3

ASP.NET MVC 3 baut auf ASP.NET MVC-1 und 2, großartige Features, die Ihren Code vereinfachen und ermöglichen eine umfassendere Erweiterbarkeit hinzufügen. Dieses Thema enthält eine Übersicht über viele neue Features, die in dieser Version, die in den folgenden Abschnitten organisiert enthalten sind:

- [Erweiterbare Gerüstbau MvcScaffold-Integration](#BM_MvcScaffolding)
- [HTML 5 aktiviert-Projektvorlagen](#BM_HTML5)
- [Das Razor-Ansichtsmodul](#BM_TheRazorViewEngine)
- [Unterstützung für mehrere Ansichtsmodule](#BM_Support_for_Multiple_View_Engines)
- [Controller-Verbesserungen](#BM_Controller_Improvements)
- [JavaScript und Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Verbesserungen der Modell-Überprüfung](#BM_Model_Validation_Improvements)
- [Dependency Injection-Verbesserungen](#BM_Dependency_Injection_Improvements)
- [Weitere neuen Funktionen](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Erweiterbare Gerüstbau MvcScaffold-Integration

Das neue Gerüstbau System erleichtert es, zu übernehmen, und starten Sie produktiv verwenden, wenn Sie völlig neu das Framework, allgemeine Entwicklungsaufgaben automatisieren, wenn Sie Erfahrung haben und wissen, was Sie tun.

Dies wird durch neue NuGet unterstützt *Gerüstbau* Paket namens **MvcScaffolding**. Der Begriff "Gerüstbau" durch viele Software-Technologien verwendet wird, bedeutet "durch einen grundlegenden Überblick über Ihre Software, die Sie können schnell generieren dann bearbeiten und anpassen". Das Gerüst-Paket, das wir für ASP.NET MVC erstellen ist erheblich in verschiedenen Szenarien hilfreich:

- **Wenn Sie zum ersten Mal ASP.NET MVC vertraut machen**, denn es ermöglicht Ihnen eine schnelle Möglichkeit, um einige nützliche, Arbeitscode abzurufen, die Sie bearbeiten und Ihren Anforderungen entsprechend anpassen können. Sie ersparen Sie sich von Duplizierungsprogrammen Blick auf eine leere Seite und ohne Idee, wo Sie beginnen!
- **Wenn Sie ASP.NET MVC gut kennen und einige neue Add-on-Technologie sind jetzt Durchsuchen** z. B. eine Objekt-objektrelationale Zuordnungen, ein Ansichtsmodul, einer Bibliothek testen usw., da der Ersteller des diese Technologie auch ein Gerüst-Paket für sie erstellt haben kann.
- **Wenn Ihre Arbeit umfasst das Erstellen von wiederholt ähnliche Klassen oder Dateien des irgendeine**, da Sie benutzerdefinierter gerüstbauer erstellen können, die Ausgabe Testfixtures, Bereitstellungsskripts oder was Sie benötigen. Den jeder im Team kann Ihr benutzerdefinierter gerüstbauer zu verwenden.

Andere MvcScaffolding-Features aufgeführt:

- Unterstützung für c# und VB-Projekten
- Unterstützung für den Razor und ASPX-Module anzeigen
- Unterstützt das Gerüstbau in ASP.NET MVC-Bereiche und benutzerdefinierte Ansicht Layouts/Master verwenden
- Sie können die Ausgabe leicht anpassen, indem die T4-Vorlagen bearbeiten
- Sie können völlig neue gerüstbauer mit Systemlogik PowerShell und benutzerdefinierten T4-Vorlagen hinzufügen. Diese (und alle benutzerdefinierten Parameter sie zugewiesen haben) automatisch in der befehlszeilenergänzungsliste Konsole angezeigt.
- Sie erhalten die NuGet-Pakete, die zusätzliche gerüstbauer für verschiedene Technologien enthält (z. B. besteht eine Proof-of-Concept eine für LINQ to SQL jetzt) und kombinieren und diese zusammen

ASP.NET MVC 3 Tools Update enthält z. B. große Visual Studio-Unterstützung für dieses System Gerüstbau:

- Das Hinzufügen eines Dialogfeld Controller unterstützt jetzt die automatische Gerüst des Create, Read, Update und Delete Controlleraktionen und zugehörigen Sichten. Standardmäßig Gerüste dies Datenzugriffscode mithilfe von EF Code First.
- Unterstützt das Dialogfeld Controller hinzufügen *erweiterbare Gerüste* über NuGet Pakete wie *MvcScaffolding*. Dadurch können unter Verwendung der benutzerdefinierten Gerüste in das Dialogfeld "-", sodass Sie Gerüste für andere datenzugriffstechnologien z. B. NHibernate oder sogar JET mit ODBCDirect erstellen, wenn Sie also tendieren sind!

Weitere Informationen zu Gerüstbau in ASP.NET MVC 3 finden Sie unter den folgenden Ressourcen:

- Steve Sandersons post-Serie, einschließlich: 

    1. [Einführung: Das Gerüst für erstellen Sie das ASP.NET MVC 3-Projekt mit dem MvcScaffolding-Paket](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standardverfahren: Typische Anwendungsfälle und Optionen](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [1: N-Beziehungen](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Gerüstbau Gerüstbauaktionen und Komponententests](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Überschreiben die T4-Vorlagen](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Dieser Beitrag: Erstellen benutzerdefinierter gerüstbauer](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselmans Post aus seiner Sitzung PDC 2010 [einen Blog mit Microsoft "Unbenannte Paket von Web diesbezüglich führen, schätzen" erstellen](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3-Versionsanmerkungen](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5-Projektvorlagen

Das Dialogfeld "Neues Projekt" enthält ein Kontrollkästchen aktivieren HTML 5-Versionen von Projektvorlagen. Diese Vorlagen nutzen: Modernizr 1.7 um Kompatibilität 5 HTML und CSS 3 in niedrigeren Browser unterstützen.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Das Razor-Ansichtsmodul

ASP.NET MVC 3 enthält ein neues Anzeigemodul, die mit dem Namen Razor, die folgende Vorteile bietet:

- Razor-Syntax ist das Bereinigen und präzise, erfordern eine Mindestanzahl von Tastatureingaben.
- Razor ist einfach, teilweise lernen, da sie über vorhandene Sprachen wie c# und Visual Basic basiert.
- Visual Studio bietet IntelliSense und farbliche Kennzeichnung für Razor-Syntax.
- Razor-Ansichten möglich Komponententests ausgeführt werden, ohne dass erforderlich ist, führen Sie die Anwendung oder einen Webserver starten.

Einige neuen Razor-Funktionen umfassen Folgendes:

- `@model` Die Syntax zum Angeben des Typs an die Ansicht übergeben wird.
- `@* *@` Kommentarsyntax.
- Die Möglichkeit, Standardwerte anzugeben (z. B. `layoutpage`) einmal für die gesamte Website.
- Die `Html.Raw` Methode für die Anzeige von Text ohne HTML-Codierung es.
- Unterstützung für das Freigeben von Code für mehrere Ansichten (*\_viewstart.cshtml* oder  *\_viewstart.vbhtml* Dateien).

Razor enthält auch neue HTML-Hilfsmethoden, z. B. Folgendes an:

- `Chart` Rendert ein Diagramm, bietet die gleichen Funktionen wie das Diagrammsteuerelement in ASP.NET 4.
- `WebGrid` Rendert ein Datenraster, paging und Sortieren von Funktionen an.
- `Crypto` Verwendet Hashalgorithmen ordnungsgemäß erstellen Salt-Wert und ein Hashwert erstellt Kennwörter.
- `WebImage` Rendert ein Bild an.
- `WebMail` Sendet eine E-Mail.

Weitere Informationen zu Razor finden Sie unter den folgenden Ressourcen:

- [Einführung in Razor Scott Guthries Blog-post](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Einführung in Scott Guthries Blog-Post der @model Schlüsselwort](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthries Blog-Beitrag Einführung in Razor-layouts](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor-API-Kurzübersicht](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3-Versionsanmerkungen](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Unterstützung für mehrere Ansichtsmodule

Die **Ansicht hinzufügen** Dialogfeld in ASP.NET MVC 3 können Sie das Ansichtsmodul auswählen, Sie arbeiten möchten, und die **neues Projekt** Dialogfeld können Sie das Standardansichtsmodul für ein Projekt angeben. Sie können auswählen, das Ansichtsmodul Web Forms (ASPX), Razor, oder eine Open-Source-Ansichtsmodul wie z. B. [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), oder [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Controller-Verbesserungen

### <a name="global-action-filters"></a>Globale Aktionsfilter

Unter Umständen möchten Sie Logik ausführen, bevor eine Aktionsmethode ausgeführt wird, oder nach einer Aktionsmethode ausgeführt wird. Um dies zu unterstützen, bereitgestellten ASP.NET MVC 2 Aktionsfilter verwendet werden. Aktionsfilter sind benutzerdefinierte Attribute, die ein deklaratives Mittel Hinzufügen des Verhaltens einfügen vor und nach Abschluss der Aktion zu bestimmten Controlleraktionsmethoden bereitstellen. In einigen Fällen möchten jedoch möglicherweise einfügen vor oder nach Abschluss der Aktion Verhalten angeben, die für alle Aktionsmethoden gelten. MVC 3 können Sie die globalen Filter angeben, indem Sie sie zum Hinzufügen der `GlobalFilters` Auflistung. Weitere Informationen zu globalen Aktionsfilter verwendet werden finden Sie unter den folgenden Ressourcen:

- [Scott Guthries Blog auf das MVC 3 Preview?](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtern in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Neue "ViewBag"-Eigenschaft

Unterstützung von MVC 2-Controller eine `ViewData` -Eigenschaft, die Ihnen ermöglicht, Daten an eine Vorlage anzeigen, die unter Verwendung eines Wörterbuchs spät gebundene API übergeben. In MVC 3, können Sie auch etwas einfacher Syntax mit dem `ViewBag` Eigenschaft, um den gleichen Zweck zu erreichen. Beispielsweise anstelle des Schreibens von `ViewData["Message"]="text"`, können Sie schreiben `ViewBag.Message="text"`. Sie müssen keine stark typisierte Klassen zu verwenden, definieren die `ViewBag` Eigenschaft. Da es sich um eine dynamische Eigenschaft handelt, können Sie stattdessen nur abrufen oder Festlegen von Eigenschaften und lösen sie dynamisch zur Laufzeit. Intern `ViewBag` Eigenschaften gespeichert sind, als Name/Wert-Paare in der `ViewData` Wörterbuch. (Hinweis: in den meisten Vorabversionen von MVC 3, die `ViewBag` hieß diese Eigenschaft die `ViewModel` Eigenschaft.)

### <a name="new-actionresult-types"></a>Neue "ActionResult"-Typen

Die folgenden `ActionResult` Typen und zugehörigen Hilfsmethoden werden neue oder verbesserte in MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). HTTP-Statuscode 404 zurückgegeben an den Client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Gibt eine temporäre Umleitung (Statuscode "HTTP 302") oder eine permanente Umleitung (301 HTTP-Statuscode ""), je nach einem booleschen Parameter zurück. In Verbindung mit dieser Änderung der [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) -Klasse verfügt jetzt über drei Methoden zum Ausführen von permanenten leitet: `RedirectPermanent`, `RedirectToRoutePermanent`, und `RedirectToActionPermanent`. Diese Methoden zurückgeben eine Instanz von `RedirectResult` mit der `Permanent` -Eigenschaftensatz auf `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Gibt einen benutzerdefinierten HTTP-Statuscode zurück.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript und Ajax-Verbesserungen

Ajax und Validierung Hilfsprogramme in MVC 3 verwenden standardmäßig einen unaufdringlichen JavaScript-Ansatz. Unaufdringliches JavaScript vermeidet Räumen Inline-JavaScript in HTML. Dies macht Ihre HTML kleiner und weniger überladen und erleichtert es, auszutauschen, oder passen Sie die JavaScript-Bibliotheken. Überprüfung Hilfsprogramme in MVC 3 auch verwenden, die `jQueryValidate` -Plug-in standardmäßig. Wenn MVC 2-Verhalten soll, können Sie deaktivieren, unaufdringliches JavaScript mithilfe einer *"Web.config"* Datei festlegen. Weitere Informationen zu den JavaScript und Ajax-Verbesserungen finden Sie unter den folgenden Ressourcen:

- [Grundlegende Einführung in unaufdringliches JavaScript auf die Wikipedia-Website](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilsons unaufdringliches JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilsons unaufdringliches JavaScript-Validierung Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (Lernprogramm auf der ASP.NET-Website)
- [MVC 3-Versionsanmerkungen](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Die clientseitige Überprüfung standardmäßig aktiviert

In früheren Versionen von MVC explizit aufrufen, müssen die `Html.EnableClientValidation` Methode aus einer Sicht, um die clientseitige Validierung zu aktivieren. In MVC 3 ist dies nicht mehr erforderlich, da die clientseitige Überprüfung standardmäßig aktiviert ist. (Sie können diese Option deaktivieren mit einer Einstellung in der *"Web.config"* Datei.)

In der Reihenfolge für die clientseitige Validierung funktioniert müssen Sie immer noch die entsprechenden jQuery und jQuery Validation Bibliotheken auf Ihrer Website verweisen. Sie können diese Bibliotheken auf einem eigenen Server zu hosten oder Verweis auf ein Content Delivery Network (CDN) wie die CDNs von Microsoft oder Google.

### <a name="remote-validator"></a>Die Remotebestätigung

ASP.NET MVC 3 unterstützt die neue [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) Klasse, die Sie nutzen von jQuery-Validierung-Plug-ins ermöglicht die remotebestätigung Unterstützung. Dies ermöglicht die clientseitige Validierungsbibliothek automatisch serverseitige Aufrufen eine benutzerdefinierte Methode, die Sie auf dem Server zu definieren, damit die Validierungslogik ausgeführt werden, die nur ausgeführt werden können.

Im folgenden Beispiel die `Remote` Attribut gibt an, dass die Clientvalidierung eine Aktion, die mit dem Namen angerufen `UserNameAvailable` auf die `UsersController` Klasse, um zu überprüfen der `UserName` Feld.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Das folgende Beispiel zeigt den entsprechenden Controller an.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Weitere Informationen zur Verwendung der `Remote` -Attribut angegeben wird, finden Sie unter [Vorgehensweise: Implementieren der Remotevalidierung in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in der MSDN Library.

### <a name="json-binding-support"></a>Unterstützung für JSON-Bindung

ASP.NET MVC 3 bietet integrierte JSON-Bindung-Unterstützung, der aktiviert Aktionsmethoden zum Empfangen von JSON-codierte Daten und Modell binden sie Aktionsmethodenparameter. Diese Funktion ist nützlich in Szenarien, die im Zusammenhang mit Client-Vorlagen und Datenbindung. (Client-Vorlagen aktivieren Sie zum Formatieren und Anzeigen von ein einzelnes Datenelement oder ein Satz von Datenelementen anhand von Vorlagen, die auf dem Client ausgeführt.) MVC 3 können Sie ganz einfach Clientvorlagen mit Aktionsmethoden, auf dem Server verbinden, das Senden und Empfangen von JSON-Daten. Weitere Informationen zur Unterstützung von JSON-Bindung finden Sie unter der **JavaScript und AJAX-Verbesserungen** Abschnitt [Guthries MVC 3 Preview Blogbeitrag](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Verbesserungen der Modell-Überprüfung

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations" Metadaten-Attributen

ASP.NET MVC 3 unterstützt `DataAnnotations` Metadaten Attribute wie `DisplayAttribute`.

### <a name="validationattribute-class"></a>"ValidationAttribute"-Klasse

Die `ValidationAttribute` Klasse wurde verbessert die Leistung in .NET Framework 4, um ein neues unterstützen `IsValid` Überladung, die enthält weitere Informationen zu aktuellen Validierungskontext, z. B. welches Objekt überprüft wird. Dies ermöglicht umfangreichere Szenarien, in dem Sie den aktuellen Wert auf Grundlage einer anderen Eigenschaft des Modells überprüfen können. Beispielsweise ist die neue `CompareAttribute` -Attributs können Sie die Werte von zwei Eigenschaften eines Modells vergleichen. Im folgenden Beispiel die `ComparePassword` Eigenschaft muss übereinstimmen. die `Password` Feld, damit es gültig ist.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Überprüfung von Schnittstellen

Die [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) -Schnittstelle ermöglicht Ihnen das Ausführen der Validierung auf Modellebene und dadurch werden Sie zur Bereitstellung der Validierung Fehlermeldungen, die in den Zustand des gesamten Modells oder zwischen zwei Eigenschaften in das Modell spezifisch sind . MVC 3 ruft nun Fehler aus der `IValidatableObject` -Schnittstelle beim wurden die modellbindung, und automatisch Flags oder Highlights Felder innerhalb einer Ansicht mithilfe der integrierten HTML-Formular-Hilfsprogrammen betroffen.

Die [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) Schnittstelle ermöglicht ASP.NET MVC, um zur Laufzeit zu ermitteln, ob ein Validator Unterstützung für Clientvalidierung verfügt. Diese Schnittstelle wurde entwickelt, sodass er mit einer Vielzahl von Überprüfung Frameworks integriert werden kann.

Weitere Informationen zur Überprüfung Schnittstellen finden Sie unter der **Modell Überprüfung Verbesserungen** Abschnitt [Guthries MVC 3 Preview Blogbeitrag](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Beachten Sie jedoch, dass der Verweis auf "IValidateObject" im Blog "IValidatableObject" werden soll.)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Dependency Injection-Verbesserungen

ASP.NET MVC 3 bietet eine bessere Unterstützung für die Anwendung (Dependency Injection, DI) und für die Integration mit Containern Abhängigkeitsinjektion oder die Umkehrung des Steuerelements (IOC). Unterstützung für DI wurde hinzugefügt, in den folgenden Bereichen:

- Domänencontroller (registrieren und Räumen Controllerfactorys, Räumen Controller).
- Sichten (registrieren und Räumen Ansichtsmodule, Abhängigkeiten in Ansichtsseiten Räumen).
- Aktionsfilter verwendet werden (Suchen und Filtern Räumen).
- Modellbinder (registrieren und Räumen).
- Modell Validierungsanbietern (registrieren und Räumen).
- Modellmetadaten-Anbieter (registrieren und Räumen).
- Wertanbieter (registrieren und Räumen).

MVC 3 unterstützt die [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) -Bibliothek und DI-Container, der diese Bibliothek unterstützt `IServiceLocator` Schnittstelle. Sie unterstützt auch eine neue `IDependencyResolver` Schnittstelle, die leichter DI Frameworks integriert werden kann.

Weitere Informationen zu DI in MVC 3 finden Sie unter den folgenden Ressourcen:

- [Brad Wilsons Serie von Blogbeiträgen auf Dienstidentifizierung](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3-Versionsanmerkungen](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Weitere neuen Funktionen

### <a name="nuget-integration"></a>NuGet-Integration

ASP.NET MVC 3 wird automatisch installiert und aktiviert NuGet im Rahmen des Setups. NuGet ist eine kostenlose Open Source-Paketmanager, der ganz einfach zu finden, installieren und Verwenden von .NET Bibliotheken und Tools in Ihren Projekten. Es funktioniert mit allen Visual Studio-Projekttypen (einschließlich ASP.NET Web Forms und ASP.NET MVC).

Mit NuGet können Entwickler, die open-Source-Projekte (z. B. Projekte wie Moq, NHibernate Ninject, StructureMap, NUnit, Windsor, RhinoMocks und Elmah) zum Packen ihrer Bibliotheken und registrieren sie in einem Onlinekatalog verwalten. Es ist dann einfach für .NET-Entwickler, die zum Verwenden einer dieser Bibliotheken das Paket suchen und installieren es in Projekten, denen sie arbeiten möchten.

Mit ASP.NET 3 Tools Update umfassen Projektvorlagen JavaScript-Bibliotheken vorinstallierte NuGet-Pakete, sodass sie über NuGet aktualisiert werden. Entity Framework Code First wird auch als ein NuGet-Paket bereits installiert.

Weitere Informationen zu NuGet finden Sie in der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Ausgabecaching Teilrendering von Seiten

ASP.NET MVC unterstützt Zwischenspeichern der Ausgabe der gesamte Seitenantworten seit Version 1. MVC 3 unterstützt auch Zwischenspeichern der Ausgabe Teilrendering von Seiten können Sie problemlos Cachebereiche oder Fragmente einer Antwort. Weitere Informationen zum Zwischenspeichern finden Sie unter der **teilweise Seitenausgabecache** Abschnitt [Scott Guthries Blog Post für MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) und die **untergeordnete Aktion Zwischenspeichern der Ausgabe** Teil der [MVC 3-Anmerkungen zu dieser Version](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Eine präzise Kontrolle über die Anforderungsüberprüfung

ASP.NET MVC verfügt über integrierte anforderungsüberprüfung, automatisch mit der vor XSS und HTML-Injection-Angriffen geschützt. Allerdings unter Umständen explizit deaktivieren anforderungsüberprüfung, möchten Sie z. B. Wenn Sie möchten Benutzer HTML Inhalte (z. B. in Blogeinträgen oder CMS-Inhalt) bereitstellen können. Sie können jetzt Hinzufügen einer [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) -Attribut auf Modelle oder Modelle zum Deaktivieren der Anforderungsvalidierung auf Grundlage einzelner Eigenschaften während der modellbindung anzeigen. Weitere Informationen zur anforderungsüberprüfung finden Sie unter den folgenden Ressourcen:

- Die **unaufdringliches JavaScript und Validierung** im Abschnitt [Scott Guthries Blog Post für MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [MVC 3-Versionsanmerkungen](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>"Neues Projekt" Extensible (Dialogfeld)

In ASP.NET MVC 3 können Sie die Projektvorlagen, Ansichtsmodule, hinzufügen und Komponententestframeworks Projekt auf die **neues Projekt** (Dialogfeld).

### <a name="template-scaffolding-improvements"></a>Vorlage Gerüstbau Verbesserungen

ASP.NET MVC 3-gerüstbauvorlagen führen besser identifizieren von Primärschlüssel-Eigenschaften für Modelle und behandeln sie entsprechend als in früheren Versionen von MVC. (Z. B. stellen die folgenden gerüstbauvorlagen jetzt sicher, dass der Primärschlüssel nicht als ein bearbeitbares Formularfeld Gerüstbau.)

Das Erstellen und Bearbeiten von Gerüste jetzt verwenden standardmäßig die `Html.EditorFor` Helper anstelle von der `Html.TextBoxFor` Helper. Dies verbessert die Unterstützung für Metadaten für das Modell in Form von Daten Anmerkung Attribute, wenn die **Ansicht hinzufügen** Dialogfeld generiert eine Ansicht.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Neue Überladungen für "Html.LabelFor" und "Html.LabelForModel"

Neue Überladungen der Methode wurde für die `LabelFor` und `LabelForModel` Hilfsmethoden. Die neue Überladungen ermöglichen es Ihnen, anzugeben oder zu den Bezeichnungstext überschreiben.

### <a name="sessionless-controller-support"></a>Sessionless-Controller-Unterstützung

In ASP.NET MVC 3 Sie angeben können, ob eine Controllerklasse Sitzungsstatus verwendet werden soll, und wenn dies der Fall ist, gibt an, ob Sitzungszustand muss Lese-/Schreibzugriff oder schreibgeschützt. Weitere Informationen zur Unterstützung von sessionless-Controller finden Sie unter [MVC 3-Anmerkungen zu dieser Version](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Neue "AdditionalMetadataAttribute"-Klasse

Sie können die [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) -Attributs zum Auffüllen der `ModelMetadata.AdditionalValues` Wörterbuch für eine Modelleigenschaft. Beispielsweise verfügt ein Ansichtsmodell eine Eigenschaft, die nur an einen Administrator angezeigt werden soll, können Sie diese Eigenschaft versehen, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Diese Metadaten ist Anzeige oder Editor-Vorlage zur Verfügung gestellt, wenn ein Produktmodell für die Ansicht gerendert wird. Es liegt bei Ihnen die Metadateninformationen zu interpretieren.

### <a name="accountcontroller-improvements"></a>AccountController-Verbesserungen

Die AccountController in der Internet-Projektvorlage wurde erheblich verbessert.

### <a name="new-intranet-project-template"></a>Neues Intranet-Projektvorlage

Eine neue Vorlage des Intranet-Projekt enthalten ist die Windows-Authentifizierung aktiviert und die AccountController entfernt.
