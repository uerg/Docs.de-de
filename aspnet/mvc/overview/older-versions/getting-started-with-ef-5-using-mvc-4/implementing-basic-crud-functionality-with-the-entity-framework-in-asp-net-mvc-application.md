---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementieren grundlegende CRUD-Funktionalität mit Entity Framework in ASP.NET MVC-Anwendung (2 von 10) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: acec5c9641b1de230956478c4396d1d541fcb0eb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875322"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementieren grundlegende CRUD-Funktionalität mit Entity Framework in ASP.NET MVC-Anwendung (2 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm erstellt Sie eine MVC-Anwendung, die gespeichert und Daten mit dem Entity Framework und SQL Server LocalDB angezeigt. In diesem Lernprogramm müssen Sie überprüfen und anpassen, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Code, der die MVC-Gerüstbau automatisch für Sie in den Controller und Ansichten erstellt.

> [!NOTE]
> Es ist üblich, dass das Repositorymuster implementiert wird, um eine Abstraktionsebene zwischen Ihrem Controller und der Datenzugriffsebene zu erstellen. Um diese Lernprogramme einfach zu halten, wird nicht implementieren Sie ein Repository bis zu einem späteren Lernprogramm dieser Reihe.


In diesem Lernprogramm erstellen Sie den folgenden Webseiten:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Erstellen eine Seite "Details"

Der scaffolded Code für den Studenten `Index` Seite außer acht gelassen der `Enrollments` -Eigenschaft, da diese Eigenschaft eine Auflistung enthält. In der `Details` Seite zeigen Sie den Inhalt der Auflistung in einer HTML-Tabelle.

 In *Controllers\StudentController.cs*, der Aktionsmethode für den `Details` anzeigen verwendet die `Find` Methode zum Abrufen einer einzelnes `Student` Entität. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Der Schlüsselwert als an die Methode übergeben wird die `id` Parameter und ergibt sich aus der Weiterleitung von Daten in der **Details** Link auf der Seite "Index". 

