---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Grundlegendes zu ASP.NET AJAX-Authentifizierung und-Profilanwendungsdienste | Microsoft Docs
author: scottcate
description: Der Authentifizierungsdienst kann Benutzer Anmeldeinformationen angeben, um ein Authentifizierungscookie zu erhalten, und der Gatewaydienst benutzerdefinierten zulassen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Grundlegendes zu ASP.NET AJAX-Authentifizierung und-Profilanwendungsdienste
====================
durch [Scott Kate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Der Authentifizierungsdienst kann Benutzer Anmeldeinformationen angeben, um ein Authentifizierungscookie zu erhalten, und dient der Gatewaydienst benutzerdefinierte Benutzerprofile zulassen von ASP.NET. Verwendung von ASP.NET AJAX-Authentifizierungsdienst ist kompatibel mit der standardmäßigen ASP.NET-Formularauthentifizierung, damit Anwendungen derzeit unter Verwendung der Formularauthentifizierung (z. B. mit der Anmeldung steuern), nicht beeinträchtigt werden wird durch ein Upgrade auf den Authentifizierungsdienst AJAX.


## <a name="introduction"></a>Einführung

Im Rahmen des .NET Framework 3.5 wird übermittelt Microsoft ein Upgrade beträchtliche Anzahl an Umgebung; nicht nur eine neue Entwicklungsumgebung verfügbar ist, sondern die neuen Funktionen der Language-Integrated Query (LINQ) und andere spracherweiterungen sind in Planung. Darüber hinaus sind einige bekannten Funktionen von anderen Toolsets, insbesondere die ASP.NET AJAX-Erweiterungen, als die .NET Framework-Basisklassenbibliothek erstrangige Mitglieder enthalten wird. Diese Erweiterungen ermöglichen viele neue rich-Client-Funktionen, einschließlich Teilrendering von Seiten ohne eine vollständige Seite-Aktualisierung, den Zugriff auf Webdienste über Clientskripts (einschließlich ASP.NET profilerstellungs-API) und eine umfangreiche clientseitige API entwickelt, um viele die Steuerelement-Schemas in der Menge der ASP.NET serverseitiges Steuerelement sichtbar zu spiegeln.

In diesem Whitepaper, prüft die Implementierung und Verwendung der Profilerstellungsdaten ASP.NET und Formularauthentifizierung Dienste wie sie durch den AJAX-Erweiterungen für Microsoft ASP.NET AJAX ExtensionsThe verfügbar gemacht werden Stellen Formularauthentifizierung unterstützt werden, da sie äußerst einfach (sowie der Datenprofilerstellungs-Dienst) wird über einen Webdienst-Proxy-Skript bereitgestellt. Die AJAX-Erweiterungen unterstützen auch benutzerdefinierten Authentifizierung über die AuthenticationServiceManager-Klasse.

In diesem Whitepaper basiert auf der Beta 2-Version von Visual Studio 2008 und .NET Framework 3.5. In diesem Whitepaper wird davon ausgegangen, dass Sie mit Visual Studio 2008 Beta 2 nicht Visual Web Developer Express arbeiten und gebe Exemplarische Vorgehensweisen gemäß der Benutzeroberfläche von Visual Studio. Einige Codebeispiele können-Projektvorlagen in Visual Web Developer Express nicht verfügbar, nutzen.

## <a name="profiles-and-authentication"></a>*Profile und Authentifizierung*

Die Microsoft ASP.NET-Profile und den Authentifizierungsdiensten werden von der ASP.NET-Formularauthentifizierung-System bereitgestellt und Standardkomponenten von ASP.NET sind. Die ASP.NET AJAX-Erweiterungen bieten Skriptzugriff auf diese Dienste über Skriptproxys über ein Modell recht einfach, unter dem Namespace Sys.Services der AJAX-Clientbibliothek.

Der Authentifizierungsdienst kann Benutzer Anmeldeinformationen angeben, um ein Authentifizierungscookie zu erhalten, und dient der Gatewaydienst benutzerdefinierte Benutzerprofile zulassen von ASP.NET. Verwendung von ASP.NET AJAX-Authentifizierungsdienst ist kompatibel mit der standardmäßigen ASP.NET-Formularauthentifizierung, damit Anwendungen derzeit unter Verwendung der Formularauthentifizierung (z. B. mit der Anmeldung steuern), nicht beeinträchtigt werden wird durch ein Upgrade auf den Authentifizierungsdienst AJAX.

Die Profil-Dienst ermöglicht die automatische Integration und Speicherung von Benutzerdaten auf der Grundlage der Mitgliedschaft der Authentifizierungsdienst bereitgestellt. Die gespeicherten Daten werden von der Datei "Web.config" angegeben, und die verschiedenen profilerstellungs-Anbieter behandeln die datenverwaltung. Wie bei der Authentifizierungsdienst ist Profildienst AJAX kompatibel mit der standardmäßigen ASP.NET-Profildienst, sodass Seiten, die derzeit spezifische Funktionen von ASP.NET Profildienst nicht beeinträchtigt werden sollten, indem ein einschließlich AJAX-Unterstützung.

Die ASP.NET-Authentifizierung und Profilerstellung Dienste selbst in eine Anwendung integrieren ist außerhalb des Bereichs dieses Whitepaper verfasst. Weitere Informationen zu diesem Thema finden Sie in der MSDN Library verweisen auf Artikel Verwalten von Benutzern durch Mitgliedschaft an [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET enthält auch ein Hilfsprogramm automatisch Mitgliedschaft mit einem SQL-Server einrichten, das der Dienstanbieter für Standard-Authentifizierung für die ASP.NET-Mitgliedschaft ist. Weitere Informationen finden Sie im Artikel ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe) am [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Mithilfe den ASP.NET AJAX-Authentifizierungsdienst*

Die Authentifizierung in ASP.NET AJAX-Dienst muss in der Datei "Web.config" aktiviert werden:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Der Authentifizierungsdienst erfordert ASP.NET-Formularauthentifizierung aktiviert werden und erfordert von Cookies auf dem Clientbrowser aktiviert sein (ein Skript eine cookieless Sitzung ist nicht möglich, da Sitzungen ohne Cookies URL-Parameter erforderlich sind).

Sobald die AJAX-Dienst-Authentifizierung aktiviert und konfiguriert ist, kann Clientskripts sofort des Objekts Sys.Services.AuthenticationService nutzen. In erster Linie Clientskripts nutzen sollten die `login` Methode und `isLoggedIn` Eigenschaft. Mehrere Eigenschaften vorhanden, um die Standardwerte für die Anmeldemethode bereitstellen, eine große Anzahl von Parametern annehmen kann.

*Sys.Services.AuthenticationService-Elemente*

*Login-Methode:*

Die login() Methode startet eine Anforderung an die Anmeldeinformationen des Benutzers zu authentifizieren. Diese Methode ist asynchron und blockiert die Ausführung nicht.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| userName | Erforderlich. Die zu authentifizierende Benutzername. |
| Kennwort | Optional (Standardwert null). Das Kennwort des Benutzers. |
| isPersistent | Optional (Standardwert "false"). Gibt an, ob der Benutzer Authentifizierungscookie sitzungsübergreifend beibehalten werden sollen. Wenn "false" wird den Benutzer abzumelden, wenn der Browser geschlossen ist oder die Sitzung abläuft. |
| redirectUrl | Optional (Standardwert null). Die URL, an den Browser nach erfolgreicher Authentifizierung umgeleitet werden soll. Wenn dieser Parameter Null oder eine leere Zeichenfolge ist, tritt keine Umleitung. |
| customInfo | Optional (Standardwert null). Dieser Parameter wird derzeit nicht verwendet und ist für die zukünftige Verwendung reserviert. |
| loginCompletedCallback | Optional (Standardwert null). Die Funktion, die aufgerufen wird, wenn die Anmeldung erfolgreich abgeschlossen wurde. Wenn angegeben, überschreibt dieser Parameter die DefaultLoginCompleted-Eigenschaft. |
| failedCallback | Optional (Standardwert null). Die Funktion aufrufen, wenn die Anmeldung fehlgeschlagen ist. Wenn angegeben, überschreibt dieser Parameter die DefaultFailedCallback-Eigenschaft. |
| userContext | Optional (Standardwert null). Benutzerdefinierte Kontextdaten, die an die Rückruffunktionen übergeben werden sollen. |

*Rückgabewert:*

Diese Funktion umfasst nicht den Rückgabewert. Allerdings sind eine Reihe von Verhaltensweisen beim Abschluss von einen Aufruf dieser Funktion enthalten:

- Die aktuelle Seite werden entweder aktualisiert oder geändert werden, wenn die `redirectUrl` Parameter war weder Null noch eine leere Zeichenfolge.
- Jedoch, wenn der Parameter Null oder eine leere Zeichenfolge ist, wurde die `loginCompletedCallback` -Parameter oder `defaultLoginCompletedCallback` Eigenschaft wird aufgerufen.
- Wenn der Aufruf an den Webdienst ein Fehler auftritt, die `failedCallback` Parameter `defaultFailedCallback` Eigenschaft wird aufgerufen.

*Logout-Methode:*

Die logout()-Methode entfernt die Anmeldeinformationen Cookie und meldet den aktuellen Benutzer aus der Webanwendung.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| redirectUrl | Optional (Standardwert null). Die URL, an den Browser nach erfolgreicher Authentifizierung umgeleitet werden soll. Wenn dieser Parameter Null oder eine leere Zeichenfolge ist, tritt keine Umleitung. |
| logoutCompletedCallback | Optional (Standardwert null). Die Funktion, die aufgerufen wird, wenn die Abmeldung erfolgreich abgeschlossen wurde. Wenn angegeben, überschreibt dieser Parameter die DefaultLogoutCompleted-Eigenschaft. |
| failedCallback | Optional (Standardwert null). Die Funktion aufrufen, wenn die Anmeldung fehlgeschlagen ist. Wenn angegeben, überschreibt dieser Parameter die DefaultFailedCallback-Eigenschaft. |
| userContext | Optional (Standardwert null). Benutzerdefinierte Kontextdaten, die an die Rückruffunktionen übergeben werden sollen. |

*Rückgabewert:*

Diese Funktion umfasst nicht den Rückgabewert. Allerdings sind eine Reihe von Verhaltensweisen beim Abschluss von einen Aufruf dieser Funktion enthalten:

- Die aktuelle Seite werden entweder aktualisiert oder geändert werden, wenn die `redirectUrl` Parameter war weder Null noch eine leere Zeichenfolge.
- Jedoch, wenn der Parameter Null oder eine leere Zeichenfolge ist, wurde die `logoutCompletedCallback` -Parameter oder `defaultLogoutCompletedCallback` Eigenschaft wird aufgerufen.
- Wenn der Aufruf an den Webdienst ein Fehler auftritt, die `failedCallback` Parameter `defaultFailedCallback` Eigenschaft wird aufgerufen.

*DefaultFailedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, wenn ein Fehler für die Kommunikation mit dem Webdienst auftritt. Sie sollten einen Delegaten (oder Funktionsverweis) erhalten.

Der von dieser Eigenschaft angegebenen Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| error | Gibt die Fehlerinformationen an. |
| userContext | Gibt die Benutzerkontextinformationen bereitgestellt, wenn die an- bzw. Abmeldung-Funktion aufgerufen wurde. |
| methodName | Der Name der Aufrufmethode. |

*DefaultLoginCompletedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, bei der Anmeldung Webdienstaufruf abgeschlossen wurde. Sie sollten einen Delegaten (oder Funktionsverweis) erhalten.

Der von dieser Eigenschaft angegebenen Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| validCredentials | Gibt an, ob der Benutzer über gültige Anmeldeinformationen bereitgestellt. `true` Wenn der Benutzer erfolgreich angemeldet hat; andernfalls `false`. |
| userContext | Gibt die Benutzerkontextinformationen bereitgestellt, wenn die Anmeldefunktion aufgerufen wurde. |
| methodName | Der Name der Aufrufmethode. |

*DefaultLogoutCompletedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, wenn Logout Webdienstaufruf abgeschlossen wurde. Sie sollten einen Delegaten (oder Funktionsverweis) erhalten.

Der von dieser Eigenschaft angegebenen Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| Ergebnis | Dieser Parameter werden immer `null`; sie ist für die zukünftige Verwendung reserviert. |
| userContext | Gibt die Benutzerkontextinformationen bereitgestellt, wenn die Anmeldefunktion aufgerufen wurde. |
| methodName | Der Name der Aufrufmethode. |

*IsLoggedIn-Eigenschaft (Get):*

Diese Eigenschaft ruft den aktuellen Authentifizierungsstatus des Benutzers ab. Es ist von dem Objekt ScriptManager während einer Seitenanforderung festgelegt.

Diese Eigenschaft gibt `true` , wenn der Benutzer zurzeit, andernfalls angemeldet ist gibt es `false`.

*Path-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt programmgesteuert den Speicherort des Authentifizierungsdiensts Web. Es kann verwendet werden, um den Standardanbieter für die Authentifizierung sowie eine überschreiben deklarativ in der Path-Eigenschaft des ScriptManager-Steuerelement AuthenticationService untergeordneter Knoten festgelegt (Weitere Informationen finden Sie unter Verwenden einer benutzerdefinierten Service Authentifizierungsanbieter Thema weiter unten).

Beachten Sie, dass der Speicherort des Authentifizierungsdiensts standardmäßig nicht ändert. Allerdings können mit ASP.NET AJAX den Speicherort eines Webdiensts angeben, die die gleichen Klassenschnittstelle als ASP.NET AJAX-Proxys für die Authentifizierung Dienst bereitstellt.

Beachten Sie außerdem, dass diese Eigenschaft nicht auf einen Wert festgelegt werden soll, die aus dem aktuellen Standort die Skript-Anforderung weiterleitet. Da die aktuelle Anwendung keine Anmeldeinformationen für die Authentifizierung erhalten würden, wäre es nutzlos; Darüber hinaus die zugrunde liegende Technologie AJAX Cross-Site-Anforderungen sollten nicht gesendet, und ggf. eine Sicherheitsausnahme im Clientbrowser zu generieren.

Diese Eigenschaft ist ein `String` Objekt, das den Pfad an den Webdienst für die Authentifizierung darstellt.

*Timeout-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt, dass die Länge der Wartezeit für den Authentifizierungsdienst, vor der Anmeldung Anforderung fehlgeschlagen ist. Wenn es sich bei Ablauf des Timeouts beim Warten auf eines Aufrufs abgeschlossen wird der Rückruf Fehler bei der Anforderung aufgerufen werden, und der Aufruf nicht abgeschlossen.

Diese Eigenschaft ist ein `Number` Objekt, das die Anzahl der Millisekunden zu warten, bis Ergebnisse aus dem Authentifizierungsdienst darstellt.

*Codebeispiel: Anmeldung bei der Authentifizierungsdienst*

Das folgende Markup wird ein Beispiel ASP.NET-Seite mit einem einfachen Skriptaufruf die Anmelde- und Abmeldeereignisse Methoden der Klasse AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Zugreifen auf Daten über AJAX-ASP.NET-PROFILERSTELLUNG

Die ASP.NET-PROFILERSTELLUNG-Dienst wird ebenfalls über ASP.NET AJAX-Erweiterungen verfügbar gemacht. Da der ASP.NET-profilerstellungs-Dienst eine umfassende und präzise API nach dem Speichern und Abrufen von Benutzerdaten bietet, kann dies eine ausgezeichnete Produktivitätstool sein.

Die Profildienst muss in "Web.config" aktiviert sein. Es ist nicht standardmäßig. Zu diesem Zweck sicher, dass die `profileService` untergeordnetes Element wurde aktiviert = "true" angegebenen in "Web.config", und Sie angegeben haben, welche Eigenschaften gelesen oder wie folgt geschrieben werden können:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Die Profildienst muss ebenfalls konfiguriert werden. Obwohl der Profilerstellungsdaten Dienstkonfiguration außerhalb des Bereichs dieses Whitepaper verfasst wird, ist es sinnvoll zu wissen, dass Gruppen gemäß Konfiguration der profileinstellungen als untergeordnete Eigenschaften der Gruppenname zugegriffen werden können. Z. B. mit dem folgenden Profil-Abschnitt angegeben:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Clientskripts würde Name, Address.Line1 Address.Line2, Address.City, Address.State, Address.Zip und BackgroundColor als Eigenschaften des Felds "Eigenschaften" der Klasse ProfileService zugreifen können.

Sobald die Profilerstellung AJAX-Dienst konfiguriert ist, wird diese sofort auf Seiten zur Verfügung; Sie müssen jedoch vor der Verwendung einmal geladen werden.

*Sys.Services.ProfileService-Elemente*

*Feld für die Eigenschaften:*

Das Eigenschaftenfeld stellt alle konfigurierten Profildaten als untergeordneten Eigenschaften, die mit der Convention Operatorname Punkt verwiesen werden können. Eigenschaften, die die untergeordneten Elemente des Eigenschaftengruppen sind, werden als GroupName.PropertyName bezeichnet. In der oben aufgeführten Beispiel-Profilkonfiguration können Sie zum Abrufen des Status des Benutzers, den folgenden Bezeichner verwenden:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Load-Methode:*

Lädt eine ausgewählte Liste oder alle Eigenschaften vom Server an.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| propertyNames | Optional (Standardwert null). Die Eigenschaften vom Server geladen werden soll. |
| loadCompletedCallback | Optional (Standardwert null). Die Funktion, die aufgerufen wird, wenn der Ladevorgang abgeschlossen ist. |
| failedCallback | Optional (Standardwert null). Die Funktion, die aufgerufen wird, wenn ein Fehler auftritt. |
| userContext | Optional (Standardwert null). Die Kontextinformationen, die an die Rückruffunktion übergeben werden. |

Die Lastfunktion keinen Rückgabewert. Wenn der Aufruf erfolgreich abgeschlossen, es entweder aufrufen wird die `loadCompletedCallback` Parameter oder `defaultLoadCompletedCallback` Eigenschaft. Wenn der Aufruf fehlgeschlagen ist oder das Timeout ist, entweder abgelaufen die `failedCallback` Parameter oder `defaultFailedCallback` Eigenschaft aufgerufen wird.

Wenn die `propertyNames` Parameter nicht angegeben wird, werden alle Lese-konfigurierte Eigenschaften vom Server abgerufen.

*Save-Methode:*

Die save()-Methode speichert das Profil des Benutzers ASP.NET angegebenen Eigenschaftenliste (oder alle Eigenschaften).

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| propertyNames | Optional (Standardwert null). Die Eigenschaften, die auf dem Server gespeichert werden. |
| saveCompletedCallback | Optional (Standardwert null). Die Funktion aufrufen, beim Speichern abgeschlossen wurde. |
| failedCallback | Optional (Standardwert null). Die Funktion, die aufgerufen wird, wenn ein Fehler auftritt. |
| userContext | Optional (Standardwert null). Die Kontextinformationen, die an die Rückruffunktion übergeben werden. |

Der Speichervorgang Funktion hat keinen Rückgabewert. Wenn der Aufruf erfolgreich ausgeführt wird, wird es rufen Sie entweder die `saveCompletedCallback` Parameter oder `defaultSaveCompletedCallback` Eigenschaft. Wenn der Aufruf fehlgeschlagen ist oder das Timeout ist, entweder abgelaufen die `failedCallback` oder `defaultFailedCallback` Eigenschaft aufgerufen wird.

Wenn die `propertyNames` -Parameter ist null, alle Profileigenschaften werden an den Server gesendet werden und der Server entscheidet, welche Eigenschaften gespeichert werden können und welche nicht.

*DefaultFailedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, wenn ein Fehler für die Kommunikation mit dem Webdienst auftritt. Sie sollten einen Delegaten (oder Funktionsverweis) erhalten.

Der von dieser Eigenschaft angegebenen Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| Fehler | Gibt die Fehlerinformationen an. |
| userContext | Gibt an, die Benutzerkontextinformationen bereitgestellt, wenn das Laden oder Speichern der Funktion aufgerufen wurde. |
| methodName | Der Name der Aufrufmethode. |

*DefaultSaveCompleted-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion, die nach dem Abschluss des Speicherns von Profildaten für den Benutzer aufgerufen werden soll. Sie sollten einen Delegaten (oder Funktionsverweis) erhalten.

Der von dieser Eigenschaft angegebenen Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| numPropsSaved | Gibt die Anzahl von Eigenschaften, die gespeichert wurden. |
| userContext | Gibt an, die Benutzerkontextinformationen bereitgestellt, wenn das Laden oder Speichern der Funktion aufgerufen wurde. |
| methodName | Der Name der Aufrufmethode. |

*DefaultLoadCompleted-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion, die nach dem Abschluss des Ladevorgangs von Profildaten für den Benutzer aufgerufen werden soll. Sie sollten einen Delegaten (oder Funktionsverweis) erhalten.

Der von dieser Eigenschaft angegebenen Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| numPropsLoaded | Gibt die Anzahl von Eigenschaften geladen werden. |
| userContext | Gibt an, die Benutzerkontextinformationen bereitgestellt, wenn das Laden oder Speichern der Funktion aufgerufen wurde. |
| methodName | Der Name der Aufrufmethode. |

*Path-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt programmgesteuert den Speicherort des Webdiensts Profil. Es kann verwendet werden, um der Dienstanbieter für Standard-Profil als auch eine überschreiben deklarativ in der Path-Eigenschaft des ScriptManager-Steuerelement ProfileService untergeordneter Knoten festgelegt.

Beachten Sie, dass der Speicherort des Diensts Profil standardmäßig nicht ändert. Allerdings können mit ASP.NET AJAX den Speicherort eines Webdiensts angeben, die die gleichen Klassenschnittstelle als ASP.NET AJAX-Proxys für die Authentifizierung Dienst bereitstellt.

Beachten Sie außerdem, dass diese Eigenschaft nicht auf einen Wert festgelegt werden soll, die aus dem aktuellen Standort die Skript-Anforderung weiterleitet. Die zugrunde liegende Technologie AJAX Cross-Site-Anforderungen sollten nicht gesendet und eine Sicherheitsausnahme im Clientbrowser generieren.

Diese Eigenschaft ist ein `String` Objekt, das den Pfad mit dem Webdienst Profil darstellt.

*Timeout-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt, dass die Länge der Zeit Profildienst warten, bevor Sie die Last vorausgesetzt, oder speichern die Anforderung fehlgeschlagen ist. Wenn es sich bei Ablauf des Timeouts beim Warten auf eines Aufrufs abgeschlossen wird der Rückruf Fehler bei der Anforderung aufgerufen werden, und der Aufruf nicht abgeschlossen.

Diese Eigenschaft ist ein `Number` Objekt, das die Anzahl der Millisekunden zu warten, bis Ergebnisse aus dem Profildienst darstellt.

*Codebeispiel: Laden von Profildaten beim Laden von Seiten*

Der folgende Code überprüft, ob ein Benutzer authentifiziert wurde, und wenn dies der Fall ist, wird bevorzugte Background-Farbe für den Benutzer als die Seite geladen.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Einen Dienstanbieter für die benutzerdefinierte Authentifizierung verwenden*

Die ASP.NET AJAX-Erweiterungen können Sie einen benutzerdefiniertes Skript-Dienstanbieter zu erstellen, indem Sie Ihre Funktionalität über eine benutzerdefinierte Webdienst verfügbar gemacht. Um verwendet werden, der Webdienst verfügbar machen zwei Methoden `Login` und `Logout`; und die gleiche Methodensignaturen wie der standardmäßige Authentifizierung in ASP.NET AJAX-Webdienst dieser Methoden angegeben werden.

Nachdem Sie die benutzerdefinierte Webdienst erstellt haben, müssen Sie den Pfad, und entweder deklarativ auf der Seite programmgesteuert im Code oder über das Clientskript angeben.

*Um den Pfad deklarativ festzulegen:*

Schließen Sie den Pfad deklarativ festlegen, das AuthenticationService untergeordnete Element des Objekts auf der ASP.NET-Seite ScriptManager:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*So legen den Pfad in Code:*

Geben Sie den Pfad über die Instanz des Skript-Manager, um den Pfad programmgesteuert festzulegen:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*So legen Sie den Pfad im Skript fest*

Um den Pfad in Skript programmgesteuert festzulegen, verwenden die `path` Eigenschaft der Klasse AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Der Beispielwebdienst für die benutzerdefinierte Authentifizierung*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Zusammenfassung

ASP.NET-Dienste - speziell die profilerstellung, Mitgliedschaft und Authentifizierung-Dienste - werden einfach JavaScript im Browser Clients verfügbar gemacht. Dadurch können Entwickler Codeprobleme clientseitige Authentifizierungsmechanismus nahtlos integrieren ohne Steuerelemente, z. B. UpdatePanels möchten die anstrengende abhängig. Profildaten können vom Client als auch geschützt werden, durch Nutzung von Web-Konfigurationseinstellungen; keine Daten ist standardmäßig verfügbar und Entwickler müssen opt-in für Profileigenschaften.

Darüber hinaus können Entwickler durch vereinfachte Web Service Implementierungen mit entsprechenden Methodensignaturen erstellen, CustomScript-Anbieter für diese systeminternen ASP.NET-Dienste erstellen. Unterstützung für diese Techniken vereinfacht die Entwicklung rich Client-Anwendungen beim bietet Entwicklern einer breiten Palette von Flexibilität, bestimmte Anforderungen erfüllen.

## <a name="bio"></a>*Bio*

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und Präsidenten des myKB.com ist ([www.myKB.com](http://www.myKB.com)), in dem er zum Schreiben von ASP.NET spezialisiert-basierten Anwendungen, die Wissensdatenbank softwarelösungen konzentriert. Scott hergestellt werden kann, per e-Mail an [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Weiter](understanding-asp-net-ajax-localization.md)
