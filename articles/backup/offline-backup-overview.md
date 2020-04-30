---
title: Introducción a la copia de seguridad sin conexión
description: Conozca los componentes de la copia de seguridad sin conexión. Por ejemplo, la copia de seguridad sin conexión basada en Azure Data Box y la copia de seguridad sin conexión basada en el servicio Azure Import/Export.
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: a33810c633d8c7ca79c559355935d3c11853bc0a
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2020
ms.locfileid: "82160928"
---
# <a name="overview-of-offline-backup"></a>Introducción a la copia de seguridad sin conexión

En este artículo se ofrece una introducción a la copia de seguridad sin conexión.

Normalmente, las copias de seguridad iniciales completas en Azure transfieren grandes cantidades de datos en línea y requieren un mayor ancho de banda de red en comparación con las copias de seguridad sucesivas que solo transfieren los cambios incrementales. Las oficinas remotas o los centros de datos en determinadas zonas geográficas no siempre tienen suficiente ancho de banda de red. Por este motivo, estas copias de seguridad iniciales tardan varios días. Durante este tiempo, las copias de seguridad usan continuamente la misma red aprovisionada con las aplicaciones que se ejecutan en el centro de datos local.

Azure Backup admite la copia de seguridad sin conexión, que permite transferir los datos de copia de seguridad inicial sin conexión, sin el uso de ancho de banda de red. Proporciona un mecanismo para copiar los datos de copia de seguridad en dispositivos de almacenamiento físico. Los dispositivos se envían después a un centro de recursos de Azure cercano y se cargan en un almacén de Recovery Services. Este proceso garantiza una transferencia sólida de datos de copia de seguridad sin tener que usar ningún ancho de banda de red.

## <a name="offline-backup-options"></a>Opciones de copia de seguridad sin conexión

La copia de seguridad sin conexión se ofrece en dos modos en función de la propiedad de los dispositivos de almacenamiento:

- Copia de seguridad sin conexión basada en Azure Data Box (versión preliminar)
- Copia de seguridad sin conexión basada en el servicio Azure Import/Export

## <a name="offline-backup-based-on-azure-data-box-preview"></a>Copia de seguridad sin conexión basada en Azure Data Box (versión preliminar)

