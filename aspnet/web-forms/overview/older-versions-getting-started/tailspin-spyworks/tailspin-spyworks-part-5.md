---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Teil 5: Geschäftslogik | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 5 Fügt einer Geschäftslogik.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-5-business-logic"></a>Teil 5: Geschäftslogik
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 5 Fügt einer Geschäftslogik.


## <a id="_Toc260221671"></a>  Hinzufügen einer Geschäftslogik

Wir möchten unsere Warenkorb Erfahrungen verfügbar werden, wenn jemand auf unserer Website besucht. Besucher werden in der Lage zu durchsuchen und dem Warenkorb Elemente hinzufügen, selbst wenn sie nicht registriert oder angemeldet sind. Beim Auschecken bereitstehen sie erhält die Option zum Authentifizieren und wenn sie nicht sind noch Elemente werden zum Erstellen eines Kontos können.

Dies bedeutet, dass wir die Logik zum Konvertieren von des Einkaufswagen Sinn macht aus einem anonymen Status in "registriert" Benutzerzustand implementieren müssen.

Wir erstellen ein Verzeichnis namens "Klassen" und dann mit der rechten Maustaste auf den Ordner und erstellen Sie eine neue "Class"-Datei mit dem Namen MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Wie bereits erwähnt, wir erweitern die Klasse, die Seite "MyShoppingCart.aspx" implementiert, und wir erledigen dies mit. NET leistungsstarke "Teilklasse" erstellen.

Der generierte Aufruf unsere MyShoppingCart.aspx.cf Datei sieht wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Beachten Sie die Verwendung des Schlüsselworts "partiell" aus.

Die Klassendatei, die wir gerade generiert wurde, sieht wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Wir werden unsere Implementierungen zusammenführen, indem Sie diese Datei als auch die partial-Schlüsselwort hinzugefügt.

Unsere neue Klassendatei sieht nun wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Die erste Methode, die wir unsere Klasse hinzufügen, ist die Methode "AddItem". Dies ist die Methode, die letztlich auf die Links "Auf Grafiken hinzufügen" auf den Seiten Produktliste und Produktdetails klickt der Benutzer aufgerufen werden.

Fügen Sie die folgenden mit Anweisungen am oberen Rand der Seite.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Und fügen Sie diese Methode der MyShoppingCart-Klasse.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Wir werden LINQ to Entities verwenden, um festzustellen, ob das Element bereits im Einkaufswagen ist. Wenn wir also die Bestellmenge des Elements aktualisieren, erstellen Sie andernfalls einen neuen Eintrag für das ausgewählte Element

Um diese Methode aufrufen, implementieren wir eine AddToCart.aspx-Seite, die nicht nur Klasse diese Methode, aber dann angezeigt, die aktuelle eine = Einkaufswagen, nachdem das Element hinzugefügt wurde.

Mit der rechten Maustaste auf den Namen der Projektmappe im Projektmappen-Explorer und das Hinzufügen und neue Seite mit dem Namen AddToCart.aspx, wie wir bereits durchgeführt haben.

Während wir dieser Seite anzuzeigenden Zwischenergebnisse: Niedrig stock Probleme usw., in unserem Implementierung verwenden könnten, wird die Seite nicht tatsächlich gerendert werden, aber stattdessen rufen Sie die Logik "Hinzufügen" und geleitet.

Hierzu fügen wir den folgenden Code zur Seite\_Load-Ereignis.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Beachten Sie, dass wir das Produkt aus einer QueryString-Parameter und Aufrufen der AddItem-Methode der Klasse zum Einkaufswagen hinzufügen abrufen.

Sofern keine Fehler auftreten erhält die Kontrolle der SHoppingCart.aspx-Seite, wir als Nächstes vollständig implementiert werden. Wenn ein Fehler wird eine Ausnahme ausgelöst werden soll.

Derzeit haben wir noch nicht implementiert einen globalen Fehlerhandler damit würde diese Ausnahme nicht durch die Anwendung wechseln, aber wir dies in Kürze behoben werden.

