---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: "Hinzufügen einer Validierung für das Modell (c#) | Microsoft Docs"
author: Rick-Anderson
description: "Erstellen eines Domänencontrollers"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 1e1235972d1e16153faee113af09edaa676d70d8
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation-to-the-model-c"></a>Hinzufügen einer Validierung für das Modell (c#)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.
> 
> 
> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung. [Die C#-Version herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.


In diesem Abschnitt fügen Sie Validierungslogik auf die `Movie` Modell, und Sie stellen sicher, dass jederzeit die Validierungsregeln, wenn ein Benutzer versucht erzwungen werden, erstellen oder Bearbeiten eines Films mithilfe der Anwendung.

## <a name="keeping-things-dry"></a>Halten Dinge TROCKENEN

Einer der Kernaspekte Entwurf von ASP.NET MVC ist trocken ("wiederhole Dich nicht"). ASP.NET MVC vertraut zu machen, können Funktionen oder Verhalten nur einmal angeben, und dann überall in einer Anwendung berücksichtigt werden. Dadurch reduziert die Menge an Code, den Sie schreiben müssen und den Code, den Sie viel einfacher schreiben zu verwalten.

Zur Unterstützung der Überprüfung von ASP.NET MVC und Entity Framework Code First ist ein hervorragendes Beispiel für das TROCKENEN Prinzip in Aktion. Sie können Validierungsregeln deklarativ angeben, an einem Ort (in der Modellklasse), und klicken Sie dann diese Regeln überall in der Anwendung erzwungen werden.

Sehen wir uns, wie Sie diese Überprüfung-Unterstützung in der Film-Anwendung nutzen können.

## <a name="adding-validation-rules-to-the-movie-model"></a>Hinzufügen von Validierungsregeln, die Film-Modell

Sie beginnen, indem Sie die Überprüfung Programmlogik zum Hinzufügen der `Movie` Klasse.

Öffnen Sie Datei *Movie.cs*. Hinzufügen einer `using` -Anweisung am Anfang der Datei, die verweist auf die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Der Namespace ist Teil von .NET Framework. Er bietet einen integrierten Satz von Validierungsattributen, die deklarativ auf eine beliebige Klasse oder eine Eigenschaft angewendet werden können.

Jetzt aktualisieren der `Movie` Klasse, um die integrierte nutzen [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), und [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Validierungsattribute . Verwendung des folgenden Codes als Beispiel dafür, wo Sie die Attribute gelten.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Die Validierungsattribute geben das Verhalten an, das Sie in den Modelleigenschaften erzwingen möchten, auf die sie angewendet werden. Die `Required` Attribut gibt an, dass eine Eigenschaft muss einen Wert besitzen; in diesem Beispiel verfügt über ein Film Werte für die `Title`, `ReleaseDate`, `Genre`, und `Price` Eigenschaften, damit es gültig ist. Das Attribut `Range` schränkt einen Wert auf einen bestimmten Bereich ein. Mit dem Attribut `StringLength` können Sie die maximale Länge einer Zeichenfolgeneigenschaft und optional die minimale Länge festlegen.

Code wird zuerst sichergestellt, dass der Validierungsregeln, die Sie, auf eine Modellklasse angeben erzwungen werden, bevor die Anwendung Änderungen in der Datenbank gespeichert. Z. B. der folgenden Code wird eine Ausnahme ausgelöst bei der `SaveChanges` -Methode aufgerufen wird, da einige erforderliche `Movie` Eigenschaftswerte fehlen und der Preis ist 0 (null) (also außerhalb des gültigen Bereichs).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Mit Validierungsregeln, die von .NET Framework automatisch erzwungen. dadurch, dass Ihre Anwendung unter Umständen stabiler. Darüber hinaus wird sichergestellt, dass Sie die Validierung nicht vergessen und nicht versehentlich falsche Daten in die Datenbank übernehmen.

Hier ist eine vollständige codeauflistung für die aktualisierte *Movie.cs* Datei:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Validierungsfehler Benutzeroberfläche in ASP.NET MVC

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der */Movies* URL.

Klicken Sie auf die **Film erstellen** Link, um einen neuen Film hinzufügen. Füllen Sie das Formular mit einigen ungültigen Werten, und klicken Sie dann auf die **erstellen** Schaltfläche.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Beachten Sie, wie das Formular automatisch eine Hintergrundfarbe zum verwendet die Textfelder hervorzuheben, die ungültige Daten enthalten, und verfügt über eine entsprechende Überprüfungsfehlermeldung neben jeweils ausgegeben. Die Fehlermeldungen entsprechen die Fehlerzeichenfolgen, die Sie angegeben, wenn Sie mit Anmerkungen versehen der `Movie` Klasse. Die Fehler werden erzwungen, die clientseitige (mit JavaScript) und serverseitigen (für den Fall, dass ein Benutzer JavaScript deaktiviert wurde).

Ein echter Vorteil ist, dass Sie nicht in eine einzelne Codezeile ändern müssen die `MoviesController` Klasse oder in der *Create.cshtml* anzeigen, um diese Überprüfung Benutzeroberfläche zu aktivieren. Der Controller und Ansichten, die Sie zuvor in diesem Lernprogramm automatisch erstellt entnommen oben Validierungsregeln, die Sie angegeben verwenden von Attributen auf die `Movie` modellschemas.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Wie in der Validierung erstellen, anzeigen und Aktionsmethode erstellen

Sie fragen sich vielleicht, wie die Benutzeroberfläche für die Validierung ohne Aktualisierungen von Code im Controller oder in Ansichten generiert wurde. Die nächste Auflistung zeigt, was die `Create` Methoden in der `MovieController` sieht der Klasse. Sie sind gegenüber, wie Sie diese zuvor in diesem Lernprogramm erstellt haben.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Die erste Aktionsmethode zeigt das erste Formular erstellen. Die zweite behandelt die Formular-Post. Die zweite `Create` Methodenaufrufe `ModelState.IsValid` überprüfen, ob der Film Validierungsfehler aufweist. Beim Aufrufen dieser Methode werden alle Validierungsattribute ausgewertet, die auf das Objekt angewendet wurden. Wenn das Objekt Validierungsfehler, hat die `Create` Methode erneut an das Formular. Wenn keine Fehler vorliegen, speichert die Methode den neuen Film in der Datenbank.

Im folgenden finden Sie die *Create.cshtml* Vorlage anzeigen, die Sie zuvor im Lernprogramm Gerüstbau. Sie wird von den oben erläuterten Aktionsmethoden zum Anzeigen des anfänglichen Formulars und zum erneuten Anzeigen des Formulars bei einem Fehler verwendet.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Beachten Sie, wie der Code verwendet eine `Html.EditorFor` Hilfsmethode zum Ausgeben der `<input>` -Element für jede `Movie` Eigenschaft. Neben dieser Hilfsfunktion aufgerufen wird, die `Html.ValidationMessageFor` Hilfsmethode. Diese zwei Hilfsmethoden arbeiten mit dem Modellobjekt, das vom Controller an die Ansicht übergeben wird (in diesem Fall eine `Movie` Objekt). Sie suchen Sie automatisch nach Validierungsattributen für die Fehlermeldungen Modell und die Anzeige entsprechend angegeben.

Wirklich Nützliches über diesen Ansatz ist, dass der Controller weder die Vorlage zur Erstellung von Ansicht nichts über den tatsächlichen Validierungsregeln erzwungen wird oder über die spezifischen Fehlermeldungen angezeigt weiß. Die Validierungsregeln und Fehlerzeichenfolgen werden nur in der `Movie`-Klasse angegeben.

Wenn Sie die Validierungslogik später ändern möchten, können genau zentral möchten. Sie müssen sich keine Gedanken darüber machen, ob die verschiedenen Teile der Anwendung inkonsistent sind und wie Regeln erzwungen werden: Die gesamte Validierungslogik wird zentral definiert und überall verwendet. Dies hält den Code sehr übersichtlich und vereinfacht die Verwaltung und Entwicklung. Und dies bedeutet, dass Sie das DRY-Prinzip vollständig einhalten.

## <a name="adding-formatting-to-the-movie-model"></a>Hinzufügen von Formatierung zur Film-Modell

Öffnen Sie Datei *Movie.cs*. Die [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace stellt zusätzlich zu den integrierten Satz von Validierungsattributen Formatierungsattribute bereit. Wenden Sie die [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut und einem [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) Enumerationswert der freigabetermin und den Preis Feldern. Der folgende code zeigt die `ReleaseDate` und `Price` Eigenschaften mit dem entsprechenden [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Attribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Alternativ können Sie explizit festlegen einer [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) Wert. Der folgende Code zeigt die Version Date-Eigenschaft mit einer Formatzeichenfolge für Datum (d. h., "d"). Sie würden diese verwenden, um anzugeben, dass Sie auf die Zeit als Teil des Datums für die Veröffentlichung nicht möchten.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Im folgenden Codebeispiel Formate der `Price` Eigenschaft als Währung.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Die vollständige `Movie` ist unten dargestellt.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Im nächsten Teil der Reihe überprüfen wir die Anwendung und nehmen einige Verbesserungen an den automatisch generierten Methoden `Details` und `Delete` vor.

>[!div class="step-by-step"]
[Zurück](adding-a-new-field.md)
[Weiter](improving-the-details-and-delete-methods.md)
