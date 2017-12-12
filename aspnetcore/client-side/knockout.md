---
title: Knockout.js MVVM-Framework in ASP.NET Core
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Knockout.js MVVM-Framework in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/)

Knockout ist eine beliebte JavaScript-Bibliothek, die die Erstellung von komplexen datenbasierte Benutzeroberflächen vereinfacht. Sie können alleine oder zusammen mit anderen Bibliotheken, z. B. jQuery verwendet werden. Der Hauptzweck ist Benutzeroberflächenelemente an eine zugrunde liegende Datenmodell definiert, die als JavaScript-Objekt, zu binden, wenn Änderungen an der Benutzeroberfläche vorgenommen werden, die das Modell aktualisiert wird, und umgekehrt. Knockout erleichtert die Verwendung eines Musters Model-View-ViewModel (MVVM) eine Webanwendung clientseitige Verhalten. Die beiden wichtigsten Konzepte, die eine Kennenlernen muss bei der Arbeit mit Knockout des MVVM Implementierung werden Wahrnehmbare Elemente und Bindungen.

## <a name="getting-started"></a>Erste Schritte

Knockout wird bereitgestellt, als eine einzelne JavaScript-Datei, also installieren und verwenden, ist sehr einfach mit [Bower](bower.md). Angenommen, Sie bereits haben [Bower](bower.md) und [gulp](using-gulp.md) konfiguriert ist, öffnen Sie *"bower.JSON"* in Ihre ASP.NET Core Projekt, und fügen Sie die Abhängigkeit Knockout, wie hier gezeigt:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

Mit diesem erfüllt können Sie dann manuell Bower ausführen durch Öffnen der Taskausführungs-Explorer (unter Ansicht ‣ Weitere Fenster ‣ Taskausführungs-Explorer), und klicken Sie dann unter Aufgaben mit der Maustaste, auf Bower und Ausführung auswählen. Das Ergebnis sollte etwa wie folgt aussehen:

![bower ausgeführten Knockout in Taskausführungs-Explorer](knockout/_static/bower-knockout.png)

Wenn Sie in Ihrem Projekts suchen nun `wwwroot` Ordner sollte Knockout unter dem Ordner "Lib" installiert.

![im Ordner "Lib" installiert Knockout](knockout/_static/wwwroot-knockout.png)

Es wird empfohlen, dass in Ihrer produktionsumgebung Sie Knockout über ein Netzwerk für die Inhaltsübermittlung oder CDN, verweisen, wie dies die Wahrscheinlichkeit erhöht, dass Ihre Benutzer bereits eine zwischengespeicherte Kopie der Datei müssen und daher nicht auf allen herunterladen müssen. Knockout ist auf mehrere CDNs, einschließlich Microsoft Ajax-CDNS, hier verfügbar:

[http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Um Knockout auf einer Seite einzuschließen, die sie verwenden, fügen Sie einfach eine `<script>` Element auf die Datei verweist, von wo Sie sie (mit der Anwendung oder über ein CDN) gehostet werden:

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Wahrnehmbare Elemente ViewModels und einfache Bindung

Sie können bereits mit JavaScript zum Bearbeiten von Elementen auf einer Webseite, entweder über direkten Zugriff auf das DOM oder eine Bibliothek, z. B. jQuery vertraut sein. Diese Art von Verhalten in der Regel erfolgt durch Schreiben von Code, um Elementwerte in Reaktion auf bestimmte Benutzeraktionen direkt festzulegen. Mit Knockout erfolgt ein deklarativer Ansatz stattdessen über die Elemente auf der Seite Eigenschaften für ein Objekt gebunden sind. Anstatt das Schreiben von Code zum Bearbeiten von DOM-Elementen, Benutzeraktionen interagieren einfach das ViewModel-Objekt und Knockout übernimmt das sicherstellen, dass die Seitenelemente synchronisiert werden.

Betrachten Sie als einfaches Beispiel der Liste unten aus. Er enthält eine `<span>` Element mit einem `data-bind` Attribut gibt an, dass der Text-Inhalt AuthorName gebunden werden soll. Im nächsten Schritt in eine JavaScript-Blocks eine Variable ViewModel, mit einer einzelnen Eigenschaft definiert wird `authorName`, auf einen beliebigen Wert festgelegt. Schließlich ein Aufruf von `ko.applyBindings` vorgenommen wird, wird in dieser ViewModel-Variable übergeben.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

Bei der Anzeige in den Browser, den Inhalt der <span> Element mit dem Wert in der Variablen ViewModel ersetzt wird:

![einfache Bindung Knockout](knockout/_static/simple-binding-screenshot.png)

Wir haben jetzt einfache unidirektionale Bindung arbeiten. Beachten Sie, dass nie im Code wir JavaScript, um den Inhalt der Spanne einen Wert zuzuweisen geschrieben haben. Wenn wir das ViewModel bearbeiten möchten, können wir übernehmen dies einen Schritt weiter und eine HTML-Eingabe Textbox hinzufügen und binden Sie mit dem Wert, z. B. so:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Die Seite wird erneut geladen, sehen wir, dass dieser Wert auf das Eingabefeld tatsächlich gebunden ist:

![eingabebindung Knockout](knockout/_static/input-binding-screenshot.png)

Jedoch wenn wir den Wert in das Textfeld zu ändern, den entsprechenden Wert in der `<span>` Elemente nicht ändern. Warum?

Das Problem besteht darin, dass nichts benachrichtigt den `<span>` , die es benötigt, um aktualisiert werden. Einfach aktualisieren ViewModel ist nicht allein ausreichend, es sei denn, das ViewModel-Eigenschaften in eine Sonderform umschlossen ist. Wir verwenden müssen **Wahrnehmbare Elemente** im ViewModel für alle Eigenschaften, die nur geändert, die automatisch aktualisiert, sobald sie auftreten müssen. Durch Ändern der ViewModel verwenden `ko.observable("value")` statt nur "Value" aktualisiert das ViewModel HTML-Elemente, die auf den Wert gebunden sind, wenn eine Änderung erfolgt. Beachten Sie, dass Eingabefelder ihr Wert nicht aktualisieren, bis sie den Fokus, verlieren, sodass Sie Änderungen an Elementen gebunden, während der Eingabe angezeigt werden.

> [!NOTE]
> Hinzufügen der Unterstützung für live aktualisieren, nachdem jedes Keypress lediglich hinzugefügt wird `valueUpdate: "afterkeydown"` auf die `data-bind` des Attributs. Sie können dieses Verhalten auch abrufen, mithilfe von `data-bind="textInput: authorName"` sofortige Updates Werte abrufen. 

Unsere ViewModel nach der Aktualisierung, um ko.observable zu verwenden:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Knockout unterstützt eine Reihe von verschiedenen Arten von Bindungen. Bisher haben wir gesehen, wie zum Binden an `text` und `value`. Sie können auch auf alle angegebenen Attribute binden. Für die Instanz, um einen Hyperlink erstellen, mit der ein Ankertag der `src` Attribut auf das ViewModel gebunden werden kann. Knockout unterstützt auch die Bindung an Funktionen. Zur Veranschaulichung nehmen wir das ViewModel Einbeziehung des Autors Twitter Handle anzuzeigen und zu aktualisieren das Twitter-Handle als Link zu der Autor Twitter-Seite. Dazu müssen Sie drei Phasen verwenden.

Fügen Sie zunächst den HTML-Code, um den Link wird in Klammern nach dem Namen des Autors zeige anzuzeigen:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Aktualisieren Sie als Nächstes das ViewModel, um die TwitterUrl und TwitterAlias umfassen:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Beachten Sie, dass zu diesem Zeitpunkt noch nicht wir noch die TwitterUrl fahren Sie mit der richtigen URL für diesen Alias Twitter aktualisiert – nur am twitter.com verweist. Beachten Sie außerdem, dass wir eine neue Knockout-Funktion verwenden, `computed`, für TwitterUrl. Dies ist eine Observable-Funktion, mit die alle Benutzeroberflächenelemente benachrichtigt wird, wenn er geändert wird. Hierfür auf anderen Eigenschaften im ViewModel zugreifen, müssen wir jedoch ändern wie wir das ViewModel erstellen, sodass jede Eigenschaft einen eigenen-Anweisung ist.

Die überarbeitete ViewModel-Deklaration ist unten dargestellt. Es ist jetzt als Funktion deklariert. Beachten Sie, dass jede Eigenschaft einen eigenen-Anweisung nun mit einem Semikolon beendet ist. Beachten Sie außerdem den Zugriff auf den Eigenschaftswert TwitterAlias müssen, sodass dessen Verweis enthält () ausgeführt.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

Das Ergebnis funktioniert wie erwartet im Browser:

![Knockout hyperlink](knockout/_static/hyperlink-screenshot.png)

Knockout unterstützt auch die Bindung auf bestimmte Ereignisse des UI-Element, z. B. das Click-Ereignis. Dadurch können Sie problemlos und deklarativ Benutzeroberflächenelemente Funktionen innerhalb der Anwendung ViewModel gebunden. Als einfaches Beispiel können Sie eine Schaltfläche hinzufügen, wenn angeklickt, ändert der Autor TwitterAlias GROSSBUCHSTABEN werden.

Zunächst wird die Schaltfläche "hinzufügen", Bindung auf der Schaltfläche klicken Sie auf Ereignis, und verweisen auf den Namen der Funktion, die wir möchten das ViewModel hinzufügen:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Klicken Sie dann fügen Sie die Funktion das ViewModel hinzu, und verknüpfen sie das ViewModel-Status ändern. Beachten Sie, dass wir um einen neuen Wert der Eigenschaft TwitterAlias festzulegen, rufen Sie es als eine Methode und übergeben Sie den neuen Wert ein.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

Der Code ausgeführt wird, und klicken auf die Schaltfläche ändert den angezeigten Link wie erwartet:

![Hyperlink Großbuchstaben](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Ablaufsteuerung

Knockout umfasst Bindungen, die bedingte und Schleifen-Vorgänge ausführen können. Schleifenvorgänge sind besonders nützlich zum Binden von Listen der Daten an UI-Listen, Menüs und Rastern oder Tabellen. Die foreach-Bindung wird ein Array durchlaufen. Bei Verwendung mit einem Observable-Array wird wird er automatisch die Elemente der Benutzeroberfläche aktualisiert, wenn Elemente hinzugefügt oder aus dem Array entfernt werden, ohne dass jedes Element in der Benutzeroberflächenautomatisierungs-Struktur neu zu erstellen. Im folgenden Beispiel wird eine neue ViewModel ein Observable Array von Spiel Ergebnisse enthält. Sie einer einfachen Tabelle gebunden ist, mit zwei Spalten mit einer `foreach` -Bindung auf die `<tbody>` Element. Jede `<tr>` Element innerhalb `<tbody>` wird auf ein Element der Auflistung GameResults gebunden werden.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

Beachten Sie, dass dieses Mal wir ViewModel mit dem Großbuchstaben "V" verwenden, da wir erwarten, dass sie mit "new" (im Aufruf ApplyBindings) erstellen. Bei der Ausführung Ergebnisseite der in der folgenden Ausgabe:

![Knockout Datensatzansichts-Modell](knockout/_static/record-screenshot.png)

Um zu zeigen, wird die Funktionsfähigkeit der Observable-Auflistung, fügen Sie ein wenig mehr Funktionalität. Wir können gehören die Fähigkeit zum Aufzeichnen der Ergebnisse von einem anderen Spiel ViewModel, und fügen Sie eine Schaltfläche und einige Elemente der Benutzeroberfläche arbeiten Sie mit dieser neuen Funktion.  Zunächst erstellen wir die AddResult-Methode:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Binden Sie diese Methode mit einer Schaltfläche mithilfe der `click` Bindung:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Öffnen Sie die Seite im Browser, und klicken Sie auf die Schaltfläche ein paar Mal, was zu einer neuen Tabellenzeile mit jedem klicken:

![Hinzufügen von Ergebnissen](knockout/_static/record-addresult-screenshot.png)

Es gibt einige Möglichkeiten zum Hinzufügen neuer Datensätze in der Benutzeroberfläche, in der Regel entweder Inline oder in einer separaten Form unterstützt. Wir können problemlos ändern Sie die Tabelle, Textfelder und Dropdownlists verwenden, damit alles bearbeitbar ist. Ändern Sie einfach die `<tr>` Element wie gezeigt:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Beachten Sie, dass `$root` bezieht sich auf den Stamm ViewModel, also, in dem die möglichen Optionen verfügbar gemacht werden. `$data`bezieht sich auf das das aktuelle Modell in einem bestimmten Kontext – in diesem Fall ist es auf ein einzelnes Element des Arrays ResultChoices bezieht sich von denen jeder eine einfache Zeichenfolge ist.

Durch diese Änderung wird das gesamte Raster bearbeitet werden:

![Bearbeitbares Raster](knockout/_static/editable-grid-screenshot.png)

Wenn wir nicht waren Knockout verwenden, wir konnte erreichen all dies unter Verwendung von jQuery, aber es wahrscheinlich nicht würde annähernd so effizient sein. Knockout verfolgt die gebundenen Daten, die Elemente in das ViewModel entsprechen, welche Elemente der Benutzeroberfläche und aktualisiert nur die Elemente, die hinzugefügt, entfernt oder aktualisiert werden müssen. Es würde erheblichen Aufwand Softwareverteilungsfeatures uns mit jQuery oder direkte DOM-Manipulation und selbst dann, wenn wir aggregieren Sie Ergebnisse (z. B. einen Datensatz Win-Verlust) basierend auf Daten der Tabelle anzeigen möchten, wir müssten einmal durchlaufen sie und Analysieren der HTML-Elemente.  Mit Knockout ist das Anzeigen des Datensatzes Win-Loss unbedeutend. Sie können Berechnungen innerhalb der ViewModel selbst und zeigen Sie es mit einem einfachen Text-Bindung und einem `<span>`.

Um die Zeichenfolge der Gewinn / Verlust Datensatz zu erstellen, können wir eine berechnete Observable-Objekt. Beachten Sie, die auf Observable-Eigenschaften in das ViewModel verweist Funktionsaufrufe muss, andernfalls werden sie nicht den Wert der die Observable-Objekt abrufen (d. h. `gameResults()` nicht `gameResults` im Code gezeigt):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Binden Sie diese Funktion auf einen innerhalb der `<h1>` Element am oberen Rand der Seite:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

Das Ergebnis:

![Win-Verlust](knockout/_static/record-winloss-screenshot.png)

Zeilen hinzufügen oder ändern das ausgewählte Element in der Ergebnisspalte für eine beliebige Zeile aktualisiert den Datensatz, der am oberen Rand des Fensters angezeigt.

Zusätzlich zur Bindung mit Werten können Sie auch nahezu jede rechtlichen JavaScript-Ausdruck in einer Bindung. Z. B. wenn ein Element der Benutzeroberfläche nur angezeigt werden sollen, unter bestimmten Umständen kann z. B. wenn ein Wert für einen bestimmten Schwellenwert überschreitet können Sie dies logisch innerhalb der Bindungsausdruck angeben:

```html
<div data-bind="visible: customerValue > 100"></div>
```

Dies `<div>` ist nur sichtbar, wenn die CustomerValue mehr als 100 ist.

## <a name="templates"></a>Vorlagen

Knockout wurde Unterstützung für Vorlagen, damit Sie problemlos von Ihrem Verhalten, die Benutzeroberfläche trennen oder inkrementellen Laden von UI-Elemente in einer großen Anwendung bei Bedarf können. Wir können unserer vorherigen Beispiel um jede Zeile eine eigenen Vorlage machen, indem Sie einfach abzufangen HTML out in eine Vorlage, und Angeben der Vorlage nach dem Namen in der Data-Bind-Aufruf auf Aktualisieren `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Knockout unterstützt auch andere Textvorlagen-Module, wie z. B. der jQuery.tmpl-Bibliothek und des Underscore.js-Vorlagenmodul.

## <a name="components"></a>Komponenten

Komponenten können Sie zum Organisieren und Wiederverwenden von UI-Code in der Regel zusammen mit den ViewModel-Daten, die dem UI-Code abhängt. Um eine Komponente zu erstellen, müssen Sie einfach zum Angeben der Vorlage und seine ViewModel und geben sie einen Namen. Hierzu wird `ko.components.register()` aufgerufen. Zusätzlich zum Definieren von Vorlagen und Viewmodel Inline, sie können geladen werden aus externen Dateien mit einer Bibliothek wie *"Require.js"*, wodurch sehr sauber und effizienten Codes.

## <a name="communicating-with-apis"></a>Bei der Kommunikation mit APIs

Knockout funktionieren mit Daten im JSON-Format. Ein gängiges Verfahren zum Abrufen und Speichern von Daten mit Knockout mit jQuery, die unterstützt wird die `$.getJSON()` Funktion zum Abrufen von Daten, und die `$.post()` Methode, um Daten über den Browser an einen API-Endpunkt zu senden. Natürlich, wenn Sie eine andere Möglichkeit zum Senden und Empfangen von JSON-Daten lieber, funktioniert Knockout mit ihm auch.

## <a name="summary"></a>Zusammenfassung

Knockout bietet eine einfache und elegante Möglichkeit, Elemente der Benutzeroberfläche mit dem aktuellen Status der Clientanwendung, definiert in einem ViewModel gebunden. Knockout des Bindungssyntax wird verwendet, das Data-Bind-Attribut, HTML-Elementen, die zu verarbeitende angewendet wird. Knockout kann effizient Rendern und großen Datasets aktualisieren, indem Sie Benutzeroberflächenelemente nachverfolgen und Verarbeitung nur Änderungen an den betroffenen Elemente. Große Anwendungen können zusammensetzen Benutzeroberflächen-Logik, die mithilfe von Vorlagen und Komponenten, die bei Bedarf aus externen Dateien geladen werden können. Derzeit ist Version 3, Knockout eine stabile JavaScript-Bibliothek, die Webanwendungen verbessern können, die rich-Client Interaktivität erfordern.
