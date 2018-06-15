## <a name="register-the-database-context"></a><span data-ttu-id="54581-101">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="54581-101">Register the database context</span></span>

<span data-ttu-id="54581-102">In diesem Schritt wird der Datenbankkontext beim Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) registriert.</span><span class="sxs-lookup"><span data-stu-id="54581-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="54581-103">Dienste (z. B. der Datenbankkontext), die beim Abhängigkeitsinjektionscontainer (DI) registriert sind, sind für Controller verfügbar.</span><span class="sxs-lookup"><span data-stu-id="54581-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="54581-104">Registrieren Sie den Datenbankkontext mithilfe der integrierten Unterstützung der [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) beim Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="54581-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="54581-105">Ersetzen Sie den Inhalt der Datei *Startup.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="54581-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="54581-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="54581-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="54581-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="54581-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>

::: moniker-end  

<span data-ttu-id="54581-108">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="54581-108">The preceding code:</span></span>

* <span data-ttu-id="54581-109">Entfernt nicht verwendeten Code.</span><span class="sxs-lookup"><span data-stu-id="54581-109">Removes the unused code.</span></span>
* <span data-ttu-id="54581-110">Gibt an, dass eine In-Memory Database in den Dienstcontainer eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="54581-110">Specifies an in-memory database is injected into the service container.</span></span>
