---
title: 'Tutorial de aplicación web: configuración de la aplicación cliente'
description: Este tutorial le guía a través de los pasos para registrar una aplicación pública con el fin de preparar la implementación de una aplicación web
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: mihansen
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: 58e21e46edfe1d03d42bf2202bcf1f22282631a9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "84870458"
---
# <a name="client-application-registration"></a>Registro de una aplicación cliente
En el tutorial anterior, ha implementado y configurado Azure API for FHIR. Ahora que la configuración de Azure API for FHIR ya está realizada, vamos a registrar una aplicación cliente pública. Para conocer más detalles o solucionar problemas puede leer la guía paso a paso para [registrar una aplicación cliente pública](register-public-azure-ad-client-app.md), pero en este tutorial hemos indicado los pasos principales a continuación.

1. Vaya a Azure Active Directory.
1. Seleccione **Registro de aplicación** --> **Nuevo registro**.
1. Asigne un nombre a la aplicación y configure el URI de redirección como https://www.getpostman.com/oauth2/callback


![Registro de una aplicación cliente](media/tutorial-web-app/reg-public-app.png)

## <a name="client-application-settings"></a>Configuración de la aplicación cliente
Una vez registrada la aplicación, copie el identificador de aplicación (cliente) de la página Información general. Necesitará este valor más adelante al acceder al cliente.

![Copiar el identificador de la aplicación](media/tutorial-web-app/app-id.png)

A continuación, establezca las opciones de autenticación correctas. Seleccione **Autenticación** en el menú izquierdo. Seleccione las casillas **Token de acceso** y **Token de identificador**. También puede configurar el URI de redirección en preparación para cuando cree la aplicación web en la cuarta parte de este tutorial. Para ello, agregue https://\<WEB-APP-NAME>.azurewebsites.net a la lista de URI de redirección. Si elige un nombre diferente durante el paso en el que [escribe la aplicación web](tutorial-web-app-write-web-app.md), tendrá que volver y actualizar esto.

![Configuración de la autenticación de la aplicación](media/tutorial-web-app/app-authentication.png)

Ahora que ya ha configurado la autenticación correcta, establezca los permisos de la API. 
1. Seleccione **Permisos de API** y haga clic en **Agregar un permiso**.
1. En **API usadas en mi organización**, busque las API de salud de Azure.
1. Seleccione **user_impersonation** y haga clic en **Agregar permisos**.

## <a name="next-steps"></a>Pasos siguientes
Ahora tiene una aplicación cliente pública. El próximo tutorial le guiará a través de las pruebas y la obtención de acceso a esta aplicación mediante Postman.

>[!div class="nextstepaction"]
>[Prueba de la aplicación cliente en Postman](tutorial-web-app-test-postman.md)
