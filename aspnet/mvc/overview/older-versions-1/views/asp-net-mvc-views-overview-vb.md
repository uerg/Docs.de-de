---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC-Ansichten (Übersicht) (VB) | Microsoft Docs
author: StephenWalther
description: Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet sie von einer HTML-Seite? In diesem Lernprogramm Stephen Walther Ansichten erläutert und veranschaulicht, wie können Sie t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-views-overview-vb"></a>ASP.NET MVC-Ansichten (Übersicht) (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet sie von einer HTML-Seite? In diesem Lernprogramm Stephen Walther Ansichten erläutert und veranschaulicht, wie Sie Daten anzeigen und HTML-Hilfsmethoden innerhalb einer Ansicht nutzen können.


Der Zweck dieses Lernprogramms werden Sie eine kurze Einführung in ASP.NET MVC-Ansichten, Anzeigen von Daten und HTML-Hilfsmethoden zur Verfügung stellen. Am Ende dieses Lernprogramms sollten Sie verstehen, wie neue Ansichten erstellen, übergeben von Daten von einem Domänencontroller zu einer Ansicht und Verwenden von HTML-Hilfsmethoden zum Generieren des Inhalts in einer Ansicht angezeigt werden.

## <a name="understanding-views"></a>Grundlegendes zu Sichten

Im Gegensatz zu ASP.NET oder Active Server Pages schließt ASP.NET MVC nicht nichts ein, die direkt auf eine Seite entspricht. In einer ASP.NET MVC-Anwendung besteht keine Seite auf dem Datenträger, der den Pfad in der URL entspricht, die Sie in der Adressleiste des Browsers eingeben. Am ehesten auf eine Seite in einer ASP.NET MVC-Anwendung ist etwas bezeichnet eine *Ansicht*.

In einer ASP.NET MVC-Anwendung werden eingehende Browseranforderungen Controlleraktionen zugeordnet. Eine Controlleraktion könnte eine Sicht zurückgeben. Eine Controlleraktion führen jedoch möglicherweise aus einen anderen Typ der Aktion, z. B. umleiten Sie zu einem anderen Controlleraktion.

Auflisten von 1 enthält einen einfachen Controller mit dem Namen der HomeController. HomeController macht zwei benannte Index() und Details() Controlleraktionen verfügbar.

**1 – HomeController.vb auflisten**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

Sie können die erste Aktion, die Aktion Index() aufrufen, indem Sie die folgende URL in der Adressleiste des Browsers eingeben:

/Home/Index

Sie können die zweite Aktion, die Aktion Details() aufrufen, indem Sie diese Adresse in Ihren Browser eingeben:

/Home/Details

Die Aktion Index() gibt eine Ansicht zurück. Die meisten Aktionen, die Sie erstellen werden Sichten zurückgegeben. Sie können jedoch eine Aktion andere Arten von Aktionsergebnisse zurückgeben. Die Details()-Aktion gibt z. B. eine RedirectToActionResult, die eingehende Anforderung an die Aktion Index() umleitet.

Die Aktion Index() enthält die folgende Codezeile:

View()

Diese Codezeile gibt eine Sicht, die sich unter folgendem Pfad auf dem Webserver sein müssen:

\Views\Home\Index.aspx

Der Pfad zur Ansicht ist von den Namen des Controllers und der Name der Controlleraktion abgeleitet.

Falls gewünscht, können Sie zur Ansicht explizit sein. Die folgende Codezeile gibt eine Ansicht mit dem Namen Fred zurück:

Anzeigen (Fred)

Wenn diese Codezeile ausgeführt wird, wird eine Sicht aus dem folgenden Pfad zurückgegeben:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Wenn Sie planen, erstellen Sie Komponententests für ASP.NET MVC-Anwendung ist es eine gute Idee, explizit zu Sichtnamen sein. Auf diese Weise können Sie einen Komponententest, um sicherzustellen, dass die erwarteten Ansicht durch eine Controlleraktion zurückgegebener erstellen.


