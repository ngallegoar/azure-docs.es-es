---
title: Azure Lab Services y Azure DevTest Labs
description: Comparación entre Azure DevTest Labs y Azure Lab Services.
ms.topic: overview
ms.date: 06/26/2020
ms.openlocfilehash: b1cd476faf6c457033ffeace03cd2e37b51e8578
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85480089"
---
# <a name="compare-azure-devtest-labs-and-azure-lab-services"></a>Comparación entre Azure DevTest Labs y Azure Lab Services
Hay dos servicios en Azure que permiten configurar entornos de laboratorio en la nube. 

- **Azure DevTest Labs**: este servicio permite configurar rápidamente un entorno para un equipo (por ejemplo: un entorno de desarrollo o entorno de pruebas en la nube). El propietario de un laboratorio crea uno, aprovisiona Windows o máquinas virtuales Linux, instala el software y las herramientas que necesite y las pone a disposición de los usuarios del laboratorio. Los usuarios del laboratorio se conectan a las máquinas virtuales (VM) del laboratorio y las usan para trabajos cotidianos o proyectos a corto plazo. Cuando los usuarios empiecen a usar los recursos del laboratorio, el administrador podrá analizar el coste y uso entre varios laboratorios y establecer directivas exhaustivas para optimizar los costos de la organización o el equipo.
- **Azure Lab Services**: permite crear tipos de laboratorios administrados. Actualmente, laboratorios educativos es el único tipo de laboratorio administrado compatible con Azure Lab Services. El servicio gestiona la administración de toda la infraestructura de un tipo de laboratorio administrado, desde la sincronización de las máquinas virtuales hasta la gestión de errores y el escalado de dicha infraestructura. Una vez que un administrador de TI crea una cuenta de laboratorio en Azure Lab Services, un instructor puede configurar rápidamente un laboratorio para su clase, especificar el número y tipo de máquinas virtuales necesarias para los ejercicios de la clase y agregar usuarios a la clase. En cuanto un usuario se registra en la clase puede acceder a la máquina virtual para realizar los ejercicios de la clase.  

## <a name="key-capabilities"></a>Principales capacidades

Estos servicios (Azure DevTest Labs y Azure Lab Services) admiten las siguientes características o funcionalidades clave:

- **Configuración rápida y flexible de un laboratorio**. Con Azure Lab Services, los propietarios de laboratorios pueden configurar un laboratorio rápidamente según sus necesidades. El servicio ofrece la opción de ocuparse de todo el trabajo de infraestructura de Azure de los tipos de laboratorios administrados o permite que los propietarios del laboratorio administren por sí mismos y personalicen la infraestructura de la suscripción del propietario del laboratorio. El servicio proporciona escalado integrado y resiliencia de la infraestructura para laboratorios que administre el servicio.
- **Experiencia simplificada para usuarios de laboratorios**. En un laboratorio administrado, como uno educativo, los usuarios pueden registrarse en un laboratorio con un código de registro y acceder a él en cualquier momento para usar sus recursos. En un laboratorio creado en DevTest Labs, el propietario de un laboratorio puede conceder permisos a usuarios del laboratorio para crear máquinas virtuales, obtener acceso a ellas, administrar y volver a usar discos de datos y configurar secretos reutilizables.  
- **Optimización de costos y análisis**. Un propietario de laboratorio puede establecer programaciones de laboratorio para apagar e iniciar máquinas virtuales automáticamente. El propietario del laboratorio puede establecer una programación para especificar las franjas horarias de disponibilidad de las máquinas virtuales del laboratorio para los usuarios, establecer directivas de uso por usuario o por laboratorio para optimizar costos y analizar las tendencias de actividad y uso de un laboratorio. Actualmente, para los laboratorios administrados, como es el caso de los educativos, existe un subconjunto más pequeño de opciones de optimización de costos y análisis.
- **Seguridad incrustada**. Un propietario de laboratorio puede configurar redes virtuales privadas y una subred para un laboratorio, además de habilitar una dirección IP pública compartida. Los usuarios de laboratorio pueden obtener acceso de forma segura a los recursos mediante su red virtual configurada con ExpressRoute o VPN de sitio a sitio. (solo disponible actualmente en DevTest Labs)
- **Integración en flujos de trabajo y herramientas**. Azure Lab Services le permite integrar laboratorios en los sistemas de administración y el sitio web de la organización. Puede aprovisionar automáticamente entornos a partir de las herramientas de integración continua (CI) o implementación continua (CD). (solo disponible actualmente en DevTest Labs)

