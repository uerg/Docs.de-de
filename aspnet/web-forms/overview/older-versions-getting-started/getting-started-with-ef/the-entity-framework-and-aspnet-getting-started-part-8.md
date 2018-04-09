---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 8 | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 8
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Mithilfe von Dynamic Data-Funktionen zum Formatieren und Überprüfen von Daten

Im vorherigen Lernprogramm implementiert Sie gespeicherte Prozeduren. In diesem Lernprogramm erfahren Sie, wie die Dynamic Data-Funktionen die folgenden Vorteile bieten kann:

- Felder werden automatisch für die Anzeige basierend auf deren Datentyp formatiert.
- Felder werden automatisch überprüft, basierend auf deren Datentyp.
- Sie können Metadaten in das Datenmodell zum Anpassen der Formatierung und Validierung Verhalten hinzufügen. Wenn Sie dazu, Sie die Formatierung und Validierung Regeln, an einer Stelle hinzufügen können, und sie sind automatisch überall angewendet, greifen Sie mithilfe von Dynamic Data-Steuerelemente.

Um anzuzeigen, wie dies funktioniert, ändern Sie die Steuerelemente, die Sie verwenden, um die Anzeige und Bearbeitung von Feldern in der vorhandenen *Students.aspx* Seite, und Sie fügen Formatierung und Validierung Metadaten auf die Felder und Datum der `Student` Entitätstyp.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Verwenden von DynamicField- und DynamicControl-Steuerelemente

Öffnen der *Students.aspx* Seite und klicken Sie in der `StudentsGridView` Steuerung Replace die **Namen** und **Registrierungsdatum** `TemplateField` Elemente durch Folgendes Markup:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Dieses Markup verwendet `DynamicControl` steuert anstelle von `TextBox` und `Label` Steuerelemente in der Student benennen Vorlagenfeld und verwendet eine `DynamicField` -Steuerelement für das Registrierungsdatum. Es werden keine Formatzeichenfolgen angegeben.

Hinzufügen einer `ValidationSummary` nach steuern die `StudentsGridView` Steuerelement.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

