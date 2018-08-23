---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iteration #7 – Hinzufügen von Ajax-Funktionen (VB) | Microsoft-Dokumentation'
author: microsoft
description: In der siebten Iteration verbessern wir die Reaktionsfähigkeit und Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b9c6ff228e73ce63f7a0b046110db656103d6d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835349"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iteration #7 – Hinzufügen von Ajax-Funktionen (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> In der siebten Iteration verbessern wir die Reaktionsfähigkeit und Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)
  

In dieser Reihe von Tutorials erstellen wir eine gesamte Anwendung Kontaktverwaltung ab Beginn um den Vorgang abzuschließen. Die Contact Manager-Anwendung können Sie zum Speichern von Kontaktinformationen - Namen, Telefonnummern und e-Mail-Adressen – eine Liste der Personen an.

Wir erstellen die Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir nach und nach der Anwendung. Das Ziel dieses mehrere Iteration-Ansatzes ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration wir der Contact Manager in der einfachsten maximal möglicher Größe erstellen. Wir fügen Unterstützung für grundlegender Datenbankvorgänge: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Optimieren der Anwendung gut. In dieser Iteration verbessern wir die Darstellung der Anwendung durch Ändern der Standard-Masterseite für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 – Hinzufügen der formularüberprüfung. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Stellen Sie Iteration #4 – lose koppeln der Anwendung. In dieser vierten Iteration nutzen wir einige Entwurfsmuster für Software zu verwalten und ändern Sie die Kontakt-Manager-Anwendung zu vereinfachen. Z. B. gestalten wir unsere Anwendung, die dem Repositorymuster und dem Dependency Injection-Muster verwenden.

- Iteration #5 – Erstellen von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und zu ändern, indem Sie die Komponententests hinzufügen. Wir unsere Data Model-Klassen modellieren und erstellen Sie Komponententests für unseren Controller und die Validierungslogik.

- Iteration #6 – Verwenden der testgesteuerten Entwicklung. In dieser Iteration sechsten fügen wir neue Funktionen zu unserer Anwendung zuerst Schreiben von Komponententests und Code für die Komponententests schreiben hinzu. In dieser Iteration fügen wir wenden Sie sich an Gruppen hinzu.

- Iteration #7 - Ajax-Funktionen hinzufügen. In der siebten Iteration verbessern wir die Reaktionsfähigkeit und Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax.

## <a name="this-iteration"></a>Diese Iteration

In dieser Iteration der Contact Manager-Anwendung gestalten wir unsere Anwendung zum Verwenden von Ajax. Durch die Nutzung von Ajax, stellen wir unsere Anwendung steigern der Reaktionsfähigkeit. Wir können vermeiden, eine ganze Seite rendern, wenn wir nur eine bestimmte Region auf einer Seite zu aktualisieren müssen.

Wir werden unsere Ansicht "Index" so umgestalten, dass wir t muss die gesamte Seite erneut Raten, wenn jemand eine neue Gruppe von Kontakte auswählt. Stattdessen klickt ein Benutzer eine Gruppe von Kontakte, wir nur die Liste der Kontakte aktualisieren und lassen den Rest der Seite allein.

Wir ändern außerdem die Möglichkeit, unsere Delete-link funktioniert. Anstatt eine separate Bestätigungsseite, werden wir ein JavaScript-Bestätigungsdialogfeld angezeigt. Wenn Sie bestätigen, dass Sie einen Kontakt zu löschen möchten, wird ein HTTP DELETE-Vorgang für den Server So löschen Sie den Kontaktdatensatz aus der Datenbank ausgeführt.

Darüber hinaus werden wir nutzen von jQuery Animationseffekte unserer Ansicht "Index" hinzu. Wir werden eine Animation anzeigen, wenn die neue Liste mit Kontakten vom Server abgerufen wird, ist.

Abschließend müssen wir die ASP.NET AJAX-Framework-Unterstützung für die Verwaltung von Browserverlauf nutzen. Wir erstellen von Verlaufspunkten, wenn wir auf einen Ajax-Aufruf zum Aktualisieren der Kontaktliste ausführen. Auf diese Weise der Browser rückwärts- und vorwärtsnavigationselement Schaltflächen wird funktionieren.

