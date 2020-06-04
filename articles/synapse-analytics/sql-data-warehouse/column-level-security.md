---
title: ¿Qué es la seguridad de nivel de columna de Azure Synapse?
description: La seguridad de nivel de columna permite a los clientes controlar el acceso a las columnas de tablas de base de datos según el contexto de ejecución del usuario o la pertenencia a un grupo, lo cual permite simplificar el diseño y la codificación de seguridad de la aplicación, e implementar restricciones en el acceso a las columnas.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/19/2020
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: b0a783ad5db86ca783ff1cebceec8d77ab528047
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687921"
---
# <a name="column-level-security"></a>Seguridad de nivel de columna

La seguridad de nivel de columna permite a los clientes controlar el acceso a columnas de tablas según el contexto de ejecución del usuario o su pertenencia a grupos.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
Desde que se publicó este vídeo, la [seguridad de nivel de fila](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) también está disponible en Azure Synapse.

La seguridad de nivel de columna simplifica el diseño y la codificación de la seguridad de la aplicación y esto le permite restringir el acceso a las columnas para proteger información confidencial. Por ejemplo, garantiza que determinados usuarios puedan acceder solo a ciertas columnas de una tabla pertenecientes a su departamento. La lógica de la restricción de acceso está ubicada en el nivel de base de datos en lugar de estar alejado de los datos en otro nivel de aplicación. La base de datos aplica las restricciones de acceso cada vez que se intenta acceder a los datos desde cualquier nivel. Esta restricción hace que la seguridad resulte más sólida y confiable, ya que reduce el área expuesta del sistema de seguridad global. Además, la seguridad de nivel de columna también elimina la necesidad de introducir vistas para filtrar las columnas a fin de imponer restricciones de acceso a los usuarios.

Puede implementar este tipo de seguridad con la instrucción [GRANT](/sql/t-sql/statements/grant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) de T-SQL. Con este mecanismo, se admite tanto la autenticación de SQL como la de Azure Active Directory (AAD).

![cls](./media/column-level-security/cls.png)

## <a name="syntax"></a>Sintaxis

```syntaxsql
GRANT <permission> [ ,...n ] ON
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]
    TO <database_principal> [ ,...n ]
    [ WITH GRANT OPTION ]
    [ AS <database_principal> ]
<permission> ::=
    SELECT
  | UPDATE
<database_principal> ::=
      Database_user
    | Database_role
    | Database_user_mapped_to_Windows_User
    | Database_user_mapped_to_Windows_Group
```

## <a name="example"></a>Ejemplo

En el ejemplo siguiente, se muestra cómo impedir que `TestUser` acceda a la columna `SSN` de la tabla `Membership`:

Cree la tabla `Membership` con la columna SSN destinada a almacenar números del seguro social:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

Permita que `TestUser` acceda a todas las columnas excepto a la columna SSN, que contiene información confidencial:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

Las consultas ejecutadas como `TestUser` producirán un error si incluyen la columna SSN:

```sql
SELECT * FROM Membership;

-- Msg 230, Level 14, State 1, Line 12
-- The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Casos de uso

Estos son algunos ejemplos de cómo se usa la seguridad de nivel de columna en la actualidad:

- Una empresa de servicios financieros solo permite a los administradores de cuentas obtener acceso a los números del seguro social (SSN), números de teléfono y otra información de identificación personal (PII) del cliente.
- Un profesional sanitario solo permite a los médicos y a las enfermeras acceder a los historiales médicos confidenciales mientras que impide ver estos datos a los miembros del departamento de facturación.
