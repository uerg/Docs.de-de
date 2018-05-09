---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulieren von Entity Framework bei Komponententests für ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Diese Anleitung und die Anwendung wird gezeigt, wie Komponententests für Ihre Web-API 2-Anwendung erstellen, die das Entity Framework verwendet. Es zeigt die Vorgehensweise beim Ändern der...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulieren von Entity Framework bei Komponententests für ASP.NET Web API 2
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Diese Anleitung und die Anwendung wird gezeigt, wie Komponententests für Ihre Web-API 2-Anwendung erstellen, die das Entity Framework verwendet. Es zeigt, wie den scaffolded Controller zum Aktivieren und übergeben ein Kontextobjekt für Tests zu ändern und wie Sie Testobjekte erstellen, die mit Entity Framework verwendet.
> 
> Eine Einführung in Komponententests mit ASP.NET Web-API finden Sie unter [Komponententests mit ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> In diesem Lernprogramm wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten der ASP.NET Web-API vertraut sind. Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
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
- [Erstellen Sie die Modellklasse](#modelclass)
- [Fügen Sie den Controller hinzu](#controller)
- [Abhängigkeitsinjektion hinzufügen](#dependency)
- [Installieren von NuGet-Pakete im Test-Projekt](#testpackages)
- [Erstellen von Testkontext](#testcontext)
- [Erstellen von tests](#tests)
- [Ausführen von tests](#runtests)

Wenn Sie bereits die Schritte in [Komponententests mit ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), können Sie mit dem Abschnitt überspringen [fügen Sie den Controller](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2017 Community, Professional oder Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Herunterladen von code

Herunterladen der [abgeschlossenes Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt zum Herunterladen enthält Komponententestcode für dieses Thema und die [Einheit Testen von ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) Thema.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Anwendung mit Komponententestprojekt erstellen

Sie können entweder ein Komponententestprojekt erstellen, wenn Ihre Anwendung erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung. In diesem Lernprogramm wird ein Komponententestprojekt erstellen, wenn die Anwendung zu erstellen.

Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.

Wählen Sie im Fenster Neues ASP.NET-Projekt den **leere** Vorlage und Hinzufügen von Ordnern und Verweise für die Web-API core. Wählen Sie die **Komponententests hinzufügen** Option. Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**. Sie können diesen Namen beibehalten.

![Komponententestprojekt erstellen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Nach dem Erstellen der Anwendung können Sie sehen sie enthält zwei Projekte - **StoreApp** und **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Erstellen Sie die Modellklasse

Fügen Sie in Ihrem Projekt StoreApp eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**. Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Erstellen Sie die Projektmappe.

<a id="controller"></a>
## <a name="add-the-controller"></a>Fügen Sie den Controller hinzu

Mit der rechten Maustaste in des Ordners Controller, und wählen Sie **hinzufügen** und **neues Gerüstelement**. Wählen Sie die Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework.

![Hinzufügen eines neuen Controllers](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Geben Sie die folgenden Werte an:

- Controllername: **ProductController**
- Modellschemas: **Produkt**
- Datenkontextklasse: [Wählen Sie **neuen Datenkontext** Schaltfläche, die in der unten angezeigten Werte füllt]

![Geben Sie die controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Klicken Sie auf **hinzufügen** auf den Domänencontroller mit automatisch generiertem Code zu erstellen. Der Code enthält Methoden zum Erstellen, abrufen, aktualisieren und Löschen von Instanzen der Product-Klasse. Der folgende Code zeigt die Methode für ein Produkt hinzuzufügen. Beachten Sie, dass die Methode eine Instanz zurückgegeben **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult ist eine der neuen Funktionen in Web-API 2 und Unit Testentwicklung vereinfacht.

Im nächsten Abschnitt, passen Sie den generierten Code zu vereinfachen und Testobjekte an den Controller übergeben.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Abhängigkeitsinjektion hinzufügen

Derzeit ist die Klasse ProductController fest programmiert, dass eine Instanz der Klasse StoreAppContext verwenden. Verwenden Sie ein Muster mit der Bezeichnung Abhängigkeitsinjektion ändern Sie die Anwendung und entfernen die Abhängigkeit hartcodiert. Die Aufhebung dieser Abhängigkeit können Sie beim Testen in einem simulierten-Objekt übergeben.

Mit der rechten Maustaste die **Modelle** Ordner, und fügen Sie eine neue Schnittstelle namens **IStoreAppContext**.

Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Öffnen Sie die Datei StoreAppContext.cs und nehmen Sie die folgenden hervorgehobenen Änderungen vor. Die wichtigen Änderungen zu beachten sind:

- StoreAppContext-Klasse implementiert IStoreAppContext-Schnittstelle
- MarkAsModified-Methode implementiert wird


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Öffnen Sie die ProductController.cs-Datei. Ändern Sie den vorhandenen Code entsprechend den hervorgehobenen Code. Diese Änderungen die Abhängigkeit auf StoreAppContext unterbrechen und Aktivieren von anderen Klassen, die in einem anderen Objekt für das Context-Klasse übergeben. Diese Änderung können Sie während der Komponententests in einem Testkontext übergeben.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Es ist eine weitere Änderung, die Sie in ProductController vornehmen müssen. In der **PutProduct** -Methode, ersetzen Sie die Zeile, die in der Entitätszustand schaltet durch einen Aufruf an die Methode MarkAsModified geändert.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Erstellen Sie die Projektmappe.

Sie können nun können Sie das Testprojekt einrichten.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installieren von NuGet-Pakete im Test-Projekt

Wenn Sie die leere Vorlage verwenden, um eine Anwendung erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete. Andere Vorlagen, z. B. die Web-API-Vorlage enthalten einige NuGet-Pakete in das Komponententestprojekt. Für dieses Lernprogramm müssen Sie die Entity Framework-Paket und das Paket Microsoft ASP.NET Web API 2 Core des Testprojekts einschließen.

Mit der rechten Maustaste des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**. Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.

![Verwalten von Paketen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Suchen Sie von Online-Paketen und installieren Sie das EntityFramework-Paket (Version 6.0 oder höher). Wenn es angezeigt wird, dass das Paket EntityFramework bereits installiert ist, haben Sie möglicherweise das StoreApp Projekt statt auf das Projekt StoreApp.Tests ausgewählt.

![Hinzufügen von Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.

![Installieren Sie Web api Core-Pakets](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Schließen Sie das NuGet-Pakete verwalten-Fenster.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Erstellen von Testkontext

Fügen Sie eine Klasse mit dem Namen **TestDbSet** dem Testprojekt. Diese Klasse dient als Basisklasse für das Test-DataSet. Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Fügen Sie eine Klasse mit dem Namen **TestProductDbSet** auf das Testprojekt, das den folgenden Code enthält.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Fügen Sie eine Klasse mit dem Namen **TestStoreAppContext** und Ersetzen Sie den vorhandenen Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Tests erstellen

Standardmäßig enthält das Testprojekt eine leeren Test-Datei mit dem Namen **"UnitTest1.cs"**. Diese Datei zeigt die Attribute, dass Sie zum Erstellen der Testmethoden verwenden. Für dieses Lernprogramm können Sie diese Datei löschen, da Sie eine neue Testklasse hinzugefügt werden.

Fügen Sie eine Klasse mit dem Namen **TestProductController** dem Testprojekt. Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Tests durchführen

Sie sind jetzt bereit, um die Tests auszuführen. Alle von der Methode, die mit markiert sind die **TestMethod** Attribut wird getestet werden. Aus der **Test** Menüelement klicken, führen Sie die Tests.

![Tests durchführen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.

![Testergebnisse](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
