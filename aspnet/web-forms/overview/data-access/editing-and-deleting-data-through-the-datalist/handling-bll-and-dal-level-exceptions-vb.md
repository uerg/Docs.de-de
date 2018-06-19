---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Ausnahmebehandlung BLL und DAL-Ebene (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm sehen wir, tactfully Behandeln von Ausnahmen während der Aktualisierung einer bearbeitbaren DataList-Workflow ausgelöst wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ee0eeff08b6ba490b401540de833eecd7122a17
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880101"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Ausnahmebehandlung BLL und DAL-Ebene (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) oder [PDF herunterladen](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> In diesem Lernprogramm sehen wir, tactfully Behandeln von Ausnahmen während der Aktualisierung einer bearbeitbaren DataList-Workflow ausgelöst wird.


## <a name="introduction"></a>Einführung

In der [Übersicht von bearbeiten und Löschen von Daten in das DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) Lernprogramm erstellten DataList, die einfach bearbeiten und Löschen von Funktionen bereitgestellt. Während voll funktionsfähig ist, wurde es kaum anwenderfreundlich, wie alle Fehler, die während der Bearbeitung aufgetreten sind, oder löschen Verarbeiten einer nicht behandelten Ausnahme geführt hat. Z. B. den Produktnamen s auslassen, oder bei der Bearbeitung eines Produkts der Eingabe des Preiswerts sehr kostengünstig!, löst eine Ausnahme aus. Da diese Ausnahme im Code nicht abgefangen wird, bewegt sich es bis zu die ASP.NET-Laufzeit, in dem die Ausnahmedetails s Klicken Sie dann auf der Webseite angezeigt.

Wie wir gesehen, in haben der [Behandlung von BLL- und DAL-Ebene Ausnahmen auf einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Lernprogramm, wenn eine Ausnahme, aus die Tiefe von der Geschäftslogik oder der Datenzugriffsebene ausgelöst wird, die Details der Ausnahme werden zurückgegeben, das ObjectDataSource und Klicken Sie dann an die GridView. Wurde erläutert, wie der ordnungsgemäßen Behandlung dieser Ausnahmen durch das Erstellen `Updated` oder `RowUpdated` -Ereignishandler für das ObjectDataSource oder GridView, eine Ausnahme zu suchen, und gibt dann an, dass die Ausnahme behandelt wurde.

Unsere DataList Lernprogramme, allerdings sind t mit der ObjectDataSource zum Aktualisieren und Löschen von Daten. Wir sind stattdessen direkt gegen die BLL arbeiten. Um die Ausnahmen, die von der BLL oder DAL stammen erkennen zu können, müssen wir Ausnahmebehandlungscode in den Code-Behind unsere ASP.NET-Seite zu implementieren. In diesem Lernprogramm sehen wir mehr tactfully Behandeln von Ausnahmen, die während eines bearbeitbaren DataList s aktualisieren Workflows ausgelöst.

> [!NOTE]
> In der *eine Übersicht über die von bearbeiten und Löschen von Daten in das DataList* Lernprogramm verschiedene Techniken zum Bearbeiten und Löschen von Daten aus der DataList erörtert einige Techniken beteiligt mit einem ObjectDataSource zum Aktualisieren und wird gelöscht. Wenn Sie diese Verfahren einsetzen, können Sie behandeln von Ausnahmen von der BLL oder DAL über das ObjectDataSource-s `Updated` oder `Deleted` -Ereignishandler.


## <a name="step-1-creating-an-editable-datalist"></a>Schritt 1: Erstellen einer bearbeitbaren DataList

Bevor wir zur Behandlung von Ausnahmen, die während der Workflow für die Aktualisierung auftreten kümmern, können Sie s erstellen Sie zunächst eine bearbeitbare DataList. Öffnen der `ErrorHandling.aspx` auf der Seite der `EditDeleteDataList` Ordner hinzufügen, DataList in den Designer, legen Sie seine `ID` Eigenschaft `Products`, und fügen Sie eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse s `GetProducts()` Methode für die Auswahl zeichnet; legen Sie die Dropdownlisten in den INSERT-, Update-, und Löschen von Registerkarten (keine).


[![Zurückgeben der Produktinformationen mithilfe der GetProducts()-Methode](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Abbildung 1**: Zurückgeben von Informationen für das Produkt verwenden die `GetProducts()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Nach Abschluss des Assistenten ObjectDataSource Visual Studio erstellt automatisch ein `ItemTemplate` für DataList. Ersetzen Sie dies mit einer `ItemTemplate` , werden alle Produktnamen s und Preis angezeigt und enthält eine Schaltfläche "Bearbeiten". Als Nächstes erstellen Sie eine `EditItemTemplate` mit einem TextBox-Websteuerelement für Name "und" Price "und" Schaltflächen Aktualisieren und "Abbrechen". Legen Sie schließlich das DataList s `RepeatColumns` -Eigenschaft auf 2.

Nach der Änderung sollte Ihre deklarative s-Seitenmarkup etwa wie folgt aussehen. Überprüfen sicher, dass die Bearbeitung, "Abbrechen", und Update-Schaltflächen verfügen über ihre `CommandName` Eigenschaften auf "Abbrechen", bearbeiten und aktualisieren bzw. festgelegt.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Für dieses Lernprogramm DataList muss Ansichtszustand s aktiviert werden.


Können Sie unseren Fortschritt über einen Browser anzeigen (siehe Abbildung 2).


[![Jedes Produkt enthält eine Schaltfläche "Bearbeiten"](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Abbildung 2**: jedes Produkt enthält eine Schaltfläche zum Bearbeiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Die Schaltfläche "Bearbeiten" wird derzeit nur einen Postback er verfügt über t noch stellen das Produkt bearbeitet werden. Zum Aktivieren der Bearbeitung müssen wir Ereignishandler für das DataList s erstellen `EditCommand`, `CancelCommand`, und `UpdateCommand` Ereignisse. Die `EditCommand` und `CancelCommand` Ereignisse einfach aktualisieren, das DataList s `EditItemIndex` Eigenschaft und jedes öffentliche Seitenindex zu DataList Daten:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

Die `UpdateCommand` -Ereignishandler ist etwas komplizierter. Muss innerhalb des bearbeiteten Produkts s Lesen `ProductID` aus der `DataKeys` Auflistung zusammen mit den Produktnamen s und den Preis aus der Textfelder in der `EditItemTemplate`, und rufen Sie anschließend die `ProductsBLL` Klasse s `UpdateProduct` Methode vor der Rückgabe das DataList vorab Bearbeitung Datenbankzustands.

Jetzt verwenden lassen s nur genauen denselben Code aus der `UpdateCommand` -Ereignishandler in der *Übersicht von bearbeiten und Löschen von Daten in das DataList* Lernprogramm. Fügen wir den Code, um ordnungsgemäß Behandeln von Ausnahmen in Schritt 2 ein.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

Bei Eingabe ungültig sein in Form einer falsch formatierten Einzelpreis, eine ungültige Einheit Preis z. B.-$5,00 oder die Auslassung des Produkts s Namens ein, die eine Ausnahme ausgelöst wird. Da die `UpdateCommand` Ereignishandler schließt alle Ausnahmebehandlungscode an diesem Punkt nicht, bis zu die ASP.NET-Laufzeit die Ausnahme Blase wird, wo es für den Endbenutzer angezeigt werden (siehe Abbildung 3).


![Wenn eine nicht behandelte Ausnahme auftritt, wird der Endbenutzer eine Fehlerseite angezeigt.](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Abbildung 3**: Wenn eine nicht behandelte Ausnahme auftritt, sieht der Endbenutzer eine Fehlerseite


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Schritt 2: Ordnungsgemäß Behandeln von Ausnahmen in die UpdateCommand-Ereignishandler

Während der Aktualisierung Workflow können Ausnahmen auftreten, der `UpdateCommand` -Ereignishandler, die BLL oder der DAL. Z. B. wenn ein Benutzer mit einen Preis von Too eingibt viel Leistung beanspruchen, die `Decimal.Parse` -Anweisung in der `UpdateCommand` Ereignishandler löst eine `FormatException` Ausnahme. Wenn der Benutzer den Namen des Produkts s keine oder der Preis einen negativen Wert hat, wird die DAL eine Ausnahme ausgelöst.

Wenn eine Ausnahme auftritt, möchten wir eine informationsmeldung auf der Seite selbst anzeigen. Hinzufügen eines Web-Label-Steuerelement auf der Seite ", dessen `ID` auf festgelegt ist `ExceptionDetails`. Konfigurieren Sie den Bezeichnungstext s zum Anzeigen in einem roten, sehr groß ist, fett und kursiv Schriftart durch Zuweisen der `CssClass` Eigenschaft, um die `Warning` CSS-Klasse, die in definiert ist die `Styles.css` Datei.

Wenn ein Fehler auftritt, möchten wir nur die Bezeichnung, die einmal angezeigt werden. Bei nachfolgenden Postbacks sollte also die Bezeichnung s Warnmeldung verschwinden. Dazu entweder gelöscht wird, die Bezeichnung s `Text` Eigenschaft oder Einstellungen seiner `Visible` Eigenschaft, um `False` in der `Page_Load` -Ereignishandler (wie wir wieder in die [Behandlung von BLL- und DAL-Ebene Ausnahmen in einer ASP Seite .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) Lernprogramm) oder durch das Deaktivieren der Bezeichnung s Ansicht Status-Unterstützung. Lassen Sie s letztere Option zu verwenden.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Wenn eine Ausnahme ausgelöst wird, müssen wir weisen Sie die Details der Ausnahme, um die `ExceptionDetails` Label-Steuerelement s `Text` Eigenschaft. Da seinen Ansichtszustand, bei nachfolgenden Postbacks deaktiviert ist der `Text` Eigenschaft s programmgesteuerte Änderungen verloren, das Zurückkehren zur des Standardtexts (eine leere Zeichenfolge), dadurch wird die Warnmeldung ausgeblendet.

Um zu bestimmen, wann ein Fehler ausgelöst wurde um eine nützliche Nachricht auf der Seite anzuzeigen, müssen wir Hinzufügen einer `Try ... Catch` -block, um die `UpdateCommand` -Ereignishandler. Die `Try` Teil enthält Code, der zu einer Ausnahme führen könnte, während die `Catch` -Block enthält Code, der bei einer Ausnahme ausgeführt wird. Sehen Sie sich die [Exception Handling Fundamentals](https://msdn.microsoft.com/library/2w8f0bss.aspx) auf .NET Framework-Dokumentation weitere Informationen im Abschnitt der `Try ... Catch` Block.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Wenn eine Ausnahme eines beliebigen Typs ausgelöst wird, durch den Code der `Try` Block, der `Catch` -Blöcke Code Ausführung beginnt. Der Typ der Ausnahme, die ausgelöst wird, `DbException`, `NoNullAllowedException`, `ArgumentException`, und unmittelbar auf hängt, genau, den Fehler ursprünglich vorausging. Wenn dort s ein Problem auf Datenbankebene, eine `DbException` ausgelöst. Wenn für ein Unzulässiger Wert eingegeben wird die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, oder `ReorderLevel` Felder, ein `ArgumentException` ausgelöst, Hinzufügen von Code, um zu überprüfen, ob diese Feldwerte in der `ProductsDataTable` Klasse (finden Sie unter der [ Erstellen eine Geschäftslogikschicht](../introduction/creating-a-business-logic-layer-vb.md) Lernprogramm).

Wir können eine zusätzliche hilfreiche Erläuterung für den Endbenutzer bereitstellen, indem Sie den Meldungstext für den Typ der Ausnahme basieren. Der folgende Code, der in einem Formular fast identisch verwendet wurde wieder in die [Behandlung von BLL- und DAL-Ebene Ausnahmen auf einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) Lernprogramm bietet diese Detailebene:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Um dieses Lernprogramm abgeschlossen haben, rufen Sie einfach die `DisplayExceptionDetails` Methode aus der `Catch` Block übergeben wird, in das aufgefangene `Exception` Instanz (`ex`).

Mit der `Try ... Catch` festliegen blockieren, die Benutzer werden als Zahlen 4 und 5 anzeigen eine ausführlichere Fehlermeldung angezeigt. Beachten Sie, die bei einer Ausnahme DataList bleibt im Bearbeitungsmodus befindet. Dies ist, da nach dem Auftreten der Ausnahme, die ablaufsteuerung sofort an umgeleitet wird die `Catch` Block, umgehen den Code, der das DataList Datenbankzustands vorab Bearbeitung zurückgibt.


[![Eine Fehlermeldung wird angezeigt, wenn ein Benutzer ein Feld erforderlich wird ausgelassen.](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Abbildung 4**: eine Fehlermeldung wird angezeigt, wenn ein Benutzer ein Feld erforderlich, keine ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Eine Fehlermeldung wird angezeigt, wenn einen Preis negativ](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Abbildung 5**: eine Fehlermeldung wird angezeigt, wenn einen Preis negativ ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Zusammenfassung

Geben Sie die GridView und ObjectDataSource nach der Ebene Ereignishandler, die enthalten Informationen über alle Ausnahmen, die während des aktualisieren und Löschen von Workflows ausgelöst wurden, sowie Eigenschaften, die festgelegt werden können, um anzugeben, und zwar unabhängig davon, ob die Ausnahme wurde behandelt. Diese Funktionen sind jedoch nicht verfügbar beim Arbeiten mit DataList und die BLL direkt verwenden. Stattdessen werden für die Implementierung der Ausnahmebehandlung verantwortlich.

In diesem Lernprogramm wurde erläutert, wie eine bearbeitbare DataList s Aktualisieren des Workflows durch Hinzufügen von Ausnahmebehandlung ergänzen ein `Try ... Catch` -block, um die `UpdateCommand` -Ereignishandler. Wenn eine Ausnahme, während der Workflow für die Aktualisierung ausgelöst wird, die `Catch` -Blöcke Code ausführt, Anzeigen von nützliche Informationen in den `ExceptionDetails` Bezeichnung.

An diesem Punkt ermittelt DataList nicht zu verhindern, dass Ausnahmen von vornherein geschieht. Obwohl wir wissen, dass ein negativer Preis führt zu einer Ausnahme, die es unklar hinzugefügt t noch keine Funktionen zur proaktiv einen Benutzer verhindern, dass solche ungültige Eingabe Eingabe. In unserem nächsten Lernprogramm sehen wir, wie die Ausnahmen, die durch ungültige Benutzereingaben verursacht werden, durch Hinzufügen von Steuerelementen für die Validierung in Minimieren der `EditItemTemplate`.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Entwurfsrichtlinien für Ausnahmen](https://msdn.microsoft.com/library/ms298399.aspx)
- [Error Logging Modules und Ereignishandler (ELMAH)](http://workspaces.gotdotnet.com/elmah) (eine Open Source-Bibliothek für die Protokollierung von Fehlern)
- [Enterprise Library für .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (einschließlich der Ausnahme Management Application Block)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Ken Pespisa. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](performing-batch-updates-vb.md)
> [Weiter](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