## <a name="adding-content-to-a-view"></a>Hinzufügen von Inhalt zu einer Ansicht

Eine Sicht ist ein Standard (X) HTML-Dokument, die Skripts enthalten kann. Sie können Skripts verwenden, dynamische Inhalte zu einer Sicht hinzufügen.

Beispielsweise zeigt die Ansicht im Codebeispiel 2 das aktuelle Datum und die Uhrzeit.

**Listing 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

Beachten Sie, dass der Text der HTML-Seite in 2 auflisten das folgende Skript enthält:

&lt;"% Response.Write(DateTime.Now)"&gt;

Verwenden Sie das Skript-Trennzeichen &lt;"und" %&gt; um Anfang und Ende eines Skripts zu markieren. Dieses Skript wird in Visual Basic geschrieben. Es zeigt das aktuelle Datum und die Uhrzeit an, durch Aufrufen der Methode Response.Write() für das Rendern von Inhalt an den Browser. Die Skript-Trennzeichen &lt;"und" %&gt; können verwendet werden, um eine oder mehrere Anweisungen ausführen.

Da Sie Response.Write() so häufig aufrufen, bietet Microsoft eine Verknüpfung zum Aufrufen der Response.Write()-Methode. Die Ansicht im Codebeispiel 3 verwendet die Trennzeichen &lt;% = "und" %&gt; als Kurzform für Response.Write() aufrufen.

**3 – Views\Home\Index2.aspx auflisten**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

Jeder können Sie um dynamische Inhalte in einer Ansicht zu generieren. Normalerweise ll Sie verwenden Visual Basic .NET oder C#-, um Ihre Controller und Ansichten zu schreiben.

## <a name="using-html-helpers-to-generate-view-content"></a>Verwenden von HTML-Hilfsmethoden zum Generieren von Inhalt anzeigen

Hinzufügen von Inhalt zu einer Sicht zu vereinfachen, profitieren Sie von so genannte ein *HTML-Hilfsobjekt*. Ein HTML-Hilfsobjekt, ist im Allgemeinen eine Methode, die eine Zeichenfolge generiert. Sie können HTML-Hilfsmethoden zum Generieren von standardmäßigen HTML-Elemente, z. B. Textfelder, Links, Dropdownlisten und Listenfelder verwenden.

Beispielsweise die Ansicht im Codebeispiel 4 nutzt drei HTML-Hilfsmethoden--bilden die Hilfsprogramme BeginForm(), TextBox() und Password() – um eine Anmeldung zu generieren (siehe Abbildung 1).

**Listing 4 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


