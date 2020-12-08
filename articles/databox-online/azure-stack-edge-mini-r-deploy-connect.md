---
title: 'Tutorial: Conexión a Azure Stack Edge Mini R en Azure Portal'
description: Aprenda cómo conectarse al dispositivo Azure Stack Edge Mini R mediante la interfaz de usuario web local.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/20/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge Mini R so I can use it to transfer data to Azure.
ms.openlocfilehash: fe76391a5cfce8d7d39e47131db108ab87e5aed5
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464993"
---
# <a name="tutorial-connect-to-azure-stack-edge-mini-r"></a>Tutorial: Conexión a Azure Stack Edge Mini R

Este tutorial describe cómo conectarse al dispositivo Azure Stack Edge Mini R mediante la interfaz de usuario web local.

El proceso de conexión puede tardar unos 5 minutos en completarse.

En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
>
> * Requisitos previos
> * A conectarse a un dispositivo físico



## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar el dispositivo de Azure Stack Edge, compruebe lo siguiente:

* Ha instalado el dispositivo físico como se detalla en [Instalación de Azure Stack Edge](azure-stack-edge-mini-r-deploy-install.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Conexión a la configuración de la interfaz de usuario web local

1. Configure el adaptador Ethernet en el equipo para conectarse al dispositivo de Azure Stack Edge Pro con la dirección IP estática 192.168.100.5 y la subred 255.255.255.0.

2. Conecte el equipo a PUERTO 1 en el dispositivo. Si conecta el equipo directamente al dispositivo (sin un conmutador), use un cable cruzado o un adaptador Ethernet USB. Use la siguiente ilustración para identificar el PUERTO 1 en el dispositivo.

    ![Cableado para Wi-Fi](./media/azure-stack-edge-mini-r-deploy-install/wireless-cabled.png)

[!INCLUDE [azure-stack-edge-gateway-delpoy-connect](../../includes/azure-stack-edge-gateway-deploy-connect.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió sobre lo siguiente:

> [!div class="checklist"]
> * Requisitos previos
> * A conectarse a un dispositivo físico


Para obtener información sobre cómo configurar los valores de red del dispositivo, consulte:

> [!div class="nextstepaction"]
> [Configuración de la red](./azure-stack-edge-mini-r-deploy-configure-network-compute-web-proxy.md)
