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
ms.openlocfilehash: 9451b6636f2f87806e3d1e39fec4e9e4d4390485
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706822"
---
# <a name="data-residency-for-azure-network-watcher"></a>Residencia de datos en Azure Network Watcher
A excepción del servicio Connection Monitor (versión preliminar), Azure Network Watcher no almacena datos de los clientes.


## <a name="connection-monitor-preview-data-residency"></a>Residencia de datos de Connection Monitor (versión preliminar)
El servicio Connection Monitor (versión preliminar) almacena datos de los clientes. Network Watcher almacena automáticamente estos datos en una sola región. Por tanto, Connection Monitor (versión preliminar) satisface automáticamente los requisitos de residencia de datos en la región, incluidos los que se especifican en el [Centro de confianza](https://azuredatacentermap.azurewebsites.net/).

## <a name="singapore-data-residency"></a>Residencia de datos en Singapur

En Azure, la característica que permite almacenar los datos de clientes en una sola región solo está disponible actualmente en la región Sudeste Asiático (Singapur) de la geoárea Asia Pacífico. En todas las demás regiones, los datos del cliente se almacenan en la geoárea. Para más información, consulte el [Centro de confianza](https://azuredatacentermap.azurewebsites.net/).

## <a name="next-steps"></a>Pasos siguientes

* Lea la información general de [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).
