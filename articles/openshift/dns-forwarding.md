---
title: Configuración del reenvío de DNS para Red Hat OpenShift en Azure 4
description: Configuración del reenvío de DNS para Red Hat OpenShift en Azure 4
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 6784c69c049945f3bd4977ecd647d57ec59bf9f1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82232639"
---
# <a name="configure-dns-forwarding-on-an-azure-red-hat-openshift-4-cluster"></a>Configuración del reenvío de DNS en un clúster de Red Hat OpenShift en Azure 4

Para configurar el reenvío de DNS en un clúster de Red Hat OpenShift en Azure, deberá modificar el operador de DNS. Esta modificación permitirá que los pods de aplicación que se ejecutan dentro del clúster resuelvan los nombres hospedados en un servidor DNS privado fuera de dicho clúster. Estos pasos se documentaron para OpenShift 4.3 [en este vínculo](https://docs.openshift.com/container-platform/4.3/networking/dns-operator.html).

Por ejemplo, si quiere reenviar todas las solicitudes DNS para que un servidor DNS 192.168.100.10 resuelva a *.example.com, puede editar la configuración del operador al ejecutar el siguiente comando:
 
```bash
oc edit dns.operator/default
```
 
Se iniciará un editor y podrá reemplazar `spec: {}` por:
 
 ```yaml
spec:
  servers:
  - forwardPlugin:
      upstreams:
      - 192.168.100.10
    name: example-dns
    zones:
    - example.com
```

Guarde el archivo y salga de su editor.

## <a name="next-steps"></a>Pasos siguientes
Consulte más información sobre el reenvío de DNS para OpenShift 4.3 en [este vínculo](https://docs.openshift.com/container-platform/4.3/networking/dns-operator.html).