---
title: Proveedores de Azure Monitor para soluciones de SAP | Microsoft Docs
description: En este artículo se ofrecen respuestas a las preguntas más frecuentes sobre Azure Monitor para soluciones de SAP.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/30/2020
ms.author: radeltch
ms.openlocfilehash: 267d600270e834cf4f1f077452fda7459fac3029
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86525456"
---
# <a name="azure-monitor-for-sap-solutions-providers-preview"></a>Proveedores de Azure Monitor para soluciones de SAP (versión preliminar)

## <a name="overview"></a>Información general  

En el contexto de Azure Monitor para soluciones de SAP, un *tipo de proveedor* hace referencia a un *proveedor* específico. Por ejemplo, *SAP HANA*, que se configura para un componente específico del entorno de SAP, como la base de datos de SAP HANA. Un proveedor contiene la información de conexión del componente correspondiente y ayuda a recopilar datos de telemetría de dicho componente. Un recurso de Azure Monitor para soluciones de SAP (conocido también como recurso de supervisión de SAP) se puede configurar con varios proveedores del mismo tipo de proveedor o con varios proveedores de varios tipos de proveedor.
   
Los clientes pueden optar por configurar distintos tipos de proveedor para habilitar la recopilación de datos del componente correspondiente en su entorno de SAP. Por ejemplo, los clientes pueden configurar un proveedor para el tipo de proveedor de SAP HANA, otro proveedor para el tipo de proveedor de clúster de alta disponibilidad, etc.  

Los clientes también pueden optar por configurar varios proveedores de un tipo de proveedor específico para reutilizar el mismo recurso de monitor de SAP y el grupo administrado asociado. Obtenga más información sobre el grupo de recursos administrados. En la versión preliminar pública, se admiten los siguientes tipos de proveedor:   
- SAP HANA
- Clúster de alta disponibilidad
- Microsoft SQL Server

![Proveedores de Azure Monitor para soluciones de SAP](./media/azure-monitor-sap/azure-monitor-providers.png)

Se recomienda a los clientes configurar al menos un proveedor de los tipos de proveedor disponibles en el momento de implementar el recurso de supervisión de SAP. Mediante la configuración de un proveedor, los clientes inician la recopilación de datos del componente correspondiente para el que está configurado el proveedor.   

Si los clientes no configuran ningún proveedor en el momento de implementar el recurso de supervisión de SAP, aunque este se implemente correctamente, no se recopilarán datos de telemetría. Los clientes tienen la opción de agregar proveedores después de la implementación a través de un recurso de supervisión de SAP en Azure Portal. Los clientes pueden agregar o eliminar proveedores del recurso de supervisión de SAP en cualquier momento.

> [!Tip]
> Si quiere que Microsoft implemente un proveedor específico, deje comentarios a través del vínculo que aparece al final de este documento o póngase en contacto con su equipo de cuentas.  

## <a name="provider-type-sap-hana"></a>Tipo de proveedor de SAP HANA

Los clientes pueden configurar uno o varios proveedores del tipo *SAP HANA* para habilitar la recopilación de datos de la base de datos de SAP HANA. El proveedor de SAP HANA se conecta a la base de datos de SAP HANA a través del puerto de SQL, extrae datos de telemetría de la base de datos y los envía al área de trabajo de Log Analytics en la suscripción del cliente. El proveedor de SAP HANA recopila datos cada minuto de la base de datos de SAP HANA.  

En la versión preliminar pública, los clientes deberían ver los datos siguientes con el proveedor de SAP HANA: uso de la infraestructura subyacente, estado del host de SAP HANA, replicación del sistema de SAP HANA y datos de telemetría de la copia de seguridad de SAP HANA. Para configurar el proveedor de SAP HANA, se requiere la dirección IP del host, el número de puerto de SQL de HANA y el nombre de usuario y la contraseña de SYSTEMDB. Se recomienda a los clientes configurar el proveedor de SAP HANA con SYSTEMDB, pero se pueden configurar proveedores adicionales con otros inquilinos de base de datos.

