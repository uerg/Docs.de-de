Ersetzen Sie den Inhalt der Razor-Ansichtsdatei *Views/HelloWorld/Index.cshtml* durch Folgendes:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Navigieren Sie zu `http://localhost:xxxx/HelloWorld`. Die `Index`-Methode im `HelloWorldController` hatte nicht viel zu tun. Sie diente zum Ausführen der Anweisung `return View();`, die angab, dass die Methode eine Ansichtsvorlagendatei zum Rendern einer Antwort im Browser verwenden sollte. Da Sie den Namen der Ansichtsvorlagendatei nicht explizit angegeben haben, verwendete MVC standardmäßig die Ansichtsdatei *Index.cshtml* im Ordner */Views/HelloWorld*. Die folgende Abbildung zeigt die Zeichenfolge "Hello from our View Template!“, die in der Ansicht hartcodiert ist.

![Browserfenster](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Wenn Ihr Browserfenster zu klein ist (z.B. auf einem Mobilgerät), müssen Sie ggf. rechts oben auf die Navigationsschaltfläche mit den [drei horizontalen Linien](http://getbootstrap.com/components/#navbar) tippen, um die Links **Home**, **About** und **Contact** anzuzeigen.

![Browserfenster mit Hervorhebung der Navigationsschaltfläche mit den drei horizontalen Linien](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Tippen Sie auf die Menülinks (**MvcMovie**, **Home**, **About**). Auf jeder Seite wird dasselbe Menülayout gezeigt. Das Menülayout wird mithilfe der Datei *Views/Shared/_Layout.cshtml* implementiert. Öffnen Sie die Datei *Views/Shared/_Layout.cshtml*.

[Layout](xref:mvc/views/layout)-Vorlagen ermöglichen Ihnen, das HTML-Containerlayout Ihrer Website zentral anzugeben und dann auf mehrere Seiten Ihrer Website anzuwenden. Suchen Sie die Zeile `@RenderBody()`. `RenderBody` ist ein Platzhalter, bei dem alle ansichtsspezifischen Seiten, die Sie erstellen, von der Layoutseite *umschlossen* angezeigt werden. Wenn Sie beispielsweise auf den Link **About** tippen, wird die Ansicht **Views/Home/About.cshtml** in der `RenderBody`-Methode gerendert.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Ändern des Titels und Menülinks in der Layoutdatei

Ändern Sie im title-Element `MvcMovie` in `Movie App`. Ändern Sie wie unten hervorgehoben den Ankertext in der Layoutvorlage von `MvcMovie` in `Movie App` und den Controller von `Home` in `Movies`:

::: moniker range="<= aspnetcore-2.0"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=7,31)]
::: moniker-end

>[!WARNING]
> Wir haben den Controller `Movies` noch nicht implementiert, sodass beim Klicken auf diesen Link der Fehler 404 (Nicht gefunden) zurückgegeben wird.

Speichern Sie Ihre Änderungen, und tippen Sie auf den Link **About**. Wie Sie sehen, wird der Titel der Browserregisterkarte nun als **About - Movie App** anstatt als **About - Mvc Movie** angezeigt: 

![Über die Registerkarte](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Tippen Sie auf den Link **Kontakt**, und beachten Sie, dass der Text für den Titel und den Anker ebenfalls **Movie App** anzeigt. Wir haben also eine Änderung des Texts und Titels in der Layoutvorlage einmalig vorgenommen, die von der gesamten Website übernommen wird.

Untersuchen Sie die Datei *Views/_ViewStart.cshtml*:


```HTML
@{
    Layout = "_Layout";
}
```

Die Datei *Views/_ViewStart.cshtml* fügt jeder Ansicht die Datei *Views/Shared/_Layout.cshtml* hinzu. Sie können mithilfe der `Layout`-Eigenschaft eine andere Layoutansicht festlegen oder diese auf `null` festlegen, damit keine Layoutdatei verwendet wird.

Ändern Sie den Titel der Ansicht `Index`.

Öffnen Sie die Datei *Views/HelloWorld/Index.cshtml*. Es gibt zwei Stellen, an denen eine Änderung vorgenommen werden kann:

   * Der Text, der in der Titelleiste des Browsers angezeigt wird.
   * Die sekundäre Kopfzeile (`<h2>`-Element).

Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` im Code oben legt die `Title`-Eigenschaft des Wörterbuchs `ViewData`auf „Movie List“ fest. Die `Title`-Eigenschaft wird im HTML-Element `<title>` der Layoutseite verwendet:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Speichern Sie die Änderung, und navigieren Sie zu `http://localhost:xxxx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.) Der Titel des Browsers wird mithilfe des Elements `ViewData["Title"]` erstellt, das wir in der Ansichtsvorlange *Index.cshtml* festgelegt haben, und des zusätzlichen Elements „- Movie App“, das in der Layoutdatei hinzugefügt wurde.

Beachten Sie außerdem, wie der Inhalt der Ansichtsvorlage *Index.cshtml* mit der Ansichtsvorlage *Views/Shared/_Layout.cshtml* zusammengeführt und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung. Weitere Informationen dazu finden Sie unter [Layout](xref:mvc/views/layout).

![Ansicht mit Filmliste](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Unser kleiner Beitrag zu den „Daten“ (in diesem Fall die Meldung "Hello from our View Template!") ist jedoch hartcodiert. Die MVC-Anwendung weist ein „V“ (View [Ansicht]) auf, und Sie verfügen über ein „C“ (Controller), aber noch nicht über ein „M“ (Modell).

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Controlleraktionen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. In eine Controllerklasse schreiben Sie den Code, der die eingehenden Browseranforderungen verarbeitet. Der Controller ruft Daten aus einer Datenquelle ab und entscheidet, welche Art von Antwort an den Browser zurückgesendet wird. Ansichtsvorlagen können von einem Controller zum Generieren und Formatieren einer HTML-Antwort an den Browser verwendet werden.

Controller sind für die Bereitstellung der benötigten Daten zuständig, damit eine Ansichtsvorlage eine Antwort liefern kann. Eine bewährte Methode: Ansichtsvorlagen sollten **keine**  Geschäftslogik ausführen bzw. nicht direkt mit einer Datenbank interagieren. Eine Ansichtsvorlage sollte stattdessen nur mit den Daten arbeiten, die ihr vom Controller bereitgestellt werden. Durch Beibehaltung dieser Bereichstrennung bleibt Ihr Code übersichtlich, testfähig und verwaltbar.

Derzeit verwendet die `Welcome`-Methode in der `HelloWorldController`-Klasse die Parameter `name` und `ID` und gibt anschließend die Werte direkt an den Browser aus. Anstatt den Controller diese Antwort als Zeichenfolge rendern zu lassen, passen Sie den Controller so an, dass er eine Ansichtsvorlage verwendet. Die Ansichtsvorlage erstellt eine dynamische Antwort, was bedeutet, dass entsprechende Datenbestandteile vom Controller an die Ansicht übergeben werden müssen, damit eine Antwort erstellt wird. Lassen Sie hierzu den Controller die dynamischen Daten (Parameter), die die Ansichtsvorlage benötigt, in einem `ViewData`-Wörterbuch ablegen, auf das die Ansichtsvorlage zugreifen kann.

Kehren Sie zur Datei *HelloWorldController.cs* zurück, und ändern Sie die `Welcome`-Methode so, dass die Werte `Message` und `NumTimes` dem Wörterbuch `ViewData` hinzugefügt werden. Das Wörterbuch `ViewData` ist ein dynamisches Objekt, was bedeutet, dass Sie ihm einfach die gewünschten Inhalte hinzufügen können. Das `ViewData`-Objekt weist erst dann definierte Eigenschaften auf, nachdem Sie ihm etwas hinzugefügt haben. Das [MVC-Modellbindungssystem](xref:mvc/models/model-binding) ordnet automatisch die benannten Parameter (`name` und `numTimes`) aus der Abfragezeichenfolge auf der Adressleiste den Parametern der Methode zu. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

Das Wörterbuchobjekt `ViewData` enthält Daten, die an die Ansicht übergeben werden. 

Erstellen Sie die Ansichtsvorlage für die Begrüßung namens *Views/HelloWorld/Welcome.cshtml*.

Erstellen Sie eine Schleife in der Ansichtsvorlage *Welcome.cshtml*, die „Hello“ `NumTimes` anzeigt. Ersetzen Sie den Inhalt von *Views/HelloWorld/Welcome.cshtml* durch Folgendes:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Speichern Sie die Änderungen, und navigieren Sie zur folgenden URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Daten werden der URL entnommen und mithilfe der [MVC-Modellbindung](xref:mvc/models/model-binding) an den Controller übergeben. Der Controller packt die Daten in einem `ViewData`-Wörterbuch und übergibt das Objekt an die Ansicht. Die Ansicht rendert dann die Daten im HTML-Format im Browser.

![Die Ansicht „About“ mit der Beschriftung „Welcome“ und der viermal gezeigten Wortfolge „Hello Rick“](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

Im obigen Beispiel haben wir das Wörterbuch `ViewData` zum Übergeben von Daten vom Controller an eine Ansicht verwendet. Später in diesem Tutorial verwenden wir eine Ansichtsmodell, um Daten von einem Controller an eine Ansicht zu übergeben. Der Ansatz mit dem Ansichtsmodell für das Übergeben von Daten ist im Allgemeinen dem Ansatz mit dem Wörterbuch `ViewData` vorzuziehen. Weitere Informationen finden Sie unter [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc).

Das war also eine Art eines „M“ für Modell, jedoch nicht der Art Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.
