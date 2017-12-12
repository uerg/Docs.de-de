---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Datengebundene Steuerelemente | Microsoft Docs
author: microsoft
description: "Die meisten ASP.NET-Anwendungen basieren auf ein gewisses Maß an Darstellung von Daten aus einer Back-End-Datenquelle. Datengebundene Steuerelemente wurden entscheidende Teil interagierenden w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 3ebb0f9a7a2f071b7bf7aa3855920f1a5784a61f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="data-bound-controls"></a>Datengebundene Steuerelemente
====================
durch [Microsoft](https://github.com/microsoft)

> Die meisten ASP.NET-Anwendungen basieren auf ein gewisses Maß an Darstellung von Daten aus einer Back-End-Datenquelle. Datengebundene Steuerelemente wurden entscheidende Teil der Interaktion mit Daten in dynamische Webanwendungen. ASP.NET 2.0 führt einige wesentlichen Verbesserungen für datengebundene Steuerelemente, einschließlich einer neuen BaseDataBoundControl-Klasse und deklarative Syntax.


Die meisten ASP.NET-Anwendungen basieren auf ein gewisses Maß an Darstellung von Daten aus einer Back-End-Datenquelle. Datengebundene Steuerelemente wurden entscheidende Teil der Interaktion mit Daten in dynamische Webanwendungen. ASP.NET 2.0 führt einige wesentlichen Verbesserungen für datengebundene Steuerelemente, einschließlich einer neuen BaseDataBoundControl-Klasse und deklarative Syntax.

Die BaseDataBoundControl fungiert als Basisklasse für die DataBoundControl-Klasse und die HierarchicalDataBoundControl-Klasse. In diesem Modul besprechen wir die folgenden Klassen, die von DataBoundControl abgeleitet werden:

- AdRotator
- List-Steuerelemente
- GridView
- FormView
- DetailsView

Die folgenden Klassen wird, die von HierarchicalDataBoundControl Klasse ableiten, auch erläutert:

- TreeView
- Menü
- SiteMapPath

## <a name="databoundcontrol-class"></a>DataBoundControl-Klasse

Die DataBoundControl-Klasse ist eine abstrakte Klasse (MustInherit in VB gekennzeichnet) verwendet, um die Interaktion mit tabellarischen oder Listendaten. Die folgenden Steuerelemente sind einige der Steuerelemente, die von DataBoundControl abgeleitet werden.

## <a name="adrotator"></a>AdRotator

AdRotator-Steuerelement können Sie eine Grafik Banner auf einer Webseite angezeigt wird, die mit einer bestimmten URL verknüpft ist. Die Grafik, die angezeigt wird, wird mithilfe von Eigenschaften für das Steuerelement gedreht. Die Häufigkeit von einer bestimmten Ad, die auf einer Seite anzeigen kann konfiguriert werden, mithilfe der **Eindrücke** Eigenschaft und anzeigen, können mithilfe von Filtern nach Schlüsselwörtern gefiltert werden.

AdRotator-Steuerelemente verwenden eine XML-Datei oder eine Tabelle in einer Datenbank für Daten. Die folgenden Attribute werden in XML-Dateien verwendet, so konfigurieren Sie das AdRotator-Steuerelement.

### <a name="imageurl"></a>ImageUrl
Die URL eines Bilds für die werbeeinblendung angezeigt werden soll.

### <a name="navigateurl"></a>NavigateUrl
Die URL, die der Benutzer beim Ad geklickt wird, ausgeführt werden soll. Dies sollte die URL-codiert sein.

### <a name="alternatetext"></a>AlternateText
Der alternative Text, der in einer QuickInfo angezeigt und von Bildschirmsprachausgaben gelesen wird. Zeigt auch, wenn das angegebene nach ImageUrl Bild nicht verfügbar ist.

### <a name="keyword"></a>Stichwort
Definiert ein Schlüsselwort, das bei Verwendung von Filtern verwendet werden kann. Wenn angegeben, werden nur diese werbeeinblendungen mit einem Schlüsselwort mit dem Stichwort Filter angezeigt werden.

### <a name="impressions"></a>Eindrücke
Eine Gewichtung-Zahl, die bestimmt, wie oft eine bestimmte Ad wahrscheinlich angezeigt wird. Es ist relativ zu den Eindruck von den anderen anzeigen in der gleichen Datei. Der Maximalwert der der kollektiven Eindrücke für alle werbeeinblendungen in eine XML-Datei ist 2,048,000,000 1.

### <a name="height"></a>Höhe
Die Höhe der Anzeige in Pixel.

### <a name="width"></a>Breite
Die Breite der Anzeige in Pixel.


> [!NOTE]
> Die Höhe und Breite Attribute überschreiben die Höhe und Breite für das AdRotator-Steuerelement selbst.


Eine typische XML-Datei könnte folgendermaßen aussehen:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Im obigen Beispiel ist die Anzeige für Contoso zweimal so häufig wie der Ad für die Website für ASP.NET aufgrund der Wert für das Attribut Eindrücke angezeigt wird.

Klicken Sie zum Anzeigen von Werbung aus der oben genannten XML-Datei ein AdRotator-Steuerelement zu einer Seite hinzufügen, und legen Sie die **AdvertisementFile** Eigenschaft so, dass der XML-Datei verweist, wie unten dargestellt:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Bei Auswahl eine Datenbanktabelle als Datenquelle für das Steuerelement AdRotator verwenden, müssen Sie zunächst Einrichten einer Datenbank unter Verwendung des folgenden Schemas:

| **Spaltenname** | **Datentyp** | **Beschreibung** |
| --- | --- | --- |
| ID | int | Primärschlüssel. Diese Spalte kann einen beliebigen Namen aufweisen. |
| ImageUrl | Nvarchar (*Länge*) | Der relative oder absolute URL des Bilds, das für die werbeeinblendung angezeigt wird. |
| NavigateUrl | Nvarchar (*Länge*) | Die Ziel-URL für die Anzeige. Wenn Sie keinen Wert angeben, ist die Anzeige keinen Link. |
| AlternateText | Nvarchar (*Länge*) | Der Text angezeigt, wenn das Bild nicht gefunden werden kann. In einigen Browsern wird der Text als QuickInfo angezeigt. Alternativer Text wird auch für den Zugriff verwendet, damit Benutzer, die in der Abbildung sehen können ihre Beschreibung lesen Sprachausgabe hören können. |
| Stichwort | Nvarchar (*Länge*) | Eine Kategorie für die Anzeige auf der die Seite gefiltert werden kann. |
| Eindrücke | int(4)-Datentyp | Eine Zahl, die die Wahrscheinlichkeit, wie oft die werbeeinblendung angezeigt werden. Je größer die Zahl, desto häufiger Ad wird angezeigt. Die Summe aller Eindrücke Werte in der XML-Datei kann 2.048.000.000-1 nicht überschreiten. |
| Breite | int(4)-Datentyp | Die Breite des Bilds in Pixel. |
| Höhe | int(4)-Datentyp | Die Höhe des Bilds in Pixel. |

In Fällen, in denen Sie bereits eine Datenbank mit einem anderen Schema verfügen, können Sie die **AlternateTextField**, **ImageUrlField**, und **NavigateUrlField** Eigenschaften zuordnen der AdRotator-Attribute, die vorhandene Datenbank. Zum Anzeigen der Daten aus der Datenbank im AdRotator-Steuerelement ein Datenquellen-Steuerelement auf der Seite hinzufügen, konfigurieren Sie die Verbindungszeichenfolge für das Datenquellensteuerelement, zeigen Sie auf die Datenbank und legen Sie das AdRotator Steuerelement **DataSourceID** Eigenschaft, die die ID des Datenquellen-Steuerelements. Verwenden Sie in Fällen, in dem Sie eine AdRotator Ads programmgesteuert konfigurieren müssen die AdCreated-Ereignis. Das Ereignis AdCreated akzeptiert zwei Parameter; ein Objekt und die anderen einer Instanz von AdCreatedEventArgs. Die AdCreatedEventArgs ist ein Verweis auf die Anzeige, die erstellt wird.

Der folgende Codeausschnitt legt die ImageUrl, NavigateUrl und AlternateText für eine Werbung programmgesteuert:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>List-Steuerelemente

List-Steuerelemente umfassen die ListBox, DropDownList CheckBoxList, RadioButtonList und BulletedList. Jedes dieser Steuerelemente kann Daten, die an eine Datenquelle gebunden werden. Sie verwenden ein Feld in der Datenquelle als Anzeigetext und können ein zweites Feld optional als der Wert eines Elements. Elemente können auch statisch zur Entwurfszeit hinzugefügt werden, und Sie können Mischen von statischen Elementen und dynamischen Elemente, die aus einer Datenquelle hinzugefügt.

Binden Sie an Daten ein Listenfeld-Steuerelement, fügen Sie ein Datenquellen-Steuerelement auf der Seite hinzu. Geben Sie einen SELECT-Befehl für das Datenquellensteuerelement, und legen Sie dann die DataSourceID-Eigenschaft des Steuerelements auf die ID des Datenquellen-Steuerelements. Verwenden der **DataTextField** und **DataValueField** Eigenschaften zum Definieren der Anzeigetext und den Wert für das Steuerelement. Darüber hinaus können Sie die **DataTextFormatString** Eigenschaft, um die Darstellung des Texts Anzeige wie folgt steuern:

| **Expression (Ausdruck)** | **Beschreibung** |
| --- | --- |
| Preis: {0:C} | Für numerische/Dezimaldaten. Zeigt das Literal "Preis:" gefolgt von Zahlen im Währungsformat. Das Währungsformat hängt von der kultureinstellung, die in das Culture-Attribut angegeben wird, auf die **Seite** Richtlinie oder in der Datei "Web.config". |
| {0:D4} | Für ganzzahlige Daten. Kann nicht bei Dezimalzahlen verwendet werden. Ganze Zahlen werden in einem Feld Nullen angezeigt, die vier Zeichen breit ist. |
| {0:N2}% | Für numerische Daten. Zeigt die Zahl mit 2 Dezimalstellen mit einfacher Genauigkeit, woraufhin das Literal "%". |
| {0:000.0} | Für numerische/Dezimaldaten. Zahlen werden auf eine Dezimalstelle gerundet. Zahlen mit weniger als drei Ziffern werden durch Nullen ergänzt. |
| {0:D} | Für Datum/Uhrzeit-Daten. Zeigt lange Datumsformat ("Donnerstag, 06. August 1996"). Das Datumsformat hängt von der Kultureinstellung der Seite oder der Datei Web.config ab. |
| {0:d} | Für Datum/Uhrzeit-Daten. Zeigt das kurze Datumsformat ("12/31/99"). |
| {0:JJ-MM-TT} | Für Datum/Uhrzeit-Daten. Zeigt Datum im numerischen Jahr-Monat-Tag-Format (96-08-06) |

## <a name="gridview"></a>GridView

GridView-Steuerelements für das tabellarische Daten anzeigen und bearbeiten die Verwendung eines deklarativen Ansatzes ermöglicht und ist der Nachfolger des DataGrid-Steuerelements. Die folgenden Funktionen stehen in des GridView-Steuerelements.

- Binden von Steuerelementen für Datenquellen, z. B. SqlDataSource an Daten.
- Integrierte Sortierfunktionen.
- Integrierte aktualisieren und Löschen von Funktionen.
- Integrierte Pagingfunktionen.
- Funktionen für das integrierte Zeile markieren.
- Programmgesteuerter Zugriff auf das Objektmodell GridView dynamisch festlegen von Eigenschaften, Ereignisse zu behandeln und so weiter.
- Mehrfachen Schlüsselfelder.
- Mehrere Datenfelder für den Hyperlinkspalten.
- Anpassbare Darstellung durch Designs und Stile.

**Spaltenfelder**

Jede Spalte des GridView-Steuerelements wird von einem DataControlField-Objekt dargestellt. Wird standardmäßig die AutoGenerateColumns-Eigenschaft auf festgelegt ist **"true"**, die ein AutoGeneratedField-Objekt für jedes Feld in der Datenquelle erstellt. Anschließend wird jedes Feld als Spalte in der GridView-Steuerelements in der Reihenfolge gerendert, der jedes Feld in der Datenquelle angezeigt wird. Sie können auch manuell steuern, welche Spalte im Felder angezeigt, die **GridView** Steuerelement durch Festlegen der **AutoGenerateColumns** Eigenschaft **"false"** an und definieren dann eine eigene Spalte feldauflistung. Andere Spaltenfeldtypen bestimmen das Verhalten der Spalten im Steuerelement.

Die folgende Tabelle enthält die unterschiedlichen Feld Spaltentypen, die verwendet werden können.

| **Spalte-Feldtyp** | **Beschreibung** |
| --- | --- |
| BoundField | Zeigt den Wert eines Felds in einer Datenquelle. Dies ist der Standardtyp für die Spalte des GridView-Steuerelements. |
| ButtonField | Zeigt eine Befehlsschaltfläche für jedes Element im des GridView-Steuerelements an. Dadurch können Sie eine Spalte mit benutzerdefinierten Schaltflächen-Steuerelemente, z. B. das Hinzufügen oder die Schaltfläche "entfernen" erstellen. |
| CheckBoxField | Zeigt ein Kontrollkästchen für jedes Element im des GridView-Steuerelements an. Dieses Feld Spaltentyp wird häufig verwendet, um Felder mit einem booleschen Wert anzuzeigen. |
| CommandField | Zeigt die vordefinierte Befehlsschaltflächen auswählen, bearbeiten oder Löschen von Vorgängen ausführen. |
| HyperLinkField | Zeigt den Wert eines Felds in einer Datenquelle als Hyperlink an. Diese Spaltenfeldtyp können Sie ein zweites Feld an die URL des Links zu binden. |
| ImageField | Zeigt ein Bild für jedes Element im des GridView-Steuerelements an. |
| TemplateField | Zeigt benutzerdefinierte Inhalte für jedes Element in einer angegebenen Vorlage entsprechend des GridView-Steuerelements. Diese Spaltenfeldtyp können Sie eine benutzerdefinierte Spalte ein Feld zu erstellen. |

Um eine Spalte feldauflistung deklarativ zu definieren, zunächst hinzufügen, öffnende und schließende  **&lt;Spalten&gt;**  Tags zwischen dem Start- und Endtags des GridView-Steuerelements. Als Nächstes Listen Sie die Spaltenfelder, die zwischen den öffnenden und schließenden enthalten sein sollen  **&lt;Spalten&gt;**  Tags. Die Auflistung der Spalten in der aufgeführten Reihenfolge werden die angegebenen Spalten hinzugefügt. Die **Spalten** Auflistung speichert "all" die Spalte im Steuerelement Felder und ermöglicht Ihnen die Spaltenfelder im GridView-Steuerelement programmgesteuert zu verwalten.

Explizit deklarierten Spaltenfelder können in Kombination mit automatisch generierten Spaltenfelder angezeigt werden. Wenn beide verwendet werden, werden zuerst explizit deklarierten Spaltenfelder gerendert gefolgt von den automatisch generierten Spaltenfelder.

## <a name="binding-to-data"></a>Binden an Daten

GridView-Steuerelements an ein Datenquellen-Steuerelement gebunden werden kann (z. B. **SqlDataSource**, **ObjectDataSource**usw.) sowie wie jede Datenquelle, die die "System.Collections.IEnumerable" implementiert Schnittstelle (z. B. System.Data.DataView, System.Collections.ArrayList oder System.Collections.Hashtable). Verwenden Sie eine der folgenden Methoden zum Binden des GridView-Steuerelements an den entsprechenden Datenquellentyp aus:

- Legen Sie zum Binden an ein Datenquellen-Steuerelement die DataSourceID-Eigenschaft des GridView-Steuerelements auf den ID-Wert des Datenquellen-Steuerelements ein. Des GridView-Steuerelements automatisch an angegebene Datenquellen-Steuerelements gebunden und Nutzen der Datenquelle des Steuerelements Funktionen zum Sortieren, aktualisieren, löschen und Pagingfunktionen ausführen. Dies ist die bevorzugte Methode zum Binden an Daten.
- Um mit einer Datenquelle zu binden, die die Schnittstelle "System.Collections.IEnumerable" implementiert, programmgesteuert die DataSource-Eigenschaft des GridView-Steuerelements mit der Datenquelle festgelegt, und rufen Sie die DataBind-Methode. Bei Verwendung dieser Methode stellt keine des GridView-Steuerelements bereit, integrierte sortieren, aktualisieren, löschen und paging Funktionalität. Sie müssen diese Funktion selbst bereitstellen.

## <a name="data-operations"></a>Datenvorgänge

GridView-Steuerelement stellt zahlreiche integrierte Funktionen, mit denen der Benutzer zum Sortieren, aktualisieren, löschen, auswählen und Paging von Elementen im Steuerelement. Wenn des GridView-Steuerelements an ein Datenquellen-Steuerelement gebunden ist, kann des GridView-Steuerelements von der Datenquelle des Steuerelements-Funktionen nutzen und automatische Sortierung, aktualisieren und Löschen von Funktionalität bereitzustellen.

> [!NOTE]
> Zum Sortieren, aktualisieren und Löschen bei anderen Arten von Datenquellen kann des GridView-Steuerelements unterstützen; Allerdings müssen Sie einen entsprechenden Ereignishandler mit der Implementierung für diese Vorgänge bereitstellen.


Sortieren kann der Benutzer zum Sortieren der Elemente im des GridView-Steuerelements in Bezug auf eine bestimmte Spalte, indem Sie auf den Spaltenüberschrift klicken. Legen Sie zum Aktivieren der Sortierung der AllowSorting-Eigenschaft auf **"true"**.

Die automatische aktualisieren, löschen und Auswahl der Funktionen sind aktiviert, wenn eine Schaltfläche in einem **ButtonField** oder **TemplateField** Feld für die Spalte mit dem "Bearbeiten", "Delete" und "Auswählen", Befehlsnamen bzw. geklickt wird. GridView-Steuerelement auch automatisch hinzugefügt. eine **CommandField** Spaltenfeld mit einer bearbeiten, löschen oder Schaltfläche auswählen, wenn die Eigenschaft AutoGenerateEditButton, AutoGenerateDeleteButton oder AutoGenerateSelectButton auf festgelegtist**"true"**bzw.

> [!NOTE]
> Einfügen von Datensätzen in der Datenquelle wird direkt vom GridView-Steuerelement nicht unterstützt. Allerdings ist es möglich, Einfügen von Datensätzen mithilfe des GridView-Steuerelements in Verbindung mit der DetailsView oder FormView-Steuerelement.


Des GridView-Steuerelements kann die Datensätze automatisch über mehrere Seiten aufteilen, anstatt alle Datensätze in der Datenquelle gleichzeitig anzuzeigen. Um das Paging aktivieren möchten, legen Sie die AllowPaging-Eigenschaft auf **"true"**.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des GridView-Steuerelements anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuerelements festlegen. Die folgende Tabelle enthält die verschiedenen Stileigenschaften.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| AlternatingRowStyle | Die Stileigenschaften für die abwechselnde Zeilen in der GridView-Steuerelements. Wenn diese Eigenschaft festgelegt ist, die Datenzeilen werden angezeigt abwechselnd die RowStyle-Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| EditRowStyle | Die Stileigenschaften für die Zeile im GridView-Steuerelement bearbeitet wird. |
| EmptyDataRowStyle | Die Stileigenschaften für die leere Datenzeile im GridView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die Stileigenschaften für die Fußzeile des GridView-Steuerelements. |
| HeaderStyle | Die Stileigenschaften für die Kopfzeile des GridView-Steuerelements. |
| PagerStyle | Die Stileigenschaften für die Pagerzeile des GridView-Steuerelements. |
| RowStyle | Die Stileigenschaften für die Zeilen in der GridView-Steuerelements. Wenn die **AlternatingRowStyle** -Eigenschaft ebenfalls festgelegt wird, werden die Datenzeilen abwechselnd angezeigt der **RowStyle** Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| SelectedRowStyle | Die Stileigenschaften für die ausgewählte Zeile im des GridView-Steuerelements. |

Sie können auch ein- oder Ausblenden von verschiedenen Teilen des Steuerelements. Die folgende Tabelle enthält die Eigenschaften, die steuern, welche Teile angezeigt oder ausgeblendet werden.

| **Property** | **Beschreibung** |
| --- | --- |
| ShowFooter | Anzeigen oder Ausblenden der Fußzeilenbereich des GridView-Steuerelements. |
| ShowHeader | Anzeigen oder Ausblenden der Headerabschnitt eines des GridView-Steuerelements. |

### <a name="events"></a>Ereignisse

GridView-Steuerelement bietet mehrere Ereignisse, denen Sie programmieren können. Dadurch können Sie eine benutzerdefinierte Routine ausgeführt wird, sobald ein Ereignis auftritt. In der folgenden Tabelle listet die Ereignisse, die vom GridView-Steuerelement unterstützt wird.

| **Event** | **Beschreibung** |
| --- | --- |
| PageIndexChanged | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber nachdem des GridView-Steuerelements den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, wenn eine Aufgabe auszuführen, nachdem der Benutzer zu einer anderen Seite im Steuerelement navigiert werden muss. |
| PageIndexChanging | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, jedoch bevor die GridView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |
| RowCancelingEdit | Tritt auf, wenn eine Zeile Schaltfläche "Abbrechen" geklickt wird, jedoch bevor des GridView-Steuerelements verlässt Bearbeitungsmodus. Dieses Ereignis wird häufig verwendet, beenden Sie den Abbruchvorgang. |
| RowCommand | Tritt auf, wenn im GridView-Steuerelement geklickt wird. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn das Steuerelement geklickt wird. |
| RowCreated | Tritt auf, wenn eine neue Zeile in der GridView-Steuerelement erstellt wird. Dieses Ereignis wird häufig verwendet, um den Inhalt einer Zeile zu ändern, wenn die Zeile erstellt wird. |
| RowDataBound | Tritt auf, wenn eine Datenzeile an Daten im GridView-Steuerelement gebunden ist. Dieses Ereignis wird häufig verwendet, um den Inhalt einer Zeile zu ändern, wenn die Zeile an Daten gebunden ist. |
| RowDeleted | Tritt auf, wenn eine Zeile Löschen geklickt wird, aber nachdem des GridView-Steuerelements den Datensatz aus der Datenquelle gelöscht. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| RowDeleting | Tritt auf, wenn eine Zeile der Schaltfläche "löschen" geklickt wird, aber bevor die GridView-Steuerelement, den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| RowEditing | Tritt auf, wenn eine Zeile der Schaltfläche "Bearbeiten" geklickt wird, aber bevor die GridView Steuerelement in den Bearbeitungsmodus wechselt. Dieses Ereignis wird häufig verwendet, um den Vorgang abzubrechen. |
| RowUpdated | Tritt auf, wenn eine Zeile der Schaltfläche "Aktualisieren" geklickt wird, aber nachdem des GridView-Steuerelements die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Updatevorgangs überprüfen. |
| RowUpdating | Tritt auf, wenn eine Zeile der Schaltfläche "Aktualisieren" geklickt wird, aber bevor die GridView-Steuerelement die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Aktualisierung abzubrechen. |
| SelectedIndexChanged | Tritt auf, wenn eine Zeile auswählen geklickt wird, aber nachdem des GridView-Steuerelements die select-Vorgang behandelt. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, nachdem eine Zeile im Steuerelement ausgewählt ist. |
| SelectedIndexChanging | Tritt auf, wenn eine Zeile auswählen geklickt wird, aber vor die GridView-Steuerelement den Auswahlvorgang behandelt. Dieses Ereignis wird häufig verwendet, um die Auswahl abzubrechen. |
| sortiert | Tritt auf, wenn auf der Link zum Sortieren einer Spalte geklickt wird, aber nachdem des GridView-Steuerelements Sortiervorgang behandelt. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, nachdem der Benutzer auf einen Link zum Sortieren einer Spalte geklickt hat. |
| Sortieren | Tritt auf, wenn der Link zum Sortieren einer Spalte geklickt wird, aber bevor die GridView-Steuerelement den Sortiervorgang behandelt. Dieses Ereignis wird häufig verwendet, den Sortiervorgang abbrechen oder eine benutzerdefinierte Sortierroutine auszuführen. |

## <a name="formview"></a>FormView

Die FormView-Steuerelement wird verwendet, um die Anzeige eines einzelnen Datensatzes aus einer Datenquelle. Sie ähnelt, DetailsView-Steuerelement, außer benutzerdefinierten Vorlagen anstelle von Zeilenfeldern angezeigt. Erstellen eigene Vorlagen, erhalten Sie mehr Flexibilität beim steuern, wie die Daten angezeigt werden. Die FormView-Steuerelement unterstützt die folgenden Funktionen:

- Binden von Steuerelementen für Datenquellen, z. B. SqlDataSource und ObjectDataSource an Daten.
- Integrierte Einfügefunktionen.
- Integrierte aktualisieren und Löschen von Funktionen.
- Integrierte Pagingfunktionen.
- Programmgesteuerter Zugriff auf das Objektmodell FormView dynamisch festlegen von Eigenschaften, Ereignisse zu behandeln und so weiter.
- Anpassbare Darstellung durch benutzerdefinierte Vorlagen, Designs und Stile.

## <a name="templates"></a>Vorlagen

Für das FormView-Steuerelement angezeigt werden sollen müssen Sie zum Erstellen von Vorlagen für die verschiedenen Teile des Steuerelements. Die meisten Vorlagen sind optional. Allerdings müssen Sie eine Vorlage für den Modus erstellen, in denen das Steuerelement so konfiguriert ist. Beispielsweise muss ein FormView-Steuerelement, das Einfügen von Datensätzen unterstützt eine Insert-Elementvorlage definiert haben. Die folgende Tabelle enthält die verschiedenen Vorlagen, die Sie erstellen können.

| **Typ "Template"** | **Beschreibung** |
| --- | --- |
| EditItemTemplate | Definiert den Inhalt für die Datenzeile, wenn FormView-Steuerelement im Bearbeitungsmodus befindet. Diese Vorlage enthält normalerweise Eingabesteuerelemente und Befehlsschaltflächen, mit denen der Benutzer einen vorhandenen Datensatz bearbeiten kann. |
| EmptyDataTemplate | Definiert den Inhalt für die leere Datenzeile angezeigt, wenn die FormView-Steuerelement an eine Datenquelle gebunden ist, die keine Datensätze enthält. Diese Vorlage enthält normalerweise Inhalte an die Benutzer darauf hinzuweisen, dass die Datenquelle keine Datensätze enthält. |
| FooterTemplate | Definiert den Inhalt für die Fußzeile. Diese Vorlage enthält in der Regel zusätzliche Inhalte, die Sie in der Fußzeile anzeigen möchten. Als Alternative können Sie einfach den Text, der in der Fußzeile angezeigt wird, durch Festlegen der Eigenschaft FooterText angeben. |
| HeaderTemplate | Definiert den Inhalt für die Kopfzeile. Diese Vorlage enthält in der Regel zusätzliche Inhalte, die in der Kopfzeile angezeigt werden sollen. Als Alternative können Sie einfach den Text, der in der Kopfzeile angezeigt, durch Festlegen der HeaderText-Eigenschaft angeben. |
| ItemTemplate | Definiert den Inhalt für die Datenzeile, wenn FormView-Steuerelement im Nur-Lese-Modus befindet. Diese Vorlage enthält normalerweise Inhalt, um die Werte eines vorhandenen Datensatzes anzuzeigen. |
| InsertItemTemplate | Definiert den Inhalt für die Datenzeile, wenn FormView-Steuerelement im Einfügemodus befindet. Diese Vorlage enthält normalerweise Eingabesteuerelemente und Befehlsschaltflächen, mit denen der Benutzer einen neuen Datensatz hinzufügen kann. |
| PagerTemplate | Definiert den Inhalt für die Pagerzeile angezeigt, wenn die Auslagerung-Funktion aktiviert ist (wenn die AllowPaging-Eigenschaft auf festgelegt ist **"true"**). Diese Vorlage enthält in der Regel Steuerelemente, mit denen der Benutzer zu einem anderen Datensatz navigieren kann. |

Eingabesteuerelemente in der Elementvorlage bearbeiten und Insert-Elementvorlage können mithilfe eines Ausdrucks für die bidirektionale Bindung an die Felder einer Datenquelle gebunden werden. Dadurch wird das Steuerelement FormView automatisch Extrahieren der Werte des Eingabesteuerelements für ein Update oder insert-Vorgang. Bidirektionale Bindungsausdrücke ermöglichen außerdem Eingabesteuerelemente in einer Elementvorlage Bearbeiten automatisch auf die ursprünglichen Werte des Felds angezeigt.

### <a name="binding-to-data"></a>Binden an Daten

Die FormView-Steuerelement an ein Datenquellen-Steuerelement gebunden werden kann (z. B. **SqlDataSource**, AccessDataSource, **ObjectDataSource** usw.), oder für jede Datenquelle, die implementiert die "System.Collections.IEnumerable"-Schnittstelle (z. B. System.Data.DataView System.Collections.ArrayList und System.Collections.Hashtable). Verwenden Sie eine der folgenden Methoden zum Binden der FormView-Steuerelements an den entsprechenden Datenquellentyp aus:

- Legen Sie zum Binden an ein Datenquellen-Steuerelement die DataSourceID-Eigenschaft des Steuerelements FormView auf den ID-Wert des Datenquellen-Steuerelements ein. FormView-Steuerelement automatisch an das angegebene Datenquellensteuerelement gebunden und Nutzen der Datenquelle Zugriffssteuerungsfunktionen einfügen, aktualisieren, löschen und Pagingfunktionen ausführen. Dies ist die bevorzugte Methode zum Binden an Daten.
- Mit einer Datenquelle zu binden, implementiert die **"System.Collections.IEnumerable"** Schnittstelle, legen Sie die DataSource-Eigenschaft des Steuerelements FormView programmgesteuert auf die Datenquelle aus, und rufen Sie die DataBind-Methode. Bei Verwendung dieser Methode stellt der FormView-Steuerelement keine integrierte einfügen, aktualisieren, löschen und Pagingfunktionen bereit. Sie müssen diese Funktionalität bereit, mit dem entsprechenden Ereignis.

## <a name="data-operations"></a>Datenvorgänge

Die FormView-Steuerelement stellt zahlreiche integrierte Funktionen, mit denen der Benutzer zu aktualisieren, löschen, einfügen und Durchblättern von Elementen im Steuerelement. Wenn FormView-Steuerelement an ein Datenquellen-Steuerelement gebunden ist, kann das FormView-Steuerelement der Datenquelle des Steuerelements-Funktionen nutzen und automatische aktualisieren, löschen, einfügen und auslagern Funktionalität bereitstellen. Die FormView-Steuerelement kann Update, Delete, Insert und Auslagerungsvorgänge mit anderen Typen von Datenquellen unterstützen; Sie müssen jedoch mit der Implementierung für diese Vorgänge ein geeigneten ereignishandlers bereitstellen.

Da das Steuerelement FormView Vorlagen verwendet wird, stellt er keine Möglichkeit zum automatischen Generieren von Befehlsschaltflächen zum Ausführen, aktualisieren, löschen oder Einfügen von Vorgängen bereit. Sie müssen diese Befehlsschaltflächen manuell in die entsprechende Vorlage einschließen. Die FormView-Steuerelement erkennt bestimmte Schaltflächen, sind ihre **CommandName** Eigenschaften, die auf bestimmte Werte festgelegt sind. Die folgende Tabelle enthält die Schaltflächen, die das Steuerelement FormView erkennt.

| **Button** (Schaltfläche) | **CommandName-Wert** | **Beschreibung** |
| --- | --- | --- |
| Abbrechen | "Cancel" | In aktualisieren oder Einfügen von Vorgängen verwendet, um den Vorgang abzubrechen und um die vom Benutzer eingegebenen Werte zu verwerfen. Die FormView-Steuerung wird dann wieder in den Modus, der von der DefaultMode-Eigenschaft angegeben. |
| Löschen | "Löschen" | Bei Löschen von Vorgängen verwendet, die um den angezeigten Datensatz aus der Datenquelle zu löschen. Löst das ItemDeleting und ItemDeleted-Ereignis aus. |
| Bearbeiten | "Bearbeiten" | Zum Aktualisieren der Vorgänge im FormView-Steuerelement in den Bearbeitungsmodus zu versetzen. Der Inhalt im angegebenen der **EditItemTemplate** Eigenschaft wird für die Datenzeile angezeigt. |
| Insert | "Einfügen" | Bei Einfügevorgängen verwendet, die versuchen, einen neuen Datensatz in der Datenquelle mit den Werten, die vom Benutzer bereitgestellte einzufügen. Löst das ItemInserting und ItemInserted-Ereignis aus. |
| Neu | "New" | Verwendet in Einfügevorgängen zum Platzieren des FormView-Steuerelements im Einfügemodus. Der Inhalt im angegebenen der **InsertItemTemplate** Eigenschaft wird für die Datenzeile angezeigt. |
| Seite | Seite """ | In Auslagerungsvorgänge verwendet, um eine Schaltfläche in der Pagerzeile darzustellen, die Paging ausführt. Den Pagingvorgang, legen die **CommandArgument** -Eigenschaft der Schaltfläche "Weiter", "Zurück", "First", "Letzte" oder der Index der Seite für die Navigation. Löst das PageIndexChanging und PageIndexChanged-Ereignis aus. |
| Aktualisieren | "Update" | Verwendet in Aktualisierungsvorgängen für den Versuch, den angezeigten Datensatz in der Datenquelle mit den vom Benutzer bereitgestellten Werten zu aktualisieren. Löst das ItemUpdating und ItemUpdated-Ereignis aus. |

Schaltfläche (die löscht den angezeigten Datensatzes sofort), wenn die Schaltfläche Bearbeiten oder neu geklickt wird, die FormView-Steuerelement in den Bearbeitungsmodus wechselt, oder Einfügemodus bzw., im Gegensatz zu löschen. In den Bearbeitungsmodus wechseln, der Inhalt enthalten, der **EditItemTemplate** Eigenschaft wird für das aktuelle Datenelement angezeigt. In der Regel ist die Bearbeitungselementvorlage definiert, dass die Schaltfläche "Bearbeiten", ein Update mit einer Schaltfläche "Abbrechen ersetzt wird". Eingabesteuerelemente, die für das Feld-Datentyp (z. B. ein Textfeld oder ein CheckBox-Steuerelement) geeignet sind, werden auch in der Regel mit der Wert eines Felds für den Benutzer zum Ändern angezeigt. Der Datensatz in der Datenquelle, auf die Schaltfläche "Aktualisieren" aktualisiert werden, während, Änderungen auf die Schaltfläche "Abbrechen abbricht".

Entsprechend den Inhalt in die **InsertItemTemplate** Eigenschaft ist für das Datenelement angezeigt, wenn das Steuerelement im Einfügemodus befindet. Die Insert-Elementvorlage wird in der Regel definiert, wird die Schaltfläche "Neu" durch ein INSERT- und eine Schaltfläche "Abbrechen" ersetzt, und die leere Eingabesteuerelemente für den Benutzer zur Eingabe der Werte für den neuen Eintrag angezeigt werden. Fügt ein Klicken auf die Schaltfläche zum Einfügen des Datensatzes in der Datenquelle während, Änderungen auf die Schaltfläche "Abbrechen abbricht".

Die FormView-Steuerelement enthält eine Auslagerung-Funktion, die der Benutzer navigieren zu anderen Datensätzen in der Datenquelle ermöglicht. Wenn aktiviert, wird eine Pagerzeile im FormView-Steuerelement angezeigt, die die Steuerelemente für die Seitennavigation enthält. Legen Sie zum Aktivieren von Paging der **AllowPaging** Eigenschaft **"true"**. Sie können die Pagerzeile anpassen, indem die Eigenschaften der enthaltenen Objekte in der PagerStyle und die PagerSettings-Eigenschaft festlegen. Anstatt die Pagerzeile integrierte Benutzeroberfläche verwenden, können Sie Ihre eigene Benutzeroberfläche erstellen, mit der **PagerTemplate** Eigenschaft.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des Steuerelements FormView anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuerelements festlegen. Die folgende Tabelle enthält die verschiedenen Stileigenschaften.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| EditRowStyle | Die Stileigenschaften für die Datenzeile wird das FormView-Steuerelement im Bearbeitungsmodus befindet. |
| EmptyDataRowStyle | Die Stileigenschaften für die leere Datenzeile in der FormView-Steuerelement angezeigt werden, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die Stileigenschaften für die Fußzeile des FormView-Steuerelements. |
| HeaderStyle | Die Stileigenschaften für die Kopfzeile der FormView-Steuerelement. |
| InsertRowStyle | Die Stileigenschaften für die Datenzeile wird das FormView-Steuerelement im Einfügemodus. |
| PagerStyle | Die Stileigenschaften für die Pagerzeile in der FormView-Steuerelement angezeigt wird, wenn die Auslagerung-Funktion aktiviert ist. |
| RowStyle | Die Stileigenschaften für die Datenzeile, wenn FormView-Steuerelement im Nur-Lese-Modus befindet. |

## <a name="events"></a>Ereignisse

Die FormView-Steuerelement bietet mehrere Ereignisse, denen Sie programmieren können. Dadurch können Sie eine benutzerdefinierte Routine ausgeführt wird, sobald ein Ereignis auftritt. In der folgenden Tabelle werden die Ereignisse, die vom Steuerelement FormView unterstützt aufgeführt.

| **Event** | **Beschreibung** |
| --- | --- |
| ItemCommand | Tritt auf, wenn auf eine Schaltfläche in einem FormView-Steuerelement geklickt wird. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn das Steuerelement geklickt wird. |
| ItemCreated | Tritt auf, nachdem alle FormViewRow Objekte in der FormView-Steuerelement erstellt werden. Dieses Ereignis wird häufig verwendet, um die Werte eines Datensatzes zu ändern, bevor er angezeigt wird. |
| ItemDeleted | Tritt auf, wenn eine Schaltfläche "löschen" (eine Schaltfläche mit seiner **CommandName** -Eigenschaft auf "Löschen" festgelegt) geklickt wird, aber erst, nachdem das Steuerelement FormView den Datensatz aus der Datenquelle gelöscht. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| ItemDeleting | Tritt auf, wenn eine Schaltfläche "löschen" geklickt wird, aber bevor die FormView-Steuerelement, den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| ItemInserted | Tritt auf, wenn eine Schaltfläche Einfügen (eine Schaltfläche mit seiner **CommandName** -Eigenschaft auf "Einfügen") geklickt wird, aber erst, nachdem das Steuerelement FormView den Datensatz eingefügt. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Einfügevorgangs zu überprüfen. |
| ItemInserting | Tritt auf, wenn eine Insert-Schaltfläche geklickt wird, aber vor der FormView-Steuerelement des Datensatzes einfügen. Dieses Ereignis wird häufig verwendet, um den Insert-Vorgang "Abbrechen". |
| ItemUpdated | Tritt auf, wenn eine Schaltfläche "Aktualisieren" (eine Schaltfläche mit seiner **CommandName** -Eigenschaft auf "Update" festgelegt) geklickt wird, aber erst, nachdem das Steuerelement FormView die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Updatevorgangs überprüfen. |
| ItemUpdating | Tritt auf, wenn eine Schaltfläche "Aktualisieren" geklickt wird, aber bevor die FormView-Steuerelement den Datensatz aktualisiert. Dieses Ereignis wird häufig verwendet, um den Updatevorgang "Abbrechen". |
| ModeChanged | Tritt auf, nachdem das Steuerelement FormView Modi ändert (zu bearbeiten, einfügen oder nur-Lese-Modus). Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn das Steuerelement FormView Modus ändert. |
| ModeChanging | Tritt ein, bevor das Steuerelement FormView Modi ändert (zu bearbeiten, einfügen oder nur-Lese-Modus). Dieses Ereignis wird häufig verwendet, um eine Änderung des Modus "Abbrechen". |
| PageIndexChanged | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber erst, nachdem die FormView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, wenn eine Aufgabe auszuführen, nachdem der Benutzer zu einem anderen Datensatz im Steuerelement navigiert werden muss. |
| PageIndexChanging | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber vor der FormView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |

## <a name="detailsview"></a>DetailsView

DetailsView-Steuerelement wird verwendet, um einen einzelnen Datensatz aus einer Datenquelle in einer Tabelle anzuzeigen, in dem jedes Feld des Datensatzes in einer Zeile der Tabelle angezeigt. Sie können in Kombination mit einem GridView-Steuerelement für Master / Detail-Szenarien verwendet werden. Das DetailsView-Steuerelement unterstützt die folgenden Funktionen:

- Binden von Steuerelementen für Datenquellen, z. B. SqlDataSource an Daten.
- Integrierte Einfügefunktionen.
- Integrierte aktualisieren und Löschen von Funktionen.
- Integrierte Pagingfunktionen.
- Programmgesteuerter Zugriff auf das Objektmodell DetailsView dynamisch festlegen von Eigenschaften, Ereignisse zu behandeln und so weiter.
- Anpassbare Darstellung durch Designs und Stile.

## <a name="row-fields"></a>Zeilenfeldern

Jede Datenzeile im DetailsView-Steuerelement wird durch das Feldsteuerelement erstellt. Andere Zeilenfeldtypen bestimmen das Verhalten der Zeilen im Steuerelement. Feldsteuerelemente abgeleitet DataControlField ab. Die folgende Tabelle enthält die andere Zeilenfeldtypen, die verwendet werden können.

| **Spalte-Feldtyp** | **Beschreibung** |
| --- | --- |
| BoundField | Zeigt den Wert eines Felds in einer Datenquelle als Text an. |
| ButtonField | Zeigt eine Befehlsschaltfläche im DetailsView-Steuerelement. Dadurch können Sie eine Zeile mit einem benutzerdefinierten Schaltflächen-Steuerelements, wie z. B. eine Schaltfläche hinzufügen oder Entfernen angezeigt. |
| CheckBoxField | Zeigt ein Kontrollkästchen im DetailsView-Steuerelement. Diese Zeilenfeldtyp wird häufig verwendet, um Felder mit einem booleschen Wert anzuzeigen. |
| CommandField | Integrierte Befehl zeigt Schaltflächen zum Ausführen von "Bearbeiten", insert oder delete-Vorgänge im DetailsView-Steuerelement. |
| HyperLinkField | Zeigt den Wert eines Felds in einer Datenquelle als Hyperlink an. Diese Zeilenfeldtyp können Sie ein zweites Feld an die URL des Links zu binden. |
| ImageField | Zeigt ein Bild im DetailsView-Steuerelement. |
| TemplateField | Zeigt benutzerdefinierte Inhalte für eine Zeile in einer angegebenen Vorlage entsprechend DetailsView-Steuerelement. Diese Zeilenfeldtyp können Sie ein benutzerdefiniertes Zeilenfeld zu erstellen. |

Standardmäßig ist die AutoGenerateRows-Eigenschaft auf festgelegt **"true"**, generiert die automatisch mit der Datenquelle gebundenen Feld Zeilenobjekts für jedes Feld gebunden. Gültige bindbare Typen sind Zeichenfolgen-, DateTime, Decimal, Guid und den Satz von primitiven Typen. Jedes Feld wird in einer Zeile als Text, in der Reihenfolge angezeigt in der jedes Feld in der Datenquelle angezeigt wird.

Generiert automatisch die Zeilen bietet eine schnelle und einfache Möglichkeit, um jedes Feld im Datensatz anzuzeigen. Zum Nutzen des DetailsView erweiterten des Steuerelements jedoch Funktionen, die Zeilenfelder einschließt, DetailsView-Steuerelement explizit deklariert werden müssen. Deklarieren der Zeilenfelder, legen Sie zuerst die **AutoGenerateRows** Eigenschaft **"false"**. Als Nächstes fügen Sie öffnende und schließende  **&lt;Felder&gt;**  Tags zwischen dem Start- und Endtag des Steuerelements DetailsView. Listen Sie schließlich die Zeilenfelder, die zwischen den öffnenden und schließenden enthalten sein sollen  **&lt;Felder&gt;**  Tags. Fields-Auflistung, in der aufgeführten Reihenfolge werden die angegebenen Zeilenfelder hinzugefügt. Die **Felder** Auflistung können Sie die Zeilenfeldern im DetailsView-Steuerelement programmgesteuert zu verwalten.

> [!NOTE]
> Automatisch generierte Felder der Fields-Auflistung nicht hinzugefügt werden.


## <a name="binding-to-data"></a>Binden an Daten

DetailsView-Steuerelement kann z. B. an ein Datenquellen-Steuerelement gebunden werden **SqlDataSource** oder AccessDataSource, oder für jede Datenquelle, die die Schnittstelle "System.Collections.IEnumerable" z. B. System.Data.DataView, implementiert System.Collections.ArrayList und System.Collections.Hashtable.

Verwenden Sie eine der folgenden Methoden, DetailsView-Steuerelement an den entsprechenden Datenquellentyp binden:

- Legen Sie die DataSourceID-Eigenschaft des Steuerelements DetailsView auf den ID-Wert des Datenquellen-Steuerelements, zum Binden an ein Datenquellen-Steuerelement. DetailsView-Steuerelement wird automatisch an das angegebene Datenquellen-Steuerelement gebunden. Dies ist die bevorzugte Methode zum Binden an Daten.
- Mit einer Datenquelle zu binden, implementiert die **"System.Collections.IEnumerable"** Schnittstelle, legen Sie die DataSource-Eigenschaft des Steuerelements DetailsView programmgesteuert auf die Datenquelle aus, und rufen Sie die DataBind-Methode.

## <a name="security"></a>Sicherheit

Dieses Steuerelement kann verwendet werden, Benutzereingaben angezeigt werden, die möglicherweise böswillige Clientskripts umfassen. Überprüfen Sie alle Informationen, die für ausführbare Skripts, SQL-Anweisungen oder anderen Code von einem Client gesendet wird, vor der Anzeige in der Anwendung. ASP.NET bietet eine Funktion für den Überprüfung eingabeanforderung zum Blockieren von Skript und HTML in Benutzereingaben.

## <a name="data-operations"></a>Datenvorgänge

DetailsView-Steuerelement enthält integrierte Funktionen, mit denen der Benutzer zu aktualisieren, löschen, einfügen und Durchblättern von Elementen im Steuerelement. Wenn DetailsView-Steuerelement an ein Datenquellen-Steuerelement gebunden ist, kann das DetailsView-Steuerelement der Datenquelle des Steuerelements-Funktionen nutzen und automatische aktualisieren, löschen, einfügen und auslagern Funktionalität bereitstellen.

Das Steuerelement DetailsView kann Update, Delete, Insert und Auslagerungsvorgänge mit anderen Typen von Datenquellen unterstützen; Sie müssen jedoch die Implementierung für diese Vorgänge in einem entsprechenden Ereignishandler bereitstellen.

DetailsView-Steuerelement auch automatisch hinzugefügt. eine **CommandField** Zeilenfeld mit einer Schaltfläche Bearbeiten, löschen oder neu durch Festlegen der Eigenschaften AutoGenerateEditButton, AutoGenerateDeleteButton oder AutoGenerateInsertButton auf **"true"**bzw. Schaltfläche (die löscht den ausgewählten Datensatz sofort), wenn die Schaltfläche Bearbeiten oder neu geklickt wird, DetailsView Steuerelement in den Bearbeitungsmodus wechselt, oder Einfügemodus, im Gegensatz zu löschen. In den Bearbeitungsmodus wechseln wird die Schaltfläche "Bearbeiten" durch ein aktualisieren und eine Schaltfläche "Abbrechen" ersetzt. Eingabesteuerelemente, die für das Feld-Datentyp (z. B. ein Textfeld oder ein CheckBox-Steuerelement) geeignet sind, werden mit der Wert eines Felds für den Benutzer ändern angezeigt. Der Datensatz in der Datenquelle, auf die Schaltfläche "Aktualisieren" aktualisiert werden, während, Änderungen auf die Schaltfläche "Abbrechen abbricht". Ebenso im Einfügemodus, die Schaltfläche "Neu" wird mit einer INSERT- und eine Schaltfläche "Abbrechen" ersetzt, und leere Eingabesteuerelemente für den Benutzer zur Eingabe der Werte für den neuen Eintrag angezeigt werden.

DetailsView-Steuerelement enthält eine Auslagerung-Funktion, die der Benutzer navigieren zu anderen Datensätzen in der Datenquelle ermöglicht. Wenn aktiviert, werden die Steuerelemente für die Seitennavigation in einer Pagerzeile angezeigt. Um das Paging aktivieren möchten, legen Sie die AllowPaging-Eigenschaft auf **"true"**. Die Pagerzeile kann mithilfe der PagerStyle und PagerSettings-Eigenschaften angepasst werden.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung von DetailsView-Steuerelement anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuerelements festlegen. Die folgende Tabelle enthält die verschiedenen Stileigenschaften.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| AlternatingRowStyle | Die Stileigenschaften für die abwechselnde Zeilen im DetailsView-Steuerelement. Wenn diese Eigenschaft festgelegt ist, die Datenzeilen werden angezeigt abwechselnd die RowStyle-Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| CommandRowStyle | Die Stileigenschaften für die Zeile, die die integrierte Befehlsschaltflächen im DetailsView-Steuerelement enthält. |
| EditRowStyle | Die Stileigenschaften für die Datenzeilen, DetailsView-Steuerelement wird im Bearbeitungsmodus befindet. |
| EmptyDataRowStyle | Die Stileigenschaften für die leere Datenzeile im DetailsView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die Stileigenschaften für die Fußzeile des DetailsView-Steuerelements. |
| HeaderStyle | Die Stileigenschaften für die Kopfzeile von DetailsView-Steuerelement. |
| InsertRowStyle | Die Stileigenschaften für die Datenzeilen, DetailsView-Steuerelement wird im Einfügemodus. |
| PagerStyle | Die Stileigenschaften für die Pagerzeile von DetailsView-Steuerelement. |
| RowStyle | Die Stileigenschaften für die Zeilen im DetailsView-Steuerelement. Wenn die **AlternatingRowStyle** -Eigenschaft ebenfalls festgelegt wird, werden die Datenzeilen abwechselnd angezeigt der **RowStyle** Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| FieldHeaderStyle | Die Stileigenschaften für die Headerspalte "von DetailsView-Steuerelement. |

## <a name="events"></a>Ereignisse

DetailsView-Steuerelement bietet mehrere Ereignisse, denen Sie programmieren können. Dadurch können Sie eine benutzerdefinierte Routine ausgeführt wird, sobald ein Ereignis auftritt. Die folgende Tabelle listet die Ereignisse von DetailsView-Steuerelement unterstützt. DetailsView-Steuerelement erbt außerdem diese Ereignisse von seinen Basisklassen: DataBinding, datengebundenen Disposed, Init, Load, PreRender und rendern.

| **Event** | **Beschreibung** |
| --- | --- |
| ItemCommand | Tritt auf, wenn im DetailsView-Steuerelement geklickt wird. |
| ItemCreated | Tritt auf, nachdem alle DetailsViewRow Objekte im DetailsView-Steuerelement erstellt werden. Dieses Ereignis wird häufig verwendet, um die Werte eines Datensatzes zu ändern, bevor er angezeigt wird. |
| ItemDeleted | Tritt auf, wenn auf eine Schaltfläche "löschen" geklickt wird, aber nachdem DetailsView-Steuerelement den Datensatz aus der Datenquelle gelöscht. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| ItemDeleting | Tritt auf, wenn eine Schaltfläche "löschen" geklickt wird, jedoch bevor DetailsView-Steuerelement, den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| ItemInserted | Tritt auf, wenn eine Insert-Schaltfläche geklickt wird, aber nachdem DetailsView-Steuerelement den Datensatz einfügt. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Einfügevorgangs zu überprüfen. |
| ItemInserting | Tritt auf, wenn eine Insert-Schaltfläche geklickt wird, aber vor dem DetailsView-Steuerelement des Datensatzes einfügen. Dieses Ereignis wird häufig verwendet, um den Insert-Vorgang "Abbrechen". |
| ItemUpdated | Tritt auf, wenn auf eine Schaltfläche "Aktualisieren" geklickt wird, aber erst, nachdem die Zeile aktualisiert, DetailsView-Steuerelement. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Updatevorgangs überprüfen. |
| ItemUpdating | Tritt auf, wenn eine Schaltfläche "Aktualisieren" geklickt wird, jedoch bevor DetailsView Steuerelement den Datensatz aktualisiert. Dieses Ereignis wird häufig verwendet, um den Updatevorgang "Abbrechen". |
| ModeChanged | Tritt auf, nachdem das Steuerelement DetailsView Modi (bearbeiten, einfügen oder nur-Lese-Modus) geändert. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn das Steuerelement DetailsView Modus ändert. |
| ModeChanging | Tritt auf, bevor das Steuerelement DetailsView Modi (bearbeiten, einfügen oder nur-Lese-Modus) geändert. Dieses Ereignis wird häufig verwendet, um eine Änderung des Modus "Abbrechen". |
| PageIndexChanged | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber nachdem DetailsView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, wenn eine Aufgabe auszuführen, nachdem der Benutzer zu einem anderen Datensatz im Steuerelement navigiert werden muss. |
| PageIndexChanging | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, jedoch bevor DetailsView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |

## <a name="the-menu-control"></a>Menu-Steuerelement

Das Menüsteuerelement in ASP.NET 2.0 dient eine voll ausgestattete Navigationssystem sein. Es kann problemlos mit hierarchischen Daten-Quellen wie etwa die SiteMapDataSource datengebundenen sein.

Eine Menüstruktur-Steuerelemente kann deklarativ oder dynamisch definiert werden und besteht aus einem einzelnen Stammknoten und eine beliebige Anzahl von untergeordneten Knoten. Der folgende Code definiert deklarativ ein Menü für das Menüsteuerelement.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Im obigen Beispiel ist der Knoten Home.aspx der Stammknoten. Alle anderen Knoten, die innerhalb des Stammknotens auf verschiedenen Ebenen geschachtelt sind.

Es gibt zwei Arten von Menüs, die das Steuerelement gerendert werden kann; statische Menüs und dynamische Menüs. Statischen Menüs bewirkt bestehen von Menüelementen, die immer sichtbar sind. Dynamische Menüs bestehen aus Menüelemente, die nur angezeigt werden, wenn der Benutzer mit der Maus darauf zeigt. Kunden können häufig verwechseln, statischen Menüs mit Menüs deklarativ definiert und dynamische Menüs mit Menüs, die datengebundene zur Laufzeit sind. In der Tat sind die dynamischen und statischen Menüs unabhängig von der Methode der Auffüllung. Die Begriffe *statische* und *dynamische* finden Sie unter nur fest, ob das Menü statisch angezeigten standardmäßig oder nur wird angezeigt, wenn der Benutzer eine Aktion ausführt.

Die **StaticDisplayLevels** Eigenschaft dient zum Konfigurieren, wie viele Ebenen des Menüs statisch und daher standardmäßig angezeigt werden. Im obigen Beispiel, das Festlegen der **StaticDisplayLevels** Eigenschaft auf einen Wert von 2 würde dazu führen, dass im Menü, um den Home-Knoten, die Musik Knoten- und den Knoten Filme statisch angezeigt. Alle anderen Knoten würde dynamisch angezeigt werden, wenn der Benutzer über den übergeordneten Knoten bewegt wird.

Die **MaximumDynamicDisplayLevels** Eigenschaft konfiguriert die maximale Anzahl der dynamischen Ebenen wird im Menü anzeigen kann. Dynamische Menüs auf einer höheren Ebene als der Wert von der **MaximumDynamicDisplayLevels** Eigenschaft werden verworfen.

> [!NOTE]
> Es ist fast sicher, dass die Situationen auftreten können, in denen Menüs angezeigt werden nicht aufgrund der MaximumDynamicDisplayLevels-Eigenschaft zu rendern. In diesen Fällen stellen Sie sicher, dass die Eigenschaft ausreichend festgelegt ist, um die Anzeige von Menüs, die Kunden zu ermöglichen.


## <a name="data-binding-the-menu-control"></a>Das Menüsteuerelement Binden von Daten

Das Menüsteuerelement kann einer beliebigen Datenquelle hierarchische Daten, z. B. die SiteMapDataSource oder die XMLDataSource gebunden werden. Die SiteMapDataSource ist die häufigste Methode für die Datenbindung, sodass ein Menüsteuerelement, da sie die Datei Web.sitemap eingespeisten und dessen Schema eine bekannte-API, um das Menüsteuerelement bietet. Die Liste unten zeigt eine einfache Web.sitemap-Datei.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Beachten Sie, dass nur ein SiteMapNode Stammelement, in diesem Fall der Startseite das Element vorhanden ist. Mehrere Attribute können für jede SiteMapNode konfiguriert werden. Die am häufigsten verwendeten Attribute sind:

- **URL** gibt die URL angezeigt, wenn ein Benutzer das Menüelement klickt. Wenn dieses Attribut nicht vorhanden ist, wird der Knoten einfach Posten wieder geklickt haben.
- **Titel** gibt den Text, der auf das Menüelement angezeigt wird.
- **Beschreibung** als Dokumentation für den Knoten verwendet. Auch als QuickInfo angezeigt, wenn der Mauszeiger über dem Knoten gezeigt wird.
- **SiteMapFile** für geschachtelte Funktionen "Sitemaps" ermöglicht. Dieses Attribut muss auf eine wohlgeformte ASP.NET Sitemap-Datei verweisen.
- **Rollen** ermöglicht die Darstellung eines Knotens durch ASP.NET aus Sicherheitsgründen gesteuert werden.

Beachten Sie, dass diese Attribute optional sind, das Verhalten des Menüs möglicherweise nicht wie erwartet wird, wenn sie nicht angegeben werden. Z. B. wenn die *Url* -Attribut angegeben ist, aber die *Beschreibung* -Attribut ist nicht, der Knoten nicht sichtbar sein und es werden keine Möglichkeit, mit der angegebenen URL navigieren.

## <a name="controlling-a-menus-operation"></a>Einen Vorgang Menüs steuern

Es gibt mehrere Eigenschaften, die sich auf eine ASP.NET Menüsteuerelement auswirken. die **Ausrichtung** -Eigenschaft, die **DisappearAfter** -Eigenschaft, die **StaticItemFormatString** -Eigenschaft, und die **StaticPopoutImageUrl**Eigenschaft sind nur einige dieser.

- Die **Ausrichtung** kann festgelegt werden, entweder *horizontale* oder *vertikale* und steuert, ob die statischen Menüelemente sind horizontal angeordnet in einer Zeile oder vertikal gestapelten Diagrammen und gestapelten nach miteinander verknüpfen. Diese Eigenschaft wirkt sich nicht auf dynamische Menüs aus.
- Die **DisappearAfter** Eigenschaft konfiguriert, wie lange ein dynamisches Menü sichtbar bleiben soll, nachdem die Maus wurde von es weg verschoben. Der Wert wird in Millisekunden und der Standardwert ist 500 angegeben. Festlegen dieser Eigenschaft auf einen Wert von-1 wird das Menü nie automatisch ausgeblendet wird. In diesem Fall wird das Menü nur ausgeblendet, klickt der Benutzer außerhalb der im Menü.
- Die **StaticItemFormatString** Eigenschaft erleichtert die konsistente Wortlaut in Ihrem Menüsystem zu verwalten. Beim Angeben dieser Eigenschaft *{0}* eingegeben werden anstelle der Beschreibung, die in der Datenquelle angezeigt wird. Beispielsweise würden Sie das Menüelement aus Übung 1 sagen, besuchen Sie unsere Produktseite usw. werden sollen, besuchen Sie unsere {0}-Seite für die StaticItemFormatString angeben. Zur Laufzeit wird ASP.NET jedem Vorkommen von {0} durch die richtige Beschreibung für das Menüelement ersetzt.
- Die **StaticPopoutImageUrl** Eigenschaft gibt an, das Bild, das verwendet wird, um anzugeben, dass ein bestimmtes Menü Knoten über untergeordnete Knoten verfügt, die mit dem Mauszeiger auf ihn zugegriffen werden kann. Dynamische Menüs weiterhin das Standardabbild verwenden.

## <a name="templated-menu-controls"></a>Auf Vorlagen basierende Menüsteuerelemente

Das Menüsteuerelement ist ein Steuerelement mit Vorlagen und zwei verschiedene ItemTemplates ermöglicht. die StaticItemTemplate und die DynamicItemTemplate. Mithilfe dieser Vorlagen können Sie problemlos Steuerelemente oder Benutzersteuerelemente Menüs hinzufügen.

Um die Vorlagen in Visual Studio .NET zu bearbeiten, klicken Sie auf die Smarttag-Taste auf das Menü, und wählen Vorlagen bearbeiten Sie. Sie können dann zwischen den StaticItemTemplate oder die DynamicItemTemplate bearbeiten auswählen.

Alle Steuerelemente hinzugefügt werden, um die StaticItemTemplate werden in einem statischen Menü angezeigt, beim Laden der Seite. Alle Steuerelemente hinzugefügt werden, um die DynamicItemTemplate werden auf alle Popupmenüs angezeigt.

## <a name="menu-events"></a>Menü-Ereignisse

Menu-Steuerelement verfügt über zwei Ereignisse, die eindeutig sind; die **MenuItemClicked** und **MenuItemDatabound** Ereignis.

Die MenuItemClicked-Ereignis wird ausgelöst, wenn auf ein Menüelement geklickt wird. Das MenuItemDatabound-Ereignis wird ausgelöst, wenn ein Menüelement an Daten gebunden ist. Die **MenuEventArgs** übergeben, die auf das Ereignis-Handler ermöglicht den Zugriff auf das Menüelement über die Eigenschaft "".

## <a name="controlling-a-menus-appearance"></a>Steuern einer Darstellung des Menüs

Sie können auch die Darstellung des ein Menüsteuerelement mit einem oder mehreren viele Formate zu Format Menüs des beeinflussen. Zu diesen zählen **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**, und **DynamicHoverStyle**. Diese Eigenschaften werden mithilfe von einer Zeichenfolge im standardmäßigen HTML-Format konfiguriert. Beispielsweise sind die folgenden den Stil für dynamische Menüs betroffen.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Wenn Sie keines der Hover-Stile verwenden, müssen Sie zum Hinzufügen einer &lt;Head&gt; Element auf der Seite mit den *Runat* -Elementgruppe *Server*.


Menüsteuerelemente unterstützen auch die Verwendung von ASP.NET 2.0-Designs.

## <a name="the-treeview-control"></a>TreeView-Steuerelement

TreeView-Steuerelement zeigt Daten in einer Baumstruktur an. Wie bei den Menu-Steuerelement kann es problemlos an eine beliebige hierarchische-Datenquelle z. B. die SiteMapDataSource datengebunden sein.

Die erste Frage, die Kunden wahrscheinlich zu Fragen über das TreeView-Steuerelement in ASP.NET 2.0 ist, und zwar unabhängig davon, ob es auf das TreeView-IE-Websteuerelement beziehen, die für ASP.NET verfügbar war 1.x. Es ist nicht. Die ASP.NET 2.0 TreeView-Steuerelement wurde von Grund auf und bietet deutliche Verbesserung über die IE TreeView WebControl, die zuvor verfügbar waren.

Ich mich wird nicht im Detail auf wie eine TreeView-Steuerelement an einer Siteübersicht gebunden, während er, auf genau die gleiche Weise wie das Menüsteuerelement ausgeführt wird. TreeView-Steuerelement hat jedoch einige deutliche Unterschiede in der Weise, die er verwendet wird.

Standardmäßig wird eine TreeView-Steuerelement vollständig erweitert angezeigt. Um die Ebene der Erweiterung nach dem anfänglichen Laden zu ändern, Ändern der **ExpandDepth** Eigenschaft des Steuerelements. Dies ist besonders wichtig, in den Fällen, in der Baumstruktur datengebundenen nach bestimmten Knoten zu erweitern.

## <a name="databinding-the-treeview-control"></a>DataBinding das TreeView-Steuerelement

Im Gegensatz zu den Menu-Steuerelement liegen die Stärken der Baumstruktur Verarbeitung großer Datenmengen. Neben dem Datenbindung zu einem SiteMapDataSource oder XMLDataSource ist der Baumstruktur daher häufig Daten, die an ein DataSet oder anderen relationalen Daten gebunden. In Fällen, in denen das TreeView-Steuerelement an die große Mengen an Daten gebunden ist, ist es am besten, nur an Daten binden, die tatsächlich im Steuerelement angezeigt wird. Anschließend können Sie Daten auf zusätzliche Daten binden, wie TreeView-Knoten erweitert werden.

In diesen Fällen die **PopulateOnDemand** Eigenschaft der TreeView sollte festgelegt werden, um *"true"*. Sie müssen eine Implementierung zum Bereitstellen der **TreeNodePopulate** Methode.

## <a name="data-binding-without-postback"></a>Die Datenbindung ohne PostBack

Beachten Sie, dass wenn Sie zum ersten Mal einen Knoten im vorherigen Beispiel erweitern, wird die Seite zurückübertragen und aktualisiert. Thats nicht auf ein Problem in diesem Beispiel, aber Sie können sich, dass es in einer produktiven Umgebung mit einer großen Menge von Daten möglicherweise vorstellen. Eine bessere Szenario wäre in der TreeView würde weiterhin dynamisch seiner Knoten aufgefüllt, aber ohne einen Beitrag zum Server zurückzusenden.

Durch Festlegen der **PopulateNodesFromClient** und **PopulateOnDemand** Eigenschaften auf "true", das ASP.NET TreeView-Steuerelement werden die Knoten ohne Postback dynamisch aufgefüllt. Wenn der übergeordnete Knoten erweitert wird, eine Anforderung XMLHttp wird vom Client vorgenommen, und die OnTreeNodePopulate-Ereignis ausgelöst wird. Der Server antwortet mit einer XML-Dateninsel, die anschließend mit Daten verwendet wird, binden die untergeordneten Knoten.

ASP.NET erstellt dynamisch den Clientcode, der diese Funktion implementiert. Die &lt;Skript&gt; Tags, die das Skript verweist auf eine Datei AXD generiert werden. Die Liste unten zeigt z. B. die Skript-Links, um den Skriptcode, der die Anforderung XMLHttp generiert.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Wenn Sie die Datei AXD oben in Ihrem Browser durchsuchen, und öffnen, sehen Sie den Code, der die Anforderung XMLHttp implementiert. Diese Methode wird verhindert, dass Kunden die Skriptdatei ändern.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Steuern des Vorgangs für das TreeView-Steuerelement

TreeView-Steuerelement verfügt über mehrere Eigenschaften, die die Funktion des Steuerelements beeinflussen. Die offensichtlichsten Eigenschaften sind die **ShowCheckBoxes**, **ShowExpandCollapse**, und **ShowLines**.

Die **ShowCheckBoxes** Eigenschaft wirkt sich auf, und zwar unabhängig davon, ob Knoten ein Kontrollkästchen beim Rendern angezeigt. Die gültigen Werte für diese Eigenschaft sind **keine**, **Root**, **übergeordneten**, **Endknoten**, und **alle**. Diese betreffen das TreeView-Steuerelement wie folgt:

| **Eigenschaftswert** | **Auswirkung** |
| --- | --- |
| Keine | Kontrollkästchen werden nicht auf den Knoten angezeigt. Dies ist die Standardeinstellung. |
| Stamm | Ein Kontrollkästchen wird nur für den Stammknoten angezeigt. |
| Übergeordnetes Element | Ein Kontrollkästchen wird nur auf diesen Knoten angezeigt, die über untergeordnete Knoten verfügen. Diese untergeordneten Knoten möglich übergeordneten Knoten oder Blattknoten. |
| Blattebene | Ein Kontrollkästchen wird nur auf diesen Knoten angezeigt, die keine untergeordneten Knoten besitzen. |
| Alle | Ein Kontrollkästchen wird auf allen Knoten angezeigt. |

Wenn das Kontrollkästchen verwendet werden, die **CheckedNodes** Eigenschaft gibt eine Auflistung von TreeView-Knoten, die beim Postback Einchecken zurück.

Die **ShowExpandCollapse** Eigenschaft steuert die Darstellung des Bilds erweitern/reduzieren neben Stamm- und den übergeordneten Knoten. Wenn diese Eigenschaft, um festgelegt wird **"false"**, TreeView-Knoten werden als Hyperlinks gerendert und sind erweitert/reduziert, indem Sie auf den Link klicken.

Die **ShowLines** Eigenschaft steuert, ob Zeilen angezeigt werden übergeordnete Knoten mit untergeordneten Knoten verbinden. Wenn **"false"** (Standard), werden keine Linien angezeigt. Wenn **"true"**, das TreeView-Steuerelement verwendet Bilder Zeilen vom angegebenen Ordner die **LineImagesFolder** Eigenschaft.

Zum Anpassen der Darstellung von TreeView Zeilen enthält Visual Studio .NET 2005 eine Line-Designer-Tool. Sie können dieses Tool, mit der Smart Tag-Schaltfläche für das TreeView-Steuerelement als unten zugreifen.


![](data-bound-controls/_static/image1.jpg)

**Abbildung 1**


Beim Auswählen der **Liniengrafiken anpassen** Menüoption, Line-Designers können Sie so konfigurieren Sie die Darstellung der TreeView Zeilen gestartet werden.

## <a name="treeview-events"></a>TreeView-Ereignisse

TreeView-Steuerelement verfügt über die folgenden eindeutigen Ereignisse:

- SelectedNodeChanged-tritt auf, wenn ein Knoten ausgewählt ist, basierend auf den **SelectAction** Eigenschaft.
- TreeNodeCheckChanged tritt auf, wenn ein Knoten Checkboxs Status geändert wird.
- TreeNodeExpanded-Ereignis tritt auf, wenn ein Knoten erweitert wird auf Grundlage der **SelectAction** Eigenschaft.
- Das TreeNodeCollapsed tritt auf, wenn ein Knoten reduziert wird.
- TreeNodeDataBound tritt auf, wenn ein Knoten Daten gebunden werden.
- TreeNodePopulate tritt auf, wenn ein Knoten aufgefüllt wird.

Die **SelectAction** Eigenschaft können Sie konfigurieren, welche Ereignis ausgelöst wird, wenn ein Knoten ausgewählt ist. Die Eigenschaft SelectAction bietet die folgenden Aktionen:

- TreeNodeSelectAction.Expand löst TreeNodeExpanded-Ereignis, wenn der Knoten ausgewählt wurde.
- TreeNodeSelectAction.None löst kein Ereignis aus, wenn der Knoten ausgewählt wurde.
- TreeNodeSelectAction.Select löst das SelectedNodeChanged-Ereignis, wenn der Knoten ausgewählt ist.
- TreeNodeSelectAction.SelectExpand löst das SelectedNodeChanged-Ereignis und das Ereignis TreeNodeExpanded-Ereignis, wenn der Knoten ausgewählt ist.

## <a name="controlling-appearance-with-styles"></a>Steuern der Darstellung mit Formatvorlagen

TreeView-Steuerelement bietet zahlreiche Eigenschaften zur Steuerung der Darstellung des Steuerelements mit Formatvorlagen. Die folgenden Eigenschaften sind verfügbar.

| **Eigenschaftenname** | **Steuerelemente** |
| --- | --- |
| HoverNodeStyle | Steuert den Stil der Knoten an, wenn der Mauszeiger über ihnen gezeigt wird. |
| LeafNodeStyle | Steuert die Art des Blattknoten. |
| NodeStyle | Steuert den Stil für alle Knoten. Bestimmte Knotenstile (z. B. LeafNodeStyle) Überschreiben dieses Format. |
| ParentNodeStyle | Steuert den Stil für alle übergeordneten Knoten. |
| RootNodeStyle | Steuert den Stil für den Stammknoten. |
| SelectedNodeStyle | Steuert den Stil für den ausgewählten Knoten. |

Jede dieser Eigenschaften ist schreibgeschützt. Sie werden jedoch jede Return eine **TreeNodeStyle** Objekt und die Eigenschaften dieses Objekts können modifiziert mithilfe der *Eigenschaft Untereigenschaften* Format. Z. B. zum Festlegen der **ForeColor** Eigenschaft von der **SelectedNodeStyle**, würden Sie die folgende Syntax verwenden:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Beachten Sie, dass die oben genannten Tag nicht geschlossen ist. Ist, dass wenn Sie die hier gezeigte deklarative Syntax zu verwenden, würden Sie die TreeViews Knoten in der HTML-Code enthalten.

Die Stileigenschaften können auch angegeben werden, im Code mithilfe der *property.subproperty* Format. Z. B. zum Festlegen der **ForeColor** Eigenschaft von der **RootNodeStyle** im Code verwenden Sie die folgende Syntax:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Eine umfassende Liste der verschiedenen Stileigenschaften finden Sie unter der MSDN-Dokumentation für das TreeNodeStyle-Objekt.


## <a name="the-sitemappath-control"></a>Das SiteMapPath-Steuerelement

Das Steuerelement SiteMapPath stellt ein Navigationssteuerelement für Brot (Breadcrumb) für ASP.NET-Entwickler, bereit. Wie die anderen Steuerelemente zur Seitennavigation kann es leicht, dass Daten um hierarchische Daten Quellen wie z. B. die SiteMapDataSource oder XmlDataSource gebunden sein.

Ein Steuerelement SiteMapPath SiteMapNodeItem Objekten besteht. Es gibt drei Arten von Knoten; der Stammknoten, übergeordneten Knoten und dem aktuellen Knoten. Der Stammknoten ist der Knoten am oberen Rand der hierarchischen Struktur. Der aktuelle Knoten stellt die aktuelle Seite. Alle anderen Knoten werden die übergeordneten Knoten.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Steuerung der Ausführung des SiteMapPath-Steuerelements

Die Eigenschaften, die den Betrieb des Steuerelements SiteMapPath steuern sind wie folgt:

| **Property** | **Beschreibung der Eigenschaft** |
| --- | --- |
| ParentLevelsDisplayed | Steuert, wie viele übergeordneten Knoten angezeigt werden. Der Standardwert ist-1 keine Einschränkung für die Anzahl der übergeordneten Knoten angezeigt erzwingt. |
| PathDirection | Steuert die Richtung des SiteMapPath an. Gültige Werte sind RootToCurrent (Standard) und CurrentToRoot. |
| PathSeparator | Eine Zeichenfolge, die die Zeichen gesteuert, das Knoten in einem Steuerelement SiteMapPath trennt. Der Standardwert ist:. |
| RenderCurrentNodeAsLink | Ein boolescher Wert, der steuert, ob der aktuelle Knoten als Link gerendert wird. Der Standardwert ist "false". |
| SkipLinkText | Dies hilft dabei mit Barrierefreiheit, wenn die Seite von Bildschirmsprachausgaben angezeigt wird. Diese Eigenschaft ermöglicht Bildschirmsprachausgaben SiteMapPath-Steuerelement zu überspringen. Um dieses Feature deaktivieren, legen Sie die Eigenschaft auf "String.Empty" ein. |

## <a name="templated-sitemappath-controls"></a>Auf Vorlagen basierende SiteMapPath-Steuerelemente

Die SiteMapControl ist ein Steuerelement mit Vorlagen, und daher können Sie unterschiedliche Vorlagen für die Verwendung in Anzeigen des Steuerelements definieren. Klicken Sie zum Bearbeiten von Vorlagen in einem Steuerelement SiteMapPath klicken Sie auf die Smarttag-Taste auf das Steuerelement, und wählen Sie im Menü Vorlagen bearbeiten. Dadurch werden die SiteMapTasks im Menü angezeigt, wie unten in dem Sie zwischen den verschiedenen verfügbaren Vorlagen auswählen können.


![](data-bound-controls/_static/image2.jpg)

**Abbildung 2**


Die **NodeTemplate** Vorlage bezieht sich auf einem beliebigen Knoten im SiteMapPath. Wenn der Knoten einen Stammknoten oder der aktuelle Knoten ist und eine **RootNodeTemplate** oder **CurrentNodeTemplate** ist so konfiguriert ist, wird die NodeTemplate überschrieben wird.

## <a name="sitemappath-events"></a>SiteMapPath-Ereignisse

Das SiteMapPath-Steuerelement verfügt über zwei Ereignisse, die nicht von der Control-Klasse abgeleitet werden; die **ItemCreated** Ereignis und die **ItemDataBound** Ereignis. Die ItemCreated-Ereignis wird ausgelöst, wenn ein SiteMapPath-Element erstellt wird. ItemDataBound wird ausgelöst, wenn während der Datenbindung eines Knotens SiteMapPath die DataBind-Methode aufgerufen wird. Ein **SiteMapNodeItemEventArgs** -Objekt bietet Zugriff auf den bestimmten SiteMapNodeItem über die Item-Eigenschaft.

## <a name="controlling-appearance-with-styles"></a>Steuern der Darstellung mit Formatvorlagen

Die folgenden Formate sind für das Formatieren eines Steuerelements SiteMapPath verfügbar.

| **Eigenschaftenname** | **Steuerelemente** |
| --- | --- |
| CurrentNodeStyle | Steuert die Formatierung des Texts für den aktuellen Knoten. |
| RootNodeStyle | Steuert die Formatierung des Texts für den Stammknoten. |
| NodeStyle | Steuert die Formatierung des Texts für alle Knoten, vorausgesetzt, dass ein CurrentNodeStyle oder RootNodeStyle nicht zutrifft. |

Die NodeStyle-Eigenschaft wird von der CurrentNodeStyle oder die RootNodeStyle überschrieben. Jede dieser Eigenschaften ist schreibgeschützt und gibt eine **Stil** Objekt. Um die Darstellung eines Knotens mit einer dieser Eigenschaften beeinflussen möchten, müssen Sie zum Festlegen der Eigenschaften des Style-Objekts, das zurückgegeben wird. Beispielsweise ändert der folgende Code die Forecolor-Eigenschaft des aktuellen Knotens.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Die Eigenschaft kann auch programmgesteuert wie folgt angewendet werden:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Wenn eine Vorlage angewendet wird, wird das Format nicht angewendet werden.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Übung 1: Konfigurieren einer ASP.NET Menu-Steuerelement

1. Erstellen einer neuen Website.
2. Hinzufügen einer Siteübersicht-Datei von Datei "," Neu "," Datei auswählen und Siteübersicht aus der Liste der Vorlagen für die Datei an.
3. Öffnen Sie die Siteübersicht (Web.sitemap standardmäßig), und ändern Sie, damit es die unten aufgeführte aussieht. Die Seiten, die Sie in der Zuordnungsdatei Standort verknüpfen, nicht wirklich vorhanden sind, aber, die ein Problem für diese Übung nicht werden.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Öffnen Sie das Standardformular für die Web, in der Entwurfsansicht.
5. Fügen Sie im Abschnitt "Navigation" der Toolbox auf die Seite neue Menu-Steuerelement hinzu.
6. Fügen Sie eine neue SiteMapDataSource im Abschnitt "Daten" der Toolbox hinzu. SiteMapDataSource wird automatisch die Web.sitemap-Datei auf Ihrer Website verwenden. (Die Datei Web.sitemap *müssen* werden im Stammordner der Website.)
7. Klicken Sie auf das Steuerelement, und klicken Sie dann auf das Smarttag-Schaltfläche, um das Dialogfeld "Menüaufgaben" anzuzeigen.
8. Wählen Sie in der Dropdownliste wählen Sie die Datenquelle SiteMapDataSource1.
9. Klicken Sie auf den Link AutoFormat, und wählen Sie ein Format für das Menü.
10. Legen Sie im Bereich Eigenschaften den **StaticDisplayLevels** -Eigenschaft auf 2. Das Menüsteuerelement sollte jetzt den Knoten Home, Produkte und Dienste im Designer angezeigt.
11. Durchsuchen Sie die Seite in Ihrem Browser auf das Menü verwenden. (Da die Seiten der Siteübersicht Sie Ve hinzugefügt nicht tatsächlich vorhanden sind, einen Fehler sehen Sie, wenn Sie versuchen, und navigieren Sie zu ihnen.)

Experimentieren Sie mit der StaticDisplayLevels und die MaximumDynamicDisplayLevels Eigenschaften ändern, und prüfen Sie, deren Einfluss auf das Rendern im Menüs aus.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Übung 2: Dynamisches Binden von TreeView-Steuerelement

In dieser Übung wird davon ausgegangen, dass Sie SQL Server lokal ausgeführt wird und dass die Northwind-Datenbank auf SQL Server-Instanz vorhanden ist. Wenn diese Bedingungen nicht erfüllt sind, ändern Sie die Verbindungszeichenfolge in der Stichprobe. Beachten Sie, dass Sie möglicherweise auch SQL Server-Authentifizierung anstelle einer vertrauten Verbindung angeben müssen.

1. Erstellen einer neuen Website.
2. Wechseln Sie zur Codeansicht für "default.aspx", und Ersetzen Sie der gesamte Code durch den unten aufgeführten Code. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Speichern Sie die Seite als treeview.aspx.
4. Durchsuchen Sie die Seite.
5. Wenn die Seite erstmals angezeigt wird, zeigen Sie die Quelle der Seite in Ihrem Browser. Beachten Sie, dass nur die sichtbaren Knoten an den Client gesendet wurden.
6. Klicken Sie auf das Pluszeichen neben jedem Knoten.
7. Anzeigen der Quelle auf der Seite erneut. Beachten Sie, dass nun die neu angezeigten Knoten vorhanden sind.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Übung 3: Details anzeigen und Bearbeiten von Daten mithilfe eines GridView und DetailsView

1. Erstellen einer neuen Website.
2. Fügen Sie eine neue Datei "Web.config" der Website hinzu.
3. Fügen Sie eine Verbindungszeichenfolge in die Datei "Web.config" wie unten dargestellt: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Möglicherweise müssen Sie die Verbindungszeichenfolge, die basierend auf Ihrer Umgebung zu ändern.
4. Speichern und schließen Sie die Datei "web.config".
5. Öffnen Sie Default.aspx, und fügen Sie ein neues SqlDataSource-Steuerelement hinzu.
6. Ändern Sie die ID des Steuerelements SqlDataSource **Produkte**.
7. In der **SqlDataSource-Aufgaben** Menü klicken Sie auf **Datenquelle konfigurieren**.
8. Wählen Sie **Northwind** in der Dropdownliste für die Verbindung, und klicken Sie auf Weiter.
9. Wählen Sie **Produkte** aus der **Namen** Dropdownliste, und überprüfen Sie die **"ProductID"**, **ProductName**, **UnitPrice**, und **"UnitsInStock"** Kontrollkästchen wie unten dargestellt. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Klicken Sie auf **Weiter**.
11. Klicken Sie auf **Fertig stellen**.
12. Wechseln Sie zur Datenquellensicht an, und überprüfen Sie den Code, der generiert wurde. Beachten Sie, dass die **SelectCommand**, **DeleteCommand**, **InsertCommand**, und **UpdateCommand** , wurden hinzugefügt, um die ': SqlDataSource -Steuerelement. Beachten Sie außerdem die Parameter, die hinzugefügt wurden.
13. Wechseln Sie zur Entwurfsansicht, und fügen Sie ein neues GridView-Steuerelement auf der Seite hinzu.
14. Wählen Sie **Produkte** aus der **Datenquelle auswählen** Dropdownliste.
15. Überprüfen Sie **Paging aktivieren** und **Auswahl aktivieren** wie unten dargestellt. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Klicken Sie auf die **Spalten bearbeiten** verknüpfen, und stellen Sie sicher, dass **Felder automatisch generieren** aktiviert ist.
17. Klicken Sie auf **OK**.
18. Mit dem GridView-Steuerelement ausgewählt haben, klicken Sie auf die Schaltfläche neben dem **DataKeyNames** Eigenschaft im Bereich "Eigenschaften".
19. Wählen Sie **"ProductID"** aus der **verfügbaren Felder** aus, und klicken Sie auf die  **&gt;**  Schaltfläche, um sie hinzuzufügen.
20. Klicken Sie auf OK.
21. Fügen Sie ein neues SqlDataSource-Steuerelement auf der Seite hinzu.
22. Ändern Sie die ID des Steuerelements SqlDataSource **Details**.
23. Wählen Sie im Menü ': SqlDataSource-Aufgaben **Datenquelle konfigurieren**.
24. Wählen Sie **Northwind** aus der Dropdownliste und auf **Weiter**.
25. Wählen Sie **Produkte** aus der **Namen** Dropdownliste, und überprüfen Sie die  **\***  Kontrollkästchen in der **Spalten** Listbox.
26. Klicken Sie auf die **, in denen** Schaltfläche.
27. Wählen Sie **"ProductID"** aus der **Spalte** Dropdownliste.
28. Wählen Sie  **=**  in der Dropdownliste Operator.
29. Wählen Sie **Steuerelement** aus der **Quelle** Dropdownliste.
30. Wählen Sie **GridView1** aus der **Kontroll-ID** Dropdownliste.
31. Klicken Sie auf die **hinzufügen** Schaltfläche, um der WHERE-Klausel hinzuzufügen.
32. Klicken Sie auf **OK**.
33. Klicken Sie auf die **erweitert** Schaltfläche aus, und überprüfen Sie die **generieren INSERT, UPDATE und DELETE-Anweisungen** Kontrollkästchen.
34. Klicken Sie auf **OK**.
35. Klicken Sie auf **Weiter** , und klicken Sie auf **Fertig stellen**.
36. Fügen Sie ein DetailsView-Steuerelement auf der Seite "ein.
37. In der **Datenquelle auswählen** Dropdownliste, wählen Sie **Details**.
38. Überprüfen Sie die **Bearbeiten aktivieren** Kontrollkästchen wie unten dargestellt. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Speichern Sie die Seite, und Durchsuchen von "default.aspx".
40. Klicken Sie auf die **wählen** neben unterschiedliche Datensätze, DetailsView Update automatisch angezeigt.
41. Klicken Sie auf die **bearbeiten** Link im DetailsView-Steuerelement.
42. Nehmen Sie eine Änderung auf den Eintrag, und klicken Sie auf **Update**.
