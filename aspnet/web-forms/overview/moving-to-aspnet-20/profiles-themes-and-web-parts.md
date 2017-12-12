---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profile, Designs und Webparts | Microsoft Docs
author: microsoft
description: "Es gibt wichtige Änderungen an der Konfiguration und Instrumentation in ASP.NET 2.0. Die neue API zum ASP.NET Konfiguration ermöglicht Änderungen an der Konfiguration erfolgen Pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: c9fe97dbd5fe10cbde25b9daf5ddd35b2d7eaab5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="profiles-themes-and-web-parts"></a>Profile, Designs und Webparts
====================
durch [Microsoft](https://github.com/microsoft)

> Es gibt wichtige Änderungen an der Konfiguration und Instrumentation in ASP.NET 2.0. Die neue ASP.NET Konfigurations-API ermöglicht konfigurationsänderungen programmgesteuert erfolgen. Darüber hinaus zahlreiche neue Konfigurationseinstellungen vorhanden für neue Konfigurationen und -Instrumentation zulassen.


ASP.NET 2.0 stellt eine erhebliche Verbesserung im Bereich von personalisierten Websites dar. Zusätzlich zu den Mitgliedschaft Funktionen Weve bereits abgedeckt verbessern ASP.NET-Profilen, Designs und Webparts erheblich Personalisierung auf Websites.

## <a name="aspnet-profiles"></a>ASP.NET-Profilen

ASP.NET-Profilen ähneln Sitzungen. Der Unterschied besteht darin, dass ein Profil erhalten bleiben, während eine Sitzung verloren geht, wenn der Browser geschlossen wird. Ein weiterer wichtiger Unterschied zwischen Sitzungen und Profile werden Profile stark typisiert sind, bietet Ihnen daher während des Entwicklungsprozesses mithilfe von IntelliSense.

Ein Profil ist in der Konfigurationsdatei für den Computer oder die Datei "Web.config" für die Anwendung definiert. (Sie können nicht in einen Unterordner-Datei "Web.config" ein Profil definieren.) Der folgende Code definiert ein Profil zum Speichern zuerst den Besuchern der Website und dem Nachnamen an.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Der Standarddatentyp für eine Profileigenschaft ist System.String. Im obigen Beispiel wurde kein Datentyp angegeben. Aus diesem Grund sind die FirstName und LastName-Eigenschaften sowohl vom Typ Zeichenfolge. Wie bereits erwähnt,-Profil, das Eigenschaften stark typisiert sind. Der folgende Code Fügt eine neue Eigenschaft für Alter der Daten, die vom Typ Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profile sind im Allgemeinen mit der Formularauthentifizierung mit ASP.NET verwendet. Bei Verwendung in Kombination mit der Formularauthentifizierung verfügt jeder Benutzer über ein eigenes Profil, die ihre Benutzer-ID zugeordnet ist Allerdings ist es auch möglich, ermöglichen die Verwendung von Profilen in einer anonymen Anwendung mittels der &lt;AnonymousIdentification&gt; Element in der Konfigurationsdatei zusammen mit den **AllowAnonymous** Attribut Im folgenden gezeigt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Wenn ein anonymer Benutzer die Website aufruft, erstellt ASP.NET eine Instanz des **ProfileCommon** für den Benutzer. Dieses Profil verwendet eine eindeutige ID gespeichert, die in einem Cookie im Browser zur Identifizierung des Benutzers als eindeutigen Besucher. Auf diese Weise können Sie Profilinformationen für Benutzer speichern anonym beim Durchsuchen.

## <a name="profile-groups"></a>Profilgruppen

Es ist möglich, Eigenschaften von Profilen. Durch Gruppierung von Eigenschaften ist es möglich, mehrere Profile für eine bestimmte Anwendung zu simulieren.

Die folgende Konfiguration konfiguriert eine FirstName und LastName-Eigenschaft für zwei Gruppen. Käufer und Interessenten.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Es kann dann zum Festlegen von Eigenschaften für eine bestimmte Gruppe wie folgt:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Das Speichern von komplexen Objekten

Bisher wurden die Beispiele, die wir abgedeckt haben einfache Datentypen in einem Profil gespeichert. Es ist auch möglich, komplexe Datentypen in einem Profil zu speichern, indem Sie die Methode der Verwendung von Serialisierung angeben der **SerializeAs** -Attribut wie folgt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In diesem Fall ist der Typ PurchaseInvoice. Die PurchaseInvoice-Klasse muss als serialisierbar markiert werden und kann eine beliebige Anzahl von Eigenschaften enthalten. Beispielsweise verfügt PurchaseInvoice eine Eigenschaft mit dem Namen **NumItemsPurchased**, ersehen Sie diese Eigenschaft im Code wie folgt:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil-Vererbung

Es ist möglich, ein Profil für die Verwendung in mehreren Anwendungen zu erstellen. Erstellen Sie eine Profilklasse, die von ProfileBase abgeleitet wird, können Sie ein Profil in mehrere Anwendungen wiederverwenden, indem die **erbt** -Attribut an, wie unten dargestellt:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In diesem Fall wird die Klasse **PurchasingProfile** sieht wie folgt:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Profilanbieter

ASP.NET-Profilen verwenden das Anbietermodell. Der Standardanbieter speichert die Informationen in einer SQL Server Express-Datenbank in der App\_Datenordner der Webanwendung mit dem Anbieter SqlProfileProvider. Wenn die Datenbank nicht vorhanden ist, wird dieses durch die ASP.NET automatisch erstellt, wenn das Profil versucht wird, um Informationen zu speichern.

In einigen Fällen können jedoch Sie Ihren eigenen Anbieter entwickeln möchten. Das Profilfeature ASP.NET können Sie problemlos andere Anbieter verwenden.

Erstellen Sie einen benutzerdefiniertes Profil-Anbieter bei:

- Wie in einer FoxPro- oder einer Oracle-Datenbank, die nicht der Profil-Anbieter, die in .NET Framework unterstützt wird, müssen Sie Profilinformationen in einer Datenquelle zu speichern.
- Sie müssen zum Verwalten von Profilinformationen mit einem Datenbankschema, das sich aus dem Datenbankschema, das von der .NET Framework enthaltenen Anbieter verwendet. Ein gängiges Beispiel ist, dass Sie Profilinformationen mit Benutzerdaten in einer vorhandenen SQL Server-Datenbank zu integrieren möchten.

### <a name="required-classes"></a>Erforderlichen Klassen

Um einen Anbieter zu implementieren, erstellen Sie eine Klasse, die abstrakte System.Web.Profile.ProfileProvider-Klasse erbt. Die **ProfileProvider** abstrakte Klasse erbt wiederum die System.Configuration.SettingsProvider abstrakte Klasse, die die abstrakte System.Configuration.Provider.ProviderBase-Klasse erbt. Aufgrund dieser Vererbungskette zusätzlich zu den erforderlichen Mitgliedern von der **ProfileProvider** -Klasse, müssen Sie die erforderlichen Mitgliedern der implementieren die **SettingsProvider** und  **ProviderBase** Klassen.

Die folgenden Tabellen beschreiben die Eigenschaften und Methoden, die Sie implementieren müssen, aus der **ProviderBase**, **SettingsProvider**, und **ProfileProvider** abstrakte Klassen.

### <a name="providerbase-members"></a>ProviderBase-Member

| **Datenmember** | **Beschreibung** |
| --- | --- |
| Initialize-Methode | Verwendet als Eingabe, die den Namen der Anbieterinstanz und eine NameValueCollection von Konfigurationseinstellungen. Dient zum Festlegen von Optionen und Eigenschaftswerte für die Anbieterinstanz, einschließlich implementierungsspezifische Werte und Optionen, die in der Konfiguration des Computers oder der Datei "Web.config" angegeben. |

### <a name="settingsprovider-members"></a>SettingsProvider Elemente

| **Datenmember** | **Beschreibung** |
| --- | --- |
| Parameter "ApplicationName"-Eigenschaft | Der Anwendungsname, die mit jedem Profil gespeichert wird. Der Profilanbieter verwendet den Namen der Anwendung zum Speichern von Informationen zum Profil separat für jede Anwendung. Dadurch können mehrere ASP.NET-Anwendungen auf die gleiche Datenquelle ohne einen Konflikt verwenden, wenn ein Benutzername in verschiedenen Anwendungen erstellt wird. Alternativ können mehrere ASP.NET-Anwendungen eine Profildatenquelle freigeben, durch Angeben des Anwendungsnamens der gleichen. |
| GetPropertyValues-Methode | Akzeptiert als Eingabe einen SettingsContext und ein SettingsPropertyCollection-Objekt. Die **SettingsContext** enthält Informationen über den Benutzer. Die Informationen können als Primärschlüssel mit dem Eigenschafteninformationen für Profil für den Benutzer abgerufen. Verwenden der **SettingsContext** Objekt auf, um den Benutzernamen und gibt an, ob der Benutzer authentifiziert oder anonym ist. Die **SettingsPropertyCollection** enthält eine Auflistung von SettingsProperty-Objekten. Jede **SettingsProperty** Objekt enthält den Namen und Typ der Eigenschaft als auch zusätzliche Informationen, z. B. der Standardwert für die Eigenschaft und gibt an, ob die Eigenschaft schreibgeschützt ist. Die **GetPropertyValues** Methode füllt eine SettingsPropertyValueCollection mit SettingsPropertyValue-Objekte, die auf der Grundlage der **SettingsProperty** Objekte als Eingabe bereitgestellt. Die Werte aus der Datenquelle für den angegebenen Benutzer werden an den PropertyValue Eigenschaften zugewiesen, für jede **SettingsPropertyValue** -Objekt und die gesamte Auflistung wird zurückgegeben. Aufrufen der Methode wird auch den LastActivityDate Wert für das angegebene Benutzerprofil auf das aktuelle Datum und die Uhrzeit aktualisiert. |
| SetPropertyValues-Methode | Verwendet als Eingabe eine **SettingsContext** und ein **SettingsPropertyValueCollection** Objekt. Die **SettingsContext** enthält Informationen über den Benutzer. Die Informationen können als Primärschlüssel mit dem Eigenschafteninformationen für Profil für den Benutzer abgerufen. Verwenden der **SettingsContext** Objekt auf, um den Benutzernamen und gibt an, ob der Benutzer authentifiziert oder anonym ist. Die **SettingsPropertyValueCollection** enthält eine Auflistung von **SettingsPropertyValue** Objekte. Jede **SettingsPropertyValue** Objekt enthält die Namen, Typ und Wert der Eigenschaft als auch zusätzliche Informationen, z. B. der Standardwert für die Eigenschaft und gibt an, ob die Eigenschaft schreibgeschützt ist. Die **SetPropertyValues** Methode aktualisiert die Profileigenschaftswerte in der Datenquelle für den angegebenen Benutzer. Aufrufen der Methode auch bei Updates die **LastActivityDate** und LastUpdatedDate Werte für das angegebene Benutzerprofil auf das aktuelle Datum und die Uhrzeit. |

### <a name="profileprovider-members"></a>ProfileProvider Mitglieder

| **Datenmember** | **Beschreibung** |
| --- | --- |
| DeleteProfiles-Methode | Akzeptiert als Eingabe ein Zeichenfolgenarray des Benutzers aus der Datenquelle alle Profil Informationen und die Eigenschaftswerte für den angegebenen Namen entspricht, den Namen der Anwendung, in denen die **Parameter "ApplicationName"** Eigenschaftswert. Wenn Ihre Datenquelle Transaktionen unterstützt, wird empfohlen, dass Sie alle Löschvorgänge in einer Transaktion ausgeführt und ein der Transaktion Rollback und löst eine Ausnahme aus, wenn eine Delete-Vorgang ein Fehler auftritt. |
| DeleteProfiles-Methode | Akzeptiert als Eingabe eine Auflistung von ProfileInfo Objekte und löscht aus der Datenquelle alle Profil Informationen und die Eigenschaftswerte für jedes Profil entspricht, den Namen der Anwendung, in denen die **Parameter "ApplicationName"** Eigenschaftswert. Wenn Ihre Datenquelle Transaktionen unterstützt, wird empfohlen, dass Sie alle Löschvorgänge in eine Transaktion aufnehmen und Rollback der Transaktion und löst eine Ausnahme aus, wenn eine Delete-Vorgang ein Fehler auftritt. |
| DeleteInactiveProfiles-Methode | Übernimmt als Eingabewert ein ProfileAuthenticationOption und DateTime-Objekt und löschungen aus der Quelle alle Profilinformationen und Eigenschaftswerte, in denen das Datum der letzten Aktivität kleiner oder gleich dem angegebenen Datum und Uhrzeit ist und den Namen der Anwendung entspricht der **Parameter "ApplicationName"** Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile nur authentifizierte Profile oder alle Profile gelöscht werden soll. Wenn Ihre Datenquelle Transaktionen unterstützt, wird empfohlen, dass Sie alle Löschvorgänge in eine Transaktion aufnehmen und Rollback der Transaktion und löst eine Ausnahme aus, wenn eine Delete-Vorgang ein Fehler auftritt. |
| GetAllProfiles-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert, der eine ganze Zahl, die den Seitenindex angibt, der eine ganze Zahl, der angibt, die Seitengröße und einem Verweis auf eine ganze Zahl, die auf die Gesamtzahl der Profile festgelegt werden. Gibt eine ProfileInfoCollection, die enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, den Namen der Anwendung, in denen entspricht die **Parameter "ApplicationName"** Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Die Ergebnisse der **GetAllProfiles** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Seitengrößenwert gibt die maximale Anzahl von **ProfileInfo** Objekte zurückzugebenden in der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 die erste Seite bezeichnet. Der Parameter für die Gesamtzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), um die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Seitenindexwert 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben das sechste bis zehnte Profile enthält. Der Datensätze insgesamt Wert wird auf 13 festgelegt, wenn die Methode zurückkehrt. |
| GetAllInactiveProfiles-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert, eine **"DateTime"** -Objekt, eine ganze Zahl, die den Seitenindex angibt, eine ganze Zahl, der angibt, die Seitengröße und einem Verweis auf eine ganze Zahl, die festgelegt werden um die Gesamtzahl von Profilen. Gibt eine **ProfileInfoCollection** enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, in dem das Datum der letzten Aktivität kleiner oder gleich dem angegebenen ist **"DateTime"**  , in dem Anwendungsnamen übereinstimmt, und die **Parameter "ApplicationName"** Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Die Ergebnisse der **GetAllInactiveProfiles** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Seitengrößenwert gibt die maximale Anzahl von **ProfileInfo** Objekte zurückzugebenden in der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 die erste Seite bezeichnet. Der Parameter für die Gesamtzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), um die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Seitenindexwert 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben das sechste bis zehnte Profile enthält. Der Datensätze insgesamt Wert wird auf 13 festgelegt, wenn die Methode zurückkehrt. |
| FindProfilesByUserName-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert eine Zeichenfolge, enthält einen Benutzernamen, eine ganze Zahl, die den Seitenindex angibt, eine ganze Zahl, der angibt, die Seitengröße und einem Verweis auf eine ganze Zahl, die auf die Gesamtzahl der festgelegt werden Profile. Gibt eine **ProfileInfoCollection** enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle, in dem Benutzernamen den angegebenen Benutzernamen übereinstimmt, und, in dem Namen der Anwendung entspricht der **Parameter "ApplicationName"** Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Wenn Ihre Datenquelle zusätzliche Suchfunktionen, z. B. Platzhalterzeichen unterstützt, können Sie eine erweiterte Suchfunktionen für Benutzernamen bereitstellen. Die Ergebnisse der **FindProfilesByUserName** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Seitengrößenwert gibt die maximale Anzahl von **ProfileInfo** Objekte zurückzugebenden in der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 die erste Seite bezeichnet. Der Parameter für die Gesamtzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), um die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Seitenindexwert 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben das sechste bis zehnte Profile enthält. Der Datensätze insgesamt Wert wird auf 13 festgelegt, wenn die Methode zurückkehrt. |
| FindInactiveProfilesByUserName-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert, der eine Zeichenfolge, enthält einen Benutzernamen ein, eine **"DateTime"** -Objekt, eine ganze Zahl, die den Seitenindex angibt, eine ganze Zahl, die die Seitengröße angibt und ein einen Verweis auf eine ganze Zahl, die auf die Gesamtzahl der Profile festgelegt werden. Gibt eine **ProfileInfoCollection** enthält **ProfileInfo** Objekte für alle Profile in der Datenquelle dem Benutzernamen übereinstimmt, in denen der angegebene Benutzername, wobei das Datum der letzten Aktivität ist kleiner als oder gleich einem angegebenen **"DateTime"**, und den Namen der Anwendung, in denen entspricht der **Parameter "ApplicationName"** Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile nur authentifizierte Profile oder alle Profile zurückgegeben werden sollen. Wenn Ihre Datenquelle zusätzliche Suchfunktionen, z. B. Platzhalterzeichen unterstützt, können Sie eine erweiterte Suchfunktionen für Benutzernamen bereitstellen. Die Ergebnisse der **FindInactiveProfilesByUserName** Methode durch den Seitenindex und Seitengrößenwert eingeschränkt werden. Der Seitengrößenwert gibt die maximale Anzahl von **ProfileInfo** Objekte zurückzugebenden in der **ProfileInfoCollection**. Der Indexwert für die Seite gibt an, welche Seite der Ergebnisse zurückgegeben, wobei 1 die erste Seite bezeichnet. Der Parameter für die Gesamtzahl der Datensätze ist ein Out-Parameter (können **ByRef** in Visual Basic), um die Gesamtzahl der Profile festgelegt ist. Wenn der Datenspeicher enthält 13 Profile für die Anwendung und der Seitenindexwert 2 mit einer Seitengröße von 5, z. B. die **ProfileInfoCollection** zurückgegeben das sechste bis zehnte Profile enthält. Der Datensätze insgesamt Wert wird auf 13 festgelegt, wenn die Methode zurückkehrt. |
| GetNumberOfInActiveProfiles-Methode | Verwendet als Eingabe eine **ProfileAuthenticationOption** Wert und eine **"DateTime"** -Objekt und gibt die Anzahl der Profile für alle in der Datenquelle, in dem das Datum der letzten Aktivität kleiner oder gleich der angegebenen ist,zurück. **"DateTime"** , in dem Anwendungsnamen übereinstimmt, und die **Parameter "ApplicationName"** Eigenschaftswert. Die **ProfileAuthenticationOption** Parameter gibt an, ob nur anonyme Profile nur authentifizierte Profile oder alle Profile, die gezählt werden. |

### <a name="applicationname"></a>ApplicationName

Da Profilanbieter Profilinformationen separat für jede Anwendung speichern, müssen Sie sicherstellen, dass Ihre Datenschema den Namen der Anwendung enthält und für die Abfragen und Updates auch den Namen der Anwendung enthält. Beispielsweise der folgende Befehl dient zum Abrufen eines Eigenschaftswerts aus einer Datenbank, basierend auf den Benutzernamen und gibt an, ob das Profil anonym ist, und stellt sicher, dass die **Parameter "ApplicationName"** Wert ist in der Abfrage enthalten.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET-Designs

## <a name="what-are-aspnet-20-themes"></a>Was ASP.NET 2.0-Designs sind?

Einer der wichtigsten Aspekte einer Webanwendung ist ein einheitliches Erscheinungsbild am gesamten Standort. ASP.NET 1.x-Entwickler verwenden Cascading Style Sheets (CSS) in der Regel um ein konsistentes Aussehen und Verhalten zu implementieren. ASP.NET 2.0 Designs wird erheblich nach CSS verbessern, da sie den ASP.NET Developer Zugriff auf die Darstellung der ASP.NET-Serversteuerelemente sowie HTML-Elemente definieren zu gewähren. ASP.NET-Designs können auf einzelne Steuerelemente, die eine bestimmte Webseite oder eine gesamte Webanwendung angewendet werden. Designs verwenden eine Kombination von CSS-Dateien, eine optionale Designdatei und einem optionalen Bilderverzeichnis aus, wenn Bilder benötigt werden. Die Designdatei steuert die visuelle Darstellung des ASP.NET-Serversteuerelemente.

