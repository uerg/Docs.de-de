# <a name="custom-model-binding-demo"></a>Demo zur benutzerdefinierten Modellbindung

Testen Sie `ByteArrayModelBinder`, indem Sie die App ausführen und eine Base64-codierte Zeichenfolge an den `ImageController`-Endpunkt (`/api/image/`) übergeben. Geben Sie die Datei- und Dateinameneigenschaften im Anforderungstext als Formulardaten an. Verwenden Sie dafür z.B. [Postman](https://www.getpostman.com/) oder ein ähnliches Tool. Sie können diese [Beispielzeichenfolge](Base64String.txt) verwenden. Das Ergebnis wird dann mit dem angegebenen Dateinamen im Ordner *wwwroot/images/upload* gespeichert.

Probieren Sie folgende Endpunkte aus, um das Beispiel für die benutzerdefinierte Bindung zu testen:

* /api/authors/1
* /api/authors/2 (NICHT GEFUNDEN)
* /api/boundauthors/1
* /api/boundauthors/2 (NICHT GEFUNDEN)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (KEIN INHALT): Diese Aktion überprüft nicht auf NULL-Werte und gibt *404 Not Found* (404 – Nicht gefunden) zurück.