## <a name="why-use-ajax"></a>Gründe für die Verwendung von Ajax

Mithilfe von Ajax hat viele Vorteile. Eine Anwendung führt eine bessere benutzererfahrung zunächst Ajax-Funktionen hinzugefügt. In einer normalen Web-Anwendung muss die gesamte Seite zurück an den Server jedes Mal gebucht werden, das ein Benutzer eine Aktion ausführt. Wenn Sie eine Aktion ausführen, müssen die Browser-sperren und der Benutzer warten, bis die gesamte Seite abgerufen und erneut angezeigt wird.

Dies wäre eine unzulässige Erfahrungen mit einer Desktopanwendung. Aber in der Vergangenheit wir kurzlebig mit diesem Benutzer negatives Erlebnis im Falle einer Webanwendung, da wir nicht wusste, dass wir besser machen können. Wir dachten, dass wurde eine Einschränkung von Webanwendungen, in Wirklichkeit nur eine Einschränkung von unserem Imaginations war.

In einer Ajax-Anwendung Einbau Sie zusätzlichen t muss die Benutzeroberfläche zum Stillstand so aktualisieren Sie eine Seite zu bringen. Stattdessen können Sie eine asynchrone Anforderung im Hintergrund, um die Seite aktualisieren ausführen. Einbau zusätzlichen t-Force-den Benutzer warten, bis der Seite aktualisiert wird.

Durch die Nutzung von Ajax, können Sie auch die Leistung Ihrer Anwendung verbessern. Beachten Sie, wie die Kontakt-Manager-Anwendung ohne Ajax-Funktionalität jetzt funktioniert. Wenn Sie eine Gruppe von Kontakte klicken, muss die gesamte Ansicht "Index" erneut angezeigt werden. Die Liste der Kontakte und die Liste der Kontakt Gruppen müssen aus den Datenbankserver abgerufen werden. Alle diese Daten müssen über das Netzwerk vom Webserver an Webbrowser übergeben werden.

Nachdem wir unsere Anwendung von Ajax-Funktionen hinzugefügt haben, können wir jedoch die erneute Anzeigen der gesamten Seite, klickt ein Benutzer eine Gruppe von Kontakte vermeiden. Wir müssen nicht mehr die wenden Sie sich an Gruppen aus der Datenbank abrufen. Wir raten auch t muss die gesamte Ansicht "Index" über das Netzwerk übertragen. Durch die Nutzung von Ajax, reduzieren wir die Menge der Arbeit, die unsere Datenbank-Server ausführen müssen, und reduzieren wir die Menge des Netzwerkverkehrs, die von der Anwendung erforderlich sind.

## <a name="don-t-be-afraid-of-ajax"></a>Don ' t werden die Aufrüstung von Ajax

Einige Entwickler vermeiden Sie mithilfe von Ajax, da sie die älteren Browsern kümmern. Sie möchten sicherstellen, dass ihre Webanwendungen weiterhin funktionieren, wenn von einem Browser zugegriffen wird, die JavaScript nicht unterstützt. Da Ajax JavaScript abhängt, vermeiden Sie einige Entwickler mithilfe von Ajax.

Jedoch berücksichtigen, wie Sie Ajax implementieren möchten können dann Sie Anwendungen erstellen, die mit sowohl Uplevel und älteren Browsern funktionieren. Die Kontakt-Manager-Anwendung funktioniert mit Browsern mit Unterstützung für JavaScript und Browser, die nicht der Fall ist.

Wenn Sie die Kontakt-Manager-Anwendung mit einem Browser verwenden, die JavaScript unterstützt müssen eine bessere benutzererfahrung Sie. Z. B. Wenn Sie eine Gruppe von Kontakte klicken, wird nur in die Region der Seite, die Kontakte angezeigt aktualisiert werden.

