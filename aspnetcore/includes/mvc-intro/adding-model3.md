
## <a name="test-the-app"></a>Testen der App

* Führen Sie die App aus, und tippen Sie auf den Link **Mvc Movie**.
* Tippen Sie auf den Link **Neu erstellen**, und erstellen Sie einen Film.

  ![Erstellen Sie die Ansicht mit den Feldern für ein Genre, den Preis, das Veröffentlichungsdatum und den Titel.](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* Sie können unter Umständen in das Feld `Price` keine Dezimaltrennzeichen oder Kommas eingeben. Zur Unterstützung der [jQuery-Validierung](https://jqueryvalidation.org/) in nicht englischen Gebietsschemas, in denen ein Komma („,“) als Dezimaltrennzeichen verwendet wird, und Nicht-US-englische Datums- und Uhrzeitformate, müssen Sie Schritte zur Globalisierung Ihrer App ausführen. Weitere Informationen finden Sie unter https://github.com/aspnet/Docs/issues/4076 und [Zusätzliche Ressourcen](#additional-resources). Geben Sie einstweilen ganze Zahlen wie 10 ein.

<a name="displayformatdatelocal"></a>

* In manchen Gebietsschemas müssen Sie das Datumsformat angeben. Betrachten Sie den unten markierten Code.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Mit `DataAnnotations` beschäftigen wir uns später im Tutorial.

Wenn Sie auf **Erstellen** tippen, wird das Formular an den Server bereitgestellt, auf dem die Filminformationen in einer Datenbank gespeichert werden. Die App leitet an die URL */Movies* weiter, auf der die neu erstellten Filminformationen angezeigt werden.

![Filmansicht mit neu erstellen Filmeinträgen](../../tutorials/first-mvc-app/adding-model/_static/h.png)

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.
