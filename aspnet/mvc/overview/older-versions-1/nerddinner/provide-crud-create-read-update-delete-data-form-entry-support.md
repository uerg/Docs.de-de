---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: "Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag Support | Microsoft Docs"
author: microsoft
description: "Schritt 5 zeigt, wie durch Aktivieren der Unterstützung für das Bearbeiten, erstellen und Löschen von Abendessen mit als auch die auszuführenden unsere DinnersController-Klasse."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 5a314a1761527d8a2273166a743e3deac012a557
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag-Unterstützung
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 5 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 5 zeigt, wie durch Aktivieren der Unterstützung für das Bearbeiten, erstellen und Löschen von Abendessen mit als auch die auszuführenden unsere DinnersController-Klasse.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner, Schritt 5: Erstellen Sie, aktualisieren Sie und löschen Sie der Formularszenarien

Wir haben eingeführt, Controller und Ansichten und deren Verwendung zum Implementieren einer Auflistung/Detail-Erfahrung für Abendessen am Standort behandelt. Im nächsten Schritt werden unsere DinnersController Klasse weiter und Unterstützung zum Bearbeiten, erstellen und Löschen von Abendessen mit als auch aktivieren.

### <a name="urls-handled-by-dinnerscontroller"></a>URLs, die vom DinnersController verarbeitet

Wir zuvor Aktionsmethoden, die Unterstützung für zwei URLs implementiert DinnersController hinzugefügt: */Dinners* und */Dinners/Informationen / [Id]*.

| **URL** | **VERB** | **Zweck** |
| --- | --- | --- |
| */Dinners/* | GET | Eine HTML-Liste der bevorstehenden Abendessen angezeigt. |
| */Dinners/Informationen / [Id]* | GET | Anzeigen von Details zu einem bestimmten Dinner. |

Fügen wir jetzt Aktionsmethoden zur Implementierung von drei zusätzliche URLs: */Dinners/bearbeiten / [Id], / Abendessen/Create-,*und*/Dinners/löschen / [Id]*. Diese URLs werden Unterstützung für die Bearbeitung vorhandener Abendessen neue Abendessen erstellen und Löschen von Abendessen aktiviert.

Wir unterstützen sowohl HTTP GET und HTTP POST-Verb Interaktionen mit diesen neuen URLs. HTTP-GET-Anforderungen an diese URLs werden die anfängliche HTML-Ansicht der Daten (ein Formular Dinner Daten im Fall von "Bearbeiten", ein leeres Formular im Fall von "erstellen" und eine Delete-Bestätigungsbildschirm im Fall von "Delete") angezeigt. HTTP-POST-Anforderungen an diese URLs werden Speichern/aktualisieren/löschen die Dinner-Daten in unserem DinnerRepository (und von dort auf die Datenbank).

| **URL** | **VERB** | **Zweck** |
| --- | --- | --- |
| */Dinners/bearbeiten / [Id]* | GET | Anzeigen eines bearbeitbaren HTML-Formulars mit Dinner Daten aufgefüllt. |
| BEREITSTELLEN | Speichern Sie die Formular-Änderungen für einen bestimmten Dinner mit der Datenbank. |
| */ Abendessen/erstellen* | GET | Zeigen Sie ein leeres HTML-Formular, das Benutzern ermöglicht, neue Abendessen definieren. |
| BEREITSTELLEN | Erstellen Sie eine neue Dinner, und speichern Sie sie in der Datenbank. |
| */Dinners/löschen / [Id]* | GET | Anzeige löschen Bestätigungsbildschirm angezeigt. |
| BEREITSTELLEN | Löscht die angegebene Dinner aus der Datenbank an. |

### <a name="edit-support"></a>Bearbeiten-Unterstützung

Fangen Sie durch die Implementierung des Szenarios "Bearbeiten".

#### <a name="the-http-get-edit-action-method"></a>Der HTTP-GET bearbeiten Action-Methode

Wir beginnen mit "GET"-Verhalten unsere Aktionsmethode bearbeiten HTTP zu implementieren. Diese Methode wird aufgerufen, wenn die */Dinners/bearbeiten / [Id]* URL angefordert wird. Die Implementierung sieht etwa aus:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Der obige Code verwendet die DinnerRepository beim Abrufen eines Objekts Dinner. Klicken Sie dann eine Vorlage anzeigen, die unter Verwendung des Objekts Dinner gerendert. Da wir einen Vorlagennamen zur Erstellung explizit übergeben noch nicht die *View()* Hilfsmethode, es wird den Standardpfad konventionsbasierten verwenden, zum Auflösen der Vorlage anzeigen: /Views/Dinners/Edit.aspx.

