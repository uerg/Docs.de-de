---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Datenquellen-Steuerelemente | Microsoft-Dokumentation
author: microsoft
description: Das DataGrid-Steuerelement in ASP.NET 1.x eine große Verbesserung beim Datenzugriff in Webanwendungen gekennzeichnet. Allerdings war es so benutzerfreundlich wie konnte nicht...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: ba00024e93beba6eab226dd0d381d8734061e095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823938"
---
<a name="data-source-controls"></a>Datenquellen-Steuerelemente
====================
durch [Microsoft](https://github.com/microsoft)

> Das DataGrid-Steuerelement in ASP.NET 1.x eine große Verbesserung beim Datenzugriff in Webanwendungen gekennzeichnet. Allerdings war es nicht so benutzerfreundlich wie es hätten verwendet werden kann. Es immer noch erforderlich, eine erhebliche Menge an Code, die viele nützliche Funktionen daraus zu gewinnen. Dies ist das Modell in der alle Data Access-Aufgaben in 1.x.


Das DataGrid-Steuerelement in ASP.NET 1.x eine große Verbesserung beim Datenzugriff in Webanwendungen gekennzeichnet. Allerdings war es nicht so benutzerfreundlich wie es hätten verwendet werden kann. Es immer noch erforderlich, eine erhebliche Menge an Code, die viele nützliche Funktionen daraus zu gewinnen. Dies ist das Modell in der alle Data Access-Aufgaben in 1.x.

ASP.NET 2.0 Adressen mit teilweise mit dem Datenquellen-Steuerelemente. Die Datenquellen-Steuerelementen in ASP.NET 2.0 bieten Entwicklern ein deklaratives Modell für das Abrufen von Daten, Daten anzeigen und Bearbeiten von Daten. Datenquellen-Steuerelemente dient, eine konsistente Darstellung von Daten in datengebundenen Steuerelementen unabhängig von der Quelle dieser Daten bereitzustellen. Das Herzstück der Datenquellen-Steuerelementen in ASP.NET 2.0 ist Sie die DataSourceControl abstrakte Klasse. Die DataSourceControl-Klasse bietet eine grundlegende Implementierung von IDataSource-Schnittstelle und der IListSource-Schnittstelle, die den Sie das Datenquellen-Steuerelement als Datenquelle eines datengebundenen Steuerelements (über die neue DataSourceID-Wert-Eigenschaft zuweisen können weiter unten behandelt) und die Daten darin als Liste verfügbar machen. Jede Liste der Daten aus einem Datenquellen-Steuerelement wird als ein DataSourceView-Objekt verfügbar gemacht. Zugriff auf die DataSourceView-Instanzen wird durch die IDataSource-Schnittstelle bereitgestellt. Beispielsweise gibt die GetViewNames-Methode zurück, eine ICollection, die Sie zum Auflisten der DataSourceViews ermöglicht, die mit bestimmten Datenquellen-Steuerelement verknüpft ist, und die GetView-Methode können Sie eine bestimmte Instanz der DataSourceView anhand Ihres Namens zugreifen.

Datenquellen-Steuerelemente haben keine Benutzeroberfläche an. Sie werden implementiert, und da Server gesteuert wird, sodass sie die deklarativen Syntax unterstützen können, damit sie Zugriff auf seitenspezifischen Status haben, falls gewünscht. Datenquellen-Steuerelemente werden keine HTML-Markup an dem Client gerendert werden.

> [!NOTE]
> Wie Sie später sehen werden, Zwischenspeichern gibt es auch Vorteile, die mithilfe des Datenquellen-Steuerelemente abgerufen.


## <a name="storing-connection-strings"></a>Das Speichern von Verbindungszeichenfolgen

Bevor wir uns mit Blick auf die Datenquellen-Steuerelemente zu konfigurieren, sollten wir eine neue Funktion in ASP.NET 2.0-Verbindungszeichenfolgen zu behandeln. ASP.NET 2.0 werden einen neuen Abschnitt in der Konfigurationsdatei mit dem Sie ganz einfach Speichern von Verbindungszeichenfolgen, die dynamisch zur Laufzeit gelesen werden können. Die &lt;ConnectionStrings&gt; Abschnitt erleichtert das Speichern von Verbindungszeichenfolgen.

Der folgende Codeausschnitt fügt eine neue Verbindungszeichenfolge hinzu.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Wie bei der &lt;"appSettings"&gt; Abschnitt der &lt;ConnectionStrings&gt; im angezeigten Abschnitt außerhalb von den &lt;"System.Web"&gt; Abschnitt in der Konfigurationsdatei.


Um diese Verbindungszeichenfolge zu verwenden, können Sie die folgende Syntax, beim Festlegen der ConnectionString-Attribut eines Serversteuerelements.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Die &lt;ConnectionStrings&gt; Abschnitt kann auch verschlüsselt werden, damit vertraulicher Informationen nicht verfügbar gemacht wird. Diese Möglichkeit wird in einem Modul weiter unten erläutert.

## <a name="caching-data-sources"></a>Zwischenspeichern von Datenquellen

Jede DataSourceControl stellt vier Eigenschaften zum Konfigurieren der Zwischenspeicherung bereit; EnableCaching, CacheDuration, CacheExpirationPolicy und CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching ist, dass eine boolesche Eigenschaft, die bestimmt, und zwar unabhängig davon, ob das Zwischenspeichern für das Datenquellen-Steuerelement aktiviert ist.

## <a name="cacheduration-property"></a>CacheDuration-Eigenschaft

Die CacheDuration-Eigenschaft legt fest, die Anzahl der Sekunden an, denen der Cache gültig bleibt. Wenn diese Eigenschaft auf **0** bewirkt, dass der Cache gültig, bis Sie explizit für ungültig erklärt bleibt.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy-Eigenschaft

Die CacheExpirationPolicy-Eigenschaft kann festgelegt werden entweder **Absolute** oder **gleitend**. Festlegen auf Absolute bedeutet, die die maximale Zeitspanne, die die Daten zwischengespeichert werden, werden die Anzahl der Sekunden, die durch das CacheDuration-Eigenschaft angegeben ist. Die Ablaufzeit wird durch Festlegung auf gleitend, zurückgesetzt, wenn jeder Vorgang ausgeführt wird.

## <a name="cachekeydependency-property"></a>CacheKeyDependency-Eigenschaft

Wenn ein Zeichenfolgenwert für die CacheKeyDependency-Eigenschaft angegeben wird, wird eine neue, für diese Zeichenfolge basierende Cacheabhängigkeit ASP.NET einrichten. Dadurch können Sie explizit den Cache ungültig gemacht werden, indem einfach ändern oder Entfernen der CacheKeyDependency.

**Wichtige**: Wenn Identitätswechsel aktiviert ist und der Zugriff auf die Datenquelle und/oder der Inhalt der Daten beruht auf der Identität des Clients, es wird empfohlen, dass die Zwischenspeicherung deaktiviert werden, indem EnableCaching auf "false" festlegen. Wenn das Zwischenspeichern ist in diesem Szenario aktiviert, und ein anderen Benutzer als der Benutzer, der die Daten ursprünglich angefordert hat, eine Anforderung gibt, wird die Autorisierung mit der Datenquelle nicht erzwungen. Die Daten werden einfach aus dem Cache bedient werden.

## <a name="the-sqldatasource-control"></a>Dem SqlDataSource-Steuerelement

Das SqlDataSource-Steuerelement kann ein Entwickler in einer relationalen Datenbank, die ADO.NET unterstützt gespeicherten Daten zuzugreifen. Sie können den Anbieter "System.Data.SqlClient" verwenden, SQL Server-Datenbank, den Anbieter "System.Data.OleDb", der System.Data.Odbc-Anbieter oder der Anbieter System.Data.OracleClient Oracle den Zugriff auf den Zugriff auf. Aus diesem Grund wird dem SqlDataSource-Steuerelement sicherlich nicht nur verwendet für den Zugriff auf Daten in einer SQL Server-Datenbank.

Um dem SqlDataSource-Steuerelement verwenden möchten, und indem Sie einfach Geben Sie einen Wert für die ConnectionString-Eigenschaft, geben Sie einen SQL-Befehl oder gespeicherte Prozedur. Das SqlDataSource-Steuerelement übernimmt der Arbeit mit der zugrunde liegenden Architektur von ADO.NET. Öffnet die Verbindung, fragt die Datenquelle oder führt die gespeicherte Prozedur, die Daten zurückgegeben werden, und schließt dann die Verbindung für Sie.

> [!NOTE]
> Da die DataSourceControl-Klasse automatisch die Verbindung geschlossen wird, sollten sie die Anzahl der Anrufe von Kunden durch den Verlust von Verbindungen mit der Datenbank generiert reduzieren.


Der folgende Codeausschnitt bindet ein DropDownList-Steuerelement an ein SqlDataSource-Steuerelement, das mithilfe der Verbindungszeichenfolge, die in der Konfigurationsdatei wie oben gezeigt gespeichert sind.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Wie oben gezeigt wurde, gibt der Datenreader, der dem SqlDataSource-Steuerelement den Modus für die Datenquelle an. Im obigen Beispiel wird die DataSourceMode auf DataReader festgelegt. In diesem Fall wird dem SqlDataSource-Steuerelement ein IDataReader-Objekt, das mit einem Vorwärtscursor und schreibgeschützte Cursor zurückgegeben. Der angegebene Typ des Objekts, das zurückgegeben wird, wird vom Anbieter gesteuert, die verwendet wird. In diesem Fall verwende ich den Anbieter "System.Data.SqlClient" gemäß der &lt;ConnectionStrings&gt; -Abschnitt der Datei "Web.config". Aus diesem Grund wird das Objekt, das zurückgegeben wird, vom Typ SqlDataReader sein. Durch Angabe des Werts DataSourceMode DataSet, können die Daten in einem DataSet auf dem Server gespeichert werden. In diesem Modus können Sie Funktionen wie sortieren, Paging usw. hinzufügen. Wenn ich die Datenbindung wurde dem SqlDataSource-Steuerelement zu einem GridView-Steuerelement, würde ausgewählt den DataSet-Modus. Im Fall von einem DropDownList-Steuerelement ist der DataReader-Modus jedoch die richtige Wahl.

> [!NOTE]
> Beim Zwischenspeichern von einem SqlDataSource-Steuerelement oder einem AccessDataSource, muss der Datenreader DataSet festgelegt werden. Es wird eine Ausnahme ausgelöst, wenn mit einem DataReader DataSourceMode der Zwischenspeicherung aktiviert.


## <a name="sqldatasource-properties"></a>SqlDataSource-Eigenschaften

Im folgenden werden einige der Eigenschaften des dem SqlDataSource-Steuerelement.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Ein boolescher Wert, der angibt, ob ein select-Befehl abgebrochen wird, wenn einer der Parameter null ist. Standardmäßig "true".

### <a name="conflictdetection"></a>ConflictDetection

In einer Situation, in denen mehrere Benutzer eine Datenquelle zur gleichen Zeit aktualisieren werden können, bestimmt die ConflictDetection-Eigenschaft das Verhalten von dem SqlDataSource-Steuerelement. Diese Eigenschaft wird zu einer der Werte der Enumeration ConflictOptions ausgewertet. Diese Werte sind **CompareAllValues** und **OverwriteChanges**. Wenn auf OverwriteChanges, letzten Person, die Daten in der Datenquelle zu schreiben, alle vorherigen Änderungen überschreibt. Wenn die ConflictDetection-Eigenschaft auf CompareAllValues festgelegt ist, Parameter für die von SelectCommand zurückgegebenen Spalten erstellt und Parameter werden auch zum Speichern der ursprünglichen Werte in jeder dieser Spalten, die dem SqlDataSource-Steuerelement, sodass erstellt Bestimmen Sie, und zwar unabhängig davon, ob die Werte geändert haben, da SelectCommand ausgeführt wurde.

### <a name="deletecommand"></a>DeleteCommand

Legt fest oder ruft die SQL-Zeichenfolge, die beim Löschen von Zeilen aus der Datenbank verwendet. Dies kann entweder eine SQL-Abfrage oder der Name einer gespeicherten Prozedur sein.

### <a name="deletecommandtype"></a>DeleteCommandType

Legt fest oder ruft dem Typ der Delete-Befehl, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Gibt die Parameter zurück, die durch die DeleteCommand des Objekts mit dem SqlDataSource-Steuerelement verknüpfte SqlDataSourceView verwendet werden.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Diese Eigenschaft wird verwendet, um das Format der Parameter mit den ursprünglichen Werten in Fällen anzugeben, in denen die ConflictDetection-Eigenschaft auf CompareAllValues festgelegt ist. Der Standardwert ist {0} was bedeutet, dass die ursprüngliche Wertparameter den gleichen Namen wie der ursprüngliche Parameter dauert. Das heißt, ist der Name des Felds "EmployeeID", der ursprünglichen Value-Parameter wäre @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Legt fest oder ruft die SQL-Zeichenfolge, die zum Abrufen von Daten aus der Datenbank verwendet wird. Dies kann entweder eine SQL-Abfrage oder der Name einer gespeicherten Prozedur sein.

### <a name="selectcommandtype"></a>SelectCommandType

Legt fest oder ruft dem Typ der select-Befehl, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Gibt zurück, die Parameter, die von SelectCommand des Objekts mit dem SqlDataSource-Steuerelement verknüpfte SqlDataSourceView verwendet werden.

### <a name="sortparametername"></a>SortParameterName

Übernimmt oder bestimmt den Namen der Parameter einer gespeicherten Prozedur, der beim Abrufen von Sortieren von Daten mit den Datenquellen-Steuerelement verwendet wird. Nur gültig, wenn SelectCommandType auf StoredProcedure festgelegt ist.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Eine durch Semikolons getrennte Zeichenfolge, die die Datenbanken und Tabellen in einer SQL Server-Cacheabhängigkeit verwendet angibt. (SQL-cacheabhängigkeiten werden in einem Modul weiter unten erläutert.)

### <a name="updatecommand"></a>UpdateCommand

Legt fest oder ruft die SQL-Zeichenfolge, die beim Aktualisieren von Daten in der Datenbank verwendet wird. Dies kann entweder eine SQL-Abfrage oder der Name einer gespeicherten Prozedur sein.

### <a name="updatecommandtype"></a>UpdateCommandType

Legt fest oder ruft dem Typ des updatebefehls, entweder eine SQL-Abfrage (Text) oder eine gespeicherte Prozedur (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Gibt die Parameter zurück, die durch die UpdateCommand für das SqlDataSourceView-Objekt, das mit dem SqlDataSource-Steuerelement verknüpfte verwendet werden.

## <a name="the-accessdatasource-control"></a>AccessDataSource-Steuerelement

AccessDataSource-Steuerelement von der SqlDataSource-Klasse abgeleitet ist und wird verwendet, um Daten in einer Microsoft Access-Datenbank gebunden werden soll. Die ConnectionString-Eigenschaft für das AccessDataSource-Steuerelement ist eine schreibgeschützte Eigenschaft. Anstatt die ConnectionString-Eigenschaft, wird die Datendatei-Eigenschaft verwendet, um auf die Access-Datenbank zu verweisen, wie unten dargestellt.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Die AccessDataSource der "ProviderName", der dem Basis-SqlDataSource-Steuerelement wird immer auf "System.Data.OleDb" festgelegt und eine Verbindung mit der Datenbank mithilfe von Microsoft.Jet.OLEDB.4.0 OLE DB-Anbieters. AccessDataSource-Steuerelement können keine Verbindung mit einer Access-Datenbank ein Kennwort geschützt. Wenn Sie für die Verbindung mit einer Datenbank ein Kennwort geschützt haben, sollten Sie das SqlDataSource-Steuerelement verwenden.

> [!NOTE]
> Access-Datenbanken, die innerhalb der Website gespeicherten platziert werden soll, in der App\_Verzeichnis "Data". ASP.NET lässt sich nicht auf Dateien in diesem Verzeichnis zu durchsuchenden aus. Sie müssen das Prozesskonto lesen und Schreiben der App Berechtigungen zu erteilen\_Verzeichnis "Data" bei Verwendung von Access-Datenbanken.


## <a name="the-xmldatasource-control"></a>XmlDataSource-Steuerelement

Die XmlDataSource wird verwendet, um eine XML-Daten zu datengebundenen Steuerelementen Datenbindung. Können Sie an eine XML-Datei mit der Datendatei Eigenschaft binden, oder kann an eine XML-Zeichenfolge, die mithilfe der Data-Eigenschaft gebunden. Die XmlDataSource stellt XML-Attribute als bindbar Felder. In Fällen, in dem Sie zur Bindung an Werte, die nicht als Attribute dargestellt werden müssen, müssen Sie eine XSL-Transformation verwenden. Sie können auch die XPath-Ausdrücke zum Filtern von XML-Daten verwenden.

Beachten Sie die folgende XML-Datei ein:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Beachten Sie, dass die XmlDataSource eine XPath-Eigenschaft verwendet *Menschen/* um Filtern nur die &lt;Person&gt; Knoten. Die DropDownList-Datenbindung klicken Sie dann auf das LastName-Attribut, das mithilfe der DataTextField-Eigenschaft.

Während der XmlDataSource-Steuerelement an Daten auf nur-Lese XML-Daten gebunden werden soll in erster Linie verwendet wird, ist es möglich, die XML-Datendatei zu bearbeiten. Beachten Sie, dass in diesen Fällen Automatisches Einfügen, aktualisieren und Löschen von Informationen in der XML-Datei erfolgt kein automatisches wie bei anderen Datenquellen-Steuerelemente. Stattdessen müssen Sie Code schreiben, um die Daten mithilfe der folgenden Methoden der XmlDataSource-Steuerelement manuell zu bearbeiten.

### <a name="getxmldocument"></a>GetXmlDocument

Ruft ein XmlDocument-Objekt, das mit dem XML-Code abgerufen, indem die XmlDataSource ab.

### <a name="save"></a>Speichern

Speichert das XML-Dokument im Arbeitsspeicher in der Datenquelle an.

Es ist wichtig zu wissen, dass die Save-Methode funktioniert nur, wenn die beiden folgenden Bedingungen erfüllt sind:

1. Die XmlDataSource verwendet die DataFile-Eigenschaft, um an eine XML-Datei anstelle der Daten-Eigenschaft zu binden, zum Binden an XML-Daten im Arbeitsspeicher.
2. Es ist keine Transformation über die Transformation oder TransformFile-Eigenschaft angegeben.

Beachten Sie, dass die Save-Methode zu unerwarteten Ergebnissen bei der von mehreren Benutzern gleichzeitig aufgerufen führen kann.

## <a name="the-objectdatasource-control"></a>Das ObjectDataSource-Steuerelement

Die Datenquellen-Steuerelemente, die wir bis zu diesem Zeitpunkt abgedeckt haben sind eine ausgezeichnete Wahl für Anwendungen mit zwei Ebenen, wobei das Datenquellen-Steuerelement direkt mit dem Datenspeicher kommuniziert. Allerdings sind viele reale Anwendungen Anwendungen mit mehreren Ebenen, in denen möglicherweise ein Datenquellen-Steuerelement für die Kommunikation mit einem Geschäftsobjekt, das wiederum mit der Datenschicht kommunizieren. In diesen Fällen werden mit dem ObjectDataSource-Steuerelement die Rechnung gut füllt. Dem ObjectDataSource-Steuerelement funktioniert in Verbindung mit einem Quellobjekt. Das ObjectDataSource-Steuerelement wird eine Instanz der Source-Objekt, rufen Sie die angegebene Methode und Dispose der Objektinstanz alle innerhalb des Bereichs einer einzelnen Anforderung erstellt, verfügt das Objekt Instanzmethoden anstelle von statischen Methoden (Shared in Visual Basic). Aus diesem Grund muss Ihr Objekt zustandslos sein. Also sollte das Objekt abrufen und alle erforderlichen Ressourcen innerhalb der Spanne einer einzelnen Anforderung freigeben. Sie können steuern, wie das Quellobjekt durch Behandeln des Ereignisses ObjectCreating des ObjectDataSource-Steuerelement erstellt wird. Sie können erstellen Sie eine Instanz des Quellobjekts, und legen Sie dann die ObjectInstance-Eigenschaft der ObjectDataSourceEventArgs-Klasse, die dieser Instanz. Das ObjectDataSource-Steuerelement verwendet die Instanz, die im Ereignis ObjectCreating statt Sie selbst eine Instanz erstellt wird.

Wenn das Quellobjekt für ein ObjectDataSource-Steuerelement öffentliche statische Methoden (Shared in Visual Basic) verfügbar, die zum Abrufen und Ändern von Daten aufgerufen werden kann macht, wird ein ObjectDataSource-Steuerelement diese Methoden direkt aufrufen. Wenn ein ObjectDataSource-Steuerelement eine Instanz des Quellobjekts erstellen muss, um Methodenaufrufe zu machen, muss das Objekt einen öffentlichen Konstruktor enthalten, der keine Parameter akzeptiert. Das ObjectDataSource-Steuerelement ruft diesen Konstruktor, wenn es sich um eine neue Instanz der dem Quellobjekt erstellt.

Wenn das Quellobjekt nicht über einen öffentlichen Konstruktor ohne Parameter enthält, können Sie eine Instanz des Quellobjekts erstellen, die durch das ObjectDataSource-Steuerelement im Ereignis ObjectCreating verwendet werden.

## <a name="specifying-object-methods"></a>Angeben von Objektmethoden

Das Quellobjekt für ein ObjectDataSource-Steuerelement kann enthalten eine beliebige Anzahl von Methoden, die verwendet werden, um auswählen, einfügen, aktualisieren oder Löschen von Daten. Diese Methoden werden durch das ObjectDataSource-Steuerelement, das basierend auf den Namen der Methode aufgerufen, wie mithilfe der SelectMethod, InsertMethod, UpdateMethod bzw. DeleteMethod-Eigenschaft des ObjectDataSource-Steuerelement identifiziert. Das Quellobjekt zählen auch eine optionale SelectCount-Methode, die identifiziert wird, indem Sie das ObjectDataSource-Steuerelement, das mithilfe der SelectCountMethod-Eigenschaft, die die Anzahl der Gesamtzahl von Objekten in der Datenquelle zurückgibt. Das ObjectDataSource-Steuerelement ruft die SelectCount-Methode, nach eine Select-Methode aufgerufen wurde, um die Gesamtzahl der Datensätze in der Datenquelle für die Verwendung abzurufen, wenn paging.

## <a name="lab-using-data-source-controls"></a>Lab mithilfe des Datenquellen-Steuerelemente

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Übung 1: Anzeigen von Daten mit dem SqlDataSource-Steuerelement

In der folgenden Übung wird das SqlDataSource-Steuerelement für die Verbindung zur Northwind-Datenbank verwendet. Es wird davon ausgegangen, dass Sie Zugriff auf die Northwind-Datenbank auf einer SQL Server 2000-Instanz verfügen.

1. Erstellen Sie eine neue ASP.NET-Website.
2. Fügen Sie eine neue Datei "Web.config"-Datei hinzu.

    1. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf Neues Element hinzufügen.
    2. Wählen Sie die Webkonfigurationsdatei aus der Liste der Vorlagen aus, und klicken Sie auf Hinzufügen.
3. Bearbeiten der &lt;ConnectionStrings&gt; wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Wechseln Sie zur Codeansicht und fügen Sie ein ConnectionString-Attribut und einem SelectCommand-Attribut, um die &lt;Asp: SqlDataSource&gt; steuern, wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Fügen Sie ein neues GridView-Steuerelement hinzu, von der Entwurfsansicht.
6. Wählen Sie aus der Dropdownliste "Datenquelle auswählen" im GridView-Aufgaben SqlDataSource1.
7. Mit der rechten Maustaste auf Default.aspx, und wählen Sie die im Browser anzeigen. Klicken Sie auf Ja, wenn Sie aufgefordert werden, um zu speichern.
8. Das GridView zeigt die Daten aus der Tabelle Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Übung 2: Bearbeiten von Daten mit dem SqlDataSource-Steuerelement

In der folgenden Übung zeigt, wie zur Datenbindung eine DropDownList steuern, mit der deklarativen Syntax und können Sie die Daten aus der DropDownList-Steuerelement zu bearbeiten.

1. Löschen Sie in der Entwurfsansicht das GridView-Steuerelement aus "default.aspx" ein. 

    **Wichtige**: das SqlDataSource-Steuerelement auf der Seite zu verlassen.
2. Fügen Sie ein DropDownList-Steuerelement an "default.aspx" ein.
3. Wechseln Sie zur Quellansicht.
4. Hinzufügen ein Attributs DataSourceId DataTextField und DataValueField zur Verwendung der &lt;Asp: DropDownList&gt; steuern, wie folgt: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Speichern Sie "default.aspx", und zeigen Sie ihn im Browser an. Beachten Sie, dass die DropDownList alle Produkte aus der Datenbank Northwind enthält.
6. Schließen Sie den Browser.
7. Fügen Sie ein neues Textfeldsteuerelement unter das DropDownList-Steuerelement in der Quellansicht "default.aspx" hinzu. Ändern Sie die ID-Eigenschaft des Textfelds in TxtProductName an.
8. Fügen Sie eine neue Schaltflächen-Steuerelement hinzu, unter dem TextBox-Steuerelement. Ändern Sie die ID-Eigenschaft der Schaltfläche in BtnUpdate und der Text-Eigenschaft **Update Product Name**.
9. Fügen Sie das UpdateCommand-Eigenschaft und zwei neue UpdateParameters in der Quellansicht "default.aspx" wie folgt dem SqlDataSource-Steuerelement-Tag an: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Beachten Sie, dass es, dass zwei Parameter ("ProductName" und "ProductID") hinzugefügt, die in diesem Code aktualisieren. Diese Parameter werden der Text-Eigenschaft der TxtProductName Textfeld und die SelectedValue-Eigenschaft der DropDownList DdlProducts zugeordnet.
10. Wechseln Sie zur Entwurfsansicht, und doppelklicken Sie auf das Schaltflächen-Steuerelement einen Ereignishandler hinzufügen.
11. Fügen Sie den folgenden Code, um die BtnUpdate\_klicken Sie auf Code: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Mit der rechten Maustaste auf Default.aspx, und wählen sie im Browser anzuzeigen. Klicken Sie auf Ja, wenn Sie aufgefordert werden, um alle Änderungen zu speichern.
13. ASP.NET 2.0 bei partiellen Klassen kann für die Kompilierung zur Laufzeit. Es ist nicht erforderlich, zum Erstellen einer Anwendung, um codeänderungen wirksam werden.
14. Wählen Sie ein Produkt aus der Dropdownliste aus.
15. Geben Sie einen neuen Namen für das ausgewählte Produkt in das Textfeld ein, und klicken Sie dann auf die Schaltfläche "Aktualisieren".
16. Der Name des Produkts wird in der Datenbank aktualisiert.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Übung 3: verwenden das ObjectDataSource-Steuerelement

In dieser Übung wird das ObjectDataSource-Steuerelement und ein Quellobjekt verwenden, um die Interaktion mit der Northwind-Datenbank veranschaulicht.

1. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf Neues Element hinzufügen.
2. Wählen Sie in der Vorlagenliste Web Form. Ändern Sie den Namen in object.aspx, und klicken Sie auf Hinzufügen.
3. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf Neues Element hinzufügen.
4. Wählen Sie in der Vorlagenliste Klasse. Ändern Sie den Namen der Klasse zu NorthwindData.cs, und klicken Sie auf Hinzufügen.
5. Klicken Sie auf "Ja", wenn Sie aufgefordert werden, die Klasse der App hinzufügen\_Ordner "Code".
6. Fügen Sie der NorthwindData.cs-Datei den folgenden Code hinzu: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Fügen Sie den folgenden Code zur Quellansicht des object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Speichern Sie alle Dateien, und navigieren Sie object.aspx.
9. Interagieren Sie mit der Schnittstelle, indem Sie Details anzeigen, bearbeiten die Mitarbeiter, Mitarbeiter hinzufügen und Löschen von Mitarbeitern.
