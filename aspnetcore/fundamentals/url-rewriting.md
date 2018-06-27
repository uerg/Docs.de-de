---
title: URL-umschreibende Middleware in ASP.NET Core
author: guardrex
description: Informationen zum Umschreiben und Umleiten von URL mit URL-umschreibender Middleware in ASP.NET Core-Anwendungen
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d3484e222c4412a427d086c1b71a12b81095ba72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276346"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>URL-umschreibende Middleware in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex) und [Mikael Mengistu](https://github.com/mikaelm12)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Bei der URL-Umschreibung werden die Anforderungs-URLs verändert, die auf mindesten einer vordefinierten Regel basieren. Es wird eine Abstraktion zwischen den Speicherorten und Adressen von Ressourcen erstellt, sodass diese nicht eng miteinander verknüpft sind. Für folgende Szenarios kann die URL-Umschreibung angewendet werden:

* Temporäres oder permanentes Verschieben oder Ersetzen von Serverressourcen, wobei stabile Locators für diese Ressourcen erhalten bleiben.
* Verteilen der Verarbeitung von Anforderungen auf verschiedene Apps oder Bereiche einer App.
* Entfernen, Hinzufügen oder Umorganisieren von URL-Segmenten in eingehenden Anforderungen.
* Optimieren von öffentlichen URLs für die Suchmaschinenoptimierung (Search Engine Optimization, SEO).
* Zulassen, dass unterstützte öffentliche URLs verwendet werden, damit die Benutzer den Inhalt einsehen können, der ihnen beim Klicken auf einen Link angezeigt wird.
* Umleiten von unsicheren Anforderungen auf sichere Endpunkte.
* Vermeiden von Hotlinking von Bildern.

Es gibt verschiedene Möglichkeiten, Regeln zum Ändern der URL zu definieren. Sie können z.B. RegEx, die Apache-Modulregeln „mod_rewrite“, die Regeln zum IIS-Umschreibungsmodul oder benutzerdefinierte logische Regeln verwenden. In diesem Artikel wird das Umschreiben von URLs beschrieben. Außerdem erhalten Sie Anweisungen zur Verwendung der URL-umschreibenden Middleware in ASP.NET Core-Apps.

> [!NOTE]
> Wenn Sie URLs umschreiben, kann das negative Auswirkungen auf die Leistung einer App haben. Wenn möglich, sollten Sie so wenig Regeln wie möglich erstellen und darauf achten, dass sie nicht zu kompliziert sind.

## <a name="url-redirect-and-url-rewrite"></a>Umleiten und Umschreiben von URLs

Auf den ersten Blick scheint der Unterschied zwischen dem *Umleiten* und *Umschreiben* von URLs eher gering zu sein, allerdings haben die beiden Verfahren sehr unterschiedliche Auswirkungen auf die Bereitstellung von Ressourcen für Clients. Die URL-umschreibenden Middleware von ASP.NET Core kann für beide Vorgänge verwendet werden.

Bei der *Umleitung von URLs* handelt es sich um einen clientseitigen Vorgang, bei dem der Client angewiesen wird, auf eine Ressource unter einer anderen Adresse zuzugreifen. Dafür ist ein Roundtrip zum Server erforderlich. Die Umleitungs-URL, die an den Client zurückgegeben wird, wird in der Adressleiste des Browsers angezeigt, wenn der Client eine neue Anforderung an die Ressource sendet. 

Wenn `/resource` auf `/different-resource` *umgeleitet* wird, fordert der Client `/resource` an. Der Server sendet dann die Antwort, dass der Client die Ressource unter `/different-resource` abrufen soll. In der Antwort ist außerdem ein Statuscode enthalten, aus dem entnommen werden kann, ob die Umleitung temporär oder permanent ist. Der Client führt unter der Umleitungs-URL eine neue Anforderung für die Ressource aus.