Jetzt erstellen wir diese Vorlage anzeigen. Wir werden dies durchführen, indem Sie mit der rechten Maustaste innerhalb der Methode bearbeiten, und wählen den Kontextmenübefehl "Ansicht hinzufügen":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Innerhalb des Dialogfelds "Ansicht hinzufügen" fügen wir anzugeben, dass wir ein Objekt Dinner unsere Vorlage anzeigen, wie das Modell übergibt sind, und wählen die automatische Gerüst eine Vorlage für "Bearbeiten":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Wenn wir die Schaltfläche "Hinzufügen" klicken, werden Visual Studio eine neue "Edit.aspx" Vorlage-Datei für uns im Verzeichnis "\Views\Dinners" hinzufügen. Es werden auch neue Ansicht "Edit.aspx" der Vorlage innerhalb der Code-Editor – aufgefüllt, die mit einer anfänglichen "Bearbeiten" Gerüst Implementierung wie unten öffnen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Wir nehmen einige Änderungen an Standardeinstellung "Bearbeiten" Gerüst generiert, und aktualisieren Sie die Vorlage bearbeiten anzeigen, um den folgenden Inhalt haben (die nur einige Eigenschaften, die wir verfügbar machen möchten, nicht entfernt werden):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Ausführen der Anwendung und die Anforderung der *"/ Abendessen/bearbeiten/1"* URL sehen wir die folgende Seite:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Das HTML-Markup generiert, die für unsere Ansicht sieht wie unten. Wird Sie standard-HTML mit einer &lt;Formular&gt; Element, das eine HTTP POST an führt die */Dinners/Edit/1* URL beim "Speichern" &lt;Eingabetyp = "submit" /&gt; gedrückt wird. Eine HTML &lt;input Type = "Text" /&gt; Element wurde Ausgabe für jede Eigenschaft bearbeitet werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() und Html.TextBox() Html-Hilfsmethoden

Unsere "Edit.aspx" Ansichtenvorlage verwendet mehrere "Html" Hilfsmethoden: Html.ValidationSummary(), Html.BeginForm() Html.TextBox() und Html.ValidationMessage(). Zusätzlich zum Generieren von HTML-Markup für uns diese Hilfsmethoden bieten integrierte Fehlerbehandlung und die Validierung unterstützt.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm()-Hilfsmethode

Die Hilfsmethode Html.BeginForm() ist, was die HTML-Ausgabe &lt;Formular&gt; Element in unserem Markup. In unserem Edit.aspx Ansichtenvorlage werden Sie feststellen, dass wir eine C#-Anweisung "using" bei Verwendung dieser Methode angewendet wird. Die geöffnete geschweifte Klammer kennzeichnet den Anfang der &lt;Formular&gt; Inhalt und die schließende geschweifte Klammer ist was bedeutet, dass das Ende der &lt;/form&gt; Element:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Auch wenn Sie feststellen, dass die Anweisung "using" Ansatz für ein Szenario wie folgt unnatürlichen, können Sie eine Kombination von Html.BeginForm() und Html.EndForm() (die die gleiche Aufgabe ist):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Html.BeginForm() ohne Parameter aufrufen, führt er ein Formularelement ausgegeben, das die HTTP-POST an die URL der aktuellen Anforderung durchführt. Also warum unser Bearbeitungsansicht generiert eine  *&lt;bilden Aktion = "/ Abendessen/bearbeiten/1"-Methode = "post"&gt;*  Element. Wir konnten Alternativ haben expliziten Parameter an übergeben Html.BeginForm() würden wir zu einer anderen URL bereitstellen.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox()-Hilfsmethode

Unsere Edit.aspx-Sicht verwendet die Hilfsmethode Html.TextBox() zur Ausgabe &lt;input Type = "Text" /&gt; Elemente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Die oben genannten Html.TextBox() Methode nimmt einen Parameter – der verwendet wird, um beide Attribute den Id-Namen des geben die &lt;Eingabetyp = "Text" /&gt; Element Ausgabe als auch die Modelleigenschaft zum Auffüllen des Textbox-Wert aus. Beispielsweise das Dinner wir übergebene Objekt in der Bearbeitungsansicht hatte einen Eigenschaftswert "Title" von ".NET Futures", und daher unsere Html.TextBox("Title")-Methodenaufruf Ausgabe:  *&lt;Eingabe-Id = "Title" Name = "Title" Type = "Text" Value = ".NET Futures" /&gt;* .

Alternativ können wir verwenden den ersten Html.TextBox() Parameter geben die Id/Name des Elements, und klicken Sie dann den Wert für die Verwendung als zweiten Parameter explizit übergeben:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Oft möchten wir führen Sie die benutzerdefinierte Formatierung auf den Wert, der die Ausgabe ist. Statische Methode erstellt in .NET String.Format() eignet sich für diese Szenarien. Unsere Edit.aspx Ansichtenvorlage ist hierbei Formatieren des Werts EventDate (also der Typ "DateTime"), damit er nicht Sekunden für die Zeit angezeigt:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Dritten Parameter an Html.TextBox() kann optional verwendet werden, um zusätzliche HTML-Attribute auszugeben. Der folgende Codeausschnitt veranschaulicht, wie eine zusätzliche Größe rendern = "30" Attribut und einer Klasse = "Mycssclass"-Attribut auf die &lt;Eingabetyp = "Text" /&gt; Element. Beachten Sie, wie wir den Namen der Klasse Attribut mit Escapezeichen werden eine "@" character because "Klasse" ist ein reserviertes Schlüsselwort in c#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementieren der HTTP-POST bearbeiten Action-Methode

Wir haben jetzt die HTTP-GET-Version von unserer bearbeiten Aktionsmethode implementiert. Wenn ein Benutzer fordert die */Dinners/Edit/1* Erhalt eine HTML-Seite wie die folgende URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Drücken die Schaltfläche "Speichern" bewirkt, dass ein Formular-POST-Methode für die */Dinners/Edit/1* URL, und übermittelt die HTML &lt;Eingabe&gt; Werte mithilfe der HTTP-POST-Verbs bilden. Nehmen wir jetzt Verhalten implementieren, die HTTP POST unsere Aktionsmethode bearbeiten – dem Speichern der Dinner behandelt.

