---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximieren der Leistung mit Entity Framework 4.0 in eine ASP.NET 4-Webanwendung | Microsoft Docs
author: tdykstra
description: Diese Reihe von Lernprogrammen baut auf der Contoso-University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0 Tutorial Reihe erstellt wird. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: b85645eebf2822b33df944692736ea9d9b69b9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891411"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximieren der Leistung mit Entity Framework 4.0 in eine ASP.NET 4-Webanwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Diese Reihe von Lernprogrammen in der Contoso-University Webanwendung durch die erstellte builds der [erste Schritte mit dem Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Reihe von Lernprogrammen. Wenn Sie die frühere Lernprogramme nicht abgeschlossen wurde, als Ausgangspunkt für dieses Lernprogramm können Sie [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die durch das vollständige Lernprogramm Reihe erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie stellen Sie diese auf die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Lernprogramm haben Sie gesehen, wie Parallelitätskonflikte behandelt. Dieses Lernprogramm zeigt die Optionen zum Optimieren der Leistung einer ASP.NET Web-Anwendung, die das Entity Framework verwendet. Lernen Sie verschiedene Methoden zum Maximieren der Leistung oder für die Diagnose von Leistungsproblemen.

In den folgenden Abschnitten aufgeführten Informationen stellen ist sehr wahrscheinlich in einer Vielzahl von Szenarien nützlich sein:

- Laden Sie verwandte Daten effizient.
- Verwalten Sie Ansichtszustand.

In den folgenden Abschnitten aufgeführten Informationen stellen möglicherweise nützlich, wenn Sie einzelne Abfragen, Leistungsprobleme vorhanden:

- Verwenden der `NoTracking` Zusammenführungsoption.
- Kompilieren Sie vorab LINQ-Abfragen.
- Überprüfen Sie die Abfragebefehle, die an die Datenbank gesendet.

Im folgenden Abschnitt aufgeführten Informationen stellen ist möglicherweise nützlich für Anwendungen, die sehr große Datenmodelle aufweisen:

- Vorabgenerieren von Sichten.

> [!NOTE]
> Leistung der Webanwendung wird von vielen Faktoren ab, einschließlich der Größe der Anforderungs-und Antwortdaten, die Geschwindigkeit von Datenbankabfragen, wie viele Anforderungen, dass der Server in die Warteschlange kann und wie schnell sie bedienen kann, und auch die Effizienz aller beeinflusst. Bibliotheken mit Clientskripts, die Sie verwenden können. Wenn Leistung in Ihrer Anwendung wichtig ist, oder testen oder Erfahrung zeigt, dass die Leistung der Anwendung nicht zufriedenstellend ist, sollten Sie die normale Protokoll für die leistungsoptimierung folgen. Um zu bestimmen, wo Leistungsengpässe auftreten zu messen Sie, und beheben Sie die Bereiche, die die größte Auswirkung auf die gesamtleistung der Anwendung haben.
> 
> Dieses Thema konzentriert sich hauptsächlich auf die Weise, in denen Sie potenziell die Leistung von Entity Framework in ASP.NET ausdrücklich verbessern können. Hier die Vorschläge sind nützlich, wenn Sie feststellen, dass Zugriff auf Daten für Leistungsengpässe in der Anwendung wird. Außer wie bereits erwähnt, die Methoden, die hier erläuterten berücksichtigt werden sollten nicht &quot;bewährte Methoden&quot; im Allgemeinen – viele von ihnen sind nur in Ausnahmesituationen oder Adresse sehr bestimmte Arten von Leistungsengpässe geeignet.


Um das Lernprogramm zu starten, starten Sie Visual Studio, und öffnen Sie die Contoso University-Web-Anwendung, die Sie im vorherigen Lernprogramm gearbeitet haben.

## <a name="efficiently-loading-related-data"></a>Effizientes Laden von verknüpften Daten

Es gibt mehrere Möglichkeiten, das Entity Framework in die Navigationseigenschaften einer Entität verknüpfte Daten geladen werden können:

