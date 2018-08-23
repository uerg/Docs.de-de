---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximieren der Leistung bei Entitätsframework 4.0 in eine ASP.NET 4-Webanwendung | Microsoft-Dokumentation
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0-Tutorial-Reihe erstellt wird. ICH...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7d7c66289f09179a98e09532172477d5b06c70bd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838967"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximieren der Leistung bei Entitätsframework 4.0 in eine ASP.NET 4-Webanwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Tutorial-Reihe. Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Tutorial wurde erläutert, wie man nebenläufigkeitskonflikte behandelt. Dieses Tutorial Zeigt Optionen für die Verbesserung der Leistung einer ASP.NET-Webanwendung, die das Entity Framework verwendet. Sie erfahren, dass mehrere Methoden zum Maximieren der Leistung oder für die Diagnose von Leistungsproblemen.

Informationen in den folgenden Abschnitten wird wahrscheinlich in einer Vielzahl von Szenarien nützlich sein:

- Verwandte Daten effizient zu laden.
- Verwalten Sie Ansichtszustand.

In den folgenden Abschnitten vorgestellten Informationen möglicherweise hilfreich, wenn Sie einzelne Abfragen, vorhanden Leistungsprobleme zur Folge haben:

- Verwenden der `NoTracking` Zusammenführungsoption.
- Kompilieren Sie LINQ-Abfragen vor.
- Überprüfen Sie die Abfragebefehle, die an die Datenbank gesendet.

Im folgenden Abschnitt dargelegten Informationen ist möglicherweise nützlich für Anwendungen, die sehr große Datenmodelle haben:

- Vorgenerieren von Ansichten.

> [!NOTE]
> Leistung der Webanwendung ist von vielen Faktoren ab, einschließlich der Größe der Anforderungs-und Antwortdaten, die Geschwindigkeit von Abfragen, wie viele Anforderungen, dass der Server in die Warteschlange kann und wie schnell sie bedienen kann, und sogar die Effizienz aller betroffen Bibliotheken mit Clientskripts, die Sie verwenden können. Wenn die Leistung in Ihrer Anwendung von entscheidender Bedeutung ist, oder testen oder Erfahrung zeigt, dass die Leistung der Anwendung nicht zufriedenstellend ist, sollten Sie die normalen Protokolls zum Optimieren der Leistung befolgen. Um zu bestimmen, wo Leistungsengpässe auftreten zu messen Sie, und beheben Sie die Bereiche, die die größte Auswirkung auf die gesamtleistung der Anwendung.
> 
> Dieses Thema konzentriert sich hauptsächlich auf die Weise, in denen Sie möglicherweise die Leistung von Entity Framework in ASP.NET speziell verbessern können. Hier die Vorschläge sind nützlich, wenn Sie feststellen, dass Zugriff auf Daten der Leistungsengpässe in Ihrer Anwendung ist. Außer wie bereits erwähnt, die hier erläuterten Methoden berücksichtigt werden sollten nicht &quot;bewährte Methoden&quot; im Allgemeinen – viele davon werden nur in Ausnahmefällen oder Adresse sehr spezifische Arten der Leistungsengpässe geeignet.


Um das Lernprogramm zu starten, starten Sie Visual Studio, und öffnen Sie die Contoso University-Webanwendung, die Sie im vorherigen Tutorial verwendet wurden.

## <a name="efficiently-loading-related-data"></a>Effizientes Laden zugehöriger Daten

Es gibt mehrere Möglichkeiten, Entity Framework zugehörige Daten in die Navigationseigenschaften einer Entität laden kann:

