---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 – Abhängigkeitsinjektion | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: Dieser praktischen Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC und ASP.NET MVC 4 haben. Wenn Sie nicht ASP.NET MVC 4-Filter, bevor wir Rec verwendet haben...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827176"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 – Abhängigkeitsinjektion

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse der **ASP.NET MVC** und **ASP.NET MVC 4 filtert**. Wenn Sie nicht verwendet haben **ASP.NET MVC 4 filtert** vor, wir empfehlen Ihnen, sich mit Windows Live ID **Aktionsfilter in ASP.NET MVC benutzerdefinierte** Hands-On Lab.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Trainingskit, erhältlich über enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [Abhängigkeitsinjektion in ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

In **objektorientierten Programmierung** -Paradigma Objekte zusammenarbeiten in einem Modell für die Zusammenarbeit, in denen es Mitwirkende und Benutzer. Natürlich generiert dieses Kommunikationsmodell Abhängigkeiten zwischen Objekten und Komponenten, die immer schwierig zu verwalten, wenn die Komplexität erhöht.

![Klasse von Abhängigkeiten und Modellieren von Komplexität](aspnet-mvc-4-dependency-injection/_static/image1.png "Klasse Abhängigkeiten und Modellieren von Komplexität")

*Abhängigkeiten der Klasse und die Modellkomplexität*

Sie haben wahrscheinlich schon die **Factorymuster** und die Trennung zwischen der Schnittstelle und die Implementierung, die mit Diensten, bei dem die Clientobjekte oft verantwortlich für die dienstidentifizierung.

Der Dependency Injection-Muster ist eine bestimmte Implementierung des Inversion of Control. **Inversion of Control (IoC) steuerungsumkehr** bedeutet, dass Objekte nicht andere Objekte erstellt werden, um ihre Arbeit zu erledigen, auf denen sie basieren. Stattdessen erhalten sie die Objekte, die sie aus einer externen Quelle (z. B. eine XML-Konfigurationsdatei) benötigen.

**Dependency Injection (DI)** bedeutet, dass dies geschieht ohne Eingreifen Objekt, in der Regel von einer Framework-Komponente, die Konstruktorparameter übergibt und legen Sie Eigenschaften fest.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Das Dependency Injection (DI)-Entwurfsmuster

Das Ziel der Abhängigkeitsinjektion auf einer hohen Ebene ist, eine Clientklasse (z. B. *der dsmessages*) benötigt etwas, das eine Schnittstelle erfüllt (z. B. *IClub*). Ist es unerheblich, wie der konkrete Typ ist (z. B. *WoodClub, IronClub, WedgeClub* oder *PutterClub*), Personen, die behandelt werden sollen (z. B. eine gute *Caddy*). Der Abhängigkeitskonfliktlöser in ASP.NET MVC können Sie die Abhängigkeit Logik, die an anderer Stelle zu registrieren (z. B. einen Container oder ein *Behälter Kreuz*).

![Dependency Injection Diagramm](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection-Abbildung")

*Dependency Injection - Golf-Beispiel*

Die Vorteile der Verwendung von Dependency Injection-Muster und Inversion of Control, sind die folgenden:

- Reduziert die Klassenkopplung
- Erhöht die Wiederverwendung von code
- Verbessert die codeverwaltbarkeit von
- Verbessert die Anwendungstests

> [!NOTE]
> Abhängigkeitsinjektion mit Entwurfsmuster "abstrakte Factory" manchmal verglichen wird, aber es gibt ein geringfügigen Unterschied zwischen den beiden Ansätzen. DI ist ein Framework, hinter arbeiten, um Abhängigkeiten zu lösen, indem die Factorys und die registrierten Dienste aufrufen.


Nun, da Sie den Dependency Injection-Muster verstanden haben, werden in dieser Übungseinheit erfahren Sie wie für die anzuwendende in ASP.NET MVC 4. Starten Sie mithilfe der Abhängigkeitsinjektion in die **Controller** eine Datenbank-Access-Dienst umfassen. Als Nächstes Sie Dependency Injection zum Anwenden der **Ansichten** , nutzen einen Dienst und Anzeigen von Informationen. Zum Schluss erweitern die DI auf ASP.NET MVC 4-Filter, Sie Einfügen eines benutzerdefinierten Aktionsfilters in der Projektmappe.

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Integrieren von ASP.NET MVC 4 mit Unity für Dependency Injection, die mithilfe von NuGet-Pakete
- Dependency Injection in ASP.NET MVC-Controller verwenden
- Dependency Injection in ASP.NET MVC-Ansicht verwenden
- Verwenden Sie Dependency Injection in einem ASP.NET MVC-Aktionsfilter

> [!NOTE]
> Diese Übungseinheit Unity.Mvc3 NuGet-Paket für die Auflösung von Abhängigkeiten verwendet, aber es ist möglich, alle Dependency Injection-Framework mit ASP.NET MVC 4 arbeiten anpassen.


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang B: Verwenden von Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:

1. [Übung 1: Einfügen eines Controllers](#Exercise1)
2. [Übung 2: Einfügen einer Ansicht](#Exercise2)
3. [Übung 3: Einfügen von Filtern](#Exercise3)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **30 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Übung 1: Einfügen eines Controllers

In dieser Übung lernen Sie, wie Dependency Injection in ASP.NET MVC-Controller verwendet werden, durch die Integration von Unity unter Verwendung eines NuGet-Pakets. Aus diesem Grund umfasst Sie Dienste in Ihre Controller MvcMusicStore, um die Logik von der Datenzugriff zu trennen. Die Dienste erstellt eine neue Abhängigkeit im controllerkonstruktor, der aufgelöst wird mithilfe der Abhängigkeitsinjektion mit Hilfe der **Unity**.

Dadurch erfahren Sie, wie, sind flexibler und einfacher zu verwalten und Testen weniger verbundene Anwendungen zu generieren. Außerdem lernen Sie, wie Sie ASP.NET MVC mit Unity zu integrieren.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Informationen zu StoreManager Service

Die MVC Music Store jetzt in der Begin-Lösung bereitgestellt enthält einen Dienst, der die Store-Controller-Daten, die mit dem Namen verwaltet **StoreService**. Im folgenden finden Sie die Store-dienstimplementierung. Beachten Sie, dass alle Methoden modellentitäten zurückgeben.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** von der Begin Projektmappe jetzt nutzt **StoreService**. Alle Datenverweise wurden aus entfernt **StoreController**, und nun möglich, ändern, den aktuellen Datenzugriffsanbieter ohne jede Methode, die verbraucht **StoreService**.

Finden Sie nachfolgend die **StoreController** Implementierung hat eine Abhängigkeit mit **StoreService** im Klassenkonstruktor.

> [!NOTE]
> Die Abhängigkeit eingeführt, die in dieser Übung bezieht sich auf **Inversion of Control** (IoC).
> 
> Die **StoreController** Klassenkonstruktor empfängt eine **IStoreService** Typparameter, der zum Ausführen von Dienstaufrufe von innerhalb der Klasse erforderlich ist. Allerdings **StoreController** implementiert nicht den Standardkonstruktor (ohne Parameter), die jedem Controller zum Arbeiten mit ASP.NET MVC.
> 
> Um die Abhängigkeit zu beheben, muss der Controller erstellt werden, indem eine abstrakte Factory (eine Klasse, die jedes Objekt des angegebenen Typs zurückgibt).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Sie erhalten einen Fehler, wenn die Klasse versucht, die StoreController zu erstellen, ohne das Dienstobjekt senden, wie es keinen parameterloser Konstruktor deklariert gibt.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Aufgabe 1: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Begin-Anwendung, die den Dienst in den Store-Controller enthält, die den Datenzugriff von der Anwendungslogik trennt.

Wenn die Anwendung ausgeführt wird, erhalten Sie eine Ausnahme aus, wie der Controller-Dienst nicht als Parameter, wird standardmäßig übergeben wird:

1. Öffnen der **beginnen** Lösung befindet sich in **Source\Ex01 einfügen Controller\Begin**.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Drücken Sie **STRG + F5** um die Anwendung ohne Debuggen auszuführen. Sie erhalten die Fehlermeldung &quot; **kein parameterloser Konstruktor für dieses Objekt definierten**&quot;:

    ![Fehler beim Ausführen von ASP.NET MVC-Begin-Anwendung](aspnet-mvc-4-dependency-injection/_static/image3.png "Fehler während der Ausführung beginnen ASP.NET MVC-Anwendung")

    *Fehler beim Ausführen von ASP.NET MVC-Begin-Anwendung*
3. Schließen Sie den Browser.

Arbeiten Sie in den folgenden Schritten auf die Music Store-Projektmappe, die Abhängigkeit einzufügen, die diesem Controller benötigt.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Aufgabe 2 – einschließlich Unity in MvcMusicStore-Lösung

In dieser Aufgabe enthält **Unity.Mvc3** NuGet-Paket mit der Lösung.

> [!NOTE]
> Unity.Mvc3 Paket wurde für ASP.NET MVC 3 entwickelt, aber es ist vollständig kompatibel mit ASP.NET MVC 4.
> 
> Unity für die Instanz wird von einer einfachen und erweiterbaren Container für Abhängigkeitsinjektion mit optionalem Support und Abfangen von Typen. Es ist ein universell einsetzbarer Container für die Verwendung in eine beliebige Art von .NET-Anwendung. Es enthält alle allgemeinen Funktionen finden Sie in der Dependency Injection Mechanismen, einschließlich: Erstellen von Objekten, die Abstraktion von Anforderungen durch Angeben von Abhängigkeiten auf der Common Language Runtime und Flexibilität durch verzögern der Konfigurations der Komponente auf den Container.


1. Installieren Sie **Unity.Mvc3** NuGet-Paket in der **MvcMusicStore** Projekt. Zu diesem Zweck öffnen Sie die **-Paket-Manager-Konsole** aus **Ansicht** | **Other Windows**.
2. Führen Sie den folgenden Befehl ein.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installieren von NuGet-Paket Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet-Paket installieren")

    *Installieren von NuGet-Paket Unity.Mvc3*
3. Sobald die **Unity.Mvc3** -Paket installiert ist, untersuchen Sie die Dateien und Ordner, die sie automatisch hinzugefügt, um die Unity-Konfiguration zu vereinfachen.

    ![Unity.Mvc3-Paket installiert](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3-Pakets installiert")

    *Unity.Mvc3-Pakets installiert*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Aufgabe 3: Registrieren von Unity in "Global.asax.cs" Anwendung\_starten

In dieser Aufgabe aktualisieren Sie die **Anwendung\_starten** Methode befindet sich in **"Global.asax.cs"** den Unity-Bootstrapper-Initialisierer aufrufen und aktualisieren Sie dann auf die Bootstrapper-Datei registrieren der Dienst und der Controller, die Sie für die Abhängigkeitsinjektion verwenden.

1. Nun werden Sie von der Bootstrapper die Datei, die den Unity-Container initialisiert und Abhängigkeitskonfliktlöser verknüpfen. Öffnen Sie zu diesem Zweck **"Global.asax.cs"** und fügen Sie folgenden hervorgehobenen Code innerhalb der **Anwendung\_starten** Methode.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - initialisieren Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Open **Bootstrapper.cs** Datei.
3. Die folgenden Namespaces: **MvcMusicStore.Services** und **MusicStore.Controllers**.

    (Codeausschnitt - *ASP.NET Dependency Injection, Lab - Ex01 - Bootstrapper, Hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Ersetzen Sie dies **BuildUnityContainer** Methode den Inhalt durch den folgenden Code, der Store-Controller und Store-Dienst registriert.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller und Dienst*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung aus, um sicherzustellen, dass sie nun nach dem einschließen von Unity geladen werden können.

1. Drücken Sie **F5** zum Ausführen der Anwendung, die Anwendung sollte jetzt laden, ohne alle Fehler angezeigt.

    ![Ausführen der Anwendung über Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Ausführen der Anwendung über Dependency Injection")

    *Ausgeführte Anwendung über Dependency Injection*
2. Navigieren Sie zu **/Store**. Dies wird aufgerufen, **StoreController**, das wird jetzt erstellt, mit **Unity**.

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC Music Store*
3. Schließen Sie den Browser.

In den folgenden Übungen lernen Sie, wie Sie den Dependency Injection-Bereich für die Verwendung in ASP.NET MVC-Ansichten und Aktionsfilter zu erweitern.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Übung 2: Einfügen einer Ansicht

In dieser Übung lernen Sie, wie Sie Dependency Injection in einer Ansicht mit den neuen Features von ASP.NET MVC 4 für Unity-Integration zu verwenden. Zu diesem Zweck wird einen benutzerdefinierten Dienst in der Store durchsuchen Ansicht aufgerufen, die eine Nachricht und eine der folgenden Abbildung angezeigt werden.

Klicken Sie dann Sie das Projekt mit Unity zu integrieren und erstellen einen benutzerdefinierte Abhängigkeitskonfliktlöser zum Einfügen der Abhängigkeiten.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Aufgabe 1: Erstellen einer Ansicht, die einen Dienst nutzt

In dieser Aufgabe erstellen Sie eine Ansicht, die führt ein Dienstaufruf aus, um eine neue Abhängigkeit zu generieren. Der Dienst besteht in einen einfachen Messagingdienst in dieser Lösung enthalten.

1. Öffnen der **beginnen** Lösung befindet sich in der **Source\Ex02 einfügen View\Begin** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
      > 
      > Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Enthalten die **MessageService.cs** und die **IMessageService.cs** Klassen befindet sich in der **Source \Assets** Ordner **/Dienste**. Dazu, mit der Maustaste **Services** Ordner, und wählen **vorhandenes Element hinzufügen**. Suchen Sie den Pfad der Dateien, und enthalten Sie sind.

    ![Hinzufügen von Nachrichtendienst und Dienstschnittstelle](aspnet-mvc-4-dependency-injection/_static/image8.png "Nachrichtendienst und Dienstschnittstelle hinzufügen")

    *Hinzufügen von Message-Dienst und Dienstschnittstelle*

    > [!NOTE]
    > Die **IMessageService** Schnittstelle definiert zwei Eigenschaften, die von implementiert die **MessageService** Klasse. Diese Eigenschaften -**Nachricht** und **ImageUrl**– speichern Sie die Nachricht und die URL des Bilds angezeigt werden sollen.
3. Erstellen Sie den Ordner **/Pages** Stammordner des Projekts, und fügen Sie dann die vorhandene Klasse hinzu **MyBasePage.cs** aus **Source\Assets**. Die Basisseite, die, der Sie erbt, hat die folgende Struktur.

    ![Ordner "Pages"](aspnet-mvc-4-dependency-injection/_static/image9.png "Ordner \"Pages\"")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Open **Browse.cshtml** anzeigen **/Views/Store** Ordner, und stellen sie die von erben **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. In der **Durchsuchen** anzuzeigen, fügen Sie einen Aufruf von **MessageService** anzuzeigende ein Bild und eine Nachricht vom Dienst abgerufen.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Aufgabe 2: eine benutzerdefinierte Abhängigkeitskonfliktlöser sowie eine benutzerdefinierte Ansichtsseite

In der vorherigen Aufgabe eingefügt Sie eine neue Abhängigkeit in der Ansicht ein Dienstaufrufs darin ausführen. Nun werden Sie diese Abhängigkeit aufgelöst, durch die Implementierung der Abhängigkeitsinjektion für ASP.NET MVC-Schnittstellen **IViewPageActivator** und **IDependencyResolver**. Sie umfasst in der Projektmappe eine Implementierung von **IDependencyResolver** , die mit dem Dienst abrufen mithilfe von Unity behandeln werden. Klicken Sie dann, enthält Sie eine andere benutzerdefinierte Implementierung der **IViewPageActivator** -Schnittstelle, die die Erstellung der Sichten lösen werden.

> [!NOTE]
> Da ASP.NET MVC 3 mussten die Implementierung für Dependency Injection die Schnittstellen zum Registrieren von Diensten vereinfacht. **IDependencyResolver** und **IViewPageActivator** sind Bestandteil von ASP.NET MVC 3-Funktionen für Dependency Injection.
> 
> **-Der IDependencyResolver** Interface ersetzt die vorherige IMvcServiceLocator. Implementierer der IDependencyResolver müssen es sich um eine Instanz des Dienstes oder eine Sammlung von Diensten zurückgeben.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** Schnittstelle bietet eine präzisere Steuerung wie Ansichtsseiten über Dependency Injection instanziiert werden. Die Klassen, implementieren **IViewPageActivator** Schnittstelle kann Instanzen anzeigen, die mit Kontextinformationen zu erstellen.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Erstellen Sie die /**Factorys** -Ordner im Stammordner des Projekts.
2. Umfassen **CustomViewPageActivator.cs** zu Ihrer Lösung von **/Sources/Assets/** zu **Factorys** Ordner. Zu diesem Zweck Maustaste der **/Factories** Ordner **hinzufügen | Vorhandenes Element** und wählen Sie dann **CustomViewPageActivator.cs**. Diese Klasse implementiert die **IViewPageActivator** Schnittstelle, die den Unity-Container enthalten soll.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** ist verantwortlich für die Verwaltung von die Erstellung einer Ansicht mithilfe eines Unity-Containers.
3. Umfassen **UnityDependencyResolver.cs** Datei **/Quellen/Assets** zu **/Factories** Ordner. Zu diesem Zweck Maustaste der **/Factories** Ordner **hinzufügen | Vorhandenes Element** und wählen Sie dann **UnityDependencyResolver.cs** Datei.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** -Klasse ist eine benutzerdefinierte DependencyResolver für Unity. Wenn ein Dienst in der Unity-Container nicht gefunden wird, ist die Basis-Resolver invocated.

In der folgenden Aufgabe werden beide Implementierungen registriert werden, damit das Modell, das den Speicherort der Dienste und die Ansichten kennen, können.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Aufgabe 3: Registrieren für Dependency Injection in Unity-Container

In dieser Aufgabe fügen Sie die vorherigen Schritte zusammen, um die Stellen Dependency Injection funktioniert ein.

Bis jetzt hat Ihre Lösung die folgenden Elemente:

- Ein **Durchsuchen** Ansicht, erbt **MyBaseClass** und nutzt **MessageService**.
- Eine Zwischenklasse -**MyBaseClass**-Listenfeldsteuerelement mit Abhängigkeitsinjektion, die für die Dienstschnittstelle deklariert.
- Ein Dienst - **MessageService** - und die Schnittstelle **IMessageService**.
- Eine benutzerdefinierte Abhängigkeitskonfliktlöser für Unity - **UnityDependencyResolver** -behandelt, die die Service abrufen.
- Eine Ansichtsseite Activator - **CustomViewPageActivator** -Seite erstellt.

Einzufügende **Durchsuchen** Ansicht nun registrieren Sie den benutzerdefinierten Abhängigkeitskonfliktlöser im Unity-Container.

1. Open **Bootstrapper.cs** Datei.
2. Registrieren einer Instanz von **MessageService** in den Unity-Container, um den Dienst zu initialisieren:

    (Codeausschnitt - *ASP.NET Lab - Ex02 - Register-Nachrichtendienst Injection*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Hinzufügen eines Verweises auf **MvcMusicStore.Factories** Namespace.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Factorys Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrieren Sie **CustomViewPageActivator** als eine Seite anzeigen Activator in Unity-Container:

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Ersetzen von ASP.NET MVC 4-Standard-Abhängigkeitskonfliktlöser mit einer Instanz von **UnityDependencyResolver**. Zu diesem Zweck ersetzen **Initialise** Methode, die mit den folgenden Code:

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Update-Abhängigkeitskonfliktlöser*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC bietet standardmäßige Dependency Resolver-Klasse. Um mit benutzerdefinierten Abhängigkeitskonfliktlöser wie arbeiten, die wir für Unity erstellt haben, muss dieser Konfliktlöser ersetzt werden.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung überprüfen, ob der Browser Store nutzt den Dienst und zeigt das Bild und die Nachricht abgerufen:

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Klicken Sie auf **Rock** innerhalb des Genres-Menü und finden Sie unter wie die **MessageService** an die Ansicht eingefügt wurde und die Willkommensnachricht und das Bild geladen. In diesem Beispiel werden wir eingeben, &quot; **Rock**&quot;:

    ![MVC Music Store - Ansichtsinjektion](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - Ansichtsinjektion")

    *MVC Music Store - Ansichtsinjektion*
3. Schließen Sie den Browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Übung 3: Einfügen von Aktionsfiltern

In der vorhergehenden praktische Übungseinheit **benutzerdefinierte Aktionsfilter** jetzt haben Sie Filter anpassen und Injection. In dieser Übung lernen Sie, wie Sie Filter mit Dependency Injection einfügen, mit der Unity-Container. Zu diesem Zweck fügen Sie mit der Music Store-Lösung eines benutzerdefinierten Aktionsfilters hinzu, das die Aktivitäten des Standorts verfolgen wird.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Aufgabe 1: z. B. die Nachverfolgungs-Filter in der Projektmappe

In dieser Aufgabe werden Sie in der Music Store ein benutzerdefinierten Aktionsfilters zum Verfolgen von Ereignissen einschließen. Als benutzerdefinierte Aktionsfilter werden Konzepte bereits in der vorherigen Lektion behandelt &quot;benutzerdefinierte Aktionsfilter&quot;, Sie werden nur die Filter-Klasse aus dem Ordner "Assets" dieser Übungseinheit, und klicken Sie dann einen Filteranbieter für Unity zu erstellen:

1. Öffnen der **beginnen** Lösung befindet sich in der **Source\Ex03 - Aktion Filter\Begin einfügen** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
      > 
      > Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Umfassen **TraceActionFilter.cs** Datei **/Quellen/Assets** zu **/filtert** Ordner.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Dieser Filter für die benutzerdefinierte Aktion ausführt, mit der ASP.NET-Ablaufverfolgung. Sie können überprüfen, &quot;ASP.NET MVC 4-lokal und dynamische Aktionsfilter&quot; Lab Weitere Verweise.
3. Fügen Sie die leere Klasse **FilterProvider.cs** zum Projekt im Ordner   **/filtert.**
4. Hinzufügen der **System.Web.Mvc** und **Microsoft.Practices.Unity** Namespaces im **FilterProvider.cs**.

    (Codeausschnitt - *Dependency Injection Lab - Ex03 - Filter ASP.NET-Anbieter hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Legen Sie die Klasse, die von erben **IFilterProvider** Schnittstelle.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Hinzufügen einer **IUnityContainer** -Eigenschaft in der **FilterProvider** Klasse, und klicken Sie dann einen Klassenkonstruktor, um die Zuweisung von Container zu erstellen.

    (Codeausschnitt - *ASP.NET Dependency Injection-Lab - Ex03 - Filterkonstruktor Anbieter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Der Klassenkonstruktor des Filter-Anbieter ist nicht das Erstellen einer **neue** -Objekt innerhalb. Der Container als Parameter übergeben wird, und die Abhängigkeit von Unity gelöst wird.
7. In der **FilterProvider** Klasse, implementieren Sie die Methode **GetFilters** aus **IFilterProvider** Schnittstelle.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Filter-Anbieter GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Aufgabe 2: registrieren und aktivieren den Filter

In dieser Aufgabe können Sie die Website nachverfolgen. Zu diesem Zweck, registrieren Sie den Filter im **Bootstrapper.cs BuildUnityContainer** Methode, um die Ablaufverfolgung starten:

1. Open **"Web.config"** befindet sich im Stammverzeichnis des Projekts und Aktivieren der nachverfolgung der Ablaufverfolgung auf "System.Web"-Gruppe.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Open **Bootstrapper.cs** am Stammverzeichnis des Projekts.
3. Hinzufügen eines Verweises auf die **MvcMusicStore.Filters** Namespace.

    (Codeausschnitt - *ASP.NET Dependency Injection, Lab - Ex03 - Bootstrapper, Hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Wählen Sie die **BuildUnityContainer** -Methode und registrieren Sie den Filter im Unity-Container. Sie müssen beim Filteranbieter als auch den Aktionsfilter zu registrieren.

    (Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider und ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe, die Sie die Anwendung ausgeführt werden und testen, ob der Filter für die benutzerdefinierte Aktion die aktivitätsablaufverfolgung ist:

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Klicken Sie auf **Rock** innerhalb des Genres-Menüs. Sie können auf mehrere Genres durchsuchen, wenn Sie möchten.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Navigieren Sie zu **/Trace.axd** angezeigt, und klicken Sie dann auf die Anwendung-Ablaufverfolgung **Details anzeigen**.

    ![Application-Ablaufverfolgungsprotokoll](aspnet-mvc-4-dependency-injection/_static/image12.png "Anwendung-Ablaufverfolgungsprotokoll")

    *Application-Ablaufverfolgungsprotokoll*

    ![-Anwendungsüberwachung - Anforderungsdetails](aspnet-mvc-4-dependency-injection/_static/image13.png "Anwendung verfolgen - Anforderungsdetails")

    *-Anwendungsüberwachung - Request-Details*
4. Schließen Sie den Browser.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Von dieser praktischen Übungseinheit haben Sie gelernt, Dependency Injection in ASP.NET MVC 4 zu verwenden, indem Sie die Integration von Unity unter Verwendung eines NuGet-Pakets. Um dies zu erreichen, haben Sie Dependency Injection in Controllern, Ansichten und Aktionsfilter verwendet.

Es wurden die folgenden Konzepte behandelt:

- Abhängigkeitsinjektion in ASP.NET MVC 4-features
- Unity-Integration mit Unity.Mvc3 NuGet-Paket
- Abhängigkeitsinjektion in Controller
- Abhängigkeitsinjektion in Ansichten
- Abhängigkeitsinjektion von Aktionsfiltern

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-dependency-injection/_static/image19.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-dependency-injection/_static/image20.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-dependency-injection/_static/image21.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-dependency-injection/_static/image22.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-dependency-injection/_static/image23.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-dependency-injection/_static/image24.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
