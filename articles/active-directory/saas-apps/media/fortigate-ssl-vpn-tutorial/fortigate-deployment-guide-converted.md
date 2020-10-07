---
title: Guía de implementación de FortiGate | Microsoft Docs
description: Configure y trabaje con el producto de firewall de nueva generación FortiGate de Fortinet.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 357eb0a60e6246996de9ab75337ecc213d845ae7
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91273337"
---
# <a name="fortigate-deployment-guide"></a>Guía de implementación de FortiGate

Con esta guía de implementación, obtendrá información sobre cómo configurar y trabajar con el producto de firewall de nueva generación FortiGate de Fortinet.

## <a name="redeem-the-fortigate-license"></a>Canje de la licencia de FortiGate

El producto de firewall de nueva generación FortiGate de Fortinet está disponible como una máquina virtual en infraestructura como servicio (IaaS) de Azure. Existen dos modelos de licencia para esta máquina virtual: pago por uso y traiga su propia licencia.

Fortinet podría proporcionar licencias a los miembros del equipo de "acceso híbrido seguro (SHA) de puesta en producción" de Azure Active Directory (Azure AD). En los casos en los que no se proporcione ninguna licencia, la implementación del modo pago por uso también funcionará.

Si se ha emitido una licencia, Fortinet proporciona un código de registro que se debe canjear en línea.

![Captura de pantalla del código de registro de FortiGate SSL VPN.](registration-code.png)

1. Regístrese en https://support.fortinet.com/.
2. Después del registro, inicie sesión en https://support.fortinet.com/.
3. Vaya a **Asset** (Recurso) > **Register/Activate** (Registrar/activar).
4. Escriba el código de registro proporcionado por Fortinet.
5. Especifique el código de registro, seleccione **The product will be used by a non-government user** (El producto lo utilizará un usuario no gubernamental) y haga clic en **Next** (Siguiente).
6. Escriba una descripción del producto (por ejemplo, FortiGate), establezca el asociado de Fortinet como **Other** (Otro) > **Microsoft** y haga clic en **Next** (Siguiente).
7. Acepte el **contrato de registro del producto Fortinet** y seleccione **Next** (Siguiente).
8. Acepte los **términos** y seleccione **Confirm** (Confirmar).
9. Seleccione **License File Download** (Descargar archivo de licencia) y guarde la licencia para más adelante.


## <a name="download-firmware"></a>Descarga del firmware

La VM de Azure de FortiGate de Fortinet no se incluye con la versión de firmware necesaria para la autenticación de SAML. La versión más reciente debe obtenerse de Fortinet.

1. Inicie sesión en https://support.fortinet.com/.
2. Vaya a **Download** (Descargar) > **Firmware Images** (Imágenes de firmware).
3. A la derecha de **Release Notes** (Notas de la versión), seleccione **Download** (Descargar).
4. Seleccione **v6**. > **6.** > **6.4.** .
5. Para descargar **FGT_VM64_AZURE-v6-build1637-FORTINET.out**, seleccione el vínculo **HTTPS** en la misma fila.
6. Guarde el archivo para más adelante.


## <a name="deploy-the-fortigate-vm"></a>Implementación de la máquina virtual de FortiGate

1. Vaya a https://portal.azure.com e inicie sesión en la suscripción en la que desea implementar la máquina virtual de FortiGate.
2. Cree un nuevo grupo de recursos o abra el grupo de recursos en el que desea implementar la máquina virtual de FortiGate.
3. Seleccione **Agregar**.
4. En **Buscar en Marketplace**, escriba *Forti*. Seleccione **Fortinet FortiGate Next-Generation Firewall** (Firewall de nueva generación FortiGate de Fortinet).
5. Seleccione el plan de software (traiga su propia licencia si tiene una licencia o pago por uso de lo contrario). Seleccione **Crear**.
6. Rellene la configuración de la VM.

    ![Captura de pantalla de creación de una máquina virtual.](virtual-machine.png)

7. Establezca **Tipo de autenticación** en **Contraseña** y proporcione credenciales administrativas para la VM.
8. Seleccione **Revisar + crear** > **Crear**.
9. Espere a que la implementación de la VM se complete.


### <a name="set-a-static-public-ip-address-and-assign-a-fully-qualified-domain-name"></a>Establecimiento de una dirección IP pública estática y asignación de un nombre de dominio completo

