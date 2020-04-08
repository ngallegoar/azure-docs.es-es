---
title: Acceso condicional
description: Aprenda a configurar el acceso condicional para Azure SQL Database y Azure Synapse.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.custom: sql-data-warehouse
ms.date: 02/06/2020
tag: azure-synpase
ms.openlocfilehash: cd56ccf2e6a4ceb0d81c25b5b9e795176be66b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124904"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-azure-synapse-analytics"></a>Acceso condicional (MFA) con Azure SQL Database y Azure Synapse Analytics

Azure [SQL Database](sql-database-technical-overview.md), [Instancia administrada](sql-database-managed-instance.md) y [Azure Synapse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) admiten el acceso condicional de Microsoft. 

> [!NOTE]
> Este tema se aplica al servidor de Azure SQL y a las instancias de SQL Database y Azure Synapse que se crean en dicho servidor. Para simplificar, se usa SQL Database cuando se hace referencia tanto a SQL Database como a Azure Synapse.

Los pasos siguientes muestran cómo configurar SQL Database para aplicar una directiva de Acceso condicional.  

## <a name="prerequisites"></a>Prerrequisitos  
- Debe configurar la instancia de SQL Database o el grupo de SQL en Azure Synapse para admitir la autenticación de Azure Active Directory. Para ver los pasos concretos, consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL Database o Azure Synapse](sql-database-aad-authentication-configure.md).  
- Cuando se habilita la autenticación multifactor, debe conectarse con una herramienta admitida, como la versión más reciente de SSMS. Para más información, consulte [Configuración de Multi-Factor Authentication de Azure SQL Database para SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Configuración de la CA para Azure SQL DB/DW  
1. Inicie sesión en Portal, seleccione **Azure Active Directory** y, después, **Acceso condicional**. Para más información, consulte [Referencia técnica del acceso condicional de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
   ![Hoja Acceso condicional](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. En la hoja **Conditional Access-Policies** (Acceso condicional: Directivas), haga clic en **Nueva directiva**, proporcione un nombre y haga clic en **Configure rules** (Configurar reglas).  
3. En **Asignaciones**, seleccione **Usuarios y grupos**, active **Seleccionar usuarios y grupos** y seleccione el usuario o el grupo para el acceso condicional. Haga clic en **Seleccionar** y en **Listo** para aceptar la selección.  
   ![Seleccionar usuarios y grupos](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. Seleccione **Aplicaciones en la nube** y haga clic en **Seleccionar aplicaciones**. Verá todas las aplicaciones disponibles para el acceso condicional. Seleccione **Azure SQL Database**, en la parte inferior, haga clic en **Seleccionar** y después en **Listo**.  
   ![Seleccionar SQL Database](./media/sql-database-conditional-access/select-sql-database.png)  
   Si no encuentra **Azure SQL Database** como aparece en la tercera captura de pantalla siguiente, realice estos pasos:   
   - Inicie sesión en la instancia de Azure SQL DB/DW mediante SSMS con una cuenta de administrador de AAD.  
   - Ejecute `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
   - Inicie sesión en AAD y compruebe que Azure SQL Database y Azure Synapse aparecen entre las aplicaciones en su instancia de AAD.  

5. Seleccione **Controles de acceso**, seleccione **Conceder** y active la directiva que desea aplicar. Para este ejemplo, se selecciona **Requerir autenticación multifactor**.  
   ![Seleccionar Conceder acceso](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Resumen  
La aplicación seleccionada (Azure SQL Database) que permite conectarse a Azure SQL DB/DW con Azure AD Premium ahora aplica la directiva de Acceso condicional seleccionada, **Requerir autenticación multifactor**.  
Si tiene preguntas sobre Azure SQL Database y Azure Synapse con respecto a la autenticación multifactor, póngase en contacto con MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Pasos siguientes  

Para ver un tutorial, consulte [Protección de bases de datos de Azure SQL](sql-database-security-tutorial.md).
