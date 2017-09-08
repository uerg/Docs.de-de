---
title: Integrationstests zu legen, die in ASP.NET Core
author: ardalis
description: "So verwenden Sie ASP.NET Core Integrationstests zu legen, um sicherzustellen, dass eine Anwendung Komponenten ordnungsgemäß funktionieren."
keywords: ASP.NET Core Integrationstests
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 40d534f2-89b3-4b09-9c2c-3494bf9991c9
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: d30af6edc31fbce2ebb77c57be3fd78231c54b50
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2017
---
# <a name="integration-testing-in-aspnet-core"></a>Integrationstests zu legen, die in ASP.NET Core

Durch [Steve Smith](http://ardalis.com)

Integrationstests wird sichergestellt, dass eine Anwendung Komponenten ordnungsgemäß funktionieren, wenn es sich bei zusammen assembliert. ASP.NET Core unterstützt Integrationstests zu legen, mit der Komponententest-Frameworks und einem integrierten Test Webhost, der verwendet werden kann, um Mehraufwand von Netzwerk-Anforderungen zu verarbeiten.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample)

## <a name="introduction-to-integration-testing"></a>Einführung in die Integrationstests

Integrationstests stellen Sie sicher, dass unterschiedliche Teile einer Anwendung ordnungsgemäß zusammenarbeiten. Im Gegensatz zu [UnitTests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), Integrationstests häufig Infrastruktur anwendungsaspekten, z. B. eine Datenbank, Dateisystem, Netzwerkressourcen, oder webanforderungen und-Antworten umfassen. Komponententests Fakes oder Pseudoobjekten anstelle dieser Bedenken, aber der Zweck der Integrationstests wird bestätigt, dass das System arbeitet wie erwartet mit diesen Systemen.

Integrationstests, tendenziell, weil sie größere Codesegmente Übung und sie von Infrastrukturelemente, abhängig sind erheblich langsamer als Komponententests. Daher ist es eine gute Idee, wie viele Integrationstests einzuschränken, dass Sie schreiben, insbesondere dann, wenn Sie das gleiche Verhalten mit einem Komponententest testen können.

> [!NOTE]
> Wenn einige Verhaltensweisen getestet werden kann, verwenden einen Komponententest oder einen Integrationstest lieber den Komponententest, da er sein werden fast immer schneller. Möglicherweise müssen Dutzende oder Hunderte von Komponententests mit vielen unterschiedlichen Eingaben jedoch nur eine Handvoll Integrationstests, die die wichtigsten Szenarien abdecken.

Testen die Logik innerhalb der eigenen Methoden ist in der Regel die Domäne des Komponententests. Testen die Funktionsweise der Anwendungsstatus innerhalb seiner Frameworks, z. B. ASP.NET Core oder mit einer Datenbank ist, in denen Integrationstests sprachbasierte möglich. Es ist nicht zu viele Integrationstests, um sicherzustellen, dass eine Zeile in die Datenbank schreiben und Lesen Sie es wieder können Sie Ihre dauern. Sie müssen nicht alle möglichen Permutation der vom Datenzugriffscode zu testen – müssen Sie nur testen genug, um erhalten Sie mehr Gewissheit, die die Anwendung ordnungsgemäß funktioniert.

## <a name="integration-testing-aspnet-core"></a>Integration testen ASP.NET Core

Um bis zur Integrationstests zu legen zu erhalten, müssen Sie ein Testprojekt erstellen, fügen Sie einen Verweis zum Projekt Web ASP.NET Core und installieren einen Test Runner. Dieser Prozess wird beschrieben, der [UnitTests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) Dokumentation sowie detaillierte Anweisungen zum Ausführen von Tests und Empfehlungen für die Benennung der Tests und Testklassen.

> [!NOTE]
> Trennen Sie die Komponententests und Integrationstests mit anderen Projekten. Dadurch wird sichergestellt, dass Sie versehentlich Infrastruktur Bedenken in Komponententests einführen nicht und können Sie problemlos auswählen, welcher Satz von Tests ausführen.

### <a name="the-test-host"></a>Testhost

