---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL-Routing | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 556ef01304d0b5a3cca3606d71ef055ce4b2dc5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389149"
---
<a name="url-routing"></a>URL-Routing
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial ändern Sie die Wingtip Toys-beispielanwendung, um URL-routing zu unterstützen. Routing können Ihre Webanwendung in URLs verwenden, die benutzerfreundliche, leichter zu merken und von Suchmaschinen besser unterstützt werden. Dieses Tutorial baut auf dem vorherigen Lernprogramm "Mitgliedschaft und Verwaltung" und ist Teil der tutorialreihe Wingtip Toys.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Vorgehensweise: Registrieren von Routen für eine ASP.NET Web Forms-Anwendung.
- Das Hinzufügen von Routen zu einer Webseite.
- Auswählen von Daten aus einer Datenbank aus, um Routen zu unterstützen.

## <a name="aspnet-routing-overview"></a>Übersicht über ASP.NET das Routing

URL-routing, können Sie zum Konfigurieren einer Anwendung zum Akzeptieren von Anforderungs-URLs, die nicht auf physischen Dateien zugeordnet sind. Eine Anforderungs-URL ist einfach die URL, die ein Benutzer in ihrem Browser eine Seite auf Ihrer Website sucht eingibt. Sie verwenden routing, um die URLs zu definieren, die semantisch für Benutzer von Bedeutung sind, und suchen-suchmaschinenoptimierung (SEO) unterstützen kann.

