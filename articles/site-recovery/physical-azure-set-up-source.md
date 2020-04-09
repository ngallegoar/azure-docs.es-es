---
title: Configuración del servidor de configuración para la recuperación ante desastres de servidores físicos en Azure mediante Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar el servidor de configuración local para la recuperación ante desastres de servidores físicos locales en Azure.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: ramamill
ms.openlocfilehash: 902c14211e91a1500c6b50cd790b347e337c4f70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229132"
---
# <a name="set-up-the-configuration-server-for-disaster-recovery-of-physical-servers-to-azure"></a>Configuración del servidor de configuración para la recuperación ante desastres de servidores físicos en Azure

En este artículo se describe cómo configurar el entorno local para comenzar a replicar servidores físicos que ejecutan Windows o Linux en Azure.

## <a name="prerequisites"></a>Prerrequisitos

En este artículo se supone que ya cuenta con lo siguiente:
- Almacén de Recovery Services en [Azure Portal](https://portal.azure.com "Portal de Azure").
- Un equipo físico para instalar el servidor de configuración.
- Si deshabilitó TLS 1.0 en la máquina en la que va a instalar al servidor de configuración, asegúrese de que TLs 1.2 esté habilitado y de que esté instalada la versión 4.6 o versiones posteriores de .NET Framework en la máquina (con la criptografía segura habilitada). [Más información](https://support.microsoft.com/help/4033999/how-to-resolve-azure-site-recovery-agent-issues-after-disabling-tls-1).

### <a name="configuration-server-minimum-requirements"></a>Requisitos mínimos del servidor de configuración
En la siguiente tabla se muestran los requisitos mínimos de hardware, software y red para un servidor de configuración.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> El servidor de configuración no admite los servidores proxy basados en HTTPS.

## <a name="choose-your-protection-goals"></a>Selección de los objetivos de protección

1. En Azure Portal, vaya a la hoja de los almacenes de **Recovery Services** y seleccione su almacén.
2. En el menú de **Recurso** del almacén, haga clic en **Introducción** > **Site Recovery** > **Paso 1: Preparar la infraestructura** > **Objetivo de protección**.

    ![Elegir objetivos](./media/physical-azure-set-up-source/choose-goals.png)
3. En **Objetivo de protección**, seleccione **To Azure** (En Azure) y, luego, **No virtualizado/otro**; después haga clic en **Aceptar**.

    ![Elegir objetivos](./media/physical-azure-set-up-source/physical-protection-goal.png)

## <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

1. En **Preparar origen**, si no tiene un servidor de configuración, haga clic en **+Servidor de configuración** para agregar uno.

   ![Configurar origen](./media/physical-azure-set-up-source/plus-config-srv.png)
2. En la hoja **Agregar servidor**, compruebe que **Servidor de configuración** aparezca en **Tipo de servidor**.
4. Descargue el archivo de instalación unificada de Site Recovery.
5. Descargue la clave de registro del almacén. Necesita la clave de registro cuando ejecuta la instalación unificada. La clave será válida durante cinco días a partir del momento en que se genera.

    ![Configurar origen](./media/physical-azure-set-up-source/set-source2.png)
6. En la máquina que usa como el servidor de configuración, ejecute la **instalación unificada de Azure Site Recovery** para instalar el servidor de configuración, el servidor de procesos y el servidor de destino maestro.

#### <a name="run-azure-site-recovery-unified-setup"></a>Ejecución de la instalación unificada de Azure Site Recovery

> [!TIP]
> El registro del servidor de configuración presenta errores si la hora del reloj del sistema de su equipo tiene más de cinco minutos de diferencia con respecto a la hora local. Sincronice el reloj del sistema con un [servidor horario](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) antes de comenzar la instalación.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> El servidor de configuración se puede instalar a través de una línea de comandos. [Más información](physical-manage-configuration-server.md#install-from-the-command-line).


## <a name="common-issues"></a>Problemas comunes

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Pasos siguientes

El paso siguiente implica [configurar el entorno de destino](physical-azure-set-up-target.md) en Azure.
