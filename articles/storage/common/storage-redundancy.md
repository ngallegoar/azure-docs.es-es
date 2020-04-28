---
title: Redundancia de datos
titleSuffix: Azure Storage
description: Los datos de su cuenta de Microsoft Azure Storage se replican para garantizar la durabilidad y la alta disponibilidad. Entre las configuraciones de redundancia se incluyen el almacenamiento con redundancia local (LRS), el almacenamiento con redundancia de zona (ZRS), el almacenamiento con redundancia geográfica (GRS), el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS), el almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar) y el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS) (versión preliminar).
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 78f7c935e64276e7f4862dad966b99bff6bd246d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81481937"
---
# <a name="azure-storage-redundancy"></a>Redundancia de Azure Storage

Azure Storage siempre almacena varias copias de los datos, con el fin de protegerlos de eventos planeados y no planeados, como errores transitorios del hardware, interrupciones del suministro eléctrico o cortes de la red y desastres naturales masivos. La redundancia garantiza que la cuenta de almacenamiento cumpla el [contrato de nivel de servicio (SLA) para Azure Storage](https://azure.microsoft.com/support/legal/sla/storage/), incluso en caso de errores.

A la hora de decidir qué opción de redundancia es la más adecuada para su escenario, tenga en cuenta la solución intermedia entre los costos más bajos y disponibilidad y durabilidad más altas. Entre los factores que ayudan a determinar qué opción de redundancia debe elegir se incluye:  

- Cómo se replican los datos en la región primaria.
- Si los datos se replicarán en una segunda ubicación que está alejada geográficamente de la región primaria, para protegerse frente a desastres regionales.
- Si la aplicación necesita acceso de lectura a los datos replicados en la región secundaria en caso de que la región primaria deje de estar disponible por cualquier motivo.

## <a name="redundancy-in-the-primary-region"></a>Redundancia en la región primaria

Los datos de una cuenta de Azure Storage siempre se replican tres veces en la región primaria. Azure Storage ofrece dos métodos para replicar los datos en la región primaria:

- El **almacenamiento con redundancia local (LRS)** copia los datos de forma sincrónica tres veces dentro de una única ubicación física en la región primaria. LRS es la opción de replicación menos costosa, pero no se recomienda para las aplicaciones que requieren de alta disponibilidad.
- El **almacenamiento con redundancia de zona (ZRS)** copia los datos de forma sincrónica en tres zonas de disponibilidad de Azure en la región primaria. En el caso de las aplicaciones que requieren de alta disponibilidad, Microsoft recomienda usar ZRS en la región primaria, además de replicación en una región secundaria.

### <a name="locally-redundant-storage"></a>Almacenamiento con redundancia local

El almacenamiento con redundancia local (LRS) replica los datos tres veces dentro de una única ubicación física en la región primaria. El almacenamiento con redundancia local proporciona una durabilidad mínima del 99,999999999 % (once nueves) de los objetos en un año determinado.

LRS es la opción de redundancia de costo más bajo y ofrece la menor durabilidad en comparación con otras opciones. LRS protege los datos frente a errores en la estantería de servidores y en la unidad. No obstante, si se produce un desastre como un incendio o una inundación en el centro de datos, es posible que todas las réplicas de una cuenta de almacenamiento con LRS se pierdan o no se puedan recuperar. Para mitigar este riesgo, Microsoft recomienda el uso del [almacenamiento con redundancia de zona](#zone-redundant-storage) (ZRS), el [almacenamiento con redundancia geográfica](#geo-redundant-storage) (GRS) o el [almacenamiento con redundancia de zona geográfica (versión preliminar)](#geo-zone-redundant-storage-preview) (GZRS).

Las solicitudes de escritura a una cuenta de almacenamiento que usa LRS se producen de forma sincrónica. Las operaciones de escritura se devuelven correctamente solo después de que los datos se escriben en las tres réplicas.

LRS es una buena opción para los siguientes escenarios:

- Si la aplicación almacena los datos que se pueden reconstruir fácilmente si se produce una pérdida de datos, puede optar por LRS.
- Si la aplicación está restringida a la replicación de datos solo dentro de un país o región debido a requisitos de gobernanza de datos, puede optar por LRS. En algunos casos, las regiones emparejadas en las que los datos se replican geográficamente pueden estar en otro país o región. Para más información sobre las regiones emparejadas, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).

### <a name="zone-redundant-storage"></a>Almacenamiento con redundancia de zona

El almacenamiento con redundancia de zona (ZRS) replica los datos de Azure Storage de forma sincrónica en tres zonas de disponibilidad de Azure en la región primaria. Cada zona de disponibilidad es una ubicación física individual con alimentación, refrigeración y redes independientes. ZRS proporciona a los objetos de datos de Azure Storage una durabilidad de al menos el 99,9999999999 % (doce nueves) durante un año determinado.

Con ZRS, los datos son accesibles para las operaciones de escritura y lectura incluso si una zona deja de estar disponible. Si una zona pasa a no estar disponible, Azure realiza actualizaciones de red, como el redireccionamiento de DNS. Estas actualizaciones pueden afectar a la aplicación si se accede a los datos antes de que se completen dichas actualizaciones. Al diseñar aplicaciones para ZRS, siga los procedimientos para el control de errores transitorios, incluida la implementación de directivas de reintentos con retroceso exponencial.

Las solicitudes de escritura a una cuenta de almacenamiento que usa ZRS se producen de forma sincrónica. Las operaciones de escritura se devuelven correctamente solo después de que los datos se escriben en todas las réplicas de las tres zonas de disponibilidad.

Microsoft recomienda el uso de ZRS en la región primaria para las situaciones que requieren coherencia, durabilidad y alta disponibilidad. ZRS ofrece un rendimiento excelente, una latencia baja y resistencia para los datos si dicha región deja de estar disponible temporalmente. No obstante, ZRS por sí solo podría no proteger los datos frente a un desastre regional en el que varias zonas resulten afectadas permanentemente. Para protegerse frente a desastres regionales, Microsoft recomienda usar [almacenamiento con redundancia de zona geográfica](#geo-zone-redundant-storage-preview) (GZRS), que usa ZRS en la región primaria y también replica geográficamente los datos en una región secundaria.

En la tabla siguiente se muestran los tipos de cuentas de almacenamiento que admiten ZRS en cada región:

|    Tipo de cuenta de almacenamiento    |    Regiones admitidas    |    Servicios admitidos    |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
|    Uso general v2<sup>1</sup>    | Sudeste de Asia<br /> Este de Australia<br /> Norte de Europa<br />  Oeste de Europa<br /> Centro de Francia<br /> Japón Oriental<br /> Norte de Sudáfrica<br /> Sur de Reino Unido 2<br /> Centro de EE. UU.<br /> Este de EE. UU.<br /> Este de EE. UU. 2<br /> Oeste de EE. UU. 2    |    Blobs en bloques<br /> Blobs en páginas<sup>2</sup><br /> Recursos compartidos de archivos (estándar)<br /> Tablas<br /> Colas<br /> |
|    BlockBlobStorage<sup>1</sup>    | Oeste de Europa<br /> Este de EE. UU.    |    Solo blobs en bloques    |
|    FileStorage    | Oeste de Europa<br /> Este de EE. UU.    |    Solo Azure Files    |

<sup>1</sup> El nivel de archivo no se admite actualmente en las cuentas de ZRS.<br />
<sup>2</sup> Las cuentas de almacenamiento que contienen discos administrados de Azure para máquinas virtuales siempre usan almacenamiento con redundancia local. Los discos no administrados de Azure también deben usar almacenamiento con redundancia local. Es posible crear una cuenta de almacenamiento para discos no administrados de Azure que use almacenamiento con redundancia geográfica, pero no se recomienda debido a los posibles problemas de coherencia en la replicación geográfica asincrónica. Ni los discos administrados ni los no administrados admiten ZRS o GZRS. Para obtener más información acerca de los discos administrados, consulte [Precios de Azure Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).

Para obtener información sobre qué regiones admiten ZRS, consulte **Soporte técnico de servicios por región** en [¿Qué son las zonas de disponibilidad en Azure?](../../availability-zones/az-overview.md).

## <a name="redundancy-in-a-secondary-region"></a>Redundancia en una región secundaria

En el caso de las aplicaciones que requieren de alta disponibilidad, puede optar por copiar los datos de la cuenta de almacenamiento en una región secundaria que esté a cientos de kilómetros de distancia de la región primaria. Si la cuenta de almacenamiento se copia a una región secundaria, sus datos se mantienen incluso ante un apagón regional completo o un desastre del cual la región primaria no se puede recuperar.

Cuando crea una cuenta de almacenamiento, selecciona la región principal de la cuenta. La región secundaria emparejada se determina según la región primaria y no es posible cambiarla. Para obtener más información sobre las regiones compatibles con Azure, consulte [Regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).

Azure Storage ofrece dos opciones para copiar los datos a una región secundaria:

- El **almacenamiento con redundancia geográfica (GRS)** copia los datos de forma sincrónica tres veces dentro de una única ubicación física en la región primaria mediante LRS. A continuación, copia los datos de forma asincrónica en una única ubicación física en la región secundaria.
- El **almacenamiento con redundancia de zona geográfica (GZRS)** copia los datos de forma sincrónica en tres zonas de disponibilidad de Azure en la región primaria mediante ZRS. A continuación, copia los datos de forma asincrónica en una única ubicación física en la región secundaria.

La principal diferencia entre GRS y GZRS es la forma en que los datos se replican en la región primaria. Dentro de la ubicación secundaria, los datos siempre se replican de forma sincrónica tres veces mediante LRS.

Con GRS o GZRS, los datos de la ubicación secundaria no están disponible para el acceso de lectura o escritura a menos que suceda una conmutación por error en la región secundaria. Para obtener acceso de lectura a la región secundaria, configure la cuenta de almacenamiento para usar el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) o el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS). Para obtener más información, consulte [Acceso de lectura a los datos de la región secundaria](#read-access-to-data-in-the-secondary-region).

Si la región primaria deja de estar disponible, puede optar por conmutar por error a la región secundaria (versión preliminar). Una vez completada la conmutación por error, la región secundaria se convierte en la región primaria y puede leer y escribir datos de nuevo. Para obtener más información sobre la recuperación ante desastres y el método de conmutación por error a la región secundaria, consulte [Recuperación ante desastres y conmutación por error de la cuenta (versión preliminar)](storage-disaster-recovery-guidance.md).

> [!IMPORTANT]
> Dado que los datos se replican en la región secundaria de forma asincrónica, un error que afecte a la región primaria puede producir la pérdida de datos si no se puede recuperar dicha región. El intervalo entre las escrituras más recientes en la región primaria y la última escritura en la región secundaria se conoce como objetivo de punto de recuperación (RPO). El RPO indica el punto en el tiempo al que se pueden recuperar los datos. Normalmente, Azure Storage tiene un RPO inferior a 15 minutos, aunque actualmente no hay ningún contrato de nivel de servicio sobre cuánto tiempo tarda la replicación de datos en la región secundaria.

### <a name="geo-redundant-storage"></a>Almacenamiento con redundancia geográfica

El almacenamiento con redundancia geográfica (GRS) copia los datos de forma sincrónica tres veces dentro de una única ubicación física en la región primaria mediante LRS. Después, copia los datos de forma asincrónica en una única ubicación física de una región secundaria que se encuentra a cientos de miles de kilómetros de distancia de la región primaria. GRS proporciona a los objetos de datos de Azure Storage una durabilidad de al menos el 99,99999999999999 % (dieciséis nueves) durante un año determinado.

Una operación se escritura se confirma primero en la ubicación principal y se replica mediante LRS. Después, la actualización se replica de manera asincrónica en la región secundaria. Cuando los datos se escriben en la ubicación secundaria, también se replican dentro de esa ubicación con LRS.

### <a name="geo-zone-redundant-storage-preview"></a>Almacenamiento con redundancia de zona geográfica (versión preliminar)

El almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar) combina la alta disponibilidad que proporciona la redundancia entre zonas de disponibilidad con la protección frente a apagones regionales que proporciona la replicación geográfica. Los datos de una cuenta de almacenamiento de GZRS se almacenan en tres [zonas de disponibilidad de Azure](../../availability-zones/az-overview.md) en la región primaria y también se replican en una región geográfica secundaria para protegerlos frente a desastres regionales. Microsoft recomienda el uso de GZRS en aplicaciones que requieran de coherencia, durabilidad y disponibilidad máximas, además de rendimiento excelente y resistencia para la recuperación ante desastres.

