---
title: 'Procedimientos recomendados para desarrolladores: seguridad de pods en Azure Kubernetes Service (AKS)'
description: Obtenga información acerca de los procedimientos recomendados para desarrolladores para la protección de pods en Azure Kubernetes Services (AKS)
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: zarhoads
ms.openlocfilehash: 2ee41770f9993da381215f03ebf67aea3cc7dd9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475585"
---
# <a name="best-practices-for-pod-security-in-azure-kubernetes-service-aks"></a>Procedimientos recomendados para la seguridad de pods en Azure Kubernetes Service (AKS)

A la hora de desarrollar y ejecutar aplicaciones en Azure Kubernetes Service (AKS), es importante tener en cuenta la seguridad de los pods. Las aplicaciones se deben diseñar con el principio del número mínimo de privilegios necesarios. Mantener seguros los datos privados es prioritario para los clientes. No deseará que credenciales como las cadenas de conexión de base de datos, las claves o los secretos y los certificados se vean expuestas al mundo exterior, donde un atacante podría aprovechar esos secretos con propósitos malintencionados. No debe incorporarlas al código ni insertarlas en las imágenes de contenedor. Este enfoque crearía un riesgo de exposición y limitaría la posibilidad de rotar esas credenciales, ya que las imágenes de contenedor se deberían volver a generar.

Este artículo de procedimientos recomendados se centra en cómo proteger los pods en AKS. Aprenderá a:

> [!div class="checklist"]
> * Usar el contexto de seguridad del pod para limitar el acceso a los procesos y los servicios o una elevación de privilegios
> * Autenticarse con otros recursos de Azure mediante identidades administradas del pod
> * Solicitar y recuperar credenciales de un almacén digital, como Azure Key Vault

También puede consultar los procedimientos recomendados sobre la [seguridad del clúster][best-practices-cluster-security] y la [administración de imágenes de contenedor][best-practices-container-image-management].

## <a name="secure-pod-access-to-resources"></a>Protección del acceso del pod a los recursos

**Guía de procedimiento recomendado**: para la ejecución como un usuario o grupo diferentes y limitar el acceso a los procesos y servicios del nodo subyacente, defina la configuración del contexto de seguridad del pod. Asigne el menor número de privilegios necesarios.

Para que las aplicaciones se ejecuten correctamente, los pods se deben ejecutar como un usuario o grupo definidos y no como *root*. El elemento `securityContext` de un pod o un contenedor permite definir valores de configuración como *runAsUser* o *fsGroup* para asumir los permisos adecuados. Asigne solo los permisos de usuario o grupo necesarios y no utilice el contexto de seguridad como un medio para asumir permisos adicionales. El *runAsUser*, elevación de privilegios y otros valores de configuración de las capacidades de Linux solo están disponibles en los pods y los nodos de Linux.

Cuando se ejecuta como un usuario que no es root, los contenedores no se pueden enlazar a los puertos con privilegios inferiores al 1024. En este escenario, los servicios de Kubernetes se pueden utilizar para ocultar el hecho de que una aplicación se ejecuta en un puerto determinado.

Un contexto de seguridad del pod también puede definir funcionalidades o permisos adicionales para el acceso a los procesos y servicios. Se pueden establecer las siguientes definiciones de contexto de seguridad comunes:

* **allowPrivilegeEscalation** define si el pod puede asumir privilegios de usuario *root*. Diseñe las aplicaciones de modo que este valor siempre esté establecido en *false*.
* **Funcionalidades de Linux**: permite el acceso del pod a los procesos de nodo subyacente. Tenga cuidado con la asignación de estas funcionalidades. Asigne el menor número de privilegios necesarios. Para más información, consulte las [funcionalidades de Linux][linux-capabilities].
* **Etiquetas SELinux** es un módulo de seguridad del kernel de Linux que le permite definir directivas para el acceso a los servicios, los procesos y el sistema de archivos. De nuevo, asigne el menor número de privilegios necesarios. Para más información, consulte las [opciones de SELinux disponibles en Kubernetes][selinux-labels].

El siguiente ejemplo de manifiesto YAML de pod establece la configuración del contexto de seguridad para definir:

