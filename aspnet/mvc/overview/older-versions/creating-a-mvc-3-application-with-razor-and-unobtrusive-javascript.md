---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript | Microsoft-Dokumentation
author: microsoft
description: Die Benutzerliste-Beispielwebanwendung veranschaulicht, wie einfach es ist die Erstellung von ASP.NET MVC 3-Anwendungen, die die Razor-Ansichts-Engine verwenden. Die Beispiel-Anwendung-s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 39ed35c1b7d5c702ffea6908daeac5ca12f1693e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398012"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript
====================
durch [Microsoft](https://github.com/microsoft)

> Die Benutzerliste-Beispielwebanwendung veranschaulicht, wie einfach es ist die Erstellung von ASP.NET MVC 3-Anwendungen, die die Razor-Ansichts-Engine verwenden. Die beispielanwendung veranschaulicht, wie die neue Razor-ansichtsengine mit ASP.NET MVC 3-Version und die Visual Studio 2010 zum Erstellen einer fiktiven Benutzerliste-Websites, die Funktionen wie das Erstellen, anzeigen, bearbeiten und Löschen von Benutzern enthält.
> 
> In diesem Tutorial wird beschrieben, die ausgeführt wurden, um die Benutzerliste ASP.NET MVC 3-beispielanwendung zu erstellen. Visual Studio-Projekts mit c# und VB-Quellcode ist zur Ergänzung dieses Themas verfügbar: [herunterladen](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Wenn Sie Fragen zu diesem Tutorial haben, Posten Sie diese auf die [MVC-Forum](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Übersicht

Die Anwendung, die Sie erstellen, ist eine einfache Benutzer-Liste-Website. Benutzer können eingeben, anzeigen und Aktualisieren von Benutzerinformationen.

![Beispielwebsite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Sie können das abgeschlossene Projekt VB und c# [hier](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Erstellen der Webanwendung

Um das Lernprogramm zu starten, öffnen Sie Visual Studio 2010, und erstellen Sie ein neues Projekt mit der *ASP.NET MVC 3-Webanwendung* Vorlage. Nennen Sie die Anwendung &quot;Mvc3Razor&quot;.

[![Neues MVC 3-Projekt](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**, wählen Sie die Razor-Ansichts-Engine, und klicken Sie dann auf **OK**.

![Dialogfeld "neues ASP.NET MVC 3-Projekt"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In diesem Tutorial werden Sie nicht dem ASP.NET-Mitgliedschaftsanbieter verwenden können Sie die Dateien, die für die zugeordnete Anmeldung und Mitgliedschaft löschen können. In **Projektmappen-Explorer**, entfernen Sie die folgenden Dateien und Verzeichnisse:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (und alle Dateien in dieses Verzeichnis)

![Soln "exp"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Bearbeiten der  <em>\_Layout.cshtml</em> -Datei und Ersetzen Sie das Markup in der `<div>` Element mit dem Namen `logindisplay` mit der Meldung <em>&quot;</em>Anmeldung deaktiviert&quot;. Das folgende Beispiel zeigt das neue Markup:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Hinzufügen des Modells

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Modelle* Ordner **hinzufügen**, und klicken Sie dann auf **Klasse**.

![Neue Benutzer Mdl-Klasse](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nennen Sie die Klasse `UserModel`. Ersetzen Sie den Inhalt von der *UserModel* -Datei mit den folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

Die `UserModel` Klasse stellt Benutzer dar. Jeder Member der Klasse mit dem versehen ist die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut aus dem ["DataAnnotations"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace. Die Attribute in der ["DataAnnotations"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace bieten automatische Client- und serverseitige Validierung für Webanwendungen.

Öffnen der `HomeController` -Klasse und das Hinzufügen einer `using` Direktive, damit Sie zugreifen können, die `UserModel` und `Users` Klassen:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Direkt nach der `HomeController` Deklaration, fügen Sie den folgenden Kommentar und der Verweis auf eine `Users` Klasse:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

Die `Users` Klasse ist ein vereinfachtes, die in-Memory-Datenspeicher, die Sie in diesem Tutorial verwenden. In einer echten Anwendung würden Sie eine Datenbank verwenden, um Benutzerinformationen zu speichern. Die ersten Zeilen der `HomeController` Datei werden im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Erstellen Sie die Anwendung, sodass das Benutzermodell Assistenten für den Gerüstbau im nächsten Schritt zur Verfügung stehen.

## <a name="creating-the-default-view"></a>Erstellen die Standardansicht

Der nächste Schritt ist zum Hinzufügen einer Aktionsmethode und können die Benutzer anzeigen.

Löschen Sie die vorhandenen *Views\Home\Index* Datei. Erstellen Sie ein neues *Index* Datei, die die Benutzer angezeigt.

In der `HomeController` Klasse, ersetzen Sie den Inhalt von der `Index` Methode durch den folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Klicken Sie auf auf die `Index` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**.

![Ansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Wählen Sie die **eine stark typisierte Ansicht erstellen** Option. Für **Datenklasse anzeigen**Option **Mvc3Razor.Models.UserModel**. (Wenn Sie nicht sehen **Mvc3Razor.Models.UserModel** in die **Datenklasse anzeigen** Feld müssen Sie das Projekt zu erstellen.) Stellen Sie sicher, dass die Ansichts-Engine, um festgelegt ist **Razor**. Legen Sie **Inhalt anzeigen** zu **Liste** , und klicken Sie dann auf **hinzufügen**.

![Hinzufügen der Ansicht "Index"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Die neue Ansicht erstellt automatisch das Gerüst der Benutzerdaten, die an die `Index` anzeigen. Untersuchen Sie die neu generierte *Views\Home\Index* Datei. Die **neu erstellen**, **bearbeiten**, **Details**, und **löschen** Links funktionieren nicht, aber der Rest der Seite ist funktionsfähig. Führen Sie die Seite. Sie sehen eine Liste von Benutzern.

![Indexseite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Öffnen der *"Index.cshtml"* -Datei und Ersetzen Sie die `ActionLink` Markup für **bearbeiten**, **Details**, und **löschen** durch den folgenden Code :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Der Benutzername wird als ID verwendet, finden Sie den ausgewählten Datensatz in die **bearbeiten**, **Details**, und **löschen** Links.

## <a name="creating-the-details-view"></a>Die Detailansicht erstellen

Der nächste Schritt besteht, zum Hinzufügen einer `Details` Aktionsmethode und anzeigen, um Benutzerinformationen anzuzeigen.

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Fügen Sie die folgenden `Details` Methode, um den home-Controller:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Klicken Sie auf auf die `Details` -Methode, und wählen Sie dann <strong>Ansicht hinzufügen</strong>. Überprüfen Sie, ob die <strong>Datenklasse anzeigen</strong> enthält <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Legen Sie <strong>Inhalt anzeigen</strong> zu <strong>Details</strong> , und klicken Sie dann auf <strong>hinzufügen</strong>.

![Hinzufügen der Detailansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Führen Sie die Anwendung, und wählen Sie einen Details-Link. Der automatische Gerüstbau zeigt jede Eigenschaft im Modell.

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Erstellen die Bearbeitungsansicht

Fügen Sie die folgenden `Edit` Methode, um den home-Controller.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Hinzufügen einer Ansicht wie in den vorherigen Schritten, jedoch festgelegt **Inhalt anzeigen** zu **bearbeiten**.

![Hinzufügen der Ansicht "Bearbeiten"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Führen Sie die Anwendung, und bearbeiten Sie den ersten und letzten Namen von einem Benutzer. Wenn Sie gegen verstoßen `DataAnnotation` Einschränkungen, die auf angewendet wurden die `UserModel` -Klasse, wenn der Übermittlung des Formulars, sehen Sie Validierungsfehler, die von Server-Code erstellt werden. Wenn Sie den ersten Namen ändern, z. B. &quot;Ann&quot; zu &quot;ein&quot;, wenn Sie das Formular übermitteln der folgende Fehler wird angezeigt, auf dem Formular:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In diesem Tutorial, behandeln Sie den Benutzernamen als Primärschlüssel. Aus diesem Grund kann der benutzernameneigenschaft geändert werden. In der *Edit.cshtml* -Datei, direkt nach der `Html.BeginForm` -Anweisung, legen Sie den Benutzernamen ein, ein ausgeblendetes Feld sein. Dies bewirkt, dass die Eigenschaft im Modell übergeben werden. Das folgende Codefragment zeigt die Platzierung der `Hidden` Anweisung:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Ersetzen Sie die `TextBoxFor` und `ValidationMessageFor` Markup für den mit dem Namen einer `DisplayFor` aufrufen. Die `DisplayFor` Methode zeigt die Eigenschaft als nur-Lese Element. Das folgende Beispiel zeigt das vollständige Markup. Die ursprüngliche `TextBoxFor` und `ValidationMessageFor` Aufrufe mit den Razor-Kommentare beginnen und End-Kommentar Zeichen auskommentiert werden (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Aktivieren die clientseitige Validierung

Um die clientseitige Validierung in ASP.NET MVC 3 zu aktivieren, müssen Sie zwei Flags festlegen, und Sie müssen drei JavaScript-Dateien einschließen.

Öffnen Sie die Anwendung die *"Web.config"* Datei. Stellen Sie sicher `that ClientValidationEnabled` und `UnobtrusiveJavaScriptEnabled` festgelegt sind, auf "true" in den Anwendungseinstellungen. Das folgende Fragment aus dem Stammverzeichnis *"Web.config"* -Datei veranschaulicht die richtigen Einstellungen:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Festlegen von `UnobtrusiveJavaScriptEnabled` auf unaufdringliche Ajax ermöglicht und die unaufdringliche Clientvalidierung "true". Wenn Sie die unaufdringliche Validierung verwenden, werden die Validierungsregeln in HTML5-Attribute umgewandelt. Die Namen der HTML5-Attribute bestehen nur Kleinbuchstaben, Zahlen und Bindestrichen aus.

Festlegen von `ClientValidationEnabled` auf "true" ermöglicht die clientseitige Validierung. Durch Festlegen dieser Schlüssel in der Anwendung *"Web.config"* -Datei, Sie aktivieren die Clientvalidierung und unaufdringliches JavaScript für die gesamte Anwendung. Sie können auch aktivieren oder deaktivieren diese Einstellungen in einzelne Ansichten oder Controllermethoden, die mit dem folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Sie müssen auch mehrere JavaScript-Dateien in die gerenderte Ansicht einfügen. Eine einfache Möglichkeit, den JavaScript-Code in allen Ansichten enthalten ist, zum Hinzufügen der *Views\Shared\\"_Layout.cshtml"* Datei. Ersetzen Sie die `<head>` Element der  *\_Layout.cshtml* -Datei mit den folgenden Code:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Die ersten beiden jQuery-Skripts, die von der Microsoft Ajax Content Delivery Network (CDN) gehostet werden. Durch das Microsoft Ajax CDN nutzen, können Sie die ersten Leistung Ihrer Anwendungen erheblich verbessern.

Führen Sie die Anwendung, und klicken Sie auf einen Bearbeitungslink. Quellcode der Seite im Browser anzeigen. Die Browserquelle zeigt viele Attribute des Formulars `data-val` (zur datenvalidierung). Wenn die Clientvalidierung und unaufdringliches JavaScript aktiviert ist, enthalten Felder mit einer Client-Validation-Regel für die Eingabe der `data-val="true"` Attribut unaufdringlichen Validierung auslösen. Z. B. die `City` Feld im Modell ergänzt wurde die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut, das Ergebnisse im HTML-Code im folgenden Beispiel gezeigt:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Für jede Regel Clientvalidierung Element wird, die das Formular enthält ein Attribut hinzugefügt `data-val-rulename="message"`. Mithilfe der `City` Feld vorherigen Beispiel lautet der erforderlichen Client-Validation-Regel generiert die `data-val-required` -Attribut und der Meldung &quot;der City-Feld ist erforderlich,&quot;. Führen Sie die Anwendung, einen Benutzer zu bearbeiten und löschen die `City` Feld. Wenn Sie aus dem Feld tab, sehen Sie eine Fehlermeldung für die clientseitige Validierung.

![Stadt erforderlich](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Auf ähnliche Weise für jeden Parameter in der Clientvalidierung Regel wird ein Attribut hinzugefügt, die das Formular enthält `data-val-rulename-paramname=paramvalue`. Z. B. die `FirstName` -Eigenschaft die Anmerkungen die [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Attribut, und gibt eine Mindestlänge von 3 und einer maximalen Länge von 8. Der mit dem Namen Gültigkeitsprüfungsregel `length` ist der Name des Parameters `max` und den Wert des Parameters 8. Das folgende Beispiel zeigt den HTML-Code, der für generiert die `FirstName` Feld, wenn Sie einen Benutzer bearbeiten:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Weitere Informationen zu der unaufdringlichen Validierung, finden Sie im Eintrag [unaufdringliche Clientvalidierung in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilsons Blog.

> [!NOTE]
> In ASP.NET MVC 3-Beta müssen Sie manchmal zum Senden des Formulars, um die clientseitige Validierung zu starten. Dies kann für die endgültige Version geändert werden.


## <a name="creating-the-create-view"></a>Erstellen die Ansicht "erstellen"

Der nächste Schritt besteht, zum Hinzufügen einer `Create` Aktionsmethode und anzeigen, damit den Benutzer einen neuen Benutzer erstellen können. Fügen Sie die folgenden `Create` Methode, um den home-Controller:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Hinzufügen einer Ansicht wie in den vorherigen Schritten, jedoch festgelegt **Inhalt anzeigen** zu **erstellen**.

![Create View (Ansicht erstellen)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Die Anwendung auszuführen, wählen Sie die **erstellen** verknüpfen, und fügen Sie einen neuen Benutzer hinzu. Die `Create` Methode nutzt automatisch die clientseitige und serverseitige Validierung. Versucht, einen Benutzernamen eingeben, die Leerzeichen, z. B. enthält &quot;Ben X&quot;. Wenn Sie die Registerkarte aus dem Feld für den Benutzernamen, ein Fehler die clientseitige Validierung (`White space is not allowed`) wird angezeigt.

## <a name="add-the-delete-method"></a>Fügen Sie die Delete-Methode

Um das Tutorial abzuschließen, fügen Sie die folgenden `Delete` Methode, um den home-Controller:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Hinzufügen einer `Delete` anzeigen, wie in den vorherigen Schritten festlegen **Inhalt anzeigen** zu **löschen**.

![Ansicht löschen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Sie haben nun eine einfache, aber voll funktionsfähige ASP.NET MVC 3-Anwendung mit der Validierung.
