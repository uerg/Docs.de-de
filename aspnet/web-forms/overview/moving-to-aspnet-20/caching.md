---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Zwischenspeichern | Microsoft Docs
author: microsoft
description: Ein Überblick über das caching ist wichtig für eine gut funktionierenden ASP.NET-Anwendung. ASP.NET 1.x angeboten drei verschiedene Optionen für das caching; ein Ausgabecaching...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 90faaae75cc85585efa05e6e50eabe8c990d076e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="caching"></a>Zwischenspeicherung
====================
durch [Microsoft](https://github.com/microsoft)

> Ein Überblick über das caching ist wichtig für eine gut funktionierenden ASP.NET-Anwendung. ASP.NET 1.x angeboten drei verschiedene Optionen für das caching; Zwischenspeichern der Ausgabe, Fragment Zwischenspeichern und die Cache-API.


Ein Überblick über das caching ist wichtig für eine gut funktionierenden ASP.NET-Anwendung. ASP.NET 1.x angeboten drei verschiedene Optionen für das caching; Zwischenspeichern der Ausgabe, Fragment Zwischenspeichern und die Cache-API. ASP.NET 2.0 bietet alle drei Methoden, aber einige wichtige zusätzlichen Funktionen hinzugefügt. Mehrere neue Cache Abhängigkeiten vorhanden sind, und Entwickler haben jetzt die Möglichkeit, benutzerdefinierte Cache Abhängigkeiten sowie zu erstellen. Die Konfiguration des Zwischenspeicherns wurde in ASP.NET 2.0 auch erheblich verbessert.

## <a name="new-features"></a>Neue Funktionen

## <a name="cache-profiles"></a>Cacheprofile

Cacheprofile können Entwickler bestimmte cacheeinstellungen definieren, die klicken Sie dann auf einzelne Seiten angewendet werden können. Beispielsweise haben einige Seiten, die aus dem Cache nach 12 Stunden abgelaufen sein sollte, können Sie problemlos ein Cacheprofil erstellen, die auf diesen Seiten angewendet werden können. Verwenden Sie zum Hinzufügen einer neuen Cacheprofil der &lt;OutputCacheSettings&gt; Abschnitt in der Konfigurationsdatei. Im folgenden ist beispielsweise die Konfiguration eines Cacheprofils aufgerufen *Twoday* , konfiguriert eine Cachedauer von 12 Stunden.

[!code-xml[Main](caching/samples/sample1.xml)]

Um eine bestimmte Seite dieses Cacheprofil zuzuweisen, verwenden Sie das Attribut "Cacheprofile" der @ OutputCache-Direktive wie unten dargestellt:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Benutzerdefinierte Cache-Abhängigkeiten

ASP.NET 1.x Entwickler cried out für benutzerdefinierte Cache Abhängigkeiten. In ASP.NET 1.x, die CacheDependency-Klasse wurde versiegelt, welche verhindert hat, dass Entwickler aus ihren eigenen Klassen ableiten. In ASP.NET 2.0 solche Einschränkung entfernt, und Entwickler sind frei, um ihre eigenen benutzerdefinierten Cache Abhängigkeiten zu entwickeln. Die CacheDependency-Klasse ermöglicht die Erstellung einer benutzerdefinierten Cacheabhängigkeit basierend auf Dateien, Verzeichnisse oder Cacheschlüssel.

Der folgende Code erstellt z. B. eine neue benutzerdefinierte Cacheabhängigkeit basierend auf eine Datei namens stuff.xml befindet sich im Stammverzeichnis der Website:

[!code-csharp[Main](caching/samples/sample3.cs)]

Wenn die Datei stuff.xml ändert, wird in diesem Szenario wird das zwischengespeicherte Element ungültig.

Es ist auch möglich, erstellen Sie eine benutzerdefinierte Cacheabhängigkeit Cacheschlüssel zu verwenden. Mit dieser Methode wird das Entfernen des Cacheschlüssels die zwischengespeicherten Daten ungültig. Dies wird anhand des folgenden Beispiels veranschaulicht:

[!code-csharp[Main](caching/samples/sample4.cs)]

Um das Element für ungültig zu erklären, das oben eingefügt wurde, entfernen Sie einfach das Element, das in den Cache fungieren als Cacheschlüssel eingefügt wurde.

[!code-csharp[Main](caching/samples/sample5.cs)]

Beachten Sie, dass der Schlüssel des Elements, das als Cacheschlüssel fungiert, die den Wert an das Array von Cacheschlüsseln hinzugefügt identisch sein muss.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>SQL-Cache-Abhängigkeiten Abruf-basierte<em>(so genannte tabellenbasierte Abhängigkeiten)</em>

SQL Server 7 und 2000 verwenden das Abrufintervall-basierte Modell für SQL-Cache-Abhängigkeiten. Das Abrufintervall-basierte Modell wird ein Trigger verwendet, für eine Datenbanktabelle, die ausgelöst wird, wenn Daten in der Tabelle ändern. Die Updates Auslösen einer **ChangeId** -Feld in der Tabelle für die Benachrichtigung, die ASP.NET regelmäßig überprüft. Wenn die **ChangeId** Feld aktualisiert wurde, ASP.NET weiß, dass die Daten geändert haben und es werden die zwischengespeicherten Daten ungültig ist.

> [!NOTE]
> SQL Server 2005 können auch den Abruf-basiertes Modell verwenden, aber da die Abruf-basiertes Modell nicht die effizienteste Modell ist, ist es ratsam, eine abfragebasierte-Modell (weiter unten erläutert) mit SQL Server 2005 verwenden.


In der Reihenfolge für eine SQL-Cacheabhängigkeit mit dem Abruf-basierte Modell ordnungsgemäß funktioniert benötigen die Tabellen Benachrichtigungen aktiviert. Dies kann programmgesteuert mithilfe der Klasse SqlCacheDependencyAdmin erreicht werden oder indem Sie das Aspnet\_regsql.exe-Hilfsprogramm.

Die folgende Befehlszeile registriert die Products-Tabelle in der Northwind-Datenbank befindet sich auf eine SQL Server-Instanz, die mit dem Namen *Dbase* für SQL-cache-Abhängigkeit.

[!code-console[Main](caching/samples/sample6.cmd)]

Im folgenden finden eine Erläuterung der Befehlszeilenoptionen, die im obigen Befehl verwendet:

| **Befehlszeilenschalter** | **Zweck** |
| --- | --- |
| -S *server* | Gibt den Servernamen an. |
| -Ed | Gibt an, dass die Datenbank für SQL-Cacheabhängigkeit aktiviert werden sollen. |
| -d: *Datenbank\_Name* | Gibt den Namen der Datenbank, der für SQL-Cacheabhängigkeit aktiviert werden sollen. |
| -E | Gibt an, Aspnet\_Regsql sollten Windows-Authentifizierung verwenden, beim Verbinden mit der Datenbank. |
| -et | Gibt an, dass wir eine Datenbanktabelle für SQL-Cacheabhängigkeit aktiviert ist. |
| t - *Tabelle\_Name* | Gibt den Namen der Datenbanktabelle an, für die SQL-Cacheabhängigkeit aktiviert werden. |

> [!NOTE]
> Es stehen anderen Schaltern für Aspnet\_regsql.exe. Führen Sie eine vollständige Liste Aspnet\_regsql.exe-? über die Befehlszeile.


Beim Ausführen von mit diesem Befehl werden die folgenden Änderungen an SQL Server-Datenbank vorgenommen:

- Ein **AspNet\_SqlCacheTablesForChangeNotification** -Tabelle hinzugefügt wird. Diese Tabelle enthält eine Zeile für jede Tabelle in der Datenbank, für die eine SQL-Cacheabhängigkeit aktiviert wurde.
- Die folgenden gespeicherten Prozeduren werden in der Datenbank erstellt werden:


| AspNet\_SqlCachePollingStoredProcedure | Fragt das AspNet\_SqlCacheTablesForChangeNotification-Tabelle und gibt alle Tabellen, die für SQL-Cacheabhängigkeit und der Wert des ChangeId für jede Tabelle aktiviert werden. Diese gespeicherte Prozedur wird für das Abrufen verwendet, um festzustellen, ob Daten geändert haben. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Gibt alle Tabellen durch Abfragen der AspNet für den SQL-Cacheabhängigkeit aktiviert\_SqlCacheTablesForChangeNotification-Tabelle und gibt alle Tabellen, die für SQL aktiviert cache-Abhängigkeit. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Eine Tabelle für die SQL-Cacheabhängigkeit durch Hinzufügen von den entsprechenden Eintrag in der Benachrichtigungstabelle registriert, und fügt den Trigger. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Hebt die Registrierung einer Tabelle für die SQL-Cacheabhängigkeit durch das Entfernen des Eintrags in der Benachrichtigungstabelle, und den Trigger entfernt. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualisiert die Benachrichtigungstabelle durch Erhöhen der ChangeId für die geänderte Tabelle. ASP.NET verwendet diesen Wert, um festzustellen, ob die Daten geändert haben. Wie nachfolgend beschrieben wird, wird diese gespeicherte Prozedur ausgeführt, durch den Trigger erstellt, wenn die Tabelle aktiviert ist. |


- Ein SQL Server-Trigger aufgerufen ***Tabelle\_Namen *\_AspNet\_SqlCacheNotification\_Trigger** für die Tabelle erstellt wird. Dieser Trigger ausgeführt wird, das AspNet\_SqlCacheUpdateChangeIdStoredProcedure, wenn eine INSERT-, Update- oder DELETE für die Tabelle ausgeführt wird.
- SQL Server-Rolle namens **Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** der Datenbank hinzugefügt wird.

Die **Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server-Rolle verfügt über EXEC-Berechtigungen für das AspNet\_SqlCachePollingStoredProcedure. Damit die Abruf-Modell ordnungsgemäß funktioniert, müssen Sie das Aspnet Ihrer Prozesskonto hinzufügen\_ChangeNotification\_ReceiveNotificationsOnlyAccess-Rolle. Das Aspnet\_regsql.exe-Tool wird nicht für Sie.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurieren von Abruf-basierte SQL-Cache-Abhängigkeiten

Es sind mehrere Schritte, die zum Konfigurieren der SQL-Cache-Abhängigkeiten Abruf-basierte erforderlich sind. Der erste Schritt ist so aktivieren Sie die Datenbank und die Tabelle wie oben erläutert. Nachdem dieser Schritt abgeschlossen ist, wird der Rest der Konfiguration wie folgt:

- Konfigurieren die Konfigurationsdatei für ASP.NET.
- Konfigurieren von SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurieren die Konfigurationsdatei für ASP.NET

Zusätzlich zum Hinzufügen einer Verbindungszeichenfolge an, wie in einem vorherigen Modul erläutert, müssen Sie auch Konfigurieren einer &lt;Cache&gt; Element mit einem &lt;SqlCacheDependency&gt; -Element wie unten dargestellt:

[!code-xml[Main](caching/samples/sample7.xml)]

Mit dieser Konfiguration können Sie eine SQL-Cacheabhängigkeit auf die *Pubs* Datenbank. Beachten Sie, die die PollTime Attribut in der &lt;SqlCacheDependency&gt; Element standardmäßig zu 60.000 Millisekunden oder 1 Minute. (Dieser Wert darf nicht kleiner als 500 Millisekunden sein.) In diesem Beispiel wird die &lt;hinzufügen&gt; -Element fügt eine neue Datenbank und überschreibt die PollTime 9000000 Millisekunden festgelegt.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurieren von SqlCacheDependency

Der nächste Schritt besteht SqlCacheDependency konfigurieren. Die einfachste Möglichkeit hierzu ist den Wert für das Attribut "SqlDependency" in der @ Outcache-Direktive wie folgt angeben:

[!code-aspx[Main](caching/samples/sample8.aspx)]

In der oben genannten @ OutputCache-Direktive ist für eine SQL-Cacheabhängigkeit konfiguriert die *Autoren* -Tabelle in der *Pubs* Datenbank. Mehrere Abhängigkeiten können konfiguriert werden, indem sie mit einem Semikolon getrennt wie folgt:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Eine andere Methode zum Konfigurieren von SqlCacheDependency ist programmgesteuert möglich. Der folgende Code erstellt eine neue SQL-Cacheabhängigkeit auf die *Autoren* -Tabelle in der *Pubs* Datenbank.

[!code-csharp[Main](caching/samples/sample10.cs)]

Einer der Vorteile programmgesteuert definieren, die SQL-Cacheabhängigkeit wird, dass Sie keine Ausnahmen behandeln können, die auftreten können. Angenommen, Sie versuchen, eine SQL-Cacheabhängigkeit für eine Datenbank definieren, die nicht für die Benachrichtigung aktiviert wurde eine **DatabaseNotEnabledForNotificationException** Ausnahme wird ausgelöst. In diesem Fall können Sie versuchen, aktivieren die Datenbank für Benachrichtigungen durch Aufrufen der **SqlCacheDependencyAdmin.EnableNotifications** -Methode und übergeben sie den Datenbanknamen.

Ebenso, wenn Sie versuchen, eine SQL-Cacheabhängigkeit für eine Tabelle definieren, die nicht für die Benachrichtigung aktiviert wurde eine **TableNotEnabledForNotificationException** ausgelöst. Rufen Sie dann die **SqlCacheDependencyAdmin.EnableTableForNotifications** Methode und übergeben sie den Datenbanknamen und den Tabellennamen.

Im folgenden Codebeispiel veranschaulicht die Behandlung von Ausnahmen ordnungsgemäß zu konfigurieren, wenn Sie eine SQL-Cacheabhängigkeit konfigurieren.

[!code-csharp[Main](caching/samples/sample11.cs)]

Weitere Informationen: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Abfragebasierte SQL-Cache-Abhängigkeiten (nur SQLServer 2005)

Bei Verwendung der SQL Server 2005 für SQL-Cacheabhängigkeit ist die Abruf-basiertes Modell nicht erforderlich. Bei Verwendung mit SQL Server 2005, SQL-Cache-Abhängigkeiten kommunizieren, direkt über die SQL-Verbindungen mit SQL Server-Instanz (keine weitere Konfiguration ist erforderlich) mit SQL Server 2005-abfragebenachrichtigungen.

Die einfachste Methode zum Abfragen basierende Benachrichtigungen aktiviert ist, soll dies deklarativ durch Festlegen der **SqlCacheDependency** Attribut von dem Datenquellenobjekt **CommandNotification** und zum Festlegen der **EnableCaching** -Attribut **"true"**. Mit dieser Methode ist kein Code erforderlich. Wenn das Ergebnis eines Befehls für die Daten datenquellenänderungen ausgeführt, wird es der Cachedaten ungültig.

Im folgende Beispiel wird ein Datenquellen-Steuerelement für SQL-Cacheabhängigkeit konfiguriert:

[!code-aspx[Main](caching/samples/sample12.aspx)]

In diesem Fall, wenn in die Abfrage angegeben die **SelectCommand** gibt ein anderes Ergebnis, als er ursprünglich war, die Ergebnisse, die zwischengespeichert werden, werden für ungültig erklärt.

Sie können auch angeben, dass alle Datenquellen für SQL-Cache-Abhängigkeiten aktiviert werden, durch Festlegen der **"SqlDependency"** Attribut des der **@ OutputCache** -Direktive **CommandNotification** . Das folgende Beispiel veranschaulicht dies.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Weitere Informationen zu abfragebenachrichtigungen in SQL Server 2005 finden Sie in der SQL Server-Onlinedokumentation.


Eine andere Methode zum Konfigurieren von einer Abfrage basierenden SQL-Cacheabhängigkeit wird dazu programmgesteuert mithilfe der SqlCacheDependency-Klasse. Im folgenden Codebeispiel wird veranschaulicht, wie dies erreicht wird.

[!code-csharp[Main](caching/samples/sample14.cs)]

Weitere Informationen: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Nach der Zwischenspeicher

Eine Seite Caching kann die Leistung einer Webanwendung erheblich erhöhen. In einigen Fällen benötigen Sie jedoch die meisten der Seite zwischengespeichert werden und einige Fragmente innerhalb der Seite dynamisch zu sein. Wenn Sie eine Seite mit Nachrichten, Geschichten, die für einen festgelegten Zeitraum vollständig statisch ist erstellen, können Sie z. B. die gesamte Seite zwischengespeichert werden festlegen. Wenn Sie möchten das Drehen von Ad-Banner enthalten, die auf jeder Seitenanforderung geändert, muss der Teil der Seite mit der Ankündigung dynamisch sein. Damit können eine Seite zwischenzuspeichern, jedoch manche Inhalte dynamisch zu ersetzen, können Sie ASP.NET nach der Zwischenspeicher verwenden. Nach der Cache-Ersetzung ist die gesamte Seite Ausgabecache mit bestimmten Teilen, die als von der Zwischenspeicherung ausgenommen gekennzeichnet. Im Beispiel für die Ad-Banner können Sie nach der Zwischenspeicher nutzen, sodass Ads dynamisch für jeden Benutzer und für jede Seite-Aktualisierung erstellt AdRotator-Steuerelement.

Es gibt drei Möglichkeiten, um nach der Zwischenspeicher zu implementieren:

- Deklarativ mithilfe der Ersetzung-Steuerelement.
- Programmgesteuert mithilfe der Ersetzung-Steuerungs-API.
- Implizit mithilfe des AdRotator-Steuerelements.

### <a name="substitution-control"></a>Substitution-Steuerelement

Die ASP.NET-Substitution-Steuerelements gibt einen Abschnitt einer zwischengespeicherten Seite, die dynamisch erstellt, sondern wird zwischengespeichert. Platzieren Sie ein Ersatz-Steuerelement an der Position auf der Seite, in dem die dynamische Inhalte angezeigt werden soll. Zur Laufzeit ruft das Ersatz-Steuerelement eine Methode, die Sie mit der MethodName-Eigenschaft angeben. Die Methode muss eine Zeichenfolge zurückgeben, die dann den Inhalt des Steuerelements Ersetzung ersetzt. Die Methode muss es sich um eine statische Methode auf der Seite "oder" UserControl Containersteuerelements sein. Mithilfe des Steuerelements Ersetzung bewirkt, dass die clientseitige Cacheability in Server-Cacheability geändert werden, damit die Seite nicht auf dem Client zwischengespeichert wird. Dadurch wird sichergestellt, dass es sich bei zukünftige Anfragen zur Seite Aufrufen der Methode erneut aus, um dynamische Inhalte zu generieren.

### <a name="substitution-api"></a>Ersetzung API

Um dynamische Inhalte für eine zwischengespeicherte Seite programmgesteuert zu erstellen, rufen Sie die [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) Methode im Seitencode, den Namen einer Methode als Parameter übergeben. Die Methode, die die Erstellung des dynamischen Inhalts behandelt, nimmt einen einzelnen [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) Parameter und gibt eine Zeichenfolge zurück. Die zurückgegebene Zeichenfolge ist der Inhalt, der am angegebenen Speicherort ersetzt wird. Ein Vorteil von Aufrufen der Methode WriteSubstitution deklarativ anstatt das Ersatz-Steuerelement ist, dass Sie eine Methode des Aufrufs einer statischen Methode von der Seite "oder" UserControl-Objekt, anstatt jedes beliebige Objekt aufrufen können.

Aufrufen der Methode WriteSubstitution bewirkt, dass die clientseitige Cacheability in Server-Cacheability geändert werden, damit die Seite nicht auf dem Client zwischengespeichert wird. Dadurch wird sichergestellt, dass es sich bei zukünftige Anfragen zur Seite Aufrufen der Methode erneut aus, um dynamische Inhalte zu generieren.

### <a name="adrotator-control"></a>AdRotator-Steuerelement

AdRotator Webserversteuerelement implementierten Unterstützung nach der Zwischenspeicher intern. Wenn Sie ein AdRotator-Steuerelement auf der Seite ablegen, wird es eindeutige Ankündigungen für jede Anforderung, unabhängig davon, ob die übergeordnete Seite zwischengespeichert wird gerendert. Daher wird eine Seite, die ein Steuerelement AdRotator enthält nur serverseitig zwischengespeichert.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy-Klasse

Die ControlCachePolicy-Klasse ermöglicht die programmgesteuerte Kontrolle des Fragments Zwischenspeichern mithilfe von Benutzersteuerelementen. ASP.NET bettet Benutzersteuerelemente in einem [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) Instanz. Die BasePartialCachingControl-Klasse stellt ein Benutzersteuerelement, das eine Zwischenspeichern aktiviert Ausgabe hat.

Beim Zugriff auf die [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) Eigenschaft von einem [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) -Steuerelement, erhalten Sie immer ein gültiges ControlCachePolicy-Objekt. Jedoch wenn Sie Zugriff auf die [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) Eigenschaft eine [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) Steuerelement nur dann, wenn das Benutzersteuerelement durch bereits umschlossen wird, erhalten Sie ein gültiges ControlCachePolicy-Objekt ein BasePartialCachingControl-Steuerelement. Wenn es nicht umschlossen ist, wird von der Eigenschaft zurückgegebenen Objekts ControlCachePolicy Ausnahmen auslösen, wenn Sie versuchen, es zu bearbeiten, da sie nicht über eine zugeordnete BasePartialCachingControl verfügt. Um zu bestimmen, ob eine Instanz eines Benutzersteuerelements Zwischenspeichern unterstützt ohne Ausnahmen zu generieren, überprüfen Sie die [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) Eigenschaft.

Verwenden der ControlCachePolicy-Klasse ist eine von mehreren Möglichkeiten, die Sie Zwischenspeichern der Ausgabe aktivieren können. Die folgende Liste beschreibt die Methoden beschrieben, die Sie verwenden können, um Ausgabe-caching zu aktivieren:

- Verwenden der [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) Richtlinie aktivieren Ausgabecaching in deklarativen Szenarios.
- Verwenden der [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) Attribut, um das Zwischenspeichern für ein benutzerdefiniertes Steuerelement in einer Code-Behind-Datei aktivieren.
- Verwenden der ControlCachePolicy-Klasse, um die cacheeinstellungen in programmgesteuerten Szenarien angeben, in dem Sie arbeiten mit BasePartialCachingControl-Instanzen, die mit einem der dynamischgeladenundcacheaktiviertemithilfeeinerdervorherigenMethodenwurden[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) Methode.

Eine ControlCachePolicy-Instanz kann nur zwischen den Init PreRender Phasen des Lebenszyklus des Steuerelements erfolgreich bearbeitet werden. Wenn Sie ein Objekt ControlCachePolicy nach der Transformationsphase der PreRender ändern, löst ASP.NET eine Ausnahme aus, da Änderungen vorgenommen, nachdem das Steuerelement gerendert wird tatsächlich cacheeinstellungen beeinflussen können (ein Steuerelement ist in der Phase Render zwischengespeichert). Schließlich ist eine Instanz eines Benutzersteuerelements (und daher seine ControlCachePolicy-Objekt) nur für eine programmgesteuerte Bearbeitung verfügbar, wenn es tatsächlich gerendert wird.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Änderungen an der Caching-Konfiguration – die &lt;zwischenspeichern&gt; Element

Es gibt mehrere Änderungen an die Cachekonfiguration in ASP.NET 2.0. Die &lt;zwischenspeichern&gt; Element ist neu in ASP.NET 2.0 und bietet die Möglichkeit zum Zwischenspeichern konfigurationsänderungen in der Konfigurationsdatei. Die folgenden Attribute stehen zur Verfügung.

| **Element** | **Beschreibung** |
| --- | --- |
| **cache** | Optionales Element. Definiert die globale Anwendungseinstellungen-Cache. |
| **outputCache** | Optionales Element. Gibt eine anwendungsweite Ausgabecache-Einstellungen. |
| **outputCacheSettings** | Optionales Element. Gibt die Ausgabe-Cache-Einstellungen, die auf Seiten in der Anwendung angewendet werden können. |
| **sqlCacheDependency** | Optionales Element. Konfiguriert die SQL-Cache-Abhängigkeiten für eine ASP.NET-Anwendung an. |

### <a name="the-ltcachegt-element"></a>Die &lt;Cache&gt; Element

Die folgenden Attribute stehen zur Verfügung, in der &lt;Cache&gt; Element:

| **Attribut** | **Beschreibung** |
| --- | --- |
| **disableMemoryCollection** | Optionale **booleschen** Attribut. Ruft ab oder legt einen Wert, der angibt, ob die Cache-Speicher-Auflistung, die auftritt, wenn der Computer nicht genügend Arbeitsspeicher vorhanden ist, deaktiviert ist. |
| **disableExpiration** | Optionale **booleschen** Attribut. Ruft ab oder legt einen Wert, der angibt, ob durch den Cacheablauf deaktiviert ist. Wenn deaktiviert, zwischengespeicherte Elemente nicht ablaufen, und im Hintergrund von abgelaufenen Cacheelementen Aufräumvorgang findet nicht statt. |
| **privateBytesLimit** | Optionale **Int64** Attribut. Ruft ab oder legt einen Wert, der angibt, dass die maximale Größe der privaten Bytes einer Anwendung, bevor der Cache Abgelaufene Objekte abgelaufen ist und versucht, Speicherplatz freizugeben. Dieser Grenzwert schließt sowohl vom Cache verwendete Arbeitsspeicher als auch normalen Arbeitsspeicher speicherplatzaufwand aus der laufenden Anwendung. Eine Einstellung von 0 (null) gibt an, dass ASP.NET eine eigene Heuristik verwenden wird, um festzulegen, wann die Freigabe von Arbeitsspeicher starten. |
| **percentagePhysicalMemoryUsedLimit** | Optionale **Int32** Attribut. Ruft ab oder legt einen Wert, der angibt, der der maximale Prozentsatz des Arbeitsspeichers des Computers, die von einer Anwendung genutzt werden kann, bevor der Cache Abgelaufene Objekte abgelaufen ist und versucht, diese speicherauslastung sowohl vom Cache sowie verwendeten Arbeitsspeicher umfasst Arbeitsspeicher freizugeben als die normale speicherauslastung der ausgeführten Anwendung. Eine Einstellung von 0 (null) gibt an, dass ASP.NET eine eigene Heuristik verwenden wird, um festzulegen, wann die Freigabe von Arbeitsspeicher starten. |
| **privateBytesPollTime** | Optionale **TimeSpan** Attribut. Ruft ab oder legt einen Wert, der angibt, der des Zeitintervalls zwischen Abruf für private Bytes-speicherauslastung der Anwendung fest. |

### <a name="the-ltoutputcachegt-element"></a>Die &lt;OutputCache&gt; Element

Die folgenden Attribute stehen für die &lt;OutputCache&gt; Element.


|       <strong>Attribut</strong>        |                                                                                                                                                                                                                                                       <strong>Beschreibung</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Optionale <strong>booleschen</strong> Attribut. Aktiviert bzw. deaktiviert den Seitenausgabecache. Wenn deaktiviert, werden keine Seiten unabhängig von den Einstellungen programmgesteuerten oder deklarative zwischengespeichert. Standardwert ist <strong>"true"</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Optionale <strong>booleschen</strong> Attribut. Aktiviert bzw. deaktiviert die Anwendungscache Fragment. Wenn deaktiviert, werden keine Seiten zwischengespeichert, unabhängig von der [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) Richtlinie oder Zwischenspeichern verwendete Profil. Enthält eine Cache-Control-Header gibt an, dass upstream-Proxy-Server sowie die Browser-Clients nicht zum Cache Seitenausgabe versuchen sollte. Standardwert ist <strong>"false"</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Optionale <strong>booleschen</strong> Attribut. Ruft ab oder legt einen Wert, der angibt, ob die <strong>Cache-Control: Private</strong> Header vom Modul Ausgabe-Cache standardmäßig gesendet wird. Standardwert ist <strong>"false"</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Optionale <strong>booleschen</strong> Attribut. Aktiviert/deaktiviert das Senden einer Http "<strong>variieren: \</ strong ><em>"-Header in der Antwort. Mit der Standardeinstellung "false", eine "</em>* variieren: \* <strong>"-Header für die Seiten im Ausgabecache gesendet. Wenn die Vary-Header gesendet wird, können für verschiedene Versionen, die zwischengespeichert werden, was in den Vary-Header angegeben wird Basis. Beispielsweise <em>variieren: Benutzer-Agents</em> verschiedene Versionen einer Seite auf Grundlage der Benutzer-Agent, von die Anforderung gespeichert wird. Standardwert ist ** "false"</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Die &lt;OutputCacheSettings&gt; Element

Die &lt;OutputCacheSettings&gt; -Element ermöglicht für die Erstellung des Cacheprofile, die wie oben beschrieben. Das einzige untergeordnete Element für die &lt;OutputCacheSettings&gt; Element ist der &lt;OutputCacheProfiles&gt; -Element für Cacheprofile konfigurieren.

### <a name="the-ltsqlcachedependencygt-element"></a>Die &lt;SqlCacheDependency&gt; Element

Die folgenden Attribute stehen für die &lt;SqlCacheDependency&gt; Element.

| **Attribut** | **Beschreibung** |
| --- | --- |
| **enabled** | Erforderliche **booleschen** Attribut. Gibt an, und zwar unabhängig davon, ob Änderungen abgerufen werden. |
| **pollTime** | Optionale **Int32** Attribut. Legt die Häufigkeit, mit der SqlCacheDependency die Datenbanktabelle Änderungen abfragt. Dieser Wert entspricht der Anzahl der Millisekunden zwischen den einzelnen abrufen. Es kann nicht auf weniger als 500 Millisekunden festgelegt werden. Standardwert ist 1 Minute. |

### <a name="more-information"></a>Weitere Informationen

Es gibt einige zusätzliche Informationen, die Sie zur Cachekonfiguration bewusst sein sollten.

- Wenn der Workerprozess-Grenzwert private Bytes nicht festgelegt ist, wird der Cache einen der folgenden Beschränkungen des verwenden: 

    - X86 2 GB: 800 MB oder 60 % des physischen Arbeitsspeichers, welcher Wert kleiner ist.
    - X86 3 GB: 1800 MB oder 60 % des physischen Arbeitsspeichers, welcher Wert kleiner ist.
    - x 64: 1 Terabyte oder 60 % des physischen Arbeitsspeichers welcher Wert kleiner ist.
- Wenn sowohl der privaten Arbeitsprozess Bytes beschränken und &lt;Zwischenspeichern PrivateBytesLimit /&gt; festgelegt ist, wird der Cache wird das Minimum der beiden verwendet werden.
- Genau wie in 1.x, wir Einträge im Cache löschen und Aufrufen von GC. Sammeln von zwei Ursachen haben: 

    - Wir sind sehr nahe bei den Grenzwert für private bytes
    - Der verfügbare Arbeitsspeicher ist nahe bei oder weniger als 10 %
- Können Sie effektiv trim deaktivieren und verfügbaren Speichermangel-cache durch Festlegen von &lt;PercentagePhysicalMemoryUseLimit zwischenspeichern /&gt; bis 100.
- Im Gegensatz zu 1.x, wird die Aufrufe trim und Sammeln von 2.0 angehalten, wenn die letzte GC. Erfassen gar nicht private Bytes oder die Größe des verwalteten Heaps von mehr als 1 % des Grenzwerts Arbeitsspeicher (Cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Die benutzerdefinierte Cache Abhängigkeiten

1. Erstellen einer neuen Website.
2. Hinzufügen einer neuen XML-Datei cache.xml aufgerufen, und speichern Sie sie in das Stammverzeichnis der Webanwendung.
3. Fügen Sie den folgenden Code zur Seite\_Load-Methode im Code-Behind von "default.aspx": 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Fügen Sie am oberen Rand "default.aspx" in der Quellansicht Folgendes ein: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Durchsuchen von "default.aspx". Was sagen die Zeit?
6. Aktualisieren Sie den Browser. Was sagen die Zeit?
7. Öffnen Sie cache.xml, und fügen Sie den folgenden Code hinzu: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Cache.xml zu speichern.
9. Aktualisieren Sie Ihren Browser. Was sagen die Zeit?
10. Erläutern Sie, warum die Zeit aktualisiert, anstatt die zuvor zwischengespeicherte Werte:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Übungseinheit 2: Verwenden von Abruf-basierten Cache Abhängigkeiten

Diese Übung verwendet das Projekt, das Sie in dem vorherigen Modul erstellt, die haben für die Bearbeitung der Daten in der Northwind-Datenbank über ein Steuerelement GridView und DetailsView ermöglicht.

1. Öffnen Sie das Projekt in Visual Studio 2005.
2. Führen Sie das Aspnet\_Regsql Hilfsprogramm in der Datenbank Northwind, um die Datenbank und der Products-Tabelle zu aktivieren. Verwenden Sie den folgenden Befehl aus einer Visual Studio-Eingabeaufforderung ein: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Fügen Sie der Datei "Web.config" Folgendes ein: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Fügen Sie eine neue Webform showdata.aspx aufgerufen.
5. Fügen Sie die folgenden @ Outputcache-Direktive zur Seite "showdata.aspx": 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Fügen Sie den folgenden Code zur Seite\_Laden von showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Showdata.aspx ein neues SqlDataSource-Steuerelement hinzu, und konfigurieren Sie, um eine Verbindung mit der Datenbank Northwind zu verwenden. Klicken Sie auf Weiter.
8. Aktivieren Sie die Kontrollkästchen ProductName und "ProductID", und klicken Sie auf Weiter.
9. Klicken Sie auf Fertigstellen.
10. Fügen Sie eine neue GridView auf der Seite "showdata.aspx" hinzu.
11. Wählen Sie SqlDataSource1 aus der Dropdownliste aus.
12. Speichern und showdata.aspx durchsuchen. Notieren Sie sich die Zeit angezeigt.
