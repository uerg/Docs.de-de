---
uid: mvc/overview/getting-started/introduction/adding-validation
title: "Hinzufügen einer Validierung | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: b59965b2fab00cb64db06574d5ca3c6388daa7c8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation"></a>Hinzufügen der Validierung
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

In diesem in diesem Abschnitt fügen Sie Validierungslogik auf die `Movie` Modell, und Sie stellen sicher, dass jederzeit die Validierungsregeln, wenn ein Benutzer versucht erzwungen werden, erstellen oder Bearbeiten eines Films mithilfe der Anwendung.

## <a name="keeping-things-dry"></a>Halten Dinge TROCKENEN

Einer der Kernaspekte Entwurf von ASP.NET MVC ist [TROCKENEN](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;wiederhole Dich nicht&quot;). ASP.NET MVC vertraut zu machen, können Funktionen oder Verhalten nur einmal angeben, und dann überall in einer Anwendung berücksichtigt werden. Dadurch reduziert die Menge an Code, den Sie schreiben müssen und den Code, den Sie weniger Fehler fehleranfällig und leichter schreiben zu verwalten.

Zur Unterstützung der Überprüfung von ASP.NET MVC und Entity Framework Code First ist ein hervorragendes Beispiel für das TROCKENEN Prinzip in Aktion. Sie können Validierungsregeln deklarativ angeben, an einem Ort (in der Modellklasse), und die Regeln werden überall in der Anwendung erzwungen.

Sehen wir uns, wie Sie diese Überprüfung-Unterstützung in der Film-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln, die Film-Modell

Sie beginnen, indem Sie die Überprüfung Programmlogik zum Hinzufügen der `Movie` Klasse.

Öffnen Sie Datei *Movie.cs*. Beachten Sie, dass die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace enthält keine `System.Web`. DataAnnotations bietet einen integrierten Satz von Validierungsattributen, die deklarativ auf eine beliebige Klasse oder eine Eigenschaft angewendet werden können. (Es enthält auch die Formatierungsattribute wie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , die die Hilfe für Formatierung und Validierung stellen keine.)

