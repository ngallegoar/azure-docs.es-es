---
title: Instalación de Azure FarmBeats
description: En este artículo se describe cómo instalar Azure FarmBeats en la suscripción de Azure.
author: usha-rathnavel
ms.topic: article
ms.date: 1/17/2020
ms.author: atinb
ms.openlocfilehash: 5863dcc20fb13f1bb203c68ad168655371130601
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674555"
---
# <a name="install-azure-farmbeats"></a>Instalación de Azure FarmBeats

En este artículo se describe cómo instalar Azure FarmBeats en la suscripción de Azure.

Azure FarmBeats es una oferta de negocio a negocio disponible en Azure Marketplace. Permite la agregación de conjuntos de datos de agricultura entre proveedores y la generación de información procesable. Azure FarmBeats permite crear modelos de inteligencia artificial (AI) o de aprendizaje automático (ML) basados en conjuntos de datos combinados. Los dos componentes principales de Azure FarmBeats son:

- **Centro de datos** : Una capa de API que permite la agregación, normalización y contextualización de varios conjuntos de datos agrícolas entre distintos proveedores.

- **Acelerador** : aplicación web que se basa en el centro de datos. Sirve de inicio en el desarrollo y visualización del modelo. El acelerador usa las API de Azure FarmBeats para mostrar la visualización de los datos ingeridos de los sensores como gráficos y la visualización de la salida del modelo como mapas.

## <a name="general-information"></a>Información general

### <a name="components-installed"></a>Componentes instalados

Al instalar Azure FarmBeats, se aprovisionan los siguientes recursos en la suscripción de Azure:

| Recursos de Azure instalados  | Componente de Azure FarmBeats  |
|---------|---------|
| Application Insights   |      Centro de datos y acelerador      |
| App Service     |     Centro de datos y acelerador     |
| Plan de servicio de aplicación   | Centro de datos y acelerador  |
| API Connection    |  Centro de datos       |
| Azure Cache for Redis       | Centro de datos      |
| Azure Cosmos DB   |  Centro de datos       |
| Azure Data Factory V2       |     Centro de datos y acelerador      |
| Cuenta de Azure Batch    | Centro de datos   |
| Azure Key Vault |  Centro de datos y acelerador        |
| Cuenta de Azure Maps       |     Acelerador    |
| Espacio de nombres del centro de eventos    |     Centro de datos      |
| Aplicación lógica      |  Centro de datos       |
| Cuenta de almacenamiento      |     Centro de datos y acelerador      |
| Time Series Insights     |    Centro de datos    |

### <a name="costs-incurred"></a>Costos en los que se incurre

