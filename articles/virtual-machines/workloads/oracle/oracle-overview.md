---
title: Soluciones de Oracle en Microsoft Azure | Microsoft Docs
description: Obtenga información sobre las opciones para implementar aplicaciones y soluciones de Oracle en Microsoft Azure, incluidos la ejecución completa en la infraestructura de Azure o el uso de la conectividad entre nubes con Oracle Cloud Infrastructure (OCI).
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.openlocfilehash: 6ab41182d59f8c96edfca4854aad5f9a13f53436
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74806616"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Introducción a las aplicaciones y soluciones de Oracle en Azure

En este artículo se presentan funcionalidades para ejecutar soluciones de Oracle mediante la infraestructura de Azure. Consulte también la información más detallada disponible para las [imágenes de máquina virtual de Oracle](oracle-vm-solutions.md) en Azure Marketplace y la funcionalidad en versión preliminar para [interconectar Azure con Oracle Cloud Infrastructure (OCI)](oracle-oci-overview.md).

## <a name="oracle-databases-on-azure-infrastructure"></a>Bases de datos de Oracle en la infraestructura de Azure

Ejecute las bases de datos de Oracle en la infraestructura de Azure mediante imágenes de Oracle Database en Oracle Linux disponibles en Azure Marketplace:

* Oracle Database 12.1, 12.2 y 18.3 Enterprise Edition 

* Oracle Database 12.1, 12.2 y 18.3 Standard Edition 

También puede elegir configurar Oracle Database en una imagen que no sea de Oracle Linux disponible en Azure, basar una solución en una imagen personalizada que cree desde cero en Azure o cargar una imagen personalizada desde el entorno local.

Puede configurar opcionalmente varios discos conectados y mejorar el rendimiento de la base de datos mediante la instalación de Oracle Automated Storage Management (ASM).

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Aplicaciones en Oracle Linux y WebLogic Server

Ejecute aplicaciones empresariales en Azure en sistemas operativos compatibles con Oracle. Las siguientes imágenes están disponibles en Azure Marketplace:

* Oracle WebLogic Server 12.1.2

* Oracle Linux con Unbreakable Enterprise Kernel (UEK) 6.8, 6.9, 6.10, 7.3, 7.4, 7.5 y 7.6 

## <a name="high-availability-and-disaster-recovery-options"></a>Opciones de alta disponibilidad y recuperación ante desastres

* Configure [Oracle Data Guard](https://docs.oracle.com/cd/B19306_01/server.102/b14239/concepts.htm#g1049956), [Active Data Guard con FSFO](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/index.html), [Sharding](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/admin/sharding-overview.html) o [Golden Gate](https://www.oracle.com/middleware/technologies/goldengate.html) en la infraestructura de Azure en combinación con [Availability Zones](../../../availability-zones/az-overview.md) para conseguir una alta disponibilidad en la región. También puede realizar estas configuraciones en varias regiones de Azure para una mejor disponibilidad y recuperación ante desastres.

* Use [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) para orquestar y administrar la recuperación ante desastres para las máquinas virtuales de Oracle Linux en Azure y en sus servidores físicos o locales. 

* Habilite Oracle Real Application Clusters (RAC) en Azure mediante [Azure VMWare Solution](https://docs.azure.cloudsimple.com/oracle-rac/) o [FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="backup-oracle-workloads"></a>Copia de seguridad de cargas de trabajo de Oracle

* Copia de seguridad de máquinas virtuales de Oracle mediante [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)

* Realice una copia de seguridad de su instancia de Oracle Database con Oracle RMAN y, opcionalmente, use [Azure Blob Fuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux) para montar una [cuenta de Azure Blob Storage de gran redundancia](https://docs.microsoft.com/azure/storage/common/storage-redundancy) y escriba sus copias de seguridad de RMAN en ella para aumentar la resistencia.

## <a name="integration-of-azure-with-oci-preview"></a>Integración de Azure con OCI (versión preliminar)

Ejecute aplicaciones de Oracle en la infraestructura de Azure, conectada a las bases de datos de back-end de Oracle Cloud Infraestructure (OCI). Esta solución usa las siguientes funcionalidades: 

* **Redes entre nubes**: use la interconexión directa entre Azure ExpressRoute y Oracle FastConnect para establecer conexiones de gran ancho de banda, privadas y de baja latencia entre la aplicación y el nivel de la base de datos.
* **Identidad integrada**: configure la identidad federada entre Azure AD y Oracle IDCS para crear un único origen de identidad para las soluciones. Habilite el inicio de sesión único para administrar los recursos en OCI y Azure.

### <a name="deploy-oracle-applications-on-azure"></a>Implementación de aplicaciones de Oracle en Azure

Utilice plantillas de Terraform para configurar la infraestructura de Azure e instalar aplicaciones de Oracle validadas y compatibles para que se ejecuten en la configuración entre nubes:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Aplicaciones comerciales de Oracle
* Oracle Hyperion Financial Management

Implemente también aplicaciones personalizadas en Azure que se conecten con OCI y con otros servicios de Azure.

### <a name="set-up-oracle-databases-in-oci"></a>Configuración de bases de datos de Oracle en OCI

Use Oracle Database Cloud Services (Autonomous Database, RAC, Exadata, DBaaS, Single Node) junto con las aplicaciones de Oracle que se ejecutan en Azure. Más información sobre [opciones de la base de datos de OCI](https://docs.cloud.oracle.com/iaas/Content/Database/Concepts/databaseoverview.htm). 
 

## <a name="licensing"></a>Licencias

La implementación de aplicaciones de Oracle en Azure se basa en el modelo "traiga su propia licencia". Se supone que tiene la licencia correcta para usar software Oracle y que cuenta con un acuerdo de soporte técnico actual con Oracle. Oracle ha garantizado la movilidad de licencias del entorno local a Azure. Consulte las [preguntas más frecuentes](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html) sobre la relación entre Oracle y Azure.

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de la implementación de [imágenes de máquinas virtuales de Oracle](oracle-vm-solutions.md) en la infraestructura de Azure.

* Más información acerca de cómo [interconectar Azure con OCI](oracle-oci-overview.md).

* Consulte la [Sesión de introducción a Oracle en Azure](https://myignite.techcommunity.microsoft.com/sessions/82915) de Ignite 2019. 
