<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="a1c05-101">Hinzufügen einer Datenbank-Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="a1c05-101">Add a database connection string</span></span>

<span data-ttu-id="a1c05-102">Fügen Sie der Datei *appsettings.json* eine Verbindungszeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="a1c05-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="a1c05-103">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="a1c05-103">Register the database context</span></span>

<span data-ttu-id="a1c05-104">Registrieren Sie den Kontext der Datenbank mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a1c05-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>