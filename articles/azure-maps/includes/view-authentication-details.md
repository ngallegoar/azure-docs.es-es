---
title: Visualización de detalles de autenticación de Azure Maps
description: Use Azure Portal para ver detalles de autenticación de Azure Maps.
author: philmea
ms.author: philmea
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 9146844a6b83f78ad99ef7cd1aec4b028daf3ff6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988431"
---
Los detalles de autenticación de la cuenta de Azure Maps se pueden ver en Azure Portal. Una vez allí, vaya a su cuenta y seleccione **Autenticación** en el menú **Configuración**.

![Detalles de la autenticación](../media/how-to-manage-authentication/how-to-view-auth.png)

Una vez creada una cuenta de Azure Maps, se muestra el valor `x-ms-client-id` de Azure Maps en la página de detalles de autenticación de Azure Portal. Este valor representa la cuenta que se utilizará para las solicitudes a la API de REST. Este valor se debe almacenar en la configuración de la aplicación y recuperarse antes de realizar las solicitudes HTTP al usar la autenticación de Azure AD con Azure Maps.
