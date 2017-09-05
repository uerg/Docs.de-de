---
title: Modellvalidierung in ASP.NET Core MVC
author: rick-anderson
description: "Führt ein modellvalidierung in ASP.NET Core MVC."
keywords: ASP.NET Core, MVC-Validierung
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 514c7770b7f508893a059c1adcf56204160aceda
ms.sourcegitcommit: 275a5381b6172b4f0b5fcd1d252aff03d3dae166
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Einführung in die modellvalidierung in ASP.NET Core MVC

Durch [Rachel Appel](http://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Einführung in die modellvalidierung

Bevor eine Anwendung Daten in einer Datenbank speichert, muss die app Daten überprüft werden. Für Sicherheitsrisiken, bestätigt, dass nach Typ, Größe und entsprechend formatiert ist, und muss den Regeln entsprechen, müssen Daten überprüft werden. Überprüfung ist erforderlich, obwohl sie redundante und einfacher zu implementieren sein kann. In MVC geschieht, Validierung auf dem Client und Server.

Glücklicherweise verfügt über .NET Überprüfung in Validierungsattribute abstrahiert. Diese Attribute enthalten Validierungscode, gesenkt und die Menge an Code, den Sie schreiben müssen.

[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Validierungsattribute

Validierungsattribute sind eine Möglichkeit, modellvalidierung zu konfigurieren, damit es grundsätzlich Überprüfung auf Felder in Datenbanktabellen ähnlich ist. Dazu gehören Einschränkungen wie z. B. das Zuweisen von Datentypen oder Pflichtfelder. Andere Typen von Validierung umfasst die Anwendung von Mustern auf Daten zum Erzwingen von Geschäftsregeln, z. B. eine Kreditkarte, Telefonnummer oder e-Mail-Adresse. Validierungsattribute stellen erzwingen diese Anforderungen wesentlich einfacher und leichter zu verwenden.

Im folgenden finden Sie eine mit Anmerkungen `Movie` -Modell aus einer app, die Informationen zu Videos und TV-Serien speichert. Die meisten Eigenschaften erforderlich sind, und mehrere Zeichenfolgeneigenschaften Länge gelten. Darüber hinaus steht eine Einschränkung numerischen Bereich vorhanden, für die `Price` Eigenschaft von 0 bis $999,99, zusammen mit einem benutzerdefinierten Validierungsattribut.

[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]

Durch das Modell einfach lesen wird die Regeln zu Daten für diese app, erleichtert es, den Code beizubehalten. Im folgenden sind einige gängige integrierte Validierungsattribute:

* `[CreditCard]`: Überprüft die Eigenschaft weist eine Kreditkarte-Format.

* `[Compare]`: Zwei Eigenschaften in einem Modell Übereinstimmung überprüft.

* `[EmailAddress]`: Überprüft die Eigenschaft hat eine e-Mail-Format.

* `[Phone]`: Überprüft die Eigenschaft verfügt über ein Telefon-Format.

* `[Range]`: Überprüft die Eigenschaft Wert in den angegebenen Bereich fällt.

* `[RegularExpression]`: Überprüft, dass die Daten mit den angegebenen regulären Ausdruck übereinstimmen.

* `[Required]`: Eine Eigenschaft erforderlich macht.

* `[StringLength]`: Überprüft, dass eine Zeichenfolgeneigenschaft höchstens die angegebene maximale Länge verfügt.

* `[Url]`: Überprüft die Eigenschaft verfügt über eine URL-Format.

MVC unterstützt jedes Attribut, das von abgeleitet ist `ValidationAttribute` zu Validierungszwecken. Viele nützliche Validierungsattribute finden Sie in der [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations(v=vs.110).aspx) Namespace.

Es kann sein, dass Instanzen, in denen Sie mehr Funktionen als integrierte Attribute bieten erforderlich, ist. Für diese Zeiten können Sie eigene benutzerdefinierte Validierungsattribute erstellen, durch Ableiten von `ValidationAttribute` oder ändern Ihr Modell implementiert `IValidatableObject`.

## <a name="model-state"></a>Modellstatus

Modellstatus darstellt Validierungsfehler in übermittelte Formularwerte in HTML.

MVC weiterhin Überprüfen von Feldern bis erreicht die maximale Anzahl von Fehlern (standardmäßig 200). Konfigurieren Sie diese Zahl, indem das Einfügen von den folgenden Code in die `ConfigureServices` Methode in der *Startup.cs* Datei:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Modellstatus Behandeln von Fehlern

Modellvalidierung vor jeder Controlleraktion aufgerufen wurde, und es wird die Aktionsmethode dafür verantwortlich, um zu überprüfen `ModelState.IsValid` und entsprechend reagieren. In vielen Fällen wird die entsprechende Reaktion irgendeine Fehlerantwort, idealerweise mit Details zu den Grund, warum modellvalidierung Fehler, zurückgegeben.

Einige apps wählen Sie eine Standardkonvention für den Umgang mit modellvalidierungsfehler, führen in diesem Fall einen Filter auf einer geeigneten Stelle, um eine solche Richtlinie zu implementieren sein kann. Sie sollten das Verhalten Ihrer Aktionen mit gültigen und ungültigen modellzustände testen.

## <a name="manual-validation"></a>Manuelle Validierung

Nachdem wurden die modellbindung und die Überprüfung abgeschlossen sind, empfiehlt es sich um Teile davon zu wiederholen. Beispielsweise ein Benutzer kann Text in einem Feld erwartet eine ganze Zahl eingegeben haben, oder müssen Sie möglicherweise einen Wert für die Eigenschaft für ein Modell zu berechnen.

Sie müssen möglicherweise Überprüfung manuell ausführen. Rufen Sie hierzu die `TryValidateModel` Methode, wie hier gezeigt:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Benutzerdefinierte Validierung)

Validierungsattribute arbeiten für die häufigsten Überprüfung Anforderungen. Einige Validierungsregeln sind jedoch spezifisch für Ihr Unternehmen, da sie nicht nur sind allgemeine Daten-Überprüfungen, z. B. ein Feld sicherstellen erforderlich ist, oder, dass es sich um einen Bereich von Werten entspricht. Für diese Szenarien sind benutzerdefinierte Validierungsattribute eine großartige Lösung. Erstellen eine eigene benutzerdefinierte Validierungsattribute in MVC ist einfach. Nur Vererben der `ValidationAttribute`, und überschreiben die `IsValid` Methode. Die `IsValid` Methode akzeptiert zwei Parameter, der erste ist ein Objekt mit dem Namen *Wert* und die zweite ein `ValidationContext` Objekt mit dem Namen *ValidationContext*. *Wert* bezieht sich auf den tatsächlichen Wert aus dem Feld, das das benutzerdefinierte Validierungssteuerelement überprüft wird.

Im folgenden Beispiel wird eine Geschäftsregel gibt an, dass Benutzer auf die "Genre" nicht festlegen können *klassischen* für einen Film, nach dem 1960 veröffentlicht wird. Die `[ClassicMovie]` Attribut überprüft zuerst, die "Genre", und wenn es sich um einen klassischen handelt, dann überprüft das Veröffentlichungsdatum angezeigt, dass er nach 1960 liegt. Wenn sie nach dem 1960 freigegeben wird, schlägt die Validierung fehl. Das Attribut akzeptiert einen ganzzahligen Parameter, die für das Jahr, das Sie verwenden können, um Daten zu überprüfen. Sie können den Wert des Parameters in der Attributkonstruktor erfassen, wie hier gezeigt:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

Die `movie` Variable oben stellt ein `Movie` Objekt, das die Daten aus der Übermittlung des Formulars überprüft enthält. In diesem Fall überprüft der Validierungscode, das Datum und die "Genre" in der `IsValid` Methode der `ClassicMovieAttribute` Klasse gemäß den Regeln. Bei einer erfolgreichen Validierung `IsValid` gibt eine `ValidationResult.Success` Code, und wenn die Validierung fehlschlägt, eine `ValidationResult` mit einer Fehlermeldung. Wenn ein Benutzer ändert die `Genre` Feld und das Formular übermittelt die `IsValid` Methode der `ClassicMovieAttribute` wird überprüft, ob der Film Klassisches ist. Wie alle integrierten Attribut gelten die `ClassicMovieAttribute` auf eine Eigenschaft, z. B. `ReleaseDate` sicherstellen Überprüfung erfolgt, wie im vorherigen Codebeispiel gezeigt. Da das Beispiel funktioniert, nur mit `Movie` Typen, eine bessere Option ist die Verwendung `IValidatableObject` wie im folgenden Abschnitt gezeigt.

Alternativ konnte dieser denselben Code in das Modell platziert werden, durch die Implementierung der `Validate` Methode für die `IValidatableObject` Schnittstelle. Während Sie eigene benutzerdefinierte Validierungsattribute funktioniert gut für die einzelne Eigenschaften zu überprüfen, implementieren `IValidatableObject` kann verwendet werden, um auf Klassenebene Überprüfung implementieren, wie hier zu sehen.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]

