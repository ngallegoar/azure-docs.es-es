---
title: Acceso condicional
description: Aprenda a configurar el acceso condicional de Azure SQL Database, Instancia administrada de Azure SQL y Azure Synapse Analytics.
titleSuffix: Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.topic: how-to
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.custom: sqldbrb=1
ms.date: 04/28/2020
tag: azure-synpase
ms.openlocfilehash: ad80f68c1ab3b3583c5a22de49b77211571f345e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91443986"
---
# <a name="conditional-access-with-azure-sql-database-and-azure-synapse-analytics"></a>Acceso condicional con Azure SQL Database y Azure Synapse Analytics

[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

[Azure SQL Database](sql-database-paas-overview.md), [Instancia administrada de Azure SQL](../managed-instance/sql-managed-instance-paas-overview.md) y [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) admiten el acceso condicional de Microsoft.

Los siguientes pasos muestran cómo configurar Azure SQL Database, Instancia administrada de SQL o Azure Synapse para aplicar una directiva de acceso condicional.  

## <a name="prerequisites"></a>Requisitos previos

- Azure SQL Database, Instancia administrada de Azure SQL o el grupo de Azure SQL de Azure Synapse deben estar configurados para admitir la autenticación de Azure Active Directory (Azure AD). Para ver los pasos concretos, consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL Database o Azure Synapse](authentication-aad-configure.md).  
- Si Multi-Factor Authentication está habilitado, debe conectarse con una herramienta admitida, como la versión más reciente de SQL Server Management Studio (SSMS). Para más información, consulte [Configuración de Multi-Factor Authentication de Azure SQL Database para SQL Server Management Studio](authentication-mfa-ssms-configure.md).  

## <a name="configure-conditional-access"></a>Configure el acceso condicional

> [!NOTE]
> En el siguiente ejemplo se usa Azure SQL Database, pero deberá seleccionar el producto adecuado para el que quiera configurar el acceso condicional.

1. Inicie sesión en Azure Portal, seleccione **Azure Active Directory** y, después, **Acceso condicional**. Para más información, consulte [Referencia técnica del acceso condicional de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
   ![Hoja Acceso condicional](./media/conditional-access-configure/conditional-access-blade.png)

2. En la hoja **Conditional Access-Policies** (Acceso condicional: Directivas), haga clic en **Nueva directiva**, proporcione un nombre y haga clic en **Configure rules** (Configurar reglas).  
3. En **Asignaciones**, seleccione **Usuarios y grupos**, active **Seleccionar usuarios y grupos** y seleccione el usuario o el grupo para el acceso condicional. Haga clic en **Seleccionar** y en **Listo** para aceptar la selección.  
   ![Seleccionar usuarios y grupos](./media/conditional-access-configure/select-users-and-groups.png)  

4. Seleccione **Aplicaciones en la nube** y haga clic en **Seleccionar aplicaciones**. Verá todas las aplicaciones disponibles para el acceso condicional. Seleccione **Azure SQL Database**, en la parte inferior, haga clic en **Seleccionar** y después en **Listo**.  
   ![Seleccionar SQL Database](./media/conditional-access-configure/select-sql-database.png)  
   Si no encuentra **Azure SQL Database** como aparece en la tercera captura de pantalla siguiente, realice estos pasos:
   - Conéctese a la base de datos en Azure SQL Database por medio de SSMS usando una cuenta de administrador de Azure AD.  
   - Ejecute `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
   - Inicie sesión en Azure AD y compruebe que Azure SQL Database, Instancia administrada de SQL o Azure Synapse aparezcan entre las aplicaciones en su instancia de Azure AD.  

5. Seleccione **Controles de acceso**, seleccione **Conceder** y active la directiva que desea aplicar. Para este ejemplo, se selecciona **Requerir autenticación multifactor**.  
   ![Seleccionar Conceder acceso](./media/conditional-access-configure/grant-access.png)  

## <a name="summary"></a>Resumen

Ahora, la aplicación seleccionada (Azure SQL Database) con Azure AD Premium aplica la directiva de acceso condicional seleccionada, **Requerir autenticación multifactor**.

Si tiene preguntas sobre Azure SQL Database y Azure Synapse con respecto a la autenticación multifactor, póngase en contacto con <MFAforSQLDB@microsoft.com>.  

## <a name="next-steps"></a>Pasos siguientes  

Para ver un tutorial, consulte [Protección de una base de datos de Azure SQL Database](secure-database-tutorial.md).
