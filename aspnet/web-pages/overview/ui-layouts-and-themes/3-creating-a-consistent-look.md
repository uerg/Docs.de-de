---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Erstellen ein konsistentes Layout in der ASP.NET Web Pages (Razor)-Websites | Microsoft Docs
author: tfitzmac
description: "Um die zum Erstellen von Webseiten für Ihre Website effizienter gestalten, können Sie wiederverwendbare Blöcke von Inhalten (z. B. Kopf- und Fußzeilen) für die Website, und Sie c erstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Erstellen ein konsistentes Layout in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel erläutert die Verwendung Layoutseiten auf einer Website für ASP.NET Web Pages (Razor) wiederverwendbare Blöcke von Inhalten (z. B. Kopf- und Fußzeilen) erstellen und ein konsistentes Erscheinungsbild für alle Seiten auf der Website erstellen können.
> 
> **Lernen Sie:** 
> 
> - Vorgehensweise: Erstellen von wiederverwendbaren Blöcke von Inhalten, z. B. Kopf- und Fußzeilen.
> - Vorgehensweise: Erstellen Sie ein konsistentes Erscheinungsbild für alle Seiten auf Ihrer Website mithilfe eines Layouts dargestellt.
> - Wie Daten zur Laufzeit zu einer Layoutseite übergeben.
> 
> Dies sind die Funktionen von ASP.NET im Artikel:
> 
> - Inhaltsblöcke, Dateien, die HTML-formatierten Inhalt auf mehreren Seiten einzufügenden enthalten.
> - Layoutseiten Seiten, die HTML-formatierten Inhalt enthalten, die von Seiten auf der Website gemeinsam genutzt werden können.
> - Die `RenderPage`, `RenderBody`, und `RenderSection` Methoden, die ASP.NET anweisen, wo Sie die Seitenelemente eingefügt werden soll.
> - Die `PageData` Wörterbuch, das Sie Daten zwischen Inhaltsblöcke und Layoutseiten gemeinsam nutzen kann.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Informationen zu Layoutseiten

Viele Websites weisen Inhalte, die auf jeder Seite, z. B. eine Kopfzeile und Fußzeile oder ein Dialogfeld mit der Benutzer informiert, dass sie, in angemeldet sind der angezeigt wird. ASP.NET ermöglicht die Erstellung eine separate Datei mit einem Content-Block, der Text, Markup und Code, wie eine normale Webseite enthalten kann. Sie können den Content-Block klicken Sie dann auf anderen Seiten auf der Website einfügen, in dem die Informationen angezeigt werden sollen. Auf diese Weise müssen Sie kopieren und fügen den gleichen Inhalt in jeder Seite. Gemeinsamen Inhalte wie folgt erstellen erleichtert auch die Site zu aktualisieren. Wenn Sie den Inhalt ändern müssen, Sie nur eine einzelne Datei aktualisieren können und die Änderungen werden dann überall angezeigt wurde der Inhalt eingefügt.

Das folgende Diagramm zeigt, wie Inhalt Arbeit blockiert wird. Wenn ein Browser vom Webserver eine Seite anfordert, fügt ASP.NET die Inhaltsblöcke an dem Punkt, in dem der `RenderPage` wird auf der Hauptseite aufgerufen. Die fertig gestellten (zusammengeführte) Seite wird dann an den Browser gesendet.

![Konzeptionelles Diagramm zeigt, wie die RenderPage-Methode eine Seite auf die verwiesen wird in der aktuellen Seite eingefügt.](3-creating-a-consistent-look/_static/image1.jpg)

In diesem Verfahren erstellen Sie eine Seite, die zwei Inhaltsblöcke (eine Kopf- und Fußzeilen) verweist, die in separaten Dateien gespeichert sind. Sie können diese gleichen Inhaltsblöcke in eine andere Seite in Ihrer Website verwenden. Wenn Sie fertig sind, erhalten Sie eine Seite wie folgt:

![Screenshot der Seite im Browser, der sich ergibt, die von der Ausführung einer Seite, die Aufrufe der Methode RenderPage enthält.](3-creating-a-consistent-look/_static/image2.jpg)

