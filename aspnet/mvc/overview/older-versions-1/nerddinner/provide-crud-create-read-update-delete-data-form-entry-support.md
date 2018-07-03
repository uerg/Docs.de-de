---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag-Unterstützung | Microsoft-Dokumentation
author: microsoft
description: Schritt 5 zeigt, wie unsere DinnersController-Klasse durch Aktivieren der Unterstützung für das Bearbeiten, erstellen und Löschen von Dinner auch dabei werden wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 821684c0753967fc1a693b061d5d539951cd7c23
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388807"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag-Unterstützung
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 5 von einem kostenlosen ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 5 zeigt, wie unsere DinnersController-Klasse durch Aktivieren der Unterstützung für das Bearbeiten, erstellen und Löschen von Dinner auch dabei werden wird.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner, Schritt 5: Erstellen, aktualisieren und Löschen von Formularszenarien

Wir haben eingeführt, Controllern und Ansichten und erläutert, wie Sie diese verwenden, um eine Liste/Details-Oberfläche für die Dinner auf Website implementieren. Im nächsten Schritt werden unsere Klasse "dinnerscontroller" weitergehen und Aktivieren der Unterstützung für das Bearbeiten, erstellen und Löschen von Dinner auch mit.

### <a name="urls-handled-by-dinnerscontroller"></a>URLs, die von "dinnerscontroller" verarbeitet

Zuvor hinzugefügten Aktionsmethoden zu "dinnerscontroller", die Unterstützung für zwei URLs implementiert: *"/ dinners"* und *"/ dinners" / Details / [Id]*.

| **URL** | **VERB** | **Zweck** |
| --- | --- | --- |
| */Dinners/* | GET | Eine HTML-Liste mit anstehenden Dinner angezeigt. |
| *"/ Dinners" / Details / [Id]* | GET | Anzeigen von Details zu einem bestimmten Dinner. |

Fügen wir jetzt Aktionsmethoden zur Implementierung von drei zusätzliche URLs: <em>"/ dinners" / Edit / [Id] "," / Dinners/erstellen,</em>und<em>"/ dinners" / Delete / [Id]</em>. Diese URLs werden Unterstützung für die Bearbeitung vorhandener Dinner, neue Dinner erstellen und Löschen von Dinner aktiviert.

Wir unterstützen HTTP GET- und HTTP POST-Verb-Interaktionen mit diesen neuen URLs. HTTP GET-Anforderungen an diese URLs werden die erste HTML-Ansicht der Daten (ein Formular mit den Dinner-Daten im Fall von "Bearbeiten" aufgefüllt, ein leeres Formular im Fall von "erstellen" und ein Bestätigungsfenster "löschen" im Fall von "Delete") angezeigt. HTTP-POST-Anforderungen an diese URLs werden Speichern/aktualisieren/löschen die Dinner-Daten in unsere "dinnerrepository" (und von dort aus in der Datenbank).

| **URL** | **VERB** | **Zweck** |
| --- | --- | --- |
| *"/ Dinners" / Edit / [Id]* | GET | Zeigen Sie ein bearbeitbaren HTML-Formular mit Dinner-Daten aufgefüllt. |
| POST | Speichern Sie die formularänderungen für ein bestimmtes Essen, auf die Datenbank an. |
| */Dinners/Create* | GET | Zeigen Sie ein leeres HTML-Formular, das Benutzern ermöglicht, neue Dinner zu definieren. |
| POST | Erstellen Sie eine neue Dinner, und speichern Sie sie in der Datenbank. |
| *"/ Dinners" / Delete / [Id]* | GET | Anzeige löschen Bestätigungsbildschirm angezeigt. |
| POST | Löscht das angegebene Dinner aus der Datenbank an. |

### <a name="edit-support"></a>Bearbeiten-Unterstützung

Beginnen Sie durch die Implementierung des Szenarios "Bearbeiten".

#### <a name="the-http-get-edit-action-method"></a>Die HTTP-GET-Edit-Aktion-Methode

Wir beginnen, durch die Implementierung von HTTP "GET"-Verhaltens unsere Edit-Aktionsmethode. Diese Methode wird aufgerufen, wenn die *"/ dinners" / Edit / [Id]* URL angefordert wird. Unsere Implementierung sieht wie aus:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Der obige Code verwendet die "dinnerrepository", um ein Dinner-Objekt abzurufen. Klicken Sie dann eine ansichtsvorlage unter Verwendung des Dinner-Objekts gerendert. Da wir nicht explizit einen Vorlagennamen zur Erstellung erfolgreich abgeschlossen haben die *View()* Hilfsmethode verwendet den konventionsbasierten Standardpfad für die ansichtsvorlage zu beheben: /Views/Dinners/Edit.aspx.

