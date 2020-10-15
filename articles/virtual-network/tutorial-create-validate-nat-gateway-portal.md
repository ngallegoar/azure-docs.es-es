---
title: 'Tutorial: Creación y prueba de una puerta de enlace de NAT: Azure Portal'
titlesuffix: Azure Virtual Network NAT
description: En este tutorial se muestra cómo crear una puerta de enlace de NAT mediante Azure Portal y cómo probar el servicio NAT.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumundD
Customer intent: I want to test a NAT Gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.subservice: nat
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: d798725892a9586c17cd7023863fe5cf7df05cb6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84417844"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-portal-and-test-the-nat-service"></a>Tutorial: Creación de una puerta de enlace de NAT mediante Azure Portal y prueba del servicio NAT

En este tutorial, creará una puerta de enlace de NAT para proporcionar conectividad saliente a las máquinas virtuales de Azure. Para probar la puerta de enlace de NAT, implemente una máquina virtual de origen y destino. La prueba de la puerta de enlace de NAT se realizará mediante conexiones salientes a una dirección IP pública desde la máquina virtual de origen a la de destino.  Para simplificar las cosas, en este tutorial se implementan el origen y el destino en dos redes virtuales diferentes del mismo grupo de recursos.

Si lo prefiere, puede seguir estos pasos con la [CLI de Azure](tutorial-create-validate-nat-gateway-cli.md) o [Azure PowerShell](tutorial-create-validate-nat-gateway-powershell.md) en lugar del portal.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="prepare-the-source-for-outbound-traffic"></a>Preparación del origen para el tráfico saliente

Le guiaremos a través de la configuración de un entorno de prueba completo y la ejecución de las pruebas en los siguientes pasos. Comenzaremos con el origen, que usará el recurso de la puerta de enlace de NAT que se creará posteriormente.

## <a name="virtual-network-and-parameters"></a>Red virtual y parámetros

Antes de implementar una máquina virtual y usar la puerta de enlace de NAT, es necesario crear el grupo de recursos y la red virtual.

En los pasos de esta sección, tendrá que reemplazar los siguientes parámetros por la siguiente información:

| Parámetro                   | Value                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupNAT |
| **\<virtual-network-name>** | myVNetsource          |
| **\<region-name>**          | Este de EE. UU. 2      |
| **\<IPv4-address-space>**   | 192.168.0.0/16          |
| **\<subnet-name>**          | mySubnetsource        |
| **\<subnet-address-range>** | 192.168.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-source-virtual-machine"></a>Creación de la máquina virtual de origen

Ahora se va a crear una máquina virtual para usar el servicio NAT. Esta máquina virtual tiene una dirección IP pública en el nivel de instancia que le permite el acceso a la máquina virtual. El servicio NAT tiene en cuenta la dirección del flujo y reemplazará el destino predeterminado de Internet en la subred. La dirección IP pública de la máquina virtual no se usará para las conexiones salientes.

Para probar la puerta de enlace de NAT, asignaremos un recurso de dirección IP pública como una IP pública de nivel de instancia para acceder a esta máquina virtual desde el exterior. Esta dirección solo se usa para el acceso de prueba.  Se va a demostrar cómo el servicio NAT tiene prioridad sobre otras opciones de salida.

Como ejercicio, también puede crear esta máquina virtual sin una dirección IP pública y crear otra máquina virtual para usarla como Jumpbox sin una dirección IP pública.

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Proceso** > **Ubuntu Server 18.04 LTS**, o busque **Ubuntu Server 18.04 LTS** en el cuadro de búsqueda de Marketplace.

2. En **Crear una máquina virtual**, escriba o seleccione los valores siguientes en la pestaña **Básico**:
   - **Suscripción** > **Grupo de recursos**: Seleccione **myResourceGroupNAT**.
   - **Detalles de instancia** > **Nombre de máquina virtual**: escriba **myVMsource**.
   - **Detalles de instancia** > **Región** > seleccione **Este de EE. UU. 2**.
   - **Cuenta de administrador** > **Autenticación**: Seleccione **Contraseña**.
   - **Cuenta de administrador**> escriba la información de **Nombre de usuario**, **Contraseña** y **Confirmar contraseña**.
   - **Reglas de puerto de entrada** > **Puertos de entrada públicos**: Seleccione **Permitir los puertos seleccionados**.
   - **Reglas de puerto de entrada** > **Seleccionar puertos de entrada**: Seleccione **SSH (22)** .
   - Seleccione la pestaña **Redes** o seleccione **Siguiente: Discos** y, después, **Siguiente: Redes**.

