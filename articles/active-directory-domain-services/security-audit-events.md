---
title: Habilitación de auditorías de seguridad para Azure AD Domain Services | Microsoft Docs
description: Aprenda a habilitar auditorías de seguridad para centralizar el registro de eventos para el análisis y las alertas en Azure AD Domain Services
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 662362c3-1a5e-4e94-ae09-8e4254443697
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: b2138818a9092999dd54b14664f7146f087c4fed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78328653"
---
# <a name="enable-security-audits-for-azure-active-directory-domain-services"></a>Habilitación de auditorías de seguridad para Azure Active Directory Domain Services

Las auditorías de seguridad de Azure Active Directory Domain Services (Azure AD DS) permiten a Azure transmitir eventos de seguridad a recursos de destino. Estos recursos incluyen Azure Storage, áreas de trabajo de Azure Log Analytics o Azure Event Hubs. Después de habilitar los eventos de auditoría de seguridad, Azure AD DS envía todos los eventos auditados de la categoría seleccionada al recurso de destino.

Puede archivar eventos en Azure Storage y transmitir eventos a software de administración de eventos e información de seguridad (SIEM) (o equivalente) mediante Azure Event Hubs, o bien realizar sus propios análisis y usar áreas de trabajo de Azure Log Analytics en Azure Portal.

> [!IMPORTANT]
> Las auditorías de seguridad de Azure AD DS solo están disponibles para instancias basadas en Azure Resource Manager. Para obtener información sobre cómo migrar, vea [Versión preliminar: Migración de Azure AD Domain Services desde el modelo de red virtual clásica a Resource Manager][migrate-azure-adds].

## <a name="audit-event-categories"></a>Categorías de eventos de auditoría

Las auditorías de seguridad de Azure AD DS se alinean con las auditorías tradicionales de los controladores de dominio de AD DS tradicionales. En entornos híbridos, puede reutilizar patrones de auditoría existentes para que se pueda usar la misma lógica al analizar los eventos. En función del escenario que deba analizar o cuyos problemas deba solucionar, es necesario establecer como destino las distintas categorías de eventos de auditoría.

Hay disponibles las siguientes categorías de eventos de auditoría:

