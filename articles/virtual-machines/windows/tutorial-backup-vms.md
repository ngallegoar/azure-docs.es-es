---
title: 'Tutorial: Copia de seguridad de máquinas virtuales Windows en Azure Portal'
description: En este tutorial, aprenderá a usar Azure Portal para proteger las máquinas virtuales Windows con Azure Backup.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: recovery
ms.topic: tutorial
ms.workload: infrastructure
ms.date: 06/06/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 5dc41522add580b96e178328f47bd88fc1fbf052
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91397069"
---
# <a name="tutorial-back-up-and-restore-files-for-windows-virtual-machines-in-azure"></a>Tutorial: Copia de seguridad y restauración de archivos en máquinas virtuales Windows en Azure

Para proteger sus datos realice copias de seguridad a intervalos regulares. Azure Backup crea puntos de recuperación que se almacenan en almacenes de recuperación con redundancia geográfica. Cuando se realiza una restauración desde un punto de recuperación, se puede restaurar toda la máquina virtual o determinados archivos. En este artículo se explica cómo restaurar un único archivo en una máquina virtual que ejecuta Windows Server e IIS. Si aún no tiene una máquina virtual que pueda usar, puede crear una mediante la guía de [inicio rápido de Windows](quick-create-portal.md). En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una copia de seguridad de una máquina virtual.
> * Programar una copia de seguridad diaria.
> * Restaurar un archivo desde una copia de seguridad.

## <a name="backup-overview"></a>Introducción a Backup

Cuando el servicio Azure Backup inicia una copia de seguridad, desencadena la extensión de copia de seguridad para que tome una instantánea de un momento dado. El servicio Azure Backup usa la [extensión VMSnapshot](../extensions/vmsnapshot-windows.md). La extensión se instala cuando se realiza la primera copia de seguridad de la máquina virtual, en caso de que esta esté en ejecución. Si no se está ejecutando la máquina virtual, el servicio Azure Backup toma una instantánea del almacenamiento subyacente (ya que no se produce ninguna escritura de la aplicación mientras se detiene la máquina virtual).

Cuando se toma una instantánea de las máquinas virtuales de Windows, el servicio Azure Backup se coordina con el servicio de instantáneas de volumen (VSS) para obtener una instantánea coherente de los discos de la máquina virtual. Después de que el servicio Azure Backup toma la instantánea, se transfieren los datos al almacén. Para que el proceso resulte más eficaz, el servicio identifica y transfiere únicamente los bloques de datos que han cambiado desde la última copia de seguridad.

Cuando finaliza la transferencia de datos, se elimina la instantánea y se crea un punto de recuperación.

## <a name="create-a-backup"></a>Creación de una copia de seguridad
Cree una copia de seguridad diaria programada simple en un almacén de Recovery Services. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. En el menú de la izquierda, haga clic en **Máquinas virtuales**. 
1. En la lista, seleccione la máquina virtual de la que quiere realizar una copia de seguridad.
1. En la hoja de la máquina virtual, en la sección **Operaciones**, haga clic en **Copia de seguridad**. Se abre la hoja **Habilitar copia de seguridad**.
1. En **Almacén de Recovery Services**, haga clic en **Create new** (Crear nuevo) y especifique el nombre del nuevo almacén. Se crea un almacén en la misma ubicación y el mismo grupo de recursos en que se encuentra la máquina virtual.
1. En **Elegir directiva de copia de seguridad**, mantenga el valor predeterminado **DailyPolicy (nuevo)** y, después, haga clic en **Habilitar copia de seguridad**.
1. Para crear un punto de recuperación inicial, en la hoja **Copia de seguridad** haga clic en **Realizar copia de seguridad ahora**.
1. En la hoja **Realizar copia de seguridad ahora**, haga clic en el icono del calendario, use el control de calendario para elegir el tiempo que se va a conservar el punto de restauración y haga clic en **Aceptar**.
1. En la hoja **Copia de seguridad** de la máquina virtual, verá el número de puntos de restauración completos.


    ![Puntos de recuperación](./media/tutorial-backup-vms/backup-complete.png)
    
La primera copia de seguridad tarda aproximadamente 20 minutos. Cuando la copia de seguridad finalice, pase a la parte siguiente de este tutorial.

