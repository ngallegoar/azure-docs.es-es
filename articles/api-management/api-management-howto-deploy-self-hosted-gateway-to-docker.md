---
title: Implementación de una puerta de enlace autohospedada de Azure API Management en Docker | Microsoft Docs
description: Aprenda a implementar una puerta de enlace autohospedada de Azure API Management en Docker
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: b5acda30f7f4a01e1b7b6ac82652452c3c40e3bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75768510"
---
# <a name="deploy-an-azure-api-management-self-hosted-gateway-to-docker"></a>Implementación de una puerta de enlace autohospedada de Azure API Management en Docker

En este artículo se detallan los pasos para implementar una puerta de enlace autohospedada de Azure API Management en un entorno de Docker.

> [!NOTE]
> La característica de puerta de enlace autohospedada se encuentra en versión preliminar. Durante la versión preliminar, la puerta de enlace autohospedada solo está disponible en los niveles Desarrollador y Premium sin cargo adicional. El nivel de Desarrollador está limitado a una única implementación de puerta de enlace autohospedada.

## <a name="prerequisites"></a>Prerequisites

- Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
- Cree un entorno de Docker. [Docker for Desktop](https://www.docker.com/products/docker-desktop) es una buena opción para fines de desarrollo y evaluación. Consulte la [documentación de Docker](https://docs.docker.com) para obtener información sobre todas las ediciones de Docker, sus características y la documentación completa de Docker.
- [Aprovisione un recurso de puerta de enlace en la instancia de API Management](api-management-howto-provision-self-hosted-gateway.md)

> [!NOTE]
> La puerta de enlace autohospedada se empaqueta como un contenedor de Docker basado en Linux x86-64.

## <a name="deploy-the-self-hosted-gateway-to-docker"></a>Implementación de la puerta de enlace autohospedada en Docker

1. Seleccione **Puertas de enlace** en **Configuración**.
2. Seleccione el recurso de puerta de enlace que desea implementar.
3. Seleccione **Implementación**.
4. Tenga en cuenta que en el cuadro de texto **Token** se generará automáticamente un nuevo token con los valores predeterminados de **Expiración** y **Clave secreta**. Ajuste uno o ambos si lo desea y seleccione **Generar** para crear un nuevo token.
4. Asegúrese de que **Docker** esté seleccionado en **Scripts de implementación**.
5. Seleccione el vínculo del archivo **env.conf** junto al **Entorno** para descargar el archivo.
6. Seleccione el icono **copiar** situado en el extremo derecho del cuadro de texto **Ejecutar** para guardar el comando de Docker en el portapapeles.
7. Pegue el comando en la ventana de terminal (o comando). Ajuste las asignaciones de puerto y el nombre del contenedor según sea necesario. Tenga en cuenta que el comando espera que el archivo de entorno descargado esté presente en el directorio actual.
```
    docker run -d -p 80:8080 -p 443:8081 --name <gateway-name> --env-file env.conf mcr.microsoft.com/azure-api-management/gateway:<tag>
```
8. Ejecute el comando. El comando indica al entorno de Docker que ejecute el contenedor mediante la imagen de la puerta de enlace autohospedada descargada de Microsoft Container Registry y que asigne los puertos HTTP (8080) y HTTPS (8081) del contenedor a los puertos 80 y 443 del host.
9. Ejecute el siguiente comando para comprobar que el pod de puerta de enlace se está ejecutando:
```console
docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                         NAMES
895ef0ecf13b        mcr.microsoft.com/azure-api-management/gateway:beta   "/bin/sh -c 'dotnet …"   5 seconds ago       Up 3 seconds        0.0.0.0:80->8080/tcp, 0.0.0.0:443->8081/tcp   my-gateway
```
10. Vuelva a Azure Portal y confirme que el nodo de puerta de enlace que acaba de implementar notifique que su estado sea correcto.

![estado de la puerta de enlace](media/api-management-howto-deploy-self-hosted-gateway-to-docker/status.png)

> [!TIP]
> Use el comando <code>console docker container logs <gateway-name></code> para ver una instantánea del registro de la puerta de enlace autohospedada.
>
> Use el comando <code>docker container logs --help</code> para ver todas las opciones de visualización del registro.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la puerta de enlace autohospedada, consulte [Introducción a la puerta de enlace autohospedada de Azure API Management](self-hosted-gateway-overview.md).
* [Configure el nombre de dominio personalizado para la puerta de enlace autohospedada](api-management-howto-configure-custom-domain-gateway.md).