Beachten Sie auch die Verwendung der Anweisung Debug.Fail() (verfügbar über `using System.Diagnostics;)`

Ist die Anwendung innerhalb des Debuggers ausgeführt wird, diese Methode zeigt eine detaillierte Dialogfeld mit Informationen zu den Anwendungsstatus zusammen mit der Fehlermeldung, die wir an.

Wenn die Ausführung in der Produktion Debug.Fail()-Anweisung ignoriert wird.

Beachten Sie im Code über einen Aufruf an eine Methode in unserer shopping Cart-Klassennamen "GetShoppingCartId".

Fügen Sie den Code, um die Methode wie folgt zu implementieren.

Beachten Sie, dass wir auch hinzugefügt haben, aktualisieren und Auschecken Schaltflächen und eine Bezeichnung, in dem Einkaufswagen "insgesamt" angezeigt werden können.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Wir können unsere warenkorbsoftware nun Elemente hinzufügen, aber wir haben die Logik zum Einkaufswagen angezeigt, nachdem ein Produkt hinzugefügt wurde nicht implementiert.

Daher auf der Seite MyShoppingCart.aspx fügen EntityDataSource-Steuerelement und ein Steuerelement GridVire wie folgt wir.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Rufen Sie das Formular im Designer, damit Sie können einen auf die Schaltfläche mit den Einkaufskorb aktualisieren Doppelklick und generieren den Click-Ereignishandler, der in der Deklaration im Markup angegeben ist.

Implementieren wir die Details später jedoch auf diese Weise lassen Sie uns erstellt und die Anwendung ohne Fehler ausgeführt.

Wenn Sie die Anwendung auszuführen und ein Element zum Einkaufswagen hinzufügen wird dies angezeigt.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Beachten Sie, dass wir durch die Implementierung von drei benutzerdefinierter Spalten aus der "Default" Rasteranzeige abwichen haben.

Die erste ist ein bearbeitet werden, die für die Menge "Gebunden" Feld:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Das nächste ist eine "berechnete" Spalte, in dem das gesamte Zeilenelement angezeigt (das Element Kosten und die Menge aus, um sortiert zu werden):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Schließlich haben wir eine benutzerdefinierte Spalte, die ein CheckBox-Steuerelement enthält, die der Benutzer verwendet, um anzugeben, dass das Element aus dem Einkaufswagen Diagramm entfernt werden soll.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Wie Sie sehen können, die Reihenfolge, die gesamte Zeile wir also leer ist Logik zum Berechnen der Order Total hinzufügen.

Eine Methode "GetTotal" implementieren wir zuerst unsere MyShoppingCart-Klasse.

Fügen Sie in der Datei MyShoppingCart.cs den folgenden Code ein.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Klicken Sie dann auf der Seite\_Load-ereignishandlers wir werden unsere GetTotal-Methode aufrufen können. Zur gleichen Zeit fügen wir einen Test, um festzustellen, ob der Einkaufswagen leer ist, und die Anzeige entsprechend anpassen, wenn es sich handelt.

Jetzt ist der Einkaufswagen leer erhalten wir dies:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Und falls nicht, siehe wir unsere insgesamt.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Allerdings ist diese Seite noch nicht abgeschlossen.

Wir benötigen zusätzliche Logik, neu zu berechnen den Einkaufswagen Sinn macht, durch das Entfernen von Elementen, die zum Löschen ausgewählt und durch neue Menge Werte bestimmen, wie einige im Raster vom Benutzer geändert wurde.

Ermöglicht es, fügen Sie eine "RemoveItem"-Methode, um unsere einkaufswagenklasse in MyShoppingCart.cs, um den Fall abzudecken, wenn ein Benutzer ein Element zum Entfernen markiert.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Jetzt sehen wir Ad eine aufzurufende Methode die Situation behandelt wird, wenn ein Benutzer einfach ändert sich die Qualität der GridView Reihenfolge aufweisen.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Mit den grundlegenden entfernen und Update-Features an Stelle können wir die Logik implementieren, die tatsächlich den Einkaufswagen Sinn macht, in der Datenbank aktualisiert. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Sie werden feststellen, dass diese Methode zwei Parameter erwartet. Eine dem Warenkorb-Id und das andere ist ein Array von Objekten des benutzerdefinierten Typs.

Um die Abhängigkeit der unsere Logik Benutzer Schnittstelle Einzelheiten zu minimieren, haben wir eine Datenstruktur definiert, mit denen wir stattgefunden hat die shopping Cart-Elemente zum getesteten Codes unsere Methode des GridView-Steuerelements direkt zugreifen müssen.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

In unsere Datei MyShoppingCart.aspx.cs können wir diese Struktur in unserem Update Schaltfläche klicken-Ereignishandler wie folgt verwenden. Beachten Sie, zusätzlich zum Aktualisieren der Einkaufswagen den Einkaufswagen insgesamt sowie aktualisiert wird.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Beachten Sie bei Interesse diese Codezeile aus:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() ist eine spezielle Hilfsfunktion, die wie folgt in MyShoppingCart.aspx.cs implementiert wird.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Dies stellt eine saubere Möglichkeit, den Zugriff auf die Werte der gebundenen Elemente in unserer GridView-Steuerelements bereit. Da unsere "Element entfernen" CheckBox-Steuerelement nicht gebunden ist müssen wir darauf zugreifen, über die FindControl()-Methode.

Zu diesem Zeitpunkt in der Entwicklung des Projekts erhalten wir zum Implementieren des Auscheckvorgangs bereit.

Bevor wir dabei verwenden Sie Visual Studio zum Generieren der Mitgliedschaftsdatenbank und Hinzufügen eines Benutzers im Repository der Mitgliedschaft.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-4.md)
> [Weiter](tailspin-spyworks-part-6.md)
