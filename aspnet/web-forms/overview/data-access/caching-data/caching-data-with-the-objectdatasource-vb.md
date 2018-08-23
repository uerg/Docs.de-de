---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Caching kann den Unterschied zwischen einer langsamen und eine schnelle Webanwendung bedeuten. In diesem Tutorial wird die erste von vier, die einen detaillierten Einblick in die ausgabezwischenspeicherung in ASP.NET ausführen...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: baa6fd0c290c0b09cf137f12ce62f50bae52be23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837739"
---
<a name="caching-data-with-the-objectdatasource-vb"></a>Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) oder [PDF-Datei herunterladen](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> Caching kann den Unterschied zwischen einer langsamen und eine schnelle Webanwendung bedeuten. Dieses Tutorial ist der erste von vier, die einen detaillierten Einblick in die ausgabezwischenspeicherung in ASP.NET verwenden. Erfahren Sie, die grundlegenden Konzepte von Zwischenspeicherung und zwischengespeichert werden an die Darstellungsschicht über das ObjectDataSource-Steuerelement.


## <a name="introduction"></a>Einführung

In der Informatik *zwischenspeichern* ist der Prozess dauert, Daten oder Informationen, die zum Abrufen aufwendig ist, und speichern eine Kopie der Datei an einem Speicherort, der für den Zugriff auf schneller ist. Für datengesteuerte Anwendungen nutzen große und komplexe Abfragen im Allgemeinen die Mehrheit der Anwendung s bei. Solche ein s Anwendungsleistung, dann kann häufig durch Speichern der Ergebnisse von teuren Datenbankabfragen in s Anwendungsspeicher verbessert werden.

ASP.NET 2.0 bietet eine Vielzahl von Optionen zum Zwischenspeichern. Eine gesamte Webseite oder das Benutzersteuerelement s gerenderten Markup kann zwischengespeichert werden, über *ausgabezwischenspeicherung*. Die ObjectDataSource und SqlDataSource-Steuerelemente bieten Funktionen ebenfalls, wodurch, sodass Daten zum Zwischenspeichern auf der Steuerelementebene zwischengespeichert werden. Und ASP.NET s *Datencache* bietet eine umfangreiche Cache-API, die Seitenentwickler programmgesteuert Zwischenspeichern von Objekten ermöglicht. Das Zwischenspeichern in diesem Tutorial und die folgenden drei untersuchenden mithilfe der "ObjectDataSource"-s-Funktionen als auch in Datencache. Wir beleuchte auch, wie Sie anwendungsweite in Datencache beim Start und auf die zwischengespeicherte Daten durch die Verwendung von SQL-cacheabhängigkeiten aktuell halten. In diesen Tutorials werden keine Zwischenspeicherung der Ausgabe untersuchen. Einen detaillierten Einblick in die Zwischenspeicherung der Ausgabe werden soll, finden Sie unter [Ausgabezwischenspeicherung in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Caching kann an einer beliebigen Stelle in der Architektur, aus der Datenzugriffsebene nach oben auf der Darstellungsschicht angewendet werden. In diesem Tutorial betrachten wir die Anwendung von caching in der Darstellungsschicht über das ObjectDataSource-Steuerelement. Im nächsten Tutorial untersuchenden Zwischenspeichern von Daten auf der Geschäftslogikebene.

## <a name="key-caching-concepts"></a>Schlüssel, die Caching-Konzepte

Mit Caching kann eine Anwendung s erheblich verbessert allgemeine Leistung und Skalierbarkeit, indem Sie Daten, die zum Generieren aufwendig ist, und speichern eine Kopie der Datei an einem Speicherort an, die effizienter zugegriffen werden kann. Da der Cache nur eine Kopie der tatsächlichen, zugrunde liegenden Daten enthält, können mehr aktuell ist, oder *veraltete*, wenn die zugrunde liegenden Daten ändern. Als Lösung kann Seitenentwickler angeben, Kriterien, die mit dem das Element im Cache werden *entfernt* aus dem Cache, verwenden Sie entweder:

- **Zeitbasierten Kriterien** ein Element zum Cache für eine Absolute oder gleitende Dauer hinzugefügt werden kann. Beispielsweise kann ein Entwickler eine Dauer von, z. B. 60 Sekunden hinweisen. Das zwischengespeicherte Element wird mit einer absoluten Dauer 60 Sekunden, nachdem er hinzugefügt wurde, Cache, unabhängig davon, wie häufig darauf zugegriffen wurde entfernt. Mit der gleitenden Dauer entfernt das zwischengespeicherte Element wird 60 Sekunden nach dem letzten Zugriff.
- **Abhängigkeit meldungsbasierte** eine Abhängigkeit kann zugeordnet ist, mit einem Element, wenn dem Cache hinzugefügt werden. Wenn die Abhängigkeit von Element s ändert, wird es aus dem Cache entfernt. Die Abhängigkeit kann es sich um eine Datei, einem anderen Element im Cache oder eine Kombination aus beidem sein. ASP.NET 2.0 ermöglicht auch SQL-cacheabhängigkeiten, die ermöglichen Entwicklern den Cache ein Element hinzugefügt und entfernt werden, wenn die zugrunde liegenden Datenbankdaten geändert wird. Wir werden untersuchen, SQL-cacheabhängigkeiten im kommenden [mithilfe von SQL-Cacheabhängigkeiten](using-sql-cache-dependencies-vb.md) Tutorial.

Unabhängig von der angegebenen Entfernung Kriterien, ein Element im Cache möglicherweise *aufgeräumt* , bevor die zeitbasierte oder Abhängigkeit basierenden Kriterien erfüllt sind. Wenn der Cache seine Kapazität erreicht hat, müssen vorhandene Elemente entfernt werden, bevor neue hinzugefügt werden können. Daher, wenn Sie programmgesteuert mit zwischengespeicherten Daten es wichtige s einbeziehen, dass Sie immer davon ausgehen, dass die zwischengespeicherten Daten möglicherweise nicht vorhanden. Betrachten wir das Muster für Daten aus dem Cache programmgesteuert in unserem nächsten Tutorial, den Zugriff auf *Zwischenspeichern von Daten in der Architektur*.

Caching bietet eine kostengünstige Möglichkeit, für die Ärmel mehr Leistung aus einer Anwendung ist. Als [Steven Smith](http://aspadvice.com/blogs/ssmith/) muss in seinem Artikel [ASP.NET-Caching: Techniken und bewährte Verfahren](https://msdn.microsoft.com/library/aa478965.aspx):

Caching kann eine gute Möglichkeit, gut genug Leistung zu erzielen ohne viel Zeit und Analyse sein. Speicher ist günstig, also wenn Sie die Leistung erzielen können, Sie durch Zwischenspeichern der Ausgabe für 30 Sekunden, statt sich mit einem Tag oder einer Woche, die versucht, den Code oder die Datenbank zu optimieren müssen, führen Sie die Cachinglösung (vorausgesetzt, 30: zweite alte Daten ist in Ordnung), und fahren. Schließlich wird schlechte wahrscheinlich, informieren Sie sich daher natürlich Sie versuchen sollten, den ordnungsgemäßen Entwurf Ihrer Anwendungen. Wenn Sie nur zum Erzielen einer guten heute genügend Leistung benötigen, Zwischenspeichern kann jedoch sein [Ansatz ausgezeichnet], kaufen Sie Zeit haben, um Ihre Anwendung zu einem späteren Zeitpunkt zu gestalten, wenn Sie Zeit dazu haben.

Zwar Zwischenspeichern spürbaren leistungsverbesserungen bereitstellen kann, es ist nicht verfügbar in allen Situationen, z. B. bei Anwendungen, die in Echtzeit, häufig aktualisieren von Daten verwenden, oder, in denen veraltete Daten auch in Kürze kurzlebig ist nicht zulässig. Aber für die meisten Anwendungen, Zwischenspeicherung verwendet werden soll. Weitere Informationen zur Zwischenspeicherung in ASP.NET 2.0 finden Sie unter der [Zwischenspeicherung für die Leistung](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) Teil der [ASP.NET 2.0-Schnellstart-Lernprogramme](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Schritt 1: Erstellen der Zwischenspeicherung von Webseiten

Bevor wir unsere Erkundung der s caching-Funktionen "ObjectDataSource" beginnen, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Lernprogramm sowie die folgenden drei benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `Caching`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme cacherelevanten hinzu.](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die Lernprogramme cacherelevanten hinzu.


Wie in den anderen Ordnern `Default.aspx` in die `Caching` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Abbildung 2: Hinzufügen der SectionLevelTutorialListing.ascx Benutzersteuerelement an "default.aspx"](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Abbildung 2**: Abbildung 2: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image4.png))


Abschließend fügen Sie diese Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach über das Arbeiten mit Binärdaten `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für die Zwischenspeicherung Tutorials.


![Die Sitemap enthält jetzt die Einträge für die Caching-Lernprogramme](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Abbildung 3**: die Sitemap enthält jetzt Einträge für die Zwischenspeicherung Lernprogramme


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Schritt 2: Anzeigen einer Liste der Produkte in einer Webseite

In diesem Tutorial erfahren Sie, wie die ObjectDataSource-Steuerelement s integrierte Funktionen zum zwischenspeichernden verwenden. Bevor wir auf diese Funktionen suchen kann, sollten, wir zuerst eine Seite, um arbeiten. Let s Erstellen einer Webseite, die zum Auflisten von Produktinformationen abgerufen, indem ein ObjectDataSource-Steuerelement aus einer GridView-Ansicht verwendet das `ProductsBLL` Klasse.

Öffnen Sie zunächst die `ObjectDataSource.aspx` auf der Seite die `Caching` Ordner. Einer GridView-Ansicht aus der Toolbox in den Designer ziehen, legen Sie dessen `ID` Eigenschaft `Products`, und sein Smarttag, auswählen, um es an ein neues ObjectDataSource-Steuerelement, das mit dem Namen binden `ProductsDataSource`. Konfigurieren Sie zum Arbeiten mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL-Klasse](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Abbildung 4**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image8.png))


Lassen Sie für diese Seite s, die eine bearbeitbare GridView zu erstellen, damit wir prüfen können, was geschieht, wenn die Daten, die zwischengespeichert werden, in dem ObjectDataSource-Steuerelement über die GridView-s-Schnittstelle geändert werden. Lassen Sie die Dropdownliste in der Registerkarte mit der Option auswählen, die auf seine Standardwerte zurückgesetzt `GetProducts()`, aber ändern Sie das ausgewählte Element in der Registerkarte "Updates", um die `UpdateProduct` Überladung verwenden, akzeptiert `productName`, `unitPrice`, und `productID` als Eingabeparameter.


[![Legen Sie das UPDATE Registerkarte s Dropdown-Liste an die entsprechenden UpdateProduct-Überladung](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Abbildung 5**: Legen Sie die Registerkarte "UPDATE"-s-Dropdown-Listenfeld auf der angemessen `UpdateProduct` überladen ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image11.png))


Klicken Sie abschließend die Dropdownlisten auf den Registerkarten für INSERT- und DELETE auf (keine) festgelegt, und klicken Sie auf "Fertig stellen". Nach Abschluss des Konfigurieren von Datenquellen-Assistenten, legt Visual Studio das "ObjectDataSource"-s `OldValuesParameterFormatString` Eigenschaft `original_{0}`. Wie unter der [eine Übersicht der einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Tutorial, diese Eigenschaft muss aus der deklarativen Syntax entfernt oder wieder auf den Standardwert festgelegt `{0}`, in der Reihenfolge für unseren Workflow aktualisieren ohne Fehler fortgesetzt werden.

Darüber hinaus fügt nach dem Abschluss des Assistenten für Visual Studio ein Feld an die GridView für die einzelnen Datenfelder, Produkt. Entfernen Sie alle außer den `ProductName`, `CategoryName`, und `UnitPrice` BoundFields. Aktualisieren Sie als Nächstes die `HeaderText` Eigenschaften der einzelnen diese BoundFields Product Category und Preis bzw. Da die `ProductName` ist ein Pflichtfeld, das BoundField in ein TemplateField konvertieren und einen RequiredFieldValidator zum Hinzufügen der `EditItemTemplate`. Auf ähnliche Weise konvertieren die `UnitPrice` BoundField in ein TemplateField und fügen Sie eine CompareValidator, um sicherzustellen, dass der vom Benutzer eingegebene Wert eine gültige Currency-Wert, s, die größer als oder gleich 0 (null). Zusätzlich zu diesen Änderungen gerne alle ästhetischen Änderungen, z. B. rechten führen die `UnitPrice` -Wert, oder das angeben, die Formatierung für die `UnitPrice` Text in den schreibgeschützten Modus, und bearbeiten-Schnittstellen.

Stellen Sie die GridView durch Aktivieren des Kontrollkästchens Bearbeiten aktivieren, in das GridView-s-Smarttag bearbeitet werden. Auch die Kontrollkästchen der Paging aktivieren und die Sortierung zu aktivieren.

> [!NOTE]
> Benötigen Sie eine Übersicht über die GridView-s-Bearbeitungsschnittstelle anpassen? Wenn dies der Fall ist, verweisen zurück auf die [Anpassen der Benutzeroberfläche für die Änderung der Daten](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) Tutorial.


[![Aktivieren Sie GridView-Unterstützung zum Bearbeiten, Sortieren und Paging](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Abbildung 6**: Aktivieren der GridView-Unterstützung für bearbeiten, Sortieren und Paging ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image14.png))


Nachdem diese GridView-Änderungen vorgenommen wurden, sollte GridView und "ObjectDataSource" s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Wie in Abbildung 7 dargestellt, listet die bearbeitbaren GridView Name, Kategorie und Preis für jedes der Produkte in der Datenbank. Können Sie die Ergebnisse durchlaufen, die Sortierung der Seite "s"-Funktionalität zu testen und Bearbeiten eines Datensatzes.


[![Jedes Produkt s Name, Kategorie und Preise finden Sie in sortierbar, navigierbaren, bearbeitbaren GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Abbildung 7**: jedes Produkt s Name, Kategorie und Preise finden Sie in sortierbar, navigierbaren, bearbeitbaren GridView ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Schritt 3: Untersuchen bei der "ObjectDataSource" Anfordern von Daten ist

Die `Products` GridView Ruft ab, die Daten für die anzuzeigenden durch Aufrufen der `Select` -Methode der der `ProductsDataSource` "ObjectDataSource". Diese "ObjectDataSource" erstellt eine Instanz der Geschäftslogikebene Zuordnungsvorgänge `ProductsBLL` -Klasse und ruft seine `GetProducts()` -Methode, die wiederum, die Daten von Zugriffsschicht s aufruft `ProductsTableAdapter` s `GetProducts()` Methode. Die DAL-Methode, eine Verbindung mit der Northwind-Datenbank her, und gibt den konfigurierten `SELECT` Abfrage. Diese Daten werden dann an die DAL, die sie in verpackt zurückgegeben eine `NorthwindDataTable`. Das DataTable-Objekt wird an die BLL, zurückgegeben, das an dem ObjectDataSource-Steuerelement, das an die GridView zurückgegeben zurückgegeben. GridView erstellt dann eine `GridViewRow` Objekt für die einzelnen `DataRow` in der Datentabelle und jeder `GridViewRow` schließlich in den HTML-Code, der an den Client zurückgegeben und angezeigt, auf dem Besucher s-Browser gerendert wird.

Diese Abfolge von Ereignissen tritt jedes Mal, die, das die GridView zu der zugrunde liegenden Daten gebunden werden muss. In diesem Fall, wenn die Seite zuerst zugegriffen wird, wenn von einer Seite mit Daten in eine andere verschieben, beim Sortieren von GridView, oder wenn Sie die GridView-s-Daten über den integrierten bearbeiten oder Löschen von Schnittstellen zu ändern. Wenn der GridView-s-Ansichtszustand deaktiviert ist, wird die GridView sowie jedes Postback erneut gebunden. GridView kann auch werden explizit erneut gebunden an seine Daten durch Aufrufen der `DataBind()` Methode.

Um die Häufigkeit ehesten mit der die Daten aus der Datenbank abgerufen wurden, können Sie s Anzeige eine Meldung angezeigt, wenn die Daten erneut abgerufen werden. Fügen Sie ein Label-Steuerelement über die GridView, die mit dem Namen `ODSEvents`. Löschen Sie die `Text` Eigenschaft, und legen dessen `EnableViewState` Eigenschaft `False`. Unter der Bezeichnung, fügen Sie ein Steuerelement Schaltfläche hinzu, und legen Sie dessen `Text` Eigenschaft Postbacks.


[![Hinzufügen einer Bezeichnung und eine Schaltfläche auf der Seite über GridView](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Abbildung 8**: Hinzufügen einer Bezeichnung und eine Schaltfläche auf der Seite über die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image20.png))


Während des Data Access-Workflows, das "ObjectDataSource"-s `Selecting` Ereignis wird ausgelöst, bevor das zugrunde liegende Objekt erstellt wird und dessen konfigurierten aufgerufene Methode. Erstellen Sie einen Ereignishandler für dieses Ereignis aus, und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

Jedes Mal, wenn dem ObjectDataSource-Steuerelement eine Anforderung an der Architektur für die Daten sendet, zeigt die Bezeichnung den Text auswählen-Ereignis ausgelöst.

Besuchen Sie diese Seite in einem Browser aus. Wenn die Seite zuerst aufgerufen wird, wird das Ereignis auswählen des Texts wird ausgelöst, angezeigt. Klicken Sie auf die Schaltfläche mit den Postback, und beachten Sie, dass der Text verschwindet (vorausgesetzt, dass die GridView s `EnableViewState` -Eigenschaftensatz auf `True`, der Standardwert). Dies liegt daran, beim Postback GridView wird aus dem Ansichtszustand wiederhergestellt und auf dem ObjectDataSource-Steuerelement für die Daten aus diesem Grund t zu aktivieren. Sortieren, paging oder das Bearbeiten der Daten, bewirkt jedoch, dass der GridView, sich erneut an seine Datenquelle bindet und aus diesem Grund wird die auswählen-Ereignis ausgelöst, Text wird erneut angezeigt.


[![Wenn mit der Datenquelle erneut die GridView gebunden ist, wird die auswählen-Ereignis ausgelöst angezeigt.](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Abbildung 9**: immer die GridView wird erneut gebunden, mit der Datenquelle, die auswählen-Ereignis ausgelöst wird ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![Durch Klicken auf das Postback-Schaltfläche bewirkt, dass der GridView, die aus dem Ansichtszustand wiederhergestellt werden](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Abbildung 10**: Klicken Sie auf die Schaltfläche mit den Postback bewirkt, dass die GridView, die aus dem Ansichtszustand wiederhergestellt werden ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image26.png))


Es mag es ineffizient, die Daten der Datenbank jedes Mal abrufen, die Daten über ausgelagert oder sortiert. Schließlich hat seit wir erneut Standardpaging, dem ObjectDataSource-Steuerelement alle Datensätze abgerufen, wenn die erste Seite anzeigen. Auch wenn die GridView sortieren und paging-Unterstützung nicht bereitstellt, müssen die Daten aus der Datenbank jedes Mal abgerufen werden, die zuerst die Seite besucht wird, von einem Benutzer (und bei jedem Postback, wenn der Ansichtszustand deaktiviert ist). Wenn jedoch die GridView die gleichen Daten für alle Benutzer angezeigt wird, diese zusätzliche datenbankanforderungen überflüssig. Warum nicht Zwischenspeichern von zurückgegebenen Ergebnisse der `GetProducts()` -Methode und die Bindung der GridView, die zwischengespeicherte Ergebnisse?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Schritt 4: Zwischenspeichern der Daten, die mit dem ObjectDataSource-Steuerelement

Wenn Sie einfach einige Eigenschaften festlegen, kann dem ObjectDataSource-Steuerelement für die automatisch die abgerufenen Daten im Datencache ASP.NET cache konfiguriert werden. Die folgende Liste enthält die Cache-bezogenen Eigenschaften des dem ObjectDataSource-Steuerelement:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) muss festgelegt werden, um `True` zum Aktivieren der Zwischenspeicherung. Die Standardeinstellung ist `False`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) die Zeitspanne in Sekunden, die die Daten zwischengespeichert werden. Der Standard ist 0. Dem ObjectDataSource-Steuerelement wird nur die Daten zwischengespeichert, wenn `EnableCaching` ist `True` und `CacheDuration` auf einen Wert größer als 0 (null) festgelegt ist.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) kann festgelegt werden, um `Absolute` oder `Sliding`. Wenn `Absolute`, dem ObjectDataSource-Steuerelement speichert die abgerufenen Daten für `CacheDuration` Sekunden; Wenn `Sliding`, die Daten ablaufen, nur, nachdem es für die nicht zugegriffen wurde `CacheDuration` Sekunden. Die Standardeinstellung ist `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) verwenden Sie diese Eigenschaft, einer vorhandenen Cacheabhängigkeit die "ObjectDataSource"-s-Cacheeinträge zugeordnet werden soll. Die "ObjectDataSource"-s-Dateneinträge können vorzeitig aus dem Cache entfernt werden, von der zugehörigen ablaufen `CacheKeyDependency`. Diese Eigenschaft wird am häufigsten verwendet, um eine SQL-Cacheabhängigkeit mit dem ObjectDataSource-s-Cache zuzuordnen, ein Thema wird erläutert, in der Zukunft [mithilfe von SQL-Cacheabhängigkeiten](using-sql-cache-dependencies-vb.md) Tutorial.

S konfigurieren lassen die `ProductsDataSource` ObjectDataSource-Steuerelement zum Zwischenspeichern von Daten für 30 Sekunden für absoluten Skala. Legen Sie das "ObjectDataSource"-s `EnableCaching` Eigenschaft `True` und die zugehörige `CacheDuration` Eigenschaft auf 30. Lassen Sie die `CacheExpirationPolicy` -Eigenschaft auf die Standardeinstellung verwenden, `Absolute`.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zum Zwischenspeichern von Daten für 30 Sekunden](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Abbildung 11**: Konfigurieren von dem ObjectDataSource-Steuerelement zum Zwischenspeichern von Daten für 30 Sekunden ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-with-the-objectdatasource-vb/_static/image29.png))


