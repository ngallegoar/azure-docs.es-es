---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6c594dbab51c46c382c21b4d87595cd7322f6036
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2020
ms.locfileid: "84465130"
---
Vaya a la cuenta de almacenamiento para la que desea restringir el punto de conexión público a redes virtuales específicas. En la tabla de contenido de la cuenta de almacenamiento, seleccione la entrada **Firewalls y redes virtuales**. 

En la parte superior de la página, seleccione el botón de radio **Redes seleccionadas**. Esta acción anulará la ocultación de una serie de opciones para controlar la restricción del punto de conexión público. Haga clic en **+ Add existing virtual network** (+ Agregar red virtual existente) para seleccionar la red virtual específica a la que se debe permitir el acceso a la cuenta de almacenamiento a través del punto de conexión público. Para ello, es necesario seleccionar una red virtual y una subred para esa red virtual. 

Active **Allow trusted Microsoft services to access this service account** (Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de servicio) para permitir que servicios de confianza de terceros, como Azure File Sync, accedan a la cuenta de almacenamiento.

[![Captura de pantalla de la hoja Firewalls y redes virtuales con una red virtual específica a la que se permite el acceso a la cuenta de almacenamiento mediante el punto de conexión público](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png)](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png#lightbox)