---
title: Creación de acciones y recursos personalizados
description: En este tutorial se explica cómo crear acciones y recursos personalizados en Azure Resource Manager. También se muestra cómo interoperan los flujos de trabajo personalizados con plantillas de Azure Resource Manager, la CLI de Azure, Azure Policy y el registro de actividades de Azure.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 45f18727b53b802ba746da41b47fe955543ed9d0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75648552"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Creación de acciones y recursos personalizados en Azure

Un proveedor personalizado es un contrato entre Azure y un punto de conexión. Con los proveedores personalizados, puede cambiar los flujos de trabajo en Azure agregando nuevas API en Azure Resource Manager. Con estas API personalizadas, Resource Manager puede usar nuevas funcionalidades de implementación y administración.

En este tutorial se le guía por un ejemplo sencillo de cómo agregar nuevas acciones y recursos a Azure y cómo integrarlos.

## <a name="set-up-azure-functions-for-azure-custom-providers"></a>Configuración de Azure Functions para los proveedores personalizados de Azure

En la primera parte de este tutorial se describe cómo configurar una aplicación de funciones de Azure para que actúe con proveedores personalizados:

- [Configuración de Azure Functions para los proveedores personalizados de Azure](./tutorial-custom-providers-function-setup.md)

Los proveedores personalizados pueden funcionar con cualquier dirección URL pública.

## <a name="author-a-restful-endpoint-for-custom-providers"></a>Creación de un punto de conexión de RESTful para proveedores personalizados

En la segunda parte de este tutorial se explica cómo crear un punto de conexión de RESTful para proveedores personalizados:

- [Creación de un punto de conexión de RESTful para proveedores personalizados](./tutorial-custom-providers-function-authoring.md)

## <a name="create-and-use-a-custom-provider"></a>Creación y uso de un proveedor personalizado

En la tercera parte de este tutorial se describe cómo crear un proveedor personalizado y cómo usar sus acciones y recursos personalizados:

- [Creación y uso de un proveedor personalizado](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido sobre los proveedores personalizados y cómo crear uno. Para continuar al tutorial siguiente, consulte [Tutorial: Configuración de Azure Functions para los proveedores personalizados de Azure](./tutorial-custom-providers-function-setup.md).

Si busca referencias o un inicio rápido, estos son algunos vínculos útiles:

- [Inicio rápido: Creación de un proveedor de recursos personalizados de Azure e implementación de recursos personalizados](./create-custom-provider.md)
- [Cómo: Adición de acciones personalizadas a la API REST de Azure](./custom-providers-action-endpoint-how-to.md)
- [Cómo: Adición de recursos personalizados a la API REST de Azure](./custom-providers-resources-endpoint-how-to.md)
