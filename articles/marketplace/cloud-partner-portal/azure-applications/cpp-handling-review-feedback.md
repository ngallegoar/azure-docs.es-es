---
title: Administración de los comentarios de revisión de aplicaciones de Azure | Azure Marketplace
description: Explica cómo usar Azure DevOps para administrar los comentarios de revisión de ofertas de aplicaciones de Azure para Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: dsindona
ms.openlocfilehash: b5f290bae908ac753801eef2c9b8394ca1bb7a40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285306"
---
# <a name="handling-review-feedback"></a>Administración de los comentarios de revisión

En este artículo se explica cómo acceder al entorno de Azure DevOps que usa el equipo de revisión de Microsoft Azure Marketplace.  Si se detectan problemas críticos en su oferta de la aplicación de Azure durante el paso de **revisión de Microsoft**, puede iniciar sesión en este sistema para la información detallada sobre estos problemas (comentarios de revisión).  Después de corregir todos estos problemas, debe volver a enviar la oferta para proceder a su publicación en Azure Marketplace.  En el siguiente diagrama se ilustra cómo se relacionan estos comentarios con la publicación.

![Pasos de publicación con comentarios de Azure DevOps](./media/pub-flow-vsts-access.png)

Normalmente, a los problemas de revisión se hace referencia con solicitudes de incorporación de cambios (PR).  Cada solicitud de incorporación de cambios está vinculada a un elemento de [Azure DevOps](https://azure.microsoft.com/services/devops/) (anteriormente llamado Visual Studio Team Services) con los detalles sobre el problema.  En la siguiente imagen se muestra un ejemplo de una referencia de solicitud de incorporación de cambios de revisión.  Para situaciones complejas, los equipos de revisión y soporte técnico también pueden escribir por correo electrónico. 

![Pestaña Status (Estado) con los comentarios de revisión](./media/status-tab-ms-review.png)


## <a name="azure-devops-access"></a>Acceso de Azure DevOps

Para ver los elementos de la solicitud de incorporación de cambios a los que se hace referencia en los comentarios de revisión, los publicadores deben primero contar con la autorización pertinente.  De lo contrario, los publicadores nuevos recibirán una página de respuesta de tipo `401 - Not Authorized` cuando intenten ver estas solicitudes.  Para solicitar el acceso a este repositorio de Azure DevOps, realice los pasos siguientes:

1. Recopile la siguiente información:
    - El nombre y el identificador del publicador
    - Tipo de oferta (aplicación de Azure), nombre de la oferta e identificador de la SKU
    - Vínculo de la solicitud de incorporación de cambios, por ejemplo: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>` Esta dirección URL se puede recuperar del mensaje de notificación o de la dirección de la página de respuesta de tipo 401.
    - Direcciones de correo electrónico de las personas de su organización de publicación a las que desea conceder acceso.  Esta lista debe incluir las direcciones del propietario que proporcionó al registrarse como publicador en Cloud Partner Portal.
2. Cree una incidencia de soporte técnico.  En la barra de título de Cloud Partner Portal, seleccione el botón **Help** (Ayuda) y elija **Support** (Soporte técnico) en el menú.  El navegador web predeterminado debe iniciar la página de Microsoft con el nuevo incidente de soporte técnico e ir a ella.  (Es posible que deba iniciar sesión).
3. Especifique **Problem type** (Tipo de problema) como **Marketplace Onboarding** (Incorporación a Marketplace) y **Category** (Categoría) como **Access problem** (Problema de acceso), y seleccione **Start request** (Iniciar solicitud).

    ![Categoría de la incidencia de soporte técnico](./media/support-incident1.png)

4. En la página **Step 1 of 2** (Paso 1 de 2), proporcione la información de contacto y seleccione **Continue** (Continuar).
5. En la página **Step 2 of 2** (Paso 2 de 2), especifique un título de incidente (por ejemplo, `Request Azure DevOps access`) y proporcione la información recopilada en el primer paso (arriba).  Lea y acepte el contrato y seleccione **Send** (Enviar).

Si el incidente se creó correctamente, se mostrará una página de confirmación.  Guarde la información de confirmación en esta página para consultarla en el futuro.  El equipo de soporte técnico de Microsoft debe responder a la solicitud de acceso en unos días hábiles.


## <a name="reviewing-the-pull-request"></a>Revisión de la solicitud de incorporación de cambios 

Use el procedimiento siguiente para revisar los problemas que se documentan en la solicitud de incorporación de cambios.

1. En la sección de **Revisión de Microsoft** del formulario **Pasos de publicación**, haga clic en un vínculo de solicitud de incorporación de cambios para iniciar el explorador e ir a la página **Información general** (página principal) de esta solicitud.  En la siguiente imagen se muestra una página principal de ejemplo de un problema crítico de una oferta de la aplicación Contoso.  Esta página contiene información resumida útil acerca de los problemas de revisión que se detectaron en la aplicación de Azure.  

    [![Página principal de la solicitud de incorporación de cambios](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)
    <br/> *Haga clic en la imagen para expandirla.*
    
2. (Opcional) En el lado derecho de la ventana, en la sección **Directivas**, haga clic en el mensaje emitido (en este ejemplo: **Policy Validation failed**) (Error durante la validación de directivas) para investigar los detalles de bajo nivel del problema, incluidos los archivos de registro asociados.  Normalmente, los errores aparecen en la parte inferior de los archivos de registro.

3. En el menú en el lado izquierdo de la página principal, seleccione **Archivos** para mostrar la lista de archivos que componen los recursos técnicos de esta oferta.  Los revisores de Microsoft deberían haber agregado comentarios que describen los problemas críticos detectados.  En el ejemplo siguiente, se detectaron dos problemas. 

    [![Página principal de la solicitud de incorporación de cambios](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)
    <br/> *Haga clic en la imagen para expandirla.*

4. Haga clic en cada nodo de comentario en el árbol de la izquierda para ver el comentario en el contexto del código circundante.  Corrija el código fuente en el proyecto de equipo para corregir el problema que se describe en el comentario.

> [!Note]
> No puede editar los recursos técnicos de su oferta dentro del entorno de Azure DevOps del equipo de revisión.  Para los publicadores, este es un entorno de solo lectura para el código fuente contenido.  No obstante, puede dejar las respuestas a los comentarios para beneficio del equipo de revisión de Microsoft.

   En el ejemplo siguiente, el publicador ha revisado, corregido y respondido al primer problema.

   ![Primera respuesta de revisión y comentario](./media/first-comment-reply.png)


## <a name="next-steps"></a>Pasos siguientes

Después de corregir los problemas críticos que se documentan en las solicitudes de incorporación de cambios de la revisión, debe [volver a publicar la oferta de la aplicación de Azure](./cpp-publish-offer.md).
