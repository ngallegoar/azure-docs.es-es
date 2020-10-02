---
title: Configuración de Azure Defender para Storage
titleSuffix: Azure Storage
description: Configure Azure Defender para Storage para detectar anomalías en la actividad de la cuenta y enviarle notificaciones si hay intentos potencialmente dañinos de acceso a su cuenta.
services: storage
author: tamram
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: d2ad302042b277cf29b3a7b22af88b662686b3fd
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993044"
---
# <a name="configure-azure-defender-for-storage"></a>Configuración de Azure Defender para Storage

Azure Defender para Storage proporciona una capa adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o vulnerarlas. Esta capa de protección le permite afrontar las amenazas sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de la seguridad.

Las alertas de seguridad se desencadenan cuando se producen anomalías en una actividad. Estas alertas de seguridad se integran en [Azure Security Center](https://azure.microsoft.com/services/security-center/) y también se envían por correo electrónico a los administradores de las suscripciones, con detalles de la actividad sospechosa y recomendaciones sobre cómo investigar y solucionar las amenazas.

El servicio ingiere los registros de recursos de las solicitudes de lectura, escritura y eliminación en Blob Storage y en Azure Files (versión preliminar) para la detección de amenazas. Para investigar las alertas de Azure Defender para Storage, puede ver la actividad de almacenamiento relacionada mediante el registro de Storage Analytics. Para más información, consulte **Configuración del registro** en [Supervisión de una cuenta de almacenamiento en Azure Portal](storage-monitor-storage-account.md#configure-logging).

## <a name="availability"></a>Disponibilidad

Azure Defender para Storage está disponible actualmente para Blob Storage, Azure Files (versión preliminar) y Azure Data Lake Storage Gen2 (versión preliminar). Los tipos de cuenta que admiten Azure Defender incluyen cuentas de uso general v2, de blob en bloques y de almacenamiento de blobs. Azure Defender para Storage está disponible en todas las nubes públicas y nubes del gobierno de EE. UU., pero no en otras regiones de nubes soberanas o de Azure Government.

Las cuentas con espacios de nombres jerárquicos habilitados para Data Lake Storage admiten transacciones que usan las API de Azure Blob Storage y las API de Data Lake Storage. Los recursos compartidos de Azure admiten transacciones en SMB.

Para obtener más información, incluida una evaluación gratuita de 30 días, consulte la [página de precios de Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

En la lista siguiente se resume la disponibilidad de Azure Defender para Storage:

- Estado de la versión:
  - [Blob Storage](https://azure.microsoft.com/services/storage/blobs/) (disponibilidad general)
  - [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) (la versión preliminar admite transacciones SMB y REST)
  - Azure Data Lake Storage Gen2 (versión preliminar)
- Nubes:<br>
    ✔ Nubes comerciales<br>
    ✔ US Gov<br>
    ✘ China Gov, otros gobiernos

## <a name="set-up-azure-defender"></a>Configuración de Azure Defender

Azure Defender para Storage se puede configurar de distintas formas, que se describen en las secciones siguientes.

### <a name="azure-security-center"></a>[Azure Security Center](#tab/azure-security-center)

Cuando se suscribe al nivel Estándar en Azure Security Center, Azure Defender se configura automáticamente en las cuentas de almacenamiento. Puede habilitar o deshabilitar Azure Defender para las cuentas de almacenamiento en una suscripción específica de la manera siguiente:

1. Inicie **Azure Security Center** en [Azure Portal](https://portal.azure.com).
1. En el menú principal, en **Administración**, seleccione **Precios y configuración**.
1. Seleccione la suscripción para la que desea habilitar o deshabilitar Azure Defender.
1. Seleccione **Azure Defender en** para habilitar Azure Defender para la suscripción.
1. En **Select Azure Defender plan by resource type** (Seleccionar plan de Azure Defender por tipo de recurso), busque la fila **Almacenamiento** y seleccione **Habilitado** en la columna **Plan**.
1. Guarde los cambios.

    :::image type="content" source="media/azure-defender-storage-configure/enable-azure-defender-security-center.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

Azure Defender ya está habilitado para todas las cuentas de almacenamiento de esta suscripción.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Inicie [Azure Portal](https://portal.azure.com/).
1. Vaya a la cuenta de almacenamiento. En **Configuración**, seleccione **Seguridad avanzada**.
1. Seleccione **Enable Azure Defender for Storage** (Habilitar Azure Defender para Storage).

    :::image type="content" source="media/azure-defender-storage-configure/enable-azure-defender-portal.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

Azure Defender ya está habilitado para esta cuenta de almacenamiento.

### <a name="template"></a>[Plantilla](#tab/template)

Use una plantilla de Azure Resource Manager para implementar una cuenta de Azure Storage con Azure Defender habilitado. Para más información, consulte [Storage account with Advanced Threat Protection](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/) (Cuenta de Storage con Advanced Threat Protection).

### <a name="azure-policy"></a>[Azure Policy](#tab/azure-policy)

Use una instancia de Azure Policy para habilitar Azure Defender en las cuentas de almacenamiento de un grupo de recursos o suscripción específicos.

1. Inicie la página **Directiva: Definiciones** de Azure.
1. Busque la directiva **Implementar Azure Defender en cuentas de Storage**.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy-definitions.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

1. Seleccione una suscripción a Azure o un grupo de recursos.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy2.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

1. Asigne la directiva.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy1.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

### <a name="rest-api"></a>[REST API](#tab/rest-api)

Use comandos de la API REST para crear, actualizar u obtener la configuración de Azure Defender para una cuenta de almacenamiento específica.

- [Creación de Advanced Threat Protection](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
- [Obtención de Advanced Threat Protection](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Use los siguientes cmdlets de PowerShell:

- [Habilitación de Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
- [Obtención de Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
- [Deshabilitación de Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

---

## <a name="explore-security-anomalies"></a>Exploración de anomalías de seguridad

Cuando se producen anomalías en la actividad de almacenamiento, recibe una notificación por correo electrónico con información acerca del evento de seguridad sospechoso. Los detalles del evento incluyen:

- La naturaleza de la anomalía
- El nombre de la cuenta de almacenamiento
- La hora del evento
- El tipo de almacenamiento
- Las causas posibles
- Los pasos de investigación
- Los pasos para la corrección

El correo electrónico también incluye detalles acerca de las posibles causas y las medidas recomendadas para investigar y mitigar la potencial amenaza.

:::image type="content" source="media/azure-defender-storage-configure/storage-advanced-threat-protection-alert-email.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

Las alertas de seguridad actuales se pueden revisar y administrar desde el [mosaico de alertas de seguridad](../../security-center/security-center-managing-and-responding-alerts.md) de Azure Security Center. Al hacer clic en una alerta específica se proporcionan detalles y acciones para investigar la amenaza actual y afrontar las amenazas futuras.

:::image type="content" source="media/azure-defender-storage-configure/storage-advanced-threat-protection-alert.png" alt-text="Captura de pantalla que muestra cómo habilitar Azure Defender para Storage en Security Center":::

## <a name="security-alerts"></a>Alertas de seguridad

Las alertas las generan los intentos inusuales y potencialmente dañinos de acceso o aprovechamiento de cuentas de almacenamiento. Para obtener una lista de las alertas de Azure Storage, consulte [Alertas para Azure Storage](../../security-center/alerts-reference.md#alerts-azurestorage).

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [Registros en cuentas de Azure Storage](/rest/api/storageservices/About-Storage-Analytics-Logging)
- Más información acerca de [Azure Security Center](../../security-center/security-center-intro.md)