Wir beginnen, indem Sie eine überladene Methode für "Bearbeiten" Aktion an unsere DinnersController, die weist ein Attribut "AcceptVerbs" auf, der angibt, dass sie HTTP POST-Szenarien behandelt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Wenn das Attribut [AcceptVerbs] für überladene Aktionsmethoden angewendet wird, verarbeitet ASP.NET-MVC automatisch verteilen Anforderungen an die richtige Aktionsmethode abhängig von der eingehenden HTTP-Verb. HTTP POST-Anforderungen an */Dinners/bearbeiten / [Id]* URLs werden die oben dargestellte bearbeiten-Methode, während alle anderen HTTP-Verb Anforderungen zur */Dinners/bearbeiten / [Id]*URLs geht auf die erste bearbeiten-Methode implementiert (die wurde kein [AcceptVerbs]-Attribut).

| **Seite Thema: Warum über HTTP-Verben unterscheiden?** |
| --- |
| Sie Fragen sich vielleicht – warum wir mit einer einzelnen URL und sein Verhalten über das HTTP-Verb unterschieden werden? Warum haben nur zwei separate URLs zu laden und speichern die Änderungen behandelt? Zum Beispiel: /Dinners/bearbeiten / [Id] zum Anzeigen der Anfangszustand des Formulars und /Dinners/Save / [Id], behandeln die Formular-Post zu speichern? Der Nachteil mit publishing zwei separate URLs besteht darin, dass in Fällen, in denen wir /Dinners/Save/2 veröffentlichen, und klicken Sie dann um die HTML-Formular aufgrund eines Fehlers Eingabe anzuzeigen, der Endbenutzer beendet wird, dass Sie die Abendessen/Save/2-URL in die Adressleiste des Browsers (da ausgeführt wurde die URL des Formulars an gesendet). Wenn der Endbenutzer dieser redisplayed Seite können Sie ihren Browser-Favoriten-Liste Lesezeichen oder die URL kopieren/fügt und per an einen "Friend e-Mail", erhalten sie eine URL, die in der Zukunft nicht funktioniert (da diese URL Post Werten abhängt) speichern. Verfügbar machen, eine einzelne URL (z. B.: /Dinners/Edit/[id]) und Unterscheidung der Verarbeitung von HTTP-Verb, sie können ruhig Endbenutzern das Lesezeichen Bearbeitungsseite und/oder die URL an andere Benutzer senden. |

#### <a name="retrieving-form-post-values"></a>Abrufen von Werten der Formular-Post

Es gibt eine Vielzahl von Methoden, mit denen, die wir zugreifen können, gebucht Formularparameter in unserer HTTP POST-Methode "Bearbeiten" aus. Ein einfacher Ansatz ist, verwenden Sie die Anforderung-Eigenschaft nur auf die Basisklasse für Controller, Zugriff auf die formularauflistung und die bereitgestellten Werte direkt abrufen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Der oben beschriebene Ansatz ist jedoch etwas verbose, insbesondere, nachdem es die Fehlerbehandlungslogik hinzufügen.

Ein Ansatz besser für dieses Szenario besteht darin, nutzen die integrierte *UpdateModel()* Hilfsmethode für die Controller-Basisklasse. Es unterstützt das Aktualisieren der Eigenschaften eines Objekts, die wir mit der eingehenden Formularparameter übergeben. Er verwendet Reflektion, um zu bestimmen, die Eigenschaftennamen für das Objekt, und klicken Sie dann automatisch konvertiert und weist Werte auf Grundlage von den Eingabewerten vornimmt, die vom Client gesendet.

Wir konnten die UpdateModel()-Methode verwenden, um unsere HTTP-POST-Aktion Bearbeiten mit diesem Code zu vereinfachen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Wir können jetzt besuchen der */Dinners/Edit/1* URL, und ändern Sie den Titel der unsere Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Wenn wir die Schaltfläche "Speichern" klicken, eine Formular-Post an unsere bearbeiten-Aktion ausgeführt, und die aktualisierten Werte in der Datenbank beibehalten werden. Wir werden dann an die URL der Details für die Dinner umgeleitet (der die neu gespeicherten Werte angezeigt wird):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Bearbeiten der Fehlerbehandlung

Unsere aktuellen HTTP-POST-Implementierung funktioniert die Operation problemlos – außer wenn Fehler vorhanden sind.

Wenn ein Benutzer ein Fehler unterläuft, Bearbeiten eines Formulars ändert, müssen wir sicherstellen, dass das Formular mit informativ Fehlermeldung erneut angezeigt wird, die sie an, um sie diesen Fehler beheben führt. Dies gilt auch für Fälle, in denen ein Endbenutzer falsche Eingabe sendet (z. B.: eine falsch formatierte Datumszeichenfolge), wie es ein Verstoß gegen eine Codeanalyseregel Business ist jedoch sowie Where Fällen das Eingabeformat ungültig ist. Wenn Fehler auftreten, dass die Eingabedaten der Benutzer, die ursprünglich eingegeben haben, damit sie nicht ihre Änderungen manuell Auffüllen des Formulars beibehalten werden soll. Dieser Prozess sollte so oft wie erforderlich wiederholen, bis das Formular erfolgreich abgeschlossen ist.

