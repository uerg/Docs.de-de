---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Hinzufügen eines Modells | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a>Hinzufügen eines Modells
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Kinofilmen in einer Datenbank. Diese Klassen werden die &quot;Modell&quot; Teil von ASP.NET MVC-Anwendung.

Verwenden Sie eine .NET Framework Datenzugriffs-Technologie als bezeichnet den [Entity Framework](https://docs.microsoft.com/ef/) definieren und mit diesen Modellklassen arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt ein Entwicklung Paradigma aufgerufen *Code First*. Code können zuerst Modellobjekte zu erstellen, indem Sie einfache Klassen schreiben. (Diese werden auch bezeichnet als POCO-Klassen aus &quot;Plain-Old CLR-Objekte.&quot;) Sie können dann die Datenbank im Handumdrehen von Klassen, dadurch kann einen sehr sauber und eine schnelle Entwicklungsworkflow erstellt haben. Wenn Sie beim ersten Erstellen der Datenbank erforderlich sind, können Sie dieses Lernprogramm aus, um weitere Informationen zu app-Entwicklung von MVC und EF weiterhin folgen. Sie können dann folgen Tom Fizmakens [ASP.NET Gerüstbau](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) Lernprogramms, das Datenbank beim ersten Ansatz behandelt.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner wählen **hinzufügen**, und wählen Sie dann **Klasse**.

![](adding-a-model/_static/image1.png)

Geben Sie die *Klasse* Namen &quot;Film&quot;.

Fügen Sie die folgenden fünf Eigenschaften, um die `Movie` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Wir verwenden die `Movie` Klasse, um Kinofilmen in einer Datenbank darzustellen. Jede Instanz von einem `Movie` -Objekt wird eine Zeile in einer Datenbanktabelle und jede Eigenschaft der entsprechen der `Movie` Klasse werden an eine Spalte in der Tabelle zugeordnet.

Hinweis: Damit System.Data.Entity und die verknüpfte Klasse verwenden zu können, müssen Sie installieren die [Entity Framework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/). Führen Sie den Link, um weitere Anweisungen aus.

Fügen Sie in der gleichen Datei die folgenden `MovieDBContext` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Die `MovieDBContext` Klasse darstellt, den Entity Framework Film Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank. Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.

Um zu verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Dazu können Sie manuell hinzufügen, die mit-Anweisung, oder Sie können die roten Wellenlinien zeigen, klicken Sie auf `Show potential fixes` , und klicken Sie auf `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Hinweis: Einige nicht verwendete `using` Anweisungen wurden entfernt. Visual Studio wird nicht verwendete Abhängigkeiten als grau angezeigt. Sie können nicht verwendete Abhängigkeiten entfernen, indem Sie mit dem Mauszeiger auf die grauen Abhängigkeiten, klicken Sie auf `Show potential fixes` , und klicken Sie auf **nicht verwendete Using-Direktiven entfernen.**

![](adding-a-model/_static/image3.png)

Schließlich haben wir ein Modell (das M in MVC) hinzugefügt. Im nächsten Abschnitt Arbeiten Sie mit der Datenbank-Verbindungszeichenfolge.

> [!div class="step-by-step"]
> [Zurück](adding-a-view.md)
> [Weiter](creating-a-connection-string.md)
