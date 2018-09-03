---
title: Testen von Controllerlogik in ASP.NET Core
author: ardalis
description: Informationen zum Testen von Controllerlogik in ASP.NET Core mit Moq und xUnit
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751552"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Testen von Controllerlogik in ASP.NET Core

Von [Steve Smith](https://ardalis.com/)

Controller sind ein zentraler Bestandteil jeder ASP.NET Core MVC-Anwendung. Daher sollten Sie auch darauf vertrauen können, dass sie in Ihrer App wie beabsichtigt funktionieren. Automatisierte Tests können Ihnen dieses Vertrauen vermitteln. Sie können zudem dabei helfen, Fehler aufzudecken, bevor diese die Produktion erreichen. Sie sollten auf keinen Fall unnötige Aufgaben innerhalb der Controller platzieren. Zudem sollten die Tests ausschließlich auf die Controlleraufgaben ausgerichtet sein.

Controllerlogik sollte minimal und nicht auf Geschäftslogik oder Infrastruktur ausgerichtet sein (wie z.B. Datenzugriff). Testen Sie die Controllerlogik, nicht das Framework. Testen Sie das *Verhalten* des Controllers bei gültigen oder ungültigen Eingaben. Testen Sie die Antworten des Controllers auf der Grundlage des Ergebnisses des Geschäftsvorgangs, den er ausführt.

Typische Aufgaben des Controllers:

* Überprüfen von `ModelState.IsValid`
* Zurückgeben einer Fehlerantwort, wenn `ModelState` ungültig ist
* Abrufen einer Geschäftsentität aus dem Persistenzspeicher
* Ausführen einer Aktion für die Geschäftsentität
* Speichern der Geschäftsentität im Persistenzspeicher
* Zurückgeben eines entsprechenden `IActionResult`

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Komponententests der Controllerlogik

[Komponententests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) beinhalten das Testen einer App-Komponente isoliert von ihrer Infrastruktur und ihren Abhängigkeiten. Bei einem Komponententest der Controllerlogik werden nur die Inhalte einer einzelnen Aktion getestet, nicht das Verhalten ihrer Abhängigkeiten oder des Frameworks selbst. Konzentrieren Sie sich bei der Durchführung eines Komponententests für Ihre Controlleraktionen daher nur auf deren Verhalten. Bei einem Komponententest des Controllers werden z.B. [Filter](xref:mvc/controllers/filters), [Routing](xref:fundamentals/routing), [Modellbindung](xref:mvc/models/model-binding) o.Ä. vermieden. Durch die Fokussierung auf nur einen Aspekt sind Komponententests in der Regel einfach zu schreiben und schnell in der Ausführung. Gut geschriebene Komponententests können häufig ohne großen Mehraufwand ausgeführt werden. Komponententests erkennen jedoch keine Probleme bei der Interaktion zwischen den Komponenten. Dazu dienen [Integrationstests](xref:test/integration-tests).

Wenn Sie benutzerdefinierte Filter und Routen schreiben, sollten Sie für diese isoliert einen Komponententest durchführen, der nicht Bestandteil eines Tests für eine bestimmte Controlleraktion ist.

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

Im ersten Test wird bestätigt, dass bei ungültigem `ModelState` das gleiche `ViewResult` zurückgegeben wird wie für eine `GET`-Anforderung. Beachten Sie, dass der Test für ein ungültiges Modell gar nicht erfolgreich durchgeführt werden soll. Da die Modellbindung nicht ausgeführt wird, wäre dies ohnehin nicht möglich (obwohl bei einem [Integrationstest](xref:test/integration-tests) eine Trainingsmodellbindung verwendet wird). In diesem Fall wird nicht die Modellbindung getestet. Bei diesen Komponententests wird nur das Verhalten des Codes in der Aktionsmethode getestet.

Im zweiten Test wird überprüft, ob bei gültigem `ModelState` eine neue `BrainstormSession` (über das Repository) hinzugefügt wird. Die Methode gibt ein `RedirectToActionResult` mit den erwarteten Eigenschaften zurück. Simulierte Aufrufe, die nicht aufgerufen werden, werden normalerweise ignoriert. Durch Aufrufen von `Verifiable` am Ende des Einrichtungsaufrufs können sie im Test jedoch überprüft werden. Dies erfolgt mit dem Aufruf `mockRepo.Verify`. Damit schlägt der Test fehl, wenn die erwartete Methode nicht aufgerufen wurde.

> [!NOTE]
> Mit der in diesem Beispiel verwendeten Moq-Bibliothek können überprüfbare (oder „strikte“) Pseudoobjekte mit nicht überprüfbaren Pseudoobjekten (auch „nicht-strikte“ Pseudoobjekte oder „Stubs“ genannt) ganz einfach kombiniert werden. Weitere Informationen finden Sie unter [Customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior) (Anpassen des Verhaltens von Pseudoobjekten mit Moq).

Ein anderer Controller in der App zeigt Informationen zu einer bestimmten Brainstormingsitzung an. Er verfügt auch über Programmlogik für den Umgang mit ungültigen ID-Werten:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Die Controlleraktion verfügt über drei Fälle, die getestet werden können, und zwar einen für jede `return`-Anweisung:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Die App stellt Funktionen als Web-API bereit (eine Liste mit Ideen, die einer Brainstormingsitzung zugeordnet werden, sowie eine Methode zum Hinzufügen von neuen Ideen zu einer Sitzung):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

Die `ForSession`-Methode gibt eine Liste von `IdeaDTO`-Typen zurück. Geben Sie Ihre Geschäftsdomänenentitäten nicht direkt über API-Aufrufe zurück, da sie häufig mehr Daten enthalten, als die Client-API anfordert. Außerdem verknüpfen sie unnötigerweise das interne Domänenmodell Ihrer App mit der extern zur Verfügung gestellten API. Die Zuordnung zwischen Domänenentitäten und den Typen, die Sie über das Netzwerk zurückgeben, kann manuell ausgeführt werden (wie hier gezeigt mithilfe von LINQ-`Select`) oder mithilfe einer Bibliothek wie etwa [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Die Komponententests für die API-Methoden `Create` und `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Fügen Sie wie bereits erwähnt dem Controller einen Modellfehler als Teil des Tests hinzu, um das Verhalten der Methode bei ungültigem `ModelState` zu testen. Testen Sie in den Komponententests nicht die Modellvalidierung oder -bindung. Testen Sie nur das Verhalten Ihrer Aktionsmethode bei einem bestimmten `ModelState`-Wert.

Der zweite Test hängt davon ab, ob das Repository NULL zurückgibt. Daher wird das Pseudorepository so konfiguriert, dass es NULL zurückgibt. Es ist nicht nötig, eine Testdatenbank (z.B. im Arbeitsspeicher) und eine Abfrage zu erstellen, die dieses Ergebnis zurückgibt. Dies können Sie auch wie bereits gezeigt in einer einzigen Anweisung erledigen.

Im letzten Test wird überprüft, ob die `Update`-Methode des Repositorys aufgerufen wird. Wie schon zuvor wird das Pseudoobjekt mit `Verifiable` aufgerufen. Anschließend wird die `Verify`-Methode des Pseudorepositorys aufgerufen, um zu bestätigen, dass die überprüfbare Methode ausgeführt wurde. Es gehört nicht zu den Aufgaben eines Komponententests sicherzustellen, dass die Daten von der `Update`-Methode gespeichert wurden. Dies kann mit einem Integrationstest durchgeführt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:test/integration-tests>
