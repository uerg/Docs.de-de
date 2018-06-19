---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Verwenden von AJAX zum Übermitteln von dynamischen | Microsoft Docs
author: microsoft
description: Schritt 10 implementiert die Unterstützung für angemeldete Benutzer auf Antwort ihr Interesse an der Teilnahme an einem Essen, mit einem Ajax-basierten Ansatz, der innerhalb der Dinner Detail integriert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870179"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Mithilfe von AJAX dynamische Updates bereitstellen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 10 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 10 implementiert die Unterstützung für angemeldete Benutzer auf Antwort ihr Interesse an der Teilnahme an einem Essen, mit einem Ajax-basierten Ansatz, der innerhalb der Dinner Detailseite integriert.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner Schritt 10: AJAX aktivieren Einladung akzeptiert

Nehmen wir jetzt implementieren die Unterstützung für angemeldete Benutzer auf ihr Interesse an der Teilnahme an einem Dinner RSVP. Dadurch wird es mit einem AJAX-basierten Ansatz, der innerhalb der Dinner Detailseite integriert.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Gibt an, ob der Benutzer auf die Antwort gebeten wird

Beim Besuch können die */Dinners/Informationen / [Id*] URL, um Details zu einem bestimmten Dinner finden Sie unter:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Die Aktionsmethode wird implementiert Details() wie folgt:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Unsere erste Schritt beim Implementieren der Unterstützung der Antwort werden unsere Dinner-Objekt (innerhalb der Dinner.cs partiellen Klasse, die wir zuvor erstellt) eine Hilfsmethode "IsUserRegistered(username)" hinzugefügt. Diese Hilfsmethode gibt "true" oder "false", je nachdem, ob der Benutzer derzeit für die Dinner um Antwort gebeten wird:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Wir können klicken Sie dann den folgenden Code hinzufügen, um unsere "Details.aspx" haben-Ansicht-Vorlage zum Anzeigen einer entsprechenden Meldung gibt an, ob der Benutzer registriert ist oder nicht für das Ereignis:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Und jetzt Wenn ein Benutzer eine Dinner besucht für die sie registriert werden sie sehen diese Meldung:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Und wenn sie eine Dinner sie nicht registriert besuchen, sind für die sie sehen die folgende Meldung:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementieren die Register-Aktion-Methode

Jetzt fügen Sie die Funktionalität zum Aktivieren von Benutzern auf die Antwort für eine Dinner Seite "Details" erforderlich sind.

Um dies zu implementieren, wir erstellen eine neue "RSVPController"-Klasse indem Sie mit der rechten Maustaste auf das Verzeichnis \Controllers und Add -&gt;Controller Menübefehl aus.

Implementieren wir eine Aktionsmethode "Register" innerhalb der neuen RSVPController-Klasse, die eine Id für eine Dinner als Argument akzeptiert, ruft das entsprechende Dinner-Objekt, geprüft, wenn der angemeldete Benutzer derzeit in der Liste der Benutzer ist, die für sie registriert haben und nicht Fügt ein Objekt für die Antwort für sie:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Beachten Sie, über wie wir eine einfache Zeichenfolge als Ausgabe der Aktionsmethode zurückgegeben werden. Wir konnten diese Nachricht in eine Vorlage anzeigen – eingebettet haben, da es so klein ist. wir verwenden ihn aber nur die Hilfsmethode Content() auf die Basisklasse für Controller und der Rückgabewert eine zeichenfolgenmeldung wie oben.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Aufrufen der RSVPForEvent-Aktionsmethode, die mithilfe von AJAX

Wir verwenden AJAX aufzurufenden Aktionsmethode Register aus unserem Detailansicht. Implementieren Dies ist recht einfach. Zunächst fügen wir zwei Skriptverweise-Bibliothek:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

