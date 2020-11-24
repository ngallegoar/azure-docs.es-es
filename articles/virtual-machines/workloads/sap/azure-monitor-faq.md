---
title: 'Preguntas frecuentes: Azure Monitor para soluciones de SAP | Microsoft Docs'
description: En este artículo, obtendrá respuestas a preguntas frecuentes sobre las soluciones de Azure Monitor para SAP.
author: rdeltcheva
ms.service: virtual-machines
ms.topic: article
ms.date: 06/30/2020
ms.author: radeltch
ms.reviewer: cynthn
ms.openlocfilehash: f872264d5f7bdf4abded516412fe55c417a92e1e
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566119"
---
# <a name="azure-monitor-for-sap-solutions-faq-preview"></a>Preguntas frecuentes acerca de las soluciones de Azure Monitor para SAP (versión preliminar)
## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

En este artículo se ofrecen respuestas a preguntas frecuentes (P+F) sobre soluciones de Azure Monitor para SAP.  

 - **¿Tengo que pagar por Azure Monitor para soluciones de SAP?**  
No hay ninguna cuota de licencias para Azure Monitor para soluciones de SAP.  
Sin embargo, los clientes son responsables de asumir el costo de los componentes del grupo de recursos administrados.  

 - **¿En qué regiones está este servicio disponible para la versión preliminar pública?**  
En el caso de la versión preliminar pública, este servicio estará disponible en el Este de EE. UU. 2, Oeste de EE. UU. 2, Este de EE. UU. y Oeste de Europa.  

 - **¿Es necesario proporcionar permisos para permitir la implementación de un grupo de recursos administrados en mi suscripción?**  
No, no se necesitan permisos explícitos.  

 - **¿Dónde se encuentra la VM del recopilador?**  
En el momento de implementar el recurso de Azure Monitor para soluciones de SAP, se recomienda que los clientes elijan la misma red virtual para supervisar el recurso que usa su servidor de SAP HANA. Por lo tanto, se recomienda que la VM del recopilador resida en la misma red virtual que el servidor de SAP HANA. Si los clientes usan una base de datos que no es HANA, la VM del recopilador residirá en la misma red virtual que la base de datos que no sea HANA.  

 - **¿Qué versiones de HANA se admiten?**  
HANA 1.0 SPS 12 (Rev. 120 o superior) y HANA 2.0 SPS03 o superior  

 - **¿Qué configuraciones de implementación de HANA se admiten?**  
Se admiten las siguientes configuraciones:
   - Nodo único (escalado vertical) y varios nodos (escalado horizontal)  
   - Contenedor de una sola base de datos (HANA 1.0 SPS 12) y varios contenedores de base de datos (HANA 1.0 SPS 12 o HANA 2.0)  
   - Conmutación por error de host automática (n + 1) y HSR  

 - **¿Qué versiones de SQL Server se admiten?**  
SQL Server 2012 SP4 o posterior.  

 - **¿Qué configuraciones de SQL Server se admiten?**  
Se admiten las siguientes configuraciones:
   - Instancias independientes predeterminadas o designadas de una máquina virtual  
   - Instancias o instancias en clúster de una configuración Always On cuando se usa el nombre virtual del recurso en clúster o el nombre del cliente de escucha AlwaysOn. Actualmente, no se recopilan métricas específicas de clúster o AlwaysOn.    
   - Actualmente, Azure SQL Database no se admite  

 - **¿Qué ocurre si elimino accidentalmente el grupo de recursos administrados?**  
El grupo de recursos administrados está bloqueado de forma predeterminada. Por lo tanto, las posibilidades de que los clientes eliminen accidentalmente el grupo de recursos administrados son minúsculas.  
Si un cliente elimina el grupo de recursos administrados, Azure Monitor para soluciones de SAP dejará de funcionar. El cliente tendrá que implementar un nuevo recurso de Azure Monitor para soluciones de SAP y volver a empezar.  

 - **¿Qué roles necesito en mi suscripción de Azure para implementar el recurso de Azure Monitor para soluciones de SAP?**  
Rol de colaborador.  

 - **¿Cuál es el SLA de este producto?**  
Las versiones preliminares están excluidas de los contratos de nivel de servicio. Lea las condiciones de licencia completas mediante la imagen de Marketplace de Azure Monitor para soluciones de SAP.  

 - **¿Puedo supervisar todo el panorama desde esta solución?**  
Actualmente, puede supervisar la base de datos de HANA, la infraestructura subyacente, el clúster de alta disponibilidad y Microsoft SQL Server en versión preliminar pública.  

 - **¿Este servicio reemplaza a SAP Solution Manager?**  
No. Los clientes pueden seguir usando SAP Solution Manager para la supervisión de procesos empresariales.  

 - **¿Cuál es el valor de este servicio respecto a las soluciones tradicionales, como SAP HANA Cockpit/Studio?**  
Azure Monitor para soluciones de SAP no es específico para bases de datos de HANA. Azure Monitor para soluciones de SAP también admite AnyDB.  

## <a name="next-steps"></a>Pasos siguientes

- Cree su primer recurso de Azure Monitor para soluciones de SAP.
