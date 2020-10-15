---
title: Configuración de Micro Focus CICS BankDemo para Micro Focus Enterprise Developer 4.0 en Azure Virtual Machines
description: Ejecute la aplicación Micro Focus BankDemo en Azure Virtual Machines (VM) para aprender a utilizar Micro Focus Enterprise Server y Enterprise Developer.
author: sread
ms.author: sread
ms.date: 03/30/2020
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 7fb72b9a7d0d655f99d1e5cf194f7c6f26976a37
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86508056"
---
# <a name="set-up-micro-focus-cics-bankdemo-for-micro-focus-enterprise-developer-40-on-azure"></a>Configuración de Micro Focus CICS BankDemo para Micro Focus Enterprise Developer 4.0 en Azure

Al configurar Micro Focus Enterprise Server 4.0 y Enterprise Developer 4.0 en Azure, puede probar las implementaciones de cargas de trabajo de IBM z/OS. En este artículo se muestra cómo configurar CICS BankDemo, una aplicación de ejemplo que se incluye con Enterprise Developer.

CICS significa Customer Information Control System, la plataforma de transacciones utilizada por muchas de las aplicaciones de sistema central en línea. La aplicación BankDemo es ideal para aprender cómo funcionan Enterprise Server y Enterprise Developer y cómo administrar e implementar una aplicación real completa con terminales de pantalla verde.

> [!NOTE]
> Próximamente: instrucciones para configurar [Micro Focus Enterprise Server 5.0](https://techcommunity.microsoft.com/t5/azurecat/micro-focus-enterprise-server-5-0-quick-start-template-on-azure/ba-p/1160110) en máquinas virtuales de Azure.

## <a name="prerequisites"></a>Requisitos previos

- Una máquina virtual con [Enterprise Developer](set-up-micro-focus-azure.md). Tenga en cuenta que Enterprise Developer tiene incluida una instancia completa de Enterprise Server para fines de desarrollo y pruebas. Esta instancia es la instancia de Enterprise Server utilizada para la demostración.

- [SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). Descárguelo e instálelo en la máquina virtual de Enterprise Developer. Enterprise Server requiere una base de datos para la administración de las regiones de CICS y la aplicación BankDemo también usa una base de datos de SQL Server llamada BANKDEMO. En esta demostración se asume que usa SQL Server Express para ambas bases de datos. Al instalar, seleccione la instalación básica.

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) (SSMS). SSMS se usa para administrar las bases de datos y ejecutar un script de T-SQL. Descárguelo e instálelo en la máquina virtual de Enterprise Developer.

