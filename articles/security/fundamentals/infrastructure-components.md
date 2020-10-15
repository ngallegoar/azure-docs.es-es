---
title: Componentes y límites del sistema de información de Azure
description: En este artículo se proporciona una descripción general de la arquitectura y administración de Microsoft Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 68535f70507e7a81d217f4148314a3d76ec832ea
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68727212"
---
# <a name="azure-information-system-components-and-boundaries"></a>Componentes y límites del sistema de información de Azure
En este artículo se proporciona una descripción general de la arquitectura y administración de Azure. El entorno del sistema de Azure se compone de las siguientes redes:

- Red de producción de Microsoft Azure (red de Azure)
- Red corporativa de Microsoft (red corporativa)

La responsabilidad de las operaciones y el mantenimiento de estas redes corresponde a equipos de TI distintos.

## <a name="azure-architecture"></a>Arquitectura de Azure
Azure es una plataforma de informática en la nube y una infraestructura para compilar, implementar y administrar aplicaciones y servicios a través de una red de centros de datos. Microsoft administra estos centros de datos. En función del número de recursos que especifique, Azure crea máquinas virtuales (VM) basadas en las necesidades de los recursos. Estas máquinas virtuales se ejecutan en un hipervisor de Azure, que está diseñado para su uso en la nube y no es accesible al público.

En cada nodo de servidor físico de Azure hay un hipervisor que se ejecuta directamente sobre el hardware. El hipervisor divide un nodo en un número variable máquinas virtuales invitadas. Cada nodo tiene también una máquina virtual raíz, que ejecuta el sistema operativo host. El Firewall de Windows está habilitado en cada VM. El usuario define qué puertos son direccionables mediante la configuración del archivo de definición de servicio. Estos puertos son los únicos abiertos y direccionables, interna o externamente. El hipervisor y el sistema operativo raíz actúan como mediadores en todo el tráfico y el acceso al disco y a la red.

En la capa de host, las VM de Azure ejecutan una versión personalizada y protegida de Windows Server más reciente. Azure usa una versión de Windows Server que incluye solo los componentes necesarios para hospedar las máquinas virtuales. Esto mejora el rendimiento y reduce la superficie expuesta a ataques. El hipervisor, que no depende de la seguridad del sistema operativo, impone los límites de la máquina.

### <a name="azure-management-by-fabric-controllers"></a>Administración de Azure con los controladores de tejido

En Azure, las máquinas virtuales que se ejecutan en servidores físicos (hojas o nodos) se agrupan en clústeres en un número aproximado de 1000. Un componente de software de plataforma de escalado horizontal y redundante, llamado controlador de tejido (FC), administra las máquinas virtuales por separado.

Cada FC administra el ciclo de vida de las aplicaciones que se ejecutan en su clúster, aprovisiona y supervisa el estado del hardware bajo su control. Ejecuta operaciones autónomas, como la reencarnación de instancias de máquina virtual en servidores con estado correcto cuando determina que un servidor ha generado un error. El controlador de tejido también realiza las operaciones de administración de aplicaciones, como la implementación, actualización y escalado horizontal de las aplicaciones.

El centro de datos se divide en clústeres. Los clústeres aíslan los errores al nivel del controlador de tejido, y evitan que ciertas clases de errores afecten a los servidores más allá del clúster en el que se producen. Los controladores de tejido que atienden a un clúster de Azure determinado se agrupan en un clúster de controlador de tejido.

### <a name="hardware-inventory"></a>Inventario de hardware

Durante el proceso de configuración de arranque el controlador de tejido prepara un inventario de hardware y dispositivos de red de Azure. El hardware y los componentes de red nuevos que ingresan en el entorno de producción de Azure deben seguir el proceso de configuración de arranque. El FC es responsable de administrar todo el inventario enumerado en el archivo de configuración datacenter.xml.

### <a name="fc-managed-operating-system-images"></a>Imágenes del sistema operativo administrado por controlador de tejido

El equipo del sistema operativo proporciona imágenes en forma de discos duros virtuales que se implementan en todas los host y máquinas virtuales invitadas en el entorno de producción de Azure. El equipo crea estas imágenes base siguiendo un proceso automatizado de compilación sin conexión. La imagen base es una versión del sistema operativo en la que el kernel y otros componentes principales se han modificado y optimizado para admitir el entorno de Azure.

Hay tres tipos de imágenes de sistema operativo administrado por tejido:

- Host: se trata de un sistema operativo personalizado que se ejecuta en las máquinas virtuales de host.
- Nativo: es un sistema operativo nativo que se ejecuta en inquilinos (por ejemplo, Azure Storage). Este sistema operativo no tiene ningún hipervisor.
- Invitado: es un sistema operativo invitado que se ejecuta en máquinas virtuales invitadas.

