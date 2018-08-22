---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Teil 5: Geschäftslogik | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 5 fügt etwas Geschäftslogik.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823935"
---
<a name="part-5-business-logic"></a>Teil 5: Geschäftslogik
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 5 fügt etwas Geschäftslogik.


## <a id="_Toc260221671"></a>  Hinzufügen von Geschäftslogik

Wir möchten unsere Einkaufserlebnis verfügbar sein, wenn jemand auf unserer Website besucht. Besucher werden suchen und Hinzufügen von Elementen zum Einkaufswagen, auch wenn sie nicht registriert oder angemeldet sind. Wenn sie bereit sind, sehen Sie sich diese erhalten der Möglichkeit, zu authentifizieren, und wenn dies nicht der Fall noch Elemente werden zum Erstellen eines Kontos können.

Dies bedeutet, dass die Logik zum Konvertieren des Einkaufswagens ein Status "Anonym" in einen "registriert" Benutzerzustand implementieren müssen.

Wir erstellen ein Verzeichnis namens "Klassen" und dann mit der rechten Maustaste auf den Ordner und erstellen Sie eine neue "Class"-Datei mit dem Namen MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Wie bereits erwähnt, wir erweitern die Klasse, die Seite "MyShoppingCart.aspx" implementiert, und wir erledigen dies mit. NET leistungsstarke "Partial Class" erstellen.

Der generierte Aufruf für unsere MyShoppingCart.aspx.cf-Datei sieht wie folgt aus.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Beachten Sie die Verwendung des Schlüsselworts "partiell".

Die Klassendatei, die wir gerade erstellt haben, sieht wie folgt aus.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Wir werden unsere Implementierungen zusammen, indem diese Datei auch das partial-Schlüsselwort hinzugefügt.

Unsere neue Klassendatei sieht nun folgendermaßen aus.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Die erste Methode, mit der wir unsere Klasse hinzugefügt wird, ist die Methode "AddItem". Dies ist die Methode, die letztlich auf die Links "Hinzufügen, Kunst" auf den Seiten Product List "und" Produktdetails klickt der Benutzer aufgerufen werden.

Fügen Sie Folgendes an der mit Anweisungen am oberen Rand der Seite.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Ein, und fügen Sie die folgende Methode der MyShoppingCart-Klasse.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Wir werden LINQ to Entities verwenden, um festzustellen, ob das Element bereits im Einkaufswagen ist. Wenn also, wir die Bestellmenge des Elements aktualisieren, erstellen Sie andernfalls einen neuen Eintrag für das ausgewählte Element

Um diese Methode aufrufen, werden wir implementieren eine AddToCart.aspx-Seite, die nicht nur Klasse diese Methode, aber dann angezeigt, die aktuelle eine = Warenkorb, nachdem das Element hinzugefügt wurde.

Mit der rechten Maustaste auf den Namen der Projektmappe im Projektmappen-Explorer und das Hinzufügen und neue Seite, die mit dem Namen AddToCart.aspx, wie wir bereits durchgeführt haben.

Während wir auf dieser Seite anzuzeigenden Zwischenergebnisse wie Niedrig vordefinierten Probleme usw., in unserer Implementierung verwenden könnten, die Seite wird nicht tatsächlich gerendert werden, aber stattdessen rufen Sie die Logik der "Add" und umleiten.

Zu diesem Zweck fügen wir den folgenden Code auf der Seite\_Load-Ereignis.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Beachten Sie, dass wir das Produkt zum Einkaufswagen hinzufügen, aus dem QueryString-Parameter und Aufrufen der Methode "AddItem" unserer Klasse abgerufen werden.

Sofern keine Fehler auftreten erhält die Kontrolle der SHoppingCart.aspx-Seite, wir als Nächstes vollständig implementiert werden. Wenn ein Fehler vorhanden sein soll, wird eine Ausnahme ausgelöst.

Derzeit haben wir noch nicht implementiert einen globalen Fehlerhandler, diese Ausnahme wird von der Anwendung unbehandelt, aber wir werden dies in Kürze beheben.

