---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN-Startup-Klasse Erkennung | Microsoft-Dokumentation
author: Praburaj
description: Dieses Tutorial veranschaulicht das Konfigurieren der OWIN-Startup-Klasse. Für weitere Informationen zu OWIN können Sie sich die Übersicht des Katana-Projekt ansehen. Dieses Tutorial wurde...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 4e753187f1caae646402712c2abc28856ae71a79
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910706"
---
<a name="owin-startup-class-detection"></a>OWIN-Startup-Klasse Erkennung
====================
durch [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Dieses Tutorial veranschaulicht das Konfigurieren der OWIN-Startup-Klasse. Weitere Informationen zu OWIN, finden Sie unter [eine Übersicht des Katana-Projekt](an-overview-of-project-katana.md). Dieses Tutorial wurde von Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan und Howard Dierking ( [ @howard \_Dierking](https://twitter.com/howard_dierking) ) geschrieben.
>
> ## <a name="prerequisites"></a>Vorraussetzungen
>
> [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)


## <a name="owin-startup-class-detection"></a>OWIN-Startup-Klasse Erkennung

 Jede OWIN-Anwendung verfügt über eine Startup-Klasse, in der Sie Komponenten für die Pipeline der Anwendung angeben können. Es gibt verschiedene Möglichkeiten, wie Sie Ihre Startup-Klasse mit der Laufzeit verbinden können, die davon abhängt, welches Hosting-Modell Sie geauswählt haben (OwinHost, IIS und IIS Express). Die Startup-Klasse, die in diesem Tutorial gezeigt wird, kann in jeder Hostanwendung verwendet werden. Nutzen Sie eine dieser Ansätze um die Startup-Klasse, mit der Hosting-Laufzeit zu verbinden:

1. **Benennungskonvention**: Katana durchsucht den globalen Namespace und alle Assemblies nach einer Klasse namens `Startup`.
2. **OwinStartup Attribut**: Dies ist der Ansatz, den die meisten Entwickler zur Angabe der Startklasse verwenden. Das folgende Attribut legt als Startup-Klasse `TestStartup` fest und gibt als ihren Namespace `StartupDemo` an.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   Das `OwinStartup` -Attribut überschreibt die Benennungskonvention. Sie können auch einen Anzeigenamen mit diesem Attribut angeben, jedoch müssen Sie in diesem Fall auch das `appSetting` Element in der Konfigurationsdatei verwenden.
3. **Das AppSetting-Element in der Konfigurationsdatei**: Das `appSetting` -Element überschreibt das `OwinStartup` -Attribut und die Benennungskonvention. Sie können mehrere Starup-Klassen haben (jede mit einem `OwinStartup` Attribut) und können die zu landende Startup-Klasse in einer mit-Markup ähnlichen Konfigurationsdatei wie folgt konfigurieren:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Der folgende Key, der die "Startup"-Klasse und die Assembly explizit angibt kann auch verwendet werden:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Der folgende XML-Code in der Konfigurationsdatei gibt den Anzeigenamen Startup-Klassennamen `ProductionConfiguration` an.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Das obenstehende Markup muss verwendet werden, durch den folgenden `OwinStartup` Attribut, das gibt einen Anzeigenamen und bewirkt, dass die `ProductionStartup2` Klasse ausgeführt.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. So deaktivieren Sie OWIN-Startup-Ermittlung Hinzufügen der `appSetting owin:AutomaticAppStartup` mit einem Wert von `"false"` in der Datei "Web.config".

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Erstellen einer ASP.NET Web-App mithilfe von OWIN-Startup

1. Erstellen Sie eine leere ASP.NET-Webanwendung und nennen sie **StartupDemo**. – Installieren `Microsoft.Owin.Host.SystemWeb` mithilfe des NuGet-Paket-Managers. Aus der **Tools** die Option **NuGet Paket-Manager**, und **Paket-Manager Konsole**. Geben Sie folgenden Befehl ein:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Hinzufügen einer OWIN-Startup-Klasse. In Visual Studio 2013 rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**. – In der **neues Element hinzufügen** Dialogfeld Geben Sie *OWIN* in das Suchfeld ein, und ändern Sie den Namen "Startup.cs", und klicken Sie dann auf **hinzufügen**.

     ![](owin-startup-class-detection/_static/image1.png)

   Das nächste Mal, die Sie hinzufügen möchten eine *Owin-Startklasse*, werden in zur Verfügung, aus der **hinzufügen** im Menü.

     ![](owin-startup-class-detection/_static/image2.png)

   Alternativ können Sie klicken Sie mit der rechten Maustaste auf das Projekt und wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**, und wählen Sie dann die **Owin-Startklasse**.

     ![](owin-startup-class-detection/_static/image3.png)

- Ersetzen Sie den generierten Code in die *"Startup.cs"* Datei durch Folgendes:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  Die `app.Use` Lambda-Ausdruck wird verwendet, um die angegebenen middlewarekomponente, für die OWIN-Pipeline zu registrieren. In diesem Fall wird die Protokollierung von eingehenden Anforderungen vor dem Antworten auf die eingehende Anforderung festgelegt. Die `next` Parameter ist der Delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Aufgabe](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) an die nächste Komponente in der Pipeline. Die `app.Run` Lambda-Ausdruck wird die Pipeline, um eingehende Anforderungen verknüpft und stellt den Antwortmechanismus bereit.
     > [!NOTE]
     > Im obigen Code haben wir eine auskommentiert ist die `OwinStartup` -Attribut, und wir haben die Konvention der Ausführung der Klasse, die mit dem Namen der vertrauenden Seite `Startup` . – drücken Sie ***F5*** zum Ausführen der Anwendung. Drücken Sie mehrmals aktualisieren.

    ![](owin-startup-class-detection/_static/image4.png) Hinweis: Die Anzahl, die in den Abbildungen in diesem Tutorial gezeigt entspricht nicht die Anzahl der verfügbaren. Die Millisekunden-Zeichenfolge wird verwendet, um eine neue Antwort anzuzeigen, wenn Sie die Seite aktualisieren.
  Sehen Sie die Ablaufverfolgungsinformationen in die **Ausgabe** Fenster.

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Fügen Sie weitere Startup-Klassen

In diesem Abschnitt fügen wir eine andere Startup-Klasse. Sie können mehrere OWIN-Startklasse Ihrer Anwendung hinzufügen. Beispielsweise empfiehlt es sich beim Start-Klassen für die Entwicklung, Test und Produktion erstellen.

1. Erstellen Sie eine neue OWIN-Startup-Klasse, und nennen Sie sie `ProductionStartup`.
2. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Drücken Sie F5-Steuerelement, um die app auszuführen. Die `OwinStartup` Attribut gibt an, die Produktions-Startup-Klasse ausgeführt wird.

    ![](owin-startup-class-detection/_static/image6.png)
4. Erstellen Sie eine andere OWIN-Startup-Klasse und nennen sie `TestStartup`.
5. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   Die `OwinStartup` Attribut-Überladung, die oben genannten gibt `TestingConfiguration` als die *benutzerfreundliche* Name der Startup-Klasse.
6. Öffnen der *"Web.config"* Datei, und fügen Sie den Schlüssel der OWIN-App-Start die angibt, den Anzeigenamen der Startup-Klasse:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Drücken Sie F5-Steuerelement, um die app auszuführen. Das app-Einstellungselement verwendet wird, Vorrang, und der Konfiguration ausgeführt wird.

    ![](owin-startup-class-detection/_static/image7.png)
8. Entfernen der *Anzeigenamen* Namen aus der `OwinStartup` -Attribut in der `TestStartup` Klasse.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Ersetzen Sie den Schlüssel zum Systemstart OWIN-App in der *"Web.config"* Datei durch Folgendes:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Wiederherstellen der `OwinStartup` Attribut in jeder Klasse die Attribut-Standardcode, die von Visual Studio generiert:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Die OWIN-App-Start-Schlüssel unten bewirkt, dass die Produktions-Klasse, ausgeführt.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Der letzte Schlüssel zum Systemstart gibt der Startmethode für die Konfiguration an. Die folgende Schlüssel zum Systemstart OWIN-App können Sie zum Ändern des Namens der Configuration-Klasse, `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Verwenden von Owinhost.exe

1. Ersetzen Sie die Datei "Web.config" mit folgendem Markup:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Der letzte Schlüssel wins, also in diesem Fall `TestStartup` angegeben ist.
2. Installieren Sie aus der PMC Owinhost:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navigieren Sie zum Ordner Anwendung (dem Ordner mit den *"Web.config"* Datei) und in eine Eingabeaufforderung und geben:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Im Befehlsfenster wird angezeigt:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Startet einen Browser mit der URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost berücksichtigt die Start-Konventionen, die oben aufgeführten.
5. Im Befehlsfenster die EINGABETASTE, um OwinHost zu beenden.
6. In der `ProductionStartup` -Klasse verwenden, fügen Sie das folgende OwinStartup-Attribut, die angibt, den Anzeigenamen *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. In der Eingabeaufforderung und geben:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Die Produktions-Startup-Klasse wird geladen.
    ![](owin-startup-class-detection/_static/image9.png)
   Die Anwendung verfügt über mehrere Klassen von Start, und in diesem Beispiel weisen wir eine verzögerte die Startup-Klasse, um bis zur Laufzeit zu laden.
8. Testen Sie die folgenden Startoptionen für die Common Language Runtime:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
