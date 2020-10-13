---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 09/30/2019
ms.author: alkohli
ms.openlocfilehash: ca7b83d24f2416b224963559361faf5a7775cd0d
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91631550"
---
Cuando Microsoft recibe y examina el dispositivo, el estado del pedido se actualiza a **Received** (Recibido). El dispositivo, a continuación, se somete a una verificación física de daños o signos de manipulación.

Una vez completada la comprobación, el dispositivo Data Box se conecta a la red del centro de datos de Azure. La copia de datos se inicia automáticamente. Dependiendo del tamaño de los datos, la operación de copia puede tardar horas o días en completarse. Puede supervisar el progreso del trabajo de copia en el portal.

Una vez finalizada la copia, el estado del pedido se actualiza a **Completed** (Completado).

Compruebe que los datos se han cargado en Azure antes de eliminarlos del origen. Los datos pueden estar en:

- Sus cuentas de Azure Storage. Al copiar los datos en Data Box, estos se cargan en una de las siguientes rutas de acceso de la cuenta de Azure Storage:

  - Para los blobs en bloques y los blobs en páginas: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Para Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Como alternativa, puede ir a su cuenta de almacenamiento de Azure en Azure Portal e ir desde allí.

- Sus grupos de recursos de disco administrados. Al crear discos administrados, los discos duros virtuales se cargan como blobs en páginas y se convierten en discos administrados. Los discos administrados se conectan a los grupos de recursos especificados en el momento de creación del pedido. 

    - Si la copia en los discos administrados de Azure se realizó correctamente, puede ir a **Detalles del pedido** en Azure Portal y tomar nota de los grupos de recursos especificados para los discos administrados.

        ![Identificación de grupos de recursos de disco administrados](media/data-box-verify-upload-return/order-details-managed-disk-resource-groups.png)

        Vaya al grupo de recursos anotado y busque los discos administrados.

        ![Disco administrado conectado a grupos de recursos](media/data-box-verify-upload-return/managed-disks-resource-group.png)

    - Si copió un VHDX o un VHD dinámico o de diferenciación, el VHD o VHDX se cargan en la cuenta de almacenamiento provisional como si fueran un blob en páginas, pero se producen un error en la conversión del disco duro virtual a disco administrado. Vaya a su almacenamiento provisional **Cuenta de almacenamiento > Blobs** y seleccione el contenedor adecuado (SSD estándar, HDD estándar o SSD Premium). Los discos duros virtuales se cargan como blobs en páginas en la cuenta de almacenamiento provisional y acumulan cargos.


## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Prerequisites
> * Preparación para el envío
> * Envío de Data Box a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de datos de Data Box

Pase al siguiente artículo para obtener información sobre cómo administrar Data Box a través de la interfaz de usuario web local.

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar Azure Data Box](../articles/databox/data-box-local-web-ui-admin.md)