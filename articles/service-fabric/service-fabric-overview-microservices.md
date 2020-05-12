---
title: Introducción a los microservicios de Azure
description: Información general sobre por qué la creación de aplicaciones de nube con un enfoque de microservicios es importante para el desarrollo de aplicaciones modernas y cómo Azure Service Fabric proporciona una plataforma para lograrlo.
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: sfrev
ms.openlocfilehash: 9e0c93423b3639fb2bc3739f0d6569c4c4cec8da
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792875"
---
# <a name="why-use-a-microservices-approach-to-building-applications"></a>¿Por qué usar un enfoque de microservicios para crear aplicaciones?

Que los desarrolladores de software factorizan una aplicación en sus distintos componentes no es nada nuevo. Normalmente, se usa un enfoque con niveles con un almacén en el back-end, lógica de negocios en el nivel intermedio y una interfaz de usuario (IU) en el front-end. Lo que *ha* cambiado en los últimos años es que los desarrolladores ahora crean aplicaciones distribuidas para la nube.

Estas son algunas de las cambiantes necesidades empresariales:

* Un servicio que se crea y opera a escala para llegar a clientes en nuevas regiones geográficas.
* Entrega más rápida de características y funcionalidades para poder responder a las demandas de los clientes de forma ágil.
* Mejora de la utilización de los recursos para reducir costos.

Todas estas necesidades afectan a la *forma* en que se crean las aplicaciones.

Para más información acerca del enfoque de Azure a los microservicios, consulte [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservicios: Una revolución en las aplicaciones con la tecnología de la nube).

## <a name="monolithic-vs-microservices-design-approach"></a>Enfoque de diseño monolítico frente a microservicios

Las aplicaciones evolucionan con el tiempo. Las aplicaciones que triunfan evolucionan por ser útiles a sus usuarios. Las que fracasan no evolucionan y terminan por entrar en desuso. Esta es la pregunta: ¿cuánto sabe acerca de los requisitos actuales y cuáles serán en el futuro? Por ejemplo, supongamos que está creando una aplicación de informes para un departamento de su empresa. Está seguro de que la aplicación solo se aplica al ámbito de la compañía y que los informes no se guardarán mucho tiempo. El enfoque es diferente al adoptado, por ejemplo, para crear un servicio que entrega contenido de vídeo a decenas de millones de clientes.

A veces, terminar un servicio como una prueba de concepto es el factor determinante. Sabrá que la aplicación podrá modificarse más adelante. No tiene mucho sentido volcarse en la ingeniería de algo que no se va a usar nunca. Por otra parte, cuando las empresas crean aplicaciones para la nube, lo que espera es crecimiento y uso. El crecimiento y la escala son imprevisibles. Queremos crear prototipos rápidamente y, al mismo tiempo, saber que vamos por buen camino para tenerlo todo controlado. Este es el enfoque Lean Startup: compilar, medir, aprender e iterar.

En la época de cliente-servidor, tendíamos a centrarnos en la creación de aplicaciones en capas mediante el uso de tecnologías específicas en cada nivel. El término aplicación *monolítica* ha surgido para describir estos enfoques. Las interfaces tendían a estar entre los niveles y normalmente utilizaban un diseño más estrechamente acoplado entre los componentes de cada nivel. Los desarrolladores diseñaban y factorizaban clases compiladas en bibliotecas y las vinculaban entre sí en algunos archivos ejecutables y DLL.

Un enfoque de diseño monolítico tiene varias ventajas. A menudo las aplicaciones monolíticas son más fáciles de diseñar y las llamadas entre los componentes son más rápidas, ya que generalmente se realizan a través de comunicación entre procesos (IPC). Además, todo el mundo prueba un único producto, lo que tiende a ser un uso más eficaz de los recursos humanos. La desventaja es que se produce un acoplamiento estrecho entre las capas en niveles y no se pueden escalar los componentes individuales. Si necesita realizar actualizaciones o correcciones, tendrá que esperar hasta que otros usuarios finalicen sus pruebas. Es más difícil ser ágiles.

