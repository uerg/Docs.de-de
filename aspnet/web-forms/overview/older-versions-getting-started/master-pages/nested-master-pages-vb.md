---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Geschachtelte Masterseiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Zeigt, wie eine Masterseite innerhalb einer anderen geschachtelt werden.
ms.author: aspnetcontent
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e58eef87afce7d1b7e29445bdbf48baf5f65d3e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836261"
---
<a name="nested-master-pages-vb"></a>Geschachtelte Masterseiten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Zeigt, wie eine Masterseite innerhalb einer anderen geschachtelt werden.


## <a name="introduction"></a>Einführung

Im Laufe der vergangenen neun Tutorials haben wir gesehen, wie Sie einen websiteweiten Layouts mit Masterseiten implementieren. Kurz gesagt, erlauben Sie Masterseiten, Entwickler der Seite, um allgemeine Markup auf der Masterseite sowie bestimmte Regionen zu definieren, die auf einer Inhaltsseite-Seite-an-Content-Basis angepasst werden kann. Die ContentPlaceHolder-Steuerelemente auf einer Masterseite anzugeben, die anpassbare Regionen; das benutzerdefinierte Markup für die ContentPlaceHolder-Steuerelemente sind in der Inhaltsseite über ContentControl-Elemente definiert.

Die Masterseite-Techniken, die bisher haben wir untersucht haben eignen sich hervorragend, wenn Sie einem einzelnen Layout für die gesamte Website verwendet haben. Viele große Websites müssen jedoch ein Sitelayout, das angepasst wird, über die verschiedenen Abschnitte. Betrachten Sie beispielsweise eine health care-Anwendung durch Krankenhausmitarbeiter in zum Verwalten von Patientendaten, Aktivitäten und die Abrechnung verwendet. Möglicherweise gibt es drei Arten von Webseiten in dieser Anwendung:

- Mitarbeiter Member-spezifische Seiten, in denen Mitarbeiter Verfügbarkeit aktualisieren können, Zeitpläne anzeigen, oder fordern Urlaub.
- Patienten-spezifische Seiten, in denen Mitarbeiter anzeigen oder Bearbeiten von Informationen für einen bestimmten Patienten.
- Abrechnung-spezifische Seiten, in dem Accountants aktuelle überprüfen, Anspruch Statusangaben und Finanzberichte.

Ein gebräuchliches Layout, z. B. ein Menü im oberen Bereich und eine Reihe von häufig verwendeten Links am unteren Rand teilen jeder Seite. Die Mitarbeiter, Patienten und Abrechnung-spezifische Seiten müssen jedoch möglicherweise diese allgemeinen Layouts anpassen. Beispielsweise sollte z. B. alle Mitarbeiter-spezifische Seiten eine Calendar "und" Task-Liste, die mit der Verfügbarkeit und einen täglichen Zeitplan des aktuell angemeldeten Benutzers enthalten. Möglicherweise müssen alle Patienten-spezifische Seiten angezeigt werden, der Name, Adresse und Informationen zur Versicherung für den Patienten, deren Informationen bearbeitet wird.

Es ist möglich, eine solche benutzerdefinierte Layouts erstellen, indem *geschachtelte Masterseiten*. Um das oben beschriebene Szenario zu implementieren, würden wir erstellen Sie zunächst eine Masterseite, die die websiteweite Inhalte Layout, das Menü und die Fußzeile mit ContentPlaceHolder-Steuerelemente die anpassbaren Bereiche definieren definiert. Wir erstellen nun drei verschachtelte Gestaltungsvorlagen, eine für jede Webseite. Jede geschachtelte Masterseite würde es sich um den Inhalt zwischen den Typ der Inhaltsseiten definieren, die die Masterseite verwenden. Das heißt, würde die geschachtelten Masterseite für Inhaltsseiten Patienten-spezifische Markup und programmgesteuerte Logik für die Anzeige von Informationen zu den Patienten, die bearbeitet wird enthalten. Beim Erstellen einer neuen Patienten-spezifische Seite würden wir es an dieser geschachtelten Masterseite binden.

In diesem Lernprogramm beginnt mit der die Vorteile der verschachtelte Gestaltungsvorlagen hervorheben. Es veranschaulicht klicken Sie dann zum Erstellen und Verwenden von geschachtelten Masterseiten.