Jetzt erstellen wir diese Vorlage anzeigen. Wir tun dies, indem Sie mit der rechten Maustaste innerhalb der Methode bearbeiten und den Befehl "Ansicht hinzufügen" im Kontextmenü auswählen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Das Dialogfeld "Ansicht hinzufügen" in "geben wir, dass wir zu unserer Vorlage anzeigen, wie das Modell ein Dinner-Objekt übergeben, und wählen die Auto-Gerüst eine Vorlage mit"Bearbeiten":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, werden Visual Studio eine neue "Edit.aspx" ansichtsvorlagendatei für uns innerhalb des Verzeichnisses "\Views\Dinners" hinzugefügt. Es wird auch die neue "Edit.aspx" ansichtsvorlage im Code-Editor – mit einer anfänglichen "Bearbeiten" Scaffold Implementierung wie unten gefüllt geöffnet:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Wir nehmen einige Änderungen an der standardmäßigen "Bearbeiten" Scaffold generiert, und aktualisieren Sie die ansichtsvorlage bearbeiten, um den Inhalt weiter unten haben (die nur einige der Eigenschaften, die wir nicht verfügbar machen möchten, wird entfernt):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Ausführen der Anwendung und die Anforderung der *"/ Dinners/Edit/1"* URL, die wir die folgende Seite angezeigt wird:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Das HTML-Markup generiert, die durch unsere Ansicht sieht wie folgt aus. Es ist die standard-HTML – mit einer &lt;Formular&gt; -Element, das HTTP POST für führt die */Dinners/Edit/1* URL beim "Speichern" &lt;Eingabetyp = "Absenden" /&gt; Schaltfläche wird mithilfe von Push übertragen. Eine HTML &lt;Eingabetyp = "Text" /&gt; Element wurde die Ausgabe für jede Eigenschaft bearbeitet werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() und Html.TextBox() Html-Hilfsmethoden

Unsere "Edit.aspx" ansichtsvorlage verwendet mehrere "HTML-Hilfsmethoden" zur Verfügung: Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() und Html.ValidationMessage(). Zusätzlich zum Generieren von HTML-Markup für uns, diese Hilfsmethoden bieten integrierte Verarbeitung und Validierung unterstützen.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm()-Hilfsmethode

Die Html.BeginForm()-Hilfsmethode ist, welche Ausgabe von den HTML-Code &lt;Formular&gt; Element im unser Markup. In unserem Edit.aspx-Vorlage anzeigen werden Sie feststellen, dass wir eine C#-Anweisung "using" bei Verwendung dieser Methode angewendet wird. Die geöffnete geschweifte Klammer kennzeichnet den Anfang der &lt;Formular&gt; Inhalt und die schließende geschweifte Klammer ist was bedeutet, dass das Ende der &lt;/form&gt; Element:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Sie können auch, wenn Sie feststellen, dass die Anweisung "using"-Ansatz für ein derartiges Szenario unnatürlichen, können Sie eine Kombination von Html.BeginForm() und Html.EndForm() (die die gleiche Aufgabe ausführt):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Html.BeginForm() ohne Parameter aufrufen, wird es ein Formularelement ausgegeben, die eine HTTP-POST zur URL für die aktuelle Anforderung ausführt. D. h., warum unser Bearbeitungsansicht generiert eine *&lt;form Aktion = "/ Dinners/Edit/1"-Methode = "post"&gt;* Element. Wir können auch haben explizite Parameter an übergeben Html.BeginForm() Wenn wir zu einer anderen URL veröffentlichen möchten.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox()-Hilfsmethode

Unserer Ansicht Edit.aspx verwendet die Hilfsmethode Html.TextBox() zur Ausgabe &lt;Eingabetyp = "Text" /&gt; Elemente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Die oben angegebenen Html.TextBox()-Methode nimmt einen einzelnen Parameter – der verwendet wird, sowohl die Name/Id-Attribute des an den &lt;Eingabetyp = "Text" /&gt; Element, um die Ausgabe als auch die Modelleigenschaft zum Auffüllen des Textbox-Werts aus. Z. B. die Dinner-Objekt, das wir an die Bearbeitungsansicht übergeben hatte "Title" des Werts von ".NET Futures", und daher unsere Html.TextBox("Title") Methodenaufruf Ausgabe: *&lt;Eingabe-Id = "Title" Name = "Title" Type = "Text" Value = ".NET Futures" /&gt;*.

Alternativ können wir den ersten Html.TextBox()-Parameter verwenden, zum Angeben der Id/Name des Elements, und klicken Sie dann den Wert für die Verwendung als zweiten Parameter explizit übergeben:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Häufig möchten wir führen Sie die benutzerdefinierte Formatierung auf den Wert, der Ausgabe ist. String.Format() statische Methode erstellt in .NET ist für die folgenden Szenarien nützlich. Unsere Edit.aspx ansichtsvorlage verwendet dies zum Formatieren des EventDate-Werts (was der Typ "DateTime" ist), damit es nicht Sekunden für die Zeit angezeigt:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Ein dritter Parameter Html.TextBox() kann optional verwendet werden, um zusätzliche HTML-Attribute auszugeben. Der folgende Codeausschnitt veranschaulicht, wie zum Rendern einer zusätzlichen Speicherplatz = "30"-Attribut und eine Klasse = "Mycssclass"-Attribut auf die &lt;Eingabetyp = "Text" /&gt; Element. Beachten Sie, wie wir den Namen des Attributs für die Klasse mit Escapezeichen sind ein "@" character because "Klasse" ist ein reserviertes Schlüsselwort in c#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementieren die HTTP-POST-Edit-Aktion-Methode

Wir haben jetzt die HTTP-GET-Version unserer Edit-Aktionsmethode, die implementiert werden. Wenn ein Benutzer fordert die */Dinners/Edit/1* URL, die sie erhalten eine HTML-Seite wie folgt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Auf die Schaltfläche "Speichern" bewirkt, dass ein Formular-Post an die */Dinners/Edit/1* -URL und sendet den HTML-Code &lt;Eingabe&gt; Werte, die mit dem HTTP POST-Verb zu bilden. Lassen Sie uns nun Verhalten implementieren, die HTTP POST unsere Aktionsmethode bearbeiten – das Speichern des Dinner behandelt.

