---
title: Administración de dispositivos con Azure Portal | Microsoft Docs
description: Aprenda a usar Azure Portal para administrar dispositivos.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 05/28/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf3f6455be992502182fb942f0e6db089051ab1a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85253161"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Administración de identidades de dispositivos con Azure Portal

Con la administración de identidades de dispositivos en Azure Active Directory (Azure AD), puede garantizar que los usuarios tienen acceso a los recursos desde dispositivos que cumplen los estándares de seguridad y cumplimiento.

Este artículo:

- Da por hecho que está familiarizado con la [introducción a la administración de identidades de dispositivos en Azure Active Directory](overview.md)
- Proporciona información sobre la administración de identidades de dispositivos mediante el portal de Azure AD

![La vista Todos los dispositivos en Azure Portal](./media/device-management-azure-portal/all-devices-azure-portal.png)

## <a name="manage-device-identities"></a>Administración de identidades de dispositivo

El portal de Azure AD conforma una ubicación central desde la que pueden administrarse las identidades de los dispositivos. Para obtener esta ubicación, use un [vínculo directo](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) o:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** > **Dispositivos**.

La página **Dispositivos** le permitirá:

- Configuración de las opciones del dispositivo
- Búsqueda de dispositivos
- Realizar tareas de administración de identidades de dispositivos
- Consultar los registros de auditoría relacionados con el dispositivo  
  
## <a name="configure-device-settings"></a>Configuración de dispositivo

Para poder administrar las identidades de dispositivos en Azure Portal, los dispositivos deben estar [registrados o unidos](overview.md) a Azure AD. Como administrador, puede ajustar el proceso de registro y unión de dispositivos al configurar los valores del dispositivo.

La página de configuración del dispositivo le permite configurar las opciones relacionadas con las identidades de dispositivo:

![La configuración de dispositivos en relación con Azure AD](./media/device-management-azure-portal/device-settings-azure-portal.png)

- **Los usuarios pueden unir sus dispositivos a Azure AD**: esta opción le permite seleccionar qué usuarios podrán registrar sus dispositivos como dispositivos de unión a Azure AD. El valor predeterminado es **Todos**.

> [!NOTE]
> La opción **Los usuarios pueden inscribir dispositivos en Azure AD**  solo puede utilizarse en Unión a Azure AD en Windows 10.

- **Administradores locales adicionales en dispositivos unidos a Azure AD**: puede seleccionar a qué usuarios se conceden derechos de administrador local en un dispositivo. Los usuarios agregados aquí se agregan al rol *Administradores de dispositivos* de Azure AD. De forma predeterminada, a los administradores globales de Azure AD y a los propietarios de dispositivos se les conceden derechos de administrador local. Esta opción es una capacidad de la edición Premium disponible en productos como Azure AD Premium o Enterprise Mobility Suite (EMS).
- **Los usuarios pueden registrar sus dispositivos con Azure AD**: esta opción se debe configurar para que los dispositivos con Windows 10 personal, iOS, Android y macOS se puedan registrar en Azure AD. Si selecciona **Ninguno**, los dispositivos no podrán registrarse con Azure AD. La inscripción en Microsoft Intune o Administración de dispositivos móviles (MDM) para Office 365 exige registrarse. Si ha configurado alguno de estos servicios, se selecciona **TODOS** y **NINGUNO** no está disponible.
- **Requerir Multi-Factor Authentication para unir dispositivos**: puede decidir si se exige a los usuarios proporcionar un factor de autenticación adicional para unir su dispositivo a Azure AD. El valor predeterminado es **No**. Se recomienda exigir Multi-Factor Authentication al registrar un dispositivo. Antes de habilitar Multi-Factor Authentication para este servicio, debe asegurarse de que está configurado para los usuarios que registran sus dispositivos. Para más información sobre los distintos servicios de Azure Multi-Factor Authentication, vea [Introducción a Azure Multi-Factor Authentication](../authentication/concept-mfa-whichversion.md). 

> [!NOTE]
> La configuración de **Requerir Multi-factor Authentication para unir dispositivos** se aplica a los dispositivos que están unidos a Azure AD o registrados en Azure AD. Este parámetro no se aplica a los dispositivos unidos a Azure AD híbrido.

- **Número máximo de dispositivos**: esta opción permite seleccionar el número máximo de dispositivos unidos a Azure AD o registrados en Azure AD que puede tener un usuario en Azure AD. Si un usuario alcanza esta cuota, no puede agregar dispositivos adicionales hasta que se quitan uno o varios de los dispositivos existentes. El valor predeterminado es **20**.

