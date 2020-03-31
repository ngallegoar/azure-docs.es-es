---
title: Conexión a una cuenta de Azure Cosmos con Azure Private Link
description: Aprenda a acceder de forma segura a la cuenta de Azure Cosmos desde una máquina virtual mediante la creación de un punto de conexión privado.
author: malopMSFT
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: allensu
ms.openlocfilehash: b7a50a2dabc9503ca5dbdd3388e29cfc69963885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252598"
---
# <a name="connect-privately-to-an-azure-cosmos-account-using-azure-private-link"></a>Conexión privada a una cuenta de Azure Cosmos con Azure Private Link

Un punto de conexión privado de Azure es el bloque de creación fundamental para Private Link en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera privada con recursos de Private Link.

En este artículo, obtendrá información sobre cómo crear una máquina virtual en una red virtual de Azure, una cuenta de Azure Cosmos con un punto de conexión privado mediante Azure Portal. Después, puede acceder de forma segura a la cuenta de Azure Cosmos desde la VM.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-a-vm"></a>Crear una VM

## <a name="virtual-network-and-parameters"></a>Red virtual y parámetros

En esta sección, creará una red virtual y la subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link (una cuenta de Azure Cosmos en este ejemplo).

En esta sección, tendrá que reemplazar los siguientes parámetros de los pasos por la información siguiente:

| Parámetro                   | Value                |
|-----------------------------|----------------------|
| **\<nombre-de-grupo-recursos>**  | myResourceGroup|
| **\<virtual-network-name>** | myVirtualNetwork         |
| **\<region-name>**          | Centro-Oeste de EE. UU.     |
| **\<espacio-de-direcciones-IPv4>**   | 10.1.0.0\16          |
| **\<nombre-de-subred>**          | mySubnet        |
| **\<subnet-address-range>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-the-virtual-machine"></a>Creación de la máquina virtual

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
    | Confirm Password | Reescriba la contraseña. |
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

## <a name="create-an-azure-cosmos-account"></a>Creación de una cuenta de Azure Cosmos

Cree una [cuenta de Azure Cosmos SQL API](../cosmos-db/create-cosmosdb-resources-portal.md#create-an-azure-cosmos-db-account). Para simplificar, puede crear la cuenta de Azure Cosmos en la misma región que los demás recursos (o sea, "WestCentralUS").

## <a name="create-a-private-endpoint-for-your-azure-cosmos-account"></a>Creación de un punto de conexión privado para la cuenta de Azure Cosmos

Cree un vínculo privado para su cuenta de Azure Cosmos, tal como se describe en la sección [Creación de un vínculo privado con Azure Portal](../cosmos-db/how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-the-azure-portal) del artículo vinculado.

## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Conéctese a la máquina virtual *myVm* desde Internet de la siguiente manera:

1. En la barra de búsqueda del portal, escriba *myVm*.

1. Seleccione el botón **Conectar**. Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

1. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo *.rdp* descargado.

    1. Cuando se le pida, seleccione **Conectar**.

    1. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

        > [!NOTE]
        > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

1. Seleccione **Aceptar**.

1. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

1. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.  

## <a name="access-the-azure-cosmos-account-privately-from-the-vm"></a>Acceso a la cuenta de Azure Cosmos de forma privada desde la máquina virtual

En esta sección, se conectará de forma privada a la cuenta de Azure Cosmos mediante el punto de conexión privado. 

1. Para incluir la dirección IP y la asignación de DNS, inicie sesión en la máquina virtual *myVM*, abra el archivo `c:\Windows\System32\Drivers\etc\hosts` e incluya la información de DNS del paso anterior en el siguiente formato:

   [Dirección IP privada] [Punto de conexión de la cuenta].documents.azure.com

   **Ejemplo**:

   10.1.255.13 mycosmosaccount.documents.azure.com

   10.1.255.14 mycosmosaccount-eastus.documents.azure.com


1. En el Escritorio remoto de *myVM*, instale [Explorador de Microsoft Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).

1. Elija **Cuentas de Cosmos DB (vista preliminar)** con el botón secundario.

1. Seleccione **Conectarse a Cosmos DB**.

1. Seleccione **API**.

1. Escriba la cadena de conexión pegando la información copiada anteriormente.

1. Seleccione **Next** (Siguiente).

1. Seleccione **Conectar**.

1. Examine los contenedores y las bases de datos de Azure Cosmos desde *mycosmosaccount*.

1. (Opcionalmente) agregue nuevos elementos a *mycosmosaccount*.

1. Cierre la conexión de Escritorio remoto a *myVM*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar el punto de conexión privado, la cuenta de Azure Cosmos y la máquina virtual, elimine el grupo de recursos y todos los recursos que contiene: 

1. Escriba *myResourceGroup* en el cuadro **Buscar** de la parte superior del portal y seleccione *myResourceGroup* en los resultados de búsqueda.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, creó una máquina virtual en una red virtual, una cuenta de Azure Cosmos y un punto de conexión privado. Se conectó a una máquina virtual desde Internet y se comunicó de forma segura con la cuenta de Azure Cosmos con el vínculo privado.

* Para más información sobre los puntos de conexión privados, vea [¿Qué es un punto de conexión privado de Azure?](private-endpoint-overview.md).

* Para más información sobre la limitación del punto de conexión privado al usar Azure Cosmos DB, consulte el artículo [Vínculo privado de Azure con Azure Cosmos DB](../cosmos-db/how-to-configure-private-endpoints.md).
