---
title: Administración de clientes potenciales de Marketplace en la nube | Azure Marketplace y AppSource
description: Información general sobre diversos temas relacionados con la publicación de ofertas y los artefactos técnicos en Azure Marketplace y AppSource
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: dsindona
ms.openlocfilehash: 94510d02a28e0364f1c715dbcf9ff641fe2b14fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286139"
---
# <a name="lead-management-for-cloud-marketplace"></a>Administración de clientes potenciales de Marketplace en la nube


Los clientes son el centro de cualquier negocio bien llevado. En la transformación de las adquisiciones de producto de hoy en día, los vendedores tienen que centrarse en conectar directamente con los clientes y generar una relación. Este es el motivo por el que generar clientes potenciales de alta calidad es una herramienta esencial para su ciclo de ventas. Tras publicar su oferta en [Cloud Partner Portal](https://cloudpartner.azure.com/), hay herramientas que le permiten recibir la información de contacto de clientes inmediatamente después de que un cliente exprese interés o implemente el producto en Marketplace. 



## <a name="what-are-leads-in-the-marketplace"></a>¿Qué son los clientes potenciales en Marketplace?

Los clientes potenciales son los clientes que están interesados en sus productos de Marketplace o que están implementándolos desde allí. Sin importar si el producto está publicado en Azure Marketplace o en AppSource, podrá recibir clientes potenciales una vez que la función esté configurada correctamente desde su CRM en las publicaciones de Cloud Partner Portal. 


## <a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Conexión del sistema de CRM con Cloud Partner Portal

Para empezar a recibir clientes potenciales, el conector de administración de clientes potenciales en Cloud Partner Portal está diseñado para conectarse fácilmente con la información de CRM a una lista de sistemas de CRM disponibles. Ahora puede aprovechar fácilmente los clientes potenciales generados por Marketplace sin un importante esfuerzo de ingeniería por integrarse con un sistema externo.

Estas son las instrucciones paso a paso sobre cómo conectar cada uno de los destinos posibles de clientes potenciales:

**Dynamics CRM Online**: [haga clic aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) para ver instrucciones sobre cómo configurar Dynamics CRM Online a fin de obtener clientes potenciales. - 

**Marketo**: [haga clic aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo) para ver instrucciones que le permitan establecer la configuración de clientes potenciales de Marketo a fin de obtener clientes potenciales. - 

**Salesforce**: [haga clic aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce) para ver instrucciones sobre cómo configurar la instancia de Salesforce a fin de obtener clientes potenciales. - 

**Tabla de Azure** - [Haga clic aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table) para ver instrucciones para configurar la cuenta de Azure Storage a fin de obtener clientes potenciales en una tabla de Azure.

**Punto de conexión HTTPS** - [Haga clic aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https) para ver instrucciones que le permitan establecer la configuración de su punto de conexión HTTPS para obtener clientes potenciales.

Una vez que haya configurado correctamente el destino de clientes potenciales y haya presionado Publicar en su oferta, validaremos la conexión y le enviaremos un cliente potencial de prueba. Si está viendo la oferta antes de publicarla en Internet, también puede probar la conexión de los clientes potenciales tratando de adquirir la oferta en el entorno de versión preliminar. Es importante asegurarse de que la configuración de los clientes potenciales se mantenga actualizada para no perder ninguno de estos clientes; así pues, asegúrese de actualizar estas conexiones siempre que cambie algo por su parte.


### <a name="what-are-the-next-steps"></a>¿Cuáles son los siguientes pasos?

Una vez que esté implementada la configuración técnica, debería incorporar estos clientes potenciales a la estrategia actual de ventas y marketing y los procesos operativos. Estamos interesados en recibir una mejor perspectiva sobre el proceso de ventas global, y deseamos trabajar en estrecha colaboración con usted para conseguir clientes potenciales de alta calidad y una cantidad suficiente de datos que le permitan tener éxito. Agradecemos sus comentarios acerca de cómo podemos optimizar y mejorar los clientes potenciales que le enviamos con datos adicionales para obtener los mejores resultados de estos clientes. Díganos si está interesado en ofrecer comentarios y sugerencias para ayudar a que su equipo de ventas tenga mayor éxito con los clientes potenciales de Marketplace.



## <a name="common-lead-configuration-errors-during-publishing-on-cloud-partner-portal"></a>Errores comunes de configuración de clientes potenciales durante la publicación en Cloud Partner Portal 

**No se pudo guardar el cliente potencial en Dynamics CRM. Compruebe la configuración de la cuenta de Dynamics CRM. LastCRMError: no se puede iniciar sesión en Dynamics CRM; LastCRMException:** 

> Si ha seleccionado la autenticación de O365, compruebe si la cuenta de usuario y la contraseña son válidas. Si ha seleccionado AAD, compruebe si el identificador del inquilino, el identificador de la aplicación y la clave secreta de la aplicación coinciden con lo que ha configurado en AAD. Siga las instrucciones que se proporcionan [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics). Si el nombre de usuario y la contraseña de la cuenta son válidos, asegúrese de que tiene acceso a Dynamics 365 y una licencia asignada (es decir, los pasos 11 al 15 si usa Azure Active Directory o la configuración de seguridad si usa un usuario de Office). 

 
**No se pudo guardar el cliente potencial en Dynamics CRM. El usuario no tiene permisos de creación para el atributo leadsourcecode en la entidad de cliente potencial.** 

> La aplicación o el usuario no tienen roles de seguridad para el sistema de escritura de clientes potenciales de Microsoft Marketplace. Siga los pasos 11 a 15 que se indican [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) si usa Azure Active Directory o la configuración de seguridad si usa un usuario de Office.

**No se pudo guardar el cliente potencial en Dynamics CRM mediante AAD. Excepción: No se ha podido encontrar el inquilino. Esta instancia puede ocurrir si no hay suscripciones activas para el inquilino.**  

> El identificador de directorio proporcionado en la sección de administración de clientes potenciales no es un directorio válido. Obtenga el identificador de directorio basándose en las instrucciones que aparecen en el paso 2 (en Azure Active Directory, a partir de [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**No se pudo guardar el cliente potencial en Dynamics CRM. LastCRMError: error de SecLib::RetrievePrivilegeForUser: no hay ningún rol asignado al usuario.**  

> Resolución: asigne el rol de seguridad al sistema de escritura de clientes potenciales de Microsoft Marketplace. Siga las instrucciones que se proporcionan [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) en la configuración de seguridad. 

**No se pudo guardar el cliente potencial en Dynamics CRM mediante AAD. Excepción: No se encontró la aplicación con el identificador en el directorio.** 

> El identificador de aplicación proporcionado en la sección de administración de clientes potenciales no es un directorio válido. Obtenga el identificador de directorio basándose en las instrucciones que aparecen en el paso 8 (en Azure Active Directory, a partir de [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**No se pudo guardar el cliente potencial en Dynamics CRM mediante AAD. Excepción: El identificador del inquilino solicitado no es válido y el formato de dominio externo no es válido** 

> El identificador de directorio proporcionado en la sección de administración de clientes potenciales no es un directorio válido. Obtenga el identificador de directorio basándose en las instrucciones que aparecen en el paso 2 (en Azure Active Directory, a partir de [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**No se pudo guardar el cliente potencial en Dynamics CRM mediante AAD. Excepción: Error al validar las credenciales: se ha proporcionado un secreto de cliente no válido.** 

> Resolución: Inicie sesión en Azure Portal y compruebe que la clave de aplicación coincida con lo que hay en Cloud Partner Portal. Obtenga la contraseña basándose en las instrucciones que aparecen en el paso 10 (en Azure Active Directory, a partir de [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics)). 

**No se pudo guardar el cliente potencial en Dynamics CRM. LastCRMError: Se agotó el tiempo de espera del canal de solicitud mientras se esperaba una respuesta después de 00:02:00. Aumente el valor de tiempo de espera que se pasa a la llamada a la solicitud o aumente el valor de SendTimeout en el enlace. El tiempo asignado a esta operación puede haber sido una fracción de un tiempo de espera mayor.**  

> Resolución: Inicie sesión en Cloud Partner Portal, compruebe los detalles del escaparate electrónico >> Lead destination (Destino de clientes potenciales) >> Dirección URL y compruebe que la instancia de Dynamic CRM sea válida.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**¿Qué son los clientes potenciales y por qué son importantes para mí como anunciante en Marketplace?** 

Los clientes potenciales son los clientes que están implementando sus productos desde Marketplace. Independientemente de que el producto aparezca en [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us) o [AppSource](https://appsource.microsoft.com/), podrá recibir a los clientes potenciales que estén interesados en su producto si ha configurado el destino de clientes potenciales en su oferta.  


**¿Dónde puedo obtener ayuda para la configuración de mi destino de clientes potenciales?** 

Puede encontrar documentación aquí: [Obtención de clientes potenciales](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads), o envíe una incidencia de soporte técnico mediante aka.ms/marketplacepublishersupport, donde debe seleccionar el tipo de oferta y la administración de clientes potenciales. 



**¿Debo configurar un destino de clientes potenciales para poder publicar una oferta en Marketplace?**

Sí, si va a publicar una aplicación SaaS de contacto o servicios de consultoría.  
 


**¿Cómo puedo confirmar que la configuración de cliente potencial es correcta?**

Después de configurar su oferta y el destino de clientes potenciales, publique su oferta. En el paso de validación de clientes potenciales, Marketplace enviará un cliente potencial de prueba al destino de clientes potenciales configurado en su oferta. 


**¿Cómo puedo encontrar al cliente potencial de prueba?**


Busque "MSFT_TEST" en el destino de clientes potenciales; a continuación, le mostramos los datos de un cliente potencial de prueba de ejemplo: 

company = MSFT_TEST_636573304831318844 

country = US 

description = MSFT_TEST_636573304831318844 

email = MSFT_TEST_636573304831318844@test.com

encoding = UTF-8 

encoding = UTF-8 

first_name = MSFT_TEST_636573304831318844 

last_name = MSFT_TEST_636573304831318844 

lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|\<Nombre de la oferta> 

oid = 00Do0000000ZHog 

phone = 1234567890 

title = MSFT_TEST_636573304831318844 

 

**Tengo una oferta activa, pero no veo a los clientes potenciales.**

Cada cliente potencial tendrá datos que se pasen en campos al destino de clientes potenciales seleccionado; los clientes potenciales se incluirán en este formato: **Acción de origen | Oferta** 

  *Orígenes:* :

    "AzureMarketplace", 
    "AzurePortal", 
    "TestDrive",  
    "SPZA" (acronym for AppSource) 

  *Acciones:*

    "INS" - Stands for Installation. This is on Azure Marketplace or AppSource whenever a customer hits the button to acquire your product. 
    "PLT" - Stands for Partner Led Trial. This is on AppSource whenever a customer hits the Contact me button. 

    "DNC" - Stands for Do Not Contact. This is on AppSource whenever a Partner who was cross listed on your app page gets requested to be contacted. We are sharing the heads up that this customer was cross listed on your app, but they do not need to be contacted. 

    "Create" - This is inside Azure Portal only and is whenever a customer purchases your offer to their account. 

    "StartTestDrive" - This is for Test Drives only and is whenever a customer starts their test drive. 


  *Ofertas:*

    "checkpoint.check-point-r77-10sg-byol", 
    "bitnami.openedxcypress", 
    "docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a" 

 

  *Estos son los datos de ejemplo de la información del cliente*

    { 

    "FirstName":"John", 

    "LastName":"Smith", 

    "Email":"jsmith@microsoft.com", 

    "Phone":"1234567890", 

    "Country":"US", 

    "Company":"Microsoft", 

    "Title":"CTO" 

    } 

Obtenga más detalles en [Lead Info](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) (Información de cliente potencial). 


**He configurado el blob de Azure como destino de cliente potencial, ¿por qué no veo al cliente potencial?** 

El cliente potencial se escribe solo cuando se selecciona el almacenamiento de blobs de Azure como destino de clientes potenciales. Cambie a la tabla de Azure para recibir a los clientes potenciales en tiempo real. 


**He recibido un correo electrónico desde Marketplace, ¿por qué no puedo encontrar al cliente potencial en mi instancia de CRM?**  

Es posible que el dominio de correo electrónico del usuario final sea .edu. Por motivos de privacidad, no se pasan lo datos de información de identificación personal provenientes del dominio .edu. Envíe una incidencia de soporte técnico a través de aka.ms/marketplacepublishersupport. 


 **He configurado la tabla o el blob de Azure como destino de cliente potencial, ¿cómo puedo ver a los clientes potenciales?** 

Puede tener acceso a las tablas o blobs desde Azure Portal, o puede descargar e instalar el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) gratuitamente para ver las tablas o blobs de su cuenta de almacenamiento de Azure. 


**He configurado la tabla de Azure como destino del cliente potencial, ¿puedo recibir una notificación cada vez que Marketplace envíe un nuevo cliente potencial?** 

Sí, siga las instrucciones para configurar la combinación de función y tabla de Azure en la documentación que se proporciona [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table). 



**He configurado Salesforce como destino de cliente potencial, ¿por qué no puedo encontrar a los clientes potenciales?** 

Compruebe si el campo de web en el formulario para clientes potenciales es un campo obligatorio de tipo lista desplegable. En caso afirmativo, cámbielo por un campo de texto no obligatorio.  
 

**Hubo un problema con el destino de clientes potenciales y perdí algunos clientes potenciales. ¿Pueden enviármelos en un correo electrónico?** 

Debido a las directivas de PII (información de identificación personal), no podemos compartir la información de clientes potenciales a través de correo electrónico no protegido. 



**He configurado Azure Storage (blobs o tablas) como el destino de clientes potenciales, ¿cuánto va a costar?** 

Los datos de generación de clientes potenciales son pocos (menos de 1 GB para casi todos los anunciantes). El costo dependerá del número de clientes potenciales recibido; si se reciben 1000 clientes en un mes, el costo aproximado sería de 50 céntimos de USD. 
