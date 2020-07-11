---
title: Instalación del entorno de desarrollo y pruebas de IBM zD&T en Azure | Microsoft Docs
description: Implementación del entorno de desarrollo y pruebas (zD&T) de IBM Z en la infraestructura como servicio (IaaS) de Azure Virtual Machine (VM).
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 04/02/2019
tags: ''
keywords: ''
ms.openlocfilehash: 67af4eae03b773fad9cf38964152c1fb9d623bd5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "72025939"
---
# <a name="install-ibm-zdt-devtest-environment-on-azure"></a>Instalación del entorno de desarrollo y pruebas de IBM zD&T en Azure

Para crear un entorno de desarrollo y pruebas para las cargas de trabajo de sistemas centrales en IBM Z Systems, puede implementar el entorno de desarrollo y pruebas (zD&T) de IBM Z en la infraestructura como servicio (IaaS) de Azure Virtual Machine (VM).

Con zD&T, puede aprovechar el ahorro en los costos de la plataforma x86 para sus entornos de desarrollo y pruebas menos críticos y, luego, volver a insertar las actualizaciones en un entorno de producción de Z System. Para más información, consulte las [instrucciones de instalación de IBM ZD&T](https://www-01.ibm.com/support/docview.wss?uid=swg24044565#INSTALL).

Azure y Azure Stack admiten las siguientes versiones:

- zD&T Personal Edition
- zD&T Parallel Sysplex
- zD&T Enterprise Edition

Todas las ediciones de zD&T solo se ejecutarán en sistemas Linux x86, no en Windows Server. Enterprise Edition se admite en Red Hat Enterprise Linux (RHEL) o Ubuntu/Debian. Hay disponibles imágenes de máquina virtual de RHEL y Debian para Azure.

En este artículo se muestra cómo configurar zD&T Enterprise Edition en Azure para poder usar el servidor web de zD&T Enterprise Edition para la creación y administración de entornos. La instalación de zD&T no instala ningún entorno. Se deben crear por separado como paquetes de instalación. Por ejemplo, Application Developers Controlled Distributions (ADCD) son imágenes de volumen de entornos de prueba. Están contenidas en imágenes ZIP en la distribución de medios disponibles en IBM. Consulte cómo [configurar un entorno ADCD en Azure](demo.md).

Para más información, consulte la [introducción a zD&T](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zdt.overview.gs.doc/topics/c_product_overview.html) en IBM Knowledge Center.

En este artículo se muestra cómo configurar la versión Enterprise Edition del entorno de desarrollo y pruebas de Z (zD&T) en Azure. Luego, puede usar el servidor web de zD&T Enterprise Edition para crear y administrar entornos basados en Z en Azure.

## <a name="prerequisites"></a>Requisitos previos

> [!NOTE]
> IBM permite la instalación de zD&T Enterprise Edition únicamente en entornos de desarrollo y pruebas, *no* en entornos de producción.

- Suscripción a Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Necesita acceso a los medios, que solo está disponibles para clientes y asociados de IBM. Para más información, póngase en contacto con su representante de IBM o consulte la información de contacto en el sitio web de [zD&T](https://www.ibm.com/us-en/marketplace/z-systems-development-test-environment).

- Un [servidor de licencias](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Es necesario para acceder a los entornos. La forma en que se cree depende de cómo se autorice el software desde IBM:

     - **Servidor de licencias basado en hardware**: requiere un dispositivo de hardware USB que contenga la licencia Rational Token necesaria para acceder a todas las partes del software. Se la debe proporciona IBM.

     - **Servidor de licencias basado en software**: requiere que configure un servidor centralizado para la administración de las claves de licencia. Este método es preferible y requiere que configure las claves que recibe de IBM en el servidor de administración.

## <a name="create-the-base-image-and-connect"></a>Creación de la imagen base y conexión

1. En Azure Portal, [cree una máquina virtual](/azure/virtual-machines/linux/quick-create-portal) con la configuración del sistema operativo que quiera. En este artículo se da por hecho una máquina virtual B4ms (con 4 vCPU y 16 GB de memoria) que ejecuta Ubuntu 16.04.

2. Una vez creada la máquina virtual, abra los puertos de entrada 22 para SSH, 21 para FTP y 9443 para el servidor web.

3. Obtenga las credenciales SSH que se muestran en la hoja **Información general** de la máquina virtual mediante el botón **Conectar**. Seleccione la pestaña **SSH** y copie el comando de inicio de sesión SSH en el Portapapeles.

4. Inicie sesión en un [shell de Bash](/azure/cloud-shell/quickstart) desde su equipo local y pegue el comando. El formato es **ssh\<user id\>\@\<IP Address\>** . Cuando se le soliciten las credenciales, escríbalas para establecer una conexión con el directorio de inicio.

## <a name="copy-the-installation-file-to-the-server"></a>Copia del archivo de instalación en el servidor

El archivo de instalación del servidor web es **ZDT\_Install\_EE\_V12.0.0.1.tgz**. Se incluye en los medios proporcionados por IBM. Debe cargar este archivo a la máquina virtual de Ubuntu.

1. Desde la línea de comandos, escriba el siguiente comando para asegurarse de que todo está actualizado en la imagen recién creada:

    ```
    sudo apt-get update
    ```

2. Cree el directorio de instalación:

    ```
    mkdir ZDT
    ```

3. Copie el archivo de la máquina local a la máquina virtual:

    ```
    scp ZDT_Install_EE_V12.0.0.1.tgz  your_userid@<IP Address /ZDT>   =>
    ```
    
> [!NOTE]
> Este comando copia el archivo de instalación en el directorio ZDT del directorio de inicio, que varía dependiendo de si el cliente ejecuta Windows o Linux.

## <a name="install-the-enterprise-edition"></a>Instalación de la versión Enterprise Edition

1. Vaya al directorio ZDT y descomprima el archivo ZDT\_Install\_EE\_V12.0.0.1.tgz mediante los siguientes comandos:

    ```
    cd ZDT
    chmod 755 ZDT\_Install\_EE\_V12.0.0.0.tgz
    ```

2. Ejecute el instalador:

    ```
    ./ZDT_Install_EE_V12.0.0.0.x86_64
    ```

3. Seleccione **1** para instalar Enterprise Server.

4. Presione **Entrar** y lea detenidamente los contratos de licencia. Al final de la licencia, escriba **Sí** para continuar.

5. Cuando se le pida que cambie la contraseña para el usuario recién creado, **ibmsys1**, use el comando **sudo passwd ibmsys1** y escriba la nueva contraseña.

6. Para comprobar si la instalación fue correcta, escriba:

    ```
    dpkg -l | grep zdtapp
    ```

7. Compruebe que la salida contenga la cadena **zdtapp 12.0.0.0**, que indica que el paquete se ha instalado correctamente.

### <a name="starting-enterprise-edition"></a>Inicio de Enterprise Edition

Tenga en cuenta que cuando se inicia el servidor web, se ejecuta con el identificador de usuario de zD&T que se creó durante el proceso de instalación.

1. Para iniciar el servidor web, use el identificador de usuario raíz para ejecutar el siguiente comando:

    ```
    sudo /opt/ibm/zDT/bin/startServer
    ```

2. Copie la dirección URL que ha generado el script, que se parece a esta:

    ```
    https://<your IP address or domain name>:9443/ZDTMC/login.htm
    ```

3. Pegue la dirección URL en un explorador web para abrir el componente de administración de la instalación de zD&T.

## <a name="next-steps"></a>Pasos siguientes

[Set up an Application Developers Controlled Distribution (ADCD) in IBM zD&T v1](./demo.md) (Configuración de una instancia de Application Developers Controlled Distribution (ADCD) en IBM zD&T v1)
