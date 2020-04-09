---
title: Creación y administración de aplicaciones de Azure IoT Central desde el portal de CSP | Microsoft Docs
description: Como CSP, cómo crear una aplicación de Azure IoT Central en nombre del cliente.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: how-to
manager: philmea
ms.openlocfilehash: 02481d5dcbaba15c9b17a27348207d9af64f3355
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982045"
---
# <a name="create-and-manage-an-azure-iot-central-application-from-the-csp-portal"></a>Creación y administración de una aplicación de Azure IoT Central desde el portal de CSP

El programa Proveedor de soluciones en la nube (CSP) de Microsoft es un programa de revendedores de Microsoft. Su intención es proporcionar a nuestros partners de canal un solo programa para distribuir todos los servicios en línea comerciales de Microsoft. Más información acerca del [programa Proveedor de soluciones en la nube](https://partner.microsoft.com/cloud-solution-provider).

Como CSP, puede crear y administrar aplicaciones de Microsoft Azure IoT Central en nombre de los clientes a través del [Centro de partners de Microsoft](https://partnercenter.microsoft.com/partner/home). Cuando los CSP crean las aplicaciones de Azure IoT Central en nombre de los clientes, como ocurre con otros servicios de Azure administrados por ellos, administran la facturación para los clientes. Un cargo para Azure IoT Central aparecerá en su factura total en el Centro de partners de Microsoft.

Para empezar, inicie sesión en su cuenta de Microsoft Partner Portal y seleccione un cliente para el que desee crear una aplicación de Azure IoT Central. Vaya a Administración del servicio para el cliente desde la barra de navegación izquierda.

![Centro de partners de Microsoft, vista de cliente](media/howto-create-and-manage-applications-csp/image1.png)

Azure IoT Central aparece como servicio disponible para su administración. Seleccione el vínculo de Azure IoT Central de la página para crear nuevas aplicaciones o administrar las aplicaciones existentes para este cliente.

![Azure IoT Central disponible para su administración](media/howto-create-and-manage-applications-csp/image2.png)

Va a la página Administrador de aplicaciones de Azure IoT Central. Azure IoT Central mantiene el contexto de que vino del Centro de partners de Microsoft para administrar ese cliente concreto. Puede ver cómo aparece esto confirmado en el encabezado de la página Administrador de aplicaciones. Desde aquí puede ir a una aplicación existente que había creado anteriormente para que este cliente la administrara, o bien crear una nueva aplicación para el cliente.

![Creación de un administrador para los CSP](media/howto-create-and-manage-applications-csp/image3.png)

Para crear una aplicación de Azure IoT Central, seleccione **Compilar** en el menú izquierdo. Elija una de las plantillas del sector o **Aplicación personalizada** para crear una aplicación desde cero. De esta manera se cargará la página de creación de aplicaciones. Debe completar todos los campos de esta página y luego eligir **Crear**. A continuación encontrará más información sobre cada uno de los campos.

![Página Crear aplicación para los CSP](media/howto-create-and-manage-applications-csp/image4.png)

![Página Crear aplicación para los CSP](media/howto-create-and-manage-applications-csp/image4-1.png)

![Página Crear aplicación para la información de facturación de los CSP](media/howto-create-and-manage-applications-csp/image4-2.png)

## <a name="pricing-plan"></a>Plan de precios

Solo puede crear aplicaciones que usen un plan de tarifa estándar como CSP. Para presentar Azure IoT Central al cliente, puede crear una aplicación que use el plan de tarifa gratis por separado. Obtenga más información sobre los planes de tarifa gratis y estándar en la [página de precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

Solo puede crear aplicaciones que usen un plan de tarifa estándar como CSP. Para presentar Azure IoT Central al cliente, puede crear una aplicación que use el plan de tarifa gratis por separado. Obtenga más información sobre los planes de tarifa gratis y estándar en la [página de precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="application-name"></a>Nombre de la aplicación

El nombre de la aplicación se muestra en la página **Administrador de aplicaciones** y dentro de cada aplicación de Azure IoT Central. Puede elegir cualquier nombre para la aplicación de Azure IoT Central. Elija un nombre que tenga sentido para usted y para otros usuarios de la organización.

## <a name="application-url"></a>Dirección URL de la aplicación

La dirección URL de la aplicación es el vínculo a la aplicación. Puede guardar un marcador para ella en el explorador o compartirla con otros usuarios.

Cuando escriba el nombre de la aplicación, la dirección URL de la misma se genera automáticamente. Si lo prefiere, puede elegir una dirección URL diferente para la aplicación. Cada dirección URL de Azure IoT Central debe ser única dentro de Azure IoT Central. Verá un mensaje de error si la dirección URL que elige ya se está utilizando.

## <a name="directory"></a>Directorio

Como Azure IoT Central tiene el contexto de que vino para administrar el cliente que seleccionó en Microsoft Partner Portal, verá solo el inquilino de Azure Active Directory para el cliente en el campo Directorio. 

Un inquilino de Azure Active Directory contiene identidades de usuario, credenciales y otra información de la organización. Se pueden asociar varias suscripciones de Azure a un solo inquilino de Azure Active Directory.

Para más información, consulte [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Suscripción de Azure

Una suscripción de Azure permite crear instancias de los servicios de Azure. Azure IoT Central busca automáticamente todas las suscripciones de Azure del cliente a las que usted tiene acceso y las muestra en una lista desplegable en la página **Crear aplicación**. Elija una nueva suscripción de Azure para crear una nueva aplicación de Azure IoT Central.

Si no tiene una suscripción de Azure, puede crear una en el Centro de partners de Microsoft. Después de crear la suscripción de Azure, vuelva a la página **Crear aplicación**. La nueva suscripción aparecerá en el cuadro de lista desplegable **Suscripción de Azure**.

Para más información, consulte [Suscripciones de Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="location"></a>Location

**Ubicación** es la [zona geográfica](https://azure.microsoft.com/global-infrastructure/geographies/) en la que desea crear la aplicación. Normalmente, se debe elegir la ubicación más cercana físicamente a los dispositivos para un rendimiento óptimo. Actualmente, se puede crear una aplicación de IoT Central en las zonas geográficas **Australia**, **Asia Pacífico**, **Europa**, **Estados Unidos**, **Reino Unido** y **Japón**. Una vez que elija una ubicación, no puede mover la aplicación a otra más adelante.

## <a name="application-template"></a>Plantilla de la aplicación

Seleccione la plantilla de aplicación que desea utilizar para la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear una aplicación de Azure IoT Central como CSP, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)
