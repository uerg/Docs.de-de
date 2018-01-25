---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "Komponententests für ASP.NET Web API 2 | Microsoft Docs"
author: tfitzmac
description: "Diese Anleitung und die Anwendung wird gezeigt, wie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen. In diesem Lernprogramm wird gezeigt, wie eine Einheit Test Proj eingeschlossen wird..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-aspnet-web-api-2"></a>Komponententests für ASP.NET Web API 2
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Diese Anleitung und die Anwendung wird gezeigt, wie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen. In diesem Lernprogramm wird gezeigt, wie ein Komponententestprojekt in der Projektmappe, und Schreiben Testmethoden, mit die die zurückgegebenen Werte aus einem Controllermethode überprüft wird.
> 
> In diesem Lernprogramm wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten der ASP.NET Web-API vertraut sind. Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Die Komponententests in diesem Thema sind einfache Datenszenarien absichtlich beschränkt. Komponententests, die erweiterte Datenszenarien, finden Sie unter [imitieren Entity Framework beim Einheit Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web-API 2


## <a name="in-this-topic"></a>In diesem Thema

Dieses Thema enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prereqs)
- [Herunterladen von code](#download)
- [Anwendung mit Komponententestprojekt erstellen](#appwithunittest)

    - [Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu](#whencreate)
    - [Komponententestprojekt zu einer vorhandenen Anwendung hinzufügen](#addtoexisting)
- [Einrichten der Web-API 2-Anwendung](#setupproject)
- [Installieren von NuGet-Pakete im Test-Projekt](#testpackages)
- [Erstellen von tests](#tests)
- [Ausführen von tests](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2017 Community, Professional oder Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Herunterladen von code

Herunterladen der [abgeschlossenes Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Herunterladbare Projekt umfasst Komponententestcode für dieses Thema und die [imitieren Entity Framework beim Einheit ASP.NET Web API zum Testen der](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) Thema.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Anwendung mit Komponententestprojekt erstellen

Sie können entweder ein Komponententestprojekt erstellen, wenn Ihre Anwendung erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung. Dieses Lernprogramm zeigt beide Methoden zum Erstellen eines Komponententestprojekts. Um dieses Lernprogramm auszuführen, können Sie beide Vorgehensweisen verwenden.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu

Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.

![Projekt erstellen](unit-testing-with-aspnet-web-api/_static/image1.png)

Wählen Sie im Fenster Neues ASP.NET-Projekt den **leere** Vorlage und Hinzufügen von Ordnern und Verweise für die Web-API core. Wählen Sie die **Komponententests hinzufügen** Option. Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**. Sie können diesen Namen beibehalten.

![Komponententestprojekt erstellen](unit-testing-with-aspnet-web-api/_static/image2.png)

Nach dem Erstellen der Anwendung, sehen Sie sich, dass es zwei Projekte enthält.

![zwei Projekte](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Komponententestprojekt zu einer vorhandenen Anwendung hinzufügen

Wenn Sie nicht das Komponententestprojekt erstellt haben, wenn Sie Ihre Anwendung erstellt haben, können Sie eine zu einem beliebigen Zeitpunkt hinzufügen. Nehmen wir beispielsweise an, dass Sie bereits über eine Anwendung mit dem Namen StoreApp verfügen und Komponententests hinzugefügt werden soll. Klicken Sie zum Hinzufügen eines Komponententestprojekts mit der rechten Maustaste in der Projektmappe, und wählen Sie **hinzufügen** und **neues Projekt**.

![Neues Projekt zur Projektmappe hinzugefügt](unit-testing-with-aspnet-web-api/_static/image4.png)

Wählen Sie **Test** im linken Bereich, und wählen **Komponententestprojekt** für den Projekttyp. Nennen Sie das Projekt **StoreApp.Tests**.

![Fügen Sie Komponententestprojekt hinzu](unit-testing-with-aspnet-web-api/_static/image5.png)

Das Komponententestprojekt sehen Sie in der Projektmappe.

Fügen Sie einen Projektverweis auf das ursprüngliche Projekt hinzu, in das Komponententestprojekt.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Einrichten der Web-API 2-Anwendung

Fügen Sie in Ihrem Projekt StoreApp eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**. Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Erstellen Sie die Projektmappe.

Mit der rechten Maustaste in des Ordners Controller, und wählen Sie **hinzufügen** und **neues Gerüstelement**. Wählen Sie **Web-API 2-Controller: leere**.

![Hinzufügen eines neuen Controllers](unit-testing-with-aspnet-web-api/_static/image6.png)

Legen Sie den Namen des Controllers auf **SimpleProductController**, und klicken Sie auf **hinzufügen**.

![Geben Sie die controller](unit-testing-with-aspnet-web-api/_static/image7.png)

Ersetzen Sie den vorhandenen Code durch folgenden Code: Zur Vereinfachung dieses Beispiels werden die Daten in einer Liste anstatt einer Datenbank gespeichert. Die in dieser Klasse definierte Liste stellt die Produktionsdaten dar. Beachten Sie, dass der Controller enthält einen Konstruktor, der als Parameter eine Liste von Product-Objekten akzeptiert. Dieser Konstruktor können Sie Testdaten übergeben bei Komponententests bereit. Der Controller enthält auch zwei **Async** Methoden, um Komponententests für asynchrone Methoden zu veranschaulichen. Diese asynchronen Methoden wurden durch den Aufruf implementiert **Task.FromResult** äußeren Code, aber normalerweise die Methoden minimieren würde ressourcenintensive Vorgänge enthalten.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Gibt die GetProduct-Methode eine Instanz von der **IHttpActionResult** Schnittstelle. IHttpActionResult ist eine der neuen Funktionen in Web-API 2 und Unit Testentwicklung vereinfacht. Klassen, die die IHttpActionResult-Schnittstelle implementieren befinden sich in der [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) Namespace. Diese Klassen stellen mögliche Antworten auf eine Aktion Anfrage, und sie HTTP-Statuscodes entsprechen.

Erstellen Sie die Projektmappe.

Sie können nun können Sie das Testprojekt einrichten.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installieren von NuGet-Pakete im Test-Projekt

Wenn Sie die leere Vorlage verwenden, um eine Anwendung erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete. Andere Vorlagen, z. B. die Web-API-Vorlage enthalten einige NuGet-Pakete in das Komponententestprojekt. Für dieses Lernprogramm müssen Sie das Paket Microsoft ASP.NET Web API 2 Core des Testprojekts einschließen.

Mit der rechten Maustaste des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**. Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.

![Verwalten von Paketen](unit-testing-with-aspnet-web-api/_static/image8.png)

Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.

![Installieren Sie Web api Core-Pakets](unit-testing-with-aspnet-web-api/_static/image9.png)

Schließen Sie das NuGet-Pakete verwalten-Fenster.

<a id="tests"></a>
## <a name="create-tests"></a>Tests erstellen

Standardmäßig enthält das Testprojekt eine leeren Test-Datei, die mit dem Namen "UnitTest1.cs". Diese Datei zeigt die Attribute, dass Sie zum Erstellen der Testmethoden verwenden. Für die Komponententests können Sie diese Datei verwenden oder erstellen eine eigene Datei.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

In diesem Lernprogramm erstellen Sie eine eigene Testklasse. Sie können die Datei "UnitTest1.cs" löschen. Fügen Sie eine Klasse mit dem Namen **TestSimpleProductController.cs**, und Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Tests durchführen

Sie sind jetzt bereit, um die Tests auszuführen. Alle von der Methode, die mit markiert sind die **TestMethod** Attribut wird getestet werden. Aus der **Test** Menüelement klicken, führen Sie die Tests.

![Tests durchführen](unit-testing-with-aspnet-web-api/_static/image11.png)

Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.

![Testergebnisse](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Zusammenfassung

Sie haben dieses Lernprogramm abgeschlossen. Die Daten in diesem Lernprogramm wurde absichtlich legen den Schwerpunkt auf Komponententests Bedingungen vereinfacht. Komponententests, die erweiterte Datenszenarien, finden Sie unter [imitieren Entity Framework beim Einheit Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
