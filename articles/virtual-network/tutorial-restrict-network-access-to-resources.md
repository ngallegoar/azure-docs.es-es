---
title: 'Restricción del acceso a los recursos de PaaS (tutorial): Azure Portal'
description: En este tutorial aprenderá a limitar y restringir el acceso de la red a los recursos de Azure, como Azure Storage y Azure SQL Database, con puntos de conexión de servicio de red virtual mediante Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 08/23/2018
ms.author: kumud
ms.openlocfilehash: 85fc5687b82947ed16bde0c30ca2b947514ba958
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74186367"
---
# <a name="tutorial-restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Tutorial: Restricción del acceso de la red a los recursos de PaaS mediante puntos de conexión de servicio de red virtual mediante Azure Portal.

Los puntos de conexión de servicio de red virtual permiten que el acceso de la red a algunos recursos de servicio de Azure esté restringido a una subred de la red virtual. También se puede quitar el acceso de Internet a los recursos. Los puntos de conexión de servicio proporcionan a la red virtual conexión directa con los servicios de Azure compatibles, de modo que se puede usar el espacio de direcciones privadas de la red virtual para acceder a los servicios de Azure. El tráfico destinado a los recursos de Azure a través de los puntos de conexión de servicio siempre se mantiene en la red troncal de Microsoft Azure. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una red virtual con una subred
> * Agregar una subred y habilitar un punto de conexión de servicio
> * Crear un recurso de Azure y permitir que la red solo pueda acceder a él desde una subred
> * Implementar una máquina virtual en cada subred
> * Confirmar el acceso a un recurso desde una subred
> * Confirmar que se ha denegado el acceso a un recurso desde una subred e Internet

