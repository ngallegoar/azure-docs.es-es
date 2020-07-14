---
title: 'Tutorial: Protección de un centro virtual mediante Azure Firewall Manager'
description: En este tutorial aprenderá a proteger un centro virtual con Azure Firewall Manager mediante Azure Portal.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: c44daa67b4029c73c57ca82d72ee0a9759dd4c2d
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563662"
---
# <a name="tutorial-secure-your-virtual-hub-using-azure-firewall-manager"></a>Tutorial: Protección de un centro virtual mediante Azure Firewall Manager

Mediante Azure Firewall Manager puede crear centros virtuales protegidos y así proteger el tráfico en la nube destinado a direcciones IP privadas, PaaS de Azure e Internet. El enrutamiento del tráfico al firewall es automático, por lo que no es necesario crear rutas definidas por el usuario (UDR).

![protección de la red en la nube](media/secure-cloud-network/secure-cloud-network.png)

Firewall Manager también admite una arquitectura de red virtual de centro. Para ver una comparación entre los tipos de arquitectura de red virtual de centro y centro virtual protegido, consulte [¿Cuáles son las opciones de arquitectura de Azure Firewall Manager?](vhubs-and-vnets.md)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear la red virtual de tipo hub-and-spoke
> * Crear un centro virtual protegido
> * Conexión de las redes virtuales en estrella tipo hub-and-spoke
> * Creación de una directiva de firewall y protección del centro
> * Enrutamiento del tráfico al centro
> * Probar el firewall

## <a name="create-a-hub-and-spoke-architecture"></a>Creación de una arquitectura en estrella tipo hub-and-spoke

En primer lugar, cree una red virtual de radio donde pueda colocar los servidores.

### <a name="create-a-spoke-virtual-network-and-subnets"></a>Creación de una red virtual y subredes de radio

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En **Redes**, seleccione **Red virtuales**.
2. En **Suscripción**, seleccione la suscripción.
1. En **Grupo de recursos**, seleccione **Crear nuevo**, escriba **FW-Manager** como nombre y seleccione **Aceptar**.
2. En **Nombre**, escriba **Spoke-01**.
3. En **Región**, seleccione **(EE. UU.) Este de EE. UU.** .
4. Seleccione **Siguiente: Direcciones IP**.
1. En **Espacio de direcciones**, acepte el valor **10.0.0.0/16** predeterminado.
3. En **Nombre de subred**, seleccione **predeterminada**.
4. Cambie el nombre de subred a **Workload-SN**.
5. En **Intervalo de direcciones de subred**, escriba **10.0.1.0/24**.
6. Seleccione **Guardar**.

A continuación, cree una subred para un servidor de salto.

1. Haga clic en **Agregar subred**.
4. En **Nombre de subred**, escriba **Jump-SN**.
5. En **Intervalo de direcciones de subred**, escriba **10.0.2.0/24**.
6. Seleccione **Agregar**.

Ahora cree la red virtual.

1. Seleccione **Revisar + crear**.
2. Seleccione **Crear**.

### <a name="create-the-secured-virtual-hub"></a>Creación del centro virtual protegido

Cree el centro virtual protegido con Firewall Manager.

1. En la página principal de Azure Portal, seleccione **Todos los servicios**.
2. En el cuadro de búsqueda, escriba **Firewall Manager** y seleccione **Firewall Manager**.
3. En la página **Firewall Manager**, seleccione **Ver los centros virtuales seguros**.
4. En la página **Firewall Manager | Centros virtuales protegidos**, seleccione **Crear un nuevo centro virtual protegido**.
5. En **Grupo de recursos**, seleccione **FW-Manager**.
7. En **Región**, seleccione **Este de EE. UU.** .
1. En **Nombre del centro virtual protegido**, escriba **Hub-01**.
2. En **Espacio de direcciones del concentrador**, escriba **10.1.0.0/16**.
3. Como nombre de la nueva WAN virtual, escriba **Vwan-01**.
4. Deje desactivada la casilla **Incluir VPN Gateway para habilitar asociados de seguridad de confianza**.
5. Seleccione **Siguiente: Azure Firewall**.
6. Acepte el valor predeterminado **Azure Firewall** **Habilitado** y seleccione **Siguiente: Asociados de seguridad de confianza**.
7. Acepte el valor predeterminado **Asociado de seguridad de confianza** **Deshabilitado** y seleccione **Siguiente: Review + create** (Revisar y crear).
8. Seleccione **Crear**. La implementación tardará unos 30 minutos.

