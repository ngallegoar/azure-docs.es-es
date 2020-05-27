---
title: Introducción a la API de etcd en Azure Cosmos DB
description: Este artículo proporciona una introducción y las ventajas principales de la API de etcd en Azure Cosmos DB
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: 16aac5c765c36c49919685ee58e8034786ddf1ae
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83797356"
---
# <a name="introduction-to-the-azure-cosmos-db-etcd-api-preview"></a>Introducción a la API de etcd en Azure Cosmos DB (versión preliminar)

Azure Cosmos DB es un servicio de base de datos con varios modelos y distribución global de Microsoft para aplicaciones críticas. Ofrece una distribución global inmediata, escalado elástico de rendimiento y almacenamiento, latencias de menos de 10 ms en el percentil 99 y alta disponibilidad garantizada, todo ello respaldado por acuerdos de nivel de servicio líderes del sector.

[Etcd](https://github.com/etcd-io/etcd) es un almacén distribuido de pares clave-valor. En [Kubernetes](https://kubernetes.io/), etcd se usa para almacenar el estado y la configuración de los clústeres de Kubernetes. Garantizar la disponibilidad, confiabilidad y rendimiento de etcd es crucial para el estado general del clúster, la escalabilidad, la disponibilidad de elasticidad y el rendimiento de un clúster de Kubernetes.

La API etcd de Azure Cosmos DB le permite usar Azure Cosmos DB como almacén de back-end de Azure Kubernetes. La API etcd de Azure Cosmos DB está actualmente en versión preliminar. Azure Cosmos DB implementa el protocolo de conexión etcd. Con la API de etcd en Azure Cosmos DB, los desarrolladores obtendrán automáticamente una instancia de Kubernetes altamente fiable, [disponible](high-availability.md) y [distribuida globalmente](distribute-data-globally.md). Esta API permite a los desarrolladores escalar la administración del estado de Kubernetes en un servicio de PaaS nativo totalmente administrado en la nube. 

> [!NOTE]
> A diferencia de otras API de Azure Cosmos DB, no se puede aprovisionar una cuenta de API de etcd mediante Azure Portal, la CLI o el SDK de Azure. Puede aprovisionar una cuenta de API de etcd implementando la plantilla de Resource Manager para obtener instrucciones detalladas, consulte el artículo [Procedimientos de uso de Azure Kubernetes con Azure Cosmos DB](bootstrap-kubernetes-cluster.md). La API de etcd en Azure Cosmos DB está actualmente en versión preliminar limitada. También puede [registrarse para obtener la versión preliminar](https://aka.ms/cosmosetcdapi-signup), rellenando el formulario de registro.

## <a name="wire-level-compatibility"></a>Compatibilidad de nivel de conexión

Azure Cosmos DB implementa el protocolo de transferencia de etcd versión 3 y permite que los servidores de API del [nodo principal](https://kubernetes.io/docs/concepts/overview/components/) usen Azure Cosmos DB del mismo modo que lo harían en un entorno de etcd instalado de forma local. La API de etcd admite la autenticación mutua de TLS. 

El siguiente diagrama muestra los componentes del clúster de Kubernetes. En el patrón de clúster, el servidor de la API usa la API de etcd de Azure Cosmos DB, en lugar del etcd instalado localmente. 

![Azure Cosmos DB implementando el protocolo de conexión etcd](./media/etcd-api-introduction/etcd-api-wire-protocol.png)

## <a name="key-benefits"></a>Ventajas principales

### <a name="no-etcd-operations-management"></a>Sin administración de las operaciones etcd

Como servicio en la nube nativo totalmente administrado, Azure Cosmos DB elimina la necesidad de Kubernetes de que los desarrolladores configuren y administren etcd. La API de etcd en Azure Cosmos DB es escalable, altamente disponible, tolerante a errores y ofrece un alto rendimiento. La sobrecarga de configurar la replicación en varios nodos, realizar las actualizaciones graduales, las revisiones de seguridad y supervisar el estado de etcd se controla mediante Azure Cosmos DB.

### <a name="global-distribution--high-availability"></a>Alta disponibilidad y distribución globales 

Mediante la API de etcd, Azure Cosmos DB garantiza una disponibilidad del 99,99 % para las lecturas de datos y los escribe en una sola región, y una disponibilidad del 99,999 % en varias regiones. 

### <a name="elastic-scalability"></a>Escalabilidad elástica

Azure Cosmos DB ofrece escalabilidad elástica para leer y escribir solicitudes de diferentes regiones.
A medida que crece el clúster de Kubernetes, la cuenta de la API de etcd en Azure Cosmos DB se amplía elásticamente sin tiempo de inactividad. Almacenar datos de etcd en Azure Cosmos DB, en lugar de en los nodos maestros de Kubernetes también permite un escalado más flexible del nodo maestro. 

### <a name="security--enterprise-readiness"></a>Seguridad y preparación para empresas

Cuando los datos de etcd se almacenan en Azure Cosmos DB, los desarrolladores de Kubernetes obtienen automáticamente [cifrado integrado en reposo](database-encryption-at-rest.md), [certificaciones y cumplimiento normativo](compliance.md) y [funcionalidades de copia de seguridad y restauración](online-backup-and-restore.md) compatibles con Azure Cosmos DB. 

## <a name="next-steps"></a>Pasos siguientes

* [Procedimientos de uso de Azure Kubernetes con Azure Cosmos DB](bootstrap-kubernetes-cluster.md)
* [Ventajas clave de Azure Cosmos DB](introduction.md)
* [Guía de inicio rápido del motor de AKS](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md)