Wenn Sie andererseits, Sie verwenden die Kontakt-Manager-Anwendung mit einem Browser, die JavaScript nicht unterstützt (oder, bei dem JavaScript deaktiviert) müssen eine etwas nicht das bevorzugte Tag Benutzeroberfläche Sie. Z. B. Wenn Sie eine Gruppe von Kontakte klicken, muss die gesamte Ansicht "Index" an den Browser gebucht werden zum Anzeigen der entsprechenden Liste der Kontakte.

## <a name="adding-the-required-javascript-files"></a>Hinzufügen der erforderlichen JavaScript-Dateien

Es müssen drei JavaScript-Dateien verwenden, um die Ajax-Funktionen, die Anwendung hinzuzufügen. Alle drei dieser Dateien befinden sich im Ordner "Scripts" einer neuen ASP.NET MVC-Anwendung.

Wenn Sie Ajax in mehrere Seiten in Ihrer Anwendung verwenden möchten stellt dann sinnvoll, die erforderlichen JavaScript-Dateien in Ihrer Anwendung s anzeigen-Masterseite einschließen. Auf diese Weise werden die JavaScript-Dateien in alle Seiten in Ihrer Anwendung automatisch berücksichtigt.

Fügen Sie der folgende JavaScript-Code enthält, in der &lt;Head&gt; Tag der Masterseite anzeigen:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Umgestaltung der Ansicht "Index", um Ajax zu verwenden.

Lassen Sie s zunächst, ändern unsere Ansicht "Index", sodass Sie auf eine Gruppe von Kontakten nur die Region der Sicht aktualisiert, die Kontakte angezeigt. Das rote Kästchen in Abbildung 1 enthält die Region, die aktualisiert werden sollen.


[![Nur Kontakte aktualisieren](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Abbildung 01**: nur Kontakte aktualisieren ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-7-add-ajax-functionality-vb/_static/image2.png))


Der erste Schritt ist der Teil der Ansicht zu trennen, der für die asynchrone Aktualisierung in eine separate partielle (User-Steuerelement) werden soll. Der Teil der Ansicht "Index", die die Tabelle von Kontakten anzeigt, wurde in die partielle in Codebeispiel 1 verschoben.

**1 – Views\Contact\ContactList.ascx auflisten**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Beachten Sie, dass die partielle in Codebeispiel 1 ein anderes Modell als Ansicht "Index". Die *Inherits* -Attribut in der &lt;% @ Page %&gt; Richtlinie gibt an, dass die partielle von den ViewUserControl erbt&lt;Gruppe&gt; Klasse.

Aktualisierte Ansicht "Index" ist im Codebeispiel 2 enthalten.

**Codebeispiel 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Es gibt zwei Dinge, die Sie über die aktualisierte Ansicht im Codebeispiel 2 sehen. Beachten Sie, dass der gesamte Inhalt in die partielle verschoben wird zunächst mit einem Aufruf von Html.RenderPartial() ersetzt. Die Html.RenderPartial()-Methode wird aufgerufen, wenn die Ansicht "Index" zunächst angefordert wird, um den anfänglichen Satz von Kontakten anzuzeigen.

Zweitens Beachten Sie, dass die Html.ActionLink() verwendet wird, wenden Sie sich an Gruppen anzeigen durch eine Ajax.ActionLink() ersetzt wurde. Die Ajax.ActionLink() wird mit den folgenden Parametern aufgerufen:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Der erste Parameter darstellt, des für den Link anzuzeigende Texts, der zweite Parameter stellt die Routenwerte und der dritte Parameter repräsentiert die Ajax-Optionen. In diesem Fall verwenden wir die UpdateTargetId Ajax-Option, um auf den HTML-Code zeigen &lt;Div&gt; Tag, das Aktualisieren, nachdem die Ajax-Anforderung abgeschlossen werden soll. Wir aktualisieren möchten die &lt;Div&gt; Tag mit der neuen Liste der Kontakte.

Die aktualisierte Index()-Methode des Controllers Kontakt wird in Programmausdruck 3 enthalten.

