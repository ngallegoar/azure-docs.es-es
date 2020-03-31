---
title: Modelo de operaciones de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Modelo de operaciones de SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e147e4a5f104ca4cd1a10a776c907e3f9f1d6128
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616967"
---
# <a name="operations-model-and-responsibilities"></a>Modelo de operaciones y responsabilidades

El servicio proporcionado con SAP HANA en Azure (Instancias grandes) se alinea con los servicios de IaaS de Azure. Obtiene una instancia de HANA (instancias grandes) con un sistema operativo instalado que está optimizado para SAP HANA. Al igual que con las máquinas virtuales de IaaS de Azure, la mayoría de las tareas necesarias para proteger el sistema operativo, instalar el software adicional que necesite, instalar HANA, operar el sistema operativo y HANA y actualizar el sistema operativo y HANA son de su responsabilidad. Microsoft no exige que actualice el sistema operativo ni HANA.

![Responsabilidades de SAP HANA en Azure (Instancias grandes)](./media/hana-overview-architecture/image2-responsibilities.png)

Como se muestra en el diagrama, SAP HANA en Azure (instancias grandes) es una oferta de IaaS de varios inquilinos. La división de responsabilidad reside en el límite entre el sistema operativo y la infraestructura, en su mayor parte. Microsoft es responsable de todos los aspectos del servicio por debajo de la línea del sistema operativo. Usted es responsable de todos los aspectos del servicio encima de la línea. El sistema operativo es su responsabilidad. Puede seguir usando la mayor parte de los métodos locales que emplee actualmente para el cumplimiento normativo, la seguridad, la administración de aplicaciones, la base y la administración del sistema operativo. Los sistemas aparecen como si estuvieran en la red en todos los aspectos.

Este servicio está optimizado para SAP HANA, por lo que hay áreas en las que debe colaborar con Microsoft para usar las funcionalidades de la infraestructura subyacente a fin de obtener resultados óptimos.

En la lista siguiente, se proporcionan más detalles sobre cada una de las capas y sus responsabilidades:

**Redes**: todas las redes internas para la demarcación de instancias grandes que ejecutan SAP HANA. Su responsabilidad incluye el acceso al almacenamiento, la conectividad entre las instancias (para el escalado horizontal y otras funciones), la conectividad con la infraestructura y la conectividad con Azure cuando el nivel de aplicación de SAP se hospeda en máquinas virtuales. También incluye la conectividad de WAN entre los centros de datos de Azure para la replicación con fines de recuperación ante desastres. El inquilino se encarga de dividir todas las redes en particiones, que tienen aplicada calidad de servicio.

**Almacenamiento**: almacenamiento virtualizado con particiones para todos los volúmenes que necesitan los servidores de SAP HANA, así como para las instantáneas. 

**Servidores**: los servidores físicos dedicados a ejecutar las bases de datos de SAP HANA asignadas a los inquilinos. Los servidores de clase de tipo I de SKU se abstraen del hardware. Con estos tipos de servidores, la configuración del servidor se recopila y se mantiene en perfiles, que se pueden mover de un hardware físico a otro hardware físico. Este movimiento (manual) de un perfil mediante operaciones se puede comparar ligeramente a la recuperación de servicios de Azure. Los servidores de las SKU de clase de tipo II no ofrecen esta funcionalidad.

**SDDC**: software de administración que sirve para administrar los centros de datos como entidades definidas por software. Permite a Microsoft agrupar recursos por motivos de escalabilidad, disponibilidad y rendimiento.

**Sistema operativo**: el que elija (SUSE Linux o Red Hat Linux) que se ejecuta en los servidores. Las imágenes de sistema operativo que se le proporcionan son las imágenes ofrecidas por el proveedor de Linux individual a Microsoft para ejecutar SAP HANA. Debe tener una suscripción con el proveedor de Linux para la imagen optimizada de SAP HANA específica. Es responsable del registro de las imágenes con el proveedor del sistema operativo. 

Desde el momento en que Microsoft le hace entrega del producto, usted es responsable de cualquier revisión adicional del sistema operativo Linux. Esta aplicación de revisiones incluye paquetes adicionales que podrían ser necesarios para una correcta instalación de SAP HANA y que no se hayan incluido por el proveedor específico de Linux en sus imágenes de sistema operativo optimizadas para SAP HANA. (Para más información, consulte la documentación de instalación de HANA de SAP y las notas de SAP.) 

Usted es responsable de aplicar revisiones al SO debido a un funcionamiento inadecuado o la optimización del sistema operativo y sus controladores en relación con el hardware de servidor específico. También es responsable de la aplicación de revisiones de seguridad o funcionales del sistema operativo. 

Es también su responsabilidad la supervisión y planificación de la capacidad de:

- Consumo de recursos de CPU.
- Consumo de memoria.
- Volúmenes de disco relacionados con espacio disponible, IOPS y latencia.
- Tráfico de volumen de red entre HANA (instancias grandes) y el nivel de aplicación de SAP.

La infraestructura subyacente de HANA (instancias grandes) proporciona funcionalidad para las copias de seguridad y la restauración del volumen del sistema operativo. Usar esta funcionalidad es también su responsabilidad.

**Middleware**: la instancia de SAP HANA, principalmente. La administración, las operaciones y la supervisión son responsabilidad suya. Puede usar la funcionalidad que se proporciona para usar instantáneas de almacenamiento para la copia de seguridad y la restauración, además de para la recuperación ante desastres. Estas capacidades se proporcionan con la infraestructura. Sus responsabilidades incluyen también el diseño de la alta disponibilidad o la recuperación ante desastres con estas funcionalidades, aprovecharlas y supervisar que las instantáneas de almacenamiento se hayan ejecutado correctamente.

**Data**: los administrados por SAP HANA y otros datos, como archivos de copia de seguridad que se encuentran en volúmenes o recursos compartidos de archivos. Sus responsabilidades incluyen la supervisión del espacio libre en disco y la administración del contenido de los volúmenes. También es responsable de supervisar la ejecución correcta de las copias de seguridad de los volúmenes de disco y de las instantáneas de almacenamiento. La ejecución correcta de la replicación de datos en los sitios de recuperación ante desastres es responsabilidad de Microsoft.

**Aplicaciones:** las de la aplicación de SAP o, en el caso de aplicaciones que no son de SAP, el nivel de aplicación de estas. Sus responsabilidades incluyen la implementación, administración, operaciones y supervisión de esas aplicaciones. Usted es responsable de planear la capacidad de consumo de recursos de CPU, consumo de memoria, consumo de Azure Storage y consumo de ancho de banda de red dentro de las redes virtuales. También es responsable de planear la capacidad de consumo de recursos de redes virtuales para SAP HANA en Azure (instancias grandes).

**Redes WAN**: las conexiones que establece entre la implementación local y las implementaciones de Azure para las cargas de trabajo. Todos los clientes con HANA (instancias grandes) usan Azure ExpressRoute para la conectividad. Esta conexión no forma parte de la solución SAP HANA en Azure (instancias grandes). Usted es responsable de la configuración de esta conexión.

**Archivo**: es posible que prefiera archivar copias de los datos con sus propios métodos en cuentas de almacenamiento. El archivado conlleva tareas de administración, cumplimiento normativo, costos y operaciones. Es su responsabilidad generar copias de archivo y copias de seguridad en Azure y almacenarlas con un método compatible.

Consulte el [Acuerdo de Nivel de Servicio para SAP HANA en Azure (Instancias grandes)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/).

**Pasos siguientes**
- Consulte [Arquitectura de SAP HANA (instancias grandes) en Azure](hana-architecture.md).
