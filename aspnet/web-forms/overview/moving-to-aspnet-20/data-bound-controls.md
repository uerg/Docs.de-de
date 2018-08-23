---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Datengebundene Steuerelemente | Microsoft-Dokumentation
author: microsoft
description: Die meisten ASP.NET-Anwendungen basieren auf einem gewissen Grad der Darstellung von Daten aus einer Back-End-Datenquelle. Datengebundene Steuerelemente wurden eine entscheidende Teil der Interaktion mit...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: b115109c7307d05dc9e620378a51a71407204740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836860"
---
<a name="data-bound-controls"></a>Datengebundene Steuerelemente
====================
durch [Microsoft](https://github.com/microsoft)

> Die meisten ASP.NET-Anwendungen basieren auf einem gewissen Grad der Darstellung von Daten aus einer Back-End-Datenquelle. Datengebundene Steuerelemente wurden pivotal Teil der Interaktion mit Daten in dynamischer Webanwendungen. ASP.NET 2.0 bietet einige erheblichen Verbesserungen von datengebundenen Steuerelementen, einschließlich einer neuen BaseDataBoundControl-Klasse und die deklarative Syntax.


Die meisten ASP.NET-Anwendungen basieren auf einem gewissen Grad der Darstellung von Daten aus einer Back-End-Datenquelle. Datengebundene Steuerelemente wurden pivotal Teil der Interaktion mit Daten in dynamischer Webanwendungen. ASP.NET 2.0 bietet einige erheblichen Verbesserungen von datengebundenen Steuerelementen, einschließlich einer neuen BaseDataBoundControl-Klasse und die deklarative Syntax.

Die BaseDataBoundControl fungiert als Basisklasse für die DataBoundControl-Klasse und die HierarchicalDataBoundControl-Klasse. In diesem Modul besprechen wir die folgenden Klassen, die von DataBoundControl abgeleitet:

- AdRotator
- Listensteuerelemente
- GridView
- FormView
- DetailsView

Darüber werden hinaus die folgenden Klassen erläutert werden, die von HierarchicalDataBoundControl-Klasse abgeleitet werden:

- TreeView
- Menü
- SiteMapPath

## <a name="databoundcontrol-class"></a>DataBoundControl-Klasse

Die DataBoundControl-Klasse ist eine abstrakte Klasse (markiert als MustInherit in VB) verwendet, um die Interaktion mit tabellarischen oder Listendaten. Die folgenden Steuerelemente sind einige der Steuerelemente, die von DataBoundControl abgeleitet werden.

## <a name="adrotator"></a>AdRotator

Das Steuerelement AdRotator können Sie ein grafisches Banner auf einer Webseite angezeigt, die an eine bestimmte URL verknüpft ist. Die Grafik, die angezeigt wird, wird mithilfe von Eigenschaften für das Steuerelement gedreht. Die Häufigkeit von einer bestimmten Ad, die auf einer Seite anzeigen kann konfiguriert werden, mithilfe der **Eindrücke** -Eigenschaft und anzeigen, können mithilfe von Filtern nach Schlüsselwörtern gefiltert werden.

AdRotator Steuerelemente werden entweder eine XML-Datei oder eine Tabelle in einer Datenbank für Daten verwenden. Die folgenden Attribute werden in XML-Dateien verwendet, um das Steuerelement AdRotator konfigurieren.

### <a name="imageurl"></a>ImageUrl
Die URL eines Bilds für die werbeeinblendung angezeigt werden soll.

### <a name="navigateurl"></a>NavigateUrl
Die URL, die der Benutzer beim Ad geklickt wird, ausgeführt werden soll. Dies sollte die URL-codiert sein.

### <a name="alternatetext"></a>AlternateText
Der alternative Text, der in einer QuickInfo angezeigt und von Bildschirmsprachausgaben gelesen wird. Wird auch angezeigt, wenn das angegebene ImageUrl Bild nicht verfügbar ist.

### <a name="keyword"></a>Stichwort
Definiert ein Schlüsselwort, das beim Verwenden von Schlüsselwortfiltern verwendet werden kann. Wenn angegeben, werden nur diese werbeeinblendungen mit einem Schlüsselwort, das dem Schlüsselwortfilter entsprechenden angezeigt werden.

### <a name="impressions"></a>Seitenaufrufe
Eine Gewichtung-Zahl, die bestimmt, wie oft eine bestimmte Ad wahrscheinlich angezeigt wird. Es ist relativ zu den Eindruck von den anderen anzeigen in der gleichen Datei. Der maximale Wert des der kollektiven Eindrücke für alle Werbung in einer XML-Datei ist 2,048,000,000 1.

### <a name="height"></a>Höhe
Die Höhe der Anzeige in Pixel.

### <a name="width"></a>Breite
Die Breite der Anzeige in Pixel.


> [!NOTE]
> Die Höhe und Breite Attribute überschreiben Sie die Höhe und Breite für das AdRotator-Steuerelement selbst.


Eine typische XML-Datei könnte folgendermaßen aussehen:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Im obigen Beispiel ist die Anzeige für Contoso doppelt so oft wie der Ad für die Website mit ASP.NET aufgrund des Werts für das Attribut Eindrücke angezeigt werden.

Klicken Sie zum Anzeigen von Werbung von der obigen XML-Datei, ein AdRotator-Steuerelement zu einer Seite hinzufügen, und legen Sie die **AdvertisementFile** Eigenschaft, um auf die XML-Datei zu verweisen, wie unten dargestellt:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Wenn Sie eine Datenbanktabelle als Datenquelle für das Steuerelement AdRotator verwenden möchten, müssen Sie zunächst eine Datenbank mithilfe des folgenden Schemas:

| **Name der Spalte** | **Datentyp** | **Beschreibung** |
| --- | --- | --- |
| Id | int | Primärschlüssel. Diese Spalte kann einen beliebigen Namen besitzen. |
| ImageUrl | Nvarchar (*Länge*) | Der relative oder absolute URL des Bilds, das für die werbeeinblendung angezeigt wird. |
| NavigateUrl | Nvarchar (*Länge*) | Die Ziel-URL für die Anzeige. Wenn Sie keinen Wert angeben, ist die Anzeige keinen Link. |
| AlternateText | Nvarchar (*Länge*) | Der Text angezeigt, wenn das Bild nicht gefunden werden kann. In einigen Browsern wird der Text als QuickInfo angezeigt. Alternativtext dient auch für den Zugriff auf, damit Benutzer, die nicht in der Grafik angezeigt seine Beschreibung, die laut lesen zu hören. |
| Stichwort | Nvarchar (*Länge*) | Eine Kategorie für die Anzeige auf der Seite filtern kann. |
| Seitenaufrufe | int(4)-Datentyp | Eine Zahl, die die Wahrscheinlichkeit wie oft die Anzeige angezeigt wird. Je größer die Zahl, desto häufiger der Anzeige angezeigt wird. Die Summe aller Eindrücke-Werte in der XML-Datei darf 2.048.000.000-1 nicht überschreiten. |
| Breite | int(4)-Datentyp | Die Breite des Bilds in Pixel. |
| Höhe | int(4)-Datentyp | Die Höhe des Bilds in Pixel. |

In Fällen, in denen Sie bereits eine Datenbank mit einem anderen Schema verfügen, können Sie die **AlternateTextField**, **ImageUrlField**, und **NavigateUrlField** Eigenschaften zum Zuordnen der AdRotator-Attribute, die der vorhandenen Datenbank. Zum Anzeigen der Daten aus der Datenbank im AdRotator-Steuerelement ein Datenquellen-Steuerelement auf der Seite hinzufügen, konfigurieren Sie die Verbindungszeichenfolge für das Datenquellen-Steuerelement, zeigen Sie mit der Datenbank und legen Sie die AdRotator **DataSourceID** Eigenschaft, die die ID des Datenquellen-Steuerelement. Verwenden Sie in Fällen, in dem Sie eine AdRotator werbeeinblendungen programmgesteuert konfiguriert werden muss die AdCreated-Ereignis. Das Ereignis AdCreated akzeptiert zwei Parameter an; eine ein-Objekt, und das andere eine Instanz des AdCreatedEventArgs. Die AdCreatedEventArgs ist ein Verweis auf die Anzeige, die erstellt wird.

Der folgende Codeausschnitt legt die ImageUrl, NavigateUrl und AlternateText für eine Ad programmgesteuert:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Listensteuerelemente

Listensteuerelemente enthalten die ListBox, DropDownList, "CheckBoxList", RadioButtonList und BulletedList. Jedes dieser Steuerelemente kann Daten, die an eine Datenquelle gebunden werden. Sie verwenden ein Feld in der Datenquelle als Anzeigetext und können ein zweites Feld optional als der Wert eines Elements. Elemente können auch statisch zur Entwurfszeit hinzugefügt werden, und Sie statische Elemente und dynamischen Elemente, die aus einer Datenquelle hinzugefügt mischen können.

Klicken Sie auf Daten binden ein Listensteuerelement, ein Datenquellen-Steuerelement auf der Seite hinzufügen. Geben Sie einen SELECT-Befehl für das Datenquellen-Steuerelement, und legen Sie dann auf die ID des Datenquellen-Steuerelement die Eigenschaft "DataSourceID" des Steuerelements. Verwenden der **DataTextField** und **DataValueField** Eigenschaften, die den anzuzeigenden Text und der Wert für das Steuerelement zu definieren. Darüber hinaus können Sie mithilfe der **DataTextFormatString** Eigenschaft, um die Darstellung des Anzeigetexts wie folgt steuern:

| **Expression (Ausdruck)** | **Beschreibung** |
| --- | --- |
| Preis: {0:C} | Für Daten, die numerische/Dezimalzahl. Zeigt das Literal "Preis:" gefolgt von Zahlen im Währungsformat an. Das Währungsformat hängt von der kultureinstellung, die in das Culture-Attribut angegeben wird, auf die **Seite** -Direktive oder in der Datei "Web.config". |
| {0:D4} | Für ganzzahlige Daten. Kann nicht bei Dezimalzahlen verwendet werden. Ganze Zahlen werden in einem Feld Nullen angezeigt, die vier Zeichen breit ist. |
| {0:N2}% | Für numerische Daten. Zeigt die Zahl mit 2 Dezimalstellen mit einfacher Genauigkeit gefolgt von dem Literal "%". |
| {0:000.0} | Für Daten, die numerische/Dezimalzahl. Zahlen werden auf eine Dezimalstelle gerundet. Zahlen mit weniger als drei Ziffern werden durch Nullen ergänzt. |
| {0:D} | Für Datum/Uhrzeit-Daten. Zeigt lange Datumsformat ("Donnerstag, 06. August 1996"). Das Datumsformat hängt von der Kultureinstellung der Seite oder der Datei Web.config ab. |
| {0:d} | Für Datum/Uhrzeit-Daten. Zeigt das kurze Datumsformat ("12/31/99"). |
| {0:JJ-MM-TT} | Für Datum/Uhrzeit-Daten. Zeigt Datum im numerischen Jahr-Monat-Tag-Format (96-08-06) |

## <a name="gridview"></a>GridView

Das GridView-Steuerelement ermöglicht eine tabellarische Daten anzeigen und bearbeiten die Verwendung eines deklarativen Ansatzes und ist der Nachfolger von DataGrid-Steuerelement. Die folgenden Funktionen stehen im GridView-Steuerelement.

- Die Bindung an Datenquellen-Steuerelemente, z. B. SqlDataSource-Steuerelement.
- Integrierte Sortierfunktionen.
- Integrierte aktualisieren und Löschen von Funktionen.
- Integrierte Pagingfunktionen.
- Funktionen für das integrierte Zeile markieren.
- Den programmgesteuerten Zugriff auf das GridView-Objektmodell zum dynamischen Festlegen von Eigenschaften, Ereignisse verarbeiten und so weiter.
- Mehrfachen Schlüsselfelder.
- Mehrere Datenfelder für die Linkspalten.
- Anpassbare Darstellung durch Designs und Stile.

**Spaltenfelder**

Jede Spalte im GridView-Steuerelement wird von einem DataControlField-Objekt dargestellt. In der Standardeinstellung die AutoGenerateColumns-Eigenschaftensatz auf **"true"**, wodurch ein AutoGeneratedField-Objekt für jedes Feld in der Datenquelle erstellt. Jedes Feld wird dann als eine Spalte im GridView-Steuerelement in der Reihenfolge gerendert, der jedes Feld in der Datenquelle angezeigt wird. Sie können auch manuell steuern, welche Felder, in angezeigt Spalte die **GridView** Steuerelement durch Festlegen der **AutoGenerateColumns** Eigenschaft **"false"** und durch anschließendes definieren Ihre eigenen Spalte feldauflistung. Feldtypen für die andere Spalte bestimmen das Verhalten der Spalten im Steuerelement.

Die folgende Tabelle enthält die andere Spaltenfeldtypen, die verwendet werden können.

| **Feldtyp für Spalte** | **Beschreibung** |
| --- | --- |
| BoundField | Zeigt den Wert eines Felds in einer Datenquelle. Dies ist der Standardtyp für die Spalte des GridView-Steuerelements. |
| ButtonField | Zeigt eine Befehlsschaltfläche für jedes Element im GridView-Steuerelement. Dadurch können Sie zum Erstellen einer Spalte des benutzerdefinierten Schaltflächen-Steuerelemente, z. B. das Hinzufügen oder die Schaltfläche "entfernen". |
| CheckBoxField | Zeigt ein Kontrollkästchen für jedes Element im GridView-Steuerelement. Diese Spaltenfeldtyp wird häufig verwendet, um Felder mit einem booleschen Wert anzuzeigen. |
| CommandField | Zeigt die vordefinierte Befehlsschaltflächen zum Ausführen von auswählen, bearbeiten oder Löschen von Vorgängen. |
| HyperLinkField | Zeigt den Wert eines Felds in einer Datenquelle, als Link. Diese Spaltenfeldtyp können Sie ein zweites Feld an die URL des Links zu binden. |
| ImageField | Zeigt ein Bild für jedes Element im GridView-Steuerelement. |
| TemplateField | Zeigt den benutzerdefinierten Inhalt für jedes Element in einer angegebenen Vorlage entsprechend GridView-Steuerelement. Diese Spaltenfeldtyp können Sie zum Erstellen eines benutzerdefinierten Felds. |

Um eine Spalte feldauflistung deklarativ zu definieren, zunächst hinzufügen, öffnende und schließende **&lt;Spalten&gt;** Tags zwischen den öffnenden und schließenden Tags des GridView-Steuerelements. Als Nächstes Listen Sie die Spaltenfelder, die zwischen den öffnenden und schließenden enthalten sein sollen **&lt;Spalten&gt;** Tags. Die Auflistung der Spalten in der aufgeführten Reihenfolge werden die angegebenen Spalten hinzugefügt. Die **Spalten** Auflistung speichert alle die Spalte im Steuerelement Felder und ermöglicht es Ihnen, die die Spaltenfelder im GridView-Steuerelement programmgesteuert zu verwalten.

Explizit deklarierten Spaltenfelder können in Kombination mit automatisch generierten Spaltenfelder angezeigt werden. Wenn beide verwendet werden, werden zuerst explizit deklarierten Spaltenfelder gerendert gefolgt von die Felder automatisch generierte Spalte.

## <a name="binding-to-data"></a>Binden an Daten

Das GridView-Steuerelement an ein Datenquellensteuerelement gebunden werden kann (z. B. **SqlDataSource**, **"ObjectDataSource"** usw.) sowie wie jede Datenquelle, die die System.Collections.IEnumerable implementiert Schnittstelle (z. B. System.Data.DataView System.Collections.ArrayList oder System.Collections.Hashtable). Verwenden Sie eine der folgenden Methoden zum Binden des GridView-Steuerelements auf den entsprechenden Datenquellentyp:

- Um an ein Datenquellen-Steuerelement zu binden, wird die Eigenschaft "DataSourceID" des GridView-Steuerelements auf den ID-Wert, der das Datenquellen-Steuerelement fest. Das GridView-Steuerelement wird automatisch bindet an das angegebene Datenquellen-Steuerelement, und profitieren von der Datenquelle des Steuerelements-Funktionen zum Sortieren, aktualisieren, löschen und Pagingfunktionen ausführen. Dies ist die bevorzugte Methode zum Binden an Daten.
- Um mit einer Datenquelle zu binden, der ' System.Collections.IEnumerable '-Schnittstelle implementiert, programmgesteuert die DataSource-Eigenschaft des GridView-Steuerelements mit der Datenquelle festgelegt, und rufen Sie dann die DataBind-Methode. Wenn Sie diese Methode verwenden zu können, bietet GridView-Steuerelement keine integrierte sortieren, aktualisieren, löschen und Funktionalität der Auslagerungsdatei. Sie müssen diese Funktion selbst bereitstellen.

## <a name="data-operations"></a>Datenvorgänge

Das GridView-Steuerelement bietet zahlreiche integrierte Funktionen, mit denen den Benutzer zum Sortieren, zu aktualisieren, löschen, wählen und Paging von Elementen im Steuerelement. Wenn das GridView-Steuerelement an ein Datenquellensteuerelement gebunden ist, kann GridView-Steuerelement profitieren Sie von der Datenquelle Funktionen des Steuerelements und bieten automatische Sortieren, aktualisieren und Löschen von Funktionen.

> [!NOTE]
> Das GridView-Steuerelement kann zum Sortieren, aktualisieren und Löschen bei anderen Arten von Datenquellen unterstützen; Allerdings müssen Sie für die Implementierung für diese Vorgänge einen geeigneten Ereignishandler bereit.


Sortieren kann der Benutzer auf Elemente im GridView-Steuerelement in Bezug auf eine bestimmte Spalte sortieren, indem Sie auf die Spaltenüberschrift klicken. Legen Sie zum Aktivieren der Sortierung die AllowSorting-Eigenschaft auf **"true"**.

Die automatische aktualisieren, löschen und Auswählen der Funktionen werden aktiviert, wenn eine Schaltfläche in einem **ButtonField** oder **TemplateField** Spaltenfeld, mit dem Befehlsnamen "Bearbeiten", "Delete" und "Select" geklickt wird. Das GridView-Steuerelement auch automatisch hinzugefügt. eine **CommandField** Spaltenfeld mit einer bearbeiten, löschen oder Schaltfläche auswählen, wenn die Eigenschaft AutoGenerateEditButton AutoGenerateDeleteButton oder AutoGenerateSelectButton auf festgelegtist **"true"** bzw.

> [!NOTE]
> Einfügen von Datensätzen in der Datenquelle wird durch das GridView-Steuerelement nicht direkt unterstützt. Allerdings ist es möglich, zum Einfügen von Datensätzen mithilfe der GridView-Steuerelement in Verbindung mit der DetailsView oder FormView-Steuerelement.


Das GridView-Steuerelement kann die Datensätze automatisch über mehrere Seiten aufteilen, anstatt alle Datensätze in der Datenquelle zur gleichen Zeit. Legen Sie zum Aktivieren von Paging die AllowPaging-Eigenschaft auf **"true"**.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des GridView-Steuerelements anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuerelements festlegen. Die folgende Tabelle enthält die verschiedenen Stileigenschaften.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| AlternatingRowStyle | Die stileinstellungen für die abwechselnde Zeilen im GridView-Steuerelement. Wenn diese Eigenschaft festgelegt ist, die Datenzeilen werden angezeigt Wechsel zwischen den RowStyle-Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| EditRowStyle | Die stileinstellungen für die Zeile im GridView-Steuerelement bearbeitet wird. |
| EmptyDataRowStyle | Die stileinstellungen für die leere Datenzeile im GridView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die stileinstellungen für die Footerzeile des GridView-Steuerelements. |
| HeaderStyle | Die stileinstellungen für die Kopfzeile der GridView-Steuerelement. |
| PagerStyle | Die stileinstellungen für die Pagerzeile des GridView-Steuerelements. |
| RowStyle | Die stileinstellungen für die Datenzeilen im GridView-Steuerelement. Wenn die **AlternatingRowStyle** -Eigenschaft ebenfalls festgelegt wird, werden die Datenzeilen abwechselnd angezeigt der **RowStyle** Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| SelectedRowStyle | Die stileinstellungen für die ausgewählte Zeile im GridView-Steuerelement. |

Sie können auch ein- oder Ausblenden von verschiedenen Teilen des Steuerelements. Die folgende Tabelle enthält die Eigenschaften, die steuern, welche Teile angezeigt oder ausgeblendet werden.

| **Property** | **Beschreibung** |
| --- | --- |
| ShowFooter | Anzeigen oder Ausblenden den Fußzeilenbereich des GridView-Steuerelement. |
| ShowHeader | Anzeigen oder Ausblenden der Headerbereich des GridView-Steuerelements. |

### <a name="events"></a>Ereignisse

Das GridView-Steuerelement bietet mehrere Ereignisse, denen Sie programmieren können. Dadurch können Sie eine benutzerdefinierte Routine ausgeführt wird, sobald ein Ereignis eintritt. Die folgende Tabelle enthält die Ereignisse, die von der GridView-Steuerelement unterstützt werden.

| **Event** | **Beschreibung** |
| --- | --- |
| PageIndexChanged | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, allerdings nachdem das GridView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, wenn eine Aufgabe auszuführen, nachdem der Benutzer auf eine andere Seite im Steuerelement navigiert werden muss. |
| PageIndexChanging | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber vor der GridView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |
| RowCancelingEdit | Tritt auf, wenn eine Zeile auf die Schaltfläche "Abbrechen" geklickt wird, aber bevor das GridView-Steuerelement den Bearbeitungsmodus verlässt. Dieses Ereignis wird häufig verwendet, beenden Sie den Abbruchvorgang. |
| RowCommand | Tritt auf, wenn eine Schaltfläche im GridView-Steuerelement geklickt wird. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn eine Schaltfläche in das Steuerelement geklickt wird. |
| RowCreated | Tritt auf, wenn eine neue Zeile im GridView-Steuerelement erstellt wird. Dieses Ereignis wird häufig verwendet, ändern Sie den Inhalt einer Zeile aus, wenn die Zeile erstellt wird. |
| RowDataBound | Tritt auf, wenn eine Datenzeile an Daten in einem GridView-Steuerelement gebunden ist. Dieses Ereignis wird häufig verwendet, ändern Sie den Inhalt einer Zeile aus, wenn die Zeile an Daten gebunden ist. |
| RowDeleted | Tritt auf, wenn auf die Schaltfläche zum Löschen einer Zeile geklickt wird, nachdem das GridView-Steuerelement den Datensatz aus der Datenquelle gelöscht. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| RowDeleting | Tritt auf, wenn die Schaltfläche zum Löschen einer Zeile geklickt wird, aber bevor der GridView-Steuerelement, den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| RowEditing | Tritt auf, wenn eine Zeile auf die Schaltfläche "Bearbeiten" geklickt wird, aber vor der GridView Steuerelement in den Bearbeitungsmodus wechselt. Dieses Ereignis wird häufig verwendet, um den Vorgang abzubrechen. |
| RowUpdated | Tritt auf, wenn eine Zeile auf die Schaltfläche "Aktualisieren" geklickt wird, allerdings nachdem das GridView-Steuerelement die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Ergebnisse der Update-Vorgang zu überprüfen. |
| RowUpdating | Tritt auf, wenn eine Zeile auf die Schaltfläche "Aktualisieren" geklickt wird, aber vor der GridView-Steuerelement die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Aktualisierung abzubrechen. |
| SelectedIndexChanged | Tritt auf, wenn eine Zeile die auswählen-Schaltfläche geklickt wird, allerdings nachdem das GridView-Steuerelement den Auswahlvorgang behandelt. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, nachdem eine Zeile im Steuerelement ausgewählt ist. |
| SelectedIndexChanging | Tritt auf, wenn eine Zeile die auswählen-Schaltfläche geklickt wird, aber vor der GridView-Steuerelement den Auswahlvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Auswahlvorgang abzubrechen. |
| Sortiert | Tritt auf, wenn der Link zum Sortieren einer Spalte geklickt wird, allerdings nachdem das GridView-Steuerelement den Sortiervorgang behandelt. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, nachdem der Benutzer auf einen Link zum Sortieren einer Spalte geklickt hat. |
| Sortieren | Tritt auf, wenn der Link zum Sortieren einer Spalte geklickt wird, aber vor der GridView-Steuerelement den Sortiervorgang behandelt. Dieses Ereignis wird häufig verwendet, den Sortiervorgang abbrechen oder eine benutzerdefinierte Sortierung Routine durchführen. |

## <a name="formview"></a>FormView

Das FormView-Steuerelement wird verwendet, um die Anzeige eines einzelnen Datensatzes aus einer Datenquelle. Es ähnelt der DetailsView-Steuerelement, mit der Ausnahme von benutzerdefinierten Vorlagen anstelle von Zeilenfeldern wird angezeigt. Erstellen eigener Vorlagen bietet Ihnen mehr Flexibilität bei der Steuerung, wie die Daten angezeigt werden. Das FormView-Steuerelement unterstützt die folgenden Features:

- Die Bindung an Datenquellen-Steuerelemente wie SqlDataSource und ObjectDataSource-Steuerelement.
- Integrierte Einfügefunktionen.
- Integrierte aktualisieren und Löschen von Funktionen.
- Integrierte Pagingfunktionen.
- Den programmgesteuerten Zugriff auf das FormView-Objektmodell zum dynamischen Festlegen von Eigenschaften, Ereignisse verarbeiten und so weiter.
- Anpassbare Darstellung durch benutzerdefinierte Vorlagen, Designs und Stile.

## <a name="templates"></a>Vorlagen

Für das FormView-Steuerelement, um Inhalt anzuzeigen müssen Sie zum Erstellen von Vorlagen für die verschiedenen Teile des Steuerelements. Die meisten Vorlagen ist optional. Allerdings müssen Sie eine Vorlage für den Modus erstellen, in denen das Steuerelement konfiguriert ist. Beispielsweise muss ein FormView-Steuerelement, das Einfügen von Datensätzen unterstützt eine Insert-Item-Vorlage definiert haben. Die folgende Tabelle enthält die verschiedenen Vorlagen, die Sie erstellen können.

| **Vorlagentyp** | **Beschreibung** |
| --- | --- |
| EditItemTemplate | Definiert den Inhalt für die Datenzeile, wenn das FormView-Steuerelement im Bearbeitungsmodus befindet. Diese Vorlage enthält normalerweise die Benutzereingabe-Steuerelemente und Befehlsschaltflächen, mit denen der Benutzer einen vorhandenen Datensatz bearbeiten kann. |
| EmptyDataTemplate | Definiert den Inhalt für die leere Datenzeile angezeigt, wenn das FormView-Steuerelement an eine Datenquelle gebunden ist, die keine Datensätze enthält. Diese Vorlage enthält in der Regel Inhalte an den Benutzer darauf aufmerksam, dass die Datenquelle keine Datensätze enthält. |
| FooterTemplate | Definiert den Inhalt für die Footerzeile. Diese Vorlage enthält in der Regel zusätzliche Inhalte, die Sie in der Fußzeile anzeigen möchten. Als Alternative können Sie einfach Text, der in der Fußzeile angezeigt wird, durch Festlegen der Eigenschaft FooterText angeben. |
| Header-Vorlage | Definiert den Inhalt für die Headerzeile. Diese Vorlage enthält in der Regel zusätzliche Inhalte, die Sie in der Kopfzeile anzeigen möchten. Als Alternative können Sie einfach Text, der in der Kopfzeile angezeigt wird, durch Festlegen der HeaderText-Eigenschaft angeben. |
| ItemTemplate-Element | Definiert den Inhalt für die Datenzeile, wenn das FormView-Steuerelement im schreibgeschützten Modus befindet. Diese Vorlage enthält in der Regel Inhalte aus, um die Werte eines vorhandenen Datensatzes anzuzeigen. |
| InsertItemTemplate | Definiert den Inhalt für die Datenzeile, wenn das FormView-Steuerelement im Einfügemodus befindet. Diese Vorlage enthält normalerweise die Benutzereingabe-Steuerelemente und Befehlsschaltflächen, mit denen der Benutzer einen neuen Datensatz hinzufügen kann. |
| PagerTemplate | Definiert den Inhalt der Pagerzeile angezeigt, wenn das Pagingfeature aktiviert ist (wenn die AllowPaging-Eigenschaft auf festgelegt ist **"true"**). Diese Vorlage enthält in der Regel Steuerelemente, die mit denen der Benutzer zu einem anderen Datensatz navigieren kann. |

Benutzereingabe-Steuerelemente in der Elementvorlage bearbeiten und Einfügen-Item-Vorlage können auf die Felder für die Datenquelle gebunden werden, mithilfe eines Ausdrucks für die bidirektionale Bindung. Dadurch wird das FormView-Steuerelement automatisch Extrahieren der Werte des Eingabesteuerelements für ein Update oder insert-Vorgang. Bidirektionale Bindungsausdrücke ermöglichen außerdem die Benutzereingabe-Steuerelemente in einer Elementvorlage bearbeiten, die ursprünglichen Werte des Felds automatisch angezeigt.

### <a name="binding-to-data"></a>Binden an Daten

Das FormView-Steuerelement an ein Datenquellensteuerelement gebunden werden kann (z. B. **SqlDataSource**, AccessDataSource, **"ObjectDataSource"** und so weiter), oder für jede Datenquelle, die implementiert die ' System.Collections.IEnumerable '-Schnittstelle (z. B. System.Data.DataView System.Collections.ArrayList und System.Collections.Hashtable). Verwenden Sie eine der folgenden Methoden, um das FormView-Steuerelement an den entsprechenden Datenquellentyp zu binden:

- Um an ein Datenquellen-Steuerelement zu binden, wird die Eigenschaft DataSourceID-Wert, der das FormView-Steuerelement auf den ID-Wert, der das Datenquellen-Steuerelement fest. Das FormView-Steuerelement wird automatisch an der angegebenen Datenquellen-Steuerelement gebunden, und profitieren von der Datenquelle des Steuerelements-Funktionen zum Ausführen von einfügen, aktualisieren, löschen und Pagingfunktionen. Dies ist die bevorzugte Methode zum Binden an Daten.
- Mit einer Datenquelle zu binden, implementiert die **System.Collections.IEnumerable** Schnittstelle, legen Sie die DataSource-Eigenschaft, der das FormView-Steuerelement programmgesteuert auf die Datenquelle aus, und rufen Sie dann die DataBind-Methode. Wenn Sie diese Methode verwenden zu können, bietet das FormView-Steuerelement keine integrierte einfügen, aktualisieren, löschen und Pagingfunktionen. Sie müssen diese Funktionen bereitstellen, indem Sie die Verwendung des entsprechenden Ereignisses.

## <a name="data-operations"></a>Datenvorgänge

Das FormView-Steuerelement bietet zahlreiche integrierte Funktionen, mit denen den Benutzer zu aktualisieren, löschen, einfügen und Paging von Elementen im Steuerelement. Wenn das FormView-Steuerelement an ein Datenquellensteuerelement gebunden ist, kann das FormView-Steuerelement der Datenquelle des Steuerelements-Funktionen nutzen und bieten automatische aktualisieren, löschen, einfügen und Auslagern von Funktionen. Das FormView-Steuerelement kann Update, Delete, Insert und Pagingvorgänge bei anderen Arten von Datenquellen unterstützen; Allerdings müssen Sie einen entsprechenden Ereignishandler mit der Implementierung für diese Vorgänge bereitstellen.

Da Vorlagen von FormView-Steuerelement verwendet wird, stellt es eine Möglichkeit zum automatischen Generieren von Befehlsschaltflächen zum Ausführen, aktualisieren, löschen oder Einfügen von Vorgängen keine bereit. Sie müssen diese Befehlsschaltflächen manuell in die entsprechende Vorlage einschließen. Das FormView-Steuerelement erkennt bestimmte Schaltflächen, die ihre **CommandName** Eigenschaften, die auf bestimmte Werte festgelegt sind. Die folgende Tabelle enthält die Befehlsschaltfläche, die das FormView-Steuerelement erkennt.

| **Button** (Schaltfläche) | **CommandName-Wert** | **Beschreibung** |
| --- | --- | --- |
| Abbrechen | "Abbrechen" | Verwendet, aktualisieren oder Einfügen von Vorgängen, den Vorgang abzubrechen und um die vom Benutzer eingegebenen Werte zu verwerfen. Gibt zurück, klicken Sie dann das FormView-Steuerelement in den Modus durch die Eigenschaft "DefaultMode" angegeben. |
| Löschen | "Löschen" | Zum Löschen der Vorgänge um angezeigten Datensatzes aus der Datenquelle zu löschen. Löst die Ereignisse ItemDeleting und ItemDeleted. |
| Bearbeiten | "Bearbeiten" | Verwendet in Aktualisierungsvorgängen das FormView-Steuerelement im Bearbeitungsmodus zu versetzen. Der Inhalt die **EditItemTemplate** Eigenschaft wird für die Datenzeile angezeigt. |
| Insert | "Insert" | Wird in Einfügevorgängen verwendet, um zu versuchen, einen neuen Datensatz in der Datenquelle, die mit den Werten, die vom Benutzer bereitgestellte einzufügen. Löst die Ereignisse ItemInserting und ItemInserted. |
| Neu | "New" | Verwendet in Einfügevorgängen einzufügenden das FormView-Steuerelement im Einfügemodus befindet. Der Inhalt die **InsertItemTemplate** Eigenschaft wird für die Datenzeile angezeigt. |
| Seite | "Page" | In Pagingvorgänge verwendet, um die eine Schaltfläche in der Pagerzeile darstellt, die Paging ausführt. Legen Sie zum Angeben des Pagingvorgangs der **CommandArgument** Eigenschaft der Schaltfläche "Weiter", "Prev", "First", "Last" oder der Index der Seite, zu der navigiert. Löst die Ereignisse "PageIndexChanging" ausgelöst und PageIndexChanged. |
| Update | "Aktualisieren" | In Aktualisierungsvorgängen verwendet, um zu versuchen, den angezeigten Datensatz in der Datenquelle mit den Werten, die vom Benutzer bereitgestellte aktualisieren. Löst das ItemUpdating und ItemUpdated-Ereignis aus. |

Schaltfläche (die löscht angezeigten Datensatzes sofort), wenn die Schaltfläche "Bearbeiten" oder "New" geklickt wird, die FormView-Steuerelement in den Bearbeitungsmodus wechselt oder Einfügemodus bzw., im Gegensatz zu den Löschvorgang. Im Bearbeitungsmodus befindet, der Inhalt enthalten, der **EditItemTemplate** Eigenschaft wird für das aktuelle Datenelement angezeigt. In der Regel ist die Bearbeitungselementvorlage so definiert, dass die Schaltfläche "Bearbeiten" mit einem Update und Schaltfläche "Abbrechen" ersetzt wird. Eingabesteuerelemente, die für Sie in der Datentyp des Felds (z. B. ein Textfeld oder ein CheckBox-Steuerelement) geeignet sind, werden auch in der Regel mit der Wert eines Felds für den Benutzer zum Ändern angezeigt. Den Datensatz in der Datenquelle, klicken Sie auf die Schaltfläche "Aktualisieren" aktualisiert werden, während Änderungen auf die Schaltfläche "Abbrechen" abgebrochen werden.

Entsprechend den Inhalt in die **InsertItemTemplate** Eigenschaft wird für das Datenelement angezeigt, wenn das Steuerelement im Einfügemodus befindet. Die Insert-Item-Vorlage ist in der Regel so definiert, dass die Schaltfläche "neue" wird mit einem Einfüge- und Schaltfläche "Abbrechen" ersetzt, und leeren Benutzereingabe-Steuerelemente werden angezeigt, für den Benutzer die Werte für den neuen Datensatz eingeben. Klicken Sie auf die Schaltfläche "Insert" Einfügen des Datensatzes in der Datenquelle, während Änderungen auf die Schaltfläche "Abbrechen" abgebrochen werden.

Das FormView-Steuerelement stellt eine Paging-Funktion, die dem Benutzer ermöglicht, navigieren Sie zu anderen Datensätzen in der Datenquelle bereit. Wenn aktiviert, wird eine Pagerzeile in das FormView-Steuerelement angezeigt, die Steuerelemente für die Seitennavigation enthält. Legen Sie zum Aktivieren von Paging das **AllowPaging** Eigenschaft **"true"**. Sie können die Pagerzeile anpassen, durch Festlegen der Eigenschaften der Objekte in der PagerStyle und die PagerSettings-Eigenschaft. Anstatt die Pagerzeile integrierte Benutzeroberfläche zu verwenden, können Sie Ihre eigene Benutzeroberfläche erstellen, mit der **PagerTemplate** Eigenschaft.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des das FormView-Steuerelement anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuerelements festlegen. Die folgende Tabelle enthält die verschiedenen Stileigenschaften.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| EditRowStyle | Die stileinstellungen für die Datenzeile, wenn das FormView-Steuerelement im ist Bearbeitungsmodus befindet. |
| EmptyDataRowStyle | Die stileinstellungen für die leere Datenzeile in das FormView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die stileinstellungen für die Footerzeile, der das FormView-Steuerelement. |
| HeaderStyle | Die stileinstellungen für die Kopfzeile der FormView-Steuerelement. |
| InsertRowStyle | Die stileinstellungen für die Datenzeile, wenn das FormView-Steuerelement im ist Einfügemodus befindet. |
| PagerStyle | Die stileinstellungen für die Pagerzeile in das FormView-Steuerelement angezeigt wird, wenn das Pagingfeature aktiviert ist. |
| RowStyle | Die stileinstellungen für die Datenzeile, wenn das FormView-Steuerelement im schreibgeschützten Modus befindet. |

## <a name="events"></a>Ereignisse

Das FormView-Steuerelement bietet mehrere Ereignisse, denen Sie programmieren können. Dadurch können Sie eine benutzerdefinierte Routine ausgeführt wird, sobald ein Ereignis eintritt. Die folgende Tabelle enthält die Ereignisse, die durch das FormView-Steuerelement unterstützt.

| **Event** | **Beschreibung** |
| --- | --- |
| ItemCommand | Tritt auf, wenn eine Schaltfläche in einem FormView-Steuerelement geklickt wird. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn eine Schaltfläche in das Steuerelement geklickt wird. |
| ItemCreated | Tritt auf, nachdem alle FormViewRow-Objekte in das FormView-Steuerelement erstellt werden. Dieses Ereignis wird häufig verwendet, um die Werte eines Datensatzes zu ändern, bevor er angezeigt wird. |
| ItemDeleted | Tritt auf, wenn eine Schaltfläche "löschen" (eine Schaltfläche mit der **CommandName** -Eigenschaft auf "Löschen" festgelegt) geklickt wird, allerdings nachdem das FormView-Steuerelement den Datensatz aus der Datenquelle gelöscht. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| ItemDeleting | Tritt auf, wenn eine Löschen-Schaltfläche geklickt wird, aber bevor FormView-Steuerelement, den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| ItemInserted | Tritt auf, wenn eine Schaltfläche Einfügen (eine Schaltfläche mit der **CommandName** -Eigenschaft auf "Insert" festgelegt) geklickt wird, allerdings nachdem das FormView-Steuerelement den Datensatz eingefügt. Dieses Ereignis wird häufig verwendet, um die Ergebnisse der Insert-Vorgang zu überprüfen. |
| ItemInserting | Tritt auf, wenn eine Einfügeschaltfläche geklickt wird, aber vor der FormView-Steuerelement des Datensatzes einfügen. Dieses Ereignis wird häufig verwendet, den Insert-Vorgang abgebrochen. |
| ItemUpdated | Tritt auf, wenn eine Aktualisieren-Schaltfläche (eine Schaltfläche mit der **CommandName** -Eigenschaft auf "Aktualisieren" festgelegt) geklickt wird, allerdings nachdem das FormView-Steuerelement die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Ergebnisse der Update-Vorgang zu überprüfen. |
| ItemUpdating | Tritt auf, wenn eine Aktualisieren-Schaltfläche geklickt wird, aber bevor das FormView-Steuerelement den Datensatz aktualisiert. Dieses Ereignis wird häufig verwendet, um den Updatevorgang abzubrechen. |
| ModeChanged | Tritt auf, nachdem das FormView-Steuerelement den Modus ändert (zu bearbeiten, einfügen oder nur-Lese Modus). Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn das FormView-Steuerelement den Modus ändert. |
| ModeChanging | Tritt auf, bevor das FormView-Steuerelement den Modus ändert (zu bearbeiten, einfügen oder nur-Lese Modus). Dieses Ereignis wird häufig verwendet, eine die modusänderung abzubrechen. |
| PageIndexChanged | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, allerdings nachdem das FormView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, wenn eine Aufgabe auszuführen, nachdem der Benutzer zu einem anderen Datensatz in das Steuerelement navigiert werden muss. |
| PageIndexChanging | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber bevor das FormView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |

## <a name="detailsview"></a>DetailsView

DetailsView-Steuerelement wird verwendet, um einen einzelnen Datensatz aus einer Datenquelle in einer Tabelle anzuzeigen, in dem jedes Feld des Datensatzes in einer Zeile der Tabelle angezeigt. Sie können in Kombination mit einem GridView-Steuerelement für die Master-Detail-Szenarien verwendet werden. Das DetailsView-Steuerelement unterstützt die folgenden Features:

- Die Bindung an Datenquellen-Steuerelemente, z. B. SqlDataSource-Steuerelement.
- Integrierte Einfügefunktionen.
- Integrierte aktualisieren und Löschen von Funktionen.
- Integrierte Pagingfunktionen.
- Den programmgesteuerten Zugriff auf das DetailsView-Objektmodell zum dynamischen Festlegen von Eigenschaften, Ereignisse verarbeiten und so weiter.
- Anpassbare Darstellung durch Designs und Stile.

## <a name="row-fields"></a>Felder der Überschriftenzeile

Jede Datenzeile im DetailsView-Steuerelement wird durch das Deklarieren eines Feldsteuerelements erstellt. Andere Zeilenfeldtypen bestimmen das Verhalten der Zeilen im Steuerelement. Feldsteuerelemente, abgeleitet von DataControlField ab. Die folgende Tabelle enthält die andere Zeilenfeldtypen, die verwendet werden können.

| **Feldtyp für Spalte** | **Beschreibung** |
| --- | --- |
| BoundField | Zeigt den Wert eines Felds in einer Datenquelle als Text an. |
| ButtonField | Zeigt eine Befehlsschaltfläche im DetailsView-Steuerelement an. Dadurch können Sie eine Zeile mit einer benutzerdefinierten Schaltflächen-Steuerelement, z. B. eines "Add" oder eine Schaltfläche "entfernen" anzeigen. |
| CheckBoxField | Zeigt ein Kontrollkästchen im DetailsView-Steuerelement. Diese Zeilenfeldtyp wird häufig verwendet, um Felder mit einem booleschen Wert anzuzeigen. |
| CommandField | Integrierten Befehl zeigt Schaltflächen zum Ausführen von "Bearbeiten", insert oder delete-Vorgänge im DetailsView-Steuerelement. |
| HyperLinkField | Zeigt den Wert eines Felds in einer Datenquelle, als Link. Dieses Feld Zeilentyp können Sie ein zweites Feld an die URL des Links zu binden. |
| ImageField | Zeigt ein Bild im DetailsView-Steuerelement. |
| TemplateField | Zeigt den benutzerdefinierten Inhalt für eine Zeile in einer angegebenen Vorlage entsprechend DetailsView-Steuerelement. Dieses Feld Zeilentyp können Sie ein benutzerdefiniertes Zeilenfeld zu erstellen. |

In der Standardeinstellung die AutoGenerateRows-Eigenschaftensatz auf **"true"**, die einen gebundenen Zeile Field-Objekt für jedes Feld eines Typs bindbare in der Datenquelle automatisch generiert. Gültige bindbare Typen sind, String, DateTime, Decimal, Guid und der Satz von primitiven Typen. Jedes Feld wird in einer Zeile als Text in der Reihenfolge angezeigt in der jedes Feld in der Datenquelle angezeigt wird.

Automatische Generierung von Zeilen bietet eine schnelle und einfache Möglichkeit, um jedes Feld im Datensatz anzuzeigen. Zum Verwenden von DetailsView erweiterten des Steuerelements jedoch Funktionen, die Sie explizit deklarieren müssen, die Zeilenfelder DetailsView-Steuerelement einschließt. Deklarieren die Felder der Überschriftenzeile, legen Sie zuerst die **AutoGenerateRows** Eigenschaft **"false"**. Fügen Sie als Nächstes öffnende und schließende **&lt;Felder&gt;** Tags zwischen die öffnenden und schließenden Tags eines DetailsView-Steuerelement. Listen Sie abschließend die Zeilenfelder, die zwischen den öffnenden und schließenden enthalten sein sollen **&lt;Felder&gt;** Tags. Der Fields-Sammlung in der aufgeführten Reihenfolge werden die angegebenen Zeilenfelder hinzugefügt. Die **Felder** Sammlung können Sie die Zeilenfelder im DetailsView-Steuerelement programmgesteuert zu verwalten.

> [!NOTE]
> Automatisch generierte, die Felder der Fields-Sammlung nicht hinzugefügt werden.


## <a name="binding-to-data"></a>Binden an Daten

DetailsView-Steuerelement gebunden werden kann, ein Datenquellen-Steuerelement, z. B. **SqlDataSource** oder AccessDataSource, oder für jede Datenquelle, die die System.Collections.IEnumerable-Schnittstelle, wie z. B. System.Data.DataView, implementiert. System.Collections.ArrayList und System.Collections.Hashtable.

Verwenden Sie eine der folgenden Methoden zum Binden von DetailsView-Steuerelement an den entsprechenden Datenquellentyp:

- Um an ein Datenquellen-Steuerelement zu binden, wird die Eigenschaft "DataSourceID" von DetailsView-Steuerelement auf den ID-Wert, der das Datenquellen-Steuerelement fest. Das DetailsView-Steuerelement wird automatisch an der angegebenen Datenquellen-Steuerelement gebunden. Dies ist die bevorzugte Methode zum Binden an Daten.
- Mit einer Datenquelle zu binden, implementiert die **System.Collections.IEnumerable** Schnittstelle, legen Sie die DataSource-Eigenschaft von DetailsView-Steuerelement programmgesteuert auf die Datenquelle aus, und rufen Sie dann die DataBind-Methode.

## <a name="security"></a>Sicherheit

Dieses Steuerelement kann verwendet werden, zum Anzeigen von Benutzereingaben, u. u. bösartige Clientskripts enthalten können. Überprüfen Sie alle Informationen, die für das ausführbare Skript, SQL-Anweisungen oder anderen Code von einem Client gesendet wird, vor der Anzeige in Ihrer Anwendung. ASP.NET bietet eine Funktion für den Überprüfung eingabeanforderung blockskript und HTML in einer Benutzereingabe.

## <a name="data-operations"></a>Datenvorgänge

DetailsView-Steuerelement bietet integrierte Funktionen, mit denen den Benutzer zu aktualisieren, löschen, einfügen und Paging von Elementen im Steuerelement. Wenn DetailsView-Steuerelement an ein Datenquellensteuerelement gebunden ist, kann DetailsView-Steuerelement von der Datenquelle des Steuerelements-Funktionen nutzen und bieten automatische aktualisieren, löschen, einfügen und Auslagern von Funktionen.

Das DetailsView-Steuerelement kann Update, Delete, Insert und Pagingvorgänge bei anderen Arten von Datenquellen unterstützen; Allerdings müssen Sie die Implementierung für diese Vorgänge in einem geeigneten Ereignishandler bereitstellen.

Das DetailsView-Steuerelement auch automatisch hinzugefügt. eine **CommandField** Zeilenfeld mit einer Schaltfläche Bearbeiten, löschen oder neu durch Festlegen der Eigenschaften AutoGenerateEditButton AutoGenerateDeleteButton oder AutoGenerateInsertButton zu **"true"** bzw. Im Gegensatz zu den Löschvorgang Schaltfläche (die löscht des ausgewählten Datensatzes sofort), wenn die Schaltfläche "Bearbeiten" oder "New" geklickt wird, die DetailsView-Steuerelement in den Bearbeitungsmodus wechselt, oder einfügen. Im Bearbeitungsmodus befindet wird die Schaltfläche "Bearbeiten" mit einem Update und Schaltfläche "Abbrechen" ersetzt. Eingabesteuerelemente, die für Sie in der Datentyp des Felds (z. B. ein Textfeld oder ein CheckBox-Steuerelement) geeignet sind, werden mit der Wert eines Felds für den Benutzer zu ändern, angezeigt. Den Datensatz in der Datenquelle, klicken Sie auf die Schaltfläche "Aktualisieren" aktualisiert werden, während Änderungen auf die Schaltfläche "Abbrechen" abgebrochen werden. Ebenso im Einfügemodus befindet, die Schaltfläche "neue" wird mit einem Einfüge- und Schaltfläche "Abbrechen" ersetzt, und leeren Benutzereingabe-Steuerelemente werden angezeigt, für den Benutzer die Werte für den neuen Datensatz eingeben.

Das DetailsView-Steuerelement stellt eine Paging-Funktion, die dem Benutzer ermöglicht, navigieren Sie zu anderen Datensätzen in der Datenquelle bereit. Wenn aktiviert, werden die Steuerelemente für die Seitennavigation in einer Pagerzeile angezeigt. Legen Sie zum Aktivieren von Paging die AllowPaging-Eigenschaft auf **"true"**. Die Pagerzeile kann mithilfe der PagerStyle und PagerSettings Eigenschaften angepasst werden.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung der DetailsView-Steuerelement anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuerelements festlegen. Die folgende Tabelle enthält die verschiedenen Stileigenschaften.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| AlternatingRowStyle | Die stileinstellungen für die abwechselnde Zeilen im DetailsView-Steuerelement. Wenn diese Eigenschaft festgelegt ist, die Datenzeilen werden angezeigt Wechsel zwischen den RowStyle-Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| CommandRowStyle | Die stileinstellungen für die Zeile mit den integrierten Befehlsschaltflächen im DetailsView-Steuerelement. |
| EditRowStyle | Bearbeitungsmodus die stileinstellungen für die Datenzeilen, wenn im DetailsView-Steuerelement befindet. |
| EmptyDataRowStyle | Die stileinstellungen für die leere Datenzeile im DetailsView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die stileinstellungen für die Footerzeile von DetailsView-Steuerelement. |
| HeaderStyle | Die stileinstellungen für die Kopfzeile der DetailsView-Steuerelement. |
| InsertRowStyle | Die stileinstellungen für die Datenzeilen, wenn DetailsView-Steuerelement in ist Einfügemodus. |
| PagerStyle | Die stileinstellungen für die Pagerzeile von DetailsView-Steuerelement. |
| RowStyle | Die stileinstellungen für die Datenzeilen im DetailsView-Steuerelement. Wenn die **AlternatingRowStyle** -Eigenschaft ebenfalls festgelegt wird, werden die Datenzeilen abwechselnd angezeigt der **RowStyle** Einstellungen und die **AlternatingRowStyle** Einstellungen. |
| FieldHeaderStyle | Die stileinstellungen für die Headerspalte "der DetailsView-Steuerelement. |

## <a name="events"></a>Ereignisse

Das DetailsView-Steuerelement bietet mehrere Ereignisse, denen Sie programmieren können. Dadurch können Sie eine benutzerdefinierte Routine ausgeführt wird, sobald ein Ereignis eintritt. Die folgende Tabelle enthält die Ereignisse, die von DetailsView-Steuerelement unterstützt werden. Das DetailsView-Steuerelement erbt außerdem diese Ereignisse aus ihrer Basisklassen: Datenbindung, Datenbindung, Disposed, Init, Load, PreRender und rendern.

| **Event** | **Beschreibung** |
| --- | --- |
| ItemCommand | Tritt auf, wenn eine Schaltfläche im DetailsView-Steuerelement geklickt wird. |
| ItemCreated | Tritt auf, nachdem alle DetailsViewRow-Objekte im DetailsView-Steuerelement erstellt werden. Dieses Ereignis wird häufig verwendet, um die Werte eines Datensatzes zu ändern, bevor er angezeigt wird. |
| ItemDeleted | Tritt auf, wenn eine Löschen-Schaltfläche geklickt wird, aber nachdem DetailsView-Steuerelement den Datensatz aus der Datenquelle gelöscht. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| ItemDeleting | Tritt auf, wenn eine Löschen-Schaltfläche geklickt wird, aber bevor DetailsView-Steuerelement, den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| ItemInserted | Tritt auf, wenn auf eine Einfügeschaltfläche geklickt wird, und zwar nachdem DetailsView-Steuerelement den Datensatz eingefügt wird. Dieses Ereignis wird häufig verwendet, um die Ergebnisse der Insert-Vorgang zu überprüfen. |
| ItemInserting | Tritt auf, wenn eine Einfügeschaltfläche geklickt wird, aber bevor DetailsView-Steuerelement des Datensatzes einfügen. Dieses Ereignis wird häufig verwendet, den Insert-Vorgang abgebrochen. |
| ItemUpdated | Tritt auf, wenn eine Aktualisieren-Schaltfläche geklickt wird, allerdings nachdem das DetailsView-Steuerelement die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um die Ergebnisse der Update-Vorgang zu überprüfen. |
| ItemUpdating | Tritt auf, wenn eine Aktualisieren-Schaltfläche geklickt wird, aber bevor DetailsView-Steuerelement den Datensatz aktualisiert. Dieses Ereignis wird häufig verwendet, um den Updatevorgang abzubrechen. |
| ModeChanged | Tritt auf, nachdem das DetailsView-Steuerelement Modi (bearbeiten, einfügen oder nur-Lese Modus) geändert. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn das DetailsView-Steuerelement den Modus ändert. |
| ModeChanging | Tritt auf, bevor das DetailsView-Steuerelement Modi (bearbeiten, einfügen oder nur-Lese Modus) geändert. Dieses Ereignis wird häufig verwendet, eine die modusänderung abzubrechen. |
| PageIndexChanged | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, allerdings nachdem DetailsView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, wenn eine Aufgabe auszuführen, nachdem der Benutzer zu einem anderen Datensatz in das Steuerelement navigiert werden muss. |
| PageIndexChanging | Tritt auf, wenn eine der Pagerschaltflächen geklickt wird, aber bevor das DetailsView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |

## <a name="the-menu-control"></a>Menu-Steuerelement

Das Steuerelement in ASP.NET 2.0 fungiert als ein Navigationssystem mit vollem Funktionsumfang. Es kann über eine Datenbindung an hierarchische Datenquellen wie z. B. die SiteMapDataSource ganz einfach sein.

Steuerelemente mit eine Menüstruktur kann deklarativ oder dynamisch definiert werden, und es besteht aus einem einzelnen Stammknoten und eine beliebige Anzahl von untergeordneten Knoten. Der folgende Code definiert deklarativ ein Menü für das Steuerelement.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Im obigen Beispiel ist der Home.aspx-Knoten den Stammknoten. Alle anderen Knoten, die innerhalb des Stammknotens auf verschiedenen Ebenen geschachtelt sind.

Es gibt zwei Arten von Menüs, die das Steuerelement gerendert werden kann. Menüs mit statischen und dynamischen Menüs. Statischen Menüs bewirkt bestehen von Menüelementen, die immer sichtbar sind. Dynamische Menüs bestehen aus Menüelemente, die nur angezeigt werden, wenn der Benutzer mit der Maus darauf zeigt. Kunden können häufig verwechseln, statischen Menüs bewirkt mit Menüs, die deklarativ definiert und dynamischen Menüs mit Menüs, die Datenbindung zur Laufzeit sind. In der Tat sind die dynamischen und statischen Menüs unabhängig von der Methode der Auffüllung. Die Begriffe *statische* und *dynamische* finden Sie nur an, und zwar unabhängig davon, ob Sie im Menü statisch standardmäßig angezeigten oder nur angezeigt, wenn der Benutzer eine Aktion ausführt.

Die **StaticDisplayLevels** Eigenschaft wird verwendet, um die konfigurieren, wie viele Ebenen des Menüs statisch und aus diesem Grund wird standardmäßig angezeigt werden. Im obigen Beispiel, das Festlegen der **StaticDisplayLevels** Eigenschaft mit einem Wert von 2 würde dazu führen, dass im Menü anzuzeigenden statisch dem Home-Knoten, der Music-Knoten und der Filme-Knoten. Alle anderen Knoten würde dynamisch angezeigt werden, wenn der Benutzer über den übergeordneten Knoten bewegt wird.

Die **MaximumDynamicDisplayLevels** Eigenschaft konfiguriert die maximale Anzahl der Ebenen der dynamischen Menü angezeigt werden können. Dynamische Menüs auf einer höheren Ebene als der Wert von der **MaximumDynamicDisplayLevels** Eigenschaft werden verworfen.

> [!NOTE]
> Es ist fast sicher, dass Situationen auftreten können, in denen Menüs nicht angezeigt werden, aufgrund der MaximumDynamicDisplayLevels-Eigenschaft zu rendern. In diesen Fällen stellen Sie sicher, dass die Eigenschaft ausreichend festgelegt ist, um die Anzeige der Kunden Menüs zu ermöglichen.


## <a name="data-binding-the-menu-control"></a>Das Steuerelement für die Datenbindung

Das Menüsteuerelement kann an jede beliebige hierarchische Datenquelle wie z. B. die SiteMapDataSource oder die XMLDataSource gebunden werden. Die SiteMapDataSource ist die häufigste Methode für die Datenbindung an ein Steuerelement, da Datenfeeds auf der Web.sitemap-Datei und ihr Schema eine bekannte-API, für das Menüsteuerelement stellt. Die Liste unten zeigt eine einfache Web.sitemap-Datei.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Beachten Sie, dass nur ein SiteMapNode Stammelement, in diesem Fall der Startseite das Element vorhanden ist. Mehrere Attribute können für jede SiteMapNode konfiguriert werden. Die am häufigsten verwendeten Attribute sind:

- **URL** gibt die URL, um anzuzeigen, wenn ein Benutzer das Menüelement klickt. Wenn dieses Attribut nicht vorhanden ist, wird der Knoten zurück, wenn geklickt einfach buchen.
- **Titel** gibt den Text, der auf das Menüelement angezeigt wird.
- **Beschreibung** als Dokumentation für den Knoten verwendet. Auch als QuickInfo angezeigt, wenn der Mauszeiger über den Knoten gezeigt wird.
- **SiteMapFile** geschachtelte Sitemaps ermöglicht. Dieses Attribut muss auf eine wohlgeformte ASP.NET Sitemap-Datei verweisen.
- **Rollen** ermöglicht die Darstellung eines Knotens durch ASP.NET die Einschränkung aus Sicherheitsgründen gesteuert werden.

Beachten Sie, dass während dieser Attribute alle optional sind, das Verhalten des Menüs möglicherweise nicht wie erwartet wird, wenn sie nicht angegeben werden. Z. B. wenn die *Url* -Attribut angegeben ist, aber die *Beschreibung* -Attribut ist nicht, der Knoten werden nicht angezeigt und stehen keine Möglichkeit, mit der angegebenen URL zu navigieren.

## <a name="controlling-a-menus-operation"></a>Einen Vorgang Menüs steuern

Es gibt mehrere Eigenschaften, die sich auf eine ASP.NET Menu-Steuerelement auswirken. die **Ausrichtung** -Eigenschaft, die **DisappearAfter** -Eigenschaft, die **StaticItemFormatString** -Eigenschaft, und die **StaticPopoutImageUrl**Eigenschaft sind nur einige davon.

- Die **Ausrichtung** kann festgelegt werden, entweder *horizontale* oder *vertikale* und steuert, ob die statische Menüelemente horizontal angeordnet in einer Zeile oder vertikal ausgerichtet sind, und bei gestapelt miteinander. Diese Eigenschaft wirkt sich nicht auf dynamische Menüs aus.
- Die **DisappearAfter** Eigenschaft konfiguriert, wie lange ein dynamisches Menü angezeigt bleiben soll, nachdem der Mauszeiger wurde nicht in der sie. Der Wert wird in Millisekunden und der Standardwert ist 500 angegeben. Durch Festlegen dieser Eigenschaft auf einen Wert von-1 wird das Menü nicht automatisch ausgeblendet wird. In diesem Fall wird das Menü nur entfernt, klickt der Benutzer außerhalb der im Menü.
- Die **StaticItemFormatString** Eigenschaft erleichtert das einheitliche Sprache in Ihrem Menüsystem zu verwalten. Wenn Sie diese Eigenschaft angeben *{0}* eingegeben werden, anstelle der Beschreibung, die in der Datenquelle angezeigt wird. Z. B. um das Menüelement aus Übung 1 sagen, besuchen Sie unsere Seite "Produkte" usw., würden Sie angeben besuchen Sie unsere {0} Seite für die StaticItemFormatString. Zur Laufzeit wird ASP.NET ersetzt jedes Vorkommen von {0} mit der richtigen Beschreibung für das Menüelement.
- Die **StaticPopoutImageUrl** Eigenschaft gibt an, das Bild, das verwendet wird, um anzugeben, dass es sich bei ein bestimmten Menü-Knoten über untergeordnete Knoten verfügt, die auf mit dem Mauszeiger auf ihn zugegriffen werden kann. Dynamische Menüs werden weiterhin das Standardabbild zu verwenden.

## <a name="templated-menu-controls"></a>Auf Vorlagen basierenden Menu-Steuerelemente

Das Menüsteuerelement ist ein Steuerelement mit Vorlagen und zwei verschiedene "ItemTemplates" ermöglicht. die StaticItemTemplate und die DynamicItemTemplate. Diese Vorlagen verwenden, können Sie problemlos Serversteuerelemente oder Benutzersteuerelemente Menüs hinzufügen.

Um die Vorlagen in Visual Studio .NET zu bearbeiten, klicken Sie auf die Smarttag-Schaltfläche im Menü aus, und wählen Vorlagen bearbeiten. Anschließend können Sie zwischen den StaticItemTemplate oder die DynamicItemTemplate bearbeiten.

Die StaticItemTemplate hinzugefügte Steuerelemente werden in einem statischen Menü angezeigt, beim Laden der Seite. Die DynamicItemTemplate hinzugefügte Steuerelemente werden auf alle Popup-Menüs angezeigt.

## <a name="menu-events"></a>Menü-Ereignisse

Das Steuerelement verfügt über zwei Ereignisse, die eindeutig sind; die **MenuItemClicked** und **MenuItemDatabound** Ereignis.

Die MenuItemClicked-Ereignis wird ausgelöst, wenn ein Menüelement geklickt wird. Die MenuItemDatabound-Ereignis wird ausgelöst, wenn ein Menüelement an Daten gebunden ist. Die **MenuEventArgs** übergeben, die auf das Ereignis-Handler ermöglicht den Zugriff auf das Menüelement, über die Item-Eigenschaft.

## <a name="controlling-a-menus-appearance"></a>Steuern der Darstellung eines Menüs

Sie können auch beeinflussen, die Darstellung des ein Menüsteuerelement mit mindestens einer der vielen Stile Menüs zur Verfügung. Dazu zählen **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**, und **DynamicHoverStyle**. Diese Eigenschaften werden mithilfe einer standard-HTML-Format-Zeichenfolge konfiguriert. Z. B. wirkt die folgenden den Stil für dynamische Menüs.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Wenn Sie keines der Hover-Stile verwenden, müssen Sie hinzufügen eine &lt;Head&gt; Element im auf die Seite mit den *Runat* -Elementgruppe *Server*.


Menu-Steuerelemente unterstützen auch die Verwendung von ASP.NET 2.0-Designs.

## <a name="the-treeview-control"></a>Das TreeView-Steuerelement

Das TreeView-Steuerelement zeigt Daten in einer baumartigen Struktur. Wie bei den Menu-Steuerelement, kann es problemlos an jede beliebige hierarchische Datenquelle wie z. B. die SiteMapDataSource datengebunden sein.

Die erste Frage, die Kunden wahrscheinlich Fragen über das TreeView-Steuerelement in ASP.NET 2.0 ist, und zwar unabhängig davon, ob er mit dem TreeView-IE-WebControl verknüpft ist, die für ASP.NET verfügbar war 1.x. Es ist nicht. Das ASP.NET 2.0-TreeView-Steuerelement wurde von Grund auf und bietet deutliche Verbesserung über das Internet Explorer-TreeView-Websteuerelement, die zuvor verfügbar waren.

Ich gehe nicht auf Details dazu, wie einem TreeView-Steuerelement zu einer sitezuordnung zu binden, wie sie in der genau die gleiche Weise wie das Steuerelement ausgeführt wird. Das TreeView-Steuerelement hat jedoch einige deutliche Unterschiede in der Weise, die sie ausgeführt wird.

Standardmäßig wird ein TreeView-Steuerelement vollständig erweiterte angezeigt. Ändern Sie zum Ändern der Ebene der Erweiterung nach dem ersten Laden der **ExpandDepth** -Eigenschaft des Steuerelements. Dies ist besonders wichtig, in Fällen, in denen der Strukturansicht datengebundenen nach bestimmten Knoten erweitern.

## <a name="databinding-the-treeview-control"></a>Datenbindung des TreeView-Steuerelements

Im Gegensatz zu den Menu-Steuerelement bietet der Strukturansicht sich gut für die Verarbeitung großer Datenmengen. Zusätzlich zur Datenbindung an eine SiteMapDataSource oder XMLDataSource ist der Baumstruktur aus diesem Grund häufig Daten an ein DataSet oder anderen relationalen Daten gebunden. In Fällen, in dem das TreeView-Steuerelement an die große Mengen an Daten gebunden ist, empfiehlt es sich nur an Daten gebunden werden soll, die tatsächlich im Steuerelement angezeigt wird. Anschließend können Sie Daten auf zusätzliche Daten zu binden, wie das TreeView-Knoten erweitert werden.

In diesen Fällen die **PopulateOnDemand** Eigenschaft von TreeView sollte festgelegt werden, um *"true"*. Sie müssen eine Implementierung für Bereitstellen der **TreeNodePopulate** Methode.

## <a name="data-binding-without-postback"></a>Datenbindung ohne PostBack

Beachten Sie, dass wenn Sie zum ersten Mal einen Knoten im vorherigen Beispiel erweitern, wird die Seite wieder veröffentlicht und aktualisiert. Thats kein Problem in diesem Beispiel, aber Sie können sich vorstellen, dass es in einer produktionsumgebung mit einer großen Menge von Daten sein könnte. Ein besserer Szenario wäre in der Strukturansicht weiterhin dynamisch füllen Sie die Knoten jedoch ohne einen Beitrag an den Server zurückgesendet.

Durch Festlegen der **PopulateNodesFromClient** und **PopulateOnDemand** Eigenschaften auf "true", das ASP.NET TreeView-Steuerelement werden dynamisch aufzufüllen Knoten ohne ein Postback-Ereignis. Wenn der übergeordnete Knoten erweitert wird, wird eine XMLHttp-Anforderung vom Client gesendet wird, und die OnTreeNodePopulate-Ereignis ausgelöst wird. Der Server antwortet mit einer XML-Dateninsel, die dann mit Daten verwendet wird, binden die untergeordneten Knoten.

ASP.NET erstellt dynamisch den Clientcode, der diese Funktion implementiert. Die &lt;Skript&gt; Tags, die das Skript verweist auf eine Datei AXD generiert werden. Beispielsweise zeigt die folgenden Liste die Skript-Links, um den Skriptcode, der die XMLHttp-Anforderung generiert.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Wenn Sie die Datei AXD oben in Ihrem Browser navigieren, und öffnen, sehen Sie den Code, der die XMLHttp-Anforderung implementiert. Diese Methode wird verhindert, dass Kunden die Skriptdatei ändern.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Steuerung der Ausführung des TreeView-Steuerelements

Das TreeView-Steuerelement verfügt über mehrere Eigenschaften, die den Betrieb des Steuerelements beeinflussen. Die offensichtlichsten Eigenschaften sind die **ShowCheckBoxes**, **ShowExpandCollapse**, und **ShowLines**.

Die **ShowCheckBoxes** Eigenschaft wirkt sich auf, und zwar unabhängig davon, ob Knoten ein Kontrollkästchen, die beim Rendern angezeigt. Die gültigen Werte für diese Eigenschaft sind **keine**, **Stamm**, **übergeordneten**, **Blattebene**, und **alle**. Diese wirken sich auf das TreeView-Steuerelement wie folgt:

| **Eigenschaftswert** | **Auswirkung** |
| --- | --- |
| Keiner | Kontrollkästchen werden nicht auf den Knoten angezeigt. Dies ist die Standardeinstellung. |
| Stammverzeichnis | Ein Kontrollkästchen wird nur für den Stammknoten angezeigt. |
| Übergeordnetes Element | Ein Kontrollkästchen wird nur auf diesen Knoten angezeigt, die untergeordnete Knoten besitzen. Diese untergeordneten Knoten möglich übergeordneten Knoten oder Blattknoten. |
| Blattelemente | Ein Kontrollkästchen wird nur auf diesen Knoten angezeigt, die keine untergeordneten Knoten besitzen. |
| Alle | Ein Kontrollkästchen wird auf allen Knoten angezeigt. |

Wenn das Kontrollkästchen verwendet werden, die **CheckedNodes** Eigenschaft gibt eine Auflistung von TreeView-Knoten, die beim Postback überprüft werden.

Die **ShowExpandCollapse** -Eigenschaft steuert die Darstellung des Bilds erweitern/reduzieren neben Stamm- und übergeordneten Knoten. Wenn diese Eigenschaft, um festgelegt wird **"false"**, TreeView-Knoten werden als Hyperlinks gerendert und sind aufgeklappt/Zugeklappt, indem Sie auf den Link klicken.

Die **ShowLines** Eigenschaft steuert, ob Linien angezeigt werden untergeordnete Knoten mit übergeordneten Knoten. Wenn **"false"** (Standardeinstellung), werden keine Linien angezeigt. Wenn **"true"**, das TreeView-Steuerelement verwendet Bilder von Zeilen in den Ordner in der **LineImagesFolder** Eigenschaft.

Um die Darstellung des TreeView-Linien anzupassen, enthält Visual Studio .NET 2005 ein Line-Designer-Tool. Sie können dieses Tool, mit der Smarttag-Schaltfläche für das TreeView-Steuerelement als unten zugreifen.


![](data-bound-controls/_static/image1.jpg)

**Abbildung 1**


Beim Auswählen der **Liniengrafiken anpassen** Menüoption-Designers die Zeile wird gestartet werden, sodass Sie so konfigurieren Sie die Darstellung des TreeView-Zeilen.

## <a name="treeview-events"></a>TreeView-Ereignisse

Das TreeView-Steuerelement verfügt über die folgenden eindeutigen Ereignisse:

- SelectedNodeChanged-tritt auf, wenn ein Knoten ausgewählt ist, basierend auf den **SelectAction** Eigenschaft.
- TreeNodeCheckChanged tritt auf, wenn ein Knoten Checkboxs Zustand geändert wird.
- TreeNodeExpanded-Ereignis tritt auf, wenn ein Knoten erweitert ist, basierend auf den **SelectAction** Eigenschaft.
- Das TreeNodeCollapsed tritt auf, wenn ein Knoten reduziert wird.
- TreeNodeDataBound tritt auf, wenn ein Knoten Daten gebunden werden.
- TreeNodePopulate tritt auf, wenn ein Knoten aufgefüllt ist.

Die **SelectAction** Eigenschaft können Sie so konfigurieren Sie das Ereignis ausgelöst wird, wenn ein Knoten ausgewählt wird. Die Eigenschaft SelectAction bietet die folgenden Aktionen:

- TreeNodeSelectAction.Expand löst TreeNodeExpanded-Ereignis, wenn der Knoten ausgewählt ist.
- TreeNodeSelectAction.None löst kein Ereignis, wenn der Knoten ausgewählt wird.
- TreeNodeSelectAction.Select löst das SelectedNodeChanged-Ereignis, wenn der Knoten ausgewählt wird.
- TreeNodeSelectAction.SelectExpand löst das SelectedNodeChanged-Ereignis und das Ereignis TreeNodeExpanded-Ereignis, wenn der Knoten ausgewählt wird.

## <a name="controlling-appearance-with-styles"></a>Steuern der Darstellung mit Stilen

Das TreeView-Steuerelement bietet zahlreiche Eigenschaften zum Steuern der Darstellung des Steuerelements mit Stilen. Die folgenden Eigenschaften sind verfügbar.

| **Eigenschaftenname** | **Steuerelemente** |
| --- | --- |
| HoverNodeStyle | Steuert den Stil der Knoten an, wenn die Maus darauf gezeigt wird. |
| LeafNodeStyle | Steuert den Stil der Endknoten. |
| NodeStyle | Steuert das Format für alle Knoten. Bestimmte Knotenstile (z. B. LeafNodeStyle) außer Kraft setzen dieser Stil. |
| ParentNodeStyle | Steuert den Stil für alle übergeordneten Knoten. |
| RootNodeStyle | Steuert das Format für den Stammknoten. |
| SelectedNodeStyle | Steuert das Format für den ausgewählten Knoten. |

Jede dieser Eigenschaften ist schreibgeschützt. Allerdings werden sie jeder Rückgabe eine **TreeNodeStyle** Objekt und die Eigenschaften des Objekts, können mithilfe von geändert werden die *Eigenschaft-Untereigenschaft* Format. Z. B. Festlegen der **ForeColor** Eigenschaft der **SelectedNodeStyle**, verwenden Sie die folgende Syntax:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Beachten Sie, dass die oben genannten Tag nicht geschlossen ist. Dies liegt daran bei Verwendung die deklarative Syntax, die hier gezeigten würden Sie TreeViews Knoten in der HTML-Code einschließen.

Die Stileigenschaften können auch angegeben werden, im Code mithilfe der *property.subproperty* Format. Z. B. Festlegen der **ForeColor** Eigenschaft der **RootNodeStyle** in Code, verwenden Sie die folgende Syntax:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Eine umfassende Liste der Eigenschaften der anderen finden Sie unter der MSDN-Dokumentation für das TreeNodeStyle-Objekt.


## <a name="the-sitemappath-control"></a>Das Steuerelement SiteMapPath

Das Steuerelement SiteMapPath bietet ein Navigationssteuerelement für einen Teil (Breadcrumb) für Entwickler von ASP.NET-ANWENDUNGEN. Wie die anderen Steuerelemente zur Seitennavigation kann es ganz einfach sein, dass Daten an hierarchische Daten Quellen wie XmlDataSource oder SiteMapDataSource gebunden.

Ein SiteMapPath-Steuerelement besteht aus SiteMapNodeItem-Objekte. Es gibt drei Arten von Knoten aus. der Stammknoten, übergeordneten Knoten und der aktuelle Knoten. Der Stammknoten ist der Knoten am Anfang der hierarchischen Struktur. Den aktuellen Knoten darstellt, die aktuelle Seite. Alle anderen Knoten sind die übergeordneten Knoten.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Steuerung der Ausführung der SiteMapPath-Steuerelement

Die Eigenschaften, die die Ausführung des SiteMapPath-Kontrolle steuern sind wie folgt aus:

| **Property** | **Beschreibung der Eigenschaft** |
| --- | --- |
| ParentLevelsDisplayed | Steuert, wie vielen übergeordneten Knoten angezeigt werden. Der Standardwert ist-1 keine Einschränkung hinsichtlich der Anzahl der übergeordneten Knoten angezeigt erzwingt. |
| PathDirection | Steuert die Richtung des SiteMapPath. Gültige Werte sind RootToCurrent (Standard) und CurrentToRoot. |
| PathSeparator | Eine Zeichenfolge, die das Zeichen steuert, das Knoten in ein SiteMapPath-Steuerelement trennt. Der Standardwert ist:. |
| RenderCurrentNodeAsLink | Ein boolescher Wert, der steuert, ob der aktuelle Knoten als Link gerendert wird. Der Standardwert ist "false". |
| SkipLinkText | Hilfe bei der Barrierefreiheit, wenn die Seite, die von der Sprachausgabe angezeigt wird. Diese Eigenschaft ermöglicht Bildschirmsprachausgaben SiteMapPath-Kontrolle zu überspringen. Um dieses Feature deaktivieren, wird die Eigenschaft auf String.Empty festgelegt. |

## <a name="templated-sitemappath-controls"></a>Auf Vorlagen basierenden SiteMapPath-Steuerelemente

Die SiteMapControl ist ein Steuerelement mit Vorlagen, und Sie können daher verschiedene Vorlagen für die Verwendung bei der Anzeige des Steuerelements definieren. Um Vorlagen in ein SiteMapPath-Steuerelement zu bearbeiten, klicken Sie auf die Smarttag-Schaltfläche für das Steuerelement, und wählen Sie im Vorlagen bearbeiten. Dadurch werden das Menü "SiteMapTasks" angezeigt, wie unten in dem Sie zwischen den verschiedenen verfügbaren Vorlagen auswählen können.


![](data-bound-controls/_static/image2.jpg)

**Abbildung 2**


Die **NodeTemplate** Vorlage bezieht sich auf alle Knoten in der SiteMapPath. Wenn der Knoten einen Stammknoten oder der aktuelle Knoten ist und eine **RootNodeTemplate** oder **CurrentNodeTemplate** wird konfiguriert, der die NodeTemplate überschrieben wird.

## <a name="sitemappath-events"></a>SiteMapPath-Ereignisse

Das Steuerelement SiteMapPath verfügt über zwei Ereignisse, die nicht von der Control-Klasse abgeleitet werden; die **ItemCreated** Ereignis und die **ItemDataBound** Ereignis. Die ItemCreated-Ereignis wird ausgelöst, wenn ein SiteMapPath-Element erstellt wird. ItemDataBound wird ausgelöst, wenn die DataBind-Methode, während der Datenbindung eines SiteMapPath-Knotens aufgerufen wird. Ein **SiteMapNodeItemEventArgs** Objekt bietet Zugriff auf den bestimmten SiteMapNodeItem über die Item-Eigenschaft.

## <a name="controlling-appearance-with-styles"></a>Steuern der Darstellung mit Stilen

Die folgenden Stile sind für die Formatierung für ein SiteMapPath-Steuerelement verfügbar.

| **Eigenschaftenname** | **Steuerelemente** |
| --- | --- |
| CurrentNodeStyle | Steuert den Stil des Texts für den aktuellen Knoten. |
| RootNodeStyle | Steuert den Stil des Texts für den Stammknoten. |
| NodeStyle | Steuert den Stil des Texts für alle Knoten, vorausgesetzt, dass eine CurrentNodeStyle oder RootNodeStyle nicht zutrifft. |

Die NodeStyle-Eigenschaft wird durch die CurrentNodeStyle oder die RootNodeStyle überschrieben. Jede dieser Eigenschaften ist schreibgeschützt und gibt eine **Stil** Objekt. Um die Darstellung eines Knotens mit einer dieser Eigenschaften beeinflussen möchten, müssen Sie die Eigenschaften des Stilobjekts festgelegt, der zurückgegeben wird. Der folgende Code ändert beispielsweise die Forecolor-Eigenschaft des aktuellen Knotens.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Die Eigenschaft kann auch programmgesteuert wie folgt angewendet werden:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Wenn eine Vorlage angewendet wird, wird das Format nicht angewendet werden.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Übung 1: Konfigurieren einer ASP.NET Menu-Steuerelement

1. Erstellen Sie eine neue Website.
2. Fügen Sie eine Site Map-Datei durch Auswahl Datei "," Neu ","-Datei und Sitemap aus der Liste der Vorlagen aus.
3. Öffnen Sie die Sitemap (Web.sitemap standardmäßig), und ändern Sie diese, damit sie die unten aufgeführte aussieht. Die Seiten, die Sie in der Sitezuordnungsdatei verknüpfen, nicht wirklich vorhanden sind, aber diese kein Problem für diese Übung.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Öffnen Sie das Standard-Web-Form, in der Entwurfsansicht.
5. Fügen Sie im Navigationsbereich der Toolbox ein neues Menu-Steuerelement auf der Seite hinzu.
6. Fügen Sie aus dem Datenbereich der Toolbox einer neuen SiteMapDataSource verbunden ist. Die SiteMapDataSource werden automatisch die Web.sitemap-Datei auf Ihrer Website verwendet. (Der Web.sitemap-Datei *müssen* im Stammordner der Website sein.)
7. Klicken Sie auf das Steuerelement, und klicken Sie dann auf die Smarttag-Schaltfläche, um die Anzeige des Dialogfelds im Menüaufgaben.
8. Wählen Sie in der Dropdownliste "Datenquelle auswählen" SiteMapDataSource1 ein.
9. Klicken Sie auf die AutoFormat-Link, und wählen Sie ein Format für das Menü.
10. Legen Sie im Bereich Eigenschaften den **StaticDisplayLevels** Eigenschaft auf 2. Das Steuerelement sollte jetzt Knotens Home, Produkte und Dienste im Designer angezeigt werden.
11. Durchsuchen Sie die Seite in Ihrem Browser, um das Menü verwenden. (Da die Seiten, die Sie speichern die Sitemap hinzugefügt gar nicht existieren, Fehler sehen Sie, wenn Sie versuchen, und navigieren Sie zu ihnen.)

Experimentieren Sie mit der StaticDisplayLevels und die MaximumDynamicDisplayLevels Eigenschaften ändern, und sehen Sie, wie sie Auswirkungen auf wie das Menü gerendert wird.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Übung 2: Dynamisches Binden einer TreeView-Steuerelement

In dieser Übung wird davon ausgegangen, dass Sie SQL Server, die lokal ausgeführt haben und die Northwind-Datenbank auf SQL Server-Instanz vorhanden ist. Wenn diese Bedingungen nicht erfüllt sind, ändern Sie die Verbindungszeichenfolge in das Beispiel aus. Beachten Sie, dass Sie möglicherweise auch SQL Server-Authentifizierung anstelle einer vertrauten Verbindung angeben müssen.

1. Erstellen Sie eine neue Website.
2. Wechseln Sie zur Codeansicht für "default.aspx" ein, und Ersetzen Sie sämtlichen Code durch den unten aufgeführten Code. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Speichern Sie die Seite als treeview.aspx.
4. Durchsuchen Sie die Seite.
5. Wenn die Seite zuerst angezeigt wird, zeigen Sie den Quellcode der Seite in Ihrem Browser. Beachten Sie, dass nur die sichtbaren Knoten, die an den Client gesendet wurden.
6. Klicken Sie auf das Pluszeichen neben einem Knoten.
7. Die Quelle wird auf der Seite erneut. Beachten Sie, dass die neu angezeigten Knoten jetzt vorhanden sind.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Übung 3: Details anzeigen und Bearbeiten von Daten mit einem GridView und DetailsView

1. Erstellen Sie eine neue Website.
2. Fügen Sie eine neue Datei "Web.config" der Website hinzu.
3. Fügen Sie der Datei "Web.config" wie unten dargestellt eine Verbindungszeichenfolge hinzu: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Möglicherweise müssen Sie die Verbindungszeichenfolge, die basierend auf Ihrer Umgebung zu ändern.
4. Speichern und schließen Sie die Datei "web.config".
5. Öffnen Sie "default.aspx", und fügen Sie ein neues SqlDataSource-Steuerelement hinzu.
6. Ändern Sie die ID des Steuerelements SqlDataSource **Produkte**.
7. In der **SqlDataSource-Aufgaben** Menü klicken Sie auf **Konfigurieren von Datenquellen**.
8. Wählen Sie **Northwind** in der Dropdownliste für die Verbindung, und klicken Sie auf Weiter.
9. Wählen Sie **Produkte** aus der **Namen** Dropdownliste, und überprüfen Sie die **"ProductID"**, **ProductName**, **UnitPrice**, und **UnitsInStock** Kontrollkästchen wie unten dargestellt. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Klicken Sie auf **Weiter**.
11. Klicken Sie auf **Fertig stellen**.
12. Wechseln Sie zur Quellansicht, und überprüfen Sie den Code, der generiert wurde. Beachten Sie, dass die **SelectCommand**, **DeleteCommand**, **InsertCommand**, und **UpdateCommand** , wurden hinzugefügt, um dem SqlDataSource-Steuerelement -Steuerelement. Beachten Sie auch die Parameter, die hinzugefügt wurden.
13. Wechseln Sie zur Entwurfsansicht, und fügen Sie ein neues GridView-Steuerelement auf der Seite hinzu.
14. Wählen Sie **Produkte** aus der **Datenquelle auswählen** Dropdownliste.
15. Überprüfen Sie **Paging aktivieren** und **Auswahl aktivieren** wie unten dargestellt. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Klicken Sie auf die **Spalten bearbeiten** verknüpfen, und stellen Sie sicher, dass **Felder automatisch generieren** aktiviert ist.
17. Klicken Sie auf **OK**.
18. Mit dem GridView-Steuerelement ausgewählt haben, klicken Sie auf die Schaltfläche neben dem **DataKeyNames** Eigenschaft im Bereich Eigenschaften.
19. Wählen Sie **"ProductID"** aus der **verfügbaren Felder** aus, und klicken Sie auf die **&gt;** , um es hinzuzufügen.
20. Klicken Sie auf OK.
21. Fügen Sie ein neues SqlDataSource-Steuerelement auf der Seite hinzu.
22. Ändern Sie die ID des Steuerelements SqlDataSource **Details**.
23. Wählen Sie im Aufgabenmenü SqlDataSource-Steuerelement, **Konfigurieren von Datenquellen**.
24. Wählen Sie **Northwind** in der Dropdownliste auf **Weiter**.
25. Wählen Sie <strong>Produkte</strong> aus der <strong>Namen</strong> Dropdownliste, und überprüfen Sie die <strong> \</ strong > * Kontrollkästchen in der <strong>Spalten</strong> ListBox-Element.
26. Klicken Sie auf die **, in denen** Schaltfläche.
27. Wählen Sie **"ProductID"** aus der **Spalte** Dropdownliste.
28. Wählen Sie **=** in der Dropdownliste "Operator".
29. Wählen Sie **Steuerelement** aus der **Quelle** Dropdownliste.
30. Wählen Sie **GridView1** aus der **Steuerelement-ID** Dropdownliste.
31. Klicken Sie auf die **hinzufügen** , um der WHERE-Klausel hinzuzufügen.
32. Klicken Sie auf **OK**.
33. Klicken Sie auf die **erweitert** Schaltfläche, und überprüfen Sie die **generieren INSERT, UPDATE und DELETE-Anweisungen** Kontrollkästchen.
34. Klicken Sie auf **OK**.
35. Klicken Sie auf **Weiter** , und klicken Sie auf **Fertig stellen**.
36. Fügen Sie einem DetailsView-Steuerelement auf der Seite hinzu.
37. In der **Datenquelle auswählen** Dropdownliste wählen **Details**.
38. Überprüfen Sie die **Bearbeiten aktivieren** Kontrollkästchen wie unten dargestellt. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Speichern Sie die Seite, und navigieren Sie "default.aspx".
40. Klicken Sie auf die **wählen** neben unterschiedliche Datensätze, die das DetailsView-Update automatisch angezeigt.
41. Klicken Sie auf die **bearbeiten** Link im DetailsView-Steuerelement.
42. Nehmen Sie eine Änderung auf den Eintrag, und klicken Sie auf **Update**.
