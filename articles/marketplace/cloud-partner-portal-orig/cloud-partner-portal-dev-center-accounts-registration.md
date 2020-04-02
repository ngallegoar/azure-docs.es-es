---
title: Crear una cuenta de desarrollador de Microsoft | Azure Marketplace
description: Requisitos y pasos para crear una cuenta de desarrollador de Microsoft.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 161abde1ef0dfd86842fb56afe699fa632e0d9ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280378"
---
<a name="create-a-microsoft-developer-account"></a>Crear una cuenta de desarrollador de Microsoft
====================================

En este artículo se describe cómo convertirse en un desarrollador de Microsoft aprobado para poder publicar en Azure Marketplace.

## <a name="create-a-microsoft-account"></a>Creación de una cuenta Microsoft

Para iniciar el proceso de publicación, debe completar el registro en el **Centro para desarrolladores de Microsoft**. Para iniciar el proceso de publicación, usará la misma cuenta registrada en **[Cloud Partner Portal](https://cloudpartner.azure.com/)** .

### <a name="general-account-guidelines"></a>Directrices generales de la cuenta

Es recomendable tener solo una cuenta de Microsoft para las ofertas de Azure Marketplace. Esta cuenta no debería ser específica de servicios ni de ofertas.

La dirección que constituye el nombre de usuario debe estar en su dominio y estar controlada por su equipo de TI. Todas las actividades relacionadas con la publicación deben realizarse a través de esta cuenta.

>[!WARNING]
>No se admiten palabras como "Azure" y "Microsoft" en el registro de la cuenta de Microsoft. Evite el uso de estas palabras para completar el proceso de creación y registro de cuentas.

### <a name="company-account-guidelines"></a>Directrices de la cuenta de empresa

Siga estas instrucciones si más de una persona necesitará acceso a la cuenta mediante el inicio de sesión con la cuenta de Microsoft que se usó para abrir la cuenta.

>[!IMPORTANT]
>Para permitir que varios usuarios accedan a la cuenta del Centro para desarrolladores, se recomienda usar Azure Active Directory para asignar roles a usuarios individuales. Pueden acceder a la cuenta mediante inicio de sesión con sus credenciales de Azure AD individuales. Para más información, consulte [Manage account users](https://docs.microsoft.com/windows/uwp/publish/manage-account-users) (Administrar usuarios de una cuenta).

-   Para crear una cuenta de Microsoft, use una dirección de correo electrónico que pertenezca al dominio de la empresa, pero no a una única persona. Por ejemplo, windowsapps\@fabrikam.com.
-   Limite el acceso a esta cuenta de Microsoft a la menor cantidad de desarrolladores posible.
-   Configure una lista de distribución de correo electrónico corporativo que incluya a todos los usuarios que necesiten tener acceso a la cuenta de desarrollador y agregue esta dirección de correo electrónico a la información de seguridad. Esto permite que todos los empleados de la lista reciban códigos de seguridad cuando sea necesario y administren la información de seguridad de la cuenta de Microsoft. Si no es posible configurar una lista de distribución, el propietario de la cuenta de correo electrónico individual deberá permitir el acceso y el uso compartido del código de seguridad cuando se le solicite (por ejemplo, cuando se agregue información de seguridad nueva a la cuenta o cuando se deba obtener acceso a ella desde un dispositivo nuevo).
-   Agregue un número de teléfono de la empresa que no necesite extensión y al que puedan tener acceso los miembros clave del equipo.
-   En general, los desarrolladores deben usar dispositivos de confianza para iniciar sesión en la cuenta de desarrollador de la empresa. Todos los miembros clave del equipo deben tener acceso a estos dispositivos de confianza. Esto disminuirá la necesidad de enviar códigos de seguridad cuando se obtiene acceso a la cuenta.
-   Si desea permitir el acceso a la cuenta desde un equipo que no es de confianza, limite el acceso a un máximo de cinco desarrolladores. Idealmente, estos desarrolladores deben obtener acceso a la cuenta desde máquinas que compartan la misma ubicación geográfica y de la red.
-   Revise con frecuencia la [información de seguridad de su empresa](https://account.live.com/proofs/Manage) para asegurarse de que está actualizada.

>[!IMPORTANT]
>Se debe tener acceso a la cuenta de desarrollador principalmente desde equipos de confianza. Esto resulta fundamental porque la cantidad de códigos generados por cuenta a la semana es limitada. También permite lograr la experiencia de inicio de sesión más óptima.
>
>Para obtener más información, consulte [Directrices adicionales para cuentas de empresa](https://msdn.microsoft.com/windows/uwp/publish/opening-a-developer-account#additional-guidelines-for-company-accounts).

### <a name="to-create-a-microsoft-account"></a>Creación de una cuenta de Microsoft

1.  Abra una nueva sesión de exploración de incógnito en Chrome o de InPrivate en Internet Explorer para asegurarse de que no tiene una sesión iniciada en una cuenta existente.
2.  Registre el correo electrónico (de acuerdo con las instrucciones anteriores) como una cuenta de Microsoft mediante este [vínculo](https://signup.live.com/signup.aspx). Complete las siguientes instrucciones de registro:

    - Cuando registre la cuenta como cuenta de Microsoft, debe proporcionar un número de teléfono válido para que el sistema pueda enviarle un código de verificación de la cuenta a través de un mensaje de texto o una llamada automatizada.
    - Al registrar una cuenta como cuenta de Microsoft, debe proporcionar un identificador de correo electrónico válido para recibir un correo electrónico automatizado de verificación de la cuenta.
    - Compruebe la dirección de correo electrónico enviada a la lista de distribución.

    Ahora está listo para usar la nueva cuenta de Microsoft en el Centro para desarrolladores de Microsoft.

## <a name="register-your-account-in-microsoft-developer-center"></a>Registre la cuenta en el Centro para desarrolladores de Microsoft

El Centro para desarrolladores de Microsoft se usa para registrar la información de la empresa una sola vez. El usuario inscrito debe ser un representante válido de la compañía y debe proporcionar su información personal, como una forma de validar su identidad. La persona que se inscriba debe usar una cuenta Microsoft que se comparta en la empresa, **y esa misma cuenta debe usarse en Cloud Partner Portal.** Debe asegurarse de que su compañía no tenga ya una cuenta del Centro para desarrolladores de Microsoft antes de intentar crear una nueva. Durante el proceso, recopilaremos la información de dirección, los datos de la cuenta bancaria y la información fiscal de la compañía. Normalmente, estos datos se pueden obtener de contactos financieros o comerciales.

>[!IMPORTANT]
>Debe completar los siguientes componentes del perfil de desarrollador para avanzar por las distintas fases de la creación e implementación de ofertas.

| Perfil del desarrollador     | Para iniciar el borrador    | Ensayo       | Publicación gratis y plantilla de solución   | Publicación de ofertas comerciales   |
|---------------------- |----------------   |-----------    |-------------------------------------  |---------------------  |
| Registro de la compañía  | Obligatorio         | Obligatorio     | Obligatorio                             | Obligatorio             |
| Identificación de perfil fiscal        | Opcional          | Opcional      | Opcional                              | Obligatorio             |
| Cuenta bancaria          | Opcional          | Opcional      | Opcional                              | Obligatorio             |

>[!NOTE]
>El sistema "traiga su propia licencia" (BYOL) solo se admite para máquinas virtuales y se considera una oferta gratuita.

### <a name="register-your-company-account"></a>Registro de su cuenta de compañía

1. Abra una nueva sesión de exploración de InPrivate en Internet Explorer o de incógnito en Chrome para asegurarse de que no tiene una sesión iniciada en una cuenta personal.

2. Vaya al [Centro para desarrolladores de Windows](https://dev.windows.com/registration?accountprogram=azure) para registrarse como vendedor. Lea la siguiente nota importante antes de continuar.

   ![Verificación de la cuenta de Microsoft](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-verify.jpg)

    >[!IMPORTANT]
    >Asegúrese de que el identificador de correo electrónico o la DL (se recomienda usar una lista de distribución para no tener que depender de otras personas) que va a utilizar para registrarse en el Centro de desarrollo es una cuenta Microsoft. En caso contrario, siga este vínculo para registrarla. Además, no se puede utilizar ningún identificador de correo electrónico con el dominio de empresa de Microsoft para registrarse en el Centro de desarrollo.

   ![Inicio de sesión del Centro de desarrollo](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-login.jpg)

3. Ejecute el asistente "Ayúdenos a proteger su cuenta" para verificar su identidad mediante un número de teléfono o correo electrónico.

4. En Registro-Información de cuenta, seleccione **País o región de la cuenta** en la lista desplegable y, a continuación, **Siguiente**.

   ![Seleccionar país o región](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_04.png)

    >[!WARNING]
    >Países o regiones de "origen de venta": Para vender sus servicios en Azure Marketplace, la entidad registrada debe proceder de uno de los países o regiones de "origen de venta" aprobados que se muestran en la lista desplegable. Esta restricción se debe a motivos de pago e impuestos. Para más información, consulte las directivas de participación de Marketplace.

5. Seleccione **Empresa** como "Tipo de cuenta" y, a continuación, **Siguiente**.

    >[!IMPORTANT]
    >Para comprender mejor los tipos de cuenta y decidir cuál es el mejor, consulte los tipos de cuenta, las ubicaciones y las tarifas de la siguiente captura de pantalla.

    ![Tipos de cuenta para vendedores](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_05.png)

6. Especifique el **Nombre para mostrar del publicador**. Suele ser el nombre de la empresa.

    >[!NOTE]
    >El nombre para mostrar del publicador especificado en el Centro para desarrolladores no se muestra en Azure Marketplace al publicar la oferta. Pero esta información es necesaria para finalizar el proceso de registro.

7. Escriba la **información de contacto** para que se compruebe la cuenta.

    >[!IMPORTANT]
    >La información proporcionada debe ser precisa, ya que se usará en nuestro proceso de verificación para aprobar la empresa en el Centro para desarrolladores.

8. Escriba la información de contacto del **aprobador de la compañía**. El aprobador de la compañía es la persona que puede verificar que está autorizado para crear una cuenta en el Centro de desarrollo en nombre de su organización. Después de proporcionar esta información, seleccione **Siguiente** para pasar a la **sección Pago**.

    ![Identificar el aprobador de la compañía](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_08.png)

9. Especifique la información de pago para la cuenta. Si tiene un código de promoción que cubre el costo de registro, puede indicarlo aquí. De lo contrario, proporcione su información de tarjeta de crédito (o PayPal en mercados admitidos). Seleccione **Siguiente** para pasar a la **Revisión** final.

   ![Registro de pago](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_09.png)

10. Revise la información de la cuenta y confirme que todo es correcto. Lea y acepte los términos y condiciones del [Contrato para publicadores de Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/?LinkID=699560). Active la casilla para indicar que ha leído y aceptado estos términos.

11. Haga clic en **Finalizar** para confirmar el registro. Se envía un mensaje de confirmación a su dirección de correo electrónico.

12. Si tiene previsto publicar únicamente ofertas gratuitas, seleccione [Go to the Cloud Partner Portal](https://cloudpartner.azure.com/) (Ir a Cloud Partner Portal) y vaya directamente a la sección "Registro de su cuenta en Cloud Partner Portal" de este artículo.

### <a name="commercial-offers"></a>Ofertas comerciales

Si tiene previsto publicar ofertas comerciales, como una oferta de máquina virtual que use un modelo de facturación por horas, debe proporcionar datos fiscales y bancarios. Para ello, inicie sesión en su cuenta del Centro para desarrolladores y seleccione **Update your account information** (Actualizar la información de la cuenta). Siga las instrucciones de la sección siguiente: "Agregar datos fiscales y bancarios".

>[!IMPORTANT]
>No podrá enviar una oferta comercial a producción sin proporcionar sus datos fiscales y bancarios.

Si prefiere actualizar sus datos fiscales y bancarios en otro momento, vaya directamente a la sección "Registrar la cuenta en Cloud Partner Portal" de este artículo.

>[!NOTE]
>Le recomendamos que proporcione los datos fiscales y bancarios lo antes posible, ya que la validación de la información fiscal lleva cierto tiempo.

### <a name="add-banking-and-tax-information"></a>Agregar datos fiscales y bancarios

Para publicar ofertas comerciales de compra, necesitará agregar información de pago y fiscal, y enviarla para su validación en el Centro para desarrolladores.

**Para proporcionar sus datos bancarios**

1.  Inicie sesión en el [Cntro para desarrolladores de Microsoft](https://dev.windows.com/registration?accountprogram=azure) con su cuenta Microsoft.
2.  Seleccione **Cuenta de pago** en el menú de la izquierda y, en **Elegir método de pago**, seleccione **Cuenta bancaria** o **PayPal**.

    >[!NOTE]
    >Si tiene ofertas comerciales que compran los clientes en Marketplace, esta es la cuenta donde recibirá el pago para esas compras.
3.  Escriba la información de pago y seleccione **Guardar**.

    >[!IMPORTANT]
    >Si tiene que actualizar o cambiar su cuenta de pago, siga los pasos anteriores para reemplazar la información actual con la nueva.
    >
    >El hecho de cambiar la cuenta de pago puede dar lugar a retrasos en los pagos de hasta un ciclo completo. Este retraso se produce porque es necesario comprobar el cambio de la cuenta, al igual que hicimos cuando configuró por primera vez la cuenta de pago. Aunque se le seguirá pagando el importe completo después de que se haya comprobado la cuenta, los pagos pendientes del ciclo actual se agregarán al siguiente.

4.  Seleccione **Next** (Siguiente).

**Para proporcionar datos fiscales**

1.  Inicie sesión en el [Centro para desarrolladores de Microsoft](https://dev.windows.com/registration?accountprogram=azure) con su cuenta Microsoft (si es necesario).
2.  En el menú de la izquierda, seleccione **Perfil fiscal**.
3.  En la página **Configure su formulario fiscal**:
    - Seleccione el país o región donde tiene su residencia permanente.
    - Seleccione el país o región donde tiene la ciudadanía principal.
    - Seleccione **Next** (Siguiente).
4.  Escriba la información fiscal y, después, seleccione **Siguiente**.

>[!WARNING]
>No podrá insertar las ofertas comerciales en producción sin proporcionar sus datos bancarios y fiscales en la cuenta del Centro para desarrolladores de Microsoft.

### <a name="developer-center-registration-issues"></a>Problemas de registro del Centro para desarrolladores

Si tiene problemas para registrarse en el Centro para desarrolladores, siga estos pasos para abrir una incidencia de soporte técnico.

1.  Vaya al [vínculo de soporte técnico](https://developer.microsoft.com/windows/support).
2.  En **Contacto**, seleccione **Enviar un incidente**.
    ![Abrir una incidencia](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_02.png)
3.  En **Tipo de problema**, seleccione "Help with Dev Center" (Ayuda con Centro para desarrolladores) y, en **Categoría**, seleccione "Publicar y administrar aplicaciones". Seleccione **Start email** (Iniciar correo electrónico).

    ![identificar el tipo de problema](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_03.png)

4.  Se mostrará una página de inicio de sesión. Use cualquier cuenta de Microsoft para iniciar sesión. Si no tiene ninguna cuenta de Microsoft, [cree una](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).

5.  Proporcione información detallada sobre el problema y seleccione **Enviar** para enviar la incidencia.

    ![enviar una incidencia](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_05.png)

## <a name="register-your-account-in-the-cloud-partner-portal"></a>Registro de su cuenta en Cloud Partner Portal

[Cloud Partner Portal](https://cloudpartner.azure.com/) se usa para publicar y administrar ofertas.

1.  Abra una nueva sesión de exploración de incógnito en Chrome o de InPrivate en Internet Explorer para asegurarse de que no tiene una sesión iniciada en una cuenta personal.
2.  Vaya a [Cloud Partner Portal](https://cloudpartner.azure.com/).
3.  Si es un usuario nuevo y va a iniciar sesión por primera vez en [Cloud Partner Portal](https://cloudpartner.azure.com/), tendrá que hacerlo con el mismo identificador de correo electrónico con el que registró la cuenta del Centro de desarrollo. De esta manera, se vincularán entre sí la cuenta del Centro de desarrollo y la de Cloud Partner Portal.

Más adelante, puede agregar a los demás miembros de la empresa que trabajan en la aplicación. Para agregarlos como colaboradores o propietarios en Cloud Partner Portal, siga los pasos de la siguiente sección.

Si a usted se le agregó como colaborador o propietario en el portal Cloud Partner Portal, podrá iniciar sesión con su propia cuenta.

>[!TIP]
>Las directivas de participación se describen en el sitio web de Azure.

## <a name="manage-users-as-owners-or-contributors-in-the-cloud-partner-portal"></a>Administrar usuarios como propietarios o colaboradores en Cloud Partner Portal

[Pasos para administrar usuarios en Cloud Partner Portal](./cloud-partner-portal-manage-users.md)


## <a name="next-steps"></a>Pasos siguientes

Ahora que se creó la cuenta y se registró, puede iniciar el proceso de publicación de Azure Marketplace.
