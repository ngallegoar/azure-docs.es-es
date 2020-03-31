---
title: Creación de una aplicación de Android
description: Siga este tutorial para empezar a usar back-ends de aplicación móvil de Azure para el desarrollo de aplicaciones Android.
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 9be9402bff1a1263de3c6f21b78899464c50c823
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459576"
---
# <a name="create-an-android-app"></a>Creación de una aplicación de Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Información general
En este tutorial se muestra cómo agregar un servicio back-end basado en la nube a una aplicación móvil de Android con un back-end de la aplicación móvil de Azure.  Creará tanto un back-end de aplicación móvil nuevo como una aplicación de Android simple de la *lista de tareas pendientes* que almacene los datos de la aplicación en Azure.

Completar este tutorial es un requisito previo para todos los demás tutoriales de Android sobre cómo usar la característica Mobile Apps en Azure App Service.

## <a name="prerequisites"></a>Prerrequisitos
Para completar este tutorial, necesitará lo siguiente:

* [Herramientas para desarrolladores de Android](https://developer.android.com/sdk/index.html), que incluyen el entorno de desarrollo integrado de Android Studio y la plataforma de Android más reciente.
* Android SDK de Azure Mobile.
* Una [cuenta de Azure activa](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Aplicaciones móviles de Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Creación de una conexión de base de datos y configuración del proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-android-app"></a>Ejecución de la aplicación Android
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
