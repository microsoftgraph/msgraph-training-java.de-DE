---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661053"
---
# <a name="how-to-run-the-completed-project"></a>Vorgehensweise Ausführen des abgeschlossenen Projekts

## <a name="prerequisites"></a>Voraussetzungen

Um das abgeschlossene Projekt in diesem Ordner auszuführen, benötigen Sie Folgendes:

- [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) und [Gradle](https://gradle.org/) auf Ihrem Entwicklungscomputer installiert. Wenn Sie nicht über das JDK oder Gradle verfügen, besuchen Sie die vorherigen Links für Downloadoptionen. (**Hinweis:** dieses Lernprogramm wurde mit der openjdk-Version 14.0.0.36 und Gradle 6.7.1 geschrieben. Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, aber das wurde nicht getestet.)
- Ein Microsoft-Arbeits-oder Schulkonto.

Wenn Sie kein Microsoft-Konto haben, können Sie [sich für das Office 365 Entwicklerprogramm registrieren](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrieren einer Webanwendung im Azure-Active Directory Admin Center

1. Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.

1. Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.

    ![Screenshot der APP-Registrierungen ](/tutorial/images/aad-portal-app-registrations.png)

1. Wählen Sie **Neue Registrierung** aus. Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.

    - Legen Sie **Name** auf `Java Graph Tutorial` fest.
    - Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis** fest.
    - Lassen Sie **URI umleiten** leer.

    ![Screenshot der Seite "Anwendung registrieren"](/tutorial/images/aad-register-an-app.png)

1. Wählen Sie **Registrieren** aus. Kopieren Sie auf der Seite **Java Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, die Sie im nächsten Schritt benötigen.

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](/tutorial/images/aad-application-id.png)

1. Klicken Sie auf den Link **Umleitungs-URI hinzufügen** . Suchen Sie auf der Seite " **Umleitungs-URIs** " den Abschnitt **vorgeschlagene Umleitungs-URIs für öffentliche Clients (Mobile, Desktop)** . Wählen Sie den `https://login.microsoftonline.com/common/oauth2/nativeclient` URI aus.

    ![Screenshot der Seite "Umleitungs-URIs"](/tutorial/images/aad-redirect-uris.png)

1. Suchen Sie den **standardmäßigen Clienttyp** Abschnitt, und ändern Sie die Option **Anwendung als öffentlichen Client** Umschalten auf **Ja**, und wählen Sie dann **Speichern** aus.

    ![Ein Screenshot des Typs "Standard Clienttyp"](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Konfigurieren des Beispiels

1. Benennen `oAuth.properties.example` Sie die Datei in `oAuth.properties` .
1. Bearbeiten Sie die `oAuth.properties` Datei, und nehmen Sie die folgenden Änderungen vor.
    1. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Navigieren Sie in der Befehlszeilenschnittstelle (CLI) zum Projektverzeichnis, und führen Sie die folgenden Befehle aus.

```Shell
./gradlew --console plain run
```
