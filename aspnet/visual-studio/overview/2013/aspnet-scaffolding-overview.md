---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: ASP.NET-Gerüstbau in Visual Studio 2013 | Microsoft-Dokumentation
author: tfitzmac
description: ASP.NET-Gerüstbau ist eine neue Funktion, die in Visual Studio 2013 enthalten ist.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836442"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>ASP.NET-Gerüstbau in Visual Studio 2013
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET-Gerüstbau ist eine neue Funktion, die in Visual Studio 2013 enthalten ist.


## <a name="overview"></a>Übersicht

ASP.NET-Gerüstbau ist ein Code-Generierung-Framework für Webanwendungen mit ASP.NET. Visual Studio 2013 umfasst die vorinstallierte Code-Generatoren für MVC und Web-API-Projekte. Sie hinzufügen Gerüstbau zu Ihrem Projekt, wenn Sie schnell Code hinzufügen, die mit Datenmodellen interagieren möchten. Mithilfe von Gerüstbau kann die Zeit für die Entwicklung von standard-Datenvorgänge in Ihrem Projekt verringern.

Standardmäßig Visual Studio 2013 unterstützt nicht das Generieren von Code für eine Web Forms-Projekt, aber Sie mit Web Forms Gerüstbau durch Hinzufügen von MVC-Abhängigkeiten auf das Projekt, oder Installieren einer Erweiterung verwenden können. Beide Ansätze werden unten angezeigt.

Visual Studio 2013 Update 2 (derzeit RC) bietet die Möglichkeit, ASP.NET-Gerüstbau für die Anforderungen Ihres Szenarios zu erweitern. Mit dieser Funktionalität können Sie eine benutzerdefinierte gerüstvorlage erstellen und Dialogfeld "Neues Gerüst hinzufügen" hinzufügen. In der angepassten Vorlage Geben Sie den Code, der generiert wird, wenn ein Elements mit Gerüst hinzufügen. Weitere Informationen finden Sie unter [erstellen eine benutzerdefinierte Gerüstbauer für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um ASP.NET-Gerüstbau verwenden zu können, benötigen Sie Folgendes:

- Microsoft Visual Studio 2013
- Web-Entwicklertools (Teil der Standardinstallation der Visual Studio 2013)
- ASP.NET Web Frameworks und Tools 2013 (Teil der Standardinstallation der Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Hinzufügen eines Elements mit Gerüst zum MVC oder Web-API

Um ein Gerüst hinzuzufügen, mit der rechten Maustaste entweder auf das Projekt oder auf einen Ordner innerhalb des Projekts, und wählen **hinzufügen** – **neues Gerüstelement**, wie in der folgenden Abbildung dargestellt.

![Element mit Gerüst hinzufügen](aspnet-scaffolding-overview/_static/image1.png)

Von der **Gerüst hinzufügen** Fenster, wählen Sie das Gerüst hinzufügen.

![Wählen Sie Gerüst](aspnet-scaffolding-overview/_static/image2.png)

Die **Controller hinzufügen** Fenster bietet Ihnen die Möglichkeit, wählen Sie Optionen für die Generierung des Controllers, z. B., ob Sie die neuen asynchronen Features von Entity Framework 6 verwenden möchten.

![Controller hinzufügen](aspnet-scaffolding-overview/_static/image3.png)

Die entsprechenden Klassen und die Seiten werden für Ihr Szenario erstellt. Die folgende Abbildung zeigt beispielsweise die MVC-Controller und Ansichten, die durch Gerüstbau für eine Modellklasse namens Filme erstellt wurden.

![Die erstellten Dateien](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Hinzufügen eines Elements mit Gerüst zum Web Forms

Um Gerüst hinzuzufügen, das Web Forms-Code generiert, müssen Sie eine Erweiterung zu Visual Studio installieren oder MVC-Abhängigkeiten hinzufügen. Beide Ansätze werden unten angezeigt, jedoch müssen Sie nur eine der folgenden Ansätze ausführen.

### <a name="web-forms-scaffolding-extension"></a>Web Forms-Gerüst-Erweiterung

Sie können Visual Studio-Erweiterung, mit denen Sie Gerüstbau für Web Forms-Projekten verwenden können, installieren. Wählen Sie in Visual Studio **Tools** und dann **Erweiterungen und Updates**. Suchen Sie in diesem Dialogfeld der Visual Studio Gallery für **Web Forms Scaffolding**.

![Installieren von Web Forms-Gerüst](aspnet-scaffolding-overview/_static/image5.png)

Weitere Informationen finden Sie unter [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>MVC-Abhängigkeiten

Wählen Sie zum Hinzufügen der MVC-Abhängigkeiten **hinzufügen** - **neues Gerüstelement**. Wählen Sie im Fenster "Gerüst hinzufügen" **MVC-Abhängigkeiten**, wie unten dargestellt.

![MVC-Abhängigkeiten hinzufügen](aspnet-scaffolding-overview/_static/image6.png)

Es gibt zwei Optionen für den Gerüstbau für MVC. Minimaler und vollständiger. Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Bei Auswahl der Option "vollständig", die mindestens erforderliche Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt. Um Gerüstbau problemlos verwenden zu können, wählen Sie die vollständige Abhängigkeiten.

![Wählen Sie die vollständige Abhängigkeiten](aspnet-scaffolding-overview/_static/image7.png)

Nach dem Hinzufügen von Abhängigkeiten, wird Ihnen eine **"Readme.txt"** Datei. Sorgfältig befolgen Sie die Anweisungen in dieser Datei, um sicherzustellen, dass das Projekt ordnungsgemäß funktioniert.

Wenn Sie die Schritte in der Datei "Readme.txt" abgeschlossen haben, können Sie ein neues gerüstelement hinzufügen, wie im vorherigen Abschnitt über MVC und Web-API dargestellt. Die automatisch generierte Ansichten und Controller funktionieren ordnungsgemäß in Ihrem Projekt.

## <a name="tutorials"></a>Tutorials

Um eine benutzerdefinierte gerüstbauer erstellen zu können, finden Sie unter [erstellen eine benutzerdefinierte Gerüstbauer für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Informationen zum Anpassen der generierten Dateien finden Sie unter [Gewusst wie: Anpassen der generierten Dateien über das Dialogfeld "Neues Gerüstelement"](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Ein Beispiel für die mithilfe von Gerüstbau mit **Database First-Entwicklung**, finden Sie unter [EF Database First mit ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Ein Beispiel für die mithilfe von Gerüstbau in einer **MVC** Projekt, finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Ein Beispiel für die mithilfe von Gerüstbau in einer **Web-API-** Projekt, finden Sie unter [erstellen Sie eine REST-API mit Attributrouting in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
