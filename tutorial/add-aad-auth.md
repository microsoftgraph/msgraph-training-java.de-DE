---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634745"
---
<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten. In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) in die Anwendung integrieren.

Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/com/Contoso** -Verzeichnis mit dem Namen **oAuth. Properties**. Fügen Sie den folgenden Text in die Datei ein.

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID, die Sie im Azure-Portal erstellt haben.

> [!IMPORTANT]
> Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei **oAuth. Properties** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.

Öffnen Sie **app. Java** , und fügen `import` Sie die folgenden Anweisungen hinzu.

```java
import java.io.IOException;
import java.util.Properties;
```

Fügen Sie dann den folgenden Code direkt vor `Scanner input = new Scanner(System.in);` der Codezeile ein, um die Datei **oAuth. Properties** zu laden.

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/com/Contoso** -Verzeichnis mit dem Namen **Authentication. Java** , und fügen Sie den folgenden Code hinzu.

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/organizations/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

Fügen Sie in **app. Java**den folgenden Code unmittelbar vor der `Scanner input = new Scanner(System.in);` Codezeile ein, um ein Zugriffstoken abzurufen.

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

Fügen Sie dann die folgende Reihe nach `// Display access token` dem Kommentar hinzu.

```java
System.out.println("Access token: " + accessToken);
```

Erstellen Sie die APP, und führen Sie Sie aus. Die Anwendung zeigt eine URL und einen Gerätecode an.

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

Öffnen Sie einen Browser, und navigieren Sie zu der angezeigten URL. Geben Sie den bereitgestellten Code ein und melden Sie sich an. Kehren Sie nach Abschluss zurück zur Anwendung, und wählen Sie die **1 aus. Option Zugriffstoken anzeigen** , um das Zugriffstoken anzuzeigen.