Die erste Bibliothek verweist auf die ASP.NET AJAX-Skriptdatei auf Clientseite Kernbibliothek. Diese Datei ist ca. 24 KB (komprimiert) und die clientseitige AJAX-Kernfunktionalität enthält. Die zweite Bibliothek enthält Hilfsfunktionen, die in integrierten ASP.NETs AJAX-Hilfsmethoden integriert (das in Kürze verwendet wird).

Wir können führt Update der Vorlagencode anzeigen, die wir zuvor hinzugefügt haben, sodass statt Outputing eine Meldung "Sie sind nicht für dieses Ereignis registriert", es stattdessen einem Link, der gerendert werden beim übertragen soll einen AJAX-Aufruf, der unsere RSVPForEvent Aktionsmethode auf unsere Antwort Controller aufruft und den Benutzer RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Die oben verwendete Ajax.ActionLink()-Hilfsmethode wird erstellt in ASP.NET MVC und ähnelt die Hilfsmethode Html.ActionLink(), anstatt einen Standardnavigation einen AJAX-Aufruf an die Aktionsmethode vereinfacht, wenn auf der Link geklickt wird. Wir sind über Aufrufen die Aktionsmethode "Registrieren" auf dem Controller "Antwort" und die DinnerID als Parameter "Id" an sie übergibt. Der letzte AjaxOptions-Parameter übergeben werden gibt an, dass wir verwenden möchten, nehmen den Inhalt von der Aktionsmethode zurückgegeben und aktualisieren den HTML-Code &lt;Div&gt; Element auf der Seite, deren Id ist "Rsvpmsg".

Und jetzt Wenn ein Benutzer auf eine Dinner durchsucht sie sind nicht registriert, für noch sehen sie einen Link zur Antwort dafür:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Wenn sie den Link "Antwort für dieses Ereignis" auf treffen sie einen AJAX-Aufruf an die Register-Aktion-Methode auf dem Controller Antwort und bei Abschluss sehen sie eine aktualisierte Meldung wie im folgenden:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Die Netzwerkbandbreite und der Datenverkehr beteiligt, wenn diese AJAX-Aufruf wirklich einfache ist. Klickt der Benutzer auf den Link "Für dieses Ereignis Antwort", kleine HTTP POST-Netzwerk wird eine Anforderung an die */Dinners/Register/1* URL, die bei der Übertragung unten aussieht:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Und die Antwort vom unsere Aktionsmethode Register ist einfach:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Diese einfache Aufruf ist schnell und funktioniert auch über ein langsames Netzwerk.

### <a name="adding-a-jquery-animation"></a>Hinzufügen von einem jQuery-Animationen

Die AJAX-Funktionalität, die wir implementiert funktioniert gut und schnell. In einigen Fällen kann es so schnell, aber passieren, dass ein Benutzer nicht bemerkt, dass die Antwort-Verknüpfung durch neuen Text ersetzt wurde. Um das Ergebnis etwas deutlicher zu machen, können wir eine einfache Animation, um die Aufmerksamkeit auf die Änderungsnachricht lenken hinzufügen.

Die Standardeinstellung ASP.NET MVC-Projektvorlage enthält jQuery – eine ausgezeichnete (und sehr beliebte) open Source-JavaScript-Bibliothek, die auch von Microsoft unterstützt wird. jQuery bietet eine Reihe von Funktionen, einschließlich einer nice HTML-DOM-Auswahl und Effekte-Bibliothek.

Verwendung von jQuery fügen wir zuerst einen Skriptverweis auf ihn. Da wir jQuery in unterschiedlichen Stellen in unserer Website verwenden möchten, fügen Skriptverweis in unserer Masterseitendatei Site.master wir damit, dass alle Seiten, die sie verwenden können.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Tipp: Stellen Sie sicher, dass Sie den JavaScript-Intellisense-Hotfix für Visual Studio 2008 SP1 installiert haben, die es umfangreichere Intellisense-Unterstützung für JavaScript-Dateien ermöglicht (einschließlich jQuery). Sie können es herunterladen: http://tinyurl.com/vs2008javascripthotfix*

