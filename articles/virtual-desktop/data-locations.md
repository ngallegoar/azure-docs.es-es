---
title: 'Ubicaciones de datos para Windows Virtual Desktop: Azure'
description: Una breve introducción a las ubicaciones en las que se almacenan los datos y metadatos de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5f48586f38c310c77c61f470d3e2fbc819c33f71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128043"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Ubicaciones de datos para Windows Virtual Desktop

Windows Virtual Desktop está disponible actualmente para todas las ubicaciones geográficas. Inicialmente, los metadatos del servicio solo se pueden almacenar en el área geográfica de Estados Unidos (EE. UU.). Los administradores pueden elegir la ubicación para almacenar los datos de usuario cuando creen las máquinas virtuales del grupo de hosts y los servicios asociados, como los servidores de archivos. Obtenga más información sobre las ubicaciones geográficas de Azure en el [mapa de centros de datos de Azure](https://azuredatacentermap.azurewebsites.net/).

>[!NOTE]
>Microsoft no controla ni limita las regiones en las que usted o sus usuarios pueden acceder a los datos específicos de la aplicación o del usuario.

>[!IMPORTANT]
>Windows Virtual Desktop almacena información de metadatos globales como los nombres de inquilinos, nombres de grupos de hosts, nombres de grupos de aplicaciones y nombres principales de usuario en un centro de datos ubicado en Estados Unidos. Los metadatos almacenados se cifran en reposo y los reflejos con redundancia geográfica se mantienen en Estados Unidos. Todos los datos del cliente, como la configuración de la aplicación y los datos de usuario, residen en la ubicación que el cliente elige y no los administra el servicio.

Los metadatos del servicio se replican en Estados Unidos con fines de recuperación ante desastres.