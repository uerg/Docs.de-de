---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmierung ASP.NET-Webseiten (Razor) mithilfe von Visual Studio | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Anhang wird erläutert, wie Sie Visual Studio 2010 noch Visual Web Developer 2010 Express Programm ASP.NET Web Pages mit Razor-Syntax verwenden können.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5b8df17ec1021d133579e23cb4f5b0d0f67d4c7c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020454"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmieren von ASP.NET-Webseiten (Razor) mithilfe von Visual Studio
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Visual Studio oder Visual Web Developer Express für Websites mit ASP.NET Web Pages (Razor)-Programm verwenden können.
>
> Sie lernen Folgendes
>
> - Was müssen Sie (falls zutreffend) installieren, um mit ASP.NET Web Pages in Ihrer Version von Visual Studio arbeiten.
> - Wie Sie Support für ASP.NET Web Pages Visual Web Developer 2010 Express hinzufügen.
> - Informationen zu den Funktionen zum Arbeiten mit ASP.NET Razor-Seiten, einschließlich IntelliSense und der Debugger in Visual Studio zu verwenden.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 und WebMatrix 2.


Sie können ASP.NET Web Pages mit Razor-Syntax, die mithilfe von WebMatrix oder viele andere Code-Editoren programmieren. Sie können auch Microsoft Visual Studio verwenden, eine voll funktionsfähige integrierte Entwicklungsumgebung (IDE), die einen Reihe leistungsstarken Tools bietet für viele Arten von Anwendungen (nicht nur Websites) erstellen. Um mit ASP.NET Razor-Seiten arbeiten, können Sie [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Zwei besonders nützlich, Features, die Visual Studio für die Programmierung mit ASP.NET Razor-Webseiten bereitstellt, sind:

- *IntelliSense*. Die IntelliSense-Funktion, die in Visual Studio integriert ist umfassender als IntelliSense-Funktionen in WebMatrix.
- *Debugger*. Mit dem Debugger können Sie Ihren Code zu beheben, indem Sie ein Programm wird beendet, während Sie ausgeführt wird, untersuchen Variablen, und den Code Zeile für Zeile durchlaufen.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Mithilfe von Visual Studio mit verschiedenen Versionen von ASP.NET Web Pages

Zum Entwickeln von ASP.NET Web-apps in Visual Studio 2017 installieren die **ASP.NET und Webentwicklung** arbeitsauslastung.

Visual Studio 2012 und Visual Studio 2013 enthalten Unterstützung für ASP.NET Web Pages. (Die Pakete, die erforderlich sind, um die Unterstützung von ASP.NET Web Pages sind installiert, bei der Installation von Visual Studio.)

Visual Studio 2010 wird nicht standardmäßig für ASP.NET Web Pages unterstützen. Um ASP.NET Web Pages mit Visual Studio 2010 verwenden zu können, müssen Sie das ASP.NET MVC-Paket installieren. Um ASP.NET Web Pages 2 zu erhalten, installieren Sie ASP.NET MVC 4.

Die folgende Tabelle enthält die Unterstützung für ASP.NET Web Pages in verschiedenen Versionen von Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Installieren Sie ASP.NET MVC 4 | (Enthalten) | (Enthalten) |
| **ASP.NET-Webseiten 3** |  | Update für ASP.NET Web Pages 3 über NuGet | (Enthalten) |

Um mit Visual Studio 2010 arbeiten, finden Sie unter [Installieren der Datenbankunterstützung für ASP.NET Web Pages in Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Starten von Visual Studio von WebMatrix

Wenn Sie ein Projekt, klicken Sie in WebMatrix gestartet haben und Visual Studio wechseln möchten, bietet WebMatrix eine Schaltfläche, um das Projekt ganz einfach in Visual Studio zu öffnen. Sie benötigen Visual Studio installiert wird, auf dem Computer für diese Schaltfläche aktiviert werden. Die folgende Abbildung zeigt die Schaltfläche mit den in WebMatrix.

![Starten Sie Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Wenn Sie die Schaltfläche klicken, wird das Projekt in Visual Studio geöffnet. Sie können WebMatrix und Visual Studio hin-und problemlos wechseln. Sie werden benachrichtigt, wenn alle Dateien, die in der anderen Umgebung geändert haben und neu geladen werden, um die neuesten Änderungen abzurufen müssen.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Erstellen von ASP.NET Razor-Website in Visual Studio

So erstellen eine ASP.NET Razor-Website in Visual Studio:

1. Öffnen Sie Visual Studio.
2. In der **Datei** Menü klicken Sie auf **neue Website**.

    ![neue Website erstellen](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. In der **neue Website** Dialogfeld wählen die Sprache (Visual c# oder Visual Basic) verwenden.
4. Wählen Sie die **ASP.NET-Website (Razor)** Vorlage.

    ![Razor-Website](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Klicken Sie auf **OK**.

Das neue Projekt vorhanden ist und mit einige Standard-Webseiten, die Ihnen beim Einstieg helfen aufgefüllt wird.

### <a name="using-intellisense"></a>Using IntelliSense

Nun, dass Sie eine Website erstellt haben, können Sie die Funktionsweise von IntelliSense in Visual Studio sehen.

1. Öffnen Sie auf der Website, die Sie gerade erstellt haben, die *Default.cshtml* Seite.
2. Nach der `<h3>` Geben Sie Tags auf der Seite `@ServerInfo.` (samt Punkt). Beachten Sie, wie zeigt IntelliSense die verfügbaren Methoden für die `ServerInfo` -Hilfsmethode in einem Dropdown-Liste.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Wählen Sie die `GetHtml` Methode aus der Liste und drücken Sie dann die EINGABETASTE. IntelliSense füllt automatisch die Methode ab. (Wie mit einer beliebigen Methode in c# werden soll, das Sie hinzufügen müssen `()` Zeichen nach der Methode.) Den vollständigen Code für die `GetHtml` Methode sieht wie im folgenden Beispiel aus:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Drücken Sie STRG + F5, um die Seite auszuführen. Dies ist, sieht die Seite, wie wenn in einem Browser angezeigt:

    ![Standardseite im browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Schließen Sie den Browser.

### <a name="using-the-debugger"></a>Verwenden des Debuggers

1. Am oberen Rand der *Default.cshtml* Seite nach der Zeile, die mit beginnt `Page.Title`, fügen Sie die folgende Codezeile hinzu:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. In den grauen Rand des Editors auf der linken Seite des Codes, klicken Sie neben auf diese neue Zeile zum Hinzufügen einer *Haltepunkt*. Ein Haltepunkt ist ein Marker, der weist den Debugger Beenden der Ausführung des Programms an diesem Punkt an, damit Sie sehen, was passiert.

    ![Haltepunkt festlegen](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Entfernen Sie den Aufruf an die `ServerInfo.GetHtml` -Methode, und fügen Sie einen Aufruf von der `@myTime` Variable an. Dieser Aufruf zeigt die aktuelle Zeitwert, der von der nächsten Codezeile zurückgegeben wird.
4. Drücken Sie F5, um die Seite im Debugger auszuführen. Die Seite hält am Breakpoint, den Sie festlegen. Die folgende Abbildung zeigt die Seite im Editor mit dem Haltepunkt (in gelb dargestellt).

    ![Haltepunkt](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Klicken Sie in der Debug-Symbolleiste auf die **Einzelschritt** Schaltfläche (oder drücken Sie F11), um die nächste Codezeile auszuführen. Bei jedem Klicken auf diese Schaltfläche, fahren Sie die Ausführung in die nächste Zeile des Codes fort.

    ![Einzelschritt in Schaltfläche](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Überprüfen Sie den Wert, der die `myTime` Variable, indem Sie den Mauszeiger darüber halten oder überprüfen im angezeigten Werte die **"lokal"** und **Aufrufliste** Windows. Der Wert der Variablen wird von Visual Studio angezeigt.

    ![Show-Time-Werten](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Wenn Sie fertig sind, untersuchen Variablen und Code schrittweise durchlaufen, drücken Sie F5, um fortzufahren, Ausführen der Seite, ohne bei jeder Zeile. Wenn Sie alle den Code schrittweise durchlaufen abgeschlossen haben, zeigt der Browser die Seite.

Weitere Informationen zum Debugger und zum Debuggen von Code in Visual Studio finden Sie unter [Exemplarische Vorgehensweise: Debuggen von Webseiten in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Verwenden von Razor in ASP.NET MVC-Projekte mit Visual Studio

Die Razor-Syntax wird auch umfassend in ASP.NET MVC-Projekten verwendet. MVC ist eine leistungsfähige, auf Mustern basierende Funktionen zum Entwickeln dynamischer Websites. Wenn Ihre ASP.NET Web Pages-Website schwierig zu verwalten ist, empfiehlt es sich, sollten ihn in einer ASP.NET MVC-Anwendung zu konvertieren. Ein Beispiel zum Erstellen einer MVC-Anwendung finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installieren von Visual Studio 2010-Unterstützung für ASP.NET Web Pages

In diesem Abschnitt veranschaulicht, wie Visual Web Developer Express 2010 und die ASP.NET Web Pages-Tools für Visual Studio zu installieren.

1. Wenn Sie bereits über den Webplattform-Installer haben, laden Sie sie von folgender URL ein:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Führen Sie den Webplattform-Installer.
3. Klicken Sie auf die **Produkte** Registerkarte.

    ![Registerkarte "WebPI-Produkte"](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Suchen Sie nach **ASP.NET MVC 4** (für ASP.NET Web Pages 2), und klicken Sie dann auf **hinzufügen**. Diese Produkte sind Visual Studio-Tools zum Erstellen von ASP.NET Razor-Websites.

    ![Optionen der WebPi-Installation](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Klicken Sie auf **installieren** um die Installation abzuschließen.
