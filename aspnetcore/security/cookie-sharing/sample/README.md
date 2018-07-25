# <a name="cookie-sharing-sample-app"></a>Cookie-Beispiel-App freigeben

Das Beispiel veranschaulicht die Cookies, die Freigabe über drei apps, die Cookie-Authentifizierung zu verwenden:

| Projekt                             | Beschreibung |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core-Razor-Seiten-app ohne ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | ASP.NET Core MVC-app mit ASP.NET Core-Identität |
| CookieAuthWithIdentity.NETFramework | ASP.NET Framework-MVC-app mit ASP.NET Identity |

Anweisungen:

1. Führen Sie die CookieAuth.Core-app. Registrieren Sie einen Benutzer. Die app authentifiziert den Benutzer auf, wenn der Benutzer registriert ist. Abmelden des Benutzers.
1. Führen Sie in der gleichen Browsersitzung die CookieAuthWithIdentity.Core-app ein. Registrieren Sie demselben Benutzer, als für die Core-app verwendet. Die app authentifiziert den Benutzer auf, wenn der Benutzer registriert ist. Abmelden des Benutzers.
1. Führen Sie in der gleichen Browsersitzung die CookieAuthWithIdentity.NETFramework-app ein. Registrieren Sie demselben Benutzer wie mit anderen apps verwendet. Die app authentifiziert den Benutzer auf, wenn der Benutzer registriert ist. Abmelden des Benutzers.
1. Melden Sie sich der Benutzer auf eine der drei apps. Das Authentifizierungscookie wird von den apps gemeinsam genutzt. Beachten Sie, dass der Benutzer automatisch in die anderen beiden apps angemeldet ist.
1. Melden Sie sich der Benutzer aus einer beliebigen App. Beachten Sie, dass der Benutzer die anderen zwei apps automatisch abgemeldet wird.

In diesem Beispiel wird veranschaulicht, die in beschriebenen Features der [Freigeben von Cookies zwischen apps](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) Thema.
