---
title: Actualización de una oferta existente de Marketplace comercial
description: Cómo realizar actualizaciones en una oferta de Marketplace comercial existente, lo que incluye editar y eliminar un borrador, cancelar una solicitud de publicación, detener la venta de una oferta o plan, y sincronizar audiencias privadas.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/16/2020
ms.openlocfilehash: 8b8362a4d5ad742f29637ed03133bc6983bd5dd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285153"
---
# <a name="update-an-existing-offer-in-the-commercial-marketplace"></a>Actualización de una oferta existente en Marketplace comercial

Puede ver las ofertas existentes en la pestaña **Información general** del [portal de Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) en Centro de partners.

Para actualizar una oferta existente que actualmente está en directo en Marketplace:

1. Seleccione el nombre de la oferta que desea actualizar. El estado de la oferta puede aparecer como **Activa**, **Publicación en curso**, **Borrador**, **Se necesita atención**, o **Not being sold in the marketplace** (No está a la venta en Marketplace), si ya había elegido con anterioridad detener la venta de la oferta. Una vez seleccionado, se abrirá la página **Información general de la oferta** para dicha oferta.
2. Seleccione **Actualizar** en la tarjeta de información general o el elemento en el menú de navegación izquierdo para el área que desea actualizar. Puede que desee actualizar alguno de estos elementos: **Configuración de la oferta**, **Propiedades**, **Descripción de la oferta**, **Vista previa**, **Configuración técnica** , **Información general del plan** o **Versión de prueba**. 
3. Realice los cambios y, después seleccione **Guardar**. Repita este proceso hasta que completar todos los cambios.

## <a name="review-and-publish-an-updated-offer"></a>Revisión y publicación de una oferta actualizada

Cuando esté listo para publicar la oferta actualizada, seleccione **Publicar** desde cualquier página. Se abrirá la página **Review and publish** (Revisar y publicar). En esta página puede realizar las siguientes acciones:

- Consultar el estado de finalización de las secciones de la oferta que ha actualizado: 
    - **Cambios sin publicar**: La sección se ha actualizado y completado. Se han proporcionado todos los datos necesarios y no ha habido errores en las actualizaciones.
    - **Incompletos**: Las actualizaciones realizadas en la sección han introducido errores que tienen que corregirse o es necesario proporcionar más información.
- Proporcionar información adicional al equipo de pruebas de certificación para asegurarse de que las pruebas se realizan sin problemas.
- Enviar la oferta actualizada para la publicación seleccionando **Enviar**.  Le enviaremos un correo electrónico cuando haya alguna versión preliminar de la oferta actualizada disponible para su revisión y aprobación.

> [!IMPORTANT]
> Debe revisar la versión preliminar de la oferta una vez que esté disponible y seleccionar **Transmitir** para publicar su oferta actualizada para los destinatarios previstos (públicos o privados).

## <a name="update-a-plan-within-an-existing-offer"></a>Actualización de un plan dentro de una oferta existente

Para realizar cambios en un plan dentro de una oferta existente que ya haya publicado:

- Con la página **Información general de la oferta** abierta para la oferta específica, elija el plan que desee cambiar. Si el plan no es accesible desde la lista **Información general del plan**, seleccione **Ver todos los planes**.
- Puede elegir seleccionar el **Nombre**, **Modelo de precios** o **Disponibilidad**. *En la actualidad los planes solo están disponibles en inglés (Estados Unidos)* .
- Seleccione **Guardar** después de realizar los cambios necesarios en el nombre, descripción, precios o disponibilidad para la audiencia del plan. 
- Seleccione **Publicar** cuando esté listo para publicar las actualizaciones. Se abrirá la página **[Review and publish](#review-and-publish-an-updated-offer)** (Revisar y publicar) proporcionando un estado de finalización para las actualizaciones.

## <a name="compare-changes-to-marketplace-offers"></a>Comparación de cambios en las ofertas de Marketplace

Puede auditar los cambios que realice en una oferta [publicada](#compare-changes-to-published-offer) o de [versión preliminar](#compare-changes-to-a-preview-offer) antes de que estén en directo utilizando **Compare** (Comparar).

>[!NOTE]
> Una oferta publicada es una oferta que se ha publicado correctamente en estado Versión preliminar o En directo.

A continuación encontrará información general sobre auditorías:

- Puede usar **Compare** (Comparar) en cualquier momento del proceso de edición.
- Seleccione un campo en la página **Compare** (Comparar) para ir al valor que desea modificar.
- Si está listo para publicar sus actualizaciones, seleccione **Review and publish** (Revisar y publicar).
- Para ver los valores de todos los campos, hasta de los que no están actualizados, seleccione el filtro **Todos los campos**. Para modificar los filtros de estos campos, seleccione **Modified fields** (Campos modificados) y, después, seleccione uno de los siguientes filtros:
    - El filtro **Removed values** (Valores eliminados) muestra los campos que ha publicado y que está eliminando completamente.
    - El filtro **Added values** (Valores agregados) muestra los campos que no publicó originalmente y agrega ahora.
    - El filtro **Edited values** (Valores editados) muestra campos que se habían publicado, pero ahora ha actualizado el contenido.

      >[!NOTE]
      > Si alguno de estos filtros no está disponible, indica que no ha realizado ninguna actualización de ese tipo.

- Para ver solo los valores que no se han actualizado, seleccione el campo **Unchanged fields** (Campos sin cambios). Los valores de los campos que se muestran para la versión de borrador y la publicada serán los mismos.

  ![Los filtros para comparar las actualizaciones de una oferta publicada o de versión preliminar](./media/compare-changes-marketplace.png)

>[!NOTE]
> Las siguientes páginas no admiten actualmente **Compare** (Comparar):
>- Audiencia de revendedores de CSP
>- Configuración técnica de la versión de prueba
>- Listas de Marketplace de la versión de prueba
>- Venta conjunta
>- Archivos complementarios

### <a name="compare-changes-to-published-offer"></a>Comparación de cambios en ofertas publicadas

Siga las instrucciones que aparecen a continuación para comparar los cambios de la oferta publicada:

1. Seleccione **Compare** (Comparar) en la barra de comandos de la página. La página **Compare** (Comparar) proporciona versiones en paralelo de los cambios guardados de esta oferta y de la oferta de Marketplace publicada.
2. Si se accede a **Compare** (Comparar) desde una página concreta de la oferta, la predeterminado muestra solo los cambios que se han realizado en esa página.
    - Si desea comparar los cambios en todas las páginas, cambie la página en **Seleccione una página que comparar**.  
    - Si desea comparar los cambios realizados en la oferta en todas las páginas, seleccione **Todas las páginas**.

Al ser la predeterminada, **Compare** (Comparar) compara los nuevos cambios en la oferta de Marketplace activa.

Recuerde volver a publicar la oferta después de realizar actualizaciones para que los cambios surtan efecto.

### <a name="compare-changes-to-a-preview-offer"></a>Comparación de cambios en una oferta de versión preliminar

Si tiene cambios en versión preliminar que no están activos, puede comparar los cambios nuevos con la versión preliminar de la oferta de Marketplace.

Siga las instrucciones que se indican a continuación para comparar los cambios nuevos con la versión preliminar de la oferta de Marketplace:

1. Seleccione **Compare** (Comparar) en la barra de comandos de la página.
2. Seleccione la lista desplegable **Con** y cambie la opción de **Live offer** (Oferta en directo) a **Preview offer** (Oferta en versión preliminar).
3. La página **Compare** (Comparar) proporciona versiones en paralelo que muestran los cambios.

>[!NOTE]
>Si la oferta aún no está activa, pero la ha publicado en versión preliminar, no tiene la opción de compararla con una oferta activa.

Recuerde volver a publicar la oferta después de realizar actualizaciones para que los cambios surtan efecto.

## <a name="delete-a-draft-offer"></a>Eliminación de una oferta borrador

Puede eliminar una oferta borrador (una que no se ha publicado), seleccione **Eliminar borrador** en la página **Información general de la oferta**. Esta opción no estará disponible si ha publicado previamente la oferta.

Después de confirmar que desea eliminar la oferta borrador, la oferta ya no estará visible ni accesible en Centro de partners y se abrirá la página **Todas las ofertas**.

## <a name="delete-a-draft-plan"></a>Eliminación de una plan borrador

Para eliminar un plan que no se ha publicado, seleccione **Eliminar borrador** desde la página **Información general del plan**. Esta opción no estará disponible si ha publicado previamente la oferta.

Después de confirmar que desea eliminar el plan borrador, el plan ya no estará visible ni accesible en Centro de partners.

## <a name="cancel-publishing"></a>Cancelación de la publicación

Para cancelar una oferta con el estado **Publicación en curso**:

- Seleccione el nombre de la oferta para abrir la página **Información general de la oferta**.
- Seleccione **Cancelar publicación** en la esquina superior derecha de la página.
- Confirme que desea detener la publicación de la oferta.

Si desea publicar la oferta más adelante, tendrá que volver a empezar el proceso de publicación.

> [!NOTE]
> La publicación de una oferta solo se puede detener si esta aún no ha llegado al paso de aprobación por parte del publicador. Después de seleccionar **Publicar** ya no tendrá la opción de cancelar la publicación.

## <a name="stop-selling-an-offer-or-plan"></a>Detención de la venta de una oferta o un plan

Para detener la venta de una oferta después de que ya se haya publicado, seleccione **Dejar de vender** desde la página **Información general de la oferta**. Para detener la venta de un plan, seleccione **Dejar de vender** desde la página **Información general del plan**. (La opción de detener la venta de un plan solo está disponible si tiene más de un plan en la oferta. Puede elegir detener la venta de un plan sin afectar a otros planes dentro de la oferta).

En el plazo de unas pocas horas después de confirmar que desea detener la venta de la oferta o del plan, estos ya no estarán visible en Marketplace y ningún nuevo cliente podrá descargarlos.

Los clientes que hayan adquirido anteriormente la oferta o el plan, podrán seguir usándolos. Pueden volver a descargarlos, pero no tendrán actualizaciones si más adelante actualiza y vuelve a publicar la oferta o el plan. 

Una vez completada la solicitud para detener la venta de la oferta o el plan, podrá verlo aún en el portal de Marketplace comercial en Centro de partners. Para eliminar el borrador, seleccione **Eliminar borrador** desde las páginas **Información general de la oferta** o **Información general del plan**. 

Si decide volver a mostrar o vender esta oferta o este plan, siga las instrucciones para [actualizar una oferta existente](#update-an-existing-offer-in-the-commercial-marketplace). No olvide que después de realizar cualquier cambio tiene que volver a **publicar** la oferta o el plan.

## <a name="remove-offers-from-existing-customers"></a>Eliminación de ofertas de clientes existentes

Para eliminar ofertas de los clientes existentes, [presente una solicitud de soporte técnico](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff). Seleccione el tipo de oferta específico en la lista **Tipo de problema** y luego seleccione **Remove a published offer** (Quitar una oferta publicada) en la lista **Categoría**. Envíe la solicitud. El equipo de soporte técnico le guiará en el proceso de eliminación de la oferta.

## <a name="sync-private-plan-audiences"></a>Sincronización de audiencias de plan privado

Si su oferta incluye uno o varios planes que están configurados para que solo estén disponibles a una audiencia privada restringida, puede actualizar solo la audiencia que tenga acceso a ese plan privado sin publicar otros cambios en la oferta. 

Para actualizar y sincronizar la audiencia privada de sus planes:

- Modifique la audiencia en uno o varios planes privados mediante el botón + **Agregar un ID** o **Importar clientes (csv)** y, a continuación, guarde los cambios.
- Seleccione **Sync private audience** (Sincronizar audiencia privada) desde la página **Información general del plan**.

**Sync private audience** (Sincronizar audiencia privada) publicará solo los cambios para sus audiencias privadas, sin publicar ninguna otra actualización que haya podido realizar en la oferta borrador.

## <a name="next-steps"></a>Pasos siguientes

- [Comprobación del estado de publicación de una oferta de Marketplace comercial](./publishing-status.md)
