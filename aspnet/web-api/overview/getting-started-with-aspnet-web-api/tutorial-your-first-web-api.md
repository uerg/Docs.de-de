---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Erste Schritte mit ASP.NET Web API 2 (c#)
author: MikeWasson
description: HTTP ist nicht nur für Webseiten bereitstellen. Es ist auch eine leistungsstarke Plattform zum Erstellen von APIs, die Dienste und Daten verfügbar machen. HTTP ist einfacher, flexibler, und Ubiq...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="get-started-with-aspnet-web-api-2-c"></a>Erste Schritte mit ASP.NET Web API 2 (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP ist nicht nur für Webseiten bereitstellen. HTTP ist auch eine leistungsstarke Plattform zum Erstellen von APIs, die Dienste und Daten verfügbar machen. HTTP ist einfacher, flexibler und weit verbreitete. Nahezu jede Plattform, der Sie vorstellen können hat eine HTTP-Clientbibliothek aus, sodass HTTP-Diensten eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten herkömmliche desktopanwendungen erreichen können.
 
ASP.NET Web-API ist ein Framework zum Erstellen von Web-APIs auf .NET Framework. In diesem Lernprogramm verwenden Sie ASP.NET Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web-API 2

Finden Sie unter [erstellen Sie eine Web-API mit ASP.NET Core und Visual Studio für Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) für eine neuere Version dieses Lernprogramms.

## <a name="create-a-web-api-project"></a>Erstellen Sie eine Web-API-Projekt

In diesem Lernprogramm verwenden Sie ASP.NET Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt. Die Webseite Front-End-verwendet jQuery, um die Ergebnisse anzuzeigen.

![](tutorial-your-first-web-api/_static/image1.png)

Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET-Webanwendung**. Nennen Sie das Projekt "ProductsApp", und klicken Sie auf **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage. Klicken Sie unter &quot;Hinzufügen von Ordnern und Verweise für core&quot;, überprüfen Sie **Web-API**. Klicken Sie auf **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Sie können auch erstellen, ein Web-API-Projekt mit der &quot;Web-API&quot; Vorlage. Die Web-API-Vorlage verwendet ASP.NET MVC, um API-Hilfeseiten zu ermöglichen. Ich bin die leere Vorlage für dieses Lernprogramm verwenden, da Web-API ohne MVC angezeigt werden soll. Im Allgemeinen müssen Sie nicht wissen, ASP.NET MVC, Web-API verwenden.


## <a name="adding-a-model"></a>Hinzufügen eines Modells

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. ASP.NET Web-API können automatisch das Modell, um JSON-, XML- oder ein anderes Format zu serialisieren und Schreiben Sie die serialisierten Daten in den Text der HTTP-Antwortnachricht. Solange ein Client das Serialisierungsformat lesen kann, können sie das Objekt deserialisieren. Die meisten Clients können Sie XML oder JSON analysiert werden. Darüber hinaus kann der Client der Formatelemente anzugeben durch Festlegen des Accept-Headers in der HTTP-Anforderungsnachricht.

Zunächst erstellen ein einfaches Modell, das ein Produkt darstellt.

Wenn der Projektmappen-Explorer nicht sichtbar ist, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**. Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models. Wählen Sie im Kontextmenü der **hinzufügen** wählen Sie dann **Klasse**.

![](tutorial-your-first-web-api/_static/image4.png)

Benennen Sie die Klasse &quot;Produkt&quot;. Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Hinzufügen eines Controllers

In der Web-API eine *Controller* ist ein Objekt, das HTTP-Anforderungen verarbeitet. Wir werden einen Controller hinzufügen, der eine Liste mit Produkten oder ein einzelnes Produkt, das durch ID angegebene zurückgeben

> [!NOTE]
> Wenn Sie ASP.NET MVC verwendet haben, sind Sie bereits mit Domänencontrollern vertraut. Web-API-Controller mit MVC-Controller vergleichbar sind, aber erben die **ApiController** -Klasse statt der **Controller** Klasse.

In **Projektmappen-Explorer**, mit der rechten Maustaste in des Ordners Controllers. Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.

![](tutorial-your-first-web-api/_static/image5.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Web-API-Controller - leere**. Klicken Sie auf **Hinzufügen**.

![](tutorial-your-first-web-api/_static/image6.png)

In der **Controller hinzufügen** Dialogfeld Namen des Controllers &quot;ProductsController&quot;. Klicken Sie auf **Hinzufügen**.

![](tutorial-your-first-web-api/_static/image7.png)

Das Gerüst erstellt eine Datei namens ProductsController.cs im Ordner "Controller".

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Sie müssen Ihren Domänencontrollern in einen Ordner namens Controller gestellt. Der Ordnername ist eine komfortable Methode, um Ihre Quelldateien zu organisieren.


Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen. Ersetzen Sie den Code in dieser Datei durch Folgendes:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Um das Beispiel einfach zu halten, werden Produkte in einem Array fester Größe innerhalb der Controllerklasse gespeichert. In einer realen Anwendung würde Sie natürlich Abfragen einer Datenbank oder einer anderen externen Datenquelle verwenden.

Der Controller definiert zwei Methoden, die Produkte zurückgibt:

- Die `GetAllProducts` Methodenrückgabe die gesamte Liste der Produkte als ein **IEnumerable&lt;Produkt&gt;**  Typ.
- Die `GetProduct` Methode sucht ein einzelnes Produkt nach seiner ID.

Das ist alles! Sie verfügen über eine funktionierende-Web-API. Jede Methode auf dem Controller entspricht eine oder mehrere URIs:

| Controllermethode | URI |
| --- | --- |
| GetAllProducts | /api/products |
| GetProduct | /api/products/*id* |

Für die `GetProduct` -Methode, die *Id* im URI ist ein Platzhalter. Um das Produkt mit der ID 5 zu erhalten, ist der URI z. B. `api/products/5`.

Weitere Informationen dazu, wie die Web-API-HTTP-Anforderungen an Controllermethoden weiterleitet, finden Sie unter [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Aufrufen der Web-API mit Javascript und jQuery

In diesem Abschnitt fügen wir eine HTML-Seite, die von AJAX verwendet wird, um die Web-API aufzurufen. Wir verwenden jQuery die AJAX-Aufrufe ausführen und die Seite mit den Ergebnissen zu aktualisieren.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen**, und wählen Sie dann **neues Element**.

![](tutorial-your-first-web-api/_static/image9.png)

In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Web** Knoten unter **Visual C#-**, und wählen Sie dann die **HTML-Seite** Element. Nennen Sie die Seite &quot;"Index.HTML"&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Ersetzen Sie alles in dieser Datei durch Folgendes:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Es gibt mehrere Möglichkeiten, jQuery zu erhalten. In diesem Beispiel verwendet die [Microsoft Ajax-CDN](../../../ajax/cdn/overview.md). Sie können auch herunterladen von [ http://jquery.com/ ](http://jquery.com/), und das ASP.NET "Web-API"-Projektvorlage enthält auch jQuery.

### <a name="getting-a-list-of-products"></a>Abrufen einer Liste von Produkten

Um eine Liste der Produkte zu erhalten, senden Sie eine HTTP GET-Anforderung an &quot;/api/Products&quot;.

Die jQuery [GetJSON](http://api.jquery.com/jQuery.getJSON/) -Funktion sendet eine AJAX-Anforderung. Für die Antwort Array von JSON-Objekten enthält. Die `done` -Funktion gibt an, einen Rückruf, der aufgerufen wird, wenn die Anforderung erfolgreich ist. Der Rückruf aktualisieren wir das DOM mit den Produktinformationen.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Abrufen eines Produkts nach ID

Um ein Produkt nach ID zu erhalten, senden Sie eine HTTP GET-Anforderung an &quot;/API/Produkte/*Id*&quot;, wobei *Id* wird die Produkt-ID

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Wir rufen Sie immer noch `getJSON` , dieses Mal jedoch die AJAX-Anforderung senden wir uns die ID im Anforderungs-URI konzentrieren. Die Antwort vom diese Anforderung ist eine JSON-Darstellung eines einzelnen Produkts.

## <a name="running-the-application"></a>Ausführen der Anwendung

Drücken Sie F5, um die Anwendung zu debuggen. Die Webseite sollte wie folgt aussehen:

![](tutorial-your-first-web-api/_static/image11.png)

Um ein Produkt nach ID zu erhalten, geben Sie die ID aus, und klicken Sie auf Suchen:

![](tutorial-your-first-web-api/_static/image12.png)

Wenn Sie eine ungültige ID eingeben, gibt der Server einen HTTP-Fehler zurück:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Verwenden zum Anzeigen des HTTP-Anforderung und Antwort F12

Bei der Arbeit mit einem HTTP-Dienst kann das finden in der HTTP-Anforderung und die Anforderungsnachrichten sehr nützlich sein. Sie erreichen dies, indem Sie die F12-Entwicklungstools in Internet Explorer 9. Drücken Sie in Internet Explorer 9, **F12** zu den Tools zu öffnen. Klicken Sie auf die **Netzwerk** Registerkarte, und drücken Sie **starten erfassen**. Wechseln Sie nun zurück an die Webseite, und drücken Sie **F5** auf der Webseite erneut geladen. Internet Explorer wird den HTTP-Datenverkehr zwischen dem Browser und dem Webserver erfassen. Die Zusammenfassungsansicht zeigt den Netzwerkdatenverkehr für eine Seite an:

![](tutorial-your-first-web-api/_static/image14.png)

Suchen Sie den Eintrag für den relativen URI "api-Produkte /". Wählen Sie diesen Eintrag, und klicken Sie auf **wechseln Sie zur detaillierten Ansicht**. In der Detailansicht sind Registerkarten, um die Anforderung und Antwort-Header und Texte anzuzeigen. Angenommen, Sie klicken Sie auf die **Anforderungsheader** Registerkarte können Sie sehen, dass die vom Client angefordert &quot;Application/Json&quot; im Accept-Header.

![](tutorial-your-first-web-api/_static/image15.png)

Wenn Sie auf der Registerkarte "Text Antwort" klicken, können Sie sehen, wie die Produktliste in JSON serialisiert wurde. Andere Browser verwenden eine ähnliche Funktionalität. Ist ein weiteres nützliches Tool [Fiddler](http://www.fiddler2.com/fiddler2/), eine Web debugging-Proxy. Sie können Fiddler an den HTTP-Verkehr sowie zum Verfassen von HTTP-Anforderungen, die vollständige Kontrolle über die HTTP-Header in der Anforderung enthält.

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App auf Azure ausgeführt wird

Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.

## <a name="next-steps"></a>Nächste Schritte

- Ein ausführlicheres Beispiel für einen HTTP-Dienst, POST, PUT und DELETE-Aktionen unterstützt und schreibt in einer Datenbank, finden Sie unter [mithilfe von Web-API 2 mit Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Weitere Informationen zum Erstellen von flüssig und schnell reagierend Webanwendungen über einen HTTP-Dienst finden Sie unter [ASP.NET einseitigen Anwendung](../../../single-page-application/index.md).
- Informationen zum Bereitstellen einer Visual Studio-Webprojekt nach Azure App Service finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
