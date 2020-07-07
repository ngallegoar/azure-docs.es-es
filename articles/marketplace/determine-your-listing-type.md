---
title: 'Determinación de la opción de publicación: marketplace comercial de Microsoft'
description: En este artículo se describen los criterios de idoneidad y los requisitos para publicar ofertas en Microsoft AppSource y Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: 7c8a8bcef548ae752b6ddeb7663dd82b3f245349
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85957892"
---
# <a name="determine-your-publishing-option"></a>Determinar la opción de publicación

La opción de publicación que elige para su oferta está relacionada directamente con los requisitos de idoneidad y las ventajas de GTM de Marketplace comercial. Y, aún más importante, la selección de la opción de publicación y el tipo de oferta definen cómo interactúan los usuarios con su oferta de Marketplace comercial.

Para configurar su oferta, necesita comprender los siguientes conceptos clave de Marketplace comercial: las opciones de publicación, la configuración y los tipos de oferta, y las llamadas a la acción que rigen cómo y dónde se presenta la oferta en los escaparates de Marketplace comercial.

En este artículo, aprenderá lo siguiente:

- Cómo determinar el escaparate adecuado para su solución
- Qué opciones de publicación y llamadas a la acción están disponibles en cada escaparate
- Qué tipos de ofertas están disponibles para cada opción de publicación

## <a name="commercial-marketplace-publishing-options"></a>Opciones de publicación de Marketplace comercial

En la tabla siguiente se muestran las opciones de publicación de los tipos de oferta en Microsoft AppSource y Azure Marketplace.

|   | **Lista (contacto)**  | **Lista (evaluación)**  | **Gratis** | **BYOL** | **Transacción**|
| :--------- | :----------- | :------------ | :----------- | :---------- |:---------- |
| **Máquina virtual** |  |  |  | Azure Marketplace |  Azure Marketplace |
| **Aplicaciones de Azure (varias máquinas virtuales)** |  |  | Azure Marketplace | Azure Marketplace | Azure Marketplace  |
| **Imagen de contenedor** |  |  | Azure Marketplace | Azure Marketplace |   |
| **Módulo IoT Edge** |  |  | Azure Marketplace | Azure Marketplace |   |
| **Servicios administrados** |  |  |  | Azure Marketplace |   |
| **Servicios de consultoría** | Ambos escaparates |  |  |  |   |
| **Aplicación SaaS** | Ambos escaparates | Ambos escaparates | Ambos escaparates |  | Ambos escaparates* |
| **Aplicación de Microsoft 365** | AppSource | AppSource |  |  | AppSource**  |
| **Complemento de Dynamics 365** |  AppSource | AppSource |  |  |   |
| **PowerApps** | AppSource |AppSource  |  |  |   |

&#42;Las ofertas de transacción de aplicación SaaS en Microsoft AppSource actualmente son con tarjeta de crédito.

&#42;&#42; Las ofertas de Microsoft 365 se instalan gratis y se pueden monetizar a través de una oferta de SaaS como servicio de licencia. Para más información, consulte el artículo sobre cómo [monetizar el complemento de Office 365 a través de Marketplace comercial de Microsoft](/office/dev/store/monetize-addins-through-microsoft-commercial-marketplace).

## <a name="selecting-a-storefront"></a>Selección de un escaparate

Antes de seleccionar una opción de publicación, es importante comprender los requisitos de idoneidad de escaparate para las soluciones, las aplicaciones y los servicios de Marketplace comercial. Cada escaparate cumple con los requisitos únicos del cliente y apunta a audiencias específicas. El tipo de oferta, las funcionalidades de transacción y la categoría o el sector determinarán dónde publicar su oferta.

Las aplicaciones de **Microsoft AppSource** son soluciones de línea de negocio que se pueden basar en Azure o diseñar para: Dynamics 365, Office 365, Power BI o Power Apps. Los servicios de consultoría de AppSource son ofertas de servicios profesionales que ayudan a los clientes a empezar a usar Dynamics 365 y Power BI o a acelerar su uso.

Las aplicaciones de **Azure Marketplace** son soluciones técnicas de "bloques de creación" integradas en Azure o creadas para Azure y dirigidas a un público de desarrolladores o expertos en TI. Los servicios de consultoría de Azure Marketplace son ofertas de servicios profesionales que ayudan a los clientes a empezar a usar Azure o a acelerar su uso.

>[!Note]
>Listados cruzados (para aplicaciones de SaaS): si una oferta de compra o versión de evaluación cumple los criterios para un público de usuarios técnicos y empresariales, se publicará en ambos escaparates. A continuación se incluye más información sobre las opciones de publicación.

## <a name="choose-a-publishing-option"></a>Selección de la opción de publicación

Las opciones de publicación disponibles ofrecen un compromiso diferenciado del cliente y le ofrecen acceso al uso compartido de clientes potenciales y a [ventajas de Marketplace comercial](gtm-your-marketplace-benefits.md). Tenga en cuenta las llamadas a la acción que se corresponden con la opción de publicación:

| **Opción de publicación**    | **Descripción**  |
| :------------------- | :-------------------|
| **Lista** | Lista sencilla de la aplicación o servicio que permite a los usuarios de Marketplace comercial solicitarle que se conecte con el cliente a través de la llamada a la acción **Ponerse en contacto conmigo**. |
| **Versión de prueba** | Use Marketplace comercial para mejorar la detectabilidad y automatizar el aprovisionamiento de la experiencia de evaluación de la solución, permitiendo a los usuarios potenciales usar su experiencia dentro de la aplicación de Microsoft, SaaS o IaaS sin ningún costo extra durante un tiempo limitado antes de comprarla. Las llamadas a la acción que se usan para la opción de publicación de prueba son: **Evaluación gratuita** o **Versión de prueba**. |
| **BYOL** | Use Marketplace comercial para mejorar la detectabilidad y automatizar el aprovisionamiento de su solución, así como para completar la transacción financiera por separado. Los tipos de ofertas BYOL son ideales para las migraciones desde el entorno local a la nube. La llamada a la acción es **Obténgalo ahora**.
| **Transacción** | Las ofertas de Transact se venden a través de Marketplace comercial. Microsoft es responsable de la facturación y el cobro. La llamada a la acción es **Obténgalo ahora**.|

> [!Note]
> Cuando se usa la opción de publicación de transacciones, es importante comprender las consideraciones sobre precios, facturación y pago antes de seleccionar un tipo de oferta y crear la oferta. Para más información, revise el artículo sobre las [funcionalidades de transacción de Marketplace comercial](./marketplace-commercial-transaction-capabilities-and-considerations.md).

## <a name="next-steps"></a>Pasos siguientes

- Una vez que decida una opción de publicación, estará listo para [seleccionar el tipo de oferta](./publisher-guide-by-offer-type.md) que se usará para presentar la oferta.
- Revise los requisitos de idoneidad de las opciones de publicación por tipo de oferta para finalizar la selección y la configuración de la oferta.
- Revise los patrones de publicación por escaparate para ver ejemplos de cómo la solución se asigna a un tipo de oferta y configuración.