Para una experiencia de usuario coherente, establezca la dirección IP pública asignada a la VM de FortiGate para que se asigne de forma estática. Además, asígnela a un nombre de dominio completo (FQDN).

1. Vaya a https://portal.azure.com y abra la configuración de la VM de FortiGate.
2. En la pantalla **Información general**, seleccione la dirección IP pública.

    ![Captura de pantalla de FortiGate SSL VPN.](public-ip-address.png)

3. Seleccione **Estática** > **Guardar**.

Si posee un nombre de dominio enrutable públicamente para el entorno en el que se va a implementar la VM de FortiGate, cree un registro de host (A) para la VM. Este registro se asigna a la dirección IP pública anterior que está asignada estáticamente.

### <a name="create-a-new-inbound-network-security-group-rule-for-tcp-port"></a>Creación de una regla del grupo de seguridad de red de entrada para el puerto TCP

1. Vaya a https://portal.azure.com y abra la configuración de la VM de FortiGate.
2. En el menú de la izquierda, seleccione **Redes**. Se muestran la interfaz de red y las reglas del puerto de entrada.
3. Seleccione **Agregar regla de puerto de entrada**.
4. Cree una nueva regla de puerto de entrada para TCP 8443.

    ![Captura de pantalla de Agregar regla de seguridad de entrada.](port-rule.png)

5. Seleccione **Agregar**.


## <a name="create-a-custom-azure-app-for-fortigate"></a>Creación de una aplicación de Azure personalizada para FortiGate

1. Vaya a https://portal.azure.com y abra el panel Azure AD del inquilino que proporcionará la identidad para los inicios de sesión de FortiGate.
2. En el menú de la izquierda, seleccione **Aplicaciones empresariales**.
3. Seleccione **Nueva aplicación** > **Aplicación situada fuera de la galería**.
4. Escriba un nombre (por ejemplo, FortiGate) y seleccione **Agregar**.
5. En el menú de la izquierda, seleccione **Usuarios y grupos**.
6. Agregue los usuarios que podrán iniciar sesión y seleccione **Asignar**.
7. En el menú de la izquierda, seleccione **Inicio de sesión único**.
8. Seleccione **SAML**.
9. En la sección **Configuración básica de SAML**, seleccione el icono del lápiz para editar la configuración.
10. Configurar lo siguiente:
    - El **Identificador (identificador de entidad)** para que sea `https://<address>/remote/saml/metadata`.
    - La **Dirección URL de respuesta (URL del Servicio de consumidor de aserciones)** para que sea `https://<address>/remote/saml/login`.
    - La **URL de cierre de sesión** para que sea `https://<address>/remote/saml/logout`.

    `<address>` es el FQDN o la dirección IP pública que se asignó a la VM de FortiGate.

11. Guarde cada una de estas direcciones URL para su uso posterior: Id. de entidad, Dirección URL de respuesta y URL de cierre de sesión.
12. Seleccione **Guardar** y cierre **Configuración básica de SAML**.
13. En **3: Certificado de firma de SAML**, descargue el **Certificado (Base64)** y guárdelo para más adelante.
14. En **4: Set up (App Name)** (4: Configurar [Nombre de la aplicación]), copie la dirección URL de inicio de sesión de Azure, el identificador de Azure AD y la dirección URL de cierre de sesión de Azure, y guárdelos para más tarde.
15. En **2: Atributos y notificaciones de usuario**, haga clic en el icono del lápiz para editar la configuración.
16. Seleccione **Agregar nueva notificación** y establezca el nombre en **nombre de usuario**.
17. Establezca el atributo de origen en **user.userprincipalname**.
18. Seleccione **Guardar** > **Agregar una notificación de grupo** > **Todos los grupos**.
19. Seleccione **Personalizar nombre de la notificación del grupo** y establezca el nombre en **grupo**.
20. Seleccione **Guardar**.


## <a name="prepare-for-group-matching"></a>Preparación para la coincidencia de grupos

FortiGate permite diferentes experiencias del portal de usuarios después del inicio de sesión en función de la pertenencia a grupos. Por ejemplo, puede haber una experiencia para el grupo de marketing y otra para el grupo de finanzas. Procedimiento para crear grupos para los usuarios:

1. Vaya a https://portal.azure.com y abra el panel Azure AD del inquilino que proporcionará la identidad para los inicios de sesión de FortiGate.
2. Seleccione **Grupos** > **Nuevo grupo**.
3. Cree un grupo con los detalles siguientes:
    - Tipo de grupo = Seguridad
    - Nombre del grupo = `a meaningful name`
    - Descripción del grupo = `a meaningful description for the group`
    - Tipo de pertenencia = Asignado.
    - Miembros = `users for the user experience that will map to this group`