> [!NOTE]
> La configuración del **número máximo de dispositivos** se aplica a los dispositivos que están unidos a Azure AD o registrados en Azure AD. Este parámetro no se aplica a los dispositivos unidos a Azure AD híbrido.

- **Los usuarios pueden sincronizar la configuración y los datos de aplicaciones en distintos dispositivos**: de forma predeterminada, esta opción se establece en **NINGUNO**. La selección de usuarios o grupos concretos o de TODOS permite sincronizar la configuración y los datos de aplicación del usuario en sus dispositivos Windows 10. Más información sobre cómo funciona la sincronización en Windows 10.
Esta opción es una capacidad Premium disponible en productos como Azure AD Premium o Enterprise Mobility Suite (EMS).

## <a name="locate-devices"></a>Búsqueda de dispositivos

Tiene dos opciones para buscar dispositivos registrados y unidos:

- **Todos los dispositivos** en la sección **Administrar** de la página **Dispositivos**.  
- **Dispositivos** en la sección **Administrar** de la página **Usuarios**.

Con ambas opciones, puede ir a una vista que:

- Permite buscar dispositivos si se usa el nombre para mostrar o el id. de dispositivo como filtro.
- Proporciona información general detallada de los dispositivos registrados y unidos.
- Permite realizar tareas comunes de administración de dispositivos.

>[!TIP]
>
>* Si ve un dispositivo "unido a Azure AD híbrido" con un estado "Pendiente" en la columna REGISTRADO, indica que el dispositivo se ha sincronizado desde Azure AD Connect y está esperando a completar el registro del cliente. Lea más información sobre cómo [planear la implementación de la unión a Azure AD híbrido](hybrid-azuread-join-plan.md). Puede encontrar más información en el artículo [Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory](faq.md).
>
>* Para algunos dispositivos iOS, los nombres de dispositivos que contienen apóstrofos podrían usar caracteres diferentes similares a los apóstrofos. Por tanto, buscar esos dispositivos resulta un poco complicado. Si no ve correctamente los resultados de la búsqueda, asegúrese de que la cadena de búsqueda contiene caracteres de apóstrofo que coincidan.

## <a name="device-identity-management-tasks"></a>Tareas de administración de identidades de dispositivos

Como administrador global o administrador de dispositivos en la nube, puede administrar los dispositivos registrados o unidos. Los administradores de servicios de Intune pueden:

- Actualizar dispositivos: algunos ejemplos son las operaciones diarias, como habilitar o deshabilitar dispositivos.
- Eliminar dispositivos: cuando un dispositivo se retira, se debe eliminar en Azure AD.

En esta sección, se proporciona información sobre las tareas comunes de administración de identidades de dispositivos.

### <a name="manage-an-intune-device"></a>Administración de un dispositivo de Intune

Si es administrador de Intune, puede administrar los dispositivos marcados como **Microsoft Intune**. Si el dispositivo no está inscrito con Microsoft Intune, la opción "Administrar" estará atenuada.

