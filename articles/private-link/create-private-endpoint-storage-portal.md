---
title: Conexión privada a una cuenta de almacenamiento mediante el punto de conexión privado de Azure
description: Aprenda cómo conectarse de forma privada a una cuenta de almacenamiento en Azure mediante un punto de conexión privado.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 111e6e2f80c3460f363c496b7b32befdca16250d
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115109"
---
# <a name="connect-privately-to-a-storage-account-using-azure-private-endpoint"></a>Conexión privada a una cuenta de almacenamiento mediante el punto de conexión privado de Azure
Un punto de conexión privado de Azure es el bloque de creación fundamental para Private Link en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera privada con recursos de Private Link.

En este inicio rápido, obtendrá información sobre cómo crear una máquina virtual en una red virtual de Azure, una cuenta de almacenamiento con un punto de conexión privado mediante Azure Portal. Después, puede acceder de forma segura a la cuenta de almacenamiento desde la VM.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-vm"></a>Crear una VM
En esta sección, va a crear una red virtual y la subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link (una cuenta de almacenamiento en este ejemplo).

## <a name="virtual-network-and-parameters"></a>Red virtual y parámetros

En esta sección, va a crear una red virtual y una subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link.

En los pasos de esta sección, tendrá que reemplazar los siguientes parámetros por la siguiente información:

| Parámetro                   | Value                |
|-----------------------------|----------------------|
| **\<nombre-de-grupo-recursos>**  | myResourceGroup |
| **\<virtual-network-name>** | myVirtualNetwork          |
| **\<region-name>**          | Centro-Oeste de EE. UU.      |
| **\<espacio-de-direcciones-IPv4>**   | 10.1.0.0\16          |
| **\<nombre-de-subred>**          | mySubnet        |
| **\<subnet-address-range>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]


### <a name="create-virtual-machine"></a>Crear máquina virtual

1. En la parte superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Proceso** > **Máquina virtual**.

1. En **Creación de una máquina virtual: conceptos básicos**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.  |
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la máquina virtual | Escriba *myVm*. |
    | Region | Seleccione **WestCentralUS**. |
    | Opciones de disponibilidad | Deje el valor predeterminado **No se requiere redundancia de la infraestructura**. |
    | Imagen | Seleccione **Windows Server 2019 Datacenter**. |
    | Size | Deje el valor predeterminado **Estándar DS1 v2**. |
    | **CUENTA DE ADMINISTRADOR** |  |
    | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirm Password | Vuelva a escribir la contraseña. |
    | **REGLAS DE PUERTO DE ENTRADA** |  |
    | Puertos de entrada públicos | Deje el valor predeterminado **Ninguno**. |
    | **AHORRE DINERO** |  |
    | ¿Ya tiene una licencia de Windows? | Deje el valor predeterminado **No**. |
    |||

1. Seleccione **Siguiente: Discos**.

1. En **Creación de una máquina virtual: Discos**, deje los valores predeterminados y seleccione **Siguiente: Redes**.

1. En **Creación de una máquina virtual: Redes**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Virtual network | Deje el valor predeterminado **MyVirtualNetwork**.  |
    | Espacio de direcciones | Deje el valor predeterminado **10.1.0.0/24**.|
    | Subnet | Deje el valor predeterminado **mySubnet (10.1.0.0/24)** .|
    | Dirección IP pública | Deje el valor predeterminado **(new) myVm-ip**. |
    | Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
    | Selección de puertos de entrada | Seleccione **HTTP** y **RDP**.|
    ||

1. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración.

1. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

## <a name="create-your-private-endpoint"></a>Creación de un punto de conexión privado
En esta sección, creará una cuenta de almacenamiento privada con un punto de conexión privado para ella. 

1. En la parte superior izquierda de la pantalla en Azure Portal, seleccione **Crear un recurso** > **Almacenamiento** > **Cuenta de almacenamiento**.

1. En **Crear cuenta de almacenamiento: Aspectos básicos**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.|
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la cuenta de almacenamiento  | Escriba *mystorageaccount*. Si el nombre ya existe, cree uno único. |
    | Region | Seleccione **WestCentralUS**. |
    | Rendimiento| Deje el valor predeterminado **Estándar**. |
    | Tipo de cuenta | Deje el valor predeterminado **Storage (uso general v2)** . |
    | Replicación | Seleccione **Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)** . |
    |||
  
