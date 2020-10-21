---
title: Configuración del análisis de dependencias sin agente en Azure Migrate Server Assessment
description: Configure el análisis de dependencias sin agente en Azure Migrate Server Assessment.
ms.topic: how-to
ms.date: 6/08/2020
ms.openlocfilehash: 57e5add810cf4fac232bce08fc7ca96df0a7c3a0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91667476"
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
- La recopilación de datos de dependencia se puede configurar simultáneamente para 1000 servidores. Se puede analizar un número mayor de servidores mediante la secuenciación en lotes de 1000.

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

Puede visualizar las dependencias aproximadamente 6 horas después de iniciar la detección de dependencias. Si desea habilitar varias máquinas, puede usar [PowerShell](#start-or-stop-dependency-discovery-using-powershell) para hacerlo.

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

Si desea detener la dependencia en varias máquinas, puede usar [PowerShell](#start-or-stop-dependency-discovery-using-powershell) para hacerlo.


## <a name="start-or-stop-dependency-discovery-using-powershell"></a>Inicio o detención de la detección de dependencias con PowerShell

Descargue el módulo de PowerShell del repositorio de [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale) en GitHub.


### <a name="log-in-to-azure"></a>Inicio de sesión en Azure

1. Inicie sesión en su suscripción de Azure con el cmdlet Connect-AzAccount.

    ```PowerShell
    Connect-AzAccount
    ```
    Si usa Azure Government, utilice el siguiente comando.
    ```PowerShell
    Connect-AzAccount -EnvironmentName AzureUSGovernment
    ```

2. Seleccione la suscripción en la que creó el proyecto de Azure Migrate. 

    ```PowerShell
    select-azsubscription -subscription "Fabrikam Demo Subscription"
    ```

3. Importe el módulo de PowerShell AzMig_Dependencies descargado.

    ```PowerShell
    Import-Module .\AzMig_Dependencies.psm1
    ```

### <a name="enable-or-disable-dependency-data-collection"></a>Habilitación o deshabilitación de la recopilación de datos de dependencia

1. Obtenga la lista de máquinas virtuales de VMware detectadas en el proyecto de Azure Migrate con los siguientes comandos. En el ejemplo siguiente, el nombre del proyecto es FabrikamDemoProject y el grupo de recursos al que pertenece es FabrikamDemoRG. La lista de máquinas se guardará en FabrikamDemo_VMs.csv.

    ```PowerShell
    Get-AzMigDiscoveredVMwareVMs -ResourceGroupName "FabrikamDemoRG" -ProjectName "FabrikamDemoProject" -OutputCsvFile "FabrikamDemo_VMs.csv"
    ```

    En el archivo, puede ver el nombre para mostrar de la máquina virtual, el estado actual de la recopilación de dependencias y el identificador de ARM de todas las máquinas virtuales detectadas. 

2. Para habilitar o deshabilitar las dependencias, cree un archivo CSV de entrada. El archivo debe tener una columna con el encabezado "ARM ID". Se omitirán los demás encabezados que pueda haber en el archivo CSV. Puede crear el CSV con el archivo generado en el paso anterior. Cree una copia del archivo que conserve las máquinas virtuales en las que desea habilitar o deshabilitar las dependencias. 

    En el ejemplo siguiente, se habilita el análisis de dependencias en la lista de máquinas virtuales del archivo de entrada FabrikamDemo_VMs_Enable.csv.

    ```PowerShell
    Set-AzMigDependencyMappingAgentless -InputCsvFile .\FabrikamDemo_VMs_Enable.csv -Enable
    ```

    En el ejemplo siguiente, se deshabilita el análisis de dependencias en la lista de máquinas virtuales del archivo de entrada FabrikamDemo_VMs_Disable.csv.

    ```PowerShell
    Set-AzMigDependencyMappingAgentless -InputCsvFile .\FabrikamDemo_VMs_Disable.csv -Disable
    ```

## <a name="visualize-network-connections-in-power-bi"></a>Visualización de las conexiones de red en Power BI

Azure Migrate ofrece una plantilla de Power BI que puede usar para visualizar las conexiones de red de muchos servidores a la vez, así como para filtrar por proceso y servidor. Para visualizar las conexiones, cargue la plantilla de Power BI con los datos de dependencia según las instrucciones siguientes.

1. Descargue el módulo de PowerShell y la plantilla de Power BI del repositorio de [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale) en GitHub.

2. Inicie sesión en Azure con las siguientes instrucciones: 
- Inicie sesión en su suscripción de Azure con el cmdlet Connect-AzAccount.

    ```PowerShell
    Connect-AzAccount
    ```

- Si usa Azure Government, utilice el siguiente comando.

    ```PowerShell
    Connect-AzAccount -EnvironmentName AzureUSGovernment
    ```

- Seleccione la suscripción en la que creó el proyecto de Azure Migrate. 

    ```PowerShell
    select-azsubscription -subscription "Fabrikam Demo Subscription"
    ```

3. Importe el módulo de PowerShell AzMig_Dependencies descargado.

    ```PowerShell
    Import-Module .\AzMig_Dependencies.psm1
    ```

4. Ejecute el siguiente comando: Este comando descarga los datos de dependencias en un CSV y los procesa para generar una lista de dependencias únicas que se puede usar para la visualización en Power BI. En el ejemplo siguiente, el nombre del proyecto es FabrikamDemoProject y el grupo de recursos al que pertenece es FabrikamDemoRG. Se descargarán las dependencias de las máquinas detectadas por FabrikamAppliance. Las dependencias únicas se guardarán en FabrikamDemo_Dependencies.csv.

    ```PowerShell
    Get-AzMigDependenciesAgentless -ResourceGroup FabrikamDemoRG -Appliance FabrikamAppliance -ProjectName FabrikamDemoProject -OutputCsvFile "FabrikamDemo_Dependencies.csv"
    ```

5. Abra la plantilla de Power BI descargada.

6. Cargue los datos de dependencia descargados en Power BI.
    - Abra la plantilla en Power BI.
    - Haga clic en **Obtener datos** en la barra de herramientas. 
    - Elija **Texto/CSV** en Orígenes de datos comunes.
    - Elija el archivo de dependencias descargado.
    - Haga clic en **Cargar**.
    - Verá que se importa una tabla con el nombre del archivo CSV. Puede ver la tabla en la barra de campos de la derecha. Cambie su nombre a AzMig_Dependencies.
    - Haga clic en Actualizar en la barra de herramientas.

    Se destacarán el gráfico de conexiones de red y las segmentaciones de nombre del servidor de origen, nombre del servidor de destino, nombre del proceso de origen y nombre del proceso de destino con los datos importados.

7. Visualice el mapa de conexiones de red filtrando por servidores y procesos. Guarde el archivo.


## <a name="next-steps"></a>Pasos siguientes

[Agrupación de máquinas](how-to-create-a-group.md) para su evaluación.