## <a name="where-are-themes-stored"></a>Wo befinden sich Designs gespeichert?

Der Speicherort Designs sind unterscheidet sich basierend auf ihren Gültigkeitsbereich. Designs, die für jede Anwendung angewendet werden können, werden im folgenden Ordner gespeichert:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Ein Design, das auf eine bestimmte Anwendung spezifisch ist, wird in einer App gespeichert\_Designs\&Lt; Design\_Namen&gt; Verzeichnis im Stammverzeichnis der Website.

> [!NOTE]
> Eine Designdatei sollten nur Eigenschaften ändern, die Darstellung zu beeinflussen.

Ein globales Design ist ein Design, das auf eine beliebige Anwendung oder Website, die auf dem Webserver ausgeführten angewendet werden kann. Diese Themen werden standardmäßig im Verzeichnis ASP.NETClientfiles\Themes gespeichert, die innerhalb des Verzeichnisses v2.x.xxxxx befindet. Alternativ können Sie das Designdateien in das Aspnet verschieben\_Clientsystem/\_Web / /Themes/ [Version] [Design\_Name] Ordner im Stammverzeichnis der Website.

Anwendungsspezifische Designs können nur auf die Anwendung angewendet werden, in denen die Dateien befinden. Diese Dateien werden in der App gespeicherten\_Designs /&lt;Design\_Namen&gt; Verzeichnis im Stammverzeichnis der Website.

## <a name="the-components-of-a-theme"></a>Die Komponenten eines Designs

Ein Design besteht aus einer oder mehreren CSS-Dateien, eine optionale Designdatei und eine optionale Ordner "Abbilder". Die CSS-Dateien können ein beliebiger Name sein soll (d. h. "default.CSS" oder theme.css usw.) und im Stammverzeichnis des Ordners Designs werden muss. Die CSS-Dateien werden verwendet, um gewöhnliche CSS-Klassen und Attribute für bestimmte Selektoren zu definieren. Eine CSS-Klassen einem Seitenelement Zuweisen der **CSSClass** Eigenschaft wird verwendet.

Die Designdatei ist eine XML-Datei, die Definitionen für ASP.NET-Serversteuerelemente enthält. Die unten aufgeführte Code ist eine Beispieldatei für das Design.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Abbildung 1** unten zeigt eine kleine ASP.NET-Seite durchsucht, ohne ein Design angewendet. **Abbildung 2** zeigt die gleiche Datei mit einem Design angewendet. Die Hintergrundfarbe und Textfarbe werden über eine CSS-Datei konfiguriert. Die Darstellung der Schaltfläche und ein Textfeld werden mithilfe der oben aufgeführten Skindatei konfiguriert.


![Kein Design](profiles-themes-and-web-parts/_static/image1.gif)

**Abbildung 1**: kein Design


![Design](profiles-themes-and-web-parts/_static/image2.gif)

**Abbildung 2**: Design


Die oben aufgeführten Designdatei definiert ein Standarddesign für alle TextBox-Steuerelemente und Schaltflächen-Steuerelemente. Das bedeutet, dass auf diese Darstellung jeder TextBox-Steuerelement und ein Button-Steuerelement auf einer Seite eingefügt dauert. Außerdem können Sie ein Design aus, die auf bestimmte Instanzen dieser Steuerelemente mit angewendet werden, kann die **SkinID** Eigenschaft des Steuerelements.

Der folgende Code definiert ein Design für ein Schaltflächen-Steuerelement. Nur die Schaltflächen-Steuerelemente mit einem **SkinID** Eigenschaft **GoButton** gelangen auf die Darstellung des Designs.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Sie können nur ein Standarddesign pro Server Steuerelementtyp haben. Wenn Sie zusätzliche Designs benötigen, sollten Sie die Eigenschaft SkinID verwenden.

## <a name="applying-themes-to-pages"></a>Anwenden von Designs auf Seiten

Ein Design kann mithilfe einer der folgenden Methoden angewendet werden:

- In der &lt;Seiten&gt; -Element der Datei "Web.config"
- In der @Page Richtlinie einer Seite
- Programmgesteuert

## <a name="applying-a-theme-in-the-configuration-file"></a>Ein Design in der Konfigurationsdatei anwenden

Um ein Design in der Konfigurationsdatei Anwendungen anzuwenden, verwenden Sie die folgende Syntax:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Der Name des Designs, das hier angegebene muss den Namen des Ordners Designs übereinstimmen. Dieser Ordner kann entweder in einer beliebigen anderen weiter oben in diesem Kurs erwähnten Speicherorten vorhanden sein. Wenn Sie versuchen, ein Design anwenden, die nicht vorhanden ist, wird ein Fehler bei der Konfiguration auftreten.

## <a name="applying-a-theme-in-the-page-directive"></a>Ein Design in der Seitendirektive anwenden

Sie können auch ein Design in der @ Page-Direktive anwenden. Diese Methode können Sie ein Design für eine bestimmte Seite verwenden.

Ein Design in der @Page Richtlinie verwenden Sie die folgende Syntax:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Das hier angegebene Design muss erneut, den Designordner übereinstimmen, wie bereits erwähnt. Wenn Sie versuchen, ein Design anwenden, die nicht vorhanden ist, wird ein Buildfehler auftreten. Visual Studio ebenfalls markieren Sie das Attribut und benachrichtigt Sie, dass keine solche Design vorhanden ist.

## <a name="applying-a-theme-programmatically"></a>Ein Design anwenden programmgesteuert

