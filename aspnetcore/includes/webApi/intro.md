## <a name="overview"></a>Übersicht

In diesem Tutorial wird die folgende API erstellt:

|API | description | Anforderungstext | Antworttext |
|--- | ---- | ---- | ---- |
|GET /api/todo | Alle To-do-Elemente abrufen | Keiner | Array von To-do-Elementen|
|GET /api/todo/{id} | Ein Element nach ID abrufen | Keiner | To-do-Element|
|POST /api/todo | Neues Element hinzufügen | To-do-Element | To-do-Element |
|PUT /api/todo/{id} | Vorhandenes Element aktualisieren &nbsp; | To-do-Element | Keiner |
|DELETE /api/todo/{id} &nbsp; &nbsp; | Löschen eines Elements &nbsp; &nbsp; | Keiner | Keiner|

<br>

Das folgende Diagramm zeigt den Grundentwurf der App.

![Der Client wird von einem Feld auf der linken Seite dargestellt. Er sendet eine Anforderung und erhält von der Anwendung (Feld auf der rechten Seite) eine Antwort. Im Anwendungsfeld stellen drei Felder den Controller, das Modell und die Datenzugriffsschicht dar. Die Anforderung geht im Controller der Anwendung ein, und Lese-/Schreibvorgänge erfolgen zwischen Controller und Datenzugriffsschicht. Das Modell wird serialisiert und in der Antwort an den Client zurückgegeben.](../../tutorials/first-web-api/_static/architecture.png)

* Der Client ist das Medium, das die Web-API nutzt (mobile App, Browser usw.). In diesem Tutorial wird kein Client erstellt. [Postman](https://www.getpostman.com/) oder [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) wird als Client verwendet, um die App zu testen.

* Ein *Modell* ist ein Objekt, das die Daten in der App darstellt. In diesem Fall ist das einzige Modell ein To-do-Element. Modelle werden als C#-Klassen dargestellt, die auch als **P**lain **O**ld **C**# **O**bject (POCOs) bezeichnet werden.

* Ein *Controller* ist ein Objekt, das HTTP-Anforderungen verarbeitet und die HTTP-Antwort erstellt. Diese App hat einen einzelnen Controller.

* Die App verwendet keine beständige Datenbank, um das Tutorial einfach zu halten. Die Beispiel-App speichert To-do-Elemente in einer In-Memory Database.
