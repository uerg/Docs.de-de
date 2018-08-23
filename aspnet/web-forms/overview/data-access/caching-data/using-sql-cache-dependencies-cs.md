---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: Verwenden von SQL-Cacheabhängigkeiten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Die einfachste zwischenspeicherstrategie werden zwischengespeicherte Daten nach einer angegebenen Zeitspanne ablaufen können. Aber diese einfache Vorgehensweise bedeutet, dass die zwischengespeicherten Daten Maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: ddd0ce9e8e0f69da6f9c0f65165e4842d460f0c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828032"
---
<a name="using-sql-cache-dependencies-c"></a>Verwenden von SQL-Cacheabhängigkeiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) oder [PDF-Datei herunterladen](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> Die einfachste zwischenspeicherstrategie werden zwischengespeicherte Daten nach einer angegebenen Zeitspanne ablaufen können. Dieser einfache Ansatz bedeutet jedoch, dass die zwischengespeicherten Daten verwaltet, mit der zugrunde liegenden Datenquelle, sodass sich die veraltete Daten zu lang oder aktuelle Daten, die zu früh abgelaufen ist. Ein besserer Ansatz ist die Verwendung die SqlCacheDependency-Klasse, damit Daten zwischengespeichert bleiben, bis die zugrunde liegenden Daten in der SQL-Datenbank geändert wurde. In diesem Tutorial erfahren Sie, wie.


## <a name="introduction"></a>Einführung

Die Zwischenspeicherung Techniken untersucht, der [Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement](caching-data-with-the-objectdatasource-cs.md) und [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-cs.md) Tutorials verwendet eine zeitbasierte Ablauf So entfernen Sie die Daten aus dem Cache nach einem angegebenen Zeitraum. Dieser Ansatz ist die einfachste Möglichkeit, die leistungsverbesserungen von Zwischenspeichern für Daten "begrenzte Veraltung" auszugleichen. Durch Auswahl einer Zeit-Ablauf der *x* Sekunden, Seitenentwickler concedes genießen Sie die Leistungsvorteile des caching für nur *x* in Sekunden, aber können verlassen Sie sich, dass ihre Daten nie länger als maximal veraltet sein werden der *x* Sekunden. Natürlich für statische Daten die *x* kann erweitert werden und die Lebensdauer der Webanwendung, wie in untersucht wurde die [Zwischenspeichern von Daten beim Anwendungsstart](caching-data-at-application-startup-cs.md) Tutorial.

Beim Zwischenspeichern von Daten wird eine zeitbasierte Ablauf wird häufig für die einfache Verwendung ausgewählt, jedoch ist häufig eine unzureichende Lösung. Im Idealfall würde Daten in der Datenbank zwischengespeicherte bleiben, bis die zugrunde liegenden Daten in der Datenbank geändert wurde; erst dann würde der Cache entfernt werden. Dieser Ansatz maximiert die Leistungsvorteile des caching und die Dauer der veraltete Daten minimiert. Allerdings müssen um diese Vorteile gibt es einige System vorhanden sein, die bekannt sind, wenn die zugrunde liegenden Datenbankdaten geändert wurde, und die entsprechenden Werte aus dem Cache entfernt. Vor ASP.NET 2.0 waren Seitenentwickler, die für die Implementierung dieses Systems verantwortlich.

ASP.NET 2.0 bietet eine [ `SqlCacheDependency` Klasse](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) und entfernt, die notwendige Infrastruktur, um zu bestimmen, wenn eine Änderung in der Datenbank aufgetreten ist, damit die entsprechenden Elemente zwischengespeichert werden kann. Es gibt zwei Verfahren, um zu bestimmen, wenn die zugrunde liegenden Daten geändert haben: Benachrichtigung und abrufen. Nach der Erläuterung der Unterschiede zwischen Benachrichtigung und abrufen, erstellen wir die Infrastruktur zum Abruf zu unterstützen, und klicken Sie dann die Informationen zur Verwendung der `SqlCacheDependency` Klasse in deklarativen und die programmgesteuerte Szenarien.

## <a name="understanding-notification-and-polling"></a>Grundlegendes zur Benachrichtigung und Abrufen

Es gibt zwei Techniken, die verwendet werden können, um zu bestimmen, wann die Daten in einer Datenbank geändert wurde: Benachrichtigung und abrufen. Benachrichtigung die Datenbank wird automatisch die ASP.NET-Laufzeit benachrichtigt, wenn die Ergebnisse einer bestimmten Abfrage geändert wurden, seit der letzten der Abfrage Ausführung aus, an diesem Punkt werden die zwischengespeicherten Elemente, die der Abfrage zugeordnet werden entfernt. Mit der Abruf behält die Datenbank-Server Informationen, wenn bestimmte Tabellen zuletzt aktualisiert wurden. Die ASP.NET-Laufzeit ruft regelmäßig die Datenbank aus, um zu überprüfen, welche Tabellen geändert haben, da sie in den Cache eingegeben wurden. Diese Tabellen, deren Daten geändert wurden, haben, ihre zugehörigen Cache-Elemente, die entfernt wird.

Die Benachrichtigungsoption erfordert weniger Einrichtung als abrufen und eine feiner abgestimmte ist, da es sich um die Änderungen nicht auf Tabellenebene, sondern auch auf Abfrageebene verfolgt. Leider sind Benachrichtigungen nur in den vollständigen Editionen von Microsoft SQL Server 2005 (d. h., der nicht-Express-Editionen) zur Verfügung. Allerdings kann die Abrufoption für alle Versionen von Microsoft SQL Server von 7.0 auf 2005 verwendet werden. Da diese Tutorials die Express-Edition von SQL Server 2005 verwenden, konzentrieren wir uns auf das Einrichten und verwenden die Abrufoption. Prüfen Sie im Abschnitt weiter lesen am Ende dieses Tutorials Weitere Ressourcen für SQL Server 2005-s-Benachrichtigungsfunktionen aus.

Mit abrufen, die Datenbank konfiguriert werden muss, um eine Tabelle mit dem Namen einzuschließen `AspNet_SqlCacheTablesForChangeNotification` Listenfeldsteuerelement mit drei Spalten - `tableName`, `notificationCreated`, und `changeId`. Diese Tabelle enthält eine Zeile für jede Tabelle, die Daten enthält, die möglicherweise in einer SQL-Cacheabhängigkeit in der Webanwendung verwendet werden. Die `tableName` Spalte gibt den Namen der Tabelle beim `notificationCreated` gibt an, das Datum und Uhrzeit, die die Zeile der Tabelle hinzugefügt wurde. Die `changeId` Spalte ist vom Typ `int` und einen Anfangswert von 0 hat. Der Wert wird mit jeder Änderung an der Tabelle erhöht.

Zusätzlich zu den `AspNet_SqlCacheTablesForChangeNotification` Tabelle muss die Datenbank auch Trigger für jede der Tabellen, die angezeigt werden, möglicherweise in einer SQL-Cacheabhängigkeit einschließen. Diese Trigger werden ausgeführt, wenn eine Zeile eingefügt, aktualisiert oder gelöscht wird, und erhöhen Sie die Tabelle s `changeId` Wert `AspNet_SqlCacheTablesForChangeNotification`.

Die ASP.NET-Laufzeit verfolgt die aktuelle `changeId` für eine Tabelle, die beim Zwischenspeichern von Daten mithilfe einer `SqlCacheDependency` Objekt. Die Datenbank in regelmäßigen Abständen aktiviert ist und ein beliebiger `SqlCacheDependency` Objekte, deren `changeId` unterscheidet sich von dem Wert in der Datenbank entfernt werden, da eine abweichende `changeId` Wert gibt an, es wurde eine Änderung an der Tabelle, da die Daten zwischengespeichert wurde.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Schritt 1: Untersuchen der`aspnet_regsql.exe`Befehlszeilenprogramm

Die Datenbank muss mit dem Abruf Ansatz Setup, um die Infrastruktur, die oben beschriebenen enthalten sein: einer vordefinierten-Tabelle (`AspNet_SqlCacheTablesForChangeNotification`), eine Reihe von gespeicherten Prozeduren und Trigger für jede der Tabellen, die in SQL-cacheabhängigkeiten im Web verwendet werden können die Anwendung. Diese Tabellen, gespeicherten Prozeduren und Trigger können über das Programm über die Befehlszeile erstellt werden `aspnet_regsql.exe`, befindet sich der `$WINDOWS$\Microsoft.NET\Framework\version` Ordner. Zum Erstellen der `AspNet_SqlCacheTablesForChangeNotification` Tabelle und der zugehörigen gespeicherten Prozeduren, führen Sie den folgenden von der Befehlszeile aus:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Um diese Befehle auszuführen, der angegebenen Datenbank-Anmeldename in muss, der [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) und [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) Rollen. Untersuchen Sie das T-SQL-gesendet, um die Datenbank, indem die `aspnet_regsql.exe` Befehl Zeile Programm, beziehen sich auf [in diesem Blogeintrag](http://scottonwriting.net/sowblog/posts/10709.aspx).


Beispielsweise fügen die Infrastruktur für den Abruf mit einer Microsoft SQL Server-Datenbank mit dem Namen `pubs` auf einem Datenbankserver mit dem Namen `ScottsServer` mithilfe der Windows-Authentifizierung, navigieren Sie in das entsprechende Verzeichnis und, über die Befehlszeile eingeben:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Nachdem die Datenbankebene-Infrastruktur hinzugefügt wurde, müssen wir den Trigger auf diese Tabellen hinzufügen, die in SQL-cacheabhängigkeiten verwendet werden. Verwenden der `aspnet_regsql.exe` über die Befehlszeile erneut programmieren, aber geben Sie den Tabellennamen mit der `-t` wechseln und anstatt der `-ed` wechseln verwenden `-et`, wie folgt:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Die Trigger Hinzufügen der `authors` und `titles` Tabellen auf die `pubs` Datenbank auf `ScottsServer`, verwenden Sie:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

In diesem Tutorial fügen Sie die Trigger die `Products`, `Categories`, und `Suppliers` Tabellen. Wir werden die bestimmten Befehlszeilensyntax in Schritt 3 betrachten.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Schritt 2: Verweisen auf in einer Microsoft SQL Server 2005 Express Edition-Datenbank`App_Data`

Die `aspnet_regsql.exe` Befehlszeilenprogramm erforderlich, dass die Datenbank- und Servername, fügen die erforderlichen Abruf-Infrastruktur. Aber was die Datenbank- und Servername, für eine Microsoft SQL Server 2005 Express-Datenbank ist, die in befinden die `App_Data` Ordner? Anstatt zu ermitteln, was die Datenbank und Server-Namen sind, ich Ve gefunden wird, dass der einfachste Ansatz zum Anfügen der Datenbank, ist die `localhost\SQLExpress` Instanz der Datenbank, und benennen Sie die Daten mithilfe [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Wenn Sie eine Vollversion von SQL Server 2005 auf Ihrem Computer installiert haben, müssen Sie wahrscheinlich bereits SQL Server Management Studio auf Ihrem Computer installiert. Wenn Sie nur die Express Edition haben, können Sie die kostenlose Herunterladen [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Schließen Sie Visual Studio starten. Öffnen Sie SQL Server Management Studio, und wählen Sie zum Herstellen einer Verbindung mit der `localhost\SQLExpress` Server mithilfe der Windows-Authentifizierung.


![Fügen Sie an den Server Localhost\SQLExpress an](using-sql-cache-dependencies-cs/_static/image1.gif)

**Abbildung 1**: Anfügen an die `localhost\SQLExpress` Server


Nach dem Herstellen einer Verbindung mit dem Server, Management Studio zeigt den Server und Unterordner für die Datenbanken, Sicherheit und So weiter. Mit der rechten Maustaste auf den Ordner "Datenbanken", und wählen Sie die Attach-Option. Hierdurch wird das Dialogfeld "Datenbanken anfügen" (siehe Abbildung 2). Klicken Sie auf die Schaltfläche "hinzufügen", und wählen Sie die `NORTHWND.MDF` Datenbankordner in Ihrem Web-Anwendung s. `App_Data` Ordner.


[![Fügen Sie die NORTHWND an. MDF-Datenbank aus dem Ordner "App_Data"](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Abbildung 2**: Fügen Sie der `NORTHWND.MDF` -Datenbank von der `App_Data` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image2.png))


Dadurch wird die Datenbank auf den Ordner "Datenbanken" hinzugefügt. Der Name der Datenbank ist möglicherweise den vollständigen Pfad zur Datei oder der vollständige Pfad mit vorangestelltem eine [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Zu vermeiden, geben Sie in diesen lange Datenbanknamen Verwendung das Aspnet\_regsql.exe-Befehlszeilentool, benennen Sie die Datenbank in einen mehr Benutzerfreundlicher Namen mit der rechten Maustaste auf die Datenbank gerade angefügt und Auswahl umbenennen. Ich Ve DataTutorials Meine Datenbank umbenannt.


![Benennen Sie die angefügte Datenbank in einer mehr Benutzerfreundlicher Namen](using-sql-cache-dependencies-cs/_static/image3.gif)

**Abbildung 3**: die angefügte Datenbank in einen mehr Benutzerfreundlicher Namen umbenennen


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Schritt 3: Hinzufügen der Abruf-Infrastruktur mit der Datenbank Northwind

Nun, wir angefügt haben die `NORTHWND.MDF` -Datenbank von der `App_Data` Ordner wir erneut kann jetzt die Abruf-Infrastruktur hinzugefügt werden. Angenommen, Sie haben die Datenbank zu DataTutorials umbenannt werden, führen Sie die folgenden vier Befehle aus:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Klicken Sie nach dem Ausführen dieser vier Befehle, mit der rechten Maustaste auf den Datenbanknamen in Management Studio, wechseln Sie zu dem Untermenü Tasks, und wählen Sie trennen. Klicken Sie dann schließen Sie Management Studio, und öffnen Sie Visual Studio erneut.

Sobald Visual Studio erneut geöffnet wurde, einen Drilldown in die Datenbank über den Server-Explorer. Beachten Sie die neue Tabelle (`AspNet_SqlCacheTablesForChangeNotification`), die neue gespeicherte Prozeduren und Trigger auf die `Products`, `Categories`, und `Suppliers` Tabellen.


![Die Datenbank enthält jetzt die erforderlichen Abruf-Infrastruktur](using-sql-cache-dependencies-cs/_static/image4.gif)

**Abbildung 4**: die Datenbank enthält jetzt die erforderlichen Abruf-Infrastruktur


## <a name="step-4-configuring-the-polling-service"></a>Schritt 4: Konfigurieren des Diensts abrufen

Nach dem Erstellen der erforderlichen Tabellen, Trigger und gespeicherte Prozeduren in der Datenbank an, der der letzte Schritt ist so konfigurieren Sie den Dienst abrufen, das erfolgt über `Web.config` durch Angeben der Datenbanken verwenden und das Abrufintervall in Millisekunden. Das folgende Markup fragt den Northwind-Datenbank einmal pro Sekunde.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

Die `name` Wert in der `<add>` Element (NorthwindDB) ordnet einen lesbaren Namen mit einer bestimmten Datenbank. Bei der Arbeit mit SQL-cacheabhängigkeiten müssen wir verweisen auf den Namen der hier definierten sowie die Tabelle, der die zwischengespeicherten Daten basiert. Wir sehen, wie Sie mit der `SqlCacheDependency` Klasse Programmgesteuertes Verbinden von SQL-cacheabhängigkeiten mit zwischengespeicherten Daten in Schritt 6.

Nach eine SQL-Cacheabhängigkeit eingerichtet wurde, den Abruf-System eine Verbindung mit der Datenbanken, die definiert, der `<databases>` Elemente jeder `pollTime` Millisekunden und führen Sie die `AspNet_SqlCachePollingStoredProcedure` gespeicherte Prozedur. Diese gespeicherte Prozedur - hinzugefügten zurück, in Schritt 3: Verwenden der `aspnet_regsql.exe` Befehlszeilentool - gibt die `tableName` und `changeId` Werte für jeden Datensatz in `AspNet_SqlCacheTablesForChangeNotification`. Veraltete SQL-cacheabhängigkeiten, die aus dem Cache entfernt werden.

Die `pollTime` Einstellung führt einen Kompromiss zwischen Leistung und Daten "begrenzte Veraltung". Eine kleine `pollTime` Wert erhöht die Anzahl der Anforderungen an die Datenbank, aber mehr schnell veraltete Daten aus dem Cache entfernt. Eine größere `pollTime` Wert verringert die Anzahl der Anforderungen, erhöht aber die Verzögerung zwischen den Back-End-bei datenänderungen und wenn die zugehörigen Elementen entfernt werden. Glücklicherweise wird die datenbankanforderung einer einfachen gespeicherten Prozedur, s, die nur wenige Zeilen zurückgeben, aus einer Tabelle einfacher, ausgeführt. Jedoch Experimentieren mit verschiedenen `pollTime` Werten, um einen optimalen Kompromiss zwischen zwischen ermitteln Datenbank zugreifen und Daten "begrenzte Veraltung" für Ihre Anwendung. Die kleinste `pollTime` zulässige Wert ist 500.

> [!NOTE]
> Das obige Beispiel enthält eine einzelne `pollTime` Wert in der `<sqlCacheDependency>` -Element, aber Sie können optional angeben, die `pollTime` Wert in der `<add>` Element. Dies ist nützlich, wenn Sie mehrere Datenbanken angegeben haben, und der abrufhäufigkeit pro Datenbank angepasst werden soll.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Schritt 5: Deklarativ arbeiten, mit der SQL-Cacheabhängigkeiten

In Schritt 1 bis 4 erläutert, wie die notwendige Infrastruktur einrichten und Konfigurieren des Systems abrufen. Mit dieser Infrastruktur vorhanden ist können wir nun, das dem Datencache Elemente mit einer zugeordneten SQL-Cacheabhängigkeit mithilfe von deklarativen oder programmtechnischen Techniken hinzufügen. In diesem Schritt untersuchen wir deklarativ arbeiten mit SQL-cacheabhängigkeiten. In Schritt 6 betrachten wir der programmgesteuerte Ansatz.

Die [Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement](caching-data-with-the-objectdatasource-cs.md) Tutorial untersucht die deklarative Zwischenspeichern Funktionen von dem ObjectDataSource-Steuerelement. Legen Sie ganz einfach die `EnableCaching` Eigenschaft `true` und `CacheDuration` Eigenschaft einige Zeitintervall, das "ObjectDataSource" werden die Daten, die von des zugrunde liegenden Objekts zurückgegeben wird, für das angegebene Intervall automatisch zwischengespeichert. Dem ObjectDataSource-Steuerelement können auch eine oder mehrere SQL-cacheabhängigkeiten.

Um deklarativ mithilfe von SQL-cacheabhängigkeiten zu demonstrieren, öffnen Sie die `SqlCacheDependencies.aspx` auf der Seite die `Caching` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox in den Designer. Legen Sie die GridView s `ID` zu `ProductsDeclarative` und sein Smarttag, auswählen, um die Bindung an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSourceDeclarative`.


[![Erstellen Sie eine neue, mit dem Namen ProductsDataSourceDeclarative "ObjectDataSource"](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Abbildung 5**: Erstellen einer neuen "ObjectDataSource" mit dem Namen `ProductsDataSourceDeclarative` ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image4.png))


Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse, und legen Sie die Dropdownliste in der Registerkarte "SELECT" `GetProducts()`. Wählen Sie in der Registerkarte "Updates" der `UpdateProduct` -Überladung mit drei Eingabeparameter - `productName`, `unitPrice`, und `productID`. Legen Sie die Dropdownlisten auf (keine), auf den Registerkarten INSERT- und DELETE.


[![Verwenden Sie die UpdateProduct-Überladung mit drei Eingabeparameter](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Abbildung 6**: Verwenden der UpdateProduct-Überladung, mit drei Eingabeparameter ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image6.png))


[![Legen Sie die Dropdown-Liste auf (keine) für die INSERT- und DELETE-Registerkarten](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Abbildung 7**: Legen Sie die Dropdown-Liste (keine) für die einfügen und Löschen von Registerkarten ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image8.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten werden Visual Studio BoundFields und CheckBoxFields in den GridView-Ansicht für die einzelnen Datenfelder erstellt. Entfernen Sie alle Felder jedoch `ProductName`, `CategoryName`, und `UnitPrice`, und formatieren Sie diese Felder nach Bedarf. GridView s Smarttags überprüfen Sie die Kontrollkästchen Paging aktivieren, aktivieren Sie sortieren und Bearbeiten aktivieren. Visual Studio wird festgelegt, das "ObjectDataSource"-s `OldValuesParameterFormatString` Eigenschaft `original_{0}`. In der Reihenfolge für die GridView s bearbeiten-Funktion ordnungsgemäß funktioniert, entweder entfernen Sie diese Eigenschaft vollständig aus die deklarative Syntax oder wieder auf den Standardwert `{0}`.

Fügen Sie schließlich ein Label-Steuerelement über das GridView und seine `ID` Eigenschaft `ODSEvents` und die zugehörige `EnableViewState` Eigenschaft `false`. Nach diesen Änderungen sollte im deklarativen Markup Ihrer Seite s etwa wie folgt aussehen. Beachten Sie, dass haben eine Reihe von ästhetischen angepasst werden, um die GridView-Felder, die nicht zur Veranschaulichung der Abhängigkeit-Funktionalität in SQL-Cache erforderlich sind vorgenommen.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für das "ObjectDataSource"-s `Selecting` Ereignis und in fügen sie den folgenden Code:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Bedenken Sie, dass das "ObjectDataSource"-s `Selecting` Ereignis ausgelöst wird, nur beim Abrufen von Daten aus der zugrunde liegende Objekt. Wenn es sich bei dem ObjectDataSource-Steuerelement, der von seinen eigenen Cache auf die Daten zugreift, wird dieses Ereignis nicht ausgelöst.

Besuchen Sie diese Seite über einen Browser. Seit es sollte noch in der Implementierung der Zwischenspeicherung, jedes Mal, Sie Seite, zu sortieren oder bearbeiten das Raster der Seite, speichern wie in Abbildung 8 dargestellt anzeigen, der Text, die auswählen-Ereignis ausgelöst wird.


[![Das "ObjectDataSource"-s-Auswahl-Ereignis ausgelöst wird, jedes Mal, die die GridView bearbeitet, ausgelagert wird, oder die sortiert](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Abbildung 8**: das "ObjectDataSource" s `Selecting` löst jede Ereigniszeit GridView werden ausgelagert, bearbeiteter oder sortiert ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image10.png))


Wie in beschrieben der [Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement](caching-data-with-the-objectdatasource-cs.md) Tutorial Festlegen der `EnableCaching` Eigenschaft `true` bewirkt, dass dem ObjectDataSource-Steuerelement zum Zwischenspeichern von Daten für die Dauer angegeben, die von der `CacheDuration` Eigenschaft. Dem ObjectDataSource-Steuerelement verfügt auch über eine [ `SqlCacheDependency` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), die eine oder mehrere SQL-cacheabhängigkeiten hinzufügt, auf die zwischengespeicherten Daten, die mit dem Muster:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

In denen *DatabaseName* ist der Name der Datenbank gemäß der `name` Attribut der `<add>` Element im `Web.config`, und *TableName* ist der Name der Datenbanktabelle. Z. B. ein ObjectDataSource-Steuerelement zu erstellen, die auf unbestimmte Zeit zwischenspeichert, die basierend auf einer SQL-Cacheabhängigkeit für die Northwind-s `Products` Tabelle, legen Sie das "ObjectDataSource"-s `EnableCaching` Eigenschaft `true` und die zugehörige `SqlCacheDependency` Eigenschaft NorthwindDB:Products.

> [!NOTE]
> Können Sie eine SQL-Cacheabhängigkeit *und* eine zeitbasierte Ablauf durch Festlegen von `EnableCaching` zu `true`, `CacheDuration` auf das Zeitintervall und `SqlCacheDependency` zu den Datenbank- und Namen. Dem ObjectDataSource-Steuerelement entfernen seine Daten, wenn die zeitbasierte Ablaufzeit erreicht ist oder die Abruf-System fest, dass die zugrunde liegenden Datenbankdaten geändert wurde, je nachdem, was zuerst eintritt.


GridView in `SqlCacheDependencies.aspx` zeigt Daten aus zwei Tabellen – `Products` und `Categories` (das Produkt s `CategoryName` Feld abgerufen wird, über eine `JOIN` auf `Categories`). Aus diesem Grund soll an zwei SQL-cacheabhängigkeiten: NorthwindDB:Products; NorthwindDB:Categories.


[![Konfigurieren Sie das "ObjectDataSource", um Unterstützung für das Zwischenspeichern mithilfe von SQL-Cacheabhängigkeiten für die Kategorien und Produkte](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Abbildung 9**: Konfigurieren Sie auf dem ObjectDataSource-Steuerelement, unterstützen das Zwischenspeichern mithilfe von SQL-Cacheabhängigkeiten `Products` und `Categories` ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image12.png))


Rufen Sie nach dem Konfigurieren der "ObjectDataSource", um Unterstützung für das Zwischenspeichern wird die Seite über einen Browser ein. In diesem Fall das Text-Auswahl-Ereignis wird ausgelöst, sollte auf der ersten Besuch der Seite angezeigt werden, jedoch sollte verschwinden beim paging, sortieren oder durch Klicken auf die bearbeiten "oder" Abbrechen ". Dies ist, da die Daten in den "ObjectDataSource"-s-Cache geladen werden, gibt es erst er bleibt die `Products` oder `Categories` Tabellen geändert werden, oder die Daten über die GridView aktualisiert werden.

Nach dem Auslösen von paging für das Raster, und beachten Sie den Mangel an das Ereignis auswählen Text, öffnen Sie ein neues Browserfenster, und navigieren Sie zu den Grundlagen-Lernprogramm, in der bearbeiten, einfügen und Löschen im Abschnitt (`~/EditInsertDelete/Basics.aspx`). Aktualisieren Sie den Namen oder den Preis eines Produkts. Klicken Sie dann auf den ersten Browserfenster anzeigen einer anderen Seite der Daten, Sortieren des Rasters, oder klicken Sie auf eine Zeile s-Schaltfläche "Bearbeiten". Dieses Mal die auswählen-Ereignis ausgelöst wird, sollte erneut angezeigt, als die zugrunde liegenden Datenbank, die Daten wurden (siehe Abbildung 10) geändert. Wenn der Text nicht angezeigt wird, warten Sie einige Minuten, und versuchen Sie es noch mal. Beachten Sie, dass Änderungen an der Dienst abrufen Prüfungen der `Products` Tabelle alle `pollTime` Millisekunden, daher besteht eine Verzögerung zwischen Wenn die zugrunde liegenden Daten aktualisiert wird und wenn die zwischengespeicherten Daten entfernt wird.


[![Ändern die Products-Tabelle entfernt die zwischengespeicherten Produktdaten](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Abbildung 10**: Ändern der Products-Tabelle entfernt die Produktdaten an zwischengespeichert ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Schritt 6: Programmgesteuert arbeiten, mit der`SqlCacheDependency`Klasse

Die [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-cs.md) Tutorial erläutert die Vorteile der Verwendung einer separaten Caching-Schicht in der Architektur im Gegensatz zu eng gekoppelt, das Zwischenspeichern mit dem ObjectDataSource-Steuerelement. In diesem Lernprogramm erstellt es einen `ProductsCL` Klasse, um zu veranschaulichen, mit dem Datencache programmgesteuert arbeiten. Um SQL-cacheabhängigkeiten in der Caching-Schicht zu nutzen, verwenden Sie die `SqlCacheDependency` Klasse.

Mit dem Abruf-System eine `SqlCacheDependency` Objekt muss ein bestimmtes Paar aus Datenbank und Tabelle zugeordnet sein. Der folgende Code erstellt z.B. eine `SqlCacheDependency` -Objekt auf Grundlage der Northwind-Datenbank s `Products` Tabelle:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Die beiden Eingabeparameter der `SqlCacheDependency` s Konstruktor sind bzw. den Datenbank- und Tabellennamen. Wie Sie mit der "ObjectDataSource"-s `SqlCacheDependency` -Eigenschaft, der Datenbanknamen verwendet, ist identisch mit dem im angegebenen Wert der `name` Attribut des der `<add>` Element im `Web.config`. Der Tabellenname ist der tatsächliche Name der Datenbanktabelle.

Zuordnen einer `SqlCacheDependency` mit einem Element, das dem Datencache hinzugefügt wird, gehen Sie die `Insert` methodenüberladungen, die eine Abhängigkeit akzeptiert. Der folgende Code fügt *Wert* , das dem Datencache für eine unbestimmte Dauer, ordnet jedoch mit einem `SqlCacheDependency` auf die `Products` Tabelle. Kurz gesagt, *Wert* wird im Cache verbleiben, bis es aufgrund von arbeitsspeicherbeschränkungen entfernt wird, oder da, die der Abruf-System erkannt hat die `Products` Tabelle wurde geändert, da er zwischengespeichert wurde.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

Die Caching-Schicht s `ProductsCL` Klasse speichert derzeit Daten aus der `Products` Tabelle über eine zeitbasierte Ablauf von 60 Sekunden. Lassen Sie s, die dieser Klasse zu aktualisieren, sodass sie stattdessen SQL-cacheabhängigkeiten verwendet. Die `ProductsCL` Klasse s `AddCacheItem` -Methode, die für das Hinzufügen von Daten in den Cache verantwortlich ist, enthält derzeit den folgenden Code:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Aktualisieren Sie diesen Code mit einer `SqlCacheDependency` Objekt, sondern mit der `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Um diese Funktionalität zu testen, fügen Sie einer GridView-Ansicht auf der Seite unterhalb der vorhandenen `ProductsDeclarative` GridView. Legen Sie diese neuen GridView s `ID` zu `ProductsProgrammatic` und durch sein Smarttag, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSourceProgrammatic`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsCL` Klasse. dabei wird die Dropdownlisten in der SELECT und UPDATE-Registerkarten, um `GetProducts` und `UpdateProduct`bzw.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsCL-Klasse](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Abbildung 11**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsCL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image16.png))


[![Wählen Sie die GetProducts-Methode aus der Registerkarte "SELECT" s Dropdown-Liste](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Abbildung 12**: Wählen Sie die `GetProducts` Methode aus der Registerkarte "Wählen Sie" s Dropdown-Liste ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image18.png))


[![Wählen Sie aus der Updateliste Registerkarte s Dropdown-Liste der UpdateProduct-Methode](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Abbildung 13**: Wählen Sie aus der Registerkarte "UPDATE"-s-Dropdown-Liste der UpdateProduct-Methode ([klicken Sie, um das Bild in voller Größe anzeigen](using-sql-cache-dependencies-cs/_static/image20.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten werden Visual Studio BoundFields und CheckBoxFields in den GridView-Ansicht für die einzelnen Datenfelder erstellt. Wie Sie mit der ersten GridView, die auf dieser Seite hinzugefügt, entfernen Sie alle Felder jedoch `ProductName`, `CategoryName`, und `UnitPrice`, und formatieren Sie diese Felder nach Bedarf. GridView s Smarttags überprüfen Sie die Kontrollkästchen Paging aktivieren, aktivieren Sie sortieren und Bearbeiten aktivieren. Wie bei der `ProductsDataSourceDeclarative` "ObjectDataSource", Visual Studio wird festgelegt. die `ProductsDataSourceProgrammatic` "ObjectDataSource" s `OldValuesParameterFormatString` Eigenschaft `original_{0}`. In der Reihenfolge für die GridView s bearbeiten-Funktion ordnungsgemäß funktioniert, legen Sie diese Eigenschaft zurück, in `{0}` (oder entfernen Sie die eigenschaftenzuweisung vollständig aus der deklarativen Syntax).

Nach Abschluss dieser Aufgaben, sollte das daraus resultierende GridView und "ObjectDataSource" deklarative Markup wie folgt aussehen:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

So testen Sie die SQL-Cacheabhängigkeit in der Caching-Schicht, legen Sie einen Haltepunkt der `ProductCL` Klasse s `AddCacheItem` -Methode, und klicken Sie dann dem Debuggen beginnen. Wenn Sie zum ersten Mal besuchen `SqlCacheDependencies.aspx`, sollte der Haltepunkt erreicht werden, wie die Daten zum ersten Mal angefordert und im Cache platziert wurden. Klicken Sie anschließend auf eine andere Seite der GridView zu verschieben, oder eine der Spalten zu sortieren. Dies bewirkt, dass der GridView, die Daten erneut abzufragen, aber die Daten gefunden werden sollte, im Cache seit dem `Products` Datenbanktabelle wurde nicht geändert. Wenn die Daten wiederholt nicht im Cache gefunden werden, stellen Sie sicher, dass auf Ihrem Computer über ausreichend Arbeitsspeicher verfügbar ist, und versuchen Sie es erneut.

Öffnen Sie nach dem Paging durch ein paar Seiten der GridView, ein zweites Browserfenster, und navigieren Sie zu den Grundlagen-Lernprogramm, in der bearbeiten, einfügen und Löschen im Abschnitt (`~/EditInsertDelete/Basics.aspx`). Aktualisieren eines Datensatzes aus der Tabelle Products und zeigen Sie anschließend aus dem ersten Browserfenster, eine neue Seite, oder klicken Sie auf einen der Sortierung-Header.

In diesem Szenario sehen Sie zwei Möglichkeiten: entweder der Haltepunkt wird erreicht, gibt an, dass die zwischengespeicherten Daten aufgrund der Änderung in der Datenbank entfernt wurde oder der Haltepunkt wird nicht erreicht, was bedeutet, dass `SqlCacheDependencies.aspx` ist nun veraltete Daten angezeigt. Wenn der Haltepunkt nicht getroffen wird, ist es wahrscheinlich, da der Abruf-Dienst noch nicht ausgelöst wurde, da die Daten geändert wurden. Beachten Sie, dass Änderungen an der Dienst abrufen Prüfungen der `Products` Tabelle alle `pollTime` Millisekunden, daher besteht eine Verzögerung zwischen Wenn die zugrunde liegenden Daten aktualisiert wird und wenn die zwischengespeicherten Daten entfernt wird.

> [!NOTE]
> Diese Verzögerung ist eher angezeigt werden, wenn eines der Produkte über die GridView in Bearbeitung `SqlCacheDependencies.aspx`. In der [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-cs.md) Tutorial, die wir hinzugefügt, die `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit, um sicherzustellen, dass die Daten, die bearbeitet wird, über die `ProductsCL` s-Klasse `UpdateProduct` Methode aus dem Cache entfernt wurde. Allerdings ersetzt diese Cacheabhängigkeit beim Ändern der `AddCacheItem` Methode weiter oben in diesem Schritt und somit die `ProductsCL` Klasse werden weiterhin die zwischengespeicherten Daten angezeigt, bis die Abruf-System die Änderung fest der `Products` Tabelle. Erfahren Sie, wie auf die Rückmeldung der `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit in Schritt 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Schritt 7: Ein zwischengespeichertes Element mehrere Abhängigkeiten zugeordnet

Bedenken Sie, dass die `MasterCacheKeyArray` Cacheabhängigkeit wird verwendet, um sicherzustellen, dass *alle* produktbezogene Daten aus dem Cache entfernt werden, wenn ein Element in diesem Zusammenhang aktualisiert wird. Z. B. die `GetProductsByCategoryID(categoryID)` Methode Caches `ProductsDataTables` Instanzen für jeden eindeutigen *CategoryID* Wert. Wenn eines dieser Objekte entfernt wird, die `MasterCacheKeyArray` Cacheabhängigkeit wird sichergestellt, dass die anderen auch entfernt werden. Wenn die zwischengespeicherten Daten geändert werden, besteht die Möglichkeit ohne diese Cacheabhängigkeit, dass andere zwischengespeicherte Produktdaten möglicherweise veraltet. Daher es wichtig, dass wir verwalten die `MasterCacheKeyArray` Abhängigkeit den Zwischenspeicher bei Verwendung von SQL-cacheabhängigkeiten. Allerdings speichern die Daten s `Insert` Methode kann nur ein einzelnes Abhängigkeitsobjekt.

Bei der Arbeit mit SQL-cacheabhängigkeiten müssen wir darüber hinaus mehreren Datenbanktabellen als Abhängigkeiten zuzuordnen. Z. B. die `ProductsDataTable` zwischengespeicherte in die `ProductsCL` -Klasse enthält die Kategorie und Lieferanten Namen für jedes Produkt, aber die `AddCacheItem` Methode verwendet nur eine Abhängigkeit auf `Products`. In diesem Fall, wenn der Benutzer den Namen der Kategorie oder Lieferant, aktualisiert die zwischengespeicherten Produktdaten werden im Cache verbleiben und veraltet sein. Aus diesem Grund werden wir die Produktdaten an zwischengespeicherte Objekt abhängig machen soll nicht nur die `Products` -Tabelle jedoch in der `Categories` und `Suppliers` auch Tabellen.

Die [ `AggregateCacheDependency` Klasse](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) bietet eine Möglichkeit für ein Element im Cache mehrere Abhängigkeiten zugeordnet. Erstellen Sie zunächst eine `AggregateCacheDependency` Instanz. Fügen Sie den Satz von Abhängigkeiten mithilfe der `AggregateCacheDependency` s `Add` Methode. Wenn das Element in Datencache danach einfügen möchten, übergeben die `AggregateCacheDependency` Instanz. Wenn *alle* von der `AggregateCacheDependency` s-Instanz Abhängigkeiten ändern, wird das zwischengespeicherte Element entfernt werden.

Das folgende Beispiel zeigt den aktualisierten Code für die `ProductsCL` Klasse s `AddCacheItem` Methode. Die Methode erstellt die `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit zusammen mit `SqlCacheDependency` von Objekten für die die `Products`, `Categories`, und `Suppliers` Tabellen. Diese werden alle in einer kombiniert `AggregateCacheDependency` Objekt mit dem Namen `aggregateDependencies`, die übergeben wird dann in der `Insert` Methode.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Testen Sie diesen neuen Code heraus. Jetzt wird nun die `Products`, `Categories`, oder `Suppliers` Tabellen dazu führen, dass die zwischengespeicherten Daten entfernt werden. Darüber hinaus die `ProductsCL` s-Klasse `UpdateProduct` -Methode, die aufgerufen wird, beim Bearbeiten eines Produkts über GridView, entfernt der `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit, wodurch die zwischengespeicherten `ProductsDataTable` entfernt werden und die Daten erneut auf dem nächsten abgerufen werden sollen Anforderung.

> [!NOTE]
> SQL-cacheabhängigkeiten können auch verwendet werden, mit [ausgabezwischenspeicherung](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Eine Demonstration dieser Funktion finden Sie unter: [mithilfe ASP.NET-Ausgabecache mit SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Zusammenfassung

Beim Zwischenspeichern von Daten werden im Idealfall die Daten im Cache verbleiben, bis er in der Datenbank geändert wird. Mit ASP.NET 2.0 können SQL-cacheabhängigkeiten erstellt und in deklarativ und programmatisch Szenarien verwendet werden. Eine der Herausforderungen bei diesem Ansatz wird ermittelt, wenn Sie die Daten geändert wurde. Die Vollversionen von Microsoft SQL Server 2005 bieten Notification-Funktionen, die eine Anwendung warnen können, wenn ein Ergebnis der Abfrage geändert hat. Für die Express Edition von SQL Server 2005 und älteren Versionen von SQL Server muss stattdessen ein Abruf-System verwendet werden. Einrichten der erforderlichen Abruf-Infrastruktur ist recht einfach.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Verwenden von Abfragebenachrichtigungen in Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Erstellen eine Abfragebenachrichtigung](https://msdn.microsoft.com/library/ms188669.aspx)
- [Ausgabezwischenspeicherung in ASP.NET mit der `SqlCacheDependency` Klasse](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server-Registrierungstool (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Übersicht über die `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Marko Rangel Teresa Murphy und Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](caching-data-at-application-startup-cs.md)
> [Weiter](caching-data-with-the-objectdatasource-vb.md)
