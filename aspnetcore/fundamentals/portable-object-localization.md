---
title: Konfigurieren Sie portable Objekt Lokalisierung
author: sebastienros
description: "Dieser Artikel führt Portable Objektdateien und erläutert die Schritte zu ihrer Verwendung in einer ASP.NET Core-Anwendung mit dem Framework Core Orchard."
keywords: ASP.NET Core, Lokalisierung, Kultur, Sprache, portable Objekt
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Konfigurieren Sie portable Objekt Lokalisierung Orchard Core

Durch [Sébastien Ros](https://github.com/sebastienros) und [Scott Addie](https://twitter.com/Scott_Addie)

Dieser Artikel führt Sie durch die Schritte für die Verwendung von portablen Objekt (PO)-Dateien in einer ASP.NET Core-Anwendung mit der [Orchard Core](https://github.com/OrchardCMS/OrchardCore) Framework.

**Hinweis:** Orchard Core ist kein Microsoft-Produkt. Microsoft bietet daher keine Unterstützung für diese Funktion an.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Was ist ein PO-Datei?

PO-Dateien werden als Textdateien, die mit den übersetzten Zeichenfolgen für eine bestimmte Sprache verteilt. Einige Vorteile der PO-Dateien vorzuziehen *resx* Includedateien:
- PO-Dateien unterstützen Pluralisierung; *resx* Dateien bieten keine Unterstützung Pluralisierung.
- PO-Dateien werden nicht kompiliert, z. B. *resx* Dateien. Daher sind spezialisierte Tools und die Build-Schritte nicht erforderlich.
- PO-Dateien funktionieren gut mit collaborative online Bearbeitungstools.

### <a name="example"></a>Beispiel

Hier ist eine PO-Beispieldatei, die die Übersetzung für zwei Zeichenfolgen in Französisch, einschließlich eines mit der Pluralform enthält:

*FR.PO*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

In diesem Beispiel wird die folgende Syntax verwendet:

- `#:`: Ein Kommentar, der angibt, des Kontexts der Zeichenfolge, die übersetzt werden. Die gleiche Zeichenfolge möglicherweise unterschiedlich übersetzt werden, je nachdem, wo sie verwendet wird.
- `msgid`: Die nicht übersetzten Zeichenfolge.
- `msgstr`: Die übersetzte Zeichenfolge.

Im Fall von Pluralisierung-Unterstützung können mehrere Einträge definiert werden.

- `msgid_plural`: Die Zeichenfolge, nicht übersetzte im plural.
- `msgstr[0]`: Die übersetzte Zeichenfolge für die Groß-/Kleinschreibung 0.
- `msgstr[N]`: Die übersetzte Zeichenfolge für die Groß-/Kleinschreibung N.

Die PO-Dateispezifikation verwendbaren [hier](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Konfigurieren der Unterstützung der Bestellung in ASP.NET Core

Dieses Beispiel beruht auf einer ASP.NET Core MVC-Anwendung, die aus einer Visual Studio-2017-Projektvorlage generiert.

### <a name="referencing-the-package"></a>Verweisen auf das Paket

Hinzufügen eines Verweises auf die `OrchardCore.Localization.Core` NuGet-Paket. Es steht auf [MyGet](https://www.myget.org/) am folgenden Paketquelle: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

Die *csproj* Datei enthält nun eine Zeile ähnlich der folgenden (die Versionsnummer kann davon abweichen):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Registrierung des Diensts

Fügen Sie die erforderlichen Dienste, die `ConfigureServices` Methode *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Die erforderliche Middleware zum Hinzufügen der `Configure` Methode *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Fügen Sie den folgenden Code, um Ihre Wahl Razor-Ansicht. *About.cshtml* wird in diesem Beispiel verwendet.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

Ein `IViewLocalizer` Instanz eingefügt und zum Übersetzen des Texts "Hello World!" verwendet wird.

### <a name="creating-a-po-file"></a>Erstellen einer bestellungsdatei

Erstellen Sie eine Datei mit dem Namen  *<culture code>per* Stammordner für Ihre Anwendung. In diesem Beispiel wird der Dateiname ist *fr.po* , weil die französische Sprache verwendet wird:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Diese Datei speichert die Zeichenfolge übersetzen und die Zeichenfolge Französisch übersetzt. Übersetzungen, die für ihre übergeordnete Kultur zurückgesetzt werden, wenn erforderlich. In diesem Beispiel wird die *fr.po* Datei wird verwendet, wenn die angeforderte Kultur ist `fr-FR` oder `fr-CA`.

### <a name="testing-the-application"></a>Testen der Anwendung

Führen Sie die Anwendung, und navigieren Sie zu der URL `/Home/About`. Der Text **Hello World!** wird angezeigt.

Navigieren Sie zu der URL `/Home/About?culture=fr-FR`. Der Text **Bonjour le Monde!** wird angezeigt.

## <a name="pluralization"></a>Pluralisierung

PO-Dateien unterstützen Pluralisierung Forms, das ist nützlich, wenn die gleiche Zeichenfolge übersetzt werden Weise basierend auf einer Kardinalität muss. Diese Aufgabe wird durch die Tatsache komplizierter hergestellt, dass jede Sprache benutzerdefinierte Regeln zum Auswählen der definiert zu verwendende Zeichenfolge auf die Kardinalität basieren.

Das Paket Orchard Lokalisierung bietet eine API, um diese anderen Pluralform automatisch aufgerufen.

### <a name="creating-pluralization-po-files"></a>Erstellen Pluralisierung PO-Dateien

Fügen Sie den folgenden Inhalt, der zuvor erwähnten *fr.po* Datei:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Finden Sie unter [was eine PO-Datei ist?](#what-is-a-po-file) eine Erläuterung, was jeder Eintrag in diesem Beispiel dargestellt.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Hinzufügen einer Sprache mit verschiedenen Pluralisierung forms

Englisch und Französisch Zeichenfolgen, die im vorherigen Beispiel verwendet wurden. Englisch und Französisch haben nur zwei Formen der Pluralisierung und teilen die gleichen Regeln für die Form, dass die erste Pluralform eine Kardinalität von eins zugeordnet ist. Alle anderen Kardinalität wird die zweite Pluralform zugeordnet.

Nicht in allen Sprachen gemeinsam die gleichen Regeln. Dies wird mit der Sprache Tschechische veranschaulicht die drei Pluralform hat.

Erstellen der `cs.po` Datei wie folgt und beachten Sie, wie der Pluralisierung drei verschiedene Übersetzungen benötigt:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Für die Aufnahme Tschechische lokalisierten Versionen hinzufügen `"cs"` zur Liste der unterstützten Kulturen in die `ConfigureServices` Methode:

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

Bearbeiten der *Views/Home/About.cshtml* Datei plural lokalisierte Zeichenfolgen für mehrere Kardinalitäten gerendert:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Hinweis:** In einem realen Szenario, eine Variable wird verwendet, um die Anzahl darstellt. Hier wiederholen wir den gleichen Code mit drei verschiedenen Werten, eine sehr speziellen Fall verfügbar zu machen.

Beim Wechsel Kulturen, sehen Sie Folgendes:

Für `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Für `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Für `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Beachten Sie, dass für die Tschechische Kultur, die drei Übersetzungen unterschiedlich sind. Die Kulturen Französisch und Englisch teilen die gleiche Konstruktion für die beiden letzten übersetzten Zeichenfolgen.

## <a name="advanced-tasks"></a>Erweiterte Aufgaben

### <a name="contextualizing-strings"></a>Contextualizing Zeichenfolgen

Anwendungen enthalten häufig die Zeichenfolgen, die an mehreren Stellen übersetzt werden. Dieselbe Zeichenfolge, die möglicherweise einer unterschiedlichen Übersetzung in bestimmte Speicherorte innerhalb einer app (Razor-Ansichten oder Klassendateien). Eine Datei der Bestellung unterstützt das Konzept eines Kontexts Datei, die verwendet werden können, zum Kategorisieren von der Zeichenfolge dargestellt wird. Verwenden einen Kontext, kann eine Zeichenfolge unterschiedlich, je nach Kontext (oder mangelndes ein Kontext) übersetzt werden.

Die Bestellung Lokalisierung Dienste verwenden Sie den Namen der vollständigen Klasse oder die Sicht, die verwendet wird, wenn eine Zeichenfolge zu übersetzen. Dies erfolgt durch Festlegen des Werts auf die `msgctxt` Eintrag.

Betrachten Sie eine kleinere zusätzlich zu den vorherigen *fr.po* Beispiel. Eine Razor-Ansicht finden Sie unter *Views/Home/About.cshtml* kann als Dateikontext definiert werden, durch Festlegen der reservierten `msgctxt` Wert des Eintrags:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Mit der `msgctxt` textübersetzung als solche festgelegt wurde, tritt auf, wenn Navigieren zur `/Home/About?culture=fr-FR`. Die Übersetzung wird nicht ausgeführt, beim Navigieren zur `/Home/Contact?culture=fr-FR`.

Wenn keine bestimmten Eintrag mit einem bestimmten Dateikontext abgeglichen wird, wird eine entsprechende PO-Datei ohne einen Kontext Orchard-Core-fallback-Mechanismus sucht. Vorausgesetzt, dass keine bestimmte Dateikontext für definiert ist *Views/Home/Contact.cshtml*, navigieren Sie zu `/Home/Contact?culture=fr-FR` lädt Sie z. B. eine bestellungsdatei:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Ändern den Speicherort der PO-Dateien

Der standardmäßige Speicherort der PO-Dateien kann geändert werden, `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

In diesem Beispiel die PO-Dateien geladen werden, aus der *Lokalisierung* Ordner.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementieren einen benutzerdefinierten Logik für die Suche nach Lokalisierungsdateien

Wenn komplexer Logik erforderlich ist, um die PO-Dateien, suchen die `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` Schnittstelle implementiert und als Dienst registriert werden kann. Dies ist nützlich, wenn PO-Dateien an unterschiedlichen Speicherorten gespeichert werden können oder die Dateien müssen innerhalb einer Hierarchie von Ordnern gefunden werden.

### <a name="using-a-different-default-pluralized-language"></a>Verwenden einer anderen pluralisiert Standardsprache

Das Paket enthält eine `Plural` Erweiterungsmethode, die für zwei Pluralform spezifisch ist. Erstellen Sie für Sprachen, die weitere Pluralform erfordern eine Erweiterungsmethode. Mit einer Erweiterungsmethode, müssen Sie keine Lokalisierungsdatei für die Standardsprache angeben &mdash; ursprünglichen Zeichenfolgen bereits direkt im Code verfügbar sind.

Sie können den stärker generische `Plural(int count, string[] pluralForms, params object[] arguments)` Überladung, die ein Array von Zeichenfolgen von Übersetzungen akzeptiert.