* El pod se ejecuta con el identificador de usuario *1000* y es parte del grupo con el identificador *2000*
* No se pueden elevar los privilegios para usar `root`
* Permite a las funcionalidades de Linux acceder a las interfaces de red y al reloj en tiempo real del host (hardware)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  containers:
    - name: security-context-demo
      image: nginx:1.15.5
    securityContext:
      runAsUser: 1000
      fsGroup: 2000
      allowPrivilegeEscalation: false
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```

Colabore con el operador del clúster para determinar la configuración de contexto de seguridad que necesita. Intente diseñar las aplicaciones para minimizar los permisos adicionales y el acceso que requiere el pod. Hay características de seguridad adicionales para limitar el acceso mediante AppArmor y seccomp (informática segura) que los operadores del clúster pueden implementar. Para más información, consulte [Protección del acceso del contenedor a los recursos][apparmor-seccomp].

## <a name="limit-credential-exposure"></a>Limitación de la exposición de credenciales

**Guía de procedimiento recomendado**: no defina las credenciales en el código de aplicación. Use identidades administradas para los recursos de Azure para permitir que el pod solicite acceso a otros recursos. Se debería usar también un almacén digital, como Azure Key Vault, para almacenar y recuperar las credenciales y claves digitales. Las identidades administradas de pod está pensadas para usar únicamente con pods de Linux y las imágenes de contenedor.

Para limitar el riesgo de exposición de las credenciales en el código de la aplicación, evite el uso de credenciales compartidas o fijas. Las credenciales y las claves no se deberían incluir directamente en el código. Si se exponen estas credenciales, la aplicación se debe actualizar y volver a implementar. Un enfoque mejor es dar a los pods sus propias identidades y formas para autenticarse a sí mismos o recuperar automáticamente las credenciales de un almacén digital.

Los siguientes [proyectos de código abierto de AKS asociados][aks-associated-projects] le permiten autenticar automáticamente los pods o las credenciales de la solicitud y las claves desde un almacén digital:

* identidades administradas para los recursos de Azure y
* el controlador FlexVol de Azure Key Vault

El soporte técnico de Azure no admite los proyectos de código abierto de AKS asociados. Se proporcionan para recopilar comentarios y errores de nuestra comunidad. Estos proyectos no se recomienda para su uso en producción.

### <a name="use-pod-managed-identities"></a>Uso de identidades administradas del pod

Las identidades administradas de los recursos de Azure permiten a un pod autenticarse a sí mismo en cualquier servicio de Azure que lo permita, como Storage o SQL. Se asigna al pod una identidad de Azure que le permite autenticarse en Azure Active Directory y recibir un token digital. Este token digital se puede presentar a otros servicios de Azure que comprueban si el pod está autorizado para acceder al servicio y realizar las acciones necesarias. Este enfoque significa que no es necesario ningún secreto para las cadenas de conexión de base de datos, por ejemplo. El flujo de trabajo simplificado de la identidad administrada del pod se muestra en el diagrama siguiente:

![Flujo de trabajo simplificado de la identidad administrada del pod en Azure](media/developer-best-practices-pod-security/basic-pod-identity.png)

Con una identidad administrada, el código de la aplicación no necesita incluir credenciales para acceder a un servicio como Azure Storage. Como cada pod se autentica con su propia identidad, puede auditar y revisar el acceso. Si la aplicación se conecta con otros servicios de Azure, utilice identidades administradas para limitar la reutilización de credenciales y el riesgo de exposición.

Para más información acerca de las identidades de los pods, consulte [Configuración de un clúster de AKS para usar identidades administradas de pods con las aplicaciones][aad-pod-identity].

### <a name="use-azure-key-vault-with-flexvol"></a>Uso de Azure Key Vault con FlexVol

Las identidades administradas de pods funcionan muy bien para autenticarse en los servicios de Azure compatibles. Para sus propios servicios o aplicaciones sin identidades administradas para los recursos de Azure, aún se autentica mediante credenciales o claves. Se puede utilizar un almacén digital para almacenar estas credenciales.

Cuando las aplicaciones necesitan una credencial, se comunican con el almacén digital, recuperan las credenciales más recientes y, a continuación, se conectan con el servicio solicitado. Azure Key Vault puede ser este almacén digital. El flujo de trabajo simplificado para recuperar una credencial de Azure Key Vault mediante identidades administradas de pods se muestra en el diagrama siguiente:

![Flujo de trabajo simplificado para recuperar una credencial de Key Vault mediante una identidad administrada de pods](media/developer-best-practices-pod-security/basic-key-vault-flexvol.png)

Con Key Vault, puede almacenar y rotar periódicamente secretos como credenciales, claves de cuenta de almacenamiento o certificados. Puede integrar Azure Key Vault con un clúster de AKS mediante un FlexVolume. El controlador FlexVolume permite que el clúster de AKS pueda recuperar de forma nativa las credenciales de Key Vault y proporcionárselas de forma segura solo al pod solicitante. Colabore con el operador del clúster para implementar el controlador FlexVol de Key Vault en los nodos de AKS. Puede usar una identidad administrada de pods para solicitar acceso a Key Vault y recuperar las credenciales que necesita mediante el controlador FlexVolume.

Azure Key Vault con FlexVol está pensado para su uso con aplicaciones y servicios que se ejecutan en nodos y pods de Linux.

## <a name="next-steps"></a>Pasos siguientes

Este artículo se centra en cómo proteger los pods. Para implementar algunas de estas áreas, consulte los artículos siguientes:

* [Uso de identidades administradas para recursos de Azure con AKS][aad-pod-identity]
* [Integración de Azure Key Vault con AKS][aks-keyvault-flexvol]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity#demo
[aks-keyvault-flexvol]: https://github.com/Azure/kubernetes-keyvault-flexvol
[linux-capabilities]: http://man7.org/linux/man-pages/man7/capabilities.7.html
[selinux-labels]: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#selinuxoptions-v1-core
[aks-associated-projects]: https://github.com/Azure/AKS/blob/master/previews.md#associated-projects

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[aks-pod-identities]: operator-best-practices-identity.md#use-pod-identities
[apparmor-seccomp]: operator-best-practices-cluster-security.md#secure-container-access-to-resources
