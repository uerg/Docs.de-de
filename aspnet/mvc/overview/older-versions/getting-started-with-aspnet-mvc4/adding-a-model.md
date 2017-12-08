---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: "Hinzufügen eines Modells | Microsoft Docs"
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a>Hinzufügen eines Modells
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank. Diese Klassen werden die &quot;Modell&quot; Teil der ASP.NET MVC-Anwendung.

Verwenden Sie eine .NET Framework Datenzugriffs-Technologie als bezeichnet den [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) definieren und mit diesen Modellklassen arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*. Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben. (Diese werden auch bezeichnet als POCO-Klassen aus &quot;Plain-Old CLR-Objekte.&quot;) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.

![](adding-a-model/_static/image1.png)

Geben Sie die *Klasse* Namen &quot;Film&quot;.

Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen. Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.

Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank. Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.

Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Die vollständige *Movie.cs* Datei wird unten gezeigt. (Mehrere using-Anweisungen, die nicht benötigt wurden entfernt.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB

Die `MovieDBContext` erstellten Klasse behandelt die Aufgabe der Herstellen einer Verbindung mit der Datenbank und die Zuordnung `Movie` -Objekten, die Datenbankdatensätze. Eine Frage, die möglicherweise aufgefordert, ist jedoch, wie Sie zur Angabe der Datenbank hergestellt wird. Sie erledigen, die Sie durch Hinzufügen von Verbindungsinformationen in der *"Web.config"* -Datei der Anwendung.

Öffnen Sie das Stammverzeichnis der Anwendung *"Web.config"* Datei. (Nicht die *"Web.config"* in der Datei die *Ansichten* Ordner.) Öffnen der *"Web.config"* Datei rot umrandet.

![](adding-a-model/_static/image2.png)

Fügen Sie die folgende Verbindungszeichenfolge, um die `<connectionStrings>` Element in der *"Web.config"* Datei.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Das folgende Beispiel zeigt einen Teil der *"Web.config"* Datei mit der neuen Verbindungszeichenfolge hinzugefügt:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Diese kleine Menge an und XML-Code ist alles, was Sie benötigen, um darzustellen, und speichern die Filmdaten in einer Datenbank zu schreiben.

Als Nächstes müssen Sie ein neues erstellen `MoviesController` -Klasse, die Sie verwenden können, an die Filmdaten angezeigt und ermöglichen Benutzern die neuen Film-Angebote erstellen.

>[!div class="step-by-step"]
[Zurück](adding-a-view.md)
[Weiter](accessing-your-models-data-from-a-controller.md)
