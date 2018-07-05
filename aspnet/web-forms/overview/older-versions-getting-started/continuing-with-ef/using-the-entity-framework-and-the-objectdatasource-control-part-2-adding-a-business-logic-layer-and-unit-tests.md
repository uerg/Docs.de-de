---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 2: Hinzufügen einer Geschäftslogikebene und Komponententests | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0-Tutorial-Reihe erstellt wird. ICH...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6adeb0e49fa754c42bdff6e0bb058f7b73a3f68f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832557"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 2: Hinzufügen einer Geschäftslogikebene und Komponententests
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Tutorial-Reihe. Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Tutorial erstellt Sie eine n-Tier-Webanwendung mithilfe von Entity Framework und die `ObjectDataSource` Steuerelement. In diesem Tutorial wird gezeigt, wie Geschäftslogik hinzufügen, während die Geschäftslogik-Ebene (BLL) und der Datenzugriffsebene (DAL) separaten beibehalten, und es wird gezeigt, wie die BLL automatisierte Komponententests zu erstellen.

In diesem Tutorial müssen Sie die folgenden Aufgaben ausführen:

- Erstellen Sie eine Repositoryschnittstelle, die die Datenzugriffs-Methoden deklariert, die Sie benötigen.
- Implementieren Sie die Repositoryschnittstelle, in der repositoryklasse.
- Erstellen Sie eine Geschäftslogik-Klasse, die "Repository"-Klasse zum Ausführen von Datenzugriffs-Funktionen aufruft.
- Verbinden der `ObjectDataSource` Steuerelement auf die Geschäftslogik Klasse anstelle von "Repository"-Klasse.
- Erstellen Sie ein Komponententestprojekt und einer Repository-Klasse, die Auflistungen im Arbeitsspeicher für den Datenspeicher verwendet.
- Erstellen Sie einen Komponententest für Geschäftslogik, die Sie die Geschäftslogik-Klasse, und klicken Sie dann den Test ausführen, und finden Sie unter Fehler hinzufügen möchten.
- Die Geschäftslogik in der Geschäftslogik-Klasse implementieren und führen Sie erneut die Einheit zu testen, und finden Sie unter er erfolgreich abgeschlossen.

Arbeiten Sie mit der *Departments.aspx* und *DepartmentsAdd.aspx* Seiten, die Sie im vorherigen Tutorial erstellt haben.

## <a name="creating-a-repository-interface"></a>Erstellen einer Repositoryschnittstelle

Sie erstellen Sie zunächst die Repositoryschnittstelle.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

In der *DAL* Ordner eine neue Klassendatei zu erstellen, nennen Sie es *ISchoolRepository.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Die Schnittstelle definiert eine Methode für jede der die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Methoden, die Sie in der "Repository"-Klasse erstellt haben.

In der `SchoolRepository` -Klasse im *SchoolRepository.cs*, um anzugeben, dass diese Klasse implementiert die `ISchoolRepository` Schnittstelle:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Erstellen einer Geschäftslogik-Klasse

Als Nächstes erstellen Sie die Geschäftslogik-Klasse. Sie tun dies, damit Sie die Geschäftslogik hinzufügen können, die von ausgeführt werden, die `ObjectDataSource` zu steuern, obwohl Sie nicht, die noch ist. Jetzt wird die neue Geschäftslogik-Klasse nur die gleichen CRUD-Vorgänge ausführen, die das Repository ausführt.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Erstellen Sie einen neuen Ordner, und nennen Sie sie *BLL*. (In einer realen Anwendung würde die Geschäftslogik-Ebene in der Regel als Klassenbibliothek implementiert werden, ein separates Projekt, aber in diesem Tutorial der Einfachheit halber werden BLL-Klassen in einem Projektordner gespeichert.)

In der *BLL* Ordner eine neue Klassendatei zu erstellen, nennen Sie es *SchoolBL.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Dieser Code erstellt die gleichen CRUD-Methoden, die Sie gesehen haben, weiter oben in der repositoryklasse, anstatt direkt auf die Entity Framework-Methoden, er ruft jedoch das Repository-Klasse, Methoden.

Die Class-Variable, die einen Verweis auf die "Repository"-Klasse enthält, die als ein Schnittstellentyp definiert ist, und der Code, der die "Repository"-Klasse instanziiert ist in zwei Konstruktoren enthalten. Der parameterlose Konstruktor wird von verwendet die `ObjectDataSource` Steuerelement. Es erstellt eine Instanz der `SchoolRepository` -Klasse, die Sie zuvor erstellt haben. Der andere Konstruktor kann jeden beliebigen Code, der instanziiert die Geschäftslogik-Klasse ein Objekt übergeben, die die Repositoryschnittstelle implementiert.

