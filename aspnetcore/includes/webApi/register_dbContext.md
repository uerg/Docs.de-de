## <a name="register-the-database-context"></a>Registrieren des Datenbankkontexts

Um den Datenbankkontext in den Controller einzufügen, müssen wir ihn beim Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) registrieren. Registrieren Sie den Datenbankkontext mithilfe der integrierten Unterstützung der [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) beim Dienstcontainer. Ersetzen Sie den Inhalt der Datei *Startup.cs* durch Folgendes:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Der vorangehende Code:

* Entfernt den Code, der nicht verwendet wird.
* Gibt an, dass eine In-Memory Database in den Dienstcontainer eingefügt wird.
