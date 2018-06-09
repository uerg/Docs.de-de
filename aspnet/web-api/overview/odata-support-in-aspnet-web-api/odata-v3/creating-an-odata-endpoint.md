---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Erstellen einen OData-v3-Endpunkt mit Web-API 2 | Microsoft Docs
author: MikeWasson
description: Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Methode zur Strukturdaten Datenabfragen durchzuführen, und die Daten bearbeiten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874628"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Erstellen einen OData-v3-Endpunkt mit Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Die [Open Data Protocol](http://www.odata.org/) (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, die Daten Abfragen und bearbeiten das DataSet über CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen). OData unterstützt AtomPub (XML) und JSON-Format. OData definiert auch eine Möglichkeit, die Metadaten zu den Daten verfügbar zu machen. Clients können die Metadaten verwenden, um die Typinformationen und Beziehungen für das Dataset zu ermitteln.
> 
> ASP.NET Web-API vereinfacht die einen OData-Endpunkt für ein Dataset zu erstellen. Sie können steuern, genau dem OData-Vorgänge der Endpunkt unterstützt. Sie können mehrere OData-Endpunkte, zusammen mit nicht-OData-Endpunkte hosten. Sie haben die vollständige Kontrolle über Ihr Datenmodell, Back-End-Geschäftslogik und Datenschicht.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web-API 2
> - OData-Version 3
> - Entity Framework 6
> - [Fiddler Web Debugging-Proxy (Optional)](http://www.fiddler2.com)
> 
> Web API OData-Unterstützung wurde hinzugefügt, [ASP.NET und Web-Toolsupdate 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Dieses Lernprogramm verwendet jedoch Gerüstbau, die in Visual Studio 2013 hinzugefügt wurde.


In diesem Lernprogramm erstellen Sie einen einfache OData-Endpunkt, den Clients abgefragt werden können. Außerdem erstellen Sie einen C#-Client für den Endpunkt. Nach Abschluss dieses Lernprogramms wird der nächste Satz von Lernprogrammen wird gezeigt, wie weitere Funktionen, einschließlich entitätsbeziehungen, Aktionen, hinzufügen, und wählen Sie die $erweitern / $.

- [Visual Studio-Projekt erstellen](#create-project)
- [Ein Entitätsmodell hinzufügen](#add-model)
- [Fügen Sie einem OData-Controller hinzu](#add-controller)
- [Hinzufügen des EDM und eine Route](#edm)
- [Ausgangswert für die Datenbank (Optional)](#seed-db)
- [Untersuchen den OData-Endpunkt](#explore)
- [OData-Serialisierungsformate](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

In diesem Lernprogramm erstellen Sie einen OData-Endpunkt, der grundlegende CRUD-Vorgänge unterstützt. Der Endpunkt wird eine einzelne Ressource, eine Liste der Produkte verfügbar machen. Spätere Lernprogrammen werden weitere Funktionen hinzufügen.

Starten Sie Visual Studio, und wählen Sie **neues Projekt** von der Startseite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie den Visual C#-Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie **ASP.NET-Webanwendung** Vorlage.

![](creating-an-odata-endpoint/_static/image1.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage. Unter &quot;Hinzufügen von Ordnern und Verweise für core... &quot;, überprüfen Sie **Web-API**. Klicken Sie auf **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Ein Entitätsmodell hinzufügen

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. Für dieses Lernprogramm benötigen wir ein Modell, das ein Produkt darstellt. Das Modell entspricht unserer OData-Entitätstyp.

Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models. Wählen Sie im Kontextmenü der **hinzufügen** wählen Sie dann **Klasse**.

![](creating-an-odata-endpoint/_static/image3.png)

In der **hinzufügen** -Element Dialogfeld aus, nennen Sie die Klasse &quot;Produkt&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Gemäß der Konvention werden Modellklassen im Ordner Models platziert. Sie keine dieser Konvention in Ihren eigenen Projekten, aber wir es für dieses Lernprogramm verwenden.


Fügen Sie die folgende Klassendefinition in der Datei Product.cs hinzu:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Die ID-Eigenschaft werden die Entitätsschlüssel. Clients können die Produkte anhand der ID. Abfragen. Dieses Feld wird auch der Primärschlüssel in der Back-End-Datenbank sein.

Erstellen Sie jetzt das Projekt. Im nächsten Schritt verwenden wir einige Visual Studio-Gerüstbau, die Reflektion verwendet, um die Product-Typs zu suchen.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Fügen Sie einem OData-Controller hinzu

Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Sie definieren einen separaten Controller für jede Entität in Sie OData-Dienst festlegen. In diesem Lernprogramm erstellen wir einen einzelnen Controller.

Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers. Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.

![](creating-an-odata-endpoint/_static/image5.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld &quot;Web API 2 OData-Controller mit Aktionen unter Verwendung von Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

In der **Controller hinzufügen** Dialogfeld Namen des Controllers "ProductsController". Wählen Sie die &quot;asynchrone Controlleraktionen verwenden&quot; Kontrollkästchen. In der **Modell** Dropdown-Liste, wählen Sie die Product-Klasse.

![](creating-an-odata-endpoint/_static/image7.png)

Klicken Sie auf die **neuen Datenkontext...**  Schaltfläche. Lassen Sie den Standardnamen für den Datentyp für den Kontext, und klicken Sie auf **hinzufügen**.

![](creating-an-odata-endpoint/_static/image8.png)

Klicken Sie auf "hinzufügen", die im Dialogfeld Controller hinzufügen "auf den Controller hinzufügen".

![](creating-an-odata-endpoint/_static/image9.png)

Hinweis: Wenn Sie eine Fehlermeldung erhalten, der besagt, dass &quot;es wurde ein Fehler beim Abrufen des Typs... &quot;, stellen Sie sicher, dass Sie das Visual Studio-Projekt erstellt, nachdem Sie die Produktklasse hinzugefügt. Das Gerüst verwendet Reflektion, um die Klasse zu suchen.

![](creating-an-odata-endpoint/_static/image10.png)

Das Gerüst fügt zwei Codedateien zum Projekt hinzu:

- Products.cs definiert, die Web-API-Controller, der den OData-Endpunkt implementiert wird.
- ProductServiceContext.cs stellt Methoden zum Abfragen der zugrunde liegenden Datenbank, die Verwendung von Entity Framework bereit.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Hinzufügen des EDM und eine Route

Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs. Diese Klasse enthält den Konfigurationscode für die Web-API. Ersetzen Sie diesen Code durch Folgendes:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Dieser Code führt zwei Aufgaben aus:

- Erstellt ein Entity Data Model (EDM) für den OData-Endpunkt an.
- Fügt eine Route für den Endpunkt hinzu.

Ein EDM ist eine abstrakte Modell der Daten. Das EDM ist das Metadatendokument erstellen und definieren die URIs für den Dienst verwendet. Die **ODataConventionModelBuilder** EDM werden anhand eines Satzes von Standard-Benennungskonventionen EDM erstellt. Dieser Ansatz erfordert den geringsten Code. Wenn Sie mehr Kontrolle über das EDM soll, können Sie mithilfe der **ODataModelBuilder** Klasse EDM zu erstellen, indem Sie die Eigenschaften, Schlüssel und Navigationseigenschaften explizit hinzufügen.

Die **EntitySet** Methode fügt eine Entität, auf das EDM festgelegt:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Die Zeichenfolge "Products" definiert den Namen der Entitätenmenge. Der Name des Controllers muss der Name der Entitätenmenge überein. In diesem Lernprogramm wird die Entitätenmenge "Products" heißt und der Controller `ProductsController`. Wenn Sie mit dem Namen die Entität "ProductSet", nennen Sie den Controller `ProductSetController`. Beachten Sie, dass ein Endpunkt mehrere Entitätenmengen verfügen kann. Rufen Sie **EntitySet&lt;T&gt;**  für jede Entität festlegen, und definieren Sie einen entsprechenden Controller.

Die **MapODataRoute** Methode fügt eine Route für den OData-Endpunkt.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Der erste Parameter ist ein Anzeigename für die Route. Dieser Name werden von Clients des Diensts nicht angezeigt. Der zweite Parameter ist der URI-Präfix für den Endpunkt. Wenn Sie diesen Code, der URI für die Produkte Entitätenmenge lautet http://<em>Hostname</em>  /Odata/Produkte. Ihre Anwendung kann mehr als ein OData-Endpunkt besitzen. Rufen Sie für jeden Endpunkt <strong>MapODataRoute</strong> , und geben Sie einen eindeutigen Routennamen und einen eindeutigen URI-Präfix.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Ausgangswert für die Datenbank (Optional)

In diesem Schritt verwenden Sie Entity Framework, um die Datenbank mit einigen Testdaten Startwerten. Dieser Schritt ist optional, aber sie können die OData-Endpunkt sofort zu testen.

Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Dadurch wird einen Ordner namens Migrationen und eine Codedatei mit dem Namen "Configuration.cs" hinzugefügt.

![](creating-an-odata-endpoint/_static/image12.png)

Öffnen Sie diese Datei, und fügen Sie folgenden Code zu der `Configuration.Seed` Methode.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Diese Befehle generiert Code, der die Datenbank erstellt und führt dann diesen Code.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Untersuchen den OData-Endpunkt

In diesem Abschnitt verwenden wir die [Fiddler Webproxy Debuggen](http://www.fiddler2.com) Anforderungen an den Endpunkt gesendet werden, und überprüfen Sie die Antwortnachrichten. Dies hilft Ihnen beim Verständnis der Funktionen von einem OData-Endpunkt.

Drücken Sie F5, um mit dem Debuggen beginnen, in Visual Studio. Standardmäßig wird Visual Studio in Ihrem Browser geöffnet, mit denen `http://localhost:*port*`, wobei *Port* ist die Portnummer, die in den projekteinstellungen konfiguriert.

Sie können die Portnummer in den projekteinstellungen ändern. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**. Wählen Sie im Eigenschaftenfenster **Web**. Geben Sie die Portnummer unter **Projekt-Url**.

### <a name="service-document"></a>-Dienstdokument

Die *dienstdokument* enthält eine Liste von Entitätenmengen für den OData-Endpunkt. Um das dienstdokument zu erhalten, senden Sie eine GET-Anforderung an die Stamm-URI des Diensts ein.

Mit Fiddler, geben Sie den folgenden URI in die **Composer** Registerkarte: `http://localhost:port/odata/`, wobei *Port* ist die Portnummer.

![](creating-an-odata-endpoint/_static/image13.png)

Klicken Sie auf die **Execute** Schaltfläche. Fiddler sendet eine HTTP GET-Anforderung an die Anwendung. Die Antwort in der Liste der Web-Sitzungen sollte angezeigt werden. Wenn alles funktioniert, wird der Statuscode 200 sein.

![](creating-an-odata-endpoint/_static/image14.png)

Doppelklicken Sie auf die Antwort in der Liste Web Sitzungen, um die Details der Antwort auf die Registerkarte "Inspektoren" anzuzeigen.

![](creating-an-odata-endpoint/_static/image15.png)

Die unformatierte HTTP-Antwortnachricht sollte etwa wie folgt aussehen:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Standardmäßig werden mit Web-API das dienstdokument in AtomPub-Format zurückgegeben. Um JSON-Anforderung, fügen Sie die folgende Kopfzeile auf die HTTP-Anforderung:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Nachdem die HTTP-Antwort eine JSON-Nutzlast enthält:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Mit dem Dienstmetadatendokument

Die *dienstmetadatendokument* beschreibt das Datenmodell des Diensts mithilfe einer XML-Sprache, die die Conceptual Schema Definition Language (CSDL) aufgerufen. Das Metadatendokument veranschaulicht die Struktur der Daten in den Dienst und zum Generieren von Clientcode verwendet werden kann.

Um das Metadatendokument zu erhalten, senden Sie eine GET-Anforderung `http://localhost:port/odata/$metadata`. Hier werden die Metadaten für den Endpunkt in diesem Lernprogramm gezeigt.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Entitätenmenge

Um die Entitätenmenge für die Produkte zu erhalten, senden Sie eine GET-Anforderung `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entität

Um ein einzelnes Produkt zu erhalten, senden Sie eine GET-Anforderung `http://localhost:port/odata/Products(1)`, wobei "1" die Produkt-ID ist

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData-Serialisierungsformate

OData unterstützt auch mehrere Serialisierungsformate:

- Atom-Pub (XML)
- JSON "Light" (eingeführt in OData v3)
- JSON "verbose" (OData v2)

Standardmäßig verwendet die Web-API AtomPubJSON "Light" Format. 

Um AtomPub-Format zu erhalten, legen Sie den Accept-Header auf "Application/Atom + Xml" aus. Hier ist ein Beispiel-Antworttext:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Sie können ein offensichtlich Nachteil der Atom-Format finden Sie unter: Es ist wesentlich ausführlicher als die JSON-Light. Wenn ein Client ausgeführt wird, der AtomPub versteht, kann der Client dieses Format über JSON bevorzugen jedoch.

So sieht die JSON-light-Version derselben Entität aus:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Die JSON-light-Format wurde in Version 3 des OData-Protokolls eingeführt. Für die Abwärtskompatibilität kann ein Client das ältere "verbose" JSON-Format anfordern. Um das ausführliche JSON-Anforderung, legen Sie den Accept-Header auf `application/json;odata=verbose`. Hier ist die ausführliche Version:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Dieses Format vermittelt weitere Metadaten in den Antworttext, der erheblichen Mehraufwand über eine gesamte Sitzung hinzufügen kann. Darüber hinaus fügt eine Dereferenzierungsebene hinzu, durch das Objekt in einer Eigenschaft mit dem Namen "d" wrapping.

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von Entitätsbeziehungen](working-with-entity-relations.md)
- [Hinzufügen von OData-Aktionen](odata-actions.md)
- [Rufen Sie den OData-Dienst aus einem .NET-Client](calling-an-odata-service-from-a-net-client.md)
