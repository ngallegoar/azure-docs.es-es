---
title: Opciones de seguridad para Hive en Azure HDInsight
description: Opciones de seguridad para Hive en clústeres estándar y ESP.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/02/2020
ms.openlocfilehash: f8296018a9534d5e2ce5dd7b84ebcad49cac00b3
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96460023"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Opciones de seguridad para Hive en Azure HDInsight

En este documento se describen las opciones de seguridad recomendadas para Hive en HDInsight. Estas opciones se pueden configurar a través de Ambari.

!["Opciones de seguridad para Hive"](./media/hdinsight-security-options-for-hive/security-options-hive.png "Opciones de seguridad para Hive")

## <a name="hiveserver2-authentication"></a>Autenticación de HiveServer2

En el caso de los clústeres estándar, el valor recomendado para la autenticación de HiveServer2 es el valor predeterminado, que es Ninguno. Para habilitar la autenticación, se recomienda actualizar a un clúster [ESP](../domain-joined/hdinsight-security-overview.md) (Enterprise Security Package). 

En los clústeres ESP, la autenticación [Kerberos](https://web.mit.edu/Kerberos/) está habilitada de manera predeterminada. No se admiten los módulos Pluggable Authentication Modules (PAM) ni los esquemas de autenticación personalizados.

## <a name="hiveserver2-authorization"></a>Autorización de HiveServer2

En el caso de los clústeres estándar, el valor predeterminado es Ninguno. [Se puede habilitar SqlStdAuth (autorización basada en estándares de SQL)](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization). No se admite la autorización a través de [Apache Ranger](https://ranger.apache.org/) para los clústeres estándar. Se recomienda actualizar a un clúster ESP para la autorización de Ranger. 

En el caso de los clústeres ESP, la autorización a través de Ranger está habilitada de manera predeterminada. 


## <a name="ssl-encryption-for-hiveserver2"></a>Cifrado SSL para HiveServer2

No se recomienda habilitar SSL de Hiveserver2 para los clústeres estándar o ESP. En su lugar, SSL se habilita en la puerta de enlace. El [cifrado en tránsito](../domain-joined/encryption-in-transit.md) se puede habilitar para cifrar las comunicaciones entre los nodos del clúster mediante el [protocolo de seguridad de Internet (IPSec)](https://en.wikipedia.org/wiki/IPsec).


## <a name="next-steps"></a>Pasos siguientes
* [Información general de la autenticación de HiveServer2](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [Información general de la autorización de HiveServer2](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization)
* [Habilitación de la autorización de Hive basada en estándares SQL](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [Apache Ranger con Hive](../domain-joined/apache-domain-joined-run-hive.md)