Zunächst müssen wir unsere "dinnerscontroller", die ein Attribut "AcceptVerbs" auf, der angibt, dass es sich um Szenarien mit HTTP POST behandelt eine überladene Aktionsmethode "Bearbeiten" hinzugefügt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Wenn das Attribut [AcceptVerbs] für überladene Aktionsmethoden angewendet wird, verarbeitet ASP.NET MVC automatisch die Verteilung von Anforderungen an die entsprechende Aktionsmethode abhängig von der eingehenden HTTP-Verb. HTTP-POST-Anforderungen an <em>"/ dinners" / Edit / [Id]</em> URLs werden gesendet, während alle anderen HTTP-Verb-Anforderungen an die oben dargestellte bearbeiten-Methode <em>"/ dinners" / Edit / [Id]</em>URLs geht die erste bearbeiten-Methode implementiert (das war keine, ein Attribut [AcceptVerbs]).

| **Seite-Thema: Warum über HTTP-Verben unterscheiden?** |
| --- |
| Sie Fragen sich vielleicht – warum wir mit einer einzelnen URL und das Verhalten über das HTTP-Verb unterschieden werden? Warum nicht zwei separate URLs laden und speichern die Änderungen behandelt lassen? Zum Beispiel: "/ dinners" / Edit / [Id] des anfänglichen Formulars und "/ dinners" / speichern / [Id], behandeln das Formular-Post, um den Bericht speichern? Der Nachteil mit Veröffentlichung zwei separate URLs besteht darin, dass in Fällen, in denen wir in /Dinners/Save/2 Posten, und klicken Sie dann das HTML-Formular aufgrund eines Fehlers Eingabe erneut anzeigen möchten, der Endbenutzer die Dinner/speichern/2-URL in die Adressleiste des Browsers letztendlich wird (da das war die URL die Form, die an gesendet). Wenn der Endbenutzer dieser redisplayed Seite können Sie ihre Browser-Favoriten, Lesezeichen oder kopieren/fügt die URL und sendet es per an einen Freund e-Mail, kommen sie werden eine URL, die nicht in der Zukunft funktionieren, (da diese URL von Post-Werten abhängig ist) zu speichern. Durch Verfügbarmachen von einer einzelnen URL (z. B.: /Dinners/Edit/[id]) und Differenzierung von der Verarbeitung von HTTP-Verb, ist für Endbenutzer auf der Seite "Bearbeiten" Lesezeichen und/oder senden die URL an andere Benutzer sicher. |

#### <a name="retrieving-form-post-values"></a>Abrufen von Werten der Formular-Post

Es gibt eine Vielzahl von Möglichkeiten, wie, die wir zugreifen können, die Formular-Parametern in die HTTP POST-Methode "Bearbeiten" bereitgestellt. Ein einfacher Ansatz ist, verwenden Sie die Anforderung-Eigenschaft nur für die Basisklasse für Controller, der formularauflistung zugreifen und die bereitgestellten Werte direkt abrufen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Der oben beschriebene Ansatz ist jedoch etwas umständlich, insbesondere, wenn wir die Verwendung einer Fehlerbehandlungslogik hinzufügen.

Ein besserer Ansatz für dieses Szenario ist, nutzen die integrierte *UpdateModel()* Hilfsmethode für die Controller-Basisklasse. Es unterstützt das Aktualisieren der Eigenschaften eines Objekts, die wir mit der eingehenden Formularparameter übergeben. Er verwendet Reflektion, um zu bestimmen, die Eigenschaftennamen für das Objekt, und klicken Sie dann automatisch konvertiert und weist Werte auf Grundlage der Eingabewerten, die vom Client gesendet werden.

Wir konnten die UpdateModel()-Methode verwenden, um unsere HTTP-POST-Aktion Bearbeiten mit dem folgenden Code zu vereinfachen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Wir können jetzt besuchen der */Dinners/Edit/1* URL, und ändern Sie den Titel des unsere Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Wenn wir auf die Schaltfläche "Speichern" klicken, eine formularbereitstellung auf unsere Bearbeitungsaktion ausgeführt und die aktualisierten Werte werden in der Datenbank beibehalten. Wir werden dann an die URL für die Details für das Dinner umgeleitet (die den neu gespeicherten Werte angezeigt werden):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Behandeln von Fehlern der Bearbeiten

Unsere aktuellen HTTP-POST-Implementierung funktioniert die Operation problemlos – außer wenn Fehler vorhanden sind.

Wenn ein Benutzer einen Fehler, die Bearbeitung eines Formulars macht, müssen wir sicherstellen, dass das Formular mit einer aussagekräftigen Fehlermeldung erneut angezeigt wird, die sie an, um das Problem zu beheben führt. Dies schließt Fälle, in denen ein Endbenutzer falsche Eingabe sendet (z. B.: eine falsch formatierte Datumszeichenfolge), wie das Eingabeformat auch ausgabeinhalten designereinstellungen gültig ist, aber es eine geschäftsregelverletzung gibt. Wenn Fehler auftreten, dass die Eingabedaten der Benutzer ursprünglich eingegeben haben, damit sie nicht die Änderungen manuell Auffüllen der Form beibehalten werden soll. Dieser Prozess soll so oft wie erforderlich wiederholt, bis das Formular erfolgreich abgeschlossen wurde.

