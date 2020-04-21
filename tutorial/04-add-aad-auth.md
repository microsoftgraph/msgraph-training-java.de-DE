---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612061"
---
<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten. In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) in die Anwendung integrieren.

1. Erstellen Sie im Verzeichnis **./src/main/resources** ein neues Verzeichnis mit dem Namen **graphtutorial** .

1. Erstellen Sie eine neue Datei im Verzeichnis **./src/main/resources/graphtutorial** mit dem Namen **oAuth. Properties**, und fügen Sie den folgenden Text in die Datei ein.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID, die Sie im Azure-Portal erstellt haben.

    > [!IMPORTANT]
    > Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei **oAuth. Properties** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.

1. Öffnen Sie **app. Java** , und fügen `import` Sie die folgenden Anweisungen hinzu.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Fügen Sie den folgenden Code direkt vor `Scanner input = new Scanner(System.in);` der Codezeile ein, um die Datei **oAuth. Properties** zu laden.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

1. Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **Authentication. Java** , und fügen Sie den folgenden Code hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. Fügen Sie in **app. Java**den folgenden Code unmittelbar vor der `Scanner input = new Scanner(System.in);` Codezeile ein, um ein Zugriffstoken abzurufen.

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. Fügen Sie die folgende Reihe nach `// Display access token` dem Kommentar hinzu.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Führen Sie die App aus. Die Anwendung zeigt eine URL und einen Gerätecode an.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Öffnen Sie einen Browser, und navigieren Sie zu der angezeigten URL. Geben Sie den bereitgestellten Code ein und melden Sie sich an. Kehren Sie nach Abschluss zurück zur Anwendung, und wählen Sie die **1 aus. Option Zugriffstoken anzeigen** , um das Zugriffstoken anzuzeigen.
