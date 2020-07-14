---
title: Configuración de una aplicación virtual de red en Azure HDInsight
description: Aprenda a configurar otras características adicionales para el dispositivo virtual de red en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: 805be8d5c9ab4f6316251adbb9bce3e99f4fa01d
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086677"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>Configuración de una aplicación virtual de red en Azure HDInsight

> [!Important]
> La siguiente información **solo** es necesaria si desea configurar una aplicación virtual de red (NVA) distinta a Azure Firewall.

Azure Firewall está configurado automáticamente para permitir el tráfico para muchos de los escenarios comunes más importantes. Si usa otra aplicación virtual de red, tendrá que configurar algunas características adicionales. Tenga en cuenta los siguientes factores al configurar la aplicación virtual de red:

* Los servicios que admiten puntos de conexión de servicio pueden configurarse con puntos de conexión de servicio, lo que provoca la omisión de NVA, normalmente para consideraciones de costos o rendimiento.
* Las dependencias de dirección IP son para tráfico que no sea HTTP/HTTPS (tráfico TCP y UDP).
* Los puntos de conexión HTTP/HTTPS de FQDN se pueden incluir en la lista blanca del dispositivo NVA.
* Asigne la tabla de rutas que creó a la subred de HDInsight.

## <a name="service-endpoint-capable-dependencies"></a>Dependencias compatibles con los puntos de conexión de servicio

Opcionalmente, puede habilitar uno o varios de los siguientes puntos de conexión de servicio, lo que provocará que se omita el NVA. Esta opción puede ser útil para grandes cantidades de transferencias de datos con el fin de ahorrar costos y también para optimizar el rendimiento. 

| **Punto de conexión** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>Dependencias de dirección IP

| **Punto de conexión** | **Detalles** |
|---|---|
| Las direcciones IP se publican [aquí](hdinsight-management-ip-addresses.md) | Estas direcciones IP sirven para el plano de control de HDInsight y deben incluirse en el UDR para evitar el enrutamiento asimétrico. |
| Direcciones IP privadas de AAD-DS | Solo es necesario para los clústeres de ESP.|


### <a name="fqdn-httphttps-dependencies"></a>Dependencias HTTP/HTTPS de FQDN

> [!Important]
> La lista siguiente proporciona solo algunos de los FQDN más importantes. Puede obtener la lista completa de FQDN (principalmente, Azure Storage y Azure Service Bus) para configurar NVA [en este archivo](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json). Estas dependencias las usan las operaciones del plano de control de HDInsight para crear un clúster correctamente.

| **Punto de conexión**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Pasos siguientes

* [Uso del firewall para restringir el tráfico de salida](./hdinsight-restrict-outbound-traffic.md)
* [Arquitectura de red virtual de Azure HDInsight](hdinsight-virtual-network-architecture.md)
