---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Teil 1: Übersicht und Erstellen des Projekts | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>Teil 1: Übersicht und Erstellen des Projekts
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework ist ein Framework für/objektrelationalen Zuordnung. Es wird die Domänenobjekte im Code Entitäten in einer relationalen Datenbank zugeordnet. Den meisten Fällen müssen Sie keinen auf der Datenbankebene kümmern, da Entity Framework es für Sie übernimmt. Code bearbeitet die Objekte, und Änderungen in einer Datenbank beibehalten werden.

## <a name="about-the-tutorial"></a>Informationen zum Lernprogramm

In diesem Lernprogramm erstellen Sie eine einfachen Store-Anwendung. Es gibt zwei wesentliche Teile der Anwendung. Normale Benutzer können Produkte anzeigen und Aufträge erstellen:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Administratoren können erstellen, löschen oder bearbeiten die Produkte:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erfahren

Hier ist Sie lernen:

- Verwendung von Entity Framework mit ASP.NET Web-API.
- Wie Sie knockout.js verwenden, um eine dynamische Clientbenutzeroberfläche zu erstellen.
- Wie Sie die Formularauthentifizierung mit Web-API verwenden, um Benutzer zu authentifizieren.

Obwohl dieses Lernprogramm eigenständig ist, empfiehlt es sich, lesen Sie zuerst die folgenden Lernprogramme:

- [Ihre erste ASP.NET Web-API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Erstellen eine Web-API, die unterstützt CRUD-Vorgänge](../creating-a-web-api-that-supports-crud-operations.md)

Einige Kenntnisse [ASP.NET-MVC](../../../../mvc/index.md) ist auch hilfreich.

## <a name="overview"></a>Übersicht

Auf hoher Ebene sieht die Architektur der Anwendung zur Verfügung:

- ASP.NET MVC generiert die HTML-Seiten für den Client.
- ASP.NET Web-API macht die CRUD-Vorgänge für die Daten ("Products" und "Orders").
- Entity Framework übersetzt, die c#-Modelle, die von Web-API in Datenbankentitäten verwendet wird.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Das folgende Diagramm zeigt, wie die Domänenobjekte auf verschiedenen Ebenen der Anwendung dargestellt werden: die Datenbankebene, das Objektmodell und schließlich das Wire-Format, die zum Übertragen von Daten an den Client über HTTP verwendet wird.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

Sie können das Tutorial-Projekt mit Visual Web Developer Express oder die Vollversion von Visual Studio erstellen.

Aus der **starten** auf **neues Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt "ProductStore", und klicken Sie auf **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung** , und klicken Sie auf **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Die Vorlage "Internetanwendung" erstellt eine ASP.NET MVC-Anwendung, die Formularauthentifizierung unterstützt. Wenn Sie die Anwendung jetzt ausführen, ist es bereits einige Funktionen:

- Neue Benutzer können registrieren, indem Sie auf den Link "Register" in der oberen rechten Ecke.
- Registrierte Benutzer können durch Klicken auf den Link "Anmelden" anmelden.

Informationen zur Mitgliedschaft werden in einer Datenbank beibehalten, die automatisch erstellt wird. Weitere Informationen zur Formularauthentifizierung in ASP.NET MVC finden Sie unter [Exemplarische Vorgehensweise: Verwenden der Formularauthentifizierung in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualisieren Sie die CSS-Datei

Dieser Schritt ist kosmetischer Natur, jedoch werden die Seiten gerendert werden wie die früheren Screenshots vorgenommen.

Klicken Sie im Projektmappen-Explorer erweitern Sie den Inhaltsordner, und öffnen Sie die Datei mit dem Namen "Site.CSS" ändern. Fügen Sie die folgenden CSS-Stile hinzu:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Nächste](using-web-api-with-entity-framework-part-2.md)
