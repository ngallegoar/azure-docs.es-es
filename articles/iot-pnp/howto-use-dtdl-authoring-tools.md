---
title: Uso de una herramienta para crear y validar modelos DTDL | Microsoft Docs
description: Instale el editor de DTDL para Visual Studio Code o Visual Studio 2019 y úselo para crear modelos de IoT Plug and Play.
author: dominicbetts
ms.author: dobett
ms.date: 09/14/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 59ea0258d6262f652e9ac563bbda5ec89c4c5819
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91579467"
---
# <a name="install-and-use-the-dtdl-authoring-tools"></a>Instalación y uso de las herramientas de creación de DTDL

Los modelos de [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) son archivos JSON. Puede usar una extensión para Visual Studio Code o Visual Studio 2019 para crear y validar estos archivos de modelo.

## <a name="install-and-use-the-vs-code-extension"></a>Instalación y uso de la extensión de VS Code

La extensión de DTDL para VS Code agrega las siguientes características de creación de DTDL:

- Validación de la sintaxis de la versión 2 de DTDL.
- IntelliSense, incluido Autocompletar, para ayudarle con la sintaxis del lenguaje.
- La posibilidad de crear interfaces desde la paleta de comandos.

Para instalar la extensión de DTDL, vaya al [editor de DTDL para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl). También puede buscar **DTDL** en la vista de extensiones de VS Code.

Cuando haya instalado la extensión, úsela para ayudarlo a crear archivos de modelo de DTDL en VS Code:

- La extensión proporciona validación de la sintaxis en los archivos del modelo DTDL y resalta los errores, tal y como se muestra en la siguiente captura de pantalla:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-validation.png" alt-text="Validación de modelo en VS Code":::

- Use IntelliSense y Autocompletar al editar modelos de DTDL:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-intellisense.png" alt-text="Validación de modelo en VS Code":::

- Cree una interfaz de DTDL. El siguiente comando crea un archivo JSON con una nueva interfaz. La interfaz incluye telemetría de ejemplo, propiedades y definiciones de comandos.

## <a name="install-and-use-the-visual-studio-extension"></a>Instalación y uso de la extensión de Visual Studio

La extensión de DTDL para Visual Studio 2019 agrega las siguientes características de creación de DTDL:

- Validación de la sintaxis de la versión 2 de DTDL.
- IntelliSense, incluido Autocompletar, para ayudarle con la sintaxis del lenguaje.

Para instalar la extensión de DTDL, vaya a [Compatibilidad con lenguaje DTDL para Visual Studio 2019](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16dtdllanguagesupport). También puede buscar **DTDL** en **Administrar extensiones** en Visual Studio.

Cuando haya instalado la extensión, úsela para ayudarlo a crear archivos de modelo de DTDL en Visual Studio:

- La extensión proporciona validación de la sintaxis en los archivos del modelo DTDL y resalta los errores, tal y como se muestra en la siguiente captura de pantalla:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-validation-2.png" alt-text="Validación de modelo en VS Code":::

- Use IntelliSense y Autocompletar al editar modelos de DTDL:

    :::image type="content" source="media/howto-use-dtdl-authoring-tools/model-intellisense-2.png" alt-text="Validación de modelo en VS Code":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo de procedimientos, ha aprendido a usar las extensiones de DTDL para Visual Studio Code y Visual Studio 2019 para crear y validar archivos de modelo de DTDL. El siguiente paso sugerido es obtener información sobre cómo usar [Azure IoT Explorer con los modelos y dispositivos](./howto-use-iot-explorer.md).
