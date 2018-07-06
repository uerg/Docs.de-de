---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Zwischenspeichern von Daten beim Anwendungsstart (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In einer beliebigen Webanwendung einige Daten häufig verwendet werden, und einige Daten nur selten verwendet werden. Die Leistung unserer ASP.NET Anwendung b verbessert werden kann...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: cf63cafbd4fd3937d18afa0d72a48868d76d3888
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841379"
---
<a name="caching-data-at-application-startup-vb"></a>Zwischenspeichern von Daten beim Anwendungsstart (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> In einer beliebigen Webanwendung einige Daten häufig verwendet werden, und einige Daten nur selten verwendet werden. Wir können die Leistung der Anwendung ASP.NET verbessern, indem Sie laden im voraus die häufig verwendete Daten, eine Technik, die als bekannt. Dieses Tutorial veranschaulicht einen Ansatz für proaktives laden, die zum Laden von Daten in den Cache beim Start der Anwendung ist.


## <a name="introduction"></a>Einführung

Die beiden vorherigen Tutorials haben Zwischenspeichern von Daten in die Darstellung und das Zwischenspeichern von Ebenen. In [Zwischenspeichern von Daten mit dem ObjectDataSource-Steuerelement](caching-data-with-the-objectdatasource-vb.md), wir haben uns über die caching-Funktionen zum Zwischenspeichern von Daten in der Darstellungsschicht "ObjectDataSource"-s verwenden. [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) untersucht der Zwischenspeicherung in eine neue, eigenständige Caching-Schicht. In diesen Tutorials verwendet beide *reaktive laden* bei der Arbeit mit dem Datencache. Mit reaktiver laden, jedes Mal, die die Daten angefordert wird, das System überprüft zuerst, ob es s im Cache. Wenn dies nicht der Fall, er ruft die Daten aus der ursprünglichen Quelle, z. B. die Datenbank, und klicken Sie dann im Cache gespeichert. Der Hauptvorteil reaktive geladen wurde, ist die einfache Implementierung. Einer der Nachteile ist ihrer inkonsistenten Leistung, anforderungsübergreifend. Angenommen Sie, eine Seite, die die Caching-Ebene aus dem vorherigen Lernprogramm verwendet wird, um Produktinformationen anzuzeigen. Wenn diese Seite zum ersten Mal besucht oder besucht zum ersten Mal nach dem die zwischengespeicherten Daten aufgrund von arbeitsspeicherbeschränkungen oder die angegebene Ablaufzeit erreicht haben entfernt wurde, müssen die Daten aus der Datenbank abgerufen werden. Aus diesem Grund werden diese Benutzer Anforderungen dauert länger als Benutzer Anfragen, die bereitgestellt werden, können vom Cache.

*Proaktives laden* bietet einer Strategie zur alternativen cacheverwaltung, glättet sich die Leistung für Anforderungen durch Laden der zwischengespeicherten Daten, bevor sie s erforderlich sind. Proaktives Laden verwendet normalerweise einen Prozess aus, der in regelmäßigen Abständen überprüft oder wird benachrichtigt, wenn ein Update für den zugrunde liegenden Daten aufgetreten ist. Dieser Vorgang aktualisiert dann den Cache, um es dem aktuellen Stand halten. Proaktives Laden ist besonders hilfreich, wenn die zugrunde liegenden Daten aus einer Verbindung mit langsamen Datenbank, einen Webdienst oder einer besonders langsam Datenquelle stammen. Dieser Ansatz ist jedoch die proaktive geladen wurde, schwieriger zu implementieren, da es erfordert, erstellen, verwalten und einen Prozess, um die Änderungen überprüfen und aktualisieren Sie den Cache bereitstellen.

Eine Abwandlung des proaktives laden und der Typ, den wir in diesem Tutorial untersuchen werde ist das Laden von Daten in den Cache beim Start der Anwendung. Dieser Ansatz ist besonders nützlich für die Zwischenspeicherung von statischer Daten, z. B. die Datensätze in Tabellen der Suche.

> [!NOTE]
> Ausführlichere Informationen zu den Unterschieden zwischen proaktive und reaktive laden sowie Listen von vor- und Nachteile Empfehlungen für Implementierung, finden Sie in der [Verwalten des Inhalts eines Caches](https://msdn.microsoft.com/library/ms978503.aspx) Teil der [ Handbuch zur Architektur der .NET Framework-Anwendungen für die Zwischenspeicherung](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Schritt 1: Bestimmen, welche Daten in Cache beim Start der Anwendung

Die Zwischenspeicherung Beispiele zur Verwendung von reaktive lädt, die wir in die vorherigen beiden Tutorials Arbeit mit Daten, die in regelmäßigen Abständen geändert und nimmt keine exorbitantly lange zum Generieren untersucht werden. Aber wenn sich die zwischengespeicherten Daten nie ändert, ist der Ablauf von reaktive Laden verwendet überflüssig. Auch wenn die Daten der Zwischenspeicherung eine unglaublich lange Zeit zum Generieren akzeptiert, klicken Sie dann diese Benutzer, deren Anforderungen finden Sie der Cache leeren muss Sender eine lange Wartezeit während der zugrunde liegenden Daten, werden abgerufen. Erwägen Sie die Zwischenspeicherung von statischen Daten sowie Daten, die eine außergewöhnlich lange dauert, um beim Start der Anwendung zu generieren.

Während Datenbanken viele dynamische haben, haben häufig ändern von Werten, die meisten auch eine viel von statischen Daten. Praktisch alle Datenmodelle haben z. B. eine oder mehrere Spalten, die einen bestimmten Wert aus einem festen Satz von Optionen enthalten. Ein `Patients` Datenbanktabelle möglicherweise eine `PrimaryLanguage` Spalte, die Menge von Werten, Englisch, Spanisch, Französisch, Russisch, Japanisch und So weiter sein kann. Häufig werden diese Arten von Spalten mithilfe von implementiert *Nachschlagetabellen*. Statt zu speichern die Zeichenfolge, die Englisch oder Französisch in der `Patients` Tabelle, eine zweite Tabelle wird erstellt, die in der Regel zwei Spalten – ein eindeutiger Bezeichner und eine zeichenfolgenbeschreibung – mit einem Eintrag für jeden möglichen Wert verfügt. Die `PrimaryLanguage` -Spalte in der `Patients` Tabelle speichert den entsprechenden Bezeichner in der Nachschlagetabelle. In Abbildung 1 ist Patienten John Doe s primäre Sprache Englisch sowie während Ed Johnson s Russisch.


![Die Sprachen-Tabelle ist eine Suche Tabelle verwendet, durch die Patienten-Tabelle](caching-data-at-application-startup-vb/_static/image1.png)

**Abbildung 1**: die `Languages` Tabelle ist eine Suche Tabelle verwendet, durch die `Patients` Tabelle


Die Benutzeroberfläche zum Bearbeiten oder erstellen einen neuen Patienten zählen eine Dropdown-Liste der zulässigen Sprachen, die aufgefüllt wird, durch die Datensätze in der `Languages` Tabelle. Ohne Zwischenspeicherung, besucht jedes Mal, die diese Schnittstelle ist das System muss eine Abfrage der `Languages` Tabelle. Dies ist Vergeudung und unnötige da ändern Sie die Werte für Nachschlagetabellen sehr selten, wenn überhaupt.

Wir konnten Sie Zwischenspeichern der `Languages` Daten, die mit den gleichen reaktive laden Verfahren, die in den vorherigen Tutorials untersucht. Reaktive zu laden, verwendet jedoch eine zeitbasierte-Ablauf, die für die statischen Nachschlage-Tabellendaten nicht erforderlich ist. Beim Zwischenspeichern mithilfe von reactive laden besser als gar keine Zwischenspeicherung wäre, wäre die beste Vorgehensweise proaktiv Laden von Daten der Suche in den Cache beim Start der Anwendung.

In diesem Tutorial werden wir das Cache-Lookup-Tabellendaten und anderen statischen Informationen betrachten.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Schritt 2: Untersuchen die verschiedenen Möglichkeiten zum Zwischenspeichern von Daten

Informationen kann programmgesteuert in einer ASP.NET-Anwendung mithilfe einer Vielzahl von Ansätzen zwischengespeichert werden. Wir haben bereits gesehen, wie die in vorherigen Tutorials verwendet werden soll. Sie können auch Objekte können zwischengespeichert werden, programmgesteuert mithilfe von *statische Member* oder *Anwendungszustand*.

Bei der Arbeit mit einer Klasse muss in der Regel zunächst die Klasse instanziiert werden, bevor ihre Mitglieder zugegriffen werden können. Zum Aufrufen einer Methode von einer der Klassen in unserer Business Logic Layer, müssen wir beispielsweise zuerst eine Instanz der Klasse erstellen:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Bevor wir aktivieren können *SomeMethod* oder Arbeiten mit *SomeProperty*, erstellen wir zunächst eine Instanz der Klasse mit dem `New` Schlüsselwort. *SomeMethod* und *SomeProperty* sind mit einer bestimmten Instanz verknüpft. Die Lebensdauer dieser Member ist auf die Lebensdauer der ihr zugeordnete-Objekt gebunden. *Statische Member*, auf der anderen Seite sind Variablen, Eigenschaften und Methoden, die von gemeinsam genutzt werden *alle* Instanzen der Klasse und daher über eine Lebensdauer, die so lange wie die Klasse. Statische Member sind gekennzeichnet durch das Schlüsselwort `Shared`.

Zusätzlich zum statischen Elementen können Daten mithilfe von Anwendungsstatus zwischengespeichert werden. Jede ASP.NET-Anwendung verwaltet eine Name/Wert-Auflistung, s, die für alle Benutzer und die Seiten der Anwendung gemeinsam verwendet. Diese Auflistung kann zugegriffen werden, mithilfe der [ `HttpContext` Klasse](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` Eigenschaft](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx), und verwendet eine ASP.NET Seite s Code-Behind-Klasse wie folgt:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Das Datencache bietet eine viel umfassendere API zum Zwischenspeichern von Daten, die Mechanismen für die Zeit und Dependency-basierten Cacheobjekte, Prioritäten für Cache-Element und So weiter bereitstellen. Statische Member und Anwendungsstatus müssen solche Features manuell durch den Seitenentwickler hinzugefügt werden. Beim Zwischenspeichern von Daten beim Start der Anwendung für die Lebensdauer der Anwendung sind jedoch die Vorteile der Daten Cache s völlig irrelevant. In diesem Tutorial betrachten Code wir, die allen drei Techniken zum Zwischenspeichern von statischer Daten verwendet.

## <a name="step-3-caching-thesupplierstable-data"></a>Schritt 3: Zwischenspeichern der`Suppliers`-Tabellendaten

Der Datenbanktabellen wir Ve implementiert, um Datum Northwind enthalten keine herkömmlichen Nachschlagetabellen. Die vier DataTables, die in der DAL implementiert alle Tabellen im Modell, deren Werte nicht statisch sind. Anstatt die Zeit, die DAL, und klicken Sie dann eine neue Klasse und Methoden an die BLL, eine neue DataTable hinzufügen für dieses Tutorial s nur kann nehmen, die die `Suppliers` Tabellendaten s ist statisch. Aus diesem Grund können wir diese Daten beim Start der Anwendung Zwischenspeichern.

Erstellen Sie zunächst eine neue Klasse namens `StaticCache.cs` in die `CL` Ordner.


![Erstellen Sie die StaticCache.vb-Klasse im Ordner "CL"](caching-data-at-application-startup-vb/_static/image2.png)

**Abbildung 2**: Erstellen der `StaticCache.vb` -Klasse in der `CL` Ordner


Wir müssen eine Methode, die lädt die Daten beim Start in den entsprechenden Cachespeicher, als auch Methoden, die Daten aus diesem Cache zurückgeben.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Der obige Code verwendet eine statische Membervariable, `suppliers`, um die Ergebnisse von aufzunehmen der `SuppliersBLL` Klasse s `GetSuppliers()` -Methode, die aufgerufen wird die `LoadStaticCache()` Methode. Die `LoadStaticCache()` Methode während der Start der Anwendung s aufgerufen werden soll. Sobald diese Daten beim Start der Anwendung geladen wurde, kann einer beliebigen Seite, die zum Arbeiten mit Lieferantendaten Aufrufen der `StaticCache` Klasse s `GetSuppliers()` Methode. Aus diesem Grund erfolgt der Aufruf an die Datenbank zum Abrufen von Lieferanten nur, einmal beim Start der Anwendung.

Statt eine statische Membervariable wie des Cachespeicher zu verwenden, können wir auch Anwendungszustand oder in Datencache verwendet worden sein. Der folgende Code zeigt die Verwendung von Anwendungsstatus retooled-Klasse:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

In `LoadStaticCache()`, die Lieferanteninformationen werden gespeichert, auf die Anwendungsvariable *Schlüssel*. Diese e zurückgegeben wird, den entsprechenden Typ (`Northwind.SuppliersDataTable`) von `GetSuppliers()`. Während der Anwendungsstatus zugegriffen werden kann, in die CodeBehind-Klassen von ASP.NET-Seiten aus aufzurufen `Application("key")`, in der Architektur muss `HttpContext.Current.Application("key")` zum Abrufen des aktuellen `HttpContext`.

Ebenso kann das Datencache als Cachespeicher, wie im folgenden Code gezeigt verwendet werden:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Verwenden Sie zum Hinzufügen eines Elements, das dem Datencache ohne Ablaufdatum zeitbasierte der `System.Web.Caching.Cache.NoAbsoluteExpiration` und `System.Web.Caching.Cache.NoSlidingExpiration` Werte als Eingabeparameter. Durch diese jeweilige Überlastung des Datencaches s `Insert` Methode ausgewählt wurde, damit wir angeben könnten die *Priorität* für das Cacheelement. Die Priorität wird verwendet, um zu bestimmen, welche Elemente aus dem Cache löschen sind, wenn der Speicherplatz knapp wird. Hier verwenden wir die Priorität `NotRemovable`, die sicherstellt, dass dieses Element im Cache t gewonnen geleert werden.

> [!NOTE]
> Dieser Download Tutorial s implementiert die `StaticCache` -Klasse unter Verwendung des statischen Member-Variable-Ansatzes. Der Code für die Anwendung Anwendungszustands und der Cache-Techniken ist verfügbar in den Kommentaren in der Klassendatei.


## <a name="step-4-executing-code-at-application-startup"></a>Schritt 4: Ausführen von Code beim Start der Anwendung

Um Code auszuführen, wenn eine Webanwendung zuerst gestartet wird, müssen wir eine spezielle Datei namens erstellen `Global.asax`. Diese Datei kann Ereignishandler für die Anwendung-, Sitzungs-enthalten und Anforderungsebenen-Ereignisse und ist hier, in denen wir Code hinzufügen können, die ausgeführt wird, sobald die Anwendung gestartet wird.

Hinzufügen der `Global.asax` Datei in Ihrer Web-Anwendung s Root-Verzeichnis aus, indem mit der rechten Maustaste auf den Namen der Website-Projekt in Visual Studio-s-Projektmappen-Explorer und neues Element hinzufügen. Wählen Sie den Typ der Globale Anwendungsklasse-Element, und klicken Sie dann auf die Schaltfläche "hinzufügen", klicken Sie im Dialogfeld Neues Element hinzufügen.

> [!NOTE]
> Wenn Sie bereits haben eine `Global.asax` Datei in Ihrem Projekt, das globale Anwendungsklasse work Item Type wird nicht in das Dialogfeld "Neues Element hinzufügen" aufgeführt.


[![Die Datei "Global.asax" s-Stammverzeichnis Ihrer Web-Anwendung hinzufügen](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Abbildung 3**: Hinzufügen der `Global.asax` Datei auf Ihre Webanwendung s Stammverzeichnis ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-at-application-startup-vb/_static/image5.png))


Der Standardwert `Global.asax` Dateivorlage enthält fünf Methoden in einer serverseitigen `<script>` Tag:

- **`Application_Start`** Führt beim ersten der Webanwendung starten
- **`Application_End`** wird ausgeführt, wenn die Anwendung heruntergefahren wird
- **`Application_Error`** ausgeführt wird, wenn eine nicht behandelte Ausnahme die Anwendung erreicht.
- **`Session_Start`** wird ausgeführt, wenn eine neue Sitzung erstellt wird
- **`Session_End`** wird ausgeführt, wenn eine Sitzung abgelaufen oder wurde abgebrochen

Die `Application_Start` Ereignishandler nur einmal während des Lebenszyklus einer Anwendung s aufgerufen wird. Starten der Anwendung zum erste Mal eine ASP.NET-Ressource aus der Anwendung angefordert wird und weiterhin ausgeführt, bis die Anwendung neu gestartet wird, dies passieren kann, durch Ändern des Inhalts der `/Bin` Ordner ändern `Global.asax`, Ändern der Inhalt in die `App_Code` Ordner, oder ändern Sie die `Web.config` -Datei, andere Ursachen. Finden Sie unter [ASP.NET Application Life Cycle Overview](https://msdn.microsoft.com/library/ms178473.aspx) für eine ausführlichere Erläuterung für den Lebenszyklus der Anwendung.

Für diese Tutorials müssen wir nur zum Hinzufügen von Code die `Application_Start` -Methode, die so gerne anderen entfernen. In `Application_Start`, rufen Sie einfach die `StaticCache` Klasse s `LoadStaticCache()` -Methode, die laden und die Lieferanteninformationen zwischengespeichert wird:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Alles, was, s wird es! Beim Start der Anwendung die `LoadStaticCache()` -Methode die Lieferanteninformationen der BLL erfassen und speichern Sie es in eine statische Membervariable (oder den Cache zu speichern, Sie beendet werden, verwenden Sie der `StaticCache` Klasse). Um dieses Verhalten zu überprüfen, legen Sie einen Haltepunkt in der `Application_Start` Methode, und führen Sie Ihre Anwendung. Beachten Sie, dass der Haltepunkt beim Starten Anwendung erreicht wird. Nachfolgende Anforderungen führen jedoch nicht die `Application_Start` auszuführende Methode.


[![Verwenden Sie einen Haltepunkt, stellen Sie sicher, dass der Application_Start-Ereignishandler ausgeführt wird ist](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Abbildung 4**: Verwenden Sie einen Haltepunkt zu überprüfen, die die `Application_Start` -Ereignishandler ist ausgeführt wird ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Wenn Sie nicht erreichen, führen Sie die `Application_Start` Haltepunkt beim ersten Starten des Debuggens, es ist, da es sich bei Ihrer Anwendung bereits gestartet wurde. Erzwingen, dass die Anwendung neu starten, indem das Ändern Ihrer `Global.asax` oder `Web.config` Dateien, und versuchen Sie es dann erneut. Sie können einfach hinzufügen (eine leere Zeile am Ende eine dieser Dateien können Sie die Anwendung schnell neu starten oder entfernen).


## <a name="step-5-displaying-the-cached-data"></a>Schritt 5: Anzeigen der zwischengespeicherten Daten

An diesem Punkt die `StaticCache` -Klasse verfügt über eine Version von Lieferantendaten zwischengespeichert, die beim Start der Anwendung, die über zugegriffen werden kann die `GetSuppliers()` Methode. Um mit diesen Daten auf der Darstellungsschicht zu arbeiten, können wir ein ObjectDataSource-Steuerelement verwenden oder programmgesteuert Aufrufen der `StaticCache` Klasse s `GetSuppliers()` Methode von einer ASP.NET Seite s Code-Behind-Klasse. Lassen Sie s sehen Sie sich mit dem ObjectDataSource-Steuerelement und GridView-Steuerelemente zum Anzeigen der zwischengespeicherten Lieferanteninformationen.

Öffnen Sie zunächst die `AtApplicationStartup.aspx` auf der Seite die `Caching` Ordner. Ziehen Sie aus der Toolbox in den Designer und einer GridView-Ansicht der `ID` Eigenschaft `Suppliers`. Aus der GridView Smarttag s wählen Sie als Nächstes erstellen Sie eine neue, mit dem Namen "ObjectDataSource" `SuppliersCachedDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `StaticCache` Klasse s `GetSuppliers()` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der StaticCache-Klasse](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Abbildung 5**: Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `StaticCache` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-at-application-startup-vb/_static/image11.png))


[![Verwenden Sie die GetSuppliers()-Methode zum Abrufen der zwischengespeicherten Lieferantendaten](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Abbildung 6**: Verwenden der `GetSuppliers()` Methode, um die Lieferantendaten zwischengespeichert abzurufen ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-at-application-startup-vb/_static/image14.png))


Nach dem Fertigstellen des Assistenten für Visual Studio automatisch hinzugefügt BoundFields für die einzelnen Datenfelder in `SuppliersDataTable`. GridView und "ObjectDataSource" s deklarative Markup sollte in etwa wie folgt aussehen:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Abbildung 7 zeigt die Seite, wenn Sie über einen Browser angezeigt. Die Ausgabe ist gleich hatten wir Daten abgerufen, die von der BLL s `SuppliersBLL` -Klasse, aber mit der `StaticCache` -Klasse gibt die Lieferantendaten als zwischengespeicherte beim Start der Anwendung zurück. Sie können Haltepunkte festlegen, in der `StaticCache` Klasse s `GetSuppliers()` Methode, um dieses Verhalten zu überprüfen.


[![Die Lieferantendaten zwischengespeichert wird in einer GridView-Ansicht angezeigt.](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Abbildung 7**: die Lieferantendaten zwischengespeichert wird angezeigt, in einer GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Zusammenfassung

Die meisten jedes Datenmodell enthält eine viel statische Daten, die in der Regel in Form von Nachschlagetabellen implementiert. Da diese Informationen statisch, gibt es s gibt keinen Grund, die ständig auf die Datenbank jedes Mal zugreifen, diese Informationen angezeigt werden muss. Darüber hinaus aufgrund seiner statischer Natur beim Zwischenspeichern von Daten dort s keine Notwendigkeit einer ablaufangabe. In diesem Tutorial wurde erläutert, wie diese Daten nutzen, und in Datencache und Status der Anwendung, und über eine statische Membervariable zwischengespeichert. Diese Informationen werden beim Start der Anwendung zwischengespeichert und verbleiben im Cache während der Lebensdauer der Anwendung s.

In diesem Tutorial und die letzten beiden wir haben Zwischenspeichern von Daten für die Dauer der Lebensdauer der Anwendung s sowie zum Verwenden des zeitbasierten Cacheobjekte erläutert. Beim Zwischenspeichern von Daten, jedoch kann eine zeitbasierte Ablauf auf andere als ideal sein. Anstatt in regelmäßigen Abständen leeren des Cache, wäre es am besten mit nur das zwischengespeicherte Element entfernen, wenn die zugrunde liegenden Datenbankdaten geändert werden. Dieses Ideal ist möglich, durch die Verwendung von SQL-cacheabhängigkeiten, die untersuchen wir in unserem nächsten Tutorial.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Teresa Murphy und Zack Jones. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](caching-data-in-the-architecture-vb.md)
> [Weiter](using-sql-cache-dependencies-vb.md)
