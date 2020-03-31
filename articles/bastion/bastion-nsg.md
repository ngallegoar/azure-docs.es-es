---
title: Uso de máquinas virtuales y grupos de seguridad de red en Azure Bastion
description: En este artículo se describe cómo incorporar acceso a grupos de seguridad de red con Azure Bastion.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 15abee4688a2f6aefa2b08ad2b8eee6622d56be2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087276"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>Trabajo con acceso a grupos de seguridad de red y Azure Bastion

Al trabajar con Azure Bastion, puede usar grupos de seguridad de red (NSG). Para más información, consulte [Grupos de seguridad](../virtual-network/security-overview.md). 

![Architecture](./media/bastion-nsg/nsg-architecture.png)

En este diagrama:

* El host de Bastion se implementa en la red virtual.
* El usuario se conecta a Azure Portal con cualquier explorador HTML5.
* El usuario va a la máquina virtual de Azure para RDP/SSH.
* Integración de conexión: sesión de RDP/SSH con un solo clic dentro del explorador
* No se requiere ninguna dirección IP pública en la máquina virtual de Azure.

## <a name="network-security-groups"></a><a name="nsg"></a>Grupos de seguridad de red

En esta sección se muestra el tráfico de red entre el usuario y Azure Bastion, y para dirigirse a las máquinas virtuales de la red virtual:

### <a name="azurebastionsubnet"></a>AzureBastionSubnet

Azure Bastion se implementa en concreto en la subred AzureBastionSubnet.

* **Tráfico de entrada:**

   * **Tráfico de entrada procedente de Internet pública:** Azure Bastion creará una dirección IP pública que necesita el puerto 443 habilitado en la dirección IP pública para el tráfico de entrada. El puerto 3389/22 no tiene que abrirse en la subred AzureBastionSubnet.
   * **Plano de control del tráfico de entrada procedente de Azure Bastion:** Para la conectividad del plano de control, habilite el puerto 443 entrante desde la etiqueta de servicio de **GatewayManager**. De este modo, se permite que el plano de control, es decir, el administrador de puerta de enlace, pueda comunicarse con Azure Bastion.

* **Tráfico de salida:**

   * **Tráfico de salida a máquinas virtuales de destino:** Azure Bastion se comunicará con las máquinas virtuales de destino a través de la dirección IP privada. Los grupos de seguridad de red tienen que permitir el tráfico de salida a otras subredes de máquinas virtuales de destino para el puerto 3389 y 22.
   * **Salida del tráfico a otros puntos de conexión públicos de Azure:** Azure Bastion debe ser capaz de conectarse a varios puntos de conexión públicos dentro de Azure (por ejemplo, para almacenar registros de diagnóstico y los registros de medición). Por esta razón, Azure Bastion necesita una salida hacia 443 para la etiqueta de servicio **AzureCloud**.

* **Subred de VM de destino:** Se trata de una subred que contiene la máquina virtual de destino a la que quiere conectarse mediante RDP/SSH.

   * **Tráfico de entrada procedente de Azure Bastion:** Azure Bastion se comunicará con la máquina virtual de destino a través de la dirección IP privada. Los puertos RDP/SSH (puertos 3389/22, respectivamente) tienen que abrirse en la máquina virtual de destino a través de la dirección IP privada. Como procedimiento recomendado, puede agregar el intervalo de direcciones IP de la subred de Azure Bastion en esta regla para permitir que solo Bastion pueda abrir estos puertos en las máquinas virtuales de destino de la subred de la máquina virtual de destino.

## <a name="apply-nsgs-to-azurebastionsubnet"></a><a name="apply"></a>Aplicación de grupos de seguridad de red en AzureBastionSubnet

Si crea y aplica un grupo de seguridad de red a ***AzureBastionSubnet***, asegúrese de que ha agregado las siguientes reglas en él. Si no agrega estas reglas, se producirá un error en la creación o actualización de los grupos de seguridad de red:

* **Conectividad del plano de control:** entrante en 443 desde GatewayManager
* **Registro de diagnóstico y otros:** saliente en 443 a AzureCloud. Todavía no se admiten etiquetas regionales dentro de esta etiqueta de servicio.
* **Máquina virtual de destino:** saliente de 3389 y 22 a VirtualNetwork

Hay un ejemplo de regla de grupo de seguridad de red disponible como referencia en esta [plantilla de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azure-bastion-nsg).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Bastion, consulte las [preguntas frecuentes](bastion-faq.md).
