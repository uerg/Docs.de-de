<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="918ed-101">Hinzufügen einer Datenbank-Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="918ed-101">Add a database connection string</span></span>

<span data-ttu-id="918ed-102">Fügen Sie der Datei *appsettings.json* eine Verbindungszeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="918ed-102">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="918ed-103">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="918ed-103">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="918ed-104">Datenbankkontext registrieren</span><span class="sxs-lookup"><span data-stu-id="918ed-104">Register the database context</span></span>

<span data-ttu-id="918ed-105">Registrieren Sie den Kontext der Datenbank mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="918ed-105">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>