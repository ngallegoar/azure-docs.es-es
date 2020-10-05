---
title: Azure Private Link para Azure Data Factory
description: Más información sobre el funcionamiento de Azure Private Link en Azure Data Factory.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/01/2020
ms.openlocfilehash: 48ab83db3dcbcf5c99b640ccab205ed1f0ee7ca1
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604383"
---
# <a name="azure-private-link-for-azure-data-factory"></a>Azure Private Link para Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Con Azure Private Link, puede conectarse a varias implementaciones de plataforma como servicio (PaaS) en Azure a través de un punto de conexión privado. Un punto de conexión privado es una dirección IP privada dentro de una red virtual y una subred específicas. Para obtener una lista de las implementaciones PaaS que admiten la funcionalidad Private Link, vea la [documentación de Private Link](https://docs.microsoft.com/azure/private-link/). 

## <a name="secure-communication-between-customer-networks-and-azure-data-factory"></a>Comunicación segura entre las redes del cliente y Azure Data Factory 
Puede configurar una red virtual de Azure como una representación lógica de la red en la nube. Esta acción tiene estas ventajas derivadas:
* Ayuda a proteger los recursos de Azure frente a ataques en redes públicas.
* Permite que las redes y Data Factory se comuniquen entre sí de forma segura. 

También puede conectar una red local a la red virtual mediante la configuración de una conexión VPN del protocolo de seguridad de Internet (IPsec) (de sitio a sitio) o una conexión de Azure ExpressRoute (emparejamiento privado). 

Asimismo, puede instalar un entorno de ejecución de integración autohospedado en una máquina local o en una máquina virtual de la red virtual. Esto le permite:
* Ejecutar actividades de copia entre un almacén de datos en la nube y un almacén de datos en una red privada.
* Distribuir las actividades de transformación frente a los recursos de proceso en una red local o una red virtual de Azure. 

Se requieren varios canales de comunicación entre Azure Data Factory y la red virtual del cliente, tal y como se muestra en la tabla siguiente:

| Domain | Port | Descripción |
| ---------- | -------- | --------------- |
| `pe-adf.azure.com` | 443 | Un plano de control, necesario para la creación y supervisión en Data Factory. |
| `*.{region}.datafactory.azure.net` | 443 | El entorno de ejecución de integración autohospedado lo necesita para conectarse al servicio Data Factory. |
| `*.servicebus.windows.net` | 443 | Lo necesita el entorno de ejecución de integración autohospedado para la creación interactiva. |
| `download.microsoft.com` | 443 | Lo necesita el entorno de ejecución de integración autohospedado para descargar las actualizaciones. |

Con la compatibilidad de Private Link para Azure Data Factory, puede:
* Crear un punto de conexión privado en la red virtual.
* Habilitar la conexión privada a una instancia de factoría de datos específica. 

Las comunicaciones con el servicio Azure Data Factory pasan a través de Private Link y ayudan a ofrecer conectividad privada segura. No es necesario configurar el dominio y el puerto anteriores en la red virtual ni el firewall corporativo para proporcionar una manera más segura de proteger los recursos.  

![Diagrama de Private Link para la arquitectura de Azure Data Factory.](./media/data-factory-private-link/private-link-architecture.png)

La habilitación del servicio Private Link para cada uno de los canales de comunicación anteriores ofrece la siguiente funcionalidad:
- **Admitido**:
   - Puede crear y supervisar la factoría de datos en la red virtual, incluso si bloquea todas las comunicaciones salientes.
   - Las comunicaciones de comandos entre el entorno de ejecución de integración autohospedado y el servicio Azure Data Factory se pueden realizar de forma segura en un entorno de red privado. El tráfico entre el entorno de ejecución de integración autohospedado y el servicio Azure Data Factory pasa a través Private Link. 
- **No admitido actualmente**:
   - La creación interactiva que usa el entorno de ejecución de integración autohospedado pasa a través de Private Link como, por ejemplo, en el caso de las pruebas de conexiones, el examen de la lista de carpetas y de tablas, la obtención de esquemas y la vista previa de los datos.
   - La nueva versión del entorno de ejecución de integración autohospedado se puede descargar automáticamente desde el Centro de descarga de Microsoft si habilita la actualización automática.

   > [!NOTE]
   > Para las funcionalidades que no se admiten actualmente deberá configurar el dominio y el puerto previamente mencionados en la red virtual o el firewall corporativo. 

> [!WARNING]
> Cuando cree un servicio vinculado, asegúrese de que las credenciales se almacenan en un almacén de claves de Azure. De lo contrario, las credenciales no funcionarán al habilitar Private Link en Azure Data Factory.

## <a name="set-up-private-link-for-azure-data-factory"></a>Configuración de Private Link para Azure Data Factory
Puede crear puntos de conexión privados mediante [Azure Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal), PowerShell o la CLI de Azure.

También puede ir a Azure Data Factory en Azure Portal y crear un punto de conexión privado, como se muestra aquí:

![Captura de pantalla del panel "Conexiones de punto de conexión privado" para crear un punto de conexión privado.](./media/data-factory-private-link/create-private-endpoint.png)


Si desea bloquear el acceso público a esta instancia de Azure Data Factory y solo permitir el acceso a través de Private Link, deshabilite el acceso de red para Azure Data Factory en Azure Portal, como se muestra aquí:

![Captura de pantalla del panel "Acceso de red" para crear un punto de conexión privado.](./media/data-factory-private-link/disable-network-access.png)

> [!NOTE]
> Deshabilitar el acceso a la red pública solo es aplicable al entorno de ejecución de integración autohospedado, no a Azure Integration Runtime ni a SQL Server Integration Services (SSIS) Integration Runtime.

> [!NOTE]
> Puede seguir accediendo al portal de Azure Data Factory a través de la red pública después de deshabilitar el acceso a la red pública.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una factoría de datos con la interfaz de usuario de Azure Data Factory](quickstart-create-data-factory-portal.md)
- [Introducción al servicio Azure Data Factory](introduction.md)
- [Creación visual de Azure Data Factory](author-visually.md)

