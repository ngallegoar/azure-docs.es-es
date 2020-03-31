---
title: Administración de los comentarios de revisión para las ofertas de aplicaciones de Azure en Marketplace comercial
description: Cómo administrar los comentarios de revisión de las ofertas de aplicaciones de Azure para mostrarlas o venderlas en Azure Marketplace, AppSource, o bien a través del programa Proveedor de soluciones en la nube (CSP) mediante el portal de Marketplace comercial del Centro de partners de Microsoft.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/11/2019
ms.openlocfilehash: 3932f3fc71dc6427b6cdf93d3a7bc58534a9981c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279800"
---
# <a name="handling-review-feedback"></a>Administración de los comentarios de revisión

En este artículo se explica cómo acceder al entorno de Azure DevOps que usa el equipo de revisión de Microsoft Azure Marketplace. Si se detectan problemas críticos en su oferta de la aplicación de Azure durante el paso de **revisión de Microsoft**, puede iniciar sesión en este sistema para la información detallada sobre estos problemas (comentarios de revisión). Después de corregir todos estos problemas, debe volver a enviar la oferta para proceder a su publicación en Azure Marketplace. En el siguiente diagrama se ilustra cómo se relacionan estos comentarios con la publicación.

![Proceso de comentarios de revisión](./media/review-feedback-process.png)

Normalmente, a los problemas de revisión se hace referencia con solicitudes de incorporación de cambios (PR). Cada solicitud de incorporación de cambios está vinculada a un elemento de [Azure DevOps](https://azure.microsoft.com/services/devops/) (anteriormente llamado Visual Studio Team Services) con los detalles sobre el problema. La imagen siguiente muestra un ejemplo de la experiencia del Centro de partners si se detectan problemas durante las revisiones. 

![Estado de publicación](./media/publishing-status.png)

La solicitud de incorporación de certificados que contiene detalles específicos sobre el envío se mencionará en el vínculo "Ver informe de certificación". Para situaciones complejas, los equipos de revisión y soporte técnico también pueden escribir por correo electrónico.

## <a name="azure-devops-access"></a>Acceso de Azure DevOps

Todos los usuarios con acceso al rol "desarrollador" en el Centro de partners podrán ver los elementos de la solicitud de incorporación de cambios a los que se hace referencia en los comentarios de revisión.

<!---
To view the PR items referenced in review feedback, publishers must first be granted proper authorization. Otherwise, new publishers receive a 401 - Not Authorized response page when trying to view PRs. To request access to this Azure DevOps repository, perform the following steps:

1. Collect the following information:
    * Your publisher name and ID
    * Offer type (Azure app), offer name, and SKU ID
    * The pull request link, for example: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>`. This URL can be retrieved from the notification message or the address of the 401 response page.
    * The email address(es) of the individuals from your publishing organization that you want access granted to. This list should include the owner address(es) you provided when registering as a publisher on the Cloud Partner Portal (or Partner Center??).
1. Create a support incident. In the title bar of the Cloud Partner Portal (??), select the **Help** button, then choose **Support** from the menu. Your default web browser should launch and navigate to the Microsoft new support incident page. (You may have to sign in first.)
1. Specify the **Problem type** as **marketplace onboarding** and **Category** as **Access problem**, then select **Start request**.

    ![Create an incident](./media/create-an-incident.png)

1. In **Step 1 of 2** page, supply your contact information and select **Continue**.
1. In **Step 2 of 2** page, specify an incident title (for example Request Azure DevOps access) and supply the information you collected in the first step (above). Read and accept the agreement, then select **Submit**.

If the incident creation was successful, a confirmation page is displayed. Save the confirmation information on this page for your reference. The Microsoft Support Team should reply to your access request within a few business days. --->

## <a name="reviewing-the-pull-request"></a>Revisión de la solicitud de incorporación de cambios

Use el procedimiento siguiente para revisar los problemas que se documentan en la solicitud de incorporación de cambios.

1. En las secciones de **Revisión de Microsoft** del formulario Pasos de publicación, haga clic en un vínculo de solicitud de incorporación de cambios para iniciar el explorador e ir a la página **Información general** (página principal) de dicha solicitud. En la siguiente imagen se muestra un ejemplo de página principal para un problema crítico de una oferta de la aplicación Contoso. Esta página contiene información resumida útil acerca de los problemas de revisión que se detectaron en la aplicación de Azure.

    [![Página principal de la solicitud de incorporación de cambios](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)
    <br/> *Haga clic en la imagen para expandirla.*

1. (Opcional) En el lado derecho de la ventana, en la sección **Directivas**, haga clic en el mensaje emitido (en este ejemplo: **Policy Validation failed**) (Error durante la validación de directivas) para investigar los detalles de bajo nivel del problema, incluidos los archivos de registro asociados. Normalmente, los errores aparecen en la parte inferior de los archivos de registro.
1. En el menú en el lado izquierdo de la página principal, seleccione **Archivos** para mostrar la lista de archivos que componen los recursos técnicos de esta oferta. Los revisores de Microsoft deberían haber agregado comentarios que describen los problemas críticos detectados. En el ejemplo siguiente, se detectaron dos problemas.

    [![Página principal de la solicitud de incorporación de cambios](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)
    <br/> *Haga clic en la imagen para expandirla.*

1. Haga clic en cada nodo de comentario en el árbol de la izquierda para ver el comentario en el contexto del código circundante. Corrija el código fuente en el proyecto de equipo para corregir el problema que se describe en el comentario.

>[!Note]
>No puede editar los recursos técnicos de su oferta dentro del entorno de Azure DevOps del equipo de revisión. Para los publicadores, este es un entorno de solo lectura para el código fuente contenido. No obstante, puede dejar las respuestas a los comentarios para beneficio del equipo de revisión de Microsoft.

   En el ejemplo siguiente, el publicador ha revisado, corregido y respondido al primer problema.

   ![Primera respuesta de revisión y comentario](./media/first-comment-reply.png)

## <a name="next-steps"></a>Pasos siguientes

Después de corregir los problemas críticos que se documentan en las solicitudes de incorporación de cambios de la revisión, debe [volver a publicar la oferta de la aplicación de Azure](./create-new-azure-apps-offer.md#publish).