**Codebeispiel 3 - Controllers\ContactController.vb (Index-Methode)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Die aktualisierte Index()-Aktion gibt bedingt eine von zwei zurück. Die Index() Aktion aufgerufen wird, durch eine Ajax-Anforderung zurück, wenn der Controller eine partielle. Andernfalls gibt die Aktion Index() eine ganze Sicht zurück.

Beachten Sie, dass die Index()-Aktion nicht so viele Daten, die beim Aufrufen durch eine Ajax-Anforderung zurückgegeben. Im Kontext einer normalen Anforderung wird eine Liste aller wenden Sie sich an Gruppen und die ausgewählte Gruppe von Kontakten von Aktion des Indexes zurückgegeben. Im Kontext einer Ajax-Anforderung gibt die Aktion Index() nur die ausgewählte Gruppe zurück. AJAX bedeutet weniger Arbeit auf dem Datenbankserver.

Unsere geänderte Ansicht "Index" funktioniert im Fall von sowohl älteren als auch keine komplexe Browser. Wenn Sie auf eine Gruppe von Kontakten und Ihr Browser JavaScript unterstützt, wird nur in die Region der Sicht, die die Liste der Kontakte enthält aktualisiert. Wenn Sie auf der anderen Seite Ihr Browser JavaScript nicht unterstützt, wird die gesamte Ansicht aktualisiert.


Unsere aktualisierten Ansicht "Index" hat ein Problem. Wenn Sie eine Gruppe von Kontakte klicken, wird die ausgewählte Gruppe nicht hervorgehoben. Da die Liste der Gruppen außerhalb der Region angezeigt werden, die während einer Ajax-Anforderung aktualisiert wird, wird die richtige Gruppe nicht hervorgehoben. Dieses Problem korrigieren wir im nächsten Abschnitt.


## <a name="adding-jquery-animation-effects"></a>Hinzufügen von jQuery Animationseffekten

Normalerweise, wenn Sie einen Link auf einer Webseite klicken, können die Browser-Statusanzeige Sie erkennen, und zwar unabhängig davon, ob der Browser den aktualisierten Inhalt aktiv abgerufen wird. Beim Durchführen einer Ajax-Anforderung auf der anderen Seite zeigt die Statusanzeige der Browser keine Fortschritte. Dadurch können Benutzer nervös werden. Woher wissen Sie, ob der Browser fixiert hat?

Es gibt mehrere Möglichkeiten, die Sie für einen Benutzer angeben können, dass beim Ausführen einer Ajax-Anforderung Arbeiten ausgeführt werden. Ein Ansatz ist eine einfache Animation angezeigt. Beispielsweise können Sie sich eine Region abgeblendet, wenn eine Ajax-Anforderung beginnt und die Region eingeblendet, wenn die Anforderung abgeschlossen ist.

Wir verwenden die jQuery-Bibliothek, der Bestandteil von Microsoft ASP.NET MVC-Framework, um die Auswirkungen der Animation zu erstellen. Aktualisierte Ansicht "Index" ist in Listing 4 enthalten.

**Programmausdruck 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Beachten Sie, dass die aktualisierten Ansicht "Index" drei neue JavaScript-Funktionen enthält. Die ersten beiden Funktionen mithilfe von jQuery ausgeblendet werden sollen, und in der Liste der Kontakte eingeblendet, wenn Sie eine neue Gruppe von Kontakte klicken. Die dritte Funktion zeigt eine Fehlermeldung angezeigt, wenn ein Ajax-Anforderung zu einem Fehler (z. B. Netzwerktimeout).

Die erste Funktion übernimmt auch Hervorheben der ausgewählten Gruppe. Eine Klasse = ausgewählte Attribut an das übergeordnete Element (das LI-Element), der das Element geklickt wurde. In diesem Fall vereinfacht jQuery Auswahl der richtigen Elemente, und fügen Sie die CSS-Klasse.

Diese Skripts sind an die gruppenlinks mithilfe des Parameters Ajax.ActionLink() AjaxOptions gebunden. Der aktualisierte Ajax.ActionLink() Methodenaufruf sieht folgendermaßen aus:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Hinzufügen von Unterstützung für Browser-Verlauf