![Ein Web-API-Dienstendpunkt wurde auf dem Server kurzzeitig von Version 1 (v1) auf Version 2 (v2) geändert. Der Client sendet eine Anforderung an den Dienst unter dem Pfad für Version 1: „/v1/api“. Der Server sendet eine „302 – Gefunden“-Antwort mit dem neuen, temporären Pfad für Version 2: „/v2/api“. Der Client sendet eine zweite Anforderung an den Dienst unter der Umleitungs-URL. Der Server gibt den Statuscode „200 – OK“ zurück.](url-rewriting/_static/url_redirect.png)

Wenn Anforderungen auf eine andere URL umgeleitet werden, müssen Sie angeben, ob die Umleitung temporär oder permanent sein soll. Der Statuscode „301 – Permanent verschoben“ wird verwendet, wenn der Ressource eine neue permanente URL zugewiesen wurde, und Sie dem Client die Anweisung geben möchten, dass alle zukünftigen Anforderungen an die Ressource die neue URL verwenden sollen. *Der Client kann die Antwort zwischenspeichern, wenn der Statuscode 301 empfangen wird.* Der Statuscode „302 – Gefunden“ wird verwendet, wenn die Umleitung temporär ist oder häufig verändert wird. Dann speichert der Client die Umleitungs-URL nicht und verwendet sie auch nicht erneut. Weitere Informationen finden Sie unter [RFC 2616: Status Code Definitions (RFC 2616: Statuscodedefinitionen)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Bei der *Umschreibung einer URL* handelt es sich um einen serverseitigen Vorgang, über den Ressourcen von einer anderen Ressourcenadresse bereitgestellt werden. Wenn eine URL umgeschrieben wird, ist kein Roundtrip zum Server erforderlich. Die umgeschriebene URL wird nicht an den Server zurückgegeben und nicht in der Adressleiste des Browsers angezeigt. Wenn `/resource` in `/different-resource` *umgeschrieben* wird, sendet der Client die Anforderung `/resource`, und der Server ruft die Ressource *intern* unter `/different-resource` ab. Auch wenn der Client die Ressource unter der umgeschriebenen URL abrufen kann, erhält er nicht die Information, dass die Ressource unter der umgeschriebenen URL gespeichert ist, wenn er die Anforderung sendet und eine Antwort erhält.

![Ein Web-API-Dienstendpunkt wurde auf dem Server von Version 1 (v1) auf Version 2 (v2) geändert. Der Client sendet eine Anforderung an den Dienst unter dem Pfad für Version 1: „/v1/api“. Die Anforderungs-URL wird umgeschrieben, damit über den Pfad für Version 2 („/v2/api“) auf den Dienst zugegriffen werden kann. Der Dienst sendet eine Antwort an den Client mit dem Statuscode „200 – OK“.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL-umschreibende Beispiel-App

Mit der [URL-umschreibenden Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) können Sie die Features der URL-umschreibenden Middleware testen. Die App wendet Regeln zum Umschreiben und Umleiten an und stellt die umgeschriebene oder umgeleitete URL dar.

## <a name="when-to-use-url-rewriting-middleware"></a>Empfohlene Verwendung der URL-umschreibenden Middleware

Verwenden Sie die URL-umschreibende Middleware, wenn Sie das [URL-Umschreibungsmodul](https://www.iis.net/downloads/microsoft/url-rewrite) nicht mit IIS unter Windows Server verwenden können, wenn Sie unter Apache-Server nicht mit dem [Apache-Modul „mod_rewrite“](https://httpd.apache.org/docs/2.4/rewrite/) arbeiten können, das [Umschreiben von URLs unter Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) nicht funktioniert oder Ihre App von [HTTP.sys-Server](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)) gehostet wird. Sie sollten mit IIS, Apache oder Nginx serverbasierte Technologien zum Umschreiben von URLs verwenden, da die Middleware nicht alle Features dieser Module unterstützt und die Leistung der Middleware nicht mit der Leistung der Module übereinstimmt. Dennoch funktionieren einige Features der Servermodule nicht mit ASP.NET Core-Projekten – z.B. die Einschränkungen `IsFile` und `IsDirectory` des IIS-Umschreibungsmoduls. Verwenden Sie in diesem Szenario stattdessen die Middleware.

## <a name="package"></a>Package

Fügen Sie einen Verweis auf das [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)-Paket hinzu, um die Middleware in Ihrem Projekt zu verwenden. Dieses Feature ist für alle Apps verfügbar, die für ASP.NET Core 1.1 oder höher konzipiert sind.

## <a name="extension-and-options"></a>Erweiterung und Optionen

Legen Sie Regeln zum Umschreiben und Umleiten von URLs fest, indem Sie eine Instanz der `RewriteOptions`-Klasse erstellen und Erweiterungsmethoden für jede Regel hinzufügen. Verketten Sie mehrere Regeln in der Reihenfolge miteinander, in der sie verarbeitet werden sollen. Die `RewriteOptions` werden an die URL-umschreibende Middleware übergeben, wenn diese mit `app.UseRewriter(options);` zu der Anforderungspipeline hinzugefügt wird.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

### <a name="url-redirect"></a>Umleitungs-URL

Verwenden Sie `AddRedirect`, um Anforderungen umzuleiten. Der erste Parameter enthält Ihren RegEx, damit dieser dem Pfad der eingehenden URL zugeordnet werden kann. Beim zweiten Parameter handelt es sich um eine Ersatzzeichenfolge. Der dritte Parameter gibt, falls vorhanden, den Statuscode an. Wenn Sie den Statuscode nicht angeben, wird standardmäßig „302 – Gefunden“ zurückgegeben, was bedeutet, dass die Ressource kurzzeitig verschoben oder ersetzt wurde.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

Aktivieren Sie in Ihrem Browser die Entwicklertools, und senden Sie eine Anforderung an die Beispiel-App mit dem Pfad `/redirect-rule/1234/5678`. Der RegEx stimmt mit dem Anforderungspfad unter `redirect-rule/(.*)` überein, und der Pfad wird durch `/redirected/1234/5678` ersetzt. Die Umleitungs-URL wird mit dem Statuscode „302 – Gefunden“ an den Client zurückgesendet. Unter der Umleitungs-URL sendet der Browser eine neue Anforderung, die in dessen Adressleiste angezeigt wird. Da keine Regeln in der Beispiel-App für die Umleitungs-URL gelten, wird für die zweite Anforderung die Antwort „200 – OK“ von der App zurückgegeben, und im Antworttext wird die Umleitungs-URL angezeigt. Wenn eine URL *weitergeleitet* wird, wird ein Roundtrip zum Server ausgelöst.

> [!WARNING]
> Seien Sie vorsichtig, wenn Sie die Umleitungsregeln einrichten. Bei jeder Anforderung an die App werden Ihre Umleitungsregeln überprüft – auch nach einer Umleitung. Dabei kann schnell aus Versehen eine Dauerschleife von Umleitungen entstehen.

Ursprüngliche Anforderung: `/redirect-rule/1234/5678`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_redirect.png)

Der Teil des Ausdruck in Klammern wird als *Erfassungsgruppe* bezeichnet. Der Punkt (`.`) im Ausdruck steht für *Übereinstimmung mit beliebigem Zeichen*. Das Sternchen (`*`) steht für *Übereinstimmung mit dem vorausgehenden Zeichen (keinmal oder mindestens einmal)*. Daher werden die letzten beiden Pfadsegmente der URL (`1234/5678`) von der Erfassungsgruppe erfasst `(.*)`. Alle Werte, die Sie in der Anforderungs-URL nach `redirect-rule/` angeben, werden von dieser Erfassungsgruppe erfasst.

Erfassungsgruppen werden in der Ersetzungszeichenfolge mit dem Dollarzeichen (`$`) in die Zeichenfolge eingefügt. Danach folgt die Sequenznummer der Erfassung. Der erste Wert der Erfassungsgruppe wird mit `$1` abgerufen, der zweite mit `$2`. Dies wird in Sequenzen für die Erfassungsgruppen Ihres RegEx weitergeführt. Nur eine Erfassungsgruppe ist in der Beispiel-App im RegEx der Umleitungsregel enthalten. Das bedeutet, dass es in die Ersetzungszeichenfolge nur eine Gruppe eingefügt wird, nämlich `$1`. Wenn die Regel angewendet wird, ändert sich die URL in `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>URL-Umleitung an einen sicheren Endpunkt

Verwenden Sie `AddRedirectToHttps`, um HTTP-Anforderungen auf denselben Host und Pfad mithilfe von HTTPS (`https://`) umzuleiten. Wenn kein Statuscode angegeben wird, wird für die Middleware standardmäßig „302 – Gefunden“ zurückgegeben. Wenn kein Port angegeben wird, wird für die Middleware standardmäßig `null` zurückzugeben. Das heißt, das Protokoll ändert sich in `https://`, und der Client greift auf die Ressource auf Port 443 zu. Im Beispiel wird dargestellt, wie Sie den Statuscode „301 – Permanent verschoben“ festlegen und den Port in 5001 ändern.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Verwenden Sie `AddRedirectToHttpsPermanent`, um unsichere Anforderungen auf denselben Host und Pfad mit einem sicheren HTTPS-Protokoll (`https://` auf Port 443) umzuleiten. Die Middleware legt den Statuscode auf „301 – Permanent verschoben“ fest.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Wenn Sie eine Umleitung auf HTTPS ohne weitere erforderliche Umleitungsregeln durchführen, wird empfohlen, Middleware für HTTPS-Umleitung zu verwenden. Weitere Informationen finden Sie im Artikel zum [Erzwingen von HTTPS](xref:security/enforcing-ssl#require-https).

Anhand der Beispiel-App wird veranschaulicht, wie `AddRedirectToHttps` oder `AddRedirectToHttpsPermanent` verwendet werden sollen. Fügen Sie die Erweiterungsmethode zu den `RewriteOptions` hinzu. Senden Sie eine unsichere Anforderung an die App unter einer beliebigen URL. Schließen Sie die Sicherheitswarnung des Browsers, in der Sie darüber informiert werden, dass das selbstsignierte Zertifikat nicht vertrauenswürdig ist, oder erstellen sie eine Ausnahme, um dem Zertifikat zu vertrauen.

Ursprüngliche Anforderung über `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_redirect_to_https.png)

Ursprüngliche Anforderung über `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Umschreiben einer URL

Erstellen Sie mithilfe von `AddRewrite` eine Regel zum Umschreiben von URLs. Der erste Parameter enthält Ihren RegEx, damit dieser der eingehenden URL zugeordnet werden kann. Beim zweiten Parameter handelt es sich um eine Ersatzzeichenfolge. Der dritte Parameter (`skipRemainingRules: {true|false}`) teilt der Middleware mit, ob sie zusätzliche Umschreibungsregeln überspringen soll, wenn die aktuelle Regel angewendet wird.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

Ursprüngliche Anforderung: `/rewrite-rule/1234/5678`

![Browserfenster mit Entwicklertools, die die Anforderung und Antwort nachverfolgen](url-rewriting/_static/add_rewrite.png)

Im RegEx fällt besonders das Zirkumflexzeichen (`^`) am Anfang des Ausdrucks auf. Das bedeutet, dass die Übereinstimmung schon am Anfang des URL-Pfads beginnt.

Im zuvor genannten Beispiel zur Umleitungsregel (`redirect-rule/(.*)`) gibt es kein Zirkumflexzeichen am Anfang des RegEx. Daher kann jedes beliebige Zeichen vor `redirect-rule/` im Pfad stehen, damit es zu einer Übereinstimmung kommt.

| Pfad                               | Match |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Ja   |
| `/my-cool-redirect-rule/1234/5678` | Ja   |
| `/anotherredirect-rule/1234/5678`  | Ja   |

Die Umschreibungsregel (`^rewrite-rule/(\d+)/(\d+)`) stimmt nur mit Pfaden überein, wenn sie mit `rewrite-rule/` beginnen. Beachten Sie die unterschiedlichen Übereinstimmungen zwischen der Umschreibungsregel und der Umleitungsregel.

| Pfad                              | Match |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Ja   |
| `/my-cool-rewrite-rule/1234/5678` | Nein    |
| `/anotherrewrite-rule/1234/5678`  | Nein    |

Auf den `^rewrite-rule/`-Teil des Ausdruck folgen zwei Erfassungsgruppen: `(\d+)/(\d+)`. `\d` steht für *Übereinstimmung mit einer Ziffer (Zahl)*. Das Pluszeichen (`+`) steht für *match one or more of the preceding character* (Übereinstimmung mit mindestens einem vorausgehenden Zeichen). Aus diesem Grund muss die URL eine Zahl enthalten, auf die ein Schrägstrich und eine weitere Zahl folgt. Die Erfassungsgruppen werden in die umgeschriebene URL als `$1` und `$2` eingefügt. Über die Ersetzungszeichenfolge der Umschreibungsregel werden die Erfassungsgruppen in die Abfragezeichenfolge eingefügt. Der angeforderte `/rewrite-rule/1234/5678`-Pfad wird umgeschrieben, um eine Ressource unter `/rewritten?var1=1234&var2=5678` abzurufen. Wenn es in der ursprünglichen Abfrage eine Abfragezeichenfolge gibt, bleibt sie erhalten, wenn die URL umgeschrieben wird.

Es gibt keinen Roundtrip zum Server, um die Ressource abzurufen. Wenn es die Ressource gibt, wird sie abgerufen und dem Client mit dem Statuscode „200– OK“ zurückgegeben. Da der Client nicht umgeleitet wird, ändert sich die URL in der Adressleiste des Browsers nicht. Für den Client hat der URL-Umschreibungsvorgang nie stattgefunden.

> [!NOTE]
> Verwenden Sie `skipRemainingRules: true` so häufig wie möglich, da das Abgleichen von Regeln sehr umfangreich ist und die Antwortzeit der App verringert. Führen Sie folgende Schritte aus, um die schnellsten App-Antwortzeiten zu erreichen:
> * Sortieren Sie Ihre Umschreibungsregeln von der am häufigsten abgeglichenen Regel zu der am seltensten abgeglichenen Regel.
> * Überspringen Sie die Verarbeitung der übrigen Regeln, wenn es zu einer Übereinstimmung kommt und keine zusätzlichen Regelverarbeitungen erforderlich sind.

### <a name="apache-modrewrite"></a>Apache: „mod_rewrite“

Wenden Sie die Apache-Regeln „mod_rewrite“ mit `AddApacheModRewrite` an. Vergewissern Sie sich, dass die Regeldatei mit der App bereitgestellt wird. Weitere Informationen zu diesen Regeln finden Sie unter [Apache: „mod_rewrite“](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Zum Lesen der Regeldatei *ApacheModRewrite.txt* wird `StreamReader` verwendet.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Der erste Parameter verwendet eine `IFileProvider`-Schnittstelle, die über [Dependency Injection](dependency-injection.md) bereitgestellt wird. `IHostingEnvironment` wird eingefügt, um `ContentRootFileProvider` bereitzustellen. Der zweite Parameter steht für den Pfad Ihrer Regeldatei (*ApacheModRewrite.txt* in der Beispiel-App).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

Die Beispiel-App leitet Anforderungen von `/apache-mod-rules-redirect/(.\*)` auf `/redirected?id=$1` um. Der Antwortstatuscode lautet „302 – Gefunden“.

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Ursprüngliche Anforderung: `/apache-mod-rules-redirect/1234`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten nachverfolgen](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Unterstützte Servervariablen

Die Middleware unterstützt die folgenden Servervariablen für die Apache „mod_rewrite“:

* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Regeln zum IIS-Umschreibungsmodul

Verwenden Sie `AddIISUrlRewrite`, um die Regeln zu verwenden, die für das IIS-Umschreibungsmodul gelten. Vergewissern Sie sich, dass die Regeldatei mit der App bereitgestellt wird. Geben Sie Ihrer Middleware nicht die Anweisung, die *web.config*-Datei zu verwenden, wenn sie auf der Windows Server-IIS ausgeführt wird. Die Regeln sollten mit IIS außerhalb der *web.config*-Datei gespeichert werden, um Konflikte mit dem IIS-Umschreibungsmodul zu vermeiden. Weitere Informationen und Beispiele zu den Regeln zum IIS-Umschreibungsmodul finden Sie unter [Using Url Rewrite Module 2.0 (Verwenden des URL-Umschreibungsmoduls 2.0)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) und [URL Rewrite Module Configuration Reference (Konfigurationsreferenz des URL-Umschreibungsmoduls)](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Zum Lesen der Regeldatei *IISUrlRewrite.xml* wird `StreamReader` verwendet.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Der erste Parameter verwendet eine `IFileProvider`-Schnittstelle, wohingegen es sich bei dem zweiten Parameter um einen Pfad zu einer XML-Regeldatei handelt (*IISUrlRewrite.xml* in der Beispiel-App).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

Die Beispiel-App schreibt Anforderungen von `/iis-rules-rewrite/(.*)` in `/rewritten?id=$1` um. Die Antwort wird an den Client mit dem Statuscode „200 – OK“ gesendet.

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Ursprüngliche Anforderung: `/iis-rules-rewrite/1234`

![Browserfenster mit Entwicklertools, die die Anforderung und Antwort nachverfolgen](url-rewriting/_static/add_iis_url_rewrite.png)

Wenn Sie über ein aktives IIS-Umschreibungsmodul verfügen, für das die Regeln auf Serverebene konfiguriert sind, die negative Auswirkungen auf Ihre App hätten, können Sie das IIS-Umschreibungsmodul für die App deaktivieren. Weitere Informationen finden Sie unter [Disabling IIS modules (Deaktivieren von IIS-Modulen)](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nicht unterstützte Features

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die im Lieferumfang von ASP.NET Core 2.x enthaltene Middleware unterstützt die folgenden Features des IIS-URL-Umschreibungsmoduls nicht:

* Ausgehende Regeln
* Benutzerdefinierte Servervariablen
* Platzhalter
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Die im Lieferumfang von ASP.NET Core 1.x enthaltene Middleware unterstützt die folgenden Features des IIS-URL-Umschreibungsmoduls nicht:

* Globale Regeln
* Ausgehende Regeln
* Umschreibungszuordnungen
* Benutzerdefinierte Antwortaktionen
* Benutzerdefinierte Servervariablen
* Platzhalter
* Action:CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Unterstützte Servervariablen

Die Middleware unterstützt die folgenden Servervariablen für das IIS-URL-Umschreibungsmodul:

* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> Sie können `IFileProvider` auch über `PhysicalFileProvider` abrufen. Über diesen Ansatz können Sie flexibler einen Speicherort für die Dateien der Umschreibungsregeln auswählen. Vergewissern Sie sich, dass die Dateien zu den Umschreibungsregeln auf dem Server unter dem von Ihnen angegebenen Pfad bereitgestellt werden.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Methodenbasierte Regel

Verwenden Sie `Add(Action<RewriteContext> applyRule)`, um Ihre eigene Regellogik in einer Methode zu implementieren. Über `RewriteContext` können Sie `HttpContext` in Ihrer Methode verwenden. `context.Result` bestimmt, wie die zusätzliche Pipelineverarbeitung erfolgt.

| context.Result                       | Aktion                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (Standardwert) | Regeln weiter anwenden                                         |
| `RuleResult.EndResponse`             | Regeln nicht mehr anwenden und Antwort senden                       |
| `RuleResult.SkipRemainingRules`      | Regeln nicht mehr anwenden, und den Kontext an die nächste Middleware senden |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

In der Beispiel-App wird eine Methode dargestellt, die Anforderungen für Pfade weiterleitet, die auf *.xml* enden. Wenn Sie eine Anforderung an `/file.xml` senden, wird diese an `/xmlfiles/file.xml` weitergeleitet. Der Statuscode wird auf „301 – Permanent verschoben“ festgelegt. Sie müssen für eine Umleitung den Statuscode auf eine genaue Antwort festlegen. Ansonsten wird der Statuscode „200 – OK“ zurückgegeben und es wird keine Umleitung zum Client ausgeführt.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Ursprüngliche Anforderung: `/file.xml`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten der file.xml-Datei nachverfolgen](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>IRule-basierte Regel

Verwenden Sie `Add(IRule)`, um Ihre eigene Regellogik in eine Klasse zu implementieren, die von `IRule` abgeleitet wird. Wenn Sie `IRule` verwenden, können Sie flexibler entscheiden, ob Sie eine methodenbasierte Regel verwenden möchten. Ihre abgeleitete Klasse kann an den Stellen einen Konstruktor enthalten, an denen Sie Parameter für die `ApplyRule`-Methode übergeben.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

Die Werte für die Parameter in der Beispiel-App für `extension` und `newPath` werden auf verschiedene Bedingungen geprüft. `extension` muss einen Wert enthalten, der *.png*, *.jpg* oder *.gif* entspricht. Wenn `newPath` nicht gültig ist, wird `ArgumentException` nicht ausgelöst. Wenn Sie eine Anforderung an *image.png* senden, wird diese an `/png-images/image.png` weitergeleitet. Wenn Sie eine Anforderung an *image.jpg* senden, wird diese an `/jpg-images/image.jpg` weitergeleitet. Der Statuscode ist auf „301 – Permanent verschoben“ festgelegt, und `context.Result` erhält die Anweisung, Verarbeitungsregeln nicht mehr anzuwenden und Antworten zu senden.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Ursprüngliche Anforderung: `/image.png`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten der image.png-Datei nachverfolgen](url-rewriting/_static/add_redirect_png_requests.png)

Ursprüngliche Anforderung: `/image.jpg`

![Browserfenster mit Entwicklertools, die die Anforderungen und Antworten der image.jpg-Datei nachverfolgen](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>RegEx-Beispiele

| Ziel | RegEx-Zeichenfolge und<br>übereinstimmendes Beispiel | Ersetzungszeichenfolge und<br>Ausgabebeispiel |
| ---- | :-----------------------------: | :------------------------------------: |
| Umschreiben des Pfads in die Abfragezeichenfolge | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Entfernen des nachgestellten Schrägstrichs | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Erzwingen des nachgestellten Schrägstrichs | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Vermeiden des Umschreibens von bestimmten Anforderungen | `^(.*)(?<!\.axd)$` oder `^(?!.*\.axd$)(.*)$`<br>Ja: `/resource.htm`<br>Nein: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Ändern der Anordnung von URL-Segmenten | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Ersetzen von URL-Segmenten | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Application Startup (Starten von Anwendungen)](startup.md)
* [Middleware](xref:fundamentals/middleware/index)
* [Reguläre Ausdrücke in .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](/dotnet/articles/standard/base-types/quick-ref)
* [Apache: „mod_rewrite“](https://httpd.apache.org/docs/2.4/rewrite/)
* [Using Url Rewrite Module 2.0 (for IIS) (Verwenden des URL-Umschreibungsmoduls 2.0 (für IIS))](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL Rewrite Module Configuration Reference (Konfigurationsreferenz für das URL-Umschreibungsmodul)](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Forum zum IIS-URL-Umschreibungsmodul](https://forums.iis.net/1152.aspx)
* [Einfache Strukturierung von URLs](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 URL Rewriting Tips and Tricks (10 Tipps zum Umschreiben von URL)](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [To slash or not to slash (Schrägstriche setzen oder nicht setzen)](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
