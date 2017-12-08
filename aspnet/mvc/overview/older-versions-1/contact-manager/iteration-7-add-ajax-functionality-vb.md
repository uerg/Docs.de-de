---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: "Iteration #7 – Add Ajax-Funktionen (VB) | Microsoft Docs"
author: microsoft
description: "In der siebten Iteration werden die Reaktionsfähigkeit und die Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax verbessern."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: fa50fdea8ac165be3f8e96322ec049196a511ebe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iteration #7 – Add Ajax-Funktionen (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> In der siebten Iteration werden die Reaktionsfähigkeit und die Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax verbessern.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)
  

In dieser Reihe von Lernprogrammen erstellen wir eine vollständige Contact Management-Anwendung von Grund auf Fertig stellen. Die Kontakt-Manager-Anwendung können Sie zum Speichern von Kontaktinformationen - Namen, Telefonnummern und e-Mail-Adressen - eine Liste der Personen.

Erstellen der Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir allmählich die Anwendung. Das Ziel dieses Ansatzes für mehrere Iteration ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration erstellen wir die Kontakt-Manager in der einfachsten möglich. Wir fügen die Unterstützung für die grundlegenden Datenbankvorgängen: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Stellen Sie die Anwendung gut aussehen. In dieser Iteration haben wir die Darstellung der Anwendung verbessern, indem Ändern der Standard-Gestaltungsvorlage für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 - formularvalidierung hinzufügen. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen e-Mail-Adressen und Telefonnummern.

- Iteration #4 – Stellen Sie die Anwendung, die lose miteinander verbunden. In dieser dritten Iteration nutzen wir mehrere Software-Entwurfsmuster, damit sie leichter zu verwalten und ändern die Kontakt-Manager-Anwendung. Beispielsweise gestalten wir unsere Anwendung in das Repository und die Abhängigkeitsinjektion-Muster verwenden.

- Iteration #5: Erstellen von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und ändern, indem Sie Komponententests hinzufügen. Wir unsere Daten Modellklassen modellieren und erstellen Sie Komponententests für unsere Controller und die Validierungslogik.

- Iteration #6 – verwenden Sie eine testgesteuerte Entwicklung. In dieser Iteration sechste wir neue Funktionalität Hinzufügen der Anwendung durch Schreiben von Komponententests zuerst und Code für die Komponententests schreiben. In dieser Iteration fügen wir Kontakt Gruppen hinzu.

- Iteration #7 - Ajax-Funktionen hinzufügen. In der siebten Iteration werden die Reaktionsfähigkeit und die Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax verbessern.

## <a name="this-iteration"></a>Diese Iteration

In dieser Iteration der Kontakt-Manager-Anwendung gestalten wir unsere Anwendung Ajax verwenden. Durch die Nutzung von Ajax, stellen wir unsere Anwendung anpassbar. Wir können vermeiden, eine gesamte Seite gerendert, wenn wir benötigen nur eine bestimmte Region auf einer Seite zu aktualisieren.

Wir werden unsere Indexansicht so umgestalten, dass wir t muss die gesamte Seite erneut Verschlüsselungskennwort, wenn jemand eine neue Gruppe von Kontakte auswählt. Stattdessen klickt ein Benutzer eine Gruppe von Kontakte, wir nur die Liste der Kontakte aktualisieren, und lassen den Rest der Seite allein.

Ändern wir auch die Möglichkeit, die unsere Delete link funktioniert. Anstatt eine separate Bestätigungsseite zeigen wir ein Bestätigungsdialogfeld JavaScript. Wenn Sie bestätigen, dass Sie einen Kontakt zu löschen möchten, wird ein HTTP DELETE-Vorgang für den Server So löschen Sie den Kontaktdatensatz aus der Datenbank ausgeführt.

Darüber hinaus werden wir unsere Indexansicht Animationseffekte hinzuzufügende jQuery nutzen. Wir müssen eine Animation anzeigen, wenn die neue Liste mit Kontakten vom Server abgerufen werden.

