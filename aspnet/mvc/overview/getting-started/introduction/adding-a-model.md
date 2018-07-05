---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Hinzufügen eines Modells | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818462"
---
<a name="adding-a-model"></a>Hinzufügen eines Modells
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In diesem Abschnitt fügen Sie einige Klassen für die Verwaltung von Filmen in einer Datenbank. Diese Klassen bilden die &quot;Modell&quot; Teil der ASP.NET MVC-app.

Verwenden Sie eine .NET Framework-Datenzugriff-Technologie als bezeichnet die [Entity Framework](https://docs.microsoft.com/ef/) definieren und mit diesen Modellklassen arbeiten. Der Entity Framework (häufig als EF bezeichnet) unterstützt, wird, ein Paradigma für die Entwicklung aufgerufen *Code First*. Code zuerst können Sie Modellobjekte zu erstellen, indem einfache Klassen schreiben. (Diese werden auch bezeichnet als POCO-Klassen von &quot;einfache alte CLR-Objekte.&quot;) Sie können dann die Datenbank erstellt haben, im laufenden Betrieb von Klassen, die einen Entwicklungsworkflow sehr übersichtlich und eine schnelle ermöglicht haben. Wenn Sie zum ersten Erstellen der Datenbank erforderlich sind, können Sie dieses Lernprogramm aus, um mehr über MVC und EF-app-Entwicklung erfahren weiterhin folgen. Führen Sie dann Tom Fizmakens [ASP.NET-Gerüstbau](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) Tutorial, in dem die Datenbank-first-Ansatz behandelt.

## <a name="adding-model-classes"></a>Hinzufügen von Modellklassen

In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.

![](adding-a-model/_static/image1.png)

Geben Sie die *Klasse* Namen &quot;Film&quot;.

Fügen Sie die folgenden fünf Eigenschaften, die `Movie` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Wir verwenden die `Movie` Klasse zur Darstellung von Filmen in einer Datenbank. Jede Instanz eine `Movie` eine Zeile in einer Datenbanktabelle, und jede Eigenschaft des Objekts entspricht der `Movie` Klasse wird an eine Spalte in der Tabelle zugeordnet.

Hinweis: Um System.Data.Entity und die verknüpfte Klasse verwenden, müssen Sie installieren die [Entity Framework-NuGet-Pakets](https://www.nuget.org/packages/EntityFramework/). Führen Sie den Link, um weitere Anweisungen zu erhalten.

Fügen Sie in der gleichen Datei Folgendes `MovieDBContext` Klasse:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Die `MovieDBContext` Klasse darstellt, die Entity Framework filmdatenbankkontext, die verarbeitet werden, abrufen, speichern und Aktualisieren von `Movie` -Klasseninstanzen in einer Datenbank. Die `MovieDBContext` leitet sich von der `DbContext` Basisklasse, die vom Entity Framework bereitgestellt.

Damit Sie verweisen können `DbContext` und `DbSet`, müssen Sie die folgende hinzufügen `using` -Anweisung am Anfang der Datei:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Hierzu können Sie manuell hinzufügen, die mit-Anweisung, oder Sie können die roten Wellenlinien zeigen, klicken Sie auf `Show potential fixes` , und klicken Sie auf `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Hinweis: Einige nicht verwendete `using` Anweisungen entfernt wurden. Visual Studio wird nicht verwendete Abhängigkeiten als grau angezeigt. Sie können die nicht verwendete Abhängigkeiten entfernen, indem Sie mit dem Mauszeiger auf die grauen Abhängigkeiten, klicken Sie auf `Show potential fixes` , und klicken Sie auf **nicht verwendete Using-Direktiven entfernen.**

![](adding-a-model/_static/image3.png)

Schließlich haben wir ein Modell (das M in MVC) hinzugefügt. Im nächsten Abschnitt Arbeiten Sie mit der Datenbank-Verbindungszeichenfolge.

> [!div class="step-by-step"]
> [Zurück](adding-a-view.md)
> [Weiter](creating-a-connection-string.md)
