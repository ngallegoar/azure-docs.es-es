---
title: Conexión privada a una aplicación web mediante un punto de conexión privado de Azure
description: Conexión privada a una aplicación web mediante un punto de conexión privado de Azure
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: article
ms.date: 03/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 2f10c7378ae7681b14df6e96b6a6f1adac832d1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80287822"
---
# <a name="connect-privately-to-a-web-app-using-azure-private-endpoint-preview"></a>Conexión privada a una aplicación web mediante el punto de conexión privado de Azure (versión preliminar)

Un punto de conexión privado de Azure es el bloque de creación fundamental para Private Link en Azure. Le permite conectarse de forma privada a la aplicación web.
En este inicio rápido, aprenderá a implementar una aplicación web con un punto de conexión privado y a conectarse a esta aplicación web desde una máquina virtual.

Para obtener más información, consulte [Uso de puntos de conexión privados para una aplicación web de Azure][privatenedpointwebapp].

> [!Note]
>La versión preliminar está disponible en las regiones del Este de EE. UU. y Oeste de EE. UU. 2 para todas las aplicaciones web PremiumV2 de Windows y Linux. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="virtual-network-and-virtual-machine"></a>Virtual Network y Virtual Machines

En esta sección, va a crear la red virtual y la subred para hospedar la VM que se usa para acceder a la aplicación web mediante un punto de conexión privado.

### <a name="create-the-virtual-network"></a>Crear la red virtual

En esta sección, creará una red virtual y una subred.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Red virtual**, o busque **Red virtual** en el cuadro de búsqueda.

1. En **Crear red virtual**, escriba o seleccione esta información en la pestaña Conceptos básicos:

   > [!div class="mx-imgBorder"]
   > ![Crear red virtual][1]

1. Haga clic en **"Siguiente: Direcciones IP >"** y escriba o seleccione esta información:

   > [!div class="mx-imgBorder"]
   >![Configurar direcciones IP][2]

1. En la sección subred, haga clic en **"+ Agregar subred"** , escriba la siguiente información y haga clic en **"Agregar"** .

   > [!div class="mx-imgBorder"]
   >![Agregar subred][3]

1. Haga clic en **"Revisar y crear"** .

1. Una vez superada la validación, haga clic en **"Crear"** .

### <a name="create-virtual-machine"></a>Crear máquina virtual

1. En la parte superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Proceso** > **Máquina virtual**.

1. En Creación de una máquina virtual: conceptos básicos, escriba o seleccione esta información:

   > [!div class="mx-imgBorder"]
   >![Conceptos básicos de la máquina virtual][4]

1. Seleccione **"Siguiente: Discos"**

   Conserve la configuración predeterminada.

1. Seleccione **"Siguiente: Redes"** y seleccione esta información:

   > [!div class="mx-imgBorder"]
   >![Redes][5]

1. Haga clic en **"Revisar y crear"** .

1. Cuando reciba el mensaje de Validación superada, haga clic en **"Crear"** .

## <a name="create-your-web-app-and-private-endpoint"></a>Creación de la aplicación web y el punto de conexión privado

En esta sección, creará una aplicación web privada con un punto de conexión privado para ella.

> [!Note]
>La característica de punto de conexión privado solo está disponible para SKU Premium V2.

### <a name="web-app"></a>Aplicación web

1. En la parte superior izquierda de la pantalla en Azure Portal, seleccione **Crear un recurso** > **Web** > **Aplicación web**.

1. En Crear aplicación web: Conceptos básicos, escriba o seleccione esta información:

   > [!div class="mx-imgBorder"]
   >![Conceptos básicos de aplicación web][6]

1. Seleccione **"Revisar y crear"**

1. Cuando reciba el mensaje de Validación superada, haga clic en **"Crear"** .

### <a name="create-the-private-endpoint"></a>Creación del punto de conexión privado

1. En las propiedades de la aplicación web, seleccione **Configuración** > **Redes** y haga clic en **"Configurar las conexiones de punto de conexión privado"**

   > [!div class="mx-imgBorder"]
   >![Redes de aplicaciones web][7]

