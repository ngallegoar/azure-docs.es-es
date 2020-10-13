---
title: Obtención de soporte técnico para el portal de marketplace comercial en el Centro de partners
description: Obtenga información acerca de las opciones de soporte técnico en el Centro de partners, incluida la forma de presentar una solicitud de soporte técnico.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 09/18/2020
ms.openlocfilehash: d69001922520fc7162071b4828492bb1d9f80d0a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91330024"
---
# <a name="support-for-the-commercial-marketplace-program-in-partner-center"></a>Soporte técnico para el programa de marketplace comercial en el Centro de partners

Microsoft ofrece soporte para una amplia variedad de productos y servicios. Encontrar el equipo de soporte técnico correcto es importante para garantizar una respuesta oportuna y adecuada. Considere los siguientes escenarios que le ayudarán a dirigir su consulta al equipo adecuado:

- Si es anunciante y tiene una pregunta de un cliente, indíquele que solicite soporte técnico a través de los vínculos de soporte técnico en  [Azure Portal](https://portal.azure.com/).

- Si es anunciante y tiene una pregunta relacionada con la aplicación o el servicio, revise las siguientes opciones de soporte técnico.

## <a name="support-options-for-publishers"></a>Opciones de soporte técnico para los anunciantes

1. Inicie sesión en el [programa Marketplace comercial en el Centro de partners](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) con su cuenta profesional. (Si aún no lo ha hecho, tendrá que [crear una cuenta del Centro de partners](./create-account.md)).

2. En el menú superior del lado derecho de la página, seleccione el icono **Soporte técnico**.
 
3. El panel **Ayuda y soporte técnico** aparecerá el panel desde el lado derecho de la página.
 
   ![Menú desplegable de soporte técnico](./media/support/commercial-marketplace-support-pane.png)

    También puede ir al panel **Página principal** y seleccionar **Ayuda y soporte técnico**.

   ![Ayuda y soporte técnico desde la página principal](./media/support/homepage-help-support.png)

4. Seleccione **[Documentación](../index.yml)** para revisar las respuestas completas a preguntas y recursos.

5. Seleccione **[Foro de la Comunidad de partners de Marketplace](https://www.microsoftpartnercommunity.com/t5/Azure-Marketplace-and-AppSource/bd-p/2222)** para responder sus preguntas aprovechando el conocimiento de otros anunciantes de Microsoft.

6. Seleccione **[Ayuda adicional](https://aka.ms/marketplacepublishersupport)** para abrir un vale **Nueva solicitud de soporte técnico**.  

## <a name="how-to-open-a-support-ticket"></a>Cómo abrir una incidencia de soporte técnico

Ahora está listo para abrir una incidencia de soporte técnico en la pantalla **Ayuda y soporte técnico**.

![Ayuda y soporte técnico](./media/support/help-and-support.png)

>[!Note]
>Si ha iniciado sesión en el Centro de partners, la experiencia de soporte técnico será mejor.

**Opción 1:** Escriba palabras clave como: Marketplace, aplicación de Azure, oferta de SaaS, administración de cuentas, administración de clientes potenciales, problema de implementación, pago, etc.

**Opción 2:** Examine temas -> seleccione **Categoría** = Marketplace comercial -> seleccione el **tema** adecuado y, a continuación, el **subtema**.

Una vez que haya encontrado el tema que desea, seleccione **Review Solutions** (Revisar soluciones).

![Paso siguiente](./media/support/next-step.png)

Las opciones siguientes estarán disponibles:

* Para seleccionar otro tema, seleccione un vínculo de tema diferente en **selected issue** (problema seleccionado).
* Revise la descripción de este problema, si está disponible.  Es el texto que se muestra sobre los **pasos recomendados**.
* Revise los **pasos recomendados**, si están disponibles.
* Revise los **documentos recomendados**, si están disponibles.

![Soluciones recomendadas](./media/support/recommended-solutions.png)

En caso de que no encuentre la respuesta que busca en las **soluciones recomendadas**, haga clic en **provide issue details** (proporcionar detalles del problema).  Rellene el formulario y todos los campos obligatorios para ayudar a acelerar el proceso de resolución.  A continuación, haga clic en **Enviar**.

>[!Note]
>Si no ha iniciado sesión en el Centro de partners y el tema requiere autenticación, se le pedirá que inicie sesión para poder continuar.  Para los temas públicos, no se requiere autenticación.

## <a name="track-your-existing-support-requests"></a>Realizar un seguimiento de las solicitudes de soporte técnico existentes

Para revisar todas sus incidencias abiertas y cerradas, vaya a **Marketplace comercial** en la barra de navegación izquierda y, a continuación, seleccione **Soporte técnico**.

## <a name="record-issue-details-with-a-har-file"></a>Registro de los detalles del problema con un archivo HAR

Para ayudar a los agentes de soporte técnico a solucionar el problema, considere la posibilidad de adjuntar un archivo con formato de almacenamiento HTTP (HAR) a la incidencia de soporte técnico. HAR files son registros de solicitudes de red en un explorador web.

> [!WARNING]
> Los archivos HAR pueden registrar datos confidenciales acerca de su cuenta del Centro de partners.

### <a name="microsoft-edge-and-google-chrome"></a>Microsoft Edge o Google Chrome

Para generar un archivo HAR mediante **Microsoft Edge** o **Google Chrome**:

1. Vaya a la página web donde está experimentando el problema.
2. En la esquina superior derecha de la ventana, seleccione el icono de puntos suspensivos y, a continuación, **Más herramientas** > **Herramientas de desarrollo**. Puede presionar F12 como método abreviado de teclado.
3. En el panel Herramientas de desarrollo, seleccione la pestaña **Red**.
4. Seleccione **Stop recording network log** (Dejar de grabar el registro de red) y **Borrar** para quitar los registros existentes. El icono de registro cambiará a gris.

    ![Cómo quitar registros existentes en Microsoft Edge o Google Chrome](media/support/chromium-stop-clear-session.png)

5. Seleccione **Record network log** (Grabar el registro de red) para iniciar la grabación. Al iniciar la grabación, el icono de registro se volverá rojo.
 
    ![Cómo iniciar la grabación en Microsoft Edge o Google Chrome](media/support/chromium-start-session.png)

6. Reproduzca el problema que desea solucionar.
7. Una vez que haya reproducido el problema, seleccione **Stop recording network log** (Dejar de grabar el registro de red).
8. Seleccione **Exportar HAR**, marcado con un icono de flecha hacia abajo y guarde el archivo.

    ![Cómo exportar un archivo HAR en Microsoft Edge o Google Chrome](media/support/chromium-network-export-har.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

Para generar un archivo HAR mediante **Mozilla Firefox**:

1. Vaya a la página web donde está experimentando el problema.
1. En la esquina superior derecha de la ventana, seleccione el icono de puntos suspensivos y, a continuación, **Desarrollador web** > **Alternar herramientas**. Puede presionar F12 como método abreviado de teclado.
1. Seleccione la pestaña **Red** y **Limpiar** para quitar los registros existentes.
 
    ![Cómo quitar los registros existentes en Mozilla Firefox](media/support/firefox-clear-session.png)
 
1. Reproduzca el problema que desea solucionar.
1. Una vez que haya reproducido el problema, seleccione la opción de **exportación o importación de HAR** > **Guardar todo como HAR**.
 
    ![Cómo exportar un archivo HAR en Mozilla Firefox](media/support/firefox-network-export-har.png)

### <a name="apple-safari"></a>Apple Safari

Para generar un archivo HAR mediante **Safari**:

1. Habilite las herramientas de desarrollo en Safari: seleccione **Safari** > **Preferencias**. Diríjase a la pestaña **Avanzadas** y, a continuación, seleccione **Mostrar menú Desarrollar en la barra de menús**.
1. Vaya a la página web donde está experimentando el problema.
1. Seleccione **Desarrollar** y, a continuación, seleccione **Mostrar inspector web**.
1. Seleccione la pestaña **Red** y **Borrar elementos de red** para quitar los registros existentes.
 
    ![Cómo quitar los registros existentes en Safari](media/support/safari-clear-session.png)
 
1. Reproduzca el problema que desea solucionar.
1. Una vez que haya reproducido el problema, seleccione **Exportar** y guarde el archivo.
 
    ![Cómo exportar un archivo HAR en Safari](media/support/safari-network-export-har.png)

## <a name="next-steps"></a>Pasos siguientes

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
