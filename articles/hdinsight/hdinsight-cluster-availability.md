---
title: 'Supervisión: registros de Apache Ambari y Azure Monitor: Azure HDInsight'
description: Aprenda a usar los registros de Ambari y Azure Monitor para supervisar la disponibilidad y estado del clúster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/06/2020
ms.openlocfilehash: 383366fa3e436c79bed28a7c47f1e9daa5f0d9de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060188"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-and-azure-monitor-logs"></a>Supervisión de la disponibilidad del clúster con los registros de Apache Ambari y Azure Monitor

Los clústeres de HDInsight incluyen Apache Ambari, que proporciona información de estado en un vistazo y alertas predefinidas, así como integración de registros de Azure Monitor, que proporciona métricas y registros que se pueden consultar, así como alertas configurables.

En este documento se muestra cómo usar estas herramientas para supervisar el clúster y le guía por algunos ejemplos para configurar una alerta de Ambari, supervisar un índice de disponibilidad de nodo y crear una alerta de Azure Monitor que se desencadena cuando no se haya recibido ningún latido desde uno o varios nodos en cinco horas.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>Panel

Para acceder al panel de Ambari, seleccione el vínculo **Inicio de Ambari** de la sección **Paneles de clúster** de la hoja de información general de HDInsight en Azure Portal, tal como se muestra a continuación. También puede acceder a él yendo a `https://CLUSTERNAME.azurehdinsight.net` en un explorador en el que CLUSTERNAME es el nombre del clúster.

![Vista de portal de recursos de HDInsight](media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png)

Luego, se le pedirá un nombre de usuario y contraseña de inicio de sesión del clúster. Escriba las credenciales que eligió al crear el clúster.

Se le redirigirá al panel de Ambari, que contiene widgets que muestran una serie de métricas para ofrecerle una vista rápida del estado del clúster de HDInsight. Estos widgets muestran métricas como, por ejemplo, el número de elementos DataNodes (nodos de trabajo) en directo y elementos JournalNodes (nodo de zookeeper), el tiempo de actividad del elemento NameNodes (nodos principales), así como también métricas específicas para determinados tipos de clúster, como el tiempo de actividad de YARN ResourceManager para los clústeres de Spark y Hadoop.

