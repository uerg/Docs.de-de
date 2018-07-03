---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profile, Designs und Webparts | Microsoft-Dokumentation
author: microsoft
description: Es gibt wesentliche Änderungen in der Konfiguration und Instrumentierung in ASP.NET 2.0. Die neue ASP.NET-Konfigurations-API ermöglicht Änderungen an der Konfiguration Pull Request vorgenommen werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 505f03d339eebc871726fc5b3c385e595ad523d4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378455"
---
<a name="profiles-themes-and-web-parts"></a>Profile, Designs und Webparts
====================
durch [Microsoft](https://github.com/microsoft)

> Es gibt wesentliche Änderungen in der Konfiguration und Instrumentierung in ASP.NET 2.0. Die neue ASP.NET-Konfigurations-API ermöglicht Änderungen an der Konfiguration programmgesteuert erfolgen. Darüber hinaus viele neue Konfigurationseinstellungen vorhanden ermöglichen neue Konfigurationen und Instrumentation.


ASP.NET 2.0 stellt eine deutliche Verbesserung der im Bereich der personalisierten Websites dar. Neben der Mitgliedschaft Features sechsteilige bereits behandelt bedeutend erweitert werden ASP.NET-Profile, Designs und Webparts-Personalisierung in Websites.

## <a name="aspnet-profiles"></a>ASP.NET-Profile

ASP.NET-Profilen ähneln Sitzungen. Der Unterschied besteht darin, dass ein Profil dauerhaft ist, während eine Sitzung verloren geht, wenn der Browser geschlossen wird. Ein weiterer wichtiger Unterschied zwischen Sitzungen und Profilen ist, dass Profile stark typisiert sind, aus diesem Grund bietet Ihnen IntelliSense während des Entwicklungsprozesses.

Ein Profil wird in der Konfigurationsdatei für den Computer oder die Datei "Web.config" für die Anwendung definiert. (Sie können nicht in der Datei web.config Unterordner ein Profil definieren.) Der folgende Code definiert ein Profil zum Speichern zuerst die Website-Besucher und Nachnamen.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Der Standarddatentyp für eine Profileigenschaft ist System.String. Im obigen Beispiel wurde kein Datentyp angegeben. Aus diesem Grund sind die FirstName und LastName-Eigenschaften von Typ "String". Wie bereits erwähnt, Profile, die Eigenschaften, stark typisiert sind. Der folgende Code Fügt eine neue Eigenschaft für das Alter, die vom Typ Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profile werden bei der Formularauthentifizierung mit ASP.NET in der Regel verwendet werden. Wenn in Kombination mit der Formularauthentifizierung verwendet wird, hat jeder Benutzer ein eigenes Profil, das mit ihrer Benutzer-ID verknüpft ist Es ist jedoch auch möglich, die Verwendung von Profilen in einer anonymen mithilfe ermöglichen die &lt;AnonymousIdentification&gt; Element in der Konfigurationsdatei zusammen mit den **"AllowAnonymous"** als Attribut Im folgenden dargestellt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Wenn ein anonymer Benutzer die Website aufruft, erstellt ASP.NET eine Instanz des **ProfileCommon** für den Benutzer. Dieses Profil verwendet eine eindeutige ID, die in einem Cookie im Browser gespeichert, zur Identifizierung des Benutzers als eindeutige Besucher. Auf diese Weise können Sie Informationen zum Eigenschaftsprofil für Benutzer speichern anonym beim Durchsuchen.

## <a name="profile-groups"></a>Profilgruppen

Es ist möglich, Eigenschaften von Profilen. Nach Eigenschaften gruppieren ist es möglich, mehrere Profile für eine bestimmte Anwendung zu simulieren.

Die folgende Konfiguration konfiguriert eine FirstName und LastName-Eigenschaft für zwei Gruppen; Käufer und Interessenten.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Es ist dann möglich, die Eigenschaften für eine bestimmte Gruppe wie folgt festgelegt:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Das Speichern von komplexen Objekten

Bisher wurden in den Beispielen dargelegten einfachen Datentypen in einem Profil gespeichert. Es ist auch möglich, komplexe Datentypen in einem Profil zu speichern, durch Angeben der Methode der Verwendung der Serialisierung die **SerializeAs** -Attribut wie folgt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In diesem Fall ist der Typ PurchaseInvoice. Die PurchaseInvoice-Klasse als serialisierbar markiert werden muss und kann eine beliebige Anzahl von Eigenschaften enthalten. Wenn PurchaseInvoice eine Eigenschaft namens hat z. B. **NumItemsPurchased**, sehen Sie sich diese Eigenschaft im Code wie folgt:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil-Vererbung

Es ist möglich, ein Profil für die Verwendung in mehreren Anwendungen zu erstellen. Erstellen Sie eine Profilklasse, die von ProfileBase abgeleitet wird, können Sie ein Profil in mehreren Anwendungen wiederverwenden, indem Sie mit der **erbt** -Attribut an, wie unten dargestellt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In diesem Fall die Klasse **PurchasingProfile** sieht wie folgt:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Profilanbieter

ASP.NET-Profile verwenden Sie das Anbietermodell. Der Standardanbieter speichert die Informationen in einer SQL Server Express-Datenbank in der App\_Datenordner der Webanwendung mit dem Anbieter SqlProfileProvider. Wenn die Datenbank nicht vorhanden ist, wird dieses durch die ASP.NET automatisch erstellt, wenn das Profil versucht wird, um Informationen zu speichern.

In einigen Fällen können jedoch Sie Ihren eigenen Profilanbieter entwickeln möchten. Das Profilfeature von ASP.NET können Sie problemlos andere Anbieter verwenden.

Sie erstellen einen benutzerdefinierten Profilprovider bei:

- Sie müssen speichern Profilinformationen in einer Datenquelle, z. B. in einer FoxPro-Datenbank oder einer Oracle-Datenbank, die nicht durch den Profilanbietern mit .NET Framework unterstützt wird.
- Sie müssen zum Verwalten von Profilinformationen mit einem Datenbankschema, das aus dem Datenbankschema, der in .NET Framework enthaltenen Anbieter unterscheidet. Ein gängiges Beispiel ist, dass Sie Benutzerdaten in einer vorhandenen SQL Server-Datenbank Informationen zum Eigenschaftsprofil integrieren möchten.

### <a name="required-classes"></a>Klassen, die erforderlich

Um einen Profilanbieter zu implementieren, erstellen Sie eine Klasse, die abstrakte System.Web.Profile.ProfileProvider-Klasse erbt. Die **ProfileProvider** abstrakte Klasse erbt wiederum die System.Configuration.SettingsProvider abstrakte Klasse, die die abstrakte System.Configuration.Provider.ProviderBase-Klasse erbt. Aufgrund dieser Vererbungskette zusätzlich zu den erforderlichen Member der **ProfileProvider** -Klasse, Sie müssen die erforderlichen Member implementieren die **SettingsProvider** und  **ProviderBase** Klassen.

Die folgende Tabelle beschreibt die Eigenschaften und Methoden, die Sie, von implementieren müssen der **ProviderBase**, **SettingsProvider**, und **ProfileProvider** abstrakte Klassen.

### <a name="providerbase-members"></a>ProviderBase-Member

| **Member** | **Beschreibung** |
| --- | --- |
| Initialize-Methode | Verwendet als Eingabe den Namen der Anbieterinstanz und ein NameValueCollection-Konfigurationseinstellungen. Zum Festlegen von Optionen und die Werte für die Anbieterinstanz, einschließlich implementierungsspezifische Werte und Optionen, die in der Konfiguration des Computers oder der Datei "Web.config" angegeben. |

### <a name="settingsprovider-members"></a>SettingsProvider Mitglieder

| **Member** | **Beschreibung** |
| --- | --- |
| ApplicationName-Eigenschaft | Der Name der Anwendung, die mit jedem Profil gespeichert sind. Der Profilanbieter verwendet den Namen der Anwendung zum Speichern von Informationen zum Eigenschaftsprofil für jede Anwendung getrennt. Dadurch können mehrere ASP.NET-Anwendungen verwenden Sie die gleiche Datenquelle ohne Konflikte aus, wenn ein Benutzername in verschiedenen Anwendungen erstellt wird. Alternativ können mehrere ASP.NET-Anwendungen eine Profil-Datenquelle freigeben, indem Sie den gleichen Anwendungsnamen angeben. |
| GetPropertyValues-Methode | Verwendet als Eingabe einen SettingsContext und ein SettingsPropertyCollection-Objekt. Die **SettingsContext** enthält Informationen über den Benutzer. Sie können die Informationen als Primärschlüssel verwenden, Profileigenschafteninformationen für den Benutzer abrufen. Verwenden der **SettingsContext** Objekt auf, um den Benutzernamen ein und gibt an, ob der Benutzer authentifiziert oder anonym ist. Die **SettingsPropertyCollection** enthält eine Auflistung von SettingsProperty-Objekten. Jede **SettingsProperty** Objekt enthält den Namen und Typ der Eigenschaft als auch zusätzliche Informationen wie z. B. der Standardwert für die Eigenschaft und gibt an, ob die Eigenschaft schreibgeschützt ist. Die **GetPropertyValues** Methode füllt eine SettingsPropertyValueCollection mit SettingsPropertyValue-Objekte, die auf der Grundlage der **SettingsProperty** Objekte, die als Eingabe bereitgestellt. Die Werte aus der Datenquelle für den angegebenen Benutzer werden an den PropertyValue-Eigenschaften zugewiesen, für die einzelnen **SettingsPropertyValue** -Objekt und die gesamte Auflistung wird zurückgegeben. Aufrufen der Methode aktualisiert wird, wird damit auch den Wert "LastActivityDate" für das Profil für die angegebenen Benutzer auf das aktuelle Datum und die Uhrzeit. |
| SetPropertyValues-Methode | Verwendet als Eingabe eine **SettingsContext** und **SettingsPropertyValueCollection** Objekt. Die **SettingsContext** enthält Informationen über den Benutzer. Sie können die Informationen als Primärschlüssel verwenden, Profileigenschafteninformationen für den Benutzer abrufen. Verwenden der **SettingsContext** Objekt auf, um den Benutzernamen ein und gibt an, ob der Benutzer authentifiziert oder anonym ist. Die **SettingsPropertyValueCollection** enthält eine Auflistung von **SettingsPropertyValue** Objekte. Jede **SettingsPropertyValue** Objekt enthält den Namen, Typ und Wert der Eigenschaft als auch zusätzliche Informationen wie z. B. der Standardwert für die Eigenschaft und gibt an, ob die Eigenschaft schreibgeschützt ist. Die **SetPropertyValues** Methode aktualisiert die Profileigenschaftswerte in der Datenquelle für den angegebenen Benutzer. Aufrufen der Methode auch Updates der **LastActivityDate** und LastUpdatedDate-Werte für die angegebene Benutzerprofil auf dem aktuellen Datum und Uhrzeit. |

### <a name="profileprovider-members"></a>ProfileProvider Mitglieder

| **Member** | **Beschreibung** |
| --- | --- |
| DeleteProfiles-Methode | Verwendet als Eingabe ein Zeichenfolgenarray der Benutzer, Namen, und werden alle Profile und Eigenschaftswerte für den angegebenen Namen aus der Datenquelle gelöscht, wobei die Namen der Anwendung entspricht der **ApplicationName** -Eigenschaftswert. Wenn Sie die Datenquelle Transaktionen unterstützt, empfiehlt es sich, dass Sie alle Löschvorgänge in einer Transaktion enthalten, und dass Sie ein der Transaktion Rollback und löst eine Ausnahme aus, wenn Delete-Vorgang ein Fehler auftritt. |
| DeleteProfiles-Methode | Akzeptiert als Eingabe eine Auflistung von ProfileInfo Objekte aus der Datenquelle alle Profile und Eigenschaftswerte für jedes Profil entspricht, in dem auf den Namen der Anwendung die **ApplicationName** -Eigenschaftswert. Wenn Sie die Datenquelle Transaktionen unterstützt, empfiehlt es sich, dass Sie alle Löschvorgänge in einer Transaktion enthalten ein der Transaktion Rollback und löst eine Ausnahme aus, wenn Delete-Vorgang ein Fehler auftritt. |
| DeleteInactiveProfiles-Methode | Übernimmt als Eingabewert ein ProfileAuthenticationOption und DateTime-Objekt und löschungen aus der Quelle alle Profilinformationen und Standardwerte für die Eigenschaft, in denen das Datum der letzten Aktivität, die kleiner oder gleich dem angegebenen Datum und Uhrzeit ist und den Namen der Anwendung entspricht der **ApplicationName** -Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile, authentifizierte Profile oder alle Profile gelöscht werden soll. Wenn Sie die Datenquelle Transaktionen unterstützt, empfiehlt es sich, dass Sie alle Löschvorgänge in einer Transaktion enthalten ein der Transaktion Rollback und löst eine Ausnahme aus, wenn Delete-Vorgang ein Fehler auftritt. |
| GetAllProfiles-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert, der eine ganze Zahl, der angibt, den Seitenindex, der eine ganze Zahl, der angibt, die Größe einer Seite und einen Verweis auf eine ganze Zahl, die auf die Gesamtzahl von Profilen festgelegt werden. Gibt eine ProfileInfoCollection, die enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, die den Namen der Anwendung, in denen entspricht der **ApplicationName** -Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile, authentifizierte Profile oder alle Profile zurückgegeben werden. Die Ergebnisse der **GetAllProfiles** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** zurückzugebenden in Objekte der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 für die erste Seite steht. Der Parameter für die Gesamtanzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), die auf die Gesamtzahl von Profilen festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Indexwert von Seite 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben, das sechste bis zehnte Profile enthält. Der Wert für die Gesamtanzahl der Datensätze wird auf 13 festgelegt, beim Beenden der Methode. |
| GetAllInactiveProfiles-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert eine **"DateTime"** -Objekt, eine ganze Zahl, der angibt, den Seitenindex, eine ganze Zahl, der angibt, die Größe einer Seite und einen Verweis auf eine ganze Zahl, die festgelegt werden um die Gesamtzahl von Profilen. Gibt eine **ProfileInfoCollection** , enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, in denen das Datum der letzten Aktivität kleiner oder gleich dem angegebenen ist **"DateTime"**  und den Namen der Anwendung, in denen entspricht der **ApplicationName** -Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile, authentifizierte Profile oder alle Profile zurückgegeben werden. Die Ergebnisse der **GetAllInactiveProfiles** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** zurückzugebenden in Objekte der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 für die erste Seite steht. Der Parameter für die Gesamtanzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), die auf die Gesamtzahl von Profilen festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Indexwert von Seite 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben, das sechste bis zehnte Profile enthält. Der Wert für die Gesamtanzahl der Datensätze wird auf 13 festgelegt, beim Beenden der Methode. |
| FindProfilesByUserName-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert eine Zeichenfolge, enthält einen Benutzernamen, eine ganze Zahl, der angibt, den Seitenindex, eine ganze Zahl, der angibt, die Größe einer Seite und einen Verweis auf eine ganze Zahl, die auf die Gesamtzahl der festgelegt wird Profile. Gibt eine **ProfileInfoCollection** , enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, in denen der Benutzername den angegebenen Benutzernamen übereinstimmt und den Namen der Anwendung, in denen entspricht der **ApplicationName** -Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile, authentifizierte Profile oder alle Profile zurückgegeben werden. Wenn Ihre Datenquelle zusätzliche Suchfunktionen, z. B. Platzhalterzeichen unterstützt, können Sie umfangreichere Suchfunktionen für Benutzernamen bereitstellen. Die Ergebnisse der **FindProfilesByUserName** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** zurückzugebenden in Objekte der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 für die erste Seite steht. Der Parameter für die Gesamtanzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), die auf die Gesamtzahl von Profilen festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Indexwert von Seite 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben, das sechste bis zehnte Profile enthält. Der Wert für die Gesamtanzahl der Datensätze wird auf 13 festgelegt, beim Beenden der Methode. |
| FindInactiveProfilesByUserName-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert, der eine Zeichenfolge, enthält einen Benutzernamen ein, eine **"DateTime"** -Objekt, eine ganze Zahl, der angibt, den Seitenindex, eine ganze Zahl, die die Seitengröße angibt und ein Ein Verweis auf eine ganze Zahl, die auf die Gesamtzahl von Profilen festgelegt werden. Gibt eine **ProfileInfoCollection** , enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, in denen der Benutzername den angegebenen Benutzernamen übereinstimmt, in denen das Datum der letzten Aktivität ist kleiner als oder gleich der angegebenen **"DateTime"**, und den Namen der Anwendung, in denen entspricht der **ApplicationName** -Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile, authentifizierte Profile oder alle Profile zurückgegeben werden. Wenn Ihre Datenquelle zusätzliche Suchfunktionen, z. B. Platzhalterzeichen unterstützt, können Sie umfangreichere Suchfunktionen für Benutzernamen bereitstellen. Die Ergebnisse der **FindInactiveProfilesByUserName** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Wert für die Seitengröße gibt die maximale Anzahl von **ProfileInfo** zurückzugebenden in Objekte der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 für die erste Seite steht. Der Parameter für die Gesamtanzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), die auf die Gesamtzahl von Profilen festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Indexwert von Seite 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben, das sechste bis zehnte Profile enthält. Der Wert für die Gesamtanzahl der Datensätze wird auf 13 festgelegt, beim Beenden der Methode. |
| GetNumberOfInActiveProfiles-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert und einem **"DateTime"** Objekt und gibt die Anzahl aller Profile in der Datenquelle, in denen das Datum der letzten Aktivität kleiner als oder gleich der angegebenen ist,zurück. **"DateTime"** und den Namen der Anwendung, in denen entspricht der **ApplicationName** -Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile, authentifizierte Profile oder alle Profile, die gezählt werden. |

