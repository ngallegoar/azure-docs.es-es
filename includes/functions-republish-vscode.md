---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/20/2020
ms.author: glenga
ms.openlocfilehash: 574756aa9d9bac4aa42febf6a4fca4c62014db3f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84732485"
---
1. En Visual Studio Code, seleccione F1 para abrir la paleta de comandos. En la paleta de comandos, busque y seleccione **Azure Functions: Deploy to function app** (Azure Functions: Implementar en la aplicación de funciones).

1. Se le pedirá que **inicie sesión en Azure** si todavía no lo ha hecho. Después de iniciar sesión en el explorador, vuelva a Visual Studio Code. Si tiene varias suscripciones, **seleccione una suscripción** que contenga la aplicación de funciones.

1. Seleccione la aplicación de funciones existente en Azure. Cuando se le presente una advertencia sobre cómo sobrescribir todos los archivos de la aplicación de funciones, seleccione **Deploy** (Implementar) para confirmar la advertencia y continuar.

El proyecto se vuelve a compilar y empaquetar, y se carga en Azure. El proyecto existente se reemplaza por el nuevo paquete y se reinicia la aplicación de funciones.