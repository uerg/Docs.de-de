# <a name="update-the-generated-pages"></a><span data-ttu-id="5d15b-101">Aktualisieren der generierten Seiten</span><span class="sxs-lookup"><span data-stu-id="5d15b-101">Update the generated pages</span></span>

<span data-ttu-id="5d15b-102">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5d15b-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5d15b-103">Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="5d15b-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="5d15b-104">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5d15b-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="5d15b-106">Aktualisieren des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="5d15b-106">Update the generated code</span></span>

<span data-ttu-id="5d15b-107">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="5d15b-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](code/Models/Movie.cs?highlight=2,11-12)]
