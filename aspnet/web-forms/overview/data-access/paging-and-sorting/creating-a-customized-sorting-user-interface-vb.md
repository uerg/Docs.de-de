---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: "Erstellen einer benutzerdefinierten Sortierung Benutzeroberfläche (VB) | Microsoft Docs"
author: rick-anderson
description: "Beim Anzeigen einer langen Liste von Daten sortiert werden, es kann sehr hilfreich sein, gruppieren Sie verknüpfte Daten durch die Einführung von Trennzeichen für Zeilen. In diesem Lernprogramm sehen wir wie Anmelde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: c9d6229c88e4fd67f384a5ec459ed661f32f0a50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Erstellen einer benutzerdefinierten Sortierung Benutzeroberfläche (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) oder [PDF herunterladen](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Beim Anzeigen einer langen Liste von Daten sortiert werden, es kann sehr hilfreich sein, gruppieren Sie verknüpfte Daten durch die Einführung von Trennzeichen für Zeilen. In diesem Lernprogramm sehen wir, wie solche eine Sortierung Benutzeroberfläche erstellen.


## <a name="introduction"></a>Einführung

Wenn eine lange Liste von anzeigen sortierte Daten vorhanden sind nur wenige unterschiedliche Werte in der sortierten Spalte an, ein Endbenutzer möglicherweise als schwer zu ersehen, genau die Differenz Grenzen eintreten. Es gibt z. B. 81 Produkte in der Datenbank, jedoch nur neun verschiedenen Kategorie-Optionen (acht eindeutigen Kategorien sowie die `NULL` Option). Betrachten Sie den Fall eines Benutzers interessiert ist, untersuchen die Produkte, die unter der Kategorie "Seafood" liegen. Von einer Seite mit einer Liste *alle* der Produkte in einer einzelnen GridView, kann der Benutzer entscheiden, ihre sinnvollsten ist, um die Ergebnisse nach Kategorie sortiert werden, die zusammen gruppiert werden sämtliche Produkte Seafood zusammen. Nach dem Sortieren nach Kategorie, benötigt der Benutzer klicken Sie dann durch die Liste Rückerstattungsrichtlinie gesucht, in denen die Produkte Seafood gruppiert beginnen und enden. Da die Ergebnisse in alphabetischer Reihenfolge geordnet sind der Kategorienamen der Seafood Produkte zu finden, ist nicht schwierig, aber noch müssen eng Scannen der Liste der Elemente im Raster.

Damit können die Grenzen zwischen sortierte Gruppen markieren, nutzen viele Websites eine Benutzeroberfläche, die ein Trennzeichen zwischen den solche Gruppen hinzufügt. Trennzeichen, wie die in Abbildung 1 gezeigten ermöglicht einem Benutzer schneller finden eine bestimmte Gruppe und die Grenzen zu identifizieren, als auch ermitteln, welche unterschiedlichen Gruppen in den Daten vorhanden sind.


