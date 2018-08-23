---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 8 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 249677c9e0eca248710bf730e57a7aeca5a9b5ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836767"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 8
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Mithilfe von Dynamic Data-Funktionen zum Formatieren und Überprüfen von Daten

Im vorherigen Tutorial implementiert Sie gespeicherte Prozeduren. In diesem Tutorial lernen Sie, wie Dynamic Data-Funktionen für die folgenden Vorteile bieten, kann Folgendes:

- Felder werden automatisch für die Anzeige basierend auf deren Datentyp formatiert.
- Felder werden automatisch überprüft basierend auf deren Datentyp.
- Sie können Metadaten in das Datenmodell zum Anpassen der Formatierung und Validierung Verhalten hinzufügen. Wenn Sie dies tun, Sie die Regeln für Formatierung und Validierung an einer Stelle hinzufügen können und sie können automatisch überall angewendet, greifen Sie mithilfe von Dynamic Data-Steuerelemente.

Um anzuzeigen, wie dies funktioniert, ändern Sie die Steuerelemente, die Sie verwenden, um das Anzeigen und Bearbeiten von Feldern in der vorhandenen *Students.aspx* Seite, und Sie fügen Formatierung und Validierung von Metadaten auf die Felder Name und Datum der `Student` Entitätstyp.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Verwenden von DynamicField- und DynamicControl-Steuerelemente

Öffnen der *Students.aspx* Seite und klicken Sie in der `StudentsGridView` Steuerelement ersetzen die **Namen** und **Registrierungsdatum** `TemplateField` Elemente mit dem folgenden Markup:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Dieses Markup verwendet `DynamicControl` steuert anstelle von `TextBox` und `Label` Steuerelemente in der "Student" nennen Vorlagenfeld und verwendet eine `DynamicField` -Steuerelement für das Registrierungsdatum. Es werden keine Formatzeichenfolgen angegeben.

Hinzufügen einer `ValidationSummary` nach steuern die `StudentsGridView` Steuerelement.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

In der `SearchGridView` -Steuerelement ersetzen, die das Markup für die **Namen** und **Registrierungsdatum** Spalten, wie Sie wurde der `StudentsGridView` zu steuern, mit der Ausnahme weglassen der `EditItemTemplate` Element. Die `Columns` Element der `SearchGridView` Steuerelement enthält jetzt das folgende Markup:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Open *Students.aspx.cs* und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Hinzufügen eines ereignishandlers für der Seite des `Init` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Dieser Code gibt an, dass Dynamic Data Formatierung und Validierung in diesen von datengebundenen Steuerelementen für Felder angeben, wird die `Student` Entität. Sie erhalten eine Fehlermeldung wie im folgenden Beispiel, wenn Sie die Seite ausführen, in der Regel bedeutet dies haben Sie vergessen, rufen Sie die `EnableDynamicData` -Methode in der `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Führen Sie die Seite.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

In der **Registrierungsdatum** Spalte wird die Dauer und das Datum angezeigt, weil der Eigenschaftentyp ist `DateTime`. Sie soll, die später behoben werden.

Vorerst Beachten Sie, dass dynamische Daten automatisch grundlegende Validierung bereitstellt. Klicken Sie beispielsweise auf **bearbeiten**, deaktivieren Sie das Feld Date, und klicken Sie auf **Update**, und Sie sehen, dass dynamische Daten automatisch ein erforderliches Feld. dadurch, da der Wert nicht NULL sein, in das Datenmodell kann. Die Seite zeigt ein Sternchen an, nach Feld und eine Fehlermeldung in die `ValidationSummary` Steuerelement:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Sie weglassen der `ValidationSummary` zu steuern, da Sie auch den Mauszeiger über das Sternchen, um die Fehlermeldung enthalten können:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamische Daten überprüft auch, diese Daten eingegeben haben, der **Registrierungsdatum** Feld ist ein gültiges Datum:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Wie Sie sehen können, ist dies eine generische Fehlermeldung. Im nächsten Abschnitt sehen Sie, wie Nachrichten als auch Validierung und Formatierungsregeln angepasst werden.

## <a name="adding-metadata-to-the-data-model"></a>Hinzufügen von Metadaten des Datenmodells

In der Regel möchten Sie die Funktionalität von dynamischen Daten anpassen. Beispielsweise können Sie ändern, wie Daten angezeigt werden und den Inhalt der Fehlermeldungen. Sie anpassen, in der Regel auch Regeln für die datenvalidierung um bieten mehr Funktionen als was Dynamic Data bietet automatisch basierend auf Datentypen. Zu diesem Zweck erstellen Sie partielle Klassen, die Entitätstypen entsprechen.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** -Projekt, wählen **Verweis hinzufügen**, und fügen einen Verweis auf `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

