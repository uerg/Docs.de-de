---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekts zum ASP.NET MVC 5 und Web-API 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: ASP.NET MVC 5 und Web-API 2 schalten eine Vielzahl an neuen Features, wie das attributrouting, Authentifizierungsfilter und vieles mehr.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7ccdc1bf7a1b1b8d5d9c5906eeeab9535b26df6c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369246"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 und Web-API 2 schalten eine Vielzahl an neuen Features, wie das attributrouting, Authentifizierungsfilter und vieles mehr. Finden Sie unter [ https://www.asp.net/vnext ](https://www.asp.net/core) Weitere Details.
> 
> Diese exemplarische Vorgehensweise führt Sie mit den Schritten erforderlich, um die Anwendung auf die neueste Version aktualisieren.  
> 
> > [!NOTE]
> > Informieren Sie sich [ASP.NET and Web Tools für Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md) Informationen zu wichtigen Änderungen von MVC 4 und Web-API auf die nächste Version.
> 
>   
> 
> Dieser Artikel geschrieben wurde, indem Youngjune Hongkong und Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Upgradeschritte

1. Sichern Sie Ihr Projekt. In dieser exemplarischen Vorgehensweise müssen Sie Ihrer Projektdatei, die Paketkonfiguration und die web.config-Dateien ändern.
2. Zum Aktualisieren von Web-API, Web-API 2 in "Global.asax", zu ändern:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   auf

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Stellen Sie sicher, dass alle Pakete, die Ihre Projekte verwenden mit MVC 5 und Web-API 2 kompatibel sind. Die folgende Tabelle zeigt, wie die MVC 4 und Web-API im Zusammenhang mit Paketen als geändert werden müssen. Wenn Sie ein Paket, die eines der verfügen unten aufgeführten Paketen abhängig ist, wenden Sie sich an die Herausgebern aus, um die neueren Versionen zu erhalten, die mit MVC 5 und Web-API-2 kompatibel sind. Wenn Sie den Quellcode für diese Pakete haben, sollten Sie mit den neuen Assemblys von MVC 5 und Web-API 2 neu kompilieren.   

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
    | Microsoft-Web-Hilfsprogramme | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft-Webhilfsprogramme wurde durch Microsoft.AspNet.WebHelpers ersetzt. Sie sollte das alte Paket zuerst entfernen und installieren Sie dann auf das neuere Paket.   
    >   
    > Es gibt keine übergreifenden Versionskompatibilität zwischen wichtige ASP.NET Pakete. Es ist z. B. MVC 5 mit nur Razor-3 und nicht die Razor-2 kompatibel.
4. Öffnen Sie Ihr Projekt in Visual Studio 2013.
5. Entfernen Sie keines der folgenden ASP.NET NuGet-Pakete, die installiert werden. Sie werden diese mithilfe der Paket-Manager-Konsole (PMC) entfernt. Wählen Sie zum Öffnen der PMC die **Tools** Menü, und wählen Sie dann **Bibliothekspaket-Manager,** wählen Sie dann **-Paket-Manager-Konsole**. Das Projekt möglicherweise nicht alle diese umfassen.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Dieses Paket wird in der Regel hinzugefügt werden, beim Upgrade von MVC 3 auf MVC 4. Führen Sie den folgenden Befehl in der PMC, um ihn zu entfernen:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Dieses Paket wurde umbenannt wurde, als `Microsoft.AspNet.WebHelpers`. Führen Sie den folgenden Befehl in der PMC, um ihn zu entfernen:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Dieses Paket enthält eine problemumgehung für einen Fehler in MVC 4, das in MVC 5 behoben wurde. Führen Sie den folgenden Befehl in der PMC, um ihn zu entfernen:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Aktualisieren Sie alle ASP.NET NuGet-Pakete, die Verwendung der PMC. Führen Sie in der PMC die folgenden Befehl ein:  
    `Update-Package`  
   Die `Update-Package` Befehl ohne Parameter alle Pakete aktualisiert werden. Sie können Pakete einzeln aktualisieren, mit dem ID-Argument. Führen Sie für Weitere Informationen zu den Update-Befehl, `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Aktualisieren Sie die Anwendung *"Web.config"* Datei

Achten Sie darauf, dass Sie diese Änderungen in der app vornehmen *"Web.config"* Datei, nicht die *"Web.config"* Datei die *Ansichten* Ordner.

Suchen Sie die `<runtime>/<assemblyBinding>` Abschnitt, und nehmen Sie die folgenden Änderungen:

1. In den Elementen mit dem Namensattribut "System.Web.Mvc" ändern Sie die Versionsnummer von "4.0.0.0" auf "5.0.0.0". (Zwei Änderungen in diesem Element.)
2. Elemente mit dem Namensattribut &quot;System.Web.Helpers "und &quot;System.Web.WebPages&quot; ändern Sie die Versionsnummer von"2.0.0.0","3.0.0.0". Vier Änderungen erfolgt, zwei in jedem der Elemente.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Suchen Sie die `<appSettings>` aus, und aktualisieren Sie die Webpages:version aus 2.0.0.0.0, 3.0.0.0, wie unten dargestellt:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Entfernen Sie alle Vertrauensebenen als voll. Zum Beispiel:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Update der *"Web.config"* Dateien im Ordner "Views"

Wenn Ihre Anwendung Bereiche verwendet wird, müssen Sie auch jede aktualisieren *"Web.config"* Datei die *Ansichten* untergeordneten Ordner jedes Ordners Bereich.

1. Aktualisieren Sie alle Elemente, die "System.Web.Mvc" Version "4.0.0.0" auf Version "5.0.0.0" enthalten.  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Aktualisieren Sie alle Elemente, die "System.Web.WebPages.Razor" Version "2.0.0.0" Version "3.0.0.0" enthalten. Wenn in diesem Abschnitt "System.Web.WebPages" enthält, aktualisieren Sie die Elemente von Version "2.0.0.0", Version "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Wenn Sie entfernt die `Microsoft-Web-Helpers` Installieren von NuGet-Paket in einem vorherigen Schritt `Microsoft.AspNet.WebHelpers` mit dem folgenden Befehl in der PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Wenn Ihre app verwendet die [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) -Methode, fügen Sie den folgenden der *"Web.config"* Datei.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Abschließende Schritte

Erstellen Sie und Testen Sie die Anwendung.

Entfernen Sie die MVC 4-Projekttyp GUID aus den Projektdateien.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Namens des Projekts, und wählen Sie dann **Projekt entladen**.
2. Mit der rechten Maustaste in des Projekts, und wählen Sie Projektname.csproj bearbeiten.
3. Suchen Sie die `ProjectTypeGuids` -Element, und klicken Sie dann entfernen Sie die MVC 4-Projekt-GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Speichern Sie und schließen Sie die Projektdatei öffnen.
5. Mit der rechten Maustaste in des Projekts, und wählen Sie **Projekt erneut laden**.
