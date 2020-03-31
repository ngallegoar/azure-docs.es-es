---
title: Perfil e identificador de usuario para su uso con la versión preliminar de Azure Notebooks
description: Cómo crear y administrar su perfil de usuario y el identificador de usuario con Azure Notebooks, que pasa a formar parte de la dirección URL de los cuadernos compartidos.
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: d90eebf1b7b463e038bc5e54f51df0eb6ca746c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646286"
---
# <a name="your-profile-and-user-id-for-azure-notebooks-preview"></a>Perfil e identificador de usuario para la versión preliminar de Azure Notebooks

En el espacio eficaz y de colaboración de Azure Notebooks, su perfil de usuario presenta su imagen pública a otros usuarios:

[![Página de perfil de Azure Notebooks](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

El identificador de usuario forma parte de las direcciones URL que usa para compartir los proyectos y los cuadernos. En la lista siguiente se describen los distintos patrones de dirección URL:

- `https://notebooks.azure.com/<user_id>`: Su página de perfil.
- `https://notebooks.azure.com/<user_id>/projects`: Sus proyectos. Usted verá todos los proyectos; otros usuarios solo ven los proyectos públicos.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Archivos de proyecto.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Clones de un proyecto específico.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: Vista previa en HTML de un cuaderno o archivo específico.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="your-user-id"></a>Su identificador de usuario

Al iniciar sesión en Azure Notebooks por primera vez, se asigna automáticamente un identificador de usuario temporal a su cuenta, como "anon-idr3ca". Mientras tenga un identificador de usuario que comience con "anon-", Azure Notebooks le pedirá que lo cambie cuando inicie sesión:

![Aviso para crear un identificador de usuario al iniciar sesión en Azure Notebooks](media/accounts/create-user-id.png)

También aparece un comando **Configurar identificador de usuario** junto al nombre de usuario temporal:

![Comando Configurar identificador de usuario que aparece cuando se usa un identificador temporal](media/accounts/configure-user-id-command.png)

También puede cambiar el identificador de usuario en cualquier momento en la página del perfil.

Un identificador de usuario debe estar compuesto por entre cuatro y seis letras, números y guiones. No se permite ningún otro carácter y el identificador de usuario no puede comenzar o finalizar con un guion ni usar varios guiones en una fila. Puesto que los identificadores de usuario son únicos en todas las cuentas de Azure Notebooks, verá un mensaje que indica que el identificador de usuario ya está en uso. (El mensaje también aparece si intenta usar una marca registrada de Microsoft como identificador de usuario). En estos casos, elija otro identificador de usuario.

> [!Important]
> El cambio de identificador invalida cualquier dirección URL que haya compartido usando el identificador anterior. Puede cambiar el identificador de vuelta a su identificador anterior para volver a validar los vínculos. Sin embargo, es posible que otro usuario reclame un identificador sin usar mientras tanto.

## <a name="your-profile"></a>Su perfil

Su perfil se compone de información visible públicamente en la dirección URL `https://notebooks.azure.com/<user_id>`. La página del perfil también muestra los proyectos utilizados recientemente y todos los proyectos destacados.

Para editar su perfil, utilice el comando **Editar información de perfil** en la página del perfil. Las secciones del perfil son las siguientes:

| Sección | Contenido |
| --- | --- |
| Foto de perfil | Imagen que se muestra en la página del perfil. |
| Información de cuenta | Nombre para mostrar, identificador de usuario y cuenta de correo electrónico pública. La cuenta de correo electrónico proporciona a otros usuarios un medio para comunicarse con usted y puede ser diferente de la [cuenta](azure-notebooks-user-account.md) que usa para iniciar sesión en Azure Notebooks. |
| Información del perfil | Ubicación, empresa, puesto, sitio web y una breve descripción de usted mismo. |
| Perfiles de redes sociales | Sus identificadores de GitHub, Twitter y Facebook, si desea compartirlos. |
| Configuración de privacidad | Proporciona dos comandos:<ul><li>**Exportar mi perfil**: crea y descarga un archivo *.zip* que contiene toda la información que Azure Notebooks guarda en su perfil, incluida la fotografía, la información de perfil y los registros de seguridad.</li><li>**Eliminar mi cuenta**: Elimina de forma permanente toda la información personal almacenada en Azure Notebooks.</li></ul> |
| Habilitar las características del sitio | Le permite controlar aspectos del comportamiento de Azure Notebooks:<ul><li>**Front-end unificado para cuadernos**: permite un inicio más rápido de los cuadernos y mejor persistencia.</li><li>**Ejecutar en JupyterLab de forma predeterminada**: De forma predeterminada, Azure Notebooks proporciona una interfaz de usuario simple que es adecuada para la mayoría de los usuarios. JupyterLab proporciona una interfaz más rica pero más complicada para los usuarios con experiencia.</li><li>**Sitio web de VNext**: habilita el diseño web modernizado que se muestra en esta documentación.</li></ul> |

## <a name="next-steps"></a>Pasos siguientes  

> [!div class="nextstepaction"]
> [Tutorial: create an run a Jupyter notebook to do linear regression](tutorial-create-run-jupyter-notebook.md) (Tutorial: Creación y ejecución de un cuaderno de Jupyter Notebook para hacer regresión lineal)