### <a name="applicationname"></a>ApplicationName

Da Profilanbieter Profilinformationen separat für jede Anwendung speichern, müssen Sie sicherstellen, dass das Datenschema den Namen der Anwendung enthält, und, dass Abfragen und Aktualisierungen auch den Namen der Anwendung enthalten. Beispielsweise der folgende Befehl dient zum Abrufen eines Eigenschaftswerts aus einer Datenbank, die basierend auf den Namen des Benutzers und gibt an, ob das Profil anonym ist, und stellt sicher, dass die **ApplicationName** Wert ist in der Abfrage enthalten.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET-Designs

## <a name="what-are-aspnet-20-themes"></a>Was sind die Designs für ASP.NET 2.0?

Einer der wichtigsten Aspekte einer Webanwendung ist eines konsistenten Aussehens und Verhaltens für den Standort aus. Cascading Stylesheets (CSS) werden von ASP.NET 1.x-Entwickler in der Regel zum Implementieren von eines konsistenten Aussehens und Verhaltens verwenden. ASP.NET 2.0 Designs wird erheblich bei CSS verbessern, da sie die Möglichkeit, die die Darstellung von ASP.NET-Serversteuerelementen als auch für HTML-Elemente definieren die ASP.NET-Entwickler ermöglichen. ASP.NET-Designs können einzelne Steuerelemente, einer bestimmten Webseite oder eine ganze Webanwendung angewendet werden. Designs verwenden eine Kombination von CSS-Dateien, eine optionale Skindatei und eine optionale Verzeichnis "Images" aus, wenn Abbilder erforderlich sind. Die Designdatei steuert die visuelle Darstellung von ASP.NET-Serversteuerelementen.

