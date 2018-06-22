---
title: Razor-Seiten Komponententests in ASP.NET Core
author: guardrex
description: Informationen Sie zum Erstellen von Komponententests für Razor-Seiten-apps.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: bde1bef78fcc7ac1d570057d54636ea0f5490de8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274406"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor-Seiten Komponententests in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

ASP.NET Core unterstützt Komponententests von Razor-Seiten-apps. Tests für die Daten zuzugreifen (Layer, DAL), und gewährleisten Sie Seite Modelle:

* Teile einer app Razor-Seiten arbeiten unabhängig voneinander und zusammen als eine Einheit während der Erstellung der app an.
* Klassen und Methoden haben Bereiche Verantwortung beschränkt.
* Zusätzlicher Dokumentation vorhanden ist, auf wie die Anwendung Verhalten soll.
* Regressionen, die Fehler, die durch das Updates auf den Code zu geschaltet sind, werden während der automatischen Erstellung und Bereitstellung gefunden.

In diesem Thema wird davon ausgegangen, dass Sie ein grundlegendes Verständnis der Razor-Seiten-apps und Komponententests verfügen. Wenn Sie mit Razor-Seiten-apps oder Test-Konzepten nicht vertraut sind, finden Sie unter den folgenden Themen:

* [Introduction to Razor Pages (Einführung in Razor Pages)](xref:razor-pages/index)
* [Erste Schritte mit Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [UnitTests von c# in .NET Core mit Dotnet Test- und xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Das Beispielprojekt besteht aus zwei Web-apps:

| App         | Projektordner                        | Beschreibung |
| ----------- | ------------------------------------- | ----------- |
| Nachrichten-app | *Src/RazorPagesTestSample*            | Ermöglicht es einem Benutzer hinzufügen, löschen Sie eine, löschen Sie alle und Analysieren von Nachrichten. |
| Test-app    | *tests/RazorPagesTestSample.Tests*    | Komponententest die Nachrichten-app verwendete: Datenzugriff Layer (DAL) und Index-Seitenmodell. |

Die Tests können ausgeführt werden, z. B. Verwenden von der integrierten Test Funktionen eine IDE [Visual Studio](https://www.visualstudio.com/vs/). Wenn [Visual Studio Code](https://code.visualstudio.com/) oder der Befehlszeile, und führen Sie den folgenden Befehl an einer Eingabeaufforderung in das *tests/RazorPagesTestSample.Tests* Ordner:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organisation der Nachrichten-app

Die Nachrichten-app ist ein einfaches Razor-Seiten Nachrichtensystem, das mit den folgenden Merkmalen:

* Die Indexseite der app (*Pages/Index.cshtml* und *Pages/Index.cshtml.cs*) eine Benutzeroberfläche und die Seite stellt Modellmethoden bereit, um zu steuern, die Hinzufügung, löschen und Analyse von Nachrichten (durchschnittliche Wörtern pro Nachricht) .
* Eine Nachricht wird beschrieben, durch die `Message` Klasse (*Data/Message.cs*) mit zwei Eigenschaften: `Id` (Schlüssel) und `Text` (Nachricht). Die `Text` Eigenschaft ist erforderlich, und auf 200 Zeichen beschränkt.
* Nachrichten werden gespeichert mit [Entity Framework des speicherinternen Datenbank](/ef/core/providers/in-memory/)&#8224;.
* Die app enthält einen Datenzugriffsebene (DAL) in seiner Datenbank Context-Klasse `AppDbContext` (*Data/AppDbContext.cs*). Die DAL-Methoden werden markiert `virtual`, dadurch wird die Methoden für die Verwendung in den Tests imitieren.
* Wenn die Datenbank auf app-Starts leer ist, wird der Nachrichtenspeicher mit drei Nachrichten initialisiert. Diese *Seeding Nachrichten* werden auch in Tests verwendet.

&#8224;Das Thema EF [Test mit InMemory](/ef/core/miscellaneous/testing/in-memory), wird erläutert, wie eine Datenbank im Arbeitsspeicher für Tests mit MSTest verwendet. In diesem Thema verwendet die [xUnit](https://xunit.github.io/) Testframework. Test-Konzepte und Test-Implementierungen in anderen Testframeworks sind ähnlich, aber nicht identisch.

Auch wenn die app nicht verwendet die [Repositorymusters](http://martinfowler.com/eaaCatalog/repository.html) ist ein Beispiel für effektive nicht die [Arbeitseinheit (UoW) Muster](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor-Seiten unterstützt diese Muster der Entwicklung. Weitere Informationen finden Sie unter [Entwerfen der Infrastruktur Persistenzebene](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementieren das Repository und die Einheit der Arbeit Muster in einer ASP.NET MVC-Anwendung](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), und [Testcontroller Logik](/aspnet/core/mvc/controllers/testing) (im Beispiel wird das Repositorymuster implementiert).

## <a name="test-app-organization"></a>Testen von app-Organisation

Die Test-app ist eine Konsolen-app innerhalb der *tests/RazorPagesTestSample.Tests* Ordner.

| Test-app-Ordners | Beschreibung |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* der Komponententests enthält, damit die DAL.</li><li>*IndexPageTests.cs* Komponententests für das Modell Index enthält.</li></ul> |
| *Hilfsprogramme*     | Enthält die `TestingDbContextOptions` Methode verwendet, um die neue Datenbank Kontextoptionen für die einzelnen DAL-Komponententests erstellen, sodass die Datenbank auf die Baseline-Bedingung für jeden Test zurückgesetzt wird. |

Das Testframework ist [xUnit](https://xunit.github.io/). Das Objekt, das imitieren Framework ist [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Komponententests für die Daten zuzugreifen (Layer, DAL)

Die Nachricht app hat eine DAL mit vier Methoden in der `AppDbContext` Klasse (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Jede Methode hat ein oder zwei Komponententests im Test-app.

| Die DAL-Methode               | Funktion                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Ruft eine `List<Message>` aus der Datenbank, sortiert nach der `Text` Eigenschaft. |
| `AddMessageAsync`        | Fügt eine `Message` mit der Datenbank.                                          |
| `DeleteAllMessagesAsync` | Löscht alle `Message` Einträge aus der Datenbank.                           |
| `DeleteMessageAsync`     | Löscht eine einzelne `Message` aus der Datenbank durch `Id`.                      |

Komponententests für die DAL erfordern [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) beim Erstellen eines neuen `AppDbContext` für jeden Test. Ein Ansatz zum Erstellen der `DbContextOptions` für jeden Test ist die Verwendung einer [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Das Problem bei diesem Ansatz ist, dass jeder Test die Datenbank im unabhängig von Ihrem Status der vorherige Test sie angehalten wurde erhält. Dies kann problematisch sein, beim Versuch, das atomarische Komponententests schreiben, die nicht miteinander in Konflikt. Erzwingen der `AppDbContext` um einen neuen Datenbankkontext für jeden Test verwenden möchten, geben Sie eine `DbContextOptions` -Instanz, die basierend auf einer neuen Service-Anbieter. Die Test-app zeigt, wie Sie diesen Vorgang mit seiner `Utilities` class-Methode `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Mithilfe der `DbContextOptions` in der DAL-Einheit Tests jeder Test automatisch mit einer neuen Datenbank-Instanz ausführen können:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Jede Testmethode in die `DataAccessLayerTest` Klasse (*UnitTests/DataAccessLayerTest.cs*) folgt einem ähnlichen Muster von Act Assert anordnen:

1. Anordnen: Für den Test die Datenbank konfiguriert ist und/oder das erwartete Ergebnis definiert ist.
1. ACT: Der Test ausgeführt wird.
1. Assert-: Assertionen werden, um zu bestimmen, ob das Testergebnis erfolgreich ist.

Z. B. die `DeleteMessageAsync` Methode ist verantwortlich für das Entfernen einer einzelnen Nachricht erkennbar seine `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Es gibt zwei Tests für diese Methode. Ein Test wird überprüft, dass die Methode eine Nachricht löscht, wenn die Nachricht in der Datenbank vorhanden ist. Die andere Methode Tests, die die Datenbank ändern nicht, wenn die Nachricht `Id` für das Löschen nicht vorhanden ist. Die `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Methode wird unten gezeigt:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Die Methode führt zuerst den Diagrammfelder Schritt, abgewickelt Vorbereitung für den Act-Schritt. Das seeding Nachrichten abgerufen und in gespeicherten `seedMessages`. Das seeding Nachrichten werden in der Datenbank gespeichert. Die Nachricht mit einem `Id` von `1` zum Löschen festgelegt ist. Wenn die `DeleteMessageAsync` Methode ausgeführt wird, wird die erwarteten Nachrichten müssen alle Nachrichten mit Ausnahme der Datensatz mit einer `Id` von `1`. Die `expectedMessages` -Variable stellt diese erwartete Ergebnis dar.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Die Methode fungiert: die `DeleteMessageAsync` Methode ausgeführt wird, übergibt der `recId` von `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Zum Schluss die Methode erhält das `Messages` aus dem Kontext und vergleicht ihn mit der `expectedMessages` Bestätigung, dass die beiden gleich sind:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Damit verglichen werden kann, die die beiden `List<Message>` identisch sind:

* Die Nachrichten geordnet `Id`.
* Nachrichtenpaare verglichen werden, auf die `Text` Eigenschaft.

Eine ähnliche Testmethode `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` überprüft das Ergebnis des Versuchs, eine Nachricht zu löschen, die nicht vorhanden ist. In diesem Fall muss die erwarteten Nachrichten in der Datenbank entspricht der tatsächlichen Nachrichten nach der `DeleteMessageAsync` Methode ausgeführt wird. Es darf keine Änderung an der Datenbank-Inhalt:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Komponententests für die Seite "-Modell-Methoden

Eine andere Gruppe von Komponententests ist verantwortlich für Tests der Seitenmethoden-Modell. In der Nachrichten-app befinden sich die Index-Seite-Modelle der `IndexModel` -Klasse im *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Seite "-Modell-Methode | Funktion |
| ----------------- | -------- |
| `OnGetAsync` | Ruft die Nachrichten aus der DAL für die Benutzeroberfläche mit der `GetMessagesAsync` Methode. |
| `OnPostAddMessageAsync` | Wenn die `ModelState` gültig ist, werden Aufrufe `AddMessageAsync` zum Hinzufügen einer Nachricht in der Datenbank. |
| `OnPostDeleteAllMessagesAsync` | Aufrufe `DeleteAllMessagesAsync` alle Nachrichten in der Datenbank gelöscht. |
| `OnPostDeleteMessageAsync` | Führt `DeleteMessageAsync` So löschen Sie eine Nachricht mit der `Id` angegebenen. |
| `OnPostAnalyzeMessagesAsync` | Wenn eine oder mehrere Nachrichten in der Datenbank sind, berechnet die durchschnittliche Anzahl von Wörtern pro Nachricht. |

Die Seite Modellmethoden werden getestet mit sieben Tests in der `IndexPageTests` Klasse (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Die Tests verwenden das vertraute anordnen Assert Act-Muster. Diese Tests konzentrieren:

* Bestimmen, ob die Methoden das richtige Verhalten führen Sie bei der `ModelState` ist ungültig.
* Bestätigen die Methoden der richtigen erzeugen `IActionResult`.
* Überprüfen, dass die Eigenschaft Wert Zuweisungen ordnungsgemäß vorgenommen werden.

Diese Gruppe von Tests häufig simulieren Sie die Methoden der DAL, erzeugen die erwartete Daten für den Act-Schritt, in dem eine Seite-Modell-Methode ausgeführt wird. Z. B. die `GetMessagesAsync` Methode der `AppDbContext` ist Mocks Ausgabe erstellt. Wenn eine Seite Modell Methode diese Methode ausgeführt wird, gibt das Mock das Ergebnis zurück. Die Daten stammen nicht aus der Datenbank. Dadurch wird die vorhersagbare, zuverlässige testbedingungen für die Verwendung von der DAL in die Seite Modell Tests erstellt.

Die `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` testen zeigt wie die `GetMessagesAsync` Methode ist für die Seitenmodell Mocks erstellt:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Wenn die `OnGetAsync` Methode im Act Schritt ausgeführt wird, ruft er die Seitenmodell `GetMessagesAsync` Methode.

Komponententest Act Schritt (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` Seite "-Modell `OnGetAsync` Methode (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Die `GetMessagesAsync` Methode in der DAL ist nicht das Ergebnis für den Aufruf dieser Methode zurück. Die mocked Version der Methode gibt das Ergebnis zurück.

In der `Assert` Schritt, die tatsächlichen Nachrichten (`actualMessages`) zugewiesen sind, aus der `Messages` Eigenschaft des Modells Seite. Eine typüberprüfung wird auch ausgeführt, wenn die Nachrichten zugeordnet sind. Die erwarteten und tatsächlichen Nachrichten von verglichen werden ihre `Text` Eigenschaften. Der Test bestätigt, die die beiden `List<Message>` Instanzen enthalten die gleichen Meldungen.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Andere Tests in dieser Gruppe erstellen Seite Model-Objekte, die implizit enthalten die `DefaultHttpContext`, die `ModelStateDictionary`, wird ein `ActionContext` zum Herstellen der `PageContext`, eine `ViewDataDictionary`, und ein `PageContext`. Diese sind nützlich, in die Tests durchzuführen. Die Nachrichten-app z. B. richtet eine `ModelState` Fehler mit `AddModelError` überprüfen, ob ein gültiger `PageResult` wird zurückgegeben, wenn `OnPostAddMessageAsync` ausgeführt wird:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [UnitTests von c# in .NET Core mit Dotnet Test- und xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testen von Controllern](xref:mvc/controllers/testing)
* [Komponententest-Codes](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Integrationstests](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Erste Schritte mit xUnit.net (.NET Core/ASP.NET Kerne)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq-Schnellstart](https://github.com/Moq/moq4/wiki/Quickstart)
