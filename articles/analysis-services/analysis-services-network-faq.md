---
title: Preguntas frecuentes acerca de la conectividad de red de Analysis Services | Microsoft Docs
description: En este artículo se proporcionan respuestas a algunas de las preguntas más habituales acerca de la conectividad de red de Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b60cf34e8efed2ed63b6e35cfaf7445edb701610
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838508"
---
# <a name="frequently-asked-questions-about-analysis-services-network-connectivity"></a>Preguntas frecuentes acerca de la conectividad de red de Analysis Services

En este artículo se proporcionan respuestas a las preguntas habituales acerca de la conexión a cuentas de almacenamiento, orígenes de datos, firewalls y direcciones IP.

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

**Pregunta**: ¿Cómo puedo realizar copias de seguridad y restaurarlas mediante una cuenta de almacenamiento que está detrás de un firewall?   
**Respuesta**: Azure Analysis Services no utiliza direcciones IP fijas ni etiquetas de servicio. El intervalo de direcciones IP que usan los servidores de Analysis Services puede ser cualquier valor que esté en el intervalo de direcciones IP para la *región de Azure*. Como las direcciones IP del servidor son variables y pueden cambiar con el tiempo, las reglas del firewall deben permitir todo el intervalo de direcciones IP de la región de Azure en la que se encuentre el servidor.

**Pregunta**: Mi cuenta de Azure Storage se encuentra en una región distinta de la del servidor de Analysis Services. ¿Cómo establezco la configuración del firewall de la cuenta de almacenamiento?   
**Respuesta**: si la cuenta de almacenamiento está en otra región, configure el firewall de la cuenta de almacenamiento para permitir el acceso desde **Redes seleccionadas**. En el **intervalo de direcciones** del firewall, especifique el intervalo de direcciones IP para la región en la que se encuentra el servidor de Analysis Services. Para obtener los intervalos de direcciones IP de las regiones de Azure, consulte [Intervalos de direcciones IP de Azure y etiquetas de servicio: nube pública](https://www.microsoft.com/download/details.aspx?id=56519). Se admite la configuración del firewall de la cuenta de almacenamiento para permitir el acceso desde todas las redes, aunque es preferible elegir Redes seleccionadas y especificar un intervalo de direcciones IP. 

**Pregunta**: mi cuenta de Azure Storage se encuentra en la misma región que mi servidor de Analysis Services. ¿Cómo puedo configurar el firewall de la cuenta de almacenamiento?   
**Respuesta**: dado que el servidor de Analysis Services y la cuenta de almacenamiento se encuentran en la misma región, las comunicaciones entre ellos usan intervalos de direcciones IP internas, por lo que no se permite la configuración de un firewall para usar Redes seleccionadas y la especificación de un intervalo de direcciones IP. Si las directivas de la organización exigen un firewall, debe configurarse para permitir el acceso desde todas las redes.


## <a name="data-source-connections"></a>Conexiones a orígenes de datos

**Pregunta**: Tengo una red virtual para el sistema de origen de datos. ¿Cómo puedo permitir que los servidores de Analysis Services accedan a la base de datos desde la red virtual?   
**Respuesta**: Azure Analysis Services no puede unirse a una red virtual. La mejor solución aquí es instalar y configurar una puerta de enlace de datos local en la red virtual y, a continuación, configurar los servidores de Analysis Services con la propiedad del servidor **AlwaysUseGateway**. Para más información, consulte [Uso de la puerta de enlace para orígenes de datos en una red virtual de Azure (VNet)](analysis-services-vnet-gateway.md).

**Pregunta**: Tengo una base de datos de origen detrás de un firewall. ¿Cómo puedo configurar el firewall para permitir que el servidor de Analysis Services tenga acceso a él?   
**Respuesta**: Azure Analysis Services no utiliza direcciones IP fijas ni etiquetas de servicio. El intervalo de direcciones IP que usan los servidores de Analysis Services puede ser cualquier valor que esté en el intervalo de direcciones IP para la *región de Azure*. Debe proporcionar el *intervalo completo* de direcciones IP para la región de Azure del servidor en las reglas del firewall de la base de datos de origen. Otra alternativa, y posiblemente más segura, es configurar una puerta de enlace de datos local. Después, puede configurar los servidores de Analysis Services con la propiedad de servidor [AlwaysUseGateway](analysis-services-vnet-gateway.md#configure-alwaysusegateway-property)y, a continuación, asegurarse de que la puerta de enlace de datos local tenga una dirección IP permitida por las reglas de firewall del origen de datos.

## <a name="azure-apps-with-ip-address"></a>Aplicaciones de Azure con dirección IP

**Pregunta**: Uso una aplicación basada en Azure (por ejemplo, Azure Functions, Azure Data Factory) con una dirección IP que cambia sobre la marcha. ¿Cómo puedo administrar las reglas del firewall de Azure Analysis Services para permitir dinámicamente la dirección IP en la que se ejecuta la aplicación?   
**Respuesta**: Azure Analysis Services no admite vínculos privados, redes virtuales ni etiquetas de servicio. Hay algunas soluciones de código abierto (por ejemplo, https://github.com/mathwro/Scripts/blob/master/Azure/AllowAzure-AnalysisServer.ps1) ) que detectan la dirección IP de la aplicación cliente, y actualizan las reglas del firewall de forma automática y temporal.


## <a name="next-steps"></a>Pasos siguientes

[Instalación y configuración de una puerta de enlace de datos local](analysis-services-gateway-install.md)   
[Conexión a orígenes de datos locales con la puerta de enlace de datos local](analysis-services-gateway.md)   
[Uso de la puerta de enlace para orígenes de datos en una red virtual de Azure (VNet)](analysis-services-vnet-gateway.md)
