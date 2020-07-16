---
title: Uso compartido de Azure Dev Spaces
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Aprenda a usar Azure Dev Spaces para compartir un espacio de desarrollo en Azure Kubernetes Service con otros usuarios del equipo.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: 3cef7cd6a5113401cb1df3dd6c76e4dea3a7524d
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229302"
---
# <a name="share-azure-dev-spaces"></a>Uso compartido de Azure Dev Spaces

Con Azure Dev Spaces, puede compartir el espacio de desarrollo con otras personas de su equipo. Cada programador puede trabajar en su propio espacio sin riesgo de interrumpir a los demás. Además, trabajar en conjunto en un solo espacio puede permitirle probar código de un extremo a otro sin tener que crear objetos ficticios ni simular dependencias. Consulte la guía de [información sobre el desarrollo en equipo](../team-development-nodejs.md) para obtener más información.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Configuración de un espacio de desarrollo para varios desarrolladores

1. Cree una instancia de Dev Space en Azure. Elija [.NET Core y VS Code](../get-started-netcore.md), [.NET Core y Visual Studio](../get-started-netcore-visualstudio.md) o [Node.js y VS Code](../get-started-nodejs.md). Debe tener acceso de propietario o colaborador a la suscripción de Azure seleccionada.
1. Asegúrese de que cada miembro del equipo tenga los [permisos adecuados para acceder al controlador de Azure Dev Spaces](../troubleshooting.md#incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis). Por ejemplo, puede configurar el **grupo de recursos** del espacio de Azure Dev Spaces para [conceder acceso de colaborador](../../role-based-access-control/role-assignments-portal.md) a cada miembro del equipo. Puede comprobar el grupo de recursos de un espacio de desarrollo mediante la ejecución de este comando: `azds show-context`
1. Pida a los miembros del equipo que **seleccionen el espacio de desarrollo** para desarrollar en él.
   * **Línea de comandos o VS Code**: Para ver los espacios de Azure Dev Spaces existentes a los que tiene acceso: `azds space list`. Para seleccionar un espacio de desarrollo: `azds space select`.
   * **IDE de Visual Studio**: Abra un proyecto en Visual Studio y seleccione **Azure Dev Spaces** en la lista desplegable de configuración de inicio. En el cuadro de diálogo que se abre, seleccione un clúster existente.

     ![Desplegable de configuración de inicio de Visual Studio](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte sobre el [desarrollo en equipo](../team-development-nodejs.md) para más información.
