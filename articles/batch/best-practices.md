---
title: Procedimientos recomendados
description: Obtenga información sobre los procedimientos recomendados y sugerencias útiles para desarrollar su solución de Azure Batch.
ms.date: 04/03/2020
ms.topic: article
ms.openlocfilehash: 43a0020953ea44593cf38298a78547194751fc72
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82117512"
---
# <a name="azure-batch-best-practices"></a>Procedimientos recomendados de Azure Batch

En este artículo se describe una colección de procedimientos recomendados para usar el servicio de Azure Batch de forma eficaz y eficiente. Estos procedimientos recomendados proceden de nuestra experiencia con Batch y las experiencias de clientes de Batch. Es importante comprender este artículo para evitar los problemas de diseño, posibles problemas de rendimiento y los antipatrones durante el desarrollo para Batch y su uso.

En este artículo, aprenderá lo siguiente:

> [!div class="checklist"]
> - Cuáles son los procedimientos recomendados.
> - Por qué debería usar los procedimientos recomendados
> - Qué puede ocurrir si no consigue seguir el procedimiento recomendado.
> - Cómo seguir los procedimientos recomendados.

## <a name="pools"></a>Grupos

Los grupos de Batch son los recursos de proceso para ejecutar trabajos en el servicio de Batch. En las secciones siguientes se proporcionan instrucciones sobre los procedimientos recomendados que se deben seguir al trabajar con grupos de Batch.

### <a name="pool-configuration-and-naming"></a>Configuración y nomenclatura de grupos

- **Modo de asignación de grupo** Al crear una cuenta de Batch, puede elegir entre dos modos de asignación de grupo: **Servicio de Batch** o **Suscripción de usuario**. En la mayoría de los casos, debe usar el modo predeterminado servicio Batch, en el que los grupos se asignan en segundo plano en las suscripciones administradas por Batch. En el modo de suscripción de usuario alternativo, tanto las máquinas virtuales de Batch como otros recursos se crean directamente en su suscripción cuando se crea un grupo. Las cuentas de suscripción de usuario se utilizan principalmente para habilitar un subconjunto importante, pero pequeño, de escenarios. Puede obtener más información sobre el modo de suscripción de usuario en [Configuración adicional para el modo de suscripción de usuario](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode).

- **Tenga en cuenta el tiempo de ejecución del trabajo y de la tarea al determinar la asignación del trabajo al grupo.**
    Si tiene trabajos compuestos principalmente de tareas de ejecución breve y los recuentos de tareas totales esperados son pequeños (por lo que el tiempo de ejecución global previsto del trabajo no es largo), no asigne un nuevo grupo a cada trabajo. El tiempo de asignación de los nodos acortará el tiempo de ejecución del trabajo.

- **Los grupos deben tener más de un nodo de proceso.**
    No se garantiza que los nodos individuales estén siempre disponibles. Aunque no es habitual, los errores de hardware, las actualizaciones del sistema operativo y un sinfín de otros problemas pueden hacer que los nodos individuales estén sin conexión. Si la carga de trabajo de Batch requiere un progreso determinista y garantizado, debe asignar grupos con varios nodos.

- **No reutilice los nombres de recursos.**
    Los recursos de Batch (trabajos, grupos, etc.) suelen ser inestables a lo largo del tiempo. Por ejemplo, puede crear un grupo el lunes, eliminarlo el martes y, después, crear otro grupo el jueves. Cada recurso nuevo que cree debe recibir un nombre único que no haya usado antes. Esto puede hacerse mediante el uso de un GUID (como el nombre de recurso completo o como parte del mismo) o insertando la hora en que se creó el recurso en el nombre del mismo. Batch admite la propiedad [DisplayName](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.jobspecification.displayname?view=azure-dotnet), que se puede usar para dar a un recurso un nombre legible, incluso si el identificador de recurso real es algo que no es descriptivo. El uso de nombres únicos facilita la diferenciación de un recurso determinado en los registros y las métricas. También elimina la ambigüedad si alguna vez tiene que archivar un caso de soporte técnico para un recurso.

- **Continuidad durante el mantenimiento y errores del grupo.**
    Es mejor que los trabajos usen los grupos dinámicamente. Si los trabajos usan el mismo grupo para todo, existe la posibilidad de que los trabajos no se ejecuten si algo sale mal con el grupo. Esto es especialmente importante para las cargas de trabajo que dependen del tiempo. Para solucionar este error, seleccione o cree un grupo de forma dinámica cuando programe cada trabajo, o tenga una manera de invalidar el nombre del grupo para que pueda omitir un grupo incorrecto.