4. Repita los pasos 3 y 4 para las experiencias de usuario adicionales.
5. Una vez creados los grupos, seleccione cada grupo y registre el **Id. de objeto** de cada uno.
6. Guarde estos identificadores de objeto y los nombres de grupo para más adelante.


## <a name="configure-the-fortigate-vm"></a>Configuración de la máquina virtual de FortiGate

Las secciones siguientes le guiarán a través de la configuración de la VM de FortiGate.

### <a name="install-the-license"></a>Instalación de la licencia

1. Vaya a `https://<address>`. Aquí, `<address>` es el FQDN o la dirección IP pública asignada a la VM de FortiGate.

2. Continúe después de los errores de certificado.
3. Inicie sesión con las credenciales de administrador proporcionadas durante la implementación de la VM de FortiGate.
4. Si la implementación usa el modelo traiga su propia licencia, verá un aviso para cargar una licencia. Seleccione el archivo de licencia creado anteriormente y cárguelo. Seleccione **Aceptar** y reinicie la VM de FortiGate.

    ![Captura de pantalla de la licencia de VM de FortiGate.](license.png)

5. Después del reinicio, inicie sesión de nuevo con las credenciales de administrador para validar la licencia.

### <a name="update-firmware"></a>Actualizar el firmware

1. Vaya a `https://<address>`. Aquí, `<address>` es el FQDN o la dirección IP pública asignada a la VM de FortiGate.

2. Continúe después de los errores de certificado.
3. Inicie sesión con las credenciales de administrador proporcionadas durante la implementación de la VM de FortiGate.
4. En el menú de la izquierda, seleccione **System** (Sistema) > **Firmware**.
5. En **Firmware Management** (Administración de firmware), haga clic en **Browse** (Examinar) y seleccione el archivo de firmware descargado anteriormente.
6. Ignore la advertencia y haga clic en **Backup config and upgrade** (Realizar copia de seguridad de la configuración y actualizar).

    ![Captura de pantalla de administración de firmware.](backup-configure-upgrade.png)

7. Seleccione **Continuar**.
8. Cuando se le pida que guarde la configuración de FortiGate (como un archivo .conf), haga clic en **Save** (Guardar).
9. Espere a que se cargue y se aplique el firmware. Espere a que se reinicie la VM de FortiGate.
10. Una vez reiniciada, inicie sesión de nuevo con las credenciales de administrador.
11. Cuando se le pida que configure el panel, seleccione **Later** (Más adelante).
12. Cuando comience el vídeo del tutorial, seleccione **OK** (Aceptar).

### <a name="change-the-management-port-to-tcp"></a>Cambio del puerto de administración a TCP

1. Vaya a `https://<address>`. Aquí, `<address>` es el FQDN o la dirección IP pública asignada a la VM de FortiGate.

2. Continúe después de los errores de certificado.
3. Inicie sesión con las credenciales de administrador proporcionadas durante la implementación de la VM de FortiGate.
4. En el menú de la izquierda, seleccione **System** (Sistema).
5. En **Administration Settings** (Configuración de administración), cambie el puerto HTTPS a **8443** y seleccione **Apply** (Aplicar).
6. Una vez que se aplica el cambio, el explorador intenta volver a cargar la página de administración, pero se produce un error. A partir de ahora, la dirección de esta página es `https://<address>`.

    ![Captura de pantalla de carga del certificado remoto.](certificate.png)

### <a name="upload-the-azure-ad-saml-signing-certificate"></a>Carga del certificado de firma de SAML para Azure AD

1. Vaya a `https://<address>`. Aquí, `<address>` es el FQDN o la dirección IP pública asignada a la VM de FortiGate.

2. Continúe después de los errores de certificado.
3. Inicie sesión con las credenciales de administrador proporcionadas durante la implementación de la VM de FortiGate.
4. En el menú de la izquierda, seleccione **System** (Sistema) > **Certificates** (Certificados).
5. Seleccione **Importar** > **Certificado remoto**.
6. Busque el certificado descargado de la implementación de la aplicación personalizada FortiGate en el inquilino de Azure. Selecciónelo y elija **OK** (Aceptar).

### <a name="upload-and-configure-a-custom-ssl-certificate"></a>Carga y configuración de un certificado SSL personalizado