Con una cuenta de almacenamiento de GZRS, puede seguir leyendo y escribiendo datos si una zona de disponibilidad deja de estar disponible o es irrecuperable. Además, los datos se mantienen en caso de un apagón completo de una región o de un desastre tras el que la región primaria no se puede recuperar. El almacenamiento con redundancia de zona geográfica (GZRS) está diseñado para proporcionar una durabilidad mínima del 99,99999999999999 % (16 nueves) de los objetos en un año determinado.

Solo las cuentas de almacenamiento de uso general v2 son compatibles con GZRS y RA-GZRS. Para más información acerca de los tipos de cuentas de almacenamiento, consulte la [Introducción a la cuenta de Azure Storage](storage-account-overview.md). GZRS y RA-GZRS admiten blobs en bloques, blobs en páginas (salvo para discos VHD), archivos, tablas y colas.

GZRS y RA-GZRS están disponibles actualmente en las versiones preliminares de las siguientes regiones:

- Sudeste de Asia
- Norte de Europa
- Oeste de Europa
- Japón Oriental
- Sur de Reino Unido 2
- Este de EE. UU.
- Este de EE. UU. 2
- Centro de EE. UU.
- Oeste de EE. UU. 2

Microsoft sigue habilitando GZRS y RA-GZRS en otras regiones de Azure. Revise periódicamente la página [Actualizaciones de servicios Azure](https://azure.microsoft.com/updates/) para información sobre las regiones admitidas.

Para obtener información sobre los precios de la versión preliminar, consulte los precios de la versión preliminar de GZRS para [blobs](https://azure.microsoft.com/pricing/details/storage/blobs), [archivos](https://azure.microsoft.com/pricing/details/storage/files/), [colas](https://azure.microsoft.com/pricing/details/storage/queues/) y [tablas](https://azure.microsoft.com/pricing/details/storage/tables/).

> [!IMPORTANT]
> Microsoft recomienda no usar las características en vista previa (GB) para las cargas de trabajo de producción.

## <a name="read-access-to-data-in-the-secondary-region"></a>Acceso de lectura a los datos de la región secundaria

El almacenamiento con redundancia geográfica (con GRS o GZRS) replica los datos en otra ubicación física de la región secundaria para protegerlos frente a los apagones regionales. Sin embargo, los datos están disponibles para su lectura solo si el cliente o Microsoft inician una conmutación por error de la región primaria a la secundaria. Cuando habilita el acceso de lectura a la región secundaria, los datos están disponibles para leerse si la región primaria deja de estar disponible. Para obtener acceso de lectura a la región secundaria, habilite el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) o el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS).

