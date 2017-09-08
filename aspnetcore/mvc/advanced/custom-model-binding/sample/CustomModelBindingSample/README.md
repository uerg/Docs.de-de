# <a name="custom-model-binding-demo"></a>Benutzerdefinierte Modell Bindung Demo

Sie können testen, die `ByteArrayModelBinder` durch Ausführen der Anwendung und der Bereitstellung einer base64-codierte Zeichenfolge an den Endpunkt ImageController (/ api/Images /). Sie sollten die Datei und den Dateinamen Proparties angeben, in der Anforderung als Formulardaten Text (mit Postman oder ein ähnliches Tool). Sie können [dieser Beispielzeichenfolge](Base64String.txt). Das Ergebnis wird im Ordner "Wwwroot" / Images/Upload mit dem Dateinamen gespeichert werden, die Sie angegeben haben.

Um das Beispiel für die benutzerdefinierte Bindung zu testen, versuchen Sie die folgenden Endpunkte: /api/authors/1 /api/authors/2 (nicht gefunden) /api/boundauthors/1 /api/boundauthors/2 (nicht gefunden) /api/boundauthors/get/1 /api/boundauthors/get/2 (kein Inhalt) – diese Aktion nicht für überprüfen NULL und zurückgeben, Nichtgefunden