1. Erstellen Sie im Stammordner der Website, eine Datei namens *Index.cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Erstellen Sie im Stammordner, einen Ordner namens *Shared*.

    > [!NOTE]
    > Es ist üblich, die zum Speichern von Dateien, die für Webseiten in einem Ordner namens freigegeben sind *Shared*.
4. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Header.cshtml*.
5. Ersetzen Sie jeglichen vorhandenen Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Beachten Sie, dass der Dateiname  *\_Header.cshtml*, mit einem Unterstrich (\_) als Präfix. ASP.NET wird keine Seite an den Browser gesendet werden, wenn dessen Name mit einem Unterstrich beginnt. Dadurch wird verhindert, dass Personen anfordern (versehentlich oder auf andere Weise) diese Seiten direkt. Es ist ratsam, einen Unterstrich Name-Seiten, die Inhaltsblöcke, enthalten verwendet werden, da Sie nicht wirklich Benutzer können auf diesen Seiten &#8212;anfordern möchten. Sie sind ausschließlich vorhanden in anderen Seiten eingefügt werden.
6. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Footer.cshtml* und Ersetzen Sie den Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. In der *Index.cshtml* Seite, fügen Sie die beiden Aufrufe von der `RenderPage` Methode, wie hier gezeigt:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Dies zeigt, wie einen Content-Block in eine Webseite einzufügen. Rufen Sie die `RenderPage` Methode und übergeben den Namen der Datei, deren Inhalt an diesem Punkt eingefügt werden soll. Hier fügen Sie den Inhalt der  *\_Header.cshtml* und  *\_Footer.cshtml* Dateien in der *Index.cshtml* Datei.
8. Führen Sie die *Index.cshtml* Seite in einem Browser. (In WebMatrix, in der **Dateien** Arbeitsbereich mit der rechten Maustaste in der das, und wählen Sie dann **in Browser starten**.)
9. Zeigen Sie im Browser die Seitenquelle. (Z. B. in Internet Explorer Maustaste auf die Seite, und klicken Sie dann auf **Quelltext anzeigen**.)

    Somit können Sie das Markup für die Webseite zu finden, das an den Browser gesendet wird, der die Index-Seitenmarkup mit die Inhaltsblöcke kombiniert. Das folgende Beispiel zeigt den Quellcode für die Seite, die für gerendert wird *Index.cshtml*. Die Aufrufe von `RenderPage` , die Sie eingefügt *Index.cshtml* durch den tatsächlichen Inhalt der Kopf- und Fußzeile Dateien ersetzt wurden.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Erstellen ein konsistentes Erscheinungsbild Layoutseiten mit

Bisher haben Sie gesehen, dass sie einfach den gleichen Inhalt auf mehreren Seiten enthalten ist. Ein strukturierter Ansatz zum Erstellen von eines konsistenten Erscheinungsbilds für eine Website ist die Verwendung von Layoutseiten. Eine Layoutseite definiert die Struktur einer Webseite, aber keine eigentlichen Inhalt enthalten. Nachdem Sie eine Layoutseite erstellt haben, können Sie Webseiten erstellen, die den Inhalt enthalten, und sie dann auf der Seite "Layout" zu verknüpfen. Wenn diese Seiten angezeigt werden, müssen sie entsprechend der Seite "Layout" formatiert. (In diesem Sinn verhält sich eine Layoutseite als eine Art der Vorlage für Inhalte, die in anderen Seiten definiert ist.)

Layoutseite wird genau wie jeder HTML-Seite, außer dass es sich um einen Aufruf von enthält die `RenderBody` Methode. Die Position der `RenderBody` Methode in der Seite "Layout" bestimmt, in dem die Informationen aus der Seite Inhalt eingeschlossen werden.

