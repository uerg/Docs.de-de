---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: "Eine Übersicht über bearbeiten und Löschen von Daten in das DataList (c#) | Microsoft Docs"
author: rick-anderson
description: "Während der DataList integrierte bearbeiten und Löschen von Funktionen fehlen, werden in diesem Lernprogramm erfahren Sie, wie DataList erstellen, unterstützt wird, bearbeiten und Löschen von o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c7a1c7a9839f2f56658618958c234e0064cb427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Eine Übersicht über bearbeiten und Löschen von Daten in das DataList (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) oder [PDF herunterladen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Während der DataList integrierte bearbeiten und Löschen von Funktionen fehlen, in diesem Lernprogramm sehen wie DataList erstellt, die unterstützt werden, bearbeiten und Löschen von der zugrunde liegenden Daten.


## <a name="introduction"></a>Einführung

In der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogramm erläutert, wie einfügen, aktualisieren und Löschen von Daten mithilfe der Anwendungsarchitektur, einem ObjectDataSource und GridView, DetailsView und FormView Steuerelemente. Mit der ObjectDataSource und diese drei Data-Websteuerelemente wurde ein Kinderspiel und Aktivieren eines Kontrollkästchens lediglich aus einem smart Tag beteiligt einfache Änderung von Schnittstellen implementieren. Kein Code erforderlich, um geschrieben werden.

Leider fehlt das DataList integrierten bearbeiten und Löschen von Funktionen, die Bestandteil des GridView-Steuerelements. Diese fehlenden Funktionalität ist teilweise aufgrund der Tatsache, dass das DataList ein New Relic aus der vorherigen Version von ASP.NET verwenden, wenn deklarative Datenquellen-Steuerelementen und Code frei Änderung Datenseiten nicht verfügbar waren. Während DataList in ASP.NET 2.0 nicht die gleiche ausgegeben Möglichkeiten zur Datenänderung als GridView anbieten, können wir ASP.NET 1.x Techniken verwenden, um solche Funktionen einzuschließen. Dieser Ansatz erfordert viel Code, wie wir in diesem Lernprogramm sehen werden, die DataList haben jedoch bestimmte Ereignisse und Eigenschaften vorhanden, die in diesem Prozess zu unterstützen.

In diesem Lernprogramm sehen wir, wie eine DataList erstellt, die unterstützt werden, bearbeiten und Löschen von der zugrunde liegenden Daten. Erweiterte bearbeiten und Löschen von Szenarien, einschließlich Eingabefeld Validierung, ordnungsgemäß Behandeln von Ausnahmen, die ausgelöst wird, aus der Datenzugriff oder Logik Geschäftsschichten usw. untersucht zukünftige Lernprogrammen.

> [!NOTE]
> Wie das DataList Wiederholungsmodul-Steuerelement verfügt nicht über die Out-of Funktionen zum Einfügen, aktualisieren oder löschen. Während solche Funktionen hinzugefügt werden kann, enthält das DataList Eigenschaften und Ereignisse, die nicht im Wiederholungsmodul gefunden, die Hinzufügen von Funktionen zu vereinfachen. Dieses Lernprogramm und zukünftige Spalten, die bearbeiten und Löschen von betrachten konzentriert sich daher unbedingt auf DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Schritt 1: Erstellen, bearbeiten und Löschen von Lernprogramme Web Pages

Bevor wir beginnen, zum Aktualisieren und Löschen von Daten aus einem DataList untersuchen, können Sie s, schalten Sie zuerst einen Moment Zeit, die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm und weiter mehrere diejenigen benötigen. Starten, indem Sie einen neuen Ordner namens `EditDeleteDataList`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

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


Wie in den anderen Ordnern `Default.aspx` in den `EditDeleteDataList` Ordner sind Lernprogramme im Abschnitt aufgeführt. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Fügen Sie abschließend die Seiten hinzu, als Einträge an die `Web.sitemap` Datei. Insbesondere das folgende Markup nach dem Master-/Detail-Berichte hinzufügen, mit dem DataList und Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält die Elemente jetzt für das DataList bearbeiten und Löschen von Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für die DataList bearbeiten und Löschen von Lernprogrammen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Abbildung 3**: die Siteübersicht enthält jetzt die Einträge für die DataList bearbeiten und Löschen von Lernprogrammen


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Schritt 2: Untersuchen von Techniken für aktualisieren und Löschen von Daten

