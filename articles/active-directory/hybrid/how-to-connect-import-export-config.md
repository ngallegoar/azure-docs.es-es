---
title: Cómo importar y exportar opciones de configuración de Azure AD Connect
description: En este documento se describen las preguntas frecuentes sobre el aprovisionamiento en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/13/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a94d356cb3c0345f575b4b5a44aa7f228535e66d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019886"
---
# <a name="importing-and-exporting-azure-ad-connect-configuration-settings-public-preview"></a>Importación y exportación de opciones de configuración de Azure AD Connect (versión preliminar pública) 

Las implementaciones de Azure AD Connect varían de una instalación en modo Rápido de bosque único a implementaciones complejas que se sincronizan entre varios bosques mediante reglas de sincronización personalizadas. Debido al gran número de opciones y mecanismos de configuración, es esencial comprender qué opciones están en vigor y poder implementar rápidamente un servidor con una configuración idéntica. Esta característica presenta la capacidad de catalogar la configuración de un servidor de sincronización determinado e importar la configuración en una nueva implementación. Se pueden comparar distintas instantáneas de configuración de sincronización para visualizar fácilmente las diferencias entre dos servidores, o del mismo servidor a lo largo del tiempo. 

Cada vez que se cambia la configuración desde el asistente de Azure AD Connect, un nuevo archivo de configuración JSON con marca de tiempo se exporta automáticamente a  **%ProgramData%\AADConnect**. El nombre de archivo de configuración tiene el formato **Applied-SynchronizationPolicy-*.JSON**, donde la última parte del nombre de archivo es una marca de tiempo. 

> [!IMPORTANT]
> Solo se exportan automáticamente los cambios realizados por Azure AD Connect. Todo cambio realizado mediante PowerShell, Synchronization Service Manager o el editor de reglas de sincronización debe exportarse a petición según sea necesario para mantener una copia actualizada. La exportación a petición también se puede usar para colocar una copia de la configuración en una ubicación segura para fines de recuperación ante desastres. 

## <a name="exporting-azure-ad-connect-settings"></a>Exportación de la configuración de Azure AD Connect 

Para ver un resumen de las opciones de configuración, abra la herramienta Azure AD Connect y elija la tarea adicional denominada: Ver o exportar la configuración actual. Se muestra un resumen rápido de la configuración, junto con la capacidad de exportar la configuración completa del servidor. 

De forma predeterminada, la configuración se exportará a **%ProgramData%\AADConnect**; sin embargo, puede optar por guardar la configuración en una ubicación protegida para garantizar la disponibilidad en caso de desastre. La configuración se exporta con el formato de archivo JSON y no debe crearse ni editarse manualmente para garantizar la coherencia lógica. La importación de un archivo creado o editado manualmente no se admite y puede provocar resultados inesperados. 

## <a name="importing-azure-ad-connect-settings"></a>Importación de la configuración de Azure AD Connect 

Para importar una configuración anteriormente exportada, realice lo siguiente:
 
1. Instale **Azure AD Connect** en un nuevo servidor. 
2. Seleccione la opción **Personalizar** después de la página de **bienvenida**. 
3. Haga clic en **Importar configuración de sincronización**. Busque el archivo de configuración JSON anteriormente exportado.  
4. Haga clic en **Instalar**.

![Instalar componentes](media/how-to-connect-import-export-config/import1.png)

> [!NOTE]
> Invalide la configuración de esta página, como el uso de SQL Server en lugar de LocalDB o el uso de una cuenta de servicio existente en lugar de la VSA predeterminada, etc. Estas opciones no se importan desde el archivo de opciones de configuración, pero sí para fines comparativos e informativos.

### <a name="import-installation-experience"></a>Experiencia de instalación de la importación 

La experiencia de instalación de la importación sigue siendo sencilla a propósito, con entradas mínimas del usuario para facilitar la reproducibilidad de un servidor existente.  

Estos son los únicos cambios que se pueden realizar durante la experiencia de instalación. Todos los demás cambios se pueden realizar después de la instalación desde el asistente de Azure AD Connect. 
- **Credenciales de Azure Active Directory** : De forma predeterminada se sugiere el nombre de cuenta para el administrador global de Azure que se usa para configurar el servidor original, y  **DEBE** cambiarse si desea sincronizar la información con un nuevo directorio.
- **Inicio de sesión del usuario** : Las opciones de inicio de sesión configuradas para el servidor original están seleccionadas de forma predeterminada y solicitarán automáticamente las credenciales u otra información necesaria durante la configuración. En casos excepcionales, puede ser necesario configurar un servidor con otras opciones para evitar cambiar el comportamiento del servidor activo. De lo contrario, simplemente presione Siguiente para usar la misma configuración. 
- **Credenciales del directorio local** : Para cada directorio local incluido en la configuración de sincronización, debe proporcionar credenciales para crear una cuenta de sincronización o suministrar una cuenta de sincronización personalizada creada previamente. Este procedimiento es idéntico a la experiencia de instalación limpia, con la excepción de que no se pueden agregar ni quitar directorios. 
- **Opciones de configuración** : Al igual que con una instalación limpia, puede optar por configurar las opciones iniciales de si desea iniciar la sincronización automática o habilitar el modo de almacenamiento provisional. La principal diferencia es que el modo de almacenamiento provisional está habilitado intencionadamente de forma predeterminada para permitir la comparación de los resultados de la configuración y la sincronización antes de exportar activamente los resultados a Azure. 

![Conexión de directorios](media/how-to-connect-import-export-config/import2.png)

> [!NOTE]
> Solo un servidor de sincronización puede estar en el rol principal y exportar activamente los cambios de configuración a Azure. Todos los demás servidores deben colocarse en Modo provisional. 

