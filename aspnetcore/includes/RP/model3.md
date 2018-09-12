<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="2a12a-101">Hinzufügen von Tools für den Gerüstbau und Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="2a12a-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="2a12a-102">Fügen Sie die folgenden Zeilen der *RazorPagesMovie.csproj*-Datei direkt vor dem abschließenden `</Project>`-Tag hinzu:</span><span class="sxs-lookup"><span data-stu-id="2a12a-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```  
<span data-ttu-id="2a12a-103">Führen Sie in der Befehlszeile die folgenden .NET Core-CLI-Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="2a12a-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="2a12a-104">Mit dem `DotNetCliToolReference`-Element und dem `add package`-Befehl werden die für die Gerüstbau-Engine benötigten Tools installiert.</span><span class="sxs-lookup"><span data-stu-id="2a12a-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="2a12a-105">Mit dem Befehl `ef migrations add InitialCreate` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2a12a-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="2a12a-106">Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="2a12a-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="2a12a-107">Das Argument `InitialCreate` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="2a12a-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="2a12a-108">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="2a12a-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="2a12a-109">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="2a12a-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="2a12a-110">Mit dem Befehl `ef database update` führen Sie in der Datei  *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2a12a-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