## <a name="where-are-themes-stored"></a>Wo befinden sich Designs gespeichert?

Der Speicherort Designs sind, unterscheidet sich basierend auf ihren Bereich. Designs, die an eine beliebige Anwendung angewendet werden können, werden in den folgenden Ordner gespeichert:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Ein Design, das für eine bestimmte Anwendung befindet sich in einem `App\_Themes\<Theme\_Name>` Verzeichnis im Stammverzeichnis der Website.

> [!NOTE]
> Eine Skin-Datei sollte nur Eigenschaften ändern, die Darstellung zu beeinflussen.

Ein globales Design ist ein Design, das auf alle Anwendungen oder Websites, die auf dem Webserver ausgeführten angewendet werden kann. Diese Themen werden standardmäßig im Verzeichnis ASP.NETClientfiles\Themes gespeichert, die innerhalb des Verzeichnisses v2.x.xxxxx ist. Alternativ können Sie die Dateien in das Aspnet verschieben\_Clientsystem/\_Web / [Version] /Themes/ [Design\_Name] Ordner im Stammverzeichnis Ihrer Website.

Anwendungsspezifische Designs können nur für die Anwendung angewendet werden, in denen die Dateien befinden. Diese Dateien werden gespeichert, der `App\_Themes/<theme\_name>` Verzeichnis im Stammverzeichnis der Website.

## <a name="the-components-of-a-theme"></a>Die Komponenten eines Designs

Ein Design besteht aus einer oder mehreren CSS-Dateien, eine optionale Skindatei und einen optionalen Images-Ordner. Die CSS-Dateien können ein beliebiger Name sein soll (d. h. "default.CSS" oder theme.css usw.) und muss sich im Stammverzeichnis des Themes-Ordner. Die CSS-Dateien werden verwendet, um normale CSS-Klassen und Attribute für bestimmte Selektoren zu definieren. Um einen der CSS-Klassen in einem Seitenelement, übernehmen die **CSSClass** Eigenschaft wird verwendet.

Die Designdatei ist eine XML-Datei, die Definitionen von Eigenschaften für ASP.NET-Serversteuerelemente enthält. Die nachstehend aufgeführten Code ist eine Beispiel-Designdatei.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Abbildung 1** unten zeigt eine kleine ASP.NET-Seite durchsucht, ohne ein Design angewendet. **Abbildung 2** zeigt die gleiche Datei mit zugewiesenem Design. Die Hintergrundfarbe und die Textfarbe werden über eine CSS-Datei konfiguriert. Die Darstellung der Schaltfläche und ein Textfeld werden mithilfe der oben aufgeführten Skindatei konfiguriert.


![Ist kein Design](profiles-themes-and-web-parts/_static/image1.gif)

**Abbildung 1**: kein Design


![Design](profiles-themes-and-web-parts/_static/image2.gif)

**Abbildung 2**: Design


Die oben aufgeführten Designdatei definiert eine Skin für einen Standardwert für alle TextBox-Steuerelemente und Schaltflächen-Steuerelemente. Das bedeutet, dass auf diese Darstellung alle TextBox-Steuerelement und ein Button-Steuerelement, die auf einer Seite eingefügt dauert. Sie können auch definieren, ein Design, die auf bestimmte Instanzen dieser Steuerelemente mit angewendet werden, kann die **SkinID** -Eigenschaft des Steuerelements.

Der folgende Code definiert eine Skin für ein Schaltflächen-Steuerelement. Nur die Schaltflächen-Steuerelemente mit einer **SkinID** Eigenschaft **GoButton** gelangen auf die Darstellung des Designs.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Sie können nur ein Standarddesign pro Server Steuerelementtyp haben. Wenn Sie zusätzliche Designs benötigen, sollten Sie die Eigenschaft SkinID verwenden.

## <a name="applying-themes-to-pages"></a>Anwenden von Designs auf Seiten

Ein Design kann mithilfe einer der folgenden Methoden angewendet werden:

- In der &lt;Seiten&gt; -Element der Datei "Web.config"
- In der @Page -Direktive der Seite
- Programmgesteuert

## <a name="applying-a-theme-in-the-configuration-file"></a>Anwenden eines Designs in der Konfigurationsdatei

Um ein Design in der Konfigurationsdatei für Anwendungen anzuwenden, verwenden Sie die folgende Syntax:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Der hier angegebene Name des Designs muss der Name des Ordners Designs übereinstimmen. Dieser Ordner kann entweder in einem der Standorte, die weiter oben in diesem Kurs vorhanden sein. Wenn Sie versuchen, ein Design anwenden, die nicht vorhanden ist, wird ein Fehler bei der Konfiguration auftreten.

## <a name="applying-a-theme-in-the-page-directive"></a>Anwenden eines Designs in der Page-Direktive

Sie können auch ein Design in der @ Page-Direktive anwenden. Dieser Methode können Sie ein Design für eine bestimmte Seite verwenden.

Ein Design in der @Page Richtlinie, verwenden Sie die folgende Syntax:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Das Design, die hier angegebenen muss erneut, den Ordner "Theme" übereinstimmen, wie bereits erwähnt. Wenn Sie versuchen, ein Design anwenden, die nicht vorhanden ist, erfolgt Build ein Fehler auftritt. Visual Studio auch markieren Sie das Attribut und benachrichtigt Sie, dass keine solche Design vorhanden ist.

## <a name="applying-a-theme-programmatically"></a>Anwenden eines Designs programmgesteuert

Um ein Design programmgesteuert anwenden zu können, müssen Sie angeben der **Design** Eigenschaft für die Seite in der **Seite\_PreInit** Methode.

