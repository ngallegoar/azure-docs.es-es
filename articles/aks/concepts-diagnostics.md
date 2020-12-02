---
title: Introducción a los diagnósticos de Azure Kubernetes Service (AKS)
description: Obtenga información sobre los clústeres de autodiagnóstico en Azure Kubernetes Service.
services: container-service
author: yunjchoi
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: yunjchoi
ms.openlocfilehash: 9d06ad2f740394a4959f3665c5d6311e71841241
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94960842"
---
# <a name="azure-kubernetes-service-diagnostics-preview-overview"></a>Introducción a los diagnósticos de Azure Kubernetes Service (versión preliminar)

La solución de problemas de clústeres de Azure Kubernetes Service (AKS) es una parte importante del mantenimiento de su clúster, especialmente si este ejecuta cargas de trabajo críticas. AKS Diagnostics es una experiencia inteligente de diagnóstico automático que le ayuda a identificar y resolver problemas en su clúster. AKS Diagnostics es nativa de la nube y se puede usar sin necesidad de configuración o de costos de facturación adicionales.

Esta característica ahora está en versión preliminar pública.

## <a name="open-aks-diagnostics"></a>Abrir AKS Diagnostics

Para acceder a AKS Diagnostics:

- Vaya a su clúster de Kubernetes en [Azure Portal](https://portal.azure.com).
- En el panel izquierdo, haga clic en **Diagnosticar y solucionar problemas**, lo que abrirá AKS Diagnostics.
- Elija la categoría que describa mejor el problema del clúster con las palabras clave del icono de la página principal o escriba una palabra clave que describa mejor su problema en la barra de búsqueda, por ejemplo, _Problemas del nodo de clúster_.

![Página principal](./media/concepts-diagnostics/aks-diagnostics-homepage.png)

## <a name="view-a-diagnostic-report"></a>Visualización de un informe de diagnóstico

Después de hacer clic en una categoría, podrá ver un informe de diagnóstico específico del clúster. El informe de diagnóstico señala de manera inteligente si hay algún problema en el clúster con iconos de estado. Puede ver información detallada sobre cada tema haciendo clic en **Más información** para ver una descripción detallada del problema, las acciones recomendadas, los vínculos a documentos útiles, las métricas relacionadas y los datos de registro. Los informes de diagnóstico se generan de forma inteligente en función del estado actual del clúster después de ejecutar diversas comprobaciones. Los informes de diagnóstico pueden ser una herramienta útil para identificar el problema del clúster y encontrar los pasos siguientes para resolver el problema.

![Informe de diagnóstico](./media/concepts-diagnostics/diagnostic-report.png)

![Informe de diagnóstico expandido](./media/concepts-diagnostics/node-issues.png)

## <a name="cluster-insights"></a>Información de clúster

Las siguientes comprobaciones de diagnóstico están disponibles en **Información de clúster**.

### <a name="cluster-node-issues"></a>Problemas del nodo de clúster

Problemas del nodo de clúster comprueba la existencia de problemas relacionados con el nodo que pueden hacer que el clúster se comporte de forma inesperada.

- Problemas de preparación del nodo
- Errores del nodo
- No hay suficientes recursos
- El nodo no cuenta con una dirección IP configurada
- Errores de CNI del nodo
- Nodo no encontrado
- Nodo apagado
- Error de autenticación del nodo
- Proxy de kube del nodo obsoleto

### <a name="create-read-update--delete-operations"></a>Operaciones de creación, lectura, actualización y eliminación (CRUD)

Las operaciones de CRUD comprueban si alguna de estas operaciones podrían causar problemas en su clúster.

- Error de operación de eliminación de subred en uso
- Estado de operación de eliminación del grupo de seguridad de red
- Error de operación de eliminación de la tabla de rutas en uso
- Error de aprovisionamiento de recursos a los que se hace referencia
- Error de operación de eliminación de dirección IP pública
- Error de implementación debido a la cuota de implementación
- Error de operación debido a la directiva de la organización
- Falta de registro de suscripción
- Error de aprovisionamiento de la extensión de máquina virtual
- Capacidad de subred
- Cuota de errores superada

### <a name="identity-and-security-management"></a>Administración de identidades y seguridad

La administración de identidades y seguridad detecta errores de autenticación y autorización que pueden impedir la comunicación con el clúster.

- Errores de autorización de nodo
- 401 errores
- 403 errores

## <a name="next-steps"></a>Pasos siguientes

Recopile registros para ayudarle a solucionar los problemas de los clústeres mediante el uso de [AKS Periscope](https://aka.ms/aksperiscope).

Lea la sección [Prácticas de evaluación de prioridades](https://docs.microsoft.com/azure/architecture/operator-guides/aks/aks-triage-practices) de la guía de operaciones del día 2 de AKS.

Publique sus preguntas o comentarios en [UserVoice](https://feedback.azure.com/forums/914020-azure-kubernetes-service-aks) agregando "[Diag]" en el título.
