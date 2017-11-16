
## <a name="test-the-app"></a><span data-ttu-id="ba4d5-101">Testen der App</span><span class="sxs-lookup"><span data-stu-id="ba4d5-101">Test the app</span></span>

* <span data-ttu-id="ba4d5-102">Führen Sie die App aus, und tippen Sie auf den Link **Mvc Movie**.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="ba4d5-103">Tippen Sie auf den Link **Neu erstellen**, und erstellen Sie einen Film.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-103">Tap the **Create New** link and create a movie.</span></span>

  ![Erstellen Sie die Ansicht mit den Feldern für ein Genre, den Preis, das Veröffentlichungsdatum und den Titel.](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="ba4d5-105">Sie können unter Umständen in das Feld `Price` keine Dezimaltrennzeichen oder Kommas eingeben.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="ba4d5-106">Zur Unterstützung der [jQuery-Validierung](https://jqueryvalidation.org/) in nicht englischen Gebietsschemas, in denen ein Komma („,“) als Dezimaltrennzeichen verwendet wird, und Nicht-US-englische Datums- und Uhrzeitformate, müssen Sie Schritte zur Globalisierung Ihrer App ausführen.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="ba4d5-107">Weitere Informationen finden Sie unter https://github.com/aspnet/Docs/issues/4076 und [Zusätzliche Ressourcen](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="ba4d5-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="ba4d5-108">Geben Sie einstweilen ganze Zahlen wie 10 ein.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="ba4d5-109">In manchen Gebietsschemas müssen Sie das Datumsformat angeben.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="ba4d5-110">Betrachten Sie den unten markierten Code.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-110">See the highlighted code below.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="ba4d5-111">Mit `DataAnnotations` beschäftigen wir uns später im Tutorial.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="ba4d5-112">Wenn Sie auf **Erstellen** tippen, wird das Formular an den Server bereitgestellt, auf dem die Filminformationen in einer Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="ba4d5-113">Die App leitet an die URL */Movies* weiter, auf der die neu erstellten Filminformationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Filmansicht mit neu erstellen Filmeinträgen](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="ba4d5-115">Erstellen Sie ein paar weitere Filmeinträge.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-115">Create a couple more movie entries.</span></span> <span data-ttu-id="ba4d5-116">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.</span><span class="sxs-lookup"><span data-stu-id="ba4d5-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
