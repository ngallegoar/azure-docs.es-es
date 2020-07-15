---
title: 'Tutorial: Inicio del Lector inmersivo con el ejemplo de código de iOS de Swift'
titleSuffix: Azure Cognitive Services
description: En este tutorial, configurará y ejecutará una aplicación Swift de ejemplo que inicia el Lector inmersivo.
services: cognitive-services
author: dylankil
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: dylankil
ms.openlocfilehash: a9259026747102dd65be3bb8da853735098667db
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049333"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-swift-ios-code-sample"></a>Tutorial: Inicio del Lector inmersivo con el ejemplo de código de iOS de Swift

En la [introducción](./overview.md), se indica lo que es el Lector inmersivo y cómo implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, alumnos de idiomas y alumnos con diferencias de aprendizaje. En este tutorial se explica cómo crear una aplicación iOS que inicia el Lector inmersivo. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar y ejecutar una aplicación Swift para iOS mediante un proyecto de ejemplo
> * Adquisición de un token de acceso
> * Iniciar el Lector inmersivo con contenido de muestra

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Un recurso del Lector inmersivo configurado para la autenticación de Azure Active Directory. Siga [estas instrucciones](./how-to-create-immersive-reader.md) para realizar la configuración. Necesitará algunos de los valores que se crean aquí cuando configure las propiedades del entorno. Guarde la salida de la sesión en un archivo de texto para futuras referencias.
* [macOS](https://www.apple.com/macos)
* [Git](https://git-scm.com/)
* [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk)
* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

## <a name="configure-authentication-credentials"></a>Configuración de las credenciales de autenticación

1. Abra Xcode y abra **immersive-reader-sdk/js/samples/ios/quickstart-swift/quickstart-swift.xcodeproj**.
2. En el menú superior, haga clic en **Product > Scheme > Edit Scheme...** (Producto > Esquema > Editar esquema).
3. En la vista **Run** (Ejecutar), haga clic en la pestaña **Arguments** (Argumentos).
4. En la sección **Environment Variables** (Variables de entorno), agregue los siguientes nombres y valores, y proporcione los valores especificados al crear el recurso del Lector inmersivo.

```text
TENANT_ID=<YOUR_TENANT_ID>
CLIENT_ID=<YOUR_CLIENT_ID>
CLIENT_SECRET<YOUR_CLIENT_SECRET>
SUBDOMAIN=<YOUR_SUBDOMAIN>
```

Asegúrese de no confirmar el cambio anterior en el control de código fuente, ya que contiene secretos que no deben hacerse públicos.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Iniciar el Lector inmersivo con contenido de muestra

1. En Xcode, presione **Ctrl-R** para ejecutar el proyecto.

## <a name="next-steps"></a>Pasos siguientes

* Explorar el [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](./reference.md)
* Ver muestras de código en [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/)
