---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661081"
---
<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt erstellen Sie eine grundlegende Java Console-app.

1. Öffnen Sie die Befehlszeilenschnittstelle (CLI) in einem Verzeichnis, in dem Sie das Projekt erstellen möchten. Führen Sie den folgenden Befehl aus, um ein neues Gradle-Projekt zu erstellen.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Nachdem das Projekt erstellt wurde, überprüfen Sie, ob es funktionsfähig ist, indem Sie den folgenden Befehl ausführen, um die app in ihrer CLI auszuführen.

    ```Shell
    ./gradlew --console plain run
    ```

    Wenn die APP funktioniert, sollte Sie ausgegeben werden `Hello World.` .

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Bevor Sie fortfahren, fügen Sie einige zusätzliche Abhängigkeiten hinzu, die Sie später verwenden werden.

- [Microsoft Authentication Library (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) , um den Benutzer zu authentifizieren und Zugriffstoken zu erwerben.
- [Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.
- [SLF4J NOP Bindung](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) zum Unterdrücken der Protokollierung von MSAL.

1. Öffnen Sie **./Build.gradle**. Aktualisieren `dependencies` Sie den Abschnitt, um diese Abhängigkeiten hinzuzufügen.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. Fügen Sie am Ende von **./Build.gradle** Folgendes hinzu.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

Wenn Sie das Projekt das nächste Mal erstellen, werden diese Abhängigkeiten von Gradle heruntergeladen.

## <a name="design-the-app"></a>Entwerfen der App

1. Öffnen Sie die Datei **./src/main/java/graphtutorial/app.Java** , und ersetzen Sie den Inhalt durch Folgendes.

    ```java
    package graphtutorial;

    import java.util.InputMismatchException;
    import java.util.Scanner;

    /**
     * Graph Tutorial
     *
     */
    public class App {
        public static void main(String[] args) {
            System.out.println("Java Graph Tutorial");
            System.out.println();

            Scanner input = new Scanner(System.in);

            int choice = -1;

            while (choice != 0) {
                System.out.println("Please choose one of the following options:");
                System.out.println("0. Exit");
                System.out.println("1. Display access token");
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

                // Process user choice
                switch(choice) {
                    case 0:
                        // Exit the program
                        System.out.println("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    Dadurch wird ein einfaches Menü implementiert, und die Auswahl des Benutzers wird von der Befehlszeile aus gelesen.
