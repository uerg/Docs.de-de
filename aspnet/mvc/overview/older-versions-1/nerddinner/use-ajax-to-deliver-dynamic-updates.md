---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Bereitstellen von dynamischen Updates mithilfe von AJAX | Microsoft-Dokumentation
author: microsoft
description: Schritt 10-implementiert die Unterstützung für den angemeldeten Benutzer RSVP ihr Interesse an Ihre Teilnahme an einem Dinner, die mit einem Ajax-basierten Ansatz, integriert in die Dinner-Details...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 789c9880dc43c5d15ee510d9856a4c4378019b71
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372329"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Bereitstellen von dynamischen Updates mithilfe von AJAX
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 10 ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 10-implementiert die Unterstützung für den angemeldeten Benutzer RSVP ihr Interesse an Ihre Teilnahme an einem Dinner, die mit einem Ajax-basierten Ansatz, der innerhalb der Dinner-Detailseite integriert.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner Schritt 10: Aktivieren von Bestätigungen AJAX akzeptiert

Nun implementieren wir Unterstützung für angemeldete Benutzer auf ihr Interesse an Ihre Teilnahme an einem Dinner zu antworten. Dadurch wird es mit einem AJAX-basierten Ansatz, der innerhalb der Dinner-Detailseite integriert.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Gibt an, ob der Benutzer um Antwort gebeten wird

Benutzer besuchen können die *"/ dinners" / Details / [Id*] URL, um Details zu einem bestimmten Dinner anzuzeigen:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Die Details() Action-Methode wird implementiert, wie folgt:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Im ersten Schritt RSVP-Unterstützung implementieren werden das Dinner-Objekt (in der partiellen Dinner.cs-Klasse, die wir zuvor erstellt) eine Hilfsmethode "IsUserRegistered(username)" hinzugefügt. "True" oder "false", je nachdem, ob der Benutzer derzeit für das Dinner um Antwort gebeten wird, gibt diese Hilfsmethode zurück:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Wir können klicken Sie dann den folgenden Code hinzufügen, um unsere Details.aspx-View-Vorlage zum Anzeigen von einer entsprechenden Meldung angezeigt, der angibt, ob der Benutzer registriert ist oder nicht für das Ereignis:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Und nun, wenn ein Benutzer eine Dinner besucht, für die sie registriert sind, sie erhalten diese Nachricht:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Und wenn sie eine Dinner sie nicht registriert besuchen, sind für die sie sehen die folgende Meldung angezeigt:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementieren die Register-Aktion-Methode

Fügen Sie die erforderlichen Funktionen zur Benutzern ermöglichen, die sich auf der Detailseite zum RSVP zu einem Dinner jetzt aus.

Um dies zu implementieren, werden wir eine neue "RSVPController"-Klasse erstellen, indem Sie mit der rechten Maustaste auf das Verzeichnis \Controllers und Auswählen der hinzufügen -&gt;Kontextmenübefehl von "Controller".

Wir implementieren eine Aktionsmethode "Registrieren", in der neuen RSVPController-Klasse, die eine Id zu einem Dinner als Argument akzeptiert, wird das entsprechende Dinner-Objekt, überprüft, wenn der angemeldete Benutzer derzeit in der Liste der Benutzer ist, die für sie registriert haben und ein RSVP-Objekt wird nicht für sie hinzugefügt:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Beachten Sie, oben, wie wir eine einfache Zeichenfolge als Ausgabe der Aktionsmethode zurückgegeben werden. Wir konnten diese Nachricht in eine ansichtsvorlage – eingebettet haben, aber da es so klein ist verwenden wir die Content()-Hilfsmethode für die Basisklasse für Controller und zurück, der eine zeichenfolgenmeldung wie oben.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Aufruf der RSVPForEvent-Aktionsmethode, die mithilfe von AJAX

Wir verwenden AJAX, die Register-Aktion-Methode aus unserer Ansicht aufrufen. Implementieren Dies ist recht einfach. Zunächst fügen wir zwei Skriptverweise-Bibliothek:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