Los sistemas operativos host y nativo administrados por controlador de tejido están diseñados para su uso en la nube y no son accesibles públicamente.

#### <a name="host-and-native-operating-systems"></a>Sistemas operativos host y nativo

Los sistemas operativos host y nativo son imágenes reforzadas de sistemas operativos que hospedan a los agentes de tejido, y se ejecutan en un nodo de proceso (se ejecuta como la primera máquina virtual en el nodo) y en los nodos de almacenamiento. La ventaja de utilizar imágenes base optimizadas de host y nativo es que se reduce la superficie expuesta por las API o los componentes no utilizados. Estos elementos pueden suponer grandes riesgos de seguridad y aumentar la superficie del sistema operativo. Los sistemas operativos de superficie reducida solo incluyen los componentes necesarios para Azure.

#### <a name="guest-operating-system"></a>Sistema operativo invitado

Los componentes internos de Azure que se ejecutan en las máquinas virtuales de sistema operativo invitado, no tienen oportunidad de ejecutar el Protocolo de escritorio remoto. Cualquier cambio realizado en los valores de configuración de referencia tiene que pasar por el proceso de cambio y administración de versiones.

## <a name="azure-datacenters"></a>Centros de datos de Azure
El equipo Microsoft Cloud Infrastructure and Operations (MCIO) administra la infraestructura física y las instalaciones de los centros de datos para todos los servicios en línea de Microsoft. MCIO es principalmente responsable de administrar los controles físicos y de entorno dentro de los centros de datos, así como de administrar y brindar soporte técnico a los dispositivos de red perimetrales externos (como enrutadores perimetrales y enrutadores de centros de datos). MCIO también es responsable de configurar el hardware de servidor mínimo en bastidores en el centro de datos. Los clientes no tienen ninguna interacción directa con Azure.

## <a name="service-management-and-service-teams"></a>Administración de servicios y equipos de servicio
El soporte técnico del servicio de Azure está a cargo de varios grupos de ingeniería, conocidos como equipos de servicio. Cada uno de los equipos de servicio es responsable de un área de soporte técnico para Azure. Cada equipo de servicio tiene que tener un ingeniero disponible las 24 horas del día, los 7 días de la semana, para investigar y resolver errores en el servicio. De manera predeterminada, los equipos de servicio no tienen acceso físico al hardware que funciona en Azure.

Los equipos de servicio son:

- Plataforma de aplicaciones
- Azure Active Directory
- Azure Compute
- Red de Azure
- Servicios de ingeniería en la nube
- ISSD: Seguridad
- Autenticación multifactor
- SQL Database
- Storage

## <a name="types-of-users"></a>Tipos de usuarios
Los empleados (o contratistas) de Microsoft se consideran usuarios internos. Todos los demás usuarios se consideran usuarios externos. Todos los usuarios internos de Azure tienen un estado de empleado clasificado con un nivel de confidencialidad que define su acceso a los datos del cliente (con acceso o sin acceso). En la tabla siguiente se describen los privilegios de usuario en Azure (permiso de autorización una vez que se realiza la autenticación):

