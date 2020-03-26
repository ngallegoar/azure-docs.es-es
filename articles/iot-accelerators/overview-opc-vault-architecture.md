---
title: 'Arquitectura de OPC Vault: Azure | Microsoft Docs'
description: Arquitectura del servicio de administración de certificados de OPC Vault
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 1e08968034134e2b9ab3b8064387d18663d5c866
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "71200144"
---
# <a name="opc-vault-architecture"></a>Arquitectura de OPC Vault

En este artículo se proporciona información general sobre el microservicio OPC Vault y el módulo IoT Edge de OPC Vault.

Las aplicaciones OPC UA usan certificados de instancia de aplicación para proporcionar seguridad de nivel de aplicación. Una conexión segura se establece mediante criptografía asimétrica, para la que los certificados de aplicación proporcionan el par de claves pública y privada. Los certificados pueden ser autofirmados o estar firmados por una entidad de certificación (CA).

Una aplicación OPC UA tiene una lista de certificados de confianza que representan las aplicaciones en las que confía. Estos certificados pueden ser autofirmados, estar firmados por una entidad de certificación, o pueden ser una entidad de certificación raíz o una subentidad de certificación en sí mismos. Si un certificado de confianza forma parte de una cadena de certificados más grande, la aplicación confía en todos los certificados encadenados al certificado en la lista de confianza. Esto es así siempre que se pueda validar la cadena de certificados completa.

La diferencia principal entre confiar en certificados autofirmados y confiar en un certificado de una entidad de certificación es el esfuerzo de instalación necesario para implementar y mantener la confianza. También hay un esfuerzo adicional para hospedar una entidad de certificación específica de la empresa. 

Para distribuir la confianza para los certificados autofirmados para varios servidores con una sola aplicación cliente, debe instalar todos los certificados de la aplicación de servidor en la lista de confianza de la aplicación cliente. Además, debe instalar el certificado de la aplicación cliente en todas las listas de confianza de aplicaciones de servidor. Este esfuerzo administrativo supone una carga considerable, y es aún mayor si hay que tener en cuenta la duración de los certificados y su renovación.

El uso de una entidad de certificación específica de empresa puede simplificar en gran medida la administración de la confianza con varios servidores y clientes. En este caso, el administrador genera un certificado de instancia de aplicación firmado por una entidad de certificación una vez para cada cliente y servidor que se utiliza. Además, el certificado de entidad de certificación se instala en todas las listas de confianza de aplicaciones, así como en todos los servidores y clientes. Con este enfoque, solo es necesario renovar los certificados expirados y reemplazarlos para las aplicaciones afectadas.

El servicio de administración de certificados de OPC UA de Azure IoT industrial es nuestra solución para administrar una entidad de certificación específica de empresa para aplicaciones OPC UA basadas en el microservicio OPC Vault. Este servicio se basa en el microservicio de OPC Vault. OPC Vault proporciona un microservicio para hospedar una entidad de certificación específica de la empresa en una nube segura. Esta solución está respaldada por servicios protegidos por Azure Active Directory (Azure AD), Azure Key Vault con módulos de seguridad de hardware (HSM), Azure Cosmos DB y, opcionalmente, IoT Hub como almacén de aplicaciones.

El microservicio OPC Vault está diseñado para admitir un flujo de trabajo basado en roles, en el que los administradores de seguridad y los aprobadores con derechos de firma en Azure Key Vault aprueban o rechazan dichas solicitudes.

Para ofrecer compatibilidad con las soluciones de OPC UA existentes, los servicios incluyen compatibilidad con un módulo perimetral con respaldo de microservicio del almacén de OPC. Esto implementa el **servidor de detección global de OPC UA y la interfaz de administración de certificados** para distribuir certificados y listas de confianza según la parte 12 de la especificación. 


## <a name="architecture"></a>Architecture

La arquitectura se basa en el microservicio OPC Vault con un módulo IoT Edge de OPC Vault para la red de la fábrica y una experiencia de usuario web de ejemplo para controlar el flujo de trabajo:

