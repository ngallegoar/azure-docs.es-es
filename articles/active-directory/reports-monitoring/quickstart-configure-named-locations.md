---
title: Configuración de ubicaciones con nombre en Azure Active Directory | Microsoft Docs
description: Aprenda a configurar ubicaciones con nombre.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: df45ab0a7b1729ae6c1602c9769cd5b6da26f6ac
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74014355"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Inicio rápido: Configuración de ubicaciones con nombre en Azure Active Directory

Con las ubicaciones con nombre, puede etiquetar intervalos de direcciones IP de confianza en su organización. Azure AD usa las ubicaciones con nombre para:
- Detectar falsos positivos en las [detecciones de riesgo](concept-risk-events.md). Iniciar sesión desde una ubicación de confianza reduce el riesgo de inicio de sesión del usuario.   
- Configurar el [acceso condicional basado en la ubicación](../conditional-access/location-condition.md).

En este tutorial, obtendrá información sobre cómo configurar ubicaciones con nombre en su entorno.

## <a name="prerequisites"></a>Prerequisites

Para completar este inicio rápido necesita instalar:

* Un inquilino de Azure AD. Regístrese para una [evaluación gratuita](https://azure.microsoft.com/trial/get-started-active-directory/). 
* Un usuario, que es un administrador global del inquilino.
* Un intervalo IP que esté establecido y sea confiable en su organización. El intervalo IP debe estar en el formato **Enrutamiento de interdominios sin clases (CIDR)** .

## <a name="configure-named-locations"></a>Configuración de ubicaciones con nombre

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. En el panel de la izquierda, seleccione **Azure Active Directory** y luego seleccione **Acceso condicional** en la sección **Seguridad**.

    ![Pestaña Acceso condicional](./media/quickstart-configure-named-locations/entrypoint.png)

3. En la página **Acceso condicional**, seleccione **Ubicaciones con nombre** y luego **Nueva ubicación**.

    ![Ubicación con nombre](./media/quickstart-configure-named-locations/namedlocation.png)

6. Rellene el formulario de la página nueva. 

   * En el cuadro **Nombre**, escriba el nombre de la ubicación con nombre.
   * En **Intervalos IP**, escriba el intervalo IP en formato CIDR.  
   * Haga clic en **Crear**.
    
     ![La hoja Nuevo](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

- [Acceso condicional de Azure AD](../active-directory-conditional-access-azure-portal.md).
- [Condiciones de ubicación del acceso condicional de Azure AD](../conditional-access/location-condition.md).
- [Informe de inicios de sesión de riesgo](concept-risky-sign-ins.md)  
