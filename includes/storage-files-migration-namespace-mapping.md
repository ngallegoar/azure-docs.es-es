---
title: Asignación de una estructura de carpetas a una topología de Azure File Sync
description: Asigne una estructura de archivos y carpetas existente a recursos compartidos de archivos de Azure para su uso con Azure File Sync. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 25c333aae49bff8d0596d4f5403c18576bf198b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124725"
---
En este paso, está evaluando cuántos recursos compartidos de archivos de Azure necesita. Una sola instancia de Windows Server (o clúster) puede sincronizar hasta 30 recursos compartidos de archivos de Azure.

Es posible que tenga más carpetas en los volúmenes que actualmente comparte localmente como recursos compartidos de SMB en sus usuarios y aplicaciones. Lo más sencillo sería prever un recurso compartido local para una asignación 1:1 a un recurso compartido de archivos de Azure. Si tiene un número suficientemente pequeño, por debajo de 30 para un solo servidor Windows Server, se recomienda una asignación 1:1.

Si tiene más de 30 recursos compartidos, a menudo no es necesario asignar un recurso compartido local 1:1 a un recurso compartido de archivos de Azure.
Considere las opciones siguientes:

#### <a name="share-grouping"></a>Agrupación de recursos compartidos

Por ejemplo, si el Departamento de RR. HH. tiene un total de 15 recursos compartidos, podría considerar la posibilidad de almacenar todos los datos de recursos humanos en un solo recurso compartido de archivos de Azure. Almacenar varios recursos compartidos locales en un recurso compartido de archivos de Azure no le impide crear los 15 recursos compartidos de SMB habituales en la instancia local de Windows Server. Solo significa que organiza las carpetas raíz de estos 15 recursos compartidos como subcarpetas en una carpeta común. A continuación, sincronizará esta carpeta común con un recurso compartido de archivos de Azure. De este modo, solo se necesita un único recurso compartido de archivos de Azure en la nube para este grupo de recursos compartidos locales.

#### <a name="volume-sync"></a>Sincronización de volúmenes

Azure File Sync admite la sincronización de la raíz de un volumen con un recurso compartido de archivos de Azure.
Si sincroniza la carpeta raíz, todas las subcarpetas y archivos terminarán en el mismo recurso compartido de archivos de Azure.

La sincronización de la raíz del volumen no siempre será la mejor respuesta. La sincronización de varias ubicaciones presenta ventajas: ayuda a mantener un menor número de elementos por ámbito de sincronización. La configuración de Azure File Sync con un número de elementos menor no solo es beneficiosa para la sincronización de archivos. Un número de elementos menor también beneficia a otros escenarios como:

* restauración en la nube a partir de una instantánea de recursos compartidos de archivos de Azure tomada como copia de seguridad
* la recuperación ante desastres de un servidor local puede acelerarse significativamente
* los cambios realizados directamente en un recurso compartido de archivos de Azure (sin sincronización) se pueden detectar y sincronizar más rápido

#### <a name="a-structured-approach-to-a-deployment-map"></a>Un enfoque estructurado de una asignación de implementación

Antes de implementar el almacenamiento en la nube en un paso posterior, es importante crear una asignación entre carpetas locales y recursos compartidos de archivos de Azure. Esta asignación informará de cuántos recursos del "grupo de sincronización" de Azure File Sync aprovisionará y cuáles serán estos. Un grupo de sincronización está relacionado con el recurso compartido de archivos de Azure y la carpeta de su servidor, y establece una conexión de sincronización.

Para tomar la decisión sobre cuántos recursos compartidos de archivos de Azure necesita, revise los límites y procedimientos recomendados siguientes. Esto le ayudará a optimizar la asignación:

* Un servidor con el agente de Azure File Sync instalado puede sincronizarse con hasta 30 recursos compartidos de archivos de Azure.
* Un recurso compartido de archivos de Azure se implementa dentro de una cuenta de almacenamiento. Esto hace que la cuenta de almacenamiento sea un destino de escalado para los números de rendimiento como IOPS y rendimiento. Dos recursos compartidos de archivos de Azure estándar (no Premium) podrían saturar teóricamente el rendimiento máximo que puede ofrecer una cuenta de almacenamiento. Si solo tiene previsto asociar Azure File Sync a estos recursos compartidos de archivos, la agrupación de varios recursos compartidos de archivos en la misma cuenta de almacenamiento no creará ningún problema. Revise los destinos de rendimiento para obtener información más detallada sobre las métricas pertinentes que se deben tener en cuenta. Si tiene previsto mover una aplicación a Azure que usará el recurso compartido de archivos de Azure de forma nativa, es posible que necesite un mayor rendimiento del recurso compartido de archivos de Azure. Si esto es una posibilidad, incluso en el futuro, lo mejor es asignar un recurso compartido de archivos de Azure a su propia cuenta de almacenamiento.
* Hay un límite de 250 cuentas de almacenamiento por suscripción en una sola región de Azure.

