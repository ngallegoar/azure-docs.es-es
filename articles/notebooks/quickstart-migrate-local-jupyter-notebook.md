---
title: Migración de un cuaderno de Jupyter Notebook local a Azure Notebooks (versión preliminar)
description: Transfiera rápidamente un cuaderno de Jupyter Notebook a Azure Notebooks (versión preliminar) desde el equipo local o una dirección URL web y, posteriormente, a compartirlo con fines de colaboración.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: baf05d7adb1340d712ff0fc87436d5bbac51bc8f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77064332"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook-in-azure-notebooks-preview"></a>Inicio rápido: Migración de un cuaderno de Jupyter Notebook local a Azure Notebooks (versión preliminar)

En este inicio rápido, se migra a Azure Notebooks un cuaderno de Jupyter Notebook desde un equipo local u otra dirección URL de archivo accesible. 

Usted es el único que puede acceder a los cuadernos de Jupyter Notebook que estén en su propio equipo. Puede compartir sus archivos, pero en ese caso los destinatarios tendrán sus copias locales del cuaderno y es difícil incorporar sus cambios. Aunque almacene cuadernos en un repositorio en línea compartido como GitHub, cada colaborador debe tener una instalación local de Jupyter configurada como suya.

Al migrar a Azure Notebooks los cuadernos locales o los que están en el repositorio, puede compartirlos al instante con sus colaboradores, quienes solo necesitan un explorador para verlos y ejecutarlos. Si inician sesión en Azure Notebooks, también pueden realizar cambios.

## <a name="prerequisites"></a>Prerrequisitos

- Un [cuaderno de Jupyter Notebook](https://jupyter-notebook.readthedocs.io) en su equipo local o en otra dirección URL de archivo a la que se pueda acceder. 

## <a name="create-a-project-on-azure-notebooks"></a>Creación de un proyecto en Azure Notebooks

En este inicio rápido, se muestra cómo migrar un cuaderno desde su equipo local u otra dirección URL de archivo a la que se pueda acceder. Para migrar los cuadernos desde un repositorio de GitHub, consulte [Inicio rápido: Clonación de un cuaderno](quickstart-clone-jupyter-notebook.md).

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. Vaya a [Azure Notebooks](https://notebooks.azure.com) e inicie sesión. (Para más información, consulte [Quickstart - Sign in to Azure Notebooks](quickstart-sign-in-azure-notebooks.md) [Inicio rápido: Inicio de sesión en Azure Notebooks]).

1. En la página del perfil público, seleccione **My Projects** (Mis proyectos) en la parte superior de la página:

    ![Vínculo Mis proyectos en la parte superior de la ventana del explorador](media/quickstarts/my-projects-link.png)

1. En la página **My Projects** (Mis proyectos), seleccione **New Project** (Nuevo proyecto) (método abreviado de teclado: n). Si la ventana del explorador es estrecha, el botón puede aparecer solo como **+** :

    ![Comando New Project (Nuevo proyecto) en la página My Projects (Mis proyectos)](media/quickstarts/new-project-command.png)

1. En la ventana emergente **Crear nuevo proyecto**, introduzca los valores adecuados para el cuaderno que va a migrar en los campos **Nombre del proyecto** e **Id. de proyecto**, desactive las opciones **Proyecto público** y **Crear un archivo README.md** y, después, seleccione **Crear**.

## <a name="upload-the-local-notebook"></a>Carga del cuaderno local

1. En la página del proyecto, seleccione **Cargar** (que puede aparecer como solo una flecha hacia arriba si la ventana del explorador es pequeña) y, después, seleccione 1. En la ventana emergente que aparece, seleccione **From computer** (Desde el equipo) si el cuaderno se encuentra en el sistema de archivos local, o **From URL** (Desde una URL) si su cuaderno está en línea:

    ![Comando para cargar un cuaderno desde una dirección URL o el equipo local](media/quickstarts/upload-from-computer-url-command.png)

   De nuevo, si el cuaderno se encuentra en un repositorio de GitHub, siga los pasos de [Inicio rápido: Clonación de un cuaderno](quickstart-clone-jupyter-notebook.md).

   - Si usa **From Computer** (Desde el equipo), arrastre y coloque los archivos *.ipynb* en la ventana emergente o seleccione **Elegir archivos** y, después, vaya a los archivos que quiere importar. A continuación, seleccione **Cargar**. Los archivos cargados tienen el mismo nombre que los archivos locales. No es necesario cargar el contenido de ninguna de las carpetas *.ipynb_checkpoints*.

     ![Ventana emergente para cargar desde equipo](media/quickstarts/upload-from-computer-popup.png)

   - Si usa **From URL** (Desde la URL), escriba la dirección de origen en el campo **Dirección URL del archivo** y el nombre de archivo que quiere asignar al cuaderno en el proyecto en el campo **Nombre de archivo**. A continuación, seleccione **Cargar**. Si tiene varios archivos con direcciones URL diferentes, use el comando **Agregar archivo** para comprobar la primera dirección URL que escribió, después de que la ventana emergente proporcione nuevos campos para otro archivo.

     ![Ventana emergente para cargar desde URL](media/quickstarts/upload-from-url-popup.png)

1. Abra y ejecute el cuaderno recién cargado para comprobar su contenido y la operación. Cuando haya terminado, seleccione **Archivo** > **Detener y cerrar** para cerrar el cuaderno.

1. Para compartir un vínculo en el cuaderno cargado, haga clic en el archivo del proyecto y seleccione **Copiar vínculo** (método abreviado de teclado: y) y, después, péguelo en el mensaje adecuado. Como alternativa, puede compartir el proyecto como un todo con el control **Compartir** en la página del proyecto.

1. Para editar los archivos que no sean cuadernos, haga clic con el botón derecho en el archivo del proyecto y seleccione **Editar archivo** (método abreviado de teclado: i). La acción predeterminada, **Ejecutar** (método abreviado de teclado: r), solo muestra el contenido del archivo y no permite la edición.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Creación y ejecución de un cuaderno de Jupyter Notebook para hacer regresión lineal](tutorial-create-run-jupyter-notebook.md)
