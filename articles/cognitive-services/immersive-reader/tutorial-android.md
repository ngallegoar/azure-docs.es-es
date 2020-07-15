---
title: 'Tutorial: Inicio del Lector inmersivo con los ejemplos de código de Android'
titleSuffix: Azure Cognitive Services
description: En este tutorial, configurará y ejecutará una aplicación Android de ejemplo que inicia el Lector inmersivo.
services: cognitive-services
author: dylankil
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: dylankil
ms.openlocfilehash: d847b4ab9f3c06634390e1f67dfed36df938c3ad
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049327"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-android-java-code-sample"></a>Tutorial: Inicio del Lector inmersivo mediante el ejemplo de código Java de Android

En la [introducción](./overview.md), se indica lo que es el Lector inmersivo y cómo implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, alumnos de idiomas y alumnos con diferencias de aprendizaje. En este tutorial se explica cómo crear una aplicación Android que inicia el Lector inmersivo. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar y ejecutar una aplicación para Android con un proyecto de ejemplo
> * Adquisición de un token de acceso
> * Iniciar el Lector inmersivo con contenido de muestra

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Un recurso del Lector inmersivo configurado para la autenticación de Azure Active Directory. Siga [estas instrucciones](./how-to-create-immersive-reader.md) para realizar la configuración. Necesitará algunos de los valores que se crean aquí cuando configure las propiedades del entorno. Guarde la salida de la sesión en un archivo de texto para futuras referencias.
* [Git](https://git-scm.com/)
* [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk)
* [Android Studio](https://developer.android.com/studio)

## <a name="configure-authentication-credentials"></a>Configuración de las credenciales de autenticación

1. Inicie Android Studio y abra el proyecto desde el directorio **immersive-reader-sdk/js/samples/quickstart-java-android** (Java) o **immersive-reader-sdk/js/samples/quickstart-kotlin** (Kotlin).

2. Cree un archivo denominado **env** en la carpeta **/assets** y agregue la siguiente información, proporcionando los valores según corresponda. Asegúrese de no confirmar este archivo en el control de código fuente, ya que contiene secretos que no deben hacerse públicos.

```text
TENANT_ID=<YOUR_TENANT_ID>
CLIENT_ID=<YOUR_CLIENT_ID>
CLIENT_SECRET=<YOUR_CLIENT_SECRET>
SUBDOMAIN=<YOUR_SUBDOMAIN>
```

## <a name="launch-the-immersive-reader-with-sample-content"></a>Iniciar el Lector inmersivo con contenido de muestra

1. Elija un emulador de dispositivos de AVD Manager y ejecute el proyecto.

## <a name="next-steps"></a>Pasos siguientes

* Explorar el [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](./reference.md)
* Ver muestras de código en [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/)