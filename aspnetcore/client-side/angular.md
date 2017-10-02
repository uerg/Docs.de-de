---
title: "Mithilfe von AngularJS für einseitige Anwendungen (SPAs)"
author: rick-anderson
description: Erfahren Sie, wie eine SPA-Stil ASP.NET-Anwendung mithilfe von AngularJS erstellen
keywords: ASP.NET Core AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4aecf9e9bd11cc7e2b36b40955178d9e9368c185
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>Mithilfe von AngularJS für einseitige Anwendungen (SPAs) mit ASP.NET Core


Durch [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) und [Scott Addie](https://scottaddie.com)

In diesem Artikel erfahren Sie, wie eine SPA-Stil ASP.NET-Anwendung mithilfe von AngularJS erstellt werden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>Was ist AngularJS?

[AngularJS](https://angularjs.org/) einer modernen JavaScript-Frameworks von Google, die häufig zum Arbeiten mit Single Page Applications (SPAs) verwendet wird. AngularJS ist open Source unter MIT-Lizenz und den Fortschritt bei der Entwicklung von AngularJS auf gefolgt werden [des GitHub-Repositorys](https://github.com/angular/angular.js). Die Bibliothek wird Angular aufgerufen, da HTML angular geformten Klammern verwendet.

AngularJS ist eine DOM-Manipulation-Klassenbibliothek wie jQuery, aber es wird eine Teilmenge von jQuery aufgerufen jQLite verwendet. AngularJS basiert hauptsächlich auf deklarative HTML-Attribute, die Sie der HTML-Tags hinzufügen können. Versuchen Sie AngularJS in Ihrem Browser die [Code School Website](https://www.codeschool.com/courses/shaping-up-with-angularjs) oder [W3Schools Website](https://www.w3schools.com/angular/).

Dieser Artikel konzentriert sich auf AngularJS mit einige Hinweise auf, in dem Drehmoment Überschrift ist.

## <a name="getting-started"></a>Erste Schritte

Um mithilfe von AngularJS in Ihrer ASP.NET-Anwendung zu starten, müssen Sie die Installation als Teil Ihres Projekts, oder verweisen sie über ein Netzwerk für Inhaltsübermittlung (CDN) aus.

### <a name="installation"></a>Installation

Es gibt verschiedene Möglichkeiten zum Hinzufügen von AngularJS an Ihre Anwendung ein. Wenn Sie eine neue ASP.NET Core-Webanwendung in Visual Studio starten, können Sie unter Verwendung der integrierten AngularJS hinzufügen [Bower](bower.md) unterstützen. Open *"bower.JSON"*, und fügen Sie einen Eintrag, um die `dependencies` Eigenschaft:

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Beim Speichern der *"bower.JSON"* Angular-Datei in Ihrem Projekts installiert *"Wwwroot" / Lib* Ordner. Darüber hinaus wird es aufgeführt innerhalb der `Dependencies/Bower` Ordner. Siehe folgender Screenshot.

![Projektmappen-Explorer mit der AngularJS-Projekt](angular/_static/angular-solution-explorer.png)

Als Nächstes fügen Sie eine `<script>` Verweis auf das Ende der `<body>` Teil der HTML-Seite oder *_Layout.cshtml* Datei, wie hier gezeigt:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

Es wird empfohlen, dass Produktionsanwendungen CDNs für allgemeine Bibliotheken wie AngularJS verwenden. Sie können aus einer von mehreren CDNs, wie diese AngularJS verweisen:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Nachdem Sie einen Verweis auf haben die *angular.js* Skriptdatei, jetzt können Sie mithilfe von AngularJS in Webseiten zu beginnen.

## <a name="key-components"></a>Wichtige Komponenten

AngularJS umfasst eine Reihe von wesentlichen Komponenten, wie z. B. *Direktiven*, *Vorlagen*, *Repeater*, *Module*,  *Controller*, *Komponenten*, *Komponente Router* und vieles mehr. Betrachten wir nun, wie diese Komponenten zusammenarbeiten, um Webseiten Verhalten hinzuzufügen.

### <a name="directives"></a>Anweisungen

Mithilfe von AngularJS [Direktiven](https://docs.angularjs.org/guide/directive) HTML mit benutzerdefinierten Attributen und Elementen erweitern. AngularJS-Direktiven werden definiert über `data-ng-*` oder `ng-*` Präfixe (`ng` ist die Kurzform für winkelförmig). Es gibt zwei Arten von AngularJS-Direktiven:

   1. **Primitive Direktiven**: Diese werden vom Team Angular vordefiniert und sind Teil des AngularJS-Framework.

   2. **Benutzerdefinierte Direktiven**: Hierbei handelt es sich um benutzerdefinierte Direktiven, die Sie definieren können.

Der primitiven Direktiven in allen AngularJS-Anwendungen verwendet wird die `ng-app` -Direktive, die die AngularJS-Anwendung gestartet wird. Diese Direktive kann angewendet werden, um die `<body>` Tag oder ein untergeordnetes Element des Texts. Sehen Sie ein Beispiel, in Aktion an. Angenommen, Sie sind in einem ASP.NET-Projekt, können Sie entweder hinzufügen eine HTML-Datei, um die `wwwroot` Ordner, oder fügen Sie eine neue Controlleraktion und eine zugeordnete Ansicht. In diesem Fall habe ich ein neues hinzugefügt `Directives` Aktionsmethode `HomeController.cs`. Die zugeordnete Ansicht wird hier gezeigt:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Um diese Beispiele sind voneinander unabhängig zu halten, verwende ich nicht freigegebenen Layoutdatei. Beachten Sie, dass wir das Body-Tag mit ergänzt die `ng-app` Richtlinie an, dass diese Seite ist eine AngularJS-Anwendung. Die `{{2+2}}` ist ein Angular Datenbindungsausdruck, die Sie in wenigen Augenblicken mehr zu lernen soll. Hier ist das Ergebnis auf, wenn Sie diese Anwendung auszuführen:

![Einfache Angular-Direktive](angular/_static/simple-directive.png)

Andere primitiven Direktiven in AngularJS gehören:

`ng-controller`Bestimmt, welcher Controller JavaScript an die Sicht gebunden ist.

`ng-model`Bestimmt das Modell, zu dem die Werte der Eigenschaften eines HTML-Elements gebunden sind.

`ng-init`Wird verwendet, um die Anwendungsdaten in Form eines Ausdrucks für den aktuellen Bereich zu initialisieren.

`ng-if`Entfernt oder neu erstellt das angegebene HTML-Element im DOM basierend auf der Truthiness des Ausdrucks angegeben.

`ng-repeat`Wiederholt einen bestimmten Codeblock HTML für eine Menge von Daten an.

`ng-show`Zeigt an, oder blendet Sie aus dem angegebenen HTML-Element, das basierend auf den Ausdruck.

Eine vollständige Liste der alle primitiven Direktiven in AngularJS unterstützt, finden Sie auf der [Abschnitt "Richtlinie-Dokumentation" auf die AngularJS-Dokumentationswebsite](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Datenbindung

AngularJS bietet [Datenbindung](https://docs.angularjs.org/guide/databinding) Out-of-the-Box entweder unterstützen die `ng-bind` -Direktive oder ein Bindung Ausdruckssyntax, z. B. `{{expression}}`. AngularJS unterstützt bidirektionale Datenbindung, die bei der Daten aus einem Modell in die Synchronisierung mit einer Ansichtenvorlage jederzeit aufbewahrt wird. Alle Änderungen an der Ansicht sind automatisch im Modell widerspiegeln. Ebenso sind alle Änderungen im Modell in der Sicht widergespiegelt.

Erstellen Sie eine HTML-Datei oder eine Controlleraktion mit einer zugehörigen Ansicht mit dem Namen `Databinding`. In der Ansicht gehören:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Beachten Sie, dass Sie mithilfe von Direktiven oder Daten Bindung Modellwerte anzeigen können (`ng-bind`). Ergebnisseite sollte wie folgt aussehen:

![Einfache Datenbindung](angular/_static/simple-databinding.png)

### <a name="templates"></a>Vorlagen

[Vorlagen](https://docs.angularjs.org/guide/templates) in AngularJS werden nur einfache HTML-Seiten ergänzt, mit der AngularJS-Direktiven und Elemente. Eine AngularJS-Vorlage ist eine Mischung von Anweisungen, Ausdrücke, Filter und Steuerelemente, die mit HTML, um die Sicht bilden kombiniert werden soll.

Fügen Sie einer anderen Ansicht, um Vorlagen zu veranschaulichen, und fügen Sie Folgendes hinzu:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

Die Vorlage hat AngularJS-Direktiven wie `ng-app`, `ng-init`, `ng-model` und Syntax von Datenbindungsausdrücken zum Binden der `personName` Eigenschaft. In den Browser ausgeführt wird, sieht die Ansicht, wie der folgende Screenshot:

![Einfaches Beispiel für Vorlagen 1](angular/_static/simple-templates-1.png)

Wenn Sie den Namen ändern, indem Sie in das Eingabefeld eingeben, sehen Sie den Text neben dem Eingabefeld dynamisch aktualisieren, Anzeigen von Angular bidirektionale Datenbindung in Aktion.

![Einfaches Beispiel für Vorlagen 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Ausdrücke

[Ausdrücke](https://docs.angularjs.org/guide/expression) AngularJS sind JavaScript-ähnliches Codeausschnitte, die geschrieben werden, in der `{{ expression }}` Syntax. Die gleiche Weise wie die Daten aus diesen Ausdrücken in HTML gebunden `ng-bind` Direktiven. Der Hauptunterschied zwischen AngularJS-Ausdrücke und reguläre Ausdrücke von JavaScript wird diese AngularJS-Ausdrücke, für ausgewertet werden die `$scope` Objekt im AngularJS.

Die AngularJS-Ausdrücke im Beispiel unten Bind `personName` und eine einfache JavaScript berechneten Ausdruck:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

Im Beispiel wird der Browser zeigt auf die `personName` Daten und die Ergebnisse der Berechnung:

![Einfache Ausdrücke](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Repeater

Wiederholte in AngularJS erfolgt über einen primitiven Richtlinie aufgerufen `ng-repeat`. Die `ng-repeat` Richtlinie wiederholt ein angegebenes HTML-Element in einer Ansicht für die Länge eines Arrays aus sich wiederholenden Daten. Repeater in AngularJS können über ein Array von Zeichenfolgen oder Objekte wiederholen. Hier ist eine Beispiel für die Verwendung von wiederholten über ein Array von Zeichenfolgen:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

Die [repeat-Direktive](https://docs.angularjs.org/api/ng/directive/ngRepeat) gibt eine Reihe von Elementen in eine ungeordnete Liste aus, wie Sie in den Entwicklertools, die in diesem Screenshot gezeigten sehen können:

![Repeater-Beispiel](angular/_static/repeater.png)

Hier ist ein Beispiel, das über ein Array von Objekten wird wiederholt. Die `ng-init` Richtlinie richtet eine `names` Array, wobei jedes Element ist ein Objekt, das zuerst enthält und Nachnamen. Die `ng-repeat` Zuweisung `name in names`, gibt Sie ein Listenelement für jedes Arrayelement.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

Die Ausgabe wird in diesem Fall wie im vorherigen Beispiel identisch.

Angular bietet einige zusätzlichen Richtlinien, die helfen können Verhalten, je nachdem, in dem die Schleife in der Ausführung ist.

`$index`

Verwendung `$index` in die `ng-repeat` Schleife, um zu bestimmen, welche Indexposition die Schleife aktuell ist.

`$even` und `$odd`

Verwendung `$even` in die `ng-repeat` Schleife, um zu bestimmen, ob der aktuelle Index in der Schleife eine sogar indizierten Zeile ist. Auf ähnliche Weise verwenden `$odd` zu bestimmen, ob der aktuelle Index eine ungerade indizierten Zeile ist.

`$first` und `$last`

Verwendung `$first` in die `ng-repeat` Schleife, um zu bestimmen, ob der aktuelle Index in der Schleife die erste Zeile ist. Auf ähnliche Weise verwenden `$last` zu bestimmen, ob der aktuelle Index der letzten Zeile ist.

Im folgenden finden Sie ein Beispiel mit `$index`, `$even`, `$odd`, `$first`, und `$last` in Aktion:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

So sieht die resultierende Ausgabe aus:

![Repeater Beispiel 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`ist ein JavaScript-Objekt, das als Verbindung zwischen der Ansicht (Vorlage) und den Controller (siehe unten) fungiert. Eine Ansichtenvorlage für die in AngularJS kennt nur die Werte, die angefügt werden, um die `$scope` Objekt im Controller.

> [!NOTE]
> In der Welt MVVM der `$scope` Objekt in AngularJS wird häufig als "ViewModel" definiert. Die AngularJS-Team bezieht sich auf die `$scope` Objekt als das Datenmodell. [Erfahren Sie mehr über Bereiche in AngularJS](https://docs.angularjs.org/guide/scope).

Im folgenden ist ein einfaches Beispiel, das zeigt, wie zum Festlegen von Eigenschaften auf `$scope` innerhalb einer separaten JavaScript-Datei *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Beachten Sie die `$scope` Parameter an den Controller übergeben, Zeile 2. Dieses Objekt wurde die Sicht bekannt. In Zeile 3 wird eine Eigenschaft namens "Name", "Mary Jane" festgelegt.

Was geschieht, wenn eine bestimmte Eigenschaft von der Sicht nicht gefunden wird? Die Sicht definierten unten bezieht sich auf die Eigenschaften "Name" und "Age":

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Beachten Sie in Zeile 9, wir Angular bitten, um die Eigenschaft "Name" unter Verwendung der Abfrageausdruckssyntax anzuzeigen. Zeile 10 bezieht sich auf "age", eine Eigenschaft, die nicht vorhanden ist. Im aktuellen Beispiel zeigt den Namen "Mary Jane", und nichts für Alter der Daten festgelegt. Fehlende Eigenschaften werden ignoriert.

![Bereichsbeispiel](angular/_static/scope.png)

### <a name="modules"></a>Module

Ein [Modul](https://docs.angularjs.org/guide/module) in AngularJS ist eine Auflistung von Domänencontrollern, Dienste, Richtlinien. Die `angular.module()` wird zum Erstellen, registrieren und Abrufen von Modulen in AngularJS-Funktionsaufruf verwendet. Alle Module, einschließlich der durch die AngularJS-Team und Drittanbieter-Bibliotheken ausgeliefert müssen registriert werden, mithilfe der `angular.module()` Funktion.

Es folgt ein Codeausschnitt, das zum Erstellen eines neuen Moduls in AngularJS veranschaulicht. Der erste Parameter ist der Name des Moduls. Der zweite Parameter definiert die Abhängigkeiten auf anderen Modulen ausgeführt werden. Weiter unten in diesem Artikel wir detaillierter wie diese Abhängigkeiten übergeben ein `angular.module()` -Methodenaufruf.

```javascript
var personApp = angular.module('personApp', []);
```

Verwenden der `ng-app` Richtlinie, um eine AngularJS-Module auf der Seite darzustellen. Module verwenden, weisen Sie den Namen des Moduls, `personApp` in diesem Beispiel zu den `ng-app` -Direktive in unserer Vorlage.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Domänencontroller

[Controller](https://docs.angularjs.org/guide/controller) in AngularJS werden der erste Punkt des Eintrags für Ihren Code. Die `<module name>.controller()` wird zum Erstellen und Registrieren von Domänencontrollern in AngularJS-Funktionsaufruf verwendet. Die `ng-controller` Richtlinie wird verwendet, um eine AngularJS-Controller, der HTML-Seite darstellen. Die Rolle des Controllers in Angular Zustand und das Verhalten des Datenmodells festgelegt ist (`$scope`). Domänencontroller sollten nicht verwendet werden, auf das DOM direkt bearbeiten.

Im folgenden ist ein Codeausschnitt, der einen neuen Domänencontroller registriert. Die `personApp` Variable im Codeausschnitt verweist auf ein Angular-Modul, das für Zeile 2 definiert wird.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

Die Sicht mithilfe der `ng-controller` Richtlinie weist den Namen des Controllers:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

Die Seite zeigt "Mary" und "Jane", die entsprechen den `firstName` und `lastName` angefügte Eigenschaften, um die `$scope` Objekt:

![Controller-Beispiel](angular/_static/controllers.png)

### <a name="components"></a>Komponenten

[Komponenten](https://docs.angularjs.org/guide/component) in Angular 1.5.x ermöglichen, für die Kapselung und die Möglichkeit, einzelne HTML-Elemente erstellen. In Angular 1.4.x installiert sein können Sie die gleiche Funktion mithilfe der .directive()-Methode erreichen.

Mithilfe der .component()-Methode, um die Funktionalität der Direktive und den Controller Entwicklung vereinfacht. Weitere Vorteile umfassen eine; bereichsisolation, bewährte Methoden sind inhärenten und Migration Angular 2 wird ein einfacher Vorgang. Die `<module name>.component()` wird zum Erstellen und registrieren Komponenten in AngularJS-Funktionsaufruf verwendet.

Im folgenden ist ein Codeausschnitt, der eine neue Komponente registriert. Die `personApp` Variable im Codeausschnitt verweist auf ein Angular-Modul, das für Zeile 2 definiert wird.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

Die Ansicht, in dem wir das benutzerdefinierte HTML-Element angezeigt werden.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

Die zugehörige Vorlage, die von der Komponente verwendet:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

Die Seite zeigt "Aftab" und "Ansari", die entsprechen den `firstName` und `lastName` angefügte Eigenschaften, um die `vm` Objekt:

![Komponenten-Beispiel](angular/_static/components.png)

### <a name="services"></a>Dienste

[Services](https://docs.angularjs.org/guide/services) in AngularJS werden im Allgemeinen zum freigegebenen Code, der sofort in eine Datei abstrahiert die während der gesamten Lebensdauer einer Angular-Anwendung verwendet werden kann. Dienste werden verzögert instanziiert, was bedeutet, dass sich eine Instanz eines Diensts nicht, wenn eine Komponente, die von dem Dienst abhängt, verwendet wird. Factorys sind ein Beispiel für einen Dienst in AngularJS-Anwendungen verwendet. Factorys werden erstellt, mit der `myApp.factory()` Funktionsaufruf, wobei `myApp` ist das Modul.

Es folgt ein Beispiel für die Verwendung von Factorys in AngularJS verwenden:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Um diese Factory auf dem Controller aufzurufen, übergeben Sie `personFactory` als Parameter an die `controller` Funktion:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Mithilfe der Dienste mit einem REST-Endpunkt zu kommunizieren

Im folgenden ist ein Beispiel für die End-to-End-Dienste in AngularJS verwenden, für die Interaktion mit einem ASP.NET Core Web-API-Endpunkt. Im Beispiel ruft Daten aus der Web-API ab und zeigt die Daten in einer Vorlage anzeigen. Beginnen wir mit der Ansicht zuerst:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

In dieser Sicht haben wir eine Angular Modul mit dem Namen `PersonsApp` und ein Controller namens `personController`. Verwenden wir `ng-repeat` zum Durchlaufen der Liste der Personen ein. Es werden drei benutzerdefinierte JavaScript-Dateien in den Zeilen 17-19 verweisen.

Die *personApp.js* Datei wird zum Registrieren der `PersonsApp` -Modul und die Syntax ist ähnlich wie in vorherigen Beispielen. Wir verwenden die `angular.module` Funktion zum Erstellen einer neuen Instanz des Moduls, mit denen wir arbeiten werden wird.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Werfen wir einen Blick auf *personFactory.js*weiter unten. Wir werden des Moduls aufrufen `factory` Methode zum Erstellen einer Factory. Zeile 12 zeigt die integrierten Angular `$http` Dienst Personen Informationen von einem Webdienst abrufen.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

In *personController.js*, wir werden des Moduls aufrufen `controller` Methode, um den Controller zu erstellen. Die `$scope` des Objekts `people` -Eigenschaft von der PersonFactory (Line 13) zurückgegebenen Daten zugewiesen ist.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Werfen wir einen Blick auf die Web-API und das Modell vortransformierungsphase. Die `Person` Modell ist eine POCO (Plain Old CLR-Objekt) mit `Id`, `FirstName`, und `LastName` Eigenschaften:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

Die `Person` Controller gibt eine JSON-formatierte Liste von `Person` Objekte:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Sehen wir die Anwendung in Aktion:

![Controller mit REST-Ergebnisse](angular/_static/rest-bound.png)

Sie können [zeigen Sie die Struktur der Anwendung auf GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Weitere Informationen über die Strukturierung der AngularJS-Anwendungen, finden Sie unter [Angular Stilvorgaben des John Papa](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Um AngularJS-Module "," Domänencontroller "," Factory zu erstellen, Richtlinie und Ansicht-Dateien einfach Achten Sie beim Auschecken der Sayed Hashimi [SideWaffle Template-Pack für Visual Studio](http://sidewaffle.com/). Sayed Hashimi ist Senior Program Manager auf dem Visual Studio Web-Team bei Microsoft und SideWaffle Vorlagen gelten als der Premium-Standard. Zum Zeitpunkt der Erstellung dieses Dokuments ist SideWaffle für Visual Studio 2012, 2013 und 2015 verfügbar.

### <a name="routing-and-multiple-views"></a>Routing und mehrere Ansichten

AngularJS verfügt über einen integrierten Route Anbieter SPA (einseitigen-Anwendung) basierten Navigation zu behandeln. Sie müssen zum Arbeiten mit in AngularJS-routing, Hinzufügen der `angular-route` Bibliothek mithilfe von Bower. Sehen Sie der ["bower.JSON"](#angular-bower-json) Datei am Anfang dieses Artikels, wir es bereits im Projekt verweisen verwiesen wird.

Nach der Installation des Pakets den Skriptverweis hinzufügen (*angular route.js*) in Ihrer Ansicht anzeigen.

Jetzt sehen wir uns die Person-App, wir haben erstellt wurde, und fügen Sie die Navigation hinzu. Wir werden Stellen Sie zunächst eine Kopie der app durch Erstellen eines neuen `PeopleController` bewertungsaktion `Spa` und einer entsprechenden `Spa.cshtml` Ansicht durch Kopieren der Index.cshtml Ansicht in der `People` Ordner. Fügen Sie einen Skriptverweis auf `angular-route` (siehe Zeile 11). Auch hinzufügen, eine `div` mit markiert die `ng-view` Richtlinie (siehe Zeile 6) als Platzhalter, platzieren Sie die Sichten in. Wir werden einige zusätzliche verwenden *js* Dateien, die in den Zeilen 13 bis 16 verwiesen werden.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Werfen wir einen Blick auf *personModule.js* Datei, um festzustellen, wie wir das Modul mit dem routing instanziiert werden. Wir werden bestanden `ngRoute` als Bibliothek in das Modul. Dieses Modul wird verarbeitet, in der vorliegenden Anwendung routing.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

Die *personRoutes.js* Datei unten Routen, die basierend auf der Route-Anbieter definiert. Zeilen 4 bis 7 definieren die Navigation, indem Sie effektiv darauf hingewiesen werden, wenn eine URL mit `/persons` wird angefordert, verwenden Sie eine Vorlage mit der Bezeichnung `partials/personlist` entwickeln, indem über `personListController`. Linien 8 bis 11, geben Sie eine Detailseite mit einem Routenparameter `personId`. Wenn die URL nicht mit einem der Muster übereinstimmt, Angular standardmäßig die `/persons` anzeigen.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

Die `personlist.html` Datei ist eine Teilansicht, enthält nur die HTML-, die für die Anzeige Personenliste benötigt.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

Der Controller wird mithilfe des Moduls definiert `controller` -Funktion in *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Wenn wir diese Anwendung führen, und navigieren Sie zu der `people/spa#/persons` URL, siehe wir:

![Personen Listenansicht](angular/_static/spa-persons.png)

Wenn wir z. B. eine Detailseite navigieren `people/spa#/persons/2`, sehen wir die Teilansicht Detail:

![Person detaillierte Statusansicht](angular/_static/spa-persons-2.png)

Sehen Sie den vollständigen Quellcode und alle Dateien, die auf nicht angezeigt, in diesem Artikel [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Ereignishandler

Es gibt eine Reihe von Anweisungen in AngularJS, mit denen der Eingabeelemente in Ihre HTML-DOM. Ereignisbehandlung Funktionen hinzugefügt Es folgt eine Liste der Ereignisse, die in AngularJS integriert sind.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> Sie können Ihren eigenen Ereignishandler, die mit Hinzufügen der [benutzerdefinierte Direktiven feature AngularJS](https://docs.angularjs.org/guide/directive).

Sehen wir uns an, wie die `ng-click` Ereignis läuft. Erstellen Sie eine neue JavaScript-Datei mit dem Namen *eventHandlerController.js*, und fügen Sie Folgendes hinzu:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Beachten Sie das neue `sayName` -Funktion in `eventHandlerController` in Zeile 5 oben. All-Methode ist jedoch für jetzt eine JavaScript-Warnung für dem Benutzer eine Begrüßungsnachricht angezeigt wird.

Die folgende Sicht bindet eine Domänencontroller-Funktion auf eine AngularJS-Ereignis. Zeile 9 verfügt über eine Schaltfläche auf dem die `ng-click` Angular-Richtlinie angewendet wurde. Ruft unsere `sayName` -Funktion, die angefügt ist die `$scope` , in dieser Ansicht übergeben wird.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

Im aktuellen Beispiel zeigt, dass des Controllers `sayName` Funktion wird automatisch aufgerufen, wenn die Schaltfläche geklickt wird.

![Click-Ereignis](angular/_static/events.png)

Detailliertere auf AngularJS integrierte Ereignis-Handler-Direktiven, achten Sie auf die [Dokumentationswebsite](https://docs.angularjs.org/api/ng/directive/ngClick) von AngularJS.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Angular Docs](https://docs.angularjs.org)

* [Angular 2 Info](https://angular.io/)
