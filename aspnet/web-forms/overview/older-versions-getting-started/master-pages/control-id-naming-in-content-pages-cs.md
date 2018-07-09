---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Steuern der-IDs auf Inhaltsseiten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Veranschaulicht, wie ContentPlaceHolder-Steuerelemente als Benennungscontainer dienen, und stellen Sie daher programmgesteuert arbeiten mit einem Steuerelement (über FindConrol) schwierig...
ms.author: aspnetcontent
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b7ae15095b972a456f274e1326221d168f323256
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805874"
---
<a name="control-id-naming-in-content-pages-c"></a>Steuerelement-ID-Namen auf Inhaltsseiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Veranschaulicht, wie ContentPlaceHolder-Steuerelemente als Benennungscontainer dienen, und stellen Sie daher programmgesteuert ein Steuerelement, die schwierig (über FindConrol) verwenden. Prüft, ob dieses Problem und problemumgehungen. Außerdem wird erläutert, wie den sich ergebenden ClientID-Wert programmgesteuert zugreifen.


## <a name="introduction"></a>Einführung

Einschließen von allen ASP.NET-Serversteuerelementen einen `ID` -Eigenschaft, die eine eindeutige Identifikation des Steuerelements und ist die Möglichkeit, die mit dem das Steuerelement programmgesteuert in der CodeBehind-Klasse erfolgt. Auf ähnliche Weise können die Elemente in einem HTML-Dokument enthalten eine `id` -Attribut, das das Element eindeutig identifiziert; diese `id` Werte werden häufig in Client-seitige Skript verwendet, um programmgesteuert auf ein bestimmtes HTML-Element verweisen. Angesichts Sie möglicherweise davon ausgehen, die beim Rendern eines ASP.NET-Serversteuerelements in HTML-Code, dessen `ID` Wert wird verwendet, als die `id` Wert des gerenderten HTML-Elements. Dies ist nicht unbedingt der Fall, da unter bestimmten Umständen eine einzelne, Steuern mit einem einzelnen `ID` Wert möglicherweise mehrere Male in dem gerenderten Markup angezeigt. Betrachten Sie ein GridView-Steuerelement, das ein TemplateField mit ein Label-Steuerelement mit enthält ein `ID` Wert von ProductName. Wenn die GridView mit der Datenquelle zur Laufzeit gebunden ist, wird diese Bezeichnung einmal für jede GridView-Zeile wiederholt. Jede Bezeichnung muss einen eindeutigen gerendert `id` Wert.

Um solche Szenarien zu behandeln, können ASP.NET bestimmte Steuerelemente angezeigt werden, wie das Benennen von Containern. Einem Namenscontainer enthalten dient als neuer `ID` Namespace. Alle Steuerelemente, die innerhalb der Namenscontainer haben ihre gerenderten `id` Wert mit dem Präfix die `ID` des Containersteuerelements Benennungskonventionen. Z. B. die `GridView` und `GridViewRow` Klassen sind beide Benennen von Containern. Infolgedessen ein Label-Steuerelement, das in ein GridView TemplateField mit definierten `ID` ProductName erhält eines gerenderten `id` Wert `GridViewID_GridViewRowID_ProductName`. Da *GridViewRowID* ist eindeutig für jede GridView-Zeile, die resultierende `id` Werte eindeutig sind.

> [!NOTE]
> Die [ `INamingContainer` Schnittstelle](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) wird verwendet, um anzugeben, dass eine bestimmte ASP.NET-Serversteuerelement als Benennungscontainer funktionieren sollte. Die `INamingContainer` Schnittstelle ausgeschrieben nicht auf alle Methoden, die das Steuerelement implementieren, müssen; stattdessen wird es als Marker verwendet. Beim Generieren der gerenderten Markups, wenn ein Steuerelement diese Schnittstelle implementiert, klicken Sie dann das ASP.NET-Modul automatisch das Präfix der `ID` Wert, der ihm ungeordnete gerendert `id` Attributwerte. Dieser Prozess wird in Schritt2 ausführlicher erläutert.


Benennen von Containern ändern nicht nur das gerenderte `id` Attribut wirkt sich jedoch auch wie das Steuerelement programmgesteuert aus der ASP.NET-Seite Code-Behind-Klasse verwiesen werden kann. Die `FindControl("controlID")` Methode wird häufig verwendet, um auf ein Steuerelement programmgesteuert zu verweisen. Allerdings `FindControl` nicht über das Benennen von Containern durchdringen. Infolgedessen können nicht direkt verwenden Sie die `Page.FindControl` Methode, um die Steuerelemente in einer GridView-Ansicht oder andere Benennungscontainer verweisen.

