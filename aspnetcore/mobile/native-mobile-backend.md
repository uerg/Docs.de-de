---
title: "Erstellen von Back-End-Diensten für systemeigene Mobile Anwendungen"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Erstellen von Back-End-Diensten für systemeigene Mobile Anwendungen

Durch [Steve Smith](https://ardalis.com/)

Mobile apps können problemlos mit ASP.NET Core Back-End-Diensten kommunizieren.

[Anzeigen oder Herunterladen von Beispielcode für Back-End-Dienste](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Die systemeigene Mobile Beispiel-App

Dieses Lernprogramm veranschaulicht das Erstellen von Back-End-Diensten mithilfe von ASP.NET Core MVC zur Unterstützung systemeigener mobiler apps. Er verwendet die [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) als seine native Client umfasst die separaten native Clients für Android, iOS, Windows Universal und Windows Phone-Geräte. Sie können führen Sie die verknüpften Tutorial aus, um die systemeigene app zu erstellen (und die erforderlichen freien Xamarin-Tools zu installieren), sowie der Xamarin-beispiellösung herunterladen. Die Xamarin-Beispiel enthält ein ASP.NET Web API 2-Services-Projekt, der diesem Artikel ASP.NET Core app (mit keine Änderungen, die vom Client benötigte) ersetzt.

![Führen Sie Rest-Anwendung auf einem Android-Smartphone ausgeführt wird](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Features

Die app ToDoRest unterstützt auflisten, hinzufügen, löschen und Aktualisieren von Aufgaben. Jedes Element verfügt über eine ID, einen Namen, Hinweise und eine Eigenschaft, die angibt, ob er noch abgeschlossen ist.

Die Hauptansicht der Elemente, wie oben gezeigt Listet jedes Element Namen an und gibt an, ob es mit einem Häkchen erfolgt.

Tippen Sie auf die `+` Symbol wird ein Element "hinzufügen" geöffnet:

![Dialogfeld "hinzufügen"](native-mobile-backend/_static/todo-android-new-item.png)

Tippen Sie auf ein Element auf dem Bildschirm Hauptliste öffnet ein Dialogfeld "Bearbeiten", in dem des Elements, Hinweise, und "Fertig" ändert Einstellungen geändert werden können oder das Element gelöscht werden kann:

![Element-Dialogfeld "Bearbeiten"](native-mobile-backend/_static/todo-android-edit-item.png)

In diesem Beispiel wird standardmäßig am developer.xamarin.com, gehostete Back-End-Dienste verwendet wird, die nur-Lese Vorgänge können konfiguriert. Um es für die ASP.NET Core-app, die im nächsten Abschnitt auf Ihrem Computer erstellt, selbst zu testen, müssen Sie der app aktualisieren `RestUrl` konstant. Navigieren Sie zu der `ToDoREST` Projekt, und öffnen Sie die *Constants.cs* Datei. Ersetzen Sie die `RestUrl` mit einer URL, die dem Computer-IP-Adresse (nicht "localhost" oder 127.0.0.1, da diese Adresse aus dem Geräteemulator, nicht von Ihrem Computer verwendet wird). Schließen Sie die Port-Nummer (5000). Um zu testen, dass Ihre Dienste mit einem Gerät arbeiten, müssen sicherstellen Sie, dass Sie nicht über eine aktive Firewall blockiert den Zugriff auf diesen Port verfügen.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Erstellen des Projekts ASP.NET Core

Erstellen Sie eine neue ASP.NET Core-Webanwendung in Visual Studio. Wählen Sie die Vorlage für die Web-API und keine Authentifizierung. Nennen Sie das Projekt *ToDoApi*.

![Dialogfeld "neues ASP.NET Web Application" mit Web-API-Projektvorlage ausgewählt](native-mobile-backend/_static/web-api-template.png)

Die Anwendung, die auf alle Anforderungen an Port 5000 reagieren soll. Update *"Program.cs"* einschließen `.UseUrls("http://*:5000")` um dies zu erreichen:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Stellen Sie sicher, dass Sie die Anwendung ausführen, sondern direkt hinter der IIS Express, die nicht lokalen Anforderungen standardmäßig ignoriert. Führen Sie `dotnet run` von einer Befehlszeile aus, oder wählen Sie den Namen des Anwendungsprofils aus dem Dropdownmenü Debugziel in der Symbolleiste von Visual Studio.

Fügen Sie eine Modellklasse aus, um die to-do-Elemente darstellen. Markierung erforderliche Felder, die mithilfe der `[Required]` Attribut:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Die API-Methoden erfordern in irgendeiner Weise mit Daten arbeiten. Verwenden Sie den gleichen `IToDoRepository` Schnittstelle das ursprüngliche Xamarin-Beispiel verwendet:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Für dieses Beispiel verwendet die Implementierung nur eine private Auflistung von Elementen:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Konfigurieren Sie die Implementierung in *Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

An diesem Punkt sind Sie bereit für die Erstellung der *ToDoItemsController*.

> [!TIP]
> Erfahren Sie mehr über das Erstellen von web-APIs in [Erstellen Ihrer ersten Web-API mit ASP.NET Core MVC und Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Erstellen des Controllers

Fügen Sie dem Projekt einen neuen Domänencontroller *ToDoItemsController*. Es sollte von Microsoft.AspNetCore.Mvc.Controller erben. Hinzufügen einer `Route` Attribut, um anzugeben, dass der Controller behandelt Anforderungen für Pfade ab `api/todoitems`. Die `[controller]` Token in der Route wird durch den Namen des Controllers ersetzt (Auslassen der `Controller` Suffix), und ist besonders nützlich für globale Routen. Erfahren Sie mehr über [routing](../fundamentals/routing.md).

Der Controller benötigt eine `IToDoRepository` zu funktionieren, fordern Sie eine Instanz dieses Typs über den Controller-Konstruktor. Zur Laufzeit wird diese Instanz mit der Framework-Unterstützung für bereitgestellt werden [Abhängigkeitsinjektion](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Diese API unterstützt vier verschiedene HTTP-Verben zum Ausführen von Vorgängen von CRUD (Create, Read, Update, Delete) für die Datenquelle. Die einfachste davon ist der Lesevorgang, die eine HTTP-GET-Anforderung entspricht.

### <a name="reading-items"></a>Lesen von Elementen

Eine Liste der Elemente anfordern erfolgt mit einer GET-Anforderung an die `List` Methode. Die `[HttpGet]` -Attribut auf die `List` Methode gibt an, dass diese Aktion nur GET-Anforderungen verarbeiten soll. Die Route für diese Aktion wird die Route auf dem Controller angegeben. Sie müssen nicht unbedingt der Aktionsname im Rahmen der Route verwenden zu können. Sie müssen nur sicherstellen, dass jede Aktion eine Route eindeutigen verfügt. Routing Attribute können auf dem Controller und Methodenebene bestimmte Routen erstellen angewendet werden.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

Die `List` Methodenrückgabe eine 200 OK-Antwort-Code und alle der ToDo-Elemente, die als JSON serialisiert.

Sie können testen, die neue API-Methode, die eine Vielzahl von Tools, z. B. mit [Postman](https://www.getpostman.com/docs/), hier dargestellt:

![Postman-Konsole, die mit einer GET-Anforderung für Todoitems und der Antworttext die JSON für drei zurückgegebenen Elementen anzeigen](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Erstellen von Elementen

Gemäß der Konvention wird erstellen neue Datenelemente, die dem HTTP-POST-Verb zugeordnet. Die `Create` Methode verfügt über eine `[HttpPost]` Attribut angewendet wird, und akzeptiert eine `ToDoItem` Instanz. Da die `item` übergebene Argument im Text des BEITRAGS, dieser Parameter wird mit ergänzt die `[FromBody]` Attribut.

Innerhalb der Methode das Element wird auf Gültigkeit und vorherigen Vorhandensein im Datenspeicher überprüft, und wenn keine Probleme auftreten, wird er hinzugefügt, mit dem Repository. Überprüfung `ModelState.IsValid` führt [modellüberprüfung](../mvc/models/validation.md), und in jede API-Methode, das Benutzereingaben akzeptiert, ausgeführt werden soll.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

Das Beispiel verwendet eine Enumeration mit Fehlercodes, die an dem mobilen Client übergeben werden:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Testen Sie das Hinzufügen von neuen Elementen, die mithilfe von Postman durch Auswählen der POST-Verb, das das neue Objekt im JSON-Format im Text der Anforderung bereitstellt. Sie sollten auch hinzufügen, eine Anforderung Header angeben einer `Content-Type` von `application/json`.

![Postman-Konsole, die mit einer POST- und Antwort](native-mobile-backend/_static/postman-post.png)

Die Methode gibt das neu erstellte Element in der Antwort.

### <a name="updating-items"></a>Aktualisieren von Elementen

Zeichnet die Änderung erfolgt mithilfe von HTTP PUT-Anforderungen. Abgesehen von dieser Änderung der `Edit` Methode entspricht weitgehend dem `Create`. Beachten Sie, dass, wenn der Datensatz nicht gefunden wird, wird die `Edit` Aktion zurückgegeben wird ein `NotFound` Antwort (404).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Informationen zum Postman testen, ändern Sie das Verb PUT-Befehl. Geben Sie die Daten des aktualisierten Objekts im Text der Anforderung.

![Postman-Konsole, die mit einer PUT und-Antwort](native-mobile-backend/_static/postman-put.png)

Diese Methode gibt ein `NoContent` (204) Antwort bei erfolgreicher Ausführung aus Gründen der Konsistenz mit der bereits vorhandenen-API.

### <a name="deleting-items"></a>Löschen von Elementen

Löschen von Datensätzen erfolgt durch, die DELETE-Anforderungen an den Dienst, und übergeben Sie die ID des Elements gelöscht werden soll. Mit Updates, erhalten diese Anforderungen für Elemente, die nicht vorhandene `NotFound` Antworten. Eine erfolgreiche Anforderung erhalten, andernfalls ein `NoContent` (204) Antwort.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Beachten Sie, dass nichts beim Testen der Delete-Funktionen im Text der Anforderung erforderlich ist.

![Postman-Konsole, die mit einer DELETE- und Antwort](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Allgemeine Konventionen für Web-API

Wie Sie die Back-End-Dienste für Ihre app entwickeln, sollten Sie mit einem konsistenten Satz von Konventionen oder Richtlinien für die Behandlung von querschnittliche Bedenken aufgerufen werden. Beispielsweise im oben gezeigten Dienst Anforderungen für bestimmte Datensätze, die gefunden wurden empfangen einer `NotFound` Antwort, anstelle eines `BadRequest` Antwort. Auf ähnliche Weise Befehle, die versucht, diesen Dienst, der gebunden Modelltypen immer überprüft übergeben `ModelState.IsValid` Komponentenmetadatenobjekts durch eine `BadRequest` für ungültige Modelltypen.

Nachdem Sie eine allgemeine Richtlinie für Ihre APIs identifiziert haben, können in der Regel gekapselt werden in einem [Filter](../mvc/controllers/filters.md). Erfahren Sie mehr über [zum Kapseln von allgemeinen API-Richtlinien in ASP.NET Core MVC-Anwendungen wie](https://msdn.microsoft.com/magazine/mt767699.aspx).