Um ein Design programmgesteuert zu übernehmen, müssen Sie angeben der **Design** Eigenschaft für die Seite in der **Seite\_PreInit** Methode.

Um programmgesteuert ein Design anzuwenden, verwenden Sie die folgende Syntax:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Es ist notwendig, das Thema in der PreInit Methode aufgrund der Lebenszyklus der Seite anwenden. Wenn anwenden nach diesem Punkt, dem Design "Seiten" wird bereits angewendet wurden, von der Laufzeit und eine Änderung an diesem Punkt ist zu spät im Lebenszyklus. Wenn Sie ein Design, die nicht vorhanden ist anwenden, ein **HttpException** auftritt. Wenn ein Design programmgesteuert angewendet wird, eine Buildwarnung geschieht, wenn Serversteuerelemente eine SkinID-Eigenschaft angegeben haben. Diese Warnung ist vorgesehen, um Sie zu informieren, dass kein Design deklarativ angewendet wird und ignoriert werden kann.

## <a name="exercise-1--applying-a-theme"></a>Übung 1: Anwenden eines Designs

In dieser Übung wenden Sie ein ASP.NET-Design mit einer Website an.

> [!IMPORTANT]
> Wenn Sie Microsoft Word zum Eingeben von Informationen in einer Designdatei verwenden, stellen Sie sicher, dass Sie nicht normale Anführungszeichen durch typografische ersetzen. Typografische verursacht Probleme mit Skin-Dateien.

1. Erstellen Sie eine neue ASP.NET-Website.
2. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen.
3. Wählen Sie Webkonfigurationsdatei aus der Liste der Dateien, und klicken Sie auf Hinzufügen.
4. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen.
5. Wählen Sie Skin-Datei, und klicken Sie auf Hinzufügen.
6. Klicken Sie auf "Ja", wenn Sie gefragt werden, wenn Youd wie die Datei in die App\_Ordner Designs.
7. Mit der rechten Maustaste auf den Ordner SkinFile innerhalb der App\_Designordner im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen.
8. Wählen Sie Stylesheet aus der Liste der Dateien, und klicken Sie auf Hinzufügen. Sie haben jetzt alle Dateien erforderlich, um das neue Design zu implementieren. Visual Studio hat jedoch Designordner SkinFile benannt. Mit der rechten Maustaste auf den Ordner, und ändern Sie den Namen in CoolTheme.
9. Öffnen Sie die Datei SkinFile.skin, und fügen Sie den folgenden Code am Ende der Datei: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Speichern Sie die SkinFile.skin-Datei.
11. Öffnen Sie die StyleSheet.css.
12. Ersetzen Sie alle Text durch Folgendes: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Speichern Sie die StyleSheet.css-Datei.
14. Öffnen Sie die Seite "default.aspx".
15. Ein TextBox-Steuerelement und ein Button-Steuerelement hinzufügen.
16. Speichern Sie die Seite. Durchsuchen Sie jetzt die Seite "default.aspx" ein. Sie sollten als eine normale Web Form angezeigt werden.
17. Öffnen Sie die Datei "Web.config".
18. Fügen Sie die folgenden direkt unterhalb der öffnenden `<system.web>` Tag: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Speichern Sie die Datei "Web.config". Durchsuchen Sie jetzt die Seite "default.aspx" ein. Sie sollten mit dem Design angewendet angezeigt werden.
20. Wenn es nicht bereits geöffnet ist, öffnen Sie die Seite "default.aspx" in Visual Studio.
21. Klicken Sie auf die Schaltfläche.
22. Ändern der **SkinID** GoButton Eigenschaft. Beachten Sie, dass Visual Studio eine Dropdownliste mit gültigen SkinID Werte für ein Schaltflächen-Steuerelement bereitstellt.
23. Speichern Sie die Seite. Jetzt eine Vorschau der Seite in Ihrem Browser erneut ein. Die Schaltfläche sollte "go" müsste jetzt angegeben werden. auch größere im Aussehen.

Mithilfe der **SkinID** -Eigenschaft, können Sie leicht unterschiedliche Designs für verschiedene Instanzen eines bestimmten Typs des Serversteuerelements konfigurieren.

## <a name="the-stylesheettheme-property"></a>Die StyleSheetTheme-Eigenschaft

Bisher haben wir dargelegt nur Designs, die mithilfe der Design-Eigenschaft anwenden. Wenn Sie das Design-Eigenschaft verwenden, wird die Designdatei deklarative für Webserversteuerelemente, außer Kraft gesetzt. Beispielsweise in Übung 1 angegebene eine SkinID von "GoButton" für das Schaltflächen-Steuerelement, und, die den Schaltflächentext "go" geändert. Möglicherweise haben Sie bemerkt, dass der Text-Eigenschaft der Schaltfläche im Designer auf "Button" festgelegt wurde, aber das Design überschrieben, die hat. Das Design wird immer Eigenschaft im Designer, außer Kraft gesetzt.

Wenn Sie in das Design-Designdatei mit definierten Eigenschaften überschreiben möchten Eigenschaften angegeben im Designer können Sie die **StyleSheetTheme** Eigenschaft anstelle der Design-Eigenschaft. Die StyleSheetTheme-Eigenschaft ist identisch mit der Design-Eigenschaft, mit dem Unterschied, dass sie nicht alle expliziten eigenschafteneinstellungen wie die Eigenschaft Design überschreibt.

Um dies in Aktion zu sehen, öffnen Sie die Datei "Web.config" aus dem Projekt in Übung 1, und ändern Sie die &lt;Seiten&gt; Element der folgenden:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Wechseln Sie jetzt die Seite "default.aspx", und sehen Sie, dass das Schaltflächen-Steuerelement erneut eine Text-Eigenschaft von "Button" aufweist. Das ist, da die expliziten eigenschafteneinstellung im Designer die Text-Eigenschaft festlegen, indem die GoButton SkinID außer Kraft gesetzt wird.

## <a name="overriding-themes"></a>Überschreiben von Designs

Ein globales Design kann überschrieben werden, mithilfe eines Designs mit dem gleichen Namen in der App\_Designs-Ordner der Anwendung. Das Design wird jedoch nicht in einem Szenario mit "true" Außerkraftsetzung angewendet. Wenn Designdateien in der App auftritt\_Designs-Ordner, es wird das Design mit diesen Dateien anwenden und ignoriert das globale Design.

Die StyleSheetTheme-Eigenschaft überschrieben werden kann und wie folgt in Code überschrieben werden kann:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Webparts

ASP.NET Web Parts ist einen integrierten Satz von Steuerelementen zum Erstellen von Websites, mit denen Benutzer Inhalt, Darstellung und Verhalten von Webseiten direkt in einem Browser ändern können. Die Änderungen können für alle Benutzer auf der Website oder auf einzelne Benutzer angewendet werden. Wenn der Benutzer Seiten und Steuerelemente ändern, können die Einstellungen gespeichert werden, um persönliche Einstellungen eines Benutzers in zukünftigen Browsersitzungen, ein Feature namens Personalisierung beizubehalten. Diese Webparts-Funktionen bedeuten, dass Endbenutzer eine Webanwendung dynamisch, ohne Zutun des Entwickler oder Administrator personalisieren Entwickler ermöglichen, können.

Verwenden die Webparts-Steuerelementsatz, können Sie als Entwickler Endbenutzer zu aktivieren:

- Personalisieren Sie die Seiteninhalt. Benutzer können neue Webparts-Steuerelemente zu einer Seite hinzufügen, entfernen, ausblenden oder wie gewöhnliche Fenster minimieren.
- Personalisieren Sie Seitenlayout. Benutzer können Webparts-Steuerelement in eine andere Zone ziehen, auf einer Seite oder seine Darstellung, Eigenschaften und Verhalten zu ändern.
- Exportieren und Importieren von Steuerelementen. Benutzer können importieren oder Exportieren von steuerelementeinstellungen Webparts für die Verwendung in anderen Seiten oder Websites, die Eigenschaften, Darstellung und auch die Daten in den Steuerelementen werden beibehalten. Dies reduziert die Daten Einstiegs- und Configuration-Anforderungen für Endbenutzer.
- Erstellen von Verbindungen. Benutzer können Verbindungen zwischen Steuerelementen herstellen, sodass z. B. ein Chart-Steuerelement ein Diagramm für die Daten in einem Börsenticker-Steuerelement anzeigen kann. Benutzer konnte personalisieren, nicht nur die Verbindung selbst, aber die Darstellung und Details wie das Diagrammsteuerelement die Daten anzeigt.
- Verwalten und Siteebene Einstellungen anpassen. Autorisierte Benutzer können Websiteebene Einstellungen konfigurieren, bestimmen, wer Zugriff auf eine Website oder Seite, mit der rollenbasierten Zugriff auf Steuerelemente festgelegt und so weiter. Ein Benutzer in einer Administratorrolle könnten z. B. Festlegen einer Webparts-Steuerelements, das von allen Benutzern gemeinsam genutzt werden und verhindert, dass Benutzer, die das freigegebene Steuerelement personalisieren Administratoren sind.

