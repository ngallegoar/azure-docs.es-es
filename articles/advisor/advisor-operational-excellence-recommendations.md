---
title: Mejora de la excelencia operativa para las suscripciones de Azure con Azure Advisor
description: Uso de Advisor para optimizar y obtener experiencia en la excelencia operativa de sus suscripciones de Azure
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: 4133fbe40c0fc1bdaf9c3f734bc19f94437c9acb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187423"
---
# <a name="achieve-operational-excellence-with-azure-advisor"></a>Obtención de la excelencia operacional con Azure Advisor

Las recomendaciones de excelencia operativa de Azure Advisor ayudan a los clientes con la eficacia del proceso y el flujo de trabajo, la administración de recursos y las prácticas recomendadas de implementación. Puede consultar estas recomendaciones de Advisor en la pestaña de **Excelencia operativa** del panel de Advisor.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Creación de alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten

Es recomendable configurar alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten. [Azure Service Health](https://azure.microsoft.com/features/service-health/) es un servicio gratuito que ofrece una guía personalizada y soporte técnico cuando experimenta un problema con algún servicio de Azure. Advisor identifica las suscripciones que no tienen alertas configuradas y recomienda crearlas.

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Diseño de las cuentas de almacenamiento para evitar alcanzar el límite máximo de la suscripción

Una región de Azure puede admitir un máximo de 250 cuentas de almacenamiento por suscripción. Una vez alcanzado el límite, no podrá crear más cuentas de almacenamiento en esa combinación de región y suscripción. Advisor comprobará las suscripciones y recomendaciones de superficie para que pueda diseñar menos cuentas de almacenamiento para cualquiera que esté a punto de alcanzar el límite máximo.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Asegúrese de tener acceso a los expertos en la nube de Azure cuando lo necesite

Cuando se ejecuta una carga de trabajo crítica para la empresa, es importante tener acceso al soporte técnico cuando sea necesario. Advisor identifica posibles suscripciones críticas para la empresa que no tienen soporte técnico incluido en su plan de soporte técnico y recomienda actualizar a una opción que incluya soporte técnico.

## <a name="repair-invalid-log-alert-rules"></a>Reparación de las reglas de alertas de registro no válidas

Azure Advisor detectará las reglas de alertas que tienen consultas no válidas especificadas en la sección de condiciones. Las reglas de alertas de registro se crean en Azure Monitor y se usan para ejecutar consultas de análisis en intervalos especificados. Los resultados de la consulta determinan si se debe desencadenar una alerta. Las consultas de análisis pueden convertirse en no válidas con el paso del tiempo debido a cambios en los comandos, las tablas o los recursos a los que se hace referencia. Advisor le recomendará que corrija la consulta en la regla de alertas para evitar que se deshabilite automáticamente y garantizar la cobertura de la supervisión de los recursos de Azure. [Más información sobre cómo solucionar problemas de las reglas de alertas](https://aka.ms/aa_logalerts_queryrepair)

## <a name="follow-best-practices-using-azure-policy"></a>Aplicación de las prácticas recomendadas con Azure Policy

Azure Policy es un servicio de Azure que se usa para crear, asignar y administrar directivas. Estas directivas aplican diferentes reglas y efectos a los recursos. A continuación se muestran las recomendaciones de Azure Policy para ayudarle a lograr la excelencia operativa: 
1. Administre las etiquetas mediante Azure Policy: Esta directiva agrega o reemplaza la etiqueta y el valor especificados cuando se crea o actualiza cualquier recurso. Los recursos existentes se pueden corregir con una tarea de corrección. Además, no modifica las etiquetas de los grupos de recursos.
2. Aplique los requisitos de cumplimiento geográfico mediante Azure Policy: La directiva permite restringir las ubicaciones que la organización puede especificar al implementar los recursos. 
3. Especifique las SKU de máquina virtual permitidas para las implementaciones: Esta directiva permite especificar un conjunto de SKU de máquina virtual que su organización puede implementar.
4. Aplicación de "Auditar máquinas virtuales que no usan disco administrado" con Azure Policy
5. Use "Heredar una etiqueta del grupo de recursos" mediante Azure Policy: la directiva agrega o reemplaza la etiqueta y el valor del grupo de recursos primario especificados cuando se crea o actualiza cualquier recurso. Los recursos existentes se pueden corregir con una tarea de corrección.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más sobre las recomendaciones de Advisor, consulte:
* [Introducción a Advisor](advisor-overview.md)
* [Primeros pasos](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre alta disponibilidad de Advisor](advisor-high-availability-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
