---
title: Creación de una infraestructura para un clúster en máquinas virtuales de Azure
description: En este tutorial aprenderá a configurar la infraestructura de una máquina virtual de Azure para ejecutar un clúster de Service Fabric.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: e9f62f944fff331bcf2dad1b380161e563614219
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90561847"
---
# <a name="tutorial-create-azure-vm-infrastructure-to-host-a-service-fabric-cluster"></a>Tutorial: Creación de la infraestructura de una máquina virtual de Azure para hospedar un clúster de Service Fabric

Los clústeres de Service Fabric independientes ofrecen la opción de elegir un entorno propio y crear un clúster como parte del enfoque "Cualquier sistema operativo, cualquier nube" que toma Service Fabric. En esta serie de tutoriales se crea un clúster independiente hospedado en máquinas virtuales de Azure y se instala en él una aplicación.

Este tutorial es la primera parte de una serie. En este artículo se generan los recursos de máquina virtual de Azure necesarios para hospedar el clúster de Service Fabric independiente. En futuros artículos deberá instalar el conjunto de Service Fabric independiente, una aplicación de ejemplo en el clúster y, por último, limpiarlo.

En la primera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Crear un conjunto de instancias de máquina virtual de Azure
> * Modificar el grupo de seguridad
> * Iniciar sesión en una de las instancias
> * Preparar la instancia para Service Fabric

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesitará una suscripción de Azure.  Si aún no tiene una, vaya a [Azure Portal](https://portal.azure.com) para crearla.

## <a name="create-azure-virtual-machine-instances"></a>Creación de instancias de máquina virtual de Azure

1. Inicie sesión en Azure Portal y seleccione **Máquinas virtuales** (no Máquinas virtuales [clásico]).

   ![Máquina virtual de Azure Portal][az-console]

2. Seleccione el botón **Agregar** para abrir el formulario **Crear una máquina virtual**.

3. En la pestaña **Aspectos básicos**, debe asegurarse de elegir la suscripción y el grupo de recursos que quiera (se recomienda usar un nuevo grupo de recursos).

4. Cambie el tipo de **imagen** a **Windows Server 2016 Datacenter**. 
 
5. Cambie el **tamaño** de la instancia a **Estándar DS2 v2**. Establezca un **nombre de usuario** y una **contraseña** de administrador y anote cuáles son.

6. Deje las **Reglas de puerto de entrada** bloqueadas por el momento, ya que se configurarán en la siguiente sección.

7. En la pestaña **Redes**, cree una nueva **Red virtual** y anote su nombre.

8. A continuación, establezca el **Grupo de seguridad de red de NIC** en **Avanzado**. Cree un nuevo grupo de seguridad, anote su nombre y cree las siguientes reglas para permitir el tráfico TCP de cualquier origen:

   ![Captura de pantalla que muestra la creación de reglas para permitir el tráfico TCP entrante.][sf-inbound]

   * Puerto `3389`, para RDP e ICMP (conectividad básica).
   * Puertos `19000-19003`, para Service Fabric.
   * Puertos `19080-19081`, para Service Fabric.
   * Puerto `8080`, para las solicitudes del explorador web.

   > [!TIP]
   > Para conectar las máquinas virtuales juntas en Service Fabric, las que hospedan la infraestructura deben tener las mismas credenciales.  Hay dos formas habituales de obtener credenciales coherentes: unirlas todas al mismo dominio o establecer la misma contraseña de administrador en todas las máquinas virtuales. Afortunadamente, Azure permite que todas las máquinas virtuales en la misma **Red virtual** se conecten fácilmente, por lo que nos aseguraremos de que todas las instancias estén en la misma red.

9. Agregue otra regla. Establezca el origen como **etiqueta de servicio** y establezca la etiqueta de servicio de origen en **VirtualNetwork**. Service Fabric necesita que los siguientes puertos estén abiertos para la comunicación dentro del clúster: 135,137-139,445,20001-20031,20606-20861.

   ![Captura de pantalla que muestra la creación de reglas para permitir el tráfico TCP para un clúster.][vnet-inbound]

10. El resto de las opciones son aceptables en su estado predeterminado. Si quiere, puede revisarlas e iniciar la máquina virtual después.

## <a name="creating-more-instances-for-your-service-fabric-cluster"></a>Creación de más instancias para su clúster de Service Fabric

Inicie dos **máquinas virtuales** más y asegúrese de mantener la misma configuración que se describe en la sección anterior. En concreto, mantenga el mismo nombre de usuario de administrador y contraseña. La **red virtual** y el **grupo de seguridad de red de NIC** no deben volverse a crear; seleccione los que ya creó desde el menú desplegable. La implementación de sus instancias puede tardar unos minutos.

## <a name="connect-to-your-instances"></a>Conexión a sus instancias

1. Seleccione una de las instancias de la sección **Máquina virtual**.

2. En la pestaña **Resumen**, tome nota de la dirección IP *privada*. A continuación, haga clic en **Conectar**.

3. En la pestaña **RDP**, observe que estamos usando la dirección IP pública y el puerto 3389, que abrimos de forma específica anteriormente. Descargue el archivo RDP.
 
4. Abra el archivo RDP y, cuando se le solicite, escriba el nombre de usuario y la contraseña que proporcionó en la configuración de la máquina virtual.

5. Cuando esté conectado a una instancia, debe asegurarse de que el registro remoto se encuentre en ejecución, habilitar SMB y abrir los puertos necesarios para SMB y el registro remoto.

   Para habilitar SMB, use este comando de PowerShell:

   ```powershell
   netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
   ```

6. Para abrir los puertos en el firewall, este es el comando de PowerShell:

   ```powershell
   New-NetFirewallRule -DisplayName "Service Fabric Ports" -Direction Inbound -Action Allow -RemoteAddress LocalSubnet -Protocol TCP -LocalPort 135, 137-139, 445
   ```

7. Repita este proceso para las demás instancias, y no olvide anotar las direcciones IP privadas.

## <a name="verify-your-settings"></a>Comprobación de la configuración

1. Para validar la conectividad básica, conéctese a una de las máquinas virtuales mediante RDP.

2. Abra el **símbolo del sistema** desde esa máquina virtual. A continuación, use el comando ping para conectar una máquina virtual a otra y reemplace la dirección IP siguiente con una de las direcciones IP privadas que anotó anteriormente (no use la dirección IP de la máquina virtual a la que está conectado).

   ```
   ping 172.31.20.163
   ```

   Si la salida es similar a `Reply from 172.31.20.163: bytes=32 time<1ms TTL=128` repetida cuatro veces, la conexión entre las instancias funciona.

3. Ahora, asegúrese de que el recurso compartido de SMB funciona con el comando siguiente:

   ```
   net use * \\172.31.20.163\c$
   ```

   Debe devolver `Drive Z: is now connected to \\172.31.20.163\c$.` como salida.


   Ahora las instancias están configuradas correctamente para Service Fabric.

## <a name="next-steps"></a>Pasos siguientes

En la primera parte de la serie aprendió a iniciar tres instancias de máquina virtual de Azure y a configurarlas para la instalación de Service Fabric:

> [!div class="checklist"]
> * Crear un conjunto de instancias de máquina virtual de Azure
> * Modificar el grupo de seguridad
> * Iniciar sesión en una de las instancias
> * Preparar la instancia para Service Fabric

Avance a la segunda parte de la serie para configurar Service Fabric en el clúster.

> [!div class="nextstepaction"]
> [Instalación de Service Fabric](service-fabric-tutorial-standalone-create-service-fabric-cluster.md)

<!-- IMAGES -->
[az-console]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/az-console.png
[sf-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/sf-inbound.png
[vnet-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/vnet-inbound.png
