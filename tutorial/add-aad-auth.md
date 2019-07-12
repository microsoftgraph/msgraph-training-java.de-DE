---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634745"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9741f-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="9741f-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="9741f-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="9741f-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="9741f-103">In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="9741f-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

<span data-ttu-id="9741f-104">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/com/Contoso** -Verzeichnis mit dem Namen **oAuth. Properties**.</span><span class="sxs-lookup"><span data-stu-id="9741f-104">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **oAuth.properties**.</span></span> <span data-ttu-id="9741f-105">Fügen Sie den folgenden Text in die Datei ein.</span><span class="sxs-lookup"><span data-stu-id="9741f-105">Add the following text in that file.</span></span>

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

<span data-ttu-id="9741f-106">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID, die Sie im Azure-Portal erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="9741f-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9741f-107">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei **oAuth. Properties** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="9741f-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="9741f-108">Öffnen Sie **app. Java** , und fügen `import` Sie die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="9741f-108">Open **App.java** and add the following `import` statements.</span></span>

```java
import java.io.IOException;
import java.util.Properties;
```

<span data-ttu-id="9741f-109">Fügen Sie dann den folgenden Code direkt vor `Scanner input = new Scanner(System.in);` der Codezeile ein, um die Datei **oAuth. Properties** zu laden.</span><span class="sxs-lookup"><span data-stu-id="9741f-109">Then add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

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

## <a name="implement-sign-in"></a><span data-ttu-id="9741f-110">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="9741f-110">Implement sign-in</span></span>

<span data-ttu-id="9741f-111">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/com/Contoso** -Verzeichnis mit dem Namen **Authentication. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="9741f-111">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Authentication.java** and add the following code.</span></span>

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

<span data-ttu-id="9741f-112">Fügen Sie in **app. Java**den folgenden Code unmittelbar vor der `Scanner input = new Scanner(System.in);` Codezeile ein, um ein Zugriffstoken abzurufen.</span><span class="sxs-lookup"><span data-stu-id="9741f-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

<span data-ttu-id="9741f-113">Fügen Sie dann die folgende Reihe nach `// Display access token` dem Kommentar hinzu.</span><span class="sxs-lookup"><span data-stu-id="9741f-113">Then add the following line after the `// Display access token` comment.</span></span>

```java
System.out.println("Access token: " + accessToken);
```

<span data-ttu-id="9741f-114">Erstellen Sie die APP, und führen Sie Sie aus.</span><span class="sxs-lookup"><span data-stu-id="9741f-114">Build and run the app.</span></span> <span data-ttu-id="9741f-115">Die Anwendung zeigt eine URL und einen Gerätecode an.</span><span class="sxs-lookup"><span data-stu-id="9741f-115">The application displays a URL and device code.</span></span>

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

<span data-ttu-id="9741f-116">Öffnen Sie einen Browser, und navigieren Sie zu der angezeigten URL.</span><span class="sxs-lookup"><span data-stu-id="9741f-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="9741f-117">Geben Sie den bereitgestellten Code ein und melden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="9741f-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="9741f-118">Kehren Sie nach Abschluss zurück zur Anwendung, und wählen Sie die **1 aus. Option Zugriffstoken anzeigen** , um das Zugriffstoken anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9741f-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
