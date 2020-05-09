---
title: Script para habilitar Kdump en SAP HANA (instancias grandes) | Microsoft Docs
description: Script para habilitar Kdump en SAP HANA (instancias grandes), HLI tipo I, HLI tipo II
services: virtual-machines-linux
documentationcenter: ''
author: prtyag
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/30/2020
ms.author: prtyag
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 16dc15b4369904643d0138a4b8e5b94c47868d31
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204781"
---
# <a name="enable-kdump-service"></a>Habilitar el servicio Kdump

En este documento se describen los detalles sobre cómo habilitar el servicio Kdump en una instancia grande de HANA en Azure (**tipo I y tipo II**).

## <a name="supported-skus"></a>SKU compatibles

|  Tipo de instancia grande de HANA   |  Proveedor del SO   |  Versión del paquete del SO   |  SKU        |
|-----------------------------|--------------|-----------------------|-------------|
|   Tipo I                    |  SuSE        |   SLES 12 SP3         |  S224 m      |
|   Tipo I                    |  SuSE        |   SLES 12 SP4         |  S224 m      |
|   Tipo I                    |  SuSE        |   SLES 12 SP2         |  S72m       |
|   Tipo I                    |  SuSE        |   SLES 12 SP3         |  S72m       |
|   Tipo I                    |  SuSE        |   SLES 12 SP2         |  S96        |
|   Tipo I                    |  SuSE        |   SLES 12 SP3         |  S96        |
|   Tipo II                   |  SuSE        |   SLES 12 SP3         |  S384       |
|   Tipo II                   |  SuSE        |   SLES 12 SP3         |  S576m      |
|   Tipo II                   |  SuSE        |   SLES 12 SP3         |  S384xm     |
|   Tipo II                   |  SuSE        |   SLES 12 SP4         |  S384xm     |

## <a name="prerequisites"></a>Prerrequisitos

- El servicio Kdump usa el directorio `/var/crash` para escribir volcados; asegúrese de que la partición correspondiente a este directorio tenga espacio suficiente para alojar los volcados.

## <a name="setup-details"></a>Detalles de la configuración

- El script para habilitar Kdump se puede encontrar [aquí](https://github.com/Azure/sap-hana/blob/master/tools/enable-kdump.sh).

- Ejecute este script en la instancia grande de HAN con el domando siguiente.

    > [!NOTE]
    > Se necesita el privilegio sudo para ejecutar este comando.

    ```bash
    sudo bash enable-kdump.sh
    ```

- Si el comando que genera Kdump está habilitado correctamente, reinicie el sistema para aplicar el cambio y, después, el Kdump se habilita correctamente. Reinicie el sistema para aplicar los cambios.

- Si el resultado del comando no puede realizar una operación determinada, como Exiting!!!!, el servicio Kdump no está habilitado. Consulte la sección [Problema de soporte técnico](#support-issue).

## <a name="test-kdump"></a>Prueba de Kdump

> [!NOTE]
>  La operación siguiente desencadenará un bloqueo del kernel y el reinicio del sistema.

- Desencadenar un bloqueo del kernel

    ```bash
    echo 1 > /proc/sys/kernel/sysrq
    echo c > /proc/sysrq-trigger
    ```

- Una vez que el sistema se reinicie correctamente, compruebe el directorio `/var/crash` para buscar los registros de bloqueo del kernel.

- Si `/var/crash` tiene un directorio con la fecha actual, el Kdump se habilita correctamente.

## <a name="support-issue"></a>Problema de soporte técnico

Si el script produce un error o el Kdump no está habilitado, envíe una solicitud de servicio al equipo de soporte técnico de Microsoft con los detalles siguientes.

* Identificador de la suscripción de HLI

* Nombre de servidor

* Proveedor del SO

* Versión del SO

* Versión del kernel
