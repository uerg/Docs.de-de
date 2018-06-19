---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Hinzufügen einer Validierung zum Modell | Microsoft Docs
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 39d1d9d4cb8b11f7ce5a3a85c51f652115d79db7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874547"
---
<a name="adding-validation-to-the-model"></a>Hinzufügen einer Validierung zum Modell
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


In diesem Abschnitt fügen Sie Validierungslogik auf die `Movie` Modell, und Sie stellen sicher, dass jederzeit die Validierungsregeln, wenn ein Benutzer versucht erzwungen werden, erstellen oder Bearbeiten eines Films mithilfe der Anwendung.

## <a name="keeping-things-dry"></a>Halten Dinge TROCKENEN

Einer der Kernaspekte Entwurf von ASP.NET MVC ist trocken (&quot;wiederhole Dich nicht&quot;). ASP.NET MVC vertraut zu machen, können Funktionen oder Verhalten nur einmal angeben, und dann überall in einer Anwendung berücksichtigt werden. Dadurch reduziert die Menge an Code, den Sie schreiben müssen und den Code, den Sie weniger Fehler fehleranfällig und leichter schreiben zu verwalten.

Zur Unterstützung der Überprüfung von ASP.NET MVC und Entity Framework Code First ist ein hervorragendes Beispiel für das TROCKENEN Prinzip in Aktion. Sie können Validierungsregeln deklarativ angeben, an einem Ort (in der Modellklasse), und die Regeln werden überall in der Anwendung erzwungen.

Sehen wir uns, wie Sie diese Überprüfung-Unterstützung in der Film-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln, die Film-Modell

Sie beginnen, indem Sie die Überprüfung Programmlogik zum Hinzufügen der `Movie` Klasse.

Öffnen Sie Datei *Movie.cs*. Hinzufügen einer `using` -Anweisung am Anfang der Datei, die verweist auf die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Beachten Sie der Namespace enthält keine `System.Web`. DataAnnotations bietet einen integrierten Satz von Validierungsattributen, die deklarativ auf eine beliebige Klasse oder eine Eigenschaft angewendet werden können.

