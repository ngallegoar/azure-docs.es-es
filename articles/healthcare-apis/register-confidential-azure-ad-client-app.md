---
title: 'Registro de una aplicación cliente confidencial en Azure AD: Azure API for FHIR'
description: Registre una aplicación cliente confidencial en Azure Active Directory que se autentique en nombre de un usuario y solicite acceso a las aplicaciones de recursos.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 8021fb3fa9f11ef895569f48a2ae21b3f7adcd36
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91826219"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registro de una aplicación cliente confidencial en Azure Active Directory

En este tutorial aprenderá a registrar una aplicación cliente confidencial en Azure Active Directory. 

Un registro de aplicación cliente es una representación de Azure Active Directory de una aplicación que se puede usar para autenticar en nombre de un usuario y solicitar acceso a las [aplicaciones de recursos](register-resource-azure-ad-client-app.md). Una aplicación cliente confidencial es una aplicación en la que se puede confiar para almacenar un secreto y presentar dicho secreto cuando se solicitan tokens de acceso. Ejemplos de aplicaciones confidenciales son las aplicaciones de servidor.

Para registrar una nueva aplicación confidencial en el portal, realice los pasos siguientes.

## <a name="register-a-new-application"></a>Registro de una nueva aplicación

1. En [Azure Portal](https://portal.azure.com), diríjase a **Azure Active Directory**.

1. Seleccione **App registrations** (Registros de aplicaciones).

    ![Azure Portal. Nuevo registro de aplicaciones.](media/how-to-aad/portal-aad-new-app-registration.png)

1. Seleccione **Nuevo registro**.

1. Asigne un nombre para mostrar a la aplicación.

1. Proporcione una dirección URL de respuesta. Estos detalles se pueden cambiar más adelante, pero si conoce la dirección URL de respuesta de la aplicación, indíquelo ahora.

    ![Nuevo registro de la aplicación cliente confidencial.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)
1. Seleccione **Registrar**.

## <a name="api-permissions"></a>Permisos de API

Ahora que ha registrado la aplicación, deberá seleccionar los permisos de API que esta aplicación debería poder solicitar en nombre de los usuarios:

1. Seleccione **Permisos de API**.

    ![Cliente confidencial. Permisos de API](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

1. Seleccione **Agregar un permiso**.

    Si usa Azure API for FHIR, agregará un permiso a las **Azure Healthcare API** mediante la búsqueda de dichas API en **API usadas en mi organización**. Solo las encontrará si ya tiene la [implementación de Azure API for FHIR](fhir-paas-powershell-quickstart.md).

    Si hace referencia a una aplicación de recursos diferente, seleccione el [registro de la aplicación de recursos de la API de FHIR](register-resource-azure-ad-client-app.md) que creó anteriormente en **Mis API**.


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Cliente confidencial. API de mi organización" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::.
    

3. Seleccione los ámbitos (permisos) que la aplicación confidencial debería poder solicitar en nombre del usuario:

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Cliente confidencial. API de mi organización":::.

## <a name="application-secret"></a>Secreto de aplicación

1. Seleccione **Certificados y secretos**.
1. Seleccione **Nuevo secreto de cliente**. 

    ![Cliente confidencial. Secreto de aplicación](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Proporcione una descripción y la duración del secreto (1 año, 2 años o nunca).

3. Una vez generado, se mostrará en el portal una sola vez. Tome nota del mismo y guárdelo de forma segura.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido cómo registrar una aplicación cliente confidencial en Azure Active Directory. A continuación, puede acceder al servidor de FHIR mediante Postman.
 
>[!div class="nextstepaction"]
>[Acceso a Azure API for FHIR con Postman](access-fhir-postman-tutorial.md)