Arbeiten Sie in der Regel mit Webparts in einer von drei Methoden: Erstellen von Seiten, die Webparts-Steuerelemente verwenden, das Erstellen von einzelnen Webparts-Steuerelemente oder abgeschlossen wurde, personalisierbares Web Applications, z. B. ein Portal erstellen.

## <a name="page-development"></a>Seite "-Entwicklung

Seite "können auf Entwicklerseite visuellen Entwurfstools, z. B. Microsoft Visual Studio 2005 zum Erstellen von Seiten, die Webparts verwenden. Ein Vorteil im mithilfe eines Tools wie Visual Studio eignet sich, dass die Webparts-Steuerelementsatz stellt Funktionen zum Drag-and-Drop-Erstellung und Konfiguration von Webparts-Steuerelemente in einem visuellen Designer bereit. Angenommen, Sie mithilfe des Designers eine Webparts-Zone oder ein Webparts-Editor-Steuerelement auf die Entwurfsoberfläche ziehen und konfigurieren Sie dann auf das Steuerelement rechts können Steuerelementsatz im Designer über die Benutzeroberfläche von Webparts bereitgestellt. Dies kann beschleunigen der Entwicklung von Webparts-Anwendungen und verringern Sie die Menge des Codes, den Sie schreiben müssen.

## <a name="control-development"></a>-Steuerelemententwicklung

Sie können alle vorhandenen ASP.NET-Steuerelements als Webparts-Steuerelement, einschließlich der standard-Webserversteuerelemente, benutzerdefinierte Serversteuerelemente und Benutzersteuerelemente verwenden. Für die maximale programmgesteuerte Kontrolle der Umgebung können Sie auch benutzerdefinierte Webparts-Steuerelemente erstellen, die von dem WebPart-Klasse abgeleitet werden. Für die einzelnen Webparts-Steuerelemententwicklung Sie in der Regel entweder ein Benutzersteuerelement erstellen und verwenden es als Webparts-Steuerelement oder Entwickeln eines benutzerdefiniertes Webparts-Steuerelements.

Als Beispiel für ein benutzerdefiniertes Webparts-Steuerelement entwickeln, konnte Sie ein Steuerelement zum Bereitstellen eines anderen ASP.NET-Serversteuerelemente, die möglicherweise nützlich, um das Paket als personalisierbares Webparts-Steuerelement enthaltenen Funktionen erstellen: Kalender, Listen, finanzielle Informationen News, Rechner, rich-Text-Steuerelemente zum Aktualisieren von Inhalt, bearbeitbaren Rastern verbinden, Datenbanken, Diagramme, die dynamisch aktualisieren ihre zeigt oder Wetter und Informationen zu übertragen. Wenn Sie einen visuellen Designer mit dem Steuerelement angeben, kann Klicken Sie dann alle Seite mithilfe von Visual Studio einfach ziehen Sie das Steuerelement in einer Webparts-Zone und es zur Entwurfszeit konfigurieren, ohne zusätzlichen Code schreiben zu müssen.

Personalisierung ist die Grundlage für die Webparts-Funktion. Er ermöglicht Benutzern das Ändern oder zu personalisieren Layout, Darstellung und Verhalten des Webparts-Steuerelemente auf einer Seite. Die personalisierten Einstellungen sind langlebig: werden beibehalten, nicht nur während der aktuellen Browsersitzung (wie bei Ansichtszustand) ist, aber auch in langfristigen Speicher so, dass die Einstellungen eines Benutzers für zukünftige Browsersitzungen gespeichert werden. Personalisierung ist standardmäßig für Webparts-Seiten aktiviert.

Der strukturellen Komponenten von UI Personalisierung abhängig ist, und geben Sie die grundlegende Struktur und Dienste, die von allen Steuerelementen für Webparts erforderlich sind. Eine erforderliche auf jeder Seite des Webparts strukturelle UI-Komponente ist das WebPartManager-Steuerelement. Obwohl nie sichtbar ist, hat dieses Steuerelement die wichtige Aufgabe alle Webparts-Steuerelemente auf einer Seite zu koordinieren. Beispielsweise wird es einzelnen Webparts-Steuerelemente nachverfolgt. Es verwaltet Webparts-Zonen (Bereiche, die Webparts-Steuerelemente auf einer Seite enthalten), und die Steuerelemente sind in der Zonen. Außerdem überwacht und steuert die verschiedenen Anzeigemodi, die eine Seite in einem solchen sein kann, wie durchsuchen, verbinden, bearbeiten oder Katalog Modus, und ob personalisierungsänderungen für alle Benutzer oder für einzelne Benutzer gelten. Schließlich initiiert und verfolgt Verbindungen und die Kommunikation zwischen Webparts-Steuerelemente.

Die zweite Art der strukturellen Benutzeroberflächenkomponente ist die Zone. Zonen fungieren als Layout-Managern auf einer Webparts-Seite. Sie enthalten und organisieren Steuerelemente, die von der Teilklasse (Teilsteuerelemente) abgeleitet werden, und bieten die Möglichkeit, das modulare Seitenlayout in horizontaler oder vertikaler Ausrichtung vorzunehmen. Zonen bieten auch allgemeiner und einheitlicher Benutzeroberflächenelemente (z. B. Kopf- und Fußzeile-Stil, Titel, Rahmenart, Aktionsschaltflächen usw.) für jedes Steuerelement, die sie enthalten. Diese gemeinsame Elemente werden als Chrom eines Steuerelements bezeichnet. Einige spezielle Typen von Zonen sind in den verschiedenen Anzeigemodi und mit verschiedenen Steuerelementen verwendet. Die verschiedenen Typen von Zonen werden wesentliche Webparts-Steuerelemente unten im Abschnitt beschrieben.

Web Parts-UI-Steuerelemente, alle durch von Ableiten der **Teil** Klasse, bilden die primäre Benutzeroberfläche für eine Webparts-Seite. Webparts-Steuerelementsatzes ist flexibel und in den Optionen inklusive gibt Ihnen zum Erstellen von Teilsteuerelementen. Zusätzlich zur Erstellung Ihrer eigenen benutzerdefinierten Webparts-Steuerelemente, können Sie auch vorhandene ASP.NET-Serversteuerelemente, Benutzersteuerelemente oder benutzerdefinierte Steuerelemente verwenden wie Webparts-Steuerelemente. Der wesentliche-Steuerelemente, die am häufigsten, zum Erstellen von Webparts-Seiten verwendet werden werden im nächsten Abschnitt beschrieben.

## <a name="web-parts-essential-controls"></a>Wesentliche Steuerelemente des Webparts

Die Webparts-Steuerelementsatzes ist umfangreich, aber einige Steuerelemente sind wichtig, da es für die Webparts funktionieren erforderlich sind, oder sie die Steuerelemente, die am häufigsten für Webparts-Seiten verwendet werden. Wie Sie beginnen, Webparts, und Erstellen von grundlegenden Webparts-Seiten, es hilfreich ist, mit der wichtige Webparts-Steuerelemente, die in der folgenden Tabelle beschriebenen vertraut sein.