Die Änderungen zu speichern, und rufen Sie diese Seite in einem Browser erneut. Der Markieren von Text ausgelöste Ereignis wird wie ursprünglich die Daten nicht im Cache sind beim ersten die Seite Besuch angezeigt. Jedoch nachfolgenden Postbacks ausgelöst wird, mit der Schaltfläche Postback, sortieren, paging oder das Klicken auf die bearbeiten "oder" Abbrechen " *nicht* erneuten Anzeigen der Auswahl-Ereignis wird ausgelöst, Text. Grund hierfür ist die `Selecting` Ereignis nur ausgelöst wird, wenn es sich bei dem ObjectDataSource-Steuerelement seine Daten aus der zugrunde liegenden Objekts; ruft die `Selecting` Ereignis wird nicht ausgelöst, wenn die Daten aus dem Datencache abgerufen werden.

Nach 30 Sekunden werden die Daten aus dem Cache entfernt werden. Die Daten werden auch aus dem Cache entfernt werden, wenn das "ObjectDataSource"-s `Insert`, `Update`, oder `Delete` Methoden aufgerufen werden. Daher nach 30 Sekunden bestanden wurden oder die Schaltfläche "Aktualisieren" geklickt, sortieren, paging, oder klicken Sie auf die bearbeiten "oder" Abbrechen "dazu, das" ObjectDataSource "dass führt, um die Daten aus der zugrunde liegende Objekt, Anzeigen von die auswählen-Ereignis wird ausgelöst, Text bei der `Selecting` -Ereignis ausgelöst wird. Diese zurückgegebenen Ergebnisse werden zurück in Datencache platziert.

