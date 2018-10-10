---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementieren von grundlegenden CRUD-Funktionen, mit dem Entitätsframework in ASP.NET MVC-Anwendung | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio...
ms.author: riande
ms.date: 10/05/2015
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 08b5d38b38d3323e347f0f849ccc0c25fe49efb9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912655"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementieren von grundlegenden CRUD-Funktionen, mit dem Entitätsframework in ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Contoso University Beispiel Web-Anwendung veranschaulicht, wie ASP.NET MVC 5 mit dem Entity Framework 6 Code First Visual Studio erstellt. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Im vorherigen Tutorial haben Sie eine MVC-Anwendung erstellt, die Entity Framework und SQL Server LocalDB verwendet, um Daten zu speichern und anzuzeigen. In diesem Tutorial haben werden Sie überprüfen und anpassen, erstellen, lesen, aktualisieren, löschen (CRUD) Code, der der MVC-Gerüstbau automatisch für Sie Controller und Ansichten erstellt.

> [!NOTE]
> Es ist üblich, dass das Repositorymuster implementiert wird, um eine Abstraktionsebene zwischen Ihrem Controller und der Datenzugriffsebene zu erstellen. In diesen Tutorials werden keine Repositorys verwendet, um die Verwendung des Entity Frameworks einfach und zielorientiert zu erklären. Informationen zum Implementieren von Repositorys, finden Sie unter den [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

In diesem Tutorial erstellen Sie den folgenden Webseiten:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Erstellen Sie eine Seite "Details"

Der eingerüstete Code für die Schüler/Studenten `Index` Seite außer acht gelassen der `Enrollments` -Eigenschaft, da diese Eigenschaft eine Auflistung enthält. In der `Details` Seite zeigen Sie den Inhalt der Auflistung in eine HTML-Tabelle.

In *Controllers\StudentController.cs*, der Aktionsmethode für den `Details` anzeigen verwendet die [finden](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) Methode zum Abrufen einer einzelnen `Student` Entität.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Der Schlüsselwert als an die Methode übergeben wird die `id` Parameter stammen aus *Routendaten* in die **Details** Link auf der Seite "Index".

### <a name="tip-route-data"></a>Tipp: **Weiterleiten von Daten**

Routendaten sind Daten, die die modellbindung in ein URL-Segment, das in der Routingtabelle angegebene gefunden. Die Standardroute gibt z. B. `controller`, `action`, und `id` Segmente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

In der folgenden URL ordnet die Standardroute `Instructor` als die `controller`, `Index` als die `action` und 1 der `id`; Hierbei handelt es sich um Datenwerte für die Route.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` ist ein Abfragezeichenfolgenwert. Der Modellbinder funktioniert auch, wenn Sie übergeben die `id` als Wert einer Abfragezeichenfolge:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Die URLs werden erstellt, indem `ActionLink` Anweisungen in der Razor-Ansicht. In den folgenden Code der `id` Parameter entspricht die Standardroute, daher `id` den Routendaten hinzugefügt wird.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

In den folgenden Code `courseID` ein Parameters in der Standardroute, stimmt nicht überein, sodass es als Abfragezeichenfolge hinzugefügt wird.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Um die Seite "Details" zu erstellen.

1. Open *Views\Student\Details.cshtml*.

   Jedes Feld wird angezeigt, die mit einem `DisplayFor` Helper, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Nach der `EnrollmentDate` Feld- und direkt vor dem Endtag `</dl>` markieren, fügen Sie den hervorgehobenen Code zum Anzeigen einer Liste der Registrierungen, aus, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Wenn der codeeinzug nach dem Einfügen des Codes falsch ist, drücken Sie die **STRG**+**K**, **STRG**+**D** formatieren Es ist.

    Dieser Code durchläuft die Entitäten in der Navigationseigenschaft `Enrollments`. Für jede `Enrollment` Entität in der Eigenschaft der Kurstitel und die Klasse angezeigt. Der Kurstitel wird abgerufen, von der `Course` Entität, die in gespeichert ist die `Course` Navigationseigenschaft der `Enrollments` Entität. Alle diese Daten wird aus der Datenbank automatisch abgerufen, wenn er benötigt wird. Das heißt, verwenden Sie verzögerten Laden hier. Sie haben keinen *Eager Load* für die `Courses` Navigationseigenschaft, sodass die Registrierungen nicht in derselben Abfrage abgerufen wurden, die die Schüler/Studenten erhalten haben. Stattdessen beim ersten Sie versuchen, Zugriff auf die `Enrollments` Navigationseigenschaft, eine neue Abfrage an die Datenbank gesendet wird, um die Daten abzurufen. Erfahren Sie mehr zum lazy Loading und Eager Load in den [Lesen von verwandten Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.

3. Öffnen Sie die Seite "Details" durch die Anwendung starten (**STRG**+**F5**), wählen die **Schüler/Studenten** Registerkarte, und klicken Sie dann auf die **Details** Link, um Alexander Carson. (Wenn Sie drücken **STRG**+**F5** während der *Details.cshtml* Datei geöffnet ist, erhalten Sie einen HTTP 400-Fehler. Dies ist da Visual Studio versucht, führen Sie die Seite "Details", aber es wurde nicht erreicht, über einen Link, der angibt, die "Student" angezeigt. In diesem Fall "Student/Details" aus der URL entfernen und versuchen Sie es erneut, und schließen Sie den Browser, mit der rechten Maustaste in des Projekts und klicken Sie auf **Ansicht** > **in Browser anzeigen**.)

    Die Liste der Kurse und Klassen für den ausgewählten Studenten wird angezeigt:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

4. Schließen Sie den Browser.

## <a name="update-the-create-page"></a>Aktualisieren der Seite „Erstellen“

1. In *Controllers\StudentController.cs*, ersetzen Sie die <xref:System.Web.Mvc.HttpPostAttribute> `Create` Aktionsmethode mit den folgenden Code. Dieser Code Fügt eine `try-catch` Block und entfernt `ID` aus der <xref:System.Web.Mvc.BindAttribute> Attribut für die erstellte Methode:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Dieser Code fügt die `Student` Entität, die von der ASP.NET MVC-modellbindung, erstellt die `Students` Entität festgelegt, und klicken Sie dann die Änderungen in der Datenbank gespeichert. *Modellieren der Binder* bezieht sich auf die Funktionalität von ASP.NET MVC, das macht es einfacher für Sie arbeiten mit Daten, die über ein Formular übermittelt, eine modellbindung konvertiert gebucht-Formular-Werte in der CLR-Typen und übergibt sie an die Aktionsmethode Parameter. In diesem Fall die modellbindung instanziiert ein `Student` Entität automatisch anhand der Eigenschaft Werte aus der `Form` Auflistung.

    Sie entfernt `ID` Attribut aus der Bindung, da `ID` ist der primäre Schlüsselwert der SQL-Server automatisch festlegt, wenn die Zeile eingefügt wird. Eingabe des Benutzers wird nicht festgelegt. die `ID` Wert.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Sicherheitswarnung: der `ValidateAntiForgeryToken` Attribut hilft dabei, zu verhindern, dass [websiteübergreifende anforderungsfälschung](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe. Es muss ein entsprechendes `Html.AntiForgeryToken()` -Anweisung in die Ansicht, die Sie später sehen werden.

    Die `Bind` -Attribut ist eine Möglichkeit zum Schutz vor *overposting* in erstellungsszenarios. Nehmen wir beispielsweise an die `Student` Entität enthält eine `Secret` -Eigenschaft, die Sie nicht, dass diese Webseite festlegen möchten.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Auch wenn Ihnen keine `Secret` Feld auf der Webseite, ein Hacker könnte ein Tool verwenden, z. B. [Fiddler](http://fiddler2.com/home), oder Schreiben von JavaScript-Code, zum Posten einer `Secret` -Formularwert. Ohne die <xref:System.Web.Mvc.BindAttribute> Attribut beschränkt die Felder, die die modellbindung verwendet wird, wenn es erstellt eine `Student` Instanz<em>,</em> die modellbindung, die erhielt `Secret` -Formularwert und verwenden Sie es zum Erstellen der `Student` die Entitätsinstanz. Dann würde jeder beliebige Wert in Ihre Datenbank aktualisiert werden, den der Hacker für das Formularfeld `Secret` festlegt. Die folgende Abbildung zeigt das Fiddler-Tool beim Hinzufügen der `Secret` Feld (mit dem Wert "OverPost"), um die bereitgestellten Formularwerte.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Der Wert „OverPost“ würde dann erfolgreich der Eigenschaft `Secret` der eingefügten Zeile hinzugefügt werden, obwohl Sie nie beabsichtigt haben, dass die Webseite diese Eigenschaft festlegen kann.

    Es wird empfohlen, verwenden Sie die `Include` Parameter mit der `Bind` Attribut *Whitelist* Felder. Es ist auch möglich, mit der `Exclude` Parameter *Blacklist* Felder, die Sie ausschließen möchten. Der Grund `Include` ist sicherer, besteht darin, dass wenn Sie eine neue Eigenschaft für die Entität hinzufügen, das neue Feld nicht automatisch durch geschützt ist ein `Exclude` Liste.

    Sie können verhindern, dass Overposting in bearbeitungsszenarios wird durch die Entität zuerst aus der Datenbank liest und dem anschließenden Aufrufen `TryUpdateModel`, und eine explizit erlaubte Eigenschaftenliste übergeben. Dies ist die Methode, die in diesen Tutorials verwendet.

    Eine alternative Möglichkeit zum Verhindern von Overposting, die von vielen Entwicklern bevorzugt wird, ist Ansichtsmodelle anstelle von Entitätsklassen mit modellbindung verwenden. Schließen Sie nur die Eigenschaften in dem Ansichtsmodell ein, die Sie aktualisieren möchten. Sobald die MVC-modellbindung abgeschlossen ist, kopieren Sie die ansichtsmodelleigenschaften in die Entitätsinstanz, optional wie z. B. mit einem Tool [AutoMapper](http://automapper.org/). Verwenden Sie Db. Der Eintrag für die Entitätsinstanz, die den Zustand auf Unchanged festgelegt, und legen Sie Property("PropertyName"). IsModified auf "true" in jeder Entitätseigenschaft, die im Ansichtsmodell enthalten ist. Diese Methode funktioniert sowohl im Bearbeitungsszenario als auch im Erstellungsszenario.

    Anders als die `Bind` -Attribut, das `try-catch` Block ist die einzige Änderung, die Sie am eingerüsteten Code vorgenommen haben. Wenn eine Ausnahme abgefangen wird, die von <xref:System.Data.DataException> abgeleitet wird, während die Änderungen gespeichert werden, wird eine generische Fehlermeldung angezeigt. <xref:System.Data.DataException>-Ausnahmen werden manchmal durch etwas außerhalb der Anwendung ausgelöst, und nicht durch einen Programmierfehler. Es wird empfohlen, dass der Benutzer es erneut versucht. Zwar wird es in diesem Beispiel nicht implementiert, aber eine qualitätsorientierte Produktionsanwendung würde die Ausnahme protokollieren. Weitere Informationen finden Sie im Abschnitt **Log for insight (Einblicke durch Protokollierung)** im Artikel [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure) (Überwachung und Telemetrie (Erstellen von realitätsnahen Cloud-Apps mit Azure))](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Der Code in *Views\Student\Create.cshtml* ist ähnlich, wie Sie im gesehen haben *Details.cshtml*, außer dass `EditorFor` und `ValidationMessageFor` Hilfsmethoden werden verwendet, für die einzelnen Felder anstelle von `DisplayFor`. Hier ist der relevante Code ein:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* enthält auch `@Html.AntiForgeryToken()`, das funktioniert mit den `ValidateAntiForgeryToken` -Attribut in den Controller aus, um zu verhindern, [websiteübergreifende anforderungsfälschung](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe.

    Keine Änderungen erforderlich sind, im *Create.cshtml*.

2. Führen Sie die Seite, indem Sie das Programm starten Auswählen der **Schüler/Studenten** Registerkarte, und klicken Sie dann auf **neu erstellen**.

3. Geben Sie Namen und ein ungültiges Datum, und klicken Sie auf **erstellen** auf die folgende Fehlermeldung angezeigt.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Dies ist eine serverseitige Validierung, die Sie in der Standardeinstellung erhalten. In einem späteren Tutorial sehen Sie, wie Sie Attribute hinzufügen, die Code für die clientseitige Validierung generieren. Der folgende hervorgehobene Code zeigt die modellvalidierung in der **erstellen** Methode.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Ändern Sie das Datum in einen gültigen Wert und klicken auf **Erstellen**, damit der neue Student auf der Seite **Index** angezeigt wird.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

5. Schließen Sie den Browser.

## <a name="update-the-edit-httppost-method"></a>Aktualisieren Sie die bearbeiten HttpPost-Methode

1. Ersetzen Sie die <xref:System.Web.Mvc.HttpPostAttribute> `Edit` Aktionsmethode mit den folgenden Code:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > In *Controllers\StudentController.cs*, `HttpGet Edit` Methode (ohne die `HttpPost` Attribut) verwendet die `Find` Methode, um den ausgewählten abzurufen `Student` Entität, wie Sie gesehen haben, in der `Details`Methode. Sie müssen diese Methode nicht ändern.

   Diese Änderungen implementieren eine bewährte Sicherheitsmethode, zu verhindern, dass [overposting](#overpost), der gerüstbauer generiert eine `Bind` Attribut, und die Entität hinzugefügt, von der modellbindung für die Entitätenmenge mit einem "geändert"-Flag erstellt wurde. Code nicht mehr da wird empfohlen, die `Bind` Attribut löscht vorhandenen Daten in Feldern, die nicht aufgeführt, der `Include` Parameter. In Zukunft der MVC-Controller-gerüstbauer aktualisiert werden, damit es generiert keine `Bind` Attribute für den Edit-Methoden.

   Der neue Code liest die vorhandene Entität und ruft <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> Felder aus der Benutzereingabe in den gesendeten Formulardaten aktualisieren. Die automatische änderungsnachverfolgung des Entity Frameworks legt die [EntityState.Modified](<xref:System.Data.EntityState.Modified>) Flag für die Entität. Wenn die ["SaveChanges"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufgerufen wird, die <xref:System.Data.EntityState.Modified> Flag bewirkt, dass das Entity Framework zum Erstellen von SQL-Anweisungen, um die Datenbankzeile zu aktualisieren. [Parallelitätskonflikte](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) werden ignoriert, und alle Spalten der Datenbankzeile aktualisiert werden, einschließlich derjenigen, die der Benutzer nicht geändert hat. (Einem späteren Tutorial wird gezeigt, wie man nebenläufigkeitskonflikte behandelt, und wenn Sie nur einzelne Felder in der Datenbank aktualisiert werden soll, legen Sie die Entität auf [EntityState.Unchanged](<xref:System.Data.EntityState.Unchanged>) und legen Sie einzelne Felder auf [ EntityState.Modified](<xref:System.Data.EntityState.Modified>).)

   Zum Verhindern von Overposting, sind die Felder, die von der Seite "Bearbeiten" aktualisierbar sein sollen in der Whitelist enthalten, in der `TryUpdateModel` Parameter. Derzeit sind keine zusätzlichen von Ihnen geschützten Felder vorhanden. Wenn Sie jedoch die Felder auflisten, die die Modellbindung binden soll, stellen Sie sicher, dass zukünftig hinzugefügte Felder automatisch geschützt sind, bis Sie sie explizit hier hinzufügen.

   Aufgrund dieser Änderungen ist die Methodensignatur der HttpPost-Edit-Methode die HttpGet-Edit-Methode identisch; aus diesem Grund haben Sie die Methode EditPost umbenannt.

   > [!TIP]
   >
   > **Status von Entitäten und das Anfügen und die "SaveChanges"-Methoden**
   >
   > Der Datenbankkontext verfolgt, ob die Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind. Diese Information bestimmt, was passiert, wenn Sie die Methode `SaveChanges` aufrufen. Wenn Sie z. B. eine neue Entität zum Übergeben der [hinzufügen](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) Methode, die Zustand der Entität `Added`. Klicken Sie dann beim Aufrufen der ["SaveChanges"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode, erteilt der Datenbankkontext einen SQL `INSERT` Befehl.
   >
   > Eine Entität wird möglicherweise eine der folgenden [Mitgliedstaaten](xref:System.Data.EntityState):
   >
   > - `Added`. Die Entität in der Datenbank noch nicht vorhanden. Die `SaveChanges` Methode ausgeben muss ein `INSERT` Anweisung.
   > - `Unchanged`. Die Methode `SaveChanges` muss nichts mit dieser Entität tun. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.
   > - `Modified`. Einige oder alle Eigenschaftswerte der Entität wurden geändert. Die `SaveChanges` Methode ausgeben muss ein `UPDATE` Anweisung.
   > - `Deleted`. Die Entität wurde zum Löschen markiert. Die `SaveChanges` Methode ausgeben muss eine `DELETE` Anweisung.
   > - `Detached`. Die Entität wird nicht vom Datenbankkontext nachverfolgt.
   >
   > Statusänderungen werden in einer Desktop-App in der Regel automatisch festgelegt. In einem desktop-Typ der Anwendung Sie lesen eine Entität und nehmen Änderungen an der zugehörigen Eigenschaftswerte. Dadurch wird der Entitätsstatus automatisch auf `Modified` festgelegt. Klicken Sie dann beim Aufrufen `SaveChanges`, Entity Framework generiert eine SQL `UPDATE` -Anweisung, die nur die aktuellen Eigenschaften aktualisiert, die Sie geändert haben.
   >
   > Die nicht verbundene Art der Web-apps ermöglichen nicht für die fortlaufende Sequenz. Die ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , liest eine Entität wird freigegeben, nachdem eine Seite gerendert wird. Wenn die `HttpPost` `Edit` Aktionsmethode aufgerufen wird, eine neue Anforderung aus, und Sie haben eine neue Instanz der der ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), daher Sie manuell den Entitätsstatus auf festgelegt müssen `Modified.` wird beim Aufruf `SaveChanges`, das Entity Framework aktualisiert alle Spalten der Datenbankzeile, da der Kontext nicht wissen, welche Eigenschaften Sie geändert haben.
   >
   > Wenn Sie möchten, dass die SQL `Update` Anweisung, um nur die Felder zu aktualisieren, die der Benutzer tatsächlich geändert, Sie können die ursprünglichen Werte in irgendeiner Weise (z. B. ausgeblendete Felder) speichern, damit sie verfügbar bei sind die `HttpPost` `Edit` Methode wird aufgerufen. Und erstellen eine `Student` Entität mit den ursprünglichen Werten Aufruf der `Attach` Methode, Originalversion der Entität die Entität Werte zu den neuen Werten aktualisieren und dann `SaveChanges.` Weitere Informationen finden Sie unter [ Status der Entität und SaveChanges](/ef/ef6/saving/change-tracking/entity-state) und [Lokaldaten](/ef/ef6/querying/local-data).

   HTML- und Razor-Code in *Views\Student\Edit.cshtml* ist ähnlich, wie Sie im gesehen haben *Create.cshtml*, und es sind keine Änderungen erforderlich.

2. Führen Sie die Seite nach dem Programmstart auswählen die **Kursteilnehmer** Registerkarte klicken und ein **bearbeiten** Hyperlink.

   ![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

3. Ändern Sie einige der Daten, und klicken Sie auf **Speichern**. Die geänderten Daten in der Indexseite angezeigt.

   ![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

4. Schließen Sie den Browser.

## <a name="update-the-delete-page"></a>Aktualisieren der Seite „Delete“ (Löschen)

In *Controllers\StudentController.cs*, der Vorlagencode für die <xref:System.Web.Mvc.HttpGetAttribute> `Delete` -Methode verwendet die `Find` Methode, um den ausgewählten abzurufen `Student` Entität, wie Sie gesehen haben, der `Details` und `Edit` Methoden. Allerdings müssen Sie dieser Methode und der dazugehörigen Ansicht einige Funktionen hinzufügen, um eine benutzerdefinierte Fehlermeldung zu implementieren, wenn der Aufruf von `SaveChanges` fehlschlägt.

Wie Sie bereits bei den Vorgängen zum Aktualisieren und Erstellen gesehen haben, benötigen Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Reaktion auf eine GET-Anforderung aufgerufen wird, zeigt eine Ansicht, die dem Benutzer hat die Möglichkeit, genehmigen oder abzubrechen. Wenn der Benutzer diesen Löschvorgang genehmigt, wird eine POST-Anforderung erstellt. In diesem Fall die `HttpPost` `Delete` Methode wird aufgerufen, und klicken Sie dann die Methode tatsächlich den Löschvorgang ausführt.

Fügen Sie eine `try-catch` -block, um die <xref:System.Web.Mvc.HttpPostAttribute> `Delete` Methode zum Behandeln von Fehlern, die auftreten können, wenn die Datenbank aktualisiert wird. Wenn ein Fehler auftritt, die <xref:System.Web.Mvc.HttpPostAttribute> `Delete` Methodenaufrufe der <xref:System.Web.Mvc.HttpGetAttribute> `Delete` -Methode, und übergeben sie einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die <xref:System.Web.Mvc.HttpGetAttribute> `Delete` -Methode zeigt dann die Bestätigungsseite mit Fehlermeldung Gelegenheit des Benutzers abbrechen oder versuchen Sie es erneut.

1. Ersetzen Sie die <xref:System.Web.Mvc.HttpGetAttribute> `Delete` Aktionsmethode mit den folgenden Code, der Fehlerberichterstattung verwaltet:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Dieser Code akzeptiert eine [optionalen Parameter](https://msdn.microsoft.com/library/dd264739.aspx) , der angibt, ob die Methode nach einem Fehler beim Speichern der Änderungen aufgerufen wurde. Dieser Parameter ist `false` bei der `HttpGet` `Delete` Methode wird aufgerufen, ohne Sie zu einem vorherigen Fehler. Wenn es aufgerufen wird, indem die `HttpPost` `Delete` Methode als Reaktion auf einen Datenbankaktualisierungsfehler, der Parameter ist `true` und eine Fehlermeldung an die Ansicht übergeben wird.

2. Ersetzen Sie die <xref:System.Web.Mvc.HttpPostAttribute> `Delete` Action-Methode (mit dem Namen `DeleteConfirmed`) mit den folgenden Code, der den tatsächlichen Löschvorgang ausführt und jegliche Datenbankaktualisierungsfehler abfängt.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Dieser Code Ruft die ausgewählte Entität ist, ruft dann die [entfernen](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) Methode, um den Status der Entität festzulegen, um `Deleted`. Wenn `SaveChanges` aufgerufen wird, wird eine SQL `DELETE` -Befehl generiert wird. Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der eingerüstete Code, der mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` gerne die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden, um verschiedene Methodenparameter zu enthalten.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention halten und den gleichen Namen für die `HttpPost` und `HttpGet` delete-Methoden.

    Wenn Verbessern der Leistung einer Anwendung mit hohem Volumen eine Priorität ist, können Sie vermeiden, dass eine überflüssige SQL-Abfrage zum Abrufen der zeilenupdates durch, und Ersetzen Sie dabei die Codezeilen, die aufgerufen werden die `Find` und `Remove` Methoden mit den folgenden Code:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Dieser Code instanziiert ein `Student` Entität, die nur den primären Schlüsselwert verwenden und anschließend den Entitätsstatus auf `Deleted`. Das ist alles, was Entity Framework benötigt, um die Entität löschen zu können.

    Wie bereits erwähnt, die `HttpGet` `Delete` Methode nicht die Daten zu löschen. Ausführen eines Löschvorgangs als Reaktion auf eine GET request (oder erstellen Sie zu jeder Aktion bearbeiten, Vorgang, oder einem sonstigen Vorgang, der Daten ändert) stellt ein Sicherheitsrisiko dar. Weitere Informationen finden Sie unter [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther Blog.

3. In *Views\Student\Delete.cshtml*, fügen Sie eine Fehlermeldung zwischen der `h2` Überschrift und `h3` Überschrift, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Führen Sie die Seite, indem Sie das Programm starten Auswählen der **Schüler/Studenten** Registerkarte, und klicken Sie dann auf eine **löschen** Link:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

5. Wählen Sie **löschen** auf der Seite, die besagt, **Opravdu chcete odstranit dies?**.

    Die Indexseite wird ohne den gelöschten Student angezeigt. (Sehen Sie ein Beispiel für den Fehlerbehandlungscode in Aktion in der [Tutorial zur Parallelität](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Schließen von Datenbankverbindungen

Löschen Sie die Kontextinstanz um Datenbankverbindungen zu schließen, und geben Sie die Ressourcen, die sie so bald wie möglich speichern frei, wenn Sie damit fertig sind. Warum der eingerüstete Code bietet ein [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) Methode am Ende der `StudentController` -Klasse im *StudentController.cs*, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Die Basis `Controller` -Klasse bereits implementiert die `IDisposable` Schnittstelle, damit dieser Code einfach eine Außerkraftsetzung für fügt die `Dispose(bool)` Methode, um explizit die Context-Instanz zu löschen.

## <a name="handle-transactions"></a>Verarbeiten von Transaktionen

Standardgemäß implementiert Entity Framework implizit Transaktionen. In Szenarien, in dem Sie Änderungen an mehreren Zeilen oder Tabellen vornehmen, und rufen dann `SaveChanges`, Entity Framework stellt automatisch sicher, dass alle Änderungen erfolgreich abgeschlossen oder alle fehlschlagen. Wenn ein Fehler auftritt, nachdem einige der Änderungen durchgeführt wurden, werden diese Änderungen automatisch zurückgesetzt. Für Szenarien, in dem Sie genauer kontrollieren müssen&mdash;Wenn außerhalb von Entity Framework in einer Transaktion ausgeführten Operationen enthalten sein sollen z. B.&mdash;finden Sie unter [arbeiten mit Transaktionen](/ef/ef6/saving/transactions).

## <a name="summary"></a>Zusammenfassung

Sie verfügen nun über einen vollständigen Satz von Seiten, die für einfache CRUD-Vorgänge ausführen `Student` Entitäten. MVC-Hilfsprogrammen verwendet, um die Elemente der Benutzeroberfläche für die Datenfelder zu generieren. Weitere Informationen zu MVC-Hilfsprogrammen, finden Sie unter [Rendern einer Form mithilfe von HTML-Hilfsprogramme](/previous-versions/aspnet/dd410596(v=vs.98)) (der Artikel ist für MVC 3 aber immer noch relevant ist, Informationen zu MVC 5 ist).

Im nächsten Tutorial erweitern Sie die Funktionalität der Indexseite durch Hinzufügen von sortieren und paging.

Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können.

Links zu anderen Ressourcen des Entity Framework finden Sie im [ASP.NET-Datenzugriff – empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
