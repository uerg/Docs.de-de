# <a name="how-to-buildrun-secure-user-data-sample"></a>Wie Sie Build/Datenbeispiel sichere Benutzer ausführen.

* Legen Sie das Kennwort, mit dem Geheimnis-Manager-Tool:

  `dotnet user-secrets set SeedUserPW <pw>`

* Die Datenbank zu aktualisieren:

    `dotnet ef database update`

* Aktivieren Sie SSL im Projekt
