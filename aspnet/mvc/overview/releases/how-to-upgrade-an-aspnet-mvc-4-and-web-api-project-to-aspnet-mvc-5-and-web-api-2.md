---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Zum Aktualisieren eines ASP.NET MVC 4 und Web-API-Projekt für ASP.NET MVC 5 und Web-API 2 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 und Web-API 2 schalten einen Host der neuen Features, einschließlich routing-Attribut, Authentifizierungsfilter und vieles mehr.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874729"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>So aktualisieren Sie eine ASP.NET MVC 4 und Web-API-Projekt für ASP.NET MVC 5 und Web-API 2
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 und Web-API 2 schalten einen Host der neuen Features, einschließlich routing-Attribut, Authentifizierungsfilter und vieles mehr. Finden Sie unter [ https://www.asp.net/vnext ](https://www.asp.net/core) Weitere Details.
> 
> Diese exemplarische Vorgehensweise führt Sie mit den Schritten erforderlich, um Ihre Anwendung auf die neueste Version zu aktualisieren.  
> 
> > [!NOTE]
> > Finden Sie unter [ASP.NET und Webtools für Visual Studio 2013-Versionshinweise](../../../visual-studio/overview/2013/release-notes.md) Informationen über wichtige Änderungen von MVC 4 und Web-API auf die nächste Version.
> 
>   
> 
> In diesem Artikel wurde von Youngjune Hongkong und Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Upgradeschritte

1. Sichern Sie das Projekt. In dieser exemplarischen Vorgehensweise müssen Sie Änderungen an Ihrer Projektdatei, die Paketkonfiguration und die web.config-Dateien.
2. Für das Upgrade von Web-API auf Web-API 2, in der Datei global.asax zu ändern:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   auf

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Stellen Sie sicher, dass alle Pakete, mit denen Ihre Projekte mit MVC 5 und Web-API 2 kompatibel sind. Die folgende Tabelle zeigt, wie die MVC 4 und Web-API im Zusammenhang mit Paketen nicht geändert werden müssen. Wenn Sie ein Paket, die eines der verfügen unten aufgeführten Pakete abhängig ist, wenden Sie sich an den Verlegern, um die neueren Versionen zu erhalten, die mit MVC 5 und Web-API 2 kompatibel sind. Wenn Sie den Quellcode für diese Pakete haben, sollten Sie mit den neuen Assemblys von MVC 5 und Web-API 2 neu kompilieren.   

    | **Paket-Id** | **Alte version** | **Neue version** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | Entfernt |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | Entfernt |
    | Microsoft-Web-Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Webhilfsprogramme wurde durch Microsoft.AspNet.WebHelpers ersetzt. Sie sollten entfernen Sie zuerst die alte Paket, und installieren Sie dann auf neuere Paket.   
    >   
    > Es gibt keine Cross Versionskompatibilität zwischen wichtigen ASP.NET Pakete aus. MVC 5 ist beispielsweise nur mit Razor-3, und nicht die Razor-2 kompatibel.
4. Öffnen Sie das Projekt in Visual Studio 2013.
5. Entfernen Sie eines der folgenden ASP.NET NuGet-Pakete, die installiert werden. Sie werden diese mithilfe der Paket-Manager-Konsole (PMC) entfernt. Wählen Sie zum Öffnen der PMC der **Tools** , und wählen Sie dann im Menü **Bibliotheks-Paket-Manager,** wählen Sie dann **Package Manager Console**. Das Projekt möglicherweise nicht alle diese umfassen.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Dieses Paket wird beim Upgrade von MVC 3 zur MVC 4 in der Regel hinzugefügt. Führen Sie den folgenden Befehl in der Systemmonitor, um es zu entfernen:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Dieses Paket wurde umbenannt wurde, als `Microsoft.AspNet.WebHelpers`. Führen Sie den folgenden Befehl in der Systemmonitor, um es zu entfernen:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Dieses Paket enthält eine Umgehung für einen Programmfehler in MVC 4, die in MVC 5 behoben wurde. Führen Sie den folgenden Befehl in der Systemmonitor, um es zu entfernen:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Aktualisieren Sie alle mit der Systemmonitor ASP.NET NuGet-Pakete. Führen Sie in der Systemmonitor den folgenden Befehl ein:  
    `Update-Package`  
   Die `Update-Package` Befehl ohne Parameter jedes Paket aktualisiert werden. Sie können Pakete einzeln aktualisieren, mit dem ID-Argument. Führen Sie für Weitere Informationen zu den Updatebefehl, `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Aktualisieren Sie die Anwendung *"Web.config"* Datei

Achten Sie darauf, dass Sie diese Änderungen vorgenommen haben, in der app *"Web.config"* Datei nicht das *"Web.config"* in der Datei die *Ansichten* Ordner.

Suchen Sie die `<runtime>/<assemblyBinding>` Abschnitt, und nehmen Sie die folgenden Änderungen:

1. In den Elementen mit dem Namensattribut "System.Web.Mvc" ändern Sie die Versionsnummer von "4.0.0.0" auf "5.0.0.0". (Zwei Änderungen in diesem Element).
2. In Elementen mit dem Namensattribut &quot;System.Web.Helpers "und &quot;System.Web.WebPages&quot; ändern Sie die Versionsnummer von"2.0.0.0","3.0.0.0". Vier Änderungen erfolgt, zwei in jedem der Elemente.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Suchen Sie die `<appSettings>` Abschnitt, und aktualisieren Sie die Webpages:version aus 2.0.0.0.0, 3.0.0.0, wie unten dargestellt:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Entfernen Sie alle Vertrauensebenen als voll. Zum Beispiel:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Update der *"Web.config"* Dateien unter dem Ordner Views

Wenn Ihre Anwendung Bereiche verwendet wird, müssen Sie auch jede aktualisieren *"Web.config"* in der Datei die *Ansichten* Unterordner des Ordners für jeden Bereich.

1. Aktualisieren Sie alle Elemente, die das "System.Web.Mvc" von Version "4.0.0.0" Version "5.0.0.0" enthalten.  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Aktualisieren Sie alle Elemente, die das "System.Web.WebPages.Razor" von Version "2.0.0.0" Version "3.0.0.0" enthalten. Wenn in diesem Abschnitt "System.Web.WebPages" enthält, aktualisieren Sie die enthaltenen Elemente von Version "2.0.0.0" Version "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Wenn Sie entfernt die `Microsoft-Web-Helpers` Installieren von NuGet-Paket in einem vorherigen Schritt `Microsoft.AspNet.WebHelpers` mit dem folgenden Befehl in die PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Wenn Ihre app verwendet die [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) -Methode, fügen Sie Folgendes an der *"Web.config"* Datei.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Abschließende Schritte

Erstellen und Testen der Anwendung.

Entfernen Sie die MVC 4-Projekttyp GUID von Projektdateien.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie dann **Projekt entladen**.
2. Mit der rechten Maustaste des Projekts, und wählen Sie Projektname.csproj bearbeiten.
3. Suchen Sie die `ProjectTypeGuids` -Element, und entfernen die MVC 4-Projekt-GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Speichern Sie und schließen Sie die Projektdatei öffnen.
5. Mit der rechten Maustaste des Projekts, und wählen Sie **Projekt erneut laden**.
