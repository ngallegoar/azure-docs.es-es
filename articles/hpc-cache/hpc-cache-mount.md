---
title: Montaje de una instancia de Azure HPC Cache
description: Conexión de clientes a un servicio de Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: rohogue
ms.openlocfilehash: f176e30cfaf9a52e4f58091b7fc76098a4c88a48
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657349"
---
# <a name="mount-the-azure-hpc-cache"></a>Montaje de la instancia de Azure HPC Cache

Después de crear la memoria caché, los clientes NFS pueden acceder a ella con un sencillo comando `mount`. El comando conecta una ruta de acceso de destino de almacenamiento específica de Azure HPC Cache a un directorio local en el equipo cliente.

El comando mount se compone de estos elementos:

* Una de las direcciones de montaje de la caché (que aparece en la página información general de la caché)
* La ruta de acceso del espacio de nombres virtual que estableció al crear el destino de almacenamiento
* Ruta de acceso local que se va a usar en el cliente
* Parámetros del comando que optimizan el éxito de este tipo de montaje de NFS

La página **Instrucciones de montaje** de la memoria caché recopila la información y las opciones recomendadas y crea un prototipo de comando mount que puede copiar. Consulte [Uso de la utilidad de instrucciones de montaje](#use-the-mount-instructions-utility) para más información.

## <a name="prepare-clients"></a>Preparación de los clientes

Asegúrese de que los clientes pueden montar la instancia de Azure HPC Cache según las instrucciones de esta sección.

### <a name="provide-network-access"></a>Acceso de red

Los equipos cliente deben tener acceso de red a la red virtual y la subred privada de la memoria caché.

Por ejemplo, cree máquinas virtuales cliente dentro de la misma red virtual, o bien use un punto de conexión, una puerta de enlace u otra solución de la red virtual para acceder desde fuera. (Recuerde que no se debe hospedar nada que no sea la propia memoria caché dentro de la subred de la memoria caché).

### <a name="install-utilities"></a>Instalación de utilidades

Instale el software de utilidad de Linux adecuado para admitir el comando de montaje de NFS:

* Para Red Hat Enterprise Linux o SuSE: `sudo yum install -y nfs-utils`
* Para Ubuntu o Debian: `sudo apt-get install nfs-common`

### <a name="create-a-local-path"></a>Creación de una ruta de acceso local

Cree una ruta de acceso al directorio local en cada cliente para conectarse a la memoria caché. Cree una ruta de acceso para cada destino de almacenamiento que desee montar.

Ejemplo: `sudo mkdir -p /mnt/hpc-cache-1/target3`

## <a name="use-the-mount-instructions-utility"></a>Uso de la utilidad de instrucciones de montaje

Abra la página **Instrucciones de montaje** de la sección **Configurar** de la vista de memoria caché de Azure Portal.

![captura de pantalla de una instancia de Azure HPC Cache en el portal, con la página Configurar > Instrucciones de montaje cargada](media/mount-instructions.png)

La página del comando de montaje incluye información sobre el proceso de montaje del cliente y los requisitos previos, además de los campos que puede usar para crear un comando de montaje que se pueda copiar.

Para usar esta página, siga este procedimiento:

<!--1.  In step one of **Mounting your file system**, enter the path that the client will use to access the Azure HPC Cache storage target.

   * This path is local to the client.
   * After you provide the directory name, the field populates with a command you can copy. Use this command on the client directly or in a setup script to create the directory path on the client VM. -->

1. Revise los requisitos previos de cliente e instale las utilidades necesarias para usar el comando `mount` de NFS como se describió anteriormente en [Preparación de los clientes](#prepare-clients).

1. El paso uno de **Montaje del sistema de archivos**<!-- label will change --> proporciona un comando de ejemplo para crear la ruta de acceso local en el cliente. Esta es la ruta de acceso que el cliente usará para acceder al contenido de la instancia de Azure HPC Cache.

   Tome nota del nombre de la ruta de acceso para que pueda modificarla en el comando, si es necesario.

1. En el paso dos, seleccione una de las direcciones IP disponibles. Aquí se enumeran todos los [puntos de montaje de cliente](#find-mount-command-components) de la memoria caché. Asegúrese de que tiene un sistema para equilibrar la carga entre todas las direcciones IP.

1. El campo del paso tres se rellena automáticamente con un prototipo de comando de montaje. Haga clic en el símbolo de copia en el lado derecho del campo para copiarlo automáticamente en el Portapapeles.

   > [!NOTE]
   > Compruebe el comando copiado antes de usarlo. Es posible que tenga que personalizar la ruta de acceso de montaje de cliente y la ruta de acceso del espacio de nombres virtual del destino de almacenamiento, que todavía no se pueden seleccionar en esta interfaz. También debe actualizar las opciones del comando de montaje para reflejar las [opciones recomendadas](#mount-command-options) siguientes. Consulte [Descripción de la sintaxis del comando mount](#understand-mount-command-syntax) para obtener ayuda.

1. Use el comando mount copiado (con las ediciones necesarias) en el equipo cliente para conectarlo al destino de almacenamiento en la instancia de Azure HPC Cache. Puede emitir el comando directamente desde la línea de comandos del cliente o incluir el comando mount en un script o una plantilla de instalación del cliente.

## <a name="understand-mount-command-syntax"></a>Descripción de la sintaxis del comando mount

El comando mount tiene el siguiente formato:

> sudo mount *cache_mount_address*:/*namespace_path* *local_path* {*options*}

Ejemplo:

```bash
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount 10.0.0.28:/blob-demo-0722 ./hpccache/ -o hard,proto=tcp,mountproto=tcp,retry=30
root@test-client:/tmp#
```

Si este comando se ejecuta correctamente, el contenido de la exportación de almacenamiento será visible en el directorio ``hpccache`` del cliente.

### <a name="mount-command-options"></a>Opciones del comando de montaje

Para garantizar un montaje sólido del cliente, pase esta configuración y argumentos en el comando de montaje:

> mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}

| Configuración recomendada del comando de montaje | |
--- | ---
``hard`` | Los montajes flexibles en Azure HPC Cache están asociados con errores de la aplicación y la posible pérdida de datos.
``proto=tcp`` | Esta opción es compatible con el control adecuado de los errores de la red NFS.
``mountproto=tcp`` | Esta opción es compatible con el control adecuado de errores de red para las operaciones de montaje.
``retry=<value>`` | Configure ``retry=30`` para evitar errores de montaje transitorios. (Se recomienda usar un valor diferente en los montajes de primer plano).

### <a name="find-mount-command-components"></a>Búsqueda de componentes del comando mount

Si desea crear un comando de montaje sin usar la página **Instrucciones de montaje**, puede encontrar las direcciones de montaje en la página **Información general** de la memoria caché y las rutas de acceso del espacio de nombres virtual en la página **Destinos de almacenamiento**.

![captura de pantalla de la página de información general de la instancia de Azure HPC Cache, con un cuadro de resaltado alrededor de la lista de direcciones de montaje en la parte inferior derecha](media/hpc-cache-mount-addresses.png)

> [!NOTE]
> Las direcciones de montaje de la caché corresponden a las interfaces de red dentro de la subred de la caché. En un grupo de recursos, estas NIC aparecen con nombres que terminan en `-cluster-nic-` y en un número. No las modifique ni elimine o la caché dejará de estar disponible.

Las rutas de acceso del espacio de nombres virtual se muestran en la página **Destinos de almacenamiento**. Haga clic en un nombre de destino de almacenamiento para ver sus detalles, incluidas las rutas de acceso del espacio de nombres agregadas asociadas a él.

![captura de pantalla del panel de destino de almacenamiento de la caché, con un cuadro de resaltado alrededor de una entrada en la columna Ruta de acceso de la tabla](media/hpc-cache-view-namespace-paths.png)

## <a name="next-steps"></a>Pasos siguientes

* Para mover datos a los destinos de almacenamiento de la caché, lea [Rellenado del nuevo almacenamiento de blobs de Azure](hpc-cache-ingest.md).