| Nombre de categoría de auditoría | Descripción |
|:---|:---|
| Inicio de sesión de la cuenta|Audita los intentos para autenticar los datos de la cuenta en un controlador de dominio o en un administrador de cuentas de seguridad (SAM) local.</p>Configuración de la directiva de inicio y cierre de sesión e intentos de seguimiento de eventos para acceder a un equipo determinado. La configuración y los eventos de esta categoría se centran en la base de datos de cuenta que se está utilizando. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar validación de credenciales](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-credential-validation)</li><li>[Auditar servicio de autenticación Kerberos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-authentication-service)</li><li>[Auditar operaciones de vales de servicio Kerberos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations)</li><li>[Auditar otros eventos de inicio y cierre de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li></ul>|
| Administración de cuentas|Audita los cambios en grupos y cuentas de usuarios y equipos. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar administración de grupos de aplicaciones](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-group-management)</li><li>[Auditar administración de cuentas de equipo](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-computer-account-management)</li><li>[Auditar administración de grupos de distribución](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-distribution-group-management)</li><li>[Auditar otros eventos de administración de cuentas](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-account-management-events)</li><li>[Auditar administración de grupos de seguridad](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-group-management)</li><li>[Auditar administración de cuentas de usuario](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-account-management)</li></ul>|
| Seguimiento de detalles|Audita las actividades de aplicaciones individuales y usuarios de ese equipo, y ayuda a entender cómo se usa un equipo. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar actividad DPAPI](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-dpapi-activity)</li><li>[Auditar actividad PNP](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-pnp-activity)</li><li>[Auditar creación de procesos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-creation)</li><li>[Auditar finalización de procesos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-termination)</li><li>[Auditar eventos de RPC](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-rpc-events)</li></ul>|
| Acceso a Directory Services|Audita los intentos para acceder y modificar objetos en Active Directory Domain Services (AD DS). Estos eventos de auditoría se registran solo en controladores de dominio. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar replicación de servicio de directorio detallada](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-directory-service-replication)</li><li>[Auditar el acceso del servicio de directorio](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-access)</li><li>[Auditar cambios de servicio de directorio](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)</li><li>[Auditar replicación de servicio de directorio](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-replication)</li></ul>|
| Cierre de sesión-Inicio de sesión|Audita los intentos de iniciar sesión en un equipo de forma interactiva o a través de una red. Estos eventos son útiles para el seguimiento de la actividad de usuario y la identificación de posibles ataques contra los recursos de red. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar bloqueo de cuentas](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-account-lockout)</li><li>[Auditar reclamaciones de usuario o dispositivo](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-device-claims)</li><li>[Auditar modo extendido de IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-extended-mode)</li><li>[Auditar pertenencia a grupos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-group-membership)</li><li>[Auditar modo principal de IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-main-mode)</li><li>[Auditar modo rápido de IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-quick-mode)</li><li>[Auditar cierre de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logoff)</li><li>[Auditar inicio de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logon)</li><li>[Auditar servidor de directivas de redes](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-network-policy-server)</li><li>[Auditar otros eventos de inicio y cierre de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li><li>[Auditar inicio de sesión especial](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-special-logon)</li></ul>|
|Acceso a objetos| Audita los intentos de acceso a determinados objetos o tipos de objetos en un equipo o red. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar aplicación generada](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-generated)</li><li>[Auditar servicios de certificación](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-certification-services)</li><li>[Auditar recurso compartido de archivos detallado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-file-share)</li><li>[Auditar recurso compartido de archivos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-share)</li><li>[Auditar sistema de archivos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-system)</li><li>[Auditar conexión de plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection)</li><li>[Auditar colocación de paquetes de la plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)</li><li>[Auditar manipulación de identificadores](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-handle-manipulation)</li><li>[Auditar objeto de kernel](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kernel-object)</li><li>[Auditar otros eventos de acceso a objetos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-object-access-events)</li><li>[Auditar Registro](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-registry)</li><li>[Auditar almacenamiento extraíble](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-removable-storage)</li><li>[Auditar SAM](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sam)</li><li>[Auditar almacenamiento provisional de directiva de acceso central](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-central-access-policy-staging)</li></ul>|
|Cambio de directiva|Audita los cambios en directivas de seguridad importantes en una red o sistema local. Los administradores suelen ser los responsables de establecer las directivas para ayudar a proteger los recursos de red. La supervisión de los cambios o intentos para cambiar estas directivas puede ser un aspecto importante de la administración de seguridad para una red. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar cambio de directiva de auditoría](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-audit-policy-change)</li><li>[Auditar cambio de directiva de autenticación](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authentication-policy-change)</li><li>[Auditar cambio de directiva de autorización](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authorization-policy-change)</li><li>[Auditar cambio de directiva de la plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-policy-change)</li><li>[Auditar cambio de directiva del nivel de reglas de MPSSVC](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-mpssvc-rule-level-policy-change)</li><li>[Auditar otros eventos de cambio de directiva](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-policy-change-events)</li></ul>|
|Uso de privilegios| Audita el uso de determinados permisos en uno o varios sistemas. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar uso de privilegios no confidenciales](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-non-sensitive-privilege-use)</li><li>[Auditar uso de privilegios confidenciales](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sensitive-privilege-use)</li><li>[Auditar otros eventos de uso de privilegios](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-privilege-use-events)</li></ul>|
|Sistema| Audita los cambios del nivel de sistema en un equipo no incluido en otras categorías y que tienen posibles implicaciones de seguridad. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar controlador IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-driver)</li><li>[Auditar otros eventos del sistema](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-system-events)</li><li>[Auditar cambio de estado de seguridad](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-state-change)</li><li>[Auditar extensión del sistema de seguridad](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-system-extension)</li><li>[Auditar integridad del sistema](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-system-integrity)</li></ul>|

## <a name="event-ids-per-category"></a>Identificadores de eventos por categoría

 Las auditorías de seguridad de Azure AD DS registran los siguientes identificadores de eventos cuando la acción específica desencadena un evento que se puede auditar:

| Nombre de categoría de eventos | Id. de evento |
|:---|:---|
|Seguridad de inicio de sesión de la cuenta|4767, 4774, 4775, 4776, 4777|
|Seguridad de administración de la cuenta|4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4730, 4731, 4732, 4733, 4734, 4735, 4737, 4738, 4740, 4741, 4742, 4743, 4754, 4755, 4756, 4757, 4758, 4764, 4765, 4766, 4780, 4781, 4782, 4793, 4798, 4799, 5376, 5377|
|Seguridad de seguimiento de detalles|None|
|Seguridad de acceso de DS|5136, 5137, 5138, 5139, 5141|
|Seguridad de cierre de sesión-inicio de sesión|4624, 4625, 4634, 4647, 4648, 4672, 4675, 4964|
|Seguridad de acceso de objeto|None|
|Seguridad de cambio de directiva|4670, 4703, 4704, 4705, 4706, 4707, 4713, 4715, 4716, 4717, 4718, 4719, 4739, 4864, 4865, 4866, 4867, 4904, 4906, 4911, 4912|
|Seguridad de uso de privilegios|4985|
|Seguridad del sistema|4612, 4621|

## <a name="security-audit-destinations"></a>Destinos de auditorías de seguridad

Puede usar Azure Storage, Azure Event Hubs o áreas de trabajo de Azure Log Analytics como recurso de destino para las auditorías de seguridad de Azure AD DS. Estos destinos se pueden combinar. Por ejemplo, puede usar Azure Storage para archivar eventos de auditoría de seguridad, pero un área de trabajo de Azure Log Analytics para analizar la información a corto plazo y notificarla.

En la tabla siguiente se esbozan los escenarios para cada tipo de recurso de destino.

> [!IMPORTANT]
> Debe crear el recurso de destino antes de habilitar las auditorías de seguridad de Azure AD DS. Puede crear estos recursos mediante Azure Portal, Azure PowerShell o la CLI de Azure.

| Recurso de destino | Escenario |
|:---|:---|
|Azure Storage| Este destino se debe usar cuando la necesidad principal es almacenar eventos de auditoría de seguridad para fines de archivado. Se pueden usar otros destinos para fines de archivado, pero esos destinos proporcionan funcionalidades que van más allá de la principal necesidad de archivado. <br /><br />Antes de habilitar eventos de auditoría de seguridad de Azure AD DS, [cree una cuenta de Azure Storage](../storage/common/storage-account-create.md).|
|Azure Event Hubs| Este destino debe usarse cuando la principal necesidad es compartir eventos de auditoría de seguridad con otro software, como software de análisis de datos o software de administración de eventos e información de seguridad (SIEM).<br /><br />Antes de habilitar eventos de auditoría de seguridad de Azure AD DS, [cree un centro de eventos mediante Azure Portal](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)|
|Área de trabajo de Azure Log Analytics| Este destino debe usarse cuando la principal necesidad es analizar y revisar auditorías de seguridad directamente en Azure Portal.<br /><br />Antes de habilitar eventos de auditoría de seguridad de Azure AD DS, [cree un área de trabajo de Log Analytics en Azure Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).|

## <a name="enable-security-audit-events-using-the-azure-portal"></a>Habilitar eventos de auditoría de seguridad mediante Azure Portal

Para habilitar eventos de auditoría de seguridad de Azure AD DS mediante Azure Portal, siga los pasos siguientes.

> [!IMPORTANT]
> Las auditorías de seguridad de Azure AD DS no son retroactivas. No se pueden recuperar ni reproducir eventos del pasado. Azure AD DS solo puede enviar los eventos que se producen después de la habilitación de las auditorías de seguridad.

1. Inicie sesión en Azure Portal en https://portal.azure.com.
1. En la parte superior de Azure Portal, busque y seleccione **Azure AD Domain Services**. Elija el dominio administrado como, por ejemplo, *aaddscontoso.com*.
1. En la ventana de Azure AD DS, seleccione **Configuración de diagnóstico** en el lado izquierdo.
1. No hay ningún diagnóstico configurado de forma predeterminada. Para empezar, seleccione **Agregar configuración de diagnóstico**.

    ![Agregar configuración de diagnóstico de Azure AD Domain Services](./media/security-audit-events/add-diagnostic-settings.png)

