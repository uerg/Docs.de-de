<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="798dc-101">Tools für den Gerüstbau hinzufügen und die anfängliche Migration ausführen</span><span class="sxs-lookup"><span data-stu-id="798dc-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="798dc-102">Führen Sie in der Befehlszeile die folgenden .NET Core-CLI-Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="798dc-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="798dc-103">Mit dem Befehl `add package` werden die für das Gerüstbau-Modul benötigten Tools installiert.</span><span class="sxs-lookup"><span data-stu-id="798dc-103">The `add package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="798dc-104">Mit dem Befehl `ef migrations add InitialCreate` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="798dc-104">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="798dc-105">Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="798dc-105">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="798dc-106">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="798dc-106">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="798dc-107">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="798dc-107">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="798dc-108">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="798dc-108">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="798dc-109">Mit dem Befehl `ef database update` führen Sie in der Datei  *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="798dc-109">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>