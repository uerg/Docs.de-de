---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Erstellen Sie ein neues ASP.NET MVC-Projekt | Microsoft Docs
author: microsoft
description: Schritt 1 wird gezeigt, wie die grundlegenden NerdDinner Anwendungsstruktur an Position eingefügt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869256"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="7063e-103">Erstellen eines neuen ASP.NET MVC-Projekts</span><span class="sxs-lookup"><span data-stu-id="7063e-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="7063e-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7063e-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="7063e-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="7063e-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="7063e-106">Dies ist Schritt 1 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.</span><span class="sxs-lookup"><span data-stu-id="7063e-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="7063e-107">Schritt 1 wird gezeigt, wie die grundlegenden NerdDinner Anwendungsstruktur an Position eingefügt.</span><span class="sxs-lookup"><span data-stu-id="7063e-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="7063e-108">Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.</span><span class="sxs-lookup"><span data-stu-id="7063e-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="7063e-109">NerdDinner, Schritt 1: Datei -&gt;neues Projekt</span><span class="sxs-lookup"><span data-stu-id="7063e-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="7063e-110">Wir beginnen, unsere NerdDinner Anwendung durch Auswählen der **Datei -&gt;neues Projekt** Menüelement in Visual Studio 2008 oder die kostenlose Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="7063e-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="7063e-111">Hierdurch wird das Dialogfeld "Neues Projekt" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7063e-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="7063e-112">Um eine neue ASP.NET MVC-Anwendung zu erstellen, müssen wir wählen Sie den Knoten "Web" auf der linken Seite des Dialogfelds und wählen Sie dann die Projektvorlage "ASP.NET MVC-Webanwendung" auf der rechten Seite:</span><span class="sxs-lookup"><span data-stu-id="7063e-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="7063e-113">*Wichtig: Stellen Sie sicher, dass Sie heruntergeladen und ASP.NET MVC – andernfalls es in das Dialogfeld "Neues Projekt" angezeigt wird nicht installiert haben. Können Sie V2 von der [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) , wenn Sie es noch nicht installiert haben (ASP.NET MVC finden Sie in der "Web Platform -&gt;Frameworks und Laufzeiten" Abschnitt).*</span><span class="sxs-lookup"><span data-stu-id="7063e-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="7063e-114">Das neue Projekt Namen, die wir werden zum Erstellen von "NerdDinner", und klicken Sie dann auf die Schaltfläche "ok", um ihn zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7063e-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="7063e-115">Beim Klicken auf "ok" wird Visual Studio ein Dialogfeld angezeigt, die wir erstellen Sie ein Komponententestprojekt für die neue Anwendung auch optional auffordert.</span><span class="sxs-lookup"><span data-stu-id="7063e-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="7063e-116">Testprojekt für diese Komponente ermöglicht es, zu der automatisierten Tests zu erstellen, durch die die Funktionalität und Verhalten der Anwendung überprüft (etwas wir behandeln wie weiter unten in diesem Lernprogramm Aufgabe).</span><span class="sxs-lookup"><span data-stu-id="7063e-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="7063e-117">Die "Testframework" Dropdownliste oben im Dialogfeld wird aufgefüllt, alle verfügbaren ASP.NET-MVC Unit Test Projektvorlagen auf dem Computer installiert.</span><span class="sxs-lookup"><span data-stu-id="7063e-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="7063e-118">Versionen können für MBUnit, NUnit und XUnit heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="7063e-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="7063e-119">Die integrierte Visual Studio-Komponententest-Frameworks wird ebenfalls unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7063e-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="7063e-120">*Hinweis: Die Visual Studio-Komponententestframework ist nur mit Visual Studio 2008 Professional und höheren Versionen verfügbar. Wenn Sie Visual Studio 2008 Standard Edition oder Visual Web Developer 2008 Express verwenden, müssen Sie herunterladen und installieren die Erweiterungen NUnit, MBUnit oder XUnit für ASP.NET MVC, in der Reihenfolge für diesen Dialog angezeigt werden. Das Dialogfeld wird nicht angezeigt, wenn keine Testframeworks installiert.*</span><span class="sxs-lookup"><span data-stu-id="7063e-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="7063e-121">Wir verwenden den Standardnamen für die "NerdDinner.Tests" für das Testprojekt, das wir erstellen, und verwenden Sie die Option für "Visual Studio Einheit testen" Framework.</span><span class="sxs-lookup"><span data-stu-id="7063e-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="7063e-122">Beim Klicken auf erstellt die Schaltfläche "ok" Visual Studio eine Lösung für uns darin - eine für die Webanwendung und eine für unsere Komponententests zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="7063e-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="7063e-123">Untersuchen die Verzeichnisstruktur NerdDinner</span><span class="sxs-lookup"><span data-stu-id="7063e-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="7063e-124">Wenn Sie eine neue ASP.NET MVC-Anwendung mit Visual Studio erstellen, fügt automatisch eine Reihe von Dateien und Verzeichnisse zum Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="7063e-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="7063e-125">ASP.NET MVC-Projekte in der Standardeinstellung sind Verzeichnisse vorhanden sechs auf oberster Ebene:</span><span class="sxs-lookup"><span data-stu-id="7063e-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="7063e-126">**Verzeichnis**</span><span class="sxs-lookup"><span data-stu-id="7063e-126">**Directory**</span></span> | <span data-ttu-id="7063e-127">**Purpose**</span><span class="sxs-lookup"><span data-stu-id="7063e-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="7063e-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="7063e-128">**/Controllers**</span></span> | <span data-ttu-id="7063e-129">Wo Sie Controllerklassen platziert, die URL-Anforderungen zu verarbeiten</span><span class="sxs-lookup"><span data-stu-id="7063e-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="7063e-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="7063e-130">**/Models**</span></span> | <span data-ttu-id="7063e-131">Legen Sie Sie, in denen Klassen, die darstellen und Bearbeiten von Daten</span><span class="sxs-lookup"><span data-stu-id="7063e-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="7063e-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="7063e-132">**/Views**</span></span> | <span data-ttu-id="7063e-133">In dem Sie Benutzeroberflächen-Vorlagendateien einfügen, die für renderingausgabe zuständig sind</span><span class="sxs-lookup"><span data-stu-id="7063e-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="7063e-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="7063e-134">**/Scripts**</span></span> | <span data-ttu-id="7063e-135">Hier können Sie JavaScript-Bibliothek-Dateien und Skripts (. js) einfügen</span><span class="sxs-lookup"><span data-stu-id="7063e-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="7063e-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="7063e-136">**/Content**</span></span> | <span data-ttu-id="7063e-137">Hier können Sie CSS- und Bilddateien und anderen nicht-dynamische/nicht-JavaScript-Inhalt einfügen</span><span class="sxs-lookup"><span data-stu-id="7063e-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="7063e-138">**/App\_Data**</span><span class="sxs-lookup"><span data-stu-id="7063e-138">**/App\_Data**</span></span> | <span data-ttu-id="7063e-139">Sofern Sie Datendateien gespeichert, Lese-/Schreibzugriff werden soll.</span><span class="sxs-lookup"><span data-stu-id="7063e-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="7063e-140">ASP.NET MVC ist diese Struktur nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7063e-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="7063e-141">In der Tat Entwickler, die auf große Anwendungen arbeiten in der Regel unterteilt die Anwendung oben an verschiedenen Projekten, die es mehr verwaltbar zu machen (z. B.: Modellklassen Daten gehen Sie jedoch oftmals in einem separaten Klassenbibliotheksprojekt aus der Webanwendung).</span><span class="sxs-lookup"><span data-stu-id="7063e-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="7063e-142">Die Standardprojektstruktur bieten jedoch nice Directory Standardkonvention, mit denen wir unsere anwendungsaspekten sauber zu halten.</span><span class="sxs-lookup"><span data-stu-id="7063e-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="7063e-143">Wenn wir das Verzeichnis /Controllers erweitern fest wir, dass Visual Studio dem Projekt zwei Controllerklassen – HomeController "und" AccountController – standardmäßig hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="7063e-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="7063e-144">Wenn wir das Verzeichnis /Views erweitern, finden wir drei Unterverzeichnisse – / Home, AccountType und Beispiele – sowie mehrere Vorlage, die darin enthaltenen Dateien standardmäßig ebenfalls dem Projekt hinzugefügt wurden:</span><span class="sxs-lookup"><span data-stu-id="7063e-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="7063e-145">Wenn wir die Scripts-Verzeichnisse und/Content erweitern, wird erkennbar, dass eine "Site.CSS" ändern-Datei, die zum Formatieren von HTML stets auf dem Standort verwendet wird, als auch JavaScript-Bibliotheken, die ASP.NET AJAX und jQuery aktivieren können innerhalb der Anwendung unterstützen:</span><span class="sxs-lookup"><span data-stu-id="7063e-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="7063e-146">Wenn wir das Projekt NerdDinner.Tests erweitern finden wir zwei Klassen, die Komponententests für unsere Controllerklassen enthalten:</span><span class="sxs-lookup"><span data-stu-id="7063e-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="7063e-147">Diese Standarddateien hinzugefügt, die von Visual Studio bieten uns eine grundlegende Struktur für eine funktionierende Anwendung - mit Startseite zur Seite, die Anmeldung/Abmeldung/Registrierung Seiten Konto und ein nicht behandelter Fehler-Seite (alle wired von Volltextkatalogen und direktes arbeiten) abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="7063e-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="7063e-148">Ausführen der Anwendung NerdDinner</span><span class="sxs-lookup"><span data-stu-id="7063e-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="7063e-149">Wir können das Projekt ausführen, indem Sie die Auswahl entweder die **Debug -&gt;Debuggen** oder **Debug -&gt;Starten ohne Debugging** Menüelemente:</span><span class="sxs-lookup"><span data-stu-id="7063e-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="7063e-150">Dies wird starten integrierte ASP.NET Web-Server, der mit Visual Studio enthalten ist, und führen Sie die Anwendung:</span><span class="sxs-lookup"><span data-stu-id="7063e-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="7063e-151">Im folgenden ist die Startseite für unser neues Projekt (URL: "/") bei der Ausführung:</span><span class="sxs-lookup"><span data-stu-id="7063e-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="7063e-152">Klicken auf die Registerkarte "About" wird eine zu Seite (URL: "/ Home oder bald"):</span><span class="sxs-lookup"><span data-stu-id="7063e-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="7063e-153">Auf den Link "Anmelden", in der oberen rechten Ecke, führt zu einer Anmeldeseite (URL: "/ Account/LogOn")</span><span class="sxs-lookup"><span data-stu-id="7063e-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="7063e-154">Wenn wir haben ein Anmeldekonto klicken wir den Link Register (URL: "/ Account/Register") zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="7063e-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="7063e-155">Der Code zum Implementieren der oben genannten Home, und die Abmeldung / register Funktionalität wurde in der Standardeinstellung hinzugefügt, wenn wir unser neue Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="7063e-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="7063e-156">Wir werden sie als Ausgangspunkt der Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="7063e-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="7063e-157">Testen der Anwendung NerdDinner</span><span class="sxs-lookup"><span data-stu-id="7063e-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="7063e-158">Wenn wir die Professional Edition oder eine höhere Version von Visual Studio 2008 verwenden, können wir die integrierten innerhalb von Visual Studio-IDE-Unterstützung für Komponententests, zum Testen des Projekts:</span><span class="sxs-lookup"><span data-stu-id="7063e-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="7063e-159">Auswahl einer der oben erwähnten Optionen wird im Bereich "Testergebnisse" in der IDE öffnen, und geben Sie uns mit erfolgreich/Fehler-Status für die 27 Komponententests in unserer neuen Projekt enthalten, die die integrierte Funktion abdecken:</span><span class="sxs-lookup"><span data-stu-id="7063e-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="7063e-160">Weiter unten in diesem Lernprogramm wir sprechen Sie mehr über automatisierte Tests und fügen zusätzliche Komponententests, die die Anwendungsfunktionalität abdecken, die wir implementieren.</span><span class="sxs-lookup"><span data-stu-id="7063e-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="7063e-161">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="7063e-161">Next Step</span></span>

<span data-ttu-id="7063e-162">Wir haben jetzt eine grundlegende Anwendungsstruktur vorhanden.</span><span class="sxs-lookup"><span data-stu-id="7063e-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="7063e-163">Nehmen wir jetzt [Erstellen einer Datenbank zum Speichern von unseren Anwendungsdaten](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="7063e-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7063e-164">[Zurück](introducing-the-nerddinner-tutorial.md)
> [Weiter](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="7063e-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
