---
title: 'Configuración de una directiva de acceso condicional en modo de solo informe: Azure Active Directory'
description: Uso del modo de solo informe en el acceso condicional para ayudar con la adopción
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/01/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb13eb699f770d4d7f896b8179888d1514b1c46d
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690364"
---
# <a name="configure-a-conditional-access-policy-in-report-only-mode"></a>Configuración de una directiva de acceso condicional en modo de solo informe

Para configurar una directiva de acceso condicional en modo de solo informe:

> [!IMPORTANT]
> Si su organización aún no lo ha hecho, realice la [Configuración de la integración de Azure Monitor con Azure AD](#set-up-azure-monitor-integration-with-azure-ad). Este proceso debe realizarse antes de que los datos estén disponibles para su revisión.

1. Inicie sesión en **Azure Portal** como administrador de acceso condicional, administrador de seguridad o administrador global.
1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Configure las condiciones de directiva y los controles de concesión requeridos según sea necesario.
1. En **Habilitar directiva**, establezca la opción en modo de **solo informe**.
1. Seleccione **Guardar**.

> [!TIP]
> Puede editar el estado **Habilitar directiva** de una directiva existente de **Activado** a **Solo informe**, pero al hacerlo se deshabilitará la aplicación de directivas. 

Consulte el resultado de solo consulta en los registros de inicio de sesión de Azure AD.

Para ver el resultado de una directiva de solo informe para un inicio de sesión determinado:

1. Inicie sesión en **Azure Portal** como lector de informes, lector de seguridad, administrador de seguridad o administrador global.
1. Vaya a **Azure Active Directory** > **Inicios de sesión**.
1. Seleccione un inicio de sesión o agregue filtros para restringir los resultados.
1. En el cajón **Detalles**, seleccione la pestaña **Solo informe** para ver las directivas evaluadas durante el inicio de sesión.

> [!NOTE]
> Al descargar los registros de inicios de sesión, seleccione formato JSON para incluir datos de resultados de solo informe de acceso condicional.

## <a name="set-up-azure-monitor-integration-with-azure-ad"></a>Configuración de la integración de Azure Monitor con Azure AD

Para ver el impacto agregado de las directivas de acceso condicional mediante el nuevo libro Conditional Access Insights, debe integrar Azure Monitor con Azure AD y exportar los registros de inicio de sesión. Hay dos pasos para configurar esta integración: 

1. [Regístrese para obtener una suscripción a Azure Monitor y cree un área de trabajo](/azure/azure-monitor/learn/quick-create-workspace).
1. [Exporte los registros de inicio de sesión de Azure AD a Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics).

Para más información sobre los precios de Azure Monitor, consulte la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/). Encontrará recursos para calcular los costos, establecer un límite diario o personalizar el período de retención de datos en el artículo [Administrar el uso y los costos con los registros de Azure Monitor](../../azure-monitor/platform/manage-cost-storage.md#estimating-the-costs-to-manage-your-environment).

## <a name="view-conditional-access-insights-workbook"></a>Consulta del libro Conditional Access Insights

Una vez que haya integrado sus registros de Azure AD con Azure Monitor, podrá supervisar el impacto de las directivas de acceso condicional mediante los nuevos libros Conditional Access Insights.

1. Inicie sesión en **Azure Portal** como administrador de seguridad o administrador global.
1. Vaya a **Azure Active Directory** > **Workbooks**.
1. Seleccione **Conditional Access Insights**.
1. Seleccione una o varias directivas en el menú desplegable **Directiva de acceso condicional**. Todas las directivas habilitadas están seleccionadas de forma predeterminada.
1. Seleccione un intervalo de tiempo (si el intervalo de tiempo supera el conjunto de datos disponible, el informe mostrará todos los datos disponibles). Una vez que haya establecido los parámetros **Directiva de acceso condicional** e **Intervalo de tiempo**, se cargará el informe.
   1. Opcionalmente, busque **usuarios** o **aplicaciones** para restringir el ámbito del informe.
1. Seleccione entre ver los datos en el intervalo de tiempo por el número de usuarios o el número de inicios de sesión.
1. Dependiendo de la **Vista de datos**, en el **Resumen de impacto** se muestra el número de usuarios o inicios de sesión en el ámbito de los parámetros elegidos, agrupados por Número total, **Correcto**, **Error**, **Se requiere una acción del usuario** y **No aplicado**. Seleccione un icono para examinar los inicios de sesión de un tipo de resultado determinado. 
   1. Si ha cambiado los parámetros del libro, puede elegir guardar una copia para un uso futuro. Seleccione el icono Guardar en la parte superior del informe y proporcione un nombre y una ubicación donde guardar.
1. Desplácese hacia abajo para ver el desglose de los inicios de sesión para cada condición.
1. Consulte los **Detalles de inicio de sesión** en la parte inferior del informe para investigar los eventos de inicio de sesión individuales filtrados por las selecciones anteriores.

> [!TIP] 
> ¿Es necesario explorar en profundidad una consulta determinada o exportar los detalles de inicio de sesión? Seleccione el botón situado a la derecha de cualquier consulta para abrir la consulta en Log Analytics. Seleccione Exportar para exportar a CSV o Power BI.

## <a name="common-problems-and-solutions"></a>Problemas y soluciones habituales

### <a name="why-are-the-queries-in-the-workbook-failing"></a>¿Por qué se producen errores en las consultas del libro?

Los clientes han observado que, en ocasiones, se producen errores en las consultas si varias áreas de trabajo o áreas de trabajo erróneas se asocian al libro. Para corregir este problema, haga clic en **Editar** en la parte superior del libro y, a continuación, en el icono con forma de engranaje de Configuración. Seleccione y, a continuación, quite las áreas de trabajo no asociadas al libro. Solo debe haber un área de trabajo asociada a cada libro.

### <a name="why-doesnt-the-conditional-access-policies-dropdown-parameter-contain-my-policies"></a>¿Por qué el parámetro del menú desplegable Directivas de acceso condicional no contiene mis directivas?

El menú desplegable Directivas de acceso condicional se rellena consultando los inicios de sesión más recientes durante un período de 4 horas. Si un inquilino no tiene ningún inicio de sesión en las últimas 4 horas, es posible que el menú desplegable esté vacío. Si este retraso es un problema persistente, como en los inquilinos pequeños con inicios de sesión poco frecuentes, los administradores pueden editar la consulta para el menú desplegable Directivas de acceso condicional y ampliar el tiempo para la consulta, de modo que supere las 4 horas.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

Para obtener más información acerca de los libros de Azure AD, consulte [Cómo usar los libros de Azure Monitor en informes de Azure Active Directory](../reports-monitoring/howto-use-azure-monitor-workbooks.md).
