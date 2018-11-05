---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Erstellen eines konsistenten Layouts in der ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: Um es zum Erstellen von Webseiten für Ihre Website effizienter zu gestalten, können Sie wiederverwendbare Blöcke von Inhalten (z.B. Kopf- und Fußzeilen) für Ihre Website ein, und Sie c erstellen...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 83aef41c0baaeca6a25e09b4ea797ce9ee963a85
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021546"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Erstellen eines konsistenten Layouts in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel erläutert die Verwendung Layoutseiten auf einer Website für ASP.NET Web Pages (Razor) wiederverwendbare Blöcke von Inhalten (z.B. Kopf- und Fußzeilen) erstellen und ein einheitliches Aussehen für alle Seiten auf der Website erstellen können.
> 
> **Sie lernen Folgendes:** 
> 
> - Vorgehensweise: Erstellen von wiederverwendbaren Codeblöcke Inhalte wie Kopf- und Fußzeilen.
> - Vorgehensweise: Erstellen Sie ein einheitliches Aussehen für alle Seiten auf Ihrer Website mit einem Layout.
> - Wie Daten zur Laufzeit zu einer Layoutseite zu übergeben.
> 
> Dies sind die Funktionen von ASP.NET in diesem Artikel:
> 
> - Die Inhalte der Blöcke, die Dateien sind, die HTML-formatierten Inhalt, die auf mehreren Seiten eingefügt werden.
> - Layoutseiten, die Seiten sind, die HTML-formatierten Inhalt enthalten, die von Seiten auf der Website gemeinsam genutzt werden können.
> - Die `RenderPage`, `RenderBody`, und `RenderSection` -Methoden, die ASP.NET mitzuteilen, wohin Elemente einfügen soll.
> - Die `PageData` Wörterbuch, in dem Sie Daten zwischen Inhaltsblöcke und Layoutseiten gemeinsam nutzen kann.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Informationen zu Layoutseiten

Viele Websites verfügen, Inhalt, der angezeigt wird, auf jeder Seite, wie eine Kopfzeile und Fußzeile oder ein Dialogfeld mit der Benutzer darüber informiert, dass sie angemeldet sind. ASP.NET können Sie eine separate Datei mit dem Content-Block zu erstellen, das Text, Markup und Code, wie eine normale Webseite enthalten kann. Sie können den Inhaltsblock klicken Sie dann auf anderen Seiten auf der Website einfügen, in dem Sie die Informationen angezeigt werden sollen. Auf diese Weise müssen Sie kopieren und fügen den gleichen Inhalt in jede Seite. Erstellen gemeinsamen Inhalte wie folgt vereinfacht auch die Site zu aktualisieren. Wenn Sie den Inhalt ändern müssen, Sie nur eine einzelne Datei aktualisieren können und die Änderungen werden dann überall angezeigt wurde der Inhalt eingefügt.

Das folgende Diagramm zeigt, wie Inhalt Arbeit blockiert wird. Wenn ein Browser eine Seite vom Server anfordert, fügt ASP.NET die Inhaltsblöcke, an dem Punkt, in denen die `RenderPage` Methode wird auf der Hauptseite aufgerufen. Fertig gestellten (zusammengeführte) Seite wird dann an den Browser gesendet werden.

![Konzeptionelles Diagramm zeigt, wie die RenderPage-Methode eine Seite auf die verwiesen wird in der aktuellen Seite eingefügt.](3-creating-a-consistent-look/_static/image1.jpg)

In diesem Verfahren erstellen Sie eine Seite, die zwei Inhaltsblöcke (eine Kopfzeile und eine Fußzeile) verweist, die in separaten Dateien gespeichert sind. Sie können die gleichen Inhalte Blöcke in einer beliebigen Seite auf Ihrer Website verwenden. Wenn Sie fertig sind, erhalten Sie eine Seite wie folgt:

![Screenshot der Seite im Browser, der sich ergibt, die von der Ausführung einer Seite, die Aufrufe der RenderPage-Methode enthält.](3-creating-a-consistent-look/_static/image2.jpg)

