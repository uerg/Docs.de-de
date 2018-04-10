---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Mithilfe von SQL-Cache-Abhängigkeiten (VB) | Microsoft Docs
author: rick-anderson
description: Die einfachste zwischenspeicherstrategie werden zwischengespeicherte Daten nach einer festgelegten Zeitspanne ablaufen zu lassen. Aber diese einfache Ansatz bedeutet, dass die zwischengespeicherten Daten Maintai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 452d856fe352ef2eb7dfcc3f3acd6aa5bcb5ae41
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-sql-cache-dependencies-vb"></a>Mithilfe von SQL-Cache-Abhängigkeiten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) oder [PDF herunterladen](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> Die einfachste zwischenspeicherstrategie werden zwischengespeicherte Daten nach einer festgelegten Zeitspanne ablaufen zu lassen. Aber diese einfache Ansatz bedeutet, dass die zwischengespeicherten Daten verwaltet, mit der zugrunde liegenden Datenquelle, was veraltete Daten, die zu lang gehalten werden oder die aktuellen Daten, die zu früh abgelaufen ist. Ein besserer Ansatz ist die Verwendung von SqlCacheDependency-Klasse, sodass Daten zwischengespeichert bleiben, bis die zugrunde liegenden Daten in der SQL-Datenbank geändert wurde. In diesem Lernprogramm erfahren Sie, wie.


## <a name="introduction"></a>Einführung

Das zwischenspeichernden Techniken in untersucht die [Zwischenspeichern von Daten mit der ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) und [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) Lernprogramme verwendet eine zeitbasierte Ablauf So entfernen Sie die Daten aus dem Cache nach einem angegebenen Zeitraum. Dieser Ansatz ist die einfachste Möglichkeit, um die Leistungsvorteile des Zwischenspeicherns für Daten Überalterung auszugleichen. Durch Auswahl einer Zeit Ablauf *x* Sekunden, der Entwickler einer Seite concedes genießen Sie die Leistungsvorteile von caching für nur *x* Sekunden, aber können einfach halten, dass ihre Daten nie länger als maximal veraltet sein werden der *x* Sekunden. Natürlich für statische Daten *x* können auf die Lebensdauer der Webanwendung erweitert werden, wie in überprüft wurde die [Zwischenspeichern von Daten beim Anwendungsstart](caching-data-at-application-startup-vb.md) Lernprogramm.

Beim Zwischenspeichern von Datenbankdaten eine zeitbasierte Ablauf häufig der einfachen Verwendung ausgewählt ist, jedoch ist häufig eine Lösung für unzureichend. Im Idealfall würden Daten in der Datenbank zwischengespeicherte bleiben, bis die zugrunde liegenden Daten in der Datenbank geändert wurde; nur dann würde der Cache entfernt werden. Dieser Ansatz maximiert die Leistungsvorteile von caching und die Dauer der veraltete Daten minimiert. Allerdings müssen um es diese Vorteile einige System vorhanden sein, die bekannt, wenn Daten in der zugrunde liegenden Datenbank geändert wurde, und die entsprechenden Elemente aus dem Cache entfernt. Vor dem ASP.NET 2.0 wurden Entwickler von Seiten für die Implementierung dieses Systems verantwortlich.

ASP.NET 2.0 verfügt über eine [ `SqlCacheDependency` Klasse](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) und die erforderliche Infrastruktur, um zu bestimmen, wenn eine Änderung in der Datenbank aufgetreten ist, damit die entsprechenden Elemente zwischengespeichert werden, kann entfernt werden. Es gibt zwei Techniken, um zu bestimmen, wenn die zugrunde liegenden Daten geändert haben: Benachrichtigung und abrufen. Nach der Abstimmung der Unterschiede zwischen Benachrichtigung und Abruf, wir erstellen die Infrastruktur zum Abruf unterstützen und untersuchen Sie zum Verwenden der `SqlCacheDependency` Klasse in deklarativen und programmgesteuert Szenarien.

## <a name="understanding-notification-and-polling"></a>Grundlegendes zur Benachrichtigung und Abrufen

Es gibt zwei Techniken, die verwendet werden können, um zu bestimmen, wann die Daten in einer Datenbank geändert wurde: Benachrichtigung und abrufen. Mit der Benachrichtigung die Datenbank wird automatisch die ASP.NET-Laufzeit benachrichtigt, wenn die Ergebnisse einer bestimmten Abfrage geändert wurden, seit der letzten der Abfrage Ausführung, an welchem Punkt die zwischengespeicherte Elemente, die der Abfrage zugeordnet werden entfernt. Mit der Abruf behält der Datenbankserver Informationen, wenn bestimmte Tabellen zuletzt aktualisiert wurden. Die ASP.NET-Laufzeit ruft regelmäßig die Datenbank, um zu überprüfen, welche Tabellen geändert haben, da sie in den Cache eingegeben wurden. Diese Tabellen, deren Daten geändert wurden, haben ihre zugehörigen Cacheelemente entfernt.

