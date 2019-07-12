---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634759"
---
<!-- markdownlint-disable MD002 MD041 -->

Öffnen Sie die Befehlszeilenschnittstelle (CLI) in einem Verzeichnis, in dem Sie das Projekt erstellen möchten. Führen Sie den folgenden Befehl aus, um ein neues Maven-Projekt zu erstellen.

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> Sie können unterschiedliche Werte für die Gruppen-ID`DgroupId` (Parameter) und Artefakt-`DartifactId` ID (Parameter) eingeben, als die oben angegebenen Werte. Im Beispielcode in diesem Lernprogramm wird davon ausgegangen, dass die Gruppen-ID `com.contoso` verwendet wurde. Wenn Sie einen anderen Wert verwenden, müssen Sie in jedem `com.contoso` Beispielcode durch Ihre Gruppen-ID ersetzen.

Wenn Sie dazu aufgefordert werden, bestätigen Sie die Konfiguration, und warten Sie, bis das Projekt erstellt wurde. Nachdem das Projekt erstellt wurde, überprüfen Sie, ob es funktionsfähig ist, indem Sie die folgenden Befehle ausführen, um die app in ihrer CLI zu verpacken und auszuführen.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

Wenn die APP funktioniert, sollte Sie ausgegeben `Hello World!`werden. Bevor Sie fortfahren, fügen Sie einige zusätzliche Abhängigkeiten hinzu, die Sie später verwenden werden.

- [Microsoft Authentication Library (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) , um den Benutzer zu authentifizieren und Zugriffstoken zu erwerben.
- [Microsoft Graph SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.
- [SLF4J NOP Bindung](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) zum Unterdrücken der Protokollierung von MSAL.

Öffnen Sie **./graphtutorial/Pom.XML**. Fügen Sie das folgende innerhalb `<dependencies>` des-Elements hinzu.

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

Wenn Sie das Projekt das nächste Mal erstellen, werden diese Abhängigkeiten von Maven heruntergeladen.

## <a name="design-the-app"></a>Entwerfen der APP

Öffnen Sie die Datei **./graphtutorial/src/main/java/com/Contoso/app.Java** , und ersetzen Sie den Inhalt durch Folgendes.

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

Dadurch wird ein einfaches Menü implementiert, und die Auswahl des Benutzers wird von der Befehlszeile aus gelesen.
