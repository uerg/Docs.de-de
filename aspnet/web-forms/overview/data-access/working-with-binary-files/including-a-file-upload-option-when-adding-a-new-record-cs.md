---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Option z. B. eine Datei hochladen, wenn Sie einen neuen Datensatz (c#) hinzufügen | Microsoft Docs
author: rick-anderson
description: Dieses Lernprogramm zeigt, wie eine Weboberfläche erstellen, die können den Benutzer sowohl geben Text-Daten und Hochladen von Binärdateien. Um die verfügbaren Optionen t veranschaulichen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: ab63f9b0c536be81fdaa894985bae6a6b9346fa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Z. B. eine Datei hochladen-Option beim Hinzufügen eines neuen Datensatzes (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) oder [PDF herunterladen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Dieses Lernprogramm zeigt, wie eine Weboberfläche erstellen, die können den Benutzer sowohl geben Text-Daten und Hochladen von Binärdateien. Um die verfügbaren Optionen zum Speichern von binären Daten zu veranschaulichen, wird eine Datei in der Datenbank gespeichert werden, während die andere im Dateisystem gespeichert ist.


## <a name="introduction"></a>Einführung

In den vorherigen zwei Lernprogrammen untersucht wir Techniken zum Speichern von Binärdaten, die Datenmodell für die Anwendung s erläutert, wie das FileUpload-Steuerelement zum Senden von Dateien vom Client an den Webserver und wurde erläutert, wie diese binären Daten in einer W vorhanden verwenden zugeordnet ist "EB"-Steuerelement. Wir haben noch zu erörtern, wie das Datenmodell jedoch hochgeladene Daten zugeordnet.

In dieses Lernprogramms erstellen wir eine Webseite, um eine neue Kategorie hinzuzufügen. Auf dieser Seite müssen zusätzlich zu den Textfelder für die s-Kategorienamen und eine Beschreibung ein zwei FileUpload-Steuerelemente für die neue Kategorie s Grafik und eine für die Broschüren enthalten. Das hochgeladene Bild direkt in den neuen Eintrag s gespeichert `Picture` Spalte während der Broschüren gespeichert werden die `~/Brochures` Ordner mit dem Pfad der Datei, die im neuen Datensatz s gespeichert `BrochurePath` Spalte.

Vor dem Erstellen dieser neuen Webseite, müssen wir die Architektur zu aktualisieren. Die `CategoriesTableAdapter` Hauptabfrage s ist, kann nicht abgerufen werden. die `Picture` Spalte. Folglich automatisch generierte `Insert` Methode ist nur für Eingaben die `CategoryName`, `Description`, und `BrochurePath` Felder. Daher müssen wir eine zusätzliche Anmeldemethode im TableAdapter erstellen, die für alle vier fordert `Categories` Felder. Die `CategoriesBLL` Klasse in der Business Logic Layer wird ebenfalls aktualisiert werden müssen.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Schritt 1: Hinzufügen einer`InsertWithPicture`Methode, um die`CategoriesTableAdapter`

Wir der Erstellung der `CategoriesTableAdapter` zurück in die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm wir konfiguriert er zum automatischen Generieren von `INSERT`, `UPDATE`, und `DELETE` Anweisungen anhand der Hauptabfrage. Darüber hinaus wir der TableAdapter den direkten DB Ansatz einsetzen, die die Methoden erstellt angewiesen `Insert`, `Update`, und `Delete`. Führen Sie diese Methoden automatisch generierte `INSERT`, `UPDATE`, und `DELETE` Anweisungen und folglich annehmen von Eingabeparametern, die basierend auf den Spalten, die von der Hauptabfrage zurückgegeben. In der [Hochladen von Dateien](uploading-files-cs.md) Lernprogramm augmentiert, wir dass die `CategoriesTableAdapter` s hauptspeicherpool für Abfragen verwenden die `BrochurePath` Spalte.

Da die `CategoriesTableAdapter` s Hauptabfrage verweist nicht auf die `Picture` Spalte, wir können weder fügen Sie einen neuen Datensatz hinzu noch aktualisieren ein vorhandenes Datensatzes mit einem Wert für die `Picture` Spalte. Um diese Informationen aufzuzeichnen, können wir entweder eine neue Methode erstellen, in dem TableAdapter, die speziell für das Einfügen eines Datensatzes mit Binärdaten verwendet wird, oder wir können anpassen, die automatisch generierte `INSERT` Anweisung. Das Problem bei der Anpassung der automatisch generierte `INSERT` -Anweisung ist, dass es besteht das Risiko, dass unsere Anpassungen, die vom Assistenten überschrieben. Angenommen, wir angepasst der `INSERT` Anweisung, um die Verwendung von umfassen die `Picture` Spalte. Dadurch würde die TableAdapter aktualisiert `Insert` Methode, um einen zusätzlichen Eingabeparameter für die Kategorie s Bild s Binärdaten enthalten. Wir konnten dann erstellen Sie eine Methode in der Business Logic Layer verwenden Sie diese DAL-Methode und das Aufrufen dieser Methode BLL über die Präsentationsebene, und alles wunderbar funktionieren würde. D. h. erst beim nächsten Mal wir TableAdapter über den TableAdapter-Konfigurations-Assistenten konfiguriert. Sobald der Assistent abgeschlossen, unsere Änderungen an der `INSERT` Anweisung überschrieben, die `Insert` Methode würde wieder her, um die alte Form, und getesteten Codes würde nicht mehr kompiliert!

> [!NOTE]
> Diese aufwendig ist ein nicht-Problem bei der Verwendung von gespeicherten Prozeduren anstelle von Ad-hoc-SQL-Anweisungen. Eine zukünftige Lernprogramm wird mithilfe von gespeicherten Prozeduren anstelle von Ad-hoc-SQL-Anweisungen in der Datenzugriffsebene untersuchen.


Um dieses zu vermeiden kann anfallenden, anstatt die automatisch generierten SQL-Anweisungen anpassen s stattdessen eine neue Methode für den TableAdapter erstellen. Diese Methode mit der Bezeichnung `InsertWithPicture`, akzeptiert die Werte für die `CategoryName`, `Description`, `BrochurePath`, und `Picture` Spalten und führen Sie eine `INSERT` -Anweisung, die alle vier Werte in einen neuen Datensatz gespeichert.

Öffnen Sie das typisierte DataSet, und klicken Sie im Designer mit der Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie im Kontextmenü der Abfrage hinzufügen. Dadurch wird der TableAdapter-Abfrage Assistenten aus, der zuerst uns gefragt, wie die TableAdapter-Abfrage für die Datenbank zugreifen, sollte gestartet. Wählen Sie SQL-Anweisungen, und klicken Sie auf Weiter. Als Nächstes fordert für den Typ der Abfrage generiert werden soll. Da wir re erstellen eine Abfrage aus, um einen neuen Datensatz zum Hinzufügen der `Categories` Tabelle, wählen Sie einfügen, und klicken Sie auf Weiter.


[![Wählen Sie die INSERT-Option](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Abbildung 1**: Wählen Sie die Option Einfügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Jetzt müssen wir geben die `INSERT` SQL-Anweisung. Der Assistent automatisch schlägt vor, Ihnen eine `INSERT` Anweisung, die Hauptabfrage des TableAdapter s entspricht. In diesem Fall es s ein `INSERT` -Anweisung, die fügt die `CategoryName`, `Description`, und `BrochurePath` Werte. Aktualisieren Sie die Anweisung, damit die `Picture` Spalte dient zusammen mit einem `@Picture` -Parameter wie folgt:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Im letzten Bildschirm des Assistenten werden Sie gefragt, benennen Sie die neue Methode des TableAdapter. Geben Sie `InsertWithPicture` , und klicken Sie auf ' Fertig stellen '.


[![Name der neuen Methode des TableAdapter InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Abbildung 2**: Benennen Sie die neue Methode des TableAdapter `InsertWithPicture` ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Schritt 2: Aktualisieren der Geschäftslogikebene

Da die Darstellungsschicht sollte nur eine Verbindung mit der Business Logic Layer, anstatt umgehen sie fahren Sie direkt mit der Datenzugriffsebene, wir eine Methode BLL erstellen müssen, die die DAL-Methode aufruft, soeben erstellt wurde (`InsertWithPicture`). In diesem Lernprogramm erstellen Sie eine Methode in der `CategoriesBLL` Klasse mit dem Namen `InsertWithPicture` , akzeptiert als Eingabe drei `string` s und einer `byte` Array. Die `string` Eingabeparameter sind für den Kategorienamen s, Beschreibung und Broschüren Dateipfad, während die `byte` Array ist, für den binären Inhalt des Bilds Kategorie s. Wie der folgende Code zeigt, ruft diese Methode auf BLL entsprechende DAL-Methode:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Stellen Sie sicher, dass Sie vor dem Hinzufügen der typisierte DataSet gespeichert haben die `InsertWithPicture` Methode, um die BLL. Da die `CategoriesTableAdapter` Klassencode basierend auf dem typisierten DataSet wird automatisch generiert, wenn Sie Ich möchte das typisierte DataSet zuerst die Änderungen speichern die `Adapter` Eigenschaft, gewonnene t erfahren, ob die `InsertWithPicture` Methode.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Schritt 3: Auflisten von vorhandenen Kategorien hinzufügen und ihre binäre Daten

In diesem Lernprogramm erstellen wir eine Seite, die ein Endbenutzer auf dem System, eine neue Kategorie hinzufügen kann. Geben Sie ein Bild und Broschüren für die neue Kategorie. In der [vorherigen Lernprogramm](displaying-binary-data-in-the-data-web-controls-cs.md) wir eine GridView mit einem TemplateField und ImageField verwendet, und jede s Kategoriename angezeigt, Beschreibung, Bild und einen Link zu seiner Broschüren herunterladen. Lassen Sie s replizieren diese Funktion in diesem Lernprogramm erstellen eine Seite, die sowohl alle vorhandene Kategorien Listet und ermöglicht eine neue erstellt werden soll.

Öffnen Sie zunächst die `DisplayOrDownload.aspx` Seite aus der `BinaryData` Ordner. Wechseln Sie zur Quellansicht und die GridView und ObjectDataSource s deklarative Syntax kopieren, Einfügen in die `<asp:Content>` Element im `UploadInDetailsView.aspx`. Darüber hinaus nicht vergessen, über kopiert der `GenerateBrochureLink` Methode aus der CodeBehind-Klasse der `DisplayOrDownload.aspx` zu `UploadInDetailsView.aspx`.


[![Kopieren Sie die deklarative Syntax von DisplayOrDownload.aspx zum UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Abbildung 3**: Kopieren Sie die Deklarationssyntax von `DisplayOrDownload.aspx` auf `UploadInDetailsView.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Nach dem Kopieren der deklarativen Syntax und `GenerateBrochureLink` aufzurufende Methode über die `UploadInDetailsView.aspx` Seite, zeigen Sie die Seite über einen Browser, um sicherzustellen, dass alles ordnungsgemäß kopiert wurde. Eine GridView, die acht Kategorien auflisten, die einen Link zum Herunterladen der Broschüren als auch die Kategorie s Bild enthält, sollte angezeigt werden.


[![Jede Kategorie zusammen mit seiner Binärdaten sollte jetzt angezeigt.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Abbildung 4**: Sie sollten jetzt finden Sie unter jeder Kategorie zusammen mit einem Binärdaten ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Schritt 4: Konfigurieren der`CategoriesDataSource`mit dem Support einfügen

Die `CategoriesDataSource` ObjectDataSource verwendet werden, indem die `Categories` GridView bieten derzeit nicht die Möglichkeit zum Einfügen von Daten. Um über dieses Datenquellen-Steuerelement einfügen zu unterstützen, müssen wir ordnen Sie seine `Insert` Methode, um eine Methode in der zugrunde liegenden Objekts `CategoriesBLL`. Insbesondere soll für die Zuordnung der `CategoriesBLL` Methode, die wir, wieder in Schritt2 hinzugefügt `InsertWithPicture`.

Starten Sie, indem Sie auf den Link Konfigurieren von Datenquellen über das ObjectDataSource-s-Smarttag. Der erste Bildschirm zeigt das Objekt, das die Datenquelle so konfiguriert ist, dass arbeiten `CategoriesBLL`. Lassen Sie diese Einstellung als-ist, und klicken Sie neben der Verschiebung der Anwendungszeit auf dem Bildschirm Datenmethoden definieren. Wechseln Sie zur Registerkarte "Einfügen", und wählen Sie die `InsertWithPicture` Methode aus der Dropdown-Liste. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Konfigurieren der ObjectDataSource zur Verwendung der InsertWithPicture-Methode](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren der ObjectDataSource verwenden die `InsertWithPicture` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Nach Abschluss des Assistenten, kann Visual Studio bitten Sie Sie nach Bedarf aktualisieren von Feldern und Schlüssel, die die Daten Web neu generieren, wird Felder steuert. Wählen Sie Nein, da Ja Feld Anpassungen überschrieben werden, die Sie vorgenommen haben, können.


Das ObjectDataSource wird nach Abschluss des Assistenten enthalten nun einen Wert für die `InsertMethod` Eigenschaft sowie `InsertParameters` veranschaulicht, die folgenden deklarativem Markup für die Kategorie der vier Spalten:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Schritt 5: Erstellen der einfügen-Schnittstelle

Erster in behandelt die [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), DetailsView-Steuerelement bietet eine integrierte einfügende-Schnittstelle, die verwendet werden kann, bei der Arbeit mit einem Datenquellen-Steuerelement, das Einfügevorgänge unterstützt. Lassen Sie s, DetailsView-Steuerelement zu dieser Seite oben GridView, die dauerhaft die einfügende Schnittstelle gerendert werden dadurch kann einen Benutzer schnell hinzufügen wie eine neue Kategorie hinzufügen. Beim Hinzufügen einer neuen Kategorie in der DetailsView, GridView unter ihm wird automatisch aktualisiert wird und die neue Kategorie angezeigt.

Start durch Ziehen einer DetailsView aus der Toolbox auf den Designer über die GridView Festlegen seiner `ID` Eigenschaft `NewCategory` und gelöscht wird, die `Height` und `Width` Eigenschaftswerte. Binden Sie es aus dem DetailsView s smart Tag zur vorhandenen `CategoriesDataSource` und aktivieren Sie das Kontrollkästchen einfügen aktivieren.


[![Binden von DetailsView an die CategoriesDataSource und Einfügen aktivieren](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Abbildung 6**: binden, DetailsView der `CategoriesDataSource` und Einfügen aktivieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Um DetailsView in die einfügende Schnittstelle dauerhaft zu rendern, legen dessen `DefaultMode` Eigenschaft `Insert`.

Beachten Sie, DetailsView fünf BoundFields hat `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, und `BrochurePath` zwar die `CategoryID` BoundField wird in der einfügen-Schnittstelle nicht gerendert, da seine `InsertVisible` -Eigenschaftensatz auf `false`. Diese BoundFields vorhanden ist, da sie die zurückgegebene Spalten sind die `GetCategories()` Methode, die ist, was das ObjectDataSource aufruft, um die Daten abzurufen. Zum Einfügen, aber wir Verschlüsselungskennwort t möchten, können den Benutzer aus, geben Sie einen Wert für `NumberOfProducts`. Darüber hinaus muss ihnen das Hochladen eines Bilds für die neue Kategorie sowie zum Hochladen von PDF-Datei für die Broschüren gewähren.

Entfernen der `NumberOfProducts` BoundField aus der, DetailsView vollständig und aktualisieren Sie dann die `HeaderText` Eigenschaften der `CategoryName` und `BrochurePath` BoundFields, Kategorie und Broschüren, bzw. Konvertieren Sie als Nächstes die `BrochurePath` BoundField in ein TemplateField und Hinzufügen einer neuen TemplateField für das Bild dieses neue TemplateField ein `HeaderText` Wert des Bilds. Verschieben der `Picture` TemplateField, sodass diese zwischen ist die `BrochurePath` TemplateField und CommandField.


![Binden von DetailsView an die CategoriesDataSource und Einfügen aktivieren](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Abbildung 7**: binden, DetailsView der `CategoriesDataSource` und Einfügen aktivieren


Wenn Sie konvertiert die `BrochurePath` BoundField in ein TemplateField mithilfe des Dialogfelds Felder bearbeiten, um die TemplateField enthält ein `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate`. Nur die `InsertItemTemplate` ist erforderlich, jedoch so wünschen, entfernen Sie die anderen beiden Vorlagen. An diesem Punkt sollte die DetailsView s deklarative Syntax wie folgt aussehen:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Hinzufügen von FileUpload-Steuerelementen für die Broschüren und Bildfelder

Gegenwärtig die `BrochurePath` TemplateField s `InsertItemTemplate` enthält ein Textfeld, während die `Picture` TemplateField enthält keine Vorlagen. Wir aktualisieren diese zwei TemplateField s müssen `InsertItemTemplate` s FileUpload Steuerelemente verwenden.

Aus den DetailsView s smart Tag, wählen Sie die Vorlagen bearbeiten aus, und wählen Sie dann die `BrochurePath` TemplateField s `InsertItemTemplate` aus der Dropdown-Liste. Entfernen Sie das Textfeld ein, und ziehen Sie dann ein FileUpload-Steuerelement aus der Toolbox in die Vorlage. Legen Sie das Steuerelement FileUpload s `ID` auf `BrochureUpload`. Auf ähnliche Weise Hinzufügen eines FileUpload-Steuerelements, um die `Picture` TemplateField s `InsertItemTemplate`. Legen Sie dieses Steuerelement FileUpload s `ID` auf `PictureUpload`.


[![Hinzufügen eines FileUpload-Steuerelements zu den InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Abbildung 8**: Hinzufügen eines Steuerelements FileUpload, um die `InsertItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Nach dem Herstellen dieser Ergänzungen, werden zwei TemplateField s deklarative Syntax verwendet:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Wenn ein Benutzer eine neue Kategorie hinzufügt, möchten wir sicherstellen, dass die Broschüren und Bild von den richtigen Dateityp aufweist. Für die Broschüren muss der Benutzer eine PDF-Datei angeben. Für das Bild, die wir benötigen die Benutzer eine Bilddatei hochladen, aber lassen wir *alle* image nur Image Dateien eines bestimmten Typs, z. B. gif oder JPG-Format vorliegen? Um die verschiedenen Dateitypen zu ermöglichen, müssen wir d erweitern die `Categories` Schema, um eine Spalte enthalten, die den Dateityp aufzeichnet, damit dieses Typs über an den Client gesendet werden kann `Response.ContentType` in `DisplayCategoryPicture.aspx`. Da wir t Verschlüsselungskennwort wird eine solche Spalte haben, wäre es unerlässlich, um Benutzer zu beschränken, auf die Bereitstellung einer bestimmten Bilddateityp. Die `Categories` Tabelle s vorhandener Bilder sind Bitmaps JPG-Format sind allerdings eine besser geeignete Dateiformat für Bilder, die über das Internet bereitgestellt.

Wenn ein Benutzer einen falschen Dateityp hochlädt, müssen wir auf "Abbrechen" die Einfügung und zeigt folgende Meldung angezeigt, die das Problem. Fügen Sie eine Bezeichnung Websteuerelement unterhalb der DetailsView hinzu. Festlegen seiner `ID` Eigenschaft, um `UploadWarning`deaktivieren, seine `Text` Eigenschaftensatz, der `CssClass` Eigenschaft in "Warnung", und die `Visible` und `EnableViewState` Eigenschaften `false`. Die `Warning` CSS-Klasse definiert ist, im `Styles.css` und rendert den Text in eine große, Rot, kursiv, fette Schriftart.

> [!NOTE]
> Im Idealfall der `CategoryName` und `Description` BoundFields konvertiert von TemplateFields und Einfügen von Schnittstellen angepasst. Die `Description` einfügen-Schnittstelle, z. B. würde wahrscheinlich besser geeignet über ein mehrzeiliges Textfeld. Und seit der `CategoryName` Spalte akzeptiert keine `NULL` Werte, ein RequiredFieldValidator hinzugefügt werden sollen, um sicherzustellen, dass der Benutzer stellt einen Wert für den neuen Kategorienamen s bereit. Diese Schritte sind an dem Leser als Übung übrig. Verweisen auf [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) für einen umfassenden Einblick, erweitern die Änderung Schnittstellen.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Schritt 6: Hochgeladene Broschüren Web Server s im Dateisystem speichern

Wenn der Benutzer die Werte für eine neue Kategorie eingegeben und klickt auf die Schaltfläche zum Einfügen, asynchronen Postback und das Einfügen von Workflow erweitert. Erste, DetailsView s [ `ItemInserting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) ausgelöst wird. Anschließend wird die s ObjectDataSource `Insert()` Methode wird aufgerufen, was dazu führt, in einen neuen Datensatz hinzugefügt wird die `Categories` Tabelle. Danach, DetailsView s [ `ItemInserted` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) ausgelöst wird.

Vor dem ObjectDataSource s `Insert()` Methode aufgerufen wird, müssen Sie zunächst sicherstellen, dass die entsprechenden Dateitypen vom Benutzer hochgeladen wurden und speichern Sie die Broschüren PDF-Datei im Web Server s-Dateisystem. Erstellen Sie einen Ereignishandler für das DetailsView s `ItemInserting` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Der Ereignishandler wird gestartet, durch Verweisen auf die `BrochureUpload` FileUpload-Steuerelement, DetailsView s Vorlagen. Klicken Sie dann wird eine Broschüren hochgeladen wurde, die Erweiterung der hochgeladenen Datei s untersucht. Wenn die Erweiterung nicht ist. PDF-Datei, klicken Sie dann eine Warnung wird angezeigt, die Einfügung wird abgebrochen und beendet die Ausführung des ereignishandlers.

> [!NOTE]
> Vertrauensstellungen der vertrauenden Seite auf der hochgeladenen Datei s-Erweiterung ist nicht zur Sicherstellung, dass die hochgeladene Datei ein PDF-Dokument ist eine sichere Technik. Der Benutzer möglicherweise ein gültiges PDF-Dokument mit der Erweiterung `.Brochure`, oder konnte nicht in der PDF-Dokument erstellt und ihm haben eine `.pdf` Erweiterung. Die binäre Inhalt der Datei s müssen programmgesteuert untersucht werden, um den Dateityp mehr abschließend sicherzustellen. Solche gründliche Ansätze sind jedoch häufig Ausgangsstruktur; Überprüfen die Erweiterung ist für die meisten Szenarien ausreichend.


Entsprechend der Anleitung unter dem [Hochladen von Dateien](uploading-files-cs.md) Tutorial, muss darauf geachtet werden beim Speichern im Dateisystem Dateien, damit dieses Uploads einen Benutzer s nicht einem anderen s überschrieben wird. Für dieses Lernprogramm versuchen wir, die denselben Namen wie die hochgeladene Datei verwenden. Wenn es eine Datei in bereits die `~/Brochures` Verzeichnis mit diesem gleichen Dateinamen, allerdings müssen wir eine Zahl am Ende anfügen, bis ein eindeutiger Name gefunden wird. Z. B., wenn der Benutzer eine Broschüren mit dem Namen Dateiuploads `Meats.pdf`, aber es bereits eine Datei namens ist `Meats.pdf` in der `~/Brochures` Ordner ändern wir den gespeicherten Dateinamen an `Meats-1.pdf`. Wenn vorhanden ist, müssen wir versuchen `Meats-2.pdf`usw., bis ein eindeutiger Dateiname gefunden wird.

Der folgende code verwendet die [ `File.Exists(path)` Methode](https://msdn.microsoft.com/library/system.io.file.exists.aspx) zu bestimmen, ob eine Datei mit dem angegebenen Namen ist bereits vorhanden. Wenn dies der Fall ist, wird er weiterhin wiederholen Sie den neuen Dateinamen für die Broschüren, bis kein Konflikt gefunden wird.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Wenn ein gültiger Dateiname gefunden wurde, muss die Datei in das Dateisystem und die ObjectDataSource s gespeichert werden sollen `brochurePath``InsertParameter` Wert aktualisiert werden, damit diese Dateinamen in die Datenbank geschrieben werden muss. Wie wir gesehen, wieder in haben die *Hochladen von Dateien* Lernprogramm die Datei gespeichert werden kann mithilfe des Steuerelements FileUpload s `SaveAs(path)` Methode. Aktualisiert das ObjectDataSource `brochurePath` verwenden die `e.Values` Auflistung.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Schritt 7: Hochgeladene Bild in der Datenbank speichern.

Zum Speichern der hochgeladenen Bilds in der neuen `Categories` aufzuzeichnen, müssen wir die ObjectDataSource s die hochgeladene Inhalt im Binärformat zuweisen `picture` Parameter in der DetailsView s `ItemInserting` Ereignis. Bevor wir diese Zuordnung vornehmen, müssen wir jedoch, zunächst sicherstellen, dass das hochgeladene Bild eine JPG- und nicht einem anderen Bildtyp ist. Können Sie wie in Schritt 6 s die hochgeladene Bild s-Dateierweiterung zu verwenden, um den Typ zu ermitteln.

Während der `Categories` Tabelle ermöglicht `NULL` Werte für die `Picture` Spalte derzeit alle Kategorien haben ein Bild. Lassen Sie s erzwingen, dass der Benutzer ein Bild angeben, wenn Sie eine neue Kategorie über diese Seite hinzufügen. Der folgende Code überprüft, um sicherzustellen, dass ein Bild hochgeladen wurde, und ob er eine entsprechende Erweiterung hat.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Dieser Code sollte *vor* den Code aus Schritt 6, damit liegt ein Problem mit dem Hochladen von Bildern, wird der Ereignishandler beendet werden, bevor die Broschüren-Datei im Dateisystem gespeichert wird.

Vorausgesetzt, dass eine entsprechende Datei hochgeladen wurde, weisen Sie die hochgeladene Inhalt im Binärformat mit dem Bild-Parameterwert s mit der folgenden Codezeile:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Die vollständige`ItemInserting`-Ereignishandler

Der Vollständigkeit halber hier ist die `ItemInserting` -Ereignishandler in seiner Gesamtheit:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Schritt 8: Korrigieren und die`DisplayCategoryPicture.aspx`Seite

Let s nehmen einen Moment Zeit, um zu testen, die einfügende Schnittstelle und `ItemInserting` Ereignishandler, die in den letzten Schritten erstellt wurde. Besuchen Sie die `UploadInDetailsView.aspx` Seite über einen Browser und der Versuch, eine Kategorie hinzufügen, aber lassen Sie das Bild oder ein nicht-JPG-Bild oder eine PDF-Format Broschüren angeben. In diesen Fällen sollten folgende Fehlermeldung wird angezeigt, und das Einfügen der Workflow abgebrochen.


[![Eine Warnmeldung wird angezeigt, wenn ein ungültiger Typ für die Datei hochgeladen wird](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Abbildung 9**: eine Warnmeldung wird angezeigt, wenn ein ungültiger Typ für die Datei wird hochgeladen ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Wenn Sie überprüft haben, dass die Seite erfordert ein Bild hochgeladen werden und erzielter t PDF-Format oder nicht JPG-Dateien übernehmen, fügen Sie eine neue Kategorie mit ein gültige JPG-Bild, das Broschüren-Feld leer lassen. Nach dem Klicken auf die Schaltfläche zum Einfügen, wird die Seite postback und ein neuer Datensatz werden hinzugefügt werden, um die `Categories` Tabelle mit den hochgeladenen Image s binären Inhalt direkt in der Datenbank gespeichert. GridView wird aktualisiert und zeigt eine Zeile für die neu hinzugefügte Kategorie allerdings, wie in Abbildung 10 gezeigt, die neue Kategorie s Grafik ist nicht korrekt dargestellt.


[![Die neue Kategorie s, auf dem Bild nicht angezeigt wird](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Abbildung 10**: die neue Kategorie s Bild wird nicht angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Ist der Grund, das das neue Bild wird nicht angezeigt, da die `DisplayCategoryPicture.aspx` Seite, die eine angegebene Kategorie s Bild zurückgibt ist so konfiguriert, dass um Bitmaps zu verarbeiten, die einen OLE-Header vorhanden sind. Dieser Header 78 Byte aus entfernt wird die `Picture` s'-Spalte binären Inhalt vor dem Senden an den Client zurück. Aber die JPG-Datei, die wir gerade hochgeladenen für die neue Kategorie verfügt nicht über diesen OLE-Header; aus diesem Grund sind ungültig, die erforderlichen Bytes von s binäre Bilddaten entfernt.

Da es sich jetzt beide Bitmaps mit OLE-Header und JPG-Format in die `Categories` Tabelle, müssen wir aktualisieren `DisplayCategoryPicture.aspx` , damit sie den OLE-Header für die ursprüngliche acht Kategorien Striping und umgeht diese Striping für neuere Kategorie Datensätze. In unserem nächsten Lernprogramm untersucht, wie ein vorhandenen Datensatz s-Image zu aktualisieren, und wir werden alle alten Kategorie Bilder aktualisieren, sodass sie die JPG-Format sind. Verwenden Sie vorerst jedoch den folgenden Code in `DisplayCategoryPicture.aspx` um die OLE-Header nur für die ursprüngliche acht Kategorien zu entfernen:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Durch diese Änderung wird die JPG-Bild jetzt richtig in die GridView gerendert.


[![Die JPG-Images für die neuen Kategorien sind ordnungsgemäß gerendert.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Abbildung 11**: JPG Bilder für die neuen Kategorien sind ordnungsgemäß gerendert ([klicken Sie hier, um das Bild in voller Größe angezeigt](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Schritt 9: Durch das Löschen der Broschüren bei einer Ausnahme

Eine der Herausforderungen von Binärdaten im Web Server s-Dateisystem gespeichert ist, dass es sich um eine Trennung zwischen dem Datenmodell und die binären Daten führt. Daher immer ein Datensatz gelöscht wird, müssen die entsprechenden binären Daten im Dateisystem auch entfernt werden. Dies kann sprachbasierte möglich, wenn Sie auch einfügen. Das folgende Szenario: ein Benutzer eine neue Kategorie angeben, ein gültiges Bild und Broschüren hinzufügt. Beim Klicken auf die Schaltfläche Insert ein ein Postback erfolgt und die DetailsView-s `ItemInserting` Ereignis wird ausgelöst, die Broschüren die Web Server s im Dateisystem speichern. Weiter, das ObjectDataSource-s `Insert()` Methode aufgerufen wird, ruft die der `CategoriesBLL` Klasse s `InsertWithPicture` aufruft der `CategoriesTableAdapter` s `InsertWithPicture` Methode.

Nun, was geschieht, wenn die Datenbank offline ist oder wenn Fehler in der `INSERT` SQL-Anweisung? Deutlich schlägt einfügen fehl, damit die Datenbank keine neue Kategoriezeile hinzugefügt werden. Jedoch können wir noch die hochgeladene Broschüren-Datei auf dem Webserver-s-Datei-System! Diese Datei muss bei einer Ausnahme während des Workflows für das Einfügen von gelöscht werden.

Wie zuvor erläutert in der [Behandlung von BLL- und DAL-Ebene Ausnahmen auf einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Lernprogramm beim Auslösen einer Ausnahme aus in die Tiefen der Architektur ist es durch die verschiedenen Anwendungsebenen protokolliert wird. Bei der Präsentationsschicht, können wir feststellen, ob eine Ausnahme, von der DetailsView s aufgetreten ist `ItemInserted` Ereignis. Dieser Ereignishandler stellt auch die Werte der ObjectDataSource s `InsertParameters`. Wir können aus diesem Grund erstellen Sie einen Ereignishandler für das `ItemInserted` Ereignis, das überprüft, ob eine Ausnahme aufgetreten, und, wenn dies der Fall ist, löscht die Datei, die durch das ObjectDataSource s angegebene `brochurePath` Parameter:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Zusammenfassung

Es gibt eine Reihe von Schritten, die ausgeführt werden muss, um eine webbasierte Schnittstelle zum Hinzufügen von Datensätzen, die Binärdaten enthalten bereitzustellen. Wenn die binären Daten direkt in der Datenbank gespeichert wird, werden wahrscheinlich müssen Sie die Architektur aktualisieren Hinzufügen von bestimmten Methoden, um Fälle zu behandeln, in dem binäre Daten eingefügt werden. Nach die Architektur aktualisiert wurde, ist der nächste Schritt die einfügende Schnittstelle erstellt mithilfe einer DetailsView, die ein Steuerelement FileUpload für jedes Feld Binärdaten enthalten angepasst wurde erreicht werden kann. Die hochgeladenen Daten können dann die Web Server s im Dateisystem gespeichert oder auf einen Daten-Quelle-Parameter in der DetailsView s zugewiesen `ItemInserting` -Ereignishandler.

Speichern von Binärdaten in das Dateisystem erfordert mehr Planung, als das Speichern von Daten direkt in der Datenbank. Ein Benennungsschema muss ausgewählt werden, um einem Benutzer s Upload einer anderen s überschreiben zu vermeiden. Darüber hinaus müssen zusätzliche Schritte ausgeführt werden, die hochgeladene Datei löschen, wenn die Datenbank einfügen ein Fehler auftritt.

Wir haben jetzt die Möglichkeit zum Hinzufügen von neuer Kategorien mit dem System mit einem Broschüren und Bild, aber es gibt es noch zu betrachten zum Aktualisieren einer vorhandenen Kategorie s-Binärdaten oder die Binärdaten für eine gelöschte Kategorie ordnungsgemäß zu entfernen. Wir werden diese beiden Themen in den nächsten Lernprogrammen untersuchen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Dave Gardner Teresa Murphy und Bernadette Leigh. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-binary-data-in-the-data-web-controls-cs.md)
> [Weiter](updating-and-deleting-existing-binary-data-cs.md)
