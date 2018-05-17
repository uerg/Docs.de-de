## <a name="implement-the-other-crud-operations"></a>Implementieren der anderen CRUD-Vorgänge

In den folgenden Abschnitten werden dem Controller `Create`-, `Update`- und `Delete`-Methoden hinzugefügt.

### <a name="create"></a>Erstellen

Fügen Sie die folgende `Create`-Methode hinzu:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Der vorherige Code ist eine HTTP POST-Methode, die durch das Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird. Das [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Der vorherige Code ist eine HTTP POST-Methode, die durch das Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird. MVC ruft den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung ab.
::: moniker-end

Die `CreatedAtRoute`-Methode:

* Gibt eine 201-Antwort zurück. HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.
* Fügt der Antwort einen Adressheader hinzu. Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück. Siehe [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Verwendet die „GetTodo“ genannte Route, um die URL zu erstellen. Die Route mit dem Namen „GetTodo“ wird in `GetById` definiert:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Verwenden von Postman zum Senden einer Erstellungsanforderung

* Starten Sie die Anwendung.
* Öffnen Sie Postman.

![Postman-Konsole](../../tutorials/first-web-api/_static/pmc.png)

* Aktualisieren Sie die Portnummer in der localhost-URL.
* Legen Sie die HTTP-Methode auf *POST* fest.
* Klicken Sie auf die Registerkarte **Body** (Text).
* Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.
* Legen Sie den Typ auf *JSON (application/json)* fest.
* Geben Sie einen Anforderungstext mit einem To-Do-Element ein, der folgendem JSON-Code ähnelt:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Klicken Sie auf die Schaltfläche **Senden**.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Wenn nach dem Klicken auf **Senden** keine Antwort angezeigt wird, deaktivieren Sie die Option **SSL certification verification** (Überprüfung der SSL-Zertifizierung). Diese finden Sie unter **Datei** > **Einstellungen**. Klicken Sie erneut auf die Schaltfläche **Senden**, nachdem Sie diese Einstellung deaktiviert haben.
::: moniker-end

Klicken Sie auf die Registerkarte **Header** im Bereich **Antwort**, und kopieren Sie den Headerwert von **Location** (Speicherort):

![Registerkarte „Header“ in der Postman-Konsole](../../tutorials/first-web-api/_static/pmc2.png)

Der Adressheader-URI kann verwendet werden, um auf das neue Element zuzugreifen.

### <a name="update"></a>Update

Fügen Sie die folgende `Update`-Methode hinzu:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` ähnelt `Create`, verwendet allerdings HTTP PUT. Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität (nicht nur die Deltas) sendet. Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.

Verwenden Sie Postman, um den Namen des To-do-Elements in „walk cat“ zu ändern:

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Löschen

Fügen Sie die folgende `Delete`-Methode hinzu:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Die `Delete`-Antwort lautet [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Verwenden Sie Postman, um das To-do-Element zu löschen:

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
