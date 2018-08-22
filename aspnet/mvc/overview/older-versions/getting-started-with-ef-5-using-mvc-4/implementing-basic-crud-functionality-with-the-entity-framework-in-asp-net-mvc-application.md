---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementieren von grundlegenden CRUD-Funktionen, mit dem Entitätsframework in ASP.NET MVC-Anwendung (2 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 999a598b6f9c9a16c596cb6c8d7bb46439876f01
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826325"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementieren von grundlegenden CRUD-Funktionen, mit dem Entitätsframework in ASP.NET MVC-Anwendung (2 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Tutorial erstellt Sie eine MVC-Anwendung, die Daten, die mit dem Entity Framework und SQL Server LocalDB speichert und anzeigt. In diesem Tutorial überprüfen und anpassen, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Code, der der MVC-Gerüstbau automatisch für Sie Controller und Ansichten erstellt.

> [!NOTE]
> Es ist üblich, dass das Repositorymuster implementiert wird, um eine Abstraktionsebene zwischen Ihrem Controller und der Datenzugriffsebene zu erstellen. Zur Vereinfachung in diesen Tutorials wird nicht implementieren Sie ein Repository bis zu einem späteren Tutorial dieser Reihe.


In diesem Tutorial erstellen Sie den folgenden Webseiten:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Erstellen eine Seite "Details"

Der eingerüstete Code für die Schüler/Studenten `Index` Seite außer acht gelassen der `Enrollments` -Eigenschaft, da diese Eigenschaft eine Auflistung enthält. In der `Details` Seite zeigen Sie den Inhalt der Auflistung in eine HTML-Tabelle.

 In *Controllers\StudentController.cs*, der Aktionsmethode für den `Details` anzeigen verwendet die `Find` Methode zum Abrufen einer einzelnen `Student` Entität. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Der Schlüsselwert als an die Methode übergeben wird die `id` Parameter und stammt aus dem Weiterleiten von Daten in die **Details** Link auf der Seite "Index". 

1. Open *Views\Student\Details.cshtml*. Jedes Feld wird angezeigt, die mit einem `DisplayFor` Helper, wie im folgenden Beispiel gezeigt: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Nach der `EnrollmentDate` Feld- und direkt vor dem Endtag `fieldset` markieren, fügen Sie Code zum Anzeigen einer Liste der Registrierungen, hinzu, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Dieser Code durchläuft die Entitäten in der Navigationseigenschaft `Enrollments`. Für jede `Enrollment` Entität in der Eigenschaft der Kurstitel und die Klasse angezeigt. Der Kurstitel wird abgerufen, von der `Course` Entität, die in gespeichert ist die `Course` Navigationseigenschaft der `Enrollments` Entität. Alle diese Daten wird aus der Datenbank automatisch abgerufen, wenn er benötigt wird. (Das heißt, verwenden Sie verzögerten Laden hier. Sie haben keinen *Eager Load* für die `Courses` Navigationseigenschaft, damit beim ersten Versuch, auf diese Eigenschaft zuzugreifen, eine Abfrage an die Datenbank gesendet wird, um die Daten abzurufen. Erfahren Sie mehr zum lazy Loading und Eager Load in den [Lesen von verwandten Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Reihe.)
3. Führen Sie die Seite durch Auswählen der **Schüler/Studenten** Registerkarte, und klicken auf eine **Details** Link, um Alexander Carson. Die Liste der Kurse und Klassen für den ausgewählten Studenten wird angezeigt:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualisieren die Seite "erstellen"

1. In *Controllers\StudentController.cs*, ersetzen die `HttpPost``Create` Aktionsmethode mit den folgenden Code zum Hinzufügen einer `try-catch` Block und die [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) an die erstellte Methode: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Dieser Code fügt die `Student` Entität, die von der ASP.NET MVC-modellbindung, erstellt die `Students` Entität festgelegt, und klicken Sie dann die Änderungen in der Datenbank gespeichert. (*Model Binders* bezieht sich auf die Funktionalität von ASP.NET MVC, das macht es einfacher für Sie arbeiten mit Daten, die über ein Formular übermittelt, eine modellbindung konvertiert gebucht-Formular-Werte in der CLR-Typen und übergibt sie an die Aktionsmethode Parameter. Der Modellbinder Druckmethode instanziiert ein `Student` Entität automatisch anhand der Eigenschaft Werte aus der `Form` Sammlung.)

    Die `ValidateAntiForgeryToken` Attribut hilft dabei, zu verhindern, dass [websiteübergreifende anforderungsfälschung](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Führen Sie die Seite durch Auswählen der **Schüler/Studenten** Registerkarte und auf **neu erstellen**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Validierung einiger Daten funktioniert in der Standardeinstellung. Geben Sie Namen und ein ungültiges Datum, und klicken Sie auf **erstellen** auf die folgende Fehlermeldung angezeigt.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Der folgende hervorgehobene Code zeigt die modellvalidierung.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Ändern Sie das Datum in einen gültigen Wert ein, z. B. 9/1/2005, und klicken Sie auf **erstellen** auf den neuen Studenten, die in angezeigt werden, finden Sie unter den **Index** Seite.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualisieren die Seite "POST bearbeiten"

In *Controllers\StudentController.cs*, wird die `HttpGet` `Edit` Methode (ohne die `HttpPost` Attribut) verwendet die `Find` Methode, um den ausgewählten abzurufen `Student` Entität, wie Sie gesehen haben. in der `Details` Methode. Sie müssen diese Methode nicht ändern.

Ersetzt jedoch die `HttpPost` `Edit` Aktionsmethode mit den folgenden Code zum Hinzufügen einer `try-catch` Block und die [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Dieser Code ist ähnlich wie Sie im gesehen haben die `HttpPost` `Create` Methode. Anstatt die Entität, die von der modellbindung auf die Entitätenmenge erstellt, legt dieser Code jedoch ein Flag für die Entität, der angibt, dass er geändert wurde. Wenn die ["SaveChanges"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufgerufen wird, die ["geändert"](https://msdn.microsoft.com/library/system.data.entitystate.aspx) Flag bewirkt, dass das Entity Framework zum Erstellen von SQL-Anweisungen, um die Datenbankzeile zu aktualisieren. Alle Spalten der Datenbankzeile aktualisiert werden, einschließlich derjenigen, die der Benutzer nicht geändert haben, und nebenläufigkeitskonflikte werden ignoriert. (Sie erfahren, wie Parallelität in einem späteren Tutorial dieser Reihe behandelt können.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Status von Entitäten und das Anfügen und die "SaveChanges"-Methoden

Der Datenbankkontext verfolgt, ob die Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind. Diese Information bestimmt, was passiert, wenn Sie die Methode `SaveChanges` aufrufen. Wenn Sie z. B. eine neue Entität zum Übergeben der [hinzufügen](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) Methode, die Zustand der Entität `Added`. Klicken Sie dann beim Aufrufen der ["SaveChanges"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode, erteilt der Datenbankkontext einen SQL `INSERT` Befehl.

Eine Entität möglicherweise eines der[folgende Zustände](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added` Die Entität in der Datenbank noch nicht vorhanden. Die `SaveChanges` Methode ausgeben muss ein `INSERT` Anweisung.
- `Unchanged` Die Methode `SaveChanges` muss nichts mit dieser Entität tun. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.
- `Modified` Einige oder alle Eigenschaftswerte der Entität wurden geändert. Die `SaveChanges` Methode ausgeben muss ein `UPDATE` Anweisung.
- `Deleted` Die Entität wurde zum Löschen markiert. Die `SaveChanges` Methode ausgeben muss eine `DELETE` Anweisung.
- `Detached` Die Entität wird nicht vom Datenbankkontext nachverfolgt.

Statusänderungen werden in einer Desktop-App in der Regel automatisch festgelegt. In einem desktop-Typ der Anwendung Sie lesen eine Entität und nehmen Änderungen an der zugehörigen Eigenschaftswerte. Dadurch wird der Entitätsstatus automatisch auf `Modified` festgelegt. Klicken Sie dann beim Aufrufen `SaveChanges`, Entity Framework generiert eine SQL `UPDATE` -Anweisung, die nur die aktuellen Eigenschaften aktualisiert, die Sie geändert haben.

Die nicht verbundene Art der Web-apps ermöglichen nicht für die fortlaufende Sequenz. Die ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , liest eine Entität wird freigegeben, nachdem eine Seite gerendert wird. Wenn die `HttpPost` `Edit` Aktionsmethode aufgerufen wird, eine neue Anforderung aus, und Sie haben eine neue Instanz der der ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), daher Sie manuell den Entitätsstatus auf festgelegt müssen `Modified.` wird beim Aufruf `SaveChanges`, das Entity Framework aktualisiert alle Spalten der Datenbankzeile, da der Kontext nicht wissen, welche Eigenschaften Sie geändert haben.

Wenn Sie möchten, dass die SQL `Update` Anweisung, um nur die Felder zu aktualisieren, die der Benutzer tatsächlich geändert, Sie können die ursprünglichen Werte in irgendeiner Weise (z. B. ausgeblendete Felder) speichern, damit sie verfügbar bei sind die `HttpPost` `Edit` Methode wird aufgerufen. Anschließend können Sie erstellen eine `Student` Entität mit den ursprünglichen Werten, Aufruf der `Attach` Methode mit der Originalversion der Entität, die Werte dieser Entität zu den neuen Werten aktualisiert, und rufen dann `SaveChanges.` Weitere Informationen finden Sie unter [ Status von Entitäten und "SaveChanges"](https://msdn.microsoft.com/data/jj592676) und [lokale Daten](https://msdn.microsoft.com/data/jj592872) in der MSDN-Datenentwicklercenter.

Der Code in *Views\Student\Edit.cshtml* ist ähnlich, wie Sie im gesehen haben *Create.cshtml*, und es sind keine Änderungen erforderlich.

Führen Sie die Seite durch Auswählen der **Schüler/Studenten** Registerkarte, und klicken Sie dann auf eine **bearbeiten** Hyperlink.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Ändern Sie einige der Daten, und klicken Sie auf **Speichern**. Die geänderten Daten in der Indexseite angezeigt.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren die Seite "löschen"

In *Controllers\StudentController.cs*, der Vorlagencode für die `HttpGet` `Delete` -Methode verwendet die `Find` Methode, um den ausgewählten abzurufen `Student` Entität, wie Sie gesehen haben, der `Details` und `Edit` Methoden. Allerdings müssen Sie dieser Methode und der dazugehörigen Ansicht einige Funktionen hinzufügen, um eine benutzerdefinierte Fehlermeldung zu implementieren, wenn der Aufruf von `SaveChanges` fehlschlägt.

Wie Sie bereits bei den Vorgängen zum Aktualisieren und Erstellen gesehen haben, benötigen Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Reaktion auf eine GET-Anforderung aufgerufen wird, zeigt eine Ansicht, die dem Benutzer hat die Möglichkeit, genehmigen oder abzubrechen. Wenn der Benutzer diesen Löschvorgang genehmigt, wird eine POST-Anforderung erstellt. In diesem Fall die `HttpPost` `Delete` Methode wird aufgerufen, und klicken Sie dann die Methode tatsächlich den Löschvorgang ausführt.

Fügen Sie eine `try-catch` -block, um die `HttpPost` `Delete` Methode zum Behandeln von Fehlern, die auftreten können, wenn die Datenbank aktualisiert wird. Wenn ein Fehler auftritt, die `HttpPost` `Delete` Methodenaufrufe der `HttpGet` `Delete` -Methode, und übergeben sie einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die `HttpGet Delete` -Methode zeigt dann die Seite "Bestätigung" zusammen mit der Fehlermeldung, so erhält der Benutzer eine Möglichkeit zum Abbrechen oder Wiederholen Sie den Vorgang.

1. Ersetzen Sie die `HttpGet` `Delete` Aktionsmethode mit den folgenden Code, der Fehlerberichterstattung verwaltet: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Dieser Code akzeptiert eine [optional](https://msdn.microsoft.com/library/dd264739.aspx) boolescher Parameter, der angibt, ob sie nach einem Fehler beim Speichern der Änderungen aufgerufen wurde. Dieser Parameter ist `false` bei der `HttpGet` `Delete` Methode wird aufgerufen, ohne Sie zu einem vorherigen Fehler. Wenn es aufgerufen wird, indem die `HttpPost` `Delete` Methode als Reaktion auf einen Datenbankaktualisierungsfehler, der Parameter ist `true` und eine Fehlermeldung an die Ansicht übergeben wird.
2. Ersetzen Sie die `HttpPost` `Delete` Action-Methode (mit dem Namen `DeleteConfirmed`) mit den folgenden Code, der den tatsächlichen Löschvorgang ausführt und jegliche Datenbankaktualisierungsfehler abfängt.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Dieser Code Ruft die ausgewählte Entität ist, ruft dann die [entfernen](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) Methode, um den Status der Entität festzulegen, um `Deleted`. Wenn `SaveChanges` aufgerufen wird, wird eine SQL `DELETE` -Befehl generiert wird. Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der eingerüstete Code, der mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` gerne die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden, um verschiedene Methodenparameter zu haben.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention halten und den gleichen Namen für die `HttpPost` und `HttpGet` delete-Methoden.

     Wenn Verbessern der Leistung einer Anwendung mit hohem Volumen eine Priorität ist, können Sie vermeiden, dass eine überflüssige SQL-Abfrage zum Abrufen der zeilenupdates durch, und Ersetzen Sie dabei die Codezeilen, die aufgerufen werden die `Find` und `Remove` Methoden mit den folgenden Code, wie in gelb dargestellt. Markieren Sie:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Dieser Code instanziiert ein `Student` Entität, die nur den primären Schlüsselwert verwenden und anschließend den Entitätsstatus auf `Deleted`. Das ist alles, was Entity Framework benötigt, um die Entität löschen zu können.

     Wie bereits erwähnt, die `HttpGet` `Delete` Methode nicht die Daten zu löschen. Ausführen eines Löschvorgangs als Reaktion auf eine GET request (oder erstellen Sie zu jeder Aktion bearbeiten, Vorgang, oder einem sonstigen Vorgang, der Daten ändert) stellt ein Sicherheitsrisiko dar. Weitere Informationen finden Sie unter [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther Blog.
3. In *Views\Student\Delete.cshtml*, fügen Sie eine Fehlermeldung zwischen der `h2` Überschrift und `h3` Überschrift, wie im folgenden Beispiel gezeigt:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Führen Sie die Seite durch Auswählen der **Schüler/Studenten** Registerkarte, und klicken auf eine **löschen** Link:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Klicken Sie auf **Löschen**. Die Indexseite wird ohne den gelöschten Student angezeigt. (Sehen Sie ein Beispiel für den Fehlerbehandlungscode in Aktion in der [Behandeln von Parallelität](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorials weiter unten in dieser Reihe.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Sicherstellen, dass Verbindungen mit der Datenbank nicht geöffnet bleiben

Um sicherzustellen, dass Verbindungen mit der Datenbank ordnungsgemäß geschlossen werden, und die Ressourcen, die sie sich freigegebenen gespeichert werden, sollte darauf, dass die Context-Instanz freigegeben wird. Warum der eingerüstete Code bietet ein [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) Methode am Ende der `StudentController` -Klasse im *StudentController.cs*, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Die Basis `Controller` -Klasse bereits implementiert die `IDisposable` Schnittstelle, damit dieser Code einfach eine Außerkraftsetzung für fügt die `Dispose(bool)` Methode, um explizit die Context-Instanz zu löschen.

## <a name="summary"></a>Zusammenfassung

Sie verfügen nun über einen vollständigen Satz von Seiten, die für einfache CRUD-Vorgänge ausführen `Student` Entitäten. MVC-Hilfsprogrammen verwendet, um die Elemente der Benutzeroberfläche für die Datenfelder zu generieren. Weitere Informationen zu MVC-Hilfsprogrammen, finden Sie unter [Rendern einer Form mithilfe von HTML-Hilfsprogramme](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (Seite ist für MVC 3 jedoch weiterhin für MVC 4 relevant ist).

Im nächsten Tutorial erweitern Sie die Funktionalität der Indexseite durch Hinzufügen von sortieren und paging.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