Die Benachrichtigungsoption erfordert weniger Setup als Abruf und genauere ist, da Änderungen auf Abfrageebene statt auf Tabellenebene verfolgt. Leider werden Benachrichtigungen nur in den vollständigen Editionen von Microsoft SQL Server 2005 (d. h., der nicht-Express-Editionen) verfügbar. Allerdings kann die Abruf-Option für alle Versionen von Microsoft SQL Server 7.0 auf 2005 verwendet werden. Da diese Lernprogramme die Express Edition von SQL Server 2005 verwenden, konzentrieren wir uns auf einrichten und verwenden die Option abrufen. Finden Sie am Ende dieses Lernprogramms für weitere Ressourcen für SQL Server 2005-s-Benachrichtigungsfunktionen in Abschnitt weiter lesen.

Mit Abruf, die Datenbank konfiguriert werden, damit eine Tabelle mit dem Namen `AspNet_SqlCacheTablesForChangeNotification` , die drei Spalten - hat `tableName`, `notificationCreated`, und `changeId`. Diese Tabelle enthält eine Zeile für jede Tabelle, die Daten enthält, die eventuell in einer SQL-Cacheabhängigkeit in der Webanwendung verwendet werden. Die `tableName` Spalte gibt den Namen der Tabelle beim `notificationCreated` gibt an, Datum und Uhrzeit, die die Zeile der Tabelle hinzugefügt wurde. Die `changeId` Spalte ist vom Typ `int` und hat einen Anfangswert von 0. Der Wert wird mit jeder Änderung an der Tabelle erhöht.

Zusätzlich zu den `AspNet_SqlCacheTablesForChangeNotification` Tabelle, die Datenbank muss auch Trigger für jede der Tabellen, die angezeigt werden, möglicherweise in einer SQL-Cacheabhängigkeit eingeschlossen werden sollen. Diese Trigger werden ausgeführt, wenn eine Zeile eingefügt, aktualisiert oder gelöscht wird, und erhöhen Sie die Tabelle s `changeId` Wert in `AspNet_SqlCacheTablesForChangeNotification`.

Die ASP.NET-Laufzeit verfolgt den aktuellen `changeId` für eine Tabelle, die beim Zwischenspeichern von Daten mithilfe einer `SqlCacheDependency` Objekt. Die Datenbank in regelmäßigen Abständen aktiviert ist und ein beliebiger `SqlCacheDependency` -Objekte, deren `changeId` unterscheidet sich von dem Wert in der Datenbank entfernt werden, da eine abweichende `changeId` Wert gibt an, da die Daten zwischengespeichert wurde eine Änderung an der Tabelle stattgefunden hat.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Schritt 1: Untersuchen der`aspnet_regsql.exe`Programm über die Befehlszeile

