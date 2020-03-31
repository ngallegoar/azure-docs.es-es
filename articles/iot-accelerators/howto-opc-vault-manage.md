---
title: 'Administración del servicio de certificados de OPC Vault: Azure | Microsoft Docs'
description: Administre los certificados de entidad de certificación raíz y los permisos de usuario de OPC Vault.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 890a25ed2cf11d657cad930815d78dbf968cc9f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "71203657"
---
# <a name="manage-the-opc-vault-certificate-service"></a>Administración del servicio de certificados de OPC Vault

En este artículo se explican las tareas administrativas para el servicio de administración de certificados de OPC Vault en Azure. Incluye información sobre cómo renovar certificados de CA del emisor, cómo renovar la lista de revocación de certificados (CRL) y cómo conceder y revocar acceso a los usuarios.

## <a name="create-or-renew-the-root-ca-certificate"></a>Creación o renovación del certificado de entidad de certificación raíz

Después de implementar OPC Vault, debe crear el certificado de CA de raíz. Sin un certificado de CA del emisor válido, no se puede firmar ni emitir ningún certificado de aplicación. Consulte [Certificados](howto-opc-vault-secure-ca.md#certificates) para administrar los certificados con una vigencia segura y razonable. Renueve un certificado de CA del emisor una vez transcurrida la mitad de su vigencia. Al renovar, tenga en cuenta también que la vigencia configurada de un certificado de aplicación firmado recientemente no debe superar la vigencia del certificado de CA del emisor.
> [!IMPORTANT]
> Se requiere el rol "Administrador" para crear o renovar el certificado de CA del emisor.

1. Abra el servicio de certificados en `https://myResourceGroup-app.azurewebsites.net` e inicie sesión.
2. Vaya a **Grupos de certificados**.
3. Se muestra un grupo de certificados predeterminado. Seleccione **Editar**.
4. En **Edit Certificate Group Details** (Editar los detalles del grupo de certificados), puede modificar el nombre del firmante y la vigencia de los certificados de CA y de la aplicación. El firmante y la duración solo se deben establecer una vez antes de que se emita el primer certificado de entidad de certificación. Los cambios de vigencia durante las operaciones pueden generar vigencias incoherentes entre los certificados emitidos y las CRL.
5. Escriba un firmante válido, (por ejemplo, `CN=My CA Root, O=MyCompany, OU=MyDepartment`).<br>
   > [!IMPORTANT]
   > Si cambia de firmante, debe renovar el certificado de emisor o el servicio no podrá firmar los certificados de la aplicación. El firmante de la configuración se comprueba en relación con el firmante del certificado del emisor activo. Si los firmantes no coinciden, se rechaza la firma del certificado.
6. Seleccione **Guardar**.
7. Si en este momento se encuentra con un error de tipo "prohibido", es porque las credenciales de usuario no tienen permiso de administrador para modificar o crear un nuevo certificado raíz. De forma predeterminada, el usuario que implementó el servicio tiene roles de administrador y de firma con el servicio. Es necesario agregar otros usuarios a los roles de Aprobador, Escritor o Administrador, según corresponda en el registro de aplicación de Azure Active Directory (Azure AD).
8. Seleccione **Detalles**. Se debería mostrar la información actualizada.
9. Seleccione **Renovar certificado de CA** para emitir el primer certificado de CA del emisor o para renovar el certificado del emisor. Después, seleccione **Aceptar**.
10. Después de unos segundos, verá los **Detalles del certificado**. Para descargar el certificado de CA y la CRL más recientes para distribuirlos a las aplicaciones OPC UA, seleccione **Emisor** o **CRL**.

Ahora, el servicio de administración de certificados de OPC UA está listo para emitir certificados para las aplicaciones OPC UA.

## <a name="renew-the-crl"></a>Renovación de la CRL

La renovación de la CRL es una actualización que se debe distribuir a las aplicaciones a intervalos regulares. Los dispositivos OPC UA, que admiten la extensión X509 del punto de distribución de CRL, pueden actualizar directamente la CRL desde el punto de conexión del microservicio. Otros dispositivos de OPC UA pueden requerir actualizaciones manuales o se pueden actualizar mediante las extensiones de inserción de servidor GDS (*) para actualizar las listas de confianza con los certificados y las CRL.

En el flujo de trabajo siguiente, todas las solicitudes de certificado con el estados eliminado se revocan en las CRL, que se corresponden con el certificado de CA del emisor para el que se emitieron. El número de versión de la CRL se incrementa en 1. <br>
> [!NOTE]
> Todas las CRL emitidas son válidas hasta que expira el certificado de CA del emisor. Esto se debe a que la especificación OPC UA no requiere un modelo de distribución obligatorio y determinista para la CRL.

> [!IMPORTANT]
> El rol Administrador es necesario para renovar la CRL del emisor.

1. Abra el servicio de certificados en `https://myResourceGroup.azurewebsites.net` e inicie sesión.
2. Vaya a la página **Grupos de certificados**.
3. Seleccione **Detalles**. Al hacerlo, se debería mostrar la información actual del certificado y la CRL.
4. Seleccione **Update CRL Revocation List (CRL)** (Actualizar lista de revocación de CRL [CRL]) para emitir una CRL actualizada para todos los certificados del emisor activos en el almacenamiento de OPC Vault.
5. Después de unos segundos, verá los **Detalles del certificado**. Para descargar el certificado de CA y la CRL más recientes para distribuirlos a las aplicaciones OPC UA, seleccione **Emisor** o **CRL**.

## <a name="manage-user-roles"></a>Administrar los roles de usuario

Puede administrar los roles de usuario para el microservicio OPC Vault en la aplicación empresarial de Azure AD. Para obtener una descripción detallada de las definiciones de roles, consulte [Roles](howto-opc-vault-secure-ca.md#roles).

De manera predeterminada, un usuario autenticado en el inquilino puede iniciar sesión en el servicio como un Lector. Los roles con privilegios elevados requieren la administración manual en Azure Portal o mediante PowerShell.

### <a name="add-user"></a>Agregar usuario

1. Abra Azure Portal.
2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales**.
3. Elija el registro del microservicio de OPC Vault (de manera predeterminada, el valor de `resourceGroupName-service`).
4. Vaya a **Usuarios y grupos**.
5. Seleccione **Agregar usuario**.
6. Seleccione o invite al usuario para la asignación a un rol específico.
7. Seleccione el rol de los usuarios.
8. Seleccione **Asignar**.
9. Para los usuarios con el rol Administrador o Aprobador, continúe para agregar directivas de acceso de Azure Key Vault.

### <a name="remove-user"></a>Quitar usuario

1. Abra Azure Portal.
2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales**.
3. Elija el registro del microservicio de OPC Vault (de manera predeterminada, el valor de `resourceGroupName-service`).
4. Vaya a **Usuarios y grupos**.
5. Seleccione un usuario con un rol para quitarlo y, a continuación, seleccione **Quitar**.
6. Quite de las directivas de Azure Key Vault a los usuarios eliminados del rol Administrador o Aprobador.

### <a name="add-user-access-policy-to-azure-key-vault"></a>Agregar una directiva de acceso de usuario a Azure Key Vault

Se requieren directivas de acceso adicionales para los Aprobadores y los Administradores.

De manera predeterminada, la identidad del servicio solo tiene permisos limitados para acceder a Key Vault y evitar que se realicen operaciones o cambios con privilegios elevados sin suplantación del usuario. Los permisos de servicios básicos son Get y List, tanto para secretos como para certificados. En el caso de los secretos, solo hay una excepción: el servicio puede eliminar una clave privada del almacén secreto después de que la acepte un usuario. Todas las demás operaciones requieren permisos suplantados del usuario.

#### <a name="for-an-approver-role-the-following-permissions-must-be-added-to-key-vault"></a>Para un rol Aprobador, se deben agregar los permisos siguientes a Key Vault:

1. Abra Azure Portal.
2. Vaya al grupo `resourceGroupName` de OPC Vault usado durante la implementación.
3. Vaya a la instancia de Key Vault `resourceGroupName-xxxxx`.
4. Vaya a **Directivas de acceso**.
5. Seleccione **Agregar nuevo**.
6. Omita la plantilla. No hay ninguna plantilla que cumpla los requisitos.
7. Elija **Seleccionar la entidad de seguridad** y seleccione el usuario que se va a agregar o invite a un usuario nuevo al inquilino.
8. Seleccione los siguientes **Permisos clave**: **Obtener**, **Mostrar** y **Firmar**.
9. Seleccione los siguientes **Permisos secretos**: **Obtener**, **Mostrar**, **Establecer** y **Eliminar**.
10. Seleccione los siguientes **Permisos de certificado**: **Obtener** y **Mostrar**.
11. Seleccione **Aceptar** y, después, **Guardar**.

#### <a name="for-an-administrator-role-the-following-permissions-must-be-added-to-key-vault"></a>Para un rol de Administrador, se deben agregar los permisos siguientes a Key Vault:

1. Abra Azure Portal.
2. Vaya al grupo `resourceGroupName` de OPC Vault usado durante la implementación.
3. Vaya a la instancia de Key Vault `resourceGroupName-xxxxx`.
4. Vaya a **Directivas de acceso**.
5. Seleccione **Agregar nuevo**.
6. Omita la plantilla. No hay ninguna plantilla que cumpla los requisitos.
7. Elija **Seleccionar la entidad de seguridad** y seleccione el usuario que se va a agregar o invite a un usuario nuevo al inquilino.
8. Seleccione los siguientes **Permisos clave**: **Obtener**, **Mostrar** y **Firmar**.
9. Seleccione los siguientes **Permisos secretos**: **Obtener**, **Mostrar**, **Establecer** y **Eliminar**.
10. Seleccione los siguientes **Permisos de certificado**: **Obtener**, **Mostrar**, **Actualizar**, **Crear** e **Importar**.
11. Seleccione **Aceptar** y, después, **Guardar**.

### <a name="remove-user-access-policy-from-azure-key-vault"></a>Quitar la directiva de acceso de usuario de Azure Key Vault

1. Abra Azure Portal.
2. Vaya al grupo `resourceGroupName` de OPC Vault usado durante la implementación.
3. Vaya a la instancia de Key Vault `resourceGroupName-xxxxx`.
4. Vaya a **Directivas de acceso**.
5. Busque el usuario que quiera quitar y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a administrar los usuarios y certificados de OPC Vault, puede:

> [!div class="nextstepaction"]
> [Comunicación segura de los dispositivos OPC](howto-opc-vault-secure.md)