3. En la pestaña **Redes**, asegúrese de que está seleccionado lo siguiente:
   - **Red virtual**: **myVnetsource**
   - **Subred**: **mySubnetsource**
   - **Dirección IP pública** > seleccione **Crear nueva**.  En la ventana **Crear dirección IP pública**, escriba **myPublicIPsourceVM** en el campo **Nombre**. Seleccione **Estándar** para la **SKU**. Deje el resto de valores predeterminados y haga clic en **Aceptar**.
   - **Grupo de seguridad de red de NIC**: Seleccione **Básica**.
   - **Puertos de entrada públicos**: Seleccione **Permitir los puertos seleccionados**.
   - **Seleccionar puertos de entrada**: Confirme que **SSH** está seleccionado.

4. En la pestaña **Administración**, en **Supervisión**, establezca **Diagnósticos de arranque** en **Desactivado**.

5. Seleccione **Revisar + crear**.

6. Revise la configuración y haga clic en **Crear**.

## <a name="create-the-nat-gateway"></a>Creación de la puerta de enlace de NAT

Puede usar uno o varios recursos de dirección IP pública o prefijos de IP pública (o ambos) con la puerta de enlace de NAT. Se agregará un recurso de IP pública, un prefijo de dirección IP pública y un recurso de puerta de enlace de NAT.

En esta sección se detalla cómo crear y configurar los componentes siguientes del servicio NAT mediante el recurso de puerta de enlace de NAT:
  - Un grupo de direcciones IP públicas y un prefijo de dirección IP pública que se usarán en los flujos salientes traducidos por el recurso de puerta de enlace de NAT.
  - Cambie el tiempo de espera de inactividad de 4 (el valor predeterminado) a 10 minutos.

### <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **Dirección IP pública**, o busque **Dirección IP pública** en el cuadro de búsqueda de Marketplace. 

2. En **Crear dirección IP pública**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Versión de la dirección IP | Seleccione **IPv4**.
    | SKU | Seleccione **Estándar**.
    | Nombre | Escriba **myPublicIPsource**. |
    | Suscripción | Seleccione su suscripción.|
    | Resource group | Seleccione **myResourceGroupNAT**. |
    | Location | Seleccione **Este de EE. UU. 2**.|

3. Deje el resto de valores predeterminados y seleccione **Crear**.

### <a name="create-a-public-ip-prefix"></a>Creación de un prefijo de dirección IP pública

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **Prefijo de dirección IP pública**, o busque **Prefijo de dirección IP pública** en el cuadro de búsqueda de Marketplace.

2. En **Crear un prefijo de dirección IP pública**, escriba o seleccione los valores siguientes en la pestaña **Básico**:
   - **Suscripción** > **Grupo de recursos**: Seleccione **myResourceGroupNAT**>.
   - **Detalles de instancia** > **Nombre**: escriba **myPublicIPprefixsource**.
   - **Detalles de instancia** > **Región**: Seleccione **Este de EE. UU. 2**.
   - **Detalles de instancia** > **Tamaño del prefijo**: seleccione **/31 (2 direcciones)** .

3. Deje el resto de valores predeterminados y seleccione **Revisar y crear**.

4. Revise la configuración y, a continuación, seleccione **Crear**.


### <a name="create-a-nat-gateway-resource"></a>Creación de un recurso de puerta de enlace de NAT

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **puerta de enlace de NAT**, o busque **puerta de enlace de NAT** en el cuadro de búsqueda de Marketplace.

2. En **Crear puerta de enlace de traducción de direcciones de red (NAT)** , escriba o seleccione los siguientes valores en la pestaña **Básico**:
   - **Suscripción** > **Grupo de recursos**: Seleccione **myResourceGroupNAT**.
   - **Detalles de instancia** > **Nombre de puerta de enlace de NAT**: escriba **myNATgateway**.
   - **Detalles de instancia** > **Región**: Seleccione **Este de EE. UU. 2**.
   - **Detalles de instancia** > **Tiempo de espera de inactividad (minutos)** : escriba **10**.
   - Seleccione la pestaña **IP pública** o seleccione **Siguiente: IP pública**.

