---
title: Uso de detecciones de riesgo con el acceso condicional de Azure Active Directory
description: En este inicio rápido, aprenderá a configurar una directiva de acceso condicional de Azure Active Directory (Azure AD) para bloquear inicios de sesión en función de los riesgos de la sesión.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: be447b001b0b2e14999aac98ba2125f8cbfe9853
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77186614"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Inicio rápido: Bloqueo del acceso cuando se detecta riesgo en una sesión con el acceso condicional de Azure Active Directory  

Para mantener el entorno protegido, puede bloquear el inicio de sesión de los usuarios sospechosos. [Azure Active Directory (Azure AD) Identity Protection](../active-directory-identityprotection.md) analiza todos los inicios de sesión y calcula la probabilidad que hay de que el legítimo propietario de una cuenta de usuario no haya sido el que haya realizado un intento de inicio de sesión. Dicha probabilidad (baja, media, alta) se indica en forma de un valor calculado denominado [niveles de riesgo de inicio de sesión](concept-conditional-access-conditions.md#sign-in-risk). Al establecer la condición de riesgo del inicio de sesión, puede configurar una directiva de acceso condicional que responda a niveles de riesgo de inicio de sesión concretos.

En este inicio rápido se muestra cómo configurar una [directiva de acceso condicional](../active-directory-conditional-access-azure-portal.md) que bloquee un inicio de sesión cuando se haya detectado un nivel de riesgo de inicio de sesión configurado.

![Creación de directiva](./media/app-sign-in-risk/1000.png)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para completar el escenario de este tutorial, necesita:

- **Acceso a la edición Azure AD Premium P2**: dado que el acceso condicional es una funcionalidad de Azure AD Premium P1, necesita una edición de P2, ya que el escenario de este inicio rápido requiere Identity Protection.
- **Identity Protection**: el escenario de esta guía de inicio rápido requiere que se habilite Identity Protection. Si no sabe cómo habilitarlo, consulte [Habilitación de Azure Active Directory Identity Protection](../identity-protection/overview-identity-protection.md).
- **Tor Browser**: [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) está diseñado para ayudarle a mantener su privacidad en Internet. Identity Protection detecta un inicio de sesión desde Tor Browser como inicios de sesión desde direcciones IP anónimas, que tienen un nivel de riesgo medio. Para más información, consulte [Detecciones de riesgos de Azure Active Directory](../reports-monitoring/concept-risk-events.md).  
- **Una cuenta de prueba denominada Alain Charon**: si no sabe cómo crear una cuenta de prueba, consulte [Agregar usuarios basados en la nube](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Prueba del inicio de sesión

El objetivo de este paso es asegurarse de que la cuenta de prueba puede acceder a su inquilino mediante Tor Browser.

**Para probar el inicio de sesión:**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como **Alain Charon**.
1. Cierre la sesión.

## <a name="create-your-conditional-access-policy"></a>Creación de la directiva de acceso condicional

El escenario de esta guía de inicio rápido usa un inicio de sesión desde Tor Browser para generar la detección de riesgo **Inicios de sesión desde direcciones IP anónimas**. El nivel de riesgo de esta detección es medio. Para responder a esta detección de riesgo, la condición de riesgo de inicio de sesión se establece en medio. En un entorno de producción, debe establecer la condición de riesgo de inicio de sesión en alto o en medio y alto.

En esta sección se muestra cómo crear la directiva de acceso condicional necesaria. En la directiva, establezca:

| Configuración | Value |
| --- | --- |
| Usuarios y grupos | Alain Charon  |
| Aplicaciones de nube | Todas las aplicaciones en la nube |
| Riesgo de inicio de sesión | Media |
| Conceder | Bloquear acceso |

![Creación de directiva](./media/app-sign-in-risk/130.png)

**Para configurar la directiva de acceso condicional:**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. En Azure Portal, en la barra de navegación izquierda, haga clic en **Azure Active Directory**.

   ![Azure Active Directory](./media/app-sign-in-risk/02.png)

1. En la página **Azure Active Directory**, en la sección **Seguridad**, haga clic en **Acceso condicional**.

   ![Acceso condicional](./media/app-sign-in-risk/03.png)

1. En la página **Acceso condicional**, en la barra de herramientas de la parte superior, haga clic en **Agregar**.

   ![Nombre](./media/app-sign-in-risk/108.png)

1. La página **Nuevo**, en el cuadro de texto **Nombre**, escriba **Bloquear acceso si el nivel de riesgo es medio**.

   ![Nombre](./media/app-sign-in-risk/104.png)

1. En la sección **Asignación**, haga clic en **Usuarios y grupos**.

   ![Usuarios y grupos](./media/app-sign-in-risk/06.png)

1. En la página **Usuarios y grupos**:

   ![Acceso condicional](./media/app-sign-in-risk/107.png)

   1. Haga clic en **Seleccionar usuarios y grupos** y, luego, seleccione **Usuarios y grupos**.
   1. Haga clic en **Seleccionar**.
   1. En la página **Seleccionar**, seleccione **Alain Charon** y haga clic en **Seleccionar**.
   1. En la página **Usuarios y grupos**, haga clic en **Listo**.
1. Haga clic en **Aplicaciones en la nube**.

   ![Aplicaciones de nube](./media/app-sign-in-risk/08.png)

1. En la página **Aplicaciones en la nube**:

   ![Acceso condicional](./media/app-sign-in-risk/109.png)

   1. Haga clic en **All cloud apps** (Todas las aplicaciones en la nube).
   1. Haga clic en **Done**(Listo).
1. Haga clic en **Condiciones**.

   ![Controles de acceso](./media/app-sign-in-risk/19.png)

1. En la página **Condiciones**:

   ![Nivel de riesgo del inicio de sesión](./media/app-sign-in-risk/21.png)

   1. Haga clic en **Riesgo de inicio de sesión**.
   1. En **Configurar**, haga clic en **Sí**.
   1. En el nivel de riesgo de inicio de sesión, seleccione **Medio**.
   1. Haga clic en **Seleccionar**.
   1. En la página **Condiciones**, haga clic en **Listo**.
1. En la sección **Controles de acceso**, haga clic en **Conceder**.

   ![Controles de acceso](./media/app-sign-in-risk/10.png)

1. En la página **Conceder**:

   ![Acceso condicional](./media/app-sign-in-risk/105.png)

   1. Seleccione **Block access** (Bloquear acceso).
   1. Haga clic en **Seleccionar**.
1. En la sección **Habilitar directiva**, haga clic en **Activar**.

   ![Habilitar directiva](./media/app-sign-in-risk/18.png)

1. Haga clic en **Crear**.

## <a name="evaluate-a-simulated-sign-in"></a>Evaluación de un inicio de sesión simulado

Ahora que ha configurado la directiva de acceso condicional, probablemente quiera saber si funciona según lo previsto. Como primer paso, use la **herramienta de directivas What If** del acceso condicional para simular un inicio de sesión del usuario de prueba. La simulación calcula el impacto que este inicio de sesión tiene en las directivas y genera un informe de simulación.  

Al ejecutar la **herramienta de directivas What If** para este escenario, la opción **Block access for medium risk level** (Bloquear el acceso si el nivel de riesgo es medio) debe aparecer en **Policies that will apply** (Directivas que se van a aplicar).

![Usuario](./media/app-sign-in-risk/117.png)

**Para evaluar la directiva de acceso condicional:**

1. En la página [Acceso condicional - Directivas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies), en el menú de la parte superior, haga clic en **What If**.  

   ![What If](./media/app-sign-in-risk/14.png)

1. Haga clic en **Usuario**, seleccione **Alan Charon** en la página **Usuarios** y haga clic en **Seleccionar**.

   ![Usuario](./media/app-sign-in-risk/116.png)

1. En **Riesgo de inicio de sesión**, seleccione **Medio**.

   ![Usuario](./media/app-sign-in-risk/119.png)

1. Haga clic en **What If**.

## <a name="test-your-conditional-access-policy"></a>Prueba de la directiva de acceso condicional

En la sección anterior, ha aprendido a evaluar un inicio de sesión de simulado. Aparte de la simulación, también debe probar la directiva de acceso condicional para asegurarse de que funciona según lo previsto.

Para probar la directiva, pruebe a iniciar sesión en [Azure Portal](https://portal.azure.com) como **Alan Charon** mediante Tor Browser. La directiva de acceso condicional debe bloquear el intento de inicio de sesión.

![Multi-Factor Authentication](./media/app-sign-in-risk/118.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no sea necesario, elimine el usuario de prueba, Tor Browser y la directiva de acceso condicional:

- Si no sabe cómo eliminar un usuario de Azure AD, consulte [Eliminación de usuarios desde Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Para eliminar la directiva, selecciónela y, a continuación, haga clic en **Eliminar** en la barra de herramientas de acceso rápido.

   ![Multi-Factor Authentication](./media/app-sign-in-risk/33.png)

- Para obtener instrucciones para quitar Tor Browser, consulte [Uninstalling](https://tb-manual.torproject.org/uninstalling/) (Desinstalación).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solicitar la aceptación de las condiciones de uso](require-tou.md)
> [Requerir Multi-Factor Authentication para aplicaciones específicas](app-based-mfa.md)