Um ein Design programmgesteuert anwenden möchten, verwenden Sie die folgende Syntax:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Es ist erforderlich, das Thema in der PreInit-Methode aufgrund der Lebenszyklus der Seite anwenden. Wenn Sie sie anwenden nach diesem Punkt, das Seiten-Design wird bereits angewendet wurden, von der Laufzeit und eine Änderung an diesem Punkt ist zu spät im Lebenszyklus. Wenn Sie ein Design, die nicht vorhanden ist anwenden, eine **HttpException** auftritt. Wenn ein Design programmgesteuert angewendet wird, wird eine Buildwarnung auftreten, wenn es sich bei aller Steuerelemente, die eine SkinID-Eigenschaft, die angegeben haben. Diese Warnung ist vorgesehen, um Sie zu informieren, dass kein Design deklarativ angewendet wird und ignoriert werden kann.

## <a name="exercise-1--applying-a-theme"></a>Übung 1: Anwenden eines Designs

In dieser Übung werden Sie ein ASP.NET-Design auf eine Website anwenden.

> [!IMPORTANT]
> Wenn Sie Microsoft Word eingeben von Informationen in eine Skin-Datei verwenden, stellen Sie sicher, dass Sie keine reguläre Anführungszeichen durch typografische Anführungszeichen ersetzen. Typografische Anführungszeichen verursacht Probleme mit Skin-Dateien.

1. Erstellen Sie eine neue ASP.NET-Website.
2. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen.
3. Wählen Sie die Webkonfigurationsdatei aus der Liste der Dateien, und klicken Sie auf Hinzufügen.
4. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen.
5. Wählen Sie die Skin-Datei, und klicken Sie auf Hinzufügen.
6. Klicken Sie auf "Ja", wenn Sie gefragt, ob eine wie die Datei in der App\_Themes-Ordner.
7. Mit der rechten Maustaste auf den Ordner SkinFile innerhalb der App\_Themes-Ordner im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen.
8. Wählen Sie das Stylesheet aus der Liste der Dateien, und klicken Sie auf Hinzufügen. Sie haben jetzt alle Dateien erforderlich, das neue Design zu implementieren. Allerdings hat Visual Studio Ihrem Designordner SkinFile benannt. Mit der rechten Maustaste auf diesen Ordner, und ändern Sie den Namen in CoolTheme.
9. Öffnen Sie die SkinFile.skin-Datei, und fügen Sie den folgenden Code das Ende der Datei hinzu: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Speichern Sie die SkinFile.skin-Datei.
11. Öffnen Sie die StyleSheet.css.
12. Ersetzen Sie alle Text durch Folgendes: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Speichern Sie die StyleSheet.css-Datei.
14. Öffnen Sie die Seite "default.aspx".
15. Fügen Sie ein TextBox-Steuerelement und ein Button-Steuerelement.
16. Speichern Sie die Seite. Navigieren Sie jetzt die Seite "default.aspx" ein. Es sollte als eine normale Web Form angezeigt werden.
17. Öffnen Sie die Datei "Web.config".
18. Fügen Sie die folgenden direkt unter der öffnenden `<system.web>` Tag: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Speichern Sie die Datei "Web.config". Navigieren Sie jetzt die Seite "default.aspx" ein. Es sollte angezeigt werden mit dem Design angewendet.
20. Wenn sie nicht bereits geöffnet ist, öffnen Sie die Seite "default.aspx" in Visual Studio.
21. Wählen Sie die Schaltfläche.
22. Ändern der **SkinID** GoButton Eigenschaft. Beachten Sie, dass Visual Studio eine Dropdownliste mit gültigen Werten für SkinID für ein Schaltflächen-Steuerelement bereitstellt.
23. Speichern Sie die Seite. Jetzt zeigen Sie die Seite in Ihrem Browser erneut. Die Schaltfläche "go" müsste jetzt angegeben werden. und sollten größere dargestellt werden.

Mithilfe der **SkinID** -Eigenschaft, können Sie ganz einfach verschiedene Designs für verschiedene Instanzen von einem bestimmten Typ von Serversteuerelement konfigurieren.

## <a name="the-stylesheettheme-property"></a>Die StyleSheetTheme-Eigenschaft

Bisher haben wir gesprochen, nur anwenden von Designs, die mithilfe der Design-Eigenschaft. Wenn Sie die Design-Eigenschaft verwenden zu können, werden die Designdatei für Webserversteuerelemente deklarative Einstellungen überschrieben. In Übung 1, z. B. Sie eine SkinID von "GoButton" für das Schaltflächen-Steuerelement angegeben und, die der Text der Schaltfläche auf "go" geändert. Möglicherweise haben Sie bemerkt, dass die Text-Eigenschaft der Schaltfläche im Designer auf "Button" festgelegt wurde, aber das Design, die, außer Kraft gesetzt. Das Design hat immer Vorrang vor alle eigenschafteneinstellungen im Designer.

Wenn Sie möchten, die in der designskindatei mit definierten Eigenschaften außer Kraft setzen können Eigenschaften angegeben im Designer können Sie die **StyleSheetTheme** Eigenschaft anstelle der Design-Eigenschaft. Die StyleSheetTheme-Eigenschaft ist identisch mit der Design-Eigenschaft, außer dass es nicht alle expliziten eigenschafteneinstellungen überschreibt, wie die Design-Eigenschaft.

Um dies in Aktion zu sehen, öffnen Sie die Datei "Web.config" aus dem Projekt in Übung 1, und ändern Sie die `<pages>` Element der folgenden:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Navigieren Sie jetzt die Seite "default.aspx", und sehen Sie, dass das Schaltflächen-Steuerelement auch eine Text-Eigenschaft des "Button" hat. Das ist da die Einstellung der explizite Eigenschaft im Designer für die Text-Eigenschaft festlegen, indem die GoButton SkinID außer Kraft gesetzt wird.

## <a name="overriding-themes"></a>Überschreiben von Designs

Ein globales Design kann überschrieben werden, mithilfe eines Designs mit dem gleichen Namen in der App\_Themes-Ordner der Anwendung. Das Design wird jedoch nicht in einem Szenario mit "true" Außerkraftsetzung angewendet werden. Wenn die Dateien in der App auftritt\_Themes-Ordner, es gilt das Design, verwenden diese Dateien und das globale Design ignoriert.

Die StyleSheetTheme-Eigenschaft ist überschreibbar und kann wie folgt in Code überschrieben werden:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Webparts

ASP.NET-Webparts ist eine integrierte Gruppe von Steuerelementen zum Erstellen von Websites, mit denen Endbenutzer den Inhalt, Darstellung und Verhalten von Webseiten direkt in einem Browser ändern können. Die Änderungen können auf alle Benutzer auf der Website oder für einzelne Benutzer angewendet werden. Wenn Benutzer Seiten und Steuerelemente ändern, können die Einstellungen gespeichert werden, um die persönliche Einstellungen eines Benutzers über zukünftige Browsersitzungen ein Feature namens Personalisierung beizubehalten. Diese Webparts-Funktionen bedeuten, dass es sich bei Entwicklern helfen können, die Endbenutzer eine Webanwendung dynamisch ohne Entwickler oder Administrator eingreifen zu personalisieren.

Verwenden die Webparts-Steuerelementsatz, können Sie als Entwickler Endbenutzer zu aktivieren:

- Personalisieren Sie die Seiteninhalt. Benutzer können neue Webparts-Steuerelementen zu einer Seite hinzufügen, entfernen Sie sie, ausblenden oder können Sie diese wie gewöhnliche Fenster minimieren.
- Personalisieren Sie Seitenlayout. Benutzer können ziehen Sie ein Webparts-Steuerelement mit einer anderen Zone auf einer Seite oder die Darstellung, Eigenschaften und Verhalten zu ändern.
- Exportieren Sie und importieren Sie die Steuerelemente. Benutzer können Konfigurationseinstellungen importieren oder exportieren Webparts-Steuerelement für die Verwendung in andere Webseiten oder Websites, beibehalten werden, die Eigenschaften, Darstellung und sogar die Daten in den Steuerelementen. Dadurch werden Daten Eintrag und die Konfiguration die Anforderungen für Endbenutzer.
- Erstellen von Verbindungen. Benutzer können Verbindungen zwischen Steuerelementen herstellen, z. B. ein Chart-Steuerelement, ein Diagramm für die Daten in einem Börsenticker-Steuerelement anzeigen kann. Benutzer können personalisieren, nicht nur die Verbindung selbst, aber die Darstellung und Details wie das Diagrammsteuerelement die Daten anzeigt.
- Verwalten Sie und Personalisieren Sie Workflowdokumentbibliothek auf Siteebene-Einstellungen. Autorisierte Benutzer können Workflowdokumentbibliothek auf Siteebene-Einstellungen konfigurieren, bestimmen, wer Zugriff auf eine Website oder Seite, rollenbasierten Zugriff auf Steuerelemente festgelegt und so weiter. Beispielsweise kann ein Benutzer in einer Administratorrolle Festlegen einer Webparts-Steuerelements, das von allen Benutzern gemeinsam genutzt werden, und zu verhindern, dass Benutzer, die keine Administratoren personalisieren das freigegebene Steuerelement.