| Role | Interno o externo | Nivel de confidencialidad | Privilegios autorizados y funciones desempeñadas | Tipo de acceso
| --- | --- | --- | --- | --- |
| Ingeniero de centro de datos de Azure | Interno | Sin acceso a los datos de clientes | Administrar la seguridad física de las instalaciones. Llevar a cabo patrullas dentro y fuera del centro de datos y supervisar todos los puntos de entrada. Acompañar al personal sin autorización que proporciona servicios generales (como comidas o limpieza) o trabajos de TI en el centro de datos desde su entrada hasta su salida del centro de datos. Realizar la supervisión y el mantenimiento de rutina del hardware de red. Llevar a cabo la administración de incidentes y el trabajo break-fix utilizando una variedad de herramientas. Realizar la supervisión y el mantenimiento de rutina del hardware físico en los centros de datos. Acceder al entorno a petición de los propietarios de las instalaciones. Capacidad para llevar a cabo investigaciones forenses, registro de informe de incidentes, y para exigir requisitos obligatorios de aprendizaje de seguridad y directivas. Propiedad operativa y mantenimiento de las herramientas de seguridad críticas, como escáneres y recopilación de registros. | Acceso persistente al entorno. |
| Evaluación de prioridades en los incidentes de Azure (ingenieros de respuesta rápida) | Interno | Acceso a los datos de clientes | Administrar las comunicaciones entre los equipos de MCIO, soporte técnico e ingeniería. Evaluar los incidentes de la plataforma, los problemas de implementación y las solicitudes de servicio. | Acceso Just-In-Time al entorno, con acceso persistente limitado a sistemas que no son de clientes. |
| Ingenieros de implementación de Azure | Interno | Acceso a los datos de clientes | Implementar y actualizar los componentes de la plataforma, el software y los cambios de configuración programados en apoyo de Azure. | Acceso Just-In-Time al entorno, con acceso persistente limitado a sistemas que no son de clientes. |
| Soporte técnico de interrupción de clientes de Azure (inquilino) | Interno | Acceso a los datos de clientes | Depurar y diagnosticar los errores e interrupciones de la plataforma para los inquilinos de proceso individuales y las cuentas de Azure. Analizar los errores. Impulsar las correcciones críticas en la plataforma o el cliente e impulsar mejoras técnicas en el soporte técnico. | Acceso Just-In-Time al entorno, con acceso persistente limitado a sistemas que no son de clientes. |
| Ingenieros de sitio activo de Azure (ingenieros de supervisión) e incidente | Interno | Acceso a los datos de clientes | Diagnosticar y mitigar el mantenimiento de la plataforma mediante herramientas de diagnóstico. Impulsar correcciones para los controladores de volumen, reparar elementos que son resultado de las interrupciones y ayudar en las acciones de restauración de interrupciones. | Acceso Just-In-Time al entorno, con acceso persistente limitado a sistemas que no son de clientes. |
|Clientes de Azure | Externo | N/D | N/D | N/D |

Azure utiliza identificadores únicos para autenticar a usuarios de la organización y clientes (o procesos que actúan en nombre de usuarios de la organización). Esto se aplica a todos los recursos y dispositivos que forman parte del entorno de Azure.

### <a name="azure-internal-authentication"></a>Autenticación interna de Azure

Las comunicaciones entre los componentes internos de Azure están protegidas con cifrado TLS. En la mayoría de los casos, los certificados X.509 son autofirmados. Los certificados con conexiones a las que podría accederse desde fuera de la red de Azure son una excepción al igual que los certificados para los controladores de tejido. Una entidad emisora (CA) de Microsoft, respaldada por una CA raíz de confianza, emite los certificados de los FC. Esto permite que las claves públicas de FC se sustituyan fácilmente. Además, las herramientas de desarrollador de Microsoft usan claves públicas de controlador de tejido. Cuando los desarrolladores envían nuevas imágenes de aplicación, estas se cifran con una clave pública de controlador de tejido con el fin de proteger los secretos insertados.

### <a name="azure-hardware-device-authentication"></a>Autenticación de dispositivos de hardware de Azure

El controlador de tejido mantiene un conjunto de credenciales (claves o contraseñas) que se usan para autenticarse en varios dispositivos de hardware bajo su control. Microsoft utiliza un sistema para evitar el acceso a estas credenciales. De forma específica, el transporte, la persistencia y el uso de estas credenciales está diseñado para evitar que los desarrolladores, los administradores y el personal y servicios de copia de seguridad de Azure tengan acceso a información confidencial o privada.

Microsoft usa cifrado basado en la clave pública de identidad maestra del controlador de tejido. Esto se produce durante los tiempos de configuración y reconfiguración del controlador de tejido, para transferir las credenciales usadas para acceder a dispositivos de hardware de red. Cuando el controlador de tejido necesita las credenciales, las recupera y descifra.

### <a name="network-devices"></a>Dispositivos de red

El equipo de red de Azure configura las cuentas de servicio de red para permitir que un cliente de Azure se autentique en los dispositivos de red (enrutadores, conmutadores y equilibradores de carga).

## <a name="secure-service-administration"></a>Administración segura del servicio
El personal de operaciones de Azure tiene que usar estaciones de trabajo de administración seguras (SAW). Los clientes pueden implementar controles similares mediante el uso de estaciones de trabajo de acceso con privilegios. Con las SAW, el personal administrativo utiliza una cuenta administrativa asignada individualmente que es independiente de la cuenta estándar del usuario. La estación de trabajo de administración segura (SAW) se basa en dicha práctica de separación de cuentas, y proporciona una estación de trabajo de confianza para las cuentas confidenciales.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que Microsoft hace para ayudar a proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](physical-security.md)
- [Disponibilidad de la infraestructura de Azure](infrastructure-availability.md)
- [Arquitectura de red de Azure](infrastructure-network.md)
- [Red de producción de Azure](production-network.md)
- [Características de seguridad de Azure SQL Database](infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](protection-customer-data.md)
