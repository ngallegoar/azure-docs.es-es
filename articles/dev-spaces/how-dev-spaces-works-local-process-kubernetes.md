---
title: Funcionamiento de Proceso local con Kubernetes
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Describe los procesos para usar Proceso local con Kubernetes para conectar el equipo de desarrollo con el clúster de Kubernetes.
keywords: Proceso local con Kubernetes, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 443783eb7f5359318cf8efbec8b6466a80fa1e85
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84316266"
---
# <a name="how-local-process-with-kubernetes-works"></a>Funcionamiento de Proceso local con Kubernetes

Proceso local con Kubernetes le permite ejecutar y depurar el código en el equipo de desarrollo mientras sigue conectado al clúster de Kubernetes con el resto de aplicaciones o servicios. Por ejemplo, si tiene una arquitectura de microservicios de gran tamaño con muchos servicios y bases de datos interdependientes, la replicación de esas dependencias en el equipo de desarrollo puede resultar difícil. Además, compilar e implementar código en el clúster de Kubernetes para cada cambio de código durante el desarrollo del bucle interno puede ser lento y difícil de usar con un depurador, además de llevarle mucho tiempo.

Proceso local con Kubernetes evita tener que compilar e implementar el código en el clúster creando en su lugar una conexión directa entre el equipo de desarrollo y el clúster. La conexión del equipo de desarrollo al clúster durante la depuración le permite probar y desarrollar rápidamente el servicio en el contexto de la aplicación completa sin necesidad de crear ninguna configuración de Docker o Kubernetes.

Proceso local con Kubernetes redirige el tráfico entre el clúster de Kubernetes conectado y el equipo de desarrollo. Esta redirección del tráfico permite que el código del equipo de desarrollo y los servicios que se ejecutan en el clúster de Kubernetes se comuniquen como si estuvieran en el mismo clúster de Kubernetes. Proceso local con Kubernetes también proporciona una manera de replicar variables de entorno y volúmenes montados disponibles para pods en el clúster de Kubernetes en el equipo de desarrollo. Proporcionar acceso a las variables de entorno y a los volúmenes montados en el equipo de desarrollo le permite trabajar rápidamente en el código sin tener que replicar dichas dependencias manualmente.

## <a name="using-local-process-with-kubernetes"></a>Uso de Proceso local con Kubernetes

Para usar Proceso local con Kubernetes, necesita [Visual Studio Code][vs-code] con las extensiones de [Azure Dev Spaces][azds-vs-code] y [Azure Kubernetes Service][az-aks-vs-code] instaladas y ejecutándose en macOS o Windows 10, así como la [CLI de Azure Dev Spaces instalada][azds-cli]. También puede usar [Visual Studio 2019][visual-studio] en ejecución en Windows 10 con las cargas de trabajo de *ASP.NET y desarrollo web* y *desarrollo de Azure* instaladas y la marca de característica en versión preliminar *AzureDevSpacesTools.LocalKubernetesDebugging* habilitada, así como la [CLI de Azure Dev Spaces instalada][azds-cli]. Al usar Proceso local con Kubernetes para establecer una conexión con el clúster de Kubernetes, tiene la opción de redirigir todo el tráfico hacia y desde un pod nuevo o existente en el clúster al equipo de desarrollo.

> [!NOTE]
> Al usar Proceso local con Kubernetes, se le pedirá el nombre del servicio que desea redirigir al equipo de desarrollo. Esta opción es una forma cómoda de identificar un POD para el redireccionamiento. Todo el redireccionamiento entre el clúster de Kubernetes y el equipo de desarrollo es para un pod.

Cuando Proceso local con Kubernetes establece una conexión con el clúster, hace lo siguiente:

* Le pide que configure el servicio para reemplazar en el clúster, el puerto del equipo de desarrollo que se va a usar para el código y la tarea de inicio del código como una acción única.
* Reemplaza el contenedor del pod en el clúster por un contenedor de agente remoto que redirige el tráfico al equipo de desarrollo.
* Ejecuta [kubectl port-forward][kubectl-port-forward] en el equipo de desarrollo para reenviar el tráfico desde el equipo de desarrollo al agente remoto que se ejecuta en el clúster.
* Recopila información de entorno del clúster mediante el agente remoto. Esta información de entorno incluye variables de entorno, servicios visibles, montajes de volúmenes y montajes de secretos.
* Configura el entorno en Visual Studio o Visual Studio Code de modo que el servicio del equipo de desarrollo pueda tener acceso a las mismas variables como si se estuviera ejecutando en el clúster.  
* Actualiza el archivo de hosts para asignar servicios en el clúster a direcciones IP locales en el equipo de desarrollo. Estas entradas de archivo de host permiten que el código que se ejecuta en el equipo de desarrollo realice solicitudes a otros servicios que se ejecutan en el clúster. Para actualizar el archivo de hosts, Proceso local con Kubernetes solicitará acceso de administrador en el equipo de desarrollo al conectarse al clúster.
* Comienza a ejecutar y depurar el código en el equipo de desarrollo. Si es necesario, Proceso local con Kubernetes liberará los puertos necesarios en el equipo de desarrollo mediante la detención de los servicios o procesos que usan actualmente esos puertos.

Después de establecer una conexión con el clúster, puede ejecutar y depurar el código de forma nativa en el equipo, sin contenedores, y el código puede interactuar directamente con el resto del clúster. Cualquier tráfico que reciba el agente remoto se redirige al puerto local especificado durante la conexión para que el código que se ejecuta de forma nativa pueda aceptar y procesar ese tráfico. Las variables de entorno, los volúmenes y los secretos del clúster se ponen a disposición del código que se ejecuta en el equipo de desarrollo. Además, debido a que las entradas de archivo de host y el reenvío de puerto que Proceso local con Kubernetes agregó al equipo del desarrollador, el código puede enviar tráfico de red a los servicios que se ejecutan en el clúster mediante los nombres de servicio del clúster, y ese tráfico se reenvía a los servicios que se ejecutan en el clúster. El tráfico se enruta entre el equipo de desarrollo y el clúster todo el tiempo que está conectado.

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

Al usar Proceso local con Kubernetes para conectarse al clúster, los registros de diagnóstico del clúster se registran en el [directorio temporal][azds-tmp-dir] del equipo de desarrollo. Con Visual Studio Code, también puede usar el comando *Mostrar información de diagnóstico* para imprimir las variables de entorno y las entradas DNS actuales del clúster.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a usar Proceso local con Kubernetes para conectarse al equipo de desarrollo local en el clúster, consulte [Uso de Proceso local con Kubernetes con Visual Studio Code][local-process-kubernetes-vs-code] y [Uso de Proceso local con Kubernetes con Visual Studio][local-process-kubernetes-vs].

[azds-cli]: how-to/install-dev-spaces.md#install-the-client-side-tools
[azds-tmp-dir]: troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-aks-vs-code]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools
[local-process-kubernetes-vs-code]: how-to/local-process-kubernetes-vs-code.md
[local-process-kubernetes-vs]: how-to/local-process-kubernetes-visual-studio.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[visual-studio]: https://www.visualstudio.com/vs/
[vs-code]: https://code.visualstudio.com/download