---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Hinzufügen der Überprüfung zum Modell | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 47c3f16d4592d2f61c6f1c3c1988e3622cb84a00
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384802"
---
<a name="adding-validation-to-the-model"></a>Hinzufügen der Überprüfung zum Modell
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.


In diesem Abschnitt fügen Sie Validierungslogik auf die `Movie` Modell, und stellen sicher, dass die Validierungsregeln immer, die ein Benutzer versucht erzwungen werden, erstellen oder Bearbeiten eines Films mit der Anwendung.

## <a name="keeping-things-dry"></a>Halten Dinge DRY

Einer der Entwurfsgrundsätze von ASP.NET MVC Core DRY ist (&quot;wiederhole Dich nicht&quot;). ASP.NET MVC empfiehlt Ihnen, Funktionalität und Verhalten nur einmal angeben, und klicken Sie dann es überall in einer Anwendung berücksichtigt werden. Dadurch reduziert die Menge an Code, den Sie schreiben müssen und den Code, den Sie weniger Fehler verursachenden und einfacher schreiben zu verwalten.

ASP.NET MVC und Entity Framework Code First angebotene Unterstützung der Validierung ist ein gutes Beispiel des DRY-Prinzips in Aktion. Sie können deklarativ Validierungsregeln an zentraler Stelle (in der Modellklasse) angeben und die Regeln werden überall in der Anwendung erzwungen.

Sehen wir uns, wie Sie diese überprüfungsunterstützung in der Movie-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln zum Modell "Movie"

Sie beginnen mit der Überprüfung Programmlogik zum Hinzufügen der `Movie` Klasse.

Öffnen Sie Datei *Movie.cs*. Hinzufügen einer `using` -Anweisung am Anfang der Datei, die verweist auf die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Beachten Sie, dass der Namespace enthält keine `System.Web`. "DataAnnotations" bietet einen integrierten Satz von Validierungsattributen, die Sie deklarativ auf eine Klasse oder Eigenschaft anwenden können.