ASP.NET Core umfasst einen Testhost, der Integration Testprojekte hinzugefügt werden kann und zum Hosten von verwendeten ASP.NET Core Anwendungen bedient Test fordert, ohne die Notwendigkeit einer echten Webhost. Das bereitgestellte Beispiel enthält ein Integrationstestprojekt die konfiguriert wurde, verwenden Sie [xUnit](https://xunit.github.io) und dem Host zu testen. Er verwendet die `Microsoft.AspNetCore.TestHost` NuGet-Paket.

Einmal die `Microsoft.AspNetCore.TestHost` Paket im Projekt enthalten ist, können zum Erstellen und Konfigurieren einer `TestServer` in den Tests. Der folgende Test zeigt, wie sicherzustellen, dass eine Anforderung auf den Stamm eines Standorts "Hello World!" zurückgibt und sollte ausgeführt wurde erfolgreich gegen die Standardeinstellung ASP.NET Core leere Web-Vorlage, die von Visual Studio erstellt haben.

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Bei diesem Test wird das Anordnen Act Assert-Muster verwendet. Der Diagrammfelder Schritt erfolgt in der Konstruktor, der eine Instanz erstellt `TestServer`. Ein konfiguriertes `WebHostBuilder` verwendet werden, zum Erstellen einer `TestHost`; in diesem Beispiel die `Configure` Methode aus dem System unter dem Test (SUT) `Startup` Klasse wird zum Übergeben der `WebHostBuilder`. Diese Methode wird zum Konfigurieren der Anforderungspipeline, der die `TestServer` identisch mit der Konfiguration des Servers SUT würde.

In der Act-Teil des Tests wird eine Anforderung an die `TestServer` -Instanz für den Pfad "/" und die Antwort wird zurück in eine Zeichenfolge gelesen. Diese Zeichenfolge wird mit der erwarteten Zeichenfolge "Hello World!" verglichen. Wenn sie übereinstimmen, wird der Test erfolgreich; Andernfalls schlägt fehl.

Jetzt können Sie einige zusätzliche Integrationstests, um sicherzustellen, dass es sich bei der es sich um Primzahlen Prüfen der Funktionalität über die Webanwendung funktioniert hinzufügen:

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Beachten Sie, dass Sie nicht wirklich versuchen So testen Sie die Richtigkeit von Primzahl Checker mit diesen Tests jedoch statt Erwartungen die Webanwendung ausführt. Sie haben bereits Coverage für Komponententests, die Ihnen vertrauen, in sofortigen `PrimeService`, wie Sie hier sehen können:

![Test-Explorer](integration-testing/_static/test-explorer.png)

Weitere Informationen finden Sie Informationen zu den Komponententests in der [UnitTests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) Artikel.

## <a name="refactoring-to-use-middleware"></a>Umgestaltung Middleware verwenden

Umgestaltung versteht man das Ändern einer Anwendung Code aus, um den Entwurf zu verbessern, ohne dessen Verhalten zu ändern. Es sollte idealerweise ausgeführt werden, wenn es eine Sammlung ist von Tests, übergeben, da diese Hilfe stellen Sie sicher, dass das System Verhalten vor und nach der Änderung des unverändert. Betrachten die Möglichkeit, die in der die Logik überprüft es sich um Primzahlen wird in der Web-Anwendungsverzeichnis implementiert `Configure` Methode finden Sie unter:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

Dieser Code funktioniert allerdings handelt es sich zurückgelegt wie möchten Sie diese Art von Funktion implementieren, in einer ASP.NET Core-Anwendung auch so einfach, weil es sich handelt. Angenommen, was die `Configure` Methode würde wie folgt aussehen, ggf. viel Code hinzugefügt, jedes Mal, wenn Sie einen anderen URL-Endpunkt hinzufügen.

Eine Option zu berücksichtigen ist das Hinzufügen von [MVC](xref:mvc/overview) an der Anwendung und Erstellen eines Domänencontrollers, auf die wesentlichen Überprüfung behandeln. Allerdings muss die vorausgesetzt, dass Sie derzeit keine anderen MVC Funktionalität, einem bit overkill.

Sie können jedoch von ASP.NET Core profitieren [Middleware](xref:fundamentals/middleware), die hilft uns zu kapseln die Primzahlen Logik in eine eigene Klasse wird überprüft und eine bessere Leistung erzielen [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) in der `Configure` Methode.

Der Pfad darf die Middleware verwendet als Parameter angegeben werden, damit die Middleware-Klasse erwartet werden sollen eine `RequestDelegate` und eine `PrimeCheckerOptions` Instanz in seinem Konstruktor. Wenn der Pfad der Anforderung nicht übereinstimmt, was diese Middleware ist so konfiguriert, dass Sie davon ausgehen dass, Sie einfach die nächste Middleware in der Kette aufrufen und keine weiteren Aktionen. Der Rest des Codes Implementierung, die im war `Configure` ist jetzt der `Invoke` Methode.

> [!NOTE]
> Da die Middleware hängt die `PrimeService` Service, Sie können auch eine Instanz dieses Diensts mit dem Konstruktor anfordern. Das Framework bietet dieser Dienst über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection), sofern es konfiguriert wurde, z. B. in `ConfigureServices`.

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Da diese Middleware fungiert als Endpunkt in der Anforderung Delegatenkette auf, wenn ihr Pfad übereinstimmt, wird es nicht aufgerufen, `_next.Invoke` Wenn diese Middleware verarbeitet die Anforderung.

Mit diesem Middleware eingesetzt sind und einige nützliche Erweiterungsmethoden erstellt, um die Lesbarkeit zu konfigurieren, die umgestalteten `Configure` -Methode sieht folgendermaßen aus:

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Folgende dieses refactoring sind Sie sicher, dass die Webanwendung weiterhin wie zuvor funktioniert, da die Integrationstests erfolgreich alle übergeben werden.

> [!NOTE]
> Es ist eine gute Idee, übertragen Sie die Änderungen zur quellcodeverwaltung, nachdem Sie eine Umgestaltung abschließen und die Tests bestanden. Wenn Sie die testgesteuerte Entwicklung, üben sind [erwägen der Commit für die Red-Green-Refactor Zyklus](http://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Ressourcen

* [Komponententests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Middleware](xref:fundamentals/middleware)
* [Testen von Domänencontrollern](xref:mvc/controllers/testing)
