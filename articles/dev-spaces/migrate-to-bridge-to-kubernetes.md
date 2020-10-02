---
title: Migración a Bridge to Kubernetes
services: azure-dev-spaces
ms.date: 09/21/2020
ms.topic: conceptual
description: Describe los procesos que posibilitan el funcionamiento de Azure Dev Spaces
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Bridge to Kubernetes
ms.openlocfilehash: b585ee20efb7b377a041152996ef41d8c59c539e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993739"
---
# <a name="migrating-to-bridge-to-kubernetes"></a>Migración a Bridge to Kubernetes

Bridge to Kubernetes proporciona una alternativa más ligera a muchos de los escenarios de desarrollo que funcionan con Azure Dev Spaces. Bridge to Kubernetes es una experiencia solo del lado cliente que usa extensiones en [Visual Studio][vs] y [Visual Studio Code][vsc].  

Bridge to Kubernetes ayuda a la experiencia de desarrollo, ya que permite que una aplicación de Kubernetes establecida incluya un servicio que se ejecuta en la estación de trabajo de desarrollo local. A diferencia de Dev Spaces, Bridge to Kubernetes reduce las complejidades de los bucles internos al sortear la necesidad de crear configuraciones de Docker y Kubernetes, lo que permite a los desarrolladores centrarse exclusivamente en la lógica de negocios de su código de microservicio.

Bridge to Kubernetes le ayuda a iterar el código que se ejecuta en el equipo de desarrollo, al tiempo que usa las dependencias y la configuración existente de su entorno de Kubernetes. En cambio, Azure Dev Spaces implementa el microservicio en el entorno de Kubernetes antes de poder depurar el servicio de forma remota e iterar el código.

En este artículo se proporciona una comparación entre Azure Dev Spaces y Bridge to Kubernetes, así como las instrucciones para migrar de Azure Dev Spaces a Bridge to Kubernetes.

## <a name="development-approaches"></a>Enfoques de desarrollo

![Enfoques de desarrollo](media/migrate-to-btk/development-approaches.svg)

Azure Dev Spaces ayudó a los desarrolladores de Kubernetes a trabajar con código que se ejecuta directamente en su clúster de AKS, evitando la necesidad de un entorno local que no se parecía al entorno implementado. Este enfoque mejoró determinados aspectos del desarrollo, pero también incorporó un requisito previo de aprendizaje y mantenimiento de conceptos adicionales, como Docker, Kubernetes y Helm, antes de poder empezar a usar Azure Dev Spaces.

Bridge to Kubernetes permite a los desarrolladores trabajar directamente en sus equipos de desarrollo mientras interactúan con el resto del clúster. Este enfoque aprovecha la familiaridad y la velocidad de ejecución del código directamente en sus equipos de desarrollo, a la vez que les permite compartir las dependencias y el entorno que proporciona su clúster. Este enfoque también aprovecha la fidelidad y el escalado que aporta la ejecución en Kubernetes.

## <a name="feature-comparison"></a>Comparación de características

Azure Dev Spaces y Bridge to Kubernetes tienen características similares, pero también difieren en varias áreas:

| Requisito  | Azure Dev Spaces  | Bridge to Kubernetes  |
|---------------|-------------------|--------------------------------|
| Azure Kubernetes Service | En 15 regiones de Azure | Cualquier región del servicio AKS    |
| **Seguridad** |
| Acceso de seguridad necesario en el clúster  | Colaborador del clúster de AKS  | RBAC de Kubernetes: actualización de implementación   |
| Acceso de seguridad necesario en el equipo de desarrollo  | N/D  | Administrador local/sudo   |
| **Facilidad de uso** |
| Independiente de los artefactos de Kubernetes y Docker  | No  | Sí   |
| Reversión automática de los cambios, posterior a la depuración  | No  | Sí   |
| **Entornos** |
| Funciona con Visual Studio 2019  | Sí  | Sí   |
| Funciona con Visual Studio Code  | Sí  | Sí   |
| Funciona con la CLI  | Sí  | No   |
| **Compatibilidad con el sistema operativo** |
| Funciona en Windows 10  | Sí  | Sí  |
| Funciona en Linux  | Sí  | Sí  |
| Funciona en macOS  | Sí  | Sí  |
| **Capabilities** |
| Aislamiento del desarrollador o desarrollo en equipo  | Sí  | Sí  |
| Sobrescritura selectiva de variables de entorno  | No  | Sí  |
| Creación de Dockerfile y gráfico de Helm  | Sí  | No  |
| Implementación persistente del código en Kubernetes  | Sí  | No  |
| Depuración remota en un pod de Kubernetes  | Sí  | No  |
| Depuración local, conectada a Kubernetes  | No  | Sí  |
| Depuración de varios servicios a la vez en la misma estación de trabajo  | Sí  | Sí  |