3. En la pestaña **IP pública**, escriba o seleccione los siguientes valores:
   - **Direcciones IP públicas**: Seleccione **myPublicIPsource**.
   - **Prefijos de direcciones IP públicas**: Seleccione **myPublicIPprefixsource**.
   - Seleccione la pestaña **Subred** o seleccione **Siguiente: Subred**.

4. En la pestaña **Subred**, escriba o seleccione los siguientes valores:
   - **Red virtual**: Seleccione **myResourceGroupNAT** > **myVnetsource**.
   - **Nombre de subred**: Active la casilla situada junto a **mySubnetsource**.

5. Seleccione **Revisar + crear**.

6. Revise la configuración y, a continuación, seleccione **Crear**.

Todo el tráfico saliente a destinos de Internet usa ahora el servicio NAT.  No es necesario configurar una UDR.


## <a name="prepare-destination-for-outbound-traffic"></a>Preparación del destino para el tráfico saliente

Ahora se va a crear un destino para el tráfico saliente traducido por el servicio NAT para que pueda probarlo.


## <a name="virtual-network-and-parameters-for-destination"></a>Red virtual y parámetros para el destino

Antes de implementar una máquina virtual como destino, es necesario crear una red virtual donde pueda residir esta máquina. Los siguientes pasos son los mismos que para la máquina virtual de origen con algunos pequeños cambios para exponer el punto de conexión de destino.

En los pasos de esta sección, tendrá que reemplazar los siguientes parámetros por la siguiente información:

| Parámetro                   | Value                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupNAT |
| **\<virtual-network-name>** | myVNetdestination          |
| **\<region-name>**          | Este de EE. UU. 2      |
| **\<IPv4-address-space>**   | 10.1.0.0/16          |
| **\<subnet-name>**          | mySubnetdestination        |
| **\<subnet-address-range>** | 10.1.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-destination-virtual-machine"></a>Creación de la máquina virtual de destino

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Proceso** > **Ubuntu Server 18.04 LTS**, o busque **Ubuntu Server 18.04 LTS** en el cuadro de búsqueda de Marketplace.

2. En **Crear una máquina virtual**, escriba o seleccione los valores siguientes en la pestaña **Básico**:
   - **Suscripción** > **Grupo de recursos**: Seleccione **myResourceGroupNAT**.
   - **Detalles de instancia** > **Nombre de máquina virtual**: escriba **myVMdestination**.
   - **Detalles de instancia** > **Región** > seleccione **Este de EE. UU. 2**.
   - **Cuenta de administrador** > **Autenticación**: Seleccione **Contraseña**.
   - **Cuenta de administrador**> escriba la información de **Nombre de usuario**, **Contraseña** y **Confirmar contraseña**.
   - **Reglas de puerto de entrada** > **Puertos de entrada públicos**: Seleccione **Permitir los puertos seleccionados**.
   - **Reglas de puerto de entrada** > **Seleccionar puertos de entrada**: Seleccione **SSH (22)** y **HTTP (80)** .
   - Seleccione la pestaña **Redes** o seleccione **Siguiente: Discos** y, después, **Siguiente: Redes**.

3. En la pestaña **Redes**, asegúrese de que está seleccionado lo siguiente:
   - **Red virtual**: **myVnetdestination**
   - **Subred**: **mySubnetdestination**
   - **Dirección IP pública** > seleccione **Crear nueva**.  En la ventana **Crear dirección IP pública**, escriba **myPublicIPdestinationVM** en el campo **Nombre**. Seleccione **Estándar** para la **SKU**. Deje el resto de valores predeterminados y haga clic en **Aceptar**.
   - **Grupo de seguridad de red de NIC**: Seleccione **Básica**.
   - **Puertos de entrada públicos**: Seleccione **Permitir los puertos seleccionados**.
   - **Seleccionar puertos de entrada**: Confirme que **SSH** y **HTTP** está seleccionado.

4. En la pestaña **Administración**, en **Supervisión**, establezca **Diagnósticos de arranque** en **Desactivado**.

5. Seleccione **Revisar + crear**.

6. Revise la configuración y, a continuación, seleccione **Crear**.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Preparación de un servidor web y prueba de la carga en la máquina virtual de destino

En primer lugar, es necesario detectar la dirección IP de la máquina virtual de destino. 

1. En el lado izquierdo del portal, seleccione **Grupos de recursos**.
2. Seleccione **myResourceGroupNAT**.
3. Seleccione **myVMdestination**.
4. En **Información general**, copie el valor de **Dirección IP pública** y péguelo en el Bloc de notas para que pueda usarlo para acceder a la máquina virtual.