![Diagrama de la arquitectura de OPC Vault](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="opc-vault-microservice"></a>Microservicio OPC Vault

El microservicio OPC Vault consta de las siguientes interfaces a fin de implementar el flujo de trabajo para distribuir y administrar una entidad de certificación específica de empresa para aplicaciones OPC UA.

### <a name="application"></a>Application 
- Una aplicación OPC UA puede ser un servidor o un cliente, o ambos. OPC Vault actúa en este caso como una entidad de registro de aplicaciones. 
- Además de las operaciones básicas para registrar, actualizar y anular el registro de aplicaciones, también hay interfaces para buscar y consultar aplicaciones con expresiones de búsqueda. 
- Las solicitudes de certificado deben hacer referencia a una aplicación válida para procesar una solicitud y emitir un certificado firmado con todas las extensiones específicas de OPC UA. 
- El servicio de aplicación está respaldado por una base de datos de Azure Cosmos DB.

### <a name="certificate-group"></a>Grupo de certificados
- Un grupo de certificados es una entidad, que almacena una entidad de certificación raíz o un certificado de subentidad de certificación, incluida la clave privada para firmar certificados. 
- Se pueden configurar la longitud de la clave RSA, la longitud de hash SHA-2 y las duraciones tanto para los certificados de la entidad de certificación emisora como de la aplicación firmados. 
- Los certificados de entidad de certificación se almacenan en Azure Key Vault, con el nivel 2 de HSM de FIPS 140-2. La clave privada nunca deja el almacenamiento seguro porque la firma se realiza mediante una operación Key Vault protegida de Azure AD. 
- Puede renovar los certificados de entidad de certificación a lo largo del tiempo y que permanezcan en un almacenamiento seguro gracias al historial de Key Vault. 
- La lista de revocación de cada certificado de entidad de certificación también se almacena en Key Vault como secreto. Cuando se anula el registro de una aplicación, un administrador también revoca el certificado de aplicación de la lista de revocación de certificados.
- Puede revocar certificados únicos, así como certificados por lotes.

### <a name="certificate-request"></a>Solicitud de certificado
Una solicitud de certificado implementa el flujo de trabajo para generar un nuevo par de claves o un certificado firmado mediante una solicitud de firma de certificado (CSR) para una aplicación OPC UA. 
- La solicitud se almacena en una base de datos con información adjunta similar a la del asunto o a una solicitud de firma de certificado y una referencia a la aplicación OPC UA. 
- La lógica de negocios en el servicio valida la solicitud en función de la información almacenada en la base de datos de la aplicación. Por ejemplo, el URI de la aplicación en la base de datos debe coincidir con el URI de la aplicación en la solicitud de firma de certificado.
- Un administrador de seguridad con derechos de firma (es decir, el rol de aprobador) aprueba o rechaza la solicitud. Si se aprueba la solicitud, se genera un nuevo par de claves o un certificado firmado (o ambos). La nueva clave privada se almacena de forma segura en Key Vault y el nuevo certificado público firmado se almacena en la base de datos de solicitudes de certificado.
- El solicitante puede sondear el estado de la solicitud hasta que se apruebe o se revoque. Si se aprueba la solicitud, se pueden descargar e instalar la clave privada y el certificado en el almacén de certificados de la aplicación OPC UA.
- El solicitante ya puede aceptar la solicitud para eliminar información innecesaria de la base de datos de solicitudes. 

A lo largo de la duración de un certificado firmado, se puede eliminar una aplicación o una clave puede estar en peligro. En tal caso, un administrador de entidades de certificación puede:
- Eliminar una aplicación, que también elimina todas las solicitudes de certificado pendientes y aprobadas de la aplicación. 
- Eliminar únicamente una solicitud de certificado si solo se renueva o está en peligro una clave.

Ahora las solicitudes de certificado aprobadas y aceptadas en peligro están marcadas como eliminadas.

El administrador puede renovar con regularidad la lista de revocación de certificados de la entidad de certificación del emisor. En el momento de la renovación, se revocan todas las solicitudes de certificado eliminadas y los números de serie de los certificados se agregan a la lista de revocación de certificados. Las solicitudes de certificado revocadas se marcan como revocadas. En los eventos urgentes, también se pueden revocar solicitudes de certificado individuales.

Por último, las listas de revocación de certificados actualizadas están disponibles para su distribución a los clientes y servidores de OPC UA participantes.

## <a name="opc-vault-iot-edge-module"></a>Módulo IoT Edge de OPC Vault
Para admitir un servidor de detección global de la red de fábrica, puede implementar el módulo de OPC Vault en el perímetro. Ejecútelo como una aplicación .NET Core local o iníciela en un contenedor de Docker. Tenga en cuenta que, debido a la falta de compatibilidad con la autenticación de Auth2 en la pila de .NET Standard de OPC UA actual, la funcionalidad del módulo perimetral de OPC Vault está limitada a un rol de lector. No se puede suplantar a un usuario desde el módulo perimetral al microservicio mediante la interfaz estándar de GDS de OPC UA.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre la arquitectura de OPC Vault, puede hacer lo siguiente:

> [!div class="nextstepaction"]
> [Compilación e implementación de OPC Vault](howto-opc-vault-deploy.md)