![Pantalla del panel de uso de Apache Ambari](media/hdinsight-cluster-availability/apache-ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Hosts: ver el estado de nodo individual.

También puede ver información de estado de los nodos individuales. Seleccione la pestaña **Hosts** para ver una lista de todos los nodos del clúster y ver información básica sobre cada nodo. La marca de verificación verde a la izquierda de cada nombre de nodo indica que todos los componentes están activos en el nodo. Si un componente está inactivo en un nodo, verá un triángulo de alerta rojo en lugar de la marca de verificación verde.

![Vista de hosts de Apache Ambari para HDInsight](media/hdinsight-cluster-availability/apache-ambari-hosts1.png)

Luego, puede seleccionar el **nombre** de un nodo para ver métricas más detalladas del host para ese nodo concreto. En esta vista se muestra la disponibilidad y el estado de cada componente individual.

![Vista de nodo único de hosts de Apache Ambari](media/hdinsight-cluster-availability/apache-ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Alertas de Ambari

Ambari también ofrece varias alertas configurables que pueden proporcionar una notificación de determinados eventos. Cuando se desencadenan alertas, se muestran en la esquina superior izquierda de Ambari en una notificación de color rojo que contiene el número de alertas. Al seleccionar esta notificación aparece una lista de las alertas actuales.

![Recuento de alertas actuales de Apache Ambari](media/hdinsight-cluster-availability/apache-ambari-alerts.png)

Para ver una lista de definiciones de alertas y sus estados, seleccione la pestaña **Alertas**, tal como se muestra a continuación.

![Ver las definiciones de alertas de Ambari](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari ofrece muchas alertas predefinidas relacionadas con la disponibilidad, entre las que se incluyen las siguientes:

| Nombre de la alerta                        | Descripción   |
|---|---|
| Resumen de estado de DataNode           | Esta alerta de nivel de servicio se desencadena si hay elementos DataNodes en mal estado.|
| Estado de disponibilidad alta de NameNode | Esta alerta de nivel de servicio se desencadena si el elemento NameNode activo o NameNode en espera no está en ejecución.|
| Porcentaje de JournalNodes disponible    | Esta alerta se desencadena si el número de elementos JournalNodes inactivos en el clúster es mayor que el umbral crítico configurado. Agrega los resultados de las comprobaciones del proceso JournalNode. |
| Porcentaje de DataNodes disponible       | Esta alerta se desencadena si el número de elementos DataNodes inactivos en el clúster es mayor que el umbral crítico configurado. Agrega los resultados de las comprobaciones del proceso DataNode.|

Encontrará [aquí](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui) una lista completa de alertas de Ambari que ayudan a supervisa la disponibilidad de un clúster.

Para ver los detalles de una alerta o modificar los criterios, seleccione el **nombre** de la alerta. Tome **Resumen de estado de DataNode** como ejemplo. Puede ver una descripción de la alerta, así como los criterios específicos que desencadenarán una alerta de "advertencia" o "crítica", así como el intervalo de comprobación de los criterios. Para editar la configuración, seleccione el botón **Editar** de la esquina superior derecha del cuadro de configuración.

![Configuración de alertas de Apache Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Aquí, puede editar la descripción y, lo que es más importante, el intervalo de comprobación y los umbrales para las alertas de advertencia o críticas.

![Vista de edición de las configuraciones de alertas de Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

En este ejemplo, puede hacer que 2 elementos DataNodes en mal estado desencadenen una alerta crítica y 1 elemento DataNode en mal estado desencadenen solo una advertencia. Cuando termine la edición, seleccione **Guardar**.

### <a name="email-notifications"></a>Notificaciones por correo electrónico

Opcionalmente, también puede configurar notificaciones por correo electrónico para las alertas de Ambari. Para ello, en la pestaña **Alertas**, haga clic en el botón **Acciones** de la esquina superior izquierda y seleccione **Administrar notificaciones**.

![Acción de administración de las notificaciones de Ambari](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Se abrirá un cuadro de diálogo para administrar las notificaciones de alerta. Seleccione **+** en la parte inferior del cuadro de diálogo y rellene los campos obligatorios para proporcionar a Ambari los detalles del servidor de correo electrónico desde el que se van a enviar correos electrónicos.

> [!TIP]
> La configuración de notificaciones por correo electrónico de Ambari puede ser una buena manera de recibir alertas en un solo lugar a la hora de administrar muchos clústeres de HDInsight.

## <a name="azure-monitor-logs-integration"></a>Integración de los registros de Azure Monitor

Los registros de Azure Monitor permiten la recolección de los datos generados por varios recursos, como clústeres de HDInsight, y su agregación en un solo lugar para lograr una experiencia de supervisión unificada.

Como requisito previo, necesitará un área de trabajo de Log Analytics para almacenar los datos recopilados. Si aún no ha creado una, puede seguir estas instrucciones: [Crear un área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>Habilitar la integración de registros de Azure Monitor de HDInsight

En la página de recursos del clúster de HDInsight, en el portal, seleccione **Azure Monitor**. Luego, seleccione **Habilitar** y seleccione el área de trabajo de Log Analytics de la lista desplegable.

![HDInsight Operations Management Suite](media/hdinsight-cluster-availability/azure-portal-monitoring.png)

### <a name="query-metrics-and-logs-tables"></a>Consultar tablas de métricas y registros

Cuando se haya habilitado la integración de registros de Azure Monitor (esto puede tardar unos minutos), vaya al recurso **Área de trabajo de Log Analytics** y seleccione **Registros**.

![Registros del área de trabajo de Log Analytics](media/hdinsight-cluster-availability/hdinsight-portal-logs.png)

La hoja Registros muestra un número de consultas de ejemplo, como:

| Nombre de la consulta                      | Descripción                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Disponibilidad de los equipos hoy en día    | Crear un gráfico del número de equipos que envían registros, cada hora                     |
| Enumerar latidos                 | Lista de todos los latidos del equipo de la última hora                           |
| Último latido de cada equipo | Mostrar el último latido enviado por cada equipo                             |
| Equipos desconectados           | Lista de todos los equipos conocidos que no enviaron un latido en las últimas 5 horas |
| Índice de disponibilidad               | Calcular el índice de disponibilidad de cada equipo conectado                |

Por ejemplo, ejecute la consulta de ejemplo **Índice de disponibilidad**. Para ello, seleccione **Ejecutar** en esa consulta, tal como se muestra en la captura de pantalla anterior. Esto mostrará el índice de disponibilidad de cada nodo del clúster como un porcentaje. Si habilitó varios clústeres de HDInsight para enviar métricas a la misma área de trabajo de Log Analytics, verá el índice de disponibilidad para todos los nodos en los clústeres que se muestran.

![Consulta de ejemplo de "índice de disponibilidad" de los registros del área de trabajo de Log Analytics](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE]  
> El índice de disponibilidad se mide en un período de 24 horas, por lo que el clúster tendrá que ejecutarse durante al menos 24 horas antes de poder ver índice de disponibilidad precisos.

Puede anclar esta tabla a un panel compartido al hacer clic en **Anclar** de la esquina superior derecha. Si no tiene ningún panel compartido en el que se puede escribir, puede ver cómo crear uno aquí: [Creación y uso compartido de paneles en Azure Portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-and-share-a-dashboard).

### <a name="azure-monitor-alerts"></a>Alertas de Azure Monitor

También puede configurar alertas de Azure Monitor que se desencadenarán cuando el valor de una métrica o los resultados de una consulta cumplan ciertas condiciones. Por ejemplo, vamos a crear una alerta para enviar un correo electrónico cuando uno o varios nodos no hayan enviado un latido en 5 horas (es decir, se supone que no está disponible).

En la hoja **Registros**, ejecute la consulta de ejemplo **Equipos no disponibles**. Para ello, seleccione **Ejecutar** en esa consulta, tal como se muestra a continuación.

![Ejemplo de registros de "equipos no disponibles" del área de trabajo de Log Analytics](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Si todos los nodos están disponibles, esta consulta debería devolver 0 resultados por ahora. Haga clic en **Nueva regla de alertas** para comenzar a configurar la alerta para esta consulta.

![Nueva regla de alertas del área de trabajo de Log Analytics](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Una alerta tiene tres componentes: el *recursos* para el que crear la regla (en este caso, el área de Log Analytics), la *condición* para desencadenar la alerta y los *grupos de acciones*  que determinan lo que sucederá cuando se desencadene la alerta.
Haga clic en el **título de la condición**, tal como se muestra abajo, para finalizar la configuración de la lógica de señal.

![Condición de regla de creación de alertas del portal](media/hdinsight-cluster-availability/portal-condition-title.png)

Se abrirá **Configurar lógica de señal**.

Establezca la sección **Lógica de alerta** tal como se indica abajo:

*Basado en: número de resultados, Condición: Mayor que, Umbral: 0.*

Dado que esta consulta devuelve solo nodos no disponibles como resultado, si el número de resultados es mayor que 0, se debería desencadenar la alerta.

En la sección **Se evaluó basándose en**, establezca el **período** y la **frecuencia** según la frecuencia con la que desea comprobar si hay nodos no disponibles.

Para esta alerta, debería asegurarse de que **Período=Frecuencia**. Puede encontrar más información sobre el período, la frecuencia y otros parámetros de alerta [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Seleccione **Listo** cuando haya terminado de configurar la lógica de señal.

![Configuración de la lógica de señal en la regla de alertas](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Si no dispone de un grupo de acciones existente, haga clic en **Crear nuevo** en la sección **Grupos de acciones**.

![Creación de un nuevo grupo de acciones en la regla de alertas](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Se abrirá **Agregar grupo de acciones**. Elija un valor de **Nombre del grupo de acciones**, **Nombre corto**, **Suscripción** y **Grupo de recursos**. En la sección **Acciones**, elija un valor de **Nombre de acción** y seleccione **Correo electrónico/SMS/Inserción/Voz** como el valor de **Tipo de acción.**

> [!NOTE]
> Hay muchas otras acciones que una alerta puede desencadenar además de correo electrónico, SMS, inserción o voz, como una función de Azure, LogicApp, Webhook, ITSM y Automation Runbook. [Más información](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information).

Se abrirá **Correo electrónico/SMS/Inserción/Voz**. Elija un valor de **Nombre** para el destinatario, **active** la casilla **Correo electrónico** y escriba una dirección de correo electrónico a la que enviar la alerta. Seleccione **Aceptar** en **Correo electrónico/SMS/Inserción/Voz** y luego en **Agregar grupo de acciones** para terminar de configurar el grupo de acciones.

![Incorporación de un grupo de acciones en la regla de alertas](media/hdinsight-cluster-availability/portal-add-action-group.png)

Después de que se cierren estas hojas, debería ver el grupo de acciones en la sección **Grupos de acciones**. Por último, complete la sección **Detalles de alerta**. Para ello, escriba un valor en **Nombre de la regla de alertas** y en **Descripción**, y luego elija **Gravedad**. Haga clic en **Crear regla de alerta** para finalizar.

![Finalización después de la creación de una regla de alertas del portal](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> La capacidad de especificar el valor de **Gravedad** es una herramienta eficaz que puede usarse al crear varias alertas. Por ejemplo, podría crear una alerta para generar una advertencia (gravedad 1) si un único nodo principal se queda inactivo y otra alerta crítica (gravedad 0) en el improbable caso de que ambos nodos principales dejen de funcionar.

Cuando se cumple la condición de la alerta, esta última se desencadena y recibirá un correo electrónico con los detalles de la misma, que tendrá el aspecto siguiente:

![Ejemplo de correo electrónico de alerta de Azure Monitor](media/hdinsight-cluster-availability/portal-oms-alert-email.png)

También puede ver todas las alertas desencadenadas, agrupadas por gravedad. Para ello, vaya a **Alertas** en su **Área de trabajo de Log Analytics**.

![Alertas del área de trabajo de Log Analytics](media/hdinsight-cluster-availability/hdi-portal-oms-alerts.png)

Al seleccionar una agrupación de gravedad (por ejemplo, las de **Gravedad 1** tal como se ha mencionado anteriormente), se mostrarán los registros de todas las alertas de esta gravedad que se hayan desencadenado, como se muestra a continuación:

![Alertas de gravedad 1 del área de trabajo de Log Analytics](media/hdinsight-cluster-availability/portal-oms-alerts-sev1.png)

## <a name="next-steps"></a>Pasos siguientes

- [Disponibilidad y confiabilidad de clústeres de Apache Hadoop en HDInsight](hdinsight-high-availability-linux.md)