### <a name="design-your-applications-for-read-access-to-the-secondary"></a>Diseño de aplicaciones para el acceso de lectura a la región secundaria

Si la cuenta de almacenamiento está configurada para usar el acceso de lectura a la región secundaria, puede diseñar sus aplicaciones para que fácilmente pasen a leer datos de la región secundaria si la región primaria deja de estar disponible por cualquier motivo. La región secundaria siempre está disponible para el acceso de lectura, por lo que puede probar la aplicación para asegurarse de que leerá desde la región secundaria si se produce un apagón. Para obtener más información sobre cómo diseñar aplicaciones para alta disponibilidad, consulte [Diseño de aplicaciones de alta disponibilidad mediante el almacenamiento con redundancia geográfica con acceso de lectura](storage-designing-ha-apps-with-ragrs.md).

Cuando está habilitado el acceso de lectura a la región secundaria, los datos se pueden leer desde el punto de conexión secundario y desde el punto de conexión primario de la cuenta de almacenamiento. El punto de conexión secundario anexa el sufijo *–secondary* al nombre de la cuenta. Por ejemplo, si el punto de conexión primario de Blob Storage es `myaccount.blob.core.windows.net`, el punto de conexión secundario es `myaccount-secondary.blob.core.windows.net`. Las claves de acceso de la cuenta son iguales para los puntos de conexión primario y secundario.