Schalten Sie schließlich Vorteil von ASP.NET AJAX-Framework-Unterstützung für die Verwaltung von Browserverlauf. Wir erstellen Verlaufspunkten, sobald wir einen Ajax-Aufruf zum Aktualisieren der Kontaktliste ausführen. Auf diese Weise der Browser rückwärts und Vorwärts-Schaltflächen werden arbeiten.

## <a name="why-use-ajax"></a>Gründe für die Verwendung von Ajax

Mithilfe von Ajax weist viele Vorteile. Eine Anwendung führt eine bessere benutzererfahrung zunächst Ajax-Funktionen hinzugefügt. In einer normalen Web-Anwendung muss die gesamte Seite zurück an den Server werden jedes Mal zurückgesendet, das ein Benutzer eine Aktion ausführt. Wenn Sie eine Aktion ausführen, müssen der Browser sperren und der Benutzer warten Sie, bis die gesamte Seite abgerufen und angezeigt wird.

Das wäre eine unzulässige Erfahrung im Falle einer desktop-Anwendung. Aber bisher wir entsprechen mit diesem Benutzer negatives Erlebnis bei einer Webanwendung, da nicht bekannt, dass wir alle besser machen können. Wir dachten, dass eine Einschränkung des Webanwendungen angezeigt wurde, die in Wirklichkeit nur eine Einschränkung des unsere Imaginations war.

In einer Ajax-Anwendung Verschlüsselungskennwort Sie t muss die Benutzeroberfläche zum Stillstand nur auf eine Seite aktualisieren zu bringen. Stattdessen können Sie eine asynchrone Anforderung im Hintergrund, um die Seite aktualisieren ausführen. Sie Verschlüsselungskennwort t erzwingen den Benutzer warten, während die auf der Seite aktualisiert wird.

Durch die Nutzung von Ajax, können Sie auch die Leistung Ihrer Anwendung verbessern. Erwägen Sie, wie die Kontakt-Manager-Anwendung ohne Ajax-Funktionen jetzt funktioniert. Wenn Sie eine Gruppe von Kontakte klicken, muss die gesamte Indexansicht erneut angezeigt. Die Liste der Kontakte und Liste der Kontakt Gruppen müssen vom Datenbankserver abgerufen werden. Alle diese Daten müssen über das Netzwerk vom Webserver an Webbrowser übergeben werden.

Nachdem wir unsere Anwendung Ajax-Funktionen hinzugefügt haben, können wir jedoch die erneute Anzeigen der gesamten Seite klickt ein Benutzer eine Gruppe von Kontakte vermieden. Wir müssen nicht mehr auf die Kontakten-Gruppen aus der Datenbank. Wir raten auch t muss die gesamte Indexansicht über das Netzwerk übertragen. Durch die Nutzung von Ajax, wir verringern Sie die Menge der Arbeit, die unsere Datenbankserver ausführen müssen, und wir unsere Anwendung erforderlichen Netzwerkverkehr reduzieren.

## <a name="don-t-be-afraid-of-ajax"></a>Ich möchte werden sorgen von Ajax

Einige Entwickler vermeiden Sie die Verwendung von Ajax, da diese älteren Browsern kümmern. Sie möchten sicherstellen, dass ihre Webanwendungen weiterhin funktionieren, wenn von einem Browser zugegriffen, die JavaScript nicht unterstützt. Da JavaScript Ajax abhängig ist, vermeiden Sie einige Entwickler mithilfe von Ajax.

Jedoch wenn Sie sorgfältig wie Ajax implementieren können dann Sie Anwendungen erstellen, die mit Browsern komplexe und Downlevel arbeiten. Unsere Kontakt-Manager-Anwendung funktioniert mit Browsern mit Unterstützung für JavaScript und Browser, die nicht der Fall.

Wenn Sie die Kontakt-Manager-Anwendung mit einem Browser verwenden, die JavaScript unterstützt werden, müssen Sie eine bessere benutzererfahrung. Z. B. Wenn Sie eine Gruppe von Kontakte klicken, wird nur der Bereich der Seite, die Kontakte zeigt aktualisiert werden.