| **Webparts-Steuerelement** | **Beschreibung** |
| --- | --- |
| WebPartManager | Verwaltet alle Webparts-Steuerelemente auf einer Seite. (Und einzige) **WebPartManager** -Steuerelement für alle Webparts-Seite erforderlich ist. |
| CatalogZone | CatalogPart Steuerelemente enthält. Verwenden Sie diese Zone, um einen Katalog von Webparts-Steuerelemente erstellen, in dem Benutzer Steuerelemente zum Hinzufügen zu einer Seite auswählen können. |
| EditorZone | EditorPart Steuerelemente enthält. Verwenden Sie diese Zone, um Benutzern das Bearbeiten und Personalisieren von Webparts-Steuerelemente auf einer Seite zu aktivieren. |
| WebPartZone | Enthält, und stellt allgemeine Layout für die Webparts-Steuerelemente, aus die die Hauptbenutzeroberfläche einer Seite zusammensetzt. Verwenden Sie diese Zone aus, wenn Sie Seiten mit Webparts-Steuerelemente erstellen. Seiten können einer oder mehreren Zonen enthalten. |
| ConnectionsZone | WebPartConnection Steuerelemente enthält, und bietet eine Benutzeroberfläche zum Verwalten von Verbindungen. |
| WebPart (GenericWebPart) | Gibt die primäre Benutzeroberfläche wieder. Die meisten Web Parts UI-Steuerelementen fallen in diese Kategorie. Für maximale programmgesteuerte Kontrolle, können Sie benutzerdefinierte Webparts-Steuerelemente, die von der Basisklasse ableiten erstellen **WebPart** Steuerelement. Sie können auch vorhandene Serversteuerelemente, Benutzersteuerelemente oder benutzerdefinierte Steuerelemente verwenden, wie Webparts-Steuerelemente. Wenn diese Steuerelemente in einer Zone platziert werden die **WebPartManager** Steuerelement automatisch umgebrochen wird mit **GenericWebPart** Steuerelemente zur Laufzeit, sodass Sie sie mit den Webparts-Funktionen verwenden können. |
| CatalogPart | Enthält eine Liste der verfügbaren Webparts-Steuerelemente, die Benutzer auf der Seite hinzufügen können. |
| WebPartConnection | Erstellt eine Verbindung zwischen zwei Webparts-Steuerelemente auf einer Seite. Die Verbindung definiert eine der Webparts-Steuerelemente als Anbieter (von Daten), und der andere als Consumer. |
| EditorPart | Dient als Basisklasse für die spezialisierten Editor-Steuerelemente. |
| EditorPart-Steuerelemente (AppearanceEditorPart, LayoutEditorPart BehaviorEditorPart und PropertyGridEditorPart) | Ermöglicht es Benutzern, um verschiedene Aspekte des Web Parts Benutzeroberflächen-Steuerelemente auf einer Seite zu personalisieren. |

## <a name="lab-create-a-web-part-page"></a>Lab: Erstellen einer Webpartseite

In dieser Übung erstellen Sie eine Webpart-Seite, die Informationen über ASP.NET-Profilen beibehält.

### <a name="creating-a-simple-page-with-web-parts"></a>Erstellen eine einfache Seite mit den Webparts

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie eine Seite, die Webparts-Steuerelemente zum Anzeigen statischen Inhalts verwendet. Der erste Schritt beim Arbeiten mit Webparts wird eine Seite mit zwei erforderlichen strukturellen Elemente zu erstellen. Eine Webparts-Seite benötigt zuerst ein Steuerelement WebPartManager nachverfolgen sowie alle Webparts-Steuerelemente zu koordinieren. Zweitens, benötigt eine Webparts-Seite einer oder mehreren Zonen, d. h. zusammengesetzte Steuerelemente, WebPart oder andere Serversteuerelemente enthalten und einen bestimmten Bereich einer Seite einnehmen.

> [!NOTE]
> Sie müssen nicht gar nichts Unternehmen, um Personalisierung des Webparts zu aktivieren. Sie ist für die Webparts-Steuerelementsatz standardmäßig aktiviert. Wenn Sie zuerst eine Webparts-Seite auf einer Website ausführen, richtet ASP.NET einen Standardanbieter für die Personalisierung um benutzerspezifische personalisierungseinstellungen zu speichern. Weitere Informationen zur Personalisierung finden Sie unter Web Parts Personalization Overview.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>So erstellen Sie eine Seite für die Aufnahme von Webparts-Steuerelemente

1. Schließen Sie die Standardseite, und fügen Sie eine neue Seite, um die Site namens WebPartsDemo.aspx.
2. Wechseln Sie zur **Entwurf** anzeigen.
3. Aus der **Ansicht** Menü, stellen Sie sicher, dass der **nicht sichtbare Gesamtwerte Steuerelemente** und **Details** Optionen ausgewählt sind, damit Sie sehen können, Layouttags und Steuerelemente, die nicht über eine Benutzeroberfläche verfügen.
4. Platzieren Sie die Einfügemarke an der Stelle vor dem  **&lt;Div&gt;**  tags auf der Entwurfsoberfläche, und drücken Sie EINGABETASTE, um eine neue Zeile hinzuzufügen. Positionieren Sie die Einfügemarke an der Stelle vor dem neue-Zeile-Zeichen, klicken Sie auf die **Block Format** Dropdownlisten-Steuerelements auf das Menü, und wählen die **Überschrift 1** Option. Fügen Sie den Text, in der Überschrift **Demo Webparts-Seite**.
5. Aus der **WebParts** Registerkarte Toolbox, ziehen Sie eine **WebPartManager** -Steuerelement auf die Seite, und positionieren sie direkt hinter dem neue-Zeile-Zeichen und vor der  **&lt;Div&gt;**  Tags.   
  
 Die **WebPartManager** Steuerelement wird die Ausgabe erstellt, nicht gerendert, damit es als graues Feld auf der Designeroberfläche angezeigt wird.
6. Die Einfügemarke innerhalb der  **&lt;Div&gt;**  Tags.
7. In der **Layout** Menü klicken Sie auf **Tabelle einfügen**, und erstellen Sie eine neue Tabelle mit einer Zeile und drei Spalten. Klicken Sie auf die **Cell Properties** auswählen **oben** aus der **vertikale Ausrichtung** Dropdown-Liste, klicken Sie auf **OK**, und klicken Sie auf **OK** erneut aus, um die Tabelle zu erstellen.
8. Ziehen Sie ein Steuerelement WebPartZone in der linken Spalte ein. Mit der rechten Maustaste die **WebPartZone** steuern, wählen Sie **Eigenschaften**, und legen Sie die folgenden Eigenschaften:   
  
 ID: SidebarZone   
  
 HeaderText: Randleiste
9. Ziehen Sie eine zweite **WebPartZone** -Steuerelement in der mittleren Spalte, und legen Sie die folgenden Eigenschaften:   
  
 ID: MainZone   
  
 HeaderText: Main
10. Speichern Sie die Datei.

Die Seite verfügt jetzt über zwei verschiedene Zonen, die Sie einzeln steuern können. Allerdings muss weder Zone alle Inhalte, damit erstellen die Inhalte der nächste Schritt besteht. In dieser exemplarischen Vorgehensweise zur Arbeit mit Webparts-Steuerelemente, in denen nur statische Inhalte angezeigt.

Das Layout einer Webparts-Zone wird angegeben, indem eine  **&lt;Zonetemplate&gt;**  Element. In der Zonenvorlage können Sie jedes ASP.NET-Steuerelement hinzufügen, ob es sich um ein benutzerdefiniertes Webparts-Steuerelement, ein benutzerdefiniertes Steuerelement oder einem vorhandenen Serversteuerelement handelt. Beachten Sie, dass hier Sie das Label-Steuerelement verwenden, und mit denen Sie einfach statischen Text hinzufügen. Beim Platzieren einer regulären Webserversteuerelement in einer **WebPartZone** Zone ASP.NET behandelt das Steuerelement als Webparts-Steuerelement zur Laufzeit, die Funktionen von Webparts für das Steuerelement aktiviert.

**Um Inhalte für die main-Zone erstellen**

1. In **Entwurf** anzuzeigen, ziehen Sie eine **Bezeichnung** -Steuerelement aus der **Standard** Registerkarte der Toolbox in den Inhaltsbereich der Zone, deren **ID** Eigenschaft wird auf MainZone festgelegt.
2. Wechseln Sie zur **Quelle** anzeigen. Beachten Sie, dass eine  **&lt;Zonetemplate&gt;**  Element wurde hinzugefügt, um umschließen die **Bezeichnung** Steuerelement in MainZone.
3. Hinzufügen eines Attributs mit dem Namen **Titel** auf die  **&lt;Asp: Label&gt;**  Element, und legen Sie dessen Wert auf Inhalt. Entfernen Sie den Text = "Label"-Attribut aus der  **&lt;Asp: Label&gt;**  Element. Zwischen dem Start- und Endtag des der  **&lt;Asp: Label&gt;**  Element, z. B. Hinzufügen von Text **Startseite Willkommen** innerhalb eines Paares  **&lt;h2 &gt;**  Element-Tags. Der Code sollte wie folgt aussehen. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Speichern Sie die Datei.

Als Nächstes erstellen Sie ein Benutzersteuerelement, die auch auf die Seite als Webparts-Steuerelement hinzugefügt werden kann.

### <a name="to-create-a-user-control"></a>So erstellen Sie ein benutzerdefiniertes Steuerelement

1. Fügen Sie ein neues Web-Benutzersteuerelement auf Ihrer Website, die als ein Steuerelement für die Suche dient. Deaktivieren Sie die Option zum **Quellcode in einer separaten Datei platzieren**. Fügen Sie es im selben Verzeichnis wie die Seite "WebPartsDemo.aspx" hinzu, und nennen Sie sie SearchUserControl.ascx.   
  
    > [!NOTE]
    > Das Benutzersteuerelement in dieser exemplarischen Vorgehensweise implementiert keine tatsächlichen Suchfunktion. Sie dient nur für Webparts-Funktionen zu veranschaulichen.