Jetzt aktualisieren der `Movie` Klasse, um die integrierte nutzen [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), und [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierungsattribute . Verwendung des folgenden Codes als Beispiel dafür, wo Sie die Attribute gelten.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Führen Sie die Anwendung, und erhalten Sie erneut den folgenden zur Laufzeitfehler:

***Das Modell, sichern den Kontext "MovieDBContext" wurde geändert, seit der Erstellung der Datenbank. Erwägen Sie Code First-Migrationen zum Aktualisieren der Datenbank ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Wir werden Migrationen verwenden, um das Schema zu aktualisieren. Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster, und geben Sie die folgenden Befehle:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse mit dem angegebenen Namen (*AddDataAnnotationsMig*), und klicken Sie in der `Up` Methode können Sie den Code, der updates anzuzeigen die schemaeinschränkungen. Die `Title` und `Genre` Felder sind nicht mehr NULL-Werte zulässt (d. h., geben Sie einen Wert) und die `Rating` Feld hat eine maximale Länge von 5.

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Die `Required` Attribut gibt an, dass eine Eigenschaft muss einen Wert besitzen; in diesem Beispiel verfügt über ein Film Werte für die `Title`, `ReleaseDate`, `Genre`, und `Price` Eigenschaften, damit es gültig ist. Das Attribut `Range` schränkt einen Wert auf einen bestimmten Bereich ein. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen. Systeminterne Typen (z. B. `decimal, int, float, DateTime`) standardmäßig erforderlich sind, und müssen nicht die `Required` Attribut.

Code wird zuerst sichergestellt, dass der Validierungsregeln, die Sie, auf eine Modellklasse angeben erzwungen werden, bevor die Anwendung Änderungen in der Datenbank gespeichert. Z. B. der folgenden Code wird eine Ausnahme ausgelöst bei der `SaveChanges` -Methode aufgerufen wird, da einige erforderliche `Movie` Eigenschaftswerte fehlen und der Preis ist 0 (null) (also außerhalb des gültigen Bereichs).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Mit Validierungsregeln, die von .NET Framework automatisch erzwungen. dadurch, dass Ihre Anwendung unter Umständen stabiler. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

Hier ist eine vollständige codeauflistung für die aktualisierte *Movie.cs* Datei:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Validierungsfehler Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der */Movies* URL.

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzufügen. Füllen Sie das Formular mit einigen ungültigen Werten, und klicken Sie dann auf die **erstellen** Schaltfläche.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> auf die jQuery-Validierung für nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (&quot;,&quot;) Sie müssen für ein Dezimaltrennzeichen einschließen *globalize.js* und Ihre spezifischen *cultures/globalize.cultures.js* Datei (aus [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`. Der folgende Code zeigt die Änderungen an der Views\Movies\Edit.cshtml-Datei zur Bearbeitung der &quot;fr-FR&quot; Kultur:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Beachten Sie, wie das Formular automatisch eine roten Rahmenfarbe zum verwendet die Textfelder hervorzuheben, die ungültige Daten enthalten, und verfügt über eine entsprechende Überprüfungsfehlermeldung neben jeweils ausgegeben. Die Fehlermeldungen werden sowohl auf Clientseite (mithilfe von JavaScript und jQuery) als auch auf Serverseite erzwungen (wenn ein Benutzer JavaScript deaktiviert hat).

Ein echter Vorteil ist, dass Sie nicht in eine einzelne Codezeile ändern müssen die `MoviesController` Klasse oder in der *Create.cshtml* anzeigen, um diese Überprüfung Benutzeroberfläche zu aktivieren. Die Controller und Ansichten, die Sie zuvor in diesem Tutorial erstellt haben, haben die angegebenen Validierungsregeln automatisch übernommen (mithilfe der Validierungsattribute für die Eigenschaften der Modellklasse `Movie`).

Ihnen möglicherweise aufgefallen für die Eigenschaften `Title` und `Genre`, das erforderliche Attribut wird nicht erzwungen, bis der Übermittlung des Formulars (drücken Sie die **erstellen** Schaltfläche), oder geben Sie Text in das Eingabefeld und entfernt sie. Für ein Feld möglich (z. B. die Felder auf die Erstellungsansicht) anfänglich leer ist und auf dem nur das erforderliche Attribut und keine anderen Validierungsattribute, ist Sie Folgendes ein, um die Validierung auslösen:

1. Registerkarte ", in das Feld ein.
2. Geben Sie Text ein.
3. Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld.
4. Auf der Registerkarte zurück in das Feld ein.
5. Entfernen Sie den Text ein.
6. Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld.

Die oben angegebene Reihenfolge wird die erforderliche Überprüfung auslösen, ohne dafür die Schaltfläche "Absenden". Drücken einfach die Schaltfläche "Absenden" ohne Eingabe eines der Felder wird die clientseitige Validierung ausgelöst. Die Formulardaten werden erst an den Server gesendet, wenn auf Clientseite keine Validierungsfehler mehr auftreten. Können Sie testen, wenn Sie einen Haltepunkt in der HTTP-Post-Methode oder mithilfe der [Fiddler-Tool](http://fiddler2.com/fiddler2/) oder der Internet Explorer 9 [F12 Entwicklertools](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Wie in der Validierung erstellen, anzeigen und Aktionsmethode erstellen

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. Die nächste Auflistung zeigt, was die `Create` Methoden in der `MovieController` sieht der Klasse. Sie sind gegenüber, wie Sie diese zuvor in diesem Lernprogramm erstellt haben.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Die erste `Create`-Aktionsmethode (HTTP GET) zeigt das erste Formular „Create“ an. Die zweite Version (`[HttpPost]`) verarbeitet die Formularbereitstellung. Die zweite `Create`-Methode (die `HttpPost`-Version) ruft `ModelState.IsValid` auf, um zu überprüfen, ob der Film Validierungsfehler aufweist. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungsfehler enthält, zeigt die `Create`-Methode das Formular erneut an. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank. In unserem Film-Beispiel wir verwenden, **Form wird nicht an den Server gesendet, treten Validierungsfehler auf der Clientseite; ermittelt das zweite** `Create` **wird nie aufgerufen**. Die Clientvalidierung ist deaktiviert, wenn Sie JavaScript in Ihrem Browser deaktivieren, und die HTTP-POST `Create` Methodenaufrufe `ModelState.IsValid` überprüfen, ob der Film Validierungsfehler aufweist.

Sie können einen Haltepunkt in der `HttpPost Create`-Methode festlegen und überprüfen, ob die Methode tatsächlich niemals aufgerufen wird und die clientseitige Validierung die Formulardaten nicht sendet, wenn Validierungsfehler gefunden werden. Wenn Sie JavaScript in Ihrem Browser deaktivieren und dann das Formular mit Fehlern senden, wird der Haltepunkt erreicht. Sie erhalten auch ohne JavaScript weiterhin eine vollständige Validierung. Die folgende Abbildung zeigt, wie JavaScript in Internet Explorer deaktiviert.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Die folgende Abbildung zeigt, wie JavaScript im Firefox-Browser deaktiviert wird.

![](adding-validation-to-the-model/_static/image5.png)

Die folgende Abbildung zeigt, wie JavaScript mit der Chrome-Browser deaktiviert.

![](adding-validation-to-the-model/_static/image6.png)

Im folgenden finden Sie die *Create.cshtml* Vorlage anzeigen, die Sie zuvor im Lernprogramm Gerüstbau. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Beachten Sie, wie der Code verwendet eine `Html.EditorFor` Hilfsmethode zum Ausgeben der `<input>` -Element für jede `Movie` Eigenschaft. Neben dieser Hilfsfunktion aufgerufen wird, die `Html.ValidationMessageFor` Hilfsmethode. Diese zwei Hilfsmethoden arbeiten mit dem Modellobjekt, das vom Controller an die Ansicht übergeben wird (in diesem Fall eine `Movie` Objekt). Sie suchen Sie automatisch nach Validierungsattributen für die Fehlermeldungen Modell und die Anzeige entsprechend angegeben.

Wirklich Nützliches über diesen Ansatz ist, dass der Controller weder die Vorlage zur Erstellung von Ansicht nichts über den tatsächlichen Validierungsregeln erzwungen wird oder über die spezifischen Fehlermeldungen angezeigt weiß. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben. Diese gleichen Validierungsregeln werden automatisch angewendet, die Bearbeitungsansicht und alle anderen Sichten Vorlagen, die Sie erstellen können, die das Modell zu bearbeiten.

Falls Sie die Validierungslogik später ändern möchten, Sie können dazu genau zentral durch Hinzufügen von Validierungsattributen für das Modell (in diesem Beispiel wird die `movie` Klasse). Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Und dies bedeutet, dass Sie das DRY-Prinzip vollständig einhalten.

## <a name="adding-formatting-to-the-movie-model"></a>Hinzufügen von Formatierung zur Film-Modell

Öffnen Sie die Datei *Movie.cs*, und überprüfen Sie die Klasse `Movie`. Die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt zusätzlich zu den integrierten Satz von Validierungsattributen Formatierungsattribute bereit. Wir haben bereits angewendet haben eine [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Enumerationswert der freigabetermin und den Preis Feldern. Der folgende code zeigt die `ReleaseDate` und `Price` Eigenschaften mit dem entsprechenden [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Die [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribute sind keine Validierungsattribute, sie werden verwendet, um dem Ansichtsmodul anweisen, wie HTML zu rendern. Im Beispiel oben die `DataType.Date` Attribut zeigt die Film Datumsangaben als nur Datumsangaben ohne Uhrzeit an. Beispielsweise die folgenden [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Attribute werden nicht überprüft das Format der Daten:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Die oben aufgeführten Attribute werden nur Hinweise für das Ansichtsmodul formatieren die Daten bereitstellen (und geben Sie Attribute wie z. B. &lt;eine&gt; für URLs und &lt;eine Href =&quot;mailto:EmailAddress.com&quot; &gt; zum Abrufen von e-Mails. Sie können die [der reguläre Ausdruck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) Attribut, um das Format der Daten zu überprüfen.

Eine alternative Methode zum Verwenden der `DataType` Attribute die, Sie explizit festlegen einer [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) Wert. Der folgende Code zeigt die Version Date-Eigenschaft mit einer Formatzeichenfolge für Datum (nämlich &quot;d&quot;). Sie würden diese verwenden, um anzugeben, dass Sie auf die Zeit als Teil des Datums für die Veröffentlichung nicht möchten.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Die vollständige `Movie` ist unten dargestellt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller. Das Veröffentlichungsdatum und Preis werden ordentlich formatiert. Die folgende Abbildung zeigt das Veröffentlichungsdatum und Preis mit &quot;fr-FR&quot; als Kultur.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Die folgende Abbildung zeigt die gleichen Daten angezeigt, wobei die Standardkultur (Englisch, USA).

![](adding-validation-to-the-model/_static/image8.png)

Im nächsten Teil der Reihe überprüfen wir die Anwendung und nehmen einige Verbesserungen an den automatisch generierten Methoden `Details` und `Delete` vor.

> [!div class="step-by-step"]
> [Zurück](adding-a-new-field-to-the-movie-model-and-table.md)
> [Weiter](examining-the-details-and-delete-methods.md)
