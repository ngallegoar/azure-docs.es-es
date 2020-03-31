---
title: Administración de cargas de trabajo
description: Guía para la implementación de la administración de cargas de trabajo en Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 1809902fb153c5c9c83a9d00b6f817fde975d0fe
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349906"
---
# <a name="what-is-workload-management"></a>¿Qué es la administración de cargas de trabajo?

La ejecución de cargas de trabajo mixtas puede suponer desafíos de recursos en sistemas ocupados.  Los arquitectos de soluciones buscan formas de separar las actividades clásicas de almacenamiento de datos (como la carga, la transformación y la consulta de datos) para asegurarse de que existan recursos suficientes para cumplir con los Acuerdos de Nivel de Servicio.  

El aislamiento de los servidores físicos puede conducir a depósitos de infraestructura que están infrautilizados, sobrereservados o en un estado en el que las cachés se están preparan constantemente con hardware que se inicia y se detiene.  Un esquema de administración de cargas de trabajo correcto administra los recursos de manera eficaz, garantiza un uso de recursos muy eficaz y maximiza la rentabilidad de la inversión (ROI).

Una carga de trabajo de almacenamiento de datos hace referencia a todas las operaciones que tienen lugar en un almacenamiento de datos. La profundidad y amplitud de estos componentes dependen del nivel de madurez del almacenamiento de datos.  La carga de trabajo del almacenamiento de datos abarca: 
- Todo el proceso de carga de datos en el almacenamiento 
- La realización de informes y análisis de almacenamiento de datos
- La administración de datos en el almacenamiento de datos 
- La exportación de datos desde el almacenamiento de datos

La capacidad de rendimiento de un almacén de datos viene determinada por las [unidades de almacenamiento de datos](what-is-a-data-warehouse-unit-dwu-cdwu.md).
- Para ver los recursos asignados a todos los perfiles de rendimiento, consulte [Límites de memoria y concurrencia](memory-concurrency-limits.md).
- Para ajustar la capacidad, puede [escalarla o reducirla verticalmente](quickstart-scale-compute-portal.md).


## <a name="workload-management-concepts"></a>Conceptos sobre la administración de cargas de trabajo
En el pasado, con SQL Analytics en Azure Synapse, el rendimiento de las consultas se administrada mediante [clases de recursos](resource-classes-for-workload-management.md).  Clases de recursos permitidas para asignar memoria a una consulta basada en la pertenencia a roles.  El desafío principal con las clases de recursos es que, una vez que se configura, no hay ninguna gobernanza ni capacidad para controlar la carga de trabajo.  

Por ejemplo, conceder una pertenencia a roles de usuario ad hoc a smallrc permitía que el usuario consumiera el 100 % de la memoria del sistema.  Con las clases de recursos, no hay ninguna manera de reservar y garantizar que los recursos estén disponibles para las cargas de trabajo críticas.

La administración de cargas de trabajo de SQL Analytics en Azure Synapse consta de tres conceptos de alto nivel: [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md), [Importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md) y [Aislamiento de la carga de trabajo](sql-data-warehouse-workload-isolation.md).  Estas funcionalidades proporcionan más control sobre cómo la carga de trabajo usa los recursos del sistema.

La clasificación de la carga de trabajo es el concepto de asignar una solicitud a un grupo de cargas de trabajo y establecer los niveles de importancia.  Históricamente, esta asignación se realizaba a través de la pertenencia a roles mediante [sp_addrolemember](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#change-a-users-resource-class).  Esto se puede hacer ahora a través de [CREATE WORKLOAD CLASSIFER](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql).  La capacidad de clasificación proporciona un conjunto más completo de opciones, como la etiqueta, la sesión y el tiempo para clasificar las solicitudes.

La importancia de la carga de trabajo influye en el orden en el que una solicitud obtiene acceso a los recursos.  En un sistema ocupado, una solicitud con mayor importancia tiene el primer acceso a los recursos.  La importancia también puede garantizar el acceso ordenado a los bloqueos. 

El aislamiento de la carga de trabajo reserva recursos para un grupo de cargas de trabajo.  Los recursos reservados de un grupo de cargas de trabajo se conservan exclusivamente para ese grupo de cargas de trabajo a fin de garantizar la ejecución.  Los grupos de cargas de trabajo también permiten definir la cantidad de recursos que se asignan por solicitud, de manera muy similar a las clases de recursos.  Los grupos de cargas de trabajo ofrecen la posibilidad de reservar o limitar la cantidad de recursos que puede consumir un conjunto de solicitudes.  Por último, los grupos de cargas de trabajo son un mecanismo para aplicar reglas, como el tiempo de expiración de las consultas, a las solicitudes.  


## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la clasificación de la carga de trabajo, consulte [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).  
- Para más información sobre el aislamiento de la carga de trabajo, consulte [Aislamiento de la carga de trabajo](sql-data-warehouse-workload-isolation.md).  
- Para más información sobre la importancia de la carga de trabajo, consulte [Importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md).  
- Para más información sobre la supervisión de la administración de cargas de trabajo, vea [Supervisión del portal de administración de cargas de trabajo](sql-data-warehouse-workload-management-portal-monitor.md).  
