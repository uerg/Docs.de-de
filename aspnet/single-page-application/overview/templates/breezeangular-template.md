---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze.js-/Angular.js-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: Breeze/Angular-Single-Page-Application-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 541d1a71b58a0d55651d823dc7425b7d1ce73cdb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378059"
---
<a name="breezeangular-template"></a>Breeze.js-/Angular.js-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Die Breeze/Angular-MVC-Vorlage wurde von Ward Bell geschrieben.
> 
> [Die Breeze/Angular MVC-Vorlage herunterladen](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) ist eine open-Source-Bibliothek von Google für die Erstellung von Single Page Applications (SPAs). Die Datenbindung, Abhängigkeitsinjektion und Bildschirm-Verwaltung bietet. Kombinieren Sie sie mit [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), einer anderen open-Source-Bibliothek für die datenmodellierung und die datenverwaltung und Sie haben die wesentlichen Zutaten für eine große HTML/JavaScript-Client-app.

Die Breeze/Angular-SPA-Vorlage ist eine Variation auf die [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md) im ASP.NET and Web Tools 2012.2-Update enthalten. Wenn Sie Visual Studio haben, müssen ein Beispiel für SPA und in weniger als 60 Sekunden Sie.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Nach außen, ähnelt die Anwendung die die KnockoutJS SPA-Vorlage. Aber es ist sehr unterschiedlich sind, im Hintergrund. Die Knockout.js-Vorlage verwendet Knockout für die Datenbindung und unformatierte AJAX für den Datenzugriff. Die Breeze/Angular-Vorlage verwendet die Angular für die Datenbindung und Breeze für den Datenzugriff. Diese Bibliotheken ermöglichen zusätzliche Funktionen, einschließlich der Seitennavigation und Verlaufstabellen.

Hier ist der Anwendung – Seite "Info":

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Diese Seite wird ein Ausführungsprotokoll von Ereignissen angezeigt, während der aktuellen benutzersitzung, einschließlich:

- Paging. Beachten Sie die Erstellung des Todo-Controller auf #2 und #7.
- Remoteabfragen (3) und lokalen Cache-Abfragen (#7).
- Neue speichern (5, #6) und (4) Entitäten geändert.
- Änderungen, die auf dem Client (#9), damit der Benutzer die Fehler korrigieren kann, vor dem Ausführen eines Commits für Änderungen in der Datenbank überprüft.

Vorhanden ist, auf die Zukunft in dieser Vorlage, einschließlich:

- Dynamisches Laden von HTML-Ansichtsvorlagen.
- Benutzerdefinierte Datenbindung durch Angular "-Direktiven."
- Modularität und die Abhängigkeitsinjektion.
- Fragen Sie ab, Filter, Sortierungen, Paging, Projektionen und Aufnahme von verknüpften Entitäten.
- Freigeben von Daten auf mehrere Bildschirme.
- Speichern als einzelne Transaktion mehrere Änderungen.
- Regeln zur Überprüfung weitergegeben automatisch vom Server an den JavaScript-Client.

Fangen wir also an.

## <a name="create-a-breezeangular-template-project"></a>Erstellen Sie ein Projekt für Breeze/Angular-Vorlage

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Die Vorlage wird als eine Datei für Visual Studio-Erweiterung (VSIX) verpackt. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

In der **neues Projekt** Assistenten **Breeze Angular-SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Drücken Sie STRG + F5 zum Erstellen und Ausführen der Anwendungs ohne Debuggen, oder drücken Sie F5, um mit Debuggen auszuführen.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Wenn die Anwendung zuerst ausgeführt wird, wird einen Anmeldebildschirm angezeigt. Klicken Sie auf den Link "Registrierung", und eine neue Seite glides in der Ansicht, in dem Sie einen Benutzernamen und ein Kennwort eingeben. (Die Seiten für Anmeldung und Registrierung werden mithilfe von ASP.NET MVC erstellt.) Wenn Sie das Registrierungsformular übermitteln, generiert der Server eine "Todolist" mit zwei Elementen, die für Ihr Konto an. Präsentiert er diese für Sie in diesem gelb.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Jetzt können Sie in der der SPA Land. Alles, was Sie finden Sie unter den sowie beim Bearbeiten von TODO-Elemente gerendert und auf dem Client mithilfe von Knockout und Breeze verwaltet. Untersuchen Sie die app als ein Benutzer ein... aber eines Entwicklers Augen. Verwenden Sie die Developer Tools in Ihrem Browser, um den Netzwerkdatenverkehr zu erfassen. (In InternetExplorer: Drücken Sie F12, wählen Sie die **Netzwerk** Registerkarte, und klicken Sie auf **startet das Sammeln von**.) Jetzt versuchen Sie Folgendes aus:

- Fügen Sie ein neues Todo-Element hinzu.
- Klicken Sie auf die Bezeichnung, und bearbeiten Sie den Titel der Todo-Element
- Kontrollkästchen Sie ein, um das Element abgeschlossen zu markieren. Beachten Sie, dass das Textfeld deaktiviert ist, damit der Titel nicht mehr bearbeitet werden kann.
- Klicken Sie auf das "X" rechts neben der Bezeichnung. Das Element nicht mehr angezeigt wird und aus der Datenbank gelöscht wird.
- Wählen Sie ein anderes Element aus, und deaktivieren Sie den Titel. Sie erhalten einen Validierungsfehler, dass der Titel erforderlich ist. Nach einer kurzen Pause wird der vorherige Titel wiederhergestellt.
- Geben Sie eine unglaublich lange Titel. Sie erhalten einen anderen Validierungsfehler, dass der Titel zu lang.
- Klicken Sie auf die Schaltfläche "Todo-Liste hinzufügen". Eine neue Liste, die auf der linken Seite der vorherigen Liste angezeigt wird.
- Experimentieren Sie mit der "Todolist"-Titel, auslösen erforderlich und die Länge Überprüfungen.
- Klicken Sie auf, in das Textfeld Titel ein, um die Fehlermeldung zu löschen.
- Klicken Sie auf das "X" in den Kreis in der oberen rechten Ecke, um die "Todolist" und seine TODO-Elemente zu löschen.
- Klicken Sie auf den Link "Info" in der oberen rechten Ecke, um ein Protokoll dieser Aktivitäten anzuzeigen.

Die Validierungslogik wird von Breeze clientseitige ausgeführt. Validierungsattribute für die Server-Modell-Klassen werden an den Client weitergegeben und automatisch ausgeführt werden, bevor der Client den Server kontaktiert.

Überprüfen Sie den Netzwerkdatenverkehr. Beachten Sie, dass es keine Aufrufe an den Server, gab Wenn Breeze ein Fehler erkannt. Jede gültige Änderung führte zu einer POST-Anforderung "/ api/Todo /" SaveChanges"". Breeze bündelt die Änderungen und sendet diese zusammen als eine einzelne Anforderung an die Web-API-Controllers `SaveChanges` Methode. Unterscheidet sich vom KockoutJS SPA-Vorlage, die PUT, POST und DELETE-Anforderungen für jedes Element einzeln ist.

Beachten Sie, dass kein Netzwerkdatenverkehr vorhanden ist, wenn Sie zwischen der "Todolist" sowie zu den Seiten wechseln. Das ist da die Abfrage in den lokalen Cache von Breeze eingeschränkt wurde.

## <a name="peek-inside"></a>Peek-in

Diese Anwendung verfügt über eine Clientseite und einer serverseitigen. Der Client-Side-Stapel besteht aus ein wenig HTML und eine Kombination aus Anwendung JavaScript-Module (im Ordner "app") und Drittanbieter-JavaScript-Bibliotheken (im Ordner "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Die Benutzeroberflächen-Architektur trennt die HTML-Widgets der Ansichten aus dem zugrunde liegenden Presentation-Code in den Controllern. Die Angular-Datenbindungssystem koordiniert Ansichten und Controllern, sodass die einzelnen weder eine eingehende Kenntnis der anderen durchführen kann.

Der Controller fragt den Datenkontext zum Abrufen und speichern die Modellelemente. Der Datenkontext delegiert die meisten Aufgaben zu Breeze, das aus Abfrageergebnissen JSON-modellobjekten mit selbstnachverfolgung erstellt.

Der serverseitige Stapel besteht aus einige Entwickler das Codieren und drei Prinzip-.NET-Bibliotheken: Web-API, Entity Framework und Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Die grundlegende Architektur ist identisch mit der KockoutJS SPA-Vorlage. Die Implementierung ist jedoch viel einfacher: die DTOs gelöscht wurden, und die meisten Details Entity Framework an Breeze.NET delegiert wurden.

## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, dass Sie den Code, die durch MDX Untersuchen der [umfassende Diskussion](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) von sowohl Client als auch die Server-Stapel auf der Website zum Kinderspiel.

Versuchen Sie, ob Sie spielen mit Breeze clientseitige Abfrage; Fügen Sie einige Filter und Sortierungen. Sie können weitere Eigenschaften des Modells und weitere Entitäten erhalten ein besseres Gefühl für die Entwicklung von SPAS End-to-End hinzufügen. Wenn Sie das Design sicher sind, können sich die Todo-Features entfernen und Ersetzen sie durch Ihre eigenen.

Viel Spaß beim Programmieren!
