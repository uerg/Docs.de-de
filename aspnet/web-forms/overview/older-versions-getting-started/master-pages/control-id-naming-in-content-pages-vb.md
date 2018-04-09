---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: ID auf Inhaltsseiten (VB) Benennung steuern | Microsoft Docs
author: rick-anderson
description: Veranschaulicht, wie ContentPlaceHolder Steuerelemente als einem Benennungscontainer dienen, und stellen Sie daher programmgesteuert arbeiten mit einem Steuerelement schwierig (über FindConrol)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 288afbb6851e23de4725f9e6351ae12ccecefaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="control-id-naming-in-content-pages-vb"></a>Steuerelement-ID, die Benennung in Inhaltsseiten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Veranschaulicht, wie ContentPlaceHolder Steuerelemente als einem Benennungscontainer dienen, und stellen Sie daher programmgesteuert arbeiten mit einem Steuerelement schwierig (über FindConrol). Prüft, ob dieses Problem und problemumgehungen. Außerdem wird erläutert, wie den Ergebniswert ClientID programmgesteuert zugreifen.


## <a name="introduction"></a>Einführung

Alle ASP.NET-Serversteuerelemente enthalten eine `ID` -Eigenschaft, die eindeutig identifiziert wird das Steuerelement und dient dazu, mit dem das Steuerelement programmgesteuert in der CodeBehind-Klasse erfolgt. Auf ähnliche Weise sind die Elemente in einem HTML-Dokument ein `id` -Attribut, das das Element eindeutig; diese `id` Werte werden häufig in clientseitigem Skript verwendet, um programmgesteuert auf ein bestimmtes HTML-Element verweisen. Angesichts Sie möglicherweise angenommen, wenn ein ASP.NET-Serversteuerelement in HTML gerendert wird seine `ID` Wert dient als die `id` Wert des gerenderten HTML-Elements. Dies ist nicht unbedingt der Fall, da unter bestimmten Umständen zu eine einzelnen steuern mit einem einzelnen `ID` Wert möglicherweise mehrere Male in der gerenderten Markups angezeigt. Betrachten Sie ein GridView-Steuerelement, das ein TemplateField mit einer Bezeichnung Websteuerelement mit umfasst eine `ID` Wert `ProductName`. Wenn die GridView zu seiner Datenquelle zur Laufzeit gebunden ist, wird diese Bezeichnung für jede Zeile GridView einmal wiederholt. Jede Bezeichnung muss eine eindeutige gerendert `id` Wert.

Um solche Szenarien entworfen werden, können ASP.NET bestimmte Steuerelemente als Benennen von Containern angezeigt werden. Ein Benennungscontainer dient als neue `ID` Namespace. Alle Serversteuerelemente, die innerhalb der Benennungscontainer haben ihre gerenderten `id` Wert mit dem Präfix der `ID` naming Containersteuerelements. Z. B. die `GridView` und `GridViewRow` Klassen sind beide Benennen von Containern. Folglich ein Label-Steuerelement in ein GridView TemplateField mit definierten `ID` `ProductName` erhält einen gerenderten `id` Wert `GridViewID_GridViewRowID_ProductName`. Da *GridViewRowID* ist eindeutig für jede GridView-Zeile, die den resultierenden `id` Werte eindeutig sind.

> [!NOTE]
> Die [ `INamingContainer` Schnittstelle](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) wird verwendet, um anzugeben, dass eine bestimmte ASP.NET-Serversteuerelement als einem Benennungscontainer fungieren soll. Die `INamingContainer` Schnittstelle ausgeschrieben nicht auf alle Methoden, die das Serversteuerelement implementieren muss, wird stattdessen als Markierung verwendet. Beim Generieren der gerenderten Markups aus, wenn ein Steuerelement diese Schnittstelle implementiert, klicken Sie dann das Modul für ASP.NET automatisch das Präfix der `ID` Wert abhängigen gerendert `id` Werte des Attributs. Dieser Prozess wird in Schritt2 ausführlicher erläutert.


Benennen von Containern ändern nicht nur die gerenderten `id` -Attributwert, aber auch beeinflussen, wie das Steuerelement programmgesteuert aus der ASP.NET-Seite Code-Behind-Klasse verwiesen werden kann. Die `FindControl("controlID")` Methode wird häufig verwendet, um programmgesteuert ein Websteuerelement verweisen. Allerdings `FindControl` nicht über das Benennen von Containern durchdringen. Sie können nicht direkt verwenden Sie daher die `Page.FindControl` Methode, um Steuerelemente innerhalb einer GridView oder andere Benennungscontainer verweisen.

