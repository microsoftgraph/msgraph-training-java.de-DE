---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634759"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="037b2-101">Öffnen Sie die Befehlszeilenschnittstelle (CLI) in einem Verzeichnis, in dem Sie das Projekt erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="037b2-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="037b2-102">Führen Sie den folgenden Befehl aus, um ein neues Maven-Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="037b2-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="037b2-103">Sie können unterschiedliche Werte für die Gruppen-ID`DgroupId` (Parameter) und Artefakt-`DartifactId` ID (Parameter) eingeben, als die oben angegebenen Werte.</span><span class="sxs-lookup"><span data-stu-id="037b2-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="037b2-104">Im Beispielcode in diesem Lernprogramm wird davon ausgegangen, dass die Gruppen-ID `com.contoso` verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="037b2-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="037b2-105">Wenn Sie einen anderen Wert verwenden, müssen Sie in jedem `com.contoso` Beispielcode durch Ihre Gruppen-ID ersetzen.</span><span class="sxs-lookup"><span data-stu-id="037b2-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="037b2-106">Wenn Sie dazu aufgefordert werden, bestätigen Sie die Konfiguration, und warten Sie, bis das Projekt erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="037b2-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="037b2-107">Nachdem das Projekt erstellt wurde, überprüfen Sie, ob es funktionsfähig ist, indem Sie die folgenden Befehle ausführen, um die app in ihrer CLI zu verpacken und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="037b2-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="037b2-108">Wenn die APP funktioniert, sollte Sie ausgegeben `Hello World!`werden.</span><span class="sxs-lookup"><span data-stu-id="037b2-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="037b2-109">Bevor Sie fortfahren, fügen Sie einige zusätzliche Abhängigkeiten hinzu, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="037b2-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="037b2-110">[Microsoft Authentication Library (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) , um den Benutzer zu authentifizieren und Zugriffstoken zu erwerben.</span><span class="sxs-lookup"><span data-stu-id="037b2-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="037b2-111">[Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="037b2-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="037b2-112">[SLF4J NOP Bindung](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) zum Unterdrücken der Protokollierung von MSAL.</span><span class="sxs-lookup"><span data-stu-id="037b2-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="037b2-113">Öffnen Sie **./graphtutorial/Pom.XML**.</span><span class="sxs-lookup"><span data-stu-id="037b2-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="037b2-114">Fügen Sie das folgende innerhalb `<dependencies>` des-Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="037b2-114">Add the following inside the `<dependencies>` element.</span></span>

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.4.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>0.4.0-preview</version>
</dependency>
```

<span data-ttu-id="037b2-115">Wenn Sie das Projekt das nächste Mal erstellen, werden diese Abhängigkeiten von Maven heruntergeladen.</span><span class="sxs-lookup"><span data-stu-id="037b2-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="037b2-116">Entwerfen der APP</span><span class="sxs-lookup"><span data-stu-id="037b2-116">Design the app</span></span>

<span data-ttu-id="037b2-117">Öffnen Sie die Datei **./graphtutorial/src/main/java/com/Contoso/app.Java** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="037b2-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

```java
package com.contoso;

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
            System.out.println("2. List calendar events");

            try {
                choice = input.nextInt();
            } catch (InputMismatchException ex) {
                // Skip over non-integer input
                input.nextLine();
            }

            // Process user choice
            switch(choice) {
                case 0:
                    // Exit the program
                    System.out.println("Goodbye...");
                    break;
                case 1:
                    // Display access token
                case 2:
                    // List the calendar
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        }

        input.close();
    }
}
```

<span data-ttu-id="037b2-118">Dadurch wird ein einfaches Menü implementiert, und die Auswahl des Benutzers wird von der Befehlszeile aus gelesen.</span><span class="sxs-lookup"><span data-stu-id="037b2-118">This implements a basic menu and reads the user's choice from the command line.</span></span>