1. Escriba un nombre para la configuración de diagnóstico, como *aadds-auditing*.

    Active la casilla correspondiente al destino de auditoría de seguridad que quiere. Puede elegir entre una cuenta de Azure Storage, un centro de eventos de Azure o un área de trabajo de Log Analytics. Estos recursos de destino ya deben existir en la suscripción de Azure. No es posible crear los recursos de destino en este asistente.

    ![Habilitación del destino necesario y el tipo de eventos de auditoría que se van a capturar](./media/security-audit-events/diagnostic-settings-page.png)

    * **Almacenamiento de Azure**
        * Seleccione **Archivar en una cuenta de almacenamiento** y luego **Configurar**.
        * Seleccione la **Suscripción** y la **Cuenta de almacenamiento** que quiere usar para archivar los eventos de auditoría de seguridad.
        * Cuando esté listo, seleccione **Aceptar**.
    * **Azure Event Hubs**
        * Seleccione **Transmitir a un centro de eventos** y luego **Configurar**.
        * Seleccione la **Suscripción** y el **Espacio de nombres del centro de eventos**. En caso necesario, seleccione también un **Nombre del centro de eventos** y luego un **Nombre de directiva de centro de eventos**.
        * Cuando esté listo, seleccione **Aceptar**.
    * **Áreas de trabajo de Azure Log Analytics**
        * Seleccione **Enviar a Log Analytics** y luego la **Suscripción** y el **Área de trabajo de Log Analytics** que quiere usar para almacenar los eventos de auditoría de seguridad.

1. Seleccione las categorías de registro que desee incluir para el recurso de destino determinado. Si envía los eventos de auditoría a una cuenta de Azure Storage, también puede configurar una directiva de retención que defina el número de días que se conservan los datos. Un valor predeterminado de *0* conserva todos los datos y no rota eventos tras un período de tiempo.

    Puede seleccionar diferentes categorías de registro para cada recurso de destino dentro de una única configuración. Esto le permite elegir qué categorías de registros de Log Analytics quiere conservar y qué categorías de registros quiere archivar, por ejemplo.

1. Cuando termine, seleccione **Guardar** para confirmar los cambios. Los recursos de destino empiezan a recibir eventos de auditoría de seguridad de Azure AD DS poco después de que se guarde la configuración.

## <a name="enable-security-audit-events-using-azure-powershell"></a>Habilitar eventos de auditoría de seguridad con Azure PowerShell