- *Lazy Loading (verzögertes Laden)*. Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen. Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt zu mehreren Abfragen an die Datenbank gesendet – eine für die Entität selbst und eine jedes Mal, die Daten, die für die Entität verknüpft abgerufen werden muss. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Eager Loading (vorzeitiges Laden)*. Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen. Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind. Geben Sie unverzüglichem Laden mithilfe der `Include` -Methode, wie Sie haben bereits gesehen in diesen Lernprogrammen.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Explizites Laden*. Dies gleicht dem verzögerten Laden, mit dem Unterschied, dass Sie explizit die verknüpften Daten im Code abrufen; Es ist nicht automatisch ausgeführt, wenn Sie Zugriff auf eine Navigationseigenschaft. Laden Sie die verwandte Daten, die manuell mithilfe der `Load` Methode der Navigationseigenschaft für Auflistungen, oder Sie verwenden die `Load` -Methode des Reference (Eigenschaft) für Eigenschaften, die ein einzelnes Objekt enthalten. (Beispielsweise, rufen Sie die `PersonReference.Load` -Methode zum Laden der `Person` Navigationseigenschaft eine `Department` Entität.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Da sie nicht die Eigenschaftswerte sofort abrufen, verzögertes Laden und explizites Laden sind auch beide genannt *verzögertes Laden*.

Verzögertes Laden ist das Standardverhalten bei einem Objektkontext, der vom Designer generiert wurde. Wenn Sie öffnen die *SchoolModel.Designer.cs* Datei, die die Kontext-Objektklasse definiert, finden Sie drei Konstruktormethoden und jede von ihnen die folgende Anweisung beinhaltet:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Im Allgemeinen sollten Sie benötigen Sie verknüpfte Daten für jede Entität, die abgerufen werden, unverzüglichem Laden die beste Leistung bietet, da eine einzelne Abfrage, die an die Datenbank gesendet, in der Regel effizienter als separate Abfragen für jede Entität abgerufen wird. Andererseits, wenn Sie eine Entität Navigationseigenschaften nur selten zugreifen müssen oder nur für einen kleinen Satz von Entitäten, verzögertes Laden oder explizites Laden effizienter, möglicherweise unverzüglichem Laden mehr Daten als benötigt abrufen würde.

In einer Webanwendung verzögertes Laden von relativ kleinen Wert trotzdem, möglicherweise daran, dass Benutzeraktionen, die sich auf die Notwendigkeit von verknüpften Daten im Browser stattfinden, die keine Verbindung mit dem Objektkontext verfügt, die die Seite gerendert. Andererseits, wenn Sie Databind ein Steuerelement Sie in der Regel, welche Daten wissen Sie benötigen, und daher im Allgemeinen es ist am besten auf unverzüglichem Laden oder verzögertes Laden basierend auf eignet sich wie in jedem Szenario.

Darüber hinaus können einem datengebundenen Steuerelement auf ein Entitätsobjekt, nach der Objektkontext verworfen wird. In diesem Fall würde ein Versuch, eine Navigationseigenschaft-verzögerte Laden fehl. Die Fehlermeldung, die ausgehändigt ist eindeutig: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Die `EntityDataSource` Steuerelement verzögertes Laden standardmäßig deaktiviert. Für die `ObjectDataSource` Steuerelement, dass Sie für das aktuelle Lernprogramm verwenden (oder wenn Sie den Objektkontext von Seitencode zugreifen), es gibt mehrere Möglichkeiten, die möglich verzögerten Laden, die standardmäßig deaktiviert. Wenn Sie einen Objektkontext instanziieren, können Sie es deaktivieren. Sie können z. B. die folgende Zeile hinzufügen, an die Konstruktormethode des der `SchoolRepository` Klasse:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Treffen Sie für die Anwendung Contoso University Objektkontext automatisch deaktivieren Sie lazy loading so, dass diese Eigenschaft nicht festgelegt werden, wenn ein Kontext instanziiert wird.

Öffnen der *SchoolModel.edmx* Daten modellieren, klicken Sie auf der Entwurfsoberfläche, und legen Sie dann im Bereich "Eigenschaften" die **verzögerten Laden aktiviert** Eigenschaft `False`. Speichern Sie und schließen Sie das Datenmodell.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Verwalten des Ansichtszustands

Um Update-Funktionalität zu gewährleisten, muss eine ASP.NET-Webseite die ursprünglichen Eigenschaftswerte einer Entität speichern, wenn eine Seite gerendert wird. Während der Verarbeitung des Steuerelements Postback können, den ursprünglichen Zustand der Entität erneut erstellen, und rufen Sie der Entität `Attach` -Methode vor dem Übernehmen von Änderungen und zum Aufrufen der `SaveChanges` Methode. Standardmäßig verwenden die ASP.NET Web Forms-Steuerelemente Ansichtszustand auf um die ursprünglichen Werte zu speichern. Ansichtszustand kann jedoch die Leistung beeinträchtigen, da sie in ausgeblendeten Feldern gespeichert ist, die im Wesentlichen die Seitengröße erhöhen können, die verschiedene andere und aus den Browser gesendet wird.

Techniken für die Verwaltung von Ansichtszustand oder alternativen, wie z. B. Sitzungsstatus, nicht nur für das Entity Framework, damit dieses Lernprogramm in diesem Thema im Detail gehen nicht. Weitere Informationen finden Sie unter den Links am Ende des Lernprogramms.

Version 4 von ASP.NET bietet jedoch eine neue Methode mit den Ansichtszustand, die jeder Entwickler ASP.NET Web Forms-Anwendungen bewusst sein sollten zu arbeiten: die `ViewStateMode` Eigenschaft. Diese neue Eigenschaft kann auf der Seite oder eines Steuerelements Ebene festgelegt werden, und er ermöglicht es Ihnen, Ansichtszustand standardmäßig für eine Seite zu deaktivieren und aktivieren Sie ihn nur für Steuerelemente, die sie benötigen.

Für Anwendungen, in denen die Leistung kritisch ist, empfiehlt sich immer Ansichtszustand auf Seitenebene deaktivieren und aktivieren Sie ihn nur für Steuerelemente, die dies erfordern. Die Größe des Anzeigestatus in der Contoso-University Seiten wäre nicht durch diese Methode erheblich verbessert werden, aber um anzuzeigen, wie es funktioniert, müssen Sie es für Ausführen der *Instructors.aspx* Seite. Diese Seite enthält viele Steuerelemente, einschließlich einer `Label` -Steuerelement mit Ansichtszustand deaktiviert. Keines der Steuerelemente auf dieser Seite müssen tatsächlich Ansicht Zustand aktiviert haben. (Die `DataKeyNames` Eigenschaft von der `GridView` Steuerelement gibt Status, die zwischen Postbacks beibehalten werden muss, aber diese Werte im Steuerelementzustand, die von betroffen wird nicht beibehalten werden die `ViewStateMode` Eigenschaft.)

Die `Page` Richtlinie und `Label` Steuerelementmarkup derzeit im folgende Beispiel ähnelt:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Die folgenden Änderungen vornehmen:

- Hinzufügen `ViewStateMode="Disabled"` auf die `Page` Richtlinie.
- Entfernen Sie `ViewStateMode="Disabled"` aus der `Label` Steuerelement.

Das Markup sieht nun wie im folgende Beispiel:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Der Ansichtszustand ist nun für alle Steuerelemente deaktiviert. Wenn Sie später ein Steuerelement hinzufügen, der Ansichtszustand zu verwenden, müssen Sie, lediglich enthalten die `ViewStateMode="Enabled"` Attribut für dieses Steuerelement.

## <a name="using-the-notracking-merge-option"></a>Mithilfe der NoTracking-MergeOption

Bei ein Objektkontext Datenbankzeilen abgerufen, und erstellt von Entitätsobjekten, die sie darstellen, nachverfolgt standardmäßig ebenfalls diese Entitätsobjekten, die mit der Objektstatus-Manager. Diese Verfolgungsdaten als Cache fungiert und werden verwendet, wenn Sie eine Entität aktualisieren. Da eine Webanwendung, die in der Regel kurzlebiges Objekt Kontext Instanzen verfügt, wird in Abfragen häufig Daten zurückgeben, die nicht verfolgt werden müssen, da es sich bei der Objektkontext, der sie liest verworfen wird, bevor eine beliebige Entität gelesenen erneut verwendet werden oder aktualisiert.

Im Entity Framework können Sie angeben, ob der Objektkontext Entitätsobjekte, durch Festlegen verfolgt einer *Zusammenführungsoption*. Sie können der MergeOption für einzelne Abfragen oder Entitätenmengen festlegen. Wenn Sie für eine Entitätssammlung festlegen, bedeutet, dass an, dass Sie die Standardzusammenführungsoption für alle Abfragen festlegen, die für die Entitätenmenge erstellt werden.

Überwachung nicht benötigt für die Anwendung Contoso University für keines der Entitätenmengen, die Sie aus dem Repository zu erreichen, daher Sie die MergeOption, um festlegen können `NoTracking` für diese Entitätenmengen beim Instanziieren der Objektkontext in der Repository-Klasse. (Beachten Sie, dass in diesem Lernprogramm Festlegen der MergeOption eine spürbare Auswirkung auf die Leistung der Anwendung nicht. Die `NoTracking` ist wahrscheinlich nur in bestimmten Szenarien hohe Datenvolumen ein Observable leistungsverbesserung vornehmen.)

Öffnen Sie im Ordner "DAL" die *SchoolRepository.cs* Datei, und fügen Sie eine Konstruktormethode, die der MergeOption festlegt, für die Entität legt fest, dass das Repository zugreift:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Vorab Kompilieren von LINQ-Abfragen

Beim ersten, der das Entity Framework innerhalb der Lebensdauer eine Entity SQL-Abfrage ausgeführt wird eine bestimmte `ObjectContext` Instanz, dauert es einige Zeit zum Kompilieren der Abfrage. Das Ergebnis der Kompilierung wird zwischengespeichert, was bedeutet, dass nachfolgende Ausführungen der Abfrage wesentlich schneller, da sind. LINQ-Abfragen folgen ein ähnlichen Muster, mit dem Unterschied, dass einige der den Arbeitsaufwand beim Kompilieren der Abfrage erfolgt jedes Mal, wenn die Abfrage ausgeführt wird. Das heißt, sind für LINQ-Abfragen werden standardmäßig nicht alle Ergebnisse der Kompilierung zwischengespeichert.

Wenn Sie eine LINQ-Abfrage, die Sie wahrscheinlich in einem Objektkontext Leben wiederholt ausführen verfügen, können Sie Code schreiben, die bewirkt, dass alle Kompilierungsergebnisse zum ersten Mal zwischengespeichert werden, die LINQ-Abfrage ausgeführt wird.

Als eine Abbildung dieser Schritt erfolgt für zwei `Get` Methoden in der `SchoolRepository` -Klasse, von denen keine Parameter annehmen (der `GetInstructorNames` Methode), und eine, die einen Parameter erforderlich ist (die `GetDepartmentsByAdministrator` Methode). Diese Methoden müssen wie diese jetzt tatsächlich nicht kompiliert werden, da sie LINQ-Abfragen nicht vorhanden sind:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Allerdings, damit Sie die kompilierte Abfragen ausprobieren können, müssen Sie fortgesetzt, als ob diese als die folgenden LINQ-Abfragen geschrieben:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Ändern Sie den Code in diesen Methoden konnten Sie, was oben gezeigten hat, und führen Sie die Anwendung aus, um sicherzustellen, dass es, bevor Sie fortfahren funktioniert. Aber die folgenden Anweisungen stürzen vorkompilierte Versionen davon erstellen.

Erstellen Sie eine Klassendatei in der *DAL* Ordner, nennen Sie sie *SchoolEntities.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Dieser Code erstellt eine partielle Klasse, die automatisch generierte Objekt Context-Klasse erweitert. Die partielle Klasse schließt zwei kompilierte LINQ-Abfragen, die mithilfe der `Compile` Methode der `CompiledQuery` Klasse. Es erstellt auch Methoden, die Sie verwenden können, die Abfragen aufrufen. Speichern Sie und schließen Sie diese Datei.

Im nächsten Schritt in *SchoolRepository.cs*, ändern Sie den vorhandenen `GetInstructorNames` und `GetDepartmentsByAdministrator` Methoden im Repository Klasse, sodass sie die kompilierte Abfragen aufrufen:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Führen Sie die *Departments.aspx* Seite, um sicherzustellen, dass er funktioniert es hat sich nichts geändert. Die `GetInstructorNames` Methode wird aufgerufen, um die Dropdownliste Administrator Auffüllen und die `GetDepartmentsByAdministrator` Methode wird aufgerufen, wenn Sie auf **Update** um sicherzustellen, dass keine Dozenten Administrator von mehreren ist Abteilung.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Sie haben bereits kompilierte Abfragen in der Contoso-University Anwendung nur, wie Sie dies durchführen, finden Sie unter nicht verwendet werden, da es messbar leistungsverbesserung. Vorkompilieren von LINQ-Abfragen Code ein Maß an Komplexität hinzugefügt haben, achten Sie darauf, dass er nur für Abfragen verwenden, die tatsächlich Leistungsengpässe in der Anwendung darstellen.

## <a name="examining-queries-sent-to-the-database"></a>Untersuchung von Abfragen an die Datenbank gesendet

Wenn Sie Leistungsprobleme untersuchen, ist es manchmal hilfreich zu wissen, die genaue SQL-Befehle, die das Entity Framework in die Datenbank sendet. Bei Verwendung mit einem `IQueryable` Objekt eine Möglichkeit hierfür ist die Verwendung der `ToTraceString` Methode.

In *SchoolRepository.cs*, ändern Sie den Code in die `GetDepartmentsByName` Methode, um die in folgendem Beispiel dargestellt:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Die `departments` Variablen umgewandelt werden muss, um eine `ObjectQuery` geben nur möglich, weil die `Where` Methode am Ende der vorhergehenden Zeile erstellt eine `IQueryable` Objekt; ohne die `Where` -Methode, die Umwandlung nicht erforderlich.

Festlegen eines Haltepunkts auf die `return` Zeile, und führen Sie die *Departments.aspx* Seite im Debugger. Wenn den Haltepunkt erreicht wird, untersuchen die `commandText` -Variable in der **"lokal"** Fenster und verwenden Sie die Text-Schnellansicht (das Lupensymbol in der **Wert** Spalte), deren Wert in der anzuzeigen**Text-Schnellansicht** Fenster. Sie können die gesamten SQL-Befehl anzeigen, der aus diesem Code ergibt:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Als Alternative bietet das IntelliTrace-Feature in Visual Studio Ultimate eine Möglichkeit zum Anzeigen von SQL-Befehle, die vom Entity Framework, die Sie ändern den Code oder sogar einen Haltepunkt festlegen, erfordern nicht generiert.

> [!NOTE]
> Sie können die folgenden Verfahren ausführen, nur, wenn Sie Visual Studio Ultimate haben.


Wiederherstellen des ursprünglichen Codes in der `GetDepartmentsByName` -Methode, und führen Sie die *Departments.aspx* Seite im Debugger.

Wählen Sie in Visual Studio die **Debuggen** klicken Sie dann im Menü **IntelliTrace**, und klicken Sie dann **IntelliTrace-Ereignisse**.

[![image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

In der **IntelliTrace** Fenster, klicken Sie auf **alle unterbrechen**.

[![image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Die **IntelliTrace** Fenster zeigt eine Liste der aktuellsten Ereignisse:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klicken Sie auf die **ADO.NET** Zeile. Es wird erweitert, um Sie den Befehlstext anzeigen:

[![image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Sie können den gesamten Text Befehlszeichenfolge kopieren, in die Zwischenablage aus der **"lokal"** Fenster.

Angenommen, Sie mit einer Datenbank mit Weitere Tabellen, Beziehungen und Spalten als das einfache arbeiteten `School` Datenbank. Sie möglicherweise feststellen, dass es sich bei einer Abfrage, die alle Informationen erfasst müssen Sie in einer einzelnen `Select` Anweisung mit mehreren `Join` Klauseln wird für die effiziente Zusammenarbeit zu komplex. In diesem Fall können Sie von Eager laden zum expliziten Laden zur Vereinfachung der Abfrage wechseln.

Versuchen Sie es z. B. mit dem Ändern des Codes in der `GetDepartmentsByName` Methode im *SchoolRepository.cs*. Derzeit darin, dass Methode Sie eine Objektabfrage haben, hat `Include` Methoden für die `Person` und `Courses` Navigationseigenschaften. Ersetzen Sie die `return` -Anweisung mit Code, der explizites Laden ausführt, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Führen Sie die *Departments.aspx* Seite im Debugger aus, und überprüfen Sie die **IntelliTrace** Fenster wieder wie gewohnt. Jetzt, obwohl Sie eine einzelne Abfrage vor dem vorhanden war, sehen Sie eine lange Abfolge von ihnen.

[![image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klicken Sie auf der ersten **ADO.NET** Zeile, um Näheres zu komplexen Abfrage Sie wurde zuvor angezeigt.

[![image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Die Abfrage von Abteilungen geworden ist eine einfache `Select` Abfragen ohne `Join` -Klausel, aber es folgt eine eigene Abfrage, die verwandte Kurse und ein Administrator abrufen, die ursprüngliche zurückgegebenes mithilfe eines Satzes von zwei Abfragen für jede Abteilung Abfrage.

> [!NOTE]
> Wenn Sie verzögerte lassen kann aktiviert, laden das Muster, das Sie mit der gleichen Abfrage mehrere Male wiederholt sehen, verhindert das verzögerte Laden von kommen. Ein Muster, das Sie in der Regel vermeiden möchten, ist verzögertes Laden von verknüpften Daten für jede Zeile der primären Tabelle. Es sei denn, Sie überprüft haben, dass eine einzelne Join-Abfrage effizient zu komplex wird, würden Sie in der Regel in solchen Fällen verbessern, indem Sie die Änderung der primären Abfrage unverzüglichem Laden verwendet werden.


## <a name="pre-generating-views"></a>Vorab Generieren von Sichten

Wenn ein `ObjectContext` Objekt zuerst in einer neuen Anwendungsdomäne erstellt wird, Entity Framework generiert einen Satz von Klassen, die für den Datenbankzugriff verwendet. Diese Klassen heißen *Ansichten*, und wenn Sie ein Modell sehr umfangreiche Daten verfügen, Generierung dieser Ansichten kann verzögern der Website als Antwort auf die erste Anforderung für eine Seite nach eine neuen Anwendungsdomäne initialisiert wird. Sie können diese Verzögerung der ersten Anforderung reduzieren, indem Sie die Ansichten zur Kompilierzeit statt zur Laufzeit erstellen.

> [!NOTE]
> Wenn Ihre Anwendung einen extrem großen Datenmodell besitzt oder einem großen Datenmodell verfügt über, aber Sie nicht Bedenken bezüglich eines Leistungsproblems hindeuten, die nur die erste Seitenanforderung wirkt sich auf, nachdem IIS wiederverwendet wird, können Sie diesen Abschnitt überspringen. Anzeigen, die Erstellung nicht jedes Mal, wenn Sie instanziieren zieht ein `ObjectContext` Objekt, da die Ansichten in der Anwendungsdomäne zwischengespeichert werden. Aus diesem Grund, es sei denn, Sie häufig Ihre Anwendung in IIS wiederverwenden möchten, nur sehr wenige Seitenanforderungen vorab generierten Sichten vorteilhaft.


Sie können vorabgenerieren von Sichten, die mithilfe der *EdmGen.exe* Befehlszeilentool oder mithilfe einer *Text Template Transformation Toolkit Toolkit* (T4)-Vorlage. In diesem Lernprogramm verwenden Sie eine T4-Vorlage.

In der *DAL* Ordner Hinzufügen einer Datei mit der **Textvorlage** Vorlage (diese befindet sich in der **allgemeine** Knoten in der **installierte Vorlagen** Liste), und nennen Sie sie *SchoolModel.Views.tt*. Ersetzen Sie den vorhandenen Code in der Datei durch den folgenden Code ein:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Dieser Code generiert Sichten für ein *EDMX* -Datei, die sich im gleichen Ordner wie die Vorlage befindet und hat, die den gleichen Namen wie die Vorlage. Angenommen, Ihre Vorlagendatei heißt *SchoolModel.Views.tt*, sieht sie für eine Modell-Datendatei, die mit dem Namen *SchoolModel.edmx*.

Speichern Sie die Datei, und klicken Sie dann mit der rechten Maustaste in der Datei im **Projektmappen-Explorer** , und wählen Sie **benutzerdefiniertes Tool ausführen**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio generiert eine Codedatei, die die Ansichten erstellt mit dem Namen *SchoolModel.Views.cs* basierend auf der Vorlage. (Ihnen möglicherweise aufgefallen, dass die Codedatei generiert wird, vor der Auswahl **benutzerdefiniertes Tool ausführen**, sobald Sie die Vorlagendatei speichern.)

[![Image01 abgerufen wird](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Sie können jetzt führen Sie die Anwendung und stellen Sie sicher, dass er funktioniert es hat sich nichts geändert.

Weitere Informationen zu vorab generierten Sichten finden Sie unter den folgenden Ressourcen:

- [Vorgehensweise: Vorabgenerieren von Sichten zum Verbessern der Abfrageleistung](https://msdn.microsoft.com/library/bb896240.aspx) auf der MSDN-Website. Erklärt, wie die `EdmGen.exe` Befehlszeilentool Sichten vorab generiert werden.
- [Isolieren der Leistung mit vorkompilierter/Pre-generated Ansichten in Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) im Windows Server AppFabric Customer Advisory Team-Blog.

Dies schließt die Einführung zum Verbessern der Leistung in einer ASP.NET Web-Anwendung, die das Entity Framework verwendet. Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Überlegungen zur Leistung (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) auf der MSDN-Website.
- [Leistungsbezogener Beiträge im Entity Framework-Team-Blog](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF Zusammenführungsoptionen und kompilierte Abfragen](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Blogbeitrag, die unerwartete Verhalten der kompilierten Abfragen und Merge erläutert Optionen wie z. B. `NoTracking`. Wenn Sie kompilierte Abfragen verwenden, bearbeiten oder Merge-optionseinstellungen in Ihrer Anwendung ändern möchten, lesen Sie diese zuerst.
- [Entity Framework-bezogene Beiträge in den Daten und Modellierung Customer Advisory Team-Blog](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Enthält Beiträge zu kompilierte Abfragen und den Visual Studio 2010-Profiler verwenden, um Leistungsprobleme zu ermitteln.
- [Entity Framework-Forumthread mit Ratschläge zum Verbessern der Leistung von hoch komplexer Abfragen](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [ASP.NET State Management Recommendations](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Mit dem Entity Framework und das ObjectDataSource: benutzerdefinierte Paging](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Blogbeitrag, die von der ContosoUniversity-Anwendung erstellt, die in diesen Lernprogrammen beschrieben, wie zum Implementieren von Paging in builds, die die *Departments.aspx* Seite.

Überprüft die des nächsten Lernprogramms zu den wichtigen Verbesserungen zu Entity Framework, die neu in Version 4.

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Weiter](what-s-new-in-the-entity-framework-4.md)
