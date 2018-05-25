---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Neuigkeiten in Web Forms in ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: Die neue Version der ASP.NET Web Forms stellt eine Reihe von Verbesserungen konzentriert sich auf die benutzererfahrung verbessern, bei der Arbeit mit Daten. In früheren Versionen von...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: e230faac0dc81b67d74945dc98eee80f83205f65
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Was ist neu in WebForms in ASP.NET 4.5
====================
Durch [Web Lager Team](https://twitter.com/webcamps)

> Die neue Version der ASP.NET Web Forms stellt eine Reihe von Verbesserungen konzentriert sich auf die benutzererfahrung verbessern, bei der Arbeit mit Daten.
> 
> In früheren Versionen von Web Forms, wenn die Datenbindung verwenden, zum Ausgeben von des Wert von einem Objektelement und verwendet Sie Datenbindungsausdrücke Bind() oder Eval(). In der neuen Version von ASP.NET sind Sie deklarieren, welche Art von Daten ein Steuerelements durch Verwendung einer neuen ItemType-Eigenschaft gebunden werden soll. Durch Festlegen dieser Eigenschaft wird eine stark typisierte Variable zu verwenden, um die Vorteile der Visual Studio-Entwicklungsumgebung, z. B. IntelliSense, Member-Navigation und kompilierzeitüberprüfung empfangen werden.
> 
> Mit den von datengebundenen Steuerelementen können Sie jetzt auch eigene benutzerdefinierte Methoden zum auswählen, aktualisieren, löschen und Einfügen von Daten, angeben die Interaktion zwischen der Steuerelemente der Seite und die Anwendungslogik vereinfachen. Darüber hinaus haben Modell Bindungsfunktionen wurde für ASP.NET hinzugefügt Dies bedeutet, dass Sie Daten aus der Seite direkt in den Typ der Methodenparameter zuordnen können.
> 
> Validieren von Benutzereingaben sollte auch mit der neuesten Version von Web Forms einfacher sein. Sie können jetzt Ihren Modellklassen mit Validierungsattribute aus Versehen der **System.ComponentModel.DataAnnotations** Namespace und Anforderung, die steuert, alle Ihre Website eine Benutzereingabe, die Informationen zu überprüfen. Die clientseitige Validierung in Web Forms ist jetzt in jQuery, gibt cleaner clientseitigen Code und unaufdringliches JavaScript-Funktionen integriert.
> 
> Im Bereich Anforderung Validierung haben wurden Verbesserungen vorgenommen, damit sie leichter selektiv anforderungsüberprüfung für bestimmte Teile der Anwendung deaktivieren, oder Lesen für ungültig erklärten Anforderungsdaten verwendet werden.
> 
> Einige in Web Forms Nutzen der neuen Funktionen von HTML5-Steuerelemente wurden Verbesserungen vorgenommen:
> 
> - Die TextMode-Eigenschaft des Textfeld-Steuerelements wurde aktualisiert, um die neuen HTML5-Eingabetypen wie e-Mail, "DateTime" und So weiter zu unterstützen.
> - Das FileUpload-Steuerelement unterstützt jetzt mehrere Dateiuploads in Browsern, die mit Unterstützung für diese HTML5-Funktion.
> - Validierungssteuerelement steuert jetzt Unterstützung validierenden HTML5 Eingabeelemente.
> - Neue HTML5-Elemente, die Attribute verfügen, die eine URL jetzt darstellen unterstützen Runat =&quot;Server&quot;. Daher können Sie ASP.NET Konventionen in der URL-Pfade sein, z. B. der ~-Operator zum Stammverzeichnis der Anwendung darstellen (z. B. &lt;video Runat =&quot;Server&quot; Src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - UpdatePanel-Steuerelements wurde zur Unterstützung der Buchung HTML5 Eingabefelder behoben.
> 
> In der offiziellen ASP.NET-Portal finden Sie weitere Beispiele für die neuen Funktionen in ASP.NET Web Forms 4.5: [Neuigkeiten in ASP.NET 4.5 und Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Verwenden von stark typisierten Datenbindungsausdrücke
- Verwenden Sie neue Modell Bindungsfunktionen in Web Forms
- Verwenden Sie Wertanbieter für die Zuordnung von Daten der Seite zu Code-Behind-Methoden
- Verwenden von Datenanmerkungen für die Validierung von Benutzereingaben
- Nehmen Sie Advange Unobstrusive die clientseitige Validierung mit jQuery in Web Forms
- Differenzierte Anforderungsvalidierung implementieren
- Implementieren Sie asynchrone Verarbeitung in Web Forms-Seite

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Übung 1: Der Modellbindung in ASP.NET Web Forms](#Exercise1)
2. [Übung 2: Überprüfen von Daten](#Exercise2)
3. [Übung 3: Asynchrone Seite Verarbeitung in der ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Übung 1: Der Modellbindung in ASP.NET Web Forms

Die neue Version der ASP.NET Web Forms stellt eine Reihe von Verbesserungen verbessern die Erfahrung beim Arbeiten mit Daten konzentrieren. Während dieser Übung werden Sie erfahren Sie mehr über die stark typisierte Datensteuerelemente und der modellbindung.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Aufgabe 1: Verwenden von stark typisierten Daten-Bindungen

In dieser Aufgabe werden neue stark typisierte Bindungen in ASP.NET 4.5 verfügbar ermittelt werden.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex1-ModelBinding/Begin/** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **Customers.aspx** Seite. Platzieren Sie eine nicht nummerierte Liste im Haupt-Steuerelement, und enthalten Sie einen Repeater innerhalb für jeden Kunden auflisten. Legen Sie den Namen des wiederholungsmoduls auf **CustomersRepeater** wie im folgenden Code gezeigt.

    In früheren Versionen von Web Forms Wenn die Datenbindung verwenden, zum Ausgeben von des Wert eines Elements in einem Objekt du Datenbindung, würden Sie einen Datenbindungsausdruck zusammen mit einem Aufruf der Eval-Methode übergeben den Namen der Member verwenden, als Zeichenfolge.

    Zur Laufzeit werden dieser Aufrufe Eval mithilfe von Reflektion und das derzeit gebundene Objekt um den Wert des Elements mit dem angegebenen Namen zu lesen, und das Ergebnis in HTML angezeigt. Dieser Ansatz erleichtert an Daten für beliebige, unshaped Daten gebunden werden soll.

    Leider, verlieren Sie viele der problembehandlungserlebnis Entwicklungszeit Funktionen in Visual Studio, einschließlich IntelliSense für Elementnamen, die Unterstützung für die Navigation (z. B. Gehe zu Definition), und Zeitpunkt der Kompilierung überprüft.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Öffnen der **Customers.aspx.cs** Datei.
4. Fügen Sie die folgende Anweisung verwenden.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. In der **Seite\_laden** -Methode, fügen Sie Code zum Auffüllen der Repeater mit der Liste der Kunden.

    (Codeausschnitt - *Web Forms Lab - Ex01 - Bindung Kunden Datenquelle*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Die Lösung verwendet EntityFramework zusammen mit CodeFirst erstellen und auf die Datenbank zugreifen. Im folgenden Code wird die CustomersRepeater an eine materialisierte Abfrage gebunden, die alle Kunden aus der Datenbank zurückgibt.
6. Drücken Sie **F5** , führen Sie die Projektmappe, und fahren mit der **Kunden** Bild, um die Repeater in Aktion anzuzeigen. Wie die Lösung CodeFirst verwendet wird, wird die Datenbank erstellt und in der lokalen Instanz von SQL Express aufgefüllt werden, wenn die Anwendung ausgeführt.

    ![Auflisten der Kunden mit einer Repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "der Kunden mit einer Repeater auflisten")

    *Die Kunden mit ein Repeater auflisten*

    > [!NOTE]
    > In Visual Studio 2012 ist IIS Express der Standardwebserver für die Entwicklung.
7. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.
8. Ersetzen Sie nun die Implementierung, um stark typisierte Bindungen verwendet werden. Öffnen der **Customers.aspx** Seite, und verwenden Sie die neue **ItemType** Attribut im wiederholungsmodul festzulegende der **Kunden** den Typ der Bindung.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Der ItemType-Eigenschaft können Sie zu deklarieren, welche Art von Daten das Steuerelement gebunden werden soll und können Sie mit stark typisierten innerhalb des datengebundenen Steuerelements binden.
9. Ersetzen Sie den Inhalt durch folgenden Code ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Mit den oben genannten Ansätze ein Nachteil ist, dass die Aufrufe an Eval() und Bind() spät gebundene - was bedeutet, dass das Übergeben von Zeichenfolgen, um die Eigenschaftennamen darstellen. Dies bedeutet, dass Sie nicht Intellisense für die Elementnamen, Unterstützung für die Navigation im Code (z. B. Gehe zu Definition), und Zeitpunkt der Kompilierung überprüft Unterstützung erhalten.

    Festlegen der ItemType-Eigenschaft führt dazu, dass zwei neue typisierte Variablen an, die im Rahmen der Datenbindungsausdrücke generiert werden: **Element** und **BindItem**. Sie können diese stark typisierten Variablen verwenden, in der Datenbindungsausdrücke und erhalten von allen Vorteilen des Visual Studio-Entwicklungsumgebung.

    Die &quot; **:** &quot; im Ausdruck verwendeten wird automatisch HTML-codieren die Ausgabe um Sicherheitsprobleme (z. B. Cross-Site scripting Angriffe) zu vermeiden. Diese Notation seit .NET 4 für das Schreiben von Antwort verfügbar war, aber es ist jetzt auch in Datenbindungsausdrücke verfügbar.

    > [!NOTE]
    > Die Item-Member funktioniert für unidirektionale Bindung. Wenn Sie bidirektionale Bindung verwenden möchten die **BindItem** Member.

    ![IntelliSense-Unterstützung in stark typisierten Bindung](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "IntelliSense-Unterstützung in stark typisierten Bindung")

    *IntelliSense-Unterstützung in stark typisierten Bindung*
10. Drücken Sie **F5** , führen Sie die Projektmappe, und wechseln zu der Seite "Kunden", um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren wie erwartet.

    ![Auflisten von Kundendetails](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Kundendetails auflisten")

    *Auflisten von Kundendetails*
11. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Aufgabe 2 – Einführung zum Modellbindung für Webformulare

In früheren Versionen von ASP.NET Web Forms Wenn Sie bidirektionale Datenbindung, vornehmen wollten sowohl abrufen und Aktualisieren von Daten, mussten Sie ein Objekt für die Datenquelle verwendet wird. Dies könnte eine Objektdatenquelle, die eine SQL-Datenquelle, die eine LINQ-Datenquelle usw. sein. Jedoch wenn Ihr Szenario benutzerdefinierter Code erforderlich, für die Behandlung der Daten, mussten die Objektdatenquelle verwenden, und dies birgt geschaltet. Beispielsweise mussten Sie komplexe Typen zu vermeiden, und Sie benötigen, um Ausnahmen zu behandeln, die Validierungslogik ausgeführt.

In der neuen Version von ASP.NET Web Forms unterstützen die datengebundenen Steuerelemente wurden die modellbindung. Dies bedeutet, dass Sie können wählen, aktualisieren, einfügen und Löschmethoden direkt in das datengebundene Steuerelement Logik aus der CodeBehind-Datei oder von einer anderen Klasse aufgerufen.

Informationen zu finden, verwenden Sie eine GridView zum Auflisten von Produktkategorien, die mit dem neuen **SelectMethod** Attribut. Dieses Attribut ermöglicht Ihnen die Angabe eine Methode zum Abrufen der GridView-Daten.

1. Öffnen der **Products.aspx** Seite und enthalten eine **GridView**. Konfigurieren Sie die GridView, wie unten dargestellt, um stark typisierte Bindungen verwenden und Sortieren und paging aktivieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Verwenden Sie die neue **SelectMethod** Attribut so konfigurieren Sie die GridView Aufrufen einer **GetCategories** Methode zur Auswahl der Daten.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Öffnen der **Products.aspx.cs** Code-Behind-Datei, und fügen Sie die folgenden using-Anweisungen.

    (Codeausschnitt - *Web Forms Lab - Ex01 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Fügen Sie einen privaten Member in der **Produkte** Klasse, und weisen Sie eine neue Instanz der **ProductsContext**. Diese Eigenschaft speichert die Entity Framework-Datenkontext, der Ihnen ermöglicht, mit der Datenbank herstellen.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Erstellen einer **GetCategories** Methode, um die Liste der Kategorien, die mit LINQ abzurufen. Die Abfrage enthält die **Produkte** Eigenschaft, sodass die GridView die Menge der Produkte in jeder Kategorie angezeigt werden kann. Beachten Sie, dass die Methode ein raw IQueryable-Objekt, das die Abfrage gibt sein zu einem späteren Zeitpunkt der Lebenszyklus der Seite ausgeführt.

    (Codeausschnitt - *Web Forms Lab - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > In früheren Versionen von ASP.NET Web Forms erforderlich aktivieren sortieren und paging, Ihre eigene Logik Repository in einem Kontext einer Objektdatenquelle mit Ihren eigenen benutzerdefinierten Code schreiben und alle erforderlichen Parameter empfangen. Jetzt, wie die Datenbindungsmethoden können IQueryable-Objekt zurückgeben, und dies eine Abfrage stellt weiterhin um ausgeführt werden, ASP.NET kümmern Ändern der Abfrage hinzufügen die richtige Sortierung und Paging-Parameter.
6. Drücken Sie **F5** Debuggen Sie den Standort aus, und wechseln zur Seite "Produkte". Sollte angezeigt werden, dass die GridView, wobei die Kategorien, die von der GetCategories-Methode zurückgegeben aufgefüllt wird.

    ![Auffüllen einer GridView mit modellbindung](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Auffüllen einer GridView mit modellbindung")

    *Auffüllen einer GridView mit modellbindung*
7. Drücken Sie **UMSCHALT**+**F5** Beenden des Debuggens.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Aufgabe 3 - Wertanbieter in wurden die Modellbindung

Wurden die modellbindung nicht nur ermöglicht es Ihnen, benutzerdefinierte Methoden zum Arbeiten mit Ihrer Daten direkt in das datengebundene Steuerelement angeben, sondern auch zum Zuordnen von Daten aus der Seite in Parametern von diesen Methoden. Wertattribute-Anbieter können Sie auf der Methodenparameter den Wert angeben. Zum Beispiel:

- Steuerelemente auf der Seite "
- Abfragezeichenfolgen-Werte
- Anzeigen von Daten
- Sitzungszustand
- Cookies
- Gesendete Formulardaten.
- Ansichtszustand
- Benutzerdefinierte Wertanbieter werden ebenfalls unterstützt.

Wenn Sie ASP.NET MVC 4 verwendet haben, bemerken Sie, dass die Bindung modellunterstützung ähnlich ist. In der Tat diese Features ASP.NET-MVC entnommen und in verschoben wurden die **System.Web** Assembly als auch in Web Forms verwenden können.

In dieser Aufgabe aktualisieren Sie die GridView, um die Ergebnisse durch die Anzahl der Produkte in jeder Kategorie filtern Filter-Parameter mit modellbindung empfangen.

1. Wechseln Sie zurück zu den **Products.aspx** Seite.
2. Fügen Sie am Anfang der GridView, eine **Bezeichnung** und ein **ComboBox** die Anzahl der Produkte für die einzelnen Kategorien auswählen, wie unten dargestellt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Hinzufügen einer **EmptyDataTemplate** an die GridView, eine Meldung angezeigt, wenn keine Kategorien mit den ausgewählten Anzahl der Produkte vorhanden sind.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Öffnen der **Products.aspx.cs** Code-Behind, und fügen Sie die folgende Anweisung.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Ändern der **GetCategories** um eine ganze Zahl zu erhalten **MinProductsCount** Argument und die zurückgegebenen Ergebnisse zu filtern. Ersetzen Sie dazu die Methode durch den folgenden Code ein.

    (Codeausschnitt - *Web Forms Lab - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Die neue **[Steuerelement]** -Attribut auf die **MinProductsCount** Argument kennen, der Wert muss aufgefüllt sein, ein Steuerelement auf der Seite mit ASP.NET kann davon betroffen ist. ASP.NET wird für jedes Steuerelement, das entsprechend dem Namen des Arguments (MinProductsCount) suchen und führen Sie die erforderliche Zuordnung und die Konvertierung in den Parameter mit der Control-Wert zu füllen.

    Das Attribut bietet auch einen überladenen Konstruktor, der Ihnen ermöglicht, die das Steuerelement über den den Wert abgerufen werden soll.

    > [!NOTE]
    > Eines der Ziele von dem Datenbindung-Funktionen ist die Menge des Codes zu reduzieren, die für die Interaktion von Seite geschrieben werden muss. Abgesehen von den Wertanbieter [Steuerelement] können Sie andere modellbindungs-Anbieter in Ihrer Methodenparameter. Einige davon sind in der Einführung Aufgabe aufgeführt.
6. Drücken Sie **F5** Debuggen Sie den Standort aus, und wechseln zur Seite "Produkte". Wählen Sie eine Zahl von Produkten in der Dropdown-Liste, und beachten Sie, wie die GridView jetzt aktualisiert wird.

    ![Filtern die GridView mit der ein Wert aus der Dropdown-Liste](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView mit der ein Wert aus der Dropdown-Liste filtern")

    *Filtern die GridView mit der ein Wert aus der Dropdown-Liste*
7. Beenden des Debuggens.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Aufgabe 4: mit Modellbindung für das Filtern

In dieser Aufgabe fügen Sie eine zweite, untergeordneten GridView, die Produkte in der ausgewählten Kategorie angezeigt.

1. Öffnen der **Products.aspx** Seite, und aktualisieren Sie die Kategorien GridView Taste automatisch generiert.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Fügen Sie eine zweite **GridView** mit dem Namen **ProductsGrid** unten. Festlegen der **ItemType** auf **WebFormsLab.Model.Product**, die **DataKeyNames** auf **"ProductID"** und die **SelectMethod**  auf **GetProducts**. Legen Sie **AutoGenerateColumns** auf **"false"** und fügen Sie die Spalten ProductId, ProductName, Beschreibung und UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Öffnen der **Products.aspx.cs** Code-Behind-Datei. Implementieren der **GetProducts** -Methode erhalten von der Kategorie "GridView" die Kategorie-ID und die Produkte zu filtern. Wurden die modellbindung wird legen Sie den Wert des Parameters mit der ausgewählten Zeile in der **CategoriesGrid**. Da die Argumentnamen und Steuerelementname nicht übereinstimmen, sollten Sie den Namen des Steuerelements in der Wertanbieter Steuerelement angeben.

    (Codeausschnitt - *Web Forms Lab - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Dieser Ansatz erleichtert die Einheit diese Testmethoden. Für einen Komponententest Test-Kontext, in denen Web Forms nicht ausgeführt wird, wird das [Steuerelementattribut] keine bestimmte Aktion ausgeführt.
4. Öffnen der **Products.aspx** Seite, und suchen Sie die Produkte GridView. Aktualisieren Sie die Produkte GridView, um einen Link zum Bearbeiten des ausgewählten Produkts anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Öffnen der **ProductDetails.aspx** Code-Behind-Seite, und Ersetzen Sie die **SelectProduct** -Methode durch folgenden Code.

    (Codeausschnitt - *Forms Lab - Ex01 - SelectProduct-Webmethode*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Beachten Sie, dass die **[QueryString]** Attribut wird verwendet, um die Parameter der Methode "ProductID"-Parameters in der Abfragezeichenfolge zu füllen.
6. Drücken Sie **F5** Debuggen Sie den Standort aus, und wechseln zur Seite "Produkte". Wählen Sie eine beliebige Kategorie aus den Kategorien GridView, und beachten Sie, dass die Produkte GridView wird aktualisiert.

    ![Mit den Produkten aus der ausgewählten Kategorie](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Produkte aus der ausgewählten Kategorie anzeigen")

    *Mit den Produkten aus der ausgewählten Kategorie*
7. Klicken Sie auf die **Ansicht** Link für ein Produkt auf der Seite "ProductDetails.aspx" zu öffnen.

    Beachten Sie, dass die Seite das Produkt mit der mit dem Parameter "ProductID" aus der Abfragezeichenfolge SelectMethod abruft.

    ![Anzeigen von die Produktdetails](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "die Produktdetails anzeigen")

    *Die Produktdetails anzeigen*

    > [!NOTE]
    > Die Fähigkeit, geben Sie eine HTML-Beschreibung wird in der nächsten Übung implementiert werden.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Aufgabe 5: mit Modellbindung für Update-Vorgänge

In der vorherigen Aufgabe haben Sie wurden die modellbindung hauptsächlich zum Auswählen von Daten verwendet, in dieser Aufgabe erfahren Sie, wie in Aktualisierungsvorgängen wurden die modellbindung verwenden.

Aktualisieren Sie die Kategorien GridView, damit die Benutzer Kategorien aktualisieren können.

1. Öffnen der **Products.aspx** Seite, und aktualisieren Sie die Kategorien auf die Schaltfläche "Bearbeiten" automatisch zu generieren und verwenden Sie die neue GridView **UpdateMethod** -Attribut zum Angeben einer **UpdateCategory**Methode, um das ausgewählte Element zu aktualisieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Der DataKeyNames-Attribut in die GridView zu definieren, die die Elemente sind, die das Modell gebundene Objekt eindeutig zu identifizieren und daher die den Parametern, die mindestens die Update-Methode erhalten soll.
2. Öffnen der **Products.aspx.cs** Code-Behind-Datei und Implementieren der **UpdateCategory** Methode. Die Methode sollte die Kategorie-ID zum Laden der aktuellen Kategorie, die Werte aus der GridView aufzufüllen, und aktualisieren Sie dann die Kategorie angezeigt.

    (Codeausschnitt - *Web Forms Lab - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Die neue **TryUpdateModel** Methode in der Page-Klasse ist verantwortlich für das Auffüllen der Model-Objekts unter Verwendung der Werte aus den Steuerelementen auf der Seite. In diesem Fall wird es ersetzen Sie die aktualisierten Werte aus der aktuellen GridView-Zeile, die bearbeitet wird, in der **Kategorie** Objekt.

    > [!NOTE]
    > Die nächste Übung wird die Verwendung der ModelState.IsValid zum Überprüfen der Daten, die vom Benutzer eingegeben werden, wenn das Objekt bearbeiten erläutert.
3. Führen Sie den Standort aus, und wechseln Sie zur Seite "Produkte". Bearbeiten einer Kategorie an. Geben Sie einen neuen Namen ein, und klicken Sie dann auf **Update** damit die Änderungen beibehalten.

    ![Bearbeiten von Kategorien](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Bearbeiten von Kategorien")

    *Bearbeiten von Kategorien*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Übung 2: Überprüfen von Daten

In dieser Übung erfahren Sie mehr über die neuen Funktionen der Data Validation in ASP.NET 4.5. Sie werden die neuen Funktionen der unaufdringlichen Überprüfung in Web Forms Auschecken. Verwenden Sie datenanmerkungen in der Anwendung Modellklassen für die Validierung von Benutzereingaben und schließlich erfahren Sie, wie aktivieren oder Deaktivieren der anforderungsüberprüfung in einzelne Steuerelemente auf einer Seite.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Aufgabe 1 – unaufdringlichen Überprüfung

Formulare mit komplexen Daten einschließlich Validierungssteuerelemente tendenziell zu viel JavaScript-Code auf der Seite generieren, die CA. 60 % des Codes darstellen können. Unaufdringlichen Überprüfung aktiviert wird Ihr HTML-Code übersichtlicher und besseren Überblick über Ihre aussehen.

Aktivieren Sie in diesem Abschnitt unaufdringlichen Validierung in ASP.NET, die von beiden Konfigurationen generierten HTML-Code verglichen werden soll.

1. Öffnen Sie **Visual Studio 2012** , und öffnen Sie die **beginnen** Projektmappe befindet sich in der **Source\Ex2 Validation\Begin** Ordner dieser Anleitung. Alternativ können Sie Ihre Arbeit auf Ihre vorhandene Lösung aus der vorherigen Übung fortsetzen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie dazu im Projektmappen-Explorer klicken Sie auf die **WebFormsLab** Projekt **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Drücken Sie **F5** zum Starten der Webanwendung. Wechseln Sie zu den Kunden Seite, und klicken Sie auf die **fügen Sie einen neuen Kunden** Link.
3. Mit der rechten Maustaste auf die Seite "Browser", und wählen Sie **Quelltext anzeigen** Option, um die von der Anwendung generierten HTML-Code zu öffnen.

    ![Anzeigen der Seite HTML-Code](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Anzeigen der Seite HTML-Code")

    *Anzeigen der Seite HTML-code*
4. Blättern Sie in den Quellcode für die Seite, und beachten Sie, dass ASP.NET JavaScript-Code und Daten Validierungssteuerelemente auf der Seite um die Überprüfungen ausführen und Anzeigen der Fehlerliste eingefügt wurde.

    ![Überprüfung von JavaScript-Code in CustomerDetails Seite](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Überprüfung JavaScript-Code in CustomerDetails-Seite")

    *Überprüfung von JavaScript-Code in CustomerDetails-Seite*
5. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.
6. Aktivieren Sie jetzt unaufdringlichen Überprüfung. Open **"Web.config"** und suchen Sie **ValidationSettings:UnobtrusiveValidationMode** -Schlüssel in der **"appSettings"** Abschnitt **.** Legen Sie den Schlüsselwert auf **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Sie können diese Eigenschaft auch festlegen, der &quot; **Seite\_laden** &quot; Ereignis in Fällen, die Sie nur für einige Seiten unaufdringlichen Überprüfung aktivieren möchten.
7. Open **CustomerDetails.aspx** , und drücken Sie **F5** zum Starten der Web-Anwendung.
8. Drücken Sie F12-Taste, um die Internet Explorer-Entwicklertools zu öffnen. Nachdem die Entwicklertools geöffnet ist, wählen Sie die Registerkarte "Skript". Wählen Sie **CustomerDetails.aspx** Beachten Sie, dass die Skripts zum Ausführen von jQuery auf der Seite erforderlich wurden aus dem Menü, und ergreifen Sie in den Browser des lokalen Standorts geladen wurde.

    ![Laden die jQuery JavaScript-Dateien direkt aus dem lokalen IIS-Server](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "beim Laden des jQuery JavaScript-Dateien direkt aus dem lokalen IIS-Server")

    *Laden die jQuery JavaScript-Dateien direkt aus dem lokalen IIS-server*
9. Schließen Sie den Browser, um Sie zu Visual Studio zurückzukehren. Öffnen der **Site.Master** Datei erneut, und suchen Sie die **ScriptManager**. Fügen Sie das Attribut **EnableCdn** Eigenschaft mit dem Wert **"true"**. Dies erzwingt eine jQuery aus den online-URL und nicht von der lokalen Website URL geladen werden soll.
10. Open **CustomerDetails.aspx** in Visual Studio. Drücken Sie die Taste F5, um die Website ausgeführt. Sobald Internet Explorer geöffnet wird, drücken Sie F12-Taste, um den Entwicklertools zu öffnen. Wählen Sie die **Skript** Registerkarte, und führen Sie einen Blick auf die Dropdown-Liste. Beachten Sie, dass die jQuery JavaScript-Dateien nicht mehr aus dem lokalen Standort, aber statt aus dem online jQuery CDN geladen werden.

    ![Laden die jQuery JavaScript-Dateien aus dem CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "beim Laden des jQuery JavaScript-Dateien aus dem CDN")

    *Laden die jQuery JavaScript-Dateien aus dem CDN*
11. Quellcode der HTML-Seite erneut mit der Quelle Ansichtsoption im Browser zu öffnen. Beachten Sie, dass durch die Aktivierung der unaufdringlichen Überprüfung ASP.NET den eingefügten JavaScript-Code durch Data - ersetzt \*Attribute.

    ![Unaufdringlichen Validierungscode](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "unaufdringlichen Validierungscode")

    *Unaufdringlichen Validierungscode*

    > [!NOTE]
    > In diesem Beispiel haben Sie gesehen, wie eine Validierung mit datenanmerkungen Zusammenfassung nur wenige HTML und JavaScript-Zeilen vereinfacht wurde. Zuvor ohne Überprüfung der unaufdringlichen, die Validierungssteuerelemente, die Sie hinzufügen, desto größer Validierungscodes JavaScript wächst.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Aufgabe 2: beim Validieren des Modells mit Datenanmerkungen

ASP.NET 4.5 wird Anmerkungen datenüberprüfung für Web Forms eingeführt. Sie können jetzt Einschränkungen in Ihren Modellklassen definieren und verwenden sie für alle Ihre Webanwendung, anstatt ein Validierungssteuerelement für jede Eingabe zu. In diesem Abschnitt erfahren Sie, wie datenanmerkungen zum Überprüfen einer kundenformulars neu/bearbeiten.

1. Open **CustomerDetail.aspx** Seite. Beachten Sie, die der Kunde Vorname und zweiter name in der **EditItemTemplate** und **InsertItemTemplate** Abschnitte werden mit einem RequiredFieldValidator-Steuerelementen überprüft. Daher Sie so viele Validierungssteuerelemente als Bedingung aus, um zu überprüfen müssen, werden die einzelnen Validierungssteuerelemente auf eine bestimmte Bedingung zugeordnet ist.
2. Hinzufügen von datenanmerkungen zum Überprüfen der Customer-Modell-Klasse. Open **Customer.cs** -Klasse in der **Modell** Ordner und *ergänzen* jede Eigenschaft, die mit Datenattributen für die Anmerkung.

    (Codeausschnitt - *Web Forms Lab - Ex02 - Datenanmerkungen*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4.5 wurde erweitert, dass die vorhandene Anmerkung Datensammlung. Dies sind einige der datenanmerkungen Sie können: [CreditCard], [Phone], [EmailAddress], [Bereich], [vergleichen,] [Url], [FileExtensions], [Required], [Schlüssel], [der reguläre Ausdruck].
    > 
    > Einige Anwendungsbeispiele:
    > 
    > [Schlüssel]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Sie können auch eigene Fehlermeldungen innerhalb jedes Attribut definieren.
3. Open **CustomerDetails.aspx** und entfernen Sie alle RequiredFieldvalidators für die vor-und Nachname Felder in den in EditItemTemplate und InsertItemTemplate Abschnitten des FormView-Steuerelements.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Ein Vorteil der Verwendung von datenanmerkungen ist, dass Validierungslogik in Ihrer Anwendungsseiten nicht dupliziert werden. Sie einmal im Modell definieren, und verwenden es über alle Anwendungsseiten, die Daten zu bearbeiten.
4. Open **CustomerDetails.aspx** Code-Behind, und suchen Sie die SaveCustomer-Methode. Diese Methode wird aufgerufen, wenn einen neuen Kunde einfügen und empfängt die Customer-Parameter von FormView Steuerelementwerte. Wenn die Zuordnung zwischen der Steuerelemente der Seite und die Parameter Objekt auftritt, ASP.NET ausgeführt wird, die modellvalidierung gegen die datenanmerkung Attribute und ModelState Wörterbuch mit den aufgetretenen Fehler auszufüllen, wenn alle.

    Die ModelState.IsValid gibt nur "true", wenn alle Felder in Ihrem Modell gültig sind, nach dem Ausführen der Validierung zurück.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Hinzufügen einer **ValidationSummary** Steuerelement am Ende der Seite "CustomerDetails" zum Anzeigen der Liste der Fehler im Objektmodell.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    Die **ShowModelStateErrors** wird eine neue Eigenschaft für die ValidationSummary zu steuern, die bei Festlegung auf **"true"**, das Steuerelement wird der Fehler aus dem Wörterbuch ModelState anzeigen. Diese Fehler stammen aus der datenüberprüfung von Anmerkungen.
6. Drücken Sie **F5** um die Web-Anwendung auszuführen. Füllen Sie das Formular mit einige fehlerhafte Werte ein, und klicken Sie auf **speichern** auszuführenden Überprüfung. Beachten Sie die fehlerzusammenfassung unten ein.

    ![Validierung mit Datenanmerkungen](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validierung mit Datenanmerkungen")

    *Validierung mit Datenanmerkungen*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Aufgabe 3: benutzerdefinierte Fehlerbehandlung mit ModelState

Behandeln von Datenbankfehlern z. B. eine Zeichenfolge zu lang oder eine Verletzung des eindeutigen Schlüssels kann in einer früheren Version von Web Forms vorsehen, Auslösen von Ausnahmen im Code Repository, und klicken Sie dann die Ausnahmebehandlung auf die Code-Behind-Fehler angezeigt. Eine große Menge an Code ist erforderlich, um eine Aktion relativ einfach.

In Web Forms 4.5 kann das ModelState-Objekt verwendet werden, zum Anzeigen der Fehler auf der Seite aus dem Modell oder aus der Datenbank konsistent.

In dieser Aufgabe fügen Sie Code aus, um ordnungsgemäß Behandeln von Datenbankausnahmen und die entsprechende Meldung an dem Benutzer unter Verwendung des ModelState-Objekts anzeigen.

1. Während die Anwendung weiterhin ausgeführt wird, versuchen Sie, den Namen einer Kategorie, die über einen duplizierten Wert zu aktualisieren.

    ![Aktualisieren eine Kategorie mit einem doppelten Namen](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "aktualisieren eine Kategorie mit einem doppelten Namen")

    *Aktualisieren eine Kategorie mit einem doppelten Namen*

    Beachten Sie, die eine Ausnahme, aufgrund ausgelöst wird der &quot;eindeutige&quot; Einschränkung von der **CategoryName** Spalte.

    ![Doppelte Namen für die Ausnahme](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "doppelte Namen für die Ausnahme")

    *Doppelte Namen für die Ausnahme*
2. Beenden des Debuggens. In der **Products.aspx.cs** Code-Behind-Datei, ein Update der **UpdateCategory** Methode, um die von der Datenbank ausgelöste Ausnahmen behandelt. SaveChanges()-Methodenaufruf, und fügen Sie einen Fehler auf, die **ModelState** Objekt.

    Die neue **TryUpdateModel** Methode aktualisiert das Category-Objekt, das aus der Datenbank, die mit den Formulardaten, die vom Benutzer bereitgestellte abgerufen.

    (Codeausschnitt - *Web Forms Lab - Ex02 - UpdateCategory Behandeln von Fehlern*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Im Idealfall müssten Sie zum Ermitteln der Ursache von der DbUpdateException und überprüfen Sie, ob die Hauptursache einen Verstoß gegen eine unique-Einschränkung ist.
3. Open **Products.aspx** und Hinzufügen einer **ValidationSummary** Steuerelement unterhalb der Kategorien GridView zum Anzeigen der Liste der Fehler im Objektmodell.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Führen Sie den Standort aus, und wechseln Sie zur Seite "Produkte". Versuchen Sie, den Namen einer Kategorie, die über einen duplizierten Wert zu aktualisieren.

    Beachten Sie, die die Ausnahme behandelt wurde, und die Fehlermeldung wird angezeigt, der **ValidationSummary** Steuerelement.

    ![Fehler der Kategorie dupliziert](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "dupliziert Kategorie-Fehler")

    *Doppelte Kategorie-Fehler*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Aufgabe 4: der Anforderungsvalidierung in ASP.NET Web Forms 4.5

Die Anforderung Überprüfungsfunktion in ASP.NET bietet ein gewisses Maß an Standardeinstellung Schutz vor Angriffen siteübergreifendem Skripting (XSS). In früheren Versionen von ASP.NET anforderungsüberprüfung wurde standardmäßig aktiviert und kann nur für eine gesamte Seite deaktiviert werden. Mit der neuen Version von ASP.NET Web Forms können jetzt die anforderungsüberprüfung für ein einzelnes Steuerelement deaktivieren, lazy Anforderung Validierung oder Zugriff auf nicht überprüfte Anforderungsdaten (Achten Sie in diesem Fall!).

1. Drücken Sie **STRG + F5** starten den Standort ohne Debuggen aus, und wechseln zur Seite "Produkte". Wählen Sie eine Kategorie aus, und klicken Sie dann auf die **bearbeiten** Link auf eines der Produkte.
2. Geben Sie eine Beschreibung, die potenziell gefährlichen Inhalt enthält, z. B. einschließlich der HTML-Tags ein. Nehmen Sie beachten Sie, dass die Ausnahme, die ausgelöst wird, die Anforderungsvalidierung in Anspruch.

    ![Bearbeiten ein Produkt mit potenziell gefährlichen Inhalt](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "ein Produkt mit potenziell gefährlichen Inhalt bearbeiten")

    *Ein Produkt mit potenziell gefährlichen Inhalt bearbeiten*

    ![Ausnahme aufgrund Anforderungsvalidierung](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "aufgrund Anforderungsvalidierung ausgelöste Ausnahme")

    *Aufgrund der anforderungsüberprüfung ausgelöste Ausnahme*
3. Schließen Sie die Seite, und drücken Sie in Visual Studio **UMSCHALT + F5** debugging beenden.
4. Öffnen der **ProductDetails.aspx** Seite, und suchen Sie die **Beschreibung** Textfeld.
5. Fügen Sie der neuen **ValidateRequestMode** Eigenschaft, um das Textfeld ein, und legen Sie dessen Wert auf **deaktiviert**.

    Die neue **ValidateRequestMode** -Attributs können Sie die Anforderungsvalidierung präzise auf jedes Steuerelement zu deaktivieren. Dies ist hilfreich, wenn eine Eingabe, die HTML-Code erhalten können, aber die Überprüfung für den Rest der Seite arbeiten beibehalten möchten, verwenden möchten.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Drücken Sie **F5** um die Webanwendung auszuführen. Öffnen Sie die Produktseite bearbeiten erneut, und abgeschlossen Sie eine HTML-Tags einschließlich produktbeschreibung. Beachten Sie, dass Sie die Beschreibung jetzt HTML-Inhalt hinzufügen können.

    ![Die Anforderungsvalidierung für die produktbeschreibung deaktiviert](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Anforderungsvalidierung für die produktbeschreibung deaktiviert")

    *Anforderungsüberprüfung für die produktbeschreibung deaktiviert*

    > [!NOTE]
    > In einer produktionsanwendung sollte Sie bereinigen den HTML-Code eingegeben haben, durch den Benutzer aus, um sicherzustellen, dass nur sichere HTML-Tags eingegeben werden (z. B. stehen keine &lt;Skript&gt; Tags). Zu diesem Zweck können Sie [Microsoft Web Protection Bibliothek](https://www.nuget.org/packages/AntiXSS).
7. Bearbeiten Sie das Produkt erneut. Geben Sie HTML-Code in das Feld Name, und klicken Sie auf **speichern**. Beachten Sie, die Überprüfung anfordern ist nur für das Feld "Beschreibung" deaktiviert und die restlichen Felder re noch gegen potenziell gefährlichen Inhalt.

    ![In den restlichen Feldern aktiviert Überprüfung anfordern](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Anforderungsvalidierung in den restlichen Feldern aktiviert")

    *Fordern Sie in den restlichen Feldern aktiviert Überprüfung an*

    ASP.NET Web Forms 4.5 umfasst eine neue Anforderung Validierungsmodus Anforderungsvalidierung verzögert ausführen. Mit der Anforderung Überprüfung Modus **4.5**, wenn ein Stück Code greift auf *Request.Form [&quot;Schlüssel&quot;]*, ASP.NET 4.5 Anforderung Überprüfung wird nur Trigger Anforderungsvalidierung für dieses bestimmte Element in der formularauflistung.

    Darüber hinaus schließt ASP.NET 4.5 jetzt Core Codierung Routinen aus der Microsoft-Anti-XSS-Bibliothek v4. 0. Die Anti-XSS-Codierung Routinen, die von der neuen implementiert *AntiXssEncoder* Typ gefunden, in der neuen **System.Web.Security.AntiXss** Namespace. Mit der **EncoderType** Parameter für die Verwendung konfiguriert *AntiXssEncoder*, alle Ausgabedateien Codierung in ASP.NET automatisch die neue codieren Routinen verwendet.
8. ASP.NET 4.5 Anforderungsvalidierung unterstützt auch nicht überprüfte Zugriff auf Anforderungsdaten verwendet werden. ASP.NET 4.5 Fügt eine neue Auflistungseigenschaft, um die **HttpRequest** bezeichnetes Objekt **Unvalidated**. Wenn Sie beim Navigieren in **HttpRequest.Unvalidated** haben Sie Zugriff auf die allgemeinen Teile von Anforderungsdaten verwendet werden, einschließlich Formulare, Formularwerte, Cookies, URLs und So weiter.

    ![Request.Unvalidated Objekt](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated-Objekt")

    *Request.Unvalidated-Objekt*

    > [!NOTE]
    > **Verwenden Sie die HttpRequest.Unvalidated-Eigenschaft, mit Bedacht vorgehen!** Stellen Sie sicher, dass Sie eine benutzerdefinierte Validierung sorgfältig auf die unformatierten Daten sicherstellen, dass problematische Text nicht Roundtrip ausgeführt und an andere Kunden gerendert ausführen!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Übung 3: Asynchrone Seite Verarbeitung in der ASP.NET Web Forms

In dieser Übung werden Sie auf die neue asynchrone-Seite, die Verarbeitung von Funktionen in ASP.NET Web Forms eingeführt werden.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Aufgabe 1: Aktualisieren des Produkts Detailseite zum Hochladen und Anzeigen von Bildern

In dieser Aufgabe aktualisieren Sie die Produktseite für die Details zum zulassen, dass die Benutzer geben Sie eine Bild-URL für das Produkt, und zeigen Sie es in der schreibgeschützten Sicht. Erstellen Sie eine lokale Kopie der angegebenen Image synchron herunterladen. In der nächsten Aufgabe aktualisieren Sie diese Implementierung um Arbeiten asynchron zu vereinfachen.

1. Open **Visual Studio 2012** und laden die **beginnen** Projektmappe befindet sich im **Source\Ex3 Async\Begin** aus dieser Übungseinheit-Ordner. Alternativ können Sie auf Ihre vorhandene Lösung aus den früheren Übungen Ihre Arbeit fortsetzen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie dazu im Projektmappen-Explorer klicken Sie auf die **WebFormsLab** Projekt, und wählen Sie **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **ProductDetails.aspx** Seite Datenquelle und Hinzufügen eines Felds in die FormView ItemTemplate zum Anzeigen des Bilds Produkt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Fügen Sie ein Feld zum Angeben der Bild-URL in die FormView EditTemplate hinzu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Öffnen der **ProductDetails.aspx.cs** Code-Behind-Datei, und fügen Sie die folgenden Namespace-Direktiven.

    (Codeausschnitt - *Web Forms Lab - Ex03 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Erstellen einer **UpdateProductImage** Methode zum Speichern von remote-Images in der lokalen **Bilder** Ordner, und Aktualisieren der Product-Entität mit dem neuen Wert des Image-Speicherort.

    (Codeausschnitt - *Web Forms Lab - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Update der **UpdateProduct** aufzurufende der **UpdateProductImage** Methode.

    (Codeausschnitt - *Web Forms Lab - Ex03 - UpdateProductImage Aufruf*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Führen Sie die Anwendung, und versuchen Sie, ein Bild für ein Produkt hochladen. Beispielsweise können Sie die folgenden Bild-URL aus Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Festlegen eines Bilds für ein Produkt](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Festlegen eines Bilds für ein Produkt")

    *Festlegen eines Bilds für ein Produkt*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Aufgabe 2: Hinzufügen von asynchronen Verarbeitung auf der Detailseite des Produkts

In dieser Aufgabe aktualisieren Sie die Produktseite Details, um Arbeiten asynchron zu vereinfachen. Sie werden eine lang ausgeführte Aufgabe - Downloadvorgang Image - mithilfe von ASP.NET 4.5 asynchrone Seite Verarbeitung optimiert.

Asynchrone Methoden in Webanwendungen können Optimierung verwendeten Threadpools ASP.NET verwendet werden. In ASP.NET gibt es sind eine begrenzte Anzahl von Threads im Threadpool für die Teilnahme anfordert, daher bei der alle Threads mit der ausgelastet ist, sind ASP.NET lehnt neue Anforderungen ab, sendet Fehlermeldungen für die Anwendung und Ihrer Website nicht verfügbar macht.

Zeitaufwändige Operationen auf der Website sind gute Kandidaten für die asynchrone Programmierung, da sie den zugewiesenen Thread für eine lange Zeit belegen. Dies schließt lang andauernder Anforderungen, die mit vielen unterschiedlichen Elementen und Seiten, die offline-Vorgänge, solche Abfragen einer Datenbank oder den Zugriff auf einen externen Webserver erfordern. Der Vorteil besteht darin, dass bei Verwendung von asynchronen Methoden für diese Vorgänge während der Verarbeitung der Seite wird der Thread freigegeben und an den Thread zurückgegeben pool und können verwendet werden, um auf eine neue Seitenanforderung teilnehmen. Dies bedeutet dass, die Seite wird in einem Thread aus dem Threadpool Verarbeitung gestartet und Verarbeitung in einer anderen möglicherweise abgeschlossen werden, nachdem die asynchrone Verarbeitung abgeschlossen ist.

1. Öffnen der **ProductDetails.aspx** Seite. Hinzufügen der **Async** Attribut in der **Seite** Element, und legen Sie es auf **"true"**. Dieses Attribut weist ASP.NET für die-Schnittstelle implementiert.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Fügen Sie eine Bezeichnung am unteren Rand der Seite, um die Details der Threads mit der Seite angezeigt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Öffnen Sie das **ProductDetails.aspx.cs** und fügen Sie die folgenden Namespace-Direktiven.

    (Codeausschnitt - *Web Forms Lab - Ex03 - Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Ändern der **UpdateProductImage** Methode zum Herunterladen des Abbilds mit einer asynchronen Aufgabe. Ersetzen Sie die **WebClient** **DownloadFile** Methode mit der **DownloadFileTaskAsync** Methode und enthalten die **"await"** Schlüsselwort.

    (Codeausschnitt - *Web Forms Lab - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Die RegisterAsyncTask registriert eine neue Seite asynchrone Aufgabe, die in einem anderen Thread ausgeführt werden. Er empfängt einen Lambda-Ausdruck, mit der Aufgabe (t) ausgeführt werden. Der **"await"** -Schlüsselwort in der **DownloadFileTaskAsync** -Methode konvertiert den Rest der Methode einen Rückruf, der nach asynchron aufgerufen wird, die **DownloadFileTaskAsync** -Methode abgeschlossen wurde. ASP.NET wird die Ausführung der Methode fortgesetzt, indem alle HTTP-Anforderung ursprüngliche Werte automatisch verwalten. Das neue asynchrone Programmiermodell in .NET 4.5 können Sie asynchronen Code schreiben, sehr ähnlich wie synchroner Code aussieht, und dass der Compiler die Bereichsregeln Rückruffunktionen oder Fortsetzung Code verarbeitet.
    > [!NOTE]
    > RegisterAsyncTask und PageAsyncTask wurden bereits seit .NET 2.0 zur Verfügung. Der Await-Schlüsselworts Neuware des asynchronen Programmiermodells von .NET 4.5 ist und kann zusammen mit den neuen TaskAsync Methoden aus dem .NET WebClient-Objekt verwendet werden.
5. Fügen Sie Code, um die Threads anzuzeigen, auf denen der Code gestartet, und die Ausführung beendet. Aktualisieren Sie zu diesem Zweck die **UpdateProductImage** -Methode durch folgenden Code.

    (Codeausschnitt - *Web Forms-Lab - Ex03 - anzeigen Threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Öffnen Sie die Website **"Web.config"** Datei. Fügen Sie die folgenden AppSetting-Variable.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Drücken Sie **F5** um die Anwendung auszuführen und Bilder für das Produkt hochladen. Beachten Sie die Threads-ID, in dem der Code gestartet oder beendet unterscheiden kann. Dies liegt daran asynchrone Aufgaben in einem separaten Thread aus dem Threadpool für ASP.NET ausführen. Klicken Sie nach Abschluss die Aufgabe wird ASP.NET legt den Task wieder in die Warteschlange und weist eine der verfügbaren Threads.

    ![Ein Bild asynchron herunterladen](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "asynchron ein Bild herunterladen")

    *Ein Bild herunterladen asynchron*

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung auf Azure Folgendes [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben die folgenden Konzepte behandelt veranschaulicht und wurde:

- Verwenden von stark typisierten Datenbindungsausdrücke
- Verwenden Sie neue Modell Bindungsfunktionen in Web Forms
- Verwenden Sie Wertanbieter für die Zuordnung von Daten der Seite zu Code-Behind-Methoden
- Verwenden von Datenanmerkungen für die Validierung von Benutzereingaben
- Nehmen Sie Advange Unobstrusive die clientseitige Validierung mit jQuery in Web Forms
- Differenzierte Anforderungsvalidierung implementieren
- Implementieren Sie asynchrone Verarbeitung in Web Forms-Seite

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

In diesem Anhang wird gezeigt, wie eine neue Website aus dem Azure-Portal erstellen und Veröffentlichen der Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Azure bereitgestellt werden.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Azure-Portal

1. Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Sie können mit Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei dem Portal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Azure ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung in Azure von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in Azure für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen in Azure.

    ![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung in Azure aus Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement im Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **Dashboard des Servers**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
   - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
   - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren von Zielverbindungszeichenfolge](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
