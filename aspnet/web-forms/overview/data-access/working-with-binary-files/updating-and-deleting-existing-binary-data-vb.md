---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Aktualisieren und Löschen von vorhandenen Binärdaten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir gesehen, wie das GridView-Steuerelement sie auf einfache Weise zu bearbeiten und Löschen von Textdaten. In diesem Tutorial sehen wir, wie das GridView-Steuerelement auch machen...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd5aea4bfc1a38dc3364cdf2657d3dca2b82022c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830454"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualisieren und Löschen von vorhandenen Binärdaten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) oder [PDF-Datei herunterladen](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> In den vorherigen Tutorials haben wir gesehen, wie das GridView-Steuerelement sie auf einfache Weise zu bearbeiten und Löschen von Textdaten. In diesem Tutorial sehen wir, wie das GridView-Steuerelement dadurch können zum Bearbeiten und Löschen von Binärdaten, ob diese binäre Daten in der Datenbank gespeichert oder im Dateisystem gespeichert ist.


## <a name="introduction"></a>Einführung

Über die letzten drei Lernprogramme wir hinzugefügt haben einiges an Funktionen zum Arbeiten mit Binärdaten. Wir beginnen, durch das Hinzufügen einer `BrochurePath` Spalte der `Categories` Tabelle und die Architektur entsprechend aktualisiert. Wir auch Daten von Zugriffsschicht und Business Logic Layer Methoden für die Kategorien-s-Tabelle vorhandene hinzugefügt `Picture` Spalte, die den binäre Inhalt s der Bilddatei enthält. Wir erstellt haben, Webseiten, um den binären Daten in einer GridView-Ansicht einen Downloadlink für die Broschüre, mit der Kategorie s in präsentieren einer `<img>` Element und hinzugefügt haben, eine DetailsView, damit Benutzer eine neue Kategorie hinzufügen und seine Broschüre und Bild-Daten hochladen.

Alle diese bleibt implementiert werden kann bearbeiten und löschen vorhandene Kategorien, die wir in diesem Tutorial mithilfe von GridView s integrierte bearbeiten und Löschen von Funktionen zu erreichen, werden. Wenn Sie eine Kategorie zu bearbeiten, werden der Benutzer kann optional ein neues Bild hochladen oder verfügen über die Kategorie, die weiterhin die vorhandene Ressourcengruppe verwenden. Für die Broschüre können sie entweder die vorhandene Broschüre verwenden, um eine neue Broschüre hochzuladen oder um anzugeben, dass die Kategorie mehr als eine Broschüre zugeordnet hat. Lassen Sie s beginnen!

## <a name="step-1-updating-the-data-access-layer"></a>Schritt 1: Aktualisieren der Datenzugriffsschicht

Die DAL wurde automatisch generierter `Insert`, `Update`, und `Delete` Methoden, aber diese Methoden wurden generiert basierend auf der `CategoriesTableAdapter` s Hauptabfrage, der keine enthält die `Picture` Spalte. Aus diesem Grund die `Insert` und `Update` Methoden enthalten keine Parameter zur Angabe der binären Daten für das Category-s-Bild. Wie der [vorherigen Lernprogramm](including-a-file-upload-option-when-adding-a-new-record-vb.md), müssen wir erstellen eine neue TableAdapter-Methode zum Aktualisieren der `Categories` Tabelle, wenn Sie Binärdaten angeben.

Öffnen Sie das typisierte DataSet, und klicken Sie im Designer mit der Maustaste auf die `CategoriesTableAdapter` s-Header und Abfrage hinzufügen im Kontextmenü der Launche TableAdapter-Konfigurations-Assistenten. Der Assistent wird gestartet, indem wir gefragt, wie die TableAdapter-Abfrage für die Datenbank zugreifen, sollten. Wählen Sie die SQL-Anweisungen, und klicken Sie auf Weiter. Im nächste Schritt fordert für den Typ der Abfrage generiert werden soll. Da wir erneut erstellen eine Abfrage, um einen neuen Eintrag hinzufügen die `Categories` Tabelle, wählen Sie aktualisieren, und klicken Sie auf Weiter.