[![Das Dialogfeld "Neues Projekt"](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Abbildung 01**: ein Anmeldename-Standardformat ([klicken Sie hier, um das Bild in voller Größe angezeigt](asp-net-mvc-views-overview-vb/_static/image2.png))


Alle Methoden, HTML-Hilfsmethoden sind in der Eigenschaft Html der Ansicht aufgerufen. Rendern Sie beispielsweise ein Textfeld durch Aufrufen der Html.TextBox()-Methode.

Beachten Sie, dass Sie die Skript-Trennzeichen verwenden &lt;% = "und" %&gt; beim Aufrufen der Html.TextBox() und die Html.Password() Hilfsprogramme. Diese Hilfsprogramme werden einfach eine Zeichenfolge zurück. In diesem Fall müssen Sie eine Response.Write() aufrufen, um die Zeichenfolge an den Browser zu rendern.

Mit HTML-Hilfsmethoden ist optional. Sie Ihnen das Leben erleichtern durch Reduzieren der HTML- und Skripts, die Sie schreiben müssen. Die Ansicht im Codebeispiel 5 rendert die genaue dasselbe Format wie die Ansicht im Codebeispiel 4 ohne Verwendung von HTML-Hilfsmethoden.

**Listing 5 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

Sie haben auch die Möglichkeit, eigene HTML-Hilfsmethoden zu erstellen. Beispielsweise können Sie eine Hilfsmethode GridView() erstellen, die einen Satz von Datenbankdatensätzen automatisch in einer HTML-Tabelle anzeigt. In diesem Thema im Lernprogramm werden untersucht **Erstellen von benutzerdefinierten HTML-Hilfsprogrammen**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Mithilfe der Ansichtsdaten auf Daten an eine Ansicht zu übergeben

Sie verwenden die Ansichtsdaten, um Daten von einem Controller an eine Ansicht zu übergeben. Stellen Sie Anzeigedaten, wie ein Paket, das Sie über die e-Mail zu senden. Alle Daten, die von einem Controller an eine Ansicht übergeben müssen mit diesem Paket gesendet werden. Der Controller im Codebeispiel 6 fügt z. B. eine Meldung, um Daten anzuzeigen.

**6 – ProductController.vb auflisten**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

Der Controller ViewData-Eigenschaft stellt eine Auflistung von Name-Wert-Paaren dar. Auflisten von 6 fügt die Index()-Methode ein Element auf die Ansicht Daten-Auflistung, die mit dem Namen Nachricht mit dem Wert Hello World!. Wenn die Ansicht durch die Index()-Methode zurückgegeben wird, werden automatisch die Ansichtsdaten in die Ansicht übergeben.

Die Ansicht im Codebeispiel 7 Ruft die Nachricht aus den Ansichtsdaten ab und rendert die Nachricht an den Browser.

**Listing 7 -- \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Beachten Sie, dass die Sicht die Html.Encode() HTML-Hilfsmethode beim Rendern der Nachrichteninhalts nutzt. Das HTML-Hilfsobjekt Html.Encode() codiert Sonderzeichen wie z. B. &lt; und &gt; in Zeichen, die sicher auf einer Webseite angezeigt werden. Wenn Sie Inhalt nicht, die ein Benutzer auf einer Website sendet rendern, sollten Sie den Inhalt, der JavaScript-Injection-Angriffe zu verhindern, dass codieren.

(Da die Nachricht selbst in den ProductController erstellt haben, werden wir Verschlüsselungskennwort t zum Codieren der Nachricht eigentlich benötigen. Es ist jedoch Gewohnheit immer die Html.Encode()-Methode aufgerufen wird, beim Abrufen der Anzeigen des Inhalts von Daten in einer Ansicht anzeigen.)

Auflisten von 7 nutzte wir Anzeigedaten, um eine einfache Zeichenfolge-Nachricht von einem Controller an eine Ansicht zu übergeben. Anzeigen von Daten können auch um andere Arten von Daten, z. B. eine Auflistung von Datenbank-Datensätze von einem Domänencontroller zu einer Ansicht zu übergeben. Z. B. wenn der Inhalt der Datenbanktabelle Produkte in einer Ansicht angezeigt werden sollen, und klicken Sie dann die Auflistung der Datenbank zu übergeben, würde Datensätze in der Sicht Daten.

Sie haben auch die Möglichkeit, stark typisierten Ansichtsdaten aus einem Controller an eine Ansicht übergeben. In diesem Thema im Lernprogramm werden untersucht **Grundlegendes zu stark typisierte Daten anzeigen und Ansichten**.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm bereitgestellten eine kurze Einführung in ASP.NET MVC-Ansichten, Anzeigen von Daten und HTML-Hilfsmethoden. Im ersten Abschnitt haben Sie gelernt, wie neue Ansichten zu Ihrem Projekt hinzugefügt wird. Sie haben gelernt, dass Sie eine Ansicht und den richtigen Ordner hinzufügen, um über einen bestimmten Controller aufrufen. Als Nächstes besprochen haben wir das Thema des HTML-Hilfsmethoden. Sie haben gelernt, wie HTML-Hilfsmethoden einfach generiert werden standard-HTML-Inhalt aktivieren. Schließlich haben Sie gelernt, wie das Nutzen von Anzeigedaten, um Daten von einem Controller an eine Ansicht zu übergeben.

> [!div class="step-by-step"]
> [Zurück](passing-data-to-view-master-pages-cs.md)
> [Weiter](creating-custom-html-helpers-vb.md)
