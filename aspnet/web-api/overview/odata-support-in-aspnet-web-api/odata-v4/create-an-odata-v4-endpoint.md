---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Erstellen Sie einen OData v4-Endpunkt mithilfe von ASP.NET Web-API 2.2 | Microsoft Docs
author: MikeWasson
description: Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Methode zur Abfragen und Bearbeiten von Datasets über CRUD-Vorgänge...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508049"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Erstellen Sie einen OData v4-Endpunkt mithilfe von ASP.NET Web-API 2.2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datasets über CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen).
> 
> ASP.NET Web-API unterstützt v3 und v4 des Protokolls. Sie können auch einen v4-Endpunkt, der Seite-an-Seite ausgeführt wird mit einem v3-Endpunkt.
> 
> In diesem Lernprogramm wird gezeigt, wie auf einen OData v4-Endpunkt zu erstellen, der CRUD-Vorgänge unterstützt werden.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web-API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> Die OData-Version 3, finden Sie unter [Erstellen eines OData-v3-Endpunkts](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

In Visual Studio aus der **Datei** klicken Sie im Menü **neu** &gt; **Projekt**.

Erweitern Sie **installiert** &gt; **Vorlagen** &gt; **Visual C#-** &gt; **Web**, und wählen Sie die  **ASP.NET Web Application** Vorlage. Nennen Sie das Projekt &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

In der **neues Projekt** wählen Sie im Dialogfeld die **leere** Vorlage. Unter &quot;Hinzufügen von Ordnern und core Verweise... &quot;, klicken Sie auf **Web-API**. Klicken Sie auf **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Installieren Sie die OData-Pakete

Aus der **Tools** klicken Sie im Menü **NuGet Package Manager** &gt; **Package Manager Console**. Geben Sie die im Paket-Manager-Konsole:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Mit diesem Befehl wird der aktuelle OData-NuGet-Pakete installiert.

## <a name="add-a-model-class"></a>Fügen Sie eine Modellklasse hinzu

Ein *Modell* ist ein Objekt, das eine Datenentität in der Anwendung darstellt.

Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models. Wählen Sie im Kontextmenü der **hinzufügen** &gt; **Klasse**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Gemäß der Konvention werden Modellklassen befinden sich im Ordner Models, aber Sie müssen dieser Konvention in Ihren eigenen Projekten.


Nennen Sie die Klasse `Product`. Ersetzen Sie in der Datei Product.cs den Standardcode durch Folgendes:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

Die `Id` Eigenschaft ist für den Entitätsschlüssel. Clients können Entitäten nach Schlüssel Abfragen. Um das Produkt mit der ID 5 zu erhalten, ist der URI z. B. `/Products(5)`. Die `Id` Eigenschaft werden auch der Primärschlüssel in der Back-End-Datenbank.

## <a name="enable-entity-framework"></a>Aktivieren von Entity Framework

In diesem Lernprogramm wird Entity Framework (EF) Code First verwendet, um die Back-End-Datenbank zu erstellen.

> [!NOTE]
> Web API OData erfordert keine EF. Verwenden Sie Datenzugriffsschicht, die in Modellen Datenbankentitäten übersetzen kann.


Installieren Sie zunächst das NuGet-Paket für EF. Aus der **Tools** klicken Sie im Menü **NuGet Package Manager** &gt; **Package Manager Console**. Geben Sie die im Paket-Manager-Konsole:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Öffnen Sie die Datei "Web.config", und fügen Sie folgenden Abschnitt in der **Konfiguration** Element nach dem **"configSections"** Element.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Diese Einstellung wird eine Verbindungszeichenfolge für eine LocalDB-Datenbank hinzugefügt. Wenn Sie die app lokal ausführen, wird diese Datenbank verwendet werden.

Als Nächstes fügen Sie eine Klasse, die mit dem Namen `ProductsContext` Ordner Models:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Im Konstruktor `"name=ProductsContext"` gibt den Namen der Verbindungszeichenfolge.

## <a name="configure-the-odata-endpoint"></a>Konfigurieren Sie den OData-Endpunkt

Öffnen Sie die Datei App\_Start/WebApiConfig.cs. Fügen Sie die folgenden **mit** Anweisungen:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Fügen Sie folgenden Code, der **registrieren** Methode:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Dieser Code führt zwei Aufgaben aus:

- Erstellt ein Entity Data Model (EDM).
- Fügt eine Route hinzu.

Ein EDM ist eine abstrakte Modell der Daten. Das EDM wird verwendet, um das dienstmetadatendokument zu erstellen. Die **ODataConventionModelBuilder** Klasse erstellt ein EDM mit Standard-Benennungskonventionen. Dieser Ansatz erfordert den geringsten Code. Wenn Sie mehr Kontrolle über das EDM soll, können Sie mithilfe der **ODataModelBuilder** Klasse EDM zu erstellen, indem Sie die Eigenschaften, Schlüssel und Navigationseigenschaften explizit hinzufügen.

Ein *Route* teilt mit Web-API, wie HTTP-Anforderungen an den Endpunkt weitergeleitet. Rufen Sie zum Erstellen einer OData v4-Route dem **MapODataServiceRoute** Erweiterungsmethode.

Wenn die Anwendung mehrere OData-Endpunkte verfügt, erstellen Sie eine separate Route für jeden. Weisen Sie jede Route, einen eindeutigen Routennamen und ein Präfix.

## <a name="add-the-odata-controller"></a>Fügen Sie den OData-Controller hinzu

Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Sie erstellen einen separaten Controller für jede Entität im OData-Dienst festlegen. In diesem Lernprogramm erstellen Sie einen Controller für die `Product` Entität.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Ordners Controller, und wählen **hinzufügen** &gt; **Klasse**. Nennen Sie die Klasse `ProductsController`.

> [!NOTE]
> Die Version des Lernprogramms für OData v3 verwendet die **Controller hinzufügen** Gerüstbau. Derzeit ist kein Gerüst für OData v4.


Ersetzen Sie den Standardcode in ProductsController.cs durch Folgendes.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Der Controller verwendet die `ProductsContext` Klasse Zugriff auf die Datenbank mithilfe von EF. Beachten Sie, die der Controller überschreibt die **Dispose** Methode zum Freigeben der **ProductsContext**.

Dies ist der Ausgangspunkt für den Controller. Fügen Sie anschließend die Methoden für alle CRUD-Vorgänge.

## <a name="querying-the-entity-set"></a>Die Entitätenmenge Abfragen

Fügen Sie die folgenden Methoden hinzu `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Die parameterlose Version der `Get` Methodenrückgabe die gesamte Sammlung von Produkten. Die `Get` Methode mit einem *Schlüssel* Parameter sucht nach einem Produkt anhand ihres Schlüssels (in diesem Fall die `Id` Eigenschaft).

Die **[EnableQuery]** -Attributs können Clients die Abfrage mithilfe von Abfrageoptionen wie $filter, $sort und $page ändern. Weitere Informationen finden Sie unter [OData-Abfrageoptionen unterstützen](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Hinzufügen einer Entität an die Entitätssammlung

Fügen Sie die folgende Methode hinzu, um Clients zum Hinzufügen eines neuen Produkts zur Datenbank aktivieren `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Aktualisieren einer Entität

OData unterstützt zwei unterschiedliche Semantiken zum Aktualisieren einer Entität, Patch- und PUT an.

- Patch für führt ein teilweises Update. Der Client gibt nur die Eigenschaften zu aktualisieren.
- PUT ersetzt die gesamte Entität.

Der Nachteil von PUT ist, dass der Client gesendet werden muss Werte für alle Eigenschaften in der Entität, einschließlich der Werte, die nicht geändert werden. Die [OData-Spezifikation](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) gibt an, dass die PATCHDATEI bevorzugt wird.

In jedem Fall sieht der Code für Patch- und PUT-Methoden:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Im Fall von PATCH, der Controller verwendet die **Delta&lt;T&gt;**  Typ, um die Änderungen nachzuverfolgen.

## <a name="deleting-an-entity"></a>Löschen einer Entität

Fügen Sie die folgende Methode hinzu, um Clients So löschen Sie ein Produkt aus der Datenbank aktivieren `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
