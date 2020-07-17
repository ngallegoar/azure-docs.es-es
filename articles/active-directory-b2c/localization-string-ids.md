---
title: 'Identificadores de cadenas de localización: Azure Active Directory B2C | Microsoft Docs'
description: Especifique los identificadores de una definición de contenido con un identificador api.signuporsignin en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/23/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1cd18ae9fd3814765e77dad672909343d651913e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85355512"
---
# <a name="localization-string-ids"></a>Identificadores de cadenas de localización

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

El elemento **Localization** permite la compatibilidad con varios idiomas o configuraciones regionales en la directiva para los recorridos del usuario. En este artículo se proporciona la lista de identificadores de localización que puede usar en la directiva. Para familiarizarse con la localización de la interfaz de usuario, vea [Localización](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Elementos de la página de registro o de inicio de sesión

Los siguientes identificadores sirven para una definición de contenido con el identificador `api.signuporsignin`.

| ID | Valor predeterminado |
| -- | ------------- |
| **local_intro_email** | Inicie sesión con su cuenta existente. |
| **logonIdentifier_email** | Dirección de correo electrónico |
| **requiredField_email** | Escriba su dirección de correo electrónico. |
| **invalid_email** | Escriba una dirección de correo electrónico válida. |
| **email_pattern** | ^[a-zA-Z0-9.!#$%&'' *+/=?^_\`{\|}~-]+@[a-zA-Z0-9-]+(?:\\.[a-zA-Z0-9-]+)* $ |
| **local_intro_username** | Inicie sesión con su nombre de usuario. |
| **logonIdentifier_username** | Nombre de usuario |
| **requiredField_username** | Escriba su nombre de usuario. |
| **password** | Contraseña |
| **requiredField_password** | Escriba su contraseña. |
| **invalid_password** | La contraseña que ha escrito no tiene el formato esperado. |
| **forgotpassword_link** | ¿Ha olvidado la contraseña? |
| **createaccount_intro** | ¿No tiene una cuenta? |
| **createaccount_link** | Regístrese ahora |
| **divider_title** | O BIEN |
| **cancel_message** | El usuario ha olvidado su contraseña. |
| **button_signin** | Iniciar sesión |
| **social_intro** | Inicie sesión con su cuenta social. |
  **remember_me** |Mantenga la sesión iniciada.|
| **unknown_error** | Tenemos problemas para iniciar su sesión. Inténtelo de nuevo más tarde. |

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de registro o de inicio de sesión:

![Elementos de la experiencia de usuario de la página de registro o de inicio de sesión](./media/localization-string-ids/localization-susi.png)

El identificador de los proveedores de identidades se configura en el elemento **ClaimsExchange** del recorrido del usuario. Para localizar el título del proveedor de identidades, **ElementType** está establecido en `ClaimsProvider`, mientras que **StringId** se establece en el identificador de `ClaimsExchange`.

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="MicrosoftExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccount" />
  </ClaimsExchanges>
</OrchestrationStep>
```

En el ejemplo siguiente se localiza el proveedor de identidades de Facebook en árabe:

```xml
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

### <a name="sign-up-or-sign-in-error-messages"></a>Mensajes de error al iniciar sesión o registrarse

| ID | Valor predeterminado |
| -- | ------------- |
| **UserMessageIfInvalidPassword** | La contraseña es incorrecta. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Parece que no encontramos su cuenta. |
| **UserMessageIfOldPasswordUsed** | Parece que ha usado una contraseña antigua. |
| **DefaultMessage** | Nombre de usuario o contraseña no válidos. |
| **UserMessageIfUserAccountDisabled** | Su cuenta se ha bloqueado. Contacte con la persona responsable de soporte técnico para desbloquearla y vuelva a intentarlo. |
| **UserMessageIfUserAccountLocked** | Su cuenta se bloqueó temporalmente para impedir un uso no autorizado. Vuelva a intentarlo más tarde. |
| **AADRequestsThrottled** | Hay demasiadas solicitudes en este momento. Espere un poco y vuelva a intentarlo. |

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Elementos de la interfaz de usuario de páginas autoafirmadas y de registro

Los siguientes identificadores corresponden a una definición de contenido con un identificador `api.localaccountsignup` o a cualquier definición de contenido que empiece con `api.selfasserted`, como `api.selfasserted.profileupdate` y `api.localaccountpasswordreset`.

| ID | Valor predeterminado |
| -- | ------------- |
| **ver_sent** | Se ha enviado el código de verificación a: |
| **ver_but_default** | Valor predeterminado |
| **cancel_message** | El usuario ha cancelado la introducción de información afirmada por él mismo. |
| **preloader_alt** | Espere |
| **ver_but_send** | Envíe un código de comprobación. |
| **alert_yes** | Sí |
| **error_fieldIncorrect** | Un campo o varios se han rellenado incorrectamente. Compruebe las entradas y vuelva a intentarlo. |
| **year** | Year |
| **verifying_blurb** | Espere mientras se procesa la información. |
| **button_cancel** | Cancelar |
| **ver_fail_no_retry** | Ha hecho demasiados intentos incorrectos. Inténtelo de nuevo más tarde. |
| **month** | Month |
| **ver_success_msg** | Se ha verificado la dirección de correo electrónico. Ahora puede continuar. |
| **months** | Enero, febrero, marzo, abril, mayo, junio, julio, agosto, septiembre, octubre, noviembre, diciembre |
| **ver_fail_server** | Estamos teniendo problemas para comprobar su dirección de correo electrónico. Escriba una dirección de correo electrónico válida e inténtelo de nuevo. |
| **error_requiredFieldMissing** | Falta un campo obligatorio. Rellene todos los campos obligatorios y vuelva a intentarlo. |
| **initial_intro** | Proporcione los detalles que se indican a continuación. |
| **ver_but_resend** | Envíe un código nuevo. |
| **button_continue** | Crear |
| **error_passwordEntryMismatch** | Los campos de entrada de contraseña no coinciden. Escriba la misma contraseña en ambos campos y vuelva a intentarlo. |
| **ver_incorrect_format** | Formato incorrecto. |
| **ver_but_edit** | Cambie la dirección de correo electrónico. |
| **ver_but_verify** | Compruebe el código. |
| **alert_no** | No |
| **ver_info_msg** | Se ha enviado el código de verificación a su bandeja de entrada. Cópielo en el siguiente cuadro de entrada de texto. |
| **day** | Día |
| **ver_fail_throttled** | Ha habido demasiadas solicitudes para verificar esta dirección de correo electrónico. Espere un poco y vuelva a intentarlo. |
| **helplink_text** | ¿Qué es esto? |
| **ver_fail_retry** | Este código no es correcto. Inténtelo de nuevo. |
| **alert_title** | Cancele la entrada de información. |
| **required_field** | Esta información es obligatoria. |
| **alert_message** | ¿Confirma que quiere cancelar la introducción de información? |
| **ver_intro_msg** | La comprobación es necesaria. Haga clic en el botón Enviar. |
| **ver_input** | Código de verificación |

### <a name="sign-up-and-self-asserted-pages-error-messages"></a>Mensajes de error en las páginas autoafirmadas y de registro

| ID | Valor predeterminado |
| -- | ------------- |
| **UserMessageIfClaimsPrincipalAlreadyExists** | Ya existe un usuario con el identificador especificado. Elija otro. |
| **UserMessageIfClaimNotVerified** | Reclamación no comprobada: {0} |
| **UserMessageIfIncorrectPattern** | Patrón incorrecto para: {0} |
| **UserMessageIfMissingRequiredElement** | Falta un elemento obligatorio: {0} |
| **UserMessageIfValidationError** | Error en la validación de: {0} |
| **UserMessageIfInvalidInput** | {0} tiene una entrada no válida. |
| **ServiceThrottled** | Hay demasiadas solicitudes en este momento. Espere un poco y vuelva a intentarlo. |

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de registro:

![Página de registro con los nombres de los elementos de la interfaz de usuario etiquetados](./media/localization-string-ids/localization-sign-up.png)

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de registro después de que el usuario haga clic en el botón para enviar el código de verificación:

![Elementos de la experiencia de usuario al comprobar el correo electrónico en la página de registro](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Elementos de la interfaz de usuario en la página de autenticación por teléfono

Los siguientes identificadores sirven para una definición de contenido con el identificador `api.phonefactor`.

| ID | Valor predeterminado |
| -- | ------------- |
| **button_verify** | Llamarme |
| **country_code_label** | Código de país |
| **cancel_message** | El usuario ha cancelado la autenticación multifactor. |
| **text_button_send_second_code** | Envía un código nuevo |
| **code_pattern** | \\d{6} |
| **intro_mixed** | Entre sus datos nos consta el número siguiente. Podemos enviarle un código por SMS o por teléfono para autenticar su identidad. |
| **intro_mixed_p** | Entre sus datos nos constan los números siguientes. Elija un número al que podamos llamarle o enviarle un código por SMS para autenticar su identidad. |
| **button_verify_code** | Verifique el código |
| **requiredField_code** | Escriba el código de verificación que ha recibido. |
| **invalid_code** | Escriba el código de seis dígitos que ha recibido. |
| **button_cancel** | Cancelar |
| **local_number_input_placeholder_text** | Número de teléfono |
| **button_retry** | Reintento |
| **alternative_text** | No tengo mi teléfono |
| **intro_phone_p** | Entre sus datos nos constan los números siguientes. Elija un número al que podamos llamarle para autenticar su identidad. |
| **intro_phone** | Entre sus datos nos consta el número siguiente. Le llamaremos para autenticar su identidad. |
| **enter_code_text_intro** | Escriba su código de verificación a continuación.  |
| **intro_entry_phone** | Escriba un número a continuación al que podamos llamar para autenticar su identidad. |
| **intro_entry_sms** | Escriba un número a continuación al que podamos enviarle un código por SMS para autenticar su identidad. |
| **button_send_code** | Envié el código |
| **invalid_number** | Escriba un número de teléfono válido. |
| **intro_sms** | Entre sus datos nos consta el número siguiente. Le enviaremos un código por SMS para autenticar su identidad. |
| **intro_entry_mixed** | Escriba un número a continuación al que podamos llamar o enviar un código por SMS para autenticar su identidad. |
| **number_pattern** | ^\\+(?:[0-9][\\x20-]?){6,14}[0-9]$ |
| **intro_sms_p** |Entre sus datos nos constan los números siguientes. Elija un número al que podamos enviarle un código por SMS para autenticar su identidad. |
| **requiredField_countryCode** | Seleccione su código de país. |
| **requiredField_number** | Escriba su número de teléfono. |
| **country_code_input_placeholder_text** |País o región |
| **number_label** | Número de teléfono |
| **error_tryagain** | El número de teléfono que ha proporcionado está ocupado o no está disponible. Compruebe el número y vuelva a intentarlo. |
| **error_incorrect_code** | El código de verificación que ha escrito no figura en nuestros registros. Inténtelo de nuevo o solicite uno nuevo. |
| **countryList** | {\"DEFAULT\":\"País/Región\",\"AF\":\"Afganistán\",\"AX\":\"Islas Åland\",\"AL\":\"Albania\",\"DZ\":\"Argelia\",\"AS\":\"Samoa Americana\",\"AD\":\"Andorra\",\"AO\":\"Angola\",\"AI\":\"Anguila\",\"AQ\":\"Antártida\",\"AG\":\"Antigua y Barbuda\",\"AR\":\"Argentina\",\"AM\":\"Armenia\",\"AW\":\"Aruba\",\"AU\":\"Australia\",\"AT\":\"Austria\",\"AZ\":\"Azerbaiyán\",\"BS\":\"Bahamas\",\"BH\":\"Bahréin\",\"BD\":\"Bangladés\",\"BB\":\"Barbados\",\"BY\":\"Belarús\",\"BE\":\"Bélgica\",\"BZ\":\"Belice\",\"BJ\":\"Benín\",\"BM\":\"Bermudas\",\"BT\":\"Bután\",\"BO\":\"Bolivia\",\"BQ\":\"Bonaire\",\"BA\":\"Bosnia y Herzegovina\",\"BW\":\"Botsuana\",\"BV\":\"Isla Bouvet\",\"BR\":\"Brasil\",\"IO\":\"Territorio Británico del Océano Índico\",\"VG\":\"Islas Vírgenes Británicas\",\"BN\":\"Brunei\",\"BG\":\"Bulgaria\",\"BF\":\"Burkina Faso\",\"BI\":\"Burundi\",\"CV\":\"Cabo Verde\",\"KH\":\"Camboya\",\"CM\":\"Camerún\",\"CA\":\"Canadá\",\"KY\":\"Islas Caimán\",\"CF\":\"República Centroafricana\",\"TD\":\"Chad\",\"CL\":\"Chile\",\"CN\":\"China\",\"CX\":\"Isla de Navidad\",\"CC\":\"Islas Cocos (Keeling)\",\"CO\":\"Colombia\",\"KM\":\"Comores\",\"CG\":\"Congo\",\"CD\":\"Congo (RDC)\",\"CK\":\"Islas Cook\",\"CR\":\"Costa Rica\",\"CI\":\"Côte d'Ivoire\",\"HR\":\"Croacia\",\"CU\":\"Cuba\",\"CW\":\"Curazao\",\"CY\":\"Chipre\",\"CZ\":\"República Checa\",\"DK\":\"Dinamarca\",\"DJ\":\"Yibuti\",\"DM\":\"Dominica\",\"DO\":\"República Dominicana\",\"EC\":\"Ecuador\",\"EG\":\"Egipto\",\"SV\":\"El Salvador\",\"GQ\":\"Guinea Ecuatorial\",\"ER\":\"Eritrea\",\"EE\":\"Estonia\",\"ET\":\"Etiopía\",\"FK\":\"Islas Malvinas\",\"FO\":\"Islas Feroe\",\"FJ\":\"Islas Fiji\",\"FI\":\"Finlandia\",\"FR\":\"Francia\",\"GF\":\"Guayana Francesa\",\"PF\":\"Polinesia francesa\",\"TF\":\"Territorios Australes Franceses\",\"GA\":\"Gabón\",\"GM\":\"Gambia\",\"GE\":\"Georgia\",\"DE\":\"Alemania\",\"GH\":\"Ghana\",\"GI\":\"Gibraltar\",\"GR\":\"Grecia\",\"GL\":\"Groenlandia\",\"GD\":\"Granada\",\"GP\":\"Guadalupe\",\"GU\":\"Guam\",\"GT\":\"Guatemala\",\"GG\":\"Guernsey\",\"GN\":\"Guinea\",\"GW\":\"Guinea-Bisáu\",\"GY\":\"Guayana\",\"HT\":\"Haití\",\"HM\":\"Isla Heard e Islas McDonald\",\"HN\":\"Honduras\",\"HK\":\"RAE de Hong Kong\",\"HU\":\"Hungría\",\"IS\":\"Islandia\",\"IN\":\"India\",\"ID\":\"Indonesia\",\"IR\":\"Irán\",\"IQ\":\"Iraq\",\"IE\":\"Irlanda\",\"IM\":\"Isla de Man\",\"IL\":\"Israel\",\"IT\":\"Italia\",\"JM\":\"Jamaica\",\"JP\":\"Japón\",\"JE\":\"Jersey\",\"JO\":\"Jordania\",\"KZ\":\"Kazajistán\",\"KE\":\"Kenia\",\"KI\":\"Kiribati\",\"KR\":\"Corea\",\"KW\":\"Kuwait\",\"KG\":\"Kirguistán\",\"LA\":\"Laos\",\"LV\":\"Letonia\",\"LB\":\"Líbano\",\"LS\":\"Lesoto\",\"LR\":\"Liberia\",\"LY\":\"Libia\",\"LI\":\"Liechtenstein\",\"LT\":\"Lituania\",\"LU\":\"Luxemburgo\",\"MO\":\"RAE de Macao\",\"MK\":\"Macedonia del Norte\",\"MG\":\"Madagascar\",\"MW\":\"Malaui\",\"MY\":\"Malasia\",\"MV\":\"Maldivas\",\"ML\":\"Mali\",\"MT\":\"Malta\",\"MH\":\"Islas Marshall\",\"MQ\":\"Martinica\",\"MR\":\"Mauritania\",\"MU\":\"Mauricio\",\"YT\":\"Mayotte\",\"MX\":\"México\",\"FM\":\"Micronesia\",\"MD\":\"Moldova\",\"MC\":\"Mónaco\",\"MN\":\"Mongolia\",\"ME\":\"Montenegro\",\"MS\":\"Montserrat\",\"MA\":\"Marruecos\",\"MZ\":\"Mozambique\",\"MM\":\"Myanmar\",\"NA\":\"Namibia\",\"NR\":\"Nauru\",\"NP\":\"Nepal\",\"NL\":\"Países Bajos\",\"NC\":\"Nueva Caledonia\",\"NZ\":\"Nueva Zelanda\",\"NI\":\"Nicaragua\",\"NE\":\"Níger\",\"NG\":\"Nigeria\",\"NU\":\"Niue\",\"NF\":\"Isla Norfolk\",\"KP\":\"Corea del Norte\",\"MP\":\"Islas Marianas del Norte\",\"NO\":\"Noruega\",\"OM\":\"Omán\",\"PK\":\"Pakistán\",\"PW\":\"Palaos\",\"PS\":\"Autoridad Nacional Palestina\",\"PA\":\"Panamá\",\"PG\":\"Papúa Nueva Guinea\",\"PY\":\"Paraguay\",\"PE\":\"Perú\",\"PH\":\"Filipinas\",\"PN\":\"Islas Pitcairn\",\"PL\":\"Polonia\",\"PT\":\"Portugal\",\"PR\":\"Puerto Rico\",\"QA\":\"Qatar\",\"RE\":\"Reunión\",\"RO\":\"Rumanía\",\"RU\":\"Rusia\",\"RW\":\"Ruanda\",\"BL\":\"San Bartolomé\",\"KN\":\"San Cristóbal y Nieves\",\"LC\":\"Santa Lucía\",\"MF\":\"San Martín\",\"PM\":\"San Pedro y Miquelón\",\"VC\":\"San Vicente y las Granadinas\",\"WS\":\"Samoa\",\"SM\":\"San Marino\",\"ST\":\"Santo Tomé y Príncipe\",\"SA\":\"Arabia Saudí\",\"SN\":\"Senegal\",\"RS\":\"Serbia\",\"SC\":\"Seychelles\",\"SL\":\"Sierra Leona\",\"SG\":\"Singapur\",\"SX\":\"Sint Maarten\",\"SK\":\"Eslovaquia\",\"SI\":\"Eslovenia\",\"SB\":\"Islas Salomón\",\"SO\":\"Somalia\",\"ZA\":\"Sudáfrica\",\"GS\":\"Georgia del Sur e Islas Sandwich del Sur\",\"SS\":\"Sudán del Sur\",\"ES\":\"España\",\"LK\":\"Sri Lanka\",\"SH\":\"Santa Elena, Ascensión y Tristán da Cunha\",\"SD\":\"Sudán\",\"SR\":\"Surinam\",\"SJ\":\"Svalbard\",\"SZ\":\"Suazilandia\",\"SE\":\"Suecia\",\"CH\":\"Suiza\",\"SY\":\"Siria\",\"TW\":\"Taiwán\",\"TJ\":\"Tayikistán\",\"TZ\":\"Tanzania\",\"TH\":\"Tailandia\",\"TL\":\"Timor-Leste\",\"TG\":\"Togo\",\"TK\":\"Tokelau\",\"TO\":\"Tonga\",\"TT\":\"Trinidad y Tobago\",\"TN\":\"Túnez\",\"TR\":\"Turquía\",\"TM\":\"Turkmenistán\",\"TC\":\"Islas Turcas y Caicos\",\"TV\":\"Tuvalu\",\"UM\":\"Islas menores alejadas de los EE. UU.\",\"VI\":\"Islas Vírgenes de los Estados Unidos\",\"UG\":\"Uganda\",\"UA\":\"Ucrania\",\"AE\":\"Emiratos Árabes Unidos\",\"GB\":\"Reino Unido\",\"US\":\"Estados Unidos\",\"UY\":\"Uruguay\",\"UZ\":\"Uzbekistán\",\"VU\":\"Vanuatu\",\"VA\":\"Ciudad del Vaticano\",\"VE\":\"Venezuela\",\"VN\":\"Vietnam\",\"WF\":\"Wallis y Futuna\",\"YE\":\"Yemen\",\"ZM\":\"Zambia\",\"ZW\":\"Zimbabue\"} |
| **error_448** | No se puede establecer contacto con el número de teléfono que ha proporcionado. |
| **error_449** | El usuario ha superado el número de reintentos permitidos. |
| **verification_code_input_placeholder_text** | Código de verificación |

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de inscripción a la autenticación multifactor:

![Elementos de la experiencia de usuario al comprobar el correo electrónico en la página de registro](./media/localization-string-ids/localization-mfa1.png)

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de validación de la autenticación multifactor:

![Elementos de la experiencia de usuario al comprobar el correo electrónico en la página de registro](./media/localization-string-ids/localization-mfa2.png)

## <a name="verification-display-control-user-interface-elements"></a>Elementos de la interfaz de usuario del control de pantalla de verificación

Los siguientes son los identificadores de un [control de pantalla de verificación](display-control-verification.md).

| ID | Valor predeterminado |
| -- | ------------- |
|verification_control_but_change_claims |Change |
|verification_control_fail_send_code |No se pudo enviar el código; vuelva a intentarlo más tarde. |
|verification_control_fail_verify_code |No se pudo comprobar el código; vuelva a intentarlo más tarde. |
|verification_control_but_send_code |Envié el código |
|verification_control_but_send_new_code |Envíe un código nuevo. |
|verification_control_but_verify_code |Verifique el código |
|verification_control_code_sent| Se ha enviado el código de verificación. Cópielo en el siguiente cuadro de entrada de texto. |

### <a name="example"></a>Ejemplo

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_change_claims">Change</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_fail_send_code">Failed to send the code, please try again later.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_fail_verify_code">Failed to verify the code, please try again later.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_send_code">Send Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_send_new_code">Send New Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_verify_code">Verify Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_code_sent">Verification code has been sent. Please copy it to the input box below.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="restful-service-error-messages"></a>Mensajes de error del servicio RESTful

Estos son los identificadores de los mensajes de error de [perfil técnico del servicio RESTful](restful-technical-profile.md):

| ID | Valor predeterminado |
| -- | ------------- |
|DefaultUserMessageIfRequestFailed | No se pudo establecer la conexión con el punto de conexión del servicio RESTful. Dirección URL del servicio RESTful: {0} |
|UserMessageIfCircuitOpen | {0} Dirección URL del servicio RESTful: {1} |
|UserMessageIfDnsResolutionFailed | No se pudo resolver el nombre de host del punto de conexión de servicio RESTful. Dirección URL del servicio RESTful: {0} |
|UserMessageIfRequestTimeout | No se pudo establecer la conexión con el punto de conexión del servicio RESTful dentro del límite de tiempo de espera de {0}segundos. Dirección URL del servicio RESTful: {1} |


### <a name="example"></a>Ejemplo

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="DefaultUserMessageIfRequestFailed">Failed to establish connection to restful service end point.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfCircuitOpen">Unable to connect to the restful service end point.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfDnsResolutionFailed">Failed to resolve the hostname of the restful service endpoint.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfRequestTimeout">Failed to establish connection to restful service end point within timeout limit.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="azure-mfa-error-messages"></a>Mensajes de error de Azure MFA

Los siguientes son los identificadores de los mensajes de error de un [perfil técnico de Azure MFA](multi-factor-auth-technical-profile.md).

| ID | Valor predeterminado |
| -- | ------------- |
|UserMessageIfCouldntSendSms | No se puede enviar un SMS al teléfono. Pruebe con otro número de teléfono. |
|UserMessageIfInvalidFormat | El número de teléfono no tiene un formato válido. Corríjalo y vuelva a intentarlo.|
|UserMessageIfMaxAllowedCodeRetryReached | Se escribió un código incorrecto demasiadas veces. Vuelva a intentarlo más tarde.|
|UserMessageIfServerError | No se pudo usar el servicio MFA. Vuelva a intentarlo más tarde.|
|UserMessageIfThrottled | La solicitud se ha limitado. Vuelva a intentarlo más tarde.|
|UserMessageIfWrongCodeEntered|Se escribió un código incorrecto. Vuelva a intentarlo más tarde.|

### <a name="example"></a>Ejemplo

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfCouldntSendSms">Cannot Send SMS to the phone, please try another phone number.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidFormat">Your phone number is not in a valid format, please correct it and try again.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfMaxAllowedCodeRetryReached">Wrong code entered too many times, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfServerError">Cannot use MFA service, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfThrottled">Your request has been throttled, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfWrongCodeEntered">Wrong code entered, please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="azure-ad-sspr"></a>Autoservicio de restablecimiento de contraseña de Azure AD

Estos son los identificadores de los mensajes de error de un [perfil técnico de SSPR de Azure AD](aad-sspr-technical-profile.md).

| ID | Valor predeterminado |
| -- | ------------- |
|UserMessageIfChallengeExpired | El código ha expirado.|
|UserMessageIfInternalError | El servicio de correo electrónico ha encontrado un error interno. Vuelva a intentarlo más tarde.|
|UserMessageIfThrottled | Se han enviado demasiadas solicitudes. Vuelva a intentarlo más tarde.|
|UserMessageIfVerificationFailedNoRetry | Se ha superado el número máximo de intentos de comprobación.|
|UserMessageIfVerificationFailedRetryAllowed | Error de comprobación. Vuelva a intentarlo.|


### <a name="example"></a>Ejemplo

```XML
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInternalError">We are having trouble verifying your email address. Please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfThrottled">There have been too many requests to verify this email address. Please wait a while, then try again.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfChallengeExpired">That code is expired. Please request a new code.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedNoRetry">You've made too many incorrect attempts. Please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedRetryAllowed">That code is incorrect. Please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="one-time-password-error-messages"></a>Mensajes de error de contraseña única

Los siguientes son los identificadores de los mensajes de error de un [perfil técnico de contraseña única](one-time-password-technical-profile.md).

| ID | Valor predeterminado |
| -- | ------------- |
|UserMessageIfMaxRetryAttempted |La comprobación proporcionada de la contraseña única ha superado el número máximo de intentos. |
|UserMessageIfSessionDoesNotExist |La sesión de comprobación de contraseña única ha expirado. |
|UserMessageIfSessionConflict |La sesión de comprobación de contraseña única presenta un conflicto. |
|UserMessageIfInvalidCode |La contraseña única que se proporcionó para la comprobación no es correcta. |
|UserMessageIfVerificationFailedRetryAllowed |Este código no es correcto. Inténtelo de nuevo. | 

### <a name="example"></a>Ejemplo

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfSessionDoesNotExist">You have exceed the maximum time allowed.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfMaxRetryAttempted">You have exceed the number of retries allowed.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidCode">You have entered the wrong code.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfSessionConflict">Cannot verify the code, please try again later.</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedRetryAllowed">That code is incorrect. Please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="claims-transformations-error-messages"></a>Mensajes de error de transformaciones de notificaciones

Los siguientes son los identificadores de los mensajes de error de transformaciones de notificaciones:

| ID | Transformación de notificaciones | Valor predeterminado |
| -- | ------------- |------------- |
|UserMessageIfClaimsTransformationBooleanValueIsNotEqual |[AssertBooleanClaimIsEqualToValue](boolean-transformations.md#assertbooleanclaimisequaltovalue) | Error de comparación de valores de notificación booleano para el tipo de notificación "inputClaim".| 
|DateTimeGreaterThan |[AssertDateTimeIsGreaterThan](date-transformations.md#assertdatetimeisgreaterthan) | Error al comparar el valor de la notificación: El operando izquierdo proporcionado es mayor que el derecho.|
|UserMessageIfClaimsTransformationStringsAreNotEqual |[AssertStringClaimsAreEqual](string-transformations.md#assertstringclaimsareequal) | Error en la comparación de valores de notificaciones mediante StringComparison "OrdinalIgnoreCase".|

### <a name="example"></a>Ejemplo

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Your email address hasn't been verified.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="DateTimeGreaterThan">Expiration date must be greater that the current date.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsTransformationStringsAreNotEqual">The email entry fields do not match. Please enter the same email address in both fields and try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```










