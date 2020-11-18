---
title: Creación de plan de migración con Azure Migrate | Microsoft Docs
description: Proporciona instrucciones sobre cómo crear un plan de migración con Azure Migrate.
author: rashi-ms
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: rajosh
ms.openlocfilehash: 8f37814e29ce0089c26e235123768296efc2c0b0
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94504931"
---
# <a name="build-migration-plan-with-azure-migrate"></a>Creación de plan de migración con Azure Migrate

Siga este artículo para crear el plan de migración en Azure con [Azure Migrate](migrate-services-overview.md). 

## <a name="define-cloud-migration-goals"></a>Definición de los objetivos de la migración a la nube

Antes de empezar, comprender y evaluar la [motivación](/azure/cloud-adoption-framework/strategy/motivations) para pasar contenido a la nube puede ayudar a conseguir el resultado empresarial esperado. Tal y como se explica en [Cloud Adoption Framework](/azure/cloud-adoption-framework), existen varios desencadenadores y resultados.   

**Evento empresarial** | **Resultado de migración**
--- | ---
Salida del centro de datos | Coste 
Fusión, adquisición o desinversión | Reducción de la complejidad técnica o de los proveedores
Reducción de gastos de capital | Optimización de las operaciones internas
Finalización del soporte técnico de tecnologías críticas | Aumento de agilidad empresarial
Respuesta a los cambios de cumplimiento normativo | Preparación para nuevas funcionalidades técnicas
Nuevos requisitos de soberanía de datos | Escalado para satisfacer demandas del mercado
Reducción de las interrupciones y mejora de la estabilidad de TI | Escalado para satisfacer demandas geográficas

Identificar la motivación ayuda a reforzar los objetivos de migración estratégicos. El siguiente paso es identificar y planear una ruta de migración adaptada a las cargas de trabajo. La herramienta [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) evalúa las cargas de trabajo locales, y proporciona orientación y herramientas que ayudan con la migración.

## <a name="understand-your-digital-estate"></a>Reconocimiento de su patrimonio digital

Comience por identificar la infraestructura local, las aplicaciones y las dependencias. Así podrá identificar las cargas de trabajo para la migración a Azure y recopilar proyecciones de costos optimizados. La herramienta Server Assessment ayuda a identificar las cargas de trabajo que se usan, las dependencias entre cargas de trabajo y la optimización de la carga de trabajo.

### <a name="workloads-in-use"></a>Cargas de trabajo que se usan

Azure Migrate usa un dispositivo de Azure Migrate ligero para realizar una detección sin agente de las máquinas virtuales de VMware locales, las máquinas virtuales de Hyper-V, otras máquinas virtuales y los servidores físicos. Con la detección continua se recopilan información de configuración de la máquina y metadatos de rendimiento, así como datos de las aplicaciones. A continuación se muestra lo que el dispositivo recopila de las máquinas locales: 

- Metadatos de las máquinas, los discos y las NIC.

- Aplicaciones instaladas, roles y características.

- Datos de rendimiento, incluido el uso de CPU y de la memoria, el número de operaciones de entrada y de salida del disco, y el rendimiento.

Tras la recopilación de datos, puede exportar la lista de inventario de aplicaciones para buscar aplicaciones y las instancias de SQL Server que se ejecutan en las máquinas. Puede usar la herramienta Azure Migrate: Database Assessment para conocer el estado de preparación de SQL Server.

 ![Inventario de aplicaciones en el portal](./media/concepts-migration-planning/application-inventory-portal.png)

 ![Exportación del inventario de aplicaciones](./media/concepts-migration-planning/application-inventory-export.png)

Junto con los datos detectados con la herramienta Server Assessment, puede usar los datos de la base de datos de administración de configuración (CMDB) para crear una vista del estado del servidor y de la base de datos, y para entender cómo se distribuyen los servidores entre unidades de negocio, propietarios de aplicaciones, zonas geográficas, etc. Esto ayuda a decidir las cargas de trabajo que se priorizarán para la migración. 

### <a name="dependencies-between-workloads"></a>Dependencias entre cargas de trabajo

Una vez detectados los servidores, puede [analizar las dependencias](concepts-dependency-visualization.md) para visualizar e identificar las dependencias entre servidores y las estrategias de optimización para mover servidores interdependientes a Azure. La visualización ayuda a reconocer si se usan determinadas máquinas o si se pueden retirar en lugar de migrarse.  El análisis de dependencias ayuda a garantizar que se incluye todo y a evitar interrupciones inesperadas durante la migración. Una vez realizados el inventario de aplicaciones y el análisis de las dependencias, podrá crear grupos de servidores de confianza alta e iniciar la evaluación de estos.

 ![Mapa de dependencia](./media/concepts-migration-planning/expand-client-group.png)

### <a name="optimization-and-sizing"></a>Optimización y dimensionamiento

Azure proporciona flexibilidad para cambiar el tamaño de la capacidad de la nube con el tiempo y la migración ofrece una oportunidad de optimizar los recursos de memoria y CPU asignados a los servidores. La evaluación de los servidores identificados ayuda a comprender el historial de rendimiento de la carga de trabajo. Esto es fundamental para el ajuste correcto del tamaño de las SKU de las máquinas virtuales de Azure y para cumplir las recomendaciones de disco en Azure.

## <a name="assess-migration-readiness"></a>Evaluación del estado de preparación de la migración


### <a name="readinesssuitability-analysis"></a>Análisis de la idoneidad y del estado de preparación

Puede exportar el informe de evaluación y filtrar estas categorías para comprender el estado de preparación de Azure:

- **Preparada para Azure**: las máquinas se pueden migrar como están a Azure, sin realizar ningún cambio. 
- **Condicionalmente preparada para Azure**: las máquinas se pueden migrar a Azure, pero necesitan cambios menores según la guía de corrección que proporciona la evaluación.
- **No está preparado para Azure**: las máquinas no se pueden migrar a Azure tal cual. Los problemas se deben corregir de acuerdo con la guía de corrección para la migración. 
- **Preparación desconocida**: Azure Migrate no puede determinar la el estado de preparación de las máquinas, no hay metadatos suficientes.

Con las evaluaciones de base de datos puede evaluar el estado de preparación para migrar los datos de SQL Server a Azure SQL Database o a instancias de Azure SQL Managed Instance. La evaluación muestra el estado de preparación para la migración (en porcentaje) de todas las instancias de SQL Server. Además, para cada instancia puede ver el destino recomendado en Azure, los posibles bloqueos de migración, el recuento de cambios importantes, el estado de preparación para Azure SQL DB o la máquina virtual de Azure SQL y el nivel de compatibilidad. Puede profundizar más para entender el impacto de los bloqueos de migración y las recomendaciones para corregirlos.

 ![Valoraciones de la base de datos:](./media/concepts-migration-planning/database-assessment-portal.png)

### <a name="sizing-recommendations"></a>Recomendaciones de dimensionamiento

Una vez que las máquinas se marcan como preparadas para Azure, Server Assessment realiza recomendaciones de dimensionamiento para identificar la SKU de las máquinas virtuales de Azure y el tipo de disco de estas. Puede obtener recomendaciones de dimensionamiento en función del historial de rendimiento (para optimizar los recursos a con la migración) o según la configuración de la máquina local, sin el historial de rendimiento. Si es una evaluación de base de datos, verá las recomendaciones de SKU para la base de datos, el plan de tarifa y el nivel de proceso.  

### <a name="get-compute-costs"></a>Obtención de los costos de proceso

La opción de dimensionamiento en función del rendimiento de las evaluaciones de Azure Migrate ayuda a ajustar el tamaño de las máquinas virtuales y debe usarse como procedimiento recomendado para optimizar las cargas de trabajo en Azure. Además de ajustar el tamaño, hay otras opciones que ahorran costos con Azure: 

