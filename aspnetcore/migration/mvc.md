---
title: Migrieren von ASP.NET MVC zu ASP.NET Core MVC
author: ardalis
description: Informationen Sie zum Migrieren eines ASP.NET MVC-Projekts zu ASP.NET Core MVC beginnen.
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 6fecc820177ca5033cfd00d632904a950c1b29bb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274774"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrieren von ASP.NET MVC zu ASP.NET Core MVC

Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), und [Scott Addie](https://scottaddie.com)

In diesem Artikel wird gezeigt, wie für den Einstieg in die Migration von einer ASP.NET MVC-Projekts zu [Core ASP.NET-MVC](../mvc/overview.md). In der Prozess wird hervorgehoben viele der Aufgaben, die von ASP.NET MVC geändert haben. Migrieren von ASP.NET MVC umfasst mehrere Schritte, und dieser Artikel behandelt die Anfangssetup, grundlegende Controller und Ansichten, statische Inhalte und Client-Side-Abhängigkeiten. Zusätzliche Artikel behandelt Migrieren der Konfiguration und ID-Code in ASP.NET MVC-Projekte, die viele gefunden.

> [!NOTE]
> Die Versionsnummern in die Beispiele sind möglicherweise keine aktuellen. Sie müssen möglicherweise Ihre Projekte entsprechend aktualisieren.

## <a name="create-the-starter-aspnet-mvc-project"></a>Erstellen von Starter ASP.NET MVC-Projekt

Um das Upgrade zu demonstrieren, beginnen wir mit eine ASP.NET MVC-app erstellen. Erstellen sie mit dem Namen *WebApp1* , damit der Namespace des Projekts ASP.NET Core entspricht wir im nächsten Schritt erstellen.

![Visual Studio-Projekt ein neues Dialogfeld "](mvc/_static/new-project.png)

![Dialogfeld "Neues Webanwendung": MVC-Projektvorlage in ASP.NET Vorlagen Bereich ausgewählt](mvc/_static/new-project-select-mvc-template.png)

*Optional:* ändern Sie den Namen der Projektmappe aus *WebApp1* auf *Mvc5*. Zeigt den Namen der neuen Projektmappe von Visual Studio (*Mvc5*), die erleichtert es, teilen Sie das Projekt über das nächste Projekt.

## <a name="create-the-aspnet-core-project"></a>Erstellen des Projekts ASP.NET Core

Erstellen Sie ein neues *leere* ASP.NET Core Web-app mit dem gleichen Namen wie das vorherige Projekt (*WebApp1*), damit die Namespaces in beiden Projekten übereinstimmen. Mit dem gleichen Namespace erleichtert es, Code zwischen den zwei Projekten kopieren. Sie müssen zum Erstellen des Projekts in einem anderen Verzeichnis als das vorherige Projekt, das den gleichen Namen verwenden.

![Dialogfeld "Neues Projekt"](mvc/_static/new_core.png)

![Dialogfeld "neues ASP.NET Web-Anwendung": leere Projektvorlage, die im Bereich der Vorlagen für ASP.NET Core ausgewählt](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Optional:* Erstellen eines neuen ASP.NET Core app mithilfe der *Webanwendung* -Projektvorlage. Nennen Sie das Projekt *WebApp1*, und wählen Sie eine Authentifizierungsoption in **einzelne Benutzerkonten**. Benennen Sie diese app *FullAspNetCore*. Erstellen dieses Projekt sparen Sie Zeit bei der Konvertierung. Betrachten Sie die Vorlage generiertem Code auf das Endergebnis finden Sie unter und im Code in das Konvertierungsprojekt zu kopieren. Es ist auch hilfreich, wenn Sie auf einen Konvertierungsschritt für den Vergleich mit der Vorlage generiert Projekt festsitzt.

## <a name="configure-the-site-to-use-mvc"></a>Konfigurieren Sie den Standort für MVC verwenden

* Wenn .NET Core verwenden möchten, wird der ASP.NET Core Metapackage auf das Projekt mit der Bezeichnung hinzugefügt `Microsoft.AspNetCore.All` standardmäßig. Dieses Paket enthält Pakete wie `Microsoft.AspNetCore.Mvc` und `Microsoft.AspNetCore.StaticFiles`. Wenn .NET Framework abzielen, müssen paketverweisen in der Datei *.csproj einzeln aufgelistet werden.

`Microsoft.AspNetCore.Mvc` ist das ASP.NET-MVC-Framework Core. `Microsoft.AspNetCore.StaticFiles` ist der Handler für statische Dateien. Die Laufzeit ASP.NET Core ist modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen (siehe [statische Dateien](xref:fundamentals/static-files)).

* Öffnen der *Startup.cs* Datei und ändern Sie den Code entsprechend der folgenden:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

Die `UseStaticFiles` Erweiterungsmethode fügt Handler für statische Dateien. Wie bereits erwähnt, ist die ASP.NET-Laufzeit modular aufgebaut, und Sie müssen explizit entscheiden Sie sich für statische Dateien dienen. Die `UseMvc` Erweiterungsmethode fügt routing. Weitere Informationen finden Sie unter [Anwendungsstart](xref:fundamentals/startup) und [Routing](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Fügen Sie einen Controller und Ansicht hinzu.

In diesem Abschnitt fügen Sie einem minimalen Controller und Ansicht dienen als Platzhalter für den ASP.NET MVC-Controller und Ansichten werden im nächsten Abschnitt migrieren.

* Hinzufügen einer *Controller* Ordner.

* Hinzufügen einer **Controllerklasse** mit dem Namen *HomeController.cs* auf die *Controller* Ordner.

![Dialogfeld „Neues Element hinzufügen“](mvc/_static/add_mvc_ctl.png)

* Hinzufügen einer *Ansichten* Ordner.

* Hinzufügen einer *Ansichten/Start* Ordner.

* Hinzufügen einer **Razor-Ansicht** mit dem Namen *Index.cshtml* auf die *Ansichten/Start* Ordner.

![Dialogfeld „Neues Element hinzufügen“](mvc/_static/view.png)

Die Projektstruktur wird unten gezeigt:

![Projektmappen-Explorer mit Dateien und Ordner von WebApp1](mvc/_static/project-structure-controller-view.png)

Ersetzen Sie den Inhalt von der *Views/Home/Index.cshtml* Datei durch Folgendes:

```html
<h1>Hello world!</h1>
```

Führen Sie die App aus.

![Öffnen Sie in Microsoft Edge-Web-app](mvc/_static/hello-world.png)

Finden Sie unter [Controller](xref:mvc/controllers/actions) und [Ansichten](xref:mvc/views/overview) für Weitere Informationen.

Nun, da wir ein minimales funktionierenden ASP.NET Core-Projekt haben, beginnen wir Migrieren von Funktionen von ASP.NET MVC-Projekt. Wir müssen die folgenden verschieben:

* die clientseitige Inhalt (CSS, Schriftarten und Skripts)

* Controller

* Ansichten

* Modelle

* Bundling

* Filter

* Melden Sie sich in/out Identität (Dies erfolgt in den nächsten Lernprogrammen.)

## <a name="controllers-and-views"></a>Controller und Ansichten

* Kopieren Sie jede der Methoden von ASP.NET MVC `HomeController` mit dem neuen `HomeController`. Beachten Sie, dass in ASP.NET-MVC integrierten Vorlage Controller Aktion Methodenrückgabetyp [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC die Aktionsmethoden return `IActionResult` stattdessen. `ActionResult` implementiert `IActionResult`, daher keine Notwendigkeit besteht, den Rückgabetyp der Aktionsmethoden zu ändern.

* Kopieren der *About.cshtml*, *Contact.cshtml*, und *Index.cshtml* Razor-Ansicht-Dateien aus dem ASP.NET MVC-Projekt dem ASP.NET Core-Projekt.

* Führen Sie die ASP.NET Core-app, und Testen Sie jede Methode zu. Wir noch nicht die Layoutdatei oder Stile migriert noch, damit die gerenderten Ansichten nur den Inhalt in den Dateien anzeigen enthalten. Sie erhalten die Layout generierte dateilinks für keinen der `About` und `Contact` anzeigt, daher Sie diese über den Browser aufrufen müssen (ersetzen Sie **4492** mit der Portnummer, die in Ihrem Projekt verwendet).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Seite "Kontakte"](mvc/_static/contact-page.png)

Beachten Sie die fehlende formatieren und Menüelementen aus. Dies soll im nächsten Abschnitt behoben werden.

## <a name="static-content"></a>Statischer Inhalt

In früheren Versionen von ASP.NET MVC statischen Inhalte wurde aus dem Stammverzeichnis des Webprojekts gehostet und wurde mit serverseitigen Dateien zusammensetzt. In ASP.NET Core, statischer Inhalt gehostet wird, der *"Wwwroot"* Ordner. Sie möchten den statischen Inhalt von alten einer ASP.NET MVC-Anwendung zu kopieren der *"Wwwroot"* Projektordners ASP.NET Core. In diesem Beispiel Konvertierung:

* Kopieren der *favicon.ico* -Datei aus dem alten MVC-Projekt in der *"Wwwroot"* Ordner des Projekts ASP.NET Core.

Die alte ASP.NET MVC-Projekt verwendet [Bootstrap](https://getbootstrap.com/) für seine Erstellen von Formaten und speichert die Bootstrap-in Dateien den *Content* und *Skripts* Ordner. Die Vorlage, die mit das alte ASP.NET MVC-Projekt generiert wird, verweist auf Bootstrap in der Layoutdatei (*Views/Shared/_Layout.cshtml*). Sie kopieren konnte die *bootstrap.js* und *bootstrap.css* Dateien von ASP.NET MVC-Projekt auf die *"Wwwroot"* Ordner in das neue Projekt. Stattdessen fügen wir Unterstützung für Bootstrap (und andere clientseitige Bibliotheken) mithilfe des CDNs im nächsten Abschnitt.

## <a name="migrate-the-layout-file"></a>Migrieren Sie die Layoutdatei

* Kopieren der *Ansichten "_viewstart.cshtml"* Datei aus des alten ASP.NET MVC-Projekts *Ansichten* Ordner, in des ASP.NET Core Projekts *Ansichten* Ordner. Die *Ansichten "_viewstart.cshtml"* Datei Core ASP.NET-MVC nicht geändert.

* Erstellen einer *Ansichten/freigegeben* Ordner.

* *Optional:* Kopie *_ViewImports.cshtml* aus der *FullAspNetCore* MVC-Projekt *Ansichten* Ordner, in des ASP.NET Core Projekts  *Sichten* Ordner. Entfernen Sie alle Namespacedeklaration in der *_ViewImports.cshtml* Datei. Die *_ViewImports.cshtml* Datei bietet Namespaces für alle Dateien anzeigen und bringt in [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro). Tag-Hilfsmethoden werden in das Neue Layoutdatei verwendet. Die *_ViewImports.cshtml* Datei ist neu in ASP.NET Core.

* Kopieren der *_Layout.cshtml* Datei aus des alten ASP.NET MVC-Projekts *Ansichten/freigegeben* Ordner, in des ASP.NET Core Projekts *Ansichten/freigegeben* Ordner.

Open *_Layout.cshtml* Datei, und nehmen Sie die folgenden Änderungen (der fertigen Code wird unten dargestellt):

* Ersetzen Sie `@Styles.Render("~/Content/css")` mit einem `<link>` Element laden *bootstrap.css* (siehe unten).

* Entfernen Sie `@Scripts.Render("~/bundles/modernizr")`.

* Kommentieren Sie Sie aus der `@Html.Partial("_LoginPartial")` Zeile (die Zeile mit umschließen `@*...*@`). Wir müssen in einem späteren Lernprogramm darauf zurück.

* Ersetzen Sie `@Scripts.Render("~/bundles/jquery")` mit einem `<script>` Element (siehe unten).

* Ersetzen Sie `@Scripts.Render("~/bundles/bootstrap")` mit einem `<script>` Element (siehe unten).

Das Ersatz-Markup für die Aufnahme der Bootstrap-CSS:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Das Ersatz-Markup für jQuery oder Bootstrap JavaScript einschließen:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Die aktualisierte *_Layout.cshtml* Datei wird unten gezeigt:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Zeigen Sie die Website im Browser. Es sollte jetzt ordnungsgemäß mit den erwarteten Formatvorlagen direktes Laden.

* *Optional:* möglicherweise möchten Sie versuchen, mithilfe der neuen Layoutdatei. Für dieses Projekt kopieren Sie die Layoutdatei aus der *FullAspNetCore* Projekt. Verwendet die Neue Layoutdatei [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro) und hat den weiteren Verbesserungen.

## <a name="configure-bundling-and-minification"></a>Konfigurieren von Bündelung und Minimierung

Informationen zum Konfigurieren von Bündelung und Minimierung finden Sie unter [Bündelung und Minimierung](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Beheben von HTTP 500-Fehler

Es gibt viele Probleme, die eine HTTP 500-Fehlermeldung verursachen können, die keine Informationen für die Quelle des Problems enthalten. Beispielsweise, wenn die *Views/_ViewImports.cshtml* -Datei enthält einen Namespace, der in Ihrem Projekt nicht vorhanden ist, erhalten Sie einen HTTP 500-Fehler. In ASP.NET Core-apps standardmäßig die `UseDeveloperExceptionPage` Erweiterung wird hinzugefügt, um die `IApplicationBuilder` ausgeführt, sobald die Konfiguration ist *Entwicklung*. Dies ist in den folgenden Code beschrieben:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core konvertiert nicht behandelte Ausnahmen in einer Web-app in HTTP 500-Fehlerantworten. Fehlerdetails werden nicht in der Regel wird in diese Antworten auf die Offenlegung von potenziell vertrauliche Informationen über den Server zu verhindern, dass aufgenommen. Finden Sie unter **Developer Ausnahme auf der Dienstkontoseite** in [Fehler behandeln](../fundamentals/error-handling.md) für Weitere Informationen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Clientseitige Entwicklung](xref:client-side/index)
* [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro)
