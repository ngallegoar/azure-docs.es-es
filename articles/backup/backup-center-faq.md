---
title: 'Centro de copia de seguridad: preguntas más frecuentes'
description: En este artículo se responden a las preguntas más frecuentes sobre el Centro de copia de seguridad
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: b10a9e73e65cf12c43ce28b429a8f12e0b960a76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993825"
---
# <a name="backup-center---frequently-asked-questions"></a>Centro de copia de seguridad: preguntas más frecuentes

## <a name="management"></a>Administración

### <a name="can-backup-center-be-used-across-tenants"></a>¿Puede usarse el Centro de copia de seguridad en los inquilinos?

Sí, si usa [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/overview) y tiene acceso delegado a las suscripciones en diferentes inquilinos, puede usar el Centro de copia de seguridad como panel único para administrar copias de seguridad en los inquilinos.

### <a name="can-backup-center-be-used-to-manage-both-recovery-services-vaults-and-backup-vaults"></a>¿Se puede usar el Centro de copia de seguridad para administrar tanto almacenes de Recovery Services como almacenes de Backup?

Sí, el Centro de copia de seguridad puede administrar tanto [almacenes de Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) como [almacenes de Backup](backup-vault-overview.md).

### <a name="is-there-a-delay-before-data-surfaces-in-backup-center"></a>¿Hay un retraso antes de que se muestren los datos en el Centro de copia de seguridad?

El objetivo del Centro de copia de seguridad es proporcionar información en tiempo real. Puede haber unos segundos de retraso entre el momento en el que aparece una entidad en una pantalla de almacén individual y el momento en el que aparece la misma entidad en el Centro de copia de seguridad.

## <a name="configuration"></a>Configuración

### <a name="do-i-need-to-configure-anything-to-see-data-in-backup-center"></a>¿Es necesario configurar algo para ver los datos en el Centro de copia de seguridad?

No. El Centro de copia de seguridad está listo para usarse. Sin embargo, para ver [Informes de Backup](https://docs.microsoft.com/azure/backup/configure-reports) en el Centro de copia de seguridad, debe configurar informes para los almacenes.

### <a name="do-i-need-to-have-any-special-permissions-to-use-backup-center"></a>¿Es necesario tener permisos especiales para usar el Centro de copia de seguridad?

El Centro de copia de seguridad como tal no necesita nuevos permisos. Siempre y cuando tenga el nivel adecuado de acceso a RBAC para los recursos que administra, puede usar el Centro de copia de seguridad para estos recursos. Por ejemplo, para ver información sobre sus copias de seguridad, necesitará acceso de **Lector** a los almacenes. Para configurar la copia de seguridad y realizar otras acciones relacionadas con la copia de seguridad, necesitará los roles **Colaborador de copias de seguridad** u **Operador de copias de seguridad**. Obtenga información acerca de los [roles RBAC para Azure Backup](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="pricing"></a>Precios

### <a name="do-i-need-to-pay-anything-extra-to-use-backup-explorer"></a>¿Es necesario pagar algo adicional para usar el Explorador de Backup?

Actualmente, no hay ningún costo adicional (aparte de los costos de la copia de seguridad) para usar el Centro de copia de seguridad. Sin embargo, si usa [Informes de Backup](https://docs.microsoft.com/azure/backup/configure-reports) en el Centro de copia de seguridad, hay un [costo implicado](https://azure.microsoft.com/pricing/details/monitor/) en el uso de registros de Azure Monitor para Informes de Backup.

## <a name="next-steps"></a>Pasos siguientes

Lea las otras preguntas más frecuentes:

* [Preguntas comunes sobre los almacenes de Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq)
* [Preguntas comunes sobre las copias de seguridad de máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq)
