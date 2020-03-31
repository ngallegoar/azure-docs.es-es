---
title: Configuración de la cuenta de Azure Batch para Project Acoustics
titlesuffix: Azure Cognitive Services
description: Este procedimiento describe cómo configurar una cuenta de Azure Batch para su uso con integraciones de motores Unity y Unreal de Project Acoustics.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 8f0f726d9d23f20698d3510ad674331ad74fb703
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "68855075"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Configuración de la cuenta de Azure Batch para Project Acoustics
Este procedimiento describe cómo configurar una cuenta de Azure Batch para su uso con integraciones de motores Unity y Unreal de Project Acoustics.

## <a name="get-an-azure-subscription"></a>Obtener una suscripción de Azure
Se necesita una [suscripción de Azure](https://azure.microsoft.com/free/) antes de configurar cuentas de Batch y Storage. Si se suscribe por primera vez, Azure ofrece algunos recursos gratis por un tiempo limitado y USD 200 de crédito.

## <a name="create-azure-batch-and-storage-accounts"></a>Creación de cuentas de Azure Batch y Storage
A continuación, siga [estas instrucciones](https://docs.microsoft.com/azure/batch/batch-account-create-portal) para configurar sus cuentas de Azure Batch y sus cuentas asociadas de Azure Storage.

Elija opciones predeterminadas para las cuentas de Batch y Storage:
  
  ![Captura de pantalla de nuevas opciones de cuentas de Azure Batch que muestra la configuración predeterminada](media/new-batch-account-create.png)

  ![Captura de pantalla de nuevas opciones de cuentas de Azure Storage que muestra la configuración predeterminada](media/batch-storage-account-create.png)

Azure tarda unos minutos en implementar las cuentas. Busque una notificación de finalización en la esquina superior derecha del portal.
  
  ![Captura de pantalla de notificaciones implementadas de cuentas de Azure](media/batch-accounts-deploy-notification.png)

Las cuentas ahora deberían aparecer en el panel.
  
  ![Captura de pantalla del panel de Azure Portal que muestra una cuenta de Batch y Storage](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Configuración de la interfaz de usuario de elaboración con credenciales de Azure
Haga clic en el vínculo de la cuenta de Batch en el panel y luego en el vínculo **Keys** (Claves) en la página de la cuenta de Batch para acceder a sus credenciales.
  
  ![Captura de pantalla de cuenta de Azure Batch con vínculo a la página Claves resaltado](media/batch-access-keys.png)

  ![Captura de pantalla de la página de claves de cuenta de Azure Batch con claves de acceso](media/batch-keys-info.png)

Haga clic en el vínculo de la **cuenta de Storage** en la página para acceder a las credenciales de su cuenta de Azure Storage.
  
  ![Captura de pantalla de la página de claves de cuenta de Azure Storage con claves de acceso](media/storage-keys-info.png)

Escriba estas credenciales en el [complemento de simulación mediante "bake" de Unity](unity-baking.md) o el [complemento de simulación mediante "bake" de Unreal](unreal-baking.md).

## <a name="node-types-and-region-support"></a>Compatibilidad de región y tipos de nodo
Project Acoustics requiere nodos de máquinas virtuales de Azure optimizados para proceso de la serie Fsv2 y H que puede que no se admitan en todas las regiones de Azure. Revise [esta tabla](https://azure.microsoft.com/global-infrastructure/services) para asegurarse de elegir la ubicación correcta para la cuenta de Batch.
![Captura de pantalla que muestra Azure Virtual Machines por región](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Actualización de la cuota
Las cuentas de Azure Batch se aprovisionan al crear la cuenta con un límite de 20 núcleos de proceso. Es posible que quiera aumentar este límite para tener tiempos de simulación mediante "bake" más rápidos, porque puede paralelizar la carga de trabajo acústica en muchos nodos, hasta el número de puntos de sondeo de la escena. Para solicitar un aumento de cuota, haga clic en el vínculo **Cuota** en la página del portal de Azure Batch y luego en **Solicitar un aumento de la cuota**:

![Captura de pantalla de la página Cuota de Azure](media/azure-quotas.png)

## <a name="next-steps"></a>Pasos siguientes
* Integre el complemento de Project Acoustics en el proyecto de [Unity](unity-integration.md) o [Unreal](unreal-integration.md).

