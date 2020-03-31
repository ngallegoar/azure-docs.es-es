---
title: Módulos de Azure IoT Edge
description: La oferta Módulo IoT Edge en Azure Marketplace para los publicadores de aplicaciones y servicios.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/22/2018
ms.author: dsindona
ms.openlocfilehash: aadbf33914f919e393a5ec88cf6fc0a6103911b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286156"
---
# <a name="iot-edge-modules"></a>Módulos de IoT Edge

La plataforma [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) está respaldada por Azure Cloud.  Esta plataforma permite a los usuarios implementar cargas de trabajo en la nube para ejecutarlas directamente en dispositivos IoT.  Un módulo IoT Edge puede ejecutar las cargas de trabajo sin conexión y realizar análisis de datos localmente. Este tipo de oferta ayuda a ahorrar ancho de banda, proteger datos locales y confidenciales y ofrecer un tiempo de respuesta de baja latencia.  Ahora tiene las opciones para aprovechar las ventajas de estas cargas de trabajo creadas previamente. Hasta ahora, solo estaban disponibles un puñado de soluciones propias de Microsoft.  Tenía que invertir tiempo y recursos en compilar sus propias soluciones personalizadas de IoT.

Al introducir los [módulos de IoT Edge en Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1), ahora tenemos un único destino para que los publicadores expongan y vendan sus soluciones a la audiencia de IoT. Los desarrolladores de IoT pueden en última instancia buscar y adquirir capacidades para acelerar el desarrollo de sus soluciones.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Principales ventajas de los módulos de IoT Edge en Azure Marketplace:

| **Para los publicadores**    | **Para los clientes (desarrolladores de IoT)**  |
| :------------------- | :-------------------|
| Llegue a millones de desarrolladores que buscan crear e implementar soluciones de IoT Edge.  | Cree una solución de IoT Edge con la confianza de usar de componentes probados y seguros. |
| Publique una vez y ejecute en cualquier hardware de IoT Edge que admita contenedores. | Reduzca el tiempo de comercialización al encontrar e implementar módulos de IoT Edge propios de Microsoft y de terceros para necesidades específicas. |
| Monetizar con opciones de facturación flexibles <ul> <li> Gratis y Traiga su propia licencia (BYOL). </li> </ul> | Realice compras con su elección de modelos de facturación. <ul> <li> Gratis y Traiga su propia licencia (BYOL). </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>¿Qué es un módulo IoT Edge?

Azure IoT Edge le permite implementar y administrar la lógica de negocios en el Edge a través de módulos. Los módulos de Azure IoT Edge son las unidades de cálculo más pequeñas que administra IoT Edge y pueden contener servicios de Microsoft (por ejemplo, Azure Stream Analytics), servicios de terceros o su propio código específico de la solución. Para obtener más información sobre los módulos de IoT Edge, vea [Información sobre los módulos de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).

**¿Cuál es la diferencia entre un tipo de oferta de contenedor y un tipo de oferta de módulo de IoT Edge?**

El tipo de oferta de módulo de IoT Edge es un tipo específico de contenedor que se ejecuta en un dispositivo de IoT Edge. Incluye valores de configuración predeterminados para ejecutarse en el contexto de IoT Edge y, opcionalmente, usa el SDK del módulo de IoT Edge que se integra con el tiempo de ejecución de IoT Edge.

## <a name="publishing-your-iot-edge-module"></a>Publicar el módulo de IoT Edge

**Seleccionar el escaparate correcto**

Los módulos de IoT Edge solo se publican en Azure Marketplace, no se aplica AppSource.  Para obtener más información sobre las diferencias y la audiencia de destino en escaparates, vea [Determinar la opción de publicación](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).
 
**Opciones de facturación**

Marketplace admite actualmente las opciones de facturación **Gratis** y **Traiga su propia licencia (BYOL)** para los módulos de IoT Edge.
 
**Opciones de publicación**

En todos los casos, los módulos de IoT Edge deben seleccionar la opción de publicación **Transact**.  Vea [Determinar la opción de publicación](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) para obtener más detalles sobre las opciones de publicación.  

## <a name="eligibility-criteria"></a>Criterios de elegibilidad

Todos los términos de los contratos de Microsoft Azure Marketplace y las directivas se aplican a las ofertas de módulo de IoT Edge.  Además, existen requisitos previos y requisitos técnicos para los módulos de IoT Edge.  

**Requisitos previos**

Para publicar un módulo de IoT Edge en Azure Marketplace, es preciso cumplir los siguientes requisitos previos:

- Acceso a Cloud Partner Portal (CPP). Para obtener más información, vea [Guía de publicación de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Hospedar el módulo de IoT Edge en Azure Container Registry. 
- Tener los metadatos del módulo IoT Edge listos como (lista no exhaustiva): 
    - Un título
    - Una descripción (en formato HTML)
    - Una imagen de logotipo (formato PNG y tamaños de imagen fijos, como 40 x 40 px, 90 x 90 px, 115 x 115 px, 255 x 115 px)
    - Un término de uso y directiva de privacidad
    - Configuración predeterminada del módulo (ruta, propiedades deseadas del gemelo, createOptions, variables de entorno)
    - Documentación
    - Contactos de soporte técnico

**Requisitos técnicos**

Los requisitos técnicos principales para que un módulo de IoT Edge pueda obtener la certificación y publicarse en Azure Marketplace se detallan en [Preparar los recursos técnicos del módulo IoT Edge](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets).  

## <a name="documentation-and-resources"></a>Documentación y recursos

[Creación de una oferta de módulo de IoT Edge](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-offer): los pasos para publicar una nueva oferta de módulo de IoT Edge con el Portal de publicación en la nube.

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho,

- [Regístrese](https://azuremarketplace.microsoft.com/sell) en Marketplace.

Si está registrado y está creando una oferta nueva o trabajando en una existente,

- Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/) para crear o completar su oferta.
- Consulte [Introducción a la publicación de ofertas de módulos IoT Edge](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts) para más información sobre cómo publicar una oferta de módulo de IoT Edge.
