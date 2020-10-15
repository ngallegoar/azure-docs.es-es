---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "71174991"
---
## <a name="terminology"></a>Terminología

Una imagen de Marketplace de Azure tiene los atributos siguientes:

* **Publicador**: Organización que ha creado la imagen. Ejemplos: Canonical, MicrosoftWindowsServer
* **Oferta**: nombre de un grupo de imágenes relacionadas creadas por un publicador. Ejemplos: UbuntuServer, WindowsServer
* **SKU**: Instancia de una oferta, por ejemplo, una versión principal de una distribución. Ejemplos: 18.04-LTS, 2019-Datacenter
* **Versión**: número de versión de una SKU de imagen. 

Para identificar una imagen de Marketplace cuando se implementa una máquina virtual mediante programación, brinde estos valores de manera individual como parámetros. Algunas herramientas aceptan un *URN* de imagen que combina estos valores separados por el carácter de dos puntos (:): *Publicador*:*Oferta*:*SKU*:*Versión*. En un URN, puede reemplazar el número de versión por "latest", que selecciona la versión más reciente de la imagen. 

Si el publicador de la imagen proporciona términos adicionales de licencia y compra, debe aceptar esos términos y habilitar la implementación mediante programación. También debe suministrar los parámetros de *plan de compra* al implementar una máquina virtual mediante programación. Consulte la sección [Implementación de una imagen con términos de Marketplace](#deploy-an-image-with-marketplace-terms).
