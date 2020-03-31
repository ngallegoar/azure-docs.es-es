---
title: Instalación del cliente Chef desde Azure Portal
description: Obtenga información sobre cómo implementar y configurar el cliente Chef desde Azure Portal.
keywords: azure, chef, devops, client, install, portal
ms.date: 02/22/2020
ms.topic: article
ms.openlocfilehash: 6e46133c598c44b314077f2d020852416d3d2745
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586366"
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>Instalación del cliente Chef desde Azure Portal
Puede agregar la extensión de cliente de Chef directamente en una máquina Linux o Windows desde Azure Portal. Este artículo le guiará a través de ese proceso con una nueva máquina virtual Linux.

## <a name="prerequisites"></a>Prerequisites

- **Suscripción de Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Chef**: si no tiene una cuenta de Chef activa, regístrese para obtener una [evaluación gratuita de Hosted Chef](https://manage.chef.io/signup). Para seguir las instrucciones de este artículo, necesitará los siguientes valores de la cuenta de Chef:
  - clave de organization_validation
  - rb
  - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>Instalación de la extensión de Chef en una nueva máquina virtual Linux
En esta sección, primero usará Azure Portal para crear una máquina Linux. Durante el proceso, también verá cómo instalar la extensión de Chef en la nueva máquina virtual.

1. Vaya a [Azure Portal](https://portal.azure.com).

1. En el menú de la izquierda, seleccione la opción **Máquinas virtuales**. Si no está la opción **Máquinas virtuales**, seleccione **Todos los servicios** y después **Máquinas virtuales**.

1. En la pestaña **Máquinas virtuales**, seleccione **Agregar**.

    ![Incorporación de una nueva máquina virtual en Azure Portal](./media/chef-extension-portal/add-vm.png)

1. En la pestaña **Proceso**, seleccione el sistema operativo deseado. Para esta demostración, se selecciona **Ubuntu Server**.

1. En la pestaña **Ubuntu Server**, seleccione **Ubuntu Server 16.04 LTS**.

    ![Al crear una máquina virtual Ubuntu, especifique la versión necesaria.](./media/chef-extension-portal/ubuntu-server-version.png)

1. En la pestaña **Ubuntu Server 16.04 LTS**, seleccione **Crear**.

    ![Ubuntu proporciona información adicional sobre su producto.](./media/chef-extension-portal/create-vm.png)

1. En la pestaña **Crear máquina virtual**, seleccione **Aspectos básicos**.

1. En la pestaña **Aspectos básicos**, especifique los siguientes valores y luego seleccione **Aceptar**.

   - **Nombre**: escriba el nombre de la nueva máquina virtual.
   - **Tipo de disco de máquina virtual**: especifique **SSD** o **HDD** como tipo de disco de almacenamiento. Para más información sobre los tipos de disco de máquina virtual, consulte el artículo [Selección de un tipo de disco](../virtual-machines/windows/disks-types.md).
   - **Nombre de usuario**: escriba un nombre de usuario al que se concederán privilegios de administrador en la máquina virtual.
   - **Tipo de autenticación**: seleccione **Contraseña**. También puede seleccionar **Clave SSH pública** y proporcionar un valor de clave SSH pública. Para los fines de esta demostración, y tal como aparece en las capturas de pantalla, se selecciona **Contraseña**.
   - **Contraseña** y **Confirmar contraseña**: escriba una contraseña para el usuario.
   - **Iniciar sesión con Azure Active Directory**: seleccione **Deshabilitado**.
   - **Suscripción**: seleccione la suscripción de Azure deseada, en caso de que tenga más de una.
   - **Grupo de recursos**: escriba un nombre para el grupo de recursos.
   - **Ubicación**: seleccione **Este de EE. UU**.

     ![Pestaña "Básico" para crear una máquina virtual](./media/chef-extension-portal/add-vm-basics.png)

1. En la pestaña **Elegir un tamaño**, seleccione el tamaño de la máquina virtual y luego haga clic en **Seleccionar**.

1. En la pestaña **Configuración**, la mayoría de los valores se rellenarán en función de los valores seleccionados en las pestañas anteriores. Seleccione **Extensiones**.

     ![Las extensiones se agregan a las máquinas virtuales con la pestaña Configuración](./media/chef-extension-portal/add-vm-select-extensions.png)

1. En la pestaña **Extensiones**, seleccione **Agregar extensión**.

     ![Seleccionar Agregar extensión para agregar una extensión a una máquina virtual](./media/chef-extension-portal/add-vm-add-extension.png)

1. En la pestaña **Nuevo recurso**, seleccione **Linux Chef Extension (1.2.3)** .

     ![Chef tiene extensiones para máquinas virtuales Linux y Windows](./media/chef-extension-portal/select-linux-chef-extension.png)

1. En la pestaña **Linux Chef Extension**, seleccione **Crear**.

1. En la pestaña **Instalar extensión**, especifique los valores siguientes y luego seleccione **Aceptar**.

    - **Dirección URL del servidor de Chef**: escriba la dirección URL del servidor de Chef que incluye el nombre de la organización, por ejemplo *https://api.chef.io/organization/mycompany* .
    - **Nombre de nodo de Chef**: escriba el nombre del nodo de Chef.
    - **Lista de ejecuciones**: escriba la lista de ejecuciones de Chef que se agrega a la máquina. Este valor se puede dejar en blanco.
    - **Nombre del cliente de validación**: escriba el nombre del cliente de validación de Chef. Por ejemplo: `tarcher-validator`.
    - **Clave de validación**: seleccione un archivo que contenga la clave de validación utilizada al arrancar las máquinas.
    - **Archivo de configuración de cliente**: seleccione un archivo de configuración para chef-client. Este valor se puede dejar en blanco.
    - **Versión de cliente de Chef**: escriba la versión del cliente de Chef que desea instalar. Este valor puede dejarse en blanco, lo que instala la versión más reciente.
    - **Modo de comprobación SSL**: seleccione **ninguno** o **punto**. Para la demostración se ha seleccionado *Ninguno*.
    - **Entorno de Chef**: escriba el entorno de Chef del que este nodo debe ser miembro. Este valor se puede dejar en blanco.
    - **Secreto de contenedor de datos cifrado**: seleccione un archivo que contenga el secreto del contenedor de datos cifrado al que esta máquina necesita acceder. Este valor se puede dejar en blanco.
    - **Certificado SSL del servidor de Chef**: seleccione el certificado SSL asignado al servidor de Chef. Este valor se puede dejar en blanco.

      ![Instalación del servidor de Chef en una máquina virtual Linux](./media/chef-extension-portal/install-extension.png)

1. Cuando se muestre la pestaña **Extensiones**, seleccione **Aceptar**.

1. Cuando se muestre la pestaña **Configuración**, seleccione **Aceptar**.

1. Cuando aparezca la pestaña **Crear**, verá un resumen de las opciones seleccionadas y especificadas. Compruebe la información, así como los **Términos de uso** y seleccione **Crear**.

Una vez completado el proceso de creación e implementación de la máquina virtual con la extensión de Chef, una notificación indica si la operación se ha realizado correctamente o con errores. Además, la página de recursos de la nueva máquina virtual se abre automáticamente en Azure Portal una vez creada.

![Instalación del servidor de Chef en una máquina virtual Linux](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Creación de una máquina virtual Windows en Azure mediante Chef](chef-automation.md)