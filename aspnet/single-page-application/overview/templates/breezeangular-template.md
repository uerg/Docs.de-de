---
uid: single-page-application/overview/templates/breezeangular-template
title: Kinderspiel/Angular-Vorlage | Microsoft Docs
author: madskristensen
description: Vorlage zum Kinderspiel/Angular einseitigen-Anwendung
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a>Kinderspiel/Angular-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Die Kinderspiel/Angular MVC-Vorlage wurde vom Ward Bell geschrieben.
> 
> [Herunterladen der Kinderspiel/Angular MVC-Vorlage](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) ist eine open Source-Bibliothek von Google zum Erstellen von Single Page Applications (SPAs). Die Datenbindung, abhängigkeiteneinschleusung und Bildschirm Management bietet. Kombinieren Sie sie mit [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), eine andere open Source-Bibliothek für die datenmodellierung und datenverwaltung und Sie haben die wesentlichen Bestandteile für eine hervorragende HTML/JavaScript-Client-app.

Die Kinderspiel/Angular SPA-Vorlage ist eine Variation auf die [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md) ASP.NET und Web Tools 2012.2 Update enthalten. Wenn Sie Visual Studio verfügen, regieren müssen Sie beispielhaft SPA einsatzbereit in weniger als 60 Sekunden.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Nach außen zeigende, sucht die Anwendung sehr ähnlich der KnockoutJS SPA-Vorlage. Aber es ist sehr unterschiedliche hinter den Kulissen. Die Vorlage KnockoutJS verwendet Knockout für die Datenbindung und unformatierte AJAX für den Datenzugriff. Die Kinderspiel/Angular-Vorlage verwendet Angular für die Datenbindung und zum Kinderspiel, für den Datenzugriff. Diese Bibliotheken aktivieren, zusätzliche Funktionen, einschließlich der Seitennavigation und Verlaufstabellen.

So sieht die Informationen zur Anwendungsseite aus:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Auf dieser Seite wird ein Ausführungsprotokoll Ereignisse während der aktuellen Sitzung des Benutzers, einschließlich:

- Paging. Beachten Sie die Erstellung des Todo-Controller am #2 und #7 ein.
- Remoteabfragen (3) und lokalen Cache-Abfragen (#7).
- Neue speichern (5, &#6;) und (4) Entitäten geändert.
- Änderungen, die auf dem Client (9), damit der Benutzer Fehler behoben werden kann, vor dem Ausführen eines Commits für Änderungen in der Datenbank überprüft.

Es besteht jedoch in dieser Vorlage untersuchen einschließlich:

- Dynamisches Laden von HTML-Ansichtsvorlagen.
- Benutzerdefinierte Bindung durch Angular "Richtlinien".
- Modularität und Dependency Injection.
- Fragen Sie ab, Filter, Sortierungen, Paging, Projektionen und der Einschluss von verknüpften Entitäten.
- Freigeben von Daten auf mehreren Bildschirmen.
- Mehrere Änderungen werden als einzelne Transaktion gespeichert.
- Vom Server an den JavaScript-Client automatisch weitergegeben Validierungsregeln.

Fangen wir also an.

## <a name="create-a-breezeangular-template-project"></a>Erstellen Sie ein Kinderspiel/Angular-Vorlagenprojekt

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Die Vorlage wird als Visual Studio-Erweiterung (VSIX)-Datei verpackt. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

In der **neues Projekt** Assistenten **Kinderspiel Angular SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Drücken Sie STRG + F5, um zu erstellen, und führen Sie die Anwendung ohne Debuggen aus, oder drücken Sie F5, um mit Debuggen auszuführen.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Wenn die Anwendung zuerst ausgeführt wird, wird einen Anmeldebildschirm angezeigt. Klicken Sie auf den Link "Registrierung", und eine neue Seite glides in der Ansicht, in dem Sie einen Benutzernamen und ein Kennwort eingeben. (Auf die Seiten für Anmeldung und Registrierung werden über ASP.NET MVC erstellt.) Wenn Sie das Registrierungsformular übermitteln, generiert der Server eine "Todolist" mit zwei Elementen für Ihr Konto. Dann stellt er diese gelben betrachtet.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Jetzt können Sie in das Land des SPA. Alles, was Sie finden Sie unter und Erfahrung beim Bearbeiten von Todos gerendert und auf dem Client mithilfe von Knockout und Kinderspiel verwaltet. Untersuchen Sie die app als Benutzer... Dabei sollten Sie ein Entwickler. Verwenden Sie die Developer Tools in Ihrem Browser, um den Netzwerkdatenverkehr zu erfassen. (In InternetExplorer: Drücken Sie F12, wählen Sie die **Netzwerk** Registerkarte, und klicken Sie auf **Aufzeichnung gestartet**.) Probieren Sie Folgendes:

- Fügen Sie ein neues TODO-Element hinzu.
- Klicken Sie auf die Bezeichnung, und bearbeiten Sie den Titel des TODO-Element
- Kontrollkästchen Sie ein, um das Element "Fertig" ändert zu kennzeichnen. Beachten Sie, dass das Textfeld deaktiviert ist, daher ist der Titel nicht mehr bearbeitet werden.
- Klicken Sie auf das "X" rechts neben der Beschriftung. Das Element wird nicht mehr und wird aus der Datenbank gelöscht.
- Wählen Sie ein anderes Element aus, und deaktivieren Sie den Titel. Sie erhalten einen Validierungsfehler, dass der Titel erforderlich ist. Nach einer kurzen Pause wird der vorherigen Titel wiederhergestellt.
- Geben Sie einen übertrieben lange Titel ein. Sie erhalten einen anderen Validierungsfehler, dass der Titel zu lang ist.
- Klicken Sie auf die Schaltfläche "Hinzufügen von Todo-Liste". Eine neue Liste, die auf der linken Seite der vorherigen Liste angezeigt wird.
- Experimentieren Sie mit der "Todolist"-Title, auslösen erforderlich und Länge Überprüfungen.
- Klicken Sie in das Textfeld Titel, deaktivieren Sie die Fehlermeldung angezeigt.
- Klicken Sie auf das "X" in der Kreis in der oberen rechten Ecke der "Todolist" und dessen Todos zu löschen.
- Klicken Sie auf den Link "Info" in der oberen rechten Ecke, um ein Protokoll dieser Aktivitäten anzuzeigen.

Die Validierungslogik wird ausgeführt-Clientseite, indem zum Kinderspiel. Validierungsattributen für die Server-Modell-Klassen werden an den Client weitergegeben und automatisch ausgeführt werden, bevor der Client den Server kontaktiert.

Überprüfen Sie den Netzwerkdatenverkehr. Beachten Sie, dass wenn Kinderspiel ein Fehler erkannt wurden keine Aufrufe an den Server. Jede gültige Änderung führte zu einer POST-Anforderung auf "/ api/Todo/SaveChanges". Kinderspiel bündelt die Änderungen und sendet sie zusammen als eine einzelne Anforderung an die Web-API-Controller `SaveChanges` Methode. Unterscheidet sich vom KockoutJS SPA-Vorlage, wodurch PUT, POST und DELETE-Anforderungen für jedes Element einzeln.

Beachten Sie, dass kein Netzwerkdatenverkehr vorhanden ist, wenn Sie zwischen der "Todolist" sowie zu den Seiten wechseln. Ist, dass die Abfrage in den lokalen Cache der Kinderspiel beschränkt ist.

## <a name="peek-inside"></a>Peek-in

Diese Anwendung verfügt über eine Clientseite und einer serverseitigen. Clientseitige Stapel besteht ein wenig HTML und eine Kombination aus JavaScript Anwendungsmodulen (im Ordner "app") plus Drittanbieter-JavaScript-Bibliotheken (im Ordner "Skripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Die UI-Architektur trennt die HTML-Widgets Ansichten aus der unterstützenden Presentation-Code im Controller. Das Angular Datenbindungsfunktionen System koordiniert Ansichten und Controllern, sodass jeder ohne fundierte Kenntnisse der anderen Fehlerereignissen kann.

Der Controller fragt den Datenkontext zum Abrufen und speichern die Modellelemente. Der Datenkontext delegiert die meiste Arbeit zum Kinderspiel, die selbst-Model-Objekte von JSON-Abfrageergebnissen erstellt.

Serverseitige Stapel besteht aus einigen Entwicklercode und drei Prinzip .NET Bibliotheken: Web-API und Entity Framework Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Die grundlegende Architektur ist identisch mit der KockoutJS SPA-Vorlage. Die Implementierung ist jedoch viel einfacher: der DTOs gelöscht wurden, und die meisten Details Entity Framework haben Breeze.NET zugewiesen wurde.

## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, Sie den Code untersuchen, durch MDX die [umfassende Erläuterung](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) des Clients und der Server-Stapel auf der Website zum Kinderspiel.

Sie könnten versuchen, arbeiten mit Kinderspiel clientseitige Abfrage; Fügen Sie einige Filter und sortiert. Sie können weitere Modelleigenschaften und weitere Entitäten an, die eine bessere Gefühl für die End-to-End-SPA Entwicklung hinzufügen. Wenn Sie den Entwurf sicher sind, können Sie TODO-Funktionen entfernen und Ersetzen Sie sie durch Ihren eigenen.

Viel Spaß beim Programmieren!