Die CRUD-Methoden, die "Repository"-Klasse und die beiden Konstruktoren aufrufen ermöglichen die Geschäftslogik-Klasse mit den Back-End-Datenspeicher verwendet werden können. Die Geschäftslogik-Klasse muss nicht zu beachten, wie die Klasse, die sie aufrufen, wird die Daten werden beibehalten. (Dies wird häufig als *ignorieren der Persistenz*.) Dadurch werden Komponententests, erleichtert, da Sie zu einer Repository-Implementierung die Geschäftslogik-Klasse verbinden können, die etwas ganz einfachen verwendet als in-Memory- `List` Sammlungen zum Speichern von Daten.

> [!NOTE]
> Technisch gesehen die Entitätsobjekte sind noch keine Dauerhaftigkeit, da sie eine Instanziierung von Klassen sind, die von Entity Framework erben `EntityObject` Klasse. Für vollständige ignorieren der Persistenz, können Sie *plain old CLR Objekte*, oder *POCOs*, anstelle von Objekten, die von erben die `EntityObject` Klasse. Die Verwendung von POCOs ist, würde den Rahmen dieses Tutorials. Weitere Informationen finden Sie unter [Prüfbarkeit und Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) auf der MSDN-Website.)


Nachdem Sie eine Verbindung herstellen können die `ObjectDataSource` Steuerelemente auf die Geschäftslogik-Klasse anstelle von an das Repository, und stellen Sie sicher, dass alles funktioniert wie vorher.

In *Departments.aspx* und *DepartmentsAdd.aspx*, ändern Sie jedes Vorkommen von `TypeName="ContosoUniversity.DAL.SchoolRepository"` zu `TypeName="ContosoUniversity.BLL.SchoolBL`". (Es gibt vier Instanzen in allen.)

Führen Sie die *Departments.aspx* und *DepartmentsAdd.aspx* Seiten, um sicherzustellen, dass sie weiterhin funktionieren wie bisher.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Erstellen eines Komponententestprojekts und der Repository-Implementierung

Fügen Sie ein neues Projekt der Projektmappe mithilfe der **Testprojekt** Vorlage, und nennen Sie sie `ContosoUniversity.Tests`.

Fügen Sie im Testprojekt einen Verweis auf `System.Data.Entity` und fügen Sie einen Projektverweis auf die `ContosoUniversity` Projekt.

Sie können jetzt die "Repository"-Klasse erstellen, die Sie mit Komponententests verwenden. Der Datenspeicher für dieses Repository wird in der Klasse sein.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Klicken Sie in das Testprojekt, und erstellen Sie eine neue Klassendatei, nennen Sie es *MockSchoolRepository.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Dieses Repository-Klasse verfügt über die gleichen CRUD-Methoden, die das Entity Framework direkt zugreift, aber sie funktionieren mit `List` Auflistungen im Arbeitsspeicher, nicht mit einer Datenbank. Dies erleichtert es für eine Testklasse einrichten und überprüfen Komponententests für die Geschäftslogik-Klasse.

## <a name="creating-unit-tests"></a>Erstellen von Komponententests

Die **testen** Projektvorlage eine Stub-Komponententestklasse für Sie erstellt und die nächste Aufgabe ist, ändern Sie diese Klasse von Komponententestmethoden hinzugefügt, für die Geschäftslogik, die die Geschäftslogik-Klasse hinzugefügt werden sollen.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso University jeder einzelnen "Instructor" kann nur der Administrator eine einzelne Abteilung sein, und Sie Geschäftslogik zum Erzwingen dieser Regel hinzufügen möchten. Sie werden gestartet, durch Hinzufügen von Tests und Ausführen der Tests aus, um Fehler anzuzeigen. Anschließend fügen Sie den Code und führen Sie erneut aus den Tests erfolgreich erhalten.

Öffnen der *"UnitTest1.cs"* -Datei und fügen `using` -Anweisungen für die Business-Logik und die Datenzugriffs-Ebenen, die Sie in der "ContosoUniversity"-Projekt erstellt haben:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Ersetzen Sie die `TestMethod1` Methode mit den folgenden Methoden:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Die `CreateSchoolBL` Methode erstellt eine Instanz der repositoryklasse, die Sie erstellt haben, für die Einheit Testprojekt, die sie dann auf eine neue Instanz der Geschäftslogik-Klasse übergibt. Die Methode verwendet dann die Geschäftslogik-Klasse zum Einfügen von drei Abteilungen, die Sie verwenden können, in den Testmethoden.