Wenn andererseits, Sie die Kontakt-Manager-Anwendung mit einem Browser verwenden unterstützt keine JavaScript (oder mit dem JavaScript deaktiviert) werden, müssen Sie eine etwas weniger wünschenswerte Benutzeroberfläche. Z. B. Wenn Sie eine Gruppe von Kontakte klicken, muss die gesamte Indexansicht zurück an den Browser veröffentlicht werden damit die entsprechende Liste der Kontakte anzuzeigen.

## <a name="adding-the-required-javascript-files"></a>Hinzufügen der erforderlichen JavaScript-Dateien

Es müssen drei JavaScript-Dateien verwenden, um unsere Anwendung Ajax-Funktionen hinzugefügt. Alle drei dieser Dateien sind im Ordner Scripts, einer neuen ASP.NET MVC-Anwendung enthalten.

Wenn Sie beabsichtigen, Ajax auf mehreren Seiten in Ihrer Anwendung verwenden stellt dann sinnvoll, die erforderlichen JavaScript-Dateien in Ihrer Anwendung s Ansicht-Masterseite einschließen. Auf diese Weise werden die JavaScript-Dateien in alle Seiten in der Anwendung automatisch einbezogen werden.

Fügen Sie der folgenden JavaScript-Code enthält, innerhalb der &lt;Head&gt; Tag der Masterseite anzeigen:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Umgestaltung die Indexansicht zur Verwendung von Ajax

Lassen Sie s starten, indem Sie unsere Indexansicht ändern, damit Sie auf eine Gruppe von Kontakten nur den Bereich der Sicht aktualisiert, die Kontakte werden angezeigt. Das rote Feld in Abbildung 1 enthält die Region, die aktualisiert werden sollen.


[![Nur Kontakte aktualisieren](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Abbildung 01**: nur Kontakte aktualisieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-7-add-ajax-functionality-vb/_static/image2.png))


Der erste Schritt ist der Teil der Ansicht zu trennen, die asynchron in eine separate partielle (Benutzer-Steuerelement) aktualisiert werden sollen. Im Abschnitt die Indexansicht, in dem die Tabelle von Kontakten angezeigt wurde in der partiellen im Codebeispiel 1 verschoben.

**1 – Views\Contact\ContactList.ascx auflisten**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Beachten Sie, dass der partiellen im Codebeispiel 1 ein anderes Modell als die Indexansicht verfügt. Die *Inherits* Attribut in der &lt;% @ Page %&gt; Richtlinie gibt an, dass die partielle von den ViewUserControl erbt&lt;Gruppe&gt; Klasse.

Die aktualisierte Indexansicht ist im Codebeispiel 2 enthalten.

**Auflisten von 2 – Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Es gibt zwei Dinge, die Sie über die aktualisierte Ansicht im Codebeispiel 2 beachten sollten. Beachten Sie, dass alle Inhalte in der partiellen verschoben wird zunächst mit einem Aufruf von Html.RenderPartial() ersetzt. Die Html.RenderPartial()-Methode wird aufgerufen, wenn die Indexansicht zuerst angefordert wird, um den anfänglichen Satz von Kontakten anzuzeigen.

Zweitens, beachten Sie, dass die Html.ActionLink() verwendet, um Kontakt Gruppen angezeigt werden. durch eine Ajax.ActionLink() ersetzt wurde. Die Ajax.ActionLink() wird mit den folgenden Parametern aufgerufen:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Der erste Parameter stellt den Text, der für den Link angezeigt, der zweite Parameter darstellt, die Routenwerte und der dritte Parameter dar, die Ajax-Optionen. In diesem Fall wir die UpdateTargetId Ajax-Option verwenden, um auf den HTML-Code zeigen &lt;Div&gt; Tag, das nach Abschluss der Ajax-Anforderung aktualisiert werden sollen. Wir aktualisieren möchten die &lt;Div&gt; Tag durch die neue Liste von Kontakten.

Auflisten von 3 enthalten die aktualisierte Index()-Methode des Controllers wenden Sie sich an.

