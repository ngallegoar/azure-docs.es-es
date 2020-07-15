---
title: Ejecución de código en contenedores Linux predeterminados
description: Azure App Service puede ejecutar el código en contenedores de Linux pregenerados. Descubra cómo puede ejecutar aplicaciones web de Linux en Azure.
keywords: azure app service, linux, oss
author: msangapu-msft
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.topic: overview
ms.date: 1/11/2019
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 891e0c18b3f95dca905fbc14b957af773135eeec
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/30/2020
ms.locfileid: "85557901"
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Introducción a Azure App Service en Linux

[Azure App Service](../overview.md) es una plataforma de procesos completamente administrada que se ha optimizado para el hospedaje de sitios y aplicaciones web. Los clientes pueden usar App Service en Linux para hospedar aplicaciones web de forma nativa en Linux para pilas de aplicaciones admitidas.

## <a name="languages"></a>Lenguajes

App Service en Linux es compatible con un toda una serie de imágenes integradas para aumentar la productividad del desarrollador. Los lenguajes incluyen: Node.js, Java (JRE 8 y JRE 11), PHP, Python, .NET Core y Ruby. Ejecute [`az webapp list-runtimes --linux`](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes) para ver los idiomas y las versiones admitidas más recientes. Si el tiempo de ejecución que la aplicación requiere no es compatible con las imágenes integradas, se ofrecen instrucciones sobre cómo [generar su propia imagen de Docker](tutorial-custom-docker-image.md) para implementar en Web App for Containers.

## <a name="deployments"></a>Implementaciones

* FTP
* Git local
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Entornos de ensayo
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) y DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Consola, publicación y depuración

* Entornos
* Implementaciones
* Consola básica
* SSH

## <a name="scaling"></a>Ampliación

* Los clientes pueden escalar o reducir aplicaciones web verticalmente mediante la modificación del nivel en su [plan de App Service](https://docs.microsoft.com/azure/app-service/overview-hosting-plans?toc=%2fazure%2fapp-service-web%2ftoc.json).

## <a name="locations"></a>Ubicaciones

Consulte el [panel de estado de Azure](https://azure.microsoft.com/status).

## <a name="limitations"></a>Limitaciones

Azure Portal solo muestra las características que funcionan actualmente para Web Apps for Containers. A medida que se habiliten más características, estas aparecerán en el portal.

App Service en Linux solo es compatible con los planes de App Service [Gratis, Básico, Estándar, Premium y Aislado](https://azure.microsoft.com/pricing/details/app-service/plans/), y no dispone de un nivel [Compartido](https://azure.microsoft.com/pricing/details/app-service/plans/). No se puede crear Aplicación web en Linux en un plan de App Service que ya hospeda Web Apps que no sean de Linux.  

Debido a una limitación actual, para el mismo grupo de recursos no puede mezclar aplicaciones de Windows y Linux en la misma región.

## <a name="troubleshooting"></a>Solución de problemas

> [!NOTE]
> Hay una nueva funcionalidad de registro integrada con la [supervisión de Azure (versión preliminar)](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs#send-logs-to-azure-monitor-preview). 
>
>

Si la aplicación no se inicia o desea comprobar el registro desde la aplicación, compruebe los registros de Docker en el directorio LogFiles. A este directorio se accede a través del sitio SCM o a través de FTP. Para registrar `stdout` y `stderr` desde un contenedor, es preciso habilitar **Registro de la aplicación** en **Registros de App Service**. Esta configuración surte efecto inmediatamente. App Service detecta el cambio y reinicia automáticamente el contenedor.

Puede acceder al sitio SCM desde **Herramientas avanzadas** en el menú **Herramientas de desarrollo**.

![Uso de Kudu para ver registros de Docker][1]

## <a name="next-steps"></a>Pasos siguientes

Los siguientes artículos introducirán App Service en Linux con aplicaciones web escritas en varios lenguajes:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](https://docs.microsoft.com/azure/app-service/containers/quickstart-php)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)
* [Go](quickstart-docker-go.md)
* [Aplicaciones de varios contenedores](quickstart-multi-container.md)

Para más información sobre App Service en Linux, consulte:

* [Peguntas más frecuentes acerca de App Service para Linux](app-service-linux-faq.md)
* [Compatibilidad de SSH con Azure App Service en Linux](app-service-linux-ssh-support.md)
* [Configuración de entornos de ensayo en Azure App Service](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Implementación continua de Docker Hub](app-service-linux-ci-cd.md)

Puede publicar preguntas y problemas en [nuestro foro](https://docs.microsoft.com/answers/topics/azure-webapps.html).

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
