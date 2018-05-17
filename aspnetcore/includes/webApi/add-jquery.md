## <a name="call-the-web-api-with-jquery"></a>Aufrufen der Web-API mit jQuery

In diesem Abschnitt wird eine HTML-Seite hinzugefügt, die jQuery verwendet, um die Web-API aufzurufen. jQuery initiiert die Anforderung und aktualisiert die Seite mit den Informationen aus der Antwort der API.

Konfigurieren Sie das Projekt dafür, statische Dateien zu unterstützen und die Standardzuordnung von Dateien zu aktivieren. Dies wird erreicht, indem die Erweiterungsmethoden [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure* aufgerufen werden. Weitere Informationen finden Sie im Artikel zu [statischen Dateien](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Fügen Sie eine HTML-Datei namens *index.html* zum Verzeichnis *wwwroot* des Projekts hinzu. Ersetzen Sie den Inhalt durch folgendes Markup:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Fügen Sie eine JavaScript-Datei namens *site.js* zum Verzeichnis *wwwroot* des Projekts hinzu. Ersetzen Sie den Inhalt durch folgenden Code:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Möglicherweise ist eine Änderung an den Starteinstellungen des ASP.NET Core-Projekts erforderlich, um die HTML-Seite lokal zu testen. Öffnen Sie *launchSettings.json* im Verzeichnis *Eigenschaften* des Projekts. Entfernen Sie die `launchUrl`-Eigenschaft, um zu erzwingen, dass die App mit *index.html* als Startseite geöffnet wird. Dies ist die Standarddatei des Projekts.

Es gibt mehrere Möglichkeiten, um jQuery herunterzuladen. Im vorherigen Codeausschnitt wurde die Bibliothek aus einem Content Delivery Network (CDN) geladen. Dieses Beispiel stellt ein vollständiges CRUD-Beispiel für das Aufrufen der API mit jQuery dar. In diesem Beispiel sind zusätzliche nützliche Features enthalten. Im Folgenden finden Sie Erklärungen zu API-Aufrufen.

### <a name="get-a-list-of-to-do-items"></a>Abrufen einer Liste von To-Do-Elementen

Senden Sie eine HTTP GET-Anforderung an */api/todo*, um eine Liste von To-Do-Elementen abzurufen.

Die jQuery-Funktion [ajax](https://api.jquery.com/jquery.ajax/) sendet eine AJAX-Anforderung an die API, die JSON-Code zurückgibt, der ein Objekt oder Array darstellt. Diese Funktion kann alle HTTP-Interaktionen verarbeiten, indem eine HTTP-Anforderung an die angegebene URL (`url`) gesendet wird. `GET` wird als `type` verwendet. Die Rückruffunktion `success` wird aufgerufen, wenn die Anforderung erfolgreich ist. Im Rückruf wird DOM mit den To-Do-Informationen aktualisiert.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Hinzufügen eines To-Do-Elements

Senden Sie eine HTTP POST-Anforderung an */api/todo/*, um ein To-Do-Element hinzuzufügen. Der Anforderungstext sollte ein To-Do-Objekt enthalten. Die Funktion [ajax](https://api.jquery.com/jquery.ajax/) verwendet `POST`, um die API aufzurufen. Bei `POST`- und `PUT`-Anforderungen stellt der Anforderungstext die Daten dar, die an die API gesendet werden. Die API erwartet einen Anforderungstext im JSON-Format. Die Optionen `accepts` und `contentType` werden auf `application/json` festgelegt, um den gesendeten und empfangenen Medientyp entsprechend zu klassifizieren. Die Daten werden mithilfe von [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) in ein JSON-Objekt konvertiert. Wenn die API den Statuscode „Erfolgreich“ zurückgibt, wird die Funktion `getData` aufgerufen, um die HTML-Tabelle zu aktualisieren.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Aktualisieren eines To-Do-Elements

Das Aktualisieren eines To-Do-Elements ähnelt dem Hinzufügen eines To-Do-Elements, da beide Vorgänge auf einem Anforderungstext basieren. Der einzige Unterschied zwischen diesen Vorgängen besteht darin, dass `url` geändert wird, um den eindeutigen Bezeichner des Elements hinzuzufügen, und `type` ist `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Löschen eines To-Do-Elements

Das Löschen eines To-Do-Elements wird durchgeführt, indem `type` im AJAX-Aufruf auf `DELETE` festgelegt und der eindeutige Bezeichner des Elements in der URL angegeben wird.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
