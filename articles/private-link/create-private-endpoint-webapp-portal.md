---
title: Conexión privada a una aplicación web mediante un punto de conexión privado de Azure (versión preliminar)
description: En este artículo se explica cómo conectarse de forma privada a una aplicación web mediante un punto de conexión privado de Azure (versión preliminar).
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: how-to
ms.date: 09/08/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: ccbcdbe9204120e1cf181136f566556ec30be871
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90054541"
---
# <a name="connect-privately-to-a-web-app-by-using-azure-private-endpoint-preview"></a>Conexión privada a una aplicación web mediante un punto de conexión privado de Azure (versión preliminar)

Un punto de conexión privado de Azure (versión preliminar) es el bloque de creación fundamental de Azure Private Link. Con un punto de conexión privado, puede conectarse de forma privada a la aplicación web. En este artículo, aprenderá a implementar una aplicación web mediante un punto de conexión privado y, después, a conectarse a ella desde una máquina virtual (VM).

Para más información, consulte [Uso de puntos de conexión privados para una aplicación web de Azure][privateendpointwebapp].

> [!Note]
> El punto de conexión privado (versión preliminar) está disponible en regiones públicas para las aplicaciones web de Windows de nivel PremiumV2, las aplicaciones web de Linux y el plan de Azure Functions Premium (a veces denominado Elástico Premium). 

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Antes de comenzar, inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-network-and-virtual-machine"></a>Creación de una red virtual y una máquina virtual

En esta sección, creará una red virtual y una subred para hospedar una máquina virtual que se usará para acceder a una aplicación web mediante un punto de conexión privado.

### <a name="create-the-virtual-network"></a>Crear la red virtual

Para crear la red virtual y la subred, haga lo siguiente:

1. En el panel izquierdo, seleccione **Crear un recurso** > **Redes** > **Red virtual**.

1. En el panel **Crear red virtual**, seleccione la pestaña **Aspectos básicos** y, luego, escriba la información que se muestra aquí:

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del panel "Crear red virtual" en Azure Portal.][1]

1. Seleccione la pestaña **Direcciones IP** y escriba la información que se muestra aquí:

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de la pestaña "Direcciones IP" del panel Crear red virtual.][2]

1. En la sección **Subred**, seleccione **Agregar subred**, escriba la información que se muestra aquí y, luego, seleccione **Agregar**.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del panel "Agregar subred".][3]

1. Seleccione **Revisar + crear**.

1. Tras una validación correcta, seleccione **Crear**.

### <a name="create-the-virtual-machine"></a>Creación de la máquina virtual

Para conectarse a la máquina virtual, haga lo siguiente:

1. En Azure Portal, en el panel izquierdo, seleccione **Crear un recurso** > **Proceso** > **Máquina virtual**.

1. En el panel **Crear una máquina virtual: Aspectos básicos**, escriba la información que se muestra aquí:

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del panel "Crear una máquina virtual".][4]

1. Seleccione **Siguiente: Discos**.

1. En el panel **Discos**, mantenga la configuración predeterminada y, luego, seleccione **Siguiente: Redes**.

1. En el panel **Redes**, escriba la información que se muestra aquí:

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de la pestaña "Redes" del panel "Crear una máquina virtual".][5]

1. Seleccione **Revisar + crear**.

1. Tras una validación correcta, seleccione **Crear**.

## <a name="create-a-web-app-and-a-private-endpoint"></a>Creación de una aplicación web y un punto de conexión privado

En esta sección, creará una aplicación web privada que usa un punto de conexión privado.

> [!Note]
> La característica de punto de conexión privado solo está disponible para el nivel PremiumV2.

### <a name="create-the-web-app"></a>Creación de la aplicación web

1. En Azure Portal, en el panel izquierdo, seleccione **Crear un recurso** > **Web** > **Aplicación web**.

1. En el panel **Aplicación web**, seleccione la pestaña **Aspectos básicos** y, luego, escriba la información que se muestra aquí:

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de la pestaña "Aspectos básicos" del panel "Aplicación web".][6]

1. Seleccione **Revisar + crear**.

1. Tras una validación correcta, seleccione **Crear**.

### <a name="create-the-private-endpoint"></a>Creación del punto de conexión privado

1. En las propiedades de la aplicación web, en **Configuración**, seleccione **Redes** y, luego, en **Conexiones de punto de conexión privado (versión preliminar)** , elija **Configurar las conexiones de punto de conexión privado**.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del vínculo "Configurar las conexiones de punto de conexión privado" en el panel Redes de la aplicación web.][7]

1. En el asistente **Conexiones de puntos de conexión privadas (versión preliminar)** , seleccione **Agregar**.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del botón Agregar en el asistente "Conexiones de puntos de conexión privadas (versión preliminar)".][8]

1. Seleccione la información correcta en las listas desplegables **Suscripción**, **Red virtual** y **Subred** y, luego, elija **Aceptar**.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del panel "Agregar punto de conexión privado (versión preliminar)".][9]

1. Supervise el progreso de la creación del punto de conexión privado.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del progreso de agregar el punto de conexión privado.][10]
   > ![Captura de pantalla del punto de conexión privado recién creado.][11]

## <a name="connect-to-the-vm-from-the-internet"></a>Conexión a la máquina virtual desde Internet

1. En el cuadro de **búsqueda** de Azure Portal, escriba **myVm**.
1. Seleccione **Conectar** y, después, **RDP**.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del botón "RDP" en el panel "myVM".][12]

1. En el panel **Connect with RDP** (Conectar con RDP), seleccione **Descargar archivo RDP**.  

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del botón "Descargar archivo RDP" en el panel "Connect with RDP" (Conectar con RDP).][13]

   Azure crea un archivo de Protocolo de Escritorio remoto (RDP) y lo descarga en su equipo.   