1. En el asistente, haga clic en **"+ Agregar"** .

   > [!div class="mx-imgBorder"]
   >![Punto de conexión privado de aplicación web][8]

1. Rellene la información de la suscripción, de la red virtual y de la subred y haga clic en **"Aceptar"** .

   > [!div class="mx-imgBorder"]
   >![Redes de aplicación web][9]

1. Revise la creación del punto de conexión privado

   > [!div class="mx-imgBorder"]
   >![Revisar][10]
   >![Vista final del punto de conexión privado][11]

## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

1. En la barra de búsqueda del portal, escriba **myVm**.
1. Seleccione el botón **Conectar**. Después de seleccionar el botón Conectar, se abre Conectar a máquina virtual, allí, seleccione **RDP**.

   > [!div class="mx-imgBorder"]
   >![Botón RDP][12]

1. Azure crea un archivo de Protocolo de escritorio remoto (.rdp) y lo descarga en su equipo después de hacer clic en **Descargar archivo RDP**.

   > [!div class="mx-imgBorder"]
   >![Descargar archivo RDP][13]

1. Abra el archivo .rdp descargado.

- Si se le pide, seleccione Conectar.
- Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

> [!Note]
> Es posible que tenga que seleccionar Más opciones > Usar otra cuenta para especificar las credenciales que escribió al crear la máquina virtual.

- Seleccione Aceptar.

1. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione Sí o Continuar.

1. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.

## <a name="access-web-app-privately-from-the-vm"></a>Acceso a la aplicación web de forma privada desde la máquina virtual

En esta sección, se conectará de manera privada a la aplicación web mediante el punto de conexión privado.

1. Obtenga la dirección IP privada del punto de conexión privado, en la barra de búsqueda escriba **Private Link** y seleccione Private Link.

   > [!div class="mx-imgBorder"]
   >![Private Link][14]

1. En el centro de Private Link, seleccione **Puntos de conexión privados** para mostrar todos los puntos de conexión privados.

   > [!div class="mx-imgBorder"]
   >![Centro de Private Link][15]

1. Seleccione el vínculo del punto de conexión privado a la aplicación web y la subred.

   > [!div class="mx-imgBorder"]
   >![Propiedades del punto de conexión privado][16]

1. Copie la dirección IP privada del punto de conexión privado y el FQDN de la aplicación web, en nuestro caso, webappdemope.azurewebsites.net 10.10.2.4

1. En myVM, compruebe que la aplicación web no es accesible a través de la dirección IP pública. Abra un explorador y pegue el nombre de la aplicación web. Debe aparecer una página de error 403 Prohibido.

   > [!div class="mx-imgBorder"]
   >![Prohibido][17]

> [!Important]
> Como esta característica se encuentra en versión preliminar, debe administrar manualmente la entrada DNS.

1. Cree la entrada de host, abra el explorador de archivos y busque el archivo de hosts.

   > [!div class="mx-imgBorder"]
   >![Archivo de hosts][18]

1. Para agregar una entrada con la dirección IP privada y el nombre público de la aplicación web edite el archivo de hosts con el Bloc de notas.

   > [!div class="mx-imgBorder"]
   >![Contenido de hosts][19]

1. Guarde el archivo.

1. Abra un explorador y escriba la dirección URL de la aplicación web.

   > [!div class="mx-imgBorder"]
   >![Sitio web con PE][20]

1. Está accediendo a la aplicación web a través del punto de conexión privado

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar el punto de conexión privado, la aplicación web y la máquina virtual, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba ready-rg en el cuadro Buscar de la parte superior del portal y seleccione ready-rg en los resultados de la búsqueda.
1. Seleccione Eliminar grupo de recursos.
1. Escriba ready-rg en la sección ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS: y seleccione Eliminar.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una máquina virtual en una red virtual, una aplicación web y un punto de conexión privado. Se ha conectado a una máquina virtual desde Internet y se ha comunicado de forma segura con la aplicación web mediante Private Link. Para más información sobre los puntos de conexión privados, consulte [¿Qué es un punto de conexión privado de Azure?][privateendpoint].

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtualmachine.png
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

<!--Links-->
[privatenedpointwebapp]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
