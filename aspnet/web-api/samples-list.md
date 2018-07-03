---
uid: web-api/samples-list
title: Liste der Web-API-Beispiele | Microsoft-Dokumentation
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 6c9fff024df6c485b8d904d39cb0a4e5b5bf7e44
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362563"
---
<a name="web-api-samples-list"></a>Liste der Web-API-Beispiele
====================
## <a name="httpclient-samples"></a>HttpClient-Beispiele

**Beispiel für Bing übersetzen** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Zeigt, wie die [Microsoft Translator-Dienst](https://msdn.microsoft.com/library/ff512419.aspx) mithilfe der **"HttpClient"** Klasse. Die Microsoft Translator-API erfordert ein OAuth-Token, das die Anwendung durch Senden einer Anforderung an den Azure-token-Server für jede Anforderung an die Translator-Dienst erhält. Das Ergebnis der token-Server wird in die Anforderung an den Dienst für die Übersetzung einfließen. Vor dem Ausführen dieses Beispiels benötigen Sie ein [Anwendungsschlüssel aus Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) und füllen Sie die Informationen in der AccessTokenMessageHandler-Sample-Klasse.

**Beispiel für Google Maps** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Verwendet **"HttpClient"** zum Herunterladen einer Zuordnung von Redmond, WA, USA aus [Google Maps-API](https://developers.google.com/maps/), speichert es als eine lokale Datei und den standardmäßigen Image Viewer öffnet.

**Twitter-Client-Beispiels** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Schreiben Sie einen einfachen Twitter-Client mit veranschaulicht **"HttpClient"**. Das Beispiel verwendet eine **HttpMessageHandler** zum Einfügen von Informationen des OAuth-Authentifizierung in den ausgehenden **HttpRequestMessage**. Das Ergebnis von Twitter mit JSON.NET gelesen. Vor dem Ausführen dieses Beispiels benötigen Sie ein [Anwendungsschlüssel aus Twitter](https://dev.twitter.com/), und geben Sie die Informationen in der OAuthMessageHandler-Sample-Klasse.

**Beispiel der Weltbank** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Zeigt, wie zum Abrufen von Daten von der Weltbank datenwebsite mithilfe von JSON.NET das Ergebnis analysieren.

## <a name="web-api-samples"></a>Web-API-Beispiele

**Erste Schritte mit ASP.NET Web-API** | [VS 2012-Quelle](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Veranschaulicht, wie eine einfache Web-API zu erstellen, die HTTP GET-Anforderungen unterstützt. Enthält den Quellcode für das Tutorial [Ihre erste ASP.NET Web-API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web-API mit JavaScript-Szenarien – Kommentare** | [VS 2012-Quelle](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Veranschaulicht, wie ASP.NET Web-API zum Erstellen von Web-APIs, die Unterstützung für Browserclients und kann aufgerufen werden, problemlos mit jQuery.

**Wenden Sie sich an Manager** | [VS 2010-Quelle](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Dieses Beispiel verwendet die ASP.NET Web-API zum Erstellen einer einfachen Kontakt-Manager-Anwendung. Die Anwendung besteht aus einer Kontakt-Manager-Web-API, die von einer ASP.NET MVC-Anwendung und einer Windows Phone-Anwendung verwendet wird, zum Anzeigen und Verwalten einer Liste von Kontakten.

**Batchverarbeitung Beispiel** | [ausführliche Beschreibung](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Veranschaulicht das Implementieren von HTTP-Batchverarbeitung in ASP.NET. Die Batchverarbeitung besteht aus zu platzieren, mehrere HTTP-Anforderungen innerhalb der einzelnen MIME-multipart-Entitätstext, der dann an den Server als ein HTTP POST gesendet wird. Die Anforderungen einzeln verarbeitet werden, und die Antworten werden in einer anderen MIME-multipart-entitätstextteils, an den Client zurückgegeben wird eingefügt.

**Content-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Zeigt, wie das Lesen und Schreiben von Anforderung und Antwort-Entitäten, die asynchron mithilfe von Streams. Die Beispielcontroller verfügt über zwei Aktionen: eine PUT-Aktion, die Entitätskörper der Anforderung asynchron liest und speichert ihn in einer lokalen Datei und eine GET-Aktion, die den Inhalt der lokalen Datei zurückgibt.

**Beispiel für die benutzerdefinierte Assembly Konfliktlöser** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Zeigt, wie ASP.NET Web-API zum unterstützen der Ermittlung von Controllern aus einer Bibliotheksassembly dynamisch geladenen zu ändern. Das Beispiel implementiert einen benutzerdefinierten **IAssembliesResolver** die Standardimplementierung Ruft auf und fügt dann die Bibliotheksassembly mit den Standard-Ergebnissen.

**Benutzerdefinierte Media Type-Formatiererbeispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Veranschaulicht das Erstellen einer benutzerdefinierten Medientyp-Standardformatierer mithilfe der **BufferedMediaTypeFormatter** Basisklasse. Diese Basisklasse ist für Formatierungsprogramme vorgesehen, die in erster Linie verwenden Sie synchrone Lesevorgänge und Schreibvorgänge in Benennungseigenschaften. Zusätzlich zum Anzeigen der medientypformatierer, das Beispiel zeigt, wie zum einbinden, durch die Registrierung als Teil der **HttpConfiguration** für Ihre Anwendung. Beachten Sie, dass es auch möglich, verwenden Sie die **MediaTypeFormatter** direkte Basisklasse für Formatierer, die in erster Linie asynchron verwenden Lese- und Schreibvorgänge.

**Benutzerdefinierte Parameter-Beispiel zum Binden von** | [ausführliche Beschreibung](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Zeigt, wie der parameterbindungsprozess umfasst, angepasst wird, der bestimmt, wie Informationen von einer Anforderung an die Action-Parameter gebunden ist. In diesem Beispiel hat der Home-Controller vier Aktionen:

1. BindPrincipal wird angezeigt, wie Sie einen IPrincipal-Parameter aus einem benutzerdefinierten Prinzipal generische, nicht über eine HTTP GET-Nachricht zu binden.
2. BindCustomComplexTypeFromUriOrBody wird angezeigt, binden Sie einen komplexen Typ-Parameter, der entweder aus dem Nachrichtentext oder vom Anforderungs-URI einer HTTP POST-Nachricht angegeben werden kann.
3. BindCustomComplexTypeFromUriWithRenamedProperty shows how to bind a complex-type parameter with a renamed property which comes from the request URI of an HTTP POST message;
4. PostMultipleParametersFromBody wird angezeigt, wie Sie mehrere Parameter aus dem Text für eine postingnachricht zu binden.

**Datei hochladen Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Zeigt, wie zum Hochladen von Dateien in ein **ApiController** mit MIME-Multipart-Datei hochladen und Einrichten von Benachrichtigungen zum Status mit **"HttpClient"** mit **ProgressNotificationHandler**. Der Controller liest den Inhalt des HTML-Datei hochladen asynchron, und eine oder mehrere Textteile in einer lokalen Datei schreibt. Die Antwort enthält Informationen über die hochgeladene Datei (oder Dateien).

**Datei hochladen in Azure Blob Store-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Dieses Beispiel ist die Beispieldatei hochladen ähnelt, aber statt die hochgeladenen Dateien auf dem lokalen Datenträger speichern lädt asynchron die Dateien in [Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) mit [Windows Azure SDK für .NET](https://www.windowsazure.com/develop/net/). Es bietet außerdem einen Mechanismus zum Auflisten der Blobs, die derzeit in der ein [Azure Blob Storage-Container](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Sie können versuchen, das Beispiel für **Azure-Speicheremulator** , die im Lieferumfang von Azure SDK. Wenn Sie haben eine [Azure Storage-Konto](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), Sie können auf den tatsächlichen Storage-Dienst ebenfalls ausführen.

**HTTP-Message-Handler-Pipelinebeispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Zeigt, wie verknüpfen **HttpMessageHandler** -Instanzen auf dem Client (**"HttpClient"**) und Server (ASP.NET Web-API). In diesem Beispiel wird der gleiche Handler auf dem Client und Server verwendet. Während es nur selten auftritt, dass der genaue gleiche Handler an beiden Orten ausgeführt werden würde, ist das Objektmodell auf Client- und Serverseite identisch.

**Hochladen von JSON-Beispiel** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Zeigt, wie zum Hoch- und Herunterladen von JSON in und aus einem **ApiController**. Das Beispiel verwendet eine minimale **ApiController** und greift auf mit **"HttpClient"**.

**Mashup-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Zeigt, wie mehrere Remotestandorte asynchron aus den Zugriff auf eine **ApiController** Aktion. Jedes Mal, wenn die Aktion erreicht wird, werden die Anforderungen asynchron ausgeführt, so dass keine Threads blockiert werden.

**Beispiel für die Arbeitsspeicher-Ablaufverfolgung** | [ausführliche Beschreibung](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Dieses Beispielprojekt erstellt ein Nuget-Paket, das einen benutzerdefinierte in-Memory-Ablaufverfolgungs-Writer in ASP.NET Web-API-Anwendungen installieren.

**Beispiel für MongoDB** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Zeigt, wie MongoDB als persistenten Speicher für eine **ApiController**, ein Repositorymuster verwenden.

**Beispiel für Antwort Text den Prozessor** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Zeigt, wie auf eine Antwortentität (d. h. eine HTTP-Antworttext) in einer lokalen Datei zu kopieren, bevor sie an den Client übertragen wird, und führen die zusätzliche Verarbeitung für diese Datei asynchron. Das Beispiel implementiert eine **HttpMessageHandler** , das die Antwortentität mit einem, die beide schreibt selbst in der Ausgabe als normale und in eine lokale Datei umschließt.

**Hochladen von "XDocument" Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Zeigt, wie einem XDocument zum Hochladen einer **ApiController** mit **PushStreamContent** und **"HttpClient"**.

**Beispiel für bindungsvalidierung** | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Zeigt, wie Sie Validierungsattribute auf Ihrer Modelle in ASP.NET Web-API verwenden können, um den Inhalt der HTTP-Anforderung zu überprüfen. Zeigt zum Markieren von Eigenschaften wie erforderlich, wie die beiden Framework definierten und benutzerdefinierte Validierungsattribute Ihr Modell mit Anmerkungen versehen und wie Fehlerantworten für ungültigen Modellstatus zurückgegeben.

**Web Form-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Zeigt ein ApiController-Element zu einem Web Forms-Projekt hinzugefügt.

**[RestBugs-Beispiel](https://github.com/howarddierking/RestBugs)**

RestBugs ist eine einfache fehlernachverfolgung-Anwendung, die zeigt, wie Sie ASP.NET Web-API und die neue HTTP-Client-Bibliothek verwenden, um ein Hypermedia-gesteuerte System zu erstellen. Das Beispiel umfasst sowohl Client-als auch Implementierungen, die mit ASP.NET Web API. Der Server verwendet einen benutzerdefinierten Formatierer von Razor zum Generieren von ressourcendarstellungen. Das Beispiel bietet auch einen node.js-Server, um die Vorteile zu veranschaulichen, die mithilfe eines Hypermedia-Entwurfs entkoppeln, Clients und Servern stammen.

## <a name="web-api-extensions-preview-samples"></a>Beispielen für Web-API-Erweiterungen (Vorschau)

**OData abfragbaren Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Zeigt, wie OData-Abfragen in ASP.NET Web-API mit Einführung der `[Queryable]` Attributs oder durch die Verwendung der **ODataQueryOptions** Action-Parameters, wodurch die Aktion, die die Abfrage manuell überprüfen möchte, bevor er ausgeführt wird, wird.

Die CustomerController zeigt die Verwendung von [Queryable]-Attribut und das OrderController wird gezeigt, wie mit dem ODataQueryOptions-Parameter. Die ResponseController ähnelt der CustomerController jedoch anstelle der GET-Aktion zurückgeben `IEnumerable<Customer>` wird ein **HttpResponseMessage**. Dadurch können wir zusätzliche Headerfelder hinzufügen, bearbeiten den Statuscode usw., während weiterhin Abfragefunktionalität verwenden. Das Beispiel veranschaulicht die Abfragen mit $orderby "," $skip "," $top "," any() "," all(), und "$filter.

**OData-Dienstbeispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Dieses Beispiel veranschaulicht, wie Sie einen OData-Dienst, bestehend aus drei Entitäten und drei Web-API-Controller zu erstellen. Die Controller bieten verschiedene Ebenen der Funktionalität in Bezug auf die OData-Funktionen, die sie verfügbar machen:

Die SupplierController verfügbar macht, eine Teilmenge der Funktionalität, einschließlich der Abfrage, abrufen, indem Schlüssel "und" erstellen, indem Sie diese Anforderungen zu behandeln:

- /Suppliers abrufen
- /Suppliers(key) abrufen
- GET-/Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

Die ProductsController verfügbar gemachten GET, PUT, POST, löschen und durch die Implementierung einer Aktion für jeden dieser Vorgänge direkt PATCHEN.

Die ProductFamilesController nutzt die EntitySetController-Basisklasse, die ein nützliches Muster für die Implementierung eines umfangreichen OData-Diensts verfügbar macht.

Darüber hinaus macht der OData-Dienst ein $metadata-Dokument, das zu den verarbeiteten Daten kann von WCF Data Service-Clients und anderen Clients, die das $metadata-Format akzeptieren.