2. Wechseln Sie zur **Entwurf** anzeigen. Aus der **Standard** Registerkarte ein TextBox-Steuerelement von der Toolbox auf die Seite ziehen.
3. Platzieren Sie die Einfügemarke nach dem Textfeld, das Sie gerade hinzugefügt haben, und drücken Sie EINGABETASTE, um eine neue Zeile hinzuzufügen.
4. Ziehen Sie ein Button-Steuerelement auf der Seite auf der neuen Zeile unterhalb des Textfelds, das Sie gerade hinzugefügt haben.
5. Wechseln Sie zur **Quelle** anzeigen. Stellen Sie sicher, dass der Quellcode für das Benutzersteuerelement wie im folgenden Beispiel dargestellt. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Speichern und schließen Sie die Datei.

Jetzt können Sie Webparts-Steuerelemente auf der Randleistenzone hinzufügen. Hinzufügen von zwei Steuerelementen auf der Randleistenzone, erstellt, enthält eine Liste von Links und anderen, die das Benutzersteuerelement wird im vorherigen Verfahren. Die Links werden hinzugefügt, als Standard **Bezeichnung** Serversteuerelement, ähnlich wie bei der Erstellung der statischen Text für die Main-Zone. Obwohl in den einzelnen Serversteuerelemente enthalten das Benutzersteuerelement direkt in der Zone (z. B. das Label-Steuerelement) enthalten sein könnten jedoch in diesem Fall sind sie nicht. Stattdessen sind sie Teil des Benutzersteuerelements, das Sie im vorherigen Verfahren erstellt haben. Dies beweist ein gängiges Verfahren zum Verpacken, die Steuerelemente und zusätzliche Funktionen, die in einem Benutzersteuerelement angezeigt werden soll, und verweisen Sie auf das Steuerelement in einer Zone als Webparts-Steuerelement.

Zur Laufzeit umschließt der Webparts-Steuerelementsatz beide Steuerelemente mit GenericWebPart-Steuerelementen. Wenn eine **GenericWebPart** Steuerelement umschließt ein Webserver-Steuerelement, das generische Teil-Steuerelement ist das übergeordnete Steuerelement und können Sie das Webserversteuerelement durch das übergeordnete Steuerelement ChildControl-Eigenschaft zugreifen. Diese Verwendung von generischen Teilsteuerelemente ermöglicht standard-Webserversteuerelemente aufweisen, die dasselbe grundlegende Verhalten und die Attribute wie Webparts-Steuerelemente, die Ableitung der **WebPart** Klasse.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Die Randleistenzone Webparts-Steuerelemente hinzu

1. Öffnen Sie die Seite "WebPartsDemo.aspx".
2. Wechseln Sie zur **Entwurf** anzeigen.
3. Ziehen Sie die Seite "Benutzer-Steuerelement" Sie erstellt haben, SearchUserControl.ascx aus **Projektmappen-Explorer** in der Zone, deren **ID** Eigenschaft auf SidebarZone festgelegt ist, und legen Sie sie es.
4. Speichern Sie die Seite "WebPartsDemo.aspx".
5. Wechseln Sie zur **Quelle** anzeigen.
6. Innerhalb der  **&lt;Asp: Webpartzone&gt;**  -Element für die SidebarZone direkt über den Verweis auf das Benutzersteuerelement, fügen eine  **&lt;Asp: Label&gt;**  Element mit dem darin enthaltenen Links, wie im folgenden Beispiel gezeigt. Darüber hinaus fügen eine **Titel** -Attribut zum Tag-Steuerelement Benutzer mit einem Wert von **Suche**, wie dargestellt. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Speichern und schließen Sie die Datei.

Jetzt können Sie Ihre Seite navigieren, in Ihrem Browser testen. Die Seite zeigt die beiden Zonen. Der folgende Screenshot zeigt die Seite.

**Webparts-Demoseite mit zwei Zonen**


![Screenshot zur exemplarischen Vorgehensweise 1 Web Teile im Vergleich](profiles-themes-and-web-parts/_static/image3.gif)

**Abbildung 3**: Web Parts VS exemplarischen Vorgehensweise 1 Screenshot


In der Titelleiste wird jedes Steuerelements ein nach unten weisenden Pfeil, der Zugriff auf eine Verbenmenü der verfügbaren Aktionen bereitstellt, die Sie für ein Steuerelement ausführen können. Klicken Sie auf das Menü für die Verben für eines der Steuerelemente, und klicken Sie auf die **Minimieren** Verb, und beachten Sie, dass das Steuerelement minimiert wird. Klicken Sie im Verben auf **wiederherstellen**, und die Steuerung der Normalgröße zurück.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Aktivieren von Benutzern für Bearbeitungsseiten und das Layout ändern

Webparts bietet die Möglichkeit für Benutzer so ändern Sie das Layout der Steuerelemente des Webparts durch Ziehen von einer Zone an einen anderen. Zusätzlich zu ermöglichen, dass Benutzer verschieben **WebPart** Steuerelemente aus einer Zone auf einen anderen, können Sie Benutzer verschiedene Eigenschaften der Steuerelemente, einschließlich ihrer Darstellung, die Layout und die Verhalten bearbeiten. Die Webparts-Steuerelementsatzes bietet grundlegende Bearbeitungsfunktionen für **WebPart** Steuerelemente. Obwohl dies in dieser exemplarischen Vorgehensweise nicht vorgesehen ist, können Sie auch benutzerdefinierten Editor-Steuerelemente, mit denen Benutzer so bearbeiten Sie die Funktionen von erstellen **WebPart** Steuerelemente. Wie bei Änderung der Position einer **WebPart** Steuerelement Bearbeiten der Eigenschaften eines Steuerelements basiert auf ASP.NET-Personalisierung, um die Änderungen zu speichern, die Benutzer vornehmen.

In diesem Teil der exemplarischen Vorgehensweise müssen Sie die Möglichkeit für Benutzer so bearbeiten Sie die grundlegenden Eigenschaften eines beliebigen hinzufügen **WebPart** Steuerelement auf der Seite. Sie fügen um diese Funktionen ermöglichen es, ein weiteres benutzerdefiniertes Steuerelement auf der Seite zusammen mit einem  **&lt;Asp: Editorzone&gt;**  -Element und zwei Bearbeitungssteuerelementen.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Zum Erstellen eines Benutzersteuerelements ermöglicht, die veränderten Seitenlayout

1. In Visual Studio auf die **Datei** klicken Sie im Menü der **neu** Untermenü, und klicken Sie auf die **Datei** Option.
2. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Webbenutzersteuerelement**. Der Name der neuen Datei DisplayModeMenu.ascx. Deaktivieren Sie die Option zum **Code in separaten Datei platzieren**.
3. Klicken Sie auf Hinzufügen, um das neue Steuerelement zu erstellen.
4. Wechseln Sie zur **Quelle** anzeigen.
5. Entfernen Sie den vorhandenen Code in der neuen Datei, und fügen Sie in den folgenden Code. Dieses Benutzersteuerelement verwendet Funktionen der Webparts-Steuerelemente, die eine Seite, um eine Sicht oder Anzeigemodus ermöglichen außerdem ermöglicht es Ihnen, ändern Sie die physische Darstellung und Layout der Seite während sind in bestimmten Anzeigemodi. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Speichern Sie die Datei, indem Sie auf die speichern-Symbol auf der Symbolleiste oder durch Auswahl **speichern** auf die **Datei** Menü.

### <a name="to-enable-users-to-change-the-layout"></a>So aktivieren Sie Benutzern das Ändern des Layouts

1. Öffnen Sie die Seite "WebPartsDemo.aspx", und wechseln Sie zur **Entwurf** anzeigen.
2. Die Einfügemarke in die **Entwurf** anzeigen direkt nach der **WebPartManager** Steuerelement, das Sie zuvor hinzugefügt haben. Eine Absatzmarke nach dem Text hinzufügen, sodass es ist eine leere Zeile nach der **WebPartManager** Steuerelement. Platzieren Sie die Einfügemarke in die leere Zeile.
3. Ziehen Sie das Benutzersteuerelement, das Sie gerade erstellt haben (die Datei heißt DisplayModeMenu.ascx) in der WebPartsDemo.aspx Seite, und legen Sie sie in der leeren Zeile.
4. Ziehen Sie ein Steuerelement EditorZone aus der **WebParts** Abschnitt der Toolbox auf die verbleibenden öffnen Tabellenzelle auf der Seite WebPartsDemo.aspx.
5. Aus der **WebParts** Abschnitt der Toolbox ziehen Sie ein Steuerelement AppearanceEditorPart und ein LayoutEditorPart-Steuerelement in der **EditorZone** Steuerelement.
6. Wechseln Sie zur **Quelle** anzeigen. Der resultierende Code in der Tabellenzelle sollte dem folgenden Code ähneln. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Speichern Sie die WebPartsDemo.aspx-Datei. Sie ein Benutzersteuerelement, mit dem Sie ändern Anzeigemodi und Seitenlayout erstellt haben, und Sie haben das Steuerelement auf der primären Webseite verwiesen.

