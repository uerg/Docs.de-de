---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: "Datenänderung Funktionalität beschränkt basierend auf dem Benutzer (VB) | Microsoft Docs"
author: rick-anderson
description: "In einer Web-Anwendung, die Benutzern ermöglicht, Daten zu bearbeiten, möglicherweise verschiedene Benutzerkonten Berechtigungen für verschiedene Daten bearbeiten. In diesem Lernprogramm untersucht, wie t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: e1bf749ce152d1c9f184d5726d5da059ffe0822f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Beschränken von Datenfunktionen Änderung basierend auf dem Benutzer (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) oder [PDF herunterladen](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> In einer Web-Anwendung, die Benutzern ermöglicht, Daten zu bearbeiten, möglicherweise verschiedene Benutzerkonten Berechtigungen für verschiedene Daten bearbeiten. In diesem Lernprogramm untersuchen wir wie die Möglichkeiten zur Datenänderung basierend auf der Website besucht Benutzer dynamisch angepasst.


## <a name="introduction"></a>Einführung

Eine Anzahl von Webanwendungen unterstützen Benutzerkonten und bieten unterschiedliche Optionen, Berichte und Funktionen, die basierend auf dem angemeldeten Benutzer. Beispielsweise möchten wir mit unseren Tutorials, möglicherweise aus der Supplier-Firmen zum Aktualisieren von Site und allgemeine Informationen zu ihren Produkten - ihren Namen und die Menge pro Einheit, melden Sie sich vielleicht – zusammen mit der Lieferanteninformationen, z. B. ihr Firmenname Benutzern ermöglichen, Adresse, Informationen zur Kontaktperson s und So weiter. Darüber hinaus möchten wir einige Benutzerkonten für Personen aus unser Unternehmen enthalten, sodass sie anmelden und Produktinformationen zu aktualisieren, wie Einheiten im Lager, neu anordnen Ebene usw. können. Unsere Webanwendung können auch anonyme Benutzer besuchen (Personen, die nicht angemeldet haben), aber würde auf nur Anzeigen von Daten zu beschränken. Mit solchen eine Benutzer-kontosystem vorhanden würden wir Daten Websteuerelementen in unserer ASP.NET-Seiten einfügen, bearbeiten und Löschen von Funktionen, die für den derzeit angemeldeten Benutzer geeignet anbieten möchten.

In diesem Lernprogramm untersuchen wir wie die Möglichkeiten zur Datenänderung basierend auf der Website besucht Benutzer dynamisch angepasst. Insbesondere erstellen wir eine Seite an den Lieferanten in einem bearbeitbaren DetailsView zusammen mit einem GridView die Anzeige von Informationen, die die vom Lieferanten bereitgestellte Produkte aufgeführt sind. Wenn der Benutzer Zugriff auf die Seite über unser Unternehmen ist, können sie: aller Lieferanten s Informationen; Bearbeiten Sie ihre Adresse; Anzeigen und bearbeiten Sie die Informationen für jedes Produkt, die vom Lieferanten bereitgestellt. Wenn der Benutzer jedoch von einem bestimmten Unternehmen ist, können sie nur anzeigen und bearbeiten Sie ihre eigenen Adressinformationen zur können nur bearbeiten, deren Produkte, die nicht als nicht mehr unterstützt markiert wurden.


