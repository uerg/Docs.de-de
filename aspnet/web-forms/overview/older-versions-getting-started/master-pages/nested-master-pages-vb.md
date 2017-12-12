---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Geschachtelte Masterseiten (VB) | Microsoft Docs
author: rick-anderson
description: Zeigt, wie eine Masterseite innerhalb einer anderen zu schachteln.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 41a9fd6b752252d563a0f15a420262cbb31c19ff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="nested-master-pages-vb"></a>Geschachtelte Masterseiten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Zeigt, wie eine Masterseite innerhalb einer anderen zu schachteln.


## <a name="introduction"></a>Einführung

Im Verlauf der letzten neun Lernprogramme haben wir gesehen, wie ein Layout standortweite mit Masterseiten implementiert wird. Kurz gesagt, ermöglichen es Masterseiten uns, den Entwickler der Seite, um allgemeine Markup in die Masterseite zusammen mit bestimmten Regionen zu definieren, die auf Basis von Seiteninhalt Inhaltsseite angepasst werden kann. Die ContentPlaceHolder-Steuerelemente auf einer Masterseite anzugeben, die anpassbare Bereiche; Der angepasste Markup für die ContentPlaceHolder-Steuerelemente sind in der Inhaltsseite über Inhaltssteuerelemente definiert.

Die Masterseite-Techniken, die wir bisher untersucht haben, sind sehr nützlich, wenn Sie ein einziges Layout für die gesamte Website verwendet haben. Allerdings müssen zahlreiche großen Websites ein Website-Layout, das angepasst wird, über verschiedene Abschnitte. Betrachten Sie beispielsweise eine health care-Anwendung wird von Personal zum Verwalten von Patienteninformationen, Aktivitäten und Abrechnung aus. Möglicherweise gibt es drei Arten von Webseiten in dieser Anwendung:

- Mitarbeiter Member-spezifische Seiten, in denen Mitarbeiter Verfügbarkeit aktualisieren können, Zeitpläne anzeigen, oder bitten Urlaub.
- Patienten-spezifische Seiten, in denen Mitarbeiter anzeigen oder bearbeiten Sie die Informationen für einen bestimmten Patienten.
- Abrechnung-spezifische Seiten, in dem Buchhaltung aktuelle überprüfen, Anspruch Statuswerte und Finanzberichte.

Jeder Seite möglicherweise ein gebräuchliches Layout, z. B. ein Menü über dem oberen und eine Reihe von häufig verwendeten Links entlang des unteren freigeben. Die Mitarbeiter-, Patienten und Abrechnung-spezifische Seiten müssen aber möglicherweise zum Anpassen dieser allgemeinen Layouts dargestellt. Vielleicht alle Mitarbeiter-spezifische Seiten sollte z. B. eine Liste Kalender und jede Aufgabe, die Verfügbarkeit und täglichen Zeitplan des aktuell angemeldeten Benutzers anzeigen, enthalten. Möglicherweise müssen alle Patienten-spezifische Seiten zum Anzeigen der Namen, Adresse und Informationen zur Versicherung für den Patienten, dessen Informationen bearbeitet wird.

Es ist möglich, mit solchen benutzerdefinierten Layouts erstellen *verschachtelte Gestaltungsvorlagen*. Um das oben beschriebene Szenario zu implementieren, würden wir beginnen, durch das Erstellen einer Masterseite, die die standortweite Layout, das Menü und die Fußzeile Inhalt mit ContentPlaceHolders definieren die anpassbare Bereiche definiert. Wir würden dann drei verschachtelte Gestaltungsvorlagen, eine für jeden Typ von Webseite erstellen. Jede geschachtelte Masterseite würden den Inhalt für den Typ des Inhaltsseiten definieren, die die Gestaltungsvorlage verwenden. Geschachtelte Masterseite für Patienten-spezifische Inhaltsseiten wäre also Markup und programmgesteuerte Logik für die Anzeige von Informationen zu den zu bearbeitenden Patienten enthalten. Beim Erstellen einer neuen Patienten-spezifische Seite würden wir es diese geschachtelten Masterseite binden.

In diesem Lernprogramm wird gestartet, da die Vorteile der verschachtelte Gestaltungsvorlagen hervorgehoben. Es zeigt dann zum Erstellen und verwenden verschachtelte Gestaltungsvorlagen.

