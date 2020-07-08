---
title: Traslado de la base de datos de Azure AD Connect desde SQL Server Express a SQL Server. | Microsoft Docs
description: En este documento se describe cómo trasladar la base de datos de Azure AD Connect desde el servidor local de SQL Server Express a un servidor SQL Server remoto.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdc8b68206161abdd1782561c904d4e670ecca22
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85358963"
---
# <a name="move-azure-ad-connect-database-from-sql-server-express-to-sql-server"></a>Traslado de la base de datos de Azure AD Connect desde SQL Server Express a SQL Server 

En este documento se describe cómo trasladar la base de datos de Azure AD Connect desde el servidor local de SQL Server Express a un servidor SQL Server remoto.  Puede usar los siguientes procedimientos para realizar esta tarea.

## <a name="about-this-scenario"></a>Acerca de este escenario
A continuación se detalla una información breve acerca de este escenario.  En este escenario, la versión 1.1.819.0 de Azure AD Connect está instalada en un único controlador de dominio Windows Server 2016.  Utiliza SQL Server 2012 Express Edition integrado para la base de datos.  La base de datos se trasladará a un servidor SQL Server 2017.

![Arquitectura del escenario](media/how-to-connect-install-move-db/move1.png)

## <a name="move-the-azure-ad-connect-database"></a>Traslado de la base de datos de Azure AD Connect
Use los pasos siguientes para trasladar la base de datos de Azure AD Connect a un servidor SQL Server remoto.

1. En el servidor de Azure AD Connect, vaya a **Servicios** y detenga el servicio de **sincronización de Microsoft Azure AD**.
2. Busque la carpeta **%ProgramFiles%\Microsoft Azure AD Sync\Data** y copie los archivos **ADSync.mdf** y **ADSync_log.ldf** en el servidor SQL Server remoto.
3. Reinicie el servicio **de sincronización de Microsoft Azure AD** en el servidor de Azure AD Connect.
4. Para desinstalar AD Azure Connect, vaya al Panel de Control, Programas, Programas y características.  Seleccione Microsoft Azure AD Connect y haga clic en Desinstalar en la parte superior.
5. En el servidor SQL Server remoto, abra SQL Server Management Studio.
6. En Bases de datos, haga clic con el botón derecho y seleccione Adjuntar.
7. En la pantalla **Adjuntar bases de datos**, haga clic en **Agregar** y vaya al archivo ADSync.mdf.  Haga clic en **OK**.
   ![Adjuntar base de datos](media/how-to-connect-install-move-db/move2.png)

8. Una vez adjuntada la base de datos, vuelva al servidor de Azure AD Connect e instale Azure AD Connect.
9. Una vez finalizada la instalación del MSI, se inicia el asistente de Azure AD Connect con la configuración del modo rápido. Cierre la pantalla haciendo clic en el icono de salida.
   ![Pantalla de bienvenida](./media/how-to-connect-install-move-db/db1.png)
10. Inicie un nuevo símbolo del sistema o sesión de PowerShell. Vaya a la carpeta \<drive>\archivos de programa\Microsoft Azure AD Connect. Ejecute el comando .\AzureADConnect.exe /useexistingdatabase para iniciar el asistente de Azure AD Connect en el modo de configuración "Usar base de datos existente".
    ![PowerShell](./media/how-to-connect-install-move-db/db2.png)
11. Aparece la pantalla de bienvenida a Azure AD Connect. Una vez que acepte los términos de licencia y el aviso de privacidad, haga clic en **Continuar**.
    ![Pantalla de bienvenida](./media/how-to-connect-install-move-db/db3.png)
12. En la pantalla **Instalar componentes necesarios**, está habilitada la opción **Usar un SQL Server existente**. Especifique el nombre del servidor de SQL Server que hospeda la base de datos de ADSync. Si la instancia del motor SQL utilizada para hospedar la base de datos de ADSync no es la instancia predeterminada del servidor de SQL Server, deberá especificar el nombre de la instancia del motor SQL. Además, si no está habilitada la exploración de SQL, también deberá especificar el número de puerto de la instancia del motor SQL. Por ejemplo:         
    ![Pantalla de bienvenida](./media/how-to-connect-install-move-db/db4.png)           

13. En la pantalla **Conectar con Azure AD**, debe proporcionar las credenciales de un administrador global de su directorio de Azure AD. Se recomienda utilizar una cuenta en el dominio predeterminado onmicrosoft.com. Esta cuenta solo se usa para crear una cuenta de servicio en Azure AD y no se utiliza una vez completado el asistente.
    ![Conexión](./media/how-to-connect-install-move-db/db5.png)
 
14. En la pantalla **Conectar sus directorios**, el bosque de AD existente configurado para la sincronización de directorios se muestra con un icono de cruz roja junto a él. Para sincronizar los cambios desde un bosque de AD local, se necesita una cuenta de AD DS. El asistente de Azure AD Connect no puede recuperar las credenciales de la cuenta de AD DS almacenada en la base de datos de ADSync porque las credenciales están cifradas y solo las puede descifrar el anterior servidor de Azure AD Connect. Haga clic en **Cambiar credenciales** para especificar la cuenta de AD DS para el bosque de AD.
    ![Directorios](./media/how-to-connect-install-move-db/db6.png)
 

15. En el cuadro de diálogo emergente, puede (i) proporcionar credenciales de administrador de la organización y dejar que Azure AD Connect cree la cuenta de AD DS en su lugar, o (ii) crear la cuenta de AD DS usted mismo y proporcionar sus credenciales a Azure AD Connect. Una vez que haya seleccionado una opción y proporcionado las credenciales necesarias, haga clic en **Aceptar** para cerrar el cuadro de diálogo emergente.
    ![Pantalla de bienvenida](./media/how-to-connect-install-move-db/db7.png)
 

16. Una vez que se hayan proporcionado las credenciales, el icono de cruz roja se reemplazará por un icono de marca de verificación verde. Haga clic en **Next**.
    ![Pantalla de bienvenida](./media/how-to-connect-install-move-db/db8.png)
 

17. En la pantalla **Listo para configurar**, haga clic en **Instalar**.
    ![Pantalla de bienvenida](./media/how-to-connect-install-move-db/db9.png)
 

18. Una vez completada la instalación, el servidor de Azure AD Connect se habilita automáticamente para el modo de ensayo. Se recomienda que revise la configuración del servidor y las exportaciones pendientes para detectar cambios inesperados antes de deshabilitar el modo de ensayo. 

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
- [Instalación de Azure AD Connect mediante una base de datos existente de ADSync](how-to-connect-install-existing-database.md)
- [Instalación de Azure AD Connect mediante permisos de administrador delegados de SQL](how-to-connect-install-sql-delegation.md)