Los microservicios solucionan estas desventajas y se adaptan mejor a los anteriores requisitos empresariales. Pero también tienen ventajas y desventajas. Las ventajas de los microservicios son que cada uno suele encapsular funcionalidades empresariales más simples, las cuales se pueden escalar o reducir horizontalmente, probar, implementar y administrar de forma independiente. Una ventaja importante del enfoque de microservicios es que los equipos están más condicionados por los escenarios empresariales que por la tecnología. Los equipos más pequeños desarrollan un microservicio en función de un escenario del cliente y usan las tecnologías que quieren usar.

Es decir, no es necesario que la organización normalice su tecnología para mantener aplicaciones de microservicio. Los equipos individuales con servicios propios pueden hacer lo más lógico en función de sus conocimientos o de lo que sea más adecuado para resolver el problema. En la práctica, es preferible tener un conjunto de tecnologías recomendadas, como un almacén NoSQL o una plataforma de aplicaciones web concretas.

La desventaja de los microservicios surge al tener que administrar un mayor número de entidades independientes y trabajar con implementaciones y controles de versiones más complejos. Aumentan el tráfico de red entre los microservicios, al igual que las latencias de red correspondientes. Una gran cantidad de servicios granulares comunicándose entre sí pueden causar una pesadilla de rendimiento. Sin herramientas que ayuden a ver estas dependencias, es difícil "ver" todo el sistema.

Los estándares hacen que el enfoque de microservicios funcione al especificar un acuerdo sobre cómo llevar a cabo la comunicación y tolerar únicamente lo que se necesita de un servicio, en lugar de usar contratos rígidos. Es importante definir estos contratos por adelantado en el diseño, ya que los servicios se actualizan de forma independiente entre sí. Otra descripción acuñada para el diseño con el enfoque de microservicios es "arquitectura orientada a servicios (SOA) específica".

***En su forma más simple, el enfoque de diseño de microservicios consiste en una federación desacoplada de servicios, con cambios independientes en cada uno, y estándares acordados para la comunicación.***

A medida que se producen más aplicaciones en la nube, se ha descubierto que la descomposición de la aplicación global en servicios independientes centrados en escenarios es a largo plazo un mejor enfoque.

## <a name="comparison-between-application-development-approaches"></a>Comparación entre los enfoques de desarrollo de aplicaciones

![Desarrollo de aplicaciones de la plataforma Service Fabric][Image1]

1) Las aplicaciones monolíticas contienen funciones específicas del dominio y normalmente se dividen en capas funcionales, como web, negocios y datos.

2) Para escalar aplicaciones monolíticas, es preciso clonarlas en varios servidores, máquinas virtuales o contenedores.

3) Las aplicaciones de microservicios separan las funciones en servicios más pequeños independientes.

4) Este enfoque de microservicios se escala horizontalmente mediante la implementación de cada servicio de manera independiente, con la creación de instancias de estos servicios en servidores, máquinas virtuales y contenedores.

La realización del diseño con un enfoque de microservicios no es apropiado para todos los proyectos, pero se adapta mejor a los objetivos de negocio descritos. Comenzar con un enfoque monolítico puede tener sentido si se sabe que después no habrá oportunidad de reprocesar el código en un diseño con microservicios. Es más frecuente comenzar con una aplicación monolítica y, poco a poco, dividirla en fases, empezando por las áreas funcionales que deban ser más escalables o ágiles.

Cuando se usa un enfoque de microservicios, la aplicación se compone de muchos servicios pequeños. Estos servicios se ejecutan en contenedores implementados en un clúster de máquinas. Los equipos más pequeños desarrollan un servicio que se centra en un escenario y, de forma independiente, prueban, controlan versiones, implementan y escalan cada servicio para que toda la aplicación pueda evolucionar.

## <a name="what-is-a-microservice"></a>¿Qué es un microservicio?

Hay distintas definiciones de microservicios. No obstante, se suelen aceptar la mayoría de estas características de microservicios:

