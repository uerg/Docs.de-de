---
title: Antwort Komprimierung Middleware für ASP.NET Core
author: guardrex
description: Informationen Sie zur Antwort Komprimierung sowie zum Antwort Komprimierung Middleware in ASP.NET Core-apps verwenden.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: 152799500577dd09247bcee8c87cde39ca20aa79
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729573"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="37abd-103">Antwort Komprimierung Middleware für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37abd-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="37abd-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37abd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="37abd-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37abd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="37abd-106">Die Netzwerkbandbreite ist eine eingeschränkte Ressource.</span><span class="sxs-lookup"><span data-stu-id="37abd-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="37abd-107">Verringern die Größe der Antwort in der Regel wird die Reaktionsfähigkeit einer App häufig erheblich erhöht.</span><span class="sxs-lookup"><span data-stu-id="37abd-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="37abd-108">Eine Möglichkeit, verringern Sie die Größe der Nutzlast ist zum Komprimieren von Antworten für eine app.</span><span class="sxs-lookup"><span data-stu-id="37abd-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="37abd-109">Antwort Komprimierung Middleware verwenden</span><span class="sxs-lookup"><span data-stu-id="37abd-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="37abd-110">Serverbasierte Antwort komprimierungstechnologien in IIS, Apache oder Nginx verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="37abd-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="37abd-111">Die Leistung der Middleware überein nicht, die von der Servermodulen wahrscheinlich.</span><span class="sxs-lookup"><span data-stu-id="37abd-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="37abd-112">[HTTP.sys-Server](xref:fundamentals/servers/httpsys) und [Kestrel](xref:fundamentals/servers/kestrel) derzeit bieten Unterstützung für die integrierte Komprimierung nicht.</span><span class="sxs-lookup"><span data-stu-id="37abd-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="37abd-113">Verwenden Sie die Komprimierung Middleware Antwort beim:</span><span class="sxs-lookup"><span data-stu-id="37abd-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="37abd-114">Kann nicht die folgenden Server-basierten komprimierungstechnologien verwendet:</span><span class="sxs-lookup"><span data-stu-id="37abd-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="37abd-115">Modul für die dynamische Komprimierung von IIS</span><span class="sxs-lookup"><span data-stu-id="37abd-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="37abd-116">Apache Mod_deflate-Modul</span><span class="sxs-lookup"><span data-stu-id="37abd-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="37abd-117">Nginx-Komprimierung und Dekomprimierung</span><span class="sxs-lookup"><span data-stu-id="37abd-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="37abd-118">Hosten direkt auf:</span><span class="sxs-lookup"><span data-stu-id="37abd-118">Hosting directly on:</span></span>
  * <span data-ttu-id="37abd-119">[HTTP.sys-Server](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="37abd-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="37abd-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="37abd-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="37abd-121">Antwort-Komprimierung</span><span class="sxs-lookup"><span data-stu-id="37abd-121">Response compression</span></span>

<span data-ttu-id="37abd-122">In der Regel kann alle Antworten, die nicht komprimiert Antwort Komprimierung profitieren.</span><span class="sxs-lookup"><span data-stu-id="37abd-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="37abd-123">In der Regel nicht systemintern komprimierte Antworten enthalten: CSS, JavaScript, HTML, XML und JSON.</span><span class="sxs-lookup"><span data-stu-id="37abd-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="37abd-124">Sie sollten nicht systemintern komprimierte Ressourcen, wie z. B. PNG-Dateien komprimieren.</span><span class="sxs-lookup"><span data-stu-id="37abd-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="37abd-125">Wenn Sie versuchen, eine systemintern komprimierte Antwort weiter zu komprimieren, wird jeder kleinen zusätzlichen Reduzierung der Größe und die Übertragung zeitlich wahrscheinlich nach der Zeit benötigt wurde, um die Komprimierung zu verarbeiten verdeckt.</span><span class="sxs-lookup"><span data-stu-id="37abd-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="37abd-126">Komprimieren Sie Dateien, die weniger als etwa 150 1000 Bytes (je nach Inhalt der Datei und die Effizienz der Komprimierung) nicht.</span><span class="sxs-lookup"><span data-stu-id="37abd-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="37abd-127">Der Aufwand für kleine Dateien komprimieren kann es sich um eine komprimierte Datei, die größer als die nicht komprimierte Datei führen.</span><span class="sxs-lookup"><span data-stu-id="37abd-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="37abd-128">Wenn ein Client komprimiertem Inhalt verarbeiten kann, muss der Client durch Senden den Server seine Funktionen informieren der `Accept-Encoding` Header mit der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="37abd-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="37abd-129">Wenn ein Server komprimierte Inhalte sendet, muss er Informationen in enthalten die `Content-Encoding` Kopfzeile auf wie die komprimierte Antwort codiert ist.</span><span class="sxs-lookup"><span data-stu-id="37abd-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="37abd-130">Inhalt Codierung Bezeichnungen, die von der Middleware unterstützt werden in der folgenden Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="37abd-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="37abd-131">`Accept-Encoding` Headerwerte</span><span class="sxs-lookup"><span data-stu-id="37abd-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="37abd-132">Middleware unterstützt</span><span class="sxs-lookup"><span data-stu-id="37abd-132">Middleware Supported</span></span> | <span data-ttu-id="37abd-133">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="37abd-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="37abd-134">Nein</span><span class="sxs-lookup"><span data-stu-id="37abd-134">No</span></span>                   | <span data-ttu-id="37abd-135">Komprimierte Brotli-Datenformat</span><span class="sxs-lookup"><span data-stu-id="37abd-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="37abd-136">Nein</span><span class="sxs-lookup"><span data-stu-id="37abd-136">No</span></span>                   | <span data-ttu-id="37abd-137">UNIX-Datenformat "komprimieren"</span><span class="sxs-lookup"><span data-stu-id="37abd-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="37abd-138">Nein</span><span class="sxs-lookup"><span data-stu-id="37abd-138">No</span></span>                   | <span data-ttu-id="37abd-139">Daten in das Datenformat "Zlib" komprimiert "Verkleinern"</span><span class="sxs-lookup"><span data-stu-id="37abd-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="37abd-140">Nein</span><span class="sxs-lookup"><span data-stu-id="37abd-140">No</span></span>                   | <span data-ttu-id="37abd-141">W3C effiziente XML-Austausch</span><span class="sxs-lookup"><span data-stu-id="37abd-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="37abd-142">Ja (Standard)</span><span class="sxs-lookup"><span data-stu-id="37abd-142">Yes (default)</span></span>        | <span data-ttu-id="37abd-143">GZIP-Dateiformat</span><span class="sxs-lookup"><span data-stu-id="37abd-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="37abd-144">Ja</span><span class="sxs-lookup"><span data-stu-id="37abd-144">Yes</span></span>                  | <span data-ttu-id="37abd-145">Bezeichner "Keine Codierung": die Antwort nicht codiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="37abd-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="37abd-146">Nein</span><span class="sxs-lookup"><span data-stu-id="37abd-146">No</span></span>                   | <span data-ttu-id="37abd-147">Netzwerk-Übertragungsformat für Java-Archive</span><span class="sxs-lookup"><span data-stu-id="37abd-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="37abd-148">Ja</span><span class="sxs-lookup"><span data-stu-id="37abd-148">Yes</span></span>                  | <span data-ttu-id="37abd-149">Alle verfügbaren Inhalte, die Codierung wird nicht explizit angefordert</span><span class="sxs-lookup"><span data-stu-id="37abd-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="37abd-150">Weitere Informationen finden Sie unter der [IANA offizielle Schreiben von Code in der Inhaltsliste](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="37abd-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="37abd-151">Die Middleware können Sie zusätzliche Komprimierung, die Anbieter für benutzerdefinierte hinzufügen `Accept-Encoding` Headerwerte.</span><span class="sxs-lookup"><span data-stu-id="37abd-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="37abd-152">Weitere Informationen finden Sie unter [benutzerdefinierte Anbieter](#custom-providers) unten.</span><span class="sxs-lookup"><span data-stu-id="37abd-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="37abd-153">Die Middleware kann das Reagieren auf Qualitätswert (Qvalue, `q`) Gewichtung, wenn vom Client gesendet, Komprimierungsschemas zu priorisieren.</span><span class="sxs-lookup"><span data-stu-id="37abd-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="37abd-154">Weitere Informationen finden Sie unter [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="37abd-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="37abd-155">Komprimierungsalgorithmen unterliegen ein Kompromiss zwischen Geschwindigkeit der Komprimierung und die Effektivität der Komprimierung.</span><span class="sxs-lookup"><span data-stu-id="37abd-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="37abd-156">*Effektivität* in diesem Kontext bezieht sich auf die Größe der Ausgabe nach der Komprimierung.</span><span class="sxs-lookup"><span data-stu-id="37abd-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="37abd-157">Die kleinste Größe wird erreicht, indem die letzte *optimale* Komprimierung.</span><span class="sxs-lookup"><span data-stu-id="37abd-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="37abd-158">Die Header beteiligten anfordern, senden, Zwischenspeichern und Empfangen von komprimiertem Inhalt sind in der folgenden Tabelle beschrieben.</span><span class="sxs-lookup"><span data-stu-id="37abd-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="37abd-159">Header</span><span class="sxs-lookup"><span data-stu-id="37abd-159">Header</span></span>             | <span data-ttu-id="37abd-160">Rolle</span><span class="sxs-lookup"><span data-stu-id="37abd-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="37abd-161">Vom Client an den Server an, dass der Inhalt an den Client zulässigen Schemas Codierung gesendet.</span><span class="sxs-lookup"><span data-stu-id="37abd-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="37abd-162">Vom Server an den Client an, dass die Codierung des Inhalts in der Nutzlast gesendet.</span><span class="sxs-lookup"><span data-stu-id="37abd-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="37abd-163">Bei der Komprimierung, die `Content-Length` Header entfernt werden, da der Inhalt Text geändert, wenn die Antwort komprimiert wird.</span><span class="sxs-lookup"><span data-stu-id="37abd-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="37abd-164">Bei der Komprimierung, die `Content-MD5` Header entfernt, da der Inhalt des Nachrichtentexts geändert hat und der Hash nicht mehr gültig ist.</span><span class="sxs-lookup"><span data-stu-id="37abd-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="37abd-165">Gibt den MIME-Typ des Inhalts an.</span><span class="sxs-lookup"><span data-stu-id="37abd-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="37abd-166">Jede Antwort angeben sollten seine `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="37abd-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="37abd-167">Die Middleware überprüft diesen Wert, um zu bestimmen, ob die Antwort, komprimiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="37abd-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="37abd-168">Die Middleware gibt einen Satz von [Standard-MIME-Typen](#mime-types) , die codiert werden können, aber Sie können ersetzen oder Hinzufügen von MIME-Typen.</span><span class="sxs-lookup"><span data-stu-id="37abd-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="37abd-169">Sendens vom Server mit einem Wert von `Accept-Encoding` für Clients und -Proxys, die `Vary` -Header angibt, an den Client oder Proxy, der zwischengespeichert werden soll (variieren) Antworten basierend auf den Wert der `Accept-Encoding` -Header der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="37abd-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="37abd-170">Das Ergebnis der Rückgabe von Inhalt mit der `Vary: Accept-Encoding` Header ist, dass beide komprimiert und nicht komprimierte Antworten getrennt zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="37abd-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="37abd-171">Untersuchen Sie die Funktionen der Middleware Komprimierung Antwort mit der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="37abd-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="37abd-172">Das Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="37abd-172">The sample illustrates:</span></span>

* <span data-ttu-id="37abd-173">Die Komprimierung von app-Antworten, die mithilfe von Gzip und benutzerdefinierte Komprimierung Anbieter.</span><span class="sxs-lookup"><span data-stu-id="37abd-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="37abd-174">Wie die Standardliste der MIME-Typen für die Komprimierung einen MIME-Typ hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="37abd-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="37abd-175">Package</span><span class="sxs-lookup"><span data-stu-id="37abd-175">Package</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37abd-176">Um die Middleware in Ihrem Projekt einzuschließen, fügen Sie einen Verweis auf die [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) Paket.</span><span class="sxs-lookup"><span data-stu-id="37abd-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="37abd-177">Dieses Feature ist für alle Apps verfügbar, die für ASP.NET Core 1.1 oder höher konzipiert sind.</span><span class="sxs-lookup"><span data-stu-id="37abd-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="37abd-178">Um die Middleware in Ihrem Projekt einzuschließen, fügen Sie einen Verweis auf die [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) Verpacken, oder verwenden Sie die [Microsoft.AspNetCore.All Metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="37abd-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="37abd-179">Um die Middleware in Ihrem Projekt einzuschließen, fügen Sie einen Verweis auf die [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) Verpacken, oder verwenden Sie die [Microsoft.AspNetCore.App Metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="37abd-179">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="37abd-180">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="37abd-180">Configuration</span></span>

<span data-ttu-id="37abd-181">Der folgende Code zeigt, wie die Antwort Komprimierung Middleware für Standard-MIME-Typen und die standardmäßige Gzip-Komprimierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="37abd-181">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

> [!NOTE]
> <span data-ttu-id="37abd-182">Verwenden Sie ein Tool wie [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), oder [Postman](https://www.getpostman.com/) festzulegende der `Accept-Encoding` Anforderungsheader und zu untersuchen, das die Antwortheader, Größe und Text.</span><span class="sxs-lookup"><span data-stu-id="37abd-182">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="37abd-183">Übermitteln eine Anforderung an die Beispiel-app ohne die `Accept-Encoding` Header und beobachten Sie, dass die Antwort nicht komprimiert ist.</span><span class="sxs-lookup"><span data-stu-id="37abd-183">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="37abd-184">Die `Content-Encoding` und `Vary` Header nicht in der Antwort vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="37abd-184">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler-Fenster, das Ergebnis einer Anforderung ohne den Accept-Encoding-Header.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="37abd-187">Übermitteln eine Anforderung an die Beispiel-app mit der `Accept-Encoding: gzip` Header und beobachten Sie, dass die Antwort komprimiert wird.</span><span class="sxs-lookup"><span data-stu-id="37abd-187">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="37abd-188">Die `Content-Encoding` und `Vary` Header in der Antwort vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="37abd-188">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler-Fenster, Ergebnis der Anforderung mit den Accept-Encoding-Header und Wert Gzip anzeigt.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="37abd-192">Anbieter</span><span class="sxs-lookup"><span data-stu-id="37abd-192">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="37abd-193">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="37abd-193">GzipCompressionProvider</span></span>

<span data-ttu-id="37abd-194">Verwenden der [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) zum Komprimieren von Antworten mit Gzip.</span><span class="sxs-lookup"><span data-stu-id="37abd-194">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="37abd-195">Dies ist der Standardanbieter für die Komprimierung, keine Parameter angegeben.</span><span class="sxs-lookup"><span data-stu-id="37abd-195">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="37abd-196">Sie können festlegen, die Komprimierung mit dem [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="37abd-196">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="37abd-197">Die Gzip-Komprimierung-Anbieter wird standardmäßig auf die höchste Komprimierungsgrad ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), die möglicherweise nicht die effizienteste Komprimierung erzeugen.</span><span class="sxs-lookup"><span data-stu-id="37abd-197">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="37abd-198">Wenn die effizienteste Komprimierung gewünscht ist, können Sie die Middleware für die optimale Komprimierung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="37abd-198">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="37abd-199">Komprimierungsgrad</span><span class="sxs-lookup"><span data-stu-id="37abd-199">Compression Level</span></span> | <span data-ttu-id="37abd-200">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="37abd-200">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="37abd-201">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="37abd-201">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="37abd-202">Komprimierung sollte so schnell wie möglich abgeschlossen, auch wenn die resultierende Ausgabe optimal komprimiert ist nicht.</span><span class="sxs-lookup"><span data-stu-id="37abd-202">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="37abd-203">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="37abd-203">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="37abd-204">Keine Komprimierung muss ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="37abd-204">No compression should be performed.</span></span> |
| [<span data-ttu-id="37abd-205">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="37abd-205">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="37abd-206">Antworten sollten optimal komprimiert werden, auch wenn die Komprimierung mehr Zeit in Anspruch nimmt.</span><span class="sxs-lookup"><span data-stu-id="37abd-206">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="37abd-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="37abd-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="37abd-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="37abd-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="37abd-209">MIME-Typen</span><span class="sxs-lookup"><span data-stu-id="37abd-209">MIME types</span></span>

<span data-ttu-id="37abd-210">Die Middleware gibt eine Reihe von MIME-Typen für die Komprimierung an:</span><span class="sxs-lookup"><span data-stu-id="37abd-210">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="37abd-211">Sie können ersetzen oder MIME-Typen mit den Optionen für die Antwort Komprimierung Middleware angefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="37abd-211">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="37abd-212">Beachten Sie diese Platzhalter-MIME-Typen, z. B. `text/*` werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="37abd-212">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="37abd-213">Die Beispiel-app Fügt einen MIME-Typ für `image/svg+xml` und komprimiert und dient der ASP.NET Core Bannerbild (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="37abd-213">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="37abd-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="37abd-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="37abd-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="37abd-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="37abd-216">Benutzerdefinierte Anbieter</span><span class="sxs-lookup"><span data-stu-id="37abd-216">Custom providers</span></span>

<span data-ttu-id="37abd-217">Sie können benutzerdefinierte Komprimierung Implementierungen mit erstellen [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="37abd-217">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="37abd-218">Die [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) steht für die Codierung, die von dieser Inhalte `ICompressionProvider` erzeugt.</span><span class="sxs-lookup"><span data-stu-id="37abd-218">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="37abd-219">Die Middleware verwendet diese Informationen, die anhand der Liste, die im angegebenen Anbieter auswählen die `Accept-Encoding` -Header der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="37abd-219">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="37abd-220">Verwenden die Beispiel-app, sendet der Client eine Anforderung mit der `Accept-Encoding: mycustomcompression` Header.</span><span class="sxs-lookup"><span data-stu-id="37abd-220">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="37abd-221">Die Middleware verwendet die Implementierung von benutzerdefinierten Komprimierung und gibt die Antwort mit einer `Content-Encoding: mycustomcompression` Header.</span><span class="sxs-lookup"><span data-stu-id="37abd-221">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="37abd-222">Der Client muss dekomprimiert die benutzerdefinierte Codierung in der Reihenfolge für die Implementierung eines benutzerdefinierten Komprimierung arbeiten können.</span><span class="sxs-lookup"><span data-stu-id="37abd-222">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="37abd-223">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="37abd-223">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="37abd-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="37abd-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="37abd-225">Übermitteln eine Anforderung an die Beispiel-app mit der `Accept-Encoding: mycustomcompression` Header und beobachten Sie die Antwortheader.</span><span class="sxs-lookup"><span data-stu-id="37abd-225">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="37abd-226">Die `Vary` und `Content-Encoding` Header in der Antwort vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="37abd-226">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="37abd-227">Der Antworttext (nicht dargestellt) ist nicht im Beispiel komprimiert.</span><span class="sxs-lookup"><span data-stu-id="37abd-227">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="37abd-228">Es ist nicht in eine Implementierung von Komprimierung der `CustomCompressionProvider` -Klasse des Beispiels.</span><span class="sxs-lookup"><span data-stu-id="37abd-228">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="37abd-229">Allerdings wird im Beispiel, in dem Sie solche ein Komprimierungsalgorithmus implementiert würde.</span><span class="sxs-lookup"><span data-stu-id="37abd-229">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler-Fenster, Ergebnis der Anforderung mit den Accept-Encoding-Header und Wert Mycustomcompression anzeigt.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="37abd-232">Komprimierung mit sicheres Protokoll</span><span class="sxs-lookup"><span data-stu-id="37abd-232">Compression with secure protocol</span></span>

<span data-ttu-id="37abd-233">Komprimierte Antworten über sichere Verbindungen können gesteuert werden, mit der `EnableForHttps` Option ist standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="37abd-233">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="37abd-234">Dynamisch generierte Seiten mit Komprimierung Sicherheitsprobleme führen kann, wie z. B. die [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) und [Verletzung](https://wikipedia.org/wiki/BREACH_(security_exploit)) Angriffe.</span><span class="sxs-lookup"><span data-stu-id="37abd-234">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="37abd-235">Den Vary-Header hinzufügen</span><span class="sxs-lookup"><span data-stu-id="37abd-235">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37abd-236">Bei der Komprimierung von Antworten auf Grundlage der `Accept-Encoding` -Header, es gibt potenziell mehrere komprimierte Versionen der Antwort und eine nicht komprimierte Version.</span><span class="sxs-lookup"><span data-stu-id="37abd-236">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="37abd-237">Um den Client und Proxy-Caches anweisen, die mehrere Versionen vorhanden sind und gespeichert werden sollen, die `Vary` Kopfzeile wird hinzugefügt, und ein `Accept-Encoding` Wert.</span><span class="sxs-lookup"><span data-stu-id="37abd-237">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="37abd-238">In ASP.NET Core 2.0 oder höher, fügt die Middleware die `Vary` Header automatisch, wenn die Antwort komprimiert wird.</span><span class="sxs-lookup"><span data-stu-id="37abd-238">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37abd-239">Bei der Komprimierung von Antworten auf Grundlage der `Accept-Encoding` -Header, es gibt potenziell mehrere komprimierte Versionen der Antwort und eine nicht komprimierte Version.</span><span class="sxs-lookup"><span data-stu-id="37abd-239">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="37abd-240">Um den Client und Proxy-Caches anweisen, die mehrere Versionen vorhanden sind und gespeichert werden sollen, die `Vary` Kopfzeile wird hinzugefügt, und ein `Accept-Encoding` Wert.</span><span class="sxs-lookup"><span data-stu-id="37abd-240">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="37abd-241">In ASP.NET Core 1.x, Hinzufügen der `Vary` Header in die Antwort erfolgt manuell:</span><span class="sxs-lookup"><span data-stu-id="37abd-241">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="37abd-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="37abd-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="37abd-243">Middleware Problem hinter einen Nginx-reverse-proxy</span><span class="sxs-lookup"><span data-stu-id="37abd-243">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="37abd-244">Wenn eine Anforderung über einen Proxy von Nginx, ist die `Accept-Encoding` Header wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="37abd-244">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="37abd-245">Dadurch wird verhindert, dass die Middleware die Antwort zu komprimieren.</span><span class="sxs-lookup"><span data-stu-id="37abd-245">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="37abd-246">Weitere Informationen finden Sie unter [NGINX: Komprimierung und Dekomprimierung](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="37abd-246">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="37abd-247">Dieses Problem wird durch verfolgt [herausfinden, Pass-Through-Komprimierung für Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="37abd-247">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="37abd-248">Arbeiten mit dynamischen IIS-Komprimierung</span><span class="sxs-lookup"><span data-stu-id="37abd-248">Working with IIS dynamic compression</span></span>

<span data-ttu-id="37abd-249">Eine aktive IIS dynamische Modul für die Komprimierung auf Serverebene, die Sie, deaktivieren Sie die App möchten konfiguriert haben, können Sie dies tun, mit der eine Ergänzung der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="37abd-249">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="37abd-250">Weitere Informationen finden Sie unter [Disabling IIS modules (Deaktivieren von IIS-Modulen)](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="37abd-250">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="37abd-251">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="37abd-251">Troubleshooting</span></span>

<span data-ttu-id="37abd-252">Ein Tool wie [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), oder [Postman](https://www.getpostman.com/), Ihnen festzulegende ermöglichen die `Accept-Encoding` Anforderungsheader und zu untersuchen, das die Antwortheader, Größe und Text.</span><span class="sxs-lookup"><span data-stu-id="37abd-252">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="37abd-253">Die Antwort-Middleware-Komprimierung komprimiert Antworten, die die folgenden Bedingungen erfüllen:</span><span class="sxs-lookup"><span data-stu-id="37abd-253">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="37abd-254">Die `Accept-Encoding` Header mit dem Wert vorhanden ist `gzip`, `*`, oder benutzerdefinierte Codierung, die einen benutzerdefinierten Komprimierung-Anbieter entspricht, die Sie eingerichtet haben.</span><span class="sxs-lookup"><span data-stu-id="37abd-254">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="37abd-255">Der Wert darf nicht sein `identity` oder qualitätswerten (Qvalue, `q`) von 0 (null) festlegen.</span><span class="sxs-lookup"><span data-stu-id="37abd-255">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="37abd-256">Der MIME-Typ (`Content-Type`) muss festgelegt sein und muss einem MIME-Typ auf konfiguriert entsprechen den [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="37abd-256">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="37abd-257">Die Anforderung dürfen keine der `Content-Range` Header.</span><span class="sxs-lookup"><span data-stu-id="37abd-257">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="37abd-258">Die Anforderung muss unsicheres-Protokoll (http) verwenden, es sei denn, in die Antwort Komprimierung middlewareoptionen sicheres Protokoll (Https) konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="37abd-258">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="37abd-259">*Beachten Sie die Gefahr [oben beschriebenen](#compression-with-secure-protocol) bei der Aktivierung der Komprimierung für sichere Inhalte.*</span><span class="sxs-lookup"><span data-stu-id="37abd-259">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37abd-260">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="37abd-260">Additional resources</span></span>

* [<span data-ttu-id="37abd-261">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="37abd-261">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="37abd-262">Middleware</span><span class="sxs-lookup"><span data-stu-id="37abd-262">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="37abd-263">Mozilla Developer Network: Accept-Encoding.</span><span class="sxs-lookup"><span data-stu-id="37abd-263">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="37abd-264">RFC 7231 Abschnitt 3.1.2.1: Inhalt Codings</span><span class="sxs-lookup"><span data-stu-id="37abd-264">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="37abd-265">RFC 7230 Abschnitt 4.2.3: Gzip-Codierung</span><span class="sxs-lookup"><span data-stu-id="37abd-265">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="37abd-266">GZIP-Spezifikation Dateiformatversion 4.3</span><span class="sxs-lookup"><span data-stu-id="37abd-266">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
