---
title: Solución de problemas de conexión a Azure Database for PostgreSQL- Hiperescala (Citus)
description: Aprenda a solucionar problemas de conexión a Azure Database for PostgreSQL- Hiperescala (Citus)
keywords: postgresql connection,connection string,connectivity issues,transient error,connection error
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 10/8/2019
ms.openlocfilehash: a47a6e1860edcb9b2bf89c25e78f6a66e8a7cf4d
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86117719"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql---hyperscale-citus"></a>Solución de problemas de conexión a Azure Database for PostgreSQL- Hiperescala (Citus)

Los problemas de conexión pueden tener varias causas, por ejemplo:

* Configuración de firewall
* Tiempo de espera de conexión agotado
* Información de inicio de sesión incorrecta
* Límite de conexiones para el grupo de servidores alcanzado
* Problemas con la infraestructura del servicio
* Mantenimiento de servicio
* Conmutación por error del nodo de coordinación al nuevo hardware

Por lo general, los problemas de conexión a Hiperescala se pueden clasificar en los siguientes:

* Errores transitorios (corta duración o intermitentes)
* Errores persistentes o no transitorios (errores que se repiten con frecuencia)

## <a name="troubleshoot-transient-errors"></a>Solución de problemas de errores transitorios

Los errores transitorios se producen por diversos motivos. Entre los más comunes se incluyen el mantenimiento del sistema, un error con hardware o software y las actualizaciones del núcleo virtual del nodo de coordinación.

La habilitación de la alta disponibilidad para los nodos del grupo de servidores de Hiperescala puede mitigar estos tipos de problemas automáticamente. Sin embargo, la aplicación debe estar preparada para perder su conexión brevemente. Además, otros eventos pueden tardar más tiempo en mitigarse; por ejemplo, cuando una transacción grande requiere una recuperación de larga duración.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Pasos para resolver los problemas de conectividad transitorios

1. Compruebe el [panel de servicios de Microsoft Azure](https://azure.microsoft.com/status) para comprobar si se produjeron interrupciones durante el tiempo en el que la aplicación informó de los errores.
2. Las aplicaciones que se conectan a un servicio en la nube como Hiperescala (Citus) deben esperar errores transitorios y reaccionar correctamente. Por ejemplo, las aplicaciones deben implementar una lógica de reintentos para controlar estos errores en lugar de mostrarlos como errores de aplicación para los usuarios.
3. Conforme un grupo de servidores se acerca a sus límites de recursos, los errores pueden parecer problemas de conectividad transitorios. Aumentar la RAM del nodo o agregar nodos de trabajo y reequilibrar los datos puede resultar de ayuda.
4. Si los problemas de conectividad continúan, superan los 60 segundos, o se producen más de una vez al día, realice una solicitud de soporte técnico de Azure; para ello, seleccione **Obtener soporte** en el sitio de [soporte técnico de Azure](https://azure.microsoft.com/support/options).

## <a name="troubleshoot-persistent-errors"></a>Solución de problemas de los errores persistentes

Si la aplicación no se puede conectar de forma persistente a Hiperescala (Citus), seguramente se deba a una configuración incorrecta del firewall o un error del usuario.

* Configuración del firewall del nodo de coordinación: Asegúrese de que el firewall del servidor de Hiperescala está configurado para permitir las conexiones desde el cliente, incluidas las puertas de enlace y los servidores proxy.
* Configuración del firewall del cliente: El firewall en el cliente debe permitir las conexiones con el servidor de bases de datos. Algunos firewalls requieren no solo permitir la aplicación por nombre, sino permitir además las direcciones IP y los puertos del servidor.
* Error del usuario: compruebe de nuevo la cadena de conexión. Puede que haya escrito incorrectamente parámetros como el del nombre del servidor. Puede encontrar cadenas de conexión para diversas plataformas de lenguaje y psql en Azure Portal. Vaya a la página **Cadenas de conexión** en el grupo de servidores de Hiperescala (Citus). Tenga en cuenta también que los clústeres de Hiperescala (Citus) tienen una sola base de datos y su nombre predefinido es **citus**.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Pasos para resolver los problemas de conectividad persistentes

1. Configure las [reglas de firewall](howto-hyperscale-manage-firewall-using-portal.md) para permitir la dirección IP del cliente. Con fines temporales de prueba solo, configure una regla de firewall empleando 0.0.0.0 como dirección IP inicial y 255.255.255.255 como dirección IP final. Esta regla abre el servidor a todas las direcciones IP. Si la regla resuelve el problema de conectividad, quítela y cree una regla de firewall para una dirección IP o intervalo de direcciones apropiadamente limitados.
2. En todos los firewalls entre el cliente e Internet, asegúrese de que el puerto 5432 está abierto para las conexiones salientes.
3. Compruebe la cadena de conexión y otras opciones de conexión.
4. Compruebe el estado del servicio en el panel.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda los conceptos de las [reglas de firewall en Azure Database for PostgreSQL: Hiperescala (Citus)](concepts-hyperscale-firewall-rules.md)
* Consulte [Administración de reglas de firewall en Azure Database for PostgreSQL: Hiperescala (Citus)](howto-hyperscale-manage-firewall-using-portal.md)