> [!NOTE]
> Wenn Sie den ausgelöste Ereignis Markieren von Text häufig angezeigt wird, selbst wenn Sie erwarten, dass dem ObjectDataSource-Steuerelement funktioniert mit zwischengespeicherten Daten, kann es aufgrund von arbeitsspeicherbeschränkungen sein. Wenn nicht genügend Arbeitsspeicher vorhanden ist, wurde möglicherweise die Daten, die dem Cache hinzugefügt, von dem ObjectDataSource-Steuerelement geleert. Wenn das ObjectDataSource-Steuerelement t scheinbar ordnungsgemäß die Daten oder nur Caches im Cache gespeichert wird die Daten sporadisch, schließen Sie einige Anwendungen, um Arbeitsspeicher freizugeben, und versuchen Sie es erneut.


Abbildung 12 wird die Workflow Zwischenspeichern "ObjectDataSource"-s veranschaulicht. Beim Auslösen des Ereignisses auswählen Text auf dem Bildschirm angezeigt wird, da die Daten nicht im Cache wurde und hatte, aus dem zugrunde liegenden Objekt abgerufen werden sollen. Wenn dieser Text nicht angezeigt wird, aber es s, da die Daten aus dem Cache verfügbar waren. Wenn die Daten aus dem Cache zurückgegeben werden dort s kein Aufruf auf die zugrunde liegende Objekt und somit keine Datenbankabfrage ausgeführt.


