---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "ASP.NET Gerüstbau in Visual Studio 2013 | Microsoft Docs"
author: tfitzmac
description: "ASP.NET Gerüstbau ist eine neue Funktion, die in Visual Studio 2013 enthalten ist."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>ASP.NET Gerüstbau in Visual Studio 2013
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Gerüstbau ist eine neue Funktion, die in Visual Studio 2013 enthalten ist.


## <a name="overview"></a>Übersicht

ASP.NET Gerüstbau ist ein Code-Generierung-Framework für ASP.NET-Webanwendungen. Visual Studio 2013 umfasst die vorinstallierte Code-Generatoren für MVC und Web-API-Projekte. Sie hinzufügen Gerüstbau zu Ihrem Projekt, wenn Sie zum schnellen Hinzufügen von Code die Interaktion mit Datenmodellen erstellen möchten. Mithilfe von Gerüstbau reduzieren die Zeitdauer zum standard-Datenvorgänge in Ihrem Projekt entwickeln.

Standardmäßig Visual Studio 2013 unterstützt keine Generieren von Code für eine Web Forms-Projekt, aber Sie können durch Hinzufügen von MVC-Abhängigkeiten für das Projekt, oder Installieren einer Erweiterung Gerüst mit Web Forms verwenden. Beide Vorgehensweisen sind unten dargestellt.

Visual Studio 2013 Update 2 (derzeit RC) bietet die Möglichkeit zum Erweitern von ASP.NET Gerüstbau entsprechend den Anforderungen Ihres Szenarios. Mit dieser Funktionalität können Sie eine angepasste Gerüstbau-Vorlage erstellen und zum Dialogfeld "Neues Gerüst hinzufügen" hinzufügen. In der benutzerdefinierten Vorlage Geben Sie den Code, der beim Hinzufügen eines gerüstelements generiert wird. Weitere Informationen finden Sie unter [erstellen eine benutzerdefinierte Scaffolder für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um ASP.NET Gerüstbau verwenden zu können, benötigen Sie Folgendes:

- Microsoft Visual Studio 2013
- Web-Entwicklertools (Teil der Standardinstallation der Visual Studio 2013)
- ASP.NET Web Frameworks und Tools 2013 (Teil der Standardinstallation der Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Hinzufügen eines gerüstelements MVC- oder Web-API

Um ein Gerüst hinzuzufügen, mit der rechten Maustaste entweder auf das Projekt oder auf einen Ordner innerhalb des Projekts, und wählen Sie **hinzufügen** – **neues Gerüstelement**, wie in der folgenden Abbildung dargestellt.

![Element mit Gerüst hinzufügen](aspnet-scaffolding-overview/_static/image1.png)

Aus der **Gerüst hinzufügen** Fenster, wählen Sie das Gerüst hinzufügen.

![Wählen Sie Gerüst](aspnet-scaffolding-overview/_static/image2.png)

Die **Controller hinzufügen** Fenster bietet Ihnen die Möglichkeit, wählen Sie Optionen für das Generieren des Controllers, z. B. ob Sie die neue asynchrone Funktionen von Entity Framework 6 verwenden möchten.

![Hinzufügen eines Controllers](aspnet-scaffolding-overview/_static/image3.png)

Die relevanten Klassen und Seiten sind für Ihr Szenario erstellt. Die folgende Abbildung zeigt beispielsweise die MVC-Controller und Ansichten, die über das Gerüst für eine Modellklasse namens Filme erstellt wurden.

![Die erstellten Dateien](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Hinzufügen eines gerüstelements Web Forms

Um Gerüst hinzuzufügen, der Web Forms-Code generiert, müssen Sie eine Erweiterung zu Visual Studio installieren oder Hinzufügen von MVC-Abhängigkeiten. Beide Vorgehensweisen sind unten aufgeführt, allerdings müssen Sie nur einen der folgenden Ansätze sind.

### <a name="web-forms-scaffolding-extension"></a>Web Forms-Erweiterung Gerüstbau

Sie können eine Visual Studio-Erweiterung, mit denen Sie Gerüstbau mit einer Web Forms-Projekt verwenden können, installieren. Wählen Sie in Visual Studio **Tools** und dann **Erweiterungen und Updates**. Suchen Sie in diesem Dialogfeld der Visual Studio Gallery für **Web Forms Gerüstbau**.

![Installieren Sie WebForms Gerüstbau](aspnet-scaffolding-overview/_static/image5.png)

Weitere Informationen finden Sie unter [Web Forms Gerüstbau](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>MVC-Abhängigkeiten

Wählen Sie zum Hinzufügen von MVC-Abhängigkeiten **hinzufügen** - **neues Gerüstelement**. Wählen Sie im Fenster Gerüst hinzufügen **MVC-Abhängigkeiten**, wie unten dargestellt.

![Hinzufügen von MVC-Abhängigkeiten](aspnet-scaffolding-overview/_static/image6.png)

Es gibt zwei Optionen für MVC Gerüstbau; Minimal "und" vollständig ". Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Wenn Sie die Option "vollständig", wählen Sie die minimale Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt. Um Gerüstbau einfach zu verwenden, aktivieren Sie vollständigen Abhängigkeiten.

![Wählen Sie die vollständige Abhängigkeiten](aspnet-scaffolding-overview/_static/image7.png)

Nach dem Hinzufügen der Abhängigkeiten, sehen Sie eine **"Readme.txt"** Datei. Gehen Sie sorgfältig vor den Anweisungen in dieser Datei, um sicherzustellen, dass das Projekt ordnungsgemäß funktioniert.

Wenn Sie die Schritte in der Datei "Readme.txt" abgeschlossen haben, können Sie ein neues Gerüst hinzufügen, wie im vorherigen Abschnitt zu MVC und Web-API dargestellt. Der automatisch generierten Sichten und der Controller funktioniert ordnungsgemäß innerhalb des Projekts.

## <a name="tutorials"></a>Lernprogramme

Um eine benutzerdefinierte Scaffolder erstellen zu können, finden Sie unter [erstellen eine benutzerdefinierte Scaffolder für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Zum Anpassen der generierten Dateien finden Sie unter [Gewusst wie: Anpassen der generierten Dateien über das Dialogfeld "Neues Gerüstelement"](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Ein Beispiel für mithilfe von Gerüstbau mit **Database First-Entwicklung**, finden Sie unter [EF Database First mit ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Ein Beispiel für mithilfe von Gerüstbau in ein **MVC** Projekt, finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Ein Beispiel für mithilfe von Gerüstbau in einem **Web-API** Projekt, finden Sie unter [erstellen Sie eine REST-API mit Routing-Attribut in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