Es posible que quiera configurar la VM de FortiGate con su propio certificado SSL que admita el FQDN que está usando. Si tiene acceso a un certificado SSL empaquetado con la clave privada en formato PFX, se puede usar para este propósito.

1. Vaya a `https://<address>`. Aquí, `<address>` es el FQDN o la dirección IP pública asignada a la VM de FortiGate.

2. Continúe después de los errores de certificado.
3. Inicie sesión con las credenciales de administrador proporcionadas durante la implementación de la VM de FortiGate.
4. En el menú de la izquierda, seleccione **System** (Sistema) > **Certificates** (Certificados).
5. Seleccione **Import** (Importar) > **Local Certificate** (Certificado local) > **PKCS #12 Certificate** (Certificado PKCS núm. 12).
6. Vaya al archivo .PFX que contiene el certificado SSL y la clave privada.
7. Proporcione la contraseña del archivo .PFX y un nombre descriptivo para el certificado. Después, seleccione **Aceptar**.
8. En el menú de la izquierda, seleccione **System** (Sistema) > **Settings** (Configuración).
9. En **Administration Settings** (Configuración de administración), expanda la lista desplegable situada junto al **certificado de servidor HTTPS** y seleccione el certificado SSL importado anteriormente.
10. Seleccione **Aplicar**.
11. Cierre la ventana del explorador y vaya a `https://<address>`.
12. Inicie sesión con las credenciales de administrador de FortiGate. Ahora debería ver el certificado SSL correcto en uso.


### <a name="perform-command-line-configuration"></a>Configuración de la línea de comandos

En las secciones siguientes se proporcionan los pasos de distintas configuraciones mediante la línea de comandos.

#### <a name="for-saml-authentication"></a>Para la autenticación SAML

1. Vaya a https://portal.azure.com y abra la configuración de la VM de FortiGate.
2. En el menú de la izquierda, seleccione **Serial Console** (Consola serie).
3. Inicie de sesión en la consola serie con las credenciales de administrador de la VM de FortiGate. En el paso siguiente, se necesitarán las direcciones URL registradas anteriormente:
    - El identificador de entidad
    - URL de respuesta
    - URL de cierre de sesión
    - Dirección URL de inicio de sesión de Azure
    - Identificador de Azure AD
    - Dirección URL de cierre de sesión de Azure
4. En la consola serie ejecute los siguientes comandos:

    ```
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert REMOTE_Cert_
    set user-name username
    set group-name group
    end
    ```
    > [!NOTE]
    > La dirección URL de cierre de sesión de Azure contiene un carácter `?`. Esto requiere una secuencia de clave especial para que se proporcione correctamente a la consola serie de FortiGate. La dirección URL suele ser `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1`. Para proporcionarla en la consola serie, escriba:
        ```
        set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation
        ```.
    A continuación, escriba CTRL+V y pegue el resto de la dirección URL para completar la línea: 
        ```
        set idp-single-logout-url
        https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.
        ```

5. Para confirmar la configuración, ejecute lo siguiente:

    ```
    show user saml
    ```

#### <a name="for-group-matching"></a>Para la coincidencia de grupos

1. Vaya a https://portal.azure.com y abra la configuración de la VM de FortiGate.
2. En el menú de la izquierda, seleccione **Serial Console** (Consola serie).
3. Inicie de sesión en la consola serie con las credenciales de administrador de la VM de FortiGate.
4. En la consola serie ejecute los siguientes comandos:

    ```
    config user group
    edit <group 1 name>
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <group 1 Object Id>
    next
    end
    next
    ```

    Para cada grupo adicional que tendrá una experiencia de portal diferente en FortiGate, repita estos comandos (a partir de la segunda línea del código).

#### <a name="for-authentication-timeout"></a>Para el tiempo de espera de autenticación

1. Vaya a https://portal.azure.com y abra la configuración de la VM de FortiGate.
2. En el menú de la izquierda, seleccione **Serial Console** (Consola serie).
3. Inicie de sesión en la consola serie con las credenciales de administrador de la VM de FortiGate.
4. En la consola serie ejecute los siguientes comandos:

    ```
    config system global
    set remoteauthtimeout 60
    end
    ```
### <a name="create-vpn-portals-and-firewall-policy"></a>Creación de portales VPN y de la directiva de firewall

1. Vaya a `https://<address>`. Aquí, `<address>` es el FQDN o la dirección IP pública asignada a la VM de FortiGate.