ASP.NET MVC umfasst einige nette integrierte Features, die Fehlerbehandlung und Formular erneut anzeigen zu erleichtern. Um diese Funktionen in Aktion sehen aktualisieren unserer Edit-Aktionsmethode durch folgenden Code:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Der obige Code ist ähnlich zur vorherigen Implementierung –, mit dem Unterschied, dass für unsere Arbeit nun einen Try/Catch-Fehlerbehandlung-Block umschlossen wird. Wenn eine Ausnahme auftritt, wird beim Aufrufen von UpdateModel() oder wenn wir versuchen, und speichern die "dinnerrepository" (die eine Ausnahme ausgelöst wird, wenn die Dinner-Objekt, das wir zu speichern versuchen, die aufgrund einer Verletzung der Regel in unserem Modell ungültig ist), unsere Erfassungsblock für Fehler behandeln ausgeführt werden. Darin werden-Schleife über jede Verletzungen, die in das Dinner-Objekt vorhanden und ein ModelState-Objekt (das wir in Kürze eingehen werde) hinzufügen. Wir erneut klicken Sie dann die Ansicht anzuzeigen.

Um dies zu arbeiten, führen wir die Anwendung erneut sehen, bearbeiten Sie eine Dinner zu, und ändern Sie, um ein leerer Titel, eine EventDate von "GEFÄLSCHTES", und verwenden eine Telefonnummer Vereinigtes Königreich, mit dem Land-Wert für die USA. Wenn wir auf die Schaltfläche "Speichern" drücken unsere Bearbeiten von HTTP-POST-Methode ist nicht möglich die Dinner gespeichert wird (da Fehler vorliegen) und wird erneut das Formular anzuzeigen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Die Anwendung hat eine ordentliche Fehlermeldungen erspart. Der Text-Elemente, mit der ungültigen Eingabe sind rot markiert, und Meldungen für Validierungsfehler werden für den Endbenutzer dazu angezeigt. Das Formular wird auch die eingegebenen Daten, die der Benutzer ursprünglich eingegebenen – beibehalten, damit sie nicht alles auffüllen.

Wie kommen Sie möglicherweise aufgefordert, es dazu? Wie die Textfelder für Titel, EventDate und "contactphone" ein markieren sich selbst in Rot und wissen, um die Werte der ursprünglich eingegebenen Benutzer auszugeben? Und die Anzeige Fehlermeldungen erhalten in der Liste oben? Die gute Nachricht ist, dass dies nicht der Fall Zauberei sein – anstatt es war, weil wir einige der integrierten ASP.NET MVC-Features verwendet, die Überprüfung von Benutzereingaben und Fehler behandeln Szenarios erleichtern.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Grundlegendes zu ModelState und die Validierung HTML-Hilfsmethoden

Controller-Klassen müssen Eigenschaftensammlung "ModelState" bietet eine Möglichkeit, um anzugeben, dass Fehler vorhanden sind, mit dem Model-Objekts an eine Ansicht übergeben wird. Fehlereinträge in der Auflistung ModelState Geben Sie den Namen der Modelleigenschaft mit dem Problem (z. B.: "Title", "EventDate" oder "ContactPhone"), und ermöglichen eine Benutzerfreundlicher Fehlermeldung angegeben werden (z. B.: "Der Titel ist erforderlich").

Die *UpdateModel()* Hilfsmethode füllt automatisch die ModelState-Auflistung, wenn beim Versuch, die Eigenschaften für das Modellobjekt Formularwerte zuweisen Fehler auftreten. Beispielsweise ist das Dinner-Objekt EventDate-Eigenschaft vom Typ "DateTime". Wenn die UpdateModel()-Methode kann nicht den Zeichenfolgenwert "GEFÄLSCHTES", in dem oben beschriebenen Szenario zugewiesen war, hinzugefügt die UpdateModel()-Methode, dass ein Eintrag der ModelState-Auflistung, der angibt, ein Zuweisungsfehler auftritt durchgeführt wurde, dieser Eigenschaft.

Entwickler können auch Code schreiben, um Fehlereinträge in die Auflistung ModelState explizit hinzufügen, wie unten in unsere "catch" Fehler Block zur Ausnahmebehandlung, Meinung der Einträge, die basierend auf dem aktiven Verletzungen der Schwellenwertregeln in der Auflistung ModelState aufgefüllt wird die Dinner-Objekt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>HTML-Hilfsobjekt ModelState-Integration

HTML-Hilfsmethoden – wie Html.TextBox() - überprüfen Sie die ModelState-Auflistung, bei der Ausgabe zu rendern. Wenn ein Fehler für das Element vorhanden ist, rendern sie die vom Benutzer eingegebenen Wert und eine CSS-Fehler-Klasse.

Z. B. in der Ansicht "Bearbeiten" verwenden die Hilfsmethode Html.TextBox() wir zum Rendern der EventDate unsere Dinner-Objekts:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Wenn die Ansicht im Fehlerszenario gerendert wurde, überprüft die Html.TextBox()-Methode die ModelState-Auflistung, um festzustellen, ob alle Fehler im Zusammenhang mit der Eigenschaft "EventDate" unsere Dinner-Objekts. Wenn ermittelt, ob ein Fehler aufgetreten, Rendern die übermittelten Benutzereingabe ("GEFÄLSCHTES") als Wert und eine CSS-Fehlerklasse hinzugefügt der &lt;Eingabetyp = "Textbox" /&gt; Markup er generiert:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Sie können die Darstellung der CSS-Error-Klasse, suchen beliebig anpassen. Die Standardklasse für das "CSS-Fehler" – "Eingabe-Überprüfung: Fehler" – wird definiert, der *\content\site.css* Stylesheet und das aussieht wie die folgende:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Diese CSS-Regel ist Ursache von unserem ungültige Eingabe Elemente wie unten hervorgehoben werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage()-Hilfsmethode

