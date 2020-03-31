---
title: Crear un servidor NFS (sistema de archivos de red) de Ubuntu para que lo utilicen los pods de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo crear manualmente un volumen de servidor NFS de Ubuntu Linux para usarlo con pods en Azure Kubernetes Service (AKS)
services: container-service
author: ozboms
ms.topic: article
ms.date: 4/25/2019
ms.author: obboms
ms.openlocfilehash: e5676710bc47557318f3e2adcf36ec0ed13d47de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596630"
---
# <a name="manually-create-and-use-an-nfs-network-file-system-linux-server-volume-with-azure-kubernetes-service-aks"></a>Crear manualmente y usar un volumen de servidor NFS (sistema de archivos de red) de Linux con Azure Kubernetes Service (AKS)
Compartir datos entre contenedores a menudo es un componente necesario de las aplicaciones y los servicios basados en contenedores. Habitualmente tiene varios pods que necesitan tener acceso a la misma información en un volumen persistente externo.    
Si bien los archivos de Azure son una posibilidad, crear un servidor NFS en una máquina virtual de Azure es otra forma de almacenamiento compartido persistente. 

En este artículo se muestra cómo crear un servidor NFS en una máquina virtual de Ubuntu. Asimismo, se explica cómo dar acceso a los contenedores de AKS a este sistema de archivos compartido.

## <a name="before-you-begin"></a>Antes de empezar
En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

El clúster de AKS debe residir en las mismas redes virtuales emparejadas que el servidor NFS. El clúster debe crearse en una red virtual existente, que puede ser la misma que la de su máquina virtual.

En la documentación se describen los pasos para configurar una red virtual existente: [Creación de un clúster de AKS en la red virtual][aks-virtual-network] y [Conexión de redes virtuales con emparejamiento de redes virtuales][peer-virtual-networks]

También se supone que ha creado una máquina Virtual de Ubuntu Linux (por ejemplo, 18.04 LTS). La configuración y el tamaño pueden ser a su gusto y pueden implementarse mediante Azure. En el caso del inicio rápido de Linux, consulte el artículo sobre la [administración de máquinas virtuales de Linux][linux-create].

Si implementa el clúster de AKS primero, Azure automáticamente rellenará el campo de red virtual al implementar la máquina de Ubuntu, conectándolos dentro de la misma red virtual. Sin embargo, si desea trabajar con redes emparejadas, consulte la documentación anterior.

## <a name="deploying-the-nfs-server-onto-a-virtual-machine"></a>Implementar el servidor NFS en una máquina virtual
A continuación se muestra el script para configurar un servidor NFS dentro de la máquina virtual de Ubuntu:
```bash
#!/bin/bash

# This script should be executed on Linux Ubuntu Virtual Machine

EXPORT_DIRECTORY=${1:-/export/data}
DATA_DIRECTORY=${2:-/data}
AKS_SUBNET=${3:-*}

echo "Updating packages"
apt-get -y update

echo "Installing NFS kernel server"

apt-get -y install nfs-kernel-server

echo "Making data directory ${DATA_DIRECTORY}"
mkdir -p ${DATA_DIRECTORY}

echo "Making new directory to be exported and linked to data directory: ${EXPORT_DIRECTORY}"
mkdir -p ${EXPORT_DIRECTORY}

echo "Mount binding ${DATA_DIRECTORY} to ${EXPORT_DIRECTORY}"
mount --bind ${DATA_DIRECTORY} ${EXPORT_DIRECTORY}

echo "Giving 777 permissions to ${EXPORT_DIRECTORY} directory"
chmod 777 ${EXPORT_DIRECTORY}

parentdir="$(dirname "$EXPORT_DIRECTORY")"
echo "Giving 777 permissions to parent: ${parentdir} directory"
chmod 777 $parentdir

echo "Appending bound directories into fstab"
echo "${DATA_DIRECTORY}    ${EXPORT_DIRECTORY}   none    bind  0  0" >> /etc/fstab

echo "Appending localhost and Kubernetes subnet address ${AKS_SUBNET} to exports configuration file"
echo "/export        ${AKS_SUBNET}(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports
echo "/export        localhost(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports

nohup service nfs-kernel-server restart
```
El servidor se reiniciará (debido al script) y podrá montar el servidor NFS en AKS.

