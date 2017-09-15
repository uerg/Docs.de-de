Ersetzen Sie den Inhalt von *Controllers/HelloWorldController.cs* durch Folgendes:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Jede `public`-Methode in einem Controller kann als HTTP-Endpunkt aufgerufen werden. Im obigen Beispiel geben beide Methoden eine Zeichenfolge zurück.  Beachten Sie die Kommentare vor jeder Methode.

Ein HTTP-Endpunkt ist eine Ziel-URL in der Webanwendung, wie z.B. `http://localhost:1234/HelloWorld`, und kombiniert das verwendete Protokoll `HTTP`, die Netzwerkadresse des Webservers (einschließlich TCP-Port) `localhost:1234` und den Ziel-URI `HelloWorld`.

Der erste Kommentar besagt, dass dies eine [HTTP GET](http://www.w3schools.com/tags/ref_httpmethods.asp)-Methode ist, die durch Anfügen von „/HelloWorld/“ an die Basis-URL aufgerufen wird. Der zweite Kommentar gibt eine [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)-Methode an, die durch Anfügen von „/HelloWorld/Welcome/“ an die URL aufgerufen wird. Im weiteren Verlauf des Tutorials nutzen Sie das Gerüstbaumodul zum Generieren von `HTTP POST`-Methoden.

Führen Sie die App im Nicht-Debugmodus aus, und fügen Sie „HelloWorld“ an den Pfad in der Adressleiste an. Die `Index`-Methode gibt eine Zeichenfolge zurück.

![Browserfenster mit der Anwendungsantwort „This is my default action“](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC ruft Controllerklassen (und darin enthaltene Aktionsmethoden) abhängig von der eingehenden URL auf. Die von MVC verwendete standardmäßige [URL-Routinglogik](../../mvc/controllers/routing.md) befolgt ein Format wie dieses, um den aufzurufenden Code zu bestimmen:

`/[Controller]/[ActionName]/[Parameters]`

Sie legen das Format für das Routing in der Datei *Startup.cs* fest.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Wenn Sie die App auszuführen und keine URL-Segmente angeben, werden standardmäßig der Controller „Home“ und die Methode „Index“ verwendet, die in der oben hervorgehobenen Vorlagenzeile angegeben ist.

Das erste URL-Segment bestimmt die auszuführende Controllerklasse. Daher wird `localhost:xxxx/HelloWorld` der `HelloWorldController`-Klasse zugeordnet. Der zweite Teil des URL-Segments bestimmt die Aktionsmethode für die Klasse. Daher bewirkt `localhost:xxxx/HelloWorld/Index` das Ausführen der `Index`-Methode der `HelloWorldController`-Klasse. Beachten Sie, dass Sie nur zu `localhost:xxxx/HelloWorld` navigieren mussten und die `Index`-Methode standardmäßig aufgerufen wurde. Der Grund hierfür ist, dass `Index` die Standardmethode ist, die für einen Controller aufgerufen wird, wenn der Methodenname nicht explizit angegeben wird. Der dritte Teil des URL-Segments (`id`) ist für Routendaten. Routendaten werden im weiteren Verlauf dieses Tutorials behandelt.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge „This is the Welcome action method...“ zurück. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![Browserfenster mit der Anwendungsantwort „This is the Welcome action method“](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Ändern Sie den Code so, dass Parameterinformationen von der URL an den Controller übergeben werden. Beispielsweise `/HelloWorld/Welcome?name=Rick&numtimes=4`. Ändern Sie `Welcome`-Methode so, dass zwei Parameter, wie im folgenden Code gezeigt, einbezogen werden. 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Der vorangehende Code:

* Verwendet das C#-Feature „optional-parameter“, um anzugeben, dass der `numTimes`-Parameter standardmäßig 1 ist, wenn kein anderer Wert für diesen Parameter übergeben wird.
* Verwendet `HtmlEncoder.Default.Encode`, um die App vor schädlicher Eingaben (über JavaScript) zu schützen. 
* Verwendet [interpolierte Zeichenfolgen](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Führen Sie die App aus, und navigieren Sie zu:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Ersetzen Sie xxxx durch Ihre Portnummer.) Sie können für `name` und `numtimes` in der URL verschiedene Werte ausprobieren. Das MVC-[Modellbindungssystem](../../mvc/models/model-binding.md) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge auf der Adressleiste den Parametern der Methode zu. Weitere Informationen finden Sie unter [Modellbindung](../../mvc/models/model-binding.md).

![Browserfenster mit der Anwendungsantwort „Hello Rick, NumTimes is: 4“](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

In der obigen Abbildung wird das URL-Segment (`Parameters`) nicht verwendet, und die Parameter `name` und `numTimes` werden als [Abfragezeichenfolgen](http://en.wikipedia.org/wiki/Query_string) übergeben. Das Fragezeichen (`?`) in der obigen URL ist ein Trennzeichen, auf das die Abfragezeichenfolgen folgen. Das Zeichen `&` trennt Abfragezeichenfolgen.

Ersetzen Sie die `Welcome`-Methode durch folgenden Code:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Führen Sie die App aus, und geben Sie die folgende URL ein: `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Browserfenster mit der Anwendungsantwort „Hello Rick, ID 3“](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Dieses Mal hat das dritte URL-Segment mit dem Routenparameter `id` übereingestimmt. Die `Welcome`-Methode enthält den Parameter `id`, der mit der URL-Vorlage in der `MapRoute`-Methode übereinstimmt. Das nachfolgende `?` (in `id?`) gibt an, dass der Parameter `id` optional ist.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

In diesen Beispielen hat der Controller den „VC“-Teil von MVC übernommen, d.h. die Aufgaben von „View“ (Ansicht) und „Controller“. Der Controller gibt HTML direkt zurück. Im Allgemeinen sollen Controller HTML nicht direkt zurückgeben, da dies sehr aufwändig zu programmieren und pflegen ist. Stattdessen verwenden Sie in der Regel eine separate Razor-Ansichtsvorlagendatei, um die HTML-Antwort zu generieren. Dies lernen Sie im nächsten Tutorial.