### <a name="check-the-last-sync-time-property"></a>Comprobación de la propiedad Hora de la última sincronización

Dado que los datos se replican en la región secundaria de forma asincrónica, la región secundaria suele estar detrás de la región primaria. Si se produce un error en la región primaria, es probable que todas las operaciones de escritura en esta región no se hayan replicado todavía en la secundaria.

Para determinar qué operaciones de escritura se han replicado en la región secundaria, la aplicación puede comprobar la propiedad **Hora de la última sincronización** de la cuenta de almacenamiento. Todas las operaciones de escritura escritas en la región primaria antes de la hora de la última sincronización se han replicado correctamente en la región secundaria, lo que significa que están disponibles para leerse desde la región secundaria. Las operaciones de escritura escritas en la región primaria después de la hora de la última sincronización puede que se hayan replicado o no en la región secundaria, lo que significa que es posible que no estén disponibles para las operaciones de lectura.

Puede consultar el valor de la propiedad **Hora de la última sincronización** mediante Azure PowerShell, la CLI de Azure o una de las bibliotecas cliente de Azure Storage. La propiedad **Hora de la última sincronización** es un valor de fecha y hora GMT. Para obtener más información, consulte [Comprobación de la propiedad Hora de la última sincronización de una cuenta de almacenamiento](last-sync-time-get.md).

