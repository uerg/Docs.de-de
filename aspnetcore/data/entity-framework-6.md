---
title: Erste Schritte mit ASP.NET Core und Entity Framework 6
author: tdykstra
description: In diesem Artikel wird die Verwendung von Entity Framework 6 in einer ASP.NET Core-Anwendung veranschaulicht.
keywords: Entity Framework, ASP.NET Core EF 6
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: e186568e27c067e29985b8a286e26b87c3186ac4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Erste Schritte mit ASP.NET Core und Entity Framework 6

Durch [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), und [Tom Dykstra](https://github.com/tdykstra)

In diesem Artikel wird die Verwendung von Entity Framework 6 in einer ASP.NET Core-Anwendung veranschaulicht.

## <a name="overview"></a>Übersicht

Um Entity Framework 6 verwenden zu können, muss das Projekt für .NET Framework kompilieren, da Entity Framework 6 .NET Core nicht unterstützt wird. Wenn Sie plattformübergreifende Funktionen benötigen, müssen so aktualisieren Sie auf [Entity Framework Core](https://docs.efproject.net).

Die empfohlene Methode zum Verwenden von Entity Framework 6 in einer ASP.NET Core-Anwendung ist der Kontext EF6 versetzt, und Modellklassen in einer Klassenbibliothek-Projekt, dessen Ziel vollständige Framework. Fügen Sie einen Verweis auf die Klassenbibliothek aus dem ASP.NET Core-Projekt hinzu. Finden Sie im Beispiel [Visual Studio-Projektmappe mit Projekten von EF6 und ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Einen EF6 Kontext kann nicht in einem Projekt auf ASP.NET Core gesetzt werden, da Projekte von .NET Core alle Funktionen nicht unterstützen, die z. B. EF6 Befehle *Enable-Migrations* erfordern.

Unabhängig von Projekttyp, in dem Sie den Kontext EF6 suchen, können nur EF6-Befehlszeilentools mit einem EF6 Kontext aus. Beispielsweise `Scaffold-DbContext` steht nur in Entity Framework Core. Wenn Sie einer Datenbank in einem Modell EF6 zurückentwickeln müssen, finden Sie unter [Code First mit einer vorhandenen Datenbank](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Verweis vollständigen Framework und EF6 im Projekt ASP.NET Core

Das ASP.NET Core-Projekt muss .NET Framework und EF6 verweisen. Z. B. die *csproj* -Datei des Projekts zu ASP.NET Core wird ähnlich wie im folgenden Beispiel aussehen (nur die relevanten Teile der Datei werden angezeigt).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Wenn Sie ein neues Projekt erstellen, verwenden Sie die **ASP.NET-Webanwendung für Core ((.NET Framework)** Vorlage.

## <a name="handle-connection-strings"></a>Behandeln von Verbindungszeichenfolgen

Die EF6-Befehlszeilentools, die Sie in das Klassenbibliotheksprojekt EF6 verwenden erfordern einen Standardkonstruktor damit Kontext instanziiert werden können. Aber Sie möchten wahrscheinlich angeben, dass die Verbindungszeichenfolge im ASP.NET Core-Projekt in diesem Fall die Kontext-Konstruktor verwendet einen Parameter benötigen, der Sie in der Verbindungszeichenfolge übergeben können. Hier ist ein Beispiel.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Da EF6 Kontext nicht über einen parameterlosen Konstruktor verfügt, wird Ihre EF6-Projekt verfügt über eine Implementierung bereitstellen [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Die Befehlszeilentools EF6 findet und den Kontext instanziiert werden können, dass diese Implementierung einsetzen. Hier ist ein Beispiel.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

In diesem Beispielcode die `IDbContextFactory` Implementierung werden in eine hartcodierte Verbindungszeichenfolge übergeben. Dies ist die Verbindungszeichenfolge, die die Befehlszeilentools verwenden. Sie sollten eine Strategie, um sicherzustellen, dass die Klassenbibliothek dieselbe Verbindungszeichenfolge verwendet, die die aufrufende Anwendung verwendet. Beispielsweise konnte den Wert von einer Umgebungsvariablen in beiden Projekten abgerufen werden.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Abhängigkeitsinjektion in ASP.NET Core Projekt einrichten

Die Core-Projekts *Startup.cs* Datei, richten Sie den Kontext EF6 für abhängigkeiteneinschleusung (DI) in `ConfigureServices`. EF Kontextobjekte sollte für eine pro Anforderung Lebensdauer begrenzt werden.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Eine Instanz des Kontexts erhalten in Ihren Domänencontrollern von mithilfe der Abhängigkeitsinjektion. Der Code ist ähnlich, was Sie für einen Kontext EF Core schreiben würden:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Beispielanwendung

Eine funktionierende beispielanwendung finden Sie unter der [Beispiel Visual Studio-Projektmappe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) , die in diesem Artikel begleitet.

In diesem Beispiel kann durch die folgenden Schritte in Visual Studio von Grund auf neu erstellt werden:

* Erstellen Sie eine Projektmappe an.

* **Hinzufügen eines neuen Projekts > Web > ASP.NET Core-Webanwendung ((.NET Framework)**

* **Hinzufügen eines neuen Projekts > klassische Windows-Desktop >-Klassenbibliothek ((.NET Framework)**

* In **Package Manager Console** (PMC) für beide Projekte, führen Sie den Befehl `Install-Package Entityframework`.

* Erstellen Sie in das Klassenbibliotheksprojekt Modell Datenklassen und Context-Klasse und eine Implementierung von `IDbContextFactory`.

* Führen Sie die Befehle in PMC für das Klassenbibliotheksprojekt, `Enable-Migrations` und `Add-Migration Initial`. Wenn Sie das ASP.NET Core-Projekt als Startprojekt festgelegt haben, fügen Sie `-StartupProjectName EF6` auf diese Befehle.

* Fügen Sie in der Core-Projekt einen Projektverweis auf das Klassenbibliotheksprojekt hinzu.

* Im Kern-Projekt in *Startup.cs*, registrieren Sie den Kontext für DI.

* Im Kern-Projekt in *appsettings.json*, fügen Sie der Verbindungszeichenfolge angegeben.

* Fügen Sie das Projekt Core einem Controller und Ansichten, um sicherzustellen, dass Sie lesen und Schreiben von Daten können. (Beachten Sie, dass ASP.NET Core MVC-Gerüstbau nicht mit dem EF6-Kontext, die von der Klassenbibliothek verwiesen funktioniert.)

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat grundlegende Leitfäden zum Verwenden von Entity Framework 6 in einer ASP.NET Core-Anwendung bereitgestellt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Entity Framework - Code-basierte Konfiguration](https://msdn.microsoft.com/data/jj680699.aspx)