![Proveedores de Azure Monitor para soluciones de SAP: SAP HANA](./media/azure-monitor-sap/azure-monitor-providers-hana.png)

## <a name="provider-type-high-availability-cluster"></a>Tipo de proveedor de clúster de alta disponibilidad
Los clientes pueden configurar uno o varios proveedores del tipo *clúster de alta disponibilidad* para habilitar la recopilación de datos de un clúster de Pacemaker en el entorno de SAP. El proveedor de clúster de alta disponibilidad se conecta a Pacemaker mediante el punto de conexión [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter), extrae datos de telemetría de la base de datos y los envía al área de trabajo de Log Analytics en la suscripción del cliente. El proveedor de clúster de alta disponibilidad recopila datos cada 60 segundos desde Pacemaker.  

En la versión preliminar pública, los clientes deberían ver los datos siguientes con el proveedor de clúster de alta disponibilidad:   
 - Estado del clúster representado como la acumulación del estado de los nodos y de los recursos 
 - [Otros](https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/doc/metrics.md) 

![Proveedores de Azure Monitor para soluciones de SAP: clúster de alta disponibilidad](./media/azure-monitor-sap/azure-monitor-providers-pacemaker-cluster.png)

Para configurar el proveedor de clúster de alta disponibilidad, debe llevar a cabo dos pasos principales: 
1. Instale [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) en *cada nodo* del clúster de Pacemaker. 
    - Los clientes pueden usar scripts de Azure Automation para implementar un clúster de alta disponibilidad. Los scripts instalarán [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) en cada nodo del clúster.  
    - Los clientes también pueden realizar la instalación de forma manual mediante los pasos descritos en [esta página](https://github.com/ClusterLabs/ha_cluster_exporter). 
2. Configure el proveedor de clúster de alta disponibilidad en *cada* nodo del clúster de Pacemaker.  
  Para configurar el proveedor de clúster de alta disponibilidad, se necesita la dirección URL de Prometheus, el nombre del clúster, el nombre del host y el identificador del sistema.   
  Se recomienda a los clientes configurar un proveedor por nodo del clúster.   

## <a name="provider-type-microsoft-sql-server"></a>Tipo de proveedor de Microsoft SQL Server

Los clientes pueden configurar uno o varios proveedores del tipo *Microsoft SQL Server* para habilitar la recopilación de datos de [SQL Server en Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/). El proveedor de SQL Server se conecta a Microsoft SQL Server a través del puerto de SQL, extrae datos de telemetría de la base de datos y los envía al área de trabajo de Log Analytics en la suscripción del cliente. Es necesario configurar SQL Server para la autenticación de SQL y crear un inicio de sesión de SQL Server, con la base de datos de SAP como base de datos predeterminada para el proveedor. El proveedor de SQL Server recopila datos de SQL Server en un intervalo que va de cada 60 segundos a cada hora.  

En la versión preliminar pública, los clientes deberían ver los datos siguientes con el proveedor de SQL Server: uso de la infraestructura subyacente, principales instrucciones SQL, tabla de mayor tamaño, problemas detectados en los registros de errores de SQL Server, procesos de bloqueo, etc.  

Para configurar el proveedor de Microsoft SQL Server, se necesitan el identificador del sistema SAP, la dirección IP del host y el número del puerto de SQL Server, así como el nombre de inicio de sesión y la contraseña de SQL Server.

![Proveedores de Azure Monitor para soluciones de SAP: SQL](./media/azure-monitor-sap/azure-monitor-providers-sql.png)

## <a name="next-steps"></a>Pasos siguientes

- Cree su primer recurso de Azure Monitor para soluciones de SAP.
- ¿Tiene alguna pregunta sobre Azure Monitor para soluciones de SAP? Consulte la sección de [preguntas más frecuentes](./azure-monitor-faq.md).