## <a name="summary-of-redundancy-options"></a>Resumen de las opciones de redundancia

En la tabla siguiente se muestra el grado de permanencia y disponibilidad de los datos en un escenario determinado, según el tipo de redundancia vigente para la cuenta de almacenamiento:

| Escenario                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS (versión preliminar)                              |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Un nodo de un centro de datos deja de estar disponible                                                                 | Sí                             | Sí                              | Sí                                  | Sí                                  |
| Un centro de datos completo (de zona o no de zona) deja de estar disponible                                           | No                              | Sí                              | Sí                                  | Sí                                  |
| Se produce un apagón en toda la región                                                                                     | No                              | No                               | Sí                                  | Sí                                  |
| Acceso de lectura a los datos en la región secundaria si la región primaria deja de estar disponible | No                              | No                               | Sí (con RA-GRS)                                   | Sí (con RA-GZRS)                                 |
| Porcentaje de durabilidad de los objetos a lo largo de un año determinado<sup>1</sup>                                          | Como mínimo 99.999999999 % (once nueves) | Como mínimo 99.9999999999 % (doce nueves) | Como mínimo 99.99999999999999 % (dieciséis nueves) | Como mínimo 99.99999999999999 % (dieciséis nueves) |
| Tipos de cuenta de almacenamiento admitidos<sup>2</sup>                                                                   | GPv2, GPv1, BlockBlobStorage, BlobStorage, FileStorage                | GPv2, BlockBlobStorage, FileStorage                             | GPv2, GPv1, BlobStorage                     | GPv2                     |
| SLA de disponibilidad para las solicitudes de lectura<sup>1</sup>  | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) para GRS<br /><br />Al menos un 99,9 % (99,99 % para el nivel de acceso esporádico) para RA-GRS | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) para GZRS<br /><br />Al menos un 99,9 % (99,99 % para el nivel de acceso esporádico) para RA-GZRS |
| SLA de disponibilidad para las solicitudes de escritura<sup>1</sup>  | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) | Al menos un 99,9 % (99 % para el nivel de acceso esporádico) |

<sup>1</sup> Para obtener información sobre las garantías de durabilidad y disponibilidad de Azure Storage, vea el [Acuerdo de Nivel de Servicio de Azure](https://azure.microsoft.com/support/legal/sla/storage/).

<sup>2</sup> Para obtener información sobre los tipos de cuentas de almacenamiento, consulte [Introducción a las cuentas de almacenamiento](storage-account-overview.md).

Todos los datos de todos los tipos de cuentas de almacenamiento y [todos los niveles (incluido el archivo)](../blobs/storage-blob-storage-tiers.md) se copian según la opción de redundancia de la cuenta de almacenamiento. Se copian los objetos, incluidos los blobs en bloques, blobs en anexos, blobs en páginas, colas, tablas y archivos.

Para obtener información sobre los precios de las diferentes opciones de redundancia, consulte los [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

> [!NOTE]
> Azure Premium Disk Storage solo admite actualmente almacenamiento con redundancia local (LRS). Las cuentas de almacenamiento de blobs en bloques admiten el almacenamiento con redundancia local (LRS) y el almacenamiento con redundancia de zona (ZRS) en determinadas regiones.

## <a name="data-integrity"></a>Integridad de datos

Azure Storage comprueba con regularidad la integridad de los datos almacenados mediante pruebas de redundancia cíclica (CRC). Si se detectan datos dañados, se reparan con los datos redundantes. Azure Storage también calcula las sumas de comprobación de todo el tráfico de red para detectar daños en los paquetes de datos al almacenar o recuperar datos.

## <a name="see-also"></a>Consulte también

- [Comprobación de la propiedad Hora de la última sincronización de una cuenta de almacenamiento](last-sync-time-get.md)
- [Cambio de la opción de redundancia para una cuenta de almacenamiento](redundancy-migration.md)
- [Diseño de aplicaciones de alta disponibilidad mediante RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
- [Recuperación ante desastres y conmutación por error de la cuenta (versión preliminar)](storage-disaster-recovery-guidance.md)