> [!NOTE]
> Geschachtelte Masterseiten sind seit Version 2.0 von .NET Framework möglich. Visual Studio 2005 jedoch nicht in die entwurfszeitunterstützung für verschachtelte Gestaltungsvorlagen enthalten. Die gute Nachricht ist, dass umfassende Funktionen zur Entwurfszeit für verschachtelte Gestaltungsvorlagen, Visual Studio 2008 bietet. Wenn Sie mit geschachtelten Masterseiten möchten, aber weiterhin Visual Studio 2005 verwenden, sehen Sie sich [Scott Guthrie](https://weblogs.asp.net/scottgu/)Blogeintrag [Tipps für geschachtelte Masterseiten in Visual Studio 2005-Design-Time-](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Die Vorteile der geschachtelte Masterseiten

Viele Websites ist eine übergeordnete Design der Website sowie mehr benutzerdefinierte Designs für bestimmte Arten von Seiten. Unsere Demo-Webanwendung haben wir beispielsweise eine rudimentäre Verwaltungsabschnitt erstellt (die Seiten in der `~/Admin` Ordner). Derzeit den Webseiten in der `~/Admin` Ordner verwenden dieselbe Masterseite als diese Seiten nicht in den Verwaltungsabschnitt (nämlich `Site.master` oder `Alternate.master`, je nachdem, auf die Auswahl des Benutzers).

> [!NOTE]
> Nehmen Sie jetzt, dass Sie unsere Website nur eine Masterseite verfügt über `Site.master`. Wir behandeln die verschachtelte Gestaltungsvorlagen mit zwei (oder mehr) Masterseiten, beginnend mit "Verwenden einer geschachtelten Master für die Verwaltung Seitenabschnitt" weiter unten in diesem Tutorial verwenden.


Angenommen Sie, wir aufgefordert wurden, passen Sie das Layout der Verwaltungsseiten sollen zusätzliche Informationen oder Links, die andernfalls nicht in den anderen Seiten der Website vorhanden wäre. Es gibt vier Verfahren zum Implementieren dieser Anforderung:

1. Manuell hinzufügen Verwaltung-spezifische Informationen und Links auf jeder Seite Inhalt in die `~/Admin` Ordner.
2. Update der `Site.master` Masterseite Verwaltung Abschnitt-spezifische Informationen und Links, und fügen Sie Code zur Masterseite anzeigen oder Ausblenden in diesen Abschnitten basierend an, ob eines der Verwaltungsseiten besucht wurde.
3. Erstellen Sie eine neue Masterseite speziell für den Verwaltungsabschnitt, kopieren Sie das Markup aus `Site.master`, fügen Sie die Verwaltung im Abschnitt-spezifische Informationen und Links, und aktualisieren Sie dann auf die Inhaltsseiten in die `~/Admin` Ordner mit diesem neuen Master Seite ".
4. Erstellen einer geschachtelten Masterseite, an die gebunden `Site.master` und die Inhaltsseiten der `~/Admin` Ordner verwenden, die diese neue von geschachtelten Masterseite. Diese geschachtelte Masterseite würde nur die zusätzliche Informationen und Links, die spezifisch für die Administration-Seiten enthalten und müssen nicht das Markup in bereits definiert. wiederholen die `Site.master`.

Die erste Option ist die am wenigsten angenehm. Der ganze Sinn der Verwendung von Masterseiten werden Abkehr von müssen manuell kopieren und fügen die allgemeine Markup für ASP.NET-Seiten. Die zweite Option ist akzeptabel, jedoch wird die Anwendung zu schlechter zu verwalten, wie es hebt Einrichten der master-Seiten mit Markup an, die nur gelegentlich angezeigt wird, und müssen Entwickler die Masterseite bearbeiten, umgehen dieses Markup und wann merken müssen, bestimmte Markup wird genau im Vergleich zu angezeigt, wenn es ausgeblendet ist. Dieser Ansatz wäre weniger Hauptfeuerlöschpumpe Anpassungen von mehr Typen von Webseiten, die durch diese einzige masterdseite untergebracht werden musste.

Die dritte Option entfernt der Masse abheben und Komplexität Probleme bei der zweiten Option die eingeblendet. Option drei der größte Nachteil ist jedoch, dass es erforderlich ist, kopieren Sie das allgemeine Layout von `Site.master` auf die neue Verwaltung Abschnitt-spezifische-Masterseite. Wenn es sich später entscheiden, ändern Sie das Layout der Website-Ebene müssen wir denken Sie daran, die sie an zwei Orten zu ändern.

Die vierte Option, geschachtelte Masterseiten, senden Sie uns die besten Optionen für die zweite und dritte. Die websiteweiten Layouts-Informationen werden in einer Datei: die Seite der obersten Ebene master - beibehalten, während die Inhalte, die spezifisch für bestimmte Regionen in verschiedene Dateien verteilt ist.

Dieses Tutorial beginnt mit einem Blick auf das Erstellen und verwenden eine einfache geschachtelte Masterseite. Wir erstellen eine völlig neue Masterseite der obersten Ebene, zwei geschachtelte Masterseiten und Inhaltsseiten für zwei. Beginnend mit "Mithilfe einer geschachtelten Master Seite für die Verwaltungsabschnitt", betrachten wir aktualisieren unsere vorhandenen Masterseite-Architektur, um die Verwendung von geschachtelten Masterseiten einzuschließen. Genauer gesagt erstellen wir eine geschachtelte Masterseite und verwenden, um die zusätzlichen benutzerdefinierten Inhalt für die Inhaltsseiten in umfassen die `~/Admin` Ordner.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Schritt 1: Erstellen einer einfachen auf oberster Ebene Masterseite

Erstellen einem Masterauftrag für den geschachtelten basierend auf einer vorhandenen Master Pages und anschließendes Aktualisieren einer vorhandenen Inhalt Seite, um diese neue geschachtelte Masterseite anstelle der obersten Ebene Masterseite zu verwenden ist gewisse Komplexität verbunden, da die vorhandenen Inhaltsseiten bereits bestimmte erwarten, dass ContentPlaceHolder-Steuerelemente, die in der obersten Ebene Masterseite definiert. Aus diesem Grund muss die geschachtelte Masterseite auch die gleichen ContentPlaceHolder-Steuerelemente mit den gleichen Namen enthalten. Darüber hinaus unsere demoanwendung bestimmten hat zwei Masterseiten (`Site.master` und `Alternate.master`) zugewiesen sind, dynamisch auf einer Inhaltsseite auf Grundlage von Präferenzen des Benutzers, die diese Komplexität weiter hinzufügt. Betrachten wir die vorhandene Anwendung mit geschachtelten Masterseiten weiter unten in diesem Tutorial wird aktualisiert, aber lassen Sie uns zuerst darauf konzentrieren, ein einfaches geschachtelte Masterseiten-Beispiel.

Erstellen Sie einen neuen Ordner namens `NestedMasterPages` und fügen Sie eine neue Masterseitendatei klicken Sie dann zu diesem Ordner mit dem Namen `Simple.master`. (Siehe Abbildung 1 für einen Screenshot des Projektmappen-Explorer nach dem dieser Ordner und die Datei hinzugefügt wurden.) Ziehen Sie die `AlternateStyles.css` Stylesheetdatei Projektmappen-Explorer auf den Designer. Dadurch wird eine `<link>` Element der Stylesheetdatei in der `<head>` Element, nach dem der Masterseite `<head>` der Elementknoten dem Elementmarkup sollte wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Fügen Sie das folgende Markup im Web Form `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Dieses Markup zeigt eine Verknüpfung mit dem Titel "Geschachtelte Masterseiten (einfach)" am oberen Rand der Seite in großer Schriftart auf dunkelblaue Hintergrund weiß. Ist die `MainContent` ContentPlaceHolder. Abbildung 1 zeigt die `Simple.master` Masterseite, die beim Laden in Visual Studio-Designer.


[![Der geschachtelte Masterseite definiert Content speziell zu den Seiten im Bereich Verwaltung](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Abbildung 01**: die geschachtelte Master Seite definiert Inhalt für die Seiten im Bereich Verwaltung ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Schritt 2: Erstellen einer einfachen, geschachtelten Masterseite

`Simple.master` enthält zwei ContentPlaceHolder-Steuerelemente: die `MainContent` ContentPlaceHolder, die wir hinzugefügt, in das Web Form zusammen mit den `head` ContentPlaceHolder in die `<head>` Element. Wenn wir eine Inhaltsseite erstellen und binden Sie es an `Simple.master` Inhaltsseite müssten zwei Inhaltssteuerelemente, die zwei ContentPlaceHolder-Steuerelemente verweisen. Auf ähnliche Weise, wenn wir eine geschachtelte Masterseite zu erstellen und binden Sie es an `Simple.master` müssen die geschachtelten Masterseite zwei Inhaltssteuerelemente.

Fügen Sie eine neue geschachtelte Masterseite, die `NestedMasterPages` Ordner mit dem Namen `SimpleNested.master`. Mit der rechten Maustaste auf die `NestedMasterPages` Ordner, und wählen Sie Neues Element hinzufügen. Dadurch wird das Dialogfeld "Neues Element hinzufügen" in Abbildung 2 dargestellt. Wählen Sie die Masterseite Vorlagentyp und geben Sie den Namen der neuen Masterseite. Um anzugeben, dass die neue Masterseite einer geschachtelten Masterseite werden soll, überprüfen Sie das Kontrollkästchen "Masterseite auswählen".

Klicken Sie dann auf die Schaltfläche "hinzufügen". Dadurch wird die gleiche SELECT-Anweisung eine Masterseite-Dialogfeld wird angezeigt, wenn die Bindung von einer Inhaltsseite auf eine Gestaltungsvorlage angezeigt (siehe Abbildung 3). Wählen Sie die `Simple.master` Masterseite in der `NestedMasterPages` Ordner, und klicken Sie auf OK.

> [!NOTE]
> Wenn Sie Ihre ASP.NET-Website das Webanwendungsprojekt-Modell verwenden, anstatt das Websiteprojekt-Modell erstellt haben, sehen Sie nicht das Kontrollkästchen "Masterseite auswählen", in das Dialogfeld "Neues Element hinzufügen" in Abbildung 2 dargestellt. Um eine geschachtelte Masterseite zu erstellen, wenn Sie das Webanwendungsprojekt-Modell verwenden, müssen Sie die geschachtelte Masterseite-Vorlage (anstelle der Masterseitenvorlage) auswählen. Nach dem Auswählen der Masterseite geschachtelte Vorlage, und klicken Sie auf Hinzufügen, wählen Sie den gleichen eine Masterseite, die in Abbildung 3 dargestellte Dialogfeld angezeigt wird.


[![Überprüfen Sie die &quot;Masterseite auswählen&quot; Kontrollkästchen, um eine geschachtelte Masterseite hinzufügen](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Abbildung 02**: Aktivieren Sie das Kontrollkästchen "Masterseite auswählen" zum Hinzufügen einer geschachtelten Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image6.png))


[![Binden Sie die geschachtelte Masterseite zur Simple.master Masterseite](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Abbildung 03**: Binden von geschachtelten Masterseite der `Simple.master` Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image9.png))


Die geschachtelte Masterseite deklarativen Markup, wie unten beschrieben, enthält zwei Inhaltssteuerelemente verweisen auf der obersten Ebene der Masterseite zwei ContentPlaceHolder-Steuerelemente.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Mit Ausnahme der `<%@ Master %>` -Anweisung anfängliche deklarativen Markup für die geschachtelte Masterseite ist identisch mit das Markup, das ursprünglich generiert wird, wenn eine Inhaltsseite auf die obersten Ebene derselben Masterseite zu binden. Wie einer Inhaltsseite `<%@ Page %>` Richtlinie, die `<%@ Master %>` hier Richtlinie enthält eine `MasterPageFile` Attribut, das die geschachtelte Masterseite übergeordnete Masterseite angibt. Der Hauptunterschied zwischen der geschachtelten Masterseite und einer Inhaltsseite auf die obersten Ebene derselben Masterseite gebunden ist, dass die geschachtelten Masterseite ContentPlaceHolder-Steuerelemente enthalten kann. Die geschachtelte Masterseite ContentPlaceHolder-Steuerelemente definieren, die Regionen, in denen die Inhaltsseiten der Markup anpassen können.

Aktualisieren Sie diese geschachtelten Masterseite, sodass es den Text "Hello, from SimpleNested!" anzeigt Klicken Sie in das Inhaltssteuerelement, das entspricht, dem `MainContent` ContentPlaceHolder-Steuerelement.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Treffen Sie diese Erweiterung, und speichern Sie die geschachtelte Masterseite und dann eine neue Seite zum Hinzufügen der `NestedMasterPages` Ordner mit dem Namen `Default.aspx`, und binden Sie es an der `SimpleNested.master` Masterseite. Beim Hinzufügen von dieser Seite ist Sie möglicherweise überrascht, dass sie keine Inhaltssteuerelemente enthält (siehe Abbildung 4). Eine Inhaltsseite kann nur Zugriff auf seine *übergeordneten* master-Seite ContentPlaceHolder-Steuerelemente. `SimpleNested.master` enthält keine ContentPlaceHolder-Steuerelemente. aus diesem Grund kann keiner Inhaltsseite, die an diese Masterseite gebunden ContentControl-Elemente enthalten.


[![Die neue Seite enthält keine ContentControl-Elemente](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Abbildung 04**: der neue Inhalt Seite enthält keine Inhaltssteuerelemente ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image12.png))


Was wir tun müssen ist, aktualisieren Sie die geschachtelte Masterseite (`SimpleNested.master`) zu ContentPlaceHolder-Steuerelemente enthalten. In der Regel sollten Ihre verschachtelten Gestaltungsvorlagen, die ein ContentPlaceHolder-Objekt für jede ContentPlaceHolder definiert, durch die übergeordnete Masterseite, wodurch dessen untergeordnete Masterseite oder Inhaltsseite auf die mit der obersten Ebene der Masterseite ContentPlaceHolder arbeiten müssen Sie -Steuerelemente.

Update der `SimpleNested.master` Masterseite ein ContentPlaceHolder-Objekt in der zwei Inhaltssteuerelemente eingeschlossen werden sollen. Geben Sie die ContentPlaceHolder-Steuerelemente den gleichen Namen wie das ContentPlaceHolder-Steuerelement, auf die, dem Ihre Inhaltssteuerelement verweist. Hinzufügen, also ein ContentPlaceHolder-Steuerelement namens `MainContent` steuern, auf den Inhalt `SimpleNested.master` , die auf die `MainContent` ContentPlaceHolder in `Simple.master`. Führen Sie dieselben Schritte in das Inhaltssteuerelement, das verweist auf die `head` ContentPlaceHolder.

> [!NOTE]
> Ich würde Ihnen empfehlen, benennen die ContentPlaceHolder-Steuerelemente in der geschachtelten Masterseite identisch mit der ContentPlaceHolder-Steuerelemente auf der obersten Ebene Masterseite, ist diese Benennungskonvention Symmetrie nicht erforderlich. Sie können die ContentPlaceHolder-Steuerelemente in Ihre geschachtelten Masterseite einen beliebigen Namen geben Sie die gewünschten. Aber ich finde es leichter zu merken, was ContentPlaceHolder-Steuerelemente entsprechen in welchen Regionen Rand der Seite, wenn meine der obersten Ebene der Masterseite und geschachtelte Masterseiten die gleichen Namen verwenden.


Nach dem Herstellen dieser Ergänzungen Ihrer `SimpleNested.master` der Masterseite deklaratives Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Löschen der `Default.aspx` Inhaltsseite, die wir gerade erstellt haben, und klicken Sie dann erneut hinzufügen, binden an die `SimpleNested.master` Masterseite. Dieses Mal fügt Visual Studio zwei Inhaltssteuerelemente die `Default.aspx`, verweisen auf die ContentPlaceHolder-Steuerelemente nun im definiert `SimpleNested.master` (siehe Abbildung 6). Fügen Sie den Text, der "Hello, from" default.aspx "!" in den Inhalt zu steuern, die auf die verwiesen wird `MainContent`.

Abbildung 5 zeigt die drei Entitäten, die zurück - `Simple.master`, `SimpleNested.master`, und `Default.aspx` – und wie diese miteinander in Beziehung stehen. Wie das Diagramm zeigt, implementiert die geschachtelten Masterseite ContentControl-Elemente des übergeordneten Elements ContentPlaceHolder. Wenn diese Bereiche auf der Seite Inhalt zugreifen kann müssen, muss die geschachtelten Masterseite eigene ContentPlaceHolder-Steuerelemente an die Inhaltssteuerelemente hinzufügen.


[![Die obersten Ebene und geschachtelte Masterseiten bestimmt den Inhalt des Layouts einer Seite](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Abbildung 05**: die obersten Ebene und geschachtelte Masterseiten bestimmen die Inhaltsseite Layout ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image15.png))


Dieses Verhalten zeigt, wie eine Seite oder Masterseite nur die übergeordnete Masterseite kennen sollten. Dieses Verhalten wird auch durch Visual Studio-Designer angezeigt. Abbildung 6 zeigt den Designer für `Default.aspx`. Während der Designer deutlich zeigt, in welchen Regionen auf der Seite Inhalt bearbeitet werden und was nicht, dass Teile sind, nicht es unterscheiden, was nicht bearbeitbare Bereiche der geschachtelten Masterseite sind und welche Bereiche der obersten Ebene Masterseite.


[![Der Inhalt Seite nun enthält ContentControl-Elemente, für die geschachtelte Masterseite ContentPlaceHolder-Steuerelemente](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Abbildung 06**: die Seite jetzt umfasst Inhalte Inhaltssteuerelemente für die geschachtelte Masterseites ContentPlaceHolder-Steuerelemente ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Schritt 3: Hinzufügen einer zweiten, einfache, geschachtelte Masterseite

Der Vorteil der verschachtelte Gestaltungsvorlagen ist offensichtlich, wenn es mehrere geschachtelte Masterseiten gibt. Um diesen Vorteil zu veranschaulichen, erstellen Sie einen anderen geschachtelten Masterseite in der `NestedMasterPages` Ordner; die Namen dieser neuen geschachtelten Masterseite `SimpleNestedAlternate.master` und binden Sie es an der `Simple.master` Masterseite. Fügen Sie in der geschachtelten Masterseite zwei Inhaltssteuerelementen ContentPlaceHolder-Steuerelemente hinzu, wie in Schritt2. Steigern Sie den Text "Hello, from SimpleNestedAlternate!" Klicken Sie in das Inhaltssteuerelement, das der obersten Ebene der Masterseite entspricht `MainContent` ContentPlaceHolder. Nach diesen Änderungen sollte die neue geschachtelte Masterseite der deklarativen Markup etwa wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Erstellen Sie eine Inhaltsseite, die mit dem Namen `Alternate.aspx` in die `NestedMasterPages` Ordner und binden Sie es an der `SimpleNestedAlternate.master` geschachtelte Masterseite. Fügen Sie den Text, der "Hello, alternativer!" Klicken Sie in das Inhaltssteuerelement, das entspricht `MainContent`. Abbildung 7 zeigt `Alternate.aspx` über Visual Studio-Designer angezeigt.


[![Alternate.aspx ist an der SimpleNestedAlternate.master Masterseite gebunden.](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Abbildung 07**: `Alternate.aspx` gebunden ist, um die `SimpleNestedAlternate.master` Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image21.png))


Vergleichen Sie den Designer in Abbildung 7 in den Designer in Abbildung 6. Beide Inhaltsseiten gemeinsam nutzen, dasselbe Layout auf der obersten Ebene Masterseite definiert (`Simple.master`), und zwar auf den Titel "Nested Master Pages-Tutorial (einfach)". Beide haben noch unterschiedliche Inhalte in ihrer übergeordneten Masterseiten - definiert den Text "Hello, from SimpleNested!" in Abbildung 6 und "Hello, from SimpleNestedAlternate!" in Abbildung 7. Gewährt, diese Unterschiede sind einfach, aber Sie könnten dieses Beispiel, um aussagekräftigere unterschieden zählen erweitern. Z. B. die `SimpleNested.master` Seite sind zum Beispiel ein Menü mit Optionen, die spezifisch für die Inhaltsseiten, wohingegen `SimpleNestedAlternate.master` möglicherweise Informationen zu den Content-Seiten, die an ihn binden müssen.

Stellen Sie sich nun vor, dass wir das Layout der übergeordnete Standort ändern mussten. Angenommen Sie, dass wir eine Liste mit allgemeinen Links alle Inhaltsseiten hinzufügen möchten. Zu diesem aktualisieren wir die Masterseite der obersten Ebene, Zweck `Simple.master`. Gibt es keine Änderungen werden sofort in der geschachtelten Masterseiten und durch Erweiterung, deren Inhaltsseiten dargestellt.

Um die einfache veranschaulichen, mit denen wir das Layout der übergeordnete Standort ändern können, öffnen Sie die `Simple.master` Masterseite, und fügen Sie das folgende Markup zwischen den `topContent` und `mainContent` `<div>` Elemente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Dies fügt zwei Links an den Anfang jeder Seite, an die gebunden `Simple.master`, `SimpleNested.master`, oder `SimpleNestedAlternate.master`; diese Änderungen gelten für alle geschachtelten Masterseiten und ihre Inhaltsseiten sofort. Abbildung 8 zeigt `Alternate.aspx` über einen Browser angezeigt. Beachten Sie die Verknüpfungen am oberen Rand der Seite "(im Vergleich mit Abbildung 7).


[![In die Masterseite der obersten Ebene geändert werden sofort in den geschachtelten Masterseiten und Inhaltsseiten für deren dargestellt.](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Abbildung 08**: in die Masterseite der obersten Ebene geändert werden sofort in den geschachtelten Masterseiten und Inhaltsseiten für deren dargestellt ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Verwenden einer geschachtelten Masterseite für den Verwaltungsabschnitt

An diesem Punkt haben Sie erfahren, die Vorteile des geschachtelten Master Seiten und haben gesehen, wie zum Erstellen und verwenden diese in einer ASP.NET-Anwendung. Die Beispiele in den Schritten 1, 2 und 3, verwendet jedoch erstellen eine neue auf oberster Ebene Masterseite, neue geschachtelte Masterseiten und Inhaltsseiten für neue. Hinzufügen von was eine neue geschachtelte Masterseite mit einer Website mit einer vorhandenen Masterseite der obersten Ebene und Inhaltsseiten?

Eine geschachtelte Masterseite in einer vorhandenen Website zu integrieren und Zuordnen zu bestehenden Inhaltsseiten erfordert etwas mehr Aufwand als ganz von vorn beginnen. Die Schritte 4, 5, 6 und 7 untersuchen diese Herausforderungen, wie wir unsere demoanwendung, um eine neue geschachtelte Masterseite mit dem Namen enthalten erweitern `AdminNested.master` , dient die ASP.NET-Seiten in und enthält Anweisungen für den Administrator der `~/Admin` Ordner.

Die Integration von einer geschachtelten Masterseite in unsere demoanwendung werden die folgenden Hürden eingeführt:

- Der vorhandene Inhalt Webseiten die `~/Admin` Ordner haben bestimmte Erwartungen ihrer Masterseite. Zunächst einmal, erwarten sie bestimmte ContentPlaceHolder-Steuerelemente vorhanden sein. Darüber hinaus die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten rufen Sie die Masterseite öffentlichen `RefreshRecentProductsGrid` legen Sie die Methode, die `GridMessageText` -Eigenschaft, oder über einen Ereignishandler für die `PricesDoubled` Ereignis. Unsere geschachtelte Masterseite muss folglich die gleiche ContentPlaceHolder-Steuerelemente und die öffentlichen Member angeben.
- In den vorherigen Tutorials, die wir verbessert die `BasePage` Klasse dynamisch Festlegen der `Page` des Objekts `MasterPageFile` -Eigenschaft basierend auf einer Sitzungsvariablen. Wie unterstützt wir dynamisch bei Verwendung von geschachtelten Masterseiten?

Geschachtelte Masterseite zu erstellen und in unsere vorhandenen Inhaltsseiten verwenden, werden diese Überprüfung in zwei Schritten auftreten. Wir untersuchen und zeitzonenbarrieren diese Probleme, sobald sie auftreten.

## <a name="step-4-creating-the-nested-master-page"></a>Schritt 4: Erstellen der geschachtelte Masterseite

Unsere erste Aufgabe ist die Erstellung die geschachtelten Masterseite, die von den Seiten im Bereich Verwaltung verwendet werden. Wie in Schritt2 beschrieben bei Hinzufügen eines neuen Masterseite geschachtelten müssen wir die geschachtelten Masterseite übergeordnete Masterseite angeben. Aber wir haben zwei der obersten Ebene Masterseiten: `Site.master` und `Alternate.master`. Beachten Sie, den wir erstellt `Alternate.master` im vorherigen Tutorial und Code geschrieben, der `BasePage` Klasse, Gruppe der `Page` des Objekts `MasterPageFile` Eigenschaft zur Laufzeit, um entweder `Site.master` oder `Alternate.master` abhängig vom Wert der `MyMasterPage` Sitzungs-Variable.

Wie konfigurieren wir unsere geschachtelte Masterseite, dass die entsprechenden auf oberster Ebene Masterseite verwendet? Wir haben zwei Möglichkeiten:

- Erstellen Sie zwei verschachtelte Gestaltungsvorlagen, `AdminNestedSite.master` und `AdminNestedAlternate.master`, und binden diese an der obersten Ebene Masterseiten `Site.master` und `Alternate.master`bzw. In `BasePage`, dann legen wir die `Page` des Objekts `MasterPageFile` auf die entsprechenden geschachtelten Masterseite.
- Erstellen Sie eine einzelne geschachtelte Masterseite und verwenden Sie die Inhaltsseiten, die diese bestimmte Masterseite verwenden. Klicken Sie dann zur Laufzeit müssen wir legen Sie der geschachtelte Masterseite `MasterPageFile` Eigenschaft, um die entsprechenden auf oberster Ebene Masterseite zur Laufzeit. (Wie Sie mittlerweile herausgefunden haben könnte, master Seiten verfügen auch über eine `MasterPageFile` Eigenschaft.)

Wir verwenden die zweite Option. Erstellen Sie eine einzelne geschachtelte Masterseitendatei in die `~/Admin` Ordner mit dem Namen `AdminNested.master`. Da beide `Site.master` und `Alternate.master` den gleichen Satz von ContentPlaceHolder-Steuerelemente haben, ist es unerheblich, welche Masterseite, binden zwar gerne damit binden `Site.master` aus Gründen der Konsistenz.


[![Hinzufügen einer geschachtelten Masterseite in den Ordner ~/Admin an.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Abbildung 09**: Hinzufügen einer geschachtelten Masterseite, die `~/Admin` Ordner. ([Klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image27.png))


Da die geschachtelte Masterseite auf eine Gestaltungsvorlage mit vier ContentPlaceHolder-Steuerelemente gebunden ist, fügt Visual Studio vier Inhaltssteuerelemente zum anfänglichen Markup für die neue geschachtelte Masterseite-Datei. Wie in den Schritten 2 und 3 ein ContentPlaceHolder-Steuerelement in den einzelnen Steuerelementen Inhalt hinzufügen, gewähren sie den gleichen Namen wie der obersten Ebene der Masterseite ContentPlaceHolder-Steuerelement. Fügen Sie auch das folgende Markup an das Inhaltssteuerelement, das entspricht, dem `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Als Nächstes definieren Sie die `instructions` CSS-Klasse der `Styles.css` und `AlternateStyles.css` CSS-Dateien. Die folgenden CSS-Regeln dazu führen, dass HTML-Elemente erstellt, in denen die `instructions` Klasse mit eine gelbe Hintergrundfarbe und ein schwarzer, durchgezogener Rahmen angezeigt werden:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Da dieses Markup der geschachtelten Masterseite hinzugefügt wurde, wird es nur auf diesen Seiten angezeigt, die diese geschachtelten Masterseite (d. h., die Seiten im Bereich Verwaltung) zu verwenden.

Nach dem Herstellen dieser Ergänzungen an Ihre geschachtelten Masterseite, sollte der deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Beachten Sie, dass jedes Inhaltssteuerelement ein ContentPlaceHolder-Steuerelement, und dass die ContentPlaceHolder-Steuerelemente `ID` Eigenschaften werden die gleichen Werte wie die entsprechenden ContentPlaceHolder-Steuerelemente auf der obersten Ebene Masterseite zugewiesen. Darüber hinaus wird die Verwaltung Abschnitt-spezifische Markup in der `MainContent` ContentPlaceHolder.

Abbildung 10 zeigt die `AdminNested.master` geschachtelte Masterseite, wenn Sie über Visual Studio-Designer angezeigt. Sehen Sie die Anweisungen in das gelbe Feld am oberen Rand der `MainContent` Inhaltssteuerelement.


[![Der geschachtelte Masterseite erweitert die Masterseite enthält Anweisungen für den Administrator.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Abbildung 10**: die geschachtelte Masterseite erweitert die Masterseite enthält Anweisungen für den Administrator. ([Klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Schritt 5: Aktualisieren von vorhandenen Inhaltsseiten um die neue geschachtelte Masterseite zu verwenden.

Jedes Mal, wenn wir im Bereich Verwaltung eine neue Seite hinzufügen müssen wir bindet, bindet es an der `AdminNested.master` Masterseite, die wir gerade erstellt haben. Aber wie sieht es mit den vorhandenen Inhaltsseiten? Alle Inhaltsseiten am Standort derzeit, leiten Sie von der `BasePage` -Klasse, die Masterseite der Inhaltsseite programmgesteuert zur Laufzeit festgelegt. Dies ist nicht das Verhalten, die für die Inhaltsseiten im Bereich Verwaltung sollten. Stattdessen möchten wir diese Inhaltsseiten, verwenden Sie immer die `AdminNested.master` Seite. Es werden die Verantwortung für die geschachtelte Masterseite, die direkt auf oberster Ebene Inhaltsseite zur Laufzeit auszuwählen.

Um die beste Methode zum erreichen dies gewünscht Verhalten ist zum Erstellen einer neuen benutzerdefinierten Basisseite-Klasse, die mit dem Namen `AdminBasePage` , reicht die `BasePage` Klasse. `AdminBasePage` können überschreiben, klicken Sie dann die `SetMasterPageFile` und legen Sie die `Page` des Objekts `MasterPageFile` der hartcodierte Wert "~ / Admin/AdminNested.master". Auf diese Weise einer beliebigen Seite, die abgeleitet `AdminBasePage` verwenden `AdminNested.master`, während einer beliebigen Seite, die abgeleitet `BasePage` müssen die `MasterPageFile` Eigenschaftensatz dynamisch entweder "~ / Site.master" oder "~ / Alternate.master" anhand des Werts von der `MyMasterPage` Sitzungsvariablen.

Starten, indem eine neue Klassendatei zum Hinzufügen der `App_Code` Ordner mit dem Namen `AdminBasePage.vb`. Haben `AdminBasePage` erweitern `BasePage` und überschreiben Sie dann die `SetMasterPageFile` Methode. Weisen Sie in dieser Methode die `MasterPageFile` den Wert "~ / Admin/AdminNested.master". Nach diesen Änderungen Ihrer Klasse sollte die Datei etwa wie folgt aussehen:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Jetzt müssen wir haben bei der Verwaltung Abschnitt leiten Sie von der bestehenden Inhaltsseiten `AdminBasePage` anstelle von `BasePage`. Wechseln Sie zu die CodeBehind-Klassendatei für jede Seite Inhalt in die `~/Admin` Ordner und diese Änderung vornehmen. Z. B. in `~/Admin/Default.aspx` ändern Sie die Deklaration der CodeBehind-Klasse aus:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Nach:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Abbildung 11 zeigt wie die obersten Ebene Masterseite (`Site.master` oder `Alternate.master`), die geschachtelte Masterseite (`AdminNested.master`), und die Inhaltsseiten für die Verwaltung im Abschnitt miteinander in Beziehung stehen.


[![Der geschachtelte Masterseite definiert Content speziell zu den Seiten im Bereich Verwaltung](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Abbildung 11**: die geschachtelte Master Seite definiert Inhalt für die Seiten im Bereich Verwaltung ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Schritt 6: Spiegelung, öffentliche Methoden und Eigenschaften der Masterseite

Bedenken Sie, dass die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten interagiert programmgesteuert mit der Masterseite: `~/Admin/AddProduct.aspx` Aufrufe die Masterseite der öffentliche `RefreshRecentProductsGrid` -Methode und legt seine `GridMessageText` Eigenschaft `~/Admin/Products.aspx` verfügt über einen Ereignishandler für die `PricesDoubled` Ereignis. Im vorherigen Tutorial erstellt es einen `MustInherit` `BaseMasterPage` Klasse, die diese öffentlichen Member definiert.

Die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten wird davon ausgegangen, dass die Masterseite abgeleitet der `BaseMasterPage` Klasse. Die `AdminNested.master` Seite jedoch derzeit noch erweitert die `System.Web.UI.MasterPage` Klasse. Als Ergebnis wird beim Zugriff auf `~/Admin/Products.aspx` ein `InvalidCastException` mit der folgenden Meldung ausgelöst: "wandelt ein Objekt vom Typ kann nicht ' ASP.admin\_Adminnested\_master" in den Typ "BaseMasterPage". "

Um dies zu beheben, wir haben müssen, die `AdminNested.master` Code-Behind-Klasse erweitern `BaseMasterPage`. Aktualisieren Sie die geschachtelte Masterseite Code-Behind-Klassendeklaration aus:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Nach:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Wir sind noch nicht fertig. Wir müssen die gekennzeichnete Member überschreiben `MustOverride`, nämlich `RefreshRecentProductsGrid` und `GridMessageText`. Diese Elemente werden von der obersten Ebene Masterseiten verwendet, um ihren Benutzeroberflächen zu aktualisieren. (Eigentlich nur die `Site.master` Masterseite verwendet diese Methoden, obwohl beide auf oberster Ebene Masterseiten diese Methoden implementieren, da beide erweitern `BaseMasterPage`.)

Wir müssen diese Member im implementieren `AdminNested.master`, diese Implementierungen müssen lediglich ist, rufen Sie das gleiche Element einfach in die obersten Ebene Masterseite, die von der geschachtelten Masterseite verwendet. Z. B. wenn eine Seite im Bereich Verwaltung ruft der geschachtelten Masterseite `RefreshRecentProductsGrid` -Methode, die geschachtelte Masterseite muss, lediglich, wiederum, rufen Sie `Site.master` oder `Alternate.master`des `RefreshRecentProductsGrid` Methode.

Um dies zu erreichen, starten Sie durch Hinzufügen des folgenden `@MasterType` -Direktive am Anfang `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Bedenken Sie, dass die `@MasterType` Richtlinie fügt eine stark typisierte-Eigenschaft für die Code-Behind-Klasse, die mit dem Namen `Master`. Überschreiben Sie dann die `RefreshRecentProductsGrid` und `GridMessageText` Mitglieder und Delegieren Sie einfach den Aufruf der `Master`die entsprechende Methode:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Mit diesem Code werden sollten Sie in der Lage zu besuchen und die Inhaltsseiten im Bereich Verwaltung verwenden. Abbildung 12 zeigt die `~/Admin/Products.aspx` Seite, wenn Sie über einen Browser angezeigt. Wie Sie sehen, enthält die Seite das Kontrollkästchen Verwaltung-Anweisungen, die in der geschachtelten Masterseite definiert ist.


[![Die Inhaltsseiten im Bereich Verwaltung gehören Anweisungen am oberen Rand jeder Seite](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Abbildung 12**: die Inhaltsseiten in die Verwaltung im Abschnitt enthalten Anweisungen an die von jeder Seite der obersten ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Schritt 7: Verwenden der entsprechenden Masterseite zur Laufzeit

Während alle Inhaltsseiten im Bereich Verwaltung voll funktionsfähig sind, wird sie alle auf oberster Ebene dieselbe Masterseite verwenden und ignorieren Sie die Masterseite, die vom Benutzer am ausgewählten `ChooseMasterPage.aspx`. Dieses Verhalten ist darauf zurückzuführen, dass die geschachtelten Masterseite verfügt über seine `MasterPageFile` statisch-eigenschaftseinstellung `Site.master` in seine `<%@ Master %>` Richtlinie.

Verwenden Sie die Seite die obersten Ebene master ausgewählt, durch den Endbenutzer wir festlegen müssen der `AdminNested.master`des `MasterPageFile` Eigenschaft, um den Wert in der `MyMasterPage` Sitzungsvariablen. Da wir der Inhaltsseiten festgelegt `MasterPageFile` Eigenschaften in `BasePage`, Sie eventuell der Meinung, dass wir der geschachtelten Masterseite festgelegt wäre `MasterPageFile` -Eigenschaft in `BaseMasterPage` oder in der `AdminNested.master`des Code-Behind-Klasse. Dies wird nicht jedoch funktioniert, weil wir festgelegt haben müssen die `MasterPageFile` Eigenschaft am Ende der PreInit-Phase. Die früheste Uhrzeit an, die wir von einer Masterseite der Lebenszyklus der Seite programmgesteuert nutzen können, ist der Init-Phase (die nach der PreInit-Phase tritt auf,).

Aus diesem Grund müssen wir legen Sie der geschachtelte Masterseite `MasterPageFile` Eigenschaft aus der Content-Basis. Der einzige Inhalt Seiten, die die `AdminNested.master` Masterseite ableiten `AdminBasePage`. Aus diesem Grund können wir diese Logik dort ablegen. In Schritt 5 wir außer Kraft gesetzt den `SetMasterPageFile` Methode des Seitenobjekts `MasterPageFile` Eigenschaft "~ / Admin/AdminNested.master". Update `SetMasterPageFile` fest, dass auch der Masterseite `MasterPageFile` Eigenschaft, um das Ergebnis in der Sitzung gespeichert:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

Die `GetMasterPageFileFromSession` -Methode, die wir hinzugefügt haben die `BasePage` Klasse, die im vorherigen Tutorial Pfad der entsprechenden Masterseite auf den Wert der Variable Sitzung basierend gibt.

Mit dieser Änderung vorgenommen wurde führt die Auswahl des Benutzers Masterseite mit dem Abschnitt für die Verwaltung. Abbildung 13 zeigt dieselbe Seite als Abbildung 12, aber erst nachdem der Benutzer die Masterseite Auswahl geändert hat `Alternate.master`.


[![Die geschachtelte Verwaltungsseite verwendet die Masterseite, die vom Benutzer ausgewählt](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Abbildung 13**: die Nested-Verwaltungsseite wird verwendet, die obersten Ebene Master Seite vom Benutzer ausgewählten ([klicken Sie, um das Bild in voller Größe anzeigen](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Zusammenfassung

Viele wie like Inhaltsseiten auf eine Gestaltungsvorlage gebunden werden können, es ist möglich, erstellen Sie geschachtelte Masterseiten, dass eine Masterseite untergeordnetes Element an eine übergeordnete Masterseite zu binden. Die untergeordnete Masterseite kann ContentControl-Elemente aller übergeordneten ContentPlaceHolder-Steuerelemente definieren; Sie können eigene ContentPlaceHolder-Steuerelemente (sowie andere Markups) klicken Sie dann diese Inhaltssteuerelemente hinzufügen. Geschachtelte Masterseiten sind sehr nützlich in großen Webanwendungen, in dem alle Seiten gemeinsam eine allumfassende Aussehen und Verhalten, aber bestimmte Abschnitte des Standorts erfordern eindeutige Anpassungen.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Geschachtelte Masterseiten mit ASP.NET](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Tipps für geschachtelte Masterseiten und zur Entwurfszeit für Visual Studio 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Visual Studio 2008 geschachtelt, die Unterstützung der Master-Codepages](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](specifying-the-master-page-programmatically-vb.md)