Die erste Bibliothek verweist auf die ASP.NET AJAX Clientskript-Kernbibliothek. Diese Datei ist ca. 24 KB Größe (komprimiert) und Core clientseitigen AJAX-Funktionen enthält. Die zweite-Bibliothek enthält Hilfsfunktionen, die Integration in integrierten ASP.NETs AJAX-Hilfsmethoden (, verwenden wir in Kürze).

Wir können, und klicken Sie dann Update Vorlagencode anzeigen, die wir zuvor hinzugefügt haben, sodass statt beim Ausgeben einer Meldung "Sie sind nicht für dieses Ereignis registriert", wir stattdessen einem Link, der gerendert werden, wenn mithilfe von Push übertragen soll einen AJAX-Aufruf ausführt, der unsere RSVPForEvent Aktionsmethode auf unsere RSVP Controller aufruft und den Benutzer RSVPs:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Die oben verwendete Ajax.ActionLink()-Hilfsmethode wird erstellt in ASP.NET MVC und ähnelt die Hilfsmethode Html.ActionLink(), anstatt eine standard-Navigation einen AJAX-Aufruf an die Aktionsmethode vereinfacht, wenn auf der Link geklickt wird. Über die Aktionsmethode "Registrieren" auf dem Controller "Bestätigung" aufrufen und wir die DinnerID als Parameter "Id" an sie übergibt. Der letzte AjaxOptions-Parameter übergeben werden gibt an, dass wir den Inhalt von der Aktionsmethode zurückgegeben, und aktualisieren den HTML-Code &lt;Div&gt; Element auf der Seite mit der Id ist "Rsvpmsg".

Und jetzt beim Benutzer zu einem Dinner navigiert sie sind nicht für registriert, aber sie erhalten einen Link zur Bestätigung dafür:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Wenn sie den Link "RSVP für dieses Ereignis" klicken sie erstellen einen AJAX-Aufruf an die Register-Aktion-Methode auf dem Controller RSVP und bei Abschluss sehen sie eine aktualisierte Meldung wie die folgende:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Die Netzwerkbandbreite und der Datenverkehr bei der dieser AJAX-Aufruf sehr Schlank ist. Klickt der Benutzer auf den Link "RSVP für dieses Ereignis", kleine HTTP POST-Netzwerk angefordert wird die */Dinners/Register/1* URL, die bei der Übertragung unter aussieht:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Und die Antwort auf unsere Register-Aktion-Methode ist einfach:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Dieser einfache Aufruf ist schnell und sogar über ein langsames Netzwerk funktioniert.

### <a name="adding-a-jquery-animation"></a>Hinzufügen von jQuery Animation

Die AJAX-Funktionalität, die wir implementiert haben, funktioniert gut und schnell. Manchmal kann es so schnell weiter, jedoch passieren, dass ein Benutzer nicht bemerkt, dass die RSVP-Verknüpfung durch neuen Text ersetzt wurde. Um das Ergebnis ein wenig sichtbarer machen können wir eine einfache Animation, um die Aufmerksamkeit auf das Aktualisieren der Nachricht hinzufügen.

Die standardmäßige ASP.NET MVC-Projektvorlage enthält jQuery – eine hervorragende (und sehr beliebtes)-open-Source-JavaScript-Bibliothek, die auch von Microsoft unterstützt wird. jQuery bietet es sich um eine Reihe von Features, darunter eine gute HTML-DOM-Auswahl und Effekte-Bibliothek.

Verwendung von jQuery fügen wir zuerst einen Skriptverweis auf sie. Da wir hierfür jQuery in einer Vielzahl von Orten innerhalb unserer Website verwenden, fügen den Skriptverweis in unserer Masterseitendatei Site.master wir so, dass alle Seiten, die sie verwenden können.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Tipp: Stellen Sie sicher, dass Sie den JavaScript-Intellisense-Hotfix für Visual Studio 2008 SP1 installiert haben, die es umfangreichere Intellisense-Unterstützung für JavaScript-Dateien ermöglicht (einschließlich jQuery). Sie können es aus: http://tinyurl.com/vs2008javascripthotfix*

