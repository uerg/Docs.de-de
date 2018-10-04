---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulieren des Entitätsframework bei Komponententests für ASP.NET Web API 2 | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Leitfaden und die Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung zu erstellen, das Entity Framework verwendet. Es veranschaulicht das Ändern der...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8945f913abe8fb8397d07a5994000fff2348f1f7
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795378"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulieren des Entitätsframework bei Komponententests für ASP.NET Web API 2
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

[Abgeschlossenes Projekt herunterladen](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Dieser Leitfaden und die Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung zu erstellen, das Entity Framework verwendet. Vorgehensweise: Ändern Sie den erstellten Controller aus, um die ermöglicht das Übergeben einer Context-Objekt, für das Testen und das Testobjekte zu erstellen, die mit Entity Framework verwendet werden.
>
> Eine Einführung in Komponententests mit ASP.NET Web-API, finden Sie unter [Komponententests mit ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
>
> In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.NET Web-API vertraut sind. Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web-API 2

## <a name="in-this-topic"></a>In diesem Thema

Dieses Thema enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prereqs)
- [Code herunterladen](#download)
- [Erstellen Sie Anwendung mit Komponententestprojekt](#appwithunittest)
- [Erstellen Sie die Model-Klasse](#modelclass)
- [Hinzufügen des Controllers](#controller)
- [Hinzufügen von Abhängigkeitsinjektion](#dependency)
- [Installieren von NuGet-Pakete im Test-Projekt](#testpackages)
- [Test erstellen](#testcontext)
- [Erstellen von tests](#tests)
- [Ausführen von tests](#runtests)

Wenn Sie bereits, dass die Schritte im haben [Komponententests mit ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), können Sie mit Abschnitt fortfahren [Hinzufügen des Controllers](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Vorraussetzungen

Visual Studio 2017 Community, Professional oder Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Code herunterladen

Herunterladen der [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Herunterladbare Projekt enthält Komponententests Codes für in diesem Thema und die [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) Thema.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Erstellen Sie Anwendung mit Komponententestprojekt

Sie können entweder ein Komponententestprojekt erstellen, wenn Sie Ihre Anwendung zu erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung. Dieses Tutorial veranschaulicht das Erstellen eines Komponententestprojekts, beim Erstellen der Anwendung.

Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.

Wählen Sie in den Fenstern neues ASP.NET-Projekt die **leere** Vorlage und fügen Sie Ordner und kernreferenzen für Web-API. Wählen Sie die **Komponententests hinzufügen,** Option. Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**. Sie können diesen Namen beibehalten.

![Komponententestprojekt erstellen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Nach dem Erstellen der Anwendung werden Sie sehen, sie enthält zwei Projekte - **StoreApp** und **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Erstellen Sie die Model-Klasse

Fügen Sie in Ihrem StoreApp-Projekt eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**. Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Erstellen Sie die Projektmappe.

<a id="controller"></a>
## <a name="add-the-controller"></a>Hinzufügen des Controllers

Mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie **hinzufügen** und **neues Gerüstelement**. Wählen Sie die Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework.

![Hinzufügen eines neuen Controllers](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Geben Sie die folgenden Werte an:

- Controllername: **ProductController**
- Modellklasse: **Produkt**
- Datenkontextklasse: [auswählen **neuer Datenkontext** Schaltfläche, die die Werte, siehe Abbildung unten ausfüllt]

![Geben Sie controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Klicken Sie auf **hinzufügen** den Controller mit automatisch generiertem Code zu erstellen. Der Code enthält Methoden zum Erstellen, abrufen, aktualisieren und Löschen von Instanzen der Product-Klasse. Der folgende Code zeigt die Methode für ein Produkt hinzufügen. Beachten Sie, dass die Methode eine Instanz der zurückgibt **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult ist eines der neuen Features in Web-API 2, und vereinfacht dadurch das Unit Testentwicklung.

Im nächsten Abschnitt wird den generierten Code zu vereinfachen angepasst Testobjekte an den Controller übergeben.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Hinzufügen von Abhängigkeitsinjektion

Derzeit ist die ProductController-Klasse eine Instanz der Klasse StoreAppContext verwenden hartcodiert. Sie werden nach dem Schema der Abhängigkeitsinjektion verwenden, ändern Sie die Anwendung, und entfernen diese Abhängigkeit hartcodiert. Diese Abhängigkeit wird aufteilen, können Sie beim Testen in ein mock-Objekt übergeben.

Mit der rechten Maustaste die **Modelle** Ordner, und fügen Sie eine neue Schnittstelle namens **IStoreAppContext**.

Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Öffnen Sie die StoreAppContext.cs-Datei, und stellen Sie die folgenden hervorgehobenen Änderungen vor. Die wichtigen Änderungen zu beachten sind:

- StoreAppContext-Klasse implementiert IStoreAppContext-Schnittstelle
- MarkAsModified-Methode wird implementiert.


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Öffnen Sie die ProductController.cs-Datei. Ändern Sie den vorhandenen Code den hervorgehobenen Code entsprechend. Diese Änderungen die Abhängigkeit auf StoreAppContext unterbrechen und aktivieren Sie andere Klassen in einem anderen Objekt der Context-Klasse übergeben. Diese Änderung können Sie während der Komponententests in einem Testkontext übergeben.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Es gibt eine weitere Änderung, die Sie ProductController vornehmen müssen. In der **PutProduct** -Methode, ersetzen die Zeile, die den Entitätszustand wird auf mit einem Aufruf der Methode MarkAsModified geändert.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Erstellen Sie die Projektmappe.

Sie können nun können Sie das Testprojekt einrichten.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installieren von NuGet-Pakete im Test-Projekt

Wenn Sie die leere Vorlage verwenden, um eine Anwendung zu erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete. Weitere Vorlagen, wie z. B. die Web-API-Vorlage, schließen Sie einige NuGet-Pakete in das Komponententestprojekt. Für dieses Tutorial müssen Sie die Entity Framework-Paket und das Microsoft ASP.NET Web API 2-Core-Paket für das Testprojekt einschließen.

Mit der rechten Maustaste in des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**. Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.

![Verwalten von Paketen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Suchen Sie die Online-Pakete und installieren Sie das EntityFramework-Paket (Version 6.0 oder höher). Wenn es angezeigt wird, dass das EntityFramework-Paket bereits installiert ist, haben Sie möglicherweise das StoreApp Projekt statt dem StoreApp.Tests-Projekt ausgewählt.

![Entity Framework hinzufügen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.

![Installieren Sie Web-api-Core-Paket](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Schließen Sie das Fenster "NuGet-Pakete verwalten".

<a id="testcontext"></a>
## <a name="create-test-context"></a>Test erstellen

Fügen Sie eine Klasse, die mit dem Namen **TestDbSet** für das Testprojekt. Diese Klasse dient als Basisklasse für das Test-DataSet. Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Fügen Sie eine Klasse, die mit dem Namen **TestProductDbSet** für das Testprojekt, die den folgenden Code enthält.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Fügen Sie eine Klasse, die mit dem Namen **TestStoreAppContext** , und Ersetzen Sie den vorhandenen Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Tests erstellen

Das Testprojekt enthält standardmäßig eine leeren Test-Datei mit dem Namen **"UnitTest1.cs"**. Diese Datei werden die Attribute, dass Sie zum Erstellen der Testmethoden verwenden. In diesem Tutorial können Sie diese Datei löschen, da Sie eine neue Testklasse hinzufügen.

Fügen Sie eine Klasse, die mit dem Namen **TestProductController** für das Testprojekt. Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Tests durchführen

Sie können nun zum Ausführen der Tests. Alle der Methode, die mit markierten Felder der **TestMethod** Attribut getestet. Von der **Test** Menüelement, führen Sie die Tests.

![Tests durchführen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.

![Testergebnisse](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