Wie Sie möglicherweise Überwachungspfads haben, werden Gestaltungsvorlagen und ContentPlaceHolders sowohl als implementiert Benennen von Containern. In diesem Lernprogramm untersuchen wir wie master Pages Auswirkungen HTML-Elements `id` Werte und Methoden zum programmgesteuerten Websteuerelemente innerhalb einer Inhaltsseite mit verweisen `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Schritt 1: Hinzufügen einer neuen ASP.NET-Seite

Zur Veranschaulichung der Konzepte erläutert, die in diesem Lernprogramm fügen Sie eine neue ASP.NET-Seite auf unserer Website. Erstellen Sie eine neue Seite mit dem Namen `IDIssues.aspx` im Stammordner, binden an die `Site.master` Masterseite.


![Fügen Sie den Inhalt Seite IDIssues.aspx in den Stammordner](control-id-naming-in-content-pages-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen der Inhaltsseite `IDIssues.aspx` in den Stammordner


Visual Studio erstellt automatisch ein Inhaltssteuerelement für jeden der vier ContentPlaceHolders der Masterseite. Wie in notiert die [ *mehrere ContentPlaceHolders und Standardinhalt* ](multiple-contentplaceholders-and-default-content-vb.md) Tutorial, wenn ein Inhaltssteuerelement nicht vorhanden ist wird stattdessen der Masterseite ContentPlaceHolder Standardinhalt ausgegeben. Da die `QuickLoginUI` und `LeftColumnContent` ContentPlaceHolders enthalten Standard-Markup für diese Seite, fahren Sie fort, und entfernen Sie die entsprechenden Inhaltssteuerelemente aus `IDIssues.aspx`. An diesem Punkt sollte die Inhaltsseite deklarativem Markup wie folgt aussehen:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

In der [ *Titel, Meta-Tags und andere HTML-Header angeben, der Gestaltungsvorlage* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) Lernprogramm wir eine benutzerdefinierte Basisseite-Klasse erstellt (`BasePage`) den Titel der Seite, die automatisch konfiguriert, wenn es sich handelt nicht explizit festgelegt wurde. Für die `IDIssues.aspx` Seite, um diese Funktionalität zu nutzen, muss der Seite Code-Behind-Klasse ableiten der `BasePage` Klasse (anstelle von `System.Web.UI.Page`). Ändern Sie die Definition der Code-Behind-Klasse, sodass er wie folgt aussieht:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Aktualisieren Sie schließlich die `Web.sitemap` Datei einen Eintrag für diese neue Lektion einzuschließen. Hinzufügen einer `<siteMapNode>` Element, und legen seine `title` und `url` Attribute "ID Naming Probleme mit der Versionskontrolle" und `~/IDIssues.aspx`zugeordnet. Nach dem Herstellen der Code Ihrer `Web.sitemap` Datei Markup sollte etwa wie folgt aussehen:


[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Wie in Abbildung 2 dargestellt, die neue Site-Eintrag für die Zuordnung in `Web.sitemap` sofort in den Lektionen-Abschnitt in der linken Spalte dargestellt wird.


![Im Abschnitt Lektionen enthält nun einen Link zum &quot;Steuerelement-ID Naming Probleme&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Abbildung 02**: Abschnitt Lektionen enthält nun einen Link zu "Steuerelement-ID Naming Probleme"


## <a name="step-2-examining-the-renderedidchanges"></a>Schritt 2: Untersuchen der gerenderten`ID`Änderungen

Um die Änderungen der ASP.NET besser zu verstehen Modul macht, um den gerenderten `id` steuert die Werte des Servers, fügen Sie ein paar Websteuerelemente der `IDIssues.aspx` Seite, und zeigen Sie dann auf die gerenderten Markups an den Browser gesendet. Insbesondere Geben Sie den Text "Geben Sie Ihr Alter:" gefolgt von einem TextBox-Steuerelement. Weiter unten auf der Seite fügen Sie ein Websteuerelement Schaltfläche und ein Label-Websteuerelement hinzu. Legen Sie des Textfelds `ID` und `Columns` Eigenschaften `Age` und 3 bzw. Legen Sie der Schaltfläche `Text` und `ID` Eigenschaften auf "Senden" und `SubmitButton`. Deaktivieren Sie der Bezeichnung `Text` Eigenschaft, und legen seine `ID` auf `Results`.

An diesem Punkt sollte Ihr Inhaltssteuerelement deklarativem Markup etwa wie folgt aussehen:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

Abbildung 3 zeigt die Seite, wenn Sie über Visual Studio-Designer angezeigt.


[![Die Seite enthält drei Websteuerelemente: ein Textfeld, Schaltfläche und Bezeichnung](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Abbildung 03**: die Seite enthält drei Websteuerelemente: ein Textfeld, einer Schaltfläche und einer Bezeichnung ([klicken Sie hier, um das Bild in voller Größe angezeigt](control-id-naming-in-content-pages-vb/_static/image5.png))


Besuchen Sie die Seite über einen Browser, und zeigen Sie die HTML-Quelle. Als Markup veranschaulicht die `id` Werte der HTML-Elemente für das Textfeld, Schaltfläche und Bezeichnung Web-Steuerelemente sind eine Kombination der `ID` Werte von der Web-Steuerelementen und der `ID` Werte für das Benennen von Containern auf der Seite.


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Wie weiter oben in diesem Lernprogramm erwähnt, werden die Gestaltungsvorlage und seine ContentPlaceHolders als Benennen von Containern dienen. Daher sowohl die gerenderten beitragen `ID` Werte ihrer geschachtelte Steuerelemente. Nehmen Sie des Textfelds `id` Attribut, z. B.: `ctl00_MainContent_Age`. Bedenken Sie, dass des Textfeldsteuerelement `ID` Wert war `Age`. Dies ist mit dem Präfix des ContentPlaceHolder Steuerelements `ID` Wert `MainContent`. Darüber hinaus ist dieser Wert der Gestaltungsvorlage Präfix `ID` Wert `ctl00`. Im Endeffekt ein `id` Attributwert bestehend aus dem `ID` Werte der Masterseite ContentPlaceHolder-Steuerelement und das Textfeld selbst.

Abbildung 4 veranschaulicht dieses Verhalten. Um zu bestimmen, das gerenderte `id` von der `Age` TextBox, beginnen Sie mit der `ID` Wert des Textfeld-Steuerelements `Age`. Arbeiten Sie als Nächstes den Weg der Steuerelementhierarchie nach oben. Bei jeder Benennungscontainer (dieser Knoten ein orangefarbenes Farbe) als Präfix den aktuellen gerendert `id` mit des Benennungscontainers `id`.


![Die Id-Attribute gerendert werden basierend auf der ID-Werte von der Benennen von Containern](control-id-naming-in-content-pages-vb/_static/image6.png)

**Abbildung 04**: der gerendert `id` Attribute werden basierend auf den `ID` Werte Benennen von Containern


> [!NOTE]
> Wie erläutert, die `ctl00` Teil des gerenderten `id` Attribut bildet die `ID` Wert, der Gestaltungsvorlage, aber Sie vielleicht wie diese `ID` Wert stammt, zu. Wir haben sie keinen in unsere Master- oder Content-Seite an einer beliebigen Stelle angegeben. Die meisten Steuerelemente auf einer ASP.NET-Seite werden explizit über deklarativem Markup der Seite hinzugefügt. Die `MainContent` ContentPlaceHolder Steuerelement wurde explizit angegeben, in das Markup der `Site.master`; das `Age` Textfeld definiert wurde `IDIssues.aspx`des Markup. Wir können angeben, die `ID` Werte für diese Typen von Steuerelementen, die über das Fenster "Eigenschaften" oder über die deklarative Syntax. Andere Steuerelemente, z. B. die Gestaltungsvorlage selbst sind nicht im deklarativen Markup definiert. Daher ihre `ID` Werte automatisch für uns generiert werden müssen. Das ASP.NET Modul legt den `ID` Werte zur Laufzeit für die Steuerelemente, deren IDs nicht explizit festgelegt wurde. Er verwendet das Namensmuster `ctlXX`, wobei *XX* ein zunehmenden Ganzzahlwert.


Da die Masterseite selbst fungiert als Benennungscontainer, die Websteuerelementen in die Masterseite definiert auch haben geändert gerenderten `id` Werte des Attributs. Z. B. die `DisplayDate` Bezeichnung, die wir, die Gestaltungsvorlage in hinzugefügt der [ *erstellen ein Layout standortweite mit Masterseiten für* ](creating-a-site-wide-layout-using-master-pages-vb.md) Lernprogramm hat Folgendes Markup Rendern:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Beachten Sie, dass die `id` -Attribut umfasst sowohl die Masterseite `ID` Wert (`ctl00`) und die `ID` Wert das Label-Steuerelement (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Schritt 3: Programmgesteuertes Verweisen Websteuerelemente über`FindControl`

Jede ASP.NET-Serversteuerelement umfasst eine `FindControl("controlID")` -Methode, die das Steuerelement Nachfolger für ein Steuerelement mit dem Namen durchsucht *ControlID*. Wenn ein solches Steuerelement gefunden wird, wird diese zurückgegeben; Wenn kein entsprechendes Steuerelement gefunden wird, `FindControl` gibt `Nothing`.

`FindControl` ist nützlich in Szenarien, in denen müssen Sie ein Steuerelement zuzugreifen, aber Sie verfügen nicht über einen direkten Verweis auf sie. Beim Arbeiten mit Daten wie z. B. die GridView Websteuerelemente Steuerelemente innerhalb der GridView-Felder in die deklarative Syntax einmal definiert werden, aber zur Laufzeit eine Instanz des Steuerelements für jede Zeile GridView erstellt wird. Folglich die Steuerelemente zur Laufzeit generierten vorhanden, aber wir haben keinen direkten Verweis aus der CodeBehind-Klasse verfügbar. Daher müssen wir verwenden `FindControl` programmgesteuert mit einem bestimmten Steuerelement innerhalb von Feldern für die GridView zu arbeiten. (Weitere Informationen zur Verwendung von `FindControl` den Zugriff auf die Steuerelemente in ein Data-Websteuerelement-Vorlagen finden Sie unter [benutzerdefinierte Formatierung basierend auf Daten](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md).) Dieses Szenario tritt auf, wenn Websteuerelemente dynamisch ein Webformular hinzufügen, das Thema behandelt [dynamische Daten Eintrag Benutzeroberflächen erstellen](https://msdn.microsoft.com/library/aa479330.aspx).

Um zu veranschaulichen, mit der `FindControl` Methode zum Suchen nach Steuerelementen innerhalb einer Inhaltsseite, erstellen Sie einen Ereignishandler für das `SubmitButton`des `Click` Ereignis. Fügen Sie in der Ereignishandler den folgenden Code, programmgesteuert verweist auf die `Age` Textfeld und `Results` Beschriftung mit der `FindControl` Methode und zeigt dann eine Nachricht in `Results` basierend auf der Eingabe des Benutzers.

> [!NOTE]
> Natürlich müssen wir verwenden müssen `FindControl` auf die Bezeichnung und einer TextBox-Steuerelemente in diesem Beispiel zu verweisen. Wir konnten verweisen sie direkt über ihre `ID` Eigenschaftswerte. Ich verwende `FindControl` hier, um zu veranschaulichen, was geschieht, wenn mit `FindControl` aus einer Inhaltsseite.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Die Syntax zum Aufrufen von verwendet, während die `FindControl` Methode unterscheidet sich in den ersten beiden Zeilen der `SubmitButton_Click`, sie semantisch gleichwertig sind. Wie bereits erwähnt, die allen ASP.NET-Serversteuerelementen enthalten eine `FindControl` Methode. Dies schließt die `Page` -Klasse, von der alle ASP.NET Code-Behind-Klassen ableiten müssen. Aus diesem Grund Aufrufen `FindControl("controlID")` entspricht dem Aufruf `Page.FindControl("controlID")`, vorausgesetzt, Sie noch nicht außer Kraft gesetzt der `FindControl` -Methode in Ihrem Code-Behind-Klasse in einer benutzerdefinierten Basisklasse.

Nach der Eingabe dieses Codes, besuchen Sie die `IDIssues.aspx` Seite über einen Browser, geben Sie Ihr Alter, und klicken Sie auf die Schaltfläche "Absenden". Wenn Sie auf die Schaltfläche "Absenden" eine `NullReferenceException` ausgelöst wird (siehe Abbildung 5).


[![Es wird eine NullReferenceException ausgelöst.](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Abbildung 05**: ein `NullReferenceException` ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](control-id-naming-in-content-pages-vb/_static/image9.png))


Wenn Sie einen Haltepunkt, in Festlegen der `SubmitButton_Click` Ereignishandler, die Sie sehen, dass beide Aufrufe von `FindControl` zurückgeben `Nothing`. Die `NullReferenceException` wird ausgelöst, wenn Sie versuchen, Zugriff auf die `Age` Textfeldss `Text` Eigenschaft.

Das Problem besteht darin, die `Control.FindControl` sucht nur *Steuerelement*des Nachfolger, die in den gleichen Benennungscontainer sind. Da die Gestaltungsvorlage ein neues Benennungscontainer, einen Aufruf von bildet `Page.FindControl("controlID")` nie Anpassungsszenarios jedoch stattdessen das Masterseite `ctl00`. (Wieder finden Sie in Abbildung 4 zeigt die Steuerelementhierarchie Anzeigen der `Page` Objekt wie das übergeordnete Element des Objekts Masterseite `ctl00`.) Aus diesem Grund die `Results` Bezeichnung und `Age` Textfeld wurden nicht gefunden und `ResultsLabel` und `AgeTextBox` Werte zugewiesen werden `Nothing`.

Es gibt zwei problemumgehungen für dieses Problem: Wir können einen Drilldown, einem Benennungscontainer zu einem Zeitpunkt, an das entsprechende Steuerelement; oder Sie können erstellen wir unser eigenes `FindControl` Methode, die Anpassungsszenarios Benennen von Containern. Betrachten wir jede dieser Optionen.

### <a name="drilling-into-the-appropriate-naming-container"></a>Drilldown zu den entsprechenden Namenscontainer

Mit `FindControl` zu verweisen die `Results` Bezeichnung oder `Age` Textfeld müssen wir Aufrufen `FindControl` aus einem Vorgänger-Steuerelement in den gleichen Benennungscontainer. Wie Abbildung 4 gezeigt, die `MainContent` ContentPlaceHolder-Steuerelement ist die einzige Vorgänger des `Results` oder `Age` , der sich im selben Benennungscontainer. In anderen Worten: Aufrufen der `FindControl` Methode aus der `MainContent` -Steuerelement, wie im folgenden Codeausschnitt gezeigt ordnungsgemäß gibt einen Verweis auf die `Results` oder `Age` Steuerelemente.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Allerdings kann nicht arbeiten wir die `MainContent` ContentPlaceHolder aus unserer Inhaltsseite Code-Behind-Klasse, die die oben aufgeführten Syntax verwenden, da in die Masterseite ContentPlaceHolder definiert ist. Stattdessen haben wir verwenden `FindControl` zum Abrufen eines Verweises auf `MainContent`. Ersetzen Sie den Code in der `SubmitButton_Click` -Ereignishandler durch den folgenden Änderungen:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Wenn Sie die Seite über einen Browser aufrufen, geben Sie Ihr Alter, und klicken Sie auf die Schaltfläche "Submit" eine `NullReferenceException` ausgelöst wird. Wenn Sie einen Haltepunkt, in Festlegen der `SubmitButton_Click` Ereignishandler, die Sie sehen, dass diese Ausnahme tritt beim Aufrufen der `MainContent` des Objekts `FindControl` Methode. Die `MainContent` Objekt gleich `Nothing` da die `FindControl` Methode kann ein Objekt mit dem Namen "MainContent" nicht finden. Die zugrunde liegende Ursache ist identisch mit der `Results` Bezeichnung und `Age` TextBox-Steuerelemente: `FindControl` beginnt die Suche am Anfang der Steuerelementhierarchie und ist nicht in einzudringen Benennen von Containern, aber die `MainContent` ContentPlaceHolder ist innerhalb der Masterseite ist die einem Benennungscontainer.

Bevor wir verwenden können, `FindControl` zum Abrufen eines Verweises auf `MainContent`, wir benötigen zunächst einen Verweis auf das Steuerelement für die Gestaltungsvorlage. Nachdem wir einen Verweis auf die Gestaltungsvorlage haben wir können einen Verweis auf die `MainContent` ContentPlaceHolder über `FindControl` und dort Verweise auf die `Results` Bezeichnung und `Age` Textfeld (erneut aus, durch die Verwendung `FindControl`). Aber wie erhalten wir einen Verweis auf die Gestaltungsvorlage? Durch Überprüfen der `id` Attribute in der gerenderten Markups ist es offensichtlich, der Gestaltungsvorlage `ID` Wert `ctl00`. Daher können wir verwenden `Page.FindControl("ctl00")` um einen Verweis auf die Gestaltungsvorlage zu erhalten, klicken Sie dann zum Abrufen eines Verweises auf dieses Objekt verwenden `MainContent`und so weiter. Der folgende Codeausschnitt veranschaulicht diese Logik:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Während dieser Code sicherlich ausgeführt wird, setzt er voraus, dass die Gestaltungsvorlage automatisch generierten `ID` immer `ctl00`. Es ist nie eine gute Idee, Annahmen über automatisch generierte Werte zu machen.

Glücklicherweise ein Verweis auf die Gestaltungsvorlage Zugriff erfolgt über die `Page` Klasse `Master` Eigenschaft. Aus diesem Grund nicht auf zurückzugreifen `FindControl("ctl00")` Abrufen eines Verweises der Masterseite, um Zugriff auf die `MainContent` ContentPlaceHolder, wir können stattdessen `Page.Master.FindControl("MainContent")`. Update der `SubmitButton_Click` -Ereignishandler durch den folgenden Code:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Dieses Mal besuchen die Seite über einen Browser Dein eingeben, und klicken Sie auf die Schaltfläche "Absenden" zeigt die Meldung in die `Results` bezeichnen, wie erwartet.


[![Das Alter des Benutzers wird in der Bezeichnung angezeigt.](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Abbildung 06**: der Benutzer-Agent wird in der Bezeichnung angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](control-id-naming-in-content-pages-vb/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rekursiv durchsuchen Benennen von Containern

Die Ursache im vorangehenden Codebeispiel wird auf die verwiesen wird die `MainContent` ContentPlaceHolder-Steuerelement aus der Masterseite und dann die `Results` Bezeichnung und `Age` TextBox-Steuerelemente aus `MainContent`, liegt daran, dass die `Control.FindControl` Methode nur durchsucht in *Steuerelement*des Namenscontainer. Mit `FindControl` informieren Sie sich innerhalb der Benennungscontainer ist in den meisten Szenarien sinnvoll, da zwei Steuerelemente in zwei verschiedenen Benennen von Containern die gleiche möglicherweise `ID` Werte. Nehmen wir eine GridView, die eine Bezeichnung Web-Steuerelement mit dem Namen definieren `ProductName` innerhalb der von seiner TemplateFields. Wenn die Daten an die GridView zur Laufzeit gebunden ist eine `ProductName` Bezeichnung wird für jede Zeile GridView erstellt. Wenn `FindControl` durchsucht alle Namen aufgerufen, Container und wir `Page.FindControl("ProductName")`, welche Label-Instanz sollte die `FindControl` zurückgeben? Die `ProductName` Bezeichnung in der ersten Zeile der GridView? Das Element in der letzten Zeile?

Daher müssen `Control.FindControl` suchen nur *Steuerelement*die namensgebungsattribute Container in den meisten Fällen sinnvoll. Aber es andere Fälle, wie mit Internetzugriff uns gibt, in dem wir eine eindeutige haben `ID` benennen Sie in allen Containern und vermeiden, dass jede Benennungscontainer in der Steuerelementhierarchie auf ein Steuerelement zuzugreifen akribisch verweisen möchten. Mit einem `FindControl` Variante, die rekursiv durchsucht alle Benennen von Containern macht sinnvoll ist, zu. .NET Framework schließt eine solche Methode leider nicht.

Die gute Nachricht ist, dass wir unser eigenes zusammenzulegen `FindControl` Methode, eine rekursive Suche alle Benennen von Containern. Tatsächlich verwenden *Erweiterungsmethoden* wir auf nachverfolgen können eine `FindControlRecursive` Methode, um die `Control` Klasse, die ihren vorhandenen begleitet `FindControl` Methode.

> [!NOTE]
> Erweiterungsmethoden sind eine Funktion, die sich neue c# 3.0 und Visual Basic 9, den Sprachen, die mit .NET Framework Version 3.5 und Visual Studio 2008 geliefert. Erweiterungsmethoden ermöglichen kurz gesagt, ein Entwickler zum Erstellen einer neuen Methode für einen vorhandenen Klassentyp über eine spezielle Syntax. Weitere Informationen zu dieser Funktion hilfreich finden Sie in meinem Artikel [Base Type-Funktionalität mit Erweiterungsmethoden erweitern](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Um die Erweiterungsmethode zu erstellen, fügen Sie eine neue Datei, die `App_Code` Ordner mit dem Namen `PageExtensionMethods.vb`. Fügen Sie eine Erweiterungsmethode mit dem Namen `FindControlRecursive` , akzeptiert als Eingabe eine `String` Parameter mit dem Namen `controlID`. Für Erweiterungsmethoden ordnungsgemäß funktioniert, ist es von entscheidender Bedeutung, dass die Klasse als gekennzeichnet werden eine `Module` und, die die Erweiterungsmethoden vorangestellt werden die `<Extension()>` Attribut. Darüber hinaus alle Erweiterungsmethoden müssen akzeptieren als ersten Parameter ein Objekt des Typs, für welche die Erweiterungsmethode gilt.

Fügen Sie folgenden Code, der `PageExtensionMethods.vb` um dies zu definieren `Module` und die `FindControlRecursive` Erweiterungsmethode:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Mit diesem Code werden, zurück zu den `IDIssues.aspx` Seite des Code-Behind-Klasse und kommentieren Sie den aktuellen `FindControl` Methodenaufrufe. Ersetzen Sie sie durch Aufrufe von `Page.FindControlRecursive("controlID")`. Über Erweiterungsmethoden praktisch ist, dass sie direkt in den Dropdownlisten IntelliSense angezeigt werden. Wie Abbildung 7 zeigt ein Beispiel, bei der Eingabe `Page` und drücken Sie dann den Zeitraum, der `FindControlRecursive` Methode ist in IntelliSense Dropdownelement zusammen mit den anderen enthalten `Control` -Klassenmethoden.


[![Erweiterungsmethoden werden in IntelliSense Dropdown-Liste enthalten.](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Abbildung 07**: Erweiterungsmethoden in IntelliSense Dropdown-Liste enthalten sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](control-id-naming-in-content-pages-vb/_static/image15.png))


Geben Sie den folgenden Code in der `SubmitButton_Click` -Ereignishandler und Testen Sie es durch die Seite besuchen, Dein eingeben und durch Klicken auf die Schaltfläche "Absenden". Wie in Abbildung 6 dargestellt, die resultierende Ausgabe wird die Meldung, "Alter Jahre alt!"


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Da Erweiterungsmethoden neu in c# 3.0 und Visual Basic 9 sind, bei Verwendung von Visual Studio 2005 können Sie keine Erweiterungsmethoden [c#]. Stattdessen müssen Sie zum Implementieren der `FindControlRecursive` -Methode in eine Hilfsklasse. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) verfügt über ein Beispiel für eine solche in seinem Blogbeitrag [Maser ASP.NET-Seiten und `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Schritt 4: Mit dem richtigen`id`-Attributwert in clientseitigem Skript

