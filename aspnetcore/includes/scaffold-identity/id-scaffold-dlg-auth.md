<span data-ttu-id="2734f-101">Führen Sie die Identität Scaffolder:</span><span class="sxs-lookup"><span data-stu-id="2734f-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2734f-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2734f-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2734f-103">Von **Projektmappen-Explorer**, mit der rechten Maustaste auf das Projekt > **hinzufügen** > **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="2734f-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="2734f-104">Im linken Bereich des der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Identität** > **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2734f-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="2734f-105">In der **ADD Identität** Dialogfeld Wählen Sie die gewünschten Optionen.</span><span class="sxs-lookup"><span data-stu-id="2734f-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="2734f-106">Wählen Ihre vorhandene Seitenlayout, oder durch falsche Markup Layout-Datei überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="2734f-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="2734f-107">Wenn eine vorhandene Datei "_Layout.cshtml" ausgewählt ist, ist es **nicht** überschrieben.</span><span class="sxs-lookup"><span data-stu-id="2734f-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="2734f-108">Z. B. `~/Pages/Shared/_Layout.cshtml` für Razor-Seiten `~/Views/Shared/_Layout.cshtml` für MVC-Projekte</span><span class="sxs-lookup"><span data-stu-id="2734f-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="2734f-109">Um Ihre vorhandenen Datenkontext zu verwenden, wählen Sie mindestens eine Datei zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="2734f-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="2734f-110">Sie müssen mindestens eine Datei auf Ihrem Datenkontext hinzufügen auswählen.</span><span class="sxs-lookup"><span data-stu-id="2734f-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="2734f-111">Wählen Sie Ihre Daten Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="2734f-111">Select your data context class.</span></span>
  * <span data-ttu-id="2734f-112">Wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2734f-112">Select **ADD**.</span></span>
* <span data-ttu-id="2734f-113">Erstellt einen neuen Benutzerkontext aus, und erstellen eine benutzerdefinierte Klasse möglicherweise für Identität:</span><span class="sxs-lookup"><span data-stu-id="2734f-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="2734f-114">Wählen Sie die **+** Schaltfläche zum Erstellen eines neuen **Datenkontextklasse**.</span><span class="sxs-lookup"><span data-stu-id="2734f-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="2734f-115">Wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2734f-115">Select **ADD**.</span></span>

<span data-ttu-id="2734f-116">Hinweis: Wenn Sie einen neuen Benutzerkontext erstellen, müssen Sie eine Datei außer Kraft setzen möchten.</span><span class="sxs-lookup"><span data-stu-id="2734f-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2734f-117">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="2734f-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2734f-118">Wenn Sie die ASP.NET Scaffolder noch nicht installiert haben, installieren Sie es jetzt ein:</span><span class="sxs-lookup"><span data-stu-id="2734f-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="2734f-119">Hinzufügen eines Verweises Paket auf [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) zum Projekt (\*csproj) Datei.</span><span class="sxs-lookup"><span data-stu-id="2734f-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="2734f-120">Führen Sie den folgenden Befehl in das Projektverzeichnis:</span><span class="sxs-lookup"><span data-stu-id="2734f-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="2734f-121">Führen Sie den folgenden Befehl zum Auflisten von Optionen Scaffolder Identität:</span><span class="sxs-lookup"><span data-stu-id="2734f-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="2734f-122">Führen Sie die Identität Scaffolder in den Projektordner mit den gewünschten Optionen.</span><span class="sxs-lookup"><span data-stu-id="2734f-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="2734f-123">Angenommen, um die Identität mit der Standardbenutzeroberfläche und die minimale Anzahl von Dateien einrichten möchten, führen Sie den folgenden Befehl an.</span><span class="sxs-lookup"><span data-stu-id="2734f-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="2734f-124">Verwenden Sie den richtigen vollqualifizierten Namen für den DB-Kontext:</span><span class="sxs-lookup"><span data-stu-id="2734f-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
