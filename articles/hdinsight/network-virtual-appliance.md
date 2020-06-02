---
title: Configuración de una aplicación virtual de red en Azure HDInsight
description: Aprenda a configurar otras características adicionales para el dispositivo virtual de red en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: cbc2104ae3c55ae3670867b7a253d812f3a4be0e
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868281"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>Configuración de una aplicación virtual de red en Azure HDInsight

> [!Important]
> La siguiente información **solo** es necesaria si desea configurar una aplicación virtual de red (NVA) distinta a Azure Firewall.

Azure Firewall está configurado automáticamente para permitir el tráfico para muchos de los escenarios comunes más importantes. Si usa otra aplicación virtual de red, tendrá que configurar algunas características adicionales. Tenga en cuenta los siguientes factores al configurar la aplicación virtual de red:

* Los servicios compatibles con puntos de conexión de servicio deben configurarse con puntos de conexión de servicio.
* Las dependencias de dirección IP son para tráfico que no sea HTTP/HTTPS (tráfico TCP y UDP).
* Los puntos de conexión HTTP/HTTPS de FQDN se pueden colocar en el dispositivo NVA.
* Los puntos de conexión HTTP/HTTPS de carácter comodín son dependencias que pueden variar en función de varios calificadores.
* Asigne la tabla de rutas que creó a la subred de HDInsight.

## <a name="service-endpoint-capable-dependencies"></a>Dependencias compatibles con los puntos de conexión de servicio

| **Punto de conexión** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>Dependencias de dirección IP

| **Punto de conexión** | **Detalles** |
|---|---|
| \*:123 | Comprobación de reloj NTP. El tráfico se comprueba en varios puntos de conexión en el puerto 123. |
| Las direcciones IP se publican [aquí](hdinsight-management-ip-addresses.md) | Estas direcciones IP son el servicio HDInsight |
| Direcciones IP privadas de AAD-DS para clústeres de ESP |
| \*:16800 para la activación de Windows de KMS |
| \*12000 para Log Analytics |

### <a name="fqdn-httphttps-dependencies"></a>Dependencias HTTP/HTTPS de FQDN

> [!Important]
> La lista siguiente proporciona solo algunos de los FQDN más importantes. Puede obtener FQDN adicionales (principalmente, Azure Storage y Azure Service Bus) para configurar NVA [en este archivo](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Punto de conexión**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Pasos siguientes

* [Uso del firewall para restringir el tráfico de salida](./hdinsight-restrict-outbound-traffic.md)
* [Arquitectura de red virtual de Azure HDInsight](hdinsight-virtual-network-architecture.md)