* Encapsulan un escenario de cliente o negocio. Qué problema está resolviendo
* Desarrollados por un pequeño equipo de ingenieros.
* Escritos en cualquier lenguaje de programación y usando cualquier plataforma.
* Constan de código y, opcionalmente, de un estado, ambos de los cuales se controlan para versiones, implementan y escalan de forma independiente.
* Interactúan con otros microservicios a través de protocolos e interfaces bien definidos.
* Tienen nombres únicos (direcciones URL) que se usan para resolver su ubicación.
* Siguen siendo coherentes y estando disponibles en caso de errores.

Para resumirlo:

***Las aplicaciones de microservicios se componen de servicios pequeños centrados en el cliente, escalables y con control de versiones independiente que se comunican entre sí mediante protocolos estándar con interfaces bien definidas.***

### <a name="written-in-any-programming-language-using-any-framework"></a>Escritos en cualquier lenguaje de programación y usando cualquier plataforma.

Como desarrolladores, deberíamos disponer de la libertad de elegir el lenguaje o plataforma que queramos según nuestras habilidades o las necesidades del servicio que estamos creando. En algunos servicios, podría valorar las ventajas de rendimiento que C++ aporta por encima de todo. En otros, la facilidad de desarrollo administrado que se obtiene de C# o Java puede ser más importante. En algunos casos, es posible que se tenga que usar una biblioteca de un asociado, una tecnología de almacenamiento de datos o método para exponer el servicio a los clientes.

Después de elegir la tecnología, se puede considerar a la administración operativa o del ciclo de vida y el escalado del servicio.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Permite que el control de versiones, la implementación y el escalado del código y del estado se realicen de forma independiente

Independientemente de la forma en que escribe los microservicios, el código y, opcionalmente, el estado se deben implementar, actualizar y escalar de forma independiente. Este problema es difícil de resolver, ya que depende de la tecnología que se elija. Para el escalado, comprender cómo particionar tanto el código como el estado presenta dificultades. Si el código y el estado usan tecnologías diferentes (algo que hoy es bastante habitual), es preciso que los scripts de implementación del microservicio puedan escalar ambos. Esta separación también trata de la agilidad y flexibilidad, por lo que puede actualizar algunos de los microservicios sin que sea preciso de actualizar todos a la vez.

Volvamos por un momento a nuestra comparación de los enfoques monolíticos y de microservicios. Este diagrama muestra las diferencias en los enfoques para almacenar el estado:

#### <a name="state-storage-for-the-two-approaches"></a>Almacenamiento de estado para los dos enfoques

![Almacenamiento de estados de la plataforma Service Fabric][Image2]

***El enfoque monolítico de la izquierda tiene una base de datos única y niveles de tecnologías específicas.***

***El enfoque de microservicios de la derecha tiene un gráfico de microservicios interconectados donde el estado normalmente tiene como ámbito el microservicio y se usan diversas tecnologías.***

En un enfoque monolítico, normalmente la aplicación usa una base de datos única. La ventaja de usar una base de datos es que está en una única ubicación, lo que facilita la implementación. Cada componente puede tener una única tabla para almacenar su estado. Los equipos tienen que separar el estado estrictamente, lo cual supone un reto. Inevitablemente, existe la tentación de agregar una columna a una tabla de clientes existente, realizar una combinación entre tablas y crear dependencias en la capa de almacenamiento. Tras esto, no es posible escalar los componentes individuales.

En el enfoque de microservicios, cada servicio administra y almacena su propio estado. Cada servicio se encarga de escalar el código y el estado juntos para satisfacer las demandas del servicio. La desventaja es que, cuando sea necesario crear vistas o consultas de los datos de la aplicación, tendrá que consultar diversos almacenes de estado. Por lo general, este problema se resuelve con un microservicio independiente que genera una vista de una colección de microservicios. Si necesita ejecutar varias consultas espontáneas en los datos, debería considerar la posibilidad de escribir los datos en cada microservicio en un servicio de almacenamiento de datos para poder realizar análisis sin conexión.

