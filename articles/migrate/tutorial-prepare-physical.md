---
title: Preparación de servidores físicos para la evaluación y migración con Azure Migrate
description: Aprenda a prepararse para la evaluación y migración de servidores físicos con Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 42eb603be0152b9e8cfb36d02e8f0602c40afe54
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77031210"
---
# <a name="prepare-for-assessment-and-migration-of-physical-servers-to-azure"></a>Preparación de la evaluación y migración de servidores físicos a Azure

En este artículo se indica cómo prepararse para la evaluación de servidores físicos en el entorno local con [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayuda a detecta las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 

Este tutorial es el primero de una serie que muestra cómo evaluar servidores físicos con Azure Migrate. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Prepare Azure. Configure los permisos de la cuenta y los recursos de Azure para poder usar Azure Migrate.
> * Preparar los servidores físicos en el entorno local para la evaluación de servidores.


> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para instrucciones detalladas, consulte los procedimientos para la evaluación de servidores físicos.


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

### <a name="azure-permissions"></a>Permisos de Azure

Necesita configurar permisos para la implementación de Azure Migrate.

**Task** | **Permisos**
--- | ---
**Crear un proyecto de Azure Migrate** | La cuenta de Azure necesita permisos para crear un proyecto.
**Registrar el dispositivo de Azure Migrate** | Azure Migrate usa un dispositivo ligero para detectar y evaluar servidores físicos con Azure Migrate Server Assessment. Este dispositivo realiza la detección de servidores y envía sus metadatos y sus datos de rendimiento a Azure Migrate.<br/><br/>Durante el registro del dispositivo, los siguientes proveedores de recursos se registran con la suscripción elegida en el dispositivo: Microsoft.OffAzure, Microsoft.Migrate y Microsoft.KeyVault. Al registrar un proveedor de recursos se configura la suscripción para que funcione con este. Para registrar los proveedores de recursos debe tener el rol colaborador o propietario de la suscripción.<br/><br/> Como parte del proceso de incorporación, Azure Migrate crea una aplicación de Azure Active Directory (Azure AD):<br/> Esta aplicación se usa para la comunicación (autenticación y autorización) entre los agentes que se ejecutan en el dispositivo con sus servicios respectivos que se ejecutan en Azure. Esta aplicación no tiene privilegios para realizar llamadas a ARM ni acceso RBAC en ningún recurso.



### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

Compruebe que tenga permiso para crear un proyecto de Azure Migrate.

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.


### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Puede asignar permisos para Azure Migrate con el fin de crear las aplicaciones de Azure AD durante el registro del dispositivo mediante uno de los métodos siguientes:

- Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
- Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

> [!NOTE]
> - Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
> - Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo.


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

El administrador de inquilinos o global puede conceder permisos como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**.

    ![Permisos de Azure AD](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones

El administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-physical-server-assessment"></a>Preparación de la valoración de servidores físicos

Para preparar la valoración de los servidores físicos, tiene que comprobar su configuración así como la de la implementación de los dispositivos.

### <a name="verify-physical-server-settings"></a>Comprobación de la configuración de los servidores físicos

1. Compruebe los [requisitos de los servidores físicos](migrate-support-matrix-physical.md#physical-server-requirements) para la valoración de los servidores.
2. Asegúrese de que los [puertos necesarios](migrate-support-matrix-physical.md#port-access) están abiertos en los servidores físicos.


### <a name="verify-appliance-settings"></a>Comprobación de la configuración del dispositivo

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-appliance.md#appliance---physical) los requisitos del dispositivo para los servidores físicos.
2. [Revise](migrate-appliance.md#url-access) las direcciones URL de Azure a las que tendrá acceso el dispositivo.
3. [Revise](migrate-appliance.md#collected-data---vmware) lo que el dispositivo va a recopilar durante la detección y la evaluación.
4. [Consulte](migrate-support-matrix-physical.md#port-access) los requisitos de acceso a los puertos para la evaluación del servidor físico.


### <a name="set-up-an-account-for-physical-server-discovery"></a>Configuración de una cuenta para la detección de servidores físicos

Azure Migrate necesita permisos para detectar los servidores en el entorno local.

- **Windows:** configure una cuenta de usuario local en todos los servidores de Windows que desee incluir en la detección. Es necesario agregar la cuenta de usuario a los grupos siguientes: - Usuarios de administración remota       - Usuarios de Monitor de rendimiento       - Usuarios de Registro de rendimiento
- **Linux:** Necesita una cuenta raíz en los servidores Linux que desee detectar.

## <a name="prepare-for-physical-server-migration"></a>Preparación para la migración de servidores físicos

Revise los requisitos para la migración de servidores físicos.

- [Revise](migrate-support-matrix-physical-migration.md#physical-server-requirements) requisitos del servidor físico para la migración.
- Azure Migrate: Server Migration utiliza un servidor de replicación para la migración del servidor físico:
    - [Revise](migrate-replication-appliance.md#appliance-requirements) los requisitos de implementación para el dispositivo de replicación y las [opciones](migrate-replication-appliance.md#mysql-installation) para instalar MySQL en el dispositivo.
    - Revise los requisitos de acceso a la [dirección URL](migrate-replication-appliance.md#url-access) y los [puertos] (migrate-replication-appliance.md#port-access) para el dispositivo de replicación.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Configurar los permisos de la cuenta de Azure.
> * Preparar los servidores físicos para la evaluación.

Continúe con el siguiente tutorial para crear un proyecto de Azure Migrate y evaluar los servidores físicos para la migración a Azure

> [!div class="nextstepaction"]
> [Evaluación de servidores físicos](./tutorial-assess-physical.md)