Wenn Sie einen Link zu eine Seite aktualisieren klicken, wird in der Regel Ihren Browserverlauf aktualisiert. Auf diese Weise können Sie die Browser-zurück-Taste, um zurück in den vorherigen Zustand der Seite Zeitpunkt zu verschieben, klicken. Z. B. Wenn Sie klicken auf die wenden Sie sich an Freunde-Gruppe, und klicken Sie dann auf die Gruppe "Business wenden Sie sich an", können Sie klicken Sie auf die Browser-zurück-Taste, um in den Zustand der Seite zu navigieren, wenn die Gruppe von Kontakte Freunde ausgewählt wurde.

Leider ist ein Ajax-Anforderung nicht Browserverlauf automatisch aktualisiert. Wenn Sie auf eine Gruppe von Kontakten und die Liste der übereinstimmenden Kontakte mit einer Ajax-Anforderung abgerufen wird, wird der Verlauf nicht aktualisiert. Sie können nicht die Browserschaltfläche zurück verwenden, zu einer Gruppe von Kontakten zurück zu navigieren, nach dem Auswählen einer neuen Gruppenstatus von Kontakten.

Benutzer können die Browserschaltfläche zurück verwenden soll Schaltfläche nach der Durchführung von Ajax-Anforderungen müssen Sie ein wenig mehr Arbeit übernehmen. Sie müssen den Browser Verlauf Verwaltungsfunktionen, die im ASP.NET AJAX-Framework erstellt nutzen.

ASP.NET AJAX-Browserverlauf, müssen Sie drei Dinge erreichen:

1. Aktivieren Sie Browserverlauf, indem die EnableBrowserHistory-Eigenschaft auf "true" festlegen.
2. Bei einer statusänderung einer Ansicht durch Aufrufen der Methode addHistoryPoint() von Verlaufspunkten zu speichern.
3. Der Zustand der Ansicht zu rekonstruieren, wenn die Navigate-Ereignis ausgelöst wird.

Aktualisierte Ansicht "Index" ist in Listing 5 enthalten.

**Programmausdruck 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

In Listing 5 ist die Browserverlauf in der pageInit()-Funktion aktiviert. Die pageInit()-Funktion wird auch verwendet, um den Ereignishandler für das Ereignis navigieren einzurichten. Die Navigate-Ereignis wird ausgelöst, wenn der Browser vorwärts oder zurück-Taste den Status der Seite wird, um zu ändern.

Die beginContactList()-Methode wird aufgerufen, wenn Sie eine Gruppe von Kontakte klicken. Diese Methode erstellt einen neuen Verlaufspunkt durch Aufrufen der addHistoryPoint()-Methode. Die Id der Gruppe "Kontakt" geklickt wird dem Verlauf hinzugefügt.

Die Gruppen-Id wird aus einem Expando-Attribut auf den Link für die Gruppe von Kontakten abgerufen. Die Verknüpfung wird mit dem folgenden Aufruf Ajax.ActionLink() gerendert.

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Der letzte Parameter, die an die Ajax.ActionLink() Fügt ein Expando-Attribut, das mit dem Namen Groupid auf den Link (Kleinbuchstaben für die XHTML-Kompatibilität).

Wenn ein Benutzer die Browserschaltfläche zurück oder die Schaltfläche "Weiterleiten" trifft, das Navigate-Ereignis wird ausgelöst, und die navigate()-Methode wird aufgerufen. Diese Methode aktualisiert die Kontakte auf der Seite entsprechend den Zustand der Seite, die der Browserverlaufspunkt an die Navigate-Methode übergeben entspricht, angezeigt.

## <a name="performing-ajax-deletes"></a>Ausführen von Ajax wird gelöscht

Aktuell, um einen Kontakt zu löschen, müssen Sie klicken auf den Link "löschen", und klicken Sie dann auf die Schaltfläche "löschen" in die Bestätigungsseite "löschen" angezeigt (siehe Abbildung 2). Dies scheint sehr viel Seitenanforderungen möchten einfach nur einen Datenbankdatensatz zu löschen.