### <a name="connect-the-hub-and-spoke-vnets"></a>Conexión de las redes virtuales en estrella tipo hub-and-spoke

Ahora puede emparejar las redes virtuales en estrella de tipo hub-and-spoke.

1. Seleccione el grupo de recursos **FW-Manager** y, después, seleccione la WAN virtual **Vwan-01**.
2. En **Conectividad**, seleccione **Conexiones de red virtual**.
3. Seleccione **Agregar conexión**.
4. En **Nombre de conexión**, escriba **hub-spoke**.
5. En **Centros**, seleccione **Hub-01**.
6. En **Grupo de recursos**, seleccione **FW-Manager**.
7. En **Red virtual**, seleccione **Spoke-01**.
8. Seleccione **Crear**.

## <a name="create-a-firewall-policy-and-secure-your-hub"></a>Creación de una directiva de firewall y protección del centro

Una directiva de firewall define colecciones de reglas para dirigir el tráfico en uno o varios centros virtuales protegidos. Creará la directiva de firewall y, a continuación, protegerá el centro.

1. En Firewall Manager, seleccione **Ver las directivas de Azure Firewall**.
2. Seleccione **Crear una directiva de Azure Firewall**.
3. En **Detalles de la directiva**, como **Nombre** escriba **Policy-01** y como **Región** seleccione **Este de EE. UU.**
4. Seleccione **Siguiente: Reglas**.
5. En la pestaña **Reglas**, seleccione **Agregar una colección de reglas**.
6. En la página **Agregar una colección de reglas**, escriba **RC-01** en **Nombre**.
7. En **Tipo de colección de reglas**, seleccione **Aplicación**.
8. En **Prioridad**, escriba **100**.
9. Asegúrese de que el valor de **Rule collection action** (Acción de la colección de reglas) es **Permitir**.
10. Como **Nombre** de la regla, escriba **Allow-msft**.
11. Para **Tipo de origen**, seleccione **Dirección IP**.
12. Para **Origen**, escriba **\*** .
13. En **Protocolo**, escriba **http,https**.
14. Asegúrese de que **Tipo de destino** es **FQDN**.
15. En **Destino**, escriba **\*.microsoft.com**.
16. Seleccione **Agregar**.
17. Seleccione **Siguiente: Inteligencia sobre amenazas**.
18. Seleccione **Siguiente: Centros de conectividad**.
19. En la pestaña **Centros de conectividad**, seleccione **Asociar centros virtuales**.
20. Seleccione **Hub-01** y, luego, seleccione **Agregar**.
21. Seleccione **Revisar + crear**.
22. Seleccione **Crear**.

Esta acción puede tardar unos cinco minutos o más en finalizar.

## <a name="route-traffic-to-your-hub"></a>Enrutamiento del tráfico al centro

Ahora debe asegurarse de que el tráfico de red se enruta a través del firewall.

1. En Firewall Manager, seleccione **Centros virtuales protegidos**.
2. Seleccione **Hub-01**.
3. En **Ajustes**, seleccione **Configuración de seguridad**.
4. En **Tráfico de Internet**, seleccione **Azure Firewall**.
5. En **Private traffic** (Tráfico privado), seleccione **Send via Azure Firewall** (Enviar a través de Azure Firewall).
10. Compruebe que la conexión **hub-spoke** muestra **Tráfico de Internet** como **Protegido**.
11. Seleccione **Guardar**.


## <a name="test-your-firewall"></a>Prueba del firewall

Para probar las reglas de firewall, debe implementar un par de servidores. Implementará Workload-Srv en la subred Workload-SN para probar las reglas de firewall y Jump-Srv para poder usar Escritorio remoto para conectarse desde Internet y, a continuación, conectarse a Workload-Srv.

### <a name="deploy-the-servers"></a>Implementación de los servidores