1. Erstellen Sie im Stammordner der Website, eine Datei namens *"Index.cshtml"*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Erstellen Sie in den Stammordner einen Ordner namens *Shared*.

    > [!NOTE]
    > Es ist üblich, dass zum Speichern von Dateien, die für Webseiten in einem Ordner namens freigegeben werden *Shared*.
4. In der *Shared* Ordner eine Datei namens  *\_Header.cshtml*.
5. Ersetzen Sie jeglichen vorhandenen Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Beachten Sie, dass der Dateiname  *\_Header.cshtml*, mit einem Unterstrich (\_) als Präfix. ASP.NET wird nicht an den Browser eine Seite gesendet, wenn der Name mit einem Unterstrich beginnt. Dadurch wird verhindert, dass Personen anfordern (versehentlich oder anderweitig) auf diesen Seiten direkt. Es ist eine gute Idee, einen Unterstrich zu Seiten, die Namen, die Inhaltsblöcke, haben Benutzer zum Anfordern dieser Seiten können nicht wirklich möchten, dass &#8212; ihr Zweck besteht ausschließlich in den anderen Seiten eingefügt werden soll.
6. In der *Shared* Ordner eine Datei namens  *\_Footer.cshtml* und Ersetzen Sie den Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. In der *"Index.cshtml"* Seite, fügen Sie die beiden Aufrufe von der `RenderPage` Methode, wie hier gezeigt:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Veranschaulicht einen Content-Block in eine Webseite einzufügen. Rufen Sie die `RenderPage` Methode und übergeben sie den Namen der Datei, deren Inhalt an dieser Stelle eingefügt werden soll. Sie sind hier einfügen der Inhalte der  *\_Header.cshtml* und  *\_Footer.cshtml* Dateien in die *"Index.cshtml"* Datei.
8. Führen Sie die *"Index.cshtml"* Seite in einem Browser. (In WebMatrix, in der **Dateien** Arbeitsbereich mit der rechten Maustaste in der das, und wählen Sie dann **in Browser starten**.)
9. Zeigen Sie den Quellcode der Seite im Browser. (Z. B. in Internet Explorer Maustaste auf die Seite, und klicken Sie dann auf **Quelltext anzeigen**.)

    Dadurch können Sie das Markup einer Webseite anzuzeigen, das an den Browser gesendet wird, die im Markup der Seite Index mit die Inhaltsblöcke kombiniert. Das folgende Beispiel zeigt den Quellcode der Seite, die für die gerendert wird *"Index.cshtml"*. Die Aufrufe an `RenderPage` , die Sie eingefügt *"Index.cshtml"* mit den eigentlichen Inhalt der Kopf- und Fußzeile Dateien ersetzt wurden.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Erstellen einer konsistenten Gestaltung, die mithilfe von Layoutseiten

Bisher haben Sie gelernt, dass es einfach, den gleichen Inhalt auf mehreren Seiten enthalten ist. Ein strukturierter Ansatz zum Erstellen eines einheitliches Aussehens für eine Website ist die Verwendung von Layoutseiten. Layoutseite definiert die Struktur einer Webseite, aber keine eigentlichen Inhalt enthalten. Nachdem Sie eine Layoutseite erstellt haben, können Sie Webseiten erstellen, die den Inhalt enthalten, und verknüpfen Sie sie auf der Seite "Layout". Wenn diese Seiten angezeigt werden, werden sie entsprechend der Layoutseite formatiert werden. (In diesem Sinn verhält sich eine Layoutseite als eine Art Vorlage für Inhalte, die in anderen Seiten definiert ist.)

Auf der Layoutseite ist wie bei jeder HTML-Seite, außer dass es sich um einen Aufruf enthält das `RenderBody` Methode. Die Position der `RenderBody` -Methode in der Layoutseite bestimmt, in dem die Informationen auf der Seite Inhalt eingeschlossen werden.