[![Die Bestätigungsseite "löschen"](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Abbildung 02**: die Bestätigungsseite "löschen" ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Es ist verlockend, überspringen Sie die Bestätigungsseite "löschen", und Löschen eines Kontakts direkt aus der Ansicht "Index". Da bei diesem Ansatz Ihrer Anwendung zu Sicherheitslücken eröffnet, vermeiden Sie Versuchung. Im Allgemeinen Sie Ich möchte einen HTTP GET-Vorgang ausführen, wenn eine Aktion aufrufen, die den Zustand Ihrer Webanwendung ändert möchten. Wenn Sie einen Löschvorgang durchführen zu können, möchten Sie zum Ausführen von HTTP POST oder besser noch, um eine HTTP DELETE-Vorgang.

Der Link "löschen" ist in der partiellen ContactList enthalten. Eine aktualisierte Version der partiellen ContactList ist im Codebeispiel 6 enthalten.

**Codebeispiel 6: Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Der Link "löschen" wird mit dem folgenden Aufruf der Methode Ajax.ImageActionLink() gerendert:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Die Ajax.ImageActionLink() ist kein standardmäßigen Bestandteil von ASP.NET MVC-Framework. Die Ajax.ImageActionLink() ist eine benutzerdefinierte Helper-Methoden, die im Contact Manager-Projekt enthalten ist.


Der AjaxOptions-Parameter verfügt über zwei Eigenschaften. Zuerst wird die Confirm-Eigenschaft verwendet, um ein Popup-JavaScript-Bestätigungsdialogfeld angezeigt. Zweitens wird die HttpMethod-Eigenschaft verwendet, um eine HTTP DELETE-Vorgang auszuführen.

Codebeispiel 7 enthält eine neue AjaxDelete()-Aktion, die mit dem Contact-Controller hinzugefügt wurde.

**Auflisten von 7 – Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Die Aktion AjaxDelete() wird mit einem AcceptVerbs-Attribut markiert. Dieses Attribut verhindert, dass die Aktion aus, die mit Ausnahme von den einzelnen HTTP-Vorgängen als eine HTTP DELETE-Vorgang aufgerufen wird. Insbesondere kann nicht dadurch mit HTTP GET aufgerufen werden.

Nachdem Sie Datenbank-Datensatz löschen, müssen Sie die aktualisierte Liste der Kontakte anzeigen, die nicht den gelöschten Datensatz enthält. Die AjaxDelete()-Methode gibt die partielle ContactList und die aktualisierte Liste der Kontakte zurück.

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir den Ajax-Funktionen zu unserer Contact Manager-Anwendung hinzugefügt. Wir verwendet Ajax, um die Reaktionsfähigkeit und Leistung der Anwendung zu verbessern.

Zunächst umgestaltet wir die Ansicht "Index", damit Sie auf eine Gruppe von Kontakten nicht die gesamte Ansicht aktualisiert wird. Stattdessen aktualisiert die auf eine Gruppe von Kontakten nur die Liste der Kontakte.

Als Nächstes verwendet haben wir jQuery Animationseffekte ausblenden und einblenden, in der Liste der Kontakte. Hinzufügen von Animationen zu einer Ajax-Anwendung kann verwendet werden, um Benutzer der Anwendung durch den entspricht einer Statusanzeige Browser bereitzustellen.

Wir werden auch browserverlaufsunterstützung, die Ajax-Anwendung hinzugefügt. Wir werden die Benutzer klicken Sie auf die Browserschaltfläche zurück und Vorwärts-Schaltflächen, um den Zustand der Ansicht "Index" ändern konnten.

Schließlich haben wir einen Link "löschen", die HTTP-DELETE-Operationen unterstützt. Durch Ausführen von Ajax löscht, können wir Benutzer Datenbankdatensätze zu löschen, ohne dass der Benutzer eine zusätzliche Delete-Seite "Bestätigung" anfordern.

> [!div class="step-by-step"]
> [Vorherige](iteration-6-use-test-driven-development-vb.md)
