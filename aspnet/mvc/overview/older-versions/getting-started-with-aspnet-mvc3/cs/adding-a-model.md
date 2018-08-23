---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Hinzufügen eines Modells (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b68f857777b1b69ca401c29261211f40df253e7a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837470"
---
<a name="adding-a-model-c"></a>Hinzufügen eines Modells (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/adding-a-model.md) in diesem Tutorial.


## <a name="adding-a-model"></a>Hinzufügen eines Modells

In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Filmen in einer Datenbank. Diese Klassen werden der "Model" Teil der ASP.NET MVC-Anwendung.

Verwenden Sie eine .NET Framework-Datenzugriff-Technologie als Entity Framework bezeichnet definieren und mit diesen Modellklassen arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt, wird, ein Paradigma für die Entwicklung aufgerufen *Code First*. Code zuerst können Sie Modellobjekte zu erstellen, indem einfache Klassen schreiben. (Diese werden auch bezeichnet als POCO-Klassen, von "Plain-Old CLR Objects" ein.) Sie können dann die Datenbank erstellt haben, im laufenden Betrieb von Klassen, die einen Entwicklungsworkflow sehr übersichtlich und eine schnelle ermöglicht haben.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.

![](adding-a-model/_static/image1.png)

Name der *Klasse* "Movie".

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Fügen Sie die folgenden fünf Eigenschaften, die `Movie` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Wir verwenden die `Movie` Klasse zur Darstellung von Filmen in einer Datenbank. Jede Instanz eine `Movie` eine Zeile in einer Datenbanktabelle, und jede Eigenschaft des Objekts entspricht der `Movie` Klasse wird an eine Spalte in der Tabelle zugeordnet.

Fügen Sie in der gleichen Datei Folgendes `MovieDBContext` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Die `MovieDBContext` Klasse darstellt, die Entity Framework filmdatenbankkontext, die verarbeitet werden, abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank. Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt. Weitere Informationen zu `DbContext` und `DbSet`, finden Sie unter [Produktivitätsverbesserungen für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Damit Sie verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Die vollständige *Movie.cs* Datei ist unten dargestellt.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server Compact

Die `MovieDBContext` erstellten Klasse übernimmt die Aufgabe der Verbindung zur Datenbank und Zuordnung `Movie` Objekte mit Datenbank-Datensätzen. Eine Frage, die Sie wahrscheinlich gebeten, die ist jedoch, nach welcher Datenbank geben sie eine Verbindung herstellen. Sie müssen dies durch das Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.

Öffnen Sie den Stammordner der Anwendung *"Web.config"* Datei. (Nicht die *"Web.config"* Datei die *Ansichten* Ordner.) Die folgende Abbildung beides anzeigen *"Web.config"* Dateien; öffnen die *"Web.config"* Datei rot markiert.

![](adding-a-model/_static/image4.png)

### 

Fügen Sie die folgende Verbindungszeichenfolge für die `<connectionStrings>` Element in der *"Web.config"* Datei.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Das folgende Beispiel zeigt einen Teil der *"Web.config"* -Datei mit der neuen Verbindungszeichenfolge hinzugefügt:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Diese geringe Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.

Als Nächstes erstellen Sie ein neues `MoviesController` -Klasse, die Sie verwenden können, die Filmdaten angezeigt und können Benutzer neue Film Auflistungen zu erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-view.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)
