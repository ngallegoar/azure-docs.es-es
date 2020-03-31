---
title: Modelo de facturación para Azure Active Directory B2C
description: Obtenga información sobre el modelo de facturación de usuarios activos mensuales (MAU) de Azure AD B2C y cómo habilitar la facturación para una suscripción de Azure específica.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/25/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 30ecb1e6faa29482a8d69dd1d08e4f127f515596
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190014"
---
# <a name="billing-model-for-azure-active-directory-b2c"></a>Modelo de facturación para Azure Active Directory B2C

El uso de Azure Active Directory B2C (Azure AD B2C) se factura a una suscripción de Azure vinculada y usa un modelo de facturación de usuarios activos mensuales (MAU). Obtenga información sobre cómo vincular un recurso de Azure AD B2C a una suscripción y cómo funciona el modelo de facturación MAU en las siguientes secciones.

> [!IMPORTANT]
> En este artículo no se incluye información sobre precios. Para conocer la información más reciente sobre la facturación del uso y los precios, consulte [Precios de Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="monthly-active-users-mau-billing"></a>Facturación de usuarios activos mensuales (MAU)

La facturación de Azure AD B2C se mide por el recuento de usuarios únicos con actividad de autenticación dentro de un mes natural, lo que se conoce como facturación de usuarios activos mensuales (MAU).

A partir del **1 de noviembre de 2019**, todos los inquilinos de Azure AD B2C recién creados se facturarán por usuarios activos mensuales (MAU). Los inquilinos existentes que se [vinculen a una suscripción](#link-an-azure-ad-b2c-tenant-to-a-subscription) a partir del 1 de noviembre de 2019 inclusive, se facturarán por usuarios activos mensuales (MAU).

Si un inquilino de Azure AD B2C existente se vinculó a una suscripción antes del 1 de noviembre de 2019, puede elegir una de las siguientes opciones:

* Actualizar al modelo de facturación mensual de usuarios activos (MAU).
* Permanecer en el modelo de facturación por autenticación.

### <a name="upgrade-to-monthly-active-users-billing-model"></a>Actualización al modelo de facturación de usuarios activos mensuales

Los propietarios de la suscripción de Azure que tienen acceso administrativo al recurso Azure AD B2C pueden cambiar al modelo de facturación MAU. Las opciones de facturación se configuran en el recurso de Azure AD B2C.

Este cambio al modo de facturación de usuarios activos mensuales (MAU) es **irreversible**. Después de convertir un recurso de Azure AD B2C al modelo de facturación basado en MAU, no se puede revertir al modelo de facturación por autenticación.

A continuación, se muestra cómo hacer el cambio a la facturación MAU para un recurso de Azure AD B2C existente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la suscripción.
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, seleccione el directorio de Azure AD B2C que quiere actualizar a la facturación MAU.<br/>
    ![Filtro de directorio y suscripción en Azure Portal](./media/billing/portal-mau-01-select-b2c-directory.png)
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. En la página **Información general** del inquilino de Azure AD B2C, seleccione el vínculo en **Nombre de recurso**. Se le dirigirá al recurso de Azure AD B2C en el inquilino de Azure AD.<br/>
    ![Vínculo de recurso de Azure AD B2C resaltado en Azure Portal](./media/billing/portal-mau-02-b2c-resource-link.png)
1. En la página **Información general** del recurso de Azure AD B2C, en **Unidades facturables**, seleccione el vínculo **Per Authentication (Change to MAU)** (Por autenticación [cambiar a MAU]).<br/>
    ![Vínculo para cambiar a MAU resaltado en Azure Portal](./media/billing/portal-mau-03-change-to-mau-link.png)
1. Seleccione **Confirmar** para completar la actualización a la facturación MAU.<br/>
    ![Cuadro de diálogo de confirmación de facturación basada en MAU en Azure Portal](./media/billing/portal-mau-04-confirm-change-to-mau.png)

### <a name="what-to-expect-when-you-transition-to-mau-billing-from-per-authentication-billing"></a>Qué esperar cuando se realiza la transición a la facturación MAU desde la facturación por autenticación

La medición basada en MAU se habilita en cuanto usted, el propietario de la suscripción o el recurso, confirma el cambio. La factura mensual reflejará las unidades de autenticación facturadas hasta el cambio y las nuevas unidades de MAU a partir del cambio.

Los usuarios no se cuentan dos veces durante el mes de la transición. A los usuarios activos únicos que se autentican antes del cambio se les cobra una tarifa por autenticación en un mes natural. Esos mismos usuarios no se incluyen en el cálculo de MAU durante el resto del ciclo de facturación de la suscripción. Por ejemplo:

* El inquilino de Contoso B2C tiene 1000 usuarios. 250 usuarios están activos en un mes determinado. El administrador de la suscripción cambia de la facturación por autenticación a usuarios activos mensuales (MAU) el décimo día del mes.
* La facturación del 1 al 10 usa el modelo por autenticación.
  * Si 100 usuarios inician sesión durante este período (del 1 al 10), esos usuarios se etiquetan como *mes pagado*.
* La facturación a partir del décimo día (el tiempo efectivo de transición) se factura con la tarifa MAU.
  * Si otros 150 usuarios inician sesión durante este período (del 10 al 30), solo se facturan los 150 adicionales.
  * La actividad continua de los primeros 100 usuarios no afecta a la facturación durante el resto del mes natural.

Durante el período de facturación de la transición, es probable que el propietario de la suscripción vea entradas para ambos métodos (por autenticación y por MAU) en el extracto de facturación de la suscripción de Azure:

* Una entrada para el uso hasta la fecha y hora del cambio que refleja la facturación por autenticación.
* Una entrada para el uso después del cambio que refleja los usuarios activos mensuales (MAU).

Para conocer la información más reciente sobre la facturación del uso y los precios de Azure AD B2C, consulte [Precios de Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="link-an-azure-ad-b2c-tenant-to-a-subscription"></a>Vincular un inquilino de Azure AD B2C a una suscripción

Los cargos por el uso de Azure Active Directory B2C (o Azure AD B2C) se facturan a una suscripción de Azure. Cuando se crea un inquilino de Azure AD B2C, es preciso que el administrador de inquilinos vincule explícitamente dicho inquilino a una suscripción de Azure.

El vínculo de la suscripción se consigue mediante la creación de un *recurso* de Azure AD B2C en la suscripción de Azure de destino. Se pueden crear varios recursos de Azure AD B2C en una sola suscripción de Azure, junto con otros recursos de Azure, como máquinas virtuales, cuentas de almacenamiento y Logic Apps. Para ver todos los recursos de la suscripción, vaya al inquilino de Azure Active Directory (Azure AD) al que está asociada la suscripción.

Se puede usar una suscripción vinculada a un inquilino de Azure AD B2C para la facturación del uso de Azure AD B2C u otros recursos de Azure, incluidos recursos adicionales de Azure AD B2C. La suscripción no se puede usar para agregar otros servicios basados en licencias de Azure o licencias de Office 365 en el inquilino de Azure AD B2C.

### <a name="prerequisites"></a>Prerrequisitos

* [Suscripción de Azure](https://azure.microsoft.com/free/)
* [Inquilino de Azure AD B2C](tutorial-create-tenant.md) que quiere vincular a una suscripción
  * Debe ser un administrador de inquilinos.
  * El inquilino no debe estar vinculado aún a una suscripción.

### <a name="create-the-link"></a>Crear el vínculo

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene la suscripción de Azure que quiere usar (*no* el directorio que contiene el inquilino de Azure AD B2C).
1. Seleccione **Crear un recurso**, escriba `Active Directory B2C` en el campo **Buscar en Marketplace** y, a continuación, seleccione **Azure Active Directory B2C**.
1. Seleccione **Crear**
1. Seleccione **Vincular un inquilino de Azure AD B2C existente a mi suscripción de Azure**.
1. Seleccione **Inquilino de Azure AD B2C** en la lista desplegable. Solo se muestran los inquilinos para los que es administrador global y que aún no están vinculados a una suscripción. El campo **Nombre del recurso de Azure AD B2C** se rellena con el nombre de dominio del inquilino de Azure AD B2C que seleccione.
1. Seleccione una **Suscripción** activa de Azure de la que sea administrador.
1. En **Grupo de recursos**, seleccione **Crear nuevo** y, luego, especifique la **Ubicación del grupo de recursos**. Aquí la configuración del grupo de recursos no tiene ningún efecto en la ubicación, el rendimiento o el estado de facturación del inquilino de Azure AD B2C.
1. Seleccione **Crear**.
    ![Página de creación de recursos de Azure AD B2C en Azure Portal](./media/billing/portal-01-create-b2c-resource-page.png)

Después de completar estos pasos para un inquilino de Azure AD B2C, la suscripción de Azure se factura según los detalles del Contrato Enterprise o de Azure Direct.

### <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Administración de los recursos del inquilino de Azure AD B2C

Después de crear el recurso de Azure AD B2C en una suscripción de Azure, debería ver un nuevo recurso de tipo "Inquilino B2C" junto con los otros recursos de Azure.

Este recurso se puede utilizar para:

* Navegar hasta la suscripción para consultar la información de facturación.
* Obtener el id. de inquilino del inquilino de Azure AD B2C en formato GUID.
* Ir al inquilino de Azure AD B2C.
* Enviar una solicitud de soporte técnico.
* Mover el recurso del inquilino de Azure AD B2C a otra suscripción de Azure o grupo de recursos.

![Página de configuración de recursos de B2C en Azure Portal](./media/billing/portal-02-b2c-resource-overview.png)

### <a name="regional-restrictions"></a>Restricciones regionales

Si ha establecido restricciones regionales para la creación de recursos de Azure en su suscripción, esta restricción puede impedir que cree el recurso de Azure AD B2C.

Para mitigar este problema, modere sus restricciones regionales.

## <a name="azure-cloud-solution-providers-csp-subscriptions"></a>Suscripciones a Azure Cloud Solution Providers (CSP)

Las suscripciones a Azure Cloud Solution Providers (CSP) son compatibles con Azure AD B2C. La funcionalidad está disponible mediante API o Azure Portal para Azure AD B2C y para todos los recursos de Azure. Los administradores de suscripción de CSP pueden vincular, mover y eliminar relaciones con Azure AD B2C de la misma manera que se hace con otros recursos de Azure.

La administración de Azure AD B2C mediante el control de acceso basado en rol no se ve afectada por la asociación entre el inquilino de Azure AD B2C y una suscripción a Azure CSP. El control de acceso basado en rol se logra mediante el uso de roles basados en inquilinos, no roles basados en suscripciones.

## <a name="change-the-azure-ad-b2c-tenant-billing-subscription"></a>Cambio de la suscripción de facturación del inquilino de Azure AD B2C

Los inquilinos de Azure AD B2C se pueden mover a otra suscripción si la de origen y la de destino existen dentro del mismo inquilino de Azure Active Directory.

Para aprender a mover recursos como el inquilino de Azure AD B2C a otra suscripción, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/management/move-resource-group-and-subscription.md).

Antes de iniciar el traslado, no olvide leer el artículo completo para comprender las limitaciones y los requisitos de esta operación. Además de las instrucciones para mover los recursos, se incluye información crítica, como una lista de comprobación previa al traslado y cómo validar la operación de traslado.

## <a name="next-steps"></a>Pasos siguientes

Para obtener la información de precios más reciente, consulte [Precios de Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
