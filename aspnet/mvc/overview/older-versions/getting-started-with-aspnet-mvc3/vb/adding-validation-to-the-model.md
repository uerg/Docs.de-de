---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Hinzufügen der Überprüfung zum Modell (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: d2b4fb9d8f673f223c923ba0705b1ea27d430f43
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830353"
---
<a name="adding-validation-to-the-model-vb"></a>Hinzufügen der Überprüfung zum Modell (VB)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET steht zur Ergänzung dieses Themas. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C# -Code bevorzugen, wechseln Sie zu der [c#-Version](../cs/adding-validation-to-the-model.md) in diesem Tutorial.


In diesem Abschnitt fügen Sie Validierungslogik auf die `Movie` Modell, und stellen sicher, dass die Validierungsregeln immer, die ein Benutzer versucht erzwungen werden, erstellen oder Bearbeiten eines Films mit der Anwendung.

## <a name="keeping-things-dry"></a>Halten Dinge DRY

Einer der Entwurfsgrundsätze von ASP.NET MVC Core ist DRY ("wiederhole Dich nicht"). ASP.NET MVC empfiehlt Ihnen, Funktionalität und Verhalten nur einmal angeben, und klicken Sie dann es überall in einer Anwendung berücksichtigt werden. Dadurch reduziert die Menge an Code, den Sie schreiben müssen und den Code, den Sie viel leichter schreiben zu verwalten.

ASP.NET MVC und Entity Framework Code First angebotene Unterstützung der Validierung ist ein gutes Beispiel des DRY-Prinzips in Aktion. Sie können deklarativ Validierungsregeln an zentraler Stelle (in der Modellklasse) angeben, und klicken Sie dann diese Regeln überall in der Anwendung erzwungen werden.

Sehen wir uns, wie Sie diese überprüfungsunterstützung in der Movie-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln zum Modell "Movie"

Sie beginnen mit der Überprüfung Programmlogik zum Hinzufügen der `Movie` Klasse.

Öffnen der *Movie.vb* Datei. Hinzufügen einer `Imports` -Anweisung am Anfang der Datei, die verweist auf die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Der Namespace ist Teil von .NET Framework. Es bietet einen integrierten Satz von Validierungsattributen, die Sie deklarativ auf eine Klasse oder Eigenschaft anwenden können.

Aktualisieren Sie jetzt die `Movie` Klasse nutzen der integrierten [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), und [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierungsattribute . Verwenden Sie den folgenden Code als Beispiel dafür, wo Sie die Attribute anwenden.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Die `Required` Attribut gibt an, dass eine Eigenschaft einen Wert verfügen muss, die in diesem Beispiel verfügt über ein Film Werte für die `Title`, `ReleaseDate`, `Genre`, und `Price` Eigenschaften, damit es gültig ist. Das Attribut `Range` schränkt einen Wert auf einen bestimmten Bereich ein. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen.

Code wird zunächst an, dass die Validierungsregeln, die Sie, auf eine Modellklasse angeben erzwungen werden, bevor die Anwendung Änderungen in der Datenbank speichert. Z. B. der folgenden Code wird eine Ausnahme ausgelöst bei der `SaveChanges` -Methode aufgerufen wird, da einige erforderliche `Movie` Eigenschaftswerte fehlen und der Preis ist 0 (null) (die außerhalb des gültigen Bereichs).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Dadurch, dass Validierungsregeln automatisch erzwungen, die von .NET Framework machen hilft Ihrer Anwendung stabiler. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

Hier ist eine vollständige codeauflistung für die aktualisierte *Movie.vb* Datei:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Fehler bei der Validierung Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der */Movies* URL.

Klicken Sie auf die **erstellen Film** Link, um einen neuen Film hinzuzufügen. Füllen Sie das Formular mit einigen ungültigen Werten aus, und klicken Sie dann auf die **erstellen** Schaltfläche.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Beachten Sie, wie das Formular automatisch eine Hintergrundfarbe verwendet hat, den Inhalt der Textfelder hervorzuheben, die ungültige Daten enthalten, und verfügt über eine entsprechende Validierungsfehlermeldung neben jeder ausgegeben. Die Fehlermeldungen entsprechen die Fehlerzeichenfolgen, die Sie angegeben, wenn Sie mit Anmerkungen versehen der `Movie` Klasse. Die Fehlermeldungen werden sowohl auf Clientseite (mithilfe von JavaScript) und serverseitige erzwungen, (für den Fall, dass ein Benutzer JavaScript deaktiviert hat).

Ein echter Vorteil ist, dass Sie nicht in eine einzige Codezeile ändern müssen die `MoviesController` Klasse oder in der *Create.vbhtml* anzeigen, um diese Benutzeroberfläche für die Validierung zu aktivieren. Die Controller und Ansichten, die Sie zuvor in diesem Tutorial automatisch erstellt übernommen Validierungsregeln, die Sie angegeben, unter Verwendung von Attributen auf die `Movie` Modellklasse.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Wie in der Validierung erstellen, anzeigen, und Erstellen von Action-Methode

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. Die nächste Liste wird gezeigt, wie die `Create` Methoden in der `MovieController` Klasse aus. Es handelt sich gegenüber, wie Sie diese zuvor in diesem Tutorial erstellt haben.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

Die erste Aktionsmethode zeigt das Formular erstellen. Die zweite verarbeitet die formularbereitstellung. Die zweite `Create` Methodenaufrufe `ModelState.IsValid` zu überprüfen, ob der Film Validierungsfehler aufweist. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungsfehler enthält die `Create` -Methode zeigt das Formular. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank.

Im folgenden finden Sie die *Create.vbhtml* Vorlage anzeigen, die Sie zuvor in diesem Tutorial erstellt haben. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Beachten Sie, wie der Code verwendet ein `Html.EditorFor` Hilfsmethode zum Ausgeben der `<input>` -Element für jede `Movie` Eigenschaft. Neben dieses Hilfsprogramm ist ein Aufruf der `Html.ValidationMessageFor` Hilfsmethode. Diese zwei Hilfsmethoden arbeiten mit dem Modellobjekt, das vom Controller an die Ansicht übergeben wird (in diesem Fall eine `Movie` Objekt). Diese Suche automatisch für Validierungsattribute, die für die Fehlermeldungen Modell und die Anzeige entsprechend angegeben.

Was ist wirklich nützlich an diesem Ansatz, dass weder der Controller noch die ansichtsvorlage erstellen nichts über den eigentlichen Validierungsregeln, die erzwungen wird, oder über die spezifischen Fehlermeldungen angezeigt bekannt ist. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben.

Wenn Sie die Validierungslogik später ändern möchten, können an genau einer Stelle möchten. Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Und dies bedeutet, dass Sie das DRY-Prinzip vollständig einhalten.

## <a name="adding-formatting-to-the-movie-model"></a>Hinzufügen von Formatierung zur Modell "Movie"

Öffnen der *Movie.vb* Datei. Die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt neben dem integrierten Satz von Validierungsattributen Formatierungsattribute bereit. Wenden Sie die [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut und einem [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Enumerationswert, um das Veröffentlichungsdatum und Preisfelder. Der folgende code zeigt die `ReleaseDate` und `Price` Eigenschaften mit dem entsprechenden [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Alternativ können Sie explizit festlegen einer [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) Wert. Der folgende Code zeigt die Release Date-Eigenschaft mit einer Formatzeichenfolge für Datum (d. h., "d"). Sie würden diese verwenden, um anzugeben, dass die Zeit als Teil das Datum der Veröffentlichung keine werden soll.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Der folgende code die Formate der `Price` Eigenschaft als Währung.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Die vollständige `Movie` ist unten dargestellt.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

In den nächsten Teil der Reihe, überprüfen Sie die Anwendung und wir nehmen einige Verbesserungen an den automatisch generierten `Details` und `Delete` Methoden...

> [!div class="step-by-step"]
> [Zurück](adding-a-new-field.md)
> [Weiter](improving-the-details-and-delete-methods.md)