In der *DAL* Ordner eine neue Klassendatei zu erstellen, nennen Sie es *Student.cs*, und Ersetzen Sie den Vorlagencode in den es durch den folgenden Code.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Dieser Code erstellt eine partielle Klasse für den `Student` Entität. Die `MetadataType` auf diese partielle Klasse angewendete Attribut identifiziert die Klasse, die Sie verwenden, um die Metadaten angeben. Die Metadatenklasse kann einen beliebigen Namen haben, aber mit den Namen der Entität sowie "Metadata" ist allgemein üblich.

Geben Sie die Attribute, die auf Eigenschaften in der Metadatenklasse angewendet Formatierung, Validierung, Regeln und Fehlermeldungen. Die hier gezeigten Attribute müssen die folgenden Ergebnisse:

- `EnrollmentDate` wird als ein Datum (ohne eine Uhrzeit) angezeigt.
- Beide Felder müssen 25 Zeichen oder weniger umfasst, sowie eine benutzerdefinierte Fehlermeldung wird bereitgestellt.
- Beide Felder sind erforderlich, und eine benutzerdefinierte Fehlermeldung wird bereitgestellt.

Führen Sie die *Students.aspx* Seite erneut, und Sie sehen, dass die Datumsangaben ohne Uhrzeit nun angezeigt werden:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Bearbeiten Sie eine Zeile aus, und wiederholen Sie die Werte in den Namensfeldern gelöscht. Die Sternchen, der angibt, Feld Fehler angezeigt, sobald Sie ein Feld verlassen, bevor Sie auf **Update**. Beim Klicken auf **Update**, der angegebene Text der Fehlermeldung wird angezeigt.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Wiederholen Sie den Namen eingeben, die länger als 25 Zeichen sind, klicken Sie auf **Update**, und der angegebene Text der Fehlermeldung wird angezeigt.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Nun, da Sie diese Regeln für Formatierung und Validierung in den Metadaten des Modells eingerichtet haben, die Regeln automatisch ausgeglichen werden auf jeder Seite, das anzeigt, oder Änderungen an diesen Feldern ausführen, kann so lange Sie verwenden `DynamicControl` oder `DynamicField` Steuerelemente. Dies reduziert die Menge von redundanten Code müssen Sie zu schreiben, durch der Programmieren und testen, einfacher macht, und es wird sichergestellt, dass datenformatierung und Validierung in der gesamten Anwendung konsistent.

## <a name="more-information"></a>Weitere Informationen

Dies schließt diese tutorialreihe zu den ersten Schritten mit dem Entity Framework. Weitere Ressourcen können Sie erfahren, wie Sie das Entity Framework verwenden, fahren Sie mit [im ersten Tutorial, in den nächsten Tutorials Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) , oder besuchen Sie den folgenden Websites:

- [Entitätsframework – häufig gestellte Fragen](http://www.ef-faq.org/introduction.html)
- [Im Entity Framework-Team-Blog](https://blogs.msdn.com/b/adonet/)
- [Entitätsframework in der MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entitätsframework in der MSDN-Datenentwicklercenter](https://msdn.microsoft.com/data/ef.aspx)
- [EntityDataSource-Steuerelement Übersicht über Webserver in der MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource-Steuerelement-API-Referenz in der MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework-Foren auf MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie lermans blog](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Vorherige](the-entity-framework-and-aspnet-getting-started-part-7.md)