Die Hilfsmethode Html.ValidationMessage() kann verwendet werden, um die Ausgabe der ModelState-Fehlermeldung, die Eigenschaft eines bestimmten Modells zugeordnet:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Der obige Code gibt:  *&lt;span-Klasse = "Feld-Validation-Error"&gt; der Wert "GEFÄLSCHTES" ist ungültig &lt; /span&gt;*

Die Hilfsmethode Html.ValidationMessage() unterstützt auch einen zweiten Parameter, mit dem Entwickler, die Text-Fehlermeldung zu überschreiben, die angezeigt wird:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Der obige Code gibt:  <em>&lt;span-Klasse = "Feld-Validation-Error"&gt;\*&lt;/span&gt;</em>anstelle der Fehler Standardtext, wenn ein Fehler vorhanden ist. die EventDate-Eigenschaft.

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary()-Hilfsmethode

Die Hilfsmethode Html.ValidationSummary() dienen zum Rendern einer zusammenfassende Fehlermeldung, die zusammen mit einem &lt;Ul&gt;&lt;li /&gt;&lt;/UL&gt; Liste mit allen detaillierte Fehlerinformationen von statusmeldungen in der ModelState-Sammlung:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Die Hilfsmethode Html.ValidationSummary() nimmt einen Parameter optionale Zeichenfolge – in der es eine zusammenfassende Fehlermeldung definiert, der über die Liste der detaillierte Fehler angezeigt:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Sie können optional CSS verwenden, überschreiben, wie es in die Fehlerliste aussieht.

#### <a name="using-a-addruleviolations-helper-method"></a>Mit der eine AddRuleViolations-Hilfsmethode

Die erste bearbeiten Sie die HTTP-POST-Implementierung verwendet eine Foreach-Anweisung innerhalb des catchblocks-Schleife über Verletzungen des Dinner-Objekts und des Controllers ModelState-Sammlung hinzufügen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Wir können diesen Code eine wenig klarer durch Hinzufügen einer "ControllerHelpers"-Klasse auf das NerdDinner-Projekt, und implementieren eine "AddRuleViolations"-Erweiterungsmethode, darin, die eine Hilfsmethode für die ASP.NET MVC ModelStateDictionary-Klasse fügt vornehmen. Diese Erweiterungsmethode kann die erforderliche Logik zum Auffüllen der ModelStateDictionary mit einer Liste von Fehlern der RuleViolation kapseln:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Wir können unsere Methode, um diese Erweiterungsmethode verwenden zum Auffüllen der ModelState-Auflistung mit unserer Verletzungen von Namensregeln Dinner HTTP-POST bearbeiten Aktion dann aktualisieren.

#### <a name="complete-edit-action-method-implementations"></a>Führen Sie die Aktion-Methodenimplementierungen bearbeiten

Der folgende Code implementiert alle der Controllerlogik, die in diesem Szenario bearbeiten erforderlich:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Das Schöne an unserer Implementierung bearbeiten ist, dass keine unsere Controllerklasse oder unsere Vorlage anzeigen möchte etwas über die spezifische Validierung oder Geschäftsregeln, die vom Dinner Modells erzwungen wird. Wir können weitere Regeln hinzufügen unseres Modells in der Zukunft und müssen keine codeänderungen vornehmen, unser Controller oder einer Ansicht in der Reihenfolge für diese unterstützt werden müssen. Dies bietet uns die Flexibilität, unsere anwendungsanforderungen in Zukunft mit einem Minimum von codeänderungen auf einfache Weise entwickeln.

### <a name="create-support"></a>Erstellen von Support

Wir haben die Implementierung des Verhaltens "Bearbeiten" unserer Klasse "dinnerscontroller" abgeschlossen. Lassen Sie uns nun die Unterstützung von "Erstellen" auf – implementieren, die Benutzer neue Dinner hinzufügen können.

#### <a name="the-http-get-create-action-method"></a>Die HTTP-GET erstellen Aktionsmethode

Wir beginnen, indem Sie "GET"-Verhalten der HTTP-Implementierung unserer Action-Methode zu erstellen. Diese Methode wird aufgerufen, wenn jemand besucht die */Dinners/erstellen* URL. Unsere Implementierung sieht folgendermaßen aus:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Der obige Code erstellt ein neues Dinner-Objekt, und weist die EventDate-Eigenschaft einer Woche in der Zukunft sein. Es rendert dann eine Ansicht, die für das neue Dinner-Objekt basiert. Da wir ein Name explizit übergeben haben die *View()* Hilfsmethode verwendet den konventionsbasierten Standardpfad für die ansichtsvorlage zu beheben: /Views/Dinners/Create.aspx.

