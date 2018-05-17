---
title: Testen von Controllerlogik in ASP.NET Core
author: ardalis
description: Informationen zum Testen von Controllerlogik in ASP.NET Core mit Moq und xUnit
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: 51b7a02c697807c9e3504b70f89370126ee0e781
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="test-controller-logic-in-aspnet-core"></a>Testen von Controllerlogik in ASP.NET Core

Von [Steve Smith](https://ardalis.com/)

Controller in ASP.NET MVC-Apps sollten möglichst klein sein und sich überwiegend mit Aspekten der Benutzeroberfläche befassen. Große Controller, die sich mit anderen Belangen als der Benutzeroberfläche befassen, können schwerer getestet und verwaltet werden.

[Beispiel anzeigen oder von GitHub herunterladen](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).

## <a name="testing-controllers"></a>Testen von Controllern

Controller sind ein zentraler Bestandteil jeder ASP.NET Core MVC-Anwendung. Daher sollten Sie auch darauf vertrauen können, dass sie in Ihrer App wie beabsichtigt funktionieren. Automatisierte Tests können Ihnen dieses Vertrauen vermitteln. Sie können zudem dabei helfen, Fehler aufzudecken, bevor diese die Produktion erreichen. Sie sollten auf keinen Fall unnötige Aufgaben innerhalb der Controller platzieren. Zudem sollten die Tests ausschließlich auf die Controlleraufgaben ausgerichtet sein.

Controllerlogik sollte minimal und nicht auf Geschäftslogik oder Infrastruktur ausgerichtet sein (wie z.B. Datenzugriff). Testen Sie die Controllerlogik, nicht das Framework. Testen Sie das *Verhalten* des Controllers bei gültigen oder ungültigen Eingaben. Testen Sie die Antworten des Controllers auf der Grundlage des Ergebnisses des Geschäftsvorgangs, den er ausführt.

Typische Aufgaben des Controllers:

* Überprüfen von `ModelState.IsValid`
* Zurückgeben einer Fehlerantwort, wenn `ModelState` ungültig ist
* Abrufen einer Geschäftsentität aus dem Persistenzspeicher
* Ausführen einer Aktion für die Geschäftsentität
* Speichern der Geschäftsentität im Persistenzspeicher
* Zurückgeben eines entsprechenden `IActionResult`

## <a name="unit-testing"></a>Unittest

Ein [Komponententest](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) beinhaltet das Testen einer App-Komponente isoliert von ihrer Infrastruktur und ihren Abhängigkeiten. Bei einem Komponententest der Controllerlogik werden nur die Inhalte einer einzelnen Aktion getestet, nicht das Verhalten ihrer Abhängigkeiten oder des Frameworks selbst. Konzentrieren Sie sich bei der Durchführung eines Komponententests für Ihre Controlleraktionen daher nur auf deren Verhalten. Bei einem Komponententest des Controllers werden z.B. [Filter](filters.md), [Routing](../../fundamentals/routing.md), [Modellbindung](../models/model-binding.md) o.Ä. vermieden. Durch die Fokussierung auf nur einen Aspekt sind Komponententests in der Regel einfach zu schreiben und schnell in der Ausführung. Gut geschriebene Komponententests können häufig ohne großen Mehraufwand ausgeführt werden. Komponententests erkennen jedoch keine Probleme bei der Interaktion zwischen den Komponenten. Dazu dienen [Integrationstests](xref:mvc/controllers/testing#integration-testing).

Wenn Sie benutzerdefinierte Filter, Routen usw. schreiben, sollten Sie für diese einen Komponententest durchführen. Er sollte jedoch nicht Bestandteil eines Tests für eine bestimmte Controlleraktion sein. Die Tests sollten vielmehr isoliert ausgeführt werden.

> [!TIP]
> [Erstellen und Ausführen von Komponententests mit Visual Studio](/visualstudio/test/unit-test-your-code).

Überprüfen Sie den folgenden Controller, um Komponententests zu veranschaulichen. Es wird eine Liste von Brainstormingsitzungen angezeigt. Außerdem können neue Brainstormingsitzungen mit einer POST-Anforderung erstellt werden:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Der Controller befolgt das [Prinzip der expliziten Abhängigkeiten](http://deviq.com/explicit-dependencies-principle/). Es wird Dependency Injection erwartet, wodurch dem Controller eine Instanz von `IBrainstormSessionRepository` bereitgestellt wird. Dadurch kann der Test relativ leicht mithilfe eines Pseudoobjektframeworks, wie z.B. [Moq](https://www.nuget.org/packages/Moq/), ausgeführt werden. Die `HTTP GET Index`-Methode verfügt weder über Schleifen noch über Verzweigungen und ruft nur eine Methode auf. Zum Testen der `Index`-Methode muss überprüft werden, ob ein `ViewResult` zurückgegeben wird, mit einem `ViewModel` aus der `List`-Methode des Repositorys.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

Mit der `HomeController`-`HTTP POST Index`-Methode (siehe oben) sollte Folgendes überprüft werden:

* Die Aktionsmethode gibt für `ViewResult` eine ungültige Anforderung zurück mit den entsprechenden Daten, wenn `ModelState.IsValid` `false` ist.

* Die `Add`-Methode im Repository wird aufgerufen, und ein `RedirectToActionResult` mit den korrekten Argumenten wird zurückgegeben, wenn `ModelState.IsValid` TRUE ist.

Ein ungültiger Modellstatus kann getestet werden, indem mithilfe von `AddModelError` wie im ersten Test unten gezeigt Fehler hinzugefügt werden.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Im ersten Test wird bestätigt, dass bei ungültigem `ModelState` das gleiche `ViewResult` zurückgegeben wird wie für eine `GET`-Anforderung. Beachten Sie, dass der Test für ein ungültiges Modell gar nicht erfolgreich durchgeführt werden soll. Da die Modellbindung nicht ausgeführt wird, wäre dies ohnehin nicht möglich (obwohl bei einem [Integrationstest](xref:mvc/controllers/testing#integration-testing) eine Trainingsmodellbindung verwendet wird). In diesem Fall wird nicht die Modellbindung getestet. Bei diesen Komponententests wird nur das Verhalten des Codes in der Aktionsmethode getestet.

Im zweiten Test wird überprüft, ob bei gültigem `ModelState` eine neue `BrainstormSession` (über das Repository) hinzugefügt wird. Die Methode gibt ein `RedirectToActionResult` mit den erwarteten Eigenschaften zurück. Simulierte Aufrufe, die nicht aufgerufen werden, werden normalerweise ignoriert. Durch Aufrufen von `Verifiable` am Ende des Einrichtungsaufrufs können sie im Test jedoch überprüft werden. Dies erfolgt mit dem Aufruf `mockRepo.Verify`. Damit schlägt der Test fehl, wenn die erwartete Methode nicht aufgerufen wurde.

> [!NOTE]
> Mit der in diesem Beispiel verwendeten Moq-Bibliothek können überprüfbare (oder „strikte“) Pseudoobjekte mit nicht überprüfbaren Pseudoobjekten (auch „nicht-strikte“ Pseudoobjekte oder „Stubs“ genannt) ganz einfach kombiniert werden. Weitere Informationen finden Sie unter [Customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior) (Anpassen des Verhaltens von Pseudoobjekten mit Moq).

Ein anderer Controller in der App zeigt Informationen zu einer bestimmten Brainstormingsitzung an. Er verfügt auch über Programmlogik für den Umgang mit ungültigen ID-Werten:

[!code-csharp[](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Die Controlleraktion verfügt über drei Fälle, die getestet werden können, und zwar einen für jede `return`-Anweisung:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Die App stellt Funktionen als Web-API bereit (eine Liste mit Ideen, die einer Brainstormingsitzung zugeordnet werden, sowie eine Methode zum Hinzufügen von neuen Ideen zu einer Sitzung):

<a name="ideas-controller"></a>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

Die `ForSession`-Methode gibt eine Liste von `IdeaDTO`-Typen zurück. Geben Sie Ihre Geschäftsdomänenentitäten nicht direkt über API-Aufrufe zurück, da sie häufig mehr Daten enthalten, als die Client-API anfordert. Außerdem verknüpfen sie unnötigerweise das interne Domänenmodell Ihrer App mit der extern zur Verfügung gestellten API. Die Zuordnung zwischen Domänenentitäten und den Typen, die Sie über das Netzwerk zurückgeben, kann manuell ausgeführt werden (wie hier gezeigt mithilfe von LINQ-`Select`) oder mithilfe einer Bibliothek wie etwa [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Die Komponententests für die API-Methoden `Create` und `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Fügen Sie wie bereits erwähnt dem Controller einen Modellfehler als Teil des Tests hinzu, um das Verhalten der Methode bei ungültigem `ModelState` zu testen. Testen Sie in den Komponententests nicht die Modellvalidierung oder -bindung. Testen Sie nur das Verhalten Ihrer Aktionsmethode bei einem bestimmten `ModelState`-Wert.

Der zweite Test hängt davon ab, ob das Repository NULL zurückgibt. Daher wird das Pseudorepository so konfiguriert, dass es NULL zurückgibt. Es ist nicht nötig, eine Testdatenbank (z.B. im Arbeitsspeicher) und eine Abfrage zu erstellen, die dieses Ergebnis zurückgibt. Dies können Sie auch wie bereits gezeigt in einer einzigen Anweisung erledigen.

Im letzten Test wird überprüft, ob die `Update`-Methode des Repositorys aufgerufen wird. Wie schon zuvor wird das Pseudoobjekt mit `Verifiable` aufgerufen. Anschließend wird die `Verify`-Methode des Pseudorepositorys aufgerufen, um zu bestätigen, dass die überprüfbare Methode ausgeführt wurde. Es gehört nicht zu den Aufgaben eines Komponententests sicherzustellen, dass die Daten von der `Update`-Methode gespeichert wurden. Dies kann mit einem Integrationstest durchgeführt werden.

## <a name="integration-testing"></a>Integrationstests

Mit [Integrationstests](../../testing/integration-testing.md) soll sichergestellt werden, dass die separaten Module innerhalb einer App korrekt zusammenarbeiten. Alle Aspekte, die Sie mit einem Komponententest testen können, können in der Regel auch mit einem Integrationstest getestet werden. Umgekehrt gilt diese Aussage jedoch nicht. Integrationstests sind jedoch meist viel langsamer als Komponententests. Daher sollten Sie wann immer möglich Komponententests durchführen und Integrationstests nur für Szenarios verwenden, an denen mehrere Mitwirkende beteiligt sind.

Auch wenn sie manchmal nützlich sein können, werden Pseudoobjekte selten in Integrationstests verwendet. In Komponententests kann mit Pseudoobjekten effektiv überprüft werden, wie sich Mitwirkende außerhalb der getesteten Komponente zu Testzwecken verhalten sollten. In einem Integrationstest sollen echte Mitwirkende bestätigen, dass das gesamte Subsystem korrekt zusammenarbeitet.

### <a name="application-state"></a>Status der Anwendung

Ein wichtiger Aspekt beim Ausführen von Integrationstests ist die Festlegung des Status der Anwendung. Tests müssen unabhängig voneinander ausgeführt werden. Daher sollte sich die App zu Beginn jedes Tests in einem bekannten Zustand befinden. Dies ist kein Problem, wenn Ihre App eine Datenbank oder einen Persistenzspeicher verwendet. Die meisten echten Apps speichern ihren Status in der Praxis jedoch auf einem Datenspeicher. Daher könnte jede Änderung durch einen Test Auswirkungen auf weitere Tests besitzen, sofern der Datenspeicher nicht zurückgesetzt wird. Wenn Sie den integrierten `TestServer` verwenden, können Sie in den Integrationstests auf einfache Weise ASP.NET Core-Apps hosten. Doch damit erhalten Sie nicht unbedingt Zugriff auf die verwendeten Daten. Wenn Sie eine tatsächliche Datenbank verwenden, können Sie beispielsweise eine Verbindung zwischen der App und einer Testdatenbank herstellen, auf die die Tests Zugriff haben. So wird sichergestellt, dass die App in einen bekannten Zustand zurückgesetzt wird, bevor der nächste Test ausgeführt wird.

In dieser Beispielanwendung wird die Unterstützung der InMemoryDatabase von Entity Framework Core verwendet. Daher kann für das Testprojekt nicht einfach eine Verbindung hergestellt werden. Stattdessen wird eine `InitializeDatabase`-Methode aus der `Startup`-Klasse der App bereitgestellt, die beim Starten der App aufgerufen wird, wenn sie sich in der `Development`-Umgebung befindet. Die Integrationstests profitieren hiervon automatisch, solange die Umgebung auf `Development` festgelegt ist. So müssen Sie nicht an das Zurücksetzen der Datenbank denken, da die InMemoryDatabase bei jedem Starten der App zurückgesetzt wird.

Die `Startup`-Klasse:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Die in Integrationstests häufig verwendete `GetTestSession`-Methode wird weiter unten gezeigt.

### <a name="accessing-views"></a>Zugreifen auf Ansichten

In jeder Integrationstestklasse wird der `TestServer` konfiguriert, der die ASP.NET Core-App ausführt. Standardmäßig hostet der `TestServer` die Web-App in dem Ordner, in dem sie ausgeführt wird – in diesem Fall dem Testprojektordner. Wenn Sie versuchen, Controlleraktionen zu testen, die `ViewResult` zurückgeben, wird Ihnen daher möglicherweise der folgende Fehler angezeigt:

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

Zum Beheben dieses Problems müssen Sie das Inhaltsstammverzeichnis des Servers so konfigurieren, dass es die Ansichten für das getestete Projekt finden kann. Rufen Sie hierzu wie folgt `UseContentRoot` in der `TestFixture`-Klasse auf:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

Die `TestFixture`-Klasse dient zum Konfigurieren und Erstellen des `TestServer` sowie zum Einrichten eines `HttpClient` zur Kommunikation mit dem `TestServer`. Jeder Integrationstest verwendet die `Client`-Eigenschaft, um eine Verbindung zum Testserver herzustellen und eine Anforderung zu senden.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

Im ersten Test weiter oben enthält `responseString` den tatsächlich gerenderten HTML-Code aus der Ansicht. Dieser kann überprüft werden, um sicherzustellen, dass er die erwarteten Ergebnisse enthält.

Im zweiten Test wird eine Formularbereitstellung mit einem eindeutigen Sitzungsnamen erstellt und für die App bereitgestellt. Dann wird überprüft, ob die erwartete Umleitung zurückgegeben wird.

### <a name="api-methods"></a>API-Methoden

Stellt Ihrer App Web-APIs bereit, sollten Sie mit automatisierten Tests bestätigen lassen, dass diese erwartungsgemäß ausgeführt werden. Der integrierte `TestServer` erleichtert das Testen von Web-APIs. Wenn Ihre API-Methoden Modellbindung verwenden, sollten Sie `ModelState.IsValid` immer überprüfen. Integrationstests eignen sich hervorragend dazu, das korrekte Funktionieren der Modellvalidierung zu überprüfen.

Die folgenden Tests beziehen sich auf die `Create`-Methode in der oben gezeigten [IdeasController](xref:mvc/controllers/testing#ideas-controller)-Klasse:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

Im Gegensatz zu den Integrationstests für Aktionen, die HTML-Ansichten zurückgeben, können Web-API-Methoden, die Ergebnisse zurückgeben, in der Regel wie im letzten Test oben gezeigt als stark typisierte Objekte deserialisiert werden. In diesem Fall deserialisiert der Test das Ergebnis in eine `BrainstormSession`-Instanz und bestätigt, dass die Idee der Liste von Ideen korrekt hinzugefügt wurde.

Weitere Beispiele für Integrationstests finden Sie im [Beispielprojekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) für diesen Artikel.
