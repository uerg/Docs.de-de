---
title: Bereiche
author: rick-anderson
description: Zeigt, wie mit Bereichen arbeiten.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 666be2da6b38ffb538ae3888ea879a4104c8fd12
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="areas"></a>Bereiche

Durch [Dhananjay Kumar](https://twitter.com/debug_mode) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Bereiche sind eine ASP.NET MVC-Funktion verwendet, um verwandte Funktionen als separaten Namespace (für das routing) und die Struktur des Veröffentlichungsordners (für Ansichten) in einer Gruppe zu organisieren. Mithilfe von Bereichen erstellt eine Hierarchie für die Weiterleitung und Hinzufügen von einem anderen Routenparameter, `area`in `controller` und `action`.

Bereiche bieten eine Möglichkeit, eine große ASP.NET Core MVC-Web-app in kleinere funktionale Einheiten zu partitionieren. Ein Bereich ist letztendlich eine MVC-Struktur in einer Anwendung. Klicken Sie in einer MVC-Projekt logische Komponenten wie das Modell, Controller und Ansicht in unterschiedlichen Ordnern beibehalten werden, und MVC verwendet Konventionen zur Namensgebung, um die Beziehung zwischen diesen Komponenten zu erstellen. Für eine große app kann es vorteilhaft sein, die app in separate hohe Ebene Bereiche der Funktionalität partitioniert sein. Z. B. eine e-Commerce-app mit mehreren Geschäftsbereichen, z. B. Auschecken, Abrechnung und Suche usw. Jede dieser Einheiten haben ihre eigenen logischen Komponentenansichten, Controller und Modelle. In diesem Szenario können Sie Bereiche verwenden, um die Geschäftskomponenten im selben Projekt physisch zu partitionieren.

Ein Bereich kann als kleinere funktionale Einheiten in einem ASP.NET Core MVC-Projekt mit einer eigenen Gruppe von Controller, Ansichten und Modellen definiert werden.

Erwägen Sie Bereiche in einer MVC Projekts, wenn:

* Ihre Anwendung mehrere allgemeine funktionalen Komponenten besteht, die logisch getrennt werden soll

* Sie möchten das MVC-Projekt zu partitionieren, sodass die einzelnen Funktionsbereiche unabhängig voneinander bearbeitet werden können

Bereich-Funktionen:

* Eine ASP.NET-MVC-Anwendung Core kann beliebig viele Bereiche enthalten.

* Jeder Bereich verfügt über einen eigenen Domänencontroller, Modelle und Ansichten

* Ermöglicht es Ihnen, große MVC-Projekte in mehrere allgemeine Komponenten zu organisieren, die unabhängig voneinander bearbeitet werden können

* Mehrere Domänencontroller mit dem gleichen Namen - unterstützt, solange sie verschiedene sind *Bereiche*

Werfen wir einen Blick auf ein Beispiel zur Veranschaulichung, wie Bereiche erstellt und verwendet werden. Angenommen, Sie haben eine Store-app, zwei unterschiedliche Gruppierungen der Controller und Ansichten verfügt: Produkte und Dienste. Ein typische Ordner-Struktur für, dass mithilfe von MVC-Bereichen unten aussieht:

* Projektname

  * Bereiche

    * Produkte

      * Domänencontroller

        * HomeController.cs

        * ManageController.cs

      * Ansichten

        * Startseite

          * Index.cshtml

        * Verwalten

          * Index.cshtml

    * Dienste

      * Domänencontroller

        * HomeController.cs

      * Ansichten

        * Startseite

          * Index.cshtml

Wenn MVC versucht, eine Ansicht in einem Bereich in der Standardeinstellung zu rendern, wird versucht, suchen Sie in den folgenden Speicherorten:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Hierbei handelt es sich um die Standardspeicherorte, die über geändert werden, können die `AreaViewLocationFormats` auf die `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Beispielsweise ist in den folgenden Code anstatt den Namen des Ordners als "Bereiche", es wurde geändert in "Kategorien".

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Beachten Sie, die die Struktur wird der *Ansichten* Ordner ist die einzige hier wichtig angesehen wird, und wie des Inhalts der Rest der Ordner *Controller* und *Modelle* ist **nicht** von Bedeutung. Angenommen, Sie benötigen eine *Controller* und *Modelle* Ordner überhaupt. Dies funktioniert, da der Inhalt des *Controller* und *Modelle* ist nur Code, der in eine .dll kompiliert wird, wobei als Inhalt der *Ansichten* erst eine Anforderung an, die Sicht wurde hergestellt.

Nachdem Sie die Ordnerhierarchie definiert haben, müssen Sie MVC mitteilen, dass jedem Controller einen Bereich zugeordnet ist. Nachholen werden, indem den Namen des Controllers, mit der `[Area]` Attribut.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Richten Sie eine Routendefinition, die mit der neu erstellten Bereiche arbeitet. Die [Routing an Controlleraktionen](routing.md) Artikel ins Detail, über das Erstellen von Route-Definitionen, einschließlich der Verwendung von herkömmlicher Routen im Vergleich zu attributenrouten wechselt. In diesem Beispiel verwenden wir eine herkömmliche Route. Öffnen Sie hierzu die *Startup.cs* Datei, und ändern Sie ihn durch Hinzufügen der `areaRoute` mit dem Namen Route-Definition, die unten.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

Navigieren zum `http://<yourApp>/products`, die `Index` Aktionsmethode von der `HomeController` in der `Products` Bereich aufgerufen wird.

## <a name="link-generation"></a>Linkgenerierung

* Generieren von Links von einer Aktion innerhalb eines Bereichs basiert Controller an eine andere Aktion in den gleichen Controller.

  Nehmen wir an, dass die aktuelle Anforderung Pfad ähnelt`/Products/Home/Create`

  HtmlHelper-Syntax:`@Html.ActionLink("Go to Product's Home Page", "Index")`

  Taghelpers Syntax:`<a asp-action="Index">Go to Product's Home Page</a>`

  Beachten Sie, dass wir nicht die Werte für "Area" und "Controller" bereitstellen müssen hier, wie sie bereits im Kontext der aktuellen Anforderung verfügbar sind. Diese Art von Werten heißen `ambient` Werte.

* Generieren von Links von einer Aktion innerhalb eines Bereichs an eine andere Aktion auf einen anderen Controller Controller basierend

  Nehmen wir an, dass die aktuelle Anforderung Pfad ähnelt`/Products/Home/Create`

  HtmlHelper-Syntax:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`

  Taghelpers Syntax:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Beachten Sie, dass hier der Ambiente-Wert von einem "Bereich" verwendet jedoch der Wert "Controller" oben explizit angegeben wird.

* Generieren von Links von einer Aktion innerhalb eines Bereichs basiert Controller an eine andere Aktion auf einen anderen Controller und einem anderen Bereich.

  Nehmen wir an, dass die aktuelle Anforderung Pfad ähnelt`/Products/Home/Create`

  HtmlHelper-Syntax:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`

  Taghelpers Syntax:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`

  Beachten Sie, dass hier keine ambient-Werte verwendet werden.

* Generieren von Links aus einer Aktion innerhalb eines Controllers Basis an eine andere Aktion auf einen anderen Controller und **nicht** in einem Bereich.

  HtmlHelper-Syntax:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`

  Taghelpers Syntax:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Da wir generieren möchten basierend Links zu einem Bereich bei den ambient-Wert für "Area" hier Controlleraktion, es leer.

## <a name="publishing-areas"></a>Veröffentlichen von Bereichen

Alle `*.cshtml` und `wwwroot/**` Dateien werden veröffentlicht, wenn die Ausgabe `<Project Sdk="Microsoft.NET.Sdk.Web">` dient in der *csproj* Datei.
