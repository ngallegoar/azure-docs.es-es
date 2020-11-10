---
title: Administración de Azure Analysis Services | Microsoft Docs
description: En este artículo se describen las herramientas que se usan para controlar las tareas de administración y gestión de un servidor de Azure Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 91ffcac98b2b919a8fc131d235e699aad4fa215d
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93078907"
---
# <a name="manage-analysis-services"></a>Administración de Analysis Services
Una vez que se ha creado un servidor de Analysis Services en Azure, es posible que haya que realizar varias tareas de administración inmediatamente o en algún momento del futuro. Por ejemplo, ejecutar el procesamiento de los datos de la actualización, controlar quién puede acceder a los modelos del servidor o supervisar el estado del servidor. Varias de estas tareas solo se pueden realizar en Azure Portal, otras en SQL Server Management Studio (SSMS) y otras se pueden realizar indistintamente en ambos.

## <a name="azure-portal"></a>Portal de Azure
[Azure Portal](https://portal.azure.com/) es el lugar donde se puede crear y eliminar servidores, supervisar los recursos de los servidores, y cambiar el tamaño de los servidores y administrar quién tiene acceso a ellos.  Si tiene problemas, también puede enviar una solicitud de soporte técnico.

![Captura de pantalla que muestra Azure Portal, donde se pueden crear y eliminar servidores, supervisar los recursos de servidor, cambiar el tamaño de los servidores y administrar quién tiene acceso a ellos.](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
En Azure, la conexión a los servidores se realiza de la misma forma que la conexión a una instancia de servidor en una organización. Desde SSMS se pueden realizar muchas de las mismas tareas, como procesar datos o crear una script de procesamiento, administrar roles y usar PowerShell.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Descarga e instalación de SSMS
Para obtener las últimas características y la experiencia más completa al conectarse al servidor de Azure Analysis Services, asegúrese de estar usando la versión más reciente de SSMS. 

[Descargue SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>Para conectarse con SSMS
 Si se usa SSMS, antes de conectarse al servidor por primera vez, asegúrese de que su nombre de usuario se incluye en el grupo de administradores de Analysis Services. Para más información, consulte la sección[Administradores del servidor y usuarios de la base de datos](#server-administrators-and-database-users) en este mismo artículo.

1. Antes de establecer la conexión, es necesario obtener el nombre del servidor. En **Azure Portal** > servidor > **Información general** > **Nombre de servidor** , copie el nombre del servidor.
   
    ![Obtención del nombre del servidor en Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. En SSMS > **Explorador de objetos** , haga clic en **Conectar** > **Analysis Services**.
3. En el cuadro de diálogo **Conectarse al servidor** , pegue el nombre del servidor y en **Autenticación** , elija uno de los siguientes tipos de autenticación:   
    > [!NOTE]
    > Se recomienda el tipo de autenticación **Active Directory - Universal compatible con MFA**.

    > [!NOTE]
    > Si inicia sesión mediante una cuenta Microsoft, Windows Live ID, Yahoo, Gmail, etc., deje el campo de la contraseña en blanco. Le pedirán la contraseña cuando haga clic en Conectar.

    **Autenticación de Windows** para usar las credenciales de dominio o nombre de usuario y contraseña de Windows.

    **Autenticación de contraseña de Active Directory** para usar una cuenta de organización. Por ejemplo, al establecer conexión desde un equipo unido que no es de dominio.

    **Active Directory - Universal compatible con MFA** para utilizar [un método de autenticación no interactivo o multifactor](../azure-sql/database/authentication-mfa-ssms-overview.md). 
   
    ![Conectar en SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Administradores del servidor y usuarios de la base de datos
En Azure Analysis Services hay dos tipos de usuarios: los administradores del servidor y los usuarios de la base de datos. Ambos tipos de usuarios deben estar en su instancia de Azure Active Directory y se deben especificar mediante la dirección de correo electrónico profesional o UPN. Para más información, consulte [Permisos de usuario y autenticación](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Solución de problemas de conexión
Cuando se conecta mediante SSMS, si experimenta problemas, puede que deba borrar la caché de inicio de sesión. Nada se almacena en caché en el disco. Para borrar la caché, cierre y reinicie el proceso de conexión. 

## <a name="next-steps"></a>Pasos siguientes
Si aún no ha implementado un modelo tabular en el nuevo servidor, ahora es buen momento para hacerlo. Para más información, consulte [Deploy to Azure Analysis Services](analysis-services-deploy.md) (Implementación en Azure Analysis Services).

Si ha implementado un modelo en un servidor, está listo para conectarse a él mediante un cliente o un explorador. Para más información, consulte [Get data from Azure Analysis Services](analysis-services-connect.md) (Obtención de datos del servidor de Azure Analysis Services).