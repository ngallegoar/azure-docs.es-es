---
author: billmath
ms.service: active-directory
ms.subservice: governance
ms.topic: include
ms.date: 10/16/2019
ms.author: billmath
ms.openlocfilehash: be33e492f44f6926f2ae2d133cf112245fba15f1
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93135023"
---
## <a name="cloud-hr-application-to-azure-active-directory-user-provisioning"></a>Aplicación de RR. HH. en la nube para el aprovisionamiento de usuarios de Azure Active Directory

Históricamente, el personal del departamento de TI ha confiado en métodos manuales para crear, actualizar y eliminar a empleados. Ha usado métodos como la carga de archivos CSV o scripts personalizados para sincronizar los datos de los empleados. Estos procesos de aprovisionamiento son propensos a errores, inseguros y difíciles de administrar.

Para administrar los ciclos de vida de la identidad de los empleados, proveedores o trabajadores temporales, el [servicio de aprovisionamiento de usuarios de Azure Active Directory (Azure AD)](../articles/active-directory/app-provisioning/user-provisioning.md) ofrece integración con aplicaciones de recursos humanos (RR. HH.) basadas en la nube. Entre los ejemplos de aplicaciones se incluyen Workday o SuccessFactors.

Azure AD usa esta integración para habilitar los siguientes flujos de trabajo de aplicación de RR. HH. en la nube:

- **Aprovisionar a los usuarios en Active Directory:** aprovisione conjuntos de usuarios seleccionados de una aplicación de RR. HH. en la nube en uno o varios dominios de Active Directory.
- **Aprovisionar a los usuarios solo en la nube en Azure AD:** en escenarios en los que no se usa Active Directory, aprovisione a los usuarios directamente desde la aplicación de RR. HH. en la nube en Azure AD.
- **Reescribir en la aplicación de RR. HH. en la nube:** reescriba los atributos de nombre de usuario y dirección de correo electrónico de Azure AD en la aplicación de RR. HH. en la nube.


## <a name="enabled-hr-scenarios"></a>Escenarios de RR. HH. habilitados

El servicio de aprovisionamiento de usuarios de Azure AD permite la automatización de los siguientes escenarios de administración del ciclo de vida de la identidad basados en RR. HH.:

- **Contratación de nuevos empleados:** cuando se agrega a un nuevo empleado a la aplicación de RR. HH. en la nube, se crea automáticamente una cuenta de usuario en Active Directory y Azure AD con la opción de reescribir los atributos de nombre de usuario y dirección de correo electrónico en la aplicación de RR. HH. en la nube.
- **Actualizaciones de los perfiles y los atributos de los empleados:** cuando se actualiza el registro de un empleado (por ejemplo, el nombre, el puesto o el jefe) en la aplicación de RR. HH. en la nube, su cuenta de usuario se actualiza automáticamente en Active Directory y Azure AD.
- **Bajas de empleados:** cuando se da de baja a un empleado en la aplicación de RR. HH. en la nube, su cuenta de usuario se deshabilita automáticamente en Active Directory y Azure AD.
- **Recontrataciones de empleados:** cuando se vuelve a contratar a un empleado en la aplicación de RR. HH. en la nube, su cuenta anterior se puede volver a activar o aprovisionar automáticamente en Active Directory y Azure AD.

## <a name="who-is-this-integration-best-suited-for"></a>¿Para quién es idónea esta integración?

La integración de aplicaciones de RR. HH. en la nube con el aprovisionamiento de usuarios de Azure AD es ideal para organizaciones que:

- Buscan una solución basada en la nube y precompilada para el aprovisionamiento de usuarios de RR. HH. en la nube.
- Necesitan un aprovisionamiento de usuarios directos de la aplicación de RR. HH. en la nube en Active Directory o Azure AD.
- Necesitan que los usuarios se aprovisionen con los datos obtenidos de la aplicación de RR. HH. en la nube.
- Necesitan combinar y mover usuarios, y dejar que se sincronicen con uno o varios bosques, dominios o unidades organizativas de Active Directory en función de únicamente la información de cambio detectada en la aplicación de RR. HH. en la nube.
- Usan Office 365 para el correo electrónico.


### <a name="key-benefits"></a>Ventajas principales

Esta función de aprovisionamiento de TI controlada por RR. HH. ofrece las siguientes ventajas empresariales importantes:

- **Aumento de la productividad:** ahora puede automatizar la asignación de las cuentas de usuario y las licencias de Office 365, así como proporcionar acceso a los grupos clave. La automatización de las asignaciones proporciona a las nuevas contrataciones acceso inmediato a sus herramientas de trabajo y aumenta la productividad.
- **Administración de riesgos:** para aumentar la seguridad, puede automatizar los cambios en función del estado de los empleados o de las pertenencias a grupos con datos que fluyen desde la aplicación de RR. HH. en la nube. La automatización de los cambios garantiza que las identidades de los usuarios y el acceso a las aplicaciones principales se actualizan automáticamente cuando los usuarios se mueven o salen de la organización.
- **Control de cumplimiento y gobernanza:** Azure AD admite registros de auditoría nativos de las solicitudes de aprovisionamiento de usuarios que hacen las aplicaciones de los sistemas de origen y de destino. Con la auditoría, puede hacer un seguimiento de quién tiene acceso a las aplicaciones desde una sola pantalla.
- **Administración del costo:** el aprovisionamiento automático reduce los costos al evitar ineficiencias y errores humanos asociados con el aprovisionamiento manual. Reduce la necesidad de desarrollar soluciones personalizadas de aprovisionamiento de usuarios a lo largo del tiempo mediante plataformas heredadas y obsoletas.