Jetzt erstellen wir diese Vorlage anzeigen. Wir erreichen dies, indem Sie mit der rechten Maustaste innerhalb der Aktionsmethode erstellen, und wählen den Befehl "Ansicht hinzufügen" im Kontextmenü. Das Dialogfeld "Ansicht hinzufügen" in "geben wir, dass wir ein Dinner-Objekt, das die ansichtsvorlage übergeben, und wählen die Auto-Scaffold"Erstellen"Vorlage:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Beim Klicken auf die Schaltfläche "Hinzufügen", Visual Studio eine neue Ansicht für Gerüst-basierter "Create.aspx" in das Verzeichnis "\Views\Dinners" zu speichern, und öffnen sie in der IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Wir nehmen einige Änderungen an der Standard "erstellen" Scaffold Datei, die generiert wurde für uns, und ändern Sie sie einrichten, um wie folgt aussehen:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Und jetzt ausführen unserer Anwendung und den Zugriff der *"/ Dinners/Create"* URL im Browser, die Benutzeroberfläche wie dem folgenden aus unserer Implementierung der erstellen-Aktion gerendert wird:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementieren die HTTP-POST-Action-Methode erstellen

Wir haben die HTTP-GET-Version des unsere Create-Aktion-Methode implementiert. Klickt ein Benutzer die Schaltfläche "Speichern" wird ein Formular-Post an die */Dinners/erstellen* -URL und sendet den HTML-Code &lt;Eingabe&gt; Werte, die mit dem HTTP POST-Verb zu bilden.

Implementieren Sie das HTTP-POST-Verhalten der jetzt unsere Aktionsmethode zu erstellen. Zunächst müssen wir unsere "dinnerscontroller", die ein Attribut "AcceptVerbs" auf, der angibt, dass es sich um Szenarien mit HTTP POST behandelt eine überladene Methode von "Erstellen" Aktion hinzufügen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Es gibt eine Vielzahl von Möglichkeiten, wie, die wir die Formular-Parametern in unsere "Erstellen" aktiviert von HTTP-POST-Methode zugreifen können.

Ein Ansatz besteht darin, ein neues Dinner-Objekt erstellen und verwenden dann die *UpdateModel()* Hilfsmethode (wie mit der Aktion bearbeiten), es mit den bereitgestellten Formularwerten zu füllen. Wir können dann unsere "dinnerrepository" hinzu, in der Datenbank gespeichert, und leiten Sie den Benutzer auf unsere Details-Aktion, die neu erstellte Dinner anhand des folgenden Codes angezeigt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternativ können wir einen Ansatz verwenden, haben wir unsere Create()-Aktionsmethode, die ein Dinner-Objekt als Methodenparameter angegeben werden. ASP.NET MVC wird dann automatisch instanziieren ein neues Dinner-Objekt, für uns, füllen Sie die Eigenschaften der formeingaben und an unserer Action-Methode übergeben:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Unsere Action-Methode, die oben genannten stellt sicher, dass die Dinner-Objekt durch Überprüfen der "ModelState.IsValid"-Eigenschaft wurde erfolgreich mit den Formular-Post-Werten aufgefüllt wurde. Geben Sie "false", treten Probleme bei der Konvertierung wird zurückgegeben (z. B.: eine Zeichenfolge mit "GEFÄLSCHTES" für die EventDate-Eigenschaft), und treten Probleme mit der handelt es sich bei unserer Aktionsmethode zeigt das Formular.

Wenn der Eingabewerten gültig sind, versucht die Action-Methode hinzufügen, und speichern das neue Dinner in der "dinnerrepository". Es dient als Wrapper für diese Arbeit innerhalb eines Try/Catch-Blocks und zeigt das Formular, wenn Verletzungen der Schwellenwertregeln für Unternehmen (die die dinnerRepository.Save()-Methode zum Auslösen einer Ausnahme führen würde) vorhanden sind.

Um diese Fehlerbehandlung in Aktion zu sehen, können wir bitten die */Dinners/erstellen* -URL und füllen Sie die Details zu einem neuen Dinner. Fehlerhafte Eingabe oder Werte werden dazu führen, dass das Erstellungsformular, die Fehler, wie unten hervorgehoben erneut angezeigt werden:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Beachten Sie, wie unser Formular erstellen genauen dieselben Validierung und Business Regeln wie für unsere Bearbeitungsformular berücksichtigt. Dies ist daran, dass unsere Validierung und Geschäftsregeln, die im Modell definiert wurden, und nicht in der Benutzeroberfläche oder die Controller der Anwendung eingebettet wurden. Dies bedeutet, dass wir können später ändern/weiterentwickeln, unsere Validierungstests oder Geschäftsregeln in einem einzelnen platzieren und Sie in unserer Anwendung anwenden. Wir müssen nicht so ändern Code innerhalb unserer bearbeiten oder erstellen die Aktionsmethoden anwenden, um neue Regeln oder vorhandene Datasets ändern automatisch berücksichtigt.

Wenn wir den Eingabewerten vornimmt beheben, und klicken Sie auf die Schaltfläche "Speichern" in diesem Fall unsere zusätzlich zu den "dinnerrepository" wird erfolgreich ausgeführt, und eine neue Dinner wird in der Datenbank hinzugefügt werden. Wir gelangen klicken Sie dann auf die *"/ dinners" / Details / [Id]* URL – wir wird, in denen angezeigt werden, mit den Details der neu erstellten Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Delete-Unterstützung

Nun fügen Sie "Löschen"-Unterstützung auf unserem "dinnerscontroller".

#### <a name="the-http-get-delete-action-method"></a>Die HTTP-GET-Delete-Aktion-Methode

