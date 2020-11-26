---
title: Residencia de datos en Azure Network Watcher | Microsoft Docs
description: Este artículo le ayudará a comprender la residencia de datos para el servicio Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2020
ms.author: damendo
ms.openlocfilehash: 2e6a92a4d08f1603f480a990ad437a90302a8189
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94966095"
---
# <a name="data-residency-for-azure-network-watcher"></a>Residencia de datos en Azure Network Watcher
A excepción del servicio Connection Monitor (versión preliminar), Azure Network Watcher no almacena datos de los clientes.


## <a name="connection-monitor-preview-data-residency"></a>Residencia de datos de Connection Monitor (versión preliminar)
El servicio Connection Monitor (versión preliminar) almacena datos de los clientes. Network Watcher almacena automáticamente estos datos en una sola región. Por tanto, Connection Monitor (versión preliminar) satisface automáticamente los requisitos de residencia de datos en la región, incluidos los que se especifican en el [Centro de confianza](https://azuredatacentermap.azurewebsites.net/).

## <a name="data-residency"></a>Residencia de datos
En Azure, la característica que permite almacenar los datos de clientes en una única región solo está disponible actualmente en la región de Sudeste Asiático (Singapur) de la geoárea Asia Pacífico y en la región Sur de Brasil (estado de Sao Paulo) de la geoárea Brasil. En todas las demás regiones, los datos del cliente se almacenan en la geoárea. Para más información, consulte el [Centro de confianza](https://azuredatacentermap.azurewebsites.net/).

## <a name="next-steps"></a>Pasos siguientes

* Lea la información general de [Network Watcher](./network-watcher-monitoring-overview.md).