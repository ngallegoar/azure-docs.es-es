---
title: Usar la ventana Azure Cloud Shell | Microsoft Docs
description: Información general sobre cómo usar la ventana Azure Cloud Shell.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: damaerte
ms.openlocfilehash: 3435d958049012750e775e2a17f43f463a0cc358
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "70860323"
---
# <a name="using-the-azure-cloud-shell-window"></a>Uso de la ventana Azure Cloud Shell

En este documento se explica cómo usar la ventana Cloud Shell.

## <a name="swap-between-bash-and-powershell-environments"></a>Intercambio entre los entornos Bash y PowerShell

Use el selector de entorno de la barra de herramientas de Cloud Shell para cambiar entre los entornos Bash y PowerShell.  
![Selección del entorno](media/using-the-shell-window/env-selector.png)

## <a name="restart-cloud-shell"></a>Reinicio de Cloud Shell
Haga clic en el icono de reinicio de la barra de herramientas de Cloud Shell para restablecer el estado de la máquina.  
![Reinicio de Cloud Shell](media/using-the-shell-window/restart.png)
> [!WARNING]
> Al reiniciar Cloud Shell se restablecerá el estado de la máquina y todos los archivos que no conserve el recurso compartido de archivos de Azure se perderán.

## <a name="change-the-text-size"></a>Cambio del tamaño del texto
Haga clic en el icono de configuración que se encuentra en la esquina superior izquierda de la ventana y, luego, mantenga el puntero sobre la opción "Tamaño de texto" y seleccione el tamaño de texto que prefiera. La selección se conservará de una sesión a otra.
![Tamaño del texto](media/using-the-shell-window/text-size.png)

## <a name="change-the-font"></a>Cambio de la fuente
Haga clic en el icono de configuración que se encuentra en la esquina superior izquierda de la ventana y, luego, mantenga el puntero sobre la opción "Fuente" y seleccione la fuente que prefiera.  La selección se conservará de una sesión a otra.
![Fuente](media/using-the-shell-window/text-font.png)

## <a name="upload-and-download-files"></a>Carga y descarga de archivos
Haga clic en el icono de carga y descarga de archivos en la parte superior izquierda de la ventana y, luego, seleccione Cargar o Descargar.  
![Carga y descarga de archivos](media/using-the-shell-window/uploaddownload.png)
* Para cargar archivos, use el elemento emergente para buscar el archivo en el equipo local, seleccione el archivo que desee y haga clic en el botón "Abrir".  El archivo se cargará en el directorio `/home/user`.
* Para descargar el archivo, escriba la ruta de acceso completa del mismo en la ventana emergente (es decir, básicamente, una ruta de acceso en el directorio `/home/user` que aparece de forma predeterminada) y seleccione el botón "Descargar".  
> [!NOTE] 
> Los archivos y las rutas de acceso de archivo distinguen entre mayúsculas y minúsculas en Cloud Shell. Compruebe el uso de mayúsculas y minúsculas en la ruta de acceso del archivo.

## <a name="open-another-cloud-shell-window"></a>Apertura de otra ventana de Cloud Shell
Cloud Shell permite varias sesiones simultáneas en pestañas del explorador posibilitando que cada sesión exista como un proceso independiente.
Si sale de una sesión, asegúrese de salir de cada ventana de sesión, ya que cada proceso se ejecuta de forma independiente aunque se ejecuten en la misma máquina.  
Haga clic en el icono para abrir nueva sesión en la parte superior izquierda de la ventana. Se abrirá una nueva pestaña con otra sesión conectada al contenedor existente.
![Abrir nueva sesión](media/using-the-shell-window/newsession.png)

## <a name="cloud-shell-editor"></a>Editor de Cloud Shell
* Consulte la página [Uso del editor de Azure Cloud Shell](using-cloud-shell-editor.md).

## <a name="web-preview"></a>Vista previa web
Haga clic en el icono de vista previa web en la parte superior izquierda de la ventana, seleccione "Configurar" y especifique el puerto que desee abrir.  Seleccione "Abrir puerto" para abrir solo el puerto, o "Abrir y examinar" para abrir el puerto y obtener una vista previa del mismo en una nueva pestaña.  
![Vista previa web](media/using-the-shell-window/preview.png)  
<br>
![Configurar puerto](media/using-the-shell-window/preview-configure.png)  
Haga clic en el icono de vista previa web en la parte superior izquierda de la ventana y seleccione "Puerto de vista previa..." para obtener una vista previa de un puerto abierto en una nueva pestaña. Haga clic en el icono de vista previa web en la parte superior izquierda de la ventana y seleccione "Cerrar puerto..." para cerrar el puerto abierto.  
![Vista previa del puerto y cerrar puerto](media/using-the-shell-window/preview-options.png)

## <a name="minimize--maximize-cloud-shell-window"></a>Minimizar y maximizar la ventana Cloud Shell
Haga clic en el icono Minimizar situado en la parte superior derecha de la ventana para ocultarla. Haga clic en el icono Cloud Shell de nuevo para mostrarla.
Haga clic en el icono Maximizar para establecer la ventana en la altura máxima. Para restablecer el tamaño anterior de la ventana, haga clic en Restaurar.  
![Minimizar o maximizar la ventana](media/using-the-shell-window/minmax.png)

## <a name="copy-and-paste"></a>Copiar y pegar
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Cambio del tamaño de la ventana Cloud Shell
Haga clic y arrastre el borde superior de la barra de herramientas hacia arriba o hacia abajo para cambiar el tamaño de la ventana Cloud Shell.

## <a name="scrolling-text-display"></a>Desplazamiento de la presentación del texto
Desplácese con el mouse o panel táctil para mover texto terminal.

## <a name="exit-command"></a>Comando Exit
Al ejecutar `exit` se terminará la sesión activa. Este comportamiento se produce de forma predeterminada después de 20 minutos sin interacción.

## <a name="next-steps"></a>Pasos siguientes

[Guía de inicio rápido de Bash en Cloud Shell](quickstart.md) <br>
[Guía de inicio rápido de PowerShell en Cloud Shell](quickstart-powershell.md)
