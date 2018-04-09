---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementieren grundlegende CRUD-Funktionalität mit Entity Framework in ASP.NET MVC-Anwendung | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 14f5143bb5086890d4a2f2fb3b98f1be88a549a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementieren grundlegende CRUD-Funktionalität mit Entity Framework in ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm erstellt Sie eine MVC-Anwendung, die gespeichert und Daten mit dem Entity Framework und SQL Server LocalDB angezeigt. In diesem Lernprogramm müssen Sie überprüfen und anpassen, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Code, der die MVC-Gerüstbau automatisch für Sie in den Controller und Ansichten erstellt.

> [!NOTE]
> Es ist üblich, dass das Repositorymuster implementiert wird, um eine Abstraktionsebene zwischen Ihrem Controller und der Datenzugriffsebene zu erstellen. In diesen Tutorials werden keine Repositorys verwendet, um die Verwendung des Entity Frameworks einfach und zielorientiert zu erklären. Informationen zum Implementieren des Repositorys, finden Sie unter der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).


In diesem Lernprogramm erstellen Sie den folgenden Webseiten:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Erstellen Sie eine Seite "Details"

Der scaffolded Code für den Studenten `Index` Seite außer acht gelassen der `Enrollments` -Eigenschaft, da diese Eigenschaft eine Auflistung enthält. In der `Details` Seite zeigen Sie den Inhalt der Auflistung in einer HTML-Tabelle.

 In *Controllers\StudentController.cs*, der Aktionsmethode für den `Details` anzeigen verwendet die [suchen](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) Methode zum Abrufen einer einzelnes `Student` Entität. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Der Schlüsselwert wird an die Methode als übergeben der `id` Parameter stammen aus *Weiterleiten von Daten* in der **Details** Link auf der Seite "Index".

### <a name="tip-route-data"></a>Tipp: **Weiterleiten von Daten**

Routendaten sind Daten, die der Modellbinder in ein URL-Segment angegeben, in der Routingtabelle gefunden. Die Standardroute gibt z. B. `controller`, `action`, und `id` Segmente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