ASP.NET MVC umfasst einige gute integrierte Funktionen, die Fehlerbehandlung und Formular erneut anzeigen zu erleichtern. Um diese Funktionen sollten in Aktion sehen wir aktualisieren unsere bearbeiten Aktionsmethode mit den folgenden Code:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Der obige Code ähnelt unserer vorherigen Implementierung – mit dem Unterschied, dass jetzt einen Try/Catch-Block Fehlerbehandlung um unsere Arbeit umschlossen wird. Wenn eine Ausnahme auftritt, wird beim Aufrufen von UpdateModel() oder wenn wir versuchen, und speichern die DinnerRepository (die eine Ausnahme ausgelöst wird, wenn das Dinner-Objekt, das wir speichern möchten, die aufgrund einer Verletzung der Regel in unserem Modell ungültig ist), unsere Catch Fehler-Block zur Ausnahmebehandlung ausgeführt werden. Darin werden-Schleife über jede regelverletzungen, die in das Dinner Objekt vorhanden und ein ModelState-Objekt (das wir weiter unten erläutert wird) hinzuzufügen. Wir erneut klicken Sie dann die Ansicht anzuzeigen.

Um diesen zu arbeiten, führen wir die Anwendung erneut sehen, bearbeiten Sie eine Dinner, und ändern Sie ihn um, einen leeren Titel, einen EventDate von "GEFÄLSCHTES", und verwenden eine Telefonnummer Vereinigtes Königreich, mit dem Wert Land USA. Drücken der Schaltfläche "Speichern" unsere Bearbeiten von HTTP-POST-Methode werden die Dinner speichern (da Fehler vorliegen) und wird wieder das Formular anzuzeigen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Die Anwendung verfügt über eine ordentliche fehlererfahrung bei. Die Textelemente mit der ungültigen Eingabe werden rot hervorgehoben, und Überprüfungsfehlermeldungen sind für den Endbenutzer über diese angezeigt. Das Formular wird auch die Eingabedaten, die der Benutzer ursprünglich eingegebenen – beibehalten, damit sie nicht alles auffüllen.

Wie erfolgen Sie Fragen sich vielleicht, dies? Wie wird die Titel, EventDate und ContactPhone Textfelder hervorgehoben selbst Rot und wissen, um die ursprünglich eingegebenen Benutzer Ausgabewerte wird? Und die Anzeige Fehlermeldungen abrufen in der Liste oben? Die gute Nachricht ist, dass dies nicht, indem Magic auftreten-statt es war, da wir einige der integrierten ASP.NET MVC-Funktionen verwendet, die Validierung von Benutzereingaben und Fehler Behandlung Szenarien zu erleichtern.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Grundlegendes zu ModelState und die HTML-Hilfsobjekt Validierungsmethoden

Controllerklassen haben Eigenschaftensammlung "ModelState" bietet eine Möglichkeit, um anzugeben, dass ein Fehler mit Model-Objekts übergeben werden, um einer Sicht ist vorhanden. Fehlereinträge in der Auflistung ModelState identifiziert den Namen der Model-Eigenschaft mit dem Problem (z. B.: "Title", "EventDate" oder "ContactPhone"), und ermöglichen eine Human-freundliche Fehlermeldung angegeben werden (z. B.: "Titel ist ein Pflichtfeld").

Die *UpdateModel()* Hilfsmethode füllt automatisch die ModelState-Auflistung, wenn Fehler beim Versuch, die Eigenschaften für das Modellobjekt Formularwerte zuweisen auftreten. Beispielsweise ist unsere Dinner Objekteigenschaft EventDate vom Typ "DateTime". Wenn die UpdateModel()-Methode kann nicht den Zeichenfolgenwert "GEFÄLSCHTES" im Szenario oben zugewiesen wurde, hinzugefügt UpdateModel()-Methode, dass ein Eintrag der ModelState-Auflistung, der angibt, ein Zuweisungsfehler auftritt, die mit dieser Eigenschaft stattgefunden hat.

Entwickler können auch Code schreiben, um Fehlereinträge in die Auflistung ModelState explizit hinzufügen, wie wir unten in unserem "catch" Fehler Block zur Ausnahmebehandlung, auf diese Weise werden die Einträge, die basierend auf der aktiven Regelverletzungen in der Auflistung ModelState aufgefüllt wird die Dinner-Objekt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>HTML-Hilfsobjekt Integration ModelState

HTML-Hilfsmethoden - wie Html.TextBox() - überprüfen Sie die ModelState-Auflistung beim Rendern der Ausgabe. Wenn ein Fehler für das Element vorhanden ist, werden sie die vom Benutzer eingegebenen Wert und eine CSS-Fehlerklasse gerendert.

In unserer Ansicht "Bearbeiten" sind wir z. B. die Hilfsmethode Html.TextBox() verwenden, um die EventDate unsere Dinner-Objekts zu rendern:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Bei der die Ansicht im Fehlerszenario gerendert wurde, überprüft die Html.TextBox()-Methode die ModelState-Auflistung, um festzustellen, ob es Fehler im Zusammenhang mit der Eigenschaft "EventDate" unsere Dinner-Objekts wurden. Wenn ermittelt, ob ein Fehler aufgetreten, die übermittelten Benutzereingabe ("GEFÄLSCHTES") als Wert gerendert und eine CSS-Fehlerklasse hinzugefügt der &lt;Eingabetyp = "Textbox" /&gt; Markup generiert:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Sie können die Darstellung der Css-Fehlerklasse suchen beliebig anpassen. CSS-Fehler Standardklasse – "Eingabe-Überprüfung-Fehler" – wird definiert, der *\content\site.css* Stylesheet und sieht wie unten:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Diese CSS-Regel ist Ursache unsere ungültige Eingabeelemente wie unten hervorgehoben werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage()-Hilfsmethode

