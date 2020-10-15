---
title: Guía de publicación para ofertas de contenedor en Azure Marketplace
description: En este artículo se describen los requisitos para publicar ofertas de contenedor en Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 09/04/2020
ms.openlocfilehash: c52fabcfc2ff22df2de6dd93f2543d625310baef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89484348"
---
# <a name="publishing-guide-for-container-offers"></a>Guía de publicación para ofertas de contenedor

Las ofertas de contenedor le ayudan a publicar la imagen de contenedor en Azure Marketplace. Use esta guía para comprender los requisitos para esta oferta. 

Las ofertas de contenedor son ofertas de transacciones que se implementan y facturan mediante Azure Marketplace. La opción que el usuario ve es "Obtener ahora".

Use el tipo de oferta *Contenedor* cuando la solución sea una imagen de contenedor de Docker configurada como instancia de servicio de contenedor de Azure basada en Kubernetes. 

> [!NOTE]
> Algunos ejemplos de instancias de servicio de contenedor de Azure basado en Kubernetes son Azure Kubernetes Service o Azure Container Instances, la opción de los clientes de Azure para un entorno en tiempo de ejecución de contenedor basado en Kubernetes.  

Microsoft admite actualmente los modelos de licencia gratuito y BYOL (traiga su propia licencia).

## <a name="container-offer-requirements"></a>Requisitos de la oferta de contenedor

| Requisito | Detalles |  
|:--- |:--- |  
| Facturación y medición | Admita el modelo de facturación gratuito o traiga su propia licencia (BYOL).<br><br> |  
| Imagen compilada a partir de un Dockerfile | Las imágenes de contenedor se deben basar en la especificación de imagen de Docker y se deben compilar a partir de un Dockerfile.<br> <br>Para obtener más información sobre cómo crear imágenes de Docker, consulte la sección "Uso" de [referencia de Dockerfile](https://docs.docker.com/engine/reference/builder/#usage).<br><br> |  
| Hospedaje en un repositorio de Azure Container Registry | Las imágenes de contenedor se deben hospedar en un repositorio de Azure Container Registry.<br> <br>Para obtener más información sobre cómo trabajar con Azure Container Registry, consulte [Inicio rápido: Creación de un registro de contenedor privado con Azure Portal](../container-registry/container-registry-get-started-portal.md).<br><br> |  
| Etiquetado de imágenes | Las imágenes de contenedor deben contener al menos 1 etiqueta (número máximo de etiquetas: 16).<br><br>Para obtener más información sobre cómo etiquetar una imagen, consulte la página de `docker tag` en el sitio de [documentación de Docker](https://docs.docker.com/engine/reference/commandline/tag).<br><br> |  

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, aprenda a [Ampliar su negocio en la nube con Azure Marketplace](https://azuremarketplace.microsoft.com/sell).

Para registrarse y empezar a trabajar en el Centro de partners, haga lo siguiente:

- [Inicie sesión en el Centro de partners](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) para crear o completar la oferta.
- Para más información, consulte [Creación de una oferta de Azure Container](./partner-center-portal/create-azure-container-offer.md).
