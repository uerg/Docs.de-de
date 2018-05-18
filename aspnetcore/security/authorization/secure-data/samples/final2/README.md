# <a name="how-to-buildrun-secure-user-data-sample"></a>Build/sichere Benutzer Datenbeispiel Ausführung

* Legen Sie das Kennwort, mit dem geheimen Schlüssel-Manager-Tool:

  `dotnet user-secrets set SeedUserPW <pw>`

* Die Datenbank zu aktualisieren:

    `dotnet ef database update`

* Aktivieren Sie SSL im Projekt