Beachten Sie auch die Verwendung der Anweisung Debug.Fail() (verfügbar über `using System.Diagnostics;)`

Ist die Anwendung innerhalb des Debuggers ausgeführt wird, diese Methode wird in diesem Dialogfeld ausführliche Informationen über die Anwendungen Zustand zusammen mit der Fehlermeldung, die wir angeben.

Bei Ausführung in der Produktion die Debug.Fail()-Anweisung ignoriert wird.

Sie werden im Code über einen Aufruf einer Methode in unserem shopping Cart-Klassennamen "GetShoppingCartId" feststellen.

Fügen Sie den Code aus, um die Methode wie folgt zu implementieren.

Beachten Sie, dass wir auch hinzugefügt haben, aktualisieren und Auschecken Schaltflächen und eine Bezeichnung, in dem der Warenkorb "Gesamt" angezeigt werden können.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Können wir unsere warenkorbsoftware jetzt Elemente hinzufügen, aber wir haben die Logik zum Warenkorb anzeigen, nachdem ein Produkt hinzugefügt wurde nicht implementiert.

Daher auf der Seite MyShoppingCart.aspx fügen ein EntityDataSource-Steuerelement und ein Steuerelement GridVire wie folgt wir.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Rufen Sie das Formular im Designer, damit Sie einen auf die Schaltfläche mit den Einkaufskorb aktualisieren Doppelklick können, und generieren den Click-Ereignishandler, der in der Deklaration im Markup angegeben ist.

Implementieren wir die Details später aber lassen Sie uns erstellen und Ausführen unserer Anwendung ohne Fehler wird dadurch.

Wenn Sie die Anwendung auszuführen und ein Element zum Einkaufswagen hinzufügen wird dies angezeigt.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Beachten Sie, dass wir aus der "Default" Rasteranzeige Berichtdienste, durch die Implementierung von drei benutzerdefinierter Spalten.

Die erste ist ein Bearbeitbarer, "Gebunden" Feld für die Menge an:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Das nächste ist eine "berechnete"-Spalte, in dem das gesamte Zeilenelement angezeigt (das Element Kosten und die Menge aus, um sortiert zu werden):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Schließlich haben wir eine benutzerdefinierte Spalte, die ein CheckBox-Steuerelement enthält, die der Benutzer verwendet, um anzugeben, dass das Element aus dem Einkaufswagen Diagramm entfernt werden soll.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Wie Sie sehen können, die Reihenfolge, die gesamte Zeile nun leer ist, berechnen Sie den Order Total Logik hinzufügen.

Eine Methode "GetTotal" implementieren wir zuerst unsere MyShoppingCart-Klasse.

Fügen Sie den folgenden Code hinzu, in der Datei MyShoppingCart.cs.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Klicken Sie dann auf der Seite\_Ereignishandler zum Laden wir werden unsere GetTotal-Methode aufrufen können. Zur gleichen Zeit fügen wir einen Test, um festzustellen, ob der Einkaufswagen leer ist, und passen Sie die Anzeige entsprechend, wenn es sich handelt.

Jetzt ist der Einkaufswagen leer erhalten wir dies:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Und falls nicht, sehen unsere insgesamt.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Allerdings ist diese Seite noch nicht abgeschlossen.

Wir benötigen zusätzlichen Logik zum Einkaufswagen ordnungsgemäß neu zu berechnen, durch Entfernen von Elementen, die zum Löschen ausgewählt und durch neue Menge Werte bestimmen, wie einige im Raster vom Benutzer geändert wurde.

Können unsere einkaufswagenklasse in MyShoppingCart.cs, um den Fall abzudecken, wenn ein Benutzer ein Element zum Entfernen markiert eine Methode "RemoveItem" hinzugefügt.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Jetzt sehen wir Ad einer Methode, die Situation zu behandeln, wenn ein Benutzer einfach die Qualität, die in den GridView-Ansicht sortiert werden ändert.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Mit den grundlegenden "Remove" und Update-Features an Stelle können wir die Logik implementieren, die tatsächlich den Einkaufswagen in der Datenbank aktualisiert. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Sie werden feststellen, dass diese Methode zwei Parameter erwartet. Eine dem Warenkorb-Id und das andere ist ein Array von Objekten des benutzerdefinierten Typs.

Um die Abhängigkeit von unserer Logik Einzelheiten der Schnittstelle von Benutzer zu minimieren, haben wir eine Datenstruktur definiert, die wir verwenden können, die Artikel im Warenkorb zu unserem Code übergeben, ohne die Methode, die direkt auf das GridView-Steuerelement zugreifen müssen.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

In unserer MyShoppingCart.aspx.cs-Datei können wir diese Struktur in unserem Update Schaltfläche klicken Sie auf-Ereignishandler wie folgt verwenden. Beachten Sie, dass zusätzlich zum Aktualisieren des Warenkorbs die Warenkorb-Summe ebenfalls aktualisiert werden.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Beachten Sie diese Codezeile, mit besonderem Interesse:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() ist eine spezielle Hilfsfunktion, die wir in MyShoppingCart.aspx.cs wie folgt implementiert wird.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Dadurch wird eine saubere Möglichkeit, den Zugriff auf die Werte der gebundenen Elemente in das GridView-Steuerelement. Da unsere "Element entfernen" CheckBox-Steuerelement nicht gebunden ist, werden wir darauf über die FindControl()-Methode zugreifen.

In dieser Phase des Projekts Entwicklung werden wir zum Implementieren des Auscheckvorgangs bereiten.

Bevor wir dabei verwenden Sie Visual Studio zum Generieren der Mitgliedschaftsdatenbank und Hinzufügen eines Benutzers in das Repository für die Mitgliedschaft.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-4.md)
> [Weiter](tailspin-spyworks-part-6.md)
