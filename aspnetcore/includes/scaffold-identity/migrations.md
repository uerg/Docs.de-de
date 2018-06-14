<span data-ttu-id="c244e-101">Der generierte Code der Identity-Datenbank erfordert [Entity Framework Core Migrationen](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="c244e-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="c244e-102">Erstellen Sie eine Migration aus, und Aktualisieren der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c244e-102">Create a migration and update the database.</span></span> <span data-ttu-id="c244e-103">Führen Sie z. B. die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="c244e-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c244e-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c244e-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c244e-105">In Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="c244e-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c244e-106">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="c244e-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="c244e-107">Die Name-Parameter von "CreateIdentitySchema" für die `Add-Migration` Befehl ist willkürlich.</span><span class="sxs-lookup"><span data-stu-id="c244e-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="c244e-108">`"CreateIdentitySchema"` Beschreibt die Migration an.</span><span class="sxs-lookup"><span data-stu-id="c244e-108">`"CreateIdentitySchema"` describes the migration.</span></span>