> [!NOTE]
> Verschachtelte Gestaltungsvorlagen wurden seit Version 2.0 von .NET Framework möglich. Allerdings in Visual Studio 2005 zur Entwurfszeit Unterstützung für verschachtelte Gestaltungsvorlagen nicht enthalten. Die gute Nachricht ist, dass Visual Studio 2008 eine komfortable zur Entwurfszeit für verschachtelte Gestaltungsvorlagen bietet. Wenn Sie mithilfe von verschachtelte Gestaltungsvorlagen interessiert sind, aber weiterhin Visual Studio 2005 verwenden, sehen Sie sich [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [Tipps für geschachtelte Masterseiten in Visual Studio 2005-Entwurfszeit](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Die Vorteile der geschachtelten Masterseiten

Viele Websites weisen eine übergreifende Standortentwurfs sowie mehr benutzerdefinierte Designs speziell für bestimmte Typen von Seiten. Z. B. in unserer Demo-Webanwendung wir haben eine rudimentäre Bereich "Verwaltung" (der Seiten in der `~/Admin` Ordner). Derzeit die Webseiten in der `~/Admin` Ordner verwenden dieselbe Masterseite als diese Seiten nicht in den Bereich "Verwaltung" (nämlich `Site.master` oder `Alternate.master`, abhängig von der Auswahl des Benutzers).

> [!NOTE]
> Nehmen Sie vorerst an, dass unsere Standort nur eine Masterseite verfügt `Site.master`. Wir müssen adressieren, verschachtelte Gestaltungsvorlagen mit zwei (oder mehr) Gestaltungsvorlagen beginnend mit "mithilfe einer geschachtelten Master Seite für die Bereich"Verwaltung"" weiter unten in diesem Lernprogramm verwenden.


Angenommen Sie, wir aufgefordert wurden, passen Sie das Layout der Verwaltungsseiten enthalten zusätzliche Informationen oder Links, die andernfalls nicht in anderen Seiten in der Website vorhanden wäre. Es gibt vier Techniken zum Implementieren dieser Anforderung:

1. Manuell hinzufügen Administration-spezifische Informationen und Links für jede Seite Inhalt in die `~/Admin` Ordner.
2. Update der `Site.master` Masterseite Verwaltung Abschnitt-spezifische Informationen und Links, und fügen Sie Code hinzu, der Masterseite angezeigt oder ausgeblendet in diesen Abschnitten gibt an, ob eine der Administration-Seiten besucht wurde anhand.
3. Erstellen eine neue Masterseite kopieren Sie speziell für den Bereich "Verwaltung" über das Markup `Site.master`Verwaltung Abschnitt-spezifische Informationen und Links hinzufügen und aktualisieren Sie dann die Inhaltsseiten in der `~/Admin` zu dieser neuen Master zu verwendenden Ordner an Seite ".
4. Erstellen eine geschachtelten Masterseite, die an bindet `Site.master` und über die Inhaltsseiten in der `~/Admin` Ordner verwenden diese neue geschachtelte Masterseite. Diese geschachtelten Masterseite würde nur die zusätzliche Informationen und Links, die speziell für die Administration-Seiten enthalten und müssen nicht das Markup in vordefinierten wiederholen `Site.master`.

Die erste Option ist die am wenigsten attraktiv. Mithilfe von Masterseiten Angelpunkt wird manuell kopieren und fügen allgemeine Markup zum neuen ASP.NET-Seiten verschoben. Die zweite Option akzeptabel ist, jedoch wird die Anwendung weniger verwaltbar, wie er von der Masterseiten mit Markup hebt, die nur gelegentlich angezeigt wird und erfordert die Bearbeitung der Masterseite Entwickler umgehen dieses Markup und wann merken zu müssen, bestimmte Markup wird genau im Vergleich zu angezeigt, wenn es ausgeblendet ist. Dieser Ansatz wäre weniger Hauptfeuerlöschpumpe als Anpassungen aus mehr Typen von Webseiten, die von dieser einzelnen Masterseite untergebracht werden musste.

Die dritte Option entfernt die Übersichtlichkeit und Komplexität Probleme mit der zweiten Option die eingeblendet. Option drei der größte Nachteil ist jedoch, dass es erforderlich ist, kopieren Sie das allgemeine Layout von `Site.master` der neuen Verwaltung Abschnitt-spezifische Masterseite. Wenn es sich später entscheiden, die das standortweite-Layout ändern, das müssen Sie denken Sie daran, die an zwei Orten zu ändern.

Die vierte Option verschachtelte Gestaltungsvorlagen, senden Sie uns das beste aus der zweiten und dritten-Optionen. Die standortweite Layoutinformationen werden in einer Datei - der obersten Ebene Masterseite - beibehalten, während die Inhalte, die spezifisch für bestimmte Regionen in verschiedenen Dateien ausgelagert wird.

In diesem Lernprogramm beginnt mit einem Blick auf das Erstellen und Verwenden einer einfachen geschachtelte Masterseite. Wir erstellen eine brandneue auf oberster Ebene Masterseite, zwei verschachtelte Gestaltungsvorlagen und zwei Inhaltsseiten. Beginnend mit "mithilfe einer geschachtelten Master Seite für die Bereich"Verwaltung"", untersuchen wir unsere vorhandene Masterseite-Architektur, um die Verwendung von verschachtelte Gestaltungsvorlagen umfassen aktualisieren. Insbesondere wir eine geschachtelte Masterseite erstellen und verwenden, um zusätzliche benutzerdefinierte Inhalte für die Inhaltsseiten in umfassen die `~/Admin` Ordner.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Schritt 1: Erstellen einer einfachen auf oberster Ebene Masterseite

Erstellen einen geschachtelte Master basierend auf einer vorhandenen Master Seiten und anschließendes Aktualisieren einer vorhandenen Inhaltsseite Verwendung dieses neuen geschachtelten Masterseite anstelle der obersten Ebene Masterseite gewisse Komplexität umfasst, da die vorhandenen Inhaltsseiten bereits bestimmte erwarten ContentPlaceHolder-Steuerelemente, die in der master-Seite auf oberster Ebene definiert. Aus diesem Grund muss die geschachtelte Gestaltungsvorlage auch die gleichen ContentPlaceHolder-Steuerelemente mit den gleichen Namen enthalten. Darüber hinaus unsere bestimmten demoanwendung hat zwei Masterseiten (`Site.master` und `Alternate.master`) dynamisch zugewiesen sind einer Inhaltsseite ausgehend von einem Benutzer, wodurch diese Komplexität weiter hinzugefügt. Betrachten wir Aktualisieren der vorhandenen Anwendung verschachtelte Gestaltungsvorlagen weiter unten in diesem Lernprogramm verwenden, aber wir zuerst den Fokus auf ein einfaches Masterseiten Beispiel geschachtelt.

Erstellen Sie einen neuen Ordner namens `NestedMasterPages` und fügen Sie dann in diesen Ordner mit dem Namen eine neue Masterseitendatei `Simple.master`. (Siehe Abbildung 1 für einen Screenshot des Projektmappen-Explorer nach dem dieser Ordner und Dateien hinzugefügt wurden.) Ziehen Sie die `AlternateStyles.css` Stylesheetdatei aus dem Projektmappen-Explorer in den Designer. Dadurch wird eine `<link>` Element an der Stylesheetdatei in der `<head>` Elements, nach denen der Gestaltungsvorlage `<head>` des Elements Markup sollte wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Als Nächstes fügen Sie das folgende Markup in das Web Form des `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Dieses Markup zeigt eine Verknüpfung mit dem Titel "Geschachtelte Masterseiten (einfach)" am oberen Rand der Seite in großer Schriftart in einem Hintergrundthread dunkelblaue weiß. Wird unter, die die `MainContent` ContentPlaceHolder. Abbildung 1 zeigt die `Simple.master` Masterseite, wenn in der Visual Studio-Designer geladen.


[![Der geschachtelte Masterseite definiert spezifische Inhalte zu den Seiten im Bereich "Verwaltung"](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Abbildung 01**: die geschachtelte Master Seite definiert Content speziell für die Seiten im Bereich "Verwaltung" ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Schritt 2: Erstellen einer einfachen geschachtelte Masterseite

`Simple.master`enthält zwei ContentPlaceHolder-Steuerelemente: die `MainContent` ContentPlaceHolder, die wir hinzugefügt, in das Web Form zusammen mit der `head` ContentPlaceHolder in die `<head>` Element. Wir würden eine Inhaltsseite zu erstellen und binden Sie es an `Simple.master` Inhaltsseite müssten zwei Content-Steuerelemente, die zwei ContentPlaceHolders verweisen. Auf ähnliche Weise, wenn wir eine geschachtelte Masterseite erstellen und binden Sie es an `Simple.master` geschachtelte Masterseite zwei Content-Steuerelemente haben.

Fügen Sie eine neue geschachtelte Masterseite der `NestedMasterPages` Ordner mit dem Namen `SimpleNested.master`. Mit der rechten Maustaste auf die `NestedMasterPages` Ordner, und wählen Sie Neues Element hinzufügen. Daraufhin wird das Dialogfeld "Neues Element hinzufügen" in Abbildung 2 dargestellt. Wählen Sie den Vorlagentyp Gestaltungsvorlage und den Namen der neuen Masterseite. Um anzugeben, dass die neue Masterseite einer geschachtelten Masterseite werden soll, das Kontrollkästchen Sie "Masterseite auswählen".

Klicken Sie anschließend auf die Schaltfläche "hinzufügen". Dadurch wird die gleiche SELECT-Anweisung eine Gestaltungsvorlage-Dialogfeld angezeigt, wenn die Bindung einer Inhaltsseite auf einer Masterseite angezeigt (siehe Abbildung 3). Wählen Sie die `Simple.master` Masterseite in der `NestedMasterPages` Ordner, und klicken Sie auf OK.

> [!NOTE]
> Wenn Sie Ihre ASP.NET-Website unter Verwendung des Webanwendungsprojekt-Modells anstelle des Modells für die Website-Projekt erstellt haben, sehen Sie nicht das Kontrollkästchen "Masterseite auswählen" im Dialogfeld "Neues Element hinzufügen" in Abbildung 2 dargestellt. Um eine geschachtelte Masterseite zu erstellen, wenn das Webanwendungsprojekt-Modell verwendet, müssen Sie die Vorlage geschachtelte Gestaltungsvorlage (anstelle der Gestaltungsvorlage-Vorlage) auswählen. Nach Auswahl der Gestaltungsvorlage geschachtelt-Vorlage, und klicken Sie auf Hinzufügen, wählen Sie das gleiche eine Gestaltungsvorlage (Dialogfeld), die in Abbildung 3 gezeigt angezeigt wird.


[![Überprüfen Sie die &quot;Masterseite auswählen&quot; Kontrollkästchen, um eine geschachtelte Gestaltungsvorlage hinzufügen](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Abbildung 02**: Aktivieren Sie das Kontrollkästchen "Masterseite auswählen", um eine geschachtelte Gestaltungsvorlage hinzuzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image6.png))


[![Binden der geschachtelten Masterseite zur Simple.master Masterseite](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Abbildung 03**: binden die Masterseite geschachtelt, um die `Simple.master` Gestaltungsvorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image9.png))


Die geschachtelte Gestaltungsvorlage deklarativem Markup, die weiter unten gezeigte enthält zwei Content-Steuerelemente, die auf der obersten Ebene Masterseite zwei ContentPlaceHolder-Steuerelemente verweisen.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Mit Ausnahme der `<%@ Master %>` Richtlinie, anfängliche deklarativem Markup für die geschachtelte Gestaltungsvorlage ist identisch mit das Markup, das anfänglich generiert wird, wenn der gleichen Masterseite der obersten Ebene eine Inhaltsseite zu binden. Wie einer Inhaltsseite `<%@ Page %>` Richtlinie, die `<%@ Master %>` hier Richtlinie enthält eine `MasterPageFile` Attribut, das die geschachtelte Gestaltungsvorlage übergeordneten Masterseite angibt. Der Hauptunterschied zwischen der geschachtelten Masterseite und einer Inhaltsseite auf die gleiche auf oberster Ebene Masterseite gebunden ist, dass die geschachtelte Masterseite ContentPlaceHolder-Steuerelemente enthalten kann. Die geschachtelte Masterseite ContentPlaceHolder-Steuerelemente definieren, die Regionen, in denen die Inhaltsseiten Markup anpassen können.

Aktualisieren Sie diese geschachtelten Masterseite, sodass sie den Text "Hello, aus SimpleNested!" angezeigt im Inhaltssteuerelement, das entspricht, der `MainContent` ContentPlaceHolder-Steuerelement.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Klicken Sie nach dem Herstellen der Code, die geschachtelten Masterseite speichern, und fügen Sie dann eine neue Seite zu den `NestedMasterPages` Ordner mit dem Namen `Default.aspx`, und binden Sie es an die `SimpleNested.master` Masterseite. Beim Hinzufügen von dieser Seite werden Sie möglicherweise überrascht, dass sie keine Inhaltssteuerelemente enthält (siehe Abbildung 4). Inhaltsseite kann nur Zugriff auf seine *übergeordneten* Seite ContentPlaceHolders "master". `SimpleNested.master`enthält keine ContentPlaceHolder-Steuerelemente. aus diesem Grund kann keine an diese Masterseite gebundene Inhaltsseite Content-Steuerelemente enthalten.


[![Die neue Seite enthält keine Inhaltssteuerelemente](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Abbildung 04**: der neue Inhalt Seite enthält keine Inhaltssteuerelemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image12.png))


Müssen wir werden die geschachtelte Gestaltungsvorlage aktualisieren (`SimpleNested.master`) ContentPlaceHolder-Steuerelemente enthalten. In der Regel möchten die verschachtelten Gestaltungsvorlagen ein ContentPlaceHolder für jede ContentPlaceHolder wodurch dessen untergeordneten Gestaltungsvorlage oder Inhaltsseite zum Arbeiten mit der obersten Ebene Masterseite ContentPlaceHolder definiert, die von der übergeordneten Masterseite einschließen Steuerelemente.

Update der `SimpleNested.master` Masterseite seine zwei Inhaltssteuerelemente eine ContentPlaceHolder einschließt. Geben Sie die ContentPlaceHolder Steuerelemente den gleichen Namen wie das ContentPlaceHolder-Steuerelement, dem ihre Inhaltssteuerelement bezieht. Hinzufügen, also ein ContentPlaceHolder-Steuerelement namens `MainContent` steuern, auf den Inhalt `SimpleNested.master` , verweist der `MainContent` ContentPlaceHolder in `Simple.master`. Führen Sie die gleiche Botschaft über das Inhaltssteuerelement, das verweist auf die `head` ContentPlaceHolder.

> [!NOTE]
> Beim Benennen von ContentPlaceHolder-Steuerelemente in der geschachtelten Masterseite identisch mit der ContentPlaceHolders auf der obersten Ebene Masterseite sollten, ist diese Benennungskonvention Symmetrie nicht erforderlich. Sie können die ContentPlaceHolder-Steuerelemente in der geschachtelten Masterseite einen beliebigen Namen geben, den Ihnen gefallen. Allerdings es ist einfacher, daran zu denken, was ContentPlaceHolders entsprechen welche Bereiche der Seite, wenn mein auf oberster Ebene Gestaltungsvorlage und verschachtelte Gestaltungsvorlagen die gleichen Namen verwenden.


Nach dem Herstellen dieser Ergänzungen der `SimpleNested.master` der Masterseite deklarativem Markup sollte ähnlich der folgenden aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Löschen der `Default.aspx` Inhaltsseite soeben erstellt wurde, und klicken Sie dann erneut hinzufügen, binden an die `SimpleNested.master` Masterseite. Dieses Mal fügt Visual Studio auf zwei Inhaltssteuerelemente an die `Default.aspx`, verweisen auf die ContentPlaceHolders jetzt in definierten `SimpleNested.master` (siehe Abbildung 6). Fügen Sie den Text "Hello, von" default.aspx "!" in den Inhalt zu steuern, die auf die verwiesen wird `MainContent`.

Abbildung 5 zeigt die drei Entitäten, die hier - `Simple.master`, `SimpleNested.master`, und `Default.aspx` - und wie sie miteinander in Beziehung stehen. Wie das Diagramm zeigt, implementiert die geschachtelten Masterseite Inhaltssteuerelemente ContentPlaceHolder seines übergeordneten Elements. Wenn diese Bereiche auf der Seite Inhalt zugreifen kann müssen, muss die geschachtelte Gestaltungsvorlage eigene ContentPlaceHolders an die Inhaltssteuerelemente hinzufügen.


[![Die auf der obersten Ebene und geschachtelte Masterseiten diktieren Layout der Inhaltsseite](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Abbildung 05**: Masterseiten der obersten Ebene und geschachtelte diktieren der Inhaltsseite Layout ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image15.png))


Dieses Verhalten wird veranschaulicht, wie eine Inhaltsseite oder Masterseite nur seine übergeordneten Masterseite gesehen ist. Dieses Verhalten wird auch durch die Visual Studio-Designer angezeigt. Abbildung 6 zeigt den Designer für `Default.aspx`. Während der Designer deutlich zeigt, welche Bereiche aus der Inhaltsseite bearbeitet werden und welche Teile werden nicht, nicht es unterscheiden, was nicht bearbeitbare Bereiche aus der geschachtelten Masterseite sind und welche Bereiche der obersten Ebene Masterseite.


[![Inhalt Seite jetzt enthält Inhaltssteuerelemente, für die geschachtelte Gestaltungsvorlage ContentPlaceHolders](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Abbildung 06**: die Seite nun enthält Inhalte Inhaltssteuerelemente für die geschachtelte Masterseite ContentPlaceHolders ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Schritt 3: Hinzufügen einer zweiten einfache geschachtelte Gestaltungsvorlage

Der Vorteil der verschachtelte Gestaltungsvorlagen ist noch deutlicher, wenn es mehrere verschachtelte Gestaltungsvorlagen gibt. Um diesen Vorteil zu veranschaulichen, erstellen Sie eine andere geschachtelte Masterseite in der `NestedMasterPages` Ordner; der Name dieses neue geschachtelte Masterseite `SimpleNestedAlternate.master` und binden Sie es an die `Simple.master` Masterseite. Fügen Sie in der geschachtelten Masterseite zwei Inhaltssteuerelementen ContentPlaceHolder-Steuerelemente hinzu, wie in Schritt2. Fügen Sie auch den Text "Hello, aus SimpleNestedAlternate!" im Inhaltssteuerelement, das der obersten Ebene Gestaltungsvorlage entspricht `MainContent` ContentPlaceHolder. Nach diesen Änderungen sollte Ihre neue geschachtelte Masterseite deklarativem Markup etwa wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Erstellen Sie eine Seite mit dem Namen `Alternate.aspx` in der `NestedMasterPages` Ordner und binden Sie es an die `SimpleNestedAlternate.master` geschachtelte Masterseite. Fügen Sie den Text "Hello, über alternative!" im Inhaltssteuerelement, das entspricht `MainContent`. Abbildung 7 zeigt `Alternate.aspx` Wenn durch die Visual Studio-Designer angezeigt.


[![Alternate.aspx wird an die Gestaltungsvorlage SimpleNestedAlternate.master gebunden.](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Abbildung 07**: `Alternate.aspx` gebunden ist, um die `SimpleNestedAlternate.master` Gestaltungsvorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image21.png))


Vergleichen Sie die Designer in Abbildung 7 in den Designer in Abbildung 6. Beide Inhaltsseiten gemeinsam dasselbe Layout in der obersten Ebene Masterseite definiert (`Simple.master`), d. h. auf den Titel "Nested Master Pages Lernprogramm (einfach)". Noch über die beiden unterschiedlichen Inhalt in ihrer übergeordneten Masterseiten - definiert den Text "Hello, aus SimpleNested!" in Abbildung 6 und "Hello, aus SimpleNestedAlternate!" Abbildung 7. Gewährt, diese Unterschiede sind einfach, jedoch könnten Sie in diesem Beispiel um aussagekräftigere unterschieden gehören erweitern. Für die Instanz, die `SimpleNested.master` Seite möglicherweise ein Menü mit den Optionen für die Inhaltsseiten enthalten, während `SimpleNestedAlternate.master` möglicherweise Informationen zu den Inhaltsseiten, die keine Bindung herstellen.

Angenommen Sie, dass es erforderlich, um das Layout der übergeordnete Standort eine Änderung vorzunehmen. Angenommen Sie, dass alle Inhaltsseiten eine Liste mit allgemeinen Links hinzugefügt werden sollten. Um wir die Gestaltungsvorlage der obersten Ebene ist, aktualisieren Sie dazu `Simple.master`. Gibt es keine Änderungen werden sofort in die verschachtelten Gestaltungsvorlagen und durch Erweiterung auch ihre Inhaltsseiten dargestellt.

Um die Einfachheit demonstrieren, mit denen wir das Layout der übergeordnete Standort ändern können, öffnen Sie die `Simple.master` Masterseite und fügen Sie das folgende Markup zwischen den `topContent` und `mainContent` `<div>` Elemente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Dies fügt zwei Links am oberen Rand jeder Seite, die an bindet `Simple.master`, `SimpleNested.master`, oder `SimpleNestedAlternate.master`; diese Änderungen gelten für alle verschachtelte Gestaltungsvorlagen und deren Inhaltsseiten sofort. Abbildung 8 zeigt `Alternate.aspx` über einen Browser angezeigt. Beachten Sie die Verknüpfungen am oberen Rand der Seite "(verglichen mit Abbildung 7).


[![In der Master-Seite auf oberster Ebene geändert werden sofort in seinen Masterseiten geschachtelt und deren Inhaltsseiten dargestellt](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Abbildung 08**: in der Master-Seite auf oberster Ebene geändert werden sofort in seinen Masterseiten geschachtelt und deren Inhaltsseiten dargestellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Verwenden einer geschachtelten Masterseite für den Bereich "Verwaltung"

An diesem Punkt haben erläutert, die Vorteile der geschachtelten Master Seiten und gezeigt, wie das Erstellen und in einer ASP.NET-Anwendung zu verwenden. In den Beispielen in den Schritten 1, 2 und 3, beteiligt jedoch das Erstellen einer neuen Masterseite der obersten Ebene, neue verschachtelte Gestaltungsvorlagen und neue Inhaltsseiten. Was Hinzufügen einer neuen geschachtelten Masterseite an eine Website mit einer vorhandenen Masterseite der obersten Ebene und Inhaltsseiten?

Integrieren von einer geschachtelten Masterseite in einer vorhandenen Website und Zuordnen zu vorhandenen Inhaltsseiten erfordert etwas mehr Aufwand als von Grund auf neu starten. Die Schritte 4, 5, 6 und 7 dieser Herausforderung untersuchen, wie wir unsere demoanwendung einschließen eine neue geschachtelten Masterseite mit dem Namen ergänzen `AdminNested.master` , wird von der ASP.NET-Seiten verwendet und enthält Anweisungen für den Administrator der `~/Admin` Ordner.

Integrieren von einer geschachtelten Masterseite in unserer demoanwendung führt folgende Nachteile:

- Der vorhandene Inhalt Seiten der `~/Admin` Ordner vorhanden sein, bestimmte Erwartungen aus ihren Masterseite. Zunächst einmal, erwarten sie bestimmte Steuerelemente ContentPlaceHolder vorhanden sein. Darüber hinaus die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten rufen die Gestaltungsvorlage öffentlichen `RefreshRecentProductsGrid` festgelegt ist, dessen `GridMessageText` -Eigenschaft, oder haben Sie einen Ereignishandler für seine `PricesDoubled` Ereignis. Unsere geschachtelte Masterseite muss daher den gleichen ContentPlaceHolders und die öffentlichen Member bereitstellen.
- In dem vorherigen Lernprogramm wir verbessert die `BasePage` Klasse dynamisch Festlegen der `Page` des Objekts `MasterPageFile` -Eigenschaft basierend auf einer Sitzungsvariablen. Wie auf wir dynamische Masterseiten unterstützen, wenn verschachtelte Gestaltungsvorlagen?

Diese beiden Probleme werden Oberfläche, wie wir die geschachtelte Gestaltungsvorlage erstellen und sie unsere vorhandenen Inhaltsseiten verwenden. Wir untersuchen und Leistungsbremsen diese Probleme, sobald sie auftreten.

## <a name="step-4-creating-the-nested-master-page"></a>Schritt 4: Erstellen der geschachtelten Masterseite

Unsere erste Aufgabe besteht darin verschachtelte Masterseite von Seiten in den Bereich "Verwaltung" verwendet werden zu erstellen. Wie in Schritt2 veranschaulichte Filtermenü beim Hinzufügen eines neuen Masterseite geschachtelt müssen wir die geschachtelte Gestaltungsvorlage übergeordneten Masterseite angeben. Jedoch können wir zwei der obersten Ebene Masterseiten: `Site.master` und `Alternate.master`. Wie bereits erwähnt, die es erstellt `Alternate.master` in dem vorherigen Lernprogramm und schrieb Code in der `BasePage` -Klasse diesem Satz der `Page` des Objekts `MasterPageFile` Eigenschaft zur Laufzeit, um entweder `Site.master` oder `Alternate.master` abhängig vom Wert der `MyMasterPage` Sitzungsvariablen.

Wie konfiguriere wir unsere geschachtelte Masterseite, sodass die entsprechenden auf oberster Ebene Gestaltungsvorlage verwendet? Wir haben zwei Optionen:

- Erstellen Sie zwei verschachtelte Gestaltungsvorlagen `AdminNestedSite.master` und `AdminNestedAlternate.master`, und binden diese an der obersten Ebene Masterseiten `Site.master` und `Alternate.master`zugeordnet. In `BasePage`, anschließend legen wir den `Page` des Objekts `MasterPageFile` der entsprechenden geschachtelten Masterseite.
- Erstellen Sie eine einzelne geschachtelte Masterseite und haben Sie die Inhaltsseiten dieser bestimmten Masterseite verwenden. Klicken Sie dann zur Laufzeit, müssten die geschachtelte Gestaltungsvorlage festgelegt `MasterPageFile` Eigenschaft, um die entsprechenden auf oberster Ebene Gestaltungsvorlage zur Laufzeit. (Wie Sie inzwischen herausgefunden haben können, haben Masterseiten auch eine `MasterPageFile` Eigenschaft.)

Die zweite Option ermöglicht die Verwendung. Erstellen Sie eine einzelne geschachtelte Masterseitendatei in der `~/Admin` Ordner mit dem Namen `AdminNested.master`. Da beide `Site.master` und `Alternate.master` haben den gleichen Satz von ContentPlaceHolder-Steuerelemente, die es spielt keine Rolle Sie, welche Masterseite binden, obwohl ich Sie gebunden fördern `Site.master` aus Gründen der Konsistenz.


[![Hinzufügen einer geschachtelten Masterseite in den Ordner ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Abbildung 09**: Hinzufügen einer geschachtelten Masterseite, um die `~/Admin` Ordner. ([Klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image27.png))


Da die geschachtelte Gestaltungsvorlage auf einer Masterseite mit vier ContentPlaceHolder Steuerelemente gebunden ist, fügt Visual Studio vier Inhaltssteuerelemente zum anfänglichen Markup für die neue geschachtelte Masterseite-Datei. Wie wir in den Schritten 2 und 3 ein ContentPlaceHolder-Steuerelement in jedes Inhaltssteuerelement hinzufügen, und geben sie Ihnen den gleichen Namen wie der obersten Ebene Masterseite ContentPlaceHolder-Steuerelement angezeigt. Fügen Sie das folgende Markup auch an das Inhaltssteuerelement, das entspricht, der `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Als Nächstes definieren Sie die `instructions` CSS-Klasse der `Styles.css` und `AlternateStyles.css` CSS-Dateien. Die folgenden CSS-Regeln dazu führen, dass HTML-Elementen erstellt, in denen die `instructions` Klasse mit einer hellen Gelb Hintergrundfarbe und einen schwarzen Rahmen durchgezogene angezeigt werden:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Da geschachtelte Masterseite dieses Markup hinzugefügt wurde, wird es nur in Webseitenbesucher angezeigt, die diese geschachtelten Gestaltungsvorlage (d. h., die Seiten in den Bereich "Verwaltung") verwenden.

Nach dem Herstellen dieser Ergänzungen der geschachtelten Masterseite, sollte ihre deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Beachten Sie, dass jedes Inhaltssteuerelement ContentPlaceHolder-Steuerelement und, verfügt die ContentPlaceHolder-Steuerelemente `ID` Eigenschaften werden die gleichen Werte wie die entsprechenden ContentPlaceHolder-Steuerelemente in der master-Seite auf oberster Ebene zugewiesen. Darüber hinaus das Verwaltung Abschnitt-spezifische Markup wird angezeigt, der `MainContent` ContentPlaceHolder.

Abbildung 10 zeigt die `AdminNested.master` geschachtelte Masterseite, wenn Sie über Visual Studio-Designer angezeigt. Sehen Sie die Anweisungen in das gelbe Kästchen am oberen Rand der `MainContent` Inhaltssteuerelement.


[![Der geschachtelte Masterseite Erweitert auf oberster Ebene Gestaltungsvorlage Einbeziehung von Anweisungen für den Administrator.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Abbildung 10**: geschachtelte Gestaltungsvorlage Erweitert auf oberster Ebene Gestaltungsvorlage Einbeziehung von Anweisungen für den Administrator. ([Klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Schritt 5: Aktualisieren der vorhandenen Inhaltsseiten Verwendung der neuen geschachtelten Masterseite

Jedes Mal, wenn wir den Bereich "Verwaltung" eine neue Seite hinzufügen wir sie zum Binden müssen der `AdminNested.master` Masterseite soeben erstellt wurde. Aber was ist mit den vorhandenen Inhaltsseiten? Alle Inhaltsseiten am Standort derzeit abgeleitet werden, aus der `BasePage` -Klasse, die Masterseite der Inhaltsseite programmgesteuert zur Laufzeit festgelegt. Dies ist nicht die für die Content-Seiten in den Bereich "Verwaltung" gewünschte Verhalten. Stattdessen werden diese Inhaltsseiten immer verwenden soll die `AdminNested.master` Seite. Es werden die Verantwortung der geschachtelten Masterseite rechts auf der obersten Ebene Inhaltsseite zur Laufzeit auswählen.

Um die beste Möglichkeit, um zu erreichen dies gewünscht Verhalten ist zum Erstellen einer neuen benutzerdefinierten Basisseite-Klasse, die mit dem Namen `AdminBasePage` , reicht die `BasePage` Klasse. `AdminBasePage`überschreiben können Sie dann die `SetMasterPageFile` und legen Sie die `Page` des Objekts `MasterPageFile` auf den hartcodierten Wert "~ / Admin/AdminNested.master". Auf diese Weise eine andere Seite, die abgeleitet `AdminBasePage` verwenden `AdminNested.master`, während eine andere Seite, die abgeleitet `BasePage` hat seine `MasterPageFile` Eigenschaftensatz dynamisch entweder "~ / Site.master" oder "~ / Alternate.master" basierend auf dem Wert von der `MyMasterPage` Sitzungsvariablen.

Starten, indem Sie eine neue Klassendatei zum Hinzufügen der `App_Code` Ordner mit dem Namen `AdminBasePage.vb`. Haben `AdminBasePage` erweitern `BasePage` und überschreiben Sie dann die `SetMasterPageFile` Methode. Weisen Sie in dieser Methode die `MasterPageFile` den Wert "~ / Admin/AdminNested.master". Nach diesen Änderungen Ihrer Klasse sollte Datei etwa wie folgt aussehen:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Jetzt müssen wir den vorhandenen Inhalt Seiten in der Verwaltung Abschnitt abgeleitet `AdminBasePage` anstelle von `BasePage`. Wechseln Sie in der Code-Behind-Klassendatei für jede Seite Inhalt in die `~/Admin` Ordner und diese Änderung vornehmen. Beispielsweise ist in `~/Admin/Default.aspx` ändern Sie die Deklaration der Code-Behind-Klasse aus:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Nach:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Abbildung 11 zeigt wie der obersten Ebene Gestaltungsvorlage (`Site.master` oder `Alternate.master`), die geschachtelte Gestaltungsvorlage (`AdminNested.master`), und die Verwaltung Abschnitt Inhaltsseiten in Beziehung stehen miteinander.


[![Der geschachtelte Masterseite definiert spezifische Inhalte zu den Seiten im Bereich "Verwaltung"](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Abbildung 11**: die geschachtelte Master Seite definiert Content speziell für die Seiten im Bereich "Verwaltung" ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Schritt 6: Spiegelung, öffentliche Methoden und Eigenschaften der Gestaltungsvorlage

Bedenken Sie, dass die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten zur programmgesteuerten Interaktion mit der Masterseite: `~/Admin/AddProduct.aspx` Aufrufe die Masterseite der öffentliche `RefreshRecentProductsGrid` -Methode und legt dessen `GridMessageText` Eigenschaft; `~/Admin/Products.aspx` verfügt über einen Ereignishandler für das `PricesDoubled` Ereignis. In dem vorherigen Lernprogramm erstellt es einen `MustInherit` `BaseMasterPage` Klasse, die diese öffentlichen Member definiert.

Die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten wird davon ausgegangen, dass ihre Masterseite abgeleitet der `BaseMasterPage` Klasse. Die `AdminNested.master` Seite jedoch derzeit erweitert die `System.Web.UI.MasterPage` Klasse. Als Ergebnis wird beim Zugriff auf `~/Admin/Products.aspx` ein `InvalidCastException` wird mit der folgenden Meldung ausgelöst: "Cast-Objekt des Typs kann nicht" ASP.admin\_Adminnested\_master ", geben Sie"BaseMasterPage"."

Um dies zu beheben, wir müssen, die `AdminNested.master` Code-Behind-Klasse erweitern `BaseMasterPage`. Aktualisieren Sie die geschachtelte Gestaltungsvorlage Code-Behind-Klassendeklaration aus:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Nach:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Wir sind noch nicht erfolgt. Wir müssen die gekennzeichnete Member überschreiben `MustOverride`, nämlich `RefreshRecentProductsGrid` und `GridMessageText`. Diese Member werden von der obersten Ebene Masterseiten verwendet, um ihren Benutzeroberflächen zu aktualisieren. (Tatsächlich nur die `Site.master` Gestaltungsvorlage verwendet diese Methoden aus, obwohl beide auf der obersten Ebene Masterseiten diese Methoden implementieren, da beide erweitern `BaseMasterPage`.)

Während dieser Elemente implementieren müssen `AdminNested.master`, diese Implementierungen müssen lediglich rufen Sie das gleiche Element einfach in die obersten Ebene Masterseite von geschachtelten Gestaltungsvorlage verwendet wird. Für die Instanz, wenn eine Seite im Bereich "Verwaltung" aufruft, der geschachtelte Gestaltungsvorlage `RefreshRecentProductsGrid` -Methode, die geschachtelte Gestaltungsvorlage muss, lediglich, wiederum, rufen Sie `Site.master` oder `Alternate.master`des `RefreshRecentProductsGrid` Methode.

Um dies zu erreichen, starten, indem Sie den folgenden Code `@MasterType` -Direktive am Anfang `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Bedenken Sie, dass die `@MasterType` Richtlinie fügt eine stark typisierte Eigenschaft für die Code-Behind-Klasse, die mit dem Namen `Master`. Überschreiben Sie dann die `RefreshRecentProductsGrid` und `GridMessageText` Elemente und einfach delegieren den Aufruf der `Master`die entsprechende Methode:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Mit diesem Code werden werden besuchen, und verwenden die Inhaltsseiten in den Bereich "Verwaltung" angezeigt. Abbildung 12 zeigt die `~/Admin/Products.aspx` Seite, wenn Sie über einen Browser angezeigt. Wie Sie sehen können, enthält die Seite das Kontrollkästchen Verwaltung Anweisungen, die in der geschachtelten Masterseite definiert ist.


[![Inhaltsseiten in den Bereich "Verwaltung" werden die Anweisungen am oberen Rand jeder Seite](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Abbildung 12**: die Inhaltsseiten in den Verwaltung Abschnitt enthalten Anweisungen bei der jede Seitenanfang ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Schritt 7: Von der entsprechenden auf oberster Ebene Masterseite zur Laufzeit

Während alle Inhaltsseiten in den Bereich "Verwaltung" voll funktionsfähig sind, alle verwenden die gleiche auf oberster Ebene Gestaltungsvorlage und ignorieren die vom Benutzer am ausgewählten Masterseite `ChooseMasterPage.aspx`. Dieses Verhalten ist darauf zurückzuführen, dass die geschachtelte Masterseite hat seine `MasterPageFile` Eigenschaft statisch festgelegt werden, um `Site.master` in seiner `<%@ Master %>` Richtlinie.

Verwenden der obersten Ebene ausgewählte, durch den Endbenutzer wir festlegen müssen Masterseite der `AdminNested.master`des `MasterPageFile` Eigenschaft auf den Wert in der `MyMasterPage` Sitzungsvariablen. Da wir die Inhaltsseiten festgelegt `MasterPageFile` Eigenschaften im `BasePage`, Sie eventuell der Meinung, dass wir der geschachtelte Gestaltungsvorlage festgelegt würden `MasterPageFile` Eigenschaft im `BaseMasterPage` oder in der `AdminNested.master`des Code-Behind-Klasse. Dies funktioniert nicht, aber da müssen wir festgelegt haben die `MasterPageFile` Eigenschaft am Ende der PreInit-Phase. Die früheste Uhrzeit, die wir programmgesteuert, in den Lebenszyklus der Seite aus einer Masterseite nutzen können ist die Init-Phase (die nach der PreInit-Phase tritt auf).

Aus diesem Grund müssen die geschachtelte Gestaltungsvorlage festgelegt `MasterPageFile` Eigenschaft aus der Inhaltsseiten. Nur Seiten, die die `AdminNested.master` Masterseite abgeleitet `AdminBasePage`. Daher können wir diese Logik dort abgelegt. In Schritt 5 wir überschrieben hat die `SetMasterPageFile` -Methode, die Page-Objekt festlegen `MasterPageFile` -Eigenschaft auf "~ / Admin/AdminNested.master". Update `SetMasterPageFile` auch der Gestaltungsvorlage festgelegt `MasterPageFile` Eigenschaft, um das Ergebnis in der Sitzung gespeichert:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

Die `GetMasterPageFileFromSession` -Methode, die wir hinzugefügt, die `BasePage` Klasse in der vorherigen Lernprogramm gibt den Pfad der entsprechenden Masterseite auf der Sitzung Variablenwert Grundlage.

Führt durch diese Änderung vorhanden die Auswahl des Benutzers Masterseite über auf den Bereich "Verwaltung". Abbildung 13 zeigt dieselbe Seite als Abbildung 12, aber nachdem der Benutzer geändert wurde, ihre Auswahl Masterseite `Alternate.master`.


[![Die geschachtelte Verwaltungsseite verwendet die vom Benutzer ausgewählten Master-Seite auf oberster Ebene](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Abbildung 13**: die Nested-Verwaltungsseite verwendet, die obersten Ebene Master Seite vom Benutzer ausgewählten ([klicken Sie hier, um das Bild in voller Größe angezeigt](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Zusammenfassung

Viele like wie Inhaltsseiten an einer Masterseite binden können, ist es möglich, erstellen Sie geschachtelte Masterseiten durch die Verwendung einer Masterseite untergeordneten zu einer übergeordneten Masterseite binden. Die untergeordnete Masterseite möglicherweise Inhaltssteuerelemente aller übergeordneten ContentPlaceHolders definieren; Sie können eigene ContentPlaceHolder-Steuerelemente (ebenso wie andere Markup) klicken Sie dann diese Inhaltssteuerelemente hinzufügen. Verschachtelte Gestaltungsvorlagen sind sehr nützlich in großen Webanwendungen, in dem alle Seiten gemeinsam eine übergreifende Aussehen und Verhalten noch bestimmte Abschnitte des Standorts müssen eindeutige angepasst.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Geschachtelte ASP.NET Masterseiten](https://msdn.microsoft.com/en-us/library/x2b3ktt7.aspx)
- [Tipps für verschachtelte Gestaltungsvorlagen und zur Entwurfszeit für Visual Studio 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Visual Studio 2008 geschachtelt Master Unterstützung von Codepages](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](specifying-the-master-page-programmatically-vb.md)
