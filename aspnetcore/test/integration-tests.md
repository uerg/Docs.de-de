---
title: Integrationstests im ASP.NET Core
author: guardrex
description: In diesem Artikel erfahren Sie, wie Integrationstests sicherstellen, dass die Komponenten einer App auf der Infrastrukturebene ordnungsgemäß funktionieren, einschließlich der Datenbank, dem Dateisystem und dem Netzwerk.
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 1895b06f1af9a9eb66c14aa5c7834497fc95d583
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277695"
---
# <a name="integration-tests-in-aspnet-core"></a>Integrationstests im ASP.NET Core

Durch [Luke Latham](https://github.com/guardrex) und [Steve Smith](https://ardalis.com/)

Integrationstests stellen Sie sicher, dass ein app-Komponenten ordnungsgemäß auf einer Ebene ausgeführt werden, die die app unterstützende Infrastruktur, z. B. die Datenbank, Dateisystem und Netzwerk enthält. ASP.NET Core unterstützt Integrationstests mithilfe eines Komponententest-Frameworks mit einem Test-Webhost und eine in-Memory-Testserver.

In diesem Thema wird angenommen, ein grundlegendes Verständnis von Komponententests. Wenn Sie nicht mit Test-Konzepten vertraut sind, finden Sie unter der [Komponententests in .NET Core und .NET Standard](/dotnet/core/testing/) Thema und seine verknüpften Inhalt.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Die Beispiel-app ist eine app Razor-Seiten und geht davon aus ein grundlegendes Verständnis der Razor-Seiten. Wenn mit Razor-Seiten nicht vertraut sind, finden Sie unter den folgenden Themen:

* [Introduction to Razor Pages (Einführung in Razor Pages)](xref:razor-pages/index)
* [Erste Schritte mit Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Komponententests für Razor-Seiten](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Einführung in die Integrationstests erfolgreich

Integrationstests Auswerten einer app-Komponenten auf einer größeren Ebene als [Komponententests](/dotnet/core/testing/). Komponententests werden verwendet, um isolierten Softwarekomponenten, wie einzelne Klasse, Methoden zu testen. Integrationstests Vergewissern Sie sich, dass zwei oder mehr app-Komponenten zusammenarbeiten, um ein erwartetes Ergebnis, möglicherweise auch jede Komponente erforderlich, um eine Anforderung zu verarbeiten zu erzeugen.

Diese breiter angelegte Tests dienen zum Testen der app-Infrastruktur und gesamte Framework, häufig mit folgenden Komponenten:

* Datenbank
* Dateisystem
* Netzwerkgeräte
* Anforderung / Antwort-pipeline

Komponententests verwenden erstellt Komponenten, bekannt als *fakes* oder *Modellieren der Objekte*, anstelle von Infrastrukturkomponenten.

Im Gegensatz zu Komponententests tests Integration:

* Verwenden Sie die tatsächlichen Komponenten, die die app in der Produktion verwendet.
* Benötigen Sie weitere Code und die Daten verarbeitet.
* Dauern Sie zum Ausführen länger.

Aus diesem Grund wird beschränken Sie die Verwendung des Integrationstests für die wichtigsten Infrastruktur-Szenarien. Wenn ein Verhalten mit einem Komponententest oder einen Integrationstest getestet werden kann, wählen Sie den Komponententest aus.

> [!TIP]
> Schreiben Sie nicht für alle möglichen Permutation der Daten und den Zugriff mit Datenbanken und Dateisysteme Integrationstests. Unabhängig davon, wie viele über eine app stellen die Interaktion mit Datenbanken und Dateisysteme können, mit Fokus lesen, schreiben, Update und Delete-Integration Tests werden in der Regel ausreichend Testdatenbank können Datei- und Systemkomponenten aus. Verwenden Sie Komponententests für routinemäßige Tests der Methodenlogik, die mit diesen Komponenten interagieren. In Komponententests Fakes/Mocks die Verwendung von Infrastruktur zu schnellere Ausführung des Tests.

> [!NOTE]
> In Diskussionen Integrationstests, das getestete Projekt wird häufig aufgerufen, die *getestetes*, oder "SUT" kurz.

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core Integrationstests

Integrationstests im ASP.NET Core benötigen Folgendes:

* Ein Testprojekt dient zum enthalten und die Tests auszuführen. Das Testprojekt enthält einen Verweis auf das getestete Projekt zu ASP.NET Core, bezeichnet der *getestetes* (SUT). _"SUT" ist in diesem Thema zum Verweisen auf die getestete app verwendet._
* Das Testprojekt einen Webhost Test für die SUT erstellt und verwendet einen Server-Testclient, um Anforderungen und Antworten auf die SUT zu behandeln.
* Ein Test Runner wird verwendet, um die Tests und den Aufrufstrukturbericht führen Sie die Testergebnisse.

Integrationstests führen Sie eine Sequenz von Ereignissen, die die üblichen *anordnen*, *Act*, und *Assert* Testschritte:

1. Die SUT Webhost ist konfiguriert.
1. Ein Server-Testclient wird zum Senden von Anforderungen an die app erstellt.
1. Die *anordnen* Testschritt wird ausgeführt: die Test-app wird vorbereitet, eine Anforderung.
1. Die *Act* Testschritt wird ausgeführt: der Client sendet die Anforderung und die Antwort empfängt.
1. Die *Assert* Testschritt ausgeführt wird: der *tatsächliche* Antwort überprüft wird, als eine *übergeben* oder *fehlschlagen* basierend auf einer *erwartet*  Antwort.
1. Der Prozess wird fortgesetzt, bis alle Tests ausgeführt werden.
1. Die Testergebnisse werden gemeldet.

In der Regel wird der Test-Webhost anders konfiguriert, als die app normal Webhost für den Test ausgeführt wird. Beispielsweise können eine andere Datenbank oder andere app-Einstellungen für die Tests verwendet werden.

Infrastrukturkomponenten, wie z. B. die Test-Webhost und in-Memory-Testserver ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), bereitgestellt oder verwaltet von der [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) Paket. Mithilfe dieses Pakets optimiert die Test-Erstellung und Ausführung.

Die `Microsoft.AspNetCore.Mvc.Testing` Paket behandelt die folgenden Aufgaben:

* Kopiert die Abhängigkeitsdatei (*\*.deps*) aus der SUT in des Testprojekts *"bin"* Ordner.
* Die Inhalten-Stamm festgelegt, die SUT Projektstamm so, dass statische Dateien und Seiten-Ansichten gefunden werden, wenn die Tests ausgeführt werden.
* Stellt die [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) Klasse für die Optimierung der SUT mit bootstrapping `TestServer`.

Die [Komponententests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) Dokumentation wird beschrieben, wie ein Projekt und eine Testseite Testprogramm, sowie detaillierte Anweisungen zum Ausführen von Tests und Empfehlungen zum bis zu testen, und Testklassen einrichten.

> [!NOTE]
> Wenn Sie ein Testprojekt für eine app zu erstellen, können trennen Sie die Komponententests von der Integrationstests, in unterschiedlichen Projekten. Dadurch wird sichergestellt, dass Tests Infrastrukturkomponenten nicht versehentlich in den Komponententests enthalten sind. Trennung von Einheiten- und Integration Tests kann auch steuern, über welchen Satz von Tests ausgeführt werden.

Es ist praktisch keinen Unterschied zwischen der Konfiguration für Tests der apps mit Razor-Seiten und MVC-apps. Der einzige Unterschied besteht darin wie die Tests benannt werden. In einer app Razor-Seiten Tests Seite Endpunkte in der Regel nach der Seite Modellklasse benannt (z. B. `IndexPageTests` zum Testen der Komponentenintegration für die Seite "Index"). In einer MVC-app, Tests sind in der Regel nach Controllerklassen organisiert und mit dem Namen nach den Domänencontrollern, die sie testen (z. B. `HomeControllerTests` So testen Sie die Komponentenintegration für den Home-Controller).

## <a name="test-app-prerequisites"></a>Testen von app-Voraussetzungen

Das Testprojekt muss:

* Haben Sie einen Paket-Verweis für [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Verwenden Sie das Webdienst-SDK in der Projektdatei (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Diese Prerequesities im überwachungsarbeitsbereich der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Überprüfen Sie die *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* Datei.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Grundlegende Tests mit dem Standardnamen WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) dient zum Erstellen einer [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) für die Integrationstests erfolgreich. `TEntryPoint` die Einstiegspunktklasse an, der die SUT in der Regel ist die `Startup` Klasse.

Test Klassen implementieren eine *Klasse Dienstanwendungsproxy* Schnittstelle (`IClassFixture`) an, dass die Klasse Tests enthält, und geben Sie freigegebene Objektinstanzen zwischen den Tests in der Klasse.

### <a name="basic-test-of-app-endpoints"></a>Grundlegenden Test des Anwendungsendpunkte

Folgende Testklasse `BasicTests`, verwendet der `WebApplicationFactory` Bootstrapping für den SUT und Bereitstellen einer ["HttpClient"](/dotnet/api/system.net.http.httpclient) mit einer Testmethode `Get_EndpointsReturnSuccessAndCorrectContentType`. Die Methode überprüft, ob der Statuscode der Antwort erfolgreich ist (im Bereich 200 299 Statuscodes) und die `Content-Type` Header ist `text/html; charset=utf-8` für mehrere app-Seiten.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) erstellt eine Instanz des `HttpClient` , der automatisch leitet folgt und verarbeitet Cookies.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Einen sicheren Endpunkt testen

Ein weiterer Test in der `BasicTests` -Klasse prüft, dass ein sicherer Endpunkt einen nicht authentifizierten Benutzer an die app-Anmeldeseite umgeleitet.

In der SUT der `/SecurePage` Seite verwendet ein [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konvention Zuweisen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf der Seite. Weitere Informationen finden Sie unter [Razor-Seiten Autorisierung Konventionen](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

In der `Get_SecurePageRequiresAnAuthenticatedUser` zu testen, eine [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) wird festgelegt, leitet zu unterbinden, indem Sie festlegen [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) auf `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Durch das untersagen von des Clients, um die Umleitung zu folgen, können die folgenden Prüfungen vorgenommen werden:

* Der Statuscode der SUT zurückgegebenes kann überprüft werden, anhand der erwarteten [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) Ergebnis, nicht den endgültigen Status-Code nach der Umleitung zu der Anmeldeseite wäre [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* Die `Location` Headerwert in der Antwort-Header wird überprüft, um sicherzustellen, dass er mit beginnt `http://localhost/Identity/Account/Login`, nicht die letzte Anmeldung Seitenantwort, in denen die `Location` Header wäre nicht vorhanden sein.

Weitere Informationen zu `WebApplicationFactoryClientOptions`, finden Sie unter der [Clientoptionen](#client-options) Abschnitt.

## <a name="customize-webapplicationfactory"></a>Anpassen von WebApplicationFactory

Web-Hostkonfiguration kann unabhängig von der Testklassen erstellt werden, durch Vererben von `WebApplicationFactory` mindestens einen benutzerdefinierten Factorys zu erstellen:

1. Erben von `WebApplicationFactory` und überschreiben [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). Die [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ermöglicht die Konfiguration der die Auflistung mit [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Seeding der Datenbank die [Beispielapp](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) erfolgt durch die `InitializeDbForTests` Methode. Die Methode wird beschrieben, der [Integration Beispiel testet: Testen von app-Organisation](#test-app-organization) Abschnitt.

2. Verwenden Sie die benutzerdefinierte `CustomWebApplicationFactory` in Testklassen. Im folgenden Beispiel wird die Factory, in der `IndexPageTests` Klasse:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Die Beispiel-app-Client ist so konfiguriert, dass zu verhindern, dass die `HttpClient` aus folgenden leitet. Wie in beschrieben die [einen sicheren Endpunkt testen](#test-a-secure-endpoint) Abschnitt erlaubt die Tests, um das Ergebnis der ersten Antwort von der app zu überprüfen. Die erste Antwort ist eine Umleitung in vielen dieser Tests mit einer `Location` Header.

3. Ein typische Test verwendet werden, die `HttpClient` und Hilfsmethoden zum Verarbeiten der Anforderung und Antwort:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Alle POST-Anforderung an die SUT erfüllen muss die antiforgery Überprüfung, die automatisch von der app ausgeführt wird [antiforgery Datenschutzsystem](xref:security/data-protection/introduction). Um für einen Test, POST-Anforderung anordnen zu können, müssen die Test-app:

1. Stellen Sie eine Anforderung für die Seite.
1. Analysieren der antiforgery Cookie und Validierung Anforderungstoken aus der Antwort.
1. Stellen Sie die POST-Anforderung mit der antiforgery Cookie und Anforderung token vorhanden.

Die `SendAsync` Erweiterung Hilfsmethoden (*Helpers/HttpClientExtensions.cs*) und die `GetDocumentAsync` Hilfsmethode (*Helpers/HtmlHelpers.cs*) in der [-Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) verwenden die [AngleSharp](https://anglesharp.github.io/) Parser behandelt die antiforgery Überprüfung mit den folgenden Methoden:

* `GetDocumentAsync` &ndash; Empfängt die [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) und gibt eine `IHtmlDocument`. `GetDocumentAsync` eine Factory, die vorbereitet verwendet eine *virtuellen Antwort* basierend auf dem ursprünglichen `HttpResponseMessage`. Weitere Informationen finden Sie unter der [AngleSharp Dokumentation](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` Erweiterungsmethoden für die `HttpClient` Verfassen einer [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) , und rufen Sie [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) der SUT anfordern. Für Überladungen `SendAsync` akzeptieren Sie die HTML-Formular (`IHtmlFormElement`) und die folgenden:
  - Schaltfläche der Form senden (`IHtmlElement`)
  - Formular Werte Auflistung (`IEnumerable<KeyValuePair<string, string>>`)
  - Schaltfläche zum Absenden (`IHtmlElement`) und Werte bilden (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) ist ein Drittanbieter-Analyse für Demonstrationszwecke in diesem Thema und die Beispiel-app verwendete Bibliothek. AngleSharp wird nicht unterstützt, oder für Integrationstests für ASP.NET Core apps erforderlich sind. Andere Parser können verwendet werden, z. B. die [Html Agilität Pack (HAP)](http://html-agility-pack.net/). Ein anderer Ansatz besteht darin Code schreiben, um das antiforgery System Überprüfung Anforderungstoken und antiforgery Cookie direkt behandeln.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Den Client mit WithWebHostBuilder anpassen

Wenn eine zusätzliche Konfiguration innerhalb einer Testmethode erforderlich ist [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) erstellt ein neues `WebApplicationFactory` mit einer [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , die durch die Konfiguration weiter angepasst wird.

Die `Post_DeleteMessageHandler_ReturnsRedirectToRoot` Testmethode, der die [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) veranschaulicht die Verwendung von `WithWebHostBuilder`. Diesem Test wird ein Datensatz löschen in der Datenbank durch das Auslösen einer Formularübergabe in der SUT ausgeführt.

Da eine andere Testen in der `IndexPageTests` Klasse führt einen Vorgang, werden alle Datensätze in der Datenbank gelöscht und kann vor dem Ausführen, der `Post_DeleteMessageHandler_ReturnsRedirectToRoot` -Methode, die Datenbank wird in dieser Testmethode, um sicherzustellen, dass ein Datensatz vorhanden ist, für die SUT löschen ist mit Anfangsdaten gefüllt. Auswählen der `deleteBtn1` Schaltfläche der `messages` Formular in der SUT wird simuliert, in der Anforderung an die SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>-Clientoptionen

Die folgende Tabelle zeigt die [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) verfügbar, für die Erstellung `HttpClient` Instanzen.

| Option | Beschreibung | Standard |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Ruft ab oder legt sie fest, und zwar unabhängig davon, ob `HttpClient` Instanzen sollten automatisch befolgen Redirect Antworten. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Ruft ab oder legt die Basisadresse des `HttpClient` Instanzen. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Ruft ab oder legt sie fest, ob `HttpClient` Instanzen Cookies behandeln soll. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Ruft ab oder legt die maximale Anzahl der Umleitung Antworten, die `HttpClient` Instanzen befolgen. | 7 |

Erstellen der `WebApplicationFactoryClientOptions` Klasse, und übergeben Sie sie an der [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) Methode (Standard sind Werte im Codebeispiel gezeigt):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Wie den Inhalt Stammpfad für die app leitet der Testinfrastruktur

Die `WebApplicationFactory` Konstruktor leitet den Stammpfad der Anwendung Inhalt durch Suchen nach einem [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) für die Assembly, die die Integrationstests erfolgreich mit einem Schlüssel gleich enthält die `TEntryPoint` Assembly `System.Reflection.Assembly.FullName`. Für den Fall, dass ein Attribut mit dem richtigen Schlüssel nicht gefunden wird, `WebApplicationFactory` ausgewichen, auf die Suche nach einer Projektmappendatei (*\*sln*) und fügt die `TEntryPoint` Assemblynamen für das Projektmappenverzeichnis. Das app-Stammverzeichnis (der Inhalt Stammpfad) wird zur Ermittlung Ansichten und Inhaltsdateien verwendet.

In den meisten Fällen ist es nicht erforderlich, explizit festzulegen, die app-Inhalts-Stamm Suchlogik normalerweise die richtige Content Stammverzeichnis zur Laufzeit findet. In besonderen Szenarien, in denen die Inhalte-Stamm, nicht gefunden wird, mithilfe der integrierten Suchalgorithmus, die app-Inhalte Stamm kann explizit oder mithilfe von benutzerdefinierter Logik angegeben werden. Rufen Sie zum Festlegen der app-Inhalts-Stamm in jenen Szenarien der `UseSolutionRelativeContentRoot` Erweiterungsmethode über die [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) Paket. Geben Sie die Lösung relativen Pfad und Namen oder Glob Dateimuster des optionalen Lösung (Standardeinstellung = `*.sln`).

Rufen Sie die [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) Erweiterung Methode mit *eine* der folgenden Ansätze:

* Beim Konfigurieren von Testklassen mit `WebApplicationFactory`, geben Sie eine benutzerdefinierte Konfiguration mit der [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Beim Konfigurieren von Testklassen mit einer benutzerdefinierten `WebApplicationFactory`, erben von `WebApplicationFactory` und überschreiben [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Deaktivieren Sie die Schattenkopiefunktion

Erstellen von Schattenkopien bewirkt, dass die Tests in einem anderen Ordner als dem Ausgabeordner ausführen. Für die Tests ordnungsgemäß funktioniert muss die Option Schattenkopiefunktion deaktiviert werden. Die [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) xUnit verwendet, und deaktiviert die Schattenkopiefunktion für xUnit dazu eine *xunit.runner.json* Datei mit der Einstellung richtig konfiguriert. Weitere Informationen finden Sie unter [konfigurieren xUnit.net mit JSON](https://xunit.github.io/docs/configuring-with-json.html).

Hinzufügen der *xunit.runner.json* Datei zum Stamm des Testprojekts mit dem folgenden Inhalt:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Tests integrationsbeispiel

Die [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) besteht aus zwei Web-apps:

| App | Projektordner | Beschreibung |
| --- | -------------- | ----------- |
| Nachrichten-app (SUT) | *Src/RazorPagesProject* | Ermöglicht es einem Benutzer hinzufügen, löschen Sie eine, löschen Sie alle und Analysieren von Nachrichten. |
| Test-app | *tests/RazorPagesProject.Tests* | Integrationstest der SUT verwendet. |

Die Tests können ausgeführt werden, z. B. Verwenden von der integrierten Test Funktionen eine IDE [Visual Studio](https://www.visualstudio.com/vs/). Wenn [Visual Studio Code](https://code.visualstudio.com/) oder der Befehlszeile, und führen Sie den folgenden Befehl an einer Eingabeaufforderung in das *tests/RazorPagesProject.Tests* Ordner:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organisation der Nachricht-app (SUT)

Die SUT wird ein Nachrichtensystem, das Razor-Seiten mit den folgenden Merkmalen:

* Die Indexseite der app (*Pages/Index.cshtml* und *Pages/Index.cshtml.cs*) eine Benutzeroberfläche und die Seite stellt Modellmethoden bereit, um zu steuern, die Hinzufügung, löschen und Analyse von Nachrichten (durchschnittliche Wörtern pro Nachricht) .
* Eine Nachricht wird beschrieben, durch die `Message` Klasse (*Data/Message.cs*) mit zwei Eigenschaften: `Id` (Schlüssel) und `Text` (Nachricht). Die `Text` Eigenschaft ist erforderlich, und auf 200 Zeichen beschränkt.
* Nachrichten werden gespeichert mit [Entity Framework des speicherinternen Datenbank](/ef/core/providers/in-memory/)&#8224;.
* Die app enthält einen Datenzugriffsebene (DAL) in seiner Datenbank Context-Klasse `AppDbContext` (*Data/AppDbContext.cs*).
* Wenn die Datenbank auf app-Starts leer ist, wird der Nachrichtenspeicher mit drei Nachrichten initialisiert.
* Die App eine `/SecurePage` , kann nur von einem authentifizierten Benutzer zugegriffen werden.

&#8224;Das Thema EF [Test mit InMemory](/ef/core/miscellaneous/testing/in-memory), wird erläutert, wie eine Datenbank im Arbeitsspeicher für Tests mit MSTest verwendet. In diesem Thema verwendet die [xUnit](https://xunit.github.io/) Testframework. Test-Konzepte und Test-Implementierungen in anderen Testframeworks sind ähnlich, aber nicht identisch.

Auch wenn die app nicht verwendet die [Repositorymusters](http://martinfowler.com/eaaCatalog/repository.html) ist ein Beispiel für effektive nicht die [Arbeitseinheit (UoW) Muster](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor-Seiten unterstützt diese Muster der Entwicklung. Weitere Informationen finden Sie unter [Entwerfen der Infrastruktur Persistenzebene](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementieren das Repository und die Einheit der Arbeit Muster in einer ASP.NET MVC-Anwendung](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), und [Testcontroller Logik](/aspnet/core/mvc/controllers/testing) (im Beispiel wird das Repositorymuster implementiert).

### <a name="test-app-organization"></a>Testen von app-Organisation

Die Test-app ist eine Konsolen-app innerhalb der *tests/RazorPagesProject.Tests* Ordner.

| Test-app-Ordners | Beschreibung |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* Testmethoden für routing, Zugriff auf eine sichere Seite durch ein nicht authentifizierter Benutzer und ein GitHub-Benutzerprofil abrufen und überprüfen das Profil Anmeldung des Benutzers enthält. |
| *IntegrationTests* | *IndexPageTests.cs* enthält die Integrationstests für die Indexseite mit benutzerdefinierten `WebApplicationFactory` Klasse. |
| *Hilfsprogramme-Hilfsprogramme* | <ul><li>*Utilities.cs* enthält die `InitializeDbForTests` Methode verwendet, um die Datenbank mit Testdaten Startwerten.</li><li>*HtmlHelpers.cs* bietet eine Methode zum Zurückgeben einer AngleSharp `IHtmlDocument` für die Verwendung durch die Testmethoden.</li><li>*HttpClientExtensions.cs* bieten Überladungen für `SendAsync` der SUT anfordern.</li></ul> |

Das Testframework ist [xUnit](https://xunit.github.io/). Integrationstests erfolgreich durchgeführt werden, mithilfe der [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), darunter die [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Da die [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) Paket wird verwendet, so konfigurieren Sie die Testserver-Host und Testen der `TestHost` und `TestServer` Pakete erfordern keine direkte paketverweisen in der Test-app-Projektdatei oder Developer Configuration in der Test-app.

**Das Seeding der Datenbank zu Testzwecken**

Integrationstests erfordern i. d. r. einen kleinen Dataset in der Datenbank vor der Ausführung des Tests. Beispielsweise ein Löschvorgang testen Aufrufe für einen Datensatz löschen, daher muss die Datenbank mindestens ein Datensatz für die Delete-Anforderung erfolgreich verarbeitet haben.

Die Datenbank mit drei Nachrichten in die Beispiel-app-Ausgangswerte *Utilities.cs* , dass Tests verwendet werden können, wenn sie ausgeführt:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Komponententests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Komponententests für Razor-Seiten](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Testen von Controllern](xref:mvc/controllers/testing)
