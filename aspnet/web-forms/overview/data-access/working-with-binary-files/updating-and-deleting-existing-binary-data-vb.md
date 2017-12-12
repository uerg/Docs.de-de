---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: "Aktualisieren und Löschen von vorhandenen Binärdaten (VB) | Microsoft Docs"
author: rick-anderson
description: "In früheren Lernprogrammen haben wir gesehen, wie des GridView-Steuerelements sie auf einfache Weise bearbeiten und Löschen von Textdaten. In diesem Lernprogramm wird ersichtlich, wie des GridView-Steuerelements auch stellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e14b19f99e9f41c5a296d73ba689095a686794db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualisieren und Löschen von vorhandenen Binärdaten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) oder [PDF herunterladen](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> In früheren Lernprogrammen haben wir gesehen, wie des GridView-Steuerelements sie auf einfache Weise bearbeiten und Löschen von Textdaten. In diesem Lernprogramm wird ersichtlich, wie des GridView-Steuerelements auch es ermöglicht, bearbeiten und Löschen von binären Daten, ob die binären Daten in der Datenbank gespeichert oder im Dateisystem gespeichert werden.


## <a name="introduction"></a>Einführung

Über die letzten drei Lernprogramme wir Ve hinzugefügt relativ viel Funktionalität für das Arbeiten mit Binärdaten. Wir gestartet, durch Hinzufügen einer `BrochurePath` Spalte der `Categories` -Tabelle und die Architektur entsprechend aktualisiert. Wir auch die Datenzugriffsebene und Business Logic Layer Methoden zum Arbeiten mit dem vorhandenen zu Kategorien Tabelle s hinzugefügt `Picture` Spalte, die den binären Inhalt s einer Bilddatei enthält. Es erstellt haben, Webseiten, um den binären Daten in einem GridView einen Downloadlink, um die Broschüren in angezeigten Bilds für die Kategorie s stellen eine `<img>` Element und eine DetailsView, damit Benutzer eine neue Kategorie hinzufügen und seine Daten Broschüren und Bild hochgeladen wurden hinzugefügt.

Alle bleibt implementiert werden ist die Fähigkeit zum Bearbeiten und löschen vorhandene Kategorien erreichen wir in diesem Lernprogramm mithilfe der GridView s integrierte bearbeiten und Löschen von Funktionen. Wenn Sie eine Kategorie zu bearbeiten, wird der Benutzer sein, wahlweise ein neues Bild hochladen oder verfügen über die Kategorie weiterhin die vorhandene Konfiguration zu verwenden. Für die Broschüren können sie wahlweise die vorhandenen Broschüren verwenden, um eine neue Broschüren hochzuladen oder um anzugeben, dass die Kategorie nicht mehr eine Broschüren zugeordnet wurde. Lassen Sie s beginnen!

## <a name="step-1-updating-the-data-access-layer"></a>Schritt 1: Aktualisieren der Datenzugriffsebene

Die DAL wurde automatisch generierter `Insert`, `Update`, und `Delete` Methoden, aber diese Methoden wurden generiert basierend auf der `CategoriesTableAdapter` s Hauptabfrage, zzgl. der `Picture` Spalte. Aus diesem Grund die `Insert` und `Update` Methoden verwenden Sie keine Parameter für die binären Daten für das Bild Kategorie s angeben. Wie der [vorherigen Lernprogramm](including-a-file-upload-option-when-adding-a-new-record-vb.md), wir erstellen eine neue TableAdapter-Methode für die Aktualisierung müssen die `Categories` Tabelle, wenn Binärdaten angeben.

Öffnen Sie das typisierte DataSet, und klicken Sie im Designer mit der Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie Abfrage hinzufügen im Kontextmenü der TableAdapter-Abfragekonfigurations-Assistent, Launche. Bitten Sie uns mit, wie die TableAdapter-Abfrage für die Datenbank zugreifen, sollte startet dieser Assistent. Wählen Sie SQL-Anweisungen, und klicken Sie auf Weiter. Als Nächstes fordert für den Typ der Abfrage generiert werden soll. Da wir re erstellen eine Abfrage aus, um einen neuen Datensatz zum Hinzufügen der `Categories` Tabelle, wählen Sie aktualisieren, und klicken Sie auf Weiter.


