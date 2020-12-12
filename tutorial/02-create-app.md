---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661081"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3b820-101">In diesem Abschnitt erstellen Sie eine grundlegende Java Console-app.</span><span class="sxs-lookup"><span data-stu-id="3b820-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="3b820-102">Öffnen Sie die Befehlszeilenschnittstelle (CLI) in einem Verzeichnis, in dem Sie das Projekt erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="3b820-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="3b820-103">Führen Sie den folgenden Befehl aus, um ein neues Gradle-Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3b820-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="3b820-104">Nachdem das Projekt erstellt wurde, überprüfen Sie, ob es funktionsfähig ist, indem Sie den folgenden Befehl ausführen, um die app in ihrer CLI auszuführen.</span><span class="sxs-lookup"><span data-stu-id="3b820-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="3b820-105">Wenn die APP funktioniert, sollte Sie ausgegeben werden `Hello World.` .</span><span class="sxs-lookup"><span data-stu-id="3b820-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="3b820-106">Installieren von Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="3b820-106">Install dependencies</span></span>

<span data-ttu-id="3b820-107">Bevor Sie fortfahren, fügen Sie einige zusätzliche Abhängigkeiten hinzu, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="3b820-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="3b820-108">[Microsoft Authentication Library (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) , um den Benutzer zu authentifizieren und Zugriffstoken zu erwerben.</span><span class="sxs-lookup"><span data-stu-id="3b820-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="3b820-109">[Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="3b820-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="3b820-110">[SLF4J NOP Bindung](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) zum Unterdrücken der Protokollierung von MSAL.</span><span class="sxs-lookup"><span data-stu-id="3b820-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="3b820-111">Öffnen Sie **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="3b820-111">Open **./build.gradle**.</span></span> <span data-ttu-id="3b820-112">Aktualisieren `dependencies` Sie den Abschnitt, um diese Abhängigkeiten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="3b820-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="3b820-113">Fügen Sie am Ende von **./Build.gradle** Folgendes hinzu.</span><span class="sxs-lookup"><span data-stu-id="3b820-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="3b820-114">Wenn Sie das Projekt das nächste Mal erstellen, werden diese Abhängigkeiten von Gradle heruntergeladen.</span><span class="sxs-lookup"><span data-stu-id="3b820-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="3b820-115">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="3b820-115">Design the app</span></span>

1. <span data-ttu-id="3b820-116">Öffnen Sie die Datei **./src/main/java/graphtutorial/app.Java** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="3b820-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="3b820-117">Dadurch wird ein einfaches Menü implementiert, und die Auswahl des Benutzers wird von der Befehlszeile aus gelesen.</span><span class="sxs-lookup"><span data-stu-id="3b820-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
