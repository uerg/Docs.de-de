---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN-Startup-Klasse Erkennung | Microsoft-Dokumentation
author: Praburaj
description: Dieses Tutorial veranschaulicht das Konfigurieren der OWIN-Startup-Klasse. Weitere Informationen zu OWIN, finden Sie in der Übersicht des Katana-Projekts. Dieses Tutorial wurde...
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
3. **Das AppSetting-Element in der Konfigurationsdatei**: Das `appSetting` -Element überschreibt das `OwinStartup` -Attribut und die Benennungskonvention. Sie können mehrere Starup-Klassen haben (jede mit einem `OwinStartup` Attribut) und können die zu ladende Startup-Klasse in einer mit-Markup ähnlichen Konfigurationsdatei wie folgt konfigurieren:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Der folgende Key, der die "Startup"-Klasse und die Assembly explizit angibt kann auch verwendet werden:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Der folgende XML-Code in der Konfigurationsdatei gibt den Anzeigenamen Startup-Klassennamen `ProductionConfiguration` an.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Das obenstehende Markup muss verwendet mit folgendem Attribut verwendet werden`OwinStartup`, das den Anzeigenamen angibt und bewirkt, dass die `ProductionStartup2` Klasse ausgeführt wird.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Um die OWIN-Startup-Ermittlung zu deaktivieren, fügen Sie der "Web.config" folgendes `appSetting owin:AutomaticAppStartup` mit dem Wert `"false"` hinzu.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Erstellen einer ASP.NET Web-App mithilfe von OWIN-Startup

1. Erstellen Sie eine leere ASP.NET-Webanwendung und nennen Sie sie **StartupDemo**. – Installieren Sie `Microsoft.Owin.Host.SystemWeb` mithilfe des NuGet-Paket-Managers. Wählen Sie aus der **Tools**-Anzeige die Option **NuGet Paket-Manager** und **Paket-Manager Konsole** aus. Geben Sie folgenden Befehl ein:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Hinzufügen einer OWIN-Startup-Klasse. Klicken Sie in Visual Studio 2013 mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**. – Geben Sie *OWIN* in dem **neues Element hinzufügen** Dialogfeld in das Suchfeld ein und ändern Sie den Namen "Startup.cs", klicken Sie anschließend auf **hinzufügen**.

     ![](owin-startup-class-detection/_static/image1.png)

   Wenn Sie das nächste Mal eine *Owin-Startklasse* hinzufügen möchten, wird dies im **hinzufügen** Menü zur Verfügung stehen.

     ![](owin-startup-class-detection/_static/image2.png)

   Alternativ können Sie mit der rechten Maustaste auf das Projekt klicken und wählen **hinzufügen**, wählen Sie dann **neues Element**, und anschließend **Owin-Startklasse**.

     ![](owin-startup-class-detection/_static/image3.png)

- Ersetzen Sie den generierten Code in der *"Startup.cs"* Datei durch Folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  Der `app.Use` Lambda-Ausdruck wird verwendet, um die angegebenen Middlewarekomponente, für die OWIN-Pipeline zu registrieren. In diesem Fall wird die Protokollierung von eingehenden Anforderungen vor dem Antworten auf die eingehende Anforderung festgelegt. Der `next` Parameter ist das Delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Aufgabe](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) an die nächste Komponente in der Pipeline. Der `app.Run` Lambda-Ausdruck verknüpft die Pipeline, um eingehende Anforderungen und stellt den Antwortmechanismus bereit.
     > [!NOTE]
     > Im obigen Code haben wir das `OwinStartup` -Attribut auskommentiert, dadurch tritt die Konvention der Ausführung der Klasse mit dem Namen `Startup` in Kraft. – drücken Sie ***F5*** zum Ausführen der Anwendung. Drücken Sie mehrmals aktualisieren.

    ![](owin-startup-class-detection/_static/image4.png) Hinweis: Die Zahlen, die Sie in den Abbildungen dieses Tutorials sehen, wird nicht den Ihrigen entsprechen. Die Millisekunden-Zeichenfolge wird verwendet, um eine neue Antwort anzuzeigen, wenn Sie die Seite aktualisieren.
  Sie können die Ablaufverfolgungsinformationen im **Ausgabe** Fenster sehen.

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Hinzufügen weiterer Startup-Klassen

In diesem Abschnitt fügen wir eine weitere Startup-Klasse hinzu. Sie können Ihrer Anwendung mehrere OWIN-Startklasse hinzufügen. Beispielsweise empfiehlt es sich eine Start-Klasse für die Entwicklung, die Tests und die Produktion zu erstellen.

1. Erstellen Sie eine neue OWIN-Startup-Klasse, und nennen Sie sie `ProductionStartup`.
2. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Drücken Sie F5, um die app auszuführen. Das `OwinStartup` Attribut gibt an, dass die Produktions-Startup-Klasse ausgeführt wird.

    ![](owin-startup-class-detection/_static/image6.png)
4. Erstellen Sie eine weitere OWIN-Startup-Klasse und nennen Sie sie `TestStartup`.
5. Ersetzen Sie den generierten Code durch den folgenden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   Die `OwinStartup` Attribut-Überladung, die oben genannten wird, gibt `TestingConfiguration` als den *bekannten* Namen der Startup-Klasse an.
6. Öffnen Sie die *"Web.config"* Datei, und fügen Sie den Key OWIN-App-Start hinzu, der den Anzeigenamen der Startup-Klasse bestimmt:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Drücken Sie F5, um die app auszuführen. Das app-Einstellungselement bekommt Vorrang, und die Test Konfiguration wird ausgeführt.

    ![](owin-startup-class-detection/_static/image7.png)
8. Entfernen Sie den *Anzeigenamen* aus dem `OwinStartup` -Attribut in der `TestStartup` Klasse.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Ersetzen Sie den OWIN-App Systemstart Key in der *"Web.config"* Datei durch Folgendes:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Stellen Sie die Version wiederher, in die `OwinStartup` Attribute von Visual Studio generiert wurden:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Der OWIN-App-Start-Key unten bewirkt, dass die Produktions-Klasse, ausgeführt wird.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Der letzte Schlüssel, der Konfiguration, gibt die Startmethode zum Systemstart an. Der folgende Schlüssel erlaubt es Ihnen den Namen der Konfigurations-Klasse in `MyConfiguration` zu ändern.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Verwenden von Owinhost.exe

1. Ersetzen Sie die Datei "Web.config" mit folgendem Markup:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Der letzte Schlüssel gewinnt, also in diesem Fall `TestStartup`.
2. Installieren Sie aus der PMC Owinhost:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navigieren Sie zum Ordner Anwendung (der Ordner mit der *"Web.config"* Datei) und öffnen Sie die Eingabeaufforderung und geben folgendes ein:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Im Befehlsfenster wird folgendes angezeigt:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Starten Sie einen Browser mit der URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost berücksichtigt die Start-Konventionen, die oben aufgeführten ist.
5. Drücken Sie im Befehlsfenster die EINGABETASTE um OwinHost zu beenden.
6. Fügen Sie das folgende OwinStartup-Attribut in der `ProductionStartup` -Klasse hinzu, das den Anzeigenamen *ProductionConfiguration* angibt.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Öffnen Sie die Eingabeaufforderung und geben folgendes ein:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Die Produktions-Startup-Klasse wird geladen.
    ![](owin-startup-class-detection/_static/image9.png)
   Die Anwendung verfügt über mehrere Start-Klassen und in diesem Beispiel weisen wir die Startup-Klasse bis zur Laufzeit zu.
8. Testen Sie die folgenden Startoptionen für die Common Language Runtime:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