**Auflisten von 3 – Controllers\ContactController.vb (Index-Methode)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

Die aktualisierte Index()-Aktion gibt bedingt mit eins von zwei Elementen. Wenn die Aktion Index() von einer Ajax-Anforderung aufgerufen wird gibt der Controller eine partielle zurück. Andernfalls wird die Aktion Index() eine ganze Sicht zurückgegeben.

Beachten Sie, dass die Index() Aktion nicht so viele Daten, die beim Aufrufen durch eine Ajax-Anforderung zurückgegeben. Im Rahmen einer normalen Anforderung wird eine Liste aller Kontakten Gruppen und der ausgewählten Gruppe von Kontakten durch die Index-Aktion zurückgegeben. Im Rahmen einer Ajax-Anforderung gibt die Aktion Index() nur die ausgewählte Gruppe zurück. AJAX bedeutet weniger Arbeit auf dem Datenbankserver.

Unsere geänderte Indexansicht funktioniert im Fall von Browsern komplexe und mit Vorgängerversionen. Wenn Sie auf eine Gruppe von Kontakten und Ihr Browser JavaScript unterstützt, wird nur der Bereich der Sicht, die die Liste der Kontakte enthält aktualisiert. Wenn andererseits, Ihr Browser JavaScript nicht unterstützt, wird die komplette Ansicht aktualisiert.


Unsere aktualisierte Indexansicht hat ein Problem. Wenn Sie eine Gruppe von Kontakte klicken, wird die ausgewählte Gruppe nicht hervorgehoben. Da die Liste der Gruppen außerhalb der Region angezeigt werden, die während einer Ajax-Anforderung aktualisiert wird, wird die richtige Gruppe nicht hervorgehoben. Dies wird im nächsten Abschnitt dieses Problem korrigiert werden.


## <a name="adding-jquery-animation-effects"></a>Hinzufügen von jQuery-Animationseffekte

Wenn Sie einen Link auf einer Webseite klicken, können Sie erkennen, und zwar unabhängig davon, ob der Browser aktiv Abrufen von aktualisierten Inhalten ist in der Regel wird die Statusanzeige Browser verwenden. Bei der Ausführung einer Ajax-Anforderung auf der anderen Seite zeigt die Browser-Statusanzeige keine Status. Dadurch kann Benutzer nervös vornehmen. Woher wissen Sie, ob der Browser fixiert wurde?

Es gibt mehrere Möglichkeiten, die Sie für einen Benutzer angeben können, dass die Arbeit beim Ausführen einer Ajax-Anforderung ausgeführt wird. Ein besteht Ansatz darin, eine einfache Animation angezeigt. Beispielsweise können Sie aus einer Region abgeblendet, wenn eine Ajax-Anforderung beginnt und in der Region abgeblendet, wenn die Anforderung abgeschlossen wird.

Wir verwenden die jQuery-Bibliothek die mit dem Microsoft ASP.NET MVC-Framework zum Erstellen der Animationseffekte enthalten ist. Die aktualisierte Indexansicht ist im Codebeispiel 4 enthalten.

**4 – Views\Contact\Index.aspx auflisten**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Beachten Sie, dass die aktualisierte Indexansicht drei neue JavaScript-Funktionen enthält. Die ersten beiden Funktionen verwenden jQuery ausblenden und einblenden in der Liste der Kontakte, wenn Sie eine neue Gruppe von Kontakte klicken. Die dritte Funktion zeigt eine Fehlermeldung angezeigt, wenn eine Ajax-Anforderung führt zu einem Fehler (z. B. Netzwerktimeout).

Die erste Funktion übernimmt außerdem die ausgewählte Gruppe hervorheben. Eine Klasse = ausgewählte Attribut wird hinzugefügt, zum übergeordneten Element (das LI-Element) des Elements geklickt haben. In diesem Fall erleichtert jQuery Auswahl der richtigen Elemente, und fügen Sie die CSS-Klasse.

Diese Skripts werden an die Gruppe Links mit Hilfe des Parameters Ajax.ActionLink() AjaxOptions gebunden. Der aktualisierte Ajax.ActionLink() Methodenaufruf sieht wie folgt:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Hinzufügen von Unterstützung für Browser-Verlauf