## <a name="recover-a-file"></a>Recuperación de un archivo

Si accidentalmente elimina o realiza cambios en un archivo, puede usar Recuperación de archivos para recuperar el archivo del almacén de Backup. Recuperación de archivos usa un script que se ejecuta en la máquina virtual para montar el punto de recuperación como unidad local. Estas unidades permanecen montadas durante 12 horas para que pueda copiar archivos desde el punto de recuperación y restaurarlos en la máquina virtual.  

En este ejemplo, se muestra cómo recuperar el archivo de imagen que se usa en la página web predeterminada de IIS. 

1. Abra un explorador y conéctese a la dirección IP de la máquina virtual para mostrar la página predeterminada de IIS.

    ![Captura de pantalla que muestra la página web de IIS predeterminada.](./media/tutorial-backup-vms/iis-working.png)

1. Conéctese a la máquina virtual.
1. En la máquina virtual, abra el **Explorador de archivos**, vaya a \inetpub\wwwroot y elimine el archivo **iisstart.png**.
1. En el equipo local, actualice el explorador para ver que la imagen en la página predeterminada de IIS ha desaparecido.

    ![Página web predeterminada de IIS](./media/tutorial-backup-vms/iis-broken.png)

1. En el equipo local, abra una nueva pestaña y vaya a [Azure Portal](https://portal.azure.com).
1. En el menú de la izquierda, seleccione **Máquinas virtuales** y seleccione la máquina virtual de la lista.
1. En la hoja de la máquina virtual, en la sección **Operaciones**, haga clic en **Copia de seguridad**. Se abre la hoja **Copia de seguridad**. 
1. En el menú de la parte superior de la hoja, seleccione **Recuperación de archivos**. Se abrirá la hoja **Recuperación de archivos**.
1. En **Paso 1: Seleccionar punto de recuperación**, seleccione un punto de recuperación en la lista desplegable.
1. En el **Paso 2: Descargar script para examinar y recuperar archivos**, haga clic en el botón **Descargar ejecutable**. Copie la contraseña del archivo y guárdela en algún lugar seguro.
1. En el equipo local, abra el **Explorador de archivos**, vaya a la carpeta **Descargas** y copie el archivo .exe descargado. El nombre de archivo lleva delante el nombre de su máquina virtual. 
1. En la máquina virtual (mediante la conexión RDP), pegue el archivo .exe en el escritorio de la máquina virtual. 
1. Vaya al escritorio de la máquina virtual y haga doble clic en el archivo .exe. Se iniciará un símbolo del sistema. El programa monta el punto de recuperación como un recurso compartido de archivos al que se puede acceder. Cuando se termine de crear el recurso compartido, escriba **q** para cerrar el símbolo del sistema.
1. En la máquina virtual, abra el **Explorador de archivos** y vaya a la letra de unidad que se usó para el recurso compartido de archivos.
1. Vaya a \inetpub\wwwroot, copie **iisstart.png** del recurso compartido de archivos y péguelo en \inetpub\wwwroot. Por ejemplo, copie F:\inetpub\wwwroot\iisstart.png y péguelo en c:\inetpub\wwwroot para recuperar el archivo.
1. En el equipo local, abra la pestaña del explorador en el que está conectado a la dirección IP de la máquina virtual que muestra la página predeterminada de IIS. Presione CTRL + F5 para actualizar la página del explorador. Ahora debería ver que la imagen se ha restaurado.
1. En el equipo local, vuelva a la pestaña del explorador de Azure Portal y, en **Paso 3: Desmontar los discos después de la recuperación**, haga clic en el botón **Desmontar discos**. Si olvida realizar este paso, la conexión al punto de montaje se cierra automáticamente al cabo de 12 horas. A las 12 horas, es preciso que descargue un script nuevo para crear un punto de montaje.





## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Crear una copia de seguridad de una máquina virtual.
> * Programar una copia de seguridad diaria.
> * Restaurar un archivo desde una copia de seguridad.

En el siguiente tutorial se explica cómo supervisar máquinas virtuales.

> [!div class="nextstepaction"]
> [Control de máquinas virtuales](tutorial-govern-resources.md)
