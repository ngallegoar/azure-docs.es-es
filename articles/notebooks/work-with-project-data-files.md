---
title: Importación y exportación de datos en proyectos con Azure Notebooks Preview
description: Aprenda a importar datos en un proyecto de Azure Notebooks Preview desde orígenes externos y a exportar datos desde un proyecto.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: e1d4a52ab7f4ad2ca3438af4bc87bec0b79f34d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646983"
---
# <a name="work-with-data-files-in-azure-notebooks-preview-projects"></a>Trabajo con archivos de datos en proyectos de Azure Notebooks Preview

Los datos son la parte esencial de muchas instancias de Jupyter Notebook, especialmente los cuadernos utilizados para la ciencia de datos. Con Azure Notebooks, puede importar fácilmente desde una variedad de orígenes en un proyecto y, a continuación, usar esos datos desde cuadernos. También puede hacer que los cuadernos generen datos que se almacenen en el proyecto, que podrá descargar más adelante para usar en otro lugar.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

El menú **Datos** de un cuaderno en ejecución también proporciona los comandos **Upload** y **Download**, que funcionan con archivos del proyecto y como archivos temporales para la sesión actual del cuaderno.

También puede usar código dentro de un cuaderno para obtener acceso a una variedad de orígenes de datos directamente, incluidos los archivos de un proyecto. También puede obtener acceso a datos arbitrarios mediante el uso de comandos en una celda de código. Dado que estos datos se almacenan en variables dentro de la sesión del cuaderno, no se guardarán en el proyecto a menos que utilice código para generar específicamente los archivos de proyecto.

La mejor manera de experimentar el uso de código en datos es dentro de un cuaderno en ejecución. Para ello, consulte el [cuaderno de ejemplo Getting to your data in Azure Notebooks (Obtención de datos en Azure Notebooks)](https://notebooks.azure.com/Microsoft/projects/samples/html/Getting%20to%20your%20Data%20in%20Azure%20Notebooks.ipynb).

El resto de este artículo proporciona detalles sobre las operaciones de archivos de nivel de proyecto.

## <a name="import-data"></a>Importar datos

Puede colocar archivos en un proyecto desde el panel del proyecto o en un cuaderno en ejecución mediante el menú **Datos** o un comando como `curl`.

### <a name="import-files-from-the-project-dashboard"></a>Importación de archivos desde el panel del proyecto

1. En el proyecto, vaya a la carpeta a la que quiere importar los archivos.

1. Seleccione el comando **Upload** y, a continuación, **From URL** (Desde dirección URL) o **Desde el equipo** y proyecte los detalles necesarios para los datos que quiere importar:

   - **From URL** (Desde dirección URL): Escriba la dirección de origen en el campo **Dirección URL del archivo** y el nombre de archivo que quiere asignar al cuaderno en el proyecto en el campo **Nombre de archivo**. A continuación, seleccione **+ Agregar archivo** para agregar la dirección URL a la lista de carga. Repita el proceso para direcciones URL adicionales y, a continuación, seleccione **Listo**.

     ![Ventana emergente para cargar desde URL](media/quickstarts/upload-from-url-popup.png)

   - **Desde el equipo**: Arrastre y coloque los archivos en la ventana emergente o seleccione **Elegir archivos** y, a continuación, vaya a los archivos de datos y seleccione los que quiere importar. Puede colocar o elegir cualquier número de archivos de cualquier tipo y formato, ya que corresponde al código del cuaderno abrir el archivo y analizar sus datos.

     ![Ventana emergente para cargar desde equipo](media/quickstarts/upload-from-computer-popup.png)

1. Una vez importados, los archivos aparecen en el panel del proyecto y se puede obtener acceso a ellos dentro del cuaderno mediante rutas de acceso relativas a la carpeta que los contiene.

### <a name="import-files-from-the-file-menu-in-a-notebook"></a>Importación de archivos desde el menú Archivo en un cuaderno

1. Dentro de un cuaderno en ejecución, seleccione el comando**File** > **Upload**:

    ![Comando de menú File Upload dentro de un cuaderno](media/file-menu-upload.png)

1. En el cuadro de diálogo que se abre, vaya a los archivos que quiere cargar y selecciónelos. Puede seleccionar el número de archivos que quiera de cualquier tipo. Seleccione **Abrir** cuando haya terminado.

1. En la ventana emergente **Estado de la carga** que aparece, seleccione una **carpeta de destino** en la lista desplegable:

    - Carpeta de la sesión ( *~/* ): Cargue archivos en la sesión actual del cuaderno, pero no cree archivos en el proyecto. La carpeta de la sesión es un elemento del mismo nivel que la carpeta del proyecto, pero no se conserva cuando finaliza la sesión. Para obtener acceso a los archivos de la sesión en código, agregue un prefijo a los nombres de archivo con la ruta de acceso relativa *../* .

        El uso de la carpeta de la sesión es útil para la experimentación y evita saturar el proyecto con archivos que puede que no necesite a largo plazo. También puede cargar archivos en la carpeta de la sesión con nombres idénticos a los archivos del proyecto sin causar conflictos ni tener que cambiarles el nombre. Por ejemplo, supongamos que tiene una versión de *data.csv* en el proyecto, pero quiere experimentar con una versión diferente de *data.csv*. Al cargar el archivo en la carpeta de la sesión, puede ejecutar el cuaderno con datos del archivo cargado (que hagan referencia a él en el código mediante *../data.csv*) en lugar de los datos del archivo del proyecto.

    - Carpeta del proyecto ( */project*): carga archivos en el proyecto, donde se puede obtener acceso a ellos mediante rutas de acceso relativas en código. Cargar un archivo en esta carpeta es lo mismo que cargar un archivo en el panel del proyecto. El archivo se guarda con el proyecto y está disponible en sesiones posteriores.

        Si intenta cargar un archivo con el mismo nombre que uno que ya existe en el proyecto, se produce un error con la carga. Para sobrescribir un archivo, cargue el archivo nuevo desde el panel del proyecto en su lugar, lo que le ofrece la opción de sobrescribir.

1. Seleccione **Iniciar carga** para completar el proceso.

### <a name="create-or-import-files-using-commands"></a>Creación o importación de archivos con comandos

Puede usar comandos dentro de un terminal o una celda de código de Python para crear archivos tanto en carpetas del proyecto como de la sesión. Por ejemplo, con comandos como `curl` y `wget`, se pueden descargar archivos desde Internet directamente.

Para descargar archivos en el terminal, seleccione el comando **Terminal** en el panel de proyecto y, a continuación, escriba los comandos adecuados:

```bash
curl https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv

wget https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv
```

Al usar una celda de código de Python en un cuaderno, agregue el prefijo `!` a los comandos.

La carpeta del proyecto es la carpeta predeterminada, por lo que al especificar un nombre de archivo de destino como *oil_price.csv*, se crea el archivo en el proyecto. Para crear un archivo de sesión, agregue el prefijo *../* al nombre, como en *../oil_price.csv*.

### <a name="create-files-in-code"></a>Creación de archivos en código

Al usar el código que crea un archivo, como la función `write_csv` de Pandas, las rutas de acceso son siempre relativas a la carpeta del proyecto. Al usar *../* , se crea un archivo de sesión que se descarta cuando se detiene y se cierra el cuaderno.

## <a name="export-files"></a>Exportación de archivos

Puede exportar datos desde el panel del proyecto o desde un cuaderno.

## <a name="export-files-from-the-project-dashboard"></a>Exportación de archivos desde el panel del proyecto

En el panel del proyecto, haga clic en un archivo y seleccione **Descargar**:

![Descargue el comando en el menú contextual del elemento del proyecto](media/download-command.png)

También puede seleccionar un archivo y usar el comando **Download** (método abreviado de teclado: d) en el panel:

![Comando Download de la barra de herramientas en el panel del proyecto](media/download-command-toolbar.png)

## <a name="export-files-from-the-data-menu-in-a-notebook"></a>Exportación de archivos desde el menú Datos en un cuaderno

1. Seleccione el comando de menú **File** > **Download**:

    ![Comando de menú Data Download dentro de un cuaderno](media/file-menu-download.png)

1. Aparece una ventana emergente que muestra las carpetas de la sesión; la carpeta *project* contiene los archivos del proyecto:

    ![Ventana emergente del comando Data Download en la que selecciona archivos y carpetas](media/file-menu-download-popup.png)

1. Seleccione los cuadros situados a la izquierda de los archivos y las carpetas que quiere descargar y, a continuación, seleccione **Descargar selección**.

1. El cuaderno prepara una único archivo *.zip* que contiene los archivos seleccionados, que puede guardar después como se suele hacer desde el explorador. El cuaderno crea un archivo *.zip* incluso cuando se descarga un único archivo.

## <a name="next-steps"></a>Pasos siguientes

- [Access cloud data in a notebook](access-data-resources-jupyter-notebooks.md) (Acceso a los datos en la nube en un cuaderno)
