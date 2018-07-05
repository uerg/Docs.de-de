---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Einzelseitenanwendung: Knockout.js-Vorlage | Microsoft-Dokumentation'
author: MikeWasson
description: Knockout-Vorlage
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 9e48c7033cefe8b6e4c456577b7a6448f9815d0b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832528"
---
<a name="single-page-application-knockoutjs-template"></a>Einzelseitenanwendung: Knockout.js-Vorlage
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Die Knockout-MVC-Vorlage ist Bestandteil von ASP.NET und Web Tools 2012.2
> 
> [Herunterladen von ASP.NET und Webtools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


Das ASP.NET und Web Tools 2012.2-Update enthält eine Vorlage (Single-Page Application, SPA) für ASP.NET MVC 4. Diese Vorlage wurde entwickelt, um Ihnen den Einstieg, schnellen Erstellen von interaktiven clientseitigen Web-apps zu erhalten.

"Single-Page Application" (SPA) ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt, und klicken Sie dann die Seite wird dynamisch aktualisiert, anstatt neue Seiten laden. Nach dem ersten Laden der Seite werden die SPA mit dem Server über AJAX-Anforderungen.

![](knockoutjs-template/_static/image1.png)

AJAX ist nichts neues, aber heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu erleichtern. Darüber hinaus sind HTML5 und CSS3 jetzt zum Erstellen umfassender Benutzeroberflächen einfacher.

Um Ihnen den Einstieg zu erleichtern, erstellt die SPA-Vorlage eine "Aufgabenliste"-beispielanwendung. In diesem Tutorial Unternehmen wir eine Einführung in der Vorlage. Wir werden zunächst sehen Sie sich die Aufgaben-App selbst und dann überprüfen Sie die Technologien mit sich bringen, die dafür erforderlichen.

## <a name="create-a-new-spa-template-project"></a>Erstellen eines neuen Projekts der SPA-Vorlage

Anforderungen:

- Visual Studio 2012 oder Visual Studio Express 2012, für das Web
- ASP.NET Web Tools 2012.2 zu aktualisieren. Sie können das Update installieren [hier](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Starten Sie Visual Studio, und wählen Sie **neues Projekt** von der Startseite. Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

![](knockoutjs-template/_static/image2.png)

In der **neues Projekt** Assistenten **Single Page Application**.

![](knockoutjs-template/_static/image3.png)

Drücken Sie F5, um die Anwendung zu erstellen und auszuführen. Wenn die Anwendung zuerst ausgeführt wird, wird einen Anmeldebildschirm angezeigt.

![](knockoutjs-template/_static/image4.png)

Klicken Sie auf die &quot;registrieren&quot; verknüpfen, und erstellen Sie einen neuen Benutzer.

![](knockoutjs-template/_static/image5.png)

Nachdem Sie sich angemeldet haben, erstellt die Anwendung eine Standard-Todo-Liste mit zwei Elementen. Klicken Sie auf "Todo-Liste hinzufügen" zum Hinzufügen einer neuen Liste.

![](knockoutjs-template/_static/image6.png)

Benennen Sie die Liste, die Liste Elemente hinzugefügt und diese abhaken. Sie können auch Elemente löschen oder eine vollständige Liste zu löschen. Die Änderungen werden automatisch in einer Datenbank auf dem Server beibehalten (tatsächlich LocalDB an diesem Punkt, da Sie die Anwendung lokal ausgeführt werden).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektur der SPA-Vorlage

Dieses Diagramm zeigt die wichtigsten Bausteine für die Anwendung.

![](knockoutjs-template/_static/image8.png)

Auf dem Server ASP.NET MVC dient den HTML-Code und verarbeitet auch formularbasierte Authentifizierung.

ASP.NET Web-API verarbeitet alle Anforderungen, die im Zusammenhang mit der ToDoLists und ToDoItems, einschließlich abrufen, erstellen, aktualisieren und löschen. Der Client tauscht Daten mit Web-API im JSON-Format.

Entity Framework (EF) ist der O/RM-Ebene. Es dient als Mittler zwischen der objektorientierten Welt von ASP.NET und der zugrunde liegenden Datenbank. Die Datenbank verwendet LocalDB, aber Sie können dies in der Datei "Web.config" ändern. In der Regel würden Sie LocalDB verwenden, für die lokale Entwicklung und stellen anschließend mit einer SQL-Datenbank auf dem Server mit EF Code First-Migration.

Klicken Sie auf der Clientseite verarbeitet die Knockout.js-Bibliothek Seitenupdates von AJAX-Anforderungen. Knockout verwendet Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren. Auf diese Weise müssen Sie keinen Code schreiben, die erläutert, der JSON-Daten, und aktualisiert das DOM. Setzen Sie stattdessen deklarativen Attribute im HTML-Code, die Knockout anweisen, wie Sie die Daten zu präsentieren.

Ein großer Vorteil dieser Architektur ist, da die Darstellungsschicht von der Anwendungslogik getrennt. Sie können den Teil der Web-API erstellen, ohne zu wissen, wie Ihre Webseite aussehen wird nichts. Auf dem Client, erstellen Sie ein "Ansichtsmodell" auf diese Daten darstellt, und das Ansichtsmodell verwendet Knockout an HTML-Code binden. Mit der Sie einfach den HTML-Code ändern, ohne dass das Ansichtsmodell. (Wir Knockout etwas später betrachten.)

## <a name="models"></a>Modelle

In Visual Studio-Projekts enthält der Ordner "Models" die Modelle, die auf dem Server verwendet werden. (Es gibt auch Modelle auf der Clientseite, wir kommen die.)

![](knockoutjs-template/_static/image9.png)

**TodoItem "Todolist"**

Hierbei handelt es sich um die Datenbankmodelle für Entity Framework Code First. Beachten Sie, dass diese Modelle Eigenschaften verfügen, die aufeinander zeigen. `ToDoList` enthält eine Auflistung von Aufgabenelementen, und jedes `ToDoItem` enthält einen Verweis auf das übergeordnete Element "Todolist" zurück. Diese Eigenschaften werden als Navigationseigenschaften, und sie eine Aufgabenliste und dessen to-do-Elemente, der 1: n Beziehung darstellen.

Die `ToDoItem` -Klasse auch das **[ForeignKey]** Attribut, um anzugeben, dass `ToDoListId` ist ein Fremdschlüssel in der `ToDoList` Tabelle. Dadurch wird EF eine foreign Key-Einschränkung mit der Datenbank hinzufügen.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Diese Klassen definieren die Daten, die an den Client gesendet werden. "DTO" steht für "Data Transfer Object". Das DTO wird definiert, wie die Entitäten in JSON serialisiert werden. Es gibt im Allgemeinen mehrere Gründe für die Verwendung von DTOs:

- Um zu steuern, welche Eigenschaften serialisiert werden. Das DTO kann es sich um eine Teilmenge der Eigenschaften aus dem Domänenmodell enthalten. Dies ist sinnvoll aus Sicherheitsgründen (um sensible Daten ausblenden) oder einfach um die Menge der Daten zu reduzieren, die Sie senden.
- So ändern Sie die Form der Daten – z. B. eine komplexere Datenstruktur zu vereinfachen.
- Um eine beliebige Geschäftslogik, die aus der DTO (Trennung von Belangen) zu halten.
- Wenn Ihre Domänenmodelle aus irgendeinem Grund nicht serialisiert werden können. Z. B. Zirkelverweise können Probleme verursachen, bei der Serialisierung eines Objekts gibt es Möglichkeiten zur Behebung dieses Problems in Web-API (finden Sie unter [zirkuläre Objektverweise behandeln](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); aber ein DTO mit einfach vollständig vermeidet das Problem.

Klicken Sie in die SPA-Vorlage die DTOs die gleichen Daten wie der Domänenmodelle enthält. Sie sind jedoch nach wie vor nützlich, da sie vermeiden, dass die Navigationseigenschaften von Zirkelverweisen, und sie das allgemeine DTO-Muster veranschaulichen.

**AccountModels.cs**

Diese Datei enthält die Modelle für die Standortmitgliedschaft. Die `UserProfile` Klasse definiert das Schema für Benutzerprofile in die Mitgliedschaftsdatenbank. (In diesem Fall ist die einzige Information, die Benutzer-ID und den Benutzernamen ein.) Die anderen Modellklassen in dieser Datei werden verwendet, um den Benutzer die Registrierung und Anmeldung Forms erstellen.

## <a name="entity-framework"></a>Entity Framework

Die SPA-Vorlage verwendet EF Code First. In Code First-Entwicklung Sie definieren die Modelle zuerst im Code und EF verwendet dann das Modell, um die Datenbank zu erstellen. Sie können auch die EF verwenden, mit einer vorhandenen Datenbank ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

Die `TodoItemContext` Klasse im Ordner "Models" leitet sich von **"DbContext"**. Diese Klasse stellt die "Glue" zwischen den Modellen und EF. Die `TodoItemContext` enthält eine `ToDoItem` Auflistung und ein `TodoList` Auflistung. Um die Datenbank abzufragen, Schreiben Sie einfach eine LINQ-Abfrage für diese Sammlungen. Beispielsweise sieht aus wie Sie die to-do-Listen für den Benutzer "Alice" Alle auswählen:

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Sie können auch neue Elemente zur Auflistung hinzufügen, Elemente, aktualisieren oder löschen Sie Elemente aus der Auflistung und die Änderungen dauerhaft in der Datenbank.

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web-API-Controller

In ASP.NET Web-API sind die Controller-Objekte, die HTTP-Anforderungen verarbeiten. Wie bereits erwähnt, verwendet die SPA-Vorlage Web-API zum Aktivieren von CRUD-Vorgänge auf `ToDoList` und `ToDoItem` Instanzen. Die Controller befinden sich im Ordner "Controllers" der Projektmappe.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Verarbeitet HTTP-Anforderungen für to-do-Elemente
- `TodoListController`: Verarbeitet HTTP-Anforderungen für to-do-Listen.

Diese Namen sind wichtig, da Web-API-URI-Pfad auf dem Controllernamen entspricht. (Um zu erfahren, wie Web-API-HTTP-Anforderungen mit Controllern weiterleitet, finden Sie unter [Routing in ASP.NET Web-API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Sehen wir uns die `ToDoListController` Klasse. Es enthält ein einzelnes Datenelement:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Die `TodoItemContext` wird verwendet, um die Kommunikation mit EF, wie oben beschrieben. Die Methoden auf dem Controller implementieren die CRUD-Vorgänge. Web-API ordnet HTTP-Anforderungen vom Client an die Controllermethoden, wie folgt aus:

| HTTP-Anforderung | Controller-Methode | Beschreibung |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Ruft eine Auflistung von to-do-Listen. |
| GET /api/todo/*id* | `GetTodoList` | Ruft eine to-Do-Liste nach ID ab |
| PUT /api/todo/*id* | `PutTodoList` | Aktualisiert eine to-Do-Liste. |
| POST /api/todo | `PostTodoList` | Erstellt eine neue to-Do-Liste an. |
| DELETE /api/todo/*id* | `DeleteTodoList` | Löscht eine TODO-Liste. |

Beachten Sie, dass die URIs für bestimmte Vorgänge Platzhalter für den ID-Wert enthalten. Um eine zu Listen mit 42-ID zu löschen, z. B. der URI ist `/api/todo/42`.

Weitere Informationen zur Verwendung von Web-API für CRUD-Vorgänge finden Sie unter [erstellen eine Web-API, CRUD-Vorgänge unterstützt](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Der Code für diesen Controller ist recht einfach. Hier sind einige interessante Punkte:

- Die `GetTodoLists` Methode verwendet eine LINQ-Abfrage zum Filtern der Ergebnisse durch die ID des angemeldeten Benutzers. Auf diese Weise werden die einem Benutzer nur die Daten, die davon gehört. Beachten Sie, dass eine Select-Anweisung verwendet wird, konvertiert die `ToDoList` in Instanzen `TodoListDto` Instanzen.
- Die PUT- und POST-Methoden überprüfen den Modellzustand vor dem Ändern der Datenbank. Wenn **ModelState.IsValid** ist "false", diese Methoden geben HTTP 400 Ungültige Anforderung zurück. Weitere Informationen zur modellvalidierung in der Web-API am [Modellvalidierung](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Die Controller-Klasse wird auch mit ergänzt die **[Authorize]** Attribut. Dieses Attribut überprüft, ob die HTTP-Anforderung authentifiziert ist. Wenn die Anforderung nicht authentifiziert ist, wird der Client empfängt HTTP-401 nicht autorisiert. Weitere Informationen zur Authentifizierung bei der [Authentifizierung und Autorisierung in ASP.NET Web-API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Die `TodoController` Klasse ähnelt `TodoListController`. Der größte Unterschied ist, dass es keine GET-Methoden, nicht definiert ist, da der Client die to-do-Elemente sowie einzelnen to-Do-Liste erhält.

## <a name="mvc-controllers-and-views"></a>MVC-Controllern und Ansichten

Die MVC-Controller befinden sich ebenfalls im Ordner "Controller" der Projektmappe. `HomeController` Rendert die HTML-Hauptdatei der Anwendung an. Die Ansicht für den Home-Controller ist in Views/Home/Index.cshtml definiert. Die Home-Ansicht rendert unterschiedliche Inhalte, abhängig davon, ob der Benutzer angemeldet ist:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Wenn Benutzer angemeldet sind, die Hauptbenutzeroberfläche angezeigt. Andernfalls wird den Bereich für die Anmeldung angezeigt. Beachten Sie, dass diese bedingte Rendering auf dem Server erfolgt. Nie versuchen, Ausblenden von vertraulichen Inhalt auf dem Client & #8212anything, die Sie in einer HTTP-Antwort senden wird angezeigt, um eine Person, die die unformatierten HTTP-Nachrichten überwacht wird.

## <a name="client-side-javascript-and-knockoutjs"></a>Clientseitige JavaScript-Code und "Knockout.js"

Jetzt aktivieren wir von der Serverseite der Anwendung an den Client. Die SPA-Vorlage verwendet eine Kombination von jQuery und "Knockout.js", um eine glatte, interaktive Benutzeroberfläche zu erstellen. Knockout.js ist eine JavaScript-Bibliothek, die einfach HTML an Daten gebunden werden können. "Knockout.js" verwendet ein Muster, die Namen "Model-View-ViewModel".

- Das Modell ist die Domänendaten (ToDo-Listen und ToDo-Elemente).
- Die Ansicht ist das HTML-Dokument.
- Das Ansichtsmodell ist ein JavaScript-Objekt, das Modelldaten enthält. Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche. Es wurde keine Kenntnisse über die HTML-Darstellung. Stattdessen stellt es sich um abstrakte Funktionen in der Ansicht, z. B. "eine Liste der ToDo-Elemente".

Die Ansicht an das Ansichtsmodell datengebunden ist. Updates an das Ansichtsmodell werden in der Ansicht automatisch wiedergegeben. Bindungen arbeiten, die andere Richtung möglich. Ereignisse in das DOM (z. B. klickt) sind eine Datenbindung an Funktionen im Ansichtsmodell, das AJAX-Aufrufe auszulösen.

Die SPA-Vorlage werden die clientseitige JavaScript-Code in drei Ebenen unterteilt:

- TODO.DataContext.js: AJAX-Anforderungen sendet.
- TODO.Model.js: definiert die Modelle.
- TODO.ViewModel.js: definiert das Ansichtsmodell.

![](knockoutjs-template/_static/image11.png)

Diese Skriptdateien befinden sich im Ordner "Scripts/app" der Projektmappe.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** behandelt alle AJAX-Aufrufe an die Web-API-Controller. (Die AJAX-Aufrufe für die Anmeldung werden in ajaxlogin.js an anderer Stelle definiert.)

**TODO.Model.js** definiert, die die clientseitige (Browser) Modelle für die to-do-Listen. Es gibt zwei Modellklassen: TodoItem und der "Todolist".

Viele der Eigenschaften in der ViewModel-Klassen sind vom Typ ""Ko.Observable "". "Observables" bilden wie die Magie von Knockout funktioniert. Von der [Knockout finden](http://knockoutjs.com/documentation/introduction.html): ein beobachtbares Objekt ist ein "JavaScript-Objekt, mit denen Abonnenten über Änderungen benachrichtigt werden kann." Wenn der Wert von einem beobachtbaren Objekt ändert, aktualisiert Knockout HTML-Elemente, die an diesen Observables gebunden sind. TodoItem hat z. B. beobachtbare Objekte für die Eigenschaften für Titel und Netzwerktool:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Sie können auch ein beobachtbares Objekt im Code abonnieren. Z. B. abonniert die TodoItem-Klasse auf Änderungen in den Eigenschaften "Netzwerktool" und "Title" ein:

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Anzeigen des Modells**

Das Ansichtsmodell ist in todo.viewmodel.js definiert. Das Ansichtsmodell ist der zentrale Punkt, in dem die Anwendung die HTML-Seite-Elemente, die Domänendaten bindet. In der SPA-Vorlage enthält das Ansichtsmodell ein observables-Array von TodoLists. Der folgende Code in das Ansichtsmodell teilt Knockout, um die Bindungen anzuwenden:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML und Datenbindung

Die HTML-Hauptdatei für die Seite wird im Views/Home/Index.cshtml definiert. Da wir die Datenbindung verwenden, ist der HTML-Code nur eine Vorlage für was tatsächlich gerendert wird. Knockout verwendet *deklarative* Bindungen. Sie können Seitenelemente an Daten binden, indem das Element ein Attribut "Data-Bind" hinzugefügt. Hier ist ein sehr einfaches Beispiel, das die Knockout-Dokumentation entnommen:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

In diesem Beispiel aktualisiert Knockout den Inhalt der **&lt;umfassen&gt;** Element mit dem Wert des `myItems.count()`. Wenn sich dieser Wert ändert, aktualisiert Knockout das Dokument an.

Knockout bietet es sich um eine Reihe von unterschiedlichen Bindungstypen. Hier sind einige der in der SPA-Vorlage verwendeten Bindungen:

- **Foreach**: können Sie eine Schleife durchlaufen, und wenden dasselbe Markup auf jedes Element in der Liste. Dies wird verwendet, um die to-do-Listen und die to-do-Elemente zu rendern. In der **Foreach**, die Bindungen werden angewendet, auf die Elemente der Liste.
- **sichtbare**: verwendet, um die Sichtbarkeit umzuschalten. Blenden Sie Markup aus, wenn eine Auflistung leer ist, oder stellen Sie die Fehlermeldung angezeigt.
- **Wert**: verwendet, um Formularwerte aufzufüllen.
- **Klicken Sie auf**: ein Click-Ereignis an eine Funktion im Ansichtsmodell gebunden.

## <a name="anti-csrf-protection"></a>Anti-CSRF-Schutz

Cross-Site Request Forgery (CSRF) handelt es sich um einen Angriff, in dem eine schädliche Website sendet eine Anforderung an einer anfälligen Website, in denen der Benutzer zurzeit angemeldet ist. Um CSRF-Angriffe zu vermeiden, verwendet ASP.NET MVC *fälschungssicherheitstoken*, so genannte Anforderung Überprüfung-Token. Die Idee ist, dass der Server ein zufällig generierte Token in eine Webseite versetzt. Wenn der Client Daten an den Server sendet, muss er diesen Wert in der Anforderungsnachricht enthalten.

Fälschungssicherheitstoken funktionieren, da die Token des Benutzers, aufgrund von Richtlinien für denselben Ursprung der schädliche Seite nicht gelesen werden kann. (Richtlinien für denselben Ursprung zu verhindern, dass Dokumente auf zwei verschiedenen Standorten aus den Zugriff auf die Inhalte des jeweils anderen gehostet.)

ASP.NET MVC bietet integrierte Unterstützung für fälschungssicherheitstoken, über die [Antifälschungsunterstützung](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) Klasse und die [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) Attribut. Diese Funktion ist derzeit nicht in Web-API integriert werden. Die SPA-Vorlage enthält jedoch eine benutzerdefinierte Implementierung für Web-API. Dieser Code wird definiert, der `ValidateHttpAntiForgeryTokenAttribute` -Klasse, die sich im Verzeichnis "Filters" der Projektmappe befindet. Weitere Informationen zu Anti-CSRF in Web-API finden Sie unter [verhindern von Cross-Site Request Forgery (CSRF) Angriffe](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Schlussbemerkung

Die SPA-Vorlage dient für Ihre ersten Schritte schnell schreiben moderner, interaktiver Webanwendungen. Er verwendet die Knockout.js-Bibliothek, um die Darstellung (HTML-Markup) von den Daten und Anwendungslogik zu trennen. Knockout ist jedoch nicht die einzige JavaScript-Bibliothek, die Sie zum Erstellen einer einseitigen Anwendung verwenden können. Wenn Sie einigen anderen Optionen ansehen möchten, sehen Sie sich die [SPA-Vorlagen-Community erstellte](../templates/index.md).