Arbeiten Sie in der Regel mit Webparts in einer von drei Methoden: Erstellen von Seiten, die Webparts-Steuerelemente verwenden, erstellen einzelnen Webparts-Steuerelemente oder vollständige, personalisierbare Webanwendungen, z. B. ein Portal zu erstellen.

## <a name="page-development"></a>Seite-Entwicklung

Seitenentwickler können visuellen Entwurfstools wie z. B. Microsoft Visual Studio 2005 zum Erstellen von Seiten, die Webparts verwenden zu können. Ein Vorteil der Verwendung ein Tool, wie z. B. Visual Studio ist, dass die Webparts-Steuerelementsatz bietet Funktionen für Drag & Drop-Erstellung und Konfiguration von Webparts-Steuerelemente in einem visuellen Designer an. Beispielsweise Sie können mit dem Designer können Sie eine Webparts-Zone oder ein Webparts-Editor-Steuerelement, auf die Entwurfsoberfläche ziehen, und konfigurieren Sie dann auf das Steuerelement direkt in den Designer über die Benutzeroberfläche bereitgestellt, die vom Webparts-Steuerelementsatz. Dies kann beschleunigt die Entwicklung von Webparts-Anwendungen und Verringern der Anzahl der Code, den Sie schreiben müssen.

## <a name="control-development"></a>Entwicklung von ASP.NET-Steuerelementen

Sie können alle bestehenden Steuerelemente von ASP.NET als Webparts-Steuerelements, einschließlich standard Webserver-Steuerelemente, benutzerdefinierte Serversteuerelemente und Benutzersteuerelemente verwenden. Programmgesteuerte Kontrolle der Umgebung zu maximieren können Sie auch benutzerdefinierte Webparts-Steuerelemente erstellen, die von der WebPart-Klasse abgeleitet sind. Für die einzelnen Webparts-Steuerelement-Entwicklung Sie in der Regel entweder ein Benutzersteuerelement erstellen und verwenden Sie diese als ein Webparts-Steuerelement, oder Entwickeln von benutzerdefinierten Webparts-Steuerelements.

Als ein Beispiel für die Entwicklung von benutzerdefinierten Webparts-Steuerelements, können Sie ein Steuerelement, um die Funktionen von anderen ASP.NET-Serversteuerelemente, die möglicherweise nützlich, um das Paket als personalisierbar Webparts-Steuerelements zur Verfügung erstellen: Kalender, Listen, finanzielle Informationen News, Rechner, rich-Text-Steuerelemente zum Aktualisieren von Inhalt, bearbeitbare Raster verbinden, auf Datenbanken, Diagramme, die dynamisch ihre angezeigt, das Aktualisieren oder Wetter und Übertragen von Informationen. Wenn Sie einen visuellen Designer mit dem Steuerelement angeben, kann Klicken Sie dann alle Seitenentwickler, die mithilfe von Visual Studio einfach ziehen Sie das Steuerelement in einer Webparts-Zone und konfigurieren sie zur Entwurfszeit ohne zusätzlichen Code schreiben zu müssen.

Personalisierung ist die Grundlage für die Webparts-Funktion. Sie können die Benutzer – zu ändern oder zu personalisieren Layout, Darstellung und Verhalten des Webparts-Steuerelemente auf einer Seite. Die personalisierten Einstellungen sind langlebig: sie sind nicht nur während der aktuellen Browsersitzung beibehalten (z.B. mit dem Ansichtszustand), aber auch in einem langfristigen Speicher, damit die Einstellungen eines Benutzers für zukünftige Browsersitzungen gespeichert werden. Personalisierung ist für die Webparts-Seiten standardmäßig aktiviert.

Die strukturellen UI-Komponenten basieren auf Personalisierung, und geben Sie die grundlegende Struktur und die Dienste, die von allen Steuerelementen für Webparts erforderlich sind. Eine strukturelle UI-Komponente auf jeder Seite des Webparts erforderlich ist, das WebPartManager-Steuerelement. Obwohl nicht sichtbar ist, hat dieses Steuerelement der kritische Task Koordinieren von alle Webparts-Steuerelemente auf einer Seite an. Es verfolgt z. B. die einzelnen Webparts-Steuerelemente. Er verwaltet die Webparts-Zonen (Regionen, die Webparts-Steuerelemente auf einer Seite enthalten), und die Steuerelemente sind in der Zonen. Außerdem überwacht und steuert die verschiedenen Anzeigemodi, die eine Seite in einem solchen befinden kann, wie durchsuchen, eine Verbindung herstellen, bearbeiten oder Katalogmodus und, ob personalisierungsänderungen für alle Benutzer oder für einzelne Benutzer gelten. Schließlich wird initiiert, und verfolgt Verbindungen und die Kommunikation zwischen Webparts-Steuerelemente.

Die zweite Art der strukturellen UI-Komponente ist die Zone. Zonen fungieren als Layout-Managern auf einer Webparts-Seite. Sie enthalten und Organisieren von Steuerelementen, die die Teilklasse (Teilsteuerelemente) abgeleitet und bieten die Möglichkeit, modulare Seitenlayout in horizontaler oder vertikaler Ausrichtung vorzunehmen. Zonen bieten auch Allgemeines und konsistentes Benutzeroberflächenelemente (z. B. Kopf- und Fußzeilen-Stil, Titel, Rahmenart, Aktionsschaltflächen usw.) für die einzelnen darin enthaltenen Steuerelemente. Diese gemeinsamen Elemente werden als Chrom eines Steuerelements bezeichnet. Mehrere spezialisierte Typen von Zonen werden in den verschiedenen Anzeigemodi und mit verschiedenen Steuerelementen verwendet. Die verschiedenen Typen von Zonen werden wesentliche Webparts-Steuerelemente unten im Abschnitt beschrieben.

Die Webparts-Benutzeroberflächenelementen-Steuerelemente, die leiten Sie von der **Teil** Klasse, bilden die primäre Benutzeroberfläche auf einer Webparts-Seite. Der Webparts-Steuerelementsatz ist flexibel und in den Optionen inklusive gibt Ihnen zum Erstellen von Teilsteuerelementen. Neben dem Erstellen Ihrer eigenen benutzerdefinierten Webparts-Steuerelemente können auch können vorhandene ASP.NET-Serversteuerelemente, Steuerelemente oder benutzerdefinierten Steuerelementen Sie als Webparts-Steuerelemente. Die wesentliche-Steuerelemente, die am häufigsten verwendet werden, für das Erstellen von Webparts-Seiten werden im nächsten Abschnitt beschrieben.

## <a name="web-parts-essential-controls"></a>Webparts-wesentliche-Steuerelemente

Der Webparts-Steuerelementsatz ist umfangreich, aber einige Steuerelemente sind wichtig, da sie für die Webparts funktionieren erforderlich sind oder weil sie die Steuerelemente, die am häufigsten für Webparts-Seiten verwendet werden. Wie Sie mit der Verwendung von Webparts beginnen, und erstellen die grundlegende Webparts-Seiten, es hilfreich ist, mit wesentlichen Webparts-Steuerelemente in der folgenden Tabelle beschriebenen vertraut sein.