Die Testmethoden, stellen Sie sicher, dass die Geschäftslogik-Klasse eine Ausnahme auslöst, wenn jemand versucht, eine neue Abteilung mit dem gleichen Administrator als einer vorhandenen Abteilung einfügen, oder wenn jemand versucht, eine Abteilung Administrator zu aktualisieren, indem er auf die ID einer Person Wer ist bereits einer anderen Abteilung-Administrator.

Die Exception-Klasse noch nicht noch erstellt werden, damit dieser Code nicht kompiliert werden. Rufen Sie kompilieren Maustaste `DuplicateAdministratorException` , und wählen Sie **generieren**, und klicken Sie dann **Klasse**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Dadurch wird eine Klasse erstellt, in das Testprojekt, das Sie löschen können, nachdem Sie die Exception-Klasse im Hauptprojekt erstellt haben. und die Geschäftslogik implementiert.

Führen Sie das Testprojekt. Wie erwartet, ein Fehler auf die Tests.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Hinzufügen von Geschäftslogik, um einen Testdurchlauf zu machen.

Als Nächstes implementieren Sie die Geschäftslogik, die macht es unmöglich, Sie als Administrator einer Abteilung eine Person festgelegt, der bereits einer anderen Abteilung ist. Sie löst eine Ausnahme von der Geschäftslogik-Ebene und fange ihn dann in der Darstellungsschicht, wenn ein Benutzer eine Abteilung bearbeitet und klickt auf **Update** nach der Auswahl eine Person, die bereits ein Administrator ist. (Sie könnten auch Dozenten entfernen, aus der Dropdown-Liste, die bereits die Administratoren sind, bevor Sie auf die Seite rendern, aber hier dient dazu, mit der Geschäftslogik-Ebene zu arbeiten.)

Zunächst erstellen die Exception-Klasse, die Sie auslösen müssen, wenn ein Benutzer versucht, ein "Instructor" mehr als eine Abteilung ernennen. In dem Hauptprojekt, erstellen Sie eine neue Klassendatei in der *BLL* Ordner, nennen Sie sie *DuplicateAdministratorException.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Jetzt löschen die temporäre *DuplicateAdministratorException.cs* im Testprojekt zuvor erstellte Datei zu kompilieren können.

