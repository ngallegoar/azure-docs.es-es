---
title: ¿Qué es la jerarquía de almacenamiento de Azure NetApp Files | Microsoft Docs
description: Describe la jerarquía del almacenamiento, incluidas las cuentas, grupos de capacidad y volúmenes de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/15/2020
ms.author: b-juche
ms.openlocfilehash: 0b150491fff953434062cc583566e1113947a679
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86504910"
---
# <a name="what-is-the-storage-hierarchy-of-azure-netapp-files"></a>¿Qué es la jerarquía de almacenamiento de Azure NetApp Files?

Antes de crear un volumen en Azure NetApp Files, debe adquirir y configurar un grupo para la capacidad aprovisionada.  Para configurar un grupo de capacidad, debe tener una cuenta de NetApp. Conocer la jerarquía de almacenamiento le ayuda a configurar y administrar los recursos de Azure NetApp Files.

> [!IMPORTANT] 
> Actualmente, Azure NetApp Files no admite la migración de recursos entre suscripciones.

## <a name="netapp-accounts"></a><a name="azure_netapp_files_account"></a>Cuentas de NetApp

- Una cuenta de NetApp actúa como una agrupación administrativa de los grupos de capacidad constituyentes.  
- No es lo mismo que la cuenta de almacenamiento general de Azure. 
- Una cuenta de NetApp es regional en el ámbito.   
- Puede tener varias cuentas de NetApp en una región, pero cada una está asociada solo a una única región.

## <a name="capacity-pools"></a><a name="capacity_pools"></a>Grupos de capacidad

- Un grupo de capacidad se mide por su capacidad aprovisionada.  
- La capacidad se aprovisiona con las SKU fijas que adquirió (por ejemplo, una capacidad de 4 TiB).
- Un grupo de capacidad puede tener solo un nivel de servicio.  
- Cada grupo de capacidad puede pertenecer a una sola cuenta de NetApp. Sin embargo, puede tener varios grupos de capacidad dentro de una cuenta de NetApp.  
- Un grupo de capacidad no se puede mover entre las cuentas de NetApp.   
  Por ejemplo, en el [Diagrama conceptual de la jerarquía de almacenamiento](#conceptual_diagram_of_storage_hierarchy) a continuación, el grupo de capacidad 1 no se puede mover de la cuenta de NetApp Este de EE. UU. a la cuenta de NetApp Oeste de EE. UU. 2.  
- No se puede eliminar un grupo de capacidad hasta que todos los volúmenes que contiene se hayan eliminado.

## <a name="volumes"></a><a name="volumes"></a>Volúmenes

- Un volumen se mide según el consumo de capacidad lógica y es escalable. 
- El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo.
- Cada volumen pertenece a un solo grupo, pero un grupo puede contener varios volúmenes. 
- Un volumen no pueden moverse entre grupos de capacidad. <!--Within the same NetApp account, you can move a volume across pools.  -->   
  Por ejemplo, en el siguiente [diagrama conceptual de la jerarquía de almacenamiento](#conceptual_diagram_of_storage_hierarchy), no puede mover los volúmenes del grupo de capacidad 1 al grupo de capacidad 2.

## <a name="conceptual-diagram-of-storage-hierarchy"></a><a name="conceptual_diagram_of_storage_hierarchy"></a>Diagrama conceptual de la jerarquía de almacenamiento 
El ejemplo siguiente muestra las relaciones de la suscripción de Azure, las cuentas de NetApp, los grupos de capacidad y los volúmenes.   

![Diagrama conceptual de la jerarquía de almacenamiento](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Pasos siguientes

- [Límites de recursos para Azure NetApp Files](azure-netapp-files-resource-limits.md)
- [Registro de Azure NetApp Files](azure-netapp-files-register.md)