Die Hilfsmethode Html.ValidationMessage() kann verwendet werden, die Eigenschaft eines bestimmten Modells zugeordnete ModelState-Fehlermeldung ausgegeben:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Der obige Code gibt:  *&lt;span-Klasse = "Feld-Validierungsfehler"&gt; der Wert "GEFÄLSCHTES" ist ungültig &lt; /span&gt;*

Die Hilfsmethode Html.ValidationMessage() unterstützt auch einen zweiten Parameter, mit dem Entwickler die Fehlermeldung für den Text zu überschreiben, die angezeigt wird:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Der obige Code gibt:  *&lt;span-Klasse = "Feld-Validierungsfehler"&gt;\*&lt;/span&gt;*anstelle der Fehler Standardtext, wenn ein Fehler. für vorliegt die EventDate-Eigenschaft.

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary()-Hilfsmethode

Html.ValidationSummary()-Hilfsmethode kann verwendet werden, um eine zusammenfassende Fehlermeldung beiliegen rendern eine &lt;Ul&gt;&lt;li /&gt;&lt;/UL&gt; Liste mit allen detaillierte Fehlerinformationen von statusmeldungen in der ModelState-Auflistung:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Die Hilfsmethode Html.ValidationSummary() akzeptiert einen optionalen Zeichenfolgenparameter – definiert eine zusammenfassende Fehlermeldung über der Liste der detaillierte Fehler anzeigen:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

CSS können optional außer Kraft setzen, wie die Fehlerliste aussieht.

#### <a name="using-a-addruleviolations-helper-method"></a>Verwenden eine AddRuleViolations-Hilfsmethode

Unsere anfänglichen HTTP-POST bearbeiten-Implementierung verwendet eine foreach-Anweisung innerhalb der Catch-Block, um-Schleife über des Dinner Objekts Regelverstöße und den Controller ModelState-Auflistung hinzugefügt werden:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Stellen wir diesen Code ein wenig Cleaner durch Hinzufügen einer "ControllerHelpers" zum Projekt NerdDinner Klasse, und implementieren eine "AddRuleViolations" Erweiterungsmethode darin, die eine Hilfsmethode, die ASP.NET MVC ModelStateDictionary-Klasse hinzugefügt. Diese Erweiterungsmethode kapseln kann die Logik zum Auffüllen der ModelStateDictionary mit einer Liste von Fehlern RuleViolation erforderlich:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Wir können unsere HTTP-POST bearbeiten Aktionsmethode zur Verwendung dieser Erweiterungsmethode beim Auffüllen der ModelState-Auflistung mit unserem Regelverstöße Dinner aktualisieren.

#### <a name="complete-edit-action-method-implementations"></a>Methodenimplementierungen für Bearbeiten Aktion abgeschlossen

Der folgende Code implementiert alle der Controllerlogik für unser Szenario bearbeiten:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Ein Vorteil von unserer Implementierung bearbeiten ist, dass unsere Controllerklasse weder unsere Vorlage anzeigen noch etwas wissen, über die spezifische Validierung oder Geschäftsregeln, die vom unserem Modell Dinner erzwungen wird. Wir zusätzliche Regeln zu unserem Modell können in Zukunft hinzufügen und müssen keine codeänderungen an unserer Controller oder einer Sicht in der Reihenfolge dafür unterstützt werden müssen vornehmen. Dadurch wird die erforderliche Flexibilität, um unsere anwendungsanforderungen zukünftig mit einem Minimum von Änderungen am Code einfach zu entwickeln.

### <a name="create-support"></a>Erstellen Sie Support

Wir haben abgeschlossen, die "Bearbeiten" Verhalten unsere DinnersController-Klasse zu implementieren. Wir nun die Unterstützung für "Erstellen" auf – implementieren, die neue Abendessen Hinzufügen von Benutzern aktiviert wird.

#### <a name="the-http-get-create-action-method"></a>Der HTTP-GET erstellen Aktionsmethode

Wir beginnen, indem Sie "GET"-Verhalten der HTTP-implementieren unsere Aktionsmethode zu erstellen. Diese Methode wird aufgerufen, wenn ein Benutzer besucht die */Abendessen/Create* URL. Die Implementierung sieht wie folgt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Der obige Code erstellt ein neues Objekt für die Dinner und weist die EventDate-Eigenschaft einer Woche in der Zukunft sein. Klicken Sie dann eine Ansicht, die auf dem neuen Dinner Objekt basiert gerendert. Da wir ein Name, der explizit übergeben noch nicht die *View()* Hilfsmethode, es wird den Standardpfad konventionsbasierten verwenden, zum Auflösen der Vorlage anzeigen: /Views/Dinners/Create.aspx.

