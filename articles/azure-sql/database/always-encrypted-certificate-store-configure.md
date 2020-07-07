---
title: Configuración de Always Encrypted mediante el almacén de certificados de Windows
description: En este artículo se muestra cómo proteger los datos confidenciales de Azure SQL Database con cifrado de base de datos mediante el asistente de Always Encrypted de SQL Server Management Studio (SSMS). También muestra cómo almacenar las claves de cifrado en el almacén de certificados de Windows.
keywords: cifrar datos, cifrado sql, cifrado de base de datos, datos confidenciales, Always Encrypted
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviwer: ''
ms.date: 04/23/2020
ms.openlocfilehash: 848a0c9817472086dbaf3973dad9c64e3ed74b10
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954254"
---
# <a name="configure-always-encrypted-by-using-the-windows-certificate-store"></a>Configuración de Always Encrypted mediante el almacén de certificados de Windows

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

En este artículo se muestra cómo proteger los datos confidenciales de Azure SQL Database o Instancia administrada de Azure SQL con cifrado de base de datos mediante el [asistente de Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-wizard) de [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). También muestra cómo almacenar las claves de cifrado en el almacén de certificados de Windows.

Always Encrypted es una tecnología de cifrado de datos que ayuda a proteger los datos confidenciales en reposo en el servidor, durante el movimiento entre cliente y servidor, y mientras están en uso, lo que garantiza que los datos confidenciales nunca aparezcan como texto no cifrado dentro del sistema de base de datos. Después de cifrar los datos, solo las aplicaciones cliente o los servidores de aplicaciones que tienen acceso a las claves pueden acceder a los datos de texto no cifrado. Para más información, consulte [Always Encrypted (motor de base de datos)](https://msdn.microsoft.com/library/mt163865.aspx).

Después de configurar la base de datos para usar Always Encrypted, creará una aplicación cliente en C# con Visual Studio para trabajar con los datos cifrados.

Siga los pasos de este artículo para obtener información sobre cómo configurar Always Encrypted para SQL Database o Instancia administrada de SQL. En este artículo, aprenderá a realizar las siguientes tareas:

* Usar el asistente de Always Encrypted en SSMS para crear [claves de Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Crear una [clave maestra de columna (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Crear una [clave de cifrado de columna (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Crear una tabla de base de datos y cifrar columnas.
* Crear una aplicación que inserta, selecciona y muestra los datos de las columnas cifradas.

## <a name="prerequisites"></a>Requisitos previos

Para este tutorial, necesitará:

* Una cuenta y una suscripción de Azure. Si no tiene una, suscríbase para [una prueba gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Una base de datos de [Azure SQL Database](single-database-create-quickstart.md) o [Instancia administrada de Azure SQL](../managed-instance/instance-create-quickstart.md).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versión 13.0.700.242 o posterior.
* [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) o posterior (en el equipo cliente).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="enable-client-application-access"></a>Habilitación de acceso de aplicaciones cliente

Debe habilitar la aplicación cliente para acceder a SQL Database o Instancia administrada de SQL mediante la configuración de una aplicación de Azure Active Directory (AAD) y la copia del *Id. de aplicación* y la *clave* que se van a necesitar para autenticar la aplicación.

Para obtener el *id. de la aplicación* y la *clave*, siga estos pasos acerca de cómo [crear una aplicación de Azure Active Directory y una entidad de servicio con acceso a los recursos](../../active-directory/develop/howto-create-service-principal-portal.md).



## <a name="connect-with-ssms"></a>Conectarse con SSMS

Abra SQL Server Management Studio (SSMS) y conéctese al servidor o administre con la base de datos.

1. Abra SSMS. (Haga clic en **Conectar** > **Motor de base de datos** para abrir la ventana **Conectar con el servidor** si no está abierta).
2. Escriba su nombre del servidor y las credenciales.

    ![Copiar la cadena de conexión](./media/always-encrypted-certificate-store-configure/ssms-connect.png)

Si se abre la ventana **Nueva regla de firewall** , inicie sesión en Azure y permita que SSMS cree una nueva regla de firewall.

## <a name="create-a-table"></a>Creación de una tabla

En esta sección, creará una tabla para almacenar datos de pacientes. Inicialmente será una tabla normal; configurará el cifrado en la sección siguiente.

1. Expanda **Bases de datos**.
2. Haga clic con el botón derecho en la base de datos **Clinic** y haga clic en **Nueva consulta**.
3. Pegue el siguiente código Transact-SQL (T-SQL) en la nueva ventana de consulta y haga clic en **Ejecutar** .
    
    ```tsql
    CREATE TABLE [dbo].[Patients](
    [PatientId] [int] IDENTITY(1,1),
    [SSN] [char](11) NOT NULL,
    [FirstName] [nvarchar](50) NULL,
    [LastName] [nvarchar](50) NULL,
    [MiddleName] [nvarchar](50) NULL,
    [StreetAddress] [nvarchar](50) NULL,
    [City] [nvarchar](50) NULL,
    [ZipCode] [char](5) NULL,
    [State] [char](2) NULL,
    [BirthDate] [date] NOT NULL
    PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
    GO
    ```

## <a name="encrypt-columns-configure-always-encrypted"></a>Cifrado de columnas (configurar Always Encrypted)

SSMS proporciona un asistente para configurar Always Encrypted de forma fácil mediante la configuración automática de CMK, CEK y columnas cifradas.

1. Expandaa **Bases de datos** > **Clinic** > **Tablas**.
2. Haga clic con el botón derecho en la tabla **Patients** y seleccione **Cifrar columnas** para abrir el asistente de Always Encrypted:

    ![Cifrar columnas](./media/always-encrypted-certificate-store-configure/encrypt-columns.png)

El Asistente para Always Encrypted incluye las siguientes secciones: **Selección de columnas**, **Configuración de la clave maestra** (CMK), **Validación** y **Resumen**.

### <a name="column-selection"></a>Selección de columnas

En la página **Introducción**, haga clic en **Siguiente** para abrir la página **Selección de columnas**. En esta página, seleccione las columnas que desea cifrar, [el tipo de cifrado y qué clave de cifrado de columna (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) desea usar.

Cifre la información **SSN** y **BirthDate** de cada paciente. La columna **SSN** usará cifrado determinista, que admite búsquedas de igualdad, combinaciones y agrupaciones. La columna **BirthDate** usará cifrado aleatorio, que no admite operaciones.

Establezca el **Tipo de cifrado** de la columna **SSN** en **Determinista** y la columna **BirthDate** en **Aleatoria**. Haga clic en **Next**.

![Cifrar columnas](./media/always-encrypted-certificate-store-configure/column-selection.png)

### <a name="master-key-configuration"></a>Configuración de la clave maestra

En la página **Configuración de la clave maestra** se configura la clave maestra de columna (CMK) y se selecciona el proveedor del almacén de claves donde se almacenará la CMK. Actualmente, puede almacenar una CMK en el almacén de certificados de Windows, en Azure Key Vault o en un módulo de seguridad de hardware (HSM). En este tutorial se muestra cómo almacenar las claves en el almacén de certificados de Windows.

Compruebe que el **almacén de certificados de Windows** esté seleccionado y haga clic en **Siguiente**.

![Configuración de la clave maestra](./media/always-encrypted-certificate-store-configure/master-key-configuration.png)

### <a name="validation"></a>Validación

Puede cifrar las columnas ahora o guardar un script de PowerShell para ejecutarlo más tarde. Para este tutorial, seleccione **Continuar para finalizar ahora** y haga clic en **Siguiente**.

### <a name="summary"></a>Resumen

Compruebe que la configuración sea correcta y haga clic en **Finalizar** para completar la configuración de Always Encrypted.

![Resumen](./media/always-encrypted-certificate-store-configure/summary.png)

### <a name="verify-the-wizards-actions"></a>Comprobación de las acciones del asistente

Una vez finalizado el asistente, la base de datos estará configurada para Always Encrypted. El asistente habrá realizado las siguientes acciones:

* Crea un CMK.
* Crea un CMK.
* Configuración de las columnas seleccionadas para el cifrado. La tabla **Patients** aún no tiene datos, pero los datos existentes en las columnas seleccionadas ahora están cifrados.

Para comprobar la creación de las claves en SSMS, vaya a **Clinic** > **Seguridad** > **Claves de Always Encrypted**. Ahora puede ver las nuevas claves que el asistente generó para usted.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Crear una aplicación cliente que funcione con los datos cifrados

Ahora que Always Encrypted está configurado, vamos a crear una aplicación que realice operaciones de *inserción* y *selección* en las columnas cifradas. Para ejecutar correctamente la aplicación de ejemplo, debe ejecutarla en el mismo equipo en el que ejecutó el asistente de Always Encrypted. Para ejecutar la aplicación en otro equipo, debe implementar los certificados de Always Encrypted en el equipo que ejecuta la aplicación cliente.  

> [!IMPORTANT]
> La aplicación debe usar objetos [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) al pasar datos de texto sin cifrar al servidor con columnas de Always Encrypted. Se generará una excepción al pasar valores literales sin usar objetos SqlParameter.

1. Abra Visual Studio y cree una nueva aplicación de consola C#. Asegúrese de que el proyecto esté establecido en **.NET Framework 4.6** o versiones posteriores.
2. Asigne al proyecto el nombre **AlwaysEncryptedConsoleApp** y haga clic en **Aceptar**.

![Nueva aplicación de consola](./media/always-encrypted-certificate-store-configure/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Modificar la cadena de conexión para habilitar Always Encrypted

En esta sección se explica cómo habilitar Always Encrypted en la cadena de conexión de su base de datos. La aplicación de consola que acaba de crear se modifica en la siguiente sección, “Aplicación de consola de ejemplo de Always Encrypted”.

Para habilitar Always Encrypted, debe agregar la palabra clave **Column Encryption Setting** a la cadena de conexión y establecerla en **Enabled**.

Puede establecerla directamente en la cadena de conexión o mediante [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). La aplicación de ejemplo de la siguiente sección muestra cómo usar **SqlConnectionStringBuilder**.

> [!NOTE]
> Este es el único cambio necesario en una aplicación cliente específica de Always Encrypted. Si tiene una aplicación existente que almacena su cadena de conexión de forma externa (es decir, en un archivo de configuración) puede habilitar Always Encrypted sin cambiar ningún código.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Modificar Always Encrypted en la cadena de conexión

Agregue la siguiente palabra clave en la cadena de conexión:

`Column Encryption Setting=Enabled`

### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Habilitar Always Encrypted con un SqlConnectionStringBuilder

El siguiente código muestra cómo habilitar Always Encrypted estableciendo [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) en [Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

```csharp
// Instantiate a SqlConnectionStringBuilder.
SqlConnectionStringBuilder connStringBuilder =
    new SqlConnectionStringBuilder("replace with your connection string");

// Enable Always Encrypted.
connStringBuilder.ColumnEncryptionSetting =
    SqlConnectionColumnEncryptionSetting.Enabled;
```

## <a name="always-encrypted-sample-console-application"></a>Aplicación de consola de ejemplo de Always Encrypted

En este ejemplo se muestra cómo:

* Modificar la cadena de conexión para habilitar Always Encrypted.
* Insertar datos en las columnas cifradas.
* Seleccionar un registro mediante el filtrado de un valor específico de una columna cifrada.

Reemplace el contenido de **Program.cs** por el código siguiente. Reemplace la cadena de conexión para la variable global connectionString en la línea directamente anterior al método Main por una cadena de conexión válida desde el Portal de Azure. Este es el único cambio que necesita realizar en este código.

Ejecute la aplicación para ver Always Encrypted en acción.

```cs
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Globalization;

namespace AlwaysEncryptedConsoleApp
{
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Data Source = SPE-T640-01.sys-sqlsvr.local; Initial Catalog = Clinic; Integrated Security = true";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();

            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            CultureInfo culture = CultureInfo.CreateSpecificCulture("en-US");
            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993", culture)
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
}
```

## <a name="verify-that-the-data-is-encrypted"></a>Comprobación del cifrado de los datos

Para comprobar rápidamente si los datos reales en el servidor se están cifrando, consulte los datos de **Patients** con SSMS. (Use su conexión actual si la configuración de cifrado de columnas no está habilitado aún).

Ejecute la siguiente consulta en la base de datos Clinic.

```tsql
SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
```

Puede ver que las columnas cifradas no contienen datos de texto no cifrado.

   ![Nueva aplicación de consola](./media/always-encrypted-certificate-store-configure/ssms-encrypted.png)

Par usar SSMS para obtener acceso a los datos de texto no cifrado, puede agregar el parámetro **Column Encryption Setting=enabled** a la conexión.

1. En SSMS, haga clic con el botón derecho en el servidor en el **Explorador de objetos** y haga clic en **Desconectar**.
2. Haga clic en **Conectar** > **Motor de base de datos** para abrir la ventana **Conectar con el servidor** y haga clic en **Opciones**.
3. Haga clic en **Parámetros de conexión adicionales** y escriba **Column Encryption Setting=enabled**.

    ![Nueva aplicación de consola](./media/always-encrypted-certificate-store-configure/ssms-connection-parameter.png)
4. Ejecute la siguiente consulta en la base de datos **Clinic** .

    ```tsql
    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
    ```

     Ahora puede ver los datos de texto no cifrado en las columnas cifradas.

    ![Nueva aplicación de consola](./media/always-encrypted-certificate-store-configure/ssms-plaintext.png)

> [!NOTE]
> Si se conecta con SSMS (o cualquier cliente) desde un equipo diferente, no tendrá acceso a las claves de cifrado y no podrá descifrar los datos.

## <a name="next-steps"></a>Pasos siguientes

Después de crear una base de datos que usa Always Encrypted, es posible que quiera hacer lo siguiente:

* Ejecutar este ejemplo desde un equipo diferente. No tendrá acceso a las claves de cifrado, por lo que no tendrá acceso a los datos de texto no cifrado y no se ejecutará correctamente.
* [Rotar y limpiar las claves](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migrar los datos que ya están cifrados con Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).
* [Implementar certificados de Always Encrypted en otros equipos cliente](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (consulte la sección “Disponibilidad de los certificados para las aplicaciones y los usuarios”).

## <a name="related-information"></a>Información relacionada

* [Always Encrypted (desarrollo de clientes)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Cifrado de datos transparente](https://msdn.microsoft.com/library/bb934049.aspx)
* [Cifrado de SQL Server](https://msdn.microsoft.com/library/bb510663.aspx)
* [Asistente de Always Encrypted](https://msdn.microsoft.com/library/mt459280.aspx)
* [Blog de Always Encrypted](https://docs.microsoft.com/archive/blogs/sqlsecurity/always-encrypted-key-metadata)
