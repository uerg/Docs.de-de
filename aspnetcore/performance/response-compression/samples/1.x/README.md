# <a name="response-compression-sample-application-aspnet-core-1x"></a>Antwort Komprimierung-beispielanwendung (ASP.NET Core 1.x)

Dieses Beispiel veranschaulicht die Verwendung von ASP.NET Core 1.x Komprimierung-Middleware zum Komprimieren von HTTP-Antworten für die Antwort. Im Beispiel veranschaulicht Gzip und benutzerdefinierte Komprimierung-Anbietern für Text- und Image-Antworten und zeigt, wie einen MIME-Typ für die Komprimierung hinzuzufügen. Das ASP.NET Core 2.x-Beispiel finden Sie unter [Antwort Komprimierung-beispielanwendung (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Die Beispiele in diesem Beispiel
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Lorem Ipsum Datei Textantwort auf 2,044 Bytes, die auf 927 Bytes komprimiert werden
    * **/testfile1kb.txt** -Datei Textantwort auf 1,033 Bytes, die auf 47 Bytes komprimiert werden
    * **/ Durchlassen** -Antwort, die als einzelne Zeichen ausgegeben wird, in Intervallen von 1 Sekunden 
  * `image/svg+xml`
    * **/Banner.SVG** -Image-Antwort 9,707 Bytes, die auf 4,459 Bytes komprimiert wird ein Scalable Vector Graphics (SVG)
* `CustomCompressionProvider`<br>Zeigt, wie einen benutzerdefinierte Komprimierung-Anbieter für die Verwendung mit der Middleware implementiert werden.

Wenn die Anforderung enthält die `Accept-Encoding` -Header, die im Beispiel fügt eine `Vary: Accept-Encoding` Header in die Antwort. Die `Vary` Header weist Caches mehrere Kopien der Antwort basierend auf alternativen Werten verwalten `Accept-Encoding`, sodass eine komprimierte (Gzip) und die nicht komprimierte Version Caches gespeichert werden, für Systeme, auf denen können entweder die komprimierten akzeptieren oder die nicht komprimierte Antwort.

## <a name="using-the-sample"></a>Verwenden des Beispiels
1. Stellen Sie eine Anforderung mit [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), oder [Postman](https://www.getpostman.com/) auf die Anwendung ohne ein `Accept-Encoding` Header, und beachten Sie die antwortnutzlast Antwortgröße, und die Antwortheader.
2. Hinzufügen einer `Accept-Encoding: gzip` Header und beachten Sie die Größe der komprimierten Antwort und die Antwortheader. Sie sehen die Antwortgröße löschen, und die `Content-Encoding: gzip` Antwortheader wird von der Beispiel-app eingeschlossen. Wenn Sie den Antworttext für die Lorem Ipsum betrachten oder **testfile1kb.txt** Antwort, sehen Sie, dass der Text, komprimierte und nicht lesbar ist.
3. Hinzufügen einer `Accept-Encoding: mycustomcompression` Header und notieren Sie sich die Antwortheader. Die `CustomCompressionProvider` ist eine leere Implementierung, die tatsächlich die Antwort komprimieren nicht, aber Sie können einen benutzerdefinierten Komprimierung Stream-Wrapper für erstellen die `CreateStream()` Methode.
