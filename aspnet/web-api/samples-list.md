---
uid: web-api/samples-list
title: Web-API prüft Liste | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038987"
---
<a name="web-api-samples-list"></a>Liste der Web-API-Beispiele
====================
## <a name="httpclient-samples"></a>HttpClient-Beispiele

**Bing übersetzen Sample** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Zeigt, wie die [Microsoft Translator Dienst](https://msdn.microsoft.com/library/ff512419.aspx) mithilfe der **"HttpClient"** Klasse. Die Microsoft Translator-dienstverwaltungs-API erfordert einen OAuth-Token, das durch Senden einer Anforderung an den Azure-token-Server für jede Anforderung an den Dienst Konvertierer, ruft die Anwendung ab. Das Ergebnis aus der token-Server ist in der Anforderung an den Übersetzungsdienst gesendet eingezogen. Vor dem Ausführen dieses Beispiels benötigen Sie ein [Anwendungsschlüssel aus Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) und füllen Sie die Informationen in der AccessTokenMessageHandler-Beispiel-Klasse.

**Google Maps Sample** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Verwendet **"HttpClient"** zum Herunterladen einer Zuordnung von Redmond, WA aus [Google Maps-API](https://developers.google.com/maps/), speichert es als eine lokale Datei und den standardmäßigen Image Viewer geöffnet.

**Beispiel für Client Twitter** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Veranschaulicht das Schreiben eines einfachen Twitter-Clients mithilfe von **"HttpClient"**. Das Beispiel verwendet eine **HttpMessageHandler** zum Einfügen von OAuth-Authentifizierungsinformationen in den ausgehenden **HttpRequestMessage**. Das Ergebnis von Twitter wird mithilfe von JSON.NET gelesen. Vor dem Ausführen dieses Beispiels benötigen Sie ein [Anwendungsschlüssel aus Twitter](https://dev.twitter.com/), und geben Sie die Informationen in der OAuthMessageHandler-Beispiel-Klasse.

**Bank World-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Veranschaulicht das Abrufen von Daten aus der Welt Bank Data-Website, über JSON.NET, um das Ergebnis zu analysieren.

## <a name="web-api-samples"></a>Web-API-Beispiele

**Erste Schritte mit ASP.NET Web API** | [VS 2012-Quelle](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Zeigt, wie eine einfache Web-API zu erstellen, die HTTP GET-Anforderungen unterstützt. Enthält den Quellcode für das Lernprogramm [Ihre erste ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web API-JavaScript-Szenarien – Kommentare** | [VS 2012-Quelle](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Zeigt, wie ASP.NET Web-API zum Erstellen von Web-APIs, die Browserclients unterstützt und können problemlos aufgerufen werden unter Verwendung von jQuery.

**Wenden Sie sich an Manager** | [VS 2010-Quelle](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Dieses Beispiel verwendet die ASP.NET Web-API zum Erstellen einer einfachen Kontakt-Manager-Anwendung. Die Anwendung besteht aus einer Kontakt-Manager-Web-API, die von einer ASP.NET MVC-Anwendung und eine Windows Phone-Anwendung anzeigen und Verwalten einer Liste von Kontakten verwendet wird.

**Batchverarbeitung Beispiel** | [ausführliche Beschreibung](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Veranschaulicht das Implementieren von HTTP-Batchverarbeitung innerhalb von ASP.NET. Die Batchverarbeitung besteht aus einfügen mehrere HTTP-Anforderungen innerhalb einer einzelnen MIME-multipart Entitätstext, die dann als HTTP POST an den Server gesendet wird. Die Anforderungen einzeln verarbeitet werden, und die Antworten in einer anderen MIME-multipart Entitätstext, die an den Client zurückgegeben wird zusammengestellt werden.

**Inhalts-Controller-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Zeigt, wie zum Lesen und Schreiben von Anforderung und Antwort-Entitäten, die asynchron mit Streams. Die Beispielcontroller verfügt über zwei Aktionen: eine PUT-Aktion, die der Anforderungsentität asynchron liest und speichert diesen in eine lokale Datei, und eine GET-Aktion, die den Inhalt der lokalen Datei zurückgibt.

**Benutzerdefinierte Assembly Konfliktlöser Beispiel** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Veranschaulicht das Ändern der ASP.NET Web-API zum unterstützen der Ermittlung von Domänencontrollern aus einer dynamisch geladene Bibliotheksassembly. Das Beispiel implementiert eine benutzerdefinierte **IAssembliesResolver** die ruft der standardmäßige Implementierung und fügt dann die Bibliotheksassembly führt das hinzu.

**Benutzerdefinierte Media Typ Formatiererbeispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Veranschaulicht das Erstellen eines benutzerdefinierten medientypformatierer mithilfe der **BufferedMediaTypeFormatter** Basisklasse. Diese Basisklasse ist für Formatierungsprogramme vorgesehen, die in erster Linie verwenden synchrone Lesevorgänge und Schreibvorgänge. Zusätzlich zum Anzeigen der medientypformatierer, die im Beispiel wird gezeigt, wie zum einbinden, durch die Registrierung als Teil der **HttpConfiguration** für Ihre Anwendung. Beachten Sie, dass es auch möglich, ist die **MediaTypeFormatter** direkte Basisklasse für Formatierungsprogramme, die in erster Linie asynchrone verwenden Lese- und Schreibvorgänge.

**Benutzerdefinierte Parameter-Beispiel zum Binden von** | [ausführliche Beschreibung](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Zeigt, wie der parameterbindungsprozess umfasst, angepasst wird, der bestimmt, wie Informationen aus einer Anforderung an die Action-Parameter gebunden ist. In diesem Beispiel hat der Home-Controller vier Aktionen:

1. BindPrincipal wird gezeigt, wie Sie einen IPrincipal-Parameter aus einer benutzerdefinierten generischen Prinzipal nicht aus einer HTTP GET-Nachricht zu binden:
2. BindCustomComplexTypeFromUriOrBody wird gezeigt, wie einen komplexen Typ-Parameter gebunden, der aus dem Nachrichtentext oder vom Anforderungs-URI einer HTTP POST-Nachricht werden konnte:
3. BindCustomComplexTypeFromUriWithRenamedProperty shows how to bind a complex-type parameter with a renamed property which comes from the request URI of an HTTP POST message;
4. PostMultipleParametersFromBody wird gezeigt, wie Sie mehrere Parameter aus dem Text für eine POST-Nachricht zu binden:

**Datei hochladen Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Zeigt, wie zum Hochladen von Dateien auf einer **ApiController** mit MIME-Multipart-Datei hochladen und zum Einrichten von Benachrichtigungen zum Status mit **"HttpClient"** mit **ProgressNotificationHandler**. Der Controller asynchron liest den Inhalt einer HTML-Datei hochladen und eine oder mehrere Textteile in einer lokalen Datei schreibt. Die Antwort enthält Informationen über die hochgeladene Datei (oder Dateien).

**Datei hochladen in Azure BLOB-Speicher – Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Dieses Beispiel ist die Datei hochladen Beispiel ähnelt, aber statt der hochgeladenen Dateien auf dem lokalen Datenträger speichern asynchron hochgeladen Dateien [Azure Blob-Speicher](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) mit [Windows Azure SDK für .NET](https://www.windowsazure.com/develop/net/). Es bietet auch einen Mechanismus zum Auflisten der Blobs, die derzeit in einer [Azure Blob-Speichercontainer](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Sie können versuchen, das Beispiel für **Azure-Speicheremulator** wird, die mit dem Azure SDK. Ist ein [Azure Storage-Konto](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), Sie können für den real-Speicherdienst ausführen.

**HTTP-Nachricht Handler Pipelinebeispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Zeigt, wie Sie von Netzwerkdaten **HttpMessageHandler** Instanzen auf dem Client (**"HttpClient"**) und Server (ASP.NET Web-API). In diesem Beispiel wird der gleiche Handler auf dem Client und Server verwendet. Während es nur selten auftritt, die genaue gleiche Handler an beiden Orten führen würde, ist das Objektmodell auf Client und Server Side identisch.

**Beispiel für JSON hochladen** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Zeigt, wie hoch-und Herunterladens von JSON in und aus einer **ApiController**. Das Beispiel verwendet eine minimale **ApiController** und greift auf mit **"HttpClient"**.

**Das Mashup Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Veranschaulicht mehrere Remotestandorte asynchron aus den Zugriff auf eine **ApiController** Aktion. Jedes Mal, wenn die Aktion erreicht wird, werden die Anforderungen asynchron ausgeführt, damit keine Threads blockiert werden.

**Arbeitsspeicher-Tracing-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Dieses Beispielprojekt erstellt ein NuGet-Paket, das ein benutzerdefinierter in-Memory-ablaufverfolgungswriter in ASP.NET Web-API-Anwendungen installieren.

**Beispiel für MongoDB** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Zeigt, wie MongoDB als den permanenten Speicher für eine **ApiController**, einem Repositorymuster verwenden.

**Antwort Text Prozessor Beispiel** | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Zeigt, wie eine Antwortentität (d. h. ein HTTP-Antworttext) in eine lokale Datei kopieren, vor der Übertragung an den Client und eine zusätzliche Verarbeitung für diese Datei asynchron auszuführen. Das Beispiel implementiert eine **HttpMessageHandler** , die die Antwortentität mit einem, dass beide schreibt selbst in die Ausgabe wie gewohnt und in eine lokale Datei einschließt.

**Hochladen von "XDocument" Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Zeigt, wie ein "XDocument" zum Hochladen einer **ApiController** mit **PushStreamContent** und **"HttpClient"**.

**Überprüfung Beispiel** | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Zeigt an, wie Validierungsattribute auf die Modelle in ASP.NET WebAPI verwenden, um den Inhalt der HTTP-Anforderung überprüfen. Veranschaulicht die Eigenschaften nach Bedarf, wie die beiden Framework definierte und eigene benutzerdefinierte Validierungsattribute Ihr Modell mit einer Anmerkung zu markieren und Fehlerantworten für ungültige modellzustände zurückgegeben.

**Web Form-Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Zeigt eine ApiController einer Web Forms-Projekt hinzugefügt.

**[RestBugs-Beispiel](https://github.com/howarddierking/RestBugs)**

RestBugs ist eine einfache fehlernachverfolgung-Anwendung, die veranschaulicht, wie ASP.NET Web-API und die neue HTTP-Client-Bibliothek verwenden, um ein System Hypermedia-driven zu erstellen. Das Beispiel umfasst sowohl Client-als auch mithilfe der ASP.NET Web API Implementierungen. Der Server verwendet ein benutzerdefiniertes Formatierungsprogramm für Razor Ressource Darstellungen zu generieren. Das Beispiel enthält auch einen node.js-Server, um die Vorteile zu veranschaulichen, die von der Verwendung eines Entwurfs Hypermedia, um Clients und Servern zu entkoppeln stammen.

## <a name="web-api-extensions-preview-samples"></a>Web-API-Erweiterungen Preview-Beispiele

**OData-abfragbare Beispiel** | [ausführliche Beschreibung](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Zeigt, wie OData-Abfragen in ASP.NET Web API mittels entstehen der `[Queryable]` -Attribut oder durch die Verwendung der **ODataQueryOptions** Action-Parameter die Aktion, die Abfrage manuell zu überprüfen, bevor er ausgeführt wird, wird dadurch.

Die CustomerController zeigt die Verwendung von [Queryable]-Attribut, und die OrderController zeigt, wie den ODataQueryOptions-Parameter. Die ResponseController ähnelt der CustomerController jedoch anstelle der GET-Aktion zurückgeben `IEnumerable<Customer>` gibt ein **HttpResponseMessage**. Dadurch können wir zusätzlichen Headerfelder hinzufügen, bearbeiten den Statuscode "" usw., während Sie weiterhin Abfragefunktionalität verwenden. Das Beispiel veranschaulicht die Abfragen mit $orderby, $skip $top, any(), all() und $filter.

**Beispiel für OData-Dienst** | [ausführliche Beschreibung](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [VS 2010-Quelle](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

In diesem Beispiel wird veranschaulicht, wie einen OData-Dienst besteht aus drei Entitäten und drei Web-API-Controller erstellt wird. Die Controller bieten verschiedene Ebenen der Funktionalität in Bezug auf die OData-Funktionalität, die sie verfügbar machen:

Die SupplierController macht eine Teilmenge der Funktionen, einschließlich der Abfrage, abrufen, indem Schlüssel und erstellen, durch die Behandlung dieser Anforderungen:

- /Suppliers abrufen
- GET /Suppliers(key)
- GET-/Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

Die ProductsController macht GET, PUT, POST, löschen und durch Implementieren einer Aktion für die einzelnen Vorgänge direkt PATCH.

Die ProductFamilesController nutzt die EntitySetController Basisklasse, die ein nützliches Muster für die Implementierung eines umfangreichen OData-Diensts verfügbar macht.

Darüber hinaus macht der OData-Dienst ein $metadata-Dokument, das die verbrauchten ermöglicht WCF Data Service-Clients und anderen Clients, die das $metadata-Format akzeptieren.
