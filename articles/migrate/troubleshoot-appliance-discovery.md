---
title: Solución de problemas de la implementación o detección de un dispositivo de Azure Migrate
description: Obtenga ayuda para implementar el dispositivo de Azure Migrate y detectar máquinas.
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 9fbf55fbe16d958bf10541894159dade26668bef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336724"
---
# <a name="troubleshoot-the-azure-migrate-appliance-and-discovery"></a>Solución de problemas del dispositivo de Azure Migrate y la detección

Este artículo le ayuda a solucionar los problemas que surgen al implementar el dispositivo de [Azure Migrate](migrate-services-overview.md) y el uso de dicho dispositivo para detectar máquinas locales.


## <a name="whats-supported"></a>¿Qué se admite?

[Revise](migrate-appliance.md) los requisitos de compatibilidad del dispositivo.


## <a name="invalid-ovf-manifest-entry"></a>"Entrada de manifiesto de OVF no válida"

Si recibe el error "The provided manifest file is invalid: Invalid OVF manifest entry" (El archivo de manifiesto proporcionado no es válido: entrada de manifiesto de OVF no válida), haga lo siguiente:

1. Compruebe si el archivo OVA del dispositivo de Azure Migrate se descargó correctamente; para ello, compruebe el valor hash. [Más información](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Si el valor hash no coincide, vuelva a descargar el archivo OVA e intente de nuevo la implementación.
2. Si la implementación sigue sin funcionar y utiliza el cliente de VMware vSphere para implementar el archivo OVF, intente implementarlo mediante el cliente web de vSphere. Si la implementación sigue sin funcionar, intente usar otro explorador web.
3. Si usa el cliente web de vSphere e intenta implementarlo en vCenter Server 6.5 o 6.7, pruebe a implementar el archivo OVA directamente en el host ESXi:
   - Conéctese directamente al host ESXi (en lugar de vCenter Server) con el cliente web (https://<*dirección IP del host*>/ui).
   - En **Home** > **Inventory** (Inicio > Inventario), seleccione **File** > **Deploy OVF template** (Archivo > Implementar plantilla OVF). Vaya al archivo OVA y complete la implementación.
4. Si la implementación sigue sin funcionar, póngase en contacto con el soporte técnico de Azure Migrate.

## <a name="cant-connect-to-the-internet"></a>No puedo conectarme a Internet

Esto puede ocurrir si la máquina del dispositivo está detrás de un proxy.

- Asegúrese de proporcionar las credenciales de autorización en caso de que el servidor proxy las necesite.
- Si usa un proxy de firewall basado en dirección URL para controlar la conectividad de salida, agregue [estas direcciones URL](migrate-appliance.md#url-access) a una lista de permitidos.
- Si usa un servidor proxy de interceptación para conectarse a Internet, importe el certificado del servidor proxy en la máquina virtual del dispositivo mediante [estos pasos](https://docs.microsoft.com/azure/migrate/concepts-collector).

##  <a name="datetime-synchronization-error"></a>Error de sincronización de fecha y hora

Un error sobre la sincronización de fecha y hora (802) indica que el reloj del servidor podría estar desincronizado con la hora actual en más de cinco minutos. Cambie la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual:

1. Abra un símbolo del sistema de administrador en la máquina virtual.
2. Para comprobar la zona horaria, ejecute **w32tm /tz**.
3. Para sincronizar la hora, ejecute **w32tm /resync**.


## <a name="unabletoconnecttoserver"></a>"UnableToConnectToServer"

Si obtiene este error de conexión, es posible que no pueda conectarse a vCenter Server *NombreDelServidor*.com:9443. Los detalles del error indican que no hay ningún punto de conexión escuchando en `https://\*servername*.com:9443/sdk` que pueda aceptar el mensaje.

- Compruebe si tiene en ejecución la versión más reciente del dispositivo. Si no es así, actualice el dispositivo a la [versión más reciente](https://docs.microsoft.com/azure/migrate/concepts-collector).
- Si el problema persiste con la última versión, es posible que el dispositivo no pueda resolver el nombre de vCenter Server especificado o que el puerto especificado sea incorrecto. De forma predeterminada, si no se especifica el puerto, el recopilador intentará conectar con el número de puerto 443.

    1. Haga ping a *NombreDelServidor*.com desde el dispositivo.
    2. Si se produce un error en el paso 1, intente conectarse a vCenter Server mediante la dirección IP.
    3. Identifique el número de puerto correcto para conectarse a vCenter Server.
    4. Compruebe que vCenter Server está en funcionamiento.


## <a name="error-6005260039-appliance-might-not-be-registered"></a>Error 60052/60039: Es posible que el dispositivo no esté registrado

- Se produce el error 60052: "The appliance might not be registered successfully to the Azure Migrate project" (El dispositivo podría no estar registrado correctamente en el proyecto de Azure Migrate) si la cuenta de Azure usada para registrar el dispositivo no tiene permisos suficientes.
    - Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el permiso de colaborador de la suscripción.
    - [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) sobre los roles y permisos de Azure necesarios.
- Puede producirse el error 60039, "The appliance might not be registered successfully to the Azure Migrate project" (El dispositivo podría no estar registrado correctamente en el proyecto de Azure Migrate) si se produce un error de registro porque no se puede encontrar el proyecto de Azure Migrate utilizado para registrar el dispositivo.
    - En Azure Portal, compruebe si el proyecto existe en el grupo de recursos.
    - Si el proyecto no existe, cree un nuevo proyecto de Azure Migrate en el grupo de recursos y vuelva a registrar el dispositivo. [Obtenga información sobre](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) cómo crear un nuevo proyecto.

## <a name="error-6003060031-key-vault-management-operation-failed"></a>Error 60030/60031: Error durante la operación de administración de Key Vault

Si recibe el error 60030 o 60031, "An Azure Key Vault management operation failed" (Se ha producido un error en la operación de administración de Azure Key Vault), haga lo siguiente:
- Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el permiso de colaborador de la suscripción.
- Asegúrese de que la cuenta tiene acceso al almacén de claves especificado en el mensaje de error y vuelva a intentar la operación.
- Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.
- [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) sobre los roles y permisos de Azure necesarios.

## <a name="error-60028-discovery-couldnt-be-initiated"></a>Error 60028: No se pudo iniciar la detección.

Error 60028: "Discovery couldn't be initiated because of an error. The operation failed for the specified list of hosts or clusters" (No se pudo iniciar la detección debido a un error. Se produjo un error en la operación para la lista especificada de hosts o clústeres) indica que no se pudo iniciar la detección en los hosts listados en el error debido a un problema de acceso o recuperación de la información de la máquina virtual. El resto de los hosts se agregaron correctamente.

- Vuelva a agregar los hosts enumerados en el error mediante la opción **Agregar host**.
- Si se produce un error de validación, revise las instrucciones de solución para corregir los errores e intente la opción **Guardar e iniciar la detección** de nuevo.

## <a name="error-60025-azure-ad-operation-failed"></a>El error 60025: Error en una operación de Azure AD 
El error 60025: "An Azure AD operation failed. The error occurred while creating or updating the Azure AD application" (Error en una operación de Azure AD. El error ha ocurrido al crear o actualizar la aplicación de Azure AD) aparece cuando la cuenta de usuario de Azure que se ha utilizado para iniciar la detección es diferente de la que se ha usado para registrar el dispositivo. Realice una de las siguientes acciones:

- Asegúrese de que la cuenta de usuario que inicia la detección es la misma que se ha usado para registrar el dispositivo.
- Proporcione permisos de acceso de la aplicación de Azure Active Directory a la otra cuenta de usuario para la que se produce el error en la operación de detección.
- Elimine el grupo de recursos creado previamente para el proyecto de Azure Migrate. Cree otro grupo de recursos para empezar de nuevo.
- [Aprenda más](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) sobre los permisos de aplicación de Azure Active Directory.


## <a name="error-50004-cant-connect-to-host-or-cluster"></a>Se produce el error 50004: No se puede conectar al host o clúster

Se produce el error 50004: No se puede conectar a un host o un clúster porque no se puede resolver el nombre del servidor. Código de error WinRM: 0x803381B9" si el servicio Azure DNS del dispositivo no puede resolver el nombre de host o el clúster que ha proporcionado.

- Si ve este error en el clúster, añada el nombre de dominio completo en el clúster.
- También puede ver este error en los hosts de un clúster. Esto indica que el dispositivo puede conectarse al clúster, pero el clúster devuelve nombres de host que no son nombres de dominio completos. Para resolver este error, actualice el archivo de hosts del dispositivo; para ello, agregue una asignación de la dirección IP y los nombres de host:
    1. Abra el Bloc de notas como administrador.
    2. Abra el archivo C:\Windows\System32\Drivers\etc\hosts.
    3. Agregue la dirección IP y el nombre de host en una fila. Repita el procedimiento para cada host o clúster en el que observe este error.
    4. Guarde el archivo de hosts y ciérrelo.
    5. Compruebe si el dispositivo puede conectarse a los hosts mediante la aplicación de administración del dispositivo. Después de 30 minutos, debería ver la información más reciente sobre estos hosts en Azure Portal.

## <a name="discovered-vms-not-in-portal"></a>Máquinas virtuales detectadas no en el portal

Si el estado de detección es "Detección en curso", pero aún no ve las máquinas virtuales en el portal, espere unos minutos:
- Una máquina virtual de VMware tarda aproximadamente 15 minutos.
- Cada host agregado tarda aproximadamente dos minutos en la detección de máquinas virtuales de Hyper-V.

Si espera y el estado no cambia, seleccione **Actualizar** en la pestaña **Servidores**. Se debería mostrar el recuento de los servidores detectados en Azure Migrate: Server Assessment y Azure Migrate: Server Migration.

Si esto no funciona y está detectando servidores VMware:

- Compruebe que la cuenta de vCenter especificada tiene los permisos establecidos correctamente y que tiene acceso al menos a una máquina virtual.
- Azure Migrate no puede detectar máquinas virtuales de VMware si a la cuenta de vCenter se le ha concedido acceso en el nivel de carpeta de VM de vCenter. [Obtenga más información](set-discovery-scope.md) sobre el ámbito de la detección.

## <a name="vm-data-not-in-portal"></a>Datos de VM que no están en el portal

Si las máquinas virtuales detectadas no aparecen en el portal o si los datos de estas están obsoletos, espere unos minutos. Los cambios realizados en los datos de configuración de las máquinas virtuales detectadas tardan 30 minutos, como máximo, en aparecer en el portal. Los cambios en los datos de las aplicaciones pueden tardar pocas horas en aparecer. Si no hay datos después de ese tiempo, intente actualizar como se indica a continuación

1. En **Servidores** > **Azure Migrate Server Assessment**, seleccione **Información general**.
2. En **Administrar**, seleccione **Agent Health**.
3. Seleccione **Actualizar agente**.
4. Espere a que se complete la operación de actualización. Ahora debería ver la información actualizada.

## <a name="deleted-vms-appear-in-portal"></a>Las máquinas virtuales eliminadas aparecen en el portal

Si elimina las máquinas virtuales y siguen apareciendo en el portal, espere treinta minutos. Si siguen apareciendo, actualice como se describió anteriormente.

## <a name="common-app-discovery-errors"></a>Errores comunes de detección de aplicaciones

Azure Migrate admite la detección de aplicaciones, roles y características mediante Azure Migrate: Server Assessment. Actualmente solo se admite la detección de aplicaciones para VMware. [Obtenga más información](how-to-discover-applications.md) sobre los requisitos y los pasos para configurar la detección de aplicaciones.

Los errores de detección de aplicaciones típicos se resumen en la tabla. 

**Error** | **Causa** | **Acción**
--- | --- | --- | ---
10000: "No se pueden detectar las aplicaciones instaladas en el servidor". | Esto puede ocurrir si el sistema operativo de la máquina no es Windows o Linux. | Use solo detección de aplicaciones para Windows o Linux.
10001: "No se pueden recuperar las aplicaciones instaladas en el servidor". | Error interno: faltan algunos archivos en el dispositivo. | Póngase en contacto con el soporte técnico de Microsoft
10002: "No se pueden recuperar las aplicaciones instaladas en el servidor". | Es posible que el agente de detección del dispositivo no funcione correctamente. | Si el problema no se resuelve en un plazo de 24 horas, póngase en contacto con el soporte técnico.
10003 "No se pueden recuperar las aplicaciones instaladas en el servidor". | Es posible que el agente de detección del dispositivo no funcione correctamente. | Si el problema no se resuelve en un plazo de 24 horas, póngase en contacto con el soporte técnico.
10004: "No se pueden detectar las aplicaciones instaladas para máquinas <Windows o Linux>." |  No se proporcionaron las credenciales para acceder a las máquinas <Windows o Linux> en el dispositivo.| Agregue una credencial al dispositivo que tenga acceso a las máquinas <Windows o Linux>.
10005: "No se puede obtener acceso al servidor local". | Es posible que las credenciales de acceso sean erróneas. | Actualice las credenciales del dispositivo. Asegúrese de que puede acceder a la máquina pertinente con ellas. 
10006: "No se puede obtener acceso al servidor local". | Esto puede ocurrir si el sistema operativo de la máquina no es Windows o Linux.|  Use solo detección de aplicaciones para Windows o Linux.
10007: "Unable to process the metadata retrieved" (No se pueden procesar los metadatos recuperados) | Este error interno se produjo al intentar deserializar JSON | Para resolverlo, póngase en contacto con el servicio de Soporte técnico de Microsoft
9000/9001/9002: "No se pueden detectar las aplicaciones instaladas en el servidor". | Es posible que las herramientas de VMware no estén instaladas o estén dañadas. | Instale o vuelva a instalar las herramientas de VMware en el equipo correspondiente y compruebe que se está ejecutando.
9003: "No se pueden detectar las aplicaciones instaladas en el servidor". | Esto puede ocurrir si el sistema operativo de la máquina no es Windows o Linux. | Use solo detección de aplicaciones para Windows o Linux.
9004: "No se pueden detectar las aplicaciones instaladas en el servidor". | Esto puede ocurrir si la máquina virtual está apagada. | Para la detección, asegúrese de que la máquina virtual está encendida.
9005: "No se pueden detectar las aplicaciones instaladas en la VM". | Esto puede ocurrir si el sistema operativo de la máquina no es Windows o Linux. | Use solo detección de aplicaciones para Windows o Linux.
9006/9007: "No se pueden recuperar las aplicaciones instaladas en el servidor". | Es posible que el agente de detección del dispositivo no funcione correctamente. | Si el problema no se resuelve en un plazo de 24 horas, póngase en contacto con el soporte técnico.
9008: "No se pueden recuperar las aplicaciones instaladas en el servidor". | Podría ser un error interno.  | Si el problema no se resuelve en un plazo de veinticuatro horas, póngase en contacto con el soporte técnico.
9009: "No se pueden recuperar las aplicaciones instaladas en el servidor". | Puede producirse si la configuración del Control de cuentas de usuario (UAC) de Windows en el servidor es restrictiva e impide la detección de las aplicaciones instaladas. | Busque la configuración "Control de cuentas de usuario" en el servidor y configure la opción de UAC del servidor en uno de los dos niveles inferiores.
9010: "No se pueden recuperar las aplicaciones instaladas en el servidor". | Podría ser un error interno.  | Si el problema no se resuelve en un plazo de veinticuatro horas, póngase en contacto con el soporte técnico.
9011: "File to download from guest is not found on the guest VM" (El archivo para descargar desde el invitado no se encuentra en la máquina virtual invitada) | El problema puede producirse debido a un error interno. | El problema debería resolverse automáticamente en 24 horas. Si el problema persiste, póngase en contacto con el soporte técnico de Microsoft.
9012: "Result file contents are empty." (El contenido del archivo de resultados está vacío) | El problema puede producirse debido a un error interno. | El problema debería resolverse automáticamente en 24 horas. Si el problema persiste, póngase en contacto con el soporte técnico de Microsoft.
9013: "A new temporary profile is created for every login to the VMware VM" (Se crea un nuevo perfil temporal para cada inicio de sesión en la máquina virtual de VMware) | Se crea un nuevo perfil temporal para cada inicio de sesión en la máquina virtual | Asegúrese de que el nombre de usuario proporcionado en las credenciales de la máquina virtual invitada esté en formato UPN.
9015: "Unable to connect to VMware VMs due to insufficient privileges on vCenter" (No se puede conectar a máquinas virtuales de VMware debido a la falta de privilegios suficientes en vCenter) | El rol Operaciones de invitado no está habilitado en la cuenta de usuario de vCenter | Asegúrese de que el rol Operaciones de invitado está habilitado en la cuenta de usuario de vCenter.
9016: "Unable to connect to VMware VMs as the guest operations agent is out of data" (No se puede conectar a máquinas virtuales de VMware porque el agente de operaciones de invitado no tiene datos) | Las herramientas de VMware no están instalado correctamente o no están actualizadas. | Asegúrese de que las herramientas de VMware están correctamente instaladas y actualizadas.
9017: "File with discovered metadata is not found on the VM." (No se encuentra el archivo con los metadatos detectados en la máquina virtual). | El problema puede producirse debido a un error interno. | Para resolverlo, póngase en contacto con el servicio de Soporte técnico de Microsoft.
9018: "PowerShell is not installed in the Guest VMs." (PowerShell no está instalado en las máquinas virtuales invitadas) | PowerShell aún no está disponible en la máquina virtual invitada. | Instale PowerShell en la máquina virtual invitada.
9019: "Unable to discover due to guest VM operation failures" (No se puede realizar la detección debido a errores de operación de la máquina virtual invitada) | No se pudo realizar la operación de invitado de VMware en la máquina virtual. | Asegúrese de que las credenciales de la máquina virtual sean válidas y que el nombre de usuario proporcionado en las credenciales de la máquina virtual invitada esté en formato UPN.
9020: "File creation permission is denied." (Se denegó el permiso de creación de archivo) | El rol asociado al usuario o a la directiva de grupo está restringiendo al usuario a la hora de crear el archivo en la carpeta | Compruebe si el usuario invitado proporcionado tiene permiso de creación para el archivo en la carpeta. Consulte las **notificaciones** en Server Assessment para ver el nombre de la carpeta.
9021: "File create permission is denied in folder System Temp Path." (Se denegó el permiso de creación de archivo en la ruta de acceso temporal del sistema de carpeta.) | No se admite la versión de las herramientas de VMware en la máquina virtual | Actualice la versión de las herramientas de VMware a una versión superior a 10.2.0.
9022: "Get WMI object access is denied." (Acceso denegado al objeto Get WMI) | El rol asociado al usuario o a la directiva de grupo está restringiendo el acceso del usuario al objeto WMI. | Póngase en contacto con el soporte técnico de Microsoft.
9023: "SystemRoot environment variable value is empty." (El valor de la variable de entorno SystemRoot está vacío) | Desconocido | Póngase en contacto con el soporte técnico de Microsoft.
9024: "TEMP environment variable value is empty." (El valor de la variable de entorno TEMP está vacío) | Desconocido | Póngase en contacto con el soporte técnico de Microsoft.
9025: "PowerShell is corrupt in the Guest VMs." (PowerShell está dañado en las máquinas virtuales invitadas) | Desconocido | Vuelva a instalar PowerShell en la máquina virtual invitada y compruebe si PowerShell puede ejecutarse en ella.
8084: "No se pueden detectar las aplicaciones debido a un error de VMware: <Exception from VMware>" | El dispositivo de Azure Migrate usa las API de VMware para detectar aplicaciones. Este problema puede producirse si vCenter Server inicia una excepción al intentar detectar las aplicaciones. El mensaje de error de VMware se muestra en el mensaje de error que aparece en el portal. | Busque el mensaje en la [documentación de VMware](https://pubs.vmware.com/vsphere-51/topic/com.vmware.wssdk.apiref.doc/index-faults.html) y siga los pasos para solucionarlo. Si no lo puede solucionar, póngase en contacto con el soporte técnico de Microsoft.



## <a name="next-steps"></a>Pasos siguientes
Configuración de un dispositivo para [VMware](how-to-set-up-appliance-vmware.md), [Hyper-V](how-to-set-up-appliance-hyper-v.md), o [servidores físicos](how-to-set-up-appliance-physical.md).
