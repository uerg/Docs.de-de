---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Teil 1: Übersicht und erstellen das Projekt | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0e4021402e8deccd2395f23b6b512679b5e9d281
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838645"
---
<a name="part-1-overview-and-creating-the-project"></a>Teil 1: Übersicht und Erstellen des Projekts
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entitätsframework ist ein Framework für Objekt-relationale Zuordnung. Es ist die Domänenobjekte in Ihrem Code Entitäten in einer relationalen Datenbank zugeordnet. Zum größten Teil, müssen Sie keinen der Datenbankschicht zu kümmern, da Entity Framework es für Sie übernimmt. Ihr Code bearbeitet, die Objekte, und Änderungen in einer Datenbank beibehalten werden.

## <a name="about-the-tutorial"></a>Über das Tutorial

In diesem Tutorial erstellen Sie eine einfache Store-Anwendung. Es gibt zwei wesentliche Teile der Anwendung. Standardbenutzer können Produkte anzeigen, und Aufträge zu erstellen:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Administratoren können zu erstellen, löschen oder Bearbeiten von Produkten:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, mit denen, die Sie lernen Folgendes

Hier ist Sie lernen Folgendes:

- Wie Sie Entity Framework mit ASP.NET Web-API zu verwenden.
- Gewusst wie "Knockout.js" verwenden, um eine dynamische Clientbenutzeroberfläche zu erstellen.
- So verwenden Sie die Formularauthentifizierung mit Web-API zum Authentifizieren von Benutzern.

Obwohl in diesem Tutorial eigenständig ist, empfiehlt es sich, zuerst die folgenden Lernprogramme zu lesen:

- [Ihre erste ASP.NET Web-API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Erstellen einer Webs-API, die unterstützt CRUD-Vorgänge](../creating-a-web-api-that-supports-crud-operations.md)

Sie benötigen Kenntnisse des [ASP.NET MVC](../../../../mvc/index.md) ist auch hilfreich.

## <a name="overview"></a>Übersicht

Auf einer hohen Ebene sieht der Architektur der Anwendung zur Verfügung:

- ASP.NET MVC generiert die HTML-Seiten für den Client.
- ASP.NET Web-API macht die CRUD-Vorgänge für die Daten ("Produkte" und "Orders") verfügbar.
- Entitätsframework übersetzt der c#-Modelle, die von Web-API verwendet werden, in der Datenbankentitäten.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Das folgende Diagramm zeigt, wie die Domänenobjekte auf verschiedenen Ebenen der Anwendung dargestellt werden: die Datenbankebene, das Objektmodell und schließlich das Wire-Format, die zum Übertragen von Daten an den Client über HTTP verwendet wird.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

Sie können das Tutorial-Projekt mit Visual Web Developer Express oder die Vollversion von Visual Studio erstellen.

Von der **starten** auf **neues Projekt**.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt "ProductStore", und klicken Sie auf **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung** , und klicken Sie auf **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Die Vorlage "Internetanwendung" erstellt eine ASP.NET MVC-Anwendung, die Formularauthentifizierung unterstützt. Wenn Sie die Anwendung jetzt ausführen, verfügt sie bereits über einige Features:

- Neue Benutzer können auf den Link "Register" in der oberen rechten Ecke registrieren.
- Registrierte Benutzer können auf den Link "Anmelden" anmelden.

Informationen zur Mitgliedschaft werden in einer Datenbank beibehalten, der automatisch erstellt wird. Weitere Informationen zur Formularauthentifizierung in ASP.NET MVC finden Sie unter [Exemplarische Vorgehensweise: Verwenden der Formularauthentifizierung in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualisieren Sie die CSS-Datei

Dieser Schritt ist kosmetischer Natur, aber machen die Seiten, wie die früheren Screenshots gerendert wird.

Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner "Content", und öffnen Sie die Datei mit dem Namen "Site.CSS". Fügen Sie die folgenden CSS-Stile hinzu:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Nächste](using-web-api-with-entity-framework-part-2.md)