Unter Verwendung von JQuery häufig geschriebenen Code verwendet eine globale "$ ()" JavaScript-Methode, die eine oder mehrere HTML-Elemente, die mithilfe einer CSS-Auswahl abruft. Beispielsweise <em>$("#rsvpmsg")</em> wählt alle HTML-Element mit der Id Rsvpmsg, während <em>$(".something")</em> wählen alle Elemente mit CSS "etwas" Klassennamen. Sie können auch noch umfassendere Abfragen wie alle aktivierten Optionsfelder "return" schreiben mithilfe einer Auswahl Abfrage z. B.: <em>$("Eingabe [@type= Radio] [@checked]")</em>.

Nachdem Sie die Elemente ausgewählt haben, können Sie Methoden aufrufen, damit Maßnahmen, wie sie verbergen: *$("#rsvpmsg").hide();*

Für unser Szenario Antwort definieren wir eine einfache JavaScript-Funktion mit dem Namen "AnimateRSVPMessage", die die "Rsvpmsg" auswählt &lt;Div&gt; und die Größe seines Textinhalts animiert. Die folgenden Code startet das kleine Text ein, und Ursachen es innerhalb eines Zeitraums 400 Millisekunden erhöhen:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Wir können dann über das Netzwerk von diesem JavaScript-Funktion, die aufgerufen wird, nachdem unsere AJAX-Aufruf erfolgreich abgeschlossen wurde, übergeben Sie seinen Namen an unsere Ajax.ActionLink()-Hilfsmethode (über die AjaxOptions "OnSuccess" Ereigniseigenschaft):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Jetzt bei und der Link "Antwort für dieses Ereignis" geklickt wird, und unsere AJAX-Aufruf erfolgreich abgeschlossen, die Inhalt gesendete Nachricht wird Back wird animieren anwachsen:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Zusätzlich zur Bereitstellung eines Ereignisses "OnSuccess", stellt der AjaxOptions-Objekt OnBegin OnFailure und OnComplete-Ereignisse, die Sie (zusammen mit einer Vielzahl von anderen Eigenschaften und nützliche Optionen) verarbeiten kann.

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Cleanup - Umgestaltung out RSVP Teilansicht

Die Vorlage unsere Details anzeigen wird gestartet, abzurufenden etwas long, welche Überstunden dessen etwas schwieriger zu verstehen, erkennen lässt. Um die Lesbarkeit des Codes zu verbessern, wir fertig stellen Sie durch das Erstellen einer Teilansicht – RSVPStatus.ascx –, die gesamte Antwort Ansichtscode für unsere Seite "Details" kapseln.

Wir können dazu mit der rechten Maustaste auf den Ordner \Views\Dinners und drücken Sie dann die Add -&gt;Menübefehl anzeigen. Wir müssen es ein Dinner-Objekt als die stark typisierte ViewModel dauern. Wir können dann den Inhalt der Antwort aus unserer Ansicht "Details.aspx" haben hinein kopieren.

Wenn Sie es fertig auch erstellen wir eine andere partielle Ansicht – EditAndDeleteLinks.ascx - auf, die unsere Code bearbeiten und löschen Link anzeigen kapselt. Wir müssen es nehmen Sie ein Dinner-Objekt als seine ViewModel stark typisiert, und die Logik bearbeiten und Löschen von unserer Ansicht "Details.aspx" haben hinein, kopieren und einfügen.

Unsere Details-Vorlage kann anzeigen, und klicken Sie dann einfach einschließen, zwei Html.RenderPartial() Methodenaufrufe unten:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Dadurch wird den Code übersichtlicher zu lesen und zu verwalten.

### <a name="next-step"></a>Nächster Schritt

Jetzt betrachten wie wir noch weiter verwenden von AJAX und interaktive Unterstützung unserer Anwendung hinzufügen können.

> [!div class="step-by-step"]
> [Zurück](secure-applications-using-authentication-and-authorization.md)
> [Weiter](use-ajax-to-implement-mapping-scenarios.md)