- **Continuidad empresarial durante el mantenimiento y errores del grupo** Hay muchas causas posibles por las que puede que un grupo no alcance el tamaño necesario que quiere, como errores internos, restricciones de capacidad, etc. Por esta razón, debe estar preparado para redestinar los trabajos a un grupo diferente (posiblemente con un tamaño de máquina virtual diferente: Batch admite esto a través de [UpdateJob](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update?view=azure-dotnet)) si es necesario. Evite el uso de un identificador de grupo estático con la esperanza de que nunca se eliminará ni cambiará.

### <a name="pool-lifetime-and-billing"></a>Vigencia del grupo y facturación

La vigencia del grupo puede variar en función del método de asignación y de las opciones que se apliquen a la configuración del grupo. Los grupos pueden tener una vigencia arbitraria y un número de nodos de proceso variable en el grupo en cualquier momento. Es su responsabilidad administrar los nodos de proceso del grupo, ya sea explícitamente o a través de las características proporcionadas por el servicio (escalado automático o autogrupo).

- **Mantenga los grupos actualizados.**
    Debe cambiar el tamaño de los grupos a cero cada pocos meses para asegurarse de que obtiene las actualizaciones y las correcciones de errores del agente de nodo más recientes. El grupo no recibirá actualizaciones de agente de nodo a menos que se vuelva a crear o se cambie el tamaño a 0 nodos de proceso. Antes de volver a crear o cambiar el tamaño del grupo, se recomienda descargar los registros del agente de nodo con fines de depuración, como se describe en la sección [Nodos](#nodes).

- **Volver a crear el grupo** De forma similar, no se recomienda eliminar y volver a crear los grupos diariamente. En su lugar, cree un nuevo grupo y actualice los trabajos existentes para que apunten al nuevo grupo. Cuando se hayan pasado todas las tareas al nuevo grupo, elimine el grupo anterior.

- **Eficacia del grupo y facturación** Batch en sí no incurre en cargos adicionales, pero se incurre en cargos por los recursos de proceso utilizados. Se le facturará por cada nodo de proceso del grupo, independientemente del estado en el que se encuentre. Esto incluye los cargos necesarios para que se ejecute el nodo, como los costos de almacenamiento y de red. Para obtener más información sobre los procedimientos recomendados, consulte [Análisis de costos y presupuestos para Azure Batch](budget.md).

### <a name="pool-allocation-failures"></a>Errores de asignación de grupos

Los errores de asignación de grupos pueden producirse en cualquier momento durante la primera asignación o en los siguientes cambios de tamaño. Esto puede deberse a un agotamiento temporal de la capacidad en una región o a errores en otros servicios de Azure en los que se basa Batch. La cuota principal no es una garantía, sino un límite.

### <a name="unplanned-downtime"></a>Tiempo de inactividad no planeado

Los grupos de Batch pueden experimentar eventos de tiempo de inactividad en Azure. Es importante tenerlo en cuenta al planear y desarrollar el escenario o el flujo de trabajo para Batch.

En caso de que se produzca un error en un nodo, Batch intentará recuperar automáticamente estos nodos de proceso en su nombre. De esta forma se puede desencadenar la reprogramación de cualquier tarea en ejecución en el nodo que se recupera. Consulte [Diseño de reintentos](#designing-for-retries-and-re-execution) para obtener más información sobre las tareas interrumpidas.

- **Dependencia de la región de Azure** Es aconsejable no depender de una sola región de Azure si tiene una carga de trabajo que depende del tiempo o de la producción. Aunque es poco frecuente, hay problemas que pueden afectar a toda una región. Por ejemplo, si su procesamiento tiene que iniciarse en un momento determinado, considere la posibilidad de escalar verticalmente el grupo de la región principal *justo antes de la hora de inicio*. Si se produce un error en la escala del grupo, puede revertir la escala de un grupo verticalmente en una región (o regiones) de copia de seguridad. Los grupos en varias cuentas de distintas regiones proporcionan una copia de seguridad preparada y de fácil acceso si se produce algún problema con otro grupo. Para más información, consulte [Diseño de la aplicación para una alta disponibilidad](high-availability-disaster-recovery.md).

## <a name="jobs"></a>Trabajos

Un trabajo es un contenedor diseñado para contener cientos, miles o incluso millones de tareas.

- **Colocación de muchas tareas en un trabajo** El uso de un trabajo para ejecutar una única tarea es ineficaz. Por ejemplo, es más eficaz usar un único trabajo que contenga 1000 tareas en lugar de crear 100 trabajos que contengan 10 tareas cada uno. La ejecución de 1000 trabajos, cada uno con una sola tarea, sería el enfoque menos eficaz, más lento y más costoso.

    No diseñe una solución de Batch que requiera miles de trabajos activos simultáneamente. No hay ninguna cuota de tareas, por lo que la ejecución de tantas tareas en el menor número de trabajos como sea posible utiliza de forma eficaz el [trabajo y las cuotas de programación de trabajos](batch-quota-limit.md#resource-quotas).

- **Vigencia del trabajo** Un trabajo de Batch tiene una vigencia indefinida hasta que se elimine del sistema. El estado de un trabajo designa si este puede aceptar más tareas para la programación. Un trabajo no se mueve automáticamente al estado completado a menos que se termine explícitamente. Esta acción se puede desencadenar automáticamente mediante la propiedad [onAllTasksComplete](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete?view=azure-dotnet) o [maxWallClockTime](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints).

Hay un [trabajo activo y cuota de programación de trabajo](batch-quota-limit.md#resource-quotas) predeterminados. Los trabajos y las programaciones de trabajos en estado completado no cuentan para esta cuota.

## <a name="tasks"></a>Tareas

Las tareas son unidades de trabajo individuales que componen un trabajo. El usuario envía las tareas y Batch las programa en nodos de proceso. Hay varias consideraciones de diseño que se deben tener en cuenta al crear y ejecutar tareas. En las siguientes secciones se explican los escenarios comunes y cómo diseñar las tareas para solventar los problemas y ejecutarlas de forma eficaz.

- **Guardado de los datos de la tarea como parte de la misma**
    Los nodos de proceso son efímeros por naturaleza. Hay muchas características en Batch, como el autogrupo y el escalado automático, que facilitan la desaparición de los nodos. Cuando los nodos dejan el grupo (debido a un cambio de tamaño o una eliminación del grupo), también se eliminan todos los archivos de dichos nodos. Por este motivo, se recomienda que antes de que se complete una tarea, mueva el resultado fuera del nodo en el que se ejecuta y a un almacén duradero, al igual que, si se produce un error en una tarea, debe mover los registros necesarios para diagnosticar el error a un almacén duradero. Batch tiene compatibilidad integrada con Azure Storage para cargar datos a través de [OutputFiles](batch-task-output-files.md), así como una variedad de sistemas de archivos compartidos, o bien puede realizar la carga usted mismo en sus tareas.

### <a name="task-lifetime"></a>Duración de la tarea

- **Eliminación de las tareas cuando se hayan completado**
    Elimine las tareas cuando ya no se necesiten o establezca una restricción de tarea [retentionTime](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime?view=azure-dotnet). Si se establece un `retentionTime`, Batch limpia automáticamente el espacio en disco que usa la tarea cuando `retentionTime` expire.

    La eliminación de tareas consigue dos cosas. Garantiza que no tiene una acumulación de tareas en el trabajo, lo que dificultaría la consulta y búsqueda de la tarea que le interesa (porque tendría que filtrar las tareas completadas). También limpia los datos de la tarea correspondiente en el nodo (siempre que todavía no se haya llegado al `retentionTime`). Esto garantiza que los nodos no se llenen con los datos de la tarea y se quede sin espacio en disco.

### <a name="task-submission"></a>Envío de tareas

- **Envío de un gran número de tareas en una colección**
    Las tareas se pueden enviar de forma individual o en colecciones. Envíe tareas en [colecciones](https://docs.microsoft.com/rest/api/batchservice/task/addcollection) hasta un máximo de 100 a la vez al realizar el envío masivo de tareas para reducir la sobrecarga y el tiempo de envío.

### <a name="task-execution"></a>Ejecución de tareas

- **Elección de las tareas máximas por nodo** Batch admite tareas de sobresuscripción en nodos (que ejecutan más tareas que núcleos tiene un nodo). Depende de usted asegurarse de que las tareas se ajustan a los nodos del grupo. Por ejemplo, su experiencia podría verse degradada si intenta programar ocho tareas, cada una de las cuales consume un 25 % de uso de CPU en un nodo (en un grupo con `maxTasksPerNode = 8`).

### <a name="designing-for-retries-and-re-execution"></a>Diseño de reintentos y reejecución

Batch puede reintentar automáticamente las tareas. Hay dos tipos de reintentos: controlados por el usuario e internos. Los reintentos controlados por el usuario los especifica el elemento [maxTaskRetryCount](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount?view=azure-dotnet) de la tarea. Cuando un programa especificado en la tarea sale con un código de salida distinto de cero, la tarea se reintenta hasta el valor del `maxTaskRetryCount`.

Aunque es poco frecuente, se puede reintentar una tarea internamente debido a errores en el nodo de proceso, como no poder actualizar el estado interno o un error en el nodo mientras la tarea se está ejecutando. La tarea se reintentará en el mismo nodo de proceso, si es posible, hasta un límite interno antes de que se desista y se aplace la tarea que Batch va a reprogramar, potencialmente en un nodo de proceso diferente.

- **Creación de tareas durables** Las tareas deben diseñarse para resistir errores y dar cabida al reintento. Esto es especialmente importante para las tareas de larga duración. Para ello, asegúrese de que las tareas generan el mismo resultado único, incluso si se ejecutan más de una vez. Una manera de lograrlo es hacer que las tareas sean de búsqueda de objetivos. Otra manera es asegurarse de que las tareas sean idempotentes (es decir, tendrán el mismo resultado independientemente del número de veces que se ejecuten).

    Un ejemplo común es una tarea de copia archivos en un nodo de proceso. Un enfoque sencillo es una tarea que copia todos los archivos especificados cada vez que se ejecuta, lo que resulta ineficaz y no se ha creado para resistir errores. En su lugar, cree una tarea para asegurarse de que los archivos se encuentran en el nodo de proceso; una tarea que no vuelva a copiar los archivos que ya están presentes. De esta manera, la tarea se retomará en el punto en que se quedó si se interrumpió.

- **Nodos de prioridad baja** No hay ninguna diferencia de diseño al ejecutar las tareas en nodos dedicados o en nodos de prioridad baja. Tanto si una tarea se reemplaza mientras se ejecuta en un nodo de prioridad baja como si se interrumpe debido a un error en un nodo dedicado, ambas situaciones se mitigarían diseñando la tarea para resistir el error.

- **Tiempo de ejecución de tareas** Evite las tareas con un tiempo de ejecución corto. Las tareas que solo se ejecutan durante uno o dos segundos no son ideales. Debe intentar realizar una cantidad significativa de trabajo en una tarea individual (un mínimo de 10 segundos y hasta horas o días). Si cada tarea se está ejecutando durante un minuto (o más), la sobrecarga de programación como una fracción del tiempo de proceso general es pequeña.

## <a name="nodes"></a>Nodos

- **Las tareas de inicio deben ser idempotentes** De forma similar a otras tareas, la tarea de inicio del nodo debe ser idempotente, ya que se volverá a ejecutar cada vez que se inicie el nodo. Una tarea idempotente es simplemente una que genera un resultado coherente cuando se ejecuta varias veces.

- **Administración de servicios de larga duración a través de la interfaz de servicios del sistema operativo**
    A veces, es necesario ejecutar otro agente junto con el agente de Batch en el nodo, por ejemplo, para recopilar datos del nodo y notificarlos. Se recomienda que estos agentes se implementen como servicios del sistema operativo, como un servicio de Windows o un servicio `systemd` de Linux.

    Al ejecutar estos servicios, no deben realizar bloqueos de archivos en los directorios administrados por Batch del nodo, ya que, de lo contrario, Batch no podrá eliminar dichos directorios debido a los bloqueos de archivos. Por ejemplo, si instala un servicio de Windows en una tarea de inicio, en lugar de iniciar el servicio directamente desde el directorio de trabajo de la tarea de inicio, copie los archivos en otro lugar (si los archivos existen simplemente omita la copia). Instalación del servicio desde dicha ubicación Cuando Batch vuelva a ejecutar la tarea de inicio, eliminará el directorio de trabajo de la tarea de inicio y la volverá a crear. Esto funciona porque el servicio tiene bloqueos de archivos en el otro directorio, no en el directorio de trabajo de la tarea de inicio.

- **Evitar la creación de uniones de directorio en Windows** Las uniones de directorios, que a veces se denominan vínculos físicos, son difíciles de tratar durante la limpieza de tareas y trabajos. Use vínculos simbólicos (soft-links) en lugar de vínculos físicos.

- **Recopilación de los registros del agente de Batch si hay algún problema** Si observa un problema relacionado con el comportamiento de un nodo o tareas en ejecución en un nodo, se recomienda recopilar los registros del agente de Batch antes de desasignar los nodos en cuestión. Puede recopilar los registros del agente de Batch mediante la API de registros del servicio de Batch de carga. Estos registros se pueden proporcionar como parte de una incidencia de soporte técnico a Microsoft y ayudarán a solucionar los problemas.

## <a name="security"></a>Seguridad

### <a name="security-isolation"></a>Aislamiento de seguridad

Con respecto al aislamiento, si su escenario requiere aislar los trabajos entre sí, debe aislarlos colocándolos en grupos independientes. Un grupo es el límite de aislamiento de seguridad en Batch y, de forma predeterminada, dos grupos no son visibles ni pueden comunicarse entre sí. Evite el uso de cuentas de Batch independientes como medio de aislamiento.

## <a name="moving"></a>Traslado

### <a name="move-batch-account-across-regions"></a>Traslado de la cuenta de Batch entre regiones

Existen varios escenarios en los que puede que deba mover su cuenta de Batch existente existentes de una región a otra. También puede realizar el traslado a otra región como parte del planeamiento de la recuperación ante desastres.

Las cuentas de Azure Batch no se pueden trasladar de una región a otra. Sin embargo, puede usar una plantilla de Azure Resource Manager para exportar la configuración actual de la cuenta de Batch.  Después, para preparar el recurso en otra región, puede exportar la cuenta de Batch a una plantilla, modificar los parámetros para que coincidan con la región de destino y, a continuación, implementar la plantilla en la nueva región. Después de cargar la plantilla en la nueva región, tendrá que volver a crear los certificados, las programaciones de trabajos y los paquetes de aplicación. Para confirmar los cambios y completar el traslado de la cuenta de Batch, recuerde eliminar la cuenta de Batch original o el grupo de recursos.

Para más información sobre Resource Manager y las plantillas, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

## <a name="connectivity-to-the-batch-service"></a>Conectividad con el servicio Batch

### <a name="network-security-groups-nsgs-and-user-defined-routes-udrs"></a>Grupos de seguridad de red (NSG) y Rutas definidas por el usuario (UDR)

Al aprovisionar [grupos de Batch en una red virtual](batch-virtual-network.md), asegúrese de seguir las instrucciones sobre el uso de la etiqueta de servicio `BatchNodeManagement`, los puertos, los protocolos y la dirección de la regla.
Se recomienda encarecidamente el uso de la etiqueta de servicio y no de las direcciones IP del servicio Batch subyacentes, ya que pueden cambiar con el tiempo. El uso de las direcciones IP del servicio Batch puede manifestarse directamente como inestabilidad o interrupciones para los grupos de Batch, ya que el servicio Batch actualiza las direcciones IP usadas a lo largo del tiempo. Si actualmente usa direcciones IP del servicio Batch en las reglas de NSG, se recomienda cambiar al uso de la etiqueta de servicio.

En el caso de las rutas definidas por el usuario, asegúrese de que tiene un proceso implementado para actualizar las direcciones IP del servicio Batch periódicamente en la tabla de rutas, ya que cambian con el tiempo. Para más información sobre cómo obtener la lista de direcciones IP del servicio Batch, vea [Etiquetas de servicio en un entorno local](../virtual-network/service-tags-overview.md). Las direcciones IP del servicio Batch se asociarán a la etiqueta de servicio `BatchNodeManagement`, o la variante regional que coincida con la región de la cuenta de Batch.

### <a name="honoring-dns"></a>Respetar DNS

Asegúrese de que los sistemas respetan el período de vida (TTL) de DNS para la dirección URL del servicio de la cuenta de Batch. Además, asegúrese de que los clientes del servicio Batch y otros mecanismos de conectividad con el servicio Batch no se basan en direcciones IP.

Si las solicitudes reciben respuestas HTTP de nivel 5xx y hay un encabezado "Conexión: cerrar" en la respuesta, el cliente del servicio Batch debe observar la recomendación cerrando la conexión existente y volviendo a resolver DNS para la dirección URL del servicio de la cuenta de Batch, e intentar realizar las siguientes solicitudes en una nueva conexión.

### <a name="retrying-requests-automatically"></a>Reintentar solicitudes automáticamente

Asegúrese de que los clientes del servicio Batch tienen las directivas de reintento adecuadas para volver a intentar automáticamente las solicitudes, incluso durante el funcionamiento normal y no exclusivamente durante períodos de tiempo de mantenimiento del servicio. Estas directivas de reintento deben abarcar un intervalo de al menos 5 minutos. Se proporcionan capacidades de reintento automático con varios SDK de Batch, como la [clase RetryPolicyProvider de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet).

