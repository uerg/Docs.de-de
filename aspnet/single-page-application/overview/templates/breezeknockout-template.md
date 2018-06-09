---
uid: single-page-application/overview/templates/breezeknockout-template
title: Kinderspiel/Knockout Vorlage | Microsoft Docs
author: madskristensen
description: Vorlage zum Kinderspiel/Knockout einseitigen-Anwendung
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506699"
---
<a name="breezeknockout-template"></a>Kinderspiel/Knockout-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Die Kinderspiel/Knockout MVC-Vorlage wurde vom Ward Bell geschrieben.
> 
> [Herunterladen der Kinderspiel/Knockout MVC-Vorlage](https://go.microsoft.com/fwlink/?LinkId=282649)


Sie haben "einseitige Anwendung" gehört (SPA) und gefragt, was ist. Während Sie Informationen lesen konnte, würden Sie stattdessen für sich selbst fest. Aber wer ist Zeit, um ein Beispiel herunterladen möchten? Wenn Sie Visual Studio haben, stehen Ihnen ein Beispiel für SPA einrichten und ausführen, in weniger als 60 Sekunden mit ASP.NET-MVC 4 "Kinderspiel/Knockout einseitigen-Anwendung" Vorlage.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Was ist die Kinderspiel/Knockout SPA-Vorlage?

Die meisten Projektvorlagen Generieren einer Anwendung Skeleton. Sie Flesh auf diese Gerüst durch Hinzufügen von Code und schließlich eine funktionierende Anwendung zu übermitteln. Die Vorlage zum Kinderspiel/Knockout SPA unterscheidet. Es wird eine beispielanwendung zu untersuchen, das Sie generiert. Es zeigt eine SPA-Anwendungsentwurf und viele der Techniken zum Erstellen einer SPA.

Die Kinderspiel/Knockout-Vorlage ist eine Variation auf die [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md) ASP.NET und Web Tools 2012.2 Update enthalten. Die Kinderspiel SPA-Vorlage generiert eine Anwendung mit die gleiche benutzererfahrung, verfügt aber über eine andere Implementierung, die mit Kinderspiel für die datenverwaltung.

Die KnockoutJS SPA-Vorlage stellt Service Requests mit unformatierten jQuery AJAX eignet sich für eine einfache Anwendung. Jedoch höher entwickelten apps über erhöhte Anforderungen an Management-datenanforderungen. Um beispielsweise die meisten Anwendungen:

- Abfragen und den Server während einer Sitzungs für erweiterte Benutzer erneut abfragen.
- Fügen Sie die Abfragefiltern, Sortieren und paging.
- Verwenden Sie die gleichen Daten auf mehreren Bildschirmen.
- Sammeln von Änderungen auf viele Objekte, und speichern Sie diese als einzelne Transaktion.
- Überprüfen Sie Änderungen auf dem Client, damit der Benutzer Fehler behoben werden kann, vor dem Ausführen eines Commits für Änderungen an der Datenbank.

Die Bibliothek BreezeJS behandelt diese Arbeiten für Sie, die Sie zum Entwickeln der Anwendung Logik und die benutzerfreundlichkeit, die wichtigsten freigeben.

[**Kinderspiel** ](http://www.breezejs.com/?utm_source=ms-spa) ist eine open Source-Bibliothek zum Erstellen von umfangreichen datenanwendungen in JavaScript und HTML, die Arten von apps, die in der Vergangenheit als eigenständige desktopanwendungen übermittelt wurden.

Die Vorlage zum Kinderspiel/Knockout unterstützen Sie dieser erste entscheidenden Schritt in Richtung einer stabiler Daten Verwaltungsinfrastruktur nutzen. Es erzeugt eine Todo-beispielanwendung, die mit der Vorlage KnockoutJS SPA nach außen zeigende identisch ist. Auf der Innenseite es ersetzt die AJAX-Datenschicht mit Kinderspiel, fast erreicht Seite-an-Seite, damit Sie die beiden vergleichen können. Natürlich berührt es kaum das volle Potenzial von einer Anwendung zum Kinderspiel. Aber Sie sehen, wie zum Kinderspiel funktioniert und wie wenig ist erforderlich, um diesen Übergang übernehmen.

Fangen wir also an.

## <a name="create-a-breezeknockout-template-project"></a>Erstellen Sie ein Kinderspiel/Knockout-Vorlagenprojekt

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Die Vorlage wird als Visual Studio-Erweiterung (VSIX)-Datei verpackt. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

In der **neues Projekt** Assistenten **Kinderspiel Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Drücken Sie STRG + F5, um zu erstellen, und führen Sie die Anwendung ohne Debuggen aus, oder drücken Sie F5, um mit Debuggen auszuführen.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

Die Validierungslogik wird ausgeführt-Clientseite, indem zum Kinderspiel. Validierungsattributen für die Server-Modell-Klassen werden an den Client weitergegeben und automatisch ausgeführt werden, bevor der Client den Server kontaktiert.

Überprüfen Sie den Netzwerkdatenverkehr. Beachten Sie, dass wenn Kinderspiel ein Fehler erkannt wurden keine Aufrufe an den Server. Jede gültige Änderung führte zu einer POST-Anforderung auf "/ api/Todo/SaveChanges". Kinderspiel bündelt die Änderungen und sendet sie zusammen als eine einzelne Anforderung an die Web-API-Controller `SaveChanges` Methode. Unterscheidet sich vom KockoutJS SPA-Vorlage, wodurch PUT, POST und DELETE-Anforderungen für jedes Element einzeln.

## <a name="peek-inside"></a>Peek-in

Diese Anwendung verfügt über eine Clientseite und einer serverseitigen. Clientseitige Stapel besteht ein wenig HTML und eine Kombination aus JavaScript Anwendungsmodulen (im Ordner "app") plus Drittanbieter-JavaScript-Bibliotheken (im Ordner "Skripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Wenn Sie die Vorlage KnockoutJS SPA untersucht haben, sollte dies sehr bekannt vorkommen. Konzentrieren Sie sich auf den blauen Feldern. Die UI-Architektur ist Model-View-ViewModel (MVVM), in dem die HTML-Widgets der Sicht aus dem unterstützenden Presentation-Code in das Ansichtsmodell ordnungsgemäß getrennt sind. Ein Daten-Bindungssystem (in diesem Fall Knockout) koordiniert die Ansicht und das Modell anzeigen, sodass jeder ohne fundierte Kenntnisse der anderen Fehlerereignissen kann.

Das Modell kapselt die TODO-Daten. Entitäten im Modell werden erstellt, indem Kinderspiel mit Knockout Observable-Eigenschaften, damit sie direkt an Widgets in der Ansicht gebunden werden können. Das Ansichtsmodell fragt den Datenkontext zum Abrufen und speichern die Modellelemente. Der Datenkontext für Netzwerkgeräte delegiert die meiste Arbeit zum Kinderspiel.

Serverseitige Stapel besteht aus einigen Entwicklercode und drei Prinzip .NET Bibliotheken: Web-API und Entity Framework Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Die grundlegende Architektur ist identisch mit der KockoutJS SPA-Vorlage. Die Implementierung ist jedoch viel einfacher: der DTOs gelöscht wurden, und die meisten Details Entity Framework haben Breeze.NET zugewiesen wurde.

## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, Sie den Code untersuchen, durch MDX die [umfassende Erläuterung](http://www.breezejs.com/spa-template?utm_source=ms-spa) des Clients und der Server-Stapel auf der Website zum Kinderspiel.

Sie könnten versuchen, arbeiten mit Kinderspiel clientseitige Abfrage; Fügen Sie einige Filter und sortiert. Sie können weitere Modelleigenschaften und weitere Entitäten an, die eine bessere Gefühl für die End-to-End-SPA Entwicklung hinzufügen. Wenn Sie den Entwurf sicher sind, können Sie TODO-Funktionen entfernen und Ersetzen Sie sie durch Ihren eigenen.

Bereit für Nächstes big bald verlaufen: Hinzufügen von clientseitigen Bildschirme und zwischen ihnen navigieren. Sie diese Vorlage SPA hinterlassen und aktivieren Sie auf eine umfassendere SPA-Stapel, z. B. [John-Papa Hot Handtuch](https://github.com/johnpapa/HotTowel#readme "Hot Handtuch"), die Durandal aus der Mischung Kinderspiel und Knockout hinzufügt.
