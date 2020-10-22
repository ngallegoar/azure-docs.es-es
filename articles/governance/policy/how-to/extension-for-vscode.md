---
title: Extensión de Azure Policy para Visual Studio Code
description: Obtenga información acerca de cómo usar la extensión de Azure Policy para Visual Studio Code para buscar alias de Azure Resource Manager.
ms.date: 10/14/2020
ms.topic: how-to
ms.openlocfilehash: ea05ffab9c57c50e451008a1ec7c534afbedf282
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92077939"
---
# <a name="use-azure-policy-extension-for-visual-studio-code"></a>Use la extensión de Azure Policy para Visual Studio Code

> Se aplica a la versión de extensión de Azure Policy **0.0.21** y versiones más recientes

Obtenga información sobre cómo usar la extensión de Azure Policy para Visual Studio Code para buscar [alias](../concepts/definition-structure.md#aliases) y revisar recursos y directivas. Primero, describiremos cómo instalar la extensión de Azure Policy en Visual Studio Code. Después, veremos cómo buscar alias.

La extensión de Azure Policy de Visual Studio Code se puede instalar en todas las plataformas compatibles con Visual Studio Code. Esta compatibilidad incluye Windows, Linux y macOS.

> [!NOTE]
> Los cambios realizados localmente en las directivas que se ven en la extensión de Azure Policy para Visual Studio Code no se sincronizan con Azure.

## <a name="prerequisites"></a>Requisitos previos

Los elementos siguientes son necesarios para completar los pasos indicados en este artículo:

- Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- [Visual Studio Code](https://code.visualstudio.com).

## <a name="install-azure-policy-extension"></a>Instalar la extensión de Azure Policy

Después de completar los requisitos previos, puede instalar la extensión de Azure Policy para Visual Studio Code siguiendo estos pasos:

1. Abra Visual Studio Code.

1. En la barra de menús, vaya a **Vista** > **Extensiones**.

1. En el cuadro de búsqueda, escriba **Azure Policy**.

1. Seleccione **Azure Policy** en los resultados de la búsqueda, luego haga clic en **Instalar**.

1. Seleccione **Recargar** cuando sea necesario.

## <a name="set-the-azure-environment"></a>Establecimiento del entorno de Azure

Para usuarios de la nube nacional, siga estos pasos para configurar el entorno de Azure en primer lugar:

1. Seleccione **Archivo\Preferencias\Configuración**.

1. Busque la siguiente cadena: _Azure: Cloud_ (Nube)

1. Seleccione la nube nacional de la lista:

   :::image type="content" source="../media/extension-for-vscode/set-default-azure-cloud-sign-in.png" alt-text="Captura de pantalla de la selección del inicio de sesión en la nube de Azure nacional para Visual Studio Code." border="false":::

## <a name="connect-to-an-azure-account"></a>Conexión a la cuenta de Azure

Para evaluar los recursos y los alias de búsqueda, debe conectarse a su cuenta de Azure. Siga estos pasos para conectarse a Azure desde Visual Studio Code:

1. Inicie sesión en Azure desde la extensión de Azure Policy o la paleta de comandos.

   - Extensión de Azure Policy

     En la extensión de Azure Policy, seleccione **Iniciar sesión en Azure**.

     :::image type="content" source="../media/extension-for-vscode/azure-cloud-sign-in-policy-extension.png" alt-text="Captura de pantalla de la selección del inicio de sesión en la nube de Azure nacional para Visual Studio Code." border="false":::

   - Paleta de comandos

     En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Azure: Sign In** (Iniciar sesión).

     :::image type="content" source="../media/extension-for-vscode/azure-cloud-sign-in-command-palette.png" alt-text="Captura de pantalla de la selección del inicio de sesión en la nube de Azure nacional para Visual Studio Code." border="false":::

1. Siga las instrucciones para iniciar sesión en Azure. Una vez establecida la conexión, el nombre de la cuenta de Azure se muestra en la barra de estado en la parte inferior de la ventana de Visual Studio Code.

## <a name="select-subscriptions"></a>Selección de suscripciones

La primera vez que inicia sesión, la extensión de Azure Policy carga los recursos y las directivas predeterminados de la suscripción. Para agregar o quitar suscripciones de los recursos y directivas que se muestran, siga estos pasos:

1. Inicie el comando de suscripción desde la paleta de comandos o el pie de página de la ventana.

   - Paleta de comandos: 

     En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Azure: Selección de suscripciones**.

   - Pie de página de ventana

     En el pie de página de la ventana, en la parte inferior de la pantalla, seleccione el segmento que coincida con **Azure: \<your account\>** .

1. Utilice el cuadro de filtro para buscar rápidamente las suscripciones por nombre. Luego, active o desactive la casilla de cada suscripción para establecer las suscripciones que se muestran en la extensión de Azure Policy. Cuando haya terminado de agregar o quitar las suscripciones que se van a mostrar, seleccione **Aceptar**.

## <a name="search-for-and-view-resources"></a>Buscar y ver recursos

La extensión de Azure Policy enumera los recursos de las suscripciones seleccionadas por el proveedor de recursos y por el grupo de recursos en el panel **Recursos**. La vista de árbol incluye las siguientes agrupaciones de recursos dentro de la suscripción seleccionada o en el nivel de suscripción:

- **Proveedores de recursos**
  - Cada proveedor de recursos registrado con recursos y recursos secundarios relacionados que tienen alias de directiva
- **Grupos de recursos**
  - Todos los recursos por el grupo de recurso en el que se encuentran

De forma predeterminada, la extensión filtra la parte del proveedor de recursos de los recursos existentes y de los que tienen alias de directiva. Cambie este comportamiento en **Configuración** > **Extensiones** > **Azure Policy** para ver todos los proveedores de recursos sin filtrado.

Los clientes con cientos o miles de recursos en una sola suscripción pueden preferir una forma de búsqueda para localizar sus recursos. La extensión de Azure Policy permite buscar un recurso específico con los pasos siguientes:

1. Empiece la interfaz de búsqueda desde la extensión de Azure Policy o la paleta de comandos.

   - Extensión de Azure Policy

     En la extensión de Azure Policy, mantenga el mouse sobre el panel **Recursos** y seleccione los puntos suspensivos, luego elija **Buscar recursos**.

   - Paleta de comandos:

     En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Recursos: Búsqueda de recursos**.

1. Si se selecciona más de una suscripción para mostrarla, use el filtro para elegir la suscripción que se va a buscar.

1. Use el filtro para elegir el grupo de recursos que se va a buscar que forma parte de la suscripción elegida anteriormente.

1. Use el filtro para seleccionar el recurso que se va a mostrar. El filtro funciona para el nombre del recurso y el tipo de recurso.

## <a name="discover-aliases-for-resource-properties"></a>Detección de alias para propiedades de recursos

Cuando se selecciona un recurso, ya sea mediante la interfaz de búsqueda o al seleccionarlo en la vista de árbol, la extensión de Azure Policy abre el archivo JSON que representa ese recurso y todos sus valores de propiedad de Azure Resource Manager.

Una vez que se abre un recurso, al mantener el mouse sobre el nombre o valor de la propiedad del administrador de recursos, se muestra el alias de Azure Policy, si existe uno. En este ejemplo, el recurso es un tipo de recurso `Microsoft.Compute/virtualMachines` y la propiedad **properties. storageProfile.imageReference.offer** se mantiene sobre ella. Al mantener el mouse se muestran los alias que coinciden.

:::image type="content" source="../media/extension-for-vscode/extension-hover-shows-property-alias.png" alt-text="Captura de pantalla de la selección del inicio de sesión en la nube de Azure nacional para Visual Studio Code." border="false":::

> [!NOTE]
> La extensión de VS Code solo expone las propiedades del modo Administrador de recursos y no muestra ninguna propiedad del modo [Proveedor de recursos](../concepts/definition-structure.md#mode).

## <a name="search-for-and-view-policies-and-assignments"></a>Búsqueda y visualización de directivas y asignaciones

La extensión de Azure Policy enumera los tipos de directivas y las asignaciones de directivas como una vista de árbol para las suscripciones seleccionadas que se mostrarán en el panel **Directivas**. Los clientes con cientos o miles de directivas o asignaciones una sola suscripción pueden preferir una forma de búsqueda para localizar sus directivas o asignaciones. La extensión de Azure Policy permite buscar una directiva o asignación específica con los pasos siguientes:

1. Empiece la interfaz de búsqueda desde la extensión de Azure Policy o la paleta de comandos.

   - Extensión de Azure Policy

     En la extensión de Azure Policy, mantenga el mouse sobre el panel **Directivas** y seleccione los puntos suspensivos, luego elija **Buscar directivas**.

   - Paleta de comandos:

     En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Directivas: Buscar directivas**.

1. Si se selecciona más de una suscripción para mostrarla, use el filtro para elegir la suscripción que se va a buscar.

1. Use el filtro para elegir el tipo de directiva o la asignación que se va a buscar y que forma parte de la suscripción elegida anteriormente.

1. Use el filtro para seleccionar la directiva o asignación que se va a mostrar. El filtro funciona para _displayName_ para la definición de directiva o la asignación de directiva.

Cuando se selecciona una asignación o directiva, ya sea a través de la interfaz de búsqueda o al seleccionarla en la vista de árbol, la extensión de Azure Policy abre el archivo JSON que representa esa asignación o directiva y todos sus valores de propiedad de administrador de recursos. La extensión puede validar el esquema JSON abierto de Azure Policy.

## <a name="sign-out"></a>Cerrar sesión

En la barra de menús, vaya a **Vista** > **Paleta de comandos** y escriba **Azure: Sign Out** (Cerrar sesión).

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md).
- Revise la [estructura de definición de Azure Policy](../concepts/definition-structure.md).
- Vea la [Descripción de los efectos de directivas](../concepts/effects.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](programmatically-create.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](remediate-resources.md).
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.