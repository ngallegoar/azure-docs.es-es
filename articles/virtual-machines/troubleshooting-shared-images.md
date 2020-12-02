---
title: Solución de problemas de imágenes compartidas en Azure
description: Obtenga información sobre cómo solucionar problemas relacionados con las galerías de imágenes compartidas.
author: olayemio
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: troubleshooting
ms.workload: infrastructure
ms.date: 10/27/2020
ms.author: olayemio
ms.reviewer: cynthn
ms.openlocfilehash: d15f58a2d87483e8e2fd6711481bfe8866ca1548
ms.sourcegitcommit: 03c0a713f602e671b278f5a6101c54c75d87658d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94920617"
---
# <a name="troubleshoot-shared-image-galleries-in-azure"></a>Solución de problemas de las galerías de imágenes compartidas de Azure

Si tiene problemas al realizar cualquier operación en galerías de imágenes compartidas, definiciones de imágenes y versiones de imágenes, vuelva a ejecutar el comando con errores en modo de depuración. El modo de depuración se activa pasando el modificador `--debug` con la CLI de Azure y el modificador `-Debug` con PowerShell. Después de encontrar el error, siga este artículo para solucionarlo.


## <a name="creating-or-modifying-a-gallery"></a>Creación o modificación de una galería ##

*El nombre de la galería no es válido. Los caracteres permitidos son caracteres alfanuméricos en inglés, con guiones bajos y puntos en el medio, hasta 80 caracteres en total. No se permiten todos los demás caracteres especiales, incluidos los guiones.*  
**Causa**: El nombre de la galería no cumple los requisitos de nomenclatura.  
**Solución alternativa**: Elija un nombre que cumpla las condiciones siguientes: 
- Tiene un límite de 80 caracteres.
- Contiene solo letras del alfabeto inglés, números, caracteres de subrayado y puntos.
- Comienza y termina con letras o números en inglés.

*El nombre de entidad "galleryName" no es válido según su regla de validación: ^[^\_\W][\w-.\_]{0,79}(?<![-.])$.*  
**Causa**: El nombre de la galería no cumple los requisitos de nomenclatura.  
**Solución alternativa**: Elija un nombre para la galería que cumpla las condiciones siguientes: 
- Tiene un límite de 80 caracteres.
- Contiene solo letras del alfabeto inglés, números, caracteres de subrayado y puntos.
- Comienza y termina con letras o números en inglés.

*El nombre de recurso proporcionado <galleryName\> tiene estos caracteres finales no válidos: <character\>. El nombre no puede acabar con los caracteres: <character\>* .  
**Causa**: El nombre de la galería finaliza con un punto o un guion bajo.  
**Solución alternativa**: Elija un nombre para la galería que cumpla las condiciones siguientes: 
- Tiene un límite de 80 caracteres.
- Contiene solo letras del alfabeto inglés, números, caracteres de subrayado y puntos.
- Comienza y termina con letras o números en inglés.

*La ubicación proporcionada <region\> no está disponible para el tipo de recurso "Microsoft.Compute/galeries". La lista de regiones disponibles para el tipo de recurso es...*  
**Causa**: La región especificada para la galería es incorrecta o requiere una solicitud de acceso.  
**Solución alternativa**: Compruebe que el nombre de la región esté escrito correctamente. Puede ejecutar este comando para ver a qué regiones tiene acceso. Si la región no aparece en la lista, envíe [una solicitud de acceso](/troubleshoot/azure/general/region-access-request-process).

*No se puede eliminar el recurso si no se han eliminado primero los recursos anidados.*  
**Causa**: Ha intentado eliminar una galería que contiene al menos una definición de imagen existente. Para poder eliminar una galería, debe estar vacía.  
**Solución alternativa**: Elimine todas las definiciones de imagen que contiene la galería y, luego, proceda a la eliminación de la galería. Si la definición de imagen contiene versiones de imagen, debe eliminar las versiones de imagen antes de eliminar las definiciones.

*El recurso <galleryName\> ya existe en la ubicación < region\_1\> en el grupo de recursos <resourceGroup\>. No se puede crear un recurso con el mismo nombre en la ubicación <region\_2\>. Seleccione un nuevo nombre de recurso.*  
**Causa**: Tiene una galería en el grupo de recursos con el mismo nombre y ha intentado crear otra con el mismo nombre, pero en otra región.  
**Solución alternativa**: Use otra galería y otro grupo de recursos.

