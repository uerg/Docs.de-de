# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Hinzufügen einer Ansicht zu einer ASP.NET Core MVC-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Abschnitt ändern Sie die `HelloWorldController`-Klasse so, dass Vorlagendateien für Razor-Ansichten verwendet werden, um den Prozess des Generierens von HTML-Antworten für einen Client sauber zu kapseln.

Sie erstellen eine Ansichtsvorlagendatei mithilfe von Razor. Razor-basierte Ansichtsvorlagen haben die Dateinamenerweiterung *.cshtml*. Sie bieten eine elegante Möglichkeit zum Erstellen einer HTML-Ausgabe mithilfe von C#.

Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ersetzen Sie in der `HelloWorldController`-Klasse die `Index`-Methode durch den folgenden Code:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Der vorangehende Code gibt ein `View`-Objekt zurück. Er verwendet eine Ansichtsvorlage zum Generieren einer HTML-Antwort an den Browser. Controllermethoden (auch Aktionsmethoden genannt), wie z. B. die `Index`-Methode oben, geben in der Regel ein [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) (oder eine von `ActionResult` abgeleitete Klasse) und keinen Typ wie „string“ zurück.