## <a name="client-side-validation"></a>Clientseitige Validierung

Clientseitige Validierung ist eine hervorragende Erleichterung für Benutzer. Es speichert aufgewendete Zeit sie andernfalls würde auf einen Roundtrip zu warten, auf dem Server. Aus geschäftlicher sogar einige wenige Bruchteilen von Sekunden multipliziert unzählige Male pro Tag fügt bis zu viel Zeit und Aufwand Frustration sein. Unkompliziert und sofortige Überprüfung kann Benutzer effizienter arbeiten und erzeugt eine bessere Qualität für ein- und Ausgabe.

Sie benötigen eine Sicht mit der richtigen JavaScript-Skriptverweise im Aufbewahrungsort für die clientseitige Validierung so funktionieren wie hier gezeigt.

[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

MVC verwendet Validierungsattribute zusätzlich zu den Metadaten des Typs von Modelleigenschaften zum Überprüfen von Daten und Anzeigen von Fehlermeldungen, die mit JavaScript. Bei Verwendung von MVC zum Rendern des Form-Elemente aus einem Modell mit [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro) oder [HTML-Hilfsmethoden](xref:mvc/views/overview) fügen sie HTML 5 [Datenattribute](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in der Form-Elemente, für die Validierung, als erforderlich wie unten gezeigt. MVC generiert die `data-` Attribute für integrierte und benutzerdefinierte Attribute. Sie können Validierungsfehler angezeigt, auf dem Client verwenden die relevanten Tags-Hilfsprogrammen, wie hier gezeigt:

[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Die Tag-Hilfsprogramme, die oben genannten Rendern den HTML-Code unten. Beachten Sie, dass die `data-` Attributen im HTML-Ausgabe entsprechen die Validierungsattribute für den `ReleaseDate` Eigenschaft. Die `data-val-required` Attribut unten enthält eine Fehlermeldung angezeigt, wenn der Benutzer nicht in der Version Datumsfeld ausfüllen, und diese Nachricht zeigt an, in dem zugehörigen `<span>` Element.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Die clientseitige Validierung verhindert Übermittlung an, bis das Format ungültig ist. Die Schaltfläche "Absenden" wird ausgeführt, JavaScript, die das Formular übermittelt oder zeigt Fehlermeldungen an.

MVC bestimmt die Typ-Attributwerte, die basierend auf dem .NET Data-Typ einer Eigenschaft, die möglicherweise mit überschrieben `[DataType]` Attribute. Die Basis `[DataType]` Attribut führt keine echte serverseitige Validierung. Browser wählen Sie ihre eigenen Fehlermeldungen und diese Fehler anzeigen, jedoch werden soll, jedoch die Überprüfung unaufdringliche jQuery-Paket überschreiben, die Nachrichten und konsistent mit anderen anzeigen kann. In diesem Fall die meisten offensichtlich, wenn Benutzer anwenden `[DataType]` Unterklassen wie z. B. `[EmailAddress]`.

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Sie können die Client-Side-Logik erstellen, für das benutzerdefinierte Attribut und [unaufdringlichen Überprüfung](http://jqueryvalidation.org/documentation/) wird für Sie automatisch im Rahmen der Überprüfung auf dem Client ausgeführt. Der erste Schritt besteht darin steuern, welche Daten-Attribute hinzugefügt werden, durch die Implementierung der `IClientModelValidator` Schnittstelle wie hier gezeigt:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Attribute, die diese Schnittstelle implementieren, können HTML-Attribute generierte Felder hinzufügen. Untersuchen die Ausgabe für die `ReleaseDate` Element zeigen, HTML, die ähnlich wie im vorherigen Beispiel wird mit der Ausnahme jetzt vorhanden ist eine `data-val-classicmovie` -Attribut, das in definiert wurde. die `AddValidation` Methode `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
data-val="true"
data-val-classicmovie="Classic movies must have a release year earlier than 1960."
data-val-classicmovie-year="1960"
data-val-required="The ReleaseDate field is required."
id="ReleaseDate" name="ReleaseDate" value="" />
```

Unaufdringlichen Überprüfung verwendet die Daten in die `data-` Attribute Fehlermeldungen angezeigt. Allerdings jQuery zu Regeln weiß nicht, oder Nachrichten, bis Sie diese des jQuery hinzufügen `validator` Objekt. Dies wird im folgenden Beispiel, das eine Methode namens addiert gezeigt `classicmovie` , enthält die benutzerdefinierte Überprüfung Clientcode auf die jQuery `validator` Objekt.

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery verfügt jetzt über die Informationen zum Ausführen der benutzerdefinierten JavaScript-Validierung als auch die anzuzeigende Fehlermeldung, wenn diese Validierungscode "false" zurückgibt.

## <a name="remote-validation"></a>Remotevalidierung

Remotevalidierung ist eine großartige Funktion zu verwenden, wenn Sie Daten auf dem Client für die Daten auf dem Server überprüfen müssen. Beispielsweise müssen Ihrer app zu überprüfen, ob eine e-Mail-Adresse oder Benutzer-Name bereits verwendet wird, und sie müssen eine große Menge von Daten dazu Abfragen. Herunterladen großer Datenmengen zum Überprüfen einer legt diese fest, oder einige Felder werden zu viele Ressourcen verbraucht. Es kann auch vertraulichen Informationen verfügbar machen. Eine Alternative besteht darin, von einem Roundtrip Anforderung an ein Feld zu überprüfen.

Sie können in zwei Schritten Remotevalidierung implementieren. Zuerst müssen Sie Ihr Modell mit Anmerkung versehen der `[Remote]` Attribut. Die `[Remote]` Attribut akzeptiert mehrere Überladungen, die zur Weiterleitung von clientseitiger JavaScript an den entsprechenden Code aufrufen können. Das Beispiel verweist auf die `VerifyEmail` Aktionsmethode, die von der `Users` Controller.

[!code-csharp[Main](validation/sample/User.cs?range=5-9)]

Der zweite Schritt ist die Validierungscode in die entsprechende Aktionsmethode einfügen, gemäß der `[Remote]` Attribut. Es gibt eine `JsonResult` , die von die Clientseite können Sie den Vorgang fortzusetzen, oder halten, und ggf. ein Fehler angezeigt.

[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]

Wenn Benutzer eine e-Mail-Nachricht eingeben, wird JavaScript in der Ansicht einen Remoteaufruf, um festzustellen, ob dieser e-Mail angenommen wurde, und wenn dies der Fall ist, wird die Fehlermeldung angezeigt. Andernfalls kann der Benutzer wie gewohnt der Übermittlung des Formulars.
