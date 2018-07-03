---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Database First mit ASP.NET MVC: Optimieren der Datenüberprüfung | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: a328aa8aec2c512d77ddabec31b3785b8742e018
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376713"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF Database First mit ASP.NET MVC: Optimieren der Datenüberprüfung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle.
> 
> Dieser Teil der Serie konzentriert sich auf das Hinzufügen von datenanmerkungen in das Datenmodell, geben Sie die überprüfungsanforderungen zu erfüllen und Formatierung anzeigen. Es wurde basierend auf dem Feedback von Benutzern im Abschnitt "Kommentare" verbessert.


## <a name="add-data-annotations"></a>Hinzufügen von datenanmerkungen

In einem vorhergehenden Thema haben Sie gesehen, gelten einige Regeln für die datenvalidierung automatisch auf die Benutzereingabe. Beispielsweise können Sie nur eine Zahl für die Eigenschaft auf Unternehmensniveau bereitstellen. Um weitere Regeln für die datenvalidierung anzugeben, können Sie Ihrer Modellklasse datenanmerkungen hinzufügen. Diese Anmerkungen werden in Ihre Webanwendung für die angegebene Eigenschaft angewendet. Sie können auch Formatierungsattribute anwenden, die sich ändern, wie die Eigenschaften angezeigt werden; z. B. das Ändern des Werts, der für Symboltitel verwendet.

In diesem Tutorial fügen Sie datenanmerkungen, um die Länge der die Werte für die FirstName, MiddleName und LastName-Eigenschaften zu beschränken. In der Datenbank sind diese Werte auf 50 Zeichen begrenzt. Allerdings wird in der Webanwendung, zeichenbeschränkung derzeit nicht erzwungen. Wenn ein Benutzer mehr als 50 Zeichen für einen dieser Werte bereitstellt, stürzt die Seite beim Versuch, den Wert in der Datenbank speichern. Sie werden auch auf Unternehmensniveau mit Werten zwischen 0 und 4 einschränken.

Öffnen der **Student.cs** Datei die **Modelle** Ordner. Fügen Sie folgenden hervorgehobenen Code zur Klasse hinzu.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Fügen Sie in Enrollment.cs folgenden hervorgehobenen Code hinzu.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Erstellen Sie die Projektmappe.

Navigieren Sie zu einer Seite zum Bearbeiten oder erstellen einen für Schüler und Studenten. Wenn Sie versuchen, mehr als 50 Zeichen eingeben, wird eine Fehlermeldung angezeigt.

![Anzeigen der Fehlermeldung](enhancing-data-validation/_static/image1.png)

Navigieren Sie zu der Seite zum Bearbeiten von Registrierungen und versucht, eine Grade-Eigenschaft über 4 bereitzustellen.

![Bereichsfehler auf Unternehmensniveau](enhancing-data-validation/_static/image2.png)

Eine vollständige Liste von datenanmerkungen Überprüfung können Sie auf Eigenschaften und Klassen anwenden, finden Sie unter [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Hinzufügen von Metadatenklassen

Die Validierungsattribute direkt an die Modellklasse hinzufügen funktioniert, wenn Sie nicht, dass die erwarten zu ändernde Datenbank; aber wenn Ihre Datenbank geändert wird und Sie die Model-Klasse zu generieren müssen, verlieren alle Attribute Sie, die Sie auf die die Model-Klasse angewendet wurde. Dieser Ansatz kann sehr ineffizient und anfällig für Verlust wichtiger Validierungsregeln sein.

Um dieses Problem zu vermeiden, können Sie eine Metadatenklasse hinzufügen, die die Attribute enthält. Wenn Sie die Model-Klasse, auf die Metadatenklasse zuordnen, werden diese Attribute für das Modell angewendet. Bei diesem Ansatz kann die Model-Klasse erneut generiert werden, ohne zu verlieren alle Attribute, die auf die Metadatenklasse angewendet wurden.

In der **Modelle** Ordner, fügen Sie eine Klasse, die mit dem Namen **Metadata.cs**.

![Fügen Sie Metadatenklasse](enhancing-data-validation/_static/image3.png)

Ersetzen Sie den Code in Metadata.cs, durch den folgenden Code.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Diese Metadatenklassen enthalten alle die Validierungsattribute, die Sie zuvor für die Modellklassen angewendet wurden. Die **Anzeige** Attribut zum Ändern des Werts, der für Symboltitel verwendet werden.

Nun müssen Sie die ViewModel-Klassen mit den Metadatenklassen zuordnen.

In der **Modelle** Ordner, fügen Sie eine Klasse, die mit dem Namen **PartialClasses.cs**.

Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Beachten Sie, die jede Klasse, als markiert ist eine `partial` -Klasse, und jedes entspricht dem Namen und Namespace wie die Klasse, die automatisch generiert wird. Das Metadatenattribut auf der partiellen Klasse anwenden, stellen Sie sicher, dass die Attribute für die datenvalidierung der automatisch generierte Klasse angewendet werden. Diese Attribute werden nicht verloren, wenn Sie die ViewModel-Klassen neu generieren, da das Attribut von Metadaten in partiellen Klassen angewendet wird, die nicht erneut generiert werden.

Öffnen Sie die ContosoModel.edmx-Datei, um die automatisch generierte Klassen erneut zu generieren. Wieder mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**. Auch wenn Sie die Datenbank nicht geändert haben, wird dieser Prozess die Klassen neu generieren. In der **aktualisieren** Registerkarte **Tabellen** und **Fertig stellen**.

![Tabellen aktualisieren](enhancing-data-validation/_static/image4.png)

Speichern Sie die ContosoModel.edmx-Datei, um die Änderungen zu übernehmen.

Öffnen Sie die Student.cs oder die Enrollment.cs-Datei, und beachten Sie, dass die Attribute für die datenvalidierung, die Sie zuvor angewendet nicht mehr in der Datei. Allerdings wird führen Sie die Anwendung aus, und beachten Sie, dass die Validierungsregeln immer noch angewendet werden, wenn Sie Daten eingeben.

> [!div class="step-by-step"]
> [Zurück](customizing-a-view.md)
> [Weiter](publish-to-azure.md)