[![Wählen Sie die Updateoption](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Abbildung 1**: Wählen Sie die UPDATE-Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Jetzt müssen wir geben die `UPDATE` SQL-Anweisung. Der Assistent automatisch schlägt vor ein `UPDATE` Anweisung, die Hauptabfrage des TableAdapter s entspricht (, der aktualisiert die `CategoryName`, `Description`, und `BrochurePath` Werte). Ändern Sie die Anweisung, damit die `Picture` Spalte dient zusammen mit einem `@Picture` -Parameter wie folgt:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Im letzten Bildschirm des Assistenten werden Sie gefragt, benennen Sie die neue Methode des TableAdapter. Geben Sie `UpdateWithPicture` , und klicken Sie auf ' Fertig stellen '.


[![Name der neuen Methode des TableAdapter UpdateWithPicture](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Abbildung 2**: Benennen Sie die neue Methode des TableAdapter `UpdateWithPicture` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Schritt 2: Hinzufügen, die Business Logic Layer-Methoden

Zusätzlich zum Aktualisieren der DAL, müssen Sie die BLL Einbeziehung von Methoden zum Aktualisieren und Löschen einer Kategorie zu aktualisieren. Dies sind die Methoden, die von der Darstellungsschicht aufgerufen werden.

Für eine Kategorie löschen, verwenden wir die `CategoriesTableAdapter` s, die automatisch generierte `Delete` Methode. Fügen Sie die folgende Methode, die `CategoriesBLL` Klasse:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Für dieses Lernprogramm Let s erstellen Sie zwei Methoden zum Aktualisieren einer Kategorie - erwartet, dass die binäre Bilddaten und ruft die `UpdateWithPicture` Methode, die wir gerade hinzugefügt haben, die `CategoriesTableAdapter` und ein anderes, das akzeptiert nur die `CategoryName`, `Description`, und `BrochurePath`und verwendet `CategoriesTableAdapter` s, die automatisch generierte Klasse `Update` Anweisung. Der Grund für die Verwendung der beiden Methoden ist, dass in einigen Fällen ein Benutzer möchten die Kategorie-s-Grafik zusammen mit ihren anderen Feldern zu aktualisieren, in dem Fall muss der Benutzer hat das neue Bild hochladen. Die Binärdaten hochgeladene Bild s können dann verwendet werden, der `UPDATE` Anweisung. In anderen Fällen kann der Benutzer nur aktualisieren, z. B. der Name und Beschreibung "interested" sein. Jedoch, wenn die `UPDATE` Anweisung erwartet, dass die binären Daten der `Picture` Spalte ebenfalls, wählen Sie dann d müssen auch diese Informationen bereitstellen. In diesem Fall eine zusätzliche Roundtrips zur Datenbank die Bilddaten für den zu bearbeitenden Datensatz wieder zurückbekommen erforderlich. Daher sollten wir zwei `UPDATE` Methoden. Der Business Logic Layer wird bestimmt, welcher Typ verwendet, ob die Bilddaten, beim Aktualisieren der Kategorie bereitgestellt werden Grundlage.

Um dies zu ermöglichen, zwei Methoden zum Hinzufügen der `CategoriesBLL` Klasse, die sowohl mit dem Namen `UpdateCategory`. Die erste Vorlage sollte drei akzeptieren `String` s, eine `Byte` Array ist, und ein `Integer` als Eingabe Parameter; das zweite, nur drei `String` s und einer `Integer`. Die `String` Eingabeparameter sind für den Kategorienamen s, Beschreibung und Broschüren Dateipfad, der `Byte` Array ist, für den binären Inhalt des Bilds Kategorie s und die `Integer` identifiziert die `CategoryID` des Datensatzes zu aktualisieren. Beachten Sie, dass die erste Überladung, der zweiten, wenn das übergebene in aufruft `Byte` Array ist `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Schritt 3: Kopieren, über das Einfügen und Sicht-Funktionen

In der [vorherigen Lernprogramm](including-a-file-upload-option-when-adding-a-new-record-vb.md) es erstellt eine Seite mit dem Namen `UploadInDetailsView.aspx` , die alle Kategorien in einer GridView aufgelistet und eine DetailsView zum Hinzufügen von neuer Kategorien mit dem System bereitgestellt. In diesem Lernprogramm werden wir die GridView Einbeziehung von bearbeiten und Löschen von Unterstützung erweitern. Anstatt zu fortfahren mit dem Sie arbeiten `UploadInDetailsView.aspx`, Let s platzieren stattdessen dieses Lernprogramm s Änderungen in der `UpdatingAndDeleting.aspx` Seite aus dem gleichen Ordner `~/BinaryData`. Kopieren und fügen Sie das deklarative Markup aus code `UploadInDetailsView.aspx` auf `UpdatingAndDeleting.aspx`.

Öffnen Sie zunächst die `UploadInDetailsView.aspx` Seite. Kopieren Sie die deklarative Syntax innerhalb der `<asp:Content>` Element, wie in Abbildung 3 dargestellt. Öffnen Sie als Nächstes `UpdatingAndDeleting.aspx` , und fügen Sie diesem Markup innerhalb seiner `<asp:Content>` Element. Auf ähnliche Weise, kopieren Sie den Code aus der `UploadInDetailsView.aspx` Seite s Code-Behind-Klasse, um `UpdatingAndDeleting.aspx`.


[![Kopieren Sie das deklarative Markup aus UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Abbildung 3**: Kopieren Sie das Deklarationsmarkup aus `UploadInDetailsView.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Nach dem Kopieren über deklaratives Markup und Code, besuchen Sie `UpdatingAndDeleting.aspx`. Daraufhin sollte die gleiche Ausgabe und haben die gleiche benutzererfahrung wie bei `UploadInDetailsView.aspx` Seite aus dem vorherigen Lernprogramm.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Schritt 4: Hinzufügen von Support, um das ObjectDataSource und GridView löschen

Wie wieder in erläutert der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Lernprogramm GridView bietet integrierte Löschen von Funktionen und diese Funktionen können an der Takt des ein Kontrollkästchen aktiviert werden, wenn die zugrunde liegende Tabelle s unterstützt die Datenquelle löschen. Derzeit das ObjectDataSource GridView gebunden ist (`CategoriesDataSource`) durch das Löschen nicht unterstützt.

Zur Behebung des Problems, klicken Sie auf die Option über das ObjectDataSource-s-Smarttag Konfigurieren von Datenquellen aus, um den Assistenten zu starten. Der erste Bildschirm zeigt, dass das ObjectDataSource konfiguriert ist, für die Zusammenarbeit mit dem `CategoriesBLL` Klasse. Drücken Sie weiter. Derzeit nur die ObjectDataSource e `InsertMethod` und `SelectMethod` Eigenschaften angegeben werden. Allerdings der Assistenten automatisch ausgefülltes Dropdownlisten in Update- und DELETE-Registerkarten mit den `UpdateCategory` und `DeleteCategory` Methoden bzw. Grund hierfür ist, in der `CategoriesBLL` wir markiert sind diese Methoden, die mithilfe der `DataObjectMethodAttribute` als die standardmäßigen Methoden zum Aktualisieren und löschen.

Vorerst Wert der UPDATE-Registerkarte "s"-Dropdownliste auf (keine), aber belassen Sie die DELETE Registerkarte "s" Dropdown-Liste festgelegt `DeleteCategory`. Wir müssen zu diesem Assistenten in Schritt 6 Update Unterstützung hinzuzufügen zurück.


[![Konfigurieren der ObjectDataSource zur Verwendung der DeleteCategory-Methode](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource verwenden die `DeleteCategory` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Nach Abschluss des Assistenten, kann Visual Studio bitten Sie Sie nach Bedarf aktualisieren von Feldern und Schlüssel, die die Daten Web neu generieren, wird Felder steuert. Wählen Sie Nein, da Ja Feld Anpassungen überschrieben werden, die Sie vorgenommen haben, können.


Das ObjectDataSource enthält jetzt einen Wert für die `DeleteMethod` Eigenschaft als auch eine `DeleteParameter`. Beachten Sie, dass bei Verwendung des Assistenten, die Methoden angeben, Visual Studio das ObjectDataSource-s legt `OldValuesParameterFormatString` Eigenschaft `original_{0}`, die führt zu Problemen bei der Aktualisierung und Löschen von Methodenaufrufen. Aus diesem Grund diese Eigenschaft zu löschen oder ihn auf den Standardwert zurücksetzen `{0}`. Wenn Sie Ihren Speicher für diese Eigenschaft ObjectDataSource aktualisieren müssen, finden Sie unter der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Lernprogramm.

Nach dem Abschließen des Assistenten und zum Beheben der `OldValuesParameterFormatString`, das ObjectDataSource s deklarative Markup sollte wie folgt aussehen wie folgt:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Fügen Sie nach dem Konfigurieren der ObjectDataSource, Löschen von Funktionen an die GridView durch Aktivieren des Kontrollkästchens löschen aktivieren aus dem GridView-s-Smarttag. Dadurch wird eine CommandField an die GridView hinzugefügt, deren `ShowDeleteButton` -Eigenschaftensatz auf `True`.


[![Aktivieren Sie Unterstützung für das Löschen von in der GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Abbildung 5**: Aktivieren der Unterstützung für das Löschen in die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Nehmen Sie einen Moment Zeit, um die Delete-Funktionalität zu testen. Es ist ein Fremdschlüssel zwischen den `Products` Tabelle s `CategoryID` und die `Categories` Tabelle s `CategoryID`, sodass eine Verletzung-Ausnahme von foreign Key-Einschränkung erhalten Sie, wenn Sie versuchen, den ersten acht Kategorien zu löschen. Um diese Funktion heraus zu testen, fügen Sie eine neue Kategorie, die sowohl eine Broschüren und Bild hinzu. Meine Testkategorie in Abbildung 6 dargestellten enthält Test Broschüren-Datei mit dem Namen `Test.pdf` und ein Testbild. Abbildung 7 zeigt die GridView, nachdem die Testkategorie hinzugefügt wurde.


[![Fügen Sie einer Testkategorie mit einem Broschüren und ein Bild hinzu](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen einer Testkategorie mit einem Broschüren und Image ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Nach dem Einfügen der Testkategorie, wird er in der GridView angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Abbildung 7**: nach dem Einfügen der Testkategorie an, in die GridView angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


Aktualisieren Sie in Visual Studio im Projektmappen-Explorer aus. Jetzt sollte eine neue Datei in die `~/Brochures` Ordner `Test.pdf` (siehe Abbildung 8).

Klicken Sie anschließend auf den Link "löschen" in der Testkategorie Zeile die Seite, um postback verursacht und die `CategoriesBLL` Klasse s `DeleteCategory` Methode ausgelöst werden. Dadurch wird die DAL s aufgerufen `Delete` -Methode, verursacht das entsprechende `DELETE` Anweisung an die Datenbank gesendet werden. Die Daten dann an die GridView gebunden ist, und das Markup wird gesendet, an den Client mit der Testkategorie nicht mehr vorhanden.

Während der Delete-Workflow entfernte der Testkategorie-Eintrag aus der `Categories` Tabelle, es seine Broschüren-Datei nicht aus dem Web Server s Dateisystem entfernt. Aktualisieren Sie im Projektmappen-Explorer, und Sie sehen, dass `Test.pdf` ist immer noch im Ruhezustand der `~/Brochures` Ordner.


![Die Test.pdf-Datei wurde nicht aus dem Web Server s Dateisystem gelöscht.](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Abbildung 8**: die `Test.pdf` Datei wurde nicht aus dem Web Server s Dateisystem gelöscht


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Schritt 5: Entfernen der gelöschten Kategoriedatei s Broschüren

Eine die Nachteile der binären Daten außerhalb der Datenbank gespeichert ist, dass zusätzliche Schritte ausgeführt werden müssen, um diese Dateien bereinigt, wenn die zugeordnete Datenbankdatensatz gelöscht wird. Geben Sie die GridView und ObjectDataSource Ereignisse, die ausgelöst werden, bevor und nachdem die Delete-Befehl ausgeführt wurde. Wir müssen tatsächlich Ereignishandler für die Ereignisse vor und nach Abschluss der Aktion zu erstellen. Vor der `Categories` Datensatz gelöscht müssen wir die PDF-Datei "s" Pfad zu ermitteln, aber wir Verschlüsselungskennwort, die PDF-Datei zu löschen, bevor die Kategorie gelöscht wird, falls eine Ausnahme aufgetreten ist und die Kategorie ist nicht gelöscht werden soll, t.

Die GridView-s [ `RowDeleting` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) ausgelöst wird, bevor das ObjectDataSource-s-Delete-Befehl aufgerufen wurde, während er sich seine [ `RowDeleted` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) nach ausgelöst wird. Erstellen von Ereignishandlern für diese beiden Ereignisse mit dem folgenden Code:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

In der `RowDeleting` Ereignishandler, d. h. die `CategoryID` der Zeile gelöscht wird stammt aus dem GridView s `DataKeys` -Auflistung, die in diesem Ereignishandler durch zugegriffen werden kann die `e.Keys` Auflistung. Als Nächstes wird die `CategoriesBLL` Klasse s `GetCategoryByCategoryID(categoryID)` wird aufgerufen, um Informationen zu den gelöschten Datensatz zurückgegeben. Wenn das zurückgegebene `CategoriesDataRow` Objekt verfügt über einen nicht-`NULL``BrochurePath` Wert es in die Seite "-Variable gespeichert `deletedCategorysPdfPath` , damit die Datei gelöscht werden kann, in der `RowDeleted` -Ereignishandler.

> [!NOTE]
> Anstatt Abrufen der `BrochurePath` details für die `Categories` aufzeichnen gelöscht wird, der `RowDeleting` Ereignishandler, d. h. es Alternativ hätten die `BrochurePath` mit dem GridView-s `DataKeyNames` Eigenschaft und Zugriff auf den Datensatz s-Wert über die `e.Keys` Auflistung. Auf diese Weise würde etwas vergrößern die GridView s Ansicht Status, aber würde verringern Sie die Menge des Codes erforderlich sind und einem Vorgang in der Datenbank speichern.


Nach der ObjectDataSource s zugrunde liegenden Delete-Befehl aufgerufen wurde, die GridView s `RowDeleted` Handler-Ereignis ausgelöst. Wenn es keine Ausnahmen gab in die Daten löschen und es ein Wert für ist `deletedCategorysPdfPath`, und klicken Sie dann die PDF-Datei aus dem Dateisystem gelöscht wird. Beachten Sie, dass diese zusätzliche Code nicht benötigt wird, um die Kategorie s zugeordneten Binärdaten ein Bild zu bereinigen. S, da die Bilddaten direkt in der Datenbank gespeichert werden, daher beim Löschen der `Categories` Zeilen auch dieses Bild Kategoriedaten s gelöscht.

Wenn die beiden Ereignishandler hinzugefügt haben, führen Sie diesen Testfall erneut aus. Wenn Sie die Kategorie löschen, wird seine zugeordneten PDF ebenfalls gelöscht.

Aktualisieren einer vorhandenen Datensatz s, die Binärdaten enthält einige interessanten Herausforderungen. Im weiteren Verlauf dieses Lernprogramms sind vertiefende Themen in der Broschüren und Bild Updatefunktionen hinzugefügt. Schritt 6 untersucht Techniken zum Aktualisieren der Broschüren während Schritt 7 sucht, auf das Bild aktualisieren.

## <a name="step-6-updating-a-category-s-brochure"></a>Schritt 6: Aktualisieren einer Kategorie s Broschüren

Wie in diesem Lernprogramm eine Übersicht der einfügen, aktualisieren und Löschen von Daten erläutert wird, bietet die GridView integrierten Bearbeitung auf Zeilenebene-Unterstützung, der von der Takt des ein Kontrollkästchen implementiert werden kann, wenn der zugrunde liegenden Datenquelle ordnungsgemäß konfiguriert ist. Derzeit die `CategoriesDataSource` ObjectDataSource wurde noch nicht Einbeziehung der Unterstützung, aktualisieren, sodass Let s hinzufügen, die in konfiguriert.

Klicken Sie auf den Link "Datenquelle konfigurieren" aus dem ObjectDataSource-s-Assistenten aus, und fahren Sie mit dem zweiten Schritt fort. Aufgrund der der `DataObjectMethodAttribute` verwendet `CategoriesBLL`, die UPDATE-Dropdownliste mit automatisch aufgefüllt werden soll die `UpdateCategory` Überladung, die vier Eingabeparameter akzeptiert (für alle Spalten jedoch `Picture`). Ändern Sie ihn so, dass sie die Überladung mit fünf Parametern verwendet.


[![Konfigurieren der ObjectDataSource zur Verwendung der UpdateCategory-Methode, die einen Parameter für das Bild enthält](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Abbildung 9**: Konfigurieren der ObjectDataSource verwenden die `UpdateCategory` Methode, die einen Parameter für enthält `Picture` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


Das ObjectDataSource enthält jetzt einen Wert für die `UpdateMethod` Eigenschaft sowie entsprechende `UpdateParameter` s. Wie in Schritt 4 notiert haben, legt Visual Studio das ObjectDataSource-s `OldValuesParameterFormatString` Eigenschaft `original_{0}` bei Verwendung des Assistenten für die Datenquelle konfigurieren. Dies wird dazu führen, dass Probleme mit dem Update und Methodenaufrufe löschen. Aus diesem Grund diese Eigenschaft zu löschen oder ihn auf den Standardwert zurücksetzen `{0}`.

Nach dem Abschließen des Assistenten und zum Beheben der `OldValuesParameterFormatString`, das ObjectDataSource s deklarative Markup sollte folgendermaßen aussehen:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Aktivieren Sie die Option Bearbeiten aktivieren, aus dem Smarttag des GridView s, um die GridView s integrierten Bearbeitungsfunktionen zu aktivieren. Dadurch wird festgelegt, die CommandField s `ShowEditButton` Eigenschaft `True`, wodurch das Hinzufügen einer Schaltfläche "Bearbeiten" (und die Schaltflächen, die mit der bearbeiteten Zeile aktualisieren "und" Abbrechen "").


[![Konfigurieren Sie die GridView zu Bearbeitung unterstützen](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Abbildung 10**: Konfigurieren Sie die GridView zu Bearbeitung unterstützen ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Besuchen Sie die Seite über einen Browser, und klicken Sie auf eines der Zeile s Schaltflächen bearbeiten. Die `CategoryName` und `Description` BoundFields als Textfelder gerendert werden. Die `BrochurePath` TemplateField verfügt nicht über ein `EditItemTemplate`, sodass es weiterhin angezeigt seine `ItemTemplate` einen Link zu der Broschüren. Die `Picture` ImageField als ein Textfeld rendert, deren `Text` Eigenschaft der Wert der ImageField s zugewiesen `DataImageUrlField` Wert, in diesem Fall `CategoryID`.


[![Die GridView verfügt nicht über eine Bearbeitung Schnittstelle für BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Abbildung 11**: GridView verfügt nicht über eine Schnittstelle bearbeiten für `BrochurePath` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Anpassen der`BrochurePath`s bearbeiten-Schnittstelle

Wir müssen eine Bearbeitung Schnittstelle zum Erstellen der `BrochurePath` TemplateField, eine, die dem Benutzer, entweder ermöglicht:

- Lassen Sie die Kategorie s Broschüren als-ist,
- Aktualisieren Sie die Kategorie s Broschüren durch Hochladen einer neuen Broschüren oder
- Entfernen Sie die Kategorie s Broschüren vollständig (im Fall, dass die Kategorie nicht mehr eine zugeordnete Broschüren verfügt).

Wir müssen auch zum Aktualisieren der `Picture` ImageField s Bearbeitungsoberfläche, aber wir erhalten, dies in Schritt 7.

GridView smart Tag s, klicken Sie auf den Link Vorlagen bearbeiten, und wählen die `BrochurePath` TemplateField s `EditItemTemplate` aus der Dropdown-Liste. Hinzufügen eines RadioButtonList Web-Steuerelements zu dieser Vorlage festlegen seiner `ID` Eigenschaft, um `BrochureOptions` und seine `AutoPostBack` Eigenschaft `True`. Eigenschaftenfenster, klicken Sie auf die Schaltfläche mit den Auslassungszeichen in der `Items` -Eigenschaft, die Hochfahren wird die `ListItem` Auflistungs-Editor. Fügen Sie die folgenden drei Optionen mit `Value` s, 1, 2 und 3, bzw.:

- Verwenden Sie die aktuelle Broschüren
- Entfernen Sie die aktuelle Broschüren
- Neue Broschüren hochladen

Legen Sie die erste `ListItem` s `Selected` Eigenschaft `True`.


![Drei ListItems RadioButtonList hinzufügen](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Abbildung 12**: Fügen Sie drei `ListItem` s, um die RadioButtonList


Fügen Sie unterhalb der RadioButtonList ein FileUpload-Steuerelement namens `BrochureUpload`. Legen Sie dessen `Visible` Eigenschaft `False`.


[![ItemTemplate ein RadioButtonList und FileUpload-Steuerelement hinzufügen](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Abbildung 13**: Hinzufügen eines RadioButtonList und FileUpload-Steuerelement, um die `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Diese RadioButtonList bietet drei Optionen für den Benutzer. Die Idee dabei ist, dass das FileUpload-Steuerelement angezeigt wird, nur, wenn die letzte Möglichkeit, neue Broschüren hochladen, ausgewählt ist. Um dies zu erreichen, erstellen Sie einen Ereignishandler für das RadioButtonList s `SelectedIndexChanged` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Da die RadioButtonList und FileUpload-Steuerelemente in einer Vorlage befinden, haben wir zum Schreiben von viel Code auf diese Steuerelemente programmgesteuert zugreifen. Die `SelectedIndexChanged` übergebene Ereignishandler ist eine Referenz zu RadioButtonList in die `sender` Eingabeparameter. Um das Steuerelement FileUpload zu erhalten, müssen wir RadioButtonList s übergeordneten Steuerelement und Verwendung erhalten die `FindControl("controlID")` Methode von dort aus. Nachdem wir einen Verweis auf die RadioButtonList und die FileUpload Steuerelemente verfügen, steuern die FileUpload s `Visible` -Eigenschaftensatz auf `True` nur, wenn die RadioButtonList s `SelectedValue` gleich 3 ist der `Value` für die neue Broschüren hochladen `ListItem`.

Nehmen Sie mit diesem Code werden einen Moment Zeit, die Bearbeitungsoberfläche zu testen. Klicken Sie auf die Schaltfläche "Bearbeiten" für eine Zeile. Zunächst muss die Option zum Verwenden einer aktuellen Broschüren ausgewählt werden. Ändern den ausgewählten Index bewirkt, dass einen Postback. Wenn die dritte Option ausgewählt ist, das FileUpload-Steuerelement angezeigt wird, andernfalls wird es ausgeblendet. Abbildung 14 zeigt die Bearbeitungsoberfläche, wenn auf die Schaltfläche "Bearbeiten" zuerst geklickt wird; Abbildung 15 zeigt die Benutzeroberfläche an, nach dem Hochladen der neuen Broschüren Option ausgewählt ist.


[![Zu Beginn der Verwendung aktuelle Broschüren Option ausgewählt ist](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Abbildung 14**: zu Beginn der Verwendung aktuelle Broschüren ausgewählt ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Der Upload neue Broschüren Option zeigt auswählen die FileUpload-Steuerelement.](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Abbildung 15**: Auswählen der Upload neue Broschüren Option zeigt das Steuerelement FileUpload ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Speichern die Broschüren Datei- und Aktualisieren der`BrochurePath`Spalte

Wenn die Schaltfläche "GridView s aktualisieren" geklickt wird, dessen `RowUpdating` Ereignis ausgelöst wird. Das ObjectDataSource Aktualisierungsbefehl für s aufgerufen wird, und klicken Sie dann die GridView-s `RowUpdated` -Ereignis ausgelöst. Wie muss mit dem Löschen von Workflow-Ereignishandler für beide Ereignisse erstellt werden. In der `RowUpdating` Ereignishandler, d. h. wir benötigen, um zu bestimmen, welche Aktion anhand der `SelectedValue` von der `BrochureOptions` RadioButtonList:

- Wenn die `SelectedValue` 1 ist, möchten wir die gleiche weiterhin verwenden `BrochurePath` Einstellung. Aus diesem Grund müssen wir das ObjectDataSource-s festlegen `brochurePath` Parameter zur vorhandenen `BrochurePath` Wert, der dem aktualisierten Datensatz gelesen. Das ObjectDataSource-s `brochurePath` Parameter kann festgelegt werden, mithilfe von `e.NewValues["brochurePath"] = value`.
- Wenn die `SelectedValue` 2 ist, dann wird den Datensatz s festlegen möchten `BrochurePath` Wert `NULL`. Dies geschieht durch Festlegen der ObjectDataSource s `brochurePath` Parameter `Nothing`, was dazu führt, in einer Datenbank `NULL` verwendet wird, der `UPDATE` Anweisung. Ist eine vorhandene Broschüren-Datei, die entfernt wird, müssen wir die vorhandene Datei zu löschen. Allerdings möchten wir nur vorgehen, wenn das Update abgeschlossen wurde, ohne eine Ausnahme auszulösen.
- Wenn die `SelectedValue` ist 3, und klicken Sie dann möchten wir stellen Sie sicher, dass der Benutzer eine PDF-Datei hochgeladen wurde und im Dateisystem zu speichern, und aktualisieren Sie den Datensatz s `BrochurePath` Spaltenwert. Darüber hinaus ist eine vorhandene Broschüren-Datei, die ersetzt wird, muss die vorherige Datei zu löschen. Allerdings möchten wir nur vorgehen, wenn das Update abgeschlossen wurde, ohne eine Ausnahme auszulösen.

Die erforderlichen Schritte zum Abschluss der RadioButtonList s `SelectedValue` ist 3 sind nahezu identisch, mit denen von der DetailsView s verwendet `ItemInserting` -Ereignishandler. Dieser Ereignishandler wird ausgeführt, wenn ein neuer Kategoriedatensatz aus dem DetailsView-Steuerelement hinzugefügt wird, wir, in hinzugefügt, der [vorherigen Lernprogramm](including-a-file-upload-option-when-adding-a-new-record-vb.md). Aus diesem Grund behooves uns diese Funktionalität out in separate Methoden umgestaltet. Insbesondere verschoben ich die allgemeine Funktionalität in zwei Methoden:

- `ProcessBrochureUpload(FileUpload, out bool)`als Eingabe akzeptiert eine Instanz eines FileUpload und eine Ausgabe boolescher Wert, der angibt, ob der Vorgang zum Löschen oder bearbeiten fortgesetzt werden soll, oder wenn es aufgrund eines Fehlers Überprüfung abgebrochen werden soll. Diese Methode gibt den Pfad zur gespeicherten Datei zurück oder `null` Wenn keine Datei gespeichert wurde.
- `DeleteRememberedBrochurePath`Löscht die Datei, die durch den Pfad in der Seite "-Variable angegebene `deletedCategorysPdfPath` Wenn `deletedCategorysPdfPath` nicht `null`.

Der Code für diese beiden Methoden folgt. Beachten Sie die Ähnlichkeit zwischen `ProcessBrochureUpload` und der s DetailsView `ItemInserting` -Ereignishandler aus dem vorherigen Lernprogramm. In diesem Lernprogramm wurden ich aktualisiert, DetailsView s Ereignishandler, um diese neuen Methoden verwenden. Laden Sie den Code, der diesem Lernprogramm aus, um die Änderungen, DetailsView-s-Ereignishandler finden Sie unter zugeordnet.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Die GridView s `RowUpdating` und `RowUpdated` Ereignishandler verwenden die `ProcessBrochureUpload` und `DeleteRememberedBrochurePath` Methoden, wie im folgenden Code dargestellt:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Hinweis wie die `RowUpdating` -Ereignishandler verwendet eine Reihe von bedingten Anweisungen zum Ausführen der entsprechenden Aktion basierend auf den `BrochureOptions` RadioButtonList s `SelectedValue` Eigenschaftswert.

Mit diesem Code werden können Sie bearbeiten eine Kategorie und damit er seine aktuelle Broschüren verwenden, verwenden keine Broschüren oder ein neues hochzuladen. Fahren Sie fort, und probieren Sie es aus. Legen Sie Haltepunkte in der `RowUpdating` und `RowUpdated` Ereignishandler, um einen Eindruck von den Workflows abzurufen.

## <a name="step-7-uploading-a-new-picture"></a>Schritt 7: Hochladen eines neuen Bilds

Die `Picture` ImageField s bearbeiten Benutzeroberfläche gerendert wird als ein Textfeld mit dem Wert aufgefüllt seine `DataImageUrlField` Eigenschaft. Während der Bearbeitung Workflows GridView übergibt einen Parameter an das ObjectDataSource mit den Namen des Parameters s den Wert der ImageField s `DataImageUrlField` -Eigenschaft und die Parameter-s-Wert den Wert eingegeben werden, in das Textfeld in der Schnittstelle bearbeiten. Dieses Verhalten ist geeignet, wenn das Bild als Datei im Dateisystem gespeichert wird und die `DataImageUrlField` enthält die vollständige URL des Bilds. Diese Umstände zeigt die Bearbeitungsoberfläche die Bild-URL für s in das Textfeld ein, die der Benutzer ändern können und wieder in der Datenbank gespeichert wurden. Gewährt, diese Standardeinstellung Schnittstelle verfügt über t ermöglicht dem Benutzer ein neues Image hochladen, aber es ist möglich, die URL des Bilds in einen anderen als den aktuellen Wert zu ändern. Für dieses Lernprogramm jedoch der Schnittstelle bearbeiten ImageField-s-Standard reicht nicht aus da die `Picture` binäre Daten direkt in der Datenbank gespeichert werden und die `DataImageUrlField` Eigenschaft enthält nur die `CategoryID`.

Um besser zu verstehen, was in unserem Lernprogramm geschieht, wenn ein Benutzer eine Zeile mit einem ImageField bearbeitet, betrachten Sie das folgende Beispiel: ein Benutzer eine Zeile mit bearbeitet `CategoryID` 10, verursacht die `Picture` ImageField als ein Textfeld mit dem Wert 10 gerendert. Stellen Sie sich vor, dass der Benutzer klickt auf die Schaltfläche "Aktualisieren" ändert den Wert im Textfeld auf 50. Ein Postback erfolgt und die GridView erstellt zunächst einen Parameter namens `CategoryID` mit dem Wert 50. Jedoch, bevor die GridView diesen Parameter gesendet (und die `CategoryName` und `Description` Parameter), er fügt die Werte aus der `DataKeys` Auflistung. Aus diesem Grund überschreibt die `CategoryID` Parameter mit der aktuellen Zeile s zugrunde liegende `CategoryID` Wert 10. Kurz gesagt, der Schnittstelle bearbeiten ImageField s hat keine Auswirkungen auf die Bearbeitung Workflow für dieses Lernprogramm da Namen von ImageField s `DataImageUrlField` -Eigenschaft und der Rasteransicht s `DataKey` Wert sind identisch.

Während der ImageField zum Anzeigen eines Bilds anhand von Datenbankdaten erleichtert, Verschlüsselungskennwort wir t ein Textfeld in der Bearbeitung-Schnittstelle bereitstellen möchten. Stattdessen möchten wir ein Steuerelement FileUpload bieten, die der Endbenutzer verwenden können, um die Kategorie s Bild zu ändern. Im Gegensatz zu den `BrochurePath` Wert für diese Lernprogramme wir Ve entschieden, um festzulegen, dass jeder Kategorie ein Bild aufweisen muss. Daher es nicht erforderlich, damit der Benutzer darauf hinweisen, dass keine zugeordnete Bild der Benutzer möglicherweise Laden Sie entweder ein neues Bild aus, oder lassen Sie das aktuelle Bild als Verschlüsselungskennwort-ist.

Um die Bearbeitung ImageField-s-Schnittstelle anzupassen, müssen wir in ein TemplateField konvertieren. Klicken Sie aus den GridView s smart Tag auf den Link für die Spalten bearbeiten, wählen Sie die ImageField und klicken Sie auf dieses Feld konvertieren in einen TemplateField-Link.


![Die ImageField in ein TemplateField konvertieren](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Abbildung 16**: die ImageField in ein TemplateField konvertieren


Konvertieren die ImageField in ein TemplateField auf diese Weise wird ein TemplateField mit zwei Vorlagen generiert. Wie in die folgende deklarative Syntax gezeigt, die `ItemTemplate` enthält ein Bild Web Steuerelement, dessen `ImageUrl` -Eigenschaft zugewiesen ist, basierend auf den ImageField s Databinding-Syntax mit `DataImageUrlField` und `DataImageUrlFormatString` Eigenschaften. Die `EditItemTemplate` ein Textfeld enthält, deren `Text` Eigenschaft auf den vom angegebenen Wert gebunden wird die `DataImageUrlField` Eigenschaft.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Wir aktualisieren müssen die `EditItemTemplate` , eine FileUpload-Steuerelement zu verwenden. Eine Verknüpfung im GridView s Smarttag klicken Sie auf die Vorlagen bearbeiten, und wählen Sie dann die `Picture` TemplateField s `EditItemTemplate` aus der Dropdown-Liste. In der Vorlage sehen Sie ein Textfeld, das dies zu entfernen. Ziehen Sie als Nächstes ein FileUpload-Steuerelement aus der Toolbox in die Vorlage festlegen seiner `ID` auf `PictureUpload`. Fügen Sie auch den Text Category s Bild ändern, geben Sie ein neues Bild. Lassen Sie um die Kategorie s Bild beizubehalten, das Feld leer, die Vorlage.


[![Hinzufügen eines Steuerelements FileUpload ItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Abbildung 17**: Hinzufügen eines Steuerelements FileUpload, um die `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Nach dem Anpassen der Benutzeroberfläche, auf der bearbeiten, anzuzeigen Sie den Status in einem Browser. Beim Anzeigen einer Zeile im Nur-Lese-Modus wird das kategoriebild s erreichte war es vor, aber durch Klicken auf die Schaltfläche "Bearbeiten" die Bild-Spalte als Text mit einem FileUpload Steuerelement rendert.


[![Die Bearbeitung Schnittstelle enthält ein FileUpload-Steuerelement](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Abbildung 18**: die Bearbeitung Schnittstelle enthält ein FileUpload-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Beachten Sie, dass das ObjectDataSource konfiguriert ist, rufen Sie auf der `CategoriesBLL` Klasse s `UpdateCategory` -Methode, die als Eingabe, die Binärdaten für das Bild als akzeptiert ein `Byte` Array. Wenn dieses Array stellt `Nothing`, jedoch die alternative `UpdateCategory` Überladung aufgerufen wird, welche Probleme die `UPDATE` SQL-Anweisung, die nicht verändert wird die `Picture` Spalte, wodurch verlassen die Kategorie s aktuelle Bild intakt. Aus diesem Grund in die GridView s `RowUpdating` Ereignishandler programmgesteuert verweisen müssen die `PictureUpload` FileUpload steuern und zu bestimmen, ob eine Datei hochgeladen wurde. Wenn eine wurde nicht hochgeladen, wir *nicht* möchten, geben Sie einen Wert für die `picture` Parameter. Andererseits, wenn eine Datei hochgeladen wurde, in der `PictureUpload` FileUpload-Steuerelement, wir möchten sicherstellen, dass es sich um JPG-Datei handelt. Wenn es ist, senden wir die binären Inhalt, auf das ObjectDataSource über die `picture` Parameter.

Wie durch den Code, der in Schritt 6 verwendet wird, großer Anteil des Codes benötigt hier bereits in der DetailsView s vorhanden ist `ItemInserting` -Ereignishandler. Aus diesem Grund ich Ve umgestaltet, die allgemeine Funktionalität in eine neue Methode `ValidPictureUpload`, und aktualisiert die `ItemInserting` -Ereignishandler, um diese Methode verwenden.

Fügen Sie den folgenden Code am Anfang der GridView s `RowUpdating` -Ereignishandler. Es wichtig, dass dieser Code den Code vorangestellt sein, die die Broschüren-Datei gespeichert werden, da wir t Verschlüsselungskennwort soll die Broschüren die Web Server s im Dateisystem zu speichern, wenn eine ungültige Bilddatei hochgeladen wird.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

Die `ValidPictureUpload(FileUpload)` Methode in einem Steuerelement FileUpload als die einzige Eingabeparameter akzeptiert und überprüft die hochgeladene Datei s-Erweiterung, um sicherzustellen, dass die hochgeladene Datei JPG; es wird nur aufgerufen, wenn eine Bilddatei hochgeladen wird. Wenn keine Datei hochgeladen wird, und klicken Sie dann der Bild-Parameter nicht festgelegt ist, und verwendet daher den Standardwert `Nothing`. Wenn ein Bild hochgeladen wurde und `ValidPictureUpload` gibt `True`, `picture` Parameter wird die Binärdaten des hochgeladene Bild zugewiesen, wenn die-Methode zurückgibt `False`, der Workflow wird abgebrochen, und der Ereignishandler beendet.

Die `ValidPictureUpload(FileUpload)` Methodencode, der von der DetailsView s umgestaltet wurde `ItemInserting` Ereignishandler, folgt:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Schritt 8: Ersetzen der ursprünglichen Kategorien Bilder mit JPG-Format

Beachten Sie, dass die ursprünglichen acht Kategorien Bilder Bitmap-Dateien, die in einer OLE-Header eingeschlossen werden. Nun, dass wir die Funktion zum Bearbeiten eines vorhandenen Datensatz s Bilds hinzugefügt haben, nehmen Sie einen Moment Zeit, um diese Bitmaps mit JPG-Format zu ersetzen. Wenn Sie weiterhin die aktuellen Kategorie Bilder verwenden möchten, können Sie diese in JPG-Format konvertieren, durch die folgenden Schritte ausführen:

1. Speichern Sie die Bitmapbilder auf der Festplatte freigeben. Besuchen Sie die `UpdatingAndDeleting.aspx` Seite in Ihrem Browser und für den ersten acht Kategorien, mit der rechten Maustaste auf das Bild aus, und wählen Sie das Bild gespeichert.
2. Öffnen Sie das Bild im Bildeditor Ihrer Wahl ein. Sie können z. B. Microsoft Paint verwenden.
3. Speichern Sie die Bitmap als JPG-Bild ein.
4. Aktualisieren Sie das Bild Kategorie s über die Bearbeitung Benutzeroberfläche, über die JPG-Datei.

Nach dem Bearbeiten einer Kategorie und die JPG-Image hochladen, das Bild wird nicht Rendern im Browser da die `DisplayCategoryPicture.aspx` Seite ist die erste 78 Byte der Bilder der ersten acht Kategorien Striping. Beheben Sie dies können Sie, indem Sie den Code, der die OLE-Header-Striping ausführt entfernt. Nach der Durchführung dieses, den `DisplayCategoryPicture.aspx``Page_Load` -Ereignishandler sollte den folgenden Code enthält:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> Die `UpdatingAndDeleting.aspx` Seite s einfügen und Bearbeiten von Schnittstellen kann etwas Arbeit verwenden. Die `CategoryName` und `Description` BoundFields, DetailsView und GridView in von TemplateFields konvertiert werden sollen. Da `CategoryName` lässt keine `NULL` Werte, ein RequiredFieldValidator hinzugefügt werden sollen. Und die `Description` Textfeld wahrscheinlich in einem mehrzeiligen Textfeld konvertiert werden sollen. Ich lassen diese Schliff als Übung für Sie.


## <a name="summary"></a>Zusammenfassung

Dieses Lernprogramm ist unsere Betrachtung arbeiten mit Binärdaten abgeschlossen. In diesem Lernprogramm und die vorherigen drei wurde erläutert, wie Binärdaten im Dateisystem oder direkt in der Datenbank gespeichert werden können. Ein Benutzer stellt Binärdaten mit dem System durch Auswählen einer Datei von ihrer Festplatte und den Datenupload an den Webserver, wobei kann im Dateisystem gespeichert oder in die Datenbank eingefügt. ASP.NET 2.0 umfasst ein FileUpload-Steuerelement, mit der eine solche so einfach wie Drag & drop Schnittstelle bereitstellen. Allerdings als notiert wurden die [Hochladen von Dateien](uploading-files-vb.md) Lernprogramm, das FileUpload-Steuerelement ist nur eignet sich ideal für relativ kleine Dateiuploads im Idealfall eine MB nicht überschreiten. Wir untersucht auch zum Verknüpfen der hochgeladener Daten mit dem zugrunde liegenden Datenmodell als auch zum Bearbeiten und löschen die binären Daten aus vorhandenen Datensätze.

Unsere nächste Satz Lernprogramme untersucht verschiedene Zwischenspeichern Techniken. Caching bietet die Möglichkeit, eine Anwendung s verbessern gesamtleistung, dass die Ergebnisse aus teuer und speichern in einem Speicherort, der schnell zugegriffen werden kann.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](including-a-file-upload-option-when-adding-a-new-record-vb.md)
