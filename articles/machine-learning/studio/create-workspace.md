---
title: Crear un área de trabajo
titleSuffix: ML Studio (classic) - Azure
description: Para usar Azure Machine Learning Studio (clásico), debe tener un área de trabajo de Machine Learning Studio. Esta área de trabajo contiene las herramientas que necesita para crear, administrar y publicar experimentos.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: 1a391a7a061d1382b5e07b45625c44fc0f5dec54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204467"
---
# <a name="create-and-share-an-azure-machine-learning-studio-classic-workspace"></a>Creación y uso compartido de un área de trabajo de Azure Machine Learning Studio (clásico)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Para usar Azure Machine Learning Studio (clásico), debe tener un área de trabajo de Machine Learning Studio. Esta área de trabajo contiene las herramientas que necesita para crear, administrar y publicar experimentos.

## <a name="create-a-studio-classic-workspace"></a>Creación de un área de trabajo de Studio (clásico)

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

    > [!NOTE]
    > Para iniciar sesión y crear un área de trabajo de Studio (clásico), debe ser un administrador de suscripciones de Azure. 
    >
    > 

2. Haga clic en **+ Nuevo**.

3. En el cuadro de búsqueda, escriba **Área de trabajo de Machine Learning Studio (clásico)** y seleccione el elemento coincidente. A continuación, en la parte inferior de la página, haga clic en **Crear**.

4. Escriba la información de su área de trabajo:

   - El *nombre de área de trabajo* puede tener hasta 260 caracteres, pero no puede terminar con un espacio. El nombre no puede incluir estos caracteres: `< > * % & : \ ? + /`
   - El *plan de servicio web* elegido (o creado), junto con el *plan de tarifa* asociado seleccionado, se usa si implementa servicios web desde esta área de trabajo.

     ![Creación de un área de trabajo de Studio (clásico)](./media/create-workspace/create-new-workspace.png)

5. Haga clic en **Crear**.

> [!NOTE]
> Machine Learning Studio (clásico) depende de la cuenta de Azure Storage que suministre para guardar los datos intermedios al ejecutar el flujo de trabajo. Una vez que se crea el área de trabajo, si se elimina la cuenta de almacenamiento y se cambian las claves de acceso, el área de trabajo dejará de funcionar y todos los experimentos que haya en ella darán error.
Si ha eliminado accidentalmente la cuenta, vuelva a crearla con el mismo nombre y en la misma región que la eliminada, y vuelva a sincronizar la clave de acceso. Si ha cambiado las claves de acceso de la cuenta de almacenamiento, vuelva a sincronizarlas en el área de trabajo mediante Azure Portal.

Una vez implementado el área de trabajo, puede abrirlo en Machine Learning Studio (clásico).

1. Vaya a Machine Learning Studio (clásico) en [https://studio.azureml.net/](https://studio.azureml.net/).

2. Seleccione el área de trabajo en la esquina superior derecha.

    ![Selección del área de trabajo](./media/create-workspace/open-workspace.png)

3. Haga clic en **my experiments** (Mis experimentos).

    ![Abrir experimentos](./media/create-workspace/my-experiments.png)

Para más información sobre cómo administrar un área de trabajo de Studio (clásico), consulte [Administración de un área de trabajo de Azure Machine Learning Studio (clásico)](manage-workspace.md).
Si tiene algún problema al crear el área de trabajo, consulte [Guía de solución de problemas: Creación y conexión a un área de trabajo de Machine Learning Studio (clásico)](troubleshooting-creating-ml-workspace.md).


## <a name="share-an-azure-machine-learning-studio-classic-workspace"></a>Uso compartido de un área de trabajo de Azure Machine Learning Studio (clásico)
Tras crear un área de trabajo de Machine Learning Studio (clásico), puede invitar usuarios y compartir el acceso a dicha área y a todos sus experimentos, conjuntos de datos, cuadernos, etc. Puede agregar usuarios en uno de estos roles:

* **Usuario**: los usuarios de un área de trabajo pueden crear, abrir, modificar y eliminar experimentos, conjuntos de datos y otros elementos en el área de trabajo.
* **Propietario**: un propietario puede invitar y quitar usuarios en el área de trabajo, además de lo que pueden hacer los usuarios.

> [!NOTE]
> La cuenta de administrador que crea el área de trabajo se agrega automáticamente al área de trabajo como propietario de dicha área. No obstante, a los otros administradores o usuarios de dicha suscripción no se les concede acceso automáticamente al área de trabajo; debe invitarlos de manera explícita.
> 
> 

### <a name="to-share-a-studio-classic-workspace"></a>Para crear un área de trabajo de Studio (clásico)

1. Inicie sesión en Machine Learning Studio (clásico) en [https://studio.azureml.net/Home](https://studio.azureml.net/Home).

2. En el panel izquierdo, haga clic en **SETTINGS** (Configuración).

3. Haga clic en la pestaña **USERS** (Usuarios).

4. Haga clic en **INVITE MORE USERS** (Invitar más usuarios) en la parte inferior de la página.

    ![Configuración de Studio](./media/create-workspace/settings.png)

5. Escriba una o varias direcciones de correo electrónico. Los usuarios necesitan una cuenta Microsoft o una cuenta de organización (de Azure Active Directory) válidas.

6. Seleccione si desea agregar los usuarios como propietario o usuario.

7. Haga clic en el botón de marca de verificación **OK** (Aceptar).

Cada usuario agregado recibirá un correo electrónico con instrucciones sobre cómo iniciar sesión en el área de trabajo compartida.

> [!NOTE]
> Para que los usuarios puedan implementar o administrar servicios web en esta área de trabajo, deben ser colaborador o administrador de la suscripción de Azure. 