- *Lazy Loading (verzögertes Laden)*. Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen. Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt zu mehreren Abfragen, die an die Datenbank gesendet – eine für die Entität selbst und eine jedes Mal, die Daten für die Entität beziehen abgerufen werden muss. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Eager Loading (vorzeitiges Laden)*. Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen. Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind. Sie geben eager Loading mit der `Include` -Methode, wie Sie bereits gesehen haben in diesen Tutorials.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Explizites Laden*. Dies ist ähnlich wie beim verzögerten Laden, mit dem Unterschied, dass Sie explizit im Code die verwandten Daten abgerufen werden; Dies erfolgt nicht automatisch beim Zugriff auf eine Navigationseigenschaft. Sie laden die Daten manuell anhand der `Load` -Methode der Navigationseigenschaft für Auflistungen, oder Sie verwenden die `Load` -Methode der die Verweiseigenschaft für Eigenschaften, die ein einzelnes Objekt enthalten. (Z. B., rufen Sie die `PersonReference.Load` Methode zum Laden der `Person` Navigationseigenschaft eine `Department` Entität.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Da sie sofort der Abrufen von Eigenschaftswerten nicht lazy Loading und explizites Laden werden auch beide genannt *verzögertes Laden*.

Lazy Loading ist das Standardverhalten für einen Objektkontext, der vom Designer generiert wurde. Wenn Sie öffnen die *SchoolModel.Designer.cs* Datei, die die Objektkontextklasse definiert, finden Sie drei Konstruktormethoden und jeder URI enthält die folgende Anweisung aus:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Im Allgemeinen, wenn Sie wissen, benötigen Sie verwandte Daten für jede Entität, die abgerufen werden, eager Loading die beste Leistung bietet, da eine einzelne Abfrage, die an die Datenbank gesendet, in der Regel effizienter als separate Abfragen für jede abgerufene Entität ist. Andererseits, wenn Sie auf Navigationseigenschaften einer Entität nur selten zugreifen müssen oder nur für eine kleine Gruppe von Entitäten, lazy Loading oder expliziten Laden effizienter, möglicherweise Eager Load mehr Daten als benötigt abrufen würde.

In einer Webanwendung Lazy Load relativ geringem Wert dennoch möglicherweise Benutzeraktionen, die die Notwendigkeit von verknüpften Daten betreffen im Browser stattfinden, die keine Verbindung mit dem Objektkontext verfügt, die die Seite gerendert. Andererseits, wenn Sie Databind ein Steuerelement Sie i. d. r., welche Daten wissen Sie benötigen, und daher im Allgemeinen es ist am besten unverzüglichem Laden oder verzögertes Laden basierend auf wird in jedem Szenario geeignet.

Darüber hinaus können einem datengebundenen Steuerelement ein Entitätsobjekt, nach der Objektkontext gelöscht wird. In diesem Fall würde der Versuch, eine Navigationseigenschaft-lazy Load fehlschlagen. Die Fehlermeldung, die Sie erhalten, ist klar: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Die `EntityDataSource` Steuerelement deaktiviert standardmäßig die Lazy Load. Für die `ObjectDataSource` Steuerelement, dass Sie für das aktuelle Tutorial nutzen (oder wenn Sie den Objektkontext Seitencode zugreifen), es gibt mehrere Möglichkeiten, Sie können lazy machen, laden, die standardmäßig deaktiviert. Sie können es deaktivieren, wenn Sie einen Objektkontext instanziieren. Sie können z. B. die folgende Zeile hinzufügen, an die Konstruktormethode von der `SchoolRepository` Klasse:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Für die Contoso University-Anwendung stellen Sie den Objektkontext automatisch deaktivieren Sie lazy loading, sodass diese Eigenschaft nicht festgelegt werden, wenn ein Kontext instanziiert wird.

Öffnen der *SchoolModel.edmx* Daten modellieren, klicken Sie auf die Entwurfsoberfläche, und legen Sie dann im Eigenschaftenbereich den **verzögerte Laden aktiviert** Eigenschaft `False`. Speichern Sie und schließen Sie das Datenmodell.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Verwalten des Ansichtszustands

Um Update-Funktionalität zu gewährleisten, müssen einer ASP.NET-Webseite die ursprünglichen Eigenschaftswerte einer Entität speichern, wenn eine Seite gerendert wird. Während des Postbacks verarbeiten das Steuerelement den ursprünglichen Zustand der Entität neu zu erstellen und aufrufen kann der Entität `Attach` Methode vor dem Übernehmen von Änderungen und Aufrufen der `SaveChanges` Methode. Standardmäßig können ASP.NET Web Forms-Datensteuerelemente Ansichtszustand um die ursprünglichen Werte zu speichern. Ansichtszustand kann jedoch die Leistung beeinträchtigen, da es in ausgeblendeten Feldern gespeichert wird, die im Wesentlichen die Seitengröße erhöhen kann, die in und aus den Browser gesendet wird.

Verfahren für die Verwaltung der Ansichtszustand oder alternativen wie der Sitzungsstatus, nicht nur für das Entity Framework, also in diesem Tutorial in diesem Thema im Detail nicht. Weitere Informationen finden Sie unter den Links am Ende des Tutorials.

Version 4 von ASP.NET bietet jedoch eine neue Art der Arbeit mit Ansichtszustand, die jeder Entwickler ASP.NET Web Forms-Anwendungen bewusst sein sollten: die `ViewStateMode` Eigenschaft. Diese neue Eigenschaft kann auf der Seite oder ein Steuerelement-Ebene festgelegt werden, und dadurch, dass Sie deaktiviert den Ansichtszustand in der Standardeinstellung für eine Seite, und aktivieren Sie sie nur für Steuerelemente, die ihn benötigen.

Für Anwendungen, in denen die Leistung kritisch ist, empfiehlt sich immer deaktiviert den Ansichtszustand auf Seitenebene, und aktivieren es nur für Steuerelemente, die dies erfordern. Die Größe des Ansichtstatus auf den Seiten für die Contoso University wäre nicht von dieser Methode erheblich verringert werden, aber um anzuzeigen, wie es funktioniert, machen Sie es für die *Instructors.aspx* Seite. Diese Seite enthält viele Steuerelemente, einschließlich einer `Label` -Steuerelement, das der Ansichtszustand deaktiviert hat. Keines der Steuerelemente auf dieser Seite müssen tatsächlich anzeigen, die Status "aktiviert". (Die `DataKeyNames` Eigenschaft der `GridView` Steuerelement gibt an, Zustand, der zwischen Postbacks beibehalten werden muss, aber diese Werte im Steuerelementzustand, die von betroffen wird nicht beibehalten werden die `ViewStateMode` Eigenschaft.)

Die `Page` Richtlinie und `Label` Markup des Steuerelements wird derzeit im folgende Beispiel ähnelt:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Stellen Sie die folgenden Änderungen:

- Hinzufügen `ViewStateMode="Disabled"` auf die `Page` Richtlinie.
- Entfernen Sie `ViewStateMode="Disabled"` aus der `Label` Steuerelement.

Das Markup sieht nun wie im folgende Beispiel:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Der Ansichtszustand ist jetzt für alle Steuerelemente deaktiviert. Wenn Sie später ein Steuerelement hinzufügen, der Ansichtszustand verwenden muss, müssen Sie, lediglich enthalten die `ViewStateMode="Enabled"` Attribut für dieses Steuerelement.

## <a name="using-the-notracking-merge-option"></a>Mithilfe der NoTracking-MergeOption

Bei ein Objektkontext Datenbankzeilen abruft und Entitätsobjekte, die diese darstellen erstellt, überwacht standardmäßig es außerdem die Entitätsobjekte, die über einen Objekt-Zustands-Manager. Diese Überwachungsdaten fungieren als Cache und werden verwendet, wenn Sie eine Entität aktualisieren. Da eine Webanwendung in der Regel kontextinstanzen kurzlebiges Objekt verfügt, wird in Abfragen häufig Daten zurückgeben, die nicht nachverfolgt werden müssen, da der Objektkontext, der diesen liest verworfen wird, bevor alle gelesenen Elemente erneut verwendet werden oder aktualisiert.

In Entity Framework können Sie angeben, ob der Objektkontext Entitätsobjekte durch Festlegen von verfolgt einen *MergeOption*. Sie können die MergeOption für einzelne Abfragen oder für Entitätenmengen festlegen. Wenn Sie es für eine Entitätssammlung festlegen, bedeutet, dass an, dass Sie die Standardzusammenführungsoption für alle Abfragen festgelegt sind, die für die Entitätenmenge erstellt werden.

Überwachung nicht erforderlich für die Contoso University-Anwendung, für alle Entitätenmengen, die Sie aus dem Repository zugreifen, daher Sie die Merge-Option, um festlegen können `NoTracking` für diese Entitätenmengen, wenn Sie den Objektkontext in die "Repository"-Klasse instanziieren. (Beachten Sie, dass in diesem Tutorial Festlegen der Zusammenführungsoption eine spürbare Auswirkung auf die Leistung der Anwendung nicht. Die `NoTracking` Option ist wahrscheinlich nur in bestimmten Szenarien umfangreiche Daten – ein Observable Leistungssteigerung zu erzielen.)

Öffnen Sie im Ordner "DAL", die *SchoolRepository.cs* Datei, und fügen Sie eine Konstruktormethode, die die Merge-Option legt fest, für die Entität wird festgelegt, dass das Repository zugreift:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Vorabkompilierung von LINQ-Abfragen

Beim ersten, die das Entity Framework eine Entity SQL-Abfrage innerhalb der Lebensdauer ausführt einer bestimmten `ObjectContext` Instanz benötigt etwas Zeit, um die Abfrage zu kompilieren. Das Ergebnis der Kompilierung wird zwischengespeichert, was bedeutet, dass alle folgenden Ausführungen der Abfrage viel schneller werden. LINQ-Abfragen folgen ein ähnlichen Muster, mit dem Unterschied, dass einige der Aufgaben erforderlich, um die Abfrage kompiliert erfolgt jedes Mal, wenn die Abfrage ausgeführt wird. Das heißt, sind für LINQ-Abfragen, standardmäßig nicht alle Ergebnisse der Kompilierung zwischengespeichert.

Wenn Sie eine LINQ-Abfrage, die wiederholt im Leben eines Objektkontexts ausgeführt werden sollen verfügen, können Sie Code schreiben, die bewirkt, dass alle für die Ergebnisse der Kompilierung zum ersten Mal zwischengespeichert werden, die die LINQ-Abfrage ausgeführt wird.

Veranschaulichung: Sie erreichen dies für zwei `Get` Methoden in der `SchoolRepository` -Klasse, von denen braucht keine Parameter (die `GetInstructorNames` Methode), und eine, die einen Parameter erforderlich ist (die `GetDepartmentsByAdministrator` Methode). Diese Methoden müssen wie diese mit jetzt man nicht kompiliert werden, da sie nicht, dass LINQ-Abfragen sind:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Allerdings so, dass Sie die kompilierte Abfragen ausprobieren können, müssen Sie fortgesetzt, als ob diese als die folgenden LINQ-Abfragen geschrieben worden:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Sie können den Code in diesen Methoden ändern, was oben hat, und führen Sie die Anwendung aus, um sicherzustellen, dass es, bevor Sie fortfahren funktioniert. Aber die folgenden Anweisungen gehen direkt vorkompilierte Versionen davon erstellen.

Erstellen Sie eine Klassendatei in der *DAL* Ordner, nennen Sie sie *SchoolEntities.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Dieser Code erstellt eine partielle Klasse, die die automatisch generierte Objektkontextklasse erweitert. Die partielle Klasse enthält zwei kompilierte LINQ-Abfragen, die mit der `Compile` Methode der `CompiledQuery` Klasse. Es erstellt auch Methoden, die Sie aufrufen, die Abfragen verwenden können. Speichern Sie und schließen Sie diese Datei.

Als Nächstes wird im *SchoolRepository.cs*, ändern Sie die vorhandene `GetInstructorNames` und `GetDepartmentsByAdministrator` Methoden im Repository Klasse, sodass sie die kompilierten Abfragen aufrufen:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Führen Sie die *Departments.aspx* Seite, um sicherzustellen, dass sie funktioniert wie vorher. Die `GetInstructorNames` Methode wird aufgerufen, um das Auffüllen der Administrator Dropdown-Liste, und die `GetDepartmentsByAdministrator` Methode wird aufgerufen, wenn Sie auf **Update** um sicherzustellen, dass keine "Instructor" Administrator mehrere ist Abteilung.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Sie haben die vorkompilierte Abfrage in der Contoso University-Anwendung nur, wie es geht nicht verwendet werden, weil sie die Leistung merklich verbessert würde. LINQ-Abfragen wird vorkompiliert ein Maß an Komplexität an Ihrem Code hinzufügen, also stellen Sie sicher, dass Sie es nur für Abfragen ausführen, die tatsächlich Leistungsengpässe in Ihrer Anwendung darstellen.

## <a name="examining-queries-sent-to-the-database"></a>Untersuchung von Abfragen, die an die Datenbank gesendet

Wenn Sie Leistungsprobleme untersuchen, ist es manchmal hilfreich zu wissen, die genaue SQL-Befehle, die das Entity Framework in die Datenbank gesendet wird. Wenn Sie mit der Sie arbeiten ein `IQueryable` Objekt, eine Möglichkeit hierzu ist die Verwendung der `ToTraceString` Methode.

In *SchoolRepository.cs*, ändern Sie den Code in die `GetDepartmentsByName` Methode, um die im folgende Beispiel entsprechen:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Die `departments` Variablen umgewandelt werden muss ein `ObjectQuery` geben, da die `Where` Methode am Ende der vorherigen Zeile erstellt eine `IQueryable` -Objekt ohne die `Where` -Methode, die Umwandlung nicht erforderlich.

Legen Sie einen Haltepunkt auf der `return` Zeile, und führen Sie die *Departments.aspx* Seite im Debugger. Wenn der Haltepunkt erreicht wird, untersuchen die `commandText` -Variable in der **"lokal"** und anschließend mit dem Text-Schnellansicht (das Lupensymbol in der **Wert** Spalte), seinen Wert in der anzeigen**Text-Schnellansicht** Fenster. Sie können die gesamten SQL-Befehl anzeigen, der durch diesen Code entsteht:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Als Alternative bietet das IntelliTrace-Feature in Visual Studio Ultimate eine Möglichkeit zum Anzeigen von SQL-Befehle, die von Entity Framework, das müssen Sie Ihren Code ändern oder sogar legen Sie einen Haltepunkt nicht generiert.

> [!NOTE]
> Sie können die folgenden Verfahren ausführen, nur, wenn Sie Visual Studio Ultimate verfügen.


Wiederherstellen des ursprünglichen Codes aus dem `GetDepartmentsByName` -Methode, und führen Sie die *Departments.aspx* Seite im Debugger.

Wählen Sie in Visual Studio die **Debuggen** Menü **IntelliTrace**, und klicken Sie dann **IntelliTrace-Ereignisse**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

In der **IntelliTrace** Fenster, klicken Sie auf **alle unterbrechen**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Die **IntelliTrace** Fenster zeigt eine Liste der zuletzt aufgetretene Ereignisse:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klicken Sie auf die **ADO.NET** Zeile. Wird erweitert, um Sie den Befehlstext anzeigen:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Sie können die Textzeichenfolge für den gesamten Befehl Kopieren, in die Zwischenablage aus der **"lokal"** Fenster.

Angenommen, Sie mit einer Datenbank mit Weitere Tabellen, Beziehungen und Spalten als die einfache arbeiteten `School` Datenbank. Unter Umständen eine Abfrage, die alle Informationen erfasst müssen Sie in einem einzelnen `Select` -Anweisung mit mehreren `Join` Klauseln ist zu komplex, um effizient arbeiten. In diesem Fall können Sie von mittels Eager Load laden zum expliziten Laden zur Vereinfachung der Abfrage wechseln.

Versuchen Sie es z. B. mit dem Ändern des Codes in der `GetDepartmentsByName` -Methode in der *SchoolRepository.cs*. Aktuell, Methode Sie eine Objektabfrage haben, hat `Include` Methoden für die `Person` und `Courses` Navigationseigenschaften. Ersetzen Sie die `return` -Anweisung mit Code, der explizites Laden ausführt, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Führen Sie die *Departments.aspx* Seite im Debugger aus, und überprüfen Sie die **IntelliTrace** Fenster erneut als bisher. Dort war nur eine einzelne Abfrage vor, sehen Sie jetzt eine lange Abfolge von ihnen an.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klicken Sie auf der ersten **ADO.NET** Zeile angezeigt, was für die komplexe Abfrage Sie passiert ist weiter oben angezeigt.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Die Abfrage von Abteilungen geworden ist eine einfache `Select` Abfragen ohne `Join` -Klausel, sondern folgt separate Abfragen, die verwandte Kurse und ein Administrator abrufen, einen Satz von zwei Abfragen für jede Abteilung zurückgegebenen von der ursprünglichen Abfrage.

> [!NOTE]
> Wenn Sie verzögerte lassen möglicherweise Lazy Load Laden aktiviert ist, die Muster, das Sie hier mit der gleichen Abfrage wiederholt in vielen Fällen sehen, sein. Ein Muster, das Sie in der Regel vermeiden möchten, ist lazy Loading von verknüpften Daten für jede Zeile der primären Tabelle. Es sei denn, Sie überprüft haben, dass es sich bei eine einzelnen joinabfrage effizient sein zu komplex ist, würden Sie in der Regel in solchen Fällen verbessern, indem Sie die Änderung der primären Abfrage um eager Loading verwenden können.


## <a name="pre-generating-views"></a>Vorab Generieren von Sichten

Wenn ein `ObjectContext` Objekt zuerst in eine neue Anwendungsdomäne erstellt wird, generiert Entity Framework einen Satz von Klassen, die zum Zugriff auf die Datenbank verwendet. Diese Klassen heißen *Ansichten*, und wenn Sie ein Modell sehr großen Datenmengen haben, Generierung dieser Ansichten verzögern der Website die Antwort auf die erste Anforderung für eine Seite nach der Initialisierung einer neuen Anwendungsdomäne. Sie können diese Verzögerung der ersten Anforderung reduzieren, indem Sie die Ansichten erstellen, zur Kompilierzeit statt zur Laufzeit.

> [!NOTE]
> Wenn Ihre Anwendung verfügt nicht über einen extrem großen Datenmodells, oder wenn es sich bei einem großen Datenmodell verfügt über ein, aber Sie keine Bedenken bezüglich eines Leistungsproblems, das nur die erste Seitenanforderung betroffen sind, nachdem IIS wiederverwendet wird, können Sie diesen Abschnitt überspringen. Anzeigen, die Erstellung nicht ausgeführt werden, wenn Sie instanziieren ein `ObjectContext` Objekt, da die Ansichten in der Anwendungsdomäne zwischengespeichert werden. Aus diesem Grund, es sei denn, Sie häufig Ihre Anwendung in IIS wiederverwendet werden, nur sehr wenige Seitenanforderungen aus vorab generierten Sichten profitieren.


Können Sie mithilfe von Ansichten vorab generieren die *EdmGen.exe* Befehlszeilentool oder mithilfe einer *Text Template Transformation Toolkit* (T4)-Vorlage. In diesem Tutorial verwenden Sie eine T4-Vorlage.

In der *DAL* Ordner hinzufügen, eine Datei mit der **Textvorlage** Vorlage (es befindet sich unter der **Allgemein** Knoten in der **installierte Vorlagen** Liste), und nennen Sie sie *SchoolModel.Views.tt*. Ersetzen Sie den vorhandenen Code in der Datei mit dem folgenden Code ein:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Dieser Code generiert Sichten für ein *EDMX* Datei, befindet sich im gleichen Ordner wie die Vorlage, die den gleichen Namen hat, wie die Datei der Vorlage. Angenommen, dem Namen der Vorlagendatei *SchoolModel.Views.tt*, sieht sie für eine Modell-Datendatei, die mit dem Namen *SchoolModel.edmx*.

Speichern Sie die Datei, und klicken Sie dann mit der rechten Maustaste in der Datei im **Projektmappen-Explorer** , und wählen Sie **benutzerdefiniertes Tool ausführen**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio generiert eine Codedatei, die die Sicht erstellt mit dem Namen *SchoolModel.Views.cs* basierend auf der Vorlage. (Sie haben vielleicht bemerkt, dass die Codedatei generiert wird, noch bevor Sie auswählen, **benutzerdefiniertes Tool ausführen**, sobald Sie die Vorlagendatei speichern.)

[![Image01 abgerufen wird](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Sie können jetzt die Anwendung auszuführen und stellen Sie sicher, dass sie funktioniert wie vorher.

Weitere Informationen zu vorab generierten Sichten finden Sie unter den folgenden Ressourcen:

- [Gewusst wie: vorgenerieren von Ansichten zum Verbessern der Abfrageleistung](https://msdn.microsoft.com/library/bb896240.aspx) auf der MSDN-Website. Erläutert, wie die `EdmGen.exe` Befehlszeilentool, um Sichten vorzugenerieren.
- [Isolieren der Leistung mit vorkompilierten/vorgenerierten Sichten im Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) im Windows Server AppFabric Customer Advisory Team-Blog.

Dies schließt die Einführung zum Verbessern der Leistung einer ASP.NET-Webanwendung, die das Entity Framework verwendet. Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Überlegungen zur Leistung (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) auf der MSDN-Website.
- [Leistung von Beiträgen im Entity Framework-Team-Blog](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF Zusammenführungsoptionen und kompilierte Abfragen](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Blog-Beitrag, die unerwartete Verhalten von kompilierte Abfragen und Zusammenführen wird erläutert, wie z. B. Optionen `NoTracking`. Wenn Sie kompilierte Abfragen verwenden oder Merge-optionseinstellungen in Ihrer Anwendung ändern möchten, lesen Sie diese zuerst.
- [Entity Framework-bezogenem veröffentlicht, in der Daten und Modellieren von Customer Advisory Team-Blog](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Enthält Beiträge zu kompilierte Abfragen und verwenden die Visual Studio 2010-Profiler, um Leistungsprobleme zu ermitteln.
- [Entity Framework-Forum-Thread mit Ratschlägen zur Verbesserung der Leistung von sehr komplexen Abfragen](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [ASP.NET State Management Recommendations](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Mithilfe von Entitätsframework und dem ObjectDataSource-Steuerelement: benutzerdefiniertes Paging](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Blog-Beitrag, der für die Anwendung ContosoUniversity in diesen Tutorials erstellt haben, implementieren Sie Paging in erläutert erstellt die *Departments.aspx* Seite.

Im nächste Tutorial werden einige der wichtigen Verbesserungen von Entity Framework, die in Version 4 neu sind.

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Weiter](what-s-new-in-the-entity-framework-4.md)
