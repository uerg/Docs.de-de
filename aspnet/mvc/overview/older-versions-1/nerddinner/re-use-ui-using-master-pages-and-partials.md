---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: "Erneute Verwendung Benutzeroberfläche mit Masterseiten und Replikatsgruppenelemente | Microsoft Docs"
author: microsoft
description: Schritt 7 sucht Methoden zum TROCKENEN Prinzip angewendet werden kann in unserer Ansichtsvorlagen Codeduplikaten, mithilfe von Teilansicht Vorlagen und Gestaltungsvorlagen zu vermeiden.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: c42cd6aca40b08a9f8461532fbfd0589901b64ad
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Verwenden Sie die Benutzeroberfläche mit Masterseiten und Replikatsgruppenelemente erneut
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 7 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 7 sucht Methoden zum "TROCKENEN Prinzip" angewendet werden kann in unserer Ansichtsvorlagen Codeduplikaten, mithilfe von Teilansicht Vorlagen und Gestaltungsvorlagen zu vermeiden.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner Schritt 7: Replikatsgruppenelemente und Masterseiten

Keines der Entwurfsphilosophie, die ASP.NET MVC Verwaltungsteam handelt es sich um das "Werden nicht wiederholt selbst"-Prinzip (häufig als "Trocken" bezeichnet). Ein TROCKENEN Design hilft die Duplizierung durch Code und eine Logik, wodurch das letztlich Anwendungen erstellen schneller und einfacher zu verwalten, zu vermeiden.

Wir haben bereits gesehen, TROCKENEN Prinzipals in verschiedenen Szenarios NerdDinner angewendet. Einige Beispiele: Überprüfungslogik wird in unserem der Modellebene über beide bearbeiten erzwungen werden, und erstellen Sie die Szenarien in unser Controller; ermöglicht implementiert. die Ansicht-Vorlage "NotFound" verwenden über die Aktionsmethoden bearbeiten "," Details "und" Delete wir erneut; verwenden wir eine Konvention - Benennungsschema mit unserer Ansichtsvorlagen, die entfällt die Notwendigkeit, den Namen explizit anzugeben, wenn wir die View() Hilfsmethode aufrufen; und wir die DinnerFormViewModel-Klasse für beide bearbeiten erneut verwenden und Aktion Szenarien zu erstellen.

Jetzt betrachten wir weisen das Prinzip"trocken" angewendet werden kann in unserer Ansichtsvorlagen Codeduplikaten es ebenfalls zu vermeiden.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Erneut besuchen Sie unsere bearbeiten und Erstellen von Vorlagen anzeigen

Wir sind derzeit zwei verschiedene Ansichtsvorlagen – "Edit.aspx" und "Create.aspx" – verwenden, um unsere Dinner-Formular-Benutzeroberfläche anzeigen. Ein sofortvergleich visual davon werden hervorgehoben, wie ähneln sie. Im folgenden finden Sie, wie der erstellen-Form aussieht:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Und was unser "Bearbeiten" Formular sieht folgendermaßen aus:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Es ist nicht viel einen Unterschied? Abgesehen von den Titel und Header Text sind die Steuerelemente des Formulars Layout und die Eingabe identisch.

Wenn wir die "Edit.aspx" und "Create.aspx" Ansichtsvorlagen zu öffnen, die wir finden, die enthaltenen identische Formular Layout und die Eingabe Steuerungscode. Diese Duplizierung bedeutet, dass wir müssen Änderungen vornehmen, zweimal jedes Mal, wenn wir einführen, oder ändern Sie eine neue Dinner-Eigenschaft - die nicht in Ordnung ist erschöpft.

### <a name="using-partial-view-templates"></a>Mithilfe von Vorlagen für partielle Ansicht

ASP.NET MVC unterstützt die Möglichkeit, die "partielle Ansicht" Vorlagen zu definieren, die verwendet werden kann, um Ansicht Renderinglogik für einen untergeordneten Teil einer Seite zu kapseln. "Replikatsgruppenelemente" bieten eine gute Möglichkeit Ansicht Renderinglogik einmal definiert, und klicken Sie dann erneut in an mehreren Stellen innerhalb einer Anwendung.

Um "Trocken-nach-oben" unsere Edit.aspx und Create.aspx Ansicht Vorlage Datenduplizierung, können wir erstellen eine Teilansicht-Vorlage, die mit dem Namen "DinnerForm.ascx", der das Formularlayout und Eingabeelemente sowohl kapselt. Wir müssen dazu auf Rechtsklicken auf unserem/Ansichten/Abendessen-Verzeichnis und die "Add -&gt;Ansicht" Menübefehl:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt. Den Namen der neuen Sicht, wir möchten "DinnerForm" zu erstellen, aktivieren Sie das Kontrollkästchen "Erstellen eine Teilansicht" innerhalb des Dialogfelds und anzugeben, dass wir es eine DinnerFormViewModel-Klasse übergeben wird:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Wenn wir die Schaltfläche "Hinzufügen" klicken, erstellt Visual Studio eine neue Vorlage für "DinnerForm.ascx" Ansicht für uns innerhalb des Verzeichnisses "\Views\Dinners".

