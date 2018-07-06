---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Anzeigen von Daten in einem Diagramm mit ASP.NET Web Pages (Razor) | Microsoft-Dokumentation
author: microsoft
description: In diesem Kapitel wird erläutert, wie Daten in einem Diagramm angezeigt wird. In den vorherigen Kapiteln haben Sie gelernt, wie Daten manuell und in einem Raster angezeigt werden. In diesem Kapitel wird erläutert...
ms.author: aspnetcontent
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 161dfa1b2c0676c79baebb00e303e8cb9df1d4e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812580"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Anzeigen von Daten in einem Diagramm mit ASP.NET Web Pages (Razor)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Artikel wird erläutert, wie Sie ein Diagramm zu verwenden, zum Anzeigen von Daten in einer ASP.NET Web Pages (Razor)-Website mithilfe der `Chart` Helper.
> 
> **Sie lernen Folgendes**:
> 
> - Informationen zum Anzeigen von Daten in einem Diagramm.
> - Informationen zum Formatieren von Diagrammen, die unter Verwendung von integrierten Designs.
> - Gewusst wie: Speichern von Diagrammen und wie sie für eine bessere Leistung zwischengespeichert.
> 
> Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Funktionen dar:
> 
> - Die `Chart` Helper.
> 
> > [!NOTE]
> > Die Informationen in diesem Artikel gelten für ASP.NET Web Pages-1.0- und Web Pages 2.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Der Diagramm-Hilfe

Wenn Sie Ihre Daten in grafischer Form anzeigen möchten, können Sie `Chart` Helper. Die `Chart` Hilfsprogramm kann ein Bild, das Daten, in einer Vielzahl von Diagrammtypen anzeigt rendern. Er unterstützt viele Optionen zum Formatieren und beschriften. Die `Chart` Hilfsprogramm kann mehr als 30 Arten von Diagrammen, einschließlich aller Typen von Diagrammen, die Sie von Microsoft Excel oder anderen Tools möglicherweise Rendern &#8212; Flächendiagramme, Balkendiagramme, Säulendiagrammen, Linien- und Diagrammen als Kreisdiagrammen sowie weitere spezialisierter Diagramme wie Kursdiagramme.