[![Jede Kategoriegruppe ist eindeutig identifiziert.](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Abbildung 1**: Jede Kategoriegruppe ist eindeutig identifiziert ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


In diesem Lernprogramm sehen wir, wie solche eine Sortierung Benutzeroberfläche erstellen.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Schritt 1: Erstellen einer standardmäßigen, sortierbaren GridView

Bevor untersucht, wie die GridView, um die erweiterte Sortierung Schnittstelle bereitzustellen zu erweitern, können Sie die s-standard, sortierbare GridView zuerst zu erstellen, der die Produkte aufgeführt sind. Öffnen Sie zunächst die `CustomSortingUI.aspx` auf der Seite der `PagingAndSorting` Ordner. Die Seite eine GridView hinzugefügt haben, legen Sie seine `ID` Eigenschaft `ProductList`, und binden es an eine neue ObjectDataSource. Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse s `GetProducts()` Methode zum Auswählen von Datensätzen.

Als Nächstes die GridView so konfigurieren, dass sie nur enthält die `ProductName`, `CategoryName`, `SupplierName`, und `UnitPrice` BoundFields und CheckBoxField nicht mehr unterstützt. Konfigurieren Sie abschließend die GridView zur Unterstützung von Sortierung durch Aktivieren des Kontrollkästchens Sortieren aktivieren, in das Smarttag für GridView s (oder durch Festlegen seiner `AllowSorting` Eigenschaft `true`). Nach dem Herstellen dieser Ergänzungen für das `CustomSortingUI.aspx` Seite deklarative Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt bisher in einem Browser anzuzeigen. Abbildung 2 zeigt sortierbare GridView aus, wenn die Daten nach Kategorie in alphabetischer Reihenfolge sortiert werden.


[![Der sortierbare GridView s werden Daten nach Kategorie sortiert.](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Abbildung 2**: der sortierbare GridView s Daten werden nach Kategorie geordnet ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Schritt 2: Untersuchen von Techniken für das Hinzufügen von Trennzeichen für Zeilen

Mit der generischen, sortierbar GridView abgeschlossen übrig bleibt die Trennzeichen für Zeilen in der GridView vor jeder eindeutige sortierten Gruppe hinzufügen können. Doch wie können solche Zeilen injiziert in die GridView? Im Wesentlichen benötigen wir zum iterieren durch die GridView s Zeilen ermittelt, auf die Unterschiede zwischen den Werten in der sortierten Spalte erfolgen, und fügen Sie der entsprechenden Trenndatei Zeile hinzu. Bei der Betrachtung der Informationen zu diesem Problem scheint es natürlich, dass die Lösung an einer beliebigen Stelle in der GridView s liegt `RowDataBound` -Ereignishandler. Wie in erläutert die [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) Lernprogramm diesen Ereignishandler wird häufig verwendet, wenn anhand von Daten aus der Zeile s auf Zeilenebene Formatierung anwenden. Allerdings die `RowDataBound` -Ereignishandler ist nicht die Lösung, wie Zeilen an die GridView programmgesteuert aus diesem Ereignishandler hinzugefügt werden können. Die GridView s `Rows` -Auflistung, in der Tat kann nur gelesen werden.

Zum Hinzufügen von zusätzlicher Zeilen an die GridView haben wir drei Auswahlmöglichkeiten angezeigt:

- Fügen Sie diese Metadaten Trennzeichen für Zeilen hinzu, auf die tatsächlichen Daten, die an die GridView gebunden ist
- Nachdem Sie auf die Daten die GridView gebunden wurde, hinzufügen, zusätzliche `TableRow` Datensammlung steuern, Instanzen mit dem GridView-s
- Erstellen Sie ein benutzerdefiniertes Steuerelement, das erweitert des GridView-Steuerelements und überschreibt die Methoden, die verantwortlich für das Erstellen des GridView-s-Struktur

Erstellen eines benutzerdefinierten Steuerelements wäre der beste Ansatz, wenn diese Funktionalität auf viele Webseiten oder über mehrere Websites erforderlich war. Allerdings würde es relativ viel Code und eine gründliche Untersuchung in die Tiefen die interne Funktionsweise des GridView-s gelten. Aus diesem Grund müssen wir die Option für dieses Lernprogramm nicht berücksichtigt.

Hinzufügen von Trennzeichen für Zeilen auf die tatsächlichen Daten, die anderen beiden Optionen für an die GridView gebunden, und Bearbeiten von GridView s steuerelementauflistung nach seiner gebunden – für potenzielle Angriffe durch das Problem anders an, und eine Erläuterung bedürfen.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Hinzufügen von Zeilen auf die Daten an die GridView gebunden

Wenn die GridView mit einer Datenquelle gebunden ist, erstellt eine `GridViewRow` für jeden Datensatz von der Datenquelle zurückgegeben. Daher können wir die erforderlichen Trennzeichen Zeilen einfügen, durch Trennzeichen Datensätze an die Datenquelle hinzufügen, bevor es an die GridView gebunden. Abbildung 3 veranschaulicht dieses Konzept.


![Ein Verfahren umfasst das Hinzufügen von Trennzeichen für Zeilen mit der Datenquelle](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Abbildung 3**: ein Verfahren umfasst das Hinzufügen von Trennzeichen für Zeilen mit der Datenquelle


Ich verwenden Begriff Trennzeichen Datensätze in Anführungszeichen, da es keine spezielle Trennzeichen Datensatz; Stattdessen müssen wir aus irgendeinem Grund ein flag, die ein bestimmter Datensatz in der Datenquelle als Trennzeichen anstelle einer normalen Datenzeile dient. Für diesen Beispielen, wir re-Bindung eine `ProductsDataTable` Instanz an die GridView, die besteht `ProductRows`. Wir möglicherweise einen Datensatz als eine Trennzeichenzeile kennzeichnen, durch Festlegen seiner `CategoryID` Eigenschaft `-1` (da dieser Wert nicht definiert t normalerweise vorhanden).

Um dieses Verfahren nutzen zu können, müssen wir d die folgenden Schritte ausführen:

1. Programmgesteuertes Abrufen von Daten an die GridView gebunden (eine `ProductsDataTable` Instanz)
2. Sortieren Sie die Daten basierend auf den GridView s `SortExpression` und `SortDirection` Eigenschaften
3. Durchlaufen der `ProductsRows` in der `ProductsDataTable`, suchen, in denen die Unterschiede in der sortierten Spalte liegen
4. An jede Gruppengrenze einfügen ein Datensatzes Trennzeichen `ProductsRow` Instanz in die Datentabelle, eine, die s `CategoryID` festgelegt `-1` (oder die Bezeichnung erfassungsebene wurde, um einen Datensatz als Trennzeichen Datensatz markieren)
5. Nach dem Räumen der Trennzeichen für Zeilen, binden Sie die Daten an die GridView programmgesteuert

Zusätzlich zu den fünf Schritte d müssen ebenfalls einen Ereignishandler für GridView s bereitstellen `RowDataBound` Ereignis. Hier prüfen wir d jeweils `DataRow` und zu bestimmen, ob es ein Trennzeichen wurde eine Zeile, deren `CategoryID` Einstellung wurde `-1`. Wenn dies der Fall ist, möchten wir d wahrscheinlich die Formatierung oder den in den Zellen angezeigten Text anpassen.

Mithilfe dieser Technik für etwas mehr als die oben beschriebenen Räumen Grenzen für die Sortierung erforderlich werden, da Sie auch einen Ereignishandler für GridView s bereitstellen müssen `Sorting` Ereignis und Beibehalten von verfolgen die `SortExpression` und `SortDirection` Werte.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Bearbeiten die GridView-s-Datensammlung dahinter steuern s datengebundenen wurde

Anstatt die Daten vor dem binden es an die GridView-messaging, können wir die Trennzeichen für Zeilen hinzufügen *nach* die Daten an die GridView gebunden wurde. Der Prozess der Datenbindung builds Serversteuerelementhierarchie der GridView s, die in Wirklichkeit ist einfach eine `Table` Instanz besteht aus einer Auflistung von Zeilen, von denen jede aus einer Auflistung von Zellen besteht. Insbesondere steuerelementauflistung GridView s enthält eine `Table` Objekt auf der Stammebene einer `GridViewRow` (abgeleitet aus der `TableRow` Klasse) für jeden Datensatz in der `DataSource` an die GridView gebunden und eine `TableCell` Objekt in jeder `GridViewRow` Instanz für jedes Datenfeld in der `DataSource`.

Um Zeilen der Trennzeichen zwischen den einzelnen Sortierung Gruppen hinzuzufügen, können wir diese Steuerelementhierarchie direkt bearbeiten, nachdem es erstellt wurde. Wir können sicher sein, dass die Hierarchie der GridView-s-Steuerelemente zum letzten Mal nach der Zeit erstellt wurde, die die Seite gerendert wird. Dieser Ansatz daher überschreibt die `Page` Klasse s `Render` Methode, die an diesem Punkt die Steuerelementhierarchie GridView s endgültigen aktualisiert wird, um die erforderlichen Trennzeichen Zeilen enthalten. Abbildung 4 zeigt diesen Vorgang.


[![Eine alternative Methode bearbeitet die Hierarchie der GridView-s-Steuerelemente](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Abbildung 4**: eine alternative Methode bearbeitet die GridView s Steuerelementhierarchie ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


Für dieses Lernprogramm verwenden wir dieser zweite Ansatz, um die Sortierung benutzerfreundlichkeit anpassen.

> [!NOTE]
> Der Code ich m darstellen, in diesem Lernprogramm basiert auf dem Beispiel in [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s Blogeintrag [Wiedergabe eine Bit mit GridView sortieren Gruppierung](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Schritt 3: Hinzufügen der Trennzeichen Zeilen an die GridView s Steuerelementhierarchie

Da möchten wir nur die Trennzeichen für Zeilen an die GridView s Steuerelementhierarchie hinzufügen, nachdem seine Steuerelementhierarchie erstellt und zum letzten Mal auf dieser Seite finden Sie unter erstellt wurde, möchten wir diese Ergänzung am Ende des Lebenszyklus der Seite, aber vor der tatsächlichen GridView c ausführen uerelement Hierarchie ist in HTML gerendert wurde. Der neueste mögliche Punkt, an dem wir dies erreichen, ist die `Page` Klasse s `Render` Ereignis, das wir in unserer Code-Behind-Klasse, die mithilfe der folgenden Methodensignatur überschreiben können:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Wenn die `Page` Klasse s ursprünglichen `Render` Methode wird aufgerufen, `base.Render(writer)` jedes der Steuerelemente auf der Seite gerendert wird, generiert das Markup, das basierend auf ihrer Hierarchie. Daher ist es unabdinglich, wir beide nennen `base.Render(writer)`, damit die Seite gerendert wird, und wir die GridView s manipulieren steuern Hierarchie vor dem Aufruf `base.Render(writer)`, sodass die GridView-s-Steuerelementhierarchie davor die Trennzeichen für Zeilen hinzugefügt wurden s wurde gerendert.

Zum Einfügen der Gruppenheader sortieren müssen wir sicherstellen, dass der Benutzer angefordert hat, dass die Daten sortiert werden. Standardmäßig den GridView-s-Inhalt nicht sortiert sind und daher wir Verschlüsselungskennwort t muss jede Gruppe sortieren Header eingeben.

> [!NOTE]
> Wenn Sie möchten die GridView nach einer bestimmten Spalte sortiert werden, wenn die Seite erstmals geladen wird, rufen Sie die GridView s `Sort` Methode ersten Besuch der Seite "(aber nicht bei nachfolgenden Postbacks). Um dies zu erreichen, fügen Sie diesen Aufruf in der `Page_Load` Ereignishandler innerhalb einer `if (!Page.IsPostBack)` bedingte. Verweisen auf die [Paging und Sortieren von Berichtsdaten](paging-and-sorting-report-data-vb.md) Tutorial Informationen Weitere Informationen über die `Sort` Methode.


Vorausgesetzt, dass die Daten sortiert wurden, unsere nächste Aufgabe besteht, um zu bestimmen, welche Spalte die Daten sortiert wurde, und klicken Sie dann auf die Unterschiede in der betreffenden Spalte s sucht Zeilen zu durchsuchen Werte. Der folgende Code stellt sicher, dass die Daten sortiert wurden, und sucht nach der Spalte, nach der die Daten sortiert wurden:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Verfügt die GridView noch werden sortiert, die GridView s `SortExpression` wurde-Eigenschaft wird nicht festgelegt. Daher möchten wir nur die Trennzeichen für Zeilen hinzufügen, wenn diese Eigenschaft einen Wert hat. Wenn dies der Fall ist, müssen wir als Nächstes den Index der Spalte zu bestimmen, mit dem die Daten sortiert wurde. Dies wird erreicht, indem die GridView s durchlaufen `Columns` Auflistung, die Suche nach der Spalte, deren `SortExpression` Eigenschaft entspricht der GridView s `SortExpression` Eigenschaft. Zusätzlich zu den Spaltenindex s nehmen wir auch die `HeaderText` -Eigenschaft, die verwendet wird, wenn die Trennzeichen für Zeilen anzeigen.

Mit dem Index der Spalte, nach der die Daten sortiert werden, besteht der letzte Schritt die Datenzeilen GridView instanzenauflistung. Wir möchten für jede Zeile zu bestimmen, ob der Wert des s sortierte Spalte aus der vorherigen Zeile s sortiert Spalte s-Wert unterscheidet sich. Wenn wir also eine neue einfügen müssen `GridViewRow` Instanz in der Hierarchie. Dies erfolgt durch den folgenden Code:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Dieser Code beginnt durch Programmgesteuertes Verweisen auf die `Table` Objekt auf der Stammebene des GridView-s-Steuerelementhierarchie gefunden, und erstellen Sie eine Zeichenfolgenvariable mit dem Namen `lastValue`. `lastValue`Dient zum Vergleichen des aktuellen Zeile s sortiert Spaltenwert mit der vorherigen s-Zeilenwert. Anschließend wird die GridView-s `Rows` Auflistung aufgezählt, und für jede Zeile befindet sich der Wert für die sortierte Spalte in der `currentValue` Variable.

> [!NOTE]
> Um zu bestimmen, den Wert der Spalte sortiert s bestimmten Zeile, die ich verwende der Zelle s `Text` Eigenschaft. Es funktioniert gut für BoundFields, aber funktioniert nicht wie gewünscht für von TemplateFields, CheckBoxFields, und so weiter. Sehen wir uns wie alternative GridView-Felder in Kürze zu berücksichtigen.


Die `currentValue` und `lastValue` Variablen dann verglichen werden. Wenn Unterschiede festgestellt werden, müssen wir Steuerelementhierarchie eine Trennzeichenzeile neue hinzu. Dies erfolgt durch den Index des zu bestimmen die `GridViewRow` in der `Table` s-Objekt `Rows` -Auflistung, Erstellen neuer `GridViewRow` und `TableCell` Instanzen und dem anschließenden Hinzufügen der `TableCell` und `GridViewRow` auf die Steuerelementhierarchie.

Beachten Sie, dass das Trennzeichen s, die einzelne Zeile `TableCell` ist so formatiert, dass es die gesamte Breite des GridView umfasst mit formatiert wird die `SortHeaderRowStyle` CSS-Klasse, und seine `Text` z. B., dass sie sowohl die Sortiergruppe zeigt Eigenschaftenname (z. B. die Kategorie "") und der Gruppe-s-Wert (z. B. Getränke). Schließlich `lastValue` wird aktualisiert, um den Wert des `currentValue`.

Die CSS-Klasse, die zum Formatieren der Sortierung Gruppenkopfzeile `SortHeaderRowStyle` muss angegeben werden, in der `Styles.css` Datei. Verwenden Sie beliebige formateinstellungen anzusprechen gerne; Die folgenden verwendet:


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Mit dem aktuellen Code fügt die Sortierung Schnittstelle sortieren Gruppenkopfzeilen beim Sortieren nach jeder BoundField (siehe Abbildung 5, die einen Screenshot beim Sortieren von Lieferant zeigt). Allerdings sind beim Sortieren von jedem anderen Feldtyp (z. B. eine CheckBoxField oder TemplateField) der Sortierung Gruppenheader kein Ausgabeziel (siehe Abbildung 6) gefunden werden.


[![Die Sortierung Schnittstelle enthält sortieren Gruppenkopfzeilen, beim Sortieren nach BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Abbildung 5**: der Sortierung Schnittstelle enthält sortieren Gruppe Header beim Sortieren nach BoundFields ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Der Sort-Gruppenheader sind fehlende beim Sortieren einer CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Abbildung 6**: Sortieren der Kopfzeilen werden fehlende beim Sortieren einer CheckBoxField ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


Der Grund, die beim Sortieren nach einer CheckBoxField fehlen der Gruppenheader Sortieren ist, da der Code derzeit nur verwendet die `TableCell` s `Text` Eigenschaft zum Bestimmen des Werts, der die sortierte Spalte für jede Zeile. Für CheckBoxFields die `TableCell` s `Text` Eigenschaft ist eine leere Zeichenfolge; stattdessen der Wert ist verfügbar, durch ein Kontrollkästchen-Websteuerelement, die innerhalb der `TableCell` s `Controls` Auflistung.

Um Feldtypen als BoundFields behandeln zu können, müssen wir den Code zu erweitern, in dem die `currentValue` Variable zugewiesen wird überprüft, ob ein Kontrollkästchen in der `TableCell` s `Controls` Auflistung. Anstatt `currentValue = gvr.Cells(sortColumnIndex).Text`, ersetzen Sie diesen Code durch Folgendes:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Dieser Code überprüft die sortierte Spalte `TableCell` für die aktuelle Zeile zu ermitteln, ob alle Steuerelemente in der `Controls` Auflistung. Wenn Sie vorhanden sind, und das erste Steuerelement ein Kontrollkästchen der `currentValue` Variable wird festgelegt, auf Ja oder Nein, je nach den Kontrollkästchen s `Checked` Eigenschaft. Der Wert stammt, andernfalls aus den `TableCell` s `Text` Eigenschaft. Diese Logik kann zum Behandeln der Sortierung für alle von TemplateFields, die in der GridView vorhanden sind, kann repliziert werden.

Durch die oben aufgeführten Code hinzufügen jetzt der Gruppenheader Sortierreihenfolge beim Sortieren nach CheckBoxField nicht mehr unterstützte vorhanden sind (siehe Abbildung 7).


[![Der Sort-Gruppenheader sind jetzt vorhanden beim Sortieren einer CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Abbildung 7**: Sortieren der Kopfzeilen werden jetzt vorhanden beim Sortieren einer CheckBoxField ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Wenn Sie über Produkte mit `NULL` Datenbank Werte für die `CategoryID`, `SupplierID`, oder `UnitPrice` Felder, diese Werte als leere Zeichenfolgen in die GridView angezeigt werden, wird standardmäßig, d. h. Trennzeichen Zeilentext s für diese Produkte mit `NULL`Werte liest z. B. Kategorie: (d. h. es s kein Name nach Kategorie: like mit Kategorie: Getränke). Wenn ein Wert, der hier angezeigt werden soll, können Sie entweder die BoundFields festlegen [ `NullDisplayText` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) auf den Text angezeigt werden soll, oder Sie können eine bedingte Anweisung in der Render-Methode hinzufügen, beim Zuweisen der `currentValue` für den Separator Zeile s `Text` Eigenschaft.


## <a name="summary"></a>Zusammenfassung

Die GridView umfasst viele integrierten Optionen zum Anpassen der Benutzeroberfläche der Sortierung nicht. Allerdings mit einem gewissen Grad Code auf niedriger Ebene es s möglich GridView s Steuerelementhierarchie zum Erstellen einer stärker angepassten Benutzeroberfläche zu optimieren. In diesem Lernprogramm wurde es erläutert, wie eine Sortierung Trennzeichen Gruppenzeile für einen sortierbaren GridView hinzufügen, die unterschiedlichen Gruppen und diesen Gruppen Grenzen leichter identifiziert werden. Weitere Beispiele für benutzerdefinierte Sortierung Schnittstellen Auschecken [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [einige ASP.NET 2.0 GridView sortieren Tipps und Tricks](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) Blogeintrag.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](sorting-custom-paged-data-vb.md)
