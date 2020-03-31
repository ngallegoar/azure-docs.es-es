---
title: Creación de una cuenta en Azure Portal (Azure Batch) | Microsoft Docs
description: Aprenda a crear una cuenta de Azure Batch en Azure Portal para ejecutar cargas de trabajo paralelas a gran escala en la nube
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3723631609a04f6d12abcaac1f9d7733bf3caa01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225984"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Creación de una cuenta de Batch con Azure Portal

Aprenda a crear una cuenta de Azure Batch en [Azure Portal][azure_portal] y seleccione las propiedades de la cuenta más adecuadas para su escenario de proceso. Aprenda dónde encontrar importantes propiedades de cuenta, como teclas de acceso y direcciones URL de la cuenta.

Para más información acerca de los escenarios y las cuentas de Batch, consulte la [descripción de las características](batch-api-basics.md).

## <a name="create-a-batch-account"></a>Crear una cuenta de Batch

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. Inicie sesión en [Azure Portal][azure_portal].

1. Seleccione **Crear un recurso** > **Proceso** > **Servicio de Batch**.

    ![Batch en Marketplace][marketplace_portal]

1. Especifique los detalles de la opción **Nueva cuenta de Batch**. Vea los siguientes detalles.

    ![Crear una cuenta de Batch][account_portal]

    a. **Suscripción**: en la que se crea la cuenta de Batch. Si tiene una sola suscripción, se selecciona de forma predeterminada.

    b. **Grupo de recursos**: seleccione un grupo de recursos existente para la nueva cuenta de Batch (también se puede crear uno nuevo).

    c. **Nombre de cuenta**: el nombre que elija debe ser único en la región de Azure en la que se crea cuenta (consulte **Ubicación** a continuación). El nombre de la cuenta solo puede contener caracteres en minúsculas o números, y su longitud debe oscilar entre 3 y 24 caracteres.

    d. **Ubicación**: región de Azure en la que se va a crear la cuenta de Batch. Solo se muestran como opciones las regiones admitidas por su suscripción y grupo de recursos.

    e. **Cuenta de almacenamiento**: una cuenta de Azure Storage opcional que asocia a su cuenta de Batch. Para que el rendimiento sea óptimo, se recomienda una cuenta de almacenamiento de uso general v2. Para conocer todas las opciones de la cuenta de almacenamiento de Batch, consulte la [introducción a las características de Batch](batch-api-basics.md#azure-storage-account). En el portal, seleccione una cuenta de almacenamiento existente o cree una.

      ![Crear una cuenta de almacenamiento][storage_account]

    f. **Modo de asignación de grupos**: En la pestaña de configuración **Avanzado** puede especificar el modo de asignación de grupo como **Servicio de Batch** o **Suscripción de usuario**. para la mayoría de los escenarios, acepte el **Servicio de Batch** predeterminado.

      ![Modo de asignación de grupos de Batch][pool_allocation]

1. Seleccione **Crear** para crear la cuenta.

## <a name="view-batch-account-properties"></a>Visualización de propiedades de la cuenta de Batch

Una vez creada la cuenta, selecciónela para acceder a su configuración y propiedades. Con las opciones del menú izquierdo puede acceder a todas las propiedades y a la configuración de la cuenta.

![Página de la cuenta de Batch en Azure Portal][account_blade]

* **Nombre de la cuenta, dirección URL y claves de Batch**: al desarrollar una aplicación con las [API de Batch](batch-apis-tools.md#azure-accounts-for-batch-development), se necesita una dirección URL de la cuenta y una clave para acceder a los recursos de Batch. (Batch también admite la autenticación de Azure Active Directory.)

    Para ver la información de acceso de la cuenta de Batch, seleccione **Claves**.

    ![Claves de la cuenta de Batch en Azure Portal][account_keys]

* Para ver el nombre y las claves de la cuenta de almacenamiento asociada con la cuenta de Batch, seleccione **Cuenta de almacenamiento**.

* Para ver las cuotas de recursos que se aplican a la cuenta de Batch, seleccione **Cuotas**. Para más información, consulte [Límites y cuotas del servicio Batch](batch-quota-limit.md).

## <a name="additional-configuration-for-user-subscription-mode"></a>Configuración adicional para el modo de suscripción de usuario

Si decide crear una cuenta de Batch en el modo de suscripción de usuario, antes, realice los siguientes pasos adicionales.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Procedimiento para permitir que Azure Batch acceda a la suscripción (operación única)

Al crear la primera cuenta de Batch en el modo de suscripción de usuario, tiene que registrar la suscripción con Batch. (si ya lo ha hecho anteriormente, vaya a la sección siguiente).

1. Inicie sesión en [Azure Portal][azure_portal].

1. Seleccione **Todos los servicios** > **Suscripciones** y seleccione la suscripción que desea usar para la cuenta de Batch.

1. En la página **Suscripción**, seleccione **Proveedores de recursos** y busque **Microsoft.Batch**. Compruebe que el proveedor de recursos **Microsoft.Batch** está registrado en la suscripción. Si no lo está, seleccione el vínculo **Registrar**.

    ![Registro del proveedor Microsoft.Batch][register_provider]

1. En la página **Suscripción**, seleccione **Control de acceso (IAM)**  > **Asignaciones de rol** > **Agregar asignación de rol**.

    ![Control de acceso a la suscripción][subscription_access]

1. En la página **Agregar asignación de rol**, seleccione el rol **Colaborador** y busque Batch API. Busque cada una de estas cadenas hasta que encuentre la API:
    1. **MicrosoftAzureBatch**.
    1. **Microsoft Azure Batch**. Los inquilinos más recientes de Azure AD pueden utilizar este nombre.
    1. **ddbf3205-c6bd-46ae-8127-60eb93363864** es el identificador de Batch API.

1. Una vez que encuentre Batch API, selecciónela y elija **Guardar**.

    ![Adición de permisos de Batch][add_permission]

### <a name="create-a-key-vault"></a>Creación de un Almacén de claves

En el modo de suscripción de usuario, se requiere una instancia de Azure Key Vault que pertenezca al mismo grupo de recursos que la cuenta de Batch que se va a crear. Asegúrese de que el grupo de recursos se encuentre en una región en la que Batch esté [disponible](https://azure.microsoft.com/regions/services/) y que la suscripción admita.

1. En [Azure Portal][azure_portal], seleccione **Nuevo** > **Seguridad** > **Key Vault**.

1. En la página **Crear Key Vault**, escriba un nombre para el almacén de claves y cree un grupo de recursos en la región que desee para su cuenta de Batch. Deje los valores predeterminados para el resto de la configuración y, a continuación, seleccione **Crear**.

Al crear la cuenta de Batch en modo de suscripción de usuario, use el grupo de recursos del almacén de claves. Especifique **Suscripción de usuario** como el modo de asignación del grupo, seleccione el almacén de claves y active la casilla para conceder acceso a Azure Batch al almacén de claves. 

Si prefiere conceder acceso al almacén de claves manualmente, vaya a la sección **Directivas de acceso** de dicho almacén, seleccione **Agregar directivas de acceso** y busque **Microsoft Azure Batch**. Una vez seleccionado, deberá configurar los **Permisos de secretos** en el menú desplegable. A Azure Batch se le deben conceder los permisos mínimos **Get**, **List**, **Set** y **Delete**.

![Permisos de secretos de Azure Batch](./media/batch-account-create-portal/secret-permissions.png)


> [!NOTE]
> Asegúrese de que las casillas de verificación **Azure Virtual Machines para implementación** y **Azure Resource Manager para implementación de plantillas** están activadas en **Directivas de acceso** para el recurso de **Key Vault** vinculado.
> 
> ![Directiva de acceso Key Vault obligatorio](./media/batch-account-create-portal/key-vault-access-policy.png) No es obligatorio cuando se crea una cuenta de Batch en Azure Portal. La opción está seleccionada de forma predeterminada.



### <a name="configure-subscription-quotas"></a>Configuración de cuotas de suscripción

No se establecen las cuotas de núcleos de forma predeterminada en las cuentas de Batch de suscripción de usuario. Las cuotas de los núcleos se deben establecer de manera manual, ya que las cuotas de núcleos de Batch estándar no se aplican a las cuentas que estén en modo de suscripción del usuario.

1. En [Azure Portal][azure_portal], seleccione la cuenta de Batch del modo de suscripción del usuario para mostrar sus propiedades y configuración.

1. En el menú izquierdo, seleccione **Cuotas** para ver y configurar las cuotas de núcleos asociadas a su cuenta de Batch.

Para más información acerca de las cuotas de núcleos del modo de suscripción del usuario, consulte [Límites y cuotas del servicio Batch](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Otras opciones de administración de la cuenta de Batch

Además de usar Azure Portal, las cuentas de Batch se pueden crear y administrar con las siguientes herramientas:

* [Cmdlets de PowerShell de Batch](batch-powershell-cmdlets-get-started.md)
* [CLI de Azure](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de los conceptos y las características del servicio de Batch, consulte la [descripción de las características de Batch](batch-api-basics.md). El artículo describe los principales recursos de Batch como grupos, nodos de proceso, trabajos y tareas, y proporciona información general acerca de las características del servicio específicas para las cargas de trabajo de proceso a gran escala.
* Para conocer los aspectos básicos del desarrollo de una aplicación habilitada para Batch, consulte la [biblioteca de cliente de Batch para .NET](quick-run-dotnet.md) o [Python](quick-run-python.md). Estas guías de inicio rápido le guían a través de una aplicación de ejemplo que usa el servicio de Batch para ejecutar una carga de trabajo en varios nodos de proceso e incluye el uso de Azure Storage para el almacenamiento provisional y la recuperación del archivo de la carga de trabajo.

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[marketplace_portal]: ./media/batch-account-create-portal/marketplace-batch.png
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[pool_allocation]: ./media/batch-account-create-portal/batch-pool-allocation.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[register_provider]: ./media/batch-account-create-portal/register_provider.png