>[!IMPORTANT]
>Copie la dirección IP pública y, luego, péguela en un bloc de notas para que pueda usarla en pasos posteriores. Indique que esta es la máquina virtual de destino.

### <a name="sign-in-to-destination-vm"></a>Inicio de sesión en la máquina virtual de destino

Abra [Azure Cloud Shell](https://shell.azure.com) en el explorador. Use la dirección IP recuperada en el paso anterior para conectarse a la máquina virtual mediante SSH.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Una vez que haya iniciado sesión, copie y pegue los siguientes comandos.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Estos comandos actualizarán la máquina virtual, instalarán Nginx y crearán un archivo de 100 Kbytes. Este archivo se recuperará de la máquina virtual de origen mediante el servicio NAT.

Cierre la sesión SSH con la máquina virtual de destino.

## <a name="prepare-test-on-source-vm"></a>Preparación de la prueba en la máquina virtual de origen

En primer lugar, es necesario detectar la dirección IP de la máquina virtual de origen.

1. En el lado izquierdo del portal, seleccione **Grupos de recursos**.
2. Seleccione **myResourceGroupNAT**.
3. Seleccione **myVMsource**.
4. En **Información general**, copie el valor de **Dirección IP pública** y péguelo en el Bloc de notas para que pueda usarlo para acceder a la máquina virtual.

>[!IMPORTANT]
>Copie la dirección IP pública y, luego, péguela en un bloc de notas para que pueda usarla en pasos posteriores. Indique que esta es la máquina virtual de origen.

### <a name="log-into-source-vm"></a>Inicio de sesión en la máquina virtual de origen

Abra una nueva pestaña de [Azure Cloud Shell](https://shell.azure.com) en el explorador.  Use la dirección IP recuperada en el paso anterior para conectarse a la máquina virtual mediante SSH. 

```azurecli-interactive
ssh <username>@<ip-address-source>
```

Copie y pegue los siguientes comandos para preparar la prueba del servicio NAT.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Este comando actualizará la máquina virtual, instalará Go, instalará [Hey](https://github.com/rakyll/hey) de GitHub y actualizará el entorno de Shell.

Ahora está listo para probar el servicio NAT.

## <a name="validate-nat-service"></a>Validación del servicio NAT

Mientras ha iniciado sesión en la máquina virtual de origen, puede usar **cURL** y **Hey** para generar solicitudes a la dirección IP de destino.

Use cURL para recuperar el archivo de 100 Kbytes.  Reemplace **\<ip-address-destination>** en el ejemplo siguiente por la dirección IP de destino que copió anteriormente.  El parámetro **--output** indica que se descartará el archivo recuperado.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

También puede generar una serie de solicitudes mediante **Hey**. Una vez más, reemplace **\<ip-address-destination>** por la dirección IP de destino que copió anteriormente.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Este comando generará 100 solicitudes, 10 de ellas a vez, con un tiempo de espera de 30 segundos y sin volver a utilizar la conexión TCP.  Cada solicitud recuperará 100 Kbytes.  Al final de la ejecución, **Hey** notificará algunas estadísticas sobre el rendimiento del servicio NAT.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, la puerta de enlace de NAT y todos los recursos relacionados. Seleccione el grupo de recursos **myResourceGroupNAT** que contiene la puerta de enlace de NAT y, luego, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado una puerta de enlace de NAT y una máquina virtual de origen y destino y, luego, ha probado la puerta de enlace de NAT.

Revise las métricas de Azure Monitor para ver el funcionamiento del servicio NAT. Diagnostique problemas, como el agotamiento de recursos de los puertos SNAT disponibles.  El agotamiento de los recursos de los puertos SNAT se soluciona fácilmente agregando recursos de dirección IP pública o recursos de prefijo de dirección IP pública adicionales (o ambos).

- Obtenga más información sobre [Virtual Network NAT](./nat-overview.md).
- Obtenga más información sobre [recursos de puerta de enlace de NAT](./nat-gateway-resource.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con la CLI de Azure](./quickstart-create-nat-gateway-cli.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con PowerShell](./quickstart-create-nat-gateway-powershell.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con Azure Portal](./quickstart-create-nat-gateway-portal.md).

> [!div class="nextstepaction"]

