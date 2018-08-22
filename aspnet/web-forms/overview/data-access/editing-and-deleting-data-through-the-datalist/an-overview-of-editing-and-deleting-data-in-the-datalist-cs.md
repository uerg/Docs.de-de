---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Eine Übersicht über bearbeiten und Löschen von Daten im DataList-Steuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Beim DataList-Steuerelement integrierte bearbeiten und Löschen von Funktionen fehlen, werden in diesem Tutorial erfahren Sie, wie einem DataList-Steuerelement zu erstellen, unterstützt wird, bearbeiten und Löschen von o...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e9268a2ca805bfae2f77e72a131968e09a92b31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826711"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Eine Übersicht über bearbeiten und Löschen von Daten im DataList-Steuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) oder [PDF-Datei herunterladen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Beim DataList-Steuerelement integrierte bearbeiten und Löschen von Funktionen fehlen, werden in diesem Tutorial erfahren Sie, wie einem DataList-Steuerelement zu erstellen, unterstützt wird, bearbeiten und Löschen von der zugrunde liegenden Daten.


## <a name="introduction"></a>Einführung

In der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Tutorial erläutert, wie Sie einfügen, aktualisieren und Löschen von Daten mit der Anwendungsarchitektur, ein ObjectDataSource-Steuerelement, und die GridView, DetailsView und FormView -Steuerelemente. Mit dem ObjectDataSource-Steuerelement, und diese drei datenwebsteuerelemente implementieren Schnittstellen für einfache Änderung war ein Kinderspiel und Beteiligten Aktivieren eines Kontrollkästchens lediglich aus einem Smarttag. Kein Code geschrieben werden musste.

Leider fehlen bei DataList-Steuerelement die integrierte bearbeiten und Löschen von Funktionen, die inhärenten im GridView-Steuerelement. Diese fehlenden Funktionalität ist teilweise aufgrund der Tatsache, dass DataList-Steuerelement eine Relic aus der vorherigen Version von ASP.NET als deklarative Datenquellen-Steuerelemente und ohne Code Änderung Datenseiten nicht verfügbar waren. Während DataList-Steuerelement in ASP.NET 2.0 keine standardmäßig gleich Möglichkeiten zur Datenänderung als GridView anbieten, können wir ASP.NET 1.x-Techniken verwenden, um solche Funktionen einzuschließen. Dieser Ansatz erfordert ein paar Codezeilen, aber in diesem Tutorial sehen, DataList-Steuerelement verfügt über einige Ereignisse und Eigenschaften, die diesen Prozess zur Verfügung.

In diesem Tutorial sehen wir, wie Sie einem DataList-Steuerelement zu erstellen, die unterstützt werden, bearbeiten und Löschen von der zugrunde liegenden Daten. Erweiterte bearbeiten und Löschen von Szenarien, einschließlich der Validierung des Eingabefelds, ordnungsgemäß Behandeln von Ausnahmen, die ausgelöst wird, aus dem Datenzugriff oder Geschäftslogikschichten und So weiter, werden zukünftige Tutorials untersuchen.

> [!NOTE]
> Wie DataList-Steuerelement das Repeater-Steuerelement verfügt nicht über die Out-of Funktionen zum Einfügen, aktualisieren oder löschen. Während dieser Funktionalität hinzugefügt werden kann, enthält DataList-Steuerelement, Eigenschaften und Ereignisse, die nicht im Wiederholungsmodul gefunden, die das Hinzufügen von Funktionen vereinfachen. In diesem Tutorial und weiterer Nachrichten, die ansehen, bearbeiten und Löschen von konzentriert sich daher unbedingt auf DataList-Steuerelement.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Schritt 1: Erstellen, bearbeiten und Löschen von Tutorials Web Pages