## <a name="creating-or-modifying-image-definitions"></a>Creación o modificación de definiciones de imagen ##

*No se permite cambiar la propiedad "galleryImage.properties.<property\>".*  
**Causa**: Intentó cambiar el tipo de sistema operativo, el estado del sistema operativo, la generación de Hyper-V, la oferta, el editor y la SKU. No se permite cambiar ninguna de estas propiedades.  
**Solución alternativa**: Mejor, cree una definición de imagen.

*El recurso <galleryName/imageDefinitionName\> ya existe en la ubicación <region\_1\> del grupo de recursos <resourceGroup\>. No se puede crear un recurso con el mismo nombre en la ubicación <region\_2\>. Seleccione un nuevo nombre de recurso.*  
**Causa**: Tiene una definición de imagen existente en la misma galería y grupo de recursos con el mismo nombre. Ha intentado crear otra definición de imagen con el mismo nombre y en la misma galería, pero en otra región.  
**Solución alternativa**: Use otro nombre para la definición de imagen o coloque esta en otra galería o grupo de recursos

*El nombre de recurso proporcionado <galleryName\>/<imageDefinitionName\> tiene estos caracteres finales no válidos: <character\>. El nombre no puede acabar con los caracteres: <character\>* .  
**Causa**: El nombre <imageDefinitionName\> termina con un punto o un guion bajo.  
**Solución alternativa**: Elija un nombre para la definición de imagen que cumpla las condiciones siguientes: 
- Tiene un límite de 80 caracteres.
- Contiene solo letras del alfabeto inglés, números, caracteres de subrayado, guiones y puntos.
- Comienza y termina con letras o números en inglés.

*El nombre de entidad <imageDefinitionName\> no es válido de acuerdo con su regla de validación: ^[^\_\\W][\\w-.\_]{0,79}(?<![-.])$"* .  
**Causa**: El nombre <imageDefinitionName\> termina con un punto o un guion bajo.  
**Solución alternativa**: Elija un nombre para la definición de imagen que cumpla las condiciones siguientes: 
- Tiene un límite de 80 caracteres.
- Contiene solo letras del alfabeto inglés, números, caracteres de subrayado, guiones y puntos.
- Comienza y termina con letras o números en inglés.

*El nombre del recurso galleryImage.properties.identifier.<property\> no es válido. No puede estar vacío. Los caracteres permitidos son letras mayúsculas o minúsculas, números, guiones (-), puntos (.), guiones bajos (\_). No se permite que los nombres finalicen con un punto (.). La longitud del nombre no puede superar <number\> caracteres.* .  
**Causa**: El valor del editor, la oferta o el SKU no cumplen los requisitos de nomenclatura.  
**Solución alternativa**: Elija un valor que cumpla las condiciones siguientes: 
- Tiene un límite de 128 caracteres para el límite de editor o 64 caracteres para la oferta y la SKU
- Contiene solo letras del alfabeto inglés, números, guiones, caracteres de subrayado y puntos.
- No termina con un punto.

*No se puede realizar la operación solicitada en el recurso anidado. No se encontró el recurso principal <galleryName\> .* .  
**Causa**: No hay ninguna galería con el nombre <galleryName\> en la suscripción y el grupo de recursos actuales.  
**Solución alternativa**: Compruebe que el nombre de la galería, la suscripción y el grupo de recursos sean correctos. De lo contrario, cree una galería llamada <galleryName\>.

*La ubicación proporcionada <region\> no está disponible para el tipo de recurso "Microsoft.Compute/galeries". La lista de regiones disponibles para el tipo de recurso es...*  
**Causa**: El nombre de <region\> es incorrecto o se requiere una solicitud de acceso.  
**Solución alternativa**: Compruebe que el nombre de la región esté escrito correctamente. Puede ejecutar este comando para ver a qué regiones tiene acceso. Si la región no aparece en la lista, envíe [una solicitud de acceso](/troubleshoot/azure/general/region-access-request-process).

