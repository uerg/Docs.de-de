---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Auf BLL- und DAL-Ebene-Ausnahmebehandlung (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial sehen wir, wie Ausnahmen, die während der Workflow für die Aktualisierung einer bearbeitbaren DataList tactfully behandelt werden.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: f83293f7b8482c2a24c3d825e271582edaadec59
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812079"
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>Auf BLL- und DAL-Ebene-Ausnahmebehandlung (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) oder [PDF-Datei herunterladen](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> In diesem Tutorial sehen wir, wie Ausnahmen, die während der Workflow für die Aktualisierung einer bearbeitbaren DataList tactfully behandelt werden.


## <a name="introduction"></a>Einführung

In der [Übersicht von bearbeiten und Löschen von Daten im DataList-Steuerelement](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) Tutorial wir einem DataList-Steuerelement, die angeboten wird, einfach bearbeiten und Löschen von Funktionen erstellt. Voll funktionsfähig ist, war es kaum Benutzerfreundlicher als alle Fehler, die während der Bearbeitung aufgetreten ist oder Prozess löschen, die in einer nicht behandelten Ausnahme geführt hat. Z. B. der Name des Produkts s auslassen, oder beim Bearbeiten eines Produkts Preis der Eingabe des Werts sehr kostengünstige!, löst eine Ausnahme aus. Da diese Ausnahme im Code nicht abgefangen wird, ausgelöst es bis zu der ASP.NET-Laufzeit, die die Details der Ausnahme s Klicken Sie dann auf der Webseite angezeigt.

Wie wir, in gesehen der [behandeln BLL- und DAL-Ebene von Ausnahmen in einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Tutorial, wenn die Tiefe der Geschäftslogik oder Datenzugriffsschichten, eine Ausnahme ausgelöst wird die Details der Ausnahme werden zurückgegeben, dem ObjectDataSource-Steuerelement und Klicken Sie dann an die GridView. Erläutert, wie diese Ausnahmen ordnungsgemäß durch das Erstellen behandeln `Updated` oder `RowUpdated` -Ereignishandler für das ObjectDataSource-Steuerelement oder eine GridView, überprüfen für eine Ausnahme aus, und gibt dann an, dass die Ausnahme behandelt wurde.

Unsere DataList Tutorials, allerdings sind t, die mit dem ObjectDataSource-Steuerelement für das Aktualisieren und Löschen von Daten. Stattdessen arbeiten wir direkt an die BLL. Um die Ausnahmen, die von der BLL oder DAL erkennen zu können, müssen wir Code im Code-Behind unsere ASP.NET-Seite zur Ausnahmebehandlung implementieren. In diesem Tutorial sehen wir, wie Sie mehr tactfully Behandlung von Ausnahmen, die ausgelöst wird, während ein bearbeitbaren DataList s Workflow aktualisieren.

> [!NOTE]
> In der *eine Übersicht über die von bearbeiten und Löschen von Daten im DataList-Steuerelement* Tutorial erläutert verschiedene Techniken zum Bearbeiten und Löschen von Daten über DataList-Steuerelement einige Techniken beteiligten Nutzung einer ObjectDataSource gegeben, für die Aktualisierung und wird gelöscht. Wenn Sie diese Techniken einsetzen, können Sie behandeln von Ausnahmen von der BLL oder DAL über das ObjectDataSource-s `Updated` oder `Deleted` -Ereignishandler.


## <a name="step-1-creating-an-editable-datalist"></a>Schritt 1: Erstellen einer bearbeitbaren DataList-Steuerelement

Bevor wir zur Behandlung von Ausnahmen, die während der Workflow für die Aktualisierung auftreten fürchten, können Sie s, erstellen Sie zunächst eine bearbeitbare DataList-Steuerelement. Öffnen der `ErrorHandling.aspx` auf der Seite die `EditDeleteDataList` Ordner hinzufügen, einem DataList-Steuerelement in den Designer, legen Sie dessen `ID` Eigenschaft `Products`, und fügen Sie eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse s `GetProducts()` Methode für die Auswahl erfasst; legen Sie die Dropdownlisten in der INSERT-, Update- und Löschen von Registerkarten (keine).


[![Zurückgeben der Produktinformationen mithilfe der GetProducts()-Methode](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Abbildung 1**: Zurückgeben von Informationen für das Produkt verwenden die `GetProducts()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


Nach Abschluss des Assistenten "ObjectDataSource" Visual Studio erstellt automatisch eine `ItemTemplate` für DataList-Steuerelement. Ersetzen Sie dies mit einer `ItemTemplate` , werden alle Produktnamen s und den Preis angezeigt und enthält eine Schaltfläche "Bearbeiten". Als Nächstes erstellen Sie eine `EditItemTemplate` mit ein TextBox-Steuerelement für den Namen "und" Price "und" Schaltflächen "Update" und "Abbrechen". Legen Sie schließlich DataList-Steuerelement s `RepeatColumns` Eigenschaft auf 2.

Nach diesen Änderungen sollte Ihre Seite s deklarativen Markup etwa wie folgt aussehen. Noch einmal überprüfen, um sicherzustellen, dass von der Bearbeitung, "Abbrechen", und Update-Schaltflächen sind ihre `CommandName` Eigenschaften festgelegt werden, zu bearbeiten, Abbrechen und entsprechend zu aktualisieren.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> Für dieses Tutorial DataList-Steuerelement muss Ansichtszustand s aktiviert werden.


Können Sie unseren Fortschritt über einen Browser anzeigen (siehe Abbildung 2).


[![Jedes Produkt enthält eine Schaltfläche "Bearbeiten"](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Abbildung 2**: jedes Produkt enthält eine Schaltfläche "Bearbeiten" ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


Die Schaltfläche "Bearbeiten" wird derzeit nur einen Postback es t dennoch gestalten des Produkts bearbeitet werden. Um die Bearbeitung zu aktivieren, müssen wir für das Erstellen von Ereignishandlern für DataList-Steuerelement s `EditCommand`, `CancelCommand`, und `UpdateCommand` Ereignisse. Die `EditCommand` und `CancelCommand` Ereignisse nicht einfach aktualisieren, DataList-Steuerelement s `EditItemIndex` -Eigenschaft und Binden der Daten an die Datenliste:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

Die `UpdateCommand` -Ereignishandler ist etwas komplizierter. Muss innerhalb des bearbeiteten Produkts s Lesen `ProductID` aus der `DataKeys` Collection zusammen mit dem die Produktnamen s und den Preis in die Textfelder ein, in der `EditItemTemplate`, und rufen Sie dann die `ProductsBLL` s-Klasse `UpdateProduct` Methode vor der Rückgabe DataList-Steuerelement vor der Bearbeitung Zustand.

Let s nur verwenden Sie vorerst genauen den gleichen Code aus der `UpdateCommand` -Ereignishandler in der *Übersicht von bearbeiten und Löschen von Daten im DataList-Steuerelement* Tutorial. Wir fügen den Code, um Ausnahmen in Schritt 2 ordnungsgemäß zu behandeln.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

Bei ungültiger Eingabe kann die werden in Form einer falsch formatierten Einzelpreis, eine ungültige Einheit Preis wie "-" $5.00 oder die Auslassung der Product-s-Name, die eine Ausnahme ausgelöst wird. Da die `UpdateCommand` -Ereignishandler enthält keine Code zur Ausnahmebehandlung an diesem Punkt, bis zu der ASP.NET-Laufzeit die Ausnahme übergeben wird, wo es für den Endbenutzer angezeigt werden (siehe Abbildung 3).


![Wenn eine nicht behandelte Ausnahme auftritt, sieht der Endbenutzer eine Fehlerseite angezeigt.](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Abbildung 3**: Wenn eine nicht behandelte Ausnahme auftritt, sieht der Endbenutzer eine Fehlerseite angezeigt.


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Schritt 2: Ordnungsgemäße Behandlung von Ausnahmen in der UpdateCommand-Ereignishandler

Während der Workflow für die Aktualisierung, Ausnahmen auftreten können, der `UpdateCommand` -Ereignishandler, die BLL oder der DAL. Z. B., wenn ein Benutzer eingibt, einen Preis von zu viel Leistung beanspruchen, die `Decimal.Parse` -Anweisung in der `UpdateCommand` Ereignishandler löst eine `FormatException` Ausnahme. Wenn der Benutzer den Namen des Produkts s ausgelassen oder der Preis auf einen negativen Wert hat, wird die DAL eine Ausnahme ausgelöst.

Wenn eine Ausnahme auftritt, möchten wir eine informationsmeldung auf der Seite selbst anzuzeigen. Hinzufügen eines Web-Bezeichnung-Steuerelement auf der Seite, dessen `ID` nastaven NA hodnotu `ExceptionDetails`. Konfigurieren Sie den Text der Bezeichnung s, zum Anzeigen in einem roten, sehr groß, fett und kursiv Schriftart durch Zuweisen der `CssClass` Eigenschaft, um die `Warning` CSS-Klasse, die in definiert ist die `Styles.css` Datei.

Wenn ein Fehler auftritt, möchten wir nur die Bezeichnung auf einmal angezeigt werden. D. h. bei nachfolgenden Postbacks ist die Warnmeldung Bezeichnung s nicht mehr angezeigt. Dies kann erreicht werden, indem Sie entweder gelöscht wird, die Bezeichnung s `Text` Eigenschaft oder die Einstellungen der `Visible` Eigenschaft `False` in die `Page_Load` -Ereignishandler (wie in der [behandeln BLL- und DAL-Ebene-Ausnahmen in einer ASP Seite .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Tutorial) oder durch Deaktivieren der Unterstützung für den Bezeichnung s anzeigen. Lassen Sie letztere Option verwenden, s an.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Wenn eine Ausnahme ausgelöst wird, weisen wir werden die Details der Ausnahme, um die `ExceptionDetails` Beschriftungs-Steuerelement s `Text` Eigenschaft. Da der Ansichtszustand deaktiviert ist, bei nachfolgenden Postbacks der `Text` programmgesteuerte s eigenschaftsänderungen verloren, Zurücksetzen auf den Standardtext (eine leere Zeichenfolge), ausblenden und die Warnmeldung.

Um zu bestimmen, wenn ein Fehler ausgelöst wurde um eine hilfreiche Meldung auf der Seite anzuzeigen, wir müssen eine `Try ... Catch` -block, um die `UpdateCommand` -Ereignishandler. Die `Try` Teil enthält Code, der zu einer Ausnahme führen kann, während die `Catch` -Block enthält Code, der bei einer Ausnahme ausgeführt wird. Sehen Sie sich die [Exception Handling Fundamentals](https://msdn.microsoft.com/library/2w8f0bss.aspx) auf .NET Framework-Dokumentation für Weitere Informationen im Abschnitt der `Try ... Catch` Block.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Wenn eine Ausnahme eines beliebigen Typs ausgelöst wird, durch den Code der `Try` Block, der `Catch` -s-Block-Code beginnt die Ausführung. Der Typ der Ausnahme, die ausgelöst wird `DbException`, `NoNullAllowedException`, `ArgumentException`, und unmittelbar auf hängt, genau, den Fehler im vornherein vorausging. Wenn dort s ein Problem auf Datenbankebene, eine `DbException` ausgelöst. Wenn für ein Unzulässiger Wert eingegeben wurde die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, oder `ReorderLevel` Felder einer `ArgumentException` ausgelöst, wenn wir Code, um zu überprüfen, ob diese Feldwerte hinzugefügt der `ProductsDataTable` Klasse (finden Sie unter der [ Erstellen einer Geschäftslogikebene](../introduction/creating-a-business-logic-layer-cs.md) Tutorial).

Wir können den Meldungstext, basiert auf dem Typ der Ausnahme eine weitere nützliche Erläuterung für den Endbenutzer bereitstellen. Der folgende Code, der in einem Formular fast identisch verwendet wurde in der [behandeln BLL- und DAL-Ebene von Ausnahmen in einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Tutorial enthält diese Detailebene:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Um dieses Tutorial abgeschlossen haben, rufen Sie einfach die `DisplayExceptionDetails` Methode aus der `Catch` Block übergeben wird, in das aufgefangene `Exception` Instanz (`ex`).

Mit der `Try ... Catch` block vorhanden, die Benutzer eine informativere Fehlermeldung wie die Abbildungen 4 und 5 anzeigen angezeigt werden. Beachten Sie, die bei einer Ausnahme DataList-Steuerelement verbleibt im Bearbeitungsmodus befindet. Dies ist, da nach dem Auftreten der Ausnahme, die ablaufsteuerung sofort an umgeleitet wird die `Catch` Block unter Umgehung des Codes, der DataList-Steuerelement in den Zustand vor der Bearbeitung gibt.


[![Eine Fehlermeldung wird angezeigt, wenn ein Benutzer ein Feld erforderlich lässt](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Abbildung 4**: eine Fehlermeldung wird angezeigt, wenn ein Benutzer ein Feld erforderlich lässt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![Eine Fehlermeldung wird angezeigt, wenn einen Preis negativ](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Abbildung 5**: eine Fehlermeldung wird angezeigt, wenn einen Preis negativ ([klicken Sie, um das Bild in voller Größe anzeigen](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>Zusammenfassung

Geben Sie auf beitragsebene Ereignishandler, die Informationen über alle Ausnahmen, die während des aktualisieren und Löschen von Workflows ausgelöst wurden, sowie Eigenschaften, die festgelegt werden können, um anzugeben, und zwar unabhängig davon, ob die Ausnahme wurde enthalten, GridView und "ObjectDataSource" behandelt. Diese Funktionen sind jedoch nicht verfügbar ist beim Arbeiten mit DataList-Steuerelement, und die BLL direkt verwenden. Stattdessen werden wir für die Implementierung der Ausnahmebehandlung verantwortlich.

In diesem Tutorial erläutert, wie Ausnahmebehandlung zum Aktualisieren von Workflows durch das Hinzufügen ein bearbeitet werden DataList-s-Hinzufügen einer `Try ... Catch` Blockieren der `UpdateCommand` -Ereignishandler. Wenn eine Ausnahme, während der Workflow für die Aktualisierung ausgelöst wird, die `Catch` -s-Block-Code ausgeführt wird, Anzeigen von nützliche Informationen in den `ExceptionDetails` Bezeichnung.

An diesem Punkt prüft DataList-Steuerelement nicht zu verhindern, dass Ausnahmen im vornherein passiert. Obwohl wir wissen, dass ein negativer Preis führt zu einer Ausnahme, die wir wurde hinzugefügt t noch keine Funktionen zur proaktiv verhindern einen Benutzer in eine solche ungültige Eingabe. In unserem nächsten Tutorial erfahren Sie, wie zu reduzieren, dass die Ausnahmen, die durch ungültige Benutzereingaben verursacht werden, durch das Hinzufügen von Steuerelementen zur gültigkeitsprüfung in die `EditItemTemplate`.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Entwurfsrichtlinien für Ausnahmen](https://msdn.microsoft.com/library/ms298399.aspx)
- [Error Logging Modules und Handler (ELMAH)](http://workspaces.gotdotnet.com/elmah) (ein Open Source-Bibliothek zum Protokollieren von Fehlern)
- [Enterprise Library für .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (einschließlich der Exception Management Application Block)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Ken Pespisa. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](performing-batch-updates-cs.md)
> [Weiter](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
