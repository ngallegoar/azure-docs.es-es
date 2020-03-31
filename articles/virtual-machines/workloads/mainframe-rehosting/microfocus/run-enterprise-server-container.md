---
title: Ejecución de Micro Focus Enterprise Server 4.0 en un contenedor de Docker en Azure Virtual Machines
description: Se vuelven a hospedar las cargas de trabajo de mainframes de IBM z/OS mediante la ejecución de Micro Focus Enterprise Server en un contenedor de Docker en Azure Virtual Machines.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 30d99c3f4767eb50361f7074c0d508fcf309faca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "61488487"
---
# <a name="run-micro-focus-enterprise-server-40-in-a-docker-container-on-azure"></a>Ejecución de Micro Focus Enterprise Server 4.0 en un contenedor de Docker en Azure

Puede ejecutar Micro Focus Enterprise Server 4.0 en un contenedor de Docker en Azure. En este tutorial se muestra cómo realizar las siguientes acciones. Utiliza la demostración de acctdemo CICS (Customer Information Control System) de Windows para Enterprise Server.

Docker agrega portabilidad y aislamiento a las aplicaciones. Por ejemplo, puede exportar una imagen de Docker desde una máquina virtual de Windows para ejecutarse en otra o desde un repositorio a un servidor Windows con Docker. La imagen de Docker se ejecuta en la nueva ubicación con la misma configuración, sin tener que instalar Enterprise Server. Es parte de la imagen. Se siguen aplicando las consideraciones de las licencias.

En este tutorial se instala la máquina virtual (VM) **Windows 2016 Datacenter con Containers** de Azure Marketplace. Esta máquina virtual incluye **Docker 18.09.0**. Los pasos siguientes muestran cómo implementar el contenedor, ejecutarlo y, a continuación, conectarse a él con un emulador 3270.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, revise estos requisitos previos:

- Suscripción a Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- El software de Micro Focus y una licencia válida (o licencia de prueba). Si es un cliente existente de Micro Focus, póngase en contacto con su representante de Micro Focus. En caso contrario, [solicite una versión de evaluación](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

     > [!NOTE]
     > Los archivos de demostración de Docker se incluyen con Enterprise Server 4.0. Este tutorial usa ent\_server\_dockerfiles\_4.0\_windows.zip. Acceda a él desde el mismo lugar que al archivo de instalación de Enterprise Server o vaya a *Micro Focus* para empezar a trabajar.

- La documentación de [Enterprise Server y Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#").

## <a name="create-a-vm"></a>Crear una VM

1. Proteja el medio desde el archivo ent\_server\_dockerfiles\_4.0\_windows.zip. Proteja el archivo de licencia ES-Docker-Prod-XXXXXXXX.mflic (necesario para compilar las imágenes de Docker).

2. Cree la máquina virtual. Para ello, abra Azure Portal, seleccione **Crear un recurso** en la esquina superior izquierda y filtre por *Servidor windows*. En los resultados, seleccione **Windows Server 2016 Datacenter: con contenedores**.

     ![Resultados de la búsqueda de Azure Portal](media/01-portal.png)

3. Para configurar las propiedades de la máquina virtual, elija los detalles de la instancia:

    1. Elija el tamaño de la máquina virtual. Para este tutorial, considere usar una máquina virtual **DS2 estándar\_v2** con 2 vCPU y 7 GB de memoria.

    2. Seleccione la **región** y el **grupo de recursos** en los que le gustaría implementarla.

    3. En **Opciones de disponibilidad**, use la configuración predeterminada.

    4. En **Nombre de usuario**, escriba la cuenta de administrador que desea utilizar y la contraseña.

    5. Asegúrese de que el **puerto RDP 3389** está abierto. Solo debe estar expuesto públicamente este puerto, para que pueda iniciar sesión en la máquina virtual. A continuación, acepte todos los valores predeterminados y haga clic en **Revisión + crear**.

     ![Creación de un panel de una máquina virtual](media/container-02.png)

4. Espere a que finalice la implementación (un par de minutos). Un mensaje indica que se ha creado la máquina virtual.

5. Haga clic en **Ir al recurso** para ir a la hoja **Introducción** para la máquina virtual.

6. A la derecha, haga clic en el **Conectar**. Las opciones de **Conectarse a una máquina virtual** aparecen a la derecha.

7. Haga clic en el botón **Descargar archivo RDP** para descargar el archivo RDP que le permite adjuntarse a la máquina virtual.

8. Tras terminar la descarga del archivo, ábralo y escriba el nombre de usuario y la contraseña que creó para la máquina virtual.

     > [!NOTE]
     > No use las credenciales corporativas para iniciar sesión. (El cliente RDP supone que desea usarlas. No lo haga).

9.  Seleccione **Más opciones** y, a continuación, seleccione las credenciales de la máquina virtual.

En este momento, la máquina virtual está ejecutándose y conectada a través de RDP. Ha iniciado sesión y está preparado para el paso siguiente.

## <a name="create-a-sandbox-directory-and-upload-the-zip-file"></a>Creación de un directorio de espacio aislado y carga del archivo zip

1.  Cree un directorio en la máquina virtual donde pueda cargar los archivos de licencia y demostración. Por ejemplo, **C:\\Sandbox**.

2.  Cargue el archivo **ent\_server\_dockerfiles\_4.0\_windows.zip** y **ES-Docker-Prod-XXXXXXXX.mflic** en el directorio que creó.

3.  Extraiga el contenido del archivo zip al directorio **ent\_server\_dockerfiles\_4.0\_windows** creado en el proceso de extracción. Este directorio incluye un archivo Léame (como el archivo .html y .txt) y dos subdirectorios, **EnterpriseServer** y **Examples**.

4.  Copie **ES-Docker-Prod-XXXXXXXX.mflic** a la unidad C:\\Sandbox\\ent\_server\_dockerfiles\_4.0\_windows\\EnterpriseServer y C:\\Sandbox\\ent\_server\_dockerfiles\_4.0\_windows\\ejemplos\\directorios CICS.

> [!NOTE]
> Asegúrese de que copiar el archivo de licencia en ambos directorios. Son necesarios en el paso de compilación de Docker, para asegurarse de que las imágenes tengan la licencia correcta.

## <a name="check-docker-version-and-create-base-image"></a>Comprobación de la versión de Docker y creación de la imagen base

> [!IMPORTANT]
> Crear la imagen de Docker correspondiente es un proceso de dos pasos. En primer lugar, cree la imagen base Enterprise Server 4.0. A continuación, cree otra imagen para la plataforma x64. Aunque puede crear una imagen x86 (de 32 bits), use la de 64 bits.

1. Abra un símbolo del sistema.

2. Compruebe que Docker está instalado. En el símbolo del sistema, escriba:

    ```
        docker version
    ```

     Por ejemplo, la versión era 18.09.0 cuando se escribía este documento.

3. Para cambiar el directorio, escriba **cd \Sandbox\ent_server_dockerfiles_4.0_windows\EnterpriseServer**.

4. Escriba **bld.bat IacceptEULA** para comenzar el proceso de compilación de la imagen inicial de base. Espere unos minutos a que este proceso se ejecute. En los resultados, tenga en cuenta las dos imágenes que se han creado, una para x64 y otra para x86:

     ![Ventana de comandos que muestra las imágenes](media/container-04.png)

5. Para crear la imagen final para la demostración de CICS, cambie al directorio de CICS; para ello, escriba **cd\\Sandbox\\ent\_server\_dockerfiles\_4.0\_windows\\Examples\\CICS**.

6. Para crear la imagen, escriba **bld.bat x64**. Espere unos minutos para que el proceso se ejecute y se indique con un mensaje que se creó la imagen.

7. Escriba **docker images** para mostrar una lista de todas las imágenes de Docker instaladas en la máquina virtual. Asegúrese de que **microfocus/es-acctdemo** es una.

     ![Ventana de comandos que muestra la imagen es-acctdemo](media/container-05.png)

## <a name="run-the-image"></a>Ejecución de la imagen 

1.  Para iniciar Enterprise Server 4.0 y la aplicación acctdemo, en el símbolo del sistema, escriba:

    ```
         docker run -p 16002:86/tcp -p 16002:86/udp -p 9040-9050:9040-9050 -p 9000-9010:9000-9010 -ti --network="nat" --rm microfocus/es-acctdemo:win_4.0_x64
    ```

2.  Instale un emulador de terminal 3270 como [x3270](http://x3270.bgp.nu/) y úselo para conectar, a través del puerto 9040, a la imagen que se está ejecutando.

3.  Obtenga la dirección IP del contenedor acctdemo, para que Docker pueda actuar como servidor DHCP para los contenedores que administra:

    1.  Obtenga el identificador del contenedor en ejecución. Escriba **Docker ps** en el símbolo del sistema y anote el identificador (**22a0fe3159d0**, en este ejemplo). Guárdelo para el siguiente paso.

    2.  Para obtener la dirección IP del contenedor acctdemo, use el identificador del contenedor del paso anterior como sigue:

    ```
       docker inspect <containerID> --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```

       Por ejemplo:

    ```   
        docker inspect 22a0fe3159d0 --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```
4. Anote la dirección IP de la imagen acctdemo. Por ejemplo, la dirección en la salida siguiente sería 172.19.202.52.

     ![Ventana de comandos que muestra la dirección IP](media/container-06.png)

5. Monte la imagen mediante el emulador. Configure el emulador para usar la dirección de la imagen acctdemo y el puerto 9040. En este ejemplo, es **172.19.202.52:9040**. La suya será similar. Se abre la pantalla **Signon to CICS** (Inicio de sesión para CICS).

    ![Pantalla Signon to CICS](media/container-07.png)

6. Inicie sesión en la región de CICS; para ello, escriba **SYSAD** en **USERID** y **SYSAD** en **Password** (Contraseña).

7. Borre la pantalla con el mapa de teclas del emulador. En x3270, seleccione la opción de menú **Keymap** (Mapa de teclas).

8. Para iniciar la aplicación acctdemo, escriba **ACCT**. Se muestra la pantalla inicial de la aplicación.

     ![Pantalla Account Demo](media/container-08.png)

9. Experimente con los tipos de cuenta de la pantalla. Por ejemplo, escriba **D** en la solicitud y **11111** en **ACCOUNT** (Cuenta). Otros números de cuenta son 22222, 33333 y así sucesivamente.

     ![Pantalla Account Demo](media/container-09.png)

10. Para mostrar la consola Enterprise Server Administration, vaya al símbolo del sistema y escriba **iniciar http:172.19.202.52:86**

     ![Consola Enterprise Server Administration](media/container-010.png)

Eso es todo. Ahora se está ejecutando y administración de una aplicación CICS en un contenedor de Docker.

## <a name="next-steps"></a>Pasos siguientes

- [Instalación de Micro Focus Enterprise Server 4.0 y Enterprise Developer 4.0 en Azure](./set-up-micro-focus-azure.md)
- [Migrar la aplicación del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