Los microservicios tienen versiones. Es posible que diferentes versiones de un microservicio se ejecuten en paralelo. Una versión más reciente de un microservicio puede producir un error durante una actualización y necesitar revertirse a una versión anterior. El control de versiones también es útil para la realización de pruebas A/B, en las que distintos usuarios experimentan versiones diferentes del servicio. Por ejemplo, es común actualizar un microservicio para que un conjunto específico de clientes prueben probar nuevas funcionalidades, antes de distribuirlas más ampliamente.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Interactúan con otros microservicios por medio de protocolos e interfaces bien definidos.

En los últimos diez años se ha publicado amplia documentación que describe los patrones de comunicación en las arquitecturas orientadas a servicios. Por lo general, la comunicación en los servicios usa un enfoque de REST con los protocolos HTTP y TCP, y XML o JSON como formato de serialización. Desde la perspectiva de la interfaz, esto supone adoptar el enfoque de diseño web. No obstante, no hay nada que le impida usar protocolos binarios o sus propios formatos de datos. Solo tenga en cuenta que la gente puede tener problemas para usar sus microservicios si estos protocolos y formatos no están disponibles públicamente.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Tiene un nombre único (dirección URL) que se usa para resolver su ubicación

El microservicio debe ser direccionable, independientemente de dónde se ejecute. Si piensa en las máquinas y en cuál de ellas se ejecuta un microservicio concreto, las cosas empeoran rápidamente.

De la misma manera que DNS resuelve una dirección URL determinada en una máquina concreta, su microservicio necesita un nombre único para que la ubicación actual sea detectable. Los microservicios necesitan nombres direccionables que sean independientes de la infraestructura en la que se ejecutan. Esto implica que existe una interacción entre la forma en que se implementa el servicio y cómo se detecta, ya que debe haber un registro del servicio. Cuando se produce un error en una máquina, el servicio de registro debe indicar a dónde se ha trasladado el servicio.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Siguen siendo coherentes y estando disponibles en el caso de errores.

Enfrentarse a errores inesperados es uno de los problemas más difíciles de resolver, sobre todo en un sistema distribuido. Gran parte del código que escribimos como desarrolladores es para controlar las excepciones. Durante las pruebas, también dedicamos más tiempo en el control de excepciones. El proceso requiere algo más que escribir código para controlar los errores. ¿Qué ocurre cuando se produce un error en la máquina en la que se ejecuta el microservicio? Es necesario detectar el error, que es un problema difícil por sí solo. Pero también necesita reiniciar el microservicio.

Por disponibilidad, es necesario que el microservicio sea resistente a los errores y pueda reiniciarse en otra máquina. Además de estos requisitos de resistencia, los datos no deben perderse y deben ser coherentes.

La resistencia es difícil de conseguir cuando se producen errores durante la actualización de la aplicación. El microservicio, si funciona con el sistema de implementación, no necesita recuperarse. Es preciso determinar si puede pasar a la versión más reciente o si es necesario revertirlo a una versión anterior para mantener la coherencia del estado. Debe considerar algunas cuestiones, como si hay máquinas suficientes como para pasar a la versión más reciente y cómo recuperar las versiones anteriores del microservicio. Para tomar estas decisiones, se requiere que el microservicio emita información de estado.

### <a name="reports-health-and-diagnostics"></a>Notifica el estado y diagnósticos

Aunque pueda parecer obvio, y a menudo se pase por alto, un microservicio debe informar sobre su mantenimiento y diagnóstico. De lo contrario, se dispondrá de poca información sobre su mantenimiento desde la perspectiva de las operaciones. Correlacionar eventos de diagnóstico en un conjunto de servicios independientes y enfrentarse a sesgos del reloj de las máquinas para entender el orden de eventos presenta dificultades. De la misma manera que se interactúa con un microservicio por medio de protocolos y formatos de datos acordados, es necesario normalizar la forma en que se registran los eventos de mantenimiento y diagnóstico que, en último término, terminarán en un almacén de eventos donde se pueden consultar y ver. Con un enfoque de microservicios, es fundamental que los diferentes equipos acuerden un único formato de registro. Debe haber un enfoque coherente para poder ver los eventos de diagnóstico en la aplicación como un todo.