## <a name="scenarios"></a>Escenarios

Estos son algunos de los escenarios que Azure DevTest Labs y Azure Lab Services admiten:

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Configuración de un laboratorio de equipos redimensionables en la nube para clases  

- Creación de un laboratorio educativo administrado. Solo indica al servicio exactamente qué necesita y creará y administrará la infraestructura del laboratorio para que pueda centrarse en la clase, no en los detalles técnicos del laboratorio.
- Proporcione a los alumnos un laboratorio de máquinas virtuales que se configuran exactamente con todo lo necesario para una clase. Asigne a cada alumno un número limitado de horas para el uso de máquinas virtuales para trabajos de clase.  
- Desplace un laboratorio de equipos físicos del centro educativo a la nube. Escale automáticamente el número de máquinas virtuales solo al umbral de costo y uso máximo que establezca en el laboratorio.
- Elimine el laboratorio con un solo clic cuando haya terminado.

### <a name="use-devtest-labs-for-development-environments"></a>Uso de DevTest Labs para entornos de desarrollo

Se puede utilizar Azure DevTest Labs para implementar numerosos escenarios clave pero uno de los principales escenarios implica el uso de DevTest Labs para máquinas de desarrollo host para desarrolladores. En este escenario, DevTest Labs ofrece estas ventajas:

- Permite que los desarrolladores puedan aprovisionar rápidamente las máquinas de desarrollo a petición.
- Aprovisione entornos de Windows y Linux con artefactos y plantillas reutilizables.
- Los desarrolladores pueden personalizar fácilmente las máquinas de desarrollo, siempre que sea necesario.
- Los administradores pueden controlar los costos al garantizar que los desarrolladores no pueden recibir más máquinas virtuales de las que necesitan para el desarrollo y las máquinas virtuales se apagan cuando no se usan.

Para obtener más información, consulte [Use DevTest Labs for development](devtest-lab-developer-lab.md) (Uso de DevTest Labs para desarrollo).

### <a name="use-devtest-labs-for-test-environments"></a>Uso de DevTest Labs para entornos de prueba

Puede usar Azure DevTest Labs para implementar numerosos escenarios clave, aunque un escenario principal implica el uso de DevTest Labs para hospedar máquinas para evaluadores. En este escenario, DevTest Labs ofrece estas ventajas:

- Los evaluadores pueden probar la versión más reciente de la aplicación aprovisionando rápidamente entornos de Windows y Linux mediante plantillas y artefactos reutilizables.
- Los evaluadores pueden escalar verticalmente la prueba de carga aprovisionando varios agentes de prueba.
- Los administradores pueden controlar los costos al garantizar que los evaluadores no pueden recibir más máquinas virtuales de las que necesitan para las pruebas y las máquinas virtuales se apagan cuando no se usan.

Para obtener más información, consulte [Use DevTest Labs for testing](devtest-lab-test-env.md) (Uso de DevTest Labs para pruebas).

## <a name="types-of-labs"></a>Tipos de laboratorios
Puede crear dos tipos de laboratorios: **tipos de laboratorios administrados** con Azure Lab Services y **laboratorios** con Azure Lab Services. Si solo quiere insertar lo que necesita en un laboratorio y permitir que el servicio configure y administre la infraestructura necesaria para dicho laboratorio, elija **tipos de laboratorios administrados**. Actualmente, el **laboratorio educativo** es el único tipo de laboratorio administrado que se puede crear con Azure Lab Services. Si quiere administrar su propia infraestructura, cree un laboratorio con **Azure DevTest Labs**.

En las siguientes secciones, se proporcionan más detalles de estos laboratorios. 

## <a name="managed-lab-types"></a>Tipos de laboratorio administrado
Azure Lab Services permite crear laboratorios cuya infraestructura la administra Azure. En este artículo se hace referencia a ellos como tipos de laboratorios administrados. Los tipos de laboratorios administrados ofrecen diferentes tipos de laboratorio que se ajustan a sus necesidades. Actualmente, el único tipo de laboratorio administrado que se admite es el **laboratorio educativo**. 

