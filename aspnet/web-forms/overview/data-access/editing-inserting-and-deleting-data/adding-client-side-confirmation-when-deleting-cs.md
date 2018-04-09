---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Hinzufügen von clientseitigen Bestätigung beim Löschen von (c#) | Microsoft Docs
author: rick-anderson
description: In den Schnittstellen, die wir bisher erstellt haben, kann ein Benutzer versehentlich löschen von Daten durch Klicken auf die Schaltfläche "löschen", wenn sie auf die Schaltfläche "Bearbeiten" klicken. In diesem t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 72b15d498e45cc519a14ecfe39111b224db88c30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Hinzufügen von clientseitigen Bestätigung beim Löschen von (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) oder [PDF herunterladen](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> In den Schnittstellen, die wir bisher erstellt haben, kann ein Benutzer versehentlich löschen von Daten durch Klicken auf die Schaltfläche "löschen", wenn sie auf die Schaltfläche "Bearbeiten" klicken. In diesem Lernprogramm fügen wir eine clientseitige Bestätigungsdialogfeld angezeigt wurde, der angezeigt wird, wenn auf die Schaltfläche "löschen" geklickt wird.


## <a name="introduction"></a>Einführung

Über die letzten mehrere Lernprogramme wir haben gesehen, wie unsere Anwendungsarchitektur ObjectDataSource und die Daten Websteuerelementen gemeinsam verwenden, um einfügen, bearbeiten und Löschen von Funktionen bereitzustellen. Das Löschen von Schnittstellen wir untersucht haben bisher wurden besteht aus einer Delete-Schaltfläche geklickt wird, bewirkt ein Postback aus, und ruft das ObjectDataSource-s `Delete()` Methode. Die `Delete()` Methode ruft dann die konfigurierte Methode aus der Business Logic Layer, in dem den Aufruf an die Datenzugriffsebene, die die tatsächliche ausstellen verteilt `DELETE` Anweisung an die Datenbank.

Während dieser Benutzeroberfläche Besucher zum Löschen von Datensätzen durch die GridView, DetailsView oder FormView-Steuerelemente kann, verfügt nicht über sie jegliche Art von Bestätigung klickt der Benutzer die Schaltfläche "löschen". Wenn ein Benutzer versehentlich klickt bearbeiten die Schaltfläche "löschen", wenn sie auf vorgesehen, der Datensatz, den sie zum Aktualisieren vorgesehen ist, wird stattdessen gelöscht werden. Um dies zu vermeiden, in diesem Lernprogramm wir ein Bestätigungsdialogfeld mit einer clientseitigen hinzufügen, der angezeigt wird, wenn auf die Schaltfläche "löschen" geklickt wird.

Der JavaScript-Code `confirm(string)` Funktion zeigt die Zeichenfolgen-Eingabeparameter an, wie der Text in ein modales Dialogfeld an, die zum Lieferumfang zwei Schaltflächen - OK und "Abbrechen" (siehe Abbildung 1). Die `confirm(string)` Funktion gibt einen booleschen Wert je nachdem, welche Schaltfläche geklickt wird (`true`, wenn der Benutzer auf OK klickt und `false` , wenn sie auf "Abbrechen" klicken).


![Die JavaScript-confirm(string) Methode zeigt ein modales clientseitige Messagebox](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Abbildung 1**: JavaScript `confirm(string)` Methode zeigt ein modales, clientseitige Messagebox an.


Während einer Formularübergabe, wenn ein Wert von `false` aus ein clientseitiges Ereignis-Handler zurückgegeben wird, und klicken Sie dann der Übermittlung des Formulars abgebrochen wird. Mit dieser Funktion können wir die Delete Schaltfläche s die clientseitige haben `onclick` -Ereignishandler der Rückgabewert eines Aufrufs von `confirm("Are you sure you want to delete this product?")`. Wenn der Benutzer auf "Abbrechen", klickt `confirm(string)` "false", wodurch verursacht der Übermittlung des Formulars auf "Abbrechen" zurück. Kein Postback werden des Produkts, dessen Schaltfläche "löschen" geklickt wurde, die/t gewonnen gelöscht. Wenn jedoch der Benutzer in das Dialogfeld zur Bestätigung auf OK klickt, wird das Postback ungehindert und das Produkt gelöscht werden. Wenden Sie sich an [Verwenden von JavaScript s `confirm()` Methode, um das Steuerelement Formularübermittlung](http://www.webreference.com/programming/javascript/confirm/) für Weitere Informationen zu dieser Technik.

Hinzufügen der erforderlichen clientseitigem Skripts unterscheidet sich geringfügig auf wenn Vorlagen als bei Verwendung einer CommandField verwenden. Aus diesem Grund wird in diesem Lernprogramm eine FormView und die GridView-Beispiel betrachten wir.

> [!NOTE]
> Verwenden die clientseitige Bestätigung Techniken, wie die in diesem Lernprogramm behandelten davon ausgegangen, dass Ihre Benutzer mit Browsern besuchen, die JavaScript-Unterstützung, dass JavaScript aktiviert ist. Wenn entweder diese Annahmen nicht "true" für einen bestimmten Benutzer sind, wird Sie auf die Schaltfläche "löschen" sofort einen Postback (nicht bestätigen Messagebox angezeigt) verursachen.


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Schritt 1: Erstellen einen FormView, unterstützt löschen

Durch hinzufügen einen FormView zum Starten der `ConfirmationOnDelete.aspx` auf der Seite der `EditInsertDelete` Ordner, binden es an eine neue ObjectDataSource, das die Produktinformationen über wieder abruft der `ProductsBLL` Klasse s `GetProducts()` Methode. Auch konfigurieren, das ObjectDataSource, damit der `ProductsBLL` Klasse s `DeleteProduct(productID)` -Methode zugeordnet ist, mit dem ObjectDataSource s `Delete()` Methode; stellen Sie sicher, dass die INSERT- und UPDATE-Registerkarten Dropdownlisten (keine) auf. Zum Schluss das Kontrollkästchen Sie Paging aktivieren im FormView s Smarttag.

Nach diesen Schritten wird die neue ObjectDataSource s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Nehmen Sie wie in diesen früheren Beispielen, die nicht die vollständigen Parallelität verwendet wurde, einen Moment Zeit, zu löschen, das ObjectDataSource-s `OldValuesParameterFormatString` Eigenschaft.

Da es an einem ObjectDataSource-Steuerelement, die unterstützt nur löschen gebunden wurde, die FormView s `ItemTemplate` bietet nur die Schaltfläche "löschen", fehlen die Schaltflächen "Neu" und "Update". Die FormView s deklarative Markup, enthält jedoch eine überflüssige `EditItemTemplate` und `InsertItemTemplate`, die entfernt werden kann. Erkundet zum Anpassen der `ItemTemplate` also, d. h. zeigt nur eine Teilmenge des Produkts Datenfelder. Ich Ve konfiguriert extrahieren zum Anzeigen der Produktname s in einer `<h3>` Überschrift über seinen Namen "Supplier" und "Category" (zusammen mit der Schaltfläche "löschen").


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Durch diese Änderungen haben wir eine voll funktionsfähige Webseite, die einem Benutzer ermöglicht, über die Produkte eine gleichzeitig die Möglichkeit, ein Produkt zu löschen, indem Sie einfach die Schaltfläche "löschen" umschalten. Abbildung 2 zeigt einen Screenshot der Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Die FormView zeigt Informationen zu einem einzigen Produkt](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Abbildung 2**: die FormView zeigt Informationen über ein einzelnes Produkt ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Schritt 2: Aufrufen der confirm(string)-Funktion aus der löschen Schaltflächen clientseitige Onclick-Ereignis

Die FormView erstellt wird, ist der letzte Schritt so konfigurieren Sie die Schaltfläche "löschen" solche, die bei es s geklickt wird, indem Sie den Besucher, der JavaScript-Code `confirm(string)` Funktion wird aufgerufen. Hinzufügen von clientseitigem Skript auf eine Schaltfläche, LinkButton oder ImageButton s clientseitige `onclick` Ereignis kann durch die Verwendung der umgesetzt werden die `OnClientClick property`, neu in ASP.NET 2.0. Da wir den Wert der haben möchten die `confirm(string)` -Funktion zurückgegeben hat, einfach legen Sie diese Eigenschaft: `return confirm('Are you certain that you want to delete this product?');`

Nach dieser Änderung sollte die LinkButton löschen s deklarative Syntax aussehen:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

S alles ist es! Abbildung 3 zeigt einen Screenshot, der durch diese Bestätigung in Aktion an. Durch Klicken auf die Schaltfläche "löschen" wird im Dialogfeld bestätigen. Klickt der Benutzer auf "Abbrechen", das Postback wird abgebrochen, und das Produkt wird nicht gelöscht. Wenn jedoch der Benutzer auf OK klickt, wird das Postback fortgesetzt und das ObjectDataSource-s `Delete()` Methode aufgerufen wird, mit dem Datenbank-Datensatz gelöscht wird.

> [!NOTE]
> Übergeben Sie die Zeichenfolge in der `confirm(string)` JavaScript-Funktion mit Apostrophe (statt Anführungszeichen) begrenzt wird. In JavaScript können Zeichenfolgen mit beiden Zeichen getrennt werden. Wir verwenden Apostrophe hier, sodass die Trennzeichen für die Zeichenfolge übergeben `confirm(string)` führen eine Mehrdeutigkeit mit dem Standardtrennzeichen für die `OnClientClick` Eigenschaftswert.


[![Eine Bestätigung wird nun angezeigt, wenn durch Klicken auf die Schaltfläche "löschen"](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Abbildung 3**: eine Bestätigung wird nun angezeigt, wenn durch Klicken auf die Schaltfläche "löschen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Schritt 3: Konfigurieren der OnClientClick-Eigenschaft für die Schaltfläche "löschen" in eine CommandField

Bei der Arbeit mit einer Schaltfläche, LinkButton oder ImageButton direkt in eine Vorlage ein Bestätigungsdialogfeld kann zugeordnet werden sie durch einfach konfigurieren die `OnClientClick` -Eigenschaft zum Zurückgeben der Ergebnisse des JavaScripts `confirm(string)` Funktion. Die CommandField - wodurch ein Feld von Schaltflächen zum Löschen einer GridView oder DetailsView hinzugefügt - verfügt jedoch nicht über ein `OnClientClick` Eigenschaft, deklarativ festgelegt werden kann. Verweisen Sie stattdessen wir müssen programmgesteuert auf die Schaltfläche "löschen" in der entsprechenden GridView oder DetailsView s `DataBound` -Ereignishandler, und legen dessen `OnClientClick` Eigenschaft vorhanden.

> [!NOTE]
> Beim Festlegen der Schaltfläche "löschen" s `OnClientClick` Eigenschaft in der entsprechenden `DataBound` Ereignishandler, wir haben Zugriff auf die Daten auf den aktuellen Datensatz gebunden wurde. Dies bedeutet, dass wir erweitern können im Meldungsfeld zur Bestätigung auf Details zu den entsprechenden Datensatz enthalten, z. B. "Sind Sie sicher, dass Sie das Produkt Chai löschen möchten?" Diese Anpassung ist auch möglich, in den Vorlagen Databinding-Syntax verwenden.


Übung Einstellung der `OnClientClick` -Eigenschaft für die Delete-Tasten in einem CommandField, Let s eine GridView zur Seite hinzufügen. Konfigurieren Sie diese GridView Verwendung FormView verwendet die gleiche ObjectDataSource-Steuerelement. Auch einschränken der GridView s BoundFields, um nur die s-Produktname, Kategorie und Lieferanten einzuschließen. Abschließend wird das Kontrollkästchen Sie löschen aktivieren aus dem GridView-s-Smarttag. Dadurch wird eine CommandField hinzugefügt, mit dem GridView s `Columns` Auflistung mit seiner `ShowDeleteButton` -Eigenschaftensatz auf `true`.

Nachdem diese Änderungen vorgenommen wurden, sollte Ihre GridView s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

Die CommandField enthält eine einzelne löschen LinkButton-Instanz, die aus den s GridView programmgesteuert zugegriffen werden kann `RowDataBound` -Ereignishandler. Nachdem Sie auf die verwiesen wird, können wir Festlegen seiner `OnClientClick` Eigenschaft entsprechend. Erstellen Sie einen Ereignishandler für das `RowDataBound` Ereignis mit dem folgenden Code:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Dieser Ereignishandler arbeitet mit Datenzeilen (diejenigen, für die Schaltfläche "löschen") und durch Programmgesteuertes Verweisen auf die Schaltfläche "löschen" beginnt. Verwenden Sie im Allgemeinen das folgende Muster:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* ist der Typ der Schaltfläche von CommandField - Schaltfläche, LinkButton oder ImageButton verwendet wird. Standardmäßig verwendet die CommandField LinkButtons, aber dies kann angepasst werden, über die CommandField s `ButtonType property`. Die *CommandFieldIndex* ist der Ordinalindex der CommandField innerhalb der GridView s `Columns` Sammlung, während die *ControlIndex* ist der Index der Schaltfläche "löschen" in der CommandField s `Controls` Auflistung. Die *ControlIndex* Wert hängt von der Schaltfläche s Position relativ zu anderen Schaltflächen in der CommandField. Beispielsweise ist die Schaltfläche nur angezeigt, in der CommandField auf die Schaltfläche "löschen", verwenden Sie einen Index 0. Wenn Sie jedoch dort s eine Schaltfläche "Bearbeiten", der die Schaltfläche "löschen" vorausgeht Index verwenden 2. Der Grund, ein Index 2 verwendet wird, ist, da zwei Steuerelementen von der CommandField, bevor Sie die Schaltfläche "löschen" hinzugefügt werden: die Schaltfläche "Bearbeiten" und ein LiteralControl, s, zum Hinzufügen von Leerzeichen zwischen den Schaltflächen Bearbeiten und löschen.

In unserem Beispiel bestimmten die CommandField LinkButtons verwendet und wird das linke Feld verfügt über eine *CommandFieldIndex* 0. Da es sich um keine anderen Schaltflächen, aber die Schaltfläche "löschen" in der CommandField, verwenden wir eine *ControlIndex* 0.

Nach Verweisen auf die Schaltfläche "löschen" in der CommandField, nehmen wir als Nächstes Informationen über das Produkt mit der aktuellen Zeile des GridView gebunden. Wir legen Sie schließlich die Schaltfläche "löschen" s `OnClientClick` Eigenschaft für das entsprechende JavaScript, der den Produktnamen s enthält. Da das JavaScript-Zeichenfolge übergeben der `confirm(string)` Funktion begrenzt wird, müssen wir mit Escapezeichen versehen, die innerhalb der Produktname s Apostrophe, Apostrophe verwenden. Insbesondere Apostrophe im Produktnamen s werden mit Escapezeichen versehen "`\'`".

Abgeschlossen Sie mit diesen Änderungen haben, klicken auf eine Schaltfläche "löschen" im GridView zeigt eine benutzerdefinierte Bestätigung Dialogfeld (siehe Abbildung 4). Als mit der Messagebox Bestätigung aus der FormView klickt der Benutzer auf "Abbrechen" das Postback abgebrochen den Löschvorgang aus, um zu verhindern.

> [!NOTE]
> Diese Technik kann auch verwendet werden, auf die Schaltfläche "löschen" in der CommandField in einem DetailsView programmgesteuert zugreifen. Für DetailsView, allerdings d Sie erstellen einen Ereignishandler für das `DataBound` Ereignis, DetailsView weist kein `RowDataBound` Ereignis.


[![Klicken auf die Schaltfläche "GridView s löschen" zeigt eine angepasste Bestätigungsdialogfeld an.](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Abbildung 4**: durch Klicken auf die GridView-s-Schaltfläche "löschen" wird ein Bestätigungsdialogfeld angepasst ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Verwenden von TemplateFields

Einer der Nachteile der CommandField ist, dass die Schaltflächen über der Indizierung zugegriffen werden müssen und dass das resultierende Objekt in der entsprechenden Schaltflächentyp (Schaltfläche, LinkButton oder ImageButton) umgewandelt werden muss. "Magische Zahlen" mit hartcodierten Typen ist problematisch, die bis zur Laufzeit ermittelt werden können. Beispielsweise, wenn Sie oder ein anderer Entwickler die CommandField an einem Zeitpunkt in der Zukunft (z. B. eine Schaltfläche "Bearbeiten") oder Änderungen neue Schaltflächen hinzugefügt der `ButtonType` -Eigenschaft, der vorhandene Code weiterhin ohne Fehler kompiliert wird, besuchen die Seite verursacht jedoch möglicherweise eine Ausnahme oder unerwartetem Verhalten führen, je nachdem wie Code geschrieben wurde und welche Änderungen vorgenommen wurden.

Ein alternativer Ansatz besteht darin, die GridView und DetailsView s CommandFields in von TemplateFields zu konvertieren. Dadurch wird ein TemplateField mit einer `ItemTemplate` , die eine LinkButton (oder Schaltfläche oder ImageButton) für jede Schaltfläche auf der CommandField hat. Diese Schaltflächen `OnClientClick` Eigenschaften können deklarativ zugewiesen werden, wie wir gesehen, mit dem FormView haben oder programmgesteuert werden, in der entsprechenden zugegriffen kann `DataBound` Ereignishandler mithilfe des folgenden Musters:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Wobei *ControlID* ist der Wert der Schaltfläche s `ID` Eigenschaft. Während dieses Muster immer noch einen fest programmiertes-Typ für die Umwandlung erforderlich ist, erübrigt es für die Indizierung, es möglich, dass das Layout ändern, ohne dass es zu einem Laufzeitfehler.

## <a name="summary"></a>Zusammenfassung

Der JavaScript-Code `confirm(string)` Funktion ist eine häufig verwendete Technik zum Steuern des Formulars Übermittlung Workflow. Bei der Ausführung zeigt die Funktion ein modales, clientseitige Dialogfeld, das zwei Schaltflächen "OK" und "Abbrechen" enthält. Wenn der Benutzer auf OK klickt der `confirm(string)` -Funktion zurückgegeben wird `true`; Wenn Sie auf "Abbrechen" `false`. Diese Funktion mit einem s Browserverhalten eine Formularübergabe Abbrechen, wenn ein Ereignishandler während der Übertragung zurückgibt gekoppelt `false`, eine Bestätigung Messagebox angezeigt, wenn Sie einen Datensatz löschen verwendet werden können.

Die `confirm(string)` Funktion kann eine Schaltfläche Web Control s die clientseitige zugeordnet werden `onclick` -Ereignishandler durch Steuerung s `OnClientClick` Eigenschaft. Bei der Arbeit mit einer Schaltfläche "löschen" in einer Vorlage – entweder in eine der Vorlagen FormView s oder in ein TemplateField, DetailsView oder GridView - kann diese Eigenschaft festgelegt werden deklarativ oder programmgesteuert, wie in diesem Lernprogramm wurde erläutert.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](implementing-optimistic-concurrency-cs.md)
> [Weiter](limiting-data-modification-functionality-based-on-the-user-cs.md)