2. Inicie sesión con las credenciales de administrador proporcionadas durante la implementación de la VM de FortiGate.
3. En el menú de la izquierda, seleccione **VPN** > **SSL-VPN Portals** (Portales SSL-VPN) > **Create New** (Crear nuevo).
6. Proporcione un nombre (que normalmente coincide con el grupo de Azure que se usa para proporcionar la experiencia de portal personalizada).
7. Haga clic en el signo más ( **+** ) situado junto a **Source IP Pools** (Grupos de direcciones IP de origen), seleccione el grupo predeterminado y haga clic en **Close** (Cerrar).
8. Personalice la experiencia para este grupo. Para las pruebas, esto puede consistir en la personalización del mensaje y el tema del portal. Aquí también puede crear marcadores personalizados que dirijan a los usuarios a recursos internos.
9. Seleccione **Aceptar**.
10. Repita los pasos 5-9 para cada grupo de Azure que tendrá una experiencia de portal personalizada.
11. En VPN, seleccione **SSL-VPN Settings** (Configuración de SSL-VPN).
12. Seleccione el signo más ( **+** ) junto a **Listen on Interfaces** (Escuchar en interfaces), seleccione **Port1** (Puerto 1) y, a continuación, seleccione **Close** (Cerrar).
14. Si se instaló previamente un certificado SSL personalizado, cambie el **certificado de servidor** para que utilice el certificado SSL personalizado en el menú desplegable.
15. En **Authentication/Portal Mapping** (Autenticación/Asignación del portal), seleccione **Create New** (Crear nuevo). Elija el primer grupo de Azure y hágalo coincidir con el portal del mismo nombre. Después, seleccione **Aceptar**.
18. Repita los pasos 15-17 para cada emparejamiento de un grupo de Azure y un portal.
19. En **Authentication/Portal Mapping** (Autenticación/Asignación del portal), edite **All Other Users/Groups** (Todos los demás usuarios/Grupos).
20. Establezca el portal en **full-access** (acceso completo) y seleccione **OK** (Aceptar) > **Apply** (Aplicar).
23. Desplácese hasta la parte superior de la página **SSL-VPN Setting** (Configuración de SSL-VPN) y seleccione **No SSL-VPN policies exist (No existen directivas de SSL-VPN). Haga clic aquí para crear una nueva directiva SSL-VPN con esta configuración.**
24. Proporcione un nombre, como **VPN Grp**. A continuación, establezca **Outgoing Interface** (Interfaz de salida) en **port** (puerto) y seleccione **Source** (Origen).
27. En **Address** (Dirección), seleccione **all** (todas).
28. En **User** (Usuario), seleccione el primer grupo de Azure.
29. Seleccione **Close** (Cerrar) > **Destination** (Destino). En **Address** (Dirección), esta suele ser la red interna. Seleccione **login.microsoft.com** para realizar pruebas.
32. Seleccione **Close** (Cerrar) > **Service** (Servicio) > **All** (Todos). A continuación, seleccione **Close** (Cerrar) > **OK** (Aceptar).
37. En el menú de la izquierda, seleccione **Policy & Objects** (Directiva y objetos) > **Firewall Policy** (Directiva de Firewall).
39. Expanda **SSL-VPN tunnel interface (ssl.root)** [Interfaz de túnel de SSL-VPN (ssl.root)] > **port** (puerto).
40. Haga clic con el botón derecho en la directiva VPN creada anteriormente (**VPN Grp 1**) y seleccione **Copy** (Copiar).
41. Haga clic con el botón derecho en la directiva VPN y seleccione **Paste** (Pegar) > **Below** (Debajo).
42. Edite la nueva directiva y asígnele un nombre diferente (por ejemplo, **VPN Grp2**). Cambie también el grupo al que se aplica (a otro grupo de Azure).
43. Haga clic con el botón derecho en la nueva directiva y establezca el estado en **Enabled** (Habilitado).


## <a name="test-sign-in-by-using-azure"></a>Prueba del inicio de sesión con Azure

1. Mediante una sesión InPrivate del explorador, vaya a `https://<address>`.  
2. El inicio de sesión debería redirigir a Azure AD para el inicio de sesión.
3. Después de proporcionar las credenciales para un usuario que se ha asignado a la aplicación FortiGate en el inquilino de Azure, debería aparecer el portal de usuarios adecuado.

    ![Captura de pantalla de FortiGate SSL VPN](test-sign-in.png)
