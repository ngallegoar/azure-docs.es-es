---
title: Direcciones IP de Azure Integration Runtime
description: Obtenga información sobre las direcciones IP a las que debe autorizar el tráfico de entrada con el fin de configurar correctamente los firewalls para proteger el acceso de red a los almacenes de datos.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/06/2020
ms.openlocfilehash: 598876e12fe04129692d0c9a842f4edb2ec00768
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086793"
---
# <a name="azure-integration-runtime-ip-addresses"></a>Direcciones IP de Azure Integration Runtime

Las direcciones IP que usa Azure Integration Runtime dependen de la región en la que se encuentra su entorno de ejecución de integración de Azure. *Todos* los entornos de ejecución de integración de Azure de una misma región usan los mismos intervalos de direcciones IP.

> [!IMPORTANT]  
> Actualmente, los flujos de datos no usan estas direcciones IP. 
>
> Puede usar estos intervalos IP para la ejecución de movimientos de datos, canalizaciones y actividades externas. Estos intervalos IP se pueden incluir en listas blancas de los almacenes de datos/grupos de seguridad de red (NSG)/firewalls para el acceso de entrada desde Azure Integration Runtime. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>Direcciones IP de Azure Integration Runtime: Regiones específicas

Permita el tráfico desde las direcciones IP enumeradas para el entorno de ejecución de integración de Azure en la región específica en la que se encuentran los recursos:

|                | Region              | Direcciones IP                                                 |
| -------------- | ------------------- | ------------------------------------------------------------ |
| Asia           | Este de Asia           | 20.189.104.128/25, </br>20.189.106.0/26, </br>13.75.39.112/28 |
| &nbsp;         | Sudeste de Asia      | 20.43.128.128/25, </br>20.43.130.0/26, </br>40.78.236.176/28 |
| Australia      | Este de Australia      | 20.37.193.0/25,</br>20.37.193.128/26,</br>13.70.74.144/28    |
| &nbsp;         | Sudeste de Australia | 20.42.225.0/25,</br>20.42.225.128/26,</br>13.77.53.160/28    |
| Brasil         | Sur de Brasil        | 191.235.224.128/25,</br>191.235.225.0/26,</br>191.233.205.160/28 |
| Canadá         | Centro de Canadá      | 52.228.80.128/25,</br>52.228.81.0/26,</br>13.71.175.80/28    |
| China          | Este de China 2        | 40.73.172.48/28,</br>52.130.0.128/25,</br>52.130.1.0/26      |
| Europa         | Norte de Europa        | 20.38.82.0/23,</br>20.38.80.192/26,</br>13.69.230.96/28      |
| &nbsp;         | Oeste de Europa         | 40.74.26.0/23,</br>40.74.24.192/26,</br>13.69.67.192/28      |
| Francia         | Centro de Francia      | 20.43.40.128/25,</br>20.43.41.0/26,</br>40.79.132.112/28     |
| India          | Centro de la India       | 52.140.104.128/25,</br>52.140.105.0/26,</br>20.43.121.48/28  |
| Japón          | Japón Oriental          | 20.43.64.128/25,</br>20.43.65.0/26,</br>13.78.109.192/28     |
| Corea          | Centro de Corea del Sur       | 20.41.64.128/25,</br>20.41.65.0/26,</br>52.231.20.64/28      |
| Sudáfrica   | Norte de Sudáfrica  | 102.133.124.104/29,</br>102.133.216.128/25,</br>102.133.217.0/26 |
| Reino Unido | Sur de Reino Unido 2            | 51.104.24.128/25,</br>51.104.25.0/26,</br>51.104.9.32/28     |
| Estados Unidos  | Centro de EE. UU.          | 20.37.154.0/23,</br>20.37.156.0/26,</br>20.44.10.64/28       |
|                | Este de EE. UU.             | 20.42.2.0/23,</br>20.42.4.0/26,</br>40.71.14.32/28           |
|                | Este de EE. UU. 2            | 20.41.2.0/23,</br>20.41.4.0/26,</br>20.44.17.80/28           |
|                | EUAP de Este de EE. UU. 2      | 20.39.8.128/26,</br>20.39.8.96/27,</br>40.75.35.144/28       |
|                | Centro-Norte de EE. UU    | 40.80.185.0/24,</br>40.80.186.0/25,</br>52.162.111.48/28      |
|                | Centro-sur de EE. UU.    | 40.119.9.0/25,</br>40.119.9.128/26,</br>13.73.244.32/28      |
|                | Centro-Oeste de EE. UU.     | 52.150.137.128/25,</br>52.150.136.192/26,</br>13.71.199.0/28 |
|                | Oeste de EE. UU.             | 40.82.250.0/23,</br>40.82.249.64/26,</br>13.86.219.208/28    |
|                | Oeste de EE. UU. 2            | 20.42.132.0/23,</br>20.42.129.64/26,</br>13.66.143.128/28    |
|                | US Gov - Virginia     | 52.127.45.96/28,</br>52.127.48.128/25,</br>52.127.49.0/26    |

## <a name="known-issue-with-azure-storage"></a>Problema conocido con Azure Storage

* Al conectarse a una cuenta de Azure Storage, las reglas de red IP no surten ningún efecto en las solicitudes que se originan en el entorno de ejecución de integración de Azure de la misma región que la cuenta de almacenamiento. Para más información, [consulte este artículo](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range). 

  En su lugar, se recomienda usar [servicios de confianza al conectarse a Azure Storage](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993). 

## <a name="next-steps"></a>Pasos siguientes

* [Consideraciones de seguridad para el movimiento de datos en Azure Data Factory](data-movement-security-considerations.md)