Wir beginnen, indem Sie die HTTP GET-Verhalten, unsere Delete-Aktion-Methode zu implementieren. Diese Methode wird aufgerufen, wenn jemand besucht die *"/ dinners" / Delete / [Id]* URL. Im folgenden ist die Implementierung:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Die Action-Methode versucht, die die Dinner zu löschenden abzurufen. Ggf. das Dinner rendert anhand eine Ansicht des Dinner-Objekts ab. Wenn das Objekt ist nicht vorhanden (oder bereits gelöscht wurde) es anzeigen Gibt eine Ansicht, die die "NotFound" rendert Vorlage, die wir zuvor erstellt, für die Aktionsmethode unsere "Details haben".

Wir können die ansichtsvorlage "Löschen" erstellen, indem Sie mit der rechten Maustaste innerhalb der Delete-Aktionsmethode, und wählen den Befehl "Ansicht hinzufügen" im Kontextmenü. Das Dialogfeld "Ansicht hinzufügen" in "geben wir, dass wir auf unsere Vorlage anzeigen, wie das Modell ein Dinner-Objekt übergeben, und wählen eine leere Vorlage zu erstellen:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, werden Visual Studio eine neue "Delete.aspx" ansichtsvorlagendatei für uns in unserer "\Views\Dinners"-Verzeichnis hinzugefügt. Wir fügen ist weiterer HTML- und Code der Vorlage ein Bestätigungsfenster löschen implementiert wie die folgende:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Der obige Code zeigt den Titel der Dinner gelöscht werden soll, und Ausgaben einer &lt;Formular&gt; -Element, das einen Beitrag zur "/ dinners" / Delete / [Id] URL ausgeführt werden, wenn der Endbenutzer auf die Schaltfläche "Löschen" darin klickt.

Ausführen unserer Anwendung und den Zugriff der *"/ Dinners/Delete / [Id]"* URL zu einem gültigen Dinner Objekt rendert, wie die folgende Benutzeroberfläche:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Seite-Thema: Warum machen wir einen Beitrag?** |
| --- |
| Sie Fragen sich vielleicht – warum wir gehen über den Aufwand zum Erstellen einer &lt;Formular&gt; innerhalb unserer Bestätigungsbildschirm löschen? Warum nicht einfach einen standard-Link zu einer Aktionsmethode eine Verknüpfung mit, die den tatsächlichen Löschvorgang ausführt? Der Grund ist, da wir möchten, achten Sie darauf, dass Sie zum Schutz vor Webcrawlern und Suchmaschinen ermitteln unsere URLs und verursacht versehentlich Daten gelöscht werden, wenn sie den Links folgen. HTTP-GET-basierte URLs werden als "sicher" zu Access/Durchforstung betrachtet, und sie sollen um nicht auf HTTP-POST zu folgen. Gute Faustregel ist, Sie sicher, dass Sie immer destruktive oder Ändern von Datenvorgängen hinter HTTP-POST-Anforderungen. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementieren die HTTP-POST Delete-Aktion-Methode

Wir haben jetzt die HTTP-GET-Version unserer Delete-Aktion-Methode implementiert eine löschen-Bestätigungsbildschirm angezeigt. Klickt ein Benutzer die Schaltfläche "Löschen". Führen Sie ein Formular-Post an die *"/ dinners" Dinner / [Id]* URL.

Lassen Sie uns nun Verhalten implementieren, die HTTP-"POST" die Delete-Aktionsmethode, die anhand des folgenden Codes:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Die HTTP-POST-Version unserer Delete-Aktion-Methode versucht, das zu löschende Dinner-Objekt. Wenn sie gefunden werden kann (da es bereits gelöscht wurde) unserer Vorlage "NotFound" gerendert. Wenn das Dinner gefunden wird, löscht es diese aus der "dinnerrepository" ein. Klicken Sie dann eine Vorlage "Deleted" gerendert.

So implementieren Sie die Vorlage "Deleted" wir mit der rechten Maustaste in der Aktionsmethode und wählen Sie im Kontextmenü "Ansicht hinzufügen" Wir nennen unsere Ansicht "Gelöschte" und es werden eine leere Vorlage (und kein stark typisiertes Modell-Objekt). Wir werden dann einige HTML-Inhalt hinzufügen:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Und jetzt ausführen unserer Anwendung und den Zugriff der *"/ Dinners/Delete / [Id]"* URL zu einem gültigen Dinner-Objekt wird gerendert, unsere Dinner Löschen bestätigen Bildschirm wie unten:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Beim Klicken auf die Schaltfläche "Löschen". Führen Sie eine HTTP-POST an die *"/ dinners" / Delete / [Id]* -URL, die Dinner aus der Datenbank löschen und unsere ansichtsvorlage "Deleted" angezeigt:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Modellbindung, Sicherheit

Wir haben zwei Möglichkeiten, verwenden Sie die integrierten modellbindungs-Features von ASP.NET MVC erläutert. Der zuerst mithilfe der Methode UpdateModel() Aktualisieren der Eigenschaften für ein vorhandenes Modellobjekt, und die Sekunde unter Verwendung ASP.NETs-Unterstützung für die Übergabe von modellobjekten in als Aktionsmethodenparameter. Beide Verfahren sind sehr leistungsstark und äußerst nützlich.