Sie können jetzt testen die Funktion zum Bearbeiten von Seiten und Layout zu ändern.

### <a name="to-test-layout-changes"></a>So testen Sie die Änderungen am layout

1. Laden Sie die Seite in einem Browser.
2. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen Sie **bearbeiten**. Die Zonentitel werden angezeigt.
3. Ziehen Sie die **Meine Links** Steuerelement der Titelleiste aus der Zone Randleiste an das Ende der Main-Zone. Die Seite sollte wie im folgenden Screenshot aussehen.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Webparts-Demoseite mit Meine Linksteuerelement verschoben


![Screenshot zur exemplarischen Vorgehensweise 2-Web Teile im Vergleich](profiles-themes-and-web-parts/_static/image4.gif)

**Abbildung 4**: Web Screenshot Vorgehensweise 2 Teile im Vergleich zur


1. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen Sie **Durchsuchen**. Die Seite wird aktualisiert, die Zonennamen werden ausgeblendet, und die **Meine Links** -Steuerelement verbleibt dort, wo Sie positioniert.
2. Um zu zeigen, wird die Funktionsfähigkeit Personalisierung, schließen Sie den Browser, und Laden Sie die Seite erneut. Die vorgenommenen Änderungen werden für zukünftige Browsersitzungen gespeichert.
3. Aus der **Anzeigemodus** klicken Sie im Menü **bearbeiten**.   
  
 Jedes Steuerelement auf der Seite wird jetzt mit einem nach unten weisenden Pfeil in der Titelleiste angezeigt, die Verben-Dropdownmenü enthält.
4. Klicken Sie auf den Pfeil, um das im Verbmenü Anzeigen der **Meine Links** Steuerelement. Klicken Sie auf die **bearbeiten** Verb.   
  
 Die **EditorZone** Steuerelement angezeigt wird, Anzeigen der EditorPart-Steuerelemente hinzugefügt.
5. In der **Darstellung** Abschnitt des Bearbeitungssteuerelements, Änderung der **Titel** Favoriten, verwenden die **Chromtyp** -Dropdownliste auswählen **nur Titel**, und klicken Sie dann auf **übernehmen**. Der folgende Screenshot zeigt die Seite im Bearbeitungsmodus befindet.

### <a name="web-parts-demo-page-in-edit-mode"></a>Webparts-Demoseite im Bearbeitungsmodus


![Screenshot zur exemplarischen Vorgehensweise 3-Web Teile im Vergleich](profiles-themes-and-web-parts/_static/image5.gif)

**Abbildung 5**: Web Screenshot Vorgehensweise 3 Teile im Vergleich zur


1. Klicken Sie auf die **Anzeigemodus** , und wählen **Durchsuchen** zurückzugebenden Durchsuchen-Modus.
2. Das Steuerelement hat jetzt einen aktualisierten Titel und kein Rahmen, wie im folgenden Screenshot gezeigt.

### <a name="edited-web-parts-demo-page"></a>Bearbeitete Webparts-Demoseite


![Screenshot zur exemplarischen Vorgehensweise 4-Web Teile im Vergleich](profiles-themes-and-web-parts/_static/image6.gif)

**Abbildung 4**: Web Screenshot Vorgehensweise 4 Teile im Vergleich zur


### <a name="adding-web-parts-at-run-time"></a>Hinzufügen von Webparts zur Laufzeit

Sie können auch Benutzer ihrer Seite Webparts-Steuerelemente zur Laufzeit hinzufügen. Zu diesem Zweck konfigurieren Sie die Seite mit einem Webparts-Katalog, der eine Liste der Webparts-Steuerelemente enthält, die Sie für Benutzer verfügbar machen möchten.

**Damit Benutzer zur Laufzeit Webparts hinzufügen können**

1. Öffnen Sie die Seite "WebPartsDemo.aspx", und wechseln Sie zur **Entwurf** anzeigen.
2. Aus der **WebParts** Registerkarte der Toolbox ziehen Sie ein CatalogZone-Steuerelement in der rechten Spalte der Tabelle, darunter die **EditorZone** Steuerelement.   
  
 Beide Steuerelemente können in der gleichen Tabellenzelle sein, da sie nicht gleichzeitig angezeigt werden.
3. Klicken Sie im Bereich "Eigenschaften" zuweisen die Zeichenfolge **Webparts hinzufügen** der HeaderText-Eigenschaft von der **CatalogZone** Steuerelement.
4. Aus der **WebParts** Abschnitt der Toolbox ziehen Sie ein DeclarativeCatalogPart-Steuerelement in den Inhaltsbereich des der **CatalogZone** Steuerelement.
5. Klicken Sie auf den Pfeil in der oberen rechten Ecke des der **DeclarativeCatalogPart** steuern, um seine Menü "Aufgaben" verfügbar zu machen, und wählen Sie dann **Vorlagen bearbeiten**.
6. Aus der **Standard** Abschnitt der Toolbox, ziehen Sie eine **FileUpload** Steuerelement und ein **Kalender** -Steuerelement auf die **WebPartsTemplate** im Abschnitt der **DeclarativeCatalogPart** Steuerelement.
7. Wechseln Sie zur **Quelle** anzeigen. Überprüfen Sie den Quellcode der  **&lt;Asp: Catalogzone&gt;**  Element. Beachten Sie, dass die **DeclarativeCatalogPart** Steuerelement enthält eine  **&lt;Webpartstemplate&gt;**  Element mit den beiden Serversteuerelemente eingeschlossen, die Sie auf der Seite hinzugefügt werden aus dem Katalog.
8. Hinzufügen einer **Titel** Eigenschaft auf die einzelnen Sie hinzugefügten Steuerelemente im Katalog mit den Zeichenfolgenwert gezeigt für jeden einzelnen Titel im folgenden Codebeispiel. Obwohl der Titel nicht auf eine Eigenschaft ist normalerweise einstellbaren für diese beiden Serversteuerelemente zur Entwurfszeit, wenn ein Benutzer diese Steuerelemente hinzufügt eine **WebPartZone** Zone aus dem Katalog zur Laufzeit, sie werden jeweils in eingeschlossen ein  **GenericWebPart** Steuerelement. Dies ermöglicht ihnen, wie Webparts-Steuerelemente zu fungieren, damit sie Titel angezeigt werden können.   
  
 Der Code für die beiden Steuerelemente enthalten, die der **DeclarativeCatalogPart** Steuerelement sollte nun folgendermaßen aussehen. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Speichern Sie die Seite.

Sie können jetzt den Katalog testen.

### <a name="to-test-the-web-parts-catalog"></a>Zum Testen des Webparts-Katalogs

1. Laden Sie die Seite in einem Browser.
2. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen Sie **Katalog**.   
  
 Der Katalog mit dem Titel **Webparts hinzufügen** wird angezeigt.
3. Ziehen Sie die **Favoriten** zurück zum Anfang der Randleistenzone aus der Main-Zone zu steuern, und legen Sie sie es.
4. In der **Webparts hinzufügen** Katalog, wählen Sie beide Kontrollkästchen, und wählen Sie dann **Main** aus der Dropdown-Liste, die die verfügbaren Zonen enthält.
5. Klicken Sie auf **hinzufügen** im Katalog. Die Main-Zone werden die Steuerelemente hinzugefügt. Wenn Sie möchten, können Sie mehrere Instanzen von Steuerelementen aus dem Katalog auf der Seite hinzufügen.   
  
 Der folgende Screenshot zeigt die Seite mit den Dateiupload-Steuerelement und den Kalender, in der Main-Zone. 

![Steuerelemente, die aus dem Katalog der Main-Zone hinzugefügt](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Klicken Sie auf die **Anzeigemodus** Dropdown-Menü, und wählen Sie **Durchsuchen**. Der Katalog wird ausgeblendet, und die Seite wird aktualisiert.
7. Schließen Sie den Browser. Laden Sie die Seite erneut. Die Änderungen vorgenommen Sie persistent zu speichern.