>[!IMPORTANT]  
>Asegúrese de reemplazar el **AKS_SUBNET** por el correcto desde el clúster o, de lo contrario, "*" abrirá el servidor NFS a todos los puertos y conexiones.

Después de crear la máquina virtual, copie el script anterior en un archivo. A continuación, puede moverlo desde la máquina local o la ubicación donde se encuentre el script a la máquina virtual mediante: 
```console
scp /path/to/script_file username@vm-ip-address:/home/{username}
```
Una vez que el script se encuentra en la máquina virtual, puede acceder mediante ssh a la máquina virtual y ejecutarla mediante este comando:
```console
sudo ./nfs-server-setup.sh
```
Si se produce un error en la ejecución debido a un error de permiso denegado, establezca el permiso de ejecución mediante el siguiente comando:
```console
chmod +x ~/nfs-server-setup.sh
```

## <a name="connecting-aks-cluster-to-nfs-server"></a>Conectar el clúster de AKS al servidor NFS
Podemos conectar el servidor NFS a nuestro clúster aprovisionando un volumen persistente y una notificación de volumen persistente que especifique cómo acceder al volumen.

Es necesario conectar ambos servicios en la misma red virtual o en redes virtuales emparejadas. Las instrucciones para configurar el clúster en la misma red virtual se encuentran en este artículo: [Creación de un clúster de AKS en la red virtual][aks-virtual-network]

Una vez que se encuentren en la misma red virtual (o en redes virtuales emparejadas), deberá aprovisionar un volumen persistente y una notificación de volumen persistente en el clúster de AKS. A continuación, los contenedores pueden montar la unidad NFS en su directorio local.

A continuación, se incluye un ejemplo de una definición de Kubernetes para el volumen persistente (esta definición presupone que el clúster y la máquina virtual están en la misma red virtual):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: <NFS_NAME>
  labels:
    type: nfs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <NFS_INTERNAL_IP>
    path: <NFS_EXPORT_FILE_PATH>
```
Reemplace **NFS_INTERNAL_IP**, **NFS_NAME** y **NFS_EXPORT_FILE_PATH** con información del servidor NFS.

También necesitará un archivo de notificación de volumen persistente. A continuación se muestra un ejemplo de lo que se debe incluir:

>[!IMPORTANT]  
>**"storageClassName"** debe seguir siendo una cadena vacía para que la notificación funcione.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <NFS_NAME>
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
  selector: 
    matchLabels:
      type: nfs
```

## <a name="troubleshooting"></a>Solución de problemas
Si no puede conectarse al servidor desde un clúster, puede que haya un problema en el directorio exportado o en su directorio primario, o bien que no tenga suficientes permisos para acceder al servidor.

Compruebe que el directorio de exportación y su directorio primario tienen 777 permisos.

Para comprobar los permisos, ejecute el comando siguiente; los directorios deberían tener los permisos *'drwxrwxrwx'* :
```console
ls -l
```

## <a name="more-information"></a>Más información
Para obtener un tutorial completo o ayuda para depurar la instalación del servidor NFS, consulte este tutorial exhaustivo:
  - [Tutorial sobre NFS][nfs-tutorial]

## <a name="next-steps"></a>Pasos siguientes

Para consultar los procedimientos recomendados asociados, consulte [Procedimientos recomendados para el almacenamiento y las copias de seguridad en Azure Kubernetes Service (AKS)][operator-best-practices-storage].

<!-- LINKS - external -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[linux-create]: https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm
[nfs-tutorial]: https://help.ubuntu.com/community/SettingUpNFSHowTo#Pre-Installation_Setup
[aks-virtual-network]: https://docs.microsoft.com/azure/aks/configure-kubenet#create-an-aks-cluster-in-the-virtual-network
[peer-virtual-networks]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[operator-best-practices-storage]: operator-best-practices-storage.md