| **Flächendiagramm** ![Beschreibung: Bild des flächendiagrammtyps](7-displaying-data-in-a-chart/_static/image1.jpg) | **Balkendiagramm** ![Beschreibung: Bild des balkendiagrammtyps](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Säulendiagramm** ![Beschreibung: Bild des säulendiagrammtyps](7-displaying-data-in-a-chart/_static/image3.jpg) | **Liniendiagramm** ![Beschreibung: Bild des liniendiagrammtyps](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Kreisdiagramm** ![Beschreibung: Bild des kreisdiagrammtyps](7-displaying-data-in-a-chart/_static/image5.jpg) | **Kursdiagramm** ![Beschreibung: Bild des kursdiagrammtyps](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Diagrammelemente

Diagramme stellen die Daten und zusätzliche Elemente, z. B. Legenden, Achsen, Reihen und So weiter. Die folgende Abbildung zeigt viele der Diagrammelemente, die Sie anpassen können, bei der Verwendung der `Chart` Helper. In diesem Artikel erfahren Sie, wie Sie festlegen (nicht alle) Elemente.

![Beschreibung: Bild der Diagrammelemente](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Erstellen eines Diagramms aus Daten

Die Daten, die Sie in einem Diagramm anzeigen, können aus einem Array, aus den Ergebnissen zurückgegeben, die aus einer Datenbank oder von Daten in eine XML-Datei sein.

### <a name="using-an-array"></a>Ein Array

Siehe [Einführung in ASP.NET Web Pages-Programmierung verwenden die Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890), ein Array können Sie eine Sammlung mit ähnlichen Elementen in einer einzelnen Variablen zu speichern. Sie können Arrays verwenden, um die Daten aufzunehmen, die Sie in Ihr Diagramm aufnehmen möchten.

Diese Prozedur zeigt, wie Sie ein Diagramm aus Daten in Arrays erstellen können mithilfe des Standarddiagrammtyp. Es wird gezeigt, wie im Diagramm auf der Seite angezeigt.

1. Erstellen Sie eine neue Datei namens *ChartArrayBasic.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Der Code zuerst erstellt ein neues Diagramm, und legt die Breite und Höhe. Geben Sie den Diagrammtitel mithilfe der `AddTitle` Methode. Um Daten hinzuzufügen, verwenden Sie die `AddSeries` Methode. In diesem Beispiel verwenden Sie die `name`, `xValue`, und `yValues` Parameter von der `AddSeries` Methode. Die `name` Parameter wird in der Diagrammlegende angezeigt. Die `xValue` Parameter enthält ein Array von Daten, die entlang der horizontalen Achse des Diagramms angezeigt wird. Die `yValues` Parameter enthält ein Array von Daten, die verwendet wird, um die vertikalen Punkte des Diagramms zu zeichnen.

    Die `Write` rendert die Methode tatsächlich im Diagramm. In diesem Fall, da Sie keinen Diagrammtyp angegeben haben die `Chart` rendert die Standarddiagramm, d.h. ein Säulendiagramm.
3. Führen Sie die Seite im Browser. Der Browser zeigt das Diagramm. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Mithilfe einer Abfrage für die Diagrammdaten

Wenn die Informationen, die Sie darstellen möchten in einer Datenbank ist, können Sie Ausführen eine Datenbankabfrage und verwenden Sie dann die Daten aus den Ergebnissen auf um das Diagramm zu erstellen. Dieses Verfahren zeigt, wie zum Lesen und Anzeigen der Daten aus der Datenbank, die in diesem Artikel erstellten [Einführung in die Arbeit mit einer Datenbank in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Hinzufügen einer *App\_Daten* Ordner in das Stammverzeichnis der Website ein, wenn der Ordner nicht bereits vorhanden ist.
2. In der *App\_Daten* Ordner, fügen Sie die Datenbankdatei mit dem Namen *SmallBakery.sdf* , finden Sie im [Einführung in die Arbeit mit einer Datenbank in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Erstellen Sie eine neue Datei namens *ChartDataQuery.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Der Code zuerst die SmallBakery-Datenbank wird geöffnet und weist sie einer Variablen namens `db`. Diese Variable steht für eine `Database` -Objekt, das Lese-und Schreibberechtigungen für die Datenbank verwendet werden kann. Als Nächstes führt der Code eine SQL-Abfrage zum Abrufen des Namens und der Preis für jedes Produkt. Der Code erstellt ein neues Diagramm und übergibt Sie die Datenbankabfrage, durch den Aufruf des Diagramms `DataBindTable` Methode. Diese Methode akzeptiert zwei Parameter: den `dataSource` Parameter ist für die Daten aus der Abfrage und die `xField` Parameter können Sie festlegen, welche Datenspalte für die x-Achse verwendet wird.

    Als Alternative zur Verwendung der `DataBindTable` -Methode, die Sie verwenden die `AddSeries` -Methode der der `Chart` Helper. Die `AddSeries` Methode ermöglicht das Festlegen der `xValue` und `yValues` Parameter. Z. B. statt der `DataBindTable` Methode wie folgt:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Sie können die `AddSeries` Methode wie folgt:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Beide die gleichen Ergebnisse zu rendern. Die `AddSeries` Methode ist flexibler, da Sie den Diagrammtyp und Daten mehr explizit angeben, können aber die `DataBindTable` Methode ist einfacher zu verwenden, wenn Sie die zusätzliche Flexibilität nicht benötigen.
5. Führen Sie die Seite in einem Browser. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Verwenden von XML-Daten

Die dritte Option für die diagrammerstellung ist die Verwendung eine XML-Datei wie die Daten für das Diagramm. Dies erfordert, dass die XML-Datei auch eine Schemadatei verfügen (*XSD* Datei), die die XML-Struktur beschreibt. Dieses Verfahren zeigt, wie Sie Daten aus einer XML-Datei zu lesen.

1. In der *App\_Daten* Ordner, erstellen Sie eine neue XML-Datei mit dem Namen *data.xml*.
2. Ersetzen Sie den vorhandenen XML-Code, mit der folgenden, die einige XML-über Mitarbeiter in einem fiktiven Unternehmen Daten. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. In der *App\_Daten* Ordner, erstellen Sie eine neue XML-Datei mit dem Namen *data.xsd*. (Beachten Sie, dass die Erweiterung dieses Mal *XSD*.)
4. Ersetzen Sie den vorhandenen XML-Code durch Folgendes: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Erstellen Sie im Stammverzeichnis der Website, eine neue Datei namens *ChartDataXML.cshtml*.
6. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Der Code erstellt zunächst einen `DataSet` Objekt. Dieses Objekt wird verwendet, um die Daten zu verwalten, die aus der XML-Datei gelesen und entsprechend den Informationen in der Schemadatei zu organisieren. (Beachten Sie, dass am Anfang der Code die Anweisung enthält `using SystemData`. Dies ist erforderlich, damit Sie die Arbeit mit können der `DataSet` Objekt. Weitere Informationen finden Sie unter [ &quot;Using&quot; -Anweisungen und vollständig qualifizierte Namen](#SB_UsingStatements) weiter unten in diesem Artikel.)

    Als Nächstes der Code erstellt eine `DataView` Objekt basierend auf dem Dataset. Die Datensicht bietet ein Objekt, das das Diagramm werden, um gebunden kann &#8212; , also lesen und zu zeichnen. Das Diagramm gebunden wird, um die Daten mit der `AddSeries` -Methode, wie Sie weiter oben gesehen haben, wenn die Daten des Arrays, außer dass diesmal diagrammerstellung der `xValue` und `yValues` Parameter werden festgelegt, um die `DataView` Objekt.

    Dieses Beispiel zeigt auch das Angeben eines bestimmten Diagrammtyps. Wenn die Daten hinzugefügt werden, der `AddSeries` -Methode, die `chartType` Parametersatz ist auch zu einem Kreisdiagramm angezeigt.
7. Führen Sie die Seite in einem Browser. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"-Anweisungen und vollständig qualifizierte Namen
> 
> .NET Framework, dem basierend auf ASP.NET Web Pages mit Razor-Syntax besteht aus Tausenden von Komponenten (Klassen). Damit praktikabel, alle diese Klassen verwenden können, sind sie in organisiert *Namespaces*, denen es sich ein wenig wie Bibliotheken. Z. B. die `System.Web` -Namespace enthält Klassen, die Browser-/Serverkommunikation, unterstützen die `System.Xml` -Namespace enthält Klassen, die das Erstellen und Lesen von XML-Dateien verwendet werden und die `System.Data` -Namespace enthält Klassen, mit denen Sie arbeiten mit Daten.
> 
> Zum Zugriff auf einer bestimmten Klasse in .NET Framework muss Code wissen nicht nur den Klassennamen, sondern auch den Namespace aus, dem die Klasse befindet. Um verwenden Sie beispielsweise die `Chart` Hilfsprogramms des, muss Code finden Sie die `System.Web.Helpers.Chart` -Klasse, die den Namespace kombiniert (`System.Web.Helpers`) mit dem Klassennamen (`Chart`). Dies bezeichnet man als der Klasse *vollqualifizierten* Namen &#8212; seine vollständige, eindeutige Position in der Umfangs von .NET Framework. Im Code würde dies wie folgt aussehen:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Es ist jedoch umständlich (und fehleranfällig), müssen Sie diese langen, den vollqualifizierten Namen verwenden, jedes Mal, wenn Sie auf eine Klasse oder das Hilfsprogramm verweisen möchten. Um Klassennamen verwenden zu erleichtern, können Sie aus diesem Grund *importieren* die Namespaces interessiert, in der Regel ist nur eine Handvoll aus der vielen in .NET Framework-Namespaces. Wenn Sie einen Namespace importiert haben, können Sie nur ein Klassenname (`Chart`) anstelle der vollqualifizierte Name (`System.Web.Helpers.Chart`). Wenn Ihr Code ausgeführt wird und ein Klassenname auftritt, finden sie in der nur die Namespaces, die Sie importiert haben, um diese Klasse zu suchen.
> 
> Wenn Sie ASP.NET Web Pages mit Razor-Syntax verwenden, um Webseiten zu erstellen, Sie in der Regel verwenden den gleichen Satz von Klassen jedes Mal, einschließlich der `WebPage` -Klasse, die verschiedene Hilfsprogramme und So weiter. Damit Sie sparen die Arbeit, die relevanten Namespaces importieren, jedes Mal, wenn Sie eine Website erstellen, ist ASP.NET konfiguriert, damit er automatisch einen Satz von Core-Namespaces für jede Website importiert. Daher mussten Sie noch nicht für den Umgang mit Namespaces, oder importieren Sie bis jetzt; alle Klassen, mit denen Sie gearbeitet haben, sind in Namespaces, die Sie bereits importiert werden.
> 
> Allerdings müssen gelegentlich Sie arbeiten mit einer Klasse, die in einem Namespace ist nicht das für Sie automatisch importiert wird. In diesem Fall können Sie entweder den vollqualifizierten Namen von dieser Klasse verwenden, oder Sie können den Namespace, der die Klasse enthält manuell importieren. Um einen Namespace zu importieren, verwenden Sie die `using` Anweisung (`import` in Visual Basic), wie Sie in einem Beispiel weiter oben gesehen haben den Artikel.
> 
> Z. B. die `DataSet` Klasse ist in der `System.Data` Namespace. Die `System.Data` Namespace ist nicht automatisch für ASP.NET Razor-Seiten zur Verfügung. Aus diesem Grund funktioniert mit der `DataSet` Klasse unter Verwendung des vollqualifizierten Namens, können Sie Code wie folgt verwenden:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Wenn Sie verwenden die `DataSet` Klasse wiederholt Sie einen Namespace wie folgt importieren und dann nur den Klassennamen in Code verwenden:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Sie können hinzufügen `using` -Anweisungen für alle anderen .NET Framework-Namespaces, die Sie verweisen möchten. Aber wie bereits erwähnt, brauchen Sie häufig dazu, da die meisten Klassen, die Sie verwenden werden in Namespaces befinden, die von ASP.NET automatisch, für die Verwendung in importiert werden *.cshtml* und *vbhtml* Seiten.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Anzeigen von Diagrammen in einer Webseite

In den Beispielen haben Sie bisher gesehen, Sie ein Diagramm erstellen, und klicken Sie dann das Diagramm gerendert wird, direkt an den Browser als Grafik. In vielen Fällen möchten Sie jedoch, um das Diagramm als Teil einer Seite, nicht nur allein im Browser anzuzeigen. Zu diesem Zweck müssen einen zweistufiger Prozess. Der erste Schritt ist, erstellen Sie eine Seite, die im Diagramm generiert, da Sie bereits gesehen haben.

Der zweite Schritt ist das resultierende Image in eine andere Seite angezeigt. Um das Bild anzuzeigen, verwenden Sie eine HTML `<img>` Element, in der gleichen Weise um jedem Bild anzuzeigen. Jedoch anstelle von Verweisen auf eine *jpg* oder *PNG* -Datei, die `<img>` Elementverweise der *.cshtml* -Datei mit der `Chart` Helper, das Diagramm wird erstellt. Wenn die Seite ausgeführt wird, wird die `<img>` Element empfängt die Ausgabe von der `Chart` Helper und rendert das Diagramm.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Erstellen Sie eine Datei mit dem Namen *ShowChart.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Der Code verwendet die `<img>` im Diagramm angezeigt werden soll, die Sie zuvor in erstellt die *ChartArrayBasic.cshtml* Datei.
3. Führen Sie die Webseite in einem Browser. Die *ShowChart.cshtml* Datei zeigt das Diagrammbild basierend auf der Code in die *ChartArrayBasic.cshtml* Datei.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Formatieren eines Diagramms

Die `Chart` Hilfsprogramm unterstützt eine große Anzahl von Optionen, mit denen Sie die Darstellung des Diagramms anpassen können. Sie können Farben, Schriftarten, Ränder und usw. festlegen. Eine einfache Möglichkeit zum Anpassen der Darstellung eines Diagramms ist die Verwendung einer *Design*. Designs sind Informationssammlungen, die angeben, wie ein Diagramm mithilfe von Schriftarten, Farben, Beschriftungen, Paletten, Rahmen und Effekten gerendert. (Beachten Sie, dass das Format eines Diagramms den Typ des Diagramms nicht angegeben ist.)

Die folgende Tabelle enthält die integrierten Designs.

| Design | Beschreibung |
| --- | --- |
| `Vanilla` | Zeigt den roten Spalten auf weißem Hintergrund an. |
| `Blue` | Zeigt blauer Spalten auf einen blauen Hintergrund mit Farbverlauf. |
| `Green` | Zeigt blauer Spalten in einem grünen Hintergrund mit Farbverlauf. |
| `Yellow` | Orange zeigt die Spalten auf einen gelben Hintergrund mit Farbverlauf. |
| `Vanilla3D` | Zeigt 3-d-Rot-Spalten auf weißem Hintergrund an. |

Sie können angeben, dass das Design zu verwenden, wenn Sie ein neues Diagramm erstellen.

1. Erstellen Sie eine neue Datei namens *ChartStyleGreen.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Dieser Code ist identisch mit dem vorherigen Beispiel, das für die Datenbank verwendet, fügt jedoch das `theme` Parameter beim Erstellen der `Chart` Objekt. Das folgende Beispiel zeigt den geänderten Code:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Führen Sie die Seite in einem Browser. Sie sehen die gleichen Daten wie zuvor, aber das Diagramm sieht besseres aus: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Speichern eines Diagramms

Bei Verwendung der `Chart` Helper als Sie bislang kennengelernt haben in diesem Artikel das Hilfsprogramm erneut das Diagramm von Grund auf neu erstellt jedes Mal, die sie aufgerufen wird. Bei Bedarf werden von der Code für das Diagramm auch die Datenbank erneut abgefragt oder erneut liest die XML-Datei zum Abrufen der Daten. In einigen Fällen auf diese Weise kann ein komplexer Vorgang sein, z. B. wenn die Datenbank, die Sie Abfragen sind sehr groß ist oder wenn die XML-Datei eine große Datenmenge enthält. Auch wenn das Diagramm nicht viele Daten beinhalten beansprucht dynamisch erstellen ein Image von Serverressourcen, und wenn viele Benutzer anfordern, die Seite oder Seiten, die im Diagramm angezeigt, es kann sein wirkt sich auf die Leistung Ihrer Website.

Damit können Sie die potenziellen Auswirkungen auf die Leistung Erstellen eines Diagramms zu reduzieren, können Sie ein Diagramm das erste Mal erstellen Sie es benötigen, und speichern Sie sie. Wenn das Diagramm in diesem Fall benötigt wird, anstatt neu zu erstellen, können Sie nur die gespeicherte Version abrufen und rendern.

Sie können ein Diagramm wie folgt speichern:

- Speichern Sie das Diagramm im Arbeitsspeicher des Computers (auf dem Server).
- Speichern Sie das Diagramm als Bilddatei an.
- Speichern Sie das Diagramm als eine XML-Datei. Dieser Option können Sie das Diagramm zu ändern, bevor Sie sie speichern.

### <a name="caching-a-chart"></a>Zwischenspeichern eines Diagramms

Nachdem Sie ein Diagramm erstellt haben, können Sie ihn Zwischenspeichern. Zwischenspeichern eines Diagramms bedeutet, dass es nicht neu erstellt werden, wenn sie wieder angezeigt werden muss. Wenn Sie ein Diagramm im Cache speichern, geben Sie ihm einen Schlüssel, der für dieses Diagramm eindeutig sein muss.

Im Cache gespeicherte Diagramme können entfernt werden, wenn der Server für den Arbeitsspeicher knapp wird. Darüber hinaus wird der Cache geleert, wenn Ihre Anwendung aus irgendeinem Grund neu gestartet wird. Daher ist die Standardmethode zum Arbeiten mit einem zwischengespeicherten Diagramm immer zuerst prüfen, ob es im Cache verfügbar ist, und falls nicht, und klicken Sie dann zum Erstellen oder neu zu erstellen.

1. Erstellen Sie im Stammverzeichnis Ihrer Website, eine Datei namens *ShowCachedChart.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Die `<img>` Tag enthält eine `src` -Attribut, das zeigt die *ChartSaveToCache.cshtml* Datei und einen Schlüssel an die Seite als Abfragezeichenfolge übergeben. Der Schlüssel enthält den Wert &quot;MyChartKey&quot;. Die *ChartSaveToCache.cshtml* -Datei enthält die `Chart` Hilfsmethode, die das Diagramm erstellt. Sie erstellen auf dieser Seite in Kürze.

    Am Ende der Seite ein Link zu einer Seite mit dem Namen besteht *ClearCache.cshtml*. Das ist eine Seite, die Sie auch in Kürze erstellen. Müssen Sie die *ClearCache.cshtml* nur für die Zwischenspeicherung für dieses Beispiel zu testen – es ist nicht, einen Link oder eine Seite, die Sie normalerweise beim Arbeiten mit Diagrammen von zwischengespeicherten enthalten ist.
3. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *ChartSaveToCache.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Der Code prüft zunächst, ob alles als der Schlüsselwert in der Abfragezeichenfolge übergeben wurde. Wenn also der Code versucht, lesen Sie ein Diagramm aus dem Cache durch Aufrufen der `GetFromCache` -Methode und übergeben sie den Schlüssel. Wenn sich herausstellt, die vorhanden "nothing" in den Cache unter diesem Schlüssel ist (die beim ersten, die das Diagramm angefordert hat, ist der Fall wäre), erstellt der Code das Diagramm wie gewohnt an. Wenn das Diagramm abgeschlossen ist, der Code speichert sie in den Cache durch Aufrufen von `SaveToCache`. Diese Methode benötigt einen Schlüssel (also das Diagramm später angefordert werden kann), und die Zeitspanne, die das Diagramm im Cache gespeichert werden soll. (Die genaue Zeit, die Sie einem Diagramm Zwischenspeichern würde hängt wie oft Sie dachten, dass die Daten, die es darstellt geändert werden können.) Die `SaveToCache` Methode erfordert außerdem eine `slidingExpiration` Parameter &#8212; Wenn dieser Wert wird festgelegt auf "true", "das Timeout Zähler wird jedes Mal zurückgesetzt Diagramm erfolgt. In diesem Fall bedeutet dies in Kraft, dass Cacheeintrag des Diagramms 2 Minuten nach der letzten Zeit abläuft, hat eine Person im Diagramm zugegriffen. (Die Alternative, um die gleitende Ablaufzeit ist ein absoluter Ablauf, was bedeutet, dass der Cacheeintrag abläuft würde genau 2 Minuten, nachdem es wurde im Cache abgelegt, unabhängig davon, wie oft sie zugegriffen wurde.)

    Der Code schließlich verwendet der `WriteFromCache` Methode zum Abrufen und Rendern das Diagramm aus dem Cache. Beachten Sie, dass diese Methode außerhalb der `if` Block, der den Cache überprüft werden, da das Diagramm aus dem Cache abgerufen werden sollen, ob das Diagramm zunächst es gab oder mussten generiert und im Cache gespeichert werden.

    Beachten Sie, dass im Beispiel die `AddTitle` Methode enthält einen Zeitstempel. (sie fügt das aktuelle Datum und die Uhrzeit &#8212; `DateTime.Now` &#8212; auf den Titel.)
5. Erstellen Sie eine neue Seite mit dem Namen *ClearCache.cshtml* und seinen Inhalt durch Folgendes ersetzen:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Diese Seite verwendet die `WebCache` Hilfsfunktion, um das Diagramm zu entfernen, die in zwischengespeicherten *ChartSaveToCache.cshtml*. Wie bereits erwähnt, müssen Sie normalerweise eine Seite wie diese haben. Sie erstellen hier nur zum Testen der Zwischenspeicherung vereinfachen.
6. Führen Sie die *ShowCachedChart.cshtml* Webseite in einem Browser. Die Seite zeigt das Diagrammbild basierend auf der Code in die *ChartSaveToCache.cshtml* Datei. Notieren Sie sich die Aussage des Zeitstempels der Diagrammtitel. 

    ![Beschreibung: Überblick über grundlegende Diagramm mit den Zeitstempel in den Diagrammtitel](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Schließen Sie den Browser.
8. Führen Sie die *ShowCachedChart.cshtml* erneut aus. Beachten Sie, dass die Zeitstempel gleich wie zuvor ist dies bedeutet, dass das Diagramm wurde nicht erneut generiert, aber stattdessen aus dem Cache gelesen wurde.
9. In *ShowCachedChart.cshtml*, klicken Sie auf die **Cache löschen** Link. Dadurch gelangen Sie zum *ClearCache.cshtml*, das meldet, dass der Cache gelöscht wurde.
10. Klicken Sie auf die **zurück zu ShowCachedChart.cshtml** verknüpfen, oder führen Sie erneut *ShowCachedChart.cshtml* von WebMatrix. Beachten Sie, dass dieses Mal der Zeitstempel geändert hat, aus, da es sich bei der Cache gelöscht wurde. Daher musste der Code zum Generieren des Diagramms, und legen Sie sie wieder in den Cache ein.

### <a name="saving-a-chart-as-an-image-file"></a>Speichern eines Diagramms als Bilddatei

Sie können auch ein Diagramm speichern, als Bilddatei (z. B. eine *jpg* Datei) auf dem Server. Anschließend können Sie die Bilddatei genauso wie jedes Image. Der Vorteil ist die Datei wird gespeichert sondern in einem temporären Cache gespeichert. Können Sie ein neue Diagrammbild speichern, zu unterschiedlichen Zeitpunkten (z. B. einmal pro Stunde) und eine permanente Aufzeichnung der Änderungen, die im Laufe der Zeit auftreten, werden beibehalten. Beachten Sie, dass Sie sicherstellen müssen, dass Ihre Web-Anwendung verfügt über die Berechtigung zum Speichern einer Datei in den Ordner auf dem Server, in dem Sie die Imagedatei speichern möchten.

1. Erstellen Sie im Stammverzeichnis Ihrer Website einen Ordner namens  *\_ChartFiles* ist es nicht bereits vorhanden.
2. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *ChartSave.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Der Code überprüft zuerst, ob die *jpg* Datei vorhanden ist, durch den Aufruf der `File.Exists` Methode. Wenn die Datei nicht vorhanden ist, erstellt der Code ein neues `Chart` aus einem Array. Diesmal ist der Code Ruft die `Save` -Methode auf und übergibt die `path` -Parameter den Dateipfad und den Dateinamen zum Speichern des Diagramms fest. Im Hauptteil der Seite ein `<img>` -Element verwendet den Pfad, um zu zeigen die *jpg* Datei angezeigt.
4. Führen Sie die *ChartSave.cshtml* Datei.
5. WebMatrix zurück. Beachten Sie, dass eine Bilddatei mit dem Namen *chart01.jpg* wurde gespeichert, der  *\_ChartFiles* Ordner.

### <a name="saving-a-chart-as-an-xml-file"></a>Speichern eines Diagramms als eine XML-Datei

Schließlich können Sie ein Diagramm als eine XML-Datei auf dem Server speichern. Ein Vorteil der Verwendung dieser Methode über das Zwischenspeichern des Diagramms oder Speichern des Diagramms in eine Datei ist, dass Sie den XML-Code vor der Anzeige des Diagramms aus, wenn Sie möchten, ändern können. Die Anwendung muss über Lese-/Schreibberechtigungen für den Ordner auf dem Server verfügen, in dem Sie die Imagedatei speichern möchten.

1. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *ChartSaveXml.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Dieser Code ist ähnlich wie der Code, den Sie bereits gesehen haben für ein Diagramm im Cache speichern, jedoch eine XML-Datei verwendet. Der Code zuerst überprüft, ob die XML-Datei vorhanden, durch den Aufruf ist der `File.Exists` Methode. Wenn die Datei vorhanden ist, erstellt der Code eine neue `Chart` -Objekt und übergibt den Dateinamen als dem `themePath` Parameter. Dadurch wird das Diagramm, das basierend auf den Inhalt der XML-Datei wird erstellt. Wenn die XML-Datei nicht bereits vorhanden ist, wird der Code erstellt ein Diagramm wie üblich und ruft dann `SaveXml` zu speichern. Das Diagramm gerendert wird, mit der `Write` -Methode, Sie haben gesehen vor.

    Wie bei der Seite, die Zwischenspeicherung wurde gezeigt, enthält dieser Code einen Zeitstempel in den Titel an.
3. Erstellen Sie eine neue Seite mit dem Namen *ChartDisplayXMLChart.cshtml* , und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Führen Sie die *ChartDisplayXMLChart.cshtml* Seite. Das Diagramm wird angezeigt. Notieren Sie sich den Zeitstempel im Titel des des Diagramms.
5. Schließen Sie den Browser.
6. WebMatrix, mit der Maustaste der  *\_ChartFiles* Ordner, klicken Sie auf **aktualisieren**, und öffnen Sie dann den Ordner. Die *XMLChart.xml* Datei in diesem Ordner erstellt wurde, indem die `Chart` Helper. 

    ![Beschreibung: Der _ChartFiles Ordner mit der XMLChart.xml-Datei, die das Diagramm Hilfsprogramm erstellt wurden.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Führen Sie die *ChartDisplayXMLChart.cshtml* Seite erneut. Das Diagramm zeigt den gleichen Zeitstempel als beim ersten die Seite ausführen. Das ist da das Diagramm aus der XML-Code generiert wird, die Sie zuvor gespeichert haben.
8. Öffnen Sie in WebMatrix die  *\_ChartFiles* Ordner, und löschen die *XMLChart.xml* Datei.
9. Führen Sie die *ChartDisplayXMLChart.cshtml* Seite noch einmal. Dieses Mal der Zeitstempel aktualisiert wurde, da die `Chart` Helper mussten, um die XML-Datei neu zu erstellen. Wenn Sie möchten, überprüfen Sie die  *\_ChartFiles* Ordner, und beachten Sie, dass die XML-Datei zurück.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die Arbeit mit einer Datenbank in der ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Verwendung von Caching in ASP.NET Web Pages-Websites zur Verbesserung der Leistung](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Klasse "Diagrammeigenschaften"](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (ASP.NET Web Pages-API-Referenz auf MSDN)
