---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Zwischenspeichern von Daten in der Architektur (VB) | Microsoft Docs
author: rick-anderson
description: "Im vorherigen Lernprogramm haben wir gelernt anzuwendende Zwischenspeicherung auf der Präsentationsebene. In diesem Lernprogramm erfahren wir unsere geschichteten Architectu nutzen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: f1d94045236cc8e1b12839ced4de1258466a626e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-the-architecture-vb"></a>Zwischenspeichern von Daten in der Architektur (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) oder [PDF herunterladen](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> Im vorherigen Lernprogramm haben wir gelernt anzuwendende Zwischenspeicherung auf der Präsentationsebene. In diesem Lernprogramm erfahren wir unsere Ebenen-Architektur zum Zwischenspeichern von Daten an die Geschäftslogikschicht nutzen. Dies erfolgt durch Erweitern der Einbeziehung einer Ebene Caching-Architektur.


## <a name="introduction"></a>Einführung

Wie wir im vorherigen Lernprogramm gesehen haben, ist das Zwischenspeichern der ObjectDataSource-s-Daten so einfach wie eine Reihe von Eigenschaften festlegen. Leider gilt das ObjectDataSource Zwischenspeicherung auf der Präsentationsschicht, die Cachingrichtlinien für eng mit der ASP.NET-Seite verbindet. Einer der Gründe für das Erstellen einer geschichteten Architektur ist solche Kupplungen unterbrochen werden können. Der Business Logic Layer werden z. B. die Geschäftslogik von der ASP.NET-Seiten aus aufzurufen, während der Datenzugriffsebene, die Daten Zugriffsdetails entkoppelt. Durch diese Entkopplung von Business-Logik und die Daten Zugriffsdetails wird empfohlen, Teil, da es das System macht besser lesbar sind besser verwaltbar und flexibler zu ändern. Darüber hinaus können für domänenwissen und Verteilung der Arbeit, die ein Entwickler arbeiten, bei dem t der Darstellungsschicht ist nicht mit der Datenbank s vertraut sein, um ihre Arbeitsmöglichkeiten müssen. Die Cachingrichtlinie für von der Darstellungsschicht Entkopplung bietet ähnliche Vorteile.

In diesem Lernprogramm werden wir unsere Architektur einschließen ergänzen eine *Zwischenspeichern Ebene* (oder kurz CL), die unsere Cacherichtlinie verwendet. Der Caching-Ebene enthält eine `ProductsCL` Klasse, die Zugriff auf Produktinformationen mit Methoden wie z. B. eine `GetProducts()`, `GetProductsByCategoryID(categoryID)`und so weiter, dass beim Aufrufen, beim ersten Versuch zum Abrufen der Daten aus dem Cache wird. Wenn der Cache leer ist, werden diese Methoden der entsprechenden Aufrufen `ProductsBLL` Methode in der BLL, die wiederum die Daten von der DAL erhalten würden. Die `ProductsCL` Methoden zwischenspeichern, die vor der Rückgabe aus der BLL abgerufenen Daten.

Wie in Abbildung 1 dargestellt, befindet sich die CL zwischen der Präsentations- und Geschäftsschichten-Logik ein.


![Das Zwischenspeichern der Ebene (CL) ist eine andere Ebene unsere-Architektur](caching-data-in-the-architecture-vb/_static/image1.png)

**Abbildung 1**: das Zwischenspeichern der Ebene (CL) ist eine andere Ebene unsere-Architektur


## <a name="step-1-creating-the-caching-layer-classes"></a>Schritt 1: Erstellen der Klassen für das Zwischenspeichern Ebene

In diesem Lernprogramm erstellen wir eine sehr einfache CL mit einer einzigen Klasse `ProductsCL` , die nur eine Handvoll Methoden besitzt. Erstellen eine vollständige Caching-Ebene aus, für die gesamte Anwendung erstellen müsste `CategoriesCL`, `EmployeesCL`, und `SuppliersCL` Klassen und eine Methode in diesen Klassen Caching-Ebene für jede Datenmethode Zugriff oder Änderung in der BLL bereitgestellt. Im Idealfall sollte der Caching-Ebene als ein separates Klassenbibliotheksprojekt implementiert werden, wie bei der BLL und der DAL ist. Allerdings wird implementieren wir es als Klasse in der `App_Code` Ordner.

Weitere ordnungsgemäß separaten CL-Klassen aus der DAL und BLL-Klasse, Let s erstellen einen neuen Unterordner in den `App_Code` Ordner. Mit der rechten Maustaste auf die `App_Code` Ordner im Projektmappen-Explorer, wählen Sie die neuen Ordner, und nennen Sie diesen Ordner `CL`. Nach diesem Ordner erstellen, hinzufügen, eine neue Klasse namens `ProductsCL.vb`.


![Fügen Sie einen neuen Ordner namens CL und eine Klasse namens ProductsCL.vb hinzu](caching-data-in-the-architecture-vb/_static/image2.png)

**Abbildung 2**: Fügen Sie einen neuen Ordner namens `CL` und eine Klasse namens`ProductsCL.vb`


Die `ProductsCL` -Klasse sollten den gleichen Satz von Daten und Methoden, wie in der entsprechenden Business Logic Layer-Klasse enthalten (`ProductsBLL`). Anstatt alle von diesen Methoden können s nur Build erstellen einige hier, um ein Gefühl für die Muster von CL verwendet. Insbesondere wir fügen die `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden in Schritt 3 und ein `UpdateProduct` überladen, die in Schritt 4. Sie können die verbleibenden hinzufügen `ProductsCL` Methoden und `CategoriesCL`, `EmployeesCL`, und `SuppliersCL` Klassen nach Belieben.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Schritt 2: Lesen und Schreiben in den Datencache

Die Funktion untersucht in dem vorherigen Lernprogramm intern Zwischenspeichern ObjectDataSource verwendet ASP.NET Datencache zum Speichern der Daten aus der BLL abgerufen. Der Datencache kann auch programmgesteuert von ASP.NET Seiten Code-Behind-Klassen oder von den Klassen in der s Webanwendungsarchitektur zugegriffen werden. Zum Lesen und Schreiben für den Datencache von einer ASP.NET Seite s-Code-Behind-Klasse, verwenden Sie Folgendes Muster:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

Die [ `Cache` Klasse](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.aspx) s [ `Insert` Methode](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.insert.aspx) bietet eine Reihe von Überladungen. `Cache("key") = value`und `Cache.Insert(key, value)` sind Synonyme und sowohl der Cache mit dem angegebenen Schlüssel ohne einen definierten Ablauf ein Element hinzuzufügen. In der Regel möchten wir eine Ablaufzeit an, beim Hinzufügen eines Elements zum Cache muss entweder als eine Abhängigkeit, die eine zeitbasierte Ablauf oder beides. Verwenden Sie eine der anderen `Insert` s methodenüberladungen, Abhängigkeit oder zeitbasierte Ablaufinformationen bereitzustellen.

Die Caching-Ebene, die s-Methoden müssen zuerst zu überprüfen, wenn die angeforderten Daten im Cache befindet, und wenn dies der Fall ist, wird es von dort aus zurück. Wenn die angeforderten Daten nicht im Cache vorhanden ist, muss die entsprechende BLL-Methode aufgerufen werden. Der Rückgabewert sollte zwischengespeichert und dann zurückgegeben, wie das folgende Sequenzdiagramm zeigt.


![Die Ebene Caching-s-Methoden Daten aus dem Cache zurückgibt, wenn es s verfügbar](caching-data-in-the-architecture-vb/_static/image3.png)

**Abbildung 3**: Methoden der Caching-Ebene Daten aus dem Cache zurückgibt, wenn es s verfügbar


Die Sequenz, die in den in Abbildung 3 erfolgt in der CL-Klassen, die mit dem folgenden Muster:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Hier *Typ* ist der Typ der Daten im Cache gespeichert werden `Northwind.ProductsDataTable`, z. B. *Schlüssel* ist der Schlüssel, der das Element im Cache eindeutig identifiziert. Wenn das Element mit dem angegebenen *Schlüssel* befindet sich nicht im Cache, klicken Sie dann *Instanz* werden `Nothing` und die Daten werden aus der entsprechenden BLL-Methode abgerufen und dem Cache hinzugefügt. Nach der Zeit `Return instance` erreicht ist, *Instanz* enthält einen Verweis auf die Daten aus dem Cache oder der BLL abgerufen.

Achten Sie darauf, dass Sie die oben genannten Muster verwenden, beim Zugriff auf Daten aus dem Cache. Das folgende Muster, das, auf den ersten Blick entspricht, sucht enthält einen feinen Unterschied, der eine Racebedingung führt. Racebedingungen sind schwer zu debuggen, da sie selbst sporadisch Offenlegen und sind schwer zu reproduzieren.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

Der Unterschied in diesem zweiten, falsche Codeausschnitt ist, statt einen Verweis auf das zwischengespeicherte Element in einer lokalen Variablen speichern, wird der Datencache direkt in die Auswertung der Anweisung zugegriffen *und* in der `Return`. Stellen Sie sich vor, wenn dieser Code erreicht wird, `Cache("key")` nicht `Nothing`, aber vor der `Return` -Anweisung erreicht wird, entfernt das System nach dem *Schlüssel* aus dem Cache. In diesem seltenen Fall, wird der Code gibt `Nothing` anstatt ein Objekt vom erwarteten Typ.

> [!NOTE]
> Der Datencache ist threadsicher, daher brauchen Sie einfache Lese- oder Schreibvorgänge Threadzugriff synchronisieren. Jedoch, wenn Sie mehrere Vorgänge für Daten im Cache, der atomarisch sein müssen, sind Sie verantwortlich für die Implementierung einer Sperre oder eines anderen Mechanismus, um Threadsicherheit zu gewährleisten. Finden Sie unter [synchronisiert den Zugriff auf die ASP.NET-Cache](http://www.ddj.com/184406369) für Weitere Informationen.


Ein Element kann programmgesteuert entfernt werden, aus dem Cache mithilfe der [ `Remove` Methode](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.remove.aspx) wie folgt:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Schritt 3: Zurückgeben von Produktinformationen aus der`ProductsCL`Klasse

Für dieses Lernprogramm s implementiert zwei Methoden für die Rückgabe von Produktinformationen aus kann die `ProductsCL` Klasse: `GetProducts()` und `GetProductsByCategoryID(categoryID)`. Wie mit der `ProductsBL` Klasse in der Logik der Geschäftsebene der `GetProducts()` in der CL Methodenrückgabe Informationen für alle Produkte als eine `Northwind.ProductsDataTable` -Objekt, während `GetProductsByCategoryID(categoryID)` gibt alle Produkte aus einer angegebenen Kategorie.

Der folgende Code zeigt einen Teil der Methoden in der `ProductsCL` Klasse:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Beachten Sie zunächst die `DataObject` und `DataObjectMethodAttribute` Attribute auf die Klassen und Methoden angewendet werden. Diese Attribute enthalten Informationen zu den ObjectDataSource-s-Assistent, der angibt, welche Klassen und Methoden in den Schritten des Assistenten s angezeigt werden soll. Da die CL-Klassen und Methoden aus einem ObjectDataSource in der Darstellungsschicht zugegriffen werden, hinzugefügt ich diese Attribute, um die entwurfszeitumgebung zu verbessern. Verweisen auf die [erstellen eine Geschäftslogikschicht](../introduction/creating-a-business-logic-layer-vb.md) Lernprogramm eine ausführlichere Beschreibung für diese Attribute und deren Auswirkungen.

In der `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden, die vom zurückgegebenen Daten dem `GetCacheItem(key)` Methode eine lokale Variable zugewiesen wird. Die `GetCacheItem(key)` -Methode, die wir in Kürze untersucht werden, gibt ein bestimmtes Element aus dem Cache, die auf der Grundlage der angegebenen *Schlüssel*. Wenn keine solche Daten im Cache gefunden werden, wird Sie aus der entsprechenden abgerufen `ProductsBLL` class-Methode, und klicken Sie dann mit dem Cache hinzugefügt die `AddCacheItem(key, value)` Methode.

Die `GetCacheItem(key)` und `AddCacheItem(key, value)` Methoden Schnittstelle mit dem Datencache, lesen und Schreiben von Werten. Die `GetCacheItem(key)` Methode ist die einfachere der zwei. Einfach den Wert zurück, aus der Cache-Klasse, die mit dem übergebenen in *Schlüssel*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)`verwendet keinen *Schlüssel* Wert wie angegeben, sondern ruft die `GetCacheKey(key)` Methode, die zurückgibt der *Schlüssel* ProductsCache - vorangestellt. Die `MasterCacheKeyArray`, das die Zeichenfolge ProductsCache, hält dient auch durch die `AddCacheItem(key, value)` -Methode, vorübergehend eingehendem.

Von einer ASP.NET Seite "s" Code-Behind-Klasse, kann der Datencache mit zugegriffen werden die `Page` Klasse s [ `Cache` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.page.cache.aspx), und ermöglicht eine Syntax wie `Cache("key") = value`, wie in Schritt2 beschrieben. Von einer Klasse in der Architektur der Datencache möglich, die entweder `HttpRuntime.Cache` oder `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)des Blogeintrag [HttpRuntime.Cache Vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) notes geringfügigen Leistungsvorteil in mit `HttpRuntime` anstelle von `HttpContext.Current`; folglich `ProductsCL` verwendet `HttpRuntime`.

