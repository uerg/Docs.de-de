---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL-Routing | Microsoft Docs
author: Erikre
description: Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: a195b36517bcae4bbeaf43fe7386e7787fd00212
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886432"
---
<a name="url-routing"></a>URL-Routing
====================
by [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm ändern Sie die Wingtip Toys-beispielanwendung für das URL-routing zu unterstützen. Routing können Ihre Webanwendung URLs verwenden, die benutzerfreundliche, leichter merken und wird von Suchmaschinen besser unterstützt werden. Dieses Lernprogramm baut auf dem vorherigen Lernprogramm "Mitgliedschaft und Verwaltung" und ist Teil der Wingtip Toys Reihe von Lernprogrammen.

## <a name="what-youll-learn"></a>Lernen Sie:

- Vorgehensweise: Registrieren von Routen für eine ASP.NET Web Forms-Anwendung.
- Das Hinzufügen von Routen zu einer Webseite.
- Auswählen von Daten aus einer Datenbank zur Unterstützung von Routen.

## <a name="aspnet-routing-overview"></a>Übersicht über ASP.NET

URL-routing können Sie so konfigurieren Sie eine Anwendung akzeptieren Anforderungs-URLs, die keinen physischen Dateien zugeordnet sind. Eine Anforderungs-URL ist einfach die URL, die ein Benutzer in ihrem Browser zum Suchen einer Seite auf Ihrer Website eingibt. Mithilfe von routing um URLs zu definieren, die für Benutzer semantisch sinnvoll sind und zur Entwicklung mit Search Engine Optimization (SEO) beitragen können.

Die Web Forms-Projektvorlage enthält standardmäßig [Friendly URLs von ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Wird ein Großteil der standardarbeiten routing mit implementiert *Friendly URLs*. In diesem Lernprogramm fügen Sie jedoch benutzerdefinierte Routingfunktionen.

Vor dem Anpassen von URL-routing, kann die beispielanwendung des Wingtip Toys mit einem Produkt mit der folgenden URL verknüpfen:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Durch das Anpassen der URL-routing, wird die beispielanwendung des Wingtip Toys demselben Produkt mit einer leichter lesbar URL verknüpfen:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Routen

Eine Route ist ein URL-Muster, das einen Handler zugeordnet ist. Der Handler kann es sich um eine physische Datei, z. B. eine ASPX-Datei in einer Web Forms-Anwendung sein. Ein Ereignishandler kann auch eine Klasse sein, die die Anforderung verarbeitet. Um eine Route zu definieren, erstellen Sie eine Instanz der Klasse Route unter Angabe der URL-Muster, den Handler und optional einen Namen für die Route.

Sie fügen die Route für die Anwendung hinzu, indem die `Route` Objekt, das die statische `Routes` Eigenschaft von der `RouteTable` Klasse. Die Routen-Eigenschaft ist ein `RouteCollection` Objekt, das alle Routen für die Anwendung speichert.

### <a name="url-patterns"></a>URL-Mustern

Ein URL-Muster kann es sich um Literalwerte und Variable Platzhalter (bezeichnet als URL-Parameter) enthalten. Literalwerte und Platzhalter befinden sich in Segmenten, die der URL durch den Schrägstrich getrennt sind (`/`) Zeichen.

Wenn eine Anforderung für Ihre Webanwendung erfolgt, die URL ist in Segmente und Platzhalter analysiert, und die Variablenwerte werden bereitgestellt, um den Anforderungshandler. Dieser Vorgang ist ähnlich wie die Daten in einer Abfragezeichenfolge analysiert und an den Anforderungshandler übergeben. In beiden Fällen wird Variableninformationen in der URL enthalten und an den Handler in Form von Schlüssel-Wert-Paaren übergeben. Sind für Abfragezeichenfolgen die Schlüssel und die Werte in der URL ein. Für Routen die Schlüssel sind die Platzhalternamen, die in der URL-Muster definiert und nur die Werte sind in der URL.

In einem URL-Muster, definieren Sie Platzhalter in geschweiften Klammern ( `{` und `}` ). Sie können mehr als ein Platzhalter in einem Segment definieren, aber die Platzhalter durch einen Literalwert getrennt werden. Beispielsweise `{language}-{country}/{action}` ist eine gültige Route-Muster. Allerdings `{language}{country}/{action}` ist kein gültiges Muster, da keine Literalwert oder als Trennzeichen zwischen den Platzhalter vorhanden ist. Aus diesem Grund kann nicht weiterleiten, wo Sie den Wert für den Platzhalter für die Sprache von den Wert für den Platzhalter Land zu trennen bestimmen.

### <a name="mapping-and-registering-routes"></a>Zuordnen und Registrieren von Routen

Bevor Sie Routen zu den Seiten des Wingtip Toys-beispielanwendung einfügen können, müssen Sie die Routen registrieren, beim Starten der Anwendung. Um die Routen zu registrieren, ändern Sie die `Application_Start` -Ereignishandler.

1. In **Projektmappen-Explorer**von Visual Studio zu suchen und öffnen Sie die *Global.asax.cs* Datei.
2. Fügen Sie den Code in gelb hervorgehoben werden die *Global.asax.cs* Datei wie folgt:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wenn die Wingtip Toys-Anwendung gestartet wird Beispiel, ruft er die `Application_Start` -Ereignishandler. Am Ende dieser Ereignishandler der `RegisterCustomRoutes` -Methode aufgerufen wird. Die `RegisterCustomRoutes` Methode fügt jeder Route durch Aufrufen der `MapPageRoute` Methode der `RouteCollection` Objekt. Routen werden mit einem Routennamen, Routen-URL und eine physische URL definiert.

Der erste Parameter ("`ProductsByCategoryRoute`") ist der Routenname. Hiermit wird die Route aufgerufen wird, wenn er benötigt wird. Der zweite Parameter ("`Category/{categoryName}`") definiert das Anzeigename ersetzen, die URL, die dynamisch sein kann, die auf Code basiert. Verwenden Sie diese Route an, wenn Sie ein Steuerelement mit Links, die generiert werden, basierend auf Daten auffüllen. Eine Route wird folgendermaßen angezeigt:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Der zweite Parameter der Route enthält einen dynamischen Wert, der von geschweiften Klammern angegeben (`{ }`). In diesem Fall die `categoryName` ist eine Variable, die verwendet wird, um den richtigen Routingpfad zu bestimmen.

> [!NOTE] 
> 
> **Optional**
> 
> Sie finden es vielleicht einfacher zu verwalten, den Code durch Verschieben der `RegisterCustomRoutes` Methode, um eine separate Klasse. In der *Logik* Ordner, erstellen Sie eine Separate `RouteActions` Klasse. Verschieben Sie die oben genannten `RegisterCustomRoutes` Methode aus der *Global.asax.cs* Datei in das neue `RoutesActions` Klasse. Verwenden der `RoleActions` Klasse und die `createAdmin` -Methode, wie ein Beispiel zum Aufrufen der `RegisterCustomRoutes` Methode aus der *Global.asax.cs* Datei.


Sie sicherlich auch bemerkt haben die `RegisterRoutes` Methode-Aufruf mithilfe der `RouteConfig` Objekt am Anfang der `Application_Start` -Ereignishandler. Dieser Aufruf erfolgt Standardrouting implementieren. Wenn Sie die Anwendung mit Visual Studio Web Forms-Projektvorlage erstellt war als Standardcode enthalten.

## <a name="retrieving-and-using-route-data"></a>Abrufen und Verwenden von Routendaten

Wie bereits erwähnt, können Routen definiert werden. Der Code, der Sie hinzugefügt haben die `Application_Start` -Ereignishandler in der *Global.asax.cs* Datei lädt die benutzerdefinierbare Routen.

### <a name="setting-routes"></a>Festlegen von Routen

Routen müssen Sie zusätzlichen Code hinzufügen. In diesem Lernprogramm verwenden Sie wurden die modellbindung zum Abrufen einer `RouteValueDictionary` -Objekt, das beim Generieren der Routen mit Daten aus einem Datensteuerelement verwendet wird. Die `RouteValueDictionary` Objekt enthält eine Liste aus Produktnamen besteht, die zu einer bestimmten Kategorie von Produkten gehören. Für jedes Produkt basierend auf den Daten und die Route wird eine Verknüpfung erstellt.

#### <a name="enable-routes-for-categories-and-products"></a>Aktivieren von Routen für die Kategorien und Produkte

Als Nächstes aktualisieren Sie die Anwendung für die Verwendung der `ProductsByCategoryRoute` zum Bestimmen der richtigen Route für jedes Produkt Kategorielink einschließen. Sie müssen außerdem ein update der *"ProductList.aspx"* Seite, um einen gerouteten Link für jedes Produkt enthalten. Die Links wird angezeigt wie vor der Änderung waren jedoch die Links jetzt URL-routing verwendet.

1. In **Projektmappen-Explorer**öffnen die *Site.Master* Seite, wenn er nicht bereits geöffnet ist.
2. Update der **ListView** -Steuerelement mit dem Namen "`categoryList`" mit den Änderungen in gelb hervorgehoben werden, sodass das Markup sieht wie folgt aus:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. In **Projektmappen-Explorer**öffnen die *"ProductList.aspx"* Seite.
4. Update der `ItemTemplate` Element von der *"ProductList.aspx"* Seite mit den Updates in gelb hervorgehoben werden, damit das Markup wie folgt aussieht:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Öffnen Sie den Code-Behind der *ProductList.aspx.cs* und fügen Sie den folgenden Namespace als Gelb hinzu:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Ersetzen Sie die `GetProducts` Methode den Code-Behind (*ProductList.aspx.cs*) durch den folgenden Code:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Fügen Sie Code für die Produktdetails hinzu.

Aktualisieren Sie nun den Code-Behind (*ProductDetails.aspx.cs*) für die *ProductDetails.aspx* Seite Routendaten verwendet. Beachten Sie, dass die neue `GetProduct` akzeptiert die Methode auch einen Wert der Abfragezeichenfolge für den Fall, in dem der Benutzer eine Verbindung mit einem Lesezeichen versehene verfügt, die die ältere nicht benutzerfreundlichen, nicht gerouteten-URL verwendet.

1. Ersetzen Sie die `GetProduct` Methode den Code-Behind (*ProductDetails.aspx.cs*) durch den folgenden Code:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung nun auf die aktualisierte Routen finden Sie unter ausführen.

1. Drücken Sie **F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Klicken Sie auf die **Produkte** Link am oberen Rand der Seite.  
 Alle Produkte werden angezeigt, auf die *"ProductList.aspx"* Seite. Die folgende URL (mit der Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/ProductList`
3. Klicken Sie anschließend auf die **Autos** Kategorielink am oberen Rand der Seite.  
 Nur Autos werden angezeigt, auf die *"ProductList.aspx"* Seite. Die folgende URL (mit der Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/Category/Cars`
4. Klicken Sie auf der Link, mit dem Namen des ersten Autos auf der Seite angezeigt ("**konvertierbar Car**") auf die Produktdetails anzuzeigen.  
 Die folgende URL (mit der Portnummer) wird für den Browser angezeigt:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Geben Sie anschließend die folgenden nicht gerouteten-URL (mit der Portnummer) in den Browser:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Der Code erkennt noch eine URL, eine Abfragezeichenfolge für die Groß-/Kleinschreibung enthält, in denen ein Benutzer eine Verbindung mit einem Lesezeichen versehene verfügt.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie Routen für Kategorien und Produkte hinzugefügt. Sie haben gelernt, wie Routen mit Datensteuerelementen integriert werden können, die modellbindung verwenden. In den nächsten Lernprogrammen implementieren Sie die globalen Fehlerbehandlung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Bereitstellen von Mitgliedschaft, OAuth und SQL-Datenbank eine sichere ASP.NET Web Forms-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - Testversion](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Zurück](membership-and-administration.md)
> [Weiter](aspnet-error-handling.md)