> [!TIP]
> Teniendo en cuenta esta información, suele ser necesario agrupar varias carpetas de nivel superior de sus volúmenes en un directorio raíz común nuevo. A continuación, sincronizará este nuevo directorio raíz y todas las carpetas que agrupó en él con un solo recurso compartido de archivos de Azure.                                                    

Esta técnica le permite permanecer en el límite de sincronización de 30 recursos compartidos de archivos de Azure por servidor.
Esta agrupación en una raíz común no tiene ningún impacto en el acceso a sus datos. Sus ACL permanecen tal cual, solo tendría que ajustar algunas rutas de acceso del recurso compartido (como los recursos compartidos SMB o NFS) que podría tener en las carpetas de servidor que ha cambiado ahora en una raíz común. No cambia nada más.

Otro aspecto importante de Azure File Sync y una experiencia y un rendimiento equilibrados es una comprensión de los factores de escala para el rendimiento de Azure File Sync. Obviamente, si los archivos se sincronizan a través de Internet, los archivos más grandes y el ancho de banda tardarán más tiempo en sincronizarse.

> [!IMPORTANT]
> El vector de escala más importante para Azure File Sync es el número de elementos (archivos y carpetas) que deben sincronizarse.

Azure File Sync admite la sincronización de hasta 100 000 elementos con un recurso compartido de archivos de Azure. Este límite se puede superar y solo muestra lo que prueba el equipo de Azure File Sync de forma regular.

Es un procedimiento recomendado mantener bajo el número de elementos por ámbito de sincronización. Ese aspecto es un factor importante que se debe tener en cuenta en la asignación de carpetas a recursos compartidos de archivos de Azure.

Incluso si en su situación, un conjunto de archivos se puede sincronizar de forma lógica con el mismo recurso compartido de archivos de Azure (mediante el enfoque de la carpeta raíz común nueva mencionado anteriormente), podría seguir siendo mejor reagrupar las carpetas de modo que se sincronicen con dos recursos compartidos de archivos de Azure en lugar de con uno. Este enfoque se puede usar para mantener el número de archivos y carpetas por recurso compartido de carpetas equilibrado en el servidor.

#### <a name="create-a-mapping-table"></a>Creación de una tabla de asignación

:::row:::
    :::column:::
        [![](media/storage-files-migration-namespace-mapping/namespace-mapping.png "An example of a mapping table. Download the file below to experience and use the content of this image.")](media/storage-files-migration-namespace-mapping/namespace-mapping-expanded.png#lightbox)
    :::column-end:::
    :::column:::
        Use una combinación de los conceptos anteriores para ayudar a determinar cuántos recursos compartidos de archivos de Azure necesita, y qué partes de los datos existentes terminarán en cuál recurso compartido de archivos de Azure.
        
        Cree una tabla donde registre sus pensamientos, de modo que pueda consultarla en el paso siguiente. Mantenerse organizado es importante, ya que puede ser fácil perder detalles del plan de asignación al aprovisionar muchos recursos de Azure a la vez. Para ayudarle a crear una asignación completa, puede descargar un archivo de Microsoft Excel como plantilla.

[//]: # (HTML aparece como la única forma de realizar la adición de una tabla anidada de dos columnas con análisis de imágenes de trabajo y texto/hipervínculo en la misma línea.)

<br>
<table>
    <tr>
        <td>
            <img src="media/storage-files-migration-namespace-mapping/excel.png" alt="Microsoft Excel file icon that helps to set the context for the type of file download for the link next to it.">
        </td>
        <td>
            <a href="https://download.microsoft.com/download/1/8/D/18DC8184-E7E2-45EF-823F-F8A36B9FF240/Azure File Sync - Namespace Mapping.xlsx">Descargue una plantilla de asignación de espacios de nombres.</a>
        </td>
    </tr>
</table>
    :::column-end:::
:::row-end:::
