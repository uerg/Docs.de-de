# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Beispiel für das Umschreiben der URL in ASP.NET Core (ASP.NET Core 2.x)

Dieses Beispiel veranschaulicht die Verwendung der Middleware für das Umschreiben der URL von ASP.NET Core 2.x. In der Anwendung werden die Optionen zum Umleiten und Umschreiben von URLs veranschaulicht. Informationen zum Beispiel für ASP.NET Core 1.x finden Sie unter [Beispiel zum Umschreiben der URL von ASP.NET Core (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x).

Beim Ausführen des Beispiels wird eine Antwort mit der umgeschriebenen oder umgeleiteten URL angezeigt, wenn eine der Regeln auf eine Anforderungs-URL angewendet wird.

## <a name="examples-in-this-sample"></a>Beispiele

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Statuscode für Erfolg: „302 – Gefunden“
  - Beispiel (Umleitung): **/redirect-rule/{capture_group}** zu **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Statuscode für Erfolg: „200 – OK“
  - Beispiel (Umschreibung): **/rewrite-rule/{capture_group_1}/{capture_group_2}** in **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Statuscode für Erfolg: „302 – Gefunden“
  - Beispiel (Umleitung): **/apache-mod-rules-redirect/{capture_group}** zu **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Statuscode für Erfolg: „200 – OK“
  - Beispiel (Umschreibung): **/iis-rules-rewrite/{capture_group}** in **/rewritten?id={capture_group}**
* `Add(RedirectXMLRequests)`
  - Statuscode für Erfolg: „301 – Permanent verschoben“
  - Beispiel (Umleitung): **/file.xml** zu **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Statuscode für Erfolg: „301 – Permanent verschoben“
  - Beispiel (Umleitung): **/image.png** zu **/png-images/image.png**
  - Beispiel (Umleitung): **/image.jpg** zu **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Verwenden eines `PhysicalFileProvider`
Sie können auch ein `IFileProvider`-Objekt abrufen, indem Sie ein `PhysicalFileProvider`-Objekt erstellen, das an die Methoden `AddApacheModRewrite()` und `AddIISUrlRewrite()` übergeben wird:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Sichere Umleitungserweiterungen
Dieses Beispiel enthält eine `WebHostBuilder`-Konfiguration für die App zur Verwendung von URLs (**https://localhost:5001**, **https://localhost**) und ein Testzertifikat (**testCert.pfx**) als Unterstützung beim Erkunden dieser Umleitungsmethoden. Fügen Sie diese zum `RewriteOptions()`-Konstruktor in **Startup.cs** hinzu, um deren Verhalten zu untersuchen.

Methode | Statuscode | Port
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | NULL (465)
`.AddRedirectToHttps()` | 302 | NULL (465)
`.AddRedirectToHttps(301)` | 301 | NULL (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
