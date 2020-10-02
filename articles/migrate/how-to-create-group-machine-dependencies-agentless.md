---
title: Configuración del análisis de dependencias sin agente en Azure Migrate Server Assessment
description: Configure el análisis de dependencias sin agente en Azure Migrate Server Assessment.
ms.topic: how-to
ms.date: 6/08/2020
ms.openlocfilehash: 2e6e562a18fa2ee0b89416ea67cc15394e760ada
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536445"
---
# <a name="analyze-machine-dependencies-agentless"></a>Análisis de las dependencias de la máquina (sin agente)

En este artículo se describe cómo configurar el análisis de dependencias sin agente en Azure Migrate Server Assessment. El [análisis de dependencias](concepts-dependency-visualization.md) le ayuda a identificar y comprender las dependencias entre máquinas para la evaluación y la migración a Azure.


> [!IMPORTANT]
> La visualización de dependencias sin agente se encuentra actualmente en versión preliminar para VM de VMware detectadas con la herramienta Azure Migrate:Server Assessment.
> Las características pueden ser limitadas o estar incompletas.
> Esta versión preliminar está incluida en el soporte al cliente y se puede usar para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="current-limitations"></a>Limitaciones actuales

- En la vista de análisis de dependencias, actualmente no se puede agregar o quitar un servidor de un grupo.
- Actualmente, no existe ningún mapa de dependencias disponible para un grupo de servidores.
- La recopilación de datos de dependencia se puede configurar simultáneamente para 400 servidores. Se puede analizar un número mayor de servidores mediante la secuenciación en lotes de 400.

## <a name="before-you-start"></a>Antes de comenzar

- [Revise](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) los sistemas los operativos compatibles y los permisos necesarios.
- Asegúrese de lo siguiente:
    - Tener un proyecto de Azure Migrate. Si no lo tiene, es el momento de [crearlo](how-to-add-tool-first-time.md).
    - Compruebe que ha [agregado ](how-to-assess.md) la herramienta Azure Migrate:Server Assessment al proyecto.
    - Configure un [dispositivo Azure Migrate](migrate-appliance.md) para detectar las máquinas locales. [Configuración de un dispositivo](how-to-set-up-appliance-vmware.md) para VM de VMware. El dispositivo detecta máquinas locales, y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment.
- Compruebe que una versión de las herramientas de VMware posterior a 10.2 está instalada en cada VM que se quiera analizar.


## <a name="create-a-user-account-for-discovery"></a>Creación de una cuenta de usuario para la detección

Configure una cuenta de usuario para que Server Assessment pueda acceder a la VM y detectar las dependencias. [Aprenda](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) sobre los requisitos de cuenta para VM Windows y Linux.


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
4. En la página **Agregar servidores**, elija el dispositivo que detecta las máquinas pertinentes.
5. En la lista de máquinas, seleccione las máquinas.
6. Haga clic en **Agregar servidores**.

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

## <a name="export-dependency-data"></a>Exportar datos de dependencia

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Haga clic en el icono **Análisis de dependencias**.
3. Haga clic en **Exportar las dependencias de la aplicación**.
4. En la página **Exportar las dependencias de la aplicación**, elija el dispositivo que detecta las máquinas correspondientes.
5. Seleccione la hora de inicio y la hora de finalización. Tenga en cuenta que solo puede descargar los datos de los últimos 30 días.
6. Haga clic en **Exportar dependencia**.

Los datos de dependencia se exportan y se descargan en formato CSV. El archivo descargado contiene los datos de dependencia en todas las máquinas habilitadas para el análisis de dependencias. 

![Exportar dependencias](./media/how-to-create-group-machine-dependencies-agentless/export.png)

### <a name="dependency-information"></a>Información de dependencia

Cada fila del CSV exportado corresponde a una dependencia observada en el intervalo de tiempo especificado. 

En la tabla siguiente se resumen los campos del CSV exportado. Tenga en cuenta que los campos del nombre del servidor, de la aplicación y del proceso solo se rellenan en los servidores que tienen habilitado el análisis de dependencia sin agente.

**Nombre del campo** | **Detalles**
--- | --- 
Intervalo de tiempo | El intervalo de tiempo durante el cual se observó la dependencia. <br/> Los datos de dependencia se capturan en intervalos de 6 horas actualmente.
Nombre de servidor de origen | Nombre de máquina de origen 
Aplicación de origen | Nombre de la aplicación en la máquina de origen 
Proceso de origen | Nombre del proceso en la máquina de origen 
Nombre del servidor de destino | Nombre del máquina de destino
IP de destino | Dirección IP de la máquina de destino
Aplicación de destino | Nombre de la aplicación en la máquina de destino
Proceso de destino | Nombre del proceso en la máquina de destino 
Puerto de destino | Número de puerto en la máquina de destino


## <a name="stop-dependency-discovery"></a>Detención de la detección de dependencias

Elija las máquinas en las que quiere detener la detección de dependencias.

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Haga clic en el icono **Análisis de dependencias**.
3. Haga clic en **Quitar servidores**.
3. En la página **Quitar servidores**, elija el **dispositivo** que está detectando las VM en las que quiere detener la detección de dependencias.
4. En la lista de máquinas, seleccione las máquinas.
5. Haga clic en **Quitar servidores**.


## <a name="next-steps"></a>Pasos siguientes

[Agrupación de máquinas](how-to-create-a-group.md) para su evaluación.