El estado es diferente de los diagnósticos. El mantenimiento hace referencia a que el microservicio notifica su estado actual para que se puedan llevar a cabo las acciones adecuadas. Un buen ejemplo es colaborar con los mecanismos de actualización e implementación para mantener la disponibilidad. Aunque un servicio puede tener un estado incorrecto debido a un bloqueo de un proceso o al reinicio de la máquina, puede seguir operativo. En este caso, lo menos recomendable es iniciar una actualización. El mejor enfoque es investigar primero, o bien dejar tiempo para que el microservicio se recupere. Los eventos de mantenimiento de un microservicio permiten tomar decisiones fundamentadas y, de hecho, ayudan a crear servicios de recuperación automática.

## <a name="guidance-for-designing-microservices-on-azure"></a>Guía para el diseño de microservicios en Azure

Visite el Centro de arquitectura de Azure para una guía sobre el [diseño y creación de microservicios en Azure](https://docs.microsoft.com/azure/architecture/microservices/).

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric como plataforma de microservicios

Azure Service Fabric surgió cuando Microsoft pasó de ofrecer productos embalados, que eran habitualmente monolíticos, a ofrecer servicios. La experiencia de crear y operar servicios grandes, como Azure SQL Database y Azure Cosmos DB, dio forma a Service Fabric. La plataforma evolucionó con el tiempo a medida que la fueron adoptando más servicios. Service Fabric tenía que ejecutarse no solo en Azure, sino también en implementaciones independientes de Windows Server.

***El objetivo de Service Fabric es resolver las complicaciones que conlleva crear y ejecutar un servicio y usar de forma eficiente los recursos de infraestructura, para que los equipos puedan resolver problemas de negocios con un enfoque de microservicios.***

Service Fabric ayuda a crear aplicaciones que usan un enfoque de microservicios al ofrecer:

* Una plataforma que proporciona servicios del sistema para implementar, actualizar, detectar y reiniciar servicios con errores, detectar servicios, enrutar mensajes, administrar el estado y supervisar el mantenimiento.
* La posibilidad de implementar aplicaciones que se ejecuten en contenedores o como procesos. Service Fabric es un organizador de contenedores y proceso.
* API de programación productivas que le ayudan a crear aplicaciones como microservicios: [ASP.NET Core, Reliable Actors y Reliable Services](service-fabric-choose-framework.md). Por ejemplo, es posible obtener información de estado y de diagnóstico, o se puede aprovechar la alta disponibilidad integrada.

***Service Fabric es independiente de la forma en que cree el servicio y permite usar cualquier tecnología. Sin embargo, proporciona API de programación integradas que facilitan la creación de microservicios.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Migración de aplicaciones existentes a Service Fabric

Service Fabric permite reutilizar código existente y modernizarse con nuevos microservicios. Hay cinco fases para la modernización de aplicaciones y puede empezar y detenerse en cualquiera de ellas. Las fases son:

1) Empiece con una aplicación monolítica tradicional.  
2) Migración: use contenedores o ejecutables invitados para hospedar código existente en Service Fabric.  
3) Modernización: se agregan nuevos microservicios con código existente en contenedores.  
4) Innovación: Descomponga la aplicación monolítica en microservicios según sus necesidades.  
5) Transformación de las aplicaciones en microservicios: transforme las aplicaciones monolíticas existentes o cree nuevas aplicaciones.

![Migración a microservicios][Image3]

Recuerde que puede *iniciar y detenerse en cualquiera de estas fases*. No tiene que avanzar a la siguiente fase. 

Veamos ejemplos de cada una de las fases.

**Migrar**  
Por las dos razones siguientes, muchas compañías migran las aplicaciones monolíticas existentes a contenedores:

