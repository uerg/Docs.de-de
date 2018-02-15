# <a name="custom-model-binding-demo"></a>Demo zur benutzerdefinierten Modellbindung

Sie können den `ByteArrayModelBinder` testen, indem Sie die Anwendung ausführen und eine Base64-codierte Zeichenfolge für den ImageController-Endpunkt bereitstellen (/api/image/). Sie sollten die Datei- und Dateinameneigenschaften im Anforderungstext als Formulardaten angeben. Verwenden Sie dafür z.B. Postman oder ein ähnliches Tool. Sie können diese [Beispielzeichenfolge](Base64String.txt) verwenden. Das Ergebnis wird dann mit dem von Ihnen angegebenen Dateinamen im Ordner „wwwroot/images/upload“ eingefügt.

Überprüfen Sie die folgenden Endpunkte, um das Beispiel für die benutzerdefinierte Bindung zu testen: „/api/authors/1 /api/authors/2“ (NICHT GEFUNDEN) „/api/boundauthors/1 /api/boundauthors/2“ (NICHT GEFUNDEN) „/api/boundauthors/get/1 /api/boundauthors/get/2“ (KEIN INHALT) – diese Aktion überprüft nicht auf NULL und gibt nicht die Meldung „Nicht gefunden“ zurück.