In der folgenden URL ordnet die Standardroute `Instructor` als die `controller`, `Index` als die `action` und 1 der `id`; Hierbei handelt es sich um Datenwerte für die Route.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? CourseID 2021 =" ist ein Wert der Abfragezeichenfolge. Der Modellbinder funktioniert auch, wenn Sie übergeben die `id` als Abfragezeichenfolgenwert:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Die URLs werden erstellt, indem `ActionLink` Anweisungen in der Razor-Ansicht. Im folgenden Code wird die `id` Parameter entspricht der Standardroute daher `id` die Routendaten hinzugefügt wird.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Im folgenden Code `courseID` einen Parameter in die Standardroute stimmt nicht überein, damit sie als eine Abfragezeichenfolge hinzugefügt wird.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Open *Views\Student\Details.cshtml*. Jedes Feld wird angezeigt, mit einem `DisplayFor` Helper, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Nach der `EnrollmentDate` Felds und unmittelbar vor der schließenden `</dl>` kennzeichnen, fügen Sie den hervorgehobenen Code, um eine Liste der Bereitstellungen, anzuzeigen, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Wenn der Codeeinzug nach dem Einfügen des Codes falsch ist, drücken Sie Strg + K + D, um diesen zu korrigieren.

    Dieser Code durchläuft die Entitäten in der Navigationseigenschaft `Enrollments`. Für jede `Enrollment` Entität in der Eigenschaft den Kurstitel und die Dienstqualität dargestellt angezeigt. Der Kurstitel wird abgerufen, von der `Course` Entität, die in gespeichert ist die `Course` Navigationseigenschaft der `Enrollments` Entität. Alle diese Daten werden aus der Datenbank abgerufen automatisch bei Bedarf. (Das heißt, verwenden Sie lazy loading, hier. Sie haben keinen *unverzüglichem Laden* für die `Courses` Navigationseigenschaft, damit die Registrierung nicht in derselben Abfrage abgerufen wurden, die die Schüler erhalten haben. Stattdessen erstmalig Sie zuzugreifen versuchen, den `Enrollments` Navigationseigenschaft, eine neue Abfrage an die Datenbank gesendet wird, um die Daten abzurufen. Erfahren Sie mehr über verzögertes Laden und unverzüglichem Laden in das [Lesen verknüpfter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.)
3. Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf eine **Details** Link, um Alexander Carson. (Wenn Sie STRG + F5 drücken, während die Details.cshtml-Datei geöffnet ist, einen HTTP 400-Fehler erhalten Sie da Visual Studio versucht, führen Sie die Seite "Details", aber es wurde nicht über einen Link, der angibt, die Studenten anzuzeigenden erreicht. -Verarbeitung, nur "Student/Details" aus der URL entfernen und versuchen Sie es erneut, oder schließen Sie den Browser rechten Maustaste auf das Projekt, und klicken Sie auf **Ansicht**, und klicken Sie dann auf **in Browser anzeigen**.)

    Die Liste der Kurse und Klassen für den ausgewählten Studenten wird angezeigt:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Aktualisieren Sie die Seite "erstellen"

1. In *Controllers\StudentController.cs*, ersetzen die `HttpPost``Create` Aktionsmethode mit den folgenden Code zum Hinzufügen einer `try-catch` blockieren, und entfernen Sie `ID` aus der [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) für die scaffolded-Methode:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Dieser Code fügt der `Student` Entität erstellt wurde, indem Sie die ASP.NET MVC-modellbindung an die `Students` Entität festgelegt, und klicken Sie dann die Änderungen in der Datenbank gespeichert. (*Modellieren Binder* bezieht sich auf die ASP.NET MVC-Funktionalität, die macht es einfacher, für die Sie zum Arbeiten mit Daten, die von einem Formular übermittelt; einen Modellbinder konvertiert gebucht Form-Werte in CLR-Typen und übergibt sie an die Aktionsmethode im Parameters. Der Modellbinder Druckmethode instanziiert einen `Student` Entität für die Eigenschaft verwenden Sie die Werte aus der `Form` Auflistung.)

    Sie entfernt `ID` Attribut aus der Bindung, da `ID` wird der Primärschlüsselwert der SQL Server automatisch festgelegt wird, wenn die Zeile eingefügt wird. Eingaben des Benutzers ist nicht festgelegt. die `ID` Wert.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Sicherheitswarnung - die `ValidateAntiForgeryToken` Attribut verhindert [websiteübergreifende anforderungsfälschung](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe. Es muss ein entsprechendes `Html.AntiForgeryToken()` -Anweisung in der Sicht, die Sie später sehen.

    Die `Bind` -Attribut ist eine Möglichkeit zum Schutz vor *zu stark Posten von Beiträgen* in Szenarien zu erstellen. Nehmen wir beispielsweise an die `Student` Entität enthält eine `Secret` -Eigenschaft, die Sie nicht, dass dieser Webseite festlegen möchten.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Auch wenn Ihnen keine `Secret` Feld auf der Webseite, die ein Hacker konnte ein Tool verwenden, z. B. [Fiddler](http://fiddler2.com/home), oder Schreiben von JavaScript, zum Bereitstellen einer `Secret` Wert bilden. Ohne die [binden](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) Attribut beschränken die Felder, die der Modellbinder verwendet werden, wenn es erstellt eine `Student` Instanz<em>,</em> der Modellbinder würde, die abholen `Secret` Wert bilden, und verwenden sie Erstellen der `Student` Entitätsinstanz. Dann würde jeder beliebige Wert in Ihre Datenbank aktualisiert werden, den der Hacker für das Formularfeld `Secret` festlegt. Die folgende Abbildung zeigt die Fiddler Tool Hinzufügen der `Secret` Feld (mit dem Wert "OverPost"), um die übermittelte Formularwerte.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    Der Wert „OverPost“ würde dann erfolgreich der Eigenschaft `Secret` der eingefügten Zeile hinzugefügt werden, obwohl Sie nie beabsichtigt haben, dass die Webseite diese Eigenschaft festlegen kann.

    Es ist eine bewährte Sicherheitsmethode verwendet die `Include` Parameter mit der `Bind` -Attribut auf *Positivliste* Felder. Es ist auch möglich, verwenden Sie die `Exclude` Parameter *Blacklist* Felder, die Sie ausschließen möchten. Der Grund `Include` ist sicherer, wenn Sie eine neue Eigenschaft für die Entität hinzufügen, wird das neue Feld durch nicht automatisch geschützt ist ein `Exclude` Liste.

    Sie können verhindern, dass Overposting in Szenarien bearbeiten ist, indem Sie zuerst die Entität aus der Datenbank liest und dem anschließenden Aufrufen `TryUpdateModel`, und übergeben Sie eine explizite zulässigen Eigenschaften-Liste. Dies ist die Methode, die in diesen Lernprogrammen verwendet.

    Eine alternative Möglichkeit, Overposting zu vermeiden, die von vielen Entwicklern bevorzugt wird, ist Ansichtsmodelle anstelle von Entitätsklassen mit wurden die modellbindung verwenden. Schließen Sie nur die Eigenschaften in dem Ansichtsmodell ein, die Sie aktualisieren möchten. Sobald die MVC-modellbindung abgeschlossen ist, kopieren Sie die Eigenschaften der anzeigen-Modell in die Entitätsinstanz, die mit optional ein Tool wie [AutoMapper](http://automapper.org/). Verwenden Sie Db. Der Eintrag für die Entitätsinstanz, legen Sie dessen Status auf unverändert, und legen Sie dann Property("PropertyName"). IsModified auf "true" für jede Eigenschaft der Entität, die in das Ansichtsmodell enthalten ist. Diese Methode funktioniert sowohl im Bearbeitungsszenario als auch im Erstellungsszenario.

    Anders als die `Bind` -Attribut, das `try-catch` Block ist die einzige Änderung, die Sie an den scaffolded Code vorgenommen haben. Wenn eine Ausnahme, die abgeleitet [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) wird erfasst, während die Änderungen gespeichert werden, wird eine allgemeine Fehlermeldung angezeigt. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) Ausnahmen werden manchmal etwas anderes außerhalb der Anwendung statt auf einen Programmierfehler verursacht, damit der Benutzer informiert wird, versuchen Sie es erneut. Zwar wird es in diesem Beispiel nicht implementiert, aber eine qualitätsorientierte Produktionsanwendung würde die Ausnahme protokollieren. Weitere Informationen finden Sie im Abschnitt **Log for insight (Einblicke durch Protokollierung)** im Artikel [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure) (Überwachung und Telemetrie (Erstellen von realitätsnahen Cloud-Apps mit Azure))](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Der Code in *Views\Student\Create.cshtml* ähnelt der im haben gesehen *Details.cshtml*, außer dass `EditorFor` und `ValidationMessageFor` Hilfen für jedes Feld statt dienen`DisplayFor`. Hier wird der relevante Code ein:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* enthält auch `@Html.AntiForgeryToken()`, dem arbeitet mit der `ValidateAntiForgeryToken` -Attribut in den Controller aus, um zu verhindern, [websiteübergreifende anforderungsfälschung](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe.

    Keine Änderungen erforderlich sind, im *Create.cshtml*.
2. Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf **neu erstellen**.
3. Geben Sie Namen und ein ungültiges Datum, und klicken Sie auf **erstellen** um die Fehlermeldung anzuzeigen.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Dies ist eine serverseitige Validierung, die Sie standardgemäß erhalten. In einem späteren Tutorial sehen Sie, wie Sie Attribute hinzufügen, die auch Code für die clientseitige Validierung generieren. Die folgende hervorgehobene Code zeigt die Modell-Überprüfung in der **erstellen** Methode.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Ändern Sie das Datum in einen gültigen Wert und klicken auf **Erstellen**, damit der neue Student auf der Seite **Index** angezeigt wird.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Aktualisieren Sie die Bearbeitung HttpPost-Methode

In *Controllers\StudentController.cs*, die `HttpGet` `Edit` Methode (der die `HttpPost` Attribut) verwendet die `Find` Methode zum Abrufen der ausgewählten `Student` Entität, als Sie gesehen haben in der `Details` Methode. Sie müssen diese Methode nicht ändern.

Ersetzen Sie jedoch die `HttpPost` `Edit` -Aktionsmethode durch folgenden Code:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Diese Änderungen implementieren eine bewährte Sicherheitsmethode, um zu verhindern, dass [overposting](#overpost), die Scaffolder generiert eine `Bind` Attribut, und die Entität erstellt, indem die Modellbinder für die Entität mit einem "geändert"-Flag festgelegt hinzugefügt. Code nicht mehr da wird empfohlen, die `Bind` Attribut löscht vorhandenen Daten in Feldern, die nicht aufgeführt, der `Include` Parameter. In der Zukunft Scaffolder der MVC-Controller wird aktualisiert, damit es keinen generiert `Bind` Attribute für Methoden bearbeiten.

Der neue Code liest, die vorhandene Entität und ruft [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) Felder aus den Benutzereingaben in den bereitgestellten Formulardaten aktualisieren. Die automatische änderungsnachverfolgung der Entity Framework legt die ["geändert"](https://msdn.microsoft.com/library/system.data.entitystate.aspx) Flag für die Entität. Wenn die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufgerufen wird, die `Modified` Kennzeichnung bewirkt, dass das Entity Framework zum Erstellen von SQL-Anweisungen, um die Datenbankzeile zu aktualisieren. [Parallelitätskonflikte](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) werden ignoriert, und alle Spalten der Datenbankzeile aktualisiert werden, einschließlich derer, die der Benutzer nicht geändert haben. (Einem späteren Lernprogramm veranschaulicht, um Parallelitätskonflikte zu behandeln, und wenn Sie nur einzelne Felder in der Datenbank aktualisiert werden soll, Sie können die Entität auf Unchanged festlegen und einzelne Felder "geändert".)

Als bewährte Methode Overposting zu verhindern, werden die Felder, die durch das Bearbeiten (Seite) aktualisierbar sein sollen in der Zulassungsliste enthalten in der `TryUpdateModel` Parameter. Derzeit sind keine zusätzlichen von Ihnen geschützten Felder vorhanden. Wenn Sie jedoch die Felder auflisten, die die Modellbindung binden soll, stellen Sie sicher, dass zukünftig hinzugefügte Felder automatisch geschützt sind, bis Sie sie explizit hier hinzufügen.

Als Ergebnis dieser Änderungen ist die Methodensignatur der HttpPost Edit-Methode HttpGet Edit-Methode identisch. aus diesem Grund haben Sie die Methode EditPost umbenannt.

> [!TIP] 
> 
> **Status der Entität und das Anfügen "und" SaveChanges-Methoden**
> 
> Der Datenbankkontext verfolgt, ob die Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind. Diese Information bestimmt, was passiert, wenn Sie die Methode `SaveChanges` aufrufen. Wenn Sie z. B. eine neue Entität zum Übergeben der [hinzufügen](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) Methode, die Zustand der Entität, um festgelegt ist `Added`. Klicken Sie dann beim Aufrufen der [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode, der Datenbankkontext stellt eine SQL `INSERT` Befehl.
> 
> Eine Entität in einem der möglicherweise die[folgende Zustände](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added` Die Entität ist noch nicht in der Datenbank vorhanden. Die `SaveChanges` Methode ausstellen muss ein `INSERT` Anweisung.
> - `Unchanged` Die Methode `SaveChanges` muss nichts mit dieser Entität tun. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.
> - `Modified` Einige oder alle Eigenschaftswerte der Entität wurden geändert. Die `SaveChanges` Methode ausstellen muss ein `UPDATE` Anweisung.
> - `Deleted` Die Entität wurde zum Löschen markiert. Die `SaveChanges` Methode ausstellen muss eine `DELETE` Anweisung.
> - `Detached` Die Entität wird nicht vom Datenbankkontext nachverfolgt.
> 
> Statusänderungen werden in einer Desktop-App in der Regel automatisch festgelegt. In einem desktop-Typ der Anwendung eine Entität zu lesen und nehmen Sie Änderungen an einige Eigenschaftswerte. Dadurch wird der Entitätsstatus automatisch auf `Modified` festgelegt. Klicken Sie dann beim Aufrufen `SaveChanges`, Entity Framework generiert eine SQL `UPDATE` -Anweisung, die nur die tatsächlichen Eigenschaften aktualisiert, die Sie geändert haben.
> 
> Die getrennte Art der Web-apps ist nicht für die fortlaufende Sequenz ermöglichen. Die [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , liest eine Entität wurde verworfen, nachdem eine Seite gerendert wird. Wenn die `HttpPost` `Edit` Aktionsmethode aufgerufen wird, erfolgt eine neue Anforderung aus, und Sie haben eine neue Instanz der der [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), daher Sie manuell festlegen der Entitätszustand müssen `Modified.` dann beim Aufrufen `SaveChanges`, Entity Framework werden alle Spalten der Datenbankzeile aktualisiert, da der Kontext kann nicht wissen, welche Eigenschaften Sie geändert haben.
> 
> Ggf. die SQL `Update` Anweisung, um nur die Felder aktualisieren, die der Benutzer tatsächlich geändert wird, können Sie die ursprünglichen Werte in irgendeiner Form (z. B. ausgeblendete Felder) speichern, sodass verfügbar wenn sind die `HttpPost` `Edit` -Methode aufgerufen wird. Anschließend können Sie erstellen eine `Student` Entität mit den ursprünglichen Werten Aufruf der `Attach` Methode mit diesem Originalversion der Entität, Werten der Entität mit den neuen Werten aktualisiert, und rufen dann `SaveChanges.` Weitere Informationen finden Sie unter [ Status der Entität und SaveChanges](https://msdn.microsoft.com/data/jj592676) und [lokale Daten](https://msdn.microsoft.com/data/jj592872) im MSDN Data Developer Center.


Der HTML- und Razor-Code in *Views\Student\Edit.cshtml* ähnelt der im haben gesehen *Create.cshtml*, und es sind keine Änderungen erforderlich.

Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte, und klicken Sie dann auf eine **bearbeiten** Link.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Ändern Sie einige der Daten, und klicken Sie auf **Speichern**. Sie sehen die geänderten Daten in die Indexseite.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Aktualisieren die Seite "löschen"

In *Controllers\StudentController.cs*, der Vorlagencode für die `HttpGet` `Delete` -Methode verwendet die `Find` Methode zum Abrufen der ausgewählten `Student` Entität, als Sie gesehen haben, der `Details` und `Edit` Methoden. Allerdings müssen Sie dieser Methode und der dazugehörigen Ansicht einige Funktionen hinzufügen, um eine benutzerdefinierte Fehlermeldung zu implementieren, wenn der Aufruf von `SaveChanges` fehlschlägt.

Wie Sie bereits bei den Vorgängen zum Aktualisieren und Erstellen gesehen haben, benötigen Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Antwort auf eine GET-Anforderung aufgerufen wird, zeigt eine Sicht, die dem Benutzer hat die Möglichkeit, zu genehmigen, oder brechen Sie den Löschvorgang. Wenn der Benutzer diesen Löschvorgang genehmigt, wird eine POST-Anforderung erstellt. In diesem Fall die `HttpPost` `Delete` Methode wird aufgerufen, und diese Methode führt dann tatsächlich den Löschvorgang.

Fügen Sie eine `try-catch` -block, um die `HttpPost` `Delete` Methode zum Behandeln von Fehlern, die auftreten können, wenn die Datenbank aktualisiert wird. Wenn ein Fehler auftritt, die `HttpPost` `Delete` Methodenaufrufe der `HttpGet` `Delete` -Methode, und übergeben sie einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die `HttpGet Delete` Methode Kriterienbereich klicken Sie dann die Seite "Bestätigung" zusammen mit der Fehlermeldung, dass der Benutzer eine Möglichkeit zum Abbrechen oder Wiederholen Sie den Vorgang.

1. Ersetzen Sie die `HttpGet` `Delete` -Aktionsmethode durch folgenden Code wird die Fehlerberichterstattung verwaltet: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Dieser Code akzeptiert ein [Optionaler Parameter](https://msdn.microsoft.com/library/dd264739.aspx) , der angibt, ob die Methode, nach einem Fehler aufgerufen wurde, um Änderungen zu speichern. Dieser Parameter ist `false` bei der `HttpGet` `Delete` Methode wird aufgerufen, ohne Sie zu einem vorherigen Fehler. Bei Aufruf durch die `HttpPost` `Delete` Methode als Reaktion auf ein Update Fehler, der Parameter ist `true` und eine Fehlermeldung an die Ansicht übergeben wird.
2. Ersetzen Sie die `HttpPost` `Delete` Aktionsmethode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code dem führt des eigentlichen Löschvorgangs und fängt alle Datenbank-Update-Fehler ab.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     Dieser Code Ruft die ausgewählte Entität ruft dann die [entfernen](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) Methode, um die Entität Status festgelegt wird, um `Deleted`. Wenn `SaveChanges` aufgerufen wird, wird eine SQL `DELETE` Befehl generiert wird. Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der scaffolded Code mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` so erteilen Sie die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden zum anderen Methodenparameter angegeben haben.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention einhalten und den gleichen Namen für die `HttpPost` und `HttpGet` Löschmethoden.

     Ist das Verbessern der Leistung einer Anwendung hoher Priorität, vermeiden Sie unnötige SQL-Abfrage zum Abrufen der zeilenupdates durch Ersetzen der Codezeilen, die aufgerufen werden der `Find` und `Remove` Methoden mit den folgenden Code:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     Dieser Code instanziiert einen `Student` Entität mithilfe den primären Schlüsselwert und dann wird der Entitätszustand `Deleted`. Das ist alles, was Entity Framework benötigt, um die Entität löschen zu können.

     Wie bereits erwähnt, die `HttpGet` `Delete` Methode löscht die Daten. Ausführen einer Delete-Vorgangs als Antwort auf einen GET-Befehl anfordern (oder zu Ausführen von Vorgängen bearbeiten zu erstellen, Vorgang oder einem sonstigen Vorgang, der Daten ändert) stellt ein Sicherheitsrisiko dar. Weitere Informationen finden Sie unter [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther Blog.
3. In *Views\Student\Delete.cshtml*, fügen Sie eine Fehlermeldung, die zwischen den `h2` Überschrift und der `h3` Überschrift, wie im folgenden Beispiel gezeigt:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf eine **löschen** Link:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. Klicken Sie auf **Löschen**. Die Indexseite wird ohne den gelöschten Student angezeigt. (Sehen Sie ein Beispiel für den Fehlerbehandlungscode in Aktion in der [Parallelität Lernprogramm](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Schließen von Datenbankverbindungen

Löschen Sie die Kontextinstanz um Datenbankverbindungen zu schließen und Freigeben der Ressourcen, die sie so bald wie möglich zu halten, wenn Sie damit fertig sind. Also warum der scaffolded Code bietet ein [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) Methode am Ende der `StudentController` -Klasse im *StudentController.cs*, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Die Basis `Controller` -Klasse bereits implementiert die `IDisposable` Schnittstelle, damit dieser Code einfach eine Überschreibung fügt die `Dispose(bool)` Methode, die Kontextinstanz explizit zu verwerfen.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Verarbeiten von Transaktionen

Standardgemäß implementiert Entity Framework implizit Transaktionen. In Szenarien, in dem Sie Änderungen auf mehrere Zeilen oder Tabellen, und rufen dann `SaveChanges`, Entity Framework wird automatisch sichergestellt, dass alle Änderungen erfolgreich abgeschlossen oder alle fehlschlagen. Wenn ein Fehler auftritt, nachdem einige der Änderungen durchgeführt wurden, werden diese Änderungen automatisch zurückgesetzt. Für Szenarien, in denen mehr, – beispielsweise gesteuert müssen wenn Vorgängen, die außerhalb von Entity Framework in einer Transaktion--enthalten sein sollen, finden Sie unter [arbeiten mit Transaktionen](https://msdn.microsoft.com/data/dn456843) auf MSDN.

## <a name="summary"></a>Zusammenfassung

Sie verfügen nun über einen vollständigen Satz von Seiten, die für einfache CRUD-Operationen ausführen `Student` Entitäten. MVC-Hilfsprogrammen wird verwendet, um Benutzeroberflächenelemente für die Datenfelder zu generieren. Weitere Informationen zu MVC-Hilfsprogrammen, finden Sie unter [Rendern einer Form mit HTML-Hilfsmethoden](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (die Seite ist für MVC 3, jedoch immer noch relevant für MVC 5 wird).

In den nächsten Lernprogrammen müssen Sie die Funktionalität der Indexseite erweitern, indem Sie hinzufügen, Sortieren und paging.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie im [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
