---
title: Administración de Azure
description: Información general de las áreas de administración para las aplicaciones y los recursos de Azure con vínculos a contenido sobre las herramientas de administración de Azure.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/07/2018
ms.openlocfilehash: 93180f088935531f83da785bb7a490f80909a6cd
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83816420"
---
# <a name="azure-management---monitoring"></a>Administración de Azure: supervisión

La supervisión de Azure es uno de los aspectos de la administración de Azure.  En este artículo se describen de forma sucinta las diferentes áreas de administración necesarias para implementar y mantener las aplicaciones y recursos en Azure e incluye vínculos a documentación que puede ayudarle a comenzar.

## <a name="management-in-azure"></a>Administración en Azure

Administración hace referencia a las tareas y procesos necesarios para mantener las aplicaciones empresariales y los recursos que las dan soporte.  Azure tiene varios servicios y herramientas que funcionan conjuntamente para proporcionar una administración completa no solo a las aplicaciones que se ejecutan en Azure, sino también en otras nubes y un entorno local.  El conocimiento de las distintas herramientas disponibles y cómo pueden usarse conjuntamente en varios escenarios de administración es el primer paso para diseñar un entorno de administración completo.

El siguiente diagrama muestra las diferentes áreas de administración necesarias para mantener cualquier aplicación o recurso.  Estas diferentes áreas pueden concebirse como las distintas partes de un ciclo de vida, donde cada una de ellos es necesaria en una sucesión continua a lo largo de la duración de un recurso.  Comienza con su implementación inicial, pasa por el funcionamiento continuado y, por último, el momento en que se retira.

![Funcionalidades de administración](media/management-overview/management-capabilities.png)


En las secciones siguientes se describen sucintamente las distintas áreas de administración y se proporcionan vínculos a información detallada acerca de los principales servicios de Azure diseñados para trabajar en ellas.

## <a name="monitor"></a>Supervisión
La supervisión es el acto de recopilar y analizar datos para determinar el rendimiento, el mantenimiento y la disponibilidad de su aplicación empresarial y de los recursos de los que esta depende. Una estrategia de supervisión eficaz facilitará la comprensión de la operación detallada de los distintos componentes de la aplicación y aumentará el tiempo de actividad con la notificación anticipada de errores críticos, para poder solucionarlos antes de que se conviertan en problemas. La supervisión en Azure se proporciona principalmente con [Azure Monitor](../azure-monitor/overview.md), que ofrece almacenes comunes para almacenar los datos de supervisión, múltiples orígenes de datos para recopilar datos de los diferentes niveles que admiten la aplicación, y características para analizar y responder a los datos recopilados.

## <a name="configure"></a>Configuración
Configurar hace referencia a la implementación inicial y a la configuración de aplicaciones y recursos, y a su mantenimiento continuado con revisiones y actualizaciones.  La automatización de estas tareas a través de scripts y directivas le permite eliminar la redundancia, reducir el tiempo y esfuerzo que debe invertir, y aumentar la precisión y eficiencia.  [Azure Automation](../automation/automation-intro.md) proporciona la mayor parte de los servicios para automatizar las tareas de configuración.  Además de runbooks para automatizar los procesos, proporciona configuración y administración de las actualizaciones, lo que le ayudará no solo a administrar la configuración mediante directivas, sino también a identificar e implementar las actualizaciones.

## <a name="govern"></a>Control
La gobernanza proporciona mecanismos y procesos para mantener el control de las aplicaciones y recursos de Azure.  Conlleva la planificación de las iniciativas y el establecimiento de prioridades estratégicas.  En Azure, la gobernanza se implementa principalmente con dos servicios.  [Azure Policy](../governance/policy/overview.md) permite crear, asignar y administrar definiciones de directivas que aplican distintas reglas y acciones a los recursos, con el fin de que estos sigan siendo compatibles con los estándares corporativos y los contratos de nivel de servicio. [Azure Cost Management](../cost-management-billing/cost-management-billing-overview.md) permite realizar un seguimiento del uso y de los gastos de la nube de los recursos no solo de Azure, sino también de otros proveedores de servicios en la nube, entre los que se incluyen AWS y Google.

## <a name="secure"></a>Seguridad
La administración de la seguridad de las aplicaciones, recursos y datos conlleva la combinación de la evaluación de las amenazas, la recopilación y el análisis de los datos de seguridad, y la garantía de que tanto las aplicaciones como los recursos están diseñados y configurados de forma segura.  La supervisión de la seguridad y el análisis de amenazas los proporciona [Azure Security Center](../security-center/security-center-intro.md), que incluye una administración unificada de la seguridad y una protección avanzada contra amenazas para cargas de trabajo en la nube híbrida.  También debe consultar [Introducción a la seguridad de Azure](../security/fundamentals/overview.md), donde encontrará información completa acerca la seguridad en Azure e instrucciones para configurar de forma segura los recursos de Azure.


## <a name="protect"></a>Protección
Por protección se entiende garantizar que las aplicaciones y los datos estén siempre disponibles, incluso en el caso de interrupciones fuera de su control.  En Azure, la protección se proporciona mediante dos servicios.  [Azure Backup](../backup/backup-introduction-to-azure-backup.md) proporciona copias de seguridad y recuperación de los datos, tanto los de la nube como los de un entorno local.    [Azure Site Recovery](../site-recovery/site-recovery-overview.md) garantiza un alta disponibilidad de cualquier aplicación, ya que proporciona continuidad empresarial y recuperación inmediata en caso de desastre.

## <a name="migrate"></a>Migrar 
La migración hace referencia a la transición de las cargas de trabajo que se ejecutan de forma local a la nube de Azure.  [Azure Migrate](../migrate/migrate-overview.md) es un servicio que ayuda a evaluar la idoneidad de la migración a Azure, lo que incluye el cálculo del costo y el ajuste de tamaño basados en el rendimiento, de las máquinas virtuales locales.  Azure Site Recovery puede ayudarle a realizar la migración real de máquinas virtuales bien [desde entornos locales](../site-recovery/migrate-tutorial-on-premises-azure.md) o [desde Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md).  [Azure Database Migration](../dms/dms-overview.md) le ayudará en la migración de varios orígenes de base de datos a plataformas de datos de Azure.