1. Open *Views\Student\Details.cshtml*. Jedes Feld wird angezeigt, mit einem `DisplayFor` Helper, wie im folgenden Beispiel gezeigt: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Nach der `EnrollmentDate` Felds und unmittelbar vor der schließenden `fieldset` kennzeichnen, fügen Sie Code aus, um eine Liste der Bereitstellungen, anzuzeigen, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Dieser Code durchläuft die Entitäten in der Navigationseigenschaft `Enrollments`. Für jede `Enrollment` Entität in der Eigenschaft den Kurstitel und die Dienstqualität dargestellt angezeigt. Der Kurstitel wird abgerufen, von der `Course` Entität, die in gespeichert ist die `Course` Navigationseigenschaft der `Enrollments` Entität. Alle diese Daten werden aus der Datenbank abgerufen automatisch bei Bedarf. (Das heißt, verwenden Sie lazy loading, hier. Sie haben keinen *unverzüglichem Laden* für die `Courses` Navigationseigenschaft, damit beim ersten Sie zuzugreifen versuchen, diese Eigenschaft, eine Abfrage an die Datenbank gesendet wird, um die Daten abzurufen. Erfahren Sie mehr über verzögertes Laden und unverzüglichem Laden in das [Lesen verknüpfter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.)
3. Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf eine **Details** Link, um Alexander Carson. Die Liste der Kurse und Klassen für den ausgewählten Studenten wird angezeigt:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualisieren die Seite "erstellen"

1. In *Controllers\StudentController.cs*, ersetzen die `HttpPost``Create` Aktionsmethode mit den folgenden Code zum Hinzufügen einer `try-catch` Block und der [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) scaffolded-Methode: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Dieser Code fügt der `Student` Entität erstellt wurde, indem Sie die ASP.NET MVC-modellbindung an die `Students` Entität festgelegt, und klicken Sie dann die Änderungen in der Datenbank gespeichert. (*Modellieren Binder* bezieht sich auf die ASP.NET MVC-Funktionalität, die macht es einfacher, für die Sie zum Arbeiten mit Daten, die von einem Formular übermittelt; einen Modellbinder konvertiert gebucht Form-Werte in CLR-Typen und übergibt sie an die Aktionsmethode im Parameters. Der Modellbinder Druckmethode instanziiert einen `Student` Entität für die Eigenschaft verwenden Sie die Werte aus der `Form` Auflistung.)

    Die `ValidateAntiForgeryToken` Attribut verhindert [websiteübergreifende anforderungsfälschung](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe.

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
2. Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf **neu erstellen**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Einige datenvalidierung funktioniert standardmäßig. Geben Sie Namen und ein ungültiges Datum, und klicken Sie auf **erstellen** um die Fehlermeldung anzuzeigen.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Die folgende hervorgehobene Code zeigt die Überprüfung des Modells.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Ändern Sie das Datum in einen gültigen Wert ein, z. B. 9/1/2005, und klicken Sie auf **erstellen** , finden in der neuen Studenten in angezeigt werden die **Index** Seite.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualisieren die POST-Bearbeitungsseite

In *Controllers\StudentController.cs*, die `HttpGet` `Edit` Methode (der die `HttpPost` Attribut) verwendet die `Find` Methode zum Abrufen der ausgewählten `Student` Entität, als Sie gesehen haben in der `Details` Methode. Sie müssen diese Methode nicht ändern.

Allerdings ersetzen die `HttpPost` `Edit` Aktionsmethode mit den folgenden Code zum Hinzufügen einer `try-catch` Block und der [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Dieser Code gleicht in gesehen haben die `HttpPost` `Create` Methode. Anstatt zum Hinzufügen von der Entität, die von den Modellbinder an die Entitätssammlung erstellt, legt dieser Code ein Flag für die Entität, der angibt, dass er geändert wurde. Wenn die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufgerufen wird, die ["geändert"](https://msdn.microsoft.com/library/system.data.entitystate.aspx) Kennzeichnung bewirkt, dass das Entity Framework zum Erstellen von SQL-Anweisungen, um die Datenbankzeile zu aktualisieren. Alle Spalten der Datenbankzeile werden aktualisiert, einschließlich derer, die der Benutzer nicht geändert haben, und Parallelitätskonflikte werden ignoriert. (Gewusst wie: Behandeln von Parallelität in einem späteren Lernprogramm dieser Reihe erfahren.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Status der Entität und das Anfügen "und" SaveChanges-Methoden

Der Datenbankkontext verfolgt, ob die Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind. Diese Information bestimmt, was passiert, wenn Sie die Methode `SaveChanges` aufrufen. Wenn Sie z. B. eine neue Entität zum Übergeben der [hinzufügen](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) Methode, die Zustand der Entität, um festgelegt ist `Added`. Klicken Sie dann beim Aufrufen der [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode, der Datenbankkontext stellt eine SQL `INSERT` Befehl.

Eine Entität in einem der möglicherweise die[folgende Zustände](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added` Die Entität ist noch nicht in der Datenbank vorhanden. Die `SaveChanges` Methode ausstellen muss ein `INSERT` Anweisung.
- `Unchanged` Die Methode `SaveChanges` muss nichts mit dieser Entität tun. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.
- `Modified` Einige oder alle Eigenschaftswerte der Entität wurden geändert. Die `SaveChanges` Methode ausstellen muss ein `UPDATE` Anweisung.
- `Deleted` Die Entität wurde zum Löschen markiert. Die `SaveChanges` Methode ausstellen muss eine `DELETE` Anweisung.
- `Detached` Die Entität wird nicht vom Datenbankkontext nachverfolgt.

Statusänderungen werden in einer Desktop-App in der Regel automatisch festgelegt. In einem desktop-Typ der Anwendung eine Entität zu lesen und nehmen Sie Änderungen an einige Eigenschaftswerte. Dadurch wird der Entitätsstatus automatisch auf `Modified` festgelegt. Klicken Sie dann beim Aufrufen `SaveChanges`, Entity Framework generiert eine SQL `UPDATE` -Anweisung, die nur die tatsächlichen Eigenschaften aktualisiert, die Sie geändert haben.

Die getrennte Art der Web-apps ist nicht für die fortlaufende Sequenz ermöglichen. Die [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , liest eine Entität wurde verworfen, nachdem eine Seite gerendert wird. Wenn die `HttpPost` `Edit` Aktionsmethode aufgerufen wird, erfolgt eine neue Anforderung aus, und Sie haben eine neue Instanz der der [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), daher Sie manuell festlegen der Entitätszustand müssen `Modified.` dann beim Aufrufen `SaveChanges`, Entity Framework werden alle Spalten der Datenbankzeile aktualisiert, da der Kontext kann nicht wissen, welche Eigenschaften Sie geändert haben.

Ggf. die SQL `Update` Anweisung, um nur die Felder aktualisieren, die der Benutzer tatsächlich geändert wird, können Sie die ursprünglichen Werte in irgendeiner Form (z. B. ausgeblendete Felder) speichern, sodass verfügbar wenn sind die `HttpPost` `Edit` -Methode aufgerufen wird. Anschließend können Sie erstellen eine `Student` Entität mit den ursprünglichen Werten Aufruf der `Attach` Methode mit diesem Originalversion der Entität, Werten der Entität mit den neuen Werten aktualisiert, und rufen dann `SaveChanges.` Weitere Informationen finden Sie unter [ Status der Entität und SaveChanges](https://msdn.microsoft.com/data/jj592676) und [lokale Daten](https://msdn.microsoft.com/data/jj592872) im MSDN Data Developer Center.

Der Code in *Views\Student\Edit.cshtml* ähnelt der im haben gesehen *Create.cshtml*, und es sind keine Änderungen erforderlich.

Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte, und klicken Sie dann auf eine **bearbeiten** Link.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Ändern Sie einige der Daten, und klicken Sie auf **Speichern**. Sie sehen die geänderten Daten in die Indexseite.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren die Seite "löschen"

In *Controllers\StudentController.cs*, der Vorlagencode für die `HttpGet` `Delete` -Methode verwendet die `Find` Methode zum Abrufen der ausgewählten `Student` Entität, als Sie gesehen haben, der `Details` und `Edit` Methoden. Allerdings müssen Sie dieser Methode und der dazugehörigen Ansicht einige Funktionen hinzufügen, um eine benutzerdefinierte Fehlermeldung zu implementieren, wenn der Aufruf von `SaveChanges` fehlschlägt.

Wie Sie bereits bei den Vorgängen zum Aktualisieren und Erstellen gesehen haben, benötigen Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Antwort auf eine GET-Anforderung aufgerufen wird, zeigt eine Sicht, die dem Benutzer hat die Möglichkeit, zu genehmigen, oder brechen Sie den Löschvorgang. Wenn der Benutzer diesen Löschvorgang genehmigt, wird eine POST-Anforderung erstellt. In diesem Fall die `HttpPost` `Delete` Methode wird aufgerufen, und diese Methode führt dann tatsächlich den Löschvorgang.

Fügen Sie eine `try-catch` -block, um die `HttpPost` `Delete` Methode zum Behandeln von Fehlern, die auftreten können, wenn die Datenbank aktualisiert wird. Wenn ein Fehler auftritt, die `HttpPost` `Delete` Methodenaufrufe der `HttpGet` `Delete` -Methode, und übergeben sie einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die `HttpGet Delete` Methode Kriterienbereich klicken Sie dann die Seite "Bestätigung" zusammen mit der Fehlermeldung, dass der Benutzer eine Möglichkeit zum Abbrechen oder Wiederholen Sie den Vorgang.

1. Ersetzen Sie die `HttpGet` `Delete` -Aktionsmethode durch folgenden Code wird die Fehlerberichterstattung verwaltet: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Dieser Code akzeptiert ein [optional](https://msdn.microsoft.com/library/dd264739.aspx) booleschen Parameter, der angibt, ob sie nach einem Fehler beim Speichern von Änderungen aufgerufen wurde. Dieser Parameter ist `false` bei der `HttpGet` `Delete` Methode wird aufgerufen, ohne Sie zu einem vorherigen Fehler. Bei Aufruf durch die `HttpPost` `Delete` Methode als Reaktion auf ein Update Fehler, der Parameter ist `true` und eine Fehlermeldung an die Ansicht übergeben wird.
2. Ersetzen Sie die `HttpPost` `Delete` Aktionsmethode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code dem führt des eigentlichen Löschvorgangs und fängt alle Datenbank-Update-Fehler ab.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Dieser Code Ruft die ausgewählte Entität ruft dann die [entfernen](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) Methode, um die Entität Status festgelegt wird, um `Deleted`. Wenn `SaveChanges` aufgerufen wird, wird eine SQL `DELETE` Befehl generiert wird. Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der scaffolded Code mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` so erteilen Sie die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden zum anderen Methodenparameter angegeben haben.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention einhalten und den gleichen Namen für die `HttpPost` und `HttpGet` Löschmethoden.

     Ist das Verbessern der Leistung einer Anwendung hoher Priorität, vermeiden Sie unnötige SQL-Abfrage zum Abrufen der zeilenupdates durch Ersetzen der Codezeilen, die aufgerufen werden der `Find` und `Remove` Methoden mit den folgenden Code, wie in gelb dargestellt. Markieren Sie:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Dieser Code instanziiert einen `Student` Entität mithilfe den primären Schlüsselwert und dann wird der Entitätszustand `Deleted`. Das ist alles, was Entity Framework benötigt, um die Entität löschen zu können.

     Wie bereits erwähnt, die `HttpGet` `Delete` Methode löscht die Daten. Ausführen einer Delete-Vorgangs als Antwort auf einen GET-Befehl anfordern (oder zu Ausführen von Vorgängen bearbeiten zu erstellen, Vorgang oder einem sonstigen Vorgang, der Daten ändert) stellt ein Sicherheitsrisiko dar. Weitere Informationen finden Sie unter [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) Stephen Walther Blog.
3. In *Views\Student\Delete.cshtml*, fügen Sie eine Fehlermeldung, die zwischen den `h2` Überschrift und der `h3` Überschrift, wie im folgenden Beispiel gezeigt:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf eine **löschen** Link:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Klicken Sie auf **Löschen**. Die Indexseite wird ohne den gelöschten Student angezeigt. (Sehen Sie ein Beispiel für den Fehlerbehandlungscode in Aktion in der [Behandeln von Parallelität](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Sicherstellen, dass Verbindungen mit der Datenbank nicht öffnen bleiben

Um sicherzustellen, dass Verbindungen mit der Datenbank ordnungsgemäß geschlossen sind und die Ressourcen, die sie von freigegebenen aufnehmen, sehen Sie darauf, dass die Kontextinstanz freigegeben ist. Also warum der scaffolded Code bietet ein [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) Methode am Ende der `StudentController` -Klasse im *StudentController.cs*, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Die Basis `Controller` -Klasse bereits implementiert die `IDisposable` Schnittstelle, damit dieser Code einfach eine Überschreibung fügt die `Dispose(bool)` Methode, die Kontextinstanz explizit zu verwerfen.

## <a name="summary"></a>Zusammenfassung

Sie verfügen nun über einen vollständigen Satz von Seiten, die für einfache CRUD-Operationen ausführen `Student` Entitäten. MVC-Hilfsprogrammen wird verwendet, um Benutzeroberflächenelemente für die Datenfelder zu generieren. Weitere Informationen zu MVC-Hilfsprogrammen, finden Sie unter [Rendern einer Form mit HTML-Hilfsmethoden](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (die Seite ist für MVC 3, jedoch weiterhin für MVC 4 relevant wird).

In den nächsten Lernprogrammen müssen Sie die Funktionalität der Indexseite erweitern, indem Sie hinzufügen, Sortieren und paging.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
