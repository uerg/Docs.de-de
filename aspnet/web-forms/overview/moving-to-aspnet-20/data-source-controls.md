---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: -Datenquellensteuerelemente | Microsoft Docs
author: microsoft
description: DataGrid-Steuerelement in ASP.NET 1.x eine hervorragende Verbesserung in Datenzugriff in Webanwendungen markiert. Allerdings war es so benutzerfreundlich wie bisher konnten nicht...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: b1ac7fb62767d61c97fe00338bc0f5087f4863b5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885894"
---
<a name="data-source-controls"></a>Datenquellen-Steuerelementen
====================
durch [Microsoft](https://github.com/microsoft)

> DataGrid-Steuerelement in ASP.NET 1.x eine hervorragende Verbesserung in Datenzugriff in Webanwendungen markiert. Allerdings war es nicht als benutzerfreundliche, wie sie hätten verwendet werden kann. Eine beträchtliche Menge an Code viele nützliche Funktionen daraus abrufen weiterhin erforderlich. Dies ist das Modell in allen Data Access-Aufgaben im 1.x.


DataGrid-Steuerelement in ASP.NET 1.x eine hervorragende Verbesserung in Datenzugriff in Webanwendungen markiert. Allerdings war es nicht als benutzerfreundliche, wie sie hätten verwendet werden kann. Eine beträchtliche Menge an Code viele nützliche Funktionen daraus abrufen weiterhin erforderlich. Dies ist das Modell in allen Data Access-Aufgaben im 1.x.

ASP.NET 2.0 Adressen mit teilweise mit Datenquellensteuerelemente. Die Datenquellen-Steuerelementen in ASP.NET 2.0 bieten Entwicklern ein deklaratives Modell für das Abrufen von Daten, Anzeigen von Daten und Bearbeiten von Daten. Datenquellen-Steuerelementen wird eine konsistente Darstellung von Daten für datengebundene Steuerelemente unabhängig von der Quelle dieser Daten angegeben werden. Das Herzstück der Datenquellen-Steuerelementen in ASP.NET 2.0 ist Sie die DataSourceControl abstrakte Klasse. Die Klasse DataSourceControl bietet eine grundlegende Implementierung der Schnittstelle "IDataSource" sein und der IListSource-Schnittstelle, die den Datenquellen-Steuerelements als Datenquelle eines datengebundenen Steuerelements (über die neue DataSourceId-Eigenschaft zuweisen Sie können weiter unten erläutert) und die Daten dort als Liste verfügbar machen. Jede Liste der Daten aus einem Datenquellensteuerelement wird als ein DataSourceView-Objekt verfügbar gemacht. Zugriff auf das DataSourceView-Instanzen wird durch die Schnittstelle "IDataSource" sein bereitgestellt. Beispielsweise erfolgt die Methodenrückgabe GetViewNames ein, die Ihnen ermöglicht, die DataSourceViews auflisten ICollection zugeordnet einer bestimmten Datenquellen-Steuerelements und der GetView-Methode können Sie eine bestimmte DataSourceView-Instanz anhand Ihres Namens zugreifen.

Datenquellen-Steuerelemente haben keine Benutzeroberfläche. Sie implementiert werden, da Server gesteuert wird, sodass deklarative Syntax zu unterstützen und so, dass sie Zugriff auf der Seitenstatus haben, falls gewünscht. Datenquellen-Steuerelementen Rendern HTML-Markup nicht an dem Client.

> [!NOTE]
> Wie Sie sehen werden, Zwischenspeichern gibt es auch Vorteile, die mithilfe von Steuerelementen für Datenquellen abgerufen.


## <a name="storing-connection-strings"></a>Das Speichern von Verbindungszeichenfolgen

Bevor wir zu betrachten zum Konfigurieren von Datenquellen-Steuerelementen, sollte eine neue Funktion in ASP.NET 2.0 über Verbindungszeichenfolgen eingegangen. ASP.NET 2.0 stellt einen neuen Abschnitt in der Konfigurationsdatei an, mit dem Sie leicht Speichern von Verbindungszeichenfolgen, die dynamisch zur Laufzeit gelesen werden kann. Die &lt;ConnectionStrings&gt; Abschnitt erleichtert das Speichern von Verbindungszeichenfolgen.

Der Codeausschnitt unten wird eine neue Verbindungszeichenfolge hinzugefügt.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Wie bei der &lt;"appSettings"&gt; Abschnitt der &lt;ConnectionStrings&gt; Abschnitt angezeigt wird, außerhalb von der &lt;system.web&gt; Abschnitt in der Konfigurationsdatei.


Um diese Verbindungszeichenfolge zu verwenden, können Sie die folgende Syntax verwenden, wenn Sie das ConnectionString-Attribut eines Steuerelements festlegen.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Die &lt;ConnectionStrings&gt; Abschnitt kann auch verschlüsselt werden, sodass vertraulicher Informationen nicht verfügbar gemacht wird. Diese Funktion wird in einem Modul weiter unten erläutert.

## <a name="caching-data-sources"></a>Zwischenspeichern von Datenquellen

Jede DataSourceControl stellt vier Eigenschaften zum Konfigurieren von caching; EnableCaching, CacheDuration CacheExpirationPolicy und CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching ist, dass eine boolesche Eigenschaft, die bestimmt, und zwar unabhängig davon, ob das Zwischenspeichern für das Datenquellensteuerelement aktiviert ist.

## <a name="cacheduration-property"></a>CacheDuration-Eigenschaft

Die CacheDuration-Eigenschaft legt die Anzahl der Sekunden an, denen der Cache gültig bleibt. Wenn diese Eigenschaft auf **0** bewirkt, dass der Cache bleibt gültig, bis Sie explizit für ungültig erklärt.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy-Eigenschaft

Die CacheExpirationPolicy-Eigenschaft kann festgelegt werden entweder **Absolute** oder **Sliding**. Festlegen auf Absolute bedeutet, der die maximale Zeitdauer, die die Daten zwischengespeichert werden, werden die Anzahl der Sekunden, die von der CacheDuration-Eigenschaft angegeben ist. Durch Festlegen auf gleitend, wird die Ablaufzeit zurückgesetzt, wenn jeder Vorgang ausgeführt wird.

## <a name="cachekeydependency-property"></a>CacheKeyDependency-Eigenschaft

Wenn ein Zeichenfolgenwert für die CacheKeyDependency-Eigenschaft angegeben wird, wird eine neue Cacheabhängigkeit auf der Grundlage dieser Zeichenfolge ASP.NET einrichten. Dadurch können Sie explizit den Cache ungültig gemacht werden, indem Sie einfach ändern oder Entfernen der CacheKeyDependency.

**Wichtige**: Wenn Identitätswechsel aktiviert ist und der Zugriff auf die Datenquelle und/oder den Inhalt der Daten basieren auf der Identität des Clients, es wird empfohlen, caching deaktiviert werden, indem EnableCaching auf "false" festgelegt. Wenn caching in diesem Szenario aktiviert ist, und ein anderen Benutzer als der Benutzer, der die Daten ursprünglich angefordert hat, eine Anforderung gibt, wird die Autorisierung für die Datenquelle nicht erzwungen. Die Daten werden einfach aus dem Cache bedient werden.

## <a name="the-sqldatasource-control"></a>SqlDataSource-Steuerelement

SqlDataSource-Steuerelement kann ein Entwickler in einer relationalen Datenbank, die ADO.NET unterstützt gespeicherten Daten zuzugreifen. Sie können den System.Data.SqlClient-Anbieter verwenden, den Zugriff auf eine SQL Server-Datenbank, den Anbieter "System.Data.OleDb", "System.Data.ODBC" Anbieter oder der System.Data.OracleClient-Anbieter von Oracle zugegriffen. Aus diesem Grund ist die SqlDataSource sicherlich nicht nur verwendet für den Zugriff auf Daten in einer SQL Server-Datenbank.

Damit die SqlDataSource verwenden zu können, und indem Sie einfach Geben Sie einen Wert für die ConnectionString-Eigenschaft, geben Sie einen SQL-Befehl oder gespeicherte Prozedur. Arbeiten mit den zugrunde liegenden ADO.NET-Architektur übernimmt die SqlDataSource-Steuerelement. Es wird die Verbindung geöffnet, fragt die Datenquelle oder führt die gespeicherte Prozedur gibt die Daten zurück und schließt dann die Verbindung.

> [!NOTE]
> Da die Klasse DataSourceControl automatisch die Verbindung geschlossen wurde, sollte dieser die Anzahl der Kunden Aufrufe von Verlusten von Verbindungen mit der Datenbank generierten beeinträchtigen.


Der Codeausschnitt unten bindet ein DropDownList-Steuerelements an ein SqlDataSource-Steuerelement mithilfe der Verbindungszeichenfolge, die in der Konfigurationsdatei wie oben gezeigt gespeichert sind.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Wie oben gezeigt wurde, gibt der Datenreader, der die SqlDataSource den Modus für die Datenquelle an. Im obigen Beispiel wird die DataSourceMode auf DataReader festgelegt. In diesem Fall gibt die SqlDataSource ein IDataReader-Objekt, das mit einem Vorwärtscursor und schreibgeschützte Cursor zurück. Der angegebene Typ des Objekts, das zurückgegeben wird, wird vom Anbieter gesteuert, das verwendet wird. In diesem Fall bei der Verwendung des System.Data.SqlClient-Anbieters gemäß den &lt;ConnectionStrings&gt; Abschnitt der Datei "Web.config". Daher wird das zurückgegebene Objekt vom Typ SqlDataReader sein. Durch Angabe des Werts DataSourceMode DataSet, können die Daten in einem DataSet auf dem Server gespeichert werden. In diesem Modus können Sie Funktionen wie das Sortieren, Paging usw. hinzufügen. Wenn ich die Datenbindung hätte die SqlDataSource an ein GridView-Steuerelement, ich würde haben den DataSet-Modus. Im Falle einer DropDownList ist das DataReader-Modus jedoch die richtige Wahl.

> [!NOTE]
> Beim Zwischenspeichern von einem SqlDataSource oder einem AccessDataSource muss der Datenreader DataSet festgelegt werden. Wenn Sie mit einem DataSourceMode DataReader Zwischenspeichern aktivieren, wird eine Ausnahme ausgelöst.


## <a name="sqldatasource-properties"></a>SqlDataSource-Eigenschaften

Im folgenden sind einige der Eigenschaften des SqlDataSource-Steuerelement.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Ein boolescher Wert, der angibt, ob ein select-Befehl abgebrochen wird, wenn einer der Parameter null ist. "True" in der Standardeinstellung werden.

### <a name="conflictdetection"></a>ConflictDetection

In einer Situation, in denen mehrere Benutzer eine Datenquelle gleichzeitig aktualisiert werden können, bestimmt die ConflictDetection-Eigenschaft das Verhalten der SqlDataSource-Steuerelement. Diese Eigenschaft wird zu einer der Werte der Enumeration ConflictOptions ausgewertet. Diese Werte sind **CompareAllValues** und **OverwriteChanges**. Wenn auf OverwriteChanges, den letzten Bearbeiter zum Schreiben von Daten in der Datenquelle festgelegt werden alle vorherigen Änderungen überschrieben. Wenn die ConflictDetection-Eigenschaft auf CompareAllValues festgelegt ist, Parameter für die von SelectCommand zurückgegebenen Spalten erstellt und Parameter werden auch erstellt, um die ursprünglichen Werte in jeder dieser Spalten ermöglicht die SqlDataSource zu halten Bestimmen Sie, und zwar unabhängig davon, ob die Werte geändert haben, da die SelectCommand ausgeführt wurde.

### <a name="deletecommand"></a>DeleteCommand

Legt fest oder ruft die SQL-Zeichenfolge, die beim Löschen von Zeilen aus der Datenbank verwendet. Dies kann entweder eine SQL-Abfrage oder den Namen einer gespeicherten Prozedur sein.

### <a name="deletecommandtype"></a>DeleteCommandType

Legt fest oder ruft dem Typ der Delete-Befehl, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Gibt die Parameter, die durch die DeleteCommand des Objekts SqlDataSourceView SqlDataSource-Steuerelement zugeordneten verwendet werden.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Diese Eigenschaft wird verwendet, um das Format der Parameter mit den ursprünglichen Werten in Fällen anzugeben, die ConflictDetection-Eigenschaft auf CompareAllValues festgelegt ist. Der Standardwert ist {0}, was bedeutet, dass Parameter der ursprüngliche Wert den gleichen Namen wie die ursprünglichen Parameter wirksam werden. Das heißt, ist der Name des Felds "EmployeeID", der ursprünglichen Wertparameter wäre @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Legt fest oder ruft die SQL-Zeichenfolge, die zum Abrufen von Daten aus der Datenbank verwendet wird. Dies kann entweder eine SQL-Abfrage oder den Namen einer gespeicherten Prozedur sein.

### <a name="selectcommandtype"></a>SelectCommandType

Legt fest oder ruft dem Typ der select-Befehl, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Gibt die Parameter, die von SelectCommand des SqlDataSource-Steuerelement zugeordneten SqlDataSourceView-Objekts verwendet werden.

### <a name="sortparametername"></a>SortParameterName

Ruft ab oder legt den Namen der Parameter einer gespeicherten Prozedur, der beim Sortieren von Daten abrufen von Datenquellen-Steuerelements verwendet wird. Nur gültig, wenn SelectCommandType auf StoredProcedure festgelegt ist.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Eine durch Semikolons getrennte Zeichenfolge, die die Datenbanken und Tabellen in einer SQL Server-Cacheabhängigkeit verwendeten angibt. (SQL-Cache-Abhängigkeiten werden in einem Modul weiter unten erläutert.)

### <a name="updatecommand"></a>UpdateCommand

Legt fest oder ruft die SQL-Zeichenfolge, die beim Aktualisieren von Daten in der Datenbank verwendet wird. Dies kann entweder eine SQL-Abfrage oder den Namen einer gespeicherten Prozedur sein.

### <a name="updatecommandtype"></a>UpdateCommandType

Legt fest oder ruft dem Typ des Update-Befehls, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Gibt die Parameter, die durch die UpdateCommand des Objekts SqlDataSourceView SqlDataSource-Steuerelement zugeordneten verwendet werden.

## <a name="the-accessdatasource-control"></a>AccessDataSource-Steuerelement

AccessDataSource-Steuerelement von der SqlDataSource-Klasse abgeleitet ist und wird verwendet, um Daten in einer Microsoft Access-Datenbank gebunden werden soll. Die ConnectionString-Eigenschaft für das AccessDataSource-Steuerelement ist eine schreibgeschützte Eigenschaft. Anstatt die ConnectionString-Eigenschaft zu verwenden, wird die DataFile-Eigenschaft verwendet, um auf die Access-Datenbank zu zeigen, wie unten dargestellt.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Die AccessDataSource der Anbietername für die Basis SqlDataSource wird immer auf "System.Data.OleDb" festgelegt und mit der Datenbank, die mit dem Microsoft.Jet.OLEDB.4.0 OLE DB-Anbieter hergestellt. AccessDataSource-Steuerelement können keine Verbindung mit einer Access-Datenbank ein Kennwort geschützt. Wenn Sie für die Verbindung mit einer Datenbank ein Kennwort geschützt haben, sollten Sie die SqlDataSource-Steuerelement verwenden.

> [!NOTE]
> Access-Datenbanken, die innerhalb der Website gespeicherten sollte in der App befinden,\_Datenverzeichnis. ASP.NET lässt keine Dateien in diesem Verzeichnis, das durchsucht werden. Sie müssen das Prozesskonto Lese- und Schreibzugriff auf die App gewähren\_Datenverzeichnis bei Verwendung von Access-Datenbanken.


## <a name="the-xmldatasource-control"></a>XmlDataSource-Steuerelement

Die XmlDataSource wird verwendet, um Daten-XML-Daten an Daten gebundene Steuerelemente binden. Sie können eine XML-Datei mithilfe der DataFile-Eigenschaft binden oder Sie können eine XML-Zeichenfolge, die mithilfe der Data-Eigenschaft binden. Die XmlDataSource stellt XML-Attribute als bindbare Felder. In Fällen, in denen Sie Werte binden, die nicht als Attribute dargestellt werden müssen, müssen Sie eine XSL-Transformation verwenden. Sie können auch die XPath-Ausdrücke zum Filtern von XML-Daten.

Betrachten Sie das folgende XML-Datei ein:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Beachten Sie, dass die XmlDataSource eine XPath-Eigenschaft wird verwendet, *Menschen/* um Filtern nur die &lt;Person&gt; Knoten. Der DropDownList Data-bindet die klicken Sie dann auf das LastName-Attribut, das mithilfe der DataTextField-Eigenschaft.

Während das XmlDataSource-Steuerelement in erster Linie zum Data-Bind auf nur-Lese XML-Daten verwendet wird, ist es möglich, die XML-Datendatei zu bearbeiten. Beachten Sie, dass in einem solchen Fall Automatisches Einfügen, aktualisieren und Löschen von Informationen in der XML-Datei erfolgt nicht automatisch wie bei anderen Steuerelementen für Datenquellen. Stattdessen müssen Sie Code schreiben, um die Daten mithilfe der folgenden Methoden des Steuerelements XmlDataSource manuell zu bearbeiten.

### <a name="getxmldocument"></a>GetXmlDocument

Ruft ein XmlDocument-Objekt, das mit dem XML-Code durch die XmlDataSource abgerufen.

### <a name="save"></a>Speichern

Speichert das XML-Dokument im Arbeitsspeicher zurück an die Datenquelle an.

Es ist wichtig zu beachten, dass die Save-Methode nur funktioniert, wenn die beiden folgenden Bedingungen erfüllt sind:

1. Die XmlDataSource wird mithilfe von DataFile-Eigenschaft um in einer XML-Datei anstelle der Daten-Eigenschaft zu binden, zum Binden an die XML-Daten im Arbeitsspeicher.
2. Es ist keine Transformation über die Transformations- oder TransformFile-Eigenschaft angegeben.

Beachten Sie außerdem, dass die Save-Methode unerwartete Ergebnisse, wenn von mehreren Benutzern gleichzeitig aufgerufen führen kann.

## <a name="the-objectdatasource-control"></a>Das ObjectDataSource-Steuerelement

Die Datenquellen-Steuerelementen, die es bis zu diesem Zeitpunkt abgedeckt haben sind eine ausgezeichnete Wahl für zwei Ebenen-Anwendungen, in dem Datenquellen-Steuerelements direkt mit dem Datenspeicher kommuniziert. Viele echten Anwendungen sind jedoch Anwendungen mit mehreren Ebenen, in denen möglicherweise ein Datenquellen-Steuerelement ein Geschäftsobjekt mitteilen müssen, die wiederum mit der Datenschicht kommuniziert. In diesen Situationen füllt das ObjectDataSource ordentlich die Rechnung. Das ObjectDataSource funktioniert in Verbindung mit einem Quellobjekt. Das ObjectDataSource-Steuerelement wird eine Instanz der Quellobjekt, rufen Sie die angegebene Methode und Dispose der Objektinstanz innerhalb des Bereichs einer einzelnen Anforderung erstellt, wenn Ihr Objekt Instanzmethoden anstelle von statischen Methoden (Shared in Visual Basic) aufweist. Deshalb muss Ihr Objekt zustandslos sein. Also sollte alle erforderliche Ressourcen innerhalb einer einzelnen Anforderung das Objekt abrufen und freigeben. Sie können steuern, wie das Quellobjekt, das durch die Behandlung des Ereignisses ObjectCreating, der das ObjectDataSource-Steuerelement erstellt wird. Sie können erstellen Sie eine Instanz des Quellobjekts, und legen Sie dann die ObjectInstance-Eigenschaft der Klasse ObjectDataSourceEventArgs an diese Instanz. Das ObjectDataSource-Steuerelement verwenden die Instanz, die im Ereignis ObjectCreating statt einer Instanz auf einem eigenen erstellt wird.

Wenn das Quellobjekt für ein ObjectDataSource-Steuerelement öffentliche statische Methoden (Shared in Visual Basic) verfügbar, die zum Abrufen und Ändern von Daten aufgerufen werden kann macht, wird ein ObjectDataSource-Steuerelement diese Methoden direkt aufrufen. Wenn einem ObjectDataSource-Steuerelement eine Instanz des Quellobjekts erstellen muss, um Methodenaufrufe vornehmen zu können, muss das Objekt einen öffentlichen Konstruktor enthalten, der keine Parameter akzeptiert. Das ObjectDataSource-Steuerelement wird diesen Konstruktor aufrufen, wenn es sich um eine neue Instanz des Quellobjekts erstellt.

Wenn das Quellobjekt, das nicht über einen öffentlichen Konstruktor ohne Parameter enthält, können Sie eine Instanz des Quellobjekts erstellen, die durch das ObjectDataSource-Steuerelement im Ereignis ObjectCreating verwendet werden.

## <a name="specifying-object-methods"></a>Angeben von Objektmethoden

Das Quellobjekt für ein ObjectDataSource-Steuerelement kann enthalten eine beliebige Anzahl von Methoden, mit denen auswählen, einfügen, aktualisieren oder Löschen von Daten. Diese Methoden werden durch das ObjectDataSource-Steuerelement, das basierend auf den Namen der Methode aufgerufen, sobald identifiziert, indem entweder der SelectMethod InsertMethod, UpdateMethod bzw. DeleteMethod-Eigenschaft von ObjectDataSource-Steuerelement. Das Quellobjekt kann auch eine optionale SelectCount Methode enthalten, die identifiziert wird, indem das ObjectDataSource-Steuerelement mithilfe der SelectCountMethod-Eigenschaft, die die Gesamtanzahl von Objekten in der Datenquelle zurückgibt. Das ObjectDataSource-Steuerelement wird die SelectCount-Methode aufrufen, nachdem eine Select-Methode zum Abrufen der Gesamtzahl der Datensätze in der Datenquelle für die Verwendung von beim paging aufgerufen wurde.

## <a name="lab-using-data-source-controls"></a>Lab mit Steuerelementen für Datenquellen

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Übung 1: Anzeigen von Daten mit SqlDataSource-Steuerelement

In der folgenden Übung werden SqlDataSource-Steuerelement für die Verbindung mit der Datenbank Northwind verwendet. Es wird davon ausgegangen, dass Sie Zugriff auf die Northwind-Datenbank auf einer SQL Server 2000-Instanz verfügen.

1. Erstellen Sie eine neue ASP.NET-Website.
2. Fügen Sie eine neue Datei "Web.config"-Datei hinzu.

    1. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf Neues Element hinzufügen.
    2. Wählen Sie die Webkonfigurationsdatei aus der Liste der Vorlagen aus, und klicken Sie auf Hinzufügen.
3. Bearbeiten der &lt;ConnectionStrings&gt; wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Wechseln Sie zur Codeansicht und ein ConnectionString-Attribut und einem SelectCommand-Attribut zum Hinzufügen der &lt;Asp: SqlDataSource&gt; steuern, wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Fügen Sie ein neues GridView-Steuerelement hinzu, von der Entwurfsansicht.
6. Wählen Sie in der Dropdownliste Datenquelle wählen Sie im Menü GridView-Aufgaben SqlDataSource1 aus.
7. Mit der rechten Maustaste auf "default.aspx" aus, und wählen Sie im Menü in Browser anzeigen. Klicken Sie auf Ja, bei der Aufforderung zum Speichern.
8. Die GridView zeigt die Daten aus der Tabelle Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Übung 2: Bearbeiten von Daten mit SqlDataSource-Steuerelement

In der folgenden Übung veranschaulicht, wie Daten binden eine DropDownList steuern, verwenden deklarative Syntax und können Sie in der DropDownList-Steuerelement angezeigten Daten zu bearbeiten.

1. Löschen Sie in der Entwurfsansicht des GridView-Steuerelements von "default.aspx" ein. 

    **Wichtige**: SqlDataSource-Steuerelement auf der Seite zu verlassen.
2. Fügen Sie einer DropDownList-Steuerelement "default.aspx" hinzu.
3. Wechseln Sie zur Quellansicht.
4. Hinzufügen ein Attributs DataSourceId DataTextField und DataValueField zur Verwendung der &lt;Asp: DropDownList&gt; steuern, wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Speichern von "default.aspx", und ihn im Browser anzeigen. Beachten Sie, dass der DropDownList alle Produkte aus der Northwind-Datenbank enthält.
6. Schließen Sie den Browser.
7. Fügen Sie ein neues Textfeldsteuerelement unterhalb des DropDownList-Steuerelements in der Quellansicht von "default.aspx" hinzu. Ändern Sie die ID-Eigenschaft des Textfelds in TxtProductName.
8. Fügen Sie ein neues Schaltflächen-Steuerelement hinzu, unter dem TextBox-Steuerelement. Ändern Sie die ID-Eigenschaft der Schaltfläche BtnUpdate und die Texteigenschaft auf **Update Product Name**.
9. Fügen Sie das UpdateCommand-Eigenschaft und zwei neue UpdateParameters in der Quellansicht von "default.aspx" wie folgt in den SqlDataSource-Tag an: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Beachten Sie, dass zwei Parameter ("ProductName" und "ProductID") hinzugefügt, die in diesem Code aktualisieren. Diese Parameter werden die Texteigenschaft des TxtProductName-Textfeld und die Eigenschaft "SelectedValue" die DdlProducts DropDownList zugeordnet.
10. Wechseln Sie zur Entwurfsansicht, und doppelklicken Sie auf das Schaltflächen-Steuerelement einen Ereignishandler hinzufügen.
11. Fügen Sie den folgenden Code, um die BtnUpdate\_klicken Sie auf Code: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Mit der rechten Maustaste auf "default.aspx", und wählen sie im Browser anzuzeigen. Klicken Sie auf Ja, wenn Sie aufgefordert werden, um alle Änderungen zu speichern.
13. ASP.NET 2.0 partielle Klassen für die Kompilierung zur Laufzeit zu ermöglichen. Es ist nicht erforderlich, zum Erstellen einer Anwendung, um wirksam codeänderungen zu finden.
14. Wählen Sie ein Produkt aus der Dropdownliste aus.
15. Geben Sie einen neuen Namen für das ausgewählte Produkt in das Textfeld ein, und klicken Sie dann auf die Schaltfläche "Aktualisieren".
16. Der Produktname wird in der Datenbank aktualisiert.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Übung 3 über das ObjectDataSource-Steuerelement

In dieser Übung wird das ObjectDataSource-Steuerelement und ein Datenquellenobjekt verwenden, um die Interaktion mit der Northwind-Datenbank veranschaulicht.

1. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf Neues Element hinzufügen.
2. Wählen Sie in der Liste der Vorlagen Web Form. Ändern Sie den Namen in object.aspx, und klicken Sie auf Hinzufügen.
3. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf Neues Element hinzufügen.
4. Wählen Sie in der Liste der Vorlagen Klasse. Ändern Sie den Namen der Klasse in NorthwindData.cs, und klicken Sie auf Hinzufügen.
5. Klicken Sie auf "Ja", wenn Sie aufgefordert werden, die Klasse der App hinzufügen\_Codeordner.
6. Fügen Sie den folgenden Code in die Datei NorthwindData.cs hinzu: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Fügen Sie den folgenden Code zur Quellansicht des object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Speichern Sie alle Dateien und Durchsuchen Sie object.aspx.
9. Interagieren Sie mit der Schnittstelle, indem Sie Details anzeigen, Bearbeiten von Mitarbeitern, Mitarbeiter hinzufügen und Löschen von Mitarbeitern.