Wenn Sie einen Link zu eine Seite aktualisieren klicken, wird in der Regel wird der Browserverlauf aktualisiert. Auf diese Weise können Sie die Browserschaltfläche "zurück" Zeit wieder in den vorherigen Zustand der Seite verschieben klicken. Z. B. Wenn Sie klicken Sie auf die Gruppe von Kontakten Freunde, und klicken Sie dann auf Kontakt Geschäftsgruppe, können Sie klicken die Browserschaltfläche "zurück" in den Zustand der Seite navigieren, wenn in der Gruppe von Kontakte Freunde ausgewählt wurde.

Leider aktualisiert Ausführen einer Ajax-Anforderung nicht Browserverlauf automatisch. Wenn Sie eine Gruppe von Kontakten klicken Sie auf, und die Liste der übereinstimmenden Kontakte mit einer Ajax-Anforderung abgerufen wird, wird der Browserverlauf nicht aktualisiert. Die Browserschaltfläche "zurück" können um an eine Gruppe von Kontakten zu navigieren, nachdem Sie eine neue Gruppe von Kontakte ausgewählt haben.

Wenn Benutzer in der Lage, verwenden Sie den Browser zurück Schaltfläche sollen nach der Durchführung von Ajax-Anforderungen müssen Sie ein wenig mehr Arbeit ausführen. In diesem Fall müssen Sie eine der Browser Verlauf Verwaltungsfunktionalität in das ASP.NET AJAX-Framework erstellten nutzen.

ASP.NET AJAX-Browserverlauf, müssen Sie drei Aktionen ausgeführt werden:

1. Aktivieren Sie Browserverlauf, indem die EnableBrowserHistory-Eigenschaft auf "true" festzulegen.
2. Bei der statusänderung einer Ansicht durch Aufrufen der Methode addHistoryPoint() Verlaufspunkten zu speichern.
3. Rekonstruieren Sie den Status der Sicht aus, wenn Navigate-Ereignis ausgelöst wird.

Die aktualisierte Indexansicht ist im Codebeispiel 5 enthalten.

**5 – Views\Contact\Index.aspx auflisten**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Auflisten von 5 ist Browserverlauf in der pageInit()-Funktion aktiviert. Die Funktion pageInit() dient auch zum Einrichten des Ereignishandler für das Ereignis navigieren. Navigate-Ereignis wird ausgelöst, wenn der Browser nach vorne oder die Schaltfläche "zurück" den Status der Seite verursacht zu ändern.

Die beginContactList()-Methode wird aufgerufen, wenn Sie eine Gruppe von Kontakte klicken. Diese Methode erstellt einen neuen Verlaufspunkt durch Aufrufen der addHistoryPoint()-Methode. Die Id der Gruppe "Kontakt" geklickt wird dem Verlauf hinzugefügt.

Die Gruppen-Id wird aus einem Expando-Attribut auf den Link für die Gruppe von Kontakten abgerufen. Der Link ist mit dem folgenden Aufruf Ajax.ActionLink() gerendert.

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Der letzte Parameter übergeben, um die Ajax.ActionLink() Fügt eine Expando-Attribut, das mit dem Namen Groupid auf den Link (Kleinbuchstaben für XHTML-Kompatibilität).

Wenn ein Benutzer den Browser zurück oder die Schaltfläche "Weiterleiten" trifft, das Navigate-Ereignis wird ausgelöst, und die navigate()-Methode aufgerufen wird. Diese Methode aktualisiert die Kontakte auf Übereinstimmung mit dem Status der Seite, die der Browserverlaufspunkt an die Navigate-Methode übergeben entspricht der Seite angezeigt.

## <a name="performing-ajax-deletes"></a>Löscht das Ausführen von Ajax

