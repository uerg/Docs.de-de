---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Owin-Start Klasse Erkennung | Microsoft Docs
author: Praburaj
description: "Dieses Lernprogramm zeigt, wie zum Konfigurieren der OWIN-Start-Klasse geladen wird. Weitere Informationen zu OWIN finden Sie eine Übersicht über Project Katana. Dieses Lernprogramm wurde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 618f8fa23630dcf9821a54415766dc015694e535
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="owin-startup-class-detection"></a>Erkennung von owin-Start-Klasse
====================
durch [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Dieses Lernprogramm zeigt, wie zum Konfigurieren der OWIN-Start-Klasse geladen wird. Weitere Informationen zu OWIN, finden Sie unter [eine Übersicht über Project Katana](an-overview-of-project-katana.md). Dieses Lernprogramm wurde von Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan und Howard Dierking ( [ @howard \_Dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Erforderliche Komponenten
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Erkennung von owin-Start-Klasse

 Jede OWIN-Anwendung verfügt über eine Autostart-Klasse, in dem Sie Komponenten für die Pipeline der Anwendung angeben. Es gibt verschiedene Möglichkeiten, die Sie mit der Common Language Runtime die Startklasse verbinden können, abhängig von der Host-Modell (OwinHost, IIS und IIS Express) auswählen. Die Startklasse. in diesem Lernprogramm gezeigt, kann in jeder hostanwendung verwendet werden. Sie verbinden die Startklasse. mit der hosting Common Language Runtime verwenden, die einen dieser Ansätze:  

1. **Namenskonvention**: Katana sucht eine Klasse namens `Startup` im Namespace der Name der Assembly oder den globalen Namespace entsprechen.
2. **OwinStartup Attribut**: Dies ist der Ansatz, der meisten Entwickler dauert die Startklasse angeben. Das folgende Attribut wird die Startklasse festgelegt, um die `TestStartup` -Klasse in der `StartupDemo` Namespace. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 Die `OwinStartup` Attribut überschreibt die Benennungskonvention verwendet wird. Sie können auch einen Anzeigenamen mit diesem Attribut angegeben ist, jedoch über einen Anzeigenamen erfordert, dass Sie auch verwenden, die `appSetting` Element in der Konfigurationsdatei.
3. **AppSetting-Element in der Konfigurationsdatei**: die `appSetting` Element überschreibt die `OwinStartup` Attribut und Benennungskonvention. Kann auch über mehrere Autostart-Klassen (jede mit einer `OwinStartup` Attribut) und konfigurieren, welche Startklasse in einer Konfigurationsdatei mit dem Markup ähnlich der folgenden geladen werden:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 Die folgende Schlüssel, der explizit die Startklasse und die Assembly angibt, kann auch verwendet werden: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 Die folgenden XML-Code in der Konfigurationsdatei gibt einen benutzerfreundlichen Start Klassennamen des `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 Die oben genannten Markup muss verwendet werden, mit den folgenden `OwinStartup` -Typattribut gibt einen Anzeigenamen und bewirkt, dass die `ProductionStartup2` Klasse ausgeführt.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. So deaktivieren Sie die OWIN-startermittlung Hinzufügen der `appSetting owin:AutomaticAppStartup` mit einem Wert von `"false"` in der Datei "Web.config".

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Erstellen einer ASP.NET Web-App mithilfe von OWIN-Start

1. Erstellen Sie eine leere ASP.NET-Webanwendung und nennen sie **StartupDemo**. -Installieren Sie `Microsoft.Owin.Host.SystemWeb` mithilfe des NuGet-Paket-Managers. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann **Package Manager Console**. Geben Sie folgenden Befehl ein:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- Fügen Sie eine OWIN-Start-Klasse. In Visual Studio 2013 mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**. – In der **neues Element hinzufügen** Dialogfeld Geben Sie *OWIN* suchen (Feld), und ändern Sie den Namen in Startup.cs, und klicken Sie dann auf **hinzufügen**.  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 Das nächste Mal hinzugefügt werden soll ein *Owin Startklasse*, werden in von der **hinzufügen** im Menü.  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 Alternativ können Sie mit der rechten Maustaste, klicken Sie auf das Projekt und wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**, und wählen Sie dann die **Owin Startklasse**.  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- Ersetzen Sie den generierten Code in der *Startup.cs* Datei durch Folgendes:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 Die `app.Use` Lambda-Ausdruck wird verwendet, um die angegebenen middlewarekomponente, für die OWIN-Pipeline zu registrieren. In diesem Fall werden wir die Protokollierung von eingehenden Anforderungen vor dem Antworten auf die eingehende Anforderung einrichten. Die `next` Parameter wird der Delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Aufgabe](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) an die nächste Komponente in der Pipeline. Die `app.Run` Lambda-Ausdruck, der Pipeline auf eingehende Anforderungen verknüpft, und stellt den Mechanismus für die Antwort.
     > [!NOTE]
     > Im obigen Code haben wir auskommentiert ist die `OwinStartup` -Attribut, und wir sind der Konvention aus der Klasse mit dem Namen der vertrauenden Seite `Startup` .-drücken ***F5*** zum Ausführen der Anwendung. Drücken Sie die aktualisierungstaste mehrmals.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
Hinweis: Die Anzahl der Bilder in diesem Lernprogramm gezeigt entspricht nicht die Zahl angezeigt. Die Millisekunde Zeichenfolge wird verwendet, um eine neue Antwort anzuzeigen, wenn Sie die Seite aktualisieren.  
 Sehen Sie die Ablaufverfolgungsinformationen in die **Ausgabe** Fenster.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Fügen Sie weitere Startup-Klassen

In diesem Abschnitt fügen wir einen anderen Startklasse. Sie können mehrere OWIN-Startklasse für Ihre Anwendung hinzufügen. Sie möchten z. B. Start Klassen für Entwicklungs-, Test- und Produktionszwecke erstellen.

1. Erstellen Sie eine neue OWIN-Start-Klasse, und nennen Sie sie `ProductionStartup`.
2. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Drücken Sie F5-Steuerelement, um die app auszuführen. Die `OwinStartup` Attribut gibt an, die Produktion Startklasse ausgeführt wird.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Erstellen Sie eine andere OWIN-Start-Klasse, und nennen Sie sie `TestStartup`.
5. Ersetzen Sie den generierten Code durch den folgenden:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 Die `OwinStartup` Attribut-Überladung, die oben genannten gibt `TestingConfiguration` als die *angezeigten* -Namen der Start-Klasse.
6. Öffnen der *"Web.config"* Datei, und fügen Sie den OWIN-App-Systemstartschlüssel gibt den Anzeigenamen der Startklasse an:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Drücken Sie F5-Steuerelement, um die app auszuführen. Das app-Einstellungen-Element akzeptiert Vorgänger und den Test Konfiguration ausgeführt wird.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Entfernen der *angezeigten* Namen aus der `OwinStartup` Attribut in der `TestStartup` Klasse.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Ersetzen Sie den Schlüssel zum Systemstart OWIN-App in der *"Web.config"* Datei durch Folgendes:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Wiederherstellen der `OwinStartup` Attribut in jeder Klasse, um den Standardcode-Attribut von Visual Studio generiert:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 Jede der OWIN-App-Systemstartschlüssel unten führt dazu, dass die Produktions-Klasse zum Ausführen. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 Die letzte Schlüssel zum Systemstart gibt die Startmethode der Konfiguration an. Die folgenden Schlüssel zum Systemstart OWIN-App können Sie den Namen der Konfigurationsklasse, die ändern `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Verwenden von Owinhost.exe

1. Ersetzen Sie die Datei "Web.config" durch das folgende Markup:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 Der Schlüssel für die zuletzt wins, dies in diesem Fall `TestStartup` angegeben ist.
2. Installieren Sie Owinhost aus der PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navigieren Sie zum Ordner Anwendung (Ordner mit der *"Web.config"* Datei) und in einem Eingabeaufforderungsfenster und Typ: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 Es wird im Befehlsfenster angezeigt: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Starten Sie einen Browser mit der URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost berücksichtigt die Start-Konventionen, die oben aufgeführten.
5. In das Befehlsfenster die EINGABETASTE, um OwinHost zu beenden.
6. In der `ProductionStartup` -Klasse verwenden, fügen Sie das folgende OwinStartup-Attribut gibt einen Anzeigenamen an *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. In der Eingabeaufforderung und geben: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 Die Startklasse Produktion wird geladen.  
    ![](owin-startup-class-detection/_static/image9.png)  
 Anwendung hat mehrere Autostart-Klassen, und in diesem Beispiel haben wir die Startklasse. bis zur Laufzeit laden verzögert.
8. Testen Sie die folgenden Startoptionen für die Common Language Runtime:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