1. En Azure Portal, seleccione **Crear un recurso**.
2. Seleccione **Windows Server 2016 Datacenter** en la lista **Popular**.
3. Especifique estos valores para la máquina virtual:

   |Configuración  |Value  |
   |---------|---------|
   |Resource group     |**FW-Manager**|
   |Nombre de la máquina virtual     |**Jump-Srv**|
   |Region     |**(EE. UU.) Este de EE. UU.**|
   |Nombre de usuario del administrador     |Escriba un nombre de usuario|
   |Contraseña     |Escriba una contraseña|

4. En **Reglas de puerto de entrada**, en **Puertos de entrada públicos**, seleccione **Permitir los puertos seleccionados**.
5. En **Seleccionar puertos de entrada**, seleccione **RDP (3389)** .
6. Acepte los restantes valores predeterminados y seleccione **Siguiente: Discos**.
7. Acepte los valores predeterminados del disco y seleccione **Siguiente: Redes**.
8. Asegúrese de que **Spoke-01** está seleccionada como red virtual y de que la subred es **Jump-SN**.
9. En **IP pública**, acepte el nombre predeterminado de la nueva dirección IP pública (Jump-Srv-ip).
11. Acepte los restantes valores predeterminados y seleccione **Siguiente: Administración**.
12. Seleccione **Desactivar** para deshabilitar los diagnósticos de arranque. Acepte los restantes valores predeterminados y seleccione **Revisar y crear**.
13. Revise la configuración en la página de resumen y seleccione **Crear**.

Use la información de la tabla siguiente para configurar otra máquina virtual llamada **Workload-Srv**. El resto de la configuración es la misma que la de la máquina virtual Srv-Jump.

|Configuración  |Value  |
|---------|---------|
|Subnet|**Workload-SN**|
|Dirección IP pública|**None**|
|Puertos de entrada públicos|**None**|

### <a name="add-a-route-table-and-default-route"></a>Incorporación de una tabla de rutas y una ruta predeterminada

Para permitir una conexión de Internet a Jump-Srv, debe crear una tabla de rutas y una ruta de puerta de enlace predeterminada a Internet desde la subred **Jump-SN**.

1. En Azure Portal, seleccione **Crear un recurso**.
2. Escriba **tabla de rutas** en el cuadro de búsqueda y, a continuación, seleccione **Tabla de rutas**.
3. Seleccione **Crear**.
4. Escriba **RT-01** en **Nombre**.
5. Seleccione la suscripción, el grupo de recursos **FW-Manager** y la región **(EE. UU.) Este de EE. UU.**
6. Seleccione **Crear**.
7. Una vez finalizada la implementación, seleccione la tabla de rutas **RT-01**.
8. Seleccione **Rutas** y después **Agregar**.
9. Escriba **jump-to-inet** como **Nombre de ruta**.
10. Escriba **0.0.0.0/0** como **Prefijo de dirección**.
11. Seleccione **Internet** como **Tipo del próximo salto**.
12. Seleccione **Aceptar**.
13. Una vez finalizada la implementación, seleccione **Subredes** y, a continuación, seleccione **Asociar**.
14. Seleccione **Spoke-01** como **Red virtual**.
15. Seleccione **Jump-SN** como **Subred**.
16. Seleccione **Aceptar**.

### <a name="test-the-rules"></a>Prueba de las reglas

Ahora, pruebe las reglas de firewall para confirmar que funcionan según lo previsto.

1. En Azure Portal, revise la configuración de red de la máquina virtual **Workload-Srv** y anote la dirección IP privada.
2. Conecte un escritorio remoto a la máquina virtual **Jump-Srv** e inicie sesión. Desde ahí, abra una conexión del escritorio remoto a la dirección IP privada de **Workload-Srv**.

3. Abra Internet Explorer y vaya a https://www.microsoft.com.
4. Seleccione **Aceptar** > **Cerrar** en las alertas de seguridad de Internet Explorer.

   Debería ver la página principal de Microsoft.

5. Vaya a https://www.google.com.

   El firewall debería bloquearle.

Con ello, ha comprobado que las reglas de firewall funcionan:

* Puede navegar al FQDN permitido pero no a ningún otro.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre los asociados de seguridad de confianza](trusted-security-partners.md)