Bevor wir beginnen, untersuchen zum Aktualisieren und Löschen von Daten aus einem DataList-Steuerelement, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Lernprogramm sowie die nächsten mehrere Explorer benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `EditDeleteDataList`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in die `EditDeleteDataList` Ordner werden in den Tutorials im Abschnitt aufgeführt. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Abschließend fügen Sie die Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach der Master/Detail-Berichte, mit dem DataList- und Wiederholungssteuerelement `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für DataList-Steuerelement bearbeiten und Löschen von Tutorials.


![Die Sitemap enthält jetzt die Einträge für DataList-Steuerelement bearbeiten und Löschen von Lernprogrammen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Abbildung 3**: die Sitemap enthält nun Einträge für DataList-Steuerelement bearbeiten und Löschen von Lernprogrammen


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Schritt 2: Untersuchen der Methoden für die aktualisieren und Löschen von Daten

Bearbeiten und Löschen von Daten mit GridView ist so einfach, da im Hintergrund GridView und "ObjectDataSource" zusammenarbeiten. Siehe die [Untersuchen der Ereignisse zugeordnet einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) Tutorial, wenn eine Zeile s-Schaltfläche "Aktualisieren" geklickt wird, weist die GridView automatisch die Felder, die bidirektionale Datenbindung an die verwendet`UpdateParameters` Auflistung von der "ObjectDataSource" und ruft dann "ObjectDataSource" s `Update()` Methode.

Leider stellt DataList-Steuerelement keine dieser integrierten Funktionen bereit. Es ist unsere Aufgabe, stellen Sie sicher, dass die "ObjectDataSource"-s-Parameter, und dass die Benutzer s Werte zugewiesen werden die `Update()` Methode wird aufgerufen. Um uns bei diesem Unterfangen zu unterstützen, bietet DataList-Steuerelement an die folgenden Eigenschaften und Ereignisse:

- **Die [ `DataKeyField` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  beim Aktualisieren oder löschen, müssen wir jedes Element im DataList-Steuerelement eindeutig identifizieren können. Legen Sie diese Eigenschaft, um das Feld für den Primärschlüssel der angezeigten Daten. Auf diese Weise füllt DataList-Steuerelement s [ `DataKeys` Auflistung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) mit dem angegebenen `DataKeyField` Wert für jedes DataList-Element.
- **Die [ `EditCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf Bearbeiten geklickt wird.
- **Die [ `CancelCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf "Abbrechen" geklickt wird.
- **Die [ `UpdateCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf Aktualisieren geklickt wird.
- **Die [ `DeleteCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf Löschen geklickt wird.

Mithilfe dieser Eigenschaften und Ereignisse werden vier Ansätze, die wir zum Aktualisieren und Löschen von Daten über DataList-Steuerelement verwenden können:

1. **Mithilfe von ASP.NET 1.x Techniken** DataList-Steuerelement vor ASP.NET 2.0 und ObjectDataSources vorhanden und konnte Daten vollständig auf programmgesteuerte Weise aktualisieren und löschen. Bei dieser Methode dem ObjectDataSource-Steuerelement vollständig Gräben und erfordert, dass wir die Daten direkt aus der Business Logic Layer, sowohl beim Abrufen der Daten zum Anzeigen an die Datenliste binden und beim Aktualisieren oder Löschen eines Datensatzes.
2. **Verwenden ein einzelnes ObjectDataSource-Steuerelement auf der Seite zum auswählen, aktualisieren und löschen** während DataList-Steuerelement die GridView s inhärenten bearbeiten und Löschen von Funktionen fehlen, gibt es s berichtsbenutzer können wir t hinzufügen in selbst. Bei diesem Ansatz wir eine "ObjectDataSource" wie in den GridView-Beispielen verwendet, jedoch müssen, erstellen Sie einen Ereignishandler für DataList-Steuerelement s `UpdateCommand` Ereignissatz, in dem wir das "ObjectDataSource"-s-Parameter, und rufen die `Update()` Methode.
3. **Verwenden ein ObjectDataSource-Steuerelement für auswählen, wird jedoch aktualisiert und das Löschen direkt gegen die BLL** bei Option 2 verwenden zu können, müssen wir ein paar Codezeilen in schreiben die `UpdateCommand` -Ereignis, das Zuweisen von Parameterwerten und so weiter. Stattdessen können wir bleiben Sie mit dem ObjectDataSource-Steuerelement für die Auswahl, aber diese aktualisieren und Löschen von Aufrufe direkt an die BLL (z. B. mit option 1). Meiner Meinung nach Schnittstellen direkt mit der BLL Aktualisieren von Daten führt zu besser lesbaren Code als das "ObjectDataSource"-s zuweisen `UpdateParameters` und das Aufrufen der `Update()` Methode.
4. **Mithilfe von deklarativen Weg, die über mehrere ObjectDataSources** die vorherigen drei Ansätze, die alle erfordern ein wenig Code. Wenn Sie d eher als viel deklarative Syntax wie möglich verwenden möchten weiterhin, werden die dritte mehrere ObjectDataSources auf der Seite enthalten. Der erste "ObjectDataSource" Ruft die Daten der BLL ab und bindet ihn an DataList-Steuerelement. Zum Aktualisieren, eine andere "ObjectDataSource" ist hinzugefügt, sondern direkt in der DataList s `EditItemTemplate`. Um einzuschließen, löschen die Unterstützung, noch eine andere "ObjectDataSource" notwendig werden der `ItemTemplate`. Bei diesem Ansatz, die diese eingebetteten "ObjectDataSource" Verwendung `ControlParameters` deklarativ die "ObjectDataSource"-s-Parameter an das Benutzereingabe-Steuerelemente gebunden werden (statt sie programmgesteuert im DataList-Steuerelement s angegeben `UpdateCommand` -Ereignishandler). Dieser Ansatz erfordert immer noch ein paar Codezeilen muss das eingebettete "ObjectDataSource"-s aufgerufen `Update()` oder `Delete()` -Befehl erfordert jedoch weit weniger als mit den anderen drei Ansätze. Der Nachteil ist, dass es sich bei dem mehrere ObjectDataSources Einrichten der Seite mit überflüssigen Daten gefüllt Ablenkung von den insgesamt besseren Lesbarkeit.

Wenn immer nur einen der folgenden Ansätze verwenden, wähle ich d Option 1 aus, da es die größte Flexibilität bietet und DataList-Steuerelement ursprünglich konzipiert wurde, um dieses Muster zu berücksichtigen. Beim DataList-Steuerelement mit dem Datenquellen-Steuerelemente ASP.NET 2.0 erweitert wurde, muss er nicht Features der offiziellen ASP.NET 2.0 Daten Websteuerelementen (die GridView, DetailsView und FormView-Steuerelement) oder alle der Erweiterungspunkte. Optionen für 2 bis 4 sind jedoch nicht ohne verdienen.

Dies und den zukünftigen bearbeiten und Löschen von Tutorials verwendet eine "ObjectDataSource" zum Abrufen der Daten anzeigen und direkte Aufrufe an die BLL, aktualisieren und Löschen von Daten (Option 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Schritt 3: DataList-Steuerelement hinzufügen und konfigurieren die "ObjectDataSource"

In diesem Tutorial erstellen wir einem DataList-Steuerelement, die Produktinformationen enthält und für jedes Produkt bietet dem Benutzer die Möglichkeit, um die Namen und den Preis zu bearbeiten und das Produkt vollständig zu löschen. Insbesondere, wir rufen die Datensätze zum Anzeigen der Nutzung einer ObjectDataSource gegeben, aber führen Sie das Update und Aktionen löschen, indem eine Schnittstelle direkt mit der BLL. Bevor wir die bearbeiten und Löschen von Funktionen, DataList-Steuerelement implementieren fürchten, können Sie s, rufen Sie zuerst auf die Seite für die Produkte in einer nur-Lese Schnittstelle. Da wir untersucht haben diese Schritte in vorherigen Tutorials ich werde gelangen sie schnell.

Öffnen Sie zunächst die `Basics.aspx` auf der Seite die `EditDeleteDataList` Ordner und in die Entwurfsansicht zu sehen, fügen Sie einem DataList-Steuerelement auf der Seite. Als Nächstes aus DataList s Smarttags, erstellen Sie eine neue "ObjectDataSource" an. Da wir mit Product-Daten arbeiten, so konfigurieren, verwenden Sie die `ProductsBLL` Klasse. Zum Abrufen *alle* Produkte, wählen Sie die `GetProducts()` -Methode in der Registerkarte "SELECT".


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL-Klasse](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Abbildung 4**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Zurückgeben der Produktinformationen mithilfe der GetProducts()-Methode](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Abbildung 5**: Zurückgeben von Informationen für das Produkt verwenden die `GetProducts()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList-Steuerelement, z. B. GridView ist nicht vorgesehen, für neue Daten eingefügt werden. Wählen Sie aus diesem Grund die (None) aus der Dropdown-Liste in der Registerkarte "Einfügen"-option. Nach Wunsch (keine) für die Update- und DELETE-Registerkarten, da die Updates und löschungen programmgesteuert über die BLL ausgeführt werden.


[![Vergewissern Sie sich, dass die Dropdownlisten in der "ObjectDataSource"-s-INSERT, UPDATE und Löschen von Registerkarten auf (keine) festgelegt sind](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Abbildung 6**: Vergewissern Sie sich, dass das Dropdown-Listen in den "ObjectDataSource" s INSERT, UPDATE, und Löschen von Registerkarten auf (keine) festgelegt sind ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


Klicken Sie nach der Konfiguration dem ObjectDataSource-Steuerelement auf "Fertig stellen", in den Designer zurückgeben. Als wir erstellt haben, die in früheren Beispielen angezeigt wird, wenn es sich bei die Konfiguration "ObjectDataSource" Visual Studio wird automatisch ergänzt einen `ItemTemplate` für DropDownList, Anzeigen aller Datenfelder. Ersetzen Sie dies `ItemTemplate` mit einer, in dem nur die s Produktnamen und der Preis angezeigt. Legen Sie außerdem die `RepeatColumns` Eigenschaft auf 2.

> [!NOTE]
> Wie unter der *Übersicht der einfügen, aktualisieren und Löschen von Daten* Tutorial beim Ändern von Daten mit dem ObjectDataSource-Steuerelement unserer Architektur erfordert, dass wir entfernen das `OldValuesParameterFormatString` Eigenschaft aus der "ObjectDataSource"-s deklaratives Markup (oder auf den Standardwert zurückgesetzt `{0}`). In diesem Tutorial werden wir jedoch dem ObjectDataSource-Steuerelement nur zum Abrufen von Daten verwenden. Daher wir müssen nicht so ändern Sie das "ObjectDataSource"-s `OldValuesParameterFormatString` Eigenschaftswert (obwohl es Schaden dazu).


Nach dem Ersetzen der standardmäßigen DataList `ItemTemplate` durch eine benutzerdefinierte-deklarative Markup auf der Seite sollte etwa wie folgt aussehen:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 7 dargestellt, zeigt DataList-Steuerelement den Produktpreis, Name und Unit für jedes Produkt in zwei Spalten an.


[![Die Namen der Produkte und Preise werden in einem zweispaltigen DataList-Steuerelement angezeigt.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Abbildung 7**: die Namen der Produkte und Preise werden in einem DataList-Steuerelement zwei Spalten angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> DataList-Steuerelement verfügt über eine Reihe von Eigenschaften, die für den Prozess aktualisieren und Löschen von erforderlich sind, und diese Werte im Ansichtszustand gespeichert werden. Wenn einem DataList-Steuerelement zu erstellen, bearbeiten oder Löschen von Daten unterstützt wird, ist es daher wichtig, dass der Ansichtszustand für DataList s aktiviert werden.  
>   
>  Der aufmerksame Leser möglicherweise Denken Sie daran, dass der Ansichtszustand deaktiviert beim Erstellen von bearbeitbaren GridViews DetailsViews und FormViews konnten wir. Dies ist, da ASP.NET 2.0-Webanwendung-Steuerelemente enthalten können *Steuerelementzustand*, persistenten Zustand wie der Ansichtszustand jedoch die angenommene Essential postbackübergreifend ist.


Deaktivieren die Ansicht in der GridView lediglich trivial Zustandsinformationen lässt, sondern behält den Zustand des Steuerelements (enthält den Status, die zum Bearbeiten und Löschen erforderlich). DataList-Steuerelement, in der ASP.NET 1.x Zeitrahmens erstellt worden muss Steuerelementzustand wird nicht verwendet und aus diesem Grund der Ansichtszustand aktiviert. Finden Sie unter [Steuerelementzustand Visual Studio. Ansichtszustand](https://msdn.microsoft.com/library/1whwt1k7.aspx) für Weitere Informationen zu den Zweck des Steuerelementzustands und wie es aus dem Ansichtszustand unterscheidet.

## <a name="step-4-adding-an-editing-user-interface"></a>Schritt 4: Hinzufügen einer Benutzeroberfläche zur Bearbeitung

Das GridView-Steuerelement besteht aus einer Auflistung von Feldern (BoundFields CheckBoxFields, von TemplateFields und So weiter). Diese Felder können ihre gerenderte Markup je nach Modus anpassen. Beispielsweise zeigt eine BoundField im schreibgeschützten Modus, der Datenfeldwert als Text; Wenn Sie sich im Bearbeitungsmodus befindet, die es rendert ein Textfeld-Web-Steuerelement, dessen `Text` Eigenschaft ist den Wert im Datenfeld zugewiesen.

DataList-Steuerelement, rendert andererseits, dessen Elemente, die mithilfe von Vorlagen. Schreibgeschützte Elemente werden mit gerendert. die `ItemTemplate` während Elemente in den Bearbeitungsmodus, über gerendert werden die `EditItemTemplate`. An diesem Punkt wurde unser DataList-Steuerelement nur ein `ItemTemplate`. Um Bearbeitungsfunktionen auf Elementebene unterstützen wir müssen ein `EditItemTemplate` , die das Markup enthält, für das bearbeitbare Element angezeigt werden soll. In diesem Tutorial verwenden wir TextBox-Websteuerelemente für die Bearbeitung des s Name und Unit Produktpreis.

Die `EditItemTemplate` kann entweder deklarativ oder mithilfe des Designers erstellt werden, (durch Auswählen der Option Vorlagen bearbeiten aus dem Smarttag DataList s). Um die Vorlagen bearbeiten-Option verwenden zu können, klicken Sie zuerst auf den Link Vorlagen bearbeiten, in das Smarttag, und wählen Sie dann die `EditItemTemplate` Element aus der Dropdown-Liste.


[![Für die Arbeit mit dem DataList-s-EditItemTemplate deaktivieren](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Abbildung 8**: Arbeiten mit dem DataList-Steuerelement s Opt `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Geben Sie als Nächstes Produktname: und den Preis: und ziehen Sie dann zwei TextBox-Steuerelemente aus der Toolbox in die `EditItemTemplate` Schnittstelle für den Designer. Legen Sie die Textfelder ein `ID` Eigenschaften `ProductName` und `UnitPrice`.


[![Fügen Sie ein Textfeld für die Product-s-Namen und Preise](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Abbildung 9**: Fügen Sie ein Textfeld für das Produkt s Name und Preis ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


Wir müssen die entsprechenden Product Feld Datenwerte binden die `Text` Eigenschaften die beiden Textfelder ein. Aus die Smarttags Textfelder ein, klicken Sie auf den Link "DataBindings bearbeiten" auf, und ordnen Sie dann die entsprechenden Datenfeld mit den `Text` -Eigenschaft, wie in Abbildung 10 dargestellt.

> [!NOTE]
> Beim Binden der `UnitPrice` Datenfeld mit dem Preis Textfeld s `Text` Feld, Sie können formatieren Sie ihn als Währungswert (`{0:C}`), eine Zahl im Standardzahlenformat (`{0:N}`), oder lassen sie nicht formatiert.


![Binden Sie ProductName und UnitPrice Datenfelder, die Texteigenschaften der TextBox-Elemente](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Abbildung 10**: Binden der `ProductName` und `UnitPrice` Daten Felder der `Text` Eigenschaften der TextBox-Elemente


Beachten Sie, wie in Abbildung 10 im Dialogfeld DataBindings bearbeiten *nicht* enthalten die bidirektionale Datenbindung-Kontrollkästchen, das beim Bearbeiten ein TemplateField im GridView oder DetailsView oder einer Vorlage in das FormView-Steuerelement vorhanden ist. Die bidirektionale Datenbindung-Funktion zulässig, den in der Web-Eingabesteuerelement automatisch der entsprechenden "ObjectDataSource"-s zugewiesen werden soll eingegebenen Wert `InsertParameters` oder `UpdateParameters` beim Einfügen oder Aktualisieren von Daten. DataList-Steuerelement unterstützt keine bidirektionale Datenbindung, später in diesem Tutorial nach dem wird Benutzer sehen ihrer Änderungen und bereit ist, um die Daten aktualisieren, müssen wir diese Textfelder programmgesteuerten Zugriff auf `Text` Eigenschaften und übergeben Sie deren Werte in der entsprechende `UpdateProduct` -Methode in der die `ProductsBLL` Klasse.

Abschließend müssen wir Updates hinzufügen und Schaltflächen zum Abbrechen der `EditItemTemplate`. Wie in beschrieben der [Master/Detail, verwenden eine Liste mit Aufzählungszeichen Liste der Masterdatensätze mit einem DataList-Steuerelement Details](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) Tutorial, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz aus geklickt wird, die innerhalb eines Repeater oder DataList-Steuerelement, das Repeater oder DataList s `ItemCommand` Ereignis wird ausgelöst. Für DataList-Steuerelement Wenn die `CommandName` Eigenschaft auf einen bestimmten Wert festgelegt ist, kann ein weiteres Ereignis ebenfalls ausgelöst werden. Die spezielle `CommandName` Eigenschaftswerte enthalten, u.a.:

- Abbrechen, löst die `CancelCommand` Ereignis
- Bearbeiten Sie löst die `EditCommand` Ereignis
- Aktualisieren Sie löst die `UpdateCommand` Ereignis

Beachten Sie, dass diese Ereignisse ausgelöst werden *zusätzlich zu* der `ItemCommand` Ereignis.

Hinzufügen der `EditItemTemplate` zwei Schaltfläche Websteuerelemente, eine, deren `CommandName` festgelegt ist, aktualisieren und die anderen s, legen Sie auf "Abbrechen". Nach dem Hinzufügen dieser zwei Schaltfläche Websteuerelemente sollte der Designer etwa wie folgt aussehen:


[![Hinzufügen von Updates und in das EditItemTemplate die Schaltflächen Abbrechen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Abbildung 11**: Hinzufügen Aktualisieren und die Schaltflächen Abbrechen, um die `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


Mit der `EditItemTemplate` vollständige sollte das deklarative DataList-s-Markup ähnlich dem folgenden aussehen:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Schritt 5: Hinzufügen, die Grundlagen, um den Bearbeitungsmodus wechseln

An diesem Punkt hat unsere DataList-Steuerelement eine bearbeitende-Schnittstelle definiert, die über die `EditItemTemplate`jedoch dort s derzeit keine Möglichkeit für einen Benutzer besuchen unsere Seite, um anzugeben, dass er eine s-Produktinformationen bearbeiten möchte. Wir müssen eine Bearbeiten-Schaltfläche, wenn geklickt, rendert, DataList-Steuerelement im Bearbeitungsmodus Element jedes Produkt hinzufügen. Starten Sie durch das Hinzufügen einer Schaltfläche "Bearbeiten", um die `ItemTemplate`, entweder mithilfe des Designers oder deklarativ. Sicher, dass die Schaltfläche "Bearbeiten" s festgelegt `CommandName` Eigenschaft zu bearbeiten.

Nachdem Sie diese Schaltfläche "Bearbeiten" hinzugefügt haben, können Sie die Seite über einen Browser anzuzeigen. Mit folgender Ergänzung sollte jedes aufgelistete Produkt eine Bearbeiten-Schaltfläche enthalten.


[![Hinzufügen von Updates und in das EditItemTemplate die Schaltflächen Abbrechen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Abbildung 12**: Hinzufügen Aktualisieren und die Schaltflächen Abbrechen, um die `EditItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Ein Postback auslöst, aber es wird auf die Schaltfläche *nicht* stellen Sie das Produkt in den Bearbeitungsmodus auflisten. Um das Produkt bearbeitbar zu machen, müssen Sie:

1. Legen Sie die DataList s [ `EditItemIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des dem `DataListItem` , deren Schaltfläche "Bearbeiten" geklickt wurde.
2. Binden Sie die Daten an die Datenliste. Wenn DataList-Steuerelement erneut gerendert wird, ist die `DataListItem` , deren `ItemIndex` DataList-Steuerelement s entspricht `EditItemIndex` rendert mit der `EditItemTemplate`.

Da DataList-Steuerelement s `EditCommand` Ereignis wird ausgelöst, wenn auf die Schaltfläche "Bearbeiten" geklickt wird, erstellen Sie eine `EditCommand` -Ereignishandler durch den folgenden Code:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

Die `EditCommand` -Ereignishandler wird in ein Objekt des Typs übergeben `DataListCommandEventArgs` als der zweite Eingabeparameter, die die enthält einen Verweis auf die `DataListItem` , deren Schaltfläche "Bearbeiten" geklickt wurde (`e.Item`). Legt die Ereignishandler zuerst DataList-Steuerelement s `EditItemIndex` auf die `ItemIndex` der bearbeitbaren `DataListItem` und klicken Sie dann die Bindung der Daten an die Datenliste durch Aufrufen der DataList s `DataBind()` Methode.

Rufen Sie nachdem dieser Ereignishandler hinzugefügt haben die Seite in einem Browser. Klicken Sie nun auf die Schaltfläche "Bearbeiten" wird die angeklickte Produkt bearbeitet werden (siehe Abbildung 13).


[![Klicken Sie auf die Schaltfläche klicken bearbeiten, werden das Produkt, das bearbeitet werden](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Abbildung 13**: Klicken Sie auf die Schaltfläche "Bearbeiten" wird die Produkt-bearbeitbare ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Schritt 6: Speichern der Benutzer-s-Änderungen

Klicken Sie auf die Abbrechen-Schaltflächen oder bearbeiteten Produkt s Update führt keine Aktion an diesem Punkt; Diese Funktionalität hinzufügen, wir zum Erstellen von Ereignishandlern für DataList-Steuerelement s müssen `UpdateCommand` und `CancelCommand` Ereignisse. Erstellen Sie zunächst die `CancelCommand` -Ereignishandler ausgeführt wird, wenn die bearbeitete-Produkt-s-Schaltfläche "Abbrechen" geklickt wird, und ihn damit beauftragt, DataList-Steuerelement in den Zustand vor der Bearbeitung zurückgeben.

Damit DataList-Steuerelement alle Elemente im schreibgeschützten Modus rendern, müssen Sie:

1. Legen Sie die DataList s [ `EditItemIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des eine nicht vorhandene `DataListItem` Index. `-1` ist Sie eine sichere Wahl, da die `DataListItem` Indizes beginnen bei `0`.
2. Binden Sie die Daten an die Datenliste. Da keine `DataListItem` `ItemIndex` es entsprechen, an die Datenliste s `EditItemIndex`, gesamte DataList-Steuerelement in einem nur-Lese Modus gerendert wird.

Diese Schritte können mit den folgenden Ereignishandlercode ausgeführt werden:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Mit folgender Ergänzung klicken Sie auf "Abbrechen" Schaltfläche gibt DataList-Steuerelement in den Zustand vor der Bearbeitung.

Ist der letzte Ereignishandler wir zum abschließen müssen der `UpdateCommand` -Ereignishandler. Dieser Ereignishandler muss:

1. Programmgesteuerten Zugriff auf den Namen des Produkts der freien Eingabe und Preis sowie das bearbeitete Produkt s `ProductID`.
2. Initiieren der Update-Vorgang durch Aufrufen der entsprechenden `UpdateProduct` überladen, der `ProductsBLL` Klasse.
3. Legen Sie die DataList s [ `EditItemIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des eine nicht vorhandene `DataListItem` Index. `-1` ist Sie eine sichere Wahl, da die `DataListItem` Indizes beginnen bei `0`.
4. Binden Sie die Daten an die Datenliste. Da keine `DataListItem` `ItemIndex` es entsprechen, an die Datenliste s `EditItemIndex`, gesamte DataList-Steuerelement in einem nur-Lese Modus gerendert wird.

Schritt 1 und 2 sind verantwortlich für das Speichern des Benutzers s Änderungen; Schritte 3 und 4 DataList-Steuerelement in den Zustand vor der Bearbeitung zurückzusetzen, nachdem die Änderungen gespeichert wurden und sind identisch mit den Schritten der `CancelCommand` -Ereignishandler.

Um die aktualisierten Produktnamen und den Preis zu erhalten, müssen wir verwenden die `FindControl` Methode, um programmgesteuert Verweis im TextBox-Web-innerhalb Steuerelemente der `EditItemTemplate`. Wir müssen auch die bearbeitete Produkt s abrufen `ProductID` Wert. Wenn wir zunächst dem ObjectDataSource-Steuerelement an die Datenliste gebunden ist, wird Visual Studio zugewiesen DataList-Steuerelement s `DataKeyField` -Eigenschaft auf dem Wert des Primärschlüssels aus der Datenquelle (`ProductID`). Dieser Wert kann dann von DataList-Steuerelement s abgerufen `DataKeys` Auflistung. Nehmen Sie einen Moment Zeit, um sicherzustellen, dass die `DataKeyField` -Eigenschaftensatz tatsächlich auf `ProductID`.

Der folgende Code implementiert die vier Schritte:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Der Ereignishandler beginnt, mit dem Lesen des bearbeiteten Produkts s `ProductID` aus der `DataKeys` Auflistung. Als Nächstes die beiden Textfelder ein, in der `EditItemTemplate` auf die verwiesen wird und ihre `Text` Eigenschaften, die in lokalen Variablen gespeichert `productNameValue` und `unitPriceValue`. Verwenden wir die `Decimal.Parse()` Methode, um den Wert von Lesen der `UnitPrice` Textfeld so, dass bei der eingegebene Wert ist ein Währungssymbol, er kann immer noch ordnungsgemäß konvertiert werden in einer `Decimal` Wert.

> [!NOTE]
> Die Werte aus der `ProductName` und `UnitPrice` Textfelder werden nur auf die "productnamevalue" und UnitPriceValue zugeordnet, wenn die Textfelder ein Text-Eigenschaften einen Wert angegeben haben. Andernfalls den Wert `Nothing` wird verwendet, für die Variablen, die beim Aktualisieren der Daten mit einer Datenbank, hat `NULL` Wert. Unser Code behandelt, also konvertiert Sie leere Zeichenfolgen in Datenbank `NULL` Werte, die das Standardverhalten der Bearbeitung Schnittstelle in der GridView, DetailsView oder FormView-Steuerelemente ist.


Nach dem Lesen der Werte, die `ProductsBLL` Klasse s `UpdateProduct` Methode aufgerufen wird, wird der Name des Produkts s übergeben, Preis, und `ProductID`. Der Ereignishandler abgeschlossen wird durch Zurückgeben von DataList-Steuerelement auf den vorab Bearbeitung Zustand mit genau derselben Logik wie in der `CancelCommand` -Ereignishandler.

Mit der `EditCommand`, `CancelCommand`, und `UpdateCommand` Ereignishandler abgeschlossen ist, ein Besucher kann die Namen und den Preis eines Produkts bearbeiten. Abbildung 14 bis 16 zeigen diese Bearbeitung Workflow in Aktion.


[![Werden beim ersten Besuch der Seite, die alle Produkte im schreibgeschützten Modus.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Abbildung 14**: Wenn die Seite zuerst besuchen zu können, werden alle Produkte im schreibgeschützten Modus ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Um ein Produkt s Namen oder den Preis zu aktualisieren, klicken Sie auf die Schaltfläche "Bearbeiten"](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Abbildung 15**: um ein Produkt s Namen oder den Preis zu aktualisieren, klicken Sie auf die Schaltfläche "Bearbeiten" ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Nach dem Ändern des Werts, klicken Sie auf Aktualisieren auf die Rückgabe in den schreibgeschützten Modus](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Abbildung 16**: nach dem Ändern des Werts, klicken Sie auf Updates auf die Rückgabe in den schreibgeschützten Modus ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Schritt 7: Hinzufügen von Delete-Funktionen

Die Schritte zum Hinzufügen von Delete-Funktionen zu einem DataList-Steuerelement ähneln denen für das Hinzufügen von Funktionen. Kurz gesagt, müssen wir eine Löschen-Schaltfläche zum Hinzufügen der `ItemTemplate` , die beim Klicken auf:

1. Liest das entsprechende Produkt s `ProductID` über die `DataKeys` Auflistung.
2. Führt den Löschvorgang durch Aufrufen der `ProductsBLL` Klasse s `DeleteProduct` Methode.
3. Bindet die Daten an die Datenliste erneut.

Lassen Sie s beginnen, indem eine Löschen-Schaltfläche zum Hinzufügen der `ItemTemplate`.

Beim Klicken auf eine Schaltfläche, deren `CommandName` bearbeiten, aktualisieren, ist, oder "Abbrechen", löst der DataList s `ItemCommand` Ereignis zusammen mit einem weiteren Ereignis (z. B., wenn Sie mit der Bearbeitung der `EditCommand` Ereignis wird ausgelöst, auch). Auf ähnliche Weise jede Schaltfläche LinkButton oder ImageButton im DataList-Steuerelement, dessen `CommandName` -Eigenschaftensatz auf Ursachen Löschen der `DeleteCommand` Ereignis ausgelöst (zusammen mit `ItemCommand`).

Fügen Sie eine Löschen-Schaltfläche neben der Schaltfläche "Bearbeiten" in der `ItemTemplate`wird durch das Festlegen der `CommandName` Eigenschaft zu löschen. Nach dem Hinzufügen dieser Schaltflächensteuerelement DataList s `ItemTemplate` deklarativer Syntax sollte wie folgt aussehen:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für das DataList s `DeleteCommand` Ereignis, mit dem folgenden Code:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Klicken Sie auf die Schaltfläche "löschen", ein Postback auslöst, und löst DataList-Steuerelement s `DeleteCommand` Ereignis. Im Ereignishandler, das auf den geklickt wurde Produkt s `ProductID` Wert erfolgt über die `DataKeys` Auflistung. Als Nächstes wird das Produkt gelöscht, durch den Aufruf der `ProductsBLL` Klasse s `DeleteProduct` Methode.

Nach dem Löschen des Produkts, es wichtig, dass wir die Daten an die Datenliste binden (`DataList1.DataBind()`), andernfalls DataList-Steuerelement weiterhin die Produktkategorie angezeigt, die gerade gelöscht wurde.

## <a name="summary"></a>Zusammenfassung

Beim DataList-Steuerelement verfügt nicht über den Punkt, und klicken Sie auf Bearbeiten und Löschen von GridView zusammenarbeitsmöglichkeiten unterstützen, kann ein kurzes Stück Code es erweitert werden diese Funktionen zur typunterstützung. In diesem Tutorial wurde erläutert, wie erstellen Sie eine zweispaltige Liste von Produkten, die gelöscht werden konnten und, dessen Name und Preis bearbeitet werden können. Hinzufügen, bearbeiten und Löschen von Unterstützung wird auch die entsprechenden Web in der `ItemTemplate` und `EditItemTemplate`, die entsprechenden Ereignishandler erstellen, lesen Sie die Schlüsselwerte der freien Eingabe und die primäre und eine Schnittstelle mit dem Unternehmen Logic-Ebene.

Während wir einfache bearbeiten und Löschen von Funktionen, um DataList-Steuerelement hinzugefügt haben, verfügt es nicht über erweiterte Features. Beispielsweise ist keine Eingabefeld Validierung – Wenn ein Benutzer eingibt, einen Preis von zu viel Leistung beanspruchen, eine Ausnahme wird ausgelöst durch `Decimal.Parse` beim Versuch, eine Konvertierung zu teuer in einer `Decimal`. Auf ähnliche Weise, wenn ein Problem vorliegt, aktualisieren Sie die Daten auf die Geschäftslogik oder Datenzugriffsschichten erhält der Benutzer mit dem standardmäßigen Fehlerbildschirm sein. Ohne jegliche Bestätigung auf die Schaltfläche Löschen ist ein versehentliches Löschen von einem Produkt alles sehr wahrscheinlich, dass.

In Zukunft treten Lernprogramme, die wir zur Verbesserung des Bearbeitung Benutzers sehen werden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Zack Jones, Ken Pespisa und Randy Schmidt. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](performing-batch-updates-cs.md)