Das folgende Diagramm zeigt, wie Inhaltsseiten und Layoutseiten zur Laufzeit zum Erzeugen von fertigen Webseite kombiniert werden. Der Browser fordert eine Inhaltsseite. Die Inhaltsseite enthält Code, der angibt, der Layoutseite für die Seite in der Struktur verwenden. Klicken Sie auf der Seite Layout der Inhalt eingefügt, an dem Punkt, in denen die `RenderBody` Methode wird aufgerufen. Content-Blöcke können auch auf der Layoutseite eingefügt werden, durch Aufrufen der `RenderPage` -Methode, die wie im vorherigen Abschnitt. Wenn die Webseite abgeschlossen ist, wird es an den Browser gesendet.

![Screenshot der Seite im Browser, der sich ergibt, die von der Ausführung einer Seite, die Aufrufe der Methode RenderBody enthält.](3-creating-a-consistent-look/_static/image3.jpg)

Das folgende Verfahren veranschaulicht das Erstellen eines Layouts und einen Hyperlink Inhaltsseiten.

1. In der *Shared* Ordner der Website, erstellen Sie eine Datei, die mit dem Namen  *\_Layout1.cshtml*.
2. Ersetzen Sie jeglichen vorhandenen Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Sie verwenden die `RenderPage` -Methode in einer Layoutseite Inhaltsblöcke eingefügt. Layoutseite darf nur ein Aufruf an die `RenderBody` Methode.
3. In der *Shared* Ordner eine Datei namens  *\_Header2.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Klicken Sie im Stammordner, erstellen Sie einen neuen Ordner, und nennen sie *Stile*.
5. In der *Stile* Ordner eine Datei namens *"Site.CSS"* und fügen Sie die folgenden Definitionen hinzu:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Diese Definitionen sind hier nur zeigen, wie die Stylesheets mit Layoutseiten verwendet werden können. Wenn Sie möchten, können Sie Ihre eigenen Formatvorlagen für diese Elemente definieren.
6. Erstellen Sie eine Datei namens im Stammordner, *Content1.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Dies ist eine Seite, die eine Layoutseite verwendet werden. Der Codeblock am oberen Rand der Seite gibt an, welche Layoutseite verwenden, um diesen Inhalt zu formatieren.
7. Führen Sie *Content1.cshtml* in einem Browser. Die gerenderte Seite verwendet das Format und das Stylesheet definiert wird,  *\_Layout1.cshtml* und der Text (Inhalt), die in definierten *Content1.cshtml*.

    ![[Image]](3-creating-a-consistent-look/_static/image4.jpg)

    Wiederholen Sie Schritt 6, um zusätzliche Inhaltsseiten zu erstellen, die klicken Sie dann die gleichen Layoutseite freigeben können.

    > [!NOTE]
    > Sie können Ihre Website einrichten, damit die gleichen Layoutseite automatisch für alle Inhaltsseiten in einem Ordner verwendet werden können. Weitere Informationen finden Sie unter [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Entwerfen von Layoutseiten, die mehrere Inhaltsabschnitte

Eine Inhaltsseite kann sich jeweils mehrere Abschnitte, das ist nützlich, wenn Sie Layouts verwenden, die mehrere Bereiche mit ersetzbaren Inhalt haben möchten. Auf der Inhaltsseite Geben Sie jedem Abschnitt einen eindeutigen Namen zu. (Der Abschnitt "Default" bleibt unbenannte.) In der Layoutseite, fügen Sie eine `RenderBody` Methode, um anzugeben, in dem Abschnitt unbenannte (Standard) angezeigt werden soll. Sie fügen Sie dann separate `RenderSection` Methoden, um benannte Abschnitte einzeln zu rendern.

Das folgende Diagramm zeigt, wie ASP.NET verarbeitet, die in mehrere Abschnitte unterteilt ist. Jeder benannten Abschnitt befindet sich in einem Abschnittsblock in der Seite Inhalt. (sie sind es benannte `Header` und `List` im Beispiel.) Das Framework Fügt Abschnitt mit dem Inhalt in der Layoutseite, an dem Punkt, in denen die `RenderSection` Methode wird aufgerufen. Abschnitt unbenannte (Standard) wird an der Stelle eingefügt, in denen die `RenderBody` Methode wird aufgerufen, wie Sie gesehen haben.

![Konzeptionelles Diagramm zeigt, wie die Methode RenderSection Verweise Abschnitte in der aktuellen Seite einfügt.](3-creating-a-consistent-look/_static/image5.jpg)

Diese Prozedur zeigt, wie eine Inhaltsseite erstellen, die mehrere Inhaltsabschnitte und wie sie mithilfe einer Layoutseite aus, die mehrere Inhaltsabschnitte unterstützt gerendert.

1. In der *Shared* Ordner eine Datei namens  *\_Layout2.cshtml*.
2. Ersetzen Sie jeglichen vorhandenen Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Sie verwenden die `RenderSection` Methode, um die Kopf-und die Liste zu rendern.
3. Erstellen Sie eine Datei namens im Stammordner, *Content2.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Diese Seite enthält einen Codeblock am oberen Rand der Seite. Jeder benannten Abschnitt befindet sich in einem Abschnittsblock. Der Rest der Seite enthält der Abschnitt zum Inhalt des standardmäßige (unbenannte).
4. Führen Sie *Content2.cshtml* in einem Browser.

    ![Screenshot der Seite im Browser, der sich ergibt, die von der Ausführung einer Seite, die Aufrufe der Methode RenderSection enthält.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Inhaltsabschnitte machen Optional

Normalerweise müssen die Abschnitte, die Sie in einer Inhaltsseite erstellen Abschnitte entsprechen, die auf der Layoutseite definiert sind. Sie können Fehler auftreten, wenn eines der folgenden auftreten:

- Er enthält einen Abschnitt, der keine entsprechenden Abschnitt auf der Layoutseite hat.
- Die Seite "Layout" enthält einen Abschnitt für die kein Inhalt vorhanden ist.
- Auf der Layoutseite enthält Methodenaufrufe, die versuchen, denselben Bereich mehr als einmal gerendert.

Jedoch können Sie dieses Verhalten für einen benannten Abschnitt überschreiben, indem Sie deklarieren im Abschnitt der Layoutseite optional sein. Dadurch können Sie mehrere Inhaltsseiten, die eine Layoutseite gemeinsam nutzen können, jedoch kann oder möglicherweise keinen Inhalt für einen bestimmten Bereich, zu definieren.

1. Open *Content2.cshtml* und entfernen Sie den folgenden Abschnitt:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Speichern Sie die Seite, und führen Sie es in einem Browser. Eine Fehlermeldung wird angezeigt, da die Inhaltsseite Inhalt für einen Abschnitt definiert, die auf der Layoutseite, nämlich den Headerbereich bereitzustellen nicht.

    ![Screenshot, der der Fehler angezeigt, der auftritt wird, wenn Sie eine Seite ausführen, ruft RenderSection-Methode im entsprechende Abschnitt wird jedoch nicht bereitgestellt.](3-creating-a-consistent-look/_static/image7.jpg)
3. In der *Shared* Ordner die  *\_Layout2.cshtml* Seite, und Ersetzen Sie diese Zeile:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    mit dem folgenden Code:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Als Alternative konnte die vorherige Codezeile mit den folgenden Codeblock, ersetzen Sie die gleichen Ergebnisse erzeugt.

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Führen Sie die *Content2.cshtml* Seite in einem Browser erneut. (Wenn Sie weiterhin auf dieser Seite im Browser geöffnet haben, können Sie nur diese aktualisieren.) Dieses Mal wird die Seite keine Fehler angezeigt werden, obwohl es keinen Header besitzt.

## <a name="passing-data-to-layout-pages"></a>Übergeben von Daten an Layoutseiten

Sie müssen möglicherweise Daten, die auf der Inhaltsseite, die Sie zum Verweisen benötigen auf in einer Layoutseite definiert. Wenn dies der Fall ist, müssen Sie die Daten auf der Seite Inhalt auf der Seite "Layout" übergeben. Beispielsweise empfiehlt es sich um den Status der Anmeldung eines Benutzers anzuzeigen, oder Sie ein- oder Ausblenden von Bereichen auf Grundlage der Benutzereingabe möchten.

Um Daten von einer Inhaltsseite zu einer Layoutseite übergeben möchten, können Sie legen Sie Werte in der `PageData` Eigenschaft der Inhaltsseite. Die `PageData` -Eigenschaft ist eine Auflistung von Name/Wert-Paaren, die die Daten enthalten, die zwischen den Seiten übergeben werden sollen. Klicken Sie auf der Seite Layout anschließend können Sie lesen Werte aus der `PageData` Eigenschaft.

Hier ist ein anderes Diagramm ein. Veranschaulicht, wie ASP.NET verwenden können, die `PageData` Eigenschaft, um die Werte von einer Inhaltsseite auf die Seite "Layout" übergeben. Wenn ASP.NET die Webseite erstellen beginnt, erstellt es die `PageData` Auflistung. In der Inhaltsseite, Schreiben Sie Code aus, um Daten zu speichern, der `PageData` Auflistung. Werte in der `PageData` -Auflistung auch von anderen Abschnitten der Seite Inhalt oder zusätzliche Inhaltsblöcke zugegriffen werden kann.

![Konzeptionelles Diagramm, das zeigt, wie eine Inhaltsseite ein Wörterbuch PageData Auffüllen und diese Informationen an die Layoutseite übergeben kann.](3-creating-a-consistent-look/_static/image8.jpg)

Das folgende Verfahren zeigt, wie Daten von einer Inhaltsseite zu einer Layoutseite übergeben wird. Wenn die Seite ausgeführt wird, wird eine Schaltfläche, die können den Benutzer ausblenden oder Anzeigen einer Liste, die auf der Layoutseite definiert ist. Wenn Benutzer auf die Schaltfläche klicken, wird einen Wert True oder False (boolescher Wert) in der `PageData` Eigenschaft. Auf der Layoutseite liest diesen Wert, und wenn er "false", blendet Sie aus der Liste. Der Wert wird auch in der Seite Inhalt verwendet, zu bestimmen, ob zum Anzeigen der **Liste ausblenden** Schaltfläche oder die **Liste anzeigen** Schaltfläche.

![[Image]](3-creating-a-consistent-look/_static/image9.jpg)

1. Erstellen Sie eine Datei namens im Stammordner, *Content3.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Der Code speichert zwei Datenelemente in der `PageData` Eigenschaft &#8212; den Titel der Webseite und "true" oder "false", um anzugeben, ob eine Liste anzuzeigen.

    Beachten Sie, dass ASP.NET Sie die HTML-Markup in die Seite bedingt mit einem Codeblock eingefügt können. Z. B. die `if/else` Block im Hauptteil der Seite bestimmt die Form anzuzeigende je nachdem, ob `PageData["ShowList"]` wird festgelegt auf "true".
2. In der *Shared* Ordner eine Datei namens  *\_Layout3.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Die Seite "Layout" enthält einen Ausdruck in der `<title>` -Element, das der Titel Wert aus der `PageData` Eigenschaft. Darüber hinaus verwendet er die `ShowList` Wert, der die `PageData` Eigenschaft, um zu ermitteln, ob der Inhaltsblock Liste anzuzeigen.
3. In der *Shared* Ordner eine Datei namens  *\_List.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Führen Sie die *Content3.cshtml* Seite in einem Browser. Die Seite wird angezeigt, mit der Liste auf der linken Seite der Seite angezeigt und eine **Liste ausblenden** unten auf die Schaltfläche.

    ![Screenshot der Seite, die die Liste und eine Schaltfläche mit dem Text "Ausblenden List" enthält.](3-creating-a-consistent-look/_static/image10.jpg)
5. Klicken Sie auf **Liste**. Die Liste wird ausgeblendet, und die Schaltfläche **Liste anzeigen**.

    ![Screenshot der Seite, die die Liste und eine Schaltfläche mit dem Text "Anzeigen der Liste" nicht enthalten ist.](3-creating-a-consistent-look/_static/image11.jpg)
6. Klicken Sie auf die **Liste anzeigen** Schaltfläche und die Liste wird erneut angezeigt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Standortweite verhaltensanpassung für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
