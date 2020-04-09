---
title: Configuración del análisis de dependencias sin agente en Azure Migrate Server Assessment
description: Configure el análisis de dependencias sin agente en Azure Migrate Server Assessment.
ms.topic: how-to
ms.date: 2/24/2020
ms.openlocfilehash: af767bf73a3b9a6f2a91298987f11974499fd694
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455713"
---
# <a name="set-up-agentless-dependency-visualization"></a>Configuración de la visualización de dependencias sin agente 

En este artículo se describe cómo configurar el análisis de dependencias sin agente en Azure Migrate Server Assessment. El [análisis de dependencias](concepts-dependency-visualization.md) le ayuda a identificar y comprender las dependencias en las máquinas que quiere evaluar y migrar a Azure.


> [!IMPORTANT]
> La visualización de dependencias sin agente se encuentra actualmente en versión preliminar solo para máquinas virtuales de VMware detectadas con la herramienta Azure Migrate Server Assessment.
> Las características pueden ser limitadas o estar incompletas.
> Esta versión preliminar está incluida en el soporte al cliente y se puede usar para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



## <a name="before-you-start"></a>Antes de comenzar

- [Aprenda sobre](concepts-dependency-visualization.md#agentless-analysis) el análisis de dependencias sin agente.
- [Revise](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) los requisitos previos y los requisitos de compatibilidad para configurar la visualización de dependencias sin agente para máquinas virtuales de VMware.
- Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
- Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
- Asegúrese de que ha configurado un [dispositivo Azure Migrate](migrate-appliance.md) para detectar las máquinas locales. Obtenga información sobre cómo configurar un dispositivo para las VM de [VMware](how-to-set-up-appliance-vmware.md). El dispositivo detecta máquinas locales, y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment.


## <a name="current-limitations"></a>Limitaciones actuales

- En este momento, no se puede agregar un servidor a un grupo ni quitarlo de este en la vista de análisis de dependencias.
- Actualmente, no existe ningún mapa de dependencias disponible para un grupo de servidores.
- Actualmente, los datos de dependencias no se pueden descargar en formato tabular.

## <a name="create-a-user-account-for-discovery"></a>Creación de una cuenta de usuario para la detección

Configure una cuenta de usuario para que Server Assessment pueda acceder a la VM para la detección. [Aprenda](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) sobre los requisitos de la cuenta.


## <a name="add-the-user-account-to-the-appliance"></a>Adición de la cuenta de usuario al dispositivo

Agregue la cuenta de usuario al dispositivo.

1. Abra la aplicación de administración del dispositivo. 
2. Navegue hasta el panel **Provide vCenter details** (Proporcionar detalles de vCenter).
3. En **Detectar aplicaciones y dependencias en VM**, haga clic en **Agregar credenciales**.
3. Elija el **Sistema operativo**, proporcione un nombre descriptivo para la cuenta y especifique los valores de **Nombre de usuario**/**Contraseña**.
6. Haga clic en **Save**(Guardar).
7. Haga clic en **Guardar e iniciar la detección**.

    ![Adición de una cuenta de usuario de VM](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Inicio de la detección de dependencias

Elija las máquinas en las que quiere habilitar la detección de dependencias.

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Haga clic en el icono **Análisis de dependencias**.
3. Haga clic en **Agregar servidores**.
3. En la página **Agregar servidores**, elija el dispositivo que detecta las máquinas pertinentes.
4. En la lista de máquinas, seleccione las máquinas.
5. Haga clic en **Agregar servidores**.

    ![Inicio de la detección de dependencias](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Puede visualizar las dependencias aproximadamente 6 horas después de iniciar la detección de dependencias.

## <a name="visualize-dependencies"></a>Visualización de dependencias

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Busque la máquina que desea ver.
3. En la columna **Dependencias**, haga clic en **Ver dependencias**.
4. Cambie el período de tiempo del que quiere ver el mapa mediante el menú desplegable **Duración**.
5. Expanda el grupo **Cliente** para mostrar las máquinas que tienen una dependencia en la máquina seleccionada.
6. Expanda el grupo **Puerto** para mostrar las máquinas que tienen una dependencia de la máquina seleccionada.
7. Para navegar a la vista de mapa de cualquiera de las máquinas dependientes, haga clic en el nombre de la máquina > **Cargar mapa del servidor**.

    ![Expansión del grupo de puertos del servidor y carga del mapa del servidor](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Expansión del grupo de clientes ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Expanda la máquina seleccionada para ver los detalles de nivel de proceso de cada dependencia.

    ![Expansión del servidor para mostrar los procesos](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> La información del proceso para una dependencia no siempre está disponible. Si no está disponible, la dependencia se representa con el proceso marcado como "Proceso desconocido".

## <a name="stop-dependency-discovery"></a>Detención de la detección de dependencias

Elija las máquinas en las que quiere detener la detección de dependencias.

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Haga clic en el icono **Análisis de dependencias**.
3. Haga clic en **Quitar servidores**.
3. En la página **Quitar servidores**, elija el **dispositivo** que está detectando las VM en las que quiere detener la detección de dependencias.
4. En la lista de máquinas, seleccione las máquinas.
5. Haga clic en **Quitar servidores**.


## <a name="next-steps"></a>Pasos siguientes

[Agrupación de las máquinas](how-to-create-a-group.md) para su evaluación.