Los tipos de laboratorios administrados le permiten ponerse en marcha inmediatamente, con una configuración mínima. El servicio controla toda la administración de la infraestructura para el laboratorio, desde la sincronización de máquinas virtuales al control de errores y el escalado de infraestructuras. Para crear un tipo de laboratorio administrado, como puede ser un laboratorio educativo, debe crear antes una cuenta de laboratorio para la organización. La cuenta de laboratorio actúa como cuenta central en la que se administran todos los laboratorios de la organización. 

Al crear y usar recursos de Azure en estos tipos de laboratorios administrados, el servicio crea y administra los recursos en las suscripciones internas de Microsoft. No se crean en su propia suscripción de Azure. El servicio realiza un seguimiento del uso de estos recursos en las suscripciones internas de Microsoft. Este uso se factura a la suscripción de Azure que contiene la cuenta de laboratorio.   

Estos son algunos de los **casos de uso de los tipos de laboratorios administrados**: 

- Proporcione a los alumnos un laboratorio de máquinas virtuales que se configuran exactamente con todo lo necesario para una clase. Asigne a cada alumno un número limitado de horas para el uso de máquinas virtuales para deberes o proyectos personales.
- Configure un grupo de máquinas virtuales de proceso de alto rendimiento para realizar investigaciones de procesos o gráficos intensivos. Ejecute las máquinas virtuales según sus necesidades y limpie las máquinas cuando haya terminado. 
- Desplace un laboratorio de equipos físicos del centro educativo a la nube. Escale automáticamente el número de máquinas virtuales solo al umbral de costo y uso máximo que establezca en el laboratorio.  
- Aprovisione rápidamente un laboratorio de máquinas virtuales para un host de Hackathon. Elimine el laboratorio con un solo clic cuando haya terminado. 


## <a name="devtest-labs"></a>DevTest Labs
Puede tener escenarios en los que quiera administrar toda la infraestructura y la configuración dentro de la suscripción. Para ello, puede crear un laboratorio con Azure DevTest Labs en Azure Portal. Para estos laboratorios, no es necesario crear ninguna cuenta de laboratorio. Estos laboratorios no aparecen en la cuenta de laboratorio (que existe para los tipos de laboratorios administrados).  

A continuación, se indican algunos **casos de uso para DevTest Labs**: 

- Aprovisione rápidamente un laboratorio de máquinas virtuales para un host de Hackathon o una sesión práctica en una conferencia. Elimine el laboratorio con un solo clic cuando haya terminado. 
- Cree un grupo de máquinas virtuales configuradas con la aplicación y permita que el equipo use fácilmente una máquina virtual para búsquedas de errores.  
- Proporcione a los desarrolladores con máquinas virtuales configuradas con todas las herramientas que necesitan. Programe el inicio y el apagado automático para minimizar costos. 
- Cree repetidamente un laboratorio de máquinas de prueba como parte de la implementación. Pruebe los últimos bits y limpie las máquinas de prueba cuando haya terminado. 
- Configure una variedad de máquinas virtuales y agentes de pruebas configuradas de manera diferente para escalar y probar el rendimiento. 
- Ofrezca sesiones de aprendizaje para los clientes con un laboratorio configurado con la versión más reciente del producto. Ofrezca a cada cliente un número limitado de horas de uso en el laboratorio. 


## <a name="managed-lab-types-vs-devtest-labs"></a>Tipos de laboratorio administrado frente a DevTest Labs
En la tabla siguiente se comparan dos tipos de laboratorios compatibles con Azure Lab Services: 

| Características | Tipos de laboratorio administrado | DevTest Labs |
| -------- | ----------------- | ---------- |
| Administración de infraestructura de Azure en el laboratorio. |  Administrado automáticamente por el servicio | Administrado por cuenta propia  |
| Resistencia integrada a problemas de infraestructura | Controlado automáticamente por el servicio | Administrado por cuenta propia  |
| Administración de suscripciones | El servicio controla la asignación de recursos en las suscripciones de Microsoft compatibles con el servicio. El servicio controla automáticamente el escalado. | Administre por cuenta propia en la suscripción de Azure. Sin escalabilidad automática de suscripciones. |
| Implementación de Azure Resource Manager en el laboratorio | No disponible | Disponible |

## <a name="next-steps"></a>Pasos siguientes

Vea los artículos siguientes: 

- [Acerca de los laboratorios educativos](../lab-services/classroom-labs-overview.md)
- [Acerca de DevTest Labs](devtest-lab-overview.md)
