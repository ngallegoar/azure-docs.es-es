---
title: Los clústeres de Azure HDInsight con cifrado de disco pierden el acceso a Key Vault
description: Pasos de solución de problemas y posibles soluciones para los problemas de acceso de Key Vault que se producen al interactuar con clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.openlocfilehash: 1e34c1002be3dffb719490fee01e481e8df45901
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90532569"
---
# <a name="scenario-azure-hdinsight-clusters-with-disk-encryption-lose-key-vault-access"></a>Escenario: Los clústeres de Azure HDInsight con cifrado de disco pierden el acceso a Key Vault

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

La alerta de Resource Health Center (RHC), `The HDInsight cluster is unable to access the key for BYOK encryption at rest`, se muestra para los clústeres de Bring Your Own Key (BYOK) en los que los nodos del clúster han perdido el acceso a Key Vault (KV) de los clientes. También se pueden ver alertas similares en la interfaz de usuario de Apache Ambari.

## <a name="cause"></a>Causa

La alerta asegura que Key Vault es accesible desde los nodos del clúster, lo que garantiza la conexión de red, el estado de Key Vault y la directiva de acceso para la identidad administrada asignada por el usuario. Esta alerta solo es una advertencia del apagado inminente del agente en reinicios posteriores del nodo; el clúster sigue funcionando hasta que se reinicien los nodos.

Vaya a la interfaz de usuario de Apache Ambari para más información acerca de la alerta en **Estado de Key Vault del cifrado de disco**. Esta alerta tendrá detalles sobre el motivo del error de comprobación.

## <a name="resolution"></a>Solución

### <a name="kvaad-outage"></a>Interrupción de Key Vault/AAD

Consulte [Disponibilidad y redundancia de Azure Key Vault](../../key-vault/general/disaster-recovery-guidance.md) y la página de estado de Azure https://status.azure.com/ para más detalles.

### <a name="kv-accidental-deletion"></a>Eliminación por error de Key Vault

* Restaure la clave eliminada en Key Vault para recuperarla automáticamente. Para más información, consulte [Recuperación de una clave eliminada](https://docs.microsoft.com/rest/api/keyvault/recoverdeletedkey).
* Póngase en contacto con el equipo de Key Vault para recuperar las eliminaciones accidentales.

### <a name="kv-access-policy-changed"></a>La directiva de acceso de Key Vault cambió

Restaure las directivas de acceso para la identidad administrada asignada por el usuario al clúster de HDI para acceder a Key Vault.

### <a name="key-permitted-operations"></a>Operaciones con clave permitidas

Para cada clave de Key Vault, puede elegir el conjunto de operaciones permitidas. Asegúrese de que ha habilitado las operaciones de encapsulado y desencapsulado para la clave BYOK

### <a name="expired-key"></a>Clave expirada

Si la expiración se ha superado y la clave no se ha rotado, restaure la clave desde el equipo de Key Vault de contacto o de HSM de copia de seguridad para borrar la fecha de expiración.

### <a name="kv-firewall-blocking-access"></a>El firewall de Key Vault bloquea el acceso

Corrija la configuración del firewall de Key Vault para permitir que los nodos del clúster de BYOK tengan acceso a Key Vault.

### <a name="nsg-rules-on-virtual-network-blocking-access"></a>Reglas de NSG en la red virtual que bloquean el acceso

Compruebe las reglas de NSG asociadas a la red virtual conectada al clúster.

## <a name="mitigation-and-prevention-steps"></a>Pasos de mitigación y prevención

### <a name="kv-accidental-deletion"></a>Eliminación por error de Key Vault

* Configure Key Vault con el [conjunto de bloqueos de recursos](../../azure-resource-manager/management/lock-resources.md).
* Realice una copia de seguridad de las claves en su módulo de seguridad de hardware.

### <a name="key-deletion"></a>Eliminación de la clave

El clúster debe eliminarse antes de la eliminación de la clave.

### <a name="kv-access-policy-changed"></a>La directiva de acceso de Key Vault cambió

Audite y pruebe periódicamente las directivas de acceso.

### <a name="expired-key"></a>Clave expirada

* Realice una copia de seguridad de las claves en el HSM.
* Use una clave sin ninguna fecha de expiración establecida.
* Si es necesario establecer la expiración, rote las claves antes de la fecha de expiración.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
