---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Hinzufügen von clientseitiger Bestätigung beim Löschen von (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In den Schnittstellen, die wir bisher erstellt haben, kann ein Benutzer versehentlich löschen von Daten durch Klicken auf die Schaltfläche "löschen", wenn sie auf die Schaltfläche "Bearbeiten" klicken, bestimmt. In diesen Typ t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: b8cca6ece2eb008170192dc1774a6f88c1b37a21
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835336"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Hinzufügen von clientseitiger Bestätigung beim Löschen von (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) oder [PDF-Datei herunterladen](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> In den Schnittstellen, die wir bisher erstellt haben, kann ein Benutzer versehentlich löschen von Daten durch Klicken auf die Schaltfläche "löschen", wenn sie auf die Schaltfläche "Bearbeiten" klicken, bestimmt. In diesem Tutorial fügen wir ein Dialogfeld von clientseitiger Bestätigung, die angezeigt wird, wenn die Schaltfläche Löschen geklickt wird.


## <a name="introduction"></a>Einführung

Über die letzten mehrere Lernprogramme wir haben gesehen, wie Sie unsere Anwendungsarchitektur, "ObjectDataSource" und die Daten Websteuerelemente gemeinsam zum Bereitstellen von einfügen, bearbeiten und Löschen von Funktionen zu verwenden. Das Löschen von Schnittstellen wir untersucht haben bisher war ein Löschvorgang aus-Schaltfläche geklickt wird, führt dazu, dass einen Postback aus, und ruft die "ObjectDataSource"-s `Delete()` Methode. Die `Delete()` Methode ruft dann die konfigurierten-Methode aus der Business Logic Layer, dem den Aufruf an die Datenzugriffsebene, die die tatsächliche Ausgabe überträgt `DELETE` Anweisung an die Datenbank.

Während dieser Benutzeroberfläche Besucher zum Löschen von Datensätzen durch die GridView, DetailsView oder FormView-Steuerelemente ermöglicht, fehlt ein beliebiges Bestätigung klickt der Benutzer die Schaltfläche "löschen". Wenn ein Benutzer versehentlich klickt bearbeiten Sie die Schaltfläche "löschen", wenn sie klicken sollen, wird der Datensatz, den sie aktualisieren soll stattdessen gelöscht. Damit dies vermieden werden in diesem Tutorial fügen wir ein Dialogfeld von clientseitiger Bestätigung, die angezeigt wird, wenn die Schaltfläche Löschen geklickt wird.

Das JavaScript `confirm(string)` Funktion zeigt die Zeichenfolgen-Eingabeparameter an, wie der Text innerhalb eines modalen Dialogfelds, das ist gut mit zwei Schaltflächen - ausgestattet und Abbrechen (siehe Abbildung 1). Die `confirm(string)` Funktionsergebnis ist einen booleschen Wert, je nachdem, welche Schaltfläche geklickt wird (`true`, wenn der Benutzer auf OK klickt und `false` Wenn sie auf "Abbrechen" klicken).


![Die JavaScript-confirm(string) Methode zeigt ein modales, Client-Side-Messagebox](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Abbildung 1**: JavaScript `confirm(string)` Methode zeigt ein modales, clientseitige Messagebox an.


Während einer Formularübergabe, wenn ein Wert von `false` von einem clientseitigen Ereignis-Handler zurückgegeben wird, und klicken Sie dann die Übermittlung des Formulars abgebrochen wird. Mit dieser Funktion können wir haben der löschen-Schaltfläche "," s-Clientseite `onclick` -Ereignishandler den Rückgabewert eines Aufrufs von `confirm("Are you sure you want to delete this product?")`. Wenn der Benutzer auf "Abbrechen", klickt `confirm(string)` gibt "false", wodurch verursacht die Übermittlung des Formulars zum abzubrechen. Kein Postback werden das Produkt, dessen löschen-Schaltfläche geklickt wurde, die gewonnen haben t gelöscht. Wenn jedoch der Benutzer in das Dialogfeld zur Bestätigung auf OK klickt, wird das Postback ungehindert und das Produkt gelöscht werden. Wenden Sie sich an [Verwenden von JavaScript-s `confirm()` Methode, um die Formularübermittlung Steuerelement](http://www.webreference.com/programming/javascript/confirm/) für Weitere Informationen zu dieser Technik.

Hinzufügen der erforderlichen Client-seitige Skript unterscheidet sich geringfügig bei Verwendung von Vorlagen als bei der Verwendung einer CommandField. Aus diesem Grund wird in diesem Tutorial sowohl einen FormView und GridView-Beispiel betrachten wir.

> [!NOTE]
> Mit clientseitiger Bestätigung Techniken wird vorausgesetzt, dass wie die in diesem Tutorial erläutert, dass Ihre Benutzer mit Browsern besuchen, die JavaScript-Unterstützung, dass JavaScript aktiviert ist. Wenn eine der Annahmen nicht "true" für einen bestimmten Benutzer sind, wird Sie auf die Schaltfläche "löschen" sofort einen Postback (nicht angezeigt. eine Messagebox bestätigen) führen.


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Schritt 1: Erstellen von einem FormView-Steuerelement, unterstützt löschen

Starten, indem einem FormView-Steuerelement, Hinzufügen der `ConfirmationOnDelete.aspx` auf der Seite die `EditInsertDelete` Ordner Binden an eine neue "ObjectDataSource", die die Produktinformationen über wieder bezieht die `ProductsBLL` s-Klasse `GetProducts()` Methode. Auch dem ObjectDataSource-Steuerelement konfigurieren, damit die `ProductsBLL` s-Klasse `DeleteProduct(productID)` -Methode zugeordnet ist, der "ObjectDataSource"-s `Delete()` Methode stellen Sie sicher, dass die INSERT- und UPDATE-Registerkarten, Dropdownlisten auf (keine) festgelegt sind. Überprüfen Sie abschließend das Kontrollkästchen Paging aktivieren, in das FormView-s-Smarttag.

Nach diesen Schritten wird die neue "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Wie in unseren früheren Beispielen, die nicht die vollständigen Parallelität verwendet haben, können Sie das "ObjectDataSource"-s löschen `OldValuesParameterFormatString` Eigenschaft.

Da es an ein ObjectDataSource-Steuerelement, das nur löschen gebunden wurde unterstützt "," der FormView-s `ItemTemplate` bietet nur die Schaltfläche "löschen", fehlt der Schaltflächen "Neu" und "Update". Die FormView s deklarative Markup, enthält jedoch eine überflüssige `EditItemTemplate` und `InsertItemTemplate`, die entfernt werden kann. Anpassen in Ruhe die `ItemTemplate` also d.h. zeigt nur eine Teilmenge des Produkts Datenfelder. Ich Ve konfiguriert Meine zum Anzeigen der Produktnamen s in eine `<h3>` Überschrift über seine Namen "Supplier" und "Category" (zusammen mit der Schaltfläche "löschen").


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Mit diesen Änderungen haben wir eine voll funktionsfähige Webseite, die einem Benutzer ermöglicht, über die Produkte einer gleichzeitig die Möglichkeit, ein Produkt zu löschen, indem Sie einfach die Schaltfläche "löschen" umschalten. Abbildung 2 zeigt einen Screenshot des unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Die FormView-Steuerelement zeigt Informationen zu einem einzigen Produkt](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Abbildung 2**: das FormView-Steuerelement zeigt Informationen über ein einzelnes Produkt ([klicken Sie, um das Bild in voller Größe anzeigen](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Schritt 2: Aufrufen der Funktion confirm(string) über das Löschen von Schaltflächen der clientseitigen Onclick-Ereignis

Mit der FormView-Steuerelement erstellt, der der letzte Schritt ist so konfigurieren Sie die Schaltfläche "löschen" solche, die bei es s geklickt wird, um dem Besucher angezeigt, den JavaScript-Code `confirm(string)` Funktion wird aufgerufen. Hinzufügen des clientseitigen Skripts auf eine Schaltfläche, LinkButton oder ImageButton s clientseitige `onclick` Ereignis kann erreicht werden, mithilfe des der `OnClientClick property`, das ist neu in ASP.NET 2.0. Da wir den Wert des möchten der `confirm(string)` -Funktion zurückgegeben hat, legen Sie einfach diese Eigenschaft auf: `return confirm('Are you certain that you want to delete this product?');`

Nach dieser Änderung sollte die deklarative Syntax des löschen LinkButton-s wie folgt aussehen:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Alles, was, s wird es! Abbildung 3 zeigt einen Screenshot des dieser Bestätigung in Aktion. Durch Klicken auf die Schaltfläche "löschen" wird im Dialogfeld bestätigen. Wenn der Benutzer auf "Abbrechen" klickt, das Postback abgebrochen, und das Produkt wird nicht gelöscht. Wenn Sie jedoch die Benutzer auf OK klickt, wird das Postback fortgesetzt und das "ObjectDataSource"-s `Delete()` Methode aufgerufen wird, verbessert den Datenbank-Datensatz gelöscht wird.

> [!NOTE]
> Die übergebene Zeichenfolge in die `confirm(string)` JavaScript-Funktion mit Apostrophe (und nicht mit Anführungszeichen) als Trennzeichen dient. In JavaScript können Zeichenfolgen mithilfe der beiden Zeichen getrennt werden. Wir verwenden Apostrophe hier, sodass die Trennzeichen für die Zeichenfolge übergeben `confirm(string)` führen eine Mehrdeutigkeit mit Trennzeichen verwendet werden, für die `OnClientClick` Eigenschaftswert.


[![Eine Bestätigung wird jetzt angezeigt, wenn durch Klicken auf die Schaltfläche "löschen"](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Abbildung 3**: eine Bestätigung wird jetzt angezeigt, wenn durch Klicken auf die Schaltfläche "löschen" ([klicken Sie, um das Bild in voller Größe anzeigen](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Schritt 3: Konfigurieren der OnClientClick-Eigenschaft für die Löschen-Schaltfläche in einem CommandField

Bei der Arbeit mit einer Schaltfläche, LinkButton oder ImageButton direkt in eine Vorlage ein Bestätigungsdialogfeld kann zugeordnet werden es einfach konfigurieren die `OnClientClick` Eigenschaft, um den JavaScript-Code zurückgeben, `confirm(string)` Funktion. Die CommandField - Dadurch wird ein Feld von Schaltflächen zum Löschen einem GridView- oder DetailsView hinzugefügt – verfügt jedoch nicht über eine `OnClientClick` -Eigenschaft, die deklarativ festgelegt werden kann. Verweisen Sie stattdessen wir müssen programmgesteuert auf die Schaltfläche "löschen" in den entsprechenden GridView- oder DetailsView s `DataBound` -Ereignishandler, und legen dessen `OnClientClick` Eigenschaft vorhanden.

> [!NOTE]
> Beim Festlegen der Schaltfläche "löschen" s `OnClientClick` Eigenschaft in den entsprechenden `DataBound` -Ereignishandler haben Zugriff auf die Daten auf den aktuellen Datensatz gebunden wurde. Dies bedeutet, dass wir die bestätigungsmeldung angezeigt, um Details zu den entsprechenden Datensatz aus, z. B. einzuschließen, "Sind Sie sicher, dass Sie das Produkt Chai löschen möchten?" erweitern können Eine solche Anpassung kann auch in Vorlagen, die mithilfe der Datenbindungssyntax.


Einstellung der Methode die `OnClientClick` -Eigenschaft für die Delete-Tasten in einem CommandField, Let s Hinzufügen einer GridView-Ansicht auf der Seite. Konfigurieren Sie diese GridView Verwendung derselbe ObjectDataSource-Steuerelement, das das FormView-Steuerelement verwendet. Auch einschränken Sie, die GridView s BoundFields sollen nur die Product-s-Name, Kategorie und Lieferanten. Schließlich das Kontrollkästchen Sie aktivieren Sie das Löschen aus dem GridView-s-Smarttag. Dadurch wird eine CommandField hinzugefügt, der GridView-s `Columns` Sammlung mit der `ShowDeleteButton` -Eigenschaftensatz auf `true`.

Nach diesen Änderungen sollte Ihre GridView s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

Die CommandField enthält eine einzelne löschen LinkButton-Instanz, die von der GridView-s programmgesteuert zugegriffen werden kann `RowDataBound` -Ereignishandler. Nachdem Sie auf die verwiesen wird, können wir Festlegen seiner `OnClientClick` Eigenschaft entsprechend. Erstellen Sie einen Ereignishandler für die `RowDataBound` Ereignis mit dem folgenden Code:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Dieser Ereignishandler arbeitet mit Datenzeilen (diejenigen, die die Schaltfläche "löschen" hat) und durch Programmgesteuertes Verweisen auf die Schaltfläche "löschen" beginnt. Verwenden Sie im Allgemeinen das folgende Muster:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* ist der Typ der Schaltfläche, die von der CommandField - Schaltfläche, LinkButton oder ImageButton verwendet wird. Standardmäßig verwendet die CommandField LinkButtons, aber dies kann angepasst werden, über die CommandField s `ButtonType property`. Die *CommandFieldIndex* ist der Ordinalindex des der CommandField in den GridView-s `Columns` Sammlung, während die *ControlIndex* ist der Schnittstellenindex der Schaltfläche "löschen" in der CommandField s `Controls` Auflistung. Die *ControlIndex* Wert hängt von der Schaltfläche "s" Position relativ zu anderen Schaltflächen in der CommandField. Beispielsweise ist die einzige Schaltfläche in der CommandField angezeigt auf die Schaltfläche "löschen", verwenden Sie einen Index 0. Wenn Sie jedoch, gibt es s eine Schaltfläche "Bearbeiten", das die Schaltfläche "löschen" vorangestellt ist einen Index der-verwenden 2. Der Grund, ein Index von 2 verwendet wird, ist, da zwei Steuerelemente, durch die CommandField, bevor Sie die Schaltfläche "löschen hinzugefügt werden": die Schaltfläche "Bearbeiten" und ein LiteralControl, s, die mit der ein Leerzeichen zwischen den Schaltflächen Bearbeiten und löschen hinzugefügt.

Für unser Beispiel der CommandField LinkButtons verwendet und wird das Feld ganz links besitzt eine *CommandFieldIndex* 0. Da es sich um keine anderen Schaltflächen, aber die Löschen-Schaltfläche in der CommandField, verwenden wir eine *ControlIndex* 0.

Nach Verweisen auf die Schaltfläche "löschen" in der CommandField, nehmen wir als Nächstes Informationen über das Produkt mit der aktuellen Zeile mit GridView gebunden. Wir legen Sie abschließend die Schaltfläche "löschen" s `OnClientClick` Eigenschaft für das entsprechende JavaScript, der den Produktnamen s enthält. Da die JavaScript-Zeichenfolge übergeben die `confirm(string)` Funktion begrenzt wird, Apostrophe, wir müssen mit Escapezeichen versehen, die innerhalb der Produktnamen s bereits, verwenden. Insbesondere bereits im Produktnamen s werden mit Escapezeichen versehen "`\'`".

Abgeschlossen Sie mit diesen Änderungen haben, klicken auf eine Schaltfläche "löschen" im GridView zeigt eine benutzerdefinierte Bestätigung Dialogfeld (siehe Abbildung 4). Als mit der Messagebox bestätigen von FormView, wird Wenn der Benutzer auf "Abbrechen" klickt des Postbacks abgebrochen und verhindert den Löschvorgang zu.

> [!NOTE]
> Diese Technik kann auch verwendet werden, programmgesteuert auf die Schaltfläche "löschen" in der CommandField in einem DetailsView zugreifen. Für die DetailsView, Sie jedoch d erstellen einen Ereignishandler für die `DataBound` Ereignisses, weil die DetailsView keine `RowDataBound` Ereignis.


[![Klicken Sie auf die Schaltfläche zum Löschen von GridView-s zeigt ein benutzerdefiniertes Bestätigungsdialogfeld](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Abbildung 4**: das GridView-s-Schaltfläche "löschen" klicken, zeigt ein Bestätigungsdialogfeld angepasst ([klicken Sie, um das Bild in voller Größe anzeigen](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Verwenden von TemplateFields

Einer der Nachteile der CommandField ist, dass die Schaltflächen über der Indizierung zugegriffen werden müssen und das resultierende Objekt in die entsprechende Schaltfläche-Typ (Schaltfläche, LinkButton oder ImageButton) umgewandelt werden muss. "Magische Zahlen" mit hartcodierten Typen problematisch, die erst zur Laufzeit nicht ermittelt werden können. Z. B. Wenn Sie oder ein anderer Entwickler die CommandField irgendwann in der Zukunft (z. B. eine Schaltfläche "Bearbeiten") oder Änderungen neue Schaltflächen hinzugefügt der `ButtonType` -Eigenschaft, der vorhandene Code weiterhin ohne Fehler kompiliert, aber auf der Seite möglicherweise ein Ausnahmefehler oder unerwartetes Verhalten, je nachdem, wie Code geschrieben wurde und welche Änderungen vorgenommen wurden.

Ein alternativer Ansatz besteht darin die GridView und DetailsView s CommandFields in von TemplateFields zu konvertieren. Dadurch wird ein TemplateField mit einer `ItemTemplate` , die ein LinkButton (oder der Schaltfläche oder ImageButton) für jede Schaltfläche auf der CommandField hat. Diese Schaltflächen `OnClientClick` Eigenschaften können deklarativ zugewiesen werden, wie wir gesehen, mit der FormView-Steuerelement haben oder programmgesteuert werden, in den entsprechenden zugegriffen kann `DataBound` Ereignishandler mithilfe des folgenden Musters:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Wo *ControlID* ist der Wert der Schaltfläche s `ID` Eigenschaft. Während dieses Muster immer noch einen hartcodierte-Typ für die Umwandlung erforderlich ist, besteht keine Notwendigkeit es für die Indizierung für das Layout ändern, ohne einen Laufzeitfehler zu ermöglichen.

## <a name="summary"></a>Zusammenfassung

Das JavaScript `confirm(string)` Funktion ist ein häufig verwendetes Verfahren für die Steuerung des Workflows für Übermittlung. Bei der Ausführung zeigt die Funktion ein modales, clientseitige Dialogfeld, das enthält zwei Schaltflächen OK und Abbrechen an. Klickt der Benutzer "OK", die `confirm(string)` -Funktion zurückgegeben `true`; Wenn Sie auf "Abbrechen" `false`. Diese Funktionalität, gekoppelt mit einem s Browserverhalten eine Formularübergabe Abbrechen, wenn ein Ereignishandler während der Übertragung zurückgibt `false`, können verwendet werden, um eine Bestätigung Messagebox angezeigt wird, wenn einen Datensatz zu löschen.

Die `confirm(string)` Funktion kann mit einer Schaltfläche Web Steuerelement s der clientseitigen verbunden werden `onclick` Ereignishandler über das Steuerelement s `OnClientClick` Eigenschaft. Bei der Arbeit mit der löschen-Schaltfläche in einer Vorlage – entweder in einem FormView-Vorlagen s oder in ein TemplateField im DetailsView oder GridView - kann diese Eigenschaft festgelegt werden entweder deklarativ oder programmgesteuert, wie in diesem Tutorial beschrieben.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](implementing-optimistic-concurrency-cs.md)
> [Weiter](limiting-data-modification-functionality-based-on-the-user-cs.md)