Wie Sie mit Ihrer Vermutung haben können, werden Masterseiten und ContentPlaceHolder-Steuerelemente sowohl implementiert wie das Benennen von Containern. In diesem Tutorial untersuchen wir wie master Pages Auswirkungen-HTML-Element `id` Werte und Methoden zum programmgesteuerten Websteuerelemente in einer Inhaltsseite mit Verweisen auf `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Schritt 1: Hinzufügen einer neuen ASP.NET-Seite

Um die in diesem Tutorial beschriebenen Konzepte zu veranschaulichen, fügen Sie eine neue ASP.NET-Seite auf unserer Website. Erstellen Sie eine neue Seite mit dem Namen `IDIssues.aspx` im Stammordner, binden an die `Site.master` Masterseite.


![Fügen Sie den Inhalt Seite IDIssues.aspx in den Stammordner](control-id-naming-in-content-pages-cs/_static/image1.png)

**Abbildung 01**: Fügen Sie die Seite Inhalte `IDIssues.aspx` in den Stammordner


Visual Studio erstellt automatisch ein ContentControl-Element für jede der Masterseite vier ContentPlaceHolder-Steuerelemente. Wie erwähnt in der [ *mehrere ContentPlaceHolder-Steuerelemente und Standardinhalt* ](multiple-contentplaceholders-and-default-content-cs.md) Tutorial, wenn ein ContentControl-Element nicht vorhanden ist wird stattdessen die Masterseite ContentPlaceHolder Standardinhalt ausgegeben. Da die `QuickLoginUI` und `LeftColumnContent` ContentPlaceHolder-Steuerelemente enthalten die Standard-Markup für diese Seite, fahren Sie fort, und entfernen Sie die entsprechenden Inhaltssteuerelemente aus `IDIssues.aspx`. An diesem Punkt sollte die Inhaltsseite deklaratives Markup wie folgt aussehen:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

In der [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm erstellten eine benutzerdefinierten Basisseite-Klasse (`BasePage`) der Titel der Seite, die automatisch konfiguriert, wenn es sich handelt nicht explizit festgelegt wurde. Für die `IDIssues.aspx` Seite, um diese Funktionalität zu nutzen, zu der Seite Code-Behind-Klasse muss leiten Sie von der `BasePage` Klasse (anstelle von `System.Web.UI.Page`). Ändern Sie die Definition für die Code-Behind-Klasse, damit sie wie folgt aussieht:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Aktualisieren Sie abschließend die `Web.sitemap` hinzu, um einen Eintrag für diese neue Lektion einzubeziehen. Hinzufügen einer `<siteMapNode>` Element, und legen dessen `title` und `url` "Steuerelement-ID-Benennungsprobleme" Attribute und `~/IDIssues.aspx`bzw. Treffen Sie diese Ergänzung Ihrer `Web.sitemap` Markup für die Datei sollte etwa wie folgt aussehen:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Wie Abbildung 2 veranschaulicht, in den neuen Standort-Zuordnungseintrag `Web.sitemap` sofort in den Lektionen-Abschnitt in der linken Spalte wiedergegeben wird.


![Abschnitt Lektionen enthält nun einen Link zum &quot;Steuerelement-ID, die Benennung von Problemen&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Abbildung 02**: Abschnitt Lektionen enthält nun einen Link zu "Probleme"Steuerelement-IDs "


## <a name="step-2-examining-the-renderedidchanges"></a>Schritt 2: Untersuchen der gerenderten`ID`Änderungen

Um die Änderungen der ASP.NET besser zu verstehen Engine macht, für das gerenderte `id` Werte des Servers steuert, fügen Sie einige Web-Steuerelemente, die `IDIssues.aspx` Seite aus, und zeigen Sie dann das gerenderte Markup, das an den Browser gesendet. Insbesondere Geben Sie den Text "Bitte geben Sie Ihr Alter:" gefolgt von der ein TextBox-Steuerelement. Weiter unten auf der Seite Hinzufügen eines Websteuerelements für Schaltfläche und ein Label-Steuerelement. Festlegen des Textfelds `ID` und `Columns` Eigenschaften `Age` und 3 bzw. Legen Sie die `Text` und `ID` Eigenschaften auf "Submit" und `SubmitButton`. Löschen Sie der Bezeichnung des `Text` Eigenschaft, und legen dessen `ID` zu `Results`.

An diesem Punkt sollte Ihre Inhaltssteuerelement deklaratives Markup etwa wie folgt aussehen:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Abbildung 3 zeigt die Seite, wenn Sie über Visual Studio-Designer angezeigt.


[![Die Seite enthält drei Websteuerelemente: ein Textfeld, Schaltfläche, und Bezeichnung](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Abbildung 03**: die Seite enthält drei Web Controls: ein Textfeld, einer Schaltfläche und einer Bezeichnung ([klicken Sie, um das Bild in voller Größe anzeigen](control-id-naming-in-content-pages-cs/_static/image5.png))


Besuchen Sie die Seite über einen Browser, und zeigen Sie die HTML-Quelle. Als Markup unten zeigt die `id` Werte der HTML-Elemente für die Textfeld, Schaltfläche und Label-Webserver-Steuerelemente sind eine Kombination der `ID` Werte von Websteuerelementen und der `ID` Werte das Benennen von Containern auf der Seite.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Wie weiter oben in diesem Tutorial erwähnt, werden sowohl die Masterseite als auch die ContentPlaceHolder-Steuerelemente wie das Benennen von Containern dienen. Daher sowohl das gerenderte beitragen `ID` Werte von geschachtelten Steuerelementen. Nutzen des Textfelds `id` Attribut, z. B.: `ctl00_MainContent_Age`. Bedenken Sie, dass des TextBox-Steuerelements `ID` Wert war `Age`. Dies ist mit dem Präfix des Steuerelements die ContentPlaceHolder `ID` Wert `MainContent`. Darüber hinaus wird dieser Wert mit der Masterseite vorangestellt `ID` Wert `ctl00`. Das Endergebnis ist eine `id` Attributwert bestehend aus dem `ID` Werte von der Masterseite, das ContentPlaceHolder-Steuerelement und das Textfeld selbst.

Abbildung 4 zeigt dieses Verhalten. Um zu bestimmen, das gerenderte `id` von der `Age` TextBox, beginnen Sie mit der `ID` Wert des TextBox-Steuerelement, `Age`. Als Nächstes arbeiten der Steuerelementhierarchie nach oben. Auf jede Benennungscontainer (diese Knoten durch eine orangefarbene Farbe), Präfix der aktuellen gerendert `id` mit des Benennungscontainers `id`.


![Die Rendered-Id-Attribute werden basierend auf der ID-Werte der Naming-Container](control-id-naming-in-content-pages-cs/_static/image6.png)

**Abbildung 04**: der gerenderten `id` Attribute werden basierend auf den `ID` Werte Benennen von Containern


> [!NOTE]
> Wie wir erläutert, die `ctl00` Teil des gerenderten `id` Attribut bildet die `ID` Wert der Masterseite, aber Sie vielleicht wie dieser `ID` Wert stammt, zu. Wir ihn nicht in unserer master oder Content-Seite an einer beliebigen Stelle angeben. Die meisten Steuerelemente auf einer ASP.NET-Seite werden explizit über deklaratives Markup der Seite hinzugefügt. Die `MainContent` ContentPlaceHolder-Steuerelement wurde explizit im Markup angegeben `Site.master`; die `Age` Textfeld definiert wurde `IDIssues.aspx`Markup. Wir können angeben, die `ID` Werte für diese Arten von Steuerelementen, die über das Eigenschaftenfenster oder über die deklarative Syntax. Andere Steuerelemente, wie die Masterseite selbst sind nicht im deklarativen Markup definiert. Daher ihre `ID` Werte automatisch für uns generiert werden müssen. Im ASP.NET-Engine wird die `ID` Werte zur Laufzeit für die Steuerelemente, deren IDs nicht explizit festgelegt wurde müssen. Er verwendet das Benennungsmuster `ctlXX`, wobei *XX* ist ein zunehmenden Integer-Wert.


Da die Masterseite selbst dient als Benennungscontainer, die Web-Steuerelemente, die in der Masterseite definierte auch wurden geändert gerenderten `id` Attributwerte. Z. B. die `DisplayDate` Bezeichnung, die wir, auf die Masterseite in hinzugefügt der [ *erstellen einen websiteweiten Layouts mit Masterseiten* ](creating-a-site-wide-layout-using-master-pages-cs.md) Tutorial wurde Folgendes Markup Rendern:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Beachten Sie, dass die `id` Attribut enthält sowohl die der Masterseite `ID` Wert (`ctl00`) und die `ID` Wert, der das Label-Steuerelement (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Schritt 3: Programmgesteuertes Verweisen Websteuerelemente über`FindControl`

Jede ASP.NET-Serversteuerelement enthält ein `FindControl("controlID")` -Methode, die das Steuerelement die untergeordneten Elemente für ein Steuerelement mit dem Namen durchsucht *ControlID*. Wenn z. B. ein Steuerelement gefunden wird, wird Sie zurückgegeben; Wenn kein entsprechendes Steuerelement gefunden wird, `FindControl` gibt `null`.

`FindControl` ist nützlich in Szenarien, in dem müssen Sie ein Steuerelement zuzugreifen, aber Sie haben keine direkten Verweis auf sie. Beim Arbeiten mit Web-Steuerelemente wie GridView, z. B. die Steuerelemente in den GridView Felder in der deklarativen Syntax einmal definiert werden, aber zur Laufzeit eine Instanz des Steuerelements für jede GridView-Zeile erstellt wird. Daher die Steuerelemente zur Laufzeit generiert vorhanden, aber wir haben keinen direkten Verweis aus der CodeBehind-Klasse verfügbar. Daher müssen wir verwenden `FindControl` programmgesteuert mit einem bestimmten Steuerelement innerhalb von GridView Feldern zu arbeiten. (Weitere Informationen zur Verwendung von `FindControl` den Zugriff auf die Steuerelemente innerhalb eines Daten-Websteuerelements Vorlagen finden Sie unter [benutzerdefinierte Formatierung basierend auf Daten](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Dieses Szenario tritt auf, wenn es sich bei Websteuerelementen dynamisch zu einem Web Form hinzufügen, ein Thema ausführlicher [erstellen dynamischer Daten Benutzeroberflächen zur Dateneingabe](https://msdn.microsoft.com/library/aa479330.aspx).

Zur Veranschaulichung der Verwendung der `FindControl` Methode zum Suchen nach Steuerelementen in einer Inhaltsseite, erstellen Sie einen Ereignishandler für die `SubmitButton`des `Click` Ereignis. Fügen Sie im Ereignishandler, den folgenden Code, der programmgesteuert verweist die `Age` Textfeld und `Results` Bezeichnung mit der `FindControl` Methode und anschließend wird eine Meldung in `Results` basierend auf der Eingabe des Benutzers.

> [!NOTE]
> Natürlich ist es möglich, wir müssen nicht verwenden `FindControl` auf die Bezeichnung und TextBox-Steuerelemente in diesem Beispiel verweisen. Wir könnten verweisen sie direkt über ihre `ID` Eigenschaftswerte. Ich verwende `FindControl` hier, um zu veranschaulichen, was geschieht, wenn mit `FindControl` von einer Inhaltsseite.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Während Sie die Syntax zum Aufrufen der `FindControl` Methode unterscheidet sich geringfügig, in den ersten beiden Zeilen der `SubmitButton_Click`, semantisch gleichwertig. Beachten Sie, die allen ASP.NET-Serversteuerelementen enthalten eine `FindControl` Methode. Dies schließt die `Page` Klasse, die von der alle ASP.NET-STEUERELEMENTE müssen Code-Behind-Klassen von abgeleitet werden. Aus diesem Grund Aufrufen `FindControl("controlID")` entspricht dem Aufruf `Page.FindControl("controlID")`, sofern Sie noch nicht außer Kraft gesetzt den `FindControl` -Methode in der CodeBehind-Klasse oder in einer benutzerdefinierten Basisklasse.

Geben Sie diesen Code, besuchen Sie die `IDIssues.aspx` Seite über einen Webbrowser, geben Sie Ihr Alter, und klicken Sie auf die Schaltfläche "Absenden". Nach dem Klicken auf die Schaltfläche "Absenden" eine `NullReferenceException` ausgelöst wird (siehe Abbildung 5).


[![Es wird eine "NullReferenceException" ausgelöst.](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Abbildung 05**: ein `NullReferenceException` ausgelöst ([klicken Sie, um das Bild in voller Größe anzeigen](control-id-naming-in-content-pages-cs/_static/image9.png))


Wenn Sie einen Haltepunkt, in Festlegen der `SubmitButton_Click` -Ereignishandler Sie sehen, dass beide Aufrufe von `FindControl` Zurückgeben einer `null` Wert. Die `NullReferenceException` wird ausgelöst, wenn wir versuchen, den Zugriff auf die `Age` Textfeldss `Text` Eigenschaft.

Das Problem besteht darin, die `Control.FindControl` nur durchsucht *Steuerelement*der untergeordneten Elemente, die *im gleichen Benennungscontainer*. Da die Masterseite einer neuen Benennungscontainer, einen Aufruf von bildet `Page.FindControl("controlID")` bedeutendste nie auf das Objekt für die Masterseite `ctl00`. (Siehe Abbildung 4 an die Steuerelementhierarchie, die zeigt die `Page` -Objekt als das übergeordnete Element des Objekts Masterseite `ctl00`.) Aus diesem Grund die `Results` Bezeichnung und `Age` Textfeld wurden nicht gefunden und `ResultsLabel` und `AgeTextBox` werden Werte zugewiesen `null`.

Es gibt zwei problemumgehungen auf diese Herausforderung: können die diagrammdarstellung, einem Benennungscontainer zu einem Zeitpunkt, an das entsprechende Steuerelement; oder wir erstellen unsere eigene `FindControl` -Methode, die bedeutendste Benennen von Containern. Betrachten wir jede dieser Optionen.

### <a name="drilling-into-the-appropriate-naming-container"></a>Drilldown in den entsprechenden Namenscontainer

Mit `FindControl` zu verweisen die `Results` Bezeichnung oder `Age` Textfeld müssen wir Aufrufen `FindControl` von einem übergeordneten Steuerelement in der gleichen Namenscontainer. Wie Abbildung 4 gezeigt, die `MainContent` ContentPlaceHolder-Steuerelement ist das einzige Vorgängerelement `Results` oder `Age` , der sich im selben Benennungscontainer. Das heißt, Aufrufen der `FindControl` Methode aus der `MainContent` -Steuerelement, wie im folgenden Codeausschnitt gezeigt ordnungsgemäß gibt einen Verweis auf die `Results` oder `Age` Steuerelemente.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Es kann jedoch nicht arbeiten mit der `MainContent` ContentPlaceHolder aus unserer Inhaltsseite Code-Behind-Klasse, die die oben aufgeführten Syntax verwenden, da die ContentPlaceHolder in die Masterseite definiert ist. Stattdessen haben wir mit `FindControl` zum Abrufen eines Verweises auf `MainContent`. Ersetzen Sie den Code in die `SubmitButton_Click` -Ereignishandler durch die folgenden Änderungen:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Wenn Sie auf die Seite über einen Browser besuchen, geben Sie Ihr Alter, und klicken Sie auf die Schaltfläche "Submit" eine `NullReferenceException` ausgelöst wird. Wenn Sie einen Haltepunkt, in Festlegen der `SubmitButton_Click` -Ereignishandler Sie sehen, dass diese Ausnahme tritt auf, beim Aufrufen der `MainContent` des Objekts `FindControl` Methode. Die `MainContent` Objekt `null` da die `FindControl` Methode kann ein Objekt mit dem Namen "MainContent" nicht finden. Der Grund dafür ist identisch mit der `Results` Bezeichnung und `Age` TextBox-Steuerelemente: `FindControl` beginnt die Suche am Anfang der Hierarchie der Steuerelemente und ist nicht durchdringen Benennen von Containern, aber die `MainContent` ContentPlaceHolder ist innerhalb der Masterseite, die einem Namenscontainer enthalten ist.

Bevor wir verwenden können, `FindControl` zum Abrufen eines Verweises auf `MainContent`, wir zunächst einen Verweis auf die Masterseite-Steuerelement. Nachdem wir einen Verweis auf die Masterseite haben wir können einen Verweis auf die `MainContent` ContentPlaceHolder über `FindControl` und von dort Verweise auf die `Results` Bezeichnung und `Age` Textfeld (in diesem Fall durch die Verwendung von `FindControl`). Aber wie bekommen wir einen Verweis auf die Masterseite? Durch Überprüfen der `id` Attribute in dem gerenderten Markup ist es offensichtlich, die der Masterseite `ID` Wert `ctl00`. Wir könnten aus diesem Grund verwenden `Page.FindControl("ctl00")` rufen Sie einen Verweis auf die Masterseite zum Abrufen eines Verweises auf das Objekt dann mit `MainContent`und so weiter. Der folgende Codeausschnitt zeigt diese Logik:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Dieser Code sicherlich funktionieren, es wird davon ausgegangen, die die Masterseite automatisch generierte `ID` immer `ctl00`. Es ist nie eine gute Idee, Annahmen über die automatisch generierten Werte.

Glücklicherweise ist ein Verweis auf die Masterseite über die `Page` Klasse `Master` Eigenschaft. Aus diesem Grund statt verwenden zu müssen `FindControl("ctl00")` um einen Verweis der Masterseite zu erhalten, um Zugriff auf die `MainContent` ContentPlaceHolder, wir können stattdessen `Page.Master.FindControl("MainContent")`. Update der `SubmitButton_Click` -Ereignishandler durch den folgenden Code:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Dieses Mal auf der Seite über einen Browser, Ihr Alter eingeben, und klicken auf die Schaltfläche "Absenden" zeigt die Meldung in die `Results` bezeichnen, wie erwartet.


[![Das Alter des Benutzers wird in der Bezeichnung angezeigt.](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Abbildung 06**: der Benutzer-Agent wird angezeigt, in der Bezeichnung ([klicken Sie, um das Bild in voller Größe anzeigen](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rekursiv durchsucht Benennen von Containern

Der Grund dafür im vorherigen Codebeispiel, das auf die verwiesen wird die `MainContent` ContentPlaceHolder-Steuerelement von der Masterseite, und klicken Sie dann die `Results` Bezeichnung und `Age` TextBox-Steuerelemente aus `MainContent`, liegt daran, dass die `Control.FindControl` Methode durchsucht nur in *Steuerelement*Benennungscontainer des. Mit `FindControl` bleiben innerhalb der Namenscontainer ist in den meisten Szenarien sinnvoll, da es sich bei zwei Steuerelemente in zwei verschiedenen Benennen von Containern gleich möglicherweise `ID` Werte. Betrachten Sie den Fall einer GridView-Ansicht, die ein Label-Steuerelement mit dem Namen definiert `ProductName` innerhalb der die von TemplateFields. Wenn die Daten an die GridView zur Laufzeit gebunden ist eine `ProductName` Bezeichnung wird für jede GridView-Zeile erstellt. Wenn `FindControl` durchsucht, bis alle Namen der Container, und wir aufgerufen `Page.FindControl("ProductName")`, welche Bezeichnung-Instanz sollte die `FindControl` zurückgeben? Die `ProductName` Bezeichnung in der ersten Zeile der GridView? Die Version in der letzten Zeile?

Daher `Control.FindControl` suchen nur *Steuerelement*Benennung des Containers in den meisten Fällen sinnvoll. Aber es andere Situationen, z. B. die für uns gibt, dass eine eindeutige `ID` in allen Benennen von Containern und zu vermeiden, dass auf jede Benennungscontainer in der Hierarchie der Steuerelemente auf einem Steuerelement sorgfältig verweisen möchten. Mit einem `FindControl` Variante, die rekursiv durchsucht alle Benennen von Containern können erkennen, zu. .NET Framework umfasst leider keine solche Methode.

Die gute Nachricht ist, dass wir unseren eigenen erstellen können `FindControl` Methode, der rekursiv durchsucht alle Benennen von Containern. Verwenden Sie in der Tat *Erweiterungsmethoden* wir speichern können, auf eine `FindControlRecursive` Methode, um die `Control` Klasse zur Ergänzung vorhandener `FindControl` Methode.

> [!NOTE]
> Erweiterungsmethoden sind eine Funktion, die sich neu zu c# 3.0 und Visual Basic 9, d. h. die Sprachen, die mit .NET Framework Version 3.5 und Visual Studio 2008 geliefert. Kurz gesagt, ermöglichen Erweiterungsmethoden für einen Entwickler zum Erstellen einer neuen Methode für einen vorhandenen Klassentyp über eine spezielle Syntax. Weitere Informationen zu diesem Feature hilfreich finden Sie in meinem Artikel [Base Typ erweitern-Funktion mit Erweiterungsmethoden](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Um die Erweiterungsmethode zu erstellen, fügen Sie eine neue Datei, die `App_Code` Ordner mit dem Namen `PageExtensionMethods.cs`. Fügen Sie eine Erweiterungsmethode namens `FindControlRecursive` , akzeptiert als Eingabe eine `string` Parameter mit dem Namen `controlID`. Für Erweiterungsmethoden ordnungsgemäß funktioniert, ist es wichtig, dass die Klasse selbst und seine Erweiterungsmethoden gekennzeichnet werden `static`. Darüber hinaus müssen alle Erweiterungsmethoden akzeptieren, wie der erste Parameter ein Objekt des Typs, für die die Erweiterungsmethode gilt, und dieser Eingabeparameter mit dem Schlüsselwort vorangestellt werden muss `this`.

Fügen Sie den folgenden Code der `PageExtensionMethods.cs` Klassendatei mit dieser Klasse zu definieren und die `FindControlRecursive` Erweiterungsmethode:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Mit diesem Code vorhanden, zurück zu den `IDIssues.aspx` , Code-Behind-Klasse und kommentieren Sie die aktuelle Seite `FindControl` Methodenaufrufe. Ersetzen Sie sie durch Aufrufen von `Page.FindControlRecursive("controlID")`. Zu Erweiterungsmethoden praktisch ist, dass sie direkt in der Dropdown-Listen von IntelliSense angezeigt werden. Wie in Abbildung 7 dargestellt, wenn Sie Seite und drücken Sie dann den Zeitraum, der `FindControlRecursive` Methode befindet sich in der IntelliSense-Dropdownliste zusammen mit anderen `Control` -Klassenmethoden.


[![Erweiterungsmethoden sind in der IntelliSense-Dropdown-Elemente enthalten.](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Abbildung 07**: Erweiterungsmethoden befinden sich in der IntelliSense-Dropdown-Elemente ([klicken Sie, um das Bild in voller Größe anzeigen](control-id-naming-in-content-pages-cs/_static/image15.png))


Geben Sie den folgenden Code in die `SubmitButton_Click` -Ereignishandler und dann testen, indem Sie besuchen die Seite, Ihr Alter eingeben und auf die Schaltfläche "Absenden" klicken. Wie in Abbildung 6 gezeigt, ist die Ausgabe die Nachricht, "Sie sind Alter Jahre!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Da Erweiterungsmethoden noch nicht mit c# 3.0 und Visual Basic 9 sind, wenn Sie Visual Studio 2005 verwenden, können nicht Sie Erweiterungsmethoden verwenden. Stattdessen müssen Sie zum Implementieren der `FindControlRecursive` -Methode in eine Hilfsklasse. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) verfügt über ein Beispiel dafür in seinem Blogbeitrag von [Maser ASP.NET-Seiten und `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Schritt 4: Mit dem richtigen`id`-Attributwert im Client-seitige Skript

