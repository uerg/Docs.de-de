---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Erstellen eine OData v3-Endpunkts mit Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, die Daten Abfragen und Bearbeiten der Daten...
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 67a41f37a09d089336dc96d393f929e56661c4f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813705"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Erstellen eine OData v3-Endpunkts mit Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Die [Open Data Protocol](http://www.odata.org/) (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, die Daten Abfragen und bearbeiten das DataSet über CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen). OData unterstützt AtomPub (XML) und JSON-Format. OData definiert auch eine Möglichkeit, Metadaten zu den Daten verfügbar zu machen. Clients können die Metadaten verwenden, um die Typinformationen und die Beziehungen für das Dataset zu ermitteln.
> 
> ASP.NET Web-API erleichtert es, um einen OData-Endpunkt für ein Dataset zu erstellen. Sie können steuern, auf genau die OData-Vorgänge der Endpunkt unterstützt. Sie können mehrere OData-Endpunkte zusammen mit nicht-OData-Endpunkte hosten. Sie haben vollständige Kontrolle über Ihr Datenmodell, die Back-End-Geschäftslogik und die Datenschicht.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web-API 2
> - OData v3
> - Entity Framework 6
> - [Fiddler Web Debugging Proxy (Optional)](http://www.fiddler2.com)
> 
> Web-API-OData-Unterstützung wurde hinzugefügt, [ASP.NET und Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). In diesem Lernprogramm verwendet jedoch Gerüst, das in Visual Studio 2013 hinzugefügt wurde.


In diesem Tutorial erstellen Sie einen einfachen OData-Endpunkt, den von Clients abgefragt werden können. Außerdem erstellen Sie einen C#-Client für den Endpunkt. Nachdem Sie dieses Tutorial abgeschlossen haben, der nächste Satz von Tutorials zeigen, wie Sie weitere Funktionen, einschließlich von entitätsbeziehungen, Aktionen hinzufügen, und wählen Sie die $erweitern / $.

- [Visual Studio-Projekt erstellen](#create-project)
- [Fügen Sie ein Entitätsmodell hinzu.](#add-model)
- [Fügen Sie einen OData-Controller hinzu.](#add-controller)
- [Hinzufügen des EDM und Weiterleiten](#edm)
- [Seeding für die Datenbank (Optional)](#seed-db)
- [Untersuchen den OData-Endpunkt](#explore)
- [OData-Serialisierungsformate](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

In diesem Tutorial erstellen Sie einen OData-Endpunkt, der grundlegende CRUD-Vorgänge unterstützt. Der Endpunkt wird eine einzelne Ressource, eine Liste der Produkte verfügbar machen. Späteren Tutorials werden der Funktionsumfang erweitert.

Starten Sie Visual Studio, und wählen Sie **neues Projekt** von der Startseite. Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie den Visual C#-Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie **der ASP.NET-Webanwendung** Vorlage.

![](creating-an-odata-endpoint/_static/image1.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage. Klicken Sie unter &quot;fügen Sie Ordner und kernreferenzen für... &quot;, überprüfen Sie **Web-API-**. Klicken Sie auf **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Fügen Sie ein Entitätsmodell hinzu.

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. Für dieses Tutorial benötigen wir ein Modell, das ein Produkt darstellt. Das Modell entspricht unserer OData-Entitätstyp.

Klicken Sie im Projektmappen-Explorer den Ordner "Models". Wählen Sie im Kontextmenü des **hinzufügen** wählen Sie dann **Klasse**.

![](creating-an-odata-endpoint/_static/image3.png)

In der **Add New** Element Dialogfeld, nennen Sie die Klasse &quot;Produkt&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Gemäß der Konvention werden ViewModel-Klassen im Ordner "Models" abgelegt. Sie müssen keine dieser Konvention in Ihren eigenen Projekten folgen, aber wir verwenden sie für dieses Tutorial.


Fügen Sie in der Datei "Product.cs" die folgende Klassendefinition hinzu:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Die ID-Eigenschaft werden die Entitätsschlüssel. Clients können die Produkte nach ID Abfragen. Dieses Feld wird auch der Primärschlüssel in der Back-End-Datenbank sein.

Erstellen Sie jetzt das Projekt. Im nächsten Schritt verwenden wir einige Visual Studio-Gerüst, die Reflektion verwendet, um den Typ des Produkts zu suchen.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Fügen Sie einen OData-Controller hinzu.

Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Sie definieren einen separaten Controller für jede Entität, die Sie OData-Dienst festgelegt. In diesem Tutorial erstellen wir einen einzelnen Controller.

Klicken Sie im Projektmappen-Explorer den Ordner "Controllers". Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.

![](creating-an-odata-endpoint/_static/image5.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld &quot;Web API 2 OData-Controller mit Aktionen unter Verwendung von Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

In der **Controller hinzufügen** Dialogfeld benennen Sie den Controller "ProductsController". Wählen Sie die &quot;Async-Controlleraktionen verwenden&quot; Kontrollkästchen. In der **Modell** Dropdown-Liste, wählen Sie die Product-Klasse.

![](creating-an-odata-endpoint/_static/image7.png)

Klicken Sie auf die **neuer Datenkontext...**  Schaltfläche. Übernehmen Sie den Standardnamen für den Datentyp für den Kontext, und klicken Sie auf **hinzufügen**.

![](creating-an-odata-endpoint/_static/image8.png)

Klicken Sie auf "hinzufügen", in das Dialogfeld "Controller hinzufügen", um den Controller hinzuzufügen.

![](creating-an-odata-endpoint/_static/image9.png)

Hinweis: Wenn Sie eine Fehlermeldung erhalten, auf welchem &quot;gab es Fehler beim Abrufen des Typs... &quot;, stellen Sie sicher, dass Sie Visual Studio-Projekt erstellt, nachdem Sie die Product-Klasse hinzugefügt. Das Gerüst verwendet Reflektion, um die Klasse zu finden.

![](creating-an-odata-endpoint/_static/image10.png)

Das Gerüst wird dem Projekt zwei Codedateien hinzugefügt:

- Products.cs definiert den Web-API-Controller, der den OData-Endpunkt implementiert.
- ProductServiceContext.cs bietet Methoden zum Abfragen der zugrunde liegenden Datenbank mithilfe von Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Hinzufügen des EDM und Weiterleiten

Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs. Diese Klasse enthält den Konfigurationscode für Web-API. Ersetzen Sie diesen Code durch Folgendes:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Dieser Code bewirkt zwei Dinge:

- Erstellt ein Entity Data Model (EDM) für den OData-Endpunkt.
- Fügt eine Route für den Endpunkt hinzu.

Ein EDM ist ein abstraktes Modell der Daten. Das EDM wird verwendet, um das Metadatendokument zu erstellen und definieren Sie die URIs für den Dienst. Die **ODataConventionModelBuilder** ein EDM mithilfe eines Satzes von Standard-Benennungskonventionen EDM erstellt. Dieser Ansatz erfordert den geringsten Code. Wenn Sie mehr Kontrolle über das EDM möchten, können Sie die **ODataModelBuilder** Klasse, um die EDM zu erstellen, durch das Hinzufügen von Eigenschaften, Schlüssel und Navigationseigenschaften explizit.

Die **EntitySet** Methode fügt eine Entität, auf das EDM festgelegt:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Die Zeichenfolge "Produkte" definiert den Namen der Entitätenmenge. Der Name des Controllers muss den Namen der Entitätenmenge überein. In diesem Tutorial die Entitätenmenge ist mit dem Namen "Products" und den Namen des Controllers `ProductsController`. Wenn Sie den Namen der Entitätenmenge "ProductSet", nennen Sie den Controller `ProductSetController`. Beachten Sie, dass ein Endpunkt mehrere Entitätenmengen kann. Rufen Sie **EntitySet&lt;T&gt;**  für jede Entität festlegen und anschließend definieren Sie einen entsprechenden Controller.

Die **MapODataRoute** Methode fügt eine Route für den OData-Endpunkt hinzu.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Der erste Parameter ist ein Anzeigename für die Route. Dieser Name wird von Clients des Dienstes nicht angezeigt. Der zweite Parameter ist der URI-Präfix für den Endpunkt. Wenn diesen Code, der URI für die Produkte Entitätenmenge lautet http://<em>Hostname</em>  /Odata/Produkte. Ihre Anwendung kann mehr als ein OData-Endpunkt haben. Rufen Sie für jeden Endpunkt <strong>MapODataRoute</strong> und geben Sie einen eindeutigen Routennamen und einem eindeutigen URI-Präfix.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Seeding für die Datenbank (Optional)

In diesem Schritt verwenden Sie Entity Framework, das Seeding der Datenbank mit einigen Testdaten. Dieser Schritt ist optional, jedoch können Sie Ihre OData-Endpunkt direkt zu testen.

Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Dadurch wird einen Ordner namens "Migrations" und eine Codedatei Configuration.cs Namens hinzugefügt.

![](creating-an-odata-endpoint/_static/image12.png)

Öffnen Sie diese Datei, und fügen Sie den folgenden Code der `Configuration.Seed` Methode.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Diese Befehle generieren Code, der die Datenbank wird erstellt und führt dann diesen Code.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Untersuchen den OData-Endpunkt

In diesem Abschnitt verwenden wir die [Fiddler Web Debugging Proxy](http://www.fiddler2.com) zum Senden von Anforderungen an den Endpunkt, und überprüfen Sie die Antwortnachrichten. Dies hilft Ihnen, das die Funktionen eines OData-Endpunkts zu verstehen.

Drücken Sie in Visual Studio F5, um mit dem Debuggen beginnen. Standardmäßig wird Visual Studio im Browser geöffnet, mit denen `http://localhost:*port*`, wobei *Port* ist die Portnummer, die in den projekteinstellungen konfiguriert.

Sie können die Portnummer in den projekteinstellungen ändern. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**. Wählen Sie im Eigenschaftenfenster **Web**. Geben Sie die Portnummer unter **Projekt-Url**.

### <a name="service-document"></a>-Dienstdokument

Die *dienstdokument* enthält eine Liste von Entitätenmengen für den OData-Endpunkt. Um das dienstdokument zu erhalten, senden Sie eine GET-Anforderung an die Stamm-URI des Diensts ein.

Mithilfe von Fiddler, geben Sie in den folgenden URI in der **Composer** Registerkarte: `http://localhost:port/odata/`, wobei *Port* gibt die Portnummer an.

![](creating-an-odata-endpoint/_static/image13.png)

Klicken Sie auf die **Execute** Schaltfläche. Fiddler sendet eine HTTP GET-Anforderung an Ihre Anwendung. Daraufhin sollte die Antwort in der Liste der Web-Sitzungen. Wenn alles funktioniert, wird der Statuscode 200 lauten.

![](creating-an-odata-endpoint/_static/image14.png)

Doppelklicken Sie auf die Antwort in der Liste Websitzungen, um die Details der Antwort auf die Registerkarte "Inspektoren" anzuzeigen.

![](creating-an-odata-endpoint/_static/image15.png)

Unformatierte HTTP-Antwortnachricht sollte etwa wie folgt aussehen:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Standardmäßig gibt das dienstdokument von Web-API im AtomPub-Format zurück. Um JSON-Anforderung, fügen Sie die folgenden Header zur HTTP-Anforderung:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Die HTTP-Antwort enthält jetzt eine JSON-Nutzlast:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dienstmetadatendokument

Die *dienstmetadatendokument* beschreibt das Datenmodell des Diensts mithilfe einer XML-Sprache, die der konzeptionellen Schemadefinitionssprache (CSDL) bezeichnet. Das Dokument zeigt die Struktur der Daten im Dienst, und Sie können zum Generieren von Clientcode verwendet werden.

Um das Metadatendokument zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/$metadata`. Hier ist die Metadaten für den Endpunkt, der in diesem Tutorial gezeigt.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Entitätenmenge

Um die Entitätenmenge für die Produkte zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entität

Um ein einzelnes Produkt zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products(1)`, wobei "1" die Produkt-ID

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData-Serialisierungsformate

OData unterstützt mehrere Serialisierungsformate:

- Atom pub-Protokolls (XML)
- JSON "Light" (eingeführt in OData v3)
- JSON "verbose" (OData v2)

Standardmäßig verwendet die Web-API-AtomPubJSON "Light" Format. 

Um AtomPub-Format zu erhalten, wird den Accept-Header auf "Application/Atom + Xml" fest. Hier ist ein Beispiel-Antworttext:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Sehen Sie ein offensichtlicher Nachteil des Atom-Format: Es ist viel ausführlicher als die JSON-Light. Wenn Sie einen Client, der AtomPub versteht verfügen, kann der Client dieses Format über JSON bevorzugen es jedoch.

So sieht die JSON-light-Version der gleichen Entität aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Die JSON-light-Format wurde in Version 3 des OData-Protokolls eingeführt. Um Abwärtskompatibilität zu gewährleisten kann ein Client das ältere "verbose" JSON-Format anfordern. Um ausführlichen JSON-Format anzufordern, legen Sie den Accept-Header auf `application/json;odata=verbose`. Hier ist die ausführliche Version aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Dieses Format gewährt mehr Metadaten in den Antworttext, der beträchtlichen Aufwand über eine gesamte Sitzung hinzufügen kann. Darüber hinaus fügt eine Dereferenzierungsebene hinzu, durch Umschließen des Objekts in einer Eigenschaft mit dem Namen "d".

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von Entitätsbeziehungen](working-with-entity-relations.md)
- [Hinzufügen von OData-Aktionen](odata-actions.md)
- [Rufen Sie den OData-Dienst aus einem .NET-Client](calling-an-odata-service-from-a-net-client.md)
