---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018818"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f79f8-101">In diesem Lernprogramm erfahren Sie, wie Sie eine Java Console-app erstellen, die die Microsoft Graph-API zum Abrufen von Kalenderinformationen für einen Benutzer verwendet.</span><span class="sxs-lookup"><span data-stu-id="f79f8-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="f79f8-102">Wenn Sie das fertige Lernprogramm einfach herunterladen möchten, können Sie das GitHub- [Repository](https://github.com/microsoftgraph/msgraph-training-java)herunterladen oder Klonen.</span><span class="sxs-lookup"><span data-stu-id="f79f8-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f79f8-103">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="f79f8-103">Prerequisites</span></span>

<span data-ttu-id="f79f8-104">Bevor Sie mit diesem Lernprogramm beginnen, sollten Sie das [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) und [Maven](https://maven.apache.org/) auf Ihrem Entwicklungscomputer installiert haben.</span><span class="sxs-lookup"><span data-stu-id="f79f8-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="f79f8-105">Wenn Sie nicht über das JDK oder Maven verfügen, besuchen Sie die vorherigen Links für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="f79f8-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="f79f8-106">Dieses Lernprogramm wurde mit der openjdk-Version 12.0.1 und Maven 3.6.1 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f79f8-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="f79f8-107">Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, jedoch nicht getestet.</span><span class="sxs-lookup"><span data-stu-id="f79f8-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="f79f8-108">Feedback</span><span class="sxs-lookup"><span data-stu-id="f79f8-108">Feedback</span></span>

<span data-ttu-id="f79f8-109">Geben Sie Feedback zu diesem Lernprogramm im [GitHub-Repository](https://github.com/microsoftgraph/msgraph-training-java)an.</span><span class="sxs-lookup"><span data-stu-id="f79f8-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
