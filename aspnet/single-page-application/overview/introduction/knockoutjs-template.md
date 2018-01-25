---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Einseitigen-Anwendung: KnockoutJS Vorlage | Microsoft Docs'
author: MikeWasson
description: Knockout-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: e6c0c45bed098a8a1160ff11e4f77244bf55ffd3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="single-page-application-knockoutjs-template"></a>Einseitigen-Anwendung: Die KnockoutJS Vorlage
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Die Knockout MVC-Projektvorlage ist Teil von ASP.NET und Web Tools 2012.2
> 
> [Herunterladen von ASP.NET und Webtools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


Das ASP.NET und Web Tools 2012.2-Update enthält eine Vorlage für Einzelseiten-Anwendung (SPA) für ASP.NET MVC 4. Diese Vorlage dient zum schnellen Erstellen von interaktiven clientseitigen Web-apps Ihnen den Einstieg.

"Einzelseiten Application" (SPA) ist der allgemeine Begriff für eine Webanwendung, die einzelne HTML-Seite geladen und aktualisiert dann die Seite dynamisch, anstatt neue Seiten zu laden. Nach dem ersten Laden der Seite finden Sie die SPA mit dem Server über AJAX-Anforderungen.

![](knockoutjs-template/_static/image1.png)

AJAX ist ein bekanntes allerdings heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu vereinfachen. Darüber hinaus machen HTML 5- und CSS3 sie umfangreiche Benutzeroberflächen erstellen einfacher.

Um Ihnen den Einstieg zu erleichtern, wird eine beispielanwendung für die "To-Do-Liste" der SPA-Vorlage erstellt. In diesem Lernprogramm führen wir eine geführte Tour durch die Vorlage. Zunächst müssen wir sehen Sie sich die Anwendung der to-Do-Liste selbst dann überprüfen Sie die Technologie Komponenten funktionieren.

## <a name="create-a-new-spa-template-project"></a>Erstellen eines neuen Projekts der SPA-Vorlage

Anforderungen:

- Visual Studio 2012 oder Visual Studio Express 2012 für das Web
- ASP.NET Web Tools 2012.2 aktualisieren. Sie können das Update installieren [hier](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Starten Sie Visual Studio, und wählen Sie **neues Projekt** von der Startseite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

![](knockoutjs-template/_static/image2.png)

In der **neues Projekt** Assistenten **einseitigen Anwendung**.

![](knockoutjs-template/_static/image3.png)

Drücken Sie F5, um die Anwendung zu erstellen und auszuführen. Wenn die Anwendung zuerst ausgeführt wird, wird einen Anmeldebildschirm angezeigt.

![](knockoutjs-template/_static/image4.png)

Klicken Sie auf die &quot;registrieren&quot; verknüpfen, und erstellen Sie einen neuen Benutzer.

![](knockoutjs-template/_static/image5.png)

Nachdem Sie sich anmelden, erstellt die Anwendung eine standardmäßigen TODO-Liste mit zwei Elementen. Klicken Sie Sie auf "Add TODO-Liste" So fügen Sie eine neue Liste hinzu.

![](knockoutjs-template/_static/image6.png)

Benennen Sie die Liste, fügen Sie Elemente zur Liste hinzu und Haken Sie diese ab. Sie können auch Elemente löschen oder eine vollständige Liste löschen. Die Änderungen werden automatisch in einer Datenbank auf dem Server beibehalten (tatsächlich LocalDB an diesem Punkt, da die Anwendung lokal ausgeführt wird).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektur der SPA-Vorlage

Dieses Diagramm zeigt die wichtigsten Bausteine für die Anwendung.

![](knockoutjs-template/_static/image8.png)

Auf der Serverseite ASP.NET-MVC dient den HTML-Code und verarbeitet auch die formularbasierte Authentifizierung.

ASP.NET Web API verarbeitet alle Anforderungen, die sich auf die ToDoLists und ToDoItems, einschließlich abrufen, erstellen, aktualisieren und Löschen von beziehen. Der Client tauscht Daten mit Web-API im JSON-Format.

Entity Framework (EF) ist der O/RM-Ebene. Es dient als Mittler zwischen der objektorientierten Welt von ASP.NET und der zugrunde liegenden Datenbank. Die Datenbank verwendet LocalDB, aber Sie können diese in der Datei "Web.config" ändern. In der Regel verwenden Sie LocalDB für lokale Entwicklung und dann mit einer SQL-Datenbank auf dem Server bereitstellen mit EF Code First-Migration.

Auf der Clientseite verarbeitet die Bibliothek Knockout.js Seitenupdates von AJAX-Anforderungen. Knockout verwendet Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren. Auf diese Weise müssen Sie Code schreiben, führt Sie durch die JSON-Daten und aktualisiert das DOM validiert werden. Setzen Sie stattdessen deklarativen Attribute im HTML-Code, der Knockout anweisen, wie die Daten zu präsentieren.

Ein großer Vorteil dieser Architektur ist, dass er die Darstellungsschicht von Anwendungslogik trennt. Sie können den Web-API Teil erstellen, ohne dass Sie wissen Ihrer Webseite Erscheinungsbild. Auf der Clientseite erstellen Sie eine "ViewModel" auf diese Daten darstellen, und das Ansichtsmodell Knockout zum Binden an den HTML-Code verwendet. Mit der Sie einfach den HTML-Code ändern, ohne dass das Modell anzeigen. (Sehen wir uns Knockout etwas höher.)

## <a name="models"></a>Modelle

In Visual Studio-Projekt enthält der Ordner Models die Modelle, die auf dem Server verwendet werden. (Es gibt auch Modelle auf der Clientseite; wir kommen die.)

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Hierbei handelt es sich um die Datenbank formt für Entity Framework Code First. Beachten Sie, dass diese Modelle Eigenschaften aufweisen, die aufeinander zeigen. `ToDoList`enthält eine Auflistung von ToDoItems, und jedes `ToDoItem` verfügt über einen Verweis zurück zum übergeordneten "Todolist". Diese Eigenschaften sind Navigationseigenschaften aufgerufen, und sie eine to-Do-Liste mit den to-do-Elementen, der 1: n-Beziehung darstellen.

Die `ToDoItem` -Klasse auch verwendet die **[ForeignKey]** Attribut angeben, dass `ToDoListId` ist ein Fremdschlüssel in der `ToDoList` Tabelle. Dies teilt EF foreign Key-Einschränkung in der Datenbank hinzufügen.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Diese Klassen definieren die Daten, die an den Client gesendet werden sollen. "DTO" steht für "Datenübertragungsobjekt." Die DTO definiert, wie die Entitäten in JSON serialisiert werden. Im Allgemeinen stehen verschiedene Gründe für das DTOs verwenden:

- Um zu steuern, welche Eigenschaften serialisiert werden. Die DTO kann es sich um eine Teilmenge der Eigenschaften aus dem Domänenmodell enthalten. Sie können dies vornehmen, aus Sicherheitsgründen (um sensible Daten ausblenden) oder einfach um gesendete Datenmenge zu reduzieren.
- So ändern Sie die Form der Daten – z. B. eine komplexere Datenstruktur zu vereinfachen.
- Beliebige Geschäftslogiken, aus der DTO (Trennung von Anliegen) beibehalten.
- Wenn Ihre Domänenmodelle aus irgendeinem Grund nicht serialisiert werden können. Z. B. Zirkelverweise können Probleme verursachen, wenn ein Objekt zu serialisieren, es gibt, Methoden zum Behandeln dieses Problems in Web-API (finden Sie unter [Behandlung von Objekt-Zirkelverweise](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); aber mit einem DTO einfach vollständig vermeidet das Problem.

Klicken Sie in der Vorlage SPA DTOs die gleichen Daten wie die Domänenmodelle enthält. Allerdings sind sie immer noch nützlich, da sie vermeiden Sie Zirkelbezüge von Navigationseigenschaften und nachweisen, dass das allgemeine DTO-Muster.

**AccountModels.cs**

Diese Datei enthält Modelle für die websitemitgliedschaft. Die `UserProfile` Klasse definiert das Schema für Benutzerprofile an der Gruppenmitgliedschaft DB. (In diesem Fall ist die einzige Information der Benutzer-ID und den Benutzernamen ein.) Die anderen Modellklassen in dieser Datei werden verwendet, auf der Benutzer die Registrierung und Anmeldung Formulare erstellen.

## <a name="entity-framework"></a>Entity Framework

EF Code First, wird die SPA-Vorlage verwendet. In Code First-Entwicklung Sie definieren die Modelle zuerst im Code und EF verwendet dann das Modell zum Erstellen der Datenbank. Sie können auch EF mit einer vorhandenen Datenbank ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

Die `TodoItemContext` Klasse im Ordner "Modelle" leitet sich von **DbContext**. Diese Klasse stellt die "Verbindung" zwischen den Modellen und EF bereit. Die `TodoItemContext` enthält eine `ToDoItem` Auflistung und ein `TodoList` Auflistung. Zum Abfragen der Datenbank schreiben Sie einfach eine LINQ-Abfrage für diese Sammlungen. Beispielsweise sieht aus wie alle von der to-do-Listen für Benutzer "Alice" ausgewählt werden können:

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Sie können auch neue Elemente zur Auflistung hinzufügen, Elemente, aktualisieren oder Elemente aus der Auflistung löschen und Beibehalten der Änderungen an der Datenbank.

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web-API-Controller

In ASP.NET Web-API sind Controller Objekte, die HTTP-Anforderungen zu verarbeiten. Wie bereits erwähnt, verwendet die Vorlage SPA Web-API zum Aktivieren von CRUD-Vorgänge auf `ToDoList` und `ToDoItem` Instanzen. Der Domänencontroller befinden sich im Ordner der Projektmappe.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Verarbeitet HTTP-Anforderungen für Aufgaben
- `TodoListController`: Http-Anforderungen für Listen mit Aufgaben behandelt.

Diese Namen sind erheblich, da die Web-API den URI-Pfad zu dem Controllernamen entspricht. (Wie Web-API-HTTP-Anfragen an Domänencontroller weiterleitet finden Sie unter [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Sehen wir uns die `ToDoListController` Klasse. Es enthält ein einzelnes Datenelement:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Die `TodoItemContext` wird verwendet, um die Kommunikation mit EF, wie zuvor beschrieben. Die Methoden auf dem Controller implementieren die CRUD-Vorgänge. Web-API werden die HTTP-Anforderungen vom Client an Controllermethoden, folgendermaßen zugeordnet:

| HTTP-Anforderung | Controllermethode | Beschreibung |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Ruft eine Auflistung von Listen mit Aufgaben ab. |
| GET /api/todo/*id* | `GetTodoList` | Ruft eine to-Do-Liste nach ID ab |
| PUT /api/todo/*id* | `PutTodoList` | Aktualisiert eine to-Do-Liste. |
| POST /api/todo | `PostTodoList` | Erstellt eine neue to-Do-Liste. |
| DELETE /api/todo/*id* | `DeleteTodoList` | Löscht eine TODO-Liste. |

Beachten Sie, dass die URIs für bestimmte Vorgänge Platzhalter für den ID-Wert enthalten. Um eine-Liste mit der ID 42 zu löschen, z. B. der URI ist `/api/todo/42`.

Weitere Informationen zur Verwendung von Web-API für CRUD-Vorgänge finden Sie unter [erstellen eine Web-API, CRUD-Vorgänge unterstützt](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Der Code für diesen Controller ist recht einfach. Hier sind einige interessante Punkte:

- Die `GetTodoLists` Methode verwendet eine LINQ-Abfrage zum Filtern der Ergebnisse mit der ID des angemeldeten Benutzers. Auf diese Weise kann ein Benutzer nur die Daten, die davon gehört sehen. Beachten Sie, dass eine Select-Anweisung verwendet wird, konvertiert der `ToDoList` -Instanzen in `TodoListDto` Instanzen.
- Die Methoden PUT und POST überprüfen den Modellzustand vor der Änderung der Datenbank. Wenn **ModelState.IsValid** lautet "false", diese Methoden geben HTTP 400 Ungültige Anforderung. Weitere Informationen zu modellüberprüfungen im Web-API bei [Modellvalidierung](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Die Controllerklasse ergänzt wird auch mit der **[Authorize]** Attribut. Dieses Attribut überprüft, ob die HTTP-Anforderung authentifiziert wurde. Wenn die Anforderung nicht authentifiziert ist, empfängt der Client den HTTP-401 nicht autorisiert. Weitere Informationen zur Authentifizierung zu [Authentifizierung und Autorisierung in ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Die `TodoController` Klasse ähnelt `TodoListController`. Der größte Unterschied ist, dass es keine GET-Methoden nicht definiert ist, da der Client die to-do-Elemente zusammen mit jeder to-Do-Liste erhalten.

## <a name="mvc-controllers-and-views"></a>MVC-Controller und Ansichten

Das MVC-Controller befinden sich auch im Ordner der Projektmappe. `HomeController`Rendert die HTML-Hauptdatei für die Anwendung an. Die Ansicht für den Home-Controller ist in Views/Home/Index.cshtml definiert. Die Home-Ansicht rendert, unterschiedlichen Inhalt, je nachdem, ob der Benutzer angemeldet ist:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Wenn Benutzer angemeldet sind, sehen sie die Hauptbenutzeroberfläche. Andernfalls Anmeldefenster angezeigt. Beachten Sie, dass dieser bedingten Rendering auf dem Server stattfinden. Nie versucht, Ausblenden von vertraulichem Inhalt auf der Clientseite & #8212anything, die Sie in einer HTTP-Antwort senden ist sichtbar für Entwickler die unformatierten HTTP-Nachrichten überwacht wird.

## <a name="client-side-javascript-and-knockoutjs"></a>Clientseitiges JavaScript und Knockout.js

Jetzt aktivieren wir von der Serverseite der Anwendung an den Client. Die Vorlage SPA verwendet eine Kombination von jQuery und Knockout.js zum Erstellen einer smooth, interaktiven Benutzeroberflächenautomatisierungs. Knockout.js ist eine JavaScript-Bibliothek, die an Daten gebunden werden HTML erleichtert. Knockout.js verwendet ein Muster mit der Bezeichnung "Model-View-ViewModel."

- Das Modell ist die Domänendaten (TODO-Listen und TODO-Elemente).
- Die Ansicht ist das HTML-Dokument.
- Das Ansichtsmodell ist ein JavaScript-Objekt, das die Modelldaten enthält. Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche. Sie hat keine Kenntnis der HTML-Darstellung. Stattdessen repräsentiert die abstrakte Funktionen in der Ansicht, z. B. "eine Liste der TODO-Elemente".

Die Ansicht für das Ansichtsmodell datengebunden ist. Updates für das ViewModel werden automatisch in der Sicht widergespiegelt. Bindungen arbeiten, die andere Richtung ebenfalls. Ereignisse in das DOM (z. B. klickt) werden von datengebundenen Funktionen für das Modell anzeigen, die AJAX-Aufrufe auslösen.

Die SPA-Vorlage werden die clientseitiges JavaScript in drei Ebenen organisiert:

- TODO.DataContext.js: AJAX-Anforderungen sendet.
- TODO.Model.js: definiert die Modelle.
- TODO.ViewModel.js: das Ansichtsmodell definiert.

![](knockoutjs-template/_static/image11.png)

Diese Skriptdateien befinden sich im Ordner "Skripts-app" der Projektmappe.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** verarbeitet alle AJAX-Aufrufe an die Web-API-Controller. (Die AJAX-Aufrufe für die Anmeldung werden in anderen Kontexten in ajaxlogin.js definiert.)

**TODO.Model.js** definiert die clientseitige (Browser) Modelle für die to-do-Listen. Es gibt zwei Modellklassen: TodoItem und "Todolist".

Viele der Eigenschaften in der Modellklassen sind vom Typ "ko.observable". Wahrnehmbare Elemente werden wie Knockout seine Magic funktioniert. Aus der [Knockout Dokumentation](http://knockoutjs.com/documentation/introduction.html): Observable-Objekt ist ein "JavaScript-Objekt, mit denen Abonnenten über Änderungen benachrichtigt werden kann." Wenn der Wert der Observable-Objekt geändert wird, aktualisiert Knockout HTML-Elemente, die an diese Wahrnehmbare Elemente gebunden sind. Beispielsweise weist die TodoItem Wahrnehmbare Elemente für den Titel und IsDone-Eigenschaften:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Sie können auch Observable-Objekt im Code abonnieren. Beispielsweise abonniert die TodoItem-Klasse auf Änderungen in den Eigenschaften "IsDone" und "Title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Anzeigen des Modells**

Das Ansichtsmodell wird in todo.viewmodel.js definiert. Das Ansichtsmodell ist der zentrale Punkt, in dem die Anwendung die Elemente der HTML-Seite an die Domänendaten bindet. In der Vorlage SPA enthält das Ansichtsmodell ein Observable Array von TodoLists. Der folgende Code in das Ansichtsmodell weist Knockout, um die Bindungen anzuwenden:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML und Datenbindung

Die wichtigsten HTML für die Seite wird in Views/Home/Index.cshtml definiert. Da wir die Datenbindung verwenden, ist der HTML-Code nur eine Vorlage für was tatsächlich gerendert wird. Knockout verwendet *deklarative* Bindungen. Sie können Seitenelemente an Daten binden, indem Sie das Element ein Attribut "Data-Bind" hinzugefügt. Hier ist ein sehr einfaches Beispiel, das aus der Dokumentation zu Knockout entnommen:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

In diesem Beispiel Knockout aktualisiert den Inhalt der  **&lt;umfassen&gt;**  Element mit dem Wert des `myItems.count()`. Wenn dieser Wert geändert wird, aktualisiert Knockout des Dokuments.

Knockout bietet eine Reihe von unterschiedlichen Bindungstypen. Hier sind einige der in der Vorlage SPA verwendeten Bindungen:

- **Foreach**: können Sie eine Schleife durchlaufen, und wenden das gleiche Markup auf jedes Element in der Liste. Hiermit wird die Listen mit Aufgaben und Einzelaufgaben gerendert. Innerhalb der **Foreach**, die Bindungen werden angewendet, um die Elemente der Liste.
- **sichtbare**: verwendet, um die Sichtbarkeit umzuschalten. Blenden Sie Markup aus, wenn eine Auflistung leer ist, oder stellen Sie die Fehlermeldung angezeigt.
- **Wert**: Formularwerte aufgefüllt.
- **Klicken Sie auf**: bindet einen Click-Ereignis an eine Funktion für das Modell anzeigen.

## <a name="anti-csrf-protection"></a>Anti-CSRF-Schutz

Cross-Site Request Fälschung Websiteübergreifender ist ein Angriff, in denen eine bösartige Website sendet eine Anforderung mit einem gefährdeten Standort, in dem der Benutzer zurzeit angemeldet ist. Zum Vermeiden von CSRF-Angriffen verwendet ASP.NET MVC *fälschungssicherheitstoken*, so genannte Anforderung Überprüfung-Token. Die Idee dabei ist, dass der Server ein zufällig generiertes Token in eine Webseite versetzt. Wenn der Client Daten an den Server sendet, muss sie diesen Wert in der Anforderungsnachricht enthalten.

Fälschungssicherheitstoken verwendet werden, da die böswillige Seite das Benutzertoken aufgrund von Richtlinien für denselben Ursprung nicht lesen kann. (Same-Origin-Richtlinien zu verhindern, dass Dokumente auf zwei verschiedenen Standorten aus den Zugriff auf den Ergebnissen anderer Inhalt hosted.)

ASP.NET MVC bietet integrierte Unterstützung für fälschungssicherheitstoken, über die [AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) Klasse und die [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) Attribut. Diese Funktion ist derzeit nicht in Web-API erstellt werden. Allerdings enthält die SPA-Vorlage eine benutzerdefinierte Implementierung für die Web-API. Dieser Code wird definiert, der `ValidateHttpAntiForgeryTokenAttribute` -Klasse, die sich im Ordner "Filter" der Projektmappe befindet. Weitere Informationen zu Anti-CSRF in Web-API finden Sie unter [verhindern Cross-Site (Angriffen Request FORGERY)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Schlussbemerkung

Die SPA-Vorlage ist für Ihre ersten Schritte schnell schreiben moderner, interaktive Webanwendungen konzipiert. Die Knockout.js-Bibliothek verwendet, um die Präsentationsinformationen (HTML-Markup) und die Daten und Anwendungslogik zu trennen. Knockout ist jedoch nicht die einzige JavaScript-Bibliothek, die Sie zum Erstellen einer SPA verwenden können. Wenn Sie einige andere Optionen durchsuchen möchten, sehen Sie sich die [SPA-Vorlagen von der Community erstellte](../templates/index.md).
