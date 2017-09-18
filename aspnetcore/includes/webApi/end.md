## <a name="implement-the-other-crud-operations"></a>Implementieren der anderen CRUD-Vorgänge

Fügen Sie Methoden `Create`, `Update` und `Delete` zum Controller hinzu. Dabei handelt sich um Variationen eines Designs, darum wird der Code mit den hervorgehobenen Hauptunterschieden dargestellt. Erstellen Sie das Projekt, nachdem Sie Code hinzugefügt oder geändert haben.

### <a name="create"></a>Erstellen

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Dies ist eine HTTP-POST-Methode, die vom [`[HttpPost]`](https://docs.microsoft.com/aspnet/core/api)-Attribut angegeben wird. Das [`[FromBody]`](https://docs.microsoft.com/aspnet/core/api)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.

Die `CreatedAtRoute`-Methode gibt die Antwort „201“ zurück, bei der es sich um die Standardantwort für eine HTTP-POST-Methode handelt, die eine neue Ressource auf dem Server erstellt. `CreatedAtRoute` fügt der Antwort außerdem einen Adressheader hinzu. Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück. Siehe [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

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

Sie können den Adressheader-URI verwenden, um auf die Ressource zuzugreifen, die Sie gerade erstellt haben. Denken Sie daran, dass die `GetById`-Methode die benannte Route `"GetTodo"` erstellt hat:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Aktualisieren

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` ähnelt `Create`, verwendet aber HTTP PUT. Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet. Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Löschen

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