3. Seleccione **Siguiente: Redes**.
4. En **Crear una cuenta de almacenamiento: Redes**, método de conectividad, seleccione **Punto de conexión privado**.
5. En **Crear una cuenta de almacenamiento: Redes**, seleccione **Agregar un punto de conexión privado**. 
6. En **Crear un punto de conexión privado**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.|
    |Location|Seleccione **WestCentralUS**.|
    |Nombre|Escriba *myPrivateEndpoint*.  |
    |Recurso secundario de almacenamiento|Deje el valor predeterminado **Blob**. |
    | **REDES** |  |
    | Virtual network  | Seleccione *MyVirtualNetwork* en el grupo de recursos *myResourceGroup*. |
    | Subnet | Seleccione *mySubnet*. |
    | **INTEGRACIÓN DE DNS PRIVADO**|  |
    | Integración con una zona DNS privada  | Deje el valor predeterminado **Sí**. |
    | Zona DNS privada  | Deje el valor predeterminado **(New) privatelink.blob.core.windows.net**. |
    |||
7. Seleccione **Aceptar**. 
8. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración. 
9. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**. 
10. Vaya a la cuenta de almacenamiento que acaba de crear.
11. Seleccione **Claves de acceso** en el menú de contenido de la izquierda.
12. Seleccione **Copiar** en la cadena de conexión para key1.
 
## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Conéctese a la máquina virtual *myVm* desde Internet de la siguiente manera:

1. En la barra de búsqueda del portal, escriba *myVm*.

1. Seleccione el botón **Conectar**. Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

1. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo *downloaded.rdp*.

    1. Cuando se le pida, seleccione **Conectar**.

    1. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

        > [!NOTE]
        > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

1. Seleccione **Aceptar**.

1. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

1. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.  

## <a name="access-storage-account-privately-from-the-vm"></a>Acceso a la cuenta de almacenamiento de forma privada desde la máquina virtual

En esta sección, se conectará de manera privada a la cuenta de almacenamiento mediante el punto de conexión privado.

1. En el Escritorio remoto de *myVm*, abra PowerShell.
2. Escriba `nslookup mystorageaccount.blob.core.windows.net`. Recibirá un mensaje similar a este:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mystorageaccount123123.privatelink.blob.core.windows.net
    Address:  10.0.0.5
    Aliases:  mystorageaccount.blob.core.windows.net
    ```
3. Instale [Explorador de Microsoft Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).
4. Seleccione **Cuentas de almacenamiento** con el botón secundario.
5. Seleccione **Conectar a Azure Storage**.
6. Seleccione **Usar una cadena de conexión**.
7. Seleccione **Next** (Siguiente).
8. Escriba la cadena de conexión pegando la información copiada anteriormente.
9. Seleccione **Next** (Siguiente).
10. Seleccione **Conectar**.
11. Examinar los contenedores de blobs desde mystorageaccount 
12. Opcionalmente, cree carpetas o cargue archivos en *mystorageaccount* . 
13. Cierre la conexión de Escritorio remoto a *myVM*. 

Opciones adicionales para acceder a la cuenta de almacenamiento:
- El Explorador de Microsoft Azure Storage es una aplicación independiente y gratuita de Microsoft que permite trabajar visualmente con los datos de Azure Storage en Windows, macOS y Linux. Puede instalar la aplicación para examinar de forma privada el contenido de la cuenta de almacenamiento. 
 
- La utilidad AzCopy es otra opción para la transferencia de datos que permiten ejecutar scripts de alto rendimiento para Azure Storage. Use AzCopy para transferir datos a y desde Blob Storage, File Storage y Table Storage. 


## <a name="clean-up-resources"></a>Limpieza de recursos 
Cuando haya terminado de usar el punto de conexión privado, la cuenta de almacenamiento y la máquina virtual, elimine el grupo de recursos y todos los recursos que contiene: 
1. Escriba *myResourceGroup* en el cuadro **Buscar** de la parte superior del portal y seleccione *myResourceGroup* en los resultados de búsqueda. 
2. Seleccione **Eliminar grupo de recursos**. 
3. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes
En este inicio rápido, ha creado una máquina virtual en una red virtual, una cuenta de almacenamiento y un punto de conexión privado. Se ha conectado a una máquina virtual desde Internet y se ha comunicado de forma segura con la cuenta de almacenamiento mediante el vínculo privado. Para más información sobre los puntos de conexión privados, vea [¿Qué es un punto de conexión privado de Azure?](private-endpoint-overview.md).