- **Instancias reservadas**: con las [instancias reservadas (RI)](https://azure.microsoft.com/pricing/reserved-vm-instances/), puede reducir significativamente los costos en comparación con los [precios de pago por uso](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/).
- **Ventaja híbrida de Azure**: con [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), puede llevar a Azure licencias de Windows Server locales con Software Assurance activo o suscripciones de Linux, y combinarlas con las opciones de las instancias reservadas.
- **Contrato Enterprise**: los [Contratos Enterprise (EA)](../cost-management-billing/manage/ea-portal-agreements.md) de Azure pueden ofrecer ahorros para suscripciones y servicios de Azure.
- **Ofertas**: hay varias [ofertas de Azure](https://azure.microsoft.com/support/legal/offer-details/). Por ejemplo, [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/pricing/dev-test/), o la [oferta de Desarrollo/pruebas - Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/), para proporcionar tarifas reducidas para las máquinas virtuales de desarrollo y pruebas.
- **Tiempo de actividad de la máquina virtual**: puede revisar el tiempo de ejecución de las máquinas virtuales de Azure (días al mes y horas al día). Apagar los equipos cuando no están en uso puede reducir los costos (no es aplicable a las instancias reservadas).
- **Target region** (Región de destino): puede crear evaluaciones en distintas regiones para averiguar si la migración a una región específica es más rentable. 

### <a name="visualize-data"></a>Visualización de datos

Puede ver los informes de evaluación del servidor (con información sobre el estado de preparación para Azure y la distribución de los costos mensuales) en el portal. También puede exportar la evaluación y enriquecer el plan de migración con visualizaciones adicionales. Puede crear varias evaluaciones, con diferentes combinaciones de propiedades, y elegir el conjunto de propiedades que mejor se adapten a su negocio.  

 ![Introducción a las evaluaciones](./media/concepts-migration-planning/assessment-summary.png)

### <a name="evaluate-gapsblockers"></a>Evaluación de brechas/bloqueadores

A medida que se determinen las aplicaciones y las cargas de trabajo que se van a migrar, identifique las restricciones de tiempo de inactividad para ellas y busque las dependencias operativas entre las aplicaciones y la infraestructura subyacente. Este análisis ayuda a planear las migraciones que cumplen su objetivo de tiempo de recuperación (RTO) y a garantizar una pérdida de datos mínima o nula. Antes de la migración se recomienda revisar y mitigar los problemas de compatibilidad o las características no admitidas que pueden bloquear la migración de la base de datos SQL o del servidor. El informe de Azure Migrate Server Assessment y Azure Migrate Database Assessment pueden ayudarle con esto. 

### <a name="prioritize-workloads"></a>Priorización de las cargas de trabajo

Después de recopilar información sobre el inventario, puede identificar las aplicaciones y las cargas de trabajo que se van a migrar primero. Desarrolle un enfoque de "aplicación y aprendizaje" para migrar las aplicaciones de forma sistemática y controlable, de modo que pueda resolver los defectos antes de empezar una migración a gran escala.

Para elegir un orden de migración prioritario, puede usar factores estratégicos como la complejidad, el tiempo para migrar, la urgencia de la empresa, las consideraciones de producción/no producción, el cumplimiento, los requisitos de seguridad, el conocimiento de la aplicación, etc. 

Estas son algunas recomendaciones:

- **Dé prioridad a los logros rápidos**: use los informes de evaluación para identificar lo más fácil, incluidos los servidores y las bases de datos que están totalmente preparados y requieren un esfuerzo mínimo para migrar a Azure. En la tabla se resumen algunas maneras de hacerlo.

    **State** | **Acción**
    --- | ---
    **M´quinas virtuales preparadas para Azure** | Exporte el informe de evaluación y filtre todas las máquinas con estado *Preparado para Azure*. Este podría ser el primer grupo de máquinas que mover a Azure mediante la herramienta [Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool).
    **Sistemas operativos sin soporte técnico** | Exporte el informe de evaluación y filtre todas las máquinas que ejecutan Windows Server 2008 R2/Windows Server 2008. Estos sistemas operativos no tienen ya soporte técnico y solo Azure proporciona actualizaciones de seguridad gratuitas durante tres años al migrarlos a Azure. Al combinar Ventaja híbrida de Azure e instancias reservadas, el ahorro podría ser mucho mayor.
    **Migración de SQL Server** | Use las recomendaciones de evaluación de bases de datos para migrar las bases de datos preparadas para Azure SQL Database mediante la herramienta Azure Migrate: Database Migration. Migre las bases de datos y las bases de datos preparadas para la máquina virtual de Azure SQL mediante Azure Migrate: Herramienta de migración del servidor.
    **Software sin soporte técnico** | Exporte el inventario de aplicaciones y filtre el software o las extensiones con un plan de soporte técnico a punto de terminar. Dé prioridad a la migración de estas aplicaciones.
    **Máquinas con aprovisionamiento insuficiente** | Exporte el informe de evaluación y filtre las máquinas con un uso de CPU (%) y de memoria (%) bajo.  Migre a una máquina virtual de Azure de tamaño correcto y ahorre costos por recursos no aprovechados.
    **Máquinas con aprovisionamiento excesivo** | Exporte el informe de evaluación y filtre las máquinas con un uso de CPU (%) y de memoria (%) elevado.  Solucione las restricciones de capacidad, evite los errores por la sobrecarga en las máquinas y aumente el rendimiento de estas al migrarlas a Azure. En Azure, use las funcionalidades de escalado automático para satisfacer la demanda.<br/><br/> Analice los informes de evaluación para investigar las restricciones de almacenamiento. Analice el número de operaciones de entrada y salida, y el rendimiento del disco, así como el tipo de disco recomendado.

- **Empiece poco a poco, hasta el máximo**: comience con el traslado de aplicaciones y cargas de trabajo que presenten un riesgo y complejidad mínimos para generar confianza en la estrategia de migración. Analice las recomendaciones de evaluación de Azure Migrate junto con el repositorio de la base de datos de administración de configuración para buscar y usar cargas de trabajo de desarrollo y pruebas como candidatas para migraciones piloto. Los comentarios y lo aprendido de las migraciones piloto pueden resultar de utilidad a la hora de empezar a migrar las cargas de trabajo de producción.  
- **Cumplimiento**: Azure mantiene la mayor cartera de cumplimiento del sector en términos de amplitud y profundidad de las ofertas. Use los requisitos de cumplimiento para priorizar las migraciones, de modo que las aplicaciones y las cargas de trabajo cumplan las normas y la legislación a nivel nacional y regional, y específicas del sector. Esto es especialmente cierto para las organizaciones con procesos crítico para la empresa, las que contienen información confidencial o las que se encuentran en sectores con regulaciones muy estrictas. En estos tipos de organizaciones, las normas y las regulaciones abundan y pueden cambiar con frecuencia, lo que dificulta el seguimiento.  

## <a name="finalize-the-migration-plan"></a>Finalización del plan de migración

Antes de finalizar el plan de migración, asegúrese de considerar y mitigar otros posibles bloqueos, como se indica a continuación: 

- **Requisitos de red**: evalúe las restricciones de ancho de banda de red y de latencia que puedan provocar retrasos imprevistos e interrumpir la velocidad de replicación de la migración.
- **Ajustes de prueba/postmigración**: permite que un búfer de tiempo realice pruebas de rendimiento y de aceptación del usuario para las aplicaciones migradas, o configure o ajuste las aplicaciones después de la migración, como la actualización de las cadenas de conexión de base de datos, la configuración de los servidores web, la realización de transferencias/limpiezas, etc.
- **Permisos**: revise los permisos de Azure recomendados y los roles de acceso de servidor/base de datos, así como los permisos necesarios para la migración.
- **Aprendizaje**: prepare su organización para la transformación digital. Una base de entrenamiento sólida es importante para el cambio correcto de la organización. Consulte los cursos de aprendizaje gratuitos de [Microsoft Learn](/learn/azure/?ocid=CM_Discovery_Checklist_PDF), incluidos los cursos sobre aspectos básicos de Azure, la arquitectura de la solución y la seguridad. Anime a su equipo a que explore las  [certificaciones de Azure](https://www.microsoft.com/learning/certification-overview.aspx?ocid=CM_Discovery_Checklist_PDF).  
- **Soporte técnico para la implementación**: obtenga soporte técnico para su implementación si lo necesita. Muchas organizaciones optan por la ayuda externa para respaldar la migración en la nube. Para pasar a Azure de forma rápida y confiable con asistencia personalizada, considere la posibilidad de un  [proveedor de servicios administrados por expertos de Azure](https://www.microsoft.com/solution-providers/search?cacheId=9c2fed4f-f9e2-42fb-8966-4c565f08f11e&ocid=CM_Discovery_Checklist_PDF) o  [FastTrack for Azure](https://azure.microsoft.com/programs/azure-fasttrack/?ocid=CM_Discovery_Checklist_PDF).  


Cree un plan de migración a la nube efectivo que incluya información detallada sobre las aplicaciones que desea migrar, la disponibilidad de la aplicación y la base de datos, las restricciones de tiempo de inactividad, y los hitos de migración. El plan considera el tiempo que tardará la copia de datos e incluye un búfer realista para las pruebas posteriores a la migración y las actividades de transferencia. 

Un plan de pruebas posterior a la migración debe incluir casos de uso sobre la función, la integración, la seguridad y pruebas de rendimiento para garantizar que las aplicaciones migradas funcionan según lo previsto y que todos los objetos de base de datos y las relaciones de los datos se han transferido correctamente a la nube.  

Cree un mapa de ruta de migración y declare una ventana de mantenimiento para migrar las aplicaciones y bases de datos con un tiempo de inactividad mínimo o inexistente y limitar el posible impacto operativo o empresarial durante la migración.  

## <a name="migrate"></a>Migrar

Se recomienda ejecutar una migración de prueba en Azure Migrate antes de iniciar una migración a gran escala. Una migración de prueba ayuda a calcular el tiempo necesario y a ajustar el plan de migración. Proporciona una oportunidad para detectar posibles problema y corregirlos antes de la migración completa.

Cuando esté listo para la migración, use la herramienta Azure Migrate: Server Migration y Azure Database Migration Service (DMS) para obtener una experiencia de migración fluida e integrada con seguimiento de un extremo a otro.

- Con la herramienta Server Migration puede migrar máquinas virtuales y servidores locales, o máquinas virtuales ubicadas en otra nube privada o pública (como AWS, GCP) casi sin tiempo de inactividad.
- Azure DMS proporciona un servicio totalmente administrado diseñado para permitir migraciones perfectas a partir de varios orígenes de base de datos en las plataformas de datos de Azure con un tiempo de inactividad mínimo.  

## <a name="next-steps"></a>Pasos siguientes

- Investigue el  [recorrido de la migración en la nube](/azure/architecture/cloud-adoption/getting-started/migrate)  en el marco de Cloud Adoption Framework para Azure.
- Obtenga una [visión global rápida](migrate-services-overview.md) de Azure Migrate y vea un [vídeo de introducción](https://youtu.be/wFfq3YPxYHE).
- Más información sobre la evaluación de las máquinas virtuales para la migración a [máquinas virtuales de Azure](concepts-assessment-calculation.md).
