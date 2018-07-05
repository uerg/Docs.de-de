---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Grundlegendes zur Authentifizierung in ASP.NET AJAX und-Profilanwendungsdienste | Microsoft-Dokumentation
author: scottcate
description: Der Authentifizierungsdienst ermöglicht es Benutzern, Anmeldeinformationen anzugeben, um ein Authentifizierungscookie zu erhalten, und der Gatewaydienst kann benutzerdefinierte geht an...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 6c08cffacb9ebde6f29398f53b2e568b4bd59d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831679"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Grundlegendes zur Authentifizierung in ASP.NET AJAX und-Profilanwendungsdienste
====================
durch [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Der Authentifizierungsdienst ermöglicht Benutzern, Anmeldeinformationen anzugeben, um ein Authentifizierungscookie zu erhalten, und ist der Gatewaydienst benutzerdefinierten Profilen können, die von ASP.NET bereitgestellt. Verwenden des Authentifizierungsdiensts ASP.NET AJAX ist bei der standardmäßigen ASP.NET-Formularauthentifizierung kompatibel, sodass es sich bei Anwendungen, die derzeit unter Verwendung der Formularauthentifizierung (z. B. steuern, mit der Anmeldung) nicht umbrochen wird durch ein Upgrade auf die AJAX-Dienst-Authentifizierung.


## <a name="introduction"></a>Einführung

Als Teil von .NET Framework 3.5 stellt Microsoft ein Upgrade beträchtliche Umgebung bereit; nicht nur eine neue Entwicklungsumgebung verfügbar ist, sondern Verknüpfungsfunktionen sind die neuen Funktionen von Language-Integrated Query (LINQ) und andere Verbesserungen der Sprache. Darüber hinaus sind einige bekannten Funktionen von anderen Toolsets, insbesondere die ASP.NET AJAX Extensions können als wesentliche Bestandteile von .NET Framework-Basisklassenbibliothek enthalten wird. Mithilfe dieser Erweiterungen können viele neue rich-Client-Features, darunter Teilrendering von Seiten ohne einer vollständigen seitenaktualisierung, den Zugriff auf Webdienste über Clientskript (einschließlich ASP.NET profilerstellungs-API) und eine umfangreiche clientseitige API entwickelt, um viele der Steuerelement-Schemas finden Sie in den Satz der ASP.NET serverseitigen Steuerelements zu spiegeln.

In diesem Whitepaper untersucht die die Implementierung und Verwendung der profilerstellung von ASP.NET und Formularauthentifizierung, Diensten, wie sie von den Microsoft ASP.NET AJAX ExtensionsThe AJAX-Erweiterungen verfügbar gemacht werden Stellen Formular-Authentifizierung unterstützt, da es unglaublich einfach (sowie der Dienst die Profilerstellung) wird über ein Webdienst-Proxy-Skript verfügbar gemacht. Die AJAX-Erweiterungen unterstützen auch die benutzerdefinierten Authentifizierung über die AuthenticationServiceManager-Klasse.

In diesem Whitepaper basiert auf der Beta-2-Version von Visual Studio 2008 und .NET Framework 3.5. In diesem Whitepaper wird vorausgesetzt, dass Sie mit Visual Studio 2008 Beta 2 nicht Visual Web Developer Express arbeiten werden und exemplarische Vorgehensweisen, gemäß der Benutzeroberfläche von Visual Studio bieten. Einige Codebeispiele möglicherweise nutzen Sie Projektvorlagen in Visual Web Developer Express nicht verfügbar.

## <a name="profiles-and-authentication"></a>*Profile und Authentifizierung*

Die Microsoft ASP.NET-Profile und Authentifizierungsdienste von der ASP.NET-Formularauthentifizierung System bereitgestellt werden und sind Standardkomponenten von ASP.NET. ASP.NET AJAX Extensions bieten Zugriff auf Skripts mit diesen Diensten über Skriptproxys, über ein Modell relativ einfach, unter dem Sys.Services-Namespace, der AJAX-Clientbibliothek.

Der Authentifizierungsdienst ermöglicht Benutzern, Anmeldeinformationen anzugeben, um ein Authentifizierungscookie zu erhalten, und ist der Gatewaydienst benutzerdefinierten Profilen können, die von ASP.NET bereitgestellt. Verwenden des Authentifizierungsdiensts ASP.NET AJAX ist bei der standardmäßigen ASP.NET-Formularauthentifizierung kompatibel, sodass es sich bei Anwendungen, die derzeit unter Verwendung der Formularauthentifizierung (z. B. steuern, mit der Anmeldung) nicht umbrochen wird durch ein Upgrade auf die AJAX-Dienst-Authentifizierung.

Der Profil-Dienst ermöglicht die automatische Integration und die Speicherung von Benutzerdaten, die basierend auf der Mitgliedschaft, gemäß Bereitstellung durch den Authentifizierungsdienst. Die gespeicherten Daten werden durch die Datei "Web.config" angegeben, und der profilerstellung Dienstanbieter behandeln, die datenverwaltung. Wie bei den Authentifizierungsdienst, ist der AJAX-Profildienst kompatibel mit dem standardmäßigen ASP.NET Profile-Dienst, sodass Seiten, die derzeit-spezifische Funktionen des ASP.NET Profile-Diensts nicht unterteilt werden sollte, durch die AJAX-Unterstützung.

Integrieren die Authentifizierung in ASP.NET und Profilerstellung von Diensten selbst in eine Anwendung ist nicht im Rahmen dieses Whitepapers. Weitere Informationen zu diesem Thema finden Sie unter der MSDN-Bibliothek verweisen auf Artikel Verwalten von Benutzern durch Mitgliedschaft auf [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET enthält auch ein Hilfsprogramm zum automatisch Mitgliedschaft bei einer SQL-Server einrichten, die der Dienstanbieter des Standard-Authentifizierung für die ASP.NET-Mitgliedschaft ist. Weitere Informationen finden Sie im Artikel ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe) am [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Verwenden des Authentifizierungsdiensts für ASP.NET AJAX*

Der Authentifizierung in ASP.NET AJAX-Dienst muss in der Datei "Web.config" aktiviert werden:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Der Authentifizierungsdienst erfordert ASP.NET-Formularauthentifizierung aktiviert werden und Cookies im Browser Clients aktiviert werden (ein Skript kann keine Sitzung ohne Cookies aktivieren, da für Sitzungen ohne Cookies URL-Parameter erforderlich ist).

Sobald die AJAX-Dienst-Authentifizierung aktiviert und konfiguriert ist, kann Clientskript sofort die Sys.Services.AuthenticationService-Objekts nutzen. In erster Linie dem Clientskript nutzen sollten die `login` Methode und `isLoggedIn` Eigenschaft. Mehrere Eigenschaften vorhanden sind, um Standardwerte für die der Login-Methode bereitzustellen, die eine große Anzahl von Parametern annehmen kann.

*Sys.Services.AuthenticationService-Elemente*

*Login-Methode:*

Die Login()-Methode startet eine Anforderung an die Anmeldeinformationen des Benutzers zu authentifizieren. Diese Methode ist asynchron und Ausführung nicht blockiert.

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| userName | Erforderlich. Die zu authentifizierende Benutzername. |
| Kennwort | Optional (Standardwert ist null). Das Kennwort des Benutzers. |
| isPersistent | Optional (standardmäßig "false"). Gibt an, ob Cookie für die Authentifizierung des Benutzers über Sitzungen hinweg beibehalten werden sollen. False gibt an, wird der Benutzer abmelden, wenn der Browser geschlossen wird oder die Sitzung abläuft. |
| redirectUrl | Optional (Standardwert ist null). Die URL, an den Browser nach erfolgreicher Authentifizierung umgeleitet werden soll. Wenn dieser Parameter Null oder eine leere Zeichenfolge ist, tritt keine Umleitung. |
| customInfo | Optional (Standardwert ist null). Dieser Parameter wird derzeit nicht verwendet und ist für die zukünftige Verwendung reserviert. |
| loginCompletedCallback | Optional (Standardwert ist null). Die Funktion, die aufgerufen werden, wenn die Anmeldung erfolgreich abgeschlossen wurde. Wenn angegeben, überschreibt dieser Parameter die DefaultLoginCompleted-Eigenschaft. |
| failedCallback | Optional (Standardwert ist null). Die Funktion aufrufen, wenn die Anmeldung fehlgeschlagen ist. Wenn angegeben, überschreibt dieser Parameter die DefaultFailedCallback-Eigenschaft. |
| userContext | Optional (Standardwert ist null). Benutzerdefinierte Kontextdaten, die an die Rückruffunktionen übergeben werden sollen. |

*Rückgabewert:*

Diese Funktion umfasst keinen Wert zurückgegeben. Es gibt jedoch eine Reihe von Verhaltensweisen nach Abschluss eines Aufrufs dieser Funktion enthalten:

- Die aktuelle Seite wird aktualisiert werden oder geändert werden, wenn die `redirectUrl` Parameter war weder Null noch eine leere Zeichenfolge.
- Jedoch, wenn der Parameter Null oder eine leere Zeichenfolge ist, wurde die `loginCompletedCallback` -Parameter oder `defaultLoginCompletedCallback` -Eigenschaft aufgerufen wird.
- Wenn der Aufruf an den Webdienst ein Fehler auftritt, die `failedCallback` Parameter `defaultFailedCallback` -Eigenschaft aufgerufen wird.

*Logout-Methode:*

Die logout()-Methode entfernt das Cookie für die Anmeldeinformationen und meldet den aktuellen Benutzer aus der Webanwendung.

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| redirectUrl | Optional (Standardwert ist null). Die URL, an den Browser nach erfolgreicher Authentifizierung umgeleitet werden soll. Wenn dieser Parameter Null oder eine leere Zeichenfolge ist, tritt keine Umleitung. |
| logoutCompletedCallback | Optional (Standardwert ist null). Die Funktion, die aufgerufen werden, wenn die Abmeldung erfolgreich abgeschlossen wurde. Wenn angegeben, überschreibt dieser Parameter die DefaultLogoutCompleted-Eigenschaft. |
| failedCallback | Optional (Standardwert ist null). Die Funktion aufrufen, wenn die Anmeldung fehlgeschlagen ist. Wenn angegeben, überschreibt dieser Parameter die DefaultFailedCallback-Eigenschaft. |
| userContext | Optional (Standardwert ist null). Benutzerdefinierte Kontextdaten, die an die Rückruffunktionen übergeben werden sollen. |

*Rückgabewert:*

Diese Funktion umfasst keinen Wert zurückgegeben. Es gibt jedoch eine Reihe von Verhaltensweisen nach Abschluss eines Aufrufs dieser Funktion enthalten:

- Die aktuelle Seite wird aktualisiert werden oder geändert werden, wenn die `redirectUrl` Parameter war weder Null noch eine leere Zeichenfolge.
- Jedoch, wenn der Parameter Null oder eine leere Zeichenfolge ist, wurde die `logoutCompletedCallback` -Parameter oder `defaultLogoutCompletedCallback` -Eigenschaft aufgerufen wird.
- Wenn der Aufruf an den Webdienst ein Fehler auftritt, die `failedCallback` Parameter `defaultFailedCallback` -Eigenschaft aufgerufen wird.

*DefaultFailedCallback-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, wenn ein Fehler für die Kommunikation mit dem Webdienst auftritt. Sie sollten einen Delegaten (oder -Funktionsreferenz) erhalten.

Der von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur besitzen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| error | Gibt die Fehlerinformationen an. |
| userContext | Gibt an, die Benutzerkontextinformationen, die bereitgestellt werden, wenn die an- bzw. Abmeldung-Funktion aufgerufen wurde. |
| methodName | Der Name der aufrufenden Methode. |

*DefaultLoginCompletedCallback-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, bei der Anmeldung Webdienstaufruf abgeschlossen ist. Sie sollten einen Delegaten (oder -Funktionsreferenz) erhalten.

Der von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur besitzen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| validCredentials | Gibt an, ob der Benutzer gültige Anmeldeinformationen angegeben. `true` Wenn der Benutzer erfolgreich angemeldet hat; andernfalls `false`. |
| userContext | Gibt an, die Benutzerkontextinformationen, die bereitgestellt werden, wenn die "Login"-Funktion aufgerufen wurde. |
| methodName | Der Name der aufrufenden Methode. |

*DefaultLogoutCompletedCallback-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, bei der Abmeldung Webdienstaufruf abgeschlossen ist. Sie sollten einen Delegaten (oder -Funktionsreferenz) erhalten.

Der von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur besitzen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| Ergebnis | Dieser Parameter ist immer, `null`; es ist für die zukünftige Verwendung reserviert. |
| userContext | Gibt an, die Benutzerkontextinformationen, die bereitgestellt werden, wenn die "Login"-Funktion aufgerufen wurde. |
| methodName | Der Name der aufrufenden Methode. |

*IsLoggedIn-Eigenschaft (Get):*

Diese Eigenschaft ruft den aktuellen Authentifizierungsstatus des Benutzers ab. Es wird durch das ScriptManager-Objekt während einer Seitenanforderung festgelegt.

Diese Eigenschaft gibt `true` , wenn der Benutzer zurzeit andernfalls angemeldet ist, gibt Sie zurück `false`.

*Path-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft bestimmt programmgesteuert den Speicherort des Authentifizierungsdiensts Web. Es kann verwendet werden, um den Standardanbieter für die Authentifizierung als auch eine außer Kraft setzen deklarativ festgelegt werden, in der Path-Eigenschaft des ScriptManager-Steuerelements AuthenticationService untergeordneten Knotens (Weitere Informationen finden Sie unter den einen benutzerdefinierten Dienst Authentifizierungsanbieter In diesem Thema weiter unten).

Beachten Sie, dass der Speicherort des Authentifizierungsdiensts standardmäßig nicht ändert. Sie können allerdings ASP.NET AJAX an den Speicherort eines Webdiensts, der die gleiche Klassenschnittstelle, wie ASP.NET AJAX-Proxys für die Authentifizierung Dienst bereitstellt.

Beachten Sie, dass diese Eigenschaft nicht auf einen Wert festgelegt werden soll, die die skriptanforderung aus dem aktuellen Standort weiterleitet. Da die aktuelle Anwendung keine Anmeldeinformationen für die Authentifizierung erhalten würde, wäre es nutzlos; Darüber hinaus die zugrunde liegende Technologie AJAX siteübergreifenden Anforderungen sollte nicht gesendet, und generiert möglicherweise im Clientbrowser eine Sicherheitsausnahme ausgelöst.

Diese Eigenschaft ist eine `String` Objekt, das den Pfad an den Webdienst für die Authentifizierung darstellt.

*Timeout-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft bestimmt, dass die Dauer für den Authentifizierungsdienst zu warten, bevor die Annahme der Anforderung der Anmeldung ein Fehler aufgetreten ist. Wenn es sich bei Ablauf des Timeouts beim Warten auf eines Aufrufs zum Abschließen der Anforderungsfehler Rückruf wird aufgerufen, und der Aufruf wird nicht abgeschlossen werden.

Diese Eigenschaft ist eine `Number` Objekt, das die Anzahl der Millisekunden warten, bis Ergebnisse vom Authentifizierungsdienst darstellt.

*Codebeispiel: Anmeldung über den Authentifizierungsdienst*

Das folgende Markup ist ein Beispiel ASP.NET mit einem einfachen Skriptaufruf die Anmelde- und Abmeldeereignisse Methoden der AuthenticationService-Klasse.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Zugreifen auf Daten über AJAX-ASP.NET-PROFILERSTELLUNG

Die ASP.NET-PROFILERSTELLUNG-Dienst ist auch über ASP.NET AJAX Extensions verfügbar gemacht. Da der Profilerstellungsdienst ASP.NET eine umfangreiche, differenzierte API nach dem Speichern und Abrufen von Benutzerdaten bietet, kann dies eine hervorragende Produktivitätstool sein.

Der Profildienst muss in "Web.config" aktiviert sein. Es ist nicht standardmäßig. Zu diesem Zweck sicher, dass die `profileService` untergeordnetes Element wurde aktiviert = "true" in angegebene Datei "Web.config", und Sie angegeben haben, welche Eigenschaften gelesen oder wie folgt geschrieben werden können:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Der Profildienst muss auch konfiguriert werden. Konfiguration des profilerstellungs-Diensts nicht im Rahmen dieses Whitepapers ist, zwar lohnt es sich, beachten Sie, dass Gruppen gemäß Konfiguration der profileinstellungen als untergeordnete Eigenschaften den Namen zugegriffen werden kann. Z. B. mit dem folgenden Profilabschnitt, die angegeben wird:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Clientskript wäre auf Name, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip und BackgroundColor als Eigenschaften das Feld "Properties" der ProfileService-Klasse zugreifen.

Sobald die Profilerstellung für AJAX-Dienst konfiguriert ist, wird es jedoch sofort in Seiten verfügbar sein. Sie müssen jedoch einmal vor der Verwendung geladen werden.

*Sys.Services.ProfileService Mitglieder*

*Feld "Properties":*

Das Feld "Properties" gibt alle konfigurierten Profildaten als untergeordneten Eigenschaften, die mit Punkt-Operator-Namenskonvention verwiesen werden können. Eigenschaften, die untergeordnete Elemente von Eigenschaftsgruppen werden als GroupName.PropertyName bezeichnet. In der oben aufgeführten Beispiel-Profilkonfiguration könnten Sie zum Abrufen des Status des Benutzers, den folgenden Bezeichner verwenden:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Load-Methode:*

Lädt eine Liste mit ausgewählten oder allen Eigenschaften vom Server an.

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| propertyNames | Optional (Standardwert ist null). Die Eigenschaften, die vom Server geladen werden. |
| loadCompletedCallback | Optional (Standardwert ist null). Die Funktion, die aufgerufen werden, wenn der Ladevorgang abgeschlossen ist. |
| failedCallback | Optional (Standardwert ist null). Die Funktion aufrufen, wenn ein Fehler auftritt. |
| userContext | Optional (Standardwert ist null). Die Kontextinformationen, die an die Callback-Funktion übergeben werden. |

Die Load-Funktion muss keinen Wert zurückgegeben. Wenn der Aufruf erfolgreich war, sie rufen Sie entweder wird die `loadCompletedCallback` Parameter oder `defaultLoadCompletedCallback` Eigenschaft. Wenn der Aufruf fehlgeschlagen ist oder das Timeout ist, entweder abgelaufen die `failedCallback` Parameter oder `defaultFailedCallback` Eigenschaft aufgerufen wird.

Wenn die `propertyNames` Parameter nicht angegeben wird, werden alle lesen konfigurierte Eigenschaften vom Server abgerufen.

*Save-Methode:*

Die Save() speichert die angegebenen Eigenschaftenliste (oder alle Eigenschaften), in das Profil des Benutzers ASP.NET.

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| propertyNames | Optional (Standardwert ist null). Die Eigenschaften, die auf dem Server gespeichert werden. |
| saveCompletedCallback | Optional (Standardwert ist null). Die Funktion zum Aufrufen der beim Speichern abgeschlossen wurde. |
| failedCallback | Optional (Standardwert ist null). Die Funktion aufrufen, wenn ein Fehler auftritt. |
| userContext | Optional (Standardwert ist null). Die Kontextinformationen, die an die Callback-Funktion übergeben werden. |

Der Speichervorgang Funktion hat keinen Rückgabewert. Wenn der Aufruf erfolgreich ausgeführt wird, ruft sie entweder die `saveCompletedCallback` Parameter oder `defaultSaveCompletedCallback` Eigenschaft. Wenn der Aufruf fehlgeschlagen ist oder das Timeout ist, entweder abgelaufen die `failedCallback` oder `defaultFailedCallback` Eigenschaft aufgerufen wird.

Wenn die `propertyNames` Parameter null ist, alle Profileigenschaften werden an den Server gesendet werden und der Server entscheidet, welche Eigenschaften gespeichert werden können und welche nicht.

*DefaultFailedCallback-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft gibt eine Funktion, die aufgerufen werden soll, wenn ein Fehler für die Kommunikation mit dem Webdienst auftritt. Sie sollten einen Delegaten (oder -Funktionsreferenz) erhalten.

Der von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur besitzen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| Fehler | Gibt die Fehlerinformationen an. |
| userContext | Gibt an, die Benutzerkontextinformationen bereitgestellt, wenn das Laden oder Speichern der Funktion aufgerufen wurde. |
| methodName | Der Name der aufrufenden Methode. |

*DefaultSaveCompleted-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft gibt eine Funktion, die nach dem Abschluss Speichern der Profildaten des Benutzers aufgerufen werden soll. Sie sollten einen Delegaten (oder -Funktionsreferenz) erhalten.

Der von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur besitzen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| numPropsSaved | Gibt die Anzahl der Eigenschaften, die gespeichert wurden. |
| userContext | Gibt an, die Benutzerkontextinformationen bereitgestellt, wenn das Laden oder Speichern der Funktion aufgerufen wurde. |
| methodName | Der Name der aufrufenden Methode. |

*DefaultLoadCompleted-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft gibt eine Funktion, die nach dem Abschluss des Ladevorgangs von Profildaten des Benutzers aufgerufen werden soll. Sie sollten einen Delegaten (oder -Funktionsreferenz) erhalten.

Der von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur besitzen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parameter:*

| **Name des Parameters** | **Bedeutung** |
| --- | --- |
| numPropsLoaded | Gibt die Anzahl von Eigenschaften geladen. |
| userContext | Gibt an, die Benutzerkontextinformationen bereitgestellt, wenn das Laden oder Speichern der Funktion aufgerufen wurde. |
| methodName | Der Name der aufrufenden Methode. |

*Path-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft bestimmt programmgesteuert den Speicherort des Webdiensts Profil. Sie können verwendet werden, um den Standardprofilanbieter für die Service als auch eine außer Kraft setzen deklarativ in der Path-Eigenschaft des ScriptManager-Steuerelements ProfileService untergeordneten Knoten festgelegt.

Beachten Sie, dass der Speicherort der Standardprofildienst nicht ändert. Sie können allerdings ASP.NET AJAX an den Speicherort eines Webdiensts, der die gleiche Klassenschnittstelle, wie ASP.NET AJAX-Proxys für die Authentifizierung Dienst bereitstellt.

Beachten Sie, dass diese Eigenschaft nicht auf einen Wert festgelegt werden soll, die die skriptanforderung aus dem aktuellen Standort weiterleitet. Die zugrunde liegende Technologie AJAX siteübergreifenden Anforderungen sollte nicht gesendet, und generiert möglicherweise im Clientbrowser eine Sicherheitsausnahme ausgelöst.

Diese Eigenschaft ist eine `String` Objekt, das den Pfad an den Profil-Webdienst darstellt.

*Timeout-Eigenschaft ("Get", "Set"):*

Diese Eigenschaft bestimmt, dass die Zeitspanne bis zum für den Profildienst vorausgesetzt, die Last oder speichern die Anforderung fehlgeschlagen ist. Wenn es sich bei Ablauf des Timeouts beim Warten auf eines Aufrufs zum Abschließen der Anforderungsfehler Rückruf wird aufgerufen, und der Aufruf wird nicht abgeschlossen werden.

Diese Eigenschaft ist eine `Number` Objekt, das die Anzahl der Millisekunden warten, bis Ergebnisse aus den Profildienst darstellt.

*Codebeispiel: Laden von Profildaten beim Laden der Seite*

Der folgende Code überprüft, ob ein Benutzer authentifiziert wurde, und wenn dies der Fall ist, lädt bevorzugte Hintergrundfarbe des Benutzers als der Seite.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Unter Verwendung eines Dienstanbieters für die benutzerdefinierte Authentifizierung*

ASP.NET AJAX Extensions können Sie einen benutzerdefiniertes Skript-Dienstanbieter zu erstellen, indem Sie die Funktionalität über einen benutzerdefinierten Webdienst bereitstellen. Um verwendet werden, muss Ihr Webdienst verfügbar zwei Methoden, machen `Login` und `Logout`; diese Methoden müssen mit den gleichen Methodensignaturen, wie der Standard-Authentifizierung in ASP.NET AJAX-Webdienst angegeben werden.

Nachdem Sie den benutzerdefinierten Webserver-Dienst erstellt haben, müssen Sie den Pfad dahin, entweder deklarativ auf der Seite Geben Sie im Code programmgesteuert oder über das Clientskript.

*Den Pfad deklarativ festgelegt werden:*

Damit den Pfad deklarativ, schließen Sie das AuthenticationService untergeordnete Element des Objekts ScriptManager auf der ASP.NET-Seite:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*So legen den Pfad in Code:*

Um den Pfad programmgesteuert festzulegen, geben Sie den Pfad über die Instanz von Ihr Skript-Manager:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*So legen Sie den Pfad im Skript fest:*

Verwenden Sie zum programmgesteuerten Festlegen den Pfad im Skript die `path` -Eigenschaft der Klasse AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Beispiel-Webdiensts für die benutzerdefinierte Authentifizierung*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Zusammenfassung

ASP.NET-Dienste – insbesondere die profilerstellung, Mitgliedschaft und Authentifizierung-Dienste – werden problemlos JavaScript im Browser Clients verfügbar gemacht. Dadurch können Entwickler ihren clientseitigen Code mit der Authentifizierungsmechanismus nahtlos integrieren, ohne je nach Steuerelementen wie z. B. UpdatePanels, die die Hauptarbeit überlassen. Profildaten können vom Client ebenfalls geschützt werden, durch die Verwendung von Web-Konfigurationseinstellungen; keine Daten standardmäßig verfügbar sind, und Entwickler müssen sich auf Profileigenschaften.

Darüber hinaus können Entwickler vereinfachte Web dienstimplementierungen mit entsprechenden Methodensignaturen erstellen, benutzerdefinierte Skript-Anbieter für diese systeminternen ASP.NET-Dienste erstellen. Unterstützung für diese Techniken vereinfacht die Entwicklung rich Client-Anwendungen und gleichzeitig den Entwicklern ein breites Spektrum an Flexibilität, bestimmte Anforderungen zu erfüllen.

## <a name="bio"></a>*Biografie*

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und ist Vorsitzender der myKB.com ([www.myKB.com](http://www.myKB.com)), in dem er spezialisiert sich auf das Schreiben von ASP.NET basierende Anwendungen, die mit dem Schwerpunkt Knowledge Base-softwarelösungen. Scott hergestellt werden kann, per e-Mail unter [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Weiter](understanding-asp-net-ajax-localization.md)