In der `SearchGridView` -Steuerelement ersetzen, die das Markup für die **Namen** und **Registrierungsdatum** Spalten, wie Sie wurde der `StudentsGridView` zu steuern, mit der Ausnahme weglassen der `EditItemTemplate` Element. Die `Columns` Element von der `SearchGridView` Steuerelement enthält jetzt das folgende Markup:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Open *Students.aspx.cs* und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Fügen Sie einen Handler für der Seite `Init` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Dieser Code gibt an, der dynamische Daten bereitgestellt wird, Formatierung und Validierung in diesen von datengebundenen Steuerelementen für die Felder der `Student` Entität. Wenn Sie beim Ausführen der Seite eine Fehlermeldung wie im folgenden Beispiel erhalten, es in der Regel bedeutet, Sie haben vergessen, rufen Sie die `EnableDynamicData` Methode in `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Führen Sie die Seite.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

In der **Registrierungsdatum** Spalte, die Zeit wird zusammen mit das Datum angezeigt, weil der Eigenschaftstyp `DateTime`. Sie müssen die später behoben werden kann.

Vorerst Beachten Sie, dass dynamische Daten automatisch grundlegende Validierung bereitstellt. Klicken Sie beispielsweise auf **bearbeiten**, deaktivieren Sie das Feld Date, klicken Sie auf **Update**, und Sie sehen, dass dynamische Daten automatisch ein erforderliches Feld dadurch, da der Wert nicht das Datenmodell auf NULL festgelegt ist. Die Seite wird ein Sternchen angezeigt, nach Feld und eine Fehlermeldung in die `ValidationSummary` Steuerelement:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Sie weglassen der `ValidationSummary` zu steuern, da Sie auch den Mauszeiger über das Sternchen finden in der Fehlermeldung enthalten können:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamische Daten überprüft auch, diese Daten eingegeben werden, der **Registrierungsdatum** Feld ist ein gültiges Datum:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Wie Sie sehen können, ist dies eine generische Fehlermeldung. Im nächsten Abschnitt sehen Sie, wie Nachrichten als auch Prüfung und Formatierungsregeln angepasst.

## <a name="adding-metadata-to-the-data-model"></a>Hinzufügen von Metadaten des Datenmodells

In der Regel möchten Sie die Funktionalität von dynamischen Daten anpassen. Beispielsweise können Sie ändern, wie Daten angezeigt werden und der Inhalt der Fehlermeldungen. Sie anpassen, in der Regel auch Daten von Validierungsregeln zum bieten mehr Funktionen als die dynamische Daten bietet automatisch basierend auf Datentypen. Zu diesem Zweck erstellen Sie partielle Klassen, die Entitätstypen entsprechen.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** -Projekt, wählen **Verweis hinzufügen**, und fügen Sie einen Verweis auf `System.ComponentModel.DataAnnotations`.

[![image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

In der *DAL* Ordner eine neue Klassendatei erstellen, nennen Sie sie *Student.cs*, und Ersetzen Sie den Vorlagencode darin, durch den folgenden Code.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Dieser Code erstellt eine partielle Klasse für die `Student` Entität. Die `MetadataType` auf diese partielle Klasse angewendete Attribut identifiziert die Klasse, die Sie an Metadaten verwenden. Die Metadatenklasse kann einen beliebigen Namen aufweisen, aber mithilfe der Entitätsname plus "Metadaten" üblich ist.

Die Attribute, die auf Eigenschaften in der Metadatenklasse angewendet, geben Sie Formatierung, Validierung, Regeln und Fehlermeldungen. Die hier aufgeführten Attribute müssen die folgenden Ergebnisse:

- `EnrollmentDate` wird als Datum (ohne ein Time) angezeigt.
- Beide Felder müssen 25 Zeichen oder weniger umfasst, sowie eine benutzerdefinierte Fehlermeldung wird bereitgestellt.
- Beide Namensfelder sind erforderlich, und eine benutzerdefinierte Fehlermeldung wird bereitgestellt.

Führen Sie die *Students.aspx* Seite erneut, und sehen Sie, dass die Datumsangaben ohne Zeiten jetzt angezeigt werden:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Bearbeiten Sie eine Zeile aus, und wiederholen Sie die Werte in den Namensfeldern deaktivieren. Das Sternchen, der angibt, Feld Fehler angezeigt werden, sobald Sie ein Feld lassen, bevor Sie auf **Update**. Beim Klicken auf **Update**, die Seite zeigt den Fehlermeldungstext, die Sie angegeben haben.

[![image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Wiederholen Sie den Namen eingeben, die mehr als 25 Zeichen sind, klicken Sie auf **Update**, und die Seite zeigt den Fehlermeldungstext, die Sie angegeben haben.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Nun, dass Sie diese Regeln Formatierung und Validierung der Metadaten des Modells eingerichtet haben, die Regeln automatisch ausgeglichen werden auf jeder Seite, die angezeigt oder können Änderungen an diesen Feldern ausführen, solange Sie verwenden `DynamicControl` oder `DynamicField` Steuerelemente. Dies reduziert die Menge der redundante Code zu schreiben, wodurch Programmierung und das Testen vereinfachen gehalten und sichergestellt, dass datenformatierung und Validierung innerhalb der gesamten Anwendung konsistent sind.

## <a name="more-information"></a>Weitere Informationen

Dies schließt diese Reihe von Lernprogrammen zu den ersten Schritten mit dem Entity Framework. Weitere Ressourcen zum Erlernen der Verwendung von Entity Framework, fahren Sie mit [im ersten Lernprogramm, in den nächsten Lernprogramm Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) oder besuchen Sie die folgenden Websites:

- [Entity Framework häufig gestellte Fragen](http://www.ef-faq.org/introduction.html)
- [Im Entity Framework-Team-Blog](https://blogs.msdn.com/b/adonet/)
- [Entity Framework in der MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework in der MSDN-Entwickler-Rechenzentrum](https://msdn.microsoft.com/data/ef.aspx)
- [EntityDataSource-Steuerelement Übersicht über Webserver in der MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource-Steuerelement-API-Referenz in der MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework-Foren auf MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog des Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Vorherige](the-entity-framework-and-aspnet-getting-started-part-7.md)
