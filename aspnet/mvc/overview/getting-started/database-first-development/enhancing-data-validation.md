---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Datenbank zuerst mit ASP.NET MVC: Verbessern der Datenüberprüfung | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879607"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF Datenbank zuerst mit ASP.NET MVC: Verbessern der Datenüberprüfung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle.
> 
> In diesem Teil der Reihe konzentriert sich auf das Hinzufügen von datenanmerkungen in das Datenmodell geben überprüfungsanforderungen zu erfüllen und die Formatierung anzeigen. Es wurde basierend auf Feedback von Benutzern im Abschnitt "Kommentare" verbessert.


## <a name="add-data-annotations"></a>Hinzufügen von datenanmerkungen

In einer früheren Thema haben Sie gesehen, werden einige Datenüberprüfungsregeln automatisch auf die Benutzereingabe angewendet. Beispielsweise können Sie nur eine Zahl für die Dienstqualität Eigenschaft bereitstellen. Um weitere Daten Validierungsregeln angeben, können Sie Ihrer Modellklasse datenanmerkungen hinzufügen. Diese Anmerkungen werden in der gesamten Webanwendung für die angegebene Eigenschaft angewendet. Sie können auch die Formatierungsattribute anwenden, die sich ändern, wie die Eigenschaften angezeigt werden; ändern z. B. für Beschriftungen verwendeten Wert ein.

In diesem Lernprogramm fügen Sie datenanmerkungen zum Beschränken der Länge der Werte für die FirstName, LastName und der MiddleName-Eigenschaften bereitgestellt. In der Datenbank sind diese Werte auf 50 Zeichen begrenzt. Allerdings wird in der Webanwendung, zeichenbeschränkung derzeit nicht erzwungen. Wenn ein Benutzer mehr als 50 Zeichen für einen dieser Werte bereitstellt, stürzt der Seite "beim Versuch, den Wert in der Datenbank speichern. Sie werden auch Dienstqualität auf Werte zwischen 0 und 4 einschränken.

Öffnen der **Student.cs** in der Datei die **Modelle** Ordner. Fügen Sie der Klasse folgenden hervorgehobenen Code hinzu.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Fügen Sie in Enrollment.cs den folgenden hervorgehobenen Code hinzu.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Erstellen Sie die Projektmappe.

Navigieren Sie zu einer Seite zum Bearbeiten oder ein Teilnehmers zu erstellen. Wenn Sie versuchen, mehr als 50 Zeichen eingeben, wird eine Fehlermeldung angezeigt.

![Anzeigen der Fehlermeldung](enhancing-data-validation/_static/image1.png)

Suchen Sie die Seite zur Bearbeitung der Registrierung, und versucht, eine Klasse über 4 bereitzustellen.

![Bereichsfehler für die Dienstqualität](enhancing-data-validation/_static/image2.png)

Eine vollständige Liste der datenanmerkungen Überprüfung kann auf Klassen und Eigenschaften angewendet werden soll, finden Sie unter [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Hinzufügen von Metadatenklassen

Die überprüfungsattribute direkt die Modellklasse hinzufügen funktioniert, wenn Sie nicht, die beabsichtigen zu ändernde Datenbank; jedoch wenn Änderungen an der Datenbank, und Sie die Modellklasse neu generieren müssen, verlieren alle Attribute Sie den Modell-Klasse angewendet werden mussten. Dieser Ansatz kann sehr ineffizient und Verlust wichtiger Validierungsregeln fehleranfällig sein.

Um dieses Problem zu vermeiden, können Sie eine Metadatenklasse hinzufügen, die die Attribute enthält. Wenn Sie den Modellklasse, um die Metadatenklasse zuordnen, werden diese Attribute auf das Modell angewendet. Bei dieser Vorgehensweise kann der Skriptobjektmodell-Klasse neu generiert werden, ohne dass auch alle Attribute, die auf die Metadatenklasse angewendet wurden.

In der **Modelle** Ordner, fügen Sie eine Klasse mit dem Namen **Metadata.cs**.

![Fügen Sie Metadatenklasse](enhancing-data-validation/_static/image3.png)

Ersetzen Sie den Code in Metadata.cs durch den folgenden Code ein.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Diese Metadatenklassen enthalten alle die überprüfungsattribute aus, denen Sie auf der Modellklassen zuvor schon angewendet wurden. Die **Anzeige** Attribut wird verwendet, um den Wert für Beschriftungen verwendete zu ändern.

Nun müssen Sie die Modellklassen mit den Metadatenklassen zuordnen.

In der **Modelle** Ordner, fügen Sie eine Klasse mit dem Namen **PartialClasses.cs**.

Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Beachten Sie, die jede Klasse als markiert ist eine `partial` -Klasse, und jedes entspricht dem Namen und Namespace wie die Klasse, die automatisch generiert wird. Das Metadaten-Attribut auf die partielle Klasse anwenden, stellen Sie sicher, dass die überprüfungsattribute Daten auf die automatisch generierte Klasse angewendet werden. Diese Attribute werden nicht verloren, wenn Sie die Modellklassen neu generieren, da das Metadatenattribut in partielle Klassen angewendet wird, die nicht erneut generiert werden.

Öffnen Sie die ContosoModel.edmx-Datei, um die automatisch generierten Klassen zu generieren. Noch einmal: mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**. Obwohl Sie die Datenbank nicht geändert haben, wird dieser Prozess die Klassen erneut generieren. In der **aktualisieren** Registerkarte **Tabellen** und **Fertig stellen**.

![Aktualisieren von Tabellen](enhancing-data-validation/_static/image4.png)

Speichern Sie die ContosoModel.edmx-Datei, um die Änderungen zu übernehmen.

Öffnen Sie die Datei Student.cs oder Enrollment.cs, und beachten Sie, dass die Validierungsattribute für Daten, die Sie zuvor angewendet nicht mehr in der Datei enthalten sind. Allerdings wird führen Sie die Anwendung aus, und beachten Sie, dass die Validierungsregeln weiterhin angewendet werden, wenn Sie Daten eingeben.

> [!div class="step-by-step"]
> [Zurück](customizing-a-view.md)
> [Weiter](publish-to-azure.md)
