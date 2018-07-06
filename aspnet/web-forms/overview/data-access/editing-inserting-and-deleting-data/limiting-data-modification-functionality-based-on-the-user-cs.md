---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Benutzerabhängiges basierend auf dem Benutzer (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In einer Webanwendung, die Benutzern ermöglicht, Daten zu bearbeiten, möglicherweise verschiedene Benutzerkonten Berechtigungen für andere Bearbeiten von Daten. In diesem Tutorial untersucht, wie t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: d011f57834ff27efd888a3f66342a7d0a2d70d8c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811392"
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Benutzerabhängiges basierend auf dem Benutzer (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) oder [PDF-Datei herunterladen](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> In einer Webanwendung, die Benutzern ermöglicht, Daten zu bearbeiten, möglicherweise verschiedene Benutzerkonten Berechtigungen für andere Bearbeiten von Daten. In diesem Tutorial betrachten wir, wie Sie die Möglichkeiten zur Datenänderung basierend auf der Website besucht Benutzer dynamisch anpassen.


## <a name="introduction"></a>Einführung

Eine Anzahl von Webanwendungen unterstützen Benutzerkonten und bieten verschiedene Optionen, Berichte und Funktionen, die basierend auf dem angemeldeten Benutzer. Beispielsweise möchten wir mit unseren Tutorials kann Benutzer aus der Supplier-Firmen, die auf der Website "und" Update allgemeine Informationen über ihre Produkte - ihren Namen und eine Menge pro Einheit, melden Sie sich vielleicht – zusammen mit der Lieferanteninformationen, z. B. ihren Firmennamen können, Adresse, Kontaktperson s Informationen und So weiter. Darüber hinaus möchten wir einige Benutzerkonten für unser Unternehmen für Personen enthalten, sodass sie anmelden und Produktinformationen zu aktualisieren, wie Einheiten auf Lager, neu anordnen Ebene usw. können. Unserer Webanwendung möglicherweise auch Fall jeder anonyme Benutzer zum Besuch (Personen, die nicht angemeldet haben), jedoch würden sie nur Anzeige von Daten einschränken. Mit solchen eine Benutzer-kontosystem vorhanden sollten wir die Daten Websteuerelemente in unserer ASP.NET-Seiten der einfügen, bearbeiten und Löschen von Funktionen, die für den derzeit angemeldeten Benutzer zu bieten.

In diesem Tutorial betrachten wir, wie Sie die Möglichkeiten zur Datenänderung basierend auf der Website besucht Benutzer dynamisch anpassen. Insbesondere erstellen wir eine Seite an, in dem die Lieferanten-Informationen in einem bearbeitbaren DetailsView zusammen mit einer GridView-Ansicht angezeigt, die Produkte von Lieferanten auflistet. Wenn der Benutzer auf der Seite über unser Unternehmen ist, können sie: Zeigen Sie alle Lieferanteninformationen s; Bearbeiten Sie ihre Adresse; Anzeigen und bearbeiten Sie die Informationen für jedes Produkt, die von den Lieferanten bereitgestellt. Wenn der Benutzer jedoch von einem bestimmten Unternehmen ist, können sie nur anzeigen und bearbeiten Sie ihre eigene Adressinformationen und können nur bearbeiten, ihre Produkte, die nicht als nicht mehr unterstützt markiert wurden.


[![Alle Lieferanten-s-Informationen können von einem Benutzer in unserem Unternehmen bearbeitet.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Abbildung 1**: ein Benutzer über unser Unternehmen können beliebige Zulieferer bearbeiten s Informationen ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Ein Benutzer von einem bestimmten Lieferanten kann nur anzeigen und Bearbeiten ihrer Informationen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Abbildung 2**: ein Benutzer einen bestimmten Anbieter können nur anzeigen und Bearbeiten ihrer Informationen ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Lassen Sie s beginnen!

> [!NOTE]
> ASP.NET 2.0 s Mitgliedschaftssystem bietet eine standardisierte, erweiterbare Plattform für das Erstellen und Verwalten von Benutzerkonten werden überprüft. Da eine Untersuchung der Mitgliedschaftssystem in diesen Tutorials nicht eingegangen ist, fakes"in diesem Tutorial stattdessen" Mitgliedschaft von auswählen, ob sie über einen bestimmten Lieferanten oder unser Unternehmen für das anonyme Besucher zulassen. Weitere Informationen zu Mitgliedschaft, finden Sie in meiner [Untersuchung von ASP.NET 2.0-s-Mitgliedschaft, Rollen und Profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) Artikel der Reihe.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Schritt 1: Der Benutzer ihre Zugriffsrechte angeben kann.

In einer realen-Webanwendung würde eine s-Benutzerkontoinformationen enthalten, ob sie für unser Unternehmen oder für einen bestimmten Lieferanten haben und diese Informationen sind aus unseren ASP.NET-Seiten programmgesteuert zugegriffen werden kann, sobald der Benutzer auf der Website angemeldet hat. Diese Informationen kann durch ASP.NET 2.0-s-Rollen-System, als auf Benutzerebene Kontoinformationen über das Profilsystem oder über bestimmte benutzerdefinierte Wege erfasst werden.

Da das Ziel in diesem Tutorial veranschaulicht die Möglichkeiten zur Datenänderung basierend auf den angemeldeten Benutzer anpassen und ist nicht für ASP.NET 2.0-Showcase-s-Mitgliedschaft, Rollen und Profil Systeme vorgesehen ist, verwenden wir einen Mechanismus sehr einfach um zu bestimmen, die Funktionen für den Benutzer, die auf der Seite – einem DropDownList-Steuerelement aus dem kann der Benutzer angeben, wenn sie anzeigen und Bearbeiten der Lieferanten-Informationen oder alternativ was werden sollen bestimmte s Lieferanteninformationen anzeigen und bearbeiten kann. Wenn der Benutzer angibt, dass sie anzeigen und Bearbeiten von Lieferanteninformationen für alle (Standard), können sie alle Lieferanten durchblättern, alle Lieferanten-s-Adressinformationen bearbeiten und Bearbeiten von Name und Menge pro Einheit für jedes Produkt, die von den ausgewählten Lieferanten bereitgestellte. Wenn der Benutzer gibt an, dass sie nur anzeigen können und bearbeiten, die ein bestimmter Lieferant, allerdings wird sie nur die Details und die Produkte für diese ein Lieferant anzeigen und nur können kann aktualisieren, den Namen und eine Menge pro Einheiteninformationen für diese Produkte, die *nicht* nicht mehr unterstützt.

Im ersten Schritt in diesem Tutorial werden dann diese DropDownList erstellen und füllen Sie es mit Lieferanten, im System. Öffnen der `UserLevelAccess.aspx` auf der Seite die `EditInsertDelete` Ordner einem DropDownList-Steuerelement hinzufügen, deren `ID` -Eigenschaftensatz auf `Suppliers`, und binden Sie diese DropDownList an eine neue, mit dem Namen "ObjectDataSource" `AllSuppliersDataSource`.


[![Erstellen Sie eine neue, mit dem Namen AllSuppliersDataSource "ObjectDataSource"](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Abbildung 3**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `AllSuppliersDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Da dieser Dropdownliste alle Lieferanten eingeschlossen werden soll, konfigurieren Sie dem ObjectDataSource-Steuerelement zum Aufrufen der `SuppliersBLL` Klasse s `GetSuppliers()` Methode. Außerdem stellen Sie sicher, dass das "ObjectDataSource"-s `Update()` Methode zugeordnet ist die `SuppliersBLL` Klasse s `UpdateSupplierAddress` -Methode, als diese "ObjectDataSource" auch verwendet werden von der DetailsView, die wir in Schritt2 hinzufügen.

Nach Abschluss des ObjectDataSource-Steuerelement-Assistenten, führen Sie die Schritte, durch Konfigurieren der `Suppliers` DropDownList so, dass es zeigt die `CompanyName` Datenfeld und der `SupplierID` Feld "Daten" als Wert für die einzelnen `ListItem`.


[![Konfigurieren von Lieferanten DropDownList CompanyName und SupplierID Datenfelder verwenden](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Abbildung 4**: Konfigurieren der `Suppliers` DropDownList verwenden die `CompanyName` und `SupplierID` von Datenfeldern ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


An diesem Punkt werden in der Dropdownliste die Unternehmensnamen von Lieferanten in der Datenbank aufgelistet. Allerdings müssen wir auch eine Option "Alle Lieferanten der anzeigen/bearbeiten" zu DropDownList einschließen. Zu diesem Zweck legen Sie die `Suppliers` DropDownList s `AppendDataBoundItems` Eigenschaft `true` und fügen Sie dann eine `ListItem` , deren `Text` -Eigenschaft ist "Aller-Lieferanten anzeigen/bearbeiten" und dessen Wert ist `-1`. Dies kann hinzugefügt werden direkt über den deklarativen Markup oder mit dem Designer indem Sie das Fenster "Eigenschaften" und durch Klicken auf die Auslassungspunkte in der DropDownList s `Items` Eigenschaft.

> [!NOTE]
> Siehe die [ *Master/Detail-Filtern mit einer DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Tutorial für eine ausführlichere Beschreibung zum Hinzufügen von einer Select All-Element zu einer datengebundenen DropDownList.


Nach der `AppendDataBoundItems` -Eigenschaft festgelegt wurde und die `ListItem` hinzugefügt haben, sollte die DropDownList s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Abbildung 5 zeigt einen Screenshot des unseren aktuellen Status, wenn Sie über einen Browser angezeigt.


[![Lieferanten DropDownList enthält ein Anzeigen aller ListItem Plus 1 für jeden Lieferanten](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Abbildung 5**: die `Suppliers` DropDownList enthält alle anzeigen `ListItem`, Plus 1 für jede Lieferanten ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Da wir möchten die Benutzeroberfläche zu aktualisieren, sobald der Benutzer die Auswahl geändert hat, legen Sie die `Suppliers` DropDownList s `AutoPostBack` Eigenschaft `true`. In Schritt2 erstellen wir ein DetailsView-Steuerelement, das die Informationen für die Lieferanten, die basierend auf der DropDownList-Auswahl angezeigt wird. Klicken Sie dann in Schritt 3, wir erstellen einen Ereignishandler für diese s DropDownList `SelectedIndexChanged` -Ereignis, in dem wir fügen Code, der die entsprechenden Lieferanteninformationen DetailsView gebunden wird anhand des ausgewählten Lieferanten.

## <a name="step-2-adding-a-detailsview-control"></a>Schritt 2: Hinzufügen von einem DetailsView-Steuerelement

Lassen Sie s eine DetailsView zu verwenden, um Lieferanteninformationen anzuzeigen. Für den Benutzer, die anzeigen und Bearbeiten aller-Lieferanten, unterstützt die DetailsView Paging, damit der Benutzer einen Datensatz des Lieferanten-Informationen zu einem Zeitpunkt durchlaufen. Wenn der Benutzer für einen bestimmten Lieferanten funktioniert, jedoch DetailsView nur bestimmte Lieferanten s Informationen angezeigt werden, und eine Paging-Schnittstelle nicht enthält. In beiden Fällen muss die DetailsView ermöglichen die Benutzer, die Lieferanten-e-Adresse, Stadt und Landesfelder zu bearbeiten.

Fügen Sie eine DetailsView auf der Seite unter der `Suppliers` DropDownList, legen die `ID` Eigenschaft, um `SupplierDetails`, und binden Sie es an der `AllSuppliersDataSource` "ObjectDataSource", die im vorherigen Schritt erstellt haben. Als Nächstes aktivieren Sie die Kontrollkästchen "Paging aktivieren und die Bearbeitung aktivieren" aus dem DetailsView-s-Smarttag.

> [!NOTE]
> Sie Ich möchte eine Option bearbeiten Aktivieren der intelligenten DetailsView s angezeigt zu kennzeichnen. s, da Sie nicht das "ObjectDataSource"-s zugeordnet werden konnte `Update()` Methode, um die `SuppliersBLL` Klasse s `UpdateSupplierAddress` Methode. Wechseln zurück, und stellen diese Konfiguration, die nach dem sollte die Option Bearbeiten aktivieren im DetailsView-s-Smarttag angezeigt. ändern können.


Da die `SuppliersBLL` s-Klasse `UpdateSupplierAddress` -Methode akzeptiert nur vier Parameter - `supplierID`, `address`, `city`, und `country` -DetailsView s BoundFields ändern, damit die `CompanyName` und `Phone` BoundFields sind schreibgeschützt. Entfernen Sie außerdem die `SupplierID` BoundField-vollständig. Zum Schluss die `AllSuppliersDataSource` ObjectDataSource-Steuerelement verfügt derzeit über seine `OldValuesParameterFormatString` -Eigenschaftensatz auf `original_{0}`. Entfernen diese eigenschafteneinstellung wird vollständig aus der deklarative Syntax oder legen Sie ihn auf den Standardwert in Ruhe `{0}`.

Nach dem Konfigurieren der `SupplierDetails` DetailsView und `AllSuppliersDataSource` "ObjectDataSource", haben wir die folgende deklarative Markup:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

An diesem Punkt DetailsView über ausgelagert werden kann und die Adressinformationen für die ausgewählte Lieferanten s aktualisiert werden kann, unabhängig von der Auswahl getroffen wurde, der `Suppliers` DropDownList (siehe Abbildung 6).


[![Alle Lieferanten Informationen kann angezeigt werden, und seine Adresse aktualisiert.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Abbildung 6**: Any Lieferanten Informationen kann angezeigt werden, und seine Adresse aktualisiert ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Schritt 3: Anzeigen von nur die ausgewählten Lieferanten-s-Informationen

Die Seite derzeit zeigt die Informationen für alle Lieferanten, unabhängig davon, ob ein bestimmter Lieferant aus ausgewählt wurde die `Suppliers` DropDownList. Um nur die Lieferanteninformationen für den ausgewählten Lieferanten anzuzeigen müssen wir eine andere "ObjectDataSource" auf unserer Seite eine hinzufügen, die Informationen zu einem bestimmten Lieferanten abruft.

Hinzufügen einer neuen "ObjectDataSource" zur Seite, nennen Sie es `SingleSupplierDataSource`. Klicken Sie auf die Datenquelle konfigurieren-Link sein Smarttag, und lassen Sie sie verwenden die `SuppliersBLL` Klasse s `GetSupplierBySupplierID(supplierID)` Methode. Wie bei der `AllSuppliersDataSource` "ObjectDataSource", haben die `SingleSupplierDataSource` "ObjectDataSource" s `Update()` Methode zugeordnet wird, um die `SuppliersBLL` s-Klasse `UpdateSupplierAddress` Methode.


[![Konfigurieren Sie die SingleSupplierDataSource "ObjectDataSource" zur Verwendung der GetSupplierBySupplierID(supplierID)-Methode](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Abbildung 7**: Konfigurieren der `SingleSupplierDataSource` "ObjectDataSource" Verwenden der `GetSupplierBySupplierID(supplierID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


Als Nächstes wir erneut einzugeben, geben Sie die Parameterquelle für die `GetSupplierBySupplierID(supplierID)` s-Methode `supplierID` input-Parameters. Da wir die Informationen für den Lieferanten, die aus der Dropdownliste, verwenden ausgewählt anzeigen möchten die `Suppliers` DropDownList s `SelectedValue` -Eigenschaft der Parameterquelle.


[![Verwenden von Lieferanten DropDownList als die SupplierID Parameterquelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Abbildung 8**: Verwenden der `Suppliers` DropDownList, als die `supplierID` Parameterquelle ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


Trotz dieser zweiten "ObjectDataSource" hinzugefügt, DetailsView-Steuerelement ist gegenwärtig konfiguriert verwenden Sie immer die `AllSuppliersDataSource` "ObjectDataSource". Wir müssen die Logik zum Anpassen der Datenquelle, die von der DetailsView je verwendet die `Suppliers` DropDownList-Element ausgewählt. Um dies zu erreichen, erstellen Sie eine `SelectedIndexChanged` -Ereignishandler für das DropDownList Lieferanten. Dies kann am einfachsten durch Doppelklicken auf die Dropdownliste im Designer erstellt werden. Dieser Ereignishandler muss, um zu bestimmen, welche Datenquelle verwendet und muss erneut zu binden die Daten zur DetailsView. Dies erfolgt durch den folgenden Code:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Der Ereignishandler beginnt mit der bestimmt, ob die Option "Alle Lieferanten der anzeigen/bearbeiten" ausgewählt wurde. Wenn es war, wird die `SupplierDetails` DetailsView s `DataSourceID` zu `AllSuppliersDataSource` und den Benutzer auf den ersten Eintrag in der Menge der Lieferanten zurückgibt, durch Festlegen der `PageIndex` Eigenschaft auf 0. Wenn Sie jedoch der Benutzer ein bestimmter Lieferant aus der Dropdownliste das DetailsView-s ausgewählt hat `DataSourceID` zugewiesen `SingleSuppliersDataSource`. Unabhängig davon, welche Daten der Datenquelle verwendet wird, die `SuppliersDetails` Modus auf den nur-Lese Modus zurückgesetzt wird, und die Daten werden erneut gebunden, DetailsView durch einen Aufruf der `SuppliersDetails` Steuerelement s `DataBind()` Methode.

Mit diesem Ereignishandler vorhanden zeigt DetailsView-Steuerelement, wenn die Option "Alle Lieferanten der anzeigen/bearbeiten" ausgewählt wurde, in diesem Fall können alle Lieferanten über die Schnittstelle zur Paging angezeigt werden, nun den ausgewählten Lieferanten. Abbildung 9 zeigt die Seite die Option "Aller-Lieferanten anzeigen/bearbeiten" ausgewählt ist; Beachten Sie, dass die Paging-Schnittstelle vorhanden ist, damit der Benutzer besuchen und beliebige Zulieferer aktualisieren. Abbildung 10 zeigt die Seite mit den Ma Maison Lieferanten ausgewählt. Nur Informationen von Ma Maison, s ist in diesem Fall angezeigt und bearbeitet werden.


[![Alle Lieferanten Informationen kann angezeigt und bearbeitet werden](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Abbildung 9**: alle Lieferanten Informationen kann angezeigt werden und bearbeiteter ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Nur die ausgewählten s von Lieferanteninformationen kann angezeigt und bearbeitet werden](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Abbildung 10**: nur die ausgewählten Lieferanten s Informationen Ansicht "und" bearbeiteter kann ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> Für dieses Tutorial steuern sowohl der DropDownList und DetailsView s `EnableViewState` muss festgelegt werden, um `true` (Standard) da das DropDownList-s `SelectedIndex` und der s DetailsView `DataSourceID` eigenschaftenänderungen s müssen postbackübergreifend gespeichert werden.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Schritt 4: Auflisten der Lieferanten-Produkte in einem bearbeitbaren GridView-Ansicht

Mit der DetailsView abgeschlossen ist wird im nächsten Schritt, um eine bearbeitbare GridView einzuschließen, die diese Produkte von den ausgewählten Lieferanten bereitgestellte aufgeführt sind. Diese GridView vorsieht, Änderungen, um nur die `ProductName` und `QuantityPerUnit` Felder. Darüber hinaus ist der Benutzer auf der Seite von einem bestimmten anderen Lieferanten, es sollten nur zulassen Updates für Produkte, die *nicht* nicht mehr unterstützt. Um dies zu erreichen, wir zunächst hinzufügen, eine Überladung von müssen, der `ProductsBLL` s-Klasse `UpdateProducts` -Methode, die akzeptiert nur die `ProductID`, `ProductName`, und `QuantityPerUnit` Felder als Eingaben. Wir haben in Einzelschritten durchlaufen dieser Prozess im Voraus in zahlreichen Tutorials lassen s, die nur den Code hier betrachten die hinzugefügt werden sollen `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Mit dieser Überladung erstellt, es erneut bereit, das GridView-Steuerelement und seine zugehörigen "ObjectDataSource" hinzufügen. Fügen Sie einer neuen GridView-Ansicht auf der Seite hinzu, legen die `ID` Eigenschaft `ProductsBySupplier`, und konfigurieren sie eine neue, mit dem Namen "ObjectDataSource" Verwendung `ProductsBySupplierDataSource`. Da wir diese GridView diese Produkte von den ausgewählten Lieferanten auflisten möchten, verwenden die `ProductsBLL` Klasse s `GetProductsBySupplierID(supplierID)` Methode. Ordnen Sie auch die `Update()` Methode, um die neue `UpdateProduct` Überladung, die wir gerade erstellt haben.


[![Konfigurieren Sie die "ObjectDataSource", um die soeben erstellte UpdateProduct-Überladung verwenden](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Abbildung 11**: Konfigurieren Sie das "ObjectDataSource" Verwenden der `UpdateProduct` überladen gerade erstellt haben ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


Wir erneut aufgefordert, auszuwählen, die Parameterquelle für die `GetProductsBySupplierID(supplierID)` s-Methode `supplierID` input-Parameters. Da die Produkte angezeigt, für den Lieferanten in DetailsView, Verwendung ausgewählt werden sollen die `SuppliersDetails` DetailsView-Steuerelement s `SelectedValue` -Eigenschaft der Parameterquelle.


[![Verwenden Sie die SuppliersDetails DetailsView s SelectedValue-Eigenschaft, wie die Parameterquelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Abbildung 12**: Verwenden der `SuppliersDetails` DetailsView s `SelectedValue` -Eigenschaft der Quelle-Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Rückgabe an die GridView, entfernen Sie alle die GridView-Felder mit Ausnahme von `ProductName`, `QuantityPerUnit`, und `Discontinued`, markieren die `Discontinued` CheckBoxField als schreibgeschützt. Überprüfen Sie auch die Option "Bearbeiten aktivieren" das GridView-s-Smarttag. Nachdem diese Änderungen vorgenommen wurden, sollte der deklarative Markup für die GridView und "ObjectDataSource" etwa wie folgt aussehen:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Wie bei unseren vorherigen ObjectDataSources, diese einem s `OldValuesParameterFormatString` -Eigenschaftensatz auf `original_{0}`, wodurch Probleme beim Versuch, ein Produktname s oder eine Menge pro Einheit zu aktualisieren. Festlegen, dass die Standardeinstellung verwenden, oder entfernen Sie diese Eigenschaft vollständig aus der deklarativen Syntax `{0}`.

Mit dieser Konfiguration abgeschlossen ist, enthält unsere Seite jetzt Produkte von den Lieferanten, die in den GridView-Ansicht ausgewählt (siehe Abbildung 13). Derzeit *alle* Produktnamen s oder Menge pro Einheit kann aktualisiert werden. Allerdings müssen wir unsere Seitenlogik zu aktualisieren, sodass eine solche Funktionalität für ausgelaufene Produkte für Benutzer, die ein bestimmter Lieferant nicht zulässig ist. Wir werden diese letzte Information in Schritt 5 in Angriff nehmen.


[![Produkte von Lieferanten ausgewählt werden angezeigt.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Abbildung 13**: die Produkte von Lieferanten ausgewählt bereitgestellt werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> Durch das Hinzufügen dieser bearbeitbaren GridView der `Suppliers` DropDownList s `SelectedIndexChanged` Ereignishandler aktualisiert werden soll, um die GridView zu einem schreibgeschützten Zustand zurückzugeben. Andernfalls, wenn ein anderer Lieferanten mitten in Bearbeitung Produktinformationen ausgewählt ist, wird der zugehörige Index in der GridView-Ansicht für den neuen Lieferanten auch bearbeitet werden. Um dies zu verhindern, legen Sie einfach das GridView-s `EditIndex` Eigenschaft `-1` in die `SelectedIndexChanged` -Ereignishandler.


Darüber hinaus denken Sie daran, dass es wichtig, dass die GridView Ansichtszustand s werden (Standardverhalten) aktiviert. Setzen Sie das GridView-s `EnableViewState` Eigenschaft `false`, führen Sie das Risiko gleichzeitiger Benutzer versehentlich löschen oder zu bearbeiten zeichnet. Finden Sie unter [Warnung: Problem der Parallelität mit ASP.NET 2.0 GridViews/DetailsView/FormViews diese Unterstützung zu bearbeiten bzw. löschen und, deren Ansichtszustand deaktiviert ist](http://scottonwriting.net/sowblog/posts/10054.aspx) für Weitere Informationen.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Schritt 5: Zuzulassen bearbeiten, nicht mehr unterstützte Produkte bei anzeigen/bearbeiten alle Lieferanten nicht ausgewählt ist.

Während der `ProductsBySupplier` GridView ist voll funktionsfähig, es derzeit gewährt zu viel Zugriff auf die Benutzer, die von einem bestimmten Lieferanten sind. Pro unsere Geschäftsregeln sollten solche Benutzer nicht eingestellte Produkte aktualisieren können. Um dies zu erzwingen, können wir ausblenden (deaktivieren oder) die Schaltfläche "Bearbeiten" in den GridView Zeilen nicht mehr unterstützte Produkte, wenn die Seite von einem Lieferanten von einem Benutzer besucht wurde.

Erstellen Sie einen Ereignishandler für das GridView-s `RowDataBound` Ereignis. In diesem Ereignishandler müssen wir bestimmen, ob der Benutzer ein bestimmter Lieferant, zugeordnet, die für dieses Tutorial bestimmt werden kann ist, durch Überprüfen der Lieferanten DropDownList s `SelectedValue` -Eigenschaft: Wenn sie s anderes Element als -1, und klicken Sie dann auf der Benutzer ist ein bestimmter Lieferant zugeordnet ist. Für solche Benutzer müssen wir bestimmen, ob das Produkt nicht mehr unterstützt wird. Können wir ziehen Sie einen Verweis auf die tatsächliche `ProductRow` Instanz gebunden, auf die GridView-Zeile über die `e.Row.DataItem` -Eigenschaft, wie unter der [ *Zusammenfassungsinformationen anzeigen, in den GridView-s-Fußzeile* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) In diesem Tutorial. Wenn das Produkt nicht mehr lieferbar ist, können wir einen programmgesteuerten Verweis auf die Schaltfläche "Bearbeiten" in den GridView-s CommandField mit den Verfahren erläutert, die im vorherigen Tutorial nehmen [ *Hinzufügen der clientseitigen Bestätigung beim Löschen von* ](adding-client-side-confirmation-when-deleting-cs.md). Nachdem wir einen Verweis haben, den können wir dann ausblenden oder deaktivieren Sie die Schaltfläche.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Mit diesem Ereignis Handler vorhanden, wenn auf dieser Seite als Benutzer von einem bestimmten Lieferanten dieser Produkte, die nicht mehr unterstützt werden können nicht bearbeitet werden, wie die Schaltfläche "Bearbeiten" ist für diese Produkte ausgeblendet. Chef Anton s Gumbo Mix ist z. B. einem ausgelaufenen Produkt für den New Orleans Cajun lerne Lieferanten. Wenn die Seite für diesen bestimmten Lieferanten zu besuchen, wird die Schaltfläche "Bearbeiten" für dieses Produkt aus der Sicht ausgeblendet (siehe Abbildung 14). Allerdings ist die Schaltfläche "Bearbeiten" beim Zugriff auf "Anzeigen/bearbeiten alle Lieferanten" verwenden, zur Verfügung (siehe Abbildung 15).


[![Für Anbieter-spezifischen Benutzer wird die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix ausgeblendet.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Abbildung 14**: für Lieferanten-spezifische Benutzer die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix ausgeblendet ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Für alle Lieferanten Benutzer anzeigen/bearbeiten wird die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix angezeigt.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Abbildung 15**: für anzeigen/bearbeiten alle Lieferanten Benutzer, die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Testmischung wird angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Überprüfen auf die Zugriffsrechte in der Geschäftslogikebene

In diesem Tutorial ASP.NET Seite verarbeitet alle Logik im Hinblick auf die Informationen, die der Benutzer angezeigt und welche Produkte, die er aktualisieren kann. Im Idealfall würden diese Logik auch an der Geschäftslogikebene vorliegen. Z. B. die `SuppliersBLL` Klasse s `GetSuppliers()` -Methode (die alle Lieferanten zurückgibt) sind zum Beispiel eine Überprüfung aus, um sicherzustellen, dass der derzeit angemeldete Benutzer *nicht* ein bestimmter Lieferant zugeordnet. Ebenso die `UpdateSupplierAddress` -Methode kann eine Überprüfung aus, um sicherzustellen, dass den derzeit angemeldeten Benutzer entweder für unser Unternehmen hat (und daher können alle Lieferanten Adressinformationen zu aktualisieren) enthalten, oder den Hersteller, deren Daten aktualisiert wird, zugeordnet ist.

Ich haben solchen Überprüfungen aus BLL-Ebene hier nicht eingeschlossen, da in diesem Tutorial die Benutzerrechten s durch einem DropDownList-Steuerelement auf der Seite festgelegt werden, die nicht auf die BLL Klassen zugreifen können. Wenn Sie das Mitgliedschaftssystem oder eines Out-die mitgelieferten Authentifizierungsschemas, die von ASP.NET bereitgestellt wird (z. B. Windows-Authentifizierung), verwenden den angemeldeten Benutzer s Informationen und Informationen zu Rollen der BLL, wodurch dieser Zugriff zugegriffen werden kann Rechte überprüft, ob auf der Präsentation und BLL Ebenen möglich.

## <a name="summary"></a>Zusammenfassung

Die meisten Sites, die Benutzerkonten bereitstellen müssen die Schnittstelle auf Grundlage der angemeldete Benutzer Datenänderungsvorgänge anpassen. Administratoren möglicherweise ein Datensatz, bearbeiten und löschen können, während nicht-Administratoren beschränkt, nur aktualisieren oder Löschen von Datensätzen, die sie selbst erstellt werden können. Beliebiges anderes Szenario könnten sein, die Daten Websteuerelemente ObjectDataSource und Business Logic Layer-Klassen können erweitert werden, um hinzuzufügen oder zu verweigern bestimmte Funktionen basierend auf den angemeldeten Benutzer. In diesem Tutorial wurde erläutert, wie die Daten angezeigt und bearbeitet werden, je nachdem, ob der Benutzer ein bestimmter Lieferant zugeordnet wurde oder wenn sie sich für unser Unternehmen gearbeitet einzuschränken.

Dieses Lernprogramm schließt unserer Untersuchung der einfügen, aktualisieren und Löschen von Daten mithilfe der GridView, DetailsView oder FormView-Steuerelemente. Ab dem nächsten Tutorial, müssen wir uns dem Hinzufügen von Paginierung und Sortierung Unterstützung aktivieren.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](adding-client-side-confirmation-when-deleting-cs.md)
> [Weiter](an-overview-of-inserting-updating-and-deleting-data-vb.md)
