---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Nutzen Sie die Benutzeroberfläche mit Masterseiten und Teilausführungen | Microsoft-Dokumentation
author: microsoft
description: Schritt 7 untersucht die Methoden, die wir "DRY-Prinzips" anwenden können in unseren Vorlagen anzeigen, um codeduplikate, mithilfe von Teilansichten, Vorlagen und Masterseiten zu vermeiden.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0da32e6ac38f10df6e581517989b3b1fd2f2328c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828302"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Nutzen Sie die Benutzeroberfläche mit Masterseiten und Teilausführungen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 7 der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 7 untersucht die Methoden, die wir "DRY-Prinzips" anwenden können in unseren Vorlagen anzeigen, um codeduplikate, mithilfe von Teilansichten, Vorlagen und Masterseiten zu vermeiden.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner, Schritt 7: Teilansichten und Masterseiten

Einer der die Entwurfsphilosophien, die ASP.NET MVC umfasst ist das "Ist nicht wiederhole dich"-Prinzip (häufig als "DRY" bezeichnet). Eine DRY Gestaltung hilft dabei, entfernen die Duplizierung von Code und die Logik, die letztlich wird von Anwendungen, die zum Erstellen schneller und einfacher zu verwalten.

Wir haben das DRY-Prinzip angewendet wird, in einigen Szenarios NerdDinner gesehen. Einige Beispiele: unsere Validierungslogik wird implementiert, in unserem Modellebene bearbeitungs-erzwungen werden, und erstellen in unserem Controller; Szenarien ermöglicht Wir verwenden wieder die ansichtsvorlage "NotFound" auf die Aktionsmethoden bearbeiten "," Details "und" Delete; Wir verwenden eine Konvention - Benennungsmuster mit unseren Vorlagen anzeigen, die entfällt die Notwendigkeit, den Namen explizit anzugeben, beim Aufrufen der Hilfsmethode View(); und wir verwenden Sie erneut die DinnerFormViewModel-Klasse für Bearbeitungs- und Aktion Szenarien zu erstellen.

Jetzt sehen wir uns Möglichkeiten, die wir "DRY-Prinzips" anwenden, können in unseren Vorlagen anzeigen, um codeduplikate gibt es auch zu entfernen.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Besuchen Sie unsere bearbeiten erneut aus, und erstellen Sie Vorlagen anzeigen

Zurzeit verwenden wir zwei verschiedenen Ansichtsvorlagen – "Edit.aspx" und "Create.aspx" –, um unsere Dinner-Formular-Benutzeroberfläche anzuzeigen. Ein sofortvergleich davon visual hervorgehoben werden wie ähnlich. Im folgenden sehen, wie die erstellen-Form aussieht:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Und was das Formular "Bearbeiten" sieht wie folgt aus:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Es ist nicht so großen Unterschied? Außer den Titel und die Header-Text sind die Steuerelemente des Formulars Layout und die Eingabe identisch.

Wenn wir die "Edit.aspx" und "Create.aspx" Ansichtsvorlagen öffnen, die gefunden werden, die enthalten sie identische Formular Layout und die Eingabe-Steuerelement-Code. Diese Duplizierung bedeutet, dass wir letztendlich Änderungen vornehmen, zweimal jedes Mal, wenn wir einführen, oder ändern Sie eine neue Eigenschaft von Dinner - handelt es sich nicht gut.

### <a name="using-partial-view-templates"></a>Mithilfe von Teilansichten, Vorlagen

ASP.NET MVC unterstützt die Möglichkeit, "Teilansicht" Vorlagen zu definieren, die verwendet werden können, um Anzeigen der Renderinglogik für einen untergeordneten Teil einer Seite zu kapseln. "Teilansichten" bieten eine gute Möglichkeit zum Rendering der ansichtslogik einmal definieren, und klicken Sie dann erneut verwenden sie an mehreren Stellen innerhalb einer Anwendung.

Um "DRY-Up" unsere Edit.aspx "und" Create.aspx Ansicht Vorlage Datenduplizierung zu unterstützen, können wir eine Teilansicht-Vorlage, die mit dem Namen "DinnerForm.ascx", die das Layout von Datenformularen und Eingabeelementen sowohl kapselt erstellen. Wir erreichen dies, indem mit der rechten Maustaste auf unser/Views/Dinners-Verzeichnis, und wählen die "Add -&gt;Ansicht" Menübefehl:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt. Den Namen der neuen Ansicht, wir möchten "DinnerForm" zu erstellen, aktivieren Sie das Kontrollkästchen "Eine Teilansicht erstellen" im Dialogfeld und anzugeben, dass es eine DinnerFormViewModel-Klasse übergeben wird:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, wird Visual Studio eine neue "DinnerForm.ascx" ansichtsvorlage für uns innerhalb des Verzeichnisses "\Views\Dinners" erstellt.

