Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Abschnitt fügen Sie die Klassen für die Verwaltung von Filmen in einer Datenbank hinzu. Verwenden Sie diese Klassen mit [Entity Framework Core](/ef/core) (EF Core), um mit einer Datenbank zu arbeiten. EF Core ist ein ORM-Framework (Objektrelationales Mapping, ORM), das den Datenzugriffscode vereinfacht, den Sie schreiben müssen.

Die Modellklassen, die Sie erstellen, werden als POCO-Klassen (von „plain-old CLR objects“) bezeichnet, da sie nicht über eine Abhängigkeit von EF Core verfügen. Sie definieren die Eigenschaften einer Datei, die in der Datenbank gespeichert ist.

In diesem Tutorial schreiben Sie zuerst die Modellklassen. Anschließend erstellt EF Core die Datenbank. Eine alternative Methode, die hier nicht aufgeführt ist, besteht darin, Modellklassen aus einer vorhandenen Datenbank zu generieren: [Getting Started with EF Core on ASP.NET Core with an Existing Database (Erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank)](/ef/core/get-started/aspnetcore/existing-db).

Beispiel [Anzeigen oder Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie).
