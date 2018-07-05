---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Komponententests für ASP.NET Web API 2 | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Leitfaden und die Anwendung veranschaulichen, wie einfacher Komponententests für Ihre Web-API 2-Anwendung zu erstellen. Dieses Tutorial zeigt, wie eine Unit Test Proj enthalten...
ms.author: aspnetcontent
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 701ced855ff2848182fdbf8d4b9e2bcf0c33341b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806214"
---
<a name="unit-testing-aspnet-web-api-2"></a>Komponententests für ASP.NET Web API 2
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Dieser Leitfaden und die Anwendung veranschaulichen, wie einfacher Komponententests für Ihre Web-API 2-Anwendung zu erstellen. In diesem Tutorial veranschaulicht einen Datenbankkomponententest-Projekt in der Projektmappe, und Schreiben von Testmethoden, die die zurückgegebenen Werte von einer Controllermethode aus zu überprüfen.
> 
> In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.NET Web-API vertraut sind. Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Die Komponententests in diesem Thema sind absichtlich auf einfachen Szenarien beschränkt. Komponententests erweiterte Data-Szenarien finden Sie unter [Mocking Entity Framework bei Komponententests Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web-API 2


## <a name="in-this-topic"></a>In diesem Thema

Dieses Thema enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prereqs)
- [Code herunterladen](#download)
- [Erstellen Sie Anwendung mit Komponententestprojekt](#appwithunittest)

    - [Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu](#whencreate)
    - [Datenbankkomponententest-Projekt zu einer vorhandenen Anwendung hinzufügen](#addtoexisting)
- [Einrichten der Web-API 2-Anwendung](#setupproject)
- [Installieren von NuGet-Pakete im Test-Projekt](#testpackages)
- [Erstellen von tests](#tests)
- [Ausführen von tests](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2017 Community, Professional oder Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Code herunterladen

Herunterladen der [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Herunterladbare Projekt enthält Komponententests Codes für in diesem Thema und die [Mocking Entity Framework beim Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) Thema.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Erstellen Sie Anwendung mit Komponententestprojekt

Sie können entweder ein Komponententestprojekt erstellen, wenn Sie Ihre Anwendung zu erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung. Dieses Tutorial zeigt beide Methoden zum Erstellen eines Komponententestprojekts. Um dieses Tutorial folgen zu können, können Sie beide Ansätze verwenden.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu

Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.

![Projekt erstellen](unit-testing-with-aspnet-web-api/_static/image1.png)

Wählen Sie in den Fenstern neues ASP.NET-Projekt die **leere** Vorlage und fügen Sie Ordner und kernreferenzen für Web-API. Wählen Sie die **Komponententests hinzufügen,** Option. Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**. Sie können diesen Namen beibehalten.

![Komponententestprojekt erstellen](unit-testing-with-aspnet-web-api/_static/image2.png)

Nach dem Erstellen der Anwendung sehen Sie sich, dass sie zwei Projekte enthält.

![zwei Projekte](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Datenbankkomponententest-Projekt zu einer vorhandenen Anwendung hinzufügen

Wenn Sie das Komponententestprojekt nicht bei der Erstellung Ihrer Anwendung erstellt haben, können Sie eine zu einem beliebigen Zeitpunkt hinzufügen. Nehmen wir beispielsweise an, dass Sie bereits über eine Anwendung namens StoreApp verfügen, und Komponententests hinzugefügt werden soll. Klicken Sie zum Hinzufügen eines Komponententestprojekts mit der rechten Maustaste in der Projektmappe, und wählen Sie **hinzufügen** und **neues Projekt**.

![Neues Projekt zu Projektmappe hinzufügen](unit-testing-with-aspnet-web-api/_static/image4.png)

Wählen Sie **Test** im linken Bereich, und wählen **Komponententestprojekt** für den Projekttyp. Nennen Sie das Projekt **StoreApp.Tests**.

![Datenbankkomponententest-Projekt hinzufügen](unit-testing-with-aspnet-web-api/_static/image5.png)

Sie sehen das Komponententestprojekt in Ihrer Lösung.

Fügen Sie in der Datenbankkomponententest-Projekt einen Projektverweis auf das ursprüngliche Projekt ein.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Einrichten der Web-API 2-Anwendung

Fügen Sie in Ihrem StoreApp-Projekt eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**. Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Erstellen Sie die Projektmappe.

Mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie **hinzufügen** und **neues Gerüstelement**. Wählen Sie **Web-API 2-Controller - leer**.

![Hinzufügen eines neuen Controllers](unit-testing-with-aspnet-web-api/_static/image6.png)

Legen Sie den Controllernamen **SimpleProductController**, und klicken Sie auf **hinzufügen**.

![Geben Sie controller](unit-testing-with-aspnet-web-api/_static/image7.png)

Ersetzen Sie den vorhandenen Code durch folgenden Code: Zur Vereinfachung dieses Beispiels werden die Daten in einer Liste anstelle einer Datenbank gespeichert. Die in dieser Klasse definierten Liste stellt die Produktionsdaten dar. Beachten Sie, dass der Controller enthält einen Konstruktor, der als Parameter eine Liste der Product-Objekte akzeptiert. Dieser Konstruktor können Sie Testdaten zu übergeben wenn Komponententests ausgeführt werden. Der Controller enthält auch zwei **Async** Methoden, um asynchrone Methoden für die Komponententests zu veranschaulichen. Diese asynchronen Methoden wurden implementiert, durch den Aufruf **Task.FromResult** Minimieren von überflüssigen Code, aber normalerweise die Methoden würde ressourcenintensive Vorgänge enthalten.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Die GetProduct-Methode gibt eine Instanz des der **IHttpActionResult** Schnittstelle. IHttpActionResult ist eines der neuen Features in Web-API 2, und vereinfacht dadurch das Unit Testentwicklung. Klassen, die die IHttpActionResult-Schnittstelle implementieren finden Sie in der [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) Namespace. Diese Klassen stellen die mögliche Antworten aus einer aktionsanforderung dar, und sie entsprechen den HTTP-Statuscodes.

Erstellen Sie die Projektmappe.

Sie können nun können Sie das Testprojekt einrichten.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installieren von NuGet-Pakete im Test-Projekt

Wenn Sie die leere Vorlage verwenden, um eine Anwendung zu erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete. Weitere Vorlagen, wie z. B. die Web-API-Vorlage, schließen Sie einige NuGet-Pakete in das Komponententestprojekt. Für dieses Tutorial müssen Sie das Microsoft ASP.NET Web API 2-Core-Paket für das Testprojekt einschließen.

Mit der rechten Maustaste in des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**. Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.

![Verwalten von Paketen](unit-testing-with-aspnet-web-api/_static/image8.png)

Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.

![Installieren Sie Web-api-Core-Paket](unit-testing-with-aspnet-web-api/_static/image9.png)

Schließen Sie das Fenster "NuGet-Pakete verwalten".

<a id="tests"></a>
## <a name="create-tests"></a>Tests erstellen

Standardmäßig enthält das Testprojekt eine leeren Test-Datei, die mit dem Namen "UnitTest1.cs". Diese Datei werden die Attribute, dass Sie zum Erstellen der Testmethoden verwenden. Für die Komponententests können Sie diese Datei verwenden oder eine eigene Datei erstellen.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

In diesem Tutorial erstellen Sie eine eigene Testklasse. Sie können die Datei "UnitTest1.cs" löschen. Fügen Sie eine Klasse, die mit dem Namen **TestSimpleProductController.cs**, und Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Tests durchführen

Sie können nun zum Ausführen der Tests. Alle der Methode, die mit markierten Felder der **TestMethod** Attribut getestet. Von der **Test** Menüelement, führen Sie die Tests.

![Tests durchführen](unit-testing-with-aspnet-web-api/_static/image11.png)

Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.

![Testergebnisse](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Zusammenfassung

Sie haben dieses Lernprogramm abgeschlossen. Die Daten in diesem Tutorial wurde absichtlich Schwerpunkte Komponententests Bedingungen vereinfacht. Komponententests erweiterte Data-Szenarien finden Sie unter [Mocking Entity Framework bei Komponententests Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
