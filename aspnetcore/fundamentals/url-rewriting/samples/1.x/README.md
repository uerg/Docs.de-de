# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>ASP.NET Core URL umschreiben Beispiel (ASP.NET Core 1.x)

Dieses Beispiel veranschaulicht die Verwendung von ASP.NET Core 1.x URL umschreiben Middleware. Die Anwendung zeigt die URL-Umleitung und URL-Optionen umschreiben. Das ASP.NET Core 2.x-Beispiel finden Sie unter [ASP.NET Core URL umschreiben-Beispiel (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

Beim Ausführen des Beispiels wird eine Antwort verarbeitet werden, die die umgeschriebene oder umgeleitet erfolgen URL zeigt, wenn eine der Regeln auf eine Anforderungs-URL angewendet wird.

## <a name="examples-in-this-sample"></a>Die Beispiele in diesem Beispiel

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - Erfolgsstatuscode: 302 (gefunden)
  - Beispiel (Umleitung): **/redirect-rule / {Capture_group}** auf **/redirected/ {Capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Erfolgsstatuscode: 200 (OK)
  - Beispiel (Schreiben): **/rewrite-rule / {capture_group_1} / {capture_group_2}** auf **/ umgeschrieben? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Erfolgsstatuscode: 302 (gefunden)
  - Beispiel (Umleitung): **/apache-mod-rules-redirect / {Capture_group}** auf **/ umgeleitet? Id = {Capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Erfolgsstatuscode: 200 (OK)
  - Beispiel (Schreiben): **/iis-rules-rewrite / {Capture_group}** auf **/ umgeschrieben? Id = {Capture_group}**
* `Add(RedirectXMLRequests)`
  - Erfolgsstatuscode: 301 (Permanent verschoben)
  - Beispiel (Umleitung): **/file.xml** auf **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Erfolgsstatuscode: 301 (Permanent verschoben)
  - Beispiel (Umleitung): **/image.png** auf **/png-images/image.png**
  - Beispiel (Umleitung): **/image.jpg** auf **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Mit einer`PhysicalFileProvider`
Erhalten Sie eine `IFileProvider` durch das Erstellen einer `PhysicalFileProvider` übergeben der `AddApacheModRewrite()` und `AddIISUrlRewrite()` Methoden:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Sichere Umleitung-Erweiterungen
Dieses Beispiel enthält `WebHostBuilder` Konfiguration für die app-URLs verwenden (**Https://localhost:5001**, **https://localhost**) und ein Testzertifikat (**testCert.pfx**), die helfen, die Sie durchsuchen diese umleiten Methoden. Fügen Sie an der `RewriteOptions()` in **Startup.cs** um ihr Verhalten zu untersuchen.

Methode | Statuscode | Port
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | Null (465)
`.AddRedirectToHttps()` | 302 | Null (465)
`.AddRedirectToHttps(301)` | 301 | Null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