Die Datenbank muss mit dem Ansatz der Abruf die Infrastruktur, die oben beschriebenen enthalten sein: einer vordefinierten-Tabelle (`AspNet_SqlCacheTablesForChangeNotification`), eine Handvoll von gespeicherten Prozeduren und Trigger für jede der Tabellen, die in SQL-Cache-Abhängigkeiten im Web verwendet werden können die Anwendung. Diese Tabellen, gespeicherte Prozeduren und Trigger können mithilfe des Programms über die Befehlszeile erstellt werden `aspnet_regsql.exe`, befindet sich der `$WINDOWS$\Microsoft.NET\Framework\version` Ordner. Zum Erstellen der `AspNet_SqlCacheTablesForChangeNotification` Tabelle und der zugehörigen gespeicherten Prozeduren, führen Sie Folgendes über die Befehlszeile:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Diese Befehle ausführen, der angegebenen Datenbank-Benutzername in muss, der [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) und [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) Rollen. Die T-SQL-gesendet, um die Datenbank durch Untersuchen der `aspnet_regsql.exe` command-Line-Programm, beziehen sich auf [Blogeintrag](http://scottonwriting.net/sowblog/posts/10709.aspx).


Z. B. zum Hinzufügen der Infrastruktur für den Abruf mit einer Microsoft SQL Server-Datenbank mit dem Namen `pubs` auf einem Datenbankserver mit dem Namen `ScottsServer` mithilfe der Windows-Authentifizierung, navigieren Sie in das entsprechende Verzeichnis und, über die Befehlszeile eingeben:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Nachdem die Infrastruktur auf Datenbankebene hinzugefügt wurde, muss der Trigger auf diese Tabellen hinzufügen, die in SQL-Cache-Abhängigkeiten verwendet wird. Verwenden Sie die `aspnet_regsql.exe` Befehlszeile erneut, aber geben Sie die Tabelle mit der `-t` wechseln und statt der `-ed` verwenden `-et`, wie folgt:


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Die Trigger zum Hinzufügen der `authors` und `titles` Tabellen in der `pubs` Datenbank auf `ScottsServer`, verwenden:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Für dieses Lernprogramm fügen Sie den Trigger, um die `Products`, `Categories`, und `Suppliers` Tabellen. Sehen wir uns die bestimmten Befehlszeilensyntax in Schritt 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Schritt 2: Verweisen auf in einer Microsoft SQL Server 2005 Express Edition-Datenbank`App_Data`

Die `aspnet_regsql.exe` Befehlszeilenprogramm muss die Datenbank und den Servernamen zum Hinzufügen der erforderlichen Abruf-Infrastruktur. Neuerungen der Datenbank und den Servernamen für eine Microsoft SQL Server 2005 Express-Datenbank, in dem sich befindet, aber die `App_Data` Ordner? Ermitteln, welche die Namen der Datenbank und Server sind, anstatt ich Ve wurde erkannt, dass die einfachste Methode zum Anfügen der Datenbank, die `localhost\SQLExpress` -Datenbankinstanz und benennen Sie die Daten mit [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Wenn Sie eine Vollversion von SQL Server 2005 auf Ihrem Computer installiert haben, müssen Sie wahrscheinlich bereits SQL Server Management Studio auf Ihrem Computer installiert. Wenn Sie nur die Express Edition verfügen, können Sie die kostenlose Herunterladen [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Starten Sie Visual Studio schließen. Als Nächstes öffnen Sie SQL Server Management Studio, und wählen Sie für die Verbindung der `localhost\SQLExpress` Server mithilfe der Windows-Authentifizierung.


![Fügen Sie an der Localhost\SQLExpress Server](using-sql-cache-dependencies-vb/_static/image1.gif)

**Abbildung 1**: Anfügen an die `localhost\SQLExpress` Server


Nach dem Herstellen einer Verbindung mit dem Server, wird die Management Studio zeigt den Server und Unterordnern für Datenbanken, Sicherheit usw. haben. Mit der rechten Maustaste auf den Ordner "Datenbanken", und wählen Sie die Attach-Option. Hierdurch wird das Dialogfeld "Datenbanken anfügen" angezeigt (siehe Abbildung 2). Klicken Sie auf die Schaltfläche "hinzufügen", und wählen Sie die `NORTHWND.MDF` Datenbankordner in Ihrer Web-Anwendung s `App_Data` Ordner.


[![Fügen Sie die NORTHWND. MDF-Datenbank aus dem Ordner "App_Data"](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Abbildung 2**: Fügen Sie der `NORTHWND.MDF` Datenbank aus der `App_Data` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image2.png))


Dadurch wird der Ordner "Datenbanken" die Datenbank hinzugefügt. Der Datenbankname ist möglicherweise den vollständigen Pfad zur Datenbankdatei oder der vollständige Pfad mit vorangestelltem eine [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Um zu vermeiden, dass in diesem langwierige Datenbanknamen eingeben, bei Verwendung der Aspnet\_regsql.exe-Befehlszeilentool, umbenennen, die die Datenbank in einen mehr Human benutzerfreundliche Namen mit der rechten Maustaste auf die Datenbank gerade angefügt und der Auswahl umbenennen. Ich Ve Meine Datenbank in DataTutorials umbenannt.


![Benennen Sie die angefügte Datenbank in einen mehr Human benutzerfreundliche Namen](using-sql-cache-dependencies-vb/_static/image3.gif)

**Abbildung 3**: Benennen Sie die angefügte Datenbank in einen mehr Human benutzerfreundliche Namen


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Schritt 3: Hinzufügen der Abruf-Infrastruktur zur Northwind-Datenbank

Nun, dass wir angefügt haben die `NORTHWND.MDF` Datenbank aus der `App_Data` Ordner wir re kann jetzt die Abruf-Infrastruktur hinzugefügt werden. Angenommen, Sie haben die Datenbank DataTutorials umbenannt haben, führen Sie die folgenden vier Befehle ein:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Klicken Sie nach dem Ausführen dieser vier Befehle, mit der rechten Maustaste auf den Datenbanknamen in Management Studio, wechseln Sie zu dem Untermenü Tasks, und wählen Sie trennen. Klicken Sie dann schließen Sie Management Studio, und öffnen Sie Visual Studio erneut.

Nachdem Sie Visual Studio geöffnet wurde, einen Drilldown in die Datenbank über den Server-Explorer. Beachten Sie die neue Tabelle (`AspNet_SqlCacheTablesForChangeNotification`), die neue gespeicherte Prozeduren und Trigger auf die `Products`, `Categories`, und `Suppliers` Tabellen.


![Die Datenbank enthält jetzt die notwendigen Abruf-Infrastruktur](using-sql-cache-dependencies-vb/_static/image4.gif)

**Abbildung 4**: die Datenbank enthält jetzt die notwendigen Abruf-Infrastruktur


## <a name="step-4-configuring-the-polling-service"></a>Schritt 4: Konfigurieren des Abrufvorgangs-Diensts

Nach dem Erstellen der erforderlichen Tabellen, Trigger und gespeicherte Prozeduren in der Datenbank, ist der letzte Schritt den Abruf-Dienst konfiguriert werden, die über erfolgt `Web.config` durch Angeben der Datenbanken verwenden und das Abrufintervall in Millisekunden. Das folgende Markup fragt den Northwind-Datenbank einmal pro Sekunde ab.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

Die `name` Wert in der `<add>` Element (NorthwindDB) ordnet einen menschenlesbaren Namen mit einer bestimmten Datenbank. Bei der Arbeit mit SQL-Cache-Abhängigkeiten müssen wir verweisen auf den Namen der hier definierten sowie die Tabelle, der die zwischengespeicherten Daten basiert. Wir sehen, wie die `SqlCacheDependency` Klasse programmgesteuert Zuordnen von SQL-Cache-Abhängigkeiten mit zwischengespeicherten Daten in Schritt 6.

Sobald eine SQL-Cacheabhängigkeit eingerichtet wurde, wird der Abruf-System mit definierten Datenbanken verbinden der `<databases>` Elemente jedes `pollTime` Millisekunden und führen Sie die `AspNet_SqlCachePollingStoredProcedure` gespeicherte Prozedur. Diese gespeicherte Prozedur - hinzugefügten Sichern in Schritt 3 mithilfe der `aspnet_regsql.exe` Befehlszeilentool - gibt die `tableName` und `changeId` Werte für jeden Datensatz in `AspNet_SqlCacheTablesForChangeNotification`. Veraltete SQL-Cache-Abhängigkeiten, die aus dem Cache entfernt werden.

Die `pollTime` Einstellung führt einen Kompromiss zwischen Leistung und Überalterung der Daten. Ein kleines `pollTime` Wert erhöht die Anzahl der Anforderungen an die Datenbank, jedoch mehr schnell veraltete Daten aus dem Cache entfernt. Eine größere `pollTime` Wert verringert die Anzahl von datenbankanforderungen, erhöht aber die Verzögerung zwischen den Back-End-bei datenänderungen und wann die zugehörigen Cacheelemente entfernt werden. Glücklicherweise wird die Anforderung einer einfachen gespeicherten Prozedur zurückgeben nur wenige Zeilen aus der eine einfache, kleine Tabelle s ausgeführt. Aber führen Sie experimentieren Sie mit verschiedenen `pollTime` Werte, um eine optimale Balance zwischen finden-Datenbank Zugriff und Daten Überalterung für Ihre Anwendung. Die kleinste `pollTime` zulässige Wert ist 500.

> [!NOTE]
> Im obigen Beispiel stellt eine einzelne `pollTime` Wert in der `<sqlCacheDependency>` -Element, aber Sie können optional angeben, die `pollTime` Wert in der `<add>` Element. Dies ist hilfreich, wenn Sie mehrere Datenbanken angegeben haben und das Abrufintervall für jede Datenbank, die angepasst werden soll.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Schritt 5: Deklarativ arbeiten, mit der SQL-Cache-Abhängigkeiten

In Schritt 1 bis 4 wurde die notwendige Infrastruktur einrichten und konfigurieren die Abruf-System. Mit dieser Infrastruktur können wir nun Elemente für den Datencache mit einer zugeordneten SQL-Cacheabhängigkeit mit programmgesteuerten oder deklarative Verfahren hinzufügen. In diesem Schritt untersuchen wir deklarativ arbeiten mit SQL-Cache-Abhängigkeiten. In Schritt 6 sehen wir uns die programmgesteuerter Ansatz.

Die [Zwischenspeichern von Daten mit der ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) Lernprogramm untersucht die zwischenspeichernden deklarationsfunktionen von "ObjectDataSource". Durch Festlegen von einfach die `EnableCaching` Eigenschaft `True` und `CacheDuration` Eigenschaft einige Zeitintervall, das ObjectDataSource werden die Daten aus der zugrunde liegenden Objekt zurückgegeben, für das angegebene Intervall automatisch zwischengespeichert. Das ObjectDataSource können auch eine oder mehrere SQL-Cache-Abhängigkeiten.

Öffnen Sie zur Veranschaulichung der SQL-Cache-Abhängigkeiten deklarativ mithilfe der `SqlCacheDependencies.aspx` auf der Seite der `Caching` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer. Legen Sie die GridView s `ID` auf `ProductsDeclarative` , und wählen Sie das Smarttag, um die Bindung an eine neue, mit dem Namen ObjectDataSource `ProductsDataSourceDeclarative`.


[![Erstellen Sie eine neue, mit dem Namen ProductsDataSourceDeclarative ObjectDataSource](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource namens `ProductsDataSourceDeclarative` ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image4.png))


Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse, und legen Sie die Dropdownliste in der Registerkarte "SELECT" `GetProducts()`. Wählen Sie in der Registerkarte "UPDATE" die `UpdateProduct` -Überladung mit drei Eingabeparameter - `productName`, `unitPrice`, und `productID`. Legen Sie die Dropdown-Listen zu (keine) auf den Registerkarten INSERT- und DELETE.


[![Verwenden Sie die Überladung UpdateProduct mit drei Eingabeparameter](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Abbildung 6**: Verwenden Sie die UpdateProduct überladen mit drei Eingabeparameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image6.png))


[![Legen Sie das Dropdown-Liste (keine) für die INSERT- und DELETE-Registerkarten](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Abbildung 7**: Legen Sie das Dropdown-Liste (keine) für die einfügen und Löschen von Registerkarten ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image8.png))


Nach Abschluss des Assistenten für die Datenquelle konfigurieren erstellt Visual Studio BoundFields und CheckBoxFields in die GridView für die einzelnen Datenfelder. Entfernen Sie alle Felder jedoch `ProductName`, `CategoryName`, und `UnitPrice`, und formatieren Sie diese Felder nach Bedarf. Aus den GridView s smart Tag die Kontrollkästchen der Paging aktivieren, aktivieren Sie sortieren und Bearbeiten aktivieren. Visual Studio wird festgelegt, dass das ObjectDataSource-s `OldValuesParameterFormatString` Eigenschaft `original_{0}`. Damit die Funktion GridView s bearbeiten, ordnungsgemäß funktioniert, entfernen Sie entweder diese Eigenschaft vollständig aus der deklarative Syntax oder wieder auf den Standardwert `{0}`.

Schließlich fügen ein Bezeichnung-Websteuerelement über die GridView, und legen seine `ID` Eigenschaft, um `ODSEvents` und seine `EnableViewState` Eigenschaft, um `False`. Nachdem diese Änderungen vorgenommen wurden, sollte Ihre deklarative s-Seitenmarkup etwa wie folgt aussehen. Beachten Sie, dass Ve versucht, eine Anzahl von Layoutgründen Anpassungen an die GridView-Felder, die nicht zur Veranschaulichung der Funktion der SQL-Cache Abhängigkeit Funktionalität erforderlich sind.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für das ObjectDataSource-s `Selecting` Ereignis und in fügen sie den folgenden Code:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Bedenken Sie, dass das ObjectDataSource-s `Selecting` Ereignis ausgelöst wird, nur, wenn Daten aus der zugrunde liegenden Objekts abgerufen. Wenn das ObjectDataSource über einen eigenen Cache auf die Daten zugreift, wird dieses Ereignis nicht ausgelöst.

Besuchen Sie diese Seite über einen Browser. Seit wir sollte Ve noch zu implementieren, jeglichen Zwischenspeicherns, jedes Mal Sie Seite, sortieren oder bearbeiten das Raster der Seite "wie in Abbildung 8 gezeigt anzeigen, die für Text, auswählen-Ereignis ausgelöst werden.


[![Das ObjectDataSource-s auswählen-Ereignis ausgelöst wird, jedes Mal, die die GridView bearbeitet, ausgelagert wird, oder die Sorted](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Abbildung 8**: das ObjectDataSource s `Selecting` ausgelöst wird jede Ereigniszeit GridView ausgelagert wird, bearbeitet oder Sorted ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image10.png))


Wie wir gesehen, in haben der [Zwischenspeichern von Daten mit der ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) Lernprogramm Festlegen der `EnableCaching` Eigenschaft `True` bewirkt, dass das ObjectDataSource zum Zwischenspeichern der Daten für die Dauer von angegebenen seine `CacheDuration` Eigenschaft. Das ObjectDataSource verfügt auch über eine [ `SqlCacheDependency` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), die eine oder mehrere Abhängigkeiten von SQL-Cache auf die zwischengespeicherten Daten unter Verwendung des Musters hinzufügt:


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Auf dem *DatabaseName* ist der Name der Datenbank gemäß der `name` Attribut von der `<add>` Element im `Web.config`, und *TableName* ist der Name der Datenbanktabelle. Z. B. ein ObjectDataSource erstellen, der Daten auf unbestimmte Zeit zwischenspeichert, basierend auf einer SQL-Cacheabhängigkeit für die Northwind-s `Products` Tabelle, legen Sie das ObjectDataSource-s `EnableCaching` Eigenschaft `True` und dessen `SqlCacheDependency` Eigenschaft NorthwindDB:Products.

> [!NOTE]
> Können Sie eine SQL-Cacheabhängigkeit *und* eine zeitbasierte Ablauf durch Festlegen von `EnableCaching` zu `True`, `CacheDuration` um das Zeitintervall und `SqlCacheDependency` an den / die die Datenbank und die Tabelle. Das ObjectDataSource wird seine Daten entfernen, wenn die zeitbasierte Ablaufzeit erreicht ist oder wenn der Abruf-System fest, dass die Daten in der zugrunde liegenden Datenbank geändert wurde, je nachdem, was zuerst eintritt.


Die GridView in `SqlCacheDependencies.aspx` zeigt Daten aus zwei Tabellen - `Products` und `Categories` (das Produkt s `CategoryName` Feld abgerufen wird, über eine `JOIN` auf `Categories`). Daher wir zwei SQL-Cache-Abhängigkeiten angeben möchten: NorthwindDB:Products; NorthwindDB:Categories.


[![Konfigurieren der ObjectDataSource zur Unterstützung von Caching mithilfe von SQL-Cache-Abhängigkeiten für die Kategorien und Produkte](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Abbildung 9**: Konfigurieren der ObjectDataSource Unterstützung Zwischenspeichern mithilfe von SQL-Cache Abhängigkeiten auf `Products` und `Categories` ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image12.png))


Nach dem Konfigurieren der ObjectDataSource zur Unterstützung von caching, rufen Sie die Seite über einen Browser. Erneut, das Text auswählen-Ereignis ausgelöst wird, sollte auf der ersten Besuch der Seite angezeigt werden, aber erledigt beim paging, sortieren oder durch Klicken auf die Schaltfläche Bearbeiten oder "Abbrechen". Dies ist, da die Daten in den ObjectDataSource-s-Cache geladen werden, gibt es erst er bleibt die `Products` oder `Categories` Tabellen geändert werden, oder die Daten über die GridView aktualisiert werden.

After-paging durch das Raster, und beachten Sie das Fehlen des Ereignisses auswählen angegeben Text, öffnen Sie ein neues Browserfenster und navigieren Sie zu den Grundlagen-Lernprogramm in bearbeiten, einfügen und Löschen im Abschnitt (`~/EditInsertDelete/Basics.aspx`). Aktualisieren Sie den Namen oder den Preis eines Produkts. Prüfen Sie dann aus, das erste Browserfenster eine andere Seite der Daten, Sortieren Sie das Raster zu, oder klicken Sie auf eine Zeile s-Schaltfläche "Bearbeiten". Dieses Mal die auswählen-Ereignis ausgelöst wird, sollte erneut angezeigt, wie die zugrunde liegenden Datenbank, die Daten wurden (siehe Abbildung 10) geändert. Wenn der Text nicht angezeigt wird, warten Sie einige Minuten, und wiederholen Sie den Vorgang. Denken Sie daran, dass Änderungen an der Dienst Abruf überprüft werden die `Products` Tabelle jedes `pollTime` Millisekunden, daher ist es eine Verzögerung zwischen, wenn die zugrunde liegenden Daten aktualisiert wird und wenn die zwischengespeicherten Daten entfernt wird.


[![Ändern der Products-Tabelle entfernt die zwischengespeicherten Produktdaten](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Abbildung 10**: Ändern der Products-Tabelle die Produktdaten zwischengespeichert entfernt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Schritt 6: Programmgesteuertes Arbeiten mit der`SqlCacheDependency`Klasse

Die [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) Lernprogramm erläutert, die Vorteile der Verwendung einer separaten Schicht für Caching in der im Gegensatz zu enge verbinden das Zwischenspeichern, wenn das ObjectDataSource-Architektur. In diesem Lernprogramm erstellten eine `ProductsCL` Klasse veranschaulicht, mit dem Datencache programmgesteuert arbeiten. Um SQL-Cache-Abhängigkeiten in der Caching-Ebene nutzen zu können, verwenden die `SqlCacheDependency` Klasse.

Mit dem Abruf-System eine `SqlCacheDependency` Objekt muss eine bestimmte Datenbank und Tabelle-Paar zugeordnet werden. Der folgende Code erstellt z. B. eine `SqlCacheDependency` -Objekts basierend auf der Northwind-Datenbank s `Products` Tabelle:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Die beiden Eingabeparameter an die `SqlCacheDependency` Konstruktor für s werden die Datenbank- und Tabellennamen bzw. LIKE mit dem ObjectDataSource s `SqlCacheDependency` -Eigenschaft, der Datenbanknamen verwendet, ist identisch mit dem im angegebenen Wert der `name` Attribut des der `<add>` Element im `Web.config`. Der Tabellenname ist der tatsächliche Name der Datenbanktabelle.

Zuordnen einer `SqlCacheDependency` mit einem Element für den Datencache hinzugefügt wird, gehen Sie die `Insert` methodenüberladungen, die eine Abhängigkeit akzeptiert. Der folgende Code fügt *Wert* für den Datencache auf unbestimmte Zeit, ordnet jedoch mit einem `SqlCacheDependency` auf die `Products` Tabelle. Kurz gesagt, *Wert* im Cache verbleiben, bis er aufgrund von arbeitsspeicherbeschränkungen entfernt wird oder die Abruf-System erkannt, die die `Products` Tabelle wurde geändert, da er zwischengespeichert wurde.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

Das Caching Ebene s `ProductsCL` Klasse speichert zurzeit Daten aus der `Products` -Tabelle und verwendet dabei eine zeitbasierte Ablauf von 60 Sekunden. Lassen Sie s, die diese Klasse zu aktualisieren, sodass sie stattdessen SQL-Cache-Abhängigkeiten verwendet. Die `ProductsCL` Klasse s `AddCacheItem` Methode, die für das Hinzufügen von Daten in den Cache verantwortlich ist, enthält zurzeit den folgenden Code:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Aktualisieren Sie diesen Code verwenden eine `SqlCacheDependency` -Objekt anstelle der `MasterCacheKeyArray` cache-Abhängigkeit:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Um diese Funktionalität zu testen, fügen Sie eine GridView zur Seite unterhalb der vorhandenen `ProductsDeclarative` GridView. Legen Sie diese neue GridView s `ID` auf `ProductsProgrammatic` und über seine Smarttag binden Sie es an eine neue ObjectDataSource mit dem Namen `ProductsDataSourceProgrammatic`. Konfigurieren der ObjectDataSource verwenden die `ProductsCL` Klasse, indem die Dropdownlisten in der SELECT und UPDATE-Registerkarten, um `GetProducts` und `UpdateProduct`zugeordnet.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsCL-Klasse](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Abbildung 11**: Konfigurieren der ObjectDataSource verwenden die `ProductsCL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image16.png))


[![Wählen Sie aus der Registerkarte "SELECT" s Dropdown-Liste die GetProducts-Methode](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Abbildung 12**: Wählen Sie die `GetProducts` Methode aus der Registerkarte "Wählen Sie" s Dropdown-Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image18.png))


[![Wählen Sie die UpdateProduct-Methode aus der Registerkarte s Dropdownelement Updateliste](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Abbildung 13**: Wählen Sie die UpdateProduct-Methode aus der Registerkarte "UPDATE"-s-Dropdown-Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-sql-cache-dependencies-vb/_static/image20.png))


Nach Abschluss des Assistenten für die Datenquelle konfigurieren erstellt Visual Studio BoundFields und CheckBoxFields in die GridView für die einzelnen Datenfelder. Wie Sie mit der ersten GridView dieser Seite hinzugefügt wird, entfernen Sie alle Felder jedoch `ProductName`, `CategoryName`, und `UnitPrice`, und formatieren Sie diese Felder nach Bedarf. Aus den GridView s smart Tag die Kontrollkästchen der Paging aktivieren, aktivieren Sie sortieren und Bearbeiten aktivieren. Wie bei der `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio festgelegt werden die `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` Eigenschaft `original_{0}`. Damit die Funktion ordnungsgemäß funktioniert, legen Sie diese Eigenschaft für die GridView s bearbeiten zurück zum `{0}` (oder die Eigenschaft die Zuweisung von deklarativer Syntax vollständig entfernen).

Die resultierende GridView und ObjectDataSource deklarative Markup sollte nach Abschluss dieser Aufgaben können wie folgt aussehen:


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

So testen Sie die SQL-Cacheabhängigkeit in der Ebene zwischenspeichern, legen Sie einen Haltepunkt der `ProductCL` Klasse s `AddCacheItem` -Methode, und klicken Sie dann dem Debuggen beginnen. Wenn Sie zum ersten Mal besuchen `SqlCacheDependencies.aspx`, sollte der Haltepunkt erreicht werden, wie die Daten zum ersten Mal angefordert und im Cache platziert wurden. Als Nächstes zu einer anderen Seite in der GridView verschieben oder eine der Spalten zu sortieren. Dies bewirkt, dass die GridView, um die Daten erneut abzufragen, aber die Daten im Cache seit gefunden werden sollte die `Products` Datenbanktabelle wurde nicht verändert. Wenn die Daten wiederholt nicht im Cache gefunden werden, stellen Sie sicher, dass auf Ihrem Computer über ausreichend Arbeitsspeicher verfügbar ist, und wiederholen Sie den Vorgang.

Nach Paging durch einige Seiten eines GridView, öffnen Sie ein zweites Browserfenster, und navigieren Sie zu den Grundlagen-Lernprogramm in bearbeiten, einfügen und Löschen im Abschnitt (`~/EditInsertDelete/Basics.aspx`). Aktualisiert einen Datensatz aus der Tabelle Products und dann aus dem ersten Browserfenster anzeigen einer neuen Seite, oder klicken Sie auf eine der angegebenen Sortierung.

In diesem Szenario finden Sie zwei Dinge: entweder der Haltepunkt erreicht, gibt an, dass die zwischengespeicherten Daten aufgrund der Änderung in der Datenbank entfernt wurde oder der Haltepunkt wird nicht erreicht, was bedeutet, dass `SqlCacheDependencies.aspx` keine veraltete Daten wird jetzt angezeigt. Wenn der Breakpoint nicht erreicht wird, ist es wahrscheinlich, da der Abruf-Dienst noch nicht ausgelöst wurde, da die Daten geändert wurden. Denken Sie daran, dass Änderungen an der Dienst Abruf überprüft werden die `Products` Tabelle jedes `pollTime` Millisekunden, daher ist es eine Verzögerung zwischen, wenn die zugrunde liegenden Daten aktualisiert wird und wenn die zwischengespeicherten Daten entfernt wird.

> [!NOTE]
> Diese Verzögerung ist wahrscheinlicher angezeigt, wenn eines der Produkte über die GridView in Bearbeitung `SqlCacheDependencies.aspx`. In der [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) Lernprogramm, die wir hinzugefügt der `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit, um sicherzustellen, dass die Daten, die bearbeitet wird, über die `ProductsCL` Klasse s `UpdateProduct` Methode aus dem Cache entfernt wurde. Allerdings ersetzten wir diese Cacheabhängigkeit beim Ändern der `AddCacheItem` Methode weiter oben in diesem Schritt und daher die `ProductsCL` Klasse weiterhin die zwischengespeicherten Daten angezeigt, solange die Abruf-System die Änderung Hinweise der `Products` Tabelle. Wir sehen, wie Sie erneut auf die `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit in Schritt 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Schritt 7: Zuordnen eines zwischengespeicherten Elements mehrere Abhängigkeiten

Bedenken Sie, dass die `MasterCacheKeyArray` Cacheabhängigkeit wird verwendet, um sicherzustellen, dass *alle* produktbezogene Daten aus dem Cache entfernt werden, wenn ein Element verknüpft sind, darin aktualisiert wird. Z. B. die `GetProductsByCategoryID(categoryID)` Methode Caches `ProductsDataTables` Instanzen für jeden eindeutigen *CategoryID* Wert. Wenn eines dieser Objekte entfernt wird, die `MasterCacheKeyArray` Cacheabhängigkeit wird sichergestellt, dass die anderen auch entfernt werden. Ohne diese Cacheabhängigkeit die zwischengespeicherten Daten geändert werden besteht die Möglichkeit, dass andere zwischengespeicherte Produktdaten möglicherweise nicht mehr aktuell. Folglich es wichtig, dass wir verwalten die `MasterCacheKeyArray` Abhängigkeit den Zwischenspeicher bei Verwendung von SQL-Cache-Abhängigkeiten. Die Daten zwischenspeichern jedoch s `Insert` Methode lässt nur für eine einzelnes Dependency-Objekt.

Bei der Arbeit mit SQL-Cache-Abhängigkeiten müssen wir darüber hinaus Verknüpfen mehrerer Datenbanktabellen als Abhängigkeiten. Z. B. die `ProductsDataTable` zwischengespeicherte in der `ProductsCL` Klasse enthält die Kategorie und Lieferanten Namen für jedes Produkt, aber die `AddCacheItem` Methode verwendet nur eine Abhängigkeit auf `Products`. In diesem Fall, wenn der Benutzer den Namen einer Kategorie oder Lieferanten, aktualisiert die zwischengespeicherten Produktdaten bleibt im Cache und veraltet sein. Wir möchten daher den zwischengespeicherten Produktdaten abhängig machen nicht nur die `Products` -Tabelle jedoch in der `Categories` und `Suppliers` sowie Tabellen.

Die [ `AggregateCacheDependency` Klasse](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) bietet eine Möglichkeit für ein Element im Cache mehrere Abhängigkeiten zuordnen. Erstellen Sie zunächst eine `AggregateCacheDependency` Instanz. Als Nächstes fügen Sie die Gruppe der Abhängigkeiten, die mithilfe der `AggregateCacheDependency` s `Add` Methode. Wenn Sie das Element anschließend in der Datencache einfügen, übergeben die `AggregateCacheDependency` Instanz. Wenn *alle* von der `AggregateCacheDependency` Abhängigkeiten s-Instanz zu ändern, wird das zwischengespeicherte Element entfernt werden.

Nachstehend sehen Sie den aktualisierten Code für die `ProductsCL` Klasse s `AddCacheItem` Methode. Erstellt die Methode die `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit zusammen mit `SqlCacheDependency` von Objekten für die `Products`, `Categories`, und `Suppliers` Tabellen. Diese werden alle in einem kombiniert `AggregateCacheDependency` Objekt mit dem Namen `aggregateDependencies`, der übergeben wird dann in der `Insert` Methode.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Diese neuen Code zu testen. Nun Änderungen an der `Products`, `Categories`, oder `Suppliers` Tabellen bewirken, dass die zwischengespeicherten Daten entfernt werden. Darüber hinaus die `ProductsCL` Klasse s `UpdateProduct` -Methode, die aufgerufen wird, wenn ein Produkt nicht über die GridView zu bearbeiten, entfernt die `MasterCacheKeyArray` Zwischenspeichern Abhängigkeit, wodurch die zwischengespeicherten `ProductsDataTable` zur Entfernung und die Daten erneut auf das nächste abgerufen werden sollen Anforderung.

> [!NOTE]
> SQL-Cache-Abhängigkeiten können auch verwendet werden, mit [ausgabezwischenspeicherung](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Eine Demonstration der dieser Funktionalität, finden Sie unter: [mithilfe von ASP.NET Zwischenspeichern der Ausgabe mit SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Zusammenfassung

Beim Zwischenspeichern von Datenbankdaten, verbleiben die Daten im Idealfall im Cache, bis sie in der Datenbank geändert wurden. Mit ASP.NET 2.0 können SQL-Cache-Abhängigkeiten erstellt und in deklarativen und programmgesteuerten Szenarien verwendet werden. Eine der Herausforderungen besteht bei diesem Ansatz wird ermittelt, wenn die Daten geändert wurden. Die Vollversion von Microsoft SQL Server 2005 bieten Benachrichtigungsfunktionen, die eine Anwendung Warnungen erhalten können, wenn sich ein Ergebnis der Abfrage geändert hat. Für die Express Edition von SQL Server 2005 und früheren Versionen von SQL Server muss stattdessen einen Abruf-System verwendet werden. Glücklicherweise ist das Einrichten der erforderlichen Abruf Infrastruktur recht einfach.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Verwenden von Abfragebenachrichtigungen in Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Erstellen einer Abfragebenachrichtigung](https://msdn.microsoft.com/library/ms188669.aspx)
- [Caching in ASP.NET mit der `SqlCacheDependency` Klasse](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Registrierungstool für ASP.NET SQL-Server (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Übersicht über `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Marko Rangel Teresa Murphy und Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](caching-data-at-application-startup-vb.md)