Jetzt erstellen wir diese Vorlage anzeigen. Wir können dies durchführen, indem Sie mit der rechten Maustaste innerhalb der erstellen-Aktionsmethode, und wählen den Kontextmenübefehl "Ansicht hinzufügen". Innerhalb des Dialogfelds "Ansicht hinzufügen" fügen wir anzugeben, dass wir ein Dinner-Objekt an der Vorlage anzeigen übergeben, und wählen die automatische Gerüst eine Vorlage "Erstellen":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, wird die Visual Studio speichern eine neue Ansicht für Gerüst basierende "Create.aspx" im Verzeichnis "\Views\Dinners", und in der IDE zu öffnen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Wir nehmen einige Änderungen an der Standardeinstellung "erstellen" Gerüst Datei, die generiert wurde für uns und, wie unten dargestellt zu ändern:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Und nun wir Ausführung von unserem Anwendung und den Zugriff der *"/ Abendessen/Create"* URL innerhalb des Browsers Benutzeroberfläche wie im folgenden wird von unseren erstellen Aktion Implementierung gerendert:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementieren die HTTP-POST Aktionsmethode erstellen

Wir haben die HTTP-GET-Version von unserer Create Action-Methode implementiert. Klickt ein Benutzer die Schaltfläche "Speichern" führt Sie eine Formular-Post an die */Abendessen/erstellen* URL, und übermittelt den HTML-Code &lt;Eingabe&gt; Werte mithilfe der HTTP-POST-Verbs bilden.

Implementieren Sie die HTTP POST-Verhalten von wir jetzt unsere Aktionsmethode zu erstellen. Wir beginnen, indem Sie eine überladene Methode der "Create"-Aktion an unsere DinnersController, die weist ein Attribut "AcceptVerbs" auf, der angibt, dass sie HTTP POST-Szenarien behandelt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Es gibt eine Vielzahl von Möglichkeiten, die wir in unserer "Erstellen" aktiviert HTTP-POST-Methode die Parameter bereitgestellten Formulars zugreifen können.

Ein Ansatz besteht darin, ein neues Dinner-Objekt erstellen und verwenden dann die *UpdateModel()* Hilfsmethode (z. B. wir mit der Aktion bearbeiten haben), das sie mit der übermittelte Formularwerte aufgefüllt. Wir können dann unsere DinnerRepository hinzu, in der Datenbank gespeichert, und leiten Sie den Benutzer an unsere Aktion Details zum Anzeigen der neu erstellten Dinner mit den folgenden Code:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternativ können wir einen Ansatz verwenden, haben wir unsere Create()--Aktionsmethode, die ein Dinner-Objekt als Parameter der Methode ergreifen. ASP.NET MVC dann automatisch instanziieren ein neues Objekt für die Dinner für uns, dessen Eigenschaften mit Formulareingaben für das zu füllen, und an unsere Aktionsmethode übergeben:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Unsere Aktionsmethode oben überprüft, dass das Dinner Objekt durch Überprüfen der ModelState.IsValid-Eigenschaft erfolgreich mit den Formular-Post-Werten aufgefüllt wurde. Dieser Befehl gibt "false" Falls stehen Eingabe Konvertierungsprobleme zurück (z. B.: eine Zeichenfolge mit "GEFÄLSCHTES" für die Eigenschaft EventDate), und unsere Aktionsmethode erneut an das Formular, wenn Probleme auftreten.

Wenn die Eingabewerte gültig sind, versucht die Aktionsmethode hinzufügen und die neue Dinner in der DinnerRepository gespeichert. Es dient als Wrapper für diese Arbeit innerhalb eines Try/Catch-Blocks und erneut an das Formular, treten Business Regelverstöße (die die dinnerRepository.Save()-Methode zum Auslösen einer Ausnahme führen würde).

Um diese Fehler bei der Verarbeitung von Verhaltensweisen in Aktion anzuzeigen, können wir Anfordern der */Abendessen/Create* URL und füllen Sie Details zu einem neuen Dinner. Falsche Eingabe oder Werte führt dazu, dass das Erstellungsformular, mit der hervorgehobenen wie Fehlern erneut angezeigt werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Beachten Sie, wie unser Formular erstellen genauen dieselben Überprüfung und Business Regeln wie unser Bearbeitungsformular berücksichtigt wird. Dies ist da unsere Überprüfung und Geschäftsregeln, die im Modell definiert wurden, und wurden nicht innerhalb der Benutzeroberfläche oder den Controller der Anwendung eingebettet. Dies bedeutet, dass wir können später ändern/weiterentwickelt unsere Überprüfung oder Geschäftsregeln in einer einzelnen platzieren, und bitten, dass in der gesamten Anwendung gelten. Es wird keine ändert keinen Code innerhalb einer unserer bearbeiten oder erstellen Sie Aktionsmethoden um keine neue Regeln oder vorhandene Datasets ändern automatisch berücksichtigt.

Wenn wir die Eingabewerte beheben, und klicken Sie auf die Schaltfläche "Speichern" erneut, unsere zusätzlich zu den DinnerRepository wird erfolgreich ausgeführt, und eine neue Dinner wird mit der Datenbank hinzugefügt werden. Es wird dann an umgeleitet werden die */Dinners/Informationen / [Id]* URL, in dem dargestellt wird und Details zu der neu erstellten Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Delete-Unterstützung

Fügen wir "Löschen" unterstützt jetzt unsere DinnersController hinzu.

#### <a name="the-http-get-delete-action-method"></a>Der HTTP-GET-Delete-Aktion-Methode