- [Visual Studio 2019](https://azure.microsoft.com/downloads/) con el Service Pack más reciente o [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/), que puede descargar de forma gratuita.

- Rumba Desktop u otro emulador 3270.

## <a name="configure-the-windows-environment"></a>Configuración del entorno de Windows

Después de instalar Enterprise Developer 4.0 en la máquina virtual, debe configurar la instancia de Enterprise Server que se incluye con él. Para ello, deberá instalar algunas características adicionales de Windows como se indica a continuación.

1. Use RDP para iniciar sesión en la máquina virtual de Enterprise Server 4.0 que creó.

2. Haga clic en el icono **Buscar** situado junto al botón **Inicio** y escriba **características de Windows**. Se abre el asistente Agregar roles y características del Administrador del servidor.

3. Seleccione **Rol de servidor web (IIS)** y, a continuación, active las opciones siguientes:

    - Herramientas de administración web
    - Compatibilidad con la administración de IIS 6 (seleccionar todas las características disponibles)
    - Consola de administración de IIS
    - Scripts y herramientas de administración de IIS
    - Servicio de administración de IIS

4. Seleccione **Servicios World Wide Web** y active las opciones siguientes:

     Características de desarrollo de aplicaciones:
    - Extensibilidad de .NET
    - ASP.NET
    - Características HTTP comunes: Agregar todas las características disponibles
    - Estado y diagnóstico: Agregar todas las características disponibles
    - Seguridad:
        - Autenticación básica
        - Autenticación de Windows

5. Seleccione **Servicio WAS (Windows Process Activation Service)** y todos sus elementos secundarios.

6. Para **Características**, active **Microsoft .NET Framework 3.5.1** y active las opciones siguientes:

    - Windows Communication Foundation HTTP Activation
    - Windows Communication Foundation Non-HTTP Activation

7. Para **Características**, active **Microsoft .NET Framework 4.6** y active las opciones siguientes:

   - Activación de canalización con nombre
   - Activación de TCP
   - Uso compartido de puertos TCP

     ![Asistente para agregar roles y características: Servicios de rol](media/01-demo-roles.png)

8. Cuando haya seleccionado todas las opciones, haga clic en **Siguiente** para instalar.

9. Después de las características de Windows, vaya a **Panel de Control \> Sistema y seguridad \> Herramientas administrativas** y seleccione  **Servicios**. Desplácese hacia abajo y asegúrese de que los siguientes servicios están en ejecución y establecidos en **Automático**:

    - **NetTcpPortSharing**
    - **Adaptador de escucha Net.Pipe**
    - **Adaptador de escucha Net.Tcp**

10. Para configurar IIS y la compatibilidad con WAS, en el menú, busque **Micro Focus Enterprise Developer Command Prompt (64 bits)** y ejecútelo como **Administrador**.

11. Escriba **wassetup –i** y presione **Entrar**.

12. Después de ejecutar el script, puede cerrar la ventana.

## <a name="configure-the-local-system-account-for-sql-server"></a>Configuración de la cuenta del sistema local para SQL Server

Algunos procesos de Enterprise Server deben poder iniciar sesión en SQL Server y crear bases de datos y otros objetos. Estos procesos usan la cuenta del sistema local, por lo que debe dar autoridad sysadmin a esa cuenta.

1. Inicie **SSMS** y haga clic en **Conectar** para conectarse al servidor local SQLEXPRESS con la autenticación de Windows. Debe estar disponible en la lista **Nombre del servidor**.

2. En el lado izquierdo, expanda la carpeta **Seguridad** y seleccione **Inicios de sesión**.

3. Seleccione **NT AUTHORITY\\SYSTEM** y seleccione **Propiedades**.

4. Seleccione **Roles de servidor** y active **sysadmin**.

     ![Ventana del Explorador de objetos de SSMS: Propiedades de inicio de sesión](media/02-demo-explorer.png)

## <a name="create-the-bankdemo-database-and-all-its-objects"></a>Creación de la base de datos BankDemo y todos sus objetos

1. Abra el **Explorador de Windows** y vaya a **C:\\Users\\Public\\Documents\\Micro Focus\\Enterprise Developer\\Samples\\Mainframe\\CICS\\DotNet\\BankDemo\\SQL**.

2. Copie el contenido del archivo **BankDemoCreateAll.SQL** en el Portapapeles.

3. Abra **SSMS**. A la derecha, haga clic en **Servidor** y seleccione **Nueva consulta**.

4. Pegue el contenido del Portapapeles en el cuadro de texto **Nueva consulta**.

5. Haga clic en **Ejecutar** desde la pestaña **Comando** encima de la consulta para ejecutar el archivo SQL.

La consulta se debería ejecutar sin errores. Cuando finalice, tendrá la base de datos de ejemplo para la aplicación BankDemo.

![Salida de SQLQuery1.sql](media/03-demo-query.png)

## <a name="verify-that-the-database-tables-and-objects-have-been-created"></a>Comprobación de la creación de las tablas y los objetos de la base de datos

1. Haga clic con el botón derecho en la base de datos **BANKDEMO** y seleccione **Actualizar**.

2. Expanda el elemento **Base de datos** y seleccione **Tablas**. Debe ver algo parecido a lo siguiente.

     ![Tabla BANKDEMO expandida en el Explorador de objetos](media/04-demo-explorer.png)

## <a name="build-the-application-in-enterprise-developer"></a>Compilación de la aplicación en Enterprise Developer

1. Abra Visual Studio e inicie sesión.

2. En la opción de menú **Archivo**, seleccione **Abrir proyecto/solución**, vaya a **C:\\Users\\Public\\Documents\\Micro Focus\\Enterprise Developer\\Samples\\Mainframe\\CICS\\DotNet\\BankDemo** y seleccione el archivo **sln**.

3. Examine con detenimiento los objetos. Los programas COBOL se muestran en el Explorador de soluciones con la extensión CBL junto con los CopyBooks (CPY) y JCL.

4. Haga clic con el botón derecho en el proyecto **BankDemo2** y seleccione **Establecer como proyecto de inicio**.

    > [!NOTE]
    > El proyecto BankDemo utiliza HCOSS (Opción de compatibilidad de host para SQL Server,) que no se utiliza para esta demostración.

5. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **BankDemo2** y seleccione **Compilar**.

    > [!NOTE]
    > La compilación en el nivel de solución produce errores, ya que no se ha configurado HCOSS.

6. Una vez compilado el proyecto, examine la ventana **Salida**. Debería parecerse a la imagen siguiente.

     ![Ventana de salida que muestra una compilación correcta](media/05-demo-output.png)

## <a name="deploy-the-bankdemo-application-into-the-region-database"></a>Implementación de la aplicación BankDemo en la base de datos de la región

1. Abra un símbolo del sistema de Enterprise Developer (64 bits) como Administrador.

2. Vaya a **%PUBLIC%\\Documents\\Micro Focus\\Enterprise Developer\\samples\\Mainframe\\CICS\\DotNet\\BankDemo**.

3. En el símbolo del sistema, ejecute **bankdemodbdeploy** e incluya el parámetro de la base de datos en la que se va a implementar, por ejemplo:

    ```
    bankdemodbdeploy (local)/sqlexpress
    ```

> [!NOTE]
> Asegúrese de usar una barra diagonal (/) y no una barra diagonal inversa (\\). Este script se ejecuta durante un tiempo.

![Administración: Ventana del símbolo del sistema de Enterprise Developer](media/06-demo-cmd.png)

## <a name="create-the-bankdemo-region-in-enterprise-administrator-for-net"></a>Creación de la región BankDemo en Enterprise Administrator for .NET

1. Abra la interfaz de usuario **Enterprise Server for .NET Administration**.

2. Para iniciar el complemento de MMC, desde el menú **Inicio** de Windows, elija **Micro Focus Enterprise Developer \> Configuration \> Enterprise Server for .NET Admin**. (Para Windows Server, elija **Micro Focus Enterprise Developer \> Enterprise Server for .NET Admin**).

3. Expanda el contenedor **Regions** (Regiones) en el panel izquierdo y, a continuación, haga clic con el botón derecho en **CICS**.

4. Seleccione **Define Region** (Definir región) para crear una nueva región de CICS llamada **BANKDEMO**, hospedada en la base de datos (local).

5. Proporcione la instancia del servidor de base de datos, haga clic en **Next** (Siguiente) y, a continuación, escriba el nombre de la región **BANKDEMO**.

     ![Cuadro de diálogo Definir región](media/07-demo-cics.png)

6. Para seleccionar el archivo de definición de región para la base de datos de regiones, busque **region\_bankdemo\_db.config** en **C:\\Users\\Public\\Documents\\Micro Focus\\Enterprise Developer\\Samples\\Mainframe\\CICS\\DotNet\\BankDemo**.

     ![Definir región: nombre de la región: BANKDEMO](media/08-demo-cics.png)

7. Haga clic en **Finalizar**

## <a name="create-xa-resource-definitions"></a>Creación de las definiciones de recursos XA

1. En el panel izquierdo de la interfaz de usuario **Enterprise Server for .NET Administration**, expanda **System** (Sistema) y, a continuación, **XA Resource Definitions** (Definiciones de recursos XA). Esta configuración define cómo interactúa la región con Enterprise Server y las bases de datos de la aplicación.

2. Haga clic con el botón derecho en **XA Resource Definitions** (Definiciones de recursos XA) y seleccione **Add Server Instance** (Agregar instancia de servidor).

3. En el cuadro de lista desplegable, seleccione **Database Service Instance** (Instancia de servicio de base de datos). Será SQLEXPRESS del equipo local.

4. Seleccione la instancia en el contenedor **XA Resource Definitions (machinename\\sqlexpress)** y haga clic en **Add** (Agregar).

5. Seleccione **Database XA Resource Definition** (Definición de recurso XA de base de datos) y, a continuación, escriba **BANKDEMO** para los campos **Name** (Nombre) y **Region** (Región).

     ![Nueva pantalla de definición de recurso XA de base de datos](media/09-demo-xa.png)

6. Haga clic en el botón de puntos suspensivos ( **...** ) para mostrar el Asistente para la cadena de conexión. En **Nombre del servidor**, escriba **(local)\\SQLEXPRESS**. Para **Inicio de sesión**, seleccione **Autenticación de Windows**. En Nombre de la base de datos, escriba **BANKDEMO**.

     ![Pantalla de edición de la cadena de conexión](media/10-demo-string.png)

7. Pruebe la conexión.

## <a name="start-the-bankdemo-region"></a>Inicio de la región BANKDEMO

> [!NOTE]
> El primer paso es importante: Debe establecer la región para que utilice la definición de recurso XA que acaba de crear.

1. Vaya a la región **BANKDEMO CICS Region** en la sección **Regions Container** (Contenedor de regiones) y, a continuación, seleccione **Edit Region Startup File** (Editar archivo de inicio de región) desde el panel **Actions** (Acciones). Desplácese hacia abajo hasta las propiedades de SQL y escriba **bankdemo** para el campo **XA resource name** (Nombre del recurso XA) o use el botón de puntos suspensivos para seleccionarlo.

2. Haga clic en el icono **Save** (Guardar) para guardar los cambios.

3. Haga clic con el botón derecho en **BANKDEMO CICS Region** en el panel **Console** (Consola) y seleccione **Start/Stop Region** (Iniciar/Detener región).

4. En la parte inferior del cuadro de texto **Start/Stop Region** (Iniciar/Detener región) que aparece en el panel central, seleccione **Start** (Iniciar). Después de unos segundos, se inicia la región.

     ![Cuadro de texto SQL Start/Stop (Iniciar/Detener SQL)](media/11-demo-sql.png)

     ![Pantalla CICS Region BANKDEMO iniciada](media/12-demo-cics.png)

## <a name="create-a-listener"></a>Creación de un agente de escucha

Cree un agente de escucha para las sesiones de TN3270 que tienen acceso a la aplicación BankDemo.

1. En el panel izquierdo, expanda **Configuration Editors** (Editores de configuración) y seleccione **Listener** (Agente de escucha).

2. Haga clic en el icono **Open File** (Abrir archivo) y seleccione el archivo **seelistener.exe.config**. Este archivo se va a editar y se carga cada vez que se inicia Enterprise Server.

3. Observe las dos regiones definidas previamente (ESDEMO y JCLDEMO).

4. Para crear una nueva región para BANKDEMO, haga clic con el botón derecho en **Regions** (Regiones) y seleccione **Add Region** (Agregar región).

5. Seleccione **BANKDEMO Region**.

6. Agregue un canal de TN3270; para ello, haga clic con el botón derecho en **BANKDEMO Region** y seleccione **Add Channel** (Agregar canal).

7. En el campo **Name** (Nombre), escriba **TN3270**. En el campo **Port** (Puerto), escriba **9024**. La aplicación ESDEMO utiliza el puerto 9230, por lo que deberá usar un puerto diferente.

8. Para guardar el archivo, haga clic en el icono **Guardar** o seleccione **Archivo** \> **Guardar**.

9. Para iniciar el cliente de escucha, haga clic en **Iniciar el agente de escucha** o elija **Opciones** \> **Iniciar el agente de escucha**.

     ![Ventanas del editor de configuración del agente de escucha](media/13-demo-listener.png)


## <a name="configure-rumba-to-access-the-bankdemo-application"></a>Configuración de Rumba para el acceso a la aplicación BankDemo

Lo último que debe hacer es configurar una sesión de 3270 con Rumba, un emulador 3270. Este paso le permite acceder a la aplicación BankDemo mediante el agente de escucha que creó.

1. Desde el menú **Inicio** de Windows, inicie Rumba Desktop.

2. En el elemento de menú **Connections** (Conexiones), seleccione **TN3270**.

3. Haga clic en **Insert** (Insertar) y escriba **127.0.0.1** para la dirección IP y **9024** para el puerto definido por el usuario.

4. En la parte inferior del cuadro de diálogo, haga clic en **Connect** (Conectar). Aparece una pantalla de CICS en negro.

5. Escriba **bank** para mostrar la pantalla inicial de 3270 para la aplicación BankDemo.

6. Para el identificador de usuario, escriba **B0001** y para la contraseña, escriba cualquier cosa. Se abre la primera pantalla BANK20.

![Pantalla de bienvenida del sistema central](media/14-demo.png)
![Pantalla del sistema central: Rumba: pantalla de demostración del subsistema](media/15-demo.png)

Felicidades. Ahora tiene una aplicación de CICS en ejecución en Azure con Micro Focus Enterprise Server.

## <a name="next-steps"></a>Pasos siguientes

- [Ejecución de Enterprise Server en contenedores de Docker en Azure](run-enterprise-server-container.md)
- [Migración del sistema central: portal](/archive/blogs/azurecat/mainframe-migration-to-azure-portal)
- [Máquinas virtuales](../../../linux/overview.md)
- [Solución de problemas](../../../troubleshooting/index.yml)
- [Demystifying mainframe to Azure migration](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/) (Desmitificación de la migración del sistema central a Azure)
