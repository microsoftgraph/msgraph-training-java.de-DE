---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634780"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="04c20-101">In diesem Lernprogramm erfahren Sie, wie Sie eine Java Console-app erstellen, die die Microsoft Graph-API zum Abrufen von Kalenderinformationen für einen Benutzer verwendet.</span><span class="sxs-lookup"><span data-stu-id="04c20-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="04c20-102">Wenn Sie das fertige Lernprogramm einfach herunterladen möchten, können Sie das GitHub- [Repository](https://github.com/microsoftgraph/msgraph-training-java)herunterladen oder Klonen.</span><span class="sxs-lookup"><span data-stu-id="04c20-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04c20-103">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="04c20-103">Prerequisites</span></span>

<span data-ttu-id="04c20-104">Bevor Sie mit diesem Lernprogramm beginnen, sollten Sie das [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) und [Maven](https://maven.apache.org/) auf Ihrem Entwicklungscomputer installiert haben.</span><span class="sxs-lookup"><span data-stu-id="04c20-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="04c20-105">Wenn Sie nicht über das JDK oder Maven verfügen, besuchen Sie die vorherigen Links für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="04c20-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="04c20-106">Dieses Lernprogramm wurde mit der openjdk-Version 12.0.1 und Maven 3.6.1 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="04c20-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="04c20-107">Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, jedoch nicht getestet.</span><span class="sxs-lookup"><span data-stu-id="04c20-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="04c20-108">Feedback</span><span class="sxs-lookup"><span data-stu-id="04c20-108">Feedback</span></span>

<span data-ttu-id="04c20-109">Geben Sie Feedback zu diesem Lernprogramm im [GitHub-Repository](https://github.com/microsoftgraph/msgraph-training-java)an.</span><span class="sxs-lookup"><span data-stu-id="04c20-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