[![Wählen Sie die Updateoption](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Abbildung 1**: Wählen Sie die UPDATE-Option ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Jetzt müssen wir geben die `UPDATE` SQL-Anweisung. Vom Assistenten automatisch vorgeschlagene ein `UPDATE` Anweisung, die in der Hauptabfrage des TableAdapter s entspricht (diejenige, die updates der `CategoryName`, `Description`, und `BrochurePath` Werte). Ändern Sie die Anweisung, damit die `Picture` enthalten zusammen mit einem `@Picture` Parameter wie folgt:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Der letzten Seite des Assistenten fordert uns um die neue Methode des TableAdapter zu nennen. Geben Sie `UpdateWithPicture` , und klicken Sie auf "Fertig stellen".


[![Name der neuen UpdateWithPicture TableAdapter-Methode](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Abbildung 2**: Benennen Sie die neue Methode des TableAdapter `UpdateWithPicture` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Schritt 2: Hinzufügen, die Business Logic Layer-Methoden

Zusätzlich zum Aktualisieren der DAL, müssen Sie die BLL enthält Methoden zum Aktualisieren und löschen eine Kategorie zu aktualisieren. Dies sind die Methoden, die aufgerufen werden, auf der Darstellungsschicht.

Für das eine Kategorie löschen, können wir die `CategoriesTableAdapter` s, die automatisch generierte `Delete` Methode. Fügen Sie die folgende Methode der `CategoriesBLL` Klasse:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Erstellen Sie zwei Methoden zum Aktualisieren einer Kategorie - erwartet, dass die binäre Bilddaten und ruft für dieses Tutorial können s der `UpdateWithPicture` Methode, die wir gerade hinzugefügt, um haben die `CategoriesTableAdapter` und eine andere, die akzeptiert nur die `CategoryName`, `Description`, und `BrochurePath`zu und verwendet `CategoriesTableAdapter` s, die automatisch generierte Klasse `Update` Anweisung. Der Grund für die Verwendung der beiden Methoden ist, dass in einigen Fällen ein Benutzer die Kategorie-s-Bild zusammen mit der anderen Feldern, aktualisieren sollen, in dem Fall muss der Benutzer hat, um das neue Bild hochzuladen. Die hochgeladene Bild s binäre Daten können dann verwendet werden, der `UPDATE` Anweisung. In anderen Fällen kann der Benutzer nur aktualisieren, z. B. der Name und Beschreibung interessiert. Jedoch möglich, wenn die `UPDATE` Anweisung erwartet, dass die Binärdaten für die `Picture` Spalte, und wir d müssen auch diese Informationen bereitstellen. Dies müsste eine zusätzliche Roundtrips zur Datenbank rüstet die Bilddaten für den Datensatz, der bearbeitet wird. Aus diesem Grund sollten Sie zwei `UPDATE` Methoden. Welcher Typ verwendet, ob Grafikdaten bereitgestellt werden, bei der Aktualisierung der Kategorie werden anhand bestimmt der Geschäftslogikschicht.

Um dies zu ermöglichen, fügen Sie zwei Methoden, die `CategoriesBLL` Klasse, die beide mit dem Namen `UpdateCategory`. Erstens sollte drei akzeptieren `String` s, eine `Byte` Array, und ein `Integer` als Eingabe Parameter; der zweite nur drei `String` s und einem `Integer`. Die `String` Eingabeparameter sind für die Kategorie s-Name, Beschreibung und Broschüre Dateipfad der `Byte` Array ist, für den binären Inhalt des Bilds Kategorie s, und die `Integer` identifiziert die `CategoryID` des Datensatzes zu aktualisieren. Beachten Sie, dass die erste Überladung, der zweiten, wenn der übergegebenen aufruft `Byte` Array `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Schritt 3: Kopieren von über das Einfügen und die View-Funktionen

In der [vorherigen Lernprogramm](including-a-file-upload-option-when-adding-a-new-record-vb.md) wir erstellt haben, eine Seite namens `UploadInDetailsView.aspx` , die alle Kategorien in einer GridView-Ansicht aufgeführt, und eine DetailsView zum Hinzufügen von neuer Kategorien mit dem System bereitgestellt. In diesem Tutorial erweitern wir die GridView Einbeziehung von bearbeiten und Löschen von Unterstützung. Anstelle von arbeiten weiterhin an der `UploadInDetailsView.aspx`, Let s stattdessen platzieren Sie dieses Tutorial s Änderungen in der `UpdatingAndDeleting.aspx` Seite aus dem gleichen Ordner, `~/BinaryData`. Kopieren und Einfügen von deklarativen Markup aus, und code aus `UploadInDetailsView.aspx` zu `UpdatingAndDeleting.aspx`.

Öffnen Sie zunächst die `UploadInDetailsView.aspx` Seite. Kopieren Sie die deklarative Syntax in der `<asp:Content>` Element, wie in Abbildung 3 dargestellt. Öffnen Sie als Nächstes `UpdatingAndDeleting.aspx` , und fügen Sie dieses Markup innerhalb der `<asp:Content>` Element. Kopieren Sie den Code aus auf ähnliche Weise die `UploadInDetailsView.aspx` Seite s CodeBehind-Klasse, um `UpdatingAndDeleting.aspx`.


[![Kopieren von deklarativen Markup aus UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Abbildung 3**: Kopieren von deklarativen Markup `UploadInDetailsView.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Besuchen Sie nach dem Kopieren über deklaratives Markup und Code `UpdatingAndDeleting.aspx`. Daraufhin sollte die gleiche Ausgabe und haben die gleiche benutzerfreundlichkeit wie bei `UploadInDetailsView.aspx` Seite aus dem vorherigen Lernprogramm.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Schritt 4: Hinzufügen von Unterstützung für das ObjectDataSource-Steuerelement und GridView löschen

Wie im erläutert die [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial GridView bietet integrierte löschen und diese Funktionen können auf die Teilstriche eines Kontrollkästchens aktiviert werden, wenn die zugrunde liegende Raster s Datenquelle unterstützt wird gelöscht. Derzeit dem ObjectDataSource-Steuerelement die GridView gebunden ist (`CategoriesDataSource`) Löschen von nicht unterstützt.

Um dies zu beheben, klicken Sie auf die Option Konfigurieren von Datenquellen aus dem "ObjectDataSource"-s-Smarttag, um den Assistenten zu starten. Der erste Bildschirm zeigt, dass es sich bei dem ObjectDataSource-Steuerelement konfiguriert ist, um die Arbeit mit der `CategoriesBLL` Klasse. Drücken Sie weiter. Derzeit nur das "ObjectDataSource" s `InsertMethod` und `SelectMethod` Eigenschaften angegeben werden. Allerdings der Assistenten automatisch die Dropdown-Listen in den Update- und DELETE-Registerkarten mit den `UpdateCategory` und `DeleteCategory` Methoden bzw. Grund hierfür ist, in der `CategoriesBLL` wir diese Methoden mithilfe von markierten Klasse die `DataObjectMethodAttribute` als die standardmäßigen Methoden zum Aktualisieren und löschen.

Jetzt legen Sie die UPDATE-Registerkarte "s" Dropdown-Liste auf (keine), aber belassen Sie die DELETE Registerkarte s Dropdown-Liste festgelegt `DeleteCategory`. Wir werden zu diesem Assistenten in Schritt 6 zum Hinzufügen der Unterstützung von Update zurückzukehren.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der DeleteCategory-Methode](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `DeleteCategory` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Nach Abschluss des Assistenten, kann Visual Studio bitten Sie Sie zum Aktualisieren von Feldern und Schlüssel können, die die Daten Web neu generieren, werden Felder steuert. Wählen Sie Nein, da Sie Ja alle Anpassungen an Feldern überschrieben werden, die Sie vorgenommen haben, können.


Dem ObjectDataSource-Steuerelement enthält jetzt einen Wert für die `DeleteMethod` Eigenschaft als auch ein `DeleteParameter`. Denken Sie daran, dass bei Verwendung des Assistenten, um die Methoden angeben, Visual Studio das "ObjectDataSource"-s legt `OldValuesParameterFormatString` Eigenschaft `original_{0}`, die bewirkt, dass Probleme mit dem Update und löschen Sie die Methodenaufrufe. Aus diesem Grund entweder vollständig zu löschen, diese Eigenschaft, oder es auf den Standardwert zurücksetzen `{0}`. Wenn Sie Ihren Speicher für diese Eigenschaft von "ObjectDataSource" aktualisieren möchten, finden Sie unter den [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial.

Nach dem Abschließen des Assistenten, und Beheben der `OldValuesParameterFormatString`, sollte "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Fügen Sie nach der Konfiguration dem ObjectDataSource-Steuerelement, das Löschen von Funktionen an die GridView durch Aktivieren des Kontrollkästchens löschen aktivieren, aus dem GridView-s-Smarttag. Dadurch wird eine CommandField hinzugefügt, an die GridView, deren `ShowDeleteButton` -Eigenschaftensatz auf `True`.


[![Aktivieren der Unterstützung für das Löschen in den GridView-Ansicht](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Abbildung 5**: Aktivieren der Unterstützung für das Löschen in den GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Nehmen Sie einen Moment Zeit, um die Löschfunktion zu testen. Es gibt Fremdschlüssel zwischen den `Products` Tabelle s `CategoryID` und `Categories` Tabelle s `CategoryID`, sodass Sie eine foreign Key-Einschränkung Verstoß Ausnahme erhalten, wenn Sie versuchen, den ersten acht Kategorien löschen. Um diese Funktion heraus zu testen, fügen Sie eine neue Kategorie, die sowohl einen Broschüre sowie ein Bild hinzu. Meine Testkategorie, dargestellt in Abbildung 6 enthält eine Datei mit dem Namen Broschüre `Test.pdf` und ein Testbild. Abbildung 7 zeigt die GridView, nachdem die Testkategorie hinzugefügt wurde.


[![Fügen Sie eine Testkategorie mit einem Broschüre und ein Bild hinzu](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen einer Testkategorie mit einem Broschüre und Image ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Nach dem Einfügen der Testkategorie, wird es in den GridView-Ansicht angezeigt](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Abbildung 7**: nach dem Einfügen der Testkategorie, wird es in den GridView-Ansicht angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


Aktualisieren Sie in Visual Studio im Projektmappen-Explorer aus. Daraufhin sollte eine neue Datei in die `~/Brochures` Ordner `Test.pdf` (siehe Abbildung 8).

Klicken Sie anschließend auf den Link "löschen" in der Testkategorie-Zeile, wodurch der postback und die `CategoriesBLL` Klasse s `DeleteCategory` Methode ausgelöst werden. Dadurch wird die DAL s aufgerufen `Delete` -Methode, verursacht die entsprechende `DELETE` Anweisung an die Datenbank gesendet werden. Die Daten werden dann erneut an die GridView gebunden, und das Markup wird gesendet, an den Client mit der Testkategorie nicht mehr vorhanden.

Während der Delete-Workflow erfolgreich den Testkategorie Datensatz entfernt die `Categories` Tabelle die Broschüre-Datei nicht aus dem Dateisystem der Web-s-Server entfernt. Aktualisieren Sie im Projektmappen-Explorer, und Sie sehen, dass `Test.pdf` sitzt immer noch in der `~/Brochures` Ordner.


![Die Test.pdf-Datei wurde nicht aus dem Web Server s-Dateisystem gelöscht.](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Abbildung 8**: die `Test.pdf` Datei wurde aus dem Dateisystem der Web-s-Server nicht gelöscht.


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Schritt 5: Entfernen der gelöschten Kategorie-s-Broschüre-Datei

Einer der Nachteile zum Speichern von Binärdaten, die außerhalb der Datenbank ist, dass zusätzliche Schritte ausgeführt werden müssen, um diese Dateien bereinigt, wenn die zugeordnete Datenbank-Datensatz gelöscht wird. GridView und "ObjectDataSource" Geben Sie die Ereignisse, die ausgelöst werden, bevor und nachdem der Delete-Befehl ausgeführt wurde. Wir müssen tatsächlich Erstellen von Ereignishandlern für die Ereignisse vor und nach Abschluss der Aktion. Bevor Sie die `Categories` Datensatz gelöscht müssen wir die PDF-Datei s-Pfad zu bestimmen, aber wir Raten, die PDF-Datei zu löschen, bevor die Kategorie gelöscht wird, falls einige Ausnahme vorhanden ist und die Kategorie ist nicht gelöscht werden soll, t.

Das GridView-s [ `RowDeleting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) wird ausgelöst, bevor der "ObjectDataSource"-s-Delete-Befehl aufgerufen wurde, während er sich seine [ `RowDeleted` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) wird ausgelöst, nachdem. Erstellen von Ereignishandlern für diese beiden Ereignisse, die mit dem folgenden Code:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

In der `RowDeleting` -Ereignishandler der `CategoryID` der Zeile gelöscht wird stammt aus der GridView-s `DataKeys` -Auflistung, die in diesem Ereignishandler über zugegriffen werden kann die `e.Keys` Auflistung. Als Nächstes die `CategoriesBLL` Klasse s `GetCategoryByCategoryID(categoryID)` wird aufgerufen, um das Zurückgeben von Informationen über den Datensatz gelöscht wird. Wenn das zurückgegebene `CategoriesDataRow` Objekt verfügt über einen nicht-`NULL``BrochurePath` Wert, und klicken Sie dann in die Seitenvariable gespeichert ist `deletedCategorysPdfPath` , damit die Datei gelöscht werden kann, in der `RowDeleted` -Ereignishandler.

> [!NOTE]
> Anstatt Abrufen der `BrochurePath` details für die `Categories` aufzeichnen gelöscht wird, der `RowDeleting` -Ereignishandler wir Alternativ hätten die `BrochurePath` der GridView-s `DataKeyNames` Eigenschaft und Zugriff auf den Datensatz s-Wert durch die `e.Keys` Auflistung. Auf diese Weise würde etwas steigern die GridView s Ansichtstatusgröße, aber würde zu der Menge an erforderlichem Code und eine Reise in der Datenbank speichern.


Nach der "ObjectDataSource" s zugrunde liegenden Delete-Befehl aufgerufen wurde, das GridView-s `RowDeleted` -Ereignis Ereignishandler ausgelöst wird. Wenn es keine Ausnahmen gab in die Daten löschen und es ein Wert für gibt `deletedCategorysPdfPath`, und klicken Sie dann die PDF-Datei aus dem Dateisystem gelöscht wird. Beachten Sie, dass diese zusätzliche Code nicht erforderlich ist, um die binären Daten s Kategorie zugeordnet sein Bild zu bereinigen. S, da die Bilddaten direkt in der Datenbank gespeichert werden, also das Löschen der `Categories` Zeilen auch dieses Bild Kategoriedaten s gelöscht.

Wenn die beiden Ereignishandler hinzugefügt haben, führen Sie diesen Testfall erneut aus. Wenn Sie die Kategorie löschen, wird die zugeordnete PDF-Datei ebenfalls gelöscht.

Aktualisiert einen vorhandenen Datensatz verknüpften s Binärdaten enthält einige interessanten Herausforderungen. Die restlichen Verlauf dieses Lernprogramms befasst sich mit der Broschüre und Bild Aktualisierungsfunktionen hinzugefügt. Schritt 6 werden die Verfahren zum Aktualisieren der Broschüre Informationen, während Schritt 7 sucht, auf das Bild wird aktualisiert.

## <a name="step-6-updating-a-category-s-brochure"></a>Schritt 6: Aktualisieren einer Kategorie-s-Broschüre

Wie in diesem Tutorial eine Übersicht der einfügen, aktualisieren und Löschen von Daten erläutert, bietet die GridView integrierte auf Zeilenebene Unterstützung für die Bearbeitung, der von der Takt eines Kontrollkästchens implementiert werden kann, wenn die zugrunde liegenden Datenquelle ordnungsgemäß konfiguriert ist. Derzeit den `CategoriesDataSource` ObjectDataSource-Steuerelement ist noch nicht enthält, wird die Unterstützung, aktualisiert, daher können s hinzu, die in konfiguriert.

Klicken Sie auf die Datenquelle konfigurieren-Link aus dem "ObjectDataSource"-s-Assistenten aus, und fahren Sie mit dem zweiten Schritt fort. Aufgrund der der `DataObjectMethodAttribute` verwendet `CategoriesBLL`, die UPDATE-Dropdown-Liste sollten automatisch aufgefüllt werden, mit der `UpdateCategory` Überladung verwenden, akzeptiert vier Eingabeparameter (für alle Spalten jedoch `Picture`). Ändern Sie dies, dass die Überladung mit fünf Parametern verwendet.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der UpdateCategory-Methode, die einen Parameter für Bild enthält](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Abbildung 9**: Konfigurieren Sie das "ObjectDataSource" Verwenden der `UpdateCategory` -Methode, die einen Parameter für enthält `Picture` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


Dem ObjectDataSource-Steuerelement enthält jetzt einen Wert für die `UpdateMethod` Eigenschaft sowie entsprechende `UpdateParameter` s. Wie in Schritt 4 erwähnt, handelt es sich bei Visual Studio legt das "ObjectDataSource"-s `OldValuesParameterFormatString` Eigenschaft `original_{0}` bei Verwendung des Assistenten für die Datenquelle konfigurieren. Dazu führen, dass Probleme mit dem Aktualisieren wird und Löschen von Methodenaufrufen. Aus diesem Grund entweder vollständig zu löschen, diese Eigenschaft, oder es auf den Standardwert zurücksetzen `{0}`.

Nach dem Abschließen des Assistenten, und Beheben der `OldValuesParameterFormatString`, "ObjectDataSource" s deklarative Markup sollte wie folgt aussehen:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Überprüfen Sie die Option "Bearbeiten aktivieren" das GridView-s-Smarttag, um die integrierten Bearbeitungsfunktionen für s GridView zu aktivieren. Dadurch wird festgelegt, die CommandField s `ShowEditButton` Eigenschaft `True`, sodass das Hinzufügen einer Schaltfläche "Bearbeiten" (und die Schaltflächen "Update" und "Abbrechen" für die Zeile, die bearbeitet wird).


[![Konfigurieren der GridView, die Bearbeitung von Support](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Abbildung 10**: konfigurieren die GridView, die Bearbeitung von Support ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Besuchen Sie die Seite über einen Browser, und klicken Sie auf eines der Zeile s Bearbeitungsschaltflächen. Die `CategoryName` und `Description` BoundFields als Textfelder gerendert werden. Die `BrochurePath` TemplateField verfügt nicht über eine `EditItemTemplate`, sodass sie weiterhin anzeigen der `ItemTemplate` einen Link zu der Broschüre. Die `Picture` ImageField als Textfeld rendert, deren `Text` Eigenschaft erhält den Wert der ImageField Zuordnungsvorgänge `DataImageUrlField` Wert in diesem Fall `CategoryID`.


[![Das GridView verfügt nicht über eine Bearbeiten-Schnittstelle für BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Abbildung 11**: GridView verfügt nicht über eine Bearbeiten-Schnittstelle für `BrochurePath` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Anpassen der`BrochurePath`bearbeiten s-Schnittstelle

Wir müssen eine bearbeitende-Schnittstelle zum Erstellen der `BrochurePath` TemplateField, eine, die dem Benutzer, entweder ermöglicht:

- Lassen Sie die Kategorie s Broschüre als-ist,
- Aktualisieren Sie die Kategorie s Broschüre durch Hochladen einer neuen Broschüre, oder
- Entfernen der Broschüre Kategorie s (in dem Fall, dass die Kategorie mehr als eine zugeordnete Broschüre hat).

Wir müssen auch zum Aktualisieren der `Picture` Bearbeitung ImageField s-Schnittstelle, aber wir erhalten, dies in Schritt 7.

GridView s Smarttags, klicken Sie auf den Link für die Vorlagen bearbeiten, und wählen die `BrochurePath` TemplateField s `EditItemTemplate` aus der Dropdown-Liste. Fügen Sie ein RadioButtonList-Steuerelement auf dieser Vorlage Festlegen der `ID` Eigenschaft `BrochureOptions` und die zugehörige `AutoPostBack` Eigenschaft, um `True`. Klicken Sie im Eigenschaftenfenster auf die Auslassungszeichen in der `Items` -Eigenschaft, dadurch wird die `ListItem` Auflistungs-Editor. Fügen Sie die folgenden drei Optionen mit `Value` s, 1, 2 und 3, bzw.:

- Verwenden Sie die aktuellen Broschüre
- Entfernen Sie die aktuellen Broschüre
- Neue Broschüre hochladen

Festlegen der ersten `ListItem` s `Selected` Eigenschaft `True`.


![Drei ListItems RadioButtonList hinzufügen](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Abbildung 12**: Fügen Sie drei `ListItem` s, um die RadioButtonList


Fügen Sie unterhalb der RadioButtonList eine "FileUpload"-Steuerelement mit dem Namen `BrochureUpload`. Legen Sie dessen `Visible` Eigenschaft `False`.


[![Fügen Sie ein RadioButtonList und ein "FileUpload"-Steuerelement auf das EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Abbildung 13**: Hinzufügen eines RadioButtonList und "FileUpload"-Steuerelement auf die `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Diese RadioButtonList bietet drei Optionen für den Benutzer. Die Idee ist, dass der FileUpload-Serversteuerelements wird nur angezeigt, wenn die letzte Option, neue Broschüre hochladen, ausgewählt ist. Um dies zu erreichen, erstellen Sie einen Ereignishandler für die s RadioButtonList `SelectedIndexChanged` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Da die RadioButtonList "und" FileUpload-Steuerelemente in einer Vorlage sind, müssen wir ein paar Codezeilen zum programmgesteuerten Festlegen dieser Steuerelemente zu schreiben. Die `SelectedIndexChanged` -Ereignishandler wird einen Verweis vom RadioButtonList in übergeben die `sender` input-Parameters. Um das Steuerelement "FileUpload" zu erhalten, müssen wir zum Abrufen von übergeordneten s RadioButtonList-Steuerelement und Verwenden der `FindControl("controlID")` Methode von dort aus. Nachdem wir einen Verweis auf die RadioButtonList und die "FileUpload"-Steuerelement haben, Steuern der "FileUpload" s `Visible` -Eigenschaftensatz auf `True` nur, wenn die RadioButtonList-s `SelectedValue` gleich 3, d.h. die `Value` für die neue Broschüre hochladen `ListItem`.

Nehmen Sie mit diesem Code werden einen Moment Zeit, die Bearbeitungsschnittstelle zu testen. Klicken Sie auf die Schaltfläche "Bearbeiten" für eine Zeile. Zunächst muss die Option aktuelle Broschüre verwenden ausgewählt werden. Ändern den ausgewählten Index auslöst ein Postback. Wenn die dritte Option ausgewählt ist, die "FileUpload"-Steuerelements angezeigt wird, andernfalls wird es ausgeblendet ist. Abbildung 14 zeigt die Bearbeitungsschnittstelle auf, wenn auf die Schaltfläche "Bearbeiten" zunächst geklickt wird; Abbildung 15 zeigt die Benutzeroberfläche auf, nachdem der Upload neue Broschüre-Option ausgewählt ist.


[![Zunächst verwenden aktuelle Broschüre, die Option aktiviert ist](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Abbildung 14**: zunächst mit aktuellen Broschüre ausgewählt ist ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Auswählen der neuen Upload-Broschüre Option zeigt die FileUpload-Serversteuerelements](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Abbildung 15**: Auswählen der neuen Upload-Broschüre Option zeigt das Steuerelement "FileUpload" ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Speichern der Broschüre-Datei, und Aktualisieren der`BrochurePath`Spalte

Wenn der GridView-s-Aktualisieren-Schaltfläche geklickt wird, dessen `RowUpdating` -Ereignis ausgelöst wird. Das "ObjectDataSource" s-Befehl "Update" aufgerufen wird, und klicken Sie dann das GridView-s `RowUpdated` -Ereignis ausgelöst wird. Mit dem Löschen von Workflow, wir müssen Sie Ereignishandler für diese beiden Ereignisse erstellen. In der `RowUpdating` -Ereignishandler müssen wir bestimmen, welche Aktion anhand der `SelectedValue` von der `BrochureOptions` RadioButtonList:

- Wenn die `SelectedValue` ist 1, wir gleich weiter nutzen möchten `BrochurePath` festlegen. Aus diesem Grund müssen wir die "ObjectDataSource"-s festgelegt `brochurePath` -Parameter dem vorhandenen `BrochurePath` Wert des Datensatzes aktualisiert wird. Das "ObjectDataSource"-s `brochurePath` Parameter kann festgelegt werden, mithilfe von `e.NewValues["brochurePath"] = value`.
- Wenn die `SelectedValue` 2 ist, dann möchten wird für den Datensatz s `BrochurePath` Wert `NULL`. Dies kann erreicht werden, durch Festlegen der "ObjectDataSource"-s `brochurePath` Parameter `Nothing`, was dazu führt, in einer Datenbank `NULL` verwendet wird, der `UPDATE` Anweisung. Ist eine vorhandene Broschüre-Datei, die entfernt wird, müssen wir die vorhandene Datei zu löschen. Allerdings möchten wir nur dies tun, wenn das Update abgeschlossen ist, ohne eine Ausnahme auszulösen.
- Wenn die `SelectedValue` ist 3, wir möchten sicherstellen, dass der Benutzer eine PDF-Datei hochgeladen wurde und speichern Sie ihn in das Dateisystem, und aktualisieren Sie den Eintrag s `BrochurePath` Spaltenwert. Ist eine vorhandene Broschüre-Datei, die ersetzt wird, müssen wir darüber hinaus die vorherige Datei zu löschen. Allerdings möchten wir nur dies tun, wenn das Update abgeschlossen ist, ohne eine Ausnahme auszulösen.

Die erforderlichen Schritte zum Abschluss der RadioButtonList s `SelectedValue` ist 3 sind nahezu identisch, verwendet werden, durch die DetailsView s `ItemInserting` -Ereignishandler. Dieser Ereignishandler wird ausgeführt, wenn ein neuer Kategoriedatensatz von DetailsView-Steuerelement hinzugefügt wird, wir, in hinzugefügt, der [vorherigen Tutorial](including-a-file-upload-option-when-adding-a-new-record-vb.md). Aus diesem Grund modelliere es uns, diese Funktion, in separate Methoden umzugestalten. Insbesondere verschoben ich die allgemeine Funktionalität in zwei Methoden:

- `ProcessBrochureUpload(FileUpload, out bool)` als Eingabe akzeptiert, eine Steuerelementinstanz "FileUpload" und eine Ausgabe boolescher Wert, der angibt, ob der Vorgang zum Löschen oder bearbeiten fortgesetzt werden soll, oder wenn es aufgrund eines Fehlers Validierung abgebrochen werden soll. Diese Methode gibt den Pfad zur gespeicherten Datei oder `null` Wenn keine Datei gespeichert wurde.
- `DeleteRememberedBrochurePath` Löscht die Datei, angegeben durch den Pfad in die Seitenvariable `deletedCategorysPdfPath` Wenn `deletedCategorysPdfPath` nicht `null`.

Der Code für diese beiden Methoden folgt. Beachten Sie die Ähnlichkeit zwischen `ProcessBrochureUpload` und der s DetailsView `ItemInserting` -Ereignishandler aus dem vorherigen Lernprogramm. In diesem Tutorial wurden ich die DetailsView-s-Ereignishandler zur Verwendung dieser neuen Methoden aktualisiert. Laden Sie den Code für dieses Lernprogramm für die Änderungen an die DetailsView-s-Ereignishandler finden Sie unter.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Das GridView-s `RowUpdating` und `RowUpdated` Ereignishandler verwenden, die `ProcessBrochureUpload` und `DeleteRememberedBrochurePath` Methoden aus, wie im folgenden Code gezeigt:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Hinweis wie die `RowUpdating` -Ereignishandler verwendet eine Reihe von bedingten Anweisungen zum Ausführen der entsprechenden Aktion, die auf der Grundlage der `BrochureOptions` RadioButtonList s `SelectedValue` -Eigenschaftswert.

Mit diesem Code werden können Sie bearbeiten eine Kategorie und haben sie die aktuellen Broschüre verwenden, verwenden Sie keine Broschüre oder ein neues hochzuladen. Fahren Sie fort, und probieren Sie es aus. Legen Sie Haltepunkte in der `RowUpdating` und `RowUpdated` Ereignishandler um gewisser des Workflows zu erhalten.

## <a name="step-7-uploading-a-new-picture"></a>Schritt 7: Hochladen eines neuen Bilds

Die `Picture` ImageField s Schnittstelle rendert als Textfeld mit dem Wert von aufgefüllt Bearbeiten der `DataImageUrlField` Eigenschaft. Während der Bearbeitung Workflow, die GridView übergibt einen Parameter dem ObjectDataSource-Steuerelement mit dem Parameternamen für s den Wert der ImageField Zuordnungsvorgänge `DataImageUrlField` -Eigenschaft und der s-Parameter den Wert in das Textfeld auf die Bearbeitungsschnittstelle eingegeben. Dieses Verhalten ist geeignet, wenn das Bild als Datei im Dateisystem gespeichert wird und die `DataImageUrlField` enthält die vollständige URL des Bilds. Mit solchen Situationen ist zeigt die Bearbeitungsschnittstelle die Image-s-URL in das Textfeld ein, die der Benutzer ändern können und die in der Datenbank gespeichert haben. Gewährt, diese Standard-Schnittstelle t ermöglicht dem Benutzer ein neues Image hochladen, aber es ermöglicht ihnen die URL des Bilds in einen anderen als den aktuellen Wert zu ändern. Für dieses Lernprogramm jedoch der ImageField-s-Standard bearbeiten-Schnittstelle reicht nicht aus da die `Picture` binäre Daten direkt in der Datenbank gespeichert werden und die `DataImageUrlField` Eigenschaft enthält nur die `CategoryID`.

Um besser zu verstehen, was in unserem Lernprogramm geschieht, wenn ein Benutzer eine Zeile mit einem ImageField bearbeitet, berücksichtigen Sie das folgende Beispiel: ein Benutzer eine Zeile mit bearbeitet `CategoryID` 10, wodurch die `Picture` ImageField als ein Textfeld mit dem Wert 10 gerendert. Stellen Sie sich, dass der Benutzer klickt auf die Schaltfläche "Aktualisieren" ändert den Wert in dieses Textfeld in 50. Ein Postback auftritt und die GridView erstellt zunächst einen Parameter namens `CategoryID` mit dem Wert 50. Allerdings bevor die GridView dieser Parameter gesendet (und die `CategoryName` und `Description` Parameter), es fügt die Werte aus der `DataKeys` Auflistung. Aus diesem Grund überschreibt es die `CategoryID` Parameter mit der aktuellen Zeile s zugrunde liegende `CategoryID` Wert 10. Kurz gesagt, die bearbeiten-Schnittstelle ImageField s hat keine Auswirkungen auf den Workflow bearbeiten für dieses Tutorial da Namen von ImageField s `DataImageUrlField` -Eigenschaft und das Raster s `DataKey` Wert sind identisch.

Während der ImageField zum Anzeigen eines Bilds, das basierend auf Daten erleichtert, Einbau Sie zusätzlichen wir t ein Textfeld in die Bearbeitungsschnittstelle bereitstellen möchten. Stattdessen möchten wir ein "FileUpload"-Steuerelement zu bieten, mit denen der Endbenutzer das Category-s-Bild zu ändern. Im Gegensatz zu den `BrochurePath` Wert für diese Tutorials wir haben entschieden, um festzulegen, dass jede Kategorie eine Vorstellung haben, muss. Aus diesem Grund, wir Raten t erforderlich, damit der Benutzer, die darauf hinweisen, dass keine zugeordnete Bild der Benutzer kann, laden Sie entweder ein neues Bild aus, oder übernehmen Sie das aktuelle Bild als-ist.

Um die Bearbeitung ImageField-s-Oberfläche anpassen zu können, müssen wir ihn in ein TemplateField konvertieren. Klicken Sie von GridView s Smarttags auf den Link für die Spalten bearbeiten, wählen Sie die ImageField, und klicken Sie auf das konvertierte dieses Feld in ein TemplateField-Link.


![Die ImageField in ein TemplateField konvertieren](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Abbildung 16**: die ImageField in ein TemplateField konvertieren


Konvertieren die ImageField in ein TemplateField auf diese Weise wird ein TemplateField mit zwei Vorlagen generiert. Wie die folgende deklarative Syntax dargestellt, die `ItemTemplate` enthält ein Image-Web-Steuerelement, dessen `ImageUrl` -Eigenschaft zugewiesen wird, mithilfe der Datenbindungssyntax, die basierend auf den ImageField s `DataImageUrlField` und `DataImageUrlFormatString` Eigenschaften. Die `EditItemTemplate` enthält ein Textfeld, dessen `Text` Eigenschaft gebunden ist, auf dem angegebenen Wert der `DataImageUrlField` Eigenschaft.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Wir müssen beim Aktualisieren der `EditItemTemplate` ein "FileUpload"-Steuerelement verwendet. Verknüpfen von GridView s Smarttag klicken Sie auf die Vorlagen bearbeiten, und wählen Sie dann die `Picture` TemplateField s `EditItemTemplate` aus der Dropdown-Liste. In der Vorlage sehen Sie ein Textfeld, dies zu entfernen. Als Nächstes ein "FileUpload"-Steuerelement aus der Toolbox ziehen, in die Vorlage, die Einstellung der `ID` zu `PictureUpload`. Fügen Sie auch den Text, der die Kategorie s Bild ändern, geben Sie ein neues Bild hinzu. Um dem Bild Kategorie s gleich zu halten, lassen Sie das Feld für die Vorlage auch leer.


[![Hinzufügen einer FileUpload-Serversteuerelements, das EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Abbildung 17**: Fügen Sie ein "FileUpload"-Steuerelement auf die `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Nachdem die Bearbeitungsschnittstelle angepasst haben, werden zeigen Sie Ihre Fortschritte in einem Browser an. Wenn eine Zeile im schreibgeschützten Modus anzeigen möchten, wird die Kategorie-s-Bild angezeigt, wie vor dem, aber durch Klicken auf die Schaltfläche "Bearbeiten" die Bild-Spalte als Text mit einem "FileUpload"-Steuerelement gerendert wird.


[![Die Bearbeitungsschnittstelle enthält ein "FileUpload"-Steuerelement](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Abbildung 18**: die bearbeiten-Schnittstelle enthält ein "FileUpload"-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Denken Sie daran, dass dem ObjectDataSource-Steuerelement, zum Aufrufen konfiguriert ist der `CategoriesBLL` Klasse s `UpdateCategory` Methode, die als Eingabe die Binärdaten für das Bild als akzeptiert eine `Byte` Array. Wenn dieses Array ist `Nothing`jedoch die alternative `UpdateCategory` Überladung aufgerufen wird, welche Probleme die `UPDATE` SQL-Anweisung, die nicht ändert die `Picture` Spalte gewährleistet so, dass die Kategorie s aktuelle Bild bleiben erhalten. Aus diesem Grund in den GridView-s `RowUpdating` Ereignishandler müssen wir verweisen auf die `PictureUpload` "FileUpload" steuern und zu bestimmen, ob eine Datei hochgeladen wurde. Wenn eine wurde nicht hochgeladen, dann wir machen *nicht* möchten, geben Sie einen Wert für die `picture` Parameter. Andererseits, wenn eine Datei hochgeladen wurde, in der `PictureUpload` FileUpload-Serversteuerelements, wir möchten sicherstellen, dass es sich um JPG-Datei handelt. Wenn es ist, senden wir die binären Inhalt, zu dem ObjectDataSource-Steuerelement über die `picture` Parameter.

Wie durch den Code, der in Schritt 6 verwendet wird, Großteil des Codes erforderlich sind hier bereits in der DetailsView s vorhanden ist `ItemInserting` -Ereignishandler. Aus diesem Grund ich Ve Umgestaltung in eine neue Methode, die allgemeine Funktionalität `ValidPictureUpload`, und aktualisiert die `ItemInserting` -Ereignishandler zum Verwenden dieser Methode.

Fügen Sie den folgenden Code am Anfang der GridView-s `RowUpdating` -Ereignishandler. Sie möchten, dass s wichtig, dass dieser Code vor dem Code, die die Broschüre-Datei gespeichert werden stammen, da wir Raten von t die Broschüre Dateisystem der Web-s-Server speichern, wenn eine ungültige Bilddatei hochgeladen wird.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

Die `ValidPictureUpload(FileUpload)` Methode in einem "FileUpload"-Steuerelement als einzige Eingabeparameter akzeptiert und die Erweiterung mit der hochgeladenen Datei s, um sicherzustellen, dass die hochgeladene Datei eine JPG überprüft; es wird nur aufgerufen, wenn eine Bilddatei hochgeladen wird. Wenn keine Datei hochgeladen ist, und klicken Sie dann die Bild-Parameter ist nicht festgelegt und verwendet daher den Standardwert `Nothing`. Wenn ein Bild hochgeladen wurde und `ValidPictureUpload` gibt `True`, `picture` Parameter ist, wenn die Methode zurückgibt, die Binärdaten des hochgeladenen Bilds; zugewiesen `False`, der Workflow wird abgebrochen, und der Ereignishandler beendet wurde.

Die `ValidPictureUpload(FileUpload)` Methodencode, mit dem aus den s DetailsView umgestaltet wurde `ItemInserting` Ereignishandler, folgt:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Schritt 8: Ersetzen Sie die ursprünglichen Kategorien Bilder durch JPGs

Denken Sie daran, dass acht Kategorien Originalbilder Bitmapdateien, die in einer OLE-Header eingeschlossen sind. Nun, da wir die Möglichkeit, bearbeiten Sie ein vorhandenes Bild für den Datensatz s hinzugefügt haben, können Sie diese Bitmaps mit JPG-Format zu ersetzen. Wenn Sie weiterhin die aktuellen Kategorie Bilder verwenden möchten, können Sie diese in JPG-Format konvertieren, durch die folgenden Schritte ausführen:

1. Speichern Sie die Bitmapbilder auf der Festplatte. Besuchen Sie die `UpdatingAndDeleting.aspx` Seite in Ihrem Browser, und für den ersten acht Kategorien, mit der rechten Maustaste auf das Bild, und wählen Sie auf das Bild zu speichern.
2. Öffnen Sie das Image in Ihre Grafik-Editor Ihrer Wahl ein. Sie können z. B. Microsoft Paint, verwenden.
3. Speichern Sie die Bitmap als JPG-Bild an.
4. Aktualisieren Sie das Bild Kategorie s über die bearbeiten-Schnittstelle, die JPG-Datei verwenden.

Nach dem Bearbeiten einer Kategorie und die JPG-Bild hochgeladen werden, das Bild wird nicht gerendert werden, im Browser da die `DisplayCategoryPicture.aspx` Seite ist die erste 78 Byte der Bilder der ersten acht Kategorien zu beschränken. Korrigieren Sie dies durch das Entfernen des Codes, der OLE-Header-stripping durchführt. Nach dem, auf diese Weise die `DisplayCategoryPicture.aspx``Page_Load` Ereignishandler müssen nur den folgenden Code:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> Die `UpdatingAndDeleting.aspx` Seite s einfügen und Bearbeiten von Schnittstellen können einen etwas höheren Arbeitsaufwand. Die `CategoryName` und `Description` BoundFields im GridView und DetailsView in von TemplateFields konvertiert werden sollen. Da `CategoryName` lässt keine `NULL` Werte, ein RequiredFieldValidator hinzugefügt werden sollen. Und die `Description` Textfeld wahrscheinlich in einem mehrzeiligen Textfeld konvertiert werden sollen. Ich lassen Sie diese Vollendung als Übung für Sie.


## <a name="summary"></a>Zusammenfassung

Dieses Tutorial ist abgeschlossen, unser Blick auf die Arbeiten mit Binärdaten. In diesem Tutorial und die vorherigen drei erläutert, wie die binäre Daten können im Dateisystem oder direkt in der Datenbank gespeichert werden. Ein Benutzer stellt binäre Daten an das System durch Auswählen einer Datei von ihrer Festplatte, und klicken Sie auf dem Webserver, wo er im Dateisystem gespeichert oder eingefügt werden kann in der Datenbank hochzuladen. ASP.NET 2.0 umfasst eine FileUpload-Serversteuerelements, mit der Bereitstellung einer Schnittstelle nicht so einfach wie Drag & drop. Wie bereits erwähnt, in der [Hochladen von Dateien](uploading-files-vb.md) Tutorial, das "FileUpload"-Steuerelement ist nur eignet sich ideal für relativ kleine Datei hochladen, die im Idealfall ein Megabyte nicht überschreiten. Außerdem haben wir untersucht, wie das zugrunde liegende Datenmodell hochgeladenen Daten zugeordnet und wie bearbeiten und löschen die binären Daten aus vorhandenen Datensätze.

Unsere nächste Reihe von Lernprogrammen werden verschiedene Zwischenspeichern Techniken untersucht. Caching bietet die Möglichkeit, eine Anwendung s verbessern gesamtleistung, indem die Ergebnisse von aufwändigen Vorgänge und deren Speicherung in einem Speicherort, der schneller zugegriffen werden kann.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](including-a-file-upload-option-when-adding-a-new-record-vb.md)
