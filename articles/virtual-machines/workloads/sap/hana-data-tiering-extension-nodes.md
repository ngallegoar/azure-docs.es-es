---
title: Capas de datos y nodos de extensión para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Capas de datos y nodos de extensión para SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a1fc1c3b332d033e453db11ce4451cd626c4217
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94967812"
---
# <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>Uso de los nodos de extensión y las capas de datos de SAP HANA

SAP admite un modelo de capas de datos para SAP BW de las diferentes versiones de SAP NetWeaver y SAP BW/4HANA. Para más información sobre el modelo de capas de datos, consulte el documento de SAP [SAP BW/4HANA and SAP BW on HANA with SAP HANA extension nodes](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#) (SAP BW/4HANA y SAP BW en HANA con nodos de extensión de SAP HANA).
Con HANA (instancias grandes), puede utilizar la configuración de la opción 1 de los nodos de extensión de SAP HANA, tal como se detalla en estas preguntas más frecuentes y en los documentos del blog de SAP. Las configuraciones de la opción-2 se pueden configurar con las siguientes SKU las instancias grandes de HANA (Instancias grandes): S72m, S192, S192m, S384 y S384m. 

Al examinar la documentación, es posible que la ventaja no se vea de inmediato. Sin embargo, si se examinan las directrices de ajuste de tamaño de SAP, se puede ver una ventaja mediante el uso de los nodos de extensión de SAP HANA de las opciones 1 y 2. Estos son algunos ejemplos:

- Las directrices de ajuste de tamaño de SAP HANA suelen requerir que la cantidad de volumen de datos sea el doble que la memoria. Si ejecuta una instancia de SAP HANA con los datos activos, solo se llenará de datos el 50 % de la memoria o menos. Lo ideal es que el resto de la memoria se use para el funcionamiento del propio SAP HANA.
- Eso significa que en una unidad S192 de la instancia grande de HANA con 2 TB de memoria que ejecuta una base de datos de SAP BW, solo tiene 1 TB de volumen de datos.
- Si usa un nodo de extensión de SAP HANA adicional de la opción 1 y también una SKU de la unidad S192 de HANA (instancias grandes), tendría 2 TB adicionales para el volumen de datos. En la configuración de la opción 2, tendría 4 TB adicionales para el volumen de datos semiactivos. En comparación con el nodo activo, la capacidad de memoria total del nodo de extensión "semiactivo" puede utilizarse para almacenar los datos para la opción 1. Se puede usar el doble de memoria para el volumen de datos en la opción 2 de la configuración del nodo de extensión de SAP HANA.
- Tendrá una capacidad de 3 TB para los datos y una relación entre activos y semiactivos de 1:2 para la opción 1. Tiene 5 TB de datos y una relación de 1:4 con la opción 2 de la configuración del nodo de extensión.

Cuanto mayor sea el volumen de datos en comparación con la memoria, más posibilidad habrá de que los datos semiactivos que pide se encuentren en el almacenamiento en disco.

**Pasos siguientes**
- Consulte [Arquitectura de SAP HANA (instancias grandes) en Azure](hana-architecture.md).