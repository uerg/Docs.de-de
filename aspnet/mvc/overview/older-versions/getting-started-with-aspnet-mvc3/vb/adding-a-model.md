---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "Hinzufügen eines Modells (VB) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>Hinzufügen eines Modells (VB)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/adding-a-model.md) dieses Lernprogramm.


## <a name="adding-a-model"></a>Hinzufügen eines Modells

In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank. Diese Klassen werden die "" Modellteil der ASP.NET MVC-Anwendung.

Verwenden Sie eine .NET Framework Datenzugriffs-Technologie, die das Entity Framework genannt definieren und mit diesen Modellklassen arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*. Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben. (Diese werden auch bezeichnet als POCO-Klassen aus "Plain-Old CLR Objects".) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.

![](adding-a-model/_static/image1.png)

Benennen Sie die Klasse "Movie".

Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen. Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.

Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank. Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt. Weitere Informationen zu `DbContext` und `DbSet`, finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `imports` -Anweisung am Anfang der Datei:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Die vollständige *Movie.vb* Datei wird unten gezeigt.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server Compact

Die `MovieDBContext` erstellten Klasse behandelt die Aufgabe der Herstellen einer Verbindung mit der Datenbank und die Zuordnung `Movie` -Objekten, die Datenbankdatensätze. Eine Frage, die möglicherweise aufgefordert, ist jedoch, wie Sie zur Angabe der Datenbank hergestellt wird. Sie erledigen, die Sie durch Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.

Öffnen Sie das Stammverzeichnis der Anwendung *"Web.config"* Datei. (Nicht die *"Web.config"* in der Datei die *Ansichten* Ordner.) Beide die folgenden Abbildung anzeigen *"Web.config"* öffnen; die Dateien der *"Web.config"* Datei rot markiert.

![](adding-a-model/_static/image2.png)

## 

Fügen Sie die folgende Verbindungszeichenfolge, um die `<connectionStrings>` Element in der *"Web.config"* Datei.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Das folgende Beispiel zeigt einen Teil der *"Web.config"* Datei mit der neuen Verbindungszeichenfolge hinzugefügt:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Diese kleine Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.

Als Nächstes müssen Sie ein neues erstellen `MoviesController` -Klasse, die Sie verwenden können, an die Filmdaten angezeigt und ermöglichen Benutzern die neuen Film-Angebote erstellen.

>[!div class="step-by-step"]
[Zurück](adding-a-view.md)
[Weiter](accessing-your-models-data-from-a-controller.md)
