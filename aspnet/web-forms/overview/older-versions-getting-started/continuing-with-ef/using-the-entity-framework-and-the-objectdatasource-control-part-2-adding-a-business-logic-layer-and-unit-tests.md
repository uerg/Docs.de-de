---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: "Verwenden das Entity Framework 4.0 und das ObjectDataSource-Steuerelement, Teil 2: hinzufügen, eine Geschäftslogikschicht und Komponententests | Microsoft Docs"
author: tdykstra
description: Diese Reihe von Lernprogrammen baut auf der Contoso-University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0 Tutorial Reihe erstellt wird. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: df37acd8901b457f7887afe767d42d53e45e4815
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Verwenden das Entity Framework 4.0 und das ObjectDataSource-Steuerelement, Teil 2: hinzufügen, eine Geschäftslogikschicht und Komponententests
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

> Diese Reihe von Lernprogrammen in der Contoso-University Webanwendung durch die erstellte builds der [erste Schritte mit dem Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Reihe von Lernprogrammen. Wenn Sie die frühere Lernprogramme nicht abgeschlossen wurde, als Ausgangspunkt für dieses Lernprogramm können Sie [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die durch das vollständige Lernprogramm Reihe erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie stellen Sie diese auf die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Lernprogramm erstellt Sie eine n-Tier-Webanwendung mithilfe von Entity Framework und die `ObjectDataSource` Steuerelement. In diesem Lernprogramm wird gezeigt, wie Geschäftslogik hinzufügen, während der Geschäftslogik Ebene (BLL) und die Datenzugriffsebene (DAL) separate gewahrt bleibt, und es wird gezeigt, wie die BLL automatisierte Komponententests zu erstellen.

In diesem Lernprogramm führen Sie die folgenden Aufgaben:

- Erstellen Sie eine Repository-Schnittstelle, die die Datenzugriffs-Methoden deklariert werden, die Sie benötigen.
- Implementieren Sie die Repository-Schnittstelle in der Repository-Klasse.
- Erstellen Sie eine Geschäftslogik-Klasse, die die Repository-Klasse, um die Datenzugriffs-Funktionen aufruft.
- Verbinden der `ObjectDataSource` Steuerelement auf die Geschäftslogik Klasse anstelle von der Repository-Klasse.
- Erstellen Sie ein Komponententestprojekt und eine Repository-Klasse, die Auflistungen im Arbeitsspeicher für seinen Datenspeicher verwendet.
- Erstellen Sie einen Komponententest für die Geschäftslogik, die auf die Geschäftslogik Klasse hinzufügen, dann führen Sie den Test und Fehler angezeigt werden sollen.
- Implementieren Sie die Geschäftslogik in der Geschäftslogik Klasse, und führen Sie die Einheit zu testen und finden Sie unter erfolgreich erneut aus.

Vorerst arbeiten Sie mit der *Departments.aspx* und *DepartmentsAdd.aspx* Seiten, die Sie im vorherigen Lernprogramm erstellt haben.

## <a name="creating-a-repository-interface"></a>Erstellen einer Repository-Schnittstelle

Sie beginnen, indem Sie zum Erstellen der Repository-Schnittstelle.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

In der *DAL* Ordner eine neue Klassendatei erstellen, nennen Sie sie *ISchoolRepository.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Die Schnittstelle definiert eine Methode für jede die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Methoden, die Sie in der Repository-Klasse erstellt haben.

In der `SchoolRepository` -Klasse im *SchoolRepository.cs*, um anzugeben, dass diese Klasse implementiert die `ISchoolRepository` Schnittstelle:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Erstellen einer Geschäftslogik Klasse

Als Nächstes erstellen Sie die Geschäftslogik Klasse. Damit Sie von Geschäftslogik hinzufügen können, die von ausgeführt wird hierfür der `ObjectDataSource` zu steuern, obwohl Sie nicht, die noch ist. Jetzt führt die neue Geschäftslogik Klasse nur die gleichen CRUD-Vorgänge aus, denen das Repository ist.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Erstellen Sie einen neuen Ordner, und nennen Sie sie *BLL*. (In einer realen Anwendung würde die Geschäftslogik Ebene in der Regel als eine Klassenbibliothek implementiert werden – ein separates Projekt – jedoch zur Vereinfachung dieses Lernprogramms BLL Klassen werden in einem Projektordner beibehalten.)

In der *BLL* Ordner eine neue Klassendatei erstellen, nennen Sie sie *SchoolBL.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Dieser Code erstellt die gleichen CRUD-Methoden, die Sie gesehen haben, weiter oben in der Repository-Klasse, anstatt direkt auf die Entity Framework-Methoden, er ruft jedoch die Repository-Klasse, Methoden.

Die Class-Variable, die einen Verweis auf die Repository-Klasse enthält als einen Schnittstellentyp definiert ist, und der Code, der die Repository-Klasse instanziiert ist in zwei Konstruktoren enthalten. Wird von der parameterlose Konstruktor verwendet den `ObjectDataSource` Steuerelement. Er erstellt eine Instanz der `SchoolRepository` -Klasse, die Sie zuvor erstellt haben. Die anderen Konstruktor können den Code, der instanziiert die Geschäftslogik Klasse, ein Objekt zu übergeben, der Repository-Schnittstelle implementiert.

Die CRUD-Methoden, die die Repository-Klasse und die beiden Konstruktoren aufrufen können sie die Geschäftslogik Klasse mit den Back-End-Datenspeicher verwendet werden. Die Geschäftslogik Klasse muss nicht beachten wie die Klasse, die ihn aufruft die Daten weiterhin besteht. (Dies wird häufig aufgerufen *Persistenz Unkenntnis*.) Dies erleichtert die Komponententests bereit, auf, weil Sie die Geschäftslogik Klasse eine Repository-Implementierung herstellen können, die etwas als einfache verwendet als in-Memory- `List` Sammlungen, um Daten zu speichern.

> [!NOTE]
> Technisch gesehen sind die Entitätsobjekten noch nicht Dauerhaftigkeit ignorierende, da von Klassen instanziiert werden, die von der Entity Framework erben `EntityObject` Klasse. Für vollständige Dauerhaftigkeit Unkenntnis, können Sie *plain old CLR-Objekte*, oder *POCOs*, anstelle von Objekten, die von erben die `EntityObject` Klasse. Mithilfe von POCOs ist nicht Gegenstand dieses Lernprogramm. Weitere Informationen finden Sie unter [Prüfbarkeit und Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) auf der MSDN-Website.)


Nachdem Sie eine Verbindung herstellen können die `ObjectDataSource` von Steuerelementen an die Geschäftslogik-Klasse statt im Repository, und stellen Sie sicher, dass alles funktioniert, es hat sich nichts geändert.

In *Departments.aspx* und *DepartmentsAdd.aspx*, ändern Sie jedes Auftreten der `TypeName="ContosoUniversity.DAL.SchoolRepository"` zu `TypeName="ContosoUniversity.BLL.SchoolBL`". (Es gibt vier Instanzen in allen.)

Führen Sie die *Departments.aspx* und *DepartmentsAdd.aspx* Seiten, um sicherzustellen, dass sie weiterhin funktionieren, wie vor.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Erstellen eines Komponententestprojekts und der Repository-Implementierung

Fügen Sie ein neues Projekt mit der Lösung die **Testprojekt** Vorlage, und nennen Sie sie `ContosoUniversity.Tests`.

Fügen Sie im Testprojekt einen Verweis auf `System.Data.Entity` und fügen Sie einen Projektverweis auf die `ContosoUniversity` Projekt.

Sie können jetzt die Repository-Klasse erstellen, die Sie mit Komponententests verwenden möchten. Der Datenspeicher für dieses Repository werden innerhalb der Klasse.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Im Testprojekt eine neue Klassendatei erstellen, nennen Sie sie *MockSchoolRepository.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Diese Klasse Repository hat dieselben CRUD-Methoden wie derjenige, der das Entity Framework greift direkt auf, aber sie funktionieren mit `List` Auflistungen im Arbeitsspeicher anstelle von mit einer Datenbank. Dies erleichtert es für eine Testklasse einrichten und Überprüfen von Komponententests für die Geschäftslogik Klasse.

## <a name="creating-unit-tests"></a>Erstellen von Komponententests

Die **testen** -Projektvorlage ein Stub-Komponententestklasse für Sie erstellt und die nächste Aufgabe besteht darin ändern diese Klasse, indem Sie Komponententestmethoden hinzugefügt, für die Geschäftslogik, die die Geschäftslogik Klasse hinzugefügt werden sollen.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Klicken Sie Contoso-Universität jeder einzelne Kursleiter kann nur der Administrator eine einzelne Abteilung und Geschäftslogik zum Erzwingen dieser Regel hinzufügen werden müssen. Starten Sie durch Hinzufügen von Tests und Ausführen der Tests aus, um diese Fehler anzuzeigen. Sie klicken Sie dann fügen Sie den Code und erneuten Ausführen der Tests ein erwarten sie erfolgreich ab.

Öffnen der *"UnitTest1.cs"* und fügen `using` -Anweisungen für die Geschäftsschichten Logik und Daten zugreifen, die Sie im Projekt ContosoUniversity erstellt:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Ersetzen Sie die `TestMethod1` Methode mit den folgenden Methoden:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Die `CreateSchoolBL` Methode erstellt eine Instanz der Repository-Klasse, die Sie erstellt haben, für das Projekt Komponententest übergibt sie dann an eine neue Instanz der Geschäftslogik Klasse. Die Methode verwendet dann die Geschäftslogik Klasse zum Einfügen von drei Abteilungen, die Sie verwenden können, in den Testmethoden.

Die Testmethoden überprüfen, ob die Geschäftslogik Klasse eine Ausnahme auslöst, wenn jemand versucht, eine neue Abteilung mit dem gleichen Administrator wie einer vorhandenen Abteilung einzufügen, oder wenn jemand versucht, eine Abteilung Administrator durch Festlegen auf die ID einer Person aktualisieren Wer ist bereits der Administrator einer anderen Abteilung.

Exception-Klasse noch nicht noch erstellt werden, damit dieser Code nicht kompiliert wird. Zum Kompilieren abrufen Maustaste `DuplicateAdministratorException` , und wählen Sie **generieren**, und klicken Sie dann **Klasse**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Dadurch wird eine Klasse erstellt, in das Testprojekt, das Sie löschen können, nachdem Sie in das Hauptprojekt Exception-Klasse erstellt haben. und die Geschäftslogik implementiert.

Führen Sie das Testprojekt. Wie erwartet, schlägt die Tests fehl.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Hinzufügen von Geschäftslogik eine Pass-Test ausführen

Als Nächstes implementieren Sie die Geschäftslogik, die macht es unmöglich, als Administrator einer Abteilung jemand festgelegt, der bereits einer anderen Abteilung ist. Sie löst eine Ausnahme von der Geschäftslogik Ebene, und klicken Sie dann in der Darstellungsschicht abfangen, wenn ein Benutzer eine Abteilung bearbeitet und klickt auf **Update** nach der Auswahl eine Person, die bereits ein Administrator ist. (Lehrkräfte konnte auch entfernt werden, aus der Dropdown-Liste, die bereits Administratoren sind, bevor Sie auf die Seite gerendert, die Zwecke des vorliegenden ist jedoch mit der Geschäftslogik Ebene arbeiten.)

Starten Sie durch das Erstellen von Exception-Klasse, die ausgelöst wird, wenn ein Benutzer versucht, einem Kursleiter mehr als eine Abteilung Administratorberechtigungen. In das Hauptprojekt, erstellen Sie eine neue Klassendatei in der *BLL* Ordner, nennen Sie sie *DuplicateAdministratorException.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Löschen Sie jetzt die temporäre *DuplicateAdministratorException.cs* -Datei, die Sie im Testprojekt zuvor erstellt haben damit kompiliert werden.

Öffnen Sie in das Hauptprojekt der *SchoolBL.cs* Datei, und fügen Sie die folgende Methode, die die Validierungslogik enthält. (Der Code bezieht sich auf eine Methode, die Sie später erstellen.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Sie müssen diese Methode aufrufen, wenn Sie einfügen oder Aktualisieren von `Department` Entitäten, um zu überprüfen, ob eine andere Abteilung bereits die gleichen Administrator hat.

Der Code Ruft eine Methode zum Durchsuchen der Datenbank für eine `Department` Entität mit dem gleichen `Administrator` Eigenschaftswert als Entität eingefügt oder aktualisiert wird. Wenn eine gefunden wird, löst der Code eine Ausnahme aus. Keine Überprüfung ist erforderlich, wenn die Entität eingefügt oder aktualisiert wird keine `Administrator` Wert und keine Ausnahme wird ausgelöst, wenn die Methode aufgerufen wird, während eines Updates und die `Department` gefundene Übereinstimmungen Entität die `Department` Entität aktualisiert wird.

Rufen Sie die neue Methode aus der `Insert` und `Update` Methoden:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

In *ISchoolRepository.cs*, fügen Sie die folgende Deklaration für die neue Datenzugriffs-Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

In *SchoolRepository.cs*, fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

In *SchoolRepository.cs*, fügen Sie die folgenden neuen Datenzugriffs-Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Dieser Code ruft `Department` Entitäten, die einen angegebenen Administrator haben. Nur eine Abteilung sollten (sofern vorhanden) wurde gefunden. Da jedoch keine Einschränkung in die Datenbank integriert ist, ist der Rückgabetyp eine Auflistung für den Fall, dass mehrere Abteilungen gefunden werden.

Standardmäßig Wenn für der Objektkontext Entitäten aus der Datenbank abgerufen werden der nachverfolgt es diese in seiner Objektstatus-Manager. Die `MergeOption.NoTracking` Parameter gibt an, dass diese Überwachung für diese Abfrage nicht ausgeführt werden wird. Dies ist erforderlich, da die Abfrage die genaue Entität zurückgeben kann, die Sie aktualisieren möchten, und klicken Sie dann Sie nicht diese Entität angefügt. Angenommen, Sie bearbeiten die Abteilung der Verlauf der *Departments.aspx* Seite und der Administrator unverändert verlassen, wird diese Abfrage den Verlauf Abteilung zurückgeben. Wenn `NoTracking` ist nicht festgelegt ist, wird der Objektkontext müsste bereits Entität Department "Verlauf" in der Objektstatus-Manager. Und wenn Sie die Versionsgeschichte Abteilung Entität, die aus dem Ansichtszustand neu erstellt wird anfügen, der Objektkontext eine Ausnahme auslöst, die besagt, `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Alternativ mit der Angabe von `MergeOption.NoTracking`, können Sie einem neuen Objektkontext nur für diese Abfrage erstellen. Da die neuen Objektkontext eigene Objektstatus-Manager verfügen würde, wäre kein Konflikt beim Aufrufen der `Attach` Methode. Neuen Objektkontext würde Metadaten und datenbankverbindung mit dem ursprünglichen Objektkontext freigeben, damit Leistungseinbuße von diesem alternativen Ansatz minimiert werden würde. Die hier gezeigte Ansatz führt jedoch die `NoTracking` option müssen Sie in anderen Kontexten hilfreich. Die `NoTracking` Option erläutert wird in einem späteren Lernprogramm dieser Reihe weiterer.)

Fügen Sie im Testprojekt befindet, die neue Datenzugriffs-Methode *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Dieser Code verwendet LINQ, um die gleichen Datenauswahl auszuführen, die die `ContosoUniversity` Projekt Repository verwendet LINQ to Entities für.

Führen Sie das Testprojekt erneut aus. Dieses Mal werden die Tests bestanden.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Behandeln von Ausnahmen ObjectDataSource

In der `ContosoUniversity` Projekt, und führen Sie die *Departments.aspx* Seite, und versuchen Sie, die der Administrator für eine Abteilung an eine Person zu ändern, der bereits für eine andere Abteilung ist. (Beachten Sie, dass Sie nur Abteilungen, die Sie im Verlauf dieses Lernprogramms hinzugefügt bearbeiten können, da die Datenbank mit ungültigen Daten vorab geladene geschaltet wurde.) Sie erhalten die folgende Server-Fehlerseite:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Sie möchten schließlich nicht Benutzer sehen diese Art von Fehler (Seite), damit Sie Fehlerbehandlungscode hinzufügen müssen. Open *Departments.aspx* , und geben Sie einen Handler für das `OnUpdated` -Ereignis für die `DepartmentsObjectDataSource`. Die `ObjectDataSource` öffnungstag jetzt ähnelt dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

In *Departments.aspx.cs*, fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Fügen Sie den folgenden Ereignishandler für das `Updated` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Wenn die `ObjectDataSource` Steuerelement wird eine Ausnahme abgefangen, wenn er versucht, das Update durchzuführen, übergibt die Ausnahme im Ereignisargument (`e`), die diesem Handler. Der Code im Ereignishandler überprüft, um festzustellen, ob die Ausnahme, die doppelte Administrator-Ausnahme ist. Wenn dies der Fall, wird der Code erstellt ein Validator-Steuerelement, das für eine Fehlermeldung enthält den `ValidationSummary` -Steuerelement zum Anzeigen.

Führen Sie die Seite, und versuchen Sie damit eine Person den Administrator zwei Abteilungen wieder. Dieses Mal die `ValidationSummary` Steuerelement zeigt eine Fehlermeldung an.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Nehmen Sie ähnliche Änderungen an der *DepartmentsAdd.aspx* Seite. In *DepartmentsAdd.aspx*, geben Sie einen Handler für das `OnInserted` -Ereignis für die `DepartmentsObjectDataSource`. Das resultierende Markup wird im folgende Beispiel ähneln.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

In *DepartmentsAdd.aspx.cs*, fügen Sie dem `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Fügen Sie den folgenden Ereignishandler hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Sie können jetzt testen der *DepartmentsAdd.aspx.cs* Seite, um sicherzustellen, dass sie versuchen, eine Person mehrere Abteilung Administratorberechtigungen auch ordnungsgemäß behandelt.

Dies schließt die Einführung zum Implementieren des Repositorymusters für die Verwendung der `ObjectDataSource` Steuerelement mit dem Entity Framework. Weitere Informationen zu den Repository-Muster und die Prüfbarkeit finden Sie im MSDN-Whitepaper [Prüfbarkeit und Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

Im folgenden Lernprogramm sehen Sie, wie Sortier- und Filterfunktionen zur Anwendung hinzugefügt werden.

>[!div class="step-by-step"]
[Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[Weiter](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