| **Webparts-Steuerelement** | **Beschreibung** |
| --- | --- |
| WebPartManager | Verwaltet alle Webparts-Steuerelemente auf einer Seite an. (Und einzigen) **WebPartManager** Steuerelement für alle Webparts-Seite erforderlich ist. |
| CatalogZone | CatalogPart-Steuerelemente enthält. Verwenden Sie diese Zone, um einen Katalog von Webparts-Steuerelemente zu erstellen, in dem Benutzer Steuerelemente zum Hinzufügen zu einer Seite auswählen können. |
| EditorZone | EditorPart-Steuerelemente enthält. Mithilfe dieser Zone können Benutzer das Bearbeiten und Webparts-Steuerelemente auf einer Seite personalisieren. |
| WebPartZone | Enthält sowie allgemeine Layout für die Webparts-Steuerelemente, aus denen die Benutzeroberfläche einer Seite. Verwenden Sie diese Zone aus, wenn Sie Seiten mit Webparts-Steuerelemente erstellen. Seiten können eine oder mehrere Zonen enthalten. |
| ConnectionsZone | WebPartConnection Steuerelemente enthält, und bietet eine Benutzeroberfläche zum Verwalten von Verbindungen. |
| WebPart (GenericWebPart) | Gibt die primäre Benutzeroberfläche wieder. Die meisten Steuerelemente von Webparts-Benutzeroberflächenelementen fallen in diese Kategorie. Für die programmgesteuerte Kontrolle zu maximieren, können Sie benutzerdefinierte Webparts-Steuerelemente, die von der Basisklasse abgeleitet werden erstellen **WebPart** Steuerelement. Sie können auch vorhandene Steuerelemente, Benutzersteuerelemente und benutzerdefinierte Steuerelemente verwenden, wie Webparts-Steuerelemente. Wenn eines dieser Steuerelemente in einer Zone befinden die **WebPartManager** Steuerelement umschließt automatisch mit **GenericWebPart** Steuerelemente zur Laufzeit, sodass Sie sie mit Webparts-Funktionen verwenden können. |
| CatalogPart | Enthält eine Liste der verfügbaren Webparts-Steuerelemente, die Benutzer auf der Seite hinzufügen können. |
| WebPartConnection | Erstellt eine Verbindung zwischen zwei Webparts-Steuerelemente auf einer Seite. Die Verbindung definiert eine der Webparts-Steuerelemente als Anbieter (von Daten), und der andere als Consumer. |
| EditorPart | Dient als Basisklasse für die spezialisierten Editor-Steuerelemente. |
| EditorPart-Steuerelemente (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart und PropertyGridEditorPart) | Benutzern Sie, die verschiedene Aspekte des Webparts-Benutzeroberflächenelementen Steuerelemente auf einer Seite personalisieren |

## <a name="lab-create-a-web-part-page"></a>Testumgebung: Erstellen Sie eine Webpart-Seite

In dieser Übungseinheit erstellen Sie eine Webpart-Seite, die Informationen über ASP.NET-Profile beibehalten werden.

### <a name="creating-a-simple-page-with-web-parts"></a>Erstellen eine einfache Seite mit Webparts

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie eine Seite, die Webparts-Steuerelemente verwendet, um statischen Inhalt anzuzeigen. Der erste Schritt bei der Arbeit mit Webparts ist eine Seite mit zwei erforderlichen strukturellen Elementen zu erstellen. Eine Webparts-Seite benötigt zuerst ein WebPartManager-Steuerelement zum Nachverfolgen und alle Webparts-Steuerelementen zu koordinieren. Zweitens benötigt eine Webparts-Seite eine oder mehrere Zonen, d. h. zusammengesetzte Steuerelemente, Webparts oder andere Steuerelemente enthalten und einen bestimmten Bereich einer Seite einnehmen.

> [!NOTE]
> Sie müssen sich nicht um nichts tun, um die Webparts-Personalisierung aktivieren. Es ist für die Webparts-Steuerelementsatz standardmäßig aktiviert. Wenn Sie zuerst eine Webparts-Seite auf einer Website ausführen, richtet ASP.NET einen Standard-Personalisierungsanbieter um benutzerspezifische personalisierungseinstellungen zu speichern. Weitere Informationen zur Personalisierung finden Sie unter Übersicht über Webserver Webparts-Personalisierung.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Zum Erstellen einer Seite, die Webparts-Steuerelemente enthalten.

1. Schließen Sie die Standardseite, und fügen Sie eine neue Seite mit dem Standort, mit dem Namen WebPartsDemo.aspx.
2. Wechseln Sie zur **Entwurf** anzeigen.
3. Von der **Ansicht** Menü stellen Sie sicher, dass die **nicht visuelle Steuerelemente** und **Details** Optionen ausgewählt sind, damit Sie sehen können, Layouttags und Steuerelemente, die nicht über eine Benutzeroberfläche verfügen.
4. Platzieren Sie die Einfügemarke vor der `<div>` tags auf der Entwurfsoberfläche, und drücken Sie EINGABETASTE, um eine neue Zeile hinzuzufügen. Position der Einfügemarke ein. bevor Sie das neue-Zeile-Zeichen, klicken Sie auf die **Blockformat** Dropdown-Listenfeld-Steuerelement auf das Menü, und wählen die **Überschrift 1** Option. Fügen Sie den Text in der Überschrift des **Webparts-Demonstrationsseite**.
5. Von der **WebParts** Registerkarte der Toolbox, ziehen Sie eine **WebPartManager** -Steuerelement auf der Seite, und positionieren es direkt nach das neue-Zeile-Zeichen und vor der `<div>`Tags.   
  
   Die **WebPartManager** Steuerelement wird keine Ausgabe, nicht gerendert, damit es als ein graues Feld auf der Designeroberfläche angezeigt wird.
6. Die Einfügemarke innerhalb der `<div>` Tags.
7. In der **Layout** Menü klicken Sie auf **Tabelle einfügen**, und erstellen Sie eine neue Tabelle, die eine Zeile und drei Spalten enthält. Klicken Sie auf die **Zelleigenschaften** klicken **oben** aus der **vertikal ausrichten** Dropdown-Liste, klicken Sie auf **OK**, und klicken Sie auf **OK** erneut aus, um die Tabelle zu erstellen.
8. Ziehen Sie ein Steuerelement WebPartZone in der linken Spalte ein. Mit der rechten Maustaste die **WebPartZone** steuern, wählen Sie **Eigenschaften**, und legen Sie die folgenden Eigenschaften:   
  
   ID: SidebarZone   
  
   HeaderText: Randleiste
9. Ziehen Sie eine zweite **WebPartZone** -Steuerelement in der mittleren Spalte aus, und legen Sie die folgenden Eigenschaften:   
  
   ID: MainZone   
  
   HeaderText: Main
10. Speichern Sie die Datei.

Die Seite verfügt jetzt über zwei unterschiedliche Zonen, die Sie separat steuern können. Keine Zone hat jedoch keinen Inhalt, damit die Inhalte der nächste Schritt besteht. In dieser exemplarischen Vorgehensweise verwenden Sie Webparts-Steuerelemente, die nur statische Inhalte anzeigen.

Das Layout des Webparts-Zone wird angegeben, indem eine &lt;Zonetemplate&gt; Element. Innerhalb der Zonenvorlage können Sie jedes ASP.NET-Steuerelement hinzufügen, ob es sich um eine benutzerdefinierte Webparts-Steuerelement, ein Steuerelement oder ein vorhandenes Serversteuerelement ist. Beachten Sie, dass hier Sie das Label-Steuerelement verwenden, und mit dem Sie statischen Text einfach hinzufügen. Wenn Sie ein reguläres Serversteuerelement in Platzieren einer **WebPartZone** Zone ASP.NET behandelt das Steuerelement als Webparts-Steuerelements zur Laufzeit, der Webparts-Funktionen für das Steuerelement ermöglicht.

**Um Inhalte für die main-Zone erstellen**

1. In **Entwurf** anzuzeigen, ziehen Sie eine **Bezeichnung** -Steuerelement aus der **Standard** Registerkarte der Toolbox in den Inhaltsbereich der Zone, deren **ID** Eigenschaft wird auf MainZone festgelegt.
2. Wechseln Sie zur **Quelle** anzeigen. Beachten Sie, dass eine &lt;Zonetemplate&gt; Element wurde hinzugefügt, um zu umschließen der **Bezeichnung** -Steuerelement in MainZone.
3. Fügen Sie ein Attribut mit dem Namen **Titel** auf die &lt;Asp: Label&gt; -Element, und legen Sie dessen Wert auf Inhalt. Entfernen Sie den Text = "Label"-Attribut aus dem &lt;Asp: Label&gt; Element. Zwischen dem öffnenden und schließenden Tags eines der &lt;Asp: Label&gt; -Element, z. B. Hinzufügen von Text **Seite Willkommen** in ein Paar &lt;h2&gt; Element-Tags. Der Code sollte wie folgt aussehen. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Speichern Sie die Datei.

Als Nächstes erstellen Sie ein Benutzersteuerelement, das ebenfalls auf der Seite ein Webparts-Steuerelement hinzugefügt werden kann.

### <a name="to-create-a-user-control"></a>So erstellen Sie ein benutzerdefiniertes Steuerelement

1. Hinzufügen eines neuen Web-Benutzersteuerelements auf Ihre Website als ein Steuerelement für die Suche verwendet werden. Deaktivieren Sie die Option zum **Code in einer separaten Datei platzieren**. Fügen Sie es im gleichen Verzeichnis wie die WebPartsDemo.aspx-Seite hinzu, und nennen Sie sie SearchUserControl.ascx.   
  
    > [!NOTE]
    > Das Benutzersteuerelement in dieser exemplarischen Vorgehensweise implementiert keine tatsächlichen Suchfunktionen. Es wird verwendet, nur um die Webparts-Features zu veranschaulichen.