Wir können dann kopieren und Einfügen des Formularlayouts von doppelten / Steuercode aus unserer Edit.aspx/ Create.aspx Ansichtsvorlagen in unserer neuen "DinnerForm.ascx" Teilansicht Vorlage eingegeben:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Wir können dann aktualisieren, dass unsere Ansichtsvorlagen bearbeiten und erstellen, zum Aufrufen der DinnerForm partielle Vorlage und zum Beseitigen der Duplizierung des Formulars. Wir können dies vom aufrufenden Html.RenderPartial("DinnerForm") in unserer Ansichtsvorlagen durchführen:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Sie können den Pfad der gewünschte beim Aufrufen von Html.RenderPartial partielle Vorlage explizit qualifizieren (z. B.: ~ Views/Dinners/DinnerForm.ascx "). In unserem Code oben jedoch nutzen das auf Konventionen basierendes Muster in ASP.NET MVC, und wir einfach "DinnerForm" als Namen für die partielle zum Rendern angeben. Wenn wir dies tun sucht ASP.NET MVC zuerst in das konventionsbasierte Ansichtenverzeichnis ("dinnerscontroller" / Views/Dinners wäre). Wenn sie nicht die partielle Vorlage findet sucht es es klicken Sie dann dafür im Verzeichnis /Views/Shared.

Wenn nur der Name der Teilansicht Html.RenderPartial() aufgerufen wird, wird ASP.NET MVC die gleichen Modell und "ViewData" Dictionary-Objekte, die von der aufrufenden ansichtsvorlage verwendet an die Teilansicht übergeben. Es gibt auch überlastete Versionen der Html.RenderPartial(), mit denen Sie eine alternative Modellobjekt und/oder ViewData-Wörterbuch für die Teilansicht verwendet übergeben. Dies ist nützlich für Szenarien, in dem Sie nur eine Teilmenge des vollständigen Modells/ViewModels übergeben möchten.

| **Seite-Thema: Warum &lt;%%&gt; anstelle von &lt;% = %&gt;?** |
| --- |
| Einer der geringfügige Vorteile, die Sie haben, mit dem obigen Code bemerkt vielleicht wird die Verwendung von einer &lt;%%&gt; anstelle von Sperren ein &lt;% = %&gt; blockieren, wenn Html.RenderPartial() aufrufen. &lt;% = %&gt; Blöcke in ASP.NET anzugeben, dass ein Entwickler zum Rendern eines angegebenen Werts möchte (z. B.: &lt;% = "Hello" %&gt; würde "Hello" Rendern). &lt;%%&gt; Blöcke stattdessen zeigen an, dass der Entwickler kann Code ausgeführt werden, dass eine Ausgabe, die darin enthaltenen gerendert explizit ausgeführt werden muss (z. B.: &lt;Response.Write("Hello") %&gt;. Der Grund wir verwenden eine &lt;%%&gt; Block mit unserem Html.RenderPartial Code oben ist, da die Html.RenderPartial()-Methode gibt keine Zeichenfolge zurück, und stattdessen gibt die Inhalte direkt an die aufrufende ansichtsvorlage den Ausgabestream. Dies wird zur Verbesserung der Leistung Effizienz, und hierfür ist es erforderlich ist, erstellen ein (potenziell sehr großen) temporäre Zeichenfolgenobjekt, das verhindert. Dies verringert die speicherauslastung und verbessert den Durchsatz der gesamten Anwendung. Ein häufiger Fehler, die beim Verwenden von Html.RenderPartial() vergessen besteht, fügen Sie ein Semikolon am Ende des Aufrufs, wenn es innerhalb einer &lt;%%&gt; Block. Beispielsweise wird dieser Code verursacht einen Compilerfehler: &lt;Html.RenderPartial("DinnerForm") %&gt; müssen Sie stattdessen schreiben: &lt;% Html.RenderPartial("DinnerForm"); %&gt; Grund hierfür ist, &lt;%%&gt; Blöcke sind eigenständige codeanweisungen, und wenn der c#-Code mithilfe von Anweisungen mit einem Semikolon beendet werden müssen. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Mithilfe der Teilansicht-Vorlagen, um Code zu verdeutlichen.

Wir haben die "DinnerForm" Teilansicht Vorlage duplizieren Ansicht Renderinglogik an mehreren Orten zu vermeiden. Dies ist der häufigste Grund für die Teilansicht Vorlagen erstellen.

Manchmal ist es immer noch sinnvoll, Teilansichten zu erstellen, selbst wenn sie nur an einer zentralen Stelle aufgerufen werden. Sehr kompliziert Ansichtsvorlagen können häufig viel einfacher zu lesen, wenn ihre Ansicht Renderinglogik extrahiert und in eine partitionierte oder mehr auch mit der Bezeichnung teilweise Vorlagen werden.

Betrachten Sie beispielsweise den folgenden Codeausschnitt aus der Datei Site.master in unserem Projekt (die wir kurz betrachten werden wird). Der Code ist recht einfach, lesen teilweise daran, dass die Logik zum Anzeigen einer Anmeldung/Abmeldung link oben – rechten Rand des Bildschirms in die partielle "LogOnUserControl" gekapselt ist:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Wenn Sie sich die erste verwechselt finden versuchen, sich mit das Markup der html-Code in eine ansichtsvorlage, überlegen Sie, ob es wäre klarer, wenn einige davon extrahiert und in gut benannt Teilansichten umgestaltet wurde.

### <a name="master-pages"></a>Masterseiten

Zusätzlich zur Unterstützung von Teilansichten, unterstützt ASP.NET MVC auch die Möglichkeit, "Masterseite" Vorlagen erstellen, die verwendet werden kann, um das allgemeine Layout und die obersten Ebene HTML-Code von einem Standort zu definieren. Die Inhalte Platzhalter, die Steuerelemente klicken Sie dann auf die Masterseite ersetzbare Bereiche zu identifizieren, die außer Kraft gesetzt werden können oder die "eingetragen" von Sichten hinzugefügt werden können. Dies bietet es sich um eine sehr effiziente (und DRY) Möglichkeit, um ein gebräuchliches Layout für eine Anwendung zu übernehmen.

Standardmäßig verfügen neue ASP.NET MVC-Projekte eine Masterseitenvorlage automatisch hinzugefügt werden. Diese Masterseite heißt "Site.master" und befindet sich im Ordner \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Die standardmäßige Site.master-Datei sieht wie folgt aus. Den äußeren HTML-Code des Standorts, zusammen mit einem Menü für die Navigation oben definiert. Es enthält zwei ersetzbare Inhaltsplatzhalter-Steuerelemente: eines für den Titel und die andere für die, in dem der primäre Inhalt einer Seite ersetzt werden soll:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Alle Vorlagen anzeigen, die wir für die NerdDinner-Anwendung ("List", "Details", "Bearbeiten", "Erstellen", "NotFound" usw.) erstellt haben wurde basierend auf dieser Vorlage Site.master. Dies wird angezeigt, über das "MasterPageFile"-Attribut, das standardmäßig, an den Anfang hinzugefügt wurde &lt;% @ Page %&gt; Anweisung, wenn wir die Ansichten, die mithilfe des Dialogfelds "Ansicht hinzufügen" erstellt:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Dies bedeutet, dass wir können den Inhalt Site.master ändern, und haben die Änderungen automatisch angewendet und werden verwendet, wenn wir unsere Vorlagen Ansicht zu rendern.

Unsere Site.master der Headerabschnitt aktualisieren wir damit, dass der Header der Anwendung "NerdDinner" anstelle von "Meine MVC-Anwendung" ist. Außerdem aktualisieren wir unsere Navigationsmenü, damit die erste Registerkarte ist "Finden Sie ein Dinner" (von des HomeController Index() Aktionsmethode behandelt), und fügen Sie eine neue Registerkarte namens "Host ein Dinner" (von der "dinnerscontroller" Create() Aktionsmethode behandelt):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Wenn wir die Datei Site.master Aktualisierung, und speichern Änderungen mit unserem Browser sehen wir unsere Header für alle Sichten in unserer Anwendung. Zum Beispiel:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Und mit der *"/ dinners" / Edit / [Id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Nächster Schritt

Teilansichten und Masterseiten bieten sehr flexible Optionen, mit die Sie problemlos Ansichten organisieren können. Sie werden feststellen, dass sie helfen, die Sie vermeiden, duplizieren die Ansicht Inhalt / code, und stellen Ihre Vorlagen anzeigen einfacher zu lesen und zu verwalten.

Lassen Sie uns nun noch einmal das Angebot-Szenario, das wir zuvor erstellt und skalierbare pagingunterstützung zu aktivieren.

> [!div class="step-by-step"]
> [Zurück](use-viewdata-and-implement-viewmodel-classes.md)
> [Weiter](implement-efficient-data-paging.md)