## <a name="migrating-settings-from-an-existing-server"></a>Migración de la configuración desde una implementación existente 

Si un servidor existente no admite la administración de la configuración, puede optar por actualizar el servidor en contexto o migrar la configuración para su uso en un nuevo servidor de almacenamiento provisional.  

La migración requiere la ejecución de un script de PowerShell que extraiga la configuración existente para su uso en una nueva instalación.  Se recomienda este método para catalogar la configuración del servidor existente y, a continuación, aplicarla a un servidor de almacenamiento provisional recién instalado.  Comparar los valores del servidor original con el servidor recién creado permitirá visualizar rápidamente los cambios entre los servidores.  Como siempre, siga el proceso de certificación de su organización para asegurarse de que no se requiera ninguna configuración adicional.  

### <a name="migration-process"></a>Proceso de migración 
Para migrar esta configuración, realice lo siguiente:

1. Inicie **AzureADConnect.msi** en el nuevo servidor de almacenamiento provisional y deténgase en la página principal de Azure AD Connect.

2. Copie **MigrateSettings.ps1** del directorio Microsoft Azure AD Connect\Tools en una ubicación del servidor existente.  Por ejemplo, C:\setup.  Donde setup es un directorio que se creó en el servidor existente.

   ![Conexión de directorios](media/how-to-connect-import-export-config/migrate1.png)

3. Ejecute el script como se muestra a continuación y guarde todo el directorio de configuración del servidor de nivel inferior. Copie este directorio en el nuevo servidor de almacenamiento provisional. Tenga en cuenta que debe copiar toda la carpeta **Exported-ServerConfiguration-** * al nuevo servidor.

   ![Conexión de directorios](media/how-to-connect-import-export-config/migrate2.png)
   ![Conexión de directorios](media/how-to-connect-import-export-config/migrate3.png)

5. Haga doble clic en el icono de **Azure AD Connect** en el escritorio para iniciarlo. Acepte el CLUF, en la página siguiente, haga clic en el botón **Personalizar**. 
6. Active la casilla **Importar configuración de sincronización** y haga clic en el botón **Examinar** para examinar la carpeta Exported-ServerConfiguration-* copiada y seleccione el archivo MigratedPolicy.json para importar la configuración migrada.

   ![Conexión de directorios](media/how-to-connect-import-export-config/migrate4.png)

7. Para comparar la configuración migrada con la configuración aplicada, busque el último archivo **Migrated-SynchronizationPolicy-*.JSON** y **Applied-SynchronizationPolicy-*.JSON** (* es la marca de tiempo) en **%ProgramData%\AADConnect**. Use la herramienta de comparación de archivos que prefiera para comparar la paridad. 

## <a name="post-installation-verification"></a>Verificación posterior a la instalación 

Comparar el archivo de configuración importado originalmente con el archivo de configuración exportado, del servidor recién implementado, es un paso esencial para comprender las diferencias entre la implementación prevista y la implementación resultante. Usar su aplicación favorita para la comparación de texto en paralelo permite obtener una visualización instantánea que resalta rápidamente cualquier cambio deseado o accidental. Si bien ahora se han eliminado muchos pasos de configuración que antes eran manuales, debe seguir el proceso de certificación de su organización para asegurarse de que no se requiere ninguna configuración adicional. Esta configuración puede producirse si se aprovecha la configuración avanzada, que no se captura actualmente en la versión preliminar pública de la administración de configuración. 

Entre las limitaciones conocidas se incluyen las siguientes: 
- **Reglas de sincronización** : La prioridad de una regla personalizada debe estar en el rango reservado de 0 a 99 para evitar conflictos con las reglas estándar de Microsoft. La colocación de una regla personalizada fuera del rango reservado puede dar lugar a que la regla personalizada se cambie a medida que se agreguen reglas estándar a la configuración. Se producirá un problema similar si la configuración contiene reglas estándar modificadas. Se recomienda encarecidamente no modificar una regla estándar; además, es probable que la ubicación de la regla sea incorrecta. 
- **Escritura diferida de dispositivo** : esta configuración está catalogada, pero no se aplica actualmente durante la configuración. Si se ha habilitado la escritura diferida de dispositivo para el servidor original, debe configurar manualmente la característica en el servidor implementado recientemente. 
- **Tipos de objeto sincronizados** : Aunque es posible restringir la lista de tipos de objeto sincronizados (usuarios, contactos, grupos, etc.) mediante Synchronization Service Manager, esta característica no se admite actualmente a través de la configuración de sincronización. Después de completar la instalación, debe volver a aplicar manualmente la configuración avanzada. 
- **Perfiles de ejecución personalizados** : Aunque es posible modificar el conjunto predeterminado de perfiles de ejecución mediante Synchronization Service Manager, esta característica no se admite actualmente a través de la configuración de sincronización. Después de completar la instalación, debe volver a aplicar manualmente la configuración avanzada. 
- **Configuración de la jerarquía de aprovisionamiento** : Esta característica avanzada de Synchronization Service Manager no se admite a través de la configuración de sincronización y se debe volver a configurar manualmente después de completar la implementación inicial. 
- **Autenticación de AD FS y PingFederate** : Los métodos de inicio de sesión asociados a estas características de autenticación se preseleccionarán automáticamente; sin embargo, debe proporcionar de forma interactiva todos los demás parámetros de configuración necesarios. 
- **Una regla de sincronización personalizada deshabilitada se importa como habilitada** Una regla de sincronización personalizada deshabilitada se importa como habilitada. Asegúrese de deshabilitarla también en el nuevo servidor.

 ## <a name="next-steps"></a>Pasos siguientes

- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Configuración personalizada](how-to-connect-install-custom.md)
- [Instalación de agentes de Azure AD Connect Health](how-to-connect-health-agent-install.md) 