[![Alle Lieferanten-s-Informationen können von einem Benutzer über unser Unternehmen bearbeitet.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Abbildung 1**: ein Benutzer über unser Unternehmen können bearbeiten Any Lieferanten s Informationen ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Ein Benutzer von einer bestimmten Lieferanten kann nur anzeigen und Bearbeiten ihrer Informationen](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Abbildung 2**: ein Benutzer aus einer bestimmten Lieferanten können nur anzeigen und Bearbeiten ihrer Informationen ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Lassen Sie s beginnen!

> [!NOTE]
> ASP.NET 2.0 s Mitgliedschaftssystem bietet eine standardisierte, erweiterbare Plattform zum Erstellen, verwalten und Überprüfen von Benutzerkonten. Da die Untersuchung von das Mitgliedschaftssystem Gegenstand mit diesen Lernprogrammen ist, fakes"in diesem Lernprogramm stattdessen" Mitgliedschaft können anonyme Besucher wählen, ob sie über einen bestimmten Lieferanten oder unser Unternehmen sind. Weitere Informationen über die Mitgliedschaft, finden Sie unter Meine [Untersuchen von ASP.NET 2.0 s Mitgliedschaft, Rollen und Profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) Artikel Reihe.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Schritt 1: Festlegen der Benutzer ihre Zugriffsrechte

In einer realen Webanwendung zählen eine s-Benutzerkontoinformationen, ob sie für unser Unternehmen oder für einen bestimmten Lieferanten funktioniert und diese Informationen wäre aus unserem ASP.NET-Seiten programmgesteuert zugegriffen werden kann, sobald der Benutzer auf die Website angemeldet hat. Diese Informationen kann durch ASP.NET 2.0 s Rollen System als Benutzerebene Kontoinformationen über das Profilsystem oder auch auf benutzerdefinierte Weise erfasst werden.

Da das Ziel dieses Lernprogramm veranschaulicht die Möglichkeiten zur Datenänderung basierend auf dem angemeldeten Benutzer anpassen und nicht, Showcase ASP.NET 2.0-s-Mitgliedschaft, Rollen und Profil Systeme vorgesehen ist, wir einen sehr einfach Mechanismus verwenden, um zu bestimmen die Funktionen für den Zugriff auf die Seite - DropDownList aus dem kann der Benutzer angeben, wenn sie anzeigen und bearbeiten Sie die Lieferanten Informationen oder alternativ was werden soll Benutzer bestimmten Lieferanten s Informationen anzeigen und bearbeiten kann. Wenn der Benutzer gibt an, dass sie anzeigen und Bearbeiten von Lieferanteninformationen für alle (Standard), können sie über alle Lieferanten Seite, alle Lieferanten s-Adressinformationen bearbeiten und bearbeiten Sie den Namen und die Menge pro Einheit für jedes Produkt, die von den ausgewählten Lieferanten bereitgestellt. Wenn der Benutzer gibt an, dass er nur anzeigen kann und bearbeiten sie einen bestimmten Lieferanten und dann nur die Details und die Produkte für diese ein Lieferant anzeigen und nur können kann aktualisieren, den Namen und die Menge pro Einheiteninformationen für diese Produkte, die *nicht* nicht mehr unterstützt.

Der erste Schritt in diesem Lernprogramm werden dann diese DropDownList erstellen, und weisen Sie ihm die Lieferanten im System. Öffnen der `UserLevelAccess.aspx` auf der Seite der `EditInsertDelete` Ordner hinzufügen eine DropDownList, deren `ID` -Eigenschaftensatz auf `Suppliers`, und binden Sie diese DropDownList an eine neue, mit dem Namen ObjectDataSource `AllSuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen AllSuppliersDataSource ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Abbildung 3**: Erstellen einer neuen ObjectDataSource namens `AllSuppliersDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Da diese DropDownList alle Lieferanten einschließen soll, konfigurieren Sie das ObjectDataSource zum Aufrufen der `SuppliersBLL` Klasse s `GetSuppliers()` Methode. Zudem sicherstellen, dass das ObjectDataSource-s `Update()` -Methode zugeordnet ist die `SuppliersBLL` Klasse s `UpdateSupplierAddress` -Methode, als diese ObjectDataSource wird auch verwendet werden von DetailsView wir in Schritt2 hinzufügen werden werden.

Führen Sie die Schritte nach dem Abschluss des Assistenten ObjectDataSource durch Konfigurieren der `Suppliers` DropDownList, dass es zeigt die `CompanyName` Feld "Daten" und verwendet die `SupplierID` Feld "Daten" als Wert für die einzelnen `ListItem`.


[![Konfigurieren der DropDownList Lieferanten, um die CompanyName und SupplierID Datenfelder verwenden](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren der `Suppliers` DropDownList zum Verwenden der `CompanyName` und `SupplierID` von Datenfeldern ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


Zu diesem Zeitpunkt sind der DropDownList die Firmennamen von Lieferanten in der Datenbank aufgeführt. Wir müssen jedoch auch eine Option "Alle Lieferanten anzeigen/bearbeiten" auf der DropDownList einschließen. Um dies zu erreichen, legen die `Suppliers` DropDownList s `AppendDataBoundItems` Eigenschaft `true` und fügen Sie dann eine `ListItem` , deren `Text` Eigenschaft ist "Alle Lieferanten anzeigen/bearbeiten" und dessen Wert ist `-1`. Dies kann hinzugefügt werden direkt über das deklarative Markup oder mithilfe des Designers indem Sie das Eigenschaftenfenster und durch Klicken auf die Auslassungspunkte in der DropDownList s `Items` Eigenschaft.

> [!NOTE]
> Verweisen auf die [ *Master/Detail-Filtern mit einer DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) für eine ausführlichere Erläuterung zum Hinzufügen von einer Select All-Element zu einem datengebundenen DropDownList Tutorial.


Nach der `AppendDataBoundItems` -Eigenschaft festgelegt wurde und die `ListItem` hinzugefügt wird, sollte der DropDownList s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Abbildung 5 zeigt einen Screenshot der aktuelle Fortschritt an, wenn Sie über einen Browser angezeigt.


[![DropDownList Lieferanten enthält ein Anzeigen aller "ListItem" Plus 1 für jeden Lieferanten](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Abbildung 5**: die `Suppliers` DropDownList enthält alle anzeigen `ListItem`, Plus eine für jede Lieferanten ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Da wir die Benutzeroberfläche aktualisiert wird, sofort, nachdem der Benutzer, deren Auswahl geändert wurde möchten, legen Sie die `Suppliers` DropDownList s `AutoPostBack` Eigenschaft `true`. In Schritt2 erstellen wir ein DetailsView-Steuerelement aus, der anzeigt, die Informationen für die basierend auf der Auswahl der DropDownList müssten. Klicken Sie dann in Schritt 3, wir erstellen einen Ereignishandler für diese s DropDownList `SelectedIndexChanged` Ereignis, in dem wir fügen Code, der die entsprechenden Lieferanteninformationen, DetailsView bindet auf Grundlage des ausgewählten Lieferanten.

## <a name="step-2-adding-a-detailsview-control"></a>Schritt 2: Hinzufügen, DetailsView-Steuerelement

Lassen Sie s eine DetailsView zu verwenden, um Lieferanteninformationen anzuzeigen. Für den Benutzer, die anzeigen und bearbeiten alle Lieferanten, unterstützt DetailsView Paging Dadurch kann der Benutzer einen Datensatz der Lieferanten-Informationen zu einem Zeitpunkt durchlaufen. Wenn der Benutzer für einen bestimmten Lieferanten arbeitet, jedoch, DetailsView nur bestimmten Lieferanten s Informationen angezeigt werden und beziehen keine Auslagerungsdatei-Schnittstelle. In beiden Fällen muss die DetailsView der Benutzer die Lieferanten s Adresse Stadt und Landesfelder bearbeiten können.

Eine DetailsView zur Seite "unter" Hinzufügen der `Suppliers` DropDownList, legen Sie seine `ID` Eigenschaft, um `SupplierDetails`, und binden Sie es an die `AllSuppliersDataSource` ObjectDataSource im vorherigen Schritt erstellt. Überprüfen Sie anschließend die Kontrollkästchen Paging aktivieren und zu bearbeiten aktivieren aus dem DetailsView-s-Smarttag.

> [!NOTE]
> Peter t eine Option bearbeiten Aktivieren der intelligenten DetailsView s angezeigt markieren s, weil keine der ObjectDataSource s zugeordnet `Update()` Methode, um die `SuppliersBLL` Klasse s `UpdateSupplierAddress` Methode. Nehmen Sie einen Moment Zeit, wechseln zurück und stellen diese Konfiguration ändern, nach dem sollte die Option Bearbeiten aktivieren im smart Tag, DetailsView s angezeigt, ein.


Da die `SuppliersBLL` Klasse s `UpdateSupplierAddress` Methode akzeptiert nur vier Parameter - `supplierID`, `address`, `city`, und `country` -ändern, DetailsView-s BoundFields, damit die `CompanyName` und `Phone` BoundFields sind schreibgeschützt. Entfernen Sie außerdem die `SupplierID` BoundField-vollständig. Schließlich die `AllSuppliersDataSource` ObjectDataSource verfügt derzeit über seine `OldValuesParameterFormatString` -Eigenschaftensatz auf `original_{0}`. Nehmen Sie einen Moment Zeit, um die Einstellung dieser Eigenschaft entfernen, die vollständig deklarative Syntax aus, oder legen sie auf den Standardwert `{0}`.

Nach dem Konfigurieren der `SupplierDetails` DetailsView und `AllSuppliersDataSource` ObjectDataSource, müssen wir die folgenden deklarative Markup:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

Zu diesem Zeitpunkt über DetailsView ausgelagert werden kann und die Adressinformationen für die ausgewählte Lieferanten s kann aktualisiert werden, und unabhängig von der getroffenen Auswahl der `Suppliers` DropDownList (siehe Abbildung 6).


[![Alle Lieferanten Informationen angezeigt werden kann, und seine Adresse aktualisiert](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Abbildung 6**: Any Lieferanten Informationen kann angezeigt werden, und seine Adresse aktualisiert ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Schritt 3: Anzeigen von nur die ausgewählten Lieferanten-s-Informationen

Unsere Seite derzeit zeigt die Informationen für alle Lieferanten unabhängig davon, ob ein bestimmter Lieferant aus ausgewählt wurde die `Suppliers` DropDownList. Um nur die Lieferanteninformationen für den ausgewählten Lieferanten anzuzeigen, müssen wir unserer Seite einer anderen ObjectDataSource hinzugefügt, die Informationen über einen bestimmten Lieferanten abruft.

Hinzufügen einer neuen ObjectDataSource auf der Seite zu benennen `SingleSupplierDataSource`. Das Smarttag, klicken Sie auf den Link für die Datenquelle konfigurieren und verwenden lassen die `SuppliersBLL` Klasse s `GetSupplierBySupplierID(supplierID)` Methode. Wie bei der `AllSuppliersDataSource` ObjectDataSource, haben die `SingleSupplierDataSource` ObjectDataSource s `Update()` Methode zugeordnet wird, um die `SuppliersBLL` Klasse s `UpdateSupplierAddress` Methode.


[![Konfigurieren der SingleSupplierDataSource ObjectDataSource zur Verwendung der GetSupplierBySupplierID(supplierID)-Methode](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Abbildung 7**: Konfigurieren der `SingleSupplierDataSource` ObjectDataSource verwenden die `GetSupplierBySupplierID(supplierID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Als Nächstes wir re aufgefordert werden, an die Parameterquelle für die `GetSupplierBySupplierID(supplierID)` Methode s `supplierID` input-Parameters. Da die Informationen für den Lieferanten, die aus der DropDownList verwenden ausgewählt angezeigt werden sollen die `Suppliers` DropDownList s `SelectedValue` Eigenschaft als Parameterquelle für.


[![Verwenden der DropDownList Lieferanten als die SupplierID Parameterquelle](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Abbildung 8**: Verwenden der `Suppliers` DropDownList als die `supplierID` Parameterquelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Auch bei diesem zweiten ObjectDataSource hinzugefügt, DetailsView-Steuerelement ist derzeit so konfiguriert, dass immer die `AllSuppliersDataSource` ObjectDataSource. Wir müssen die Logik zum Anpassen der Datenquelle, DetailsView abhängig, in der `Suppliers` DropDownList-Element ausgewählt. Um dies zu erreichen, erstellen Sie eine `SelectedIndexChanged` -Ereignishandler für die Lieferanten DropDownList. Dies kann am einfachsten durch Doppelklicken auf die DropDownList im Designer erstellt werden. Dieser Ereignishandler muss, um zu bestimmen, welche Datenquelle verwendet und muss die Daten, DetailsView binden. Dies erfolgt durch den folgenden Code:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Der Ereignishandler wird zuerst bestimmen, ob die Option "Alle Lieferanten anzeigen/bearbeiten" ausgewählt wurde. Wenn es war, wird der `SupplierDetails` DetailsView s `DataSourceID` auf `AllSuppliersDataSource` und den Benutzer auf den ersten Eintrag in der Menge der Lieferanten zurückgibt, durch Festlegen der `PageIndex` Eigenschaft auf 0. Wenn jedoch der Benutzer einen bestimmten Lieferanten aus der DropDownList, DetailsView s ausgewählt hat `DataSourceID` zugewiesen `SingleSuppliersDataSource`. Unabhängig davon, welche Daten der Datenquelle verwendet wird, die `SuppliersDetails` Modus auf den nur-Lese-Modus zurückgesetzt wird, und die Daten, DetailsView gebunden ist, durch einen Aufruf der `SuppliersDetails` Steuerelement s `DataBind()` Methode.

Mit diesem Ereignishandler vorhanden zeigt DetailsView-Steuerelement jetzt den ausgewählten Lieferanten, wenn die Option "Alle Lieferanten anzeigen/bearbeiten" ausgewählt wurde, in diesem Fall können alle Lieferanten über die Pagingbenutzeroberfläche für angezeigt werden. Abbildung 9 zeigt die Seite mit der Option "Alle Lieferanten anzeigen/bearbeiten"; Beachten Sie, dass die Auslagerungsdatei-Schnittstelle vorhanden ist, die Benutzer besuchen, und aktualisieren alle Lieferanten. Abbildung 10 zeigt die Seite mit den Ma Maison Lieferanten ausgewählt. Nur Ma Maison s Informationen wird in diesem Fall angezeigt und bearbeitet werden.


[![Alle Lieferanten Informationen kann angezeigt und bearbeitet werden](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Abbildung 9**: alle Lieferanten Informationen kann angezeigt und bearbeitet ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Nur die ausgewählten s von Lieferanteninformationen kann angezeigt und bearbeitet werden](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Abbildung 10**: nur der ausgewählten Lieferanten s Informationen Ansicht und bearbeitet werden kann ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Für dieses Lernprogramm die DropDownList und die DetailsView steuern s `EnableViewState` muss festgelegt werden, um `true` (Standard) da der DropDownList-s `SelectedIndex` und der s DetailsView `DataSourceID` eigenschaftenänderungen s müssen über Postbacks gespeichert werden.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Schritt 4: Auflisten von Lieferanten Produkte in einem bearbeitbaren GridView

Mit der DetailsView abgeschlossen ist wird im nächsten Schritt eine bearbeitbare GridView einschließen, die diese Produkte von den ausgewählten Lieferanten bereitgestellte aufgeführt sind. Diese GridView vorsieht, bearbeitet nur die `ProductName` und `QuantityPerUnit` Felder. Darüber hinaus ist der Benutzer Zugriff auf die Seite von einem bestimmten anderen Lieferanten, sie sollten nur zulassen, Updates für diese Produkte, die *nicht* nicht mehr unterstützt. Um dies zu erreichen wir zunächst eine Überladung der hinzufügen müssen der `ProductsBLL` Klasse s `UpdateProducts` -Methode, die akzeptiert nur die `ProductID`, `ProductName`, und `QuantityPerUnit` Felder als Eingaben. Es gibt es in Einzelschritten durchlaufen dieser Prozess im Vorfeld in zahlreichen Lernprogrammen lassen s, die gerade den Code hier betrachten, die hinzugefügt werden soll `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Mit dieser Überladung erstellt, es re kann jetzt des GridView-Steuerelements und seiner zugeordneten ObjectDataSource hinzugefügt werden. Die Seite neue GridView hinzugefügt haben, legen Sie seine `ID` Eigenschaft `ProductsBySupplier`, und konfigurieren, um eine neue, mit dem Namen ObjectDataSource verwenden `ProductsBySupplierDataSource`. Da wir diese GridView diese Produkte von den ausgewählten Lieferanten auflisten möchten, verwenden Sie die `ProductsBLL` Klasse s `GetProductsBySupplierID(supplierID)` Methode. Ordnen Sie auch die `Update()` Methode, um die neue `UpdateProduct` Überladung, die soeben erstellt wurde.


[![Konfigurieren der ObjectDataSource Verwendung die UpdateProduct-Überladung, die gerade erstellt haben](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Abbildung 11**: Konfigurieren der ObjectDataSource verwenden die `UpdateProduct` erstellte überladen ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Wir re aufgefordert, wählen Sie die Parameterquelle für die `GetProductsBySupplierID(supplierID)` Methode s `supplierID` input-Parameters. Da die Produkte für den Lieferanten, DetailsView, verwenden ausgewählt angezeigt werden sollen die `SuppliersDetails` DetailsView-Steuerelement s `SelectedValue` Eigenschaft als Parameterquelle für.


[![Verwenden Sie die SuppliersDetails DetailsView s "SelectedValue"-Eigenschaft als Parameterquelle für](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Abbildung 12**: Verwenden der `SuppliersDetails` DetailsView s `SelectedValue` Eigenschaft als Parameterquelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Rückgabe an die GridView entfernen alle Felder mit Ausnahme von GridView `ProductName`, `QuantityPerUnit`, und `Discontinued`, markieren das `Discontinued` CheckBoxField als schreibgeschützt. Überprüfen Sie auch die Option Bearbeiten aktivieren aus dem Smarttag des GridView s ein. Nachdem diese Änderungen vorgenommen wurden, sollte das deklarative Markup für die GridView und ObjectDataSource etwa wie folgt aussehen:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Wie bei unserer vorherigen ObjectDataSources, diese s `OldValuesParameterFormatString` -Eigenschaftensatz auf `original_{0}`, wodurch Probleme beim Versuch, ein Produktname s oder eine Menge pro Einheit zu aktualisieren. Diese Eigenschaft von der deklarativen Syntax vollständig entfernen, oder legen Sie sie auf den Standardwert `{0}`.

Mit dieser Konfiguration abgeschlossen, unsere Seite jetzt Listet die Produkte bereitgestellt, die vom Lieferanten in der GridView ausgewählt (siehe Abbildung 13). Derzeit *alle* Produktnamen s oder Menge pro Einheit aktualisiert werden kann. Allerdings müssen wir unsere Seitenlogik aktualisieren, sodass solche Funktionen für nicht mehr unterstützte Produkte für einen bestimmten Lieferanten zugeordneten Benutzer nicht zulässig ist. Wir werden diese letzte Stück in Schritt 5 konfigurieren.


[![Die Produkte, die von den ausgewählten Lieferanten bereitgestellt werden angezeigt.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Abbildung 13**: der Produkte von Lieferanten ausgewählt bereitgestellt werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Durch das Hinzufügen dieser bearbeitbaren GridView der `Suppliers` DropDownList s `SelectedIndexChanged` Ereignishandler sollte aktualisiert werden, um die GridView in einem schreibgeschützten Status wiederherzustellen. Andernfalls, wenn ein anderer Lieferanten mitten in Bearbeitung Produktinformationen ausgewählt ist, wird der zugehörige Index in die GridView für den neuen Lieferanten auch bearbeitet werden. Um dies zu verhindern, legen Sie einfach die GridView s `EditIndex` Eigenschaft `-1` in der `SelectedIndexChanged` -Ereignishandler.


Beachten Sie außerdem, dass es wichtig ist, dass die GridView Ansichtszustand s werden (das Standardverhalten) aktiviert. Wenn Sie festlegen, dass die GridView s `EnableViewState` Eigenschaft `false`, laufen Sie Gefahr, dass gleichzeitige Benutzer versehentlich löschen oder Bearbeiten von Datensätzen. Finden Sie unter [Warnung: Parallelität Problem mit ASP.NET 2.0 GridViews/DetailsView/FormViews, Bearbeitung unterstützen und/oder löschen und, deren Ansichtszustand deaktiviert](http://scottonwriting.net/sowblog/posts/10054.aspx) für Weitere Informationen.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Schritt 5: Unterbinden bearbeiten, nicht mehr unterstützte Produkte beim Anzeigen/Bearbeiten Alle Lieferanten nicht ausgewählt ist.

Während der `ProductsBySupplier` GridView ist voll funktionsfähig, es derzeit zu viele Zugriff gewährt Benutzer, die von einem bestimmten anderen Lieferanten sind. Pro unsere Geschäftsregeln sollten solche Benutzer nicht nicht mehr unterstützte Produkte aktualisieren können. Um dies zu erzwingen, können wir auszublenden (deaktivieren oder) die Schaltfläche "Bearbeiten", in den Zeilen GridView, nicht mehr unterstützte Produkte, wenn die Seite von einem Benutzer von einem anderen Lieferanten besucht wird.

Erstellen Sie einen Ereignishandler für die GridView s `RowDataBound` Ereignis. In diesem Ereignishandler müssen wir ermitteln, und zwar unabhängig davon, ob der Benutzer einen bestimmten Lieferanten zugeordnet ist, die für dieses Lernprogramm bestimmt werden kann, überprüfen Sie die Lieferanten DropDownList-s `SelectedValue` Eigenschaft – Wenn sie s anderes Element als -1, und klicken Sie dann auf der Benutzer ist Bei einem bestimmten Lieferanten zugeordnet ist. Für solche Benutzer müssen wir ermitteln, und zwar unabhängig davon, ob das Produkt nicht mehr unterstützt wird. Wir ziehen können Sie einen Verweis auf den tatsächlichen `ProductRow` Instanz gebunden werden, um die GridView Zeile über die `e.Row.DataItem` -Eigenschaft verwenden, wie in beschrieben die [ *Anzeigen von Zusammenfassungsinformationen in die GridView s Fußzeile* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Lernprogramm. Wenn das Produkt nicht mehr unterstützt wird, können wir ziehen Sie einen programmgesteuerte Verweis auf die Schaltfläche "Bearbeiten", in der GridView s mit den im vorherigen Lernprogramm beschriebenen Verfahren CommandField [ *Hinzufügen von clientseitigen Bestätigung beim Löschen von* ](adding-client-side-confirmation-when-deleting-vb.md). Nachdem wir einen Verweis haben, den wir können ausblenden oder deaktivieren Sie anschließend die Schaltfläche "".


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Mit diesem Ereignis Handler eingerichtet ist, beim Zugriff auf diese Seite als ein Benutzer diese Produkte, die nicht mehr unterstützt werden von einem bestimmten anderen Lieferanten können nicht bearbeitet werden, wie die Schaltfläche "Bearbeiten" ist für diese Produkte ausgeblendet. Chef Anton s Gumbo Mix ist z. B. einem ausgelaufenen Produkt für den Lieferanten New Orleans Cajun Delights. Wenn die Seite für diesen bestimmten Lieferanten besuchen, wird die Schaltfläche "Bearbeiten" für dieses Produkt aus Sight ausgeblendet (siehe Abbildung 14). Allerdings ist die Schaltfläche "Bearbeiten" beim Zugriff auf die mithilfe der "anzeigen/bearbeiten alle Suppliers", verfügbar ist (siehe Abbildung 15).


[![Für Anbieter-spezifischen Benutzer wird die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix ausgeblendet.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Abbildung 14**: für Lieferanten-spezifische Benutzer die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix ausgeblendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Für alle Lieferanten Benutzer anzeigen/bearbeiten wird die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix angezeigt.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Abbildung 15**: für anzeigen/bearbeiten alle Lieferanten Benutzer, für die Chef Anton s Gumbo Mix angezeigt wird, auf die Schaltfläche Bearbeiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Überprüfung auf die Zugriffsrechte in der Geschäftslogikebene

In diesem Lernprogramm ASP.NET Seite behandelt alle Logik im Hinblick auf welche Informationen, die dem Benutzer angezeigt werden kann und welche Produkte, die er aktualisieren. Im Idealfall würden diese Logik auch bei der Geschäftslogikschicht vorhanden sein. Z. B. die `SuppliersBLL` Klasse s `GetSuppliers()` -Methode (die alle Lieferanten zurückgibt) möglicherweise eine Prüfung, um sicherzustellen, dass der derzeit angemeldete Benutzer enthalten *nicht* ein bestimmter Lieferant zugeordnet. Entsprechend der `UpdateSupplierAddress` -Methode kann ein Häkchen, um sicherzustellen, dass den derzeit angemeldeten Benutzer entweder unser Unternehmen tätig (und daher können alle Lieferanten Adressinformationen zu aktualisieren) enthalten oder bezieht sich auf den Lieferanten, deren Daten aktualisiert wird.

Ich hier solchen Überprüfungen aus BLL-Ebene nicht enthalten, da in unser Tutorial aus, die Benutzerrechte s durch DropDownList auf der Seite bestimmt werden, die BLL Klassen zugreifen können. Bei der Verwendung das Mitgliedschaftssystem oder eines der Out-die mitgelieferten Authentifizierungsschemas, die von ASP.NET bereitgestellt wird (z. B. Windows-Authentifizierung), der derzeit angemeldete Benutzer s Informationen und Rolleninformationen der BLL, wodurch Zugriff zugegriffen werden kann Rechte überprüft am sowohl die Ebenen Präsentation und BLL möglich.

## <a name="summary"></a>Zusammenfassung

Die meisten Standorte, die Benutzerkonten bereitstellen müssen die Änderungen-Datenschnittstelle, die basierend auf den angemeldeten Benutzer anpassen. Administratoren möglicherweise ein Datensatz, bearbeiten und Löschen nicht-Administratoren beschränkt auf das nur aktualisieren oder Löschen von Datensätzen, die sie selbst erstellt werden können. Beliebige Szenario sein, die Daten Web Controls, ObjectDataSource, und Business Logic Layer-Klassen können erweitert werden, zum Hinzufügen oder bestimmte Funktionen, die basierend auf dem angemeldeten Benutzer zu verweigern. In diesem Lernprogramm wurde erläutert, wie die Daten angezeigt und bearbeitet werden, je nachdem, ob der Benutzer einen bestimmten Lieferanten zugeordnet wurde, oder wenn sie unser Unternehmen arbeitete einzuschränken.

In diesem Lernprogramm wird unsere Untersuchung von einfügen, aktualisieren und Löschen von Daten mit den Steuerelementen GridView, DetailsView und FormView abgeschlossen. Beginnend mit den nächsten Lernprogrammen, müssen wir unsere Aufmerksamkeit dem Hinzufügen von paging und Sortieren von Unterstützung aktivieren.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](adding-client-side-confirmation-when-deleting-vb.md)