Wir können Sie dann kopieren und einfügen das doppelten das Formularlayout / Steuerungscode aus unserem Edit.aspx/ Create.aspx Ansichtsvorlagen in unserer neuen "DinnerForm.ascx" Teilansicht Vorlage eingegeben:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Wir können unsere Ansichtsvorlagen bearbeiten und erstellen, zum Aufrufen der partiellen DinnerForm-Vorlage und entfernen Duplikate Formular aktualisieren. Wir können dies durch Aufrufen von Html.RenderPartial("DinnerForm") in unserer Ansichtsvorlagen durchführen:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Sie können den Pfad der partiellen Sie die gewünschte Vorlage beim Aufrufen von Html.RenderPartial explizit qualifizieren (z. B.: ~ Views/Dinners/DinnerForm.ascx "). Im Beispielcode oben jedoch nutzen konventionsbasierte Benennungsmuster in ASP.NET MVC, und wir Angabe "DinnerForm" als Namen für die partielle gerendert. Wenn wir dies tun sucht ASP.NET MVC zuerst im Verzeichnis "Views konventionsbasierte" (für DinnersController dies/Ansichten/Abendessen wäre). Die partielle Vorlage gefunden wird es es dann es im Verzeichnis /Views/Shared gesucht.

Wenn Html.RenderPartial() mit nur der Name der Teilansicht aufgerufen wird, werden an die Teilansicht dieselben Modell- und ViewData Wörterbuchobjekte durch die aufrufende Sicht Vorlage verwendet ASP.NET MVC übergeben. Es gibt auch überladene Versionen der Html.RenderPartial() Übergabe eines alternativen Modellobjekt und/oder ViewData Wörterbuch für die Teilansicht zu verwenden. Dies ist hilfreich für Szenarien, in denen Sie nur eine Teilmenge des vollständigen Modells/ViewModel übergeben möchten.

| **Seite Thema: Warum &lt;%%&gt; anstelle von &lt;% = %&gt;?** |
| --- |
| Eine geringfügige Dinge, die Ihnen möglicherweise mit den oben aufgeführten Code aufgefallen ist, dass wir verwenden eine &lt;%%&gt; anstelle von Sperren einer &lt;% = %&gt; blockieren, wenn Html.RenderPartial() aufrufen. &lt;% = %&gt; Blöcke in ASP.NET anzugeben, dass ein Entwickler möchte einen angegebenen Wert gerendert werden soll (z. B.: &lt;% = "Hello"-%&gt; würde "Hello" Rendern). &lt;%%&gt; Blöcke stattdessen darauf hin, dass der Entwickler kann Code ausführen, dass alle darin enthaltenen Ausgabe gerendert explizit ausgeführt werden muss (z. B.: &lt;Response.Write("Hello") %&gt;. Der Grund, wir verwenden, eine &lt;%%&gt; Block mit Beispielcode Html.RenderPartial liegt daran, dass die Html.RenderPartial()-Methode nicht geben eine Zeichenfolge zurück, und stattdessen gibt der Inhalt direkt an die aufrufende Sicht Vorlage den Ausgabestream. Dies geschieht aus Leistungsgründen Effizienz und dazu, wodurch die Notwendigkeit zum Erstellen eines Objekts (potenziell sehr großen) temporäre Zeichenfolge vermieden werden. Dies reduziert die speicherauslastung und Anwendungsdurchsatz verbessert. Ein häufiger Fehler beim Verwenden von Html.RenderPartial() vergessen besteht, fügen Sie ein Semikolon am Ende des Aufrufs wird innerhalb einer &lt;%%&gt; Block. Beispielsweise verursacht dieser Code einen Compilerfehler: &lt;Html.RenderPartial("DinnerForm") %&gt; müssen Sie stattdessen schreiben: &lt;"" Html.RenderPartial("DinnerForm");&gt; Grund hierfür ist, &lt;%%&gt; Blöcken handelt codeanweisungen eigenständig, und nach der Verwendung von C#-Code Anweisungen müssen mit einem Semikolon beendet werden soll. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Teilansicht Vorlagen verwenden, um zu verdeutlichen, Code

Wir erstellt die Vorlage "DinnerForm" Teilansicht, um zu vermeiden, duplizieren Renderinglogik an mehreren Stellen anzeigen. Dies ist der häufigste Grund für die Teilansicht Vorlagen erstellen.

In einigen Fällen sinnvoll es weiterhin, Teilansichten zu erstellen, selbst wenn sie nur an einer einzelnen Stelle aufgerufen werden. Ziemlich kompliziert Ansichtsvorlagen können häufig einfacher zu lesen, wenn ihre Sicht Renderinglogik extrahiert und in eine partitionierte oder mehr gut mit dem Namen teilweise Vorlagen werden.

Betrachten Sie beispielsweise den folgenden Codeausschnitt aus der Datei Site.master in unserem Projekt (die wir in Kürze angezeigt wird). Der Code ist relativ selbsterklärend lesen, da Logik, um eine Anmeldung/Abmeldung anzeigen link oben – rechten Rand des Bildschirms in der partiellen "LogOnUserControl" gekapselt ist:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Wenn Sie selbst die erste verwechselt finden möchten verstehen Sie das Markup html-Code in einer Vorlage anzeigen, überlegen Sie, ob es wäre klarer, wenn einige davon extrahiert und in gut benannte für Teilansichten umgestaltet.

### <a name="master-pages"></a>Masterseiten

Zusätzlich zur Unterstützung von Teilansichten, unterstützt die ASP.NET MVC auch die Möglichkeit, "Masterseite" Vorlagen erstellen, die verwendet werden kann, um das allgemeine Layout und die von einem Standort der obersten Ebene html zu definieren. Platzhalter können dann hinzugefügt werden, der Masterseite ersetzbare Regionen zu identifizieren, die außer Kraft gesetzt werden können oder die "eingetragen" von Ansichten für Inhalt. Dies ermöglicht eine sehr effektive (und TROCKENEN), ein gebräuchliches Layout für eine Anwendung anzuwenden.

Standardmäßig haben neue ASP.NET MVC-Projekte eine Masterseitenvorlage automatisch hinzugefügt. Diese Masterseite heißt "Site.master" und der konfigurierbarkeit innerhalb des Ordners \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Die Standard-Datei Site.master sieht wie unten. Den äußeren HTML-Code des Standorts, zusammen mit einem Menü für die Navigation auf der obersten definiert. Es enthält zwei ersetzbare Inhaltsplatzhalter-Steuerelemente: eines für den Titel und die andere für, in dem der primäre Inhalt einer Seite ersetzt werden soll:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Alle Vorlagen anzeigen, die wir für unsere NerdDinner-Anwendung ("Liste", "Details", "Bearbeiten", "Erstellen", "NotFound" usw.) erstellt haben müssen auf Site.master Basis dieser Vorlage wurden. Dadurch wird angegeben, über das "MasterPageFile"-Attribut, das im oberen Bereich standardmäßig hinzugefügt wurde &lt;% @ Page %&gt; -Direktive, wenn wir unsere Sichten, die mit dem Dialogfeld "Ansicht hinzufügen" erstellt:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Dies bedeutet, dass den Site.master Inhalt geändert werden kann, und haben die Änderungen automatisch angewendet und werden verwendet, wenn wir unsere Vorlagen Ansicht zu rendern.

Wir aktualisieren unsere Site.master Headerabschnitt, damit der Header der Anwendung "NerdDinner" anstelle von "Meine MVC-Anwendung" ist. Auch aktualisieren wir unsere Navigationsmenü, damit die erste Registerkarte "Suchen eines Dinner" (von der HomeController Index() Aktionsmethode behandelt) ist, und fügen Sie eine neue Registerkarte "Host ein Dinner" (von der DinnersController Create()-Aktionsmethode behandelt):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Wenn wir die Datei Site.master und Aktualisierung speichern Änderungen unsere wir unsere Header sehen Browser anzeigen bis für alle Sichten innerhalb der Anwendung an. Zum Beispiel:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Und mit der */Dinners/bearbeiten / [Id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Nächster Schritt

Replikatsgruppenelemente und Gestaltungsvorlagen bieten sehr flexible Optionen, die Ihnen ermöglichen, die Sichten ordnungsgemäß zu organisieren. Sie werden feststellen, dass sie vermeiden Sie Ansicht duplizieren content / code und stellen Ihre Vorlagen anzeigen einfacher zu lesen und zu verwalten.

Nehmen wir jetzt rufen Sie erneut auf das Angebot-Szenario, das wir zuvor erstellt und skalierbare pagingunterstützung zu aktivieren.

>[!div class="step-by-step"]
[Zurück](use-viewdata-and-implement-viewmodel-classes.md)
[Weiter](implement-efficient-data-paging.md)