Wie in diesem Lernprogramm Einführung erwähnt, ein Websteuerelement gerenderten `id` Attribut wird häufig in clientseitigem Skript verwendet, um programmgesteuert auf ein bestimmtes HTML-Element verweisen. Der folgenden JavaScript-Code verweist, beispielsweise ein HTML-Element, durch dessen `id` und anschließend den Wert in ein modales Meldungsfeld angezeigt:


[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Wie bereits erwähnt, die in ASP.NET, die Seiten, enthalten keine Benennungskonventionen, die gerenderte HTML-Element des Containers `id` Attribut entspricht des Websteuerelements `ID` Eigenschaftswert. Aus diesem Grund ist es durch hartcodierung in verlockend `id` Attributwerte in JavaScript-Code. D. h., wenn Sie wissen Sie zugreifen möchten die `Age` TextBox-Websteuerelement über die clientseitige Skripts, holen Sie dies über einen Aufruf an `document.getElementById("Age")`.

Das Problem bei diesem Ansatz ist, die bei Verwendung von Masterseiten (oder anderen Benennung Containersteuerelementen), das gerenderte HTML `id` ist nicht gleichbedeutend mit des Websteuerelements `ID` Eigenschaft. Ihre erste Neigung möglicherweise besuchen die Seite über einen Browser, und zeigen Sie die Quelle aus, um zu bestimmen, den tatsächlichen `id` Attribut. Wenn Sie die gerenderte wissen `id` Wert, Sie können fügen Sie ihn in den Aufruf von `getElementById` auf das HTML-Element zuzugreifen, Sie über die clientseitige Skripts zu arbeiten müssen. Dieser Ansatz ist kleiner als ideal, da bestimmte Änderungen an der Seite Hierarchie steuern oder Änderungen an der `ID` Eigenschaften der Steuerelemente Benennungskonvention werden das resultierende alter `id` -Attribut, damit wichtige JavaScript-Code.

Die gute Nachricht ist, die die `id` Attributwert, der gerendert wird, kann zugegriffen werden, im serverseitigen Code über des Websteuerelements [ `ClientID` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Sie sollten diese Eigenschaft verwenden, um zu bestimmen, die `id` -Attributwert in clientseitigem Skript verwendet. Beispielsweise, um eine JavaScript-Funktion auf der Seite hinzufügen, wenn aufgerufen wird, zeigt den Wert des der `Age` Textfeld in ein modales Meldungsfeld fügen Sie den folgenden Code hinzu der `Page_Load` Ereignishandler:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

Der obige Code fügt den Wert der `Age` Textfeldss `ClientID` Eigenschaft in der JavaScript-Aufruf von `getElementById`. Wenn Sie diese Seite über einen Browser und zeigen Sie die HTML-Quelle, finden Sie den folgenden JavaScript-Code:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Beachten Sie, dass wie den richtigen `id` -Attributwert `ctl00_MainContent_Age`, wird angezeigt, in dem Aufruf von `getElementById`. Da dieser Wert zur Laufzeit berechnet wird, funktioniert es unabhängig von späteren Änderungen an der Seitenhierarchie für das Steuerelement.

> [!NOTE]
> Dieses JavaScript-Beispiel zeigt lediglich eine JavaScript-Funktion hinzufügen, die das HTML-Element gerendert, die durch ein Serversteuerelement ordnungsgemäß verweist. Um diese Funktion verwenden, müssten Sie zusätzliche JavaScript, um die Funktion aufzurufen, wenn das Dokument lädt oder einige spezielle Handlung des Benutzers herausstellt erstellen. Weitere Informationen zu diesen und verwandten Themen, lesen Sie [arbeiten mit clientseitigem Skript](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Zusammenfassung

Bestimmte ASP.NET-Serversteuerelemente fungieren als Benennen von Containern, die wirkt sich auf die gerenderten `id` -Attribut Werte ihrer untergeordneten Steuerelemente sowie den Bereich von Steuerelementen durch canvassed der `FindControl` Methode. Im Hinblick auf die Masterseiten sind die Gestaltungsvorlage selbst und seine Steuerelemente ContentPlaceHolder Container benannt. Folglich müssen wir her etwas mehr Arbeit programmgesteuert Steuerelemente innerhalb der Inhaltsseite mit Verweisen zu versetzen `FindControl`. In diesem Lernprogramm zwei Techniken untersucht: Bohrzyklus in das ContentPlaceHolder-Steuerelement und ein Aufruf der `FindControl` Methode und eigenen parallelen `FindControl` Implementierung dieser rekursiv durchsucht alle Benennen von Containern.

Zusätzlich zu den serverseitigen-Probleme Benennen von Containern einführen, im Hinblick auf die Web-Steuerelemente verweisen, auch die clientseitige Probleme vorliegen. In Ermangelung Benennen von Containern, das Websteuerelement des `ID` Eigenschaftswert und gerendert `id` Attributwert sind identisch. Jedoch durch das Hinzufügen von Benennungscontainer gerenderten `id` Attribut enthält sowohl die `ID` Werte für das Websteuerelement und der durch ein in die Steuerelementhierarchie Versionsherkunft. Diese Benennungskonvention Probleme sind ein nicht-Problem, solange Sie des Websteuerelements verwenden `ClientID` -Eigenschaft zum Bestimmen der gerenderten `id` -Attributwert in Ihre Skriptdatei auf Clientseite.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Masterseiten ASP.NET und `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Erstellen von Benutzeroberflächen für Dynamic Data-Eintrag](https://msdn.microsoft.com/library/aa479330.aspx)
- [Erweitern der Funktionalität der Basistyp mit Erweiterungsmethoden [c#]](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Gewusst wie: Verweisen auf ASP.NET Master Seiteninhalt](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Master-Seiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [Arbeiten mit clientseitigem Skript](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Zack Jones und Suchi Barnerjee. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](urls-in-master-pages-vb.md)
> [Weiter](interacting-with-the-master-page-from-the-content-page-vb.md)
