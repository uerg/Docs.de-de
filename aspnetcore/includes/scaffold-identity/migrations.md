Der generierte Code der Identity-Datenbank erfordert [Entity Framework Core Migrationen](/ef/core/managing-schemas/migrations/). Erstellen Sie eine Migration aus, und Aktualisieren der Datenbank. Führen Sie z. B. die folgenden Befehle ein:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio **Package Manager Console**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Die Name-Parameter von "CreateIdentitySchema" für die `Add-Migration` Befehl ist willkürlich. `"CreateIdentitySchema"` Beschreibt die Migration an.