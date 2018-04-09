---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4-Abhängigkeitsinjektion | Microsoft Docs
author: rick-anderson
description: 'Hinweis: Diese praktische Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse der ASP.NET MVC und ASP.NET MVC 4-Filter verfügen. Wenn Sie nicht ASP.NET MVC 4-Filter, bevor wir Rec verwendet haben...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4-Abhängigkeitsinjektion

Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](https://aka.ms/webcamps-training-kit)

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC** und **ASP.NET MVC 4 filtert**. Wenn Sie nicht verwendet haben **ASP.NET MVC 4 filtert** vorher, empfehlen wir Ihnen, durchlaufen **Aktionsfilter in ASP.NET MVC benutzerdefinierte** praktische Übungseinheit.

> [!NOTE]
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit zur Nächten enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für diese Übung finden Sie unter [Abhängigkeitsinjektion in ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

In **Objekt Oriented Programming** Paradigma Objekten arbeiten zusammen in einem Modell Zusammenarbeit "Mitwirkende" und Consumer vorliegen. Natürlich generiert diese Kommunikationsmodell Abhängigkeiten zwischen Objekten und Komponenten, die schwer zu verwalten, wenn die Komplexität erhöht wird.

![Klasse von Abhängigkeiten und Containerklasse Komplexität](aspnet-mvc-4-dependency-injection/_static/image1.png "-Klasse Abhängigkeiten und Containerklasse Komplexität")

*Klasse Abhängigkeiten und Komplexität des Modells*

Sie haben wahrscheinlich schon die **Factorymuster** und die Trennung zwischen der Benutzeroberfläche und die Implementierung mit Diensten, bei dem die Clientobjekte häufig verantwortlich für die dienstidentifizierung.

Die Abhängigkeitsinjektion Muster ist eine bestimmte Implementierung des Inversion of Control. **Die Umkehrung des Steuerelements (IoC)** bedeutet, dass Objekte nicht andere Objekte auf dem sie basieren erstellen, um ihre Arbeit zu erledigen. Stattdessen erhalten sie die Objekte, die sie aus einer externen Quelle (z. B. eine XML-Konfigurationsdatei) benötigen.

**(Dependency Injection, DI)** bedeutet, dass dies geschieht ohne Eingreifen Objekt in der Regel durch eine .NET Framework-Komponente, die Konstruktorparameter übergibt und Eigenschaften festlegen.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Das Entwurfsmuster der Dependency Injection (DI)

Auf hoher Ebene, das Ziel der Abhängigkeitsinjektion besteht darin, die eine Clientklasse (z. B. *der dsmessages*) benötigt, das eine Schnittstelle erfüllt (z. B. *IClub*). Es ist nicht wichtig, was der konkrete Typ ist (z. B. *WoodClub IronClub, WedgeClub* oder *PutterClub*), Personen, die verarbeitet werden sollen (z. B. eine gute *Caddy*). Der Abhängigkeitskonfliktlöser in ASP.NET MVC können Sie Ihre Abhängigkeit-Logik, die an anderer Stelle zu registrieren (z. B. einen Container oder ein *Behälter Kreuz*).

![Dependency Injection Diagramm](aspnet-mvc-4-dependency-injection/_static/image2.png "Abhängigkeitsinjektion-Abbildung")

*Abhängigkeitsinjektion - Golf Analogie*

Vorteile der Verwendung der Abhängigkeitseinfügung Muster und Inversion of Control sind folgende:

- Reduziert die Klassenkopplungen
- Erhöht die Wiederverwendung von code
- Verbessert die Verwaltbarkeit von code
- Verbessert die Anwendung testen

> [!NOTE]
> Abhängigkeitsinjektion wird manchmal mit abstrakten Factoryentwurfsmuster verglichen, aber ein geringfügigen Unterschied zwischen beiden Ansätzen vorhanden ist. DI hat ein Framework hinter arbeiten, Abhängigkeiten, die durch Aufrufen von den Factorys und die registrierten Dienste behoben.


Nun, dass Sie das Dependency Injection Muster verstanden haben, lernen während dieser Übung für die anzuwendende in ASP.NET MVC 4 Sie. Starten Sie mithilfe der Abhängigkeitsinjektion in der **Controller** eine Datenbank-Access-Dienst umfassen. Als Nächstes Sie Abhängigkeitsinjektion zum Anwenden der **Ansichten** , nutzen einen Dienst und Anzeigen von Informationen. Zum Schluss erweitern die DI zu ASP.NET MVC 4-Filter, Sie Räumen eines benutzerdefinierten Aktionsfilters in der Projektmappe.

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Integrieren von ASP.NET MVC 4 mit Unity für Zielabhängigkeit mithilfe von NuGet-Pakete
- Verwenden Sie die Abhängigkeitsinjektion in ASP.NET MVC-Controllers
- Verwenden Sie die Abhängigkeitsinjektion in ASP.NET MVC-Ansicht
- Verwenden Sie die Abhängigkeitsinjektion innerhalb einer ASP.NET MVC-Action-Filter

> [!NOTE]
> Dieser Übung verwendeten Unity.Mvc3 NuGet-Paket für die Auflösung der Abhängigkeit, aber es ist möglich, einem Dependency Injection-Framework zum Arbeiten mit ASP.NET MVC 4 angepasst werden kann.


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang B: mithilfe von Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:

1. [Übung 1: Räumen eines Controllers](#Exercise1)
2. [Übung 2: Räumen eine Sicht](#Exercise2)
3. [Übung 3: Räumen Filter](#Exercise3)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Übung 1: Räumen eines Controllers

In dieser Übung erfahren Sie, wie Sie die Abhängigkeitsinjektion in ASP.NET MVC-Controller verwenden, durch die Integration von Unity unter Verwendung eines NuGet-Pakets. Aus diesem Grund schließt Sie Dienste in Ihre MvcMusicStore-Domänencontroller, um die Logik von der Datenzugriff zu trennen. Die Dienste erstellt eine neue Abhängigkeit im Konstruktor Controller, die mithilfe der Abhängigkeitsinjektion mit Hilfe der aufgelöst werden **Unity**.

Dieser Ansatz wird gezeigt, wie zum Generieren von weniger verbundene Anwendungen sind flexibler und einfacher zu verwalten und zu testen. Außerdem erfahren Sie, wie zum Integrieren von ASP.NET MVC mit Unity wird.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Informationen zum StoreManager-Dienst

Das MVC-Music Store jetzt in der Begin-Projektmappe bereitgestellt enthält einen Dienst, der mit dem Namen Store Controller Daten verwaltet **StoreService**. Im folgenden finden Sie die Store-Dienst-Implementierung. Beachten Sie, dass alle Methoden modellentitäten zurückgeben.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** von der Begin Projektmappe jetzt nutzt **StoreService**. Wurden alle Datenverweise daraus **StoreController**, jetzt möglich, zum Ändern des aktuellen datenzugriffsanbieters ohne jede Methode, die verbraucht und **StoreService**.

Finden Sie nachfolgend die der **StoreController** Implementierung hat eine Abhängigkeit mit **StoreService** im Klassenkonstruktor.

> [!NOTE]
> Die Abhängigkeit, die in dieser Übung eingeführt bezieht sich auf **Inversion of Control** (IoC).
> 
> Die **StoreController** Klassenkonstruktor empfängt eine **IStoreService** Type-Parameter, die zum Ausführen der Dienstaufrufe vom innerhalb der Klasse unbedingt erforderlich sind. Allerdings **StoreController** implementiert nicht die Standardkonstruktor (ohne Parameter), die jeden Controller zum Arbeiten mit ASP.NET MVC.
> 
> Um die Abhängigkeit zu beheben, muss der Controller erstellt werden, indem eine abstrakte Factory (eine Klasse, die ein Objekt des angegebenen Typs zurückgibt).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Sie erhalten einen Fehler, wenn die Klasse versucht, den StoreController zu erstellen, ohne das Dienstobjekt senden, wie es keinen parameterloser Konstruktor deklariert gibt.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Aufgabe 1: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung beginnen, die den Dienst in den Speicher-Controller enthält, die den Datenzugriff von der Anwendungslogik trennt.

Wenn die Anwendung ausgeführt wird, erhalten Sie eine Ausnahme, wie der Controller-Dienst wird standardmäßig nicht als Parameter übergeben wird:

1. Öffnen der **beginnen** Projektmappe befindet sich im **Source\Ex01 Räumen Controller\Begin**.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Drücken Sie **STRG + F5** um die Anwendung ohne Debuggen auszuführen. Sie erhalten die Fehlermeldung &quot; **keinen parameterlosen Konstruktor für dieses Objekt definierten**&quot;:

    ![Fehler beim Ausführen von Begin ASP.NET MVC-Anwendung](aspnet-mvc-4-dependency-injection/_static/image3.png "Fehler während der Ausführung beginnen ASP.NET MVC-Anwendung")

    *Fehler beim Ausführen von Begin ASP.NET MVC-Anwendung*
3. Schließen Sie den Browser.

Arbeiten Sie in den folgenden Schritten auf Music Store-Lösung, die Abhängigkeit einzufügen, die diesem Controller benötigt.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Aufgabe 2 – einschließlich Unity in MvcMusicStore-Lösung

In dieser Aufgabe schließt **Unity.Mvc3** NuGet-Paket, um die Projektmappe.

> [!NOTE]
> ASP.NET MVC 3 Unity.Mvc3 Paket entwickelt wurde, jedoch ist vollständig kompatibel mit ASP.NET MVC 4.
> 
> Unity einen einfachen und erweiterbaren abhängigkeitseinschleusungscontainer mit optionale Unterstützung für die Instanz ist, und geben abfangen. Es ist ein allzweckcontainer für die Verwendung in jede Art von Anwendung in .NET. Es enthält alle gemeinsamen Funktionen, die Dependency Injection Mechanismen, einschließlich gefunden: Erstellen von Objekten, die Abstraktion von Anforderungen durch Angeben von Abhängigkeiten zur Laufzeit und die Flexibilität, indem Sie das Verzögern der Konfigurations der Komponente auf den Container.


1. Installieren Sie **Unity.Mvc3** NuGet-Paket in der **MvcMusicStore** Projekt. Öffnen Sie hierzu die **Package Manager Console** aus **Ansicht** | **Weitere Fenster**.
2. Führen Sie den folgenden Befehl ein.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installieren von NuGet-Paket Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet-Paket wird installiert.")

    *Unity.Mvc3 NuGet-Paket wird installiert.*
3. Sobald die **Unity.Mvc3** Paket installiert ist, untersuchen Sie die Dateien und Ordner, um die Unity-Konfiguration vereinfachen und somit standardmäßig hinzugefügt.

    ![Unity.Mvc3-Paket installiert](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3-Paket installiert")

    *Unity.Mvc3-Paket installiert*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Aufgabe 3: Registrieren von Unity im Global.asax.cs Anwendung\_starten

In dieser Aufgabe aktualisieren Sie die **Anwendung\_starten** Methode befindet sich im **Global.asax.cs** den Unity-Bootstrapper-Initialisierer aufrufen und dann aktualisieren die Bootstrapper-Datei registrieren der Dienst und der Controller, die Sie für die Abhängigkeitsinjektion verwenden.

1. Sie werden nun der Bootstrapper also die Datei, die den Unity-Container initialisiert und Abhängigkeitskonfliktlöser verknüpfen. Öffnen Sie hierzu **Global.asax.cs** und fügen Sie folgenden hervorgehobenen Code innerhalb der **Anwendung\_starten** Methode.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - initialisieren Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Open **Bootstrapper.cs** Datei.
3. Fügen Sie die folgenden Namespaces hinzu: **MvcMusicStore.Services** und **MusicStore.Controllers**.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Ersetzen Sie **BuildUnityContainer** -Methode den Inhalt durch den folgenden Code, der Speicher-Controller und Store-Dienst registriert.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller und der Dienst*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung aus, um sicherzustellen, dass sie nun nach dem einschließen von Unity geladen werden können.

1. Drücken Sie **F5** zum Ausführen der Anwendung die Anwendung sollte jetzt nicht laden und Fehlermeldungen nicht angezeigt.

    ![Ausführen der Anwendung mit Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Abhängigkeitsinjektion mit Anwendung")

    *Ausführen der Anwendung mit Dependency Injection*
2. Navigieren Sie zu **/speichern**. Dies wird aufgerufen, **StoreController**, er wird nun erstellt, mit **Unity**.

    ![MVC-Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC-Music Store*
3. Schließen Sie den Browser.

In der folgenden Übung erfahren Sie, wie die Abhängigkeitsinjektion Bereich für die Verwendung in ASP.NET MVC-Ansichten und Aktionsfilter zu erweitern.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Übung 2: Räumen eine Sicht

In dieser Übung erfahren Sie, wie die Abhängigkeitsinjektion in einer Sicht mit der neuen Funktionen von ASP.NET MVC 4 für die Unity-Integration verwenden. Zu diesem Zweck rufen Sie einen benutzerdefinierten Dienst in den Speicher durchsuchen Ansicht, die eine Nachricht und eine der folgenden Abbildung angezeigt.

Klicken Sie dann, Sie Unity das Projekt integriert und erstellen eine benutzerdefinierte Abhängigkeitskonfliktlöser zum Einfügen von Abhängigkeiten.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Aufgabe 1: Erstellen einer Ansicht, die einen Dienst nutzt.

In dieser Aufgabe erstellen Sie eine Sicht, die einem Webdienstaufruf auf eine neue Abhängigkeit generieren ausführt. Der Dienst besteht in einem einfachen messaging-Dienst in dieser Lösung enthalten.

1. Öffnen der **beginnen** Projektmappe befindet sich in der **Source\Ex02 Räumen View\Begin** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
      > 
      > Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Enthalten die **MessageService.cs** und die **IMessageService.cs** Klassen befinden sich der **Source \Assets** Ordner im **/Dienste**. Zu diesem Zweck Maustaste **Services** Ordner, und wählen **vorhandenes Element hinzufügen**. Navigieren Sie zum Speicherort der Dateien und enthalten Sie sind.

    ![Hinzufügen von Nachrichtendienst und Dienstschnittstelle](aspnet-mvc-4-dependency-injection/_static/image8.png "Message Service und Dienstschnittstelle hinzufügen")

    *Hinzufügen von Nachrichtendienst und Dienstschnittstelle*

    > [!NOTE]
    > Die **IMessageService** Schnittstelle definiert zwei Eigenschaften, die implementiert werden, indem Sie die **MessageService** Klasse. Diese Eigenschaften -**Nachricht** und **ImageUrl**-speichern Sie die Nachricht und die URL des Bilds angezeigt werden sollen.
3. Erstellen Sie den Ordner **/Seiten** des Projekts root-Ordner, und fügen Sie dann auf die vorhandene Klasse **MyBasePage.cs** aus **Source\Assets**. Die Basisseite, der Sie erbt, hat die folgende Struktur.

    ![Ordner Seiten](aspnet-mvc-4-dependency-injection/_static/image9.png "Ordner Seiten")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Open **Browse.cshtml** Anzeigen von **/Ansichten/Store** Ordner, und stellen sie die von erben **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. In der **Durchsuchen** anzuzeigen, fügen Sie einen Aufruf von **MessageService** anzuzeigenden ein Bild und eine Nachricht vom Dienst abgerufen.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Aufgabe 2: z. B. eine benutzerdefinierte Abhängigkeitskonfliktlöser und eine benutzerdefinierte Ansichtsseite

In der vorherigen Aufgabe eingefügten Sie eine neue Abhängigkeit innerhalb einer Ansicht ein Dienstaufrufs darin ausführen. Sie werden nun diese Abhängigkeit beheben, indem die Abhängigkeitsinjektion für ASP.NET MVC-Schnittstellen implementieren **IViewPageActivator** und **IDependencyResolver**. Sie enthält in der Projektmappe eine Implementierung von **IDependencyResolver** , die befasst sich mit den Abruf des Diensts mithilfe von Unity. Anschließend schließt eine andere benutzerdefinierte Implementierung der **IViewPageActivator** -Schnittstelle, die durch die Erstellung der Sichten behoben werden kann.

> [!NOTE]
> Seit ASP.NET MVC 3 mussten die Implementierung für Zielabhängigkeit die Schnittstellen zum Registrieren von Diensten vereinfacht. **IDependencyResolver** und **IViewPageActivator** sind Teil von ASP.NET MVC 3-Funktionen für Zielabhängigkeit.
> 
> **-IDependencyResolver** Schnittstelle ersetzt den vorherigen IMvcServiceLocator. Implementierer der IDependencyResolver müssen eine Instanz des Diensts oder einer Dienst-Auflistung zurück.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** Schnittstelle bietet eine präzisere Steuerung wie Ansichtsseiten über Abhängigkeitsinjektion instanziiert werden. Die Klassen, implementieren **IViewPageActivator** Schnittstelle kann mithilfe von Kontextinformationen zur Ansicht-Instanzen erstellen.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Erstellen Sie die /**Factorys** -Ordner im Stammordner des Projekts.
2. Umfassen **CustomViewPageActivator.cs** der Projektmappe aus **/Datenquellen/Bestand/** auf **Factorys** Ordner. Zu diesem Zweck Maustaste die **/Factories** Ordner wählen **hinzufügen | Vorhandenes Element** und wählen Sie dann **CustomViewPageActivator.cs**. Diese Klasse implementiert die **IViewPageActivator** Schnittstelle, um die Unity-Container enthalten.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** ist verantwortlich für die Verwaltung der Erstellung einer Sicht mit einem Unity-Container.
3. Umfassen **UnityDependencyResolver.cs** aus der Datei **/Quellen/Bestand** auf **/Factories** Ordner. Zu diesem Zweck Maustaste die **/Factories** Ordner wählen **hinzufügen | Vorhandenes Element** und wählen Sie dann **UnityDependencyResolver.cs** Datei.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** Klasse ist eine benutzerdefinierte DependencyResolver für Unity. Wenn ein Dienst im Unity-Container gefunden werden kann, ist die Basis-Resolver invocated.

In der folgenden Aufgabe werden beide Implementierungen des Modells kennen den Speicherort der Dienste und die Ansichten können registriert werden.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Aufgabe 3 - Registrierung für die Abhängigkeitsinjektion in Unity-Container

In dieser Aufgabe fügen Sie die vorherigen Schritte zusammen, um die Abhängigkeitsinjektion arbeiten stellen ein.

Bis jetzt zu hat Ihre Lösung die folgenden Elemente:

- Ein **Durchsuchen** Ansicht, erbt **MyBaseClass** und nutzt **MessageService**.
- Eine Zwischenklasse -**MyBaseClass**-, die Abhängigkeitsinjektion für die Dienstschnittstelle deklariert wurde.
- Eine Dienst - **MessageService** - und seine Schnittstelle **IMessageService**.
- Eine benutzerdefinierte Abhängigkeitskonfliktlöser für Unity - **UnityDependencyResolver** -, die befasst sich mit den Dienst abrufen.
- Eine Ansichtsseite Activator - **CustomViewPageActivator** -Seite erstellt.

Zum Einfügen von **Durchsuchen** Ansicht jetzt registrieren Sie den benutzerdefinierten Abhängigkeitskonfliktlöser im Unity-Container.

1. Open **Bootstrapper.cs** Datei.
2. Registrieren einer Instanz von **MessageService** in den Unity-Container, um den Dienst zu initialisieren:

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Hinzufügen eines Verweises auf **MvcMusicStore.Factories** Namespace.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Factorys Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrieren Sie **CustomViewPageActivator** als eine Ansichtsseite Activator in den Unity-Container:

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Ersetzen Sie ASP.NET MVC 4-Standard-Abhängigkeitskonfliktlöser mit einer Instanz von **UnityDependencyResolver**. Ersetzen Sie hierzu **Initialise** Methode Inhalt durch folgenden Code:

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Update-Abhängigkeitskonfliktlöser*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC bietet standardmäßig Dependency Resolver-Klasse. Um mit benutzerdefinierten Abhängigkeitskonfliktlöser mit dem arbeiten, die wir für Unity erstellt haben, muss dieser Konfliktlöser ersetzt werden.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung überprüfen, ob der Browser Store nutzt den Dienst und zeigt das Bild und die Nachricht abgerufen:

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Klicken Sie auf **Rock** innerhalb der Genres-Menü und finden Sie unter wie die **MessageService** wurde in der Ansicht injiziert und die Begrüßungsnachricht und das Bild geladen. In diesem Beispiel werden wir eingeben &quot; **Rock**&quot;:

    ![MVC-Music Store - Ansicht Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - Ansicht-Injection")

    *MVC-Music Store - Ansicht-Injection*
3. Schließen Sie den Browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Übung 3: Räumen Aktionsfilter

In der vorherigen praktische Übungseinheit **benutzerdefinierte Aktionsfilter** Sie die Filter-Anpassung und Injection gearbeitet haben. In dieser Übung erfahren Sie, wie Sie Filter mit Abhängigkeitsinjektion einfügen, mit der Unity-Container. Zu diesem Zweck fügen Sie der Projektmappe Music Store eines benutzerdefinierten Aktionsfilters hinzu, das die Aktivität des Standorts zurückverfolgen wird.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Aufgabe 1 – z. B. den Tracking-Filter in der Projektmappe

In dieser Aufgabe werden Sie in der Music Store ein benutzerdefinierten Aktionsfilters für Ablaufverfolgungsereignisse eingefügt. Als benutzerdefinierte Aktionsfilter werden Konzepte bereits in der vorherigen Lektion behandelt &quot;benutzerdefinierte Aktionsfilter&quot;, Sie schließen Sie einfach die Filter-Klasse aus dem Ordner "Assets" in dieser Umgebung, und klicken Sie dann einen Filteranbieter für Unity zu erstellen:

1. Öffnen der **beginnen** Projektmappe befindet sich in der **Source\Ex03 - Räumen Aktion Filter\Begin** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
      > 
      > Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Umfassen **TraceActionFilter.cs** aus der Datei **/Quellen/Bestand** auf **/filtert** Ordner.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Diese benutzerdefinierten Aktionsfilters führt ASP.NET-Ablaufverfolgung. Sehen Sie sich &quot;ASP.NET MVC 4-lokale und dynamische Aktionsfilter&quot; Übung zur weiteren Referenz.
3. Fügen Sie die leere Klasse **FilterProvider.cs** zum Projekt im Ordner ""   **/filtert.**
4. Hinzufügen der **System.Web.Mvc** und **Microsoft.Practices.Unity** Namespaces in **FilterProvider.cs**.

    (Codeausschnitt - *Dependency Injection Lab - Ex03 - Filter ASP.NET-Anbieter hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Legen Sie die Klasse, die von erben **IFilterProvider** Schnittstelle.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Hinzufügen einer **IUnityContainer** Eigenschaft in der **FilterProvider** Klasse, und erstellen Sie einen Klassenkonstruktor, um den Container zuzuweisen.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Filter-Anbieterkonstruktor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Klassenkonstruktor Filter Anbieter ist nicht das Erstellen einer **neue** in Objekt. Der Container wird als Parameter übergeben, und die Abhängigkeit von Unity gelöst ist.
7. In der **FilterProvider** Klasse, implementieren Sie die Methode **GetFilters** aus **IFilterProvider** Schnittstelle.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Filter Anbieter GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Aufgabe 2: registrieren und aktivieren den Filter

In dieser Aufgabe aktivieren Sie Website-Überwachung. Registrieren Sie zu diesem Zweck wird der Filter in **Bootstrapper.cs BuildUnityContainer** Methode, um die Ablaufverfolgung starten:

1. Open **"Web.config"** befindet sich im Projektstammverzeichnis und Trace-Überwachung System.Web-Gruppe aktivieren.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Open **Bootstrapper.cs** auf Stammebene des Projekts.
3. Hinzufügen eines Verweises auf die **MvcMusicStore.Filters** Namespace.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Wählen Sie die **BuildUnityContainer** -Methode und registrieren Sie den Filter im Unity-Container. Sie müssen beim Filteranbieter als auch der Aktionsfilter zu registrieren.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider und Aktionsfilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe, die Sie die Anwendung ausgeführt werden und testen, ob der benutzerdefinierte Aktionsfilter der aktivitätsablaufverfolgung ist:

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Klicken Sie auf **Rock** im Genres-Menü. Sie können auf mehrere Genres durchsuchen, wenn Sie möchten.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Navigieren Sie zu **/Trace.axd** , finden Sie unter der Anwendung-Ablaufverfolgung, und klicken Sie dann auf **"Details anzeigen"**.

    ![Anwendung Ablaufverfolgungsprotokoll](aspnet-mvc-4-dependency-injection/_static/image12.png "Anwendung-Ablaufverfolgungsprotokoll")

    *Application-Ablaufverfolgungsprotokoll*

    ![Anwendung Trace - Anforderungsdetails](aspnet-mvc-4-dependency-injection/_static/image13.png "Anwendung Trace - Anforderungsdetails")

    *Anwendung Trace - Anforderungsdetails*
4. Schließen Sie den Browser.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch diese praktische Übungseinheit haben Sie gelernt, durch die Integration von Unity unter Verwendung eines NuGet-Pakets die Abhängigkeitsinjektion in ASP.NET MVC 4 verwenden. Um dies zu erreichen, haben Sie die Abhängigkeitsinjektion in Controller, Ansichten und Aktionsfilter verwendet.

Es wurden die folgenden Konzepte behandelt:

- Abhängigkeitsinjektion in ASP.NET MVC 4-Funktionen
- Unity-Integration mit Unity.Mvc3 NuGet-Paket
- Abhängigkeitsinjektion in Controllern
- Abhängigkeitsinjektion in Sichten
- Abhängigkeitsinjektion Aktionsfilter

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-dependency-injection/_static/image19.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-dependency-injection/_static/image20.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-dependency-injection/_static/image21.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-dependency-injection/_static/image22.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-dependency-injection/_static/image23.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-dependency-injection/_static/image24.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
