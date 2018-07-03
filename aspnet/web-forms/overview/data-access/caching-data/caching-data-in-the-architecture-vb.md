---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Zwischenspeichern von Daten in der Architektur (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Im vorherigen Tutorial haben wir wie zwischengespeichert werden auf der Darstellungsschicht. In diesem Tutorial erfahren wir, wie unsere mehrstufigen Architectu nutzen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 7776fb01d31d9f84e57de2d5d899726b52c26d19
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402001"
---
<a name="caching-data-in-the-architecture-vb"></a>Zwischenspeichern von Daten in der Architektur (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) oder [PDF-Datei herunterladen](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> Im vorherigen Tutorial haben wir wie zwischengespeichert werden auf der Darstellungsschicht. In diesem Tutorial erfahren wir, wie unsere mehrstufigen Architektur zum Zwischenspeichern von Daten an die Geschäftslogikschicht nutzen. Dies erfolgt durch Erweitern der Architektur eine Caching-Schicht enthält.


## <a name="introduction"></a>Einführung

Wie wir im vorherigen Tutorial gesehen haben, ist das Zwischenspeichern der "ObjectDataSource"-s-Daten so einfach wie eine Reihe von Eigenschaften festlegen. Leider wendet dem ObjectDataSource-Steuerelement auf der Darstellungsschicht, die Richtlinien für die Zwischenspeicherung eng mit der ASP.NET-Seite koppelt Zwischenspeichern. Einer der Gründe für das Erstellen einer mehrschichtigen Architektur ist, können solche Kopplungen unterbrochen werden. Die Business Logic Layer, entkoppelt die Geschäftslogik von der ASP.NET-Seiten aus aufzurufen, z. B., während der Datenzugriffsschicht der zum Datenzugriff entkoppelt. Dieses Entkoppeln von Business-Logik und Daten Zugriffsdetails wird bevorzugt, Teil, da das System besser lesbar, besser verwaltbar und flexibler zu ändern ist. Außerdem können für domänenwissen und Verteilung der Arbeit, die ein Entwickler die Darstellungsschicht t mit den Datenbank-s-Details vertraut sein, um ihre Arbeit benötigen. Trennen die Richtlinie für die Zwischenspeicherung auf der Darstellungsschicht bietet ähnliche Vorteile.

In diesem Tutorial werden wir zu unserer Architektur sollen Erweitern einer *Caching-Schicht* (oder CL kurz), die unsere Cacherichtlinie verwendet. Der Caching-Schicht umfasst eine `ProductsCL` -Klasse, die Zugriff auf Produktinformationen mit Methoden wie ermöglicht `GetProducts()`, `GetProductsByCategoryID(categoryID)`und so weiter, wenn aufgerufen, beim ersten Versuch zum Abrufen der Daten aus dem Cache wird. Wenn der Cache leer ist, werden diese Methoden der entsprechenden Aufrufen `ProductsBLL` -Methode in der die BLL, die wiederum die DAL die Daten erhalten würde. Die `ProductsCL` Methoden Zwischenspeichern der BLL vor der Rückgabe abgerufenen Daten.

Wie in Abbildung 1 gezeigt, befindet sich die CL zwischen der Präsentations- und Geschäftslogikschichten ein.


![Das Zwischenspeichern von Ebene (CL) wird einer anderen Ebene in unsere-Architektur](caching-data-in-the-architecture-vb/_static/image1.png)

**Abbildung 1**: das Zwischenspeichern von Ebene (CL) wird einer anderen Ebene in unsere-Architektur


## <a name="step-1-creating-the-caching-layer-classes"></a>Schritt 1: Erstellen der Zwischenspeicherung Layer-Klassen

In diesem Tutorial erstellen wir eine sehr einfache CL mit einer einzelnen Klasse `ProductsCL` , bei dem nur eine Handvoll Methoden. Erstellen eine vollständige Caching-Schicht für die gesamte Anwendung erstellen müssten `CategoriesCL`, `EmployeesCL`, und `SuppliersCL` Klassen und eine Methode in diesen Klassen Caching-Schicht für jede Data-Zugriff oder Änderung-Methode, in die BLL bereitgestellt. Im Idealfall sollte der Caching-Schicht als ein separates Klassenbibliotheksprojekt implementiert werden, wie bei den BLL- und DAL. aber wir implementieren sie als Klasse in der `App_Code` Ordner.

Auf Weitere sauber Separate CL-Klassen von der DAL und BLL-Klasse, Let s erstellen Sie einen neuen Unterordner in der `App_Code` Ordner. Mit der rechten Maustaste auf die `App_Code` Ordner im Projektmappen-Explorer, wählen Sie die neuen Ordner, und nennen Sie diesen Ordner `CL`. Nach der Erstellung dieses Ordners, hinzufügen, eine neue Klasse namens `ProductsCL.vb`.


![Fügen Sie einen neuen Ordner namens CL und eine Klasse namens ProductsCL.vb hinzu](caching-data-in-the-architecture-vb/_static/image2.png)

**Abbildung 2**: Fügen Sie einen neuen Ordner namens `CL` und eine Klasse, die mit dem Namen `ProductsCL.vb`


Die `ProductsCL` -Klasse sollte den gleichen Satz von Daten und Methoden, wie in der entsprechenden Business Logic Layer-Klasse enthalten (`ProductsBLL`). Anstatt alle von diesen Methoden können s bauen Sie einfach einige hier um ein Gefühl für die Muster, die von der CL verwendet. Wir fügen insbesondere die `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden in Schritt 3 und ein `UpdateProduct` überladen, die in Schritt 4. Sie können hinzufügen, die verbleibenden `ProductsCL` Methoden und `CategoriesCL`, `EmployeesCL`, und `SuppliersCL` Klassen in aller Ruhe.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Schritt 2: Lesen und Schreiben in Datencache

Dem ObjectDataSource-Steuerelement Zwischenspeicherfeature, die in den vorherigen Tutorials behandelt wurde, intern verwendet das Datencache ASP.NET zum Speichern der Daten, die von der BLL abgerufen. Das Datencache kann auch programmgesteuert aus ASP.NET Seiten CodeBehind- oder von den Klassen in die Architektur der Webanwendung s zugegriffen werden. Verwenden Sie zum Lesen und Schreiben in Datencache aus eine ASP.NET Seite s-Code-Behind-Klasse, die folgende Muster ein:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

Die [ `Cache` Klasse](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` Methode](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) verfügt über eine Reihe von Überladungen. `Cache("key") = value` und `Cache.Insert(key, value)` sind Synonym und sowohl der Cache mit dem angegebenen Schlüssel, ohne eine definierte Ablauf ein Element hinzugefügt. Wir möchten in der Regel eine ablaufangabe angeben, wenn Sie ein Element entweder als eine Abhängigkeit, eine zeitbasierte Ablauf oder beides auf den Cache hinzufügen. Verwenden Sie eine der anderen `Insert` s methodenüberladungen, Abhängigkeit oder zeitbasierte Ablaufinformationen bereitzustellen.

Der Caching-Ebene, die s-Methoden müssen zuerst überprüfen, wenn die angeforderten Daten im Cache befindet, und, wenn dies der Fall ist, wird es von dort aus zurück. Wenn die angeforderten Daten nicht im Cache vorhanden ist, muss die entsprechende BLL-Methode aufgerufen werden. Der Rückgabewert zwischengespeichert und dann zurückgegeben, wie das folgende Sequenzdiagramm veranschaulicht.


![Die Caching-Schicht-s-Methoden Daten aus dem Cache zurückgibt, wenn es s verfügbar](caching-data-in-the-architecture-vb/_static/image3.png)

**Abbildung 3**: die Caching-Schicht s Methoden Daten aus dem Cache zurückgibt, wenn es s verfügbar


Die Sequenz, die in Abbildung 3 dargestellten erfolgt in der CL-Klassen, die mithilfe des folgenden Musters:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Hier *Typ* ist der Typ der im Cache gespeicherten Daten `Northwind.ProductsDataTable`, z. B. *Schlüssel* ist der Schlüssel, der das Element im Cache eindeutig identifiziert. Wenn das Element mit dem angegebenen *Schlüssel* ist nicht im Cache *Instanz* werden `Nothing` und die Daten werden aus der entsprechenden BLL-Methode abgerufen und dem Cache hinzugefügt werden. Mit der Zeit `Return instance` erreicht wird, *Instanz* enthält einen Verweis auf die Daten entweder aus dem Cache oder der BLL abgerufen.

Achten Sie darauf, dass Sie die oben genannten Muster zu verwenden, beim Zugriff auf Daten aus dem Cache. Das folgende Muster, das, auf den ersten Blick entspricht, sucht enthält einen feinen Unterschied, der eine Racebedingung führt. Racebedingungen sind schwierig zu debuggen, da sie sich selbst sporadisch anzuzeigen, und sind schwer zu reproduzieren.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

Der Unterschied in dieser zweiten falschen Codeausschnitt ist, statt einen Verweis auf das zwischengespeicherte Element in einer lokalen Variablen speichern, das dem Datencache erfolgt direkt in die Auswertung der Anweisung *und* in die `Return`. Stellen Sie sich vor, wenn dieser Code erreicht wird, `Cache("key")` nicht `Nothing`, aber vor der `Return` Anweisung erreicht wird, wird das System entfernt *Schlüssel* aus dem Cache. In diesem seltenen Fall, gibt der Code `Nothing` anstatt ein Objekt des erwarteten Typs.

> [!NOTE]
> Das Datencache ist threadsicher, weshalb Sie nicht Thread-Zugang für einfache Lese- oder Schreibvorgänge zu synchronisieren. Allerdings bei Bedarf auf mehrere Vorgänge für Daten im Cache, die atomar sein müssen, sind Sie verantwortlich für die Implementierung einer Sperre oder eines anderen Mechanismus, um Threadsicherheit zu gewährleisten. Finden Sie unter [Synchronisierung auf dem ASP.NET Cache](http://www.ddj.com/184406369) für Weitere Informationen.


Ein Element kann programmgesteuert entfernt werden, aus dem Cache mithilfe der [ `Remove` Methode](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) wie folgt:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Schritt 3: Zurückgeben von Produktinformationen aus der`ProductsCL`Klasse

Für dieses Tutorial implementieren Sie zwei Methoden zur Rückgabe von Produktinformationen aus s kann die `ProductsCL` Klasse: `GetProducts()` und `GetProductsByCategoryID(categoryID)`. Wie beispielsweise die `ProductsBL` Klasse in der Business Logic Layer, die `GetProducts()` -Methode in der CL gibt Informationen zu allen Produkten als eine `Northwind.ProductsDataTable` Objekt während `GetProductsByCategoryID(categoryID)` gibt alle Produkte aus der eine angegebene Kategorie zurück.

Der folgende Code zeigt einen Teil der Methoden in der `ProductsCL` Klasse:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Beachten Sie zunächst die `DataObject` und `DataObjectMethodAttribute` Attribute, die auf die Klasse und die Methoden angewendet. Diese Attribute enthalten Informationen zu "ObjectDataSource"-s-Assistenten, der angibt, welche Klassen und Methoden in den Schritten des Assistenten s angezeigt werden soll. Da die CL-Klassen und Methoden aus einem "ObjectDataSource" in der Darstellungsschicht zugegriffen werden, hinzugefügt habe ich diese Attribute zum Verbessern der Entwurfszeit. Siehe die [Erstellen einer Geschäftslogikebene](../introduction/creating-a-business-logic-layer-vb.md) Tutorial für eine ausführlichere Beschreibung zu diesen Attributen und deren Auswirkungen.

In der `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden, die von zurückgegebenen Daten die `GetCacheItem(key)` -Methode eine lokale Variable zugewiesen wird. Die `GetCacheItem(key)` -Methode, die in Kürze untersucht werden, gibt ein bestimmtes Element aus dem Cache, der auf der Grundlage der angegebenen *Schlüssel*. Wenn keine solchen Daten im Cache gefunden werden, wird er aus der entsprechenden abgerufen `ProductsBLL` -Klassenmethode und dann auf den Cache mit den `AddCacheItem(key, value)` Methode.

Die `GetCacheItem(key)` und `AddCacheItem(key, value)` Methoden Schnittstelle mit dem Datencache, lesen und Schreiben von Werten. Die `GetCacheItem(key)` Methode ist die einfachere der beiden. Wird einfach der Wert aus dem Cache-Klasse, die mit dem übergebenen in *Schlüssel*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` verwendet keine *Schlüssel* -Wert, wie angegeben, sondern ruft die `GetCacheKey(key)` -Methode, die gibt die *Schlüssel* vorangestellt ProductsCache-. Die `MasterCacheKeyArray`, der die Zeichenfolge ProductsCache, enthält auch dient der `AddCacheItem(key, value)` -Methode, sofort sehen.

Von einer ASP.NET Seite s Code-Behind-Klasse, das dem Datencache erfolgen kann mithilfe der `Page` s-Klasse [ `Cache` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx), und ermöglicht eine Syntax wie `Cache("key") = value`, wie in Schritt2 beschrieben. Von einer Klasse innerhalb der Architektur, das dem Datencache erfolgen kann entweder `HttpRuntime.Cache` oder `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)Blogeintrag [HttpRuntime.Cache Visual Studio. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) – Anmerkungen zu dieser die geringfügige Leistungsvorteile mit `HttpRuntime` anstelle von `HttpContext.Current`; daher `ProductsCL` verwendet `HttpRuntime`.

> [!NOTE]
> Wenn Ihre Architektur wird mithilfe von Class Library-Projekten implementiert, und klicken Sie dann müssen Sie zum Hinzufügen eines Verweises auf die `System.Web` Assembly um verwenden die [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) und [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) Klassen.


Wenn das Element nicht im Cache gefunden wurde die `ProductsCL` -s-Klasse, Methoden, die Daten der BLL abzurufen, und fügen Sie es mit dem Cache hinzu der `AddCacheItem(key, value)` Methode. Hinzuzufügende *Wert* in den Cache können wir den folgenden Code, die eine Ablaufzeit von 60 Sekunden Zeit verwendet:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` Gibt die Ablaufzeit eines zeitbasierten 60 Sekunden in der zukünftigen While [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) gibt es s keine gleitende Ablaufzeit. Während dieses `Insert` methodenüberladung verfügt über die Eingabeparameter für beide einen absoluten und gleitenden Ablauf, können Sie nur eine der beiden. Wenn Sie versuchen, die sowohl eine absolute Zeit als auch eine Zeitspanne angeben der `Insert` Methode löst eine `ArgumentException` Ausnahme.

> [!NOTE]
> Diese Implementierung von der `AddCacheItem(key, value)` Methode verfügt derzeit über einige unzulänglichkeiten vorhanden. Wir behandeln und Beheben von folgenden Problemen in Schritt 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Schritt 4: Wenn die Daten im Cache für ungültig zu erklären über die Architektur geändert wird

Zusammen mit Datenabrufmethoden muss der Caching-Schicht bieten dieselben Methoden wie die BLL für das Einfügen, aktualisieren und Löschen von Daten. Die CL s datenänderungsmethoden ändern Sie die zwischengespeicherten Daten, nicht aber eher die BLL s entsprechende Daten-Änderung-Methode aufrufen und dann wird der Cache ungültig. Wie wir im vorherigen Tutorial gesehen haben, ist dies das gleiche Verhalten, die dem ObjectDataSource-Steuerelement angewendet wird, wenn die caching-Funktionen aktiviert sind und die zugehörige `Insert`, `Update`, oder `Delete` Methoden aufgerufen werden.

Die folgenden `UpdateProduct` Überladung wird veranschaulicht, wie die datenänderungsmethoden in der CL zu implementieren:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Die entsprechenden Datenänderung Business Logic Layer-Methode wird aufgerufen, aber bevor die Antwort zurückgegeben wird, müssen wir auf den Cache für ungültig erklären. Leider der Zwischenspeicher zunächst ungültig ist nicht einfach, da die `ProductsCL` s-Klasse `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden jedes Elemente dem Cache hinzufügen mit verschiedenen Schlüsseln, und die `GetProductsByCategoryID(categoryID)` Methode fügt ein anderer Cache-Element für jeden eindeutigen *CategoryID*.

Wenn den Cache für ungültig zu erklären, müssen wir entfernen *alle* der Elemente, die möglicherweise von hinzugefügt wurden die `ProductsCL` Klasse. Dies kann erreicht werden, durch Zuordnen einer *Zwischenspeichern Abhängigkeit* mit dem jedes Element im Cache in der `AddCacheItem(key, value)` Methode. Im Allgemeinen kann eine Cacheabhängigkeit auf einem anderen Element im Cache, eine Datei auf dem Dateisystem oder die Daten aus einer Microsoft SQL Server-Datenbank sein. Wenn die Abhängigkeit ändert oder aus dem Cache entfernt wird, werden automatisch die Cacheelemente, die es zugeordnet ist aus dem Cache entfernt. In diesem Tutorial ein zusätzliches Element im Cache zu erstellen, die dient als über eine Cacheabhängigkeit für alle Elemente hinzugefügt werden sollten die `ProductsCL` Klasse. Alle diese Elemente können auf diese Weise durch die Cacheabhängigkeit entfernen aus dem Cache entfernt werden.

Let-s-Update die `AddCacheItem(key, value)` Methode, sodass jedes Element zum Cache, mit dieser Methode hinzugefügt mit einer einzelnen Cacheabhängigkeit zugeordnet ist:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` ist ein Zeichenfolgenarray, das einen einzelnen Wert, der ProductsCache enthält. Erstens wird ein Cacheelement dem Cache hinzugefügt und zugewiesen werden, das aktuelle Datum und die Uhrzeit. Wenn das Element im Cache bereits vorhanden ist, wird er aktualisiert. Als Nächstes wird eine Cacheabhängigkeit erstellt. Die [ `CacheDependency` Klasse](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s Konstruktor weist eine Reihe von Überladungen, wird hier verwendet, erwartet jedoch, zwei `String` array-Eingaben. Der erste gibt des Satz von Dateien, die als Abhängigkeiten verwendet werden. Da wir Raten t dateibasierten Abhängigkeiten, den Wert verwenden möchten `Nothing` wird für den ersten Eingabeparameter verwendet. Der zweite Eingabeparameter gibt an, den Satz von Cacheschlüsseln, die als Abhängigkeiten verwendet wird. Hier geben wir unsere einzige Abhängigkeit, `MasterCacheKeyArray`. Die `CacheDependency` wird dann übergeben werden, in der `Insert` Methode.

Dank dieser Änderung auf `AddCacheItem(key, value)`, invaliding der Cache ist so einfach wie das Entfernen der Abhängigkeits.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Schritt 5: Aufrufen der Caching-Schicht auf der Darstellungsschicht

Die Caching-Schicht-s-Klassen und Methoden können verwendet werden, mit Daten arbeiten mit den Verfahren wir in diesen Tutorials untersucht haben. Um die Arbeit mit zwischengespeicherten Daten zu veranschaulichen, speichern Sie die Änderungen an der `ProductsCL` Klasse, und öffnen Sie dann die `FromTheArchitecture.aspx` auf der Seite die `Caching` Ordner und Hinzufügen einer GridView-Ansicht. Erstellen Sie von GridView s Smarttags eine neue "ObjectDataSource". Im ersten Schritt Assistenten s sollte die `ProductsCL` Klasse als eine der Optionen aus der Dropdown-Liste.


[![Die ProductsCL-Klasse ist in der Dropdown-Liste Business enthalten.](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Abbildung 4**: die `ProductsCL` Klasse befindet sich in der Dropdown-Liste Business ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-in-the-architecture-vb/_static/image6.png))


Nach der Auswahl `ProductsCL`, klicken Sie auf Weiter. Die Dropdown-Liste in der Registerkarte "SELECT" verfügt über zwei Elemente - `GetProducts()` und `GetProductsByCategoryID(categoryID)` und die Registerkarte "Updates" ist der einzige `UpdateProduct` überladen. Wählen Sie die `GetProducts()` Methode aus der Registerkarte "SELECT" und die `UpdateProducts` Methode aus der Registerkarte "Updates" und klicken Sie auf Fertig stellen.


[![Die s-Klasse für die ProductsCL-Methoden sind in den Dropdownlisten aufgelistet.](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Abbildung 5**: die `ProductsCL` s-Klassenmethoden finden Sie in den Dropdownlisten ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-in-the-architecture-vb/_static/image9.png))


Nach Abschluss des Assistenten wird Visual Studio festgelegt, das "ObjectDataSource"-s `OldValuesParameterFormatString` Eigenschaft `original_{0}` und fügen Sie den entsprechenden Feldern hinzu, an die GridView. Ändern der `OldValuesParameterFormatString` -Eigenschaft wieder auf den Standardwert `{0}`, und konfigurieren die GridView zur Unterstützung von paging, Sortieren und bearbeiten. Da die `UploadProducts` Überladung, die verwendet werden, von dem CL akzeptiert nur die bearbeitete Produktname s und den Preis, beschränken die GridView, sodass nur diese Felder bearbeitet werden.

Im vorherigen Tutorial definiert es einer GridView-Ansicht enthält Felder für die `ProductName`, `CategoryName`, und `UnitPrice` Felder. Können Sie diese Formatierung und die Struktur zu replizieren, die in diesem Fall, dass Ihre deklarativen GridView und "ObjectDataSource" s-Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

An diesem Punkt haben wir eine Seite, die Caching-Schicht verwendet. Um den Cache in Aktion sehen zu können, legen Sie Haltepunkte in der `ProductsCL` Klasse s `GetProducts()` und `UpdateProduct` Methoden. Finden Sie auf der Seite in einen Browser, und den Code schrittweise durchlaufen, die beim Sortieren und paging, um zu sehen, die Daten aus dem Cache abgerufen werden. Klicken Sie dann aktualisieren Sie einen Datensatz zu, und beachten Sie, dass der Cache ungültig ist, daher ist Abruf der BLL, wenn die Daten an die GridView neu gebunden werden.

> [!NOTE]
> Der Caching-Schicht bereitgestellt, die im Download zu diesem Artikel ist nicht abgeschlossen. Sie enthält nur eine Klasse `ProductsCL`, die nur eine Handvoll Methoden glänzt. Darüber hinaus nur eine einzelne ASP.NET-Seite verwendet die CL (`~/Caching/FromTheArchitecture.aspx`) alle anderen verweisen weiterhin die BLL direkt. Wenn Sie eine CL in Ihrer Anwendung verwenden möchten, alle Aufrufe von der Darstellungsschicht sollte zum CL, die die müssten die CL s Klassen und Methoden behandelt, diese Klassen und Methoden in der BLL, die derzeit von der Darstellungsschicht verwendet.


## <a name="summary"></a>Zusammenfassung

Während der Zwischenspeicherung auf dem Darstellungsschicht mit ASP.NET 2.0 s SqlDataSource und ObjectDataSource-Steuerelement angewendet werden kann, würde im Idealfall Zwischenspeichern von Aufgaben an einer separaten Schicht in der Architektur delegiert werden. In diesem Tutorial haben wir eine Caching-Schicht, die zwischen der Darstellungsschicht und den Geschäftslogikebene befindet. Der Caching-Schicht muss bieten den gleichen Satz von Klassen und Methoden, die in die BLL vorhanden sind und auf der Darstellungsschicht aufgerufen werden.

Die Caching-Schicht-Beispiele in diesem und den vorherigen Tutorials behandelt wurde auf *reaktive laden*. Reaktive Loading, werden die Daten in den Cache geladen, nur, wenn eine Anforderung für die Daten erfolgt und die Daten aus dem Cache nicht vorhanden ist. Daten können auch sein *proaktiv geladenen* in den Cache eine Technik, die lädt die Daten in den Cache, bevor sie tatsächlich benötigt wird. Im nächsten Tutorial erfahren Sie, ein Beispiel für proaktive geladen werden, wenn wir betrachten wie statische Werte in den Cache beim Start der Anwendung gespeichert.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](caching-data-with-the-objectdatasource-vb.md)
> [Weiter](caching-data-at-application-startup-vb.md)