Standardmäßig enthält die Web Forms-Vorlage [Friendly URLs von ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Großteil der Arbeit mit grundlegenden routing wird implementiert werden, indem *Friendly URLs*. In diesem Tutorial werden Sie jedoch benutzerdefinierte Routingfunktionen hinzufügen.

Vor dem Anpassen der URL-routing, kann die Wingtip Toys-beispielanwendung mit einem Produkt mit der folgenden URL verknüpfen:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Durch das Anpassen der URL-routing, werden die Wingtip Toys-beispielanwendung ein einfacher, lesen Sie die URL mit dem Produkt verknüpft:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Routen

Eine Route ist ein URL-Muster, das einen Handler zugeordnet ist. Der Handler kann es sich um eine physische Datei, z. B. eine ASPX-Datei in einer Web Forms-Anwendung sein. Ein Ereignishandler kann auch eine Klasse sein, die die Anforderung verarbeitet. Um eine Route definieren, erstellen Sie eine Instanz der Klasse Route durch Angabe der URL-Muster, die Ereignishandler und optional einen Namen für die Route an.

Die Route wird zur Anwendung hinzufügen, durch das Hinzufügen der `Route` Objekt an die statische `Routes` Eigenschaft der `RouteTable` Klasse. Die Routen-Eigenschaft ist eine `RouteCollection` -Objekt, das alle Routen für die Anwendung gespeichert.

### <a name="url-patterns"></a>URL-Muster

Ein URL-Muster kann es sich um Literalwerte und Variable Platzhalter (bezeichnet als URL-Parameter) enthalten. Literalwerte und Platzhalter befinden sich in Segmente der URL durch den Schrägstrich getrennt sind (`/`) Zeichen.

Wenn eine Anforderung an Ihre Web-Anwendung erfolgt, die URL in Segmente und Platzhalter zerlegt wird, und die Variablenwerte werden bereitgestellt, um den Ereignishandler der Anforderung. Dieser Prozess ist ähnlich wie die Daten in einer Abfragezeichenfolge analysiert und an den Ereignishandler der Anforderung übergeben. In beiden Fällen Informationen enthalten, die in der URL und der Handler in Form von Schlüssel-Wert-Paaren übergeben. Sind für Abfragezeichenfolgen beide Schlüssel und die Werte in der URL ein. Für Routen die Schlüssel sind die Platzhalternamen, die in der URL-Muster definiert, und nur die Werte sind in der URL.

In einem URL-Muster, definieren Sie Platzhalter in geschweiften Klammern ( `{` und `}` ). Sie können mehrere Platzhalter in einem Segment definieren, aber die Platzhalter durch einen Literalwert getrennt werden. Z. B. `{language}-{country}/{action}` ist eine gültige Route-Muster. Allerdings `{language}{country}/{action}` ist kein gültiges Muster, da kein Literalwert oder Trennzeichen zwischen den Platzhalter vorhanden ist. Routing kann daher nicht, wo Sie den Wert für den sprachplatzhalter aus dem Wert für den Platzhalter Land zu trennen bestimmen.

### <a name="mapping-and-registering-routes"></a>Zuordnen und Registrieren von Routen

Bevor Sie die Routen zu Seiten, die von das Wingtip Toys-beispielanwendung einfügen können, müssen Sie die Routen registrieren, beim Starten der Anwendung. Um die Routen zu registrieren, ändern Sie die `Application_Start` -Ereignishandler.

1. In **Projektmappen-Explorer**von Visual Studio, suchen und öffnen Sie die *"Global.asax.cs"* Datei.
2. Fügen Sie den Code in Gelb zu markiert die *"Global.asax.cs"* -Datei wie folgt:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wenn die Bezeichnung "Wingtip Toys"-Anwendung gestartet wird Beispiel, ruft er die `Application_Start` -Ereignishandler. Am Ende der Ereignishandler der `RegisterCustomRoutes` Methode wird aufgerufen. Die `RegisterCustomRoutes` Methode fügt jede Route hinzu, durch den Aufruf der `MapPageRoute` Methode der `RouteCollection` Objekt. Routen werden mit einem Routennamen, die Routen-URL und eine physische URL definiert.

Der erste Parameter ("`ProductsByCategoryRoute`") ist der Routenname. Es wird verwendet, um die Route aufgerufen, wenn er benötigt wird. Der zweite Parameter ("`Category/{categoryName}`") definiert die friendly URL, die dynamisch sein kann, auf Code basierend ersetzt. Verwenden Sie diese Route, wenn Sie ein Steuerelement mit Links, die generiert werden, basierend auf Daten auffüllen. Eine Route wird folgendermaßen angezeigt:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Der zweite Parameter der Route enthält einen dynamischen Wert, der von geschweiften Klammern angegeben (`{ }`). In diesem Fall die `categoryName` ist eine Variable, die verwendet wird, um den richtigen Routingpfad zu bestimmen.

> [!NOTE] 
> 
> **Optional**
> 
> Sie finden es vielleicht einfacher zum Verwalten Ihres Codes durch das Verschieben der `RegisterCustomRoutes` Methode, um eine separate Klasse. In der *Logik* Ordner, erstellen Sie eine Separate `RouteActions` Klasse. Verschieben Sie den oben genannten `RegisterCustomRoutes` Methode aus der *"Global.asax.cs"* Datei in das neue `RoutesActions` Klasse. Verwenden der `RoleActions` Klasse und die `createAdmin` Methode als ein Beispiel zum Aufrufen der `RegisterCustomRoutes` Methode aus der *"Global.asax.cs"* Datei.


Sie haben wahrscheinlich ebenfalls bemerkt der `RegisterRoutes` Methode mit der `RouteConfig` Objekt am Anfang der `Application_Start` -Ereignishandler. Dieser Aufruf wird durchgeführt, um Standardrouting zu implementieren. Es war als Standard-Code enthalten, wenn Sie die Anwendung mit Visual Studio Web Forms-Vorlage erstellt haben.

## <a name="retrieving-and-using-route-data"></a>Abrufen und Verwenden von Routendaten

Wie bereits erwähnt, können die Routen definiert werden. Der Code, der Sie hinzugefügt haben die `Application_Start` -Ereignishandler in der *"Global.asax.cs"* -Datei lädt die definierbaren Routen.

### <a name="setting-routes"></a>Festlegen von Routen

Routen müssen Sie zusätzlichen Code hinzufügen. In diesem Tutorial verwenden Sie modellbindung zum Abrufen einer `RouteValueDictionary` -Objekt, das beim Generieren der Routen, die mithilfe von Daten aus einem Steuerelement verwendet wird. Die `RouteValueDictionary` Objekt enthält eine Liste der Produktnamen, die auf eine bestimmte Kategorie von Produkten gehören. Für jedes Produkt, das auf Basis der Daten und die Route wird ein Link erstellt.

#### <a name="enable-routes-for-categories-and-products"></a>Aktivieren von Routen für Kategorien und Produkte

Als Nächstes aktualisieren Sie die Anwendung zur Verwendung der `ProductsByCategoryRoute` um zu bestimmen, die richtige Route für jedes Product Category-Link enthält. Aktualisieren Sie zudem die *"ProductList.aspx"* Seite, um eine geroutete Link für jedes Produkt enthalten. Die Links werden angezeigt wie vor der Änderung jedoch die Links jetzt an die URL-routing verwendet.

1. In **Projektmappen-Explorer**öffnen die *Site.Master* Seite, wenn es nicht bereits geöffnet ist.
2. Update der **ListView** Steuerelement mit dem Namen "`categoryList`" mit den Änderungen in gelb hervorgehoben, sodass das Markup wie folgt angezeigt:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. In **Projektmappen-Explorer**öffnen die *"ProductList.aspx"* Seite.
4. Update der `ItemTemplate` Element der *"ProductList.aspx"* Seite mit den Updates in gelb hervorgehoben werden, damit das Markup wie folgt aussieht:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Öffnen Sie den Code-Behind der *ProductList.aspx.cs* und fügen Sie den folgenden Namespace als Gelb:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Ersetzen Sie die `GetProducts` -Methode der Code-Behind (*ProductList.aspx.cs*) durch den folgenden Code:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Fügen Sie Code für die Produktdetails hinzu.

Aktualisieren Sie nun die Code-Behind (*ProductDetails.aspx.cs*) für die *ProductDetails.aspx* Seite, um die Weiterleitung von Daten verwenden. Beachten Sie, dass die neue `GetProduct` -Methode akzeptiert auch den Wert einer Abfragezeichenfolge für den Fall, in denen der Benutzer einen Link ein Lesezeichen erstellt hat, die die ältere nicht optimierte und nicht-Routing-URL verwendet.

1. Ersetzen Sie die `GetProduct` -Methode der Code-Behind (*ProductDetails.aspx.cs*) durch den folgenden Code:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung nun auf die aktualisierte Routen finden Sie unter ausführen.

1. Drücken Sie **F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Klicken Sie auf die **Produkte** Link am oberen Rand der Seite.  
 Alle Produkte werden angezeigt, auf die *"ProductList.aspx"* Seite. Die folgende URL (unter Verwendung Ihrer Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/ProductList`
3. Klicken Sie anschließend die **Autos** Kategorielink am oberen Rand der Seite.  
 Nur Autos werden angezeigt, auf die *"ProductList.aspx"* Seite. Die folgende URL (unter Verwendung Ihrer Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/Category/Cars`
4. Klicken Sie auf der Link, mit dem Namen des jeweils ersten Fahrzeugs auf der Seite angezeigt ("**konvertierbar sein Auto**") um die Produktdetails anzuzeigen.  
 Die folgende URL (unter Verwendung Ihrer Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Geben Sie anschließend die folgende nicht weitergeleitete URL (unter Verwendung Ihrer Portnummer) in den Browser ein:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Der Code erkennt immer noch eine URL mit einer Abfragezeichenfolge für den Fall, in denen ein Benutzer einen Link ein Lesezeichen erstellt wurde.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie Routen für Kategorien und Produkte hinzugefügt. Sie haben gelernt, wie Routen mit Datensteuerelementen integriert werden können, die modellbindung zu verwenden. Im nächsten Tutorial implementieren Sie globale Fehlerbehandlung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Bereitstellen einer sicheren ASP.NET Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Zurück](membership-and-administration.md)
> [Weiter](aspnet-error-handling.md)