Geschrieben, häufig mit der JQuery-Code verwendet einen globalen "$ ()" JavaScript-Methode, die eine oder mehrere HTML-Elemente, die mit einem CSS-Selektor abruft. Z. B. <em>$("#rsvpmsg")</em> wählt alle HTML-Element mit der Id der Rsvpmsg, während <em>$(".something")</em> wählen alle Elemente mit CSS "etwas" Klassenname. Sie können auch komplexere Abfragen wie "alle aktivierten Optionsfelds return" schreiben mithilfe einer Abfrage Selektor wie: <em>$("Input [@type= Radio] [@checked]")</em>.

Wenn Sie Elemente ausgewählt haben, können Sie Methoden aufrufen, darauf zu ergreifen, wie sie verbergen: *$("#rsvpmsg").hide();*

In diesem Szenario RSVP definieren wir eine einfache JavaScript-Funktion, die mit dem Namen "AnimateRSVPMessage", die die "Rsvpmsg" auswählt &lt;Div&gt; und die Größe seines Textinhalts animiert. Die folgenden Code wird gestartet, die kleinen Text ein, und bewirkt, dass sie über einen Zeitraum von 400 Millisekunden zu erhöhen:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Wir können dann über das Netzwerk von diesem JavaScript-Funktion, die aufgerufen werden, wenn unsere AJAX-Aufruf erfolgreich abgeschlossen wurde, übergeben Sie den Namen an unsere Ajax.ActionLink()-Hilfsmethode (mithilfe der AjaxOptions "OnSuccess" Ereigniseigenschaft):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Und jetzt den Link "RSVP für dieses Ereignis" geklickt wird, und unsere AJAX-Aufruf wird erfolgreich abgeschlossen, die Inhalt gesendete Nachricht Back wird animieren und anwachsen:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Zusätzlich zur Bereitstellung eines Ereignisses "OnSuccess", stellt der AjaxOptions-Objekt OnBegin OnFailure und OnComplete-Ereignisse, die Sie (zusammen mit verschiedenen anderen Eigenschaften und nützliche Optionen) behandeln können.

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Bereinigung - Umgestaltung, eine Teilansicht mit Antworten

Die Vorlage unserer Details anzeigen beginnt mit der ein wenig lang werden, die im Laufe der Zeit etwas schwieriger zu verstehen erleichtern wird. Um die Lesbarkeit des Codes zu verbessern, lassen Sie uns zum Schluss erstellen eine partielle Ansicht – RSVPStatus.ascx –, die alle RSVP View Code für unsere Seite "Details" zu kapseln.

Wir können dazu mit der rechten Maustaste auf den Ordner "\Views\Dinners" und wählen Sie dann die Add -&gt;Menübefehl anzeigen. Wir müssen es ein Dinner-Objekt als die stark typisierte ViewModel zu nutzen. Wir können dann kopieren und den RSVP-Inhalt aus unserer Ansicht "Details.aspx" darin einfügen.

Sobald wir damit fertig sind, außerdem erstellen wir eine andere partielle Ansicht – EditAndDeleteLinks.ascx - auf, die unseren Code bearbeiten und löschen Link anzeigen kapselt. Darüber hinaus haben Sie ein Dinner-Objekt als die stark typisierte ViewModel nutzen und die Logik für bearbeiten und Löschen von unserer Ansicht "Details.aspx" hinein kopieren und einfügen.

Unsere Details, die Vorlage kann anzeigen, und klicken Sie dann schließen Sie einfach zwei Html.RenderPartial() Methodenaufrufe am unteren Rand:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Dadurch wird der Code besser lesen und zu verwalten.

### <a name="next-step"></a>Nächster Schritt

Jetzt sehen wir uns wie wir mithilfe von AJAX noch einen Schritt weiter und interaktive Unterstützung unserer Anwendung hinzufügen können.

> [!div class="step-by-step"]
> [Zurück](secure-applications-using-authentication-and-authorization.md)
> [Weiter](use-ajax-to-implement-mapping-scenarios.md)