Das folgende Diagramm zeigt, wie Inhaltsseiten und Layoutseiten kombiniert, zur Laufzeit, um die fertigen Webseite zu erzeugen. Der Browser fordert eine Inhaltsseite. Inhaltsseite enthält Code, der angibt, auf der Layoutseite für die Seite Struktur verwenden. In der Seite "Layout" wird der Inhalt an der Stelle eingefügt, in dem die `RenderBody` -Methode aufgerufen wird. Content-Blöcke können auch in der Seite "Layout" eingefügt werden, durch Aufrufen der `RenderPage` -Methode, die Möglichkeit, die Sie im vorherigen Abschnitt ausgeführt haben. Wenn die Webseite abgeschlossen ist, wird er an den Browser gesendet.

![Screenshot der Seite im Browser, der sich ergibt, die von der Ausführung einer Seite, die Aufrufe der Methode RenderBody enthält.](3-creating-a-consistent-look/_static/image3.jpg)

Das folgende Verfahren zeigt, wie ein Layout und einen Hyperlink Inhaltsseiten zu erstellen.

1. In der *Shared* Ordner der Website, erstellen Sie eine Datei namens  *\_Layout1.cshtml*.
2. Ersetzen Sie jeglichen vorhandenen Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Verwenden Sie die `RenderPage` Methode in einer Layoutseite Inhaltsblöcke eingefügt. Eine Layoutseite darf nur ein Aufruf an die `RenderBody` Methode.
3. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Header2.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Klicken Sie im Stammordner, erstellen Sie einen neuen Ordner, und nennen Sie sie *Stile*.
5. In der *Stile* Ordner, erstellen Sie eine Datei mit dem Namen *"Site.CSS" ändern* und fügen Sie die folgenden Definitionen:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Diese Definitionen sind hier nur, um anzuzeigen, wie Stylesheets mit Layoutseiten verwendet werden können. Wenn Sie möchten, können Sie Ihre eigenen Formatvorlagen für diese Elemente definieren.
6. Erstellen Sie im Stammordner, eine Datei namens *Content1.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Dies ist eine Seite, die eine Layoutseite verwenden. Der Codeblock am oberen Rand der Seite "gibt an, welche Seite" Layout "zu verwenden, um diesen Inhalt zu formatieren.
7. Führen Sie *Content1.cshtml* in einem Browser. Die gerenderte Seite verwendet das Format und im Stylesheet definierten  *\_Layout1.cshtml* und den Text (Inhalt), die in definierten *Content1.cshtml*.

    ![[Image]](3-creating-a-consistent-look/_static/image4.jpg)

    Wiederholen Sie Schritt 6, um zusätzliche Inhaltsseiten zu erstellen, die klicken Sie dann die gleichen Layoutseite freigeben können.

    > [!NOTE]
    > Sie können Ihre Website einrichten, sodass dieselbe Layoutseite automatisch für alle Inhaltsseiten in einem Ordner verwendet werden können. Weitere Informationen finden Sie unter [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Entwerfen von Layoutseiten, die mehrere Inhaltsabschnitte haben

Inhaltsseite kann mehrere Abschnitte haben und das ist nützlich, wenn es sich bei Layouts mit verschiedenen Bereichen mit ersetzbaren Inhalt verwendet werden sollen. Auf der Inhaltsseite Geben Sie jeder Abschnitt einen eindeutigen Namen zu. (Der Standardabschnitt bleibt unbenannte.) Fügen Sie in der Seite "Layout" eine `RenderBody` Methode, um anzugeben, in dem Abschnitt unbenannte (Standard) angezeigt werden soll. Sie fügen Sie dann separate `RenderSection` Methoden, um benannte Abschnitte einzeln zu rendern.

Das folgende Diagramm zeigt, wie ASP.NET Inhalt behandelt, die wiederum mehrere Abschnitte unterteilt ist. Jeder benannte Abschnitt befindet sich in einem Abschnittsblock in der Seite Inhalt. (sie sind mit dem Namen `Header` und `List` im Beispiel.) Das Framework Layoutseite an dem Punkt Inhaltsabschnitt eingefügt, in dem die `RenderSection` -Methode aufgerufen wird. Im Abschnitt unbenannte (Standard) wird an der Stelle eingefügt, in dem die `RenderBody` -Methode aufgerufen wird, wie Sie zuvor gesehen haben.

![Konzeptionelles Diagramm zeigt, wie die Methode RenderSection Verweise Abschnitte in der aktuellen Seite einfügt.](3-creating-a-consistent-look/_static/image5.jpg)

Diese Prozedur zeigt, wie eine Inhaltsseite zu erstellen, die mehrere Inhaltsabschnitte hat und wie gerendert werden mithilfe einer Layoutseite, die mehrere Inhaltsabschnitte unterstützt.

1. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Layout2.cshtml*.
2. Ersetzen Sie jeglichen vorhandenen Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Verwenden Sie die `RenderSection` Methode, um die Kopf-und die Liste zu rendern.
3. Erstellen Sie im Stammordner, eine Datei namens *Content2.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Diese Seite "Inhalt" enthält einen Codeblock am oberen Rand der Seite. Jeder benannte Abschnitt befindet sich in einem Abschnittsblock. Der Rest der Seite enthält den Inhaltsabschnitt Standard (unbenannt).
4. Führen Sie *Content2.cshtml* in einem Browser.

    ![Screenshot der Seite im Browser, der sich ergibt, die von der Ausführung einer Seite, die Aufrufe der Methode RenderSection enthält.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Inhaltsabschnitte machen Optional

Normalerweise müssen die Abschnitte, die Sie in einer Inhaltsseite erstellen Abschnitte entsprechen, die in der Seite "Layout" definiert sind. Fehler erhalten, wenn eine der folgenden auftreten:

- Inhaltsseite enthält einen Abschnitt, der keine entsprechenden Abschnitt in der Seite "Layout" hat.
- Die Seite "Layout" enthält einen Abschnitt für die kein Inhalt vorhanden ist.
- Die Seite "Layout" enthält die Methodenaufrufe, die versuchen, denselben Bereich mehr als einmal gerendert.

Allerdings können Sie dieses Verhalten für einen benannten Bereich überschreiben, indem Sie deklarieren im Abschnitt in der Seite "Layout" optional sein. Dadurch können Sie mehrere Inhaltsseiten können eine Layoutseite kennen, jedoch kann oder möglicherweise keinen Inhalt für einen bestimmten Bereich zu definieren.

1. Open *Content2.cshtml* und entfernen Sie den folgenden Abschnitt:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Speichern Sie die Seite, und führen Sie es dann in einem Browser. Eine Fehlermeldung wird angezeigt, da der Inhaltsseite Inhalt für einen Abschnitt auf der Layoutseite, nämlich der Headerabschnitt definiert bietet.

    ![Screenshot, der der Fehler angezeigt, der auftritt wird, wenn Sie einer Seite ausführen RenderSection Methodenaufrufe im entsprechenden Abschnitt wird jedoch nicht bereitgestellt.](3-creating-a-consistent-look/_static/image7.jpg)
3. In der *Shared* Ordner die  *\_Layout2.cshtml* Seite, und Ersetzen Sie diese Zeile:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    durch den folgenden Code:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Als Alternative können konnte die vorherige Zeile des Codes mit den folgenden Codeblock, ersetzen Sie die gleichen Ergebnisse erzeugt.

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Führen Sie die *Content2.cshtml* Seite in einem Browser erneut aus. (Wenn Sie weiterhin auf dieser Seite im Browser geöffnet haben, können Sie nur ihn aktualisieren.) Dieses Mal wird die Seite und kein Fehler angezeigt, obwohl es keinen Header besitzt.

## <a name="passing-data-to-layout-pages"></a>Übergeben von Daten an Layoutseiten

Sie müssen möglicherweise Daten, die auf der Inhaltsseite, die Sie zum Verweisen benötigen auf in einer Layoutseite definiert. Wenn dies der Fall ist, müssen Sie die Daten aus der Seite Inhalt auf der Seite "Layout" übergeben. Beispielsweise empfiehlt es sich um den Anmeldestatus eines Benutzers anzuzeigen oder möglicherweise möchten ein- oder ausblenden Inhaltsbereiche basierend auf Benutzereingaben.

Zum Übergeben von Daten von einer Inhaltsseite zu einer Layoutseite Sie setzen, Werte in der `PageData` Eigenschaft der Inhaltsseite. Die `PageData` Eigenschaft ist eine Auflistung von Name/Wert-Paaren, die die Daten enthalten, die Sie zwischen Seiten übergeben möchten. Auf der Layoutseite anschließend erfahren Sie Werte aus der `PageData` Eigenschaft.

Hier ist ein anderes Diagramm an. Veranschaulicht, wie ASP.NET verwenden, können die `PageData` Eigenschaft, um Werte aus einer Inhaltsseite auf die Seite "Layout" übergeben. Wenn ASP.NET auf der Webseite erstellen beginnt, erstellt der `PageData` Auflistung. Auf der Inhaltsseite, Schreiben Sie Code aus, um Daten zu speichern, der `PageData` Auflistung. Werte in der `PageData` -Auflistung kann zugegriffen werden, indem Sie andere Abschnitte in der Inhaltsseite oder zusätzliche Inhaltsblöcke.

![Konzeptionelle Darstellung, die zeigt, wie eine Inhaltsseite ein Wörterbuch PageData Auffüllen und übergeben die Informationen auf der Seite "Layout" kann.](3-creating-a-consistent-look/_static/image8.jpg)

Das folgende Verfahren zeigt, wie Daten von einer Inhaltsseite zu einer Layoutseite übergeben wird. Wenn die Seite ausgeführt wird, zeigt eine Schaltfläche, die vom Benutzer ausblenden oder Anzeigen einer Liste, die in die Seite "Layout" definiert ist. Wenn Benutzer auf die Schaltfläche klicken, wird einen Wert "true" / "false" (boolesch) in der `PageData` Eigenschaft. Die Seite "Layout" liest diesen Wert und ist er "false" Blendet Sie aus der Liste. Der Wert wird auch auf der Inhaltsseite verwendet, zu bestimmen, ob zum Anzeigen der **ausblenden Liste** Schaltfläche oder die **Liste anzeigen** Schaltfläche.

![[Image]](3-creating-a-consistent-look/_static/image9.jpg)

1. Erstellen Sie im Stammordner, eine Datei namens *Content3.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Der Code speichert zwei Teile der Daten in der `PageData` Eigenschaft &#8212; den Titel der Webseite und "true" oder "false", um anzugeben, ob eine Liste anzuzeigen.

    Beachten Sie, dass ASP.NET Sie die HTML-Markup der Seite bedingt über einen Codeblock abgelegt können. Z. B. die `if/else` Block im Text der Seite bestimmt die Form anzuzeigende je nachdem, ob `PageData["ShowList"]` festgelegt ist auf "true".
2. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Layout3.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Die Seite "Layout" enthält einen Ausdruck in der `<title>` Element, das Ruft den Titelwert aus der `PageData` Eigenschaft. Darüber hinaus verwendet der `ShowList` Wert, der die `PageData` Eigenschaft bestimmt, ob in der Liste Inhalt Block angezeigt.
3. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_List.cshtml* und jeglichen vorhandenen Inhalt durch Folgendes ersetzen:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Führen Sie die *Content3.cshtml* Seite in einem Browser. Die Seite wird angezeigt, mit der Liste auf der linken Seite der Seite angezeigt und ein **ausblenden Liste** unten auf die Schaltfläche.

    ![Screenshot der Seite, die der Liste und eine Schaltfläche, die besagt, dass "Liste ausblenden" enthält.](3-creating-a-consistent-look/_static/image10.jpg)
5. Klicken Sie auf **ausblenden Liste**. Die Liste wird ausgeblendet, und die Schaltfläche **Liste anzeigen**.

    ![Screenshot der Seite, die keine enthält, die Liste und eine Schaltfläche, die besagt, Liste anzeigen dass.](3-creating-a-consistent-look/_static/image11.jpg)
6. Klicken Sie auf die **Liste anzeigen** Schaltfläche und die Liste wird erneut angezeigt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Anpassen des standortweite Verhaltens für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