> [!NOTE]
> Wenn Ihre Architektur wird mithilfe von Klassenbibliotheksprojekte implementiert, dann Sie einen Verweis hinzufügen müssen der `System.Web` Assembly um verwenden die [ `HttpRuntime` ](https://msdn.microsoft.com/en-us/library/system.web.httpruntime.aspx) und [ `HttpContext` ](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx) Klassen.


Wenn das Element nicht im Cache gefunden wurde die `ProductsCL` Klassenmethoden s rufen Sie die Daten aus der BLL und hinzuzufügen, den Cache mithilfe der `AddCacheItem(key, value)` Methode. Hinzuzufügende *Wert* in den Cache können wir den folgenden Code, der ein Ablaufdatum 60 Sekunden Zeit verwendet:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)`Gibt die Ablaufzeit zeitbasierte 60 Sekunden in der Zukunft While [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) gibt es s keine Ablaufzeit an. Obwohl dies `Insert` methodenüberladung Eingabeparameter aufweisen, die für beide Absolute und gleitende Ablaufzeit, können Sie nur einen der beiden. Wenn Sie versuchen, geben Sie einen absoluten Zeitpunkt und eine Zeitspanne, die `Insert` Methode löst eine `ArgumentException` Ausnahme.

> [!NOTE]
> Diese Implementierung der `AddCacheItem(key, value)` Methode hat derzeit einige Nachteile. Wir behandeln und beheben diese Probleme in Schritt 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Schritt 4: Wenn die Daten der Cache wird ungültig gemacht ist über die Architektur geändert

Zusammen mit Datenabrufmethoden muss der Caching-Ebene dieselben Methoden wie die BLL zum Einfügen, aktualisieren und Löschen von Daten enthalten. Die CL s Datenmethoden Änderung verändern sich nicht auf die zwischengespeicherten Daten, aber BLL s entsprechende Änderung Datenmethode aufrufen, und klicken Sie dann den Cache ungültig. Wie wir im vorherigen Lernprogramm gesehen haben, ist dies dasselbe Verhalten, das das ObjectDataSource gilt, wenn die caching-Funktionen aktiviert sind und die zugehörige `Insert`, `Update`, oder `Delete` Methoden aufgerufen werden.

Die folgenden `UpdateProduct` Überladung wird veranschaulicht, wie Methoden für die Änderung in der CL implementieren:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Die entsprechenden Datenänderung Business Logic Layer-Methode wird aufgerufen, aber bevor die Antwort zurückgegeben werden müssen wir den Cache für ungültig zu erklären. Leider ungültig ist nicht einfach, da die `ProductsCL` Klasse s `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden jedes Elemente hinzufügen des Caches mit unterschiedlichen Schlüsseln und die `GetProductsByCategoryID(categoryID)` Methode fügt einen anderen Cacheelement, das für jeden eindeutigen *CategoryID*.