Jetzt aktualisieren der `Movie` Klasse, um die integrierte nutzen [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [der reguläre Ausdruck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), und [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierungsattribute. Ersetzen Sie die `Movie` Klasse durch Folgendes:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

Die [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut legt die maximale Länge der Zeichenfolge ist, wird diese Einschränkung für die Datenbank und aus diesem Grund wird das Datenbankschema geändert. Klicken Sie mit der rechten Maustaste auf die **Filme** in Tabelle **Server-Explorer** , und klicken Sie auf **Tabellendefinition öffnen**:

![](adding-validation/_static/image1.png)

In der obigen Abbildung sehen Sie, alle der Zeichenfolgenfelder sind auf [NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx). Wir werden Migrationen verwenden, um das Schema zu aktualisieren. Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster, und geben Sie die folgenden Befehle:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse mit dem angegebenen Namen (`DataAnnotations`), und klicken Sie in der `Up` Methode sehen Sie den Code, der die schemaeinschränkungen aktualisiert:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Die `Genre` Feld sind nicht mehr NULL-Werte zulässt (Sie müssen einen Wert eingeben). Die `Rating` Feld hat eine maximale Länge von 5 und `Title` besitzt eine maximale Länge von 60. Die minimale Länge von 3 auf `Title` und dem Bereich auf `Price` schemaänderungen erstellt wurde.

Überprüfen Sie die Film-Schema:

![](adding-validation/_static/image2.png)

Die Zeichenfolgenfelder anzeigen, die neuen Grenzwerte für die Länge und `Genre` nicht mehr als auf NULL festlegbar aktiviert ist.

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Die Attribute `Required` und `MinimumLength` geben an, dass eine Eigenschaft einen Wert haben muss. Ein Benutzer kann allerdings ein Leerzeichen eingeben, um diese Validierung zu erfüllen. Die [der reguläre Ausdruck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) Attribut wird verwendet, um einzuschränken, welche Zeichen sein können Eingabe. Im oben angegebenen Code sind für `Genre` und `Rating` nur Buchstaben (keine Leerzeichen, Zahlen und Sonderzeichen) erlaubt. Die [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Attribut schränkt einen Wert innerhalb eines bestimmten Bereichs. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen. Werttypen (z. B. `decimal, int, float, DateTime`) sind grundsätzlich erforderlich und müssen nicht die `Required` Attribut.

Code wird zuerst sichergestellt, dass der Validierungsregeln, die Sie, auf eine Modellklasse angeben erzwungen werden, bevor die Anwendung Änderungen in der Datenbank gespeichert. Z. B. der folgenden Code löst eine [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) Ausnahme bei der `SaveChanges` -Methode aufgerufen wird, da einige erforderliche `Movie` Eigenschaftswerte fehlen:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Der obige Code wird die folgende Ausnahme ausgelöst:

*Fehler bei der Validierung für eine oder mehrere Entitäten. Finden Sie unter 'EntityValidationErrors'-Eigenschaft für weitere Details.*

Mit Validierungsregeln, die von .NET Framework automatisch erzwungen. dadurch, dass Ihre Anwendung unter Umständen stabiler. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Validierungsfehler Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL.

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzufügen. Füllen Sie das Formular mit einigen ungültigen Werten aus. Wenn die clientseitige jQuery-Validierung den Fehler erkennt, wird eine Fehlermeldung angezeigt.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> auf die jQuery-Validierung für nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (",") für ein Dezimaltrennzeichen, müssen Sie die NuGet einschließen Globalisieren wie zuvor in diesem Lernprogramm beschrieben.


Beachten Sie, wie das Formular automatisch eine roten Rahmenfarbe zum verwendet die Textfelder hervorzuheben, die ungültige Daten enthalten, und verfügt über eine entsprechende Überprüfungsfehlermeldung neben jeweils ausgegeben. Die Fehlermeldungen werden sowohl auf Clientseite (mithilfe von JavaScript und jQuery) als auch auf Serverseite erzwungen (wenn ein Benutzer JavaScript deaktiviert hat).

Ein echter Vorteil ist, dass Sie nicht in eine einzelne Codezeile ändern müssen die `MoviesController` Klasse oder in der *Create.cshtml* anzeigen, um diese Überprüfung Benutzeroberfläche zu aktivieren. Die Controller und Ansichten, die Sie zuvor in diesem Tutorial erstellt haben, haben die angegebenen Validierungsregeln automatisch übernommen (mithilfe der Validierungsattribute für die Eigenschaften der Modellklasse `Movie`). Testen Sie die Validierung mithilfe der Aktionsmethode `Edit`, und es folgt die gleiche Validierung.

Die Formulardaten werden erst an den Server gesendet, wenn auf Clientseite keine Validierungsfehler mehr auftreten. Sie können dies überprüfen, verlegen Sie einen Haltepunkt in der HTTP-Post-Methode, mit der [Fiddler-Tool](http://fiddler2.com/fiddler2/), oder der Internet Explorer [F12 Entwicklertools](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Wie in der Validierung erstellen, anzeigen und Aktionsmethode erstellen

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. Die nächste Auflistung zeigt, was die `Create` Methoden in der `MovieController` sieht der Klasse. Sie sind gegenüber, wie Sie diese zuvor in diesem Lernprogramm erstellt haben.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Die erste `Create`-Aktionsmethode (HTTP GET) zeigt das erste Formular „Create“ an. Die zweite Version (`[HttpPost]`) verarbeitet die Formularbereitstellung. Die zweite `Create`-Methode (die `HttpPost`-Version) ruft `ModelState.IsValid` auf, um zu überprüfen, ob der Film Validierungsfehler aufweist. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungsfehler enthält, zeigt die `Create`-Methode das Formular erneut an. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank. In unserem Beispiel Film **Form wird nicht an den Server gesendet, treten Validierungsfehler auf der Clientseite; ermittelt das zweite** `Create` **wird nie aufgerufen**. Die Clientvalidierung ist deaktiviert, wenn Sie JavaScript in Ihrem Browser deaktivieren, und die HTTP-POST `Create` Methodenaufrufe `ModelState.IsValid` überprüfen, ob der Film Validierungsfehler aufweist.

Sie können einen Haltepunkt in der `HttpPost Create`-Methode festlegen und überprüfen, ob die Methode tatsächlich niemals aufgerufen wird und die clientseitige Validierung die Formulardaten nicht sendet, wenn Validierungsfehler gefunden werden. Wenn Sie JavaScript in Ihrem Browser deaktivieren und dann das Formular mit Fehlern senden, wird der Haltepunkt erreicht. Sie erhalten auch ohne JavaScript weiterhin eine vollständige Validierung. Die folgende Abbildung zeigt, wie JavaScript in Internet Explorer deaktiviert.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Die folgende Abbildung zeigt, wie JavaScript im Firefox-Browser deaktiviert wird.

![](adding-validation/_static/image7.png)

Die folgende Abbildung zeigt, wie JavaScript im Chrome-Browser deaktiviert wird.

![](adding-validation/_static/image8.png)

Im folgenden finden Sie die *Create.cshtml* Vorlage anzeigen, die Sie zuvor im Lernprogramm Gerüstbau. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Beachten Sie, wie der Code verwendet eine `Html.EditorFor` Hilfsmethode zum Ausgeben der `<input>` -Element für jede `Movie` Eigenschaft. Neben dieser Hilfsfunktion aufgerufen wird, die `Html.ValidationMessageFor` Hilfsmethode. Diese zwei Hilfsmethoden arbeiten mit dem Modellobjekt, das vom Controller an die Ansicht übergeben wird (in diesem Fall eine `Movie` Objekt). Sie suchen Sie automatisch nach Validierungsattributen für die Fehlermeldungen Modell und die Anzeige entsprechend angegeben.

Wirklich nützlich an diesem Ansatz ist, dass weder der Controller noch die Ansichtsvorlage `Create` an den eigentlichen Validierungsregeln, die erzwungen werden, oder den spezifischen Fehlermeldungen, die angezeigt werden, beteiligt sind. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben. Diese gleichen Validierungsregeln werden automatisch auf die Ansicht `Edit` und alle anderen Ansichtsvorlagen angewendet, die Sie erstellen und die das Modell bearbeiten.

Falls Sie die Validierungslogik später ändern möchten, Sie können dazu genau zentral durch Hinzufügen von Validierungsattributen für das Modell (in diesem Beispiel wird die `movie` Klasse). Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Dies bedeutet, dass Sie vollständig berücksichtigt werden müssen, und die *TROCKENEN* Prinzip.

## <a name="using-datatype-attributes"></a>Verwenden von „DataType“-Attributen

Öffnen Sie die Datei *Movie.cs*, und überprüfen Sie die Klasse `Movie`. Die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt zusätzlich zu den integrierten Satz von Validierungsattributen Formatierungsattribute bereit. Wir haben bereits angewendet haben eine [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Enumerationswert der freigabetermin und den Preis Feldern. Der folgende code zeigt die `ReleaseDate` und `Price` Eigenschaften mit dem entsprechenden [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribut.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribute werden nur Hinweise für das Ansichtsmodul formatieren die Daten bereitstellen (und geben Sie Attribute wie z. B. `<a>` für URLs und `<a href="mailto:EmailAddress.com">` zum Abrufen von e-Mails. Sie können die [der reguläre Ausdruck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) Attribut, um das Format der Daten zu überprüfen. Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribut wird verwendet, um einen Datentyp angeben, die als Datenbank systeminternen Typ genauer bestimmt ist, sind sie ***nicht*** Validierungsattribute. In diesem Fall möchten wir nur zum Nachverfolgen der Datum nicht das Datum und die Uhrzeit. Die [DataType-Enumeration](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) bietet für viele Datentypen, wie z. B. *Date "," Time "," PhoneNumber "," Währung "," EmailAddress* und vieles mehr. Das `DataType`-Attribut kann der Anwendung auch ermöglichen, typspezifische Features bereitzustellen. Z. B. eine `mailto:` Link erstellt werden kann, für [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), und ein Datum Selektor kann bereitgestellt werden, für die [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) in Browsern mit Unterstützung [HTML5](http://html5.org/). Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribute ausgibt HTML 5 [Data -](http://ejohn.org/blog/html-5-data-attributes/) (ausgesprochen *Daten Dash*) Attribute, die HTML 5-Browser zu bringen. Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribute bieten keine Validierung.

`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird. Standardmäßig wird das Feld "Daten" entsprechend der Standardformate basierend auf dem Server angezeigt[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


Die `ApplyFormatInEditMode` Einstellung gibt an, dass die angegebene Formatierung auch angewendet werden soll, wenn der Wert in einem Textfeld, das für die Bearbeitung angezeigt wird. (Sie möchten nicht, die für einige Felder – z. B. für die Currency-Werte nicht empfiehlt das Währungssymbol in das Textfeld für die Bearbeitung.)

Können Sie die [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut von selbst, aber es ist im Allgemeinen empfiehlt sich, verwenden Sie die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) auch Attribut. Die `DataType` Attribut vermittelt die *Semantik* der Daten als dagegen spricht, wie er auf dem Bildschirm gerendert werden, und bietet die folgenden Vorteile, die Sie nicht mit erhalten `DisplayFormat`:

- Der Browser kann die HTML5-Funktionen (z. B. zum Anzeigen eines Kalendersteuerelements, dem Gebietsschema entsprechende Währungssymbol, Links per e-Mail usw.).
- Standardmäßig wird der Browser Rendern von Daten über das richtige Format auf Grundlage Ihrer[Gebietsschema](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Die[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) Attribut kann MVC auf das Feld nach rechts-Vorlage zum Rendern der Daten auswählen, aktivieren (die [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) verwendeten selbst die Zeichenfolge Vorlage verwendet). Weitere Informationen finden Sie in Brad Wilsons[ASP.NET MVC 2-Vorlagen](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Obwohl für MVC 2 geschrieben, gilt dieses Artikels noch auf die aktuelle Version von ASP.NET MVC.)

Bei Verwendung der `DataType` Attribut mit einem Datumsfeld müssen Sie angeben der `DisplayFormat` Attribut auch, um sicherzustellen, dass das Feld in Chrome-Browsern ordnungsgemäß gerendert wird. Weitere Informationen finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> jQuery-Validierung funktioniert nicht mit der[Bereich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Attribut und["DateTime"](https://msdn.microsoft.com/library/system.datetime.aspx). Bei folgendem Code wird z.B. stets ein clientseitiger Validierungsfehler angezeigt, auch wenn sich das Datum im angegebenen Bereich befindet:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Sie benötigen zum Deaktivieren der Validierung von jQuery Datum verwendet die [Bereich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) -Attribut mit["DateTime"](https://msdn.microsoft.com/library/system.datetime.aspx). Es ist im Allgemeinen nicht empfohlen, kompilieren Sie feste Datumsangaben in Ihren Modellen, die mit der[Bereich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Attribut und["DateTime"](https://msdn.microsoft.com/library/system.datetime.aspx) wird abgeraten.


Der folgende Code zeigt die Kombination von Attributen in einer Zeile:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

Im nächsten Teil der Reihe überprüfen wir die Anwendung und nehmen einige Verbesserungen an den automatisch generierten Methoden `Details` und `Delete` vor.

>[!div class="step-by-step"]
[Zurück](adding-a-new-field.md)
[Weiter](examining-the-details-and-delete-methods.md)