Este modo es compatible actualmente con el agente de Microsoft Azure Recovery Services (MARS), en su versión preliminar. Esta opción aprovecha las ventajas de [Azure Data Box](https://azure.microsoft.com/services/databox/) para enviar dispositivos de transferencia seguros y resistentes a la manipulación propiedad de Microsoft con conectores USB a su centro de seguridad u oficina remota. Los datos de copia de seguridad se escriben directamente en estos dispositivos. Esta opción ahorra el esfuerzo necesario para obtener sus propios discos y conectores compatibles con Azure o aprovisionar almacenamiento temporal como ubicación de almacenamiento provisional. Además, Microsoft controla la logística de transferencia de un extremo a otro, que puede seguir a través de Azure Portal.

A continuación se muestra una arquitectura que describe el movimiento de los datos de copia de seguridad con esta opción.

![Arquitectura de copia de seguridad con Azure Data Box](./media/offline-backup-overview/azure-backup-databox-architecture.png)

Este es un resumen de la arquitectura:

1. Azure Backup copia directamente los datos de copia de seguridad en estos dispositivos preconfigurados.
2. Después, puede enviar estos dispositivos de vuelta a un centro de datos de Azure.
3. Azure Data Box copia los datos en una cuenta de almacenamiento propiedad del cliente.
4. Azure Backup copia automáticamente los datos de copia de seguridad de la cuenta de almacenamiento en el almacén de Recovery Services designado. Se programan copias de seguridad en línea incrementales.

Para usar copia de seguridad sin conexión basada en Azure Data Box, consulte [Copia de seguridad sin conexión con Azure Data Box](offline-backup-azure-data-box.md).

## <a name="offline-backup-based-on-the-azure-importexport-service"></a>Copia de seguridad sin conexión basada en el servicio Azure Import/Export

Esta opción es compatible con Microsoft Azure Backup Server (MABS), System Center Data Protection Manager (DPM) DPM-A y el agente de MARS. Además, usa el [servicio Azure Import/Export](https://docs.microsoft.com/azure/storage/common/storage-import-export-service). Puede transferir los datos de la copia de seguridad inicial a Azure mediante sus propios discos y conectores compatibles con Azure. Este enfoque requiere el aprovisionamiento de un almacenamiento temporal conocido como ubicación de almacenamiento provisional y el uso de utilidades pregeneradas para dar formato y copiar los datos de copia de seguridad en discos propiedad del cliente.

A continuación se muestra una arquitectura que describe el movimiento de los datos de copia de seguridad con esta opción.

![Arquitectura del servicio Azure Import/Export de Azure Backup](./media/offline-backup-overview/azure-backup-import-export.png)

Este es un resumen de la arquitectura:

1. En lugar de enviar los datos de la copia de seguridad a través de la red, Azure Backup escribe los datos en una ubicación de almacenamiento provisional.
2. Los datos de la ubicación de almacenamiento provisional se escriben en uno o varios discos SATA mediante una utilidad personalizada.
3. Como parte del trabajo de preparación, la utilidad crea un trabajo de importación de Azure. Las unidades de disco SATA se envían al centro de datos de Azure más cercano y hacen referencia al trabajo de importación para conectar las actividades.
4. En el centro de datos de Azure, los datos de los discos se copian en una cuenta de almacenamiento de Azure.
5. Azure Backup copia los datos de la copia de seguridad de la cuenta de almacenamiento al almacén de Recovery Services. Se programan copias de seguridad incrementales.

Para usar la copia de seguridad sin conexión basada en el servicio Azure Import/Export con el agente de MARS, consulte [Flujo de trabajo de copia de seguridad sin conexión en Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export).

Para usar lo mismo con MABS o DPM-A, consulte [Flujo de copia de seguridad sin conexión para DPM y Azure Backup Server](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-).

## <a name="offline-backup-support-summary"></a>Resumen de compatibilidad de la copia de seguridad sin conexión

En la tabla siguiente se comparan las dos opciones disponibles, de modo que pueda tomar las decisiones adecuadas en función de su escenario.

| **Consideración**                                            | **Copia de seguridad sin conexión basada en Azure Data Box**                     | **Copia de seguridad sin conexión basada en el servicio Azure Import/Export**                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Modelos de implementación de Azure Backup                              | Agente de MARS (versión preliminar)                                              | Agente de MARS, MABS, DPM-A                                           |
| Número máximo de datos de copia de seguridad por servidor (MARS) o por grupo de protección (MABS, DPM-A) | [Disco de Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-disk-overview): 7,2 TB <br> [Azure Data Box:](https://docs.microsoft.com/azure/databox/data-box-overview) 80 TB       | 80 TB (hasta 10 discos de 8 TB cada uno)                          |
| Seguridad (datos, dispositivo y servicio)                           | [Datos](https://docs.microsoft.com/azure/databox/data-box-security#data-box-data-protection): cifrados con AES de 256 bits <br> [Dispositivo](https://docs.microsoft.com/azure/databox/data-box-security#data-box-device-protection): interfaz propia basada en credenciales, en carcasa resistente para copiar datos <br> [Servicio](https://docs.microsoft.com/azure/databox/data-box-security#data-box-service-protection): protegido por las características de seguridad de Azure | Datos: cifrados con BitLocker                                 |
| Aprovisionamiento de ubicación de almacenamiento provisional temporal                     | No se requiere                                                | Mayor o igual que el tamaño estimado de los datos de copia de seguridad        |
| Regiones admitidas                                           | [Regiones de Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) <br> [Regiones de Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) | [Regiones de servicios Azure Import/Export](https://docs.microsoft.com/azure/storage/common/storage-import-export-service#region-availability) |
| Envío entre países                                     | No compatible  <br>    La dirección de origen y el centro de datos de Azure de destino deben estar en el mismo país* | Compatible                                                    |
| Logística de transferencia (entrega, transporte, recogida)           | Administrada totalmente por Microsoft                                     | Administrado por el cliente                                            |
| Precios                                                      | [Precios de Azure Data Box](https://azure.microsoft.com/pricing/details/databox/) <br> [Precios de Azure Data Box Disk](https://azure.microsoft.com/pricing/details/databox/disk/) | [Precios del servicio Azure Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/) |

*Si su país no tiene un centro de datos de Azure, debe enviar los discos a uno situado en otro país.

## <a name="next-steps"></a>Pasos siguientes

- [Copia de seguridad sin conexión de Azure Backup con Azure Data Box](offline-backup-azure-data-box.md#backup-data-size-and-supported-data-box-skus)
- [Flujo de trabajo de copia de seguridad sin conexión en Azure Backup](backup-azure-backup-import-export.md)
- [Flujo de copia de seguridad sin conexión para DPM y Azure Backup Server](backup-azure-backup-server-import-export-.md)
