---
title: Detección de aplicaciones, roles y características en servidores locales con Azure Migrate
description: Obtenga información sobre cómo detectar aplicaciones, roles y características en servidores locales con Azure Migrate Server Assessment.
ms.topic: article
ms.date: 06/10/2020
ms.openlocfilehash: 535c8ae8c2d6e5d9d175e663a58d47dc76aa0529
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86118654"
---
# <a name="discover-machine-apps-roles-and-features"></a>Detección de aplicaciones, roles y características de la máquina

En este artículo se describe cómo detectar aplicaciones, roles y características en servidores locales mediante Azure Migrate: evaluación del servidor.

La detección del inventario de aplicaciones, roles y características que se ejecutan en las máquinas locales ayuda a identificar y adaptar una ruta de migración a Azure a sus cargas de trabajo. La detección de aplicaciones usa el dispositivo de Azure Migrate para realizar la acción junto con las credenciales de invitado de la máquina virtual. La detección de aplicaciones es sin agente. No se instala nada en las máquinas virtuales.

> [!NOTE]
> La detección de aplicaciones está actualmente en versión preliminar solo para máquinas virtuales de VMware y solo se limita a la detección. Todavía no se ofrece la evaluación basada en aplicaciones. 


## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha:
    - [Creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
    - [Agregado](how-to-assess.md) la herramienta de Azure Migrate: evaluación del servidor a un proyecto.
- Revise [la compatibilidad y los requisitos de la detección de aplicaciones](migrate-support-matrix-vmware.md#vmware-requirements).
- Asegúrese de que las máquinas virtuales en las que se ejecuta la detección de aplicaciones tengan instalada la versión de PowerShell 2.0 o posterior y de que estén instaladas las herramientas de VMware (posterior a 10.2.0).
- Compruebe los [requisitos ](migrate-appliance.md) para implementar el dispositivo de Azure Migrate.


## <a name="deploy-the-azure-migrate-appliance"></a>Implementación del dispositivo de Azure Migrate

1. [Revise](migrate-appliance.md#appliance---vmware) los requisitos para implementar el dispositivo de Azure Migrate.
2. Revise las direcciones URL de Azure a las que el dispositivo necesitará acceder en las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
3. [Revise los datos](migrate-appliance.md#collected-data---vmware) que la aplicación recopila durante la detección y valoración.
4. [Tenga en cuenta](migrate-support-matrix-vmware.md#port-access-requirements) los requisitos de acceso a puertos del dispositivo.
5. [Implemente el dispositivo de Azure Migrate](how-to-set-up-appliance-vmware.md) para iniciar la detección. Para implementar el dispositivo, descargue e importe una plantilla de OVA en VMware para crear el dispositivo como una VM de VMware. Configure el dispositivo y, a continuación, regístrelo con Azure Migrate.
6. Mientras implementa el dispositivo, especifique lo siguiente para iniciar la detección continua:
    - El nombre de la instancia de vCenter Server que quiere conectar.
    - Las credenciales que ha creado para que el dispositivo se conecte a vCenter Server.
    - Las credenciales de la cuenta que ha creado para que el dispositivo se conecte a las VM de Windows o Linux.

Después de implementar el dispositivo y proporcionar las credenciales, el dispositivo inicia la detección continua de los metadatos de la VM y los datos de rendimiento, junto con la detección de aplicaciones, características y roles.  La duración de la detección de aplicaciones depende del número de VM que tenga. Normalmente, la detección de aplicaciones tarda una hora para 500 VM.

## <a name="verify-permissions"></a>Comprobar los permisos

Has [creado una cuenta de vCenter Server de solo lectura](tutorial-prepare-vmware.md#set-up-permissions-for-assessment) para detección y valoración. La cuenta de solo lectura necesita privilegios habilitados para  > **las operaciones de invitado** de **Virtual Machines**, con el fin de interactuar con la máquina virtual para la detección de aplicaciones.

### <a name="add-the-user-account-to-the-appliance"></a>Adición de la cuenta de usuario al dispositivo

Agregue la cuenta de usuario como se indica a continuación:

1. Abra la aplicación de administración del dispositivo. 
2. Navegue hasta el panel **Provide vCenter details** (Proporcionar detalles de vCenter).
3. En **Detectar aplicaciones y dependencias en VM**, haga clic en **Agregar credenciales**.
3. Elija el **Sistema operativo**, proporcione un nombre descriptivo para la cuenta y especifique los valores de **Nombre de usuario**/**Contraseña**.
6. Haga clic en **Save**(Guardar).
7. Haga clic en **Guardar e iniciar la detección**.

    ![Adición de una cuenta de usuario de VM](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)


## <a name="review-and-export-the-inventory"></a>Revisión y exportación del inventario

Una vez finalizada la detección, si ha proporcionado las credenciales para la detección de aplicaciones, puede revisar y exportar el inventario de aplicaciones en Azure Portal.

1. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el recuento que se muestra para abrir la página **Servidores detectados**.

    > [!NOTE]
    > En esta fase también tiene la opción de configurar el análisis de dependencias para las máquinas detectadas, a fin de poder visualizar las dependencias entre las máquinas que quiere evaluar. [Obtener más información](concepts-dependency-visualization.md) sobre el análisis de dependencias.

2. En **Aplicaciones detectadas**, haga clic en el recuento mostrado.
3. En **Inventario de aplicaciones**, puede revisar las aplicaciones, los roles y las características que se han detectado.
4. Para exportar el inventario, en **Servidores detectados**, haga clic en **Exportar el inventario de las aplicaciones**.

El inventario de la aplicación se exporta y descarga en formato de Excel. En la hoja **Inventario de aplicaciones** se muestran todas las aplicaciones detectadas en todas las máquinas.

## <a name="next-steps"></a>Pasos siguientes

- [Cree una valoración](how-to-create-assessment.md) para los servidores detectados.
- Evalúe instancias de bases de datos de SQL Server con [Azure Migrate: valoración de la base de datos](/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017).
