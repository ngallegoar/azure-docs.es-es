---
title: 'Inicio rápido: aplicación empresarial JBoss (EAP) en Red Hat Enterprise Linux (RHEL) en una máquina virtual de Azure y un conjunto de escalado de máquinas virtuales'
description: Cómo implementar aplicaciones empresariales Java con Red Hat JBoss EAP en una máquina virtual de Azure RHEL y un conjunto de escalado de máquinas virtuales.
author: theresa-nguyen
ms.author: bicnguy
ms.topic: quickstart
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.assetid: 8a4df7bf-be49-4198-800e-db381cda98f5
ms.date: 10/30/2020
ms.openlocfilehash: d97148393d3158e38f9740d4a8f8e17dd04326d5
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93135113"
---
# <a name="deploy-enterprise-java-applications-to-azure-with-jboss-eap-on-red-hat-enterprise-linux"></a>Implementación de aplicaciones empresariales Java en Azure con JBoss EAP en Red Hat Enterprise Linux

Estas plantillas de inicio rápido le mostrarán cómo implementar [JBoss EAP](https://www.redhat.com/en/technologies/jboss-middleware/application-platform) con [RHEL](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) en máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales. Dispondrá de una aplicación Java de ejemplo en la implementación para validar la implementación. JBoss EAP es una plataforma de servidor de aplicaciones de código abierto. Ofrece seguridad, escalabilidad y rendimiento de nivel empresarial para las aplicaciones Java. RHEL es una plataforma de sistema operativo de código abierto. Permite el escalado de aplicaciones existentes y la implementación de nuevas tecnologías en todos los entornos. JBoss EAP y RHEL incluyen todo lo necesario para compilar, ejecutar, implementar y administrar aplicaciones empresariales Java en cualquier entorno. Se trata de una excelente solución de código abierto tanto para entornos locales y virtuales como para nubes privadas, públicas o híbridas.

## <a name="prerequisite"></a>Requisito previo 

* Una cuenta de Azure con una suscripción activa. Para obtener una suscripción de Azure, active sus [créditos de Azure para suscriptores de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) o [cree una cuenta gratuita](https://azure.microsoft.com/pricing/free-trial).

* Instalación de JBoss EAP: debe tener una cuenta de Red Hat con el derecho de administración de suscripciones de Red Hat (RHSM) para el JBoss EAP. Este derecho le permitirá descargar la versión de JBoss EAP probada y certificada por Red Hat.  Si no tiene el derecho de EAP, obtenga una [suscripción de evaluación de JBoss EAP](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/evaluation) antes de empezar. Para crear una nueva suscripción de Red Hat, vaya al [portal de clientes de Red Hat](https://access.redhat.com/) y configure una cuenta.
F
* [Interfaz de la línea de comandos de Azure](https://docs.microsoft.com/cli/azure/overview).

* Opciones de RHEL: elija entre Pago por uso (PAYG) o Traiga su propia suscripción (BYOS). Con BYOS, debe activar su imagen Gold de RHEL de [Red Hat Cloud Access](https://access.redhat.com/) antes de implementar la plantilla de inicio rápido.

## <a name="java-ee--jakarata-ee-application-migration"></a>Migración de aplicaciones Java EE/Jakarta EE

### <a name="migrate-to-jboss-eap"></a>Migración a JBoss EAP
JBoss EAP 7.2 y 7.3 son implementaciones certificadas de las especificaciones Java Enterprise Edition (Java EE) 8 y Jakarta EE 8. JBoss EAP proporciona opciones preconfiguradas para características como clústeres de alta disponibilidad (HA), mensajería y almacenamiento en caché distribuido. También permite a los usuarios escribir, implementar y ejecutar aplicaciones con las distintas API y servicios que proporciona JBoss EAP.  Para obtener información adicional sobre JBoss EAP, visite [Introducción a JBoss EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html-single/introduction_to_jboss_eap/index) o [Introducción a JBoss EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/introduction_to_jboss_eap/index).

 #### <a name="applications-on-jboss-eap"></a>Aplicaciones en JBoss EAP

* Aplicaciones de servicios web: los servicios web proporcionan un medio estándar de interoperar entre diferentes aplicaciones de software. Cada aplicación puede ejecutarse en plataformas y marcos diferentes. Estos servicios web facilitan la comunicación de subsistemas internos y heterogéneos. Para más información, visite [Desarrollo de aplicaciones de servicios web en EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/developing_web_services_applications/index) o [Desarrollo de aplicaciones de servicios web en EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/developing_web_services_applications/index).

* Aplicaciones Enterprise Java Beans (EJB): EJB 3.2 es una API para el desarrollo de aplicaciones Java EE y Jakarta EE distribuidas, transaccionales, seguras y portables. EJB usa componentes del lado servidor denominados beans empresariales para implementar la lógica de negocios de una aplicación de una manera desacoplada que fomenta la reutilización. Para más información, visite [Desarrollo de aplicaciones EJB en EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/developing_ejb_applications/index) o [Desarrollo de aplicaciones EJB en EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/developing_ejb_applications/index).

* Aplicaciones Hibernate: los desarrolladores y administradores pueden desarrollar e implementar aplicaciones Java Persistence API (JPA)/Hibernate con JBoss EAP. Hibernate Core es un marco de asignación relacional de objetos para el lenguaje Java. Proporciona un marco para asignar un modelo de dominio orientado a objetos a una base de datos relacional, lo que permite a las aplicaciones evitar la interacción directa con la base de datos. El administrador de entidades de Hibernate implementa las interfaces de programación y las reglas del ciclo de vida tal y como se define en la [especificación de JPA 2.1](https://www.jcp.org/en/jsr/overview). Junto con las anotaciones de Hibernate, este contenedor implementa una solución JPA completa (e independiente) sobre la instancia madura de Hibernate Core. Para más información acerca de Hibernate, visite [JPA en EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/development_guide/java_persistence_api) o [Jakarta Persistence en EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/development_guide/java_persistence_api).

#### <a name="red-hat-migration-toolkit-for-applications-mta"></a>Kit de herramientas de migración para aplicaciones (MTA) de Red Hat
El [kit de herramientas de migración para aplicaciones (MTA) de Red Hat](https://developers.redhat.com/products/mta/overview) es una herramienta de migración para servidores de aplicaciones Java. Use esta herramienta para migrar desde otro servidor de aplicaciones a JBoss EAP. Funciona con complementos de IDE para [IDE de Eclipse](https://www.eclipse.org/ide/), [áreas de trabajo de Red Hat CodeReady](https://developers.redhat.com/products/codeready-workspaces/overview) y [Visual Studio Code (VS Code)](https://code.visualstudio.com/docs/languages/java) para Java.  MTA es una herramienta gratuita y de código abierto que hará lo siguiente:
* Automatizar el análisis de aplicaciones
* Admitir el cálculo del esfuerzo
* Acelerar la migración de código
* Admitir la contenedorización
* Integración con Azure Workload Builder

### <a name="migrate-jboss-eap-from-on-premises-to-azure"></a>Migración de JBoss EAP desde un entorno local a Azure
La oferta de Azure Marketplace de JBoss EAP en RHEL se instalará y aprovisionará en máquinas virtuales de Azure en menos de 20 minutos. Puede acceder a estas ofertas desde [Azure Marketplace](https://azuremarketplace.microsoft.com/).

Esta oferta de Marketplace incluye varias combinaciones de versiones de EAP y RHEL para satisfacer sus requisitos. JBoss EAP siempre es Traiga su propia suscripción (BYOS), pero para el sistema operativo RHEL puede elegir entre BYOS o Pago por uso (PAYG). La oferta de Azure Marketplace incluye opciones de planes para JBoss EAP en RHEL, como máquinas virtuales independientes o en clúster:

* JBoss EAP 7.2 en máquina virtual de RHEL 7.7 (PAYG)
* JBoss EAP 7.2 en máquina virtual de RHEL 8.0 (PAYG)
* JBoss EAP 7.3 en máquina virtual de RHEL 8.0 (PAYG)
* JBoss EAP 7.2 en máquina virtual de RHEL 7.7 (BYOS)
* JBoss EAP 7.2 en máquina virtual de RHEL 8.0 (BYOS)
* JBoss EAP 7.3 en máquina virtual de RHEL 8.0 (BYOS)

Junto con las ofertas de Azure Marketplace, hay plantillas de inicio rápido disponibles para empezar a trabajar en el recorrido de la migración de Azure. Estos inicios rápidos incluyen plantillas y scripts pregenerados de Azure Resource Manager (ARM) para implementar JBoss EAP en RHEL en varias configuraciones y combinaciones de versiones. Tendrá lo siguiente:

* Equilibrador de carga
* Dirección IP privada para el equilibrio de carga y las máquinas virtuales
* Red virtual con una sola subred
* Configuración de máquinas virtuales (en clúster o independientes)
* Una aplicación Java de ejemplo

La arquitectura de la solución para estas plantillas incluye:

* JBoss EAP en máquina virtual de RHEL independiente
* JBoss EAP en clúster en varias máquinas virtuales de RHEL
* JBoss EAP en clúster mediante un conjunto de escalado de máquinas virtuales

#### <a name="linux-workload-migration-for-jboss-eap"></a>Migración de la carga de trabajo de Linux para JBoss EAP
Azure Workload Builder simplificará el proceso de prueba de concepto, evaluación y migración para aplicaciones Java locales en Azure. Workload Builder se integra con la herramienta de detección de Azure Migrate para identificar servidores de JBoss EAP. A continuación, generará dinámicamente un cuaderno de estrategias de Ansible para la implementación de servidores de JBoss EAP. Utiliza la herramienta MTA de Red Hat para migrar servidores desde otros servidores de aplicaciones a JBoss EAP. Entre los pasos para simplificar la migración se incluyen los siguientes:
* Evaluación: clústeres de JBoss EAP que usan máquinas virtuales de Azure o un conjunto de escalado de máquinas virtuales
* Evaluación: análisis de las aplicaciones y la infraestructura
* Configuración de la infraestructura: creación de un perfil de carga de trabajo
* Implementación y pruebas: implementación, migración y pruebas de la carga de trabajo
* Configuración posterior a la implementación: integración con datos, supervisión, seguridad, copia de seguridad, etc.

## <a name="server-configuration-choice"></a>Opciones de configuración del servidor

Para la implementación de una máquina virtual de RHEL, puede elegir PAYG o BYOS. PAYG es la opción predeterminada de las imágenes de [Azure Marketplace](https://azuremarketplace.microsoft.com). Implemente una máquina virtual de RHEL de tipo BYOS si tiene su propia imagen de sistema operativo RHEL. Asegúrese de que su cuenta de RHSM tiene derecho de BYOS mediante Cloud Access F antes de implementar las máquinas virtuales o el conjunto de escalado de máquinas virtuales.

JBoss EAP tiene eficaces funcionalidades de administración, además de proporcionar funciones y API a sus aplicaciones. Estas funcionalidades de administración son distintas en función del modo de funcionamiento usado para iniciar JBoss EAP. Es compatible con RHEL y Windows Server. JBoss EAP ofrece un modo de funcionamiento de servidor independiente para la administración de instancias discretas. También ofrece un modo de funcionamiento de dominio administrado para administrar grupos de instancias desde un único punto de control. Nota: Los dominios administrados por JBoss EAP no son compatibles con Microsoft Azure debido a que la característica de alta disponibilidad (HA) está administrada por los servicios de infraestructura de Azure. La variable de entorno denominada *EAP_HOME* se usa para indicar la ruta de acceso a la instalación de JBoss EAP.

**Iniciar JBoss EAP como servidor independiente** : el siguiente comando permite iniciar el servicio EAP en modo independiente.

```
$EAP_HOME/bin/standalone.sh
```
    
Este script de inicio usa el archivo EAP_HOME/bin/Standalone.conf para establecer algunas preferencias predeterminadas, como las opciones de JVM. La configuración se puede personalizar en este archivo. JBoss EAP usa el archivo de configuración standalone.xml para iniciarse en modo independiente de forma predeterminada, pero se puede iniciar con otro modo. Para más información sobre los archivos de configuración de servidor independiente disponibles y cómo usarlos, consulte las secciones [Archivos de configuración de servidor independiente para EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuration_guide/jboss_eap_management#standalone_server_configuration_files) o [Archivos de configuración de servidor independiente para EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/jboss_eap_management#standalone_server_configuration_files). Para iniciar JBoss EAP con una configuración diferente, use el argumento --server-config. Por ejemplo:
    
 ```
 $EAP_HOME/bin/standalone.sh --server-config=standalone-full.xml
 ```
    
Para obtener una lista completa de todos los argumentos de script de inicio disponibles y sus propósitos, use el argumento --help o consulte las secciones [Argumentos de tiempo de ejecución de servidor en EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuration_guide/reference_material#reference_of_switches_and_arguments_to_pass_at_server_runtime) o [Argumentos de tiempo de ejecución de servidor en EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/reference_material#reference_of_switches_and_arguments_to_pass_at_server_runtime).
    
JBoss EAP también puede funcionar en modo de clúster. Consulte [Introducción a los clústeres en EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuring_messaging/clusters_overview) o [Introducción a los clústeres en EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuring_messaging/clusters_overview) para más información. La mensajería en clústeres de JBoss EAP permite la agrupación de servidores de mensajería de JBoss EAP para compartir la carga de procesamiento de mensajes. Cada nodo activo del clúster es un servidor de mensajería de JBoss EAP activo, que administra sus propios mensajes y controla sus propias conexiones.

## <a name="support-and-subscription-notes"></a>Compatibilidad y notas de suscripción
Estas plantillas de inicio rápido se ofrecen como se indica a continuación: 

- El sistema operativo RHEL se ofrece como PAYG o BYOS mediante el modelo de imagen de Red Hat Gold.
- JBoss EAP se ofrece solo como BYOS.

#### <a name="using-rhel-os-with-payg-model"></a>Uso del sistema operativo RHEL con el modelo PAYG

De forma predeterminada, estas plantillas de inicio rápido usan la imagen de PAYG de RHEL 7.7 u 8.0 a petición de la galería de Azure. Las imágenes de PAYG tendrán un cargo de suscripción de RHEL adicional por hora sobre los costos de proceso, red y almacenamiento normales. Al mismo tiempo, la instancia se registrará en su suscripción de Red Hat. Esto significa que utilizará uno de sus derechos. Esta imagen de PAYG dará lugar a una "doble facturación". Puede evitar este problema si crea su propia imagen de RHEL. Para más información, lea este artículo de Red Hat Knowledge Base (KB) sobre [cómo aprovisionar una máquina virtual de RHEL para Microsoft Azure](https://access.redhat.com/articles/uploading-rhel-image-to-azure). También puede activar su imagen Gold de RHEL de [Red Hat Cloud Access](https://access.redhat.com/).

Consulte los [precios de Red Hat Enterprise Linux](https://azure.microsoft.com/pricing/details/virtual-machines/red-hat/) para más información sobre los precios de máquinas virtuales con el modelo PAYG. Para usar RHEL en el modelo PAYG, necesitará una suscripción de Azure con el método de pago especificado para [RHEL 7.7 en Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux77-ARM) o [RHEL 8.0 en Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux80-ARM). Estas ofertas requieren que se especifique un método de pago en la suscripción de Azure.

#### <a name="using-rhel-os-with-byos-model"></a>Uso del sistema operativo RHEL con el modelo BYOS

Para usar el modelo BYOS para el sistema operativo de RHEL, debe tener una suscripción válida de Red Hat con derechos para usar el sistema operativo RHEL en Azure. Complete los siguientes requisitos previos antes de implementar esta plantilla de inicio rápido:

1. Asegúrese de que tiene los derechos del sistema operativo RHEL y JBoss EAP asignados en la suscripción de Red Hat.
2. Autorice su identificador de suscripción de Azure para usar imágenes de BYOS en RHEL. Siga la [documentación de administración de suscripciones de Red Hat (RHSM)](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs) para completar el proceso, que incluye estos pasos:

    2.1 Habilite Microsoft Azure como proveedor en el panel de Red Hat Cloud Access.

    2.2 Agregue sus identificadores de suscripción de Azure.

    2.3 Habilite nuevos productos para Cloud Access en Microsoft Azure.
    
    2.4 Active imágenes de Red Hat Gold para su suscripción de Azure. Para más información, lea el capítulo sobre cómo [habilitar y mantener suscripciones para Cloud Access](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/using_red_hat_gold_images#con-gold-image-azure).

    2.5 Espere a que las imágenes de Red Hat Gold estén disponibles en su suscripción de Azure. Estas imágenes de Gold suelen estar disponibles en unas 3 horas desde el envío.
    
3. Acepte los términos y condiciones de Azure Marketplace para las imágenes de BYOS en RHEL. Puede completar este proceso mediante la ejecución de comandos de la interfaz de la línea de comandos de Azure (CLI), como se indica a continuación. Para más información, consulte la documentación de las [imágenes Gold de BYOS en RHEL en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/byos). Es importante que ejecute la versión más reciente de la CLI de Azure.

    3.1 Inicie una sesión de la CLI de Azure y autentíquese con su cuenta de Azure. Consulte [Inicio de sesión con la CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) para obtener ayuda. Asegúrese de ejecutar la versión más reciente de la CLI de Azure antes de continuar.

    3.2 Compruebe que las imágenes de BYOS en RHEL están disponibles en su suscripción mediante la ejecución del siguiente comando de la CLI. Si no obtiene ningún resultado, consulte el paso 2 y asegúrese de que su suscripción de Azure está activada para imágenes de BYOS en RHEL.
   
   ```
   az vm image list --offer rhel-byos --all
   ```

    3.3 Ejecute el siguiente comando para aceptar los términos de Marketplace para BYOS en RHEL 7.7 y RHEL 8.0 respectivamente.
   ```
   az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm77
   ``` 

   ```
   az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm8
   ``` 
   
4. La suscripción ya estará lista para implementar BYOS en RHEL 7.7 u 8.0 en máquinas virtuales de Azure.

#### <a name="using-jboss-eap-with-byos-model"></a>Uso de JBoss EAP con el modelo BYOS

JBoss EAP solo está disponible en Azure mediante el modelo BYOS. Al implementar esta plantilla, debe proporcionar sus credenciales de RHSM junto con el identificador de grupo de RHSM con derechos de EAP válidos. Si no tiene el derecho de EAP, obtenga una [suscripción de evaluación de JBoss EAP](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/evaluation) antes de empezar.

## <a name="how-to-consume"></a>Procedimiento para el consumo

Puede implementar la plantilla de las tres maneras siguientes:

- Uso de PowerShell: implemente la plantilla mediante la ejecución de los siguientes comandos: (Consulte [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) para obtener información sobre la instalación y configuración de Azure PowerShell).

  ```
  New-AzResourceGroup -Name <resource-group-name> -Location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  ```

  ```
  New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateUri <raw link to the template which can be obtained from github>
  ```
    
- Uso de la CLI de Azure: implemente la plantilla mediante la ejecución de los siguientes comandos: (Consulte [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) para más información sobre la instalación y configuración de la CLI de Azure).

  ```
  az group create --name <resource-group-name> --location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  ```

  ```
  az group deployment create --resource-group <my-resource-group> --template-uri <raw link to the template which can be obtained from github>
  ```

- Uso de Azure Portal: puede ir a las *plantillas de inicio rápido de Azure* para realizar la implementación en Azure Portal, como se indica en la sección siguiente. En el inicio rápido, haga clic en **Implementar en Azure** o en el botón **Explorar en GitHub**.

## <a name="azure-quickstart-templates"></a>Plantillas de inicio rápido de Azure

Puede empezar con una de las plantillas de inicio rápido de la combinación de JBoss EAP en RHEL que cumpla su objetivo de implementación. A continuación se muestra la lista de plantillas de inicio rápido disponibles.

* <a href="https://azure.microsoft.com/resources/templates/jboss-eap-standalone-rhel/">JBoss EAP en RHEL (máquina virtual independiente)</a>: implementará una aplicación web denominada JBoss-EAP en Azure para JBoss EAP 7.2 o 7.3 que se ejecuta en la máquina virtual de RHEL 7.7 u 8.0.

* <a href="https://azure.microsoft.com/resources/templates/jboss-eap-clustered-multivm-rhel/">JBoss EAP en RHEL (varias máquinas virtuales en clúster)</a>: esto implementará una aplicación web denominada eap-session-replication en el clúster de JBoss EAP 7.2 o 7.3 que se ejecuta en un número "n" de máquinas virtuales de RHEL 7.7 u 8.0. El usuario decide el valor de "n". Todas las máquinas virtuales se agregan al grupo de back-end de un equilibrador de carga.

* <a href="https://azure.microsoft.com/en-us/resources/templates/jboss-eap-clustered-vmss-rhel/">JBoss EAP en RHEL (conjunto de escalado de máquinas virtuales, en clúster)</a>: esto implementará una aplicación web denominada eap-session-replication en el clúster de JBoss EAP 7.2 o 7.3 que se ejecuta en instancias del conjunto de escalado de máquinas virtuales de RHEL 7.7 u 8.0.

## <a name="resource-links"></a>Vínculos de recursos:

* [Ventajas híbridas de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing)
* [Configuración de una aplicación Java para Azure App Service](https://docs.microsoft.com/azure/app-service/configure-language-java)
* [JBoss EAP en Red Hat OpenShift en Azure](https://azure.microsoft.com/services/openshift/)
* Inicio rápido de [JBoss EAP en Azure App Service (Linux)](https://docs.microsoft.com/azure/app-service/quickstart-java)
* Tutorial [Cómo implementar JBoss EAP en Azure App Service](https://github.com/JasonFreeberg/jboss-on-app-service)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [JBoss EAP 7.2](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/)
* Más información sobre [JBoss EAP 7.3](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.3/)
* Más información sobre la [administración de suscripciones de Red Hat](https://access.redhat.com/products/red-hat-subscription-management)
* Documentación de Microsoft para [Red Hat en Azure](https://aka.ms/rhel-docs)
* Implementación de [JBoss EAP en máquinas virtuales o en un conjunto de escalado de máquinas virtuales de RHEL desde Azure Marketplace](https://aka.ms/AMP-JBoss-EAP)
* Implementación de [JBoss EAP en máquinas virtuales o en un conjunto de escalado de máquinas virtuales de RHEL desde un inicio rápido de Azure](https://aka.ms/Quickstart-JBoss-EAP)
