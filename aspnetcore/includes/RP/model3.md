<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Hinzufügen von Tools für den Gerüstbau und Ausführen der anfänglichen Migration

Fügen Sie die folgenden Zeilen der *RazorPagesMovie.csproj*-Datei direkt vor dem abschließenden `</Project>`-Tag hinzu:

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
Führen Sie in der Befehlszeile die folgenden .NET Core-CLI-Befehle aus:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Mit dem `DotNetCliToolReference`-Element und dem `add package`-Befehl werden die für die Gerüstbau-Engine benötigten Tools installiert.

Mit dem Befehl `ef migrations add InitialCreate` wird Code generiert, um das anfängliche Datenbankschema zu erstellen. Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist. Das Argument `InitialCreate` wird verwendet, um die Migrationen zu benennen. Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt. Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).

Mit dem Befehl `ef database update` führen Sie in der Datei  *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.
