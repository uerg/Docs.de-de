Fügen Sie der Klasse `Movie` die folgenden Eigenschaften hinzu:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Hinzufügen einer Datenbankkontext-Klasse

Fügen Sie dem Ordner *Modelle* eine von `DbContext` abgeleitete Klasse namens *MovieContext.cs* hinzu.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs)]

Der vorangehende Code erstellt eine `DbSet`-Eigenschaft für die Entitätenmenge. In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle, und eine Entität entspricht einer Zeile in einer Tabelle.