1. Abra el archivo RDP descargado.

   a. En el símbolo del sistema, seleccione **Conectar**.  
   b. Escriba el nombre de usuario y la contraseña que especificó cuando creó la máquina virtual.

     > [!Note]
     > Para usar estas credenciales, es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta**.

1. Seleccione **Aceptar**.

   > [!Note]
   > Si recibe una advertencia de certificado durante el proceso de inicio de sesión, seleccione **Sí** o **Continuar**.

1. Cuando aparezca la ventana del escritorio de la máquina virtual, minimícela para volver al escritorio local.

## <a name="access-the-web-app-privately-from-the-vm"></a>Acceso a la aplicación web de forma privada desde la máquina virtual

En esta sección, se conectará de manera privada a la aplicación web mediante el punto de conexión privado.

1. Para obtener la dirección IP del punto de conexión privado, en el cuadro de **búsqueda**, escriba **vínculo privado** y, luego, en la lista de resultados, seleccione **Private Link**.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del vínculo "Private Link" en la lista de resultados de búsqueda.][14]

1. En Private Link Center, en el panel izquierdo, seleccione **Puntos de conexión privados** para mostrar los puntos de conexión privados.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de la lista de puntos de conexión privados en Private Link Center.][15]

1. Seleccione el punto de conexión privado que vincula a la aplicación web y la subred.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla del panel de propiedades de un punto de conexión privado.][16]

1. Copie la dirección IP privada del punto de conexión privado y el nombre de dominio completo (FQDN) de la aplicación web. En el ejemplo anterior, el identificador privado es *`webappdemope.azurewebsites.net 10.10.2.4`* .

1. En el panel **myVM**, compruebe que la aplicación web no es accesible a través de la dirección IP pública. Para ello, abra un explorador y pegue el nombre de la aplicación web. La página debe mostrar el mensaje "Error 403: Prohibido".

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de una página de error "Error 403: Prohibido".][17]

   > [!Important]
   > Dado que esta característica está en versión preliminar, debe administrar manualmente la entrada del servicio de nombres de dominio (DNS).

   En el caso de DNS, haga lo siguiente:
 
   - Use el servicio de zona privada de Azure DNS.  

     a. Cree una zona DNS privada denominada *`privatelink.azurewebsites.net`* y, luego, vincúlela a la red virtual.  
     b. Cree los dos registros A (es decir, el nombre de la aplicación y el nombre del Administrador de control de servicios [SCM]) con la dirección IP del punto de conexión privado.  
     > [!div class="mx-imgBorder"]
     > ![Captura de pantalla de registros de zona DNS privada.][21]  

   - Use el archivo *hosts* de la máquina virtual.  

     a. Cree la entrada de host, abra el Explorador de archivos y busque el archivo *hosts*.  
     > [!div class="mx-imgBorder"]
     > ![Captura de pantalla que muestra el archivo hosts en el Explorador de archivos.][18]  
     b. Agregue una entrada que contenga la dirección IP privada y el nombre público de la aplicación web. Para ello, edite el archivo *hosts* en un editor de texto.  
     > [!div class="mx-imgBorder"]
     > ![Captura de pantalla del texto del archivo hosts.][19]  
     c. Guarde el archivo.

1. Abra un explorador y escriba la dirección URL de la aplicación web.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de un explorador que muestra una aplicación web.][20]

Ahora accederá a la aplicación web mediante el punto de conexión privado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar el punto de conexión privado, la aplicación web y la máquina virtual, elimine el grupo de recursos y todos los recursos que contiene.

1. En Azure Portal, en el cuadro de **búsqueda**, escriba **ready-rgy** y, luego, seleccione **ready-rg** en la lista de resultados.

1. Seleccione **Eliminar grupo de recursos**.

1. En **Escriba el nombre del grupo de recursos**, escriba **ready-rg** y, luego, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado una máquina virtual en una red virtual, una aplicación web y un punto de conexión privado. Se ha conectado a una máquina virtual desde Internet y se ha comunicado de forma segura con la aplicación web mediante Private Link. 

Para más información sobre el punto de conexión privado (versión preliminar), consulte [¿Qué es un punto de conexión privado de Azure?][privateendpoint]

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtual-machine.png
[5]: ./media/create-private-endpoint-webapp-portal/vmnetwork.png
[6]: ./media/create-private-endpoint-webapp-portal/webapp.png
[7]: ./media/create-private-endpoint-webapp-portal/webappnetworking.png
[8]: ./media/create-private-endpoint-webapp-portal/webapppe.png
[9]: ./media/create-private-endpoint-webapp-portal/webapppenetwork.png
[10]: ./media/create-private-endpoint-webapp-portal/inprogress.png
[11]: ./media/create-private-endpoint-webapp-portal/webapppefinal.png
[12]: ./media/create-private-endpoint-webapp-portal/rdp.png
[13]: ./media/create-private-endpoint-webapp-portal/rdpdownload.png
[14]: ./media/create-private-endpoint-webapp-portal/pl.png
[15]: ./media/create-private-endpoint-webapp-portal/plcenter.png
[16]: ./media/create-private-endpoint-webapp-portal/privateendpointproperties.png
[17]: ./media/create-private-endpoint-webapp-portal/forbidden.png
[18]: ./media/create-private-endpoint-webapp-portal/explorer.png
[19]: ./media/create-private-endpoint-webapp-portal/hosts.png
[20]: ./media/create-private-endpoint-webapp-portal/webappwithpe.png
[21]: ./media/create-private-endpoint-webapp-portal/dns-private-zone-records.png


<!--Links-->
[privateendpointwebapp]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
