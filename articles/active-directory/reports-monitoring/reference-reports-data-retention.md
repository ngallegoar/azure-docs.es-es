---
title: ¿Durante cuánto tiempo Azure AD almacena los datos de informes? | Microsoft Docs
description: Descubra durante cuánto tiempo Azure almacena los distintos tipos de datos de informes.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/24/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54636600c208f8f5df9fa2e25460c63dd9f46e85
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80239542"
---
# <a name="how-long-does-azure-ad-store-reporting-data"></a>¿Durante cuánto tiempo Azure AD almacena los datos de informes?


En este artículo, aprenderá sobre las directivas de retención de datos para los diferentes informes de actividad en Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>¿Cuándo Azure AD comienza la recopilación de datos?

| Edición de Azure AD | Inicio de la recopilación |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Cuándo suscribirse |
| Azure AD Free| La primera vez que abra la [hoja de Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) o use las [API de informes](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>¿Cuándo estarán disponibles los datos de actividad en Azure Portal?

- **Inmediatamente**: si ya ha estado trabajando con informes en Azure Portal.
- **Dentro de 2 horas**: si no ha activado la generación de informes en Azure Portal.

---

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>¿En cuánto tiempo podré ver los datos de las actividades después de obtener una licencia prémium?

Si ya tiene datos de actividades con la licencia gratuita, entonces los verá inmediatamente al actualizar. Si no tiene ningún dato, tardarán uno o dos días en mostrarse en los informes después de que actualice a una licencia prémium.

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>¿Puedo ver los datos del último mes después de obtener una licencia de Azure AD Premium?

Si ha cambiado recientemente a una versión prémium (incluida una versión de prueba), inicialmente, puede ver datos de hasta siete días. Cuando se acumulan datos, puede ver los datos de los últimos 30 días.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>¿Cuándo Azure AD comienza a recopilar datos de señales de seguridad?  

En el caso de las señales de seguridad, el proceso de recopilación se inicia cuando decide usar **Identity Protection Center**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>¿Durante cuánto tiempo Azure AD almacena los datos?

**Informes de actividad**    

| Informe                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Registros de auditoría             | 7 días        | 30 días             | 30 días             |
| Inicios de sesión               | 7 días        | 30 días             | 30 días             |
| Uso de Azure MFA        | 30 días       | 30 días             | 30 días             |

Puede conservar los datos de actividad de auditoría e inicio de sesión durante un tiempo mayor que el período de retención predeterminado que se ha descrito anteriormente si los enruta a una cuenta de almacenamiento de Azure con Azure Monitor. Para obtener más información, consulte [Archivado de registros de Azure AD en una cuenta de Azure Storage](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Señales de seguridad**

| Informe         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Usuarios en riesgo  | 7 días        | 30 días             | 90 días             |
| Inicios de sesión no seguros | 7 días        | 30 días             | 90 días             |

---