2. Wechseln Sie zur **Entwurf** anzeigen. Von der **Standard** Registerkarte der Toolbox ein TextBox-Steuerelement auf die Seite ziehen.
3. Platzieren Sie die Einfügemarke hinter das Textfeld ein, die, das Sie gerade hinzugefügt haben, und drücken Sie EINGABETASTE, um eine neue Zeile hinzuzufügen.
4. Ziehen Sie ein Schaltflächen-Steuerelement auf der Seite in der neuen Zeile unterhalb des Textfelds, die Sie gerade hinzugefügt haben.
5. Wechseln Sie zur **Quelle** anzeigen. Stellen Sie sicher, dass der Quellcode für das Benutzersteuerelement wie im folgenden Beispiel aussieht. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Speichern und schließen Sie die Datei.

Jetzt können Sie Webparts-Steuerelemente, die der Randleistenzone hinzufügen. Hinzufügen von zwei Steuerelementen der Zone der Randleiste, eine mit einer Liste von Links und eine andere, die das Benutzersteuerelement ist Sie in der vorherigen Prozedur erstellt. Die Links werden hinzugefügt, als Standard **Bezeichnung** Serversteuerelement, ähnlich wie die Erstellung der statischen Text für die Main-Zone. Allerdings wird zwar die einzelnen Server in enthaltenen Steuerelemente das Benutzersteuerelement direkt in der Zone (z. B. das Label-Steuerelement) enthalten sein könnten, sie sind in diesem Fall nicht. Stattdessen sind sie Teil des Benutzersteuerelements, die Sie im vorherigen Verfahren erstellt haben. Dies zeigt eine gängige Methode zum Verpacken, beliebige Steuerelemente und zusätzliche Funktionen, die Sie in einem Benutzersteuerelement verwenden möchten und klicken Sie dann das Steuerelement in einer Zone als ein Webparts-Steuerelement zu verweisen.

Zur Laufzeit umschließt die Webparts-Steuerelementsatz beide Steuerelemente mit GenericWebPart-Steuerelementen. Wenn eine **GenericWebPart** Steuerelement umschließt ein Webserver-Steuerelement, das Steuerelement für die generische Teil ist das übergeordnete Steuerelement und Sie können das Steuerelement zugreifen, durch die übergeordnete Eigenschaft des Steuerelements ChildControl. Diese Verwendung von generischen Teilsteuerelemente ermöglicht standard Webserver-Steuerelemente aufweisen, die dasselbe grundlegende Verhalten und Attribute wie Webparts-Steuerelemente, die Ableiten der **WebPart** Klasse.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Hinzufügen von Webparts-Steuerelementen der Zone der Randleiste

1. Öffnen Sie die Seite WebPartsDemo.aspx.
2. Wechseln Sie zur **Entwurf** anzeigen.
3. Ziehen Sie die Seite "Benutzer-Steuerelement" Erstellung SearchUserControl.ascx aus **Projektmappen-Explorer** in der Zone, deren **ID** Eigenschaft auf SidebarZone festgelegt ist, und legen Sie sie es.
4. Speichern Sie die Seite WebPartsDemo.aspx.
5. Wechseln Sie zur **Quelle** anzeigen.
6. In der &lt;Asp: Webpartzone&gt; -Element für die SidebarZone, genau über den Verweis auf das Benutzersteuerelement, fügen eine &lt;Asp: Label&gt; -Element mit Links enthalten, wie im folgenden Beispiel gezeigt. Darüber hinaus Hinzufügen einer **Titel** Benutzersteuerelementtag, mit einem Wert von Attribut **Suche**, wie gezeigt. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Speichern und schließen Sie die Datei.

Jetzt können Sie Ihre Seite testen, indem Sie in Ihrem Browser zu ihr navigieren. Die Seite zeigt die beiden Zonen. Der folgende Screenshot zeigt die Seite.

**Webparts-Demoseite mit zwei Zonen**


![Screenshot der Web Teile VS exemplarischen Vorgehensweise 1](profiles-themes-and-web-parts/_static/image3.gif)

**Abbildung 3**: Web-Teile VS exemplarischen Vorgehensweise 1-Screenshot


In der Titelleiste wird jedes Steuerelements ein nach unten weisenden Pfeil, der Zugriff auf einem Verbenmenü der verfügbaren Aktionen bereitstellt, die für ein Steuerelement ausgeführt werden können. Klicken Sie auf das im Verbenmenü für eines der Steuerelemente, und klicken Sie dann die **Minimieren** Verb und beachten Sie, dass das Steuerelement minimiert wird. Klicken Sie auf das im Verbenmenü auf **wiederherstellen**, und die Steuerung zurückgegeben wird, auf die normale Größe.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Der Benutzer, Bearbeitungsseiten und das Layout ändern

Webparts können Benutzer das Layout des Webparts-Steuerelemente zu ändern, indem Sie sie aus einer Zone in eine andere ziehen. Zusätzlich zu ermöglichen, dass Benutzer verschieben **WebPart** Steuerelemente aus einer Zone in eine andere, Sie können den Benutzern erlauben, verschiedene Eigenschaften der Steuerelemente, einschließlich deren Darstellung, Layout und Verhalten. Der Webparts-Steuerelementsatz bietet grundlegende Bearbeitungsfunktionen für **WebPart** Steuerelemente. Obwohl Sie in dieser exemplarischen Vorgehensweise nicht gezeigt werden, können Sie auch erstellen, auf benutzerdefinierten Editor-Steuerelemente, mit denen Benutzer so bearbeiten Sie die Funktionen von **WebPart** Steuerelemente. Wie bei Änderung der Position einer **WebPart** Steuerelement Bearbeiten der Eigenschaften eines Steuerelements ASP.NET-Personalisierung zum Speichern der Änderungen, die Benutzer abhängig.

In diesem Teil der exemplarischen Vorgehensweise fügen Sie die Möglichkeit für Benutzer zum Bearbeiten der grundlegenden Merkmale aller **WebPart** Steuerelement auf der Seite. Sie fügen um diese Features zu aktivieren, ein weiteres benutzerdefiniertes Steuerelement auf der Seite zusammen mit einer &lt;Asp: Editorzone-&gt; Element- und Bearbeitung von zwei Steuerelementen.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Zum Erstellen eines Benutzersteuerelements, das Seitenlayout ändern können

1. In Visual Studio auf die **Datei** , wählen Sie im Menü der **neu** Untermenü, und klicken Sie auf die **Datei** Option.
2. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Web-Benutzersteuerelement**. Der Name der neuen Datei DisplayModeMenu.ascx. Deaktivieren Sie die Option zum **Quellcode in eigener Datei platzieren**.
3. Klicken Sie auf Hinzufügen, um das neue Steuerelement zu erstellen.
4. Wechseln Sie zur **Quelle** anzeigen.
5. Entfernen Sie den vorhandenen Code in der neuen Datei, und fügen Sie in den folgenden Code. Diesen Steuerungscode Benutzer verwendet Funktionen der Webparts-Steuerelemente, mit denen eine Seite, um seine Ansicht oder Anzeigemodus und versetzt Sie so ändern Sie die physikalische Darstellung und Layout der Seite, während Sie bei bestimmten Verwendungsarten angezeigt werden. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Speichern Sie die Datei, indem Sie auf den Speichervorgang Symbol auf der Symbolleiste oder durch Auswahl **speichern** auf die **Datei** Menü.

### <a name="to-enable-users-to-change-the-layout"></a>So aktivieren Sie das Layout ändern

1. Öffnen Sie die Seite WebPartsDemo.aspx, und wechseln Sie zur **Entwurf** anzeigen.
2. Die Einfügemarke in die **Entwurf** zwar unmittelbar hinter der **WebPartManager** -Steuerelement, das Sie zuvor hinzugefügt haben. Fügen Sie eine Absatzmarke nach dem Text, sodass es gibt eine leere Zeile nach der **WebPartManager** Steuerelement. Positionieren Sie die Einfügemarke in der leeren Zeile.
3. Ziehen Sie das Benutzersteuerelement, das Sie gerade erstellt haben (die Datei heißt DisplayModeMenu.ascx) in der WebPartsDemo.aspx Seite, und legen Sie sie in der leeren Zeile.
4. Ziehen Sie eine EditorZone-Steuerelement aus der **WebParts** Abschnitt der Toolbox auf die verbleibenden öffnen Tabellenzelle auf der Seite WebPartsDemo.aspx.
5. Von der **WebParts** Abschnitt der Toolbox ziehen, ein AppearanceEditorPart-Steuerelement und ein LayoutEditorPart-Steuerelement in der **EditorZone** Steuerelement.
6. Wechseln Sie zur **Quelle** anzeigen. Der resultierende Code in der Tabellenzelle sollte den folgenden Code ähneln. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Speichern Sie die WebPartsDemo.aspx-Datei. Sie erstellt haben, ein Benutzersteuerelement, mit dem Sie zum Ändern von Anzeigemodi und Seitenlayout ändern, und Sie haben einen Verweis auf das Steuerelement auf die primäre Webseite.

