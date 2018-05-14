## <a name="register-the-database-context"></a>Registrieren des Datenbankkontexts

In diesem Schritt wird der Datenbankkontext beim Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) registriert. Dienste (z. B. der Datenbankkontext), die beim Abhängigkeitsinjektionscontainer (DI) registriert sind, sind für Controller verfügbar.

Registrieren Sie den Datenbankkontext mithilfe der integrierten Unterstützung der [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) beim Dienstcontainer. Ersetzen Sie den Inhalt der Datei *Startup.cs* durch den folgenden Code:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Der vorangehende Code:

* Entfernt den Code, der nicht verwendet wird.
* Gibt an, dass eine In-Memory Database in den Dienstcontainer eingefügt wird.