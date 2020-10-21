---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 11/04/2019
ms.openlocfilehash: f5f132d257e30cd8f4fa1153087bf0df2f0f5b2c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91841867"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure.

1. En la esquina superior izquierda de Azure Portal, seleccione **+ Crear un recurso**.

    ![En la captura de pantalla se muestra la opción Crear un recurso.](media/aml-create-in-portal/create-workspace.gif)

1. Use la barra de búsqueda para encontrar **Machine Learning**.

1. Seleccione **Machine Learning**.

1. En el panel **Machine Learning**, seleccione **Crear** para comenzar.

1. Especifique la siguiente información para configurar la nueva área de trabajo:

   Campo|Descripción
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie del de las áreas de trabajo creadas por otros.
   Subscription |Seleccione la suscripción de Azure que quiera usar.
   Resource group | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. 
   Location | Seleccione la ubicación más cercana a los usuarios y los recursos de datos para crear el área de trabajo.
   Edición del área de trabajo | Seleccione **Básico** como el tipo de área de trabajo para este tutorial. El tipo de área de trabajo determina las características a las que tendrá acceso y los precios. Todo lo que se incluye en este tutorial se puede realizar en un área de trabajo de los tipos Básico o Enterprise.

1. Después de configurar el área de trabajo, seleccione **Revisar y crear**.

   > [!Warning]
   > La creación del área de trabajo en la nube puede tardar varios minutos.

   Cuando finalice el proceso, aparecerá un mensaje de implementación correcta.
 
 1. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.