Sie können jetzt die Möglichkeit, Seiten bearbeiten und ändern Sie das Layout testen.

### <a name="to-test-layout-changes"></a>So testen Sie die Änderungen am layout

1. Laden Sie die Seite in einem Browser.
2. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen **bearbeiten**. Die Zonentitel werden angezeigt.
3. Ziehen Sie die **Meine Links** Steuerelement dessen eigener Titelleiste aus der Zone Randleiste am Ende der Main-Zone. Die Seite sollte wie im folgenden Screenshot aussehen.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Webparts-Demoseite mit Meine Links-Steuerelements verschoben wurde


![Exemplarische Vorgehensweise 2-Screenshot für Webparts im Vergleich](profiles-themes-and-web-parts/_static/image4.gif)

**Abbildung 4**: Web-Teile VS Exemplarische Vorgehensweise 2-Screenshot


1. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen **Durchsuchen**. Die Seite aktualisiert wird, den Zonennamen werden ausgeblendet, und die **Meine Links** steuern, wo Sie positioniert, bleiben.
2. Um zu veranschaulichen, dass die Personalisierung funktioniert, schließen Sie den Browser, und klicken Sie dann laden Sie die Seite erneut zu. Die vorgenommenen Änderungen werden für zukünftige Browsersitzungen gespeichert.
3. Von der **Anzeigemodus** , wählen Sie im Menü **bearbeiten**.   
  
   Jedes Steuerelement auf der Seite wird jetzt mit einem Pfeil in der Titelleiste angezeigt wird, die im Dropdown-Verbenmenü enthält.
4. Klicken Sie auf den Pfeil, um auf das Verbenmenü angezeigt werden sollen die **Meine Links** Steuerelement. Klicken Sie auf die **bearbeiten** Verb.   
  
   Die **EditorZone** -Steuerelement angezeigt wird, Anzeigen der EditorPart Steuerelemente Sie hinzugefügt haben.
5. In der **Darstellung** Abschnitt des Edit-Steuerelements, Änderung der **Titel** verwenden, um meine Favoriten, die **Chromtyp** Dropdown-Liste auswählen **nur Titel**, und klicken Sie dann auf **übernehmen**. Der folgende Screenshot zeigt die Seite im Bearbeitungsmodus befindet.

### <a name="web-parts-demo-page-in-edit-mode"></a>Webparts-Demoseite im Bearbeitungsmodus


![Exemplarische Vorgehensweise 3-Screenshot für Webparts im Vergleich](profiles-themes-and-web-parts/_static/image5.gif)

**Abbildung 5**: Web-Teile VS Exemplarische Vorgehensweise 3-Screenshot


1. Klicken Sie auf die **Anzeigemodus** , und wählen **Durchsuchen** Durchsuchen-Modus zurückgegeben.
2. Das Steuerelement hat jetzt eine aktualisierte Titel und keinen Rahmen, wie im folgenden Screenshot gezeigt.

### <a name="edited-web-parts-demo-page"></a>Bearbeiteten Webparts-Demoseite


![Exemplarische Vorgehensweise 4-Screenshot für Web Teile im Vergleich](profiles-themes-and-web-parts/_static/image6.gif)

**Abbildung 4**: Web-Teile VS Exemplarische Vorgehensweise 4-Screenshot


### <a name="adding-web-parts-at-run-time"></a>Hinzufügen von Webparts zur Laufzeit

Sie können auch Benutzern, Webparts-Steuerelementen zur Laufzeit zur Seite hinzufügen. Zu diesem Zweck konfigurieren Sie die Seite mit einem Katalog von Webparts, der eine Liste der Webparts-Steuerelemente enthält, die Sie für Benutzer verfügbar machen möchten.

**Um Benutzern das Hinzufügen von Webparts zur Laufzeit zu ermöglichen.**

1. Öffnen Sie die Seite WebPartsDemo.aspx, und wechseln Sie zur **Entwurf** anzeigen.
2. Von der **WebParts** Registerkarte der Toolbox ziehen Sie eine CatalogZone-Steuerelement in der rechten Spalte der Tabelle unter der **EditorZone** Steuerelement.   
  
   Beide Steuerelemente können in der gleichen Tabellenzelle sein, da sie nicht gleichzeitig angezeigt werden.
3. Klicken Sie im Bereich "Eigenschaften" die Zeichenfolge zuzuweisen **Webparts hinzufügen** der HeaderText-Eigenschaft von der **CatalogZone** Steuerelement.
4. Von der **WebParts** Abschnitt der Toolbox ziehen Sie ein DeclarativeCatalogPart-Steuerelement in den Inhaltsbereich der **CatalogZone** Steuerelement.
5. Klicken Sie auf den Pfeil in der oberen rechten Ecke des der **DeclarativeCatalogPart** steuern, um die im Menü Tasks verfügbar zu machen, und wählen Sie dann **Vorlagen bearbeiten**.
6. Von der **Standard** Abschnitt der Toolbox ziehen Sie eine **"FileUpload"** Steuerelement und ein **Kalender** steuern, in der **WebPartsTemplate** im Abschnitt der **DeclarativeCatalogPart** Steuerelement.
7. Wechseln Sie zur **Quelle** anzeigen. Überprüfen Sie den Quellcode der &lt;Asp: Catalogzone-&gt; Element. Beachten Sie, dass die **DeclarativeCatalogPart** Steuerelement enthält eine &lt;Webpartstemplate&gt; -Element mit den beiden Serversteuerelemente eingeschlossen, die Sie auf der Seite aus dem Katalog hinzufügen können.
8. Hinzufügen einer **Titel** -Eigenschaft an jedes der Steuerelemente, die Sie dem Katalog hinzugefügt, verwenden für jeden einzelnen Titel im folgenden Codebeispiel wird den Zeichenfolgenwert, der angezeigt. Obwohl der Titel nicht über eine Eigenschaft ist normalerweise lassen sich in diese beiden Steuerelemente zur Entwurfszeit, wenn ein Benutzer fügt diese Steuerelemente auf einer **WebPartZone** Zone aus dem Katalog zur Laufzeit, sie werden jeweils in eingeschlossen ein  **GenericWebPart** Steuerelement. Dies ermöglicht ihnen, als Webparts-Steuerelementen zu fungieren, sodass sie Titel angezeigt werden.   
  
   Der Code für die beiden Steuerelemente, die innerhalb der **DeclarativeCatalogPart** Steuerelement sollte wie folgt aussehen. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Speichern Sie die Seite.

Sie können jetzt den Katalog testen.

### <a name="to-test-the-web-parts-catalog"></a>Zum Testen des Webparts-Katalogs

1. Laden Sie die Seite in einem Browser.
2. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen **Katalog**.   
  
   Der Katalog mit dem Titel **Webparts hinzufügen** wird angezeigt.
3. Ziehen Sie die **Favoriten** steuern, die von der Main-Zone zurück zum Anfang der Sidebar-Zone, und legen Sie sie es.
4. In der **Webparts hinzufügen** Katalog, wählen Sie beide Kontrollkästchen, und wählen Sie dann **Main** aus der Dropdown-Liste, die verfügbarkeitszonen enthält.
5. Klicken Sie auf **hinzufügen** im Katalog. Die Main-Zone werden die Steuerelemente hinzugefügt. Wenn Sie möchten, können Sie mehrere Instanzen von Steuerelementen aus dem Katalog zu Ihrer Seite hinzufügen.   
  
   Der folgende Screenshot zeigt die Seite mit den Dateiupload-Steuerelement und dem Kalender in der Main-Zone. 

![Steuerelemente, die Main-Zone hinzugefügt werden, aus dem Katalog](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen **Durchsuchen**. Der Katalog wird ausgeblendet, und die Seite aktualisiert wird.
7. Schließen Sie den Browser. Laden Sie die Seite erneut. Die Änderungen beibehalten haben.
