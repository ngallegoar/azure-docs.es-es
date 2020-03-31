---
title: 'Administración de la red de producción de Azure: Microsoft Azure'
description: En este artículo se describe el modo en que Microsoft administra y opera con la red de producción de Azure para proteger los centros de datos de Azure.
services: security
documentationcenter: n
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
ms.date: 05/30/2019
ms.author: terrylan
ms.openlocfilehash: d41fe409b4a44a4c2af3670d76dd3a83a300feae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727128"
---
# <a name="management-and-operation-of-the-azure-production-network"></a>Administración y funcionamiento de la red de producción de Azure    
En este artículo se describe el modo en que Microsoft administra y opera con la red de producción de Azure para proteger los centros de datos de Azure.

## <a name="monitor-log-and-report"></a>Supervisión, registro e informe

La administración y el funcionamiento de la red de producción de Azure es un esfuerzo coordinado entre los equipos de operaciones de Azure y Azure SQL Database. Los equipos usan varias herramientas de supervisión de rendimiento de sistemas y de aplicaciones en el entorno. Y usan las herramientas adecuadas para supervisar dispositivos de red, servidores, servicios y procesos de aplicación.

Para garantizar la ejecución segura de los servicios que se ejecutan en el entorno de Azure, los equipos de operaciones implementan varios niveles de supervisión, registro y generación de informes, incluidas las siguientes acciones:

- Principalmente, Microsoft Monitoring Agent (MA) recopila información de registros de supervisión y diagnóstico desde muchos lugares, incluido el controlador de tejido (FC) y el sistema operativo raíz, y la escribe en los archivos de registro. Finalmente, el agente inserta un subconjunto reducido de la información en una cuenta de almacenamiento de Azure configurada previamente. Además, el servicio de supervisión y diagnóstico independiente lee distintos datos de registro de supervisión y diagnóstico y resume la información. El servicio de supervisión y diagnóstico escribe la información en un registro integrado. Azure usa la supervisión de seguridad de Azure personalizada, que es una extensión del sistema de supervisión de Azure. Tiene componentes que observan, analizan y notifican eventos relativos a la seguridad desde varios lugares de la plataforma.

- La plataforma Windows Fabric de Azure SQL Database proporciona servicios de administración, implementación, desarrollo y control operativo de Azure SQL Database. La plataforma ofrece servicios de implementación distribuidos y de varios pasos, supervisión del mantenimiento, reparaciones automáticas y cumplimiento de las versiones de servicio. Ofrece los siguientes servicios:

   - Funcionalidades de modelado de servicios con entorno de desarrollo de alta fidelidad (los clústeres de centros de datos son escasos y caros).
   - Flujos de trabajo de implementación y actualización de un solo clic para el arranque y el mantenimiento del servicio.
   - Informes de mantenimiento con flujos de trabajo de reparación automáticos para habilitar la recuperación automática.
   - Supervisión, alertas y utilidades de depuración en tiempo real en todos los nodos de un sistema distribuido.
   - Recopilación centralizada de datos operativos y métricas para análisis distribuido de la causa principal e información del servicio.
   - Herramientas operativas para implementación, administración de cambios y supervisión.
   - Los scripts de plataforma Windows Fabric y guardián de Azure SQL Database se ejecutan continuamente y realizan la supervisión en tiempo real.

Si se produce cualquier anomalía, se activa el proceso de respuesta ante incidentes, seguido por el equipo de evaluación de incidentes de Azure. Se notifica al personal de soporte técnico de Azure adecuado para responder al incidente. El seguimiento y la resolución de problemas se documentan y administran en un sistema centralizado de vales. Las métricas de tiempo de actividad del sistema están disponibles en el acuerdo de confidencialidad (NDA) y a petición.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Red corporativa y acceso multifactor a producción
La base de usuarios de la red corporativa incluye personal de soporte técnico de Azure. La red corporativa admite funciones corporativas internas e incluye el acceso a las aplicaciones internas que se usan para la asistencia al cliente de Azure. La red corporativa está separada tanto de forma lógica como física de la red de producción de Azure. El personal de Azure tiene acceso a la red corporativa con equipos portátiles y estaciones de trabajo de Azure. Todos los usuarios deben tener una cuenta de Azure Active Directory (Azure AD), incluido un nombre de usuario y una contraseña, para acceder a los recursos de la red corporativa. El acceso a la red corporativa usa cuentas de Azure AD, que se envían a todo el personal, los contratistas y los proveedores de Microsoft, y se administra mediante Microsoft Information Technology. Los identificadores de usuario únicos distinguen al personal según su estado de empleo en Microsoft.

El acceso a las aplicaciones internas de Azure se controla mediante la autenticación con Servicios de federación de Active Directory (AD FS). AD FS es un servicio hospedado por Microsoft Information Technology que proporciona autenticación de los usuarios de la red corporativa a través de la aplicación de un token seguro y notificaciones de usuario. AD FS permite que las aplicaciones internas de Azure autentiquen usuarios en el dominio de Microsoft Corporate Active Directory. Para acceder a la red de producción desde el entorno de la red corporativa, los usuarios deben autenticarse mediante la autenticación multifactor.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](physical-security.md)
- [Disponibilidad de la infraestructura de Azure](infrastructure-availability.md)
- [Componentes y límites del sistema de información de Azure](infrastructure-components.md)
- [Arquitectura de red de Azure](infrastructure-network.md)
- [Red de producción de Azure](production-network.md)
- [Características de seguridad de Azure SQL Database](infrastructure-sql.md)
- [Supervisión de la infraestructura de Azure](infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](protection-customer-data.md)
