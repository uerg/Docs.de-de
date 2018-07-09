---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Eine Datei einschließlich der Option "Hochladen" beim Hinzufügen eines neuen Datensatzes (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial veranschaulicht die Erstellung eine Web-Schnittstelle, mit dem Benutzer geben Textdaten, sowohl binäre Dateien hochladen. Erläutern Sie die verfügbaren Optionen t...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: 5cc1db20a724c8a060e978e2360b977fb16f1e0c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842380"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Einschließlich der einer Dateiuploadoption beim Hinzufügen eines neuen Datensatzes (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) oder [PDF-Datei herunterladen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> In diesem Tutorial veranschaulicht die Erstellung eine Web-Schnittstelle, mit dem Benutzer geben Textdaten, sowohl binäre Dateien hochladen. Um die verfügbaren Optionen zum Speichern von Binärdaten zu veranschaulichen, wird eine Datei in der Datenbank gespeichert werden, während die andere Datei im Dateisystem gespeichert ist.


## <a name="introduction"></a>Einführung

In den vorherigen beiden Tutorials vorgestellt, die Techniken zum Speichern von Binärdaten, die mit dem Application-s-Datenmodell erläutert, wie Sie mithilfe der FileUpload-Serversteuerelements Dateien vom Client an den Webserver senden, und gesehen, wie diese binäre Daten in einer W dargestellt zugeordnet ist "EB"-Steuerelement. Wir haben noch zu sprechen, wie das Datenmodell jedoch hochgeladene Daten zugeordnet.

In diesem Tutorial erstellen wir eine Webseite, um eine neue Kategorie hinzuzufügen. Auf dieser Seite müssen neben der Textfelder für die s-Kategorienamen und eine Beschreibung ein zwei "FileUpload"-Steuerelemente für die neue Kategorie s Grafik und eine für die Broschüre enthalten. Das hochgeladene Bild werden direkt in den neuen Datensatz s gespeichert `Picture` Spalte während die Broschüre gespeichert werden die `~/Brochures` Ordner mit dem Pfad zu der im neuen Datensatz s gespeicherten Datei `BrochurePath` Spalte.

Vor dem Erstellen dieser neuen Webseite, müssen wir die Architektur zu aktualisieren. Die `CategoriesTableAdapter` Hauptabfrage s ist, kann nicht abgerufen werden. die `Picture` Spalte. Daher den automatisch generierten `Insert` Methode hat nur die Eingaben für die `CategoryName`, `Description`, und `BrochurePath` Felder. Aus diesem Grund müssen wir eine zusätzliche Methode im TableAdapter zu erstellen, die für alle vier auffordert `Categories` Felder. Die `CategoriesBLL` Klasse in der Geschäftslogikschicht werden auch aktualisiert werden müssen.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Schritt 1: Hinzufügen einer`InsertWithPicture`Methode, um die`CategoriesTableAdapter`

Wir die Erstellung der `CategoriesTableAdapter` zurück in die [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Tutorial wir konfiguriert er zum automatischen Generieren von `INSERT`, `UPDATE`, und `DELETE` Anweisungen auf Grundlage der Hauptabfrage. Darüber hinaus wir den TableAdapter den DB direkten Weg, einsetzen, die die Methoden erstellt angewiesen `Insert`, `Update`, und `Delete`. Diese Methoden führen Sie den automatisch generierten `INSERT`, `UPDATE`, und `DELETE` Anweisungen und, folglich annehmen von Eingabeparametern, die basierend auf den Spalten, die von der Hauptabfrage zurückgegeben. In der [Hochladen von Dateien](uploading-files-cs.md) Tutorial wir erweitert die `CategoriesTableAdapter` s hauptspeicherpool für Abfragen verwenden die `BrochurePath` Spalte.

Da die `CategoriesTableAdapter` s Hauptabfrage verweist nicht auf die `Picture` Spalte, wir können weder einen neuen Eintrag noch aktualisieren ein vorhandenes Datensatzes mit einem Wert für die `Picture` Spalte. Um diese Informationen aufzuzeichnen, können wir entweder eine neue Methode erstellen, in den TableAdapter, die speziell für das Einfügen eines Datensatzes mit Binärdaten verwendet wird, oder wir können anpassen, die automatisch generierte `INSERT` Anweisung. Das Problem mit dem Anpassen der automatisch generierte `INSERT` -Anweisung ist, dass wir Risiko besteht, dass unsere Anpassungen, die vom Assistenten überschrieben. Beispiel: Angenommen, dass wir angepasst der `INSERT` Anweisung, um die Verwendung von umfassen die `Picture` Spalte. Dadurch würde die TableAdapter aktualisiert `Insert` Methode, um einen zusätzlichen Eingabeparameter für die Kategorie s Bild s binären Daten einzufügen. Wir könnten dann erstellen Sie eine Methode in der Geschäftslogikebene dieser DAL-Methode und Aufrufen dieser Methode BLL, durch die Darstellungsschicht, und alles funktioniert einwandfrei. D. h. bis zum nächsten wir den TableAdapter mit dem TableAdapter-Konfigurations-Assistenten konfiguriert. Sobald der Assistent abgeschlossen, unsere angepasst werden, um die `INSERT` Anweisung überschrieben werden, die `Insert` Methode würde in ihrer alten Form zurückgesetzt, und unser Code würde nicht mehr kompiliert!

> [!NOTE]
> Dieser unzulänglichkeiten ist nicht auf, wenn gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen zu verwenden. Eine zukünftige Tutorial wird die Verwendung von gespeicherten Prozeduren anstelle von Ad-hoc-SQL-Anweisungen in der Datenzugriffsebene.


Zur Vermeidung dieses Potenzial kann Kopfschmerzen, statt die automatisch generierte SQL-Anweisungen anpassen s stattdessen eine neue Methode für den TableAdapter zu erstellen. Diese Methode, mit dem Namen `InsertWithPicture`, akzeptiert die Werte für die `CategoryName`, `Description`, `BrochurePath`, und `Picture` Spalten, und führen Sie eine `INSERT` -Anweisung, die alle vier Werte in einen neuen Datensatz gespeichert.

Öffnen Sie das typisierte DataSet, und klicken Sie im Designer mit der Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie im Kontextmenü der Abfrage hinzufügen. Dadurch wird der TableAdapter-Abfrage Konfigurations-Assistenten, der damit beginnt, bitten Sie uns mit, wie die TableAdapter-Abfrage für die Datenbank zugreifen, sollten. Wählen Sie die SQL-Anweisungen, und klicken Sie auf Weiter. Im nächste Schritt fordert für den Typ der Abfrage generiert werden soll. Da wir erneut erstellen eine Abfrage, um einen neuen Eintrag hinzufügen die `Categories` Tabelle, wählen Sie die INSERT aus, und klicken Sie auf Weiter.


[![Wählen Sie die INSERT-Option](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Abbildung 1**: Wählen Sie die Option Einfügen ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Jetzt müssen wir geben die `INSERT` SQL-Anweisung. Vom Assistenten automatisch vorgeschlagene ein `INSERT` Anweisung, die in der Hauptabfrage des TableAdapter s entspricht. In diesem Fall es s ein `INSERT` -Anweisung, die fügt die `CategoryName`, `Description`, und `BrochurePath` Werte. Aktualisieren Sie die Anweisung, damit die `Picture` enthalten zusammen mit einem `@Picture` Parameter wie folgt:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Der letzten Seite des Assistenten fordert uns um die neue Methode des TableAdapter zu nennen. Geben Sie `InsertWithPicture` , und klicken Sie auf "Fertig stellen".


[![Name der neuen InsertWithPicture TableAdapter-Methode](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Abbildung 2**: Benennen Sie die neue Methode des TableAdapter `InsertWithPicture` ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Schritt 2: Aktualisieren den Geschäftslogikebene

Da übersprungen, es direkt an die Datenzugriffsebene gesendet werden, müssen wir eine BLL-Methode erstellen, der DAL-Methode aufruft, wir gerade erstellt haben, statt die Darstellungsschicht sollte nur eine Verbindung mit der Geschäftslogikschicht (`InsertWithPicture`). In diesem Tutorial erstellen Sie eine Methode in der `CategoriesBLL` Klasse mit dem Namen `InsertWithPicture` , akzeptiert als Eingabe drei `string` s und einem `byte` Array. Die `string` Eingabeparameter sind für die Kategorie s-Name, Beschreibung und Broschüre Dateipfad, während die `byte` Array ist, für den binären Inhalt der Kategorie s-Abbildung. Wie in der folgende Code gezeigt wird, ruft diese Geschäftslogikschicht-Methode die entsprechende DAL-Methode:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Stellen Sie sicher, dass Sie das typisierte DataSet, vor dem Hinzufügen gespeichert haben der `InsertWithPicture` Methode, um die BLL. Da die `CategoriesTableAdapter` Klassencode basierend auf dem typisierten DataSet wird automatisch generiert, wenn Sie Ich möchte zuerst die Änderungen auf das typisierte DataSet speichern die `Adapter` gewonnen t Eigenschaft kennen die `InsertWithPicture` Methode.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Schritt 3: Auflisten der vorhandenen Kategorien und deren Binärdaten

In diesem Tutorial erstellen wir eine Seite, die ein Endbenutzer eine neue Kategorie hinzuzufügen, für das System, kann ein Bild und Broschüre bereitstellen, für die neue Kategorie. In der [vorherigen Lernprogramm](displaying-binary-data-in-the-data-web-controls-cs.md) einer GridView-Ansicht und ein TemplateField und ImageField zu jeder Kategorie s Anzeigenamen verwendet Beschreibung, Bild und einen Link zum Herunterladen der Broschüre. Lassen Sie s, die diese Funktionalität zu replizieren, in diesem Tutorial erstellen eine Seite, die alle vorhandene Kategorien enthält und für neue Dateien erstellt werden kann.

Öffnen Sie zunächst die `DisplayOrDownload.aspx` Seite die `BinaryData` Ordner. Wechseln Sie zur Quellansicht, und kopieren Sie die GridView und "ObjectDataSource" s deklarative Syntax, Einfügen in die `<asp:Content>` Element im `UploadInDetailsView.aspx`. Darüber hinaus nicht vergessen, kopiert der `GenerateBrochureLink` Methode aus der CodeBehind-Klasse der `DisplayOrDownload.aspx` zu `UploadInDetailsView.aspx`.


[![Kopieren Sie die deklarative Syntax von DisplayOrDownload.aspx zum UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Abbildung 3**: Kopieren Sie die Deklarationssyntax von `DisplayOrDownload.aspx` zu `UploadInDetailsView.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Nach dem Kopieren der deklarativen Syntax und `GenerateBrochureLink` Methode über die `UploadInDetailsView.aspx` Seite, zeigen Sie die Seite über einen Browser, um sicherzustellen, dass alles richtig kopiert wurde. Auflisten von acht Kategorien GridView sollte, die einen Link zum Herunterladen der Broschüre als auch in der Kategorie s Abbildung enthält angezeigt werden.


[![Sie sollten jetzt jede Kategorie zusammen mit der binären Daten sehen.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Abbildung 4**: Sie sollten jetzt finden Sie unter jeder Kategorie zusammen mit einem Binärdaten ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Schritt 4: Konfigurieren der`CategoriesDataSource`mit dem Support einfügen

Die `CategoriesDataSource` "ObjectDataSource" ein, die die `Categories` GridView bieten derzeit nicht die Möglichkeit zum Einfügen von Daten. Um über das Datenquellen-Steuerelement einfügen zu unterstützen, müssen wir ordnen die `Insert` Methode, um eine Methode in der zugrunde liegenden Objekts, `CategoriesBLL`. Insbesondere soll für die Zuordnung der `CategoriesBLL` Methode, die wir hinzugefügt haben, wieder in Schritt2 `InsertWithPicture`.

Starten Sie, indem Sie auf den Link "Datenquelle konfigurieren" aus dem "ObjectDataSource"-s-Smarttag. Der erste Bildschirm zeigt das Objekt, das die Datenquelle so konfiguriert ist, dass arbeiten `CategoriesBLL`. Lassen Sie diese Einstellung als-ist, und klicken Sie neben der Verschiebung der Anwendungszeit auf dem Bildschirm definieren Methoden auf. Verschieben Sie auf der Registerkarte "Einfügen", und wählen Sie die `InsertWithPicture` Methode aus der Dropdown-Liste. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der InsertWithPicture-Methode](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Abbildung 5**: Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `InsertWithPicture` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Nach Abschluss des Assistenten, kann Visual Studio bitten Sie Sie zum Aktualisieren von Feldern und Schlüssel können, die die Daten Web neu generieren, werden Felder steuert. Wählen Sie Nein, da Sie Ja alle Anpassungen an Feldern überschrieben werden, die Sie vorgenommen haben, können.


Nach Abschluss des Assistenten an, dem ObjectDataSource-Steuerelement enthält jetzt einen Wert für die `InsertMethod` Eigenschaft als auch `InsertParameters` für die Kategorie mit vier Spalten, wie das folgende deklaratives Markup veranschaulicht:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Schritt 5: Erstellen der einfügen-Schnittstelle

In erster behandelt die [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), DetailsView-Steuerelement bietet eine integrierte einfügende-Schnittstelle, die verwendet werden kann, bei der Verwendung von Datenquellen-Steuerelement, das Einfügevorgänge unterstützt. Lassen Sie s fügen einem DetailsView-Steuerelement auf dieser Seite über der GridView, die die einfügende-Schnittstelle, dauerhaft rendern, sodass Benutzer schnell eine neue Kategorie hinzuzufügen. Beim Hinzufügen einer neuen Kategorie in der DetailsView, GridView unter ihm wird automatisch aktualisiert wird und die neue Kategorie angezeigt.

Beginnen Sie, indem Sie ziehen eine DetailsView aus der Toolbox in den Designer über die GridView, Festlegen der `ID` Eigenschaft `NewCategory` und Beseitigen der `Height` und `Width` Eigenschaftswerte. Aus DetailsView s Smarttags, binden Sie ihn dem vorhandenen `CategoriesDataSource` und aktivieren Sie das Kontrollkästchen einfügen aktivieren.


[![Binden von DetailsView an die CategoriesDataSource und Einfügen aktivieren](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Abbildung 6**: DetailsView zum Binden der `CategoriesDataSource` und Einfügen aktivieren ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Um in die einfügende Schnittstelle DetailsView dauerhaft zu rendern, legen dessen `DefaultMode` Eigenschaft `Insert`.

Beachten Sie, dass die DetailsView fünf BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, und `BrochurePath` zwar die `CategoryID` BoundField ist in der einfügen-Schnittstelle nicht gerendert, weil die `InsertVisible` -Eigenschaftensatz auf `false`. Diese BoundFields vorhanden ist, da sie die von zurückgegebenen Spalten sind die `GetCategories()` -Methode, die ist, was dem ObjectDataSource-Steuerelement aufgerufen, um die Daten abzurufen. Zum Einfügen, aber wir Raten t ermöglichen dem Benutzer, die für einen Wert angeben möchten `NumberOfProducts`. Darüber hinaus müssen wir ihnen das Hochladen eines Bilds für die neue Kategorie sowie zum Hochladen einer PDF-Datei für die Broschüre gewähren.

Entfernen der `NumberOfProducts` BoundField aus dem DetailsView vollständig und aktualisieren Sie dann die `HeaderText` Eigenschaften der `CategoryName` und `BrochurePath` BoundFields Kategorie und Broschüre, bzw. Als Nächstes konvertiert der `BrochurePath` BoundField in ein TemplateField und fügen Sie ein neues TemplateField für das Bild, sodass dieser neuen TemplateField eine `HeaderText` Wert des Bilds. Verschieben der `Picture` TemplateField, damit es zwischen ist die `BrochurePath` TemplateField und CommandField.


![Binden von DetailsView an die CategoriesDataSource und Einfügen aktivieren](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Abbildung 7**: DetailsView zum Binden der `CategoriesDataSource` und Einfügen aktivieren


Wenn Sie konvertiert das `BrochurePath` BoundField in ein TemplateField über das Dialogfeld für die Felder bearbeiten, um das TemplateField enthält ein `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate`. Nur die `InsertItemTemplate` ist erforderlich, jedoch gern die anderen beiden Vorlagen zu entfernen. An diesem Punkt sollte Ihre DetailsView s deklarative Syntax wie folgt aussehen:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Hinzufügen von "FileUpload"-Steuerelemente für die Broschüre und die Bildfelder

Gegenwärtig kann die `BrochurePath` TemplateField s `InsertItemTemplate` enthält ein Textfeld, während die `Picture` TemplateField enthält keine Vorlagen. Wir müssen diese beiden TemplateField s aktualisieren `InsertItemTemplate` e zur Verwendung von "FileUpload"-Steuerelemente.

Von DetailsView s Smarttags, wählen Sie die Vorlagen bearbeiten aus, und wählen Sie dann die `BrochurePath` TemplateField s `InsertItemTemplate` aus der Dropdown-Liste. Entfernen Sie das Textfeld ein, und klicken Sie dann ziehen Sie ein "FileUpload"-Steuerelement aus der Toolbox in die Vorlage. Legen Sie das Steuerelement "FileUpload" s `ID` zu `BrochureUpload`. Auf ähnliche Weise Hinzufügen einer FileUpload-Serversteuerelements, die `Picture` TemplateField s `InsertItemTemplate`. Legen Sie diese FileUpload-Serversteuerelements s `ID` zu `PictureUpload`.


[![Hinzufügen einer FileUpload-Serversteuerelements, die InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Abbildung 8**: Fügen Sie ein "FileUpload"-Steuerelement auf die `InsertItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Wenn Sie diese Erweiterungen haben, werden die zwei TemplateField s deklarative Syntax:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Wenn ein Benutzer eine neue Kategorie hinzufügt, möchten wir sicherstellen, dass der Broschüre und Bild von den richtigen Dateityp aufweist. Für die Broschüre muss der Benutzer eine PDF-Datei angeben. Für das Bild, die wir benötigen die Benutzer eine Bilddatei hochladen, aber lassen wir *alle* image-Datei oder eines bestimmten Typs, z. B. GIF-Dateien oder JPGs nur Bilddateien? Um die verschiedenen Dateitypen zu ermöglichen, müssen wir d erweitern die `Categories` Schema, um eine Spalte enthalten, die den Dateityp aufzeichnet, damit dieses Typs über an den Client gesendet werden kann `Response.ContentType` in `DisplayCategoryPicture.aspx`. Da wir Raten von t eine solche Spalte haben, wäre es ratsam, um Benutzer zu beschränken, für die Bereitstellung nur eines bestimmten Bild-Dateityps. Die `Categories` s'-Tabelle vorhandene Images sind Bitmaps, aber JPGs ein geeigneter Dateiformat für Bilder, die über das Internet bereitgestellt.

Wenn ein Benutzer einen falschen Dateityp hochgeladen wird, müssen wir brechen Sie die Einfügung und zeigt eine Meldung angezeigt, das Problem. Fügen Sie ein Label-Steuerelement unterhalb der DetailsView hinzu. Festlegen der `ID` Eigenschaft, um `UploadWarning`, deaktivieren Sie die `Text` Eigenschaftensatz, der `CssClass` Eigenschaft in "Warnung", und die `Visible` und `EnableViewState` Eigenschaften, die `false`. Die `Warning` CSS-Klasse ist in definiert `Styles.css` und rendert den Text in einer großen, Rot, kursiv, fett-Schriftart.

> [!NOTE]
> Im Idealfall die `CategoryName` und `Description` BoundFields konvertiert von TemplateFields und Einfügen von Schnittstellen angepasst. Die `Description` -Schnittstelle, z. B. einfügen würden wahrscheinlich besser geeignet sein über einem mehrzeiligen Textfeld. Und da die `CategoryName` Spalte akzeptiert keine `NULL` Werte, ein RequiredFieldValidator hinzugefügt werden sollen, um sicherzustellen, dass der Benutzer gibt einen Wert für den neuen Namen für die Kategorie s. Dem Reader werden diese Schritte als Übung überlassen. Zurückgreifen [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) für einen detaillierten Einblick in die Schnittstellen für Änderungen erweitern.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Schritt 6: Speichern der hochgeladenen Broschüre s-Dateisystem der Web-Server

Wenn der Benutzer die Werte für eine neue Kategorie eingegeben und die Einfügen-Schaltfläche klickt, ein Postback auftritt und der Einfügen von Workflow erweitert. Zunächst wird das DetailsView-s [ `ItemInserting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) ausgelöst wird. Anschließend wird das "ObjectDataSource"-s `Insert()` Methode wird aufgerufen, was dazu führt, in dem ein neuer Datensatz hinzugefügt wird die `Categories` Tabelle. Danach die DetailsView s [ `ItemInserted` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) ausgelöst wird.

Vor dem "ObjectDataSource"-s `Insert()` Methode aufgerufen wird, müssen Sie zunächst sicherstellen, dass die entsprechenden Dateitypen vom Benutzer hochgeladenen und speichern Sie die PDF-Broschüre auf das Dateisystem der Web-s-Server. Erstellen Sie einen Ereignishandler für das DetailsView s `ItemInserting` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Der Ereignishandler startet durch Verweisen auf die `BrochureUpload` FileUpload-Serversteuerelements der DetailsView-s-Vorlagen aus. Klicken Sie dann wird eine Broschüre hochgeladen wurde, die Erweiterung der hochgeladenen Datei s überprüft. Wenn die Erweiterung nicht. PDF-Datei, klicken Sie dann eine Warnung wird angezeigt, der Einfügevorgang wird abgebrochen und beendet die Ausführung des ereignishandlers.

> [!NOTE]
> Der vertrauenden Seite für die hochgeladene Datei-s-Erweiterung ist nicht dafür, dass die hochgeladene Datei ein PDF-Dokument ist eine sichere Technik. Der Benutzer möglicherweise auf ein gültiges PDF-Dokument mit der Erweiterung `.Brochure`, oder ein nicht-PDF-Dokument erstellt und erhalten sie eine `.pdf` Erweiterung. Die binäre Inhalt der Datei s müssen programmgesteuert untersucht werden, um den Dateityp mehr abschließend ermitteln. Umfassenden Methoden, sind jedoch häufig zu viel des guten; Überprüfen die Erweiterung ist für die meisten Szenarien ausreichend.


Siehe die [Hochladen von Dateien](uploading-files-cs.md) Tutorial, Vorsicht beim Speichern im Dateisystem Dateien, damit dieses eine Benutzer-s-Uploads einen anderen s nicht überschrieben wird. In diesem Tutorial versuchen wir den gleichen Namen wie die hochgeladene Datei zu verwenden. Wenn es eine Datei in bereits die `~/Brochures` Verzeichnis unter diesem Namen, jedoch werden wir eine Zahl am Ende anfügen, bis ein eindeutiger Name gefunden wird. Wenn der Benutzer eine Broschüre-Datei, die mit dem Namen hochgeladen wird z. B. `Meats.pdf`, doch es bereits eine Datei namens ist `Meats.pdf` in die `~/Brochures` Ordner wir den gespeicherten Dateinamen zu ändern `Meats-1.pdf`. Wenn Sie vorhanden ist, versuchen wir `Meats-2.pdf`, und so weiter, bis ein eindeutiger Dateiname gefunden wird.

Der folgende code verwendet die [ `File.Exists(path)` Methode](https://msdn.microsoft.com/library/system.io.file.exists.aspx) zu bestimmen, ob eine Datei mit dem angegebenen Namen ist bereits vorhanden. Wenn dies der Fall ist, wird weiterhin wiederholen Sie den neuen Dateinamen für die Broschüre, bis kein Konflikt gefunden wird.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Nachdem Sie ein gültigen Dateinamen gefunden wurde, wird die Datei in das Dateisystem und die "ObjectDataSource"-s gespeichert werden muss `brochurePath``InsertParameter` Wert aktualisiert werden, damit der Dateiname in die Datenbank geschrieben werden muss. Wie wir, in gesehen der *Hochladen von Dateien* Tutorial die Datei gespeichert werden kann mithilfe des "FileUpload"-Steuerelements s `SaveAs(path)` Methode. Aktualisieren Sie das "ObjectDataSource"-s `brochurePath` verwenden die `e.Values` Auflistung.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Schritt 7: Speichern der hochgeladenen Abbildung in der Datenbank

Zum Speichern der hochgeladenen Abbildung in der neuen `Categories` aufzuzeichnen, müssen wir das "ObjectDataSource"-s den hochgeladenen Inhalt im Binärformat zuweisen `picture` Parameter im DetailsView s `ItemInserting` Ereignis. Bevor wir diese Zuordnung vornehmen, müssen wir jedoch zunächst sicherstellen, dass das hochgeladene Bild eine JPG und nicht einem anderen Bildtyp ist. Lassen Sie s die hochgeladene Bild-s-Dateierweiterung zu verwenden, um den Typ zu ermitteln, wie in Schritt 6.

Während der `Categories` Table erlaubt `NULL` Werte für die `Picture` Spalte derzeit alle Kategorien haben Sie ein Bild. Lassen Sie s erzwingen, dass Benutzer ein Bild angeben, wenn Sie eine neue Kategorie über diese Seite hinzufügen. Der folgende Code überprüft, um sicherzustellen, dass ein Bild hochgeladen wurde und eine entsprechende Erweiterung aufweist.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Dieser Code platziert werden soll *vor* den Code aus Schritt 6, damit liegt ein Problem mit dem Hochladen von Bildern, wird der Ereignishandler beendet werden, bevor die Broschüre-Datei im Dateisystem gespeichert wird.

Vorausgesetzt, dass eine entsprechende Datei hochgeladen wurde, weisen Sie den hochgeladenen Inhalt im Binärformat auf das Bild s Parameterwert mit der folgenden Zeile des Codes:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Die vollständige`ItemInserting`-Ereignishandler

Der Vollständigkeit halber hier ist die `ItemInserting` -Ereignishandler in seiner Gesamtheit:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Schritt 8: Korrigieren der`DisplayCategoryPicture.aspx`Seite

Let-s in Ruhe, die die einfügende Schnittstelle zu testen und `ItemInserting` -Ereignishandler, die in den letzten Schritten erstellt wurde. Besuchen Sie die `UploadInDetailsView.aspx` Seite über einen Browser, und versuchen, eine Kategorie hinzuzufügen und lassen Sie das Bild, oder geben Sie ein nicht-JPG-Bild oder eine nicht-PDF-Broschüre. In diesen Fällen sollten folgende Fehlermeldung wird angezeigt, und der Insert-Workflow abgebrochen.


[![Eine Warnmeldung wird angezeigt, wenn ein ungültiger Typ für die Datei hochgeladen wird](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Abbildung 9**: eine Warnmeldung wird angezeigt, wenn ein ungültiger Typ für die Datei wird hochgeladen ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Wenn Sie überprüft haben, dass die Seite erfordert ein Bild hochgeladen werden und gewonnenem t akzeptiert nicht im PDF-Format oder nicht-JPG-Dateien, Hinzufügen einer neuen Kategorie mit einem gültigen JPG-Bild, das Broschüre-Feld leer bleibt. Nach dem Klicken auf die Schaltfläche "Insert", wird die Seite postback und wird ein neuer Datensatz hinzugefügt werden die `Categories` Tabelle mit den binären hochgeladenen Image s-Inhalt direkt in der Datenbank gespeichert. GridView wird aktualisiert und zeigt eine Zeile für die neu hinzugefügte Kategorie, jedoch, wie in Abbildung 10 gezeigt, das die neue Kategorie s Grafik ist nicht korrekt dargestellt.


[![Die neue Kategorie s, die Bild nicht angezeigt wird](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Abbildung 10**: Bild nicht angezeigt wird, die neue Kategorie-s ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Ist der Grund, der das neue Bild wird nicht angezeigt, da die `DisplayCategoryPicture.aspx` Seite, die ein Bild für die angegebene Kategorie s zurückgibt ist so konfiguriert, um Bitmaps zu verarbeiten, die einen OLE-Header aufweisen. Dieser Header 78 Byte wird entfernt, von der `Picture` s'-Spalte binären Inhalt, bevor sie gesendet werden zurück an den Client. Die JPG-Datei, die wir gerade hochgeladen haben, für die neue Kategorie weist jedoch keine dieser OLE-Header; aus diesem Grund werden gültige, erforderlichen Bytes aus dem Image s binären Daten entfernt.

Da es jetzt beide Bitmaps mit OLE-Header und JPG-Format in die `Categories` Tabelle müssen wir aktualisieren `DisplayCategoryPicture.aspx` , damit sie den OLE-Header für die ursprüngliche acht Kategorien entfernt wird, und umgeht diese stripping für neuere Datensätze der Kategorie. In unserem nächsten Tutorial untersucht, wie Sie ein vorhandenen Datensatz s-Image zu aktualisieren, und wir alle alten Kategorie Bilder, damit sie JPGs sind. Verwenden Sie vorerst jedoch den folgenden Code in `DisplayCategoryPicture.aspx` die OLE-Header nur für die ursprüngliche acht Kategorien zu entfernen:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Durch diese Änderung wird die JPG-Bild jetzt ordnungsgemäß in den GridView-Ansicht gerendert.


[![Die JPG-Bilder für die neuen Kategorien sind ordnungsgemäß gerendert.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Abbildung 11**: die JPG-Bilder für die neuen Kategorien werden zum korrekten rendern ([klicken Sie, um das Bild in voller Größe anzeigen](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Schritt 9: Löschen der Broschüre bei einer Ausnahme

Eine der Herausforderungen beim Speichern von Binärdaten, auf das Dateisystem der Web-Server-s ist, dass es sich um eine Trennung zwischen dem Datenmodell und die binären Daten führt. Aus diesem Grund, wenn ein Datensatz gelöscht wird, müssen die entsprechenden binären Daten im Dateisystem auch entfernt werden. Dies kann ins Spiel kommen, wenn sowie einfügen. Das folgende Szenario: ein Benutzer fügt eine neue Kategorie, ein gültiges Bild und Broschüre angeben. Beim Klicken auf die Einfügen-Schaltfläche, ein Postback auftritt und die DetailsView s `ItemInserting` Ereignis wird ausgelöst, die Broschüre in das Dateisystem der Web-s-Server gespeichert. Weiter, das "ObjectDataSource"-s `Insert()` Methode wird aufgerufen, ruft der `CategoriesBLL` s-Klasse `InsertWithPicture` -Methode, die Aufrufe der `CategoriesTableAdapter` s `InsertWithPicture` Methode.

Jetzt, was geschieht, wenn die Datenbank offline ist oder wenn es ein Fehler in ist der `INSERT` SQL-Anweisung? Eindeutig schlägt der EINFÜGEVORGANG fehl, also keine neue Kategoriezeile in der Datenbank hinzugefügt wird. Aber es gibt noch die hochgeladenen Broschüre Datei auf Dateisystem der Web-Server-s! Diese Datei muss sich bei einer Ausnahme während der Einfügen von Workflow gelöscht werden soll.

Wie bereits erwähnt in der [behandeln BLL- und DAL-Ebene von Ausnahmen in einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Tutorial, wenn eine Ausnahme aus in die Tiefe der Architektur ausgelöst wird, wird es durch die verschiedenen Ebenen protokolliert. Auf der Darstellungsschicht können wir feststellen, ob eine Ausnahme, von der DetailsView s aufgetreten ist `ItemInserted` Ereignis. Dieser Ereignishandler stellt auch die Werte von "ObjectDataSource" s `InsertParameters`. Daher können wir erstellen einen Ereignishandler für die `ItemInserted` -Ereignis, das überprüft, ob eine Ausnahme aufgetreten, und, wenn dies der Fall ist, löscht die Datei, die gemäß der "ObjectDataSource"-s `brochurePath` Parameter:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Zusammenfassung

Es gibt eine Reihe von Schritten, die ausgeführt werden muss, um eine webbasierte Schnittstelle zum Hinzufügen von Einträgen, die Binärdaten enthalten bereitzustellen. Wenn die binären Daten direkt in der Datenbank gespeichert wird, ist es wahrscheinlich, die Sie benötigen zum Aktualisieren der Architektur, Hinzufügen von bestimmten Methoden um Fälle zu behandeln, in denen Binärdaten eingefügt wird. Nach die Architektur aktualisiert wurde, wird im nächsten Schritt die einfügende-Schnittstelle, erstellt, die mit einem DetailsView, die konfiguriert wurde, dass ein "FileUpload"-Steuerelement für jedes Feld Binärdaten einschließen ausgeführt werden können. Die hochgeladenen Daten können dann in das Dateisystem der Web-Server s gespeichert oder Data Source Parameter in der DetailsView s zugewiesen `ItemInserting` -Ereignishandler.

Speichern Binärdaten in das Dateisystem erfordert mehr Planung, als das Speichern von Daten direkt in der Datenbank. Ein Benennungsschema muss ausgewählt werden, um einem Benutzer s Upload einer anderen s überschreiben zu vermeiden. Darüber hinaus müssen zusätzliche Schritte ausgeführt werden, um die hochgeladene Datei zu löschen, wenn die Einfügung für die Datenbank ein Fehler auftritt.

Wir haben jetzt die Möglichkeit zum Hinzufügen von neuer Kategorien an das System mit einer Broschüre und Bild, aber wir haben noch zu Vorgehensweise beim Aktualisieren einer vorhandenen Kategorie s-Binärdaten oder die binären Daten für eine gelöschte Kategorie richtig zu entfernen. Wir werden diesen zwei Themen im nächsten Tutorial untersuchen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Dave Gardner Teresa Murphy und Bernadette Leigh. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-binary-data-in-the-data-web-controls-cs.md)
> [Weiter](updating-and-deleting-existing-binary-data-cs.md)
