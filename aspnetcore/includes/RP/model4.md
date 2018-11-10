<span data-ttu-id="681ea-101">Die folgende Tabelle zeigt die Details der Parameter des Codegenerators von ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="681ea-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="681ea-102">Parameter</span><span class="sxs-lookup"><span data-stu-id="681ea-102">Parameter</span></span>               | <span data-ttu-id="681ea-103">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="681ea-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="681ea-104">-m</span><span class="sxs-lookup"><span data-stu-id="681ea-104">-m</span></span>  | <span data-ttu-id="681ea-105">Der Name des Modells.</span><span class="sxs-lookup"><span data-stu-id="681ea-105">The name of the model.</span></span> |
| <span data-ttu-id="681ea-106">-dc</span><span class="sxs-lookup"><span data-stu-id="681ea-106">-dc</span></span>  | <span data-ttu-id="681ea-107">Der Datenkontext.</span><span class="sxs-lookup"><span data-stu-id="681ea-107">The data context.</span></span> |
| <span data-ttu-id="681ea-108">-udl</span><span class="sxs-lookup"><span data-stu-id="681ea-108">-udl</span></span> | <span data-ttu-id="681ea-109">Verwendung des Standardlayouts.</span><span class="sxs-lookup"><span data-stu-id="681ea-109">Use the default layout.</span></span> |
| <span data-ttu-id="681ea-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="681ea-110">-outDir</span></span> | <span data-ttu-id="681ea-111">Der relative Ausgabeordnerpfad zur Erstellung der Ansichten.</span><span class="sxs-lookup"><span data-stu-id="681ea-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="681ea-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="681ea-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="681ea-113">Fügt `_ValidationScriptsPartial` zu den Seiten „Create“ und „Edit“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="681ea-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="681ea-114">Verwenden Sie den `h`-Switch um Hilfe für den `aspnet-codegenerator razorpage`-Befehl zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="681ea-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="681ea-115">Testen der App</span><span class="sxs-lookup"><span data-stu-id="681ea-115">Test the app</span></span>

* <span data-ttu-id="681ea-116">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/Movies`).</span><span class="sxs-lookup"><span data-stu-id="681ea-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/Movies`).</span></span>
* <span data-ttu-id="681ea-117">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="681ea-117">Test the **Create** link.</span></span>

  ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="681ea-119">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="681ea-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="681ea-120">Wenn der folgende (oder ein ähnlicher) Fehler angezeigt wird, stellen Sie sicher, dass Migrationen ausgeführt wurden und die Datenbank aktualisiert wurde:</span><span class="sxs-lookup"><span data-stu-id="681ea-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
