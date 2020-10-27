---
title: Administración de restricciones del contexto de seguridad en Azure Red Hat OpenShift | Microsoft Docs
description: Restricciones del contexto de seguridad para administradores de clústeres de Azure Red Hat OpenShift
services: container-service
author: troy0820
ms.author: b-trconn
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: eb5c568f056a99187a0e7a78a6f89b206f2d8dec
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220251"
---
# <a name="manage-security-context-constraints-in-azure-red-hat-openshift"></a>Administración de restricciones del contexto de seguridad en Azure Red Hat OpenShift

> [!IMPORTANT]
> Red Hat OpenShift en Azure 3.11 se retirará el 30 de junio de 2022. La compatibilidad con la creación de nuevos clústeres de Red Hat OpenShift en Azure 3.11 continúa hasta el 30 de noviembre de 2020. Después de la retirada, los clústeres de Red Hat OpenShift en Azure 3.11 que queden se cerrarán para evitar vulnerabilidades de seguridad.
> 
> Siga esta guía para [crear un clúster de la versión 4 de Red Hat OpenShift en Azure](tutorial-create-cluster.md).
> Si tiene alguna pregunta específica, póngase en [contacto con nosotros](mailto:arofeedback@microsoft.com).

Las restricciones del contexto de seguridad (SCC) permiten a los administradores de clústeres controlar los permisos para los pods. Para más información sobre este tipo de API, consulte la [documentación sobre la arquitectura de las SCC](https://docs.openshift.com/container-platform/3.11/architecture/additional_concepts/authorization.html). Puede administrar las SCC en su instancia como objetos de API normales mediante la CLI.

## <a name="list-security-context-constraints"></a>Enumeración de restricciones del contexto de seguridad

Para obtener una lista actual de SCC, use este comando: 

```bash
$ oc get scc

NAME               PRIV      CAPS      SELINUX     RUNASUSER          FSGROUP     SUPGROUP    PRIORITY   READONLYROOTFS   VOLUMES
anyuid             false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    10         false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
hostaccess         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath persistentVolumeClaim secret]
hostmount-anyuid   false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath nfs persistentVolumeClaim secret]
hostnetwork        false     []        MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
nonroot            false     []        MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
privileged         true      [*]       RunAsAny    RunAsAny           RunAsAny    RunAsAny    <none>     false            [*]
restricted         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
```

## <a name="examine-an-object-for-security-context-constraints"></a>Examen de un objeto de restricciones del contexto de seguridad

Para examinar un SCC determinado, use `oc get`, `oc describe` o `oc edit`.  Por ejemplo, para examinar el SCC **restricted** , use este comando:
```bash
$ oc describe scc restricted
Name:                    restricted
Priority:                <none>
Access:
  Users:                <none>
  Groups:                system:authenticated
Settings:
  Allow Privileged:            false
  Default Add Capabilities:        <none>
  Required Drop Capabilities:        KILL,MKNOD,SYS_CHROOT,SETUID,SETGID
  Allowed Capabilities:            <none>
  Allowed Seccomp Profiles:        <none>
  Allowed Volume Types:            configMap,downwardAPI,emptyDir,persistentVolumeClaim,projected,secret
  Allow Host Network:            false
  Allow Host Ports:            false
  Allow Host PID:            false
  Allow Host IPC:            false
  Read Only Root Filesystem:        false
  Run As User Strategy: MustRunAsRange
    UID:                <none>
    UID Range Min:            <none>
    UID Range Max:            <none>
  SELinux Context Strategy: MustRunAs
    User:                <none>
    Role:                <none>
    Type:                <none>
    Level:                <none>
  FSGroup Strategy: MustRunAs
    Ranges:                <none>
  Supplemental Groups Strategy: RunAsAny
    Ranges:                <none>
```
## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md) 
