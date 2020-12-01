---
title: ActiveDirectoryInteractive se conecta a SQL
description: Ejemplo de código de C#, con explicaciones, para conectarse a Azure SQL Database mediante el modo SqlAuthenticationMethod.ActiveDirectoryInteractive.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory, has-adal-ref, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: vanto
ms.date: 04/23/2020
ms.openlocfilehash: 93831ec4c1dc3e34c2ea144e71b67dae711ee870
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94841655"
---
# <a name="connect-to-azure-sql-database-with-azure-ad-multi-factor-authentication"></a>Conexión a Azure SQL Database con Multi-Factor Authentication de Azure AD
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

En este artículo se proporciona un programa de C# que conecta con Azure SQL Database. El programa usa la autenticación de modo interactivo, que admite [Multi-Factor Authentication de Azure AD](../../active-directory/authentication/concept-mfa-howitworks.md).

Para más información sobre la compatibilidad con Multi-Factor Authentication de las herramientas de SQL, consulte [Compatibilidad de Azure Active Directory con SQL Server Data Tools (SSDT)](/sql/ssdt/azure-active-directory).

## <a name="multi-factor-authentication-for-azure-sql-database"></a>Multi-Factor Authentication para Azure SQL Database

A partir de la versión 4.7.2 de .NET Framework, la enumeración [`SqlAuthenticationMethod`](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) tiene un valor nuevo: `ActiveDirectoryInteractive`. En un programa cliente de C#, el valor de enumeración indica al sistema que use el modo interactivo de Azure Active Directory (Azure AD) que admite Multi-Factor Authentication para conectarse a Azure SQL Database. El usuario que ejecuta el programa ve los cuadros de diálogos siguientes:

* Un cuadro de diálogo que muestra el nombre de usuario de Azure AD y pide la contraseña del usuario.

   Si el dominio del usuario está federado con Azure AD, este cuadro de diálogo no aparece porque no se necesita contraseña.

   Si la directiva de Azure AD impone Multi-Factor Authentication al usuario, se muestran los dos cuadros de diálogo siguientes.

* La primera vez que un usuario pasa por Multi-Factor Authentication, el sistema muestra un cuadro de diálogo que pide un número de teléfono móvil al que enviar mensajes de texto. Cada mensaje proporciona el *código de verificación* que el usuario debe especificar en el cuadro de diálogo siguiente.

* Un cuadro de diálogo que solicita un código de verificación de Multi-Factor Authentication, que el sistema ha enviado a un teléfono móvil.

Para información sobre cómo configurar Azure AD para exigir Multi-Factor Authentication, consulte [Introducción a Multi-Factor Authentication de Azure AD en la nube](../../active-directory/authentication/howto-mfa-getstarted.md).

Para obtener capturas de pantalla de estos cuadros de diálogo, consulte [Configuración de la autenticación multifactor para SQL Server Management Studio y Azure AD](authentication-mfa-ssms-configure.md).

> [!TIP]
> Puede buscar en las API de .NET Framework con la [página de la herramientas del Explorador de API .NET](/dotnet/api/).
>
> También puede buscar directamente con el parámetro [?term=&lt;valor de búsqueda&gt;](/dotnet/api/?term=SqlAuthenticationMethod) opcional.

## <a name="configure-your-c-application-in-the-azure-portal"></a>Configuración de la aplicación de C# en Azure Portal

Antes de empezar, debe haber un [servidor SQL lógico](logical-servers.md) disponible.

### <a name="register-your-app-and-set-permissions"></a>Registro de la aplicación y establecimiento de permisos

Para usar la autenticación de Azure AD, el programa de C# se debe registrar como una aplicación de Azure AD. Para registrar una aplicación, debe ser administrador de Azure AD o usuario asignado al rol *Desarrollador de aplicaciones* de Azure AD. Para más información sobre la asignación de roles, consulte [Asignación de roles de administrador y de no administrador a usuarios con Azure Active Directory](../../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

Al completar el registro de una aplicación, se genera y se muestra un **identificador de la aplicación**. El programa debe incluir este identificador para realizar la conexión.

Para registrar y establecer los permisos necesarios para la aplicación:

1. En Azure Portal, seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.

    ![Registro de aplicación](./media/active-directory-interactive-connect-azure-sql-db/image1.png)

    Una vez que se crea el registro de aplicaciones, se genera y se muestra el valor del **identificador de la aplicación**.

    ![Identificador de aplicación mostrado](./media/active-directory-interactive-connect-azure-sql-db/image2.png)

2. Seleccione **Permisos de API** > **Agregar un permiso**.

    ![Configuración de permisos para la aplicación registrada](./media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

3. Seleccione **API usadas en mi organización** > escriba **Azure SQL Database** en la búsqueda y seleccione **Azure SQL Database**.

    ![Adición de acceso a la API de Azure SQL Database](./media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

4. Seleccione **Permisos delegados** > **user_impersonation** > **Agregar permisos**.

    ![Delegación de permisos en la API de Azure SQL Database](./media/active-directory-interactive-connect-azure-sql-db/sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)

### <a name="set-an-azure-ad-admin-for-your-server"></a>Establecimiento de un administrador de Azure AD para el servidor

Para que el programa de C# se ejecute, un administrador del [servidor SQL lógico](logical-servers.md) debe asignar un administrador de Azure AD para el servidor.

En la página **SQL Server**, seleccione **Administrador de Active Directory** > **Establecer administrador**.

Para más información sobre los usuarios y administradores de Azure AD para Azure SQL Database, consulte las capturas de pantalla que aparecen en [Configuración y administración de la autenticación de Azure Active Directory con SQL Database](authentication-aad-configure.md#provision-azure-ad-admin-sql-database).

### <a name="add-a-non-admin-user-to-a-specific-database-optional"></a>Incorporación de un usuario no administrador en una base de datos específica (opcional)

El administrador de Azure AD de un [servidor SQL lógico](logical-servers.md) puede ejecutar el programa de ejemplo de C#. Un usuario de Azure AD puede ejecutar el programa si están en la base de datos. Un administrador de SQL de Azure AD o un usuario de Azure AD que ya existe en la base de datos y tiene el permiso`ALTER ANY USER` en la base de datos pueden agregar un usuario.

Puede agregar un usuario a la base de datos con el comando [`Create User`](/sql/t-sql/statements/create-user-transact-sql) de SQL. Un ejemplo es `CREATE USER [<username>] FROM EXTERNAL PROVIDER`.

Para más información, consulte [Uso de la autenticación de Azure Active Directory para autenticación con SQL Database, Instancia administrada o Azure Synapse Analytics](authentication-aad-overview.md).

## <a name="new-authentication-enum-value"></a>Nuevo valor de enumeración de autenticación

El ejemplo de C# se basa en el espacio de nombres [`System.Data.SqlClient`](/dotnet/api/system.data.sqlclient). De especial interés para Multi-Factor Authentication es la enumeración `SqlAuthenticationMethod`, que tiene estos valores:

* `SqlAuthenticationMethod.ActiveDirectoryInteractive`

   Use este valor con un nombre de usuario de Azure AD para implementar Multi-Factor Authentication. Este valor es el enfoque del presente artículo. Produce una experiencia interactiva al mostrar cuadros de diálogo para la contraseña de usuario y, luego, para la validación de Multi-Factor Authentication si este método de autenticación se impone sobre este usuario. Este valor está disponible a partir de .NET Framework 4.7.2.

* `SqlAuthenticationMethod.ActiveDirectoryIntegrated`

  Use este valor para una cuenta *federada*. En una cuenta federada, el nombre del usuario se conoce en el dominio de Windows. Este método de autenticación no admite Multi-Factor Authentication.

* `SqlAuthenticationMethod.ActiveDirectoryPassword`

  Use este valor para la autenticación que requiere un nombre de usuario y contraseña de Azure AD. Azure SQL Database realiza la autenticación. Este método no admite Multi-Factor Authentication.

> [!NOTE]
> Si usa .NET Core, querrá usar el espacio de nombres [Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.sqlclient?view=sqlclient-dotnet-core-1.1). Para más información, consulte el siguiente [blog](https://devblogs.microsoft.com/dotnet/introducing-the-new-microsoftdatasqlclient/).

## <a name="set-c-parameter-values-from-the-azure-portal"></a>Establecimiento de los valores de parámetros de C# desde Azure Portal

Para que el programa de C# se ejecute correctamente, deberá asignar los valores correspondientes a los campos estáticos. Aquí se muestran campos con valores de ejemplo. También se muestran las ubicaciones de Azure Portal en las que puede obtener los valores necesarios.

| Nombre del campo estático | Valor de ejemplo | Dónde está en Azure Portal |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sqldb-svr.database.windows.net" | **Servidores SQL** > **Filtrar por nombre** |
| AzureAD_UserID | "auser\@abc.onmicrosoft.com" | **Azure Active Directory** > **Usuario** > **Nuevo usuario invitado** |
| Initial_DatabaseName | "myDatabase" | **Servidores SQL** > **Bases de datos SQL** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** > **Registros de aplicaciones** > **Buscar por nombre** > **Identificador de aplicación** |
| RedirectUri | new Uri("https://mywebserver.com/") | **Azure Active Directory** > **Registros de aplicaciones** > **Buscar por nombre** >  *[El-registro-de-su-aplicación]*  > **Configuración** > **RedirectURIs**<br /><br />En este artículo, cualquier valor válido sirve para RedirectUri, porque aquí no se usará. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="verify-with-sql-server-management-studio"></a>Comprobación con SQL Server Management Studio

Antes de ejecutar el programa de C#, se recomienda comprobar que la configuración de SQL Server Management Studio (SSMS) es correcta. De ese modo, cualquier error del programa de C# se puede restringir al código fuente.

### <a name="verify-server-level-firewall-ip-addresses"></a>Comprobación de las direcciones IP del firewall de nivel de servidor

Ejecute SSMS desde el mismo equipo, en el mismo edificio, donde planea ejecutar el programa de C#. Para esta prueba sirve cualquier modo de **autenticación**. Si hay cualquier indicación de que el servidor no acepta su dirección IP, consulte [Reglas de firewall de nivel de servidor y de nivel de base de datos](firewall-configure.md) para obtener ayuda.

### <a name="verify-azure-active-directory-multi-factor-authentication"></a>Comprobación de Multi-Factor Authentication para Azure Active Directory

Vuelva a ejecutar SSMS, en esta ocasión con la opción **Autenticación** establecida en **Active Directory - Universal con MFA**. Esta opción requiere la versión 17.5 o posterior de SSMS.

Para más información, consulte [Configuración de la autenticación multifactor para SQL Server Management Studio y Azure AD](authentication-mfa-ssms-configure.md).

> [!NOTE]
> Si es un usuario invitado de la base de datos, también debe proporcionar el nombre de dominio de Azure AD para la base de datos: Seleccione **Opciones** > **Nombre de dominio o ID de inquilino de AD**. Para buscar el nombre de dominio de Azure Portal, seleccione **Azure Active Directory** > **Nombres de dominio personalizados**. En el programa de C# de ejemplo, no es necesario brindar un nombre de dominio.

## <a name="c-code-example"></a>Ejemplo de código de C#

> [!NOTE]
> Si usa .NET Core, querrá usar el espacio de nombres [Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.sqlclient?view=sqlclient-dotnet-core-1.1). Para más información, consulte el siguiente [blog](https://devblogs.microsoft.com/dotnet/introducing-the-new-microsoftdatasqlclient/).

El programa de C# de ejemplo se basa en el ensamblado DLL de [*Microsoft.IdentityModel.Clients.ActiveDirectory*](/dotnet/api/microsoft.identitymodel.clients.activedirectory).

Para instalar este paquete, en Visual Studio seleccione **Proyecto** > **Administrar paquetes NuGet**. Busque e instale **Microsoft.IdentityModel.Clients.ActiveDirectory**.

Este es un ejemplo de código fuente de C#.

```csharp

using System;

// Reference to Azure AD authentication assembly
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<Your server>";
        static public string AzureAD_UserID = "<Your User ID>";
        static public string Initial_DatabaseName = "<Your Database>";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<Your App ID>";
        static public readonly Uri RedirectUri = new Uri("<Your URI>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2.
    ///  .
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  .
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider.
} // EONamespace.  End of entire program source code.

```

&nbsp;

Este es un ejemplo de la salida de prueba de C#.

```C#
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

& [Get-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator)