---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Zwischenspeichern von Daten mit der ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Zwischenspeichern kann bedeuten, dass den Unterschied zwischen einer langsamen und schnellen Web-Anwendung. Dieses Lernprogramm ist die erste von vier, die einen ausführlichen Blick auf die Zwischenspeicherung in ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c30dcba7d6ff9849371ef92c84d4ec32aa1dc73d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="caching-data-with-the-objectdatasource-c"></a>Zwischenspeichern von Daten mit der ObjectDataSource (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) oder [PDF herunterladen](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Zwischenspeichern kann bedeuten, dass den Unterschied zwischen einer langsamen und schnellen Web-Anwendung. Dieses Lernprogramm ist die erste von vier, die einen ausführlichen Blick auf die Zwischenspeicherung in ASP.NET. Erfahren Sie die grundlegenden Konzepte von caching und caching in der Darstellungsschicht über das ObjectDataSource-Steuerelement anwenden.


## <a name="introduction"></a>Einführung

In der Informatik *zwischenspeichern* versteht man das dauert, Daten oder Informationen, die aufwendig zu erhalten, und speichern eine Kopie davon an einem Speicherort, der für den Zugriff auf schneller ist. Für datengesteuerte Anwendungen nutzen große und komplexe Abfragen häufig den Großteil der Anwendung s Ausführungszeit an. Solche eine s Anwendungsleistung, dann kann häufig durch Speichern der Ergebnisse teure Datenbankabfragen im Anwendungsspeicher s verbessert werden.

ASP.NET 2.0 bietet eine Vielzahl von Optionen zum Zwischenspeichern. Eine ganze Webseite oder das Benutzersteuerelement s gerendert Markup kann zwischengespeichert werden, über *ausgabezwischenspeicherung*. Das ObjectDataSource und SqlDataSource-Steuerelemente bieten die Cache-Funktionen auch, wodurch ermöglicht Daten an die Steuerungsebene zwischengespeichert werden soll. Und ASP.NET s *Datencache* bietet eine umfangreiche Cache-API, die Entwicklern programmgesteuert Cacheobjekten ermöglicht. In diesem Lernprogramm und den nächsten drei untersuchenden mit der ObjectDataSource s caching-Funktionen als auch für den Datencache. Wir darüber hinaus eine anwendungsweite Daten während des Starts zwischengespeichert und zum zwischengespeicherte Daten durch die Verwendung von SQL-Cache-Abhängigkeiten Lizenztoken aktuell zu halten untersuchen. Diese Lernprogramme sind nicht Zwischenspeichern der Ausgabe untersuchen. Eine ausführliche Beschreibung der in der Ausgabe-caching, finden Sie unter [Ausgabezwischenspeicherung in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Zwischenspeichern kann an einer beliebigen Stelle in der Architektur von der Datenzugriffsebene sich über die Präsentationsebene angewendet werden. In diesem Lernprogramm lernen wir caching in der Darstellungsschicht über das ObjectDataSource-Steuerelement anwenden. In den nächsten Lernprogrammen untersuchenden Zwischenspeichern von Daten an der Geschäftslogikebene.

## <a name="key-caching-concepts"></a>Schlüssel, die Caching-Konzepte

Mit Caching kann eine Anwendung s erheblich verbessert Leistung und Skalierbarkeit von Daten, die aufwendig zu generieren, und speichern eine Kopie des Zertifikats in einem Speicherort, die effizienter zugegriffen werden kann. Da der Cache nur eine Kopie der tatsächlichen, eng zugrunde liegenden Daten enthält, können mehr aktuell sind, oder *veralteter*, sofern die zugrunde liegenden Daten ändert. Um dies zu begegnen, kann ein Entwickler Kriterien, die mit dem das Element im Cache werden hinweisen *entfernt* aus dem Cache, verwenden Sie entweder:

- **Zeitbasierten Kriterien** ein Elements zum Cache für eine Absolute oder eine gleitende Dauer hinzugefügt werden kann. Ein Entwickler kann z. B. eine Dauer von, z. B. 60 Sekunden anzugeben. Das zwischengespeicherte Element wird mit einem absoluten Dauer 60 Sekunden, nachdem er hinzugefügt wurde, Cache, unabhängig davon, wie häufig auf ihn zugegriffen wurde entfernt. Mit einem gleitenden Dauer wird das zwischengespeicherte Element 60 Sekunden nach dem letzten Zugriff entfernt.
- **Abhängigkeit basierende Kriterien** eine Abhängigkeit kann ein Element, wenn dem Cache hinzugefügt zugeordnet werden. Ändert die Abhängigkeit Element s wird es aus dem Cache entfernt. Die Abhängigkeit möglicherweise eine Datei, ein anderes Element im Cache oder eine Kombination aus beidem. ASP.NET 2.0 können auch SQL-Cache-Abhängigkeiten, die ermöglichen Entwicklern das Hinzufügen eines Elements in den Cache und er entfernt wird, wenn die zugrunde liegenden Datenbankdaten geändert haben. Untersuchen wir SQL-Cache-Abhängigkeiten in den kommenden [mithilfe von SQL-Cache-Abhängigkeiten](using-sql-cache-dependencies-cs.md) Lernprogramm.

Unabhängig von der angegebenen Entfernung Kriterien, ein Element im Cache möglicherweise *Anwendungscache gelöscht* , bevor die zeitbasierte oder Abhängigkeit basierenden Kriterien erfüllt. Der Cache seine Kapazität erreicht hat, müssen die vorhandene Elemente entfernt werden, bevor neue hinzugefügt werden können. Daher, wenn Sie programmgesteuert mit zwischengespeicherten Daten es wichtiger s arbeiten, dass Sie immer davon ausgehen, dass die zwischengespeicherten Daten möglicherweise nicht vorhanden. Betrachten wir das Muster verwenden, wenn Daten aus dem Cache in unserem nächsten Lernprogramm programmgesteuert zugreifen *Zwischenspeichern von Daten in der Architektur*.

Caching bietet eine kostengünstige Möglichkeit für zusammendrücken höhere Leistung aus einer Anwendung. Als [Steven Smith](http://aspadvice.com/blogs/ssmith/) Hervorhebung in seinem Artikel [ASP.NET-Caching: Techniken und bewährte Verfahren](https://msdn.microsoft.com/library/aa478965.aspx):

Caching kann eine gute Möglichkeit, die gute genügend Leistung zu erzielen, ohne viel Zeit und eine Analyse sein. Speicher ist billig, also wenn Sie die Leistung abrufen können, Sie durch Zwischenspeichern der Ausgabe für 30 Sekunden anstelle von Ausgaben täglich oder wöchentlich Codes oder der Datenbank optimieren möchten müssen, führen Sie die Cache-Lösung (vorausgesetzt, 30 - Sekunde alte Daten ist in Ordnung) und zu verschieben. Schließlich wird schlechter Entwurf wahrscheinlich Ihnen, den aktuellen Stand damit natürlich Sie versuchen sollten, funktionsfähiger Anwendungen. Wenn Sie nur gut genug Performance heute erwerben müssen, Zwischenspeichern kann jedoch sein [Ansatz ausgezeichnet], kaufen Sie viel Zeit Ihre Anwendung zu einem späteren Zeitpunkt umgestaltet werden, wenn Sie über die Zeit dafür verfügen.

Beim Zwischenspeichern spürbaren leistungsverbesserungen bieten kann, es gilt nicht in allen Situationen, z. B. mit Anwendungen, von denen Daten in Echtzeit, häufig aktualisieren oder sogar in Kürze entsprechen keine veraltete Daten ist, in denen nicht akzeptabel. Aber für die meisten Anwendungen, Zwischenspeichern verwendet werden soll. Weitere Hintergrundinformationen zum Zwischenspeichern in ASP.NET 2.0 finden Sie in der [Leistungsgründen zwischenspeichern](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) Teil der [ASP.NET 2.0-Schnellstart-Lernprogrammen](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Schritt 1: Erstellen, das Zwischenspeichern von Webseiten

Bevor wir unsere Untersuchung des ObjectDataSource s caching-Funktionen beginnen, können Sie s, schalten Sie zuerst einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm und den nächsten drei benötigen. Starten, indem Sie einen neuen Ordner namens `Caching`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Fügen Sie die ASP.NET-Seiten für die Caching-bezogene Lernprogramme](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die Caching-bezogene Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in den `Caching` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Abbildung 2: Fügen Sie der SectionLevelTutorialListing.ascx Benutzersteuerelement zu "default.aspx"](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Abbildung 2**: Abbildung 2: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Abschließend fügen Sie diese Seiten als Einträge an die `Web.sitemap` Datei. Fügen Sie insbesondere das folgende Markup nach dem Arbeiten mit Binärdaten `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält die Elemente jetzt für die caching-Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für die Caching-Lernprogramme](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Abbildung 3**: die Siteübersicht enthält jetzt die Einträge für die Caching-Lernprogramme


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Schritt 2: Anzeigen einer Liste der Produkte in einer Webseite

In diesem Lernprogramm wird untersucht, wie die ObjectDataSource-Steuerelement s integrierte Funktionen zum Zwischenspeichern verwenden. Bevor wir diese Funktionen betrachten können, benötigen wir jedoch zuerst eine Seite mit dem Sie arbeiten. Let s erstellen Sie eine Webseite, die eine GridView zum Auflisten von Produktinformationen abgerufen, indem ein ObjectDataSource von verwendet die `ProductsBLL` Klasse.

Öffnen Sie zunächst die `ObjectDataSource.aspx` auf der Seite der `Caching` Ordner. GridView aus der Toolbox in den Designer ziehen, legen Sie seine `ID` Eigenschaft `Products`, und wählen Sie die Smarttag so binden Sie es an ein neues ObjectDataSource-Steuerelement mit dem Namen `ProductsDataSource`. Konfigurieren der ObjectDataSource zur Bearbeitung der `ProductsBLL` Klasse.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL-Klasse](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Können Sie für diese Seite s eine bearbeitbare GridView zu erstellen, sodass wir prüfen können, was geschieht, wenn das ObjectDataSource zwischengespeicherte Daten über die GridView-s-Schnittstelle geändert werden. Lassen Sie die Dropdownliste in der Registerkarte "SELECT" auf den Standardwert festgelegt `GetProducts()`, aber das ausgewählte Element in der Registerkarte "UPDATE" Ändern der `UpdateProduct` Überladung, die akzeptiert `productName`, `unitPrice`, und `productID` als Eingabeparameter.


[![Festlegen der UPDATE s Dropdown-Liste an die entsprechenden UpdateProduct Überladung](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Abbildung 5**: Legen Sie die Registerkarte "UPDATE"-s-Dropdownliste auf die angemessen `UpdateProduct` überladen ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Schließlich die Dropdownlisten in den INSERT- und DELETE-Registerkarten (None) festgelegt, und klicken Sie auf ' Fertig stellen '. Nach Abschluss der Assistent zum Konfigurieren von Datenquellen, legt Visual Studio das ObjectDataSource-s `OldValuesParameterFormatString` Eigenschaft `original_{0}`. Wie in beschrieben die [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogramm, diese Eigenschaft muss die deklarative Syntax daraus oder wieder auf seinen Standardwert festgelegt werden `{0}`, in der Reihenfolge unserer Update Workflows den Vorgang fortzusetzen Sie, ohne Fehler.

Darüber hinaus fügt nach dem Abschluss des Assistenten für Visual Studio ein Feld an die GridView für jedes Produkt Datenfelder. Entfernen Sie alle außer den `ProductName`, `CategoryName`, und `UnitPrice` BoundFields. Aktualisieren Sie als Nächstes die `HeaderText` Eigenschaften des jeweiligen diese BoundFields Produkt, Kategorie und Preis, bzw. Da die `ProductName` ist ein Pflichtfeld, das BoundField in ein TemplateField konvertieren und einen RequiredFieldValidator zum Hinzufügen der `EditItemTemplate`. Auf ähnliche Weise konvertieren die `UnitPrice` BoundField in ein TemplateField und fügen Sie ein CompareValidator, um sicherzustellen, dass der vom Benutzer eingegebene Wert ist eine gültige Währung Wert s größer als oder gleich 0 (null). Zusätzlich zu diesen Änderungen wünschen, führen Sie die Änderungen Layoutgründen rechts ausrichten der `UnitPrice` Wert oder das Angeben der Formatierung für die `UnitPrice` Text in den schreibgeschützten Modus, und bearbeiten-Schnittstellen.

Stellen Sie die GridView durch Aktivieren des Kontrollkästchens Bearbeiten aktivieren, in das Smarttag für GridView s bearbeitet werden. Überprüfen Sie außerdem das Kontrollkästchen Aktivieren von Paging und Sortieren aktivieren.

> [!NOTE]
> Benötigen Sie eine Überprüfung, wie Sie die Bearbeitung GridView-s-Schnittstelle anpassen? Wenn dies der Fall ist, verweisen Sie zurück auf die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Lernprogramm.


[![Aktivieren Sie GridView-Unterstützung für das Bearbeiten, Sortieren und Paging](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Abbildung 6**: GridView-Unterstützung zum Bearbeiten, Sortieren und Paging aktivieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Nachdem diese GridView-Änderungen vorgenommen wurden, sollte die GridView und ObjectDataSource s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Wie Abbildung 7 zeigt ein Beispiel, listet die bearbeitbare GridView Name, Kategorie und Preis aller Produkte in der Datenbank. Können Sie die Seite "s" Funktionalität Sortieren der Ergebnisse über diese Seite zu testen und Bearbeiten eines Datensatzes.


[![Jedes Produkt s Name, Kategorie und Preis ist in sortierbar, navigierbaren, bearbeitbaren GridView aufgeführt](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Abbildung 7**: jedes Produkt s Name, Kategorie und Preis in sortierbar, navigierbaren, bearbeitbaren GridView aufgeführt ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Schritt 3: Anfordern von Daten wird bei der ObjectDataSource untersuchen

Der `Products` GridView Ruft ab, die Daten für die anzuzeigenden durch Aufrufen der `Select` Methode der `ProductsDataSource` ObjectDataSource. Diese ObjectDataSource erstellt eine Instanz der Business Logic Layer s `ProductsBLL` Klasse und ruft seine `GetProducts()` Methode, die ihrerseits die Datenzugriffsebene s `ProductsTableAdapter` s `GetProducts()` Methode. Die DAL-Methode eine Verbindung mit der Northwind-Datenbank her und gibt die konfigurierte `SELECT` Abfrage. Diese Daten dann an die DAL, die verpackt in zurückgegeben werden ein `NorthwindDataTable`. Der "DataTable"-Objekt wird an die BLL zurückgegeben, das an das ObjectDataSource der Rückgabe an die GridView zurückgegeben. Die GridView erstellt dann eine `GridViewRow` -Objekt für jedes `DataRow` in der "DataTable", und jeder `GridViewRow` wird schließlich gerendert, in der HTML-Code, der an den Client zurückgegeben und im Browser Besucher s angezeigt.

Diese Abfolge von Ereignissen erfolgt jedes Mal, das die GridView an die zugrunde liegenden Daten gebunden werden muss. Wenn die Seite zuerst aufgerufen wird, wenn von einer Seite der Daten in eine andere verschieben, wenn GridView sortieren oder ändern die GridView-s-Daten über den integrierten bearbeiten oder Löschen von Schnittstellen erfolgt. Wenn der Ansichtszustand für GridView s deaktiviert ist, wird die GridView auf jede Postback ebenfalls neu. Die GridView kann auch werden explizit neu auf dessen Daten durch Aufrufen seiner `DataBind()` Methode.

Um vollständig die Häufigkeit schätzen mit der die Daten aus der Datenbank abgerufen wurden, können Sie s, die eine Meldung gibt an, wann die Daten erneut abgerufen werden angezeigt. Fügen Sie eine Bezeichnung Websteuerelement über die GridView mit dem Namen `ODSEvents`. Löschen der `Text` Eigenschaft, und legen seine `EnableViewState` Eigenschaft `false`. Klicken Sie unterhalb der Bezeichnung fügen Sie ein Websteuerelement Schaltfläche hinzu, und legen Sie dessen `Text` Eigenschaft Postback.


[![Fügen Sie eine Bezeichnung und eine Schaltfläche auf der Seite über die GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Abbildung 8**: Fügen Sie eine Bezeichnung und eine Schaltfläche auf der Seite über die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Während des Data Access-Workflows das ObjectDataSource-s `Selecting` Ereignis wird ausgelöst, bevor das zugrunde liegende Objekt erstellt wird und seiner konfigurierten-Methode aufgerufen. Erstellen Sie einen Ereignishandler für dieses Ereignis, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Jedes Mal, wenn das ObjectDataSource-Architektur für Daten, eine Anforderung sendet zeigt die Bezeichnung das Ereignis durch Auswählen des Texts ausgelöst.

Besuchen Sie diese Seite in einem Browser aus. Wenn die Seite zuerst aufgerufen wird, wird das Ereignis durch Auswählen des Texts ausgelöst wird, angezeigt. Klicken Sie auf die Schaltfläche "Postback", und notieren Sie sich, dass der Text wird nicht mehr (vorausgesetzt, dass die GridView s `EnableViewState` -Eigenschaftensatz auf `true`, die Standardeinstellung). Dies liegt daran, beim Postback GridView aus seinen Ansichtszustand rekonstruiert ist und daher verfügt über t aktivieren Sie auf der ObjectDataSource für seine Daten. Sortieren, paging, oder Bearbeiten von Daten, bewirkt jedoch, dass die GridView erneut an die Datenquelle binden, und daher das Ereignis auswählen, das ausgelöst wird Text wird erneut angezeigt.


[![Sobald die GridView an die Datenquelle gebunden ist, wird auswählen-Ereignis ausgelöst wird, angezeigt.](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Abbildung 9**: immer die GridView zu seiner Datenquelle gebunden ist, auswählen-Ereignis ausgelöst wird, wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Klicken Sie auf die Schaltfläche Postback verursacht die GridView aus seinen Ansichtszustand rekonstruiert werden](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Abbildung 10**: Postback Schaltfläche bewirkt, dass die GridView aus seinen Ansichtszustand rekonstruiert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Es mag überflüssig, um Daten in der Datenbank jedes Mal abzurufen, die Daten sortiert oder über ausgelagert werden. Nachdem alle wurde seit wir re Standardnavigation verwenden, das ObjectDataSource alle Datensätze abgerufen, wenn die erste Seite anzeigen. Selbst wenn die GridView sortieren und paging-Unterstützung nicht bereitstellt, müssen die Daten aus der Datenbank jedes Mal abgerufen werden, die die Seite zuerst besucht wird von jedem Benutzer (und auf jedes Postback, wenn der Ansichtszustand deaktiviert ist). Aber die GridView dieselben Daten für alle Benutzer angezeigt wird, diese zusätzlichen datenbankanforderungen überflüssig sind. Warum nicht im cache vom zurückgegebenen Ergebnisse der `GetProducts()` -Methode und Binden der GridView auf die zwischengespeicherten Ergebnisse?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Schritt 4: Zwischenspeichern von Daten mithilfe von "ObjectDataSource"

Indem Sie einfach einige Eigenschaften festlegen, kann das ObjectDataSource für die automatisch die abgerufenen Daten im Datencache ASP.NET cache konfiguriert werden. Die folgende Liste fasst die Cache-bezogenen Eigenschaften der ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) muss festgelegt werden, um `true` zum Zwischenspeichern zu aktivieren. Die Standardeinstellung ist `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) die Zeitdauer in Sekunden, die die Daten zwischengespeichert werden. Der Standard ist 0. Das ObjectDataSource werden nur Daten zwischengespeichert, wenn `EnableCaching` ist `true` und `CacheDuration` auf einen Wert größer als 0 (null) festgelegt ist.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) kann festgelegt werden, um `Absolute` oder `Sliding`. Wenn `Absolute`, das ObjectDataSource speichert ihre abgerufenen Daten nach `CacheDuration` Sekunden; Wenn `Sliding`, die Daten ablaufen, nachdem er für nicht zugegriffen wurde `CacheDuration` Sekunden. Die Standardeinstellung ist `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) verwenden Sie diese Eigenschaft einen vorhandenen Cacheabhängigkeit Cacheeinträge ObjectDataSource s zugeordnet werden soll. Das ObjectDataSource-s-Dateneinträge können vorzeitig aus dem Cache entfernt werden, indem ablaufen lassen die zugehörigen `CacheKeyDependency`. Diese Eigenschaft wird am häufigsten verwendet, um eine SQL-Cacheabhängigkeit ObjectDataSource-s-Cache zuzuordnen, ein Thema wird untersucht, in der Zukunft [mithilfe von SQL-Cache-Abhängigkeiten](using-sql-cache-dependencies-cs.md) Lernprogramm.

S konfigurieren lassen die `ProductsDataSource` ObjectDataSource 30 Sekunden lang auf absoluten Skala seine Daten zwischenspeichern. Legen Sie das ObjectDataSource-s `EnableCaching` Eigenschaft `true` und dessen `CacheDuration` Eigenschaft auf 30. Lassen Sie die `CacheExpirationPolicy` -Eigenschaft auf den Standardwert festgelegt `Absolute`.


[![Konfigurieren der ObjectDataSource zum Zwischenspeichern der zugehörigen Daten 30 Sekunden lang](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Abbildung 11**: Konfigurieren der ObjectDataSource zum Zwischenspeichern der zugehörigen Daten 30 Sekunden ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Speichern Sie die Änderungen zu, und rufen Sie diese Seite in einem Browser. Das Markieren von Text ausgelöste Ereignis wird beim ersten die Seite Besuch angezeigt, wie ursprünglich die Daten nicht im Cache befindet. Jedoch nachfolgenden Postbacks ausgelöst durch Klicken auf die Schaltfläche "Postback", sortieren, paging, oder klicken Sie auf die Schaltfläche Bearbeiten oder "Abbrechen" *nicht* Text, das ausgelöst wird wieder das Ereignis auswählen. Grund hierfür ist die `Selecting` Ereignis nur wird ausgelöst, wenn das ObjectDataSource seine Daten aus der zugrunde liegenden Objekt ruft die `Selecting` Ereignis wird nicht ausgelöst, wenn die Daten aus dem Datencache gezogen werden.

Nach 30 Sekunden werden die Daten aus dem Cache entfernt werden. Die Daten werden auch aus dem Cache entfernt werden, wenn das ObjectDataSource-s `Insert`, `Update`, oder `Delete` Methoden aufgerufen werden. Folglich nach 30 Sekunden vergangen oder die Schaltfläche "Aktualisieren", geklickt wurde sortieren, paging, oder auf die Schaltfläche Bearbeiten oder "Abbrechen" führt dazu, dass das ObjectDataSource die Daten aus der zugrunde liegenden Objekt abrufen, Anzeigen des auswählen-Ereignisses ausgelöst Text bei der `Selecting` -Ereignis ausgelöst. Diese zurückgegebenen Ergebnisse werden wieder für den Datencache platziert.

> [!NOTE]
> Wenn Sie das Markieren von Text ausgelöste Ereignis häufig angezeigt wird, selbst wenn Sie erwarten, das ObjectDataSource dass mit zwischengespeicherten Daten, kann es aufgrund von arbeitsspeicherbeschränkungen sein. Wenn nicht genügend Arbeitsspeicher vorhanden ist, die Daten, die dem Cache hinzugefügt, durch die ObjectDataSource möglicherweise Anwendungscache gelöscht wurde haben. Wenn t ObjectDataSource ist nicht für die Daten oder nur Caches ordnungsgemäß caching werden angezeigt wird die Daten sporadisch, schließen Sie einige Programme, um Arbeitsspeicher freizugeben, und versuchen Sie es erneut.


Abbildung 12 veranschaulicht das ObjectDataSource-s Workflow Zwischenspeichern. Beim Auslösen des Ereignisses auswählen Text auf dem Bildschirm angezeigt wird, da die Daten nicht im Cache wurde und musste aus dem zugrunde liegenden Objekt abgerufen werden. Wenn dieser Text fehlt allerdings es s, da die Daten aus dem Cache verfügbar war. Wenn die Daten aus dem Cache zurückgegeben werden dort s ohne Aufruf des zugrunde liegenden Objekts und daher keine Datenbankabfrage ausgeführt.


![Die ObjectDataSource-Speicher und ruft seine Daten aus dem Datencache](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Abbildung 12**: das ObjectDataSource speichert und ruft seine Daten aus dem Datencache ab.


Jede ASP.NET-Anwendung verfügt über einen eigenen Datencache gemeinsam von allen Seiten und Besucher s-Instanz. Das bedeutet, dass die Daten im Datencache gespeichert wird, von der ObjectDataSource ebenso für alle Benutzer freigegeben ist, besuchen die Seite. Um dies zu überprüfen, öffnen Sie die `ObjectDataSource.aspx` Seite in einem Browser. Bei der ersten Seite besuchen, erscheint das ausgelöste Ereignis Markieren von Text (vorausgesetzt, dass die Daten, die dem Cache hinzugefügt, durch die vorherigen Tests mittlerweile Clusterdefinition entfernt wurde,). Öffnen Sie eine zweite Browserinstanz und kopieren und fügen Sie die URL aus der ersten Browserinstanz auf dem zweiten. In der zweiten Browserinstanz, das ausgelöste Ereignis Markieren von Text nicht angezeigt, da es verwenden, müssen die gleiche zwischengespeicherte Daten wie die erste.

Wenn die abgerufenen Daten in den Cache eingefügt wird, verwendet das ObjectDataSource einen Cache-Schlüssel-Wert, der enthält: die `CacheDuration` und `CacheExpirationPolicy` Eigenschaftswerte; der Typ des zugrunde liegenden Business-Objekts, das verwendet wird, von der ObjectDataSource, der angegeben wird über die [ `TypeName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, in diesem Beispiel); der Wert des der `SelectMethod` Eigenschaft sowie den Namen und Werte der Parameter in der `SelectParameters` Auflistung enthalten ist und die Werte für die `StartRowIndex`und `MaximumRows` Eigenschaften, die verwendet werden, bei der Implementierung [benutzerdefiniertes Paging.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Erstellen den Cache-Schlüsselwert als eine Kombination dieser Eigenschaften wird einen eindeutiger Cacheeintragsschlüssel sichergestellt, wie diese Werte ändern. Z. B. in den vergangenen Lernprogramme wir Ve erläutert, mit der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)`, womit alle Produkte für eine angegebene Kategorie. Ein Benutzer auf der Seite und Ansicht Getränke bedeutet ggf. verfügt über eine `CategoryID` 1. Wenn das ObjectDataSource seine Ergebnisse ohne Berücksichtigung zwischengespeichert der `SelectParameters` -Werten, wenn ein anderer Benutzer auf die Seite stammt anzeigen "Gewürze", während die Getränke Produkte wurden im Cache, d erscheint die zwischengespeicherten Getränke Produkte anstelle von "Gewürze". Durch Variierung der Cacheschlüssel durch diese Eigenschaften, die die Werte der enthalten die `SelectParameters`, das ObjectDataSource behält einen separate Cacheeintrag für Getränke und "Gewürze".

## <a name="stale-data-concerns"></a>Veraltete Daten Sicherheitsrisiken

Das ObjectDataSource entfernt automatisch Elemente aus dem Cache, wenn eines der `Insert`, `Update`, oder `Delete` Methoden aufgerufen wird. Dies schützt gegen veraltete Daten durch die Cache-Einträge deaktivieren, wenn die Daten über die Seite geändert werden. Es ist jedoch möglich, dass ein ObjectDataSource verwenden zwischenspeichern, um veraltete Daten weiterhin anzuzeigen. Im einfachsten Fall kann es aufgrund der Daten, die direkt in der Datenbank ändern. Ein Datenbankadministrator ausgeführt vielleicht nur ein Skript, das einige der Einträge in der Datenbank geändert wurde.

Dieses Szenario kann auch auf eine unauffälligere Weise erweitern. Während das ObjectDataSource enthaltenen Elemente aus dem Cache entfernt, wenn eine der zugehörigen Datenänderung Methoden aufgerufen wird, werden zwischengespeicherte Elemente entfernt wurden für das ObjectDataSource s bestimmte Kombination von Eigenschaftswerten (`CacheDuration`, `TypeName`, `SelectMethod`, und so weiter). Wenn Sie zwei ObjectDataSources haben, andere verwenden `SelectMethods` oder `SelectParameters`, aber dennoch kann die gleichen Daten aktualisieren, und dann eine ObjectDataSource möglicherweise einer Zeile aktualisieren und eine eigene Einträge im Cache, aber die entsprechende Zeile für die zweite ObjectDataSource ungültig wird immer noch verarbeitet werden aus den zwischengespeicherten. Überzeugen Sie sich zum Erstellen von Seiten, um diese Funktionalität bereitzustellen. Erstellen Sie eine Seite, die eine bearbeitbare GridView anzeigt, das die Daten von einem ObjectDataSource abruft, die Zwischenspeicherung verwendet wird, und ruft Daten aus der `ProductsBLL` Klasse s `GetProducts()` Methode. Fügen Sie einen anderen bearbeitbaren GridView und ObjectDataSource auf dieser Seite (oder ein anderer Computer), aber für diese zweite ObjectDataSource haben sie die Verwendung der `GetProductsByCategoryID(categoryID)` Methode. Da zwei ObjectDataSources `SelectMethod` Eigenschaften unterschiedlich sind, sie ll jeder eigene zwischengespeicherte Werte aufweisen. Wenn Sie das nächste Mal ein Produkt in einer Rasterseite, das Sie die Daten an andere Raster (Paging bearbeiten, Sortieren und So weiter) binden, werden weiterhin die alte, zwischengespeicherten Daten dienen und nicht die Änderung, die aus dem anderen Raster erstellt wurde.

Kurz gesagt, nur verwenden Sie einen zeitbasierten Cacheobjekte, wenn Sie bereit sind, haben das volle Potenzial von veralteten Daten, und verwenden Sie kürzere Cacheobjekte für Szenarien, in denen Sie unbedingt der Aktualität der Daten. Wenn keine veraltete Daten nicht zulässig ist, verzichten Zwischenspeichern auf, oder verwenden Sie SQL-Cache-Abhängigkeiten (vorausgesetzt, es ist Datenbankdaten Sie re caching). SQL-Cache-Abhängigkeiten genauer in einem späteren Lernprogramm.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm untersucht es die ObjectDataSource s integrierten caching-Funktionen. Indem Sie einfach einige Eigenschaften festlegen, können wir das ObjectDataSource aus dem angegebenen zurückgegebenen Ergebnisse zwischenspeichern anweisen `SelectMethod` in ASP.NET Datencache. Die `CacheDuration` und `CacheExpirationPolicy` -Eigenschaften angeben, die Dauer, die das Element wird zwischengespeichert und gibt an, ob es sich um einen absoluten oder gleitende Ablaufzeit handelt. Die `CacheKeyDependency` Eigenschaft ordnet alle ObjectDataSource s Cacheeinträge mit einer vorhandenen Cacheabhängigkeit. Dies kann verwendet werden, um das ObjectDataSource-s-Einträge aus dem Cache entfernen, bevor die zeitbasierte Ablaufzeit erreicht ist und normalerweise, mit der SQL-Cache-Abhängigkeiten verwendet wird.

Weil das ObjectDataSource einfach die Werte für den Datencache zwischenspeichert, konnte es ObjectDataSource s integrierte Funktionalität programmgesteuert repliziert werden. Es ist nicht t wenig Sinn, dazu an der Präsentationsschicht, da das ObjectDataSource diese Funktion ausgegeben bietet, aber Cachingfunktionen wir in einer separaten Schicht der Architektur implementieren. Zu diesem Zweck müssen wir die gleiche Logik verwendet, die für das ObjectDataSource wiederholen. Wir müssen programmgesteuert arbeiten mit den Datencache aus innerhalb der Architektur in unserem nächsten Lernprogramm untersuchen.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET-Caching: Techniken und bewährte Methoden](https://msdn.microsoft.com/library/aa478965.aspx)
- [Zwischenspeichern Handbuch zur Referenzarchitektur für .NET Framework-Anwendungen](https://msdn.microsoft.com/library/ee817645.aspx)
- [Ausgabe-Caching in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](caching-data-in-the-architecture-cs.md)