Wie in diesem Tutorial Einführung erwähnt, ein Websteuerelement gerenderten `id` Attribut wird häufig in Client-seitige Skript verwendet, um programmgesteuert auf ein bestimmtes HTML-Element verweisen. Der folgende JavaScript-Code verweist beispielsweise ein HTML-Element, durch die `id` und anschließend den Wert in ein modales Meldungsfeld angezeigt:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Beachten Sie, die in ASP.NET, die Seiten, enthalten keines Benennungscontainers, gerenderten HTML-Elements `id` Attribut entspricht des Websteuerelements `ID` -Eigenschaftswert. Aus diesem Grund ist es verlockend, hartcodieren in `id` Attributwerte in JavaScript-Code. D.h., wenn Sie wissen Sie zugreifen möchten die `Age` TextBox-Websteuerelements durch Client-seitige Skript, zu diesem Zweck über einen Aufruf an `document.getElementById("Age")`.

Das Problem bei diesem Ansatz ist, die die Verwendung von Masterseiten (oder anderen Containersteuerelementen Benennung), die gerenderte HTML `id` ist kein Synonym für des Websteuerelements `ID` Eigenschaft. Ihre erste Neigung möglicherweise finden Sie auf der Seite über einen Browser, und zeigen Sie die Quelle, um zu bestimmen, den tatsächlichen `id` Attribut. Sobald Sie, das gerenderte wissen `id` Wert, Sie können fügen Sie ihn in den Aufruf von `getElementById` auf das HTML-Element zuzugreifen, Sie mit über Client-seitige Skript arbeiten müssen. Dieser Ansatz ist nicht ideal, da bestimmte Änderungen an der Seite Hierarchie zu kontrollieren oder Änderungen an der `ID` Eigenschaften der Benennungskonvention Steuerelemente ändern, in der resultierenden `id` und grundlegende JavaScript-Code-Attribut.