![Die "ObjectDataSource" speichert und ruft seine Daten aus dem Datencache](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Abbildung 12**: dem ObjectDataSource-Steuerelement speichert und ruft seine Daten aus dem Datencache ab.


Jede ASP.NET-Anwendung hat seinen eigenen Datencache-Instanz, s, die für alle Seiten und Besucher gemeinsam verwendet. Das bedeutet, dass die Daten im Datencache gespeichert wird, von dem ObjectDataSource-Steuerelement auch für alle Benutzer freigegeben werden, die die Seite zu besuchen. Um dies zu überprüfen, öffnen Sie die `ObjectDataSource.aspx` Seite in einem Browser. Wenn die Seite zuerst besuchen zu können, erscheint der Text der Auswahl-Ereignis ausgelöst, (vorausgesetzt, dass die Daten, die dem Cache hinzugefügt, durch die vorherigen Tests jetzt entfernt wurde,). Öffnen Sie eine zweite Browserinstanz, und kopieren und fügen Sie die URL der ersten Instanz für den Browser an den zweiten. In der zweiten Browserinstanz, der Markieren von Text ausgelöste Ereignis wird nicht angezeigt, da es s, die mit dem gleichen zwischengespeicherten Daten wie die erste.

Wenn die abgerufenen Daten in den Cache eingefügt werden soll, wird dem ObjectDataSource-Steuerelement verwendet einen Cache-Key-Wert, der enthält: die `CacheDuration` und `CacheExpirationPolicy` Eigenschaftswerte, die den Typ des zugrunde liegenden Geschäftsobjekt verwendet wird, von dem ObjectDataSource-Steuerelement, das angegeben wird über die [ `TypeName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, in diesem Beispiel); der Wert des der `SelectMethod` -Eigenschaft und die Namen und die Werte der Parameter in der `SelectParameters` Auflistung und die Werte für die `StartRowIndex`und `MaximumRows` Eigenschaften, die verwendet werden, bei der Implementierung [benutzerdefiniertes Paging](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

Erstellen den Cache-Key-Wert als eine Kombination dieser Eigenschaften wird einen eindeutiger Cacheeintragsschlüssel sichergestellt, da diese Werte ändern. Z. B. in den letzten Tutorials wir Ve gesucht, mit der `ProductsBLL` s-Klasse `GetProductsByCategoryID(categoryID)`, womit alle Produkte für eine angegebene Kategorie. Ein Benutzer ggf. auf der Seite und Getränke, die eine `CategoryID` 1. Wenn dem ObjectDataSource-Steuerelement seine Ergebnisse ohne Berücksichtigung von im Cache der `SelectParameters` Werte, wenn ein anderer Benutzer auf die Seite stammt anzeigen "Gewürze", während die Getränke Produkte wurden im Cache, die sie d finden Sie unter den zwischengespeicherten trinken Produkte anstelle von "Gewürze". Durch die Änderung des Cacheschlüssels anhand dieser Eigenschaften, die die Werte der enthalten die `SelectParameters`, dem ObjectDataSource-Steuerelement verwaltet einen getrenntes Cache-Eintrag für Getränke und "Gewürze".

## <a name="stale-data-concerns"></a>Veraltete Daten Probleme

Dem ObjectDataSource-Steuerelement entfernt automatisch die Elemente aus dem Cache, wenn eines der `Insert`, `Update`, oder `Delete` Methoden aufgerufen wird. Dies schützt vor veralteter Daten durch die Cacheeinträge beseitigen, wenn die Daten über die Seite geändert werden. Allerdings ist es möglich, dass ein ObjectDataSource-Steuerelement verwenden zwischenspeichern, um veraltete Daten weiterhin anzuzeigen. Im einfachsten Fall kann es aufgrund der Daten ändern, die direkt in der Datenbank sein. Ein Datenbankadministrator ausgeführt vielleicht nur ein Skript, das einige der Einträge in der Datenbank geändert wurde.

Dieses Szenario kann auch auf eine subtilere Weise erweitern. Während dem ObjectDataSource-Steuerelement seine Elemente aus dem Cache entfernt, wenn eine der Methoden ändern Daten aufgerufen wird, die zwischengespeicherten Elemente, die entfernt werden, für das "ObjectDataSource" s-Kombination von Eigenschaftswerten (`CacheDuration`, `TypeName`, `SelectMethod`, und so weiter). Wenn Sie über zwei ObjectDataSources verfügen, mit denen verschiedene `SelectMethods` oder `SelectParameters`, aber dennoch kann die gleichen Daten aktualisieren, und klicken Sie dann ein ObjectDataSource-Steuerelement möglicherweise eine Zeile zu aktualisieren und eine eigene Einträge im Cache, aber die entsprechende Zeile für die zweite "ObjectDataSource" ungültig immer noch verarbeitet werden aus den zwischengespeicherten. Gerne können Sie zum Erstellen von Seiten, um diese Funktionalität aufweisen. Erstellen Sie eine Seite, einer bearbeitbaren GridView, der die Daten aus einer "ObjectDataSource", die mithilfe der Zwischenspeicherung konfiguriert ist angezeigt abruft, rufen Sie Daten aus, der `ProductsBLL` Klasse s `GetProducts()` Methode. Fügen Sie ein weiteres bearbeitbare GridView und ObjectDataSource-Steuerelement auf dieser Seite (oder eine andere), aber für diese zweite "ObjectDataSource" haben Sie verwendet, sollte die `GetProductsByCategoryID(categoryID)` Methode. Da die zwei ObjectDataSources `SelectMethod` Eigenschaften unterscheiden sich, sie jede haben ihre eigenen zwischengespeicherten Werte. Wenn Sie das nächste Mal ein Produkt in einem Raster, das Sie die Daten (durch Paging bearbeiten, Sortieren und So weiter) an das andere Raster binden, wird es dienen immer noch die alten, zwischengespeicherte Daten und nicht wieder die Änderung, die aus dem anderen Raster erstellt wurde.

Kurz gesagt, nur verwenden Sie zeitbasierte Cacheobjekte, wenn Sie bereit sind, besteht das Risiko von veralteten Daten aus, und verwenden Sie kürzere Cacheobjekte für Szenarien, in dem Sie unbedingt der Aktualität der Daten. Wenn veraltete Daten nicht zulässig ist, wird die Zwischenspeicherung auf, oder verwenden Sie SQL-cacheabhängigkeiten (vorausgesetzt, es der Datenbankdaten ist Sie erneut die Zwischenspeicherung). Wir werden die SQL-cacheabhängigkeiten in einem späteren Tutorial untersuchen.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial untersucht es die "ObjectDataSource" s integrierten caching-Funktionen. Wenn Sie einfach einige Eigenschaften festlegen, können wir dem ObjectDataSource-Steuerelement, die aus dem angegebenen zurückgegebenen Ergebnisse zwischenzuspeichern anweisen `SelectMethod` in Datencache ASP.NET. Die `CacheDuration` und `CacheExpirationPolicy` -Eigenschaften angeben, die Dauer der Zwischenspeicherung des Elements ist und ob es sich um eine Absolute oder gleitende Ablaufzeit ist. Die `CacheKeyDependency` Eigenschaft ordnet alle "ObjectDataSource" s-Cache-Einträge mit einer vorhandenen Cacheabhängigkeit. Dies kann verwendet werden, um die "ObjectDataSource"-s-Einträge aus dem Cache entfernen, bevor das Ablaufdatum eines zeitbasierten erreicht ist, und normalerweise, mit der SQL-cacheabhängigkeiten verwendet wird.

Da dem ObjectDataSource-Steuerelement einfach die Werte für den Datencache zwischengespeichert werden, konnten wir die integrierte "ObjectDataSource"-s-Funktion programmgesteuert repliziert werden. Es verfügt t sinnvoll sein, dies geschieht in der Darstellungsschicht, da dem ObjectDataSource-Steuerelement diese Funktionalität standardmäßig stellt, aber wir Funktionen zwischenspeichern, in einer separaten Schicht der Architektur implementieren können. Zu diesem Zweck müssen wir die gleiche Logik, die dem ObjectDataSource-Steuerelement ein, die wiederholt werden soll. Untersucht werden, wie Sie programmgesteuert mit dem Datencache von innerhalb der Architektur in unserem nächsten Tutorial arbeiten.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Zwischenspeicherung in ASP.NET: Verfahren und bewährte Methoden für](https://msdn.microsoft.com/library/aa478965.aspx)
- [Zwischenspeichern Architekturhandbuch für .NET Framework-Anwendungen](https://msdn.microsoft.com/library/ee817645.aspx)
- [Ausgabe-Caching in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-sql-cache-dependencies-cs.md)
> [Weiter](caching-data-in-the-architecture-vb.md)