![Administración de un dispositivo de Intune](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Habilitar o deshabilitar un dispositivo de Azure AD

Para habilitar o deshabilitar un dispositivo, tiene dos opciones:

- En el menú de tareas ("...") en la página **Todos los dispositivos**.

   ![Administración de un dispositivo de Intune](./media/device-management-azure-portal/71.png)

- La barra de herramientas en la página **Dispositivos**.

   ![Administración de un dispositivo de Intune](./media/device-management-azure-portal/32.png)

**Observaciones**:

- Para poder habilitar o deshabilitar un dispositivo, tiene que ser administrador global o administrador de dispositivos en la nube de Azure AD. 
- Cuando se deshabilita un dispositivo, ya no puede autenticarse correctamente con Azure AD, lo que evita que pueda acceder a los recursos de Azure AD que están protegidos por la entidad emisora de certificados de dispositivo o mediante las credenciales de WH4B.
- Al deshabilitar el dispositivo, se revocará el token de actualización principal (PRT) y todos los tokens de actualización (RT) del dispositivo.

### <a name="delete-an-azure-ad-device"></a>Eliminar un dispositivo de Azure AD

Para eliminar un dispositivo, tiene dos opciones:

- En el menú de tareas ("...") en la página **Todos los dispositivos**.

   ![Administración de un dispositivo de Intune](./media/device-management-azure-portal/72.png)

- La barra de herramientas en la página **Dispositivos**.

   ![Eliminar un dispositivo](./media/device-management-azure-portal/34.png)

**Observaciones**:

- Para eliminar un dispositivo, debe ser administrador global o administrador de Intune en Azure AD.
- La eliminación de un dispositivo:
   - Evita que un dispositivo acceda a los recursos de Azure AD.
   - Quita todos los detalles asociados al dispositivo como, por ejemplo, las claves de BitLocker de los dispositivos Windows.  
   - Representa una actividad no recuperable y no se recomienda a menos que sea necesaria.

Si es otra entidad de administración la que administra un dispositivo (por ejemplo, Microsoft Intune), asegúrese de que el dispositivo se haya borrado o retirado antes de eliminarlo de Azure AD. Revise cómo [administrar dispositivos obsoletos](manage-stale-devices.md) antes de eliminar cualquier dispositivo.

### <a name="view-or-copy-device-id"></a>Ver o copiar el id. del dispositivo

Puede usar un id. de dispositivo para verificar los detalles del id.de dispositivo en ese dispositivo o usar PowerShell durante la solución de problemas. Para acceder a la opción de copia, haga clic en el dispositivo.

![Visualización de identificador de dispositivo](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>Ver o copiar las claves de BitLocker

Puede ver y copiar las claves de BitLocker para ayudar a otros usuarios a recuperar su unidad cifrada. Estas claves solo están disponibles para dispositivos Windows cifrados y con las claves almacenadas en Azure AD. Puede copiar estas claves al acceder a los detalles del dispositivo.

![Visualización de claves de BitLocker](./media/device-management-azure-portal/36.png)

Para ver o copiar las claves de BitLocker, debe ser el propietario del dispositivo, o bien un usuario que tenga al menos uno de los siguientes roles asignados:

- Administrador de dispositivos en la nube
- Administrador global
- Administrador del departamento de soporte técnico
- Administrador de servicios de Intune
- Administrador de seguridad
- Lector de seguridad

> [!NOTE]
> Los dispositivos Windows 10 híbridos unidos a Azure AD no tienen un propietario. Por lo tanto, si está buscando un dispositivo por propietario y no lo encontró, busque por el identificador de dispositivo.

### <a name="device-list-filtering-preview"></a>Filtrado de la lista de dispositivos (versión preliminar)

Antes, solo se podía filtrar la lista de dispositivos por actividad y estado habilitado. Esta vista previa permite ahora filtrar la lista de dispositivos de acuerdo con los siguientes atributos en un dispositivo:

- Estado habilitado
- Estado de cumplimiento
- Tipo de combinación (unido a Azure AD, unido a Azure AD híbrido, registrado en Azure AD)
- Marca de tiempo de actividad
- SO
- Tipo de dispositivo (impresoras, máquinas virtuales seguras, dispositivos compartidos, dispositivos registrados)

Para habilitar la funcionalidad de filtrado de versión preliminar en la vista **Todos los dispositivos**:

![Habilitación de la funcionalidad de versión preliminar de filtrado](./media/device-management-azure-portal/device-filter-preview-enable.png)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** > **Dispositivos**.
1. Seleccione el banner que dice **Pruebe las mejoras de la nueva experiencia de filtrado de dispositivos. Haga clic para habilitar la versión preliminar.**

Ahora podrá **agregar filtros** a la vista **Todos los dispositivos**.

## <a name="audit-logs"></a>Registros de auditoría

Las actividades de un dispositivo están disponibles a través de los registros de actividad. Estos registros contienen las actividades desencadenadas por el servicio de registro de dispositivos y los usuarios:

- Creación de dispositivos y adición de propietarios o usuarios al dispositivo.
- Cambios en la configuración de un dispositivo
- Operaciones de dispositivo como eliminar o actualizar un dispositivo

El punto de entrada a los datos de auditoría está en **Registros de auditoría**, que se encuentra en la sección **Actividad** de la página **Dispositivos**.

El registro de auditoría tiene una vista de lista predeterminada que muestra:

- La fecha y hora de la repetición
- Los destinos
- El iniciador o actor (quién) de una actividad
- La actividad (qué)

![Registros de auditoría](./media/device-management-azure-portal/63.png)

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Registros de auditoría](./media/device-management-azure-portal/64.png)

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de auditoría con los siguientes campos:

- Category
- Tipo de recurso de actividad
- Actividad
- Intervalo de fechas
- Destino
- Iniciado por (actor)

Además de los filtros, puede buscar entradas concretas.

![Registros de auditoría](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Pasos siguientes

[Procedimiento para administrar los dispositivos obsoletos en Azure AD](manage-stale-devices.md)