Aktualisieren Sie jetzt die `Movie` Klasse nutzen der integrierten [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), und [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierungsattribute . Verwenden Sie den folgenden Code als Beispiel dafür, wo Sie die Attribute anwenden.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Führen Sie die Anwendung, und erhalten Sie erneut die Laufzeit-Fehlermeldung:

***Das Modell, das den Kontext 'MovieDBContext' unterstützt wurde geändert, da die Datenbank erstellt wurde. Erwägen Sie die Verwendung von Code First-Migrationen zum Aktualisieren der Datenbank ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Wir verwenden Migrationen zum Aktualisieren des Schemas. Erstellen Sie die Projektmappe, und öffnen Sie die **-Paket-Manager-Konsole** Fenster, und geben Sie die folgenden Befehle aus:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse mit dem angegebenen Namen (*AddDataAnnotationsMig*), und klicken Sie in der `Up` Methode sehen Sie den Code, der aktualisiert die schemaeinschränkungen. Die `Title` und `Genre` Felder sind nicht mehr NULL-Werte zulässt (d. h. Sie müssen ein einen Wert eingeben) und die `Rating` Feld hat eine maximale Länge von 5.

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Die `Required` Attribut gibt an, dass eine Eigenschaft einen Wert verfügen muss, die in diesem Beispiel verfügt über ein Film Werte für die `Title`, `ReleaseDate`, `Genre`, und `Price` Eigenschaften, damit es gültig ist. Das Attribut `Range` schränkt einen Wert auf einen bestimmten Bereich ein. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen. Systeminterne Typen (z. B. `decimal, int, float, DateTime`) sind standardmäßig erforderlich und müssen nicht die `Required` Attribut.

Code wird zunächst an, dass die Validierungsregeln, die Sie, auf eine Modellklasse angeben erzwungen werden, bevor die Anwendung Änderungen in der Datenbank speichert. Z. B. der folgenden Code wird eine Ausnahme ausgelöst bei der `SaveChanges` -Methode aufgerufen wird, da einige erforderliche `Movie` Eigenschaftswerte fehlen und der Preis ist 0 (null) (die außerhalb des gültigen Bereichs).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Dadurch, dass Validierungsregeln automatisch erzwungen, die von .NET Framework machen hilft Ihrer Anwendung stabiler. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

Hier ist eine vollständige codeauflistung für die aktualisierte *Movie.cs* Datei:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Fehler bei der Validierung Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der */Movies* URL.

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzuzufügen. Füllen Sie das Formular mit einigen ungültigen Werten aus, und klicken Sie dann auf die **erstellen** Schaltfläche.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> zur Unterstützung von jQuery-Validierung für nicht englische Gebietsschemas, in denen ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen, müssen Sie enthalten *globalize.js* und Ihren speziellen *cultures/globalize.cultures.js* Datei (aus [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`. Der folgende Code zeigt die Änderungen an der Views\Movies\Edit.cshtml-Datei können mit der &quot;"fr-FR"&quot; Kultur:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Beachten Sie, wie das Formular automatisch eine roten Rahmenfarbe verwendet wurde um den Inhalt der Textfelder hervorzuheben, die ungültige Daten enthalten, und verfügt über eine entsprechende Validierungsfehlermeldung neben jeder ausgegeben. Die Fehlermeldungen werden sowohl auf Clientseite (mithilfe von JavaScript und jQuery) als auch auf Serverseite erzwungen (wenn ein Benutzer JavaScript deaktiviert hat).

Ein echter Vorteil ist, dass Sie nicht in eine einzige Codezeile ändern müssen die `MoviesController` Klasse oder in der *Create.cshtml* anzeigen, um diese Benutzeroberfläche für die Validierung zu aktivieren. Die Controller und Ansichten, die Sie zuvor in diesem Tutorial erstellt haben, haben die angegebenen Validierungsregeln automatisch übernommen (mithilfe der Validierungsattribute für die Eigenschaften der Modellklasse `Movie`).

Sie vielleicht bemerkt haben, für die Eigenschaften `Title` und `Genre`, das required-Attribut wird nicht erzwungen werden, bis Sie das Formular übermittelt (drücken Sie die **erstellen** Schaltfläche), oder geben Sie Text in das Eingabefeld und entfernt sie. Für ein Feld möglich der anfänglich leer ist (z. B. die Felder in der Ansicht "erstellen") und dem nur das erforderliche Attribut und keine anderen Validierungsattribute, Folgendes ein, um die Überprüfung auszulösen:

1. Registerkarte ", in das Feld ein.
2. Geben Sie Text ein.
3. Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld.
4. Die Registerkarte zurück in das Feld ein.
5. Entfernen Sie den Text ein.
6. Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld.

Die oben angegebene Reihenfolge wird die erforderliche Überprüfung ausgelöst, ohne dass dafür auf die Schaltfläche "Senden". Drücken einfach die Schaltfläche "Senden" ohne Eingabe eines der Felder wird die clientseitige Validierung ausgelöst. Die Formulardaten werden erst an den Server gesendet, wenn auf Clientseite keine Validierungsfehler mehr auftreten. Sie können dies testen, indem Sie Sie in der HTTP-Post-Methode einen Haltepunkt einfügen oder die [Fiddler-Tool](http://fiddler2.com/fiddler2/) oder der Internet Explorer 9 [F12-Entwicklertools](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Wie in der Validierung erstellen, anzeigen, und Erstellen von Action-Methode

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. Die nächste Liste wird gezeigt, wie die `Create` Methoden in der `MovieController` Klasse aus. Es handelt sich gegenüber, wie Sie diese zuvor in diesem Tutorial erstellt haben.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Die erste `Create`-Aktionsmethode (HTTP GET) zeigt das erste Formular „Create“ an. Die zweite Version (`[HttpPost]`) verarbeitet die Formularbereitstellung. Die zweite `Create`-Methode (die `HttpPost`-Version) ruft `ModelState.IsValid` auf, um zu überprüfen, ob der Film Validierungsfehler aufweist. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungsfehler enthält, zeigt die `Create`-Methode das Formular erneut an. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank. In unserem Movie-Beispiel wir verwenden, **Form wird nicht an den Server gesendet, wenn Validierungsfehler erkannt werden, auf der Clientseite; es gibt die zweite** `Create` **wird nie aufgerufen**. Wenn Sie JavaScript in Ihrem Browser deaktivieren, wird die Clientvalidierung deaktiviert und die HTTP-POST `Create` Methodenaufrufe `ModelState.IsValid` zu überprüfen, ob der Film Validierungsfehler aufweist.

Sie können einen Haltepunkt in der `HttpPost Create`-Methode festlegen und überprüfen, ob die Methode tatsächlich niemals aufgerufen wird und die clientseitige Validierung die Formulardaten nicht sendet, wenn Validierungsfehler gefunden werden. Wenn Sie JavaScript in Ihrem Browser deaktivieren und dann das Formular mit Fehlern senden, wird der Haltepunkt erreicht. Sie erhalten auch ohne JavaScript weiterhin eine vollständige Validierung. Die folgende Abbildung zeigt, wie JavaScript in Internet Explorer deaktiviert.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Die folgende Abbildung zeigt, wie JavaScript im Firefox-Browser deaktiviert wird.

![](adding-validation-to-the-model/_static/image5.png)

Die folgende Abbildung zeigt, wie JavaScript mit der Chrome-Browser deaktiviert wird.

![](adding-validation-to-the-model/_static/image6.png)

Im folgenden finden Sie die *Create.cshtml* Vorlage anzeigen, die Sie zuvor in diesem Tutorial erstellt haben. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Beachten Sie, wie der Code verwendet ein `Html.EditorFor` Hilfsmethode zum Ausgeben der `<input>` -Element für jede `Movie` Eigenschaft. Neben dieses Hilfsprogramm ist ein Aufruf der `Html.ValidationMessageFor` Hilfsmethode. Diese zwei Hilfsmethoden arbeiten mit dem Modellobjekt, das vom Controller an die Ansicht übergeben wird (in diesem Fall eine `Movie` Objekt). Diese Suche automatisch für Validierungsattribute, die für die Fehlermeldungen Modell und die Anzeige entsprechend angegeben.

Was ist wirklich nützlich an diesem Ansatz, dass weder der Controller noch die ansichtsvorlage erstellen nichts über den eigentlichen Validierungsregeln, die erzwungen wird, oder über die spezifischen Fehlermeldungen angezeigt bekannt ist. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben. Diese gleichen Validierungsregeln gelten automatisch für die Bearbeitungsansicht, und alle anderen Ansichtsvorlagen, die Sie erstellen können, die das Modell zu bearbeiten.

Wenn Sie die Validierungslogik später ändern möchten, erreichen Sie dies an genau einer Stelle durch Hinzufügen der Validierungsattribute zum Modell (in diesem Beispiel die `movie` Klasse). Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Und dies bedeutet, dass Sie das DRY-Prinzip vollständig einhalten.

## <a name="adding-formatting-to-the-movie-model"></a>Hinzufügen von Formatierung zur Modell "Movie"

Öffnen Sie die Datei *Movie.cs*, und überprüfen Sie die Klasse `Movie`. Die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt neben dem integrierten Satz von Validierungsattributen Formatierungsattribute bereit. Wir haben bereits angewendet. eine [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Enumerationswert, um das Veröffentlichungsdatum und Preisfelder. Der folgende code zeigt die `ReleaseDate` und `Price` Eigenschaften mit dem entsprechenden [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Die [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribute sind keine Validierungsattribute, sie werden verwendet, um der Ansichts-Engine mitzuteilen, wie HTML gerendert. Im obigen Beispiel ist die `DataType.Date` Attribut zeigt die Movie-Daten als Datumsangaben Nur ohne Uhrzeit an. Beispielsweise die folgenden [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribute werden nicht überprüft das Format der Daten:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Die oben aufgeführten Attribute geben nur die Hinweise für die Anzeige-Engine zum Formatieren der Daten (und liefern Attribute wie z. B. &lt;eine&gt; für URLs und &lt;ein Href =&quot;mailto:EmailAddress.com&quot; &gt; für e-Mail-Adresse. Sie können die [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) Attribut, um das Format der Daten zu überprüfen.

Eine Alternative zur Verwendung der `DataType` Attribute, können Sie explizit festlegen einer [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) Wert. Der folgende Code zeigt die Release Date-Eigenschaft mit einer Formatzeichenfolge für Datum (d. h., &quot;d&quot;). Sie würden diese verwenden, um anzugeben, dass die Zeit als Teil das Datum der Veröffentlichung keine werden soll.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Die vollständige `Movie` ist unten dargestellt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller. Das Veröffentlichungsdatum und Preis sind ordentlich formatiert. Die folgende Abbildung zeigt das Veröffentlichungsdatum und Preis mit &quot;"fr-FR"&quot; als Kultur.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Die folgende Abbildung zeigt die gleichen Daten, die mit die Standardkultur (Englisch USA) angezeigt.

![](adding-validation-to-the-model/_static/image8.png)

Im nächsten Teil der Reihe überprüfen wir die Anwendung und nehmen einige Verbesserungen an den automatisch generierten Methoden `Details` und `Delete` vor.

> [!div class="step-by-step"]
> [Zurück](adding-a-new-field-to-the-movie-model-and-table.md)
> [Weiter](examining-the-details-and-delete-methods.md)
