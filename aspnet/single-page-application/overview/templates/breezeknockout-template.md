---
uid: single-page-application/overview/templates/breezeknockout-template
title: Breeze/Knockout-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: Breeze/Knockout-Single-Page-Application-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 48ee0463fe950c28832523986a2242417411c96a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376863"
---
<a name="breezeknockout-template"></a>Breeze/Knockout-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Die Breeze/Knockout-MVC-Vorlage wurde von Ward Bell geschrieben.
> 
> [Die Breeze/Knockout-MVC-Vorlage herunterladen](https://go.microsoft.com/fwlink/?LinkId=282649)


Sie haben gehört, von "single Page Application" (SPA) und gefragt, was es ist. Während Sie zu lesen können, würden Sie es stattdessen für sich selbst kommen. Aber wer hat die Zeit, ein Beispiel herunterladen? Wenn Sie Visual Studio haben, müssen Sie Sie ein Beispiel für SPA und ausgeführt wird, in weniger als 60 Sekunden mit ASP.NET-MVC 4 "Breeze/Knockout Single Page Application"-Vorlage.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Was ist die Breeze/Knockout-SPA-Vorlage?

Die meisten Projektvorlagen ein Gerüst der Anwendung zu generieren. Sie verstehen auf die Gerüste durch Hinzufügen von Code und schließlich bieten eine funktionierende Anwendung. Die Breeze/Knockout-SPA-Vorlage unterscheidet. Es wird eine beispielanwendung, die Sie untersuchen generiert. Ein SPA-Anwendungsentwurf und viele der Techniken zum Erstellen einer einseitigen Anwendung veranschaulicht.

Die Breeze/Knockout-Vorlage ist eine Variation auf die [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md) im ASP.NET and Web Tools 2012.2-Update enthalten. Die Breeze-SPA-Vorlage erzeugt eine Anwendung mit der gleichen Benutzeroberfläche, aber es wurde eine andere Implementierung, die mithilfe von Breeze für die datenverwaltung.

Die KnockoutJS SPA-Vorlage stellt Service Requests mit unformatierten jQuery AJAX, die für eine einfache Anwendung ausreichend ist. Jedoch komplexer apps über erhöhte Anforderungen an Daten. Um beispielsweise die meisten Anwendungen:

- Abfragen und den Server während einer Sitzungs für erweiterte Benutzer erneut abfragen.
- Fügen Sie die Abfragefiltern, Sortieren und paging hinzu.
- Nutzen Sie dieselben Daten auf mehrere Bildschirme.
- Sammeln von Änderungen an viele Objekte an, und speichern Sie sie als einzelne Transaktion.
- Überprüfen Sie Änderungen auf dem Client aus, damit der Benutzer die Fehler korrigieren kann, vor der Übernahme von Änderungen in der Datenbank.

Die BreezeJS-Bibliothek übernimmt diese Aufgaben für Sie so mehr Zeit zum Entwickeln der Anwendung Logik und die benutzerfreundlichkeit, die am wichtigsten sind.

[**Breeze** ](http://www.breezejs.com/?utm_source=ms-spa) ist eine open-Source-Bibliothek zum Erstellen von umfangreichen Daten von Anwendungen in JavaScript und HTML, die Arten von apps, die in der Vergangenheit als eigenständige desktopanwendungen übermittelt wurden.

Die Breeze/Knockout-Vorlage können Sie diese wichtige zunächst eine robustere Data Management-Infrastruktur zu nutzen. Es erzeugt eine Todo-beispielanwendung, die nach außen an die KnockoutJS SPA-Vorlage identisch ist. Innerhalb oder es ersetzt die AJAX-Datenschicht mit Breeze, damit Sie die beiden vergleichen können fast erreicht wird Seite-an-Seite. Natürlich betrifft es kaum das volle Potenzial von einer Anwendung zum Kinderspiel. Aber Sie sehen, dass Breeze wie funktioniert und wie wenig ist erforderlich, um diese neue Version umstellen.

Fangen wir also an.

## <a name="create-a-breezeknockout-template-project"></a>Erstellen Sie ein Projekt der Breeze/Knockout-Vorlage

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Die Vorlage wird als eine Datei für Visual Studio-Erweiterung (VSIX) verpackt. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

In der **neues Projekt** Assistenten **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Drücken Sie STRG + F5 zum Erstellen und Ausführen der Anwendungs ohne Debuggen, oder drücken Sie F5, um mit Debuggen auszuführen.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

Die Validierungslogik wird von Breeze clientseitige ausgeführt. Validierungsattribute für die Server-Modell-Klassen werden an den Client weitergegeben und automatisch ausgeführt werden, bevor der Client den Server kontaktiert.

Überprüfen Sie den Netzwerkdatenverkehr. Beachten Sie, dass es keine Aufrufe an den Server, gab Wenn Breeze ein Fehler erkannt. Jede gültige Änderung führte zu einer POST-Anforderung "/ api/Todo /" SaveChanges"". Breeze bündelt die Änderungen und sendet diese zusammen als eine einzelne Anforderung an die Web-API-Controllers `SaveChanges` Methode. Unterscheidet sich vom KockoutJS SPA-Vorlage, die PUT, POST und DELETE-Anforderungen für jedes Element einzeln ist.

## <a name="peek-inside"></a>Peek-in

Diese Anwendung verfügt über eine Clientseite und einer serverseitigen. Der Client-Side-Stapel besteht aus ein wenig HTML und eine Kombination aus Anwendung JavaScript-Module (im Ordner "app") und Drittanbieter-JavaScript-Bibliotheken (im Ordner "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Wenn Sie die KnockoutJS SPA-Vorlage untersucht haben, sollte dies sehr vertraut aussehen. Konzentrieren sich auf die blauen Felder. Die Benutzeroberflächen-Architektur ist Model-View-ViewModel (MVVM), in dem die HTML-Widgets der Ansicht ordnungsgemäß aus dem zugrunde liegenden Presentation-Code in das Ansichtsmodell getrennt sind. Eine Datenbindungssystem (Knockout in diesem Fall) koordiniert die Ansicht und Ansichtsmodell, sodass die einzelnen weder eine eingehende Kenntnis der anderen durchführen kann.

Das Modell kapselt die Todo-Daten. Entitäten im Modell werden erstellt, indem Breeze mit Knockout-Observable-Eigenschaften, sodass sie direkt mit Widgets in der Ansicht gebunden werden können. Das Ansichtsmodell fragt den Datenkontext zum Abrufen und speichern die Modellelemente. Der Datenkontext delegiert die meisten Aufgaben zum Kinderspiel.

Der serverseitige Stapel besteht aus einige Entwickler das Codieren und drei Prinzip-.NET-Bibliotheken: Web-API, Entity Framework und Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Die grundlegende Architektur ist identisch mit der KockoutJS SPA-Vorlage. Die Implementierung ist jedoch viel einfacher: die DTOs gelöscht wurden, und die meisten Details Entity Framework an Breeze.NET delegiert wurden.

## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, dass Sie den Code, die durch MDX Untersuchen der [umfassende Diskussion](http://www.breezejs.com/spa-template?utm_source=ms-spa) von sowohl Client als auch die Server-Stapel auf der Website zum Kinderspiel.

Versuchen Sie, ob Sie spielen mit Breeze clientseitige Abfrage; Fügen Sie einige Filter und Sortierungen. Sie können weitere Eigenschaften des Modells und weitere Entitäten erhalten ein besseres Gefühl für die Entwicklung von SPAS End-to-End hinzufügen. Wenn Sie das Design sicher sind, können sich die Todo-Features entfernen und Ersetzen sie durch Ihre eigenen.

Sie werden in Kürze für die nächste große Schritt bereit: das Hinzufügen von Bildschirmen für die clientseitige und zwischen diesen navigieren. Sie diesem SPA-Vorlage hinter uns lassen und aktivieren Sie eine vollständige SPA-Stapel, wie z. B. [John Papa die Hot Towel](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), Durandal zu Breeze und Knockout hinzugefügt.
