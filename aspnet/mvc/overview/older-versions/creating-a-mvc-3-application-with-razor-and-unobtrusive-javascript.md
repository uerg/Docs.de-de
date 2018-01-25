---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Erstellen einer MVC 3 Application with Razor and unaufdringliches JavaScript | Microsoft Docs
author: microsoft
description: Die Benutzerliste Beispielwebanwendung veranschaulicht, wie einfach es ist zum Erstellen von ASP.NET MVC 3-Anwendungen, die das Razor-Anzeigemodul verwenden. Die Beispiel-Anwendung-s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 29b45c07b5498542abbf22c4c3001b1cee41edc9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript
====================
durch [Microsoft](https://github.com/microsoft)

> Die Benutzerliste Beispielwebanwendung veranschaulicht, wie einfach es ist zum Erstellen von ASP.NET MVC 3-Anwendungen, die das Razor-Anzeigemodul verwenden. Die beispielanwendung zeigt, wie die neue Razor-Ansichtsmodul mit ASP.NET MVC 3-Version und die Visual Studio 2010 zum Erstellen einer fiktiven Benutzerliste-Websites, die Funktionen, z. B. erstellen, anzeigen, bearbeiten und Löschen von Benutzern enthält.
> 
> In diesem Lernprogramm wird beschrieben, die ausgeführt wurden, um die Benutzerliste Beispiel ASP.NET MVC 3-Anwendung zu erstellen. Visual Studio-Projekt mit c# und VB-Quellcode ist zu diesem Thema steht zur Verfügung: [herunterladen](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Wenn Sie Fragen zu diesem Lernprogramm haben, Supportfragen zu den [MVC-Forum](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Übersicht

Die Anwendung, die Sie erstellen werden müssen ist eine einfache Liste-Website. Benutzer können eingeben, anzeigen und Aktualisieren von Benutzerinformationen.

![Beispiel-site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Sie können das abgeschlossene Projekt VB- und C#- [hier](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Erstellen der Webanwendung

Um das Lernprogramm zu starten, öffnen Sie Visual Studio 2010, und erstellen Sie ein neues Projekt mit der *ASP.NET MVC 3-Webanwendung* Vorlage. Benennen Sie die Anwendung &quot;Mvc3Razor&quot;.

[![Neues MVC 3-Projekt](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**, wählen Sie das Razor-Ansichtsmodul, und klicken Sie dann auf **OK**.

![Dialogfeld "neues ASP.NET MVC 3-Projekt"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In diesem Lernprogramm werden Sie nicht den ASP.NET-Mitgliedschaftsanbieter verwenden können Sie die Dateien, die für die zugeordnete Anmeldung und Mitgliedschaft löschen können. In **Projektmappen-Explorer**, entfernen Sie die folgenden Dateien und Verzeichnisse:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (und alle Dateien in diesem Verzeichnis)

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Bearbeiten der  *\_Layout.cshtml* -Datei und Ersetzen Sie das Markup der `<div>` Element mit dem Namen `logindisplay` mit der Meldung  *&quot;* Anmeldung deaktiviert&quot;. Das folgende Beispiel zeigt das neue Markup:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Hinzufügen des Modells

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Modelle* Ordner wählen **hinzufügen**, und klicken Sie dann auf **Klasse**.

![Neue Benutzer Mdl-Klasse](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nennen Sie die Klasse `UserModel`. Ersetzen Sie den Inhalt von der *UserModel* Datei durch den folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

Die `UserModel` Klasse stellt Benutzer dar. Jeder Member der Klasse mit dem versehen ist die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut aus dem [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace. Die Attribute in der [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace stellen automatische Client- und serverseitige Validierung für Webanwendungen bereit.

Öffnen der `HomeController` -Klasse und das Hinzufügen einer `using` Direktive, damit Sie Zugriff haben die `UserModel` und `Users` Klassen:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Direkt nach der `HomeController` Deklaration, fügen Sie den folgenden Kommentar und der Verweis auf eine `Users` Klasse:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

Die `Users` Klasse ist ein vereinfachtes, in-Memory-Datenspeicher, die Sie in diesem Lernprogramm verwenden. In einer echten Anwendung würden Sie eine Datenbank zum Speichern von Benutzerinformationen verwenden. Die ersten Zeilen von der `HomeController` Datei werden im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Erstellen Sie die Anwendung aus, sodass Benutzermodell für den Gerüstbau-Assistenten im nächsten Schritt verfügbar ist.

## <a name="creating-the-default-view"></a>Erstellen die Standardansicht

Der nächste Schritt ist zum Hinzufügen einer Aktionsmethode und können die Benutzer anzeigen.

Löschen Sie die vorhandene *Views\Home\Index* Datei. Erstellen Sie ein neues *Index* Datei, die die Benutzer angezeigt.

In der `HomeController` Klasse, ersetzen Sie den Inhalt von der `Index` -Methode durch folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Mit der rechten Maustaste innerhalb der `Index` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**.

![Fügen Sie die Ansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Wählen Sie die **eine stark typisierte Ansicht erstellen** Option. Für **Datenklasse anzeigen**Option **Mvc3Razor.Models.UserModel**. (Wenn Sie nicht sehen **Mvc3Razor.Models.UserModel** in der **Datenklasse anzeigen** Feld müssen Sie das Projekt zu erstellen.) Stellen Sie sicher, dass das Ansichtsmodul, um festgelegt ist **Razor**. Legen Sie **Inhalt anzeigen** auf **Liste** , und klicken Sie dann auf **hinzufügen**.

![Indexansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Die neue Ansicht scaffolds automatisch die Benutzerdaten, die an die `Index` anzeigen. Überprüfen Sie den neu generierten *Views\Home\Index* Datei. Die **neu erstellen**, **bearbeiten**, **Details**, und **löschen** Links funktionieren nicht, aber der Rest der Seite ist funktionsfähig. Führen Sie die Seite. Sie sehen eine Liste von Benutzern.

![Indexseite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Öffnen der *Index.cshtml* -Datei und ersetzen die `ActionLink` Markup für **bearbeiten**, **Details**, und **löschen** durch den folgenden Code :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Der Benutzername wird als ID verwendet, zu dem ausgewählten Datensatz in der **bearbeiten**, **Details**, und **löschen** Links.

## <a name="creating-the-details-view"></a>Erstellen die Detailansicht

Der nächste Schritt besteht, zum Hinzufügen einer `Details` Aktionsmethode und Ansicht um Benutzerdetails anzuzeigen.

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Fügen Sie die folgenden `Details` Methode, um den home-Controller:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Mit der rechten Maustaste innerhalb der `Details` -Methode, und wählen Sie dann **Ansicht hinzufügen**. Überprüfen Sie, ob die **Datenklasse anzeigen** enthält **Mvc3Razor.Models.UserModel***.* Legen Sie **Inhalt anzeigen** auf **Details** , und klicken Sie dann auf **hinzufügen**.

![Hinzufügen der Detailansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Führen Sie die Anwendung, und wählen Sie einen Link Details. Der automatische Gerüstbau zeigt jede Eigenschaft im Modell an.

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Erstellen die Bearbeitungsansicht

Fügen Sie die folgenden `Edit` Methode, um den home-Controller.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Fügen Sie eine Ansicht aus, wie in den vorherigen Schritten jedoch festgelegt **Inhalt anzeigen** auf **bearbeiten**.

![Hinzufügen der Bearbeitungsansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Führen Sie die Anwendung, und bearbeiten Sie den ersten und letzten Namen von einem Benutzer. Wenn Sie eine verletzen `DataAnnotation` Einschränkungen, die auf angewendet wurden die `UserModel` -Klasse, bei der Übermittlung des Formulars sehen Sie Validierungsfehler, die in Servercode erstellt werden. Angenommen, Sie ändern, dass der Vorname &quot;Ann&quot; auf &quot;ein&quot;, bei der Übermittlung des Formulars die folgende Fehlermeldung wird angezeigt, auf dem Formular:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In diesem Lernprogramm haben Sie den Benutzernamen als primären Schlüssel behandelt. Aus diesem Grund kann die Name-Eigenschaft für Benutzer nicht geändert werden. In der *Edit.cshtml* Datei, direkt nach der `Html.BeginForm` -Anweisung, legen Sie den Benutzernamen ein, ein ausgeblendetes Feld sein. Dies bewirkt, dass die Eigenschaft im Modell übergeben werden. Das folgende Codefragment zeigt die Platzierung der `Hidden` Anweisung:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Ersetzen Sie die `TextBoxFor` und `ValidationMessageFor` Markup für den Benutzernamen mit einer `DisplayFor` aufrufen. Die `DisplayFor` Methode zeigt die Eigenschaft als nur-Lese Element. Das folgende Beispiel zeigt das vollständige Markup. Die ursprüngliche `TextBoxFor` und `ValidationMessageFor` Aufrufe mit Razor beginnen Kommentar und End-Kommentar Zeichen auskommentiert werden (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Die clientseitige Validierung aktivieren

Um die clientseitige Validierung in ASP.NET MVC 3 zu aktivieren, müssen Sie zwei Flags festlegen und Sie müssen drei JavaScript-Dateien.

Öffnen Sie die Anwendung *"Web.config"* Datei. Vergewissern Sie sich `that ClientValidationEnabled` und `UnobtrusiveJavaScriptEnabled` festgelegt sind, auf "true" in den Anwendungseinstellungen. Das folgende Fragment aus dem Stammverzeichnis *"Web.config"* -Datei zeigt die richtigen Einstellungen:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Festlegen von `UnobtrusiveJavaScriptEnabled` auf aktiviert unaufdringliches Ajax und unaufdringliche Clientvalidierung "true". Bei Verwendung von unaufdringlichen Überprüfung werden die Gültigkeitsprüfungsregeln verwendet in HTML5-Attribute aktiviert. HTML5-Attributnamen können nur Kleinbuchstaben, Zahlen und Bindestrichen bestehen.

Festlegen von `ClientValidationEnabled` auf "true" ermöglicht die clientseitige Validierung. Durch Festlegen dieser Schlüssel in der Anwendung *"Web.config"* Datei, Sie aktivieren die Clientvalidierung und unaufdringliches JavaScript für die gesamte Anwendung. Sie können auch aktivieren oder deaktivieren Sie diese Einstellungen in einzelne Sichten oder Controllermethoden, die mit dem folgenden Code:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Sie müssen auch einige JavaScript-Dateien in der gerenderten Ansicht enthalten. Eine einfache Möglichkeit, die in allen Ansichten der JavaScript-Code enthalten ist, zum Hinzufügen der *Views\Shared\\_Layout.cshtml* Datei. Ersetzen Sie die `<head>` Element von der  *\_Layout.cshtml* Datei durch den folgenden Code:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Die ersten beiden jQuery-Skripts, die von der Microsoft Ajax Content Delivery Network (CDN) gehostet werden. Durch das Microsoft Ajax-CDN nutzen, können Sie die erste Treffer Leistung Ihrer Anwendungen bedeutend verbessern.

Führen Sie die Anwendung, und klicken Sie auf einen Bearbeitungslink. Zeigen Sie den Quellcode der Seite im Browser. Die Browserquelle zeigt viele Attribute des Formulars `data-val` (für die datenüberprüfung). Wenn die Clientvalidierung und unaufdringliches JavaScript aktiviert ist, Eingabefelder mit einer Client-Validierungsregel enthalten die `data-val="true"` Attribut unaufdringliche Clientvalidierung auslösen. Z. B. die `City` Feld im Modell wurde mit ergänzt die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut, das Ergebnisse im HTML-Code im folgenden Beispiel gezeigt:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Für jede Regel Clientvalidierung Element wird, die das Formular enthält ein Attribut hinzugefügt `data-val-rulename="message"`. Mithilfe der `City` erforderlichen Clientvalidierung diese Regel generiert eine zuvor gezeigten Beispiel der `data-val-required` Attribut und der Meldung &quot;der Ort ist ein Pflichtfeld&quot;. Führen Sie die Anwendung, ein Benutzer bearbeiten und löschen Sie die `City` Feld. Wenn Sie aus dem Feld tab, sehen Sie eine clientseitige Validierungsfehlermeldung angezeigt.

![Ort erforderlich](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Auf ähnliche Weise für jeden Parameter in der Clientvalidierung Regel wird ein Attribut hinzugefügt, die das Formular enthält `data-val-rulename-paramname=paramvalue`. Z. B. die `FirstName` Eigenschaft mit dem versehen ist die [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut und gibt eine minimale Länge von 3 und einer maximalen Länge von 8. Der mit dem Namen Gültigkeitsprüfungsregel `length` wurde der Name des Parameters `max` und der Wert des Parameters 8. Das folgende Beispiel zeigt den HTML-Code, der generiert wird, für die `FirstName` Feld, wenn Sie ein Benutzer bearbeiten:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Weitere Informationen zu unaufdringliche Clientvalidierung, finden Sie im Eintrag [unaufdringliche Clientvalidierung in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilsons Blog.

> [!NOTE]
> In ASP.NET MVC 3 Beta müssen Sie manchmal zum Senden des Formulars, um die clientseitige Überprüfung zu starten. Dies kann für die endgültige Version geändert werden.


## <a name="creating-the-create-view"></a>Erstellen die Erstellungsansicht

Der nächste Schritt besteht, zum Hinzufügen einer `Create` Aktionsmethode und anzeigen, damit den Benutzer einen neuen Benutzer erstellen können. Fügen Sie die folgenden `Create` Methode, um den home-Controller:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Fügen Sie eine Ansicht aus, wie in den vorherigen Schritten jedoch festgelegt **Inhalt anzeigen** auf **erstellen**.

![Create View (Ansicht erstellen)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Die Anwendung auszuführen, wählen Sie die **erstellen** verknüpfen, und fügen Sie einen neuen Benutzer hinzu. Die `Create` Methode automatisch nutzt die clientseitige und serverseitige Validierung. Bei dem Versuch, einen Benutzernamen eingeben, der Leerzeichen, z. B. enthält &quot;Ben X&quot;. Wenn Sie die Registerkarte Out Feld für den Benutzernamen, die clientseitige Überprüfung ein Fehler (`White space is not allowed`) wird angezeigt.

## <a name="add-the-delete-method"></a>Fügen Sie die Delete-Methode

Um das Lernprogramm abgeschlossen haben, fügen Sie die folgenden `Delete` Methode, um den home-Controller:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Hinzufügen einer `Delete` anzeigen, wie in den vorherigen Schritten festlegen **Inhalt anzeigen** auf **löschen**.

![Ansicht löschen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Sie haben nun eine einfache, aber voll funktionsfähige ASP.NET MVC 3-Anwendung mit der Validierung.
