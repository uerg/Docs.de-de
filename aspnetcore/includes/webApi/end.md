## <a name="implement-the-other-crud-operations"></a>Implementieren der anderen CRUD-Vorgänge

In den folgenden Abschnitten werden dem Controller `Create`-, `Update`- und `Delete`-Methoden hinzugefügt.

### <a name="create"></a>Erstellen

Fügen Sie die folgende `Create`-Methode hinzu.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Der Code oben ist eine HTTP POST-Methode, die durch das [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute)-Attribut angegeben wird. Das [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.

Die `CreatedAtRoute`-Methode:

* Gibt eine 201-Antwort zurück. HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.
* Fügt der Antwort einen Adressheader hinzu. Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück. Siehe [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Verwendet die „GetTodo“ genannte Route, um die URL zu erstellen. Die Route mit dem Namen „GetTodo“ wird in `GetById` definiert:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a>Verwenden von Postman zum Senden einer Erstellungsanforderung

![Postman-Konsole](../../tutorials/first-web-api/_static/pmc.png)

* Legen Sie die HTTP-Methode auf `POST` fest.
* Wählen Sie das Optionsfeld **Body** aus.
* Wählen Sie das Optionsfeld **raw** aus.
* Legen Sie den Typ auf „JSON“ fest.
* Geben Sie im Schlüsselwert-Editor zum Beispiel folgendes To-Do-Element ein

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Klicken Sie auf **Send**.
* Klicken Sie auf die Registerkarte „Header“ im unteren Bereich, und kopieren Sie den Header **Location**:

![Registerkarte „Header“ in der Postman-Konsole](../../tutorials/first-web-api/_static/pmget.png)

Der Adressheader-URI kann verwendet werden, um auf das neue Element zuzugreifen.

### <a name="update"></a>Aktualisieren

Fügen Sie die folgende `Update`-Methode hinzu:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` ähnelt `Create`, verwendet aber HTTP PUT. Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet. Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Löschen

Fügen Sie die folgende `Delete`-Methode hinzu:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Die `Delete`-Antwort lautet [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Test `Delete`: 

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
