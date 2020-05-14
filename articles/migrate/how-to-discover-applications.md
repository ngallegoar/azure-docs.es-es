---
title: Detección de aplicaciones, roles y características en servidores locales con Azure Migrate
description: Obtenga información sobre cómo detectar aplicaciones, roles y características en servidores locales con Azure Migrate Server Assessment.
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: ff9f5489b513cd1405e6b093d7537e4cbcead041
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744624"
---
# <a name="discover-machine-apps-roles-and-features"></a>Detección de aplicaciones, roles y características de la máquina

En este artículo se describe cómo detectar aplicaciones, roles y características en servidores locales mediante Azure Migrate: Server Assessment.

La detección del inventario de aplicaciones y roles o características que se ejecutan en las máquinas locales le ayuda a identificar y planear una ruta de migración a Azure que está adaptada a sus cargas de trabajo.

> [!NOTE]
> La detección de aplicaciones está actualmente en versión preliminar solo para máquinas virtuales de VMware y solo se limita a la detección. Todavía no se ofrece la evaluación basada en aplicaciones. Valoración basada en máquina de máquinas virtuales de Hyper-V locales, máquinas virtuales de VMware y servidores físicos.

Detección de aplicaciones mediante Azure Migrate: Server Assessment es una herramienta sin agente. No se instala nada en máquinas locales y máquinas virtuales. Server Assessment usa el dispositivo de Azure Migrate para realizar la detección junto con las credenciales de invitado de la máquina. El dispositivo accede de forma remota a las máquinas de VMware mediante las API de VMware.


## <a name="before-you-start"></a>Antes de comenzar

1. Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
2. Asegúrese de haber [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment a un proyecto.
4. Compruebe los [requisitos de VMware](migrate-support-matrix-vmware.md#vmware-requirements) para detectar y evaluar las VM de VMware con el dispositivo de Azure Migrate.
5. Compruebe los [requisitos ](migrate-appliance.md) para implementar el dispositivo de Azure Migrate.
6. [Compruebe la compatibilidad y los requisitos](migrate-support-matrix-vmware.md#application-discovery) para la detección de aplicaciones.



## <a name="deploy-the-azure-migrate-appliance"></a>Implementación del dispositivo de Azure Migrate

1. [Revise](migrate-appliance.md#appliance---vmware) los requisitos para implementar el dispositivo de Azure Migrate.
2. Revise las direcciones URL de Azure a las que el dispositivo necesitará acceder en las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
3. [Revise los datos](migrate-appliance.md#collected-data---vmware) que la aplicación recopila durante la detección y valoración.
4. [Tenga en cuenta](migrate-support-matrix-vmware.md#port-access) los requisitos de acceso a puertos del dispositivo.
5. [Implemente el dispositivo de Azure Migrate](how-to-set-up-appliance-vmware.md) para iniciar la detección. Para implementar el dispositivo, descargue e importe una plantilla de OVA en VMware para crear el dispositivo como una VM de VMware. Configure el dispositivo y, a continuación, regístrelo con Azure Migrate.
6. Mientras implementa el dispositivo, especifique lo siguiente para iniciar la detección continua:
    - El nombre de la instancia de vCenter Server que quiere conectar.
    - Las credenciales que ha creado para que el dispositivo se conecte a vCenter Server.
    - Las credenciales de la cuenta que ha creado para que el dispositivo se conecte a las VM de Windows o Linux.

Después de implementar el dispositivo y proporcionar las credenciales, el dispositivo inicia la detección continua de los metadatos de la VM y los datos de rendimiento, junto con la detección de aplicaciones, características y roles.  La duración de la detección de aplicaciones depende del número de VM que tenga. Normalmente, la detección de aplicaciones tarda una hora para 500 VM.

## <a name="prepare-a-user-account"></a>Preparación de una cuenta de usuario

Cree una cuenta para usarla en la detección y agréguela al dispositivo.

### <a name="create-a-user-account-for-discovery"></a>Creación de una cuenta de usuario para la detección

Configure una cuenta de usuario para que Server Assessment pueda acceder a la VM para la detección. [Aprenda](migrate-support-matrix-vmware.md#application-discovery) sobre los requisitos de la cuenta.


### <a name="add-the-user-account-to-the-appliance"></a>Adición de la cuenta de usuario al dispositivo

Agregue la cuenta de usuario al dispositivo.

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
    > En esta fase también tiene la opción de configurar la asignación de dependencias para las máquinas detectadas, a fin de poder visualizar las dependencias entre las máquinas que quiere evaluar. [Más información](how-to-create-group-machine-dependencies.md).

2. En **Aplicaciones detectadas**, haga clic en el recuento mostrado.
3. En **Inventario de aplicaciones**, puede revisar las aplicaciones, los roles y las características que se han detectado.
4. Para exportar el inventario, en **Servidores detectados**, haga clic en **Exportar el inventario de las aplicaciones**.

El inventario de la aplicación se exporta y descarga en formato de Excel. En la hoja **Inventario de aplicaciones** se muestran todas las aplicaciones detectadas en todas las máquinas.

## <a name="next-steps"></a>Pasos siguientes

- [Cree una evaluación](how-to-create-assessment.md) para la migración mediante lift-and-shift de los servidores detectados.
- Evalúe instancias de SQL Server Database con [Azure Migrate: Evaluación de la base de datos](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017).