## <a name="kubernetes-inner-loop-development"></a>Desarrollo de bucles internos de Kubernetes

La mayor diferencia entre Azure Dev Spaces y Bridge to Kubernetes es dónde se ejecuta el código. Azure Dev Spaces ayuda a desarrollar y depurar el código de microservicio, pero requiere que el código se ejecute en el clúster. Bridge to Kubernetes le permite desarrollar y depurar el código de microservicio directamente en el equipo de desarrollo, mientras se mantiene en el contexto de la aplicación general que se ejecuta en Kubernetes. Bridge to Kubernetes extiende el perímetro del clúster de Kubernetes y permite a los procesos locales heredar la configuración de Kubernetes.

![Desarrollo de bucles internos](media/migrate-to-btk/btk-graphic-non-isolated.gif)

Al usar Bridge to Kubernetes, se establece una conexión de red entre el equipo de desarrollo y el clúster. Durante la vigencia de esta conexión, se agrega un proxy al clúster en lugar de la implementación de Kubernetes, que redirige al equipo de desarrollo las solicitudes enviadas al servicio. Cuando se desconecte, la implementación de la aplicación volverá a usar la versión original de la implementación que se ejecuta en el clúster. Este enfoque difiere de cómo funciona Azure Dev Spaces, donde el código se sincroniza con el clúster, se compila y luego se ejecuta. Además, Azure Dev Spaces tampoco revierte el código.

Bridge to Kubernetes tiene la flexibilidad de funcionar con aplicaciones que se ejecutan en Kubernetes, independientemente de su método de implementación. Si usa CI/CD para compilar y ejecutar la aplicación, independientemente de si usa herramientas establecidas o scripts personalizados, puede seguir usando Bridge to Kubernetes para desarrollar y depurar el código.

> [!TIP]
> La  [extensión de Microsoft Kubernetes][kubernetes-extension] permite desarrollar rápidamente manifiestos de Kubernetes con IntelliSense y ayuda al scaffolding de los gráficos de Helm.  

### <a name="use-visual-studio-to-transition-to-bridge-to-kubernetes-from-azure-dev-spaces"></a>Uso de Visual Studio para realizar la transición a Bridge to Kubernetes desde Azure Dev Spaces

1. Actualice el IDE de Visual Studio a la versión 16.7 o superior e instale la extensión de Bridge to Kubernetes desde [Visual Studio Marketplace][vs-marketplace].
1. Deshabilite el controlador de Azure Dev Spaces mediante Azure Portal o la [CLI de Azure Dev Spaces][azds-delete].
1. Quite el archivo `azds.yaml` del proyecto.
1. Vuelva a implementar la aplicación.
1. Configure Bridge to Kubernetes en la aplicación implementada. Para obtener más información sobre el uso de Bridge to Kubernetes en Visual Studio, consulte [Uso de Bridge to Kubernetes][use-btk-vs].
1. Inicie la depuración en Visual Studio con el perfil de depuración de Bridge to Kubernetes recién creado.

### <a name="use-visual-studio-code-to-transition-to-bridge-to-kubernetes-from-azure-dev-spaces"></a>Uso de Visual Studio Code para realizar la transición a Bridge to Kubernetes desde Azure Dev Spaces

1. Instale la [extensión de Bridge en Kubernetes][vsc-marketplace].
1. Deshabilite el controlador de Azure Dev Spaces mediante Azure Portal o la [CLI de Azure Dev Spaces][azds-delete].
1. Quite el archivo `azds.yaml` del proyecto.
1. Vuelva a implementar la aplicación.
1. Configure Bridge to Kubernetes en la aplicación implementada. Para obtener más información sobre el uso de Bridge to Kubernetes en Visual Studio Code, consulte [Uso de Bridge to Kubernetes][use-btk-vsc].
1. Inicie la depuración en Visual Studio Code con el perfil de inicio de Bridge to Kubernetes recién creado.

