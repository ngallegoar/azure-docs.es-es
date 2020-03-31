---
title: Versiones de Kubernetes compatibles en Azure Kubernetes Service
description: Obtener información sobre la directiva de soporte técnico de la versión de Kubernetes y el ciclo de vida de los clústeres en Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.topic: article
ms.date: 12/09/2019
ms.author: saudas
ms.openlocfilehash: 77d64391888957a1697a5823a1485413686682d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593451"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)

La Comunidad de Kubernetes libera versiones secundarias aproximadamente cada tres meses. Estas versiones incluyen nuevas características y mejoras. Versiones de revisión son más frecuentes (a veces semanales) y solo están previstas para correcciones de errores críticos en una versión secundaria. Estas versiones de revisión incluyen correcciones de vulnerabilidades de seguridad o errores importantes que afectan a un gran número de clientes y productos que se ejecutan en producción basada en Kubernetes.

AKS tiene como objetivo certificar y publicar nuevas versiones de Kubernetes dentro de los 30 días posteriores a la versión anterior, con sujeción a la estabilidad de la versión.

## <a name="kubernetes-versions"></a>Versiones de Kubernetes

Kubernetes usa el esquema de versiones estándar de [Versionamiento Semántico](https://semver.org/). Esto significa que cada versión de Kubernetes sigue este esquema de numeración:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
```

Cada número en la versión indica compatibilidad general con la versión anterior:

* Las versiones principales cambian cuando cambia una API incompatible o la compatibilidad con versiones anteriores deja de funcionar.
* Las versiones secundarias cambian cuando se hacen cambios de funcionalidad que son compatibles con otras versiones anteriores secundarias.
* Las versiones de revisión cambian cuando se hacen correcciones de errores compatibles con versiones anteriores.

Los usuarios deben quieren ejecutar la última versión de revisión de la versión secundaria que están ejecutando; por ejemplo, si su clúster de producción usa la versión *1.12.14* y *1.12.15* es la última versión de revisión disponible para la serie *1.12*, debe actualizar a *1.12.15* en cuanto pueda asegurarse de que el clúster esté completamente revisado y sea compatible.

## <a name="kubernetes-version-support-policy"></a>Directiva de soporte técnico de versión de Kubernetes

AKS es compatible con tres versiones secundarias de Kubernetes:

* La versión secundaria actual que está publicada en AKS (N)
* Dos versiones secundarias anteriores. Cada versión secundaria compatible también admite dos revisiones estables.

Esto se conoce como "N-2": (N [versión más reciente] - 2 [versiones secundarias]).

Por ejemplo, si AKS presenta *1.15.a* hoy, también se proporciona compatibilidad para las versiones siguientes:

Nueva versión secundaria    |    Lista de versiones admitidas
-----------------    |    ----------------------
1.15.a               |    1.15.a, 1.15.b, 1.14.c, 1.14.d, 1.13.e y 1.13.f

Donde ".letter" es representativo de las versiones de revisión.

Para obtener más información sobre las comunicaciones con respecto a los cambios de versión y las expectativas, consulte "Comunicaciones" a continuación.

Cuando se introduce una nueva versión secundaria, la versión secundaria y la versión de revisión compatibles más antiguas quedan obsoletas y se retiran. Por ejemplo, si la lista de versiones que se admiten actualmente es:

```
1.15.a
1.15.b
1.14.c
1.14.d
1.13.e
1.13.f
```

Y la versión 1.16. *de AKS; esto significa que las versiones 1.13.* (todas las versiones 1.13) se quitarán y dejarán de ser compatibles.

> [!NOTE]
> Tenga en cuenta que si los clientes ejecutan una versión incompatible de Kubernetes, se les pedirá que actualicen al solicitar soporte técnico para el clúster. Los clústeres que ejecutan versiones de Kubernetes no admitidas no están cubiertos por el [las directivas de soporte técnico de AKS](https://docs.microsoft.com/azure/aks/support-policies).

Además de lo anterior sobre las versiones secundarias, AKS admite las dos últimas versiones de **revisión** de una determinada versión secundaria. Por ejemplo, dadas las siguientes versiones admitidas:

```
Current Supported Version List
------------------------------
1.15.2, 1.15.1, 1.14.5, 1.14.4
```

Si anteriormente Kubernetes publicó 1.15.3 y 1.14.6 y AKS publica esas versiones de revisión, las versiones de revisión más antiguas quedan en desuso y se eliminan, y la lista de versiones admitidas pasa a ser:

```
New Supported Version List
----------------------
1.15.*3*, 1.15.*2*, 1.14.*6*, 1.14.*5*
```

### <a name="communications"></a>Comunicaciones

* Para nuevas versiones **secundarias** de Kubernetes
  * Todos los usuarios son notificados públicamente de la nueva versión y de la versión que se eliminará.
  * Cuando se publica una nueva versión de revisión, la versión de revisión más antigua se quita al mismo tiempo.
  * Los clientes tienen **30 días** desde la fecha de notificación pública para actualizar a una versión secundaria compatible.
* Para nuevas versiones de **revisión** de Kubernetes
  * Se notifica a todos los usuarios la publicación de la nueva versión de revisión y la necesidad de actualizar a la versión de revisión más reciente.
  * Los usuarios tienen **30 días** para actualizar a una versión de revisión más reciente compatible antes de se quite la más antigua.

AKS define una "versión publicada" como aquellas disponibles con carácter general, habilitadas en todas las mediciones de calidad de servicio/SLO y disponible en todas las regiones. AKS también puede admitir versiones preliminares que se etiquetan explícitamente y están sujetas a los términos y condiciones de la versión preliminar.

#### <a name="notification-channels-for-aks-changes"></a>Canales de notificación para los cambios de AKS

AKS publica actualizaciones de servicio periódicas que resumen las nuevas versiones de Kubernetes, los cambios de servicio y las actualizaciones de componentes que se han lanzado en el servicio en [GitHub](https://github.com/Azure/AKS/releases).

Estos cambios se aplican a todos los clientes dentro del mantenimiento periódico que se ofrece como parte del servicio administrado; algunos requieren actualizaciones explícitas, mientras que otros no requieren ninguna acción.

También se envían notificaciones a través de:

* [Notas de la versión de AKS](https://aka.ms/aks/releasenotes)
* Notificaciones de Azure Portal
* [Canal de actualización de Azure][azure-update-channel]

### <a name="supported-versions-policy-exceptions"></a>Excepciones de directiva de versiones admitidas

AKS se reserva el derecho de agregar o eliminar versiones nuevas/existentes donde se hayan identificado uno o varios problemas de seguridad o errores críticos que afecten a la producción sin previo aviso.

Las versiones de revisión específicas se pueden omitir, o su implantación puede verse acelerada en función de la gravedad del error o el problema de seguridad.

### <a name="azure-portal-and-cli-default-versions"></a>Versiones predeterminadas de Azure Portal y la CLI de Azure

Al implementar un clúster de AKS en Azure Portal o con la CLI de Azure, el clúster siempre se establece de forma predeterminada en la versión secundaria n-1 y la revisión más reciente. Por ejemplo, si AKS admite *1.15.a*, *1.15.b*, *1.14.c*, *1.14.d*, *1.13.e* y *1.13.f*, la versión predeterminada seleccionada es la *1.14.c*.

AKS elige la versión predeterminada de N-1 para proporcionar a los clientes una versión conocida, estable y revisada de forma predeterminada.

## <a name="list-currently-supported-versions"></a>Lista de versiones admitidas actualmente

Para averiguar qué versiones están disponibles actualmente para su suscripción y región, utilice el comando [az aks get-versions][az-aks-get-versions]. En el ejemplo siguiente se enumeran las versiones de Kubernetes disponibles para la región *EastUS*:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

## <a name="faq"></a>Preguntas más frecuentes

**¿Qué ocurre cuando un cliente actualiza un clúster de Kubernetes con una versión secundaria que no es compatible?**

Si la suya es la versión *n-3*, se encuentra fuera de soporte técnico y se le pedirá que actualice. Si su actualización de la versión n-3 a n-2 se completa correctamente, estará dentro de nuestras directivas de soporte técnico. Por ejemplo:

- Si la versión de AKS admitida más antigua es la*1.13.a* y la suya es la *1.12.b* o anterior, no es compatible.
- Si la actualización des *1.12.b* a *1.13.a* se realiza correctamente, estará de nuevo dentro de nuestras directivas de soporte técnico.

No se admiten las actualizaciones de versiones anteriores a la ventana admitida de *N-2*. En esos casos, se recomienda a los clientes que creen clústeres de AKS y vuelvan a implementar sus cargas de trabajo con versiones en la ventana admitida.

**¿Qué significa "fuera de soporte técnico"?**

"Fuera de soporte técnico" significa que la versión que está ejecutando está fuera de la lista de versiones admitidas, y se le pedirá que actualice el clúster a una versión compatible cuando solicite soporte técnico. Además, AKS no ofrece ningún entorno de ejecución ni otras garantías para los clústeres que estén fuera de la lista de versiones admitidas.

**¿Qué ocurre cuando un cliente escala un clúster de Kubernetes con una versión secundaria que no es compatible?**

En el caso de las versiones secundarias que no admite AKS, el escalado horizontal o vertical debería seguir funcionando, pero se recomienda encarecidamente actualizar para que el clúster vuelva a ser compatible.

**¿Puede un cliente permanecer en una versión de Kubernetes para siempre?**

Sí. No obstante, si el clúster no está en una de las versiones compatibles con AKS, el clúster está fuera de las directivas de soporte técnico de AKS. Azure no actualiza el clúster automáticamente ni lo elimina.

**¿Qué versión admite el plano de control si el grupo de nodos no está en una de las versiones de AKS compatibles?**

El plano de control debe estar dentro de una ventana de versiones de todos los grupos de nodos. Para obtener más información sobre la actualización del plano de control o de los grupos de nodos, visite la documentación sobre [cómo actualizar grupos de nodos](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo actualizar el clúster, vea [Actualización de un clúster de Azure Kubernetes Service (AKS)][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
