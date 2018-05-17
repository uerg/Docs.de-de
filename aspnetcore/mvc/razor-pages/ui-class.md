---
title: Wiederverwendbare Razor-Benutzeroberfläche in Klassenbibliotheken mit ASP.NET Core
author: Rick-Anderson
description: Informationen zum Erstellen einer wiederverwendbaren Razor-Benutzeroberfläche in einer Klassenbibliothek
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Erstellen einer wiederverwendbaren Benutzeroberfläche mithilfe eines RCL-Projekts in ASP.NET Core.

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Ansichten, Seiten, Controller, Seitenmodelle und Datenmodelle im Zusammenhang mit Razor können in einer Razor-Klassenbibliothek (Razor Class Library, RCL) zusammengefasst und erstellt werden. Die RCL kann verpackt und wiederverwendet werden. Sie kann in Anwendungen eingebunden werden, wodurch sich die in der RCL enthaltenen Ansichten und Seiten überschreiben lassen. Wenn eine Ansicht, Teilansicht oder Razor-Seite sowohl in der Web-App als auch in der RCL enthalten ist, hat das Razor-Markup (die *CSHTML*-Datei) in der Web-App Vorrang.

Für dieses Feature ist [!INCLUDE[](~/includes/2.1-SDK.md)] erforderlich.

[!INCLUDE[](~/includes/2.1.md)]

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Erstellen einer Klassenbibliothek, die eine Razor-Benutzeroberfläche enthält

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.
* Wählen Sie **ASP.NET Core-Webanwendung** aus.
* Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.
* Klicken Sie auf **Razor Class Library** (Razor-Klassenbibliothek) > **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Führen Sie `dotnet new razorclasslib` über die Befehlszeile aus. Zum Beispiel:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Weitere Informationen finden Sie unter [dotnet new](/dotnet/core/tools/dotnet-new).

------
Fügen Sie der RCL Razor-Dateien hinzu.

Es wird empfohlen, RCL-Inhalte im Ordner *Areas* (Bereiche) abzulegen. 


## <a name="referencing-razor-class-library-content"></a>Verweisen auf RCL-Inhalte

Folgende Komponenten können auf die RCL verweisen:

* NuGet-Pakete. Weitere Informationen finden Sie unter [Creating NuGet packages](/nuget/create-packages/creating-a-package) (Erstellen von NuGet-Paketen), [dotnet add package](/dotnet/core/tools/dotnet-add-package) und [Erstellen und Veröffentlichen eines NuGet-Pakets](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*-Dateien. Weitere Informationen finden Sie unter [dotnet add reference](/dotnet/core/tools/dotnet-add-reference).

### <a name="partial-files-access-in-the-rcl"></a>Zugriff auf Teildateien in der RCL

Die ASP.NET Core-Laufzeit sucht im Fall von Inhalten, die sich außerhalb der RCL befinden, nicht nach Teildateien in der RCL.

Im Beispieldownload kann innerhalb von *WebApp1\Pages\About.cshtml* beispielsweise **nicht** auf die Teilansicht *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* verwiesen werden. Seiten in der RCL (*RazorUIClassLib /*) **können hingegen durchaus** auf *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* zugreifen.

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Exemplarische Vorgehensweise: Erstellen eines RCL-Projekts und Verwenden des Projekts über ein Razor-Seiten-Projekt

Sie können das [vollständige Projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) herunterladen und testen, anstatt es zu erstellen. Der Beispieldownload enthält zusätzlichen Code sowie Links, die das Testen des Projekts erleichtern. [In diesem GitHub-Issue](https://github.com/aspnet/Docs/issues/6098) können Sie in Form von Kommentaren Feedback zu Unterschieden zwischen Beispieldownloads und ausführlichen Anleitungen geben.

### <a name="test-the-download-app"></a>Testen der heruntergeladenen App

Wenn Sie die vollständige App nicht heruntergeladen haben und das Beispielprojekt selbst erstellen möchten, können Sie mit dem [nächsten Abschnitt](#create-a-razor-class-library) fortfahren.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Öffnen Sie die *SLN*-Datei in Visual Studio. Führen Sie die App aus.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Erstellen Sie über die Eingabeaufforderung im *cli*-Verzeichnis die RCL und die Web-App.

``` CLI
dotnet build
```

Wechseln Sie ins Verzeichnis *WebApp1*, und führen Sie die App aus:

``` CLI
dotnet run
```
------

Folgen Sie den Anweisungen in [Testen des WebApp1-Projekts](#test).

## <a name="create-a-razor-class-library"></a>Erstellen einer RCL

In diesem Abschnitt wird das Erstellen einer RCL beschrieben. Dabei werden der RCL Razor-Dateien hinzugefügt.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Erstellen Sie das RCL-Projekt:

* Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.
* Wählen Sie **ASP.NET Core-Webanwendung** aus.
* Legen Sie als Namen für die App **RazorUIClassLib** fest.
* Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.
* Klicken Sie auf **Razor Class Library** (Razor-Klassenbibliothek) > **OK**.

Erstellen Sie die Web-App mit Razor-Seiten:

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe und anschließend auf **Hinzufügen** >  **Neues Projekt**.
* Wählen Sie **ASP.NET Core-Webanwendung** aus.
* Legen Sie als Namen für die App **WebApp1** fest.
* Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.
* Klicken Sie auf **Webanwendung** > **OK**.

### <a name="add-razor-files-and-folders-to-the-project"></a>Fügen Sie dem Projekt Razor-Dateien und -Ordner hinzu:

* Fügen Sie eine Razor-Datei für die Teilansicht mit dem Namen „_Message.cshtml“ unter *RazorUIClassLib/Areas/MyFeature/Pages/Shared* hinzu.
* Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* durch den folgenden Code:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Kopieren Sie die Datei *_ViewStart.cshtml* aus dem WebApp1-Projekt nach *RazorUIClassLib/Areas/MyFeature/Pages*.

  Die Datei [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) ist für die Nutzung des Layouts des Razor-Seiten-Projekts erforderlich.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Führen Sie die folgenden Befehle über die Befehlszeile aus:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Die obenstehenden Befehle haben folgende Konsequenzen:

* Die `RazorUIClassLib`-RCL wird erstellt.
* Die Seite „Razor_Message“ wird erstellt und der RCL hinzugefügt. Durch den `-np`-Parameter wird die Seite ohne `PageModel` erstellt.
* Die Datei [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) wird erstellt und der RCL hinzugefügt.

Die Datei „_ViewStart.cshtml“ ist erforderlich, um das Layout des Razor-Seiten-Projekts (das im nächsten Abschnitt hinzugefügt wird) verwenden zu können.

Aktualisieren Sie die Razor-Seiten wie folgt:

* Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* durch den folgenden Code:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* durch den folgenden Code:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` ist erforderlich, um die Teilansicht `<partial name="_Message" />` verwenden zu können. Wenn Sie nicht die `@addTagHelper`-Anweisung einbinden möchten, können Sie die Datei *_ViewImports.cshtml* hinzufügen. Zum Beispiel:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Weitere Informationen zu „_ViewImports.cshtml“ finden Sie unter [Importieren gemeinsam verwendeter Anweisungen](xref:mvc/views/layout#importing-shared-directives).

* Erstellen Sie die Klassenbibliothek, um sicherzustellen, dass keine Compilerfehler vorliegen:

``` CLI
dotnet build RazorUIClassLib
```

Die Buildausgabe enthält *RazorUIClassLib.dll* und *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* enthält den kompilierten Razor-Inhalt.

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Verwenden der Razor-Benutzeroberflächenbibliothek über ein Razor-Seiten-Projekt

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Als Startprojekt festlegen**.
* Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Buildabhängigkeiten** > **Projektabhängigkeiten**.
* Aktivieren Sie für **WebApp1** die Abhängigkeit **RazorUIClassLib**.
* Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Hinzufügen** > **Verweis**.
* Aktivieren Sie im Dialogfeld **Verweis-Manager** das Kontrollkästchen neben **RazorUIClassLib**, und klicken Sie anschließend auf **OK**.

Führen Sie die App aus.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Erstellen Sie eine Web-App mit Razor-Seiten und eine Projektmappendatei, die die Razor-Seiten-App und die RCL enthält:

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Erstellen Sie die Web-App, und führen Sie sie aus:

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a>Testen des WebApp1-Projekts

Überprüfen Sie, ob die Razor-Benutzeroberflächenbibliothek verwendet wird.

* Wechseln Sie zu `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Überschreiben von Ansichten, Teilansichten und Seiten

Wenn eine Ansicht, Teilansicht oder Razor-Seite sowohl in der Web-App als auch in der RCL enthalten ist, hat das Razor-Markup (die *CSHTML*-Datei) in der Web-App Vorrang. Wenn Sie z.B. *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* zu WebApp1 hinzufügen, hat Page1 in WebApp1 Vorrang vor Page1 in der RCL.

Ob Page1 in WebApp1 tatsächlich die Priorität zugeordnet wird, können Sie testen, indem Sie im Beispieldownload *WebApp1/Areas/MyFeature2* in *WebApp1/Areas/MyFeature* umbenennen.

Kopieren Sie die Teilansicht *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* nach *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Aktualisieren Sie das Markup, um den neuen Speicherort anzugeben. Erstellen Sie die App, und führen Sie sie aus, um sicherzustellen, dass die Version mit der Teilansicht der App verwendet wird.