Aktuell, um einen Kontakt zu löschen, müssen Sie auf den Link klicken, und klicken Sie dann auf die Schaltfläche "löschen" in der Delete-Bestätigungsseite angezeigt (siehe Abbildung 2). Dies scheint ein Großteil Seitenanforderungen Serveradministratorkonto wie Datensatzes in einer Datenbank löschen möchten.


[![Der Bestätigungsseite löschen](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Abbildung 02**: die Seite "Bestätigung löschen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Es ist verlockend, überspringen die Seite "Bestätigung löschen", und Löschen eines Kontakts direkt über die Indexansicht. Da bei diesem Ansatz die Anwendung zu Sicherheitslücken öffnet, vermeiden Sie diese Versuchung. Im Allgemeinen Sie Ich möchte einen HTTP GET-Vorgang ausführen, wenn eine Aktion aufrufen, die den Status Ihrer Webanwendung ändert möchten. Beim Ausführen einer Delete HTTP POST oder besser noch, einen HTTP DELETE-Vorgang werden soll.

Die Delete-Link ist in der partiellen ContactList enthalten. Auflisten von 6 ist eine aktualisierte Version der partiellen ContactList enthalten.

**6 – Views\Contact\ContactList.ascx auflisten**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Die Delete-Link wird mit den folgenden Aufruf der Methode Ajax.ImageActionLink() gerendert:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Die Ajax.ImageActionLink() ist ein Standardbestandteil des ASP.NET MVC-Frameworks. Die Ajax.ImageActionLink() ist eine benutzerdefinierte Hilfsmethoden, die im Projekt Contact Manager enthalten.


Der AjaxOptions-Parameter verfügt über zwei Eigenschaften. Zunächst wird die bestätigen-Eigenschaft verwendet, um ein Popup-JavaScript-Bestätigungsdialogfeld angezeigt. Zweitens wird der HttpMethod-Eigenschaft verwendet, um eine HTTP DELETE-Operation durchzuführen.

Auflisten von 7 enthält eine neue AjaxDelete()-Aktion, die mit dem Kontakt-Controller hinzugefügt wurde.

**Auflisten von 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

Die AjaxDelete()-Aktion ist mit einem AcceptVerbs-Attribut ergänzt. Dieses Attribut wird verhindert, dass die Aktion aus, mit Ausnahme von jeder HTTP-Vorgang als eine HTTP DELETE-Vorgang aufgerufen wird. Insbesondere kann nicht dadurch mit HTTP GET aufgerufen werden.

Nachdem Sie Datenbank-Datensatz gelöscht haben, müssen Sie die aktualisierte Liste der Kontakte anzeigen, die nicht mit den gelöschten Datensatz enthält. Die AjaxDelete()-Methode gibt die partielle ContactList und die aktualisierte Liste der Kontakte zurück.

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir unsere Kontakt-Manager-Anwendung die Ajax-Funktionen hinzugefügt. Ajax verwendet, um die Reaktionsfähigkeit und die Leistung der Anwendung zu verbessern.

Zunächst haben wir die Indexansicht umgestaltet, damit Sie auf eine Gruppe von Kontakten nicht die komplette Ansicht aktualisieren. Stattdessen aktualisiert die auf eine Gruppe von Kontakten nur die Liste der Kontakte.

Als Nächstes verwendet haben wir jQuery-Animationseffekte ausblenden und einblenden in der Liste der Kontakte. Hinzufügen von Animationen zu einer Ajax-Anwendung kann verwendet werden, um den Benutzern der Anwendung durch das Äquivalent einer Statusanzeige Browser bereitzustellen.

Wir haben auch Verlauf Browserunterstützung unsere Ajax-Anwendung. Wir aktiviert Benutzer klicken Sie auf den Browser zurück und Vorwärts-Schaltflächen, um den Status der Indexansicht ändern.

Schließlich haben wir einen Delete-Link, der HTTP-DELETE-Operationen unterstützt. Durch Ausführen von Ajax löscht, können wir Benutzer Datenbankdatensätze zu löschen, ohne dass der Benutzer eine zusätzliche Delete Bestätigungsseite anfordern.

>[!div class="step-by-step"]
[Zurück](iteration-6-use-test-driven-development-vb.md)
