---
title: Actualización de Data Science Virtual Machine a Ubuntu 18.04
titleSuffix: Azure Data Science Virtual Machine
description: Obtenga información sobre cómo actualizar Data Science Virtual Machine de CentOS y Ubuntu 16.04 a la versión más reciente de  Ubuntu 18.04.
keywords: aprendizaje profundo, IA, herramientas de ciencia de datos, data science virtual machine, proceso de ciencia de datos en equipo
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: samkemp
ms.author: samkemp
ms.topic: conceptual
ms.date: 10/07/2020
ms.openlocfilehash: 5b98384d4d735f4c124c6af40d6edbff896900ce
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93320975"
---
# <a name="upgrade-your-data-science-virtual-machine-to-ubuntu-1804"></a>Actualización de Data Science Virtual Machine a Ubuntu 18.04

Si tiene una instancia de Data Science Virtual Machine que ejecuta una versión anterior, como Ubuntu 16.04 o CentOS, debe migrar la DSVM a Ubuntu 18.04. La migración garantizará que obtenga las últimas revisiones del sistema operativo, controladores, software preinstalado y versiones de bibliotecas. En este documento se explica cómo migrar desde versiones anteriores de Ubuntu o de CentOS. 

## <a name="prerequisites"></a>Prerrequisitos

- Familiaridad con SSH y la línea de comandos de Linux

## <a name="overview"></a>Información general

Existen dos formas posibles de migrar:

- Migración en contexto, también denominada migración en el "mismo servidor". Esta migración actualiza la VM existente sin crear una nueva. La migración directa es la forma más sencilla de migrar desde Ubuntu 16.04 a Ubuntu 18.04.
- Migración en paralelo, también denominada migración "entre servidores". Esta migración transfiere datos desde la VM existente a una VM recién creada. La migración en paralelo es la manera de migrar de CentOS a Ubuntu 18.04. Puede preferir la migración en paralelo para la actualización entre versiones de Ubuntu si cree que la instalación anterior se ha vuelto demasiado desorganizada.

## <a name="snapshot-your-vm-in-case-you-need-to-roll-back"></a>Creación de una instantánea de la VM en caso de que necesite revertirla

En Azure Portal, use la barra de búsqueda para buscar la funcionalidad **Instantáneas**. 

:::image type="content" source="media/ubuntu_upgrade/azure-portal-search-bar.png" alt-text="Captura de pantalla que muestra Azure Portal y la barra de búsqueda, con **Instantáneas** resaltado.":::

1. Seleccione **Agregar** , lo que le llevará a la página **Crear instantánea**. Seleccione la suscripción y el grupo de recursos de la máquina virtual. En **Región** , seleccione la misma región en la que se encuentra el almacenamiento de destino. Seleccione el disco de almacenamiento de DSVM y las opciones de copia de seguridad adicionales. **HDD estándar** es un tipo de almacenamiento adecuado para este escenario de copia de seguridad.

:::image type="content" source="media/ubuntu_upgrade/create-snapshot-options.png" alt-text="Captura de pantalla que muestra las opciones de &quot;Crear instantánea&quot;.":::

2. Una vez que se completen todos los detalles y se superen las validaciones, seleccione **Revisar y crear** para validar y crear la instantánea. Cuando la instantánea finalice correctamente, verá un mensaje que indica que la implementación se ha completado.

## <a name="in-place-migration"></a>Migración en contexto

