---
title: 'Procedimientos recomendados de supervisión: Azure Database for MySQL'
description: En este artículo se describen los procedimientos recomendados para supervisar Azure Database for MySQL.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 11/23/2020
ms.openlocfilehash: 59301e26f4d42056322ba5a7cdaff1555c531096
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96354888"
---
# <a name="best-practices-for-monitoring-azure-database-for-mysql--single-server"></a>Procedimientos recomendados para supervisar Azure Database for MySQL: servidor único

Obtenga información sobre los procedimientos recomendados que se pueden usar para supervisar las operaciones de base de datos y asegurarse de que el rendimiento no se ve comprometido a medida que crece el tamaño de los datos. A medida que agregamos nuevas funcionalidades a la plataforma, seguiremos perfeccionando los procedimientos recomendados que se detallan en esta sección.

## <a name="layout-of-the-current-monitoring-toolkit"></a>Diseño del kit de herramientas de supervisión actual

Azure Database for MySQL proporciona herramientas y métodos que puede usar para supervisar el uso fácilmente, agregar o quitar recursos (como CPU, memoria o E/S), solucionar posibles problemas y ayudar a mejorar el rendimiento de una base de datos. Puede [supervisar las métricas de rendimiento](concepts-monitoring.md#metrics) de forma periódica para ver los valores promedio, máximo y mínimo de una serie de intervalos de tiempo.

Puede [configurar alertas](howto-alert-on-metric.md#create-an-alert-rule-on-a-metric-from-the-azure-portal) para un umbral de métricas, por lo que se le informará si el servidor ha alcanzado esos límites y toma las medidas adecuadas.  

Supervise el servidor de bases de datos para asegurarse de que los recursos asignados a la base de datos pueden controlar la carga de trabajo de la aplicación. Si la base de datos está alcanzando los límites de recursos, puede hacer lo siguiente:
    * Identificar y optimizar las consultas que consumen más recursos 
    * Agregar más recursos mediante la actualización del nivel de servicio.

### <a name="cpu-utilization"></a>Uso de CPU
Supervise el uso de CPU y si la base de datos está agotando los recursos de la CPU. Si el uso de la CPU es del 90% o más, debe escalar verticalmente el proceso aumentando la cantidad de núcleos virtuales o escalar al siguiente nivel de precios.  Asegúrese de que el rendimiento o la simultaneidad sean los esperados a medida que escale o reduzca verticalmente la CPU. 

### <a name="memory"></a>Memoria 
La cantidad de memoria disponible para el servidor de bases de datos es proporcional al [número de núcleos virtuales](concepts-pricing-tiers.md). Asegúrese de que la memoria es suficiente para la carga de trabajo. Realice una prueba de carga de la aplicación para comprobar que la memoria es suficiente para las operaciones de lectura y escritura. Si el consumo de memoria de la base de datos supera con frecuencia un umbral definido, significa que debe actualizar la instancia aumentando los núcleos virtuales o el nivel de rendimiento más alto. Use el [Almacén de consultas](concepts-query-store.md) y las [recomendaciones de rendimiento de consulta](concepts-performance-recommendations.md) para identificar consultas con la duración más larga y más ejecutada. Explore las oportunidades de optimización. 

### <a name="storage"></a>Storage 
La [cantidad de almacenamiento](howto-create-manage-server-portal.md#scale-compute-and-storage) aprovisionada para el servidor MySQL determina las E/S por segundo del servidor. El almacenamiento que usa el servicio incluye los archivos de base de datos, los registros de transacciones y los registros de servidor y las instantáneas de copia de seguridad. Asegúrese de que el espacio en disco consumido no supere constantemente el 85 por ciento del total de espacio en disco aprovisionado. En ese caso, debe eliminar o archivar los datos del servidor de base de datos para liberar algo de espacio. 

### <a name="network-traffic"></a>Tráfico de red 

**Rendimiento de la recepción de red, rendimiento de transmisión de red**: la velocidad del tráfico de red hacia y desde la instancia de MySQL en megabytes por segundo. Debe evaluar el requisito de rendimiento para el servidor y supervisar constantemente el tráfico si el rendimiento es menor de lo esperado. 

### <a name="database-connections"></a>Conexiones de base de datos 
**Conexiones de bases de datos**: el número de sesiones de cliente que están conectadas a Azure Database for MySQL debe estar alineado con los [límites de conexión para el tamaño de SKU seleccionado](concepts-server-parameters.md#max_connections). 


## <a name="next-steps"></a>Pasos siguientes

- [Procedimiento recomendado para el rendimiento de Azure Database for MySQL](concept-performance-best-practices.md)
- [Procedimiento recomendado para las operaciones de servidor con Azure Database for MySQL](concept-operation-excellence-best-practices.md)