Si lo prefiere, puede completar este tutorial con la [CLI de Azure](tutorial-restrict-network-access-to-resources-cli.md) o [Azure PowerShell](tutorial-restrict-network-access-to-resources-powershell.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Redes** y **Red virtual**.
3. Especifique o seleccione los siguientes datos y haga clic en **Crear**:

   |Configuración|Value|
   |----|----|
   |Nombre| myVirtualNetwork |
   |Espacio de direcciones| 10.0.0.0/16|
   |Subscription| Seleccione su suscripción.|
   |Resource group | Haga clic en **Crear nuevo** y escriba *myResourceGroup*.|
   |Location| Seleccione **Este de EE. UU**. |
   |Nombre de subred| Público|
   |Intervalo de direcciones de subred| 10.0.0.0/24|
   |Protección contra DDOS| Básica|
   |Puntos de conexión del servicio| Disabled|
   |Firewall| Disabled|

   ![Especificar información básica acerca de la red virtual](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)

## <a name="enable-a-service-endpoint"></a>Habilitación de un punto de conexión de servicio

Los punto de conexión de servicio están habilitados por servicio, por subred. Cree una subred y habilite un punto de conexión de servicio para ella.

1. En el cuadro **Search resources, services, and docs** (Buscar recursos, servicios y documentos) que encontrará en la parte superior del portal, escriba *myVirtualNetwork*. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela.
2. Agregue una subred a la red virtual. En **Configuración**, seleccione **Subredes** y **+ Subred**, tal y como se muestra en la imagen siguiente:

    ![Subred agregada](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. En **Agregar subred**, escriba o seleccione los siguientes datos y haga clic en **Aceptar**:

    |Configuración|Value|
    |----|----|
    |Nombre| Privada |
    |Intervalo de direcciones| 10.0.1.0/24|
    |Puntos de conexión del servicio| Seleccione **Microsoft.Storage** en **Servicios**.|

> [!CAUTION]
> Antes de habilitar un punto de conexión de servicio para una subred existente que tenga recursos, consulte [Modificación de la configuración de subred](virtual-network-manage-subnet.md#change-subnet-settings).

## <a name="restrict-network-access-for-a-subnet"></a>Restricción del acceso de la red para una subred

De forma predeterminada, todas las máquinas virtuales de una subred pueden comunicarse con todos los recursos. Puede limitar la comunicación hacia y desde todos los recursos de una subred mediante la creación de un grupo de seguridad de red y su asociación a la subred.

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Redes** y **Grupo de seguridad de red**.
3. En **Create a network security group** (Crear un grupo de seguridad de red), especifique o seleccione los datos siguientes y haga clic en **Crear**:

    |Configuración|Value|
    |----|----|
    |Nombre| myNsgPrivate |
    |Subscription| Seleccione su suscripción.|
    |Resource group | Seleccione **Usar existente** y, a continuación, *myResourceGroup*.|
    |Location| Seleccione **Este de EE. UU**. |

4. Cuando haya creado el grupo de seguridad de red, escriba *myNsgPrivate* en el cuadro **Search resources, services, and docs** (Buscar recursos, servicios y documentos) situado en la parte superior del portal. Cuando aparezca **myNsgPrivate** en los resultados de búsqueda, selecciónelo.
5. En **Configuración**, seleccione **Reglas de seguridad de salida**.
6. Seleccione **+Agregar**.
7. Cree una regla que permita la comunicación saliente con el servicio Azure Storage. Especifique o seleccione los siguientes datos y haga clic en **Agregar**:

    |Configuración|Value|
    |----|----|
    |Source| Seleccione **VirtualNetwork** |
    |Source port ranges| * |
    |Destination | Seleccione **Service Tag** (Etiqueta de servicio)|
    |Etiqueta de servicio de destino | Seleccione **Storage** (Almacenamiento)|
    |Intervalos de puertos de destino| * |
    |Protocolo|Any|
    |Acción|Allow|
    |Priority|100|
    |Nombre|Allow-Storage-All (Permitir-almacenar-todo)|

8. Cree otra regla de seguridad de salida que deniegue la comunicación a Internet. Esta regla invalida una regla predeterminada en todos los grupos de seguridad de red que permite la comunicación saliente de Internet. Repita los pasos 5 a 7 de nuevo, utilizando los siguientes valores:

    |Configuración|Value|
    |----|----|
    |Source| Seleccione **VirtualNetwork** |
    |Source port ranges| * |
    |Destination | Seleccione **Service Tag** (Etiqueta de servicio)|
    |Etiqueta de servicio de destino| Seleccione **Internet**|
    |Intervalos de puertos de destino| * |
    |Protocolo|Any|
    |Acción|Denegar|
    |Priority|110|
    |Nombre|Deny-Internet-All|

9. En **Configuración**, seleccione **Reglas de seguridad de entrada**.
10. Seleccione **+Agregar**.
11. Cree una regla de seguridad de entrada que permita que el tráfico de Protocolo de escritorio remoto (RDP) a la subred desde cualquier lugar. La regla invalidará cualquier regla de seguridad predeterminada que deniegue todo el tráfico de entrada procedente de Internet. Las conexiones entre Escritorio remoto y la subred están permitidas, por lo que dicha conectividad podrá probarse en un paso posterior. Seleccione **CONFIGURACIÓN**, **Reglas de seguridad de entrada** y, después, **+ Agregar**. Escriba los siguientes valores y, a continuación, seleccione **Agregar**:

    |Configuración|Value|
    |----|----|
    |Source| Any |
    |Source port ranges| * |
    |Destination | Seleccione **VirtualNetwork**|
    |Intervalos de puertos de destino| 3389 |
    |Protocolo|Any|
    |Acción|Allow|
    |Priority|120|
    |Nombre|Allow-RDP-All (Permitir-RDP-Todo)|

12. En **CONFIGURACIÓN**, seleccione **Subredes**.
13. Seleccione **+ Asociar**
14. En **Asociar subred**, seleccione **Red virtual** y, en **Elegir red virtual**, seleccione **myVirtualNetwork**.
15. En **Elegir subred**, seleccione **Privada** y después **Aceptar**.

## <a name="restrict-network-access-to-a-resource"></a>Restricción del acceso de la red a un recurso

Los pasos que deben seguirse para restringir el acceso de la red a los recursos creados con servicios de Azure habilitados para puntos de conexión de servicio varían en función del servicio. Consulte en la documentación de cada servicio concreto los pasos necesarios para dicho servicio. Como ejemplo, de aquí en adelante se explican los pasos necesarios para restringir el acceso de red en una cuenta de Azure Storage.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Almacenamiento** y, a continuación, seleccione **Cuenta de almacenamiento: blob, archivo, tabla, cola**.
3. Escriba o seleccione la siguiente información, acepte los valores predeterminados restantes y haga clic en **Crear**:

    |Configuración|Value|
    |----|----|
    |Nombre| Especifique un nombre que sea único en todas las ubicaciones de Azure, que tenga entre 3 y 24 caracteres de longitud y que esté compuesto exclusivamente de números y letras en minúscula.|
    |Tipo de cuenta|StorageV2 (uso general v2)|
    |Location| Seleccione **Este de EE. UU**. |
    |Replicación| Almacenamiento con redundancia local (LRS)|
    |Subscription| Seleccione su suscripción.|
    |Resource group | Seleccione **Usar existente** y, a continuación, *myResourceGroup*.|

### <a name="create-a-file-share-in-the-storage-account"></a>Creación de un recurso compartido de archivos en la cuenta de almacenamiento

1. Cuando haya creado la cuenta de almacenamiento, especifique su nombre en el cuadro **Buscar recursos, servicios y documentos**, situado en la parte superior del portal. Cuando el nombre de la cuenta de almacenamiento aparezca en los resultados de búsqueda, selecciónelo.
2. Seleccione **Archivos**, tal y como se muestra en la siguiente ilustración:

   ![Cuenta de almacenamiento](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 

3. Seleccione **+ Recurso compartido de archivos**.
4. Escriba *my-file-share* en **Nombre** y seleccione **Aceptar**.
5. Cierre el cuadro **Servicio Archivo**.

### <a name="restrict-network-access-to-a-subnet"></a>Restricción del acceso de la red a una subred

De forma predeterminada, las cuentas de almacenamiento aceptan conexiones de red procedentes de clientes de cualquier red, incluido Internet. Deniegue el acceso a la red desde Internet y a todas las demás subredes de todas las redes virtuales, excepto a la subred *Private* de la red virtual *myVirtualNetwork*.

1. En la opción **Configuración** de la cuenta de almacenamiento, seleccione **Firewalls and virtual networks** (Firewalls y redes virtuales).
2. Seleccione **Redes seleccionadas**.
3. Seleccione **+Agregar red virtual existente**.
4. En **Agregar redes**, seleccione los siguientes valores y haga clic en **Agregar**:

    |Configuración|Value|
    |----|----|
    |Subscription| Seleccione su suscripción.|
    |Redes virtuales|Seleccione **myVirtualNetwork** en **Redes virtuales**|
    |Subredes| Seleccione **Privada** en **Subredes**|

    ![Firewalls y redes virtuales](./media/tutorial-restrict-network-access-to-resources/storage-firewalls-and-virtual-networks.png)

5. Seleccione **Guardar**.
6. Cierre el cuadro **Firewalls and virtual networks** (Firewalls y redes virtuales).
7. En la opción **Configuración** de la cuenta de almacenamiento, seleccione **Claves de acceso**, tal y como se muestra en la siguiente ilustración:

      ![Firewalls y redes virtuales](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

8. Anote el valor del campo **Clave**, ya que tendrá que escribirlo manualmente más adelante, cuando asigne el recurso compartido de archivos a una letra de unidad de una máquina virtual.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Para probar el acceso de la red a una cuenta de almacenamiento, implemente una máquina virtual en cada subred.

### <a name="create-the-first-virtual-machine"></a>Creación de la primera máquina virtual

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Compute** y, después, seleccione **Windows Server 2016 Datacenter**.
3. Especifique o seleccione los siguientes datos y haga clic en **Aceptar**:

   |Configuración|Value|
   |----|----|
   |Nombre| myVmPublic|
   |Nombre de usuario|Escriba un nombre de usuario de su elección.|
   |Contraseña| Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
   |Subscription| Seleccione su suscripción.|
   |Resource group| Seleccione **Usar existente** y, a continuación, **myResourceGroup**.|
   |Location| Seleccione **Este de EE. UU**.|

   ![Escribir información esencial acerca de una máquina virtual](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)
4. Seleccione un tamaño para la máquina virtual y luego **Seleccionar**.
5. En **Configuración**, seleccione **Red** y haga clic en **myVirtualNetwork**. Seleccione **Subred** y **Público**, tal y como se muestra en la ilustración siguiente:

   ![Seleccionar una red virtual](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)

6. En **Grupo de seguridad de red**, seleccione **Avanzado**. El portal crea automáticamente un grupo de seguridad de red que permite el puerto 3389, que debe estar abierto para conectarse a la máquina virtual en un paso posterior. Seleccione **Aceptar** en la página **Configuración**.
7. En la página **Resumen**, seleccione **Crear** para iniciar la implementación de la máquina virtual. La máquina virtual tarda unos minutos en implementarse, pero puede continuar con el paso siguiente mientras se crea.

### <a name="create-the-second-virtual-machine"></a>Creación de la segunda máquina virtual

Repita los pasos 1 a 7, pero, en el paso 3, llame a la máquina virtual *myVmPrivate* y, en el paso 5, seleccione la subred **Privada**.

La maquina virtual tarda unos minutos en implementarse. No avance al paso siguiente hasta que finalice el proceso y la configuración se abra en el portal.

## <a name="confirm-access-to-storage-account"></a>Confirmación del acceso a la cuenta de almacenamiento

1. Cuando la máquina virtual *myVmPrivate* termine de crearse, Azure abrirá sus opciones de configuración. Conéctese a la máquina virtual seleccionando el botón **Conectar**, tal y como se muestra en la imagen siguiente:

   ![Conexión a una máquina virtual](./media/tutorial-restrict-network-access-to-resources/connect-to-virtual-machine.png)

2. Cuando seleccione el botón **Conectar**, se creará un archivo de Protocolo de Escritorio remoto (.rdp) y se descargará en el equipo.  
3. Abra el archivo .rdp descargado. Cuando se le pida, seleccione **Conectar**. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual. Puede que deba seleccionar **More choices** (Más opciones) y, luego, **Use a different account** (Usar una cuenta diferente), para especificar las credenciales que escribió al crear la máquina virtual. 
4. Seleccione **Aceptar**.
5. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe la advertencia, seleccione **Sí** o **Continuar** para continuar con la conexión.
6. En la máquina virtual *myVmPrivate*, asigne el recurso compartido de archivos de Azure a la unidad Z con PowerShell. Antes de ejecutar los comandos que siguen, reemplace `<storage-account-key>` y `<storage-account-name>` por los valores que proporcionó y recuperó en [Creación de una cuenta de almacenamiento](#create-a-storage-account).

   ```powershell
   $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
   $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
   New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
   ```

   PowerShell devuelve una salida similar a la del ejemplo siguiente:

   ```powershell
   Name           Used (GB)     Free (GB) Provider      Root
   ----           ---------     --------- --------      ----
   Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
   ```

   El recurso compartido de archivos de Azure se ha asignado correctamente a la unidad Z.

7. Confirme que la máquina virtual no tiene conectividad de salida a Internet desde un símbolo del sistema:

   ```
   ping bing.com
   ```

   Dado que el grupo de seguridad de red asociado a la subred *Private* no permite el acceso de salida a Internet, no recibirá ninguna respuesta.

8. Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Confirmación de la denegación del acceso a la cuenta de almacenamiento

1. En el cuadro *Search resources, services, and docs* (Buscar recursos, servicios y documentos) que encontrará en la parte superior del portal, escriba **myVmPublic**.
2. Cuando aparezca **myVmPublic** en los resultados de búsqueda, selecciónelo.
3. Siga los pasos 1 a 6 que se indican en [Confirmación del acceso a la cuenta de almacenamiento](#confirm-access-to-storage-account) con la máquina virtual *myVmPublic*.

   Tras una breve espera, recibirá un error `New-PSDrive : Access is denied`. El acceso se deniega porque la máquina virtual *myVmPublic* está implementada en la subred *Public*. La subred *Public* no tiene un punto de conexión de servicio habilitado para Azure Storage. La cuenta de almacenamiento solo permite el acceso a la red desde la subred *Private*, no desde la subred *Public*.

4. Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPublic*.

5. En el equipo, vaya a [Azure Portal](https://portal.azure.com).
6. Escriba el nombre de la cuenta de almacenamiento que creó en el cuadro **Search resources, services, and docs** (Buscar recursos, servicios y documentos). Cuando el nombre de la cuenta de almacenamiento aparezca en los resultados de búsqueda, selecciónelo.
7. Seleccione **Archivos**.
8. Aparecerá el error que se muestra en la siguiente ilustración:

   ![Error de acceso denegado](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)

   El acceso se deniega porque el equipo no se encuentra en la subred *Privada* de la red virtual *MyVirtualNetwork*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *myResourceGroup* en el cuadro **Buscar** que se encuentra en la parte superior del portal. Seleccione **myResourceGroup** cuando lo vea en los resultados de la búsqueda.
2. Seleccione **Eliminar grupo de recursos**.
3. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha habilitado un punto de conexión de servicio en una subred de la red virtual. Ha aprendido que puede habilitar puntos de conexión de servicio para los recursos implementados desde varios servicios de Azure. Ha creado una cuenta de Azure Storage y ha restringido el acceso de la red a la cuenta de almacenamiento que se encuentra en una subred de la red virtual. Para más información acerca de los puntos de conexión de servicio, consulte [Introducción a los puntos de conexión de un servicio](virtual-network-service-endpoints-overview.md) y [Administración de subredes](virtual-network-manage-subnet.md).

Si tiene varias redes virtuales en la cuenta, es posible que desee conectar dos de ellas para que los recursos que están dentro de cada red virtual puedan comunicarse entre sí. Para aprender a conectar redes virtuales, pase al siguiente tutorial.

> [!div class="nextstepaction"]
> [Conexión de redes virtuales](./tutorial-connect-virtual-networks-portal.md)
