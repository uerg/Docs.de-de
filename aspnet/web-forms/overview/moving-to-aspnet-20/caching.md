---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Zwischenspeichern von | Microsoft-Dokumentation
author: microsoft
description: Ein Überblick über die Zwischenspeicherung ist wichtig für eine gut funktionierende ASP.NET-Anwendung. ASP.NET 1.x angeboten drei verschiedene Optionen für die Zwischenspeicherung. die ausgabezwischenspeicherung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f52a88680db54de6271b17bd52cbdace66425e9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387698"
---
<a name="caching"></a>Zwischenspeicherung
====================
durch [Microsoft](https://github.com/microsoft)

> Ein Überblick über die Zwischenspeicherung ist wichtig für eine gut funktionierende ASP.NET-Anwendung. ASP.NET 1.x angeboten drei verschiedene Optionen für die Zwischenspeicherung. Zwischenspeichern der Ausgabe, Fragment-caching und der Cache-API.


Ein Überblick über die Zwischenspeicherung ist wichtig für eine gut funktionierende ASP.NET-Anwendung. ASP.NET 1.x angeboten drei verschiedene Optionen für die Zwischenspeicherung. Zwischenspeichern der Ausgabe, Fragment-caching und der Cache-API. ASP.NET 2.0 bietet alle drei der folgenden Methoden, aber einige wichtigen zusätzlichen Funktionen hinzugefügt. Es gibt mehrere neue cacheabhängigkeiten und Entwickler haben jetzt die Möglichkeit zum Erstellen von benutzerdefinierten cacheabhängigkeiten. Die Konfiguration des Zwischenspeicherns wurde in ASP.NET 2.0 auch erheblich verbessert.

## <a name="new-features"></a>Neue Funktionen

## <a name="cache-profiles"></a>Cacheprofile

Mit Cacheprofilen können Entwickler bestimmte Cache-Einstellungen definieren, die für einzelne Seiten angewendet werden können. Z. B. Wenn Sie einige Seiten, die aus dem Cache nach 12 Stunden abgelaufen sein sollte verfügen, können Sie problemlos ein Cacheprofil erstellen, die auf diesen Seiten angewendet werden können. Um ein neues Cacheprofil hinzuzufügen, verwenden die &lt;OutputCacheSettings&gt; Abschnitt in der Konfigurationsdatei. Z. B. im folgenden finden Sie die Konfiguration eines Cacheprofils namens *Twoday* , konfiguriert eine Cachedauer von 12 Stunden.

[!code-xml[Main](caching/samples/sample1.xml)]

Verwenden Sie zum Anwenden dieses Cacheprofil auf eine bestimmte Seite das Attribut "Cacheprofile" der @ OutputCache-Direktive, wie unten dargestellt:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Benutzerdefinierten Cacheabhängigkeiten

Entwickler von ASP.NET 1.x und eine Menge für die benutzerdefinierten cacheabhängigkeiten. In ASP.NET 1.x, die CacheDependency-Klasse wurde versiegelt, welche verhindert hat, dass Entwickler ihre eigenen Klassen abgeleitet wird. In ASP.NET 2.0 wird diese Einschränkung entfernt, und Entwickler können ihre eigenen benutzerdefinierten cacheabhängigkeiten zu entwickeln. Die CacheDependency-Klasse ermöglicht die Erstellung einer benutzerdefinierten Cache-Abhängigkeit, die basierend auf Dateien, Verzeichnisse oder Zugriffsschlüssel für den Cache.

Der folgende Code erstellt z. B. eine neue benutzerdefinierte Cacheabhängigkeit basierend auf eine Datei namens stuff.xml befindet sich im Stammverzeichnis der Webanwendung:

[!code-csharp[Main](caching/samples/sample3.cs)]

Wenn die Datei stuff.xml ändert, wird in diesem Szenario wird das zwischengespeicherte Element ungültig.

Es ist auch möglich, eine benutzerdefinierte Cacheabhängigkeit mithilfe der Zugriffsschlüssel für den Cache zu erstellen. Mit dieser Methode wird das Entfernen des Cacheschlüssels die zwischengespeicherten Daten ungültig. Dies wird anhand des folgenden Beispiels veranschaulicht:

[!code-csharp[Main](caching/samples/sample4.cs)]

Um das Element für ungültig zu erklären, das oben eingefügt wurden, entfernen Sie einfach das Element, das in den Cache als der Cacheschlüssel fungieren eingefügt wurde.

[!code-csharp[Main](caching/samples/sample5.cs)]

Beachten Sie, dass der Schlüssel des Elements, das als Cacheschlüssel fungiert in das Array von Cacheschlüsseln hinzugefügten Wert identisch sein muss.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>SQL-Cacheabhängigkeiten Abrufbasierten<em>(auch als tabellenbasierte Abhängigkeiten)</em>

SQL Server 7 und 2000 verwenden das Abrufintervall-basierte Modell für die SQL-cacheabhängigkeiten. Das Abrufintervall-basierte Modell wird ein Trigger verwendet, in einer Datenbanktabelle, die ausgelöst wird, wenn Daten in der Tabelle ändern. Auslösen von Updates, die eine **ChangeId** der Benachrichtigung-Tabelle, die ASP.NET regelmäßig überprüft. Wenn die **ChangeId** Feld wurde aktualisiert, ASP.NET weiß, dass die Daten wurden geändert, und sie macht die zwischengespeicherten Daten ungültig.

> [!NOTE]
> SQL Server 2005 können Sie auch das Abrufintervall-basierte Modell, aber da das Modell abrufbasierten nicht das effizienteste Modell ist, ist es ratsam, eine abfragebasierte-Modell (weiter unten erläutert) mit SQL Server 2005 zu verwenden.


In der Reihenfolge für eine SQL-Cacheabhängigkeit mithilfe des Modells abrufbasierten ordnungsgemäß funktioniert müssen die Tabellen Benachrichtigungen aktiviert haben. Dies kann programmgesteuert mithilfe der Klasse SqlCacheDependencyAdmin erreicht werden oder indem Sie das Aspnet\_regsql.exe-Hilfsprogramm.

Die folgende Befehlszeile registriert die Products-Tabelle in der Northwind-Datenbank befindet sich auf eine SQL Server-Instanz, die mit dem Namen *Dbase* für SQL-cache-Abhängigkeit.

[!code-console[Main](caching/samples/sample6.cmd)]

Im folgenden finden eine Erläuterung der Befehlszeilen-Switches, die im obigen Befehl verwendet:

| **Befehlszeilenschalter** | **Zweck** |
| --- | --- |
| -S *Server* | Gibt den Namen des Servers an. |
| – Ed | Gibt an, dass die Datenbank für SQL-Cacheabhängigkeit aktiviert werden soll. |
| -d *Datenbank\_Name* | Gibt den Datenbanknamen, der für SQL-Cacheabhängigkeit aktiviert werden soll. |
| -E | Gibt an, Aspnet\_Regsql sollten Windows-Authentifizierung verwenden, beim Verbinden mit der Datenbank. |
| -et | Gibt an, dass wir eine Datenbanktabelle für die SQL-Cacheabhängigkeit aktiviert werden. |
| -t *Tabelle\_Name* | Gibt den Namen der Datenbanktabelle an, für die SQL-Cacheabhängigkeit zu aktivieren. |

> [!NOTE]
> Es stehen anderen Schaltern für Aspnet\_regsql.exe. Führen Sie für eine vollständige Liste Aspnet\_regsql.exe-? über die Befehlszeile.


Beim Ausführen dieses Befehls werden die folgenden Änderungen mit der SQL Server-Datenbank vorgenommen:

- Ein **AspNet\_SqlCacheTablesForChangeNotification** -Tabelle hinzugefügt wird. Diese Tabelle enthält eine Zeile für jede Tabelle in der Datenbank, die für die eine SQL-Cacheabhängigkeit aktiviert wurde.
- Die folgenden gespeicherten Prozeduren werden in der Datenbank erstellt:


| AspNet\_SqlCachePollingStoredProcedure | Fragt das AspNet\_SqlCacheTablesForChangeNotification-Tabelle und gibt alle Tabellen, die für die SQL-Cacheabhängigkeit und den Wert der ChangeId für jede Tabelle aktiviert sind. Diese gespeicherte Prozedur wird für das Abrufen verwendet, um festzustellen, ob Daten geändert haben. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Gibt alle Tabellen für SQL-Cacheabhängigkeit aktiviert, indem Sie das AspNet Abfragen\_SqlCacheTablesForChangeNotification-Tabelle und gibt alle Tabellen, die für SQL aktiviert cache-Abhängigkeit. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Eine Tabelle für die SQL-Cacheabhängigkeit durch Hinzufügen von den entsprechenden Eintrag in der Benachrichtigungstabelle registriert, und fügt den Trigger. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Hebt die Registrierung für einer Tabelle für die SQL-Cacheabhängigkeit durch das Entfernen des Eintrags in der Benachrichtigungstabelle aus, und entfernt den Trigger. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualisiert die Benachrichtigungstabelle durch Erhöhen der ChangeId für die geänderte Tabelle. ASP.NET verwendet diesen Wert um zu bestimmen, ob die Daten geändert haben. Wie unten angegeben, wird diese gespeicherte Prozedur ausgeführt, durch den Trigger erstellt, wenn die Tabelle aktiviert ist. |


- Ein SQL Server-Trigger aufgerufen ***Tabelle\_Name *\_AspNet\_SqlCacheNotification\_Trigger** wird für die Tabelle erstellt. Dieser Trigger ausgeführt wird, das AspNet\_SqlCacheUpdateChangeIdStoredProcedure, wenn eine INSERT-, Update- oder DELETE für die Tabelle ausgeführt wird.
- SQL Server-Rolle namens **Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** wird in der Datenbank hinzugefügt.

Die **Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server-Rolle verfügt über EXEC-Berechtigungen für das AspNet\_SqlCachePollingStoredProcedure. In der Reihenfolge für das Modell abrufen ordnungsgemäß funktioniert, müssen Sie Ihr Konto hinzufügen, auf das Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess-Rolle. Das Aspnet\_regsql.exe-Tool wird dies nicht für Sie.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurieren von Abrufbasierten SQL-Cacheabhängigkeiten

Es gibt mehrere Schritte, die zum Konfigurieren der SQL-cacheabhängigkeiten abrufbasierten erforderlich sind. Der erste Schritt ist die Datenbank und die Tabelle, die wie oben beschrieben zu aktivieren. Nachdem dieser Schritt abgeschlossen ist, ist der Rest der Konfiguration wie folgt:

- Konfigurieren der Konfigurationsdatei an.
- Konfigurieren von SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurieren der Konfigurationsdatei

Zusätzlich zum Hinzufügen einer Verbindungszeichenfolge an, wie in einem vorherigen Modul erläutert, müssen Sie auch konfigurieren eine &lt;Cache&gt; -Element mit einem &lt;SqlCacheDependency&gt; -Element wie unten gezeigt:

[!code-xml[Main](caching/samples/sample7.xml)]

Mit dieser Konfiguration können Sie eine SQL-Cacheabhängigkeit für die *Pubs* Datenbank. Beachten Sie, die die PollTime-Attribut in der &lt;SqlCacheDependency&gt; Element standardmäßig zu 60.000 Millisekunden bzw. einer Minute. (Dieser Wert darf nicht kleiner als 500 Millisekunden sein.) In diesem Beispiel die &lt;hinzufügen&gt; Element Fügt eine neue Datenbank und überschreibt die PollTime, wenn diese Option auf 9000000 Millisekunden.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurieren von SqlCacheDependency

Der nächste Schritt ist die SqlCacheDependency konfigurieren. Die einfachste Möglichkeit hierzu ist den Wert für das Attribut "SqlDependency" in der @ Outcache-Anweisung wie folgt angeben:

[!code-aspx[Main](caching/samples/sample8.aspx)]

In der oben genannten @ OutputCache-Direktive ist für eine SQL-Cacheabhängigkeit konfiguriert die *Autoren* -Tabelle in der *Pubs* Datenbank. Mehrere Abhängigkeiten können konfiguriert werden, indem sie mit einem Semikolon getrennt wie folgt:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Eine andere Methode zum Konfigurieren von SqlCacheDependency werden programmgesteuert. Der folgende Code erstellt eine neue SQL-Cacheabhängigkeit für die *Autoren* -Tabelle in der *Pubs* Datenbank.

[!code-csharp[Main](caching/samples/sample10.cs)]

Einer der Vorteile der SQL-Cacheabhängigkeit programmgesteuert zu definieren ist, dass Sie alle Ausnahmen behandeln können, die auftreten können. Angenommen, Sie versuchen, eine SQL-Cacheabhängigkeit für eine Datenbank zu definieren, die nicht für die Benachrichtigung aktiviert wurde eine **DatabaseNotEnabledForNotificationException** Ausnahme ausgelöst. In diesem Fall versuchen Sie, aktivieren Sie die Datenbank für Benachrichtigungen durch Aufrufen der **SqlCacheDependencyAdmin.EnableNotifications** -Methode und übergeben sie den Datenbanknamen.

Ebenso, wenn Sie versuchen, eine SQL-Cacheabhängigkeit für eine Tabelle zu definieren, die nicht für die Benachrichtigung aktiviert wurde eine **TableNotEnabledForNotificationException** ausgelöst. Rufen Sie anschließend die **SqlCacheDependencyAdmin.EnableTableForNotifications** Methode und übergeben sie den Datenbanknamen und Tabellennamen.

Das folgende Codebeispiel veranschaulicht die Behandlung von Ausnahmen ordnungsgemäß zu konfigurieren, wenn Sie eine SQL-Cacheabhängigkeit zu konfigurieren.

[!code-csharp[Main](caching/samples/sample11.cs)]

Weitere Informationen: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Abfragebasierte SQL-Cacheabhängigkeiten (nur SQLServer 2005)

Wenn Sie SQL Server 2005 für SQL-Cacheabhängigkeit zu verwenden, ist das Abrufintervall-basierte Modell nicht erforderlich. Bei Verwendung mit SQL Server 2005, SQL-cacheabhängigkeiten zu kommunizieren direkt über die SQL-Verbindungen mit SQL Server-Instanz (keine weitere Konfiguration ist erforderlich) mit SQL Server 2005-abfragebenachrichtigungen.

Die einfachste Möglichkeit, die abfragebasierte Benachrichtigung aktiviert ist, tun Sie dies deklarativ durch Festlegen der **SqlCacheDependency** Attribut das Datenquellenobjekt **CommandNotification** und Festlegen der **EnableCaching** Attribut **"true"**. Mit dieser Methode ist kein Code erforderlich. Wenn Sie das Ergebnis eines Befehls für die Daten quelländerungen ausgeführt, wird es die Daten im Cache ungültig.

Im folgenden Beispiel wird ein Datenquellen-Steuerelement für die SQL-Cacheabhängigkeit:

[!code-aspx[Main](caching/samples/sample12.aspx)]

In diesem Fall, wenn die Abfrage angegeben, in der **SelectCommand** zurückgibt, die ein anderes Ergebnis, als er ursprünglich war, die Ergebnisse, die zwischengespeichert werden, werden für ungültig erklärt.

Sie können auch angeben, dass alle Datenquellen für SQL-cacheabhängigkeiten aktiviert werden, durch Festlegen der **"SqlDependency"** Attribut der **@ OutputCache** -Direktive **CommandNotification** . Das folgende Beispiel veranschaulicht dies.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Weitere Informationen zu abfragebenachrichtigungen in SQL Server 2005 finden Sie in der SQL Server-Onlinedokumentation.


Eine andere Methode zum Konfigurieren von einer Abfrage basierenden SQL-Cacheabhängigkeit ist dazu programmgesteuert mithilfe der SqlCacheDependency-Klasse. Im folgenden Codebeispiel wird veranschaulicht, wie dies erreicht wird.

[!code-csharp[Main](caching/samples/sample14.cs)]

Weitere Informationen: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Ersetzung nach dem Zwischenspeichern

Zwischenspeichern der Seite kann die Leistung einer Webanwendung erheblich erhöhen. In einigen Fällen benötigen Sie jedoch die meisten der Seite zwischengespeichert werden und einige Fragmenten auf der Seite dynamisch sein. Wenn Sie eine Seite mit Nachrichtenartikel, die für einen festgelegten Zeitraum vollständig statisch ist erstellen, können Sie z. B. die gesamte Seite zwischengespeichert werden festlegen. Wenn Sie ein Drehen von Ad-Banner enthalten, die auf jeder Seitenanforderung geändert beispielsweise, muss der Teil der Seite mit der Ankündigung dynamisch sein. Damit können Sie eine Seite zwischengespeichert, aber einige Inhalte dynamisch zu ersetzen, können Sie die Ersetzung von ASP.NET nach dem Zwischenspeichern verwenden. Mit Ersetzungen nach dem Zwischenspeichern wird die gesamte Seite ausgegeben, die mit bestimmten Teilen, die als von der Zwischenspeicherung ausgenommen zwischengespeichert. Im Beispiel für die Ad-Banner kann das Steuerelement AdRotator Sie Ersetzungen nach dem Zwischenspeichern nutzen, damit Ads dynamisch für jeden Benutzer und für jede seitenaktualisierung erstellt.

Es gibt drei Möglichkeiten zum Implementieren von Ersetzungen nach dem Zwischenspeichern:

- Deklarativ mithilfe der Ersetzung-Steuerelement.
- Programmgesteuert mithilfe der Ersetzung-Steuerungs-API.
- Verwenden implizit AdRotator-Steuerelement.

### <a name="substitution-control"></a>Ersetzungssteuerelement

Die ASP.NET-Substitution-Steuerelements gibt es sich um einen Abschnitt einer zwischengespeicherten Seite, der dynamisch erstellt und nicht als zwischengespeichert. Platzieren Sie ein Ersatz-Steuerelement, an der Position auf der Seite, in dem Sie den dynamischen Inhalt angezeigt werden soll. Zur Laufzeit ruft das Ersatz-Steuerelement eine Methode, die Sie mit der MethodName-Eigenschaft angeben. Die Methode muss eine Zeichenfolge zurückgeben, klicken Sie dann den Inhalt des Steuerelements Ersetzung ersetzt. Die Methode muss es sich um eine statische Methode für das enthaltende Seite oder UserControl-Steuerelement sein. Verwenden des Steuerelements für die Ersetzung bewirkt, dass die clientseitige cachefähigkeit in Server-cachefähigkeit geändert werden, damit die Seite nicht auf dem Client zwischengespeichert wird. Dadurch wird sichergestellt, dass es sich bei zukünftige Anforderungen auf der Seite der Methodenaufruf erneut aus, um dynamischen Inhalt zu generieren.

### <a name="substitution-api"></a>Ersetzung API

Sie können zum programmgesteuerten Erstellen von dynamischem Inhalt für eine zwischengespeicherte Seite Aufrufen der [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) -Methode in Ihrer Seitencode, der den Namen einer Methode als Parameter übergeben. Die Methode, die Erstellung des dynamischen Inhalts behandelt, verwendet einen einzelnen ["HttpContext"](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) Parameter und gibt eine Zeichenfolge zurück. Die zurückgegebene Zeichenfolge ist der Inhalt, der am angegebenen Speicherort ersetzt wird. Ein Vorteil von Aufrufen der Methode WriteSubstitution, anstatt das Ersatz-Steuerelement deklarativ ist, können Sie eine Methode zum Aufrufen einer statischen Methode der Seite oder das UserControl-Objekt, anstatt jedes beliebige Objekt aufrufen.

Aufrufen der Methode WriteSubstitution bewirkt, dass die clientseitige cachefähigkeit in Server-cachefähigkeit geändert werden, damit die Seite nicht auf dem Client zwischengespeichert wird. Dadurch wird sichergestellt, dass es sich bei zukünftige Anforderungen auf der Seite der Methodenaufruf erneut aus, um dynamischen Inhalt zu generieren.

### <a name="adrotator-control"></a>AdRotator-Steuerelement

AdRotator,-Steuerelement implementiert, die Unterstützung intern für Ersetzungen nach dem Zwischenspeichern. Wenn Sie ein AdRotator-Steuerelement auf der Seite platzieren, rendert sie eindeutige Ankündigungen für jede Anforderung – unabhängig davon, ob die übergeordnete Seite zwischengespeichert wird. Daher ist eine Seite, die ein Steuerelement AdRotator enthält nur serverseitig zwischengespeichert.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy-Klasse

Die ControlCachePolicy-Klasse ermöglicht die programmgesteuerte Kontrolle von Fragment-caching mit Benutzersteuerelementen. ASP.NET bettet Benutzersteuerelemente in einer [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) Instanz. Die BasePartialCachingControl-Klasse stellt es sich um ein Benutzersteuerelement, das eine Ausgabe hat Zwischenspeichern aktiviert worden sein.

Beim Zugriff auf die [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) Eigenschaft eine [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) -Steuerelement, erhalten Sie immer ein gültiges ControlCachePolicy-Objekt. Allerdings, wenn Sie Zugriff auf die [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) Eigenschaft eine [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) Steuerelement nur dann, wenn das Benutzersteuerelement von bereits umschlossen wird, erhalten Sie ein gültiges ControlCachePolicy-Objekt ein BasePartialCachingControl-Steuerelement. Wenn es nicht umschlossen ist, wird das ControlCachePolicy-Objekt, das von der Eigenschaft zurückgegebenen Ausnahmen auslösen, wenn Sie versuchen, die sie bearbeiten, da sie nicht über eine zugeordnete BasePartialCachingControl verfügt. Überprüfen, ob eine Instanz eines Benutzersteuerelements das Zwischenspeichern von unterstützt ohne Generierung von Ausnahmen, die [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) Eigenschaft.

Verwenden der ControlCachePolicy-Klasse ist eine von mehreren Möglichkeiten, die Sie die Zwischenspeicherung der Ausgabe aktivieren können. Die folgende Liste beschreibt die Methoden, die Sie zum Aktivieren der ausgabezwischenspeicherung verwenden können:

- Verwenden der [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) Direktive zum Aktivieren des ausgabezwischenspeicherung in deklarativen Szenarios.
- Verwenden der [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) Attribut zum Aktivieren der Zwischenspeicherung für ein Benutzersteuerelement in einer CodeBehind-Datei.
- Verwenden der ControlCachePolicy-Klasse, um die cacheeinstellungen in programmgesteuerte Szenarien angegeben, in dem Sie arbeiten mit BasePartialCachingControl-Instanzen, die dynamisch geladen wird, verwenden die undwurdencacheaktiviertemithilfeeinerdervorherigenMethoden[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) Methode.

Eine Instanz ControlCachePolicy kann nur zwischen den Init "und" PreRender-Phasen des Lebenszyklus des Steuerelements erfolgreich bearbeitet werden. Wenn Sie nach der PreRender Phase ein ControlCachePolicy Objekt ändern, löst ASP.NET eine Ausnahme aus, da alle Änderungen nach das Steuerelement gerendert wird, tatsächlich cacheeinstellungen auswirken können nicht (während der Render-Phase ist ein Steuerelement zwischengespeichert). Schließlich ist eine Instanz eines Benutzersteuerelements (und daher dem zugehörigen ControlCachePolicy-Objekt) nur für die programmgesteuerte Bearbeitung verfügbar, wenn es tatsächlich dargestellt wird.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Änderungen an der Caching-Konfiguration – die &lt;zwischenspeichern&gt; Element

Es gibt mehrere Änderungen in ASP.NET 2.0 Cachekonfiguration. Die &lt;zwischenspeichern&gt; Element ist neu in ASP.NET 2.0 und ermöglicht es Ihnen, die in der Konfigurationsdatei caching Konfiguration ändern. Die folgenden Attribute sind verfügbar.

| **Element** | **Beschreibung** |
| --- | --- |
| **cache** | Optionales Element. Definiert die cacheeinstellungen für globale Anwendungen. |
| **outputCache** | Optionales Element. Gibt eine anwendungsweite Ausgabecache-Einstellungen. |
| **outputCacheSettings** | Optionales Element. Gibt die Ausgabe-Cache-Einstellungen, die auf Seiten in der Anwendung angewendet werden können. |
| **sqlCacheDependency** | Optionales Element. Konfiguriert die SQL-cacheabhängigkeiten für eine ASP.NET-Anwendung. |

### <a name="the-ltcachegt-element"></a>Die &lt;Cache&gt; Element

Die folgenden Attribute stehen zur Verfügung, in der &lt;Cache&gt; Element:

| **Attribut** | **Beschreibung** |
| --- | --- |
| **disableMemoryCollection** | Optionale **booleschen** Attribut. Ruft ab oder legt einen Wert, der angibt, ob die Cachespeichersammlung, die auftritt, wenn der Computer nicht genügend Arbeitsspeicher vorhanden ist, deaktiviert ist. |
| **disableExpiration** | Optionale **booleschen** Attribut. Ruft ab oder legt einen Wert, der angibt, ob die Cacheablaufzeit deaktiviert ist. Wenn deaktiviert, zwischengespeicherte Elemente laufen nicht ab und Hintergrund durch eine Bereinigung von abgelaufenen Cacheelementen erfolgt nicht. |
| **privateBytesLimit** | Optionale **Int64** Attribut. Ruft ab oder legt einen Wert, der angibt, dass die maximale Größe der privaten Bytes einer Anwendung, bevor der Cache abgelaufene Elemente abgelaufen ist und es wird versucht, den Speicher freizugeben. Dieser Grenzwert schließt sowohl vom Cache verwendete Arbeitsspeicher als auch normale Arbeitsspeicher speicherplatzaufwand aus der ausgeführten Anwendung. Eine Einstellung von 0 (null) gibt an, dass ASP.NET eine eigene Heuristik verwendet wird, um festzulegen, wann Speicher wieder freigegeben. |
| **percentagePhysicalMemoryUsedLimit** | Optionale **Int32** Attribut. Ruft ab oder legt einen Wert, der angibt, dass der maximale Prozentsatz des Arbeitsspeichers eines Computers, die von einer Anwendung genutzt werden kann, bevor der Cache abgelaufene Elemente abgelaufen ist und es wird versucht, diese speicherauslastung sowohl vom Cache auch verwendeten Arbeitsspeicher umfasst Arbeitsspeicher freigeben als die normale speicherauslastung der ausgeführten Anwendung. Eine Einstellung von 0 (null) gibt an, dass ASP.NET eine eigene Heuristik verwendet wird, um festzulegen, wann Speicher wieder freigegeben. |
| **privateBytesPollTime** | Optionale **TimeSpan** Attribut. Ruft ab oder legt einen Wert, der angibt, der des Zeitintervalls zwischen den Abfragen für private Bytes-speichernutzung der Anwendung fest. |

### <a name="the-ltoutputcachegt-element"></a>Die &lt;OutputCache&gt; Element

Die folgenden Attribute stehen für die &lt;OutputCache&gt; Element.


|       <strong>Attribut</strong>        |                                                                                                                                                                                                                                                       <strong>Beschreibung</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Optionale <strong>booleschen</strong> Attribut. Aktiviert bzw. deaktiviert den Seitenausgabecache. Wenn deaktiviert, werden keine Seiten unabhängig von den Einstellungen für den programmgesteuerten oder deklarativen zwischengespeichert. Standardwert ist <strong>"true"</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Optionale <strong>booleschen</strong> Attribut. Aktiviert bzw. deaktiviert die Fragment-Anwendungscache. Wenn deaktiviert, werden keine Seiten zwischengespeichert, unabhängig von der [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) Direktive oder Zwischenspeichern verwendete Profil. Enthält eine Cache-Control-Header gibt an, dass upstream-Proxy-Server als auch für Browser-Clients nicht in Cache Seitenausgabe erfolgen soll. Standardwert ist <strong>"false"</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Optionale <strong>booleschen</strong> Attribut. Ruft ab oder legt einen Wert, der angibt, ob die <strong>Cache-Control: Private</strong> -Header vom Ausgabecachemodul standardmäßig gesendet wird. Standardwert ist <strong>"false"</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Optionale <strong>booleschen</strong> Attribut. Aktiviert/deaktiviert das Senden einer Http "<strong>variieren: \</ strong ><em>"-Header in der Antwort. Mit der Standardeinstellung "false", eine "</em>* variieren: \* <strong>"-Header für Seiten im Ausgabecache gesendet wird. Wenn der Vary-Header gesendet wird, ermöglicht verschiedene Versionen zwischengespeichert werden basierend auf den in der Vary-Header angegebenen. Z. B. <em>variieren: Benutzer-Agents</em> speichert verschiedene Versionen einer Seite, die basierend auf den Benutzer-Agent, der die Anforderung ausgeben. Standardwert ist ** "false"</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Die &lt;OutputCacheSettings&gt; Element

Die &lt;OutputCacheSettings&gt; -Element ermöglicht für die Erstellung von Cacheprofilen, die wie oben beschrieben. Das einzige untergeordnete Element für die &lt;OutputCacheSettings&gt; Element ist die &lt;OutputCacheProfiles&gt; -Element zum Cacheprofile konfigurieren.

### <a name="the-ltsqlcachedependencygt-element"></a>Die &lt;SqlCacheDependency&gt; Element

Die folgenden Attribute stehen für die &lt;SqlCacheDependency&gt; Element.

| **Attribut** | **Beschreibung** |
| --- | --- |
| **aktiviert** | Erforderliche **booleschen** Attribut. Gibt an, und zwar unabhängig davon, ob Änderungen abgerufen werden. |
| **pollTime** | Optionale **Int32** Attribut. Legt fest, die Häufigkeit, mit denen SqlCacheDependency fragt die Datenbanktabelle für Änderungen ab. Dieser Wert entspricht der Anzahl der Millisekunden zwischen den einzelnen abrufen. Sie können nicht auf weniger als 500 Millisekunden festgelegt werden. Standardmäßig ist der Wert beträgt 1 Minute. |

### <a name="more-information"></a>Weitere Informationen

Es gibt einige zusätzliche Informationen, die Sie zur Cachekonfiguration bewusst sein sollten.

- Wenn die Begrenzung für Worker Prozess private Bytes nicht festgelegt ist, wird der Cache eine der folgenden Grenzwerte verwenden: 

    - X86 2 GB: 800 MB oder 60 % des physischen RAM, welcher Wert kleiner ist
    - X86 3 GB: 1800 MB oder 60 % des physischen RAM, welcher Wert kleiner ist
    - x 64: 1 TB oder 60 % des physischen RAM welcher Wert kleiner ist
- Wenn sowohl der privaten Arbeitsprozess Bytes beschränkt und &lt;Zwischenspeichern PrivateBytesLimit /&gt; festgelegt ist, wird der Cache wird das Minimum der beiden verwendet werden.
- Genau wie in 1.x, wir Löschen der Einträge im Cache und GC aufgerufen. Sammeln Sie zwei Gründen: 

    - Wir sind sehr nahe bei den Grenzwert für private bytes
    - Der verfügbare Arbeitsspeicher ist nahe bei oder kleiner als 10 %
- Können Sie effektiv trim deaktivieren und Bedingungen mit unzureichendem verfügbaren Arbeitsspeicher durch Festlegen von cache &lt;Zwischenspeichern PercentagePhysicalMemoryUseLimit /&gt; bis 100.
- Im Gegensatz zu 1.x, wird die Aufrufe trim und Sammeln von 2.0 angehalten, wenn die letzte Freispeichersammlung. Erfassen gar nicht private Bytes oder die Größe des verwalteten Heaps durch mehr als 1 % der Arbeitsspeichergrenze beschränkt (Cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Benutzerdefinierten Cacheabhängigkeiten

1. Erstellen Sie eine neue Website.
2. Fügen Sie eine XML-Datei namens cache.xml hinzu, und speichern Sie sie in das Stammverzeichnis der Webanwendung.
3. Fügen Sie den folgenden Code auf der Seite\_Load-Methode im Code-Behind "default.aspx": 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Fügen Sie am Anfang von "default.aspx" in der Quellansicht Folgendes ein: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Durchsuchen Sie "default.aspx". Was mitteilen die Zeit?
6. Aktualisieren Sie den Browser. Was mitteilen die Zeit?
7. Öffnen Sie cache.xml, und fügen Sie den folgenden Code hinzu: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Cache.xml zu speichern.
9. Aktualisieren Sie Ihren Browser ein. Was mitteilen die Zeit?
10. Hier wird erläutert, warum die Zeit aktualisiert, anstatt die zuvor zwischengespeicherten Werte:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Übung 2: Verwenden von Abrufbasierten Cacheabhängigkeiten

Diese Übungseinheit wird das Projekt, das Sie im vorherigen Modul erstellt haben, die können für die Bearbeitung von Daten in der Northwind-Datenbank über ein GridView und DetailsView-Steuerelement, verwendet.

1. Öffnen Sie das Projekt in Visual Studio 2005.
2. Führen Sie das Aspnet\_Regsql-Hilfsprogramm für die Northwind-Datenbank, um die Datenbank und die Products-Tabelle zu ermöglichen. Verwenden Sie den folgenden Befehl aus einer Visual Studio-Eingabeaufforderung ein: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Fügen Sie Ihrer Datei "Web.config" Folgendes ein: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Fügen Sie eine neue Webform showdata.aspx aufgerufen.
5. Fügen Sie auf der Seite "showdata.aspx" Folgendes @ Outputcache-Direktive hinzu: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Fügen Sie den folgenden Code auf der Seite\_Last showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Showdata.aspx fügen Sie ein neues SqlDataSource-Steuerelement hinzu und konfigurieren Sie, dass die Verbindung zur Northwind-Datenbank verwendet. Klicken Sie auf Weiter.
8. Aktivieren Sie die Kontrollkästchen, ProductName und "ProductID", und klicken Sie auf Weiter.
9. Klicken Sie auf Fertigstellen.
10. Hinzufügen einer neuen GridView-Ansicht auf der Seite "showdata.aspx".
11. Wählen Sie SqlDataSource1 aus der Dropdownliste aus.
12. Speichern und showdata.aspx durchsuchen. Notieren Sie die angezeigte Zeit.
