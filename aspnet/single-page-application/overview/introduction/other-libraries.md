---
uid: single-page-application/overview/introduction/other-libraries
title: Wissen Sie eine Bibliothek als Knockout? | Microsoft-Dokumentation
author: madskristensen
description: Wissen Sie eine Bibliothek als Knockout?
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 5a863f50401a4e2bab3f772374b7fd178f6c6cdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="know-a-library-other-than-knockout"></a>Wissen Sie eine Bibliothek als Knockout?
====================
durch [Mads Kristensen](https://github.com/madskristensen)

Die [einzelnen Seite Anwendung (SPA) Vorlage](knockoutjs-template.md) ist eine hervorragende Möglichkeit zum Schreiben von Clientanwendungen für Einzelseiten beginnen. Die Vorlage verwendet [KnockoutJS](http://knockoutjs.com/) Anwendungsdaten auf DOM-Elemente zu binden.

Knockout ist jedoch nicht die einzige JavaScript-Bibliothek zum Erstellen von rich-Client-Anwendungen. Andere Bibliotheken durch ähnliche Probleme behoben werden auf unterschiedliche Weise. Möglicherweise bevorzugen Sie eine Bibliothek ein Vergleich mit anderen kennen, damit wir einige der Community erstellte Vorlagen zum Download verfügbar gemacht haben. Jede dieser Vorlagen wird eine andere Mischung von Client-JavaScript-Bibliotheken verwendet.

Um eine Vorlage von der Community erstellte installieren zu können, finden Sie auf der Vorlage Seiten unten, und klicken Sie auf die Schaltfläche "herunterladen". Die Vorlagen werden als VSIX-Dateien bereitgestellt.

## <a name="backbonejs"></a>BackboneJS

[Backbone.js SPA Vorlage](../templates/backbonejs-template.md). Diese Vorlage bietet eine anfängliche Skeleton zum Entwickeln einer [Backbone.js](http://backbonejs.org/) -Anwendung in ASP.NET MVC. Direktes es Standardbenutzer Anmeldung stellt Funktionalität bereit, einschließlich registrieren, anmelden, das Zurücksetzen von Benutzerkennwörtern und Bestätigung durch den Benutzer mit grundlegenden e-Mail-Vorlagen.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) ist eine open Source-Bibliothek für die Verwaltung von umfangreichen Daten in einem JavaScript-Client. Kinderspiel verarbeitet Abfragen, caching, änderungsnachverfolgung, Überprüfung und mehr. Zwei Vorlagen für die Funktion zum Kinderspiel:

- Die [Kinderspiel/Knockout](../templates/breezeknockout-template.md) Vorlage erweitert Knockout SPA-Vorlage, die anzeigt, wie leicht Sie eine Einzelseiten-Anwendung mit Kinderspiel für datenverwaltungs- und KnockoutJS für die Datenbindung erstellen können.
- Die [Kinderspiel/Angular](../templates/breezeangular-template.md) Vorlage verlängert außerdem die Knockout SPA-Vorlage zum Kinderspiel, jedoch mit der [AngularJS](http://angularjs.org) -Bibliothek für die Datenbindung, abhängigkeiteneinschleusung und Verwaltung Bildschirm.

Darüber hinaus die [Hot Handtuch SPA-Vorlage](../templates/hottowel-template.md) BreezeJS verwendet.

## <a name="emberjs"></a>EmberJS

[EmberJS SPA-Vorlage](../templates/emberjs-template.md). Diese Vorlage verwendet [Ember](http://emberjs.com/), eine leistungsstarke MVC-JavaScript-Bibliothek, die eine Vielzahl von Aufgaben zum Erstellen von rich Client-Anwendungen gelöst.

Die Ember SPA-Vorlage ist eine erneute Implementierung Knockout SPA-Vorlage, EmberJS und Lenkern Datenvorlagen verwenden.

## <a name="hot-towel"></a>Im laufenden Systembetrieb Handtuch

[Im laufenden Systembetrieb Handtuch SPA-Vorlage](../templates/hottowel-template.md). Diese Vorlage bringt verschiedene JavaScript-Bibliotheken, einschließlich zum Kinderspiel, Knockout, RequireJS und Bootstrap Twitter.

Im Vergleich mit den anderen Vorlagen, die hier aufgeführten bietet die Hot Handtuch Teample eine umfassendere Anwendung aus der Sie Ihre eigenen erstellen können. Gibt es weitere Konzepte zu berücksichtigen sind, aber nachdem Sie diese vertraut gemacht haben, diese Vorlage nur möglicherweise wonach Sie suchen. Wenn Sie eine SPA erstellen möchten, jedoch keine Entscheidung getroffen werden, wo Sie beginnen, sollten die im laufenden Systembetrieb Handtuch und in Sekunden stehen Ihnen eine SPA und alle Tools müssen Sie dafür erstellen.

## <a name="feature-table"></a>Feature-Tabelle

Hier werden die von jeder SPA-Vorlage bereitgestellten Funktionen:

|  | ASP.NET SPA | Backbone | Kinderspiel/Angular | Kinderspiel/KO | Ember | Im laufenden Systembetrieb Handtuch |
| --- | --- | --- | --- | --- | --- | --- |
| TODO-Beispiel | &#10003; |  | &#10003; | &#10003; | &#10003; |  |
| Bare-Vorlage |  | &#10003; |  |  |  | &#10003; |
| Navigation und Verlauf |  | &#10003; | &#10003; |  | &#10003; | &#10003; |
| Bibliotheken |  |  |  |  |  |  |
| Angular |  |  | &#10003; |  |  |  |
| &#8195; Backbone |  | &#10003; |  |  |  |  |
| Kinderspiel |  |  | &#10003; | &#10003; |  | &#10003; |
| Durandal |  |  |  |  |  | &#10003; |
| Ember |  |  |  |  | &#10003; |  |
| Knockout | &#10003; |  |  | &#10003; |  | &#10003; |