Wir beginnen, indem Sie die HTTP GET-Verhalten unsere Delete-Aktion-Methode zu implementieren. Diese Methode wird aufgerufen abrufen, wenn ein Benutzer besucht die */Dinners/löschen / [Id]* URL. Im folgenden ist die Implementierung:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Die Aktionsmethode versucht, die Dinner zu löschenden abzurufen. Ggf. die Dinner rendert Grundlage eine Ansicht des Dinner-Objekts ab. Wenn das Objekt ist nicht vorhanden (oder bereits gelöscht wurde wurde) anzeigen Gibt eine Sicht, die die "NotFound" rendert Vorlage, die wir für unsere Aktionsmethode "Details" vorher erstellt haben.

Wir können den "Löschen" Ansicht-Vorlage erstellen, indem Sie mit der rechten Maustaste innerhalb der Delete-Aktionsmethode, und wählen den Kontextmenübefehl "Ansicht hinzufügen". Innerhalb des Dialogfelds "Ansicht hinzufügen" fügen wir anzugeben, dass wir ein Objekt Dinner unsere Vorlage anzeigen, wie das Modell übergibt sind, und wählen eine leere Vorlage zu erstellen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Wenn wir die Schaltfläche "Hinzufügen" klicken, wird Visual Studio eine neue "Delete.aspx" Ansicht Vorlage-Datei für uns in unserem "\Views\Dinners" Verzeichnis hinzufügen. Wir fügen einige HTML und den Code der Vorlage eine Delete-Bestätigungsbildschirm implementiert wie unten:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Der obige Code zeigt den Titel der Dinner gelöscht werden soll, und Ausgaben einer &lt;Formular&gt; Element, das einer POST-Anforderung an der /Dinners/löschen / [Id] URL durchführt, wenn der Endbenutzer auf die Schaltfläche "Löschen" darin klickt.

Wir Ausführung von unserem Anwendung und den Zugriff der *"/ Abendessen/löschen / [Id]"* -URL für eine gültige Dinner Objekt rendert Benutzeroberfläche wie unten:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Seite Thema: Warum führen wir einen Beitrag?** |
| --- |
| Sie Fragen sich vielleicht – Warum erläutern wir, den Aufwand des Erstellens einer &lt;Formular&gt; in unserer Bestätigungsbildschirm löschen? Warum nicht nur einen standardmäßigen Hyperlink zu einer Aktionsmethode eine Verknüpfung mit, die den tatsächlichen Löschvorgang ausführt? Der Grund ist, da Achten Sie darauf, dass Sie zum Schutz vor Webcrawler und Suchmaschinen ermitteln unsere URLs und versehentlich verursacht Daten gelöscht werden, wenn sie die Links entsprechen soll. HTTP-GET-basierten URLs als "sicheren" sind, um den Zugriff/Durchforstung betrachtet, und sie nicht auf HTTP-POST diejenigen folgen soll. Eine gute Regel besteht darin sicherzustellen, immer ablegen destruktiven oder Ändern von Datenvorgängen hinter HTTP-POST-Anforderungen. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementieren der HTTP-POST-Delete-Aktion-Methode

Wir haben jetzt die HTTP-GET-Version des unsere Delete-Aktionsmethode, die implementiert eine Delete-Bestätigungsbildschirm angezeigt. Klickt ein Benutzer die Schaltfläche "Löschen" führen Sie ein Formular-Post an die */Dinners/Dinner / [Id]* URL.

Nehmen wir jetzt Verhalten implementieren, die HTTP-"POST" der Delete-Aktionsmethode, die mit den folgenden Code:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Die HTTP-POST-Version von unserer Delete-Aktion-Methode versucht, das zu löschende Objekt Dinner abzurufen. Wenn es gefunden werden kann (da dieser bereits gelöscht wurde) unsere "NotFound" Vorlage gerendert. Wenn der Dinner gefunden wird, wird dieser aus dem DinnerRepository gelöscht. Klicken Sie dann eine Vorlage für "Deleted" gerendert.

So implementieren Sie die Vorlage "Deleted" wir mit der rechten Maustaste in der Aktionsmethode und wählen Sie im Kontextmenü "Ansicht hinzufügen" Wir unsere Ansicht "Deleted" nennen und haben sie eine leere Vorlage werden (und nicht stark typisierte Model-Objekts). Wir fügen ihm dann einige HTML-Inhalt:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Und nun wir Ausführung von unserem Anwendung und den Zugriff der *"/ Abendessen/löschen / [Id]"* -URL für eine gültige Dinner Objekt wird gerendert, unsere Dinner Bestätigung des Löschens Bildschirm wie unten:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Beim Klicken auf die Schaltfläche "Löschen" führen Sie einen HTTP-POST an die */Dinners/löschen / [Id]* -URL, die der Dinner aus der Datenbank löschen, und unsere "Deleted" Ansichtenvorlage anzuzeigen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Sicherheit in Modellen Bindung

Wir haben zwei Möglichkeiten, verwenden Sie die integrierten Features der modellbindung von ASP.NET MVC erläutert. Das zuerst mithilfe der Methode UpdateModel() um Eigenschaften für ein vorhandenes Modellobjekt zu aktualisieren, und der zweite mithilfe ASP.NETs-Unterstützung für die Übergabe von Modellobjekte in als Aktionsmethodenparameter. Beide Verfahren sind sehr leistungsstark und äußerst nützlich.