*No se puede serializar el valor: <value\> como tipo: "ISO-8601", ISO8601Error: Falta el designador de tiempo ISO 8601 "T". No se puede analizar la cadena de fecha y hora <value\>* .  
**Causa**: El valor proporcionado a la propiedad no tiene el formato correcto de fecha.  
**Solución alternativa**: Proporcione una fecha en el formato válido: aaaa-MM-dd, aaaa-MM-dd'T'HH:mm:sszzz o [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

*No se puede convertir la cadena en DateTimeOffset: <value\>. Ruta de acceso "properties.<property\>"* .  
**Causa**: El valor proporcionado a la propiedad no tiene el formato correcto de fecha.  
**Solución alternativa**: Proporcione una fecha en el formato válido: aaaa-MM-dd, aaaa-MM-dd'T'HH:mm:sszzz o [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

*EndOfLifeDate debe establecerse en una fecha futura.*  
**Causa**: La propiedad de fecha de final de ciclo de vida no tiene el formato correcto como fecha posterior a la fecha de hoy.  
**Solución alternativa**: Proporcione una fecha en el formato válido: aaaa-MM-dd, aaaa-MM-dd'T'HH:mm:sszzz o [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

*argumento --<property\>: valor int no válido: <value\>*  
**Causa**: <property\> solo acepta valores enteros y <value\> no lo es.  
**Solución alternativa**: Elija un valor entero.

*El valor mínimo de <property\> debe ser mayor que el valor máximo de <property\>.*  
**Causa**: El valor mínimo proporcionado para <property\> es mayor que el valor máximo proporcionado para <property\>.  
**Solución alternativa**: Cambie los valores para que el mínimo sea menor o igual que el máximo.

*La imagen de la galería: <imageDefinitionName\> identificada por (editor: <Publisher\>, oferta: <Offer\> y sku: <SKU\>) ya existe. Elija otra combinación de editor, oferta y sku.*  
**Causa**: Ha intentado crear una definición de imagen con el mismo trío de editor, oferta y SKU que el de una definición de imagen existente en la misma galería.  
**Solución alternativa**: En una galería determinada, todas las definiciones de imagen deben tener una combinación única de editor, oferta y SKU. Elija una combinación única o una nueva galería y vuelva a crear la definición de imagen.

*No se puede eliminar el recurso si no se han eliminado primero los recursos anidados.*  
**Causa**: Ha intentado eliminar una definición de imagen que contiene versiones de imagen. Una definición de imagen debe estar vacía antes de poder eliminarla.  
**Solución alternativa**: Elimine todas las versiones de imagen que contiene la definición de imagen y, luego, proceda a eliminarla.

*No se puede enlazar el parámetro <property\>. No se puede convertir el valor <value\> al tipo <propertyType\>. No se puede hacer coincidir el nombre del identificador <value\> con un nombre de enumerador válido. Especifique uno de los siguientes nombres de enumerador e inténtelo de nuevo: <choice1\>, <choice2\>, …* .  
**Causa**: La propiedad tiene una lista restringida de valores posibles y <value\> no es uno de ellos.  
**Solución alternativa**: Elija uno de los valores posibles <choice\>.

*No se puede enlazar el parámetro <property\>. No se puede convertir el valor <value\> en el tipo &quot;System.DateTime&quot;* .  
**Causa**: El valor proporcionado a la propiedad no tiene el formato correcto de fecha.  
**Solución alternativa**: Proporcione una fecha en el formato válido: aaaa-MM-dd, aaaa-MM-dd'T'HH:mm:sszzz o [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

*No se puede enlazar el parámetro <property\>. No se puede convertir el valor <value\> en el tipo &quot;System.Int32&quot;* .  
**Causa**: <property\> solo acepta valores enteros y <value\> no lo es.  
**Solución alternativa**: Elija un valor entero.

*No se admite el tipo de cuenta de almacenamiento ZRS en esta región.*  
**Causa**: Ha elegido el almacenamiento con redundancia de zona estándar (ZRS) en una región que todavía no se admite.  
**Solución alternativa**: Cambie el tipo de cuenta de almacenamiento a **Premium\_LRS** o **Estándar\_LRS**. Consulte la documentación para obtener la [lista de regiones](/azure/storage/common/storage-redundancy#zone-redundant-storage) más reciente con la versión preliminar de ZRS habilitada.

## <a name="creating-or-updating-image-versions"></a>Creación o actualización de versiones de imagen ##

*La ubicación proporcionada <region\> no está disponible para el tipo de recurso "Microsoft.Compute/galeries". La lista de regiones disponibles para el tipo de recurso es...*  
**Causa**: El nombre de <region\> es incorrecto o se requiere una solicitud de acceso.  
**Solución alternativa**: Compruebe que el nombre de la región esté escrito correctamente. Puede ejecutar este comando para ver a qué regiones tiene acceso. Si la región no aparece en la lista, envíe [una solicitud de acceso](/troubleshoot/azure/general/region-access-request-process).

*No se puede realizar la operación solicitada en el recurso anidado. No se encontró el recurso principal <galleryName/imageDefinitionName\>.*  
**Causa**: No hay ninguna galería con el nombre <galleryName/imageDefinitionName\> en la suscripción y el grupo de recursos actuales.  
**Solución alternativa**: Compruebe que el nombre de la galería, la suscripción y el grupo de recursos sean correctos. Si no es así, cree una galería con el nombre <galleryName\> o la definición de imagen denominada <imageDefinitionName\> en el grupo de recursos indicado.

*No se puede enlazar el parámetro <property\>. No se puede convertir el valor <value\> en el tipo &quot;System.DateTime&quot;* .  
**Causa**: El valor proporcionado a la propiedad no tiene el formato correcto de fecha.  
**Solución alternativa**: Proporcione una fecha en el formato válido: aaaa-MM-dd, aaaa-MM-dd'T'HH:mm:sszzz o [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

*No se puede enlazar el parámetro <property\>. No se puede convertir el valor <value\> en el tipo &quot;System.Int32&quot;* .  
**Causa**: <property\> solo acepta valores enteros y <value\> no lo es.  
**Solución alternativa**: Elija un valor entero.

*El perfil de publicación de la versión de imagen de la galería <publishingRegions\> debe contener la ubicación de la versión de imagen <sourceRegion\>* .  
**Causa**: La ubicación de la imagen de origen (<sourceRegion\>) debe estar incluida en la lista <publishingRegions\>.  
**Solución alternativa**: Incluya < sourceRegion\> en la lista <publishingRegions\>.

*El valor <value\> del parámetro <property\> está fuera del intervalo. El valor debe estar entre <minValue\> y <maxValue\>, ambos inclusive.*  
**Causa**: <value\> está fuera del intervalo de valores posibles para <property\>.  
**Solución alternativa**: Elija un valor que esté dentro del intervalo de <minValue\> y <maxValue\>, ambos inclusive.

*No se encuentra el origen <resourceID\>. Compruebe que exista y que se encuentre en la misma región que la versión de imagen de la galería que se va a crear.*  
**Causa**: No hay ningún origen ubicado en <resourceID\> o el origen de <resourceID\> no está en la misma región que la imagen de la galería que se está creando.  
**Solución alternativa**: Compruebe que <resourceID\> sea correcto y que la región de origen de la versión de imagen de la galería sea la misma que la región de <resourceID\>.

*No se permite cambiar la propiedad "galleryImageVersion.properties.storageProfile.<diskImage\>.source.id".*  
**Causa**: El identificador de origen de una versión de imagen de la galería no se puede cambiar después de la creación.  
**Solución alternativa**: Asegúrese de que el identificador de origen sea el mismo que el identificador de origen ya existente, cambie el número de versión de la versión de imagen o elimine la versión de imagen y vuelva a intentarlo.

*Se han detectado números LUN duplicados en los discos de datos de entrada. El número LUN debe ser único para cada disco de datos.*  
**Causa**: Al crear una versión de imagen mediante una lista de discos o instantáneas de disco, dos o más discos o instantáneas de disco tienen el mismo LUN.  
**Solución alternativa**: Quite o cambie los LUN duplicados.

*Los identificadores de origen duplicados se encuentran en los discos de entrada. El identificador de origen debe ser único para cada disco.*  
**Causa**: Al crear una versión de imagen mediante una lista de discos o instantáneas de disco, dos o más discos o instantáneas de disco tienen el mismo identificador de recurso.  
**Solución alternativa**: Quite o cambie los identificadores de origen de disco duplicados.

*El identificador de propiedad <resourceID\> de la ruta de acceso "properties.storageProfile.<diskImages\>.source.id" no es válido. Espere un identificador de recurso completo que empiece por "/subscriptions/{subscriptionId}" o "/providers/{resourceProviderNamespace}/".*  
**Causa**: El valor <resourceID\> no es correcto.  
**Solución alternativa**: Compruebe que el identificador de recurso sea correcto.

*El identificador de origen <resourceID\> debe ser una imagen administrada, una máquina virtual u otra versión de imagen de la galería*.  
**Causa**: El valor <resourceID\> no es correcto.  
**Solución alternativa**: Si usa una máquina virtual, una imagen administrada o una versión de imagen de la galería como imagen de origen, compruebe que sus identificadores de recursos sean correctos.

*El identificador de origen <resourceID\> debe ser un disco administrado o una instantánea.*  
**Causa**: El valor <resourceID\> no es correcto.  
**Solución alternativa**: Si usa discos o instantáneas de disco como orígenes para la versión de la imagen, compruebe que sus identificadores de recursos sean correctos.

*No se puede crear una versión de imagen de la galería a partir de <resourceID\>, ya que el estado del sistema operativo de la imagen principal de la galería (<OsState\_1\>) no es <OsState\_2\>.*  
**Causa**: El estado del sistema operativo (generalizado o especializado) no coincide con el estado del sistema operativo especificado en la definición de imagen.  
**Solución alternativa**: Elija un origen basado en una máquina virtual con el estado del sistema operativo de <OsState\_1\> o cree una definición de imagen para las máquinas virtuales basadas en <OsState\_2\>.

*El recurso con el identificador "<resourceID\>·tiene una generación de Hipervisor distinta ["<V#\_1\>"] a la de la imagen principal de la galería ["<V#\_2\>"]* .  
**Causa**: La generación del hipervisor de la versión de imagen no coincide con la especificada en la definición de imagen. El sistema operativo de la definición de imagen es <V#\_1\> y el de la versión de imagen es <V#\_2\>.  
**Solución alternativa**: Elija un origen con la misma generación de hipervisor que la definición de imagen o cree o elija una nueva definición de imagen que tenga la misma generación de hipervisor que la versión de imagen.

*El recurso con el identificador "<resourceID\>" tiene un tipo de sistema operativo diferente ["<OsType\_1\>"] al de la generación del tipo de sistema operativo de la imagen principal de la galería ["<OsType \_2\>"]* .  
**Causa**: La generación del hipervisor de la versión de imagen no coincide con la especificada en la definición de imagen. El sistema operativo de la definición de imagen es <OsType\_1\> y el de la versión de imagen es <OsType\_2\>.  
**Solución alternativa**: Elija un origen con el mismo sistema operativo (Linux o Windows) que la definición de imagen o cree o elija una nueva definición de imagen que tenga la misma generación de sistema operativo que la versión de imagen.

*La máquina virtual de origen <resourceID\> no puede contener un disco de SO efímero.*  
**Causa**: El origen de <resourceID\> contiene un disco de SO efímero. La galería de imágenes compartida no admite actualmente discos de SO efímeros.  
**Solución alternativa**: Elija otro origen basado en una máquina virtual que no use un disco de SO efímero.

*La máquina virtual de origen <resourceID\> no puede contener discos ["<diskID\>"] almacenado en un tipo de cuenta UltraSSD.*  
**Causa**: El valor de <diskID\> del disco es un disco SSD Ultra. La galería de imágenes compartidas no admite actualmente discos SSD Ultra.  
**Solución alternativa**: Use un origen que contenga solo discos administrados SSD Premium, SSD estándar o HDD estándar.

*Se debe crear la máquina virtual de origen <resourceID\> a partir de Managed Disks.*  
**Causa**: La máquina virtual de <resourceID\> usa discos no administrados.  
**Solución alternativa**: Use un origen basado en una máquina virtual que contenga solo discos administrados SSD Premium, SSD estándar o HDD estándar.

*Demasiadas solicitudes en el origen "<resourceID\>". Reduzca el número de solicitudes en el origen o espere algún un tiempo antes de volver a intentarlo.*  
**Causa**: El origen de esta versión de imagen se está limitando actualmente debido a demasiadas solicitudes.  
**Solución alternativa**: Pruebe a crear la versión de la imagen más adelante.

*El conjunto de cifrado de disco "<diskEncryptionSetID\>" debe estar en la misma suscripción "<subscriptionID\>" que el recurso de la galería.*  
**Causa**: Los conjuntos de cifrado de disco solo se pueden usar en la misma suscripción y región en la que se crearon.  
**Solución alternativa**: Cree o use un conjunto de cifrado en la misma suscripción y región que la versión de imagen.

*El origen de cifrado "<resourceID\>" está en un identificador de suscripción diferente al de la suscripción de la versión de imagen actual de la galería "<subscriptionID\_1\>". Vuelva a intentarlo con orígenes no cifrados o use la suscripción del origen "<subcriptionID\_2\>" para crear la versión de imagen de la galería.*  
**Causa**: La galería de imágenes compartidas no admite actualmente la creación de versiones de imagen en otra suscripción desde otra imagen de origen si esta está cifrada.  
**Solución alternativa**: Use un origen no cifrado o cree la versión de imagen en la misma suscripción que el origen.

*No se encontró el conjunto de cifrado de disco <diskEncryptionSetID\>.*  
**Causa**: Es posible que el cifrado de disco no sea correcto.  
**Solución alternativa**: Compruebe que el identificador de recurso del conjunto de cifrado de disco sea correcto.

*El nombre de la versión de imagen no es válido. El nombre de la versión de imagen debe seguir el formato Principal(int).Secundaria(int).Revisión(int), por ejemplo: 1.0.0, 2018.12.1, etc.*  
**Causa**: El formato válido para una versión de imagen es de tres enteros separados por un punto. El nombre de la versión de imagen no cumplía el formato válido.  
**Solución alternativa**: Use un nombre de versión de imagen que siga el formato Principal(int).Secundaria(int).Revisión(int). Por ejemplo: 1.0.0. o 2018.12.1.

*El valor del parámetro galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.dataDiskImages.diskEncryptionSetId no es válido*.  
**Causa**: El identificador de recurso del conjunto de cifrado de disco usado en una imagen de disco de datos emplea un formato no válido.  
**Solución alternativa**: Asegúrese de que el identificador de recurso del conjunto de cifrado de disco siga el formato /subscriptions/<subscriptionID\>/resourceGroups/<resourceGroupName\>/providers/Microsoft.Compute/<diskEncryptionSetName\>.

*El valor del parámetro galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.osDiskImage.diskEncryptionSetId no es válido*.  
**Causa**: El identificador de recurso del conjunto de cifrado de disco usado en una imagen de disco del sistema operativo emplea un formato no válido.  
**Solución alternativa**: Asegúrese de que el identificador de recurso del conjunto de cifrado de disco siga el formato /subscriptions/<subscriptionID\>/resourceGroups/<resourceGroupName\>/providers/Microsoft.Compute/<diskEncryptionSetName\>.

*No se puede especificar un nuevo LUN de cifrado de imágenes de disco de datos [<number\>] con un conjunto de cifrado de disco en la región [<region\>] para la solicitud de la versión de imagen de la galería. Para actualizar esta versión, quite el nuevo LUN. Si necesita cambiar la configuración del cifrado de imágenes de discos de datos, debe crear una versión de imagen de la galería con la configuración correcta.*  
**Causa**: Se ha agregado cifrado al disco de datos de una versión de imagen existente. Esta acción no se puede hacer.  
**Solución alternativa**: Cree una versión de imagen de la galería o quite la configuración de cifrado agregada.

*El origen de la versión del artefacto de la galería solo se puede especificar directamente en storageProfile o en discos de datos o del sistema operativo individuales. Solo se puede proporcionar un tipo de origen (imagen de usuario, instantánea, disco, máquina virtual).*  
**Causa**: Falta el identificador de origen.  
**Solución alternativa**: Asegúrese de que el identificador del origen exista.

*No se encontró el origen: <resourceID\>. Compruebe que exista.*  
**Causa**: Es posible que el identificador de recurso del origen sea incorrecto.  
**Solución alternativa**: Asegúrese de que el valor del identificador de recurso del origen sea correcto.

*Se requiere un conjunto de cifrado de disco para el disco "galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.osDiskImage.diskEncryptionSetId" de la región de destino "<Region\_1\>", ya que el conjunto de cifrado de disco "<diskEncryptionSetID\>" se usa para el disco correspondiente de la región "<Region\_2\>"* .  
**Causa**: El cifrado se ha usado en el disco del sistema operativo en <Region\_2\>, pero no en <Region\_1\>.  
**Solución alternativa**: Si usa el cifrado en el disco del sistema operativo, use el cifrado en todas las regiones.

*Se requiere un conjunto de cifrado de disco para el disco "LUN <number\>" de la región de destino "<Region\_1\>", ya que el conjunto de cifrado de disco "<diskEncryptionSetID\>" se usa para el disco correspondiente de la región "<Region\_2\>"* .  
**Causa**: El cifrado se ha usado en el disco de datos en LUN <number\> en <Region\_2\>, pero no en <Region\_1\>.  
**Solución alternativa**: Si usa el cifrado en un disco de datos, use el cifrado en todas las regiones.

*Se especificó un LUN no válido [<number\>] en encryption.dataDiskImages. El LUN debe tener uno de los siguientes valores ["0,9"].*  
**Causa**: El LUN especificado para el cifrado no coincide con ninguno de los números de LUN de los discos asociados a la máquina virtual.  
**Solución alternativa**: Cambie el LUN del cifrado por el LUN de un disco de datos presente en la máquina virtual.

*Se especificaron LUN duplicados "<number\>" en la región de destino "<region\>" encryption.dataDiskImages.*  
**Causa**: La configuración de cifrado usada en <region\> ha especificado un LUN al menos dos veces.  
**Solución alternativa**: Cambie el LUN de <region\> para asegurarse de que todos los LUN sean únicos en <region\>.

*OSDiskImage y DataDiskImage no pueden apuntar al mismo blob < sourceID\>* .  
**Causa**: Los orígenes del disco del sistema operativo y al menos un disco de datos no son únicos.  
**Solución alternativa**: Cambie el origen del disco del sistema operativo o de los discos de datos para que el disco del sistema operativo y el disco de datos sean únicos.

*No se permiten regiones duplicadas en las regiones de publicación de destino.*  
**Causa**: Una región se muestra entre las regiones de publicación más de una vez.  
**Solución alternativa**: Quite la región duplicada.

*No se permite agregar nuevos discos de datos ni cambiar el LUN de un disco de datos en una imagen existente.*  
**Causa**: Una llamada de actualización a la versión de imagen contiene un nuevo disco de datos o tiene un nuevo LUN para un disco.  
**Solución alternativa**: Use los LUN y los discos de datos de la versión de imagen existente.

*El conjunto de cifrado de disco "<diskEncryptionSetID\>" debe estar en la misma suscripción "<subscriptionID\>" que el recurso de la galería.*  
**Causa**: La galería de imágenes admitidas no admite actualmente el uso de un conjunto de cifrado de disco en otra suscripción.  
**Solución alternativa**: Cree la versión de imagen y conjunto de cifrado de disco en la misma suscripción.

## <a name="creating-or-updating-a-vm-or-scale-sets-from-an-image-version"></a>Creación o actualización de una máquina virtual o conjuntos de escalado a partir de la versión de imagen ##

*El cliente tiene permiso para realizar la acción "Microsoft.Compute/galleries/images/versions/read" en el ámbito <resourceID\>, pero el inquilino actual <tenantId1\> no está autorizado para acceder a la suscripción vinculada <subscriptionId2\>.*  
**Causa**: La máquina virtual o el conjunto de escalado se crearon mediante una imagen SIG en otro inquilino. Ha intentado realizar un cambio en la máquina virtual o el conjunto de escalado, pero no tiene acceso a la suscripción que posee la imagen.  
**Solución alternativa**: Póngase en contacto con el propietario de la suscripción de la versión de imagen para conceder a esta acceso de lectura.

*La imagen de la galería <resourceID\> no está disponible en la región <region\>. Póngase en contacto con el propietario de la imagen para replicarla en esta región o cambie la región solicitada.*  
**Causa**: La máquina virtual se está creando en una región que no está entre la lista de regiones publicadas para la imagen de la galería.  
**Solución alternativa**: Replique la imagen en la región o cree una máquina virtual en una de las regiones de las regiones de publicación de la imagen de la galería.

*No se permite el parámetro "osProfile".*  
**Causa**: Se proporcionó el nombre de usuario, la contraseña o las claves SSH de administrador para una máquina virtual que se creó a partir de una versión de imagen especializada.  
**Solución alternativa**: No incluya el nombre de usuario, la contraseña o las claves SSH de administrador si tiene previsto crear una máquina virtual a partir de esa imagen. Si no es así, use una versión de imagen generalizada y especifique el nombre de usuario, la contraseña o las claves SSH de administrador.

*Falta el parámetro requerido "osProfile" (null).*  
**Causa**: La máquina virtual se crea a partir de una imagen generalizada y falta el nombre de usuario, la contraseña o las claves SSH de administrador. Como las imágenes generalizadas no conservan el nombre de usuario, la contraseña o las claves SSH de administrador, estos campos se deben especificar durante la creación de una máquina virtual o un conjunto de escalado.  
**Solución alternativa**: Especifique el nombre de usuario, la contraseña o las claves SSH de administrador o use una versión de imagen especializada.

*No se puede crear la versión de imagen de la galería desde <resourceID\>, ya que el estado del sistema operativo de la imagen principal de la galería ("Specialized") no es "Generalized".*  
**Causa**: La versión de imagen se crea a partir de un origen generalizado, pero su definición principal es especializada.  
**Solución alternativa**: Cree la versión de imagen mediante un origen especializado o use una definición principal que esté generalizada.

*No se puede actualizar el conjunto de escalado de máquinas virtuales <vmssName\>, ya que el estado actual del sistema operativo del conjunto de escalado de máquinas virtuales es generalizado, que es diferente del estado del sistema operativo de la imagen actualizada de la galería que es especializado.*  
**Causa**: La imagen de origen actual del conjunto de escalado es una imagen de origen generalizada, pero se está actualizando con una imagen de origen que es especializada. La imagen de origen actual y la nueva imagen de origen de un conjunto de escalado deben tener el mismo estado.  
**Solución alternativa**: Para actualizar el conjunto de escalado, use una versión de imagen generalizada.

*El conjunto de cifrado de disco <diskEncryptionSetId\> de la galería de imágenes compartidas <versionId\> pertenece a la suscripción <subscriptionId1\> y no se puede usar con el recurso de la suscripción <subscriptionId2\>* .  
**Causa**: El conjunto de cifrado de disco que se usa para cifrar la versión de imagen reside en una suscripción diferente de aquella que la hospeda.  
**Solución alternativa**: Use la misma suscripción para la versión de imagen y el conjunto de cifrado de disco.

*La creación de la VM o el conjunto de escalado de máquinas virtuales tarda mucho tiempo.*  
**Solución alternativa**: Compruebe que el valor de **OSType** de la versión de la imagen a partir de la cual intenta crear la máquina virtual o el conjunto de escalado de máquinas virtuales tiene el mismo valor de **OSType** que el origen que usó para crear la versión de la imagen. 

## <a name="creating-a-disk-from-an-image-version"></a>Creación de un disco a partir de una versión de imagen ##

*El valor del parámetro imageReference no es válido.*  
**Causa**: Ha intentado exportar de una versión de imagen SIG a un disco, pero ha usado una posición de LUN que no existe en la imagen.    
**Solución alternativa**: Compruebe la versión de imagen para ver qué posiciones de LUN están en uso.

## <a name="sharing-resources"></a>Uso compartido de recursos

El uso compartido de los recursos de la galería de imágenes, versiones de imágenes y definiciones de imágenes entre suscripciones se habilita mediante el [control de acceso basado en rol (RBAC de Azure)](../role-based-access-control/rbac-and-directory-admin-roles.md). 

## <a name="replication-speed"></a>Velocidad de replicación

Use la marca **--expand ReplicationStatus** para comprobar si se ha completado la replicación en todas las regiones de destino especificadas. Si no es así, espere hasta seis horas para que se complete el trabajo. Si se produce un error, desencadene el comando de nuevo para crear y replicar la versión de la imagen. Si va a replicar la versión de la imagen en muchas regiones de destino, considere la posibilidad de realizar la replicación en fases.

## <a name="azure-limits-and-quotas"></a>Límites y cuotas de Azure 

Los[límites y cuotas de Azure](../azure-resource-manager/management/azure-subscription-service-limits.md) se aplican a todos los recursos de galería de imágenes compartidas, las versiones de imágenes, definiciones de imágenes. Asegúrese de encontrarse dentro de los límites para las suscripciones. 


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [Shared Image Galleries](./linux/shared-image-galleries.md).