## <a name="team-development-in-a-shared-cluster"></a>Desarrollo en equipo en un clúster compartido

También puede usar el enrutamiento específico del desarrollador con Bridge to Kubernetes. En el escenario de desarrollo en equipo de Azure Dev Spaces se usan varios espacios de nombres de Kubernetes para aislar un servicio del resto de la aplicación mediante el concepto de espacios de nombres primarios y secundarios. Bridge to Kubernetes ofrece la misma funcionalidad, pero con características de rendimiento mejoradas y en el mismo espacio de nombres de la aplicación.

Tanto Bridge to Kubernetes como Azure Dev Spaces requieren que los encabezados HTTP estén presentes y se propaguen toda la aplicación. Si ya ha configurado la aplicación para controlar la propagación de encabezados para Azure Dev Spaces, es necesario actualizar el encabezado. Para realizar la transición a Bridge to Kubernetes desde Azure Dev Spaces, actualice el encabezado configurado de *azds-route-as* a *kubernetes-route-as*.

## <a name="evaluate-bridge-to-kubernetes"></a>Evaluación de Bridge to Kubernetes

Si quiere experimentar con Bridge to Kubernetes antes de deshabilitar Azure Dev Spaces en el clúster, la manera más fácil es usar un clúster nuevo. Si intenta usar Azure Dev Spaces y Bridge to Kubernetes al mismo tiempo en el mismo clúster, tendrá problemas al usar las características de enrutamiento en ambos.

### <a name="use-visual-studio-to-evaluate-bridge-to-kubernetes"></a>Uso de Visual Studio para evaluar Bridge to Kubernetes

1. Actualice el IDE de Visual Studio a la versión 16.7 o superior e instale la extensión de Bridge to Kubernetes desde [Visual Studio Marketplace][vs-marketplace].
1. Cree un nuevo clúster de AKS e implemente la aplicación. También puede usar una [aplicación de ejemplo][btk-sample-app].
1. Configure Bridge to Kubernetes en la aplicación implementada. Para obtener más información sobre el uso de Bridge to Kubernetes en Visual Studio, consulte [Uso de Bridge to Kubernetes][use-btk-vs].
1. Inicie la depuración en Visual Studio con el perfil de depuración de Bridge to Kubernetes recién creado.

### <a name="use-visual-studio-code-to-evaluate-bridge-to-kubernetes"></a>Uso de Visual Studio Code para evaluar Bridge to Kubernetes

1. Instale la [extensión de Bridge en Kubernetes][vsc-marketplace].
1. Cree un nuevo clúster de AKS e implemente la aplicación. También puede usar una [aplicación de ejemplo][btk-sample-app].
1. Configure Bridge to Kubernetes en la aplicación implementada. Para obtener más información sobre el uso de Bridge to Kubernetes en Visual Studio Code, consulte [Uso de Bridge to Kubernetes][use-btk-vsc].
1. Inicie la depuración en Visual Studio con el perfil de inicio de Bridge to Kubernetes recién creado.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo funciona Bridge to Kubernetes.

> [!div class="nextstepaction"]
> [Funcionamiento de Bridge to Kubernetes][how-it-works-bridge-to-kubernetes]


[azds-delete]: how-to/install-dev-spaces.md#remove-azure-dev-spaces-using-the-cli
[kubernetes-extension]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools
[btk-sample-app]: /visualstudio/containers/bridge-to-kubernetes?view=vs-2019#install-the-sample-application
[how-it-works-bridge-to-kubernetes]: /visualstudio/containers/overview-bridge-to-kubernetes
[use-btk-vs]: /visualstudio/containers/bridge-to-kubernetes?view=vs-2019#connect-to-your-cluster-and-debug-a-service
[use-btk-vsc]: https://code.visualstudio.com/docs/containers/bridge-to-kubernetes
[vs]: https://visualstudio.microsoft.com/
[vsc-marketplace]: https://marketplace.visualstudio.com/items?itemName=mindaro.mindaro
[vs-marketplace]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.mindaro
[vsc]: https://code.visualstudio.com/