Die gute Nachricht ist, die die `id` Attributwert, der gerendert werden kann zugegriffen werden, im serverseitigen Code über des Websteuerelements [ `ClientID` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Sie sollten diese Eigenschaft verwenden, um zu bestimmen, die `id` Attribut in Client-seitige Skript verwendet. Beispielsweise, um eine JavaScript-Funktion auf der Seite hinzufügen, aufgerufen wird, zeigt den Wert der die `Age` im Textfeld ein modales Meldungsfeld, fügen Sie folgenden Code, der `Page_Load` -Ereignishandler:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Der obige Code fügt den Wert des der `Age` ClientID-Eigenschaft des Textfelds an den JavaScript-Aufruf zum `getElementById`. Wenn Sie diese Seite über einen Browser und zeigen Sie die HTML-Quelle, finden Sie den folgenden JavaScript-Code:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Beachten Sie, dass wie die richtige `id` -Attributwert `ctl00_MainContent_Age`, wird im Aufruf von `getElementById`. Da dieser Wert zur Laufzeit berechnet wird, funktioniert es unabhängig von späteren Änderungen an der Steuerelementhierarchie der Seite.

> [!NOTE]
> Dieser JavaScript-Beispiel veranschaulicht lediglich eine JavaScript-Funktion hinzufügen, die ordnungsgemäß von einem Serversteuerelement gerenderte HTML-Elements verweist. Um diese Funktion verwenden, müssten Sie zum Erstellen von weiteren JavaScript-Code zum Aufrufen der Funktion, wenn das Dokument geladen wird oder eine bestimmte Benutzeraktion herausstellt. Weitere Informationen zu diesen und verwandten Themen, lesen Sie [arbeiten mit Client-seitige Skript](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Zusammenfassung

Bestimmte ASP.NET-Serversteuerelemente fungieren als Benennen von Containern, die wirkt sich auf das gerenderte `id` Attribut Werte ihrer untergeordneten Steuerelemente sowie den Bereich der Steuerelemente von canvassed der `FindControl` Methode. Im Hinblick auf die Masterseiten werden sowohl die master-Seite selbst und seine ContentPlaceHolder-Steuerelemente, die Container benennen. Daher müssen wir weiter einen etwas höheren Arbeitsaufwand programmgesteuert Steuerelemente innerhalb der Inhaltsseite mit Verweisen zu put `FindControl`. In diesem Tutorial wurde beschrieben, zwei Techniken: drilling in das ContentPlaceHolder-Steuerelement und ein Aufruf der `FindControl` Methode werden und für unseren eigenen `FindControl` Implementierung, der rekursiv durchsucht, bis alle Benennen von Containern.

Zusätzlich zu den serverseitiger Probleme Benennen von Containern eingeführt wird, im Hinblick auf die Web-Steuerelemente verweisen, gibt es auch clientseitige Probleme. In Ermangelung Benennen von Containern, das Websteuerelement des `ID` Eigenschaftswert und `id` Attributwert sind identisch. Aber durch das Hinzufügen des Benennungscontainers, das gerenderte `id` Attribut enthält sowohl die `ID` Werte für das Websteuerelement und die Benennungskonventionen Container in der Steuerelementhierarchie Versionsherkunft. Diese Benennungskonvention Probleme sind nicht, solange Sie des Websteuerelements `ClientID` Eigenschaft, um zu bestimmen, das gerenderte `id` -Attributwert in Ihrem Client-seitige Skript.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET-Masterseiten und `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Erstellen von Benutzeroberflächen zur Dateneingabe dynamische Daten](https://msdn.microsoft.com/library/aa479330.aspx)
- [Erweitern der Funktionalität der Basisklasse mit Erweiterungsmethoden](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Gewusst wie: Verweisen auf Inhalt von ASP.NET Master-Seite](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Unabhängig davon Seiten: Tipps, Tricks und fallen](http://www.odetocode.com/articles/450.aspx)
- [Arbeiten mit clientseitigen Skripts](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Zack Jones und Suchi Barnerjee. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](urls-in-master-pages-cs.md)
> [Weiter](interacting-with-the-master-page-from-the-content-page-cs.md)