El costo de Azure FarmBeats es una agregación del costo de los servicios de Azure subyacentes. La información de precios de los servicios de Azure se puede calcular con la [Calculadora de precios](https://azure.microsoft.com/pricing/calculator). El costo real de la instalación total variará en función del uso. El costo del estado estable para los dos componentes es:

- Centro de datos: menos de 10 $ al día
- Acelerador: menos de 2 $ al día

### <a name="regions-supported"></a>Regiones admitidas

Actualmente, Azure FarmBeats se admite en entornos de nube pública en las siguientes regiones:

- Este de Australia
- Centro de EE. UU.
- Este de EE. UU.
- Este de EE. UU. 2
- Oeste de EE. UU.
- Oeste de EE. UU. 2
- Norte de Europa
- Oeste de Europa
- Este de Asia
- Sudeste de Asia

### <a name="time-taken"></a>Tiempo empleado

La configuración completa de Azure FarmBeats, incluidas la preparación y la instalación, tardará menos de una hora.

## <a name="prerequisites"></a>Requisitos previos

Para iniciar la instalación real de Azure FarmBeats, debe completar los pasos siguientes:

### <a name="verify-permissions"></a>Comprobación de los permisos

Necesita los siguientes permisos en el inquilino de Azure para instalar Azure FarmBeats:

- Inquilino: creador de la aplicación de AAD
- Suscripción: propietario
- Grupo de recursos en el que se va a instalar FarmBeats: propietario

Los dos primeros permisos son necesarios para el paso [Creación de una aplicación de AAD](#create-an-aad-application). En caso necesario, puede hacer que alguien con los permisos adecuados cree la aplicación de AAD.

La persona que ejecuta la instalación de FarmBeats desde Marketplace debe ser propietario del grupo de recursos en el que se va a instalar FarmBeats. En el caso de los propietarios de suscripciones, esto se produce automáticamente cuando se crea el grupo de recursos. Para otros usuarios, cree previamente el grupo de recursos y solicite al propietario de la suscripción que le convierta en propietario del grupo de recursos.

Puede comprobar los permisos de acceso en Azure Portal si sigue las instrucciones del [Control de acceso basado en roles de Azure](../../role-based-access-control/check-access.md).

### <a name="decide-subscription-and-region"></a>Elección de la suscripción y la región

Necesitará el identificador de suscripción de Azure y la región en la que desea instalar Azure FarmBeats. Elija una de las regiones que aparecen en la sección [Regiones admitidas](#regions-supported).

Anote el **Identificador de suscripción de Azure** y la **Región de Azure** .

### <a name="create-an-aad-application"></a>Creación de una aplicación de AAD

Azure FarmBeats exige la creación y el registro de una aplicación de Azure Active Directory. Para ejecutar correctamente el script de creación de AAD, se necesitan los siguientes permisos:

- Inquilino: creador de la aplicación de AAD
- Suscripción: propietario

Ejecute los pasos siguientes en una instancia de Cloud Shell mediante el entorno de PowerShell. A los usuarios noveles se les pide que seleccionen una suscripción y que creen una cuenta de almacenamiento. Complete la instalación tal como se indica.

1. Descarga del [script generador de aplicaciones de AAD](https://aka.ms/FarmBeatsAADScript)

    ```azurepowershell-interactive
        wget -q https://aka.ms/FarmBeatsAADScript -O ./create_aad_script.ps1
    ```

2. De forma predeterminada, el archivo se descarga en el directorio principal. Vaya al directorio.

    ```azurepowershell-interactive
        cd
    ```

3. Ejecución del script de AAD

    ```azurepowershell-interactive
        ./create_aad_script.ps1
    ```

4. El script solicita las tres entradas siguientes:

    - **Nombre del sitio web de FarmBeats** : este es el prefijo de dirección URL único para la aplicación web FarmBeats. En caso de que el prefijo ya se haya usado, el script generará un error. Una vez instalado, se podrá acceder a la implementación de FarmBeats desde https://\<FarmBeats-website-name>.azurewebsites.net y las API de Swagger estarán en https://\<FarmBeats-website-name>-api.azurewebsites.net

    - **Identificador de inicio de sesión de Azure** : proporcione el identificador de inicio de sesión de Azure del usuario que quiere agregar como administrador de FarmBeats. Luego, este usuario puede conceder a otros usuarios acceso a la aplicación web FarmBeats. El identificador de inicio de sesión suele tener el formato john.doe@domain.com. También se admite el UPN de Azure.

    - **Id. de suscripción** : este es el identificador de la suscripción en la que quiere instalar Azure FarmBeats.

5. El script de AAD tarda unos 2 minutos en ejecutarse y presenta los valores en pantalla y en un archivo JSON en el mismo directorio. Si otra persona ejecuta el script, pídale que comparta esta salida con usted.

### <a name="create-sentinel-account"></a>Creación de una cuenta de Sentinel

La instalación de Azure FarmBeats permite obtener imágenes de la granja de la misión del satélite [Sentinel-2](https://scihub.copernicus.eu/) de la Agencia Espacial Europea. Para configurar esta instalación, necesita una cuenta de Sentinel.

Siga estos pasos para crear una cuenta gratuita de Sentinel:

1. Vaya a la página oficial de [registro](https://aka.ms/SentinelRegistration).
2. Proporcione los detalles necesarios (nombre, apellido, nombre de usuario, contraseña e identificador de correo electrónico) y rellene el formulario.
3. Se envía un vínculo de comprobación al identificador de correo electrónico registrado. Seleccione el vínculo proporcionado en el correo electrónico y complete la comprobación.

El proceso de registro se he completado. Anote el **nombre de usuario de Sentinel** y la **contraseña de Sentinel** una vez completada la comprobación.

## <a name="install"></a>Instalar

Ya está preparado para instalar FarmBeats. Siga los pasos siguientes para iniciar la instalación:

1. Inicie sesión en Azure Portal. Seleccione la cuenta en la esquina superior derecha y cambie al inquilino de Azure AD en el que quiere instalar Azure FarmBeats.

2. Vaya a Azure Marketplace en el portal y busque **Azure FarmBeats** en Marketplace.

3. Aparece una nueva ventana con información general sobre Azure FarmBeats. Seleccione **Crear** .

4. Aparece una nueva ventana. Complete el proceso de registro; para ello, elija la suscripción, el grupo de recursos y la ubicación correctos en los que desea instalar Azure FarmBeats.

5. Proporcione la dirección de correo electrónico que debe recibir las alertas de servicio relacionadas con Azure FarmBeats en la sección **Alertas de servicio de FarmBeats** . Seleccione **Siguiente** en la parte inferior de la página para ir a la pestaña **Dependencias** .

    ![Pestaña Aspectos básicos](./media/install-azure-farmbeats/create-azure-farmbeats-basics.png)

6. Copie las entradas individuales de la salida de [Script de AAD](#create-an-aad-application) en las entradas de la sección de aplicación de AAD.

7. Escriba el nombre de usuario y la contraseña de la [cuenta de Sentinel](#create-sentinel-account) en la sección Cuenta de Sentinel. Seleccione **Siguiente** para ir a la pestaña **Revisar y crear** .

    ![Pestaña Dependencias](./media/install-azure-farmbeats/create-azure-farmbeats-dependencies.png)

8. Una vez validados los detalles especificados, seleccione **Aceptar** . Aparece la página Términos de uso. Revise los términos y seleccione **Crear** para iniciar la instalación. Se le redirige a la página en la que puede seguir el progreso de la instalación.

Finalizada la instalación, puede comprobarla y empezar a usar el portal de FarmBeats. Para ello, vaya al nombre del sitio web proporcionado durante la instalación: https://\<FarmBeats-website-name>.azurewebsites.net. Debería ver la interfaz de usuario de FarmBeats con una opción para crear granjas.

**Datahub** se puede encontrar en https://\<FarmBeats-website-name>-api.azurewebsites.net/swagger. En él se ven los distintos objetos de la API de FarmBeats y se realizan operaciones REST en las API.

## <a name="upgrade"></a>Actualizar

Para actualizar FarmBeats a la versión más reciente, ejecute los pasos siguientes en una instancia de Cloud Shell mediante el entorno de PowerShell. El usuario debe ser el propietario de la suscripción en la que está instalado FarmBeats.

A los usuarios noveles se les pide que seleccionen una suscripción y que creen una cuenta de almacenamiento. Complete la instalación tal como se indica.

1. Descarga del [script de actualización](https://aka.ms/FarmBeatsUpgradeScript)

    ```azurepowershell-interactive
        wget –q https://aka.ms/FarmBeatsUpgradeScript -O ./upgrade-farmbeats.ps1
    ```

2. De forma predeterminada, el archivo se descarga en el directorio principal. Vaya al directorio.

    ```azurepowershell-interactive
        cd
    ```

3. Ejecución del script de actualización

    ```azurepowershell-interactive
        ./upgrade-farmbeats.ps1 -InputFilePath [Path to input.json file]
    ```

La ruta de acceso al archivo input.json es opcional. Si no se especifica, el script le pide todas las entradas necesarias. La actualización debe finalizar en unos 30 minutos.

## <a name="uninstall"></a>Desinstalación

Para desinstalar el centro de datos o el acelerador de Azure FarmBeats, complete los pasos siguientes:

1. Inicie sesión en Azure Portal y **elimine los grupos de recursos** en los que se instalaron estos componentes.

2. Vaya a Azure Active Directory y **elimine la aplicación de Azure AD** vinculada a la instalación de Azure FarmBeats.

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido cómo instalar Azure FarmBeats en la suscripción de Azure. Ahora, obtenga información sobre cómo [agregar usuarios](manage-users-in-azure-farmbeats.md#manage-users) a la instancia de Azure FarmBeats.