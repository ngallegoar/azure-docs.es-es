---
title: Uso del editor de Azure Cloud Shell | Microsoft Docs
description: Información general sobre cómo usar el editor de Azure Cloud Shell.
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
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 7f597bb5cba1a12bdb93325fe2b877ffc644e3e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "60199233"
---
# <a name="using-the-azure-cloud-shell-editor"></a>Uso del editor de Azure Cloud Shell

Azure Cloud Shell incluye un editor de archivos creado a partir del código abierto del [Editor Monaco](https://github.com/Microsoft/monaco-editor). El editor de Cloud Shell admite características como el resaltado de lenguaje, la paleta de comandos y un explorador de archivos.

![Editor de Cloud Shell](media/using-cloud-shell-editor/open-editor.png)

## <a name="opening-the-editor"></a>Abriendo el archivo

Para crear y editar archivos de forma sencilla, inicie el editor ejecutando `code .` en el terminal de Cloud Shell. De este modo se abre el editor con el directorio de trabajo activo establecido en el terminal.

Para abrir directamente un archivo y editarlo rápidamente, ejecute `code <filename>` para abrir el editor sin el explorador de archivos.

Para abrir el editor con el botón de la interfaz de usuario, haga clic en el icono del editor `{}` desde la barra de herramientas. Así se abrirá el editor y, de forma predeterminada, el Explorador de archivos con el directorio `/home/<user>`.

## <a name="closing-the-editor"></a>Cierre del editor

Para cerrar el editor, abra el panel de acciones `...` en la parte superior derecha del mismo y seleccione `Close editor`.

![Cierre del editor](media/using-cloud-shell-editor/close-editor.png)

## <a name="command-palette"></a>Paleta de comandos

Para iniciar la paleta de comandos, utilice la tecla `F1` cuando se establece el foco en el editor. También se puede abrir la paleta de comandos mediante el panel de acción.

![Paleta de comandos](media/using-cloud-shell-editor/cmd-palette.png)

## <a name="contributing-to-the-monaco-editor"></a>Contribución al Editor Monaco

La compatibilidad con lo más destacado del lenguaje en el editor de Cloud Shell tiene lugar mediante la funcionalidad de nivel superior del uso del [Editor Monaco](https://github.com/Microsoft/monaco-editor) de definiciones de sintaxis de Monarch. Para más información sobre cómo realizar contribuciones, lea la [guía para colaboradores de Mónaco](https://github.com/Microsoft/monaco-editor/blob/master/CONTRIBUTING.md).

## <a name="next-steps"></a>Pasos siguientes
[Pruebe la guía de inicio rápido para Bash en Cloud Shell](quickstart.md)
[Vea la lista completa de las herramientas integradas de Cloud Shell](features.md)