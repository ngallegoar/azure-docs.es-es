---
title: Guía de publicación para ofertas de contenedor en Azure Marketplace
description: En este artículo se describen los requisitos para publicar ofertas de contenedor en Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 11/30/2020
ms.openlocfilehash: 83c575aa40b80d9a8e39263e89a5e7860c8f8774
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95741668"
---
# <a name="publishing-guide-for-azure-container-offers"></a>Guía de publicación para ofertas de contenedor de Azure

Las ofertas de contenedor de Azure le ayudan a publicar la imagen de contenedor en Azure Marketplace. Use esta guía para comprender los requisitos para este tipo de oferta.

Las ofertas de contenedor de Azure son ofertas de transacciones que se implementan y facturan mediante Azure Marketplace. La opción que el usuario ve es "Obtener ahora".

Use el tipo de oferta Contenedor de Azure cuando la solución sea una imagen de contenedor de Docker configurada como instancia de contenedor de Azure basada en Kubernetes.

> [!NOTE]
> Una instancia de Azure Container Instances es una instancia de Docker de entorno de ejecución que proporciona la forma más rápida y sencilla de ejecutar un contenedor en Azure, sin tener que administrar ninguna máquina virtual y sin necesidad de adoptar un servicio de nivel superior. Las instancias de contenedor se pueden implementar directamente en Azure u organizarse mediante Azure Kubernetes Services o el motor de Azure Kubernetes Service.  

Microsoft admite actualmente los modelos de licencia gratuito y BYOL (traiga su propia licencia).

## <a name="container-offer-requirements"></a>Requisitos de la oferta de contenedor

| Requisito | Detalles |  
|:--- |:--- |  
| Facturación y medición | Admita el modelo de facturación gratuito o traiga su propia licencia (BYOL).<br><br> |  
| Imagen compilada a partir de un Dockerfile | Las imágenes de contenedor se deben basar en la especificación de imagen de Docker y se deben compilar a partir de un Dockerfile.<br> <br>Para obtener más información sobre cómo crear imágenes de Docker, consulte la sección "Uso" de [referencia de Dockerfile](https://docs.docker.com/engine/reference/builder/#usage).<br><br> |  
| Hospedaje en un repositorio de Azure Container Registry | Las imágenes de contenedor se deben hospedar en un repositorio de Azure Container Registry.<br> <br>Para obtener más información sobre cómo trabajar con Azure Container Registry, consulte [Inicio rápido: Creación de un registro de contenedor privado con Azure Portal](../container-registry/container-registry-get-started-portal.md).<br><br> |  
| Etiquetado de imágenes | Las imágenes de contenedor deben contener al menos 1 etiqueta (número máximo de etiquetas: 16).<br><br>Para obtener más información sobre cómo etiquetar una imagen, consulte la página de `docker tag` en el sitio de [documentación de Docker](https://docs.docker.com/engine/reference/commandline/tag).<br><br> |  

## <a name="next-steps"></a>Pasos siguientes

- Para preparar los recursos técnicos de una oferta de contenedor, consulte [Creación de recursos técnicos de Azure Container](create-azure-container-technical-assets.md).

- Para crear una oferta de Azure Container, consulte [Creación de una oferta de Azure Container en Azure Marketplace](create-azure-container-offer.md) para obtener más información.