Si va a migrar una versión anterior de Ubuntu, puede optar por realizar una migración en contexto. Esta migración no crea una nueva máquina virtual y tiene menos pasos que una migración en paralelo.  Si quiere realizar una migración en paralelo porque quiere tener más control o porque está migrando desde una distribución diferente, como CentOS, vaya a la sección [Migración en paralelo](#side-by-side-migration). 

1. En Azure Portal, inicie DSVM e inicie sesión con SSH. Para ello, seleccione **Conectar** y **SSH** , y siga las instrucciones de conexión. 

1. Una vez que se haya conectado a una sesión del terminal en la instancia de DSVM, ejecute el siguiente comando de actualización:

    ```bash
    sudo do-release-upgrade
    ```

El proceso de actualización tardará un tiempo en completarse. Una vez que termine, el programa solicitará permiso para reiniciar la máquina virtual. Responda **Sí**. Se desconectará de la sesión de SSH cuando se reinicie el sistema.

### <a name="if-necessary-regenerate-ssh-keys"></a>Regeneración de las claves SSH, de ser necesario

> [!IMPORTANT] 
> Después de la actualización y el reinicio, es posible que tenga que regenerar las claves SSH.

Una vez que la VM se haya actualizado y reiniciado, intente acceder a ella de nuevo a través de SSH. Es posible que la dirección IP haya cambiado durante el reinicio, así que confírmela antes de intentar conectarse.

Si recibe el error **REMOTE HOST IDENTIFICATION HAS CHANGED** (LA IDENTIFICACIÓN DEL HOST REMOTO HA CAMBIADO), deberá regenerar las credenciales de SSH.

:::image type="content" source="media/ubuntu_upgrade/remote-host-warning.png" alt-text="Captura de pantalla de PowerShell que muestra la advertencia de cambio de identificación de host remoto.":::

Para ello, en la máquina local, ejecute el comando:

```bash
ssh-keygen -R "your server hostname or ip"
```

Ahora debería poder conectarse con SSH. Si sigue teniendo problemas, en la página **Conectar** , siga el vínculo a **Solución de problemas de conectividad de SSH**.

## <a name="side-by-side-migration"></a>Migración en paralelo

Si va a migrar desde CentOS o quiere una instalación limpia del sistema operativo, puede realizar una migración en paralelo. Este tipo de migración tiene más pasos, pero le permite controlar exactamente qué archivos se transfieren.

Las migraciones desde otros sistemas basados en el mismo conjunto de paquetes de origen ascendente deberían ser relativamente sencillas, por ejemplo, consulte [Preguntas frecuentes sobre CentOS3](https://wiki.centos.org/FAQ/CentOS3).

Puede optar por actualizar las partes del sistema de archivos correspondientes al sistema operativo y dejar los directorios de usuario, como `/home`, tal cual. Si deja los directorios principales de los usuarios anteriores tal cual, es de esperar que haya algunos problemas con los menús de GNOME/KDE y otros elementos del escritorio. Puede ser más fácil crear nuevas cuentas de usuario y montar los directorios antiguos en otro lugar del sistema de archivos para hacer referencia al material de los usuarios, copiarlo o vincularlo después de la migración.

### <a name="migration-at-a-glance"></a>Migración de un vistazo

1.  Creación de una instantánea de la VM existente como se ha descrito anteriormente

1.  Creación de un disco a partir de esa instantánea

1.  Creación de una nueva instancia de Data Science Virtual Machine de Ubuntu

1.  Recreación de cuentas de usuario en la nueva máquina virtual

1.  Montaje del disco de la VM de la que se ha creado una instantánea como disco de datos en la nueva instancia de Data Science Virtual Machine

1.  Copia manual de los datos deseados

### <a name="create-a-disk-from-your-vm-snapshot"></a>Creación de un disco a partir de la instantánea de la VM

Si aún no ha creado una instantánea de la VM como se ha descrito anteriormente, hágalo. 

1. En Azure Portal, busque **Discos** y seleccione **Agregar** , lo que abrirá la página **Disco**.

:::image type="content" source="media/ubuntu_upgrade/portal-disks-search.png" alt-text="Captura de pantalla de Azure Portal que muestra la búsqueda de la página Discos y el botón Agregar.":::

2. Establezca la **Suscripción** , el **Grupo de recursos** y la **Región** en los valores de la instantánea de la VM. Elija un **Nombre** para el disco que va a crear.

3. Seleccione **Tipo de origen** como **Instantánea** y seleccione la instantánea de la VM como **Instantánea de origen**. Revise y cree el disco. 

:::image type="content" source="media/ubuntu_upgrade/disk-create-options.png" alt-text="Captura de pantalla del cuadro de diálogo de creación del disco, que muestra las opciones.":::

### <a name="create-a-new-ubuntu-data-science-virtual-machine"></a>Creación de una nueva instancia de Data Science Virtual Machine de Ubuntu

Cree una nueva instancia de Data Science Virtual Machine de Ubuntu mediante [Azure Portal](https://portal.azure.com) o una [plantilla de Resource Manager](./dsvm-tutorial-resource-manager.md). 

### <a name="recreate-user-accounts-on-your-new-data-science-virtual-machine"></a>Recreación de las cuentas de usuario en la nueva instancia de Data Science Virtual Machine

Puesto que solo va a copiar los datos desde el equipo antiguo, tendrá que volver a crear las cuentas de usuario y los entornos de software que quiera usar en la nueva máquina.

Linux es lo suficientemente flexible como para que pueda personalizar los directorios y las rutas de acceso de la nueva instalación según la máquina antigua. Sin embargo, en general es más fácil usar el diseño preferido de Ubuntu moderno y modificar el entorno de usuario y los scripts para adaptarse.

Para más información, consulte [Inicio rápido: Configuración de Data Science Virtual Machine para Linux (Ubuntu)](./dsvm-ubuntu-intro.md).

### <a name="mount-the-disk-of-the-snapshotted-vm-as-a-data-disk-on-your-new-data-science-virtual-machine"></a>Montaje del disco de la VM de la que se ha creado una instantánea como disco de datos en la nueva instancia de Data Science Virtual Machine

1. En Azure Portal, asegúrese de que la instancia de Data Science Virtual Machine esté en ejecución.

2. En Azure Portal, vaya a la página de la instancia de Data Science Virtual Machine. Elija la hoja **Discos** en el raíl izquierdo. Elija **Attach existing disks** (Conectar discos existentes).

3. En el menú desplegable **Nombre del disco** , seleccione el disco que creó a partir de la instantánea de la VM anterior.

:::image type="content" source="media/ubuntu_upgrade/attach-data-disk.png" alt-text="Captura de pantalla de la página de opciones de DSVM que muestra las opciones de conexión de discos.":::

4. Seleccione **Guardar** para actualizar la máquina virtual.

> [!Important]
> La VM debe estar en ejecución en el momento en que conecta el disco de datos. Si la VM no se está ejecutando, los discos se pueden agregar en un orden incorrecto, lo que conduce a un sistema confuso y que posiblemente no pueda arrancar. Si agrega el disco de datos con la VM apagada, elija la **X** junto al disco de datos, inicie la VM y vuelva a conectarla.

### <a name="manually-copy-the-wanted-data"></a>Copia manual de los datos deseados 

1. Inicie sesión en la máquina virtual en ejecución con SSH.

2. Para confirmar que ha conectado el disco creado a partir de la instantánea de la VM anterior, ejecute el siguiente comando:

    ```bash
    lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i 'sd'
    ```
    
    Los resultados deben ser similares a la siguiente imagen. En la imagen, el disco `sda1` está montado en la raíz, y `sdb2` es el disco temporal `/mnt`. El disco de datos creado a partir de la instantánea de la VM anterior se identifica como `sdc1`, pero aún no está disponible, como se demuestra en la falta de una ubicación de montaje. Sus resultados pueden tener otros identificadores, pero debería ver un patrón similar.
    
    :::image type="content" source="media/ubuntu_upgrade/lsblk-results.png" alt-text="Captura de pantalla de la salida de lsblk, que muestra la unidad de datos sin montar.":::
    
3. Para acceder a la unidad de datos, cree una ubicación para ella y móntela. Reemplace `/dev/sdc1` por el valor adecuado devuelto por `lsblk`:

    ```bash
    sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
    ```
    
4. Ahora, `/datadrive` contiene los directorios y archivos de la instancia anterior de Data Science Virtual Machine. Mueva o copie los directorios o archivos que quiera de la unidad de datos a la nueva VM como desee.

Para obtener más información, consulte [Uso del portal para conectar un disco de datos a una VM Linux](../../virtual-machines/linux/attach-disk-portal.md#connect-to-the-linux-vm-to-mount-the-new-disk).

## <a name="connect-and-confirm-version-upgrade"></a>Conexión y confirmación de la actualización de la versión

Independientemente de si realizó una migración en contexto o en paralelo, confirme que se ha actualizado correctamente. Desde una sesión del terminal, ejecute: 

```bash
cat /etc/os-release
```

Y debería ver que se ejecuta Ubuntu 18.04.

:::image type="content" source="media/ubuntu_upgrade/ssh-os-release.png" alt-text="Captura de pantalla del terminal de Ubuntu que muestra los datos de la versión del sistema operativo.":::

El cambio de versión también se muestra en Azure Portal.

:::image type="content" source="media/ubuntu_upgrade/portal-showing-os-version.png" alt-text="Captura de pantalla del portal que muestra las propiedades de DSVM, incluida la versión del sistema operativo.":::

## <a name="next-steps"></a>Pasos siguientes

- [Ciencia de datos con una instancia de Data Science Virtual Machine de Ubuntu en Azure](./linux-dsvm-walkthrough.md)
- [¿Qué herramientas se incluyen en Azure Data Science Virtual Machine?](./tools-included.md)