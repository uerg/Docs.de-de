---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Erstellen eine benutzerdefinierte Sortierung-Benutzeroberfläche (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Daten mit einer langen Liste sortiert werden, es kann sehr hilfreich sein, gruppieren Sie verknüpfte Daten durch die Einführung von Trennzeichen für Zeilen. In diesem Tutorial sehen, dass wie ASK erstellen...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: b12baa5075b4e67018d8a98a92e807d1778737c8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832134"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Erstellen eine benutzerdefinierte Sortierung-Benutzeroberfläche (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) oder [PDF-Datei herunterladen](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Wenn Daten mit einer langen Liste sortiert werden, es kann sehr hilfreich sein, gruppieren Sie verknüpfte Daten durch die Einführung von Trennzeichen für Zeilen. In diesem Tutorial sehen wir, wie Sie solche eine Sortierung Benutzeroberfläche zu erstellen.


## <a name="introduction"></a>Einführung

Wenn mit einer langen Liste sortierter Daten vorhanden sind nur wenige unterschiedliche Werte in der sortierten Spalte an, ein Benutzer finden es möglicherweise schwierig, genau die Unterschied Grenzen auftreten zu erkennen. Beispielsweise sind 81 Produkte in der Datenbank, aber nur neun andere Kategorie-Optionen (acht eindeutigen Kategorien sowie die `NULL` Option). Betrachten Sie den Fall eines Benutzers ist daran interessiert, untersuchen die Produkte, die unter der Kategorie "Seafood" fallen. Von einer Seite, die auflistet *alle* der Produkte in einer einzelnen GridView, kann der Benutzer entscheiden, ihr am sinnvollsten ist, zum Sortieren der Ergebnisse nach Kategorie, die gruppiert werden alle Produkte Seafood zusammen. Nach dem Sortieren nach Kategorie, muss der Benutzer über der Liste zu suchen gesucht, in denen die Produkte Seafood gruppiert beginnen und enden. Da die Ergebnisse nach alphabetischer Reihenfolge sortiert werden der Kategoriename der Seafood Produkte zu finden, ist nicht schwierig, aber erfordert er dennoch genau überprüft die Liste der Elemente im Raster.

Damit können die Grenzen zwischen sortierte Gruppen markieren, nutzen viele Websites eine Benutzeroberfläche, die ein Trennzeichen zwischen Gruppen hinzugefügt. Trennzeichen, wie in Abbildung 1 dargestellten ermöglicht Benutzern schneller eine bestimmte Gruppe suchen und identifizieren die Grenzen, als auch ermitteln, welche unterschiedlichen Gruppen in den Daten vorhanden sind.


[![Jede Kategoriegruppe ist eindeutig identifiziert](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Abbildung 1**: Jede Kategoriegruppe ist eindeutig identifiziert ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


In diesem Tutorial sehen wir, wie Sie solche eine Sortierung Benutzeroberfläche zu erstellen.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Schritt 1: Erstellen einer standardmäßigen, sortierbar GridView-Ansicht

Bevor wir Gewusst wie: verbessern die GridView, um die erweiterte Sortierung Oberfläche bereitzustellen, zu untersuchen, können Sie s, erstellen Sie zunächst eine standard, sortierbare GridView, die die Produkte aufgelistet werden. Öffnen Sie zunächst die `CustomSortingUI.aspx` auf der Seite die `PagingAndSorting` Ordner. Hinzufügen einer GridView-Ansicht auf der Seite, legen Sie dessen `ID` Eigenschaft `ProductList`, und binden sie an eine neue "ObjectDataSource". Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse s `GetProducts()` Methode zum Auswählen von Datensätzen.

Konfigurieren Sie anschließend die GridView, nur enthält die `ProductName`, `CategoryName`, `SupplierName`, und `UnitPrice` BoundFields und CheckBoxField nicht mehr unterstützt. Konfigurieren Sie abschließend die GridView zur Unterstützung von Sortierung durch Aktivieren des Kontrollkästchens Sortieren aktivieren, in das GridView-s-Smarttag (oder durch Festlegen seiner `AllowSorting` Eigenschaft `true`). Nach dem Herstellen dieser Ergänzungen zu den `CustomSortingUI.aspx` Seite deklarative Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt bisher in einem Browser anzuzeigen. Abbildung 2 zeigt das sortierbare GridView, wenn die Daten nach Kategorie in alphabetischer Reihenfolge sortiert ist.


[![Der sortierbare GridView-s werden Daten nach Kategorie sortiert.](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Abbildung 2**: das sortierbare GridView s, die Daten nach Kategorie sortiert werden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Schritt 2: Untersuchen von Techniken für das Trennzeichen für Zeilen hinzufügen

Mit generischen, sortierbar GridView vollständige übrig bleibt, um die Trennzeichen für Zeilen in der GridView vor jeder eindeutige sortierten Gruppe hinzufügen zu können. Aber wie können solche Zeilen eingefügt werden in der GridView? Im Wesentlichen, wir benötigen zum iterieren durch die GridView-s-Zeilen bestimmen, in denen die Unterschiede zwischen den Werten in der sortierten Spalte auftreten, und fügen Sie das entsprechende Trennzeichen-Zeile hinzu. Bei Informationen zu diesem Problem scheint es natürlich, dass die Lösung an einer beliebigen Stelle in den GridView-s liegt `RowDataBound` -Ereignishandler. Wie wir unter den [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) Tutorial dieser Ereignishandler wird häufig verwendet, wenn es sich bei anhand von Daten aus der Zeile s auf Zeilenebene Formatierung anwenden. Allerdings die `RowDataBound` -Ereignishandler ist nicht die Lösung hier, wie Zeilen an die GridView programmgesteuert aus diesem Ereignishandler hinzugefügt werden können. Das GridView-s `Rows` -Auflistung, in der Tat schreibgeschützt ist.

Zum Hinzufügen von zusätzlicher Zeilen an die GridView gibt es drei Möglichkeiten zur Auswahl:

- Fügen Sie diese Metadaten Trennzeichen für Zeilen, auf die tatsächlichen Daten, die an die GridView gebunden ist
- Nachdem Sie auf die Daten die GridView gebunden wurde, hinzufügen, zusätzliche `TableRow` Instanzen der GridView-s steuern Auflistung
- Erstellen Sie ein benutzerdefiniertes Serversteuerelement, das erweitert des GridView-Steuerelements und überschreibt die Methoden, die verantwortlich für die Erstellung der GridView-s-Struktur

Erstellen ein benutzerdefiniertes Serversteuerelement wäre am besten, wenn diese Funktionalität auf viele Webseiten oder über mehrere Websites erforderlich war. Es wäre jedoch einiges an Code und eine gründliche Untersuchung in die Tiefe der die interne Funktionsweise des GridView-s gelten. Aus diesem Grund werden wir die Option für dieses Tutorial nicht berücksichtigt.

Die anderen beiden Optionen Hinzufügen von Trennzeichen für Zeilen, die tatsächlichen Daten, an die GridView gebunden, und Bearbeiten von der GridView-s-steuerelementauflistung nach dessen wurde gebunden – das Problem anders Angriffe und eine Erläuterung bedürfen.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Hinzufügen von Zeilen auf die Daten an die GridView gebunden

Wenn die GridView mit einer Datenquelle gebunden ist, erstellt es einen `GridViewRow` für jeden Datensatz von der Datenquelle zurückgegeben. Aus diesem Grund können wir die erforderlichen Trennzeichen für Zeilen einfügen, durch Trennzeichen Datensätze an die Datenquelle hinzufügen, bevor er an die GridView gebunden. Abbildung 3 veranschaulicht dieses Konzept.


![Eine Technik wird das Hinzufügen von Trennzeichen für Zeilen mit der Datenquelle](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Abbildung 3**: eine Technik wird das Hinzufügen von Trennzeichen für Zeilen mit der Datenquelle


Ich verwende Begriff Trennzeichen Datensätze in Anführungszeichen, da es keine spezielle Trennzeichen für Datensatz; Stattdessen müssen wir irgendwie flag, die ein bestimmter Datensatz in der Datenquelle als Trennzeichen und nicht als eine Zeile für normale Daten dient. Für unseren Beispielen wir re-Bindung eine `ProductsDataTable` Instanz an die GridView, das aus besteht `ProductRows`. Wir können einen Datensatz als eine Trennzeichenzeile kennzeichnen, durch Festlegen der `CategoryID` Eigenschaft `-1` (da es sich um eine solche eine Wert konnte t normalerweise vorhanden ist).

Um dieses Verfahren nutzen zu können, müssen wir d die folgenden Schritte ausführen:

1. Programmgesteuertes Abrufen von Daten an die GridView zu binden (eine `ProductsDataTable` Instanz)
2. Sortieren Sie die Daten basierend auf der GridView-s `SortExpression` und `SortDirection` Eigenschaften
3. Durchlaufen der `ProductsRows` in die `ProductsDataTable`, suchen, wo die Unterschiede in der sortierten Spalte liegen
4. Einfügen von an jeder Gruppengrenze,, einen Trennzeichen für Datensatz `ProductsRow` Instanz in die Datentabelle, einen, der s `CategoryID` festgelegt `-1` (oder beliebige Bezeichnung erfassungsebene wurde, um einen Datensatz als Trennzeichen für Datensatz markieren)
5. Nach dem Einfügen der Trennzeichen für Zeilen, binden Sie die Daten an die GridView programmgesteuert

Zusätzlich zu den folgenden fünf Schritte aus, wir d müssen auch einen Ereignishandler für das GridView-s bereitstellen `RowDataBound` Ereignis. Hier prüfen wir d jeweils `DataRow` und ermitteln, ob es ein Trennzeichen wurde eine Zeile, deren `CategoryID` Einstellung wurde `-1`. Wenn dies der Fall ist, möchten wir d wahrscheinlich passen Sie die Formatierung oder den Text in die Zellen wird angezeigt.

Verwendung dieser Technik für Grenzen für die Sortierung einfügen etwas mehr Arbeit als oben beschrieben, wie Sie auch einen Ereignishandler für das GridView-s bereitstellen müssen `Sorting` Ereignis und beibehalten zu verfolgen, der die `SortExpression` und `SortDirection` Werte.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Bearbeiten der GridView-s-Auflistung nach dem sie steuern s wurde an Daten gebunden

Anstatt die Daten vor dem Binden an die GridView-messaging, wir können die Trennzeichen für Zeilen hinzufügen *nach* die Daten an die GridView gebunden wurde. Der Prozess der Datenbindung erstellt, der Steuerelementhierarchie GridView s, die in der Praxis ist einfach eine `Table` Instanz besteht aus einer Auflistung von Zeilen, von denen jede aus einer Auflistung von Zellen besteht. Insbesondere die steuerelementauflistung des GridView-s enthält eine `Table` Objekt am Stamm, eine `GridViewRow` (ergibt sich aus der `TableRow` Klasse) für jeden Datensatz in die `DataSource` an die GridView gebunden und ein `TableCell` Objekt in jeder `GridViewRow` Instanz für jedes Datenfeld in der `DataSource`.

Um Trennzeichen für Zeilen zwischen den einzelnen sortieren Gruppen hinzuzufügen, können wir diese Steuerelementhierarchie direkt bearbeiten, nachdem es erstellt wurde. Wir können sicher sein, dass die Hierarchie der GridView-s-Steuerelemente zum letzten Mal mit der Zeit erstellt wurde, die die Seite gerendert wird. Dieser Ansatz aus diesem Grund überschreibt die `Page` Klasse s `Render` Methode, die an diesem Punkt-die Steuerelementhierarchie GridView s endgültige aktualisiert wird, um die erforderlichen Trennzeichen für Zeilen enthalten. Dieser Prozess wird in Abbildung 4 dargestellt.


[![Eine alternative Methode bearbeitet die Hierarchie der GridView-s-Steuerelemente](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Abbildung 4**: eine alternative Methode ändert das GridView-s-Serversteuerelement-Hierarchie ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


In diesem Tutorial wird dieser zweite Ansatz verwendet, um die Sortierung benutzererfahrung anpassen.

> [!NOTE]
> Der Code ich m darstellen, in diesem Tutorial basiert auf dem Beispiel unter [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s Blogeintrag [spielen etwas mit GridView sortieren Gruppierung](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Schritt 3: Hinzufügen von Trennzeichen für Zeilen, die Hierarchie der GridView-s-Steuerelemente

Da möchten wir nur die Trennzeichen für Zeilen die Hierarchie der GridView-s-Steuerelemente hinzufügen, nachdem seine Steuerelementhierarchie erstellt und zum letzten Mal auf dieser Seite finden Sie unter erstellt wurde, sollten Sie diese Ergänzung am Ende des Lebenszyklus der Seite, aber vor der tatsächlichen GridView c ausführen quellcodeverwaltung-Hierarchie ist in HTML gerendert. Der neueste Punkt an der wir dies erreichen können, ist die `Page` Klasse s `Render` -Ereignis, das wir in unserem Code-Behind-Klasse, die mit die folgende Methodensignatur überschreiben können:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Wenn die `Page` s, die ursprüngliche Klasse `Render` Methode wird aufgerufen, `base.Render(writer)` jedes der Steuerelemente auf der Seite gerendert wird, generiert das Markup, das basierend auf ihrer Hierarchie. Daher ist es zwingend erforderlich, dass wir beide rufen `base.Render(writer)`, damit die Seite gerendert wird, und dass wir das GridView-s bearbeiten-Steuerelementhierarchie vor dem Aufruf `base.Render(writer)`, sodass die GridView-s-Steuerelementhierarchie vor dem Trennzeichen für Zeilen hinzugefügt wurden s wurde gerendert.

Zum Einfügen der Gruppenheader sortieren müssen wir sicherstellen, dass der Benutzer angefordert hat, dass die Daten sortiert werden. In der Standardeinstellung der GridView-s-Inhalt nicht sortiert sind, und daher Wir raten t muss jede Gruppe sortieren Header eingeben.

> [!NOTE]
> Wenn Sie möchten die GridView nach einer bestimmten Spalte sortiert werden, wenn die Seite zuerst geladen wird, rufen Sie die GridView-s `Sort` Methode ersten Besuch der Seite (aber nicht bei nachfolgenden Postbacks). Um dies zu erreichen, fügen Sie diesen Aufruf in die `Page_Load` Ereignishandler innerhalb einer `if (!Page.IsPostBack)` bedingte. Verweisen zurück auf die [Paging und Sortieren von Berichtsdaten](paging-and-sorting-report-data-vb.md) Tutorial Informationen Weitere Informationen zu den `Sort` Methode.


Vorausgesetzt, dass die Daten sortiert wurden, unsere nächste Aufgabe ist, um zu bestimmen, welche Spalten, klicken Sie dann Werte, die Unterschiede in der Spalte s nach Zeilen zu durchsuchen und die Daten sortiert wurde. Der folgende Code stellt sicher, dass die Daten sortiert wurden, und sucht nach der Spalte, die mit der die Daten sortiert wurden:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Wenn das GridView hat noch sein sortiert, die GridView s `SortExpression` Eigenschaft wurde wird nicht festgelegt. Daher möchten wir nur die Trennzeichen für Zeilen hinzugefügt werden soll, wenn diese Eigenschaft einen Wert verfügt. Wenn dies der Fall ist, müssen wir als Nächstes den Index der Spalte zu bestimmen, mit dem die Daten sortiert wurde. Dies geschieht durch Durchlaufen der GridView-s `Columns` Auflistung, die nach der Spalte gesucht, deren `SortExpression` Eigenschaft gleich die GridView-s `SortExpression` Eigenschaft. Zusätzlich zu den Spaltenindex s, nehmen wir außerdem die `HeaderText` -Eigenschaft, die verwendet wird, wenn die Trennzeichen für Zeilen anzeigen.

Der Index der Spalte mit der die Daten sortiert sind, ist der letzte Schritt zum Aufzählen von den Zeilen der GridView. Für jede Zeile müssen wir bestimmen, ob s der vorherigen Zeile s sortiert Spaltenwert der Wert des s sortierte Spalte unterscheidet. Wenn wir also eine neue einfügen müssen `GridViewRow` -Instanz in die Hierarchie der Steuerelemente. Dies erfolgt durch den folgenden Code:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Dieser Code beginnt, durch Programmgesteuertes Verweisen auf die `Table` Objekt am Stamm der GridView-s-Steuerelementhierarchie gefunden, und erstellen Sie eine Zeichenfolgenvariable mit dem Namen `lastValue`. `lastValue` Dient zum Wert der aktuellen Zeile s sortiert Spalte mit der vorherigen s-Zeilenwert verglichen werden soll. Anschließend wird das GridView-s `Rows` Auflistung aufgezählt und für jede Zeile befindet sich der Wert, der die sortierte Spalte in der `currentValue` Variable.

> [!NOTE]
> Zum Bestimmen des Werts der Spalte sortiert s bestimmten Zeile die Zelle s verwende `Text` Eigenschaft. Dies eignet sich gut für BoundFields, werden jedoch nicht wie gewünscht für von TemplateFields, CheckBoxFields, arbeiten und so weiter. Wir werden wie alternative GridView-Felder in Kürze berücksichtigen betrachten.


Die `currentValue` und `lastValue` Variablen werden dann verglichen. Wenn Unterschiede festgestellt werden, müssen wir eine neue Trennzeichenzeile für die Hierarchie der Steuerelemente hinzu. Dies wird erreicht, indem bestimmt den Index des der `GridViewRow` in die `Table` s-Objekt `Rows` -Auflistung, Erstellen neuer `GridViewRow` und `TableCell` Instanzen und zum anschließenden Hinzufügen der `TableCell` und `GridViewRow` auf der Hierarchie der Steuerelemente.

Beachten Sie, dass das Trennzeichen für s, die einzelne Zeile `TableCell` ist so formatiert, dass es sich um die gesamte Breite des GridView umfasst mit formatiert wird die `SortHeaderRowStyle` CSS-Klasse, und verfügt über seine `Text` Eigenschaft z. B., dass sowohl die Gruppe für das Sortieren angezeigt (z. B. Kategorie) zu benennen und der Gruppe-s-Wert (z. B. Getränke). Zum Schluss `lastValue` wird aktualisiert, um den Wert der `currentValue`.

Die CSS-Klasse, die zum Formatieren der Sortierung Gruppenkopfzeile verwendet `SortHeaderRowStyle` in angegeben werden, muss die `Styles.css` Datei. Verwenden Sie die formateinstellungen für Sie interessant sein können, Ich habe verwendet Folgendes:


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Durch den aktuellen Code aus, die Sortierung-Schnittstelle fügt Kopfzeilen von Gruppen sortieren, beim Sortieren nach jeder BoundField (siehe Abbildung 5, die einen Screenshot beim Sortieren von Lieferant zeigt). Allerdings sind beim Sortieren von jedem anderen Feldtyp (z. B. eine CheckBoxField oder TemplateField) der Sort-Gruppenheader an keiner Stelle (siehe Abbildung 6) gefunden werden.


[![Die Sortierung Schnittstelle enthält Header, Gruppe sortieren, beim Sortieren nach BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Abbildung 5**: das Sortieren-Schnittstelle enthält sortieren Header beim Sortieren von Gruppen von BoundFields ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Der Sort-Gruppenheader sind fehlende beim Sortieren einer CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Abbildung 6**: die Kopfzeilen von Gruppen sortieren sind fehlende beim Sortieren einer CheckBoxField ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


Der Sort-Gruppenheader beim Sortieren nach einer CheckBoxField fehlen ist, weil der Code derzeit nur verwendet die `TableCell` s `Text` Eigenschaft zum Bestimmen des Werts, der die sortierte Spalte für jede Zeile. Für CheckBoxFields die `TableCell` s `Text` Eigenschaft ist eine leere Zeichenfolge; stattdessen des Werts über ein Kontrollkästchen-Steuerelement, das innerhalb der `TableCell` s `Controls` Auflistung.

Um Feldtypen als BoundFields behandeln zu können, müssen wir den Code zu erweitern, in denen die `currentValue` Variable zugewiesen wird überprüft, ob ein Kontrollkästchen in der `TableCell` s `Controls` Auflistung. Anstelle von `currentValue = gvr.Cells(sortColumnIndex).Text`, ersetzen Sie diesen Code durch Folgendes:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Dieser Code überprüft die sortierte Spalte `TableCell` für die aktuelle Zeile zu bestimmen, ob alle Steuerelemente in der `Controls` Auflistung. Wenn Sie vorhanden sind, und das erste Steuerelement ein Kontrollkästchen, die `currentValue` Variable wird festgelegt, auf Ja oder Nein, je nach den Kontrollkästchen s `Checked` Eigenschaft. Der Wert stammt, andernfalls aus der `TableCell` s `Text` Eigenschaft. Diese Logik kann repliziert werden, um Sortierung für alle von TemplateFields, die ggf. in der GridView vorhanden, zu verarbeiten.

Mit dem obigen Code außerdem der Sort-Gruppenheader vorhanden sind beim Sortieren nach CheckBoxField nicht mehr unterstützt (siehe Abbildung 7).


[![Der Sort-Gruppenheader sind jetzt vorhanden beim Sortieren einer CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Abbildung 7**: die Kopfzeilen von Gruppen sortieren sind jetzt vorhanden beim Sortieren einer CheckBoxField ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Wenn Sie über Produkte mit `NULL` Datenbank Werte für die `CategoryID`, `SupplierID`, oder `UnitPrice` Felder, diese Werte als leere Zeichenfolgen in den GridView-Ansicht angezeigt werden, standardmäßig, d. h. die Trennzeichen Zeilentext s für diese Produkte mit `NULL`Werte gelesen werden, wie Kategorie: (d. h. es s keine Namen nach Kategorie: wie Sie mit der Kategorie: Getränke). Wenn ein Wert, der hier angezeigt werden soll, können Sie entweder die BoundFields festlegen [ `NullDisplayText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) auf den Text, angezeigt werden sollen oder Sie können eine bedingte Anweisung in der Render-Methode hinzufügen, beim Zuweisen der `currentValue` auf das Trennzeichen Zeile s `Text` Eigenschaft.


## <a name="summary"></a>Zusammenfassung

Das GridView enthält viele integrierte Optionen für das Anpassen der Benutzeroberfläche der Sortierung keine. Allerdings mit etwas Code auf niedriger Ebene es möglich, optimieren die GridView-s-Steuerelementhierarchie um eine individuellere Benutzeroberfläche zu erstellen. In diesem Tutorial wurde erläutert, wie eine Art Separator Gruppenzeile ein sortierbar GridView, hinzufügen. die unterschiedlichen Gruppen und diesen Gruppen Grenzen leichter identifiziert. Weitere Beispiele für benutzerdefinierte Sortierung Schnittstellen, sehen Sie sich [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [einige ASP.NET 2.0 GridView sortieren Tipps und Tricks](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) Blogeintrag.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Vorherige](sorting-custom-paged-data-vb.md)
