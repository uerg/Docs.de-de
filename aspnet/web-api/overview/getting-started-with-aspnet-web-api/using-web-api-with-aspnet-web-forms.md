---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Mithilfe von Web-API mit ASP.NET Web Forms | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
ms.locfileid: "26536079"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Mithilfe von Web-API mit ASP.NET Web Forms
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Obwohl der ASP.NET Web-API mit ASP.NET MVC verpackt wird, ist es einfach, Web-API zu einer herkömmlichen ASP.NET Web Forms-Anwendung hinzufügen. Dieses Lernprogramm führt Sie durch die Schritte aus.

## <a name="overview"></a>Übersicht

Um Web-API in einer Web Forms-Anwendung zu verwenden, gibt es zwei Hauptschritte:

- Fügen Sie einen Web-API-Controller, die von abgeleitet ist die **ApiController** Klasse.
- Hinzufügen einer Routentabelle zu den **Anwendung\_starten** Methode.

## <a name="create-a-web-forms-project"></a>Erstellen eines Web Forms-Anwendungsprojekts

Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET Web Forms-Anwendung**. Geben Sie einen Namen für das Projekt, und klicken Sie auf **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Erstellen Sie das Modell und der Controller

Dieses Lernprogramm verwendet die gleichen Modell und Controller-Klassen wie der [Einstieg](tutorial-your-first-web-api.md) Lernprogramm.

Fügen Sie zuerst eine Modellklasse hinzu. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**. Benennen Sie die Klasse Product, und fügen Sie die folgende Implementierung:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Fügen Sie einen Web-API-Controller zu dem Projekt. eine *Controller* ist das Objekt, das HTTP-Anforderungen für die Web-API behandelt.

In **Projektmappen-Explorer**, mit der rechten Maustaste des Projekts. Wählen Sie **neues Element hinzufügen**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Klicken Sie unter **installierte Vorlagen**, erweitern Sie **Visual C#-** , und wählen Sie **Web**. Wählen Sie dann aus der Liste der Vorlagen, **Web-API-Controllerklasse**. Nennen Sie den Controller "ProductsController", und klicken Sie auf **hinzufügen**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

Die **neues Element hinzufügen** Assistenten wird eine Datei namens ProductsController.cs erstellt. Löschen Sie die Methoden, die der Assistent enthalten, und fügen Sie die folgenden Methoden:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Weitere Informationen zu den Code in diesem Controller, finden Sie unter der [Einstieg](tutorial-your-first-web-api.md) Lernprogramm.

## <a name="add-routing-information"></a>Hinzufügen von Routinginformationen

Als Nächstes wir fügen eine URI-Route also dieses URIs im Format &quot;/API/Produkte/&quot; an den Controller weitergeleitet werden.

In **Projektmappen-Explorer**, doppelklicken Sie auf "Global.asax", um die Code-Behind-Datei Global.asax.cs zu öffnen. Fügen Sie die folgenden **mit** Anweisung.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Fügen Sie folgenden Code, der **Anwendung\_starten** Methode:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Weitere Informationen zu Routingtabellen, finden Sie unter [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Fügen Sie die clientseitige AJAX

Das ist alles, die müssen Sie eine Web-API zu erstellen, die Clients zugreifen können. Nun fügen wir eine HTML-Seite, jQuery verwendet wird, zum Aufrufen der API, hinzu.

Stellen Sie sicher, dass die Gestaltungsvorlage (z. B. *Site.Master*) umfasst eine `ContentPlaceHolder` mit `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Öffnen Sie die Datei "default.aspx". Ersetzen Sie den Standardtext, der Sie im Abschnitt Inhalt ist an, wie dargestellt:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Fügen Sie einen Verweis auf die jQuery-Quelldatei in die `HeaderContent` Abschnitt:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Hinweis: Sie können problemlos hinzufügen Skriptverweis per Drag & Drop die Datei aus **Projektmappen-Explorer** in das Fenster des Code-Editor.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Fügen Sie die folgenden Skriptblock hinzu, unter dem jQuery-Skript-Tag:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Wenn das Dokument geladen wird, wird dieses Skript eine AJAX-Anforderung an &quot;-api-Produkte&quot;. Die Anforderung gibt eine Liste der Produkte im JSON-Format zurück. Das Skript fügt die Produktinformationen der HTML-Tabelle.

Wenn Sie die Anwendung ausführen, sollten sie wie folgt aussehen:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
