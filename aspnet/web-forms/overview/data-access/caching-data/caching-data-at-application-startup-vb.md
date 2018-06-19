---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Zwischenspeichern von Daten beim Start der Anwendung (VB) | Microsoft Docs
author: rick-anderson
description: In einer beliebigen Webanwendung einige Daten häufig verwendet werden, und einige Daten nur selten verwendet werden. Wir können unsere ASP.NET Anwendung b leistungsverbesserung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: f8f322dae89480fc7ed5586d7f8eeb4c67d7839f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876344"
---
<a name="caching-data-at-application-startup-vb"></a>Zwischenspeichern von Daten beim Start der Anwendung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> In einer beliebigen Webanwendung einige Daten häufig verwendet werden, und einige Daten nur selten verwendet werden. Wir können unsere ASP.NET-Anwendung die Leistung verbessern, indem laden im Voraus häufig verwendete Daten, eine Technik, die als bekannt. Dieses Lernprogramm veranschaulicht eine Möglichkeit, proaktives laden, die zum Laden von Daten in den Cache beim Start der Anwendung ist.


## <a name="introduction"></a>Einführung

Zwischenspeichern von Daten in der Präsentations- und Zwischenspeichern von Ebenen in den beiden vorherigen Lernprogrammen betrachtet. In [Zwischenspeichern von Daten mit der ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), mit der caching-Funktionen zum Zwischenspeichern von Daten in der Darstellungsschicht ObjectDataSource s erläutert. [Zwischenspeichern von Daten in der Architektur](caching-data-in-the-architecture-vb.md) Zwischenspeichern in eine neue, separate Zwischenspeichern Ebene untersucht. Sowohl mit diesen Lernprogrammen verwendet *reaktive laden* bei der Arbeit mit dem Datencache. Beim reaktiven laden, jedes Mal, die die Daten angefordert werden, das System zuerst überprüft, ob es s im Cache. Wenn dies nicht der Fall, er ruft die Daten aus der ursprünglichen Quelle, z. B. die Datenbank, und klicken Sie dann im Cache gespeichert. Der Hauptvorteil reaktive geladen wurde, wird der einfachen Implementierung. Einer der Nachteile ist seine ungleichmäßige Leistung anforderungsübergreifend. Angenommen Sie, eine Seite, die die Caching-Ebene aus dem vorherigen Lernprogramm verwendet wird, um Produktinformationen anzuzeigen. Wenn diese Seite wird zum ersten Mal besucht oder besucht zum ersten Mal nach dem die zwischengespeicherten Daten aufgrund von arbeitsspeicherbeschränkungen oder dem angegebenen Ablaufdatum erreicht haben Clusterdefinition entfernt wurde, müssen die Daten aus der Datenbank abgerufen werden. Aus diesem Grund werden dieser Benutzer fordert dauert länger als Benutzer-Anforderungen, die bereitgestellt werden, können vom Cache.

*Proaktives laden* bietet eine alternative Cache Verwaltungsstrategie, glättet die speicherleistung anforderungsübergreifend durch Laden der zwischengespeicherten Daten, bevor sie s erforderlich sind. Proaktives Laden verwendet normalerweise einen Prozess, der regelmäßig überprüft oder wird benachrichtigt, wenn ein Update der zugrunde liegenden Daten stattgefunden hat. Dieser Vorgang aktualisiert dann den Cache, um den Lizenztoken aktuell zu halten. Proaktives Laden ist besonders nützlich, wenn die zugrunde liegenden Daten über eine langsame Verbindung, einen Webdienst oder eine andere besonders lange Datenquelle stammen. Dieser Ansatz ist jedoch die proaktive geladen wurde, schwieriger zu implementieren, erstellen, verwalten und Bereitstellen eines Prozesses, um auf Änderungen überprüfen und aktualisieren Sie den Cache benötigt wird.