Wenn den Cache ungültig macht, müssen wir entfernen *alle* der Elemente, die durch hinzugefügt worden sein könnten die `ProductsCL` Klasse. Dies geschieht durch das Zuordnen einer *Zwischenspeichern Abhängigkeit* mit der jedes Element im Cache in die `AddCacheItem(key, value)` Methode. Im Allgemeinen kann eine Cacheabhängigkeit ein anderes Element im Cache eine Datei auf dem Dateisystem oder Daten aus einer Microsoft SQL Server-Datenbank sein. Wenn die Abhängigkeit ändert oder aus dem Cache entfernt wird, werden automatisch den zugeordneten Cacheelementen aus dem Cache entfernt. Dieses Lernprogramm möchten wir ein weiteres Element im Cache zu erstellen, die dient als über eine Cacheabhängigkeit für alle Elemente hinzugefügt, die `ProductsCL` Klasse. Alle diese Elemente können auf diese Weise durch die Cacheabhängigkeit entfernen aus dem Cache entfernt werden.

Let-s-Update die `AddCacheItem(key, value)` Methode, damit jedes Element dem Cache hinzugefügt, durch diese Methode ist eine einzelne Cacheabhängigkeit zugeordnet:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray`ist ein Array von Zeichenfolgen, die einen einzelnen Wert, ProductsCache enthält. Zuerst wird ein Cacheelement, das dem Cache hinzugefügt und zugewiesen, das aktuelle Datum und die Uhrzeit. Wenn das Cacheelement, das bereits vorhanden ist, wird er aktualisiert. Als Nächstes wird eine Cacheabhängigkeit erstellt. Die [ `CacheDependency` Klasse](https://msdn.microsoft.com/en-US/library/system.web.caching.cachedependency(VS.80).aspx) s Konstruktor verfügt über mehrere Überladungen, jedoch wird hier verwendet erwartet zwei `String` array Eingaben. Die erste Vorlage gibt den Satz von Dateien als Abhängigkeit verwendet werden soll. Da wir Verschlüsselungskennwort t dateibasierten Abhängigkeiten, den Wert verwenden möchten `Nothing` wird für den ersten Eingabeparameter verwendet. Der zweite Eingabeparameter gibt die Menge der Cacheschlüssel, der als Abhängigkeit verwendet. Hier geben wir unsere einzige Abhängigkeit `MasterCacheKeyArray`. Die `CacheDependency` wird dann in übergeben der `Insert` Methode.

Dank dieser Änderung auf `AddCacheItem(key, value)`, invaliding der Cache ist so einfach wie das Entfernen der Abhängigkeitsverhältnis.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Schritt 5: Aufrufen der Caching-Ebene aus der Darstellungsschicht

Die Ebene Caching-s-Klassen und Methoden können mit Daten arbeiten mit den Verfahren wir in diesen Lernprogrammen untersucht Ve verwendet werden. Zum Arbeiten mit zwischengespeicherten Daten zu veranschaulichen, speichern Sie die Änderungen an der `ProductsCL` Klasse, und öffnen Sie dann die `FromTheArchitecture.aspx` auf der Seite der `Caching` Ordner und fügen Sie eine GridView hinzu. Erstellen Sie eine neue ObjectDataSource aus den GridView s smart Tag. Im ersten Schritt Assistenten s sollte die `ProductsCL` Klasse als eine der Optionen aus der Dropdown-Liste.


[![Die ProductsCL-Klasse ist in der Dropdownliste mit der Business-Objekt enthalten.](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Abbildung 4**: die `ProductsCL` -Klasse in der Dropdownliste mit der Business-Objekt enthalten ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-in-the-architecture-vb/_static/image6.png))


Nach der Auswahl `ProductsCL`, klicken Sie auf Weiter. Die Dropdownliste in der Registerkarte "SELECT" verfügt über zwei Elemente - `GetProducts()` und `GetProductsByCategoryID(categoryID)` und die Registerkarte "UPDATE" wurde das einzige `UpdateProduct` überladen. Wählen Sie die `GetProducts()` Methode aus der Registerkarte "SELECT" und die `UpdateProducts` Methode aus der Registerkarte "UPDATE" und klicken Sie auf Fertig stellen.


[![Die ProductsCL Klassenmethoden s sind in der Dropdown-Listen aufgeführt.](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Abbildung 5**: die `ProductsCL` Klassenmethoden s sind in der Dropdown-Listen aufgeführt ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-in-the-architecture-vb/_static/image9.png))


Nach Abschluss des Assistenten wird Visual Studio das ObjectDataSource-s festgelegt `OldValuesParameterFormatString` Eigenschaft `original_{0}` und die entsprechenden Felder an die GridView hinzufügen. Ändern der `OldValuesParameterFormatString` -Eigenschaft zurück auf den Standardwert `{0}`, und konfigurieren Sie die GridView zur Unterstützung von paging, Sortieren und bearbeiten. Da die `UploadProducts` Überladung verwendet, die für die CL akzeptiert nur die bearbeitete Produktname s und Preis, beschränken die GridView, damit nur diese Felder bearbeitet werden.

In dem vorherigen Lernprogramm definiert wir eine GridView, um Felder für die `ProductName`, `CategoryName`, und `UnitPrice` Felder. Wahlweise können diese Formatierung und die Struktur zu replizieren, in diesem Fall, dass die deklarative s GridView und ObjectDataSource-Markup sollte ähnlich der folgenden aussehen:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

Zu diesem Zeitpunkt haben wir eine Seite, die die Caching-Ebene verwendet. Um den Cache in Aktion sehen zu können, legen Sie Haltepunkte in der `ProductsCL` Klasse s `GetProducts()` und `UpdateProduct` Methoden. Besuchen Sie die Seite in einem Browser und den Code schrittweise durchlaufen, wenn die Sortierung und paging, um die Daten anzuzeigen, aus dem Cache abgerufen. Klicken Sie dann aktualisieren Sie einen Datensatz zu, und beachten Sie, dass der Cache für ungültig erklärt und folglich wird Abruf der BLL, wenn die Daten an die GridView gebunden ist.

> [!NOTE]
> Die Caching-Ebene, die gemäß des Downloads zu diesem Artikel ist nicht abgeschlossen. Sie enthält nur eine Klasse `ProductsCL`, die nur eine Handvoll Methoden Sport. Darüber hinaus nur eine ASP.NET-Seite verwendet die CL (`~/Caching/FromTheArchitecture.aspx`) alle anderen weiterhin die BLL verweisen Sie direkt auf. Wenn Sie eine CL in Ihrer Anwendung verwenden möchten, alle Aufrufe von der Darstellungsschicht sollte rufen Sie CL, die dies erfordern würde die CL s-Klassen und Methoden behandelt diese Klassen und Methoden in der BLL aktuell von der Darstellungsschicht verwendet wird.


## <a name="summary"></a>Zusammenfassung

Beim Zwischenspeichern auf dem Darstellungsschicht mit ASP.NET 2.0 s SqlDataSource und ObjectDataSource-Steuerelement angewendet werden kann, würde idealerweise Zwischenspeichern Zuständigkeiten auf einer separaten Schicht in der Architektur delegiert werden. In diesem Lernprogramm haben wir eine Caching-Ebene, die zwischen der Präsentationsschicht und der Business Logic Layer befindet. Die Caching-Ebene muss den gleichen Satz von Klassen und Methoden, die in der BLL vorhanden sein und aus der Darstellungsschicht heißen enthalten.

Die Ebene Caching-Beispiele, die wir in diesem Handbuch und den vorherigen Lernprogrammen untersucht ausgestellt *reaktive laden*. Mit reaktive laden, werden die Daten in den Cache geladen, nur, wenn eine für die Daten Anforderung und die Daten aus dem Cache nicht vorhanden ist. Daten können auch sein *proaktiv geladenen* in den Cache eine Technik, die lädt die Daten in den Cache, bevor sie tatsächlich benötigt wird. In den nächsten Lernprogrammen sehen wir ein Beispiel für die proaktive loading, wenn wir betrachten wie statische Werte in den Cache beim Start der Anwendung gespeichert.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](caching-data-with-the-objectdatasource-vb.md)
[Weiter](caching-data-at-application-startup-vb.md)