Öffnen Sie in dem Hauptprojekt, das *SchoolBL.cs* Datei, und fügen Sie die folgende Methode, die die Validierungslogik enthält. (Der Code bezieht sich auf eine Methode, die Sie später erstellen.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Sie müssen diese Methode aufrufen, wenn Sie einfügen oder aktualisieren `Department` Entitäten, um zu überprüfen, ob eine andere Abteilung bereits die gleiche Administrator verfügt.

Der Code Ruft eine Methode zum Suchen der Datenbank für eine `Department` Entität mit dem gleichen `Administrator` Eigenschaftswert als die Entität eingefügt oder aktualisiert wird. Wenn eine gefunden wird, löst der Code eine Ausnahme aus. Keine Überprüfung ist erforderlich, wenn die Entität eingefügt oder aktualisiert werden keine `Administrator` Wert und keine Ausnahme wird ausgelöst, wenn die Methode aufgerufen wird, während einer Aktualisierung und die `Department` Entität Übereinstimmungen gefunden der `Department` Entität aktualisiert wird.

Rufen Sie die neue Methode über die `Insert` und `Update` Methoden:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

In *ISchoolRepository.cs*, fügen Sie die folgende Deklaration für die neue Datenzugriffs-Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

In *SchoolRepository.cs*, fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

In *SchoolRepository.cs*, fügen Sie die folgende neue Datenzugriffs-Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Dieser Code ruft `Department` Entitäten, die einen angegebenen Administrator haben. Nur eine Abteilung sollten (sofern vorhanden) wurde gefunden. Aber da keine Einschränkung in der Datenbank erstellt wurde, ist der Rückgabetyp eine Auflistung für den Fall, dass mehrere Abteilungen gefunden werden.

In der Standardeinstellung, wenn der Objektkontext Entitäten aus der Datenbank abruft verfolgt des sie sie in der Objektstatus-Manager. Die `MergeOption.NoTracking` Parameter gibt an, dass diese nachverfolgung für diese Abfrage nicht ausgeführt werden wird. Dies ist erforderlich, da die Abfrage die genaue Entität zurückgeben kann, die Sie aktualisieren möchten, und klicken Sie dann Sie nicht diese Entität anfügen können. Angenommen, Sie bearbeiten, dass die Abteilung der Verlauf der *Departments.aspx* Seite und der Administrator unverändert lassen, gibt diese Abfrage die Verlaufs-Abteilung. Wenn `NoTracking` ist nicht festgelegt ist, wird der Objektkontext müsste bereits die Verlaufs-Entität "Department" in einen Objekt-Zustands-Manager. Und wenn Sie die abteilungsentität Verlauf, die aus dem Ansichtszustand neu erstellt wird anfügen, der Objektkontext eine Ausnahme auslösen würde, die besagt, `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Als Alternative zum Angeben von `MergeOption.NoTracking`, können Sie einem neuen Objektkontext nur für diese Abfrage erstellen. Da die neuen Objektkontext eine eigene Objektstatus-Manager einrichten möchten, wäre kein Konflikt beim Aufrufen der `Attach` Methode. Die neuen Objektkontext teilten Metadaten und die Datenbank mit dem ursprünglichen Objektkontext dieser Alternativen Vorgehensweise die Leistungseinbußen durch minimale wäre. Der hier gezeigte Ansatz führt jedoch die `NoTracking` option, die in anderen Kontexten nützlich finden Sie. Die `NoTracking` Option erläutert wird weiter unten in einem späteren Tutorial dieser Reihe.)

Fügen Sie in das Testprojekt, und die neue Datenzugriffs-Methode *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Dieser Code verwendet LINQ, um die Datenauswahl der gleichen durchführen, die die `ContosoUniversity` projektrepository verwendet LINQ to Entities für.

Führen Sie das Testprojekt erneut aus. Dieses Mal werden die Tests bestanden.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Behandeln von Ausnahmen von "ObjectDataSource"

In der `ContosoUniversity` Projekt, und führen Sie die *Departments.aspx* Seite, und versuchen Sie, die der Administrator für eine Abteilung an eine Person zu ändern, die bereits einer anderen Abteilung-Administrator ist. (Beachten Sie, dass Sie nur Abteilungen, die Sie im Verlauf dieses Lernprogramms hinzugefügt bearbeiten können, da die Datenbank mit ungültigen Daten vorab geladen wird.) Sie erhalten eine die folgenden Seite der Server-Fehler:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Sie möchten nicht die Benutzer sehen diese Art von Fehler (Seite), müssen Sie Fehlerbehandlungscode hinzufügen. Open *Departments.aspx* , und geben Sie einen Handler für die `OnUpdated` Ereignis die `DepartmentsObjectDataSource`. Die `ObjectDataSource` öffnungstag jetzt das folgende Beispiel ähnelt.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

In *Departments.aspx.cs*, fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Fügen Sie den folgenden Ereignishandler für die `Updated` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Wenn die `ObjectDataSource` Steuerelement wird eine Ausnahme abgefangen, wenn er versucht, das Update auszuführen, die er übergibt die Ausnahme im Ereignisargument (`e`), die diesem Handler. Der Code im Ereignishandler überprüft, um festzustellen, ob die Ausnahme, die doppelte Administrator-Ausnahme ist. Wenn es sich handelt, wird der Code erstellt ein Validator-Steuerelement, das für eine Fehlermeldung enthält die `ValidationSummary` -Steuerelement angezeigt.

Führen Sie die Seite, und versuchen Sie, eine Person wieder den Administrator zwei Abteilungen zu machen. Dieses Mal die `ValidationSummary` Steuerelement wird eine Fehlermeldung angezeigt.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Nehmen Sie ähnliche Änderungen an der *DepartmentsAdd.aspx* Seite. In *DepartmentsAdd.aspx*, geben Sie einen Handler für die `OnInserted` Ereignis die `DepartmentsObjectDataSource`. Das daraus resultierende Markup wird im folgende Beispiel ähneln.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

In *DepartmentsAdd.aspx.cs*, fügen Sie dem `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Fügen Sie den folgenden Ereignishandler hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Sie können nun testen der *DepartmentsAdd.aspx.cs* Seite, um sicherzustellen, dass sie auch richtig versucht, eine Person mehr als eine Abteilung ernennen behandelt.

Dies schließt die Einführung zum Implementieren der Repositorymuster für die Verwendung der `ObjectDataSource` Steuerelement mit dem Entity Framework. Weitere Informationen über die Repository-Muster und testbarkeit finden Sie im MSDN-Whitepaper [Prüfbarkeit und Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

Im folgenden Tutorial sehen Sie das Hinzufügen von Sortier- und Filterfunktionen zur Anwendung.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Weiter](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
