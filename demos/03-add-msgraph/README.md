---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634717"
---
# <a name="how-to-run-the-completed-project"></a>Vorgehensweise Ausführen des abgeschlossenen Projekts

## <a name="prerequisites"></a>Voraussetzungen

Um das abgeschlossene Projekt in diesem Ordner auszuführen, benötigen Sie Folgendes:

- [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) und [Maven](https://maven.apache.org/) auf Ihrem Entwicklungscomputer installiert. Wenn Sie nicht über das JDK oder Maven verfügen, besuchen Sie die vorherigen Links für Downloadoptionen. (**Hinweis:** dieses Lernprogramm wurde mit der openjdk-Version 12.0.1 und Maven 3.6.1 geschrieben. Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, aber das wurde nicht getestet.)
- Ein Microsoft-Arbeits-oder Schulkonto.

Wenn Sie kein Microsoft-Konto haben, können Sie [sich für das Office 365 Entwicklerprogramm registrieren](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrieren einer Webanwendung im Azure-Active Directory Admin Center

1. Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.

1. Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App** -Registrierungen unter **Manage**aus.

    ![Ein Screenshot der APP-Registrierungen ](/tutorial/images/aad-portal-app-registrations.png)

1. Wählen Sie **Neue Registrierung** aus. Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.

    - Legen Sie **Name** auf `Java Graph Tutorial` fest.
    - Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis**fest.
    - Lassen Sie **URI umleiten** leer.

    ![Screenshot der Seite "Anwendung registrieren"](/tutorial/images/aad-register-an-app.png)

1. Wählen Sie **Registrieren** aus. Kopieren Sie auf der Seite **Java Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, die Sie im nächsten Schritt benötigen.

    ![Ein Screenshot der Anwendungs-ID der neuen App-Registrierung](/tutorial/images/aad-application-id.png)

1. Klicken Sie auf den Link Umleitungs- **URI hinzufügen** . Suchen Sie auf der Seite "Umleitungs- **URIs** " den Abschnitt vorgeschlagene Umleitungs- **URIs für öffentliche Clients (Mobile, Desktop)** . Wählen Sie `https://login.microsoftonline.com/common/oauth2/nativeclient` den URI aus.

    ![Screenshot der Seite "Umleitungs-URIs"](/tutorial/images/aad-redirect-uris.png)

1. Suchen Sie den **standardmäßigen Clienttyp** Abschnitt, und ändern Sie die Option **Anwendung als öffentlichen Client** Umschalten auf **Ja**, und wählen Sie dann **Speichern**aus.

    ![Ein Screenshot des Typs "Standard Clienttyp"](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Konfigurieren des Beispiels

1. Benennen Sie `oAuth.properties.example` die Datei `oAuth.properties`in.
1. Bearbeiten Sie `oAuth.properties` die Datei, und nehmen Sie die folgenden Änderungen vor.
    1. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Navigieren Sie in der Befehlszeilenschnittstelle (CLI) zum Projektverzeichnis, und führen Sie die folgenden Befehle aus.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```
