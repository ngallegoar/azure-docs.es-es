---
title: Variables de entorno de tiempo de ejecución de tareas
description: Instrucciones y referencia de las variable de entorno de tiempo de ejecución de tareas para Azure Batch Analytics.
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: 6b8ade312146802ede6e12181a082a8fcd3842fe
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85960918"
---
# <a name="azure-batch-runtime-environment-variables"></a>Variables de entorno de tiempo de ejecución de Azure Batch

El [servicio Azure Batch](https://azure.microsoft.com/services/batch/) establece las siguientes variables del entorno en nodos de proceso. Puede hacer referencia a estas variables del entorno en líneas de comandos de la tarea y en los programas y secuencias de comandos que ejecutan las líneas de comandos.

Para obtener más información sobre el uso de variables de entorno con Batch, consulte [Configuración del entorno para las tareas](./jobs-and-tasks.md#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Visibilidad de la variable del entorno

Estas variables del entorno solo son visibles en el contexto de la **tarea del usuario**, es decir, la cuenta de usuario en el nodo donde se ejecuta una tarea. *No* las verá si [se conecta de forma remota](./error-handling.md#connect-to-compute-nodes) a un nodo de proceso a través del protocolo de escritorio remoto (RDP) o de Secure Shell (SSH) y muestra la lista de variables de entorno. Esto se debe a que la cuenta de usuario que se usa para la conexión remota no es la misma que la cuenta que utiliza la tarea.

Para obtener el valor actual de una variable de entorno, inicie `cmd.exe` en un nodo de proceso de Windows o `/bin/sh` en un nodo de Linux:

`cmd /c set <ENV_VARIABLE_NAME>`

`/bin/sh printenv <ENV_VARIABLE_NAME>`

## <a name="command-line-expansion-of-environment-variables"></a>Expansión de línea de comandos de las variables del entorno

Las líneas de comandos que ejecutan las tareas en nodos de proceso no se ejecutan en un shell. Por tanto, estas líneas de comandos no se pueden beneficiar de forma nativa de las características del shell, como la expansión de variables del entorno (esto incluye `PATH`). Para beneficiarse de estas características, debe **invocar el shell** en la línea de comandos. Por ejemplo, ejecute `cmd.exe` en nodos de proceso en Windows o `/bin/sh` en nodos de Linux:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Variables de entorno

| Nombre de la variable                     | Descripción                                                              | Disponibilidad | Ejemplo |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | El nombre de la cuenta de Batch a la que pertenece la tarea.                  | Todas las tareas.   | mybatchaccount |
| AZ_BATCH_ACCOUNT_URL            | La dirección URL de la cuenta de Batch. | Todas las tareas. | `https://myaccount.westus.batch.azure.com` |
| AZ_BATCH_APP_PACKAGE            | Un prefijo de todas las variables de entorno de paquete de aplicación. Por ejemplo, si la aplicación "FOO" versión "1" se instala en un grupo, la variable de entorno es AZ_BATCH_APP_PACKAGE_FOO_1 (en Linux) o AZ_BATCH_APP_PACKAGE_FOO#1 (en Windows). AZ_BATCH_APP_PACKAGE_FOO_1 apunta a la ubicación en la que se descargó el paquete (una carpeta). Al usar la versión predeterminada del paquete de la aplicación, use la variable de entorno AZ_BATCH_APP_PACKAGE sin los números de versión. Si está en Linux y el nombre del paquete de aplicación es "Agent-linux-x64" y la versión es "1.1.46.0", el nombre del entorno es: AZ_BATCH_APP_PACKAGE_agent_linux_x64_1_1_46_0 (con guiones bajos y en minúsculas). Obtenga más información [aquí](./batch-application-packages.md#execute-the-installed-applications). | Cualquier tarea con un paquete de aplicación asociado. También está disponible para todas las tareas si el nodo mismo tiene paquetes de aplicación. | AZ_BATCH_APP_PACKAGE_FOO_1 (Linux) o AZ_BATCH_APP_PACKAGE_FOO#1 (Windows) |
| AZ_BATCH_AUTHENTICATION_TOKEN   | Token de autenticación que concede acceso a un conjunto limitado de operaciones de servicio de Batch. Esta variable de entorno solo está presente si [authenticationTokenSettings](/rest/api/batchservice/task/add#authenticationtokensettings) está establecido al [agregar la tarea](/rest/api/batchservice/task/add#request-body). El valor del token se usa en las API de Batch como credenciales para crear un cliente de Batch, como en [BatchClient.Open() .NET API](/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_). | Todas las tareas. | Token de acceso de OAuth2 |
| AZ_BATCH_CERTIFICATES_DIR       | Un directorio en el [directorio de trabajo de la tarea][files_dirs] en el que se almacenan los certificados para nodos de proceso de Linux. Esta variable de entorno no se aplica a los nodos de ejecución de Windows.                                                  | Todas las tareas.   |  /mnt/batch/tasks/workitems/batchjob001/job-1/task001/certs |
| AZ_BATCH_HOST_LIST              | La lista de nodos que se asignan a una [tarea de varias instancias][multi_instance] en el formato `nodeIP,nodeIP`. | Tareas principales y secundarias de varias instancias. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Especifica si el nodo actual es el nodo maestro de una [tarea de varias instancias][multi_instance]. Los valores posibles son `true` y `false`.| Tareas principales y secundarias de varias instancias. | `true` |
| AZ_BATCH_JOB_ID                 | El identificador de la cuenta a la que pertenece la tarea. | Todas las tareas excepto la tarea de inicio. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | La ruta de acceso completa del [directorio de tareas][files_dirs] de preparación del trabajo en el nodo. | Todas las tareas excepto la tarea de inicio y la tarea de preparación del trabajo. Solo está disponible si el trabajo está configurado con una tarea de preparación del trabajo. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | La ruta de acceso completa del [directorio de trabajo de tareas][files_dirs] de preparación del trabajo en el nodo. | Todas las tareas excepto la tarea de inicio y la tarea de preparación del trabajo. Solo está disponible si el trabajo está configurado con una tarea de preparación del trabajo. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_MASTER_NODE            | La dirección IP y puerto del nodo de ejecución en el que se ejecuta la tarea principal de una [tarea de varias instancias][multi_instance]. No use el puerto especificado aquí para la comunicación de MPI o NCCL: está reservado para el servicio Azure Batch. En su lugar, use la variable MASTER_PORT, ya sea estableciéndola con un valor pasado a través del argumento de la línea de comandos (el puerto 6105 es una buena opción predeterminada) o, usando el valor, AML establece si lo hace. | Tareas principales y secundarias de varias instancias. | `10.0.0.4:6000` |
| AZ_BATCH_NODE_ID                | El identificador del nodo al que se asignó la tarea. | Todas las tareas. | tvm-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_IS_DEDICATED      | Si `true`, el nodo actual es un nodo dedicado. Si `false`, es un [nodo de prioridad baja](batch-low-pri-vms.md). | Todas las tareas. | `true` |
| AZ_BATCH_NODE_LIST              | La lista de nodos que se asignan a una [tarea de varias instancias][multi_instance] en el formato `nodeIP;nodeIP`. | Tareas principales y secundarias de varias instancias. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_NODE_MOUNTS_DIR        | Ruta de acceso completa de la ubicación de [montaje del sistema de archivos](virtual-file-mount.md) de nivel de nodo donde residen todos los directorios de montaje. Los recursos compartidos de archivos de Windows usan una letra de unidad, por lo que, para Windows, la unidad de montaje forma parte de dispositivos y unidades.  |  Todas las tareas, incluida la tarea de inicio, tienen acceso al usuario, dado que el usuario es consciente de los permisos de montaje para el directorio montado. | En Ubuntu, por ejemplo, la ubicación es `/mnt/batch/tasks/fsmounts`. |
| AZ_BATCH_NODE_ROOT_DIR          | La ruta de acceso completa de la raíz de todos los [directorios de Batch][files_dirs] en el nodo. | Todas las tareas. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | La ruta de acceso completa del [directorio compartido][files_dirs] en el nodo. Todas las tareas que se ejecutan en un nodo tienen acceso de lectura/escritura a este directorio. Las tareas que se ejecutan en otros nodos no tienen acceso remoto a este directorio (no es un directorio de red "compartido"). | Todas las tareas. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | La ruta de acceso completa del [directorio de la tarea de inicio][files_dirs] en el nodo. | Todas las tareas. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | El identificador del grupo en el que se ejecuta la tarea. | Todas las tareas. | batchpool001 |
| AZ_BATCH_TASK_DIR               | La ruta de acceso completa del [directorio de tareas][files_dirs] en el nodo. Este directorio contiene `stdout.txt` y `stderr.txt` para la tarea, y AZ_BATCH_TASK_WORKING_DIR. | Todas las tareas. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | El identificador de la tarea actual. | Todas las tareas excepto la tarea de inicio. | task001 |
| AZ_BATCH_TASK_SHARED_DIR | Una ruta de acceso de directorio que es idéntica para la tarea principal y cada tarea secundaria de una [tarea de varias instancias][multi_instance]. La ruta de acceso existe en cada nodo en el que se ejecuta la tarea de varias instancias y es accesible en modo lectura/escritura para los comandos de tarea que se ejecutan en ese nodo (tanto el [comando de coordinación][coord_cmd] y el [comando de la aplicación][app_cmd]). Las tareas secundarias o la tarea principal que se ejecutan en otros nodos no tienen acceso remoto a este directorio (no es un directorio de red "compartido"). | Tareas principales y secundarias de varias instancias. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_TASK_WORKING_DIR       | La ruta de acceso completa del [directorio de trabajo en tareas][files_dirs] en el nodo. La tarea que se está ejecutando tiene acceso de lectura/escritura a este directorio. | Todas las tareas. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | La lista de nodos y el número de núcleos por nodo que se asignan a una [tarea de varias instancias][multi_instance]. Los nodos y núcleos se muestran en el formato `numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, donde el número de nodos es seguido de una o varias direcciones IP de nodo y el número de núcleos para cada una. |  Tareas principales y secundarias de varias instancias. |`2 10.0.0.4 1 10.0.0.5 1` |

[files_dirs]: ./files-and-directories.md
[multi_instance]: ./batch-mpi.md
[coord_cmd]: ./batch-mpi.md#coordination-command
[app_cmd]: ./batch-mpi.md#application-command