Bearbeiten und Löschen von Daten mit GridView ist so einfach, da im Hintergrund der GridView und ObjectDataSource zusammen funktionieren. Entsprechend der Anleitung unter dem [untersuchen die Ereignisse zugeordneten einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) Lernprogramm, wenn eine Zeile s-Schaltfläche "Aktualisieren" geklickt wird, weist die GridView automatisch ihre Felder, die bidirektionale Datenbindung, die verwendet`UpdateParameters` Auflistung von seiner ObjectDataSource und anschließend aufgerufen, s ObjectDataSource `Update()` Methode.

Leider stellt das DataList keine keines dieser integrierten Funktionen bereit. Ist dafür verantwortlich, um sicherzustellen, dass das ObjectDataSource-s-Parameter, und dass der s Benutzerwerte zugewiesen werden seine `Update()` -Methode aufgerufen wird. Um uns diese Aufgabe zu erleichtern, stellt das DataList die folgenden Eigenschaften und Ereignisse:

- **Die [ `DataKeyField` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  beim Aktualisieren oder löschen, müssen wir jedes Element in der DataList eindeutig identifizieren können. Legen Sie diese Eigenschaft, um das Feld für den Primärschlüssel der angezeigten Daten. Auf diese Weise wird das DataList s Auffüllen [ `DataKeys` Auflistung](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) mit dem angegebenen `DataKeyField` Wert für jedes DataList-Element.
- **Die [ `EditCommand` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf Bearbeiten geklickt wird.
- **Die [ `CancelCommand` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf "Abbrechen" geklickt wird.
- **Die [ `UpdateCommand` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf Aktualisieren geklickt wird.
- **Die [ `DeleteCommand` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  wird ausgelöst, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` -Eigenschaftensatz auf Delete geklickt wird.

Verwenden diese Eigenschaften und Ereignisse, Sie stehen vier Möglichkeiten, die zum Aktualisieren und Löschen von Daten aus der DataList verwendet werden können:

1. **Mithilfe von ASP.NET 1.x Techniken** DataList vorhanden waren, bevor Sie ASP.NET 2.0 und ObjectDataSources und zu aktualisieren und Löschen von Daten vollständig durch programmgesteuerte Möglichkeit werden konnte. Dieses Verfahren der ObjectDataSource vollständig Gräben und setzt voraus, dass wir die Daten direkt aus der Business Logic Layer, sowohl beim Abrufen der Daten zum Anzeigen an DataList binden und beim Aktualisieren oder Löschen eines Datensatzes.
2. **Mit einem einzelnen ObjectDataSource-Steuerelement auf der Seite zum auswählen, aktualisieren und löschen** während DataList die GridView s inhärenten bearbeiten und Löschen von Funktionen fehlen, dort s keinen Grund, wir können t hinzufügen in uns. Bei diesem Ansatz wir verwenden eine ObjectDataSource ebenso wie in den Beispielen GridView. allerdings müssen einen Ereignishandler für das DataList s erstellen `UpdateCommand` Ereignissatz, auf dem wir das ObjectDataSource-s-Parameter, und rufen die `Update()` Methode.
3. **Mit einem ObjectDataSource-Steuerelement für auswählen, aber aktualisieren und Löschen von direkt gegen die BLL** bei der Verwendung der Option 2 müssen wir schreiben viel Code in die `UpdateCommand` Ereignis, das Zuweisen von Parameterwerten und so weiter. Wir stattdessen, können bleiben Sie mit der Verwendung der ObjectDataSource für die Auswahl, aber die aktualisieren und Löschen von Aufrufe direkt gegen die BLL (z. B. mit option 1). In meiner Meinung nach Aktualisierung von Daten direkt mit der BLL Schnittstellenfunktion führt zu einer besser lesbaren Code als das ObjectDataSource-s zuweisen `UpdateParameters` und Aufrufen seiner `Update()` Methode.
4. **Verwenden deklarative Weise über mehrere ObjectDataSources** die vorherigen drei Ansätze, die alle erfordern ein wenig Code. Wenn Sie d stattdessen als möglichst viel deklarative Syntax verwenden möchten weiterhin, wird eine letzte Option mehrere ObjectDataSources auf der Seite enthalten. Die erste ObjectDataSource ruft Daten aus der BLL ab und bindet sie an das DataList. Für die Aktualisierung einer anderen ObjectDataSource ist hinzugefügt, sondern direkt in der DataList s `EditItemTemplate`. Einbeziehung der Unterstützung löschen noch einen anderen ObjectDataSource erforderlich der `ItemTemplate`. Bei diesem Ansatz diese ObjectDataSource s verwenden eingebettete `ControlParameters` deklarativ ObjectDataSource-s-Parameter an die Benutzer-Eingabesteuerelemente gebunden (anstatt programmgesteuert DataList s einhalten `UpdateCommand` Ereignishandler). Dieser Ansatz erfordert immer noch viel Code aufrufen, die eingebettete ObjectDataSource s müssen `Update()` oder `Delete()` -Befehl erfordert jedoch wesentlich kleiner als mit den anderen drei Ansätze. Der Nachteil daran ist, dass mehrere ObjectDataSources die Seite überflüssigen Daten gefüllt vom Vordergrundtext aus der gesamten Lesbarkeit.

Wenn gezwungen, immer nur einen der folgenden Ansätze verwenden, wählen Sie ich d Option 1, da es die größte Flexibilität bietet und DataList ursprünglich entworfen wurde, um dieses Muster zu berücksichtigen. Während der DataList Zusammenarbeit mit ASP.NET 2.0-Datenquellensteuerelemente erweitert wurde, muss er nicht Features der offiziellen ASP.NET 2.0 Daten Websteuerelemente (GridView, DetailsView und FormView) oder alle der Erweiterungspunkte. Optionen 2 bis 4 sind jedoch nicht ohne Wert.

Dies und den zukünftigen bearbeiten und Löschen von Lernprogramme verwenden eine ObjectDataSource zum Abrufen der Daten anzeigen und direkte Aufrufe an die BLL zu aktualisieren und Löschen von Daten (Option 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Schritt 3: Hinzufügen von DataList und seine ObjectDataSource konfigurieren

In diesem Lernprogramm erstellen wir ein DataList, die Produktinformationen enthält, und für jedes Produkt bietet dem Benutzer die Möglichkeit, den Namen und den Preis bearbeiten und auf das Produkt vollständig zu löschen. Insbesondere wird die Datensätze abrufen, um mithilfe von einem ObjectDataSource anzuzeigen, aber das Update und Löschaktionen von Schnittstellenfunktion direkt mit der BLL. Bevor wir implementieren die bearbeiten und Löschen von Funktionen, um die DataList kümmern, können Sie s erhalten zunächst die Seite, um die Produkte in einer nur-Lese Oberfläche anzuzeigen. Seit wir untersucht Ve Schritte im vorherigen Lernprogrammen ich werde gelangen sie schnell.

Öffnen Sie zunächst die `Basics.aspx` auf der Seite der `EditDeleteDataList` Ordner, und fügen Sie in der Entwurfsansicht ein DataList auf der Seite. Als Nächstes aus den DataList s smart Tag, erstellen Sie eine neue ObjectDataSource an. Da wir mit Produktdaten arbeiten, so konfigurieren, verwenden Sie die `ProductsBLL` Klasse. Abzurufenden *alle* Produkte, wählen Sie die `GetProducts()` Methode in der Registerkarte "SELECT".


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL-Klasse](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Zurückgeben der Produktinformationen mithilfe der GetProducts()-Methode](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Abbildung 5**: Zurückgeben von Informationen für das Produkt verwenden die `GetProducts()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList, wie die GridView dient nicht zum Einfügen von neuen Daten. Wählen Sie aus diesem Grund option (keine) aus der Dropdown-Liste in der Registerkarte "Einfügen". Nach Wunsch (keine) für Update- und DELETE-Registerkarten, da die Updates und Löschvorgänge programmgesteuert über die BLL ausgeführt werden.


[![Vergewissern Sie sich, dass die Dropdownlisten in ObjectDataSource s, INSERT-, Update- und Registerkarten löschen (keine) eingestellt sind](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Abbildung 6**: Vergewissern Sie sich, dass das Dropdown-Listen in ObjectDataSource s einfügen, aktualisieren und Löschen von Registerkarten auf (keine) festgelegt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


Nach dem Konfigurieren der ObjectDataSource, klicken Sie auf "Fertig stellen", in den Designer zurückgeben. Wir haben, die in früheren Beispielen angezeigt wird, wenn das ObjectDataSource-Konfiguration, Visual Studio automatisch abgeschlossen wird erstellt eine `ItemTemplate` für der DropDownList Anzeige der Datenfelder. Ersetzen Sie diesen `ItemTemplate` mit einem, in dem nur die Produktnamen s und Preis angezeigt. Legen Sie außerdem die `RepeatColumns` -Eigenschaft auf 2.

> [!NOTE]
> Wie in beschrieben die *Übersicht des einfügen, aktualisieren und Löschen von Daten* Lernprogramm, das beim Ändern von Daten mithilfe der ObjectDataSource unsere Architektur erfordert, dass wir Entfernen der `OldValuesParameterFormatString` Eigenschaft über das ObjectDataSource-s deklarativem Markup (oder auf den Standardwert zurückgesetzt `{0}`). In diesem Lernprogramm werden wir jedoch die ObjectDataSource nur zum Abrufen von Daten verwenden. Daher wir müssen nicht so ändern Sie die s ObjectDataSource `OldValuesParameterFormatString` Eigenschaftswert (obwohl es ist nicht dazu Schaden).


Nach dem Ersetzen der Standardeinstellung DataList `ItemTemplate` durch eine benutzerdefinierte-deklarative Markup auf der Seite sollte ähnlich der folgenden aussehen:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt über einen Browser anzuzeigen. Wie Abbildung 7 zeigt, zeigt das DataList den Produktpreis Name und Komponententests für jedes Produkt in zwei Spalten an.


[![Die Namen der Produkte und Preise werden in einem zweispaltigen DataList angezeigt.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Abbildung 7**: die Namen der Produkte und Preise werden in einem zweispaltigen DataList angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> DataList verfügt über eine Reihe von Eigenschaften, die für den Prozess zum Aktualisieren und Löschen von erforderlich sind, und diese Werte werden im Ansichtszustand gespeichert. Daher unterstützt Sie DataList erstellen, bearbeiten oder Löschen von Daten, ist es wichtig, dass das DataList-s-Ansichtszustand aktiviert werden.  
>   
>  Der aufmerksame Leser möglicherweise Beachten Sie, dass wir Ansichtszustand zu deaktivieren, wenn bearbeitbaren GridViews, DetailsViews und FormViews erstellen konnten. Dies ist, da ASP.NET 2.0-Websteuerelemente enthalten können *Steuerelementzustand*, welche persistenten Zustand über Postbacks wie Ansichtszustand sondern angenommene Essential ist.


Deaktivieren die Ansicht Status in der GridView lediglich triviale Zustandsinformationen ausgelassen, aber verwaltet den Zustand des Steuerelements (einschließlich den Status für bearbeiten und Löschen erforderlich). DataList, dass in den ASP.NET 1.x Zeitrahmen erstellt wurde Steuerelementzustand wird nicht verwendet und benötigen daher Ansichtszustand aktiviert. Finden Sie unter [Steuerelementzustand Vs. Ansichtszustand](https://msdn.microsoft.com/en-us/library/1whwt1k7.aspx) für Weitere Informationen zu den Zweck der Steuerelementzustand und wie es Ansichtszustand unterscheidet.

## <a name="step-4-adding-an-editing-user-interface"></a>Schritt 4: Hinzufügen einer Benutzeroberfläche zur Bearbeitung

GridView-Steuerelement besteht aus einer Auflistung von Feldern (BoundFields, CheckBoxFields von TemplateFields und So weiter). Diese Felder können ihre gerenderten Markups in Abhängigkeit ihrer Modus anpassen. Beispielsweise zeigt ein BoundField seine Datenfeldwert im Nur-Lese Modus als Text; Wenn in den Bearbeitungsmodus wechseln, rendert sie ein TextBox-Web-Steuerelement, dessen `Text` Eigenschaft ist den Wert des Felds zugewiesen.

Das DataList rendert andererseits, dessen Elemente mithilfe von Vorlagen. Schreibgeschützte Elemente mit gerendert werden die `ItemTemplate` während Elemente in den Bearbeitungsmodus, über gerendert werden die `EditItemTemplate`. An diesem Punkt ist unsere DataList nur ein `ItemTemplate`. Um Bearbeitungsfunktionen Elementebene unterstützen wir müssen einen `EditItemTemplate` , die das Markup enthält, für die bearbeitbaren Element angezeigt werden soll. Für dieses Lernprogramm verwenden wir TextBox-Websteuerelemente zum Bearbeiten des s Name und Unit Produktpreis.

Die `EditItemTemplate` kann entweder deklarativ oder über den Designer erstellt werden, (durch Auswählen der Option Vorlagen bearbeiten aus dem Smarttag DataList s). Um die Vorlagen bearbeiten können, klicken Sie zuerst auf den Link Vorlagen bearbeiten, in das Smarttag, und wählen Sie dann die `EditItemTemplate` Element aus der Dropdown-Liste.


[![Arbeiten mit ItemTemplate s DataList abonnieren](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Abbildung 8**: Arbeiten mit DataList s Opt `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Geben Sie als Nächstes in Produktname: und Preis: und ziehen Sie dann zwei TextBox-Steuerelemente aus der Toolbox in die `EditItemTemplate` Schnittstelle des Designers. Legen Sie die Textfelder ein `ID` Eigenschaften `ProductName` und `UnitPrice`.


[![Fügen Sie ein Textfeld für die s-Produktname und Preis](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Abbildung 9**: Fügen Sie ein Textfeld für das Produkt s Namen und den Preis ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


Wir müssen entsprechende Produkt Datenfeldwerte zum Binden der `Text` Eigenschaften für die beiden Textfelder ein. Smart Tags für die Textfelder ein, klicken Sie auf den Link-Datenbindungen bearbeiten, und ordnen Sie dann das entsprechende Feld mit der `Text` -Eigenschaft verwenden, wie in Abbildung 10.

> [!NOTE]
> Beim Binden der `UnitPrice` Feld "Daten" auf den Preis TextBox s `Text` Feld, Sie können formatieren Sie ihn als Währungswert (`{0:C}`), eine allgemeine Zahl (`{0:N}`), oder lassen Sie ihn nicht formatiert.


![Binden Sie um die Eigenschaften von Text für die Textfelder ein ProductName und UnitPrice Datenfelder](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Abbildung 10**: Binden der `ProductName` und `UnitPrice` Datenfelder die `Text` Eigenschaften der Textfelder


Beachten Sie, wie in Abbildung 10 im Dialogfeld DataBindings bearbeiten funktioniert *nicht* enthalten die bidirektionale Datenbindung-Kontrollkästchen, das beim Bearbeiten einer TemplateField GridView oder DetailsView oder einer Vorlage in der FormView vorhanden ist. Die bidirektionale Datenbindung-Funktion zulässig, den Wert eingegeben werden, in das Eingabesteuerelement Web, automatisch mit dem entsprechenden ObjectDataSource s zugewiesen werden `InsertParameters` oder `UpdateParameters` beim Einfügen oder Aktualisieren von Daten. DataList unterstützt keine bidirektionale Datenbindung, weiter unten in diesem Lernprogramm nach dem Benutzer wird eingehendem ihr ändert und bereit ist, die Daten zu aktualisieren, müssen wir diese Textfelder programmgesteuerten Zugriff auf `Text` Eigenschaften und übergeben Sie deren Werte in der entsprechende `UpdateProduct` Methode in der `ProductsBLL` Klasse.

Schließlich müssen hinzuzufügende Update und Schaltflächen zum Abbrechen der `EditItemTemplate`. Wie wir gesehen, in haben der [Master/Detail, verwenden eine Aufzählung der Master-Datensätze mit Details DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) Lernprogramm, wenn eine Schaltfläche, LinkButton oder ImageButton, deren `CommandName` festgelegt wird, die von innerhalb einer Repeater oder DataList, geklickt wird die Repeater oder DataList s `ItemCommand` Ereignis wird ausgelöst. Für das DataList Wenn die `CommandName` Eigenschaft auf einen bestimmten Wert festgelegt ist, kann auch ein zusätzliches Ereignis ausgelöst werden. Die speziellen `CommandName` Eigenschaftswerte zählen u. a.:

- "Abbrechen" löst die `CancelCommand` Ereignis
- Bearbeiten Sie löst die `EditCommand` Ereignis
- Aktualisieren Sie löst die `UpdateCommand` Ereignis

Beachten Sie, dass diese Ereignisse eintreten *zusätzlich zu* der `ItemCommand` Ereignis.

Hinzufügen der `EditItemTemplate` zwei Schaltfläche Websteuerelemente, ein, deren `CommandName` , Update- und die anderen s, legen Sie auf "Abbrechen" festgelegt ist. Nach dem Hinzufügen dieser zwei Schaltfläche Websteuerelemente sollte der Designer etwa wie folgt aussehen:


[![Hinzufügen von Updates und ItemTemplate die Schaltflächen Abbrechen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Abbildung 11**: Update hinzufügen und die Schaltflächen "Abbrechen", um die `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


Mit der `EditItemTemplate` abgeschlossen sollte DataList s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Schritt 5: Hinzufügen der Aufgaben, die um den Bearbeitungsmodus wechseln

An diesem Punkt hat unsere DataList bearbeitende über definierte Schnittstelle seine `EditItemTemplate`jedoch dort s derzeit keine Möglichkeit für einen Benutzer besuchen unsere-Seite, um anzugeben, dass er eine s-Produktinformationen bearbeiten möchte. Wir müssen eine Schaltfläche "Bearbeiten", dass beim Klicken auf rendert, DataList im Bearbeitungsmodus Element jedes Produkt hinzufügen. Starten Sie durch Hinzufügen einer Schaltfläche "Bearbeiten", um die `ItemTemplate`, durch den Designer oder deklarativ. Sicher, legen Sie die Schaltfläche "Bearbeiten" s `CommandName` Eigenschaft zu bearbeiten.

Nachdem Sie diese Schaltfläche "Bearbeiten" hinzugefügt haben, nehmen Sie einen Moment Zeit, um die Seite über einen Browser anzeigen. Durch diesen Zusatz sollte jedes aufgelistete Produkt eine Schaltfläche "Bearbeiten" enthalten.


[![Hinzufügen von Updates und ItemTemplate die Schaltflächen Abbrechen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Abbildung 12**: Update hinzufügen und die Schaltflächen "Abbrechen", um die `EditItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Klicken auf die Schaltfläche bewirkt, dass einen Postback im Gegensatz zu *nicht* die Produktliste in den Bearbeitungsmodus schalten. Damit das Produkt bearbeitet werden kann, muss Folgendes:

1. Legen Sie die DataList s [ `EditItemIndex` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des der `DataListItem` , deren Schaltfläche "Bearbeiten" geklickt wurde.
2. Binden Sie die Daten an die Datenliste. Wenn DataList neu gerendert, wird die `DataListItem` , deren `ItemIndex` DataList s entspricht `EditItemIndex` mit gerendert wird seine `EditItemTemplate`.

Seit der DataList s `EditCommand` Ereignis wird ausgelöst, wenn auf die Schaltfläche "Bearbeiten" geklickt wird, erstellen Sie eine `EditCommand` -Ereignishandler durch den folgenden Code:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

Die `EditCommand` -Ereignishandler übergeben wird, in ein Objekt des Typs `DataListCommandEventArgs` als der zweite Eingabeparameter, die die enthält einen Verweis auf die `DataListItem` , deren Schaltfläche "Bearbeiten" geklickt wurde (`e.Item`). Der Ereignishandler legt zunächst die DataList s `EditItemIndex` auf die `ItemIndex` von die bearbeitbare `DataListItem` , und klicken Sie dann die Daten an die Datenliste durch Aufrufen der DataList s bindet `DataBind()` Methode.

Wenn Ereignishandler hinzugefügt haben, rufen Sie die Seite in einem Browser aus. Klicken Sie nun auf die Schaltfläche "Bearbeiten" wird das Produkt geklickt wurde bearbeitet werden kann (siehe Abbildung 13).


[![Klicken mit dem Bearbeiten Schaltfläche wird das Produkt, die bearbeitet werden kann](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Abbildung 13**: auf die Schaltfläche "Bearbeiten" bearbeitbar das Produkt ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Schritt 6: Speichern der Benutzer-s-Änderungen

Klicken Sie auf die bearbeitete Produkt s Update oder "Abbrechen"-Schaltflächen nichts an diesem Punkt; Diese Funktionalität hinzufügen, müssen wir die Ereignishandler für das DataList s erstellen `UpdateCommand` und `CancelCommand` Ereignisse. Erstellen Sie zunächst die `CancelCommand` -Ereignishandler ausgeführt wird, wenn die bearbeitete Produkt s-Schaltfläche "Abbrechen" geklickt wird, und ihn damit beauftragt, bei der Rückgabe das DataList Datenbankzustands vorab bearbeiten.

Damit das DataList sämtliche Elemente in den schreibgeschützten Modus rendern, müssen wir:

1. Legen Sie die DataList s [ `EditItemIndex` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des eine nicht existierende `DataListItem` Index. `-1`ist eine sichere Wahl, da die `DataListItem` ausgehend von Indizes `0`.
2. Binden Sie die Daten an die Datenliste. Seit Nein `DataListItem` `ItemIndex` vorangestellten entsprechen DataList s `EditItemIndex`, wird die gesamte DataList in einem nur-Lese Modus gerendert werden.

Diese Schritte können mit den folgenden Ereignishandlercode ausgeführt werden:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Durch diese hinzufügen klicken Sie auf "Abbrechen" Schaltfläche gibt DataList Datenbankzustands vorab bearbeiten.

Ist der letzte Ereignishandler zum abschließen müssen der `UpdateCommand` -Ereignishandler. Dieser Ereignishandler muss:

1. Programmgesteuerten Zugriff auf den Namen des Produkts Benutzer eingegeben und Preis als auch die bearbeitete Produkt s `ProductID`.
2. Veranlassen Sie die Aktualisierung durch Aufrufen der entsprechenden `UpdateProduct` überladen, die der `ProductsBLL` Klasse.
3. Legen Sie die DataList s [ `EditItemIndex` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) auf den Index des eine nicht existierende `DataListItem` Index. `-1`ist eine sichere Wahl, da die `DataListItem` ausgehend von Indizes `0`.
4. Binden Sie die Daten an die Datenliste. Seit Nein `DataListItem` `ItemIndex` vorangestellten entsprechen DataList s `EditItemIndex`, wird die gesamte DataList in einem nur-Lese Modus gerendert werden.

Schritt 1 und 2 sind verantwortlich für das Speichern des Benutzers s Änderungen; die Schritte 3 und 4 zurückgegeben DataList Datenbankzustands vorab bearbeiten, nachdem die Änderungen gespeichert wurden und sind identisch mit den Schritten, die ausgeführt werden, der `CancelCommand` -Ereignishandler.

Um die aktualisierten Produktnamen und bestimmter Preis zu erhalten, müssen wir verwenden die `FindControl` Methode, um programmgesteuert Verweis der TextBox-innerhalb Websteuerelemente der `EditItemTemplate`. Wir müssen auch die bearbeitete Produkt s abrufen `ProductID` Wert. Wenn wir zunächst die ObjectDataSource an DataList gebunden, zugewiesene Visual Studio DataList s `DataKeyField` -Eigenschaft auf den primären Schlüsselwert aus der Datenquelle (`ProductID`). Dieser Wert kann dann aus der DataList s abgerufen `DataKeys` Auflistung. Nehmen einen Moment Zeit, um sicherzustellen, dass die `DataKeyField` -Eigenschaftensatz ist tatsächlich auf `ProductID`.

Der folgende Code implementiert die vier Schritte:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Der Ereignishandler beginnt mit dem Lesen des bearbeiteten Produkts s `ProductID` aus der `DataKeys` Auflistung. Anschließend werden die beiden Textfelder ein, in der `EditItemTemplate` verwiesen wird und ihre `Text` Eigenschaften, die in lokalen Variablen gespeichert `productNameValue` und `unitPriceValue`. Verwenden wir die `Decimal.Parse()` -Methode zum Lesen des Werts aus der `UnitPrice` Textfeld so, dass bei der eingegebene Wert weist ein Währungssymbol, es kann immer noch ordnungsgemäß in konvertiert werden ein `Decimal` Wert.

> [!NOTE]
> Die Werte aus den `ProductName` und `UnitPrice` Textfelder ein werden nur auf die ProductNameValue und UnitPriceValue zugewiesen, wenn es sich bei der Textfelder Texteigenschaften einen angegebenen Wert haben. Andernfalls der Wert `Nothing` wird verwendet, für die Variablen, die die Auswirkung der Aktualisierung von Daten mit einer Datenbank hat `NULL` Wert. D. h. getesteten Codes behandelt konvertiert leere Zeichenfolgen in Datenbank `NULL` Werte, die das Standardverhalten der Bearbeitung in GridView, DetailsView und FormView-Schnittstelle ist.


Nach dem Lesen der Werte der `ProductsBLL` Klasse s `UpdateProduct` -Methode aufgerufen wird, übergeben den Namen des Produkts s, Preis, und `ProductID`. Der Ereignishandler abgeschlossen ist, wird durch Zurückgeben von DataList Datenbankzustands vorab Bearbeitung mit genau derselben Logik wie in der `CancelCommand` -Ereignishandler.

Mit der `EditCommand`, `CancelCommand`, und `UpdateCommand` Ereignishandler abgeschlossen ist, ein Besucher kann die Namen und den Preis eines Produkts bearbeiten. Abbildung 14 bis 16 Anzeigen dieser Bearbeitung Workflow in Aktion.


[![Werden beim ersten Besuch der Seite ", alle Produkte im schreibgeschützten Modus.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Abbildung 14**: bei der ersten Seite besuchen, werden alle Produkte im schreibgeschützten Modus ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Um ein Produkt s Namen oder den Preis zu aktualisieren, klicken Sie auf die Schaltfläche "Bearbeiten"](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Abbildung 15**: um ein Produkt s Name oder der Preis zu aktualisieren, klicken Sie auf die Schaltfläche "Bearbeiten" ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Nach dem Ändern des Werts, klicken Sie auf aktualisieren, um zurück zu den nur-Lese-Modus](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Abbildung 16**: nach dem Ändern des Werts, klicken Sie auf aktualisieren, um zurück in den schreibgeschützten Modus ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Schritt 7: Hinzufügen von Delete-Funktionen

Die Schritte zum Hinzufügen von Delete-Funktionen zu einem DataList ähneln denen für das Hinzufügen von Bearbeitungsfunktionen. Kurz gesagt, müssen wir eine Schaltfläche "löschen" zum Hinzufügen der `ItemTemplate` , die beim Klicken auf:

1. Liest das entsprechende Produkt s `ProductID` über die `DataKeys` Auflistung.
2. Führt Sie durch Aufrufen den Löschvorgang der `ProductsBLL` Klasse s `DeleteProduct` Methode.
3. Bindet die Daten an die Datenliste.

Können s starten, indem Sie eine Schaltfläche "löschen" zum Hinzufügen der `ItemTemplate`.

Beim Klicken auf eine Schaltfläche, deren `CommandName` ist bearbeiten, aktualisieren, oder "Abbrechen", löst das DataList s `ItemCommand` Ereignis zusammen mit der ein zusätzliches Ereignis (z. B. wenn der Bearbeitungsmodus mit der `EditCommand` Ereignis wird auch ausgelöst). Auf ähnliche Weise eine Schaltfläche, LinkButton oder ImageButton in DataList, deren `CommandName` Eigenschaftensatz Ursachen Löschen der `DeleteCommand` Ereignis ausgelöst (zusammen mit `ItemCommand`).

Hinzufügen einer Schaltfläche "löschen" neben der Schaltfläche "Bearbeiten", in der `ItemTemplate`wird durch das Festlegen der `CommandName` Eigenschaft zu löschen. Nach dem Hinzufügen dieses Schaltflächensteuerelement DataList s `ItemTemplate` deklarativer Syntax sollte wie folgt aussehen:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für das DataList s `DeleteCommand` Ereignis, mit dem folgenden Code:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Bewirkt, dass einen Postback und löst das DataList-s auf die Schaltfläche "löschen" `DeleteCommand` Ereignis. Im Ereignishandler, die geklickt wurde Produkt s `ProductID` Wert erfolgt über die `DataKeys` Auflistung. Als Nächstes wird das Produkt gelöscht, durch Aufrufen der `ProductsBLL` Klasse s `DeleteProduct` Methode.

Nach dem Löschen des Produkts es wichtig, dass wir die Daten der DataList binden (`DataList1.DataBind()`), andernfalls das DataList des Produkts zeigen weiterhin, die gerade gelöscht wurde.

## <a name="summary"></a>Zusammenfassung

Während DataList verfügt nicht über den Punkt, und klicken Sie auf Bearbeiten und Löschen von Unterstützung durch die GridView gefallen hat, kann mit einem kurzen Bit des Codes er erweitert werden zu diesen Funktionen gehören. In diesem Lernprogramm haben wir gesehen, zum Erstellen einer zweispaltige Liste von Produkten, die gelöscht werden konnte und, deren Namen und den Preis bearbeitet werden konnte. Hinzufügen, bearbeiten und Löschen von Unterstützung wird durch den entsprechenden Websteuerelementen in einschließlich der `ItemTemplate` und `EditItemTemplate`, die entsprechenden Ereignishandler erstellen, lesen Sie die Benutzer eingegeben und primäre Schlüsselwerte und eine Schnittstelle mit dem Business Logic Layer.

Während es grundlegende bearbeiten und Löschen von Funktionen, um die DataList hinzugefügt haben, fehlt erweiterte Funktionen. Ist beispielsweise keine Validierung Eingabefeld - es, wenn ein Benutzer mit einen Preis von Too eingibt viel Leistung beanspruchen, eine Ausnahme wird ausgelöst durch `Decimal.Parse` beim Versuch, die zu konvertierende teure in einem `Decimal`. Auf ähnliche Weise, wenn ein Problem vorliegt, aktualisieren Sie die Daten auf die Geschäftslogik oder Datenzugriffsebene erhält der Benutzer mit dem Bildschirm Standardfehler werden. Ohne dabei jegliche Art von Bestätigung auf die Schaltfläche "löschen" ist ein versehentliches Löschen von einem Produkt zu allen wahrscheinlich.

Kommen in Zukunft Lernprogramme, die wir zum Verbessern des Bearbeitung Benutzers sehen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Zack Jones Ken Pespisa und Randy Schmidt. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Nächste](performing-batch-updates-cs.md)