Diese Power setzt ebenfalls mit Verantwortung. Es ist wichtig, immer über die Sicherheit paranoid sein, wenn Benutzereingaben akzeptieren, und dies gilt auch beim Binden von Objekten an Formulareingaben. Sie sollten darauf achten, HTML-Codierung immer alle vom Benutzer eingegebenen Werten zum Vermeiden von HTML und JavaScript-Injection-Angriffen, und achten Sie darauf, dass Sie von SQL-Injection-Angriffen (Hinweis: wir mit LINQ to SQL für die Anwendung, die Parameter, um zu verhindern, dass diese automatisch codiert Arten von Angriffen). Sie sollten nie abhängig ist, clientseitige Validierung allein und nutzen immer eine serverseitige Validierung zum Schutz gegen Hacker versucht, ungültige Werte zu senden.

Ein Sicherheitsvorkehrungen-Element, um sicherzustellen, dass Sie sind im Wesentlichen bei Verwendung von der Bindung Features von ASP.NET MVC ist der Bereich der Objekte, die Sie binden. Insbesondere sollen Sie sicherstellen, dass Sie die Auswirkungen auf die Sicherheit der Eigenschaften vertraut sein, wenn Sie die erlauben gebunden werden, und stellen Sie sicher, dass Sie nur diese Eigenschaften ermöglichen, die tatsächlich von einem Endbenutzer zu aktualisierenden aktualisiert werden soll.

Standardmäßig versucht die UpdateModel()-Methode, die alle Eigenschaften auf das Modellobjekt zu aktualisieren, die eingehenden Parameterwerte in Form entsprechen. Ebenso können Objekte, die standardmäßig auch als Aktionsmethodenparameter übergeben all ihre Eigenschaften festlegen über Formularparameter verfügen.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bindung auf Grundlage messungs-Sperren

Sie können Bindungsrichtlinie regelmäßig messungs-sperren, durch Bereitstellen einer expliziten "Einschlussliste" von Eigenschaften, die aktualisiert werden können. Dies kann durch Übergeben eines zusätzlichen Zeichenfolge, die Array-Parameters an die Methode UpdateModel() wie unten erfolgen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objekte, die auch als Aktionsmethodenparameter übergeben unterstützen ein [Bind]-Attribut, die es ermöglicht, eine "include Liste" der zulässigen Eigenschaften wie unten angegeben werden:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bindung auf Basis eines Typs Sperren

Sie können auch die Bindungsregeln regelmäßig pro Typ sperren. Dadurch können Sie die Bindungsregeln einmal angeben und dann bitten, dass in allen Szenarien (z. B. in UpdateModel und Aktion Methode Parameter Szenarien) in allen Controllern und Aktionsmethoden anwenden.

Sie können die Bindungsregeln pro Typ anpassen, durch ein [Bind]-Attribut auf einen Typ hinzufügen, oder registrieren es in der Datei "Global.asax" der Anwendung (hilfreich bei Szenarien, in dem Sie den Typ besitzen). Anschließend können Sie der Bind-Attribut einschließen und ausschließen Eigenschaften zu steuern, welche Eigenschaften sind für bestimmte Klasse oder Schnittstelle gebunden.

Wir verwenden diese Technik für die Dinner-Klasse in unserer NerdDinner-Anwendung und fügen ein [Bind]-Attribut hinzu, die die Liste der bindbare Eigenschaften auf den folgenden beschränkt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Beachten Sie, dass wir die Einladung-Auflistung, über die Bindung manipuliert werden nicht zugelassen sind, noch können wir die DinnerID oder HostedBy Eigenschaften über die Bindung festgelegt werden. Aus Sicherheitsgründen müssen wir stattdessen nur diese bestimmten Eigenschaften, die mit expliziten Code in unserer Aktionsmethoden bearbeiten.

### <a name="crud-wrap-up"></a>CRUD-Zusammenfassung

ASP.NET MVC umfasst eine Reihe von integrierten Funktionen, mit deren Hilfe bei der Implementierung von Formular Posten von Beiträgen Szenarien. Eine Vielzahl von diesen Funktionen verwendet, um Unterstützung für CRUD-UI über unsere DinnerRepository bieten.

Wir sind einen Modell mit Fokus Ansatz verwenden, um die Anwendung implementieren. Dies bedeutet, dass unsere Überprüfung und Business-Regel, dass unsere Modellebene – und nicht innerhalb von unserer Controllern oder Sichten definiert wird. Unsere Controllerklasse weder unsere Ansichtsvorlagen wissen nichts über den jeweiligen Geschäftsregeln, die vom unsere Dinner Modellklasse erzwungen wird.

Damit bleibt unsere Anwendungsarchitektur bereinigen und zum Testen vereinfachen. Wir können unsere Modellebene zusätzliche Geschäftsregeln in Zukunft hinzufügen und *müssen keine Änderungen am Code vornehmen* unsere Controller oder die Ansicht in der Reihenfolge dafür unterstützt werden müssen. Dies geht uns sehr viel systemverarbeitungszeit in stetig weiterentwickelt, und ändern die Anwendung in der Zukunft bereitzustellen.

Unsere DinnersController jetzt ermöglicht Dinner Angebote/Details als auch erstellen, bearbeiten und delete-Unterstützung. Der vollständige Code für die Klasse finden Sie unter:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt grundlegende CRUD (Create, Read, Update und Delete) Unterstützung bei der Implementierung in unsere DinnersController-Klasse.

Jetzt betrachten wie wir ViewData und ViewModel-Klassen verwenden können, auch umfangreichere UI auf unseren Formularen zu aktivieren.

>[!div class="step-by-step"]
[Zurück](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[Weiter](use-viewdata-and-implement-viewmodel-classes.md)