* Reducción de costos debidos a la consolidación y eliminación de hardware existente o a la ejecución de aplicaciones de mayor densidad.
* Un contrato de implementación coherente entre el desarrollo y las operaciones.

Las reducciones de costos son sencillas. En Microsoft, muchas de las aplicaciones existentes se están creando en contenedores, lo que supone un ahorro de millones de dólares. La implementación coherente es difícil de evaluar, pero también es muy importante. Esto significa que los desarrolladores pueden elegir las tecnologías que más les convengan, pero las operaciones solo aceptarán un único método para implementar y administrar las aplicaciones. Evita que las operaciones se vean afectadas por la complejidad de muchas tecnologías diferentes auxiliares o que obliguen a los desarrolladores a elegir solo algunas. Básicamente, cada aplicación se convierte en un contenedor de imágenes de implementación independientes.

Muchas organizaciones se detienen aquí. Ya tienen las ventajas de los contenedores y Service Fabric ofrece la experiencia de administración completa de experiencia, incluida implementación, actualizaciones, control de versiones, reversiones y supervisión de estado.

**Modernización**  
La modernización es la incorporación de nuevos servicios junto a código existente en contenedores. Si va a escribir otro código, lo mejor es optar por dar pequeños pasos en la ruta a los microservicios. Esto podría significar la adición de un nuevo punto de conexión de la API REST o una nueva lógica de negocios. De ese modo, se inicia el proceso de creación de microservicios y se pone en práctica su desarrollo e implementación.

**Innovación**  
Un enfoque de microservicios se adapta a las cambiantes necesidades empresariales. En esta fase, hay que decidir si es necesario empezar a dividir la aplicación monolítica en servicios o innovar. Un ejemplo clásico sería una base de datos que se usa como cola de flujo de trabajo y que se convierte en un cuello de botella de procesamiento. A medida que las solicitudes de flujo de trabajo aumentan, es necesario distribuir el trabajo a escala. Tome esa parte particular de la aplicación que no se escala, o que necesita actualizarse con más frecuencia, divídala como un microservicio e innove.

**Transformación de las aplicaciones en microservicios**  
En esta fase es dónde la aplicación se compone de (o divide en) microservicios. Para llegar a este punto, ha hecho el viaje de los microservicios. Puede empezar aquí, pero hacerlo sin una plataforma de microservicios que le ayude requiere una importante inversión.

### <a name="are-microservices-right-for-my-application"></a>¿Son los microservicios adecuados para mi aplicación?

Es posible. En Microsoft, a medida que más y más equipos empezaron a crear para la nube por razones empresariales, muchos de ellos constataron las ventajas de adoptar el enfoque de microservicios. Por ejemplo, Bing lleva años usando microservicios. Para otros equipos, el enfoque de microservicios era nuevo. Estos equipos descubrieron que se planteaban problemas difíciles de resolver fuera de sus principales campos de especialización. Por esta razón, Service Fabric ganó popularidad como la tecnología para la creación de servicios.

El objetivo de Service Fabric es reducir las complejidades que conlleva crear aplicaciones de microservicios, con el fin de que no haya que realizar muchos cambios costosos de diseño. Comience poco a poco, escale cuando sea necesario, deje de usar algunos servicios, agregue otros nuevos y evolucione con el uso del cliente. También sabemos que hay muchos otros problemas que aún deben resolverse para que los microservicios sean más accesibles para la mayoría de los desarrolladores. Los contenedores y el modelo de programación de actores son ejemplos de pequeños pasos en esa dirección. Estamos seguros de que surgirán más innovaciones para facilitar un enfoque de microservicios.

## <a name="next-steps"></a>Pasos siguientes

* [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservicios: Una revolución en las aplicaciones con la tecnología de la nube)
* [Centro de arquitectura de Azure: Creación de microservicios en Azure](https://docs.microsoft.com/azure/architecture/microservices/)
* [Procedimientos recomendados para aplicaciones y clústeres de Azure Service Fabric](service-fabric-best-practices-overview.md)
* [Información general sobre la terminología de Service Fabric](service-fabric-technical-overview.md)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
