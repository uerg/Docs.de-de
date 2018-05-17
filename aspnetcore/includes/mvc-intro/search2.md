<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Die vorherige `Index`-Methode:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Die aktualisierte `Index`-Methode mit `id`-Parameter:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Sie können nun den Suchtitel als Routendaten (ein URL-Segment) anstatt als Wert einer Abfragezeichenfolge übergeben.

![Indexansicht mit dem der URL hinzugefügten Wort „ghost“ und einer zurückgegebenen Filmliste mit zwei Filmen: Ghostbusters und Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

Sie können jedoch von den Benutzern nicht erwarten, dass sie jedes Mal die URL ändern, wenn sie nach einem Film suchen möchten. Deshalb fügen Sie nun Benutzeroberflächenelemente zum besseren Filtern von Filmen hinzu. Wenn Sie die Signatur der `Index`-Methode geändert haben, um das Übergeben des routengebundenen Parameters `ID` zu testen, ändern Sie sie erneut so, dass sie einen Parameter namens `searchString` verwendet:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Öffnen Sie die Datei *Views/Movies/Index.cshtml*, und fügen Sie das hervorgehobene `<form>`-Markup hinzu:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

Das HTML-Tag `<form>` nutzt das [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms). Wenn Sie nun das Formular senden, wird die Filterzeichenfolge an die Aktion `Index` des „movies“-Controllers übermittelt. Speichern Sie Ihre Änderungen, und testen Sie dann den Filter.

![Indexansicht mit dem in das Filtertextfeld eingegebenen Wort „ghost“](../../tutorials/first-mvc-app/search/_static/filter.png)

Entgegen Ihrer Erwartung gibt es keine `[HttpPost]`-Überladung der `Index`-Methode. Diese ist nicht erforderlich, da die Methode nicht den Status der App ändert, sondern bloß Daten filtert.

Sie können die folgende `[HttpPost] Index`-Methode hinzufügen.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

Der Parameter `notUsed` dient zum Erstellen einer Überladung für die `Index`-Methode. Damit beschäftigen wir uns später im Tutorial.

Wenn Sie diese Methode hinzufügen, entspricht die aufrufende Aktionsinstanz der `[HttpPost] Index`-Methode, und die `[HttpPost] Index`-Methode wird wie in der folgenden Abbildung gezeigt ausgeführt.

![Browserfenster mit der Antwort der Anwendung von „Von HttpPost-Index“: Filter für „ghost“](../../tutorials/first-mvc-app/search/_static/fo.png)

Doch selbst wenn Sie diese `[HttpPost]`-Version der `Index`-Methode hinzufügen, gibt es eine Einschränkung für die gesamte Implementierung. Stellen Sie sich vor, Sie möchten eine bestimmte Suche als Favorit speichern oder einen Link an Freunde senden, auf den diese klicken können, um dieselbe gefilterte Liste von Filmen anzuzeigen. Beachten Sie, dass die URL der HTTP POST-Anforderung identisch mit der URL der GET-Anforderung (localhost:xxxxx/Filme/Index) ist. Es sind keine Suchinformationen in der URL vorhanden. Die Informationen in der Suchzeichenfolge werden an den Server als [Formularfeldwert](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data) gesendet. Sie können dies mit den Entwicklertools für den Browser oder dem exzellenten Tool [Fiddler](http://www.telerik.com/fiddler) überprüfen. Die folgende Abbildung zeigt die Entwicklertools für den Browser Chrome:

![Registerkarte „Netzwerk“ der Entwicklertools in Microsoft Edge mit einem Anforderungstext mit dem „searchString“-Wert „ghost“](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

Sie können den Suchparameter und das [XSRF](xref:security/anti-request-forgery)-Token im Anforderungstext erkennen. Wie im vorherigen Tutorial erwähnt, generiert das [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms) ein [XSRF](xref:security/anti-request-forgery)-Fälschungssicherheitstoken. Da wir keine Daten ändern, müssen wir nicht das Token in der Controllermethode validieren.

Da sich der Suchparameter im Anforderungstext und nicht in der URL befindet, können Sie diese Suchinformationen nicht als Favorit speichern oder mit anderen teilen. Wir beheben dies durch Angeben der Anforderung als `HTTP GET`.