Diese Berechtigung ist auch mit Verantwortung. Es ist wichtig, immer paranoide Benutzer über die Sicherheit sein, wenn Benutzereingaben akzeptiert, und dies gilt auch beim Binden von Objekten an Formulareingabe. Sie sollten darauf achten, HTML-Codierung immer der freien Eingabe von Werten HTML und JavaScript-Injection-Angriffe zu vermeiden, und achten Sie darauf, dass Sie von SQL-Injection-Angriffen (Hinweis: mit LINQ to SQL sind wir für unsere Anwendung, die Parameter, um zu verhindern, dass diese automatisch codiert Arten von Angriffen). Sie sollten nie verlassen sich auf die clientseitige Validierung allein und verwenden immer eine serverseitige Validierung zum Schutz gegen Hacker, die Sie falsche Werte senden.

Ein zusätzliche Sicherheit-Element, um sicherzustellen, dass Sie sich vorstellen, der Bindung Funktionen von ASP.NET MVC ist der Bereich der Objekte, die Sie binden. Insbesondere möchten sicherstellen, dass Sie verstehen, dass die Auswirkungen auf die Sicherheit der Eigenschaften, die Sie zulassen, die gebunden werden, und stellen Sie sicher, dass Sie nur diese Eigenschaften gestatten, die tatsächlich durch einen Endbenutzer aktualisiert werden, aktualisiert werden sollen.

Standardmäßig versucht die UpdateModel()-Methode, die alle Eigenschaften für das Modellobjekt zu aktualisieren, die eingehenden Parameterwerte in Form entsprechen. Ebenso können als Parameter übergebene Objekte Action-Methode auch standardmäßig alle ihre Eigenschaften, die über die Formular-Parametern festgelegt haben.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Sperren der Bindung auf einer pro-Usage-basis

Sie können die Richtlinie auf einer pro-Usage-Basis sperren, durch die Bereitstellung eine explizite "include Liste" von Eigenschaften, die aktualisiert werden können. Dies kann durch Übergeben eines zusätzlichen Zeichenfolge, die Array-Parameters der Methode UpdateModel() wie unten durchgeführt werden:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objekte, die auch als Aktionsmethodenparameter übergeben unterstützen ein [Bind]-Attribut, die es ermöglicht, eine "include Liste" der zulässigen Eigenschaften wie unten angegeben werden:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Sperren der Bindung einer Typ-basis

Sie können auch die Bindungsregeln auf einer pro-Typ-Basis sperren. Dadurch können Sie die Bindungsregeln für die einmal angeben und dann haben sie die in allen Szenarien (einschließlich der Methode Parameter Szenarien mit sowohl UpdateModel und Aktion) für alle Controller und Aktionsmethoden anwenden.

Sie können die Bindungsregeln pro Typ anpassen, durch ein [Bind]-Attribut auf einen Typ hinzufügen oder indem Sie sie in der Global.asax-Datei der Anwendung (nützlich für Szenarien, in denen Sie nicht den Typ besitzen) zu registrieren. Anschließend können Sie die Bind-Attribut einschließen und Ausschließen von Eigenschaften, die steuern, welche Eigenschaften sind an bestimmte Klasse oder Schnittstelle.

Wir verwenden Sie dieses Verfahren für die Dinner-Klasse in unserer NerdDinner-Anwendung, und fügen Sie ein [Bind]-Attribut hinzu, die die Liste der bindbare Eigenschaften auf Folgendes beschränkt:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Beachten Sie, dass wir die Bestätigungen-Auflistung, die über Datenbindung bearbeitet werden nicht zugelassen sind, und lassen wir die DinnerID oder HostedBy-Eigenschaft, die über die Bindung festgelegt werden. Aus Sicherheitsgründen müssen wir stattdessen nur bestimmten Bearbeitung dieser Eigenschaften mit expliziten Code innerhalb von unserem Aktionsmethoden.

### <a name="crud-wrap-up"></a>CRUD-Zusammenfassung

ASP.NET MVC umfasst eine Reihe von integrierten Funktionen, mit deren Hilfe bei der Implementierung der Form senden Szenarien. Eine Reihe von diesen Funktionen verwendet, um Unterstützung für CRUD-UI auf unserem "dinnerrepository" bereitstellen.

Wir verwenden einen Modell-orientierten Ansatz zum Implementieren von unserer Anwendung. Dies bedeutet, dass jegliche Überprüfung und Business-Regel, dass die Logik in unser Modellschicht – und nicht in unserem Controller oder Ansichten definiert ist. Weder für unsere Controller-Klasse als auch für unsere Ansichtsvorlagen wissen nichts über den jeweiligen Geschäftsregeln, die durch unsere Dinner-Modellklasse erzwungen wird.

Dies unsere Anwendungsarchitektur bereinigen beibehalten, und Testen vereinfachen. Wir können unsere Modellschicht zusätzliche Geschäftsregeln in der Zukunft hinzufügen und *müssen keine Änderungen am Code vornehmen* auf unser Controller oder die Ansicht in der Reihenfolge für diese unterstützt werden müssen. Dies geht, uns zu viel Flexibilität weiterentwickeln und die Anwendung in der Zukunft ändern.

Unsere "dinnerscontroller" nun ermöglicht die Dinner-Angebote/Details, als auch erstellen, bearbeiten und löschen unterstützt. Der vollständige Code für die Klasse finden Sie weiter unten:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt die grundlegenden CRUD (Create, Read, Update und Delete)-Unterstützung, die in unserer Klasse "dinnerscontroller" implementieren.

Jetzt sehen wir uns wie wir "ViewData" und "ViewModel"-Klassen verwenden können, um noch umfassendere Benutzeroberfläche auf unseren Formularen zu aktivieren.

> [!div class="step-by-step"]
> [Zurück](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Weiter](use-viewdata-and-implement-viewmodel-classes.md)