Eine andere Art von proaktives laden und der Typ, den wir in diesem Lernprogramm untersuchen müssen wird Daten in den Cache beim Anwendungsstart geladen werden. Dieser Ansatz eignet sich besonders zum Zwischenspeichern von statischer Daten, z. B. die Datensätze in Datenbanktabellen für die Suche.

> [!NOTE]
> Eine eingehendere Betrachtung der Unterschiede zwischen proaktive und reaktive laden als auch Listen mit vor- und Nachteile Implementierung Empfehlungen finden Sie in der [Verwalten des Inhalts eines Caches](https://msdn.microsoft.com/library/ms978503.aspx) Teil der [ Handbuch zur Referenzarchitektur für .NET Framework-Anwendungen Caching](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Schritt 1: Bestimmen, welche Daten zum Cache beim Anwendungsstart

Das Zwischenspeichern Beispiele zur Verwendung der reaktive laden, die wir in den vorherigen zwei Lernprogramme Arbeit auch mit Daten, die ggf. in regelmäßigen Abständen ändern und nutzt nicht exorbitantly lang zum Generieren untersucht werden. Aber die zwischengespeicherten Daten nie ändert, Ablauf von reaktive Laden verwendet überflüssig ist. Ebenso, wenn die Daten zwischengespeichert generieren äußerst lange ausgeführt wird, klicken Sie dann diese Benutzer, deren Anforderungen zu suchen, der Cache leeren muss für eine lange warten, während die zugrunde liegenden Daten Sender, wird abgerufen. Erwägen Sie, Zwischenspeichern von statischen Daten sowie Daten, die eine außergewöhnlich lange dauert beim Start der Anwendung zu generieren.

Während Datenbanken viele dynamische besitzen, häufig geänderte Werte aufweisen, die meisten auch viel statische Daten. Nahezu alle Datenmodelle haben z. B. eine oder mehrere Spalten, die einen bestimmten Wert aus einen festen Satz von Optionen enthalten. Ein `Patients` Datenbanktabelle möglicherweise eine `PrimaryLanguage` Spalte, deren Werte konnte Englisch, Spanisch, Französisch, Russisch, Japanisch usw. sein. Häufig, werden diese Typen von Spalten mit implementiert *Nachschlagetabellen*. Anstatt das Speichern der Zeichenfolge Englisch oder Französisch in der `Patients` Tabelle, eine zweite Tabelle wird erstellt, die in der Regel zwei Spalten – ein eindeutiger Bezeichner und eine zeichenfolgenbeschreibung – mit einem Datensatz für jeden möglichen Wert verfügt. Die `PrimaryLanguage` Spalte in der `Patients` Tabelle speichert den entsprechenden Bezeichner in der Nachschlagetabelle. In Abbildung 1 ist Patienten John Doe s primäre Sprache Englisch, Ed Johnson s Russisch.


![Die Tabelle Sprachen verwenden, wird eine Suche Tabelle durch die Patienten-Tabelle](caching-data-at-application-startup-vb/_static/image1.png)

**Abbildung 1**: die `Languages` Tabelle verwenden, wird eine Suche Tabelle durch die `Patients` Tabelle


Die Benutzeroberfläche zum Bearbeiten oder erstellen einen neuen Patienten zählen eine Dropdown-Liste der zulässigen Sprachen ausgefüllt, indem die Datensätze in der `Languages` Tabelle. Ohne caching, besucht jedes Mal, die diese Schnittstelle ist das System muss die `Languages` Tabelle. Dies ist sehr aufwändig und unnötige da Tabellenwerte Suche sehr selten ändern falls überhaupt.

Wir konnten Zwischenspeichern der `Languages` Daten, die mit den gleichen reaktive laden Verfahren in den vorherigen Lernprogrammen untersucht. Reaktive geladen ist, verwendet jedoch eine zeitbasierte-Ablauf, die für die statischen Nachschlage-Tabellendaten nicht erforderlich ist. Beim Zwischenspeichern mit reaktive laden überhaupt kein Zwischenspeichern besser wäre, wäre der beste Ansatz die Tabellendaten für die Suche in den Cache beim Anwendungsstart proaktiv zu laden.

In diesem Lernprogramm werden wir das Tabellendaten für Cache-Suche und andere statische Informationen betrachten.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Schritt 2: Untersuchen die unterschiedlichen Möglichkeiten zum Zwischenspeichern von Daten

Informationen kann programmgesteuert in einer ASP.NET-Anwendung mithilfe einer Vielzahl von Ansätze zwischengespeichert werden. Wir haben bereits gesehen, wie den Datencache in vorherigen Lernprogrammen verwenden. Alternativ können Sie Objekte können zwischengespeichert werden, programmgesteuert mithilfe von *statische Member* oder *Anwendungsstatus*.

Bei der Arbeit mit einer Klasse muss in der Regel zunächst die Klasse instanziiert werden, bevor ihre Member zugegriffen werden können. Um eine Methode von einer der Klassen in unserer Business Logic Layer aufrufen, müssen wir zunächst eine Instanz der Klasse erstellen:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Bevor wir aufrufen können *SomeMethod* oder funktionieren mit *SomeProperty*, wir müssen zunächst erstellen Sie eine Instanz der Klasse unter Verwendung der `New` Schlüsselwort. *SomeMethod* und *SomeProperty* mit einer bestimmten Instanz verknüpft sind. Die Lebensdauer dieser Member wird mit der Lebensdauer der ihr zugeordnete-Objekt gebunden. *Statische Member*, andererseits, sind Variablen, Eigenschaften und Methoden, die gemeinsame *alle* Instanzen der Klasse und folglich über eine Lebensdauer, solange die Klasse. Statische Member sind gekennzeichnet durch das Schlüsselwort `Shared`.

Zusätzlich zum statischen Elementen können Daten mithilfe von Anwendungsstatus zwischengespeichert werden. Jede ASP.NET-Anwendung verwaltet einer Auflistung von Name-Wert, s, die alle Benutzer und die Seiten der Anwendung gemeinsam. Diese Auflistung kann zugegriffen werden, mithilfe der [ `HttpContext` Klasse](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` Eigenschaft](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx), und von einer ASP.NET Seite "s" Code-Behind-Klasse verwendet wie folgt:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Der Datencache bietet eine viel umfangreichere API zum Zwischenspeichern von Daten, die Mechanismen für Cacheobjekte Zeit und die Abhängigkeit basiert, Cache Element Prioritäten usw. bereitstellen. Statische Member und Anwendungsstatus müssen die solche Funktionen manuell vom Entwickler Seite hinzugefügt werden. Beim Zwischenspeichern von Daten beim Start der Anwendung für die Lebensdauer der Anwendung, allerdings sind die Daten Cache s Vorteile hinfällig. In diesem Lernprogramm sehen wir uns Code, die allen drei Techniken zum Zwischenspeichern von statischer Daten verwendet.

## <a name="step-3-caching-thesupplierstable-data"></a>Schritt 3: Zwischenspeichern der`Suppliers`Tabellendaten

Die Datenbanktabellen wir Ve bisheriges implementiert Northwind beinhalten keine herkömmlichen Nachschlagetabellen. Die vier DataTables implementiert in unserer DAL alle Tabellen im Modell, deren Werte nicht statisch sind. Anstatt die Zeit zum Hinzufügen einer neuen "DataTable" der DAL, und klicken Sie dann eine neue Klasse und Methoden, um die BLL Ausgaben für dieses Lernprogramm s an Ihre Seite nehmen an, die die `Suppliers` Tabellendaten s ist statisch. Aus diesem Grund können wir diese Daten beim Start der Anwendung Zwischenspeichern.

Um zu starten, erstellen Sie eine neue Klasse namens `StaticCache.cs` in den `CL` Ordner.


![Erstellen einer Klasse StaticCache.vb im Ordner "CL"](caching-data-at-application-startup-vb/_static/image2.png)

**Abbildung 2**: Erstellen der `StaticCache.vb` -Klasse in der `CL` Ordner


Hinzufügen einer Methode, die die Daten während des Starts in der entsprechenden Cachespeicher lädt sowie Methoden, die Daten aus diesen Cache zurückgeben müssen.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Der obige Code verwendet eine statische Membervariable `suppliers`zur Aufnahme der Ergebnisse von der `SuppliersBLL` Klasse s `GetSuppliers()` -Methode, die aufgerufen wird, aus der `LoadStaticCache()` Methode. Die `LoadStaticCache()` -Methode ist dafür vorgesehen, die während des Starts des s aufgerufen wird. Sobald diese Daten beim Anwendungsstart geladen wurden, kann eine beliebige Seite, die zur Bearbeitung von Lieferantendaten muss Aufrufen der `StaticCache` Klasse s `GetSuppliers()` Methode. Aus diesem Grund geschieht, den Aufruf an die Datenbank zum Abrufen von Lieferanten nur einmal am Anfang der Anwendung.

Statt eine statische Membervariable als Cachespeicher zu verwenden, könnte wir Alternativ Anwendungsstatus oder der Datencache verwendet haben. Der folgende Code zeigt die Verwendung des Anwendungszustands retooled Klasse:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

In `LoadStaticCache()`, die Lieferanteninformationen werden gespeichert, an die Anwendungsvariable *Schlüssel*. Er e zurückgegeben wird, in den entsprechenden Typ (`Northwind.SuppliersDataTable`) von `GetSuppliers()`. Während des Anwendungszustands zugegriffen werden kann, in den Code-Behind-Klassen von ASP.NET-Seiten aus aufzurufen `Application("key")`, in der Architektur wir verwenden `HttpContext.Current.Application("key")` zum Abrufen des aktuellen `HttpContext`.

Ebenso kann der Datencache als Cachespeicher, wie im folgenden Code gezeigt verwendet werden:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Verwenden Sie zum Hinzufügen eines Elements für den Datencache ohne Ablaufdatum zeitbasierte der `System.Web.Caching.Cache.NoAbsoluteExpiration` und `System.Web.Caching.Cache.NoSlidingExpiration` Werte als Eingabeparameter. Diese bestimmte Überladung der Datencache s `Insert` Methode ausgewählt wurde, damit wir angeben konnte die *Priorität* des Cacheelements. Die Priorität wird verwendet, um zu bestimmen, welche Elemente aus dem Cache Räumen auf, wenn der Speicherplatz knapp wird. Hier verwenden wir die Priorität `NotRemovable`, wodurch sichergestellt wird, dass dieses Element im Cache/t gewonnen Anwendungscache gelöscht werden.

> [!NOTE]
> Dieses Lernprogramm s Download implementiert die `StaticCache` -Klasse unter Verwendung des statischen Member-Variable-Ansatzes. Der Code für die Anwendung Anwendungszustands und der Cache-Techniken steht in den Kommentaren in der Klassendatei.


## <a name="step-4-executing-code-at-application-startup"></a>Schritt 4: Ausführen von Code beim Start der Anwendung

Um Code auszuführen, wenn eine Webanwendung zuerst gestartet wird, müssen wir erstellen eine spezielle Datei namens `Global.asax`. Diese Datei kann Ereignishandler für die Anwendung-, Session-enthalten, und Anforderungsebenen-Ereignisse, und es ist hier, in denen wir Code hinzufügen können, die bei jedem der Anwendung Start ausgeführt wird.

Hinzufügen der `Global.asax` Datei in Ihrer Web-Anwendung s Root-Verzeichnis aus, indem Sie mit der rechten Maustaste auf den Namen der Website-Projekt in Visual Studio s. Projektmappen-Explorer und neues Element hinzufügen. Klicken Sie im Dialogfeld Neues Element hinzufügen wählen Sie die Globale Anwendungsklasse Elementtyp aus, und klicken Sie dann auf die Schaltfläche "hinzufügen".

> [!NOTE]
> Falls Sie noch eine `Global.asax` Datei in Ihrem Projekt, das globale Anwendungsklasse work Item Type wird nicht in das Dialogfeld "Neues Element hinzufügen" aufgeführt.


[![Die Datei "Global.asax" s-Stammverzeichnis der Web-Anwendung hinzufügen](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Abbildung 3**: Hinzufügen der `Global.asax` Datei in der Webanwendung s Stammverzeichnis ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-at-application-startup-vb/_static/image5.png))


Die Standardeinstellung `Global.asax` Vorlage enthält fünf Methoden in einer serverseitigen `<script>` Tag:

- **`Application_Start`** Führt beim Starten der Webanwendung
- **`Application_End`** wird ausgeführt, wenn die Anwendung heruntergefahren wird
- **`Application_Error`** wird immer ausgeführt, wenn eine nicht behandelte Ausnahme die Anwendung erreicht.
- **`Session_Start`** wird ausgeführt, wenn eine neue Sitzung erstellt wird
- **`Session_End`** wird ausgeführt, wenn eine Sitzung abgelaufen ist oder abgebrochen wird

Die `Application_Start` Ereignishandler wird nur einmal während des Lebenszyklus einer Anwendung s aufgerufen. Starten der Anwendung zum erste Mal eine ASP.NET-Ressource aus der Anwendung angefordert wird und weiterhin ausgeführt, bis die Anwendung neu gestartet wird, dies geschehen kann, durch Ändern des Inhalts der `/Bin` Ordner ändern `Global.asax`, Ändern der den Inhalt der `App_Code` Ordner, oder ändern Sie die `Web.config` -Datei zwischen hat andere Ursachen. Verweisen auf [Übersicht über ASP.NET den Lebenszyklus](https://msdn.microsoft.com/library/ms178473.aspx) für eine ausführlichere Erläuterung im Lebenszyklus Anwendung.

Für diese Lernprogramme müssen wir nur zum Hinzufügen von Code die `Application_Start` Methode, die so gerne So entfernen Sie die anderen. In `Application_Start`, rufen Sie einfach die `StaticCache` Klasse s `LoadStaticCache()` -Methode, die geladen und der Lieferanteninformationen zwischenzuspeichern:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

S alles ist es! Beim Start der Anwendung die `LoadStaticCache()` -Methode ziehen Sie die Lieferanteninformationen aus der BLL und speichern Sie sie in einer statischen Membervariablen gespeichert (oder den Cache zu speichern, Sie verwenden in sah die `StaticCache` Klasse). Um dieses Verhalten zu überprüfen, legen Sie einen Haltepunkt der `Application_Start` Methode, und führen Sie die Anwendung. Beachten Sie, dass der Haltepunkt beim Starten Anwendung erreicht wird. Nachfolgende Anforderungen führen jedoch nicht die `Application_Start` Methode ausgeführt.


[![Verwenden Sie einen Haltepunkt, stellen Sie sicher, dass der Application_Start-Ereignishandler ausgeführt wird](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Abbildung 4**: Verwenden Sie einen Haltepunkt zu überprüfen, die die `Application_Start` -Ereignishandler wird gerade ausgeführt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Wenn Sie nicht erreicht, führen Sie die `Application_Start` Haltepunkt beim ersten Starten des Debuggens ist, da die Anwendung bereits begonnen wurde. Erzwingen Sie die Anwendung neu starten, indem Sie ändern die `Global.asax` oder `Web.config` Dateien, und wiederholen Sie den Vorgang. Sie können einfach hinzufügen (eine leere Zeile am Ende eine dieser Dateien können Sie die Anwendung schnell neu starten oder entfernen).


## <a name="step-5-displaying-the-cached-data"></a>Schritt 5: Anzeigen der zwischengespeicherten Daten

An diesem Punkt der `StaticCache` -Klasse verfügt über eine Version von Lieferantendaten, die beim Start der Anwendung, die über zugegriffen werden kann zwischengespeichert seine `GetSuppliers()` Methode. Zum Arbeiten mit diesen Daten aus der Darstellungsschicht wir verwenden eine ObjectDataSource oder programmgesteuert aufrufen können die `StaticCache` Klasse s `GetSuppliers()` Methode von einer ASP.NET Seite "s" Code-Behind-Klasse. Lassen Sie s sehen Sie sich mit den ObjectDataSource und GridView-Steuerelementen zum Anzeigen der zwischengespeicherten Lieferanteninformationen.

Öffnen Sie zunächst die `AtApplicationStartup.aspx` auf der Seite der `Caching` Ordner. Ziehen Sie eine GridView aus der Toolbox in den Designer, Festlegen seiner `ID` Eigenschaft `Suppliers`. Aus der GridView Smarttag s wählen Sie als Nächstes erstellen Sie eine neue, mit dem Namen ObjectDataSource `SuppliersCachedDataSource`. Konfigurieren der ObjectDataSource verwenden die `StaticCache` Klasse s `GetSuppliers()` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der StaticCache-Klasse](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Abbildung 5**: Konfigurieren der ObjectDataSource verwenden die `StaticCache` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-at-application-startup-vb/_static/image11.png))


[![Verwenden Sie die GetSuppliers()-Methode zum Abrufen der zwischengespeicherten Lieferantendaten](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Abbildung 6**: Verwenden der `GetSuppliers()` Methode, um die Lieferantendaten zwischengespeichert abzurufen ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-at-application-startup-vb/_static/image14.png))


Nach Abschluss des Assistenten für Visual Studio automatisch hinzugefügt BoundFields für die einzelnen Datenfelder in `SuppliersDataTable`. GridView und ObjectDataSource s deklarative Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Abbildung 7 zeigt die Seite, wenn Sie über einen Browser angezeigt. Die Ausgabe entspricht dem hatten wir die Daten aus den s BLL abgerufen `SuppliersBLL` Klasse, aber mithilfe der `StaticCache` Klasse gibt die Lieferantendaten als zwischengespeicherten beim Anwendungsstart. Sie können Haltepunkte festlegen, der `StaticCache` Klasse s `GetSuppliers()` Methode, um dieses Verhalten zu überprüfen.


[![Die Lieferantendaten zwischengespeichert wird in einem GridView angezeigt.](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Abbildung 7**: die Lieferantendaten zwischengespeichert wird angezeigt, in einem GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Zusammenfassung

Die meisten jedes Datenmodell enthält eine viel statische Daten, die in der Regel in Form von Nachschlagetabellen implementiert. Da diese Informationen statisch ist, ist dort s keinen Grund, ständig die Datenbank bei jedem Zugriff auf diese Informationen angezeigt werden muss. Darüber hinaus aufgrund seiner statischer Natur hat beim Zwischenspeichern von Daten dort s keine Notwendigkeit, ein Ablaufdatum. In diesem Lernprogramm wurde erläutert, wie solche Daten und speichert sie in der Datencache Anwendungsstatus, und über eine statische Membervariable zwischen. Diese Informationen werden beim Start der Anwendung zwischengespeichert und verbleiben im Cache während der Lebensdauer der Anwendung s.

In diesem Lernprogramm und den letzten beiden wir haben erläutert, das Zwischenspeichern von Daten für die Dauer der die Lebensdauer der Anwendung s als auch einen zeitbasierten Cacheobjekte. Beim Zwischenspeichern von Datenbankdaten, jedoch kann eine zeitbasierte Ablauf auf weniger als ideal erweisen. Anstatt in regelmäßigen Abständen leeren des Caches, wäre es am besten, nur das zwischengespeicherte Element entfernen, wenn Daten in der zugrunde liegenden Datenbank geändert werden. Dieses Ideal ist möglich, durch die Verwendung von SQL-Cache-Abhängigkeiten, die untersuchen wir in unserer nächsten Lernprogramm.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Teresa Murphy und Zack Jones. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](caching-data-in-the-architecture-vb.md)
> [Weiter](using-sql-cache-dependencies-vb.md)