Para habilitar eventos de auditoría de seguridad de Azure AD DS mediante Azure PowerShell, siga estos pasos. Si es necesario, primero [instale el módulo Azure PowerShell y conéctese a la suscripción de Azure](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Las auditorías de seguridad de Azure AD DS no son retroactivas. No se pueden recuperar ni reproducir eventos del pasado. Azure AD DS solo puede enviar los eventos que se producen después de la habilitación de las auditorías de seguridad.

1. Autentíquese en la suscripción de Azure con el cmdlet [Connect-AzAccount](/powershell/module/Az.Accounts/Connect-AzAccount). Cuando se le solicite, escriba las credenciales de la cuenta.

    ```azurepowershell
    Connect-AzAccount
    ```

1. Cree el recurso de destino para los eventos de auditoría de seguridad.

    * **Azure Storage** - [Creación de una cuenta de almacenamiento mediante Azure PowerShell](../storage/common/storage-account-create.md?tabs=azure-powershell)
    * **Azure Event Hubs** - [Creación de un centro de eventos mediante Azure PowerShell](../event-hubs/event-hubs-quickstart-powershell.md) Es posible que también deba usar el cmdlet [New-AzEventHubAuthorizationRule](/powershell/module/az.eventhub/new-azeventhubauthorizationrule) para crear una regla de autorización que conceda permisos de Azure AD DS al *espacio de nombres* del centro de eventos. La regla de autorización debe incluir los derechos para **Administrar**, **Escuchar** y **Enviar**.

        > [!IMPORTANT]
        > Asegúrese de establecer la regla de autorización en el espacio de nombres del centro de eventos y no en el propio centro de eventos.

    * **Áreas de trabajo de Azure Log Analytic** - [Creación de un área de trabajo de Log Analytics con Azure PowerShell](../azure-monitor/learn/quick-create-workspace-posh.md)

1. Obtenga el identificador de recurso del dominio administrado de Azure AD DS con el cmdlet [Get-AzResource](/powershell/module/Az.Resources/Get-AzResource). Cree una variable denominada *$aadds.ResourceId* para conservar el valor:

    ```azurepowershell
    $aadds = Get-AzResource -name aaddsDomainName
    ```

1. Emplee el cmdlet [Set-AzDiagnosticSetting](/powershell/module/Az.Monitor/Set-AzDiagnosticSetting) para definir la configuración de Azure Diagnostics a fin de usar el recurso de destino de los eventos de auditoría de seguridad de Azure AD Domain Services. En los ejemplos siguientes, se usa la variable *$aadds.ResourceId* del paso anterior.

    * **Azure Storage**: reemplace *storageAccountId* por el nombre de la cuenta de almacenamiento:

        ```powershell
        Set-AzDiagnosticSetting `
            -ResourceId $aadds.ResourceId `
            -StorageAccountId storageAccountId `
            -Enabled $true
        ```

    * **Azure Event Hubs**: reemplace *eventHubName* por el nombre del centro de eventos y *eventHubRuleId* por el identificador de la regla de autorización:

        ```powershell
        Set-AzDiagnosticSetting -ResourceId $aadds.ResourceId `
            -EventHubName eventHubName `
            -EventHubAuthorizationRuleId eventHubRuleId `
            -Enabled $true
        ```

    * **Áreas de trabajo de Azure Log Analytics**: reemplace *workspaceId* por el identificador del área de trabajo de Log Analytics:

        ```powershell
        Set-AzureRmDiagnosticSetting -ResourceId $aadds.ResourceId `
            -WorkspaceID workspaceId `
            -Enabled $true
        ```

## <a name="query-and-view-security-audit-events-using-azure-monitor"></a>Consultar y ver eventos de auditoría de seguridad con Azure Monitor

Las áreas de trabajo de Log Analytics permiten ver y analizar los eventos de auditoría de seguridad mediante Azure Monitor y el lenguaje de consulta Kusto. Este lenguaje de consulta está diseñado para un uso de solo lectura que ofrece eficaces funcionalidades analíticas con una sintaxis fácil de leer. Para obtener más información para empezar a usar el lenguaje de consulta Kusto, vea los siguientes artículos:

* [Documentación sobre Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/)
* [Introducción a los análisis de registros de Azure Monitor](../azure-monitor/log-query/get-started-portal.md)
* [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/get-started-queries.md)
* [Creación y uso compartido de paneles de datos de Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md)

Las consultas de ejemplo siguientes se pueden usar para empezar a analizar eventos de auditoría de seguridad de Azure AD DS.

### <a name="sample-query-1"></a>Consulta de ejemplo 1

Vea todos los eventos de bloqueo de cuentas de los últimos siete días:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

### <a name="sample-query-2"></a>Consulta de ejemplo 2

Vea todos los eventos de bloqueo de cuentas (*4740*) entre el 3 de febrero de 2020 a las 9:00 y el 10 de febrero de 2020 a medianoche, ordenados de forma ascendente por fecha y hora:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2020-02-03 09:00) and TimeGenerated <= datetime(2020-02-10)
| where OperationName has "4740"
| sort by TimeGenerated asc
```

### <a name="sample-query-3"></a>Consulta de ejemplo 3

Vea eventos de inicio de sesión de cuenta de hace siete días (a partir de ahora) de la cuenta denominada user:

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-4"></a>Consulta de ejemplo 4

Vea eventos de inicio de sesión de cuenta de hace siete días a partir de ahora de la cuenta denominada user que ha intentado iniciar sesión con una contraseña incorrecta (*0xC0000006a*):

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc000006a" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-5"></a>Consulta de ejemplo 5

Vea eventos de inicio de sesión de cuenta de hace siete días a partir de ahora de la cuenta denominada user que ha intentado iniciar sesión mientras la cuenta estaba bloqueada (*0xC0000234*):

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-6"></a>Consulta de ejemplo 6

Vea el número de eventos de inicio de sesión de cuenta de hace siete días a partir de ahora de todos los intentos de inicio de sesión que se han producido por parte de todos los usuarios bloqueados:

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| summarize count()
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información concreta sobre Kusto, vea los siguientes artículos:

* [Introducción](/azure/kusto/query/) al lenguaje de consulta Kusto.
* [Tutorial de Kusto](/azure/kusto/query/tutorial) para familiarizarse con los conceptos básicos de consultas.
* [Consultas de ejemplo](/azure/kusto/query/samples) que le ayudan a aprender nuevas maneras de ver los datos.
* [Procedimientos recomendados](/azure/kusto/query/best-practices) de Kusto para optimizar las consultas para un mejor rendimiento.

<!-- LINKS - Internal -->
[migrate-azure-adds]: migrate-from-classic-vnet.md
